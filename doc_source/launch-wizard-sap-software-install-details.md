# Make SAP application software available for AWS Launch Wizard to deploy SAP<a name="launch-wizard-sap-software-install-details"></a>

This section describes steps to upload the SAP application software to Amazon S3 to make it available for Launch Wizard to deploy SAP\.

AWS Launch Wizard supports the following software versions\. To install a software version, you must provide the SAP software files to Launch Wizard by downloading them from the [SAP Support Portal](https://support.sap.com/en/index.html) and then uploading them to Amazon S3\. To access and use the files for installation, Launch Wizard requires them to be formatted according to the Amazon S3 file path syntax listed in the following table\. 

------
#### [ NetWeaver 7\.52 ]


| CD name | Versions | CD number | Amazon S3 file path | 
| --- | --- | --- | --- | 
| SWPM | SWPM 1\.0 latest version  | SWPM10SP30\_0\-20009701\.SAR | S3://<Your SAP software bucket>/<Path representing NW version>/SWPM | 
| SAPCAR | latest |  N/A | S3://<Your SAP software bucket>/<Path representing NW version>/SAPCAR | 
| Exports | NW 7\.52  | 51051806\_part1\.exe 51051806\_part2\.rar | S3://<Your SAP software bucket>/<Path representing NW version>/Exports | 
| Kernel components | NW 7\.53 |  `igsexe_12-80003187.sar` `igshelper_17-10010245.sar` `SAPEXE_700-80002573.SAR`  `SAPEXEDB_700-80002572.SAR` `SAPHOSTAGENT49_49-20009394.SAR`  | S3://<Your SAP software bucket>/<Path representing NW version>/Kernel | 
| SAP HANA Client | 2\.5  | IMDB\_CLIENT20\_005\_111\-80002082\.SAR | S3://<Your SAP software bucket>/<Path representing NW version>/HANA\_Client\_Software | 

The following HANA DB versions are supported\.

**Note**  
Use the latest CDs for the version\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-software-install-details.html)

------
#### [ NetWeaver 7\.50 ]


| CD name | Versions | CD number | Amazon S3 file path | 
| --- | --- | --- | --- | 
| SWPM | SWPM 1\.0 latest version  | SWPM10SP30\_0\-20009701\.SAR | S3://<Your SAP software bucket>/<Path representing NW version>/SWPM | 
| SAPCAR | latest |  N/A | S3://<Your SAP software bucket>/<Path representing NW version>/SAPCAR | 
| Exports | NW 7\.50 | 51050829\_3\.ZIP   | S3://<Your SAP software bucket>/<Path representing NW version>/Exports | 
| Kernel components | NW 7\.53 |  `igsexe_12-80003187.sar` `igshelper_17-10010245.sar` `SAPEXE_700-80002573.SAR`  `SAPEXEDB_700-80002572.SAR` `SAPHOSTAGENT49_49-20009394.SAR`  | S3://<Your SAP software bucket>/<Path representing NW version>/Kernel | 
| SAP HANA Client | 2\.5  | IMDB\_CLIENT20\_005\_111\-80002082\.SAR | S3://<Your SAP software bucket>/<Path representing NW version>/HANA\_Client\_Software | 

The following HANA DB versions are supported\.

**Note**  
Use the latest CDs for the version\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-software-install-details.html)

------
#### [ BW/4HANA 2\.0 ]


| CD name | Versions | CD number | Amazon S3 file path | 
| --- | --- | --- | --- | 
| SWPM | SWPM 2\.0 latest version  | SWPM20SP07\_0\-80003424\.SAR | S3://<Your SAP software bucket>/<Path representing NW version>/SWPM | 
| SAPCAR | latest |  N/A | S3://<Your SAP software bucket>/<Path representing NW version>/SAPCAR | 
| Exports | BW4HANA 2\.0  | BW4HANA200\_INST\_EXPORT\_1\.zip through BW4HANA200\_INST\_EXPORT\_7\.zip  | S3://<Your SAP software bucket>/<Path representing NW version>/Exports | 
| Language Packages  | BW4HANA 2\.0  | BW4HANA200\_NW\_LANG\* | S3://<Your SAP software bucket>/<Path representing NW version>/Exports | 
| Kernel components | NW 7\.77 |  `igsexe_12-80003187.sar` `igshelper_17-10010245.sar` `SAPEXE_300-80004393.SAR`  `SAPEXEDB_300-80004392.SAR` `SAPHOSTAGENT49_49-20009394.SAR`  | S3://<Your SAP software bucket>/<Path representing NW version>/Kernel | 
| SAP HANA Client | 2\.5  | IMDB\_CLIENT20\_005\_111\-80002082\.SAR | S3://<Your SAP software bucket>/<Path representing NW version>/HANA\_Client\_Software | 

