---
title: "Run Static Website for Cheap"
date: 2019-11-1T16:29:27-07:00
draft: false
tags: ["amazonwebservices"]
description: "Using Hugo and AWS S3 to setup highly scalable, easy to maintain website"
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

1. Create a bucket that matches your domain.  In my case that domain is brianpsheridan.com 
2. Under Properties, choose static webhosting and choose *Use this bucket to host a website*  
3. Type *index.html* in the Index Document
4. On Permission tab unblock public access and change the public policy to 
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::brianpsheridan.com/*"
        }
    ]
}
```

5. Upload your hugo code from the *public* folder to your s3 bucket
6. Goto Route 53 and creat a record set 

![Route 53 Create Record Set](../../images/posts/Route53Create.png)

> At this point you can also add another bucket(www.< sitename >.com) and DNS entry if you want your sit to redirect the alias www to your root site.  You will just setup redirect in the static web hosting part of s3, point it to your bucket that you used for your site.  Then create an alias in route 53 for www that points to your newly created bucket.


## <a id="pipeline"></a>Create a Pipeline to deploy site

> I use Azure Devops to do my deployments.  They have a built in task for Hugo and uploading to S3.

[Hugo Documentation for Deploying](https://gohugo.io/commands/hugo_deploy/)

1. Run Hugo command to generate your site, give it a destination
2. Upload the files from your destination to s3

### The Yaml file for Azure Devops

>  You will need to AWS Credentials in your Azure Devops project with a role that has the ability to upload to the s3 Bucket and change permissions on files
```

trigger:
- master

pool:
  vmImage: 'windows-latest'

steps:
- task: HugoTask@1
  inputs:
    source: 
    destination: '$(System.DefaultWorkingDirectory)/public'

- task: S3Upload@1
  inputs:
    awsCredentials: 'AWS'
    regionName: 'us-west-2'
    bucketName: 'brianpsheridan.com'
    sourceFolder: '$(System.DefaultWorkingDirectory)/public'
    globExpressions: '**'
    filesAcl: 'public-read'

```

This will get you all setup to run a cheap site on AWS using Hugo that can handle any sort of traffic.  

There are some more advanced topics such as setting Cloudfront so your data is cached 
