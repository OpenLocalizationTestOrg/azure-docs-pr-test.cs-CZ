---
title: "aaaHow tooSet až počítače pro vývoj Media Services pomocí rozhraní .NET"
description: "Další informace o hello předpoklady pro Media Services pomocí hello sady Media Services SDK pro .NET. Také zjistěte, jak toocreate aplikace Visual Studio."
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
ms.openlocfilehash: a5a2af3211d8156fd7dea99831fb769df4130d41
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="media-services-development-with-net"></a>Vývoj pro Media Services pomocí rozhraní .NET
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Toto téma popisuje, jak toostart vývoj Media Services aplikací s použitím rozhraní .NET.

Hello **Azure Media Services .NET SDK** knihovny vám umožní tooprogram proti Media Services pomocí rozhraní .NET. toomake ho i jednodušší toodevelop s rozhraním .NET, hello **rozšíření Azure Media Services .NET SDK** knihovna je k dispozici. Tato knihovna obsahuje sadu metod rozšíření a pomocných funkcí, které zjednodušují kódu .NET. Obě knihovny jsou k dispozici prostřednictvím **NuGet** a **Githubu**.

## <a name="prerequisites"></a>Požadavky
* Účet Media Services v novém nebo existujícím předplatném Azure. V tématu hello [jak tooCreate účtu Media Services](media-services-portal-create-account.md).
* Operační systémy: Windows 10, Windows 7, Windows 2008 R2 nebo Windows 8.
* Rozhraní .NET framework 4.5.
* Visual Studio.

## <a name="create-and-configure-a-visual-studio-project"></a>Vytvoření a konfigurace projektu Visual Studia
V této části se dozvíte, jak toocreate projektu v sadě Visual Studio a její nastavení pro vývoj pro Media Services.  V takovém případě hello projekt je konzolová aplikace C# Windows, ale hello stejné kroky nastavení, které jsou tady uvedené platí tooother typy projektů, které můžete vytvořit pro Media Services aplikace (například aplikace Windows Forms nebo webovou aplikaci ASP.NET).

Tato část uvádí, jak toouse **NuGet** tooadd sady Media Services .NET SDK rozšíření a další závislé knihovny.

Alternativně můžete získat nejnovější sadu Media Services .NET SDK bits hello z webu GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) nebo [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), vytvoření hello řešení a přidání projektu klienta toohello hello odkazy. Všechny potřebné závislosti hello získat stažené a rozbalené automaticky.

1. Vytvořte novou konzolovou aplikaci v jazyce C# v sadě Visual Studio. Zadejte hello **název**, **umístění**, a **název řešení**a pak klikněte na tlačítko OK.
2. Vytvoření řešení hello.
3. Použití **NuGet** tooinstall a přidejte **rozšíření Azure Media Services .NET SDK** (**windowsazure.mediaservices.extensions**). Při instalaci tohoto balíčku se nainstaluje také **sada SDK služby Media Services pro .NET** a přidá všechny ostatní požadované závislosti.
   
    Ujistěte se, že máte nejnovější verzi hello nuget nainstalována. Další informace a pokyny pro instalaci, najdete v části [NuGet](http://nuget.codeplex.com/).
4. V Průzkumníku řešení klikněte pravým tlačítkem na název hello hello projektu a zvolte spravovat balíčky NuGet balíčky.
   
    Zobrazí se dialogové okno Spravovat balíčky NuGet Hello.
5. V hello Online galerie, vyhledejte MediaServices rozšíření Azure, zvolte rozšíření Azure Media Services .NET SDK a potom klikněte na tlačítko Instalovat hello.
   
    Hello projekt se mění a odkazuje na toohello rozšíření sady SDK Media Services pro rozhraní .NET, sady Media Services .NET SDK a přidání dalších závislých sestavení.
6. toopromote čisticí vývojové prostředí, zvažte, povolení obnovení balíčků NuGet. Další informace najdete v tématu [obnovení balíčků NuGet "](http://docs.nuget.org/consume/package-restore).
7. Přidat odkaz příliš**System.Configuration** sestavení. Toto sestavení obsahuje hello System.Configuration. **ConfigurationManager** tedy třídy používané tooaccess konfigurační soubory (například App.config).
   
    tooadd odkazů pomocí dialogu hello spravovat odkazy, klikněte pravým tlačítkem na název projektu hello v Průzkumníku řešení hello. Pak vyberte Přidat a odkazy.
   
    Otevře se dialogové okno Spravovat odkazy Hello.
8. V části sestavení rozhraní .NET framework najít a vybrat sestavení System.Configuration hello a kliknutím na tlačítko OK.
9. Otevřete soubor App.config hello a přidejte *appSettings* soubor toohello oddílu.     
   
    Nastavte hello hodnoty, které jsou potřebné tooconnect toohello Media Services API. Další informace najdete v tématu [hello přístup k rozhraní API služby Azure Media Services pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md). 

    Pokud používáte [ověření uživatele](media-services-use-aad-auth-to-access-ams-api.md#types-of-authentication) konfiguračního souboru bude pravděpodobně mít hodnoty pro doménu klienta Azure AD a hello koncový bod AMS REST API.
    
    >[!Important]
    >Většina ukázky kódu v dokumentaci k Azure Media Services hello nastavit, použijte uživatele (interaktivní) typ ověřování tooconnect toohello rozhraní API pro AMS. Tato metoda ověřování bude fungovat i pro správu nebo monitorování nativní aplikace: mobilní aplikace, aplikace pro Windows a konzolové aplikace. Tato metoda ověřování není vhodný pro server, webové služby, rozhraní API typu aplikací.  Další informace najdete v tématu [hello přístup k rozhraní API pro AMS při ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).

        <configuration>
        ...
            <appSettings>
              <add key="AADTenantDomain" value="YourAADTenantDomain" />
              <add key="MediaServiceRESTAPIEndpoint" value="YourRESTAPIEndpoint" />
            </appSettings>

        </configuration>

10. Přepsat existující hello **pomocí** příkazy od hello začátku souboru Program.cs hello s hello následující kód.
           
        using System;
        using System.Configuration;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Collections.Generic;
        using System.Linq;

V tomto okamžiku jsou připravené toostart vývoj aplikace s Media Services.    

## <a name="example"></a>Příklad

Zde je malý příklad, který se připojuje toohello AMS rozhraní API a uvádí všechny dostupné procesory médií.
    
    class Program
    {
        // Read values from hello App.config file.
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

Nyní [toohello AMS rozhraní API se můžete připojit](media-services-use-aad-auth-to-access-ams-api.md) a spusťte [vývoj](media-services-dotnet-get-started.md).


## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

