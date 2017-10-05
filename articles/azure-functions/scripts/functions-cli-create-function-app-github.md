---
title: "Vytvoření funkce aplikace a nasazení funkce kód z Githubu | Microsoft Docs"
description: "Ukázka skriptu Azure CLI – vytvoření funkce aplikace a nasazení kódu funkce z Githubu"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/27/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: d67e85f91c80efe464fceb1105243bedfba83a0f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-function-app-and-deploy-function-code-from-github"></a><span data-ttu-id="0836a-103">Vytvoření funkce aplikace a nasazení funkce kód z Githubu</span><span class="sxs-lookup"><span data-stu-id="0836a-103">Create a function app and deploy function code from GitHub</span></span>

<span data-ttu-id="0836a-104">Tento ukázkový skript vytvoří funkce aplikace pomocí [plánu spotřeby](../functions-scale.md#consumption-plan) s související prostředky a nasadí funkce kódu z veřejného úložiště GitHub (bez průběžné nasazování).</span><span class="sxs-lookup"><span data-stu-id="0836a-104">This sample script creates a function app using the [consumption plan](../functions-scale.md#consumption-plan) with its related resources, and deploys your function code from a public GitHub repository (without continuous deployment).</span></span> <span data-ttu-id="0836a-105">Průběžné doručování kód funkce z Githubu, najdete v tématu [vytvořit aplikaci funkce a průběžně nasadit z Githubu](functions-cli-create-function-app-github-continuous.md)</span><span class="sxs-lookup"><span data-stu-id="0836a-105">For continuous delivery of function code from GitHub, read [Create a function app and continuously deploy from GitHub](functions-cli-create-function-app-github-continuous.md)</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="0836a-106">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="0836a-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="0836a-107">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="0836a-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="0836a-108">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="0836a-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="0836a-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="0836a-109">Sample script</span></span>

<span data-ttu-id="0836a-110">Tato ukázka funkce Azure aplikace vytvoří a nasadí kód funkce z Githubu.</span><span class="sxs-lookup"><span data-stu-id="0836a-110">This sample creates an Azure Function app and deploys function code from GitHub.</span></span>

<span data-ttu-id="0836a-111">[!code-azurecli-interactive[hlavní](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github/deploy-function-app-with-function-github.sh?highlight=3 "vytvoření aplikace pro funkce pomocí nasazení z Githubu")]</span><span class="sxs-lookup"><span data-stu-id="0836a-111">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github/deploy-function-app-with-function-github.sh?highlight=3 "Create a function app with deployment from GitHub")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="0836a-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="0836a-112">Script explanation</span></span>

<span data-ttu-id="0836a-113">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="0836a-113">Each command in the table links to command specific documentation.</span></span> <span data-ttu-id="0836a-114">Tento skript používá následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="0836a-114">This script uses the following commands:</span></span>

| <span data-ttu-id="0836a-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="0836a-115">Command</span></span> | <span data-ttu-id="0836a-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="0836a-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="0836a-117">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="0836a-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="0836a-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="0836a-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="0836a-119">Vytvořit účet úložiště az</span><span class="sxs-lookup"><span data-stu-id="0836a-119">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="0836a-120">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="0836a-120">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="0836a-121">Vytvoření az functionapp</span><span class="sxs-lookup"><span data-stu-id="0836a-121">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/appservice/web#delete) | <span data-ttu-id="0836a-122">Vytvoří aplikaci Azure funkce.</span><span class="sxs-lookup"><span data-stu-id="0836a-122">Creates an Azure Function app.</span></span> |
| [<span data-ttu-id="0836a-123">Konfigurace správy zdrojového kódu webové služby App Service az</span><span class="sxs-lookup"><span data-stu-id="0836a-123">az appservice web source-control config</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | <span data-ttu-id="0836a-124">Přidruží aplikaci funkce Git nebo Mercurial úložiště.</span><span class="sxs-lookup"><span data-stu-id="0836a-124">Associates a function app with a Git or Mercurial repository.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="0836a-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0836a-125">Next steps</span></span>

<span data-ttu-id="0836a-126">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="0836a-126">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="0836a-127">Další ukázky skriptu rozhraní příkazového řádku funkce Azure lze nalézt v [dokumentace Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="0836a-127">Additional Azure Functions CLI script samples can be found in the [Azure Functions documentation](../functions-cli-samples.md).</span></span>
