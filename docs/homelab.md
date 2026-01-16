---
sidebar_position: 1
---

# 🏗️ Homelab

Welcome to my homelab. I overengineer solutions at home for sake of practice and learning tools I encounter in a professional settings.

This section is currently under construction (and it will probably stay this way for a long time) as I document my journey building a self-hosted lab for experimenting and ultimately digital self-sufficiency.

I am currently managing a Kubernetes cluster here: [GitHub k3s-ansible](https://github.com/dhkaneda/k3s-ansible).

## Hardware

| Name            |Role              | OS              | Device                           | Processor        | RAM    | Storage              | Vendor      | Cost       |
|-----------------|------------------|-----------------|----------------------------------|------------------|--------|----------------------|-------------|------------|
|zakuii           | Spare            | Arch            | **Lenovo ThinkPad X250**         | Intel i5-5300U   | 8 GB   | 256 GB SSD           | Craigslist  | $80        |
|newtype-1        | Daily Driver     | CachyOS         | **Lenovo ThinkPad T480**         | Intel i5-8350U   | 32 GB  | 512 GB SSD           | eBay        | $300       |
|rx78, mkii, zeta | Kubernetes node  | Arch Linux, k3s | **HP EliteDesk 800 G2** (3x)     | Intel i5-6500T   | 16 GB  | 512 GB SSD           | eBay        | $70 ($210) |
|anaheim          | NAS              | TrueNAS Scale   | **HP Z2 Gen 4 SFF**              | Intel i3-8100    | 16 GB  | 256 GB SSD, 2 TB HDD | Facebook    | $160       |
|minovsky         | Managed Switch   | RouterOS        | **Mikrotik CRS310-8G+2S+IN**     | 98DX226S         | 256 MB | 32 MB                | Amazon      | $200       |
|zz               | GPU Rig          | CachyOS         | **Custom Built** Details below   | AMD Ryzen 5 5500 | 16 GB  | 2 TB SSD             | Newegg      | $810       |
||||||||Total Cost:|$1540|

### GPU Rig

| Component   | Model                                 | Price |
| ------------|---------------------------------------|------|
| Motherboard | MSI B450i motherboard                 | $130 |
| Graphics    | EVGA RTX 3060 12GB                    | $250 |
| Processor   | AMD Ryzen 5 5500                      | $90  |
| RAM         | Corsair Vengeance DDR4 2400MHz 32GB   | $90  |
| Storage     | 2x Crucial BX500 1TB SSDs ($75 each)  | $150 |
| Power       | EVGA 650W B5 PSU                      | $75  |
| Case        | Thermaltake Core V1                   | $50  |
| OS          | Arch Linux: CachyOS                   | $0   |
| Total       |                                       | $835 |
