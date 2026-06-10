# 🎫 Kerberoasting Attack - CTF Challenge

## 📋 Overview

**Kerberoasting Attack** is an interactive, browser-based Capture The Flag (CTF) challenge designed for cybersecurity training. This challenge focuses on detecting Kerberoasting attacks where attackers request Ticket Granting Service (TGS) tickets for multiple service accounts to extract and crack their passwords offline. Participants analyze Event 4769 (TGS-REQ) logs to identify suspicious patterns and implement proper remediation.

## 🎯 Learning Objectives

By completing this CTF, participants will learn:

- **Kerberoasting Detection**: Identify TGS requests with RC4 encryption for multiple service accounts
- **Event Log Analysis**: Analyze Event 4769 for suspicious service ticket requests
- **Attack Pattern Recognition**: Detect single user requesting tickets for multiple SPNs
- **Password Cracking Awareness**: Understand how weak service account passwords enable offline cracking
- **Remediation Strategies**: Implement Group Managed Service Accounts (gMSA) and strong passwords

## 🛠️ Challenge Tasks (5 Total)

| Task | Description | Skill Focus |
|------|-------------|-------------|
| **Task 1** | Identify the attack technique (Kerberoasting) | Attack Recognition |
| **Task 2** | Find log event with RC4 encryption (Event 4769) | Event Log Analysis |
| **Task 3** | Identify the malicious user account (jsmith) | User Attribution |
| **Task 4** | Determine offline cracking tool (hashcat) | Password Cracking |
| **Task 5** | Recommend remediation (gMSA) | Remediation |

## 🚀 Quick Start

### Prerequisites
- A modern web browser (Chrome, Firefox, Edge, Safari)
- No server required - runs entirely in the browser
- No installation needed

### Access the Challenge
1. Open the HTML file directly in your browser
2. Enter your name
3. Use the password: `45_2026`
4. Complete all 5 tasks to capture the flag

### Hosting on GitHub Pages
1. Fork or clone this repository
2. Go to repository Settings > Pages
3. Select the branch (usually `main`) and save
4. Access via `https://your-username.github.io/repository-name`

## 🎮 How to Play

### Login
```
Password: 45_2026
Name: Enter any name (progress is saved locally)
```

### Game Features

- **Attack Flow Diagram**: Visual representation of Kerberoasting attack with pulsing attacker node
- **Kerberos Ticket Viewer**: Simulated Event 4769 logs showing TGS requests with encryption types
- **Color-coded Encryption Badges**: RC4 (red/crackable) vs AES (green/secure)
- **Service Principal Display**: List of requested SPNs with timestamps
- **hashcat Command Reference**: Example cracking command for TGS hashes
- **gMSA Remediation Guidance**: Best practices for service account management
- **Answer Validation**: Immediate feedback on submitted answers
- **Progress Tracking**: Local storage saves your progress across sessions

### Completing Tasks
1. Read each task description carefully
2. Analyze the Kerberos ticket viewer and event logs
3. Identify the suspicious user and attack pattern
4. Type your answer in the input field
5. Click "Submit" to validate
6. Complete all 5 tasks to reveal the flag

## 🏆 Flag

```
FLAG{KERBEROASTING}
```

The flag is revealed only after completing all 5 tasks successfully.

## 📊 Challenge Details

### Attack Flow Diagram

```
⚔️ Kerberoasting Attack Flow:
├─ Attacker (jsmith)
│  ├── ❶ TGS-REQ (multiple SPNs) → Domain Controller
│  ├── ❷ TGS-REP (RC4 encrypted) ← Domain Controller
│  └── ❸ Extract hash → Crack offline
└─ Domain Controller
   └── Returns service tickets without checking privileges
```

### TGS Request Log (Event 4769)

