---
title: "Ukázka skriptu Azure CLI - mapy vlastní doménu do webové aplikace | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - mapy vlastní doménu do webové aplikace"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 5ac4a680-cc73-4578-bcd6-8668c08802c2
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 6712be8a551731fbafd92ef19564e89399e23e76
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="map-a-custom-domain-to-a-web-app"></a><span data-ttu-id="f81ff-103">Namapovat vlastní doménu do webové aplikace</span><span class="sxs-lookup"><span data-stu-id="f81ff-103">Map a custom domain to a web app</span></span>

<span data-ttu-id="f81ff-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a potom mapuje `www.<yourdomain>` k němu.</span><span class="sxs-lookup"><span data-stu-id="f81ff-104">This sample script creates a web app in App Service with its related resources, and then maps `www.<yourdomain>` to it.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="f81ff-105">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f81ff-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="f81ff-106">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="f81ff-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="f81ff-107">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="f81ff-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="f81ff-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="f81ff-108">Sample script</span></span>

<span data-ttu-id="f81ff-109">[!code-azurecli-interactive[hlavní](../../../cli_scripts/app-service/configure-custom-domain/configure-custom-domain.sh?highlight=3 "namapovat vlastní doménu do webové aplikace")]</span><span class="sxs-lookup"><span data-stu-id="f81ff-109">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Map a custom domain to a web app")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="f81ff-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="f81ff-110">Script explanation</span></span>

<span data-ttu-id="f81ff-111">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="f81ff-111">This script uses the following commands.</span></span> <span data-ttu-id="f81ff-112">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="f81ff-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="f81ff-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="f81ff-113">Command</span></span> | <span data-ttu-id="f81ff-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="f81ff-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f81ff-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="f81ff-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="f81ff-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="f81ff-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="f81ff-117">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="f81ff-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="f81ff-118">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="f81ff-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="f81ff-119">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="f81ff-119">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="f81ff-120">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="f81ff-120">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="f81ff-121">Přidejte název az webapp konfigurace hostitele</span><span class="sxs-lookup"><span data-stu-id="f81ff-121">az webapp config hostname add</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/hostname#add) | <span data-ttu-id="f81ff-122">Mapuje vlastní doménu do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f81ff-122">Maps a custom domain to a web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f81ff-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f81ff-123">Next steps</span></span>

<span data-ttu-id="f81ff-124">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f81ff-124">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="f81ff-125">Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="f81ff-125">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
