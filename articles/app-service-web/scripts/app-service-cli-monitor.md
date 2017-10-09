---
title: "Ukázka skriptu rozhraní příkazového řádku - aaaAzure monitorování webové aplikace s protokoly webového serveru | Microsoft Docs"
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
ms.openlocfilehash: 012b800c03af77387bed3d098fae3c96d82aee29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-web-app-with-web-server-logs"></a><span data-ttu-id="217e2-103">Monitorování webové aplikace s protokoly webového serveru</span><span class="sxs-lookup"><span data-stu-id="217e2-103">Monitor a web app with web server logs</span></span>

<span data-ttu-id="217e2-104">V tomto scénáři vytvoříte skupinu prostředků, plán služby app service, webové aplikace a konfiguraci protokolů hello webové aplikace tooenable webového serveru.</span><span class="sxs-lookup"><span data-stu-id="217e2-104">In this scenario you will create a resource group, app service plan, web app and configure hello web app tooenable web server logs.</span></span> <span data-ttu-id="217e2-105">Pak si stáhnete hello souborů protokolu ke kontrole.</span><span class="sxs-lookup"><span data-stu-id="217e2-105">You will then download hello log files for review.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="217e2-106">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="217e2-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="217e2-107">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="217e2-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="217e2-108">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="217e2-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="217e2-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="217e2-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/monitor-with-logs/monitor-with-logs.sh "Monitor Logs")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="217e2-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="217e2-110">Script explanation</span></span>

<span data-ttu-id="217e2-111">Tento skript používá hello následující příkazy toocreate skupinu prostředků, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="217e2-111">This script uses hello following commands toocreate a resource group, web app, and all related resources.</span></span> <span data-ttu-id="217e2-112">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="217e2-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="217e2-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="217e2-113">Command</span></span> | <span data-ttu-id="217e2-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="217e2-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="217e2-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="217e2-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="217e2-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="217e2-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="217e2-117">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="217e2-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="217e2-118">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="217e2-118">Creates an App Service plan.</span></span> <span data-ttu-id="217e2-119">Toto je jako serverové farmy pro Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="217e2-119">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="217e2-120">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="217e2-120">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="217e2-121">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="217e2-121">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="217e2-122">Konfigurace protokolu az webapp</span><span class="sxs-lookup"><span data-stu-id="217e2-122">az webapp log config</span></span>](https://docs.microsoft.com/cli/azure/webapp/log#config) | <span data-ttu-id="217e2-123">Nakonfiguruje protokoly, které se uchová webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="217e2-123">Configures which logs an Azure web app will persist.</span></span> |
| [<span data-ttu-id="217e2-124">AZ webapp protokolu stahování</span><span class="sxs-lookup"><span data-stu-id="217e2-124">az webapp log download</span></span>](https://docs.microsoft.com/cli/azure/webapp/log#download) | <span data-ttu-id="217e2-125">Hello stahování protokolů z hello tooyour aplikace Azure web pro místní počítač.</span><span class="sxs-lookup"><span data-stu-id="217e2-125">Downloads hello logs of hello an Azure web app tooyour local machine.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="217e2-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="217e2-126">Next steps</span></span>

<span data-ttu-id="217e2-127">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="217e2-127">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="217e2-128">Další ukázky skriptu rozhraní příkazového řádku služby aplikace naleznete v hello [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="217e2-128">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
