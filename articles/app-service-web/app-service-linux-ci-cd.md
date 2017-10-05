---
title: "Průběžné nasazování pomocí Azure webové aplikace v systému Linux | Microsoft Docs"
description: "Jak nastavit průběžné nasazování ve webové aplikaci Azure v systému Linux."
keywords: "služby Azure app service, linux, operačních systémů, acr"
services: app-service
documentationcenter: 
author: ahmedelnably
manager: erikre
editor: 
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: aelnably;wesmc
ms.openlocfilehash: f8f7d51003f8a55b7f51e8cc2cea838e8e5a6196
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="continuous-deployment-with-azure-web-app-on-linux"></a><span data-ttu-id="2499c-104">Průběžné nasazování pomocí webové aplikace Azure v systému Linux</span><span class="sxs-lookup"><span data-stu-id="2499c-104">Continuous deployment with Azure Web App on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

<span data-ttu-id="2499c-105">V tomto kurzu nakonfigurujete průběžné nasazování pro bitovou kopii vlastní kontejner ze spravované [registru kontejner Azure](https://azure.microsoft.com/en-us/services/container-registry/) úložiště nebo [úložiště Docker Hub](https://hub.docker.com).</span><span class="sxs-lookup"><span data-stu-id="2499c-105">In this tutorial, you configure continuous deployment for a custom container image from Managed [Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry/) repositories or [Docker Hub](https://hub.docker.com).</span></span>

## <a name="step-1---sign-in-to-azure"></a><span data-ttu-id="2499c-106">Krok 1 – znak v Azure</span><span class="sxs-lookup"><span data-stu-id="2499c-106">Step 1 - Sign in to Azure</span></span>

<span data-ttu-id="2499c-107">Přihlaste se k portálu Azure na http://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="2499c-107">Sign in to the Azure portal at http://portal.azure.com</span></span>

## <a name="step-2---enable-container-continuous-deployment-feature"></a><span data-ttu-id="2499c-108">Krok 2 – povolení kontejneru průběžné nasazování funkce</span><span class="sxs-lookup"><span data-stu-id="2499c-108">Step 2 - Enable container continuous deployment feature</span></span>

<span data-ttu-id="2499c-109">Můžete povolit pomocí funkce průběžné nasazování [rozhraní příkazového řádku Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) a spuštěním následujícího příkazu</span><span class="sxs-lookup"><span data-stu-id="2499c-109">You can enable the continuous deployment feature using [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) and executing the following command</span></span>

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
``` 

<span data-ttu-id="2499c-110">V  **[portál Azure](https://portal.azure.com/)**, klikněte **služby App Service** možnost na levé straně stránky.</span><span class="sxs-lookup"><span data-stu-id="2499c-110">In the **[Azure portal](https://portal.azure.com/)**, click the **App Service** option on the left of the page.</span></span>

<span data-ttu-id="2499c-111">Klikněte na název vaší aplikace, který chcete provést konfiguraci úložiště Docker Hub průběžné nasazování pro.</span><span class="sxs-lookup"><span data-stu-id="2499c-111">Click on the name of your app that you want to configure Docker Hub continuous deployment for.</span></span>

<span data-ttu-id="2499c-112">V **nastavení aplikace**, přidat aplikaci názvem `DOCKER_ENABLE_CI` s hodnotou `true`.</span><span class="sxs-lookup"><span data-stu-id="2499c-112">In the **App settings**, add an app setting called `DOCKER_ENABLE_CI` with the value `true`.</span></span>

![Vložit obrázek nastavení aplikace](./media/app-service-webapp-service-linux-ci-cd/step2.png)

## <a name="step-3---prepare-webhook-url"></a><span data-ttu-id="2499c-114">Krok 3 – Příprava adresa URL Webhooku</span><span class="sxs-lookup"><span data-stu-id="2499c-114">Step 3 - Prepare Webhook URL</span></span>

<span data-ttu-id="2499c-115">Můžete získat pomocí adresy URL Webhooku [rozhraní příkazového řádku Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) a spuštěním následujícího příkazu</span><span class="sxs-lookup"><span data-stu-id="2499c-115">You can obtain the Webhook URL using [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) and executing the following command</span></span>

```azurecli-interactive
az webapp deployment container -n sname1 -g rgname -e true --show-cd-url
``` 

<span data-ttu-id="2499c-116">Pro adresu URL Webhooku, musíte mít následující koncový bod: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.</span><span class="sxs-lookup"><span data-stu-id="2499c-116">For the Webhook URL, you need to have the following endpoint: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.</span></span>

<span data-ttu-id="2499c-117">Můžete získat vaše `publishingusername` a `publishingpwd` stažením webové aplikace profil publikování se pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2499c-117">You can obtain your `publishingusername` and `publishingpwd` by downloading the web app publish profile using the Azure portal.</span></span>

![Vložit obrázek přidání webhooku 2](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="step-4---add-a-web-hook"></a><span data-ttu-id="2499c-119">Krok 4 – přidání webové háku</span><span class="sxs-lookup"><span data-stu-id="2499c-119">Step 4 - Add a web hook</span></span>

### <a name="azure-container-registry"></a><span data-ttu-id="2499c-120">Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="2499c-120">Azure Container Registry</span></span>

<span data-ttu-id="2499c-121">V okně portálu registru, klikněte na tlačítko **Webhooky**, kliknutím na vytvořit nový webhooku **přidat**.</span><span class="sxs-lookup"><span data-stu-id="2499c-121">In your registry portal blade, click **Webhooks**, create a new webhook by clicking **Add**.</span></span> <span data-ttu-id="2499c-122">V **vytvoření webhooku** okno, zadejte název vaší webhooku.</span><span class="sxs-lookup"><span data-stu-id="2499c-122">In the **Create webhook** blade, give your webhook a name.</span></span> <span data-ttu-id="2499c-123">Pro identifikátor URI pro Webhook, budete muset zadat adresu URL získané z **krok 3**</span><span class="sxs-lookup"><span data-stu-id="2499c-123">For the Webhook URI, you need to provide the URL obtained from **Step 3**</span></span>

<span data-ttu-id="2499c-124">Zajistěte, aby definujte rozsah jako úložiště obsahující bitové kopie kontejneru.</span><span class="sxs-lookup"><span data-stu-id="2499c-124">Make sure that you define the scope as the repo that contains your container image.</span></span>

![Vložit obrázek webhooku](./media/app-service-webapp-service-linux-ci-cd/step3ACRWebhook-1.png)

<span data-ttu-id="2499c-126">Při aktualizaci získá bitovou kopii, získat webové aplikace automaticky aktualizován s novou bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="2499c-126">When the image gets updated, the web app get updated automatically with the new image.</span></span>

### <a name="docker-hub"></a><span data-ttu-id="2499c-127">Úložiště docker Hub</span><span class="sxs-lookup"><span data-stu-id="2499c-127">Docker Hub</span></span>

<span data-ttu-id="2499c-128">Na stránce úložiště Docker Hub klikněte na tlačítko **Webhooky**, pak **vytvoření WEBHOOKU A**.</span><span class="sxs-lookup"><span data-stu-id="2499c-128">In your Docker Hub page, click **Webhooks**, then **CREATE A WEBHOOK**.</span></span>

![Vložit obrázek přidání webhooku 1](./media/app-service-webapp-service-linux-ci-cd/step3-1.png)

<span data-ttu-id="2499c-130">Pro adresu URL Webhooku, budete muset zadat adresu URL získané z **krok 3**</span><span class="sxs-lookup"><span data-stu-id="2499c-130">For the Webhook URL, you need to provide the URL obtained from **Step 3**</span></span>

![Vložit obrázek přidání webhooku 2](./media/app-service-webapp-service-linux-ci-cd/step3-2.png)

<span data-ttu-id="2499c-132">Při aktualizaci získá bitovou kopii, získat webové aplikace automaticky aktualizován s novou bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="2499c-132">When the image gets updated, the web app get updated automatically with the new image.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2499c-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2499c-133">Next steps</span></span>
* [<span data-ttu-id="2499c-134">Co je Azure webové aplikace v systému Linux?</span><span class="sxs-lookup"><span data-stu-id="2499c-134">What is Azure Web App on Linux?</span></span>](./app-service-linux-intro.md)
* [<span data-ttu-id="2499c-135">Kontejner Azure registru</span><span class="sxs-lookup"><span data-stu-id="2499c-135">Azure Container Registry</span></span>](https://azure.microsoft.com/en-us/services/container-registry/)
* [<span data-ttu-id="2499c-136">Pomocí PM2 konfigurace pro Node.js v Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="2499c-136">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="2499c-137">Pomocí .NET Core v Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="2499c-137">Using .NET Core in Azure Web App on Linux</span></span>](app-service-linux-using-dotnetcore.md)
* [<span data-ttu-id="2499c-138">Použití Ruby v Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="2499c-138">Using Ruby in Azure Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)
* [<span data-ttu-id="2499c-139">Jak používat vlastní image Docker pro webové aplikace Azure v systému Linux</span><span class="sxs-lookup"><span data-stu-id="2499c-139">How to use a custom Docker image for Azure Web App on Linux</span></span>](./app-service-linux-using-custom-docker-image.md)
* [<span data-ttu-id="2499c-140">Webové aplikace Azure App Service v systému Linux – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="2499c-140">Azure App Service Web App on Linux FAQ</span></span>](./app-service-linux-faq.md) 
* [<span data-ttu-id="2499c-141">Správa webové aplikace v systému Linux pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="2499c-141">Manage Web App on Linux using Azure CLI 2.0</span></span>](./app-service-linux-cli.md)



