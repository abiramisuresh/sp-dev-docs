---
title: PAYG billing for SharePoint Embedded 
description: This articles explains how to set up PAYG billing
ms.date: 11/28/2023
ms.service: sharepoint-online
ms.localizationpriority: high
---

# SharePoint Embedded  Billing experience
This document outlines the steps an admin will have to follow to set up billing, evaluate actual and forecast costs, and pay invoices on SharePoint Embedded . 

SharePoint Embedded  will be a consumption based PAYG (Pay-as-you-go) offering from SharePoint Online, which means you will pay only for what you use. As a Developer admin for SharePoint Embedded , you will set up billing on a container type using SharePoint Online Management Shell and manage forecast of actual costs and invoices in Microsoft Azure portal. 

<img width="800" alt="Biling payg" src="../../images/billing-manage.png">

## Set up Pay-as-you-go (PAYG) for SharePoint Embedded 

To enable PAYG on SharePoint Embedded , you need:

  1. Existing SharePoint tenancy
  2. An Azure subscription in the tenancy
  3. A resource group attached to the azure subscription 

## Roles and Permissions
- Admin who will set up billing relationship for SharePoint Embedded  will need to have owner or contributor permissions on Azure subscription. 
- Admin will need to have a SharePoint administrator role to operate billing cmdlets
 
## Steps to establish billing 

1. Download and install the [latest version of SharePoint Online Management Shell](https://www.microsoft.com/en-us/download/details.aspx?id=35588)
2. Open SharePoint Online Management Shell from **Start** screen, type **sharepoint**, and then click **SharePoint Online Management Shell**.
3. Connect to SPO service using `Connect-SPOService` cmdlet by providing admin credentials associated with tenancy. For information on [how to use Connect-SPOService](https://learn.microsoft.com/en-us/powershell/module/sharepoint-online/connect-sposervice?view=sharepoint-ps), refer the linked documentation.
4. Create container type using `New-SPOContainerType` cmdlet by providing an **azure subscription**, **resource group** associated with the subscription and a **region**.
   
   a. If you do not have an azure subscription, you can create on by following steps here to [create an Azure subscription in your tenancy](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/initial-subscriptions)
   
   b. If you do not have a resource group, you can create on by following steps here to [create a resource group](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal)
   

```ps
New-SPOContainerType -ContainerTypeName <ContainerTypeName> -OwningApplicationId <OwningApplicationId> -AzureSubscriptionId <AzureSubscriptionId> -ResourceGroup <ResourceGroup> -Region <Region>
```  

 ## Applicable guidelines
 1. Every container type must have an owning application
 2. A single owning app can only own one container type at a time
 3. An azure subscription can be attached to any number of container types

<br></br>
## View/Edit billing properties 

1. View the properties of billing
   You can view properties of ContainerType and associated billing properties by using one of the two PowerShell cmdlets

   a. To see all containertypes and billing properties except associated region use:

```ps
Get-SPOContainerType
```


   b. To see billing properties associated with a container type including region use:
   
```ps
Get-SPOContainerType -ContainerTypeId <ContainerTypeId>
```

3. Update Azure subscription or resource group associated with a container type use:
   
```ps
Set-SPOContainerType -ContainerTypeId <ContainerTypeId> [-AzureSubscriptionId <AzureSubscriptionId>] [-ResourceGroup <ResourceGroup>]
```

## Pricing & PAYG meters

Please review the Pricing and PAYG meters for SharePoint Embedded in the [terms of service](../../terms-of-service.md)

##  Manage consumption and invoices in Azure portal

1. Sign in to [**Azure Portal**](https://portal.azure.com/)
2. Search for **Cost Management + Billing** 
3. Select **Cost Analysis** to see
     - **Actual cost**

![image](../../images/billing-1.png)
       
Select '**+ Add Filter**' to see breakdown of cost by '**containertypeid**' and '**tenantid**'
         - Select **Tag**
         - In the key,value pair select '**containertypeid**' or '**tenantid**' and select respective containertypeid and tenantid
           
![image](../../images/billing-2.png)


5. Select **Billing** on the left navigation to see monthly invoices
6. Set up **Budget alerts** on cost by following steps [here](https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/cost-mgt-alerts-monitor-usage-spending)
