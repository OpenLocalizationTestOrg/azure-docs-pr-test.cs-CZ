---
title: "Ověření s Mobile Engagementem rozhraní REST API"
description: "Popisuje, jak ověřit pomocí rozhraní API REST služby Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: da82cb36-957a-4e19-a805-b44733cf6597
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 10/05/2016
ms.author: wesmc;ricksal
ms.openlocfilehash: b05181d9252c0a804648e01b4058019278ae5abe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="authenticate-with-mobile-engagement-rest-apis"></a><span data-ttu-id="13f8e-103">Ověření s Mobile Engagementem rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="13f8e-103">Authenticate with Mobile Engagement REST APIs</span></span>
## <a name="overview"></a><span data-ttu-id="13f8e-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="13f8e-104">Overview</span></span>
<span data-ttu-id="13f8e-105">Tento dokument popisuje, jak se získat token k ověření s rozhraními API sady Mobile Engagement REST platný Oauth AAD.</span><span class="sxs-lookup"><span data-stu-id="13f8e-105">This document describes how to get a valid AAD Oauth token to authenticate with the Mobile Engagement REST APIs.</span></span> 

<span data-ttu-id="13f8e-106">Předpokládá se, že máte platné předplatné Azure a jste vytvořili aplikaci Mobile Engagement, pomocí jednoho z našich [vývojáře kurzy](mobile-engagement-windows-store-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="13f8e-106">It is assumed that you have a valid Azure subscription and you have created a Mobile Engagement app using one of our [Developer Tutorials](mobile-engagement-windows-store-dotnet-get-started.md).</span></span>

## <a name="authentication"></a><span data-ttu-id="13f8e-107">Authentication</span><span class="sxs-lookup"><span data-stu-id="13f8e-107">Authentication</span></span>
<span data-ttu-id="13f8e-108">Microsoft Azure Active Directory na základě tokenu se používá k ověřování OAuth.</span><span class="sxs-lookup"><span data-stu-id="13f8e-108">A Microsoft Azure Active Directory based OAuth token is used for authentication.</span></span> 

<span data-ttu-id="13f8e-109">Aby žádosti o ověření rozhraní API musí hlavičku autorizace přidány do každého požadavku, která je v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="13f8e-109">In order to authentication an API request, an authorization header must be added to every request which is of the following form:</span></span>

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

> [!NOTE]
> <span data-ttu-id="13f8e-110">Azure Active Directory tokeny vyprší za 1 hodinu.</span><span class="sxs-lookup"><span data-stu-id="13f8e-110">Azure Active Directory tokens expire in 1 hour.</span></span>
> 
> 

<span data-ttu-id="13f8e-111">Existuje několik způsobů, jak získat token.</span><span class="sxs-lookup"><span data-stu-id="13f8e-111">There are several ways to get a token.</span></span> <span data-ttu-id="13f8e-112">Vzhledem k tomu, že rozhraní API se obvykle nazývají z cloudové služby, kterou chcete použít klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="13f8e-112">Since the APIs are generally called from a cloud service, you want to use an API key.</span></span> <span data-ttu-id="13f8e-113">Klíč rozhraní API v Azure terminologii nazývá hlavní heslo služby.</span><span class="sxs-lookup"><span data-stu-id="13f8e-113">An API key in Azure terminology is called a Service principal password.</span></span> <span data-ttu-id="13f8e-114">Následující postup popisuje jeden způsob, jak ručně nastavení.</span><span class="sxs-lookup"><span data-stu-id="13f8e-114">The following procedure describes one way to setting it up manually.</span></span>

### <a name="one-time-setup-using-script"></a><span data-ttu-id="13f8e-115">Jednorázové instalace (pomocí skriptu.)</span><span class="sxs-lookup"><span data-stu-id="13f8e-115">One-time setup (using script)</span></span>
<span data-ttu-id="13f8e-116">Postupujte podle sadu pokynů k provedení instalace pomocí skriptu prostředí PowerShell, který přebírá minimální hodnota času pro instalaci, ale používá většina povolenou výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="13f8e-116">You should follow the set of instructions below to perform the setup using a PowerShell script which takes the minimum time for setup but uses the most permissible defaults.</span></span> <span data-ttu-id="13f8e-117">Volitelně můžete také podle pokynů [ruční instalaci](mobile-engagement-api-authentication-manual.md) tohoto postupu z portálu Azure přímo a proveďte jemnějšího konfigurace.</span><span class="sxs-lookup"><span data-stu-id="13f8e-117">Optionally, you can also follow the instructions in the [manual setup](mobile-engagement-api-authentication-manual.md) for doing this from the Azure portal directly and do finer configuration.</span></span> 

1. <span data-ttu-id="13f8e-118">Získat nejnovější verzi prostředí Azure PowerShell z [zde](http://aka.ms/webpi-azps).</span><span class="sxs-lookup"><span data-stu-id="13f8e-118">Get the latest version of Azure PowerShell from [here](http://aka.ms/webpi-azps).</span></span> <span data-ttu-id="13f8e-119">Další informace o pokyny ke stažení, uvidíte to [odkaz](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="13f8e-119">For more information on the download instructions, you can see this [link](/powershell/azure/overview).</span></span>  
2. <span data-ttu-id="13f8e-120">Po instalaci prostředí Azure PowerShell, použijte následující příkazy k zajištění, že máte **Azure modulu** nainstalován:</span><span class="sxs-lookup"><span data-stu-id="13f8e-120">Once Azure PowerShell is installed, use the following commands to ensure that you have the **Azure module** installed:</span></span>
   
    <span data-ttu-id="13f8e-121">a.</span><span class="sxs-lookup"><span data-stu-id="13f8e-121">a.</span></span> <span data-ttu-id="13f8e-122">Zkontrolujte, zda že je k dispozici v seznamu dostupných modulů modulu Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="13f8e-122">Make sure the Azure PowerShell module is available in the list of available modules.</span></span> 
   
        Get-Module –ListAvailable 
   
    ![K dispozici moduly Azure][1]
   
    <span data-ttu-id="13f8e-124">b.</span><span class="sxs-lookup"><span data-stu-id="13f8e-124">b.</span></span> <span data-ttu-id="13f8e-125">Pokud v seznamu nenajdete modulu Azure PowerShell budete muset spustit následující:</span><span class="sxs-lookup"><span data-stu-id="13f8e-125">If you do not find the Azure PowerShell module in the above list then you need to run the following:</span></span>
   
        Import-Module Azure 
3. <span data-ttu-id="13f8e-126">Přihlášení k Azure Resource Manager z prostředí PowerShell spuštěním následujícího příkazu a poskytuje uživatelské jméno a heslo pro účet Azure:</span><span class="sxs-lookup"><span data-stu-id="13f8e-126">Login to the Azure Resource Manager from PowerShell by running the following command and providing your user name and password for your Azure account:</span></span> 
   
        Login-AzureRmAccount
4. <span data-ttu-id="13f8e-127">Pokud máte více předplatných, pak byste měli spustit následující:</span><span class="sxs-lookup"><span data-stu-id="13f8e-127">If you have multiple subscriptions then you should run the following:</span></span>
   
    <span data-ttu-id="13f8e-128">a.</span><span class="sxs-lookup"><span data-stu-id="13f8e-128">a.</span></span> <span data-ttu-id="13f8e-129">Získat seznam všech odběrů a zkopírujte ID předplatného odběr, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="13f8e-129">Get a list of all your subscriptions and copy the SubscriptionId of the subscription you want to use.</span></span> <span data-ttu-id="13f8e-130">Zajistěte, aby že toto předplatné je stejný jako ten, který má aplikace Mobile Engagementu, který se chystáte interakci s pomocí rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="13f8e-130">Make sure this subscription is the same one which has the Mobile Engagement App which you are going to interact with using the APIs.</span></span> 
   
        Get-AzureRmSubscription
   
    <span data-ttu-id="13f8e-131">b.</span><span class="sxs-lookup"><span data-stu-id="13f8e-131">b.</span></span> <span data-ttu-id="13f8e-132">Spusťte následující příkaz poskytování ID odběru konfigurace odběru, který se má použít.</span><span class="sxs-lookup"><span data-stu-id="13f8e-132">Run the following command providing the SubscriptionId to configure the subscription to be used.</span></span>
   
        Select-AzureRmSubscription –SubscriptionId <subscriptionId>
5. <span data-ttu-id="13f8e-133">Zkopírujte text pro [New-AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) skriptu do místního počítače a uložte ho jako rutiny prostředí PowerShell (například `APIAuth.ps1`) a spusťte ho `.\APIAuth.ps1`.</span><span class="sxs-lookup"><span data-stu-id="13f8e-133">Copy the text for the [New-AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) script to your local machine and save it as a PowerShell cmdlet (e.g. `APIAuth.ps1`) and execute it `.\APIAuth.ps1`.</span></span> 
6. <span data-ttu-id="13f8e-134">Skript zobrazí výzvu k poskytují vstup pro **principalName**.</span><span class="sxs-lookup"><span data-stu-id="13f8e-134">The script will ask you to provide an input for **principalName**.</span></span> <span data-ttu-id="13f8e-135">Zadejte vhodný název sem, chcete použít k vytvoření aplikace Active Directory (např. APIAuth).</span><span class="sxs-lookup"><span data-stu-id="13f8e-135">Provide a suitable name here that you want to use to create your Active Directory application (e.g. APIAuth).</span></span> 
7. <span data-ttu-id="13f8e-136">Po dokončení skriptu zobrazí následující čtyři hodnoty, které potřebujete k ověření prostřednictvím kódu programu s AD proto nezapomeňte zkopírovat je.</span><span class="sxs-lookup"><span data-stu-id="13f8e-136">After the script completes, it will display the following four values that you will need to authenticate programmatically with AD so make sure to copy them.</span></span> 
   
    <span data-ttu-id="13f8e-137">**TenantId**, **SubscriptionId**, **ApplicationId**, a **tajný klíč**.</span><span class="sxs-lookup"><span data-stu-id="13f8e-137">**TenantId**, **SubscriptionId**, **ApplicationId**, and **Secret**.</span></span>
   
    <span data-ttu-id="13f8e-138">Budete používat TenantId jako `{TENANT_ID}`, ApplicationId jako `{CLIENT_ID}` a tajný jako `{CLIENT_SECRET}`.</span><span class="sxs-lookup"><span data-stu-id="13f8e-138">You will use TenantId as `{TENANT_ID}`, ApplicationId as `{CLIENT_ID}` and Secret as `{CLIENT_SECRET}`.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="13f8e-139">Výchozí zásady zabezpečení mohou blokovat spouštění skriptů prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="13f8e-139">Your default security policy may block you from running a PowerShell scripts.</span></span> <span data-ttu-id="13f8e-140">Pokud ano, můžete dočasně konfiguraci vaší zásady spouštění povolit spuštění skriptu pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="13f8e-140">If so, you temporarily configure your execution policy to allow script execution using the following command:</span></span>
   > 
   > <span data-ttu-id="13f8e-141">Set-ExecutionPolicy RemoteSigned</span><span class="sxs-lookup"><span data-stu-id="13f8e-141">Set-ExecutionPolicy RemoteSigned</span></span>
   > 
   > 
8. <span data-ttu-id="13f8e-142">Zde je, jak bude vypadat sadu rutin PS.</span><span class="sxs-lookup"><span data-stu-id="13f8e-142">Here is how the set of PS cmdlets would look like.</span></span> 
   
    ![][3]
9. <span data-ttu-id="13f8e-143">Zkontrolujte v portálu pro správu Azure, novou aplikaci AD byl vytvořen s názvem, která jste zadali na skript volá **principalName** pod **zobrazit aplikace Moje společnost vlastní**.</span><span class="sxs-lookup"><span data-stu-id="13f8e-143">Check in the Azure Management portal that a new AD application was created with the name you provided to the script called **principalName** under **Show Applications my company owns**.</span></span>
   
    ![][4]

#### <a name="steps-to-get-a-valid-token"></a><span data-ttu-id="13f8e-144">Kroky, chcete-li získat platný token</span><span class="sxs-lookup"><span data-stu-id="13f8e-144">Steps to get a valid token</span></span>
1. <span data-ttu-id="13f8e-145">Volání rozhraní API s následujícími parametry a nezapomeňte nahradit klienta\_ID, klient\_ID a klienta\_tajný klíč:</span><span class="sxs-lookup"><span data-stu-id="13f8e-145">Call the API with the following parameters and make sure to replace the TENANT\_ID, CLIENT\_ID and CLIENT\_SECRET:</span></span>
   
   * <span data-ttu-id="13f8e-146">**Adresa URL požadavku** jako *https://login.microsoftonline.com/ {klienta\_ID} / oauth2/token*</span><span class="sxs-lookup"><span data-stu-id="13f8e-146">**Request URL** as *https://login.microsoftonline.com/{TENANT\_ID}/oauth2/token*</span></span>
   * <span data-ttu-id="13f8e-147">**Hlavičku HTTP Content-Type** jako *application/x--www-form-urlencoded*</span><span class="sxs-lookup"><span data-stu-id="13f8e-147">**HTTP Content-Type header** as *application/x-www-form-urlencoded*</span></span>
   * <span data-ttu-id="13f8e-148">**Text žádosti HTTP** jako *udělit\_typ = klient\_přihlašovací údaje & client_id = {klienta\_ID} & tajný klíč client_secret = {klienta\_tajný klíč} & resource=https%3A%2F%2Fmanagement.core.windows.net%2F*</span><span class="sxs-lookup"><span data-stu-id="13f8e-148">**HTTP Request Body** as *grant\_type=client\_credentials&client_id={CLIENT\_ID}&client_secret={CLIENT\_SECRET}&resource=https%3A%2F%2Fmanagement.core.windows.net%2F*</span></span>
     
     <span data-ttu-id="13f8e-149">Toto je požadavek příklad:</span><span class="sxs-lookup"><span data-stu-id="13f8e-149">The following is an example request:</span></span>
     
       <span data-ttu-id="13f8e-150">POST / {TENANT_ID} / oauth2/token hostitele HTTP/1.1: login.microsoftonline.com Content-Type: grant_type application/x--www-form-urlencoded = client_credentials & client_id = {CLIENT_ID} & tajný klíč client_secret = {tajný klíč CLIENT_SECRET} & Zdroj Př napětí = https % 3A % 2f%2Fmanagement.Core.Windows.NET%2f</span><span class="sxs-lookup"><span data-stu-id="13f8e-150">POST /{TENANT_ID}/oauth2/token HTTP/1.1   Host: login.microsoftonline.com   Content-Type: application/x-www-form-urlencoded   grant_type=client_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&reso   urce=https%3A%2F%2Fmanagement.core.windows.net%2F</span></span>
     
     <span data-ttu-id="13f8e-151">Tady je příklad odpověď:</span><span class="sxs-lookup"><span data-stu-id="13f8e-151">Here is an example response:</span></span>
     
       <span data-ttu-id="13f8e-152">Protokol HTTP/1.1 200 OK Content-Type: application/json; charset = utf-8 Content-Length: 1234</span><span class="sxs-lookup"><span data-stu-id="13f8e-152">HTTP/1.1 200 OK   Content-Type: application/json; charset=utf-8   Content-Length: 1234</span></span>
     
       <span data-ttu-id="13f8e-153">{"token_type": "Nosiče", "expires_in": "3599", "expires_on": "1445395811", "not_before": "144 5391911 prostředků","": "https://management.core.windows.net/", "access_token": {ACCESS_TOKEN}}</span><span class="sxs-lookup"><span data-stu-id="13f8e-153">{"token_type":"Bearer","expires_in":"3599","expires_on":"1445395811","not_before":"144   5391911","resource":"https://management.core.windows.net/","access_token":{ACCESS_TOKEN}}</span></span>
     
     <span data-ttu-id="13f8e-154">Tento příklad zahrnuté kódování URL parametrů POST, `resource` hodnota je ve skutečnosti `https://management.core.windows.net/`.</span><span class="sxs-lookup"><span data-stu-id="13f8e-154">This example included URL encoding of the POST parameters, `resource` value is actually `https://management.core.windows.net/`.</span></span> <span data-ttu-id="13f8e-155">Dávejte pozor, abyste také adresu URL kódování `{CLIENT_SECRET}` jako může obsahovat speciální znaky.</span><span class="sxs-lookup"><span data-stu-id="13f8e-155">Be careful to also URL encode `{CLIENT_SECRET}` as it may contain special characters.</span></span>
     
     > [!NOTE]
     > <span data-ttu-id="13f8e-156">Pro testování, můžete použít nástroj pro klienta na HTTP jako [Fiddler](http://www.telerik.com/fiddler) nebo [rozšíření Chrome Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop)</span><span class="sxs-lookup"><span data-stu-id="13f8e-156">For testing, you can use an HTTP client tool like [Fiddler](http://www.telerik.com/fiddler) or [Chrome Postman extension](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop)</span></span> 
     > 
     > 
2. <span data-ttu-id="13f8e-157">Každé volání rozhraní API nyní obsahují hlavičce autorizace požadavku:</span><span class="sxs-lookup"><span data-stu-id="13f8e-157">Now in every API call, include the authorization request header:</span></span>
   
        Authorization: Bearer {ACCESS_TOKEN}
   
    <span data-ttu-id="13f8e-158">Pokud se vrátil kód stavu 401, zkontrolujte text odpovědi se vás může upozornit tokenu vypršela.</span><span class="sxs-lookup"><span data-stu-id="13f8e-158">If you get a 401 status code returned, check the response body, it might tell you the token is expired.</span></span> <span data-ttu-id="13f8e-159">V takovém případě získáte nový token.</span><span class="sxs-lookup"><span data-stu-id="13f8e-159">In that case, get a new token.</span></span>

## <a name="using-the-apis"></a><span data-ttu-id="13f8e-160">Pomocí rozhraní API</span><span class="sxs-lookup"><span data-stu-id="13f8e-160">Using the APIs</span></span>
<span data-ttu-id="13f8e-161">Teď, když máte platný token, jste připraveni k volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="13f8e-161">Now that you have a valid token, you are ready to make the API calls.</span></span>

1. <span data-ttu-id="13f8e-162">V každé žádosti o rozhraní API musíte předat token platný, neprošlé, který jste získali v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="13f8e-162">In each API request, you will need to pass a valid, unexpired token which you obtained in the previous section.</span></span>
2. <span data-ttu-id="13f8e-163">Musíte se připojit některé parametry do požadavku URI, který identifikuje vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="13f8e-163">You will need to plug in some parameters into the request URI which identifies your application.</span></span> <span data-ttu-id="13f8e-164">Požadavek URI vypadá takto</span><span class="sxs-lookup"><span data-stu-id="13f8e-164">The request URI looks like the following</span></span>
   
        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/
   
    <span data-ttu-id="13f8e-165">K získání parametrů, klikněte na název vaší aplikace a klikněte na řídicí panel a vy se zobrazí na stránce jako na následujícím obrázku se 3 parametry.</span><span class="sxs-lookup"><span data-stu-id="13f8e-165">To get the parameters, click on your application name and click Dashboard and you will see a page like the following with all the 3 parameters.</span></span>
   
   * <span data-ttu-id="13f8e-166">**1** `{subscription-id}`</span><span class="sxs-lookup"><span data-stu-id="13f8e-166">**1** `{subscription-id}`</span></span>
   * <span data-ttu-id="13f8e-167">**2** `{app-collection}`</span><span class="sxs-lookup"><span data-stu-id="13f8e-167">**2** `{app-collection}`</span></span>
   * <span data-ttu-id="13f8e-168">**3** `{app-resource-name}`</span><span class="sxs-lookup"><span data-stu-id="13f8e-168">**3** `{app-resource-name}`</span></span>
   * <span data-ttu-id="13f8e-169">**4** název vaše skupiny prostředků bude **MobileEngagement** Pokud jste vytvořili novou.</span><span class="sxs-lookup"><span data-stu-id="13f8e-169">**4** Your Resource Group name is going to be **MobileEngagement** unless you created a new one.</span></span> 
     
     ![Mobile Engagement rozhraní API URI parametry][2]

> [!NOTE]
> <br/>
> 
> 1. <span data-ttu-id="13f8e-171">Ignorujte adresu kořenové rozhraní API, jako to byla pro předchozí rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="13f8e-171">Ignore the API Root Address as this was for the previous APIs.</span></span><br/>
> 2. <span data-ttu-id="13f8e-172">Pokud jste vytvořili aplikaci pomocí portálu Azure Classic budete muset použijte název prostředku aplikace, které se liší od názvu aplikace, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="13f8e-172">If you created the app using Azure Classic portal then you need to use the Application Resource name which is different than the Application name itself.</span></span> <span data-ttu-id="13f8e-173">Pokud jste vytvořili aplikaci na portálu Azure doporučujeme, abyste používali samotný (není žádný rozdíl mezi název prostředku aplikace a název aplikace pro aplikace vytvořené v nového portálu) název aplikace.</span><span class="sxs-lookup"><span data-stu-id="13f8e-173">If you created the app in the Azure Portal then you should use the App Name itself (there is no differentiation between Application Resource Name and App Name for apps created in the new portal).</span></span>  
> 
> 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/ad-app-creation.png



