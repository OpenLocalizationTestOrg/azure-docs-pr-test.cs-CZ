---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření webové aplikace ASP.NET Core v kontejner Docker | Microsoft Docs"
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
ms.openlocfilehash: 23106345bfbbf1f68757d99010db98e7c9a7da49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-aspnet-core-web-app-in-a-docker-container"></a><span data-ttu-id="7bf5b-103">Vytvoření webové aplikace ASP.NET Core v kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="7bf5b-103">Create an ASP.NET Core web app in a Docker container</span></span>

<span data-ttu-id="7bf5b-104">V tomto scénáři se dozvíte, jak toocreate skupinu prostředků Linux aplikace služby plánování a webové aplikace a nasazení aplikace technologie ASP.NET Core pomocí kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="7bf5b-104">In this scenario you will learn how toocreate a resource group, Linux app service plan, and web app, and deploy an ASP.NET Core application using a Docker Container.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="7bf5b-105">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="7bf5b-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="7bf5b-106">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="7bf5b-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="7bf5b-107">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="7bf5b-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="sample-script"></a><span data-ttu-id="7bf5b-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="7bf5b-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-linux-docker/deploy-linux-docker.sh?highlight=6 "Linux Docker")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="7bf5b-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="7bf5b-109">Script explanation</span></span>

<span data-ttu-id="7bf5b-110">Tento skript používá hello následující příkazy toocreate skupinu prostředků, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="7bf5b-110">This script uses hello following commands toocreate a resource group, web app, and all related resources.</span></span> <span data-ttu-id="7bf5b-111">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="7bf5b-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="7bf5b-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="7bf5b-112">Command</span></span> | <span data-ttu-id="7bf5b-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="7bf5b-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="7bf5b-114">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="7bf5b-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="7bf5b-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="7bf5b-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="7bf5b-116">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="7bf5b-116">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="7bf5b-117">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="7bf5b-117">Creates an App Service plan.</span></span> <span data-ttu-id="7bf5b-118">Toto je jako serverové farmy pro Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="7bf5b-118">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="7bf5b-119">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="7bf5b-119">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="7bf5b-120">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="7bf5b-120">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="7bf5b-121">AZ webapp konfigurace kontejneru sady</span><span class="sxs-lookup"><span data-stu-id="7bf5b-121">az webapp config container set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/container#set) | <span data-ttu-id="7bf5b-122">Nastaví kontejner Docker hello hello webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="7bf5b-122">Sets hello Docker container for hello Azure web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="7bf5b-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7bf5b-123">Next steps</span></span>

<span data-ttu-id="7bf5b-124">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7bf5b-124">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="7bf5b-125">Další ukázky skriptu rozhraní příkazového řádku služby aplikace naleznete v hello [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="7bf5b-125">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
