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
cd ../../prometheus_grafana/
docker-compose up -d
```

###Test
```
docker-compose ps

        Name                        Command               State                    Ports                  
-----------------------------------------------------------------------------------------------------------
prometheus_grafana_1      /run.sh                          Up      0.0.0.0:3000->3000/tcp,:::3000->3000/tcp
prometheus_prometheus_1   /bin/prometheus --web.enab ...   Up      0.0.0.0:9000->9090/tcp,:::9000->9090/tcp

```
http://<ip-host>:9000/ UI prometheus
http://<ip-host>:3000/ UI grafana (admin/admin)

# logging stack Promtail, Loki
```bash
cd cd promtail_loki
docker-compose up -d
```
Add datasource to Grafana
http://<IP>:3100/

###k8s
```
kubectl create ns monitoring
git clone https://github.com/geksogen/monitoring_prometheus_grafana.git
cd k8s/app
kubectl apply -f .
```
### Test
```
kubectl -n monitoring run -i -t nginx --rm=true --image=nginx -- bash
curl hey-service.monitoring.svc:8000
curl hey-service.monitoring.svc:8000/metrics
```
### Deploy Prometheus
cd ../prometheus/
kubectl apply -f .


###Clear
```bash
docker-compose down --rmi all -v --remove-orphans
docker stop $(docker ps -a -q)
docker system prune -a

kubectl -n monitoring delete all -l app=hey
kubectl -n monitoring delete deployment prometheus-deployment
kubectl -n monitoring delete svc prometheus-service
kubectl -n monitoring delete cm prometheus-config
kubectl -n monitoring delete service hey-service
```

