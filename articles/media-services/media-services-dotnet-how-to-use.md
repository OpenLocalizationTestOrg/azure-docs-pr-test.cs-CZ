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
# <a name="media-services-development-with-net"></a><span data-ttu-id="a51cd-104">Vývoj pro Media Services pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="a51cd-104">Media Services development with .NET</span></span>
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

<span data-ttu-id="a51cd-105">Toto téma popisuje, jak toostart vývoj Media Services aplikací s použitím rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="a51cd-105">This topic discusses how toostart developing Media Services applications using .NET.</span></span>

<span data-ttu-id="a51cd-106">Hello **Azure Media Services .NET SDK** knihovny vám umožní tooprogram proti Media Services pomocí rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="a51cd-106">hello **Azure Media Services .NET SDK** library enables you tooprogram against Media Services using .NET.</span></span> <span data-ttu-id="a51cd-107">toomake ho i jednodušší toodevelop s rozhraním .NET, hello **rozšíření Azure Media Services .NET SDK** knihovna je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="a51cd-107">toomake it even easier toodevelop with .NET, hello **Azure Media Services .NET SDK Extensions** library is provided.</span></span> <span data-ttu-id="a51cd-108">Tato knihovna obsahuje sadu metod rozšíření a pomocných funkcí, které zjednodušují kódu .NET.</span><span class="sxs-lookup"><span data-stu-id="a51cd-108">This library contains a set of extension methods and helper functions that simplify your .NET code.</span></span> <span data-ttu-id="a51cd-109">Obě knihovny jsou k dispozici prostřednictvím **NuGet** a **Githubu**.</span><span class="sxs-lookup"><span data-stu-id="a51cd-109">Both libraries are available through **NuGet** and **GitHub**.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a51cd-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a51cd-110">Prerequisites</span></span>
* <span data-ttu-id="a51cd-111">Účet Media Services v novém nebo existujícím předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="a51cd-111">A Media Services account in a new or existing Azure subscription.</span></span> <span data-ttu-id="a51cd-112">V tématu hello [jak tooCreate účtu Media Services](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="a51cd-112">See hello topic [How tooCreate a Media Services Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="a51cd-113">Operační systémy: Windows 10, Windows 7, Windows 2008 R2 nebo Windows 8.</span><span class="sxs-lookup"><span data-stu-id="a51cd-113">Operating Systems: Windows 10, Windows 7, Windows 2008 R2, or Windows 8.</span></span>
* <span data-ttu-id="a51cd-114">Rozhraní .NET framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="a51cd-114">.NET Framework 4.5.</span></span>
* <span data-ttu-id="a51cd-115">Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a51cd-115">Visual Studio.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="a51cd-116">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="a51cd-116">Create and configure a Visual Studio project</span></span>
<span data-ttu-id="a51cd-117">V této části se dozvíte, jak toocreate projektu v sadě Visual Studio a její nastavení pro vývoj pro Media Services.</span><span class="sxs-lookup"><span data-stu-id="a51cd-117">This section shows you how toocreate a project in Visual Studio and set it up for Media Services development.</span></span>  <span data-ttu-id="a51cd-118">V takovém případě hello projekt je konzolová aplikace C# Windows, ale hello stejné kroky nastavení, které jsou tady uvedené platí tooother typy projektů, které můžete vytvořit pro Media Services aplikace (například aplikace Windows Forms nebo webovou aplikaci ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="a51cd-118">In this case, hello project is a C# Windows console application, but hello same setup steps shown here apply tooother types of projects you can create for Media Services applications (for example, a Windows Forms application or an ASP.NET Web application).</span></span>

<span data-ttu-id="a51cd-119">Tato část uvádí, jak toouse **NuGet** tooadd sady Media Services .NET SDK rozšíření a další závislé knihovny.</span><span class="sxs-lookup"><span data-stu-id="a51cd-119">This section shows how toouse **NuGet** tooadd Media Services .NET SDK extensions and other dependent libraries.</span></span>

<span data-ttu-id="a51cd-120">Alternativně můžete získat nejnovější sadu Media Services .NET SDK bits hello z webu GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) nebo [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), vytvoření hello řešení a přidání projektu klienta toohello hello odkazy.</span><span class="sxs-lookup"><span data-stu-id="a51cd-120">Alternatively, you can get hello latest Media Services .NET SDK bits from GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) or [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), build hello solution, and add hello references toohello client project.</span></span> <span data-ttu-id="a51cd-121">Všechny potřebné závislosti hello získat stažené a rozbalené automaticky.</span><span class="sxs-lookup"><span data-stu-id="a51cd-121">All hello necessary dependencies get downloaded and extracted automatically.</span></span>

