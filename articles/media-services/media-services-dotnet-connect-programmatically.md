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
# <a name="connecting-toomedia-services-account-using-media-services-sdk-for-net"></a><span data-ttu-id="abc31-103">Připojení tooMedia účtu služby pomocí sady Media Services SDK pro .NET</span><span class="sxs-lookup"><span data-stu-id="abc31-103">Connecting tooMedia Services Account using Media Services SDK for .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="abc31-104">REST</span><span class="sxs-lookup"><span data-stu-id="abc31-104">REST</span></span>](media-services-rest-connect-programmatically.md)
> * [<span data-ttu-id="abc31-105">.NET</span><span class="sxs-lookup"><span data-stu-id="abc31-105">.NET</span></span>](media-services-dotnet-connect-programmatically.md)
> 
> 

<span data-ttu-id="abc31-106">Toto téma popisuje, jak tooobtain tooMicrosoft programové připojení Azure Media Services, pokud jsou programování s hello sady Media Services SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="abc31-106">This topic describes how tooobtain a programmatic connection tooMicrosoft Azure Media Services when you are programming with hello Media Services SDK for .NET.</span></span>

## <a name="connecting-toomedia-services"></a><span data-ttu-id="abc31-107">Připojení služby tooMedia</span><span class="sxs-lookup"><span data-stu-id="abc31-107">Connecting tooMedia Services</span></span>
<span data-ttu-id="abc31-108">tooconnect tooMedia služeb prostřednictvím kódu programu, musíte mít dříve nastavil účet Azure, nakonfigurované na tento účet Media Services a potom nastavit projekt sady Visual Studio pro vývoj pomocí hello sady Media Services SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="abc31-108">tooconnect tooMedia Services programmatically, you must have previously set up an Azure account, configured Media Services on that account, and then set up a Visual Studio project for development with hello Media Services SDK for .NET.</span></span> <span data-ttu-id="abc31-109">Další informace najdete v tématu Nastavení pro vývoj s hello sady Media Services SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="abc31-109">For more information, see Setup for Development with hello Media Services SDK for .NET.</span></span>

<span data-ttu-id="abc31-110">Na konci procesu instalace účtu Media Services hello na hello, jste získali hello následující požadované hodnoty připojení.</span><span class="sxs-lookup"><span data-stu-id="abc31-110">At hello end of hello Media Services account setup process, you obtained hello following required connection values.</span></span> <span data-ttu-id="abc31-111">Pomocí těchto programové připojení toomake tooMedia služby.</span><span class="sxs-lookup"><span data-stu-id="abc31-111">Use these toomake programmatic connections tooMedia Services.</span></span>

* <span data-ttu-id="abc31-112">Název účtu Media Services.</span><span class="sxs-lookup"><span data-stu-id="abc31-112">Your Media Services account name.</span></span>
* <span data-ttu-id="abc31-113">Klíče účtu Media Services.</span><span class="sxs-lookup"><span data-stu-id="abc31-113">Your Media Services account key.</span></span>

<span data-ttu-id="abc31-114">Tyto hodnoty, toofind přejděte toohello Azure Management Portal, vyberte svůj účet Media Service a klikněte na hello "**Správa KLÍČŮ**" ikonu na hello dolní části okna portálu hello.</span><span class="sxs-lookup"><span data-stu-id="abc31-114">toofind these values, go toohello Azure Managment Portal, select your Media Service account, and click on hello “**MANAGE KEYS**” icon on hello bottom of hello portal window.</span></span> <span data-ttu-id="abc31-115">Kliknutím na hello ikonu další tooeach textové pole kopie hello hodnota toohello schránky systému.</span><span class="sxs-lookup"><span data-stu-id="abc31-115">Clicking on hello icon next tooeach text box copies hello value toohello system clipboard.</span></span>

