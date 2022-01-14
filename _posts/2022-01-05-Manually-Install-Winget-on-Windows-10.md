---
title:  "Manually Install Winget on Windows 10"
excerpt: "Bypass installations restrictions..."
categories: Tips and Tricks
author_profile: true
author: komo
layout: single
classes: wide
show_date: true
read_time: true
words_per_minute: 200
header:
  teaser: assets/images/wingetimage4.png
  overlay_image: assets/images/wingetimage4.png
  overlay_filter: 1.0

---
# Manually install winget on windows 10
## Introduction
<!-- wp:paragraph {"canvasClassName":"cnvs-block-core-paragraph-1641411783100"} -->
<p>If you are curious like me and eager to get to use the new window package manager, you certainly needed to use winget command line tool</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"canvasClassName":"cnvs-block-core-paragraph-1641411783104"} -->
<p>The winget client is the main utility to manage packages on a windows 10 / 11 machines.<br>It is mainly installed as part of the windows app installer which can be directly installed from the windows app store check the following article for details <a href="https://devblogs.microsoft.com/commandline/windows-package-manager-1-0/">https://devblogs.microsoft.com/commandline/windows-package-manager-1-0/</a> .<br><br>However, in case you are running a device where installing apps from the windows app store is disabled for certain reasons and you still wish to use the winget command line tool, here are the steps that should be followed for you in order to get the winget command line tool installed.</p>
<!-- /wp:paragraph -->
<div style="border: solid; border-color: #e0f2ff; background-color: #e0f2ff; border-radius: 10px; width: 90%; padding: 10px; margin: auto;">
<p style="color: #003a66;" align="center">at this time winget is supported with windows 10 versions (1809) or later</p>
</div>
### 1. Download the windows app installer
Download the latest release of windows app installer msixbundle available at the following github repo https://github.com/microsoft/winget-cli/releases
<figure class="wp-block-image size-large"><img src="https://i.ibb.co/mX8fMDz/image-1641203235853-0.png" alt=""/><figcaption> image-1641203235853-0.png </figcaption></figure>
### 2. Download the latest c++ runtime desktop bridge (optional but recommended)
Download the latest c++ runtime desktop bridge package from the following location
https://docs.microsoft.com/en-us/troubleshoot/cpp/c-runtime-packages-desktop-bridge
<div style="border: solid; border-color: #e0f2ff; background-color: #e0f2ff; border-radius: 10px; width: 90%; padding: 10px; margin: auto;">
<p style="color: #003a66;" align="center">make sure to select the package the corresponds to your device architecture</p>
</div>
### 3. Open powershell 7 and run the following
{% highlight powershell %}
Import-module appx -UseWindowsPowerShell 
Add-appxpackage '<pathtodowloadedc++packageName>' 
Add-appxpackage '<pathtodownloadedmsixbundleName>'
{% endhighlight %} 
### 4. verify successful installation
Once you have successfully ran the above mentioned commands you can run the winget command from powershell or cmd to check if it was successfully installed 
<figure class="wp-block-image size-large"><img src="https://i.ibb.co/dGkCcpw/image-1641205149293-0.png" alt=""/><figcaption> image-1641205149293-0.png </figcaption></figure>
## Conclusion
in this post we saw how to install and use the winget client to install windows applications. By using the windows package manager approach you will be able to manage installations much faster and easier!

Hope you enjoy it :)