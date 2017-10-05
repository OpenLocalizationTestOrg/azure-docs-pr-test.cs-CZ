---
title: "Řeší Azure úrovni Instance veřejnou IP adresu (klasická) | Microsoft Docs"
description: "Pochopit instance adresy úrovně veřejné IP (splnění) a jak spravovat pomocí prostředí PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: 07eef6ec-7dfe-4c4d-a2c2-be0abfb48ec5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2016
ms.author: jdial
ms.openlocfilehash: 773043f2841ec7539b0d49357dec6bcb9f4f78a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="instance-level-public-ip-classic-overview"></a><span data-ttu-id="51a0f-103">Přehled veřejné IP (klasické) instance</span><span class="sxs-lookup"><span data-stu-id="51a0f-103">Instance level public IP (Classic) overview</span></span>
<span data-ttu-id="51a0f-104">Instance úrovně veřejné IP (splnění) je veřejná IP adresa, kterou můžete přiřadit přímo k instanci role virtuálního počítače nebo cloudové služby, a nikoli pro cloudovou službu, která jsou umístěny ve vaší instanci virtuálního počítače nebo role.</span><span class="sxs-lookup"><span data-stu-id="51a0f-104">An instance level public IP (ILPIP) is a public IP address that you can assign directly to a VM or Cloud Services role instance, rather than to the cloud service that your VM or role instance reside in.</span></span> <span data-ttu-id="51a0f-105">SPLNĚNÍ neberou v místě z virtuální IP (VIP) přiřazený ke cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="51a0f-105">An ILPIP doesn’t take the place of the virtual IP (VIP) that is assigned to your cloud service.</span></span> <span data-ttu-id="51a0f-106">Místo toho je další IP adresu, která můžete použít pro připojení přímo k vaší instanci virtuálního počítače nebo role.</span><span class="sxs-lookup"><span data-stu-id="51a0f-106">Rather, it’s an additional IP address that you can use to connect directly to your VM or role instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="51a0f-107">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="51a0f-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="51a0f-108">Tento článek se věnuje použití klasického modelu nasazení.</span><span class="sxs-lookup"><span data-stu-id="51a0f-108">This article covers using the classic deployment model.</span></span> <span data-ttu-id="51a0f-109">Společnost Microsoft doporučuje vytváření virtuálních počítačů prostřednictvím Správce prostředků.</span><span class="sxs-lookup"><span data-stu-id="51a0f-109">Microsoft recommends creating VMs through Resource Manager.</span></span> <span data-ttu-id="51a0f-110">Musíte rozumět jak [IP adresy](virtual-network-ip-addresses-overview-classic.md) pracovní v Azure.</span><span class="sxs-lookup"><span data-stu-id="51a0f-110">Make sure you understand how [IP addresses](virtual-network-ip-addresses-overview-classic.md) work in Azure.</span></span>

![Rozdíl mezi splnění a virtuální IP adresy](./media/virtual-networks-instance-level-public-ip/Figure1.png)

<span data-ttu-id="51a0f-112">Jak je znázorněno na obrázku 1, Cloudová služba přistupuje pomocí virtuální IP adresu, když jednotlivé virtuální počítače jsou obvykle získat přístup pomocí virtuálních IP adres:&lt;číslo portu&gt;.</span><span class="sxs-lookup"><span data-stu-id="51a0f-112">As shown in Figure 1, the cloud service is accessed using a VIP, while the individual VMs are normally accessed using VIP:&lt;port number&gt;.</span></span> <span data-ttu-id="51a0f-113">Přiřazením splnění konkrétní virtuální počítač, tento virtuální počítač je přístupný přímo pomocí tuto IP adresu.</span><span class="sxs-lookup"><span data-stu-id="51a0f-113">By assigning an ILPIP to a specific VM, that VM can be accessed directly using that IP address.</span></span>

