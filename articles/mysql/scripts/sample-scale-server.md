---
title: "Ukázky tooscale databázi Azure pro server databáze MySQL aaaAzure rozhraní příkazového řádku | Microsoft Docs"
description: "Tento ukázkový skript rozhraní příkazového řádku škáluje Azure databáze MySQL serveru tooa výkonu různé úrovně po dotazování hello metriky."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 05/31/2017
ms.openlocfilehash: 721ef9db35a5f3be7a38438c1abb724187b18b75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-scale-an-azure-database-for-mysql-server-using-azure-cli"></a>Sledování a škálování Azure Database pro MySQL server pomocí rozhraní příkazového řádku Azure
Tento ukázkový skript rozhraní příkazového řádku škáluje jedné databáze Azure pro úroveň různých výkonu tooa MySQL serveru po dotazování hello metriky.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript
V tento ukázkový skript změňte hello zvýrazněná řádky toocustomize hello jméno a heslo správce. Nahraďte id předplatného hello používá hello az monitorování příkazů s vlastní id předplatného.[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh?highlight=15-16 "Create and scale Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení
Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/delete-mysql.sh  "Delete hello resource group.")]

## <a name="script-explanation"></a>Vysvětlení skriptu
Tento skript používá hello následující příkazy. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| **Příkaz** | **Poznámky k** |
|---|---|
| [Vytvoření skupiny az](/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [server databáze mysql az vytvořit](/cli/azure/mysql/server#create) | Vytvoří MySQL serveru, který je hostitelem databáze hello. |
| [seznam metriky az monitorování](/cli/azure/monitor/metrics#list) | Hodnota metriky seznamu hello hello prostředků. |
| [Odstranění skupiny az](/cli/azure/group#delete) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky
- Přečtěte si další informace o hello příkazového řádku Azure CLI: [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).
- Zkuste další skripty: [ukázky rozhraní příkazového řádku Azure pro databázi Azure pro databázi MySQL](../sample-scripts-azure-cli.md)
- Další informace o škálování najdete v tématu [úrovně služeb](../concepts-service-tiers.md) a [výpočetní jednotky a jednotek úložiště](../concepts-compute-unit-and-storage.md).
