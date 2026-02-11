# Dhaka City Network - Enterprise Network Implementation

## Project Overview

This project implements a secure enterprise network connecting six major areas of Dhaka City using Cisco Packet Tracer. The network enables efficient communication for government, commerce, and education sectors.

**Base Network:** `91.16.0.0/16`

---

## Network Topology Diagram

```
                                    [91.16.0.0/21]
                                    KAMRANGIRCHAR
                                         |
                                  R-Kamrangirchar
                                         |
                              91.16.16.140/30 (Directly Attached Static)
                                         |
[91.16.16.0/25]                    R-Gulshan -------- [91.16.12.0/23]
SECRETARIAT                              |              GULSHAN
     |                          91.16.16.128/30 (Recursive Static)
     |                                   |
R-Secretariat ---------------------------|
     |\
     | \  91.16.16.132/30 (RIP)
     |  \
     |   R-Motijheel -------- [91.16.8.0/22]
     |        |                 MOTIJHEEL
     |   91.16.16.144/30 (RIP)
     |        |
     |   R-Dhanmondi -------- [91.16.14.0/24]
     |        |                 DHANMONDI
     |   91.16.16.148/30 (Floating Static Backup)
     |________|
     |
91.16.16.136/30 (RIP)
     |
R-Mirpur -------- [91.16.15.0/24]
                    MIRPUR
```

---

## VLSM Subnetting

### VLSM Tree

```
91.16.0.0/16 (Base Network)
│
├── 91.16.0.0/21 ─────────────── Kamrangirchar (1250 hosts)
│   Range: 91.16.0.1 - 91.16.7.254
│
├── 91.16.8.0/22 ─────────────── Motijheel (680 hosts)
│   Range: 91.16.8.1 - 91.16.11.254
│
├── 91.16.12.0/23 ────────────── Gulshan (430 hosts)
│   Range: 91.16.12.1 - 91.16.13.254
│
├── 91.16.14.0/24 ────────────── Dhanmondi (220 hosts)
│   Range: 91.16.14.1 - 91.16.14.254
│
├── 91.16.15.0/24 ────────────── Mirpur (180 hosts)
│   Range: 91.16.15.1 - 91.16.15.254
│
├── 91.16.16.0/25 ────────────── Secretariat (95 hosts)
│   Range: 91.16.16.1 - 91.16.16.126
│
└── 91.16.16.128/25 ─────────── Point-to-Point Links
    ├── 91.16.16.128/30 ──────── Link 1: Secretariat ↔ Gulshan
    ├── 91.16.16.132/30 ──────── Link 2: Secretariat ↔ Motijheel
    ├── 91.16.16.136/30 ──────── Link 3: Secretariat ↔ Mirpur
    ├── 91.16.16.140/30 ──────── Link 4: Gulshan ↔ Kamrangirchar
    ├── 91.16.16.144/30 ──────── Link 5: Motijheel ↔ Dhanmondi
    └── 91.16.16.148/30 ──────── Link 6: Dhanmondi ↔ Secretariat (Backup)
```

### Host Requirements Calculation

| Location      | Hosts Required | Usable Needed | Block Size | Subnet Mask     | CIDR |
| ------------- | -------------- | ------------- | ---------- | --------------- | ---- |
| Kamrangirchar | 1250           | 1252          | 2048       | 255.255.248.0   | /21  |
| Motijheel     | 680            | 682           | 1024       | 255.255.252.0   | /22  |
| Gulshan       | 430            | 432           | 512        | 255.255.254.0   | /23  |
| Dhanmondi     | 220            | 222           | 256        | 255.255.255.0   | /24  |
| Mirpur        | 180            | 182           | 256        | 255.255.255.0   | /24  |
| Secretariat   | 95             | 97            | 128        | 255.255.255.128 | /25  |

---

## IP Address Tables

### LAN Networks

