---
title: "Ověřování pomocí Azure AD pro přístup k rozhraní API služby Azure Media Services s .NET | Microsoft Docs"
description: "Toto téma ukazuje, jak používat ověřování Azure Active Directory (Azure AD) pro přístup k rozhraní API Azure Media Services (AMS) pomocí rozhraní .NET."
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
ms.openlocfilehash: a9355200a05a3aa1b494b76977d38ddc42bfe179
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-ad-authentication-to-access-azure-media-services-api-with-net"></a><span data-ttu-id="1d1bb-103">Používat ověřování Azure AD pro přístup k rozhraní API služby Azure Media Services pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="1d1bb-103">Use Azure AD authentication to access Azure Media Services API with .NET</span></span>

<span data-ttu-id="1d1bb-104">Počínaje windowsazure.mediaservices 4.0.0.4, Azure Media Services podporuje ověřování založené na Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1d1bb-104">Starting with windowsazure.mediaservices 4.0.0.4, Azure Media Services supports authentication based on Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="1d1bb-105">Toto téma ukazuje, jak používat ověřování Azure AD pro přístup k rozhraní API služby Azure Media Services pomocí rozhraní Microsoft .NET.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-105">This topic shows you how to use Azure AD  authentication to access Azure Media Services API with Microsoft .NET.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1d1bb-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1d1bb-106">Prerequisites</span></span>

