chmod 600 ~/.kube/config

###https://artifacthub.io/packages/helm/prometheus-community/kube-prometheus-stack


helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
kubectl create namespace monitoring
helm install prometheus prometheus-community/kube-prometheus-stack --namespace monitoring --values ./values.yaml


kubectl apply -f scrape_config.yaml

#### delete old version
kubectl delete crd alertmanagerconfigs.monitoring.coreos.com
kubectl delete crd alertmanagers.monitoring.coreos.com
kubectl delete crd podmonitors.monitoring.coreos.com
kubectl delete crd probes.monitoring.coreos.com
kubectl delete crd prometheusagents.monitoring.coreos.com
kubectl delete crd prometheuses.monitoring.coreos.com
kubectl delete crd prometheusrules.monitoring.coreos.com
kubectl delete crd scrapeconfigs.monitoring.coreos.com
kubectl delete crd servicemonitors.monitoring.coreos.com
kubectl delete crd thanosrulers.monitoring.coreos.com

### alert for cpu
100 - (avg by (instance)(irate(node_cpu_seconds_total{instance="192.168.0.132:9100",mode="idle"}[1m]))) * 100