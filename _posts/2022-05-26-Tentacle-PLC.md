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
> Tentacle PLC is a modern Soft PLC runtime with the basic features of a traditional programmable logic controller platform, but the process logic is written in Javascript.

Excerpt from [Tentacle PLC Guide](https://www.tentacleplc.com/guide/)

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
    - See [Determine Code Server Password](#determine-code-server-password) to learn how I retrieved the password

### Determine Code Server Password:
- I followed the instructions on this [link](https://www.baeldung.com/ops/docker-container-filesystem#2-spawning-a-shell-in-a-running-container)
- Identify the container id with ```sudo docker ps```:
- Pass /bin/bash as the argument with -it option to docker exec ```docker exec -it [CONTAINER_ID] /bin/bash```:
- Change directory to config.yaml file location ```cd ~/.config/code-server```:
- Print config.yaml file contents to terminal ```cat config.yaml```:
- Copy the password from the terminal

## PLC Programming Steps:
- The goal was to get a simple PLC program working and connect to a MQTT broker.
- After the installation and configuration, it didn't take long to achieve this goal
- To start programming I went to the code-server url ```http://localhost:8080``` and enter password. See [determine code server password](#determine-code-server-password)
- Followed the [Tentacle PLC Guide](https://www.tentacleplc.com/guide/directory-structure.html#the-runtime-directory) to create a test program

### Configure config.json:
- I copied the example [config.json](https://www.tentacleplc.com/guide/directory-structure.html#the-config-json-file) code in the Tentacle PLC Guide
- I modified the MQTT configuration and put in the credentials for my HiveMQ Cloud Broker
- I left the modbus section empty as I don't have a modbus device at the moment. This section is for configuring the connection to a Modbus Remote IO device

### Configure variable.json:
- I copied the example [variables.json](https://www.tentacleplc.com/guide/variables.html#variables-json) code in the Tentacle PLC Guide
- I commented out the modbus variables and motor variable (lines 14-85) as I don't have a modbus device connected

### Configure programs:
- I created 2 files:
    - main.js
    - secondary.js

### Files used in program:
- [config.json](https://github.com/aott33/tentacle-plc-testing/blob/main/config.json)
- [variables.json](https://github.com/aott33/tentacle-plc-testing/blob/main/variables.json)
- [main.js](https://github.com/aott33/tentacle-plc-testing/blob/main/main.js)
- [secondary.js](https://github.com/aott33/tentacle-plc-testing/blob/main/secondary.js)

### Summary:
- The initial installation and configuration took the most time as I am not too familiar with docker
    - After this project, I am definitely more confident but have much more to learn
    - I am sure others will find the configuration process quite straightforward
- After the installation and setup, the programming portion was very nice and straightforward
- The PLC programming portion is well documented and I was able to get a simple PLC program running
- I like the GraphQL API capabilities
    - The PLC stopped running and I was able to restart the PLC using the `restartPlc` [GraphQL mutation](https://www.tentacleplc.com/guide/graphql.html#mutation)
- I like that each variable in `variables.json` is published using the Sparkplug B specification
    - [MQTT Variables section from Tentacle PLC](https://www.tentacleplc.com/guide/mqtt.html#variables)

### Next Steps:
- Integrate the Tentacle PLC with [Ignition](https://inductiveautomation.com/)
- Create a more complex PLC program for a real world application
- Connect Remote IO
 
