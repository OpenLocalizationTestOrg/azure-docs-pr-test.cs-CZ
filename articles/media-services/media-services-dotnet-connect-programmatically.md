---
title: "Účet služby tooMedia aaaConnecting pomocí rozhraní .NET"
description: "Toto téma ukazuje, jak tooconnect tooMedia služby pomocí rozhraní .NET."
services: media-services
documentationcenter: 
author: juliako
manager: erikre
editor: 
ms.assetid: a8412a29-59dc-44a0-ace0-be79a97dab63
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
ms.openlocfilehash: a23bd285f7cae17ae5831e1e50e73947afbb9a3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-toomedia-services-account-using-media-services-sdk-for-net"></a>Připojení tooMedia účtu služby pomocí sady Media Services SDK pro .NET
> [!div class="op_single_selector"]
> * [REST](media-services-rest-connect-programmatically.md)
> * [.NET](media-services-dotnet-connect-programmatically.md)
> 
> 

Toto téma popisuje, jak tooobtain tooMicrosoft programové připojení Azure Media Services, pokud jsou programování s hello sady Media Services SDK pro .NET.

## <a name="connecting-toomedia-services"></a>Připojení služby tooMedia
tooconnect tooMedia služeb prostřednictvím kódu programu, musíte mít dříve nastavil účet Azure, nakonfigurované na tento účet Media Services a potom nastavit projekt sady Visual Studio pro vývoj pomocí hello sady Media Services SDK pro .NET. Další informace najdete v tématu Nastavení pro vývoj s hello sady Media Services SDK pro .NET.

Na konci procesu instalace účtu Media Services hello na hello, jste získali hello následující požadované hodnoty připojení. Pomocí těchto programové připojení toomake tooMedia služby.

* Název účtu Media Services.
* Klíče účtu Media Services.

Tyto hodnoty, toofind přejděte toohello Azure Management Portal, vyberte svůj účet Media Service a klikněte na hello "**Správa KLÍČŮ**" ikonu na hello dolní části okna portálu hello. Kliknutím na hello ikonu další tooeach textové pole kopie hello hodnota toohello schránky systému.

## <a name="creating-a-cloudmediacontext-instance"></a>Vytvoření CloudMediaContext Instance
programové ošetření Media Services je třeba toocreate toostart **CloudMediaContext** instanci, která představuje kontext server hello. Hello **CloudMediaContext** obsahuje odkazy na kolekce tooimportant včetně úloh, prostředky, soubory, zásady přístupu a lokátory.

> [!NOTE]
> Hello **CloudMediaContext** třída není bezpečná pro přístup z více vláken. Měli byste vytvořit nový CloudMediaContext na vlákno nebo jednotlivou sadu operací.
> 
> 

CloudMediaContext má pět přetížení konstruktor. Je doporučené toouse konstruktory, které provést **MediaServicesCredentials** jako parametr. Další informace najdete v tématu hello **opakovaného použití tokeny služby Řízení přístupu** , který následuje. 

Hello následující příklad používá veřejný konstruktor CloudMediaContext(MediaServicesCredentials credentials) hello:

    // _cachedCredentials and _context are class member variables. 
    _cachedCredentials = new MediaServicesCredentials(
                    _mediaServicesAccountName,
                    _mediaServicesAccountKey);

    _context = new CloudMediaContext(_cachedCredentials);


## <a name="reusing-access-control-service-tokens"></a>Opětovné použití tokeny služby Řízení přístupu
Tato část uvádí, jak tooreuse služby Řízení přístupu tokeny pomocí CloudMediaContext konstruktory, které provést MediaServicesCredentials jako parametr.

