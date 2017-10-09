---
title: "Ukázka skriptu aaaAzure prostředí PowerShell - směrovat provoz prostřednictvím zařízení virtuální sítě | Microsoft Docs"
description: "Azure PowerShell ukázka skriptu - směrovat provoz prostřednictvím brány firewall sítě virtuálního zařízení."
services: virtual-network
documentationcenter: virtual-network
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: 3b999f3289d654c00d5becb973e2883896457d52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>Směrovat provoz prostřednictvím sítě virtuálního zařízení

Tento ukázkový skript vytvoří virtuální síť s podsítí front-end a back-end. Vytvoří také virtuální počítač s IP Adresou předávání povoleno tooroute provozu mezi dvěma podsítěmi hello. Po spuštění skriptu hello můžete nasadit software pro sítě, jako například aplikací brány firewall, toohello virtuálních počítačů.

V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)a poté spusťte `Login-AzureRmAccount` toocreate připojení s Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript


[!code-powershell[main](../../../powershell_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.ps1 "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```
## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy toocreate hello skupinu prostředků, virtuální sítě a skupiny zabezpečení sítě. Každý příkaz v dokumentaci k toocommand specifické hello tabulky odkazů.

| Příkaz | Poznámky |
|---|---|
| [Nový AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Nový AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Vytvoří virtuální síť Azure a front-end podsítě. |
| [Nové AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Vytvoří back-end a DMZ podsítě. |
| [Nové AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Vytvoří veřejnou hello tooaccess IP adresy virtuálních počítačů z hello Internetu. |
| [Nové AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Vytvoří rozhraní virtuální sítě a předávání IP povolit pro ni. |
| [Nové AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Vytvoří skupinu zabezpečení sítě (NSG). |
| [Nové AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Vytvoří pravidla NSG, které umožňují, že porty HTTP a HTTPS příchozí toohello virtuálních počítačů. |
| [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)| Partnerů hello skupiny Nsg a toosubnets tabulky trasy. |
| [Nové AzureRmRouteTable](/powershell/module/azurerm.network/new-azurermroutetable)| Vytvoří směrovací tabulku pro všechny trasy. |
| [Nové AzureRMRouteConfig](/powershell/module/azurerm.network/new-azurermrouteconfig)| Vytvoří trasy tooroute provoz mezi podsítěmi a hello Internet prostřednictvím hello virtuálních počítačů. |
| [Nový AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Vytvoří virtuální počítač a připojí tooit hello síťový adaptér. Tento příkaz také určuje toouse bitové kopie virtuálního počítače hello a pověření pro správu. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup)  | Odstraní skupinu prostředků a všechny prostředky, které obsahuje. |

## <a name="next-steps"></a>Další kroky

Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

Další ukázky sítě skript prostředí PowerShell najdete v hello [přehled sítě Azure dokumentaci](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
