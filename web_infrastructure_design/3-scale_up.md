https://i.imgur.com/whxo4up.png

# 3. Scale Up

Scaling up the infrastructure for `www.foobar.com` by adding a load balancer cluster and splitting the components onto dedicated servers.

## Components added

- **1 additional server**.
- **1 additional load balancer (HAProxy)**, configured as a **cluster** with the first one.
- **Split components onto dedicated servers**: one server for the web tier (Nginx), one server for the application, one server for the database (MySQL).

## Why each element is added

- **Second load balancer (cluster)**: to remove the **load balancer SPOF** that remained in tasks 1 and 2. The two load balancers form a cluster, typically **Active-Passive** with a floating IP: the active LB receives traffic, the passive one monitors it via a heartbeat and automatically takes over (failover) if the active one fails. The service stays available even if one load balancer dies.
- **Splitting web / application / database onto dedicated servers**: instead of every server doing everything, each component runs on its own machine.

## Why splitting the components matters (separation of concerns)

- **Independent scaling**: each tier (web, application, database) has different resource needs. If the web tier saturates but not the database, you add only web servers without touching the rest. You size each tier according to its own load.
- **Per-role optimization**: each server can be configured and tuned specifically for its task (a database server needs lots of RAM and fast disk; a web server needs to handle network connections well).
- **Fault isolation and maintenance**: a problem on one tier does not directly affect the others, and you can upgrade or maintain one tier without touching the others.

## Specifics to explain

- For every added element, the justification: the second load balancer in a cluster removes the LB SPOF (high availability via failover); dedicated web/application/database servers allow independent scaling of each tier, per-role optimization, and fault isolation.
- **What a load balancer cluster is**: two load balancers working together for high availability, typically Active-Passive with automatic failover if the active one goes down.
