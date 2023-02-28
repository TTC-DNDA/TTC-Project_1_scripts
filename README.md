# Trek Tech Onboarding Automation Script

Deontae Carter
Dylan Dempsey
Nicholas Loaicono
Marco Aliaga

# Main

# Enable File and Printer Sharing

Set-NetFirewallRule -DisplayGroup "File And Printer Sharing" -Enabled True

# Enable ICMP 

netsh advfirewall firewall add rule name="Allow incoming ping requests IPv4" dir=in action=allow protocol=icmpv4 

# Remote Management

# RDP

reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f

# NLA 

Set-ItemProperty ‘HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\‘ -Name “UserAuthentication” -Value 1

# Firewall Rule

Enable-NetFirewallRule -DisplayGroup “Remote Desktop”

# Bloatware Remover

iex ((New-Object System.Net.WebClient).DownloadString('https://git.io/debloat'))

# Remote Event Viewer

Set-NetFirewallRule -DisplayGroup 'Remote Event Log Management' -Enabled True -PassThru

# Update computers remotely

Invoke-WuJob -ComputerName $Computers -Script { ipmo PSWindowsUpdate; Install-WindowsUpdate -AcceptAll -IgnoreReboot | Out-File "C:\Windows\PSWindowsUpdate.log"} -RunNow -Confirm:$false -Verbose -ErrorAction Ignore

# Enable Hyper-V

Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All

# Enable Script Execution

powershell.exe Set-ExecutionPolicy Bypass -Force

# End
