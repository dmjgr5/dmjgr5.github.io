---
layout: default
title: Docker Layer Analysis
parent: Jfrog
grand_parent: CI/CD Tools 
permalink: /docs/cicdtools/jfrog-dockerlayer
nav_order: 31
---

# Docker Layer Analysis
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---


{: .note }
Docker Image 의 Base Image 를 확인하기 위한 분석 내용으로 정리해보고자 한다.
예시로서 아래와 같은 Docker Image 가 있다는 가정하에 분석한다.
```bash

ㄴ spring-vue-image:90
  : FROM openjdk:17
  : ...

    ㄴ spring-vue-image-child:95
      : FROM dmjgr11.jfrog.io/spring-vue-starter-repo/spring-vue-image:90
      : ...

            ㄴ spring-vue-image-child-child:97
              : FROM dmjgr11.jfrog.io/spring-vue-starter-repo/spring-vue-image-child:95
              : ...



```

### Child of Child Image

```bash
https://dmjgr11.jfrog.io/artifactory/api/docker/spring-vue-starter-repo/v2/spring-vue-image-child-child/manifests/97

{
  "schemaVersion" : 1,
  "name" : "spring-vue-image-child-child",
  "tag" : "97",
  "architecture" : "amd64",
  "fsLayers" : [ {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {

  ## 중간 생략 

    "blobSum" : "sha256:38a980f2cc8accf69c23deae6743d42a87eb34a54f02396f3fcfd7c2d06e2c5b"
  } ],
  "history" : [ {
    "v1Compatibility" : "{\"container\":\"bbbf8f6fbd7fdca14df2542cefd8bd2707fe184821588df701a8289a187c5a07\",\"parent\":\"5c9af3c061f590ba4c44c90f1eb10b959c9376fefd3419b1ed87e09ffe962ae8\",\"throwaway\":true,\"os\":\"linux\",\"created\":\"2023-11-24T14:35:34.142786732Z\",\"container_config\":{\"Hostname\":\"bbbf8f6fbd7f\",\"Domainname\":\"\",\"User\":\"\",\"AttachStdin\":false,\"AttachStdout\":false,\"AttachStderr\":false,\"Tty\":false,\"OpenStdin\":false,\"StdinOnce\":false,\"Env\":[\"PATH=/usr/java/openjdk-17/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin\",\"JAVA_HOME=/usr/java/openjdk-17\",\"LANG=C.UTF-8\",\"JAVA_VERSION=17.0.2\",\"MY_NAME=IAM CHILD OF CHILD\"],\"Cmd\":[\"/bin/sh\",\"-c\",\"#(nop) \",\"ENV MY_NAME=IAM CHILD OF CHILD\"],\"Image\":\"sha256:d2ee3fab8b7fa900411abee2f27d10d00872fe11d386bac642fd515c91adeedd\",\"Volumes\":null,\"WorkingDir\":\"/app\",\"Entrypoint\":[\"java\",\"-jar\",\"springVueStarter-0.0.1-SNAPSHOT.jar\"],\"OnBuild\":null,\"Labels\":{}},\"id\":\"16b1dc92aea35373c31ccff66aea179c0436d7fc4e5a630cd0267628fa2d5e8b\",\"config\":{\"Hostname\":\"\",\"Domainname\":\"\",\"User\":\"\",\"AttachStdin\":false,\"AttachStdout\":false,\"AttachStderr\":false,\"Tty\":false,\"OpenStdin\":false,\"StdinOnce\":false,\"Env\":[\"PATH=/usr/java/openjdk-17/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin\",\"JAVA_HOME=/usr/java/openjdk-17\",\"LANG=C.UTF-8\",\"JAVA_VERSION=17.0.2\",\"MY_NAME=IAM CHILD OF CHILD\"],\"Cmd\":null,\"Image\":\"sha256:d2ee3fab8b7fa900411abee2f27d10d00872fe11d386bac642fd515c91adeedd\",\"Volumes\":null,\"WorkingDir\":\"/app\",\"Entrypoint\":[\"java\",\"-jar\",\"springVueStarter-0.0.1-SNAPSHOT.jar\"],\"OnBuild\":null,\"Labels\":null},\"docker_version\":\"24.0.5\",\"architecture\":\"amd64\"}"
  }, {
  
  
  ## 중간 생략

    "v1Compatibility" : "{\"id\":\"eff06ff0328aa3b368603706fe11484281cafb352051fc4d247c10f364674c78\",\"parent\":\"e9799d31bb42665d55ea83786be278d044bdd77b7045b9e27dd976d80eba1fb0\",\"comment\":null,\"created\":\"2022-04-27T21:54:28.871350186Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  CMD [\\\"jshell\\\"]\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"e9799d31bb42665d55ea83786be278d044bdd77b7045b9e27dd976d80eba1fb0\",\"parent\":\"eb51a7d6c41e9e10355f32f85033e48f4dfeed042f8d793b965eb9de8bc5ba36\",\"comment\":null,\"created\":\"2022-04-27T21:54:28.248363534Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c set -eux; \\t\\tarch=\\\"$(objdump=\\\"$(command -v objdump)\\\" && objdump --file-headers \\\"$objdump\\\" | awk -F '[:,]+[[:space:]]+' '$1 == \\\"architecture\\\" { print $2 }')\\\"; \\tcase \\\"$arch\\\" in \\t\\t'i386:x86-64') \\t\\t\\tdownloadUrl='https://download.java.net/java/GA/jdk17.0.2/dfd4a8d0985749f896bed50d7138ee7f/8/GPL/openjdk-17.0.2_linux-x64_bin.tar.gz'; \\t\\t\\tdownloadSha256='0022753d0cceecacdd3a795dd4cea2bd7ffdf9dc06e22ffd1be98411742fbb44'; \\t\\t\\t;; \\t\\t'aarch64') \\t\\t\\tdownloadUrl='https://download.java.net/java/GA/jdk17.0.2/dfd4a8d0985749f896bed50d7138ee7f/8/GPL/openjdk-17.0.2_linux-aarch64_bin.tar.gz'; \\t\\t\\tdownloadSha256='13bfd976acf8803f862e82c7113fb0e9311ca5458b1decaef8a09ffd91119fa4'; \\t\\t\\t;; \\t\\t*) echo >&2 \\\"error: unsupported architecture: '$arch'\\\"; exit 1 ;; \\tesac; \\t\\tcurl -fL -o openjdk.tgz \\\"$downloadUrl\\\"; \\techo \\\"$downloadSha256 *openjdk.tgz\\\" | sha256sum --strict --check -; \\t\\tmkdir -p \\\"$JAVA_HOME\\\"; \\ttar --extract \\t\\t--file openjdk.tgz \\t\\t--directory \\\"$JAVA_HOME\\\" \\t\\t--strip-components 1 \\t\\t--no-same-owner \\t; \\trm openjdk.tgz*; \\t\\trm -rf \\\"$JAVA_HOME/lib/security/cacerts\\\"; \\tln -sT /etc/pki/ca-trust/extracted/java/cacerts \\\"$JAVA_HOME/lib/security/cacerts\\\"; \\t\\tln -sfT \\\"$JAVA_HOME\\\" /usr/java/default; \\tln -sfT \\\"$JAVA_HOME\\\" /usr/java/latest; \\tfor bin in \\\"$JAVA_HOME/bin/\\\"*; do \\t\\tbase=\\\"$(basename \\\"$bin\\\")\\\"; \\t\\t[ ! -e \\\"/usr/bin/$base\\\" ]; \\t\\talternatives --install \\\"/usr/bin/$base\\\" \\\"$base\\\" \\\"$bin\\\" 20000; \\tdone; \\t\\tjava -Xshare:dump; \\t\\tfileEncoding=\\\"$(echo 'System.out.println(System.getProperty(\\\"file.encoding\\\"))' | jshell -s -)\\\"; [ \\\"$fileEncoding\\\" = 'UTF-8' ]; rm -rf ~/.java; \\tjavac --version; \\tjava --version\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"eb51a7d6c41e9e10355f32f85033e48f4dfeed042f8d793b965eb9de8bc5ba36\",\"parent\":\"9f856f090b3536454e5dfb06417462f2b7f391d5dbcc5d4d8ce39eccc26f6528\",\"comment\":null,\"created\":\"2022-04-27T21:54:18.81978611Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  ENV JAVA_VERSION=17.0.2\"]}}"
  }, {
    
    # 중간 생략

    }, {
    "v1Compatibility" : "{\"id\":\"307d1c5b9e9926994500bc7e8c7be52284699ad8b7bceaed378f6b7cbb328a91\",\"parent\":\"\",\"comment\":null,\"created\":\"2022-04-27T20:33:04.547787858Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop) ADD file:9893213a9ea238f53ac68d87a3cf2f05d86763688392e5ddb6a2c9b60d3550a6 in / \"]}}"
  } ],
  "signatures" : [ {
    "header" : {
      "jwk" : {
        "crv" : "P-256",
        "kty" : "EC",
        "x" : "xcUHWc_FUb-I5ohDPSuFL5LZ2MttJiq50YV6GkdNeWc",
        "y" : "aC8lnoi5AviTdmIEXXo9kpnuVITwaGV_FrVLxwzcSAw"
      },
      "alg" : "ES256"
    },
    "signature" : "0kZo0XIcVUE4S6qeSB6JcytUe7eKwn1Jh6Ll9GoldpCdv3sWYvrUvXz1JyZX0JhUQ4P8s11_eb-8Pv06z_khSw",
    "protected" : "eyJmb3JtYXRMZW5ndGgiOjk5NzIsImZvcm1hdFRhaWwiOiJDbjAiLCJ0aW1lIjoiMjAyMy0xMS0yNFQyOjQ0OjQ2KzAwMDAifQ"
  } ]
}
```

