# Installation

The Azure VM Connector installation consists of multiple steps that are required to complete the installation successfully. The connector requires the installation of a Windows Agent.

## Supported Software Levels
The following software levels are required to implement the Azure VM Connector.

- OpCon Release 25.0.3 or higher (if Solution Manager sub-type required). 
- OpCon Release 19.0 or higher (if Enterprise Manager sub-type required).
- OpCon Rest API.
- Uses an embedded Open-JDK (Version 11).

## Installation
The installation process consists of the following steps:

- Azure VM Connector Installation.
- Azure VM Connector Configuration.
- For Enterprise Manager sub-type installation
    - Adding Azure VM Connector job subtype to Enterprise Manager.
    - Creating global properties that sub-type uses.
- For Solution Manager sub-type installation.
    - Creating AzureVM scripts.
    - Creating AzureVM Agent.    

### Azure VM Connector Installation
The Azure VM connector can be installed on any Windows Server as long as there is an OpCon MSLSAM Agent installed on the Windows Server. 

Download AzureVM_Windows.zip file from the desired [release available here](https://github.com/SMATechnologies/azure-vm-java/releases).

After download, extract the zip file to the location you'd like to install the connector to. Once unzipped, everything needed should be located under the root folder of that directory.

When installing using the Enterprise Manager sub-type, the connector can be installed on any Windows system that contains a Windows agent.

When installing using the Solution Manager sub-type, the connector must be installed on either the same Windows server as the OpCon installation or the same Windows server as the Relay installation.

After the extraction, the root installation directory contains the connector executable (AzureVM.exe), the encryption software (Encrypt.exe), the Connector.config file and two directories, java and emplugins. 

The java directory contains the java software required to execute the connector (OpenJDK 11) and the emplugins directory contains the job sub-type plugin for Enterprise Manager.

#### Encrypt Utility
The Encrypt utility uses standard 64 bit encryption.

Supports a -v argument and displays the encrypted value

On Windows, example on how to encrypt the value "abcdefg":

```
EncryptValue.exe -v "abcdefg"

```
#### Azure VM Connector Configuration
The Azure Vm connector uses a configuration file **Connector.config** that contains the Azure account information.

The account information consists of the subscription ID, the tenant ID, the client ID and secret key. These values can be retrieved from the Azure environment.

The connection subsction ID, tenant ID, client ID and the secret key must be encrypted using the **Encrypt.exe** program.

The encryption tool provides basic encryption capabilities to prevent configuration information from being displayed in clear text.

The connector also connects to the OpCon environment to save virtual machine addresses in global properties. This connection uses a application token when connecting to OpCon through the OpCon Rest-API. The application token must be encrypted using the **Encrypt.exe** program.

The connector provides a **--setup** switch which can be used to set the OpCon Api information in the Connector.config. An application token of **CONNECTORS** is created and the returned token is encrypted and inserted in Connector.config as OPCONAPI_TOKEN. 

When using this switch the following arguments are required.
 
Argument       | Description
-------------- | ---------------
-username      | An OpCon user name that is a member of the ocadm role.
-password      | The password of the OpCon user.
-address       | The address and port number of the OpCon Rest-API server - this information in inserted into the Connector.config as OPCON-ADDRESS.
-tls           | Indicates if the OpCon Rest-API requires a tls connection - this information is inserted into the Connector.config as OPCONAPI_USING_TLS.

```
AzureVM.exe --setup -username name -password pwd -address server:port -tls

```

**Connector.config** file example:
```
[CONNECTOR]
NAME=Azure VM Connector
DEBUG=OFF

[MSAZURE]
TENANT = (encrypted value)
SUBSCRIPTION = (encrypted value)
CLIENT = (encrypted value)
KEY = (encrypted value)

[OPCON API]
OPCONAPI_ADDRESS = address:port
OPCONAPI_USING_TLS = True
OPCONAPI_TOKEN = (encrypted value)

```

Keyword             | Type | Description
------------------- | ---- | -----------
TENANT              | Text | **encrypted value** is the tenant ID encrypted using the using the **Encrypt.exe** program. 
SUBSCRIPTION        | Text | **encrypted value** is the subscription ID encrypted using the using the **Encrypt.exe** program. 
CLIENT              | Text | **encrypted value** is the client ID encrypted using the using the **Encrypt.exe** program. 
KEY                 | Text | **encrypted value** is the secret key encrypted using the using the **Encrypt.exe** program. 
OPCON_API_ADDRESS   | Text | is the address of the OpCon System and in inserted into the configuration file using the --setup switch on **AzureVM.exe**
OPCON_API_USING_TLS | Text | indicates if the OpCon System is using tls and in inserted into the configuration file using the --setup switch on **AzureVM.exe**
OPCON_API_TOKEN     | Text | **encrypted value** is the application token used to connect to the OpCon System and into inserted in the configuration file using

### Enterprise Manager sub-type installation

Copy the Enterprise Manager plug-in from the ***installation_dir***\\emplugins directory to the dropins directory of the Enterprise Manager installation. 

If the dropins directory does not exist, create the dropins directory off the root directory. 

Restart Enterprise Manager and a new Windows job subtype called Azure VM will be visible.

If not restart Enterprise Manager using 'Run as Administrator'. After this Enterprise Manager can be used normally.

### Create AzureVmPath Global Property
Create a global property **AzureVmPath** that contains the full path of the installation directory.

### Create Special Properties
The Azure VM connector uses three global properties to hold the values of images and the associated sizes.

Name                       | Description
-------------------------- | -----------
**AZURE_WINDOWS_SERVERS**  | Create the global property and add the values contained below using a comma to separate them. The doubles quotes surrounding the values must be retained. These values will then be visible in the drop-down list (see List of Windows Virtual Machines). Image names can be added to the drop-down list by editing the property. 
**AZURE_LINUX_SERVERS**    | Create the global property and add the values contained in below using a comma to separate them. The doubles quotes surrounding the values must be retained. These values will then be visible in the drop-down list (see List of Linux Virtual Machines).
**AZURE_SERVER_SIZES**     | Create the global property and add the values contained below using a comma to separate them. The doubles quotes surrounding the values must be retained. These values will then be visible in the drop-down list (see List of Virtual Machine Sizes).

```
List of Windows Virtual Machines 

"MicrosoftSQLServer_SQL2012SP3-WS2012R2_Enterprise","MicrosoftSQLServer_SQL2012SP4-WS2012R2_Enterprise","MicrosoftSQLServer_SQL2012SP4-WS2012R2_Express","MicrosoftSQLServer_SQL2012SP4-WS2012R2_Standard","MicrosoftSQLServer_SQL2014SP2-WS2012R2_Enterprise","MicrosoftSQLServer_SQL2014SP2-WS2012R2_Express",
"MicrosoftSQLServer_SQL2014SP2-WS2012R2_Standard","MicrosoftSQLServer_sql2014sp3-ws2012r2_enterprise","MicrosoftSQLServer_sql2014sp3-ws2012r2_express","MicrosoftSQLServer_sql2014sp3-ws2012r2_standard","MicrosoftSQLServer_SQL2016-WS2012R2_Enterprise","MicrosoftSQLServer_SQL2016SP1-WS2016_Enterprise","MicrosoftSQLServer_SQL2016SP1-WS2016_Express","MicrosoftSQLServer_SQL2016SP1-WS2016_Standard","MicrosoftSQLServer_SQL2016SP2-WS2016_Enterprise","MicrosoftSQLServer_SQL2016SP2-WS2016_Express","MicrosoftSQLServer_SQL2016SP2-WS2016_Standard","MicrosoftSQLServer_SQL2017-WS2016_Enterprise,"MicrosoftWindowsServer_WindowsServer_2012-Datacenter","MicrosoftWindowsServer_WindowsServer_2012-R2-Datacenter","MicrosoftWindowsServer_WindowsServer_2016-Datacenter","MicrosoftWindowsServer_WindowsServer_2019-Datacenter"

List of Linux Virtual Machines 

"Canonical_UbuntuServer_12.04.3-LTS","Canonical_UbuntuServer_12.04.4-LTS","Canonical_UbuntuServer_12.04.5-LTS","Canonical_UbuntuServer_14.04.0-LTS","Canonical_UbuntuServer_14.04.1-LTS","Canonical_UbuntuServer_14.04.2-LTS","Canonical_UbuntuServer_14.04.2-LTS","Canonical_UbuntuServer_14.04.3-LTS","Canonical_UbuntuServer_14.04.4-LTS","Canonical_UbuntuServer_14.04.5-LTS","Canonical_UbuntuServer_16.04-LTS","Canonical_UbuntuServer_16.04.0","Canonical_UbuntuServer_18.04","Canonical_UbuntuServer_18.10","RedHat_RHEL_6.7","RedHat_RHEL_6.8","RedHat_RHEL_6.9","RedHat_RHEL_6.10","RedHat_RHEL_7.2","RedHat_RHEL_7.3","RedHat_RHEL_7.4",
"RedHat_RHEL_7.5","SUSE_SLES_11-SP4","SUSE_SLES_12-SP4","SUSE_SLES_15","SUSE_SLES_15","MicrosoftSQLServer_SQL2017-RHEL7_Enterprise","MicrosoftSQLServer_SQL2017-RHEL7_Express","MicrosoftSQLServer_SQL2017-RHEL7_Standard","MicrosoftSQLServer_SQL2017-SLES12SP2_Enterprise","MicrosoftSQLServer_SQL2017-SLES12SP2_Express","MicrosoftSQLServer_SQL2017-Ubuntu1604_Enterprise","MicrosoftSQLServer_SQL2017-Ubuntu1604_Express"

List of Virtual Machine Sizes

"Standard_A0","Standard_A1","Standard_A2","Standard_A3","Standard_A5","Standard_A4","Standard_A6","Standard_A7”,“Basic_A0”,“Basic_A1”,“Basic_A2”,“Basic_A3”,“Basic_A4","Standard_D1_v2","Standard_D2_v2","Standard_D3_v2","Standard_D4_v2","Standard_D5_v2","Standard_D11_v2","Standard_D12_v2","Standard_D13_v2","Standard_D14_v2","Standard_D15_v2","Standard_D2_v2_Promo","Standard_D3_v2_Promo","Standard_D4_v2_Promo","Standard_D5_v2_Promo","Standard_D11_v2_Promo","Standard_D12_v2_Promo","Standard_D13_v2_Promo","Standard_D14_v2_Promo","Standard_F1","Standard_F2","Standard_F4","Standard_F8","Standard_F16","Standard_A1_v2","Standard_A2m_v2","Standard_A2_v2","Standard_A4m_v2","Standard_A4_v2","Standard_A8m_v2","Standard_A8_v2","Standard_DS1","Standard_DS2","Standard_DS3","Standard_DS4","Standard_DS11","Standard_DS12","Standard_DS13","Standard_DS14","Standard_D2_v3","Standard_D4_v3","Standard_D8_v3","Standard_D16_v3","Standard_D32_v3","Standard_D1","Standard_D2","Standard_D3","Standard_D4","Standard_D11","Standard_D12","Standard_D13","Standard_D14","Standard_B1ms","Standard_B1s","Standard_B2ms","Standard_B2s","Standard_B4ms","Standard_B8ms","Standard_DS1_v2","Standard_DS2_v2","Standard_DS3_v2","Standard_DS4_v2","Standard_DS5_v2","Standard_DS11-1_v2","Standard_DS11_v2","Standard_DS12-1_v2","Standard_DS12-2_v2","Standard_DS12_v2","Standard_DS13-2_v2","Standard_DS13-4_v2","Standard_DS13_v2","Standard_DS14-4_v2","Standard_DS14-x8_v2","Standard_DS14_v2","Standard_DS2_v2_Promo","Standard_DS3_v2_Promo","Standard_DS4_v2_Promo","Standard_DS5_v2_Promo","Standard_DS11_v2_Promo","Standard_DS12_v2_Promo","Standard_DS13_v2_Promo","Standard_DS14_v2_Promo","Standard_F1s","Standard_F2s","Standard_F4s","Standard_F8s","Standard_F16s","Standard_D64_v3","Standard_D2s_v3","Standard_D4s_v3","Standard_D8s_v3","Standard_D16s_v3","Standard_D32s_v3","Standard_D64s_v3","Standard_E2_v3","Standard_E4_v3","Standard_E8_v3","Standard_E16_v3","Standard_E20_v3","Standard_E32_v3","Standard_E64i_v3","Standard_E64_v3","Standard_E2s_v3","Standard_E4-2s_v3","Standard_E4s_v3","Standard_E8-2s_v3","Standard_E8-4s_v3","Standard_E8s_v3","Standard_E16-4s_v3","Standard_E16-8s_v3","Standard_E16s_v3","Standard_E20s_v3","Standard_E32-8s_v3","Standard_E32-16s_v3","Standard_E32s_v3","Standard_E64-16s_v3","Standard_E64-32s_v3","Standard_E64is_v3","Standard_E64s_v3","Standard_DS15_v2","Standard_F2s_v2","Standard_F4s_v2","Standard_F8s_v2","Standard_F16s_v2","Standard_F32s_v2","Standard_F64s_v2",
"Standard_F72s_v2","Standard_G1","Standard_G2","Standard_G3","Standard_G4","Standard_G5","Standard_GS1","Standard_GS2","Standard_GS3","Standard_GS4","Standard_GS4-4","Standard_GS4-8","Standard_GS5","Standard_GS5-8","Standard_GS5-16","Standard_L4s","Standard_L8s","Standard_L16s","Standard_L32s"

```

### Solution Manager sub-type installation

It should be noted that all interactions with the Solution Manager sub-type can only be completed using Solution Manager.

Download the ACSAZUREVM zip file from the ftp site under **OpCon Releases\\Integrations\\AZUREVM**.

Extract the **ACSAZUREVM** directory and copy this into the **\\SAM\\plugins** for OpCon and relay installations.

For OpCon installations stop and restart the **SMA OpCon RestAPI** and **SMA OpCon Service Manager** services, for Relay stop and restart the **Relay Service**.

#### Create the scripts
When using the Solution Manager sub-type, two scripts must be created. The first script contains the Connector.config information and the second script contains the drop-down list information.

Using Solution Manager
- Select **Library**.
- Select **Scripts**.
- Select **Script Types** from the upper right hand corner. 
    - Select **+Add**
    - In the ***Name*** field enter **ACSAZUREVM**.
    - In the ***File Extension field*** enter **txt**.
    - In the ***Description*** field enter **Used for ACSAZUREVM Integration**.
    - Select Save.
- Select **Script Runners** from the upper right hand corner. 
    - Select **+Add**
    - In the ***Name*** field enter **ACSAZUREVM**.
    - In the ***OS*** field select **AzureVM** from the drop-down list.
    - In the ***Type*** field select **ACSAZUREVM** from the drop-down list.
    - In the ***Command*** field enter **cmd.exe /c**.
    - Select Save.
- Select **Scripts** from the upper right hand corner.
    - Create the Connector.config script.
    - Select **+Add**.
    - In the ***Name*** field enter a name for the script. It is suggested using the proposed agent name and append **_config** to the name. 
    - In the ***Type*** field select **ACSAZUREVM** from the drop-down list.
    - Assign the required roles.
    - In the ***Script*** paste the contents of the created Connector.config file.
    - Select Save.
    - Create the drop-down information script.
    - Select **+Add**.
    - In the ***Name*** field enter a name for the script. It is suggested using the proposed agent name and append **_data** to the name. 
    - In the ***Type*** field select **ACSAZUREVM** from the drop-down list.
    - Assign the required roles.
    - In the ***Script*** paste the information below.
    - Select Save.

The information for the _data script is provided below. Additional information can be added to the script by using the key names IMAGE, SIZE and REGION and the required values. The data values are passed each time a job is defined.

```
TYPE Windows
TYPE Linux
REGION Australia Central
REGION Australia Central 2
REGION Australia East
REGION Australia Southeast
REGION Brasil South
REGION Canada Central
REGION Canada East
REGION Central India
REGION Central US
REGION China East
REGION China East 2
REGION China North
REGION China North 2
REGION East Asia
REGION East US
REGION East US 2
REGION France Central
REGION France South
REGION Germany Central
REGION Germany North
REGION Japan East
REGION Japan West
REGION Korea Central
REGION Korea South
REGION North Central US
REGION North Europe
REGION South Central US
REGION South India
REGION SouthEast Asia
REGION UK South
REGION UK West
REGION West Central US
REGION West Europe
REGION West India
REGION West US 2
REGION West US
IMAGE MicrosoftSQLServer_SQL2012SP3-WS2012R2_Enterprise
IMAGE MicrosoftSQLServer_SQL2012SP4-WS2012R2_Enterprise
IMAGE MicrosoftSQLServer_SQL2012SP4-WS2012R2_Express
IMAGE MicrosoftSQLServer_SQL2012SP4-WS2012R2_Standard
IMAGE MicrosoftSQLServer_SQL2014SP2-WS2012R2_Enterprise
IMAGE MicrosoftSQLServer_SQL2014SP2-WS2012R2_Express
IMAGE MicrosoftSQLServer_SQL2014SP2-WS2012R2_Standard
IMAGE MicrosoftSQLServer_sql2014sp3-ws2012r2_enterprise
IMAGE MicrosoftSQLServer_sql2014sp3-ws2012r2_express
IMAGE MicrosoftSQLServer_sql2014sp3-ws2012r2_standard
IMAGE MicrosoftSQLServer_SQL2016-WS2012R2_Enterprise
IMAGE MicrosoftSQLServer_SQL2016SP1-WS2016_Enterprise
IMAGE MicrosoftSQLServer_SQL2016SP1-WS2016_Express
IMAGE MicrosoftSQLServer_SQL2016SP1-WS2016_Standard
IMAGE MicrosoftSQLServer_SQL2016SP2-WS2016_Enterprise
IMAGE MicrosoftSQLServer_SQL2016SP2-WS2016_Express
IMAGE MicrosoftSQLServer_SQL2016SP2-WS2016_Standard
IMAGE MicrosoftSQLServer_SQL2017-WS2016_Enterprise
IMAGE MicrosoftWindowsServer_WindowsServer_2012-Datacenter
IMAGE MicrosoftWindowsServer_WindowsServer_2012-R2-Datacenter
IMAGE MicrosoftWindowsServer_WindowsServer_2016-Datacenter
IMAGE MicrosoftWindowsServer_WindowsServer_2019-Datacenter
IMAGE Canonical_UbuntuServer_12.04.3-LTS
IMAGE Canonical_UbuntuServer_12.04.4-LTS
IMAGE Canonical_UbuntuServer_12.04.5-LTS
IMAGE Canonical_UbuntuServer_14.04.0-LTS
IMAGE Canonical_UbuntuServer_14.04.1-LTS
IMAGE Canonical_UbuntuServer_14.04.2-LTS
IMAGE Canonical_UbuntuServer_14.04.2-LTS
IMAGE Canonical_UbuntuServer_14.04.3-LTS
IMAGE Canonical_UbuntuServer_14.04.4-LTS
IMAGE Canonical_UbuntuServer_14.04.5-LTS
IMAGE Canonical_UbuntuServer_16.04-LTS
IMAGE Canonical_UbuntuServer_16.04.0
IMAGE Canonical_UbuntuServer_18.04
IMAGE Canonical_UbuntuServer_18.10
IMAGE RedHat_RHEL_6.7
IMAGE RedHat_RHEL_6.8
IMAGE RedHat_RHEL_6.9
IMAGE RedHat_RHEL_6.10
IMAGE RedHat_RHEL_7.2
IMAGE RedHat_RHEL_7.3
IMAGE RedHat_RHEL_7.4
IMAGE RedHat_RHEL_7.5
IMAGE SUSE_SLES_11-SP4
IMAGE SUSE_SLES_12-SP4
IMAGE SUSE_SLES_15
IMAGE SUSE_SLES_15
IMAGE MicrosoftSQLServer_SQL2017-RHEL7_Enterprise
IMAGE MicrosoftSQLServer_SQL2017-RHEL7_Express
IMAGE MicrosoftSQLServer_SQL2017-RHEL7_Standard
IMAGE MicrosoftSQLServer_SQL2017-SLES12SP2_Enterprise
IMAGE MicrosoftSQLServer_SQL2017-SLES12SP2_Express
IMAGE MicrosoftSQLServer_SQL2017-Ubuntu1604_Enterprise
IMAGE MicrosoftSQLServer_SQL2017-Ubuntu1604_Express
SIZE Standard_A0
SIZE Standard_A1
SIZE Standard_A2
SIZE Standard_A3
SIZE Standard_A5
SIZE Standard_A4
SIZE Standard_A6
SIZE Standard_A7
SIZE Basic_A0
SIZE Basic_A1
SIZE Basic_A2
SIZE Basic_A3
SIZE Basic_A4
SIZE Standard_D1_v2
SIZE Standard_D2_v2
SIZE Standard_D3_v2
SIZE Standard_D4_v2
SIZE Standard_D5_v2
SIZE Standard_D11_v2
SIZE Standard_D12_v2
SIZE Standard_D13_v2
SIZE Standard_D14_v2
SIZE Standard_D15_v2
SIZE Standard_D2_v2_Promo
SIZE Standard_D3_v2_Promo
SIZE Standard_D4_v2_Promo
SIZE Standard_D5_v2_Promo
SIZE Standard_D11_v2_Promo
SIZE Standard_D12_v2_Promo
SIZE Standard_D13_v2_Promo
SIZE Standard_D14_v2_Promo
SIZE Standard_F1
SIZE Standard_F2
SIZE Standard_F4
SIZE Standard_F8
SIZE Standard_F16
SIZE Standard_A1_v2
SIZE Standard_A2m_v2
SIZE Standard_A2_v2
SIZE Standard_A4m_v2
SIZE Standard_A4_v2
SIZE Standard_A8m_v2
SIZE Standard_A8_v2
SIZE Standard_DS1
SIZE Standard_DS2
SIZE Standard_DS3
SIZE Standard_DS4
SIZE Standard_DS11
SIZE Standard_DS12
SIZE Standard_DS13
SIZE Standard_DS14
SIZE Standard_D2_v3
SIZE Standard_D4_v3
SIZE Standard_D8_v3
SIZE Standard_D16_v3
SIZE Standard_D32_v3
SIZE Standard_D1
SIZE Standard_D2
SIZE Standard_D3
SIZE Standard_D4
SIZE Standard_D11
SIZE Standard_D12
SIZE Standard_D13
SIZE Standard_D14
SIZE Standard_B1ms
SIZE Standard_B1s
SIZE Standard_B2ms
SIZE Standard_B2s
SIZE Standard_B4ms
SIZE Standard_B8ms
SIZE Standard_DS1_v2
SIZE Standard_DS2_v2
SIZE Standard_DS3_v2
SIZE Standard_DS4_v2
SIZE Standard_DS5_v2
SIZE Standard_DS11-1_v2
SIZE Standard_DS11_v2
SIZE Standard_DS12-1_v2
SIZE Standard_DS12-2_v2
SIZE Standard_DS12_v2
SIZE Standard_DS13-2_v2
SIZE Standard_DS13-4_v2
SIZE Standard_DS13_v2
SIZE Standard_DS14-4_v2
SIZE Standard_DS14-x8_v2
SIZE Standard_DS14_v2
SIZE Standard_DS2_v2_Promo
SIZE Standard_DS3_v2_Promo
SIZE Standard_DS4_v2_Promo
SIZE Standard_DS5_v2_Promo
SIZE Standard_DS11_v2_Promo
SIZE Standard_DS12_v2_Promo
SIZE Standard_DS13_v2_Promo
SIZE Standard_DS14_v2_Promo
SIZE Standard_F1s
SIZE Standard_F2s
SIZE Standard_F4s
SIZE Standard_F8s
SIZE Standard_F16s
SIZE Standard_D64_v3
SIZE Standard_D2s_v3
SIZE Standard_D4s_v3
SIZE Standard_D8s_v3
SIZE Standard_D16s_v3
SIZE Standard_D32s_v3
SIZE Standard_D64s_v3
SIZE Standard_E2_v3
SIZE Standard_E4_v3
SIZE Standard_E8_v3
SIZE Standard_E16_v3
SIZE Standard_E20_v3
SIZE Standard_E32_v3
SIZE Standard_E64i_v3
SIZE Standard_E64_v3
SIZE Standard_E2s_v3
SIZE Standard_E4-2s_v3
SIZE Standard_E4s_v3
SIZE Standard_E8-2s_v3
SIZE Standard_E8-4s_v3
SIZE Standard_E8s_v3
SIZE Standard_E16-4s_v3
SIZE Standard_E16-8s_v3
SIZE Standard_E16s_v3
SIZE Standard_E20s_v3
SIZE Standard_E32-8s_v3
SIZE Standard_E32-16s_v3
SIZE Standard_E32s_v3
SIZE Standard_E64-16s_v3
SIZE Standard_E64-32s_v3
SIZE Standard_E64is_v3
SIZE Standard_E64s_v3
SIZE Standard_DS15_v2
SIZE Standard_F2s_v2
SIZE Standard_F4s_v2
SIZE Standard_F8s_v2
SIZE Standard_F16s_v2
SIZE Standard_F32s_v2
SIZE Standard_F64s_v2
SIZE Standard_F72s_v2
SIZE Standard_G1
SIZE Standard_G2
SIZE Standard_G3
SIZE Standard_G4
SIZE Standard_G5
SIZE Standard_GS1
SIZE Standard_GS2
SIZE Standard_GS3
SIZE Standard_GS4
SIZE Standard_GS4-4
SIZE Standard_GS4-8
SIZE Standard_GS5
SIZE Standard_GS5-8
SIZE Standard_GS5-16
SIZE Standard_L4s
SIZE Standard_L8s
SIZE Standard_L16s
SIZE Standard_L32s

```
#### Create AzureVM Agent Definition

Using Solution Manager
- Select **Library**.
- Select **Agents**.
    - Select **+Add**
    - In the ***Name*** field enter the name of the agent.
    - Select **AzureVM** from the ***Type*** drop-down list.
    - In the ***AzureVM Settings*** section enter the required information.
    - In the ***Client Information*** section
        - In the ***Directory*** field enter the installation directory of the AzureVM Connector.
        - In the ***Name*** field insert **AzureVM.exe** (default value).
        - In the ***Config File Name*** field insert **Connector.config** (default value).
    - In the ***Config Script*** section
        - Select **ACSAZUREVM** from the ***Script Runner*** drop-down list.
        - Select the config script you previously created from the ***Script*** drop-down list.
    - In the ***Drop-down Script*** section
        - Select **ACSAZUREVM** from the ***Script Runner*** drop-down list.
        - Select the data script you previously created from the ***Script*** drop-down list.
    - In the ***Name*** field enter the name of the agent.

- Select **Save**.
