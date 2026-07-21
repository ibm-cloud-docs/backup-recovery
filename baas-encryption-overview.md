---

copyright:
  years: 2024, 2026
lastupdated: "2026-07-21"

keywords: encryption, security, sse-c, key protect, {{site.data.keyword.hscrypto}}

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Encrypting your data
{: #encrypting_your_data}

{{site.data.keyword.cos_full}} provides several options to encrypt your data.
{: shortdesc}

By default, all objects that are stored in {{site.data.keyword.baas_full_notm}} are encrypted by using randomly generated keys and an all-or-nothing-transform (AONT). AONT is an internal data protection technique that prevents partial data reconstruction. While this default encryption model provides at-rest security, some workloads need full control over the data encryption keys used. You can manage your keys manually on a per-object basis by providing your own encryption keys - referred to as [Server-Side Encryption with Customer-Provided Keys (SSE-C)](/docs/cloud-object-storage?topic=cloud-object-storage-sse-c).

With {{site.data.keyword.cos_short}} you also have a choice to use our integration capabilities with {{site.data.keyword.cloud}} Key Management Services like {{site.data.keyword.keymanagementservicelong}} and {{site.data.keyword.hscrypto}}. Depending on the security requirements, you can decide whether to use IBM Key Protect or IBM {{site.data.keyword.hscrypto}} for your IBM Cloud Object Storage buckets.

## Choosing the Right Encryption Model
{: #choose-right-encryption}

[{{site.data.keyword.keymanagementservicefull}}](/docs/key-protect?topic=key-protect-about) helps you provision encrypted keys for apps across {{site.data.keyword.cloud}} services. As you manage the lifecycle of your keys, you can benefit from knowing that your keys are secured by FIPS 140-2 Level 3 certified cloud-based hardware security modules (HSMs) that protect against the theft of information.

[{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-overview) is a single-tenant, dedicated HSM that is controlled by you. The service is built on FIPS 140-2 Level 4-certified hardware, the highest offered by any cloud provider in the industry.

| Feature | {{site.data.keyword.keymanagementservicefull}} | {{site.data.keyword.hscrypto}} |
|--------|-------------------------------|-------------------------------|
| Type of Service | Multi-tenant, cloud-based key management service | Single-tenant, dedicated hardware security module (HSM) |
| Purpose | Provision encrypted keys for apps across IBM Cloud services | Provide customer-controlled dedicated HSMs for maximum isolation |
| Hardware Security Level | FIPS 140-2 Level 3 certified HSMs | FIPS 140-2 Level 4 certified hardware (highest in cloud industry) |
| Isolation | Shared (multi-tenant) | Fully isolated, single-tenant |
| Control Model | IBM manages and operates the HSM infrastructure | Customer has full operational control |
| Security Focus | Strong key security for general cloud workloads | Highest assurance for highly sensitive workloads |
| Protection Against Theft | Protected by cloud-based HSMs with tamper resistance | Protected by advanced Level 4-certified tamper-detecting hardware |
{: caption="Encryption Model" caption-side="bottom"}

To use IBM Key Protect or IBM {{site.data.keyword.hscrypto}}, see [Server-Side Encryption with IBM Key Protect (SSE-KP)](/docs/backup-recovery?topic=backup-recovery-kp) and [Server-Side Encryption with Hyper Protect Crypto Services](/docs/backup-recovery?topic=backup-recovery-hpcs).
