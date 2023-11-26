---
layout: default
title: Docker Image Layer
parent: Docker
grand_parent: Build & Package
permalink: /docs/buildpackage/docker-imagelayer/
nav_order: 30
---

# Docker Image Layer
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---

{: .highlight }
Docker Image 는 주로 dockerfile 빌드를 통해 생성되면, 이는 docker file 에서의 각 커맨드 별로 레이어가 쌓여서 생성되는 것이라면 이해하기 쉽다. 도커 Image Layer가 어떻게 쌓여 있는지 확인하기 위해 docker history 커맨드를 살펴보고자 샘플 예제를 기준으로 정리한다.

## Dockerfile

```sh
FROM openjdk:17
  
WORKDIR /app

COPY springVueStarter-0.0.1-SNAPSHOT.jar .

ENTRYPOINT ["java","-jar", "springVueStarter-0.0.1-SNAPSHOT.jar"]
```

## Jenkins 빌드 시 로그

```sh
Step 1/4 : FROM openjdk:17
 ---> 5e28ba2b4cdb
Step 2/4 : WORKDIR /app
 ---> Using cache
 ---> f0001b6226f4
Step 3/4 : COPY springVueStarter-0.0.1-SNAPSHOT.jar .
 ---> Using cache
 ---> 925cc8c298f9
Step 4/4 : ENTRYPOINT ["java","-jar", "springVueStarter-0.0.1-SNAPSHOT.jar"]
 ---> Using cache
 ---> 53ae2020bd8e
Successfully built 53ae2020bd8e
Successfully tagged user1/spring-vue-starter-repo:90
```

## layer 분석

### Image Layer on docker history command

