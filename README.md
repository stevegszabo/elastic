# elastic

```
helm repo add elastic https://helm.elastic.co
helm repo update

helm -n default install eck-crds eck-operator-crds/ 
helm -n base-eck install eck eck-operator/ \
--set=installCRDs=false \
--set=managedNamespaces='{elastic-01}' \
--set=createClusterScopedResources=false \
--set=webhook.enabled=false \
--set=config.validateStorageClass=false
```

```
ELASTIC_PASSWORD=$(kc -n elastic-01 get secret elastic-01-es-elastic-user -o json | jq -r .data.elastic | base64 -d)

kc -n elastic-01 port-forward svc/elastic-01-es-http 9200:9200

curl -v -k -u "elastic:$ELASTIC_PASSWORD" https://localhost:9200/
curl -v -k -u "elastic:$ELASTIC_PASSWORD" https://localhost:9200/_cluster/health?pretty | jq -r .
curl -v -k -u "elastic:$ELASTIC_PASSWORD" https://localhost:9200/_cat
curl -v -k -u "elastic:$ELASTIC_PASSWORD" https://localhost:9200/_cat/master?v
curl -v -k -u "elastic:$ELASTIC_PASSWORD" https://localhost:9200/_cat/nodes?v
curl -v -k -u "elastic:$ELASTIC_PASSWORD" https://localhost:9200/_cat/tasks?v
curl -v -k -u "elastic:$ELASTIC_PASSWORD" https://localhost:9200/_cat/health?v
curl -v -k -u "elastic:$ELASTIC_PASSWORD" https://localhost:9200/_cat/indices?v
curl -v -k -u "elastic:$ELASTIC_PASSWORD" https://localhost:9200/_cat/shards?v
curl -v -k -u "elastic:$ELASTIC_PASSWORD" https://localhost:9200/_cat/segments?v

curl -v -k -u "elastic:$ELASTIC_PASSWORD" https://localhost:9200/_nodes/elastic-01-es-master-0 | jq -r .
curl -v -k -u "elastic:$ELASTIC_PASSWORD" https://localhost:9200/_nodes/elastic-01-es-master-1 | jq -r .
curl -v -k -u "elastic:$ELASTIC_PASSWORD" https://localhost:9200/_nodes/elastic-01-es-master-2 | jq -r .

curl -v -k -u "elastic:$ELASTIC_PASSWORD" https://localhost:9200/_nodes/elastic-01-es-data-0 | jq -r .
curl -v -k -u "elastic:$ELASTIC_PASSWORD" https://localhost:9200/_nodes/elastic-01-es-data-1 | jq -r .
curl -v -k -u "elastic:$ELASTIC_PASSWORD" https://localhost:9200/_nodes/elastic-01-es-data-2 | jq -r .

curl -v -k -u "elastic:$ELASTIC_PASSWORD" https://localhost:9200/_data_stream | jq -r .data_streams[].name

curl -v -k -u "elastic:$ELASTIC_PASSWORD" https://localhost:9200/.ds-logs-kubernetes.container-default-2025.08.24-000016/ | jq -r .
curl -v -k -u "elastic:$ELASTIC_PASSWORD" https://localhost:9200/.ds-logs-kubernetes.container-default-2025.08.24-000016/_search?q=argo | jq -r .
curl -v -k -u "elastic:$ELASTIC_PASSWORD" https://localhost:9200/.ds-logs-kubernetes.container-default-2025.08.24-000016/_search?q=argo | jq -r .hits.hits[0]
```
