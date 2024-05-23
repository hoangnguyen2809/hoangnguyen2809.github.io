---
title: "Programmable Network Switch Implementation Using P4"
excerpt: "Implemented network functionalities using the P4 programming language <br /><img src='/images/ids.png'>"
collection: portfolio
---

# Overview

Implemented network functionalities using the P4 programming language, including a Layer 2 (L2) firewall and an Intrusion Detection System (IDS). The project involved setting up a programmable network environment, experimenting with the P4 language, and developing control plane programs.

## Objectives

- Familiarize with the P4 language ecosystem.
- Explore control plane APIs.
- Implement a simple L2 firewall using P4.
- Implement a simple intrusion detection system (IDS) using P4.

## Key Achievements

### Explored and Experimented with the P4 Environment

- Set up a network topology and instantiated a software switch using Mininet and P4.
- Collected basic outputs and traces to understand the network behavior.
- Enabled and inspected PCAP files and packet processing logs for deeper insights.

### Developed a Layer 2 (L2) Firewall

- Implemented static MAC address to port mapping.
- Added firewall functionality to block traffic from specific hosts.
- Tested and verified firewall behavior using Mininet commands and log inspections.

### Implemented a Stateless and Stateful IDS

- Designed and implemented a stateless IDS to inspect TCP payloads for known malicious signatures.
- Extended the stateless IDS to a stateful IDS that examines multiple packets in a flow to detect distributed signatures.
- Configured control plane scripts to handle IDS alerts and maintain flow state.

## Technical Skills Gained

### P4 Programming

- Writing P4 programs to define packet processing behavior on programmable switches.
- Using tables, actions, and control flow constructs in P4.

### Network Setup and Configuration

- Configuring virtual network environments using VirtualBox/VMWare and Mininet.
- Creating network topologies and managing network interfaces.

### Control Plane Development

- Implementing control plane logic using Python.
- Populating and managing P4 table entries via Python APIs.

### Packet Analysis

- Capturing and analyzing network traffic using PCAP files and tools like tshark.
- Logging and inspecting packet processing steps to debug and verify network behavior.

## Tools and Technologies Used

- **P4 Language:** For programming network switch behavior.
- **Mininet:** To create and manage virtual network topologies.
- **VirtualBox/VMWare:** For setting up virtual machines running the P4 environment.
- **tshark:** For analyzing packet captures.
- **Python:** For control plane scripts and network configuration.

## Supporting Documents:

Code are available at [Github](https://github.com/hoangnguyen2809/Firewall-IDS) <br>
