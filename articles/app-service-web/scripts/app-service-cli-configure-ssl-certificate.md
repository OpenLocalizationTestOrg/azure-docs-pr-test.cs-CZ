---
title: "Ukázka skriptu Azure CLI - vazby SSL vlastní certifikát do webové aplikace | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - vazby vlastní certifikát SSL pro webovou aplikaci"
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
ms.openlocfilehash: d4fab3fb2c297bf5f498b63bee46692febb9180b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="bind-a-custom-ssl-certificate-to-a-web-app"></a><span data-ttu-id="11e28-103">Vlastní certifikát SSL vazbu do webové aplikace</span><span class="sxs-lookup"><span data-stu-id="11e28-103">Bind a custom SSL certificate to a web app</span></span>

<span data-ttu-id="11e28-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a potom vytvoří vazbu certifikátu SSL vlastního názvu domény.</span><span class="sxs-lookup"><span data-stu-id="11e28-104">This sample script creates a web app in App Service with its related resources, then binds the SSL certificate of a custom domain name to it.</span></span> <span data-ttu-id="11e28-105">Pro tuto ukázku budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="11e28-105">For this sample, you will need:</span></span>

* <span data-ttu-id="11e28-106">Přístup na stránku konfigurace DNS doménového registrátora.</span><span class="sxs-lookup"><span data-stu-id="11e28-106">Access to your domain registrar's DNS configuration page.</span></span>
* <span data-ttu-id="11e28-107">Platné. Soubor PFX a heslo pro certifikát SSL chcete nahrát a jejich vazby.</span><span class="sxs-lookup"><span data-stu-id="11e28-107">A valid .PFX file and its password for the SSL certificate you want to upload and bind.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="11e28-108">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="11e28-108">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="11e28-109">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="11e28-109">Run `az --version` to find the version.</span></span> <span data-ttu-id="11e28-110">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="11e28-110">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="sample-script"></a><span data-ttu-id="11e28-111">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="11e28-111">Sample script</span></span>

<span data-ttu-id="11e28-112">[!code-azurecli-interactive[hlavní](../../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "vlastní certifikát SSL vazbu do webové aplikace")]</span><span class="sxs-lookup"><span data-stu-id="11e28-112">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="11e28-113">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="11e28-113">Script explanation</span></span>

<span data-ttu-id="11e28-114">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="11e28-114">This script uses the following commands.</span></span> <span data-ttu-id="11e28-115">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="11e28-115">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="11e28-116">Příkaz</span><span class="sxs-lookup"><span data-stu-id="11e28-116">Command</span></span> | <span data-ttu-id="11e28-117">Poznámky</span><span class="sxs-lookup"><span data-stu-id="11e28-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="11e28-118">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="11e28-118">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="11e28-119">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="11e28-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="11e28-120">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="11e28-120">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="11e28-121">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="11e28-121">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="11e28-122">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="11e28-122">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="11e28-123">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="11e28-123">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="11e28-124">Přidejte název az webapp konfigurace hostitele</span><span class="sxs-lookup"><span data-stu-id="11e28-124">az webapp config hostname add</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/hostname#add) | <span data-ttu-id="11e28-125">Mapuje vlastní doménu do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="11e28-125">Maps a custom domain to a web app.</span></span> |
| [<span data-ttu-id="11e28-126">AZ webapp konfigurace ssl nahrávání</span><span class="sxs-lookup"><span data-stu-id="11e28-126">az webapp config ssl upload</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/ssl#upload) | <span data-ttu-id="11e28-127">Odešle certifikát SSL pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="11e28-127">Uploads an SSL certificate to a web app.</span></span> |
| [<span data-ttu-id="11e28-128">AZ webapp konfigurace protokolu ssl vazby</span><span class="sxs-lookup"><span data-stu-id="11e28-128">az webapp config ssl bind</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/ssl#bind) | <span data-ttu-id="11e28-129">Váže nahraný certifikát SSL pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="11e28-129">Binds an uploaded SSL certificate to a web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="11e28-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="11e28-130">Next steps</span></span>

<span data-ttu-id="11e28-131">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="11e28-131">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="11e28-132">Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="11e28-132">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
