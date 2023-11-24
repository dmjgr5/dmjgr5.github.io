---
layout: default
title: Jfrog AQL
parent: Jfrog
grand_parent: Devops
permalink: /docs/devops/jfrog-aql/
nav_order: 31
---

# Jfrog AQL
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---


{: .note }
Artifactory Query Language 를 이용하여 다양한 조건으로 빠른 검색이 가능하다.   
참조 URL : [https://jfrog.com/help/r/jfrog-artifactory-documentation/artifactory-query-language](https://jfrog.com/help/r/jfrog-artifactory-documentation/artifactory-query-language)

## Search URL

```sh
POST /api/search/aql  
items.find(
        {
                "repo":{"$eq":"libs-release-local"}
        }
)
```

## All Docker Images for All repositories including Tags

```sh
items.find(
  {
    "$and":[
      {"$or":[
        {"$and":[
          {"name":{"$eq":"manifest.json"}},
          {"$or":[
            {"@docker.manifest":{"$match":"*"}},
            {"@docker.manifest":{"$match":"library/*"}}
          ]}
        ]}
      ]},
      {"$rf":[
        {"$or":[
          {"property.key":{"$eq":"docker.manifest"}},
          {"property.key":{"$eq":"sha256"}},
          {"property.key":{"$eq":"docker.repoName"}}
        ]}
      ]}
    ]
  }
).
include("updated","type","created","actual_sha1","property.key","id","modified_by","size","actual_md5","depth","original_md5","created_by","modified","sha256","name","repo","property.value","path","original_sha1").
limit(1500)



{
    "results": [
        {
            "repo": "docker-trial",
            "path": "exposures/latest",
            "name": "manifest.json",
            "type": "file",
            "size": 426,
            "created": "2023-11-13T15:59:27.568Z",
            "created_by": "trialadmin",
            "modified": "2023-11-13T15:59:27.494Z",
            "modified_by": "trialadmin",
            "updated": "2023-11-13T15:59:27.571Z",
            "depth": 3,
            "actual_md5": "d8a7ad64eb26f820707e2fedc3915555",
            "actual_sha1": "1009c61f824d56ac635650f730257dc487cb9748",
            "properties": [
                {
                    "key": "sha256",
                    "value": "79c8e8e1199430b17e6d3d2110f93159b0cfb082e6b8fcf6d48edadefd7de799"
                },
                {
                    "key": "docker.manifest",
                    "value": "latest"
                },
                {
                    "key": "docker.repoName",
                    "value": "exposures"
                }
            ],
            "sha256": "79c8e8e1199430b17e6d3d2110f93159b0cfb082e6b8fcf6d48edadefd7de799"
        },
        {

```

## Top 10 Largest Artifacts

```bash 
items.find({
  "repo":"spring-vue-starter-repo"
}).include("name","repo","path","size")
  .sort({"$desc": ["size"]})
  .limit(10)


{
    "results": [
        {
            "repo": "spring-vue-starter-repo",
            "path": "spring-vue-image/90",
            "name": "sha256__a7203ca35e75e068651c9907d659adc721dba823441b78639fde66fc988f042f",
            "size": 187528104
        },
        {
            "repo": "spring-vue-starter-repo",
            "path": "spring-vue-image/93",
            "name": "sha256__a7203ca35e75e068651c9907d659adc721dba823441b78639fde66fc988f042f",
            "size": 187528104
        },
        {
            "repo": "spring-vue-starter-repo",
            "path": "spring-vue-image/90",
            "name": "sha256__38a980f2cc8accf69c23deae6743d42a87eb34a54f02396f3fcfd7c2d06e2c5b",
            "size": 42114164
        },
        {
            "repo": "spring-vue-starter-repo",
            "path": "spring-vue-image/93",
            "name": "sha256__38a980f2cc8accf69c23deae6743d42a87eb34a54f02396f3fcfd7c2d06e2c5b",
            "size": 42114164
        },
        {
            "repo": "spring-vue-starter-repo",
            "path": "spring-vue-image/90",
            "name": "sha256__2bde522f954fe46c4abe389ff03d4f12ef885aa822bf6a9cae730d4f89440d2f",
            "size": 16423762
        },
        {
            "repo": "spring-vue-starter-repo",
            "path": "spring-vue-image/93",
            "name": "sha256__2bde522f954fe46c4abe389ff03d4f12ef885aa822bf6a9cae730d4f89440d2f",
            "size": 16423762
        },
        {
            "repo": "spring-vue-starter-repo",
            "path": "spring-vue-image/90",
            "name": "sha256__de849f1cfbe60b1c06a1db83a3129ab0ea397c4852b98e3e4300b12ee57ba111",
            "size": 13525123
        },
        {
            "repo": "spring-vue-starter-repo",
            "path": "spring-vue-image/93",
            "name": "sha256__de849f1cfbe60b1c06a1db83a3129ab0ea397c4852b98e3e4300b12ee57ba111",
            "size": 13525123
        },
        {
            "repo": "spring-vue-starter-repo",
            "path": "spring-vue-image/90",
            "name": "sha256__53ae2020bd8e6bd80952db2ce7c7ffcf01d56c43bd1fd44c3dd918549d4eca85",
            "size": 5166
        },
        {
            "repo": "spring-vue-starter-repo",
            "path": "spring-vue-image/93",
            "name": "sha256__53ae2020bd8e6bd80952db2ce7c7ffcf01d56c43bd1fd44c3dd918549d4eca85",
            "size": 5166
        }
    ],
    "range": {
        "start_pos": 0,
        "end_pos": 10,
        "total": 10,
        "limit": 10
    }
}
```

##  Package name and Checksum in AQL



```bash
items.find().include("name","actual_sha1","actual_md5","sha256")


{
    "results": [
        {
            "name": "f3e2e779db6c-metadata-consumption-usage-2023-11-22T16-10-28.615.log.gz",
            "actual_md5": "3f5c0f28feb49d5c08dddc46b91a75a4",
            "actual_sha1": "5799ce3522cc8e42b64e3fb06cbb7e02c0e0113f",
            "sha256": "aef828205839365dd2b99d9367946172a9e91b00677426340f1deee791128dcd"
        },
        {
            "name": "e49785788224-xray-analysis-usage-2023-11-22T02-50-05.661.log.gz",
            "actual_md5": "f0d79988b7772c003d04a28bd7417a62",
            "actual_sha1": "58423a999eec2997bcfffb247e9ecd3dfd0abf44",
            "sha256": "30e6fa98fb48c2b132824d1ac5e2243c0be9e9082ff32598d34d7687ca7f6c7f"
        },
```


```
items.find({"repo": {"$eq":"spring-vue-starter-repo"}}).include("repo", "name", "property")

{
    "results": [
        {
            "repo": "spring-vue-starter-repo",
            "name": "sha256__53ae2020bd8e6bd80952db2ce7c7ffcf01d56c43bd1fd44c3dd918549d4eca85",
            "properties": [
                {
                    "key": "sha256",
                    "value": "53ae2020bd8e6bd80952db2ce7c7ffcf01d56c43bd1fd44c3dd918549d4eca85"
                }
            ]
        },
        {
```

## stat

```bash
items.find({"repo": {"$eq":"spring-vue-starter-repo"}}).include("stat")


{
    "results": [
        {
            "repo": "spring-vue-starter-repo",
            "path": "spring-vue-image/90",
            "name": "sha256__53ae2020bd8e6bd80952db2ce7c7ffcf01d56c43bd1fd44c3dd918549d4eca85",
            "type": "file",
            "size": 5166,
            "created": "2023-11-14T12:32:55.010Z",
            "created_by": "user1@gmail.com",
            "modified": "2023-11-14T12:32:54.939Z",
            "modified_by": "user1@gmail.com",
            "updated": "2023-11-14T12:32:55.012Z",
            "stats": [
                {
                    "downloads": 0,
                    "remote_downloads": 0
                }
            ]
        },
```

## never been downloaded but are older than 7 days, or just haven’t been downloaded in the last 30 days:

```sh
items.find({
    "repo": { "$eq": "spring-vue-starter-repo" },
    "$or" :[
        {
            "$and": [
                { "stat.downloads": { "$eq":null } },
                { "updated": { "$before": "7d" } }
            ]
        },
        {
            "$and": [
                { "stat.downloads": { "$gt": 0 } },
                { "stat.downloaded": { "$before": "30d" } }
            ]
        }
    ]
}).include("repo", "name", "path", "updated", "sha256", "stat.downloads", "stat.downloaded")
```

## Sorting

```sh
items.find(
  {
     "type":"file",
     "created_by":"user1@gmail.com",
     "size":{"$gt":"100"}
   })
.sort({"$desc":["size","name"]})
.limit(50)
```

## Image name TO SHA256 Digest 

```sh


items.find({ 
    "repo": "spring-vue-starter-repo",
    "path": "spring-vue-image/93",
    "name":"manifest.json"}
    )
    .include("repo","path","name","sha256","property.*")


{
    "results": [
        {
            "repo": "spring-vue-starter-repo",
            "path": "spring-vue-image/93",
            "name": "manifest.json",
            "properties": [
                {
                    "key": "docker.manifest",
                    "value": "93"
                },
                {
                    "key": "sha256",
                    "value": "4805b83bcfd574ac0201e62861c7077006286a854ca093a21dc9b35e1bcb10e7"
                },
                {
                    "key": "oci.artifact.type",
                    "value": "application/vnd.docker.container.image.v1+json"
                },
                {
                    "key": "artifactory.content-type",
                    "value": "application/vnd.docker.distribution.manifest.v2+json"
                },
                {
                    "key": "docker.repoName",
                    "value": "spring-vue-image"
                },
                {
                    "key": "docker.manifest.digest",
                    "value": "sha256:4805b83bcfd574ac0201e62861c7077006286a854ca093a21dc9b35e1bcb10e7"
                },
                {
                    "key": "docker.manifest.type",
                    "value": "application/vnd.docker.distribution.manifest.v2+json"
                }
            ],
            "sha256": "4805b83bcfd574ac0201e62861c7077006286a854ca093a21dc9b35e1bcb10e7"
        }
    ],
    "range": {
        "start_pos": 0,
        "end_pos": 1,
        "total": 1
    }
}
```




## SHA256 Digest TO Image name

```bash

items.find({"type":"file", "sha256":"4805b83bcfd574ac0201e62861c7077006286a854ca093a21dc9b35e1bcb10e7"})



{
    "results": [
        {
            "repo": "docker-trial",
            "path": "testimage/1.123",
            "name": "manifest.json",
            "type": "file",
            "size": 1372,
            "created": "2023-11-14T12:32:58.982Z",
            "created_by": "user1@gmail.com",
            "modified": "2023-11-14T12:32:58.874Z",
            "modified_by": "user1@gmail.com",
            "updated": "2023-11-14T12:32:58.984Z"
        },
        {
            "repo": "spring-vue-starter-repo",
            "path": "spring-vue-image/90",
            "name": "manifest.json",
            "type": "file",
            "size": 1372,
            "created": "2023-11-22T13:56:51.849Z",
            "created_by": "user1@gmail.com",
            "modified": "2023-11-22T13:56:51.743Z",
            "modified_by": "user1@gmail.com",
            "updated": "2023-11-22T13:56:51.851Z"
        },
        {
            "repo": "spring-vue-starter-repo",
            "path": "spring-vue-image/93",
            "name": "manifest.json",
            "type": "file",
            "size": 1372,
            "created": "2023-11-23T12:27:41.535Z",
            "created_by": "user1@gmail.com",
            "modified": "2023-11-23T12:27:41.387Z",
            "modified_by": "user1@gmail.com",
            "updated": "2023-11-23T12:27:41.537Z"
        },
        {
            "repo": "spring-vue-starter-repo",
            "path": "spring-vue-image/_uploads",
            "name": "manifest-sha256__4805b83bcfd574ac0201e62861c7077006286a854ca093a21dc9b35e1bcb10e7.json",
            "type": "file",
            "size": 1372,
            "created": "2023-11-22T13:56:51.849Z",
            "created_by": "user1@gmail.com",
            "modified": "2023-11-22T13:56:51.743Z",
            "modified_by": "user1@gmail.com",
            "updated": "2023-11-22T13:56:51.851Z"
        }
    ],
    "range": {
        "start_pos": 0,
        "end_pos": 4,
        "total": 4
    }
}
```

