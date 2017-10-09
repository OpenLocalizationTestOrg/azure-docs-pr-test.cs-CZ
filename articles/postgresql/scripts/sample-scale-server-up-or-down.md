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
# <a name="monitor-and-scale-a-single-postgresql-server-using-azure-cli"></a><span data-ttu-id="61816-103">Sledování a škálování jediného PostgreSQL serveru pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="61816-103">Monitor and scale a single PostgreSQL server using Azure CLI</span></span>
<span data-ttu-id="61816-104">Tento ukázkový skript rozhraní příkazového řádku škáluje jedné databáze Azure pro úroveň různých výkonu tooa PostgreSQL serveru po dotazování hello metriky.</span><span class="sxs-lookup"><span data-stu-id="61816-104">This sample CLI script scales a single Azure Database for PostgreSQL server tooa different performance level after querying hello metrics.</span></span> 

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="61816-105">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="61816-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="61816-106">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="61816-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="61816-107">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="61816-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="61816-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="61816-108">Sample script</span></span>
<span data-ttu-id="61816-109">V tento ukázkový skript změňte hello zvýrazněná řádky toocustomize hello jméno a heslo správce.</span><span class="sxs-lookup"><span data-stu-id="61816-109">In this sample script, change hello highlighted lines toocustomize hello admin username and password.</span></span> <span data-ttu-id="61816-110">Nahraďte id předplatného hello používá hello az monitorování příkazů s vlastní id předplatného.[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/scale-postgresql-server.sh?highlight=15-16 "Create and scale Azure Database for PostgreSQL.")]</span><span class="sxs-lookup"><span data-stu-id="61816-110">Replace hello subscription id used in hello az monitor commands with your own subscription id. [!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/scale-postgresql-server.sh?highlight=15-16 "Create and scale Azure Database for PostgreSQL.")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="61816-111">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="61816-111">Clean up deployment</span></span>
<span data-ttu-id="61816-112">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="61816-112">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/delete-postgresql.sh "Delete hello resource group.")]

## <a name="script-explanation"></a><span data-ttu-id="61816-113">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="61816-113">Script explanation</span></span>
<span data-ttu-id="61816-114">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="61816-114">This script uses hello following commands.</span></span> <span data-ttu-id="61816-115">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="61816-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="61816-116">**Příkaz**</span><span class="sxs-lookup"><span data-stu-id="61816-116">**Command**</span></span> | <span data-ttu-id="61816-117">**Poznámky k**</span><span class="sxs-lookup"><span data-stu-id="61816-117">**Notes**</span></span> |
|---|---|
| [<span data-ttu-id="61816-118">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="61816-118">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="61816-119">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="61816-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="61816-120">AZ postgres server vytvořit</span><span class="sxs-lookup"><span data-stu-id="61816-120">az postgres server create</span></span>](/cli/azure/postgres/server#create) | <span data-ttu-id="61816-121">Vytvoří PostgreSQL serveru, který je hostitelem databáze hello.</span><span class="sxs-lookup"><span data-stu-id="61816-121">Creates a PostgreSQL server that hosts hello databases.</span></span> |
| [<span data-ttu-id="61816-122">seznam metriky az monitorování</span><span class="sxs-lookup"><span data-stu-id="61816-122">az monitor metrics list</span></span>](/cli/azure/monitor/metrics#list) | <span data-ttu-id="61816-123">Hodnota metriky seznamu hello hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="61816-123">List hello metric value for hello resources.</span></span> |
| [<span data-ttu-id="61816-124">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="61816-124">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="61816-125">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="61816-125">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="61816-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="61816-126">Next steps</span></span>
- <span data-ttu-id="61816-127">Přečtěte si další informace o hello příkazového řádku Azure CLI: [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview)</span><span class="sxs-lookup"><span data-stu-id="61816-127">Read more information on hello Azure CLI: [Azure CLI documentation](/cli/azure/overview)</span></span>
- <span data-ttu-id="61816-128">Zkuste další skripty: [ukázky rozhraní příkazového řádku Azure pro databázi Azure pro PostgreSQL](../sample-scripts-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="61816-128">Try additional scripts: [Azure CLI samples for Azure Database for PostgreSQL](../sample-scripts-azure-cli.md)</span></span>
- <span data-ttu-id="61816-129">Přečtěte si další informace o škálování: [úrovně služeb](../concepts-service-tiers.md) a [výpočetní jednotky a jednotky úložiště](../concepts-compute-unit-and-storage.md)</span><span class="sxs-lookup"><span data-stu-id="61816-129">Read more information on scaling: [Service Tiers](../concepts-service-tiers.md) and [Compute Units and Storage Units](../concepts-compute-unit-and-storage.md)</span></span>
