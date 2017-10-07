---
title: "aaaUse Azure AD authentication tooaccess rozhraní API služby Azure Media Services s .NET | Microsoft Docs"
description: "Toto téma ukazuje, jak toouse tooaccess ověřování Azure Active Directory (Azure AD) Azure Media Services (AMS) rozhraní API pomocí rozhraní .NET."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: 2f750e460d9e476ad92e96adeac6500cb692cd77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-ad-authentication-tooaccess-azure-media-services-api-with-net"></a><span data-ttu-id="c6bb3-103">Azure AD authentication tooaccess rozhraní API služby Azure Media Services pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="c6bb3-103">Use Azure AD authentication tooaccess Azure Media Services API with .NET</span></span>

<span data-ttu-id="c6bb3-104">Počínaje windowsazure.mediaservices 4.0.0.4, Azure Media Services podporuje ověřování založené na Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c6bb3-104">Starting with windowsazure.mediaservices 4.0.0.4, Azure Media Services supports authentication based on Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="c6bb3-105">Toto téma ukazuje, jak toouse Azure AD authentication tooaccess rozhraní API služby Azure Media Services pomocí rozhraní Microsoft .NET.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-105">This topic shows you how toouse Azure AD  authentication tooaccess Azure Media Services API with Microsoft .NET.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c6bb3-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c6bb3-106">Prerequisites</span></span>

