---
title: "Azure CLI skriptu ukázkové – vytvoření webové aplikace ASP.NET Core v kontejner Docker | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření webové aplikace ASP.NET Core v kontejner Docker"
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
ms.openlocfilehash: 5d1e2c7d2815df5fb9c176ec290a18d330ea3f7c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-aspnet-core-web-app-in-a-docker-container"></a><span data-ttu-id="f5525-103">Vytvoření webové aplikace ASP.NET Core v kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="f5525-103">Create an ASP.NET Core web app in a Docker container</span></span>

<span data-ttu-id="f5525-104">V tomto scénáři se dozvíte, jak vytvořit skupinu prostředků, Linux plán služby app service a webové aplikace a nasazovat aplikaci ASP.NET Core pomocí kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="f5525-104">In this scenario you will learn how to create a resource group, Linux app service plan, and web app, and deploy an ASP.NET Core application using a Docker Container.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="f5525-105">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f5525-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="f5525-106">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="f5525-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="f5525-107">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="f5525-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="sample-script"></a><span data-ttu-id="f5525-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="f5525-108">Sample script</span></span>

<span data-ttu-id="f5525-109">[!code-azurecli-interactive[hlavní](../../../cli_scripts/app-service/deploy-linux-docker/deploy-linux-docker.sh?highlight=6 "Linux Docker")]</span><span class="sxs-lookup"><span data-stu-id="f5525-109">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-linux-docker/deploy-linux-docker.sh?highlight=6 "Linux Docker")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="f5525-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="f5525-110">Script explanation</span></span>

<span data-ttu-id="f5525-111">Tento skript používá následující příkazy k vytvoření skupiny prostředků, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="f5525-111">This script uses the following commands to create a resource group, web app, and all related resources.</span></span> <span data-ttu-id="f5525-112">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="f5525-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="f5525-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="f5525-113">Command</span></span> | <span data-ttu-id="f5525-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="f5525-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f5525-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="f5525-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="f5525-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="f5525-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="f5525-117">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="f5525-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="f5525-118">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="f5525-118">Creates an App Service plan.</span></span> <span data-ttu-id="f5525-119">Toto je jako serverové farmy pro Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f5525-119">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="f5525-120">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="f5525-120">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="f5525-121">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="f5525-121">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="f5525-122">AZ webapp konfigurace kontejneru sady</span><span class="sxs-lookup"><span data-stu-id="f5525-122">az webapp config container set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/container#set) | <span data-ttu-id="f5525-123">Nastaví kontejner Docker pro webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="f5525-123">Sets the Docker container for the Azure web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f5525-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f5525-124">Next steps</span></span>

<span data-ttu-id="f5525-125">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f5525-125">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="f5525-126">Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="f5525-126">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
