# 🔐 NetworkWalks Cybersecurity Internship — Week 2

**Project:** Footprinting and Network Scanning  

**Prepared by:** Keza Mika Samuella

**Internship Program:** NetworkWalks Cybersecurity & Ethical Hacking 

**Modules Completed:** W2-PM4 and W2-PM5  

**Tools:** Kali Linux, theHarvester, Nmap, Zenmap, VirtualBox, Windows  





## 1. Executive Summary

This repository documents my Week 2 practical cybersecurity internship activities, focusing on information gathering, open-source intelligence (OSINT), and network host discovery.

The project consists of two practical modules:

- **W2-PM4 — Footprinting with theHarvester:** Collecting publicly available information about a target domain using multiple OSINT sources.
- **W2-PM5 — Network Scanning with Zenmap:** Discovering active hosts on a local network, identifying their IP and MAC addresses, and visualizing the network topology.

Both activities helped me understand how cybersecurity professionals collect information during the reconnaissance and network discovery stages of a security assessment.

The project also involved troubleshooting virtual machine network connectivity and documenting technical limitations encountered during testing.

## 2. Project Objectives

The main objectives were to:

1. Understand passive reconnaissance and OSINT techniques.
2. Perform domain footprinting using theHarvester.
3. Compare reconnaissance results from different information sources.
4. Identify active devices on an authorized local network.
5. Document IP addresses, MAC addresses, and network topology.
6. Troubleshoot networking issues affecting security tools.
7. Prepare a technical report supported by practical evidence.

## 3. Lab Environment

| Component | Environment |
|---|---|
| Host operating system | Windows |
| Virtualization | Oracle VirtualBox |
| Guest operating system | Kali Linux 2026.2 |
| Reconnaissance tool | theHarvester |
| Network scanning tool | Zenmap / Nmap |
| Kali virtual network | VirtualBox NAT Network |
| Windows LAN subnet | 192.168.1.0/24 |
| Documentation | GitHub Markdown |

**Ethical considerations:** Domain reconnaissance was limited to the assignment's specified public-information sources. Network scanning was performed on the local network used for the practical exercise. No exploitation, password attacks, or vulnerability exploitation was performed.



# 4. W2-PM4 — Footprinting with theHarvester

## 4.1 Introduction

Footprinting is the process of collecting information about a target system, organization, or network before conducting further security assessments.

TheHarvester is an OSINT reconnaissance tool that searches publicly available sources for information such as:

- Email addresses
- Domain names and subdomains
- Hostnames
- IP addresses
- Other publicly indexed information

This information can help security analysts understand an organization's publicly visible digital footprint.

## 4.2 Task 1 — Footprinting Using Baidu

**Objective:** Search for publicly available information associated with the domain `microsoft.com` using the Baidu source.

**Command executed:**

`theHarvester -d microsoft.com -l 1000 -b baidu`

The command was also executed with output redirection using `tee` to preserve the results.

**Command explanation:**

| Parameter | Meaning |
|---|---|
| `-d microsoft.com` | Specifies the target domain |
| `-l 1000` | Sets the search result limit |
| `-b baidu` | Selects Baidu as the information source |
| `tee` | Displays output and saves it to a file |

### Results

The Baidu search did not return email addresses, IP addresses, people, or hosts in the captured results.

This does not mean that Microsoft has no publicly accessible infrastructure. It means the selected source did not return findings during this particular execution.

### Evidence

- [Task 1 output](PM4-theHarvester/outputs/PM4_Task1_Baidu.txt)
- [Task 1 screenshot](PM4-theHarvester/screenshots/PM4_Task1_Baidu.png)

## 4.3 Task 2 — Footprinting Using All Sources

**Objective:** Perform a broader information-gathering exercise using theHarvester's available sources.

**Command executed:**

`theHarvester -d microsoft.com -l 50 -b all`

### Initial execution

During the initial execution, the tool encountered several limitations, including:

- DNS resolution failures
- Unavailable external services
- Missing API keys
- Failed requests to certain information sources

These errors affected the completeness of the initial results.

### Troubleshooting

Investigation of the Kali Linux networking configuration identified an IP address conflict.

