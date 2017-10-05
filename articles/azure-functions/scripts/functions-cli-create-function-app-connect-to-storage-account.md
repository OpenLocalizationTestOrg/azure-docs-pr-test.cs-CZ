---
title: "Vytvoření funkce Azure, která se připojuje k Azure Storage | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření funkce Azure, která se připojuje k Azure Storage"
services: functions
documentationcenter: functions
author: rachelappel
manager: erikre
editor: 
tags: functions
ms.assetid: 
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 04/20/2017
ms.author: rachelap
ms.custom: mvc
ms.openlocfilehash: 36dbc2c181c9991a27163e3194800f63c6c0e01e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="integrate-function-app-into-azure-storage-account"></a><span data-ttu-id="9e829-103">Integrace funkce aplikace do účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="9e829-103">Integrate Function App into Azure Storage Account</span></span>

<span data-ttu-id="9e829-104">Tento ukázkový skript vytvoří aplikaci funkce a účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="9e829-104">This sample script creates a Function App and Storage Account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="9e829-105">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="9e829-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="9e829-106">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="9e829-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="9e829-107">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9e829-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="9e829-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="9e829-108">Sample script</span></span>

<span data-ttu-id="9e829-109">Tato ukázka vytvoří aplikaci funkce Azure a přidá připojovací řetězec úložiště do nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e829-109">This sample creates an Azure Function app and adds the storage connection string to an app setting.</span></span>

<span data-ttu-id="9e829-110">[!code-azurecli-interactive[hlavní](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "integrovat funkce aplikace do účtu úložiště Azure")]</span><span class="sxs-lookup"><span data-stu-id="9e829-110">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "Integrate Function App into Azure Storage Account")]</span></span>


## <a name="clean-up-deployment"></a><span data-ttu-id="9e829-111">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="9e829-111">Clean up deployment</span></span>

<span data-ttu-id="9e829-112">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků, aplikační služby a všechny související prostředky:</span><span class="sxs-lookup"><span data-stu-id="9e829-112">After the script sample has been run, the following command can be used to remove the resource group, App Service app, and all related resources:</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="9e829-113">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="9e829-113">Script explanation</span></span>

<span data-ttu-id="9e829-114">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="9e829-114">This script uses the following commands.</span></span> <span data-ttu-id="9e829-115">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="9e829-115">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="9e829-116">Příkaz</span><span class="sxs-lookup"><span data-stu-id="9e829-116">Command</span></span> | <span data-ttu-id="9e829-117">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9e829-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="9e829-118">AZ přihlášení</span><span class="sxs-lookup"><span data-stu-id="9e829-118">az login</span></span>](https://docs.microsoft.com/cli/azure/#login) | <span data-ttu-id="9e829-119">Přihlášení k Azure.</span><span class="sxs-lookup"><span data-stu-id="9e829-119">Login to Azure.</span></span> |
| [<span data-ttu-id="9e829-120">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="9e829-120">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="9e829-121">Vytvořte skupinu prostředků s umístěním</span><span class="sxs-lookup"><span data-stu-id="9e829-121">Create a resource group with location</span></span> |
| [<span data-ttu-id="9e829-122">Vytvořit účet úložiště az</span><span class="sxs-lookup"><span data-stu-id="9e829-122">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account) | <span data-ttu-id="9e829-123">vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="9e829-123">Create a storage account</span></span> |
| [<span data-ttu-id="9e829-124">Vytvoření az functionapp</span><span class="sxs-lookup"><span data-stu-id="9e829-124">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/functionapp#create) | <span data-ttu-id="9e829-125">Vytvořit novou aplikaci funkce</span><span class="sxs-lookup"><span data-stu-id="9e829-125">Create a new function app</span></span> |
| [<span data-ttu-id="9e829-126">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="9e829-126">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="9e829-127">Vyčištění</span><span class="sxs-lookup"><span data-stu-id="9e829-127">Clean up</span></span> |

## <a name="next-steps"></a><span data-ttu-id="9e829-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9e829-128">Next steps</span></span>

<span data-ttu-id="9e829-129">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9e829-129">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="9e829-130">Další ukázky skriptu rozhraní příkazového řádku funkce Azure lze nalézt v [dokumentace Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="9e829-130">Additional Azure Functions CLI script samples can be found in the [Azure Functions documentation](../functions-cli-samples.md).</span></span>
