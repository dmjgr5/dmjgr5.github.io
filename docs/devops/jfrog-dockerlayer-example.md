---
layout: default
title: Docker Layer Analysis 상세 분석
parent: Jfrog
grand_parent: Devops
permalink: /docs/devops/jfrog-dockerlayer-example/
nav_order: 32
---

# Docker Layer Analysis 상세 분석
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---


{: .note }
Base image 를 참조한 Docker image 에 대해 분석해보고자 한다.

## Dockerfile 정의

먼저 아래와 같이 3개의 dockerfile 이 있다고 가정하자. 각각은 상위 도커 이미지를 참조한다.

### Parent Image 

```sh
FROM openjdk:11
  
WORKDIR /app

COPY ./springVueStarter-0.0.1-SNAPSHOT.jar .

ENTRYPOINT ["java","-jar","-Dserver.port=8080","springVueStarter-0.0.1-SNAPSHOT.jar"]
```

{: .highlight }
ImageName - spring-vue-image:131 

### 1st Child Image 

```sh
FROM dmjgr11.jfrog.io/spring-vue-starter-repo/spring-vue-image:131
  
ENV MY_NAME="1st child"
``` 

{: .highlight }
ImageName - spring-vue-image-child:133


### 2nd Child Image


```sh
FROM dmjgr11.jfrog.io/spring-vue-starter-repo/spring-vue-image-child:133
  
ENV MY_NAME="2nd child"
```

{: .highlight }
ImageName - pring-vue-image-child-child:135

## Docker History from Jfrog

### Parent Image 

