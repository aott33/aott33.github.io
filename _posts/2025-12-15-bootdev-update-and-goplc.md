---
title: "Why Boot.dev Works and What I'm Building Next"
date: 2025-12-15 16:00:00
categories: [Backend, Learning]
tags: [golang, bootdev, plc, industrial-automation]
description: "I hit Archmage on Boot.dev. Here's why the platform works and what I'm building for my capstone project."
toc: true
---

Most coding platforms promise to teach you programming but leave you stuck in tutorial hell. Boot.dev actually got me building real projects. Now I'm taking everything I learned and applying it to a capstone that solves a real problem I've faced in my career.

This post covers three things: 

1. Why Boot.dev's approach to teaching code works
2. Where I am in the program
3. What I'm building for my capstone project

## The Problem with Learning to Code Online

I've tried multiple platforms over the years. The pattern is always the same: watch videos, follow along, get a certificate, and then realize you can't actually build anything on your own. The gap between tutorial code and production code is massive.

Boot.dev does something different. Their [philosophy](https://blog.boot.dev/about/#our-beliefs) centers on a few key ideas that resonated with me:

- **Coding is fun, don't ruin it** - Learning should feel like a game, not a chore
- **You need to build** - Reading code is not the same as writing code
- **You need to deploy** - Real projects that run on real systems
- **It's a marathon, not a sprint** - Depth over breadth

These aren't just marketing statements. The entire platform is designed around them.

## What Makes Boot.dev Different

Three things set Boot.dev apart from other platforms.

### Learn by Doing

Every course includes hands-on projects. Not toy examples, but actual applications you build from scratch. The CLI projects were my favorites. Building a Pokedex CLI and a Blog Aggregator forced me to understand how command-line tools actually work under the hood.

The pattern is always the same: learn a concept, immediately apply it, struggle a bit, figure it out. That struggle is where the learning happens. At the end of the program, you complete a capstone project that goes into your portfolio. I'll cover mine below.

### Great Community

The Discord server is genuinely useful. When I got stuck on a project, I could search for others who had the same problem. The community is active and helpful without being condescending. It's one of the better technical Discord communities I've been part of.

### Gamification That Works

Boot.dev treats the learning experience like an RPG. You earn XP for completing lessons, maintain streaks for daily practice, unlock roles every 10 levels, and get bonuses for various achievements. At level 100, you reach Archmage status and Boot.dev sends you a physical coin.

Sounds gimmicky, but it works. The gamification creates genuine motivation to keep learning. I found myself coming back daily just to maintain my streak, and that consistency compounded into real skill development.

## Current Progress

I hit Archmage this week. Here's where I stand:

![bootdev-achievements](https://github.com/user-attachments/assets/3c54a350-1e4c-4fcf-8a28-89cc6f5f0def)
_Level 100 Archmage - 1,464 lessons completed across 20 courses_

I have three more courses and the capstone project remaining. My estimate is completion by end of January.

**Profile:** [boot.dev/u/aott33](https://www.boot.dev/u/aott33)

## Capstone Project: GoPLC

For my capstone, I wanted to build something that solves a real problem, demonstrates the skills I learned, and is something I would actually use.

I work in industrial automation. The tools we use for programming PLCs (Programmable Logic Controllers) are decades behind modern software development. Proprietary IDEs, vendor lock-in, slow iteration cycles, no version control. It's painful.

GoPLC is a soft PLC written in Go. Instead of using proprietary languages like Ladder Logic or Structured Text, you write control logic in Go and use modern development tools: VS Code, Git, CI/CD pipelines.

### What is a Soft PLC?

A traditional PLC is a specialized industrial computer that runs control logic for manufacturing equipment. A soft PLC does the same thing but runs on standard hardware. Think of it as the control software without the proprietary box.

### Why Build This?

The inspiration comes from [Tentacle PLC](https://joyautomation.com/software/packages/tentacle), which proved that modern programming languages can replace IEC 61131-3 standards. GoPLC takes that philosophy and implements it in Go to test a different language.

The core idea: if you have to learn vendor-specific languages anyway, you might as well learn a real programming language that transfers to other domains.

### Project Architecture Plan

GoPLC communicates with industrial devices via standard protocols and exposes data through multiple interfaces:

**Input/Output:**
- Modbus TCP/RTU for connecting to sensors, actuators, and I/O modules
- Automatic reconnection with exponential backoff

**SCADA Integration:**
- OPC UA server for traditional industrial SCADA systems
- Sparkplug B over MQTT for modern IIoT architectures
- GraphQL API for custom integrations

**Monitoring:**
- Built-in WebUI for real-time monitoring and control
- System health, connection status, variable values, task execution

### Early UX Designs

Here are the initial mockups for the monitoring interface:

<img alt="ux-overview" src="https://github.com/user-attachments/assets/0eb6b3b6-ed02-4c3f-b668-a01ae0b6646b" />
<img alt="ux-compact" src="https://github.com/user-attachments/assets/5f085881-8d82-4e0d-8fcf-11671883bb62" />
_Main dashboard showing sources, tasks, system health, and variable values_

The interface is designed for operators who need to quickly assess system status. The alert bar at the top shows active errors and warnings. Sources and tasks display real-time connection and execution status.

<img alt="ux-alerts-expanded" src="https://github.com/user-attachments/assets/16552658-80ff-49ac-b4d1-f400dc25373c" />
<img alt="ux-alerts-compact" src="https://github.com/user-attachments/assets/decc1873-71ce-4b54-b40c-c0d827ac25f6" />
_Alert system showing connection errors and value warnings with timestamps_

Error messages are human-readable. No cryptic codes or hex values. When something fails, you can see exactly what happened and when.

### Technical Implementation

The project uses several Go libraries that handle the heavy lifting:

- `simonvetter/modbus` for Modbus TCP/RTU communication
- `gopcua/opcua` for OPC UA server
- `eclipse/paho.mqtt.golang` for MQTT (Will need to develop Sparkplug B portion)
- `99designs/gqlgen` for GraphQL

Variables are defined once in YAML and automatically exposed to all protocols. Write your control logic in native Go, and the runtime handles scheduling, scaling, and protocol translation.

### Reference Implementation

The validation case is a real tank battery control system I previously built on Allen-Bradley CompactLogix. If GoPLC can handle tank level monitoring, pump sequencing, and alarm logic with faster iteration cycles and cleaner code, the project succeeds.

### Current Status

GoPLC is currently in the planning stage. I've completed the PRD, UX design specifications, and architecture documentation. Development starts next week. I'll be posting updates as the project progresses.

**GitHub:** [github.com/aott33/go-plc](https://github.com/aott33/go-plc)

---

## Resources

**Boot.dev**
- [Get Started for Free](https://www.boot.dev/)
- [About Section](https://blog.boot.dev/about/)
- [GitHub Repository - Curriculum](https://github.com/bootdotdev/curriculum)

**GoPLC**
- [GitHub Repository](https://github.com/aott33/go-plc)

**Tentacle PLC**
- [Tentacle PLC Docs](https://joyautomation.com/software/packages/tentacle)
- [GitHub Repository](https://github.com/joyautomation/tentacle)

**Writing Framework**
- [The Algorithmic Framework for Writing Good Technical Articles](https://www.theocharis.dev/blog/algorithmic-framework-for-writing-technical-articles/)
