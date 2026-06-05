https://i.imgur.com/Nxm5Dlt.png

# 1. Distributed Web Infrastructure

A three-server web infrastructure hosting `www.foobar.com`, adding a load balancer and a second server.

## Components added

- **1 load balancer (HAProxy)**: the DNS now points to the load balancer's public IP.
- **2 identical servers**, each containing: a web server (Nginx), an application server, the application files (codebase), and a database (MySQL).

## Why each element is added

- **Load balancer**: distributes incoming traffic across the two servers. It solves scaling (more traffic absorbed) and improves availability (if one server is down, the LB sends everything to the other).
- **Second server**: redundancy and capacity. Two identical servers process requests in parallel (more throughput) and tolerate a failure.

## Specifics to explain

- **Distribution algorithm — Round Robin**: the load balancer distributes requests in turn, one to each server, in a loop (request 1 -> server 1, request 2 -> server 2, request 3 -> server 1...). Simple and evenly balanced.
- **Active-Active vs Active-Passive**: this setup is **Active-Active** — both servers work at the same time, the LB spreads traffic across both. In **Active-Passive**, only one server (active) receives traffic while the other (passive) stays on standby and takes over only if the active one fails. Active-Active = more capacity; Active-Passive = high availability without extra capacity.
- **MySQL Primary-Replica (Master-Slave) cluster**: the **Primary** is the only node that accepts **writes** (INSERT/UPDATE/DELETE). On each write it sends its changes to the **Replicas** (replication), which stay an up-to-date copy.
- **Primary vs Replica from the application's point of view**: the application **writes only to the Primary** (read/write) and can **read from the Replicas** (read only). The Replica never receives a direct write from the application, only the replication coming from the Primary.

## Issues with this infrastructure

- **SPOF**: the **load balancer is a single point of failure** (only one). If it goes down, nothing reaches the servers. The Primary database is also a SPOF for writes.
- **Security issues**: no firewall (infrastructure exposed), no HTTPS (data travels in clear text).
- **No monitoring**: nothing watches the infrastructure, so problems are discovered too late.
