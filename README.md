# ACM Dashboard for Tekton pipelines

![20220609154843](https://i.imgur.com/RxpOF3u.png)

## Requirements
* [OpenShift](https://www.openshift.com/)
* [Red Hat Advanced Cluster Management for Kubernetes](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.4)
*  [Observability service](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.4/html/observability/observing-environments-intro#enable-observability)
* Install `Red Hat OpenShift Pipelines` Operator

## Getting started 
**Once ACM is installed run the following commands to install the Observability service**  
* [Install ACM observability Service](install-acm-observability-service.md)

**Install the Grafana dev environment on cluster**
* [How to design a grafana dashboard](https://github.com/open-cluster-management/multicluster-observability-operator/tree/main/tools)


**Once the e Grafana dev enviornment is installed you can run the below command to access the UI** 
```
echo https://multicloud-console.$(oc get ingresses.config.openshift.io cluster -o jsonpath='{ .spec.domain }')/grafana-dev/
```

**Create observability-metrics-custom-allowlist.yaml**
```
cat >observability-metrics-custom-allowlist.yaml<<YAML
kind: ConfigMap
apiVersion: v1
metadata:
  name: observability-metrics-custom-allowlist
data:
  metrics_list.yaml: |
    names:
      - tekton_pipelines_controller_reconcile_count
      - tekton_pipelines_controller_pipelinerun_count
      - tekton_pipelines_controller_running_pipelineruns_count
      - kube_node_status_condition
      - container_processes
YAML
```

**Apply config map against RHACM**
```
oc apply -n open-cluster-management-observability -f observability-metrics-custom-allowlist.yaml
```

## Dashboard List 
> All the command beow need to be ran against the RHACM cluster in order to be shown on the grafana dashboard. 

### Load tekton dashboard without logging dashboard
```
oc create -f config-files/tekton-dashboard-without-logging.yaml
```

### Load tekton dashboard with logging dashboard - WIP
> still debugging dashboard
```
oc create -f config-files/tekton-dashboard-with-logging.yaml
```