```bash
GET https://dmjgr11.jfrog.io/artifactory/api/docker/spring-vue-starter-repo/v2/spring-vue-image/manifests/131


{
  "schemaVersion" : 1,
  "name" : "spring-vue-image",
  "tag" : "131",
  "architecture" : "amd64",
  "fsLayers" : [ {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:a199ac3f84ab266ff997cadbc58b7e213a365ef8fbdf5e18635e974e00af1e10"
  }, {
    "blobSum" : "sha256:b170492a3b5fd338b7dd87b44605150d06889f1e916029a3d499f9ae6dcf631a"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:db38d58ec8ab4111b072f6700f978a51985acd252aabce3be377f25162e68301"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:66223a710990a0ae7162aeed80417d30303afa3f24aafa57aa30348725e2230b"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:d85151f15b6683b98f21c3827ac545188b1849efb14a1049710ebc4692de3dd5"
  }, {
    "blobSum" : "sha256:9daef329d35093868ef75ac8b7c6eb407fa53abbcb3a264c218c2ec7bca716e6"
  }, {
    "blobSum" : "sha256:2068746827ec1b043b571e4788693eab7e9b2a95301176512791f8c317a2816a"
  }, {
    "blobSum" : "sha256:d9d4b9b6e964657da49910b495173d6c4f0d9bc47b3b44273cf82fd32723d165"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:001c52e26ad57e3b25b439ee0052f6692e5c0f2d5d982a00a8819ace5e521452"
  } ],
  "history" : [ {
    "v1Compatibility" : "{\"container\":\"47d70ad47375880b2a3826e42a84b11f3b83b923c452fbd1c3ec3b179196046d\",\"parent\":\"f80ece92c78d6dfdb0eb44e1b1276e14097387e082793df3113da76bead22840\",\"throwaway\":true,\"os\":\"linux\",\"created\":\"2023-11-26T01:26:52.991452547Z\",\"container_config\":{\"Hostname\":\"47d70ad47375\",\"Domainname\":\"\",\"User\":\"\",\"AttachStdin\":false,\"AttachStdout\":false,\"AttachStderr\":false,\"Tty\":false,\"OpenStdin\":false,\"StdinOnce\":false,\"Env\":[\"PATH=/usr/local/openjdk-11/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin\",\"JAVA_HOME=/usr/local/openjdk-11\",\"LANG=C.UTF-8\",\"JAVA_VERSION=11.0.16\"],\"Cmd\":[\"/bin/sh\",\"-c\",\"#(nop) \",\"ENTRYPOINT [\\\"java\\\" \\\"-jar\\\" \\\"-Dserver.port=8080\\\" \\\"springVueStarter-0.0.1-SNAPSHOT.jar\\\"]\"],\"Image\":\"sha256:1062200af8349a3537db4c1c34ff9077952e801c37bdc8f6c376c3f1d5ce5070\",\"Volumes\":null,\"WorkingDir\":\"/app\",\"Entrypoint\":[\"java\",\"-jar\",\"-Dserver.port=8080\",\"springVueStarter-0.0.1-SNAPSHOT.jar\"],\"OnBuild\":null,\"Labels\":{}},\"id\":\"d7e3bf73aad4ebc99ea36e2d81edb5c5e4e1886b7a1b6f95ecdd7dbb35d1e8dc\",\"config\":{\"Hostname\":\"\",\"Domainname\":\"\",\"User\":\"\",\"AttachStdin\":false,\"AttachStdout\":false,\"AttachStderr\":false,\"Tty\":false,\"OpenStdin\":false,\"StdinOnce\":false,\"Env\":[\"PATH=/usr/local/openjdk-11/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin\",\"JAVA_HOME=/usr/local/openjdk-11\",\"LANG=C.UTF-8\",\"JAVA_VERSION=11.0.16\"],\"Cmd\":null,\"Image\":\"sha256:1062200af8349a3537db4c1c34ff9077952e801c37bdc8f6c376c3f1d5ce5070\",\"Volumes\":null,\"WorkingDir\":\"/app\",\"Entrypoint\":[\"java\",\"-jar\",\"-Dserver.port=8080\",\"springVueStarter-0.0.1-SNAPSHOT.jar\"],\"OnBuild\":null,\"Labels\":null},\"docker_version\":\"24.0.5\",\"architecture\":\"amd64\"}"
  }, {
    "v1Compatibility" : "{\"id\":\"f80ece92c78d6dfdb0eb44e1b1276e14097387e082793df3113da76bead22840\",\"parent\":\"2c2f87c4aa2168e88d936b22d2df087b68f3128254ab3116323c16f1c436d47d\",\"comment\":null,\"created\":\"2023-11-26T01:26:52.305625088Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop) COPY file:8bbc62b0cb34d4df4ef293b81a5de6ebd9b1bb544088353398300187f13b041e in . \"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"2c2f87c4aa2168e88d936b22d2df087b68f3128254ab3116323c16f1c436d47d\",\"parent\":\"2557f5457bf26fc998dcefe03e58db922c0c7b9ab5c0dd5e4ca5affcce1cc294\",\"comment\":null,\"created\":\"2023-11-26T01:26:51.923977723Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop) WORKDIR /app\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"2557f5457bf26fc998dcefe03e58db922c0c7b9ab5c0dd5e4ca5affcce1cc294\",\"parent\":\"d6eed9a77235113bbd26dd9233d8954696372897202a32132ebe135a348666af\",\"comment\":null,\"created\":\"2022-08-02T05:52:12.355386837Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  CMD [\\\"jshell\\\"]\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"d6eed9a77235113bbd26dd9233d8954696372897202a32132ebe135a348666af\",\"parent\":\"7bd9969799ce17a0556569dd526d8ab7ad494bdf4217961f231eb2c9850d0b71\",\"comment\":null,\"created\":\"2022-08-02T05:52:11.750144107Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c set -eux; \\t\\tarch=\\\"$(dpkg --print-architecture)\\\"; \\tcase \\\"$arch\\\" in \\t\\t'amd64') \\t\\t\\tdownloadUrl='https://github.com/AdoptOpenJDK/openjdk11-upstream-binaries/releases/download/jdk-11.0.16%2B8/OpenJDK11U-jdk_x64_linux_11.0.16_8.tar.gz'; \\t\\t\\t;; \\t\\t'arm64') \\t\\t\\tdownloadUrl='https://github.com/AdoptOpenJDK/openjdk11-upstream-binaries/releases/download/jdk-11.0.16%2B8/OpenJDK11U-jdk_aarch64_linux_11.0.16_8.tar.gz'; \\t\\t\\t;; \\t\\t*) echo >&2 \\\"error: unsupported architecture: '$arch'\\\"; exit 1 ;; \\tesac; \\t\\twget --progress=dot:giga -O openjdk.tgz \\\"$downloadUrl\\\"; \\twget --progress=dot:giga -O openjdk.tgz.asc \\\"$downloadUrl.sign\\\"; \\t\\texport GNUPGHOME=\\\"$(mktemp -d)\\\"; \\tgpg --batch --keyserver keyserver.ubuntu.com --recv-keys EAC843EBD3EFDB98CC772FADA5CD6035332FA671; \\tgpg --batch --keyserver keyserver.ubuntu.com --keyserver-options no-self-sigs-only --recv-keys CA5F11C6CE22644D42C6AC4492EF8D39DC13168F; \\tgpg --batch --list-sigs --keyid-format 0xLONG CA5F11C6CE22644D42C6AC4492EF8D39DC13168F \\t\\t| tee /dev/stderr \\t\\t| grep '0xA5CD6035332FA671' \\t\\t| grep 'Andrew Haley'; \\tgpg --batch --verify openjdk.tgz.asc openjdk.tgz; \\tgpgconf --kill all; \\trm -rf \\\"$GNUPGHOME\\\"; \\t\\tmkdir -p \\\"$JAVA_HOME\\\"; \\ttar --extract \\t\\t--file openjdk.tgz \\t\\t--directory \\\"$JAVA_HOME\\\" \\t\\t--strip-components 1 \\t\\t--no-same-owner \\t; \\trm openjdk.tgz*; \\t\\t{ \\t\\techo '#!/usr/bin/env bash'; \\t\\techo 'set -Eeuo pipefail'; \\t\\techo 'trust extract --overwrite --format=java-cacerts --filter=ca-anchors --purpose=server-auth \\\"$JAVA_HOME/lib/security/cacerts\\\"'; \\t} > /etc/ca-certificates/update.d/docker-openjdk; \\tchmod +x /etc/ca-certificates/update.d/docker-openjdk; \\t/etc/ca-certificates/update.d/docker-openjdk; \\t\\tfind \\\"$JAVA_HOME/lib\\\" -name '*.so' -exec dirname '{}' ';' | sort -u > /etc/ld.so.conf.d/docker-openjdk.conf; \\tldconfig; \\t\\tjava -Xshare:dump; \\t\\tfileEncoding=\\\"$(echo 'System.out.println(System.getProperty(\\\"file.encoding\\\"))' | jshell -s -)\\\"; [ \\\"$fileEncoding\\\" = 'UTF-8' ]; rm -rf ~/.java; \\tjavac --version; \\tjava --version\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"7bd9969799ce17a0556569dd526d8ab7ad494bdf4217961f231eb2c9850d0b71\",\"parent\":\"150bd83bc51ff45895d89bc2a0e22b2b6d2640db9d25e8c8e5e99d5ef0ec7623\",\"comment\":null,\"created\":\"2022-08-02T05:51:55.698900058Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  ENV JAVA_VERSION=11.0.16\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"150bd83bc51ff45895d89bc2a0e22b2b6d2640db9d25e8c8e5e99d5ef0ec7623\",\"parent\":\"76ae3db4cadf676eda2bac904f9b08c4620c0edc69bada1df7f6395141e133e7\",\"comment\":null,\"created\":\"2022-08-02T05:51:55.605571697Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  ENV LANG=C.UTF-8\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"76ae3db4cadf676eda2bac904f9b08c4620c0edc69bada1df7f6395141e133e7\",\"parent\":\"4f704e3c8d62be564fab428d5f824d423888d17f29bb1a0495b321e0954792a0\",\"comment\":null,\"created\":\"2022-08-02T05:51:55.511643922Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  ENV PATH=/usr/local/openjdk-11/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"4f704e3c8d62be564fab428d5f824d423888d17f29bb1a0495b321e0954792a0\",\"parent\":\"6ba4d4d4710a6729c268a7ffbac3f8d6e138975207c5dcff733b82797fa2a9fc\",\"comment\":null,\"created\":\"2022-08-02T05:51:55.419053573Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c { echo '#/bin/sh'; echo 'echo \\\"$JAVA_HOME\\\"'; } > /usr/local/bin/docker-java-home && chmod +x /usr/local/bin/docker-java-home && [ \\\"$JAVA_HOME\\\" = \\\"$(docker-java-home)\\\" ] # backwards compatibility\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"6ba4d4d4710a6729c268a7ffbac3f8d6e138975207c5dcff733b82797fa2a9fc\",\"parent\":\"80aae24464862c379e21619d01e692826c9b569555f77d13cdbb817917097635\",\"comment\":null,\"created\":\"2022-08-02T05:51:54.830162485Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  ENV JAVA_HOME=/usr/local/openjdk-11\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"80aae24464862c379e21619d01e692826c9b569555f77d13cdbb817917097635\",\"parent\":\"16bf8653bb166af978133e007dc3ddedfa0088680a3420bafe56091030653d53\",\"comment\":null,\"created\":\"2022-08-02T05:51:54.691790584Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c set -eux; \\tapt-get update; \\tapt-get install -y --no-install-recommends \\t\\tbzip2 \\t\\tunzip \\t\\txz-utils \\t\\t\\t\\tfontconfig libfreetype6 \\t\\t\\t\\tca-certificates p11-kit \\t; \\trm -rf /var/lib/apt/lists/*\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"16bf8653bb166af978133e007dc3ddedfa0088680a3420bafe56091030653d53\",\"parent\":\"76bd2f568bfd26b10c46e1553a12f15fdfd2f79bc650d3d0acee77414d770dc4\",\"comment\":null,\"created\":\"2022-08-02T01:47:17.502915867Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c apt-get update && apt-get install -y --no-install-recommends \\t\\tgit \\t\\tmercurial \\t\\topenssh-client \\t\\tsubversion \\t\\t\\t\\tprocps \\t&& rm -rf /var/lib/apt/lists/*\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"76bd2f568bfd26b10c46e1553a12f15fdfd2f79bc650d3d0acee77414d770dc4\",\"parent\":\"158fd1860dc8ebca5c9aaf23053db479b21088720e82ad6651aee3448423f68b\",\"comment\":null,\"created\":\"2022-08-02T01:47:01.803843664Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c set -ex; \\tif ! command -v gpg > /dev/null; then \\t\\tapt-get update; \\t\\tapt-get install -y --no-install-recommends \\t\\t\\tgnupg \\t\\t\\tdirmngr \\t\\t; \\t\\trm -rf /var/lib/apt/lists/*; \\tfi\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"158fd1860dc8ebca5c9aaf23053db479b21088720e82ad6651aee3448423f68b\",\"parent\":\"85c0165b7b71b79d6ec1dc274c3f97ebf29bcb20a0bf110e4ac049db2a652913\",\"comment\":null,\"created\":\"2022-08-02T01:46:56.05932783Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c set -eux; \\tapt-get update; \\tapt-get install -y --no-install-recommends \\t\\tca-certificates \\t\\tcurl \\t\\tnetbase \\t\\twget \\t; \\trm -rf /var/lib/apt/lists/*\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"85c0165b7b71b79d6ec1dc274c3f97ebf29bcb20a0bf110e4ac049db2a652913\",\"parent\":\"d25d18af6e91fc80186a2cd6a64a2d77a56026d91d18d820965be16be01de88f\",\"comment\":null,\"created\":\"2022-08-02T01:19:54.87196988Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  CMD [\\\"bash\\\"]\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"d25d18af6e91fc80186a2cd6a64a2d77a56026d91d18d820965be16be01de88f\",\"parent\":\"\",\"comment\":null,\"created\":\"2022-08-02T01:19:54.384822699Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop) ADD file:d0f758e50c654c225f6c7f03e8a579efc9106437051573bdbae5e63b1c4b2c1f in / \"]}}"
  } ],
  "signatures" : [ {
    "header" : {
      "jwk" : {
        "crv" : "P-256",
        "kty" : "EC",
        "x" : "UUFxY1AB5-UxJXJGCCSfwS4PWAokG0KnKv2lVfPKxtw",
        "y" : "teCh4wOjbZmIYNll5rEpZlsfim1rMgwZn0_S4Urm-9A"
      },
      "alg" : "ES256"
    },
    "signature" : "E89ziY9PAUxb8QKjfWpt7gKADa9wv_1YRAYy1-xwvfCSxW23Q_zLfnFzlkbXi1BjnOiimSrqcj0yzAJGDKsWbg",
    "protected" : "eyJmb3JtYXRMZW5ndGgiOjExOTU0LCJmb3JtYXRUYWlsIjoiQ24wIiwidGltZSI6IjIwMjMtMTEtMjZUMzoxMjowNCswMDAwIn0"
  } ]
}
```

