# Spanning Tree Protocol (STP) Configuration Guide

This guide provides the specific CLI commands and steps to configure and manage **Spanning Tree Protocol (STP)** on an enterprise-grade switch.

## Step 1: Configure the Root Bridge

By default, the bridge priority is 32768. To ensure a specific switch becomes the root bridge, lower its priority (in increments of 4096).

```bash
enable
configure terminal

# Set SW1 as the primary Root Bridge for VLAN 1
spanning-tree vlan 1 priority 4096
exit
```

## Step 2: Verify Root Bridge Election

Use the `show spanning-tree` command to confirm that the switch has been elected as the root.

```bash
# Verify STP status
show spanning-tree

# Look for the line: "This bridge is the root"
```

## Step 3: Identify Port Roles and States

Understand how STP has organized the physical links to prevent loops.

```bash
# Check port roles (Root, Designated, Alternate) and states (FWD, BLK)
show spanning-tree brief
```

*   **Root Port (RP)**: The port on a non-root switch with the lowest path cost to the root.
*   **Designated Port (DP)**: The port on a segment that provides the best path to the root.
*   **Alternate Port (Altn)**: A blocked port that provides a redundant path.

## Step 4: Configure PortFast for Edge Devices

PortFast allows access ports to bypass the listening and learning states, transitioning immediately to forwarding.

```bash
interface FastEthernet0/1
spanning-tree portfast
description PC_Connection_Port
exit
```

**⚠️ Warning**: Only enable PortFast on ports connected to end-devices (PCs, printers). Enabling it on ports connected to other switches can cause network loops.

## Step 5: Implement BPDU Guard

To enhance security, enable BPDU Guard on PortFast-enabled ports. This will shut down the port if a BPDU (sent by another switch) is received, preventing unauthorized switches from being added.

```bash
interface FastEthernet0/1
spanning-tree bpduguard enable
exit
```

## Troubleshooting Commands

*   `show spanning-tree summary`: Provides a high-level view of STP states.
*   `show spanning-tree detail`: Detailed information about BPDUs and timers.
*   `debug spanning-tree events`: Real-time monitoring of STP transitions (use with caution on live networks).

---

## Author
**Manus AI**
