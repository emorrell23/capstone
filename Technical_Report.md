# GOLF COURSE RECOMMENDATION ENGINE
## TECHNICAL REPORT

The goal of this project was to recommend similar golf courses to users given a known course that a user enjoys. Results were narrowed down to a 50 mile radius given a target zip code where you want to play. The overarching goal for this project was to help golfers to find new challenges that they might not otherwise ever discover.

### Data Acquisition
The data I used for this project was a dataset I found at usabledatabases.com of over 15,000 U.S. golf courses with 34 features in the dataset including course address, par, holes, yards, course rating and slope on three different sets of tees. There were over 6,000 null values in the championship course rating feature, and there was not a suitable way to impute the null values, so I had to drop them. The majority of the dropped courses were not full length courses, so the 9,007 remaining courses was still a suitable dataset to work with for this project.

### Data Cleaning/Transformation
The first step was to join the city and state dataframes with the main dataframe in order to be able to filter by state in the main golf course recommender function. As far as data cleaning, there were some typos that had to be fixed and one course that had the incorrect state listed.

During the feature transformation process, a number of features needed to be changed from string to numeric. The columns that needed to be transformed were the driving range column (yes or no), the weekend price column (grouped from 1 to 4), and the public or private column. Individual functions were created for each one since there were some outlier values that needed to be dealt with, including misspellings. For example, in the public or private column there were six different options including semi-private, resort and military. I grouped private on its own and everything else as public since even semi-private and resort courses are all able to be played on without having a membership.

### Modeling
I used a k-means clustering model to group golf courses into clusters of similar golf courses. I used k-means because of the ease of choosing the number of clusters (k), but perhaps more importantly because of its use of Euclidean distance and how I could then use the euclidean distance between data points to produce the “most” similar golf courses to a given golf course.

As far as feature selection goes, while I was playing around with different combinations of features and seeing what the silhouette score was, I tried to keep in mind to only include features that golfers would care about, not simply trying to get the best score. If including a feature can improve the silhouette score but isn’t important to golfers, then it isn’t very useful. I want to recommend golf courses based on features that are important to golfers first and foremost, and if can improve the model as well, all the better. As such, the features I included in the model are: whether or not it has a driving range, # of holes, par, length, course rating, whether it is public or private and price.

In order to find the optimal number of clusters, I used the “Elbow Method” to plot the distortion, or distance of data points to the centroid of its cluster, against a range of possible number of clusters. The Elbow plot shows that k = 5 seem to have the largest “elbow”, or at which number of k the distortion most rapidly increase.

The k-means model with 5 clusters came back with a silhouette score of 0.63, which indicates that the clusters are pretty cohesive as separated from each other. To evaluate my clusters I looked at the descriptive statistics of each cluster as well as doing a pairpot to view each feature plotted against every other feature. I did try a few different possibilities for k (4, 5 ,6) and plotted them all out via pairplot to see what types of courses were grouped together, and, to me, 5 clusters made the most sense when looking at the groupings, so that’s what I settled on. Having 6 clusters had a better silhouette score, but I would rather have fewer clusters since I’m mainly using the Euclidean distances anyway. I had to remind myself that it wasn’t necessarily the clusters that mattered the most, but how close together the data points were, and that is more of a function of the model features. 


### Building Function to Recommend Golf Courses
Once I had the golf courses grouped into clusters, I began to build a function where the user could input a golf course and it would output the 5 most similar golf courses in a region. I determined that the function would take 3 arguments: the name of a golf course that the user knows they enjoy, the state in which that golf course resides, and the zip code where the user wants to look for similar courses. 

By inputting the state that the golf course is in, I could narrow down the dataset to look up the input course. This aims to deal with the issue that there are many golf courses across the country with the same or similar names. Also, with this, you don’t have to worry about whether its a “Golf Club” or “Golf Course” or anything else; the keyword should be enough.

In addition, the function uses the zip code where the user wants to find similar golf courses to narrow down the dataset to only include golf courses that are within 50 miles of the input zip code. This is an optional argument; if no zip code is given it will output the top 5 most similar golf courses nationwide.

The output of the function returns five golf courses. For each golf course, the function outputs the golf course name, address, phone number, number of holes, par, length, course rating, slope, whether it is public or private and the price on the weekend (on a scale of 1 to 4).

How the function works is that it takes in a state, narrows down the dataset to find the name of the golf course given and saves the index of that course. Then, if no zip code is given, it searches through the Euclidean distance array to find the top 5 most similar courses in the entire dataset. If a zip code is given, it uses the ZipCodeDatabase function (from the pyzipcode library) to find a list of zip codes within a 50 mile radius of the given zip code. That list of zip codes is used to filter the dataset to only include golf courses within a 50 mile radius. From that filtered dataset, the indexes of the 5 courses with the smallest Euclidean distance are saved. With those indexes, the function can look through the original dataset and print off the characteristics of each course.

Initial testing of the function came with a few different errors popping up, and as such, it has gone through several iterations in an attempt to become more robust. Some of the errors that occurred were if there were not any courses in the same cluster within 50 miles it would error; or if there were multiple golf courses with the same name (or multiple courses in the same facility) in the same state that would error. A couple separate if/else statements were put in place to combat those errors. The model will continue to be updated as new problems arise.


### Future steps
Since I had to drop 6,000 golf courses due to null values, one option would be to find a more complete dataset or to find a good way to impute the nulls. 

I would also like to continue to tweak the model to find the best way to create the clusters. I would love to figure out a way to incorporate the golf course architects. In addition, one idea that could improve the model is to allow the user to choose from a list of preferences what is most important to them in a golf course, and adjust the clustering model as such. 

Another option could be to get user ratings of golf courses and build a collaborative filtering model to use in combination with this model to have a hybrid golf course recommender.






