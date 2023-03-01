# monitoring_prometheus_grafana
Config demo for monitoring (pull model)

###Build and run app_java
```bash
git clone https://github.com/geksogen/monitoring_prometheus_grafana.git
cd monitoring_prometheus_grafana/app/java_app
docker build -t java-spring-hello .
docker run -p 8080:8080 --rm --name=java-spring-hello java-spring-hello
```

###Test
```
curl localhost:8080
curl localhost:8080/actuator/prometheus | grep jvm_memory_max
```

###Install and configure prometheus
```
cd ../../prometheus/
docker-compose up -d
```

###Test
```
docker-compose ps

         Name                        Command               State                    Ports                  
-----------------------------------------------------------------------------------------------------------
prometheus_prometheus_1   /bin/prometheus --web.enab ...   Up      0.0.0.0:9000->9090/tcp,:::9000->9090/tcp
```
Go to http://<ip-host>:9000/ UI prometheus

