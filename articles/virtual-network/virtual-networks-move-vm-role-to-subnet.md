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
# <a name="move-a-vm-classic-or-cloud-services-role-instance-tooa-different-subnet-using-powershell"></a>Přesunutí virtuálního počítače (klasické) nebo cloudové služby role instance tooa jiné podsíti pomocí prostředí PowerShell
Můžete použít PowerShell toomove z tooanother jednu podsíť virtuálních počítačů (klasické) v hello stejnou virtuální síť (VNet). Instance role se dají přesunout soubor .CSCFG hello úpravy, nikoli pomocí prostředí PowerShell.

> [!NOTE]
> Tento článek vysvětluje, jak toomove virtuální počítače nasazené prostřednictvím hello pouze v modelu nasazení classic.
> 
> 

Proč přesunout tooanother podsíť virtuálních počítačů? Podsíť migrace je užitečné, když hello starší podsíť je příliš malá a nelze rozšířit z důvodu tooexisting spuštěných virtuálních počítačů v této podsíti. V takovém případě můžete vytvořit nový, větší podsíť a migrovat novou podsíť toohello hello virtuální počítače a potom po dokončení migrace, můžete odstranit staré prázdný podsíť hello.

## <a name="how-toomove-a-vm-tooanother-subnet"></a>Jak toomove tooanother podsíť virtuálních počítačů
toomove virtuálního počítače, spusťte rutinu prostředí PowerShell Set-AzureSubnet hello, pomocí níže uvedeném příkladu hello jako šablona. V příkladu hello níže jsme přecházíte TestVM z jeho přítomen podsíti tooSubnet-2. Být jisti tooedit hello příklad tooreflect prostředí. Všimněte si, že vždy, když spustíte rutinu Update-AzureVM hello jako součást postupu, bude restartován virtuálního počítače jako součást procesu aktualizace hello.

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
    | Set-AzureSubnet –SubnetNames Subnet-2 `
    | Update-AzureVM

Pokud jste zadali interní statickou privátní IP pro virtuální počítač, budete mít tooclear tohoto nastavení před přesunutím hello virtuálních počítačů tooa novou podsíť. V takovém případě použijte hello následující:

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Set-AzureSubnet -SubnetNames Subnet-2 `
    | Update-AzureVM

## <a name="toomove-a-role-instance-tooanother-subnet"></a>toomove podsíť tooanother instance role
toomove instanci role, upravte soubor .CSCFG hello. V příkladu hello níže jsme přesouváte "Role0" ve virtuální síti *VNETName* z jeho přítomen podsítě příliš*podsíť 2*. Proto již byla nasazena hello role instance, změníte právě název podsítě hello = podsíť 2. Být jisti tooedit hello příklad tooreflect prostředí.

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
