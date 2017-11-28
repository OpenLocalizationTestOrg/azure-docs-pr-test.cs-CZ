---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - připojit databázi SQL webové aplikace tooa | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – připojit databázi SQL tooa webové aplikace"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 7c2efdd0-f553-4038-a77a-e953021b3f77
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: adee42cd659d977b49e71d974d240324f68f67f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-web-app-tooa-sql-database"></a><span data-ttu-id="11c8b-103">Připojit databáze SQL tooa webové aplikace</span><span class="sxs-lookup"><span data-stu-id="11c8b-103">Connect a web app tooa SQL database</span></span>

<span data-ttu-id="11c8b-104">V tomto scénáři se dozvíte, jak toocreate Azure SQL database a Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="11c8b-104">In this scenario you will learn how toocreate an Azure SQL database and an Azure web app.</span></span> <span data-ttu-id="11c8b-105">Potom propojíte hello SQL databáze toohello webovou aplikaci pomocí nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="11c8b-105">Then you will link hello SQL database toohello web app using app settings.</span></span>


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="11c8b-106">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="11c8b-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="11c8b-107">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="11c8b-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="11c8b-108">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="11c8b-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="11c8b-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="11c8b-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-sql/connect-to-sql.sh?highlight=9-10 "SQL Database")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="11c8b-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="11c8b-110">Script explanation</span></span>

<span data-ttu-id="11c8b-111">Tento skript používá hello následující příkazy toocreate skupinu prostředků, webové aplikace, databáze SQL a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="11c8b-111">This script uses hello following commands toocreate a resource group, web app, SQL database and all related resources.</span></span> <span data-ttu-id="11c8b-112">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="11c8b-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="11c8b-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="11c8b-113">Command</span></span> | <span data-ttu-id="11c8b-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="11c8b-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="11c8b-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="11c8b-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="11c8b-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="11c8b-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="11c8b-117">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="11c8b-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="11c8b-118">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="11c8b-118">Creates an App Service plan.</span></span> <span data-ttu-id="11c8b-119">Toto je jako serverové farmy pro Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="11c8b-119">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="11c8b-120">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="11c8b-120">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="11c8b-121">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="11c8b-121">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="11c8b-122">Vytvoření az sql serveru</span><span class="sxs-lookup"><span data-stu-id="11c8b-122">az sql server create</span></span>](https://docs.microsoft.com/cli/azure/sql/server#create) | <span data-ttu-id="11c8b-123">Vytvoří databázi serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="11c8b-123">Creates a SQL Database Server.</span></span>  |
| [<span data-ttu-id="11c8b-124">Vytvoření az sql db</span><span class="sxs-lookup"><span data-stu-id="11c8b-124">az sql db create</span></span>](https://docs.microsoft.com/cli/azure/sql/db#create) | <span data-ttu-id="11c8b-125">Vytvoří novou databázi s hello databáze serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="11c8b-125">Creates a new database with hello SQL Database Server.</span></span> |
| [<span data-ttu-id="11c8b-126">AZ webapp konfigurace appsettings sady</span><span class="sxs-lookup"><span data-stu-id="11c8b-126">az webapp config appsettings set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | <span data-ttu-id="11c8b-127">Vytvoří nebo aktualizuje nastavení aplikace pro webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="11c8b-127">Creates or updates an app setting for an Azure web app.</span></span> <span data-ttu-id="11c8b-128">Nastavení aplikace jsou zveřejněné jako proměnné prostředí pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="11c8b-128">App settings are exposed as environment variables for your app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="11c8b-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="11c8b-129">Next steps</span></span>

<span data-ttu-id="11c8b-130">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="11c8b-130">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="11c8b-131">Další ukázky skriptu rozhraní příkazového řádku služby aplikace naleznete v hello [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="11c8b-131">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
