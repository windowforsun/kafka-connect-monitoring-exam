

https://docs.confluent.io/platform/current/kafka/monitoring.html

https://docs.confluent.io/platform/current/connect/monitoring.html

https://debezium.io/documentation/reference/stable/operations/monitoring.html



### run connector

```bash
curl -X POST -H "Content-Type: application/json" \
--data @docker/file-source.json \
http://localhost:8083/connectors | jq
```  



### check connector config

```bash
$ curl localhost:8083/connectors/exam-file-source | jq
```

### check connector

```bash
$ curl localhost:8083/connectors/exam-file-source/status | jq
```


### delete connector

```bash
$ curl -X DELETE localhost:8083/connectors/cdc-outbox | jq
```

```bash
$ curl -X DELETE localhost:8084/connectors/cdc-outbox | jq
```


### insert field

```bash
$ docker exec -it exam-db \
mysql -uroot -proot -Dexam -e "insert into test_table(value) values('a')"
```


### check cdc topic

```bash
$ docker exec -it myKafka kafka-console-consumer.sh \
--bootstrap-server localhost:9092 \
--topic file-source-topic \
--property print.key=true \
--property print.headers=true \
--from-beginning 
```  


### check kafka connect metrics export

```bash
$ curl localhost:5556/metrics
```


```bash
$ curl localhost:5556/metrics | grep 'kafka_connect'
```

### check prometheus

localhost:9090

```bash
$ curl localhost:9090/api/v1/label/__name__/values
```

```bash
$ curl "http://localhost:9090/api/v1/query?query=kafka_connect_connect_worker_metrics_task_count" | jq
```

```bash
$ curl "http://localhost:9090/api/v1/targets?state=active&scrapePool=kafka-connect" | jq
```

### check grafana

localhost:3000