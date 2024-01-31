---
layout: default
title: Jfrog Rest API
parent: Jfrog
grand_parent: CI/CD Tools 
permalink: /docs/cicdtools/jfrog-restapi
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

### Project list

```bash
{jfrog-url}/access/api/v1/projects
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
 

### Create Token

If expires_in is not specified, expiry is 1 year as default.

```sh
POST {jfrog-url}/access/api/v1/tokens/
{ 
  "scope":"applied-permissions/user",
  "username": "mytest",
  "expires_in": "0", //second
  "description" : "test1-desc"
}

{
    "token_id": "b609b791-415c-48a7-969f-xxx",
    "access_token": "eyJxxx",
    "expires_in": 30,
    "scope": "applied-permissions/user",
    "token_type": "Bearer",
    "description": "test1-desc"

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

### Delete token  by token Id

```sh
POST {jfrog-url}/access/api/v1/tokens/877d0a0b-597d-4f56-9fb6-079ffab20210
```


### Token process

#### 0. Create Admin Bearer-Token with no expiry date.

#### 1. Create Token by Username using Admin Bearer-Token and Save user's Access-token in DB 

```sh
POST {jfrog-url}/access/api/v1/tokens/
{ 
  "scope":"applied-permissions/user",
  "username": "mytest",
  "expires_in": "0", //second
  "description" : "test1-desc"
}

{
    "token_id": "b609b791-415c-48a7-969f-xxx",
    "access_token": "eyJxxx",
    "expires_in": 30,
    "scope": "applied-permissions/user",
    "token_type": "Bearer",
    "description": "test1-desc"

```

#### 2. Get Access-token from DB and Connect to Jfrog

If token_id saved in DB, it is possible to check its expiry date with below using Admin Bearer-Token before using access-token.

```sh
GET POST {jfrog-url}/access/api/v1/tokens/76a90d6d-3206-4f23-a72a-fa8d2dd05b0b
```

#### 3. If 401 error occurs in response, create Token by Username using Admin Bearer-Token and Save user's Access-token in DB 

```sh
POST {jfrog-url}/access/api/v1/tokens/
{ 
  "scope":"applied-permissions/user",
  "username": "mytest",
  "expires_in": "0", //second
  "description" : "test1-desc"
}

{
    "token_id": "b609b791-415c-48a7-969f-xxx",
    "access_token": "eyJxxx",
    "expires_in": 30,
    "scope": "applied-permissions/user",
    "token_type": "Bearer",
    "description": "test1-desc"
 
```

#### 4. Keep going 1 ~ 4.


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

## **Docker Layer**

### Layers Visualization

```bash
GET https://dmjgr.jfrog.io/artifactory/api/docker/spring-vue-starter-repo/v2/spring-vue-image-child/manifests/95



{
  "schemaVersion" : 1,
  "name" : "spring-vue-image-child",
  "tag" : "95",
  "architecture" : "amd64",
  "fsLayers" : [ {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:2bde522f954fe46c4abe389ff03d4f12ef885aa822bf6a9cae730d4f89440d2f"
  }, {
    "blobSum" : "sha256:2678ca58edbecc75c354e98f96efa24be413e49ccc284aa51cb0d6c262738128"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:a7203ca35e75e068651c9907d659adc721dba823441b78639fde66fc988f042f"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:de849f1cfbe60b1c06a1db83a3129ab0ea397c4852b98e3e4300b12ee57ba111"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:38a980f2cc8accf69c23deae6743d42a87eb34a54f02396f3fcfd7c2d06e2c5b"
  } ],
  "history" : [ {
    "v1Compatibility" : "{\"container\":\"a2c93c431b6774ee860328409b263c704eff0a7ccb497464befc09cb85faf5b3\",\"parent\":\"6284645ce2757b9ac13f4e2f4ae4fd4e0294f8260f99ac0da174c2ba55c61431\",\"throwaway\":true,\"os\":\"linux\",\"created\":\"2023-11-24T13:38:34.194337895Z\",\"container_config\":{\"Hostname\":\"a2c93c431b67\",\"Domainname\":\"\",\"User\":\"\",\"AttachStdin\":false,\"AttachStdout\":false,\"AttachStderr\":false,\"Tty\":false,\"OpenStdin\":false,\"StdinOnce\":false,\"Env\":[\"PATH=/usr/java/openjdk-17/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin\",\"JAVA_HOME=/usr/java/openjdk-17\",\"LANG=C.UTF-8\",\"JAVA_VERSION=17.0.2\",\"MY_NAME=John Doe\"],\"Cmd\":[\"/bin/sh\",\"-c\",\"#(nop) \",\"ENV MY_NAME=John Doe\"],\"Image\":\"sha256:53ae2020bd8e6bd80952db2ce7c7ffcf01d56c43bd1fd44c3dd918549d4eca85\",\"Volumes\":null,\"WorkingDir\":\"/app\",\"Entrypoint\":[\"java\",\"-jar\",\"springVueStarter-0.0.1-SNAPSHOT.jar\"],\"OnBuild\":null,\"Labels\":{}},\"id\":\"d0412b62f2468198f91c92ef5c7f2b4eefdb53eeeb114364e263952da03aaea6\",\"config\":{\"Hostname\":\"\",\"Domainname\":\"\",\"User\":\"\",\"AttachStdin\":false,\"AttachStdout\":false,\"AttachStderr\":false,\"Tty\":false,\"OpenStdin\":false,\"StdinOnce\":false,\"Env\":[\"PATH=/usr/java/openjdk-17/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin\",\"JAVA_HOME=/usr/java/openjdk-17\",\"LANG=C.UTF-8\",\"JAVA_VERSION=17.0.2\",\"MY_NAME=John Doe\"],\"Cmd\":null,\"Image\":\"sha256:53ae2020bd8e6bd80952db2ce7c7ffcf01d56c43bd1fd44c3dd918549d4eca85\",\"Volumes\":null,\"WorkingDir\":\"/app\",\"Entrypoint\":[\"java\",\"-jar\",\"springVueStarter-0.0.1-SNAPSHOT.jar\"],\"OnBuild\":null,\"Labels\":null},\"docker_version\":\"24.0.5\",\"architecture\":\"amd64\"}"
  }, {
    "v1Compatibility" : "{\"id\":\"6284645ce2757b9ac13f4e2f4ae4fd4e0294f8260f99ac0da174c2ba55c61431\",\"parent\":\"5c038964690f2eebea3feb8a0eaf48b52de43c5a93ce2ed852dc13b6e58daf92\",\"comment\":null,\"created\":\"2023-10-27T14:08:59.080963116Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  ENTRYPOINT [\\\"java\\\" \\\"-jar\\\" \\\"springVueStarter-0.0.1-SNAPSHOT.jar\\\"]\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"5c038964690f2eebea3feb8a0eaf48b52de43c5a93ce2ed852dc13b6e58daf92\",\"parent\":\"9ce3e6d90a73c949485418a1ffe7958113c2026d1cc722c0ba64a3cd088400b0\",\"comment\":null,\"created\":\"2023-10-27T14:08:58.554940559Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop) COPY file:3708ed22897e6f3364d3a7685bf03434e941c9676059fc904b08fdc1e1c234d7 in . \"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"9ce3e6d90a73c949485418a1ffe7958113c2026d1cc722c0ba64a3cd088400b0\",\"parent\":\"eff06ff0328aa3b368603706fe11484281cafb352051fc4d247c10f364674c78\",\"comment\":null,\"created\":\"2023-10-27T14:08:58.189848571Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop) WORKDIR /app\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"eff06ff0328aa3b368603706fe11484281cafb352051fc4d247c10f364674c78\",\"parent\":\"e9799d31bb42665d55ea83786be278d044bdd77b7045b9e27dd976d80eba1fb0\",\"comment\":null,\"created\":\"2022-04-27T21:54:28.871350186Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  CMD [\\\"jshell\\\"]\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"e9799d31bb42665d55ea83786be278d044bdd77b7045b9e27dd976d80eba1fb0\",\"parent\":\"eb51a7d6c41e9e10355f32f85033e48f4dfeed042f8d793b965eb9de8bc5ba36\",\"comment\":null,\"created\":\"2022-04-27T21:54:28.248363534Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c set -eux; \\t\\tarch=\\\"$(objdump=\\\"$(command -v objdump)\\\" && objdump --file-headers \\\"$objdump\\\" | awk -F '[:,]+[[:space:]]+' '$1 == \\\"architecture\\\" { print $2 }')\\\"; \\tcase \\\"$arch\\\" in \\t\\t'i386:x86-64') \\t\\t\\tdownloadUrl='https://download.java.net/java/GA/jdk17.0.2/dfd4a8d0985749f896bed50d7138ee7f/8/GPL/openjdk-17.0.2_linux-x64_bin.tar.gz'; \\t\\t\\tdownloadSha256='0022753d0cceecacdd3a795dd4cea2bd7ffdf9dc06e22ffd1be98411742fbb44'; \\t\\t\\t;; \\t\\t'aarch64') \\t\\t\\tdownloadUrl='https://download.java.net/java/GA/jdk17.0.2/dfd4a8d0985749f896bed50d7138ee7f/8/GPL/openjdk-17.0.2_linux-aarch64_bin.tar.gz'; \\t\\t\\tdownloadSha256='13bfd976acf8803f862e82c7113fb0e9311ca5458b1decaef8a09ffd91119fa4'; \\t\\t\\t;; \\t\\t*) echo >&2 \\\"error: unsupported architecture: '$arch'\\\"; exit 1 ;; \\tesac; \\t\\tcurl -fL -o openjdk.tgz \\\"$downloadUrl\\\"; \\techo \\\"$downloadSha256 *openjdk.tgz\\\" | sha256sum --strict --check -; \\t\\tmkdir -p \\\"$JAVA_HOME\\\"; \\ttar --extract \\t\\t--file openjdk.tgz \\t\\t--directory \\\"$JAVA_HOME\\\" \\t\\t--strip-components 1 \\t\\t--no-same-owner \\t; \\trm openjdk.tgz*; \\t\\trm -rf \\\"$JAVA_HOME/lib/security/cacerts\\\"; \\tln -sT /etc/pki/ca-trust/extracted/java/cacerts \\\"$JAVA_HOME/lib/security/cacerts\\\"; \\t\\tln -sfT \\\"$JAVA_HOME\\\" /usr/java/default; \\tln -sfT \\\"$JAVA_HOME\\\" /usr/java/latest; \\tfor bin in \\\"$JAVA_HOME/bin/\\\"*; do \\t\\tbase=\\\"$(basename \\\"$bin\\\")\\\"; \\t\\t[ ! -e \\\"/usr/bin/$base\\\" ]; \\t\\talternatives --install \\\"/usr/bin/$base\\\" \\\"$base\\\" \\\"$bin\\\" 20000; \\tdone; \\t\\tjava -Xshare:dump; \\t\\tfileEncoding=\\\"$(echo 'System.out.println(System.getProperty(\\\"file.encoding\\\"))' | jshell -s -)\\\"; [ \\\"$fileEncoding\\\" = 'UTF-8' ]; rm -rf ~/.java; \\tjavac --version; \\tjava --version\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"eb51a7d6c41e9e10355f32f85033e48f4dfeed042f8d793b965eb9de8bc5ba36\",\"parent\":\"9f856f090b3536454e5dfb06417462f2b7f391d5dbcc5d4d8ce39eccc26f6528\",\"comment\":null,\"created\":\"2022-04-27T21:54:18.81978611Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  ENV JAVA_VERSION=17.0.2\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"9f856f090b3536454e5dfb06417462f2b7f391d5dbcc5d4d8ce39eccc26f6528\",\"parent\":\"8ac2449f7c71a09ae2fcf0253d007f54855bd2b51e6e7a524c12d80e81da75c0\",\"comment\":null,\"created\":\"2022-04-27T21:54:18.72299961Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  ENV LANG=C.UTF-8\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"8ac2449f7c71a09ae2fcf0253d007f54855bd2b51e6e7a524c12d80e81da75c0\",\"parent\":\"9e159ca1a23d7dd7f70a80b77a2009aab1043a985bee651535e1b4ac538cf83a\",\"comment\":null,\"created\":\"2022-04-27T21:54:18.615815298Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  ENV PATH=/usr/java/openjdk-17/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"9e159ca1a23d7dd7f70a80b77a2009aab1043a985bee651535e1b4ac538cf83a\",\"parent\":\"9ff05dccb75eb8b47fd096d2bbe95a98475434351cc3161faeda4b20474d1f27\",\"comment\":null,\"created\":\"2022-04-27T21:54:18.51503692Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  ENV JAVA_HOME=/usr/java/openjdk-17\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"9ff05dccb75eb8b47fd096d2bbe95a98475434351cc3161faeda4b20474d1f27\",\"parent\":\"819efa67053eb6082ff11632303f437a4ac4304b8c6bc179045a1a4943bb607b\",\"comment\":null,\"created\":\"2022-04-27T21:53:12.339171869Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c set -eux; \\tmicrodnf install \\t\\tgzip \\t\\ttar \\t\\t\\t\\tbinutils \\t\\tfreetype fontconfig \\t; \\tmicrodnf clean all\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"819efa67053eb6082ff11632303f437a4ac4304b8c6bc179045a1a4943bb607b\",\"parent\":\"307d1c5b9e9926994500bc7e8c7be52284699ad8b7bceaed378f6b7cbb328a91\",\"comment\":null,\"created\":\"2022-04-27T20:33:05.045198088Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  CMD [\\\"/bin/bash\\\"]\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"307d1c5b9e9926994500bc7e8c7be52284699ad8b7bceaed378f6b7cbb328a91\",\"parent\":\"\",\"comment\":null,\"created\":\"2022-04-27T20:33:04.547787858Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop) ADD file:9893213a9ea238f53ac68d87a3cf2f05d86763688392e5ddb6a2c9b60d3550a6 in / \"]}}"
  } ],
  "signatures" : [ {
    "header" : {
      "jwk" : {
        "crv" : "P-256",
        "kty" : "EC",
        "x" : "mUxMr2CKQa1X5BvRbNdXTgixkhgtzg2f-U8tHk1hkDU",
        "y" : "J7xFw_viP6wa3oVqtuva90aRv1twOO2_hzlvaBtBvJc"
      },
      "alg" : "ES256"
    },
    "signature" : "0vF2zxKBSGXAuhHUiQuIZddIO_wevu-fU08dnPAWeQAB7ZGgpI53ouZTwXqdcjKFMZX2_dAHnXx_jUS0LjTArA",
    "protected" : "eyJmb3JtYXRMZW5ndGgiOjk0ODgsImZvcm1hdFRhaWwiOiJDbjAiLCJ0aW1lIjoiMjAyMy0xMS0yNFQxOjQ3OjM0KzAwMDAifQ"
  } ]
}

```

### Checksum Search

```bash
GET https://dmjgr.jfrog.io/artifactory/api/search/checksum?sha256=2bde522f954fe46c4abe389ff03d4f12ef885aa822bf6a9cae730d4f89440d2f


    "results": [
        {
            "uri": "https://dmjgr.jfrog.io/artifactory/api/storage/spring-vue-starter-repo/spring-vue-image/94/sha256__2bde522f954fe46c4abe389ff03d4f12ef885aa822bf6a9cae730d4f89440d2f"
        },
        {
            "uri": "https://dmjgr.jfrog.io/artifactory/api/storage/spring-vue-starter-repo/spring-vue-image/93/sha256__2bde522f954fe46c4abe389ff03d4f12ef885aa822bf6a9cae730d4f89440d2f"
        },
        {
            "uri": "https://dmjgr.jfrog.io/artifactory/api/storage/spring-vue-starter-repo/spring-vue-image-child/95/sha256__2bde522f954fe46c4abe389ff03d4f12ef885aa822bf6a9cae730d4f89440d2f"
        },
        {
            "uri": "https://dmjgr.jfrog.io/artifactory/api/storage/spring-vue-starter-repo/spring-vue-image/90/sha256__2bde522f954fe46c4abe389ff03d4f12ef885aa822bf6a9cae730d4f89440d2f"
        },
        {
            "uri": "https://dmjgr.jfrog.io/artifactory/api/storage/docker-trial/testimage/1.123/sha256__2bde522f954fe46c4abe389ff03d4f12ef885aa822bf6a9cae730d4f89440d2f"
        }
    ]
}
```



---

## **Configuration**

### TLS Certificate for RABBIT MQ

```bash
GET {jfrog-url}/xray/api/v1/configuration/systemParameters
```