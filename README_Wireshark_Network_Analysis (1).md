# Wireshark Network Traffic Analysis

## Project overview
A practical network troubleshooting and packet-analysis exercise using Kali Linux, Nmap, Wireshark and Windows PowerShell. The lab explores host discovery, ICMP reachability, TCP connection establishment, HTTP request/response inspection and troubleshooting a later failed connection.

## Lab details
- Kali Linux: `10.0.0.6/24` on `eth0`
- Windows test host: `10.0.0.5` (as shown in the ICMP and HTTP test evidence)
- Additional Nmap target: `10.0.0.4`
- HTTP test service: `10.0.0.6:8000`

> The Nmap scan target (`10.0.0.4`) is distinct from the two hosts shown in the ICMP/HTTP tests (`10.0.0.5` and `10.0.0.6`). Do not conflate these addresses.

## Objectives
- Identify reachable systems and exposed TCP services in an authorised lab.
- Capture ICMP echo requests and replies.
- Observe the TCP three-way handshake.
- Inspect HTTP GET and HTTP 200 OK traffic.
- Investigate a later failed TCP connection test.
- Document findings and security recommendations.

## Commands used

```bash
nmap -sS -sV 10.0.0.4
hostname -i
ip addr show eth0
ping -c 4 10.0.0.5
```

Windows PowerShell:

```powershell
Invoke-WebRequest -Uri http://10.0.0.6:8000/ -UseBasicParsing
Test-NetConnection 10.0.0.6 -Port 8000
```

## Useful Wireshark display filters

| Filter | Purpose |
|---|---|
| `icmp` | ICMP echo traffic |
| `tcp.port == 8000` | Traffic involving TCP port 8000 |
| `tcp` | TCP packets |
| `http` | Decoded HTTP traffic |
| `ip.addr == 10.0.0.5` | Packets involving the Windows test host |
| `tcp.flags.reset == 1` | TCP reset packets |

## Key observations
1. Kali used `10.0.0.6/24`.
2. ICMP requests from `10.0.0.6` to `10.0.0.5` received replies.
3. Nmap reported several open Windows-associated services on `10.0.0.4`, including ports 135, 139, 445 and 3389.
4. A PowerShell HTTP request to `http://10.0.0.6:8000/` returned `200 OK` and the page content `Training Lab Test Page`.
5. Wireshark showed the TCP SYN, SYN/ACK and ACK sequence followed by an HTTP GET and response.
6. A later TCP connectivity test to port 8000 failed. The screenshot does not establish the cause; possible explanations include service state, firewall rules or lab timing.
7. A request for `/favicon.ico` returned `404 File not found`, which indicates the icon resource was missing rather than the main test page failing.

## Security recommendations
- Disable unnecessary services and close unused ports.
- Restrict SMB, RPC, RDP and management services to trusted networks.
- Apply host firewalls and network segmentation.
- Use HTTPS instead of HTTP for real applications.
- Patch systems and review service exposure regularly.
- Sanitise packet captures before sharing them publicly.

## Limitations
This is a controlled training exercise, not a full vulnerability assessment. An open port does not prove a vulnerability, service fingerprints can be imperfect, and the reason for the later failed connection is not proven by the available evidence.

## Evidence and report
See `report/Wireshark_Network_Traffic_Analysis_Project.pdf` and the `screenshots/` directory.

## Author
[Your name or GitHub username]

**Scope:** Only scan systems you own or have explicit permission to assess.
