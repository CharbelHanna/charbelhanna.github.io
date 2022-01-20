---
title:  Azure Bicep - New Generation ARM Templates
excerpt: Part 3 - Advanced Deployments with Bicep Modules
categories: Azure
author_profile: true
author: komo
layout: single
show_date: true
read_time: true
toc: true
toc_sticky: true
header:
  teaser: /assets/images/6.jpg
  overlay_image: /assets/images/6.jpg
  overlay_filter: 0.5
---

# Part 3 - Azure Bicep - Advanced Deployments with Bicep Modules
[Part 1 - Azure Bicep - Introduction]({% link _posts/2021-04-02-Bicep-Introduction.md %})<br>
[Part 2 - Azure Bicep - Deploy your Fist code]({% link _posts/2021-04-29-Bicep-Deploy-your-first-code.md %})

## Introduction

<p>In this part of the Bicep series we will discuss how to perform advanced deployments using Bicep as an Infrastructure As Code DSL language, while shedding the light on Bicep modules.</p>

## Prerequisites

- We assume that your development environment is setup for bicep, (if you still did not setup your development environment please follow the instructions mentioned in [Part-one]({% link _posts/2021-04-02-Bicep-Introduction.md %})
- We assume that you have minimum knowledge on how to deploy azure resources using bicep, you can follow the instructions in [Part-two]({% link _posts/2021-04-29-Bicep-Deploy-your-first-code.md %})

## The Regaton App

<p>Let's consider a scenario where want to programmatically deploy and maintain the below illustrated application  in your azure environment using bicep</p>

<figure class="wp-block-image size-large is-style-default"><img src="https://i.ibb.co/Ltkfhvt/image-1621248001664-0.png" alt="image-1621248001664-0"/></figure>

### Detailing the application components

<p>As shown in the above diagram our application &quot;Regaton&quot; consists in the following main components:</p>

<ul><li>A Web Application</li><li>An SQL Database</li><li>A storage Account<br>Yes only three Azure services but why consider it as complex ?<br>Actually if we take a deep look we notice the following:</li><li>The resources are distributed between different resource groups, this actually means that our deployment include different targets.</li><li>Azure services like the <strong>Azure SQL Database</strong> and the <strong>Azure Web app</strong>, depends on other type of services such as "<strong>SQL Server</strong>", and "<strong>App service plan</strong>" to be available therefore should also be deployed.</li></ul>

## Authoring concept

<p>Let's see how we can achieve our required deployment while meeting all the requirements.<br><br>If we were using ARM templates we would use either nested templates which are child templates that are written in the same template file or linked templates which are ARM templates stored in a remote location ( mainly a storage account) and referenced by their Template Uri in the parent template.<br><br>When the above options represent a valid solution, however they do also represent many challenges which are not simple to overcome. Fortunately these challenges were eliminated with Bicep modules.</p>

### Modularization

<p>Bicep allows splitting your deployment code into multiple local bicep files to be consumed as modules in your deployment. We refer to this concept by modularization.<br>Modularization offers many advantages among the below:</p>

<ul><li>The ability of using existing written modules in different deployments while applying minor or i many cases zero changes to the existing code.</li><li>Allows Masking the complex details from the parent bicep files as only parameters and outputs are required to be exposed.</li><li>Most importantly allows targeted deployments to specific scopes (subscription, resource groups)</li></ul>

## Deployment code structure
<p>Inspite of the above mentioned facts our Regaton application deployment code will consists of the following:</p>
<ul><li>A main bicep file that will contain the main code which is responsible of calling multiple modules each module with aim to deploy a set of resources in a target location.</li><li>While multiple parameters files can be used, deployment parameters will be stored in a single file and passed through the main bicep file to all modules.</li></ul>
<figure class="wp-block-image size-large is-style-default"><img src="https://i.ibb.co/FY7Fv5g/image-1621934471120-0.png" alt="image-1621934471120-0"/></figure>

## Bicep modules in action

<p>While the complete deployment code of the Regaton application is available for you in the following github repository <a rel="noreferrer noopener" href="https://github.com/CharbelHanna/Azure/tree/main/Bicep/Templates/Regaton" data-type="URL" data-id="https://github.com/CharbelHanna/Azure/tree/main/Bicep/Templates/Regaton" target="_blank">CharbelHanna/Azure/Bicep</a>.  let's start exploring the main bicep file &quot;regaton.bicep&quot; to discuss in details how the different modules are consumed.</p>
<p><a href="https://ibb.co/HtqtjmM"></a></p>
<figure class="wp-block-image size-large is-style-default"><img src="https://i.ibb.co/ZBxB0yw/image-1622021428194-0.png" alt="image-1622021428194-0"/></figure>

<p>Taking a closer look on the selected code, we will notice the following:</p>
<ul><li>To consume a module the following syntax is used</li></ul>

{% highlight powershell %}
module <modulesymoblikname> 'Modulepath' = {
name: <ModuleName> // the deployment name that will be logged in Azure activity logs
scope: <Deploymentscope> // specifying the scope where the deployment should take place
params: {
  your parameters goes here
  } 
}
{% endhighlight %}

<ul><li>The deployment scope is defined by referencing the symbolic name of the resource group that was created earlier, for example<strong> stgrg </strong>for the storage account resource group "regaRG3"</li></ul>

<figure class="wp-block-image size-large is-style-default"><img src="https://i.ibb.co/4m4kzRM/image-1622027693226-0.png" alt="image-1622027693226-0"/></figure>

<ul><li>Outputs from different modules are passed as parameters to other modules </li></ul>
<p>In order to configure the web app to use and connect to the storage account and SQL database that are being deployed, it is essential to get the outputs from the respective modules. This is done as following:</p>
<ul><li>Defining the output variables in the source module as below</li></ul>
<figure class="wp-block-image size-large is-style-default"><img src="https://i.ibb.co/bd8L6bp/image-1622033682892-0.png" alt="image-1622033682892-0.png"/></figure>

<blockquote class="wp-block-quote">Notice the cross reference that we are using to supply the value of the output variable storageEndpoint</blockquote>

<ul><li>Passing the output as parameters to the destination module as below</li></ul>

<figure class="wp-block-image size-large is-style-default"><img src="https://i.ibb.co/RY8wPBK/image-1622034830979-0.png" alt=""/></figure>
<!-- /wp:image -->
<!-- wp:quote {"canvasClassName":"cnvs-block-core-quote-1622035055888"} -->
notice the parameters value reference &lt;Modulesymname&gt;.outputs.&lt;variablename&gt;{: .notice}

## Conclusion

In this series I tried to shed the light on Azure Bicep as a new DSL language and how it can be used for Azure deployments and the main Bicep features. <br>
More details on how to move from  ARM templates can be found [here](https://docs.microsoft.com/en-us/azure/azure-resource-manager/bicep/compare-template-syntax) .<br>Tt is important to note that Azure Bicep is 100% supported by Microsoft and it provides Day 0 resource provider support. Any Azure resource - whether in private or public preview or GA - can be provisioned using Bicep.
