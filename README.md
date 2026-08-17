# Active Directory Administration Lab in Microsoft Azure

## Overview

In this lab, I built and administered a Windows Active Directory environment in Microsoft Azure. I deployed a Windows Server 2022 Domain Controller and Windows 10 client, configured DNS and domain connectivity, created and managed Active Directory users and organizational units, automated user creation with PowerShell, configured account lockout policies with Group Policy, and reviewed Windows event logs.

This project gave me hands-on experience with common IT support and Windows system administration tasks such as domain joins, user provisioning, password resets, account lockouts, permissions, Remote Desktop access, and authentication troubleshooting.

> **Lab note:** This environment was created for hands-on practice. Certain settings, such as disabling Windows Firewall on the Domain Controller, were used only for controlled lab testing and are not intended as production security practices.

---

## Technologies Used

- Microsoft Azure
- Windows Server 2022
- Windows 10
- Active Directory Domain Services (AD DS)
- Active Directory Users and Computers (ADUC)
- Group Policy Management
- PowerShell / PowerShell ISE
- DNS
- Remote Desktop Protocol (RDP)
- Windows Event Viewer

---

## Lab Environment

| System | Operating System | Purpose |
|---|---|---|
| **DC-1** | Windows Server 2022 | Domain Controller, Active Directory, DNS |
| **Client-1** | Windows 10 | Domain-joined client workstation |

**Domain:** `mydomain.com`

---

# Part 1 — Azure Environment Setup

## 1. Create the Domain Controller VM

I created a Windows Server 2022 virtual machine named **DC-1** in Microsoft Azure.

![DC-1 virtual machine](screenshots/01-dc1-vm-created.png)

## 2. Configure a Static Private IP for DC-1

I changed DC-1's private IP allocation from dynamic to static so the Domain Controller would maintain a consistent address.

![DC-1 static private IP](screenshots/02-dc1-static-private-ip.png)

## 3. Disable Windows Firewall for Lab Testing

For connectivity testing inside the isolated lab environment, I temporarily disabled Windows Firewall on DC-1.

![Windows Firewall disabled](screenshots/03-dc1-firewall-disabled.png)

## 4. Create Client-1

I created a Windows 10 virtual machine named **Client-1** and placed it on the same Azure virtual network as DC-1.

![Client-1 virtual machine](screenshots/04-client1-vm-created.png)

## 5. Configure Client-1 DNS

I configured Client-1 to use DC-1's private IP address as its DNS server.

![Client-1 DNS settings](screenshots/05-client1-dns-settings.png)

## 6. Test Connectivity Between Client-1 and DC-1

From Client-1, I successfully pinged the private IP address of DC-1.

![Ping DC-1](screenshots/06-client1-ping-dc1.png)

## 7. Verify DNS Configuration

I ran `ipconfig /all` on Client-1 and confirmed that its DNS server was set to DC-1.

![ipconfig all](screenshots/07-client1-ipconfig-all.png)

---

# Part 2 — Active Directory Deployment and Administration

## 8. Install Active Directory Domain Services

I installed the **Active Directory Domain Services (AD DS)** role on DC-1 and promoted the server to a Domain Controller for the `mydomain.com` domain.

![Install Active Directory Domain Services](screenshots/08-install-ad-ds.png)

## 9. Create Organizational Units

Using Active Directory Users and Computers, I created organizational units to organize users and computers, including `_EMPLOYEES`, `_ADMINS`, and `_CLIENTS`.

![Active Directory organizational units](screenshots/09-aduc-organizational-units.png)

## 10. Create a Domain Administrator Account

I created **Jane Doe** with the username `jane_admin` inside the `_ADMINS` OU.

![Create Jane admin](screenshots/10-create-jane-admin.png)

I then added `jane_admin` to the **Domain Admins** security group.

![Jane admin Domain Admins membership](screenshots/11-jane-admin-domain-admins.png)

## 11. Join Client-1 to the Domain

