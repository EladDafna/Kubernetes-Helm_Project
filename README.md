## Elad Kubernetes Project With Helm Charts

This project deploys an Apache HTTP Server (`httpd`) on Kubernetes using Helm charts. Below are the details of each component used in this deployment:

## Deployment (`deploy.yaml`)

The Deployment manages the creation and scaling of Pods that run the Apache HTTP Server. It is configured with the following specifications:
- **Replicas:** Configured to run 3 replicas.
- **Image:** Uses the `httpd` image for the Apache HTTP Server.
- **Resources:**
  - Requests:
    - CPU: 10%
    - Memory: 100Mi
  - Limits:
    - CPU: 25%
    - Memory: 100Mi
- **Volume Mount:** Mounts a Persistent Volume (`std-apache-volume`) at `/usr/local/apache2/htdocs/`.

## Persistent Volume (`pv.yaml`)

The Persistent Volume (PV) defines the storage settings for the Apache HTTP Server:
- **Storage:** Configured with a storage size of {{ .Values.persistentVolume.storage }}.
- **Access Modes:** Supports `ReadWriteOnce`.
- **Reclaim Policy:** Set to `Retain`.
- **NFS Details:**
  - **Server:** {{ .Values.nfs.server }}
  - **Path:** {{ .Values.nfs.path }}

## Persistent Volume Claim (`pvc.yaml`)

The Persistent Volume Claim (PVC) claims storage from the Persistent Volume (PV) defined earlier:
- **Storage:** Requests {{ .Values.persistentVolumeClaim.storage }} of storage capacity.
- **Access Mode:** Configured as `ReadWriteOnce`.

## Storage Class (`storageclass.yaml`)

Defines the Storage Class `std-class` which waits for the first consumer before binding a Persistent Volume.

## Helm Chart (`Chart.yaml`)

Provides metadata about the Helm chart used for this project:
- **Name:** Elad-Project
- **Description:** Helm chart for deploying Apache HTTP Server on Kubernetes.
- **Version:** 0.1.0
- **App Version:** 1.0.0
- **Keywords:** apache
- **Maintainer:**
  - **Name:** Elad Dafna
  - **Email:** eladdaf@gmail.com

## Service (`svc.yaml`)

Exposes the Apache HTTP Server Deployment internally within the Kubernetes cluster:
- **Port:** Exposes port 80 internally.
- **Selector:** Routes traffic to Pods labeled with `app: {{ .Release.Name }}--eladtest`.
