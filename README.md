
## Introduction

The purpose of this project is to implement various algorithms to predict the "rating" a user would give to a joke they haven't already rated, using the joke data set from Kaggle. The main methods that will be featured will be <b>Collaborating Filtering</b>, <b>Singular Value Decomposition</b> and some <b>Cluster Based</b> algorithms using Kmeans.

## Step 1 - Pre-Processing

Initially, our data will look something like this:

<img src='https://github.com/billgewrgoulas/Recommendation-Systems/blob/main/gif/p1.png'>

In order to apply the algorithms, we will have to create the rating matrix. Each row will represent the rating vector for each user and every one of these vectors will have as many axes as the number of jokes. Similary each column will represent the rating vector of each joke. Obviously, this will result in a matrix with many blank values since not all users have rated every joke. In this case, we will fill the NaN values with 0, so we can use sparse matrices for efficiency. In addition, we will use the ids of the jokes and users as indices for convenience.

<img src='https://github.com/billgewrgoulas/Recommendation-Systems/blob/main/gif/p6.png'>

## Step 2 - Defining Similarity

Now that we have assigned a 1xN vector to each user we need to find a way to determine how similar two users are. 
To achieve this we will use <b>Cosine similarity</b> which measures the angle between two vectors.

<img src='https://github.com/billgewrgoulas/Recommendation-Systems/blob/main/gif/p7.jpeg'>

## Step 3 - Evaluation

To test the accuracy of each algorithm we will use the <b>Root Mean Squared Error</b>. In addition, we will implement two simple algorithms that work with the means and will act as baselines.

## Step 4 - Algorithms

### User Collaborative Filtering - Main Flow

 * Get vector of u, and find indices of users that rated j
 * Compute similarities between users that rated j and user u
 * Keep k most similar users and their scores
 * Predict the score using the above formula

### Content Collaborative Filtering - Main Flow (same as UCF but it works with the joke vectors)

 * Get vector of J, and find indices of jokes that were rated by user u
 * Compute similarities between jokes that were rated by u and joke j
 * Keep k most similar jokes and their scores
 * Predict the rating using the above formula


### Improved UCF

 * In this case we will predict the deviations from the mean, and use the modified formula:
 
### Singular Value Decomposition

 * Apply SVD and keep the k largest singular values in order to produce the prediction matrix.

## Cluster-Based Methods

In this case, we will combine Kmeans with some of the previous algorithms. We will apply Kmeans on the rating matrix using the user rating vectors for the clustering, but first, we have to determine the right number of clusters. To achieve this we will create the combined Silhouette Coefficient - SSE plot to find the best number of clusters.

### Cluster-Based JA (CB-JA) - Main Flow

 * Find cluster of u and users in the cluster
 * Get ratings of j in the cluster
 * Return mean in the cluster or mean of j

### Cluster-Based UCF (CB-UCF) - Main Flow

 The idea here is to run UCF on the vectors in the cluster of the user u.

 * Find cluster of u and users in the cluster
 * Get the sparse submatrix that contains only the rating vectors of the users in the cluster.
 * Run UCF on the submatrix

### Cluster-Based UA (CB-UA) - Main Flow

 For this method we will use Kmeans to cluster the jokes using their text. To achieve this we will assign a vector to each joke using <b>TF-IDF Vectorizer</b>

 * Find cluster of j and all jokes in the cluster
 * Return the mean of the user in the cluster.


