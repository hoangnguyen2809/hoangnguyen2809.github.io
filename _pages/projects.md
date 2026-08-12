---
layout: archive
title: "Projects"
permalink: /projects/
author_profile: true
---
{% include base_path %}

A collection of projects and hands-on labs focused on **cybersecurity, infrastructure, networking, systems programming, and software development**.

---

# Cybersecurity & Infrastructure

## Internal Penetration Testing Lab

Built an isolated penetration-testing environment using **Kali Linux, Metasploitable 2, and Windows** to practice vulnerability assessment, exploitation, and post-exploitation techniques in a controlled environment.

**Technologies:** Kali Linux · Nmap · Nessus · Metasploit · VMware · Windows

* Designed and configured an isolated virtual network for offensive security testing.
* Performed host discovery, port scanning, service enumeration, and vulnerability assessment.
* Identified and validated exploitable vulnerabilities, including MS17-010.
* Practiced exploitation and post-exploitation using Metasploit and Meterpreter.
* Documented attack paths, findings, and defensive considerations.

[View Project](/portfolio/portfolio-6/) · [Technical Writeup](/posts/2025/05/vultest/)

---

## Firewall and Intrusion Detection System

Implemented programmable network-security functionality using **P4 and Python**, combining packet filtering with intrusion-detection concepts.

**Technologies:** P4 · Python · Mininet · Software-Defined Networking

* Developed packet-processing and traffic-control logic using P4.
* Implemented firewall functionality for controlling network traffic.
* Explored intrusion-detection techniques using programmable networking.
* Tested network behavior in virtualized Mininet environments.

[GitHub](https://github.com/hoangnguyen2809/Firewall-IDS)

---

## Buffer Overflow Attack Lab

Analyzed and exploited stack-based buffer overflow vulnerabilities in a controlled Linux environment as part of the SEED Security Labs.

**Technologies:** Linux · C · GDB · Memory Exploitation · SEED Labs

* Analyzed vulnerable programs and stack memory behavior.
* Developed payloads to trigger and exploit buffer overflow vulnerabilities.
* Studied the impact of common operating-system security mechanisms.
* Evaluated protections and countermeasures against memory-corruption attacks.

[GitHub](https://github.com/hoangnguyen2809/SEED_buffer_overflow) · [Technical Writeup](/posts/2024/07/blog-post-5/)

---

## TCP/IP Attack and Defense Lab

Performed controlled attacks against TCP/IP protocols to understand network vulnerabilities and the mechanisms used to mitigate them.

**Technologies:** Linux · TCP/IP · Wireshark · Scapy · SEED Labs

* Performed TCP SYN flooding and analyzed its effect on network services.
* Explored TCP reset and TCP session hijacking attacks.
* Captured and analyzed network traffic during attack scenarios.
* Evaluated protocol behavior and defensive countermeasures.

[GitHub](https://github.com/hoangnguyen2809/TCP-Attack-Lab) · [Technical Writeup](/posts/2024/04/blog-post-4/)

---

# Networking & Distributed Systems

## Programmable Network Switch Using P4

Implemented programmable network functionality using the **P4 programming language**, focusing on packet parsing, forwarding logic, and data-plane behavior.

**Technologies:** P4 · Mininet · Software-Defined Networking · Linux

* Defined custom packet-processing behavior using P4.
* Worked with packet headers, parsing, match-action tables, and forwarding rules.
* Tested programmable network behavior in a virtualized environment.
* Applied software-defined networking concepts at the data-plane level.

[View Project](/portfolio/portfolio-2/)

---

## Network Control Applications Using SDN

Developed network-control applications using **Python and Mininet** to explore Software-Defined Networking concepts.

**Technologies:** Python · Mininet · SDN · OpenFlow

* Designed and tested programmable network-control logic.
* Worked with virtual network topologies using Mininet.
* Explored centralized network control and dynamic forwarding behavior.
* Analyzed communication between the control and data planes.

[GitHub](https://github.com/hoangnguyen2809/471_SDN_project)

---

## Simple Network Telemetry Using Remote Procedure Calls

Implemented a network telemetry system using **C++ and Remote Procedure Calls** to collect and exchange information between distributed components.

**Technologies:** C++ · RPC · Networking · Distributed Systems

* Implemented communication between networked client and server components.
* Used RPC mechanisms to exchange telemetry information.
* Explored distributed communication and service-oriented program design.

[GitHub](https://github.com/hoangnguyen2809/471_RPC)

---

## Socket Programming and TCP Tunnel

Implemented TCP client/server applications and tunneling functionality using low-level socket programming in **C**.

**Technologies:** C · TCP/IP · Socket Programming · Linux

* Developed TCP client and server applications using Berkeley sockets.
* Implemented network communication and connection handling.
* Explored TCP tunneling and application-layer data forwarding.
* Gained hands-on experience with low-level network programming.

[GitHub](https://github.com/hoangnguyen2809/TCP-Daytime-Client-and-Server)

---

# Systems & Concurrent Programming

## Multithreaded PageRank

Implemented the PageRank algorithm in **C++** with multithreading to explore parallelism, synchronization, and performance in concurrent applications.

**Technologies:** C++ · Multithreading · Concurrency · Algorithms

* Implemented PageRank computation for graph-based datasets.
* Parallelized workload using multiple worker threads.
* Applied synchronization techniques to safely coordinate shared data.
* Analyzed the effect of concurrency on computational performance.

[GitHub](https://github.com/hoangnguyen2809/page_rank)

---

## Producer–Consumer Multithreading

Implemented the classic producer-consumer synchronization problem using **C++ multithreading**.

**Technologies:** C++ · Multithreading · Synchronization · Concurrency

* Implemented concurrent producer and consumer workers.
* Coordinated access to shared resources using synchronization primitives.
* Explored race conditions, mutual exclusion, and thread coordination.
* Applied operating-system concurrency concepts in a practical implementation.

[GitHub](https://github.com/hoangnguyen2809/producer_consumer)

---

# Software Development

## Chitchat

Developed a networked chat application using **Go**, focusing on client-server communication and concurrent connection handling.

**Technologies:** Go · Networking · Client-Server Architecture · Concurrency

* Implemented communication between multiple networked clients.
* Applied Go concurrency concepts for handling connections.
* Developed core messaging and connection-management functionality.

[GitHub](https://github.com/hoangnguyen2809/Chitchat)

---

## Nuisance Reporting Web Application

Developed a web application for submitting and managing nuisance reports.

**Technologies:** Angular · JavaScript · HTML · CSS

* Built interactive web interfaces using Angular.
* Implemented client-side functionality for creating and managing reports.
* Applied component-based web-development principles.

[GitHub](https://github.com/hoangnguyen2809/Nuisance-Reporting)

