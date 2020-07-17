---
layout: post
title: Securing Azure Storage with Stored Access Policy.
meta-description: Securing Azure Storage with Stored Access Policy (SAP) and Shared Access Token (SAS).
author: Haris Ahmad
published-date: 2020-07-17
categories: [Azure]
tags: [Azure, Azure Storage, Security]
post-images-base-url: /assets/img/20200717-Secure-AzureSearch
share-img: /assets/img/common/mobile-development-01.png
---

Secure your Shared Access Signature(SAS) in Azure Storage  by using Stored Access Policy (SAP). A Stored Access Policy defines access rights and restrictions. This policy can then be used to create Shared Access Signatures that can be used by clients.

An inherent benefit of using Stored Access Policy is that the Shared Access Signature can be revoked anytime by deleting the underlying Stored Access Policy. This is important, because there may be reasons to revoke the Shared Acess Signature. Without using Stored Acess Policy, the user may not be able to revoke a given Shared Access Signature, which may have an impact on the security of the Azure Storage.

Stored Access Policy is a much safer option then using Shared Access Keys  at the account level. If the Shared Access Keys at the account level are compromised, then the keys to the storage account may need to be regenerated to invalidate the SAS and keep the storage safe. This may also have an adverse impact on  existing applications that rely on SAS tokens generated with these keys.

A better option in this case would be to create Stored Access Policy for each container, and create SAS based on the SAP. If SAS key is ever compromised, you can delete the assocaited SAP and recreate a new one - and this will only impact the applications using the SAS for this particular container.

Here is how to create a Shared Access Signature key using Shared Access Policy.
* Go to portal.azure.com
* Go to the storage account.
* Go to the container where you want to apply this policy.
* Select **Access Policy** from the menu.
![Access Policy]({{ page.post-images-base-url }}/AccessPolicy.png)
* Create a new Stored Access Policy. Be sure to set the expiration date, and the permissions based on the requirements. Limit the permissions to only the ones that you absolutely need.
![Add SAP]({{ page.post-images-base-url }}/AddSAP.png)
![Stored Access Policy]({{ page.post-images-base-url }}/AddPolicy.png)
* Now go back to the storage account, and select ** Storage Exporer Prevew. 
* Select the Blob where the SAP was applied. 
* Right click and Select ** Get Shared Access Signature**.
![Get SAS ]({{ page.post-images-base-url }}/GetSharedAccess.png)
* From the Access Policy dropdown select the Stored Access Policy configured earlier.
* Select the Start time, Enstime and the permissions and create teh SAS.
![Create SAS]({{ page.post-images-base-url }}/CreateSAS.png)

Keep the SAS secure and use Azure Vault or other similar services, to limit the exposure even further.

##Additional Material
* [Shared Access Signature](https://docs.microsoft.com/en-us/azure/storage/common/storage-sas-overview)
* [Stored Access Policy](https://docs.microsoft.com/en-us/rest/api/storageservices/define-stored-access-policy)
* [Security Recommendations for Blob Storage](https://docs.microsoft.com/en-us/azure/storage/blobs/security-recommendations#:~:text=Microsoft%20recommends%20using%20Azure%20AD,saving%20them%20with%20your%20application)



