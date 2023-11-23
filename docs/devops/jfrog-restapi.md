---
layout: default
title: Jfrog Rest API
parent: Jfrog
grand_parent: Devops
permalink: /docs/devops/jfrog-restapi/
nav_order: 30
---

# Jfrog Rest API
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---


{: .note }
Jfrog 공식 웹사이트 Help Center 외에 정리된 자료를 찾기 어려워 Artifactory, Xray 와 관련된 Rest API 를 정리하였으며, 버전정보는 Artifactory 7.27.3, Xray 3.27.4 를 기준으로 작성하였습니다.   
  

## **Repositories**

### Get Repositories

```bash
GET {jfrog-url}/artifactory/api/repositories

GET {jfrog-url}/artifactory/api/repositories/7test2
```

### Create Repositories

```bash
PUT {jfrog-url}/artifactory/api/repositories/aaa-release-local

{
 "key": "aaa-release-local",
 "environments":["DEV", "PROD"],
 "rclass" : "local",
 "packageType": "docker",
 "description": "testtest n"

}
```

---

## **Local 저장소 생성**

### Repository 생성

```bash
PUT {jfrog-url}/artifactory/api/repositories/pg01
{
    "key": "pg01",
    "rclass": "local",
    "packageType": "generic",
    "description": "My local repository",
    "notes": "Optional notes about the repository"
}
```

### Project 생성

```bash
POST {jfrog-url}//access/api/v1/projects
{
  "display_name": "pg01-project",
  "description": "The binary repository manager",
  "admin_privileges": {
    "manage_members": true,
    "manage_resources": true,
    "manage_security_assets": true,
    "index_resources": true,
    "allow_ignore_rules": true
  },
  "storage_quota_bytes": 10737418240,
  "project_key": "pg01-project"
}

```

### Project 에 role 생성

```bash
POST {jfrog-url}/access/api/v1/projects/pg01-project/roles/
{
    "name": "testrole2",
    "description": "",
    "type": "CUSTOM",
    "environments": [
        "PROD",
        "DEV"
    ],
    "actions": [
        "READ_REPOSITORY",
        "ANNOTATE_REPOSITORY",
        "DEPLOY_CACHE_REPOSITORY",
        "DELETE_OVERWRITE_REPOSITORY",
        "MANAGE_XRAY_MD_REPOSITORY",
        "READ_RELEASE_BUNDLE",
        "ANNOTATE_RELEASE_BUNDLE",
        "CREATE_RELEASE_BUNDLE",
        "DISTRIBUTE_RELEASE_BUNDLE",
        "DELETE_RELEASE_BUNDLE",
        "MANAGE_XRAY_MD_RELEASE_BUNDLE",
        "READ_BUILD",
        "ANNOTATE_BUILD",
        "DEPLOY_BUILD",
        "DELETE_BUILD",
        "MANAGE_XRAY_MD_BUILD",
        "READ_SOURCES_PIPELINE",
        "TRIGGER_PIPELINE",
        "READ_INTEGRATIONS_PIPELINE",
        "READ_POOLS_PIPELINE",
        "REPORTS_SECURITY",
        "WATCHES_SECURITY",
        "POLICIES_SECURITY",
        "RULES_SECURITY",
        "READ_POLICIES_SECURITY"
    ]
}
```

### user 추가(role 부여)

```bash
PUT {jfrog-url}/access/api/v1/projects/pg01-project/users/hotmira
{
  "name": "hotmira",
  "roles": [
    "testrole"
  ]
}
```

### 서브 폴더 생성

```bash
# 빈 디렉토리일 경우 뒤에 / 까지 붙인다.(5분 초과시 삭제됨,별도 설정필요): PUT 
PUT {jfrog-url}/artifactory/pg01/p03/

PUT {jfrog-url}/artifactory/pg01/p01/docker1.png
```

### 서브폴더를 Project 에 추가

```bash
PUT {jfrog-url}/access/api/v1/projects/_/share/repositories/pg01/pg01-project

```

### 서브폴더를 Project 에 제거

```bash
DELETE  {jfrog-url}/access/api/v1/projects/_/share/repositories/docker-demo-local/pg01-project
```

---

## **Storage**

### storage

```bash
{jfrog-url}/artifactory/api/storage/dctest
```

---

## **Access**

### ping request

```bash
{jfrog-url}/access/api/v1/system/ping
```

### get tokens by Token ID

```bash
{jfrog-url}/access/api/v1/tokens

{jfrog-url}/access/api/v1/tokens/07e245ce-10ce-4b73-8fff-df873e4f3682
```

### get tokens by Username

```bash
POST {jfrog-url}/access/api/v1/tokens/
{
    "username" : "jwpark"
}
```

### project list

```bash
{jfrog-url}/access/api/v1/projects
```

---

## **Router**

### router health check

```bash
{jfrog-url}/router/api/v1/system/health
```

---

## **Event**

### webhook list

```bash
GET {jfrog-url}/event/api/v1/subscriptions
```

## Ignore Rule

### create ignore rule

```bash
POST {jfrog-url}/xray/api/v1/ignore_rules
```

### get ignore rule

```bash
GET {jfrog-url}/xray/api/v1/ignore_rules

GET {jfrog-url}/xray/api/v1/ignore_rules/ee1e24ba-523f-4206-7f45-88ab211fe712
```

### delete ignore rules

```bash
DELETE {jfrog-url}/xray/api/v1/ignore_rules/ee1e24ba-523f-4206-7f45-88ab211fe712
```

---

## **Scan**

### scan

```bash
GET {jfrog-url}/xray/api/v1/scanArtifact
```

---

## **Watches**

### get watches

```bash
GET {jfrog-url}/xray/api/v2/watches
```

---

## **Configuration**

### TLS Certificate for RABBIT MQ

```bash
GET {jfrog-url}/xray/api/v1/configuration/systemParameters
```