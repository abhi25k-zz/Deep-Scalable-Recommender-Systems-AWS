

# Amazon SageMaker

Amazon SageMaker is a fully managed service that allows developers and data scientists to build, train, and deploy machine learning models.

Amazon SageMaker includes three modules: Build, Train, and Deploy. The Build module provides a hosted environment to work with your data, experiment with algorithms, and visualize your output. The Train module allows for one-click model training and tuning at high-scale and low cost. The Deploy module provides a managed environment for you to easily host and test models for inference securely and with low latency. 

## Factorization Machines

Factorization Machines (FM) are a supervised machine learning technique introduced in 2010. FM get their name from their ability to reduce problem dimensionality thanks to matrix factorization.

Factorization machines can be used for classification or regression and are much more computationally efficient on large sparse data sets than traditional algorithms like linear regression. This property is why FM are widely used for recommendation. User count and item count are typically very large although the actual number of recommendations is very small (users don’t rate all available items!).

Here’s a simple example: Where a sparse rating matrix (dimension 4×4) is factored into a dense user matrix (dimension 4×2) and a dense item matrix (2×4). As you can see, the number of factors (2) is smaller than the number of columns of the rating matrix (4). In addition, this multiplication also lets us fill all blank values in the rating matrix, which we can then use to recommend new items to any user.

![Factorization](Factorization.png)
Source: data-artisans.com

## Implementaton on AWS

For this example, we will use Amazon SageMaker to generate movie recommendations based on the [MovieLens](https://grouplens.org/datasets/movielens/) dataset. A detailed walkthrough of our process can be found [here](Recommender_System_on_Amazon_SageMaker.ipynb)

## Limitations

SageMaker scales well for model building and deployment but preprocessing steps do not scale as well and need to be implemented in something like Spark for it to be truly distributed.

## References
1. https://aws.amazon.com/sagemaker/features/
2. https://aws.amazon.com/blogs/machine-learning/build-a-movie-recommender-with-factorization-machines-on-amazon-sagemaker/
3. https://www.csie.ntu.edu.tw/~b97053/paper/Rendle2010FM.pdf
4. https://docs.aws.amazon.com/sagemaker/latest/dg/fact-machines.html
5. https://medium.com/@julsimon/building-a-movie-recommender-with-factorization-machines-on-amazon-sagemaker-cedbfc8c93d8
6. https://hackernoon.com/should-i-use-amazon-sagemaker-for-deep-learning-dc4ae6b98fab

