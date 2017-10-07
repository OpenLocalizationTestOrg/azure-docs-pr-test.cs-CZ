---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - Bind vlastní tooa webové aplikace certifikát SSL | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - vazby vlastní tooa webové aplikace certifikát SSL"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 2fec2db84a2007fa6b005776c84d4f8cba392b46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="bind-a-custom-ssl-certificate-tooa-web-app"></a><span data-ttu-id="9a053-103">Vytvoření vazby vlastní tooa webové aplikace certifikát SSL</span><span class="sxs-lookup"><span data-stu-id="9a053-103">Bind a custom SSL certificate tooa web app</span></span>

<span data-ttu-id="9a053-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a potom vytvoří vazbu certifikátu SSL hello tooit název vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="9a053-104">This sample script creates a web app in App Service with its related resources, then binds hello SSL certificate of a custom domain name tooit.</span></span> <span data-ttu-id="9a053-105">Pro tuto ukázku budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="9a053-105">For this sample, you will need:</span></span>

* <span data-ttu-id="9a053-106">Přístup k stránku konfigurace tooyour doménového registrátora DNS.</span><span class="sxs-lookup"><span data-stu-id="9a053-106">Access tooyour domain registrar's DNS configuration page.</span></span>
* <span data-ttu-id="9a053-107">Platné. Soubor PFX a heslo pro hello SSL certifikát má tooupload a jejich vazby.</span><span class="sxs-lookup"><span data-stu-id="9a053-107">A valid .PFX file and its password for hello SSL certificate you want tooupload and bind.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="9a053-108">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="9a053-108">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="9a053-109">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="9a053-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="9a053-110">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9a053-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="sample-script"></a><span data-ttu-id="9a053-111">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="9a053-111">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate tooa web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="9a053-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="9a053-112">Script explanation</span></span>

<span data-ttu-id="9a053-113">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="9a053-113">This script uses hello following commands.</span></span> <span data-ttu-id="9a053-114">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="9a053-114">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="9a053-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="9a053-115">Command</span></span> | <span data-ttu-id="9a053-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9a053-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="9a053-117">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="9a053-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="9a053-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="9a053-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="9a053-119">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="9a053-119">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="9a053-120">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="9a053-120">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="9a053-121">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="9a053-121">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="9a053-122">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="9a053-122">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="9a053-123">Přidejte název az webapp konfigurace hostitele</span><span class="sxs-lookup"><span data-stu-id="9a053-123">az webapp config hostname add</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/hostname#add) | <span data-ttu-id="9a053-124">Mapuje vlastní domény tooa webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9a053-124">Maps a custom domain tooa web app.</span></span> |
| [<span data-ttu-id="9a053-125">AZ webapp konfigurace ssl nahrávání</span><span class="sxs-lookup"><span data-stu-id="9a053-125">az webapp config ssl upload</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/ssl#upload) | <span data-ttu-id="9a053-126">Ukládání webové aplikace tooa certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="9a053-126">Uploads an SSL certificate tooa web app.</span></span> |
| [<span data-ttu-id="9a053-127">AZ webapp konfigurace protokolu ssl vazby</span><span class="sxs-lookup"><span data-stu-id="9a053-127">az webapp config ssl bind</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/ssl#bind) | <span data-ttu-id="9a053-128">Vytvoří vazbu nahrané tooa webové aplikace certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="9a053-128">Binds an uploaded SSL certificate tooa web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="9a053-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9a053-129">Next steps</span></span>

<span data-ttu-id="9a053-130">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9a053-130">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="9a053-131">Další ukázky skriptu rozhraní příkazového řádku služby aplikace naleznete v hello [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="9a053-131">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
