# Testing monitoring on k8s "Prometheus and Grafana" with Helm

### Prometheus
> Test name: http://prometheus-prod.com/

### https://artifacthub.io/packages/helm/prometheus-community/prometheus

* $ mkdir prometheus
* $ helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
* $ helm repo update
* $ helm search repo prometheus
* $ helm search repo prometheus --version 19.7.2
* $ helm show values prometheus-community/prometheus --version 19.7.2 | tee -a prometheus/values.yaml
* $ helm list --all-namespaces
* $ helm install prometheus  prometheus-community/prometheus -f prometheus/values.yaml -n prometheus --create-namespace --debug --dry-run >> result-prometheus.log
* $ helm uninstall prometheus -n prometheus
* $ kubectl get pod,svc,pv -n prometheus
* $ kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-ext -n prometheus
* $ minikube service prometheus-server-ext -n prometheus

### Grafana
> Test name: http://grafana-prod.com/

### https://artifacthub.io/packages/helm/grafana/grafana

* $ mkdir grafana
* $ helm repo add grafana https://grafana.github.io/helm-charts
* $ helm repo update
* $ helm search repo grafana --version 6.52.4
* $ helm show values grafana/grafana --version 6.52.4 | tee -a grafana/values.yaml
* $ helm install grafana grafana/grafana -f grafana/values.yaml -n grafana --create-namespace --debug --dry-run
* $ kubectl get pod,svc,pv -n grafana
* $ kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-ext -n grafana

1. Get your 'admin' user password by running:
   $ kubectl get secret --namespace grafana grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
2. From outside the cluster, the server URL(s) are:
     http://grafana-prod.com

* $ minikube service grafana-ext -n grafana

### Integrating Grafana and Prometheus

1. On the dashboard, click on the  ‘Add data source’ option.
2. Because we are interested in Prometheus integration, simply click on the ‘Prometheus’ option.
3. Type the Prometheus server address in the URL text field.
4. Click the  ‘Save and Test’ button and you should get the output indicating ‘Data source is working’.
5. To create a dashboard for visualizing metrics, click on the plus sign on the left sidebar and click on ‘import’.

### Exampl:
> https://grafana.com/grafana/dashboards/15172-node-exporter-for-prometheus-dashboard-based-on-11074/

6. Click 'Dashboard ID copied!'.
7. Paste the link in the URL section and load.
8. Change the Prometheus Data Source Name as “Prometheus” and then Click on ‘Import’.

Grafana will begin fetching the metrics from the Prometheus server and visualize then in colorful and intuitive dashboards.