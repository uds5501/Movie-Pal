### A simple movie recommendation engine using *Surprise* module and *Weighted KNN filter*

# Basics of Model Based Collaborative Filtering

this excerpt and technique has been taken from : https://blog.dominodatalab.com/recommender-systems-collaborative-filtering/

Now that we have concrete method for defining the similarity between vectors, we can now discuss how to use this method to identify similar users. The problem set-up is as follows:

1. We have an $N X M$ matrix consisting of the ratings of $N$ users and $M$ items. Each element of the matrix $(i, j)$ represents how user $i$ rated item $j$. Since we are working with movie ratings, each rating can be expected to be an integer from 1-5 (reflecting one-star ratings to five-star ratings) if user i has rated movie j, and 0 if the user has not rated that particular movie.

2. For each user, we want to recommend a set of movies that they have not seen yet (the movie rating is 0). To do this, we will effectively use an approach that is similar to **weighted K-Nearest Neighbors.**

3. For each movie j user i has not seen yet, we find the set of users U who are similar to user i and have seen movie j.For each similar user u, we take u's rating of movie j and multiply it by the cosine similarity of user i and user u. Sum up these weighted ratings, divide by the number of users in U, and we get a weighted average rating for the movie j.

4. Finally, we sort the movies by their weighted average rankings. These average rankings serve as an estimate for what the user will rate each movie. Movies with higher average rankings are more likely to be favored by the user, so we will recommend the movies with the highest average rankings to the user.

**I said earlier that this procedure is similar to a weighted K-Nearest Neighbors algorithm. We take the set of users who have seen movie j as the training set for K-NN and each user who has not seen the movie as a test point. For each user who has not seen the movie (test point), we compute the similarity to users who have seen the movie, and assign an estimated rating based on the known ratings of the neighbors.**

For a concrete example, let's say I have not seen the movie *Hidden Figures*. I have seen and rated (on a 5-star scale) a ton of other movies though. With this information, you want to predict what I will rate Hidden Figures. **Based on my rating history, you can find a group of users who rate movies similarly to me and have also seen Hidden Figures.** 

To keep this example simple, let's say we look at the two users who are most similar to me and have seen the movie. Let's say User 1 has 95% similarity to me and gave the movie a four-star rating, and User 2 has 80% similarity to me and gave the movie a five-star rating. Now my predicted rating is the average of 0.954 = 3.8 (Similarity X Rating of User 1) and 0.805 = 4 (Similarity X Rating of User 2), so I am predicted to give the movie a rating of 3.9.
