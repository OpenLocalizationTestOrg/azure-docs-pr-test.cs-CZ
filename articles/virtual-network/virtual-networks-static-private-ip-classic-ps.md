---
title: "aaaConfigure privátních IP adres pro virtuální počítače (klasické) - prostředí Azure PowerShell | Microsoft Docs"
description: "Zjistěte, jak tooconfigure privátní IP adresy pro virtuální počítače (klasické) pomocí prostředí PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 60c7b489-46ae-48af-a453-2b429a474afd
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 99546ee9c2c4eb9aa7b67f30721d37ef9b2944f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-powershell"></a><span data-ttu-id="4f311-103">Nakonfigurovat privátní IP adresy pro virtuální počítač (klasický) pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="4f311-103">Configure private IP addresses for a virtual machine (Classic) using PowerShell</span></span>

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="4f311-104">Tento článek se týká modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="4f311-104">This article covers hello classic deployment model.</span></span> <span data-ttu-id="4f311-105">Můžete také [spravovat statickou privátní IP adresou v modelu nasazení Resource Manager hello](virtual-networks-static-private-ip-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="4f311-105">You can also [manage a static private IP address in hello Resource Manager deployment model](virtual-networks-static-private-ip-arm-ps.md).</span></span>

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

<span data-ttu-id="4f311-106">Následující příkazy prostředí PowerShell ukázkový Hello očekávat jednoduché prostředí již vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="4f311-106">hello sample PowerShell commands below expect a simple environment already created.</span></span> <span data-ttu-id="4f311-107">Pokud chcete příkazy hello toorun, jak jsou zobrazeny v tomto dokumentu, nejprve sestavení hello testovací prostředí popsané v [vytvoření virtuální sítě](virtual-networks-create-vnet-classic-netcfg-ps.md).</span><span class="sxs-lookup"><span data-stu-id="4f311-107">If you want toorun hello commands as they are displayed in this document, first build hello test environment described in [Create a VNet](virtual-networks-create-vnet-classic-netcfg-ps.md).</span></span>

## <a name="how-tooverify-if-a-specific-ip-address-is-available"></a><span data-ttu-id="4f311-108">Jak tooverify, pokud je k dispozici konkrétní IP adresu</span><span class="sxs-lookup"><span data-stu-id="4f311-108">How tooverify if a specific IP address is available</span></span>
<span data-ttu-id="4f311-109">tooverify Pokud hello IP adresu *192.168.1.101* je k dispozici ve virtuální síti s názvem *TestVNet*, spusťte následující příkaz prostředí PowerShell hello a ověření hello hodnoty pro *IsAvailable*:</span><span class="sxs-lookup"><span data-stu-id="4f311-109">tooverify if hello IP address *192.168.1.101* is available in a VNet named *TestVNet*, run hello following PowerShell command and verify hello value for *IsAvailable*:</span></span>

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 192.168.1.101 

<span data-ttu-id="4f311-110">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="4f311-110">Expected output:</span></span>

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

## <a name="how-toospecify-a-static-private-ip-address-when-creating-a-vm"></a><span data-ttu-id="4f311-111">Jak toospecify statickou privátní IP adresy při vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="4f311-111">How toospecify a static private IP address when creating a VM</span></span>
<span data-ttu-id="4f311-112">Hello následující skript prostředí PowerShell vytvoří novou cloudovou službu s názvem *TestService*, potom načte bitovou kopii z Azure, vytvoří virtuální počítač s názvem *DNS01* v hello novou cloudovou službu pomocí hello načíst image, nastaví Hello toobe virtuálního počítače v podsíti s názvem *front-endu*a nastaví *192.168.1.7* jako statickou privátní IP adresou pro hello virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="4f311-112">hello PowerShell script below creates a new cloud service named *TestService*, then retrieves an image from Azure, creates a VM named *DNS01* in hello new cloud service using hello retrieved image, sets hello VM toobe in a subnet named *FrontEnd*, and sets *192.168.1.7* as a static private IP address for hello VM:</span></span>

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage | where {$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name DNS01 -InstanceSize Small -ImageName $image.ImageName |
      Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! |
      Set-AzureSubnet –SubnetNames FrontEnd |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      New-AzureVM -ServiceName TestService –VNetName TestVNet

<span data-ttu-id="4f311-113">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="4f311-113">Expected output:</span></span>

    WARNING: No deployment found in service: 'TestService'.
    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    New-AzureService     fcf705f1-d902-011c-95c7-b690735e7412 Succeeded      
    New-AzureVM          3b99a86d-84f8-04e5-888e-b6fc3c73c4b9 Succeeded  

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a><span data-ttu-id="4f311-114">Jak tooretrieve statickou privátní IP adresu pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="4f311-114">How tooretrieve static private IP address information for a VM</span></span>
<span data-ttu-id="4f311-115">tooview hello statickou privátní IP adresu pro hello virtuální počítač vytvořen pomocí skriptu hello výše, spusťte následující příkaz prostředí PowerShell hello a sledovat hello hodnoty pro *IpAddress*:</span><span class="sxs-lookup"><span data-stu-id="4f311-115">tooview hello static private IP address information for hello VM created with hello script above, run hello following PowerShell command and observe hello values for *IpAddress*:</span></span>

    Get-AzureVM -Name DNS01 -ServiceName TestService

<span data-ttu-id="4f311-116">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="4f311-116">Expected output:</span></span>

    DeploymentName              : TestService
    Name                        : DNS01
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 192.168.1.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : DNS01
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

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a><span data-ttu-id="4f311-117">Jak tooremove statickou privátní IP adresu z virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="4f311-117">How tooremove a static private IP address from a VM</span></span>
<span data-ttu-id="4f311-118">tooremove hello statickou privátní IP adresou přidána toohello virtuálních počítačů ve skriptu hello nad spuštění hello následující příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="4f311-118">tooremove hello static private IP address added toohello VM in hello script above, run hello following PowerShell command:</span></span>

    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Remove-AzureStaticVNetIP |
      Update-AzureVM

<span data-ttu-id="4f311-119">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="4f311-119">Expected output:</span></span>

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       052fa6f6-1483-0ede-a7bf-14f91f805483 Succeeded

## <a name="how-tooadd-a-static-private-ip-address-tooan-existing-vm"></a><span data-ttu-id="4f311-120">Jak tooadd statickou privátní IP adresa tooan existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="4f311-120">How tooadd a static private IP address tooan existing VM</span></span>
<span data-ttu-id="4f311-121">tooadd statickou privátní IP adresu toohello virtuálních počítačů, které jsou vytvořené pomocí skriptu hello výše, o následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4f311-121">tooadd a static private IP address toohello VM created using hello script above, runt he following command:</span></span>

    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      Update-AzureVM

<span data-ttu-id="4f311-122">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="4f311-122">Expected output:</span></span>

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       77d8cae2-87e6-0ead-9738-7c7dae9810cb Succeeded 

## <a name="next-steps"></a><span data-ttu-id="4f311-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4f311-123">Next steps</span></span>
* <span data-ttu-id="4f311-124">Další informace o [vyhrazené veřejné IP adresy](virtual-networks-reserved-public-ip.md) adresy.</span><span class="sxs-lookup"><span data-stu-id="4f311-124">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="4f311-125">Další informace o [veřejné IP (splnění) na úrovni instance](virtual-networks-instance-level-public-ip.md) adresy.</span><span class="sxs-lookup"><span data-stu-id="4f311-125">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="4f311-126">Poraďte se hello [vyhrazené IP rozhraní REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="4f311-126">Consult hello [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>

