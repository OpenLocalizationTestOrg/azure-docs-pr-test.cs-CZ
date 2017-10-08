---
title: "aaaMove virtuální počítač (klasický) nebo cloudové služby role instance tooa jiné podsíti - prostředí Azure PowerShell | Microsoft Docs"
description: "Zjistěte, jak toomove virtuálních počítačů (klasické) a cloudové služby role instance tooa jiné podsíti pomocí prostředí PowerShell."
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
ms.openlocfilehash: c8d2de56f42a91be4a665414ea9641ffd46588fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-a-vm-classic-or-cloud-services-role-instance-tooa-different-subnet-using-powershell"></a><span data-ttu-id="54a7b-103">Přesunutí virtuálního počítače (klasické) nebo cloudové služby role instance tooa jiné podsíti pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="54a7b-103">Move a VM (Classic) or Cloud Services role instance tooa different subnet using PowerShell</span></span>
<span data-ttu-id="54a7b-104">Můžete použít PowerShell toomove z tooanother jednu podsíť virtuálních počítačů (klasické) v hello stejnou virtuální síť (VNet).</span><span class="sxs-lookup"><span data-stu-id="54a7b-104">You can use PowerShell toomove your VMs (Classic) from one subnet tooanother in hello same virtual network (VNet).</span></span> <span data-ttu-id="54a7b-105">Instance role se dají přesunout soubor .CSCFG hello úpravy, nikoli pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="54a7b-105">Role instances can be moved by editing hello CSCFG file, rather than using PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="54a7b-106">Tento článek vysvětluje, jak toomove virtuální počítače nasazené prostřednictvím hello pouze v modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="54a7b-106">This article explains how toomove VMs deployed through hello classic deployment model only.</span></span>
> 
> 

<span data-ttu-id="54a7b-107">Proč přesunout tooanother podsíť virtuálních počítačů?</span><span class="sxs-lookup"><span data-stu-id="54a7b-107">Why move VMs tooanother subnet?</span></span> <span data-ttu-id="54a7b-108">Podsíť migrace je užitečné, když hello starší podsíť je příliš malá a nelze rozšířit z důvodu tooexisting spuštěných virtuálních počítačů v této podsíti.</span><span class="sxs-lookup"><span data-stu-id="54a7b-108">Subnet migration is useful when hello older subnet is too small and cannot be expanded due tooexisting running VMs in that subnet.</span></span> <span data-ttu-id="54a7b-109">V takovém případě můžete vytvořit nový, větší podsíť a migrovat novou podsíť toohello hello virtuální počítače a potom po dokončení migrace, můžete odstranit staré prázdný podsíť hello.</span><span class="sxs-lookup"><span data-stu-id="54a7b-109">In that case, you can create a new, larger subnet and migrate hello VMs toohello new subnet, then after migration is complete, you can delete hello old empty subnet.</span></span>

## <a name="how-toomove-a-vm-tooanother-subnet"></a><span data-ttu-id="54a7b-110">Jak toomove tooanother podsíť virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="54a7b-110">How toomove a VM tooanother subnet</span></span>
<span data-ttu-id="54a7b-111">toomove virtuálního počítače, spusťte rutinu prostředí PowerShell Set-AzureSubnet hello, pomocí níže uvedeném příkladu hello jako šablona.</span><span class="sxs-lookup"><span data-stu-id="54a7b-111">toomove a VM, run hello Set-AzureSubnet PowerShell cmdlet, using hello example below as a template.</span></span> <span data-ttu-id="54a7b-112">V příkladu hello níže jsme přecházíte TestVM z jeho přítomen podsíti tooSubnet-2.</span><span class="sxs-lookup"><span data-stu-id="54a7b-112">In hello example below, we are moving TestVM from its present subnet, tooSubnet-2.</span></span> <span data-ttu-id="54a7b-113">Být jisti tooedit hello příklad tooreflect prostředí.</span><span class="sxs-lookup"><span data-stu-id="54a7b-113">Be sure tooedit hello example tooreflect your environment.</span></span> <span data-ttu-id="54a7b-114">Všimněte si, že vždy, když spustíte rutinu Update-AzureVM hello jako součást postupu, bude restartován virtuálního počítače jako součást procesu aktualizace hello.</span><span class="sxs-lookup"><span data-stu-id="54a7b-114">Note that whenever you run hello Update-AzureVM cmdlet as part of a procedure, it will restart your VM as part of hello update process.</span></span>

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
    | Set-AzureSubnet –SubnetNames Subnet-2 `
    | Update-AzureVM

<span data-ttu-id="54a7b-115">Pokud jste zadali interní statickou privátní IP pro virtuální počítač, budete mít tooclear tohoto nastavení před přesunutím hello virtuálních počítačů tooa novou podsíť.</span><span class="sxs-lookup"><span data-stu-id="54a7b-115">If you specified a static internal private IP for your VM, you'll have tooclear that setting before you can move hello VM tooa new subnet.</span></span> <span data-ttu-id="54a7b-116">V takovém případě použijte hello následující:</span><span class="sxs-lookup"><span data-stu-id="54a7b-116">In that case, use hello following:</span></span>

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Set-AzureSubnet -SubnetNames Subnet-2 `
    | Update-AzureVM

## <a name="toomove-a-role-instance-tooanother-subnet"></a><span data-ttu-id="54a7b-117">toomove podsíť tooanother instance role</span><span class="sxs-lookup"><span data-stu-id="54a7b-117">toomove a role instance tooanother subnet</span></span>
<span data-ttu-id="54a7b-118">toomove instanci role, upravte soubor .CSCFG hello.</span><span class="sxs-lookup"><span data-stu-id="54a7b-118">toomove a role instance, edit hello CSCFG file.</span></span> <span data-ttu-id="54a7b-119">V příkladu hello níže jsme přesouváte "Role0" ve virtuální síti *VNETName* z jeho přítomen podsítě příliš*podsíť 2*.</span><span class="sxs-lookup"><span data-stu-id="54a7b-119">In hello example below, we are moving "Role0" in virtual network *VNETName* from its present subnet too*Subnet-2*.</span></span> <span data-ttu-id="54a7b-120">Proto již byla nasazena hello role instance, změníte právě název podsítě hello = podsíť 2.</span><span class="sxs-lookup"><span data-stu-id="54a7b-120">Because hello role instance was already deployed, you'll just change hello Subnet name = Subnet-2.</span></span> <span data-ttu-id="54a7b-121">Být jisti tooedit hello příklad tooreflect prostředí.</span><span class="sxs-lookup"><span data-stu-id="54a7b-121">Be sure tooedit hello example tooreflect your environment.</span></span>

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
