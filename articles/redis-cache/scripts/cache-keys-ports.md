---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - Get hello název hostitele, porty a klíče pro Azure Redis Cache | Microsoft Docs"
description: "Azure ukázka skriptu rozhraní příkazového řádku - Get hello hostname, porty a klíče pro instanci služby Azure Redis Cache"
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
ms.openlocfilehash: e6e794558087d6568438c439e2bf99fc46eeb8bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-hostname-ports-and-keys-for-azure-redis-cache"></a>Získat název hostitele hello, porty a klíče pro Azure Redis Cache

V tomto scénáři zjistíte, jak používat tooretrieve hello hostname, porty a klíče instanci tooconnect tooan Azure Redis Cache.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[main](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Redis Cache")]


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy tooretrieve hello název hostitele, klíče a porty instanci služby Azure Redis Cache. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Zobrazit az redis](https://docs.microsoft.com/cli/azure/redis#show) | Načtěte podrobnosti o instanci služby Azure Redis Cache. |
| [AZ redis seznamu klíčů](https://docs.microsoft.com/cli/azure/redis#list-keys) | Načtěte přístupové klíče pro instanci služby Azure Redis Cache. |


## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu Azure Redis Cache rozhraní příkazového řádku najdete v hello [dokumentace k Azure Redis Cache](../cli-samples.md).
