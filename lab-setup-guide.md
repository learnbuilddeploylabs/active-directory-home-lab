<h1 id="top"> 🧪 Active Directory Lab Setup Guide (VMware Workstation Pro) </h1>

🎯 Ideal for beginners. No prior Active Directory experience needed.

A step-by-step guide to building your own Active Directory lab from scratch using VMware Workstation Pro. This guide walks you through creating two virtual machines: a Domain Controller (Windows Server 2019) and a Client Machine (Windows 10 Pro), joining the domain, and getting ready for AD tasks.

---

## <h2 id="table-of-contents"> 🗂️ Table of Contents </h2>

- [⚙️ VMware Workstation Pro Installation](#vmware-workstation-pro-installation)
- [Domain Controller (DC01)](#domain-controller-dc01)
  - [🛠️ Virtual Machine Setup](#virtual-machine-setup-dc01)
  - [💽 Windows OS Installation](#windows-os-installation-dc01)
  - [🌐 Network and AD Configuration](#network-and-ad-configuration-dc01)
- [Client Machine (CLIENT01)](#client-machine-client01)
  - [🛠️ Virtual Machine Setup](#virtual-machine-setup-client01)
  - [💽 Windows OS Installation](#windows-os-installation-client01)
  - [🌐 Network Configuration](#network-configuration-client01)
  - [🧑‍💻 Join CLIENT01 to the Domain](#join-client01-to-the-domain)
- [✅ Final Check](#final-check)
- [📦 What’s Next?](#whats-next)
- [🧠 Tips](#tips)
- [💬 Questions or Feedback?](#questions-or-feedback)

---

## <h2 id="vmware-workstation-pro-installation"> ⚙️ VMware Workstation Pro Installation </h2>

1. Download [VMware Workstation Pro](https://www.vmware.com/products/workstation-pro.html) from the official VMware site.
2. Run the installer as Administrator.
3. Leave the default installation settings unless you have specific reason to change them.
4. Complete the installation and reboot if prompted.
5. Run VMware Workstation Pro normally, NOT as admin. Don't be so power-hungry.

---

## Domain Controller (DC01) 

## <h2 id="virtual-machine-setup-dc01"> 🛠️ Virtual Machine Setup </h2>

### 1. Create the Virtual Machine (VM)
- Open **VMware Workstation Pro**.
- Select **Create a New Virtual Machine**.
  
![Create a New Virtual Machine](images/vmware-installation/dc01-setup/dc01_01_create-vm.PNG)
  
- Choose **Typical (recommended)** and click `Next`.
  
![Typical Installation](images/vmware-installation/dc01-setup/dc01_02_typical-setup.PNG)
  
- Select **I will install the operating system later**, then click `Next`.
  
![Install Later](images/vmware-installation/dc01-setup/dc01_03_install-os-later.PNG)
  
- **OS: Microsoft Windows** → **Version: Windows Server 2019**.
- Click `Next`.

![Select OS](images/vmware-installation/dc01-setup/dc01_04_select-os.PNG)
  
- Name the virtual machine **DC01**, choose a save location, then click `Next`.

![Name and Location](images/vmware-installation/dc01-setup/dc01_05_name-and-location.PNG)
   
- Allocate **60 GB** of storage. (Dynamic allocation is fine)  

![Disk Capacity](images/vmware-installation/dc01-setup/dc01_06_disk-capacity.PNG)

- Click `Customize Hardware`.  

![Customize Hardware](images/vmware-installation/dc01-setup/dc01_07_customize-hardware.PNG)

[🔝 Back to Top](#top)

---

### 2. Hardware Customization
- **Memory**: 4096 MB (4GB) recommended, but the default may work fine depending on your system.
- **Processors**: 2

<img src="images/vmware-installation/dc01-setup/dc01_08_ram-and-processor.PNG" alt="RAM and CPU" width="448" height="450">

- **CD/DVD (SATA)**: Choose `Use ISO image file` and load your Server 2019 ISO.

<img src="images/vmware-installation/dc01-setup/dc01_09_select-iso.PNG" alt="Select ISO" width="448" height="450">

- **Network Adapters**:
  - Leave the default NAT adapter.
  - Click 'Add'.
 
<img src="images/vmware-installation/dc01-setup/dc01_11_add-network.PNG" alt="Add Adapter" width="448" height="450">

  - Select **Network Adapter**.

![Select Adapter](images/vmware-installation/dc01-setup/dc01_12_select-network-adapter.PNG)

  - Select **Host-only: A private network shared with the host**.

<img src="images/vmware-installation/dc01-setup/dc01_13_host-only_adapter.PNG" alt="Host Only Adapter" width="448" height="450">

- Click `Close`, then `Finish`.

**You just created your first virtual machine! Good work! 🎉**

[🔝 Back to Top](#top)

---

## <h2 id="windows-os-installation-dc01"> 💽 Windows OS Installation </h2>

### Installing Windows Server 2019
- Power on **DC01**.
- When prompted to **Press any key to boot from CD or DVD**, press any key.
  - **ACT FAST!** You'll only have a moment. 
  - If you see the screen babbling about EFI, just restart the VM and maybe pay attention next time. Slow-poke.

![Press Any Key](images/os-installation/dc01-os/dc01_01_press-any-key.PNG)

- Choose **Standard (Desktop Experience)**.

![Standard (Desktop Experience)](images/os-installation/dc01-os/dc01_02_standard-desktop-exp.PNG)

- Accept the license terms, click `Next`.
- Choose **Custom: Install Windows only (advanced)**.

![Custom Install](images/os-installation/dc01-os/dc01_04_custom-install.PNG)

- Select drive and click `Next`.

![Select Drive](images/os-installation/dc01-os/dc01_05_select-drive.PNG)

- Set a strong local administrator password.

![Create Local Admin](images/os-installation/dc01-os/dc01_06_create-local-admin.PNG)

- Once installed, log in.
  - Click `Send Ctrl+Alt+Del to this virtual machine`.
  - Log in. 
- Optional: Enable RDP in **Server Manager** → **Local Server** → enable **Remote Desktop**.
- Install VMware Tools via VM menu.
  - This improves performance, mouse behavior, and screen resolution.

![Install VMware Tools](images/os-installation/dc01-os/dc01_10_install-vmware-tools.PNG)

**OS: Installed. 🎉 Good job!**

[🔝 Back to Top](#top)

---

## <h2 id="network-and-ad-configuration-dc01"> 🌐 Network & AD Configuration </h2>

### 1. Set Static IP on DC01 (Host-Only Adapter)
- **Control Panel** → **Network and Internet** → **Network and Sharing Center** → **Change adapter settings**.

![Change Adapter Settings](images/network-setup/dc01-network/dc01_01_change-adapter-settings.PNG)

- Right-click **Ethernet1** and click `Properties`.

![Ethernet1 Properties](images/network-setup/dc01-network/dc01_02_ethernet1-properties.PNG)

- Select **Internet Protocol Version 4**, then click `Properties`.

![IPv4 Properties](images/network-setup/dc01-network/dc01_03_ethernet-properties.PNG)

- Use the following:
  - IP: `192.168.100.10`
  - Subnet: `255.255.255.0`
  - Gateway: (leave blank)
  - DNS: `127.0.0.1`
 
![IP Settings](images/network-setup/dc01-network/dc01_06_ipv4-settings.PNG)

[🔝 Back to Top](#top)

---

### 2. Rename DC01
- In **Server Manager**, click on **Local Server**.
- Click on the Computer name. 
- No, not that one. 
- What did I just say? Not the words 'Computer name', the name in blue next to that. Oh, just check the screenshot...

![Computer Name](images/network-setup/dc01-network/dc01_04_computer-name.PNG)

- Click `Change`.

![Click Change](images/network-setup/dc01-network/dc01_05_click-change.PNG)

- Set Computer name to `DC01`.

![Rename Computer](images/network-setup/dc01-network/dc01_07_rename-computer.PNG)

- Click `OK`.
- Click `OK` again.
- Click `Close`.
- Click `Restart Now`
  - Yes, actually restart this time.

![Restart](images/network-setup/dc01-network/dc01_08_restart.PNG)

[🔝 Back to Top](#top)

---

### 3. Install Active Directory Domain Services (AD DS)
- Log in (obviously).
- **Server Manager** → **Manage** → **Add Roles and Features**.

![Add Roles and Features](images/ad-setup/dc01-ad/dc01_01_roles-and-features.PNG)

- Click through defaults until **Server Roles**.
- Check **Active Directory Domain Services**. 

![Check AD DS](images/ad-setup/dc01-ad/dc01_02_check-ad.PNG)

- Click `Add Features`.

![Add Features](images/ad-setup/dc01-ad/dc01_03_add-features.PNG)

- Click through remaining prompts and click `Install`. 
  - **DO NOT CLOSE THE WINDOW!**
  - You closed it, didn't you? I knew it! Don't worry, keep going and we'll learn how to pay attention to big bold warnings.

![Install](images/ad-setup/dc01-ad/dc01_04_install-ad.PNG)

- Ignore these lies. Or don't. Your call.

![Don't Close](images/ad-setup/dc01-ad/dc01_05_do-not-close.PNG)

[🔝 Back to Top](#top)

---

### 4. Promote DC01 to Domain Controller
- Select **Promote this server**.

![Promote Server](images/ad-setup/dc01-ad/dc01_06_promote-server.PNG)

- And for those of you didn't pay attention to the warning about closing the window, you'll see a yellow notification like this on your Server Manager Dashboard. Click on it.
- Click **Promote this server to a domain controller**.

![Didn't Listen](images/ad-setup/dc01-ad/dc01_07_promote-server-did-not-listen.PNG)

- Select **Add a new forest**.
- Set the Root domain name to **corp.local**.

![Domain Name](images/ad-setup/dc01-ad/dc01_08_add-forest.PNG)

- Set DSRM password.
  - Write it down.
  - You already forgot it, didn't you?
  - Seriously, you'll need it for certain recovery tasks.
- Click `Next`.

![Set DSRM Password](images/ad-setup/dc01-ad/dc01_09_dsrm-password.PNG)

- Accept defaults in the following sections and click `Install`.
  - The yellow warnings are ok. As long as there are no actual errors you'll be fine. Trust me...

![Install Forest](images/ad-setup/dc01-ad/dc01_10_install-forest.PNG)

- Server will reboot automatically after configuration.

![Reboot](images/ad-setup/dc01-ad/dc01_11_reboot.PNG)

**Congrats! You’ve just created your own domain controller! 🎉**

[🔝 Back to Top](#top)

---

## Client Machine (CLIENT01)

## <h2 id="virtual-machine-setup-client01"> 🛠️ Virtual Machine Setup </h2>

### 1. Create the virtual machine (VM).
- Open **VMware Workstation Pro**.
- Select **Create a New Virtual Machine**.
  
![Create a New Virtual Machine](images/vmware-installation/client01-setup/client01_01_create-vm.PNG)
  
- Choose **Typical (recommended)** and click `Next`.
  
![Typical Installation](images/vmware-installation/client01-setup/client01_02_typical-setup.PNG)
  
- Select **I will install the operating system later**, then click `Next`.
  
![Install Later](images/vmware-installation/client01-setup/client01_03_install-os-later.PNG)
  
- **OS: Microsoft Windows** → **Version: Windows 10**.
- Click `Next`.

![Select OS](images/vmware-installation/client01-setup/client01_04_select-os.PNG)
  
- Name the virtual machine **CLIENT01**, choose a save location, then click `Next`.

![Name and Location](images/vmware-installation/client01-setup/client01_05_name-and-location.PNG)

- Allocate **40 GB** of storage. (Dynamic allocation is fine)

![Disk Capacity](images/vmware-installation/client01-setup/client01_06_disk-capacity.PNG)

- Click `Customize Hardware`.

![Customize Hardware](images/vmware-installation/client01-setup/client01_07_customize-hardware.PNG)


[🔝 Back to Top](#top)

---

### 2. Hardware Customization
- **Memory**: 4096 MB (4GB) recommended, but the default may work fine depending on your system.
- **Processors**: 2

<img src="images/vmware-installation/client01-setup/client01_08_ram-and-processor.PNG" alt="RAM and CPU" width="448" height="450">

- **CD/DVD (SATA)**: Choose `Use ISO image file` and load your Windows 10 ISO.

<img src="images/vmware-installation/client01-setup/client01_09_select-iso.PNG" alt="Select ISO" width="448" height="450">

- **Network Adapters**:
  - Leave the default NAT adapter.
  - Click 'Add'.
 
<img src="images/vmware-installation/client01-setup/client01_10_add-network.PNG" alt="Add Adapter" width="448" height="450">

  - Select **Network Adapter**.

![Select Adapter](images/vmware-installation/client01-setup/client01_11_select-network-adapter.PNG)

  - Select **Host-only: A private network shared with the host**.

<img src="images/vmware-installation/client01-setup/client01_12_host-only_adapter.PNG" alt="Host Only Adapter" width="448" height="450">

- Click `Close`, then `Finish`.

**That's TWO virtual machines ready to go. Nicely done! 🎉**

[🔝 Back to Top](#top)

---

## <h2 id="windows-os-installation-client01"> 💽 Windows OS Installation </h2>

### Installing Windows 10
- Power on **CLIENT01**.
- When prompted to **Press any key to boot from CD or DVD**, press any key.
  - **REMEMBER**: Don't be a slow-poke here!

![Press Any Key](images/os-installation/client01-os/client01_01_press-any-key.PNG)

- Accept the license terms, click `Next`.
- Choose **Custom: Install Windows only (advanced)**.

![Custom Install](images/os-installation/client01-os/client01_03_custom-install.PNG)

- Select drive and click `Next`.

![Select Drive](images/os-installation/client01-os/client01_04_select-drive.PNG)

- Once installed, log in.
  - Choose **Offline Account**, **Domain Join Instead**, or **Skip** Microsoft sign-in (depending on version).

![Log In](images/os-installation/client01-os/client01_05_domain-join-instead.PNG)

- Create a local user like `LabUser`.
- Finish setup.
- Install VMware Tools.
  - This improves performance, mouse behavior, and screen resolution.
  - But you knew that already, didn't you? Or did you skip a section? You rebel.

![Install VMware Tools](images/os-installation/dc01-os/dc01_10_install-vmware-tools.PNG)

  - Click `Next`.
  - Select **Typical**, click `Next`.

  ![VMware Tools Typical Install](images/os-installation/dc01-os/dc01_11_vmware-tools-typical.PNG)

  - Click `Install`, then `Finish`.
  - Click `Yes` to restart.
    - There's a mandatory restart coming up in a few steps after you rename this computer, so you can hold off on restarting if you want to. Or can you...? 
    - You can (This sounds familiar, doesn't it?).

**You could do this in your sleep! Keep up the great work! 🎉**

[🔝 Back to Top](#top)

---

## <h2 id="network-configuration-client01"> 🌐 Network Configuration </h2>

### 1. Set DNS to **DC01**
- We're using **DC01** as our DNS.
- **Control Panel** → **Network and Internet** → **Network and Sharing Center** → **Change adapter settings**.

![Change Adapter Settings](images/network-setup/client01-network/client01_01_change-adapter-settings.PNG)

- Right-click **Ethernet1** and click `Properties`.

![Ethernet1 Properties](images/network-setup/client01-network/client01_02_ethernet1-properties.PNG)

- Select **Internet Protocol Version 4**, then click `Properties`.

![IPv4 Properties](images/network-setup/client01-network/client01_03_ethernet-properties.PNG)

- Set DNS to the **DC01** IP address, `192.168.100.10`.
- Click `OK`.

![Set DNS](images/network-setup/client01-network/client01_04_set-dns.PNG)

**Keep going, you're almost there! 🎉**

[🔝 Back to Top](#top)

---

## <h2 id="join-client01-to-the-domain"> 🧑‍💻 Join CLIENT01 to the Domain </h2>

### 2. Rename CLIENT01
- **Control Panel** → **System** → **Rename This PC (advanced)**.
  - **System** may sometimes be **About** depending on Windows version.

![System Settings](images/ad-setup/client01-ad/client01_01_system-settings.PNG)

- Click `Change`.

![System Properties](images/ad-setup/client01-ad/client01_02_system-properties.PNG)

- Rename to **CLIENT01**.
- Click `OK`.

![Rename Computer](images/ad-setup/client01-ad/client01_03_change-name.PNG)

- Click `OK`.

![Restart Now](images/ad-setup/client01-ad/client01_04_restart.PNG)

- Close **System Properties**
- Click `Restart Now`

![Restart Now](images/ad-setup/client01-ad/client01_08_restart.PNG)

[🔝 Back to Top](#top)

---

### 3. Join the Domain
- Log back in to **CLIENT01**. 
- **Control Panel** → **System** → **Rename This PC (advanced)**.
  - **System** may sometimes be **About** depending on Windows version.

![System Settings](images/ad-setup/client01-ad/client01_01_system-settings.PNG)

- Click `Change`.

![System Properties](images/ad-setup/client01-ad/client01_02_system-properties.PNG)

- Select **Domain**, set to **corp.local**.
- Click `OK`.

![Set Domain](images/ad-setup/client01-ad/client01_05_set-domain.PNG)

- Log in with your **DC01** credentials.
  - **CORP\administrator**
- Click `OK`.

![Join Domain](images/ad-setup/client01-ad/client01_06_log-in-admin.PNG)

- Click `OK` to close the notification.

![Domain Joined](images/ad-setup/client01-ad/client01_07_domain-joined.PNG)

- Click `OK`.

![Restart Notification](images/ad-setup/client01-ad/client01_04_restart.PNG)

- Click `CLOSE` to close **System Properties**.
- Click `Restart Now` to... well, to restart.

![Restart Now](images/ad-setup/client01-ad/client01_08_restart.PNG)

**Congrats! You now have a domain-joined client machine! 🖥️**

[🔝 Back to Top](#top)

---

## <h2 id="final-check"> ✅ Final Check </h2>

You now have a fully functional domain controller and a domain-joined Windows 10 client! 🎉  

Try logging in to CLIENT01 with **CORP/administrator**.

![Test Log In](images/ad-setup/client01-ad/client01_09_log-in-to-test.PNG)

From here, you can start testing Active Directory tasks like creating users, groups, OUs, and applying GPOs (tutorial coming soon!).


[🔝 Back to Top](#top)

---

## <h2 id="whats-next"> 📦 What's Next? </h2>

👉 [Jump to the AD Task Walkthrough ➡️](link-to-ad-tasks-readme)

Or return to your repo home and start building more!

[🔝 Back to Top](#top)

---

## <h2 id="tips"> 🧠 Tips </h2>

- Take **snapshots** at major milestones!
- Make sure **both adapters** (NAT & Host-only) are configured correctly.
- If login fails, double-check DNS settings and domain name format (`CORP\username`).


[🔝 Back to Top](#top)

---

## <h2 id="questions-or-feedback"> 💬 Questions or Feedback? </h2>

If you find anything confusing or run into trouble, feel free to open an issue or reach out. This guide is made by learners, for learners.

[🔝 Back to Top](#top)