## <a name="creating-a-cloudmediacontext-instance"></a><span data-ttu-id="abc31-116">Vytvoření CloudMediaContext Instance</span><span class="sxs-lookup"><span data-stu-id="abc31-116">Creating a CloudMediaContext Instance</span></span>
<span data-ttu-id="abc31-117">programové ošetření Media Services je třeba toocreate toostart **CloudMediaContext** instanci, která představuje kontext server hello.</span><span class="sxs-lookup"><span data-stu-id="abc31-117">toostart programming against Media Services you need toocreate a **CloudMediaContext** instance that represents hello server context.</span></span> <span data-ttu-id="abc31-118">Hello **CloudMediaContext** obsahuje odkazy na kolekce tooimportant včetně úloh, prostředky, soubory, zásady přístupu a lokátory.</span><span class="sxs-lookup"><span data-stu-id="abc31-118">hello **CloudMediaContext** includes references tooimportant collections including jobs, assets, files, access policies, and locators.</span></span>

> [!NOTE]
> <span data-ttu-id="abc31-119">Hello **CloudMediaContext** třída není bezpečná pro přístup z více vláken.</span><span class="sxs-lookup"><span data-stu-id="abc31-119">hello **CloudMediaContext** class is not thread safe.</span></span> <span data-ttu-id="abc31-120">Měli byste vytvořit nový CloudMediaContext na vlákno nebo jednotlivou sadu operací.</span><span class="sxs-lookup"><span data-stu-id="abc31-120">You should create a new CloudMediaContext per thread or per set of operations.</span></span>
> 
> 

<span data-ttu-id="abc31-121">CloudMediaContext má pět přetížení konstruktor.</span><span class="sxs-lookup"><span data-stu-id="abc31-121">CloudMediaContext has five constructor overloads.</span></span> <span data-ttu-id="abc31-122">Je doporučené toouse konstruktory, které provést **MediaServicesCredentials** jako parametr.</span><span class="sxs-lookup"><span data-stu-id="abc31-122">It is recommended toouse constructors that take **MediaServicesCredentials** as a parameter.</span></span> <span data-ttu-id="abc31-123">Další informace najdete v tématu hello **opakovaného použití tokeny služby Řízení přístupu** , který následuje.</span><span class="sxs-lookup"><span data-stu-id="abc31-123">For more information, see hello **Reusing Access Control Service Tokens** that follows.</span></span> 

<span data-ttu-id="abc31-124">Hello následující příklad používá veřejný konstruktor CloudMediaContext(MediaServicesCredentials credentials) hello:</span><span class="sxs-lookup"><span data-stu-id="abc31-124">hello following example uses hello public CloudMediaContext(MediaServicesCredentials credentials) constructor:</span></span>

    // _cachedCredentials and _context are class member variables. 
    _cachedCredentials = new MediaServicesCredentials(
                    _mediaServicesAccountName,
                    _mediaServicesAccountKey);

    _context = new CloudMediaContext(_cachedCredentials);


## <a name="reusing-access-control-service-tokens"></a><span data-ttu-id="abc31-125">Opětovné použití tokeny služby Řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="abc31-125">Reusing Access Control Service Tokens</span></span>
<span data-ttu-id="abc31-126">Tato část uvádí, jak tooreuse služby Řízení přístupu tokeny pomocí CloudMediaContext konstruktory, které provést MediaServicesCredentials jako parametr.</span><span class="sxs-lookup"><span data-stu-id="abc31-126">This section shows how tooreuse Access Control Service tokens by using CloudMediaContext constructors that take MediaServicesCredentials as a parameter.</span></span>

