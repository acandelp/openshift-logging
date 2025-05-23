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

### 3)Troubleshooting


#### 3.1) How to narrow down the problem?

- Is the issue only happening for a concrete user?
- Is the issue only happening for a concrete application?
- Is the issue only happening for the pods/applications allocated in a concrete node?
- Logging must-gather
- Kibana screenshot

#### 3.2) Must-gather paths
```
Operator version: /must-gather.local/registry-redhat-io-openshift-logging-cluster-logging-rhel8-operator-sha256-ad436419a03ffd76260906448cbcea146357dd102b89e791fc84be75ea30bb0f/cluster-logging/install
ClusterLogging instance (from 5.7): /must-gather.local/registry-redhat-io-openshift-logging-cluster-logging-rhel9-operator-sha256-463bdc2944690d56eb597314a7045fdb751c56e28bc6d45b279f194d4695be0f/namespaces/openshift-logging/logging.openshift.io/clusterloggings
ClusterLogging instance (prior 5.7): /must-gather.local/registry-redhat-io-openshift-logging-cluster-logging-rhel8-operator-sha256-ad436419a03ffd76260906448cbcea146357dd102b89e791fc84be75ea30bb0f/cluster-logging/clo
ClusterLogForwarder instance (from 5.7): /must-gather.local/registry-redhat-io-openshift-logging-cluster-logging-rhel9-operator-sha256-463bdc2944690d56eb597314a7045fdb751c56e28bc6d45b279f194d4695be0f/namespaces/openshift-logging/logging.openshift.io/clusterlogforwarders
ClusterLogForwarder instance (prior 5.7): /must-gather.local/registry-redhat-io-openshift-logging-cluster-logging-rhel8-operator-sha256-ad436419a03ffd76260906448cbcea146357dd102b89e791fc84be75ea30bb0f/cluster-logging/clo
Elasticsearch: /must-gather.local/registry-redhat-io-openshift-logging-cluster-logging-rhel9-operator-sha256-463bdc2944690d56eb597314a7045fdb751c56e28bc6d45b279f194d4695be0f/cluster-logging/es/cluster-elasticsearch
Collector Buffer: /must-gather.local/registry-redhat-io-openshift-logging-cluster-logging-rhel8-operator-sha256-ad436419a03ffd76260906448cbcea146357dd102b89e791fc84be75ea30bb0f/cluster-logging/collector
```


#### 3.3) If the customer cannot collect a must-gather
```
$ oc adm inspect ns/openshift-logging (also add the generated file)
$ oc -n openshift-logging get clusterlogging instance -o yaml > clo.txt
$ oc -n openshift-logging get clusterLogForwarder instance -o yaml > clf.txt
$ oc -n openshift-logging get csv > csv.txt
$ for pod in `oc get po -l component=elasticsearch -o jsonpath='{.items[*].metadata.name}'`; do echo $pod; oc exec -c elasticsearch $pod -- df -h /elasticsearch/persistent; done
$ for i in $(oc get pods -l component=collector | awk '/collector/ { print $1 }') ; do oc exec $i -- du -sh /var/lib/fluentd  ; done

$ oc rsh -c elasticsearch <elasticsearchpod>
# es_util --query=_cat/health?v
# es_util --query=_cat/nodes?v
# es_util --query="_cat/indices?h=health,status,index,id,pri,rep,docs.count,docs.deleted,store.size,creation.date.string&v="
```

#### 3.4)Common checks

- Logging Operator Version and Elasticsearch Operator version
- ClusterLogging Managed status
- ClusterLogging instance
- ClusterLogForwarder instance
- Elasticsearch status
- Collector Logs
- Elasticsearch Logs
- Buffer status
- Kibana Logs

#### 3.5) [Elasticsearch alerts troubleshooting](https://docs.openshift.com/container-platform/4.10/logging/troubleshooting/cluster-logging-troubleshooting-for-critical-alerts.html)

#### 3.6) Forwarding troubleshooting
- [New Commers Check List](https://docs.google.com/spreadsheets/d/1M5DT-GiFVJt9PYAjPnMQBRCvm6tKu4KurtkRzEg0LLA/edit?gid=1234451132#gid=1234451132) Understanding and troubleshooting fluentd part 1 and 2.

- Examples:
[Forwarding logs using syslog fails with error EMSGSIZE in OpenShift 4](https://access.redhat.com/solutions/5873961), [Elasticsearch reporting MapperParsingException](https://access.redhat.com/solutions/3986441)

#### 3.7) [Reproducers](https://gitlab.cee.redhat.com/aosqe/aosqe-tools/-/blob/master/logging/log_doc/deploy-log-receivers.md)