```
Time     | User   | Service (SPN)              | Encryption | Risk
---------|--------|-----------------------------|------------|---------
08:15:01 | jsmith | MSSQLSvc/sql01.corp.local  | RC4        | ⚠️ KERBEROASTING
08:15:02 | jsmith | HTTP/spweb01.corp.local    | RC4        | ⚠️ KERBEROASTING
08:15:03 | jsmith | TERMSRV/ts01.corp.local    | RC4        | ⚠️ KERBEROASTING
08:15:04 | jsmith | MSSQLSvc/sql02.corp.local  | RC4        | ⚠️ KERBEROASTING
08:15:05 | jsmith | CIFS/file01.corp.local     | RC4        | ⚠️ KERBEROASTING
08:15:06 | jsmith | HTTP/intranet.corp.local   | RC4        | ⚠️ KERBEROASTING
08:30:00 | asmith | CIFS/file01.corp.local     | AES256     | ✅ Normal
09:00:00 | jdoe   | HTTP/spweb01.corp.local    | AES128     | ✅ Normal
```

## 🔍 Investigation Walkthrough

### Task 1: Identify the Attack
The attack is **Kerberoasting**. This technique exploits how Kerberos works:
- Any authenticated domain user can request TGS tickets for any service
- The TGS ticket is encrypted with the service account's password hash
- Attackers extract these tickets and crack them offline
- No special privileges are required to request tickets
- Weak service account passwords are vulnerable to cracking

### Task 2: Identify Log Event
The key event is **Event 4769 with RC4 encryption**. RC4 encryption is significant because:
- RC4 is weaker and faster to crack than AES
- Most modern environments default to AES encryption
- RC4 requests may indicate downgrade attacks
- Attackers often request RC4 for easier cracking
- The combination of Event 4769 + RC4 = strong Kerberoasting indicator

### Task 3: Find Malicious User
**jsmith** is the attacker. This user is suspicious because:
- Requested 6 different service tickets in 5 seconds
- All requests use RC4 encryption
- Requests span multiple service types (MSSQL, HTTP, TERMSRV, CIFS)
- Normal users rarely request tickets for so many different services
- The rapid succession indicates automated tool usage

### Task 4: Offline Cracking
**hashcat** is the primary tool for cracking Kerberos TGS hashes. The process involves:
- Extract TGS hashes from network traffic or memory
- Use hashcat mode 13100 for Kerberos 5 TGS-REP
- Crack against wordlists (rockyou.txt) or rules
- Successful cracking reveals service account password
- Service account passwords often have elevated privileges

### Task 5: Remediation
**gMSA** (Group Managed Service Accounts) is the recommended solution because:
- Passwords are automatically managed by Active Directory
- Passwords rotate regularly (30 days by default)
- Passwords are complex (120+ characters)
- No administrator knows the password
- Eliminates the risk of weak manually-set passwords
- Prevents Kerberoasting by making passwords uncrackable

## 🎨 Visual Features

- **Attack Flow Diagram**: Visual representation with color-coded nodes and arrows
- **Pulsing Attacker Node**: Red pulsing animation on the attacker entity
- **Kerberos Ticket Table**: Tabular display with row highlighting for suspicious entries
- **Encryption Badges**: Red (RC4) and Green (AES) encryption type indicators
- **Timeline Display**: Chronological ordering of TGS requests
- **hashcat Command Display**: Monospace-formatted cracking command examples
- **Progress Indicators**: Visual completion status for each task
- **Glowing Flag Animation**: Celebratory golden flag reveal
- **Toast Notifications**: Non-intrusive success/error messages
- **Dark Theme**: Orange-accented UI for Kerberos attack theme

## 💾 Data Storage

- **Progress**: Saved in browser's `localStorage`
- **Persistence**: Progress survives page refreshes
- **Privacy**: All data stays on the user's device
- **Reset**: Clear browser data to reset progress

## 🛡️ Kerberoasting Detection Indicators

