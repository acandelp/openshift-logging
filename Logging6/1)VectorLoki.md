## Logging 6 (WORK IN PROGRESS)

### 1)Architecture
[Logging 6 Architecture VIDEO](https://drive.google.com/file/d/1YtNF986BtYodAE4IeSBptf27lrLoZTp8/view?usp=drive_link)
- [Logging 6 Architecture](https://docs.google.com/presentation/d/1IyCqnwTKbvF-A8q6bABBUqeT-Ey4zFCw_ntxf6QU48U/edit?slide=id.g5a662668d9_0_5#slide=id.g5a662668d9_0_5)
  
[Loki Architecture VIDEO](https://drive.google.com/file/d/1i71GS-1N2LkZnfz1WAemgUZe78MZwP_4/view?usp=drive_link)
- [Introduction to LokiStack](https://videos.learning.redhat.com/playlist/dedicated/251079123/1_ojvcvz0p/1_24vvknfn)
- [Introduction to LokiStack Operations/Dashboard](https://videos.learning.redhat.com/playlist/dedicated/251079123/1_ojvcvz0p/1_zq29kjud)
- [Upstream documentation](https://grafana.com/docs/loki/latest/get-started/architecture/)

[Architecture differences betweeken Logging 5 and Logging 6 VIDEO](https://drive.google.com/file/d/1DkoS8houF86fxZCm4BXd-bVdeQQ-d6oA/view?usp=drive_link)


### 2)[Installation and Deployment](https://docs.redhat.com/en/documentation/openshift_container_platform/4.17/html/logging/logging-6-0#installing-logging-6-0)

[Installation and Deployment VIDEO]()
 -->PENDING

- [Configuring log forwarding](https://docs.redhat.com/en/documentation/openshift_container_platform/4.17/html/logging/logging-6-0#log6x-clf)

[ClusterLogForwarder VIDEO]()
-->PENDING

#### 2.1) Useful configuration information

- [Tuning log payloads and delivery in RHOCP 4](https://access.redhat.com/solutions/7074148)

- [Performance and reliability tuning](https://docs.openshift.com/container-platform/4.14/observability/logging/performance_reliability/logging-flow-control-mechanisms.html)

- [JSON logs with Loki](https://access.redhat.com/solutions/7048604).
  
- [Why Loki needs block and object storage in RHOCP 4?](https://access.redhat.com/solutions/7062821)

- [LokistackSchemaUpgradesRequired warning alert firing in RHOCP 4](https://access.redhat.com/solutions/7063482)
  
- [Request validation Loki rate limits](https://grafana.com/docs/loki/latest/operations/request-validation-rate-limits/)
  
- [Defining retention for logs in LokiStack in RHOCP4](https://access.redhat.com/solutions/7053212)
  

#### 2.2) Collector and logStorage migration

- [How to migrate Fluentd to Vector in Red Hat OpenShift Logging 5.5+ versions ?](https://access.redhat.com/articles/6999658)

- [Migrating the log collector from fluentd to vector reducing the number of logs duplicated in RHOCP 4](https://access.redhat.com/articles/7063405)

- [Migrating the default log store from Elasticsearch to Loki in OCP 4](https://access.redhat.com/articles/6991632)


### 3)Troubleshooting

[Troubleshooting session with real cases mentioned in 3.8 VIDEO](https://drive.google.com/file/d/1OPEeoI4Un4PBFexo6g_CEjl10eLo1ENv/view)

#### 3.1 Common customer issues
- The customer cannot see the logs in the OCP Console.
- Logs are delayed in the OCP Console.
- [Logging alerts](https://docs.openshift.com/container-platform/4.14/observability/logging/logging_alerts/default-logging-alerts.html).
- [JSON logs with Loki](https://access.redhat.com/solutions/7048604).
- Logs are not sent to the third-party system.
- Some logs are not sent to the third-party system.
- Cluster Log Forwarder configuration.
- [Loki ingester is in 0/1 status](https://access.redhat.com/solutions/7028049)


#### 3.2) How to narrow down the problem?

- Is the issue only happening for a concrete user?
- Is the issue only happening for a concrete application?
- Is the issue only happening for the pods/applications allocated in a concrete node?
- Logging must-gather
- Loki Dashboards
- Collector Dashboards
- Log console screenshot

#### 3.3)Common checks

- Logging Operator Version and Loki Operator version
- ClusterLogging Managed status
- LokiStack instance
- ClusterLogging instance
- ClusterLogForwarder instance
- Collector Logs
- Loki pods logs

#### 3.4) Must-gather paths
```
Operator version: /must-gather.local/registry-redhat-io-openshift-logging-cluster-logging-rhel8-operator-sha256-ad436419a03ffd76260906448cbcea146357dd102b89e791fc84be75ea30bb0f/cluster-logging/install
ClusterLogging instance (from 5.7): /must-gather.local/registry-redhat-io-openshift-logging-cluster-logging-rhel9-operator-sha256-463bdc2944690d56eb597314a7045fdb751c56e28bc6d45b279f194d4695be0f/namespaces/openshift-logging/logging.openshift.io/clusterloggings
ClusterLogging instance (prior 5.7): /must-gather.local/registry-redhat-io-openshift-logging-cluster-logging-rhel8-operator-sha256-ad436419a03ffd76260906448cbcea146357dd102b89e791fc84be75ea30bb0f/cluster-logging/clo
ClusterLogForwarder instance (from 5.7): /must-gather.local/registry-redhat-io-openshift-logging-cluster-logging-rhel9-operator-sha256-463bdc2944690d56eb597314a7045fdb751c56e28bc6d45b279f194d4695be0f/namespaces/openshift-logging/logging.openshift.io/clusterlogforwarders
ClusterLogForwarder instance (prior 5.7): /must-gather.local/registry-redhat-io-openshift-logging-cluster-logging-rhel8-operator-sha256-ad436419a03ffd76260906448cbcea146357dd102b89e791fc84be75ea30bb0f/cluster-logging/clo
LokiStack instance: /must-gather.local/registry-redhat-io-openshift-logging-cluster-logging-rhel9-operator-sha256-695733369faba7b24b5ebecdbb23be5629965072d970f41e7560ad7e7bd20765/cluster-logging/lokistack
Vector config file: /must-gather.local/registry-redhat-io-openshift-logging-cluster-logging-rhel9-operator-sha256-695733369faba7b24b5ebecdbb23be5629965072d970f41e7560ad7e7bd20765/cluster-logging/clo/openshift-logging/collector-config_vector.toml
```


#### 3.5) If the customer cannot collect a must-gather
```
$ oc adm inspect ns/openshift-logging (also add the generated file)
$ oc -n openshift-logging get clusterlogging instance -o yaml > clo.txt
$ oc -n openshift-logging get clusterLogForwarder instance -o yaml > clf.txt
$ oc -n openshift-logging get csv > csv.txt
$ oc -n openshift-logging get LokiStack <lokistack_instance> -o yaml > lokistack.txt
```

#### 3.6) Extra information for checking Loki
```
===Metrics to identify log drops===
/// Vector discarded events
sum by(component_name) (irate(vector_buffer_discarded_events_total{component_kind="sink",component_type="loki"}[2m]))
/// Loki distributor lines received
sum by (tenant) (rate(loki_distributor_lines_received_total[5m]))
/// Loki gateway response rate
sum by (tenant, code) (rate(http_requests_total{namespace="openshift-logging",container="gateway",handler="push"}[5m]))
/// Loki discarded samples
sum by (tenant, reason) (irate(loki_discarded_samples_total[2m]))
sum by (tenant,reason)(sum_over_time(loki_discarded_samples_total{namespace="openshift-logging"}[1m]))
/// Loki ingester wal
loki_ingester_wal_records_logged_total {namespace="openshift-logging"}
loki_ingester_wal_disk_full_failures_total{namespace="openshift-logging"}
loki_ingester_wal_replay_duration_seconds{namespace="openshift-logging"}
loki_ingester_wal_logged_bytes_total{namespace="openshift-logging"}
loki_ingester_wal_discarded_bytes_total{namespace="openshift-logging"}
loki_ingester_wal_recovered_bytes_total{namespace="openshift-logging"}
```
- [Loki Dashboards](https://videos.learning.redhat.com/playlist/dedicated/251079123/1_ojvcvz0p/1_zq29kjud).


#### 3.7) Vector Troubleshooting
- [Vector Troubleshooting](https://access.redhat.com/articles/7089751) 


#### 3.8) Support cases

- [Troubleshooting cases Video](https://drive.google.com/file/d/1OPEeoI4Un4PBFexo6g_CEjl10eLo1ENv/view)

- Configuration issues [03959863](https://gss--c.vf.force.com/apex/Case_View?id=5006R00002144DH&sfdc.override=1), [03725529](https://gss--c.vf.force.com/apex/Case_View?srPos=66&srKp=500&srF=1&id=5006R00001ylVaI&sfdc.override=1), [03737922](https://gss--c.vf.force.com/apex/Case_View?srPos=11&srKp=500&srF=1&id=5006R00001ywIZr&sfdc.override=1)
  
- Storage issue [03925134](https://gss--c.vf.force.com/apex/Case_View?id=5006R000020p125&sfdc.override=1), [03866052](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R000020MyFz&sfdc.override=1).

- Read logs issue [03871025](https://gss--c.vf.force.com/apex/Case_View?srPos=47&srKp=500&srF=1&id=5006R000020NpjY&sfdc.override=1).

- Filter cases [03980750](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=500Hn00001kqp6A&sfdc.override=1), [03861975](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R000020MEMM&sfdc.override=1) 

- How to manage an unknown issue [03873885](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R000020OJ1U&sfdc.override=1).

- Interesting migration case [03809176](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R00001wgefc&sfdc.override=1#comment_a0a6R00000WYdyiQAD) 

