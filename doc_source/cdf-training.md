# Common Data Formats for Training<a name="cdf-training"></a>

To prepare for training, you can preprocess your data using a variety of AWS services, including AWS Glue, Amazon EMR, Amazon Redshift, Amazon Relational Database Service, and Amazon Athena\. After preprocessing, publish the data to an Amazon S3 bucket\. For training, the data need to go through a series of conversions and transformations, including: 
+ Training data serialization \(handled by you\) 
+ Training data deserialization \(handled by the algorithm\) 
+ Training model serialization \(handled by the algorithm\) 
+ Trained model deserialization \(optional, handled by you\) 

When using Amazon SageMaker in the training portion of the algorithm, make sure to upload all data at once\. If more data is added to that location, a new training call would need to be made to construct a brand new model\.

**Topics**
+ [Content Types Supported by Built\-In Algorithms](#cdf-common-content-types)
+ [Using Pipe Mode](#cdf-pipe-mode)
+ [Using CSV Format](#cdf-csv-format)
+ [Using RecordIO Format](#cdf-recordio-format)
+ [Trained Model Deserialization](#td-deserialization)

## Content Types Supported by Built\-In Algorithms<a name="cdf-common-content-types"></a>

The following table lists some of the commonly supported [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Channel.html#SageMaker-Type-Channel-ContentType](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Channel.html#SageMaker-Type-Channel-ContentType) values and the algorithms that use them:


**ContentTypes for Built\-in Algorithms**  

| ContentType | Algorithm | 
| --- | --- | 
| application/x\-image | Object Detection Algorithm, Semantic Segmentation | 
| application/x\-recordio |  Object Detection Algorithm  | 
| application/x\-recordio\-protobuf |  Factorization Machines, K\-Means, k\-NN, Latent Dirichlet Allocation, Linear Learner, NTM, PCA, RCF, Sequence\-to\-Sequence  | 
| application/jsonlines |  BlazingText, DeepAR  | 
| image/jpeg |  Object Detection Algorithm, Semantic Segmentation  | 
| image/png |  Object Detection Algorithm, Semantic Segmentation  | 
| text/csv |  IP Insights, K\-Means, k\-NN, Latent Dirichlet Allocation, Linear Learner, NTM, PCA, RCF, XGBoost  | 
| text/libsvm |  XGBoost  | 

For a summary of the parameters used by each algorithm, see the documentation for the individual algorithms or this [table](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-algo-docker-registry-paths.html)\.

## Using Pipe Mode<a name="cdf-pipe-mode"></a>

In *Pipe mode*, your training job streams data directly from Amazon Simple Storage Service \(Amazon S3\)\. Streaming can provide faster start times for training jobs and better throughput\. This is in contrast to *File mode*, in which your data from Amazon S3 is stored on the training instance volumes\. File mode uses disk space to store both your final model artifacts and your full training dataset\. By streaming in your data directly from Amazon S3 in Pipe mode, you reduce the size of Amazon Elastic Block Store volumes of your training instances\. Pipe mode needs only enough disk space to store your final model artifacts\. See the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AlgorithmSpecification.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AlgorithmSpecification.html) for additional details on the training input mode\.

## Using CSV Format<a name="cdf-csv-format"></a>

Many Amazon SageMaker algorithms support training with data in CSV format\. To use data in CSV format for training, in the input data channel specification, specify **text/csv** as the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Channel.html#SageMaker-Type-Channel-ContentType](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Channel.html#SageMaker-Type-Channel-ContentType)\. Amazon SageMaker requires that a CSV file does not have a header record and that the target variable is in the first column\. To run unsupervised learning algorithms that don't have a target, specify the number of label columns in the content type\. For example, in this case **'content\_type=text/csv;label\_size=0'**\. For a notebook example that uses CSV format, see [Breast Cancer Prediction](https://sagemaker-examples.readthedocs.io/en/latest/introduction_to_applying_machine_learning/breast_cancer_prediction/Breast%20Cancer%20Prediction.html)\. For more information, see [Now use Pipe mode with CSV datasets for faster training on Amazon SageMaker built\-in algorithms](http://aws.amazon.com/blogs/machine-learning/now-use-pipe-mode-with-csv-datasets-for-faster-training-on-amazon-sagemaker-built-in-algorithms/)\.

## Using RecordIO Format<a name="cdf-recordio-format"></a>

In the protobuf recordIO format, SageMaker converts each observation in the dataset into a binary representation as a set of 4\-byte floats, then loads it in the protobuf values field\. If you are using Python for your data preparation, we strongly recommend that you use these existing transformations\. However, if you are using another language, the protobuf definition file below provides the schema that you use to convert your data into SageMaker protobuf format\.

**Note**  
For an example that shows how to convert the commonly used numPy array into the protobuf recordIO format, see *[An Introduction to Factorization Machines with MNIST](https://sagemaker-examples.readthedocs.io/en/latest/introduction_to_amazon_algorithms/factorization_machines_mnist/factorization_machines_mnist.html)* \.

```
syntax = "proto2";

 package aialgs.data;

 option java_package = "com.amazonaws.aialgorithms.proto";
 option java_outer_classname = "RecordProtos";

 // A sparse or dense rank-R tensor that stores data as doubles (float64).
 message Float32Tensor   {
     // Each value in the vector. If keys is empty, this is treated as a
     // dense vector.
     repeated float values = 1 [packed = true];

     // If key is not empty, the vector is treated as sparse, with
     // each key specifying the location of the value in the sparse vector.
     repeated uint64 keys = 2 [packed = true];

     // An optional shape that allows the vector to represent a matrix.
     // For example, if shape = [ 10, 20 ], floor(keys[i] / 20) gives the row,
     // and keys[i] % 20 gives the column.
     // This also supports n-dimensonal tensors.
     // Note: If the tensor is sparse, you must specify this value.
     repeated uint64 shape = 3 [packed = true];
 }

 // A sparse or dense rank-R tensor that stores data as doubles (float64).
 message Float64Tensor {
     // Each value in the vector. If keys is empty, this is treated as a
     // dense vector.
     repeated double values = 1 [packed = true];

     // If this is not empty, the vector is treated as sparse, with
     // each key specifying the location of the value in the sparse vector.
     repeated uint64 keys = 2 [packed = true];

     // An optional shape that allows the vector to represent a matrix.
     // For example, if shape = [ 10, 20 ], floor(keys[i] / 10) gives the row,
     // and keys[i] % 20 gives the column.
     // This also supports n-dimensonal tensors.
     // Note: If the tensor is sparse, you must specify this value.
     repeated uint64 shape = 3 [packed = true];
 }

 // A sparse or dense rank-R tensor that stores data as 32-bit ints (int32).
 message Int32Tensor {
     // Each value in the vector. If keys is empty, this is treated as a
     // dense vector.
     repeated int32 values = 1 [packed = true];

     // If this is not empty, the vector is treated as sparse with
     // each key specifying the location of the value in the sparse vector.
     repeated uint64 keys = 2 [packed = true];

     // An optional shape that allows the vector to represent a matrix.
     // For Exmple, if shape = [ 10, 20 ], floor(keys[i] / 10) gives the row,
     // and keys[i] % 20 gives the column.
     // This also supports n-dimensonal tensors.
     // Note: If the tensor is sparse, you must specify this value.
     repeated uint64 shape = 3 [packed = true];
 }

 // Support for storing binary data for parsing in other ways (such as JPEG/etc).
 // This is an example of another type of value and may not immediately be supported.
 message Bytes {
     repeated bytes value = 1;

     // If the content type of the data is known, stores it.
     // This allows for the possibility of using decoders for common formats
     // in the future.
     optional string content_type = 2;
 }

 message Value {
     oneof value {
         // The numbering assumes the possible use of:
         // - float16, float128
         // - int8, int16, int32
         Float32Tensor float32_tensor = 2;
         Float64Tensor float64_tensor = 3;
         Int32Tensor int32_tensor = 7;
         Bytes bytes = 9;
     }
 }

 message Record {
     // Map from the name of the feature to the value.
     //
     // For vectors and libsvm-like datasets,
     // a single feature with the name `values`
     // should be specified.
     map<string, Value> features = 1;

     // An optional set of labels for this record.
     // Similar to the features field above, the key used for
     // generic scalar / vector labels should be 'values'.
     map<string, Value> label = 2;

     // A unique identifier for this record in the dataset.
     //
     // Whilst not necessary, this allows better
     // debugging where there are data issues.
     //
     // This is not used by the algorithm directly.
     optional string uid = 3;

     // Textual metadata describing the record.
     //
     // This may include JSON-serialized information
     // about the source of the record.
     //
     // This is not used by the algorithm directly.
     optional string metadata = 4;

     // An optional serialized JSON object that allows per-record
     // hyper-parameters/configuration/other information to be set.
     //
     // The meaning/interpretation of this field is defined by
     // the algorithm author and may not be supported.
     //
     // This is used to pass additional inference configuration
     // when batch inference is used (e.g. types of scores to return).
     optional string configuration = 5;
 }
```

After creating the protocol buffer, store it in an Amazon S3 location that Amazon SageMaker can access and that can be passed as part of `InputDataConfig` in `create_training_job`\. 

**Note**  
For all Amazon SageMaker algorithms, the `ChannelName` in `InputDataConfig` must be set to `train`\. Some algorithms also support a validation or test `input channels`\. These are typically used to evaluate the model's performance by using a hold\-out dataset\. Hold\-out datasets are not used in the initial training but can be used to further tune the model\.

## Trained Model Deserialization<a name="td-deserialization"></a>

Amazon SageMaker models are stored as model\.tar\.gz in the S3 bucket specified in `OutputDataConfig` `S3OutputPath` parameter of the `create_training_job` call\. You can specify most of these model artifacts when creating a hosting model\. You can also open and review them in your notebook instance\. When `model.tar.gz` is untarred, it contains `model_algo-1`, which is a serialized Apache MXNet object\. For example, you use the following to load the k\-means model into memory and view it: 

```
import mxnet as mx
print(mx.ndarray.load('model_algo-1'))
```