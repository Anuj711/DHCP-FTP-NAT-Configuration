# Basic DHCP, FTP, and NAT Configuration - Experiment 3

## Objective
This lab focuses on configuring essential network services and verifying their functionality:
- Configure routers as a DHCP server.
- Verify automatic IP address assignment to PCs.
- Configure and test FTP services on servers.
- Upload and download files using FTP.
- Configure static NAT for public-private IP mapping.
- Test connectivity across the network.

## Network Topology
The network consists of:
- **Router11** (192.168.20.1 and 192.168.10.1)
- **Server0** (192.168.10.11) with a gateway at 192.168.10.1

## Tasks and Configuration Steps

### **Task 1: DHCP Configuration**
1. Enter Global Configuration mode.
2. Configure DHCP pools for both networks:

#### **Router11 - DHCP Configuration**
```bash
R1(config)# ip dhcp pool POOL1
R1(dhcp-config)# network 192.168.10.0 255.255.255.0
R1(dhcp-config)# default-router 192.168.10.1
R1(dhcp-config)# dns-server 8.8.8.8

R1(config)# ip dhcp pool POOL2
R1(dhcp-config)# network 192.168.20.0 255.255.255.0
R1(dhcp-config)# default-router 192.168.20.1
R1(dhcp-config)# dns-server 8.8.8.8
```
3. Enable DHCP on the client PCs and verify assigned IPs using:
```bash
show ip dhcp binding
```

### **Task 2: FTP Configuration**
1. **Enable FTP Service on Server0**
   - Navigate to `Services > FTP` and turn it `ON`.
   - Create user accounts for FTP access.

2. **Upload a File to FTP Server**
   - On PC1, create a text file and save it as `myfile.txt`.
   - Open Command Prompt and upload the file:
   ```bash
   ftp 192.168.10.11
   Username: [Enter FTP Username]
   Password: [Enter FTP Password]
   ftp> put myfile.txt
   ftp> dir
   ftp> quit
   ```

3. **Download a File from FTP Server on PC2**
   ```bash
   ftp 192.168.10.11
   ftp> get myfile.txt
   ftp> dir
   ftp> quit
   ```

### **Task 3: Static NAT Configuration**
1. **Configure static NAT on Router11:**
```bash
R1(config)# ip nat inside source static 192.168.10.11 203.0.113.1
```
2. **Define NAT inside and outside interfaces:**
```bash
R1(config)# interface Fa0/0
R1(config-if)# ip nat outside
R1(config)# interface Fa0/1
R1(config-if)# ip nat inside
```
3. **Verify NAT configuration:**
```bash
show ip nat translations
```

## Expected Outcomes
- PCs receive dynamic IP addresses from DHCP.
- Successful file upload/download via FTP.
- Proper mapping of internal to external IP addresses using NAT.
- Verified network connectivity using `ping` and `traceroute` commands.

This experiment helps in understanding essential networking services used in real-world deployments.