위와 같이 `\"Image\":\"sha256:` 부분이 존재한다. 이 해쉬넘버가 베이스 이미지 해쉬일 것으로 추측된다. 한번 확인해 보자.

```bash
GET https://dmjgr11.jfrog.io/artifactory/api/search/checksum?sha256=d2ee3fab8b7fa900411abee2f27d10d00872fe11d386bac642fd515c91adeedd

{
    "results": [
        {
            "uri": "https://dmjgr11.jfrog.io/artifactory/api/storage/spring-vue-starter-repo/spring-vue-image-child/95/sha256__d2ee3fab8b7fa900411abee2f27d10d00872fe11d386bac642fd515c91adeedd"
        },
        {
            "uri": "https://dmjgr11.jfrog.io/artifactory/api/storage/spring-vue-starter-repo/spring-vue-image-child/_uploads/sha256__d2ee3fab8b7fa900411abee2f27d10d00872fe11d386bac642fd515c91adeedd"
        }
    ]
}
```

실제로 Base Image 로 활용했던 `spring-vue-image-child/95` 가 조회된다.

## Child Image

이 케이스는 어떨까? 

```sh
GET https://dmjgr11.jfrog.io/artifactory/api/docker/spring-vue-starter-repo/v2/spring-vue-image-child/manifests/95

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
    
    ### 중간 생략 
```
`\"Image\":\"sha256:53ae2020bd8e6bd80952db2ce7c7ffcf01d56c43bd1fd44c3dd918549d4eca85\",` 이 부분을 조회해 보면 아래와 같다.

