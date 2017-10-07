---
title: "aaaCLI příklad vytvoření Azure SQL database | Microsoft Docs"
description: "Azure CLI příklad skriptu toocreate databáze SQL"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 0d54e284e19f16387813e24d7beb7ab048a39263
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-cli-toocreate-a-single-azure-sql-database-and-configure-a-firewall-rule"></a><span data-ttu-id="d42b1-103">Použijte rozhraní příkazového řádku toocreate jedné databáze Azure SQL a nakonfigurujte pravidlo brány firewall</span><span class="sxs-lookup"><span data-stu-id="d42b1-103">Use CLI toocreate a single Azure SQL database and configure a firewall rule</span></span>

<span data-ttu-id="d42b1-104">Tento příklad skriptu rozhraní příkazového řádku Azure vytvoří Azure SQL database a nakonfigurujte pravidlo brány firewall na úrovni serveru.</span><span class="sxs-lookup"><span data-stu-id="d42b1-104">This Azure CLI script example creates an Azure SQL database and configure a server-level firewall rule.</span></span> <span data-ttu-id="d42b1-105">Po úspěšně spustit skript hello hello databáze SQL je přístupná ze všech služeb Azure a hello nakonfigurovat IP adresu.</span><span class="sxs-lookup"><span data-stu-id="d42b1-105">Once hello script has been successfully run, hello SQL Database can be accessed from all Azure services and hello configured IP address.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="d42b1-106">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="d42b1-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="d42b1-107">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="d42b1-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="d42b1-108">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d42b1-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="d42b1-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="d42b1-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/create-and-configure-database/create-and-configure-database.sh?highlight=9-10 "Create SQL Database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="d42b1-110">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="d42b1-110">Clean up deployment</span></span>

<span data-ttu-id="d42b1-111">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="d42b1-111">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="d42b1-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="d42b1-112">Script explanation</span></span>

<span data-ttu-id="d42b1-113">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="d42b1-113">This script uses hello following commands.</span></span> <span data-ttu-id="d42b1-114">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="d42b1-114">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="d42b1-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="d42b1-115">Command</span></span> | <span data-ttu-id="d42b1-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="d42b1-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d42b1-117">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="d42b1-117">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="d42b1-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="d42b1-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="d42b1-119">Vytvoření az sql serveru</span><span class="sxs-lookup"><span data-stu-id="d42b1-119">az sql server create</span></span>](/cli/azure/sql/server#create) | <span data-ttu-id="d42b1-120">Vytvoří logického serveru, hostitelé hello databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="d42b1-120">Creates a logical server that hosts hello SQL Database.</span></span> |
| [<span data-ttu-id="d42b1-121">Vytvoření brány firewall az sql serveru</span><span class="sxs-lookup"><span data-stu-id="d42b1-121">az sql server firewall create</span></span>](/cli/azure/sql/server/firewall-rule#create) | <span data-ttu-id="d42b1-122">Vytvoří brány firewall pravidla tooallow přístup tooall databází SQL na serveru hello z hello zadaný rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="d42b1-122">Creates a firewall rule tooallow access tooall SQL Databases on hello server from hello entered IP address range.</span></span> |
| [<span data-ttu-id="d42b1-123">Vytvoření az sql db</span><span class="sxs-lookup"><span data-stu-id="d42b1-123">az sql db create</span></span>](/cli/azure/sql/db#create) | <span data-ttu-id="d42b1-124">Vytvoří hello SQL Database v hello logického serveru.</span><span class="sxs-lookup"><span data-stu-id="d42b1-124">Creates hello SQL Database in hello logical server.</span></span> |
| [<span data-ttu-id="d42b1-125">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="d42b1-125">az group delete</span></span>](/cli/azure/resource#delete) | <span data-ttu-id="d42b1-126">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="d42b1-126">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d42b1-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d42b1-127">Next steps</span></span>

<span data-ttu-id="d42b1-128">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d42b1-128">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="d42b1-129">Další ukázky skriptu SQL databáze rozhraní příkazového řádku najdete v hello [dokumentace Azure SQL Database](../sql-database-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="d42b1-129">Additional SQL Database CLI script samples can be found in hello [Azure SQL Database documentation](../sql-database-cli-samples.md).</span></span>

