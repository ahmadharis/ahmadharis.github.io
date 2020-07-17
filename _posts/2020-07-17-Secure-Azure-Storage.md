---
layout: post
title: Securing Azure Storage with Stored Access Policy.
meta-description: Securing Azure Storage with Stored Access Policy (SAP) and Shared Access Token (SAS).
author: Haris Ahmad
published-date: 2020-07-17
categories: [Azure]
tags: [Azure, Azure Storage, Security]
post-images-base-url: /assets/img/20200717-Secure-Azure-Storage
share-img: /assets/img/common/cloud-networking-01.png
---

Secure your Shared Access Signature (SAS) in Azure Storage by using Stored Access Policy (SAP). A Stored Access Policy defines access rights and restrictions. This policy can then be used to create Shared Access Signatures that can be used by clients.

An inherent benefit of using Stored Access Policy is that the Shared Access Signature can be revoked anytime by deleting the underlying Stored Access Policy. Without using Stored Acess Policy, the user may not be able to revoke a given Shared Access Signature, which may have an impact on the security of the Azure Storage account.

Stored Access Policy is a much safer option. Without the use of Stored Access Policy, the only way to revoke the Shared Access Signature is to regenerate the account key. This may also have an adverse impact on existing applications that rely on Shared Access Signature tokens generated with the account keys.

A better option would be to create a Stored Access Policy for each container and create Shared Access Signature based on the Stored Access Policy. If Shared Access Signature is ever compromised, the underlying Stored Access Policy can be deleted to keep the account safe.

## Create Shared Access Signature based on Shared Access Policy
Here is how to create a Shared Access Signature key using Shared Access Policy.
* Go to [Azure Portal](https://portal.azure.com)
* Go to the storage account.
* Go to the container where you want to apply this policy.
* Select **Access Policy** from the menu.

![Access Policy]({{ page.post-images-base-url }}/access-policy.png)

* Create a new Stored Access Policy. Be sure to set the expiration date, and the permissions based on the requirements. Limit the permissions to only the ones that you absolutely need.

![Add SAP]({{ page.post-images-base-url }}/add-sap.png)

![Stored Access Policy]({{ page.post-images-base-url }}/add-policy.png)
* Now go back to the storage account, and select **Storage Explorer (preview)**. 
* Select the Blob where the Shared Access Policy was applied. 
* Right-click and Select **Get Shared Access Signature**.

![Get SAS ]({{ page.post-images-base-url }}/get-shared-access.png)

* From the Access Policy dropdown select the Stored Access Policy configured earlier.

![Create SAS]({{ page.post-images-base-url }}/create-sas.png)

Keep the Shared Access Signature secure and use Azure Vault or other similar services, to limit the exposure even further.

## Additional Material
* [Shared Access Signature](https://docs.microsoft.com/en-us/azure/storage/common/storage-sas-overview)
* [Stored Access Policy](https://docs.microsoft.com/en-us/rest/api/storageservices/define-stored-access-policy)
* [Security Recommendations for Blob Storage](https://docs.microsoft.com/en-us/azure/storage/blobs/security-recommendations)



