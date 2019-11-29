# Migrate_OS_Settings_To_DS


AUTHOR		: Yanni Kashoqa

TITLE		: OfficeScan to Deep Security Scan Configuration Migration Tool

VERSION		: 1.3

DESCRIPTION	: This PowerShell script will migrate OfficeScan scan settings and exclusions to Deep Security

DISCLAIMER	: Please feel free to make any changes or modifications as seen fit.

## FEATURES
- Extract Exclusions and Scan Settings From OfficeScan Clients Registries
- Create Exclusions containers in Deep Security
- Create Scan Configurations in Deep Security
- Create Policies in Deep Security.  The policy names will match the OfficeScan domain of the migrated system.
- The script does not import some of the new advanced new features of OfficeScan XG or Apex One
- The script does not import Web Reputation settings
- SOAP APIs do not support CVE Exploit actions. The Recommended Quarantine Action will be applied.
- Rename Action will be converted to Quarantine since Deep Security does not support the Rename Action.
- Pass Action will be converted to Deny Access since Deep Security does not support the Rename Action.

## NOT CAPABLE
The following features/options are not migrated due to lack of SOAP API functionality.  They will need to modified manually on every Policy and Scan Configuration:
- Document Exploit Protection
- Predictive Machine Learning
- Behavior Monitoring
- Process Memory Scan
- Web Reputation
- Smart Protection Server source Configuration

## REQUIRMENTS
- PowerShell 3.0
- OfficeScan 11, XP, Apex One Agents
- SOAP Web Service API must be enabled in Deep Security console (Administration > System Settings > Advanced)
- Remote systems are online and pingable
- Remote Registry Service is running on remote systems
- Account used to execute the script need to be a local admin on remote systems
- Create a DS-Config.json in the same folder with the following content:

~~~~JSON
{
    "MANAGER": "app.deepsecurity.trendmicro.com",
    "PORT": "443",
    "TENANT": "",
    "USER_NAME": "",
    "PASSWORD": "",
    "BASEPOLICY" : "",
    "SOURCEFILE" : "Migrate_OS_Settings_To_DS_SourceList.txt",
    "Enable_Alerts"  : "True",
    "Use_Same_Exclusions" : "False",
    "Cookies_Action" :   "DELETE"
}
~~~~

## DS-Config.json Explanation:
- MANAGER: FQDN of local Deep security Manager or app.deepsecurity.trendmicro.com for DSaaS.
- PORT: 4119 for local or 443 for DSaaS.
- TENANT: DSaaS Tenant name.  No value if local.
- USER_NAME: a Deep Security Administrator User Name.  User for creating the policies and scan configurations.
- PASSWORD: Password of the Deep Security Administrator
- BASEPOLICY: The Base Policy name where you want the migrated policies placed under.
- SOURCEFILE: Name of the source file where all target OfficeScan Agents are listed
- Enable_Alerts: True to enable alerting of Antimalware detection. Otherwise use False. 
- Use_Same_Exclusions: True to use the Realtime Exclusiions for Manual and Scheduled Scaning.  Otherwise use False.
- Cookies_Action: Options are: DELETE, PASS

## USAGE
- Populate the source list file Migrate_OS_Settings_To_DS_SourceList.txt with the system names leaving the header intact.
     Each system should be from a seperate OfficeScan domain
- Modify the DS-Config.json to reflect the existing environment
- Open a Powershell console as an administrator
- Navigate to the script location and execute.