- <span data-ttu-id="1d1bb-107">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-107">An Azure account.</span></span> <span data-ttu-id="1d1bb-108">Podrobnosti najdete na stránce [bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1d1bb-108">For details, see [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
- <span data-ttu-id="1d1bb-109">Účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-109">A Media Services account.</span></span> <span data-ttu-id="1d1bb-110">Další informace najdete v tématu [vytvoření účtu Azure Media Services pomocí webu Azure portal](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="1d1bb-110">For more information, see [Create an Azure Media Services account using the Azure portal](media-services-portal-create-account.md).</span></span>
- <span data-ttu-id="1d1bb-111">Nejnovější [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) balíčku.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-111">The latest [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) package.</span></span>
- <span data-ttu-id="1d1bb-112">Znalost tématu [přístup k Azure Media Services API s Přehled ověřování AAD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="1d1bb-112">Familiarity with the topic [Accessing Azure Media Services API with AAD authentication overview](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

<span data-ttu-id="1d1bb-113">Pokud používáte ověřování Azure AD pomocí služby Azure Media Services, můžete ověřovat v jednom ze dvou způsobů:</span><span class="sxs-lookup"><span data-stu-id="1d1bb-113">When you're using Azure AD authentication with Azure Media Services, you can authenticate in one of two ways:</span></span>

- <span data-ttu-id="1d1bb-114">**Ověření uživatele** ověřuje osoba, která používá aplikace komunikovat s prostředky Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-114">**User authentication** authenticates a person who is using the app to interact with Azure Media Services resources.</span></span> <span data-ttu-id="1d1bb-115">Interaktivní aplikace by měla nejprve vyzvou uživatele k zadání přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-115">The interactive application should first prompt the user for credentials.</span></span> <span data-ttu-id="1d1bb-116">Příkladem je konzolovou aplikaci správy, která se používá Autorizovaní uživatelé k monitorování kódování úloh nebo živé streamování.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-116">An example is a management console app that's used by authorized users to monitor encoding jobs or live streaming.</span></span> 
- <span data-ttu-id="1d1bb-117">**Objekt zabezpečení ověřování služby** ověřuje služby.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-117">**Service principal authentication** authenticates a service.</span></span> <span data-ttu-id="1d1bb-118">Aplikace, které běžně používají tuto metodu ověřování jsou aplikace, které běží služby démon, střední vrstvy služby nebo naplánované úlohy, jako jsou webové aplikace, funkce aplikací, aplikace logiky, rozhraní API nebo mikroslužeb.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-118">Applications that commonly use this authentication method are apps that run daemon services, middle-tier services, or scheduled jobs, such as web apps, function apps, logic apps, APIs, or microservices.</span></span>

>[!IMPORTANT]
><span data-ttu-id="1d1bb-119">Azure Media Service aktuálně podporuje model ověřování služby Řízení přístupu Azure.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-119">Azure Media Service currently supports an Azure Access Control Service  authentication model.</span></span> <span data-ttu-id="1d1bb-120">Řízení přístupu autorizace však bude na 1. června 2018 zastaralá.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-120">However, Access Control authorization is going to be deprecated on June 1, 2018.</span></span> <span data-ttu-id="1d1bb-121">Doporučujeme migrovat na Azure Active Directory authentication model co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-121">We recommend that you migrate to an Azure Active Directory authentication model as soon as possible.</span></span>

## <a name="get-an-azure-ad-access-token"></a><span data-ttu-id="1d1bb-122">Získání tokenu přístupu Azure AD</span><span class="sxs-lookup"><span data-stu-id="1d1bb-122">Get an Azure AD access token</span></span>

<span data-ttu-id="1d1bb-123">Pro připojení k Azure Media Services API pomocí ověřování Azure AD, klientská aplikace musí požádat o token přístupu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-123">To connect to the Azure Media Services API with Azure AD authentication, the client app needs to request an Azure AD access token.</span></span> <span data-ttu-id="1d1bb-124">Pokud používáte klienta Media Services .NET SDK, řadu podrobnosti o tom, jak získat přístupový token služby Azure AD jsou zabalená a zjednodušená pro v [AzureAdTokenProvider](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenProvider.cs) a [AzureAdTokenCredentials](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenCredentials.cs)třídy.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-124">When you use the Media Services .NET client SDK, many of the details about how to acquire an Azure AD access token are wrapped and simplified for you in the [AzureAdTokenProvider](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenProvider.cs) and [AzureAdTokenCredentials](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenCredentials.cs) classes.</span></span> 

<span data-ttu-id="1d1bb-125">Například nemusíte poskytnutí autority Azure AD, identifikátor URI prostředku Media Services nebo nativní podrobností o aplikaci Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-125">For example, you don't need to provide the Azure AD authority, Media Services resource URI, or native Azure AD application details.</span></span> <span data-ttu-id="1d1bb-126">Toto jsou známé hodnoty, které už jsou nakonfigurované v třídě zprostředkovatel tokenu přístupu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-126">These are well-known values that are already configured by the Azure AD access token provider class.</span></span> 

<span data-ttu-id="1d1bb-127">Pokud nepoužíváte .NET SDK služby Azure Media Service, doporučujeme použít [knihovna ověřování Azure AD](../active-directory/develop/active-directory-authentication-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="1d1bb-127">If you are not using Azure Media Service .NET SDK, we recommend that you use the [Azure AD Authentication Library](../active-directory/develop/active-directory-authentication-libraries.md).</span></span> <span data-ttu-id="1d1bb-128">Chcete-li získat hodnoty pro parametry, které budete muset použít s knihovna ověřování Azure AD, přečtěte si téma [používat portál Azure pro přístup k nastavení ověřování Azure AD](media-services-portal-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="1d1bb-128">To get values for the parameters that you need to use with Azure AD Authentication Library, see [Use the Azure portal to access Azure AD authentication settings](media-services-portal-get-started-with-aad.md).</span></span>

<span data-ttu-id="1d1bb-129">Máte také možnost nahradit výchozí implementaci **AzureAdTokenProvider** s vlastní implementaci.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-129">You also have the option of replacing the default implementation of the **AzureAdTokenProvider** with your own implementation.</span></span>

## <a name="install-and-configure-azure-media-services-net-sdk"></a><span data-ttu-id="1d1bb-130">Instalace a konfigurace Azure Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="1d1bb-130">Install and configure Azure Media Services .NET SDK</span></span>

>[!NOTE] 
><span data-ttu-id="1d1bb-131">Pokud chcete používat ověřování Azure AD pomocí .NET SDK služby Media Services, musíte mít nejnovější [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) balíčku.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-131">To use Azure AD authentication with the Media Services .NET SDK, you need to have the latest [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) package.</span></span> <span data-ttu-id="1d1bb-132">Také přidat odkaz na **Microsoft.IdentityModel.Clients.ActiveDirectory** sestavení.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-132">Also, add a reference to the **Microsoft.IdentityModel.Clients.ActiveDirectory** assembly.</span></span> <span data-ttu-id="1d1bb-133">Pokud používáte stávající aplikace, zahrnout **Microsoft.WindowsAzure.MediaServices.Client.Common.Authentication.dll** sestavení.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-133">If you are using an existing app, include the **Microsoft.WindowsAzure.MediaServices.Client.Common.Authentication.dll** assembly.</span></span> 

1. <span data-ttu-id="1d1bb-134">Vytvořte novou aplikaci konzoly C# v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-134">Create a new C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="1d1bb-135">Použití [windowsazure.mediaservices](https://www.nuget.org/packages/windowsazure.mediaservices) balíček NuGet k instalaci **Azure Media Services .NET SDK**.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-135">Use the [windowsazure.mediaservices](https://www.nuget.org/packages/windowsazure.mediaservices) NuGet package to install **Azure Media Services .NET SDK**.</span></span> 

    <span data-ttu-id="1d1bb-136">Přidání odkazů pomocí NuGet, proveďte následující kroky: v **Průzkumníku řešení**, klikněte pravým tlačítkem na název projektu a pak vyberte **spravovat balíčky NuGet balíčky**.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-136">To add references by using NuGet, take the following steps: in **Solution Explorer**, right-click the project name, and then select **Manage NuGet packages**.</span></span> <span data-ttu-id="1d1bb-137">Poté vyhledejte **windowsazure.mediaservices** a vyberte **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-137">Then, search for **windowsazure.mediaservices** and select **Install**.</span></span>
    
    <span data-ttu-id="1d1bb-138">-nebo-</span><span class="sxs-lookup"><span data-stu-id="1d1bb-138">-or-</span></span>

    <span data-ttu-id="1d1bb-139">Spusťte následující příkaz **Konzola správce balíčků** v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-139">Run the following command in **Package Manager Console** in Visual Studio.</span></span>

        Install-Package windowsazure.mediaservices -Version 4.0.0.4

3. <span data-ttu-id="1d1bb-140">Přidat **pomocí** ke zdrojovému kódu.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-140">Add **using** to your source code.</span></span>

        using Microsoft.WindowsAzure.MediaServices.Client; 

## <a name="use-user-authentication"></a><span data-ttu-id="1d1bb-141">Použít ověření uživatele</span><span class="sxs-lookup"><span data-stu-id="1d1bb-141">Use user authentication</span></span>

<span data-ttu-id="1d1bb-142">Pro připojení k Azure Media Service API s možností ověřování uživatelů, klientská aplikace potřebuje k vyžádání tokenu Azure AD pomocí následujících parametrů:</span><span class="sxs-lookup"><span data-stu-id="1d1bb-142">To connect to the Azure Media Service API with the user authentication option, the client app needs to request an Azure AD token by using the following parameters:</span></span>  

- <span data-ttu-id="1d1bb-143">Koncový bod pro klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-143">Azure AD tenant endpoint.</span></span> <span data-ttu-id="1d1bb-144">Informace o klienta můžete načíst z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-144">The tenant information can be retrieved from the Azure portal.</span></span> <span data-ttu-id="1d1bb-145">Podržte ukazatel nad přihlášeného uživatele v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-145">Hover over the signed-in user in the upper-right corner.</span></span>
- <span data-ttu-id="1d1bb-146">Identifikátor URI prostředku Media Services.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-146">Media Services resource URI.</span></span>
- <span data-ttu-id="1d1bb-147">ID klienta aplikace Media Services (nativní).</span><span class="sxs-lookup"><span data-stu-id="1d1bb-147">Media Services (native) application client ID.</span></span> 
- <span data-ttu-id="1d1bb-148">Identifikátor URI pro přesměrování služby Media Services (nativní) aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-148">Media Services (native) application redirect URI.</span></span> 

<span data-ttu-id="1d1bb-149">Hodnoty pro tyto parametry lze nalézt v **AzureEnvironments.AzureCloudEnvironment**.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-149">The values for these parameters can be found in **AzureEnvironments.AzureCloudEnvironment**.</span></span> <span data-ttu-id="1d1bb-150">**AzureEnvironments.AzureCloudEnvironment** konstanta je pomocné rutiny v sadě SDK .NET pro získání správné prostředí proměnné nastavení pro veřejné datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-150">The **AzureEnvironments.AzureCloudEnvironment** constant is a helper in the .NET SDK to get the right environment variable settings for a public Azure Data Center.</span></span> 

<span data-ttu-id="1d1bb-151">Obsahuje nastavení předem definované prostředí pro přístup k Media Services v pouze veřejné datových centrech.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-151">It contains pre-defined environment settings for accessing Media Services in the public data centers only.</span></span> <span data-ttu-id="1d1bb-152">Pro suverénní nebo státní cloudových oblastí, můžete použít **AzureChinaCloudEnvironment**, **AzureUsGovernmentEnvrionment**, nebo **AzureGermanCloudEnvironment** v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-152">For sovereign or government cloud regions, you can use **AzureChinaCloudEnvironment**, **AzureUsGovernmentEnvrionment**, or **AzureGermanCloudEnvironment** respectively.</span></span>

<span data-ttu-id="1d1bb-153">Následující příklad kódu vytvoří token:</span><span class="sxs-lookup"><span data-stu-id="1d1bb-153">The following code example creates a token:</span></span>
    
    var tokenCredentials = new AzureAdTokenCredentials("microsoft.onmicrosoft.com", AzureEnvironments.AzureCloudEnvironment);
    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
  
<span data-ttu-id="1d1bb-154">Pokud chcete spustit programové ošetření Media Services, musíte vytvořit **CloudMediaContext** instanci, která představuje kontext serveru.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-154">To start programming against Media Services, you need to create a **CloudMediaContext** instance that represents the server context.</span></span> <span data-ttu-id="1d1bb-155">**CloudMediaContext** obsahuje odkazy na důležité kolekcí včetně úloh, prostředky, soubory, zásady přístupu a lokátory.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-155">The **CloudMediaContext** includes references to important collections including jobs, assets, files, access policies, and locators.</span></span> 

<span data-ttu-id="1d1bb-156">Musíte také předat **identifikátor URI pro Media REST Services** k **CloudMediaContext** konstruktor.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-156">You also need to pass the **resource URI for Media REST Services** to the **CloudMediaContext** constructor.</span></span> <span data-ttu-id="1d1bb-157">Chcete-li získat identifikátor URI pro Media Services REST, přihlášený k portálu Azure vyberte svůj účet Azure Media Services, vyberte **přístup pomocí rozhraní API**a potom vyberte **připojit k Azure Media Services s ověřováním uživatele**.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-157">To get the resource URI for Media REST Services, sign in to the Azure portal, select your Azure Media Services account, select **API access**, and then select **Connect to Azure Media Services with user authentication**.</span></span> 

<span data-ttu-id="1d1bb-158">Následující příklad kódu vytvoří **CloudMediaContext** instance:</span><span class="sxs-lookup"><span data-stu-id="1d1bb-158">The following code example creates a **CloudMediaContext** instance:</span></span>

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);

<span data-ttu-id="1d1bb-159">Následující příklad ukazuje, jak vytvořit token Azure AD a kontextu:</span><span class="sxs-lookup"><span data-stu-id="1d1bb-159">The following example shows how to create the Azure AD token and the context:</span></span>

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
><span data-ttu-id="1d1bb-160">Pokud dojde k výjimce, která uvádí, že "vzdálený server vrátil chybu: (401) Neautorizováno" v tématu [řízení přístupu](media-services-use-aad-auth-to-access-ams-api.md#access-control) části přístup k API služby Azure Media Services s Přehled ověřování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-160">If you get an exception that says "The remote server returned an error: (401) Unauthorized," see the [Access control](media-services-use-aad-auth-to-access-ams-api.md#access-control) section of Accessing Azure Media Services API with Azure AD authentication overview.</span></span>

## <a name="use-service-principal-authentication"></a><span data-ttu-id="1d1bb-161">Objekt zabezpečení ověřování pomocí služby</span><span class="sxs-lookup"><span data-stu-id="1d1bb-161">Use service principal authentication</span></span>
    
<span data-ttu-id="1d1bb-162">Pro připojení k rozhraní API služby Azure Media Services s hlavní možností služby, aplikace střední vrstvy (webové rozhraní API nebo webovou aplikaci) musí požadavky tokenu Azure AD s následujícími parametry:</span><span class="sxs-lookup"><span data-stu-id="1d1bb-162">To connect to the Azure Media Services API with the service principal option, your middle-tier app (web API or web application) needs to requests an Azure AD token with the following parameters:</span></span>  

- <span data-ttu-id="1d1bb-163">Koncový bod pro klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-163">Azure AD tenant endpoint.</span></span> <span data-ttu-id="1d1bb-164">Informace o klienta můžete načíst z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-164">The tenant information can be retrieved from the Azure portal.</span></span> <span data-ttu-id="1d1bb-165">Podržte ukazatel nad přihlášeného uživatele v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-165">Hover over the signed-in user in the upper-right corner.</span></span>
- <span data-ttu-id="1d1bb-166">Identifikátor URI prostředku Media Services.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-166">Media Services resource URI.</span></span>
- <span data-ttu-id="1d1bb-167">Hodnoty aplikace Azure AD: **ID klienta** a **tajný klíč klienta**.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-167">Azure AD application values: the **Client ID** and **Client secret**.</span></span>

<span data-ttu-id="1d1bb-168">Hodnoty **ID klienta** a **tajný klíč klienta** parametry lze nalézt v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-168">The values for the **Client ID** and **Client secret** parameters can be found in the Azure portal.</span></span> <span data-ttu-id="1d1bb-169">Další informace najdete v tématu [Začínáme s ověřováním Azure AD pomocí portálu Azure](media-services-portal-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="1d1bb-169">For more information, see [Getting started with Azure AD authentication using the Azure portal](media-services-portal-get-started-with-aad.md).</span></span>

<span data-ttu-id="1d1bb-170">Následující příklad kódu vytvoří token pomocí **AzureAdTokenCredentials** konstruktor, který přebírá **AzureAdClientSymmetricKey** jako parametr:</span><span class="sxs-lookup"><span data-stu-id="1d1bb-170">The following code example creates a token by using the **AzureAdTokenCredentials** constructor that takes **AzureAdClientSymmetricKey** as a parameter:</span></span> 
    
    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientSymmetricKey("{YOUR CLIENT ID HERE}", "{YOUR CLIENT SECRET}"), 
                                AzureEnvironments.AzureCloudEnvironment);

    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

<span data-ttu-id="1d1bb-171">Můžete také určit **AzureAdTokenCredentials** konstruktor, který přebírá **AzureAdClientCertificate** jako parametr.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-171">You can also specify the **AzureAdTokenCredentials** constructor that takes **AzureAdClientCertificate** as a parameter.</span></span> 

<span data-ttu-id="1d1bb-172">Pokyny o tom, jak vytvořit a nakonfigurovat certifikát ve formuláři, který můžete použít Azure AD najdete v tématu [ověřování do služby Azure AD v aplikacích démon s certifikáty - postup ruční konfigurace](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md).</span><span class="sxs-lookup"><span data-stu-id="1d1bb-172">For instructions about how to create and configure a certificate in a form that can be used by Azure AD, see [Authenticating to Azure AD in daemon apps with certificates - manual configuration steps](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md).</span></span>

    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientCertificate("{YOUR CLIENT ID HERE}", "{YOUR CLIENT CERTIFICATE THUMBPRINT}"), 
                                AzureEnvironments.AzureCloudEnvironment);

<span data-ttu-id="1d1bb-173">Pokud chcete spustit programové ošetření Media Services, musíte vytvořit **CloudMediaContext** instanci, která představuje kontext serveru.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-173">To start programming against Media Services, you need to create a **CloudMediaContext** instance that represents the server context.</span></span> <span data-ttu-id="1d1bb-174">Musíte také předat **identifikátor URI pro Media REST Services** k **CloudMediaContext** konstruktor.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-174">You also need to pass the **resource URI for Media REST Services** to the **CloudMediaContext** constructor.</span></span> <span data-ttu-id="1d1bb-175">Můžete získat **identifikátor URI pro Media REST Services** hodnotu z webu Azure portal.</span><span class="sxs-lookup"><span data-stu-id="1d1bb-175">You can get the **resource URI for Media REST Services** value from the Azure portal as well.</span></span>

<span data-ttu-id="1d1bb-176">Následující příklad kódu vytvoří **CloudMediaContext** instance:</span><span class="sxs-lookup"><span data-stu-id="1d1bb-176">The following code example creates a **CloudMediaContext** instance:</span></span>

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
<span data-ttu-id="1d1bb-177">Následující příklad ukazuje, jak vytvořit token Azure AD a kontextu:</span><span class="sxs-lookup"><span data-stu-id="1d1bb-177">The following example shows how to create the Azure AD token and the context:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="1d1bb-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d1bb-178">Next steps</span></span>

<span data-ttu-id="1d1bb-179">Začínáme s [nahrávání souborů do účtu](media-services-dotnet-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="1d1bb-179">Get started with [uploading files to your account](media-services-dotnet-upload-files.md).</span></span>