The following HANA DB version is supported\.

**Note**  
\*Use the latest CDs for the version\.


| CD name | Versions | CD number | Amazon S3 file path | 
| --- | --- | --- | --- | 
| SAP HANA database software | hana\-20\-sp05  | 51054623  | S3://<Your SAP software bucket>/<Path representing NW version>/HANA\_DB\_Software | 

------
#### [ S/4HANA 1909 ]


| CD name | Versions | CD number | Amazon S3 file path | 
| --- | --- | --- | --- | 
| SWPM | SWPM 2\.0 latest version  | SWPM20SP07\_0\-80003424\.SAR | S3://<Your SAP software bucket>/<Path representing NW version>/SWPM | 
| SAPCAR | latest |  N/A | S3://<Your SAP software bucket>/<Path representing NW version>/SAPCAR | 
| Exports | S4Core 104 | S4CORE104\_INST\_EXPORT\_1\.zip through S4CORE104\_INST\_EXPORT\_25\.zip  | S3://<Your SAP software bucket>/<Path representing NW version>/Exports | 
| Language Packages  | S4 Lang 104 | S4HANAOP104\_ERP\_LANG\* | S3://<Your SAP software bucket>/<Path representing NW version>/Exports | 
| Kernel components | NW 7\.77 |  `igsexe_12-80003187.sar` `igshelper_17-10010245.sar` `SAPEXE_300-80004393.SAR`  `SAPEXEDB_300-80004392.SAR` `SAPHOSTAGENT49_49-20009394.SAR`  | S3://<Your SAP software bucket>/<Path representing NW version>/Kernel | 
| SAP HANA Client | 2\.5  | IMDB\_CLIENT20\_005\_111\-80002082\.SAR | S3://<Your SAP software bucket>/<Path representing NW version>/HANA\_Client\_Software | 

The following HANA DB version is supported\.

**Note**  
\*Use the latest CDs for the version\.


| CD name | Versions | CD number | Amazon S3 file path | 
| --- | --- | --- | --- | 
| SAP HANA Database software | hana\-20\-sp05  | 51054623  | S3://<Your SAP software bucket>/<Path representing NW version>/HANA\_DB\_Software | 

------
#### [ S/4HANA 2020 ]


| CD name | Versions | CD number | Amazon S3 file path | 
| --- | --- | --- | --- | 
| SWPM | SWPM 2\.0 latest version  | SWPM20SP07\_0\-80003424\.SAR | S3://<Your SAP software bucket>/<Path representing NW version>/SWPM | 
| SAPCAR | latest |  N/A | S3://<Your SAP software bucket>/<Path representing NW version>/SAPCAR | 
| Exports | S4Core 105 |  S4CORE105\_INST\_EXPORT\_1\.zip through S4CORE105\_INST\_EXPORT\_24\.zip | S3://<Your SAP software bucket>/<Path representing NW version>/Exports | 
| Language Packages  | S4 Lang 105 | S4HANAOP105\_ERP\_LANG\* | S3://<Your SAP software bucket>/<Path representing NW version>/Exports | 
| Kernel components | NW 7\.77 |  `igsexe_0-70005417.sar` `igshelper_17-10010245.sar` `SAPEXE_15-70005283.SAR`  `SAPEXEDB_15-70005282.SAR` `SAPHOSTAGENT49_49-20009394.SAR`  | S3://<Your SAP software bucket>/<Path representing NW version>/Kernel | 
| SAP HANA Client | 2\.5  | IMDB\_CLIENT20\_005\_111\-80002082\.SAR | S3://<Your SAP software bucket>/<Path representing NW version>/HANA\_Client\_Software | 

The following HANA DB version is supported\.

**Note**  
\*Use the latest CDs for the version\.


| CD name | Versions | CD number | Amazon S3 file path | 
| --- | --- | --- | --- | 
| SAP HANA Database software | hana\-20\-sp05  | 51054623  | S3://<Your SAP software bucket>/<Path representing NW version>/HANA\_DB\_Software | 

------
