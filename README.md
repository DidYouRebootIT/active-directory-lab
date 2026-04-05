
# 🧠 Case Study: Active Directory Lab – Initial Setup (v1)

**📅 Date:** March 2026

---

## 🎯 Goal

To simulate a basic enterprise environment using Active Directory Domain Services (AD DS) by creating a domain controller, organizing resources into structured Organizational Units and managing users and groups.

---

⚙️ Environment
Virtual Machine: Windows Server 2022
Role Installed: Active Directory Domain Services (AD DS)
Domain Controller: Configured and promoted
Tools Used: Server Manager, Active Directory Users and Computers (ADUC)

---

🧩 Tasks Performed
Installed and configured Active Directory Domain Services
Promoted the server to a Domain Controller
Created a structured OU hierarchy to simulate a multi-country organization:
Macedonia
UK
Germany
Within each country OU, created:
Users
Computers
Servers
Created distribution groups within the Macedonia Users OU
Created and configured a user account:
Petko Petkovski
Assigned the user to relevant groupsg)

---

Domain
│
├── Macedonia
│   ├── Users
│   ├── Computers
│   └── Servers
│
├── UK
│   ├── Users
│   ├── Computers
│   └── Servers
│
└── Germany
    ├── Users
    ├── Computers
    └── Servers

---

📸 Screenshots

- ![Installing AD DS](https://github.com/DidYouRebootIT/active-directory-lab/blob/main/images/1.%20Installing%20AD%20DS.jpg)
- ![Promoting the server to a Domain Controller](https://github.com/DidYouRebootIT/active-directory-lab/blob/main/images/2.%20Promoting%20the%20server%20to%20a%20Domain%20Controller.jpg)
- ![Succesfully installed AD DS](https://github.com/DidYouRebootIT/active-directory-lab/blob/main/images/3.%20Succesfully%20isntalled%20AD%20DS.jpg)
- ![Organizational units, groups and users](https://github.com/DidYouRebootIT/active-directory-lab/blob/main/images/4.%20Organizational%20units%2C%20groups%20and%20users.jpg)
  
---

💡 What I Learned
How to install and configure Active Directory Domain Services
The process of promoting a server to a Domain Controller
Best practices for structuring Organizational Units
Basic user and group management in AD
Differences between distribution groups and security groups

---

Next Steps / In Progress

Add more users and simulate departments
Implement Group Policy Objects (GPOs)
Join client machines to domain
