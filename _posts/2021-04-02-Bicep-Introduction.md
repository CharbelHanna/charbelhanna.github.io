---
title:  "Azure Bicep - New Generation ARM Templates"
excerpt: "Part 1 - Introduction"
categories: Azure
author_profile: true
author: komo
layout: single
toc: true
toc_sticky: true
show_date: true
read_time: true
header:
  teaser: /assets/images/6.jpg
  overlay_image: /assets/images/6.jpg
  overlay_filter: 0.5
---
# Background

<p>As a cloud architect working on cloud environments on daily basis trying to reply to various technical and organizational demands while coping with technology changes and updates , Infrastructure as Code has become vital to perform almost all kind of operations. <br><br>Despite the fact that there are many languages / utilities offered by third party vendors that already exists in the market (which are by the way very good and powerful), you might be in a position like I am where you have to use only native languages provided by your public cloud provider "Microsoft Azure" in this context. <br>JSON ARM templates where the only available native solution from Microsoft to be used for Azure which I was using until recently a new promising language "<strong>Bicep</strong>" has been released, which for my opinion represents the future of ARM templates.</p>

<p>That being said, I felt that it worth shedding light on Bicep and share my experience on using this new language for IaC deployments through a series of articles which I hope that you find interesting and useful.</p>

# Part 1 - Azure Bicep - Introduction
[Part 2 - Azure Bicep - Deploy your Fist code]({% link _posts/2021-04-29-Bicep-Deploy-your-first-code.md %}) <br>
[Part 3 - Azure Bicep - Advanced Deployments with Bicep modules]( {% link _posts/2021-05-26-Bicep-Deploy-using-modules.md %})


## First things first, what is Azure Bicep

As per the official documentation, "Bicep is a Domain Specific Language (DSL) for deploying Azure resources declaratively . It aims to drastically simplify the authoring experience with a cleaner syntax, improved type safety, and better support for modularity and code re-use.
Bicep is a transparent abstraction over ARM and ARM templates, which means anything that can be done in an ARM Template can be done in Bicep (outside of temporary known limitations).
All resource types, apiVersions, and properties that are valid in an ARM template are equally valid in Bicep on day one".

So in other terms Azure Bicep is a new authoring language that if once used it eliminates the need of developing standard ARM JSON templates, which I personally think is a turning point since this will drastically simplify template development hence considerably reduce authoring and deployment time.

## So how Bicep actually works?

What Bicep does under the hood actually is compiling the bicep code to ARM templates which they are in turn deployed on Azure by Azure Resource Manager, cool isn't ?

<figure class="wp-block-image size-full is-style-default"><img src="https://zerohoursleep.net/wp-content/uploads/2021/04/image-2.png" alt="" class="wp-image-2418"/><figcaption>how it actually works</figcaption></figure>


<p><strong>Wait a minute, so is this means that at the end we are always be using Azure ARM! </strong><br>Actually Yes! This means we can still benefit from all existing and NEW features and capabilities of ARM templates. Which also means that <strong>Azure Bicep is NOT a replacement of Azure ARM</strong></p>

<p><strong>So why bothering using a new language?</strong>
<br>The main reason is to simplify the authoring experience. Whether you are used to or new to ARM templates we all agree on the complexity of the syntax that they hold, which makes it hard to manage and maintain as they written in JSON format which is not considered as a user friendly language. Bicep does not only provide a simpler syntax but it also provide additional enhancements like automatic dependencies generation and much more. <br>at the time of writing this post Bicep is in version 0.13 which supports all existing ARM template features.</p>

## Bicep deployment process

The deployment process using bicep is easy and straight forward all you have to do is :
1) create your bicep code and save it as .bicep files
2) Deploy bicep code to azure using either Az CLI or Az powershell

<figure class="wp-block-image size-full is-resized is-style-default"><img src="https://zerohoursleep.net/wp-content/uploads/2021/04/image-8.png" alt="" class="wp-image-2427" width="721" height="349"/><figcaption>Bicep deployment process</figcaption></figure>
<p><br>it is to note that the deployment of bicep files can still be done using existing commands such as<br>in Az powershell</p>

{% highlight powershell %}
New-AzResourceGroupDeployment 
{% endhighlight %} 

or Az cli

{% highlight cli %}
Az deployment group
{% endhighlight %} 

So now we have an idea about what is Azure Bicep and how it works, let's jump over and start deploying our first bicep code.

## First let's setup together the development environment

<p>Bicep requires the Bicep cli to be able to compile bicep code to ARM templates which can be provided by the following:</p>

<ul><li><a href="https://docs.microsoft.com/en-us/cli/azure/install-azure-cli" target="_blank" rel="noreferrer noopener">Azure cli</a> (<strong>required version v2.20.0 or later</strong>) can automatically install Bicep cli when deploying the bicep code, there's no need for any manual installation, however you can still manually install the bicep cli by running the below command</li></ul>

{% highlight cli %}
az bicep install
{% endhighlight %} 

<figure class="wp-block-image size-large is-style-default"><img src="https://zerohoursleep.net/wp-content/uploads/2021/04/pages_Azure-Bicep-New-generation-ARM-Templates_1617049424645_0.png" alt="" class="wp-image-2429"/></figure>

<ul><li><a href="https://docs.microsoft.com/en-us/powershell/azure/install-az-ps?view=azps-5.7.0">Azure </a><a href="https://docs.microsoft.com/en-us/powershell/azure/install-az-ps?view=azps-5.7.0" target="_blank" rel="noreferrer noopener">Powershell</a><a href="https://docs.microsoft.com/en-us/powershell/azure/install-az-ps?view=azps-5.7.0"> module</a> (<strong>required version v5.6 or later</strong>) does not have the ability to install the Bicep cli directly therefore, it expect that the bicep cli is already installed. You can manually install the bicep cli by following on the installation methods mentioned in the <a href="https://github.com/Azure/bicep/blob/main/docs/installing.md#manually-install" target="_blank" rel="noreferrer noopener">reference link</a></li></ul>

You can check the version of the installed bicep cli by running

{% highlight cli %}
bicep --version
{% endhighlight %} 

<figure class="wp-block-image size-large is-resized is-style-default"><img src="https://zerohoursleep.net/wp-content/uploads/2021/04/pages_Azure-Bicep-New-generation-ARM-Templates_1617050660001_0.png" alt="" class="wp-image-2430" width="680" height="238"/></figure>

For the best authoring experience, Bicep language service can be used which is part of the Bicep visual studio extension, this is a great extension that supports validation, intellisense code completion and many more important features that can be used to give an easy and simple authoring experience

<figure class="wp-block-image size-large is-style-default"><img src="https://zerohoursleep.net/wp-content/uploads/2021/04/pages_Azure-Bicep-New-generation-ARM-Templates_1617050798507_0.png" alt="" class="wp-image-2431"/></figure>

## Conclusion

That was the first part of the Bicep series in which we introduced Bicep, how it works and what is required to start using it in your Azure deployments.
Stay tuned for Part two where we are going to get your hands dirty deploying Bicep