### 1st Child Image 

```bash
GET https://dmjgr11.jfrog.io/artifactory/api/docker/spring-vue-starter-repo/v2/spring-vue-image-child/manifests/133

{
  "schemaVersion" : 1,
  "name" : "spring-vue-image-child",
  "tag" : "133",
  "architecture" : "amd64",
  "fsLayers" : [ {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:a199ac3f84ab266ff997cadbc58b7e213a365ef8fbdf5e18635e974e00af1e10"
  }, {
    "blobSum" : "sha256:b170492a3b5fd338b7dd87b44605150d06889f1e916029a3d499f9ae6dcf631a"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:db38d58ec8ab4111b072f6700f978a51985acd252aabce3be377f25162e68301"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:66223a710990a0ae7162aeed80417d30303afa3f24aafa57aa30348725e2230b"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:d85151f15b6683b98f21c3827ac545188b1849efb14a1049710ebc4692de3dd5"
  }, {
    "blobSum" : "sha256:9daef329d35093868ef75ac8b7c6eb407fa53abbcb3a264c218c2ec7bca716e6"
  }, {
    "blobSum" : "sha256:2068746827ec1b043b571e4788693eab7e9b2a95301176512791f8c317a2816a"
  }, {
    "blobSum" : "sha256:d9d4b9b6e964657da49910b495173d6c4f0d9bc47b3b44273cf82fd32723d165"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:001c52e26ad57e3b25b439ee0052f6692e5c0f2d5d982a00a8819ace5e521452"
  } ],
  "history" : [ {
    "v1Compatibility" : "{\"container\":\"ee6731091b5195057a476420e76d36ce9c6ed4833a9ee87720108bf5ec57a7e4\",\"parent\":\"1e11cd3f23273e0beff6fb679bf83135304a8bea318d756662d822536f0305d5\",\"throwaway\":true,\"os\":\"linux\",\"created\":\"2023-11-26T01:32:43.64663755Z\",\"container_config\":{\"Hostname\":\"ee6731091b51\",\"Domainname\":\"\",\"User\":\"\",\"AttachStdin\":false,\"AttachStdout\":false,\"AttachStderr\":false,\"Tty\":false,\"OpenStdin\":false,\"StdinOnce\":false,\"Env\":[\"PATH=/usr/local/openjdk-11/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin\",\"JAVA_HOME=/usr/local/openjdk-11\",\"LANG=C.UTF-8\",\"JAVA_VERSION=11.0.16\",\"MY_NAME=1st child\"],\"Cmd\":[\"/bin/sh\",\"-c\",\"#(nop) \",\"ENV MY_NAME=1st child\"],\"Image\":\"sha256:0d46410308777973d51d3ffc0639b7670c04d34968650f56f6ed5304c5359c9d\",\"Volumes\":null,\"WorkingDir\":\"/app\",\"Entrypoint\":[\"java\",\"-jar\",\"-Dserver.port=8080\",\"springVueStarter-0.0.1-SNAPSHOT.jar\"],\"OnBuild\":null,\"Labels\":{}},\"id\":\"b1e9523a099d2679871dc64c41bba17c773b3ebaa5e8af1069b65667ad73b0b8\",\"config\":{\"Hostname\":\"\",\"Domainname\":\"\",\"User\":\"\",\"AttachStdin\":false,\"AttachStdout\":false,\"AttachStderr\":false,\"Tty\":false,\"OpenStdin\":false,\"StdinOnce\":false,\"Env\":[\"PATH=/usr/local/openjdk-11/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin\",\"JAVA_HOME=/usr/local/openjdk-11\",\"LANG=C.UTF-8\",\"JAVA_VERSION=11.0.16\",\"MY_NAME=1st child\"],\"Cmd\":null,\"Image\":\"sha256:0d46410308777973d51d3ffc0639b7670c04d34968650f56f6ed5304c5359c9d\",\"Volumes\":null,\"WorkingDir\":\"/app\",\"Entrypoint\":[\"java\",\"-jar\",\"-Dserver.port=8080\",\"springVueStarter-0.0.1-SNAPSHOT.jar\"],\"OnBuild\":null,\"Labels\":null},\"docker_version\":\"24.0.5\",\"architecture\":\"amd64\"}"
  }, {
    "v1Compatibility" : "{\"id\":\"1e11cd3f23273e0beff6fb679bf83135304a8bea318d756662d822536f0305d5\",\"parent\":\"f80ece92c78d6dfdb0eb44e1b1276e14097387e082793df3113da76bead22840\",\"comment\":null,\"created\":\"2023-11-26T01:26:52.991452547Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  ENTRYPOINT [\\\"java\\\" \\\"-jar\\\" \\\"-Dserver.port=8080\\\" \\\"springVueStarter-0.0.1-SNAPSHOT.jar\\\"]\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"f80ece92c78d6dfdb0eb44e1b1276e14097387e082793df3113da76bead22840\",\"parent\":\"2c2f87c4aa2168e88d936b22d2df087b68f3128254ab3116323c16f1c436d47d\",\"comment\":null,\"created\":\"2023-11-26T01:26:52.305625088Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop) COPY file:8bbc62b0cb34d4df4ef293b81a5de6ebd9b1bb544088353398300187f13b041e in . \"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"2c2f87c4aa2168e88d936b22d2df087b68f3128254ab3116323c16f1c436d47d\",\"parent\":\"2557f5457bf26fc998dcefe03e58db922c0c7b9ab5c0dd5e4ca5affcce1cc294\",\"comment\":null,\"created\":\"2023-11-26T01:26:51.923977723Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop) WORKDIR /app\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"2557f5457bf26fc998dcefe03e58db922c0c7b9ab5c0dd5e4ca5affcce1cc294\",\"parent\":\"d6eed9a77235113bbd26dd9233d8954696372897202a32132ebe135a348666af\",\"comment\":null,\"created\":\"2022-08-02T05:52:12.355386837Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  CMD [\\\"jshell\\\"]\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"d6eed9a77235113bbd26dd9233d8954696372897202a32132ebe135a348666af\",\"parent\":\"7bd9969799ce17a0556569dd526d8ab7ad494bdf4217961f231eb2c9850d0b71\",\"comment\":null,\"created\":\"2022-08-02T05:52:11.750144107Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c set -eux; \\t\\tarch=\\\"$(dpkg --print-architecture)\\\"; \\tcase \\\"$arch\\\" in \\t\\t'amd64') \\t\\t\\tdownloadUrl='https://github.com/AdoptOpenJDK/openjdk11-upstream-binaries/releases/download/jdk-11.0.16%2B8/OpenJDK11U-jdk_x64_linux_11.0.16_8.tar.gz'; \\t\\t\\t;; \\t\\t'arm64') \\t\\t\\tdownloadUrl='https://github.com/AdoptOpenJDK/openjdk11-upstream-binaries/releases/download/jdk-11.0.16%2B8/OpenJDK11U-jdk_aarch64_linux_11.0.16_8.tar.gz'; \\t\\t\\t;; \\t\\t*) echo >&2 \\\"error: unsupported architecture: '$arch'\\\"; exit 1 ;; \\tesac; \\t\\twget --progress=dot:giga -O openjdk.tgz \\\"$downloadUrl\\\"; \\twget --progress=dot:giga -O openjdk.tgz.asc \\\"$downloadUrl.sign\\\"; \\t\\texport GNUPGHOME=\\\"$(mktemp -d)\\\"; \\tgpg --batch --keyserver keyserver.ubuntu.com --recv-keys EAC843EBD3EFDB98CC772FADA5CD6035332FA671; \\tgpg --batch --keyserver keyserver.ubuntu.com --keyserver-options no-self-sigs-only --recv-keys CA5F11C6CE22644D42C6AC4492EF8D39DC13168F; \\tgpg --batch --list-sigs --keyid-format 0xLONG CA5F11C6CE22644D42C6AC4492EF8D39DC13168F \\t\\t| tee /dev/stderr \\t\\t| grep '0xA5CD6035332FA671' \\t\\t| grep 'Andrew Haley'; \\tgpg --batch --verify openjdk.tgz.asc openjdk.tgz; \\tgpgconf --kill all; \\trm -rf \\\"$GNUPGHOME\\\"; \\t\\tmkdir -p \\\"$JAVA_HOME\\\"; \\ttar --extract \\t\\t--file openjdk.tgz \\t\\t--directory \\\"$JAVA_HOME\\\" \\t\\t--strip-components 1 \\t\\t--no-same-owner \\t; \\trm openjdk.tgz*; \\t\\t{ \\t\\techo '#!/usr/bin/env bash'; \\t\\techo 'set -Eeuo pipefail'; \\t\\techo 'trust extract --overwrite --format=java-cacerts --filter=ca-anchors --purpose=server-auth \\\"$JAVA_HOME/lib/security/cacerts\\\"'; \\t} > /etc/ca-certificates/update.d/docker-openjdk; \\tchmod +x /etc/ca-certificates/update.d/docker-openjdk; \\t/etc/ca-certificates/update.d/docker-openjdk; \\t\\tfind \\\"$JAVA_HOME/lib\\\" -name '*.so' -exec dirname '{}' ';' | sort -u > /etc/ld.so.conf.d/docker-openjdk.conf; \\tldconfig; \\t\\tjava -Xshare:dump; \\t\\tfileEncoding=\\\"$(echo 'System.out.println(System.getProperty(\\\"file.encoding\\\"))' | jshell -s -)\\\"; [ \\\"$fileEncoding\\\" = 'UTF-8' ]; rm -rf ~/.java; \\tjavac --version; \\tjava --version\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"7bd9969799ce17a0556569dd526d8ab7ad494bdf4217961f231eb2c9850d0b71\",\"parent\":\"150bd83bc51ff45895d89bc2a0e22b2b6d2640db9d25e8c8e5e99d5ef0ec7623\",\"comment\":null,\"created\":\"2022-08-02T05:51:55.698900058Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  ENV JAVA_VERSION=11.0.16\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"150bd83bc51ff45895d89bc2a0e22b2b6d2640db9d25e8c8e5e99d5ef0ec7623\",\"parent\":\"76ae3db4cadf676eda2bac904f9b08c4620c0edc69bada1df7f6395141e133e7\",\"comment\":null,\"created\":\"2022-08-02T05:51:55.605571697Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  ENV LANG=C.UTF-8\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"76ae3db4cadf676eda2bac904f9b08c4620c0edc69bada1df7f6395141e133e7\",\"parent\":\"4f704e3c8d62be564fab428d5f824d423888d17f29bb1a0495b321e0954792a0\",\"comment\":null,\"created\":\"2022-08-02T05:51:55.511643922Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  ENV PATH=/usr/local/openjdk-11/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"4f704e3c8d62be564fab428d5f824d423888d17f29bb1a0495b321e0954792a0\",\"parent\":\"6ba4d4d4710a6729c268a7ffbac3f8d6e138975207c5dcff733b82797fa2a9fc\",\"comment\":null,\"created\":\"2022-08-02T05:51:55.419053573Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c { echo '#/bin/sh'; echo 'echo \\\"$JAVA_HOME\\\"'; } > /usr/local/bin/docker-java-home && chmod +x /usr/local/bin/docker-java-home && [ \\\"$JAVA_HOME\\\" = \\\"$(docker-java-home)\\\" ] # backwards compatibility\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"6ba4d4d4710a6729c268a7ffbac3f8d6e138975207c5dcff733b82797fa2a9fc\",\"parent\":\"80aae24464862c379e21619d01e692826c9b569555f77d13cdbb817917097635\",\"comment\":null,\"created\":\"2022-08-02T05:51:54.830162485Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  ENV JAVA_HOME=/usr/local/openjdk-11\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"80aae24464862c379e21619d01e692826c9b569555f77d13cdbb817917097635\",\"parent\":\"16bf8653bb166af978133e007dc3ddedfa0088680a3420bafe56091030653d53\",\"comment\":null,\"created\":\"2022-08-02T05:51:54.691790584Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c set -eux; \\tapt-get update; \\tapt-get install -y --no-install-recommends \\t\\tbzip2 \\t\\tunzip \\t\\txz-utils \\t\\t\\t\\tfontconfig libfreetype6 \\t\\t\\t\\tca-certificates p11-kit \\t; \\trm -rf /var/lib/apt/lists/*\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"16bf8653bb166af978133e007dc3ddedfa0088680a3420bafe56091030653d53\",\"parent\":\"76bd2f568bfd26b10c46e1553a12f15fdfd2f79bc650d3d0acee77414d770dc4\",\"comment\":null,\"created\":\"2022-08-02T01:47:17.502915867Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c apt-get update && apt-get install -y --no-install-recommends \\t\\tgit \\t\\tmercurial \\t\\topenssh-client \\t\\tsubversion \\t\\t\\t\\tprocps \\t&& rm -rf /var/lib/apt/lists/*\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"76bd2f568bfd26b10c46e1553a12f15fdfd2f79bc650d3d0acee77414d770dc4\",\"parent\":\"158fd1860dc8ebca5c9aaf23053db479b21088720e82ad6651aee3448423f68b\",\"comment\":null,\"created\":\"2022-08-02T01:47:01.803843664Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c set -ex; \\tif ! command -v gpg > /dev/null; then \\t\\tapt-get update; \\t\\tapt-get install -y --no-install-recommends \\t\\t\\tgnupg \\t\\t\\tdirmngr \\t\\t; \\t\\trm -rf /var/lib/apt/lists/*; \\tfi\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"158fd1860dc8ebca5c9aaf23053db479b21088720e82ad6651aee3448423f68b\",\"parent\":\"85c0165b7b71b79d6ec1dc274c3f97ebf29bcb20a0bf110e4ac049db2a652913\",\"comment\":null,\"created\":\"2022-08-02T01:46:56.05932783Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c set -eux; \\tapt-get update; \\tapt-get install -y --no-install-recommends \\t\\tca-certificates \\t\\tcurl \\t\\tnetbase \\t\\twget \\t; \\trm -rf /var/lib/apt/lists/*\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"85c0165b7b71b79d6ec1dc274c3f97ebf29bcb20a0bf110e4ac049db2a652913\",\"parent\":\"d25d18af6e91fc80186a2cd6a64a2d77a56026d91d18d820965be16be01de88f\",\"comment\":null,\"created\":\"2022-08-02T01:19:54.87196988Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  CMD [\\\"bash\\\"]\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"d25d18af6e91fc80186a2cd6a64a2d77a56026d91d18d820965be16be01de88f\",\"parent\":\"\",\"comment\":null,\"created\":\"2022-08-02T01:19:54.384822699Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop) ADD file:d0f758e50c654c225f6c7f03e8a579efc9106437051573bdbae5e63b1c4b2c1f in / \"]}}"
  } ],
  "signatures" : [ {
    "header" : {
      "jwk" : {
        "crv" : "P-256",
        "kty" : "EC",
        "x" : "_98faizl7eVyI8um62-pHZQil8c-bbSNyXZltly8cBI",
        "y" : "YVQARWoIRYO_mLRWg3sGtaUa1i3j5_6ZEiP_ogRozJg"
      },
      "alg" : "ES256"
    },
    "signature" : "Ycm9LQgRXLL2IcGrxd8e7kUO7NEqmUgH98PnM6Mf0HLVH2_1RJsspk31ka1r0w6fRMW8iCpRDrhOowyM35l0mg",
    "protected" : "eyJmb3JtYXRMZW5ndGgiOjEyNDUyLCJmb3JtYXRUYWlsIjoiQ24wIiwidGltZSI6IjIwMjMtMTEtMjZUMzoxNTowMSswMDAwIn0"
  } ]
}
```

