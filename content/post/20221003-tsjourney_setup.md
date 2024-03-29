---
title: "Typescript Journey - Setup"
date: 2022-10-02T08:00:00-07:00
draft: false
tags: ["learning,typescript"]
description: "Typescript Journey - Setup"
---
# Typescript Journey - Setup

I am going to using [Dev Containers](https://code.visualstudio.com/docs/remote/containers) to do my development.  This will allow me to use a docker container or Github Codespaces to do my Typescript development.

[**Here is the repo**](https://github.com/two4suited/TypescriptJourney/tree/setup)

## Prerequisites
- Docker Desktop
- Visual Studio Code
- Visual Studio Code Extension -  Dev Containers,GitHub Codespaces

## Walkthrough 

1. Setup Dev Container Configuration Files

_View -> Command Palette -> Remote Containers: Add Development Container Configuration Files_

2. Choose Alpine and Latest Version
3. Open Folder in Container
![](/static/images/posts/Screenshot 2022-10-02 at 11.36.59 AM.png)
4. Change Dockerfile and devcontainer.json to support Typescript and Node

**Dockerfile**

```
FROM mcr.microsoft.com/vscode/devcontainers/typescript-node:0-12
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install git
```
**devcontainer.json**
```
{
	"build": {"dockerfile": "Dockerfile"},
	"customizations": {
		"vscode": {
		  "extensions": ["dbaeumer.vscode-eslint"]
		}
	  },	
	// Use 'forwardPorts' to make a list of ports inside the container available locally.
	 "forwardPorts": [3000]	
}

```

_Next we will run your first Typescript App._

