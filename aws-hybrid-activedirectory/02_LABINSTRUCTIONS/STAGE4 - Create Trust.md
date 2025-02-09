# Advanced Hybrid Directory Demo

In this Stage of the Demo lesson you will be creating a two-way forest TRUST between the Simulated On-Premises environment and AWS  
This will allow you to login to one, using ID's from the other  
It will allow you to grant access to the resources of one, from the other.  

# STAGE 4A - ONPREM Ensure That Kerberos Pre-authentication Is Enabled

Connect to the JumpBox  (THE SIMULATED ONPREM ONE, NOT AWS)
Connect to the Client Instance  
Load Active Directory Users and Computers  
Select Users 
Click Admin  
Right click , properties  
Accounts tab  
Make sure `Do not require Kerberos preauthentication` is UNCHECKED  

# STAGE 4B - ONPREM Configure DNS Conditional Forwarders for ONPREM  

Open https://console.aws.amazon.com/directoryservicev2/home?region=us-east-1#!/directories  
Select the managed microsoft AD  
Note down the `DNS Addresses` (2 IPs)  
Note down `Directory DNS Name`  

On the ONPREM Jumpbox  
Click windows button  
Type DNS  
Open the DNS Management App  
Select `The following Computer`  
Enter `DC1`  
Connect  
Select `Conditional Forwarders`  
Click `Action` then `new Conditional Forwarder`  
in DNS Domain enter the Directory DNS Name you noted down   
(it should be `aws.animals4life.org`)  
Enter the IP addresses of the Directory (DNS Addresses you noted down above)  

Click `Store this conditional forwarder in Active Directory`  
Change the dropdown to `All DNS Servers in this domain`  
Click `OK`  


# STAGE 4C - AWS - MicrosoftAD Security Group

Open https://console.aws.amazon.com/vpc/home?region=us-east-1#SecurityGroups:  
Select the SG with the description below  
`AWS created security group for directoryID directory controllers`  
Select `Outbound`  
Click `Edit Outbound Rules`   
Click `Add Rule`  
Type `All Traffic`  
Destination `0.0.0.0/0`  
Click `Save`  


# STAGE 4D - AWS - Ensure That Kerberos Pre-authentication Is Enabled  

Connect to the JumpBox-AWS  
Load Active Directory Users and Computers  
Select A4L-AWS  
Select Users within A4L-AWS  
Click Admin  
Right click , properties  
Accounts tab  
Make sure `Do not require Kerberos preauthentication` is UNCHECKED  

# STAGE 4E - Configure the Trust in Your On-Premises Active Directory

Connect to the JumpBox  
Connect to the Client Instance  
open `Active Directory Domains and Trusts.`  
Right click `ad.animals4life.org` and click `properties`  
Click `Trusts`  
Click `New Trust`  
Click `Next`  
Enter `aws.animals4life.org`  
Click `Next`  
Select `Forest trust`  
Click `Next`  
Select `Two-way` click `Next`  
Click `This domain only` click `next`  
Select `Forest-wide authentication` Click next  
Enter a password for the trust  
Confirm that password  
(Note down that TRUST password you will need it again in a second)  
Click `Next`  
Click `Next`  
Choose No, do not confirm the outgoing trust. Choose Next.  
Choose No, do not confirm the incoming trust. Choose Next.  
Click `Finish`  


# STAGE 4F - Configure the Trust in Your AWS Managed Microsoft AD Directory

Go to https://console.aws.amazon.com/directoryservicev2/home?region=us-east-1#!/directories  
Click your directory  
Click `networking & Security` Tab  
Click `Add Trust Relationship`  
in `Exiting or new remote domain` type `ad.animals4life.org`  
Select `Forest Trust`  
Enter the `Trust Password`  
Select `Two Way`  
Under `Conditional Forwarder` enter the IP address of DC1   
(it should be `192.168.10.100`)  
Click `Add anothr Ip address`  
Enter the IP for DC2  
(it should be `192.168.11.100`)  
Click `Add`  

Wait for this to create it can take some time  
Make sure it verifies as OK.  

# STAGE 4G - TEST THE TRUST  

On the `JUMPBOX-AWS`  
Open Active Directory Users and Computers  
Locate `AWS Delegated Groups` in `aws.animals4life.org`  
Locate and open `AWS Delegated Administrators`  
Click `Members`  
Click `Add`  
Click `Locations`  
Select `ad.animals4life.org` (on prem domain)  
Click `OK`  
Type `Admin`  
Click `Check Names`  
Enter `admin@ad.animals4life.org` and `YOUR_DOMAIN_ADMIN_PASSWORD` for enter network credentials
Make sure the line with Logon Name `Admin` is selected  
Click `OK`, `OK`, `OK`   
The On prem admin user is now an Admin of the AWS Directory  
This is using the trust  

Logoff the JumpBox-AWS  
Login as `admin@ad.animals4life.org`  

Works!!  

# FINISH  
Once you have connected ... you can finish this part of the DEMO  

