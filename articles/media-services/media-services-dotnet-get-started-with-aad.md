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
# <a name="use-azure-ad-authentication-tooaccess-azure-media-services-api-with-net"></a>Azure AD authentication tooaccess rozhraní API služby Azure Media Services pomocí rozhraní .NET

Počínaje windowsazure.mediaservices 4.0.0.4, Azure Media Services podporuje ověřování založené na Azure Active Directory (Azure AD). Toto téma ukazuje, jak toouse Azure AD authentication tooaccess rozhraní API služby Azure Media Services pomocí rozhraní Microsoft .NET.

## <a name="prerequisites"></a>Požadavky

- Účet Azure. Podrobnosti najdete na stránce [bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/). 
- Účet Media Services. Další informace najdete v tématu [vytvoření účtu Azure Media Services pomocí portálu Azure hello](media-services-portal-create-account.md).
- Hello nejnovější [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) balíčku.
- Znalost hello tématu [přístup k Azure Media Services API s Přehled ověřování AAD](media-services-use-aad-auth-to-access-ams-api.md). 

Pokud používáte ověřování Azure AD pomocí služby Azure Media Services, můžete ověřovat v jednom ze dvou způsobů:

- **Ověření uživatele** ověřuje osoba, která používá toointeract aplikace hello s prostředky Azure Media Services. interaktivní aplikace Hello měli nejdřív hello uživatele vyzve k přihlašovací údaje. Příkladem je konzolovou aplikaci správy, který je používán oprávněným uživatelům toomonitor kódování úlohy nebo živé streamování. 
- **Objekt zabezpečení ověřování služby** ověřuje služby. Aplikace, které běžně používají tuto metodu ověřování jsou aplikace, které běží služby démon, střední vrstvy služby nebo naplánované úlohy, jako jsou webové aplikace, funkce aplikací, aplikace logiky, rozhraní API nebo mikroslužeb.

>[!IMPORTANT]
>Azure Media Service aktuálně podporuje model ověřování služby Řízení přístupu Azure. Řízení přístupu autorizace však bude toobe zastaralé na 1. června 2018. Doporučujeme, abyste co nejdříve migraci tooan model ověřování Azure Active Directory.

## <a name="get-an-azure-ad-access-token"></a>Získání tokenu přístupu Azure AD

tooconnect toohello rozhraní API služby Azure Media Services pomocí ověřování Azure AD, klientská aplikace hello musí toorequest přístupový token služby Azure AD. Při použití klienta hello Media Services .NET SDK, řadu hello podrobnosti o tom, jak jsou tooacquire přístupový token služby Azure AD zabalené a zjednodušená pro vás v hello [AzureAdTokenProvider](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenProvider.cs) a [AzureAdTokenCredentials ](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenCredentials.cs) třídy. 

Například nepotřebujete autority tooprovide hello Azure AD, identifikátor URI prostředku Media Services nebo nativní podrobností o aplikaci Azure AD. Toto jsou známé hodnoty, které už jsou nakonfigurované hello třídy zprostředkovatele tokenu přístupu Azure AD. 

Pokud nepoužíváte .NET SDK služby Azure Media Service, doporučujeme použít hello [knihovna ověřování Azure AD](../active-directory/develop/active-directory-authentication-libraries.md). tooget hodnoty parametrů hello, je nutné, aby toouse s knihovna ověřování Azure AD, najdete v části [použít nastavení ověřování Azure tooaccess portálu Azure AD hello](media-services-portal-get-started-with-aad.md).

Máte také možnost hello nahrazení hello výchozí implementaci třídy hello **AzureAdTokenProvider** s vlastní implementaci.

## <a name="install-and-configure-azure-media-services-net-sdk"></a>Instalace a konfigurace Azure Media Services .NET SDK

>[!NOTE] 
>ověřování toouse Azure AD s hello sady Media Services .NET SDK, je nutné toohave hello nejnovější [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) balíčku. Také přidat odkaz na toohello **Microsoft.IdentityModel.Clients.ActiveDirectory** sestavení. Pokud používáte stávající aplikace, zahrnout hello **Microsoft.WindowsAzure.MediaServices.Client.Common.Authentication.dll** sestavení. 

