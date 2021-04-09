Manage New Relic infrastructure agent via PowerShell
=================================================================

New Relic has great docs: https://docs.newrelic.com/docs/infrastructure/install-infrastructure-agent/windows-installation/install-infrastructure-monitoring-agent-windows/

You should read those. For the lazy, here is some basic PS code to do common sysadmin tasks related to New Relic agent. Full disclaimer: I'm not really a windows person so these are likely rather crude.


Remote Install
---------------------------------
Replace <YOUR_LICENSE_KEY>, run PS as a user with admin rights on the remote box.

```
Invoke-Command -ComputerName server1, server2 -ScriptBlock {
(New-Object System.Net.WebClient).DownloadFile("http://download.newrelic.com/infrastructure_agent/windows/newrelic-infra.msi", "$env:TEMP\newrelic-infra.msi"); ` msiexec.exe /qn /i "$env:TEMP\newrelic-infra.msi" GENERATE_CONFIG=true LICENSE_KEY="<YOUR_LICENSE_KEY>" | Out-Null; ` net start newrelic-infra}
```


Update Agent
-------------------------------
```
Stop-Service -Name "newrelic-infra"
(New-Object System.Net.WebClient).DownloadFile("https://download.newrelic.com/infrastructure_agent/windows/newrelic-infra.msi", "$env:TEMP\newrelic-infra.msi"); `
msiexec.exe /qn /i "$env:TEMP\newrelic-infra.msi"
Start-Service -Name "newrelic-infra"
```

Check status of agent
-------------------------------
```
(Get-Service newrelic-infra).Status
```

Check version of agent
-------------------------------
```
Get-WmiObject -Class Win32_Product | where vendor -eq "New Relic, Inc."
```

Start/stop agent
-------------------------------
```
Stop-Service -Name "newrelic-infra"
Start-Service -Name "newrelic-infra"
```
