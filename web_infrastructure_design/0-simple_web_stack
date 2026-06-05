https://i.imgur.com/Lb1Se4P.png

# 0. Simple Web Stack

A one-server web infrastructure designed to host a website reachable via `www.foobar.com` (pointing to server IP `8.8.8.8`).

## Components

- **1 server**: a machine that responds to client requests over the network. Here, a single server hosts everything.
- **1 domain name** `foobar.com` with a `www` record (type **A**) pointing to the server IP `8.8.8.8`.
- **1 web server (Nginx)**: the HTTP entry point. It receives requests, serves static files directly, and forwards dynamic requests to the application server.
- **1 application server**: runs the business logic (executes the application code, generates dynamic pages).
- **1 set of application files (codebase)**: the source code the application server runs.
- **1 database (MySQL)**: stores all the persistent data of the application.

## What each element is for

- **Domain name**: a human-readable address that points to an IP, so users don't have to type the IP.
- **`www` DNS record type**: an **A record**, which maps the hostname to an IPv4 address.
- **Communication with the user**: the server communicates with the user's computer over the network using the **HTTP** protocol (carried by TCP/IP).

## Request lifecycle

1. The user types `www.foobar.com`; the browser asks the **DNS**, which resolves the `www` A record to the IP `8.8.8.8`.
2. The browser sends an **HTTP request** to that IP.
3. The **web server (Nginx)** receives it, serves static content directly, and forwards dynamic requests to the **application server**, which runs the code and queries the **database** if needed, then returns the page.

## Issues with this infrastructure

- **SPOF (Single Point Of Failure)**: everything runs on a single server. If it goes down, the whole website is unreachable. Nothing is redundant.
- **Downtime during maintenance**: deploying new code often requires restarting the web/application server, making the site temporarily unavailable.
- **Cannot scale**: a single server has limited capacity (CPU, RAM, QPS). If traffic grows too much, it saturates.
