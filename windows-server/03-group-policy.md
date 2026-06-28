Lab 3 — Group Policy Object (GPO) Configuration

Date: June 2026

Platform: Windows Server 2022 Standard Evaluation, UTM on macOS (M3)


Objective

Create a Group Policy Object that restricts access to Control Panel for users in the IT Department OU, and verify it is correctly configured and applied.


Key Concepts

Group Policy

Group Policy allows administrators to define and enforce configuration settings across users and computers on a domain from a single location. A change made on the server propagates automatically to all targeted machines — no manual configuration per device required.

Group Policy Object (GPO)

A GPO is the container that holds the actual policy rules. It has no effect until linked to an OU, domain, or site. Once linked, it applies to all users and computers within that container.


Steps

1. Open Group Policy Management

Opened Server Manager > Tools > Group Policy Management. Expanded Forest: mycompany.local > Domains > mycompany.local > IT Department.

2. Create and Link a GPO


Right clicked IT Department > "Create a GPO in this domain and Link it here"
Named it: Block Control Panel


The GPO is now linked to IT Department. All policy settings inside it will apply to users in that OU.

3. Configure the Policy


Right clicked Block Control Panel > Edit
Navigated to:


User Configuration > Policies > Administrative Templates > Control Panel


Opened "Prohibit access to Control Panel and PC settings"
Set to: Enabled
Clicked Apply and OK


4. Push Policy Update

Ran the following command in Command Prompt:

gpupdate /force

Output confirmed: update completed successfully.

5. Verify GPO Status

Checked the Settings tab on the Block Control Panel GPO:

FieldValueGPO StatusEnabledLinked ToIT DepartmentPathmycompany.local/IT DepartmentOwnerMYCOMPANY\Domain Admins

Screenshot

Show Image


Outcome

GPO created, linked, and confirmed enabled. The policy will apply to any standard user in the IT Department OU upon login to a domain-joined machine.


What I Learned

Group Policy is one of the most practical tools in Windows Server administration. The key insight is that a GPO is just a container — the actual restriction is the policy setting inside it. Understanding that the Administrator account is exempt from restrictive policies by design is also important: it prevents accidental lockout of the server.

The gpupdate /force command is something IT admins use constantly after policy changes, and understanding why it exists — because policies normally only refresh every 90 minutes — adds useful context.

Challenges

IssueResolutionControl Panel still opened during testingAdministrator is exempt from GPOs by design. Policy applies to standard users like jsmithgpresult showed no RSoP data for jsmithExpected — jsmith has never logged in. Data only exists after a user session is established

Next Steps

To fully validate this lab:


Join a Windows 10/11 client VM to mycompany.local
Log in as jsmith
Confirm Control Panel is blocked
Run gpresult /r to view applied policies
