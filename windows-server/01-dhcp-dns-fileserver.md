Lab 1 — DHCP, DNS and File Server Configuration

Date: June 2026

Platform: Windows Server 2022 Standard Evaluation, UTM on macOS (M3)


Objective

Install and configure three core Windows Server roles: DHCP Server, DNS Server, and File Server.


Environment


Host: MacBook Air M3, UTM virtualisation (x64 emulation)
Guest OS: Windows Server 2022 Standard Evaluation
Network: NAT via UTM



Part 1 — DHCP Server

Overview

DHCP automatically assigns IP addresses to devices joining a network, eliminating the need for manual configuration on each device.

Configuration


Installed DHCP Server role via Server Manager
Completed post-install configuration from the flag notification
Created a new scope with the following settings:


SettingValueScope NameMyFirstScopeStart IP192.168.1.100End IP192.168.1.200Subnet Mask255.255.255.0StatusActive

Screenshot

Show Image

Outcome

DHCP server operational. Devices joining the network will automatically receive an address from the configured pool.


Part 2 — DNS Server

Overview

DNS resolves hostnames to IP addresses. In this lab, an internal DNS zone was created for the company domain, and an A record was added and verified.

Configuration


Installed DNS Server role via Server Manager
Created a Forward Lookup Zone: mycompany.local
Added an A record:


SettingValueNameserver1TypeHost (A)IP Address192.168.1.10

Troubleshooting

Initial nslookup queries failed because the network adapter was configured to use an external IPv6 DNS server by default. Fixed by setting the preferred DNS server to 127.0.0.1 in the adapter properties, pointing the server at itself.

Verification

nslookup server1.mycompany.local 127.0.0.1

Result:

Server:  localhost
Address: 127.0.0.1

Name:    server1.mycompany.local
Address: 192.168.1.10

Screenshots

Show Image

Show Image

Outcome

DNS server correctly resolving internal hostnames.


Part 3 — File Server

Overview

A file server hosts shared folders accessible to users across the network via UNC paths.

Configuration


Installed File Server role via Server Manager under File and Storage Services
Created folder: C:\CompanyFiles
Enabled sharing via folder Properties > Sharing > Advanced Sharing


Verification

Accessed the share using Run (Win+R):

\\127.0.0.1\CompanyFiles

Folder opened successfully over the network.

Outcome

Shared folder accessible via network path, confirming File Server role is functioning.


What I Learned

Configuring DHCP and DNS on an actual server made the concepts from networking theory much more concrete. The troubleshooting required for DNS — identifying that IPv6 was taking priority and overriding the local server — was a practical lesson that wouldn't come from reading alone.

Challenges

IssueResolutionx64 ISO boot looping on M3 MacSwitched to UTM emulation modenslookup returning non-existent domainSet preferred DNS to 127.0.0.1 in adapter settings
