---
title: "Příklad rozhraní příkazového řádku škáluje SQL elastického fondu Azure SQL Database | Microsoft Docs"
description: "Azure CLI ukázkový skript škálování elastický fond SQL v databázi SQL Azure"
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
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 888d2b7b7c118fede82d39881570a3b3d7b09961
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-cli-to-scale-a-sql-elastic-pool-in-azure-sql-database"></a>Použití rozhraní příkazového řádku pro škálování elastický fond SQL v databázi SQL Azure

Tento příklad skriptu rozhraní příkazového řádku Azure vytvoří SQL elastické fondy, přesune databáze ve fondu a změny úrovně výkonu elastického fondu. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[hlavní](../../../cli_scripts/sql-database/scale-pool/scale-pool.sh "přesun databáze mezi fondy")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení

Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků, logického serveru, databáze SQL a pravidla brány firewall. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření az sql serveru](https://docs.microsoft.com/cli/azure/sql/server#create) | Vytvoří logického serveru, který je hostitelem databáze SQL. |
| [Vytvoření sql az Elastická fondy](https://docs.microsoft.com/cli/azure/sql/elastic-pool#create) | Vytvoří fond elastické databáze v rámci logického serveru. |
| [Vytvoření az sql db](https://docs.microsoft.com/cli/azure/sql/db#create) | Vytvoří databázi SQL v logického serveru. |
| [aktualizace elastického fondu sql az](https://docs.microsoft.com/cli/azure/sql/elastic-pool#update) | Aktualizace fondu elastické databáze, v tomto příkladu se změní přiřazené eDTU. |
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/vm/extension#set) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu SQL databáze rozhraní příkazového řádku najdete v [dokumentace Azure SQL Database](../sql-database-cli-samples.md).
