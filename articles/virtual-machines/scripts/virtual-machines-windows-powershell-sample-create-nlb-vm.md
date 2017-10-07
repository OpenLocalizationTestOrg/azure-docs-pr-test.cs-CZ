---
title: "aaaAzure ukázkový skript prostředí PowerShell - vytvořit Vyrovnávání zatížení sítě virtuálních počítačů Windows | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – vytvoření Vyrovnávání zatížení sítě virtuálních počítačů Windows"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 60a48c5f243c8ff3ab886437ae45b21ce069ea4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-traffic-between-highly-available-virtual-machines"></a>Vyrovnávání zatížení přenosů mezi vysoce dostupné virtuální počítače

Tento ukázkový skript vytvoří vše potřebné toorun několik virtuálních počítačů Windows serveru 2016 nakonfigurovaný v s vysokou dostupností a načíst vyrovnáváním konfigurace. Po spouštění skriptu hello, budete mít tři virtuální počítače připojené k tooan Azure skupiny dostupnosti a je přístupný prostřednictvím Vyrovnávání zatížení Azure.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.ps1 "Create VM NLB")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy toocreate hello nasazení. Každou položku v tabulce hello propojí toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Nový AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Nové AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Vytvoří konfiguraci podsítě. Tato konfigurace se používá s procesem vytvoření virtuální sítě hello. |
| [Nový AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Vytvoří virtuální síť. |
| [Nové AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Vytvoří veřejnou IP adresu. |
| [Nové AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) | Vytvoří konfiguraci front-end IP adresy pro vyrovnávání zatížení. |
| [Nové AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) | Vytvoří konfiguraci fondu adres back-end pro vyrovnávání zatížení. |
| [Nové AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | Vytvoří test konfigurace pro vyrovnávání zatížení. |
| [Nové AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | Vytvoří pravidlo konfigurace pro vyrovnávání zatížení. |
| [Nové AzureRmLoadBalancerInboundNatRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) | Vytvoří příchozí konfigurace pravidla NAT nástroje pro vyrovnávání zatížení. |
| [Nové AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer) | Vytvoří nástroj pro vyrovnávání zatížení. |
| [Nové AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Vytvoří pravidlo konfiguraci skupiny zabezpečení sítě. Tato konfigurace je použité toocreate pravidlo NSG při vytvoření hello NSG. |
| [Nové AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Vytvoří skupinu zabezpečení sítě. |
| [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | Získá informace o podsíti. Tato informace se používají při vytváření síťové rozhraní. |
| [Nové AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Vytvoří rozhraní sítě. |
| [Nové AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Vytvoří konfigurace virtuálního počítače. Tato konfigurace zahrnuje informace, jako je název virtuálního počítače, operační systém a pověření pro správu. Konfigurace Hello se používá při vytváření virtuálních počítačů. |
| [Nový AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Vytvoření virtuálního počítače. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Odebere skupinu prostředků a všechny prostředky obsažené v rámci. |

## <a name="next-steps"></a>Další kroky

Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Ukázky skriptu PowerShell další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
