---
title: "App Service API Apps – co se změnilo | Microsoft Docs"
description: "Zjistěte, co je nového pro API Apps v Azure App Service."
services: app-service\api
documentationcenter: .net
author: mohitsriv
manager: erikre
editor: tdykstra
ms.assetid: a9b58066-e8fd-48b8-a651-4613b1736433
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2016
ms.author: rachelap
ms.openlocfilehash: e4e25f2cd1d39bb0113e3fe2bc37120f92227b28
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="app-service-api-apps---whats-changed"></a><span data-ttu-id="78483-103">App Service API Apps – co se změnilo</span><span class="sxs-lookup"><span data-stu-id="78483-103">App Service API Apps - What's changed</span></span>
<span data-ttu-id="78483-104">Na událost Connect() v listopadu 2015, byly množství vylepšení do služby Azure App Service [oznámeno](https://azure.microsoft.com/blog/azure-app-service-updates-november-2015/).</span><span class="sxs-lookup"><span data-stu-id="78483-104">At the Connect() event in November 2015, a number of improvements to Azure App Service were [announced](https://azure.microsoft.com/blog/azure-app-service-updates-november-2015/).</span></span> <span data-ttu-id="78483-105">Tato vylepšení zahrnují základní změny aplikace API lépe zarovnat s Mobile a webové aplikace, snížit počet koncept a zlepšit nasazení a výkon modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="78483-105">These improvements include underlying changes to API Apps to better align with Mobile and Web Apps, reduce concept count and improve deployment and runtime performance.</span></span> <span data-ttu-id="78483-106">Spuštění nové aplikace API 30. listopadu 2015, můžete vytvořit pomocí portálu správy Azure nebo nejnovější nástrojů se projeví tyto změny.</span><span class="sxs-lookup"><span data-stu-id="78483-106">Starting November 30, 2015, new API apps you create using the Azure management portal or the latest tooling will reflect these changes.</span></span> <span data-ttu-id="78483-107">Tento článek popisuje tyto změny a také jak znovu nasadit existující aplikace, které chcete využít výhod funkcí.</span><span class="sxs-lookup"><span data-stu-id="78483-107">This article describes these changes, as well as how to redeploy existing apps to take advantage of the capabilities.</span></span>

## <a name="feature-changes"></a><span data-ttu-id="78483-108">Funkce změny</span><span class="sxs-lookup"><span data-stu-id="78483-108">Feature changes</span></span>
<span data-ttu-id="78483-109">Klíčové funkce API Apps – ověřování, CORS a rozhraní API metadat – přesunuli přímo do služby App Service.</span><span class="sxs-lookup"><span data-stu-id="78483-109">The key features of API Apps – authentication, CORS and API metadata – have moved directly into App Service.</span></span> <span data-ttu-id="78483-110">Díky této změně funkce jsou dostupné přes webové, mobilní aplikace a aplikace API.</span><span class="sxs-lookup"><span data-stu-id="78483-110">With this change, the features are available across Web, Mobile and API Apps.</span></span> <span data-ttu-id="78483-111">Ve skutečnosti všechny tři sdílet stejný **Microsoft.Web/sites** typ prostředku ve službě Správce prostředků.</span><span class="sxs-lookup"><span data-stu-id="78483-111">In fact, all three share the same **Microsoft.Web/sites** resource type in Resource Manager.</span></span> <span data-ttu-id="78483-112">Brána aplikace API je už potřeby nebo nabízí s API Apps.</span><span class="sxs-lookup"><span data-stu-id="78483-112">The API Apps gateway is no longer needed or offered with API Apps.</span></span> <span data-ttu-id="78483-113">To také usnadňuje používání služby Azure API Management vzhledem k tomu, že budou existovat jenom jedné API Management gateway.</span><span class="sxs-lookup"><span data-stu-id="78483-113">This also makes it easier to use Azure API Management since there will be just the single API Management gateway.</span></span>

![Přehled aplikace API](./media/app-service-api-whats-changed/api-apps-overview.png)

<span data-ttu-id="78483-115">Princip klíčových návrhu s API Apps aktualizací je umožnit použití rozhraní API je, ve vámi zvolený jazyk.</span><span class="sxs-lookup"><span data-stu-id="78483-115">A key design principle with the API Apps update is to enable you to bring your API as is, in your language of choice.</span></span>  <span data-ttu-id="78483-116">Pokud vaše rozhraní API je už nasazená jako webovou aplikaci nebo mobilní aplikace, nemáte se znovu nasadit aplikaci, kterou chcete využít výhod nových funkcí.</span><span class="sxs-lookup"><span data-stu-id="78483-116">If your API is already deployed as a Web App or Mobile App, you do not have to redeploy your app to take advantage of the new features.</span></span> <span data-ttu-id="78483-117">Pokud jste aktuálně ve verzi preview rozhraní API aplikace, pokyny migrace je podrobně popsán níže.</span><span class="sxs-lookup"><span data-stu-id="78483-117">If you are currently on API Apps preview, migration guidance is detailed below.</span></span>

### <a name="authentication"></a><span data-ttu-id="78483-118">Authentication</span><span class="sxs-lookup"><span data-stu-id="78483-118">Authentication</span></span>
<span data-ttu-id="78483-119">Existující připraveného aplikace API, mobilní služby nebo aplikace a webové aplikace ověřování funkce mají byla unified a jsou k dispozici v jednom okně ověřování služby Azure App Service v portálu pro správu.</span><span class="sxs-lookup"><span data-stu-id="78483-119">The existing turnkey API Apps, Mobile Services/Apps and Web Apps authentication features have been unified and are available in a single Azure App Service authentication blade in the management portal.</span></span> <span data-ttu-id="78483-120">Úvod do služby ověřování v App Service, najdete v části [rozšiřování App Service ověřování / autorizace](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).</span><span class="sxs-lookup"><span data-stu-id="78483-120">For an introduction to authentication services in App Service, see [Expanding App Service authentication / authorization](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).</span></span>

<span data-ttu-id="78483-121">Pro scénáře rozhraní API existuje několik relevantní nové funkce:</span><span class="sxs-lookup"><span data-stu-id="78483-121">For API scenarios, there are a number of relevant new capabilities:</span></span>

* <span data-ttu-id="78483-122">**Podpora pro použití služby Azure Active Directory přímo**, bez nutnosti exchange token AAD pro token relace kód klienta: vašeho klienta zahrnout pouze tokeny AAD v hlavičce autorizace podle specifikace tokenu nosiče.</span><span class="sxs-lookup"><span data-stu-id="78483-122">**Support for using Azure Active Directory directly**, without client code having to exchange the AAD token for a session token: Your client can just include the AAD tokens in the Authorization header, according to the bearer token specification.</span></span> <span data-ttu-id="78483-123">Také to znamená, že žádné specifické pro aplikaci služby SDK je potřeba na straně klienta nebo serveru.</span><span class="sxs-lookup"><span data-stu-id="78483-123">This also means no App Service-specific SDK is required on the client or server side.</span></span> 
* <span data-ttu-id="78483-124">**Přístup k Service-to-service nebo "Interní"**: Pokud máte démona nebo jiného klienta potřebovali mít přístup k rozhraní API bez rozhraní, si můžete vyžádat token pomocí AAD instanční objekt a předejte ji do služby App Service pro ověřování pomocí vašeho aplikace.</span><span class="sxs-lookup"><span data-stu-id="78483-124">**Service-to-service or "Internal" access**: If you have a daemon process or some other client needing access to APIs without an interface, you can request a token using an AAD service principal and pass it to App Service for authentication with your application.</span></span>
* <span data-ttu-id="78483-125">**Odložení autorizace**: mnoho aplikací mít různou omezení přístupu pro různé části aplikace.</span><span class="sxs-lookup"><span data-stu-id="78483-125">**Deferred authorization**: Many applications have varying access restrictions for different parts of the application.</span></span> <span data-ttu-id="78483-126">Možná budete chtít některé rozhraní API je veřejně dostupné, zatímco jiné vyžadují přihlášení.</span><span class="sxs-lookup"><span data-stu-id="78483-126">Perhaps you want some APIs to be publicly available, while others require sign-in.</span></span> <span data-ttu-id="78483-127">Původní funkce ověřování/autorizace se vyjadřuje, s celý web, které vyžadují přihlášení.</span><span class="sxs-lookup"><span data-stu-id="78483-127">The original Authentication/Authorization feature was all-or-nothing, with the whole site requiring login.</span></span> <span data-ttu-id="78483-128">Tato možnost stále existuje, ale můžete případně povolit kódu aplikace k vykreslení rozhodnutí přístup po ověření uživatele služby App Service.</span><span class="sxs-lookup"><span data-stu-id="78483-128">This option still exists, but you can alternatively allow your application code to render access decisions after App Service has authenticated the user.</span></span>

<span data-ttu-id="78483-129">Další informace o nových funkcích ověřování najdete v tématu [ověřování a autorizace API Apps v Azure App Service](app-service-api-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="78483-129">For more information about the new authentication features, see [Authentication and authorization for API Apps in Azure App Service](app-service-api-authentication.md).</span></span> <span data-ttu-id="78483-130">Informace o tom, jak migrovat stávající aplikace API z předchozí modelu aplikace API k novým najdete v tématu [migraci stávající aplikace API](#migrating-existing-api-apps) dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="78483-130">For information about how to migrate existing API apps from the previous API apps model to the new one, see [Migrating existing API apps](#migrating-existing-api-apps) later in this article.</span></span>

### <a name="cors"></a><span data-ttu-id="78483-131">CORS</span><span class="sxs-lookup"><span data-stu-id="78483-131">CORS</span></span>
<span data-ttu-id="78483-132">Místo oddělený čárkami **MS_CrossDomainOrigins** aplikace nastavení, je nyní okno Konfigurace CORS na portálu správy Azure.</span><span class="sxs-lookup"><span data-stu-id="78483-132">Instead of a comma-delimited **MS_CrossDomainOrigins** app setting, there is now a blade in the Azure management portal for configuring CORS.</span></span> <span data-ttu-id="78483-133">Alternativně lze konfigurovat, pomocí nástrojů, jako je Azure PowerShell, rozhraní příkazového řádku správce prostředků nebo [Průzkumníka prostředků](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="78483-133">Alternatively, it can be configured using Resource Manager tooling such as Azure PowerShell, CLI or [Resource Explorer](https://resources.azure.com/).</span></span> <span data-ttu-id="78483-134">Nastavte **cors** vlastnost **Microsoft.Web/sites/config** typ prostředku pro vaše  **&lt;název lokality&gt;/web** prostředků.</span><span class="sxs-lookup"><span data-stu-id="78483-134">Set the **cors** property on the **Microsoft.Web/sites/config** resource type for your **&lt;site name&gt;/web** resource.</span></span> <span data-ttu-id="78483-135">Například:</span><span class="sxs-lookup"><span data-stu-id="78483-135">For example:</span></span>

    {
        "cors": {
            "allowedOrigins": [
                "https://localhost:44300"
            ]
        }
    } 

### <a name="api-metadata"></a><span data-ttu-id="78483-136">Metadata API</span><span class="sxs-lookup"><span data-stu-id="78483-136">API metadata</span></span>
<span data-ttu-id="78483-137">V okně Definice rozhraní API je nyní k dispozici přes webové, mobilní aplikace a aplikace API.</span><span class="sxs-lookup"><span data-stu-id="78483-137">The API definition blade is now available across Web, Mobile and API Apps.</span></span> <span data-ttu-id="78483-138">V portálu pro správu můžete zadat adresu url relativní nebo absolutní adresu url ukazující na koncový bod této reprezentace hostitelů Swagger 2.0 rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="78483-138">In the management portal, you can specify either a relative url or an absolute url pointing to an endpoint that hosts a Swagger 2.0 representation of your API.</span></span> <span data-ttu-id="78483-139">Alternativně můžete konfigurovat, pomocí nástroje Správce prostředků.</span><span class="sxs-lookup"><span data-stu-id="78483-139">Alternatively, it can be configured using Resource Manager tooling.</span></span> <span data-ttu-id="78483-140">Nastavte **apiDefinition** vlastnost **Microsoft.Web/sites/config** typ prostředku pro vaše  **&lt;název lokality&gt;/web** prostředků.</span><span class="sxs-lookup"><span data-stu-id="78483-140">Set the **apiDefinition** property on the **Microsoft.Web/sites/config** resource type for your **&lt;site name&gt;/web** resource.</span></span> <span data-ttu-id="78483-141">Například:</span><span class="sxs-lookup"><span data-stu-id="78483-141">For example:</span></span>

    {
        "apiDefinition":
        {
            "url": "https://myStorageAccount.blob.core.windows.net/swagger/apiDefinition.json"
        }
    }

<span data-ttu-id="78483-142">V tomto okamžiku musí být veřejně přístupná bez ověřování pro mnoho podřízené klientů (například Visual Studio REST API generování klienta a PowerApps "Přidat rozhraní API" toku) ho využívají koncový bod metadat.</span><span class="sxs-lookup"><span data-stu-id="78483-142">At this time, the metadata endpoint needs to be publicly accessible without authentication for many downstream clients (e.g. Visual Studio REST API client generation and PowerApps "Add API" flow) to consume it.</span></span> <span data-ttu-id="78483-143">To znamená, že pokud používáte ověřování aplikace služby a chcete vystavit definice rozhraní API z v rámci vaší aplikace, budete muset použít možnost ověřování odložení dříve popisované, aby trasy, která má Swagger metadata veřejné.</span><span class="sxs-lookup"><span data-stu-id="78483-143">This does mean if you are using App Service authentication and want to expose the API definition from within your app itself, you will need to use the Deferred Authentication option described earlier so that the route to your Swagger metadata is public.</span></span>

## <a name="management-portal"></a><span data-ttu-id="78483-144">Portál pro správu</span><span class="sxs-lookup"><span data-stu-id="78483-144">Management Portal</span></span>
<span data-ttu-id="78483-145">Výběr **nové > Web + mobilní > aplikace API** portálu vytvoří aplikace API, které odráží nové funkce, které jsou popsané v článku.</span><span class="sxs-lookup"><span data-stu-id="78483-145">Selecting **New > Web + Mobile > API App** in the portal will create API apps that reflect the new capabilities described in the article.</span></span> <span data-ttu-id="78483-146">**Procházet > aplikace API** zobrazí pouze tyto nové aplikace API.</span><span class="sxs-lookup"><span data-stu-id="78483-146">**Browse > API Apps** will only show these new API apps.</span></span> <span data-ttu-id="78483-147">Když přejdete do aplikace API, v okně sdílí stejné rozložení a možnosti jako webové a mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="78483-147">Once you browse into an API app, the blade shares the same layout and capabilities as those of Web and Mobile Apps.</span></span> <span data-ttu-id="78483-148">Pouze rozdíly jsou rychlý start obsah a nastavení řazení.</span><span class="sxs-lookup"><span data-stu-id="78483-148">The only differences are quickstart content and ordering of settings.</span></span>

<span data-ttu-id="78483-149">Stávající aplikace API (nebo aplikace Marketplace API vytvořená z Logic Apps) s předchozí verzí Preview možnosti stále budou viditelné v Návrháři Logic Apps a při procházení všechny prostředky ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="78483-149">Existing API apps (or Marketplace API apps created from Logic Apps) with the previous Preview capabilities will still be visible in the Logic Apps designer and when browsing all resources in a resource group.</span></span>

## <a name="visual-studio"></a><span data-ttu-id="78483-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="78483-150">Visual Studio</span></span>
<span data-ttu-id="78483-151">Většina nástrojů webové aplikace bude fungovat s nové aplikace API, protože budou sdílet stejnou základní **Microsoft.Web/sites** typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="78483-151">Most Web Apps tooling will work with new API apps since they share the same underlying **Microsoft.Web/sites** resource type.</span></span> <span data-ttu-id="78483-152">Visual Studio Azure nástrojů, ale musí být neupgradovali na verzi 2.8.1 nebo novější vzhledem k tomu, že zpřístupňuje řadu funkcí, které jsou specifické pro rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="78483-152">The Azure Visual Studio tooling, however, should be upgraded to version 2.8.1 or later since it exposes a number of capabilities specific to APIs.</span></span> <span data-ttu-id="78483-153">Stažení sady SDK z [stránky Azure stahování](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="78483-153">Download the SDK from the [Azure downloads page](https://azure.microsoft.com/downloads/).</span></span>

<span data-ttu-id="78483-154">S racionalizace typů služby App Service, publikování je také jednotná pod **publikovat > Microsoft Azure App Service**:</span><span class="sxs-lookup"><span data-stu-id="78483-154">With the rationalization of the App Service types, publish is also unified under **Publish > Microsoft Azure App Service**:</span></span>

![Publikování aplikace API](./media/app-service-api-whats-changed/api-apps-publish.png)

<span data-ttu-id="78483-156">Další informace o SDK 2.8.1, přečtěte si téma oznámení [příspěvku na blogu](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/).</span><span class="sxs-lookup"><span data-stu-id="78483-156">To learn more about SDK 2.8.1, read the announcement [blog post](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/).</span></span>

<span data-ttu-id="78483-157">Alternativně lze ručně importovat profil publikování z portálu pro správu umožňující publikování.</span><span class="sxs-lookup"><span data-stu-id="78483-157">Alternatively, you can manually import the publish profile from the management portal to enable publish.</span></span> <span data-ttu-id="78483-158">Ale Průzkumník cloudu, generování kódu a vytváření výběr aplikace API bude vyžadovat SDK 2.8.1 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="78483-158">However, Cloud Explorer, code generation and API app selection/creation will require SDK 2.8.1 or higher.</span></span>

## <a name="migrating-existing-api-apps"></a><span data-ttu-id="78483-159">Migraci stávající aplikace API</span><span class="sxs-lookup"><span data-stu-id="78483-159">Migrating existing API apps</span></span>
<span data-ttu-id="78483-160">Pokud nasadíte vlastního rozhraní API pro aplikace API v předchozí verzi Preview, žádosti o migrovat do nového modelu pro aplikace API pomocí 31. prosince 2015.</span><span class="sxs-lookup"><span data-stu-id="78483-160">If your custom API is deployed to the previous Preview version of API Apps, we request that you migrate to the new model for API Apps by December 31, 2015.</span></span> <span data-ttu-id="78483-161">Vzhledem k tomu, jak starý a nový model jsou založená na webové rozhraní API hostovaných ve službě App Service, můžete znovu použít většinu existujícího kódu.</span><span class="sxs-lookup"><span data-stu-id="78483-161">Since both the old and new model are based on Web APIs hosted in App Service, the majority of existing code can be reused.</span></span>

### <a name="hosting-and-redeployment"></a><span data-ttu-id="78483-162">Hostování a opětovné nasazení</span><span class="sxs-lookup"><span data-stu-id="78483-162">Hosting and redeployment</span></span>
<span data-ttu-id="78483-163">Postup pro opakované nasazení jsou stejné jako nasazení všechny existující webového rozhraní API do služby App Service.</span><span class="sxs-lookup"><span data-stu-id="78483-163">The steps for redeploying are the same as deploying any existing Web API to App Service.</span></span> <span data-ttu-id="78483-164">pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="78483-164">Steps:</span></span>

1. <span data-ttu-id="78483-165">Vytvoření prázdné aplikace API.</span><span class="sxs-lookup"><span data-stu-id="78483-165">Create an empty API app.</span></span> <span data-ttu-id="78483-166">To lze provést na portálu se nový > aplikace API v sadě Visual Studio z publikovat nebo z nástroje Správce prostředků.</span><span class="sxs-lookup"><span data-stu-id="78483-166">This can be done in the portal with New > API App, in Visual Studio from publish, or from Resource Manager tooling.</span></span> <span data-ttu-id="78483-167">Pokud pomocí nástroje Správce prostředků nebo šablony, nastavte **druhu** hodnotu **rozhraní api** na **Microsoft.Web/sites** typ prostředku – elementy QuickStart a nastavení v portál pro správu orientovat na rozhraní API scénáře.</span><span class="sxs-lookup"><span data-stu-id="78483-167">If using Resource Manager tooling or templates, set the **kind** value to **api** on the **Microsoft.Web/sites** resource type to have the quickstarts and settings in the management portal oriented towards API scenarios.</span></span>
2. <span data-ttu-id="78483-168">Připojte a nasazení projektu do prázdné aplikace API pomocí kteréhokoli z nasazení mechanismů podporované službou App Service.</span><span class="sxs-lookup"><span data-stu-id="78483-168">Connect and deploy your project to the empty API app using any of the deployment mechanisms supported by App Service.</span></span> <span data-ttu-id="78483-169">Čtení [dokumentaci k nasazení služby Azure App Service](../app-service-web/web-sites-deploy.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="78483-169">Read [Azure App Service deployment documentation](../app-service-web/web-sites-deploy.md) to learn more.</span></span> 

### <a name="authentication"></a><span data-ttu-id="78483-170">Authentication</span><span class="sxs-lookup"><span data-stu-id="78483-170">Authentication</span></span>
<span data-ttu-id="78483-171">Ověřovací služby App Service podporují stejné možnosti, které byly dostupné s předchozím modelu aplikace API.</span><span class="sxs-lookup"><span data-stu-id="78483-171">The App Service authentication services support the same capabilities that were available with the previous API Apps model.</span></span> <span data-ttu-id="78483-172">Pokud používáte tokeny relace a vyžadují sady SDK, pomocí následujících klientských a serverových sad SDK:</span><span class="sxs-lookup"><span data-stu-id="78483-172">If you are using session tokens and require SDKs, use the following client and server SDKs:</span></span>

* <span data-ttu-id="78483-173">Klient: [mobilního klienta Azure SDK](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)</span><span class="sxs-lookup"><span data-stu-id="78483-173">Client: [Azure Mobile Client SDK](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)</span></span>
* <span data-ttu-id="78483-174">Server: [rozšíření ověřování .NET mobilní aplikace Microsoft Azure](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/)</span><span class="sxs-lookup"><span data-stu-id="78483-174">Server: [Microsoft Azure Mobile App .NET Authentication Extension](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/)</span></span> 

<span data-ttu-id="78483-175">Pokud jste místo toho použili alpha aplikační služby sady SDK, ty jsou nyní zastaralé:</span><span class="sxs-lookup"><span data-stu-id="78483-175">If you were instead using the App Service alpha SDKs, these are now deprecated:</span></span>

* <span data-ttu-id="78483-176">Klient: [SDK Microsoft Azure App Service](http://www.nuget.org/packages/Microsoft.Azure.AppService)</span><span class="sxs-lookup"><span data-stu-id="78483-176">Client: [Microsoft Azure AppService SDK](http://www.nuget.org/packages/Microsoft.Azure.AppService)</span></span>
* <span data-ttu-id="78483-177">Server: [Microsoft.Azure.AppService.ApiApps.Service](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service)</span><span class="sxs-lookup"><span data-stu-id="78483-177">Server: [Microsoft.Azure.AppService.ApiApps.Service](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service)</span></span>

<span data-ttu-id="78483-178">Zejména s Azure Active Directory, ale žádné služby specifické pro aplikace je požadované Pokud používáte AAD token přímo.</span><span class="sxs-lookup"><span data-stu-id="78483-178">In particular with Azure Active Directory, however, no App Service-specific is required if you are using the AAD token directly.</span></span>

### <a name="internal-access"></a><span data-ttu-id="78483-179">Interní přístup</span><span class="sxs-lookup"><span data-stu-id="78483-179">Internal access</span></span>
<span data-ttu-id="78483-180">Předchozí modelu aplikace API zahrnuty úroveň integrovanou interní přístupu.</span><span class="sxs-lookup"><span data-stu-id="78483-180">The previous API Apps model included a built-in internal access level.</span></span> <span data-ttu-id="78483-181">To vyžaduje použití sady SDK pro žádosti o podepsání.</span><span class="sxs-lookup"><span data-stu-id="78483-181">This required use of the SDK for signing requests.</span></span> <span data-ttu-id="78483-182">Jak je popsáno výše, s modelem nové aplikace API objekty služby AAD slouží jako náhradní pro ověřování služby služby bez nutnosti aplikaci specifickou pro službu SDK.</span><span class="sxs-lookup"><span data-stu-id="78483-182">As described earlier, with the new API Apps model, AAD service principals can be used as an alternate for service-to-service authentication without requiring an App Service-specific SDK.</span></span> <span data-ttu-id="78483-183">Další informace v [objekt zabezpečení ověřování služby pro aplikace API v Azure App Service](app-service-api-dotnet-service-principal-auth.md).</span><span class="sxs-lookup"><span data-stu-id="78483-183">Learn more in [Service principal authentication for API Apps in Azure App Service](app-service-api-dotnet-service-principal-auth.md).</span></span>

### <a name="discovery"></a><span data-ttu-id="78483-184">Zjišťování</span><span class="sxs-lookup"><span data-stu-id="78483-184">Discovery</span></span>
<span data-ttu-id="78483-185">Předchozí aplikace API modelu měli rozhraní API pro zjišťování dalších aplikace API v době běhu ve stejné skupině prostředků za stejnou bránou.</span><span class="sxs-lookup"><span data-stu-id="78483-185">The previous API Apps model had APIs for discovering other API apps at runtime in the same resource group behind the same gateway.</span></span> <span data-ttu-id="78483-186">Toto je užitečné zejména v případě architektur, které implementují mikroslužbu vzory.</span><span class="sxs-lookup"><span data-stu-id="78483-186">This is especially useful in architectures that implement microservice patterns.</span></span> <span data-ttu-id="78483-187">Když to není podporováno přímo, jsou k dispozici několik možností:</span><span class="sxs-lookup"><span data-stu-id="78483-187">While this is not directly supported, a number of options are available:</span></span>

1. <span data-ttu-id="78483-188">Použijte rozhraní API Správce prostředků Azure pro zjišťování.</span><span class="sxs-lookup"><span data-stu-id="78483-188">Use the Azure Resource Manager API's for discovery.</span></span>
2. <span data-ttu-id="78483-189">Uveďte Azure API Management před vaše rozhraní API hostované služby App Service.</span><span class="sxs-lookup"><span data-stu-id="78483-189">Put Azure API Management in front of your App Service-hosted APIs.</span></span> <span data-ttu-id="78483-190">Azure API Management slouží jako průčelí za a můžete zadat stabilní externí přístupných adresu url, i když se změní je interní topologie.</span><span class="sxs-lookup"><span data-stu-id="78483-190">Azure API Management serves as a facade and can provide a stable external facing url even if you internal topology changes.</span></span>
3. <span data-ttu-id="78483-191">Sestavení vlastní aplikace API zjišťování a další aplikace API zaregistrovat zjišťování aplikace po spuštění.</span><span class="sxs-lookup"><span data-stu-id="78483-191">Build your own discovery API app and have other API apps register with the discovery app on startup.</span></span>
4. <span data-ttu-id="78483-192">V době nasazení naplnění koncových bodů API Apps nastavení aplikace všechny aplikace API (a klienty).</span><span class="sxs-lookup"><span data-stu-id="78483-192">At deployment time, populate the app settings of all the API apps (and clients) with the endpoints of the other API apps.</span></span> <span data-ttu-id="78483-193">Toto je přijatelná šablony nasazení a vzhledem k tomu, že aplikace API nyní získáte ovládací prvek adresy URL.</span><span class="sxs-lookup"><span data-stu-id="78483-193">This is viable in template deployments and since API Apps now give you control of the url.</span></span>

## <a name="using-api-apps-with-logic-apps"></a><span data-ttu-id="78483-194">Pomocí aplikace API s Logic Apps</span><span class="sxs-lookup"><span data-stu-id="78483-194">Using API Apps with Logic Apps</span></span>
<span data-ttu-id="78483-195">Nový model aplikace API pracuje s [aplikací logiky verze schématu 2015-08-01](../logic-apps/logic-apps-schema-2015-08-01.md).</span><span class="sxs-lookup"><span data-stu-id="78483-195">The new API apps model works well with [Logic Apps schema version 2015-08-01](../logic-apps/logic-apps-schema-2015-08-01.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="78483-196">Další kroky</span><span class="sxs-lookup"><span data-stu-id="78483-196">Next Steps</span></span>
<span data-ttu-id="78483-197">Další informace, přečtěte si články v [části dokumentace k API Apps](https://azure.microsoft.com/documentation/services/app-service/api/).</span><span class="sxs-lookup"><span data-stu-id="78483-197">To learn more, read the articles in the [API Apps Documentation section](https://azure.microsoft.com/documentation/services/app-service/api/).</span></span> <span data-ttu-id="78483-198">Byly aktualizovány tak, aby odrážela nový model pro aplikace API.</span><span class="sxs-lookup"><span data-stu-id="78483-198">They have been updated to reflect the new model for API Apps.</span></span> <span data-ttu-id="78483-199">Kromě toho oslovení ve fórech další podrobnosti a pokyny k migraci:</span><span class="sxs-lookup"><span data-stu-id="78483-199">In addition, do reach out on the forums for additional details or guidance on migration:</span></span>

* [<span data-ttu-id="78483-200">Fórum MSDN</span><span class="sxs-lookup"><span data-stu-id="78483-200">MSDN forum</span></span>](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)
* [<span data-ttu-id="78483-201">Stack Overflow</span><span class="sxs-lookup"><span data-stu-id="78483-201">Stack Overflow</span></span>](http://stackoverflow.com/questions/tagged/azure-api-apps)
