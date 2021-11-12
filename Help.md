To install and run our Kafka cluster, we need to run the following command in the same directory as our docker-compose.yml file:

docker-compose up -d

The -d flag will cause the Kafka cluster to run in the background. Now, we need to wait for Kafka to become available by running the following command.

```
while ! nc -z localhost 29092; do   
sleep 1
echo "Waiting on Kafka to launch on 29092..."
done

echo "Kafka is running"
```

Once you see the words `Kafka is running` printed to the

## Interacting with the Kafka Cluster
Log into the kafka container
```
docker-compose exec kafka bash
```

###Create a Kafka Topic
Before we start developing our Kafka Streams application, we should pre-create any topics we expect it to interact with. For example, to create a Kafka topic named tweets, we can run the following command:

```aidl
kafka-topics \
    --bootstrap-server localhost:9092 \
    --create \
    --topic tweets \
    --partitions 4
```

#### List all topics
```aidl
kafka-topics \
  --bootstrap-server localhost:9092 \
  --list
```

#### Manually create a kafka producer
```
kafka-console-producer \
    --bootstrap-server localhost:9092 \
    --topic tweets \
    --property 'key.separator=|' \
    --property 'parse.key=true'
```

#### Manually verify producer with consumer
```markdown
kafka-console-consumer \
    --bootstrap-server localhost:9092 \
    --topic tweets \
    --from-beginning \
    --property print.key=true
```
The `--bootstrap-server` and `--topic` flags tell the script to which Kafka cluster and topic we intend to produce data.

#### Producing Test Data From a File
```markdown
  kafka-console-producer \
  --bootstrap-server localhost:9092 \
  --topic tweets \
  --property 'parse.key=true' \
  --property 'key.separator=|' < /data/inputs.txt
```
