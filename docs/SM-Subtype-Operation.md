# Solution Manager Sub-type Operation

It should be noted that all interactions with the Solution Manager sub-type can only be completed using Solution Manager.

The ACS AzureVM implementation provides a Azure VM job sub-type for defining the Azure VM jobs using Solution Manager.

Before defining jobs, the drop-down and config scripts and ACSAZUREVM agent should be created (see installation section). 

## Azure VM Job Definition

When defining a AzureVM job, the job type of **AzureVM** must be selected.
Once selected, a Task Type can be selected from the drop-down list.

The following tasks are available 

- **Create Virtual Machine**
- **Deallocate Virtual Machine** 
- **Delete Virtual Machine**
- **List Virtual Machines**
- **PowerOff Virtual Machine**
- **Restart Virtual Machine**
- **Start Virtual Machine**

### Create Virtual Machine
The Create Virtual Machine task type can be used to create a new virtual machine in the Azure environment from a defined Image. 

Field                                | Description
------------------------------------ | -----------
**Resource Group**                   | Required field that defines the resource group where the virtual machine was allocated.
**Machine Name**                     | Required field that defines the name of the virtual machine to restart.
**Private IP Address Property Name** | Optional field, when present will contain the private IP Address of the virtual machine after restart.
**Public IP Address Property Name**  | Optional field, when present will contain the public IP Address of the virtual machine after restart.
**Region**                           | Required field that defines the region where the virtual machine should be created. Select the region from the drop-down list.
**Custom Image**                     | When selecting a custom image, a Disk Size is not required and the image size should match the size used when creating the custom image.
**Image**                            | Required field, select the image to use when creating the virtual machine.
**Image Size**                       | Required field, select the image size. When selecting a custom image, the image size should match the size used when creating the custom image.
**Network**                          | Required field. When a new definition is required use the default value ***new***. To add a virtual machine to an existing Virtual network/subnet enter this name in the fields.
**Address Space**                    | Required field. When a new definition is required use the default value ***10.0.0.0/24***. To add a virtual machine to an existing Virtual network/subnet enter this name in the fields.
**Admin User**                       | Enter a user name for the administrator for this virtual machine taking into account the naming requirements for your virtual machine type.
**User Password**                    | Enter the password that will be associated with the admin user.

### Deallocate Virtual Machine
The Deallocate Virtual Machine task type can be used to stop a virtual machine and deallocate all its resources.  

Field                   | Description
----------------------- | -----------
**Resource Group**      | Required field that defines the resource group where the virtual machine was allocated.
**Machine Name**        | Required field that defines the name of the virtual machine to deallocate.

### Delete Virtual Machine
The Delete Virtual Machine task type can be used to remove a virtual machine from the resource group.  

Field                   | Description
----------------------- | -----------
**Resource Group**      | Required field that defines the resource group where the virtual machine was allocated.
**Machine Name**        | Required field that defines the name of the virtual machine to delete.

### List Virtual Machines
The List Virtual Machines task type can be used to list the status of all virtual machines associated with the resource group.  

Field                   | Description
----------------------- | -----------
**Resource Group**      | Required field that defines the resource group to get a list of virtual machines for.

Information returned

```
------------------------------------------------------------------------------------------------------------------ 
Virtual Machine Name     Region              Current State                 IP Private     IP Public      OS Type    
------------------------------------------------------------------------------------------------------------------ 
LINUX001                 West Europe         PowerState/running            10.1.0.17      52.178.65.0    LINUX      
LINUX002                 West Europe         PowerState/running            10.1.0.16      13.95.134.20   LINUX      
LINUX003                 West Europe         PowerState/running            10.1.0.18      13.95.106.31   LINUX      
OpCon                    West Europe         PowerState/running            10.1.0.6       52.166.248.28  WINDOWS    
SDM2                     West Europe         PowerState/stopped            10.1.0.9       168.63.108.63  LINUX      
------------------------------------------------------------------------------------------------------------------ 

```

### PowerOff Virtual Machine
The PowerOff Virtual Machine task type can be used to shutdown a virtual machine from the resource group.  

Field                   | Description
----------------------- | -----------
**Resource Group**      | Required field that defines the resource group where the virtual machine was allocated.
**Machine Name**        | Required field that defines the name of the virtual machine to power off.

### Restart Virtual Machine
The Restart Virtual Machine task type can be used to restart a virtual machine from the resource group. If the private and public property
values are defined, the ip addresses of the virtual machine will be stored in the properties. 

Field                                | Description
------------------------------------ | -----------
**Resource Group**                   | Required field that defines the resource group where the virtual machine was allocated.
**Machine Name**                     | Required field that defines the name of the virtual machine to restart.
**Private IP Address Property Name** | Optional field, when present will contain the private IP Address of the virtual machine after restart.
**Public IP Address Property Name**  | Optional field, when present will contain the public IP Address of the virtual machine after restart.

### Start Virtual Machine
The Start Virtual Machine task type can be used to start a virtual machine from the resource group. If the private and public property
values are defined, the ip addresses of the virtual machine will be stored in the properties. 

Field                                | Description
------------------------------------ | -----------
**Resource Group**                   | Required field that defines the resource group where the virtual machine was allocated.
**Machine Name**                     | Required field that defines the name of the virtual machine to start.
**Private IP Address Property Name** | Optional field, when present will contain the private IP Address of the virtual machine after startup.
**Public IP Address Property Name**  | Optional field, when present will contain the public IP Address of the virtual machine after startup.

## Job Finished processing 

An AzureVM Scheduled JOB has the following possible return codes:

0	Success, the job completed processing.
1	Failure, an exception occurred during job processing.

This means that to check for a successful completion, the Failure Criteria should be set to NE (Not Equal) to 0.

## Logging

All information produced by the OpCon job is available in the job output and can be retrieved using the OpCon JORS capability. 
