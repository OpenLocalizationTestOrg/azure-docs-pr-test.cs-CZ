---
title: "AAA \"Azure škálování skriptu rozhraní příkazového řádku Azure databázi PostgreSQL | Microsoft Docs\""
description: "Azure CLI ukázka skriptu - škálování databáze Azure pro úroveň výkonu různých tooa PostgreSQL serveru po dotazování hello metriky."
services: postgresql
author: salonisonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.custom: mvc
ms.topic: sample
ms.date: 05/31/2017
ms.openlocfilehash: 678b28941dbb4334cb374d4888991a00b44966b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-scale-a-single-postgresql-server-using-azure-cli"></a>Sledování a škálování jediného PostgreSQL serveru pomocí rozhraní příkazového řádku Azure
Tento ukázkový skript rozhraní příkazového řádku škáluje jedné databáze Azure pro úroveň různých výkonu tooa PostgreSQL serveru po dotazování hello metriky. 

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript
V tento ukázkový skript změňte hello zvýrazněná řádky toocustomize hello jméno a heslo správce. Nahraďte id předplatného hello používá hello az monitorování příkazů s vlastní id předplatného.[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/scale-postgresql-server.sh?highlight=15-16 "Create and scale Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení
Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/delete-postgresql.sh "Delete hello resource group.")]

## <a name="script-explanation"></a>Vysvětlení skriptu
Tento skript používá hello následující příkazy. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| **Příkaz** | **Poznámky k** |
|---|---|
| [Vytvoření skupiny az](/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [AZ postgres server vytvořit](/cli/azure/postgres/server#create) | Vytvoří PostgreSQL serveru, který je hostitelem databáze hello. |
| [seznam metriky az monitorování](/cli/azure/monitor/metrics#list) | Hodnota metriky seznamu hello hello prostředků. |
| [Odstranění skupiny az](/cli/azure/group#delete) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky
- Přečtěte si další informace o hello příkazového řádku Azure CLI: [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview)
- Zkuste další skripty: [ukázky rozhraní příkazového řádku Azure pro databázi Azure pro PostgreSQL](../sample-scripts-azure-cli.md)
- Přečtěte si další informace o škálování: [úrovně služeb](../concepts-service-tiers.md) a [výpočetní jednotky a jednotky úložiště](../concepts-compute-unit-and-storage.md)
