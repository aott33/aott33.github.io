# Testing out Tentacle PLC

## Table of Contents  
- [Introduction](#introduction)  
    - [What is Tentacle PLC?](#what-is-tentacle-plc)
    - [Why am I testing Tentacle PLC?](#why-am-i-testing-tentacle-plc)
- [Hardware](#hardware)
- [Software](#software)
- [Documentation](#documentation)
- [Installation Steps Taken](#steps-taken)
    - [Ubuntu Set-up](#ubuntu-set-up)
    - [Docker Set-up](#docker-set-up) 
    - [Configure Code Server](#configure-code-server)  
- [PLC Programming Steps](#plc-programming-steps)
- [Summary](#summary)
- [Next Steps](#next-steps)

## Introduction:
### What is Tentacle PLC?
```
Tentacle PLC is a modern Soft PLC runtime with the basic features
of a traditional programmable logic controller platform, but the
process logic is written in Javascript.
```
- Excerpt from [Tentacle PLC Guide](https://www.tentacleplc.com/guide/)

### Why am I testing Tentacle PLC?
- I am taking a full-stack engineering program and learning JavaScript. So, I thought this would be a good opportunity to continue developing my skills while testing out what I believe is a great soft PLC project.

## Hardware:
### Hardware I used:
- [Advantech UNO 2271G](https://www.advantech.com/products/1-2mlj9a/uno-2271g/mod_dc90e0bd-6f2f-47d1-ad72-0e4bd245407d)
- [USB Hub](https://www.staples.com/nxt-technologies-4-port-usb-2-0-hub-nx56850/product_24401668)
- [24Vdc Power Supply](https://www.automationdirect.com/adc/shopping/catalog/power_products_(electrical)/dc_power_supplies/rhino_select_(din_rail)/psb-s_series/psb24-060s)
- USB Flash Drive

## Software:
- [Ubuntu 22.04](https://ubuntu.com/download/desktop)
- Information below is from the Tentacle PLC Guide - [Getting Started Section](https://www.tentacleplc.com/guide/getting-started.html#prerequisites)
    - [Node.js v14+](https://nodejs.org/)
    - [Docker Engine for Linux](https://docs.docker.com/engine/install/ubuntu/)
    - [Docker Compose](https://docs.docker.com/compose/install/) Note: Apparently this is installed with the Docker Engine. Need to confirm that.
    - [Docker Compose file from the Tentacle PLC Repository](https://gitlab.com/joyja/tentacle-plc/-/raw/main/docker-compose.yml?inline=false)

## Documentation:
- [Tentacle PLC Guide](https://www.tentacleplc.com/guide/)

## Steps Taken:
### Ubuntu Set-up:
- Download [Ubuntu 22.04](https://ubuntu.com/download/desktop)
- Follow the [Install Ubuntu desktop tutorial](https://ubuntu.com/tutorials/install-ubuntu-desktop#2-download-an-ubuntu-image)

### Docker Set-up:
- Follow the docker engine installation [tutorial](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)
- [If you already follow the instructions to install Docker Engine, Docker Compose should already be installed](https://docs.docker.com/compose/install/#install-compose-on-linux-systems)

### Container urls:
- Tentacle PLC
    - ```http://localhost:4000```
- Tentacle PLC UI
    - ```http://localhost:3000```
- Code Server
    - ```http://localhost:8080```
    - Note: I got this message when I tried accessing the Code server:
    ![image](https://user-images.githubusercontent.com/48938478/170784283-1dbb2edf-8368-4be3-ba27-79472bd79c64.png)
    - See below for steps taken to [determine code server password](#determine-code-server-password)
### Determine Code Server Password:
- I followed the instructions on this [link](https://www.baeldung.com/ops/docker-container-filesystem#2-spawning-a-shell-in-a-running-container)
- Identify the container id with ```sudo docker ps```:
- Pass /bin/bash as the argument with -it option to docker exec ```docker exec -it [CONTAINER_ID] /bin/bash```:
- Change directory to config.yaml file location ```cd ~/.config/code-server```:
- Print config.yaml file contents to terminal ```cat config.yaml```:

## PLC Programming Steps:
