---
layout: default
title: Repository with Project
parent: Jfrog
grand_parent: CI/CD Tools 
permalink: /docs/cicdtools/jfrog-repowithprj
nav_order: 40
---

# Repository with Project


---


{: .note }
We can assign new repository with a project when the repository created.

This Rest API can be deployed the combination with project-key and repository-key 

```bash
PUT http://127.0.0.1:8082/artifactory/api/repositories/artifacthu-pg01
{
"projectKey": "artifacthu",
"rclass" : "local",
"packageType": "alpine"
}
```

The results shown as below.

![](/assets/images/jfrogrepoprj1.png)

*[Repository information]*
{: .text-center }


![](/assets/images/jfrogrepoprj2.png)

*[Repository on Project information]*
{: .text-center }
