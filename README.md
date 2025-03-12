# Group-Policy
<p align="center">
<img src="https://github.com/user-attachments/assets/bb5a9b28-027f-4e5a-bd9d-9739c95cd5ba" height="30%" width="30%"/>
</p>

<h1>Adding Users to the Domain and configuring Group Policies.</h1>
In this tutorial, we will add users(employees) to our domain and configure Group Policies for access.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Virtual Network inside Azure Resource Group
- RDP

<h2>Operating Systems Used </h2>

- Windows 10 Pro (22H2)-Client 1
- Windows Server- DC-1(Domain Controller)

<h2>Actions and Observations</h2>

<p>
<img src="https://github.com/user-attachments/assets/70f2f67d-7876-4788-9802-a6119c9efd16" height="50%" width="50%"/>

We will use the Resource group that was setup in [Creating Active Directory](https://github.com/gustygreen/Active-Directory.git) and the Active Directory Configuration Lab [Configuring On-premises Active Directory for users](https://github.com/gustygreen/configure-ad) for this lab.
</p>

<h2>Setup Remote Desktop for non-admin users on client-1.</h2>

<p>
<img src="https://github.com/user-attachments/assets/3f206e9b-d4d8-46d5-bf0c-deabba02b550" height="50%" width="50%"/>

RDP into client-1 as Admin. Go to System-Remote Desktop-Click "select users that can remotely access this PC-Click Add-Enter "domain users"-Click Check Names-Click OK.
</p>

<p>
<img src="https://github.com/user-attachments/assets/7cef9487-8b88-41a3-81f6-908b857d0e2d" height="50%" width="50%"/>

The Domain Users are now added. Click OK.
</p>

<h2>Create and add users to Active Directory</h2>

<p>
<img src="https://github.com/user-attachments/assets/bea20e09-7af7-49c0-bc61-34741068cc60" height="50%" width="50%"/>

RDP into dc-1 as Admin. Run PowerShell ISE as an Admin. This will allow the use of a script to create the users and place them in the OU _EMPLOYEES.
Select Run button to implement the script.
</p>

<p>
<img src="https://github.com/user-attachments/assets/05463a06-b981-4fe7-affe-9728870eef0c" height="50%" width="50%"/>

Users added to the _EMPLOYEES OU can be seen by going into Active Directory Users and Computers and selecting _EMPLOYEES.
</p>

<p>
<img src="https://github.com/user-attachments/assets/51ced026-9a33-4258-8c1d-21c91deeac3d" height="50%" width="50%"/>

These users can now RDP to client-1 using the password setup when the script ran.
</p>

<h2>Group Policies</h2>
<p>Setup a Lockout Policy
</p>

<p>
<img src="https://github.com/user-attachments/assets/c2ac7c8e-32af-4331-87b4-cd845da88466" height="50%" width="50%"/>

Go to Group Policy Management(gpmc.msc). Select mydomain.com-Default Domain Policy. Right click-Edit
</p>

<p>
<img src="https://github.com/user-attachments/assets/c2ac7c8e-32af-4331-87b4-cd845da88466" height="50%" width="50%"/>

To setup a lockout policy go to Group Policy Management(gpmc.msc). Select mydomain.com-Default Domain Policy. Right click-Edit.
</p>

<p>
<img src="https://github.com/user-attachments/assets/126fec93-5dcf-4d7c-93ca-8bb31e92a707" height="50%" width="50%"/>

Go to Computer Configuration(1)-Policies(2)-Windows Settings(3)-Security Settings(4)-Account Lockout Policy(5). Select Account lockout threshold(6). Change to number of attempts(7). Click OK(8).
</p>

<p>
<img src="https://github.com/user-attachments/assets/7081fbb6-8a45-4d6d-b4c0-6d3e3b5d4b08" height="50%" width="50%"/>

This will automatically adjust the policy settings. This policy will refresh to the new rule in 90 minutes or it can be forced by the Admin using "gpupdate /force" at the Command Prompt.
</p>

<p>
<img src="https://github.com/user-attachments/assets/9ea2a72e-22c3-4962-aa36-c82a5d1b7960" height="50%" width="50%"/>

Now if the number of attempts exceeds the threshold then the user is locked out and notified.
</p>

<p>
<img src="https://github.com/user-attachments/assets/20c9df62-e0d2-4382-9839-11ea888e100e" height="50%" width="50%"/>

The user can wait the prescribed time frame and their logon will be automatically unlocked or they can create a ticket and have IT unlock the account with the above field in Active Directory.
</p>

<h2>Observing Logs</h2>

Event Viewer can be used to see what events have taken place-Logon, Logoff, Success, Failures.

<p>
<img src="https://github.com/user-attachments/assets/85cad7b8-2927-4066-a8a8-1b5bc2e40281" height="50%" width="50%"/>

Logon to client-1 as Admin. Go to Event Viewer. Look at the Security logs. Here you can see the failed attempt to logon.
</p>
