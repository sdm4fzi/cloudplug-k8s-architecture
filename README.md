# Kubernetes-based Multi-Node Deployment of Cloudplugs for SDM4FZI

Multi-node setups are crucial for manufacturing scenarios as addressed in the sdm4zi project. However, many manufacturing devices, such as the CloudPlug Edge, are designed for single node deployments limiting the dynamics of shopfloor changes drastically. In this work, the de-facto IT standard of Kubernetes is utilized for the service orchestration across a distributed system of CloudPlug nodes. The benefits of this approach are two fold. On the one hand, it enables a multi node setup with meaningful workload distribution in general, and on the other hand it defines a single entry point for e.g. maintenance through the Kubernetes control-plane.

This repository showcases the design of the demonstrator presented in the project, and explores the transition from Docker to Kubernetes. The focus is primarily on the implementation process and the resulting problems and difficulties encountered. Additionally, a comparison is made between the solution implemented with Docker and the one implemented with Kubernetes.

A summarized version of this information is described in an existing article. However, more granular details regarding individual aspects can be found in sub-sections/folders. This project was created through the collaboration of [SOTEC GmbH & Co KG 2023](https://sotec.eu) and [23 Technologies GmbH](https://23technologies.cloud/de) for the projekt [SDM4FZI](https://www.sdm4fzi.de/).

