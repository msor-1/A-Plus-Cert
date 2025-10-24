# Powershell Scripting
## My Code
```powershell
# Define output file path
$outputFile = "$env:USERPROFILE\Desktop\SystemInfo.txt"

# Collect system information
$info = @()
$info += "== System Information Script Assignment =="
$info += "OS Version:"
$info += (Get-CimInstance Win32_OperatingSystem | Select-Object Caption, Version | Format-Table -AutoSize | Out-String)
$info += "Computer Name:"
$info += $env:COMPUTERNAME
$info += "CPU Information:"
$info += (Get-CimInstance Win32_Processor | Select-Object Name, NumberOfCores, NumberOfLogicalProcessors | Format-Table -AutoSize | Out-String)
$info += "Installed RAM:"
$info += ("{0:N2} GB" -f ((Get-CimInstance Win32_ComputerSystem).TotalPhysicalMemory / 1GB))
$info += ""
$info += "IP Configuration:"
$info += (Get-NetIPAddress | Where-Object {$_.AddressFamily -eq 'IPv4' -and $_.IPAddress -ne '127.0.0.1'} | 
           Select-Object InterfaceAlias, IPAddress | Format-Table -AutoSize | Out-String)
$info += "=============================="

# Display on screen
$info | Out-String

# Export to Desktop
$outputFile = "$env:USERPROFILE\Documents\SystemInfo.txt"
$info | Out-File -FilePath $outputFile -Encoding UTF8

Write-Host "System information exported to: $outputFile"


```
