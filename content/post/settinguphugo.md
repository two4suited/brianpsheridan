---
title: "Run Static Website for Cheap"
date: 2019-11-01T16:29:27-07:00
draft: true
tags: ["amazonwebservices"]
---

## Goal
> The goal of this post is to walk through setting up a static website that runs in AWS for as cheap as possible

## Assumptions
* You have a github account and know how to setup repos and push code
* You have a AWS Account 
* You have a build system that can push code to an S3 Bucket

## Steps
1. [Install Hugo](#installhugo)
2. [Find and Download Theme](#theme)
3. [Add new content](#content)
4. [Run Site Locally](#local)
5. [Setup website and S3 bucket to host files](#bucket)
6. [Create a Pipeline to deploy site](#pipeline)

## <a id="installhugo"></a>Instal Hugo 

The easiest way to install Hugo on **Windows** is to use Chocoletely.

1. Follow Instructions to [install chocoletely]("https://chocolatey.org/install")
2. [Install Hugo]("https://gohugo.io/getting-started/installing")
```
choco install hugo -confirm
```
3. Setup a new site
```
hugo new site <name of site>
```
## <a id="theme"></a> Find and Download a Theme

1. Find a theme on this [Page]("https://themes.gohugo.io/")
2. Add Theme as submodule 
 ```
$ cd <name of site>

$ git init 

$ git submodule add <path to git repo> themes/<name of theme>
```
3. Change theme attribute in *config.toml* at root folder to the name of your theme

## <a id="content"></a>Add New Content

The important part about adding new content it to get the path correct and have the file extension of **.md**

In this example we will add a new blog post.

```
$ hugo new <path>/<nameoffile>.md
```


## <a id="local"></a>Run Site Locally

Hugo takes your markdown and creates a static html site from it.  You are able to do this locally and get realtime updates in a browser as you edit and save your markdown
```
hugo server
```
Then you can go to [http://localhost:1313]("http://localhost:1313) and see your site in action

## <a id="bucket"></a>Setup website and S3 bucket to host files

>S3 is a low cost block storage solution for storing files on AWS

To make this all work you will need a AWS account and the basic understanding of the following:
* How to upload files to S3
* How to register a site on AWS
* How to use Route 53 for DNS for your site


## <a id="pipeline"></a>Create a Pipeline to deploy site
