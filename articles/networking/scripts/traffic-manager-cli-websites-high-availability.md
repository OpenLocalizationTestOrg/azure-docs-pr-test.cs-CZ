---
title: "Ukázka skriptu Azure CLI - směrovat provoz pro vysokou dostupnost aplikací | Microsoft Docs"
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
ms.openlocfilehash: 5c3754c3c3c96e1d2f0b5b52d8108ecc3903f788
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="route-traffic-for-high-availability-of-applications"></a>Směrovat provoz pro vysokou dostupnost aplikací

Tento skript vytvoří skupinu prostředků, dva druhy služeb aplikace, dva webové aplikace, profil správce provozu a dva koncové body správce provozu. Správce provozu přesměruje přenosy na aplikaci v jedné oblasti jako primární oblasti a sekundární oblast, když aplikace v primární oblasti není k dispozici. Před spuštěním skriptu, musíte změnit hodnoty MyWebApp, MyWebAppL1 a MyWebAppL2 jedinečné hodnoty mezi Azure. Po spuštění skriptu, můžete přístup k aplikaci v primární oblasti s mywebapp.trafficmanager.net adresy URL.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.sh "Route traffic for high availability")]


## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Po spuštění ukázka skriptu, použijte příkaz lze použít k odebrání skupiny prostředků, aplikační služby a všechny související prostředky.

```azurecli
az group delete --name myResourceGroup1 --yes
az group delete --name myResourceGroup2 --yes
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků, webové aplikace, profil správce provozu a všechny související prostředky. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Vytvoří plán služby App Service. Toto je jako serverové farmy pro Azure webové aplikace. |
| [vytvoření služby App Service web az](https://docs.microsoft.com/cli/azure/appservice/web#az_appservice_web_create) | Vytvoří webové aplikace Azure v rámci plánu služby App Service. |
| [Vytvoření profilu Správce provozu sítě az](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#az_network_traffic_manager_profile_create) | Vytvoří profilu Azure Traffic Manageru. |
| [vytvořit koncový bod správce provozu sítě az](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#az_network_traffic_manager_endpoint_create) | Koncový bod se přidá do profilu Azure Traffic Manager. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [sítí Azure dokumentaci](../cli-samples.md).
