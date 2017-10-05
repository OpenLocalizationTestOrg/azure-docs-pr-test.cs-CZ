---
title: "Statické interní privátní IP - virtuální počítač Azure – Classic"
description: "Princip statické interní IP adresy (vyhrazené) a způsobu jejich správy"
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
ms.openlocfilehash: cf9ee59ca4e44ed01836c2efb1f4df5f073bf6e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-set-a-static-internal-private-ip-address-using-powershell-classic"></a><span data-ttu-id="b7b36-103">Jak nastavit statickou interní privátní IP adresu pomocí prostředí PowerShell (klasické)</span><span class="sxs-lookup"><span data-stu-id="b7b36-103">How to set a static internal private IP address using PowerShell (Classic)</span></span>
<span data-ttu-id="b7b36-104">Ve většině případů nebude muset zadat statické interní IP adresu pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b7b36-104">In most cases, you won’t need to specify a static internal IP address for your virtual machine.</span></span> <span data-ttu-id="b7b36-105">Virtuální počítače ve virtuální síti se automaticky zobrazí interní IP adresu z rozsahu, který určíte.</span><span class="sxs-lookup"><span data-stu-id="b7b36-105">VMs in a virtual network will automatically receive an internal IP address from a range that you specify.</span></span> <span data-ttu-id="b7b36-106">Ale v některých případech, zadat statickou IP adresu pro konkrétní virtuální počítač má smysl.</span><span class="sxs-lookup"><span data-stu-id="b7b36-106">But in certain cases, specifying a static IP address for a particular VM makes sense.</span></span> <span data-ttu-id="b7b36-107">Například pokud virtuální počítač se blíží ke spuštění DNS nebo bude řadič domény.</span><span class="sxs-lookup"><span data-stu-id="b7b36-107">For example, if your VM is going to run DNS or will be a domain controller.</span></span> <span data-ttu-id="b7b36-108">Statické interní IP adresu zůstává s virtuálním Počítačem i prostřednictvím stavu zastavení nebo deprovision.</span><span class="sxs-lookup"><span data-stu-id="b7b36-108">A static internal IP address stays with the VM even through a stop/deprovision state.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="b7b36-109">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="b7b36-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="b7b36-110">Tento článek se věnuje použití klasického modelu nasazení.</span><span class="sxs-lookup"><span data-stu-id="b7b36-110">This article covers using the classic deployment model.</span></span> <span data-ttu-id="b7b36-111">Společnost Microsoft doporučuje, aby většina nových nasazení používala [modelu nasazení Resource Manager](virtual-networks-static-private-ip-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="b7b36-111">Microsoft recommends that most new deployments use the [Resource Manager deployment model](virtual-networks-static-private-ip-arm-ps.md).</span></span>
> 
> 

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a><span data-ttu-id="b7b36-112">Postup ověření, pokud je k dispozici konkrétní IP adresu</span><span class="sxs-lookup"><span data-stu-id="b7b36-112">How to verify if a specific IP address is available</span></span>
<span data-ttu-id="b7b36-113">Chcete-li ověřit, pokud IP adresa *10.0.0.7* je k dispozici ve virtuální síti s názvem *TestVnet*, spusťte následující příkaz prostředí PowerShell a potom ověřte hodnotu pro *IsAvailable*:</span><span class="sxs-lookup"><span data-stu-id="b7b36-113">To verify if the IP address *10.0.0.7* is available in a vnet named *TestVnet*, run the following PowerShell command and verify the value for *IsAvailable*:</span></span>

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 10.0.0.7 

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

> [!NOTE]
> <span data-ttu-id="b7b36-114">Pokud chcete testovat příkaz výše v bezpečné prostředí, postupujte podle pokynů v [vytvoření virtuální sítě (klasické)](virtual-networks-create-vnet-classic-pportal.md) chcete vytvořit síť vnet s názvem *TestVnet* a ujistěte se, použije *10.0.0.0/8* adresní prostor.</span><span class="sxs-lookup"><span data-stu-id="b7b36-114">If you want to test the command above in a safe environment follow the guidelines in [Create a virtual network (classic)](virtual-networks-create-vnet-classic-pportal.md) to create a vnet named *TestVnet* and ensure it uses the *10.0.0.0/8* address space.</span></span>
> 
> 

## <a name="how-to-specify-a-static-internal-ip-when-creating-a-vm"></a><span data-ttu-id="b7b36-115">Určení statické interní IP při vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b7b36-115">How to specify a static internal IP when creating a VM</span></span>
<span data-ttu-id="b7b36-116">Následující skript prostředí PowerShell vytvoří novou cloudovou službu s názvem *TestService*, načte bitovou kopii z Azure a pak vytvoří virtuální počítač s názvem *TestVM* v novou cloudovou službu pomocí načtené bitové kopie, nastaví virtuálního počítače v podsíti s názvem *Subnet-1*a nastaví *10.0.0.7* jako statické interní IP adresu pro virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="b7b36-116">The PowerShell script below creates a new cloud service named *TestService*, then retrieves an image from Azure, then creates a VM named *TestVM* in the new cloud service using the retrieved image, sets the VM to be in a subnet named *Subnet-1*, and sets *10.0.0.7* as a static internal IP for the VM:</span></span>

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
    | Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
    | Set-AzureSubnet –SubnetNames Subnet-1 `
    | Set-AzureStaticVNetIP -IPAddress 10.0.0.7 `
    | New-AzureVM -ServiceName "TestService" –VNetName TestVnet

## <a name="how-to-retrieve-static-internal-ip-information-for-a-vm"></a><span data-ttu-id="b7b36-117">Jak načíst interní informace o statických IP pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="b7b36-117">How to retrieve static internal IP information for a VM</span></span>
<span data-ttu-id="b7b36-118">Chcete-li zobrazit informace o statické interní IP pro virtuální počítač vytvořený pomocí skriptu pro výše uvedené, spusťte následující příkaz prostředí PowerShell a sledovat hodnoty *IpAddress*:</span><span class="sxs-lookup"><span data-stu-id="b7b36-118">To view the static internal IP information for the VM created with the script above, run the following PowerShell command and observe the values for *IpAddress*:</span></span>

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

## <a name="how-to-remove-a-static-internal-ip-from-a-vm"></a><span data-ttu-id="b7b36-119">Postup odebrání statické interní IP z virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b7b36-119">How to remove a static internal IP from a VM</span></span>
<span data-ttu-id="b7b36-120">Chcete-li odebrat statické interní IP přidat do virtuálního počítače ve výše uvedené skriptu, spusťte následující příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="b7b36-120">To remove the static internal IP added to the VM in the script above, run the following PowerShell command:</span></span>

    Get-AzureVM -ServiceName TestService -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM

## <a name="how-to-add-a-static-internal-ip-to-an-existing-vm"></a><span data-ttu-id="b7b36-121">Jak přidat do stávajícího virtuálního počítače statické interní IP</span><span class="sxs-lookup"><span data-stu-id="b7b36-121">How to add a static internal IP to an existing VM</span></span>
<span data-ttu-id="b7b36-122">Přidání statické interní IP adresy na virtuální počítač vytvořený skript výše, o následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b7b36-122">To add a static internal IP to the VM created using the script above, runt he following command:</span></span>

    Get-AzureVM -ServiceName TestService000 -Name TestVM `
    | Set-AzureStaticVNetIP -IPAddress 10.10.0.7 `
    | Update-AzureVM

## <a name="next-steps"></a><span data-ttu-id="b7b36-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b7b36-123">Next steps</span></span>
[<span data-ttu-id="b7b36-124">Vyhrazená IP adresa</span><span class="sxs-lookup"><span data-stu-id="b7b36-124">Reserved IP</span></span>](virtual-networks-reserved-public-ip.md)

[<span data-ttu-id="b7b36-125">Úrovni instance veřejnou IP adresu (splnění)</span><span class="sxs-lookup"><span data-stu-id="b7b36-125">Instance-Level Public IP (ILPIP)</span></span>](virtual-networks-instance-level-public-ip.md)

[<span data-ttu-id="b7b36-126">Vyhrazená IP adresa REST API</span><span class="sxs-lookup"><span data-stu-id="b7b36-126">Reserved IP REST APIs</span></span>](https://msdn.microsoft.com/library/azure/dn722420.aspx)

