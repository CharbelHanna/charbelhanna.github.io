---
title:  Azure Bicep - New Generation ARM Templates
excerpt: Part 2 - Azure Bicep - Deploy Your First Code
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
# Part 2 - Azure Bicep - Deploy Your First Code<br>
[Part 1 - Azure Bicep - Introduction]({% link _posts/2021-04-02-Bicep-Introduction.md %})<br>
[Part 3 - Azure Bicep - Advanced Deployments with Bicep modules]({% link _posts/2021-05-26-Bicep-Deploy-using-modules.md %})

## Introduction

<p>In [part-one]({% link _posts/2021-04-02-Bicep-Introduction.md %}) of this series we have introduced Bicep and explained how it works. <br>so now it is time to get our hands dirty by starting deploying our first bicep code.</p>

<p>In this part I will walk you through how to deploy your Azure resources by using Bicep with emphasis on the syntax requirements and differences between Bicep and standard ARM Templates.</p>

## Prerequisites

We assume that your development environment is setup for bicep, (if you still did not setup your development environment please follow the instructions mentioned in  [part-one]({% link _posts/2021-04-02-Bicep-Introduction.md %})

## Create your Bicep file

Open vscode and create your bicep file, you can give the file any name that you want as long as you save it under the ".bicep" extension. For the purpose of this tutorial I will create a bicep file and call it main.bicep.
I like to create and save the file as an empty file first so vscode recognize it as a .bicep file and load the bicep tools extension directly as soon as I start authoring.

![image-1619113760243-0](https://i.ibb.co/DQJWR33/image-1619113760243-0.png){:height="100%" width="100%"}

## Bicep file structure

Unlike standard JSON ARM templates, bicep does not require a predefined schema to exist in the beginning of the file so the file be properly treated as a deployment template.
Which means in a bicep file you start authoring directly without adding any specific elements related to the file itself to make it readable.

The picture below shows an empty ARM template.

![image-1619418259449-0](https://i.ibb.co/xhb4p2k/image-1619418259449-0.png){:height="100%" width="100%"}
<br>
<div style="border: solid; border-color: #e0f2ff; background-color: #e0f2ff; border-radius: 10px; width: 90%; padding: 10px; margin: auto;">
<p style="color: #003a66;" align="center">Note the first lines and the of code in the top of the file</p>
</div>
<br>
<p><strong>If we take a look on a bicep file, we will directly notice the difference and how simple it is</strong></p>

![image-1619441036863-0](https://i.ibb.co/KLZ9WG0/image-1619441036863-0.png){:height="100%" width="100%"}

For example the above code permit the deployment of a public IP address resource just by specifying the resource type and related properties. cool isn't ?

## Defining Azure Resources in Bicep

Azure resources are the building bloc of a bicep file, since at the end the target is to deploy or update an azure resource, therefore, the minimum required block in a bicep file is a resource bloc.
The resource bloc define the type of the resource that you want to interact with.
Each resource bloc includes the following:

{% highlight powershell %}
resource <resouceSymbolicName> 'ResourceType@<Apiversion>' = {
property1:<somevalue>
property2:<somevalue>
.... }
{% endhighlight %} 

**There are couple of things to note here**

1) Each resource is represented by a symbolic name to be used as a reference for the resource throughout the file. 
2) The API version is appended directly to the 'ResourceType'  which allows you to directly choose the API version to be used during the deployment.
   
Ok so now we know that we need to specify the resource type but the first question that pops up is from where we can get the resource type ?
Actually the answer is very simple, thanks for the Bicep vscode extension that will automatically display the resource Type along its API version just as soon as you start typing the name of the and for finding the allowed properties, you can use the "Ctrl+Space" key combination to open the properties selector as shown below.

{% include video id="dPTMrCHuyS4" provider="youtube" %}

## Author the first code

let's assume we want to create a storage account with the following information:
- Storage account name: komobicepstg01
- Storage account type: general purpose V2
- Replication policy: local redundant
- performance tier: standard
  
based on what we have seen so far our bicep file should should contains the following code.

{% highlight powershell %}
resource stg 'Microsoft.Storage/storageAccounts@2021-02-01' = {
  name: 'komobicepstg01'
  location: 'westeurope'
  kind: 'StorageV2'
  sku: {
    name: 'Standard_LRS'
    tier: 'Standard'
  }
}
{% endhighlight %} 

## Deploying the bicep code
Now we have our code complete, we are ready to deploy it to Azure in an existing resource group, to achieve this I have I can either use powershell or Az cli, I will deploy it using powershell by running the following command.

{% highlight powershell %}
New-AzResourceGroupDeployment -templateFile "C:\Coding\Repos\Komo\bicep-tuto\App01\main.bicep" `
-ResourceGroupName bicep-tuto-rg -Verbose
{% endhighlight %}
<br>
<div style="border: solid; border-color: #e0f2ff; background-color: #e0f2ff; border-radius: 10px; width: 90%; padding: 10px; margin: auto;">
<p style="color: #003a66;" align="justify">To note that here the bicep deployment shown below will be performed on a resource group scope rather than on the subscription scope since the targetscope is not explicitly defined in t</p>
</div>
<br>

{% include video id="X0YjSATbuyo" provider="youtube" %}

## Working With Parameters

Despite that the bicep file we have created earlier works properly to deploy the intended resource, however it is would be difficult to use it for multiple deployments since all properties values are included directly within the resource bloc of the file itself.<br><br>
To overcome this problem we should use parameters to dynamically inject values for specific properties without the need of each time through the code.<br><br>
Fortunately defining and using parameters in a bicep file is easy and straight forward.<br>
In this section we will see how parameters can be defined and used with bicep.

<blockquote class="wp-block-quote"><span style="color:#1380ae" class="has-inline-color">To note that same as in ARM templates bicep supports defining parameters and setting parameters values either within the bicep file itself or in a separate file.</span></blockquote>

## Defining parameters, variables and setting values
A parameter or a variable can be defined using the following form of code
<br>
{% highlight powershell %}
param StorageAccountName string
var location = resroucegroup().location
{% endhighlight %}
<br>
A parameter value can be set using the following form of code
<br>
{% highlight powershell %}
param StorageAccountName string = 'komobicepstg01'
{% endhighlight %}
<br>

In this example we defined the storageAccountName as a string type parameter with a value
of “komobicepstg01”, and a variable name location to the value of the resource group location where the deployment will be performed.

## Calling Parameters And Variables
A parameter or a variable can be called by simply typing the parameter name like below
{% highlight powershell %}
name:StorageAccountName
location:location
{% endhighlight %}

<blockquote class="wp-block-quote"><span style="color:#1380ae" class="has-inline-color">Notice that the parameter and variable names are used directly without quotes or brackets or even mentioning if we are calling a variable or a parameter unlike the way it is required in a JSON ARM template which dramatically simplify the authoring experience.</span></blockquote>

## Putting it all together
Our bicep file should be like this

{% highlight powershell %}
//---parameters-----------
param StorageAccountName string = 'komobicepstg01'
param StorageAccountType string = 'StorageV2'
param StorageAccountSku object = {
  name: 'Standard_LRS'
  tier: 'Standard'
}

//---variables-----------
var location = resourceGroup().location

//---resources-----------
resource stg 'Microsoft.Storage/storageAccounts@2021-02-01' = {
  name:StorageAccountName
  location:location
  kind:StorageAccountType
  sku: {
    name:StorageAccountSku.name
    tier:StorageAccountSku.tier
  }
}
{% endhighlight %}

## Using a separate template file with bicep

As mentioned earlier we can use separate parameter file with bicep, this will give us more flexibility in editing and supplying parameters values without touching the bicep code.
At the time of writing this article ".json" parameters files are the only supported. That means yes the same parameters file used with ARM template can be used with a bicep file.
so our parameters file for the storage account creation should look like this

{% highlight json %}
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "StorageAccountName": {
            "value": "komobicepstg01"
        },
        "StorageAccountType": {
            "value": "StorageV2"
        },
        "StorageAccountSku": {
            "value": {
                "name": "Standard_LRS",
                "tier": "Standard"
            }
        }
    }
}
{% endhighlight %}
<br>
<blockquote class="wp-block-quote"><span style="color:#1380ae" class="has-inline-color">Note that the values set in the main.bicep file itself earlier represents the parameters default values, therefore, these values are overwritten by the values are set in the parameters file that we have just created.<br>If you prefer to not use default values like myself you can remove the parameters values from the main.bicep file.</span>
</blockquote>

{% highlight powershell %}
//---parameters-----------
param StorageAccountName string
param StorageAccountType string
param StorageAccountSku object

//---variables-----------
var location = resourceGroup().location

//---resources-----------
resource stg 'Microsoft.Storage/storageAccounts@2021-02-01' = {
  name:StorageAccountName
  location:location
  kind:StorageAccountType
  sku: {
    name:StorageAccountSku.name
    tier:StorageAccountSku.tier
  }
}
{% endhighlight %}
<br>
The previous deployment can be performed using a separate file as follows
{% highlight powershell %}
New-AzResourceGroupDeployment -templateFile "C:\Coding\Repos\Komo\bicep-tuto\App01\main.bicep" `
-TemplateparameterFile "C:\Coding\Repos\Komo\bicep-tuto\App01\parameters.json" `
-ResourceGroupName bicep-tuto-rg -Verbose
{% endhighlight %}

## Conclusion

We can clearly conclude that using bicep simplifies the deployment of azure resources while preserving the same deployment experience by using the same deployment commands used with ARM template deployment.
Also bicep capabilities provide a much simpler experience against the standard JSON ARM templates.
<br>
<br>
 hope you find this article in this series useful and helps you get starting using bicep. 
your feedback is appreciated.

