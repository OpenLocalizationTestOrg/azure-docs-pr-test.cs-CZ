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
ms.openlocfilehash: 0593d063a4935d02aae124d83b62b11e37aa3c33
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="route-traffic-for-high-availability-of-applications"></a>Směrovat provoz pro vysokou dostupnost aplikací

Tento skript vytvoří skupinu prostředků, dva druhy služeb aplikace, dva webové aplikace, profil správce provozu a dva koncové body správce provozu. Správce provozu přesměruje přenosy na aplikaci v jedné oblasti jako primární oblasti a sekundární oblast, když aplikace v primární oblasti není k dispozici. Před spuštěním skriptu, musíte změnit hodnoty MyWebApp, MyWebAppL1 a MyWebAppL2 jedinečné hodnoty mezi Azure. Po spuštění skriptu, můžete přístup k aplikaci v primární oblasti s mywebapp.trafficmanager.net adresy URL.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[hlavní](../../../cli_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.sh "směrování provozu pro zajištění vysoké dostupnosti")]


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
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Vytvoří plán služby App Service. Toto je jako serverové farmy pro Azure webové aplikace. |
| [vytvoření služby App Service web az](https://docs.microsoft.com/cli/azure/appservice/web#create) | Vytvoří webové aplikace Azure v rámci plánu služby App Service. |
| [Vytvoření profilu Správce provozu sítě az](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) | Vytvoří profilu Azure Traffic Manageru. |
| [vytvořit koncový bod správce provozu sítě az](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) | Koncový bod se přidá do profilu Azure Traffic Manager. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [sítí Azure dokumentaci](../cli-samples.md).
