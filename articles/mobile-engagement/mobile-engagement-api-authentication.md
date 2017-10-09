---
title: aaaAuthenticate s Mobile Engagement REST API
description: Popisuje, jak tooauthenticate s Azure Mobile Engagement REST API
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
ms.openlocfilehash: 9b54aa5ec3da4bcf55ffe5b7e8d1759095d0c486
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-with-mobile-engagement-rest-apis"></a><span data-ttu-id="d60be-103">Ověření s Mobile Engagementem rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="d60be-103">Authenticate with Mobile Engagement REST APIs</span></span>
## <a name="overview"></a><span data-ttu-id="d60be-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="d60be-104">Overview</span></span>
<span data-ttu-id="d60be-105">Tento dokument popisuje, jak tooget platný Oauth AAD token tooauthenticate s hello Mobile Engagement REST API.</span><span class="sxs-lookup"><span data-stu-id="d60be-105">This document describes how tooget a valid AAD Oauth token tooauthenticate with hello Mobile Engagement REST APIs.</span></span> 

<span data-ttu-id="d60be-106">Předpokládá se, že máte platné předplatné Azure a jste vytvořili aplikaci Mobile Engagement, pomocí jednoho z našich [vývojáře kurzy](mobile-engagement-windows-store-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d60be-106">It is assumed that you have a valid Azure subscription and you have created a Mobile Engagement app using one of our [Developer Tutorials](mobile-engagement-windows-store-dotnet-get-started.md).</span></span>

## <a name="authentication"></a><span data-ttu-id="d60be-107">Authentication</span><span class="sxs-lookup"><span data-stu-id="d60be-107">Authentication</span></span>
<span data-ttu-id="d60be-108">Microsoft Azure Active Directory na základě tokenu se používá k ověřování OAuth.</span><span class="sxs-lookup"><span data-stu-id="d60be-108">A Microsoft Azure Active Directory based OAuth token is used for authentication.</span></span> 

<span data-ttu-id="d60be-109">V žádosti o pořadí tooauthentication rozhraní API musí hlavičku autorizace přidány tooevery požadavek, který je hello následující formulář:</span><span class="sxs-lookup"><span data-stu-id="d60be-109">In order tooauthentication an API request, an authorization header must be added tooevery request which is of hello following form:</span></span>

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

> [!NOTE]
> <span data-ttu-id="d60be-110">Azure Active Directory tokeny vyprší za 1 hodinu.</span><span class="sxs-lookup"><span data-stu-id="d60be-110">Azure Active Directory tokens expire in 1 hour.</span></span>
> 
> 

<span data-ttu-id="d60be-111">Existuje několik způsobů tooget token.</span><span class="sxs-lookup"><span data-stu-id="d60be-111">There are several ways tooget a token.</span></span> <span data-ttu-id="d60be-112">Od hello rozhraní API jsou obecně volat z cloudové služby chcete toouse klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d60be-112">Since hello APIs are generally called from a cloud service, you want toouse an API key.</span></span> <span data-ttu-id="d60be-113">Klíč rozhraní API v Azure terminologii nazývá hlavní heslo služby.</span><span class="sxs-lookup"><span data-stu-id="d60be-113">An API key in Azure terminology is called a Service principal password.</span></span> <span data-ttu-id="d60be-114">Hello následující postup popisuje jedním ze způsobů toosetting ho až ručně.</span><span class="sxs-lookup"><span data-stu-id="d60be-114">hello following procedure describes one way toosetting it up manually.</span></span>

### <a name="one-time-setup-using-script"></a><span data-ttu-id="d60be-115">Jednorázové instalace (pomocí skriptu.)</span><span class="sxs-lookup"><span data-stu-id="d60be-115">One-time setup (using script)</span></span>
<span data-ttu-id="d60be-116">Postupujte podle hello sadu pokynů níže tooperform hello instalaci pomocí skriptu prostředí PowerShell, který přebírá hello minimální doba pro instalační program, ale používá hello nejvíce povolenou výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d60be-116">You should follow hello set of instructions below tooperform hello setup using a PowerShell script which takes hello minimum time for setup but uses hello most permissible defaults.</span></span> <span data-ttu-id="d60be-117">Volitelně můžete také podle pokynů hello v hello [ruční instalaci](mobile-engagement-api-authentication-manual.md) to chcete provést z hello přímo portál Azure a proveďte jemnějšího konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d60be-117">Optionally, you can also follow hello instructions in hello [manual setup](mobile-engagement-api-authentication-manual.md) for doing this from hello Azure portal directly and do finer configuration.</span></span> 

1. <span data-ttu-id="d60be-118">Získat nejnovější verzi prostředí Azure PowerShell z hello [zde](http://aka.ms/webpi-azps).</span><span class="sxs-lookup"><span data-stu-id="d60be-118">Get hello latest version of Azure PowerShell from [here](http://aka.ms/webpi-azps).</span></span> <span data-ttu-id="d60be-119">Další informace o pokyny ke stažení hello uvidíte to [odkaz](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d60be-119">For more information on hello download instructions, you can see this [link](/powershell/azure/overview).</span></span>  
2. <span data-ttu-id="d60be-120">Po instalaci prostředí Azure PowerShell, použijte hello následující příkazy, abyste měli hello tooensure **Azure modulu** nainstalován:</span><span class="sxs-lookup"><span data-stu-id="d60be-120">Once Azure PowerShell is installed, use hello following commands tooensure that you have hello **Azure module** installed:</span></span>
   
    <span data-ttu-id="d60be-121">a.</span><span class="sxs-lookup"><span data-stu-id="d60be-121">a.</span></span> <span data-ttu-id="d60be-122">Zkontrolujte, zda je k dispozici v seznamu dostupných modulů hello modul Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="d60be-122">Make sure hello Azure PowerShell module is available in hello list of available modules.</span></span> 
   
        Get-Module –ListAvailable 
   
    ![K dispozici moduly Azure][1]
   
    <span data-ttu-id="d60be-124">b.</span><span class="sxs-lookup"><span data-stu-id="d60be-124">b.</span></span> <span data-ttu-id="d60be-125">Pokud nenajdete modul Azure PowerShell hello hello výše seznamu musíte toorun hello následující:</span><span class="sxs-lookup"><span data-stu-id="d60be-125">If you do not find hello Azure PowerShell module in hello above list then you need toorun hello following:</span></span>
   
        Import-Module Azure 
3. <span data-ttu-id="d60be-126">Přihlášení toohello Azure Resource Manageru z prostředí PowerShell spuštěním hello následující příkaz a poskytuje uživatelské jméno a heslo pro účet Azure:</span><span class="sxs-lookup"><span data-stu-id="d60be-126">Login toohello Azure Resource Manager from PowerShell by running hello following command and providing your user name and password for your Azure account:</span></span> 
   
        Login-AzureRmAccount
4. <span data-ttu-id="d60be-127">Pokud máte více předplatných, pak byste měli spustit hello následující:</span><span class="sxs-lookup"><span data-stu-id="d60be-127">If you have multiple subscriptions then you should run hello following:</span></span>
   
    <span data-ttu-id="d60be-128">a.</span><span class="sxs-lookup"><span data-stu-id="d60be-128">a.</span></span> <span data-ttu-id="d60be-129">Získáte seznam všech předplatných a kopírování hello SubscriptionId hello odběr, který chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="d60be-129">Get a list of all your subscriptions and copy hello SubscriptionId of hello subscription you want toouse.</span></span> <span data-ttu-id="d60be-130">Zajistěte, aby toto předplatné je stejný jako ten, který má hello aplikace Mobile Engagementu, který se bude hello hello toointeract s použitím rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d60be-130">Make sure this subscription is hello same one which has hello Mobile Engagement App which you are going toointeract with using hello APIs.</span></span> 
   
        Get-AzureRmSubscription
   
    <span data-ttu-id="d60be-131">b.</span><span class="sxs-lookup"><span data-stu-id="d60be-131">b.</span></span> <span data-ttu-id="d60be-132">Hello spusťte následující příkaz, který poskytnete hello SubscriptionId tooconfigure hello předplatné toobe použít.</span><span class="sxs-lookup"><span data-stu-id="d60be-132">Run hello following command providing hello SubscriptionId tooconfigure hello subscription toobe used.</span></span>
   
        Select-AzureRmSubscription –SubscriptionId <subscriptionId>
5. <span data-ttu-id="d60be-133">Zkopírujte hello text hello [New-AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) skriptu tooyour místního počítače a uložte ho jako rutiny prostředí PowerShell (např `APIAuth.ps1`) a spusťte ho `.\APIAuth.ps1`.</span><span class="sxs-lookup"><span data-stu-id="d60be-133">Copy hello text for hello [New-AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) script tooyour local machine and save it as a PowerShell cmdlet (e.g. `APIAuth.ps1`) and execute it `.\APIAuth.ps1`.</span></span> 
6. <span data-ttu-id="d60be-134">Hello skriptu se zeptá, tooprovide na vstup pro **principalName**.</span><span class="sxs-lookup"><span data-stu-id="d60be-134">hello script will ask you tooprovide an input for **principalName**.</span></span> <span data-ttu-id="d60be-135">Zadejte vhodný název má toouse toocreate aplikace služby Active Directory (např. APIAuth).</span><span class="sxs-lookup"><span data-stu-id="d60be-135">Provide a suitable name here that you want toouse toocreate your Active Directory application (e.g. APIAuth).</span></span> 
7. <span data-ttu-id="d60be-136">Po dokončení skriptu hello zobrazí hello následující čtyři hodnoty, které budete potřebovat tooauthenticate prostřednictvím kódu programu s AD, ujistěte se, že toocopy je.</span><span class="sxs-lookup"><span data-stu-id="d60be-136">After hello script completes, it will display hello following four values that you will need tooauthenticate programmatically with AD so make sure toocopy them.</span></span> 
   
    <span data-ttu-id="d60be-137">**TenantId**, **SubscriptionId**, **ApplicationId**, a **tajný klíč**.</span><span class="sxs-lookup"><span data-stu-id="d60be-137">**TenantId**, **SubscriptionId**, **ApplicationId**, and **Secret**.</span></span>
   
    <span data-ttu-id="d60be-138">Budete používat TenantId jako `{TENANT_ID}`, ApplicationId jako `{CLIENT_ID}` a tajný jako `{CLIENT_SECRET}`.</span><span class="sxs-lookup"><span data-stu-id="d60be-138">You will use TenantId as `{TENANT_ID}`, ApplicationId as `{CLIENT_ID}` and Secret as `{CLIENT_SECRET}`.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="d60be-139">Výchozí zásady zabezpečení mohou blokovat spouštění skriptů prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d60be-139">Your default security policy may block you from running a PowerShell scripts.</span></span> <span data-ttu-id="d60be-140">Pokud ano, můžete dočasně konfiguraci vašeho provádění zásad tooallow spuštění skriptu pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d60be-140">If so, you temporarily configure your execution policy tooallow script execution using hello following command:</span></span>
   > 
   > <span data-ttu-id="d60be-141">Set-ExecutionPolicy RemoteSigned</span><span class="sxs-lookup"><span data-stu-id="d60be-141">Set-ExecutionPolicy RemoteSigned</span></span>
   > 
   > 
8. <span data-ttu-id="d60be-142">Zde je, jak bude vypadat hello sadu rutin PS.</span><span class="sxs-lookup"><span data-stu-id="d60be-142">Here is how hello set of PS cmdlets would look like.</span></span> 
   
    ![][3]
9. <span data-ttu-id="d60be-143">Na portálu pro správu Azure hello zkontrolujte, zda novou aplikaci AD byla vytvořena s názvem hello je zadaný skript toohello nazývá **principalName** pod **zobrazit aplikace Moje společnost vlastní**.</span><span class="sxs-lookup"><span data-stu-id="d60be-143">Check in hello Azure Management portal that a new AD application was created with hello name you provided toohello script called **principalName** under **Show Applications my company owns**.</span></span>
   
    ![][4]

#### <a name="steps-tooget-a-valid-token"></a><span data-ttu-id="d60be-144">Kroky tooget platný token</span><span class="sxs-lookup"><span data-stu-id="d60be-144">Steps tooget a valid token</span></span>
1. <span data-ttu-id="d60be-145">Volání rozhraní API hello s hello následující parametry a ujistěte se, zda text hello tooreplace klienta\_ID, klient\_ID a klienta\_tajný klíč:</span><span class="sxs-lookup"><span data-stu-id="d60be-145">Call hello API with hello following parameters and make sure tooreplace hello TENANT\_ID, CLIENT\_ID and CLIENT\_SECRET:</span></span>
   
   * <span data-ttu-id="d60be-146">**Adresa URL požadavku** jako *https://login.microsoftonline.com/ {klienta\_ID} / oauth2/token*</span><span class="sxs-lookup"><span data-stu-id="d60be-146">**Request URL** as *https://login.microsoftonline.com/{TENANT\_ID}/oauth2/token*</span></span>
   * <span data-ttu-id="d60be-147">**Hlavičku HTTP Content-Type** jako *application/x--www-form-urlencoded*</span><span class="sxs-lookup"><span data-stu-id="d60be-147">**HTTP Content-Type header** as *application/x-www-form-urlencoded*</span></span>
   * <span data-ttu-id="d60be-148">**Text žádosti HTTP** jako *udělit\_typ = klient\_přihlašovací údaje & client_id = {klienta\_ID} & tajný klíč client_secret = {klienta\_tajný klíč} & resource=https%3A%2F%2Fmanagement.core.windows.net%2F*</span><span class="sxs-lookup"><span data-stu-id="d60be-148">**HTTP Request Body** as *grant\_type=client\_credentials&client_id={CLIENT\_ID}&client_secret={CLIENT\_SECRET}&resource=https%3A%2F%2Fmanagement.core.windows.net%2F*</span></span>
     
     <span data-ttu-id="d60be-149">Následující Hello je požadavek příklad:</span><span class="sxs-lookup"><span data-stu-id="d60be-149">hello following is an example request:</span></span>
     
       <span data-ttu-id="d60be-150">POST / {TENANT_ID} / oauth2/token hostitele HTTP/1.1: login.microsoftonline.com Content-Type: grant_type application/x--www-form-urlencoded = client_credentials & client_id = {CLIENT_ID} & tajný klíč client_secret = {tajný klíč CLIENT_SECRET} & Zdroj Př napětí = https % 3A % 2f%2Fmanagement.Core.Windows.NET%2f</span><span class="sxs-lookup"><span data-stu-id="d60be-150">POST /{TENANT_ID}/oauth2/token HTTP/1.1   Host: login.microsoftonline.com   Content-Type: application/x-www-form-urlencoded   grant_type=client_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&reso   urce=https%3A%2F%2Fmanagement.core.windows.net%2F</span></span>
     
     <span data-ttu-id="d60be-151">Tady je příklad odpověď:</span><span class="sxs-lookup"><span data-stu-id="d60be-151">Here is an example response:</span></span>
     
       <span data-ttu-id="d60be-152">Protokol HTTP/1.1 200 OK Content-Type: application/json; charset = utf-8 Content-Length: 1234</span><span class="sxs-lookup"><span data-stu-id="d60be-152">HTTP/1.1 200 OK   Content-Type: application/json; charset=utf-8   Content-Length: 1234</span></span>
     
       <span data-ttu-id="d60be-153">{"token_type": "Nosiče", "expires_in": "3599", "expires_on": "1445395811", "not_before": "144 5391911 prostředků","": "https://management.core.windows.net/", "access_token": {ACCESS_TOKEN}}</span><span class="sxs-lookup"><span data-stu-id="d60be-153">{"token_type":"Bearer","expires_in":"3599","expires_on":"1445395811","not_before":"144   5391911","resource":"https://management.core.windows.net/","access_token":{ACCESS_TOKEN}}</span></span>
     
     <span data-ttu-id="d60be-154">Tento příklad zahrnuté kódování URL hello parametry POST, `resource` hodnota je ve skutečnosti `https://management.core.windows.net/`.</span><span class="sxs-lookup"><span data-stu-id="d60be-154">This example included URL encoding of hello POST parameters, `resource` value is actually `https://management.core.windows.net/`.</span></span> <span data-ttu-id="d60be-155">Dávejte pozor, kódování tooalso URL `{CLIENT_SECRET}` jako může obsahovat speciální znaky.</span><span class="sxs-lookup"><span data-stu-id="d60be-155">Be careful tooalso URL encode `{CLIENT_SECRET}` as it may contain special characters.</span></span>
     
     > [!NOTE]
     > <span data-ttu-id="d60be-156">Pro testování, můžete použít nástroj pro klienta na HTTP jako [Fiddler](http://www.telerik.com/fiddler) nebo [rozšíření Chrome Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop)</span><span class="sxs-lookup"><span data-stu-id="d60be-156">For testing, you can use an HTTP client tool like [Fiddler](http://www.telerik.com/fiddler) or [Chrome Postman extension](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop)</span></span> 
     > 
     > 
2. <span data-ttu-id="d60be-157">Každé volání rozhraní API nyní obsahují hlavička požadavku hello autorizace:</span><span class="sxs-lookup"><span data-stu-id="d60be-157">Now in every API call, include hello authorization request header:</span></span>
   
        Authorization: Bearer {ACCESS_TOKEN}
   
    <span data-ttu-id="d60be-158">Pokud se zobrazí kód stavu 401 vrátí, zkontrolujte hello odpovědi, ho vás může upozornit, že hello tokenu vypršela platnost.</span><span class="sxs-lookup"><span data-stu-id="d60be-158">If you get a 401 status code returned, check hello response body, it might tell you hello token is expired.</span></span> <span data-ttu-id="d60be-159">V takovém případě získáte nový token.</span><span class="sxs-lookup"><span data-stu-id="d60be-159">In that case, get a new token.</span></span>

## <a name="using-hello-apis"></a><span data-ttu-id="d60be-160">Pomocí hello rozhraní API</span><span class="sxs-lookup"><span data-stu-id="d60be-160">Using hello APIs</span></span>
<span data-ttu-id="d60be-161">Teď, když máte platný token, se volání připravené toomake hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d60be-161">Now that you have a valid token, you are ready toomake hello API calls.</span></span>

1. <span data-ttu-id="d60be-162">V každé žádosti o rozhraní API budete potřebovat toopass token platný, neprošlé, který jste získali v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="d60be-162">In each API request, you will need toopass a valid, unexpired token which you obtained in hello previous section.</span></span>
2. <span data-ttu-id="d60be-163">Budete potřebovat tooplug v některé parametry do hello požadavek URI, který identifikuje vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d60be-163">You will need tooplug in some parameters into hello request URI which identifies your application.</span></span> <span data-ttu-id="d60be-164">identifikátor URI požadavku Hello vypadá jako následující hello</span><span class="sxs-lookup"><span data-stu-id="d60be-164">hello request URI looks like hello following</span></span>
   
        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/
   
    <span data-ttu-id="d60be-165">Parametry hello tooget, klikněte na název vaší aplikace a klikněte na řídicí panel a na stránce se zobrazí, jako jsou následující hello se všemi hello 3 parametry.</span><span class="sxs-lookup"><span data-stu-id="d60be-165">tooget hello parameters, click on your application name and click Dashboard and you will see a page like hello following with all hello 3 parameters.</span></span>
   
   * <span data-ttu-id="d60be-166">**1** `{subscription-id}`</span><span class="sxs-lookup"><span data-stu-id="d60be-166">**1** `{subscription-id}`</span></span>
   * <span data-ttu-id="d60be-167">**2** `{app-collection}`</span><span class="sxs-lookup"><span data-stu-id="d60be-167">**2** `{app-collection}`</span></span>
   * <span data-ttu-id="d60be-168">**3** `{app-resource-name}`</span><span class="sxs-lookup"><span data-stu-id="d60be-168">**3** `{app-resource-name}`</span></span>
   * <span data-ttu-id="d60be-169">**4** název vaše skupiny prostředků bude toobe **MobileEngagement** Pokud jste vytvořili novou.</span><span class="sxs-lookup"><span data-stu-id="d60be-169">**4** Your Resource Group name is going toobe **MobileEngagement** unless you created a new one.</span></span> 
     
     ![Mobile Engagement rozhraní API URI parametry][2]

> [!NOTE]
> <br/>
> 
> 1. <span data-ttu-id="d60be-171">Ignorovat hello kořenové adresy rozhraní API, jako to byla pro hello předchozí rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d60be-171">Ignore hello API Root Address as this was for hello previous APIs.</span></span><br/>
> 2. <span data-ttu-id="d60be-172">Pokud jste vytvořili aplikaci hello pomocí portálu Azure Classic budete potřebovat toouse hello prostředku aplikační název, který je jiný než název aplikace hello, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="d60be-172">If you created hello app using Azure Classic portal then you need toouse hello Application Resource name which is different than hello Application name itself.</span></span> <span data-ttu-id="d60be-173">Pokud jste vytvořili aplikaci hello v hello portálu Azure doporučujeme, abyste používali hello samotný (není žádný rozdíl mezi název prostředku aplikace a název aplikace pro aplikace vytvořené v nový portál hello) název aplikace.</span><span class="sxs-lookup"><span data-stu-id="d60be-173">If you created hello app in hello Azure Portal then you should use hello App Name itself (there is no differentiation between Application Resource Name and App Name for apps created in hello new portal).</span></span>  
> 
> 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/ad-app-creation.png



