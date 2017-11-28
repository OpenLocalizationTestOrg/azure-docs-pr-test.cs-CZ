---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - namapovat vlastní doménu tooa webové aplikace | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - mapy vlastní domény tooa webové aplikace"
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
ms.openlocfilehash: 49d6be092e438a63c0a43e3207080ca4cd5ff3fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="map-a-custom-domain-tooa-web-app"></a><span data-ttu-id="bf90e-103">Mapa vlastní domény tooa webové aplikace</span><span class="sxs-lookup"><span data-stu-id="bf90e-103">Map a custom domain tooa web app</span></span>

<span data-ttu-id="bf90e-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a potom mapuje `www.<yourdomain>` tooit.</span><span class="sxs-lookup"><span data-stu-id="bf90e-104">This sample script creates a web app in App Service with its related resources, and then maps `www.<yourdomain>` tooit.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="bf90e-105">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="bf90e-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="bf90e-106">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="bf90e-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="bf90e-107">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="bf90e-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="bf90e-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="bf90e-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Map a custom domain tooa web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="bf90e-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="bf90e-109">Script explanation</span></span>

<span data-ttu-id="bf90e-110">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="bf90e-110">This script uses hello following commands.</span></span> <span data-ttu-id="bf90e-111">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="bf90e-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="bf90e-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="bf90e-112">Command</span></span> | <span data-ttu-id="bf90e-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="bf90e-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="bf90e-114">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="bf90e-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="bf90e-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="bf90e-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="bf90e-116">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="bf90e-116">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="bf90e-117">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="bf90e-117">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="bf90e-118">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="bf90e-118">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="bf90e-119">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="bf90e-119">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="bf90e-120">Přidejte název az webapp konfigurace hostitele</span><span class="sxs-lookup"><span data-stu-id="bf90e-120">az webapp config hostname add</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/hostname#add) | <span data-ttu-id="bf90e-121">Mapuje vlastní domény tooa webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="bf90e-121">Maps a custom domain tooa web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="bf90e-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bf90e-122">Next steps</span></span>

<span data-ttu-id="bf90e-123">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bf90e-123">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="bf90e-124">Další ukázky skriptu rozhraní příkazového řádku služby aplikace naleznete v hello [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="bf90e-124">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
