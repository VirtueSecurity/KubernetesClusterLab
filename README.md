# KubernetesClusterLab

The purpose of this project is to provide instructions for installing, configuring, setting up, and using a variety of Kubernetes Clusters lab environments. This project includes a folder and Markdown document for each lab environment. Each folder may include kubernetes specs, helm values specs, bash scripts, or other related resources to help in the installation, setup, configuration, and usage of the cluster.

<!-- TOC -->
* [KubernetesClusterLab](#kubernetesclusterlab)
  * [🚀 What is KubernetesClusterLab?](#-what-is-kubernetesclusterlab)
  * [🎯 Project Goals](#-project-goals)
  * [👥 Who is this for?](#-who-is-this-for)
  * [Credits](#credits)
    * [About Virtue Security](#about-virtue-security)
  * [🧰 Prerequisites](#-prerequisites)
    * [System Requirements](#system-requirements)
    * [Tools and Setup](#tools-and-setup)
  * [🔍 Available Labs](#-available-labs)
    * [Vanilla K3s](#vanilla-k3s)
    * [Multi-Tenant K3s with Kyverno](#multi-tenant-k3s-with-kyverno)
<!-- TOC -->

## 🚀 What is KubernetesClusterLab?

KubernetesClusterLab is a collection of hands-on labs, configuration guides, and resources designed to help you create environments to test your Kubernetes skills, practice manual pentesting, or run various automated Kubernetes security tools using different Kubernetes cluster implementations.

## 🎯 Project Goals

- Provide clear, detailed instructions for various Kubernetes cluster setups
- Create ready-to-use environments with example workloads for security testing and exploration
- These environments are NOT intended to be secure. In fact, they have intentional security vulnerabilities and should not be used to run production workloads

## 👥 Who is this for?

- **Beginners**: Start with basic cluster setups and gradually explore more complex configurations
- **Security Professionals**: Explore secure multi-tenant setups and policy enforcement

## Credits

KubePentest was created by [Nick Coblentz](https://github.com/nickcoblentz) and is proudly released by [Virtue Security](https://www.virtuesecurity.com/).

### About Virtue Security

Virtue Security is a specialized cybersecurity firm offering in-depth security testing services including:
- Application Penetration Testing
- Cloud Penetration Testing
- Kubernetes Penetration Testing

Visit [Virtue Security](https://www.virtuesecurity.com/) to learn more about their security services.

## 🧰 Prerequisites

### System Requirements

To use the labs in this project, you'll need:

- A Linux environment (tested on Debian/Ubuntu)
- sudo access for installing system packages
- Internet connection for downloading packages and container images

### Tools and Setup

Install the following tools to help install, setup, and interact with the cluster:

- **kubectl**: `sudo snap install --classic kubectl`
- **helm**: `sudo snap install --classic helm`
- **jq/yq**: `sudo apt install jq && sudo snap install yq`
- **kubectl plugins** (via Krew):
  ```bash
  # Install Krew
  (
    set -x; cd "$(mktemp -d)" &&
    OS="$(uname | tr '[:upper:]' '[:lower:]')" &&
    ARCH="$(uname -m | sed -e 's/x86_64/amd64/' -e 's/\(arm\)\(64\)\?.*/\1\2/' -e 's/aarch64$/arm64/')" &&
    KREW="krew-${OS}_${ARCH}" &&
    curl -fsSLO "https://github.com/kubernetes-sigs/krew/releases/latest/download/${KREW}.tar.gz" &&
    tar zxvf "${KREW}.tar.gz" &&
    ./"${KREW}" install krew
  )

  # Add to PATH
  echo 'export PATH="${KREW_ROOT:-$HOME/.krew}/bin:$PATH"' >> ~/.bashrc
  source ~/.bashrc
  kubectl krew update
  
  # Install Useful Plugins
  kubectl krew list
  kubectl krew install score
  kubectl krew install rbac-tool
  kubectl krew install who-can
  kubectl krew install kyverno
  kubectl krew install kubescape
  ```

Each lab README contains specific instructions for additional tools required for that particular environment.

## 🔍 Available Labs

The project currently includes the following lab environments:

### [Vanilla K3s](Labs/VanillaK3s/README.md)
A basic K3s Kubernetes cluster setup for general exploration and testing.

[→ Go to Lab Instructions](Labs/VanillaK3s/README.md)

### [Multi-Tenant K3s with Kyverno](Labs/MultiTenantK3sWithKyverno/README.md)

A comprehensive multi-tenant Kubernetes environment built on K3s with advanced security and monitoring features:

**Key Features:**
- Single-node K3s cluster setup
- Multi-tenant configuration with namespace isolation through Capsule
- Kyverno policy enforcement with medium risk pod security standard
- Integrated monitoring with Prometheus and Grafana
- Traefik ingress controller
- Certificate management with cert-manager
- Three example tenant workloads

[→ Go to Lab Instructions](Labs/MultiTenantK3sWithKyverno/README.md)