<span data-ttu-id="51a0f-114">Při vytváření cloudové služby v Azure, odpovídající záznamy DNS A automaticky vytvoří pro povolení přístupu ke službě prostřednictvím platný plně kvalifikovaný název domény (FQDN), místo použití skutečné VIP.</span><span class="sxs-lookup"><span data-stu-id="51a0f-114">When you create a cloud service in Azure, corresponding DNS A records are created automatically to allow access to the service through a fully qualified domain name (FQDN), instead of using the actual VIP.</span></span> <span data-ttu-id="51a0f-115">Stejný postup se stane pro splnění umožňuje přístup k instanci virtuálního počítače nebo role ve plně kvalifikovaný název domény místo splnění.</span><span class="sxs-lookup"><span data-stu-id="51a0f-115">The same process happens for an ILPIP, allowing access to the VM or role instance by FQDN instead of the ILPIP.</span></span> <span data-ttu-id="51a0f-116">Například pokud vytvoříte cloudové služby s názvem *contosoadservice*, a konfigurace webové role s názvem *contosoweb* se dvěma instancemi Azure zaregistruje následující záznamů A instancí:</span><span class="sxs-lookup"><span data-stu-id="51a0f-116">For instance, if you create a cloud service named *contosoadservice*, and you configure a web role named *contosoweb* with two instances, Azure registers the following A records for the instances:</span></span>

* <span data-ttu-id="51a0f-117">contosoweb\_IN_0.contosoadservice.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="51a0f-117">contosoweb\_IN_0.contosoadservice.cloudapp.net</span></span>
* <span data-ttu-id="51a0f-118">contosoweb\_IN_1.contosoadservice.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="51a0f-118">contosoweb\_IN_1.contosoadservice.cloudapp.net</span></span> 

> [!NOTE]
> <span data-ttu-id="51a0f-119">Můžete přiřadit pouze jeden splnění pro každou instanci virtuálního počítače nebo role.</span><span class="sxs-lookup"><span data-stu-id="51a0f-119">You can assign only one ILPIP for each VM or role instance.</span></span> <span data-ttu-id="51a0f-120">Můžete použít až 5 ILPIPs za předplatné.</span><span class="sxs-lookup"><span data-stu-id="51a0f-120">You can use up to 5 ILPIPs per subscription.</span></span> <span data-ttu-id="51a0f-121">ILPIPs nejsou podporovány u virtuálních počítačů více síťovými Kartami.</span><span class="sxs-lookup"><span data-stu-id="51a0f-121">ILPIPs are not supported for multi-NIC VMs.</span></span>
> 
> 

## <a name="why-would-i-request-an-ilpip"></a><span data-ttu-id="51a0f-122">Proč se splnění požadavku?</span><span class="sxs-lookup"><span data-stu-id="51a0f-122">Why would I request an ILPIP?</span></span>
<span data-ttu-id="51a0f-123">Pokud chcete být schopni se připojit k instanci virtuálního počítače nebo role pomocí přiřazené přímo na IP adresy, místo cloudu pomocí služby virtuální IP adresy:&lt;číslo portu&gt;, žádosti splnění virtuálního počítače nebo role instance.</span><span class="sxs-lookup"><span data-stu-id="51a0f-123">If you want to be able to connect to your VM or role instance by an IP address assigned directly to it, rather than using the cloud service VIP:&lt;port number&gt;, request an ILPIP for your VM or your role instance.</span></span>

