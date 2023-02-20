# Kubernetes-based Multi-Node Deployment of Cloudplugs for SDM4FZI

## Abstract

Multi-node setups are crucial for manufacturing scenarios as addressed in the sdm4zi project. However, many manufacturing devices, such as the CloudPlug Edge, are designed for single node deployments limiting the dynamics of shopfloor changes drastically. In this work, the de-facto IT standard of Kubernetes is utilized for the service orchestration across a distributed system of CloudPlug nodes. The benefits of this approach are two fold. On the one hand, it enables a multi node setup with meaningful workload distribution in general, and on the other hand it defines a single entry point for e.g. maintenance through the Kubernetes control-plane.

## Introduction

The [CloudPlug Edge](https://www.sotec.eu/loesungen/cloudplug-edge/) manufactured by [Sotec](https://sotec.eu) is a device enabling cloud connectivity for industrial machines.
As of now, software services for the CloudPlug are deployed as Docker containers. In particular, multiple services running on the device, are orchestrated via `docker compose` enabling a dedicated runtime enviroment for each service. Thus, the software stack on the device is easily extensible and modular. Moreover, the declarative deployment with `docker compose` ensures the services to be restarted in case of a failure. This leads to a very stable operation of the CloudPlug Edge in production scenarios.
However, the deployment strategy using `docker compose` limits the deployment to a single device. Further, there is not unique management plane for all devices in a multi-device setup. Consequently, the operation of multiple CloudPlugs can be cumbersome, as every single device needs to be accessed independently for maintenance purposes or troubleshooting. Obviously, these kind of operations contradict the general goal of the sdm4fzi project.
This paper introduces a multi-node deployment strategy based on Kubernetes enabling to manage multiple devices through a single entry point, i.e. the Kubernetes control-plane.
The rest of this article is structured as follows. Section

## Setup Overview

![systemoverview](./images/system_overview.svg#center)

The illustrated system architecture shows an abstract overview of a planned demonstrator. Different edge devices are used to ensure a wide range of variants. These are described in more detail below.

**Monitoring level device**: This device is responsible for display information from various sources. The necessary information is provided by the control level device.

**Control level device**: This device receives data from the other edge devices and processes it to make informed decisions. It acts as a central hub coordinating the actions of other devices and controlling the overall system.

**Application level device**: These devices are supposed to "reflect" the general edge devices - they are supposed to work with applications that you get via the control level device. They are responsible for performing physical actions. And may control a robotic arm or other types of equipment.

All the above devices are connected through a local network and communicate with each other to perform the overall system's tasks. The system is designed to be modular and scalable, allowing for the integration of additional devices as needed.

---


Fig: \ref{systemoverview} system overview shows a state-of-the-art single node application scenario for the Cloudplug. Opposed to the single-node setup, Fig. \ref{todo} illustrates the SDM4FZI use case including multiple CloudPlugs and a Raspberry Pi 4 Model B hosting the Kubernetes control-plane. Conceptually, this mulit-node setup can be extended by futher CloudPlug devices or even other devices can be joined to this "compute cluster" \footnote{Note that this appraoch is similar to the sdm4fzi/hello-world setup \ref{todo}}. Once registered as worker `nodes` in the Kubernetes control-plane, workload can be deployed to a variety of devices on a shopfloor. Consequently, multiple suppliers could attach, e.g. their control nodes for robots or conveyer belts to the cluster and perform software deployments and updates through the Kuberentes API.

For the sake of comprehensiveness, the setup process of the Kubernetes cluster is outlined in the following. We chose to use k3s \ref{todo} as a lightweight Kubernetes distribution for an easy bootstrap of the cluster. A major advantage of using k3s lies in the fact that it is deployed as a single binary, which can be run in `server`-mode for the control-plane components and `agent`-mode for the worker `nodes` \ref{todo}. The k3s binary was installed via the official installation script \ref{todo: https://get.k3s.io/}, and the cluster was instaciated by running:

```shell
k3s .... todo mit Sebastian abstimmen
```

on the Raspberry Pi Model 4B and

```shell
k3s .... todo mit Sebastian abstimmen
```

on the CloudPlug devices. After this initial setup, the Kubernetes API can be accessed using the `kubeconfig`-file stored in todo PFAD on the control-plane node.

## Orchestration of Services

By concept, Kubernetes allows to schedule certain `Pod`s to specific `nodes` in the cluster, i.e. the workload distribution can be controlled declaratively. In order to distinguish the nodes, the concept of `labels` is used within Kubernetes \footnote{One can also group nodes of a certain type by labels. This might be useful, when the system is built with redundancy and multiple nodes are responsible for the control of the same hardware component.}. In the proposed setup, three labels are used: `control-plane`, `worker-1`, and `worker-2`. The `control-plane` node does not host any services but the Kuberentes control-plane, so that scheduling is disabled for this `node` using a `NoSchedule` `Taint` \ref{todo}. Consequently, the production workload is scheduled to `worker-1` and `worker-2`, where `worker-1` runs the typical software stack shipped with the CloudPlug, and `worker-2` serves as plc equivalent node running the business logic of a robot arm controller. Listing \ref{todo} shows example deployment files for this orchestration strategy. Once applied to the Kubernetes API server, the control-plane components ensure that the workload is scheduled to the dedicated `node`s. Note that the state of the software deployement is defined in a single point of truth, whereas the services are running in a distributed system. This charecteristic of Kuberentes-based deployments allows for straightforward monitoring, maintanance and installation of software. Further, other Kuberenetes compliant tooling like Helm \ref{todo}, as a package manager, and Flux \ref{todo}, as a CI/CD framework could be incooperated into the system. This would enable e.g. git-ops workflows for shopfloor maintance helping to comprehensible orchestrate software stack used in production environments. In summary, this simple example shows some of the capabilties Kubernetes can enable with respect to software orchestration.

## Conclusion and Outlook

Obviously, the workload could also be distributed differently on the two worker nodes, which enables the shopfloor planner/operator to react to the dynamics in the production process. From this perspective, this simple example showcases the potential of Kubernertes to contribute to dynamic production scenarios.
