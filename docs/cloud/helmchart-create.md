---
layout: default
title: Helm Chart 생성
parent: Helm Chart
grand_parent: Cloud
permalink: /docs/cloud/helmchart-create/
nav_order: 12
---

# Helm Chart 생성
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---



## Helm Chart 생성하기

```sh
user1@user1-500R5K-501R5K-500R5Q:~/helm-test$ helm create mychart
Creating mychart
user1@user1-500R5K-501R5K-500R5Q:~/helm-test$ tree
.
└── mychart
    ├── charts
    ├── Chart.yaml
    ├── templates
    │   ├── deployment.yaml
    │   ├── _helpers.tpl
    │   ├── hpa.yaml
    │   ├── ingress.yaml
    │   ├── NOTES.txt
    │   ├── serviceaccount.yaml
    │   ├── service.yaml
    │   └── tests
    │       └── test-connection.yaml
    └── values.yaml

4 directories, 10 files

```

## Helm Chart 파일

### Chart.yaml 

```yml
user1@user1-500R5K-501R5K-500R5Q:~/helm-test/mychart$ cat Chart.yaml 
apiVersion: v2
name: mychart
description: A Helm chart for Kubernetes

# A chart can be either an 'application' or a 'library' chart.
#
# Application charts are a collection of templates that can be packaged into versioned archives
# to be deployed.
#
# Library charts provide useful utilities or functions for the chart developer. They're included as
# a dependency of application charts to inject those utilities and functions into the rendering
# pipeline. Library charts do not define any templates and therefore cannot be deployed.
type: application

# This is the chart version. This version number should be incremented each time you make changes
# to the chart and its templates, including the app version.
# Versions are expected to follow Semantic Versioning (https://semver.org/)
version: 0.1.0

# This is the version number of the application being deployed. This version number should be
# incremented each time you make changes to the application. Versions are not expected to
# follow Semantic Versioning. They should reflect the version the application is using.
# It is recommended to use it with quotes.
appVersion: "1.16.0"
```

### values.yml

```yml
user1@user1-500R5K-501R5K-500R5Q:~/helm-test/mychart$ cat values.yaml 
# Default values for mychart.
# This is a YAML-formatted file.
# Declare variables to be passed into your templates.

replicaCount: 1

image:
  repository: nginx
  pullPolicy: IfNotPresent
  # Overrides the image tag whose default is the chart appVersion.
  tag: ""

imagePullSecrets: []
nameOverride: ""
fullnameOverride: ""

serviceAccount:
  # Specifies whether a service account should be created
  create: true
  # Automatically mount a ServiceAccount's API credentials?
  automount: true
  # Annotations to add to the service account
  annotations: {}
  # The name of the service account to use.
  # If not set and create is true, a name is generated using the fullname template
  name: ""

podAnnotations: {}
podLabels: {}

podSecurityContext: {}
  # fsGroup: 2000

securityContext: {}
  # capabilities:
  #   drop:
  #   - ALL
  # readOnlyRootFilesystem: true
  # runAsNonRoot: true
  # runAsUser: 1000

service:
  type: ClusterIP
  port: 80

ingress:
  enabled: false
  className: ""
  annotations: {}
    # kubernetes.io/ingress.class: nginx
    # kubernetes.io/tls-acme: "true"
  hosts:
    - host: chart-example.local
      paths:
        - path: /
          pathType: ImplementationSpecific
  tls: []
  #  - secretName: chart-example-tls
  #    hosts:
  #      - chart-example.local

resources: {}
  # We usually recommend not to specify default resources and to leave this as a conscious
  # choice for the user. This also increases chances charts run on environments with little
  # resources, such as Minikube. If you do want to specify resources, uncomment the following
  # lines, adjust them as necessary, and remove the curly braces after 'resources:'.
  # limits:
  #   cpu: 100m
  #   memory: 128Mi
  # requests:
  #   cpu: 100m
  #   memory: 128Mi

autoscaling:
  enabled: false
  minReplicas: 1
  maxReplicas: 100
  targetCPUUtilizationPercentage: 80
  # targetMemoryUtilizationPercentage: 80

# Additional volumes on the output Deployment definition.
volumes: []
# - name: foo
#   secret:
#     secretName: mysecret
#     optional: false

# Additional volumeMounts on the output Deployment definition.
volumeMounts: []
# - name: foo
#   mountPath: "/etc/foo"
#   readOnly: true

nodeSelector: {}

tolerations: []

affinity: {}
```