* <span data-ttu-id="51a0f-124">**Aktivní FTP** -přiřazením splnění pro virtuální počítač mohl přijímat provoz z jakéhokoli portu.</span><span class="sxs-lookup"><span data-stu-id="51a0f-124">**Active FTP** - By assigning an ILPIP to a VM, it can receive traffic on any port.</span></span> <span data-ttu-id="51a0f-125">Koncové body nejsou nutné pro virtuální počítač přijímat provoz.</span><span class="sxs-lookup"><span data-stu-id="51a0f-125">Endpoints are not required for the VM to receive traffic.</span></span>  <span data-ttu-id="51a0f-126">(Https://en.wikipedia.org/wiki/File_Transfer_Protocol#Protocol_overview) [přehled protokolu FTP] naleznete podrobnosti o protokolu FTP.</span><span class="sxs-lookup"><span data-stu-id="51a0f-126">See (https://en.wikipedia.org/wiki/File_Transfer_Protocol#Protocol_overview)[FTP Protocol Overview] for details on the FTP protocol.</span></span>
* <span data-ttu-id="51a0f-127">**Odchozí IP** – odchozí přenosy pocházející z virtuálního počítače je namapovaná na splnění jako zdroj a splnění jednoznačně identifikuje virtuálního počítače na externí entity.</span><span class="sxs-lookup"><span data-stu-id="51a0f-127">**Outbound IP** - Outbound traffic originating from the VM is mapped to the ILPIP as the source and the ILPIP uniquely identifies the VM to external entities.</span></span>

> [!NOTE]
> <span data-ttu-id="51a0f-128">V minulosti adresu splnění se označuje jako veřejná adresa IP (PIP).</span><span class="sxs-lookup"><span data-stu-id="51a0f-128">In the past, an ILPIP address was referred to as a public IP (PIP) address.</span></span>
> 

## <a name="manage-an-ilpip-for-a-vm"></a><span data-ttu-id="51a0f-129">Správa splnění pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="51a0f-129">Manage an ILPIP for a VM</span></span>
<span data-ttu-id="51a0f-130">Následující úlohy umožňují vytvořit, přiřadit a odebrat ILPIPs z virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="51a0f-130">The following tasks enable you to create, assign, and remove ILPIPs from VMs:</span></span>

### <a name="how-to-request-an-ilpip-during-vm-creation-using-powershell"></a><span data-ttu-id="51a0f-131">Postup žádosti splnění během vytváření virtuálních počítačů pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="51a0f-131">How to request an ILPIP during VM creation using PowerShell</span></span>
<span data-ttu-id="51a0f-132">Následující skript prostředí PowerShell vytvoří cloudové služby s názvem *FTPService*, načte bitovou kopii z Azure, vytvoří virtuální počítač s názvem *FTPInstance* pomocí načtené bitové kopie, nastaví virtuálního počítače používat splnění a přidá virtuální počítač do nové služby:</span><span class="sxs-lookup"><span data-stu-id="51a0f-132">The following PowerShell script creates a cloud service named *FTPService*, retrieves an image from Azure, creates a VM named *FTPInstance* using the retrieved image, sets the VM to use an ILPIP, and adds the VM to the new service:</span></span>

```powershell
New-AzureService -ServiceName FTPService -Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"} `
New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"
```

### <a name="how-to-retrieve-ilpip-information-for-a-vm"></a><span data-ttu-id="51a0f-133">Jak načíst informace o splnění pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="51a0f-133">How to retrieve ILPIP information for a VM</span></span>
<span data-ttu-id="51a0f-134">Chcete-li zobrazit informace o splnění pro virtuální počítač vytvořený s předchozí skriptu, spusťte následující příkaz prostředí PowerShell a sledovat hodnoty *PublicIPAddress* a *PublicIPName*:</span><span class="sxs-lookup"><span data-stu-id="51a0f-134">To view the ILPIP information for the VM created with the previous script, run the following PowerShell command and observe the values for *PublicIPAddress* and *PublicIPName*:</span></span>

```powershell
Get-AzureVM -Name FTPInstance -ServiceName FTPService
```

<span data-ttu-id="51a0f-135">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="51a0f-135">Expected output:</span></span>
 
    DeploymentName              : FTPService
    Name                        : FTPInstance
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : ReadyRole
    IpAddress                   : 100.74.118.91
    InstanceStateDetails        : 
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : FTPInstance
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : FTPInstance
    AvailabilitySetName         : 
    DNSName                     : http://ftpservice888.cloudapp.net/
    Status                      : ReadyRole
    GuestAgentStatus            :   Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 104.43.142.188
    PublicIPName                : ftpip
    NetworkInterfaces           : {}
    ServiceName                 : FTPService
    OperationDescription        : Get-AzureVM
    OperationId                 : 568d88d2be7c98f4bbb875e4d823718e
    OperationStatus             : OK

### <a name="how-to-remove-an-ilpip-from-a-vm"></a><span data-ttu-id="51a0f-136">Postup odebrání splnění z virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="51a0f-136">How to remove an ILPIP from a VM</span></span>
<span data-ttu-id="51a0f-137">Chcete-li odebrat splnění přidat do virtuálního počítače v předchozí skriptu, spusťte následující příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="51a0f-137">To remove the ILPIP added to the VM in the previous script, run the following PowerShell command:</span></span>

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Remove-AzurePublicIP | Update-AzureVM
```

### <a name="how-to-add-an-ilpip-to-an-existing-vm"></a><span data-ttu-id="51a0f-138">Postup přidání splnění do stávajícího virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="51a0f-138">How to add an ILPIP to an existing VM</span></span>
<span data-ttu-id="51a0f-139">Pokud chcete přidat splnění na virtuální počítač vytvořený skript předchozí, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="51a0f-139">To add an ILPIP to the VM created using the script previous, run the following command:</span></span>

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Set-AzurePublicIP -PublicIPName ftpip2 | Update-AzureVM
```

## <a name="manage-an-ilpip-for-a-cloud-services-role-instance"></a><span data-ttu-id="51a0f-140">Správa splnění pro instanci role cloudové služby</span><span class="sxs-lookup"><span data-stu-id="51a0f-140">Manage an ILPIP for a Cloud Services role instance</span></span>

<span data-ttu-id="51a0f-141">Pokud chcete přidat splnění instanci role cloudové služby, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="51a0f-141">To add an ILPIP to a Cloud Services role instance, complete the following steps:</span></span>

1. <span data-ttu-id="51a0f-142">Stáhnout pomocí kroků v souboru .cscfg pro cloudové služby [postup konfigurace cloudové služby](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) článku.</span><span class="sxs-lookup"><span data-stu-id="51a0f-142">Download the .cscfg file for the cloud service by completing the steps in the [How to Configure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) article.</span></span>
2. <span data-ttu-id="51a0f-143">Aktualizovat přidáním souboru .cscfg `InstanceAddress` elementu.</span><span class="sxs-lookup"><span data-stu-id="51a0f-143">Update the .cscfg file by adding the `InstanceAddress` element.</span></span> <span data-ttu-id="51a0f-144">Následující příklad přidá splnění s názvem *MyPublicIP* role instance s názvem *WebRole1*:</span><span class="sxs-lookup"><span data-stu-id="51a0f-144">The following sample adds an ILPIP named *MyPublicIP* to a role instance named *WebRole1*:</span></span> 

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ILPIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
          <ConfigurationSettings>
        <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
          </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <InstanceAddress roleName="WebRole1">
        <PublicIPs>
          <PublicIP name="MyPublicIP" domainNameLabel="MyPublicIP" />
            </PublicIPs>
          </InstanceAddress>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>
    ```
3. <span data-ttu-id="51a0f-145">Nahrát pomocí kroků v souboru .cscfg pro cloudové služby [postup konfigurace cloudové služby](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) článku.</span><span class="sxs-lookup"><span data-stu-id="51a0f-145">Upload the .cscfg file for the cloud service by completing the steps in the [How to Configure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) article.</span></span>

## <a name="next-steps"></a><span data-ttu-id="51a0f-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="51a0f-146">Next steps</span></span>
* <span data-ttu-id="51a0f-147">Pochopit, jak [IP adresování](virtual-network-ip-addresses-overview-classic.md) funguje v modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="51a0f-147">Understand how [IP addressing](virtual-network-ip-addresses-overview-classic.md) works in the classic deployment model.</span></span>
* <span data-ttu-id="51a0f-148">Další informace o [vyhrazené IP adresy](virtual-networks-reserved-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="51a0f-148">Learn about [Reserved IPs](virtual-networks-reserved-public-ip.md).</span></span>
