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
By default, in a frame of type HDLC, PPP, or Ethernet, when it is assigned to a VLAN (VLAN k), it will be tagged, meaning a new field will be added to the frame. This introduces a new type of encapsulation, which is 802.1Q.  

When configuring a port in trunk mode, it will by default deal with frames with 802.1Q encapsulation.  

On a Layer 3 switch like the **Cisco 3650**, when configuring a port in trunk mode, it automatically uses 802.1Q encapsulation by default. However, on a Layer 2/3 switch like the **Cisco 3560**, the encapsulation type (ISL or 802.1Q) must be specified before executing the `switchport mode trunk` command. This is done using the command:  
`switchport trunk encapsulation dot1q` (for 802.1Q).


---
### . **VLANs Allowed on Trunk**

- **Explanation**: This refers to the list of VLANs that are permitted to traverse the trunk link as defined by the `switchport trunk allowed vlan` command.
- **Default Behavior**: By default, all VLANs (1-4094) are allowed on a trunk unless explicitly restricted.


---
### 2. **VLANs Allowed and Active in Management Domain**

- **Explanation**: This shows VLANs that are both:
    1. Allowed on the trunk (from the previous field).
    2. Currently active in the VLAN database on the switch (created and not administratively shut down).
- **Importance**: If a VLAN is allowed on the trunk but does not exist or is inactive in the VLAN database, it will not appear in this field.
- **Example**: If VLANs 1, 10, and 20 exist in the VLAN database and are active, they will show in this field. However, if VLAN 30 is not created, it won't appear even if it's allowed on the trunk.


