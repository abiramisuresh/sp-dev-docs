---
title: Consuming Tenant Admin
description: This article describes the role and responsibilities of Consuming Tenant Admin in SharePoint Embedded
ms.date: 11/28/2023
ms.service: sharepoint-online
ms.localizationpriority: high
---
# Consuming Tenant Admin

The organizations that use the SharePoint Embedded applications on their Microsoft 365(M365) tenants are the consuming tenants and the persona that is responsible for managing these applications on their M365 tenancy is the consuming tenant administrator. Consuming tenant administrators can perform various administrative actions on the SharePoint Embedded applications registered on their M365 tenant and on the Containers that hold the content. They can also manage tenant level configurations and ensure that data is stored in a secure, protected way that meets customers’ business and compliance policies. In this article, we describe the enterprise manageability features that are supported ans can be performed by the consuming tenant administrator.  

### 1. Consuming Tenant Admin Role 

M365 SharePoint Administrator serves as the consuming tenant admin.  Global Administrators in M365 can assign users the SharePoint Administrator. The Global Administrator role already has all the permissions of the SharePoint Administrator role. For information about assigning a user the SharePoint administrator role, see [Assign admin roles in the Microsoft 365 admin center](https://learn.microsoft.com/en-us/microsoft-365/admin/add-users/assign-admin-roles?view=o365-worldwide). 

### 2. Administration Tools

Consuming tenant admins will be able to manage SharePoint Embedded applications with PowerShell commands using [SharePoint Online Management Shell](https://learn.microsoft.com/en-us/powershell/sharepoint/sharepoint-online/connect-sharepoint-online). 

To get started using PowerShell to manage SharePoint Embedded, you have to [install](https://www.microsoft.com/en-us/download/details.aspx?id=35588) the SharePoint Online Management Shell and [connect to SharePoint Online](https://learn.microsoft.com/en-us/powershell/module/sharepoint-online/connect-sposervice?view=sharepoint-ps). You need version 16.0.24211.12000 or higher to run the commands for SharePoint Embedded.

### 3. Application Administration

With PowerShell cmdlets, tenant admin can get a list of SharePoint Embedded applications registered in their M365 tenancy. They can also view all the applications that have read and/or write access and the level of access to these SharePoint Embedded applications.
The following commands can be used manage SharePoint Embedded applications registered on your M365 tenants. 

```powershell
Get-SPOApplication
```
```powershell
Get-SPOApplication -OwningApplicationId <OwningApplicationId>
```
```powershell
Get-SPOApplication -OwningApplicationId <OwningApplicationId> -ApplicationId <ApplicationId>
```
OwningApplicationId is the ID of the SharePoint Embedded application and ApplicationId is the ID of the application that has access to the SharePoint Embedded application.
For more information about using this command, see [Get-SPOApplication cmdlet](https://learn.microsoft.com/en-us/powershell/module/sharepoint-online/get-spoapplication?view=sharepoint-ps)

### 4. Container Administration

* #### 4.1 **View Containers:** 
  Admins can get a list of all the containers for a SharePoint Embedded application using the following commands. This command will list all the active containers within the   application.

  ```powershell
  Get-SPOContainer -OwningApplicationId <OwningApplicationId> | FT
  ```
  OwningApplicationId is the ID of the SharePoint Embedded application. For more infromation about using this command, see [Get-SPOContainer cmdlet](https://learn.microsoft.com/en-us/powershell/module/sharepoint-online/get-spocontainer?view=sharepoint-ps)

* #### 4.2 **View details of a Container:** 
  Admins can get the details of a container within an application using the following command. This command will return more details of a container including StorageUsed, Ownership     details, SiteURL, Label information etc.
  
  ```powershell
  Get-SPOContainer -OwningApplicationId <OwningApplicationId> -Identity <ContainerId>  
  ```
  Here, the Identity is the ID of the Container. For more information about using this command, see [Get-SPOContainer cmdlet](https://learn.microsoft.com/en-us/powershell/module/sharepoint-online/get-spocontainer?view=sharepoint-ps)

* #### 4.3 **Delete Containers:** 
  When admins deletes a Container, it is moved into the deleted container collection. A deleted container can be restored from the collection within 93 days. If a container is deleted     from the collection, or it exceeds the 93-day retention period, it is permanently deleted.Deleting a container deletes everything within it, including all documents and files.

  Admins should notify the Container owners before you delete a Container so they can move their data to another location, and also inform users when the Container will be deleted.

> [!WARNING]
> Deleting a container may cause unexpected issues for the SharePoint Embedded application the Container belongs to and may interrupt usage of the application.
>
   ```powershell
  Remove-SPOContainer -Identity <ContainerId> 
  ```
  ContainerId is the ID of the container that will be moved to the deleted container collection. For more information about using this command, see [Remove-SPOContainer cmdlet](https://learn.microsoft.com/en-us/powershell/module/sharepoint-online/remove-spocontainer?view=sharepoint-ps)

* #### 4.4 **View deleted containers:** 
  Admins can get a list of deleted containers on the deleted container collection using the following command. For more information about using this command, see [Get-SPODeletedContainer](https://learn.microsoft.com/en-us/powershell/module/sharepoint-online/get-spodeletedcontainer?view=sharepoint-ps)
```powershell
Get-SPODeletedContainer
```
* #### 4.5 **Restore deleted containers:** 
Admins can restore a deleted container from the deleted container collection using the following command. For more information about using this command, see [Restore-SPODeletedContainer cmdlet](https://learn.microsoft.com/en-us/powershell/module/sharepoint-online/get-spodeletedcontainer?view=sharepoint-ps)
```powershell
Restore-SPODeletedContainer -Identity <ContainerId>
```
* #### 4.6 **Permanently delete Containers** 
Admins can permanently delete a Container from the deleted container collection if the Container has no further retention policies applied to it. For more information about using this command, see [Remove-SPODeletedContainer cmdlet](https://learn.microsoft.com/en-us/powershell/module/sharepoint-online/remove-spodeletedcontainer?view=sharepoint-ps) 
```powershell
Remove-SPODeletedContainer -Identity <ContainerId>
```

### 5. Tenant Administration

SharePoint Online enables admins to manage various tenant-wide settings with the [Set-SPOTenant](https://learn.microsoft.com/en-us/powershell/module/sharepoint-online/set-spotenant?view=sharepoint-ps) PowerShell command. This command allows administrators to modify global settings that impact the behavior and functionality of SharePoint Online for all users in the organization. 

These tenant-wide settings are also applicable to all SharePoint Embedded applications on the tenant. These settings include conditonal access policies, BlockDownloadFileTypePolicy, SharingCapabilty to name a few.  Full list of Set-SPOTenant [here](https://learn.microsoft.com/en-us/powershell/module/sharepoint-online/set-spotenant?view=sharepoint-ps).


#### 5.1 **Unique External Sharing settings for SharePoint Embedded:**

Admins can configure external sharing settings ONLY for SharePoint Embedded applications at the tenant level with the following commands. The external sharing features let users in your organization share content with people outside the organization (such as partners, vendors, clients, or customers), ensuring sensitive data is not accidentally shared with unauthorized users.
  
```powershell
SET-SPOTenant -ContainerSharingCapability <ContainerSharingCapabilities>
```
> [!NOTE]
> * External sharing for SharePoint embedeed is defaulted to the tenant setting set with Set-SPOTenant [-SharingCapability <SharingCapabilities>].
> * External sharing settings for SharePoint Embedded must be equally or more restrictive than the tenant-wide external sharing settings.


Other unique sharing settings for SharePoint Embedded application include:
```powershell
SET-SPOTenant -ContainerDefaultShareLinkScope 
```

```powershell
SET-SPOTenant -ContainerDefaultShareLinkRole
```

 ```powershell
SET-SPOTenant -ContainerDefaultLinkToExistingAccess 
```
For more information about the tenant level sharing settings, see [Set-SPOTenant cmdlet](https://github.com/cindylay/OfficeDocs-SharePoint-PowerShell/edit/cindy/spocontainer/sharepoint/sharepoint-ps/sharepoint-online/Get-SPOContainer.md) 

  
### 6. Security and Compliance Administration
SharePoint Embedded leverages Microsoft’s comprehensive compliance and data governance solutions to help organizations manage risks, protect, and govern sensitive data, and respond to regulatory requirements. Security and compliance solutions will work in a similar manner in the SharePoint Embedded platform as they do today in Microsoft 365 (M365) platform so that data is stored in a secure, protected way that meets customers’ business and compliance policies while making it easy for Compliance and SharePoint Administrators to enforce critical security and compliance policies on the content. For information on supported securiity and compliane capabilties, see [Security and Compliance](../security-and-compliance.md)

