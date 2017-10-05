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
# <a name="connecting-to-media-services-account-using-media-services-sdk-for-net"></a><span data-ttu-id="570fc-103">Připojení k účtu Media Services pomocí sady Media Services SDK pro .NET</span><span class="sxs-lookup"><span data-stu-id="570fc-103">Connecting to Media Services Account using Media Services SDK for .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="570fc-104">REST</span><span class="sxs-lookup"><span data-stu-id="570fc-104">REST</span></span>](media-services-rest-connect-programmatically.md)
> * [<span data-ttu-id="570fc-105">.NET</span><span class="sxs-lookup"><span data-stu-id="570fc-105">.NET</span></span>](media-services-dotnet-connect-programmatically.md)
> 
> 

<span data-ttu-id="570fc-106">Toto téma popisuje, jak získat programové připojení k Microsoft Azure Media Services, pokud jsou programování pomocí sady Media Services SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="570fc-106">This topic describes how to obtain a programmatic connection to Microsoft Azure Media Services when you are programming with the Media Services SDK for .NET.</span></span>

## <a name="connecting-to-media-services"></a><span data-ttu-id="570fc-107">Připojení ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="570fc-107">Connecting to Media Services</span></span>
<span data-ttu-id="570fc-108">Pro připojení ke službě Media Services prostřednictvím kódu programu, můžete musí mít dříve nastavil účet Azure, nakonfigurované na tento účet Media Services a nastavit projekt sady Visual Studio pro vývoj pomocí sady Media Services SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="570fc-108">To connect to Media Services programmatically, you must have previously set up an Azure account, configured Media Services on that account, and then set up a Visual Studio project for development with the Media Services SDK for .NET.</span></span> <span data-ttu-id="570fc-109">Další informace najdete v tématu Nastavení pro vývoj pomocí sady Media Services SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="570fc-109">For more information, see Setup for Development with the Media Services SDK for .NET.</span></span>

<span data-ttu-id="570fc-110">Na konci procesu instalace účtu Media Services můžete získat následující hodnoty požadované připojení.</span><span class="sxs-lookup"><span data-stu-id="570fc-110">At the end of the Media Services account setup process, you obtained the following required connection values.</span></span> <span data-ttu-id="570fc-111">Použijí pro provedení programové připojení ke službě Media Services.</span><span class="sxs-lookup"><span data-stu-id="570fc-111">Use these to make programmatic connections to Media Services.</span></span>

* <span data-ttu-id="570fc-112">Název účtu Media Services.</span><span class="sxs-lookup"><span data-stu-id="570fc-112">Your Media Services account name.</span></span>
* <span data-ttu-id="570fc-113">Klíče účtu Media Services.</span><span class="sxs-lookup"><span data-stu-id="570fc-113">Your Media Services account key.</span></span>

<span data-ttu-id="570fc-114">Tyto hodnoty, najdete na portálu Azure Management Portal, vyberte svůj účet Media Service a klikněte na "**Správa KLÍČŮ**" v dolní části okna portálu na ikonu.</span><span class="sxs-lookup"><span data-stu-id="570fc-114">To find these values, go to the Azure Managment Portal, select your Media Service account, and click on the “**MANAGE KEYS**” icon on the bottom of the portal window.</span></span> <span data-ttu-id="570fc-115">Kliknutím na ikonu vedle každého textového pole zkopírujete příslušnou hodnotu do schránky systému.</span><span class="sxs-lookup"><span data-stu-id="570fc-115">Clicking on the icon next to each text box copies the value to the system clipboard.</span></span>

## <a name="creating-a-cloudmediacontext-instance"></a><span data-ttu-id="570fc-116">Vytvoření CloudMediaContext Instance</span><span class="sxs-lookup"><span data-stu-id="570fc-116">Creating a CloudMediaContext Instance</span></span>
<span data-ttu-id="570fc-117">Spusťte programové ošetření Media Services je nutné vytvořit **CloudMediaContext** instanci, která představuje kontext serveru.</span><span class="sxs-lookup"><span data-stu-id="570fc-117">To start programming against Media Services you need to create a **CloudMediaContext** instance that represents the server context.</span></span> <span data-ttu-id="570fc-118">**CloudMediaContext** obsahuje odkazy na důležité kolekcí včetně úloh, prostředky, soubory, zásady přístupu a lokátory.</span><span class="sxs-lookup"><span data-stu-id="570fc-118">The **CloudMediaContext** includes references to important collections including jobs, assets, files, access policies, and locators.</span></span>