| Location      | Network Address | Subnet Mask     | Gateway    | Usable Range              | Broadcast    |
| ------------- | --------------- | --------------- | ---------- | ------------------------- | ------------ |
| Kamrangirchar | 91.16.0.0/21    | 255.255.248.0   | 91.16.0.1  | 91.16.0.2 - 91.16.7.254   | 91.16.7.255  |
| Motijheel     | 91.16.8.0/22    | 255.255.252.0   | 91.16.8.1  | 91.16.8.2 - 91.16.11.254  | 91.16.11.255 |
| Gulshan       | 91.16.12.0/23   | 255.255.254.0   | 91.16.12.1 | 91.16.12.2 - 91.16.13.254 | 91.16.13.255 |
| Dhanmondi     | 91.16.14.0/24   | 255.255.255.0   | 91.16.14.1 | 91.16.14.2 - 91.16.14.254 | 91.16.14.255 |
| Mirpur        | 91.16.15.0/24   | 255.255.255.0   | 91.16.15.1 | 91.16.15.2 - 91.16.15.254 | 91.16.15.255 |
| Secretariat   | 91.16.16.0/25   | 255.255.255.128 | 91.16.16.1 | 91.16.16.2 - 91.16.16.126 | 91.16.16.127 |

### Point-to-Point WAN Links

| Link | Network         | Router A      | Port A  | IP A         | Router B        | Port B  | IP B         |
| ---- | --------------- | ------------- | ------- | ------------ | --------------- | ------- | ------------ |
| 1    | 91.16.16.128/30 | R-Secretariat | Se0/0/0 | 91.16.16.129 | R-Gulshan       | Se0/0/0 | 91.16.16.130 |
| 2    | 91.16.16.132/30 | R-Secretariat | Se0/0/1 | 91.16.16.133 | R-Motijheel     | Se0/0/0 | 91.16.16.134 |
| 3    | 91.16.16.136/30 | R-Secretariat | Se0/1/0 | 91.16.16.137 | R-Mirpur        | Se0/0/0 | 91.16.16.138 |
| 4    | 91.16.16.140/30 | R-Gulshan     | Se0/0/1 | 91.16.16.141 | R-Kamrangirchar | Se0/0/0 | 91.16.16.142 |
| 5    | 91.16.16.144/30 | R-Motijheel   | Se0/0/1 | 91.16.16.145 | R-Dhanmondi     | Se0/0/0 | 91.16.16.146 |
| 6    | 91.16.16.148/30 | R-Dhanmondi   | Se0/0/1 | 91.16.16.149 | R-Secretariat   | Se0/1/1 | 91.16.16.150 |

### Server IP Assignments (Secretariat)

| Server       | IP Address | Subnet Mask     | Gateway    | DNS        | Purpose                     |
| ------------ | ---------- | --------------- | ---------- | ---------- | --------------------------- |
| DNS Server   | 91.16.16.2 | 255.255.255.128 | 91.16.16.1 | 91.16.16.2 | DNS Resolution              |
| Web Server   | 91.16.16.3 | 255.255.255.128 | 91.16.16.1 | 91.16.16.2 | www.dhaka.gov               |
| Email Server | 91.16.16.4 | 255.255.255.128 | 91.16.16.1 | 91.16.16.2 | Mail Services               |
| DHCP Server  | 91.16.16.5 | 255.255.255.128 | 91.16.16.1 | 91.16.16.2 | DHCP for Mirpur & Dhanmondi |

### End Device IP Assignments

#### Static IP Areas

