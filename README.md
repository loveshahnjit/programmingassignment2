The training data, validation data, and trained model data are uploaded to the s3 bucket: 

Steps to run model training on 4 parallel Ec2 instances using EMR on AWS:
(1) Login to AWS console
  (a) Create a IAM role for a EC2 Instance so that it can give access to s3 bucket.
  (b) Ec2 instance should be able to download the csv file and upload the file (model file) to s3.
(2) Create a cluster 
  (a) Go to launch mode and select step execution
  (b) Go to step type and select Spark Application and click on the Configure button
    (1) Set deploy mode = client
    (2) Set spark-submit options = --packages org.apache.hadoop-aws.2.7.7 
    (3) Set application location = s3://programmingassignment2/pa2.py
    (4) Set action on failure = Terminate cluster
  (c) Go to software configuration and choose Spark.
  (d) Go to hardware configuration and set Number of Instances to 4.
  (e) Click the button to create the cluster and let it run.
  (f) To see the standard output, go to the step tab and select view logs for the Spark application.
  
  Steps to run the model prediction on a single Ec2 Instance without using Docker at all.
  (1) Make sure Ec2 instance is created and has spark installed.
  (2) Copy the wine prediction file ('file_name.py') using: $scp -i <"your .pem file"> file_name.py :~/file_name.py)
  (3) Run one of 2 commands below in the EC2 instance in order to run the model prediction:
    (a) If youre using the S3 file: $spark-sumbit --packages org.apache.hadoop:hadoop-aws:2.7.7 file_name.py --test_file s3a://programmingassignment2/ValidationDataset.csv
    (b) If you're using a local file stored in the EC2 instance: $spark-submit --packages org.apache.hadoop-aws:2.7.7 file_name.py --test_file ValidationDataset.csv 

Steps to run the model prediction on a single EC2 Instance using Docker:
(1) Pretty straightforward, just need to make sure you have a ec2 instance with docker installed.
(2) Ssh into the Ec2 instance.
(3) $docker pull loveshahnjit/programmingassignment2
(3) $docker run -it loveshahnjit/wineapp:latest --test_file s3a://programmingassignment2/ValidationDataset.csv