### templates/_helpers.tpl 

```yml
user1@user1-500R5K-501R5K-500R5Q:~/helm-test/mychart$ cat templates/_helpers.tpl 
{ {/*
Expand the name of the chart.
*/} }
{ {- define "mychart.name" -} }
{ {- default .Chart.Name .Values.nameOverride | trunc 63 | trimSuffix "-" } }
{ {- end } }

{ {/*
Create a default fully qualified app name.
We truncate at 63 chars because some Kubernetes name fields are limited to this (by the DNS naming spec).
If release name contains chart name it will be used as a full name.
*/} }
{ {- define "mychart.fullname" -} }
{ {- if .Values.fullnameOverride } }
{ {- .Values.fullnameOverride | trunc 63 | trimSuffix "-" } }
{ {- else } }
{ {- $name := default .Chart.Name .Values.nameOverride } }
{ {- if contains $name .Release.Name } }
{ {- .Release.Name | trunc 63 | trimSuffix "-" } }
{ {- else } }
{ {- printf "%s-%s" .Release.Name $name | trunc 63 | trimSuffix "-" } }
{ {- end } }
{ {- end } }
{ {- end } }

{ {/*
Create chart name and version as used by the chart label.
*/} }
{ {- define "mychart.chart" -} }
{ {- printf "%s-%s" .Chart.Name .Chart.Version | replace "+" "_" | trunc 63 | trimSuffix "-" } }
{ {- end } }

{ {/*
Common labels
*/} }
{ {- define "mychart.labels" -} }
helm.sh/chart: { { include "mychart.chart" . } }
{ { include "mychart.selectorLabels" . } }
{ {- if .Chart.AppVersion } }
app.kubernetes.io/version: { { .Chart.AppVersion | quote } }
{ {- end } }
app.kubernetes.io/managed-by: { { .Release.Service } }
{ {- end } }

{ {/*
Selector labels
*/} }
{ {- define "mychart.selectorLabels" -} }
app.kubernetes.io/name: { { include "mychart.name" . } }
app.kubernetes.io/instance: { { .Release.Name } }
{ {- end } }

{ {/*
Create the name of the service account to use
*/} }
{ {- define "mychart.serviceAccountName" -} }
{ {- if .Values.serviceAccount.create } }
{ {- default (include "mychart.fullname" .) .Values.serviceAccount.name } }
{ {- else } }
{ {- default "default" .Values.serviceAccount.name } }
{ {- end } }
{ {- end } }
```

### templates/serviceaccount.yaml

```yml
user1@user1-500R5K-501R5K-500R5Q:~/helm-test/mychart$ cat templates/serviceaccount.yaml 
{ {- if .Values.serviceAccount.create -} }
apiVersion: v1
kind: ServiceAccount
metadata:
  name: { { include "mychart.serviceAccountName" . } }
  labels:
    { {- include "mychart.labels" . | nindent 4 } }
  { {- with .Values.serviceAccount.annotations } }
  annotations:
    { {- toYaml . | nindent 4 } }
  { {- end } }
automountServiceAccountToken: { { .Values.serviceAccount.automount } }
{ {- end } }
```

### templates/deployment.yaml 

