

# Amazon DSSTNE: Deep Scalable Sparse Tensor Network Engine

DSSTNE (pronounced "Destiny") is an open source software library for training and deploying recommendation
models with sparse inputs, fully connected hidden layers, and sparse outputs. It was open sourced by Amazon in 2016.

Models with weight matrices
that are too large for a single GPU can still be trained on a single host. DSSTNE has been used at Amazon
to generate personalized product recommendations for our customers at Amazon's scale. It is designed for
production deployment of real-world applications which need to emphasize speed and scale over experimental 
flexibility.

DSSTNE was built with a number of features for production recommendation workloads:

* **Multi-GPU Scale**: Training and prediction
both scale out to use multiple GPUs, spreading out computation
and storage in a model-parallel fashion for each layer.
* **Large Layers**: Model-parallel scaling enables larger networks than
are possible with a single GPU.
* **Sparse Data**: DSSTNE is optimized for fast performance on sparse datasets, common in recommendation 
problems. Custom GPU kernels perform sparse computation on the GPU, without filling in lots of zeroes.


As mentioned above, DSSTNE was built to support large, sparse layers. One of the ways it does so is by supporting model parallel training. In model parallel training, the model is distributed across N GPUs – the dataset (e.g., RDD) is replicated to all GPU nodes. Contrast this with data parallel training where each GPU only trains on a subset of the data, then shares the weights with each other using synchronization techniques such as a parameter server.

## Setup
* Follow [DSSTNE Walkthrough](DSSTNE_Walkthrough.md) for step by step instructions on setting up DSSTNE

## Scaling up
This [blog](http://blogs.aws.amazon.com/bigdata/post/TxGEL8IJ0CAXTK/Generating-Recommendations-at-Amazon-Scale-with-Apache-Spark-and-Amazon-DSSTNE) by Amazon has a detailed implementation of how to scale DSSTNE. In this architecture, data analytics and processing (i.e., CPU jobs) are executed through vanilla Spark on Amazon EMR, where the job is broken up into tasks and runs on a Spark executor. The GPU job above refers to the training or prediction of neural networks. The partitioning of the dataset for these jobs is done in Spark, but the execution of these jobs is delegated to ECS and is run inside Docker containers on the GPU slaves. Data transfer between the two clusters is done through Amazon S3.

When a GPU job is run, it is broken down into one or more GPU tasks. Like Spark, a GPU task is assigned for each partition of the data RDD. The Spark executors save their respective partitions to S3, then call ECS to run a task definition with container overrides that specify the S3 location of its input partitions and the command to execute on the specified Docker image. 

## Limitations

DSSTNE has a few limitations as well. This engine might be really good at providing faster recommendations on large datasets but implementing and optimizing this for our use case is time consuming. Amazon has provided a framework for this, but hasn’t really provided a lot of resources on how to implement and optimize the model. Also, Amazon has provisioned DSSTNE to run only on GPU’s. There is no way to run this on a CPU even for development and testing. Finally, the set-up instructions for this model is currently available only in Ubuntu and doesn’t support Windows or Mac. Few of these limitations might be because its relatively new and support may get added at a later stage.

## References
1. https://github.com/amzn/amazon-dsstne
