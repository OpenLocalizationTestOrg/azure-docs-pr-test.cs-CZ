---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - Bind vlastní tooa funkce aplikace certifikát SSL | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - vazby vlastní SSL certifikát tooa funkce aplikace v Azure"
services: functions
documentationcenter: 
author: ggailey777
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 04/10/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 692dbc03583f2978131823083f1bfd257882664c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="bind-a-custom-ssl-certificate-tooa-function-app"></a><span data-ttu-id="75f18-103">Vytvoření vazby vlastní tooa funkce aplikace certifikát SSL</span><span class="sxs-lookup"><span data-stu-id="75f18-103">Bind a custom SSL certificate tooa function app</span></span>

<span data-ttu-id="75f18-104">Tento ukázkový skript vytvoří aplikaci funkce ve službě App Service se jeho souvisejících prostředků a potom vytvoří vazbu certifikátu SSL hello tooit název vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="75f18-104">This sample script creates a function app in App Service with its related resources, then binds hello SSL certificate of a custom domain name tooit.</span></span> <span data-ttu-id="75f18-105">Tato ukázka je třeba:</span><span class="sxs-lookup"><span data-stu-id="75f18-105">For this sample, you need:</span></span>

* <span data-ttu-id="75f18-106">Přístup k stránku konfigurace tooyour doménového registrátora DNS.</span><span class="sxs-lookup"><span data-stu-id="75f18-106">Access tooyour domain registrar's DNS configuration page.</span></span>
* <span data-ttu-id="75f18-107">Platné. Soubor PFX a heslo pro hello SSL certifikát má tooupload a jejich vazby.</span><span class="sxs-lookup"><span data-stu-id="75f18-107">A valid .PFX file and its password for hello SSL certificate you want tooupload and bind.</span></span>

<span data-ttu-id="75f18-108">toobind certifikát SSL, funkce aplikace musí být vytvořeny, v rámci plánu služby App Service a nejsou v plánu spotřeby.</span><span class="sxs-lookup"><span data-stu-id="75f18-108">toobind an SSL certificate, your function app must be created in an App Service plan and not in a consumption plan.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="75f18-109">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="75f18-109">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="75f18-110">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="75f18-110">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="75f18-111">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="75f18-111">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="75f18-112">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="75f18-112">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate tooa web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="75f18-113">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="75f18-113">Script explanation</span></span>

<span data-ttu-id="75f18-114">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="75f18-114">This script uses hello following commands.</span></span> <span data-ttu-id="75f18-115">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="75f18-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="75f18-116">Příkaz</span><span class="sxs-lookup"><span data-stu-id="75f18-116">Command</span></span> | <span data-ttu-id="75f18-117">Poznámky</span><span class="sxs-lookup"><span data-stu-id="75f18-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="75f18-118">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="75f18-118">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="75f18-119">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="75f18-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="75f18-120">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="75f18-120">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="75f18-121">Vytvoří toobind vyžaduje plán App Service certifikáty SSL.</span><span class="sxs-lookup"><span data-stu-id="75f18-121">Creates an App Service plan required toobind SSL certificates.</span></span> |
| [<span data-ttu-id="75f18-122">Vytvoření az functionapp</span><span class="sxs-lookup"><span data-stu-id="75f18-122">az functionapp create</span></span>]() | <span data-ttu-id="75f18-123">Vytvoří aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="75f18-123">Creates a function app.</span></span> |
| [<span data-ttu-id="75f18-124">Přidat az název hostitele konfigurace webové služby App Service</span><span class="sxs-lookup"><span data-stu-id="75f18-124">az appservice web config hostname add</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#add) | <span data-ttu-id="75f18-125">Mapuje aplikaci funkce toohello vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="75f18-125">Maps a custom domain toohello function app.</span></span> |
| [<span data-ttu-id="75f18-126">AZ služby App Service web konfigurace ssl nahrávání</span><span class="sxs-lookup"><span data-stu-id="75f18-126">az appservice web config ssl upload</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/config/ssl#upload) | <span data-ttu-id="75f18-127">Ukládání funkce aplikace tooa certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="75f18-127">Uploads an SSL certificate tooa function app.</span></span> |
| [<span data-ttu-id="75f18-128">AZ služby App Service web konfigurace protokolu ssl vazby</span><span class="sxs-lookup"><span data-stu-id="75f18-128">az appservice web config ssl bind</span></span>](https://docs.microsoft.com/en-us/cli/azure/appservice/web/config/ssl#bind) | <span data-ttu-id="75f18-129">Vytvoří vazbu nahrané tooa funkce aplikace certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="75f18-129">Binds an uploaded SSL certificate tooa function app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="75f18-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="75f18-130">Next steps</span></span>

<span data-ttu-id="75f18-131">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="75f18-131">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="75f18-132">Další ukázky skriptu rozhraní příkazového řádku služby aplikace naleznete v hello [dokumentaci služby Azure App Service]().</span><span class="sxs-lookup"><span data-stu-id="75f18-132">Additional App Service CLI script samples can be found in hello [Azure App Service documentation]().</span></span>
