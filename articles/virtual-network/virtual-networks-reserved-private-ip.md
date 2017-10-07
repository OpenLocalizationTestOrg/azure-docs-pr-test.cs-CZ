---
title: "aaaStatic interní privátní IP - virtuální počítač Azure – Classic"
description: "Principy statické interní IP adresy (vyhrazené) a jak toomanage je"
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: 93444c6f-af1b-41f8-a035-77f5c0302bf0
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2016
ms.author: jdial
ms.openlocfilehash: 5abe1c59f2f3ed19bcf56c269dfe57ac32d4f601
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooset-a-static-internal-private-ip-address-using-powershell-classic"></a><span data-ttu-id="3e6df-103">Jak tooset interní statickou privátní IP adresy pomocí prostředí PowerShell (klasické)</span><span class="sxs-lookup"><span data-stu-id="3e6df-103">How tooset a static internal private IP address using PowerShell (Classic)</span></span>
<span data-ttu-id="3e6df-104">Ve většině případů nebude nutné toospecify statické interní IP adresu pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3e6df-104">In most cases, you won’t need toospecify a static internal IP address for your virtual machine.</span></span> <span data-ttu-id="3e6df-105">Virtuální počítače ve virtuální síti se automaticky zobrazí interní IP adresu z rozsahu, který určíte.</span><span class="sxs-lookup"><span data-stu-id="3e6df-105">VMs in a virtual network will automatically receive an internal IP address from a range that you specify.</span></span> <span data-ttu-id="3e6df-106">Ale v některých případech, zadat statickou IP adresu pro konkrétní virtuální počítač má smysl.</span><span class="sxs-lookup"><span data-stu-id="3e6df-106">But in certain cases, specifying a static IP address for a particular VM makes sense.</span></span> <span data-ttu-id="3e6df-107">Například pokud virtuální počítač má toorun DNS nebo bude řadič domény.</span><span class="sxs-lookup"><span data-stu-id="3e6df-107">For example, if your VM is going toorun DNS or will be a domain controller.</span></span> <span data-ttu-id="3e6df-108">Statické interní IP adresu zůstává hello virtuálních počítačů i prostřednictvím stavu zastavení nebo deprovision.</span><span class="sxs-lookup"><span data-stu-id="3e6df-108">A static internal IP address stays with hello VM even through a stop/deprovision state.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="3e6df-109">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="3e6df-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="3e6df-110">Tento článek se zabývá pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="3e6df-110">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="3e6df-111">Společnost Microsoft doporučuje, aby většina nových nasazení používala hello [modelu nasazení Resource Manager](virtual-networks-static-private-ip-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="3e6df-111">Microsoft recommends that most new deployments use hello [Resource Manager deployment model](virtual-networks-static-private-ip-arm-ps.md).</span></span>
> 
> 

## <a name="how-tooverify-if-a-specific-ip-address-is-available"></a><span data-ttu-id="3e6df-112">Jak tooverify, pokud je k dispozici konkrétní IP adresu</span><span class="sxs-lookup"><span data-stu-id="3e6df-112">How tooverify if a specific IP address is available</span></span>
<span data-ttu-id="3e6df-113">tooverify Pokud hello IP adresu *10.0.0.7* je k dispozici ve virtuální síti s názvem *TestVnet*, spusťte následující příkaz prostředí PowerShell hello a ověření hello hodnoty pro *IsAvailable*:</span><span class="sxs-lookup"><span data-stu-id="3e6df-113">tooverify if hello IP address *10.0.0.7* is available in a vnet named *TestVnet*, run hello following PowerShell command and verify hello value for *IsAvailable*:</span></span>

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 10.0.0.7 

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

> [!NOTE]
> <span data-ttu-id="3e6df-114">Pokud chcete, aby příkaz hello tootest výše v bezpečné prostředí, postupujte podle pokynů hello v [vytvoření virtuální sítě (klasické)](virtual-networks-create-vnet-classic-pportal.md) toocreate virtuální síť s názvem *TestVnet* a ujistěte se, používá hello  *10.0.0.0/8* adresní prostor.</span><span class="sxs-lookup"><span data-stu-id="3e6df-114">If you want tootest hello command above in a safe environment follow hello guidelines in [Create a virtual network (classic)](virtual-networks-create-vnet-classic-pportal.md) toocreate a vnet named *TestVnet* and ensure it uses hello *10.0.0.0/8* address space.</span></span>
> 
> 

## <a name="how-toospecify-a-static-internal-ip-when-creating-a-vm"></a><span data-ttu-id="3e6df-115">Jak toospecify statické interní IP při vytváření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="3e6df-115">How toospecify a static internal IP when creating a VM</span></span>
<span data-ttu-id="3e6df-116">Hello následující skript prostředí PowerShell vytvoří novou cloudovou službu s názvem *TestService*, načte bitovou kopii z Azure a pak vytvoří virtuální počítač s názvem *TestVM* v hello novou cloudovou službu pomocí hello načíst image Nastaví hello toobe virtuálního počítače v podsíti s názvem *Subnet-1*a nastaví *10.0.0.7* jako statické interní IP adresu pro hello virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="3e6df-116">hello PowerShell script below creates a new cloud service named *TestService*, then retrieves an image from Azure, then creates a VM named *TestVM* in hello new cloud service using hello retrieved image, sets hello VM toobe in a subnet named *Subnet-1*, and sets *10.0.0.7* as a static internal IP for hello VM:</span></span>

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
    | Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
    | Set-AzureSubnet –SubnetNames Subnet-1 `
    | Set-AzureStaticVNetIP -IPAddress 10.0.0.7 `
    | New-AzureVM -ServiceName "TestService" –VNetName TestVnet

## <a name="how-tooretrieve-static-internal-ip-information-for-a-vm"></a><span data-ttu-id="3e6df-117">Jak tooretrieve interní informace o statických IP pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="3e6df-117">How tooretrieve static internal IP information for a VM</span></span>
<span data-ttu-id="3e6df-118">tooview hello interní informace o statických IP pro hello virtuální počítač vytvořen pomocí skriptu hello výše, spusťte následující příkaz prostředí PowerShell hello a sledovat hello hodnoty pro *IpAddress*:</span><span class="sxs-lookup"><span data-stu-id="3e6df-118">tooview hello static internal IP information for hello VM created with hello script above, run hello following PowerShell command and observe hello values for *IpAddress*:</span></span>

    Get-AzureVM -Name TestVM -ServiceName TestService

    DeploymentName              : TestService
    Name                        : TestVM
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 10.0.0.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : TestVM
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : rsR2-797
    AvailabilitySetName         : 
    DNSName                     : http://testservice000.cloudapp.net/
    Status                      : Provisioning
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 
    PublicIPName                : 
    NetworkInterfaces           : {}
    ServiceName                 : TestService
    OperationDescription        : Get-AzureVM
    OperationId                 : 34c1560a62f0901ab75cde4fed8e8bd1
    OperationStatus             : OK

## <a name="how-tooremove-a-static-internal-ip-from-a-vm"></a><span data-ttu-id="3e6df-119">Jak tooremove statické interní IP z virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="3e6df-119">How tooremove a static internal IP from a VM</span></span>
<span data-ttu-id="3e6df-120">tooremove hello statické interní IP přidat toohello virtuálních počítačů ve skriptu hello výše, spusťte následující příkaz prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="3e6df-120">tooremove hello static internal IP added toohello VM in hello script above, run hello following PowerShell command:</span></span>

    Get-AzureVM -ServiceName TestService -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM

## <a name="how-tooadd-a-static-internal-ip-tooan-existing-vm"></a><span data-ttu-id="3e6df-121">Jak tooadd statické vnitřní tooan IP existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="3e6df-121">How tooadd a static internal IP tooan existing VM</span></span>
<span data-ttu-id="3e6df-122">tooadd statické vnitřní toohello IP virtuálních počítačů, které jsou vytvořené pomocí skriptu hello výše, o následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="3e6df-122">tooadd a static internal IP toohello VM created using hello script above, runt he following command:</span></span>

    Get-AzureVM -ServiceName TestService000 -Name TestVM `
    | Set-AzureStaticVNetIP -IPAddress 10.10.0.7 `
    | Update-AzureVM

## <a name="next-steps"></a><span data-ttu-id="3e6df-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3e6df-123">Next steps</span></span>
[<span data-ttu-id="3e6df-124">Vyhrazená IP adresa</span><span class="sxs-lookup"><span data-stu-id="3e6df-124">Reserved IP</span></span>](virtual-networks-reserved-public-ip.md)

[<span data-ttu-id="3e6df-125">Úrovni instance veřejnou IP adresu (splnění)</span><span class="sxs-lookup"><span data-stu-id="3e6df-125">Instance-Level Public IP (ILPIP)</span></span>](virtual-networks-instance-level-public-ip.md)

[<span data-ttu-id="3e6df-126">Vyhrazená IP adresa REST API</span><span class="sxs-lookup"><span data-stu-id="3e6df-126">Reserved IP REST APIs</span></span>](https://msdn.microsoft.com/library/azure/dn722420.aspx)

