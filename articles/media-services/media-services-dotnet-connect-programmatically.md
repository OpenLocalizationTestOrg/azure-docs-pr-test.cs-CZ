---
title: "Připojení k účtu Media Services pomocí rozhraní .NET"
description: "Toto téma ukazuje, jak se připojit ke službě Media Services pomocí rozhraní .NET."
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
ms.openlocfilehash: 892932116934952265a21ab17aac3434b5760136
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connecting-to-media-services-account-using-media-services-sdk-for-net"></a>Připojení k účtu Media Services pomocí sady Media Services SDK pro .NET
> [!div class="op_single_selector"]
> * [REST](media-services-rest-connect-programmatically.md)
> * [.NET](media-services-dotnet-connect-programmatically.md)
> 
> 

Toto téma popisuje, jak získat programové připojení k Microsoft Azure Media Services, pokud jsou programování pomocí sady Media Services SDK pro .NET.

## <a name="connecting-to-media-services"></a>Připojení ke službě Media Services
Pro připojení ke službě Media Services prostřednictvím kódu programu, můžete musí mít dříve nastavil účet Azure, nakonfigurované na tento účet Media Services a nastavit projekt sady Visual Studio pro vývoj pomocí sady Media Services SDK pro .NET. Další informace najdete v tématu Nastavení pro vývoj pomocí sady Media Services SDK pro .NET.

Na konci procesu instalace účtu Media Services můžete získat následující hodnoty požadované připojení. Použijí pro provedení programové připojení ke službě Media Services.

* Název účtu Media Services.
* Klíče účtu Media Services.

Tyto hodnoty, najdete na portálu Azure Management Portal, vyberte svůj účet Media Service a klikněte na "**Správa KLÍČŮ**" v dolní části okna portálu na ikonu. Kliknutím na ikonu vedle každého textového pole zkopírujete příslušnou hodnotu do schránky systému.

## <a name="creating-a-cloudmediacontext-instance"></a>Vytvoření CloudMediaContext Instance
Spusťte programové ošetření Media Services je nutné vytvořit **CloudMediaContext** instanci, která představuje kontext serveru. **CloudMediaContext** obsahuje odkazy na důležité kolekcí včetně úloh, prostředky, soubory, zásady přístupu a lokátory.

> [!NOTE]
> **CloudMediaContext** třída není bezpečná pro přístup z více vláken. Měli byste vytvořit nový CloudMediaContext na vlákno nebo jednotlivou sadu operací.
> 
> 

CloudMediaContext má pět přetížení konstruktor. Doporučuje se použít konstruktory, které nepřijímají **MediaServicesCredentials** jako parametr. Další informace najdete v tématu **opakovaného použití tokeny služby Řízení přístupu** , který následuje. 

Následující příklad používá veřejný konstruktor CloudMediaContext(MediaServicesCredentials credentials):

    // _cachedCredentials and _context are class member variables. 
    _cachedCredentials = new MediaServicesCredentials(
                    _mediaServicesAccountName,
                    _mediaServicesAccountKey);

    _context = new CloudMediaContext(_cachedCredentials);


## <a name="reusing-access-control-service-tokens"></a>Opětovné použití tokeny služby Řízení přístupu
V této části ukazuje, jak znovu použít tokeny služby Řízení přístupu pomocí CloudMediaContext konstruktory, které provést MediaServicesCredentials jako parametr.

