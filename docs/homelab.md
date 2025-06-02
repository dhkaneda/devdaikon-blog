---
sidebar_position: 1
---

# 🏗️ Homelab

Welcome to my Homelab—a digital playground where enterprise tools meet curiosity.

This section is currently under construction as I document my journey building a self-hosted environment for experimentation, learning, and digital self-sufficiency. From Kubernetes clusters to secrets management, network design to monitoring stacks, this is where I test, break, and rebuild it all. I am currently managing a Kubernetes cluster here: [GitHub k3s-ansible](https://github.com/dhkaneda/k3s-ansible).

Check back soon for posts on:

- My current homelab architecture
- How I’m using GitOps, Kubernetes, and Terraform at home
- Ideas on minimal, portable, and off-grid computing

## Hardware

| Name            |Role              | OS              | Device                           | Processor        | RAM    | Storage              | Vendor      | Cost       |
|-----------------|------------------|-----------------|----------------------------------|------------------|--------|----------------------|-------------|------------|
|zakuii           | Spare            | Kali Linux      | **Lenovo ThinkPad X250**         | Intel i5-5300U   | 8 GB   | 256 GB SSD           | Craigslist  | $80        |
|newtype-1        | Daily Driver     | Manjaro         | **Lenovo ThinkPad T480**         | Intel i5-8350U   | 32 GB  | 512 GB SSD           | eBay        | $80        |
|rx78, mkii, zeta | Kubernetes node  | Arch Linux, K3s | **HP EliteDesk 800 G2** (3x)     | Intel i5-6500T   | 16 GB  | 512 GB SSD           | eBay        | $70 ($210) |
|anaheim          | NAS              | Trunas Scale    | **HP Z2 Gen 4 SFF**              | Intel i3-8100    | 16 GB  | 512 GB SSD, 4 TB HDD | Facebook    | $160       |
|minovsky         | Managed Switch   | RouterOS        | **Mikrotik CR310-8G+2S+IN**      | 98DX226S         | 256 MB | 32 MB                | Amazon      | $200       |
|zz               | GPU Rig          | Windows 10 Pro  | **Custom Built** Details below   | AMD Ryzen 5 5500 | 16 GB  | 2 TB SSD             | Newegg      | $810       |
||||||||Total Cost:|$1540|

### GPU Rig

| Component   | Model                                 | Price |
| ------------|---------------------------------------|------|
| Motherboard | MSI B450i motherboard                 | $130 |
| Graphics    | EVGA RTX 3060 12GB                    | $250 |
| Processor   | AMD Ryzen 5 5500                      | $90  |
| RAM         | Corsair Vengeance DDR4 2400MHz 16GB   | $50  |
| Storage     | 2x Crucial BX500 1TB SSDs ($75 each)  | $150 |
| Power       | EVGA 650W B5 PSU                      | $75  |
| Case        | Thermaltake Core V1                   | $50  |
| OS          | Windows 10 Pro license                | $15  |
| Total       |                                       | $810 |