[Azure Active Directory, řízení přístupu](https://msdn.microsoft.com/library/hh147631.aspx) (také označované jako služby Řízení přístupu nebo služby ACS) je služba založená na cloudu, která poskytuje snadný způsob ověřování a povolující přístupu uživatelů toogain tootheir webových aplikací. Microsoft Azure Media Services řídí přístup tooits služeb, když protokol OAuth, který vyžaduje tokenu služby ACS. Media Services přijímá tokeny služby ACS hello ze serveru ověřování.

Při vývoji s hello sada SDK služby Media Services, protože můžete toonot pozornosti s tokeny hello hello správci kódu SDK je pro vás. Povolení hello SDK však plně spravovat hello ACS tokeny zájemců toounnecessary žádostí o token. Požaduje tokeny trvá určitou dobu a spotřebovává prostředky klienta a server hello. Navíc omezí serveru ACS hello je generovaný hello požadavky, pokud hello míry je příliš vysoká. Hello limit je 30 požadavků za sekundu, najdete v části [omezení služby ACS](https://msdn.microsoft.com/library/gg185909.aspx) další podrobnosti.

Počínaje hello sady Media Services SDK verze 3.0.0.0, můžete znovu použít hello tokeny služby ACS. Hello **CloudMediaContext** konstruktory, které nepřijímají **MediaServicesCredentials** jako parametr Povolit sdílení tokeny hello ACS mezi více kontexty. Hello MediaServicesCredentials třída zapouzdří přihlašovací údaje služby Media Services hello. Pokud je k dispozici tokenu služby ACS a znáte jeho čas vypršení platnosti, můžete vytvořit novou instanci MediaServicesCredentials s tokenem hello a předejte ji toohello konstruktoru CloudMediaContext. Všimněte si, že hello sady Media Services SDK automaticky aktualizuje tokeny vždy, když vypršení jejich platnosti. Existují dva způsoby tooreuse tokeny služby ACS, jak je znázorněno v následující příklady hello.

* Můžete mezipaměti hello **MediaServicesCredentials** objekt v paměti (například v proměnné statické třídy). Pak předejte hello mezipaměti objekt toohello CloudMediaContext konstruktor. objekt MediaServicesCredentials Hello obsahuje tokenu služby ACS, které můžete znovu použít, pokud je stále platný. Pokud hello token není platný, že se aktualizují podle hello Media Services SDK pomocí přihlašovacích údajů hello daného toohello MediaServicesCredentials konstruktor.
  
    Všimněte si, že hello **MediaServicesCredentials** objekt získá platný token po hello RefreshToken je volána. Hello **CloudMediaContext** volání hello **RefreshToken** metoda v konstruktoru hello. Pokud plánujete toosave hello hodnoty tokenu tooan externího úložiště, proveďte toocheck se, zda hello TokenExpiration hodnota je platná před uložením dat tokenů hello. Pokud není platný, zavolejte RefreshToken před ukládání do mezipaměti.
  
        // Create and cache hello Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);

        // Use hello cached credentials toocreate a new CloudMediaContext object.
        if(_cachedCredentials == null)
        {
            _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);
        }

        CloudMediaContext context = new CloudMediaContext(_cachedCredentials);

* Můžete také mezipaměti hello AccessToken řetězec a hello TokenExpiration hodnoty. hodnoty Hello později nelze použít toocreate nové MediaServicesCredentials objektu tokenu daty hello do mezipaměti.  To je obzvláště užitečná pro scénáře, kde hello token lze bezpečně sdílet mezi více procesy nebo počítače.
  
    Hello následující fragmenty kódu volání hello SaveTokenDataToExternalStorage, GetTokenDataFromExternalStorage a UpdateTokenDataInExternalStorageIfNeeded metody, které nejsou v tomto příkladu. Můžete třeba definovat tyto metody toostore, načtení a aktualizace dat tokenů v externím úložišti. 
  
        CloudMediaContext context1 = new CloudMediaContext(_mediaServicesAccountName, _mediaServicesAccountKey);
  
        // Get token values from hello context.
        var accessToken = context1.Credentials.AccessToken;
        var tokenExpiration = context1.Credentials.TokenExpiration;
  
        // Save token values for later use. 
        // hello SaveTokenDataToExternalStorage method should check 
        // whether hello TokenExpiration value is valid before saving hello token data. 
        // If it is not valid, call MediaServicesCredentials’s RefreshToken before caching.
        SaveTokenDataToExternalStorage(accessToken, tokenExpiration);
  
    Použití hello uložit hodnoty tokenu toocreate MediaServicesCredentials.

        var accessToken = "";
        var tokenExpiration = DateTime.UtcNow;

        // Retrieve saved token values.
        GetTokenDataFromExternalStorage(out accessToken, out tokenExpiration);

        // Create a new MediaServicesCredentials object using saved token values.
        MediaServicesCredentials credentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey)
        {
            AccessToken = accessToken,
            TokenExpiration = tokenExpiration
        };

        CloudMediaContext context2 = new CloudMediaContext(credentials);

    Aktualizujte token kopii hello v případě, že hello token byl aktualizován hello sada SDK služby Media Services. 

        if(tokenExpiration != context2.Credentials.TokenExpiration)
        {
            UpdateTokenDataInExternalStorageIfNeeded(accessToken, context2.Credentials.TokenExpiration);
        }


