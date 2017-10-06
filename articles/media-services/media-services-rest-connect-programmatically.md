---
title: "aaaConnecting tooMedia účtu služby pomocí rozhraní REST API | Microsoft Docs"
description: "Toto téma popisuje, jak pomocí služby tooMedia tooconnect REST API."
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 79dc64f1-15d8-4a81-b9d9-3d3c44d2e1e8
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
ms.openlocfilehash: 1d5064a3612dc96f5c5ad910d183d84fb70a3b6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-toomedia-services-account-using-media-services-rest-api"></a><span data-ttu-id="840bd-103">Připojení tooMedia účtu služby pomocí Media Services REST API</span><span class="sxs-lookup"><span data-stu-id="840bd-103">Connecting tooMedia Services Account using Media Services REST API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="840bd-104">.NET</span><span class="sxs-lookup"><span data-stu-id="840bd-104">.NET</span></span>](media-services-dotnet-connect-programmatically.md)
> * [<span data-ttu-id="840bd-105">REST</span><span class="sxs-lookup"><span data-stu-id="840bd-105">REST</span></span>](media-services-rest-connect-programmatically.md)
> 
> 

<span data-ttu-id="840bd-106">Toto téma popisuje, jak tooobtain tooMicrosoft programové připojení Azure Media Services, pokud jsou programování s hello Media Services REST API.</span><span class="sxs-lookup"><span data-stu-id="840bd-106">This topic describes how tooobtain a programmatic connection tooMicrosoft Azure Media Services when you are programming with hello Media Services REST API.</span></span>

<span data-ttu-id="840bd-107">Při přístupu k Microsoft Azure Media Services jsou požadovány dvě věci: přístupový token poskytovaný služby Řízení přístupu Azure (ACS) a hello URI Media Services sám sebe.</span><span class="sxs-lookup"><span data-stu-id="840bd-107">Two things are required when accessing Microsoft Azure Media Services: An access token provided by Azure Access Control Services (ACS), and hello URI of Media Services itself.</span></span> <span data-ttu-id="840bd-108">Můžete použít jakýmkoli způsobem, který má být při vytváření tyto požadavky, dokud, zadejte hodnoty hlavičky správné hello a předat hello přístupový token správně při volání metody ve službě Media Services.</span><span class="sxs-lookup"><span data-stu-id="840bd-108">You can use any means you want when creating these requests as long as you specify hello correct header values and pass in hello access token correctly when calling into Media Services.</span></span>

<span data-ttu-id="840bd-109">Hello následující kroky popisují hello nejběžnější pracovního postupu při použití tooMedia tooconnect hello Media Services REST API služby:</span><span class="sxs-lookup"><span data-stu-id="840bd-109">hello following steps describe hello most common workflow when using hello Media Services REST API tooconnect tooMedia Services:</span></span>

1. <span data-ttu-id="840bd-110">Získání tokenu přístupu</span><span class="sxs-lookup"><span data-stu-id="840bd-110">Getting an access token</span></span> 
2. <span data-ttu-id="840bd-111">Připojení toohello Media Services URI</span><span class="sxs-lookup"><span data-stu-id="840bd-111">Connecting toohello Media Services URI</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="840bd-112">Po úspěšném připojení toohttps://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services.</span><span class="sxs-lookup"><span data-stu-id="840bd-112">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="840bd-113">Je nutné provést následující volání toohello nový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="840bd-113">You must make subsequent calls toohello new URI.</span></span>
   > <span data-ttu-id="840bd-114">Může se také zobrazit odpověď HTTP/1.1 200 obsahující hello popis metadat rozhraní API ODATA.</span><span class="sxs-lookup"><span data-stu-id="840bd-114">You may also receive a HTTP/1.1 200 response that contains hello ODATA API metadata description.</span></span>
   > 
   > 
3. <span data-ttu-id="840bd-115">Následné rozhraní API po volání toohello nové adrese URL.</span><span class="sxs-lookup"><span data-stu-id="840bd-115">Post your subsequent API calls toohello new URL.</span></span> 
   
    <span data-ttu-id="840bd-116">Například pokud po pokusu o tooconnect, jste získali hello následující:</span><span class="sxs-lookup"><span data-stu-id="840bd-116">For example, if after trying tooconnect, you got hello following:</span></span>
   
        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
   
    <span data-ttu-id="840bd-117">Publikujte vaší následné toohttps://wamsbayclus001rest-hs.cloudapp.net/api/ volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="840bd-117">You should post your subsequent API calls toohttps://wamsbayclus001rest-hs.cloudapp.net/api/.</span></span>

    >[!NOTE]
    ><span data-ttu-id="840bd-118">Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="840bd-118">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="840bd-119">Měli byste použít hello stejné ID zásad, pokud vždy používáte hello stejné dny / přístupová oprávnění, například zásady pro lokátory, které jsou určený tooremain zavedené po dlouhou dobu (bez odeslání zásady).</span><span class="sxs-lookup"><span data-stu-id="840bd-119">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="840bd-120">Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.</span><span class="sxs-lookup"><span data-stu-id="840bd-120">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