```sh
user1@user1-500R5K-501R5K-500R5Q:~$ docker history --no-trunc user1/spring-vue-starter-repo:90
IMAGE                                                                     CREATED         CREATED BY                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           SIZE      COMMENT
sha256:53ae2020bd8e6bd80952db2ce7c7ffcf01d56c43bd1fd44c3dd918549d4eca85   3 weeks ago     /bin/sh -c #(nop)  ENTRYPOINT ["java" "-jar" "springVueStarter-0.0.1-SNAPSHOT.jar"]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  0B        
sha256:925cc8c298f96a1e65fb921a97d2070f07d0ee5d8fc2bf51a4eefa21f3d5801c   3 weeks ago     /bin/sh -c #(nop) COPY file:3708ed22897e6f3364d3a7685bf03434e941c9676059fc904b08fdc1e1c234d7 in .                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    18.2MB    
sha256:f0001b6226f42bf49196c6d2001ec429672f249af78dcae71112152b8cc21c8b   3 weeks ago     /bin/sh -c #(nop) WORKDIR /app                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       0B        
sha256:5e28ba2b4cdb3a7c3bd0ee2e635a5f6481682b77eabf8b51a17ea8bfe1c05697   19 months ago   /bin/sh -c #(nop)  CMD ["jshell"]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    0B        
<missing>                                                                 19 months ago   /bin/sh -c set -eux;   arch="$(objdump="$(command -v objdump)" && objdump --file-headers "$objdump" | awk -F '[:,]+[[:space:]]+' '$1 == "architecture" { print $2 }')";  case "$arch" in   'i386:x86-64')    downloadUrl='https://download.java.net/java/GA/jdk17.0.2/dfd4a8d0985749f896bed50d7138ee7f/8/GPL/openjdk-17.0.2_linux-x64_bin.tar.gz';    downloadSha256='0022753d0cceecacdd3a795dd4cea2bd7ffdf9dc06e22ffd1be98411742fbb44';    ;;   'aarch64')    downloadUrl='https://download.java.net/java/GA/jdk17.0.2/dfd4a8d0985749f896bed50d7138ee7f/8/GPL/openjdk-17.0.2_linux-aarch64_bin.tar.gz';    downloadSha256='13bfd976acf8803f862e82c7113fb0e9311ca5458b1decaef8a09ffd91119fa4';    ;;   *) echo >&2 "error: unsupported architecture: '$arch'"; exit 1 ;;  esac;   curl -fL -o openjdk.tgz "$downloadUrl";  echo "$downloadSha256 *openjdk.tgz" | sha256sum --strict --check -;   mkdir -p "$JAVA_HOME";  tar --extract   --file openjdk.tgz   --directory "$JAVA_HOME"   --strip-components 1   --no-same-owner  ;  rm openjdk.tgz*;   rm -rf "$JAVA_HOME/lib/security/cacerts";  ln -sT /etc/pki/ca-trust/extracted/java/cacerts "$JAVA_HOME/lib/security/cacerts";   ln -sfT "$JAVA_HOME" /usr/java/default;  ln -sfT "$JAVA_HOME" /usr/java/latest;  for bin in "$JAVA_HOME/bin/"*; do   base="$(basename "$bin")";   [ ! -e "/usr/bin/$base" ];   alternatives --install "/usr/bin/$base" "$base" "$bin" 20000;  done;   java -Xshare:dump;   fileEncoding="$(echo 'System.out.println(System.getProperty("file.encoding"))' | jshell -s -)"; [ "$fileEncoding" = 'UTF-8' ]; rm -rf ~/.java;  javac --version;  java --version   321MB     
<missing>                                                                 19 months ago   /bin/sh -c #(nop)  ENV JAVA_VERSION=17.0.2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           0B        
<missing>                                                                 19 months ago   /bin/sh -c #(nop)  ENV LANG=C.UTF-8                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  0B        
<missing>                                                                 19 months ago   /bin/sh -c #(nop)  ENV PATH=/usr/java/openjdk-17/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    0B        
<missing>                                                                 19 months ago   /bin/sh -c #(nop)  ENV JAVA_HOME=/usr/java/openjdk-17                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                0B        
<missing>                                                                 19 months ago   /bin/sh -c set -eux;  microdnf install   gzip   tar     binutils   freetype fontconfig  ;  microdnf clean all                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        39.7MB    
<missing>                                                                 19 months ago   /bin/sh -c #(nop)  CMD ["/bin/bash"]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 0B        
<missing>                                                                 19 months ago   /bin/sh -c #(nop) ADD file:9893213a9ea238f53ac68d87a3cf2f05d86763688392e5ddb6a2c9b60d3550a6 in /                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     111MB 

user1@user1-500R5K-501R5K-500R5Q:~$ docker history  user1/spring-vue-starter-repo:90
IMAGE          CREATED         CREATED BY                                      SIZE      COMMENT
53ae2020bd8e   3 weeks ago     /bin/sh -c #(nop)  ENTRYPOINT ["java" "-jar"…   0B        
925cc8c298f9   3 weeks ago     /bin/sh -c #(nop) COPY file:3708ed22897e6f33…   18.2MB    
f0001b6226f4   3 weeks ago     /bin/sh -c #(nop) WORKDIR /app                  0B        
5e28ba2b4cdb   19 months ago   /bin/sh -c #(nop)  CMD ["jshell"]               0B        
<missing>      19 months ago   /bin/sh -c set -eux;   arch="$(objdump="$(co…   321MB     
<missing>      19 months ago   /bin/sh -c #(nop)  ENV JAVA_VERSION=17.0.2      0B        
<missing>      19 months ago   /bin/sh -c #(nop)  ENV LANG=C.UTF-8             0B        
<missing>      19 months ago   /bin/sh -c #(nop)  ENV PATH=/usr/java/openjd…   0B        
<missing>      19 months ago   /bin/sh -c #(nop)  ENV JAVA_HOME=/usr/java/o…   0B        
<missing>      19 months ago   /bin/sh -c set -eux;  microdnf install   gzi…   39.7MB    
<missing>      19 months ago   /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B        
<missing>      19 months ago   /bin/sh -c #(nop) ADD file:9893213a9ea238f53…   111MB 

```

{: .important}
`#(nop)` 의 의미는 명령어가 아니라는 의미이다. 다시 말해, 실행하는 것이 아닌 것으로 대표적으로 ENV foo=bar 인 경우이다.

### Image Layer on Docker Hub

![](/assets/images/docker-dockerhistory1.png)

### Image Layer on Jfrog Artifactory

![](/assets/images/docker-dockerlayer1.png)
![](/assets/images/docker-dockerlayer2.png)
![](/assets/images/docker-dockerlayer3.png)
![](/assets/images/docker-dockerlayer4.png)
![](/assets/images/docker-dockerlayer5.png)
![](/assets/images/docker-dockerlayer6.png)


## FROM 구문 부분 비교

- docker history
![](/assets/images/docker-dockercomp-1.png)

- docker hub
![](/assets/images/docker-dockercomp-2.png)

- jfrog artifactory
![](/assets/images/docker-dockercomp-3.png)

## 결론

각각의 레이어 갯수가 동일하고 Command 가 동일한 것으로 보아 결국 docker history 로부터 얻어온 값이라는 것을 알 수 있다.