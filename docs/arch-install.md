---
slug: arch-guided-install
title:  "Arch Guided Install"
tags: [ linux, arch ]
sidebar_position: 2
---

[Get right to the guide](#booting-into-the-arch-linux-installation-medium)

## Why another guide?

At first, I didn’t think I needed a personal guide—after all, the Arch Wiki is famously well-written, and I’ve grown comfortable navigating it after installing Arch many times over the years. But with each installation, I learned something new. Sometimes that knowledge came from making mistakes and troubleshooting. Other times, it came from discovering better configurations, new packages, or improved security practices.

This guide is a culmination of those lessons: what I installed, why I chose it, in what order, and with what configuration. If it helps you—great! But primarily, it serves as a reference for my future self to save time and avoid past pitfalls.

Let’s be honest: installing Arch Linux is often described as a “pilgrimage”—and I agree. The process is intentionally difficult because it forces you to learn what’s going on under the hood. But the learning curve can be steep, and many users bounce off because of it. That’s okay. There are countless excellent guides out there, both written and on YouTube.

Still, I believe beginners can benefit from annotated, opinionated guides like this one. You might not grasp everything right away, and that’s fine. Like me, you may go through several installs over the years. And eventually, it’ll all click. Until then, every attempt is part of the process.

If your goal is to really learn Linux, I still recommend using the Arch Wiki as your primary source. But don’t install Arch as your only daily driver—at least not at first. Use a second machine or keep a backup OS ready. Take your time. Block off a week if you need to.

I created this guide to consolidate scattered notes, commands, and docs into one place tailored to my setup. I suggest you do the same. Everyone’s Arch install is different—and that’s the beauty of it.

## LVM on LUKS

I am particularly interested in using **LVM on LUKS** for my Arch Linux installation. This allows me to encrypt my entire disk while also providing the flexibility of logical volume management. I'd like to practice system configurations similar to what I may see in enterprise settings and was made aware the importance of encryption and flexibility by devs like [Mischa van den Burg](https://www.youtube.com/watch?v=qboMuv9vSpQ). I was also recently made aware of other filesystem options like **Btrfs** and **ZFS**, and I am also very interested in using those in the future. At this time, my focus is on working with Kubernetes and need to get up and running as quickly as possible. I'll be revisiting this guide in the future to explore those options.

---

## Booting into the Arch Linux Installation Medium

### Create an installation medium on a USB Drive

Pick a USB Drive (8–128 GB):

- Your mileage may vary, but choose a reputable brand for fewer issues and hardware QA variance (SanDisk, Kingston, etc.).
- Size: At least 4 GB (Arch ISO is ~1 GB).

### ⚠️ Problem: Larger USB Drives

USB drives larger than 32 GB are quite common these days. I had somehow lost all of my sub-64 GB drives over the years and did not wait to wait for a new one to arrive. I only had a 64 GB drive that I had lying around.

I read that it's recommended to use a USB stick 32 GB or smaller because many systems—especially older ones—expect bootable USBs to use the FAT32 file system.

FAT32 has a maximum volume size of 32 GB (officially on Windows), and larger drives may be formatted as exFAT or NTFS, which are not always bootable or recognized by all firmware.

### Solution: Use Ventoy

**Instead of using Rufus, Etcher, or dd to flash the ISO.*

**Ventoy** handles large drives (64 GB, 128 GB+) without compatibility issues. Even if you're going to use a 32 GB USB drive, Ventoy is a great tool for creating bootable USB drives.

<details>
<summary> Why This Bypasses the 32 GB Limitation </summary>

1. No reliance on FAT32 for the bootable ISO itself
    - Ventoy handles booting via its own partition and tools, so the USB’s size or file system type doesn't block bootability. Even if the main partition is exFAT or NTFS, Ventoy can still boot the ISOs.

1. UEFI/BIOS compatibility built-in
    - Ventoy is designed to work with both legacy BIOS and modern UEFI systems, removing guesswork about formatting or partitioning.

1. No need to format for each new ISO
    Just drag and drop ISO files onto the USB. Want Arch, Ubuntu, and Kali on the same stick? Done.

</details>

### Ventoy Installation and using the Arch Linux ISO

1. **Download** the latest version of Ventoy from the [official website](https://www.ventoy.net/en/download.html).
1. **Download** the latest Arch Linux ISO from the [Arch Linux website](https://archlinux.org/download/).
1. Use this guide to get started with Ventoy: [Ventoy Quick Start](https://www.ventoy.net/en/doc_start.html).
1. **Drag and drop** it onto the Ventoy USB—no flashing required.

### Booting into Ventoy with Arch ISO – Quick Steps

1. **Plug in your Ventoy USB** (with the Arch ISO copied to it).

1. **Power on the target computer**, (if it boots into Ventoy, skip to step 4, otherwise reboot) and immediately press the BIOS/boot menu key:  
   - Common keys: `ESC`, `F2`, `F12`,`F10`, or `DEL`, (depends on the manufacturer).  
   - You may need to **log in to the BIOS** if it's locked.
   - If you cannot, get into the BIOS, hopefully you can boot into an OS, disable fast boot, and check to make sure your keyboard is detected at startup (don't use dongles or USB extensions).

1. **Enter the Boot Menu** or change **Boot Order** to select your USB drive.

1. **Disable Secure Boot** and **Disable Legacy Boot Mode** to enable UEFI mode.

1. Save your settings and reboot.

1. **Ventoy boots up**, showing a menu of ISOs—select the Arch Linux ISO and press Enter.

### ⚠️ UEFI & Secure Boot Notes

- **Ventoy works in both UEFI and Legacy BIOS**—no need to enable CSM or Legacy Mode.  
- It also supports **Secure Boot**, but setup requires enrolling a custom key:
  - This is **for advanced users only**—misconfiguration could brick or lock your system.

Once you select the Arch ISO from Ventoy’s menu, it’ll boot into the Arch installer. You’re now ready to begin the installation!

### Ventoy's Normal or GRUB2 mode?

Some systems—especially certain UEFI firmwares—may not boot certain ISOs properly in Ventoy’s default mode. GRUB2 mode can serve as a fallback if:

- You get a blank screen or failed boot in normal mode.
- Your system has strict Secure Boot settings or weird UEFI behavior.

---

## Installing Arch Linux

### Verify Boot Mode

Run the following command to check if you're in **UEFI mode**:

```bash
ls /sys/firmware/efi
```

- If the directory exists, you're in **UEFI mode** (recommended).
- If not, you're in **BIOS mode**, and you may need to tweak your firmware settings.

---

## (Optional) Continue Installation via SSH

You may want to continue your installation of Arch via SSH for a variety of reasons:

- If you want to record your commands, it is easier to do so from a terminal on another computer.
- You might want to copy commands from this cookbook or the Arch wiki as part of your installation.
- You want to sit back on a couch or bed with your laptop if you are setting up Arch on a desktop PC.

If you won't be using SSH, skip to the next section [Partitioning](#partitioning).

If you've chosen to SSH, follow these next steps for [Ethernet](#ethernet) or [Wifi](#wi-fi-using-iwd-and-iwctl).

### Ethernet

1. Ensure your machine is connected to the network via Ethernet. This is preferred for initial installation, especially if you need internet access to install packages.

1. Plug in a working Ethernet cable if you haven’t already.

1. Check if your system has received an IP address and has a default route:

    ```bash
    ip route
    ```

    Example Output:

    ```nginx{.line-numbers}
    default via 10.0.0.1 dev eno1 proto dhcp src 10.0.0.161 metric 100
    10.0.0.0/24 dev eno1 proto kernel scope link src 10.0.0.161 metric 100
    10.0.0.1/dev eno1 proto dhcp scope link surc 10.0.0.161 metric 100
    ```

With this output, the machine intended for Arch installation can be reachable on the network at the IP address `10.0.0.161`.

<details>
<summary> ip route Output Explained </summary>

**DHCP (Dynamic Host Configuration Protocol)** is a network protocol that automatically assigns IP addresses and other network settings (like gateway and DNS) to devices, so you don’t have to configure them manually.

`default via 10.0.0.1 dev eno1 proto dhcp src 10.0.0.161 metric 100`

- **default**: This is your default route — used for traffic to IPs not in your local subnet (i.e. general internet).
- **via 10.0.0.1**: Your **default gateway** — usually your router.
- **dev eno1**: The interface used for this route (your Ethernet interface).
- **proto dhcp**: This route was configured by **DHCP**.
- **src 10.0.0.161**: This is the IP **assigned to your device** by the DHCP server.
- **metric 100**: Route priority — lower is preferred; 100 is standard.
- This line tells the system: *“For all unknown destinations, send packets to 10.0.0.1 through eno1.”*

`10.0.0.0/24 dev eno1 proto kernel scope link src 10.0.0.161 metric 100`

- **10.0.0.0/24**: The **local network subnet** — 10.0.0.x range with subnet mask `255.255.255.0`.
- **proto kernel**: This route was added automatically by the Linux kernel.
- **scope link**: Route is valid **only on this link/local network**.
- **src 10.0.0.161**: Your system’s IP in this subnet.
- **metric 100**: Same as above.
- This line tells the system: *“I can directly reach other 10.0.0.x machines via eno1.”*

`10.0.0.1 dev eno1 proto dhcp scope link src 10.0.0.161 metric 100`

- **10.0.0.1**: A specific route to your **gateway/router**.
- **dev eno1**: Goes through your Ethernet interface.
- **proto dhcp**: Added by DHCP.
- **scope link**: This IP is directly reachable (on the same subnet).
- **src 10.0.0.161**: Your IP.
- **metric 100**: Route priority.
- This line explicitly defines how to reach the router — even though it’s on the local subnet, the DHCP client sets this route just to be safe.

Altogether:

- You have a valid IP from the DHCP server: `10.0.0.161`.
- Your gateway/router is `10.0.0.1`.
- Your machine knows how to:
  - Route **local traffic** on the 10.0.0.x subnet.
  - Route **internet traffic** via 10.0.0.1.

If this explanation is still foreign, you may need to be primed on some of these concepts:

<details>
<summary> Networking crash course</summary>

**What *Is* a Network?**

A **network** is just a group of devices (computers, phones, printers, etc.) that can talk to each other. Your **home network** is a small, private version of the internet—just for your house.

**What Is a Router?**

A **router** is like your home's post office. It sits between your private network and the public internet. Its job is to:

- Assign internal addresses (like 10.0.0.12) to your devices.
- Route traffic between your devices and the internet.
- Keep your devices safely hidden behind a single public IP.

**Why 10.0.0.0?**

IP addresses like **10.0.0.0/24** come from a special range reserved for private networks (same with 192.168.x.x and 172.16.x.x). The **10.0.0.0/24** subnet gives you 256 usable addresses (10.0.0.1 to 10.0.0.254). The `/24` is a **subnet mask** and is a special notation that relies on the binary octet that comprise an IP address to specify a **range**

You’ll often see:

- **10.0.0.1** — Your router’s internal IP address (the "gateway")
- **10.0.0.x** — Other devices on the network

Why Does It Matter?

When your computer connects via Ethernet or Wi-Fi, the **DHCP server** (usually your router) gives it an IP address like 10.0.0.161, so it knows how to send and receive data inside your home network and to the outside world.

</details>

</details>

### Wi-Fi (using `iwd` and `iwctl`)

1. **Check to see if `iwd` is running.**
You can quickly check running services using `systemctl` or get a simplified view of active processes with `pstree`.

    ```bash
    systemctl status iwd
    ```

    ```bash
    pstree
    ```

1. **Start the iwd service** (if it's not already running):

    ```bash
    systemctl start iwd
    ```

1. **Launch the interactive Wi-Fi management tool:**

    ```bash
    iwctl
    ```

1. **Inside the `iwctl` prompt**, follow these steps:

    ```sh
    device list                   # List wireless devices (e.g., wlan0)
    station wlan0 scan           # Replace wlan0 with your device name
    station wlan0 get-networks   # View available networks
    station wlan0 connect <SSID> # Connect to your Wi-Fi (quotes needed if SSID has spaces)
    ```

1. **Exit the `iwctl` prompt:**

    ```sh
    exit
    ```

1. **Verify connection:**

    ```bash
    ping archlinux.org
    ```

1. **Persist auto-connection with `iwd`**
If you need to make this persistent after install, enable the service:

    ```bash
    systemctl enable iwd
    ```

    - `iwd` will auto-connect to saved networks after reboot.

### Connect to your target machine via SSH

1. Check to see if the OpenSSH daemon is running. It should be included on the Arch Installation Live medium.

    ```bash
    systemctl status sshd
    ```

1. Set up a root user password

    ```bash
    passwd
    ```

1. From another machine, connect via SSH using the IP address you saw earlier:

    ```bash
    ssh root@10.0.0.161
    ```

## Partitioning

Partitioning is often the most intimidating part of the Arch installation process—and for good reason. This is where you define how your disk will be sliced up, where the bootloader will go, and what format your system will rely on for everything from startup to storage. If you get it wrong, you might end up overwriting data or with an unbootable system.

But it’s also one of the most crucial steps. Partitioning correctly ensures your system is aligned with modern boot standards (like UEFI with GPT), allows for features like encryption or multiple OSes, and gives you control over how your system is structured.

In this section, we’ll walk through:

- Checking if your disk is using GPT

- Creating an EFI System Partition

- Setting up root, and optionally, swap or home partitions

Take your time here. Understand each command before running it. When in doubt, stop and look it up.

### Check Existing EFI Partition

1. List your disks and partitions:

    ```bash
    lsblk
    fdisk -l <disk>
    ```

    If you've got multiple disks on your system, look for the one with the size matching the drive you wish to install your operating system on. For example, the 512 GB SSD I want to install Arch on is labelled as `/dev/sda`

2. Ensure the disk is using **GPT (GUID Partition Table)**. If it's **MBR**, you’ll need to convert it. Use the following command to see more about that particular drive. You may also see a disk be labelled `dos`, this means the disk is using the **MBR** partitioning scheme.

    ```bash
    fdisk -l <disk>
    ```

### Create EFI Partition

Before we can install and boot into Arch Linux, we need a special partition for the system firmware: the EFI System Partition (ESP). This small partition—typically around 300MB to 1GB—is required for systems using UEFI (Arch Wiki suggests 1 GB), and it holds the bootloader and related files that initialize the OS. Without it, UEFI firmware won’t know how to start the system. In this step, we’ll create or identify an EFI partition to ensure your system can boot correctly.

1. If the disk is using **MBR**, convert it to **GPT** using `fdisk`:

 ```bash
 fdisk /dev/sda
 ```

- Press `g` to create a new **GPT partition table** (**Warning: This will erase all data**).

1. Create a new **EFI system partition**:

    - Press `n` (new partition).
    - Select partition number **1**.
    - First sector: **Default** (`2048`).
    - Size: **+1G**.
    - Confirm removal of vfat signature if prompted.
    - Press `t` to change type → **EFI System (1 or ef00)**.
    - Press `w` to write changes.

### Create the LVM root partition

With the EFI system partition created, the next step is to allocate space for your root filesystem. We’ll use LVM (Logical Volume Manager) to provide flexibility in managing disk space. LVM lets you resize, snapshot, and organize storage more dynamically than traditional partitions—making it ideal for a homelab or evolving system. In this step, we’ll create a partition that will serve as the physical volume backing our LVM setup.

1. Create the **root partition**:

    - Press `n` (new partition).
    - Select partition number **2**.
    - First sector: **Default**.
    - Last sector: **Default** (use remaining disk space).
    - Partition type: **Linux LVM (44)**.
    - Press `w` to save and exit.

### Use [LVM on LUKS](https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system#LVM_on_LUKS)

To protect your data and privacy, encrypting your disk is one of the most important steps you can take. Using **LUKS (Linux Unified Key Setup)** to encrypt your root partition ensures that all data stored on the disk remains secure and inaccessible without your password. By layering LVM (Logical Volume Manager) on top of the encrypted container, you gain flexible disk management—allowing you to create, resize, and manage multiple logical volumes like root, swap, and home inside a single encrypted container. This setup combines strong security with powerful storage flexibility.

1. Confirm your LVM root partition.

    ```bash
    lsblk
    ```

    `/dev/sda` should now have two partitions, one EFI partition for boot, and one LVM partition for the root filesystem. In this example the LVM partition is `/dev/sda2`.

1. Create the encrypted container. Confirm with `YES` and create password.

    ```bash
    cryptsetup luksFormat /dev/sda2
    ```

1. Open the container

    ```bash
    cryptsetup open /dev/sda2 cryptlvm
    ```

1. See your opened encrypted container with `lsblk`

### Prepare the logical volumes

1. Create a physical volume on top of the opened LUKS container:

    ```bash
    pvcreate /dev/mapper/cryptlvm
    ```

1. Create a volume group with a cool name. Replace `MyVolGroup`.

    ```bash
    vgcreate MyVolGroup /dev/mapper/cryptlvm
    ```

### Create the logical volumes

#### swap

When deciding on swap size, consider your system’s role and memory capacity. For a server or always-on node like a K3s worker, which won’t use hibernation, swap mainly serves as a safety net for occasional memory spikes rather than for suspending the system. With 16 GB of RAM, allocating around 2 to 4 GB of swap is usually sufficient to maintain system stability without wasting disk space. Larger swap sizes are generally unnecessary unless you anticipate heavy memory pressure or require hibernation support.

```bash
lvcreate -L 4G -n swap MyVolGroup
```

#### root

The root logical volume holds your operating system, system files, and installed software. When creating this volume, allocate enough space to comfortably fit your base system, applications, and future updates. For most users, 20 to 40 GB is a good starting point—large enough to avoid space issues but not so large as to waste storage. You can always resize it later if needed.

```bash
lvcreate -L 32G -n root MyVolGroup
```

#### home

The home logical volume stores your personal files, configurations, and user data. Allocating the remaining available space to this volume ensures you have plenty of room for documents, media, projects, and anything else you create or download.

```bash
lvcreate -l 100%FREE -n home MyVolGroup
```

Leave at least 256 MiB free space in the volume group to allow using [e2scrub(8)](https://man.archlinux.org/man/e2scrub.8). After creating the last volume with `-l 100%FREE`, this can be accomplished by reducing its size with `lvreduce -L -256M MyVolGroup/home`.

#### Verify Logical Volumes

After creating your logical volumes, it’s important to confirm they were set up correctly. Use the `lsblk` command to list all block devices and their partitions, including your LVM logical volumes:

```bash
lsblk
```

You should see your physical disk, the encrypted LUKS container (e.g., `/dev/mapper/cryptlvm`), and the logical volumes inside your volume group (e.g., `swap`, `root`, and `home`) listed as separate devices. This confirms that the LVM setup is recognized by the system and ready for formatting and mounting.

Example output for a volume group called `rx78`:

```bash
root@archiso ~ lsblk
NAME              MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINTS
loop0               7:0    0 824.9M  1 loop  /run/archiso/airootfs
sda                 8:0    0 476.9G  0 disk  
├─sda1              8:1    0     1G  0 part  
└─sda2              8:2    0 475.9G  0 part  
  └─cryptlvm      254:2    0 475.9G  0 crypt 
    ├─rx78-swap   254:3    0     4G  0 lvm   
    ├─rx78-root   254:4    0    32G  0 lvm   
    └─rx78-home   254:5    0 439.7G  0 lvm   
sdb                 8:16   1  58.6G  0 disk  
├─sdb1              8:17   1  58.6G  0 part  
│ ├─ventoy        254:0    0   1.2G  0 dm    
│ └─sdb1          254:1    0  58.6G  0 dm    
└─sdb2              8:18   1    32M  0 part  
```

Now is the last convenient time to rename your volume group if you wish to. Later, you may need to unmount its volumes, and deactivate it. Rename your volume group with the following command:

```bash
vgrename old_vg_name new_vg_name
```

Should you need to manage your logical volumes after this, you'll need to work with the `lvm2` tool. It comes with the Arch install medium and later, we'll install this with the Arch package manager `pacman`.

You'll quickly find the possible commands by typing in `lv`, `pv`, and `vg` and then the `Tab` key to see possible commands.

See [Arch Wiki - LVM](https://wiki.archlinux.org/title/LVM) for more.

### Format the Partitions

A filesystem gives structure to your disk, allowing the OS to store and retrieve files like programs, configs, and documents. Without one, the disk is just raw data with no organization. Formatting sets up this structure, enabling features like folders, permissions, and metadata. Different filesystems are suited for different tasks — like FAT for boot partitions or ext4 for Linux systems.

Now that the partitions and logical volumes are in place, it’s time to format them with appropriate filesystems. Each partition or volume serves a different purpose, and we use formats suited to their role:

- **FAT32** (via `mkfs.fat`) is used for the EFI system partition because it's a simple, standardized format recognized by UEFI firmware across all platforms.

- **ext4** (via `mkfs.ext4`) is a widely-used Linux filesystem that offers reliability, journaling, and good performance for the root and home logical volumes.

- **swap** (via `mkswap`) designates a space on disk to be used as virtual memory, which helps the system manage memory pressure even if RAM is full.

Using the right format ensures system compatibility, stability, and performance—especially on systems like Arch, where you build everything from the ground up.

1. Format the **EFI partition**:

    ```bash
    mkfs.fat -F32 /dev/sda1
    ```

1. Format the **root partition** and **home partition** as **ext4**:

    ```bash
    mkfs.ext4 /dev/MyVolGroup/root
    ```

    ```bash
    mkfs.ext4 /dev/MyVolGroup/home
    ```

1. Format the **swap partition**

    ```bash
    mkswap /dev/MyVolGroup/swap
    ```

You can verify all your formatted drives now with the follow command:

```bash
lsblk -f
```

For even more options:

```bash
lsblk --help
```

### Mount the Partitions

Before we can install Arch Linux, we need to "mount" our partitions—this means attaching them to specific directories in the filesystem tree so the installer knows where to put everything. Think of it like plugging in an external drive: we’re telling the system, “store root files here, user data there, and boot info over here.” Mounting prepares the /mnt directory to temporarily act as the root of our future system. If you're coming from Windows, it's similar to how external drives show up as paths when plugged in. When you "eject" a drive, you’re unmounting it. Right now, the drives are partitioned and formatted, but not yet reachable—mounting is the final step before we can begin the actual installation.

1. Mount the **root partition**:

    ```bash
    mount /dev/MyVolGroup/root /mnt
    ```

1. Mount the **home partition**:

    ```bash
    mount --mkdir /dev/MyVolGroup/home /mnt/home
    ```

1. Mount the **swap partition**

    ```bash
    swapon /dev/MyVolGroup/swap
    ```

1. Mount the **EFI partition**:

    ```bash
    mount --mkdir /dev/sda1 /mnt/boot
    ```

1. Verify all the mounted partition with `lsblk` again. Notice the mountpoints column has new entries for your mount directories.

Example output:

```bash
root@archiso ~ lsblk
NAME            MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINTS
loop0             7:0    0 824.9M  1 loop  /run/archiso/airootfs
sda               8:0    0 476.9G  0 disk  
├─sda1            8:1    0     1G  0 part  /mnt/boot
└─sda2            8:2    0 475.9G  0 part  
  └─cryptlvm    254:2    0 475.9G  0 crypt 
    ├─rx78-swap 254:3    0     4G  0 lvm   [SWAP]
    ├─rx78-root 254:4    0    32G  0 lvm   /mnt
    └─rx78-home 254:5    0 439.7G  0 lvm   /mnt/home
sdb               8:16   1  58.6G  0 disk  
├─sdb1            8:17   1  58.6G  0 part  
│ ├─ventoy      254:0    0   1.2G  0 dm    
│ └─sdb1        254:1    0  58.6G  0 dm    
└─sdb2            8:18   1    32M  0 part
```

## Install Essential Packages

This is where the real installation begins. With partitions mounted and the /mnt directory acting as the root of our new system, we can now install the core components that make up Arch Linux. The pacstrap command pulls down and installs essential packages like the base system, the Linux kernel, and firmware needed to support your hardware. This forms the foundation of your Arch install—everything else will be layered on top from here.

Run the following command to install the **base system**:

```bash
pacstrap -K /mnt base linux linux-firmware
```

You may want to install some other packages at this time too. Please take some time to learn about and decide which packages you might want. You can always install them later too. Some of these packages are installed now because we'll need them quite soon to complete the installation too.

```bash
pacstrap -K /mnt base base-devel efibootmgr git grub intel-ucode linux linux-firmware lvm2 man-db man-pages openssh polkit python python-yaml sudo tailscale texinfo tmux tree vim wget
```

These packages were chosen to provide a solid and functional base system with essential tools for both usability and system management:

- `base`, `linux`, `linux-firmware` – The core operating system and kernel.

- `base-devel` – Development tools needed for building packages, especially with yay.

- `efibootmgr`, `grub` – Tools for managing EFI boot entries and bootloading.

- `intel-ucode` – CPU microcode updates for Intel processors.

- `lvm2` – Tools for managing logical volumes.

- `man-db`, `man-pages`, `texinfo` – Documentation and manual pages.

- `git`, `python`, `python-yaml` – Scripting, config parsing, and version control.

- `sudo`, `polkit` – Privilege escalation and permission handling.

- `openssh`, `tailscale` – Remote access and secure networking.

- `tmux`, `tree`, `vim`, `wget` – Quality-of-life CLI tools.

This set balances minimalism with practicality for a daily-use, always-on system like a K3s node.

The Arch live install medium includes a wide array of preinstalled tools that make the installation process smooth—things like `wget`, `vim`, `git`, `ip`, `lsblk`, and more are ready to use out of the box. But once you reboot into your freshly installed Arch system, you'll be working with a very minimal setup. Most of those tools won't be there unless you explicitly install them. That's why we're adding these essential packages now—so you don't reboot into a bare system and realize you're missing basic utilities you relied on just moments ago during the install process.

### Generate Filesystem Table

Now that our partitions are mounted and the base system is installed, we need to ensure those same partitions are automatically mounted every time the system boots. The fstab (file system table) handles this. By generating an fstab file, we’re telling the system what to mount, where to mount it, and how. The -U flag uses UUIDs (Universally Unique Identifiers) for reliability across hardware changes, and we append this info to /mnt/etc/fstab so it’s picked up during the next boot.

Generate an `fstab` file to automatically mount partitions on boot:

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

Inspect the `fstab`

```bash
cat /mnt/etc/fstab
```

You should find that it pretty much does what it sounds like. The `fstab` file is read by the `systemd` init system (or the older mount process on non-systemd systems) during boot. Specifically, `systemd` parses `/etc/fstab` to automatically mount the listed filesystems at the appropriate mount points as part of the boot sequence.

### Chroot into the New System

Enter your newly installed Arch Linux environment:

```bash
arch-chroot /mnt
```

Notice how your terminal prompt changes.

Run `ls` to see your Linux system files and directories.

You use `arch-chroot /mnt` to "change root" into your new system’s environment. This lets you run commands as if you had booted into the installed Arch Linux. Before `arch-chroot /mnt`, your root `/` was the live install environment’s root — basically the running Arch ISO system in memory. After chroot, the root switches to your new installed system at `/mnt`.

<!-- ## System Configuration

### **Set Time Zone**

```bash
ln -sf /usr/share/zoneinfo/US/Pacific /etc/localtime
```

### **Sync Hardware Clock**

```bash
hwclock --systohc
```

### Configure Locale

1. Edit `/etc/locale.gen` and uncomment:

    ```text
    en_US.UTF-8 UTF-8
    ```

2. Generate locales:

    ```bash
    locale-gen
    ```

### Set Hostname

Create the `/etc/hostname` file with your chosen hostname:

```bash
echo "myhostname" > /etc/hostname
```

### Set Root Password

```bash
passwd
```

---

## Install and Configure GRUB

### Install GRUB and Boot Manager

```bash
pacman -S grub efibootmgr
```

### Install GRUB to the EFI System Partition (ESP)

```bash
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
```

### Generate GRUB Configuration

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

---

## Final Steps

1. Exit chroot:

    ```bash
    exit
    ```

2. Unmount partitions:

    ```bash
    umount -R /mnt
    ```

3. Reboot:

    ```bash
    reboot
    ```

## Next Steps

### Network Configuration -->
