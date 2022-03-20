# Install Apache Kafka on Ubuntu 18.04 
### 1. Install Java
Update system package list.

    $ sudo apt update
Install Java.

    $ sudo apt install openjdk-11-jre-headless -y
Verify the installation.

    $ java --version

### 2. Install Apache Kafka
Download Apache Kafka source files. 

    $ wget https://dlcdn.apache.org/kafka/3.0.0/kafka_2.12-3.1.0.tgz
Create an installation directory /usr/local/kafka-server.

    $ sudo mkdir /usr/local/kafka-server
Extract the downloaded files.

    $ sudo tar -xzf kafka_2.12-3.0.0.tgz
Move the extracted files to the installation directory.

    $ sudo mv kafka_2.12-3.0.0/* /usr/local/kafka-server
## For Apache Kafka and Apache Zookeeper to run as daemons, create systemd files for both of them.
Creating systemd file for Apache Zookeeper.

    $ sudo nano /etc/systemd/system/zookeeper.service
Add the following lines of code to the file, save and close the file.

    [Unit]

    Description=Apache Zookeeper Server

    Requires=network.target remote-fs.target

    After=network.target remote-fs.target

    [Service]

    Type=simple

    ExecStart=/usr/local/kafka-server/bin/zookeeper-server-start.sh /usr/local/kafka-server/config/zookeeper.properties

    ExecStop=/usr/local/kafka-server/bin/zookeeper-server-stop.sh

    Restart=on-abnormal

    [Install]

    WantedBy=multi-user.target
    
Creating systemd file for Apache Kafka.

    $ sudo nano /etc/systemd/system/kafka.service

Add the following lines of code to the file, save and close the file.

    [Unit]

    Description=Apache Kafka Server

    Requires=zookeeper.service

    After=zookeeper.service

    [Service]

    Type=simple

    Environment="JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64"

    ExecStart=/usr/local/kafka-server/bin/kafka-server-start.sh /usr/local/kafka-server/config/server.properties

    ExecStop=/usr/local/kafka-server/bin/kafka-server-stop.sh

    Restart=on-abnormal

    [Install]

    WantedBy=multi-user.target

Reload the systemd daemons to apply the changes.

    $ sudo systemctl daemon-reload

Change log directory in services.property

    $ sudo nano /usr/local/kafka-server/config/server.properties
    log.dir = /usr/local/kafka-server/kafka.log
    
Start kafka

    $ sudo systemctl start kafka
    
Ensure that the server has started with the command:

    $ sudo journalctl -u kafka
    $ sudo systemctl status zookeeper
    $ sudo systemctl status kafka

To enable Kafka to reboot with the system, use the command:

    $ sudo systemctl enable kafka

To configure a cluster check : 
https://dzone.com/articles/how-to-setup-kafka-cluster

To create a topic : 

      $ ./bin/kafka-topics.sh --create --topic test-topic --bootstrap-server localhost:9092 --replication-factor 1 --partitions 4
Send data via producer :

      $ bin/kafka-console-producer.sh --topic test-topic --bootstrap-server localhost:9092
Read data from consumer: 

      $ bin/kafka-console-consumer.sh --topic test-topic --from-beginning --bootstrap-server localhost:9092



