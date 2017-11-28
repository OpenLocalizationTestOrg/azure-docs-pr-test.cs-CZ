---
title: "Přidejte název CDN do Azure App Service | Microsoft Docs"
description: "Přidejte do služby Azure App Service službu Content Delivery Network (CDN) umožňující ukládání statických souborů do mezipaměti a jejich doručování ze serverů blízko zákazníkům po celém světe."
services: app-service\web
author: syntaxc4
ms.author: cfowler
ms.date: 05/31/2017
ms.topic: tutorial
ms.service: app-service-web
manager: erikre
ms.workload: web
ms.custom: mvc
ms.openlocfilehash: 257b75d01f3904661c1a188a2d53ffcb74f48f06
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="add-a-content-delivery-network-cdn-to-an-azure-app-service"></a><span data-ttu-id="1c2e8-103">Přidání služby Content Delivery Network (CDN) do služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1c2e8-103">Add a Content Delivery Network (CDN) to an Azure App Service</span></span>

<span data-ttu-id="1c2e8-104">[Azure Content Delivery Network (CDN)](../cdn/cdn-overview.md) ukládá statický webový obsah do mezipaměti na strategicky umístěných místech, a tak poskytuje maximální propustnost pro doručování obsahu uživatelům.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-104">[Azure Content Delivery Network (CDN)](../cdn/cdn-overview.md) caches static web content at strategically placed locations to provide maximum throughput for delivering content to users.</span></span> <span data-ttu-id="1c2e8-105">CDN také snižuje zatížení serveru na webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-105">The CDN also decreases server load on your web app.</span></span> <span data-ttu-id="1c2e8-106">Tento kurz ukazuje, jak přidat Azure CDN do [webové aplikace ve službě Azure App Service](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1c2e8-106">This tutorial shows how to add Azure CDN to a [web app in Azure App Service](app-service-web-overview.md).</span></span> 

<span data-ttu-id="1c2e8-107">Tady je domovská stránka ukázkového statického webu v HTML, se kterým budete pracovat:</span><span class="sxs-lookup"><span data-stu-id="1c2e8-107">Here's the home page of the sample static HTML site that you'll work with:</span></span>

![Domovská stránka ukázkové aplikace](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page.png)

<span data-ttu-id="1c2e8-109">Získáte informace:</span><span class="sxs-lookup"><span data-stu-id="1c2e8-109">What you'll learn:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1c2e8-110">Vytvořit koncový bod CDN.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-110">Create a CDN endpoint.</span></span>
> * <span data-ttu-id="1c2e8-111">Aktualizovat prostředky uložené v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-111">Refresh cached assets.</span></span>
> * <span data-ttu-id="1c2e8-112">Spravovat verze uložené v mezipaměti pomocí řetězců dotazu.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-112">Use query strings to control cached versions.</span></span>
> * <span data-ttu-id="1c2e8-113">Použít vlastní doménu pro koncový bod CDN.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-113">Use a custom domain for the CDN endpoint.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1c2e8-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1c2e8-114">Prerequisites</span></span>

<span data-ttu-id="1c2e8-115">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="1c2e8-115">To complete this tutorial:</span></span>

- <span data-ttu-id="1c2e8-116">[Nainstalovat Git](https://git-scm.com/).</span><span class="sxs-lookup"><span data-stu-id="1c2e8-116">[Install Git](https://git-scm.com/)</span></span>
- [<span data-ttu-id="1c2e8-117">Instalace rozhraní příkazového řádku Azure 2.0</span><span class="sxs-lookup"><span data-stu-id="1c2e8-117">Install Azure CLI 2.0</span></span>](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-web-app"></a><span data-ttu-id="1c2e8-118">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="1c2e8-118">Create the web app</span></span>

<span data-ttu-id="1c2e8-119">Při vytváření webové aplikace, kterou budete pracovat, postupujte podle [statické rychlý start HTML](app-service-web-get-started-html.md) prostřednictvím **přejděte do aplikace** krok.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-119">To create the web app that you'll work with, follow the [static HTML quickstart](app-service-web-get-started-html.md) through the **Browse to the app** step.</span></span>

### <a name="have-a-custom-domain-ready"></a><span data-ttu-id="1c2e8-120">Připravení vlastní domény</span><span class="sxs-lookup"><span data-stu-id="1c2e8-120">Have a custom domain ready</span></span>

<span data-ttu-id="1c2e8-121">K dokončení kroku vlastní domény tohoto kurzu, musíte mít přístup k registru systému DNS u svého poskytovatele domény (například GoDaddy) a vlastní vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-121">To complete the custom domain step of this tutorial, you need to own a custom domain and have access to your DNS registry for your domain provider (such as GoDaddy).</span></span> <span data-ttu-id="1c2e8-122">Abyste například mohli přidat záznamy DNS pro `contoso.com` a `www.contoso.com`, musíte mít přístup ke konfiguraci nastavení DNS pro kořenovou doménu `contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-122">For example, to add DNS entries for `contoso.com` and `www.contoso.com`, you must have access to configure the DNS settings for the `contoso.com` root domain.</span></span>

<span data-ttu-id="1c2e8-123">Pokud ještě nemáte název domény, zvažte nákup domény pomocí webu Azure Portal podle postupu v [kurzu k doménám App Service](custom-dns-web-site-buydomains-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="1c2e8-123">If you don't already have a domain name, consider following the [App Service domain tutorial](custom-dns-web-site-buydomains-web-app.md) to purchase a domain using the Azure portal.</span></span> 

## <a name="log-in-to-the-azure-portal"></a><span data-ttu-id="1c2e8-124">Přihlášení k webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1c2e8-124">Log in to the Azure portal</span></span>

<span data-ttu-id="1c2e8-125">Otevřete prohlížeč a přejděte na web [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1c2e8-125">Open a browser and navigate to the [Azure portal](https://portal.azure.com).</span></span>

## <a name="create-a-cdn-profile-and-endpoint"></a><span data-ttu-id="1c2e8-126">Vytvoření koncového bodu a profilu CDN</span><span class="sxs-lookup"><span data-stu-id="1c2e8-126">Create a CDN profile and endpoint</span></span>

<span data-ttu-id="1c2e8-127">Na levém navigačním panelu vyberte **App Services** a pak vyberte aplikaci, kterou jste vytvořili v [rychlém úvodu ke statickému HTML](app-service-web-get-started-html.md).</span><span class="sxs-lookup"><span data-stu-id="1c2e8-127">In the left navigation, select **App Services**, and then select the app that you created in the [static HTML quickstart](app-service-web-get-started-html.md).</span></span>

![Výběr aplikace App Service na portálu](media/app-service-web-tutorial-content-delivery-network/portal-select-app-services.png)

<span data-ttu-id="1c2e8-129">Na stránce **App Service** v části **Nastavení** vyberte **Sítě > Konfigurovat Azure CDN pro aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-129">In the **App Service** page, in the **Settings** section, select **Networking > Configure Azure CDN for your app**.</span></span>

![Výběr CDN na portálu](media/app-service-web-tutorial-content-delivery-network/portal-select-cdn.png)

<span data-ttu-id="1c2e8-131">Na stránce **Azure Content Delivery Network** zadejte pro **Nový koncový bod** nastavení tak, jak je uvedeno v tabulce.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-131">In the **Azure Content Delivery Network** page, provide the **New endpoint** settings as specified in the table.</span></span>

![Vytvoření profilu a koncového bodu na portálu](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint.png)

| <span data-ttu-id="1c2e8-133">Nastavení</span><span class="sxs-lookup"><span data-stu-id="1c2e8-133">Setting</span></span> | <span data-ttu-id="1c2e8-134">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="1c2e8-134">Suggested value</span></span> | <span data-ttu-id="1c2e8-135">Popis</span><span class="sxs-lookup"><span data-stu-id="1c2e8-135">Description</span></span> |
| ------- | --------------- | ----------- |
| <span data-ttu-id="1c2e8-136">**Profil CDN**</span><span class="sxs-lookup"><span data-stu-id="1c2e8-136">**CDN profile**</span></span> | <span data-ttu-id="1c2e8-137">myCDNProfile</span><span class="sxs-lookup"><span data-stu-id="1c2e8-137">myCDNProfile</span></span> | <span data-ttu-id="1c2e8-138">Vyberte **vytvořit nový** k vytvoření profilu CDN.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-138">Select **Create new** to create a CDN profile.</span></span> <span data-ttu-id="1c2e8-139">Profil CDN je kolekce koncových bodů CDN se stejnou cenovou úrovní.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-139">A CDN profile is a collection of CDN endpoints with the same pricing tier.</span></span> |
| <span data-ttu-id="1c2e8-140">**Cenová úroveň**</span><span class="sxs-lookup"><span data-stu-id="1c2e8-140">**Pricing tier**</span></span> | <span data-ttu-id="1c2e8-141">Akamai Standard</span><span class="sxs-lookup"><span data-stu-id="1c2e8-141">Standard Akamai</span></span> | <span data-ttu-id="1c2e8-142">[Cenová úroveň](../cdn/cdn-overview.md#azure-cdn-features) určuje poskytovatele a dostupné funkce.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-142">The [pricing tier](../cdn/cdn-overview.md#azure-cdn-features) specifies the provider and available features.</span></span> <span data-ttu-id="1c2e8-143">V tomto kurzu používáme Standard Akamai.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-143">In this tutorial, we are using Standard Akamai.</span></span> |
| <span data-ttu-id="1c2e8-144">**Název koncového bodu CDN**</span><span class="sxs-lookup"><span data-stu-id="1c2e8-144">**CDN endpoint name**</span></span> | <span data-ttu-id="1c2e8-145">Libovolný název, který je jedinečný v doméně azureedge.net</span><span class="sxs-lookup"><span data-stu-id="1c2e8-145">Any name that is unique in the azureedge.net domain</span></span> | <span data-ttu-id="1c2e8-146">K prostředkům uloženým v mezipaměti přistupujete na doméně *\<název_koncového_bodu>.azureedge.net*.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-146">You access your cached resources at the domain *\<endpointname>.azureedge.net*.</span></span>

<span data-ttu-id="1c2e8-147">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-147">Select **Create**.</span></span>

<span data-ttu-id="1c2e8-148">Azure vytvoří profil a koncový bod.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-148">Azure creates the profile and endpoint.</span></span> <span data-ttu-id="1c2e8-149">Nový koncový bod se zobrazí na stejné stránce v seznamu **Koncové body** a až se zřídí, bude jeho stav **Spuštěno**.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-149">The new endpoint appears in the **Endpoints** list on the same page, and when it's provisioned the status is **Running**.</span></span>

![Nový koncový bod v seznamu](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint-in-list.png)

### <a name="test-the-cdn-endpoint"></a><span data-ttu-id="1c2e8-151">Testování koncového bodu CDN</span><span class="sxs-lookup"><span data-stu-id="1c2e8-151">Test the CDN endpoint</span></span>

<span data-ttu-id="1c2e8-152">Pokud jste vybrali cenovou úroveň Verizon, rozšíření koncového bodu obvykle trvá přibližně 90 minut.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-152">If you selected Verizon pricing tier, it typically takes about 90 minutes for endpoint propagation.</span></span> <span data-ttu-id="1c2e8-153">U Akamai trvá rozšíření několik minut.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-153">For Akamai, it takes a couple minutes for propagation</span></span>

<span data-ttu-id="1c2e8-154">Ukázková aplikace má soubor `index.html` a složky *css*, *img* a *js* obsahující další statické prostředky.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-154">The sample app has an `index.html` file and *css*, *img*, and *js* folders that contain other static assets.</span></span> <span data-ttu-id="1c2e8-155">Cesty k obsahu jsou v koncovém bodu CDN pro všechny tyto soubory stejné.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-155">The content paths for all of these files are the same at the CDN endpoint.</span></span> <span data-ttu-id="1c2e8-156">Například obě následující adresy URL slouží k přístupu k souboru *bootstrap.css* ve složce *css*:</span><span class="sxs-lookup"><span data-stu-id="1c2e8-156">For example, both of the following URLs access the *bootstrap.css* file in the *css* folder:</span></span>

```
http://<appname>.azurewebsites.net/css/bootstrap.css
```

```
http://<endpointname>.azureedge.net/css/bootstrap.css
```

<span data-ttu-id="1c2e8-157">Přejděte v prohlížeči na následující adresu URL:</span><span class="sxs-lookup"><span data-stu-id="1c2e8-157">Navigate a browser to the following URL:</span></span>

```
http://<endpointname>.azureedge.net/index.html
```

![Domovská stránka ukázkové aplikace poskytnutá z CDN](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page-cdn.png)

 <span data-ttu-id="1c2e8-159">Zobrazí na stejné stránce, který byl dříve v webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-159">You see the same page that you ran earlier in an Azure web app.</span></span> <span data-ttu-id="1c2e8-160">Azure CDN má načíst prostředky počátek webové aplikace a jejich obsluhuje z koncového bodu CDN</span><span class="sxs-lookup"><span data-stu-id="1c2e8-160">Azure CDN has retrieved the origin web app's assets and is serving them from the CDN endpoint</span></span>

<span data-ttu-id="1c2e8-161">Abyste zajistili uložení této stránky v mezipaměti v CDN, aktualizujte stránku.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-161">To ensure that this page is cached in the CDN, refresh the page.</span></span> <span data-ttu-id="1c2e8-162">Někdy jsou k uložení požadovaného obsahu do mezipaměti v CDN potřeba dva požadavky na stejný prostředek.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-162">Two requests for the same asset are sometimes required for the CDN to cache the requested content.</span></span>

<span data-ttu-id="1c2e8-163">Další informace o vytváření koncových bodů a profilů CDN najdete v tématu [Začínáme s Azure CDN](../cdn/cdn-create-new-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="1c2e8-163">For more information about creating Azure CDN profiles and endpoints, see [Getting started with Azure CDN](../cdn/cdn-create-new-endpoint.md).</span></span>

## <a name="purge-the-cdn"></a><span data-ttu-id="1c2e8-164">Vyprázdnění CDN</span><span class="sxs-lookup"><span data-stu-id="1c2e8-164">Purge the CDN</span></span>

<span data-ttu-id="1c2e8-165">CDN pravidelně aktualizuje prostředky z původní webové aplikace podle konfigurace hodnoty TTL (Time to Live).</span><span class="sxs-lookup"><span data-stu-id="1c2e8-165">The CDN periodically refreshes its resources from the origin web app based on the time-to-live (TTL) configuration.</span></span> <span data-ttu-id="1c2e8-166">Výchozí hodnota TTL je sedm dní.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-166">The default TTL is seven days.</span></span>

<span data-ttu-id="1c2e8-167">Čas od času může být nutné aktualizovat CDN před vypršením hodnoty TTL – například při nasazení aktualizovaného obsahu do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-167">At times you might need to refresh the CDN before the TTL expiration -- for example, when you deploy updated content to the web app.</span></span> <span data-ttu-id="1c2e8-168">Aktualizaci můžete aktivovat ručním vyprázdněním prostředků CDN.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-168">To trigger a refresh, you can manually purge the CDN resources.</span></span> 

<span data-ttu-id="1c2e8-169">V této části kurzu nasadíte do webové aplikace změnu a vyprázdníte CDN, abyste v CDN aktivovali aktualizaci mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-169">In this section of the tutorial, you deploy a change to the web app and purge the CDN to trigger the CDN to refresh its cache.</span></span>

### <a name="deploy-a-change-to-the-web-app"></a><span data-ttu-id="1c2e8-170">Nasazení změny do webové aplikace</span><span class="sxs-lookup"><span data-stu-id="1c2e8-170">Deploy a change to the web app</span></span>

<span data-ttu-id="1c2e8-171">Otevřete soubor `index.html` a do nadpisu H1 přidejte „– V2“, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="1c2e8-171">Open the `index.html` file and add "- V2" to the H1 heading, as shown in the following example:</span></span> 

```
<h1>Azure App Service - Sample Static HTML Site - V2</h1>
```

<span data-ttu-id="1c2e8-172">Potvrďte změnu a nasaďte ji do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-172">Commit your change and deploy it to the web app.</span></span>

```bash
git commit -am "version 2"
git push azure master
```

<span data-ttu-id="1c2e8-173">Po dokončení nasazení přejděte na adresu URL webové aplikace a uvidíte změnu.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-173">Once deployment has completed, browse to the web app URL and you see the change.</span></span>

```
http://<appname>.azurewebsites.net/index.html
```

![V2 v nadpisu ve webové aplikaci](media/app-service-web-tutorial-content-delivery-network/v2-in-web-app-title.png)

<span data-ttu-id="1c2e8-175">Přejděte na adresu URL koncového bodu CDN pro domovskou stránku – změnu neuvidíte, protože platnost verze uložené v mezipaměti v CDN ještě nevypršela.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-175">Browse to the CDN endpoint URL for the home page and you don't see the change because the cached version in the CDN hasn't expired yet.</span></span> 

```
http://<endpointname>.azureedge.net/index.html
```

![Žádné V2 v nadpisu v CDN](media/app-service-web-tutorial-content-delivery-network/no-v2-in-cdn-title.png)

### <a name="purge-the-cdn-in-the-portal"></a><span data-ttu-id="1c2e8-177">Vyprázdnění CDN na portálu</span><span class="sxs-lookup"><span data-stu-id="1c2e8-177">Purge the CDN in the portal</span></span>

<span data-ttu-id="1c2e8-178">Pokud chcete v CDN aktivovat aktualizaci verze uložené v mezipaměti, vyprázdněte CDN.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-178">To trigger the CDN to update its cached version, purge the CDN.</span></span>

<span data-ttu-id="1c2e8-179">Na levém navigačním panelu portálu vyberte **Skupiny prostředků** a pak vyberte skupinu prostředků vytvořenou pro vaši webovou aplikaci (myResourceGroup).</span><span class="sxs-lookup"><span data-stu-id="1c2e8-179">In the portal left navigation, select **Resource groups**, and then select the resource group that you created for your web app (myResourceGroup).</span></span>

![Výběr skupiny prostředků](media/app-service-web-tutorial-content-delivery-network/portal-select-group.png)

<span data-ttu-id="1c2e8-181">V seznamu prostředků vyberte koncový bod CDN.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-181">In the list of resources, select your CDN endpoint.</span></span>

![Výběr koncového bodu](media/app-service-web-tutorial-content-delivery-network/portal-select-endpoint.png)

<span data-ttu-id="1c2e8-183">V horní části stránky **Koncový bod** klikněte na **Vyprázdnit**.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-183">At the top of the **Endpoint** page, click **Purge**.</span></span>

![Výběr možnosti Vyprázdnit](media/app-service-web-tutorial-content-delivery-network/portal-select-purge.png)

<span data-ttu-id="1c2e8-185">Zadejte cesty k obsahu, který chcete vyprázdnit.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-185">Enter the content paths you wish to purge.</span></span> <span data-ttu-id="1c2e8-186">Můžete předat úplnou cestu k souboru pro vyprázdnění jednotlivých souborů, nebo část cesty pro vyprázdnění a aktualizaci veškerého obsahu ze složky.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-186">You can pass a complete file path to purge an individual file, or a path segment to purge and refresh all content in a folder.</span></span> <span data-ttu-id="1c2e8-187">Protože jste změnili soubor `index.html`, ujistěte se, že zadáte jeho cestu.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-187">Since you changed `index.html`, make sure that is one of the paths.</span></span>

<span data-ttu-id="1c2e8-188">V dolní části stránky klikněte na **Vyprázdnit**.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-188">At the bottom of the page, select **Purge**.</span></span>

![Stránka Vyprázdnit](media/app-service-web-tutorial-content-delivery-network/app-service-web-purge-cdn.png)

### <a name="verify-that-the-cdn-is-updated"></a><span data-ttu-id="1c2e8-190">Ověření aktualizace CDN</span><span class="sxs-lookup"><span data-stu-id="1c2e8-190">Verify that the CDN is updated</span></span>

<span data-ttu-id="1c2e8-191">Počkejte na dokončení zpracování žádosti o vyprázdnění, obvykle to trvá pár minut.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-191">Wait until the purge request finishes processing, typically a couple of minutes.</span></span> <span data-ttu-id="1c2e8-192">Pokud chcete zobrazit aktuální stav, vyberte ikonu zvonku v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-192">To see the current status, select the bell icon at the top of the page.</span></span> 

![Oznámení vyprázdnění](media/app-service-web-tutorial-content-delivery-network/portal-purge-notification.png)

<span data-ttu-id="1c2e8-194">Přejděte na adresu URL koncového bodu CDN pro soubor `index.html` a nyní uvidíte text „V2“, který jste přidali do nadpisu na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-194">Browse to the CDN endpoint URL for `index.html`, and now you see the V2 that you added to the title on the home page.</span></span> <span data-ttu-id="1c2e8-195">To ukazuje, že se mezipaměť CDN aktualizovala.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-195">This shows that the CDN cache has been refreshed.</span></span>

```
http://<endpointname>.azureedge.net/index.html
```

![V2 v nadpisu v CDN](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title.png)

<span data-ttu-id="1c2e8-197">Další informace najdete v tématu [Vyprázdnění koncového bodu Azure CDN](../cdn/cdn-purge-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="1c2e8-197">For more information, see [Purge an Azure CDN endpoint](../cdn/cdn-purge-endpoint.md).</span></span> 

## <a name="use-query-strings-to-version-content"></a><span data-ttu-id="1c2e8-198">Správa verzí obsahu pomocí řetězců dotazu</span><span class="sxs-lookup"><span data-stu-id="1c2e8-198">Use query strings to version content</span></span>

<span data-ttu-id="1c2e8-199">Azure CDN nabízí následující možnosti chování při ukládání do mezipaměti:</span><span class="sxs-lookup"><span data-stu-id="1c2e8-199">The Azure CDN offers the following caching behavior options:</span></span>

* <span data-ttu-id="1c2e8-200">Ignorovat řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="1c2e8-200">Ignore query strings</span></span>
* <span data-ttu-id="1c2e8-201">Nepoužívat ukládání do mezipaměti pro řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="1c2e8-201">Bypass caching for query strings</span></span>
* <span data-ttu-id="1c2e8-202">Ukládat do mezipaměti každou jedinečnou adresu URL</span><span class="sxs-lookup"><span data-stu-id="1c2e8-202">Cache every unique URL</span></span> 

<span data-ttu-id="1c2e8-203">První z nich je výchozí, což znamená, že existuje jenom jedna verze uložené v mezipaměti majetku bez ohledu na to, řetězec dotazu v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-203">The first of these is the default, which means there is only one cached version of an asset regardless of the query string in the URL.</span></span> 

<span data-ttu-id="1c2e8-204">V této části kurzu změníte chování při ukládání do mezipaměti tak, že se do mezipaměti bude ukládat každá jedinečná adresa URL.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-204">In this section of the tutorial, you change the caching behavior to cache every unique URL.</span></span>

### <a name="change-the-cache-behavior"></a><span data-ttu-id="1c2e8-205">Změna chování mezipaměti</span><span class="sxs-lookup"><span data-stu-id="1c2e8-205">Change the cache behavior</span></span>

<span data-ttu-id="1c2e8-206">Na webu Azure Portal na stránce **Koncový bod CDN** vyberte **Mezipaměť**.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-206">In the Azure portal **CDN Endpoint** page, select **Cache**.</span></span>

<span data-ttu-id="1c2e8-207">Z rozevíracího seznamu **Chování při ukládání řetězců dotazu do mezipaměti** vyberte **Ukládat do mezipaměti každou jedinečnou adresu URL**.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-207">Select **Cache every unique URL** from the **Query string caching behavior** drop-down list.</span></span>

<span data-ttu-id="1c2e8-208">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-208">Select **Save**.</span></span>

![Výběr chování při ukládání řetězce dotazu do mezipaměti](media/app-service-web-tutorial-content-delivery-network/portal-select-caching-behavior.png)

### <a name="verify-that-unique-urls-are-cached-separately"></a><span data-ttu-id="1c2e8-210">Ověření samostatného ukládání jedinečných adres URL do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="1c2e8-210">Verify that unique URLs are cached separately</span></span>

<span data-ttu-id="1c2e8-211">V prohlížeči přejděte na domovskou stránku na koncovém bodu CDN, ale přidejte řetězec dotazu:</span><span class="sxs-lookup"><span data-stu-id="1c2e8-211">In a browser, navigate to the home page at the CDN endpoint, but include a query string:</span></span> 

```
http://<endpointname>.azureedge.net/index.html?q=1
```

<span data-ttu-id="1c2e8-212">CDN vrátí aktuální obsah webové aplikace, který obsahuje text „V2“ v nadpisu.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-212">The CDN returns the current web app content, which includes "V2" in the heading.</span></span> 

<span data-ttu-id="1c2e8-213">Abyste zajistili uložení této stránky v mezipaměti v CDN, aktualizujte stránku.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-213">To ensure that this page is cached in the CDN, refresh the page.</span></span> 

<span data-ttu-id="1c2e8-214">Otevřete soubor `index.html`, změňte „V2“ na „V3“ a nasaďte změnu.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-214">Open `index.html` and change "V2" to "V3", and deploy the change.</span></span> 

```bash
git commit -am "version 3"
git push azure master
```

<span data-ttu-id="1c2e8-215">V prohlížeči přejděte na adresu URL koncového bodu CDN s novým řetězcem dotazu, například `q=2`.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-215">In a browser, go to the CDN endpoint URL with a new query string such as `q=2`.</span></span> <span data-ttu-id="1c2e8-216">CDN získá aktuální soubor `index.html` a zobrazí „V3“.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-216">The CDN gets the current `index.html` file and displays "V3".</span></span>  <span data-ttu-id="1c2e8-217">Pokud ale přejdete na koncový bod CDN s řetězcem dotazu `q=1`, uvidíte „V2“.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-217">But if you navigate to the CDN endpoint with the `q=1` query string, you see "V2".</span></span>

```
http://<endpointname>.azureedge.net/index.html?q=2
```

![V3 v nadpisu v CDN, řetězec dotazu 2](media/app-service-web-tutorial-content-delivery-network/v3-in-cdn-title-qs2.png)

```
http://<endpointname>.azureedge.net/index.html?q=1
```

![V2 v nadpisu v CDN, řetězec dotazu 1](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title-qs1.png)

<span data-ttu-id="1c2e8-220">Výstup ukazuje, že každý řetězec dotazu je zpracovávat odděleně:</span><span class="sxs-lookup"><span data-stu-id="1c2e8-220">This output shows that each query string is treated differently:</span></span>

* <span data-ttu-id="1c2e8-221">q = 1 byl používán před, takže obsah uložený v mezipaměti, jsou vráceny (V2).</span><span class="sxs-lookup"><span data-stu-id="1c2e8-221">q=1 was used before, so cached contents are returned (V2).</span></span>
* <span data-ttu-id="1c2e8-222">q = 2 je novou, proto se nejnovější obsah webové aplikace načíst a vrátí (V3).</span><span class="sxs-lookup"><span data-stu-id="1c2e8-222">q=2 is new, so the latest web app contents are retrieved and returned (V3).</span></span>

<span data-ttu-id="1c2e8-223">Další informace najdete v tématu [Řízení chování Azure CDN při ukládání řetězců dotazu do mezipaměti](../cdn/cdn-query-string.md).</span><span class="sxs-lookup"><span data-stu-id="1c2e8-223">For more information, see [Control Azure CDN caching behavior with query strings](../cdn/cdn-query-string.md).</span></span>

## <a name="map-a-custom-domain-to-a-cdn-endpoint"></a><span data-ttu-id="1c2e8-224">Mapování vlastní domény na koncový bod CDN</span><span class="sxs-lookup"><span data-stu-id="1c2e8-224">Map a custom domain to a CDN endpoint</span></span>

<span data-ttu-id="1c2e8-225">Na koncový bod CDN namapujete vlastní doménu vytvořením záznamu CNAME.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-225">You'll map your custom domain to your CDN Endpoint by creating a CNAME record.</span></span> <span data-ttu-id="1c2e8-226">Záznam CNAME je funkce DNS, která mapuje zdrojovou doménu na cílovou doménu.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-226">A CNAME record is a DNS feature that maps a source domain to a destination domain.</span></span> <span data-ttu-id="1c2e8-227">Například můžete namapovat `cdn.contoso.com` nebo `static.contoso.com` na `contoso.azureedge.net`.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-227">For example, you might map `cdn.contoso.com` or `static.contoso.com` to `contoso.azureedge.net`.</span></span>

<span data-ttu-id="1c2e8-228">Pokud nemáte vlastní doménu, zvažte nákup domény pomocí webu Azure Portal podle postupu v [kurzu k doménám App Service](custom-dns-web-site-buydomains-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="1c2e8-228">If you don't have a custom domain, consider following the [App Service domain tutorial](custom-dns-web-site-buydomains-web-app.md) to purchase a domain using the Azure portal.</span></span> 

### <a name="find-the-hostname-to-use-with-the-cname"></a><span data-ttu-id="1c2e8-229">Zjištění názvu hostitele pro použití v záznamu CNAME</span><span class="sxs-lookup"><span data-stu-id="1c2e8-229">Find the hostname to use with the CNAME</span></span>

<span data-ttu-id="1c2e8-230">Na webu Azure Portal se na stránce **Koncový bod** ujistěte, že je na levém navigačním panelu vybrán **Přehled** a pak vyberte tlačítko **+ Vlastní doména** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-230">In the Azure portal **Endpoint** page, make sure **Overview** is selected in the left navigation, and then select the **+ Custom Domain** button at the top of the page.</span></span>

![Výběr možnosti Přidat vlastní doménu](media/app-service-web-tutorial-content-delivery-network/portal-select-add-domain.png)

<span data-ttu-id="1c2e8-232">Na stránce **Přidat vlastní doménu** se zobrazí název hostitele koncového bodu pro použití k vytvoření záznamu CNAME.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-232">In the **Add a custom domain** page, you see the endpoint host name to use in creating a CNAME record.</span></span> <span data-ttu-id="1c2e8-233">Název hostitele je odvozený z adresy URL koncového bodu CDN: **&lt;název_koncového_bodu>.azureedge.net**.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-233">The host name is derived from your CDN endpoint URL: **&lt;EndpointName>.azureedge.net**.</span></span> 

![Stránka Přidat doménu](media/app-service-web-tutorial-content-delivery-network/portal-add-domain.png)

### <a name="configure-the-cname-with-your-domain-registrar"></a><span data-ttu-id="1c2e8-235">Konfigurace záznamu CNAME u registrátora domény</span><span class="sxs-lookup"><span data-stu-id="1c2e8-235">Configure the CNAME with your domain registrar</span></span>

<span data-ttu-id="1c2e8-236">Přejděte na web registrátora vaší domény a vyhledejte část pro vytváření záznamů DNS.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-236">Navigate to your domain registrar's web site, and locate the section for creating DNS records.</span></span> <span data-ttu-id="1c2e8-237">Může to být v části, jako je **Název domény**, **DNS** nebo **Správa názvového serveru**.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-237">You might find this in a section such as **Domain Name**, **DNS**, or **Name Server Management**.</span></span>

<span data-ttu-id="1c2e8-238">Najděte část pro správu záznamů CNAME.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-238">Find the section for managing CNAMEs.</span></span> <span data-ttu-id="1c2e8-239">Možná bude nutné přejít na stránku rozšířeného nastavení a hledat slova jako CNAME, Alias nebo Subdomény.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-239">You may have to go to an advanced settings page and look for the words CNAME, Alias, or Subdomains.</span></span>

<span data-ttu-id="1c2e8-240">Vytvořit záznam CNAME, který mapuje vaši zvolenou subdomény (například **statické** nebo **cdn**) k **názvu hostitele koncového bodu** uvedené výše v portálu.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-240">Create a CNAME record that maps your chosen subdomain (for example, **static** or **cdn**) to the **Endpoint host name** shown earlier in the portal.</span></span> 

### <a name="enter-the-custom-domain-in-azure"></a><span data-ttu-id="1c2e8-241">Zadání vlastní domény v Azure</span><span class="sxs-lookup"><span data-stu-id="1c2e8-241">Enter the custom domain in Azure</span></span>

<span data-ttu-id="1c2e8-242">Vraťte se na stránku **Přidat vlastní doménu** a do dialogového okna zadejte vlastní doménu včetně poddomény.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-242">Return to the **Add a custom domain** page, and enter your custom domain, including the subdomain, in the dialog box.</span></span> <span data-ttu-id="1c2e8-243">Zadejte například `cdn.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-243">For example, enter `cdn.contoso.com`.</span></span>   
   
<span data-ttu-id="1c2e8-244">Azure ověří, že pro zadaný název domény existuje záznam CNAME.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-244">Azure verifies that the CNAME record exists for the domain name you have entered.</span></span> <span data-ttu-id="1c2e8-245">Pokud je záznam CNAME správný, vaše vlastní doména se ověří.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-245">If the CNAME is correct, your custom domain is validated.</span></span>

<span data-ttu-id="1c2e8-246">Rozšíření záznamu CNAME na názvové servery na internetu může nějakou dobu trvat.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-246">It can take time for the CNAME record to propagate to name servers on the Internet.</span></span> <span data-ttu-id="1c2e8-247">Pokud vaše doména není ověřený okamžitě, počkejte několik minut a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-247">If your domain is not validated immediately, wait a few minutes and try again.</span></span>

### <a name="test-the-custom-domain"></a><span data-ttu-id="1c2e8-248">Testování vlastní domény</span><span class="sxs-lookup"><span data-stu-id="1c2e8-248">Test the custom domain</span></span>

<span data-ttu-id="1c2e8-249">V prohlížeči přejděte na soubor `index.html` s použitím vlastní domény (například `cdn.contoso.com/index.html`) a ověřte, že výsledek je stejný, jako když přejdete přímo na `<endpointname>azureedge.net/index.html`.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-249">In a browser, navigate to the `index.html` file using your custom domain (for example, `cdn.contoso.com/index.html`) to verify that the result is the same as when you go directly to `<endpointname>azureedge.net/index.html`.</span></span>

![Domovská stránka ukázkové aplikace s použitím adresy URL vlastní domény](media/app-service-web-tutorial-content-delivery-network/home-page-custom-domain.png)

<span data-ttu-id="1c2e8-251">Další informace najdete v tématu [Mapování obsahu Azure CDN na vlastní doménu](../cdn/cdn-map-content-to-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="1c2e8-251">For more information, see [Map Azure CDN content to a custom domain](../cdn/cdn-map-content-to-custom-domain.md).</span></span>

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="1c2e8-252">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1c2e8-252">Next steps</span></span>

<span data-ttu-id="1c2e8-253">Co jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="1c2e8-253">What you learned:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1c2e8-254">Vytvořit koncový bod CDN.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-254">Create a CDN endpoint.</span></span>
> * <span data-ttu-id="1c2e8-255">Aktualizovat prostředky uložené v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-255">Refresh cached assets.</span></span>
> * <span data-ttu-id="1c2e8-256">Spravovat verze uložené v mezipaměti pomocí řetězců dotazu.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-256">Use query strings to control cached versions.</span></span>
> * <span data-ttu-id="1c2e8-257">Použít vlastní doménu pro koncový bod CDN.</span><span class="sxs-lookup"><span data-stu-id="1c2e8-257">Use a custom domain for the CDN endpoint.</span></span>

<span data-ttu-id="1c2e8-258">Informace o optimalizaci výkonu CDN v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="1c2e8-258">Learn how to optimize CDN performance in the following articles:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1c2e8-259">Vylepšení výkonu prostřednictvím komprimace souborů v Azure CDN</span><span class="sxs-lookup"><span data-stu-id="1c2e8-259">Improve performance by compressing files in Azure CDN</span></span>](../cdn/cdn-improve-performance.md)

> [!div class="nextstepaction"]
> [<span data-ttu-id="1c2e8-260">Předběžné načtení prostředků v koncovém bodu Azure CDN</span><span class="sxs-lookup"><span data-stu-id="1c2e8-260">Pre-load assets on an Azure CDN endpoint</span></span>](../cdn/cdn-preload-endpoint.md)
