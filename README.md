# Stock-market-data-analysis-with-the-help-of-kafka
## Data Engineering Project


### Technology Used
- Programming Language - Python
- Amazon Web Service (AWS)
1. S3 (Simple Storage Service)
2. Athena
3. Glue Crawler
4. Glue Catalog
5. EC2
- Apache Kafka


## Architecture 
![![Architecture](https://github.com/Jayanth0721/Stock-market-data-analysis-with-the-help-of-kafka/assets/67009936/d8bf9410-873f-4b7f-b6ab-64c5e445caa1)
]

# Connect with AWS

# Part - 01

- Download & Install Kafka in AWS Machine.

```
wget https://downloads.apache.org/kafka/3.6.1/kafka_2.13-3.6.1.tgz
```
```
tar -xvf kafka_2.12-3.3.1.tgz
```
+ Download & Install Python, library in your local system.
```
pip install kafka-python
```

Step1:-
-------------------------------
Open another cmd (or) terminal window to start kafka

- Connect with your ec2 instance
```
ssh -i "kafka_key.pem" ec2-user@ec2-xx-xx-xxx-xx.us-west-1.compute.amazonaws.com
```
But first ssh to to your ec2 machine as done above

Start Zoo-keeper:
-------------------------------
Duplicate the session & enter in a new console

Go to Kafka Folder
```
cd kafka_2.13-3.6.1
```

```
bin/zookeeper-server-start.sh config/zookeeper.properties
```



Step 2:-
-------------------------------

Start Kafka-server:
----------------------------------------
Duplicate the session & enter in a new console
```
export KAFKA_HEAP_OPTS="-Xmx256M -Xms128M
```
Go to Kafka Folder
```
cd kafka_2.13-3.6.1
```
```
bin/kafka-server-start.sh config/server.properties
```

## if you face memory issue, then try this command:
Method 1:- 
```
free -h (if not solved)
```
```
export KAFKA_HEAP_OPTS="-Xmx512m -Xms256m
```
Method 2:-
```
rm /tmp/kafka-logs/.loc
```
Method 3:- 1.Use the ps command to check for any zombie or lingering Kafka processes. If you find any, terminate them using kill.
```
ps aux | grep kafka
```
Identify the process ID (PID) of any Kafka processes that are still running and use kill to terminate them.
```
	kill -9 <PID>
```

It is pointing to private server , change server.properties so that it can run in public IP 

To do this , you can follow any of the 2 approaches shared below --
```
sudo nano config/server.properties
```
![![sudo](https://github.com/Jayanth0721/Stock-market-data-analysis-with-the-help-of-kafka/assets/67009936/b6f594a8-3a06-42d9-b17b-a294a8305eeb)
]


- change ADVERTISED_LISTENERS to public ip of the EC2 instance

![![ip-address](https://github.com/Jayanth0721/Stock-market-data-analysis-with-the-help-of-kafka/assets/67009936/75e9ff55-1d2a-438f-bb22-3c1a7e003d3e)
]



Create a topic:
-----------------------------
Duplicate the session & enter in a new console

Go to Kafka Folder
```
cd kafka_2.13-3.6.1
```
```
bin/kafka-topics.sh --create --topic stock_connect --bootstrap-server {Put the Public IP of your EC2 Instance:9092} --replication-factor 1 --partitions 1"
```
You can give any name for topic, i have used "stock_connect"

Start Producer:
--------------------------
Duplicate the session & enter in a new console

Go to Kafka Folder
```
cd kafka_2.12-3.3.1
```
Command:-
```
bin/kafka-console-producer.sh --topic stock_connect --bootstrap-server {Put the Public IP of your EC2 Instance:9092}"
```


![![Producer](https://github.com/Jayanth0721/Stock-market-data-analysis-with-the-help-of-kafka/assets/67009936/6a238da1-972d-459a-931a-80bd8d5901bc)
]


Start Consumer:
-------------------------
Duplicate the session & enter in a new console

Go to Kafka Folder
```
cd kafka_2.12-3.3.1
```
Command:-
```
bin/kafka-console-consumer.sh --topic stock_connect --bootstrap-server {Put the Public IP of your EC2 Instance:9092}"
```

![![Consumer](https://github.com/Jayanth0721/Stock-market-data-analysis-with-the-help-of-kafka/assets/67009936/ad8971c3-08ee-4a6e-bbe2-b5dc3dfb4384)
]


- You can send & receive messages by using python.
- Create a file for Producer to send messages.
- Create a file for Consumer to view messages.


# Part - 02

### S3 Bucket
- Create a new S3 Bucket.


## Send Data (CSV) to S3 Bucket, Use Crawler, Athena

- It (CSV) contain **104224** Data Sample

CASE 1:- Uploading the Entire Dataset:

* "You can easily upload the entire dataset to your cloud storage or database using Python, streamlining the process and ensuring efficient data transfer."

CASE 2:- Sending Data Iteratively, One by One:

+ "Alternatively, if you have a large dataset or want to control the data transfer, you can use Python to send data iteratively, sending each record one by one through an iterative process, allowing for more granular control and processing."


![![S3](https://github.com/Jayanth0721/Stock-market-data-analysis-with-the-help-of-kafka/assets/67009936/0fe69ae0-2375-49e4-8d37-f93f4e8169fc)
]



### Crawler

- Create a Crawler, it will make a catalog.

![Crawler](https://github.com/Jayanth0721/Stock-market-data-analysis-with-the-help-of-kafka/assets/67009936/302c7489-911e-4e11-b575-109beadd4b36)


### Athena:-

- Use Athena for Queries (Create a temporary bucket for query results).

![![total samples](https://github.com/Jayanth0721/Stock-market-data-analysis-with-the-help-of-kafka/assets/67009936/986329ba-1d91-450d-9c57-4e99273ba4e4)]