### High-Fidelity Indicators:
- Single user requesting TGS for 5+ different SPNs in under 10 seconds
- TGS requests using RC4 encryption type (0x17)
- Multiple Event 4769 from same source IP in rapid succession
- Service tickets requested for accounts with weak passwords
- TGS-REQ without subsequent service access

### Medium-Fidelity Indicators:
- TGS requests for rarely-accessed service accounts
- Requests occurring during non-business hours
- Single source requesting tickets across multiple target servers
- Unusual SPN combinations being requested

### Low-Fidelity Indicators:
- Single TGS-REQ with RC4 encryption
- Occasional service ticket requests from workstations
- Standard administrative tool usage patterns

## 📁 File Structure

```
kerberoasting-attack/
│
├── index.html          # Main CTF challenge file
├── README.md           # This documentation
└── (no other files required)
```

## 🔧 Technical Implementation

- **Pure Frontend**: HTML5, CSS3, JavaScript (Vanilla)
- **No Dependencies**: Zero external libraries
- **Responsive Design**: Works on desktop and mobile
- **Animations**: CSS keyframe animations for attacker node and flow diagram
- **Storage**: Browser localStorage API
- **Gamification**: Progress tracking, badge system, visual rewards
- **Table Visualization**: Styled ticket viewer with highlighted suspicious rows

## 📊 Kerberoasting Attack Flow

```
1. Domain Enumeration
   └── Attacker enumerates service accounts (SPNs)

2. Ticket Requests
   ├── Request TGS for each discovered SPN
   ├── No special privileges required
   └── Domain Controller returns encrypted tickets

3. Ticket Extraction
   ├── Extract TGS-REP from network traffic
   ├── Save encrypted service ticket hash
   └── Contains service account password hash

4. Offline Cracking
   ├── Use hashcat mode 13100
   ├── Crack against wordlists
   ├── Use rules and masks
   └── Recover plaintext password

5. Privilege Escalation
   ├── Service accounts often have elevated privileges
   ├── Use cracked password for lateral movement
   ├── Access sensitive systems
   └── Potential Domain Admin compromise
```

## 🔑 Kerberos Ticket Types Reference

### TGT (Ticket Granting Ticket)
- Obtained during initial authentication (AS-REQ/AS-REP)
- Encrypted with KRBTGT hash
- Used to request service tickets
- Event 4768 records TGT requests

### TGS (Ticket Granting Service)
- Requested using TGT (TGS-REQ/TGS-REP)
- Encrypted with service account's password hash
- Can be cracked offline if weak password
- Event 4769 records TGS requests

### Kerberoasting vs Golden Ticket

| Feature | Kerberoasting | Golden Ticket |
|---------|--------------|---------------|
| Privilege Required | Any domain user | KRBTGT hash |
| Target | Service account passwords | Any user/TGT |
| Detection | Event 4769 + RC4 | Missing Event 4768 |
| Impact | Service account compromise | Complete domain compromise |
| Remediation | gMSA + strong passwords | KRBTGT reset x2 |

## 🎓 Educational Use Cases

- **Cybersecurity Training Programs**
- **SOC Analyst Onboarding**
- **Active Directory Security Training**
- **Blue Team Exercises**
- **Incident Response Training**
- **Academic Courses** (Kerberos Security, AD Attacks)
- **Self-paced Learning**
- **Purple Team Exercises**

## 🔄 Version History

- **v1.0** - Initial release
  - 5 tasks with validation
  - Attack flow diagram with pulsing attacker node
  - Simulated Event 4769 logs with 8 entries
  - Encryption type visualization (RC4 vs AES)
  - hashcat command reference
  - Local storage progress tracking
  - Student login system

## 👥 Target Audience

- Security Operations Center (SOC) Analysts
- Incident Response Team Members
- Active Directory Security Specialists
- Threat Hunters
- Windows System Administrators
- Cybersecurity Students
- IT Security Professionals
- Blue Team Practitioners

---

**Happy Kerberoasting Detection! 🎫**
