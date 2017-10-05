---
title: "Ukázka skriptu Azure CLI - Get název hostitele, porty a klíče pro Azure Redis Cache | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - Get název hostitele, porty a klíče pro Azure Redis Cache instance"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: 761eb24e-2ba7-418d-8fc3-431153e69a90
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/14/2017
ms.author: sdanie
ms.openlocfilehash: cd9adc784bceb0fff5e7c2bbee2be0950c51c8f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-hostname-ports-and-keys-for-azure-redis-cache"></a>Získat název hostitele, porty a klíče pro Azure Redis Cache

V tomto scénáři zjistěte, jak načíst název hostitele, porty a klíče, které slouží k připojení k instanci služby Azure Redis Cache.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[hlavní](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Redis Cache")]


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy pro načtení názvu hostitele, klíče a porty instanci služby Azure Redis Cache. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Zobrazit az redis](https://docs.microsoft.com/cli/azure/redis#show) | Načtěte podrobnosti o instanci služby Azure Redis Cache. |
| [AZ redis seznamu klíčů](https://docs.microsoft.com/cli/azure/redis#list-keys) | Načtěte přístupové klíče pro instanci služby Azure Redis Cache. |


## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu Azure Redis Cache rozhraní příkazového řádku najdete v [dokumentace k Azure Redis Cache](../cli-samples.md).