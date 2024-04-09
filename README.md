# ensf400-lab8-kubernetes-2

## Objectives
This lab will teach us the scheduling, configmaps, canary and blue-green deployment strategies of Kubernetes. Through Minikube, a simplified Kubernetes engine running on a single computer, we practice the key concepts and usage of Kubernetes.

## Environment

### Set Up Your GitHub CodeSpaces Instance

Same as Lab 6, this lab will be performed in [GitHub CodeSpaces](https://github.com/codespaces). Create an instance using GitHub Codespaces. Choose repository `denoslab/ensf400-lab8-kubernetes-2`.


```bash
$ minikube start --nodes 3 -p ensf400
```

This step will start the Minikube service with 3 virtual nodes, stored in a profile called `ensf400`. If for some reason your Codespaces instance was stopped (e.g., not using it for a while). You can restart the minikube service using this profile by running:

```bash
$ minikube start -p ensf400
```

## Steps

Go to Section 7 - 10 and complete the steps for each section. The steps can be found in the `README.md` files in each subdirectory.

## Have Your Work Checked By a TA

The TA will check the completion of the following tasks:

- Output of Section 7.
- Output of Section 8.
- Output of Section 9.
- Output of Section 10.


Each member of the group should be able to answer all of the following questions. The TA will ask each person one question selected at random, and the student must be able to answer the question to get credit for the lab.

- Q1: Explain the scheduling strategy of Node Affinity and the scenarios to use it.
- Q2: Explain the scheduling strategy of Pod Anti-Affinity and the scenarios to use it.
- Q3: Explain the deployment strategy of blue-green deployment. How to switch between the two versions of deployments?
- Q4: Explain the deployment strategy of canary deployment. How to adjust the ratio of users getting serviced by the canary deployment?
---
# ENSF 400 - Assignment 3 - Kubernetes

This assignment has a full mark of 100. It takes up 5\% of your final grade. 

You will use Minikube in Codespaces to deploy an nginx service and 2 backend apps.

## Requirements

Based on your work for [Lab 7](https://github.com/denoslab/ensf400-lab7-kubernetes-1) and [Lab 8](https://github.com/denoslab/ensf400-lab8-kubernetes-2), deploy an `nginx` service so that:

1. A `Deployment` config defined in `nginx-dep.yaml`. The Deployment has the name `nginx-dep` with 5 replicas. The Deployment uses a base image `nginx` with the version tag `1.14.2`. Expose port `80`.
1. A `ConfigMap` defined in `nginx-configmap.yaml`, The `data` in the configmap has a key-value pair with the key being `default.conf` and value being the following:
```
upstream backend {
    server app-1:8080;
    server app-2:8080;
}

server {
    location / {
        proxy_pass http://backend;
    }
}
```
1. In the Deployment `nginx-dep`, mount the configuration file `default.conf` to the correct path of `/etc/nginx/conf.d` so that it serves as a load balancer, similar to what we have for [Assignment 2](https://github.com/denoslab/ensf400-lab5-ansible/tree/main/assignment2).
1. A `Service` config of type `ClusterIP` defined in `nginx-svc.yaml`. The service has the name `nginx-svc`, exposes port `80`, and should use label selectors to select the pods from the `Deployment` defined in the last step.
1. An `Ingress` config named `nginx-ingress.yaml` redirecting the requests to path `/` to the backend service `nginx-svc`. Example request and response:
```bash
$ curl http://$(minikube ip)/
Hello World from [app-1-dep-86f67f4f87-2d28z]!
$ curl http://$(minikube ip)/
Hello World from [app-2-dep-7f686c4d8d-lr95c]!
```
1. Write `Deployment` and `Service` for `app-1` and `app-2`, respectively.
1. Define two other `Ingress` configs named `app-1-ingress.yaml` and `app-2-ingress.yaml`, both redicting requests to `/` to the backend apps, taking `app-1` as the main deployment, and `app-2` as a canary deployment. The ingresses will redirect 70% of the traffic to `app-1` and 30% of the traffic to `app-2`. The docker images are pre-built for you. They can be downloaded using the URL below:
```
app-1: ghcr.io/denoslab/ensf400-sample-app:v1
app-2: ghcr.io/denoslab/ensf400-sample-app:v2
```

## Deliverables

Submit the files below in a zip file. There is no need for TAs to access your Codespaces. TAs will mark your assignment based on the files you submitted.

1. (10%) `nginx-dep.yaml`
1. (10%) `nginx-configmap.yaml`
1. (10%) `nginx-svc.yaml`
1. (20%) `nginx-ingress.yaml`. Include steps showing the requests using `curl` and responses from load-balanced app backends (`app-1` and `app-2`).
1. (15%) `app-1-dep.yaml`, `app-1-svc.yaml`, `app-2-dep.yaml`, `app-2-svc.yaml`.
1. (20%) `app-1-ingress.yaml` and `app-2-ingress.yaml`.
1. (15%) A `README.md` Markdown file describing the steps and outputs meeting the requirements.
