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
# <a name="monitor-and-scale-an-azure-database-for-mysql-server-using-azure-cli"></a><span data-ttu-id="8270e-103">Sledování a škálování Azure Database pro MySQL server pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="8270e-103">Monitor and scale an Azure Database for MySQL server using Azure CLI</span></span>
<span data-ttu-id="8270e-104">Tento ukázkový skript rozhraní příkazového řádku škáluje jedné databáze Azure pro úroveň různých výkonu tooa MySQL serveru po dotazování hello metriky.</span><span class="sxs-lookup"><span data-stu-id="8270e-104">This sample CLI script scales a single Azure Database for MySQL server tooa different performance level after querying hello metrics.</span></span>

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="8270e-105">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="8270e-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="8270e-106">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="8270e-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="8270e-107">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="8270e-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="8270e-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="8270e-108">Sample script</span></span>
<span data-ttu-id="8270e-109">V tento ukázkový skript změňte hello zvýrazněná řádky toocustomize hello jméno a heslo správce.</span><span class="sxs-lookup"><span data-stu-id="8270e-109">In this sample script, change hello highlighted lines toocustomize hello admin username and password.</span></span> <span data-ttu-id="8270e-110">Nahraďte id předplatného hello používá hello az monitorování příkazů s vlastní id předplatného.[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh?highlight=15-16 "Create and scale Azure Database for MySQL.")]</span><span class="sxs-lookup"><span data-stu-id="8270e-110">Replace hello subscription id used in hello az monitor commands with your own subscription id. [!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh?highlight=15-16 "Create and scale Azure Database for MySQL.")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="8270e-111">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="8270e-111">Clean up deployment</span></span>
<span data-ttu-id="8270e-112">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="8270e-112">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/delete-mysql.sh  "Delete hello resource group.")]

## <a name="script-explanation"></a><span data-ttu-id="8270e-113">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="8270e-113">Script explanation</span></span>
<span data-ttu-id="8270e-114">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="8270e-114">This script uses hello following commands.</span></span> <span data-ttu-id="8270e-115">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="8270e-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="8270e-116">**Příkaz**</span><span class="sxs-lookup"><span data-stu-id="8270e-116">**Command**</span></span> | <span data-ttu-id="8270e-117">**Poznámky k**</span><span class="sxs-lookup"><span data-stu-id="8270e-117">**Notes**</span></span> |
|---|---|
| [<span data-ttu-id="8270e-118">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="8270e-118">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="8270e-119">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="8270e-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="8270e-120">server databáze mysql az vytvořit</span><span class="sxs-lookup"><span data-stu-id="8270e-120">az mysql server create</span></span>](/cli/azure/mysql/server#create) | <span data-ttu-id="8270e-121">Vytvoří MySQL serveru, který je hostitelem databáze hello.</span><span class="sxs-lookup"><span data-stu-id="8270e-121">Creates a MySQL server that hosts hello databases.</span></span> |
| [<span data-ttu-id="8270e-122">seznam metriky az monitorování</span><span class="sxs-lookup"><span data-stu-id="8270e-122">az monitor metrics list</span></span>](/cli/azure/monitor/metrics#list) | <span data-ttu-id="8270e-123">Hodnota metriky seznamu hello hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="8270e-123">List hello metric value for hello resources.</span></span> |
| [<span data-ttu-id="8270e-124">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="8270e-124">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="8270e-125">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="8270e-125">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8270e-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8270e-126">Next steps</span></span>
- <span data-ttu-id="8270e-127">Přečtěte si další informace o hello příkazového řádku Azure CLI: [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8270e-127">Read more information on hello Azure CLI: [Azure CLI documentation](/cli/azure/overview).</span></span>
- <span data-ttu-id="8270e-128">Zkuste další skripty: [ukázky rozhraní příkazového řádku Azure pro databázi Azure pro databázi MySQL](../sample-scripts-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="8270e-128">Try additional scripts: [Azure CLI samples for Azure Database for MySQL](../sample-scripts-azure-cli.md)</span></span>
- <span data-ttu-id="8270e-129">Další informace o škálování najdete v tématu [úrovně služeb](../concepts-service-tiers.md) a [výpočetní jednotky a jednotek úložiště](../concepts-compute-unit-and-storage.md).</span><span class="sxs-lookup"><span data-stu-id="8270e-129">For more information on scaling, see [Service Tiers](../concepts-service-tiers.md) and [Compute Units and Storage Units](../concepts-compute-unit-and-storage.md).</span></span>
