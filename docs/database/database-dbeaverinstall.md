---
layout: default
title: 우분투에서 DBeaver 설치
parent: Database
nav_order: 17
---

# 우분투에서 DBeaver 설치 

{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}
----
  


## 설치파일 다운로드

[https://dbeaver.io/download/](https://dbeaver.io/download/)

위 링크에 접속 후 `Linux Debian package (installer)` 를 다운로드 한다.

## DBeaver 설치

.deb 파일을 이용하여 설치한다.

```sh
user1@user1-500R5K-501R5K-500R5Q:~/Downloads$ sudo dpkg -i dbeaver-ce_23.2.5_amd64.deb 
[sudo] password for user1: 
(Reading database ... 248750 files and directories currently installed.)
Preparing to unpack dbeaver-ce_23.2.5_amd64.deb ...
Unpacking dbeaver-ce (23.2.5) over (23.2.3) ...
Setting up dbeaver-ce (23.2.5) ...
Processing triggers for gnome-menus (3.36.0-1ubuntu1) ...
Processing triggers for desktop-file-utils (0.24-1ubuntu3) ...
Processing triggers for mime-support (3.64ubuntu1) ...
```

## DBeaver 실행

`dbeaver` 명령어로 실행한다.

```sh
user1@user1-500R5K-501R5K-500R5Q:~/Downloads$ dbeaver
> Start Eclipse Jobs Mechanism [org.eclipse.core.jobs 3.15.0.v20230808-1403]
> Start Equinox Java Authentication and Authorization Service (JAAS) [org.eclipse.equinox.security 1.4.0.v20230630-1303]
> Start DBeaver Application Standalone [org.jkiss.dbeaver.ui.app.standalone 23.2.5.202311191730]
> Start Eclipse IDE UI Application [org.eclipse.ui.ide.application 1.5.100.v20230821-1342]
> Start Eclipse IDE UI [org.eclipse.ui.ide 3.21.100.v20230825-1346]
> Start DBeaver UI Editors - Base [org.jkiss.dbeaver.ui.editors.base 1.0.119.202311191730]
> Start Expression Language [org.eclipse.core.expressions 3.9.100.v20230726-0617]
> Start DBeaver Usage Statistics [org.jkiss.dbeaver.ui.statistics 1.0.15.202311191730]
> Start DBeaver Desktop Application Core [org.jkiss.dbeaver.core 23.2.5.202311191730]
2023-11-28 22:47:11.436 - DBeaver 23.2.5.202311191730 is starting
2023-11-28 22:47:11.437 - OS: Linux 5.15.0-89-generic (amd64)
2023-11-28 22:47:11.437 - Java version: 17.0.6 by Eclipse Adoptium (64bit)
2023-11-28 22:47:11.437 - Install path: '/usr/share/dbeaver-ce'
2023-11-28 22:47:11.438 - Instance path: 'file:/home/user1/.local/share/DBeaverData/workspace6/'
2023-11-28 22:47:11.438 - Memory available 110Mb/1024Mb
2023-11-28 22:47:11.441 - Create display
2023-11-28 22:47:11.560 - Initializing DBeaver 23.2.5
2023-11-28 22:47:11.561 - Host plugin: org.jkiss.dbeaver.ui.app.standalone 23.2.5.202311191730
2023-11-28 22:47:11.561 - Initialize desktop platform...
2023-11-28 22:47:11.778 - BounceCastle bundle found. Use JCE provider BC
> Start Core File Systems [org.eclipse.core.filesystem 1.10.100.v20230717-1727]
> Start Core Resource Management [org.eclipse.core.resources 3.19.100.v20230814-1312]
> Start DBeaver UI Navigator [org.jkiss.dbeaver.ui.navigator 1.0.119.202311191730]
> Start Team Support Core [org.eclipse.team.core 3.10.100.v20230726-0617]
> Start Debug Core [org.eclipse.debug.core 3.21.100.v20230731-1425]
> Start jna [com.sun.jna 5.13.0.v20230812-1000]
> Start Internet Connection Management UI [org.eclipse.ui.net 1.5.100.v20230726-0617]
> Start Internet Connection Management [org.eclipse.core.net 1.5.100.v20230630-1232]
> Start JSch Core [org.eclipse.jsch.core 1.5.100.v20230726-0617]
2023-11-28 22:47:12.113 - Initialize base platform...
2023-11-28 22:47:12.146 - Platform initialized (585ms)
2023-11-28 22:47:12.464 - Error accessing instance server: java.net.ConnectException
2023-11-28 22:47:12.481 - Starting instance server at http://localhost:37279
2023-11-28 22:47:12.490 - Run workbench
> Start DBeaver UI [org.jkiss.dbeaver.ui 5.1.134.202311191730]
> Start Eclipse e4 Workbench SWT [org.eclipse.e4.ui.workbench.swt 0.17.100.v20230727-0604]
> Start Eclipse Application Services [org.eclipse.e4.core.services 2.4.100.v20230708-0809]
> Start Eclipse UI Application Services [org.eclipse.e4.ui.services 1.6.100.v20230630-1554]
> Start Eclipse e4 Progress View [org.eclipse.e4.ui.progress 0.4.100.v20230727-0604]
> Start Eclipse Bindings Support [org.eclipse.e4.ui.bindings 0.14.100.v20230630-1554]
> Start Eclipse e4 core commands [org.eclipse.e4.core.commands 1.1.100.v20230630-1554]
> Start EMF XML/XMI Persistence [org.eclipse.emf.ecore.xmi 2.35.0.v20230801-1141]
> Start Eclipse CSS SWT Theme Support [org.eclipse.e4.ui.css.swt.theme 0.14.100.v20230727-0604]
> Start Eclipse e4 Workbench Add-ons [org.eclipse.e4.ui.workbench.addons.swt 1.5.100.v20230727-0604]
> Start Eclipse e4 Workbench SWT Renderer [org.eclipse.e4.ui.workbench.renderers.swt 0.16.100.v20230801-1255]
> Start Eclipse Dependency Injection Extensions [org.eclipse.e4.core.di.extensions 0.18.0.v20230429-1914]
> Start Eclipse CSS SWT Support [org.eclipse.e4.ui.css.swt 0.15.100.v20230727-0604]
> Start Eclipse SDK Themes [org.eclipse.ui.themes 1.2.2300.v20230807-1354]
2023-11-28 22:47:14.599 - Configure workbench window
> Start DBeaver SQL Model [org.jkiss.dbeaver.model.sql 1.0.109.202311191730]
2023-11-28 22:47:14.811 - Total database drivers: 106 (106)
> Start Help System Core [org.eclipse.help 3.10.100.v20230726-0617]
2023-11-28 22:47:14.963 - Create actions
2023-11-28 22:47:14.979 - Disable Eclipse action set 'org.eclipse.ui.edit.text.actionSet.annotationNavigation'
2023-11-28 22:47:14.979 - Disable Eclipse action set 'org.eclipse.ui.WorkingSetActionSet'
2023-11-28 22:47:14.980 - Disable Eclipse action set 'org.eclipse.ui.actionSet.openFiles'
2023-11-28 22:47:16.056 - Initialize workbench window
2023-11-28 22:47:16.059 - Finish initialization
> Start DBeaver UI Editors - Data [org.jkiss.dbeaver.ui.editors.data 1.0.119.202311191730]
> Start DBeaver UI Editors - SQL [org.jkiss.dbeaver.ui.editors.sql 1.0.119.202311191730]
> Start GEF Classic Draw2d [org.eclipse.draw2d 3.14.0.202308311129]
> Start GEF Classic GEF (MVC) [org.eclipse.gef 3.15.0.202308311129]
> Start DBeaver ERD UI [org.jkiss.dbeaver.erd.ui 3.0.79.202311191730]
> Start DBeaver ERD Model [org.jkiss.dbeaver.erd.model 1.0.79.202311191730]
> Start Default Text Editor [org.eclipse.ui.editors 3.17.0.v20230821-1342]
> Start Text Editor Framework [org.eclipse.ui.workbench.texteditor 3.17.100.v20230821-0745]
> Start DBeaver Sample Database [org.jkiss.dbeaver.ext.sample.database 1.0.156.202311191730]
> Start DBeaver Tip of the day [org.jkiss.dbeaver.ext.ui.tipoftheday 1.0.125.202311191730]
2023-11-28 22:47:17.523 - Open [https://dbeaver.io/product/version.xml]
```

아래와 같이 툴 화면이 열린다.


![](/assets/images/dbeaver-ui.png)



