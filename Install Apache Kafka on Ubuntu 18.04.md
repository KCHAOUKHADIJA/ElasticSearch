# Install Apache Kafka on Ubuntu 18.04
### Step 1 – Install Java
Apache Kafka required Java to run. You must have java installed on your system

    $ sudo apt update
    $ sudo apt install default-jdk
### Step 2 – Download Apache Kafka
Download the Apache Kafka binary files 
official site https://kafka.apache.org/downloads

    $ wget https://www.apache.org/dist/kafka/3.1.0/kafka_2.12-3.1.0.tgz
    
Then extract the archive file

    $ tar xzf kafka_2.13-2.7.0.tgz
    $ mv kafka_2.13-2.7.0 /usr/local/kafka
### Step 3 – Setup Kafka Systemd Unit Files
Create systemd unit file for Zookeeper :

    $ sudo nano /etc/systemd/system/zookeeper.service
Add below content :

    [Unit]
    Description=Apache Zookeeper server
    Documentation=http://zookeeper.apache.org
    Requires=network.target remote-fs.target
    After=network.target remote-fs.target

    [Service]
    Type=simple
    ExecStart=/usr/local/kafka/bin/zookeeper-server-start.sh /usr/local/kafka/config/zookeeper.properties
    ExecStop=/usr/local/kafka/bin/zookeeper-server-stop.sh
    Restart=on-abnormal

    [Install]
    WantedBy=multi-user.target
Create a Kafka systemd unit file :

    $ sudo nano /etc/systemd/system/kafka.service
Add below content :

    [Unit]
    Description=Apache Kafka Server
    Documentation=http://kafka.apache.org/documentation.html
    Requires=zookeeper.service

    [Service]
    Type=simple
    Environment="JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64"
    ExecStart=/usr/local/kafka/bin/kafka-server-start.sh /usr/local/kafka/config/server.properties
    ExecStop=/usr/local/kafka/bin/kafka-server-stop.sh

    [Install]
    WantedBy=multi-user.target

Reload the systemd daemon to apply new changes

    $ systemctl daemon-reload
    
Change log directory in services.property

    $ sudo nano /usr/local/kafka-server/config/server.properties
    log.dir = /usr/local/kafka-server/kafka.log

### Step 4 – Start Kafka Server
    $ sudo systemctl start kafka
    $ sudo systemctl start zookeeper
    
### Step 5 – Create a Topic in Kafka
    $ cd /usr/local/kafka
    $ ./bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic firstTopic

The replication factor describes how many copies of data will be created. As we are running with a single instance keep this value 1.
Set the partitions options as the number of brokers you want your data to be split between. As we are running with a single broker keep this value 1.

##### list all topics
    $ ./bin/kafka-topics.sh --list --bootstrap-server localhost:9092 
##### describe the topic (properties)
    $ ./bin/kafka-topics.sh --describe --bootstrap-server localhost:9092 --topic firstTopic
##### create a producer and add data in the topic
    $ bin/kafka-console-producer.sh --topic firstTopic --bootstrap-server localhost:9092
##### create a consumer and read data from producer
    $ bin/kafka-console-consumer.sh --topic firstTopic --from-beginning --bootstrap-server localhost:9092


