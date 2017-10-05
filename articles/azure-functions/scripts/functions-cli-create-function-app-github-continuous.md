---
title: "Vytvoření funkce aplikace a nasazení funkce kód z Githubu | Microsoft Docs"
description: "Vytvoření funkce aplikace a nasazení funkce kód z Githubu"
services: functions
ms.service: functions
keywords: 
ms.devlang: azurecli
author: syntaxc4
ms.author: cfowler
ms.date: 04/27/2017
ms.topic: sample
ms.custom: mvc
ms.openlocfilehash: 67eb41d89328ab57741c419d8b718e19b947dab1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-app-service"></a><span data-ttu-id="73da0-103">Vytvoření služby aplikace</span><span class="sxs-lookup"><span data-stu-id="73da0-103">Create an App Service</span></span>

<span data-ttu-id="73da0-104">Tento ukázkový skript vytvoří funkce aplikace pomocí [plánu spotřeby](../functions-scale.md#consumption-plan) s jejími související prostředky a průběžně nasadí funkce kódu z úložiště Githubu.</span><span class="sxs-lookup"><span data-stu-id="73da0-104">This sample script creates a function app using the [consumption plan](../functions-scale.md#consumption-plan) with its related resources, and continuously deploys your function code from a GitHub repository.</span></span> <span data-ttu-id="73da0-105">V této ukázce potřebujete:</span><span class="sxs-lookup"><span data-stu-id="73da0-105">In this sample, you need:</span></span>

* <span data-ttu-id="73da0-106">Úložiště GitHub funkce kód, který máte oprávnění pro správu.</span><span class="sxs-lookup"><span data-stu-id="73da0-106">A GitHub repository with functions code, that you have administrative permissions for.</span></span>
* <span data-ttu-id="73da0-107">A [Personal Access Token (Jan)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) pro váš účet GitHub.</span><span class="sxs-lookup"><span data-stu-id="73da0-107">A [Personal Access Token (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) for your GitHub account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="73da0-108">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="73da0-108">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="73da0-109">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="73da0-109">Run `az --version` to find the version.</span></span> <span data-ttu-id="73da0-110">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="73da0-110">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="73da0-111">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="73da0-111">Sample script</span></span>

<span data-ttu-id="73da0-112">Tato ukázka funkce Azure aplikace vytvoří a nasadí kód funkce z Githubu.</span><span class="sxs-lookup"><span data-stu-id="73da0-112">This sample creates an Azure Function app and deploys function code from GitHub.</span></span>

<span data-ttu-id="73da0-113">[!code-azurecli-interactive[hlavní](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github-continuous/deploy-function-app-with-function-github-continuous.sh?highlight=3-4 "služby Azure")]</span><span class="sxs-lookup"><span data-stu-id="73da0-113">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github-continuous/deploy-function-app-with-function-github-continuous.sh?highlight=3-4 "Azure Service")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="73da0-114">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="73da0-114">Script explanation</span></span>

<span data-ttu-id="73da0-115">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="73da0-115">Each command in the table links to command specific documentation.</span></span> <span data-ttu-id="73da0-116">Tento skript používá následující:</span><span class="sxs-lookup"><span data-stu-id="73da0-116">This script uses the following:</span></span>

| <span data-ttu-id="73da0-117">Příkaz</span><span class="sxs-lookup"><span data-stu-id="73da0-117">Command</span></span> | <span data-ttu-id="73da0-118">Poznámky</span><span class="sxs-lookup"><span data-stu-id="73da0-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="73da0-119">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="73da0-119">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="73da0-120">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="73da0-120">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="73da0-121">Vytvořit účet úložiště az</span><span class="sxs-lookup"><span data-stu-id="73da0-121">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="73da0-122">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="73da0-122">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="73da0-123">Vytvoření az functionapp</span><span class="sxs-lookup"><span data-stu-id="73da0-123">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/appservice/web#delete) |
| [<span data-ttu-id="73da0-124">Konfigurace správy zdrojového kódu webové služby App Service az</span><span class="sxs-lookup"><span data-stu-id="73da0-124">az appservice web source-control config</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | <span data-ttu-id="73da0-125">Přidruží aplikaci funkce Git nebo Mercurial úložiště.</span><span class="sxs-lookup"><span data-stu-id="73da0-125">Associates a function app with a Git or Mercurial repository.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="73da0-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="73da0-126">Next steps</span></span>

<span data-ttu-id="73da0-127">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="73da0-127">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="73da0-128">Další ukázky skriptu rozhraní příkazového řádku funkce Azure lze nalézt v [dokumentace Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="73da0-128">Additional Azure Functions CLI script samples can be found in the [Azure Functions documentation](../functions-cli-samples.md).</span></span>
