---

copyright:
  years: 2024
lastupdated: "2025-07-30"

keywords:

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

_Name your file `data-security.md` and include it in the **How to** nav group in the **Enhancing security** topic group in your `toc.yaml` file. See https://test.cloud.ibm.com/docs-internal/writing?topic=writing-security-content-guidance_

_Make sure that you use the following title for your topic._

# Securing your data in _servicename_
{: #mng-data}



_The short description should be a single, concise paragraph that contains one or two sentences and no more than 50 words. Summarize your offering's support for customer-manageds key encryption and processes for handling and deleting data. See the following example for a service that supports Key Protect for BYOK and Hyper Protect Crypto Services for KYOK._

To ensure that you can securely manage your data when you use _servicename_, it is important to know exactly what data is stored and encrypted and how you can delete any stored data. Depending on your security requirements, you can encrypt data with customer-managed keys by integrating with {{site.data.keyword.cloud_notm}} key management services such as {{site.data.keyword.keymanagementserviceshort}}, which supports the bring your own key (BYOK) method, or {{site.data.keyword.hscrypto}}, which supports the keep your own key (KYOK) method.
{: shortdesc}



## How your data is stored and encrypted in _servicename_
{: #data-storage}

_Document how your offering stores and encrypts user data as it relates to user activities. What data is encrypted and what is not? How is data encrypted?_

_If you are using separate keys to encrypt each customer, provide that information here._

_Do not include any details that would give malicious persons an advantage to compromising your offering environment._


## Protecting your sensitive data in _servicename_
{: #data-encryption}

_Document if your services supports Key Protect and/or Hyper Protect Crypto Services for customer-managed keys (whether it's using BYOK or KYOK or both methods). Explain what data is encrypted with customer-managed keys and what data is not. The following is example text from a Watson service that uses Key Protect for BYOK:_

You can add a higher level of encryption protection and control to your data at rest (when it is stored) and data in motion (when it is transported) by enabling integration with {{site.data.keyword.keymanagementservicefull}}.

The data that you store in {{site.data.keyword.cloud_notm}} is encrypted at rest by using a randomly generated key. If you need to control the encryption keys, you can integrate {{site.data.keyword.keymanagementserviceshort}}. This process is commonly referred to as Bring your own keys (BYOK). With {{site.data.keyword.keymanagementserviceshort}} you can create, import, and manage encryption keys. You can assign access policies to the keys, assign users or service IDs to the keys, or give the key access only to a specific service. The first 20 keys are free.




### About customer-managed keys
{: #about-encryption}

_Include any information specific to how your service supports BYOK or KYOK with the respective integrating service. As an example, see the following sample. Most services should be able to reuse these paragraphs as-is because this is how encryption works with Key Protect on IBM Cloud and Hyper Protect Crypto Services_:

_servicename_ uses [envelope encryption](#x9860393){: term} to implement customer-managed keys. Envelope encryption describes encrypting one encryption key with another encryption key. The key used to encrypt the actual data is known as a [data encryption key (DEK)](#x4791827){: term}. The DEK itself is never stored but is wrapped by a second key that is known as the key encryption key (KEK) to create a wrapped DEK. To decrypt data, the wrapped DEK is unwrapped to get the DEK. This process is possible only by accessing the KEK, which in this case is your root key that is stored in {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}.

You own the KEK, which you create as a root key in the {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}} service. The _servicename_ service never sees the root (KEK) key. Its storage, management, and use to wrap and unwrap the DEK is performed entirely within the key management service. If you disable or delete the key, the data can no longer be decrypted.

{{site.data.keyword.keymanagementserviceshort}} keys are secured by FIPS 140-2 Level 3 certified cloud-based [hardware security modules (HSMs)](#x6704988){: term}. For more information, see [Bringing your encryption keys to the cloud](/docs/key-protect?topic=key-protect-importing-keys).

{{site.data.keyword.hscrypto}} is a single-tenant, dedicated hardware security module that is controlled by you. {{site.data.keyword.hscrypto}} features KYOK encryption capabilities backed by FIPS 140-2 Level 4-certified hardware, which is the highest offered by any cloud provider in the industry. For more information, see [Getting started with {{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-get-started).


### Enabling customer-managed keys for _servicename_
{: #using-byok}

_Document the steps a customer must take to get their keys or bring their keys to encrypt their data with your service. As an example, see: https://test.cloud.ibm.com/docs/services/watson?topic=watson-keyservice#keyservice-steps and https://test.cloud.ibm.com/docs/EventStreams?topic=EventStreams-managing_encryption#enabling_encryption and https://test.cloud.ibm.com/docs/appid?topic=appid-mng-data#enable-customer-keys-kp_

### Working with customer-managed keys for _servicename_
{: #working-with-keys}

_Document any common scenarios that can help your customers know how to use their keys to protect data within the service. As an example, see: https://test.cloud.ibm.com/docs/services/watson?topic=watson-keyservice#keyservice-using and https://test.cloud.ibm.com/docs/EventStreams?topic=EventStreams-managing_encryption#using_encryption. Document scenarios such as key rotation and temporarily preventing and restoring access. For any lifecycle events that your service does not support, such as key rotation, you must document it here._

_Add a note like the following example, so that customers know they can audit the lifecycle of these keys by using the events from the key management service. Use what applies to your use case._

You can use {{site.data.keyword.cloudaccesstraillong}} to audit the lifecycle events of your keys, such as creating a key, deleting a key, rotating a key, and more. For more information, see [{{site.data.keyword.cloudaccesstraillong_notm}} events for {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-at-events) and [{{site.data.keyword.cloudaccesstraillong_notm}} events for {{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-at-events).
{: tip}

## Deleting your data in _servicename_
{: #data-delete}

_Document how users can delete their data within the service._

_If applicable, add H3s in this section to tailor the information to particular types of data. For example, you might have a "Deleting keys" section and a "Deleting a database" section._

### Deleting _servicename_ instances
{: #service-delete}

_Include information about whether deleting the service fully erases all data. If deleting the service doesn't remove all personal data, include information about how users can completely delete their data._

_Information about how long services keep data after instances are deleted is covered in the service description. Include the following reference for users to find their data retention period._

The _servicename_ data retention policy describes how long your data is stored after you delete the service. The data retention policy is included in the _service-name_ service description, which you can find in the [{{site.data.keyword.cloud_notm}} Terms and Notices](/docs/overview?topic=overview-terms).

### Restoring deleted data for _servicename_
{: #data-restore}

_If users can restore deleted data within your service, include this optional section and the task that users can complete to do so._

_Important: Don't include information about restoring your resource via the reclamation controller because it's available only on a limited basis._
