---
title: "aaaContinuous nasazení s Azure webové aplikace v systému Linux | Microsoft Docs"
description: "Jak toosetup průběžné nasazování ve webové aplikaci Azure v systému Linux."
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
ms.openlocfilehash: f94d837e27605da58428f507ab2b0eb3af3297e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-deployment-with-azure-web-app-on-linux"></a><span data-ttu-id="cb3bc-104">Průběžné nasazování pomocí webové aplikace Azure v systému Linux</span><span class="sxs-lookup"><span data-stu-id="cb3bc-104">Continuous deployment with Azure Web App on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

<span data-ttu-id="cb3bc-105">V tomto kurzu nakonfigurujete průběžné nasazování pro bitovou kopii vlastní kontejner ze spravované [registru kontejner Azure](https://azure.microsoft.com/en-us/services/container-registry/) úložiště nebo [úložiště Docker Hub](https://hub.docker.com).</span><span class="sxs-lookup"><span data-stu-id="cb3bc-105">In this tutorial, you configure continuous deployment for a custom container image from Managed [Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry/) repositories or [Docker Hub](https://hub.docker.com).</span></span>

## <a name="step-1---sign-in-tooazure"></a><span data-ttu-id="cb3bc-106">Krok 1 – přihlášení tooAzure</span><span class="sxs-lookup"><span data-stu-id="cb3bc-106">Step 1 - Sign in tooAzure</span></span>

<span data-ttu-id="cb3bc-107">Přihlaste se toohello portál Azure na http://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="cb3bc-107">Sign in toohello Azure portal at http://portal.azure.com</span></span>

## <a name="step-2---enable-container-continuous-deployment-feature"></a><span data-ttu-id="cb3bc-108">Krok 2 – povolení kontejneru průběžné nasazování funkce</span><span class="sxs-lookup"><span data-stu-id="cb3bc-108">Step 2 - Enable container continuous deployment feature</span></span>

<span data-ttu-id="cb3bc-109">Můžete povolit pomocí funkce průběžné nasazování hello [rozhraní příkazového řádku Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) a provádění hello následující příkaz</span><span class="sxs-lookup"><span data-stu-id="cb3bc-109">You can enable hello continuous deployment feature using [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) and executing hello following command</span></span>

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
``` 

<span data-ttu-id="cb3bc-110">V hello  **[portál Azure](https://portal.azure.com/)**, klikněte na tlačítko hello **služby App Service** možnost na levé straně hello stránku hello.</span><span class="sxs-lookup"><span data-stu-id="cb3bc-110">In hello **[Azure portal](https://portal.azure.com/)**, click hello **App Service** option on hello left of hello page.</span></span>

<span data-ttu-id="cb3bc-111">Klikněte na název aplikace, které chcete tooconfigure úložiště Docker Hub průběžné nasazování pro hello.</span><span class="sxs-lookup"><span data-stu-id="cb3bc-111">Click on hello name of your app that you want tooconfigure Docker Hub continuous deployment for.</span></span>

<span data-ttu-id="cb3bc-112">V hello **nastavení aplikace**, přidat aplikaci názvem `DOCKER_ENABLE_CI` s hodnotou hello `true`.</span><span class="sxs-lookup"><span data-stu-id="cb3bc-112">In hello **App settings**, add an app setting called `DOCKER_ENABLE_CI` with hello value `true`.</span></span>

![Vložit obrázek nastavení aplikace](./media/app-service-webapp-service-linux-ci-cd/step2.png)

## <a name="step-3---prepare-webhook-url"></a><span data-ttu-id="cb3bc-114">Krok 3 – Příprava adresa URL Webhooku</span><span class="sxs-lookup"><span data-stu-id="cb3bc-114">Step 3 - Prepare Webhook URL</span></span>

<span data-ttu-id="cb3bc-115">Můžete získat pomocí adresy URL Webhooku hello [rozhraní příkazového řádku Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) a provádění hello následující příkaz</span><span class="sxs-lookup"><span data-stu-id="cb3bc-115">You can obtain hello Webhook URL using [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) and executing hello following command</span></span>

```azurecli-interactive
az webapp deployment container -n sname1 -g rgname -e true --show-cd-url
``` 

<span data-ttu-id="cb3bc-116">Hello URL webhooku se nenačetla, musíte toohave hello následující koncový bod: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.</span><span class="sxs-lookup"><span data-stu-id="cb3bc-116">For hello Webhook URL, you need toohave hello following endpoint: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.</span></span>

<span data-ttu-id="cb3bc-117">Můžete získat vaše `publishingusername` a `publishingpwd` stažením hello webovou aplikaci Publikovat profil pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cb3bc-117">You can obtain your `publishingusername` and `publishingpwd` by downloading hello web app publish profile using hello Azure portal.</span></span>

![Vložit obrázek přidání webhooku 2](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="step-4---add-a-web-hook"></a><span data-ttu-id="cb3bc-119">Krok 4 – přidání webové háku</span><span class="sxs-lookup"><span data-stu-id="cb3bc-119">Step 4 - Add a web hook</span></span>

### <a name="azure-container-registry"></a><span data-ttu-id="cb3bc-120">Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="cb3bc-120">Azure Container Registry</span></span>

<span data-ttu-id="cb3bc-121">V okně portálu registru, klikněte na tlačítko **Webhooky**, kliknutím na vytvořit nový webhooku **přidat**.</span><span class="sxs-lookup"><span data-stu-id="cb3bc-121">In your registry portal blade, click **Webhooks**, create a new webhook by clicking **Add**.</span></span> <span data-ttu-id="cb3bc-122">V hello **vytvoření webhooku** okno, zadejte název vaší webhooku.</span><span class="sxs-lookup"><span data-stu-id="cb3bc-122">In hello **Create webhook** blade, give your webhook a name.</span></span> <span data-ttu-id="cb3bc-123">Pro identifikátor URI Webhooku hello, potřebujete adresu URL hello tooprovide získané z **krok 3**</span><span class="sxs-lookup"><span data-stu-id="cb3bc-123">For hello Webhook URI, you need tooprovide hello URL obtained from **Step 3**</span></span>

<span data-ttu-id="cb3bc-124">Zajistěte, aby definovat hello oboru, jak je text hello úložiště obsahující bitové kopie kontejneru.</span><span class="sxs-lookup"><span data-stu-id="cb3bc-124">Make sure that you define hello scope as hello repo that contains your container image.</span></span>

![Vložit obrázek webhooku](./media/app-service-webapp-service-linux-ci-cd/step3ACRWebhook-1.png)

<span data-ttu-id="cb3bc-126">Při aktualizaci bitové kopie hello získá hello webové aplikace s novou bitovou kopii hello aktualizovat automaticky.</span><span class="sxs-lookup"><span data-stu-id="cb3bc-126">When hello image gets updated, hello web app get updated automatically with hello new image.</span></span>

### <a name="docker-hub"></a><span data-ttu-id="cb3bc-127">Úložiště docker Hub</span><span class="sxs-lookup"><span data-stu-id="cb3bc-127">Docker Hub</span></span>

<span data-ttu-id="cb3bc-128">Na stránce úložiště Docker Hub klikněte na tlačítko **Webhooky**, pak **vytvoření WEBHOOKU A**.</span><span class="sxs-lookup"><span data-stu-id="cb3bc-128">In your Docker Hub page, click **Webhooks**, then **CREATE A WEBHOOK**.</span></span>

![Vložit obrázek přidání webhooku 1](./media/app-service-webapp-service-linux-ci-cd/step3-1.png)

<span data-ttu-id="cb3bc-130">Hello URL webhooku se nenačetla, potřebujete adresu URL hello tooprovide získané z **krok 3**</span><span class="sxs-lookup"><span data-stu-id="cb3bc-130">For hello Webhook URL, you need tooprovide hello URL obtained from **Step 3**</span></span>

![Vložit obrázek přidání webhooku 2](./media/app-service-webapp-service-linux-ci-cd/step3-2.png)

<span data-ttu-id="cb3bc-132">Při aktualizaci bitové kopie hello získá hello webové aplikace s novou bitovou kopii hello aktualizovat automaticky.</span><span class="sxs-lookup"><span data-stu-id="cb3bc-132">When hello image gets updated, hello web app get updated automatically with hello new image.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cb3bc-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cb3bc-133">Next steps</span></span>
* [<span data-ttu-id="cb3bc-134">Co je Azure webové aplikace v systému Linux?</span><span class="sxs-lookup"><span data-stu-id="cb3bc-134">What is Azure Web App on Linux?</span></span>](./app-service-linux-intro.md)
* [<span data-ttu-id="cb3bc-135">Kontejner Azure registru</span><span class="sxs-lookup"><span data-stu-id="cb3bc-135">Azure Container Registry</span></span>](https://azure.microsoft.com/en-us/services/container-registry/)
* [<span data-ttu-id="cb3bc-136">Pomocí PM2 konfigurace pro Node.js v Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="cb3bc-136">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="cb3bc-137">Pomocí .NET Core v Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="cb3bc-137">Using .NET Core in Azure Web App on Linux</span></span>](app-service-linux-using-dotnetcore.md)
* [<span data-ttu-id="cb3bc-138">Použití Ruby v Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="cb3bc-138">Using Ruby in Azure Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)
* [<span data-ttu-id="cb3bc-139">Jak toouse vlastní Docker obrázků pro webové aplikace Azure v systému Linux</span><span class="sxs-lookup"><span data-stu-id="cb3bc-139">How toouse a custom Docker image for Azure Web App on Linux</span></span>](./app-service-linux-using-custom-docker-image.md)
* [<span data-ttu-id="cb3bc-140">Webové aplikace Azure App Service v systému Linux – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="cb3bc-140">Azure App Service Web App on Linux FAQ</span></span>](./app-service-linux-faq.md) 
* [<span data-ttu-id="cb3bc-141">Správa webové aplikace v systému Linux pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="cb3bc-141">Manage Web App on Linux using Azure CLI 2.0</span></span>](./app-service-linux-cli.md)



