---
title: "Vytvoření funkce aplikace a nasazení funkce kód z Visual Studio Team Services | Microsoft Docs"
description: "Vytvoření funkce aplikace a nasazení funkce kód z Visual Studio Team Services"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/28/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: 2ef177b55ad7ffd351156821f429e6ff8fbeccc7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-app-service"></a><span data-ttu-id="9ac85-103">Vytvoření služby aplikace</span><span class="sxs-lookup"><span data-stu-id="9ac85-103">Create an App Service</span></span>

<span data-ttu-id="9ac85-104">V tomto scénáři se dozvíte, jak vytvořit funkce aplikace pomocí [plánu spotřeby](../functions-scale.md#consumption-plan) s jejími související prostředky a průběžně nasadí funkce kódu z úložiště Visual Studio Team Services (VSTS).</span><span class="sxs-lookup"><span data-stu-id="9ac85-104">In this scenario you will learn how to create a function app using the [consumption plan](../functions-scale.md#consumption-plan) with its related resources, and continuously deploys your function code from a Visual Studio Team Services (VSTS) repository.</span></span> <span data-ttu-id="9ac85-105">V této ukázce budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="9ac85-105">In this sample, you will need:</span></span>

* <span data-ttu-id="9ac85-106">Služby VSTS úložiště s kódem funkce, které mají oprávnění pro správu.</span><span class="sxs-lookup"><span data-stu-id="9ac85-106">A VSTS repository with functions code, that you have administrative permissions for.</span></span>
* <span data-ttu-id="9ac85-107">A [Personal Access Token (Jan)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) pro váš účet GitHub.</span><span class="sxs-lookup"><span data-stu-id="9ac85-107">A [Personal Access Token (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) for your GitHub account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="9ac85-108">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="9ac85-108">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="9ac85-109">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="9ac85-109">Run `az --version` to find the version.</span></span> <span data-ttu-id="9ac85-110">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9ac85-110">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="9ac85-111">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="9ac85-111">Sample script</span></span>

<span data-ttu-id="9ac85-112">Tato ukázka funkce Azure aplikace vytvoří a nasadí funkce kód z Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="9ac85-112">This sample creates an Azure Function app and deploys function code from Visual Studio Team Services.</span></span>

<span data-ttu-id="9ac85-113">[!code-azurecli-interactive[hlavní](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "služby Azure")]</span><span class="sxs-lookup"><span data-stu-id="9ac85-113">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="9ac85-114">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="9ac85-114">Script explanation</span></span>

<span data-ttu-id="9ac85-115">Tento skript používá následující příkazy k vytvoření skupiny prostředků, webové aplikace, documentdb a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="9ac85-115">This script uses the following commands to create a resource group, web app, documentdb and all related resources.</span></span> <span data-ttu-id="9ac85-116">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="9ac85-116">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="9ac85-117">Příkaz</span><span class="sxs-lookup"><span data-stu-id="9ac85-117">Command</span></span> | <span data-ttu-id="9ac85-118">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9ac85-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="9ac85-119">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="9ac85-119">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="9ac85-120">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="9ac85-120">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="9ac85-121">Vytvořit účet úložiště az</span><span class="sxs-lookup"><span data-stu-id="9ac85-121">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="9ac85-122">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="9ac85-122">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="9ac85-123">Vytvoření az functionapp</span><span class="sxs-lookup"><span data-stu-id="9ac85-123">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/appservice/web#delete) |
| [<span data-ttu-id="9ac85-124">Konfigurace správy zdrojového kódu webové služby App Service az</span><span class="sxs-lookup"><span data-stu-id="9ac85-124">az appservice web source-control config</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | <span data-ttu-id="9ac85-125">Přidruží aplikaci funkce Git nebo Mercurial úložiště.</span><span class="sxs-lookup"><span data-stu-id="9ac85-125">Associates a function app with a Git or Mercurial repository.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="9ac85-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9ac85-126">Next steps</span></span>

<span data-ttu-id="9ac85-127">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9ac85-127">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="9ac85-128">Další ukázky skriptu rozhraní příkazového řádku funkce Azure lze nalézt v [dokumentace Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="9ac85-128">Additional Azure Functions CLI script samples can be found in the [Azure Functions documentation](../functions-cli-samples.md).</span></span>