1. <span data-ttu-id="a51cd-122">Vytvořte novou konzolovou aplikaci v jazyce C# v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a51cd-122">Create a new C# Console Application in Visual Studio.</span></span> <span data-ttu-id="a51cd-123">Zadejte hello **název**, **umístění**, a **název řešení**a pak klikněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="a51cd-123">Enter hello **Name**, **Location**, and **Solution name**, and then click OK.</span></span>
2. <span data-ttu-id="a51cd-124">Vytvoření řešení hello.</span><span class="sxs-lookup"><span data-stu-id="a51cd-124">Build hello solution.</span></span>
3. <span data-ttu-id="a51cd-125">Použití **NuGet** tooinstall a přidejte **rozšíření Azure Media Services .NET SDK** (**windowsazure.mediaservices.extensions**).</span><span class="sxs-lookup"><span data-stu-id="a51cd-125">Use **NuGet** tooinstall and add **Azure Media Services .NET SDK Extensions** (**windowsazure.mediaservices.extensions**).</span></span> <span data-ttu-id="a51cd-126">Při instalaci tohoto balíčku se nainstaluje také **sada SDK služby Media Services pro .NET** a přidá všechny ostatní požadované závislosti.</span><span class="sxs-lookup"><span data-stu-id="a51cd-126">Installing this package, also installs **Media Services .NET SDK** and adds all other required dependencies.</span></span>
   
    <span data-ttu-id="a51cd-127">Ujistěte se, že máte nejnovější verzi hello nuget nainstalována.</span><span class="sxs-lookup"><span data-stu-id="a51cd-127">Ensure that you have hello newest version of NuGet installed.</span></span> <span data-ttu-id="a51cd-128">Další informace a pokyny pro instalaci, najdete v části [NuGet](http://nuget.codeplex.com/).</span><span class="sxs-lookup"><span data-stu-id="a51cd-128">For more information and installation instructions, see [NuGet](http://nuget.codeplex.com/).</span></span>
4. <span data-ttu-id="a51cd-129">V Průzkumníku řešení klikněte pravým tlačítkem na název hello hello projektu a zvolte spravovat balíčky NuGet balíčky.</span><span class="sxs-lookup"><span data-stu-id="a51cd-129">In Solution Explorer, right-click hello name of hello project and choose Manage NuGet packages.</span></span>
   
    <span data-ttu-id="a51cd-130">Zobrazí se dialogové okno Spravovat balíčky NuGet Hello.</span><span class="sxs-lookup"><span data-stu-id="a51cd-130">hello Manage NuGet Packages dialog box appears.</span></span>
5. <span data-ttu-id="a51cd-131">V hello Online galerie, vyhledejte MediaServices rozšíření Azure, zvolte rozšíření Azure Media Services .NET SDK a potom klikněte na tlačítko Instalovat hello.</span><span class="sxs-lookup"><span data-stu-id="a51cd-131">In hello Online gallery, search for Azure MediaServices Extensions, choose Azure Media Services .NET SDK Extensions, and then click hello Install button.</span></span>
   
    <span data-ttu-id="a51cd-132">Hello projekt se mění a odkazuje na toohello rozšíření sady SDK Media Services pro rozhraní .NET, sady Media Services .NET SDK a přidání dalších závislých sestavení.</span><span class="sxs-lookup"><span data-stu-id="a51cd-132">hello project is modified and references toohello Media Services .NET SDK Extensions,  Media Services .NET SDK, and other dependent assemblies are added.</span></span>
6. <span data-ttu-id="a51cd-133">toopromote čisticí vývojové prostředí, zvažte, povolení obnovení balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="a51cd-133">toopromote a cleaner development environment, consider enabling NuGet Package Restore.</span></span> <span data-ttu-id="a51cd-134">Další informace najdete v tématu [obnovení balíčků NuGet "](http://docs.nuget.org/consume/package-restore).</span><span class="sxs-lookup"><span data-stu-id="a51cd-134">For more information, see [NuGet Package Restore"](http://docs.nuget.org/consume/package-restore).</span></span>
7. <span data-ttu-id="a51cd-135">Přidat odkaz příliš**System.Configuration** sestavení.</span><span class="sxs-lookup"><span data-stu-id="a51cd-135">Add a reference too**System.Configuration** assembly.</span></span> <span data-ttu-id="a51cd-136">Toto sestavení obsahuje hello System.Configuration. **ConfigurationManager** tedy třídy používané tooaccess konfigurační soubory (například App.config).</span><span class="sxs-lookup"><span data-stu-id="a51cd-136">This assembly contains hello System.Configuration.**ConfigurationManager** class that is used tooaccess configuration files (for example, App.config).</span></span>
   
    <span data-ttu-id="a51cd-137">tooadd odkazů pomocí dialogu hello spravovat odkazy, klikněte pravým tlačítkem na název projektu hello v Průzkumníku řešení hello.</span><span class="sxs-lookup"><span data-stu-id="a51cd-137">tooadd references using hello Manage References dialog, right-click hello project name in hello Solution Explorer.</span></span> <span data-ttu-id="a51cd-138">Pak vyberte Přidat a odkazy.</span><span class="sxs-lookup"><span data-stu-id="a51cd-138">Then, select Add and References.</span></span>
   
    <span data-ttu-id="a51cd-139">Otevře se dialogové okno Spravovat odkazy Hello.</span><span class="sxs-lookup"><span data-stu-id="a51cd-139">hello Manage References dialog appears.</span></span>
8. <span data-ttu-id="a51cd-140">V části sestavení rozhraní .NET framework najít a vybrat sestavení System.Configuration hello a kliknutím na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="a51cd-140">Under .NET framework assemblies, find and select hello System.Configuration assembly and press OK.</span></span>
9. <span data-ttu-id="a51cd-141">Otevřete soubor App.config hello a přidejte *appSettings* soubor toohello oddílu.</span><span class="sxs-lookup"><span data-stu-id="a51cd-141">Open hello App.config file and add an *appSettings* section toohello file.</span></span>     
   
    <span data-ttu-id="a51cd-142">Nastavte hello hodnoty, které jsou potřebné tooconnect toohello Media Services API.</span><span class="sxs-lookup"><span data-stu-id="a51cd-142">Set hello values that are needed tooconnect toohello Media Services API.</span></span> <span data-ttu-id="a51cd-143">Další informace najdete v tématu [hello přístup k rozhraní API služby Azure Media Services pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="a51cd-143">For more information, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

    <span data-ttu-id="a51cd-144">Pokud používáte [ověření uživatele](media-services-use-aad-auth-to-access-ams-api.md#types-of-authentication) konfiguračního souboru bude pravděpodobně mít hodnoty pro doménu klienta Azure AD a hello koncový bod AMS REST API.</span><span class="sxs-lookup"><span data-stu-id="a51cd-144">If you are using [user authentication](media-services-use-aad-auth-to-access-ams-api.md#types-of-authentication) your config file will probably have values for your Azure AD tenant domain and hello AMS REST API endpoint.</span></span>
    
    >[!Important]
    ><span data-ttu-id="a51cd-145">Většina ukázky kódu v dokumentaci k Azure Media Services hello nastavit, použijte uživatele (interaktivní) typ ověřování tooconnect toohello rozhraní API pro AMS.</span><span class="sxs-lookup"><span data-stu-id="a51cd-145">Most code samples in hello Azure Media Services documentation set, use a user (interactive) type of authentication tooconnect toohello AMS API.</span></span> <span data-ttu-id="a51cd-146">Tato metoda ověřování bude fungovat i pro správu nebo monitorování nativní aplikace: mobilní aplikace, aplikace pro Windows a konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a51cd-146">This authentication method will work well for management or monitoring native apps: mobile apps, Windows apps, and Console applications.</span></span> <span data-ttu-id="a51cd-147">Tato metoda ověřování není vhodný pro server, webové služby, rozhraní API typu aplikací.</span><span class="sxs-lookup"><span data-stu-id="a51cd-147">This authentication method is not suitable for server, web services, APIs type of applications.</span></span>  <span data-ttu-id="a51cd-148">Další informace najdete v tématu [hello přístup k rozhraní API pro AMS při ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="a51cd-148">For more information, see [Access hello AMS API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span>

        <configuration>
        ...
            <appSettings>
              <add key="AADTenantDomain" value="YourAADTenantDomain" />
              <add key="MediaServiceRESTAPIEndpoint" value="YourRESTAPIEndpoint" />
            </appSettings>

        </configuration>

10. <span data-ttu-id="a51cd-149">Přepsat existující hello **pomocí** příkazy od hello začátku souboru Program.cs hello s hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="a51cd-149">Overwrite hello existing **using** statements at hello beginning of hello Program.cs file with hello following code.</span></span>
           
        using System;
        using System.Configuration;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Collections.Generic;
        using System.Linq;

<span data-ttu-id="a51cd-150">V tomto okamžiku jsou připravené toostart vývoj aplikace s Media Services.</span><span class="sxs-lookup"><span data-stu-id="a51cd-150">At this point, you are ready toostart developing a Media Services application.</span></span>    

## <a name="example"></a><span data-ttu-id="a51cd-151">Příklad</span><span class="sxs-lookup"><span data-stu-id="a51cd-151">Example</span></span>

<span data-ttu-id="a51cd-152">Zde je malý příklad, který se připojuje toohello AMS rozhraní API a uvádí všechny dostupné procesory médií.</span><span class="sxs-lookup"><span data-stu-id="a51cd-152">Here is a small example that connects toohello AMS API and lists all available Media Processors.</span></span>
    
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

##<a name="next-steps"></a><span data-ttu-id="a51cd-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a51cd-153">Next steps</span></span>

<span data-ttu-id="a51cd-154">Nyní [toohello AMS rozhraní API se můžete připojit](media-services-use-aad-auth-to-access-ams-api.md) a spusťte [vývoj](media-services-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a51cd-154">Now [you can connect toohello AMS API](media-services-use-aad-auth-to-access-ams-api.md) and start [developing](media-services-dotnet-get-started.md).</span></span>


## <a name="media-services-learning-paths"></a><span data-ttu-id="a51cd-155">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="a51cd-155">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="a51cd-156">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="a51cd-156">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