- <span data-ttu-id="c6bb3-107">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-107">An Azure account.</span></span> <span data-ttu-id="c6bb3-108">Podrobnosti najdete na stránce [bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c6bb3-108">For details, see [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
- <span data-ttu-id="c6bb3-109">Účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-109">A Media Services account.</span></span> <span data-ttu-id="c6bb3-110">Další informace najdete v tématu [vytvoření účtu Azure Media Services pomocí portálu Azure hello](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="c6bb3-110">For more information, see [Create an Azure Media Services account using hello Azure portal](media-services-portal-create-account.md).</span></span>
- <span data-ttu-id="c6bb3-111">Hello nejnovější [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) balíčku.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-111">hello latest [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) package.</span></span>
- <span data-ttu-id="c6bb3-112">Znalost hello tématu [přístup k Azure Media Services API s Přehled ověřování AAD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="c6bb3-112">Familiarity with hello topic [Accessing Azure Media Services API with AAD authentication overview](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

<span data-ttu-id="c6bb3-113">Pokud používáte ověřování Azure AD pomocí služby Azure Media Services, můžete ověřovat v jednom ze dvou způsobů:</span><span class="sxs-lookup"><span data-stu-id="c6bb3-113">When you're using Azure AD authentication with Azure Media Services, you can authenticate in one of two ways:</span></span>

- <span data-ttu-id="c6bb3-114">**Ověření uživatele** ověřuje osoba, která používá toointeract aplikace hello s prostředky Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-114">**User authentication** authenticates a person who is using hello app toointeract with Azure Media Services resources.</span></span> <span data-ttu-id="c6bb3-115">interaktivní aplikace Hello měli nejdřív hello uživatele vyzve k přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-115">hello interactive application should first prompt hello user for credentials.</span></span> <span data-ttu-id="c6bb3-116">Příkladem je konzolovou aplikaci správy, který je používán oprávněným uživatelům toomonitor kódování úlohy nebo živé streamování.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-116">An example is a management console app that's used by authorized users toomonitor encoding jobs or live streaming.</span></span> 
- <span data-ttu-id="c6bb3-117">**Objekt zabezpečení ověřování služby** ověřuje služby.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-117">**Service principal authentication** authenticates a service.</span></span> <span data-ttu-id="c6bb3-118">Aplikace, které běžně používají tuto metodu ověřování jsou aplikace, které běží služby démon, střední vrstvy služby nebo naplánované úlohy, jako jsou webové aplikace, funkce aplikací, aplikace logiky, rozhraní API nebo mikroslužeb.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-118">Applications that commonly use this authentication method are apps that run daemon services, middle-tier services, or scheduled jobs, such as web apps, function apps, logic apps, APIs, or microservices.</span></span>

>[!IMPORTANT]
><span data-ttu-id="c6bb3-119">Azure Media Service aktuálně podporuje model ověřování služby Řízení přístupu Azure.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-119">Azure Media Service currently supports an Azure Access Control Service  authentication model.</span></span> <span data-ttu-id="c6bb3-120">Řízení přístupu autorizace však bude toobe zastaralé na 1. června 2018.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-120">However, Access Control authorization is going toobe deprecated on June 1, 2018.</span></span> <span data-ttu-id="c6bb3-121">Doporučujeme, abyste co nejdříve migraci tooan model ověřování Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-121">We recommend that you migrate tooan Azure Active Directory authentication model as soon as possible.</span></span>

## <a name="get-an-azure-ad-access-token"></a><span data-ttu-id="c6bb3-122">Získání tokenu přístupu Azure AD</span><span class="sxs-lookup"><span data-stu-id="c6bb3-122">Get an Azure AD access token</span></span>

<span data-ttu-id="c6bb3-123">tooconnect toohello rozhraní API služby Azure Media Services pomocí ověřování Azure AD, klientská aplikace hello musí toorequest přístupový token služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-123">tooconnect toohello Azure Media Services API with Azure AD authentication, hello client app needs toorequest an Azure AD access token.</span></span> <span data-ttu-id="c6bb3-124">Při použití klienta hello Media Services .NET SDK, řadu hello podrobnosti o tom, jak jsou tooacquire přístupový token služby Azure AD zabalené a zjednodušená pro vás v hello [AzureAdTokenProvider](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenProvider.cs) a [AzureAdTokenCredentials ](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenCredentials.cs) třídy.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-124">When you use hello Media Services .NET client SDK, many of hello details about how tooacquire an Azure AD access token are wrapped and simplified for you in hello [AzureAdTokenProvider](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenProvider.cs) and [AzureAdTokenCredentials](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenCredentials.cs) classes.</span></span> 

<span data-ttu-id="c6bb3-125">Například nepotřebujete autority tooprovide hello Azure AD, identifikátor URI prostředku Media Services nebo nativní podrobností o aplikaci Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-125">For example, you don't need tooprovide hello Azure AD authority, Media Services resource URI, or native Azure AD application details.</span></span> <span data-ttu-id="c6bb3-126">Toto jsou známé hodnoty, které už jsou nakonfigurované hello třídy zprostředkovatele tokenu přístupu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-126">These are well-known values that are already configured by hello Azure AD access token provider class.</span></span> 

<span data-ttu-id="c6bb3-127">Pokud nepoužíváte .NET SDK služby Azure Media Service, doporučujeme použít hello [knihovna ověřování Azure AD](../active-directory/develop/active-directory-authentication-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="c6bb3-127">If you are not using Azure Media Service .NET SDK, we recommend that you use hello [Azure AD Authentication Library](../active-directory/develop/active-directory-authentication-libraries.md).</span></span> <span data-ttu-id="c6bb3-128">tooget hodnoty parametrů hello, je nutné, aby toouse s knihovna ověřování Azure AD, najdete v části [použít nastavení ověřování Azure tooaccess portálu Azure AD hello](media-services-portal-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="c6bb3-128">tooget values for hello parameters that you need toouse with Azure AD Authentication Library, see [Use hello Azure portal tooaccess Azure AD authentication settings](media-services-portal-get-started-with-aad.md).</span></span>

<span data-ttu-id="c6bb3-129">Máte také možnost hello nahrazení hello výchozí implementaci třídy hello **AzureAdTokenProvider** s vlastní implementaci.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-129">You also have hello option of replacing hello default implementation of hello **AzureAdTokenProvider** with your own implementation.</span></span>

## <a name="install-and-configure-azure-media-services-net-sdk"></a><span data-ttu-id="c6bb3-130">Instalace a konfigurace Azure Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="c6bb3-130">Install and configure Azure Media Services .NET SDK</span></span>

>[!NOTE] 
><span data-ttu-id="c6bb3-131">ověřování toouse Azure AD s hello sady Media Services .NET SDK, je nutné toohave hello nejnovější [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) balíčku.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-131">toouse Azure AD authentication with hello Media Services .NET SDK, you need toohave hello latest [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) package.</span></span> <span data-ttu-id="c6bb3-132">Také přidat odkaz na toohello **Microsoft.IdentityModel.Clients.ActiveDirectory** sestavení.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-132">Also, add a reference toohello **Microsoft.IdentityModel.Clients.ActiveDirectory** assembly.</span></span> <span data-ttu-id="c6bb3-133">Pokud používáte stávající aplikace, zahrnout hello **Microsoft.WindowsAzure.MediaServices.Client.Common.Authentication.dll** sestavení.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-133">If you are using an existing app, include hello **Microsoft.WindowsAzure.MediaServices.Client.Common.Authentication.dll** assembly.</span></span> 

1. <span data-ttu-id="c6bb3-134">Vytvořte novou aplikaci konzoly C# v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-134">Create a new C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="c6bb3-135">Použití hello [windowsazure.mediaservices](https://www.nuget.org/packages/windowsazure.mediaservices) tooinstall balíček NuGet **Azure Media Services .NET SDK**.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-135">Use hello [windowsazure.mediaservices](https://www.nuget.org/packages/windowsazure.mediaservices) NuGet package tooinstall **Azure Media Services .NET SDK**.</span></span> 

    <span data-ttu-id="c6bb3-136">odkazy na tooadd pomocí NuGet, trvat hello následující kroky: v **Průzkumníku řešení**, klikněte pravým tlačítkem na název projektu hello a pak vyberte **spravovat balíčky NuGet balíčky**.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-136">tooadd references by using NuGet, take hello following steps: in **Solution Explorer**, right-click hello project name, and then select **Manage NuGet packages**.</span></span> <span data-ttu-id="c6bb3-137">Poté vyhledejte **windowsazure.mediaservices** a vyberte **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-137">Then, search for **windowsazure.mediaservices** and select **Install**.</span></span>
    
    <span data-ttu-id="c6bb3-138">-nebo-</span><span class="sxs-lookup"><span data-stu-id="c6bb3-138">-or-</span></span>

    <span data-ttu-id="c6bb3-139">Spuštění hello následující příkaz v **Konzola správce balíčků** v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-139">Run hello following command in **Package Manager Console** in Visual Studio.</span></span>

        Install-Package windowsazure.mediaservices -Version 4.0.0.4

3. <span data-ttu-id="c6bb3-140">Přidat **pomocí** tooyour zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-140">Add **using** tooyour source code.</span></span>

        using Microsoft.WindowsAzure.MediaServices.Client; 

## <a name="use-user-authentication"></a><span data-ttu-id="c6bb3-141">Použít ověření uživatele</span><span class="sxs-lookup"><span data-stu-id="c6bb3-141">Use user authentication</span></span>

<span data-ttu-id="c6bb3-142">tooconnect toohello rozhraní API služby Azure Media s možností ověřování uživatele hello, klientská aplikace hello potřebuje toorequest tokenu Azure AD pomocí hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="c6bb3-142">tooconnect toohello Azure Media Service API with hello user authentication option, hello client app needs toorequest an Azure AD token by using hello following parameters:</span></span>  

- <span data-ttu-id="c6bb3-143">Koncový bod pro klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-143">Azure AD tenant endpoint.</span></span> <span data-ttu-id="c6bb3-144">informace o Hello klienta lze načíst z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-144">hello tenant information can be retrieved from hello Azure portal.</span></span> <span data-ttu-id="c6bb3-145">Podržte ukazatel nad hello přihlášeného uživatele v pravém horním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-145">Hover over hello signed-in user in hello upper-right corner.</span></span>
- <span data-ttu-id="c6bb3-146">Identifikátor URI prostředku Media Services.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-146">Media Services resource URI.</span></span>
- <span data-ttu-id="c6bb3-147">ID klienta aplikace Media Services (nativní).</span><span class="sxs-lookup"><span data-stu-id="c6bb3-147">Media Services (native) application client ID.</span></span> 
- <span data-ttu-id="c6bb3-148">Identifikátor URI pro přesměrování služby Media Services (nativní) aplikace.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-148">Media Services (native) application redirect URI.</span></span> 

<span data-ttu-id="c6bb3-149">Hello hodnoty pro tyto parametry lze nalézt v **AzureEnvironments.AzureCloudEnvironment**.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-149">hello values for these parameters can be found in **AzureEnvironments.AzureCloudEnvironment**.</span></span> <span data-ttu-id="c6bb3-150">Hello **AzureEnvironments.AzureCloudEnvironment** konstanta pomocné rutiny v tooget hello .NET SDK je hello nastavení proměnné prostředí správné pro veřejné datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-150">hello **AzureEnvironments.AzureCloudEnvironment** constant is a helper in hello .NET SDK tooget hello right environment variable settings for a public Azure Data Center.</span></span> 

<span data-ttu-id="c6bb3-151">Obsahuje nastavení předem definované prostředí pro přístup k Media Services v pouze hello veřejné datových centrech.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-151">It contains pre-defined environment settings for accessing Media Services in hello public data centers only.</span></span> <span data-ttu-id="c6bb3-152">Pro suverénní nebo státní cloudových oblastí, můžete použít **AzureChinaCloudEnvironment**, **AzureUsGovernmentEnvrionment**, nebo **AzureGermanCloudEnvironment** v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-152">For sovereign or government cloud regions, you can use **AzureChinaCloudEnvironment**, **AzureUsGovernmentEnvrionment**, or **AzureGermanCloudEnvironment** respectively.</span></span>

<span data-ttu-id="c6bb3-153">Následující ukázka kódu Hello vytvoří token:</span><span class="sxs-lookup"><span data-stu-id="c6bb3-153">hello following code example creates a token:</span></span>
    
    var tokenCredentials = new AzureAdTokenCredentials("microsoft.onmicrosoft.com", AzureEnvironments.AzureCloudEnvironment);
    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
  
<span data-ttu-id="c6bb3-154">toostart programové ošetření Media Services, budete potřebovat toocreate **CloudMediaContext** instanci, která představuje kontext server hello.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-154">toostart programming against Media Services, you need toocreate a **CloudMediaContext** instance that represents hello server context.</span></span> <span data-ttu-id="c6bb3-155">Hello **CloudMediaContext** obsahuje odkazy na kolekce tooimportant včetně úloh, prostředky, soubory, zásady přístupu a lokátory.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-155">hello **CloudMediaContext** includes references tooimportant collections including jobs, assets, files, access policies, and locators.</span></span> 

<span data-ttu-id="c6bb3-156">Musíte taky toopass hello **identifikátor URI pro Media REST Services** toohello **CloudMediaContext** konstruktor.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-156">You also need toopass hello **resource URI for Media REST Services** toohello **CloudMediaContext** constructor.</span></span> <span data-ttu-id="c6bb3-157">identifikátor URI prostředku hello tooget pro Media Services REST, přihlášení toohello portál Azure, vyberte svůj účet Azure Media Services, vyberte **přístup pomocí rozhraní API**a potom vyberte **připojit tooAzure Media Services s uživatelem ověřování**.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-157">tooget hello resource URI for Media REST Services, sign in toohello Azure portal, select your Azure Media Services account, select **API access**, and then select **Connect tooAzure Media Services with user authentication**.</span></span> 

<span data-ttu-id="c6bb3-158">Hello následující příklad kódu vytvoří **CloudMediaContext** instance:</span><span class="sxs-lookup"><span data-stu-id="c6bb3-158">hello following code example creates a **CloudMediaContext** instance:</span></span>

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);

<span data-ttu-id="c6bb3-159">Hello následující příklad ukazuje, jak toocreate hello token a hello kontextu Azure AD:</span><span class="sxs-lookup"><span data-stu-id="c6bb3-159">hello following example shows how toocreate hello Azure AD token and hello context:</span></span>

    namespace AADAuthSample
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Specify your Azure AD tenant domain, for example "microsoft.onmicrosoft.com".
                var tokenCredentials = new AzureAdTokenCredentials("{YOUR AAD TENANT DOMAIN HERE}", AzureEnvironments.AzureCloudEnvironment);
    
                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
    
                // Specify your REST API endpoint, for example "https://accountname.restv2.westcentralus.media.azure.net/API".
                CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
                var assets = context.Assets;
                foreach (var a in assets)
                {
                    Console.WriteLine(a.Name);
                }
            }
    
        }
    }

>[!NOTE]
><span data-ttu-id="c6bb3-160">Pokud dojde k výjimce, která uvádí, že "hello na vzdálený server vrátil chybu: (401) Neautorizováno" v tématu hello [řízení přístupu](media-services-use-aad-auth-to-access-ams-api.md#access-control) části přístup k API služby Azure Media Services s Přehled ověřování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-160">If you get an exception that says "hello remote server returned an error: (401) Unauthorized," see hello [Access control](media-services-use-aad-auth-to-access-ams-api.md#access-control) section of Accessing Azure Media Services API with Azure AD authentication overview.</span></span>

## <a name="use-service-principal-authentication"></a><span data-ttu-id="c6bb3-161">Objekt zabezpečení ověřování pomocí služby</span><span class="sxs-lookup"><span data-stu-id="c6bb3-161">Use service principal authentication</span></span>
    
<span data-ttu-id="c6bb3-162">tooconnect toohello rozhraní API služby Azure Media Services s možností hlavní služby hello, vaše aplikace střední vrstvy (webového rozhraní API nebo webové aplikace) musí toorequests tokenu Azure AD s hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="c6bb3-162">tooconnect toohello Azure Media Services API with hello service principal option, your middle-tier app (web API or web application) needs toorequests an Azure AD token with hello following parameters:</span></span>  

- <span data-ttu-id="c6bb3-163">Koncový bod pro klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-163">Azure AD tenant endpoint.</span></span> <span data-ttu-id="c6bb3-164">informace o Hello klienta lze načíst z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-164">hello tenant information can be retrieved from hello Azure portal.</span></span> <span data-ttu-id="c6bb3-165">Podržte ukazatel nad hello přihlášeného uživatele v pravém horním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-165">Hover over hello signed-in user in hello upper-right corner.</span></span>
- <span data-ttu-id="c6bb3-166">Identifikátor URI prostředku Media Services.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-166">Media Services resource URI.</span></span>
- <span data-ttu-id="c6bb3-167">Hodnoty aplikace Azure AD: hello **ID klienta** a **tajný klíč klienta**.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-167">Azure AD application values: hello **Client ID** and **Client secret**.</span></span>

<span data-ttu-id="c6bb3-168">Hello hodnoty pro hello **ID klienta** a **tajný klíč klienta** parametry lze nalézt v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-168">hello values for hello **Client ID** and **Client secret** parameters can be found in hello Azure portal.</span></span> <span data-ttu-id="c6bb3-169">Další informace najdete v tématu [Začínáme s Azure AD authentication pomocí portálu Azure hello](media-services-portal-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="c6bb3-169">For more information, see [Getting started with Azure AD authentication using hello Azure portal](media-services-portal-get-started-with-aad.md).</span></span>

<span data-ttu-id="c6bb3-170">Hello následující příklad kódu vytvoří token pomocí hello **AzureAdTokenCredentials** konstruktor, který přebírá **AzureAdClientSymmetricKey** jako parametr:</span><span class="sxs-lookup"><span data-stu-id="c6bb3-170">hello following code example creates a token by using hello **AzureAdTokenCredentials** constructor that takes **AzureAdClientSymmetricKey** as a parameter:</span></span> 
    
    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientSymmetricKey("{YOUR CLIENT ID HERE}", "{YOUR CLIENT SECRET}"), 
                                AzureEnvironments.AzureCloudEnvironment);

    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

<span data-ttu-id="c6bb3-171">Můžete také zadat hello **AzureAdTokenCredentials** konstruktor, který přebírá **AzureAdClientCertificate** jako parametr.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-171">You can also specify hello **AzureAdTokenCredentials** constructor that takes **AzureAdClientCertificate** as a parameter.</span></span> 

<span data-ttu-id="c6bb3-172">Další informace o tom toocreate a konfiguraci certifikátu v formulář, který lze použít Azure AD, najdete v části [ověřování tooAzure AD v aplikacích démon s certifikáty - postup ruční konfigurace](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md).</span><span class="sxs-lookup"><span data-stu-id="c6bb3-172">For instructions about how toocreate and configure a certificate in a form that can be used by Azure AD, see [Authenticating tooAzure AD in daemon apps with certificates - manual configuration steps](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md).</span></span>

    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientCertificate("{YOUR CLIENT ID HERE}", "{YOUR CLIENT CERTIFICATE THUMBPRINT}"), 
                                AzureEnvironments.AzureCloudEnvironment);