[Azure Active Directory, řízení přístupu](https://msdn.microsoft.com/library/hh147631.aspx) (také označované jako služby Řízení přístupu nebo služby ACS) je Cloudová služba, která poskytuje snadný způsob ověřování a autorizace uživatelů pro přístup do své webové aplikace. Microsoft Azure Media Services řídí přístup k jeho služby ale protokolu OAuth, který vyžaduje tokenu služby ACS. Media Services přijímá tokeny služby ACS ze serveru ověřování.

Při vývoj pomocí sady Media Services SDK, je možné není zacházet s tokeny, protože sada SDK kód správce je pro vás. Ale když necháte SDK plně spravovat tokeny služby ACS vede k nepotřebné žádostí o token. Požaduje tokeny trvá určitou dobu a spotřebovává prostředky klienta a serveru. Navíc omezí server služby ACS je generovaný žádosti, pokud je příliš vysoká rychlost. Limit je 30 požadavků za sekundu, najdete v části [omezení služby ACS](https://msdn.microsoft.com/library/gg185909.aspx) další podrobnosti.

Od verze sady Media Services SDK verze 3.0.0.0, můžete znovu použít tokeny služby ACS. **CloudMediaContext** konstruktory, které nepřijímají **MediaServicesCredentials** jako parametr Povolit sdílení mezi více kontexty tokeny služby ACS. MediaServicesCredentials třída zapouzdří přihlašovací údaje služby Media Services. Pokud je k dispozici tokenu služby ACS a znáte jeho čas vypršení platnosti, můžete vytvořit novou instanci MediaServicesCredentials s tokenem a předejte ji do konstruktoru objektu CloudMediaContext. Všimněte si, že sady Media Services SDK automaticky aktualizuje tokeny vždy, když vypršení jejich platnosti. Existují dva způsoby, jak znovu použít tokeny služby ACS, jak je znázorněno v následujících příkladech.

* Může ukládat do mezipaměti **MediaServicesCredentials** objekt v paměti (například v proměnné statické třídy). Pak předejte objektu v mezipaměti CloudMediaContext konstruktoru. Objekt MediaServicesCredentials obsahuje tokenu služby ACS, které můžete znovu použít, pokud je stále platný. Pokud token není platný, se aktualizují pomocí sady Media Services SDK pomocí přihlašovacích údajů na MediaServicesCredentials konstruktor.
  
    Všimněte si, že **MediaServicesCredentials** objekt získá platný token po volání RefreshToken. **CloudMediaContext** volání **RefreshToken** metoda v konstruktoru. Pokud máte v úmyslu uložit hodnoty tokenu do externího úložiště, nezapomeňte zkontrolovat, zda je hodnota TokenExpiration platný před uložením dat tokenů. Pokud není platný, zavolejte RefreshToken před ukládání do mezipaměti.
  
        // Create and cache the Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);

        // Use the cached credentials to create a new CloudMediaContext object.
        if(_cachedCredentials == null)
        {
            _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);
        }

        CloudMediaContext context = new CloudMediaContext(_cachedCredentials);

* Můžete také mezipaměti řetězec AccessToken a TokenExpiration hodnoty. Hodnoty může později použije k vytvoření nového objektu MediaServicesCredentials se data uložená v mezipaměti tokenu.  To je obzvláště užitečná pro scénáře, kde token lze bezpečně sdílet mezi více procesy nebo počítače.
  
    Následující fragmenty kódu volání metody SaveTokenDataToExternalStorage, GetTokenDataFromExternalStorage a UpdateTokenDataInExternalStorageIfNeeded, které nejsou definované v tomto příkladu. Můžete třeba definovat tyto metody ukládání, načíst a aktualizovat token data v externím úložišti. 
  
        CloudMediaContext context1 = new CloudMediaContext(_mediaServicesAccountName, _mediaServicesAccountKey);
  
        // Get token values from the context.
        var accessToken = context1.Credentials.AccessToken;
        var tokenExpiration = context1.Credentials.TokenExpiration;
  
        // Save token values for later use. 
        // The SaveTokenDataToExternalStorage method should check 
        // whether the TokenExpiration value is valid before saving the token data. 
        // If it is not valid, call MediaServicesCredentials’s RefreshToken before caching.
        SaveTokenDataToExternalStorage(accessToken, tokenExpiration);
  
    Uložené hodnoty tokenu použijte k vytvoření MediaServicesCredentials.

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

    Aktualizujte token kopii v případě, že token byl aktualizován pomocí sady Media Services SDK. 

        if(tokenExpiration != context2.Credentials.TokenExpiration)
        {
            UpdateTokenDataInExternalStorageIfNeeded(accessToken, context2.Credentials.TokenExpiration);
        }


* Pokud máte více účtů Media Services (například pro sdílení účely nebo Geo rozdělení zatížení) do mezipaměti můžete ukládat objekty MediaServicesCredentials pomocí kolekce System.Collections.Concurrent.ConcurrentDictionary (kolekci ConcurrentDictionary představuje kolekci vláken párů klíč/hodnota, které můžete přistupovat pomocí více vláken současně). Pak můžete použít metodu GetOrAdd získat přihlašovací údaje v mezipaměti. 
  
        // Declare a static class variable of the ConcurrentDictionary type in which the Media Services credentials will be cached.  
        private static readonly ConcurrentDictionary<string, MediaServicesCredentials> mediaServicesCredentialsCache = 
            new ConcurrentDictionary<string, MediaServicesCredentials>();

        // Cache (or get already cached) Media Services credentials. Use these credentials to create a new CloudMediaContext object.
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

## <a name="connecting-to-a-media-services-account-located-in-the-north-china-region"></a>Připojení k účtu Media Services nachází v oblasti severní Čína
Pokud váš účet se nachází v oblasti severní Čína, použijte následující konstruktor:

    public CloudMediaContext(Uri apiServer, string accountName, string accountKey, string scope, string acsBaseAddress)

Například:

    _context = new CloudMediaContext(
        new Uri("https://wamsbjbclus001rest-hs.chinacloudapp.cn/API/"),
        _mediaServicesAccountName,
        _mediaServicesAccountKey,
        "urn:WindowsAzureMediaServices",
        "https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn");


## <a name="storing-connection-values-in-configuration"></a>Ukládání hodnot připojení v konfiguraci
Je vysoce doporučený postup pro uložení připojení hodnot, jako je například název účtu a hesla, obzvláště citlivé hodnoty v konfiguraci. Také je doporučený postup pro šifrování citlivých konfigurační data. Pomocí Windows systém souborů (Encrypting File System) můžete šifrovat celý konfiguračního souboru. Chcete-li povolit systémem souborů EFS na soubor, klikněte pravým tlačítkem na soubor, vyberte možnost **vlastnosti**a povolit šifrování v **Upřesnit** kartě nastavení. Nebo můžete vytvořit vlastní řešení pro šifrování vybraných částí konfiguračního souboru pomocí chráněné konfigurace. V tématu [šifrování informace o konfiguraci pomocí chráněné konfigurace](https://msdn.microsoft.com/library/53tyfkaw.aspx).

Následující soubor App.config obsahuje hodnoty požadované připojení. Hodnoty <appSettings> element jsou požadované hodnoty, které jste získali z instalačního procesu účtu Media Services.

    <configuration>
      <appSettings>
        <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
        <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
      </appSettings>
    </configuration>


Chcete-li načíst hodnoty připojení z konfigurace, můžete použít **ConfigurationManager** třídy a pak mu přiřaďte hodnoty polí ve vašem kódu:

    private static readonly string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
    private static readonly string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];



## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

