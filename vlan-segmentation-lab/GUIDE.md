# VLAN Configuration Guide (Cisco IOS)

This guide provides the specific CLI commands and steps to implement VLAN segmentation on an enterprise-grade switch.

## Step 1: Create VLANs in the Database

Enter global configuration mode and define the VLAN IDs and names.

```bash
enable
configure terminal

# Create IT VLAN
vlan 10
name IT

# Create HR VLAN
vlan 20
name HR

# Create Guest VLAN
vlan 30
name GUEST
exit
```

## Step 2: Assign Access Ports

Access ports are used for end-devices like PCs. Each port must be assigned to exactly one VLAN.

```bash
# Assign IT-PC1 to VLAN 10
interface GigabitEthernet0/1
switchport mode access
switchport access vlan 10
description IT_Department_PC

# Assign HR-PC1 to VLAN 20
interface GigabitEthernet0/2
switchport mode access
switchport access vlan 20
description HR_Department_PC

# Assign GUEST-PC1 to VLAN 30
interface GigabitEthernet0/3
switchport mode access
switchport access vlan 30
description Guest_Network_PC
exit
```

## Step 3: Configure Trunking (Uplink)

If you are connecting this switch to another switch or a router, use a trunk port to carry traffic for all VLANs.

```bash
interface GigabitEthernet0/24
switchport mode trunk
switchport trunk allowed vlan all
description Uplink_to_Core_Switch
exit
```

## Step 4: Verification Commands

Use these commands to verify your configuration is correct.

```bash
# View all VLANs and assigned ports
show vlan brief

# View status of a specific interface
show interface GigabitEthernet0/1 switchport

# View trunk status
show interfaces trunk
```

## Troubleshooting Tips

*   **Native VLAN Mismatch**: Ensure both ends of a trunk link have the same native VLAN (default is VLAN 1).
*   **VLAN Not in Database**: If a port is assigned to a VLAN that hasn't been created, traffic will be dropped.
*   **Access vs Trunk**: Never use `switchport mode trunk` for an end-user PC; this is a security risk.

---

## Author
**Manus AI**