| Device            | IP Address  | Subnet Mask     | Gateway    | DNS        |
| ----------------- | ----------- | --------------- | ---------- | ---------- |
| PC1-Secretariat   | 91.16.16.10 | 255.255.255.128 | 91.16.16.1 | 91.16.16.2 |
| PC2-Secretariat   | 91.16.16.11 | 255.255.255.128 | 91.16.16.1 | 91.16.16.2 |
| PC1-Gulshan       | 91.16.12.10 | 255.255.254.0   | 91.16.12.1 | 91.16.16.2 |
| PC2-Gulshan       | 91.16.12.11 | 255.255.254.0   | 91.16.12.1 | 91.16.16.2 |
| PC1-Motijheel     | 91.16.8.10  | 255.255.252.0   | 91.16.8.1  | 91.16.16.2 |
| PC2-Motijheel     | 91.16.8.11  | 255.255.252.0   | 91.16.8.1  | 91.16.16.2 |
| PC1-Kamrangirchar | 91.16.0.10  | 255.255.248.0   | 91.16.0.1  | 91.16.16.2 |
| PC2-Kamrangirchar | 91.16.0.11  | 255.255.248.0   | 91.16.0.1  | 91.16.16.2 |

#### DHCP Areas

| Device        | IP Assignment | Expected Range | Gateway    | DNS        |
| ------------- | ------------- | -------------- | ---------- | ---------- |
| PC1-Mirpur    | DHCP          | 91.16.15.11+   | 91.16.15.1 | 91.16.16.2 |
| PC2-Mirpur    | DHCP          | 91.16.15.11+   | 91.16.15.1 | 91.16.16.2 |
| PC1-Dhanmondi | DHCP          | 91.16.14.11+   | 91.16.14.1 | 91.16.16.2 |
| PC2-Dhanmondi | DHCP          | 91.16.14.11+   | 91.16.14.1 | 91.16.16.2 |

---

## Router Configurations

### R-Secretariat

```
enable
configure terminal
hostname R-Secretariat

!  LAN Interface
interface GigabitEthernet0/0
 ip address 91.16.16.1 255.255.255.128
 no shutdown
 exit

!  Serial to Gulshan (Recursive Static Route)
interface Serial0/0/0
 ip address 91.16.16.129 255.255.255.252
 clock rate 64000
 no shutdown
 exit

! Serial to Motijheel (RIP)
interface Serial0/0/1
 ip address 91.16.16.133 255.255.255.252
 clock rate 64000
 no shutdown
 exit

!  Serial to Mirpur (RIP)
interface Serial0/1/0
 ip address 91.16.16.137 255.255.255.252
 clock rate 64000
 no shutdown
 exit

! Serial to Dhanmondi (Backup - Floating Static receives here)
interface Serial0/1/1
 ip address 91.16.16.150 255.255.255.252
 no shutdown
 exit

!  DHCP Excluded Addresses
ip dhcp excluded-address 91.16.15.1 91.16.15.10
ip dhcp excluded-address 91.16.14.1 91.16.14.10

! DHCP Pool for Mirpur
ip dhcp pool MIRPUR-POOL
 network 91.16.15.0 255.255.255.0
 default-router 91.16.15.1
 dns-server 91.16.16.2
 exit

! DHCP Pool for Dhanmondi
ip dhcp pool DHANMONDI-POOL
 network 91.16.14.0 255.255.255.0
 default-router 91.16.14.1
 dns-server 91.16.16.2
 exit

!  Recursive Static Route to Gulshan LAN (via next-hop IP)
ip route 91.16.12.0 255.255.254.0 91.16.16.130

! Recursive Static Route to Kamrangirchar (via Gulshan)
ip route 91.16.16.140 255.255.255.252 91.16.16.130
ip route 91.16.0.0 255.255.248.0 91.16.16.130

! RIP Configuration
router rip
 version 2
 no auto-summary
 redistribute static
 network 91.16.16.0
 passive-interface GigabitEthernet0/0
 exit

end
copy run start
```

### R-Gulshan

