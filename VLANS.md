Here’s the configuration guide rewritten with commands as part of the text for easier readability:  

---

### **Step 1: Create VLANs**  
Start by entering global configuration mode:  
`configure terminal`  

To create VLANs and assign names:  
- For VLAN 10: `vlan 10`, followed by `name Sales`.  
- For VLAN 20: `vlan 20`, followed by `name IT`.  

Verify VLANs using: `show vlan`.  

---

### **Step 2: Configure Access Ports**  
Assign ports to VLANs in access mode:  
- For example, to assign port `FastEthernet0/1` to VLAN 10:  
  1. `interface FastEthernet0/1`  
  2. `switchport mode access`  
  3. `switchport access vlan 10`  

Repeat for other ports and VLANs as needed.  

---

### **Step 3: Configure Trunk Ports**  
Set a port to trunk mode and specify allowed VLANs:  
- For example, to configure `FastEthernet0/24`:  
  1. `interface FastEthernet0/24`  
  2. `switchport mode trunk`  
  3. `switchport trunk allowed vlan 10,20`  

Verify trunk interfaces with: `show interfaces trunk`.  

---

### **Step 4: Configure Inter-VLAN Routing**  
Enable IP routing on the switch by entering global configuration and running: `ip routing`.  

Create virtual interfaces (SVIs) for VLANs:  
- For VLAN 10:  
  1. `interface vlan 10`  
  2. `ip address 192.168.10.1 255.255.255.0`  
  3. `no shutdown`  

- For VLAN 20:  
  1. `interface vlan 20`  
  2. `ip address 192.168.20.1 255.255.255.0`  
  3. `no shutdown`  

Verify the status of virtual interfaces using: `show ip interface brief`.  

---

### **Step 5: Verify IP Routing**  
Display the routing table using: `show ip route`.  

---

