# capstone-golf-courses

# Golf Course Recommender Capstone project for General Assembly Data Science Immersive

This repository is a for a golf course recommendation engine that I built while in the Data Science Immersive program at General Assembly.

The goal of this project was to recommend similar golf courses to users given a known course that a user enjoys. Results were narrowed down to a 50 mile radius given a target zip code where you want to play. The overarching goal for this project was to help golfers to find new challenges that they might not otherwise ever discover.

In short, to do this I used a golf course dataset that included most all golf courses in the U.S. to use characteristics of the golf courses to group similar clusters together. I used eight different features to build a k-means clustering model to group golf courses into five different groups. I then used the Euclidean distances of the data points to find the five most similar golf courses.

I then built a golf course recommender function, which works like this: input a golf course you enjoy, the state it's in, and the zip code where you want to find similar courses. The function will then output the five most similar golf courses within a 50 mile radius of the input zip code. Pretty cool!

Please read the Technical Report for a full breakdown on how I went about this. 

Even better, check out the notebook to see how this was created and to try it out for yourself!
