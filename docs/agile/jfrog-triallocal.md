---
layout: default
title: Jfrog Trial - Local 
parent: Jfrog
grand_parent: Devops
permalink: /docs/devops/jfrog-triallocal/
nav_order: 20
---

# Jfrog Trial - Local 

---


{: .note }
Jfrog Artifactory, Xray 의 Trial 버전(30일간) 을 로컬에 설치해 보겠습니다. 여기서는 리눅스 Ubuntu 를 기준으로 설명하겠습니다.


사전에 jdk, dpkg, net-tools 을 사전 설치합니다. 


```bash
root@myubuntu18: sudo apt-get install  openjdk-11-jdk dpkg net-tools
```

다음으로 Trial 버전의 라이선스를 받기 위해 아래 링크를 통해 정보를 입력하면 해당 이메일로 license key 를 받을 수 있습니다. jfrog 설치 후 라이선스를 입력할 수 있습니다. 

[https://jfrog.com/start-free/](https://jfrog.com/start-free/)

 
![](/assets/images/jfrogtriallocal1.png)


위 그림의 정보 입력후 아래 창으로 넘어가면 관련 정보 또한 입력합니다. 중요한 점은 탭 중 Self-Hosted 를 선택하여야 합니다. 
 
![](/assets/images/jfrogtriallocal2.png)

정상적으로 마쳤다면 아래와 같이 설치 정보 링크와 Artifactory/Xray 의 라이선스를 등록한 이메일을 통해 받을 수 있습니다.


![](/assets/images/jfrogtriallocal3.png)


자, 이제 설치를 해봅시다.  아래 순으로 진행합니다.

```bash
root@myutuntu18:~# apt-get update

// 다운로드
root@myutuntu18:~# wget -O jfrog-deb-installer.tar.gz "https://releases.jfrog.io/artifactory/jfrog-prox/org/artifactory/pro/deb/jfrog-platform-trial-prox/7.27.3/jfrog-platform-trial-prox-7.27.3-deb.tar.gz"

// 압축 해제
root@myutuntu18:~# tar -xvzf jfrog-deb-installer.tar.gz

 

root@myutuntu18:~# cd jfrog-platform-trial-pro*
// 설치
root@myutuntu18:~/jfrog-platform-trial-prox-7.27.3-deb# sudo ./install.sh

Beginning JFrog Platform Trial Pro X setup


This script will install JFrog Platform Trial Pro X and its dependencies.
After installation, logs can be found at /root/jfrog-platform-trial-prox-7.27.3-deb/install.log

Installing/Verifying RabbitMQ dependencies (this may take several minutes)...


//중략

NOTE: It is highly recommended to use Artifactory with an external database (MySQL, Oracle, Microsoft SQL Server, PostgreSQL, MariaDB).
      For details about how to configure the database, refer to https://service.jfrog.org/installer/Configuring+the+Database

Start Artifactory with:
> systemctl start artifactory.service

Check Artifactory status with:
> systemctl status artifactory.service


Installation directory was set to /opt/jfrog/artifactory
You can find more information in the log directory /opt/jfrog/artifactory/var/log
System configuration templates can be found under /opt/jfrog/artifactory/var/etc
Copy any configuration you want to modify from the template to /opt/jfrog/artifactory/var/etc/system.yaml

Triggering migration script, this will migrate if needed ...
Processing triggers for man-db (2.8.3-2ubuntu0.1) ...
root@myutuntu18:~/jfrog-platform-trial-prox-7.27.3-deb#



// 서비스 시작
root@myutuntu18:~/jfrog-platform-trial-prox-7.27.3-deb# systemctl status artifactory.service
● artifactory.service - Artifactory service
   Loaded: loaded (/lib/systemd/system/artifactory.service; enabled; vendor pres
   Active: active (running) since Thu 2023-06-08 20:47:43 KST; 10s ago
  Process: 12568 ExecStart=/opt/jfrog/artifactory/app/bin/artifactoryManage.sh s
 Main PID: 15341 (java)
    Tasks: 0 (limit: 4915)
   CGroup: /system.slice/artifactory.service
           ‣ 15341 /opt/jfrog/artifactory/app/third-party/java/bin/java -Djava.u

 6월 08 20:47:42 myutuntu18 artifactoryManage.sh[12568]: frontend not running. P
 6월 08 20:47:42 myutuntu18 artifactoryManage.sh[12568]: frontend started. PID: 
 6월 08 20:47:42 myutuntu18 su[16247]: Successful su for artifactory by root
 6월 08 20:47:42 myutuntu18 su[16247]: + ??? root:artifactory
 6월 08 20:47:42 myutuntu18 su[16247]: pam_unix(su:session): session opened for 
 6월 08 20:47:43 myutuntu18 artifactoryManage.sh[12568]: Starting observability.
 6월 08 20:47:43 myutuntu18 artifactoryManage.sh[12568]: observability not runni
 6월 08 20:47:43 myutuntu18 artifactoryManage.sh[12568]: observability started. 
 6월 08 20:47:43 myutuntu18 su[16247]: pam_unix(su:session): session closed for 
 6월 08 20:47:43 myutuntu18 systemd[1]: Started Artifactory service.
lines 1-19/19 (END)
```

이후 locahost:8082 로 접속하면 아래 화면으로 이동할 수 있습니다.


![](/assets/images/jfrogtriallocal4.png)

조금 기다리면 로그인 창으로 이동하고 이메일에서 받았던 admin / password 를 입력합니다.


![](/assets/images/jfrogtriallocal5.png)


패스워드를 재지정합니다. 


![](/assets/images/jfrogtriallocal6.png)


이후 라이선스를 입력합니다.


![](/assets/images/jfrogtriallocal7.png)

아래와 같이 Jfrog Platform 에 접속하였습니다. 

![](/assets/images/jfrogtriallocal8.png)



