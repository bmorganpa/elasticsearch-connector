# Elasticsearch connector demo

## Setup
1. Download confluentinc-kafka-connect-elasticsearch-11.1.3.zip from https://www.confluent.io/hub/confluentinc/kafka-connect-elasticsearch
1. Place extracted directory under connect_plugins/ folder at the root of the repository
1. Run `docker-compose up`
1. If any of the containers fails just restart with `docker-compose restart <container_name>`

## Creating connection
Run the following curl command to create the connection:
```
curl --location --request POST 'http://localhost:8083/connectors' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "<insert_name>",
    "config": {
        "connector.class": "io.confluent.connect.elasticsearch.ElasticsearchSinkConnector",
        "connection.url": "http://elasticsearch:9200",
        "tasks.max": "1",
        "topics": "<insert_topic_name>",
        "name": "<insert_name>",
        "type.name": "_doc",
        "key.converter": "org.apache.kafka.connect.storage.StringConverter",
        "value.converter": "org.apache.kafka.connect.json.JsonConverter",
        "value.converter.schemas.enable": "false",
        "schema.ignore": "true"
    }
}'
```

## Populating the topic
Pipe messages into the kafka topic using kafkacat:
```
cat messages.txt | kafkacat -P -b localhost:9092 -K\| -t <insert_topic_name>
```
