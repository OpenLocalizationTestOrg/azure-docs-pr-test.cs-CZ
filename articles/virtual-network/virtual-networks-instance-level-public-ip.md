---
title: "adresy aaaAzure úrovni Instance veřejnou IP adresu (klasická) | Microsoft Docs"
description: "Pochopení adresy IP (splnění) úrovni veřejné instance a jak toomanage je pomocí prostředí PowerShell."
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
ms.openlocfilehash: 832143ee6fdd80b634e1cebfddc759a8cacda583
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="instance-level-public-ip-classic-overview"></a><span data-ttu-id="2e2f2-103">Přehled veřejné IP (klasické) instance</span><span class="sxs-lookup"><span data-stu-id="2e2f2-103">Instance level public IP (Classic) overview</span></span>
<span data-ttu-id="2e2f2-104">Instance úrovně veřejné IP (splnění) je veřejná IP adresa, kterou můžete přiřadit přímo instanci role virtuálního počítače nebo cloudové služby tooa, nikoli toohello Cloudová služba, která jsou umístěny ve vaší instanci virtuálního počítače nebo role.</span><span class="sxs-lookup"><span data-stu-id="2e2f2-104">An instance level public IP (ILPIP) is a public IP address that you can assign directly tooa VM or Cloud Services role instance, rather than toohello cloud service that your VM or role instance reside in.</span></span> <span data-ttu-id="2e2f2-105">SPLNĚNÍ neberou v místě hello hello virtuální IP (VIP) přiřazený tooyour cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="2e2f2-105">An ILPIP doesn’t take hello place of hello virtual IP (VIP) that is assigned tooyour cloud service.</span></span> <span data-ttu-id="2e2f2-106">Místo toho je další IP adresu, které můžete použít tooconnect přímo tooyour virtuálního počítače nebo instanci role.</span><span class="sxs-lookup"><span data-stu-id="2e2f2-106">Rather, it’s an additional IP address that you can use tooconnect directly tooyour VM or role instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2e2f2-107">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2e2f2-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="2e2f2-108">Tento článek se zabývá pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="2e2f2-108">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="2e2f2-109">Společnost Microsoft doporučuje vytváření virtuálních počítačů prostřednictvím Správce prostředků.</span><span class="sxs-lookup"><span data-stu-id="2e2f2-109">Microsoft recommends creating VMs through Resource Manager.</span></span> <span data-ttu-id="2e2f2-110">Musíte rozumět jak [IP adresy](virtual-network-ip-addresses-overview-classic.md) pracovní v Azure.</span><span class="sxs-lookup"><span data-stu-id="2e2f2-110">Make sure you understand how [IP addresses](virtual-network-ip-addresses-overview-classic.md) work in Azure.</span></span>

![Rozdíl mezi splnění a virtuální IP adresy](./media/virtual-networks-instance-level-public-ip/Figure1.png)

<span data-ttu-id="2e2f2-112">Jak je znázorněno na obrázku 1, hello Cloudová služba je získat přístup pomocí virtuální IP adresu, při hello jednotlivé virtuální počítače jsou obvykle získat přístup pomocí virtuálních IP adres:&lt;číslo portu&gt;.</span><span class="sxs-lookup"><span data-stu-id="2e2f2-112">As shown in Figure 1, hello cloud service is accessed using a VIP, while hello individual VMs are normally accessed using VIP:&lt;port number&gt;.</span></span> <span data-ttu-id="2e2f2-113">Přiřazením splnění tooa konkrétní virtuální počítač, který může být virtuální počítač získat přístup, přímo pomocí tuto IP adresu.</span><span class="sxs-lookup"><span data-stu-id="2e2f2-113">By assigning an ILPIP tooa specific VM, that VM can be accessed directly using that IP address.</span></span>

<span data-ttu-id="2e2f2-114">Při vytváření cloudové služby v Azure, odpovídající záznamy DNS A automaticky vytvoří služba toohello tooallow přístup prostřednictvím platný plně kvalifikovaný název domény (FQDN), místo použití hello skutečné VIP.</span><span class="sxs-lookup"><span data-stu-id="2e2f2-114">When you create a cloud service in Azure, corresponding DNS A records are created automatically tooallow access toohello service through a fully qualified domain name (FQDN), instead of using hello actual VIP.</span></span> <span data-ttu-id="2e2f2-115">Dobrý den, který se stane, stejný proces pro splnění povolení přístupu toohello virtuálního počítače nebo instanci role ve plně kvalifikovaný název domény místo hello splnění.</span><span class="sxs-lookup"><span data-stu-id="2e2f2-115">hello same process happens for an ILPIP, allowing access toohello VM or role instance by FQDN instead of hello ILPIP.</span></span> <span data-ttu-id="2e2f2-116">Například pokud vytvoříte cloudové služby s názvem *contosoadservice*, a konfigurace webové role s názvem *contosoweb* se dvěma instancemi Azure registry hello následující A zaznamenává pro instance hello:</span><span class="sxs-lookup"><span data-stu-id="2e2f2-116">For instance, if you create a cloud service named *contosoadservice*, and you configure a web role named *contosoweb* with two instances, Azure registers hello following A records for hello instances:</span></span>