I joined Client-1 to the `mydomain.com` Active Directory domain.

![Client-1 domain join](screenshots/12-client1-domain-join.png)

## 12. Verify Client-1 in Active Directory

I verified that Client-1 appeared as a computer object in Active Directory and organized it within the client OU structure.

![Client-1 in Active Directory](screenshots/13-client1-aduc-verification.png)

## 13. Allow Remote Desktop Access for Domain Users

I configured Client-1 to allow Remote Desktop access for normal domain users.

![Remote Desktop domain users](screenshots/14-remote-desktop-domain-users.png)

## 14. Automate User Creation with PowerShell

I used PowerShell ISE to run a script that generated multiple Active Directory user accounts.

![PowerShell user creation](screenshots/15-powershell-create-users.png)

I verified that the generated users were created in Active Directory.

![Generated users in ADUC](screenshots/16-generated-users-aduc.png)

## 15. Test a Generated Domain User

I selected one of the newly generated user accounts and tested domain authentication on Client-1.

![Generated user selected](screenshots/17-generated-user-selected.png)

![Generated domain user login](screenshots/18-generated-user-login.png)

---

# Part 3 — Account Management, Group Policy, and Event Logs

## 16. Test Failed Login Attempts

I intentionally entered an incorrect password for a domain user to test account lockout behavior.

![Failed login attempt](screenshots/19-failed-login-attempt.png)

## 17. Configure an Account Lockout Policy

Using Group Policy, I configured the domain to lock an account after **5 failed login attempts**.

![Account lockout policy](screenshots/20-account-lockout-policy.png)

![Account lockout policy summary](screenshots/21-account-lockout-policy-summary.png)

## 18. Trigger an Account Lockout

I entered the wrong password enough times to trigger the configured account lockout policy.

![Account lockout triggered](screenshots/22-account-lockout-triggered.png)

## 19. Verify and Unlock the Account

I verified that the account was locked within Active Directory.

![Locked account in ADUC](screenshots/23-account-locked-aduc.png)

I then unlocked the account.

![Unlock account](screenshots/24-unlock-account.png)

## 20. Reset the User Password

I reset the user's password in Active Directory.

![Reset password](screenshots/25-reset-password.png)

I then verified that the user could successfully authenticate again.

![Successful login after unlock](screenshots/26-successful-login-after-unlock.png)

## 21. Disable and Re-enable a User Account

I disabled the same domain account in Active Directory.

![Disable account](screenshots/27-disable-account.png)

When I attempted to sign into Client-1 with the disabled account, Windows denied the login.

![Disabled account login error](screenshots/28-disabled-account-login-error.png)

I re-enabled the account and restored access.

![Re-enable account](screenshots/29-reenable-account.png)

## 22. Review Windows Event Logs

Finally, I used Windows Event Viewer to inspect security and authentication-related events generated during the lab.

![Windows Event Viewer security logs](screenshots/30-event-viewer-security-logs.png)

---

# Skills Demonstrated

- Deploying Windows Server and Windows client VMs in Microsoft Azure
- Configuring Azure virtual networking and private IP addressing
- Configuring DNS for an Active Directory environment
- Installing Active Directory Domain Services
- Promoting Windows Server to a Domain Controller
- Creating and organizing Active Directory users and OUs
- Managing security group membership
- Joining Windows computers to an Active Directory domain
- Configuring Remote Desktop access for domain users
- Automating Active Directory user creation with PowerShell
- Configuring account lockout policies with Group Policy
- Unlocking, disabling, re-enabling, and resetting user accounts
- Troubleshooting authentication and domain login issues
- Reviewing Windows security and authentication logs

---

## Key Takeaway

This lab strengthened my understanding of how Active Directory is deployed and managed in a Windows enterprise environment. It provided practical experience with user and computer administration, authentication, Group Policy, PowerShell automation, and troubleshooting tasks commonly performed in IT support and system administration roles.
