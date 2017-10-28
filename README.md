# log monitoring and tracing

## tracing

### steps
### configure zipkin server:
 1. add dependency: compile('io.zipkin.java:zipkin-server') 
                   runtime('io.zipkin.java:zipkin-autoconfigure-ui')
 2. ./gradlew bootrun
 3. check [zipkin dashboard](http://localhost:${port}/zipkin)
 
### configure trace client(for both hello-api and user-api):
 1. add dependency: compile('org.springframework.cloud:spring-cloud-starter-sleuth')
 2. add log for http request(slf4j)
 3. start application check log
 4. add dependency:compile('org.springframework.cloud:spring-cloud-starter-zipkin')
 5. add sampler
 
## log monitoring

### steps
### configure log
 1. add log output file
 2. start application, check if log file is generated
 
### configure splunk forwarder
 1. Configure Forwarder connection to Index Server: ./splunk add forward-server hostname.domain:9997
 2. restart the forwarder
 3. go to splunk dashboard, enable the 9997 port of the indexer(setting->forwarding & receiving->receive data add new)
 4. check if the forwarder and server's connection is good: ./splunk list forward-server
 5. search index=_internal in plunk, check the log
 6. add monitor for our log file: ./splunk add monitor /path/to/app/logs/ -index ${index} -sourcetype ${sourcetype}
 7. restart the forwarder
 8. add index for server(settings->indexes->new index)
 9. call our api
 10. search our index in splunk, check the log.