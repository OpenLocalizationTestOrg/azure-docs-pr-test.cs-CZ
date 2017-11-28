---
title: "aaaCreate funkce aplikace a nasazení kódu funkce z Githubu | Microsoft Docs"
description: "Ukázka skriptu Azure CLI – vytvoření funkce aplikace a nasazení kódu funkce z Githubu"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/27/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: 026886f11909149db695d9a52d0aa37f109f64e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-app-and-deploy-function-code-from-github"></a><span data-ttu-id="3051a-103">Vytvoření funkce aplikace a nasazení funkce kód z Githubu</span><span class="sxs-lookup"><span data-stu-id="3051a-103">Create a function app and deploy function code from GitHub</span></span>

<span data-ttu-id="3051a-104">Tento ukázkový skript vytvoří aplikaci funkce pomocí hello [plánu spotřeby](../functions-scale.md#consumption-plan) s související prostředky a nasadí funkce kódu z veřejného úložiště GitHub (bez průběžné nasazování).</span><span class="sxs-lookup"><span data-stu-id="3051a-104">This sample script creates a function app using hello [consumption plan](../functions-scale.md#consumption-plan) with its related resources, and deploys your function code from a public GitHub repository (without continuous deployment).</span></span> <span data-ttu-id="3051a-105">Průběžné doručování kód funkce z Githubu, najdete v tématu [vytvořit aplikaci funkce a průběžně nasadit z Githubu](functions-cli-create-function-app-github-continuous.md)</span><span class="sxs-lookup"><span data-stu-id="3051a-105">For continuous delivery of function code from GitHub, read [Create a function app and continuously deploy from GitHub](functions-cli-create-function-app-github-continuous.md)</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="3051a-106">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="3051a-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="3051a-107">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="3051a-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="3051a-108">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="3051a-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="3051a-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="3051a-109">Sample script</span></span>

<span data-ttu-id="3051a-110">Tato ukázka funkce Azure aplikace vytvoří a nasadí kód funkce z Githubu.</span><span class="sxs-lookup"><span data-stu-id="3051a-110">This sample creates an Azure Function app and deploys function code from GitHub.</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github/deploy-function-app-with-function-github.sh?highlight=3 "Create a function app with deployment from GitHub")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="3051a-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="3051a-111">Script explanation</span></span>

<span data-ttu-id="3051a-112">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="3051a-112">Each command in hello table links toocommand specific documentation.</span></span> <span data-ttu-id="3051a-113">Tento skript používá hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="3051a-113">This script uses hello following commands:</span></span>

| <span data-ttu-id="3051a-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="3051a-114">Command</span></span> | <span data-ttu-id="3051a-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="3051a-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="3051a-116">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="3051a-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="3051a-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="3051a-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="3051a-118">Vytvořit účet úložiště az</span><span class="sxs-lookup"><span data-stu-id="3051a-118">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="3051a-119">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="3051a-119">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="3051a-120">Vytvoření az functionapp</span><span class="sxs-lookup"><span data-stu-id="3051a-120">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/appservice/web#delete) | <span data-ttu-id="3051a-121">Vytvoří aplikaci Azure funkce.</span><span class="sxs-lookup"><span data-stu-id="3051a-121">Creates an Azure Function app.</span></span> |
| [<span data-ttu-id="3051a-122">Konfigurace správy zdrojového kódu webové služby App Service az</span><span class="sxs-lookup"><span data-stu-id="3051a-122">az appservice web source-control config</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | <span data-ttu-id="3051a-123">Přidruží aplikaci funkce Git nebo Mercurial úložiště.</span><span class="sxs-lookup"><span data-stu-id="3051a-123">Associates a function app with a Git or Mercurial repository.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="3051a-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3051a-124">Next steps</span></span>

<span data-ttu-id="3051a-125">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3051a-125">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="3051a-126">Další ukázky skriptu rozhraní příkazového řádku funkce Azure lze nalézt v hello [dokumentace Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="3051a-126">Additional Azure Functions CLI script samples can be found in hello [Azure Functions documentation](../functions-cli-samples.md).</span></span>
