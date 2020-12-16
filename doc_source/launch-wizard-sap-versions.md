# Supported versions for SAP deployments<a name="launch-wizard-sap-versions"></a>

**Topics**
+ [Operating system](#launch-wizard-sap-ascs-support-os)
+ [Application](#launch-wizard-sap-versions-application)
+ [Application software and deployment patterns](#launch-wizard-sap-versions-application-install)

## Supported operating system versions for SAP deployments<a name="launch-wizard-sap-ascs-support-os"></a>

The following table provides details for the operating systems supported by Launch Wizard for SAP deployments\.


| Operating system version | Single\-node deployment | ASCS | ERS | PAS | SAP HANA database | SAP HANA database in HA cluster | 
| --- | --- | --- | --- | --- | --- | --- | 
| Red\-Hat\-Enterprise\-Linux\-7\.6\-For\-SAP\-HA\-US\-HVM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | 
| Red\-Hat\-Enterprise\-Linux\-8\.1\-For\-SAP\-HA\-US\-HVM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | 
| SuSE\-Linux\-15\-HVM | ✓ |  |  | ✓ | ✓ |  | 
| SuSE\-Linux\-15\-For\-SAP\-HVM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | 
| SuSE\-Linux\-15\-For\-SAP\-BYOS\-HVM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | 
| SuSE\-Linux\-12\-SP4\-HVM | ✓ |  |  | ✓ | ✓ |  | 
| SuSE\-Linux\-12\-SP4\-For\-SAP\-HVM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | 
| SuSE\-Linux\-12\-SP4\-For\-SAP\-BYOS\-HVM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | 
| SuSE\-Linux\-12\-SP5\-HVM | ✓ |  |  | ✓ | ✓ |  | 
| SuSE\-Linux\-12\-SP5\-For\-SAP\-HVM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | 
| SuSE\-Linux\-12\-SP5\-For\-SAP\-BYOS\-HVM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | 
| SuSE\-Linux\-15\-SP1\-HVM | ✓ |  |  | ✓ | ✓ |  | 
| SuSE\-Linux\-15\-SP1\-For\-SAP\-HVM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | 
| SuSE\-Linux\-15\-SP1\-For\-SAP\-BYOS\-HVM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | 

For Additional Application Server \(AAS\), the operating system is inherited from the operating system selected for the PAS server\. 

## Supported application versions for SAP deployments<a name="launch-wizard-sap-versions-application"></a>

The following table provides details for the application versions supported by Launch Wizard for SAP deployments\.


| Application name | Supported versions | 
| --- | --- | 
| SAP NetWeaver on HANA | 7\.5 SPS02 and 7\.5 SPS00 | 
| SAP BW/4HANA | 2\.0 | 
| SAP S/4HANA | 1909 and 2020 | 

## Supported application software installation versions and deployment patterns<a name="launch-wizard-sap-versions-application-install"></a>

The following table provides details about the supported application software installation versions for the deployment patterns supported by Launch Wizard for SAP\.


| Application and software version | OS | HANA DB versions | Single instance deployment | Distributed instance deployment with HANA scale\-up | Distributed instance deployment with HANA scale\-out | High availability deployment\* | 
| --- | --- | --- | --- | --- | --- | --- | 
| Netweaver 7\.5 SPS02 | SLES 15 SP1 |  HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04  |  Yes  | Yes | No | No | 
| Netweaver 7\.5 SPS02 | SLES 15 for SAP SP1 |  HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04  | Yes | Yes | No | Yes | 
| Netweaver 7\.5 SPS02 | SLES 15 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | No | No | 
| Netweaver 7\.5 SPS02 | SLES 15 for SAP | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | No | Yes | 
| Netweaver 7\.5 SPS02 | SLES 12 SP4 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | No | No | 
| Netweaver 7\.5 SPS02 | SLES 12 for SAP SP4 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | No | Yes | 
| Netweaver 7\.5 SPS02 | SLES 12 SP5 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | No | No | 
| Netweaver 7\.5 SPS02 | SLES 12 for SAP SP5 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | No | Yes | 
| Netweaver 7\.5 SPS02 | RHEL 7\.6 for SAP | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | No | Yes | 
| Netweaver 7\.5 SPS0 | SLES 15 SP1 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | No | No | 
| Netweaver 7\.5 SPS0 | SLES 15 for SAP SP1 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | No | Yes | 
| Netweaver 7\.5 SPS0 | SLES 15 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | No | No | 
| Netweaver 7\.5 SPS0 | SLES 15 for SAP | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | No | Yes | 
| Netweaver 7\.5 SPS0 | SLES 12 SP4 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | No | No | 
| Netweaver 7\.5 SPS0 | SLES 12 for SAP SP4 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | No | Yes | 
| Netweaver 7\.5 SPS0 | SLES 12 SP5 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | No | No | 
| Netweaver 7\.5 SPS0 | SLES 12 for SAP SP5 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | No | Yes | 
| Netweaver 7\.5 SPS0 | RHEL 7\.6 for SAP | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | No | Yes | 
| S4HANA 2020 & 1909 | SLES 15 SP1 | HANA 2\.0 SPS 05 | Yes | Yes | No | No | 
| S4HANA 2020 & 1909 | SLES 15 for SAP SP1 | HANA 2\.0 SPS 05 | Yes | Yes | No | Yes | 
| S4HANA 2020 & 1909 | SLES 15 | HANA 2\.0 SPS 05 | Yes | Yes | No | No | 
| S4HANA 2020 & 1909 | SLES 15 for SAP | HANA 2\.0 SPS 05 | Yes | Yes | No | Yes | 
| S4HANA 2020 & 1909 | SLES 12 SP5 | HANA 2\.0 SPS 05 | Yes | Yes | No | No | 
| S4HANA 2020 & 1909 | SLES 12 for SAP SP5 | HANA 2\.0 SPS 05 | Yes | Yes | No | Yes | 
| S4HANA 2020 & 1909 | RHEL 7\.6 for SAP | HANA 2\.0 SPS 05 | Yes | Yes | No | Yes | 
| BW4HANA 2\.0 | SLES 15 SP1 | HANA 2\.0 SPS 05 | Yes | Yes | No | No | 
| BW4HANA 2\.0 | SLES 15 for SAP SP1 | HANA 2\.0 SPS 05 | Yes | Yes | No | Yes | 
| BW4HANA 2\.0 | SLES 15 | HANA 2\.0 SPS 05 | Yes | Yes | No | No | 
| BW4HANA 2\.0 | SLES 15 for SAP | HANA 2\.0 SPS 05 | Yes | Yes | No | Yes | 
| BW4HANA 2\.0 | SLES 12 SP5 | HANA 2\.0 SPS 05 | Yes | Yes | No | No | 
| BW4HANA 2\.0 | SLES 12 for SAP SP5 | HANA 2\.0 SPS 05 | Yes | Yes | No | Yes | 
| BW4HANA 2\.0 | RHEL 7\.6 for SAP | HANA 2\.0 SPS 05 | Yes | Yes | No | Yes | 

**Note**  
\*If you choose to deploy SAP application software for high availability deployments, Launch Wizard deploys the software, but does not configure the SUSE/RHEL HA cluster\. You must configure the cluster after the deployment\.
