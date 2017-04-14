---
layout: post
title:  "Service Fabric vs Azure Container Services"
date:   2017-4-17 22:00:00 -0500
categories: Azure Service-Fabric Containers Azure
comments: true
---

# Service Fabric
It's an orchestration system built by Microsoft. It's been claimed that they use it to host many of their own services. 
Service Fabric can host Service Fabric services (stateful or stateless), guest executables, or guest containers. 

## Pros
- Microservices without having to use containers 
- Doesn't have to run on Azure
- Stateful services and Actor services (great for IoT scenarios)

## Bad
- Adding environment variables and other configuration is painful
- More XML Configuration
- Can't host traditional ASP.NET applications. Because Service Fabric requires an `exe` entry point, traditional ASP.NET apps were DDLs kick-started by the `Global.asax`
- Not great for migrating existing ASP.NET apps. Either have to convert web app to ASP.NET Core or host it in a Windows container with an IIS image.
- Not great for cross-functional teams. Hosting a Single Page Application would mean that the front-end developers would have get familiar with .NET and Service Fabric. 
- No easy way to share services amongst Service Fabric applications. Closing way if to use [SF Nuget](http://nuget.org/sf-nuget)

# Azure Container Services
Container Services is an offering from Azure that directly competes with Service Fabric. It is an Azure resource group in which the infrastructure is configured for you and you simply have to pick an OSS container orchestration engine. 

## Pros
- Microservices using containers only
- Excellent for business agility
- Excellent for cross-functional teams
- Pay off technical debt quicker
    - If a new version of ASP.NET comes out, you can start migrating services one by one without affecting the others.
- Pick Orchestration Engine
    - Docker Swarm or DC/OS - Only Support Linux containers as of today
    - Kubernetes - Supports both Linux and Windows containers
- Lift n' Shift Legacy Apps
    - Configure docker and use an IIS based Windows container. 
- Service Fabric has a concept of stateful actors. Since ACS is more of a IaaS offering, you can implement your actors however you want. Proto Actor is a great way to start since it's framework agnostic.

