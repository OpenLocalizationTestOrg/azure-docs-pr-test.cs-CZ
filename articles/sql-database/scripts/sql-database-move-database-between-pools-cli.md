---
title: "Elastický fond rozhraní příkazového řádku příklad přesunutí Azure SQL database SQL | Microsoft Docs"
description: "Azure CLI ukázkový skript přesunout databázi SQL v elastický fond SQL."
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 07/05/2017
ms.author: janeng
ms.openlocfilehash: 1dc31a0b20f36e28a58896ed63a5e0395ae1d3af
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-cli-to-move-an-azure-sql-database-in-a-sql-elastic-pool"></a>Pomocí rozhraní příkazového řádku přesouvat Azure SQL database v elastický fond SQL.

Tento příklad skriptu rozhraní příkazového řádku Azure vytvoří dvě elastické fondy a Azure SQL database se přesouvají z jedné elastický fond SQL do jiného elastický fond SQL a pak se posouvá databázi z elastického fondu na úroveň výkonu jedné databáze Azure. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[hlavní](../../../cli_scripts/sql-database/move-database-between-pools/move-database-between-pools.sh "přesun databáze mezi fondy")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení

Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření az sql serveru](https://docs.microsoft.com/cli/azure/sql/server#create) | Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu. |
| [Vytvoření sql az Elastická fondy](https://docs.microsoft.com/cli/azure/sql/elastic-pool#create) | Vytvoří elastického fondu v rámci logického serveru. |
| [Vytvoření az sql db](https://docs.microsoft.com/cli/azure/sql/db#create) | Vytvoří databázi logického serveru jako jednu, nebo databázi ve fondu. |
| [aktualizace databáze sql az](https://docs.microsoft.com/cli/azure/sql/db#update) | Aktualizuje vlastnosti databáze nebo přesouvá databázi do, mimo nebo mezi elastické fondy. |
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/vm/extension#set) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu SQL databáze rozhraní příkazového řádku najdete v [dokumentace Azure SQL Database](../sql-database-cli-samples.md).