### 2nd Child Image 

```bash
GET https://dmjgr11.jfrog.io/artifactory/api/docker/spring-vue-starter-repo/v2/spring-vue-image-child-child/manifests/135

{
  "schemaVersion" : 1,
  "name" : "spring-vue-image-child-child",
  "tag" : "135",
  "architecture" : "amd64",
  "fsLayers" : [ {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:a199ac3f84ab266ff997cadbc58b7e213a365ef8fbdf5e18635e974e00af1e10"
  }, {
    "blobSum" : "sha256:b170492a3b5fd338b7dd87b44605150d06889f1e916029a3d499f9ae6dcf631a"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:db38d58ec8ab4111b072f6700f978a51985acd252aabce3be377f25162e68301"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:66223a710990a0ae7162aeed80417d30303afa3f24aafa57aa30348725e2230b"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:d85151f15b6683b98f21c3827ac545188b1849efb14a1049710ebc4692de3dd5"
  }, {
    "blobSum" : "sha256:9daef329d35093868ef75ac8b7c6eb407fa53abbcb3a264c218c2ec7bca716e6"
  }, {
    "blobSum" : "sha256:2068746827ec1b043b571e4788693eab7e9b2a95301176512791f8c317a2816a"
  }, {
    "blobSum" : "sha256:d9d4b9b6e964657da49910b495173d6c4f0d9bc47b3b44273cf82fd32723d165"
  }, {
    "blobSum" : "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
  }, {
    "blobSum" : "sha256:001c52e26ad57e3b25b439ee0052f6692e5c0f2d5d982a00a8819ace5e521452"
  } ],
  "history" : [ {
    "v1Compatibility" : "{\"container\":\"333ce8ff4eb781e93aed598347203bb18f2aca1000eca99dd6515a99e6ac4265\",\"parent\":\"fca29ab85ad712b2f811746eaa12b0822eace2b468f9012ff21f415e98b776f2\",\"throwaway\":true,\"os\":\"linux\",\"created\":\"2023-11-26T01:36:46.845015764Z\",\"container_config\":{\"Hostname\":\"333ce8ff4eb7\",\"Domainname\":\"\",\"User\":\"\",\"AttachStdin\":false,\"AttachStdout\":false,\"AttachStderr\":false,\"Tty\":false,\"OpenStdin\":false,\"StdinOnce\":false,\"Env\":[\"PATH=/usr/local/openjdk-11/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin\",\"JAVA_HOME=/usr/local/openjdk-11\",\"LANG=C.UTF-8\",\"JAVA_VERSION=11.0.16\",\"MY_NAME=2nd child\"],\"Cmd\":[\"/bin/sh\",\"-c\",\"#(nop) \",\"ENV MY_NAME=2nd child\"],\"Image\":\"sha256:0e7358f4ed268aad54bdcd325d877e51c1b40dae9ae84cfc335057a51985b33e\",\"Volumes\":null,\"WorkingDir\":\"/app\",\"Entrypoint\":[\"java\",\"-jar\",\"-Dserver.port=8080\",\"springVueStarter-0.0.1-SNAPSHOT.jar\"],\"OnBuild\":null,\"Labels\":{}},\"id\":\"63d9b37bfe94c9a327ad9a32dbcfd3aebf2b56fb9ca74706acbcc08ac1f3a5ca\",\"config\":{\"Hostname\":\"\",\"Domainname\":\"\",\"User\":\"\",\"AttachStdin\":false,\"AttachStdout\":false,\"AttachStderr\":false,\"Tty\":false,\"OpenStdin\":false,\"StdinOnce\":false,\"Env\":[\"PATH=/usr/local/openjdk-11/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin\",\"JAVA_HOME=/usr/local/openjdk-11\",\"LANG=C.UTF-8\",\"JAVA_VERSION=11.0.16\",\"MY_NAME=2nd child\"],\"Cmd\":null,\"Image\":\"sha256:0e7358f4ed268aad54bdcd325d877e51c1b40dae9ae84cfc335057a51985b33e\",\"Volumes\":null,\"WorkingDir\":\"/app\",\"Entrypoint\":[\"java\",\"-jar\",\"-Dserver.port=8080\",\"springVueStarter-0.0.1-SNAPSHOT.jar\"],\"OnBuild\":null,\"Labels\":null},\"docker_version\":\"24.0.5\",\"architecture\":\"amd64\"}"
  }, {
    "v1Compatibility" : "{\"id\":\"fca29ab85ad712b2f811746eaa12b0822eace2b468f9012ff21f415e98b776f2\",\"parent\":\"1e11cd3f23273e0beff6fb679bf83135304a8bea318d756662d822536f0305d5\",\"comment\":null,\"created\":\"2023-11-26T01:32:43.64663755Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  ENV MY_NAME=1st child\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"1e11cd3f23273e0beff6fb679bf83135304a8bea318d756662d822536f0305d5\",\"parent\":\"f80ece92c78d6dfdb0eb44e1b1276e14097387e082793df3113da76bead22840\",\"comment\":null,\"created\":\"2023-11-26T01:26:52.991452547Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  ENTRYPOINT [\\\"java\\\" \\\"-jar\\\" \\\"-Dserver.port=8080\\\" \\\"springVueStarter-0.0.1-SNAPSHOT.jar\\\"]\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"f80ece92c78d6dfdb0eb44e1b1276e14097387e082793df3113da76bead22840\",\"parent\":\"2c2f87c4aa2168e88d936b22d2df087b68f3128254ab3116323c16f1c436d47d\",\"comment\":null,\"created\":\"2023-11-26T01:26:52.305625088Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop) COPY file:8bbc62b0cb34d4df4ef293b81a5de6ebd9b1bb544088353398300187f13b041e in . \"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"2c2f87c4aa2168e88d936b22d2df087b68f3128254ab3116323c16f1c436d47d\",\"parent\":\"2557f5457bf26fc998dcefe03e58db922c0c7b9ab5c0dd5e4ca5affcce1cc294\",\"comment\":null,\"created\":\"2023-11-26T01:26:51.923977723Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop) WORKDIR /app\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"2557f5457bf26fc998dcefe03e58db922c0c7b9ab5c0dd5e4ca5affcce1cc294\",\"parent\":\"d6eed9a77235113bbd26dd9233d8954696372897202a32132ebe135a348666af\",\"comment\":null,\"created\":\"2022-08-02T05:52:12.355386837Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  CMD [\\\"jshell\\\"]\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"d6eed9a77235113bbd26dd9233d8954696372897202a32132ebe135a348666af\",\"parent\":\"7bd9969799ce17a0556569dd526d8ab7ad494bdf4217961f231eb2c9850d0b71\",\"comment\":null,\"created\":\"2022-08-02T05:52:11.750144107Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c set -eux; \\t\\tarch=\\\"$(dpkg --print-architecture)\\\"; \\tcase \\\"$arch\\\" in \\t\\t'amd64') \\t\\t\\tdownloadUrl='https://github.com/AdoptOpenJDK/openjdk11-upstream-binaries/releases/download/jdk-11.0.16%2B8/OpenJDK11U-jdk_x64_linux_11.0.16_8.tar.gz'; \\t\\t\\t;; \\t\\t'arm64') \\t\\t\\tdownloadUrl='https://github.com/AdoptOpenJDK/openjdk11-upstream-binaries/releases/download/jdk-11.0.16%2B8/OpenJDK11U-jdk_aarch64_linux_11.0.16_8.tar.gz'; \\t\\t\\t;; \\t\\t*) echo >&2 \\\"error: unsupported architecture: '$arch'\\\"; exit 1 ;; \\tesac; \\t\\twget --progress=dot:giga -O openjdk.tgz \\\"$downloadUrl\\\"; \\twget --progress=dot:giga -O openjdk.tgz.asc \\\"$downloadUrl.sign\\\"; \\t\\texport GNUPGHOME=\\\"$(mktemp -d)\\\"; \\tgpg --batch --keyserver keyserver.ubuntu.com --recv-keys EAC843EBD3EFDB98CC772FADA5CD6035332FA671; \\tgpg --batch --keyserver keyserver.ubuntu.com --keyserver-options no-self-sigs-only --recv-keys CA5F11C6CE22644D42C6AC4492EF8D39DC13168F; \\tgpg --batch --list-sigs --keyid-format 0xLONG CA5F11C6CE22644D42C6AC4492EF8D39DC13168F \\t\\t| tee /dev/stderr \\t\\t| grep '0xA5CD6035332FA671' \\t\\t| grep 'Andrew Haley'; \\tgpg --batch --verify openjdk.tgz.asc openjdk.tgz; \\tgpgconf --kill all; \\trm -rf \\\"$GNUPGHOME\\\"; \\t\\tmkdir -p \\\"$JAVA_HOME\\\"; \\ttar --extract \\t\\t--file openjdk.tgz \\t\\t--directory \\\"$JAVA_HOME\\\" \\t\\t--strip-components 1 \\t\\t--no-same-owner \\t; \\trm openjdk.tgz*; \\t\\t{ \\t\\techo '#!/usr/bin/env bash'; \\t\\techo 'set -Eeuo pipefail'; \\t\\techo 'trust extract --overwrite --format=java-cacerts --filter=ca-anchors --purpose=server-auth \\\"$JAVA_HOME/lib/security/cacerts\\\"'; \\t} > /etc/ca-certificates/update.d/docker-openjdk; \\tchmod +x /etc/ca-certificates/update.d/docker-openjdk; \\t/etc/ca-certificates/update.d/docker-openjdk; \\t\\tfind \\\"$JAVA_HOME/lib\\\" -name '*.so' -exec dirname '{}' ';' | sort -u > /etc/ld.so.conf.d/docker-openjdk.conf; \\tldconfig; \\t\\tjava -Xshare:dump; \\t\\tfileEncoding=\\\"$(echo 'System.out.println(System.getProperty(\\\"file.encoding\\\"))' | jshell -s -)\\\"; [ \\\"$fileEncoding\\\" = 'UTF-8' ]; rm -rf ~/.java; \\tjavac --version; \\tjava --version\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"7bd9969799ce17a0556569dd526d8ab7ad494bdf4217961f231eb2c9850d0b71\",\"parent\":\"150bd83bc51ff45895d89bc2a0e22b2b6d2640db9d25e8c8e5e99d5ef0ec7623\",\"comment\":null,\"created\":\"2022-08-02T05:51:55.698900058Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  ENV JAVA_VERSION=11.0.16\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"150bd83bc51ff45895d89bc2a0e22b2b6d2640db9d25e8c8e5e99d5ef0ec7623\",\"parent\":\"76ae3db4cadf676eda2bac904f9b08c4620c0edc69bada1df7f6395141e133e7\",\"comment\":null,\"created\":\"2022-08-02T05:51:55.605571697Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  ENV LANG=C.UTF-8\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"76ae3db4cadf676eda2bac904f9b08c4620c0edc69bada1df7f6395141e133e7\",\"parent\":\"4f704e3c8d62be564fab428d5f824d423888d17f29bb1a0495b321e0954792a0\",\"comment\":null,\"created\":\"2022-08-02T05:51:55.511643922Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  ENV PATH=/usr/local/openjdk-11/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"4f704e3c8d62be564fab428d5f824d423888d17f29bb1a0495b321e0954792a0\",\"parent\":\"6ba4d4d4710a6729c268a7ffbac3f8d6e138975207c5dcff733b82797fa2a9fc\",\"comment\":null,\"created\":\"2022-08-02T05:51:55.419053573Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c { echo '#/bin/sh'; echo 'echo \\\"$JAVA_HOME\\\"'; } > /usr/local/bin/docker-java-home && chmod +x /usr/local/bin/docker-java-home && [ \\\"$JAVA_HOME\\\" = \\\"$(docker-java-home)\\\" ] # backwards compatibility\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"6ba4d4d4710a6729c268a7ffbac3f8d6e138975207c5dcff733b82797fa2a9fc\",\"parent\":\"80aae24464862c379e21619d01e692826c9b569555f77d13cdbb817917097635\",\"comment\":null,\"created\":\"2022-08-02T05:51:54.830162485Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  ENV JAVA_HOME=/usr/local/openjdk-11\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"80aae24464862c379e21619d01e692826c9b569555f77d13cdbb817917097635\",\"parent\":\"16bf8653bb166af978133e007dc3ddedfa0088680a3420bafe56091030653d53\",\"comment\":null,\"created\":\"2022-08-02T05:51:54.691790584Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c set -eux; \\tapt-get update; \\tapt-get install -y --no-install-recommends \\t\\tbzip2 \\t\\tunzip \\t\\txz-utils \\t\\t\\t\\tfontconfig libfreetype6 \\t\\t\\t\\tca-certificates p11-kit \\t; \\trm -rf /var/lib/apt/lists/*\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"16bf8653bb166af978133e007dc3ddedfa0088680a3420bafe56091030653d53\",\"parent\":\"76bd2f568bfd26b10c46e1553a12f15fdfd2f79bc650d3d0acee77414d770dc4\",\"comment\":null,\"created\":\"2022-08-02T01:47:17.502915867Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c apt-get update && apt-get install -y --no-install-recommends \\t\\tgit \\t\\tmercurial \\t\\topenssh-client \\t\\tsubversion \\t\\t\\t\\tprocps \\t&& rm -rf /var/lib/apt/lists/*\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"76bd2f568bfd26b10c46e1553a12f15fdfd2f79bc650d3d0acee77414d770dc4\",\"parent\":\"158fd1860dc8ebca5c9aaf23053db479b21088720e82ad6651aee3448423f68b\",\"comment\":null,\"created\":\"2022-08-02T01:47:01.803843664Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c set -ex; \\tif ! command -v gpg > /dev/null; then \\t\\tapt-get update; \\t\\tapt-get install -y --no-install-recommends \\t\\t\\tgnupg \\t\\t\\tdirmngr \\t\\t; \\t\\trm -rf /var/lib/apt/lists/*; \\tfi\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"158fd1860dc8ebca5c9aaf23053db479b21088720e82ad6651aee3448423f68b\",\"parent\":\"85c0165b7b71b79d6ec1dc274c3f97ebf29bcb20a0bf110e4ac049db2a652913\",\"comment\":null,\"created\":\"2022-08-02T01:46:56.05932783Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c set -eux; \\tapt-get update; \\tapt-get install -y --no-install-recommends \\t\\tca-certificates \\t\\tcurl \\t\\tnetbase \\t\\twget \\t; \\trm -rf /var/lib/apt/lists/*\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"85c0165b7b71b79d6ec1dc274c3f97ebf29bcb20a0bf110e4ac049db2a652913\",\"parent\":\"d25d18af6e91fc80186a2cd6a64a2d77a56026d91d18d820965be16be01de88f\",\"comment\":null,\"created\":\"2022-08-02T01:19:54.87196988Z\",\"throwaway\":true,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop)  CMD [\\\"bash\\\"]\"]}}"
  }, {
    "v1Compatibility" : "{\"id\":\"d25d18af6e91fc80186a2cd6a64a2d77a56026d91d18d820965be16be01de88f\",\"parent\":\"\",\"comment\":null,\"created\":\"2022-08-02T01:19:54.384822699Z\",\"throwaway\":false,\"container_config\":{\"Cmd\":[\"/bin/sh -c #(nop) ADD file:d0f758e50c654c225f6c7f03e8a579efc9106437051573bdbae5e63b1c4b2c1f in / \"]}}"
  } ],
  "signatures" : [ {
    "header" : {
      "jwk" : {
        "crv" : "P-256",
        "kty" : "EC",
        "x" : "ctV8MpzGS9SOX6kVVeBQ4JDdUMoGU1VsP_LBBE0GdmA",
        "y" : "sRisGzR3kiqp13dEVqSd3LAIfWqGJYQuOyQpmMfzNNI"
      },
      "alg" : "ES256"
    },
    "signature" : "G8Gh_YvTe4HvILpWA2yUkDMkxzQzOPI7JRMh9GgPFHSfl_DjQHzPSfxXRgPkfGxamAb-py36vImHJ7-rGZdQFQ",
    "protected" : "eyJmb3JtYXRMZW5ndGgiOjEyOTA3LCJmb3JtYXRUYWlsIjoiQ24wIiwidGltZSI6IjIwMjMtMTEtMjZUMzoxNzozMiswMDAwIn0"
  } ]
}
```