<span data-ttu-id="abc31-127">[Azure Active Directory, řízení přístupu](https://msdn.microsoft.com/library/hh147631.aspx) (také označované jako služby Řízení přístupu nebo služby ACS) je služba založená na cloudu, která poskytuje snadný způsob ověřování a povolující přístupu uživatelů toogain tootheir webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="abc31-127">[Azure Active Directory Access Control](https://msdn.microsoft.com/library/hh147631.aspx) (also known as Access Control Service or ACS) is a cloud-based service that provides an easy way of authenticating and authorizing users toogain access tootheir web applications.</span></span> <span data-ttu-id="abc31-128">Microsoft Azure Media Services řídí přístup tooits služeb, když protokol OAuth, který vyžaduje tokenu služby ACS.</span><span class="sxs-lookup"><span data-stu-id="abc31-128">Microsoft Azure Media Services controls access tooits services though OAuth protocol that requires an ACS token.</span></span> <span data-ttu-id="abc31-129">Media Services přijímá tokeny služby ACS hello ze serveru ověřování.</span><span class="sxs-lookup"><span data-stu-id="abc31-129">Media Services receives hello ACS tokens from an authorization server.</span></span>

<span data-ttu-id="abc31-130">Při vývoji s hello sada SDK služby Media Services, protože můžete toonot pozornosti s tokeny hello hello správci kódu SDK je pro vás.</span><span class="sxs-lookup"><span data-stu-id="abc31-130">When developing with hello Media Services SDK, you can choose toonot deal with hello tokens because hello SDK code managers them for you.</span></span> <span data-ttu-id="abc31-131">Povolení hello SDK však plně spravovat hello ACS tokeny zájemců toounnecessary žádostí o token.</span><span class="sxs-lookup"><span data-stu-id="abc31-131">However, letting hello SDK fully manage hello ACS tokens leads toounnecessary token requests.</span></span> <span data-ttu-id="abc31-132">Požaduje tokeny trvá určitou dobu a spotřebovává prostředky klienta a server hello.</span><span class="sxs-lookup"><span data-stu-id="abc31-132">Requesting tokens takes time and consumes hello client and server resources.</span></span> <span data-ttu-id="abc31-133">Navíc omezí serveru ACS hello je generovaný hello požadavky, pokud hello míry je příliš vysoká.</span><span class="sxs-lookup"><span data-stu-id="abc31-133">Also, hello ACS server throttles hello requests if hello rate is too high.</span></span> <span data-ttu-id="abc31-134">Hello limit je 30 požadavků za sekundu, najdete v části [omezení služby ACS](https://msdn.microsoft.com/library/gg185909.aspx) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="abc31-134">hello limit is 30 requests per second, see [ACS Service Limitations](https://msdn.microsoft.com/library/gg185909.aspx) for more details.</span></span>

<span data-ttu-id="abc31-135">Počínaje hello sady Media Services SDK verze 3.0.0.0, můžete znovu použít hello tokeny služby ACS.</span><span class="sxs-lookup"><span data-stu-id="abc31-135">Starting with hello Media Services SDK version 3.0.0.0, you can reuse hello ACS tokens.</span></span> <span data-ttu-id="abc31-136">Hello **CloudMediaContext** konstruktory, které nepřijímají **MediaServicesCredentials** jako parametr Povolit sdílení tokeny hello ACS mezi více kontexty.</span><span class="sxs-lookup"><span data-stu-id="abc31-136">hello **CloudMediaContext** constructors that take **MediaServicesCredentials** as a parameter enable sharing hello ACS tokens between multiple contexts.</span></span> <span data-ttu-id="abc31-137">Hello MediaServicesCredentials třída zapouzdří přihlašovací údaje služby Media Services hello.</span><span class="sxs-lookup"><span data-stu-id="abc31-137">hello MediaServicesCredentials class encapsulates hello Media Services credentials.</span></span> <span data-ttu-id="abc31-138">Pokud je k dispozici tokenu služby ACS a znáte jeho čas vypršení platnosti, můžete vytvořit novou instanci MediaServicesCredentials s tokenem hello a předejte ji toohello konstruktoru CloudMediaContext.</span><span class="sxs-lookup"><span data-stu-id="abc31-138">If an ACS token is available and its expiration time is known, you can create a new MediaServicesCredentials instance with hello token and pass it toohello constructor of CloudMediaContext.</span></span> <span data-ttu-id="abc31-139">Všimněte si, že hello sady Media Services SDK automaticky aktualizuje tokeny vždy, když vypršení jejich platnosti.</span><span class="sxs-lookup"><span data-stu-id="abc31-139">Note that hello Media Services SDK automatically refreshes tokens whenever they expire.</span></span> <span data-ttu-id="abc31-140">Existují dva způsoby tooreuse tokeny služby ACS, jak je znázorněno v následující příklady hello.</span><span class="sxs-lookup"><span data-stu-id="abc31-140">There are two ways tooreuse ACS tokens, as shown in hello examples below.</span></span>

* <span data-ttu-id="abc31-141">Můžete mezipaměti hello **MediaServicesCredentials** objekt v paměti (například v proměnné statické třídy).</span><span class="sxs-lookup"><span data-stu-id="abc31-141">You can cache hello **MediaServicesCredentials** object in memory (for example, in a static class variable).</span></span> <span data-ttu-id="abc31-142">Pak předejte hello mezipaměti objekt toohello CloudMediaContext konstruktor.</span><span class="sxs-lookup"><span data-stu-id="abc31-142">Then, pass hello cached object toohello CloudMediaContext constructor.</span></span> <span data-ttu-id="abc31-143">objekt MediaServicesCredentials Hello obsahuje tokenu služby ACS, které můžete znovu použít, pokud je stále platný.</span><span class="sxs-lookup"><span data-stu-id="abc31-143">hello MediaServicesCredentials object contains an ACS token that can be reused if it is still valid.</span></span> <span data-ttu-id="abc31-144">Pokud hello token není platný, že se aktualizují podle hello Media Services SDK pomocí přihlašovacích údajů hello daného toohello MediaServicesCredentials konstruktor.</span><span class="sxs-lookup"><span data-stu-id="abc31-144">If hello token is not valid, it will be refreshed by hello Media Services SDK using hello credentials given toohello MediaServicesCredentials constructor.</span></span>
  
    <span data-ttu-id="abc31-145">Všimněte si, že hello **MediaServicesCredentials** objekt získá platný token po hello RefreshToken je volána.</span><span class="sxs-lookup"><span data-stu-id="abc31-145">Note that hello **MediaServicesCredentials** object gets a valid token after hello RefreshToken is called.</span></span> <span data-ttu-id="abc31-146">Hello **CloudMediaContext** volání hello **RefreshToken** metoda v konstruktoru hello.</span><span class="sxs-lookup"><span data-stu-id="abc31-146">hello **CloudMediaContext** calls hello **RefreshToken** method in hello constructor.</span></span> <span data-ttu-id="abc31-147">Pokud plánujete toosave hello hodnoty tokenu tooan externího úložiště, proveďte toocheck se, zda hello TokenExpiration hodnota je platná před uložením dat tokenů hello.</span><span class="sxs-lookup"><span data-stu-id="abc31-147">If you are planning toosave hello token values tooan external storage, make sure toocheck whether hello TokenExpiration value is valid before saving hello token data.</span></span> <span data-ttu-id="abc31-148">Pokud není platný, zavolejte RefreshToken před ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="abc31-148">If it is not valid, call RefreshToken before caching.</span></span>
  
        // Create and cache hello Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);

        // Use hello cached credentials toocreate a new CloudMediaContext object.
        if(_cachedCredentials == null)
        {
            _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);
        }

        CloudMediaContext context = new CloudMediaContext(_cachedCredentials);

* <span data-ttu-id="abc31-149">Můžete také mezipaměti hello AccessToken řetězec a hello TokenExpiration hodnoty.</span><span class="sxs-lookup"><span data-stu-id="abc31-149">You can also cache hello AccessToken string and hello TokenExpiration values.</span></span> <span data-ttu-id="abc31-150">hodnoty Hello později nelze použít toocreate nové MediaServicesCredentials objektu tokenu daty hello do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="abc31-150">hello values could later be used toocreate a new MediaServicesCredentials object with hello cached token data.</span></span>  <span data-ttu-id="abc31-151">To je obzvláště užitečná pro scénáře, kde hello token lze bezpečně sdílet mezi více procesy nebo počítače.</span><span class="sxs-lookup"><span data-stu-id="abc31-151">This is especially useful for scenarios where hello token can be securely shared among multiple processes or computers.</span></span>
  
    <span data-ttu-id="abc31-152">Hello následující fragmenty kódu volání hello SaveTokenDataToExternalStorage, GetTokenDataFromExternalStorage a UpdateTokenDataInExternalStorageIfNeeded metody, které nejsou v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="abc31-152">hello following code snippets call hello SaveTokenDataToExternalStorage, GetTokenDataFromExternalStorage, and UpdateTokenDataInExternalStorageIfNeeded methods that are not defined in this example.</span></span> <span data-ttu-id="abc31-153">Můžete třeba definovat tyto metody toostore, načtení a aktualizace dat tokenů v externím úložišti.</span><span class="sxs-lookup"><span data-stu-id="abc31-153">You could define these methods toostore, retrieve, and update token data in an external storage.</span></span> 
  
        CloudMediaContext context1 = new CloudMediaContext(_mediaServicesAccountName, _mediaServicesAccountKey);
  
        // Get token values from hello context.
        var accessToken = context1.Credentials.AccessToken;
        var tokenExpiration = context1.Credentials.TokenExpiration;
  
        // Save token values for later use. 
        // hello SaveTokenDataToExternalStorage method should check 
        // whether hello TokenExpiration value is valid before saving hello token data. 
        // If it is not valid, call MediaServicesCredentials’s RefreshToken before caching.
        SaveTokenDataToExternalStorage(accessToken, tokenExpiration);
  
    <span data-ttu-id="abc31-154">Použití hello uložit hodnoty tokenu toocreate MediaServicesCredentials.</span><span class="sxs-lookup"><span data-stu-id="abc31-154">Use hello saved token values toocreate MediaServicesCredentials.</span></span>

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

    <span data-ttu-id="abc31-155">Aktualizujte token kopii hello v případě, že hello token byl aktualizován hello sada SDK služby Media Services.</span><span class="sxs-lookup"><span data-stu-id="abc31-155">Update hello token copy in case hello token was updated by hello Media Services SDK.</span></span> 

        if(tokenExpiration != context2.Credentials.TokenExpiration)
        {
            UpdateTokenDataInExternalStorageIfNeeded(accessToken, context2.Credentials.TokenExpiration);
        }


* <span data-ttu-id="abc31-156">Pokud máte více účtů Media Services (například pro sdílení účely nebo Geo rozdělení zatížení) do mezipaměti můžete ukládat objekty MediaServicesCredentials pomocí kolekce System.Collections.Concurrent.ConcurrentDictionary hello (hello Kolekce ConcurrentDictionary představuje kolekci vláken párů klíč/hodnota, které můžete přistupovat pomocí více vláken současně).</span><span class="sxs-lookup"><span data-stu-id="abc31-156">If you have multiple Media Services accounts (for example, for load sharing purposes or Geo-distribution) you can cache MediaServicesCredentials objects using hello System.Collections.Concurrent.ConcurrentDictionary collection (hello ConcurrentDictionary collection represents a thread-safe collection of key/value pairs that can be accessed by multiple threads concurrently).</span></span> <span data-ttu-id="abc31-157">Potom můžete hello GetOrAdd metoda tooget hello do mezipaměti přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="abc31-157">You can then use hello GetOrAdd method tooget hello cached credentials.</span></span> 
  
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

## <a name="connecting-tooa-media-services-account-located-in-hello-north-china-region"></a><span data-ttu-id="abc31-158">Připojení účtu Media Services tooa nachází v oblasti severní Čína hello</span><span class="sxs-lookup"><span data-stu-id="abc31-158">Connecting tooa Media Services account located in hello North China region</span></span>
<span data-ttu-id="abc31-159">Pokud váš účet se nachází v oblasti severní Čína hello, použijte následující konstruktor hello:</span><span class="sxs-lookup"><span data-stu-id="abc31-159">If your account is located in hello North China region, use hello following constructor:</span></span>

    public CloudMediaContext(Uri apiServer, string accountName, string accountKey, string scope, string acsBaseAddress)

<span data-ttu-id="abc31-160">Například:</span><span class="sxs-lookup"><span data-stu-id="abc31-160">For example:</span></span>

    _context = new CloudMediaContext(
        new Uri("https://wamsbjbclus001rest-hs.chinacloudapp.cn/API/"),
        _mediaServicesAccountName,
        _mediaServicesAccountKey,
        "urn:WindowsAzureMediaServices",
        "https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn");


## <a name="storing-connection-values-in-configuration"></a><span data-ttu-id="abc31-161">Ukládání hodnot připojení v konfiguraci</span><span class="sxs-lookup"><span data-stu-id="abc31-161">Storing Connection Values in Configuration</span></span>
<span data-ttu-id="abc31-162">Je připojení hodnoty toostore vysoce doporučený postup, obzvláště citlivé hodnoty, například název účtu a heslo v konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="abc31-162">It is a highly recommended practice toostore connection values, especially sensitive values such as your account name and password, in configuration.</span></span> <span data-ttu-id="abc31-163">Je také doporučený postup tooencrypt citlivé konfigurační data.</span><span class="sxs-lookup"><span data-stu-id="abc31-163">Also, it is a recommended practice tooencrypt sensitive configuration data.</span></span> <span data-ttu-id="abc31-164">Hello celý konfigurační soubor můžete šifrovat pomocí hello Windows systém souborů (Encrypting File System).</span><span class="sxs-lookup"><span data-stu-id="abc31-164">You can encrypt hello entire configuration file by using hello Windows Encrypting File System (EFS).</span></span> <span data-ttu-id="abc31-165">Vyberte tooenable systémem souborů EFS v souboru, klikněte pravým tlačítkem na soubor hello, **vlastnosti**a povolilo se šifrování v hello **Upřesnit** kartě nastavení. Nebo můžete vytvořit vlastní řešení pro šifrování vybraných částí konfiguračního souboru pomocí chráněné konfigurace.</span><span class="sxs-lookup"><span data-stu-id="abc31-165">tooenable EFS on a file, right-click hello file, select **Properties**, and enable encryption in hello **Advanced** settings tab. Or you can create a custom solution for encrypting selected portions of a configuration file by using protected configuration.</span></span> <span data-ttu-id="abc31-166">V tématu [šifrování informace o konfiguraci pomocí chráněné konfigurace](https://msdn.microsoft.com/library/53tyfkaw.aspx).</span><span class="sxs-lookup"><span data-stu-id="abc31-166">See [Encrypting Configuration Information Using Protected Configuration](https://msdn.microsoft.com/library/53tyfkaw.aspx).</span></span>

<span data-ttu-id="abc31-167">Hello následující soubor App.config obsahuje hodnoty hello požadované připojení.</span><span class="sxs-lookup"><span data-stu-id="abc31-167">hello following App.config file contains hello required connection values.</span></span> <span data-ttu-id="abc31-168">Hello hodnoty v hello <appSettings> element jsou hello požadované hodnoty, které jste získali z procesu instalace účtu Media Services hello.</span><span class="sxs-lookup"><span data-stu-id="abc31-168">hello values in hello <appSettings> element are hello required values that you got from hello Media Services account setup process.</span></span>

    <configuration>
      <appSettings>
        <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
        <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
      </appSettings>
    </configuration>


<span data-ttu-id="abc31-169">hodnoty připojení tooretrieve z konfigurace, můžete použít hello **ConfigurationManager** třídy a pak mu přiřaďte hello hodnoty toofields ve vašem kódu:</span><span class="sxs-lookup"><span data-stu-id="abc31-169">tooretrieve connection values from configuration, you can use hello **ConfigurationManager** class and then assign hello values toofields in your code:</span></span>

    private static readonly string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
    private static readonly string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];



## <a name="media-services-learning-paths"></a><span data-ttu-id="abc31-170">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="abc31-170">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="abc31-171">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="abc31-171">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

