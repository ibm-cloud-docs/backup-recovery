---

copyright:
  years: 2024
lastupdated: "2025-07-30"

keywords:

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

_Name your file `architecture-workload-isolation.md` and include it in the **How to** nav group in the **Enhancing security** topic group in your `toc.yaml` file. See https://test.cloud.ibm.com/docs-internal/writing?topic=writing-security-content-guidance_

_Make sure that you use the following title for your topic._

# Learning about backup-recovery architecture and workload isolation
{: #compute-isolation}



_The short description should be a single, concise paragraph that contains one or two sentences and no more than 50 words. Summarize your offering's architecture and support for isolating one tenants' workload from another._

Review the following sample architecture for _servicename_, and learn more about different isolation levels so that you can choose the solution that best meets the requirements of the workloads that you want to run in the cloud.
{: shortdesc}

## backup-recovery architecture
{: #architecture}

_Review the following example: https://cloud.ibm.com/docs/containers?topic=containers-service-arch. Review the System Architecture Guide: https://pages.github.ibm.com/CloudEngineering/system_architecture/services/multitenancy.html. Provide an architectural overview diagram identifying what runs within the IBM Service Account and what runs in the customer's account._

## backup-recovery workload isolation
{: #workload-isolation}

_Document how customer workloads are isolated from each other by plan. Do customer workloads run within the customer account?  Are customer workloads isolated within Kubernetes namespaces? Do customer workloads run on dedicated compute? Check out the example from Kubernetes: https://cloud.ibm.com/docs/containers?topic=containers-service-arch#worker-components_
