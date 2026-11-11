<picture><source media="(prefers-color-scheme: dark)" srcset="https://nextwork.ai/static/nextwork-logo-paper.svg" /><img src="https://nextwork.ai/static/nextwork-logo-leather.svg" alt="NextWork" width="300" /></picture>

# Break a Distributed System on Purpose

**Project Link:** [View Project](https://nextwork.ai/projects/56330f0f-e0c6-474e-bda9-d625ff412ba0)

**Author:** Richard Nuhu

---

![](https://nextwork.ai/richard-nuhu/uploads/56330f0f-e0c6-474e-bda9-d625ff412ba0_ris7uz4u)

## Project Overview: Exploring the CAP Theorem

### Goals and motivation

In this project, I'm building a distributed key-value store consisting of 3 Flask nodes running in Docker containers, connected on a shared network with configurable consistency and availability modes so that I can deliberately break the network between nodes, and observe firsthand why no system can guarantee perfect data consistency and availability at the same time

## Building the Distributed Node and Cluster

### Setting up the project

In this step, I'm setting up the node as a Flask application, then package it into a Docker container so that I can can run three identical copies on one machine.

![Building the Distributed Node and Cluster](https://nextwork.ai/richard-nuhu/uploads/56330f0f-e0c6-474e-bda9-d625ff412ba0_4yjkejh9)

### Project files and their roles

I created app.py which is responsible for storing the key-value pairs in memory and exposes REST endpoints for reading, writing, and replicating data between peers, Dockerfile which starts from the official python:3.12-slim image, copies the dependency list, installs Flask and requests, then copies the application code, docker-compse.yml which is responsible for orchestration config that spins up three copies on one network.and requirements.txt which is a dependency list.

## Launching the Cluster and Verifying Replication

### Normal operation goals

In this step, I'm launching all three nodes so that I can confirm that data written to one node automatically replicates to the others.

![Launching the Cluster and Verifying Replication](https://nextwork.ai/richard-nuhu/uploads/56330f0f-e0c6-474e-bda9-d625ff412ba0_9897rzta)

### Consistency before any partition

In CP mode, writes to node1 are not stored locally until it has confirmed that ALL peer nodes accepted it first.\
The CAP theorem doesn't force a trade-off yet because data is consistent (same value on all nodes), the system is available (every request gets a response), and the network is intact (no partition).\
The CAP theorem only forces a trade-off when a network partition occurs.

## Simulating a Network Partition in CP Mode

### Partition simulation approach

In this step, I'm simulating network partition so that I can observe how a system in CP mode rejects writes when replication is impossible, sacrificing availability to protect our data's consistency.

![Simulating a Network Partition in CP Mode](https://nextwork.ai/richard-nuhu/uploads/56330f0f-e0c6-474e-bda9-d625ff412ba0_tl0ju0r5)

### Why CP mode rejects writes

Node1 rejected the write because in CP mode the system cannot accept new writes until ALL nodes are reachable.

## Observing Split-Brain in AP Mode

### Switching to AP mode

In this step, I'm switching to AP mode so that I can observe how the system prioritizes availability by accepting writes during a network partition, and how this trade-off results in a split-brain scenario where different nodes hold conflicting data.

### CP vs AP trade-offs explained

In CP mode the system sacrifice availability because the system cannot accept new writes until ALL nodes are reachable again while in AP mode the system accept writes even when nodes are not reachable risking different nodes ending up with different data for the same key.

## Reflections and Key Takeaways

### Tools and concepts mastered

The key tools I used include Flask to build the key-value store nodes, Docker to containerize them, Docker Compose to orchestrate the three-node cluster (node1, node2, and node3) on the capnet bridge network.\
Key concepts I learnt include the practical trade-offs of the CAP Theorem in a live network split. Specifically, how a system in CP Mode sacrifices availability to protect consistency by rejecting updates (returning a 503 error) when replication fails, and how a system in AP Mode maintains availability but results in a split-brain scenario where different nodes hold conflicting values for the exact same key.

### Time and challenges

This project took me approximately 1.5 to 2 hours.\
The most challenging part was...

### Looking ahead

I did this project today to learn how to deliberately break the network between nodes, and observe firsthand why no system can guarantee perfect data consistency and availability at the same time\
 Another skill I want to learn is advanced container orchestration using Kubernetes to manage distributed services, handle automated rolling updates, and implement service meshes.