```sh
GET https://dmjgr11.jfrog.io/artifactory/api/search/checksum?sha256=53ae2020bd8e6bd80952db2ce7c7ffcf01d56c43bd1fd44c3dd918549d4eca85

{
    "results": [
        {
            "uri": "https://dmjgr11.jfrog.io/artifactory/api/storage/docker-trial/testimage/1.123/sha256__53ae2020bd8e6bd80952db2ce7c7ffcf01d56c43bd1fd44c3dd918549d4eca85"
        },
        {
            "uri": "https://dmjgr11.jfrog.io/artifactory/api/storage/spring-vue-starter-repo/spring-vue-image/93/sha256__53ae2020bd8e6bd80952db2ce7c7ffcf01d56c43bd1fd44c3dd918549d4eca85"
        },
        {
            "uri": "https://dmjgr11.jfrog.io/artifactory/api/storage/spring-vue-starter-repo/spring-vue-image/90/sha256__53ae2020bd8e6bd80952db2ce7c7ffcf01d56c43bd1fd44c3dd918549d4eca85"
        }
    ]
}

```

이미지로 사용된 `spring-vue-starter-repo/spring-vue-image/90` 를 찾아볼 수 있다.

## Public Base Image 

Public Base Image 로 구성된 이미지는 어떨까?