```yml
user1@user1-500R5K-501R5K-500R5Q:~/helm-test/mychart$ cat templates/deployment.yaml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: { { include "mychart.fullname" . } }
  labels:
    { {- include "mychart.labels" . | nindent 4 } }
spec:
  { {- if not .Values.autoscaling.enabled } }
  replicas: { { .Values.replicaCount } }
  { {- end } }
  selector:
    matchLabels:
      { {- include "mychart.selectorLabels" . | nindent 6 } }
  template:
    metadata:
      { {- with .Values.podAnnotations } }
      annotations:
        { {- toYaml . | nindent 8 } }
      { {- end } }
      labels:
        { {- include "mychart.labels" . | nindent 8 } }
	{ {- with .Values.podLabels } }
        { {- toYaml . | nindent 8 } }
        { {- end } }
    spec:
      { {- with .Values.imagePullSecrets } }
      imagePullSecrets:
        { {- toYaml . | nindent 8 } }
      { {- end } }
      serviceAccountName: { { include "mychart.serviceAccountName" . } }
      securityContext:
        { {- toYaml .Values.podSecurityContext | nindent 8 } }
      containers:
        - name: { { .Chart.Name } }
          securityContext:
            { {- toYaml .Values.securityContext | nindent 12 } }
          image: "{ { .Values.image.repository } }:{ { .Values.image.tag | default .Chart.AppVersion } }"
          imagePullPolicy: { { .Values.image.pullPolicy } }
          ports:
            - name: http
              containerPort: { { .Values.service.port } }
              protocol: TCP
          livenessProbe:
            httpGet:
              path: /
              port: http
          readinessProbe:
            httpGet:
              path: /
              port: http
          resources:
            { {- toYaml .Values.resources | nindent 12 } }
          { {- with .Values.volumeMounts } }
          volumeMounts:
            { {- toYaml . | nindent 12 } }
          { {- end } }
      { {- with .Values.volumes } }
      volumes:
        { {- toYaml . | nindent 8 } }
      { {- end } }
      { {- with .Values.nodeSelector } }
      nodeSelector:
        { {- toYaml . | nindent 8 } }
      { {- end } }
      { {- with .Values.affinity } }
      affinity:
        { {- toYaml . | nindent 8 } }
      { {- end } }
      { {- with .Values.tolerations } }
      tolerations:
        { {- toYaml . | nindent 8 } }
      { {- end } }

```

### templates/service.yaml 

```yml
user1@user1-500R5K-501R5K-500R5Q:~/helm-test/mychart$ cat templates/service.yaml 
apiVersion: v1
kind: Service
metadata:
  name: { { include "mychart.fullname" . } }
  labels:
    { {- include "mychart.labels" . | nindent 4 } }
spec:
  type: { { .Values.service.type } }
  ports:
    - port: { { .Values.service.port } }
      targetPort: http
      protocol: TCP
      name: http
  selector:
    { {- include "mychart.selectorLabels" . | nindent 4 } }
```

### templates/ingress.yaml 

```yml
user1@user1-500R5K-501R5K-500R5Q:~/helm-test/mychart$ cat templates/ingress.yaml 
{ {- if .Values.ingress.enabled -} }
{ {- $fullName := include "mychart.fullname" . -} }
{ {- $svcPort := .Values.service.port -} }
{ {- if and .Values.ingress.className (not (semverCompare ">=1.18-0" .Capabilities.KubeVersion.GitVersion)) } }
  { {- if not (hasKey .Values.ingress.annotations "kubernetes.io/ingress.class") } }
  { {- $_ := set .Values.ingress.annotations "kubernetes.io/ingress.class" .Values.ingress.className} }
  { {- end } }
{ {- end } }
{ {- if semverCompare ">=1.19-0" .Capabilities.KubeVersion.GitVersion -} }
apiVersion: networking.k8s.io/v1
{ {- else if semverCompare ">=1.14-0" .Capabilities.KubeVersion.GitVersion -} }
apiVersion: networking.k8s.io/v1beta1
{ {- else -} }
apiVersion: extensions/v1beta1
{ {- end } }
kind: Ingress
metadata:
  name: { { $fullName } }
  labels:
    { {- include "mychart.labels" . | nindent 4 } }
  { {- with .Values.ingress.annotations } }
  annotations:
    { {- toYaml . | nindent 4 } }
  { {- end } }
spec:
  { {- if and .Values.ingress.className (semverCompare ">=1.18-0" .Capabilities.KubeVersion.GitVersion) } }
  ingressClassName: { { .Values.ingress.className } }
  { {- end } }
  { {- if .Values.ingress.tls } }
  tls:
    { {- range .Values.ingress.tls } }
    - hosts:
        { {- range .hosts } }
        - { { . | quote } }
        { {- end } }
      secretName: { { .secretName } }
    { {- end } }
  { {- end } }
  rules:
    { {- range .Values.ingress.hosts } }
    - host: { { .host | quote } }
      http:
        paths:
          { {- range .paths } }
          - path: { { .path } }
            { {- if and .pathType (semverCompare ">=1.18-0" $.Capabilities.KubeVersion.GitVersion) } }
            pathType: { { .pathType } }
            { {- end } }
            backend:
              { {- if semverCompare ">=1.19-0" $.Capabilities.KubeVersion.GitVersion } }
              service:
                name: { { $fullName } }
                port:
                  number: { { $svcPort } }
              { {- else } }
              serviceName: { { $fullName } }
              servicePort: { { $svcPort } }
              { {- end } }
          { {- end } }
    { {- end } }
{ {- end } }
```