```
enable
configure terminal
hostname R-Gulshan

! LAN Interface
interface GigabitEthernet0/0
 ip address 91.16.12.1 255.255.254.0
 no shutdown
 exit

! Serial to Secretariat
interface Serial0/0/0
 ip address 91.16.16.130 255.255.255.252
 no shutdown
 exit

! Serial to Kamrangirchar (Directly Attached Static)
interface Serial0/0/1
 ip address 91.16.16.141 255.255.255.252
 clock rate 64000
 no shutdown
 exit

!  Recursive Static Route to Secretariat LAN
ip route 91.16.16.0 255.255.255.128 91.16.16.129

! Directly Attached Static Route to Kamrangirchar LAN
ip route 91.16.0.0 255.255.248.0 Serial0/0/1

! Static routes to RIP networks via Secretariat
ip route 91.16.8.0 255.255.252.0 91.16.16.129
ip route 91.16.14.0 255.255.255.0 91.16.16.129
ip route 91.16.15.0 255.255.255.0 91.16.16.129

! Static routes to WAN links
ip route 91.16.16.132 255.255.255.252 91.16.16.129
ip route 91.16.16.136 255.255.255.252 91.16.16.129
ip route 91.16.16.144 255.255.255.252 91.16.16.129
ip route 91.16.16.148 255.255.255.252 91.16.16.129

end
copy run start
```

### R-Kamrangirchar

```
enable
configure terminal
hostname R-Kamrangirchar

! LAN Interface
interface GigabitEthernet0/0
 ip address 91.16.0.1 255.255.248.0
 no shutdown
 exit

! Serial to Gulshan
interface Serial0/0/0
 ip address 91.16.16.142 255.255.255.252
 no shutdown
 exit

!  Directly Attached Static Route to Gulshan LAN
ip route 91.16.12.0 255.255.254.0 Serial0/0/0

! Static routes to all other networks via Gulshan (directly attached)
ip route 91.16.16.0 255.255.255.128 Serial0/0/0
ip route 91.16.8.0 255.255.252.0 Serial0/0/0
ip route 91.16.14.0 255.255.255.0 Serial0/0/0
ip route 91.16.15.0 255.255.255.0 Serial0/0/0

! Static routes to WAN links
ip route 91.16.16.128 255.255.255.252 Serial0/0/0
ip route 91.16.16.132 255.255.255.252 Serial0/0/0
ip route 91.16.16.136 255.255.255.252 Serial0/0/0
ip route 91.16.16.144 255.255.255.252 Serial0/0/0
ip route 91.16.16.148 255.255.255.252 Serial0/0/0

end
copy run start
```

### R-Motijheel

```
enable
configure terminal
hostname R-Motijheel

! LAN Interface
interface GigabitEthernet0/0
 ip address 91.16.8.1 255.255.252.0
 no shutdown
 exit

! Serial to Secretariat
interface Serial0/0/0
 ip address 91.16.16.134 255.255.255.252
 no shutdown
 exit

! Serial to Dhanmondi
interface Serial0/0/1
 ip address 91.16.16.145 255.255.255.252
 clock rate 64000
 no shutdown
 exit

! RIP Configuration
router rip
 version 2
 no auto-summary
 network 91.16.8.0
 network 91.16.16.132
 network 91.16.16.144
 passive-interface GigabitEthernet0/0
 exit

!  Floating Static Route (Backup) to Secretariat via Dhanmondi
!  Administrative distance 130 (higher than RIP's 120)
ip route 91.16.16.0 255.255.255.128 91.16.16.146 130

end
copy run start
```

### R-Dhanmondi

```
enable
configure terminal
hostname R-Dhanmondi

!  LAN Interface with DHCP Relay
interface GigabitEthernet0/0
 ip address 91.16.14.1 255.255.255.0
 ip helper-address 91.16.16.5
 no shutdown
 exit

! Serial to Motijheel
interface Serial0/0/0
 ip address 91.16.16.146 255.255.255.252
 no shutdown
 exit

! Serial to Secretariat (Backup link)
interface Serial0/0/1
 ip address 91.16.16.149 255.255.255.252
 clock rate 64000
 no shutdown
 exit

! RIP Configuration
router rip
 version 2
 no auto-summary
 network 91.16.14.0
 network 91.16.16.144
 network 91.16.16.148
 passive-interface GigabitEthernet0/0
 exit

end
copy run start
```

### R-Mirpur

