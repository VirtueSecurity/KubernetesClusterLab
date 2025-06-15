<!-- TOC -->
* [Vanilla K3s Kubernetes Cluster Install and Setup](#vanilla-k3s-kubernetes-cluster-install-and-setup)
  * [Overview](#overview)
  * [Single Node Local K3S Cluster](#single-node-local-k3s-cluster)
    * [Install](#install)
    * [Check Access](#check-access)
    * [Create Admin Service Account and Kube Config](#create-admin-service-account-and-kube-config)
    * [Create Read Only Service Account](#create-read-only-service-account)
    * [Choose a context](#choose-a-context)
    * [Start/Stop Daemon](#startstop-daemon)
    * [Uninstall](#uninstall)
    * [Install Helm Packages Into Cluster](#install-helm-packages-into-cluster)
      * [Configure Traefik - Already Installed](#configure-traefik---already-installed)
      * [Cert-Manager](#cert-manager)
      * [Prometheus](#prometheus)
      * [Get All Installed Helm Charts](#get-all-installed-helm-charts)
    * [Get to Cluster Services](#get-to-cluster-services)
      * [Traefik](#traefik)
      * [Team Workloads](#team-workloads)
      * [Prometheus](#prometheus-1)
      * [Grafana](#grafana)
<!-- TOC -->

# Vanilla K3s Kubernetes Cluster Install and Setup

This lab provides step-by-step instructions for setting up up a vanilla K3s installation with minimal customization but with all the essential components needed for a functional development environment. This guide assumes you've already reviewed the main project README and have the required prerequisites installed.

## Overview

This lab environment includes:

- **K3s**: A lightweight CNCF-certified Kubernetes distribution
- **Monitoring**: Prometheus and Grafana integration
- **Ingress**: Traefik ingress controller (pre-installed with K3s)
- **Certificates**: Automatic certificate generation with cert-manager
- **Example Workloads**: Basic applications to demonstrate functionality

## Single Node Local K3S Cluster

### Install

```bash
  sudo apt update && sudo apt upgrade
  curl -sfL https://get.k3s.io | sh -
  systemctl status k3s
● k3s.service - Lightweight Kubernetes
     Loaded: loaded (/etc/systemd/system/k3s.service; enabled; preset: enabled)
     Active: active (running) since Mon 2025-05-26 10:45:31 CDT; 17s ago
 Invocation: 03abb44649b940368904f660bf1adb28
       Docs: https://k3s.io
    Process: 1406445 ExecStartPre=/bin/sh -xc ! /usr/bin/systemctl is-enabled --quiet nm-cloud-setup.service 2>/dev/null (code=exited, status=0/SUCCESS)
    Process: 1406447 ExecStartPre=/sbin/modprobe br_netfilter (code=exited, status=0/SUCCESS)
    Process: 1406450 ExecStartPre=/sbin/modprobe overlay (code=exited, status=0/SUCCESS)
   Main PID: 1406452 (k3s-server)
      Tasks: 93
     Memory: 1.4G (peak: 1.4G)
        CPU: 18.704s
     CGroup: /system.slice/k3s.service
             ├─1406452 "/usr/local/bin/k3s server"
             ├─1406484 "containerd "
             ├─1407332 /var/lib/rancher/k3s/data/f10c229b8445ff9999c26495950a5c8b2e08fd13f1889d38f994edeebf026236/bin/containerd-shim-runc-v2 -namespace k8s.io -id 826d44e79593430e9fae5421a4a541667187ae20d000ee72f5d92624d545b2b4 -address /run/k3s/>
             ├─1407362 /var/lib/rancher/k3s/data/f10c229b8445ff9999c26495950a5c8b2e08fd13f1889d38f994edeebf026236/bin/containerd-shim-runc-v2 -namespace k8s.io -id 826fb4ddd11d31d3746b5c3a58fe1c8bbef47b28127d226e940442abcc30aaa6 -address /run/k3s/>
             ├─1407366 /var/lib/rancher/k3s/data/f10c229b8445ff9999c26495950a5c8b2e08fd13f1889d38f994edeebf026236/bin/containerd-shim-runc-v2 -namespace k8s.io -id 70bed0395e64a521547690375a394a5528d4a323838b0b0cd71ce55e35d1de6d -address /run/k3s/>
             ├─1408337 /var/lib/rancher/k3s/data/f10c229b8445ff9999c26495950a5c8b2e08fd13f1889d38f994edeebf026236/bin/containerd-shim-runc-v2 -namespace k8s.io -id 2a3a7855caa4d1dec336eb03ec678121500be150f0bd2d04cebca7e53bd93511 -address /run/k3s/>
             └─1408431 /var/lib/rancher/k3s/data/f10c229b8445ff9999c26495950a5c8b2e08fd13f1889d38f994edeebf026236/bin/containerd-shim-runc-v2 -namespace k8s.io -id 321f2a2f135770c35ac4efff7c56659977e37ca3b7ee4bc2b35f6d2447e65039 -address /run/k3s/>

May 26 10:45:45 XPS-13-9350 k3s[1406452]: I0526 10:45:45.603629 1406452 reconciler_common.go:299] "Volume detached for volume \"values\" (UniqueName: \"kubernetes.io/projected/f8766a24-eca6-45ef-9db2-87b54c4615a9-values\") on node \"ncob>
May 26 10:45:45 XPS-13-9350 k3s[1406452]: I0526 10:45:45.603635 1406452 reconciler_common.go:299] "Volume detached for volume \"tmp\" (UniqueName: \"kubernetes.io/empty-dir/f8766a24-eca6-45ef-9db2-87b54c4615a9-tmp\") on node \">
May 26 10:45:46 XPS-13-9350 k3s[1406452]: I0526 10:45:46.292345 1406452 pod_container_deletor.go:80] "Container not found in pod's containers" containerID="b3ddf8522479c88d20be31dc7cdb0e4fe3424ce19ab0f2ae9617a66b796846b7"
May 26 10:45:46 XPS-13-9350 k3s[1406452]: I0526 10:45:46.317378 1406452 job_controller.go:598] "enqueueing job" key="kube-system/helm-install-traefik" delay="1s"
May 26 10:45:46 XPS-13-9350 k3s[1406452]: I0526 10:45:46.322329 1406452 job_controller.go:598] "enqueueing job" key="kube-system/helm-install-traefik" delay="1s"
May 26 10:45:46 XPS-13-9350 k3s[1406452]: I0526 10:45:46.325362 1406452 event.go:389] "Event occurred" object="kube-system/traefik" fieldPath="" kind="HelmChart" apiVersion="helm.cattle.io/v1" type="Normal" reason="ApplyJob" message="App>
May 26 10:45:46 XPS-13-9350 k3s[1406452]: I0526 10:45:46.337630 1406452 event.go:389] "Event occurred" object="kube-system/traefik" fieldPath="" kind="HelmChart" apiVersion="helm.cattle.io/v1" type="Normal" reason="ApplyJob" message="App>
May 26 10:45:47 XPS-13-9350 k3s[1406452]: I0526 10:45:47.302260 1406452 replica_set.go:679] "Finished syncing" kind="ReplicaSet" key="kube-system/traefik-c98fdf6fb" duration="56.494µs"
May 26 10:45:47 XPS-13-9350 k3s[1406452]: I0526 10:45:47.302376 1406452 pod_startup_latency_tracker.go:104] "Observed pod startup duration" pod="kube-system/traefik-c98fdf6fb-5btk6" podStartSLOduration=1.531844016 podStartE2EDuration="4.>
May 26 10:45:47 XPS-13-9350 k3s[1406452]: I0526 10:45:47.313031 1406452 event.go:389] "Event occurred" object="kube-system/traefik" fieldPath="" kind="Service" apiVersion="v1" type="Normal" reason="UpdatedLoadBalancer" message="Updated L>
lines 1-31/31 (END)
```

### Check Access
```bash
  sudo kubectl --kubeconfig /etc/rancher/k3s/k3s.yaml get nodes
AME                    STATUS   ROLES                  AGE     VERSION
xps-13-9350   Ready    control-plane,master   6m37s   v1.32.5+k3s1
```

### Create Admin Service Account and Kube Config
```bash
  sudo kubectl --kubeconfig /etc/rancher/k3s/k3s.yaml apply -f ../MultiTenantK3sWithKyverno/install/new-service-account.yaml
  TOKEN=$(sudo kubectl --kubeconfig /etc/rancher/k3s/k3s.yaml create token my-admin-sa -n platform-team --duration=8760h)
  K3S_SERVER_IP=$(sudo kubectl --kubeconfig /etc/rancher/k3s/k3s.yaml config view --minify -o jsonpath='{.clusters[0].cluster.server}')
  K3S_CA_DATA=$(sudo kubectl --kubeconfig /etc/rancher/k3s/k3s.yaml config view --flatten -o jsonpath='{.clusters[0].cluster.certificate-authority-data}')
  echo "${K3S_CA_DATA}" | base64 -d > ~/.kube/k3s-ca.crt
  kubectl config set-cluster k3s-cluster   --server="${K3S_SERVER_IP}"   --certificate-authority="${HOME}/.kube/k3s-ca.crt"   --embed-certs=true
  kubectl config set-credentials my-admin-sa-user   --token="${TOKEN}"
  kubectl config set-context my-admin-sa-context   --cluster=k3s-cluster   --user=my-admin-sa-user   --namespace=default
  kubectl config use-context my-admin-sa-context
  kubectl get nodes
NAME                    STATUS   ROLES                  AGE   VERSION
xps-13-9350   Ready    control-plane,master   27m   v1.32.5+k3s1
```

### Create Read Only Service Account

```bash
  sudo kubectl --kubeconfig /etc/rancher/k3s/k3s.yaml apply -f ../MultiTenantK3sWithKyverno/install/read-only-sa.yaml
  TOKEN=$(sudo kubectl --kubeconfig /etc/rancher/k3s/k3s.yaml create token -n platform-team my-readonly-sa --duration=8760h)
  kubectl config set-credentials read-only-sa-user   --token="${TOKEN}"
  kubectl config set-context my-readonly-sa-context   --cluster=k3s-cluster   --user=read-only-sa-user   --namespace=default
```

### Choose a context

```bash
  kubectl config get-contexts
  kubectl config use-context my-readonly-sa-context # choose read only user or
  kubectl config use-context my-admin-sa-context # choose admin user
```

### Start/Stop Daemon

Prevent It From Starting by Default on Boot
```bash
  systemctl disable k3s
```

Status
```bash
  systemctl status k3s
```

Start
```bash
  systemctl start k3s
```

Stop

```bash
  systemctl stop k3s
```

### Uninstall

```bash
  /usr/local/bin/k3s-uninstall.sh
```


### Install Helm Packages Into Cluster

#### Configure Traefik - Already Installed

```bash
  sudo cp ../MultiTenantK3sWithKyverno/install/traefik-custom-config.yaml /var/lib/rancher/k3s/server/manifests/
```

#### Cert-Manager
```bash
  helm repo add jetstack https://charts.jetstack.io
  helm repo update
  helm search repo cert-manager #check the latest version and use it below
  helm show values jetstack/cert-manager
  helm install cert-manager jetstack/cert-manager --create-namespace --namespace cert-manager  -f ../MultiTenantK3sWithKyverno/install/cert-manager-values.yaml # check latest version 
  helm get values cert-manager -n cert-manager
  kubectl get pods -n cert-manager
  kubectl get secrets cert-manager-webhook-ca -n cert-manager -o jsonpath='{.data.tls\.crt}' | base64 --decode > install/internal-ca.crt
```

#### Prometheus

```bash
  helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
  helm repo update
  helm search repo prometheus
  helm install prometheus prometheus-community/kube-prometheus-stack --create-namespace --namespace monitoring -f ../MultiTenantK3sWithKyverno/install/prometheus-values.yaml 
  helm get values prometheus -n monitoring
Get Grafana 'admin' user password by running:

  kubectl --namespace monitoring get secrets prometheus-grafana -o jsonpath="{.data.admin-password}" | base64 -d ; echo

Access Grafana local instance:

  export POD_NAME=$(kubectl --namespace monitoring get pod -l "app.kubernetes.io/name=grafana,app.kubernetes.io/instance=prometheus" -oname)
  kubectl --namespace monitoring port-forward $POD_NAME 3000

  kubectl --namespace monitoring get pods -l "release=prometheus"
  kubectl --namespace monitoring get secrets prometheus-grafana -o jsonpath="{.data.admin-password}" | base64 -d ; echo

https://github.com/prometheus-community/helm-charts/tree/main/charts/prometheus#scraping-pod-metrics-via-annotations
```
#### Get All Installed Helm Charts

```bash
  helm list -A
```


### Get to Cluster Services

#### Traefik

https://k3s.rocks/traefik-dashboard/#expose-traefik-dashboard

Metrics:
- `kubectl --insecure-skip-tls-verify -n kube-system port-forward deployments/traefik 8080:9100`
- Visit http://127.0.0.1:8080/metrics/

![](../MultiTenantK3sWithKyverno/2025-05-28-13-35-21.png)

Dashboard:
- `kubectl -n kube-system port-forward deployments/traefik 8080:8080`
- Visit http://127.0.0.1:8080/dashboard/

![](../MultiTenantK3sWithKyverno/2025-05-28-09-20-25.png)

- http://localhost:8080/ping/

#### Team Workloads

```bash
  EXTIP=$(kubectl get -n kube-system svc traefik -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
  echo "$EXTIP team-a.k3s-cluster.local metrics.team-b.k3s-cluster.local team-b.k3s-cluster.local team-c.k3s-cluster.local"| sudo tee -a /etc/hosts
```
Visit:
- https://team-a.k3s-cluster.local/
- https://metrics.team-b.k3s-cluster.local
- https://team-b.k3s-cluster.local
- https://team-c.k3s-cluster.local/static

![](../MultiTenantK3sWithKyverno/2025-05-28-13-54-11.png)
![](../MultiTenantK3sWithKyverno/2025-05-28-13-53-53.png)
![](../MultiTenantK3sWithKyverno/2025-05-28-13-53-26.png)
![](../MultiTenantK3sWithKyverno/2025-05-28-13-53-10.png)

#### Prometheus

```bash
  kubectl -n monitoring port-forward service/prometheus-operated 9090:9090
```

Visit:
- http://localhost:9090/query

![](../MultiTenantK3sWithKyverno/2025-05-28-14-01-49.png)

#### Grafana

```bash
  kubectl -n monitoring port-forward service/prometheus-grafana 3000:80
```

Visit:
- http://localhost:3000/login
  - Username: admin
  - Password: your-strong-password (modify this in ../MultiTenantK3sWithKyverno/install/prometheus-values.yaml)

![](../MultiTenantK3sWithKyverno/2025-05-28-13-58-40.png)

![](../MultiTenantK3sWithKyverno/2025-05-28-13-59-19.png)
