---
title: "aaaCreate funkce aplikace a nasazení kódu funkce z Visual Studio Team Services | Microsoft Docs"
description: "Vytvoření funkce aplikace a nasazení funkce kód z Visual Studio Team Services"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/28/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: 774bee73025cc9ac46f8b2a6c10edbfa3c2d353b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-app-service"></a><span data-ttu-id="6f8a8-103">Vytvoření služby aplikace</span><span class="sxs-lookup"><span data-stu-id="6f8a8-103">Create an App Service</span></span>

<span data-ttu-id="6f8a8-104">V tomto scénáři se dozvíte, jak funkce aplikace pomocí toocreate hello [plánu spotřeby](../functions-scale.md#consumption-plan) s jejími související prostředky a průběžně nasadí funkce kódu z Visual Studio Team Services (VSTS) úložiště.</span><span class="sxs-lookup"><span data-stu-id="6f8a8-104">In this scenario you will learn how toocreate a function app using hello [consumption plan](../functions-scale.md#consumption-plan) with its related resources, and continuously deploys your function code from a Visual Studio Team Services (VSTS) repository.</span></span> <span data-ttu-id="6f8a8-105">V této ukázce budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="6f8a8-105">In this sample, you will need:</span></span>

* <span data-ttu-id="6f8a8-106">Služby VSTS úložiště s kódem funkce, které mají oprávnění pro správu.</span><span class="sxs-lookup"><span data-stu-id="6f8a8-106">A VSTS repository with functions code, that you have administrative permissions for.</span></span>
* <span data-ttu-id="6f8a8-107">A [Personal Access Token (Jan)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) pro váš účet GitHub.</span><span class="sxs-lookup"><span data-stu-id="6f8a8-107">A [Personal Access Token (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) for your GitHub account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="6f8a8-108">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="6f8a8-108">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="6f8a8-109">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="6f8a8-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="6f8a8-110">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="6f8a8-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="6f8a8-111">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="6f8a8-111">Sample script</span></span>

<span data-ttu-id="6f8a8-112">Tato ukázka funkce Azure aplikace vytvoří a nasadí funkce kód z Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="6f8a8-112">This sample creates an Azure Function app and deploys function code from Visual Studio Team Services.</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="6f8a8-113">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="6f8a8-113">Script explanation</span></span>

<span data-ttu-id="6f8a8-114">Tento skript používá hello následující příkazy toocreate skupinu prostředků, webové aplikace, documentdb a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="6f8a8-114">This script uses hello following commands toocreate a resource group, web app, documentdb and all related resources.</span></span> <span data-ttu-id="6f8a8-115">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="6f8a8-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="6f8a8-116">Příkaz</span><span class="sxs-lookup"><span data-stu-id="6f8a8-116">Command</span></span> | <span data-ttu-id="6f8a8-117">Poznámky</span><span class="sxs-lookup"><span data-stu-id="6f8a8-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="6f8a8-118">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="6f8a8-118">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="6f8a8-119">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="6f8a8-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="6f8a8-120">Vytvořit účet úložiště az</span><span class="sxs-lookup"><span data-stu-id="6f8a8-120">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="6f8a8-121">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="6f8a8-121">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="6f8a8-122">Vytvoření az functionapp</span><span class="sxs-lookup"><span data-stu-id="6f8a8-122">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/appservice/web#delete) |
| [<span data-ttu-id="6f8a8-123">Konfigurace správy zdrojového kódu webové služby App Service az</span><span class="sxs-lookup"><span data-stu-id="6f8a8-123">az appservice web source-control config</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | <span data-ttu-id="6f8a8-124">Přidruží aplikaci funkce Git nebo Mercurial úložiště.</span><span class="sxs-lookup"><span data-stu-id="6f8a8-124">Associates a function app with a Git or Mercurial repository.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="6f8a8-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6f8a8-125">Next steps</span></span>

<span data-ttu-id="6f8a8-126">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6f8a8-126">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="6f8a8-127">Další ukázky skriptu rozhraní příkazového řádku funkce Azure lze nalézt v hello [dokumentace Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="6f8a8-127">Additional Azure Functions CLI script samples can be found in hello [Azure Functions documentation](../functions-cli-samples.md).</span></span>
