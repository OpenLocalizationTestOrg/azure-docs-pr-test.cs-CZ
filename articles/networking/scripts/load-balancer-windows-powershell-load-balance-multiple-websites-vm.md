---
title: "aaaAzure ukázkový skript prostředí PowerShell - vyrovnávat zatížení více webů s prostředím Azure PowerShell | Microsoft Docs"
description: "Ukázkový skript prostředí PowerShell Azure - vyrovnávat zatížení více webů toohello stejného virtuálního počítače"
services: load-balancer
documentationcenter: load-balancer
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: 316818964eb6928fe4163ef69eb7f05da2dc9636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-multiple-websites"></a>Vyrovnávat zatížení více webů

Tento ukázkový skript vytvoří virtuální síť s dva virtuální počítače (VM), které jsou členy skupiny dostupnosti. Nástroj pro vyrovnávání zatížení přesměruje přenosy pro dva samostatné IP adres toohello dva virtuální počítače. Po spouštění skriptu hello můžete nasadit webový server softwaru toohello virtuální počítače a hostování více webů, každou s vlastní IP adresu.

V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)a poté spusťte `Login-AzureRmAccount` toocreate připojení s Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.ps1  "Load balance multiple web sites")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy toocreate skupinu prostředků virtuální sítě, zatížení vyrovnávání a všechny související prostředky. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Nový AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Nové AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) | Vytvoří tooprovide sady Azure dostupnosti pro vysokou dostupnost. |
| [Nové AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Vytvoří konfiguraci podsítě. Tato konfigurace se používá s procesem vytvoření virtuální sítě hello. |
| [Nový AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Vytvoří virtuální síť. |
| [Nové AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Vytvoří veřejnou IP adresu. |
| [Nové AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) | Vytvoří konfigurace IP front-endu nástroje pro vyrovnávání zatížení. |
| [Nové AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) | Vytvoří konfiguraci fondu adres back-end pro vyrovnávání zatížení. |
| [Nové AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | Vytvoří kontrolu Vyrovnávání zatížení sítě. Test Vyrovnávání zatížení sítě je použité toomonitor každý virtuální počítač v sadě hello Vyrovnávání zatížení sítě. Pokud žádné virtuální počítače bude nedostupné, není provoz směruje toohello virtuálních počítačů. |
| [Nové AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | Vytvoří pravidlo Vyrovnávání zatížení sítě. V této ukázce se vytvoří pravidlo pro port 80. HTTP přenos dorazí na hello Vyrovnávání zatížení sítě, je směrované tooport 80 jeden hello virtuálních počítačů v sadě hello Vyrovnávání zatížení sítě. |
| [Nové AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer) | Vytvoří nástroj pro vyrovnávání zatížení. |
| [Nové AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/new-azurermnetworkinterfaceipconfig) | Definuje pokročilých funkcí pro rozhraní virtuální sítě. |
| [Nové AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Vytvoří rozhraní sítě. |
| [Nové AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Vytvoří konfigurace virtuálního počítače. Tato konfigurace zahrnuje informace, jako je název virtuálního počítače, operační systém a pověření pro správu. Konfigurace Hello se používá při vytváření virtuálních počítačů. |
| [Nový AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Vytvoření virtuálního počítače. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Odebere skupinu prostředků a všechny prostředky obsažené v rámci. |

## <a name="next-steps"></a>Další kroky

Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

Další ukázky sítě skript prostředí PowerShell najdete v hello [přehled sítě Azure dokumentaci](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
