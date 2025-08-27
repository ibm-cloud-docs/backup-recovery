---

copyright:
  years: 2024
lastupdated: "2025-08-27"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Network bandwidth and cpu throttling for physical server
{: #network_bandwidth_and_cpu_throttling_for_physical_server}

28 May 2024

You can throttle the network bandwidth and CPU on the Windows and Linux physical server registered with the {{site.data.keyword.baas_full_notm}}. Network throttling optimizes the data transfer rate from the server to the {{site.data.keyword.baas_full_notm}} when a Protection Group is run. CPU throttling optimizes the CPU capacity utilized when a Protection Group with source-side deduplication is run on the server.

## Supported servers
{: #supported_servers}

The following table lists the physical servers supporting networking bandwidth and CPU throttling.


| Operating System | Network Bandwidth Throttling | CPU Throttling |
| --- | --- | --- |
| Windows | Yes | Yes\* |
| Linux | Yes | No  |
{: caption="" caption-side="bottom"}

\* CPU throttling is supported on Windows 2012 R2 and later versions.

The network bandwidth and CPU throttling does not apply to the CBMR.

## Enable network bandwidth and cpu throttling
{: #enable_network_bandwidth_and_cpu_throttling}

To enable network and CPU throttling, you must first register the physical server as a source and then edit the registered source to enable the options for network bandwidth and CPU throttling:

1. Select **Data Protection > Sources**.
2. Navigate the hierarchy to the registered physical server source for which you want to enable network bandwidth and CPU throttling.

    The edit server page appears.

3. Enable the **Throttle Network Bandwidth** option and in the **Throttle to** field, specify the maximum data transfer rate from the server to the {{site.data.keyword.baas_full_notm}}.

    To schedule network bandwidth throttling for specific days and time duration:

    1. Click **Add**.

    2. From the **On** field, select the days during which you want to optimize the network bandwidth.

    3. In the **Start Time** and **End Time** duration fields, specify the time duration for throttling.

    4. In the **Throttle to** field, specify the maximum data transfer rate from the server to the {{site.data.keyword.baas_full_notm}}.

    5. To configure multiple schedules for throttling, click **+** and repeat steps b to d.

        **Example**: If you want to limit the data transfer rate to **100 MB/S** on **Mondays** and **Wednesdays** from **12.00 to 16.00**, then from the **On** field, select **M** and **W**, set the **Start Time** as **12.00**, **End Time** as **16.00**, and in the **Throttle to** field, enter **100** as the throttling rate.

        For the days and duration for which you have not scheduled the network bandwidth throttling, {{site.data.keyword.baas_full_notm}} will take the throttling value specified at the server-level.

4. Enable the **Throttle CPU** option and in the **Throttle to** field, specify the maximum CPU capacity (in percentage) of the server that {{site.data.keyword.baas_full_notm}} backup runs with source-side deduplication enabled can utilize.

    CPU throttling is only applied when the protection run has source-side deduplication enabled.

5. Click **Save**.
