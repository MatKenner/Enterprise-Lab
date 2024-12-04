### **Setting Up Proxmox and Palo Alto VM Firewall in the Enterprise Network Lab**  

In the previous blog post, I introduced the vision and hardware behind the network lab project. Today, we’re diving into the first steps of building the foundation of our enterprise network: installing **Proxmox VE** on the HP Mini PC and deploying a **Palo Alto VM firewall** to simulate enterprise-grade network security.  

---

#### **Why Proxmox?**  
Proxmox Virtual Environment (VE) is an open-source platform that combines virtualization and containerization, making it a versatile choice for a lab environment. It allows us to host virtual machines (VMs) and containers, which will represent the core components of our network lab.  

---

### **Step 1: Installing Proxmox on the HP Mini PC**  

#### **What You Need**  
- An HP Mini PC (or comparable hardware).  
- A USB flash drive (8GB or larger).  
- The Proxmox VE ISO image (download it from [Proxmox's official site](https://www.proxmox.com)).  
- A keyboard, monitor, and network cable connected to the Mini PC.  

#### **Installation Steps**  

1. **Prepare the USB Installer**:  
   - Download the Proxmox VE ISO and use a tool like **Etcher** or **Rufus** to create a bootable USB drive.  

2. **Boot the Mini PC from USB**:  
   - Insert the USB drive into the Mini PC and restart it.  
   - Enter the BIOS/UEFI setup (commonly done by pressing `F2`, `F12`, or `Delete` during boot) and set the USB drive as the primary boot device.  

3. **Install Proxmox VE**:  
   - Follow the installation wizard.  
   - Select the storage disk for installation, configure the network interface, and set the root password and email address.  

4. **Access the Proxmox Web Interface**:  
   - Once installed, the Mini PC will display the Proxmox management URL.  
   - Access the interface from a browser on your local network (e.g., `https://<MiniPC-IP>:8006`).  

---

### **Step 2: Deploying Palo Alto VM Firewall**  

#### **What You Need**  
- A Palo Alto firewall VM image (acquire it from [Palo Alto Networks](https://www.paloaltonetworks.com/) if you have access).  
- A Proxmox-compatible format for the VM image (OVA, QCOW2, or VMDK).  
- A valid license or trial license from Palo Alto.  

#### **Steps to Deploy**  

1. **Convert the VM Image (if needed)**:  
   - If your VM image is not in QCOW2 format, convert it using tools like `qemu-img`:
     ```bash
     qemu-img convert -f vmdk -O qcow2 <input.vmdk> <output.qcow2>
     ```

2. **Upload the Image to Proxmox**:  
   - In the Proxmox web interface, go to **Datacenter → Storage → Your Storage**.  
   - Click **Content → Upload**, then upload the QCOW2 file.  

3. **Create a VM in Proxmox**:  
   - Go to **Datacenter → Create VM** and configure the following:
     - **ID**: Assign a unique VM ID.  
     - **Name**: Give it a descriptive name like `PaloAltoFirewall`.  
     - **OS**: Select "Do not use any media."  
     - **Storage**: Choose the storage location for the VM.  
   - Skip to the disk configuration, and under the disk options, choose the uploaded QCOW2 file.  

4. **Configure the VM Settings**:  
   - Allocate sufficient CPU cores, RAM, and disk space as recommended by Palo Alto (e.g., 2 cores, 4GB RAM).  
   - Add network interfaces as required for your lab setup.  

5. **Start and Configure the VM**:  
   - Boot the VM and access the Palo Alto firewall console using the Proxmox interface.  
   - Complete the initial setup wizard, including management interface configuration and activation of the license.  

---

### **Testing the Setup**  
Once Proxmox is running and the Palo Alto VM is deployed:  
- Verify connectivity to the firewall’s management interface by accessing it via a browser or SSH.  
- Configure basic policies on the Palo Alto firewall to secure traffic flows in the lab.  

---

### **Lessons Learned**  
During the setup, I encountered challenges such as configuring the correct network bridge in Proxmox to connect the Palo Alto firewall to the physical network. These types of troubleshooting exercises are invaluable for understanding how virtualization integrates with physical hardware.  

---

### **Next Steps**  
In the next blog post, I’ll configure the Cisco 2960X switch and integrate it with the Proxmox environment and the Palo Alto firewall. This will enable us to segment traffic and implement inter-VLAN routing. Stay tuned!  
