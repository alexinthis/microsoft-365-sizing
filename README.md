# Microsoft 365 Sizing PowerShell Script

[![Download zip](https://user-images.githubusercontent.com/8610203/145614905-a6d64f3a-adab-4c3f-9bf9-ffa4fdf6793f.png "Download zip")](https://github.com/rubrikinc/microsoft-365-sizing/archive/refs/heads/main.zip)


```
PS ~/Development/microsoft-365-sizing> ./Get-RubrikM365SizingInfo.ps1
[INFO] Starting the Rubrik Microsoft 365 sizing script (v3.3).
[INFO] Connecting to the Microsoft Graph API using 'Reports.Read.All' permissions.
[INFO] Retrieving the Total Storage Consumed for ...
 - Exchange
 - SharePoint
 - OneDrive
[INFO] Retrieving the Average Storage Growth Forecast for ...
 - Exchange
 - SharePoint
 - OneDrive                                                                            
[INFO] Retrieving the subscription License details.                                                                       
[INFO] Disconnecting from the Microsoft Graph API.                                                                        
[INFO] Connecting to the Microsoft Exchange Online Module.                                                                
[INFO] Retrieving all In-Place Archive Exchange Mailbox sizing information.
[INFO] Retrieving Exchange Mailbox Shared Mailbox information.                                                                                                                                                        
[INFO] Disconnecting from the Microsoft Exchange Online Module
[INFO] Calculating the forecasted total storage need for Rubrik.     

M365 Sizing information has been written to ~/Development/microsoft-365-sizing/Rubrik-MS365-Sizing.html   
```

## Requirements

* `PowerShell >= 5.1` for PowerShell Gallery.
* Microsoft 365 Global administrator credentials (Required to determine In-Place Archive and Shared Mailbox details)



## Installation

1. Download the [Get-RubrikM365SizingInfo.ps1](https://github.com/rubrikinc/microsoft-365-sizing/archive/refs/heads/main.zip) PowerShell script to your local machine
2. Install the `Microsoft.Graph.Reports` and `ExchangeOnlineManagement` modules from the PowerShell Gallery

```powershell
Install-Module Microsoft.Graph.Reports, Microsoft.Graph.Groups, ExchangeOnlineManagement
```

## Usage

1. Open a PowerShell terminal and navigate to the folder/directory where you previously downloaded the [Get-RubrikM365SizingInfo.ps1](https://github.com/rubrikinc/microsoft-365-sizing/blob/main/Get-RubrikM365SizingInfo.ps1) file.

2. Run the script.

```
./Get-RubrikM365SizingInfo.ps1
```

> NOTE - If you receive a PowerShell execution policy error message you can run the following command:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

3. Authenticate and acknowledge report access permissions in the browser window/tab that appears. This will occur twice during the script execution.

> Note: There is a known issue with the Microsoft authentication process that may result in an error message during the initial authentication process. If this occurs, re-run the script and the error will no longer show.

4. The script will run and the results will be written to a html file in the directory in which it was run.

```
.\RubrikMS365Sizing.txt
```

When 5,000 or more Shared Mailboxes or In-Place archives are detected, you will receive the following prompt:

```
[ACTION REQUIRED] In order to periodically refresh the connection to Microsoft, we need the User Principal Name used during the authentication process."
Enter the User Principal Name: 
```

The "User Principal Name" corresponds with the account name you used to sign into Microsoft 365 during the Modern Authentication process.

To filter OneDrive and Exchange results to a specific subset of users in an AzureAdGroup use the `-AzureAdGroupName` flag.

```
./Get-RubrikM365SizingInfo.ps1 -AzureAdGroupName "RubrikEmployees"
```

## What information does the script access?

The majority of the information collected is directly from the Microsoft 365 [Usage reports](https://docs.microsoft.com/en-us/microsoft-365/admin/activity-reports/activity-reports?view=o365-worldwide) that are found in the admin center.
The benefit of this approach is that the information can be pulled in bulk and does not require a complete crawl of your Microsoft 365 subscription. 

The only downside of this approach is that the Usage reports do not contain any Shared Mailbox or In-Place archive information. To gather that information Rubrik will request a seperate set of permissions to pull statistics for each Shared Mailbox and In-Place archive in your environment. Depending on the size of you environment, this can take a significant amount of time.  



## Example Output

![image](https://user-images.githubusercontent.com/51362633/188180109-74f01191-273e-447b-8824-936ca17f1b1a.png)



