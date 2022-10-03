# Operation

The connector can be used by entering arguments on the command line or using the job sub-type.

## AzureVM Job Sub-Type
The AzureVM connector provides a Job Sub-Type that can be used to simplify job definitions within OpCon.

![jobsubtype](/docs/images/azure_vm_subtype.PNG)

Select the Task from the drop-down list and enter the required values. Only values associated with the task will be enabled. Once a task has been saved, the task type cannot be changed.

When creating a virtual machine, select the attribute values from the various drop-down lists. 
Selecting the type from the **Virtual Machine Type** drop-down list (either Windows or Linux, will popoluate the Image Name drop-down lists with images associated with the Virtual Machine Type). 
Select the region from the **Region** drop-down list.
Select the image that will be used to create the virtual machine from the **Image Name** drop-down list.
If the image is a custom image, check the **Custom Image** checkbox. When selecting a custom image, a Disk Size is not required and the image size should match the size used when creating the custom image.
Select the image size from the **Image Size** drop-down list.
Enter a disk size in giga-bytes in the **Disk Size (Gb)** field when creating a non custom image.
Enter the networking requirements for the virtual machine in the **Network** and **Address Space** fields. To add a virtual machine to an existing Virtual network/subnet enter this name in the fields.
Enter a user name for the administrator for this virtual machine into the **Admin Userid** field taking into account the naming requirements for your virtual machine type.
Enter the password that will be associated with the admin user in the **Admin Userid Password** field (encrypted global properties can be used to hide the password).

## AzureVM Arguments
The AzureVM connector requires arguments to be given to function. It uses the principle of Tasks, where each task performs an action or a combination of actions against Azure Virtual Machines.

### Global
Arguments  | Description
---------- | -----------
**-rg**    | (Mandatory) The Resource group associated with the Azure request.
**-t**     | (Mandatory) The task to perform.

### Create Virtual Machine
Can be used to create a new cvirtual machine within the associated resource group.

Arguments  | Description
---------- | -----------
**-t**     | Value is **create**
**-vm**    | Required field containing the name of the virtual machine to create.
**-ippvt** | Optional field containing the name of the global property to insert the virtual machine private ipaddress into.
**-ippub** | Optional field containing the name of the global property to insert the virtual machine public ipaddress into.
**-r**     | Required field containing the region where the virtual machine should be created.
**-at**    | Required field containing the attributes that will be used to create the virtual machine.
**-ci**    | Optional field indicating if the image to be used to crate the virtual machine is a custom image.

Usage
```
AzureVM.exe -rg MY_RESOURCEGROUP -t create -vm MY_VMACHINE -ippvt MY_VMACHINE_PVT -ippub MY_VMACHINE_PUB -at "linux,LINUX003,Canonical_UbuntuServer_18.04-LTS,Standard_DS3_v2,50,myadmin,<SmaEncrypt>Ohj+VFLoM3YX0WxNNN4vJQ==</SmaEncrypt>,LzLabsEU/default,LzLabsEU/default,"
```
### Delete Virtual Machine
Can be used to delete a virtual machine.

Arguments  | Description
---------- | -----------
**-t**     | Value is **delete**
**-vm**    | Required field containing the name of the virtual machine to delete.

Usage
```
AzureVM.exe -rg MY_RESOURCEGROUP -t delete -vm MY_VMACHINE
```
### List Virtual Machines
Can be used to list information about virtual machines in the resource group.

Arguments  | Description
---------- | -----------
**-t**     | Value is **list**

Usage
```
AzureVM.exe -rg MY_RESOURCEGROUP -t list 
```
Infomation returned
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
Can be used to poweroff a virtual machine.

Arguments  | Description
---------- | -----------
**-t**     | Value is **poweroff**
**-vm**    | Required field containing the name of the virtual machine to power off.

Usage
```
AzureVM.exe -rg MY_RESOURCEGROUP -t poweroff -vm MY_VMACHINE
```
### Restart Virtual Machine
Can be used to restart a virtual machine.

Arguments  | Description
---------- | -----------
**-t**     | Value is **restart**
**-vm**    | Required field containing the name of the virtual machine to restart.
**-ippvt** | Optional field containing the name of the global property to insert the virtual machine private ipaddress into.
**-ippub** | Optional field containing the name of the global property to insert the virtual machine public ipaddress into.

Usage
```
AzureVM.exe -rg MY_RESOURCEGROUP -t restart -vm MY_VMACHINE -ippvt MY_VMACHINE_PVT -ippub MY_VMACHINE_PUB
```
### Start Virtual Machine
Can be used to start a virtual machine.

Arguments  | Description
---------- | -----------
**-t**     | Value is **start**
**-vm**    | Required field containing the name of the virtual machine to start.
**-ippvt** | Optional field containing the name of the global property to insert the virtual machine private ipaddress into.
**-ippub** | Optional field containing the name of the global property to insert the virtual machine public ipaddress into.

Usage
```
AzureVM.exe -rg MY_RESOURCEGROUP -t start -vm MY_VMACHINE -ippvt MY_VMACHINE_PVT -ippub MY_VMACHINE_PUB
```
## Exit Codes
The `AzureVM` exits `0` when the performed request succeeds. Otherwise `AzureVM` exits `1` on failure.
