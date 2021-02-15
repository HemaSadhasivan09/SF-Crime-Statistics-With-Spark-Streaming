# SF Crime Statistics With Spark Streaming
Project for Udacity Nanodegree program by Hema Sadhasivan

[![Project passed](https://img.shields.io/badge/project-passed-success.svg)](https://img.shields.io/badge/project-passed-success.svg)

## Introduction
In this project, a real-world dataset has been provided, extracted from Kaggle, on San Francisco crime incidents, and statistical analyses of the data using Apache Spark Structured Streaming need to be provided. A Kafka server will be generated to produce data and ingest it through Spark Structured Streaming.

## Resources
The files below have been included in the Project Workspace, containing three Python files that are starter code, the project dataset, and some other necessary resources.

- `producer_server.py`
- `kafka_server.py`
- `data_stream.py`
- `police-department-calls-for-service.json`
- `radio_code.json`
- `start.sh`
- `requirements.txt`

The following file should be created separately for you to check if your `kafka_server.py` is working properly:
`consumer_server.py`
   
## Project Steps 
- Come up with a Kafka topic name ("pd.service.calls") and choose a port number (9092)
- Edited the files `producer_server.py, `data_stream.py, and `kafka_server.py to complete all the "To Do" requests.
- Created consumer_server.py to check if your kafka_server.py is working properly.
- Modified the zookeeper.properties and server.properties appropriately
- Ran the project using several terminals as follow:
    - Terminal 1: To run `./start.sh`
    - Terminal 2: To run `/usr/bin/zookeeper-server-start config/zookeeper.properties`
    - Terminal 3: To run `/usr/bin/kafka-server-start config/server.properties`
    - Terminal 4: To run `python kafka_server.py`
    - Terminal 5: To run `/usr/bin/kafka-console-consumer --bootstrap-server localhost:9092 --topic pd.service.calls --from-beginning`
    - Terminal 6: To run `spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.11:2.3.4 --master local[*] data_stream.py`

## Results
The project was successfully implemented with no errors.

After running the codes from terminal 1 to 5, I was able to get the Kafka Consumer Console working. Screen capture is shown below.

##### Kafka_Consumer_Console

Pic needed

Additionally, after running the code in terminal 6 I was able to execute the Spark jobs and retrieve the Progress Reporter and Spark UI screens capture shown below.

##### Spark_Job_Progress_Reporter

Pic needed

##### Spark_Streaming_UI

Spark-UI Pic needed
    

## Project Questions

##### 1. How did changing values on the SparkSession property parameters affect the throughput and latency of the data?
I compared several values of maxOffsetPerTrigger and maxRatePerPartition. It looks like the impact on throughput and on the latency of data is not much but one reason could be it is because of the size of the datasete. However, based on the literature, very large flush intervals may lead to latency spikes. When the flush actually takes place, usually a lot of data needs to be flushed. Moreover, it is an expensive operation, and a small flush interval may lead to excessive seeks. So, I conclude by saying that there is not constant optimum flush value. It has to be tested for different values for each dataset to determine optimum value.
    
##### 2. What were the 2-3 most efficient SparkSession property key/value pairs? Through testing multiple variations on values, how can you tell these were the most optimal?
A little bit of tuning is necessary to obtain the best performance out of a Spark Streaming application. In genereal, we need to consider the below options:

1. Firstly, an appropriate batch size needs to be set so that the batches of data can be processed as fast as they are received (i.e., data processing rate should keep up with the data ingestion rate).

2. Secondly, reducing the processing time of each batch of data by efficiently utilizing the cluster resources.

Also, I did modify the configuration values as mentioned below to check for its effects. MaxRatePerPartition and the MaxOffsetPerrtigger at 100 seems to work well. As per the literature, achieving Parallelism can improve the performance. It is important to carefully choose the configuration parameter values rather than accepting the default values. It can be more like a trial and error method depending on various factors like dataset size, data processing speed, data ingestion rate, available resources and the like.

`*spark.streaming.backpressure.enabled : true
 *spark.streaming.kafka.maxRatePerPartition : 100
 *spark.sql.shuffle.partitions : 100
 *spark.default.parallelism : 100
 `
    
## Resources
A few issues were encountered during the project. I search some of the Knowledge center posts and used them as appropriate.
1. Not able to read Sample data in Line https://knowledge.udacity.com/questions/369228

2. Unable to see data on Spark UI https://knowledge.udacity.com/questions/187420

3. Config files not working as expected https://knowledge.udacity.com/questions/80037
