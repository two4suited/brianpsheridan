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
1. Install Hugo
2. Find and Download Theme
3. Add new content
4. Setup website and S3 bucket to host files
4. Create a CD Pipeline that copies code to an s3 bucket
