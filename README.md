# 3-Node Wired ARM64 K3s Edge Computing Cluster

## Project Overview
A bare-metal 3-node Kubernetes (K3s) edge computing cluster running over dedicated Gigabit Ethernet. Built to study distributed container orchestration, enterprise storage handling, and high-availability networking on ARM64 single-board computers.

## Hardware & Power Topology
To prevent USB bus power brownouts during SSD write spikes, each Radxa node uses a dedicated power isolation topology:
- **Compute:** 3x Radxa Zero 3E (8GB RAM preferred, fallback to 4GB)
- **Data Path:** Radxa USB 3.0 Type-C host port -> USB-C to SATA 7-pin data adapter -> 2.5" SATA SSD
- **Power Path:** Central Powered USB Hub (5V/4A+) -> 3x USB-A to SATA 15-pin power cable -> 2.5" SATA SSD
                                                   -> 3x USB-A to USB-C power cable -> 3x Radxa Zero 3E
- **Networking:** Dedicated 5-Port Gigabit Switch + Cat6 patch cables

## System Architecture

             +----------------------------+
             |    Powered USB Hub (5V)    |
             +---+--------------------+---+
                 |                    |
            (SBC Power)          (SSD Power)
                 |                    |
                 v                    v
             +-------+  USB Data  +-------+
             | Radxa |===========>|  SSD  |
             +---+---+            +-------+
                 |
            (Ethernet)
                 |
                 v
             +-------+
             |Switch |
             +-------+

## Software Stack
- **OS:** Debian ARM64
- **Orchestration:** K3s Lightweight Kubernetes
- **Storage:** Local ext4 persistent volume mounts via dedicated SATA adapters
