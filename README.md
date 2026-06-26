# Active Directory Home Lab

## Overview

Set up a functioning Active Directory environment from scratch using VMware Workstation. The lab consists of a Windows Server 2022 Domain Controller and a Windows 11 client machine joined to the domain. The goal was to simulate a real-world AD environment and document the process end to end.

**Tools used:** VMware Workstation, Windows Server 2022, Windows 11

**Domain:** `didyourebootit.local`

---

## Environment

| Machine | OS | Role | Hostname |
|---|---|---|---|
| VM 1 | Windows Server 2022 | Domain Controller | DidYouRebootIT-Domain-Controller |
| VM 2 | Windows 11 | Domain Client | — |

---

## Part 1 — Setting Up the Domain Controller

### Static IP Configuration

Before promoting the server to a Domain Controller, I assigned it a static IP address. A DC needs a fixed IP because client machines and DNS rely on it being at a consistent, predictable address.

**Network configuration applied:**

| Setting | Value |
|---|---|
| IP Address | 192.168.112.129 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.112.2 |
| Preferred DNS | 192.168.112.129 (itself) |
| Alternate DNS | 8.8.8.8 |

The DC points to itself as the primary DNS server because it will host the DNS role and needs to resolve its own domain. The alternate DNS (Google) is set as a fallback for external name resolution in the lab environment. In a production environment this would be handled via forwarders configured in DNS Manager rather than pointing clients to an external DNS directly.

![DC IP configuration](images/1.%20Static%20IP%20No%20More.png)

### Installing Active Directory Domain Services

With the static IP in place, I installed the AD DS role through Server Manager using the Add Roles and Features wizard. After installation, I ran the post-deployment configuration to promote the server to a Domain Controller.

**Deployment configuration:**
- Operation: Add a new forest
- Root domain name: `didyourebootit.local`

![New forest](images/6.%20New%20forest.png)

The server rebooted automatically after promotion. On login, the domain `DIDYOUREBOOTIT` was visible on the login screen, confirming the DC was active.

---

## Part 2 — Joining the Windows 11 Client to the Domain

### DNS Configuration on the Client

Before joining the domain, I updated the DNS settings on the Windows 11 VM to point to the DC's IP address (`192.168.112.129`). This is a required step — the client needs to be able to resolve `didyourebootit.local`, which only the DC's DNS server knows about. Without this, the domain join fails.

![Client IP](images/2.%20Setting%20the%20IP%20of%20the%20client.png)

### Domain Join

Joined the machine to the domain via Settings > System > About > Domain or workgroup. Entered `didyourebootit.local`, authenticated with domain administrator credentials, and restarted.

![Joining a domain](images/11.%20Joining%20a%20domain.png)

After reboot, the login screen displayed the option to sign in with a domain account, confirming the machine was successfully joined.

---

## Part 3 — Creating a Domain User

On the DC, opened Active Directory Users and Computers (ADUC) and created a new user in the default Users container:

- **Display name:** Stefanija Stefanovska
- **Logon name:** (domain account)

Logged into the Windows 11 VM using this account. Login was successful, confirming the full AD authentication chain was working — client contacts DC, DC authenticates the user, session starts.

---

## Troubleshooting — DNS Resolution Failure

During setup, `nslookup didyourebootit.local` and `ping didyourebootit.local` both failed from the Windows 11 VM even though the domain was configured correctly on the DC.

**Diagnostic step:** Ran `ipconfig /all` on the Windows 11 VM and checked the DNS Servers field.

**Root cause:** A one-digit typo in the DNS server IP address on the client's NIC settings. The VM was querying the wrong address, getting no response, and failing to resolve the domain.

**Fix:** Corrected the DNS server IP to `192.168.112.129`. Both `nslookup` and `ping` resolved immediately after.

**Takeaway:** `ipconfig /all` is the right first step when DNS isn't resolving. It immediately shows which DNS server the machine is actually querying, which either confirms or rules out a misconfiguration at the client level.

---

## What's Next

- Create Organizational Units (IT, HR, Sales) and organize users into them
- Configure Group Policy Objects: password policy, drive mapping
- Simulate a helpdesk scenario: user account lockout and unlock via ADUC
