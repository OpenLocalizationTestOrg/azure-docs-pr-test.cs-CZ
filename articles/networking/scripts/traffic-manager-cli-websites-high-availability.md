---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - směrovat provoz pro vysokou dostupnost aplikací | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - směrovat provoz pro vysokou dostupnost aplikací"
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: timlt
editor: tysonn
tags: azure-infrastructure
ms.assetid: 
ms.service: traffic-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: traffic-manager
ms.date: 07/07/2017
ms.author: kumud
ms.openlocfilehash: 2142c8bbec1dffc2f12b5500df142a429393a145
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="route-traffic-for-high-availability-of-applications"></a>Směrovat provoz pro vysokou dostupnost aplikací

Tento skript vytvoří skupinu prostředků, dva druhy služeb aplikace, dva webové aplikace, profil správce provozu a dva koncové body správce provozu. Traffic Manager směrovat provoz toohello aplikaci v jedné oblasti jako primární oblasti hello a sekundární oblasti toohello když aplikace hello v primární oblasti hello není k dispozici. Před provedením hello skriptu, je nutné změnit hello MyWebApp, MyWebAppL1 a MyWebAppL2 hodnoty toounique hodnoty v Azure. Po spuštění skriptu hello, budete mít přístup aplikace hello v hello primární oblasti s mywebapp.trafficmanager.net hello adresy URL.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.sh "Route traffic for high availability")]


## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Po spuštění ukázka skriptu hello hello postupujte podle příkaz může být použité tooremove hello prostředků skupina, aplikační služby a všechny související prostředky.

```azurecli
az group delete --name myResourceGroup1 --yes
az group delete --name myResourceGroup2 --yes
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy toocreate skupinu prostředků, webové aplikace, profil správce provozu a všechny související prostředky. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Vytvoří plán služby App Service. Toto je jako serverové farmy pro Azure webové aplikace. |
| [vytvoření služby App Service web az](https://docs.microsoft.com/cli/azure/appservice/web#create) | Vytvoří webové aplikace Azure v rámci hello plán služby App Service. |
| [Vytvoření profilu Správce provozu sítě az](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) | Vytvoří profilu Azure Traffic Manageru. |
| [vytvořit koncový bod správce provozu sítě az](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) | Přidá tooan koncový bod profilu služby Traffic Manager Azure. |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu rozhraní příkazového řádku služby aplikace naleznete v hello [sítí Azure dokumentaci](../cli-samples.md).