<span data-ttu-id="c6bb3-173">toostart programové ošetření Media Services, budete potřebovat toocreate **CloudMediaContext** instanci, která představuje kontext server hello.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-173">toostart programming against Media Services, you need toocreate a **CloudMediaContext** instance that represents hello server context.</span></span> <span data-ttu-id="c6bb3-174">Musíte taky toopass hello **identifikátor URI pro Media REST Services** toohello **CloudMediaContext** konstruktor.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-174">You also need toopass hello **resource URI for Media REST Services** toohello **CloudMediaContext** constructor.</span></span> <span data-ttu-id="c6bb3-175">Můžete získat hello **identifikátor URI pro Media REST Services** hodnotu z hello také portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c6bb3-175">You can get hello **resource URI for Media REST Services** value from hello Azure portal as well.</span></span>

<span data-ttu-id="c6bb3-176">Hello následující příklad kódu vytvoří **CloudMediaContext** instance:</span><span class="sxs-lookup"><span data-stu-id="c6bb3-176">hello following code example creates a **CloudMediaContext** instance:</span></span>

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
<span data-ttu-id="c6bb3-177">Hello následující příklad ukazuje, jak toocreate hello token a hello kontextu Azure AD:</span><span class="sxs-lookup"><span data-stu-id="c6bb3-177">hello following example shows how toocreate hello Azure AD token and hello context:</span></span>

    namespace AADAuthSample
    {
    
        class Program
        {
            static void Main(string[] args)
            {
                var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                            new AzureAdClientSymmetricKey("{YOUR CLIENT ID HERE}", "{YOUR CLIENT SECRET}"), 
                                            AzureEnvironments.AzureCloudEnvironment);
            
                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
    
                // Specify your REST API endpoint, for example "https://accountname.restv2.westcentralus.media.azure.net/API".      
                CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
                var assets = context.Assets;
                foreach (var a in assets)
                {
                    Console.WriteLine(a.Name);
                }
    
                Console.ReadLine();
            }
    
        }
    }

## <a name="next-steps"></a><span data-ttu-id="c6bb3-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c6bb3-178">Next steps</span></span>

<span data-ttu-id="c6bb3-179">Začínáme s [nahrávání souborů tooyour účet](media-services-dotnet-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="c6bb3-179">Get started with [uploading files tooyour account](media-services-dotnet-upload-files.md).</span></span>
