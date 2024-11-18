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

### Use Operator Prometheus
* Automated Monitoring Setup
* Dynamic Service Discovery
* High Availability Monitoring
```bash
# OLM (operator lifecycle manager)
curl -sL https://github.com/operator-framework/operator-lifecycle-manager/releases/download/v0.28.0/install.sh | bash -s v0.28.0
# Prometheus operator install
kubectl apply -f https://operatorhub.io/install/prometheus.yaml
# Prometheus install by operator
kubectl apply -f - <<EOF
apiVersion: monitoring.coreos.com/v1
kind: Prometheus
metadata:
  name: prometheus
spec:
  serviceAccountName: prometheus
  serviceMonitorSelector:
    matchLabels:
      team: test
  ruleSelector:
    matchLabels:
      team: test
  resources:
    requests:
      memory: 400Mi
EOF

kubectl patch svc prometheus-operated -p '{"spec": {"type": "NodePort"}}'

```

```
###Clear
```bash
docker-compose down --rmi all -v --remove-orphans
docker stop $(docker ps -a -q)
docker system prune -a
```
###
```bash
kubectl -n monitoring delete all -l app=hey
kubectl -n monitoring delete deployment prometheus-deployment
kubectl -n monitoring delete svc prometheus-service
kubectl -n monitoring delete cm prometheus-config
kubectl -n monitoring delete service hey-service
```