## <a name="access-control-address"></a><span data-ttu-id="840bd-121">Adresy MAC</span><span class="sxs-lookup"><span data-stu-id="840bd-121">Access control address</span></span>
<span data-ttu-id="840bd-122">Služba Media Services adresy MAC je https://wamsprodglobal001acs.accesscontrol.windows.net, s výjimkou oblast Severní Čína, kde je https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn.</span><span class="sxs-lookup"><span data-stu-id="840bd-122">Media Services access control address is https://wamsprodglobal001acs.accesscontrol.windows.net, except for North China region, where it is https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn.</span></span>

## <a name="getting-an-access-token"></a><span data-ttu-id="840bd-123">Získání tokenu přístupu</span><span class="sxs-lookup"><span data-stu-id="840bd-123">Getting an access token</span></span>
<span data-ttu-id="840bd-124">tooaccess Media Services přímo prostřednictvím hello REST API, načtou přístupový token ze služby ACS a použít ho při každé žádosti protokolu HTTP, které provedete do provozu hello.</span><span class="sxs-lookup"><span data-stu-id="840bd-124">tooaccess Media Services directly through hello REST API, retrieve an access token from ACS and use it during every HTTP request you make into hello service.</span></span> <span data-ttu-id="840bd-125">Tento token je podobné tooother tokeny poskytované služby ACS na základě deklarací přístup součástí hello hlavičky požadavku HTTP a pomocí protokolu hello OAuth v2.</span><span class="sxs-lookup"><span data-stu-id="840bd-125">This token is similar tooother tokens provided by ACS based on access claims provided in hello header of an HTTP request and using hello OAuth v2 protocol.</span></span> <span data-ttu-id="840bd-126">Před připojením přímo tooMedia služby nepotřebujete další nezbytné součásti.</span><span class="sxs-lookup"><span data-stu-id="840bd-126">You do not need any other prerequisites before directly connecting tooMedia Services.</span></span>

<span data-ttu-id="840bd-127">Hello následující příklad ukazuje hello hlavičku požadavku HTTP a text používá tooretrieve token.</span><span class="sxs-lookup"><span data-stu-id="840bd-127">hello following example shows hello HTTP request header and body used tooretrieve a token.</span></span>

<span data-ttu-id="840bd-128">**Záhlaví**:</span><span class="sxs-lookup"><span data-stu-id="840bd-128">**Header**:</span></span>

    POST https://wamsprodglobal001acs.accesscontrol.windows.net/v2/OAuth2-13 HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Host: wamsprodglobal001acs.accesscontrol.windows.net
    Content-Length: 120
    Expect: 100-continue
    Connection: Keep-Alive
    Accept: application/json


<span data-ttu-id="840bd-129">**Text**:</span><span class="sxs-lookup"><span data-stu-id="840bd-129">**Body**:</span></span>

<span data-ttu-id="840bd-130">Je třeba hodnoty client_id a tajný klíč client_secret hello tooprove v textu hello tohoto požadavku; client_id a tajný klíč client_secret odpovídají se, toohello AccountName a AccountKey hodnoty, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="840bd-130">You need tooprove hello client_id and client_secret values in hello body of this request; client_id and client_secret correspond toohello AccountName and AccountKey values, respectively.</span></span> <span data-ttu-id="840bd-131">Tyto hodnoty jsou poskytovány tooyou Media Services při nastavování účtu.</span><span class="sxs-lookup"><span data-stu-id="840bd-131">These values are provided tooyou by Media Services when you set up your account.</span></span> 