The Kali virtual machine had originally been configured with the static address `10.0.0.2/24`, but another device or virtual network component was already using that address.

The network configuration was changed to use DHCP.

After the change, Kali received:

- **IPv4 address:** `10.0.0.3`
- **Default gateway:** `10.0.0.1`
- **Network:** `10.0.0.0/24`

Connectivity testing using `ping 1.1.1.1` returned four successful responses with 0% packet loss.

DNS resolution was also verified using:

`getent hosts microsoft.com`

Following these checks, theHarvester was executed again.

### Retry results

The second execution successfully returned reconnaissance information from available sources.

One source, Hudson Rock, reported **44 hosts**. Additional output included Microsoft-related subdomains.

The 44-host figure represents the result reported by that particular source, not necessarily the total number of unique hosts discovered across every source.

Some services continued to report missing API credentials or other limitations.

### Evidence

- [Initial Task 2 output](PM4-theHarvester/outputs/PM4_Task2_AllSources.txt)
- [Task 2 retry output](PM4-theHarvester/outputs/PM4_Task2_AllSources_Retry.txt)
- [Task 2 initial command](PM4-theHarvester/screenshots/PM4_Task2_Initial_Command.png)
- [Task 2 initial errors](PM4-theHarvester/screenshots/PM4_Task2_Initial_Errors.png)
- [Task 2 retry](PM4-theHarvester/screenshots/PM4_Task2_Retry.png)
- [Discovered subdomains](PM4-theHarvester/screenshots/PM4_Task2_Discovered_Subdomains.png)

## 4.4 PM4 Findings and Lessons Learned

This exercise demonstrated that:

1. Different OSINT sources can return different results for the same domain.
2. Network connectivity directly affects reconnaissance tool performance.
3. Missing API credentials can limit access to certain information sources.
4. A failed query does not necessarily indicate an absence of publicly available information.
5. Reconnaissance findings should be interpreted carefully and verified before drawing security conclusions.



# 5. W2-PM5 — Network Scanning with Zenmap

## 5.1 Introduction

Zenmap is the graphical user interface for Nmap, a network discovery and security scanning tool.

It allows cybersecurity professionals to discover hosts, inspect scan results, and visualize network relationships.

For this exercise, Zenmap was installed on Windows and used to perform a Ping scan on the local network.

## 5.2 Task 1 — Zenmap Installation

Zenmap was downloaded and installed using the official Nmap website:

https://nmap.org/download.html

The installation was completed on the Windows host computer.

## 5.3 Task 2 — Local Network Identification

The Windows Command Prompt utility `ipconfig` was used to identify the active network configuration.

The confirmed network information was:

| Property | Value |
|---|---|
| Windows IPv4 address | 192.168.1.74 |
| Subnet mask | 255.255.255.0 |
| Network address | 192.168.1.0 |
| CIDR prefix | /24 |
| LAN subnet | 192.168.1.0/24 |

The subnet contains 256 IPv4 addresses, of which 254 are conventionally usable host addresses.

## 5.4 Task 3 — Live Host Discovery

A Ping scan was performed using the following Nmap command through Zenmap:

`nmap -sn 192.168.1.0/24`

**Command explanation:**

- `nmap` starts the network scanning tool.
- `-sn` enables host discovery without a port scan.
- `192.168.1.0/24` specifies the local subnet to examine.

The scan identified devices responding to host discovery probes.

## 5.5 Task 4 — Number of Live Hosts

**Result: 2 live hosts**

The scan reported two responsive hosts on the local subnet.

This does not necessarily mean that only two devices existed on the network, since some devices may not respond to discovery probes.

## 5.6 Task 5 — IP Addresses of Live Hosts

| Host | IP address |
|---|---|
| Network device (`dsldevice.lan`) | 192.168.1.254 |
| Windows host computer | 192.168.1.74 |

## 5.7 Task 6 — MAC Addresses

| IP address | MAC address | Source |
|---|---|---|
| 192.168.1.254 | 6C:C6:3B:0E:43:60 | Zenmap |
| 192.168.1.74 | 3C:F0:11:9C:23:F4 | Windows `ipconfig /all` |

