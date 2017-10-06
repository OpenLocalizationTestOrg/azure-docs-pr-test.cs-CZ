---
title: "aaaAzure ukázka skriptu prostředí PowerShell - směrovat provoz pro vysokou dostupnost aplikací | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - směrovat provoz pro vysokou dostupnost aplikací"
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: timlt
editor: georgewallace
tags: azure-infrastructure
ms.assetid: 
ms.service: traffic-manager
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: traffic-manager
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: 11d15780403b4ed79e85d7b3495bc5d674bfdaee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="route-traffic-for-high-availability-of-applications"></a>Směrovat provoz pro vysokou dostupnost aplikací

Tento skript vytvoří skupinu prostředků, dva druhy služeb aplikace, dva webové aplikace, profil správce provozu a dva koncové body správce provozu. Traffic Manager směrovat provoz toohello aplikaci v jedné oblasti jako primární oblasti hello a sekundární oblasti toohello když aplikace hello v primární oblasti hello není k dispozici. Před provedením hello skriptu, je nutné změnit hello MyWebApp, MyWebAppL1 a MyWebAppL2 hodnoty toounique hodnoty v Azure. Po spuštění skriptu hello, budete mít přístup aplikace hello v hello primární oblasti s mywebapp.trafficmanager.net hello adresy URL.

V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)a poté spusťte `Login-AzureRmAccount` toocreate připojení s Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.ps1 "Route traffic for high availability")]


Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup1
Remove-AzureRmResourceGroup -Name myResourceGroup2
```


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy toocreate skupinu prostředků, webové aplikace, profil správce provozu a všechny související prostředky. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Nový AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Nové AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | Vytvoří plán služby App Service. Toto je jako serverové farmy pro Azure webové aplikace. |
| [Nové AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Vytvoří webové aplikace Azure v rámci hello plán služby App Service. |
| [Set-AzureRmResource](/powershell/module/azurerm.resources/new-azurermresource) | Vytvoří webové aplikace Azure v rámci hello plán služby App Service. |
| [Nové AzureRmTrafficManagerProfile](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerprofile) | Vytvoří profilu Azure Traffic Manageru. |
| [Nové AzureRmTrafficManagerEndpoint](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerendpoint) | Přidá tooan koncový bod profilu služby Traffic Manager Azure. |

## <a name="next-steps"></a>Další kroky

Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

Další ukázky sítě skript prostředí PowerShell najdete v hello [přehled sítě Azure dokumentaci](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
