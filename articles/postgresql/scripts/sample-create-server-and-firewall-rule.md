---
title: "AAA \"Azure CLI Script – vytvoření Azure databáze pro PostgreSQL | Microsoft Docs\""
description: "Ukázka skriptu Azure CLI - vytvoří databázi Azure pro PostgreSQL server a nakonfiguruje pravidla brány firewall na úrovni serveru."
services: postgresql
author: salonisonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: azure-cli
ms.topic: sample
ms.date: 05/31/2017
ms.openlocfilehash: bbe31f77283aa4a3bb36d1fd9171c280594a8267
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-database-for-postgresql-server-and-configure-a-firewall-rule-using-hello-azure-cli"></a>Vytvoření databáze Azure pro PostgreSQL server a nakonfigurujte pravidlo brány firewall pomocí hello rozhraní příkazového řádku Azure
Tento ukázkový skript rozhraní příkazového řádku vytvoří databázi Azure pro PostgreSQL server a nakonfiguruje pravidla brány firewall na úrovni serveru. Jakmile hello skript byl úspěšně spuštěn, hello PostgreSQL server je přístupná ze všech služeb Azure a hello nakonfigurovat IP adresu.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript
V tento ukázkový skript upravte hello zvýrazněná řádky toocustomize hello jméno a heslo správce.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/create-postgresql-server-and-firewall-rule.sh?highlight=15-16 "Create an Azure Database for PostgreSQL, and server-level firewall rule.")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení
Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/delete-postgresql.sh "Delete hello resource group.")]

## <a name="script-explanation"></a>Vysvětlení skriptu
Tento skript používá hello následující příkazy. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| **Příkaz** | **Poznámky k** |
|---|---|
| [Vytvoření skupiny az](/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [AZ postgres server vytvořit](/cli/azure/postgres/server#create) | Vytvoří PostgreSQL serveru, který je hostitelem databáze hello. |
| [Vytvoření brány firewall serveru postgres az](/cli/azure/postgres/server/firewall-rule#create) | Vytvoří server toohello přístup tooallow pravidlo brány firewall a databáze v něm z hello zadaný rozsah IP adres. |
| [Odstranění skupiny az](/cli/azure/group#delete) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky
- Přečtěte si další informace o hello příkazového řádku Azure CLI: [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview)
- Zkuste další skripty: [ukázky rozhraní příkazového řádku Azure pro databázi Azure pro PostgreSQL](../sample-scripts-azure-cli.md)
