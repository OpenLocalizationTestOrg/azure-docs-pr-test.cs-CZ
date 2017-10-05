---
title: "Ukázka skriptu rozhraní příkazového řádku Azure - odstranění Azure Redis Cache | Microsoft Docs"
description: "Ukázka skriptu rozhraní příkazového řádku Azure - odstranění Azure Redis Cache"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: 7beded7a-d2c9-43a6-b3b4-b8079c11de4a
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/14/2017
ms.author: sdanie
ms.openlocfilehash: f959823b3a7c5b0262f693ecad1e6efc4eec4f35
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="delete-an-azure-redis-cache"></a>Odstranit Azure Redis Cache

V tomto scénáři zjistěte, jak odstranit Azure Redis Cache.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[hlavní](../../../cli_scripts/redis-cache/delete-cache/delete-cache.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy odstranit instanci služby Azure Redis Cache. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [odstranění az redis](https://docs.microsoft.com/cli/azure/redis#delete) | Odstraňte instanci služby Redis Cache. |


## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu Azure Redis Cache rozhraní příkazového řádku najdete v [dokumentace k Azure Redis Cache](../cli-samples.md).