> [!NOTE]
> <span data-ttu-id="570fc-119">**CloudMediaContext** třída není bezpečná pro přístup z více vláken.</span><span class="sxs-lookup"><span data-stu-id="570fc-119">The **CloudMediaContext** class is not thread safe.</span></span> <span data-ttu-id="570fc-120">Měli byste vytvořit nový CloudMediaContext na vlákno nebo jednotlivou sadu operací.</span><span class="sxs-lookup"><span data-stu-id="570fc-120">You should create a new CloudMediaContext per thread or per set of operations.</span></span>
> 
> 

<span data-ttu-id="570fc-121">CloudMediaContext má pět přetížení konstruktor.</span><span class="sxs-lookup"><span data-stu-id="570fc-121">CloudMediaContext has five constructor overloads.</span></span> <span data-ttu-id="570fc-122">Doporučuje se použít konstruktory, které nepřijímají **MediaServicesCredentials** jako parametr.</span><span class="sxs-lookup"><span data-stu-id="570fc-122">It is recommended to use constructors that take **MediaServicesCredentials** as a parameter.</span></span> <span data-ttu-id="570fc-123">Další informace najdete v tématu **opakovaného použití tokeny služby Řízení přístupu** , který následuje.</span><span class="sxs-lookup"><span data-stu-id="570fc-123">For more information, see the **Reusing Access Control Service Tokens** that follows.</span></span> 

<span data-ttu-id="570fc-124">Následující příklad používá veřejný konstruktor CloudMediaContext(MediaServicesCredentials credentials):</span><span class="sxs-lookup"><span data-stu-id="570fc-124">The following example uses the public CloudMediaContext(MediaServicesCredentials credentials) constructor:</span></span>

    // _cachedCredentials and _context are class member variables. 
    _cachedCredentials = new MediaServicesCredentials(
                    _mediaServicesAccountName,
                    _mediaServicesAccountKey);

    _context = new CloudMediaContext(_cachedCredentials);


## <a name="reusing-access-control-service-tokens"></a><span data-ttu-id="570fc-125">Opětovné použití tokeny služby Řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="570fc-125">Reusing Access Control Service Tokens</span></span>
<span data-ttu-id="570fc-126">V této části ukazuje, jak znovu použít tokeny služby Řízení přístupu pomocí CloudMediaContext konstruktory, které provést MediaServicesCredentials jako parametr.</span><span class="sxs-lookup"><span data-stu-id="570fc-126">This section shows how to reuse Access Control Service tokens by using CloudMediaContext constructors that take MediaServicesCredentials as a parameter.</span></span>

