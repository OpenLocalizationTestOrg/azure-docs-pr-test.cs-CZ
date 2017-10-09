---
title: "aaaOpen porty tooa virtuálního počítače pomocí prostředí Azure PowerShell | Microsoft Docs"
description: "Zjistěte, jak tooopen port / create tooyour koncový bod virtuálního počítače s Windows pomocí nasazení režimu hello Azure resource Manageru a prostředí Azure PowerShell"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: cf45f7d8-451a-48ab-8419-730366d54f1e
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: c1817a0c447ae4ce7a1ce2a1fc6927bedf2dacb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooopen-ports-and-endpoints-tooa-vm-in-azure-using-powershell"></a>Jak tooopen koncové body a porty tooa virtuálního počítače v Azure pomocí prostředí PowerShell
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Rychlé příkazy
Skupina zabezpečení sítě toocreate a pravidla seznamu ACL, musíte [hello nejnovější verzi prostředí Azure PowerShell nainstalovaný](/powershell/azureps-cmdlets-docs). Můžete také [proveďte tyto kroky, pomocí portálu Azure hello](nsg-quickstart-portal.md).

Přihlaste se tooyour účet Azure:

```powershell
Login-AzureRmAccount
```

Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami. Názvy parametrů příklad zahrnuté *myResourceGroup*, *myNetworkSecurityGroup*, a *myVnet*.

Vytvořit pravidlo s [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig). Hello následující příklad vytvoří pravidlo s názvem *myNetworkSecurityGroupRule* tooallow *tcp* přenosy na portu *80*:

```powershell
$httprule = New-AzureRmNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRule" `
    -Description "Allow HTTP" `
    -Access "Allow" `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority "100" `
    -SourceAddressPrefix "Internet" `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 80
```

Dále vytvořte skupině zabezpečení sítě s [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) a pravidlo přiřazení hello HTTP jste právě vytvořili, se následujícím způsobem. Hello následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName "myResourceGroup" `
    -Location "EastUS" `
    -Name "myNetworkSecurityGroup" `
    -SecurityRules $httprule
```

Teď umožňuje přiřadit podsíť tooa skupinu zabezpečení sítě. Hello následující příklad přiřadí existující virtuální síť s názvem *myVnet* toohello proměnná *$vnet* s [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork):

```powershell
$vnet = Get-AzureRmVirtualNetwork `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

Vaše skupina zabezpečení sítě přidružit podsíť s [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig). Hello následující příklad přiřadí hello podsíť s názvem *mySubnet* s vaší skupiny zabezpečení sítě:

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

Nakonec aktualizujte virtuální sítě s [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) v pořadí pro vaše změny tootake vliv:

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a>Další informace o skupinách zabezpečení sítě
Hello zde rychlé příkazy umožní tooget nahoru a spuštěna s průchodu tooyour přenosy virtuálních počítačů. Skupiny zabezpečení sítě zadejte mnoho funkcí a členitosti pro řízení přístupu tooyour prostředky. Další informace o [vytvoření skupiny zabezpečení sítě a seznamu ACL pravidla zde](tutorial-virtual-network.md#manage-internal-traffic).

Pro vysokou dostupnost webové aplikace měli byste umístit virtuální počítače za pro vyrovnávání zatížení Azure. Nástroj pro vyrovnávání zatížení Hello distribuuje provoz tooVMs ke skupině zabezpečení sítě, která poskytuje filtrování provozu. Další informace najdete v tématu [jak tooload vyrovnávání Linux virtuálního počítače v Azure toocreate vysoce dostupné aplikace](tutorial-load-balancer.md).

## <a name="next-steps"></a>Další kroky
V tomto příkladu jste vytvořili přenosem tooallow HTTP jednoduché pravidlo. Můžete najít informace o vytváření podrobnější prostředí v hello následující články:

* [Přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md)
* [Co je skupina zabezpečení sítě (NSG)?](../../virtual-network/virtual-networks-nsg.md)
* [Přehled Azure Resource Manageru pro vyrovnávání zatížení](../../load-balancer/load-balancer-arm.md)

