---

copyright:
  years: 2025
lastupdated: "2025-12-12"

keywords: certification, sap hana, authentication, deploy

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# SAP HANA certificate authentication
{: #sap_hana_certificate_authentication}

The SAP HANA Certification provides the necessary authentication and also verifies whether the SAP HANA database, which performs backups to the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster, is a trusted entity. This certification provides the capability to set verification which allows only trusted sources to write to the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster through the SpanFS.

## Create an SAP HANA authentication certificate
{: #create_an_sap_hana_authentication_certificate}

This section lists the steps for creating an SAP HANA authentication certificate.

1. Start the {{site.data.keyword.cloud_notm}} Backup and Recovery CLI locally and log in to the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster using the ssh command with the `support` user account and specify the IP address. <!--For more information, see [Using the {{site.data.keyword.cloud_notm}} Backup and Recovery CLI](https://docs.cohesity.com/6_8_1/Web/PDFs/681_PlatformCLI.pdf){: external} and [List of {{site.data.keyword.cloud_notm}} Backup and Recovery CLI Commands](../../CLI_Reference/Objects.htm).

    ```sh
        ssh support@198.51.100.12
    ```

1. If prompted, enter the password.

    ```sh
        \[support@198.51.100.12's password:
    ```

    The {{site.data.keyword.cloud_notm}} Backup and Recovery\_shell prompt is displayed.

1. Log in to the CLI:

    ```sh
        IBM Cloud Backup and Recovery shell# iris\_cli
    ```

    The admin prompt is displayed.

1. Optionally, run the help command for a list of CLI commands.

    ```sh
        admin@198.51.100.12> help
    ```

1. Run the cluster gen-src-cert command to generate a new SAP HANA certificate.

    ```sh
        admin@198.51.100.12 cluster gen-src-cert cert-name="newdemocert" cert-validity=100 host-name="10.2.146.175" host-target="/tmp"
        Success: Certificate "newdemocert" generation and deployment successful.
    ```

    | Parameter name | Description |
    | --- | --- |
    | cert-name | Name of the certificate file. Its naming format is ${cert-name}\_<random\_number>.cfg. |
    | cert-validity | The validity of the source certificate generated. The time is displayed in days. |
    | host-name | The IP address/host name of the target system where you want to move the certificate. This is a mandatory parameter because the certificate generation process needs details of the host machine to generate public and private keys. |
    | host-target | The file directory path on the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster where the certificate is generated.
    {: caption="Parameter name" caption-side="bottom"}

    After the SAP HANA certificate is generated on the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster, manually copy the certificate to your SAP HANA machine.

1. Update the following parameters in the parameter file.


    | Parameter name | Description |
    | --- | --- |
    | cluster\_port | Port number on {{site.data.keyword.cloud_notm}} Backup and Recovery cluster nodes to which the BACKINT executable connects.<br><br>Use the port 11117 for secure connection (certificate config path should be a valid path).<br><br>Use the port 11113 for an insecure connection (certificate config path should be an empty string).<br><br>Contact {{site.data.keyword.cloud_notm}} Backup and Recovery Support to enable these ports. |
    | certificate\_config\_path | The path to which the certificate is generated and deployed from the {{site.data.keyword.cloud_notm}} Backup and Recovery Dashboard to the SAP HANA Node.<br><br>You can use this path as the default config path: `/usr/sap/ORC/home/install/newdemocert.cfg` |
    {: caption="Parameter name" caption-side="bottom"}

1. Run the `list-source-certificates` command to check all the global certificates deployed from the cluster.


### Deploy an SAP HANA certificate in a multi-node setup
{: #deploy_an_sap_hana_certificate_in_a_multi-node_setup}

This section lists the steps to deploy an SAP HANA authentication certificate in a multi-node SAP HANA setup.

1. Generate and deploy the SAP HANA authentication certificate to any one of the nodes in a multi-node SAP HANA system using the instructions provided in the previous section.

1. Use SCP to securely transfer the certificate to a target location on all the remaining (N-1) nodes.

    ```sh
        scp <certificate\_config\_path> ip\_address:<certificate\_config\_path>
    ```

    Example

    ```sh
        scp /usr/sap/ORC/home/install/cert.cfg 10.2.146.72: /usr/sap/ORC/home/install/cert.cfg
    ```

1. Change the `cluster_port parameter` value to 11117 and update the `certificate_config_path` parameter value to the correct path of the certificate in the respective paramfile of each of the N nodes.