<span data-ttu-id="840bd-132">Všimněte si, že hello AccountKey pro váš účet Media Services musí být kódovaná adresou URL (viz [kódování procent](http://tools.ietf.org/html/rfc3986#section-2.1) při použití jako hodnota tajný klíč client_secret hello v žádosti o token přístupu.</span><span class="sxs-lookup"><span data-stu-id="840bd-132">Note that hello AccountKey for your Media Services account must be URL-encoded (see [Percent-Encoding](http://tools.ietf.org/html/rfc3986#section-2.1) when using it as hello client_secret value in your access token request.</span></span>

    grant_type=client_credentials&client_id=ams_account_name&client_secret=URL_encoded_ams_account_key&scope=urn%3aWindowsAzureMediaServices


<span data-ttu-id="840bd-133">Například:</span><span class="sxs-lookup"><span data-stu-id="840bd-133">For example:</span></span> 

    grant_type=client_credentials&client_id=amstestaccount001&client_secret=wUNbKhNj07oqjqU3Ah9R9f4kqTJ9avPpfe6Pk3YZ7ng%3d&scope=urn%3aWindowsAzureMediaServices


<span data-ttu-id="840bd-134">Hello následující příklad ukazuje hello odpověď HTTP, který obsahuje hello přístup v hello odpovědi tokenu.</span><span class="sxs-lookup"><span data-stu-id="840bd-134">hello following example shows hello HTTP response that contains hello access token in hello response body.</span></span>

    HTTP/1.1 200 OK
    Cache-Control: no-cache, no-store
    Pragma: no-cache
    Content-Type: application/json; charset=utf-8
    Expires: -1
    request-id: a65150f5-2784-4a01-a4b7-33456187ad83
    X-Content-Type-Options: nosniff
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 15 Jan 2015 08:07:20 GMT
    Content-Length: 670

    {  
       "token_type":"http://schemas.xmlsoap.org/ws/2009/11/swt-token-profile-1.0",
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }


> [!NOTE]
> <span data-ttu-id="840bd-135">Doporučujeme toocache hello "access_token" a "expires_in" hodnoty tooan externího úložiště.</span><span class="sxs-lookup"><span data-stu-id="840bd-135">It is recommended toocache hello "access_token " and "expires_in " values tooan external storage.</span></span> <span data-ttu-id="840bd-136">Hello token data může být později načíst z úložiště hello a opětovně využít v voláními rozhraní REST API Media Services.</span><span class="sxs-lookup"><span data-stu-id="840bd-136">hello token data could later be retrieved from hello storage and re-used in your Media Services REST API calls.</span></span> <span data-ttu-id="840bd-137">To je obzvláště užitečná pro scénáře, kde hello token lze bezpečně sdílet mezi více procesy nebo počítače.</span><span class="sxs-lookup"><span data-stu-id="840bd-137">This is especially useful for scenarios where hello token can be securely shared among multiple processes or computers.</span></span>
> 
> 

<span data-ttu-id="840bd-138">Ujistěte se, že hodnota "expires_in" hello toomonitor hello přístupu tokenu a podle potřeby aktualizujte voláními rozhraní REST API s nové tokeny.</span><span class="sxs-lookup"><span data-stu-id="840bd-138">Make sure toomonitor hello "expires_in" value of hello access token and update your REST API calls with new tokens as needed.</span></span>

### <a name="connecting-toohello-media-services-uri"></a><span data-ttu-id="840bd-139">Připojení toohello Media Services URI</span><span class="sxs-lookup"><span data-stu-id="840bd-139">Connecting toohello Media Services URI</span></span>
<span data-ttu-id="840bd-140">Kořenová Hello identifikátor URI pro Media Services je https://media.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="840bd-140">hello root URI for Media Services is https://media.windows.net/.</span></span> <span data-ttu-id="840bd-141">By měl původně připojení toothis identifikátor URI, a pokud jste zpátky v odpovědi 301 přesměrování, měli byste si následující volání toohello nový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="840bd-141">You should initially connect toothis URI, and if you get a 301 redirect back in response, you should make subsequent calls toohello new URI.</span></span> <span data-ttu-id="840bd-142">Kromě toho nepoužívejte veškeré auto přesměrování nebo postupujte podle logiky své žádosti.</span><span class="sxs-lookup"><span data-stu-id="840bd-142">In addition, do not use any auto-redirect/follow logic in your requests.</span></span> <span data-ttu-id="840bd-143">Příkazy HTTP a textem žádosti nebude předají toohello nový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="840bd-143">HTTP verbs and request bodies will not be forwarded toohello new URI.</span></span>

<span data-ttu-id="840bd-144">Všimněte si, že hello kořenového identifikátoru URI pro nahrávání a stahování souborů Asset je https://yourstorageaccount.blob.core.windows.net/, kde je název účtu úložiště hello hello stejné ten, který jste použili při nastavení vašeho účtu Media Services.</span><span class="sxs-lookup"><span data-stu-id="840bd-144">Note that hello root URI for uploading and downloading Asset files is https://yourstorageaccount.blob.core.windows.net/ where hello storage account name is hello same one you used during your Media Services account setup.</span></span>

<span data-ttu-id="840bd-145">Hello následující příklad ukazuje HTTP žádost toohello Media Services kořenového identifikátoru URI (https://media.windows.net/).</span><span class="sxs-lookup"><span data-stu-id="840bd-145">hello following example demonstrates HTTP request toohello Media Services root URI (https://media.windows.net/).</span></span> <span data-ttu-id="840bd-146">žádost o Hello získá zpět v odpovědi 301 přesměrování.</span><span class="sxs-lookup"><span data-stu-id="840bd-146">hello request gets a 301 redirect back in response.</span></span> <span data-ttu-id="840bd-147">Hello následného požadavku používá hello nový identifikátor URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).</span><span class="sxs-lookup"><span data-stu-id="840bd-147">hello subsequent request is using hello new URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).</span></span>     

<span data-ttu-id="840bd-148">**Požadavek HTTP**:</span><span class="sxs-lookup"><span data-stu-id="840bd-148">**HTTP Request**:</span></span>

    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: media.windows.net


<span data-ttu-id="840bd-149">**Odpověď HTTP**:</span><span class="sxs-lookup"><span data-stu-id="840bd-149">**HTTP Response**:</span></span>

    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
    Server: Microsoft-IIS/8.5
    request-id: 987d7652-497a-44e5-b815-f492e02aef97
    x-ms-request-id: 987d7652-497a-44e5-b815-f492e02aef97
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:53 GMT
    Content-Length: 164

    <html><head><title>Object moved</title></head><body>
    <h2>Object moved too<a href="https://wamsbayclus001rest-hs.cloudapp.net/api/">here</a>.</h2>
    </body></html>


<span data-ttu-id="840bd-150">**Požadavek HTTP** (pomocí hello nový identifikátor URI):</span><span class="sxs-lookup"><span data-stu-id="840bd-150">**HTTP Request** (using hello new URI):</span></span>

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: wamsbayclus001rest-hs.cloudapp.net


<span data-ttu-id="840bd-151">**Odpověď HTTP**:</span><span class="sxs-lookup"><span data-stu-id="840bd-151">**HTTP Response**:</span></span>

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1250
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    x-ms-request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:52 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata","value":[{"name":"AccessPolicies","url":"AccessPolicies"},{"name":"Locators","url":"Locators"},{"name":"ContentKeys","url":"ContentKeys"},{"name":"ContentKeyAuthorizationPolicyOptions","url":"ContentKeyAuthorizationPolicyOptions"},{"name":"ContentKeyAuthorizationPolicies","url":"ContentKeyAuthorizationPolicies"},{"name":"Files","url":"Files"},{"name":"Assets","url":"Assets"},{"name":"AssetDeliveryPolicies","url":"AssetDeliveryPolicies"},{"name":"IngestManifestFiles","url":"IngestManifestFiles"},{"name":"IngestManifestAssets","url":"IngestManifestAssets"},{"name":"IngestManifests","url":"IngestManifests"},{"name":"StorageAccounts","url":"StorageAccounts"},{"name":"Tasks","url":"Tasks"},{"name":"NotificationEndPoints","url":"NotificationEndPoints"},{"name":"Jobs","url":"Jobs"},{"name":"TaskTemplates","url":"TaskTemplates"},{"name":"JobTemplates","url":"JobTemplates"},{"name":"MediaProcessors","url":"MediaProcessors"},{"name":"EncodingReservedUnitTypes","url":"EncodingReservedUnitTypes"},{"name":"Operations","url":"Operations"},{"name":"StreamingEndpoints","url":"StreamingEndpoints"},{"name":"Channels","url":"Channels"},{"name":"Programs","url":"Programs"}]}



> [!NOTE]
> <span data-ttu-id="840bd-152">Po získání hello nový identifikátor URI, který je hello identifikátor URI, který by měl být použité toocommunicate pomocí služby Media Services.</span><span class="sxs-lookup"><span data-stu-id="840bd-152">Once you get hello new URI, that is hello URI that should be used toocommunicate with Media Services.</span></span> 
> 
> 

## <a name="media-services-learning-paths"></a><span data-ttu-id="840bd-153">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="840bd-153">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="840bd-154">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="840bd-154">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

