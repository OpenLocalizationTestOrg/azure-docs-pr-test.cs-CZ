---
title: "Ukázek Azure CLI se škálovat databázi Azure pro server databáze MySQL | Microsoft Docs"
description: "Tento ukázkový skript rozhraní příkazového řádku škáluje Azure databáze pro server databáze MySQL na úroveň výkonu různých po dotazování metriky."
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
ms.openlocfilehash: 33316ff3db382d25a444d55772c6ee4d7b7ac418
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-and-scale-an-azure-database-for-mysql-server-using-azure-cli"></a><span data-ttu-id="88125-103">Sledování a škálování Azure Database pro MySQL server pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="88125-103">Monitor and scale an Azure Database for MySQL server using Azure CLI</span></span>
<span data-ttu-id="88125-104">Tento ukázkový skript rozhraní příkazového řádku škáluje jedné databáze Azure pro server databáze MySQL na úroveň výkonu různých po dotazování metriky.</span><span class="sxs-lookup"><span data-stu-id="88125-104">This sample CLI script scales a single Azure Database for MySQL server to a different performance level after querying the metrics.</span></span>

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="88125-105">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="88125-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="88125-106">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="88125-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="88125-107">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="88125-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="88125-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="88125-108">Sample script</span></span>
<span data-ttu-id="88125-109">Tento ukázkový skript změňte zvýrazněné řádky k přizpůsobení uživatelské jméno správce a heslo.</span><span class="sxs-lookup"><span data-stu-id="88125-109">In this sample script, change the highlighted lines to customize the admin username and password.</span></span> <span data-ttu-id="88125-110">Nahraďte id předplatného použít v příkazech monitorování az s vlastní id předplatného.</span><span class="sxs-lookup"><span data-stu-id="88125-110">Replace the subscription id used in the az monitor commands with your own subscription id.</span></span>
<span data-ttu-id="88125-111">[!code-azurecli-interactive[hlavní](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh?highlight=15-16 "vytvořit a škálování Azure Database pro databázi MySQL.")]</span><span class="sxs-lookup"><span data-stu-id="88125-111">[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh?highlight=15-16 "Create and scale Azure Database for MySQL.")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="88125-112">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="88125-112">Clean up deployment</span></span>
<span data-ttu-id="88125-113">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="88125-113">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>
<span data-ttu-id="88125-114">[!code-azurecli-interactive[hlavní](../../../cli_scripts/mysql/scale-mysql-server/delete-mysql.sh  "Odstraňte skupinu prostředků.")]</span><span class="sxs-lookup"><span data-stu-id="88125-114">[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/delete-mysql.sh  "Delete the resource group.")]</span></span>

## <a name="script-explanation"></a><span data-ttu-id="88125-115">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="88125-115">Script explanation</span></span>
<span data-ttu-id="88125-116">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="88125-116">This script uses the following commands.</span></span> <span data-ttu-id="88125-117">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="88125-117">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="88125-118">**Příkaz**</span><span class="sxs-lookup"><span data-stu-id="88125-118">**Command**</span></span> | <span data-ttu-id="88125-119">**Poznámky k**</span><span class="sxs-lookup"><span data-stu-id="88125-119">**Notes**</span></span> |
|---|---|
| [<span data-ttu-id="88125-120">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="88125-120">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="88125-121">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="88125-121">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="88125-122">server databáze mysql az vytvořit</span><span class="sxs-lookup"><span data-stu-id="88125-122">az mysql server create</span></span>](/cli/azure/mysql/server#create) | <span data-ttu-id="88125-123">Vytvoří MySQL serveru, který je hostitelem databáze.</span><span class="sxs-lookup"><span data-stu-id="88125-123">Creates a MySQL server that hosts the databases.</span></span> |
| [<span data-ttu-id="88125-124">seznam metriky az monitorování</span><span class="sxs-lookup"><span data-stu-id="88125-124">az monitor metrics list</span></span>](/cli/azure/monitor/metrics#list) | <span data-ttu-id="88125-125">Zobrazí seznam hodnota metriky pro prostředky.</span><span class="sxs-lookup"><span data-stu-id="88125-125">List the metric value for the resources.</span></span> |
| [<span data-ttu-id="88125-126">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="88125-126">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="88125-127">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="88125-127">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="88125-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="88125-128">Next steps</span></span>
- <span data-ttu-id="88125-129">Přečtěte si další informace o Azure CLI: [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="88125-129">Read more information on the Azure CLI: [Azure CLI documentation](/cli/azure/overview).</span></span>
- <span data-ttu-id="88125-130">Zkuste další skripty: [ukázky rozhraní příkazového řádku Azure pro databázi Azure pro databázi MySQL](../sample-scripts-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="88125-130">Try additional scripts: [Azure CLI samples for Azure Database for MySQL](../sample-scripts-azure-cli.md)</span></span>
- <span data-ttu-id="88125-131">Další informace o škálování najdete v tématu [úrovně služeb](../concepts-service-tiers.md) a [výpočetní jednotky a jednotek úložiště](../concepts-compute-unit-and-storage.md).</span><span class="sxs-lookup"><span data-stu-id="88125-131">For more information on scaling, see [Service Tiers](../concepts-service-tiers.md) and [Compute Units and Storage Units](../concepts-compute-unit-and-storage.md).</span></span>
