---
title: "Postup nastavení počítače pro vývoj pomocí rozhraní .NET služby Media Services"
description: "Další informace o požadavcích na Media Services pomocí sady Media Services SDK pro .NET. Také zjistěte, jak vytvořit aplikaci Visual Studio."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ec2804c7-c656-4fbf-b3e4-3f0f78599a7f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/23/2017
ms.author: juliako
ms.openlocfilehash: 15828bc74937a036871b26493498232ec7cf6f06
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="media-services-development-with-net"></a>Vývoj pro Media Services pomocí rozhraní .NET
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Toto téma popisuje, jak chcete začít vyvíjet aplikace Media Services pomocí rozhraní .NET.

**Azure Media Services .NET SDK** knihovny umožňuje programu proti Media Services pomocí rozhraní .NET. Aby bylo snazší i pro vývoj pomocí rozhraní .NET, **rozšíření Azure Media Services .NET SDK** knihovna je k dispozici. Tato knihovna obsahuje sadu metod rozšíření a pomocných funkcí, které zjednodušují kódu .NET. Obě knihovny jsou k dispozici prostřednictvím **NuGet** a **Githubu**.

## <a name="prerequisites"></a>Požadavky
* Účet Media Services v novém nebo existujícím předplatném Azure. Další informace najdete v tématu [Vytvoření účtu Media Services](media-services-portal-create-account.md).
* Operační systémy: Windows 10, Windows 7, Windows 2008 R2 nebo Windows 8.
* Rozhraní .NET framework 4.5.
* Visual Studio.

## <a name="create-and-configure-a-visual-studio-project"></a>Vytvoření a konfigurace projektu Visual Studia
V této části se dozvíte, jak k vytvoření projektu v sadě Visual Studio a její nastavení pro vývoj pro Media Services.  V takovém případě projekt je konzolová aplikace C# Windows, ale stejný postup instalace, které jsou tady uvedené platí pro jiné typy projektů, které můžete vytvořit pro Media Services aplikace (například aplikace Windows Forms nebo webovou aplikaci ASP.NET).

Tato část ukazuje způsob použití **NuGet** přidat rozšíření sady Media Services .NET SDK a další závislé knihovny.

Alternativně můžete získat nejnovější sadu Media Services .NET SDK bits z webu GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) nebo [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), sestavte řešení a přidejte odkazy na projektu klienta. Všechny potřebné závislosti získat stažené a rozbalené automaticky.

1. Vytvořte novou konzolovou aplikaci v jazyce C# v sadě Visual Studio. Zadejte **název**, **umístění**, a **název řešení**a pak klikněte na tlačítko OK.
2. Sestavte řešení.
3. Použití **NuGet** k instalaci a přidání **rozšíření Azure Media Services .NET SDK** (**windowsazure.mediaservices.extensions**). Při instalaci tohoto balíčku se nainstaluje také **sada SDK služby Media Services pro .NET** a přidá všechny ostatní požadované závislosti.
   
    Ujistěte se, že máte nejnovější verzi balíčku nuget, které jsou nainstalované. Další informace a pokyny pro instalaci, najdete v části [NuGet](http://nuget.codeplex.com/).
4. V Průzkumníku řešení klikněte pravým tlačítkem na název projektu a zvolte spravovat balíčky NuGet balíčky.
   
    Zobrazí se dialogové okno Spravovat balíčky NuGet.
5. V galerii Online vyhledejte MediaServices rozšíření Azure, zvolte rozšíření Azure Media Services .NET SDK a potom klikněte na tlačítko Instalovat.
   
    Projekt se mění a odkazy na rozšíření Media Services .NET SDK, sady Media Services .NET SDK a dalších závislých sestavení jsou přidány.
6. Ke zvýšení úrovně čisticí vývojového prostředí, zvažte povolení obnovení balíčků NuGet. Další informace najdete v tématu [obnovení balíčků NuGet "](http://docs.nuget.org/consume/package-restore).
7. Přidat odkaz na **System.Configuration** sestavení. Toto sestavení obsahuje System.Configuration. **ConfigurationManager** třídu, která se používá pro přístup k souborům konfigurace (například App.config).
   
    Přidání odkazů pomocí dialogu spravovat odkazy, klikněte pravým tlačítkem na název projektu v Průzkumníku řešení. Pak vyberte Přidat a odkazy.
   
    Otevře se dialogové okno Spravovat odkazy.
8. V části sestavení rozhraní .NET framework najít a vybrat sestavení System.Configuration a klikněte na tlačítko OK.
9. Otevřete soubor App.config a přidejte *appSettings* část do souboru.     
   
    Nastavte hodnoty, které jsou potřebné pro připojení k rozhraní API služby Media Services. Další informace najdete v tématu [přístup k Azure Media Services API pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md). 

    Pokud používáte [ověření uživatele](media-services-use-aad-auth-to-access-ams-api.md#types-of-authentication) konfiguračního souboru bude pravděpodobně mít hodnoty pro doménu klienta Azure AD a koncového bodu rozhraní REST API pro AMS.
    
    >[!Important]
    >Většina ukázek kódu v dokumentaci k Azure Media Services nastavit, použijte uživatele (interaktivní) typ ověřování pro připojení k rozhraní API pro AMS. Tato metoda ověřování bude fungovat i pro správu nebo monitorování nativní aplikace: mobilní aplikace, aplikace pro Windows a konzolové aplikace. Tato metoda ověřování není vhodný pro server, webové služby, rozhraní API typu aplikací.  Další informace najdete v tématu [přístup k rozhraní API AMS pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).

        <configuration>
        ...
            <appSettings>
              <add key="AADTenantDomain" value="YourAADTenantDomain" />
              <add key="MediaServiceRESTAPIEndpoint" value="YourRESTAPIEndpoint" />
            </appSettings>

        </configuration>

10. Následujícím kódem přepište existující příkazy **using** na začátku souboru Program.cs.
           
        using System;
        using System.Configuration;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Collections.Generic;
        using System.Linq;

V tomto okamžiku jste připravení začít s vývojem aplikací Media Services.    

## <a name="example"></a>Příklad

Zde je malý příklad, který se připojuje k rozhraní API pro AMS a uvádí všechny dostupné procesory médií.
    
    class Program
    {
        // Read values from the App.config file.
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];
    
        private static CloudMediaContext _context = null;
        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
    
            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);
    
            // List all available Media Processors
            foreach (var mp in _context.MediaProcessors)
            {
                Console.WriteLine(mp.Name);
            }
    
        }

##<a name="next-steps"></a>Další kroky

Nyní [se můžete připojit k rozhraní API pro AMS](media-services-use-aad-auth-to-access-ams-api.md) a spusťte [vývoj](media-services-dotnet-get-started.md).


## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

