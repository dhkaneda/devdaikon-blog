---
sidebar_position: 1
---

# 🏗️ Homelab

Welcome to my overengineered homelab, centered around running Kubernetes on a cluster of repurposed hardware.

This section is constantly in flux and may be out of date, but I try to keep it updated as much as possible.

I am currently managing a Kubernetes cluster here: [GitHub k3s-ansible](https://github.com/dhkaneda/k3s-ansible).

## Hardware

| Name            |Role              | OS              | Device                           | Processor        | RAM    | Storage              | Vendor      | Cost       |
|-----------------|------------------|-----------------|----------------------------------|------------------|--------|----------------------|-------------|------------|
<!-- |zakuii           | Spare            | Arch            | **Lenovo ThinkPad X250**         | Intel i5-5300U   | 8 GB   | 256 GB SSD           | Craigslist  | $80        | -->
<!-- |newtype-1        | Daily Driver     | CachyOS         | **Lenovo ThinkPad T480**         | Intel i5-8350U   | 32 GB  | 512 GB SSD           | eBay        | $300       | -->
|newtype-2        | Daily Driver     | MacOS Tahoe     | **MacBook Air M4 (2025)**        | Apple M4         | 24 GB  | 512 GB SSD           | Apple       | $1100      |
|rx78, mkii, zeta | Kubernetes node  | Arch Linux, k3s | **HP EliteDesk 800 G2** (3x)     | Intel i5-6500T   | 16 GB  | 512 GB SSD           | eBay        | $70 ($210) |
|zz               | GPU Node         | Ubuntu 24.04    | **Custom Built** Details below   | AMD Ryzen 5 5500 | 32 GB  | 256 GB SSD           | Newegg      | $810       |
<!-- |anaheim          | NAS              | TrueNAS Scale   | **HP Z2 Gen 4 SFF**              | Intel i3-8100    | 16 GB  | 256 GB SSD, 2 TB HDD | Facebook    | $160       | -->
|minovsky         | Managed Switch   | RouterOS        | **Mikrotik CRS310-8G+2S+IN**     | 98DX226S         | 256 MB | 32 MB                | Amazon      | $200       |

### GPU Rig

| Component   | Model                                 | Price |
| ------------|---------------------------------------|------|
| Motherboard | MSI B450i motherboard                 | $130 |
| Graphics    | EVGA RTX 3060 12GB                    | $250 |
| Processor   | AMD Ryzen 5 5500                      | $90  |
| RAM         | Corsair Vengeance DDR4 2400MHz 32GB   | $90  |
<!-- | Storage     | 2x Crucial BX500 1TB SSDs ($75 each)  | $150 | -->
| Storage     | Samsung EVO 256 GB SSD                | Spare |
| Power       | EVGA 650W B5 PSU                      | $75  |
| Case        | Thermaltake Core V1                   | $50  |
| OS          | Ubuntu Server 24.04                   | $0   |
| Total       |                                       | $685 |
