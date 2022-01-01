# ELK-DO-k8s
[![MIT License](https://img.shields.io/apm/l/atomic-design-ui.svg?)](https://github.com/tterb/atomic-design-ui/blob/master/LICENSEs)

A basic deployment of a log monitoring system (Elasticsearch/Fluent-bit/Kibana) to a Digital Ocean Kubernetes cluster using Elastic Cloud on Kubernetes (ECK). [This Quickstart](https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-quickstart.html) implementation was followed for this deployment.

## Create k8s cluster on Digital Ocean

Sign up for a Digital Ocean account and create a Kubernetes cluster. 

Select a Kubernetes version, a (closest) datacenter region, and a cluster capacity. In my case, I chose the default 3 nodes, each with 2.5GB RAM usable & 2 vCPUs. *Note that if your cluster does not have any nodes with at least 2GiB of free memory, some pods can become stuck in `Pending` state.* 

![cluster_info.png](images/cluster_info.png)

Using this configuration generates an estimated $60/month. In my case, I had to request a Droplet limit increase from 1 to 5 in order to get my project working.

The cluster creation will take about 5 minutes to complete-- in the meantime, you can authorize doctl with your personal access token, and configure your connection to the k8s cluster.

## Deploy ECK

Elastic Cloud on Kubernetes (ECK) is the official operator by Elastic and extends basic k8s orchestration capabilities to easily deploy your Elasticsearch cluster.

First, install CRDs and the operator:

```
kubectl create -f https://download.elastic.co/downloads/eck/1.9.1/crds.yaml
kubectl apply -f https://download.elastic.co/downloads/eck/1.9.1/operator.yaml
```

You can monitor the operator logs as such:

![elastic_eck.png](images/elastic_eck.png)

## Deploy an Elasticsearch cluster

You can apply a simple Elasticsearch cluster specification:

```
kubectl create -f elasticsearch.yaml
```
The operator automatically creates and manages the k8s resources. After checking the cluster health and creation progress, you can request Elasticsearch access after getting the credentials as outline [here](https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-deploy-elasticsearch.html).

![elasticsearch.png](images/elasticsearch.png)