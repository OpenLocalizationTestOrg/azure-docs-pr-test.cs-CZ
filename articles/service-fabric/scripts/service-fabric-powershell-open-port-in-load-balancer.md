---
title: "aaaAzure ukázka skriptu prostředí PowerShell - port otevřete aplikace nástroji pro vyrovnávání zatížení | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - otevřít port v hello Vyrovnávání zatížení Azure pro aplikace Service Fabric."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 6acb28942851dce63f89f7de362b7cf1dc7b1fee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="open-an-application-port-in-hello-azure-load-balancer"></a>Otevřete port aplikace nástroji pro vyrovnávání zatížení Azure hello

Aplikace běžící v Azure Service Fabric se nachází za nástrojem pro vyrovnávání zatížení Azure hello. Tento ukázkový skript otevře port v k nástroji pro vyrovnávání zatížení Azure tak, aby aplikace Service Fabric může komunikovat s externími klienty. Parametry hello přizpůsobte podle potřeby. 

V případě potřeby nainstalujte modul Service Fabric prostředí PowerShell hello s hello [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/service-fabric/open-port-in-load-balancer/open-port-in-load-balancer.ps1 "Open a port in hello load balancer")]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy. Každý příkaz v dokumentaci k toocommand specifické hello tabulky odkazů.

| Příkaz | Poznámky |
|---|---|
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Získá prostředek služby Azure.  |
| [Get-AzureRmLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer) | Získá pro vyrovnávání zatížení Azure hello. |
| [Přidat AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig) | Přidá Vyrovnávání zatížení tooa Konfigurace testu.|
| [Get-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/get-azurermloadbalancerprobeconfig) | Získá test konfigurace pro vyrovnávání zatížení. |
| [Přidat AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig) | Přidá pravidlo Vyrovnávání zatížení tooa konfigurace. |
| [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer) | Nastaví hello cíle stavu nástroje pro vyrovnávání zatížení. |

## <a name="next-steps"></a>Další kroky

Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Další ukázky pro Azure Service Fabric Powershell naleznete v hello [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).
