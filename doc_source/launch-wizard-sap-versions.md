# Supported versions for SAP deployments<a name="launch-wizard-sap-versions"></a>

**Topics**
+ [Operating system](#launch-wizard-sap-ascs-support-os)
+ [Application](#launch-wizard-sap-versions-application)
+ [Application software and deployment patterns](#launch-wizard-sap-versions-application-install)

## Supported operating system versions for SAP deployments<a name="launch-wizard-sap-ascs-support-os"></a>

The following table provides details for the operating systems supported by Launch Wizard for SAP deployments\.


| Operating system version | Single\-node deployment | ASCS | ERS | PAS | SAP HANA database | SAP HANA database in HA cluster | 
| --- | --- | --- | --- | --- | --- | --- | 
| Red\-Hat\-Enterprise\-Linux\-8\.2\-For\-SAP\-HA\-US\-HVM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | 
| Red\-Hat\-Enterprise\-Linux\-8\.1\-For\-SAP\-HA\-US\-HVM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | 
| Red\-Hat\-Enterprise\-Linux\-7\.6\-For\-SAP\-HA\-US\-HVM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | 
| SuSE\-Linux\-15\-SP2\-HVM | ✓ |  |  | ✓ | ✓ |  | 
| SuSE\-Linux\-15\-SP2\-For\-SAP\-HVM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | 
| SuSE\-Linux\-15\-SP2\-For\-SAP\-BYOS\-HVM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | 
| SuSE\-Linux\-15\-SP1\-HVM | ✓ |  |  | ✓ | ✓ |  | 
| SuSE\-Linux\-15\-SP1\-For\-SAP\-HVM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | 
| SuSE\-Linux\-15\-SP1\-For\-SAP\-BYOS\-HVM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | 
| SuSE\-Linux\-15\-HVM | ✓ |  |  | ✓ | ✓ |  | 
| SuSE\-Linux\-15\-For\-SAP\-HVM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | 
| SuSE\-Linux\-15\-For\-SAP\-BYOS\-HVM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | 
| SuSE\-Linux\-12\-SP5\-HVM | ✓ |  |  | ✓ | ✓ |  | 
| SuSE\-Linux\-12\-SP5\-For\-SAP\-HVM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | 
| SuSE\-Linux\-12\-SP5\-For\-SAP\-BYOS\-HVM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | 
| SuSE\-Linux\-12\-SP4\-HVM | ✓ |  |  | ✓ | ✓ |  | 
| SuSE\-Linux\-12\-SP4\-For\-SAP\-HVM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | 
| SuSE\-Linux\-12\-SP4\-For\-SAP\-BYOS\-HVM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | 

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
| NetWeaver 7\.5 SPS02 | SLES 15 SP2 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | Yes | No | 
| NetWeaver 7\.5 SPS02 | SLES 15 for SAP SP2 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | Yes | Yes | 
| NetWeaver 7\.5 SPS02 | SLES 15 SP1 |  HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04  |  Yes  | Yes | Yes | No | 
| NetWeaver 7\.5 SPS02 | SLES 15 for SAP SP1 |  HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04  | Yes | Yes | Yes | Yes | 
| NetWeaver 7\.5 SPS02 | SLES 15 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | Yes | No | 
| NetWeaver 7\.5 SPS02 | SLES 15 for SAP | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | Yes | Yes | 
| NetWeaver 7\.5 SPS02 | SLES 12 SP5 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | Yes | No | 
| NetWeaver 7\.5 SPS02 | SLES 12 for SAP SP5 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | Yes | Yes | 
| NetWeaver 7\.5 SPS02 | RHEL 8\.2 for SAP | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | Yes | Yes | 
| NetWeaver 7\.5 SPS02 | RHEL 8\.1 for SAP | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | Yes | Yes | 
| NetWeaver 7\.5 SPS02 | RHEL 7\.6 for SAP | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | Yes | Yes | 
| NetWeaver 7\.5 SPS0 | SLES 15 SP2 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | Yes | No | 
| NetWeaver 7\.5 SPS0 | SLES 15 for SAP SP2 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | Yes | Yes | 
| NetWeaver 7\.5 SPS0 | SLES 15 SP1 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | Yes | No | 
| NetWeaver 7\.5 SPS0 | SLES 15 for SAP SP1 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | Yes | Yes | 
| NetWeaver 7\.5 SPS0 | SLES 15 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | Yes | No | 
| NetWeaver 7\.5 SPS0 | SLES 15 for SAP | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | Yes | Yes | 
| NetWeaver 7\.5 SPS0 | SLES 12 SP5 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | Yes | No | 
| NetWeaver 7\.5 SPS0 | SLES 12 for SAP SP5 | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | Yes | Yes | 
| NetWeaver 7\.5 SPS0 | RHEL 8\.2 for SAP | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | Yes | Yes | 
| NetWeaver 7\.5 SPS0 | RHEL 8\.1 for SAP | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | Yes | Yes | 
| NetWeaver 7\.5 SPS0 | RHEL 7\.6 for SAP | HANA 2\.0 SPS 05 & HANA 2\.0 SPS 04 | Yes | Yes | Yes | Yes | 
| S4HANA 2020 & 1909 | SLES 15 SP2 | HANA 2\.0 SPS 05 | Yes | Yes | Yes | No | 
| S4HANA 2020 & 1909 | SLES 15 for SAP SP2 | HANA 2\.0 SPS 05 | Yes | Yes | Yes | Yes | 
| S4HANA 2020 & 1909 | SLES 15 SP1 | HANA 2\.0 SPS 05 | Yes | Yes | Yes | No | 
| S4HANA 2020 & 1909 | SLES 15 for SAP SP1 | HANA 2\.0 SPS 05 | Yes | Yes | Yes | Yes | 
| S4HANA 2020 & 1909 | SLES 15 | HANA 2\.0 SPS 05 | Yes | Yes | Yes | No | 
| S4HANA 2020 & 1909 | SLES 15 for SAP | HANA 2\.0 SPS 05 | Yes | Yes | Yes | Yes | 
| S4HANA 2020 & 1909 | SLES 12 SP5 | HANA 2\.0 SPS 05 | Yes | Yes | Yes | No | 
| S4HANA 2020 & 1909 | SLES 12 for SAP SP5 | HANA 2\.0 SPS 05 | Yes | Yes | Yes | Yes | 
| S4HANA 2020 & 1909 | RHEL 8\.2 for SAP | HANA 2\.0 SPS 05 | Yes | Yes | Yes | Yes | 
| S4HANA 2020 & 1909 | RHEL 8\.1 for SAP | HANA 2\.0 SPS 05 | Yes | Yes | Yes | Yes | 
| S4HANA 2020 & 1909 | RHEL 7\.6 for SAP | HANA 2\.0 SPS 05 | Yes | Yes | Yes | Yes | 
| BW4HANA 2\.0 | SLES 15 SP2 | HANA 2\.0 SPS 05 | Yes | Yes | Yes | No | 
| BW4HANA 2\.0 | SLES 15 for SAP SP2 | HANA 2\.0 SPS 05 | Yes | Yes | Yes | Yes | 
| BW4HANA 2\.0 | SLES 15 SP1 | HANA 2\.0 SPS 05 | Yes | Yes | Yes | No | 
| BW4HANA 2\.0 | SLES 15 for SAP SP1 | HANA 2\.0 SPS 05 | Yes | Yes | Yes | Yes | 
| BW4HANA 2\.0 | SLES 15 | HANA 2\.0 SPS 05 | Yes | Yes | Yes | No | 
| BW4HANA 2\.0 | SLES 15 for SAP | HANA 2\.0 SPS 05 | Yes | Yes | Yes | Yes | 
| BW4HANA 2\.0 | SLES 12 SP5 | HANA 2\.0 SPS 05 | Yes | Yes | Yes | No | 
| BW4HANA 2\.0 | SLES 12 for SAP SP5 | HANA 2\.0 SPS 05 | Yes | Yes | Yes | Yes | 
| BW4HANA 2\.0 | RHEL 8\.2 for SAP | HANA 2\.0 SPS 05 | Yes | Yes | Yes | Yes | 
| BW4HANA 2\.0 | RHEL 8\.1 for SAP | HANA 2\.0 SPS 05 | Yes | Yes | Yes | Yes | 
| BW4HANA 2\.0 | RHEL 7\.6 for SAP | HANA 2\.0 SPS 05 | Yes | Yes | Yes | Yes | 