## "Image" 해쉬값 확인

### Parent Image 


```sh
=> \"Image\":\"sha256:1062200af8349a3537db4c1c34ff9077952e801c37bdc8f6c376c3f1d5ce5070\"

GET https://dmjgr11.jfrog.io/artifactory/api/search/checksum?sha256=1062200af8349a3537db4c1c34ff9077952e801c37bdc8f6c376c3f1d5ce5070

{
    "results": []
}
```


### 1st Child Image 


```sh
=> \"Image\":\"sha256:0d46410308777973d51d3ffc0639b7670c04d34968650f56f6ed5304c5359c9d\"
  
GET https://dmjgr11.jfrog.io/artifactory/api/search/checksum?sha256=0d46410308777973d51d3ffc0639b7670c04d34968650f56f6ed5304c5359c9d

{
    "results": [
        {
            "uri": "https://dmjgr11.jfrog.io/artifactory/api/storage/spring-vue-starter-repo/spring-vue-image/_uploads/sha256__0d46410308777973d51d3ffc0639b7670c04d34968650f56f6ed5304c5359c9d"
        },
        {
            "uri": "https://dmjgr11.jfrog.io/artifactory/api/storage/spring-vue-starter-repo/spring-vue-image/131/sha256__0d46410308777973d51d3ffc0639b7670c04d34968650f56f6ed5304c5359c9d"
        }
    ]
}
``` 


### 2nd Child Image 



```sh
=> \"Image\":\"sha256:0e7358f4ed268aad54bdcd325d877e51c1b40dae9ae84cfc335057a51985b33e\"

GET https://dmjgr11.jfrog.io/artifactory/api/search/checksum?sha256=0e7358f4ed268aad54bdcd325d877e51c1b40dae9ae84cfc335057a51985b33e
  
{
    "results": [
        {
            "uri": "https://dmjgr11.jfrog.io/artifactory/api/storage/spring-vue-starter-repo/spring-vue-image-child/_uploads/sha256__0e7358f4ed268aad54bdcd325d877e51c1b40dae9ae84cfc335057a51985b33e"
        },
        {
            "uri": "https://dmjgr11.jfrog.io/artifactory/api/storage/spring-vue-starter-repo/spring-vue-image-child/133/sha256__0e7358f4ed268aad54bdcd325d877e51c1b40dae9ae84cfc335057a51985b33e"
        }
    ]
}
```

