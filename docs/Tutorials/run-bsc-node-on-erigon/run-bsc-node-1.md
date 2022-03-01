---
title: Run a BSC Node on Erigon
id: erigon-bsc
---

# How to run a BSC Node on Erigon

## Introduction&#x20;

This guide will walk you through 3 options for setting up and launching a BSC Node on Erigon.&#x20;

Here are the main benefits of running BSC on Erigon:

* Drastically reduced disk storage - 1.2TB for Archive Node, 430GB for Pruned Node.&#x20;
* Faster sync speed > 10 blocks per second&#x20;
* Fully bootstrapped archive node in <3days.&#x20;
* Crash resilience - Erigon’s database can withstand power failures. Modularity - P2P and web3 RPC services can be run as components.

If you want to be able to run a node without buying vast disk space then Erigon is going to make a massive difference.

:::info
This guide was written for an Ubuntu Server 20.04 LTS
:::i

### **00 Prerequisites**

|      ## Requirement   |      ## Specification  |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| Storage        | 2TB on a single partition, 1.6TB state 200GB temp files (can symlink or mount folder <datadir>/etl-tmp to another disk). |
| RAM             | 16GB |
| OS              | 64-bit <br>Linux  |
| Network settings | Open up Port 22 for SSH <br>Static IP settings   |
