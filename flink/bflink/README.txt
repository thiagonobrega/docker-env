# 1. Build the images

## 1.1. Apache flink
docker build -t myfl .

Flink default ports
These are the default ports used by the Flink image:

    The Web Client is on port 8081
    JobManager RPC port 6123
    TaskManagers RPC port 6122
    TaskManagers Data port 6121

## 2.2. Kafka
docker build -t kafka -f Dockerfile.kafka .

# 2. Power on the basic cluster

FLINK_DOCKER_IMAGE_NAME=myfl KAFKA_IP=<<MASTER-PUBLIC-IP>> docker-compose up -d

## 2.1. Scale
FLINK_DOCKER_IMAGE_NAME=myfl docker-compose scale taskmanager=8

# not needed
#FLINK_DOCKER_IMAGE_NAME=myfl docker-compose scale kafka=<n>

## 2.2. Power down
FLINK_DOCKER_IMAGE_NAME=myfl KAFKA_IP=192.168.16.13 docker-compose down



# 3. Using


## get the zookeper ip

docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}'  $(docker ps | grep zookeeper| awk -F ' ' '{print $1}')

## Coping files to the container

docker cp /tmp/tmpaddon bflink_kafka_1:/tmp/

## Accessing the kafka server
docker exec -it bflink_kafka_1 /bin/bash



# 4. Extras


docker-compose scale kafka=3

start-kafka-shell.sh <DOCKER_HOST_IP> <ZK_HOST:ZK_PORT>
$KAFKA_HOME/bin/kafka-topics.sh --create --topic topic --partitions 4 --zookeeper $ZK --replication-factor 2
$KAFKA_HOME/bin/kafka-topics.sh --describe --topic topic --zookeeper $ZK
$KAFKA_HOME/bin/kafka-console-producer.sh --topic=topic --broker-list=`broker-list.sh`
