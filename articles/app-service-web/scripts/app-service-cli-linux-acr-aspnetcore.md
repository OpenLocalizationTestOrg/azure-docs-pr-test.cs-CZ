---
title: "Azure CLI skriptu ukázkové – vytvoření webové aplikace ASP.NET Core v kontejner Docker z registru kontejner Azure | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření webové aplikace ASP.NET Core v kontejner Docker z registru kontejner Azure"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 3a2d1983-ff7b-476a-ac44-49ec2aabb31a
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 2556947d7cdd1475ae82ac2e1d61ad30ebd0d29f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-aspnet-core-web-app-in-a-docker-container-from-azure-container-registry"></a><span data-ttu-id="068b1-103">Vytvoření webové aplikace ASP.NET Core v kontejner Docker z registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="068b1-103">Create an ASP.NET Core web app in a Docker container from Azure Container Registry</span></span>

<span data-ttu-id="068b1-104">V tomto scénáři se dozvíte, jak vytvořit skupinu prostředků, Linux plán služby app service a webové aplikace a nasazovat aplikaci ASP.NET Core pomocí kontejner Docker z registru kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="068b1-104">In this scenario you will learn how to create a resource group, Linux app service plan, and web app, and deploy an ASP.NET Core application using a Docker Container from the Azure Container Registry.</span></span>


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="068b1-105">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="068b1-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="068b1-106">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="068b1-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="068b1-107">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="068b1-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="068b1-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="068b1-108">Sample script</span></span>

<span data-ttu-id="068b1-109">[!code-azurecli-interactive[hlavní](../../../cli_scripts/app-service/deploy-linux-acr/deploy-linux-acr.sh?highlight=6-9 "registru kontejner Azure Linux")]</span><span class="sxs-lookup"><span data-stu-id="068b1-109">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-linux-acr/deploy-linux-acr.sh?highlight=6-9 "Linux Azure Container Registry")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="068b1-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="068b1-110">Script explanation</span></span>

<span data-ttu-id="068b1-111">Tento skript používá následující příkazy k vytvoření skupiny prostředků, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="068b1-111">This script uses the following commands to create a resource group, web app, and all related resources.</span></span> <span data-ttu-id="068b1-112">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="068b1-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="068b1-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="068b1-113">Command</span></span> | <span data-ttu-id="068b1-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="068b1-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="068b1-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="068b1-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="068b1-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="068b1-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="068b1-117">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="068b1-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="068b1-118">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="068b1-118">Creates an App Service plan.</span></span> <span data-ttu-id="068b1-119">Toto je jako serverové farmy pro Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="068b1-119">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="068b1-120">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="068b1-120">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="068b1-121">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="068b1-121">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="068b1-122">AZ webapp konfigurace kontejneru sady</span><span class="sxs-lookup"><span data-stu-id="068b1-122">az webapp config container set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/container#set) | <span data-ttu-id="068b1-123">Nastaví kontejner Docker pro webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="068b1-123">Sets the Docker container for the Azure web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="068b1-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="068b1-124">Next steps</span></span>

<span data-ttu-id="068b1-125">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="068b1-125">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="068b1-126">Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="068b1-126">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
