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

![Screenshot 2025-01-28 181603](https://github.com/user-attachments/assets/0e1cf8ae-536e-4d05-aa99-efa68d644577)


2. **Windows 10 IP Configuration**:
   - Open Network Settings → IPv4 Properties
   - Select "Use the following IP address:"
   - IP address: 192.168.20.10
   - Subnet mask: 255.255.255.0
   - Leave Default gateway empty
   - Select "Use the following DNS server addresses:"
   - Leave DNS server fields empty
  
![Picture1](https://github.com/user-attachments/assets/f2b2625c-6935-4df3-9c36-2525e0f2d810)


3. **Kali Linux Network Setup**:
   - Open Network Settings → Wired connection
   - Go to IPv4 Settings tab
   - Method: Manual
   - Add Address:
     - Address: 192.168.20.11
     - Netmask: 24
     - Leave Gateway empty
   - Leave DNS servers empty

![Picture3](https://github.com/user-attachments/assets/38c30c69-72ee-40e2-8e04-4b19af8bf76c)


### Verify Network Configuration

1. **Windows 10 Verification**:
```cmd
ipconfig
```
Verify output shows:
- IPv4 Address: 192.168.20.10
- Subnet Mask: 255.255.255.0

![Picture2](https://github.com/user-attachments/assets/d6d1f518-a34c-402e-a332-6572dc5c65d3)


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

![Picture4](https://github.com/user-attachments/assets/bb03f5fa-9017-4325-90a1-1dd7b973b9f8)


## Environment Setup

### Network Configuration
- Windows 10 VM (Target Machine)
  - IP: 192.168.20.10
  - Internal Network Mode
  - Network isolated from host
- Kali Linux VM (Attack Machine)
  - IP: 192.168.20.11
  - Internal Network Mode
  - Network isolated from host

### Prerequisites
- VMware/VirtualBox
- Windows 10 VM
- Kali Linux VM
- Network connectivity verified between VMs
- Basic understanding of network security concepts

## Network Verification Steps

1. Verify network isolation:
```bash
# On Windows VM
ipconfig /all
ping 192.168.20.11

# On Kali Linux
ip addr
ping 192.168.20.10
```

2. Verify no external connectivity:
```bash
# Test on both VMs
ping 8.8.8.8  # Should fail
```

## Security Testing Process

### 1. Initial Reconnaissance

```bash
# Basic network scan
sudo nmap -sn 192.168.20.0/24

# Detailed scan of Windows target
sudo nmap -sV -sC -O -p- 192.168.20.10 -oA initial_scan
```

### 2. Vulnerability Assessment

```bash
# Using Nmap NSE scripts
sudo nmap -p- --script vuln 192.168.20.10 -oA vuln_scan
```

### 3. Metasploit Framework Testing

1. Launch Metasploit:
```bash
msfconsole
```

2. Basic enumeration:
```bash
# Search for Windows-related modules
search type:exploit platform:windows

# Scan for common vulnerabilities
use auxiliary/scanner/smb/smb_version
set RHOSTS 192.168.20.10
run
```

### 4. Monitoring and Documentation

#### On Windows Target:
1. Enable Windows Event Logging:
   - Open Event Viewer
   - Monitor Security Events
   - Enable Audit Policies

2. Enable Windows Defender:
   - Real-time protection
   - Cloud-delivered protection
   - Automatic sample submission

#### On Kali Linux:
1. Capture network traffic:
```bash
sudo tcpdump -i any host 192.168.20.10 -w capture.pcap
```

### 5. Testing Scenarios

Document each test case:

```markdown
## Test Case Template
- Test ID: TC_001
- Description: [Brief description of the test]
- Tools Used: [List of tools]
- Command(s) Executed: [Commands used]
- Expected Result: [What should happen]
- Actual Result: [What actually happened]
- Windows Defender Response: [How the security system responded]
- Artifacts Generated: [Logs, alerts, etc.]
```

## Data Collection

### Windows VM
- Event Viewer Logs
- Windows Defender Logs
- System Resource Usage
- Network Traffic Logs

### Kali Linux
- Nmap Scan Results
- Metasploit Logs
- Network Captures
- Tool-specific Output

## Analysis Process

1. Review collected data:
   - Compare Windows Defender alerts with known actions
   - Analyze network traffic patterns
   - Review system resource usage during tests
   - Document all triggered alerts

2. Document findings:
   - Detection success rate
   - False positive rate
   - Response time
   - System impact

## Safety Measures

- Ensure VMs are properly isolated
- Regular snapshots before testing
- Monitor host system resources
- No connection to production networks
- Regular malware scans of host system

## Cleanup Procedures

1. After testing:
```bash
# Stop all Metasploit sessions
sessions -K

# Clear logs
sudo rm *.pcap
sudo rm *.xml
```

2. Reset VMs to clean state:
   - Revert to pre-testing snapshots
   - Verify system integrity
   - Clear all generated logs


## Resources

- [Nmap Documentation](https://nmap.org/book/man.html)
- [Metasploit Documentation](https://docs.metasploit.com/)
- [Windows Event Log Analysis](https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4624)
- [Windows Defender Documentation](https://docs.microsoft.com/en-us/windows/security/threat-protection/microsoft-defender-antivirus/microsoft-defender-antivirus-in-windows-10)

---