1. Vytvořte novou aplikaci konzoly C# v sadě Visual Studio.
2. Použití hello [windowsazure.mediaservices](https://www.nuget.org/packages/windowsazure.mediaservices) tooinstall balíček NuGet **Azure Media Services .NET SDK**. 

    odkazy na tooadd pomocí NuGet, trvat hello následující kroky: v **Průzkumníku řešení**, klikněte pravým tlačítkem na název projektu hello a pak vyberte **spravovat balíčky NuGet balíčky**. Poté vyhledejte **windowsazure.mediaservices** a vyberte **nainstalovat**.
    
    -nebo-

    Spuštění hello následující příkaz v **Konzola správce balíčků** v sadě Visual Studio.

        Install-Package windowsazure.mediaservices -Version 4.0.0.4

3. Přidat **pomocí** tooyour zdrojového kódu.

        using Microsoft.WindowsAzure.MediaServices.Client; 

## <a name="use-user-authentication"></a>Použít ověření uživatele

tooconnect toohello rozhraní API služby Azure Media s možností ověřování uživatele hello, klientská aplikace hello potřebuje toorequest tokenu Azure AD pomocí hello následující parametry:  

- Koncový bod pro klienta Azure AD. informace o Hello klienta lze načíst z hello portálu Azure. Podržte ukazatel nad hello přihlášeného uživatele v pravém horním rohu hello.
- Identifikátor URI prostředku Media Services.
- ID klienta aplikace Media Services (nativní). 
- Identifikátor URI pro přesměrování služby Media Services (nativní) aplikace. 

Hello hodnoty pro tyto parametry lze nalézt v **AzureEnvironments.AzureCloudEnvironment**. Hello **AzureEnvironments.AzureCloudEnvironment** konstanta pomocné rutiny v tooget hello .NET SDK je hello nastavení proměnné prostředí správné pro veřejné datového centra Azure. 

Obsahuje nastavení předem definované prostředí pro přístup k Media Services v pouze hello veřejné datových centrech. Pro suverénní nebo státní cloudových oblastí, můžete použít **AzureChinaCloudEnvironment**, **AzureUsGovernmentEnvrionment**, nebo **AzureGermanCloudEnvironment** v uvedeném pořadí.

Následující ukázka kódu Hello vytvoří token:
    
    var tokenCredentials = new AzureAdTokenCredentials("microsoft.onmicrosoft.com", AzureEnvironments.AzureCloudEnvironment);
    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
  
toostart programové ošetření Media Services, budete potřebovat toocreate **CloudMediaContext** instanci, která představuje kontext server hello. Hello **CloudMediaContext** obsahuje odkazy na kolekce tooimportant včetně úloh, prostředky, soubory, zásady přístupu a lokátory. 

Musíte taky toopass hello **identifikátor URI pro Media REST Services** toohello **CloudMediaContext** konstruktor. identifikátor URI prostředku hello tooget pro Media Services REST, přihlášení toohello portál Azure, vyberte svůj účet Azure Media Services, vyberte **přístup pomocí rozhraní API**a potom vyberte **připojit tooAzure Media Services s uživatelem ověřování**. 

Hello následující příklad kódu vytvoří **CloudMediaContext** instance:

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);

Hello následující příklad ukazuje, jak toocreate hello token a hello kontextu Azure AD:

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
>Pokud dojde k výjimce, která uvádí, že "hello na vzdálený server vrátil chybu: (401) Neautorizováno" v tématu hello [řízení přístupu](media-services-use-aad-auth-to-access-ams-api.md#access-control) části přístup k API služby Azure Media Services s Přehled ověřování Azure AD.

## <a name="use-service-principal-authentication"></a>Objekt zabezpečení ověřování pomocí služby
    
tooconnect toohello rozhraní API služby Azure Media Services s možností hlavní služby hello, vaše aplikace střední vrstvy (webového rozhraní API nebo webové aplikace) musí toorequests tokenu Azure AD s hello následující parametry:  

- Koncový bod pro klienta Azure AD. informace o Hello klienta lze načíst z hello portálu Azure. Podržte ukazatel nad hello přihlášeného uživatele v pravém horním rohu hello.
- Identifikátor URI prostředku Media Services.
- Hodnoty aplikace Azure AD: hello **ID klienta** a **tajný klíč klienta**.

Hello hodnoty pro hello **ID klienta** a **tajný klíč klienta** parametry lze nalézt v hello portálu Azure. Další informace najdete v tématu [Začínáme s Azure AD authentication pomocí portálu Azure hello](media-services-portal-get-started-with-aad.md).

Hello následující příklad kódu vytvoří token pomocí hello **AzureAdTokenCredentials** konstruktor, který přebírá **AzureAdClientSymmetricKey** jako parametr: 
    
    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientSymmetricKey("{YOUR CLIENT ID HERE}", "{YOUR CLIENT SECRET}"), 
                                AzureEnvironments.AzureCloudEnvironment);

    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

Můžete také zadat hello **AzureAdTokenCredentials** konstruktor, který přebírá **AzureAdClientCertificate** jako parametr. 

Další informace o tom toocreate a konfiguraci certifikátu v formulář, který lze použít Azure AD, najdete v části [ověřování tooAzure AD v aplikacích démon s certifikáty - postup ruční konfigurace](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md).

    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientCertificate("{YOUR CLIENT ID HERE}", "{YOUR CLIENT CERTIFICATE THUMBPRINT}"), 
                                AzureEnvironments.AzureCloudEnvironment);

toostart programové ošetření Media Services, budete potřebovat toocreate **CloudMediaContext** instanci, která představuje kontext server hello. Musíte taky toopass hello **identifikátor URI pro Media REST Services** toohello **CloudMediaContext** konstruktor. Můžete získat hello **identifikátor URI pro Media REST Services** hodnotu z hello také portálu Azure.

Hello následující příklad kódu vytvoří **CloudMediaContext** instance:

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
Hello následující příklad ukazuje, jak toocreate hello token a hello kontextu Azure AD:

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

## <a name="next-steps"></a>Další kroky

Začínáme s [nahrávání souborů tooyour účet](media-services-dotnet-upload-files.md).
