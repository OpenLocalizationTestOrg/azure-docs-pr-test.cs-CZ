---
title: aaaAdd tooan CDN Azure App Service | Microsoft Docs
description: "Přidejte toocache Content Delivery Network (CDN) tooan Azure App Service a poskytovat statické soubory ze serverů zavřít tooyour zákazníků ohledně hello, world."
services: app-service\web
author: syntaxc4
ms.author: cfowler
ms.date: 05/31/2017
ms.topic: tutorial
ms.service: app-service-web
manager: erikre
ms.workload: web
ms.custom: mvc
ms.openlocfilehash: 88b7fd884517279064472b804a6d1dc2921cbd24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-content-delivery-network-cdn-tooan-azure-app-service"></a><span data-ttu-id="7f71e-103">Přidat tooan Content Delivery Network (CDN) Azure App Service</span><span class="sxs-lookup"><span data-stu-id="7f71e-103">Add a Content Delivery Network (CDN) tooan Azure App Service</span></span>

<span data-ttu-id="7f71e-104">[Sítě doručování Azure obsahu (CDN)](../cdn/cdn-overview.md) ukládá do mezipaměti na strategicky umístěných místech tooprovide maximální propustnost pro doručování obsahu toousers statický webový obsah.</span><span class="sxs-lookup"><span data-stu-id="7f71e-104">[Azure Content Delivery Network (CDN)](../cdn/cdn-overview.md) caches static web content at strategically placed locations tooprovide maximum throughput for delivering content toousers.</span></span> <span data-ttu-id="7f71e-105">Hello CDN také snižuje zatížení serveru ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7f71e-105">hello CDN also decreases server load on your web app.</span></span> <span data-ttu-id="7f71e-106">Tento kurz ukazuje, jak Azure CDN tooa tooadd [webové aplikace v Azure App Service](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7f71e-106">This tutorial shows how tooadd Azure CDN tooa [web app in Azure App Service](app-service-web-overview.md).</span></span> 

<span data-ttu-id="7f71e-107">Tady je hello domovskou stránku hello ukázka statické HTML webového serveru, který bude fungovat s:</span><span class="sxs-lookup"><span data-stu-id="7f71e-107">Here's hello home page of hello sample static HTML site that you'll work with:</span></span>

![Domovská stránka ukázkové aplikace](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page.png)

<span data-ttu-id="7f71e-109">Získáte informace:</span><span class="sxs-lookup"><span data-stu-id="7f71e-109">What you'll learn:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7f71e-110">Vytvořit koncový bod CDN.</span><span class="sxs-lookup"><span data-stu-id="7f71e-110">Create a CDN endpoint.</span></span>
> * <span data-ttu-id="7f71e-111">Aktualizovat prostředky uložené v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="7f71e-111">Refresh cached assets.</span></span>
> * <span data-ttu-id="7f71e-112">Použití dotazu řetězce verze toocontrol do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="7f71e-112">Use query strings toocontrol cached versions.</span></span>
> * <span data-ttu-id="7f71e-113">Použijte vlastní doménu pro koncový bod CDN hello.</span><span class="sxs-lookup"><span data-stu-id="7f71e-113">Use a custom domain for hello CDN endpoint.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7f71e-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7f71e-114">Prerequisites</span></span>

<span data-ttu-id="7f71e-115">toocomplete v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="7f71e-115">toocomplete this tutorial:</span></span>