The Windows host's MAC address was obtained separately because it was not displayed in the Zenmap scan output.

A MAC address identifies a network interface at the data-link layer and can help distinguish devices on a local network.

## 5.8 Task 7 — Network Topology Visualization

Zenmap's **Topology** feature was used to generate a graphical representation of the discovered hosts.

The visualization displayed the network device, the Windows host, and Zenmap's local scanning reference point.

The topology was exported to PDF format for documentation.

**Evidence:**

- [Zenmap network topology PDF](PM5-Zenmap/topology/PM5_Network_Topology.pdf)
- [Zenmap live-host discovery screenshot](PM5-Zenmap/screenshots/PM5_Zenmap_Live_Hosts.png)

## 5.9 PM5 Findings and Lessons Learned

This exercise demonstrated how to:

1. Identify a computer's IPv4 address and subnet mask.
2. Calculate the network address using CIDR notation.
3. Discover responsive hosts using a Ping scan.
4. Identify host IP addresses and available MAC addresses.
5. Interpret basic network topology visualizations.
6. Export scan evidence for technical reporting.

No vulnerabilities were confirmed by this host-discovery exercise.



# 6. Challenges and Troubleshooting

| Challenge | Investigation / Resolution |
|---|---|
| Kali Linux had no working IPv4 route | Inspected network interfaces and NetworkManager configuration |
| Static IP address conflict | Changed the Kali network connection to DHCP |
| DNS resolution failures | Restored connectivity and verified DNS resolution |
| theHarvester source errors | Retried after fixing network connectivity |
| Missing API keys | Documented unavailable sources as tool limitations |
| Zenmap did not show the local host's MAC | Retrieved it using Windows `ipconfig /all` |
| Limited network topology detail | Interpreted the results within the limitations of a Ping scan |

Troubleshooting was an important part of the project because it required identifying the cause of failed network operations rather than simply repeating unsuccessful commands.



# 7. Security Observations and Recommendations

The exercises focused on reconnaissance and network discovery, not vulnerability exploitation.

The following general security recommendations are relevant to the techniques studied:

- Organizations should maintain an inventory of their internet-facing assets.
- Publicly exposed information should be reviewed periodically.
- Unnecessary exposure of internal information should be minimized.
- Network administrators should maintain an accurate inventory of connected devices.
- Network discovery should be performed only within an authorized scope.
- Reconnaissance results should be validated before being classified as security findings.

**Important:** The presence of a discovered host or subdomain does not, by itself, establish a security vulnerability.



# 8. Skills Developed

Through these projects, I gained practical experience in:

- Open-source intelligence (OSINT)
- Passive reconnaissance
- Domain footprinting
- Network host discovery
- IPv4 addressing and subnetting
- MAC address identification
- Virtual machine networking
- Network troubleshooting
- Technical evidence collection
- Cybersecurity documentation
- GitHub project organization



# 9. Conclusion

Week 2 of the NetworkWalks Cybersecurity Internship provided practical experience in two important stages of cybersecurity assessment: information gathering and network discovery.

Using theHarvester, I explored how publicly available sources can provide information about an organization's digital footprint. Using Zenmap, I identified active hosts on a local network and documented their IP addresses, MAC addresses, and topology.

The project also strengthened my troubleshooting skills through resolving a Kali Linux network configuration problem that initially prevented successful reconnaissance.

Overall, the experience improved my understanding of cybersecurity reconnaissance tools, their limitations, and the importance of accurate technical documentation.



# 10. Project Evidence

The supporting files are organized into the following directories:

| Directory | Contents |
|---|---|
| `PM4-theHarvester/outputs/` | Saved reconnaissance outputs |
| `PM4-theHarvester/screenshots/` | Screenshots of commands and results |
| `PM5-Zenmap/screenshots/` | Network discovery screenshots |
| `PM5-Zenmap/topology/` | Exported network topology PDF |

## References

- [Nmap Official Website](https://nmap.org/)
- [theHarvester GitHub Project](https://github.com/laramies/theHarvester)
- [NetworkWalks](https://networkwalks.com/)

