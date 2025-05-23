## Logging 5 Collector and Elasticsearch (deprecated)

### 1)Architecture

[Architecture VIDEO](https://drive.google.com/file/d/19-sXFKFRS1i9ct_QC8HMdQj2KmCKf-PB/view?usp=drive_link)
- [Logging Architecture](https://docs.google.com/presentation/d/1yK2qdHTWmQiDrcRbgEEa-nkveaPRFlIVgAGDzyethR8/edit#slide=id.gd70e42a98c_0_714) 
- [Fluentd - Limitation of the OpenShift Logging Architecture](https://access.redhat.com/solutions/3329761)
- [Vector - Tuning log payloads and delivery in RHOCP 4](https://access.redhat.com/solutions/7074148)

- [Support](https://docs.openshift.com/container-platform/4.14/observability/logging/cluster-logging-support.html)


### 2)[Installation and Deployment](https://docs.openshift.com/container-platform/4.14/observability/logging/cluster-logging-deploying.html)
[Installation and Deployment VIDEO](https://drive.google.com/file/d/1eBqDAiXHp_Q3gkIJCLT4a5Ne_HhC_yDO/view?usp=drive_link)

- [Configuring the logging Collector](https://docs.openshift.com/container-platform/4.14/observability/logging/log_collection_forwarding/cluster-logging-collector.html)
  
- [Configuring the Elasticsearch log store](https://docs.openshift.com/container-platform/4.14/observability/logging/log_storage/logging-config-es-store.html)


- [Log visualization with Kibana](https://docs.openshift.com/container-platform/4.14/observability/logging/log_visualization/logging-kibana.html)


#### 2.1) Installation Health Check
[Installation Health Check VIDEO](https://drive.google.com/file/d/1q_4DKt5ntH60wNrdY8StkJw-Jnvmx9-3/view?usp=drive_link)
```
Collector Logs
```
```
Collector Buffers:
for i in $(oc get pods -l component=collector --no-headers | grep -i running | awk '{print $1}'); do echo $i; oc exec $i -- /bin/bash -c "ls /var/lib/fluentd/*"; done
```
```
Elasticsearch Health:
oc rsh -c elasticsearch <elasticsearchpod>
es_util --query="_cat/indices?h=health,status,index,id,pri,rep,docs.count,docs.deleted,store.size,creation.date.string&v="
es_util --query=_cat/health?v
es_util --query=_cat/nodes?v
es_util --query=$INDEXNAME/_search?pretty
es_util --query=_cat/aliases?v
es_util --query=_cat/allocation?v
es_util --query=_cat/thread_pool?v
```
```
Checking the logs in Kibana UI
```
#### 2.2) ClusterLogForwarder
[ClusterLogForwarder VIDEO](https://drive.google.com/file/d/12cf4vN5DszMYXq7HlEqU2PlSFAAtjsNp/view?usp=drive_link)
- [Configuring log forwarding](https://docs.redhat.com/en/documentation/openshift_container_platform/4.10/html/logging/cluster-logging-external#cluster-logging-external)