- <span data-ttu-id="7f71e-116">[Nainstalovat Git](https://git-scm.com/).</span><span class="sxs-lookup"><span data-stu-id="7f71e-116">[Install Git](https://git-scm.com/)</span></span>
- [<span data-ttu-id="7f71e-117">Instalace rozhraní příkazového řádku Azure 2.0</span><span class="sxs-lookup"><span data-stu-id="7f71e-117">Install Azure CLI 2.0</span></span>](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-hello-web-app"></a><span data-ttu-id="7f71e-118">Vytvoření webové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="7f71e-118">Create hello web app</span></span>

<span data-ttu-id="7f71e-119">toocreate hello webovou aplikaci, která bude fungovat s, postupujte podle hello [statické rychlý start HTML](app-service-web-get-started-html.md) prostřednictvím hello **procházet toohello aplikace** krok.</span><span class="sxs-lookup"><span data-stu-id="7f71e-119">toocreate hello web app that you'll work with, follow hello [static HTML quickstart](app-service-web-get-started-html.md) through hello **Browse toohello app** step.</span></span>

### <a name="have-a-custom-domain-ready"></a><span data-ttu-id="7f71e-120">Připravení vlastní domény</span><span class="sxs-lookup"><span data-stu-id="7f71e-120">Have a custom domain ready</span></span>

<span data-ttu-id="7f71e-121">toocomplete hello vlastní domény krok tohoto kurzu potřebujete tooown vlastní doménu a mít přístup tooyour DNS registru pro poskytovatele domény (například GoDaddy).</span><span class="sxs-lookup"><span data-stu-id="7f71e-121">toocomplete hello custom domain step of this tutorial, you need tooown a custom domain and have access tooyour DNS registry for your domain provider (such as GoDaddy).</span></span> <span data-ttu-id="7f71e-122">Tooadd záznamy DNS, třeba pro `contoso.com` a `www.contoso.com`, musíte mít přístup tooconfigure hello nastavení DNS pro hello `contoso.com` kořenové domény.</span><span class="sxs-lookup"><span data-stu-id="7f71e-122">For example, tooadd DNS entries for `contoso.com` and `www.contoso.com`, you must have access tooconfigure hello DNS settings for hello `contoso.com` root domain.</span></span>

<span data-ttu-id="7f71e-123">Pokud ještě nemáte název domény, zvažte následující hello [kurzu domény služby App Service](custom-dns-web-site-buydomains-web-app.md) hello toopurchase domény pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7f71e-123">If you don't already have a domain name, consider following hello [App Service domain tutorial](custom-dns-web-site-buydomains-web-app.md) toopurchase a domain using hello Azure portal.</span></span> 

## <a name="log-in-toohello-azure-portal"></a><span data-ttu-id="7f71e-124">Přihlaste se toohello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7f71e-124">Log in toohello Azure portal</span></span>

<span data-ttu-id="7f71e-125">Otevřete prohlížeč a přejděte toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7f71e-125">Open a browser and navigate toohello [Azure portal](https://portal.azure.com).</span></span>

## <a name="create-a-cdn-profile-and-endpoint"></a><span data-ttu-id="7f71e-126">Vytvoření koncového bodu a profilu CDN</span><span class="sxs-lookup"><span data-stu-id="7f71e-126">Create a CDN profile and endpoint</span></span>

<span data-ttu-id="7f71e-127">V levé navigační hello, vyberte **App Services**a pak vyberte hello aplikaci, kterou jste vytvořili v hello [statické rychlý start HTML](app-service-web-get-started-html.md).</span><span class="sxs-lookup"><span data-stu-id="7f71e-127">In hello left navigation, select **App Services**, and then select hello app that you created in hello [static HTML quickstart](app-service-web-get-started-html.md).</span></span>

![Vyberte aplikace App Service hello portálu](media/app-service-web-tutorial-content-delivery-network/portal-select-app-services.png)

<span data-ttu-id="7f71e-129">V hello **služby App Service** stránku hello **nastavení** vyberte **sítě > Konfigurovat Azure CDN pro vaši aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="7f71e-129">In hello **App Service** page, in hello **Settings** section, select **Networking > Configure Azure CDN for your app**.</span></span>

![Vyberte CDN hello portálu](media/app-service-web-tutorial-content-delivery-network/portal-select-cdn.png)

<span data-ttu-id="7f71e-131">V hello **Azure Content Delivery Network** zadejte hello **nový koncový bod** nastavení uvedeného v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="7f71e-131">In hello **Azure Content Delivery Network** page, provide hello **New endpoint** settings as specified in hello table.</span></span>

![Vytvoření profilu a koncového bodu hello portálu](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint.png)

| <span data-ttu-id="7f71e-133">Nastavení</span><span class="sxs-lookup"><span data-stu-id="7f71e-133">Setting</span></span> | <span data-ttu-id="7f71e-134">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="7f71e-134">Suggested value</span></span> | <span data-ttu-id="7f71e-135">Popis</span><span class="sxs-lookup"><span data-stu-id="7f71e-135">Description</span></span> |
| ------- | --------------- | ----------- |
| <span data-ttu-id="7f71e-136">**Profil CDN**</span><span class="sxs-lookup"><span data-stu-id="7f71e-136">**CDN profile**</span></span> | <span data-ttu-id="7f71e-137">myCDNProfile</span><span class="sxs-lookup"><span data-stu-id="7f71e-137">myCDNProfile</span></span> | <span data-ttu-id="7f71e-138">Vyberte **vytvořit nový** toocreate profilu CDN.</span><span class="sxs-lookup"><span data-stu-id="7f71e-138">Select **Create new** toocreate a CDN profile.</span></span> <span data-ttu-id="7f71e-139">Profil CDN je kolekce koncové body CDN pomocí hello stejné cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="7f71e-139">A CDN profile is a collection of CDN endpoints with hello same pricing tier.</span></span> |
| <span data-ttu-id="7f71e-140">**Cenová úroveň**</span><span class="sxs-lookup"><span data-stu-id="7f71e-140">**Pricing tier**</span></span> | <span data-ttu-id="7f71e-141">Akamai Standard</span><span class="sxs-lookup"><span data-stu-id="7f71e-141">Standard Akamai</span></span> | <span data-ttu-id="7f71e-142">Hello [cenová úroveň](../cdn/cdn-overview.md#azure-cdn-features) určuje hello zprostředkovatele a dostupných funkcí.</span><span class="sxs-lookup"><span data-stu-id="7f71e-142">hello [pricing tier](../cdn/cdn-overview.md#azure-cdn-features) specifies hello provider and available features.</span></span> <span data-ttu-id="7f71e-143">V tomto kurzu používáme Standard Akamai.</span><span class="sxs-lookup"><span data-stu-id="7f71e-143">In this tutorial, we are using Standard Akamai.</span></span> |
| <span data-ttu-id="7f71e-144">**Název koncového bodu CDN**</span><span class="sxs-lookup"><span data-stu-id="7f71e-144">**CDN endpoint name**</span></span> | <span data-ttu-id="7f71e-145">Libovolný název, který je jedinečný v doméně azureedge.net hello</span><span class="sxs-lookup"><span data-stu-id="7f71e-145">Any name that is unique in hello azureedge.net domain</span></span> | <span data-ttu-id="7f71e-146">Přístup k prostředkům v mezipaměti v doméně hello  *\<endpointname >. azureedge.net*.</span><span class="sxs-lookup"><span data-stu-id="7f71e-146">You access your cached resources at hello domain *\<endpointname>.azureedge.net*.</span></span>

<span data-ttu-id="7f71e-147">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7f71e-147">Select **Create**.</span></span>

<span data-ttu-id="7f71e-148">Azure vytvoří hello profilu a koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="7f71e-148">Azure creates hello profile and endpoint.</span></span> <span data-ttu-id="7f71e-149">Nový koncový bod Hello se zobrazí v hello **koncové body** seznam na hello stejné stránce, a pokud je zřízený hello stav **systémem**.</span><span class="sxs-lookup"><span data-stu-id="7f71e-149">hello new endpoint appears in hello **Endpoints** list on hello same page, and when it's provisioned hello status is **Running**.</span></span>

![Nový koncový bod v seznamu](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint-in-list.png)

### <a name="test-hello-cdn-endpoint"></a><span data-ttu-id="7f71e-151">Test hello koncový bod CDN</span><span class="sxs-lookup"><span data-stu-id="7f71e-151">Test hello CDN endpoint</span></span>

<span data-ttu-id="7f71e-152">Pokud jste vybrali cenovou úroveň Verizon, rozšíření koncového bodu obvykle trvá přibližně 90 minut.</span><span class="sxs-lookup"><span data-stu-id="7f71e-152">If you selected Verizon pricing tier, it typically takes about 90 minutes for endpoint propagation.</span></span> <span data-ttu-id="7f71e-153">U Akamai trvá rozšíření několik minut.</span><span class="sxs-lookup"><span data-stu-id="7f71e-153">For Akamai, it takes a couple minutes for propagation</span></span>

<span data-ttu-id="7f71e-154">Ukázková aplikace Hello má `index.html` souboru a *šablon stylů css*, *img*, a *js* složek, které obsahují další statické prostředky.</span><span class="sxs-lookup"><span data-stu-id="7f71e-154">hello sample app has an `index.html` file and *css*, *img*, and *js* folders that contain other static assets.</span></span> <span data-ttu-id="7f71e-155">Hello obsahu cesty pro všechny tyto soubory jsou stejné hello na koncový bod CDN hello.</span><span class="sxs-lookup"><span data-stu-id="7f71e-155">hello content paths for all of these files are hello same at hello CDN endpoint.</span></span> <span data-ttu-id="7f71e-156">Například obě hello následující adresy URL přístup hello *bootstrap.css* souboru v hello *šablon stylů css* složky:</span><span class="sxs-lookup"><span data-stu-id="7f71e-156">For example, both of hello following URLs access hello *bootstrap.css* file in hello *css* folder:</span></span>

```
http://<appname>.azurewebsites.net/css/bootstrap.css
```

```
http://<endpointname>.azureedge.net/css/bootstrap.css
```

<span data-ttu-id="7f71e-157">Přejděte následující adresu URL toohello prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="7f71e-157">Navigate a browser toohello following URL:</span></span>

```
http://<endpointname>.azureedge.net/index.html
```

![Domovská stránka ukázkové aplikace poskytnutá z CDN](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page-cdn.png)

 <span data-ttu-id="7f71e-159">Zobrazí hello stejné stránka byla spuštěna dříve v webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="7f71e-159">You see hello same page that you ran earlier in an Azure web app.</span></span> <span data-ttu-id="7f71e-160">Azure CDN má načíst prostředky hello počátek webové aplikace a jejich obsluhuje z koncového bodu CDN hello</span><span class="sxs-lookup"><span data-stu-id="7f71e-160">Azure CDN has retrieved hello origin web app's assets and is serving them from hello CDN endpoint</span></span>

<span data-ttu-id="7f71e-161">tooensure, tato stránka je do mezipaměti v hello CDN, stránku hello aktualizace.</span><span class="sxs-lookup"><span data-stu-id="7f71e-161">tooensure that this page is cached in hello CDN, refresh hello page.</span></span> <span data-ttu-id="7f71e-162">Dva požadavky pro hello stejné asset jsou někdy požadované pro hello CDN toocache hello požadovaný obsah.</span><span class="sxs-lookup"><span data-stu-id="7f71e-162">Two requests for hello same asset are sometimes required for hello CDN toocache hello requested content.</span></span>

<span data-ttu-id="7f71e-163">Další informace o vytváření koncových bodů a profilů CDN najdete v tématu [Začínáme s Azure CDN](../cdn/cdn-create-new-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="7f71e-163">For more information about creating Azure CDN profiles and endpoints, see [Getting started with Azure CDN](../cdn/cdn-create-new-endpoint.md).</span></span>

## <a name="purge-hello-cdn"></a><span data-ttu-id="7f71e-164">Vyprázdnění hello CDN</span><span class="sxs-lookup"><span data-stu-id="7f71e-164">Purge hello CDN</span></span>

<span data-ttu-id="7f71e-165">Hello CDN se pravidelně aktualizuje jejích prostředcích z hello počátek webové aplikace na základě hello time to live (TTL) konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7f71e-165">hello CDN periodically refreshes its resources from hello origin web app based on hello time-to-live (TTL) configuration.</span></span> <span data-ttu-id="7f71e-166">Hello výchozí hodnota TTL je sedm dní.</span><span class="sxs-lookup"><span data-stu-id="7f71e-166">hello default TTL is seven days.</span></span>

<span data-ttu-id="7f71e-167">V některých případech bude pravděpodobně nutné toorefresh hello CDN před hello vypršení platnosti TTL – například když nasadíte aktualizované obsahu toohello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="7f71e-167">At times you might need toorefresh hello CDN before hello TTL expiration -- for example, when you deploy updated content toohello web app.</span></span> <span data-ttu-id="7f71e-168">tootrigger aktualizace, můžete ručně vyprázdnit hello CDN prostředky.</span><span class="sxs-lookup"><span data-stu-id="7f71e-168">tootrigger a refresh, you can manually purge hello CDN resources.</span></span> 

<span data-ttu-id="7f71e-169">V této části kurzu hello nasazení změn toohello webové aplikace a mazání hello CDN tootrigger hello CDN toorefresh své mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="7f71e-169">In this section of hello tutorial, you deploy a change toohello web app and purge hello CDN tootrigger hello CDN toorefresh its cache.</span></span>

### <a name="deploy-a-change-toohello-web-app"></a><span data-ttu-id="7f71e-170">Nasazení webové aplikace toohello změn</span><span class="sxs-lookup"><span data-stu-id="7f71e-170">Deploy a change toohello web app</span></span>

<span data-ttu-id="7f71e-171">Otevřete hello `index.html` souboru a přidejte "-V2" toohello H1 záhlaví, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="7f71e-171">Open hello `index.html` file and add "- V2" toohello H1 heading, as shown in hello following example:</span></span> 

```
<h1>Azure App Service - Sample Static HTML Site - V2</h1>
```

<span data-ttu-id="7f71e-172">Potvrdit změny a nasaďte ji toohello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="7f71e-172">Commit your change and deploy it toohello web app.</span></span>

```bash
git commit -am "version 2"
git push azure master
```

<span data-ttu-id="7f71e-173">Po dokončení nasazení adresy URL procházet toohello webové aplikace a v tématu hello změnit.</span><span class="sxs-lookup"><span data-stu-id="7f71e-173">Once deployment has completed, browse toohello web app URL and you see hello change.</span></span>

```
http://<appname>.azurewebsites.net/index.html
```

![V2 v nadpisu ve webové aplikaci](media/app-service-web-tutorial-content-delivery-network/v2-in-web-app-title.png)

<span data-ttu-id="7f71e-175">Vyhledejte koncový bod CDN toohello adresa URL pro domovskou stránku hello a nevidíte hello změnit, protože v mezipaměti verzi hello v hello CDN ještě nevypršela.</span><span class="sxs-lookup"><span data-stu-id="7f71e-175">Browse toohello CDN endpoint URL for hello home page and you don't see hello change because hello cached version in hello CDN hasn't expired yet.</span></span> 

```
http://<endpointname>.azureedge.net/index.html
```

![Žádné V2 v nadpisu v CDN](media/app-service-web-tutorial-content-delivery-network/no-v2-in-cdn-title.png)

### <a name="purge-hello-cdn-in-hello-portal"></a><span data-ttu-id="7f71e-177">Vyprázdnění hello CDN hello portálu</span><span class="sxs-lookup"><span data-stu-id="7f71e-177">Purge hello CDN in hello portal</span></span>

<span data-ttu-id="7f71e-178">tootrigger hello CDN tooupdate jeho verze v mezipaměti, vyprázdnit hello CDN.</span><span class="sxs-lookup"><span data-stu-id="7f71e-178">tootrigger hello CDN tooupdate its cached version, purge hello CDN.</span></span>

<span data-ttu-id="7f71e-179">V levém navigačním hello portálu, vyberte **skupiny prostředků**a pak vyberte skupinu prostředků hello, kterou jste vytvořili pro webové aplikace (myResourceGroup).</span><span class="sxs-lookup"><span data-stu-id="7f71e-179">In hello portal left navigation, select **Resource groups**, and then select hello resource group that you created for your web app (myResourceGroup).</span></span>

![Výběr skupiny prostředků](media/app-service-web-tutorial-content-delivery-network/portal-select-group.png)

<span data-ttu-id="7f71e-181">V seznamu hello prostředků vyberte koncový bod CDN.</span><span class="sxs-lookup"><span data-stu-id="7f71e-181">In hello list of resources, select your CDN endpoint.</span></span>

![Výběr koncového bodu](media/app-service-web-tutorial-content-delivery-network/portal-select-endpoint.png)

<span data-ttu-id="7f71e-183">Hello horní části hello **koncový bod** klikněte na tlačítko **mazání**.</span><span class="sxs-lookup"><span data-stu-id="7f71e-183">At hello top of hello **Endpoint** page, click **Purge**.</span></span>

![Výběr možnosti Vyprázdnit](media/app-service-web-tutorial-content-delivery-network/portal-select-purge.png)

<span data-ttu-id="7f71e-185">Zadejte cesty obsahu hello chcete toopurge.</span><span class="sxs-lookup"><span data-stu-id="7f71e-185">Enter hello content paths you wish toopurge.</span></span> <span data-ttu-id="7f71e-186">Můžete předat toopurge cesta dokončení souboru jednotlivých souborů nebo toopurge segmentu cesty a obnovit veškerý obsah ve složce.</span><span class="sxs-lookup"><span data-stu-id="7f71e-186">You can pass a complete file path toopurge an individual file, or a path segment toopurge and refresh all content in a folder.</span></span> <span data-ttu-id="7f71e-187">Vzhledem k tomu, že jste změnili `index.html`, ujistěte se, který je jedním z cesty hello.</span><span class="sxs-lookup"><span data-stu-id="7f71e-187">Since you changed `index.html`, make sure that is one of hello paths.</span></span>

<span data-ttu-id="7f71e-188">V dolní části hello hello stránky, vyberte **mazání**.</span><span class="sxs-lookup"><span data-stu-id="7f71e-188">At hello bottom of hello page, select **Purge**.</span></span>

![Stránka Vyprázdnit](media/app-service-web-tutorial-content-delivery-network/app-service-web-purge-cdn.png)

### <a name="verify-that-hello-cdn-is-updated"></a><span data-ttu-id="7f71e-190">Ověřte, že hello, který se aktualizuje CDN</span><span class="sxs-lookup"><span data-stu-id="7f71e-190">Verify that hello CDN is updated</span></span>

<span data-ttu-id="7f71e-191">Počkejte, dokud žádost o vyprázdnění hello dokončí zpracování, obvykle během několika minut.</span><span class="sxs-lookup"><span data-stu-id="7f71e-191">Wait until hello purge request finishes processing, typically a couple of minutes.</span></span> <span data-ttu-id="7f71e-192">toosee hello aktuální stav, vyberte hello ikonu zvonku v horní části hello hello stránky.</span><span class="sxs-lookup"><span data-stu-id="7f71e-192">toosee hello current status, select hello bell icon at hello top of hello page.</span></span> 

![Oznámení vyprázdnění](media/app-service-web-tutorial-content-delivery-network/portal-purge-notification.png)

<span data-ttu-id="7f71e-194">Vyhledejte adresu URL koncového bodu CDN toohello pro `index.html`, a nyní se zobrazí hello V2 přidat název toohello na domovskou stránku hello.</span><span class="sxs-lookup"><span data-stu-id="7f71e-194">Browse toohello CDN endpoint URL for `index.html`, and now you see hello V2 that you added toohello title on hello home page.</span></span> <span data-ttu-id="7f71e-195">Ukazuje to, že hello CDN mezipaměti byla aktualizována.</span><span class="sxs-lookup"><span data-stu-id="7f71e-195">This shows that hello CDN cache has been refreshed.</span></span>

```
http://<endpointname>.azureedge.net/index.html
```

![V2 v nadpisu v CDN](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title.png)

<span data-ttu-id="7f71e-197">Další informace najdete v tématu [Vyprázdnění koncového bodu Azure CDN](../cdn/cdn-purge-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="7f71e-197">For more information, see [Purge an Azure CDN endpoint](../cdn/cdn-purge-endpoint.md).</span></span> 

## <a name="use-query-strings-tooversion-content"></a><span data-ttu-id="7f71e-198">Použít obsah tooversion řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="7f71e-198">Use query strings tooversion content</span></span>

<span data-ttu-id="7f71e-199">Hello Azure CDN nabízí následující možnosti ukládání do mezipaměti chování hello:</span><span class="sxs-lookup"><span data-stu-id="7f71e-199">hello Azure CDN offers hello following caching behavior options:</span></span>

* <span data-ttu-id="7f71e-200">Ignorovat řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="7f71e-200">Ignore query strings</span></span>
* <span data-ttu-id="7f71e-201">Nepoužívat ukládání do mezipaměti pro řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="7f71e-201">Bypass caching for query strings</span></span>
* <span data-ttu-id="7f71e-202">Ukládat do mezipaměti každou jedinečnou adresu URL</span><span class="sxs-lookup"><span data-stu-id="7f71e-202">Cache every unique URL</span></span> 

<span data-ttu-id="7f71e-203">Hello první z nich je hello výchozí, což znamená, existuje jenom jedna verze uložené v mezipaměti majetku bez ohledu na to hello řetězec dotazu v hello URL.</span><span class="sxs-lookup"><span data-stu-id="7f71e-203">hello first of these is hello default, which means there is only one cached version of an asset regardless of hello query string in hello URL.</span></span> 

<span data-ttu-id="7f71e-204">V této části kurzu hello změníte hello ukládání do mezipaměti chování toocache každou jedinečnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="7f71e-204">In this section of hello tutorial, you change hello caching behavior toocache every unique URL.</span></span>

### <a name="change-hello-cache-behavior"></a><span data-ttu-id="7f71e-205">Změna chování mezipaměti hello</span><span class="sxs-lookup"><span data-stu-id="7f71e-205">Change hello cache behavior</span></span>

<span data-ttu-id="7f71e-206">V portálu Azure hello **koncový bod CDN** vyberte **mezipaměti**.</span><span class="sxs-lookup"><span data-stu-id="7f71e-206">In hello Azure portal **CDN Endpoint** page, select **Cache**.</span></span>

<span data-ttu-id="7f71e-207">Vyberte **do mezipaměti každou jedinečnou adresu URL** z hello **chování ukládání řetězců s dotazy** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="7f71e-207">Select **Cache every unique URL** from hello **Query string caching behavior** drop-down list.</span></span>

<span data-ttu-id="7f71e-208">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7f71e-208">Select **Save**.</span></span>

![Výběr chování při ukládání řetězce dotazu do mezipaměti](media/app-service-web-tutorial-content-delivery-network/portal-select-caching-behavior.png)

### <a name="verify-that-unique-urls-are-cached-separately"></a><span data-ttu-id="7f71e-210">Ověření samostatného ukládání jedinečných adres URL do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="7f71e-210">Verify that unique URLs are cached separately</span></span>

<span data-ttu-id="7f71e-211">V prohlížeči přejděte toohello domovské stránce na koncový bod CDN hello, ale obsahovat řetězec dotazu:</span><span class="sxs-lookup"><span data-stu-id="7f71e-211">In a browser, navigate toohello home page at hello CDN endpoint, but include a query string:</span></span> 

```
http://<endpointname>.azureedge.net/index.html?q=1
```

<span data-ttu-id="7f71e-212">Hello CDN vrátí hello aktuální webové aplikace obsah, který obsahuje v záhlaví hello "V2".</span><span class="sxs-lookup"><span data-stu-id="7f71e-212">hello CDN returns hello current web app content, which includes "V2" in hello heading.</span></span> 

<span data-ttu-id="7f71e-213">tooensure, tato stránka je do mezipaměti v hello CDN, stránku hello aktualizace.</span><span class="sxs-lookup"><span data-stu-id="7f71e-213">tooensure that this page is cached in hello CDN, refresh hello page.</span></span> 

<span data-ttu-id="7f71e-214">Otevřete `index.html` a změňte "V2" příliš "V3" a změnu hello.</span><span class="sxs-lookup"><span data-stu-id="7f71e-214">Open `index.html` and change "V2" too"V3", and deploy hello change.</span></span> 

```bash
git commit -am "version 3"
git push azure master
```

<span data-ttu-id="7f71e-215">Přejděte v prohlížeči, adresa URL koncového bodu CDN toohello s novou řetězce dotazu, jako `q=2`.</span><span class="sxs-lookup"><span data-stu-id="7f71e-215">In a browser, go toohello CDN endpoint URL with a new query string such as `q=2`.</span></span> <span data-ttu-id="7f71e-216">Hello CDN získá hello aktuální `index.html` souboru a zobrazí "V3".</span><span class="sxs-lookup"><span data-stu-id="7f71e-216">hello CDN gets hello current `index.html` file and displays "V3".</span></span>  <span data-ttu-id="7f71e-217">Ale pokud přejdete koncový bod CDN toohello s hello `q=1` dotaz na řetězec, najdete v části "V2".</span><span class="sxs-lookup"><span data-stu-id="7f71e-217">But if you navigate toohello CDN endpoint with hello `q=1` query string, you see "V2".</span></span>

```
http://<endpointname>.azureedge.net/index.html?q=2
```

![V3 v nadpisu v CDN, řetězec dotazu 2](media/app-service-web-tutorial-content-delivery-network/v3-in-cdn-title-qs2.png)

```
http://<endpointname>.azureedge.net/index.html?q=1
```

![V2 v nadpisu v CDN, řetězec dotazu 1](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title-qs1.png)

<span data-ttu-id="7f71e-220">Výstup ukazuje, že každý řetězec dotazu je zpracovávat odděleně:</span><span class="sxs-lookup"><span data-stu-id="7f71e-220">This output shows that each query string is treated differently:</span></span>

* <span data-ttu-id="7f71e-221">q = 1 byl používán před, takže obsah uložený v mezipaměti, jsou vráceny (V2).</span><span class="sxs-lookup"><span data-stu-id="7f71e-221">q=1 was used before, so cached contents are returned (V2).</span></span>
* <span data-ttu-id="7f71e-222">q = 2 je novou, proto se obsah hello nejnovější webové aplikace načíst a vrátí (V3).</span><span class="sxs-lookup"><span data-stu-id="7f71e-222">q=2 is new, so hello latest web app contents are retrieved and returned (V3).</span></span>

<span data-ttu-id="7f71e-223">Další informace najdete v tématu [Řízení chování Azure CDN při ukládání řetězců dotazu do mezipaměti](../cdn/cdn-query-string.md).</span><span class="sxs-lookup"><span data-stu-id="7f71e-223">For more information, see [Control Azure CDN caching behavior with query strings](../cdn/cdn-query-string.md).</span></span>

## <a name="map-a-custom-domain-tooa-cdn-endpoint"></a><span data-ttu-id="7f71e-224">Mapa koncový bod CDN tooa vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="7f71e-224">Map a custom domain tooa CDN endpoint</span></span>

<span data-ttu-id="7f71e-225">Vaše vlastní doména tooyour koncový bod CDN budete mapovat vytvořením záznamu CNAME.</span><span class="sxs-lookup"><span data-stu-id="7f71e-225">You'll map your custom domain tooyour CDN Endpoint by creating a CNAME record.</span></span> <span data-ttu-id="7f71e-226">Záznam CNAME je funkce DNS, který se mapuje zdrojové domény tooa cílové domény.</span><span class="sxs-lookup"><span data-stu-id="7f71e-226">A CNAME record is a DNS feature that maps a source domain tooa destination domain.</span></span> <span data-ttu-id="7f71e-227">Například může mapování `cdn.contoso.com` nebo `static.contoso.com` příliš`contoso.azureedge.net`.</span><span class="sxs-lookup"><span data-stu-id="7f71e-227">For example, you might map `cdn.contoso.com` or `static.contoso.com` too`contoso.azureedge.net`.</span></span>

<span data-ttu-id="7f71e-228">Pokud nemáte vlastní doménu, zvažte následující hello [kurzu domény služby App Service](custom-dns-web-site-buydomains-web-app.md) hello toopurchase domény pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7f71e-228">If you don't have a custom domain, consider following hello [App Service domain tutorial](custom-dns-web-site-buydomains-web-app.md) toopurchase a domain using hello Azure portal.</span></span> 

### <a name="find-hello-hostname-toouse-with-hello-cname"></a><span data-ttu-id="7f71e-229">Najít název hostitele toouse hello s hello CNAME</span><span class="sxs-lookup"><span data-stu-id="7f71e-229">Find hello hostname toouse with hello CNAME</span></span>

<span data-ttu-id="7f71e-230">V portálu Azure hello **koncový bod** se přesvědčte, že **přehled** je vybraný v levé navigaci a potom vyberte hello hello **+ vlastní domény** tlačítko hello horní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="7f71e-230">In hello Azure portal **Endpoint** page, make sure **Overview** is selected in hello left navigation, and then select hello **+ Custom Domain** button at hello top of hello page.</span></span>

![Výběr možnosti Přidat vlastní doménu](media/app-service-web-tutorial-content-delivery-network/portal-select-add-domain.png)

<span data-ttu-id="7f71e-232">V hello **přidat vlastní doménu** stránky, uvidíte hello koncový bod hostitele název toouse vytvořit záznam CNAME.</span><span class="sxs-lookup"><span data-stu-id="7f71e-232">In hello **Add a custom domain** page, you see hello endpoint host name toouse in creating a CNAME record.</span></span> <span data-ttu-id="7f71e-233">název hostitele Hello je odvozený od vaše adresa URL koncového bodu CDN:  **&lt;EndpointName >. azureedge.net**.</span><span class="sxs-lookup"><span data-stu-id="7f71e-233">hello host name is derived from your CDN endpoint URL: **&lt;EndpointName>.azureedge.net**.</span></span> 

![Stránka Přidat doménu](media/app-service-web-tutorial-content-delivery-network/portal-add-domain.png)

### <a name="configure-hello-cname-with-your-domain-registrar"></a><span data-ttu-id="7f71e-235">Konfigurace hello CNAME u vašeho registrátora domény</span><span class="sxs-lookup"><span data-stu-id="7f71e-235">Configure hello CNAME with your domain registrar</span></span>

<span data-ttu-id="7f71e-236">Navigace webu tooyour doménového registrátora a vyhledejte oddíl hello pro vytvoření záznamů DNS.</span><span class="sxs-lookup"><span data-stu-id="7f71e-236">Navigate tooyour domain registrar's web site, and locate hello section for creating DNS records.</span></span> <span data-ttu-id="7f71e-237">Může to být v části, jako je **Název domény**, **DNS** nebo **Správa názvového serveru**.</span><span class="sxs-lookup"><span data-stu-id="7f71e-237">You might find this in a section such as **Domain Name**, **DNS**, or **Name Server Management**.</span></span>

<span data-ttu-id="7f71e-238">Najít oddíl hello správy záznamů CNAME.</span><span class="sxs-lookup"><span data-stu-id="7f71e-238">Find hello section for managing CNAMEs.</span></span> <span data-ttu-id="7f71e-239">Může mít toogo tooan Upřesnit nastavení stránky a vyhledejte slova hello CNAME, Alias nebo subdomény.</span><span class="sxs-lookup"><span data-stu-id="7f71e-239">You may have toogo tooan advanced settings page and look for hello words CNAME, Alias, or Subdomains.</span></span>

<span data-ttu-id="7f71e-240">Vytvořit záznam CNAME, který mapuje vaši zvolenou subdomény (například **statické** nebo **cdn**) toohello **názvu hostitele koncového bodu** uvedené výše v portálu hello.</span><span class="sxs-lookup"><span data-stu-id="7f71e-240">Create a CNAME record that maps your chosen subdomain (for example, **static** or **cdn**) toohello **Endpoint host name** shown earlier in hello portal.</span></span> 

### <a name="enter-hello-custom-domain-in-azure"></a><span data-ttu-id="7f71e-241">Zadejte vlastní domény hello v Azure</span><span class="sxs-lookup"><span data-stu-id="7f71e-241">Enter hello custom domain in Azure</span></span>

<span data-ttu-id="7f71e-242">Vrátí toohello **přidat vlastní doménu** stránky a zadejte vlastní domény, včetně hello subdomény, v dialogovém okně hello.</span><span class="sxs-lookup"><span data-stu-id="7f71e-242">Return toohello **Add a custom domain** page, and enter your custom domain, including hello subdomain, in hello dialog box.</span></span> <span data-ttu-id="7f71e-243">Zadejte například `cdn.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="7f71e-243">For example, enter `cdn.contoso.com`.</span></span>   
   
<span data-ttu-id="7f71e-244">Azure ověřuje, zda existuje záznam CNAME hello pro hello název domény, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="7f71e-244">Azure verifies that hello CNAME record exists for hello domain name you have entered.</span></span> <span data-ttu-id="7f71e-245">Pokud hello CNAME je správná, je ověřit vaši vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="7f71e-245">If hello CNAME is correct, your custom domain is validated.</span></span>

<span data-ttu-id="7f71e-246">Pro hello CNAME záznamů toopropagate tooname servery na hello Internetu může trvat dobu.</span><span class="sxs-lookup"><span data-stu-id="7f71e-246">It can take time for hello CNAME record toopropagate tooname servers on hello Internet.</span></span> <span data-ttu-id="7f71e-247">Pokud vaše doména není ověřený okamžitě, počkejte několik minut a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="7f71e-247">If your domain is not validated immediately, wait a few minutes and try again.</span></span>

### <a name="test-hello-custom-domain"></a><span data-ttu-id="7f71e-248">Test hello vlastní domény</span><span class="sxs-lookup"><span data-stu-id="7f71e-248">Test hello custom domain</span></span>

<span data-ttu-id="7f71e-249">V prohlížeči přejděte toohello `index.html` souboru používání vlastní domény (například `cdn.contoso.com/index.html`) tooverify, která je výsledkem hello text hello, stejně jako když přejdete přímo příliš`<endpointname>azureedge.net/index.html`.</span><span class="sxs-lookup"><span data-stu-id="7f71e-249">In a browser, navigate toohello `index.html` file using your custom domain (for example, `cdn.contoso.com/index.html`) tooverify that hello result is hello same as when you go directly too`<endpointname>azureedge.net/index.html`.</span></span>

![Domovská stránka ukázkové aplikace s použitím adresy URL vlastní domény](media/app-service-web-tutorial-content-delivery-network/home-page-custom-domain.png)

<span data-ttu-id="7f71e-251">Další informace najdete v tématu [vlastní doménu CDN Azure mapa obsahu tooa](../cdn/cdn-map-content-to-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="7f71e-251">For more information, see [Map Azure CDN content tooa custom domain](../cdn/cdn-map-content-to-custom-domain.md).</span></span>

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="7f71e-252">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7f71e-252">Next steps</span></span>

<span data-ttu-id="7f71e-253">Co jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="7f71e-253">What you learned:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7f71e-254">Vytvořit koncový bod CDN.</span><span class="sxs-lookup"><span data-stu-id="7f71e-254">Create a CDN endpoint.</span></span>
> * <span data-ttu-id="7f71e-255">Aktualizovat prostředky uložené v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="7f71e-255">Refresh cached assets.</span></span>
> * <span data-ttu-id="7f71e-256">Použití dotazu řetězce verze toocontrol do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="7f71e-256">Use query strings toocontrol cached versions.</span></span>
> * <span data-ttu-id="7f71e-257">Použijte vlastní doménu pro koncový bod CDN hello.</span><span class="sxs-lookup"><span data-stu-id="7f71e-257">Use a custom domain for hello CDN endpoint.</span></span>

<span data-ttu-id="7f71e-258">Zjistěte, jak toooptimize CDN výkon v hello následující články:</span><span class="sxs-lookup"><span data-stu-id="7f71e-258">Learn how toooptimize CDN performance in hello following articles:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="7f71e-259">Vylepšení výkonu prostřednictvím komprimace souborů v Azure CDN</span><span class="sxs-lookup"><span data-stu-id="7f71e-259">Improve performance by compressing files in Azure CDN</span></span>](../cdn/cdn-improve-performance.md)

> [!div class="nextstepaction"]
> [<span data-ttu-id="7f71e-260">Předběžné načtení prostředků v koncovém bodu Azure CDN</span><span class="sxs-lookup"><span data-stu-id="7f71e-260">Pre-load assets on an Azure CDN endpoint</span></span>](../cdn/cdn-preload-endpoint.md)
