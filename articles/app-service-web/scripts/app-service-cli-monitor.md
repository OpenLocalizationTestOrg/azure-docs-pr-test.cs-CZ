---
title: "Ukázka skriptu Azure CLI – monitorování webové aplikace s protokoly webového serveru | Microsoft Docs"
description: "Ukázka skriptu Azure CLI – monitorování webové aplikace s protokoly webového serveru"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0887656f-611c-4627-8247-b5cded7cef60
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: df4ca5b1270ada849e231ad9608a5b1d2edda8be
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-a-web-app-with-web-server-logs"></a><span data-ttu-id="0094d-103">Monitorování webové aplikace s protokoly webového serveru</span><span class="sxs-lookup"><span data-stu-id="0094d-103">Monitor a web app with web server logs</span></span>

<span data-ttu-id="0094d-104">V tomto scénáři vytvoříte skupinu prostředků, plán služby app service, webové aplikace a konfigurace webové aplikace k povolení protokolů webového serveru.</span><span class="sxs-lookup"><span data-stu-id="0094d-104">In this scenario you will create a resource group, app service plan, web app and configure the web app to enable web server logs.</span></span> <span data-ttu-id="0094d-105">Pak si stáhnete souborů protokolu ke kontrole.</span><span class="sxs-lookup"><span data-stu-id="0094d-105">You will then download the log files for review.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="0094d-106">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="0094d-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="0094d-107">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="0094d-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="0094d-108">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="0094d-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="0094d-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="0094d-109">Sample script</span></span>

<span data-ttu-id="0094d-110">[!code-azurecli-interactive[hlavní](../../../cli_scripts/app-service/monitor-with-logs/monitor-with-logs.sh "monitorování protokolů")]</span><span class="sxs-lookup"><span data-stu-id="0094d-110">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/monitor-with-logs/monitor-with-logs.sh "Monitor Logs")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="0094d-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="0094d-111">Script explanation</span></span>

<span data-ttu-id="0094d-112">Tento skript používá následující příkazy k vytvoření skupiny prostředků, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="0094d-112">This script uses the following commands to create a resource group, web app, and all related resources.</span></span> <span data-ttu-id="0094d-113">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="0094d-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="0094d-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="0094d-114">Command</span></span> | <span data-ttu-id="0094d-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="0094d-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="0094d-116">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="0094d-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="0094d-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="0094d-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="0094d-118">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="0094d-118">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="0094d-119">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="0094d-119">Creates an App Service plan.</span></span> <span data-ttu-id="0094d-120">Toto je jako serverové farmy pro Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="0094d-120">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="0094d-121">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="0094d-121">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="0094d-122">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="0094d-122">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="0094d-123">Konfigurace protokolu az webapp</span><span class="sxs-lookup"><span data-stu-id="0094d-123">az webapp log config</span></span>](https://docs.microsoft.com/cli/azure/webapp/log#config) | <span data-ttu-id="0094d-124">Nakonfiguruje protokoly, které se uchová webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="0094d-124">Configures which logs an Azure web app will persist.</span></span> |
| [<span data-ttu-id="0094d-125">AZ webapp protokolu stahování</span><span class="sxs-lookup"><span data-stu-id="0094d-125">az webapp log download</span></span>](https://docs.microsoft.com/cli/azure/webapp/log#download) | <span data-ttu-id="0094d-126">Soubory ke stažení do protokolů webové aplikace Azure do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="0094d-126">Downloads the logs of the an Azure web app to your local machine.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="0094d-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0094d-127">Next steps</span></span>

<span data-ttu-id="0094d-128">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="0094d-128">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="0094d-129">Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="0094d-129">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
