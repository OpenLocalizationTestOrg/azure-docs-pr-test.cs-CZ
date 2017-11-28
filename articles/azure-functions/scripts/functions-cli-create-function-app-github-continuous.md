---
title: "aaaCreate funkce aplikace a nasazení kódu funkce z Githubu | Microsoft Docs"
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
ms.openlocfilehash: 4d44204b899b32af464260db51ed98bcf00eb2bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-app-service"></a><span data-ttu-id="8e188-103">Vytvoření služby aplikace</span><span class="sxs-lookup"><span data-stu-id="8e188-103">Create an App Service</span></span>

<span data-ttu-id="8e188-104">Tento ukázkový skript vytvoří aplikaci funkce pomocí hello [plánu spotřeby](../functions-scale.md#consumption-plan) s jejími související prostředky a průběžně nasadí funkce kódu z úložiště Githubu.</span><span class="sxs-lookup"><span data-stu-id="8e188-104">This sample script creates a function app using hello [consumption plan](../functions-scale.md#consumption-plan) with its related resources, and continuously deploys your function code from a GitHub repository.</span></span> <span data-ttu-id="8e188-105">V této ukázce potřebujete:</span><span class="sxs-lookup"><span data-stu-id="8e188-105">In this sample, you need:</span></span>

* <span data-ttu-id="8e188-106">Úložiště GitHub funkce kód, který máte oprávnění pro správu.</span><span class="sxs-lookup"><span data-stu-id="8e188-106">A GitHub repository with functions code, that you have administrative permissions for.</span></span>
* <span data-ttu-id="8e188-107">A [Personal Access Token (Jan)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) pro váš účet GitHub.</span><span class="sxs-lookup"><span data-stu-id="8e188-107">A [Personal Access Token (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) for your GitHub account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="8e188-108">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="8e188-108">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="8e188-109">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="8e188-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="8e188-110">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="8e188-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="8e188-111">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="8e188-111">Sample script</span></span>

<span data-ttu-id="8e188-112">Tato ukázka funkce Azure aplikace vytvoří a nasadí kód funkce z Githubu.</span><span class="sxs-lookup"><span data-stu-id="8e188-112">This sample creates an Azure Function app and deploys function code from GitHub.</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github-continuous/deploy-function-app-with-function-github-continuous.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="8e188-113">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="8e188-113">Script explanation</span></span>

<span data-ttu-id="8e188-114">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="8e188-114">Each command in hello table links toocommand specific documentation.</span></span> <span data-ttu-id="8e188-115">Tento skript používá hello následující:</span><span class="sxs-lookup"><span data-stu-id="8e188-115">This script uses hello following:</span></span>

| <span data-ttu-id="8e188-116">Příkaz</span><span class="sxs-lookup"><span data-stu-id="8e188-116">Command</span></span> | <span data-ttu-id="8e188-117">Poznámky</span><span class="sxs-lookup"><span data-stu-id="8e188-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="8e188-118">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="8e188-118">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="8e188-119">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="8e188-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="8e188-120">Vytvořit účet úložiště az</span><span class="sxs-lookup"><span data-stu-id="8e188-120">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="8e188-121">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="8e188-121">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="8e188-122">Vytvoření az functionapp</span><span class="sxs-lookup"><span data-stu-id="8e188-122">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/appservice/web#delete) |
| [<span data-ttu-id="8e188-123">Konfigurace správy zdrojového kódu webové služby App Service az</span><span class="sxs-lookup"><span data-stu-id="8e188-123">az appservice web source-control config</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | <span data-ttu-id="8e188-124">Přidruží aplikaci funkce Git nebo Mercurial úložiště.</span><span class="sxs-lookup"><span data-stu-id="8e188-124">Associates a function app with a Git or Mercurial repository.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8e188-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8e188-125">Next steps</span></span>

<span data-ttu-id="8e188-126">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8e188-126">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="8e188-127">Další ukázky skriptu rozhraní příkazového řádku funkce Azure lze nalézt v hello [dokumentace Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="8e188-127">Additional Azure Functions CLI script samples can be found in hello [Azure Functions documentation](../functions-cli-samples.md).</span></span>
