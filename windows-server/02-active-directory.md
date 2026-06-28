Lab 2 — Active Directory, Domain Controller, Users and Groups

Date: June 2026

Platform: Windows Server 2022 Standard Evaluation, UTM on macOS (M3)


Objective

Install Active Directory Domain Services, promote the server to a Domain Controller, and configure Organisational Units, user accounts, and security groups.


Key Concepts

Domain vs Workgroup

A workgroup is a peer-to-peer arrangement where each computer manages its own accounts independently. A domain places all computers and users under centralised control via a Domain Controller. Every enterprise environment uses a domain.

Domain Controller

The server running Active Directory. All authentication requests on the network pass through it. It verifies credentials, applies policies, and controls access to resources.

Organisational Unit (OU)

A container within Active Directory used to organise users and computers logically, typically by department. Policies and permissions can be applied at the OU level.


Steps

1. Install AD DS Role

Installed Active Directory Domain Services via Server Manager. Added required features when prompted.

2. Promote to Domain Controller


Clicked the flag notification in Server Manager
Selected "Promote this server to a domain controller"
Selected "Add a new forest"
Set root domain name: mycompany.local
Configured DSRM password
Completed wizard and allowed the server to restart


After restart, the login screen displayed MYCOMPANY\Administrator, confirming Active Directory is active.

3. Create Organisational Unit


Opened Tools > Active Directory Users and Computers
Right clicked mycompany.local > New > Organizational Unit
Named it: IT Department


4. Create User Account

Right clicked IT Department > New > User and configured:

FieldValueFirst NameJohnLast NameSmithLogon NamejsmithPassword Never ExpiresYes

5. Create Security Group

Right clicked IT Department > New > Group and configured:

FieldValueGroup NameIT StaffScopeGlobalTypeSecurity

6. Add User to Group

Opened IT Staff group > Members tab > Add > searched for jsmith > confirmed and saved.

Screenshot

Show Image


Outcome

Domain controller operational with domain mycompany.local. IT Department OU contains user jsmith and security group IT Staff. John Smith is a member of IT Staff.


What I Learned

The difference between a workgroup and a domain becomes very clear when you actually build one. Setting up the Domain Controller and seeing the login screen change to show the domain name made the concept real. Organisational Units make large-scale user management manageable — without them, administering hundreds of accounts in a flat structure would be unworkable.

Challenges

IssueResolutionServer restarted mid-promotionExpected behaviour — AD DS promotion requires a restart
