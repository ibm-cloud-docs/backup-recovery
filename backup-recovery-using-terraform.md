---

copyright:
  years: 2024
lastupdated: "2026-03-17"

keywords: backup recovery, terraform, guide

subcollection: backup-recovery

---
{:new_window: target="_blank"}
{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}
{:http: .ph data-hd-programlang='http'}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:go: .ph data-hd-programlang='go'}
{:faq: data-hd-content-type='faq'}
{:support: data-reuse='support'}

# Using Terraform
{: #terraform-using}

## Getting the Terraform provider
{: #terraform-getting-provider}

### Configure Provider
{: #terraform-configure}

Update your provider configuration: In provider.tf specify the latest version of IBM terraform provider. Also configure the api key directly in provider.tf, otherwise set it as an environment variable.

```sh
terraform {
  required_providers {
    ibm = {
      source  = "ibm-cloud/ibm"
      version="<latest-version>" 
    }
  }
}

provider "ibm" {
    ibmcloud_api_key = "<api-key>"
    visibility = "public"
    region = "us-east"
    # endpoints_file_path = "./endpoints.json"
}
```

### Set the endpoint for Backup Recovery
{: #terraform-set-endpoint}

You can configure the Backup Recovery endpoint in one of the following ways:

#### Option 1: Set the environment variable
{: #terraform-set-environ-var}

Set the endpoint directly in your terminal:

```sh
BACKUP_RECOVERY_ENDPOINT=https://brs-stage-us-south-01.backup-recovery.cloud.ibm.com/v2
```

#### Option 2: Use an endpoint JSON file
{: #terraform-set-endpoint-json}

Specify the endpoint in an endpoint JSON file and reference this file in your Terraform configuration or ensure the provider reads it accordingly:

```sh
{
    "BACKUP_RECOVERY_ENDPOINT" : {
        "public" : {
            "us-south": "https://brs-stage-us-south-01.backup-recovery.cloud.ibm.com/v2"
        }
    }
}
```

#### Option 3: Use resource-level parameters (overrides Options 1 and 2)
{: #terraform-set-required-params}

The following parameters can be set on all Backup Recovery related resources and datasources. If `instance_id` and `region` are provided together, the provider constructs the endpoint URL using them, which **overrides** any value set through the `BACKUP_RECOVERY_ENDPOINT` environment variable or the `endpoints.json` file.

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `endpoint_type` | String | Optional | Backup Recovery endpoint type. By default set to `"public"`. |
| `instance_id` | String | Optional | Backup Recovery instance ID. If provided along with `region`, overrides the environment variable and `endpoints.json` file. |
| `region` | String | Optional | Backup Recovery region. If provided along with `instance_id`, overrides the environment variable and `endpoints.json` file. |

### Initialize Terraform
{: #terraform-initialize}

Run the following command to install the latest provider version specified in provider.tf

```sh
terraform init --upgrade
```

## Code Examples
{: #terraform-code-examples}

### Create a connection
{: #terraform-create-connection}

```sh
resource "ibm_backup_recovery_data_source_connection" "terra_connection_instance" {
	x_ibm_tenant_id = "79mle1bk3m/"
	connection_name = "test-conn-110"
	endpoint_type   = "public"
	instance_id     = "<instance-id>"
	region          = "us-south"
}
```

### Register a data source
{: #terraform-register-data-source}

```sh
resource "ibm_backup_recovery_source_registration" "terra_source_registration_instance" {
    x_ibm_tenant_id = "79mle1bk3m/"
    environment = "kPhysical"
    connection_id = "7754198299738915743"
    endpoint_type   = "public"
    instance_id     = "<instance-id>"
    region          = "us-south"
    physical_params {
        endpoint = "172.26.1.12"
        host_type = "kLinux"
        physical_type = "kHost"
    }
}
```

### List registered data source
{: #terraform-list-register-data-source}

```sh
data "ibm_backup_recovery_source_registration" "backup_recovery_source_registration_instance"{
	x_ibm_tenant_id = "79mle1bk3m/"
	source_registration_id = 217
	endpoint_type   = "public"
	instance_id     = "<instance-id>"
	region          = "us-south"
}
```

### Create a protection policy
{: #terraform-create-protection-policy}

```sh
resource "ibm_backup_recovery_protection_policy" "baas_protection_policy_instance" {
    x_ibm_tenant_id = "79mle1bk3m/"
    endpoint_type   = "public"
    instance_id     = "<instance-id>"
    region          = "us-south"
	name = "terra-test-policy-1"
	backup_policy {
		regular {
			incremental{
				schedule{
					day_schedule {
						frequency = 2
					}
					unit = "Days"
			    }
			}
			retention {
				duration = 2
				unit = "Weeks"
			}
			primary_backup_target {
				use_default_backup_target = true
			}
		}
	}
	retry_options {
		retries = 0
		retry_interval_mins = 10
	}
}
```

### List policies
{: #terraform-create-list-policies}

```sh
data "ibm_backup_recovery_protection_policies" "backup_recovery_protection_policies_instance" {
		x_ibm_tenant_id = "79mle1bk3m/"
		ids = ["2783038300930052:1729529537348:8635"]
		endpoint_type   = "public"
		instance_id     = "<instance-id>"
		region          = "us-south"
	}
```

### Create a protection group
{: #terraform-create-protection-group}

```sh
resource "ibm_backup_recovery_protection_group" "baas_protection_group_instance" {
	x_ibm_tenant_id = "79mle1bk3m/"
	endpoint_type   = "public"
	instance_id     = "<instance-id>"
	region          = "us-south"
	policy_id = ibm_backup_recovery_protection_policy.baas_protection_policy_instance.id
	name = "terra-test-group-1"
	environment = "kPhysical"
	physical_params {
		protection_type = "kFile"
		file_protection_type_params {
            objects {
                id = 217
                file_paths{
                    included_path = "/data1/data2"
                }
            }
	    }
	}
}
```

### List protection group
{: #terraform-list-protection-group}

```sh
data "ibm_backup_recovery_protection_group" "backup_recovery_protection_group_instance" {
		x_ibm_tenant_id = "79mle1bk3m/"
		protection_group_id = "2783038300930052:1729529537348:8639"
		endpoint_type   = "public"
		instance_id     = "<instance-id>"
		region          = "us-south"
}
```

### Create protection group run
{: #terraform-create-protection-group-run}

```sh
resource "ibm_backup_recovery_protection_group_run_request" "my_name_4" {
    x_ibm_tenant_id = "79mle1bk3m/"
    group_id = "5901263190628181:1725393921826:9630"
    run_type = "kRegular"
    endpoint_type   = "public"
    instance_id     = "<instance-id>"
    region          = "us-south"
}
```

### Create a recovery
{: #terraform-create-recovery}

```sh
resource "ibm_backup_recovery" "backup_recovery_instance" {
		x_ibm_tenant_id = "79mle1bk3m/"
		snapshot_environment = "kPhysical"
		endpoint_type   = "public"
		instance_id     = "<instance-id>"
		region          = "us-south"
		name = "terra-recovery-1"
		physical_params {
		  recovery_action = "RecoverFiles"
		  objects {
			snapshot_id = "eyJhX2NsdXN0ZXJJZCI6Mjc4MzAzODMwMDkzMDA1MiwiYl9jbHVzdGVySW5jYXJuYXRpb25JZCI6MTcyOTUyOTUzNzM0OCwiY19qb2JJZCI6ODYyOCwiZV9qb2JJbnN0YW5jZUlkIjo4NjI5LCJmX3J1blN0YXJ0VGltZVVzZWNzIjoxNzMxNDA4NzUzOTI3OTI3LCJnX29iamVjdElkIjoyMTcsImhfdmF1bHRJZCI6Nzk5MDYyNDAzfQ=="
		  }
		  recover_file_and_folder_params {
			 target_environment = "kPhysical"
			 files_and_folders {
			   absolute_path = "/data/"
			 }
			 physical_target_params {
			   recover_target {
				 id = 217
			   }
			   restore_entity_type = "kRegular"
			   alternate_restore_directory = "/data/"
			 }
		  }
		}
}
```

## Using Terraform IBM Modules (TIM)
{: #terraform-ibm-modules}

As an alternative to writing Terraform configurations from scratch, you can use [Terraform IBM Modules (TIM)](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-about-tim), which deliver enterprise-grade, production-ready infrastructure components that streamline your {{site.data.keyword.baas_full_notm}} service deployment. These modules are built with security best practices, designed for scalability, and help you deploy faster while maintaining consistency across your environments.

The following Terraform IBM Modules are available for {{site.data.keyword.baas_full_notm}} service:

- **[IBM Backup and Recovery Service (BRS) Module](https://registry.terraform.io/modules/terraform-ibm-modules/backup-recovery/ibm/latest){: external}** - Provides comprehensive infrastructure automation for deploying and managing {{site.data.keyword.baas_full_notm}} service instances, including service provisioning, IAM policies, and resource configuration.

- **[IBM Backup & Recovery for IKS/ROKS with Data Source Connector](https://registry.terraform.io/modules/terraform-ibm-modules/iks-ocp-backup-recovery/ibm/latest){: external}** - Simplifies the deployment of backup and recovery solutions for IBM Cloud Kubernetes Service (IKS) and Red Hat OpenShift on IBM Cloud (ROKS) clusters, including automated Data Source Connector setup.

For more information about Terraform IBM Modules, visit the [Terraform IBM Modules registry](https://registry.terraform.io/namespaces/terraform-ibm-modules){: external}.
