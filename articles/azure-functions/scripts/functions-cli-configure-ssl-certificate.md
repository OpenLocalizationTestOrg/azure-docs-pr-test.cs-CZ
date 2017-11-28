---
title: "Ukázka skriptu Azure CLI - vazby SSL vlastní certifikát do aplikaci funkce | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - vazby vlastní certifikát SSL pro funkce aplikace v Azure"
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
ms.openlocfilehash: ddabb701d7d5615232d1f6163aa6fb166efe5cb0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="bind-a-custom-ssl-certificate-to-a-function-app"></a><span data-ttu-id="7510a-103">Vlastní certifikát SSL vazbu na aplikaci – funkce</span><span class="sxs-lookup"><span data-stu-id="7510a-103">Bind a custom SSL certificate to a function app</span></span>

<span data-ttu-id="7510a-104">Tento ukázkový skript vytvoří aplikaci funkce ve službě App Service se jeho souvisejících prostředků a potom vytvoří vazbu certifikátu SSL vlastního názvu domény.</span><span class="sxs-lookup"><span data-stu-id="7510a-104">This sample script creates a function app in App Service with its related resources, then binds the SSL certificate of a custom domain name to it.</span></span> <span data-ttu-id="7510a-105">Tato ukázka je třeba:</span><span class="sxs-lookup"><span data-stu-id="7510a-105">For this sample, you need:</span></span>

* <span data-ttu-id="7510a-106">Přístup na stránku konfigurace DNS doménového registrátora.</span><span class="sxs-lookup"><span data-stu-id="7510a-106">Access to your domain registrar's DNS configuration page.</span></span>
* <span data-ttu-id="7510a-107">Platné. Soubor PFX a heslo pro certifikát SSL chcete nahrát a jejich vazby.</span><span class="sxs-lookup"><span data-stu-id="7510a-107">A valid .PFX file and its password for the SSL certificate you want to upload and bind.</span></span>

<span data-ttu-id="7510a-108">Funkce aplikace k vytvoření vazby certifikátu SSL, musí být vytvořený v plán služby App Service a nejsou v plánu spotřeby.</span><span class="sxs-lookup"><span data-stu-id="7510a-108">To bind an SSL certificate, your function app must be created in an App Service plan and not in a consumption plan.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="7510a-109">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="7510a-109">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="7510a-110">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="7510a-110">Run `az --version` to find the version.</span></span> <span data-ttu-id="7510a-111">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="7510a-111">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="7510a-112">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="7510a-112">Sample script</span></span>

<span data-ttu-id="7510a-113">[!code-azurecli-interactive[hlavní](../../../cli_scripts/azure-functions/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "vlastní certifikát SSL vazbu do webové aplikace")]</span><span class="sxs-lookup"><span data-stu-id="7510a-113">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="7510a-114">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="7510a-114">Script explanation</span></span>

<span data-ttu-id="7510a-115">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="7510a-115">This script uses the following commands.</span></span> <span data-ttu-id="7510a-116">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="7510a-116">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="7510a-117">Příkaz</span><span class="sxs-lookup"><span data-stu-id="7510a-117">Command</span></span> | <span data-ttu-id="7510a-118">Poznámky</span><span class="sxs-lookup"><span data-stu-id="7510a-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="7510a-119">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="7510a-119">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="7510a-120">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="7510a-120">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="7510a-121">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="7510a-121">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="7510a-122">Vytvoří plán služby App Service potřebné k vytvoření vazby certifikátů SSL.</span><span class="sxs-lookup"><span data-stu-id="7510a-122">Creates an App Service plan required to bind SSL certificates.</span></span> |
| [<span data-ttu-id="7510a-123">Vytvoření az functionapp</span><span class="sxs-lookup"><span data-stu-id="7510a-123">az functionapp create</span></span>]() | <span data-ttu-id="7510a-124">Vytvoří aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="7510a-124">Creates a function app.</span></span> |
| [<span data-ttu-id="7510a-125">Přidat az název hostitele konfigurace webové služby App Service</span><span class="sxs-lookup"><span data-stu-id="7510a-125">az appservice web config hostname add</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#add) | <span data-ttu-id="7510a-126">Vlastní doména se mapuje na aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="7510a-126">Maps a custom domain to the function app.</span></span> |
| [<span data-ttu-id="7510a-127">AZ služby App Service web konfigurace ssl nahrávání</span><span class="sxs-lookup"><span data-stu-id="7510a-127">az appservice web config ssl upload</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/config/ssl#upload) | <span data-ttu-id="7510a-128">Certifikát SSL odešle do aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="7510a-128">Uploads an SSL certificate to a function app.</span></span> |
| [<span data-ttu-id="7510a-129">AZ služby App Service web konfigurace protokolu ssl vazby</span><span class="sxs-lookup"><span data-stu-id="7510a-129">az appservice web config ssl bind</span></span>](https://docs.microsoft.com/en-us/cli/azure/appservice/web/config/ssl#bind) | <span data-ttu-id="7510a-130">Váže nahraný certifikát SSL k aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="7510a-130">Binds an uploaded SSL certificate to a function app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="7510a-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7510a-131">Next steps</span></span>

<span data-ttu-id="7510a-132">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7510a-132">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="7510a-133">Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service]().</span><span class="sxs-lookup"><span data-stu-id="7510a-133">Additional App Service CLI script samples can be found in the [Azure App Service documentation]().</span></span>