* Pokud máte více účtů Media Services (například pro sdílení účely nebo Geo rozdělení zatížení) do mezipaměti můžete ukládat objekty MediaServicesCredentials pomocí kolekce System.Collections.Concurrent.ConcurrentDictionary hello (hello Kolekce ConcurrentDictionary představuje kolekci vláken párů klíč/hodnota, které můžete přistupovat pomocí více vláken současně). Potom můžete hello GetOrAdd metoda tooget hello do mezipaměti přihlašovací údaje. 
  
        // Declare a static class variable of hello ConcurrentDictionary type in which hello Media Services credentials will be cached.  
        private static readonly ConcurrentDictionary<string, MediaServicesCredentials> mediaServicesCredentialsCache = 
            new ConcurrentDictionary<string, MediaServicesCredentials>();

        // Cache (or get already cached) Media Services credentials. Use these credentials toocreate a new CloudMediaContext object.
        static public CloudMediaContext CreateMediaServicesContext(string accountName, string accountKey)
        {
            CloudMediaContext cloudMediaContext;
            MediaServicesCredentials mediaServicesCredentials;

            mediaServicesCredentials = mediaServicesCredentialsCache.GetOrAdd(
                accountName,
                valueFactory => new MediaServicesCredentials(accountName, accountKey));

            cloudMediaContext = new CloudMediaContext(mediaServicesCredentials);

            return cloudMediaContext;
        }

## <a name="connecting-tooa-media-services-account-located-in-hello-north-china-region"></a>Připojení účtu Media Services tooa nachází v oblasti severní Čína hello
Pokud váš účet se nachází v oblasti severní Čína hello, použijte následující konstruktor hello:

    public CloudMediaContext(Uri apiServer, string accountName, string accountKey, string scope, string acsBaseAddress)

Například:

    _context = new CloudMediaContext(
        new Uri("https://wamsbjbclus001rest-hs.chinacloudapp.cn/API/"),
        _mediaServicesAccountName,
        _mediaServicesAccountKey,
        "urn:WindowsAzureMediaServices",
        "https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn");


## <a name="storing-connection-values-in-configuration"></a>Ukládání hodnot připojení v konfiguraci
Je připojení hodnoty toostore vysoce doporučený postup, obzvláště citlivé hodnoty, například název účtu a heslo v konfiguraci. Je také doporučený postup tooencrypt citlivé konfigurační data. Hello celý konfigurační soubor můžete šifrovat pomocí hello Windows systém souborů (Encrypting File System). Vyberte tooenable systémem souborů EFS v souboru, klikněte pravým tlačítkem na soubor hello, **vlastnosti**a povolilo se šifrování v hello **Upřesnit** kartě nastavení. Nebo můžete vytvořit vlastní řešení pro šifrování vybraných částí konfiguračního souboru pomocí chráněné konfigurace. V tématu [šifrování informace o konfiguraci pomocí chráněné konfigurace](https://msdn.microsoft.com/library/53tyfkaw.aspx).

Hello následující soubor App.config obsahuje hodnoty hello požadované připojení. Hello hodnoty v hello <appSettings> element jsou hello požadované hodnoty, které jste získali z procesu instalace účtu Media Services hello.

    <configuration>
      <appSettings>
        <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
        <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
      </appSettings>
    </configuration>


hodnoty připojení tooretrieve z konfigurace, můžete použít hello **ConfigurationManager** třídy a pak mu přiřaďte hello hodnoty toofields ve vašem kódu:

    private static readonly string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
    private static readonly string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];



## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

