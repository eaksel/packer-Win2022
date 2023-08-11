# packer-Win2022

## What is packer-Win2022 ?

packer-Win2022 is a set of configuration files used to build automated Windows Server 2022 virtual machine images using [Packer](https://www.packer.io/).
This Packer configuration file allows you to build images for VMware Workstation, Oracle VM VirtualBox and QEMU (KVM).

## Prerequisites

- [Packer](https://www.packer.io/downloads.html)
  - <https://www.packer.io/intro/getting-started/install.html>
- A Hypervisor
  - [VMware Workstation](https://www.vmware.com/products/workstation-pro.html)
  - [Oracle VM VirtualBox](https://www.virtualbox.org/)

## How to use Packer

Commands to create an automated VM image:

To create a Windows Server 2022 VM image using VMware Workstation use the following commands:

```sh
cd c:\packer-Win2022
packer build -only=vmware-iso win2022-gui.json #Windows Server 2022 w/ GUI
packer build -only=vmware-iso win2022-core.json #Windows Server 2022 Core
packer build -only=vmware-iso win2022-gui_uefi.json #Windows Server 2022 w/ GUI using UEFI
packer build -only=vmware-iso win2022-core_uefi.json #Windows Server 2022 Core using UEFI
```

To create a Windows Server 2022 VM image using Oracle VM VirtualBox use the following commands:

```sh
cd c:\packer-Win2022
packer build -only=virtualbox-iso win2022-gui.json #Windows Server 2022 w/ GUI
packer build -only=virtualbox-iso win2022-core.json #Windows Server 2022 Core
packer build -only=virtualbox-iso win2022-gui_uefi.json #Windows Server 2022 w/ GUI using UEFI
packer build -only=virtualbox-iso win2022-core_uefi.json #Windows Server 2022 Core using UEFI
```

To create a Windows Server 2022 VM image using QEMU (KVM) use the following commands:

```sh
cd packer-Win2022
wget https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/virtio-win-0.1.229-1/virtio-win-0.1.229.iso
packer build -only=qemu win2022-gui.json #Windows Server 2022 w/ GUI
packer build -only=qemu win2022-core.json #Windows Server 2022 Core
packer build -only=qemu win2022-gui_uefi.json #Windows Server 2022 w/ GUI using UEFI
packer build -only=qemu win2022-core_uefi.json #Windows Server 2022 Core using UEFI
```

*If you omit the keyword "-only=" images for both Workstation and Virtualbox will be created.*

By default the .iso of Windows Server 2022 is pulled from <https://software-download.microsoft.com/download/sg/20348.169.210806-2348.fe_release_svc_refresh_SERVER_EVAL_x64FRE_en-us.iso>

You can change the URL to one closer to your build server. To do so change the **"iso_url"** parameter in the **"variables"** section of the win2022-*.json file.

```json
{
  "variables": {
      "iso_url": "https://software-download.microsoft.com/download/sg/20348.169.210806-2348.fe_release_svc_refresh_SERVER_EVAL_x64FRE_en-us.iso"
  }
}
```

## Configuring Input/User Locale & Timezone

To set the input/user locale and timezone according to your preferences edit the following files:

- ".\packer-Win2022\scripts\<bios|uefi>\core\autounattend.xml"
- ".\packer-Win2022\scripts\<bios|uefi>\gui\autounattend.xml"

```xml
<settings pass="specialize">
    <component xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" name="Microsoft-Windows-International-Core" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">
        <InputLocale>fr-FR</InputLocale>
        <UserLocale>fr-FR</UserLocale>
    </component>
    <component xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">
        <TimeZone>Romance Standard Time</TimeZone>
    </component>
</settings>
```

## Default credentials

The default credentials for this VM image are:

|Username|Password|
|--------|--------|
|administrator|packer|
