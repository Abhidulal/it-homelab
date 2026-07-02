# Lab 6 — Virtualisation Concepts and Hyper-V

**Date:** July 2026
**Platform:** Windows Server 2022 Standard Evaluation, UTM on macOS (M3)

---

## Objective

Understand virtualisation concepts including hypervisor types, virtual switches, virtual NICs, and snapshots. Attempt to install Hyper-V on Windows Server 2022.

---

## Key Concepts

### What is Virtualisation

Instead of buying separate physical servers for each role, one powerful server runs multiple virtual machines simultaneously. Each VM operates independently, believing it is a real physical machine, while sharing the underlying hardware resources.

### Hypervisor Types

| Type 1 | Type 2 |
|---|---|
| Runs directly on hardware | Runs on top of an existing OS |
| No underlying OS required | Requires a host OS |
| Used in enterprise datacenters | Used for personal labs |
| Examples: VMware ESXi, Hyper-V | Examples: UTM, VMware Fusion |

### Virtual Switch

A software-based network switch inside the hypervisor that connects virtual machines to each other and to the external network. Functions identically to a physical switch but exists entirely in software.

### Virtual NIC

A software-defined network card assigned to each VM. Behaves like a physical NIC from the VM's perspective but is created and managed by the hypervisor.

### Snapshots

A saved state of a VM at a specific point in time. Allows administrators to roll back to a known good state if a change causes problems. Standard practice before making significant changes to a production server.

### VMware ESXi

The most widely deployed enterprise Type 1 hypervisor. Runs directly on server hardware with no underlying OS. Common in company datacenters worldwide.

### Hyper-V

Microsoft's built-in hypervisor included with Windows Server. Type 1 hypervisor that can also function as a role on Windows Server.

---

## Lab Attempt — Hyper-V Installation

Attempted to install the Hyper-V role via Server Manager on Windows Server 2022.

Result: Installation failed with the following error:

"Hyper-V cannot be installed: The processor does not have required virtualisation capabilities."

### Root Cause

The Windows Server VM is running inside UTM in x86 emulation mode on an Apple M3 Mac. UTM's emulation layer does not expose hardware virtualisation extensions (Intel VT-x) to the guest OS. Hyper-V requires these extensions to function.

This is a known limitation of nested virtualisation in emulation mode — a hypervisor cannot run inside an emulated environment that does not pass through hardware virtualisation capabilities.

### Real World Context

In a real enterprise environment, Hyper-V would be installed on a physical server with Intel VT-x or AMD-V enabled in the BIOS. The process is identical to what was attempted here — install the role via Server Manager, create virtual switches, and deploy VMs through Hyper-V Manager.

---

## What I Learned

Virtualisation is the foundation of modern IT infrastructure. Understanding the difference between Type 1 and Type 2 hypervisors, how virtual switches connect VMs, and why snapshots are critical before changes are all concepts that apply directly to enterprise environments. The failed Hyper-V installation demonstrated a real-world hardware dependency — virtualisation requires CPU extensions that must be explicitly supported and enabled.

## Challenges

| Issue | Resolution |
|---|---|
| Hyper-V installation failed | UTM emulation does not expose hardware virtualisation extensions to the guest OS — hardware limitation, not a configuration error |
