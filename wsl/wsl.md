# Windows Subsystem for Linux (WSL)

## Installing and Update WSL

To check WSL version

```powershell
wsl.exe --version
```

To update WSL, you can use the following command:

```powershell
wsl.exe --update
```
    

## Managing Distro

---
**References**
---

### Install, register, and unregister distributions

```powershell

To list registered distributions:

```powershell   
wsl.exe --list
```

To list available distributions to install

```powershell   
wsl.exe --list --online
```

To install a distribution

```powershell
wsl.exe --install -d <distribution-name>
```

To unregister a distribution

```powershell
wsl.exe --unregister <distribution-name>
```

To set a default distribution

```powershell
wsl.exe --set-default <distribution-name>
```









## Managing VDHX (Virtual Disk Hard Disk)

---
**References**

[How to manage WSL disk space](https://learn.microsoft.com/en-us/windows/wsl/disk-space)
[How to locate the .vhdx file and disk path for your Linux distribution]https://learn.microsoft.com/en-us/windows/wsl/disk-space#how-to-locate-the-vhdx-file-and-disk-path-for-your-linux-distribution
---


### What is VDHX?

VDHX is a virtual hard disk (VHD) file that stores the filesystem of a WSL distribution. It is used to manage the storage space allocated to the distribution.


To locate the .vhdx file and disk path for your Linux distribution (replace the *<distribution-name>* with the name of your distribution):

```powershell
(Get-ChildItem -Path HKCU:\Software\Microsoft\Windows\CurrentVersion\Lxss | Where-Object { $_.GetValue("DistributionName") -eq '<distribution-name>' }).GetValue("BasePath") + "\ext4.vhdx"
```



### Check VDHX information

To check which VDHX is used by a distribution, you can use the following command:


To check the disk usage of a distribution, you can use the following command:

```powershell	
wsl.exe --system -d <distribution-name> df -h /
```


### Expand the size using DISKPART

To expand the size of a VHDX file, you can use the following steps:

1. Open a PowerShell window as an administrator.
2. Run the following command to open the Diskpart utility:

```powershell
wsl.exe --shutdown

diskpart
Select vdisk file="<pathToVHD>"
detail vdisk
expand vdisk maximum=<sizeInMegaBytes>
exit
```
