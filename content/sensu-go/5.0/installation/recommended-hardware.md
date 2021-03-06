---
title: "Hardware Requirements and Recommendations"
linkTitle: "Hardware Requirements"
description: "Guidelines for what you'll need for deploying Sensu Core to production."
weight: 3
version: "5.0"
product: "Sensu Go"
platformContent: false
menu:
  sensu-go-5.0:
    parent: installation
aliases:
  - /sensu-go/5.0/getting-started/recommended-hardware/
---

### Backend Minimum Requirements

* 64-bit Intel or AMD CPU
* 4 GB RAM
* 4 GB free disk space
* 10 mbps network link

### Backend Recommended Configuration

* 64 bit 4-core Intel or AMD CPU
* 8 GB RAM
* SSD (NVMe or SCSI)
* Gigabit ethernet

The Sensu Backend is typically CPU and storage intensive. Its use of these
resources scales linearly with the total number of checks that all agents
connecting to the backend are executing.

Sensu Backend is a massively parallel application that can scale to any number
of CPU cores. Provision roughly 1 CPU core for every 50 checks per second,
including keepalives.

Most installations will do fine with 4 CPU cores, but larger installations
may find that 8 CPU cores are necessary.

Each check that is executed will result in writes occurring in the backend.
When provisioning storage hardware, a good rule of thumb is to have twice as
many iops as you expect to have checks executing per second. Don't forget to
include keepalives in this calculation; each agent will execute a keepalive
every 20 seconds. So in a cluster of 100 agents, you can expect those agents
to consume 10 write iops for keepalives.

The Sensu Backend uses a relatively modest amount of RAM under most
circumstances, but some users with a large amount of information stored
may find that Sensu will use a large amount of RAM (2-4 GB). In many
installations, 8 GB will be more than sufficient.

### Agent Minimum Requirements

* 386, amd64, or ARM CPU (armv5 minimum)
* 128 MB RAM
* 10 mbps network link

### Agent Recommended Configuration

* 4-core amd64 or ARMv8 CPU
* 512 MB RAM
* Gigabit ethernet

The Sensu Agent itself is quite lightweight, and should be able to run on all
but the most modest hardware. However, since the agent is responsible for
executing checks, factor the agent's responsibilities into your hardware
provisioning.

### Networking Recommendations

Sensu uses websockets for communicating between the backend and the agent. This
means that all communication between a backend and an agent will occur over a
single TCP socket.

It's recommended that users connect backends and agents via gigabit ethernet,
but any somewhat-reliable network link should work. If you see websocket
timeouts in the backend logs from agentd, then you may need to use a better
network link between the backend and agents.