<span data-ttu-id="570fc-127">[Azure Active Directory, řízení přístupu](https://msdn.microsoft.com/library/hh147631.aspx) (také označované jako služby Řízení přístupu nebo služby ACS) je Cloudová služba, která poskytuje snadný způsob ověřování a autorizace uživatelů pro přístup do své webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="570fc-127">[Azure Active Directory Access Control](https://msdn.microsoft.com/library/hh147631.aspx) (also known as Access Control Service or ACS) is a cloud-based service that provides an easy way of authenticating and authorizing users to gain access to their web applications.</span></span> <span data-ttu-id="570fc-128">Microsoft Azure Media Services řídí přístup k jeho služby ale protokolu OAuth, který vyžaduje tokenu služby ACS.</span><span class="sxs-lookup"><span data-stu-id="570fc-128">Microsoft Azure Media Services controls access to its services though OAuth protocol that requires an ACS token.</span></span> <span data-ttu-id="570fc-129">Media Services přijímá tokeny služby ACS ze serveru ověřování.</span><span class="sxs-lookup"><span data-stu-id="570fc-129">Media Services receives the ACS tokens from an authorization server.</span></span>

<span data-ttu-id="570fc-130">Při vývoj pomocí sady Media Services SDK, je možné není zacházet s tokeny, protože sada SDK kód správce je pro vás.</span><span class="sxs-lookup"><span data-stu-id="570fc-130">When developing with the Media Services SDK, you can choose to not deal with the tokens because the SDK code managers them for you.</span></span> <span data-ttu-id="570fc-131">Ale když necháte SDK plně spravovat tokeny služby ACS vede k nepotřebné žádostí o token.</span><span class="sxs-lookup"><span data-stu-id="570fc-131">However, letting the SDK fully manage the ACS tokens leads to unnecessary token requests.</span></span> <span data-ttu-id="570fc-132">Požaduje tokeny trvá určitou dobu a spotřebovává prostředky klienta a serveru.</span><span class="sxs-lookup"><span data-stu-id="570fc-132">Requesting tokens takes time and consumes the client and server resources.</span></span> <span data-ttu-id="570fc-133">Navíc omezí server služby ACS je generovaný žádosti, pokud je příliš vysoká rychlost.</span><span class="sxs-lookup"><span data-stu-id="570fc-133">Also, the ACS server throttles the requests if the rate is too high.</span></span> <span data-ttu-id="570fc-134">Limit je 30 požadavků za sekundu, najdete v části [omezení služby ACS](https://msdn.microsoft.com/library/gg185909.aspx) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="570fc-134">The limit is 30 requests per second, see [ACS Service Limitations](https://msdn.microsoft.com/library/gg185909.aspx) for more details.</span></span>

<span data-ttu-id="570fc-135">Od verze sady Media Services SDK verze 3.0.0.0, můžete znovu použít tokeny služby ACS.</span><span class="sxs-lookup"><span data-stu-id="570fc-135">Starting with the Media Services SDK version 3.0.0.0, you can reuse the ACS tokens.</span></span> <span data-ttu-id="570fc-136">**CloudMediaContext** konstruktory, které nepřijímají **MediaServicesCredentials** jako parametr Povolit sdílení mezi více kontexty tokeny služby ACS.</span><span class="sxs-lookup"><span data-stu-id="570fc-136">The **CloudMediaContext** constructors that take **MediaServicesCredentials** as a parameter enable sharing the ACS tokens between multiple contexts.</span></span> <span data-ttu-id="570fc-137">MediaServicesCredentials třída zapouzdří přihlašovací údaje služby Media Services.</span><span class="sxs-lookup"><span data-stu-id="570fc-137">The MediaServicesCredentials class encapsulates the Media Services credentials.</span></span> <span data-ttu-id="570fc-138">Pokud je k dispozici tokenu služby ACS a znáte jeho čas vypršení platnosti, můžete vytvořit novou instanci MediaServicesCredentials s tokenem a předejte ji do konstruktoru objektu CloudMediaContext.</span><span class="sxs-lookup"><span data-stu-id="570fc-138">If an ACS token is available and its expiration time is known, you can create a new MediaServicesCredentials instance with the token and pass it to the constructor of CloudMediaContext.</span></span> <span data-ttu-id="570fc-139">Všimněte si, že sady Media Services SDK automaticky aktualizuje tokeny vždy, když vypršení jejich platnosti.</span><span class="sxs-lookup"><span data-stu-id="570fc-139">Note that the Media Services SDK automatically refreshes tokens whenever they expire.</span></span> <span data-ttu-id="570fc-140">Existují dva způsoby, jak znovu použít tokeny služby ACS, jak je znázorněno v následujících příkladech.</span><span class="sxs-lookup"><span data-stu-id="570fc-140">There are two ways to reuse ACS tokens, as shown in the examples below.</span></span>

* <span data-ttu-id="570fc-141">Může ukládat do mezipaměti **MediaServicesCredentials** objekt v paměti (například v proměnné statické třídy).</span><span class="sxs-lookup"><span data-stu-id="570fc-141">You can cache the **MediaServicesCredentials** object in memory (for example, in a static class variable).</span></span> <span data-ttu-id="570fc-142">Pak předejte objektu v mezipaměti CloudMediaContext konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="570fc-142">Then, pass the cached object to the CloudMediaContext constructor.</span></span> <span data-ttu-id="570fc-143">Objekt MediaServicesCredentials obsahuje tokenu služby ACS, které můžete znovu použít, pokud je stále platný.</span><span class="sxs-lookup"><span data-stu-id="570fc-143">The MediaServicesCredentials object contains an ACS token that can be reused if it is still valid.</span></span> <span data-ttu-id="570fc-144">Pokud token není platný, se aktualizují pomocí sady Media Services SDK pomocí přihlašovacích údajů na MediaServicesCredentials konstruktor.</span><span class="sxs-lookup"><span data-stu-id="570fc-144">If the token is not valid, it will be refreshed by the Media Services SDK using the credentials given to the MediaServicesCredentials constructor.</span></span>
  
    <span data-ttu-id="570fc-145">Všimněte si, že **MediaServicesCredentials** objekt získá platný token po volání RefreshToken.</span><span class="sxs-lookup"><span data-stu-id="570fc-145">Note that the **MediaServicesCredentials** object gets a valid token after the RefreshToken is called.</span></span> <span data-ttu-id="570fc-146">**CloudMediaContext** volání **RefreshToken** metoda v konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="570fc-146">The **CloudMediaContext** calls the **RefreshToken** method in the constructor.</span></span> <span data-ttu-id="570fc-147">Pokud máte v úmyslu uložit hodnoty tokenu do externího úložiště, nezapomeňte zkontrolovat, zda je hodnota TokenExpiration platný před uložením dat tokenů.</span><span class="sxs-lookup"><span data-stu-id="570fc-147">If you are planning to save the token values to an external storage, make sure to check whether the TokenExpiration value is valid before saving the token data.</span></span> <span data-ttu-id="570fc-148">Pokud není platný, zavolejte RefreshToken před ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="570fc-148">If it is not valid, call RefreshToken before caching.</span></span>
  
        // Create and cache the Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);

        // Use the cached credentials to create a new CloudMediaContext object.
        if(_cachedCredentials == null)
        {
            _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);
        }

        CloudMediaContext context = new CloudMediaContext(_cachedCredentials);

* <span data-ttu-id="570fc-149">Můžete také mezipaměti řetězec AccessToken a TokenExpiration hodnoty.</span><span class="sxs-lookup"><span data-stu-id="570fc-149">You can also cache the AccessToken string and the TokenExpiration values.</span></span> <span data-ttu-id="570fc-150">Hodnoty může později použije k vytvoření nového objektu MediaServicesCredentials se data uložená v mezipaměti tokenu.</span><span class="sxs-lookup"><span data-stu-id="570fc-150">The values could later be used to create a new MediaServicesCredentials object with the cached token data.</span></span>  <span data-ttu-id="570fc-151">To je obzvláště užitečná pro scénáře, kde token lze bezpečně sdílet mezi více procesy nebo počítače.</span><span class="sxs-lookup"><span data-stu-id="570fc-151">This is especially useful for scenarios where the token can be securely shared among multiple processes or computers.</span></span>
  
    <span data-ttu-id="570fc-152">Následující fragmenty kódu volání metody SaveTokenDataToExternalStorage, GetTokenDataFromExternalStorage a UpdateTokenDataInExternalStorageIfNeeded, které nejsou definované v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="570fc-152">The following code snippets call the SaveTokenDataToExternalStorage, GetTokenDataFromExternalStorage, and UpdateTokenDataInExternalStorageIfNeeded methods that are not defined in this example.</span></span> <span data-ttu-id="570fc-153">Můžete třeba definovat tyto metody ukládání, načíst a aktualizovat token data v externím úložišti.</span><span class="sxs-lookup"><span data-stu-id="570fc-153">You could define these methods to store, retrieve, and update token data in an external storage.</span></span> 
  
        CloudMediaContext context1 = new CloudMediaContext(_mediaServicesAccountName, _mediaServicesAccountKey);
  
        // Get token values from the context.
        var accessToken = context1.Credentials.AccessToken;
        var tokenExpiration = context1.Credentials.TokenExpiration;
  
        // Save token values for later use. 
        // The SaveTokenDataToExternalStorage method should check 
        // whether the TokenExpiration value is valid before saving the token data. 
        // If it is not valid, call MediaServicesCredentials’s RefreshToken before caching.
        SaveTokenDataToExternalStorage(accessToken, tokenExpiration);
  
    <span data-ttu-id="570fc-154">Uložené hodnoty tokenu použijte k vytvoření MediaServicesCredentials.</span><span class="sxs-lookup"><span data-stu-id="570fc-154">Use the saved token values to create MediaServicesCredentials.</span></span>

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

    <span data-ttu-id="570fc-155">Aktualizujte token kopii v případě, že token byl aktualizován pomocí sady Media Services SDK.</span><span class="sxs-lookup"><span data-stu-id="570fc-155">Update the token copy in case the token was updated by the Media Services SDK.</span></span> 

        if(tokenExpiration != context2.Credentials.TokenExpiration)
        {
            UpdateTokenDataInExternalStorageIfNeeded(accessToken, context2.Credentials.TokenExpiration);
        }


* <span data-ttu-id="570fc-156">Pokud máte více účtů Media Services (například pro sdílení účely nebo Geo rozdělení zatížení) do mezipaměti můžete ukládat objekty MediaServicesCredentials pomocí kolekce System.Collections.Concurrent.ConcurrentDictionary (kolekci ConcurrentDictionary představuje kolekci vláken párů klíč/hodnota, které můžete přistupovat pomocí více vláken současně).</span><span class="sxs-lookup"><span data-stu-id="570fc-156">If you have multiple Media Services accounts (for example, for load sharing purposes or Geo-distribution) you can cache MediaServicesCredentials objects using the System.Collections.Concurrent.ConcurrentDictionary collection (the ConcurrentDictionary collection represents a thread-safe collection of key/value pairs that can be accessed by multiple threads concurrently).</span></span> <span data-ttu-id="570fc-157">Pak můžete použít metodu GetOrAdd získat přihlašovací údaje v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="570fc-157">You can then use the GetOrAdd method to get the cached credentials.</span></span> 
  
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

## <a name="connecting-to-a-media-services-account-located-in-the-north-china-region"></a><span data-ttu-id="570fc-158">Připojení k účtu Media Services nachází v oblasti severní Čína</span><span class="sxs-lookup"><span data-stu-id="570fc-158">Connecting to a Media Services account located in the North China region</span></span>
<span data-ttu-id="570fc-159">Pokud váš účet se nachází v oblasti severní Čína, použijte následující konstruktor:</span><span class="sxs-lookup"><span data-stu-id="570fc-159">If your account is located in the North China region, use the following constructor:</span></span>

    public CloudMediaContext(Uri apiServer, string accountName, string accountKey, string scope, string acsBaseAddress)

<span data-ttu-id="570fc-160">Například:</span><span class="sxs-lookup"><span data-stu-id="570fc-160">For example:</span></span>

    _context = new CloudMediaContext(
        new Uri("https://wamsbjbclus001rest-hs.chinacloudapp.cn/API/"),
        _mediaServicesAccountName,
        _mediaServicesAccountKey,
        "urn:WindowsAzureMediaServices",
        "https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn");


## <a name="storing-connection-values-in-configuration"></a><span data-ttu-id="570fc-161">Ukládání hodnot připojení v konfiguraci</span><span class="sxs-lookup"><span data-stu-id="570fc-161">Storing Connection Values in Configuration</span></span>
<span data-ttu-id="570fc-162">Je vysoce doporučený postup pro uložení připojení hodnot, jako je například název účtu a hesla, obzvláště citlivé hodnoty v konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="570fc-162">It is a highly recommended practice to store connection values, especially sensitive values such as your account name and password, in configuration.</span></span> <span data-ttu-id="570fc-163">Také je doporučený postup pro šifrování citlivých konfigurační data.</span><span class="sxs-lookup"><span data-stu-id="570fc-163">Also, it is a recommended practice to encrypt sensitive configuration data.</span></span> <span data-ttu-id="570fc-164">Pomocí Windows systém souborů (Encrypting File System) můžete šifrovat celý konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="570fc-164">You can encrypt the entire configuration file by using the Windows Encrypting File System (EFS).</span></span> <span data-ttu-id="570fc-165">Chcete-li povolit systémem souborů EFS na soubor, klikněte pravým tlačítkem na soubor, vyberte možnost **vlastnosti**a povolit šifrování v **Upřesnit** kartě nastavení.</span><span class="sxs-lookup"><span data-stu-id="570fc-165">To enable EFS on a file, right-click the file, select **Properties**, and enable encryption in the **Advanced** settings tab.</span></span> <span data-ttu-id="570fc-166">Nebo můžete vytvořit vlastní řešení pro šifrování vybraných částí konfiguračního souboru pomocí chráněné konfigurace.</span><span class="sxs-lookup"><span data-stu-id="570fc-166">Or you can create a custom solution for encrypting selected portions of a configuration file by using protected configuration.</span></span> <span data-ttu-id="570fc-167">V tématu [šifrování informace o konfiguraci pomocí chráněné konfigurace](https://msdn.microsoft.com/library/53tyfkaw.aspx).</span><span class="sxs-lookup"><span data-stu-id="570fc-167">See [Encrypting Configuration Information Using Protected Configuration](https://msdn.microsoft.com/library/53tyfkaw.aspx).</span></span>

<span data-ttu-id="570fc-168">Následující soubor App.config obsahuje hodnoty požadované připojení.</span><span class="sxs-lookup"><span data-stu-id="570fc-168">The following App.config file contains the required connection values.</span></span> <span data-ttu-id="570fc-169">Hodnoty <appSettings> element jsou požadované hodnoty, které jste získali z instalačního procesu účtu Media Services.</span><span class="sxs-lookup"><span data-stu-id="570fc-169">The values in the <appSettings> element are the required values that you got from the Media Services account setup process.</span></span>

    <configuration>
      <appSettings>
        <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
        <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
      </appSettings>
    </configuration>


<span data-ttu-id="570fc-170">Chcete-li načíst hodnoty připojení z konfigurace, můžete použít **ConfigurationManager** třídy a pak mu přiřaďte hodnoty polí ve vašem kódu:</span><span class="sxs-lookup"><span data-stu-id="570fc-170">To retrieve connection values from configuration, you can use the **ConfigurationManager** class and then assign the values to fields in your code:</span></span>

    private static readonly string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
    private static readonly string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];



## <a name="media-services-learning-paths"></a><span data-ttu-id="570fc-171">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="570fc-171">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="570fc-172">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="570fc-172">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