```sh
GET https://dmjgr11.jfrog.io/artifactory/api/docker/spring-vue-starter-repo/v2/spring-vue-image/manifests/90


{
  "schemaVersion" : 1,
  "name" : "spring-vue-image",
  "tag" : "90",
  "architecture" : "amd64",
  "fsLayers" : [ {
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
    "v1Compatibility" : "{\"container\":\"7dc99e0582f8a72cf69609281a1a2c0c3a7819b514007d40d34c1a2790f73014\",\"parent\":\"5c038964690f2eebea3feb8a0eaf48b52de43c5a93ce2ed852dc13b6e58daf92\",\"throwaway\":true,\"os\":\"linux\",\"created\":\"2023-10-27T14:08:59.080963116Z\",\"container_config\":{\"Hostname\":\"7dc99e0582f8\",\"Domainname\":\"\",\"User\":\"\",\"AttachStdin\":false,\"AttachStdout\":false,\"AttachStderr\":false,\"Tty\":false,\"OpenStdin\":false,\"StdinOnce\":false,\"Env\":[\"PATH=/usr/java/openjdk-17/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin\",\"JAVA_HOME=/usr/java/openjdk-17\",\"LANG=C.UTF-8\",\"JAVA_VERSION=17.0.2\"],\"Cmd\":[\"/bin/sh\",\"-c\",\"#(nop) \",\"ENTRYPOINT [\\\"java\\\" \\\"-jar\\\" \\\"springVueStarter-0.0.1-SNAPSHOT.jar\\\"]\"],\"Image\":\"sha256:925cc8c298f96a1e65fb921a97d2070f07d0ee5d8fc2bf51a4eefa21f3d5801c\",\"Volumes\":null,\"WorkingDir\":\"/app\",\"Entrypoint\":[\"java\",\"-jar\",\"springVueStarter-0.0.1-SNAPSHOT.jar\"],\"OnBuild\":null,\"Labels\":{}},\"id\":\"bb29b57540358b046517e807f0161de09794647b0bdcc8e3baad2f10270e25e6\",\"config\":{\"Hostname\":\"\",\"Domainname\":\"\",\"User\":\"\",\"AttachStdin\":false,\"AttachStdout\":false,\"AttachStderr\":false,\"Tty\":false,\"OpenStdin\":false,\"StdinOnce\":false,\"Env\":[\"PATH=/usr/java/openjdk-17/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin\",\"JAVA_HOME=/usr/java/openjdk-17\",\"LANG=C.UTF-8\",\"JAVA_VERSION=17.0.2\"],\"Cmd\":null,\"Image\":\"sha256:925cc8c298f96a1e65fb921a97d2070f07d0ee5d8fc2bf51a4eefa21f3d5801c\",\"Volumes\":null,\"WorkingDir\":\"/app\",\"Entrypoint\":[\"java\",\"-jar\",\"springVueStarter-0.0.1-SNAPSHOT.jar\"],\"OnBuild\":null,\"Labels\":null},\"docker_version\":\"24.0.5\",\"architecture\":\"amd64\"}"
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
        "x" : "dGRDKBAV4COsK_f_Ll4k7QePfLE5-0DqZV9i5dLAJDw",
        "y" : "mES9BDi2ZjAyGsoGpa89i9r1akgvyJVIEekqrmrPiAM"
      },
      "alg" : "ES256"
    },
    "signature" : "KPe3Q2CF_H2i9PHl6utoMnqXZ95NstYGSHhx1rY2vk_WFkt1z7IiIlEJ_h2pQAlGSbgasGUB_TEJ6drbfzwq-g",
    "protected" : "eyJmb3JtYXRMZW5ndGgiOjg5OTIsImZvcm1hdFRhaWwiOiJDbjAiLCJ0aW1lIjoiMjAyMy0xMS0yNFQzOjU6MjcrMDAwMCJ9"
  } ]
}
```


`\"Image\":\"sha256:925cc8c298f96a1e65fb921a97d2070f07d0ee5d8fc2bf51a4eefa21f3d5801c\` 를 찾아보자.

```sh
GET https://dmjgr11.jfrog.io/artifactory/api/search/checksum?sha256=925cc8c298f96a1e65fb921a97d2070f07d0ee5d8fc2bf51a4eefa21f3d5801c

{
    "results": []
}
```

조회되지 않는다. 이것은 없다는 것은 베이스 이미지가 로컬에서 관리되지 않는 Public 이라는 것을 추즉할 수 있다. 


## 분석 요약



## 분석 결과
- 최상위 레이어 즉, Container Layer 에서 "Image" 키값을 이용하여 base image layer 에 해당되는 해쉬값을 추출 및 해당 layer 가 참조되는 Image Name 들을 확인할 수 있다.
- "tdownloadUrl" 정보는 Origin Layer 라는 것을 확인할 수 있다.
- base image 삭제 후에도 빌드는 된다. 구글링을 해보니 cache 에 이미지가 남아 있어서 그렇다고 한다.
- 그렇다고 해도 base image 의 해쉬 즉, 이미지 레이어가 있고 없고는 어떻게 알 수 있을까?

다음 장인 [Docker Layer Analysis 상세 분석] 에서 좀 더 알아보자.

