---
title: Building an Active Directory Lab — Part 1A - AutomatedLab
date: 2019-12-07 +0800
categories: [blog, windows]
tags: [windows, active directory, virtualisation, powershell, automatedlab, hyper-v, homelab]
author: Rami
---

# Building an Active Directory Lab — Part 1A: AutomatedLab

So you’d like to build an Active Directory (AD) lab and have no idea how to get started. I learnt how to build labs manually however this was quite time consuming and didn’t allow much flexibility (which I’ll cover in a future article).

To setup an AD lab today we’ll be leveraging [AutomatedLab](https://github.com/AutomatedLab/AutomatedLab). AutomatedLab is a provisioning solution that lets you deploy simple and complex labs on HyperV and Azure with simple PowerShell scripts.

## Technical Requirements:

-   Windows Server 2012 R2+/Windows 8.1+
-   Administrator Privilege
-   Intel VT-x or AMD/V capable CPU
-   A reasonable amount of RAM (8GB+)
-   Hyper-V

The full list of requirements can be found on their [website](https://automatedlab.org/en/latest/).

## Skill Requirements:

-   Basic understanding of PowerShell
-   Basic understanding of Windows Environments
-   Basic understanding of Hyper-V

# Preperation

## Download ISOs

AutomatedLab requires you to have some ISOs it can use. It initially creates a base image.

1.  Navigate to [Windows Evaluation Centre](https://www.microsoft.com/en-us/evalcenter/)
2.  Download a Windows Server and a Windows 10 image (For the purpose of this article we’ll assume Windows Server 2016 and Windows 10 Pro)

## Install AutomatedLab

Ok we’re finally ready to install AutomatedLab.

Open an escalated PowerShell Window and execute the following commands:

Install-Module -Name AutomatedLab -AllowClobber  
New-LabSourcesFolder

Great now AutomatedLab is installed!

> Note: There is a [MSI installer](https://github.com/AutomatedLab/AutomatedLab/releases) however, I’ve encountered several problems with it in the past.

## Place the ISO Images

Navigate to `D:\Labsources\ISOs` and place your ISO files in it.

Next open an elevated PowerShell Window and type in the following command (Ensure the filepath points to the right location for the LabSources folder)

Get-LabAvailableOperatingSystem -Path D:\LabSources

This should return a list of all operating system images found in the ISOs folder.

![](https://miro.medium.com/max/700/1*zfdzi3P3VoksGXbvE8IfPA.png)

# Install your first AD lab

1.  Open an elevated PowerShell ISE window and copy paste the following lines :

```powershell
New-LabDefinition -Name GettingStarted -DefaultVirtualizationEngine HyperV 
  
Add-LabMachineDefinition -Name DC1 -Memory 1GB -OperatingSystem 'Windows Server 2016 Datacenter (Desktop Experience)' -Roles RootDC -DomainName contoso.comAdd-LabMachineDefinition -Name Client1 -Memory 1GB -OperatingSystem 'Windows 10 Pro' -DomainName contoso.com  
  
Install-Lab  
  
Show-LabDeploymentSummary
```

Once you execute the script AutomatedLab will do the following:

1.  Download Sysinternals tools and place them into the LabSources folder.
2.  It will look for an ISO file that contains the specificed OS name. If it can’t find the ISO it will stop at this point.
3.  AL will create a virtual switch
4.  Then it will create base images for each operating system
5.  It will create a Windows Server 2016 root domain controller with the domain name contoso.com
6.  It will also create a Windows 10 Pro client machine connected to the contoso.com domain

Grab a coffee!

-   Once completed open an elevated Hyper-V Manager window and ensure everything is running.
-   Create a checkpoint for each VM before continuing.
-   You’ve successfully deployed your first AD lab!

# Removing the Lab

Open an elevated PowerShell window and execute the following commands:

```powershell
Get-Lab -List  
Import-Lab _LabName_  
Remove-Lab
```

It should look similar to this:

![](https://miro.medium.com/max/700/1*6bvGhKL78_TQ99aoz2DDlQ.png)

# Concluding Remarks

The next article will go through how to build an AD Lab on [Proxmox VE](https://www.proxmox.com/en/) in a more manual fashion including additional VMs.

If you have any feedback I’d love to hear it you can reach out to me on [Twitter](https://twitter.com/drunkrhin0).

My views are my own and do not reflect my employers
