---
title: "Kubernetes: A new frontier for me!"
date: 2025-12-22 16:00:00
categories: [Homelab, Linux]
tags: [arch-linux, linux, homelab, kubernetes]
description: "Week 4 of 16: Learning Kubernetes and deploying a K3s control plane and worker"
toc: true
mermaid: true
---

**See the [Homelab GitHub Repo](https://github.com/aott33/iiot-kubernetes-homelab)**

**Week:** 4 of 16

---

## Introduction

This week was supposed to be about Kubernetes. It became a masterclass in Arch Linux installation troubleshooting instead.

I spent hours debugging an encrypted Arch Linux installation that refused to boot and learned to follow the recommended installation guide.

## Background & Context

This is week 3 of a 16-week journey documenting my Homelab building process. Week 1 established the network foundation with OPNsense and WiFi. Week 2 added VLAN segmentation for OT/IT security. This week was meant to deploy the Kubernetes control plane on my gaming PC, but hardware had other plans.

**Hardware obstacle:** My gaming PC wouldn't power on this week. It tripped the circuit breaker immediately on startup, indicating a power supply or motherboard fault. Rather than delay the project for hardware troubleshooting, I pivoted to using my business PC as the K3s control plane instead.

Before starting this week, I had never installed Arch Linux. I'd used Ubuntu and other Debian-based distros, but Arch's manual installation process was new to me. I chose Arch because of the learning opportunity, it's the foundation for the K3s control plane that will run my IIoT platforms.

**This week's goals:**
1. Install Arch Linux on Business PC (IP: 192.168.20.10)
2. Deploy K3s control plane (server mode) with no workload scheduling
3. Verify control plane health and accessibility
4. Document baseline resource usage

---

These gaps won't block K3s deployment, but filling them will make me more effective at troubleshooting when things break. The Arch Wiki will be my companion for these deep dives.

---

## Summary & Lessons Learned

Week 3 is finally complete. I installed Arch Linux on the business PC after a humbling journey through hardware failure (gaming PC tripped circuit breaker, suspected PSU/motherboard fault), encryption troubleshooting, UEFI quirks, and configuration errors. The K3s control plane deployment moves to Week 4.

**What worked well:**
- **MSI `--removable` flag discovery:** Solved the initial boot failure
- **Switching to the Arch Wiki:** Official documentation is always superior to tutorials for critical infrastructure
- **Fresh start decision:** Knowing when to abandon a broken approach saved hours of additional debugging

**What didn't work:**
- **Following YouTube tutorial blindly:** Encryption added complexity I didn't need yet

**The biggest lesson: Complexity has a cost**

Encryption is valuable. LVM is powerful. But adding them *during initial installation* on a homelab system increased failure modes. I needed a working foundation first.

Next week: K3s control plane deployment (for real this time).

---

## Next Week Preview

**Week 4: K3s Control Plane - Building the Kubernetes Foundation**

With Arch Linux installed and working, I'm finally ready to deploy the Kubernetes control plane. This week I'm tackling:

1. K3s server installation on arch-andy
2. Control plane configuration (no workload scheduling)
3. Verify cluster health with `kubectl`
4. Document baseline resource usage
5. Prepare for worker node addition in Week 5

This is my first time deploying Kubernetes from scratch, so expect some learning moments.

Follow along as we finally get to the Kubernetes part of this Kubernetes homelab project!

---

## Resources & Links

**Arch Linux Documentation:**
- [Arch Linux Installation Guide](https://wiki.archlinux.org/title/Installation_guide): The official guide
- [Arch Linux Download](https://archlinux.org/download/): ISO images and torrents
- [General Recommendations](https://wiki.archlinux.org/title/General_recommendations): Post-installation configuration

**Tutorial I Followed (Initially):**
- [YouTube: Arch Linux Installation Tutorial](https://www.youtube.com/watch?v=FxeriGuJKTM): Good tutorial

**Troubleshooting Resources:**
- [MSI Motherboard Boot Issue](https://bbs.archlinux.org/viewtopic.php?id=283707): UEFI boot quirks
- [GitHub: PiKVM FSTAB Issue](https://github.com/pikvm/pikvm/issues/709): Helped diagnose mount problems

**Homelab Repository:**
- [Homelab Repo](https://github.com/aott33/iiot-kubernetes-homelab)

**Writing Framework:**
- [The Algorithmic Framework for Writing Good Technical Articles](https://www.theocharis.dev/blog/algorithmic-framework-for-writing-technical-articles/)