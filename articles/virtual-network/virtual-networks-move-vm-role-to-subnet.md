---
title: "Přesunutí virtuálního počítače (klasické) nebo instance role cloudové služby na jiné podsíti - prostředí Azure PowerShell | Microsoft Docs"
description: "Zjistěte, jak přesunout virtuální počítače (klasické) a instancí rolí cloudové služby na jiné podsíti pomocí prostředí PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: de4135c7-dc5b-4ffa-84cc-1b8364b7b427
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b094f8338394ef2e84cad3070936d715411326a4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="move-a-vm-classic-or-cloud-services-role-instance-to-a-different-subnet-using-powershell"></a><span data-ttu-id="eba18-103">Přesunutí virtuálního počítače (klasické) nebo instance role cloudové služby na jiné podsíti pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="eba18-103">Move a VM (Classic) or Cloud Services role instance to a different subnet using PowerShell</span></span>
<span data-ttu-id="eba18-104">Můžete použít PowerShell k přesunutí virtuálních počítačů (klasické) z jedné podsítě do jiné ve stejné virtuální síti (VNet).</span><span class="sxs-lookup"><span data-stu-id="eba18-104">You can use PowerShell to move your VMs (Classic) from one subnet to another in the same virtual network (VNet).</span></span> <span data-ttu-id="eba18-105">Instance role se dají přesunout úpravy soubor .CSCFG, nikoli pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="eba18-105">Role instances can be moved by editing the CSCFG file, rather than using PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="eba18-106">Tento článek vysvětluje, jak přesunout virtuální počítače nasazené prostřednictvím model nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="eba18-106">This article explains how to move VMs deployed through the classic deployment model only.</span></span>
> 
> 

<span data-ttu-id="eba18-107">Proč virtuální počítače přesunout do jiné podsítě?</span><span class="sxs-lookup"><span data-stu-id="eba18-107">Why move VMs to another subnet?</span></span> <span data-ttu-id="eba18-108">Podsíť migrace je užitečné, když starší podsíť je příliš malá a nelze rozšířit z důvodu existující virtuální počítače běžící v této podsíti.</span><span class="sxs-lookup"><span data-stu-id="eba18-108">Subnet migration is useful when the older subnet is too small and cannot be expanded due to existing running VMs in that subnet.</span></span> <span data-ttu-id="eba18-109">V takovém případě můžete vytvořit nový, větší podsíť a migrovat virtuální počítače do nové podsítě a potom po dokončení migrace, můžete odstranit staré prázdný podsíť.</span><span class="sxs-lookup"><span data-stu-id="eba18-109">In that case, you can create a new, larger subnet and migrate the VMs to the new subnet, then after migration is complete, you can delete the old empty subnet.</span></span>

## <a name="how-to-move-a-vm-to-another-subnet"></a><span data-ttu-id="eba18-110">Postup přesunutí virtuálního počítače do jiné podsítě</span><span class="sxs-lookup"><span data-stu-id="eba18-110">How to move a VM to another subnet</span></span>
<span data-ttu-id="eba18-111">Přesunout virtuální počítač, spusťte rutinu Set-AzureSubnet prostředí PowerShell, pomocí níže uvedeném příkladu jako šablona.</span><span class="sxs-lookup"><span data-stu-id="eba18-111">To move a VM, run the Set-AzureSubnet PowerShell cmdlet, using the example below as a template.</span></span> <span data-ttu-id="eba18-112">V následujícím příkladu jsme přecházíte TestVM z jeho přítomen podsíti na podsíť-2.</span><span class="sxs-lookup"><span data-stu-id="eba18-112">In the example below, we are moving TestVM from its present subnet, to Subnet-2.</span></span> <span data-ttu-id="eba18-113">Nezapomeňte upravit třeba, aby se zohlednilo vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="eba18-113">Be sure to edit the example to reflect your environment.</span></span> <span data-ttu-id="eba18-114">Všimněte si, že vždy, když spustíte rutinu Update-AzureVM jako součást postupu, bude restartován virtuální počítač jako součást procesu aktualizace.</span><span class="sxs-lookup"><span data-stu-id="eba18-114">Note that whenever you run the Update-AzureVM cmdlet as part of a procedure, it will restart your VM as part of the update process.</span></span>

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
    | Set-AzureSubnet –SubnetNames Subnet-2 `
    | Update-AzureVM

<span data-ttu-id="eba18-115">Pokud jste zadali interní statickou privátní IP pro virtuální počítač, budete muset zrušit výběr tohoto nastavení můžete přesunout virtuální počítač novou podsíť.</span><span class="sxs-lookup"><span data-stu-id="eba18-115">If you specified a static internal private IP for your VM, you'll have to clear that setting before you can move the VM to a new subnet.</span></span> <span data-ttu-id="eba18-116">V takovém případě použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="eba18-116">In that case, use the following:</span></span>

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Set-AzureSubnet -SubnetNames Subnet-2 `
    | Update-AzureVM

## <a name="to-move-a-role-instance-to-another-subnet"></a><span data-ttu-id="eba18-117">Chcete-li přesunout instanci role na jinou podsítí</span><span class="sxs-lookup"><span data-stu-id="eba18-117">To move a role instance to another subnet</span></span>
<span data-ttu-id="eba18-118">Chcete-li přesunout instanci role, upravte soubor .CSCFG.</span><span class="sxs-lookup"><span data-stu-id="eba18-118">To move a role instance, edit the CSCFG file.</span></span> <span data-ttu-id="eba18-119">V následujícím příkladu jsme přesouváte "Role0" ve virtuální síti *VNETName* z jeho přítomen podsítě k *podsíť 2*.</span><span class="sxs-lookup"><span data-stu-id="eba18-119">In the example below, we are moving "Role0" in virtual network *VNETName* from its present subnet to *Subnet-2*.</span></span> <span data-ttu-id="eba18-120">Proto již byla nasazena role instance, změníte právě název podsítě = Podsíť 2.</span><span class="sxs-lookup"><span data-stu-id="eba18-120">Because the role instance was already deployed, you'll just change the Subnet name = Subnet-2.</span></span> <span data-ttu-id="eba18-121">Nezapomeňte upravit třeba, aby se zohlednilo vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="eba18-121">Be sure to edit the example to reflect your environment.</span></span>

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
