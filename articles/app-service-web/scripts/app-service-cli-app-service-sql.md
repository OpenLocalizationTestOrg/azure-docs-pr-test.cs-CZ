---
title: "Azure CLI skriptu ukázkové – webovou aplikaci připojit k databázi SQL | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – webovou aplikaci připojit k databázi SQL"
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
ms.openlocfilehash: ec5e22bfacc12a89f1fb5882487df4829369777c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-a-web-app-to-a-sql-database"></a><span data-ttu-id="ce32f-103">Webovou aplikaci připojit k databázi SQL</span><span class="sxs-lookup"><span data-stu-id="ce32f-103">Connect a web app to a SQL database</span></span>

<span data-ttu-id="ce32f-104">V tomto scénáři se dozvíte, jak vytvářet Azure SQL database a webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="ce32f-104">In this scenario you will learn how to create an Azure SQL database and an Azure web app.</span></span> <span data-ttu-id="ce32f-105">Potom propojíte databáze SQL pro webovou aplikaci pomocí nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="ce32f-105">Then you will link the SQL database to the web app using app settings.</span></span>


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="ce32f-106">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ce32f-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="ce32f-107">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="ce32f-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="ce32f-108">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ce32f-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="ce32f-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="ce32f-109">Sample script</span></span>

<span data-ttu-id="ce32f-110">[!code-azurecli-interactive[hlavní](../../../cli_scripts/app-service/connect-to-sql/connect-to-sql.sh?highlight=9-10 "databáze SQL")]</span><span class="sxs-lookup"><span data-stu-id="ce32f-110">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-sql/connect-to-sql.sh?highlight=9-10 "SQL Database")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="ce32f-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="ce32f-111">Script explanation</span></span>

<span data-ttu-id="ce32f-112">Tento skript používá následující příkazy k vytvoření skupiny prostředků, webové aplikace, databáze SQL a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="ce32f-112">This script uses the following commands to create a resource group, web app, SQL database and all related resources.</span></span> <span data-ttu-id="ce32f-113">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="ce32f-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="ce32f-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="ce32f-114">Command</span></span> | <span data-ttu-id="ce32f-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="ce32f-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ce32f-116">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="ce32f-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="ce32f-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="ce32f-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="ce32f-118">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="ce32f-118">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="ce32f-119">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="ce32f-119">Creates an App Service plan.</span></span> <span data-ttu-id="ce32f-120">Toto je jako serverové farmy pro Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ce32f-120">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="ce32f-121">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="ce32f-121">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="ce32f-122">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="ce32f-122">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="ce32f-123">Vytvoření az sql serveru</span><span class="sxs-lookup"><span data-stu-id="ce32f-123">az sql server create</span></span>](https://docs.microsoft.com/cli/azure/sql/server#create) | <span data-ttu-id="ce32f-124">Vytvoří databázi serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="ce32f-124">Creates a SQL Database Server.</span></span>  |
| [<span data-ttu-id="ce32f-125">Vytvoření az sql db</span><span class="sxs-lookup"><span data-stu-id="ce32f-125">az sql db create</span></span>](https://docs.microsoft.com/cli/azure/sql/db#create) | <span data-ttu-id="ce32f-126">Vytvoří novou databázi s databázovým serverem SQL.</span><span class="sxs-lookup"><span data-stu-id="ce32f-126">Creates a new database with the SQL Database Server.</span></span> |
| [<span data-ttu-id="ce32f-127">AZ webapp konfigurace appsettings sady</span><span class="sxs-lookup"><span data-stu-id="ce32f-127">az webapp config appsettings set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | <span data-ttu-id="ce32f-128">Vytvoří nebo aktualizuje nastavení aplikace pro webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="ce32f-128">Creates or updates an app setting for an Azure web app.</span></span> <span data-ttu-id="ce32f-129">Nastavení aplikace jsou zveřejněné jako proměnné prostředí pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ce32f-129">App settings are exposed as environment variables for your app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ce32f-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ce32f-130">Next steps</span></span>

<span data-ttu-id="ce32f-131">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ce32f-131">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="ce32f-132">Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="ce32f-132">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
