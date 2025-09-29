<p align="center">
  <img src="https://github.com/jorge-nbb/images/raw/main/PowerShell_5.0_icon.png" alt="powershell logo" width="228"/>
</p>

<h1 align="center">System Information Script</h1>

---

## 🎯 Project Purpose

- Collect detailed system information using PowerShell  
- Output results into **Text**, **CSV**, and **HTML** formats  
- Demonstrate PowerShell automation for basic system auditing  

---

## 🛠️ Tools & Services Used

- **Windows 10 / 11 (Client-PC)**
- **PowerShell 5.1+** (or PowerShell 7)
- **Notepad** (to create/edit script)

---

## ⚙️ Configuration Steps

### STEP 1: Open PowerShell ISE

<table>
  <tr>
    <td style="vertical-align: top; width: 60%;">

1. Log into **Client-PC**  
2. Press **Start** > Search for **Windows PowerShell ISE**  
3. Right-click > **Run as administrator**  
4. Click **Yes** on the UAC prompt  

</td>
<td style="vertical-align: top; text-align: right; width: 40%;">
  <img src="https://github.com/jorge-nbb/images-sytem-information/blob/main/p1.png" alt="p1" width="350"/>
</td>
  </tr>
</table>

---

### STEP 2: Create the PowerShell Script

<table>
  <tr>
    <td style="vertical-align: top; width: 60%;">

1. In PowerShell ISE, click **File > New**  
2. Copy and paste the script below:

```powershell
# Collect-SystemInfo.ps1
# Collects system information and saves in multiple formats

# Gather system details
$ip = Get-NetIPAddress -AddressFamily IPv4 | Where-Object { $_.IPAddress -ne "127.0.0.1" } | Select-Object -First 1 -ExpandProperty IPAddress

$sysInfo = [PSCustomObject]@{
    'ComputerName' = $env:COMPUTERNAME
    'OS Version'   = (Get-CimInstance Win32_OperatingSystem).Caption
    'CPU'          = (Get-CimInstance Win32_Processor).Name
    'RAM (GB)'     = [math]::Round((Get-CimInstance Win32_PhysicalMemory | Measure-Object -Property Capacity -Sum).Sum / 1GB, 2)
    'Disk Usage'   = (Get-PSDrive C).Used / 1GB -as [int]
    'Disk Free'    = (Get-PSDrive C).Free / 1GB -as [int]
    'IP Address'   = $ip
}

# Output folder
$OutputPath = "$env:USERPROFILE\Desktop\SystemReport"
New-Item -ItemType Directory -Force -Path $OutputPath | Out-Null

# Export formats
$sysInfo | Out-File "$OutputPath\SystemReport.txt"
$sysInfo | Export-Csv "$OutputPath\SystemReport.csv" -NoTypeInformation
$sysInfo | ConvertTo-Html | Out-File "$OutputPath\SystemReport.html"

Write-Host "System report saved to $OutputPath"
```

3. Save as: `Collect-SystemInfo.ps1` on desktop

</table>

---

### STEP 3: Run the Script

<table>
  <tr>
    <td style="vertical-align: top; width: 60%;">

1. Open PowerShell as administrator
2. Allow script execution (if restricted):
   - Temporarily (only for this session):
     ```
     Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
     ```
   - Permanently (optional, allows local scripts anytime):
     ```
     Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
     ```
3. Navigate to Desktop:
```
cd $env:USERPROFILE\Desktop
```
4. Run the script:
```
.\Collect-SystemInfo.ps1
```
5. Press **Enter** and wait for the script to complete. Output files will appear in the SystemReport folder in Desktop

</td>
<td style="vertical-align: top; text-align: right; width: 40%;">
  <img src="https://github.com/jorge-nbb/images-sytem-information/blob/main/p3.png" alt="p3" width="500"/>
   <hr>
  <img src="https://github.com/jorge-nbb/images-sytem-information/blob/main/p4.png" alt="p4" width="500"/>
</td>
  </tr>
</table>

---

### STEP 4: Verify Output

<table>
  <tr>
    <td style="vertical-align: top; width: 60%;">

1. Open File Explorer > Desktop > SystemReport folder
2. Confirm the following files exist:
   - SystemReport.txt
   - SystemReport.csv
   - SystemReport.html
3. Double-click each file to view results

</td>
<td style="vertical-align: top; text-align: right; width: 40%;">
  <img src="https://github.com/jorge-nbb/images-sytem-information/blob/main/p5.png" alt="p5" width="350"/>
</td>
  </tr>
</table>

---

## 📝 Results

- System information automatically collected via PowerShell  
- Reports generated in **TXT, CSV, and HTML** formats  
- OS, CPU, RAM, disk, and IP details captured

---