```
enable
configure terminal
hostname R-Mirpur

! LAN Interface with DHCP Relay
interface GigabitEthernet0/0
 ip address 91.16.15.1 255.255.255.0
 ip helper-address 91.16.16.5
 no shutdown
 exit

! Serial to Secretariat
interface Serial0/0/0
 ip address 91.16.16.138 255.255.255.252
 no shutdown
 exit

! RIP Configuration
router rip
 version 2
 no auto-summary
 network 91.16.15.0
 network 91.16.16.136
 passive-interface GigabitEthernet0/0
 exit

end
copy run start
```

---

## Server Configurations

### DNS Server

**IP Configuration:**

- IP Address: 91.16.16.2
- Subnet Mask: 255.255.255.128
- Default Gateway: 91.16.16.1
- DNS Server: 91.16.16.2

**DNS Records:**

| Name           | Type     | Address    |
| -------------- | -------- | ---------- |
| www.dhaka.gov  | A Record | 91.16.16.3 |
| dhaka.gov      | A Record | 91.16.16.3 |
| mail.dhaka.gov | A Record | 91.16.16.4 |

### Web Server

**IP Configuration:**

- IP Address: 91.16.16.3
- Subnet Mask: 255.255.255.128
- Default Gateway: 91.16.16.1
- DNS Server: 91.16.16.2

**index.html Content:**

```html
<html>
  <head>
    <title>Dhaka City Network</title>
  </head>
  <body>
    <h1>Welcome to the Dhaka City Network</h1>
    <p>Official Government Portal of Dhaka City</p>
    <p>
      Connecting Secretariat, Gulshan, Mirpur, Motijheel, Kamrangirchar, and
      Dhanmondi
    </p>
  </body>
</html>
```

### Email Server

**IP Configuration:**

- IP Address: 91.16.16.4
- Subnet Mask: 255.255.255.128
- Default Gateway: 91.16.16.1
- DNS Server: 91.16.16.2

**Email Configuration:**

- Domain Name: dhaka.gov

**User Accounts:**

| User          | Password |
| ------------- | -------- |
| secretariat   | password |
| gulshan       | password |
| kamrangirchar | password |

### DHCP Server

**IP Configuration:**

- IP Address: 91.16.16.5
- Subnet Mask: 255.255.255.128
- Default Gateway: 91.16.16.1
- DNS Server: 91.16.16.2

**DHCP Pools:**

| Pool Name | Default Gateway | DNS Server | Start IP    | Subnet Mask   | Max Users |
| --------- | --------------- | ---------- | ----------- | ------------- | --------- |
| MIRPUR    | 91.16.15.1      | 91.16.16.2 | 91.16.15.11 | 255.255.255.0 | 170       |
| DHANMONDI | 91.16.14.1      | 91.16.16.2 | 91.16.14.11 | 255.255.255.0 | 200       |

---

## Access Control List (Email Restriction)

Email service is restricted to Secretariat, Gulshan, and Kamrangirchar only.

**Apply on R-Secretariat:**

```
enable
configure terminal

! Block email (SMTP port 25, POP3 port 110) from unauthorized networks
access-list 100 deny tcp 91.16.15.0 0.0.0.255 host 91.16.16.4 eq 25
access-list 100 deny tcp 91.16.15.0 0.0.0.255 host 91.16.16.4 eq 110
access-list 100 deny tcp 91.16.14.0 0.0.0.255 host 91.16.16.4 eq 25
access-list 100 deny tcp 91.16.14.0 0.0.0.255 host 91.16.16.4 eq 110
access-list 100 deny tcp 91.16.8.0 0.0.3.255 host 91.16.16.4 eq 25
access-list 100 deny tcp 91.16.8.0 0.0.3.255 host 91.16.16.4 eq 110
access-list 100 permit ip any any

interface GigabitEthernet0/0
 ip access-group 100 out
 exit

end
copy run start
```

---

## Routing Summary

### Routing Types Used