* <span data-ttu-id="2e2f2-117">contosoweb\_IN_0.contosoadservice.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="2e2f2-117">contosoweb\_IN_0.contosoadservice.cloudapp.net</span></span>
* <span data-ttu-id="2e2f2-118">contosoweb\_IN_1.contosoadservice.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="2e2f2-118">contosoweb\_IN_1.contosoadservice.cloudapp.net</span></span> 

> [!NOTE]
> <span data-ttu-id="2e2f2-119">Můžete přiřadit pouze jeden splnění pro každou instanci virtuálního počítače nebo role.</span><span class="sxs-lookup"><span data-stu-id="2e2f2-119">You can assign only one ILPIP for each VM or role instance.</span></span> <span data-ttu-id="2e2f2-120">Můžete si too5 ILPIPs za předplatné.</span><span class="sxs-lookup"><span data-stu-id="2e2f2-120">You can use up too5 ILPIPs per subscription.</span></span> <span data-ttu-id="2e2f2-121">ILPIPs nejsou podporovány u virtuálních počítačů více síťovými Kartami.</span><span class="sxs-lookup"><span data-stu-id="2e2f2-121">ILPIPs are not supported for multi-NIC VMs.</span></span>
> 
> 

## <a name="why-would-i-request-an-ilpip"></a><span data-ttu-id="2e2f2-122">Proč se splnění požadavku?</span><span class="sxs-lookup"><span data-stu-id="2e2f2-122">Why would I request an ILPIP?</span></span>
<span data-ttu-id="2e2f2-123">Pokud chcete toobe možné tooconnect tooyour virtuálního počítače nebo instance role pomocí IP adresy přiřazené přímo tooit, místo použití hello cloudové služby virtuálních IP adres:&lt;číslo portu&gt;, žádosti splnění virtuálního počítače nebo role instance.</span><span class="sxs-lookup"><span data-stu-id="2e2f2-123">If you want toobe able tooconnect tooyour VM or role instance by an IP address assigned directly tooit, rather than using hello cloud service VIP:&lt;port number&gt;, request an ILPIP for your VM or your role instance.</span></span>

* <span data-ttu-id="2e2f2-124">**Aktivní FTP** -přiřazením splnění tooa virtuální počítač mohl přijímat provoz z jakéhokoli portu.</span><span class="sxs-lookup"><span data-stu-id="2e2f2-124">**Active FTP** - By assigning an ILPIP tooa VM, it can receive traffic on any port.</span></span> <span data-ttu-id="2e2f2-125">Koncové body nejsou potřeba pro hello tooreceive přenosy virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2e2f2-125">Endpoints are not required for hello VM tooreceive traffic.</span></span>  <span data-ttu-id="2e2f2-126">Podrobnosti v protokolu hello FTP najdete v (https://en.wikipedia.org/wiki/File_Transfer_Protocol#Protocol_overview) [přehled protokolu FTP].</span><span class="sxs-lookup"><span data-stu-id="2e2f2-126">See (https://en.wikipedia.org/wiki/File_Transfer_Protocol#Protocol_overview)[FTP Protocol Overview] for details on hello FTP protocol.</span></span>
* <span data-ttu-id="2e2f2-127">**Odchozí IP** – odchozí přenosy pocházející z hello virtuálního počítače je mapovaná toohello splnění jako zdroj hello a hello splnění jednoznačně identifikuje entity tooexternal hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2e2f2-127">**Outbound IP** - Outbound traffic originating from hello VM is mapped toohello ILPIP as hello source and hello ILPIP uniquely identifies hello VM tooexternal entities.</span></span>

> [!NOTE]
> <span data-ttu-id="2e2f2-128">V posledních hello byla adresu splnění tooas odkazuje na veřejnou adresu IP (PIP).</span><span class="sxs-lookup"><span data-stu-id="2e2f2-128">In hello past, an ILPIP address was referred tooas a public IP (PIP) address.</span></span>
> 

## <a name="manage-an-ilpip-for-a-vm"></a><span data-ttu-id="2e2f2-129">Správa splnění pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="2e2f2-129">Manage an ILPIP for a VM</span></span>
<span data-ttu-id="2e2f2-130">Hello následující úlohy umožňují toocreate, přiřazení a odebrání ILPIPs z virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="2e2f2-130">hello following tasks enable you toocreate, assign, and remove ILPIPs from VMs:</span></span>

### <a name="how-toorequest-an-ilpip-during-vm-creation-using-powershell"></a><span data-ttu-id="2e2f2-131">Jak toorequest splnění během vytváření virtuálních počítačů pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2e2f2-131">How toorequest an ILPIP during VM creation using PowerShell</span></span>
<span data-ttu-id="2e2f2-132">Hello následující skript prostředí PowerShell vytvoří cloudové služby s názvem *FTPService*, načte bitovou kopii z Azure, vytvoří virtuální počítač s názvem *FTPInstance* pomocí hello načíst image, nastaví toouse hello virtuálních počítačů splnění a přidá novou službu toohello hello virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="2e2f2-132">hello following PowerShell script creates a cloud service named *FTPService*, retrieves an image from Azure, creates a VM named *FTPInstance* using hello retrieved image, sets hello VM toouse an ILPIP, and adds hello VM toohello new service:</span></span>

