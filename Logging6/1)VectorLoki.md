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

[Installation and Deployment VIDEO](https://drive.google.com/file/d/1hHk1padnKc9JIgi93_ANN1lBL2pIzJZj/view?usp=drive_link)

[Configuring log forwarding](https://docs.redhat.com/en/documentation/openshift_container_platform/4.17/html/logging/logging-6-0#log6x-clf)

- [Observability Forwarder third-party examples for Logging 6](https://gitlab.cee.redhat.com/aosqe/aosqe-tools/-/tree/master/logging/log_doc/clf-6.0-examples?ref_type=heads)


#### 2.1) Useful configuration information

- [Tuning log payloads and delivery in RHOCP 4](https://access.redhat.com/solutions/7074148)

- [Performance and reliability tuning](https://docs.openshift.com/container-platform/4.14/observability/logging/performance_reliability/logging-flow-control-mechanisms.html)

- [JSON logs with Loki](https://access.redhat.com/solutions/7048604).
  
- [Why Loki needs block and object storage in RHOCP 4?](https://access.redhat.com/solutions/7062821)

- [LokistackSchemaUpgradesRequired warning alert firing in RHOCP 4](https://access.redhat.com/solutions/7063482)
  
- [Request validation Loki rate limits](https://grafana.com/docs/loki/latest/operations/request-validation-rate-limits/)
  
- [Defining retention for logs in LokiStack in RHOCP4](https://access.redhat.com/solutions/7053212)
  


### 3)Troubleshooting

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
- LokiStack instance
- Observability forwarder instance Managed Status
- Observability forwarder instance Status/Conditions
- Collector Logs
- Loki pods logs
- UI Plugin status

#### 3.4) If the customer cannot collect a must-gather
```
$ oc adm inspect ns/openshift-logging (also add the generated file)
$ oc -n openshift-logging get csv > csv.txt
$ oc -n openshift-logging get observability <instance> -o yaml > observclf.txt
$ oc -n openshift-logging get LokiStack <lokistack_instance> -o yaml > lokistack.txt
$ oc -n openshift-logging get uiplugin <plugin_name> -o yaml > plugin.txt
```

#### 3.5) Extra information for checking Loki
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


#### 3.6) Vector Troubleshooting
- [Vector Troubleshooting](https://access.redhat.com/articles/7089751) 


#### 3.7) Support cases

- [Troubleshooting cases Video](https://drive.google.com/file/d/1OPEeoI4Un4PBFexo6g_CEjl10eLo1ENv/view) . Please note that the cases out of the "Logging 6" section belong to Logging 5 using Vector and Loki but since the steps to troubleshoot these tickets are the same for both versions of logging, I'm leaving them attached along with the troubleshooting video.
- Configuration issues [03959863](https://gss--c.vf.force.com/apex/Case_View?id=5006R00002144DH&sfdc.override=1), [03725529](https://gss--c.vf.force.com/apex/Case_View?srPos=66&srKp=500&srF=1&id=5006R00001ylVaI&sfdc.override=1), [03737922](https://gss--c.vf.force.com/apex/Case_View?srPos=11&srKp=500&srF=1&id=5006R00001ywIZr&sfdc.override=1) 
- Storage issue [03925134](https://gss--c.vf.force.com/apex/Case_View?id=5006R000020p125&sfdc.override=1), [03866052](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R000020MyFz&sfdc.override=1).
- Read logs issue [03871025](https://gss--c.vf.force.com/apex/Case_View?srPos=47&srKp=500&srF=1&id=5006R000020NpjY&sfdc.override=1).
- How to manage an unknown issue [03873885](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R000020OJ1U&sfdc.override=1).

Logging 6
- Filter queries [04142719](https://gss--c.vf.force.com/apex/Case_View?id=500Hn00001pDUHc&sfdc.override=1)
- JSON queries [04125308](https://gss--c.vf.force.com/apex/Case_View?id=500Hn00001o35gD&sfdc.override=1)
- Log Level clarifications [04167078](https://gss--c.vf.force.com/apex/Case_View?id=500Hn00001pF3Dt&sfdc.override=1), [04060876](https://gss--c.vf.force.com/apex/Case_View?id=500Hn00001mcIND&sfdc.override=1)
- Logging questions [04066770](https://gss--c.vf.force.com/apex/Case_View?id=500Hn00001mcZ4s&sfdc.override=1), [04058669](https://gss--c.vf.force.com/apex/Case_View?id=500Hn00001mcCY8&sfdc.override=1), [04010553](https://gss--c.vf.force.com/apex/Case_View?id=500Hn00001mZID0&sfdc.override=1)
  
 