| Route Type               | Path                                  | Description                  |
| ------------------------ | ------------------------------------- | ---------------------------- |
| Recursive Static         | Secretariat ↔ Gulshan                 | Next-hop IP specified        |
| Directly Attached Static | Gulshan ↔ Kamrangirchar               | Exit interface specified     |
| RIP v2                   | All other paths                       | Dynamic routing              |
| Floating Static (Backup) | Motijheel → Secretariat via Dhanmondi | AD=130, activates on failure |

### RIP Networks Advertised

| Router        | Networks                                                                    |
| ------------- | --------------------------------------------------------------------------- |
| R-Secretariat | 91.16.16.0, 91.16.16.128, 91.16.16.132, 91.16.16.136 + redistributed static |
| R-Motijheel   | 91.16.8.0, 91.16.16.132, 91.16.16.144                                       |
| R-Dhanmondi   | 91.16.14.0, 91.16.16.144, 91.16.16.148                                      |
| R-Mirpur      | 91.16.15.0, 91.16.16.136                                                    |

---

## Testing Procedures

### 1. Connectivity Test (Ping)

From any PC, test connectivity to all gateways:

```
ping 91.16.16.1    (Secretariat)
ping 91.16.12.1    (Gulshan)
ping 91.16.0.1     (Kamrangirchar)
ping 91.16.8.1     (Motijheel)
ping 91.16.14.1    (Dhanmondi)
ping 91.16.15.1    (Mirpur)
```

### 2. Web Access Test

From any PC → Desktop → Web Browser:

- URL: `www.dhaka.gov`
- Expected: "Welcome to the Dhaka City Network"

### 3. DHCP Verification

On Mirpur/Dhanmondi PCs → Command Prompt:

```
ipconfig
```

Verify IP is in correct DHCP range.

### 4. Email Test

1. From PC1-Secretariat → Desktop → Email → Compose
2. To: gulshan@dhaka.gov
3. Send email
4. On PC1-Gulshan → Email → Receive
5. Verify email received

### 5. Routing Table Verification

On each router:

```
show ip route
show ip rip database
show ip interface brief
```

### 6. Floating Static Route Test

1. On R-Secretariat, shutdown main link:

```
interface Serial0/0/1
shutdown
```

2. From Motijheel PC, ping 91.16.16.1
3. Traffic routes through Dhanmondi backup path

4. Restore link:

```
interface Serial0/0/1
no shutdown
```

---

## Troubleshooting Commands

```
show ip route                    !  View routing table
show ip interface brief          ! View interface status
show ip dhcp binding             ! View DHCP leases
show ip dhcp pool                ! View DHCP pool statistics
show running-config              ! View current configuration
show ip rip database             ! View RIP learned routes
show access-lists                ! View ACL configuration
ping <ip-address>                ! Test connectivity
traceroute <ip-address>          ! Trace packet path
```

---

## Files Included

1. `DhakaNetwork.pkt` - Cisco Packet Tracer file
2. `README.md` - This documentation
3. `NetworkTopology.png` - Network topology screenshot with labels
4. Command lines for all the routers

---

## Requirements Checklist

- [x] VLSM subnetting with base network 91.16.0.0/16
- [x] 6 unique subnetwork addresses assigned
- [x] DNS Server configured at Secretariat
- [x] Web Server with "Welcome to the Dhaka City Network" message
- [x] Web accessible as www.dhaka.gov from all areas
- [x] Email Server configured for Secretariat, Gulshan, Kamrangirchar only
- [x] DHCP Server providing IPs to Mirpur and Dhanmondi
- [x] Static IP addressing for other areas
- [x] Recursive static route: Secretariat ↔ Gulshan
- [x] Directly attached static route: Gulshan ↔ Kamrangirchar
- [x] RIP v2 for all other paths
- [x] Floating static backup: Motijheel → Secretariat via Dhanmondi
- [x] No default routes configured
- [x] All areas can ping each other



---

## Contribution Note

This repository is a fork of the original group project completed as part of our undergraduate coursework.