```powershell
New-AzureService -ServiceName FTPService -Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"} `
New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"
```

### <a name="how-tooretrieve-ilpip-information-for-a-vm"></a><span data-ttu-id="2e2f2-133">Jak tooretrieve splnění informace pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="2e2f2-133">How tooretrieve ILPIP information for a VM</span></span>
<span data-ttu-id="2e2f2-134">tooview hello splnění informace pro hello virtuální počítač vytvořen pomocí hello předchozí skriptu, spusťte následující příkaz prostředí PowerShell hello a sledovat hello hodnoty pro *PublicIPAddress* a *PublicIPName*:</span><span class="sxs-lookup"><span data-stu-id="2e2f2-134">tooview hello ILPIP information for hello VM created with hello previous script, run hello following PowerShell command and observe hello values for *PublicIPAddress* and *PublicIPName*:</span></span>

```powershell
Get-AzureVM -Name FTPInstance -ServiceName FTPService
```

<span data-ttu-id="2e2f2-135">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="2e2f2-135">Expected output:</span></span>
 
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

### <a name="how-tooremove-an-ilpip-from-a-vm"></a><span data-ttu-id="2e2f2-136">Jak tooremove splnění z virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="2e2f2-136">How tooremove an ILPIP from a VM</span></span>
<span data-ttu-id="2e2f2-137">tooremove hello splnění přidali toohello virtuálních počítačů v předchozí hello skriptu, spusťte následující příkaz prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="2e2f2-137">tooremove hello ILPIP added toohello VM in hello previous script, run hello following PowerShell command:</span></span>

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Remove-AzurePublicIP | Update-AzureVM
```

### <a name="how-tooadd-an-ilpip-tooan-existing-vm"></a><span data-ttu-id="2e2f2-138">Jak tooadd tooan splnění existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="2e2f2-138">How tooadd an ILPIP tooan existing VM</span></span>
<span data-ttu-id="2e2f2-139">tooadd splnění toohello virtuální počítač vytvořený pomocí skriptu hello předchozí, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="2e2f2-139">tooadd an ILPIP toohello VM created using hello script previous, run hello following command:</span></span>

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Set-AzurePublicIP -PublicIPName ftpip2 | Update-AzureVM
```

## <a name="manage-an-ilpip-for-a-cloud-services-role-instance"></a><span data-ttu-id="2e2f2-140">Správa splnění pro instanci role cloudové služby</span><span class="sxs-lookup"><span data-stu-id="2e2f2-140">Manage an ILPIP for a Cloud Services role instance</span></span>

<span data-ttu-id="2e2f2-141">tooadd instance role splnění tooa cloudové služby, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2e2f2-141">tooadd an ILPIP tooa Cloud Services role instance, complete hello following steps:</span></span>

1. <span data-ttu-id="2e2f2-142">Stáhněte si soubor .cscfg hello pro cloudové služby hello provedením hello kroky hello [jak tooConfigure cloudových služeb](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) článku.</span><span class="sxs-lookup"><span data-stu-id="2e2f2-142">Download hello .cscfg file for hello cloud service by completing hello steps in hello [How tooConfigure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) article.</span></span>
2. <span data-ttu-id="2e2f2-143">Aktualizace souboru .cscfg hello přidáním hello `InstanceAddress` elementu.</span><span class="sxs-lookup"><span data-stu-id="2e2f2-143">Update hello .cscfg file by adding hello `InstanceAddress` element.</span></span> <span data-ttu-id="2e2f2-144">Hello následující příklad přidá splnění s názvem *MyPublicIP* tooa instance role s názvem *WebRole1*:</span><span class="sxs-lookup"><span data-stu-id="2e2f2-144">hello following sample adds an ILPIP named *MyPublicIP* tooa role instance named *WebRole1*:</span></span> 

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
3. <span data-ttu-id="2e2f2-145">Nahrát soubor .cscfg hello pro cloudové služby hello provedením hello kroky hello [jak tooConfigure cloudových služeb](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) článku.</span><span class="sxs-lookup"><span data-stu-id="2e2f2-145">Upload hello .cscfg file for hello cloud service by completing hello steps in hello [How tooConfigure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) article.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2e2f2-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2e2f2-146">Next steps</span></span>
* <span data-ttu-id="2e2f2-147">Pochopit, jak [IP adresování](virtual-network-ip-addresses-overview-classic.md) funguje v modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="2e2f2-147">Understand how [IP addressing](virtual-network-ip-addresses-overview-classic.md) works in hello classic deployment model.</span></span>
* <span data-ttu-id="2e2f2-148">Další informace o [vyhrazené IP adresy](virtual-networks-reserved-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="2e2f2-148">Learn about [Reserved IPs](virtual-networks-reserved-public-ip.md).</span></span>
