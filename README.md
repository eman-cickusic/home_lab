# Security Testing Lab Documentation
> ⚠️ **Warning**: This lab environment is for educational and research purposes only. All testing should be performed in an isolated virtual environment.

## Initial Setup

### VirtualBox Configuration

1. **Windows 10 VM Network Setup**:
   - Open VirtualBox Manager
   - Select Windows 10 VM → Settings → Network
   - Set "Attached to:" to "Internal Network"
   - Name: "mytest"
   - Ensure "Enable Network Adapter" is checked
   - Set "Promiscuous Mode:" to "Deny"
   - Verify "Cable Connected" is checked

2. **Windows 10 IP Configuration**:
   - Open Network Settings → IPv4 Properties
   - Select "Use the following IP address:"
   - IP address: 192.168.20.10
   - Subnet mask: 255.255.255.0
   - Leave Default gateway empty
   - Select "Use the following DNS server addresses:"
   - Leave DNS server fields empty

3. **Kali Linux Network Setup**:
   - Open Network Settings → Wired connection
   - Go to IPv4 Settings tab
   - Method: Manual
   - Add Address:
     - Address: 192.168.20.11
     - Netmask: 24
     - Leave Gateway empty
   - Leave DNS servers empty

### Verify Network Configuration

1. **Windows 10 Verification**:
```cmd
ipconfig
```
Verify output shows:
- IPv4 Address: 192.168.20.10
- Subnet Mask: 255.255.255.0

2. **Kali Linux Verification**:
```bash
# Verify network settings
ip addr

# Test connectivity
ping 192.168.20.10
```
Expected output should show successful ping replies with(it may vary based on my output since I let it ping for a sec secods:
- 4 packets transmitted
- 4 packets received
- 0% packet loss
