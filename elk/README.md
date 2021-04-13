## ELK
```
Elasticsearch - máy chủ lưu trữ và tìm kiếm dữ liệu
Logstash - thành phần xử lý dữ liệu, sau đó nó gửi dữ liệu nhận được cho Elasticsearch để lưu trữ
Kibana - ứng dụng nền web để tìm kiếm và xem trực quan các logs
Beats - gửi dữ liệu thu thập từ log của máy đến Logstash
/etc/kibana/kibana.yml
/etc/filebeat/filebeat.yml
filebeat modules enable system ...
```

### logstasg
```
echo 'input {
  beats {
    host => "0.0.0.0"
    port => 5044
  }
}' > /etc/logstash/conf.d/02-beats-input.conf
====
echo 'output {
  elasticsearch {
    hosts => ["localhost:9200"]
    manage_template => false
    index => "%{[@metadata][beat]}-%{[@metadata][version]}-%{+YYYY.MM.dd}"
  }
}' > /etc/logstash/conf.d/30-elasticsearch-output.conf
```
![ELK](elk.png)