---
title: Manually install winget on windows 10
tags: winget, appinstaller, msibunddle
reference: https://docs.microsoft.com/en-us/windows/package-manager/winget/
public: true
---

- Introduction
  If  you are curious like me and eager to get to use the new window package manager, you certainly needed to use winget command line tool.
  
  The winget client is the main utility to manage packages on a windows 10 / 11 machines. 
  It is mainly installed as part of the windows app installer which can be directly installed from the windows app store (check the following article for details on this https://devblogs.microsoft.com/commandline/windows-package-manager-1-0/).
  
  However, in case you are running a device where installing apps from the windows app store is disabled for certain reasons, and you still wish to run an use the winget command line tool, here are the steps that should be followed for you in order to get the winget command line tool installed.
- Download the latest release of windows app installer msixbundle available at the following github repo https://github.com/microsoft/winget-cli/releases
  ![image.png](../assets/image_1641203235853_0.png)
	- download the latest c++ runtime desktop bridge package from the following location
	   https://docs.microsoft.com/en-us/troubleshoot/cpp/c-runtime-packages-desktop-bridge
	   make sure to select the package the corresponds to your device architecture mine is x64
	- Open powershell 7  and run the following command 
	  ~~~powershell
	  Import-module appx -usewindowspowershell
	  add-appxpackage  '<pathtodowloadedc++ package>'
	  add-appxpackage '<pathtodownloaded .msixbundle>'
	  ~~~powershell
	- once you have successfully ran the above mentioned commands you can run the
	  ~~~powershell
	  winget
	  ell
	  command from powershell to check if the winget command line tool is successfully installed and available for use 
	  ![image.png](../assets/image_1641205149293_0.png){:height 461, :width 723}