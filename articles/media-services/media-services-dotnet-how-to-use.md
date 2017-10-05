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
# <a name="media-services-development-with-net"></a><span data-ttu-id="e4ff1-104">Vývoj pro Media Services pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="e4ff1-104">Media Services development with .NET</span></span>
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

<span data-ttu-id="e4ff1-105">Toto téma popisuje, jak chcete začít vyvíjet aplikace Media Services pomocí rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-105">This topic discusses how to start developing Media Services applications using .NET.</span></span>

<span data-ttu-id="e4ff1-106">**Azure Media Services .NET SDK** knihovny umožňuje programu proti Media Services pomocí rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-106">The **Azure Media Services .NET SDK** library enables you to program against Media Services using .NET.</span></span> <span data-ttu-id="e4ff1-107">Aby bylo snazší i pro vývoj pomocí rozhraní .NET, **rozšíření Azure Media Services .NET SDK** knihovna je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-107">To make it even easier to develop with .NET, the **Azure Media Services .NET SDK Extensions** library is provided.</span></span> <span data-ttu-id="e4ff1-108">Tato knihovna obsahuje sadu metod rozšíření a pomocných funkcí, které zjednodušují kódu .NET.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-108">This library contains a set of extension methods and helper functions that simplify your .NET code.</span></span> <span data-ttu-id="e4ff1-109">Obě knihovny jsou k dispozici prostřednictvím **NuGet** a **Githubu**.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-109">Both libraries are available through **NuGet** and **GitHub**.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4ff1-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e4ff1-110">Prerequisites</span></span>
* <span data-ttu-id="e4ff1-111">Účet Media Services v novém nebo existujícím předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-111">A Media Services account in a new or existing Azure subscription.</span></span> <span data-ttu-id="e4ff1-112">Další informace najdete v tématu [Vytvoření účtu Media Services](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="e4ff1-112">See the topic [How to Create a Media Services Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="e4ff1-113">Operační systémy: Windows 10, Windows 7, Windows 2008 R2 nebo Windows 8.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-113">Operating Systems: Windows 10, Windows 7, Windows 2008 R2, or Windows 8.</span></span>
* <span data-ttu-id="e4ff1-114">Rozhraní .NET framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-114">.NET Framework 4.5.</span></span>
* <span data-ttu-id="e4ff1-115">Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-115">Visual Studio.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="e4ff1-116">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="e4ff1-116">Create and configure a Visual Studio project</span></span>
<span data-ttu-id="e4ff1-117">V této části se dozvíte, jak k vytvoření projektu v sadě Visual Studio a její nastavení pro vývoj pro Media Services.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-117">This section shows you how to create a project in Visual Studio and set it up for Media Services development.</span></span>  <span data-ttu-id="e4ff1-118">V takovém případě projekt je konzolová aplikace C# Windows, ale stejný postup instalace, které jsou tady uvedené platí pro jiné typy projektů, které můžete vytvořit pro Media Services aplikace (například aplikace Windows Forms nebo webovou aplikaci ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="e4ff1-118">In this case, the project is a C# Windows console application, but the same setup steps shown here apply to other types of projects you can create for Media Services applications (for example, a Windows Forms application or an ASP.NET Web application).</span></span>

<span data-ttu-id="e4ff1-119">Tato část ukazuje způsob použití **NuGet** přidat rozšíření sady Media Services .NET SDK a další závislé knihovny.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-119">This section shows how to use **NuGet** to add Media Services .NET SDK extensions and other dependent libraries.</span></span>

<span data-ttu-id="e4ff1-120">Alternativně můžete získat nejnovější sadu Media Services .NET SDK bits z webu GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) nebo [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), sestavte řešení a přidejte odkazy na projektu klienta.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-120">Alternatively, you can get the latest Media Services .NET SDK bits from GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) or [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), build the solution, and add the references to the client project.</span></span> <span data-ttu-id="e4ff1-121">Všechny potřebné závislosti získat stažené a rozbalené automaticky.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-121">All the necessary dependencies get downloaded and extracted automatically.</span></span>

1. <span data-ttu-id="e4ff1-122">Vytvořte novou konzolovou aplikaci v jazyce C# v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-122">Create a new C# Console Application in Visual Studio.</span></span> <span data-ttu-id="e4ff1-123">Zadejte **název**, **umístění**, a **název řešení**a pak klikněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-123">Enter the **Name**, **Location**, and **Solution name**, and then click OK.</span></span>
2. <span data-ttu-id="e4ff1-124">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-124">Build the solution.</span></span>
3. <span data-ttu-id="e4ff1-125">Použití **NuGet** k instalaci a přidání **rozšíření Azure Media Services .NET SDK** (**windowsazure.mediaservices.extensions**).</span><span class="sxs-lookup"><span data-stu-id="e4ff1-125">Use **NuGet** to install and add **Azure Media Services .NET SDK Extensions** (**windowsazure.mediaservices.extensions**).</span></span> <span data-ttu-id="e4ff1-126">Při instalaci tohoto balíčku se nainstaluje také **sada SDK služby Media Services pro .NET** a přidá všechny ostatní požadované závislosti.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-126">Installing this package, also installs **Media Services .NET SDK** and adds all other required dependencies.</span></span>
   
    <span data-ttu-id="e4ff1-127">Ujistěte se, že máte nejnovější verzi balíčku nuget, které jsou nainstalované.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-127">Ensure that you have the newest version of NuGet installed.</span></span> <span data-ttu-id="e4ff1-128">Další informace a pokyny pro instalaci, najdete v části [NuGet](http://nuget.codeplex.com/).</span><span class="sxs-lookup"><span data-stu-id="e4ff1-128">For more information and installation instructions, see [NuGet](http://nuget.codeplex.com/).</span></span>
4. <span data-ttu-id="e4ff1-129">V Průzkumníku řešení klikněte pravým tlačítkem na název projektu a zvolte spravovat balíčky NuGet balíčky.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-129">In Solution Explorer, right-click the name of the project and choose Manage NuGet packages.</span></span>
   
    <span data-ttu-id="e4ff1-130">Zobrazí se dialogové okno Spravovat balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-130">The Manage NuGet Packages dialog box appears.</span></span>
5. <span data-ttu-id="e4ff1-131">V galerii Online vyhledejte MediaServices rozšíření Azure, zvolte rozšíření Azure Media Services .NET SDK a potom klikněte na tlačítko Instalovat.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-131">In the Online gallery, search for Azure MediaServices Extensions, choose Azure Media Services .NET SDK Extensions, and then click the Install button.</span></span>
   
    <span data-ttu-id="e4ff1-132">Projekt se mění a odkazy na rozšíření Media Services .NET SDK, sady Media Services .NET SDK a dalších závislých sestavení jsou přidány.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-132">The project is modified and references to the Media Services .NET SDK Extensions,  Media Services .NET SDK, and other dependent assemblies are added.</span></span>
6. <span data-ttu-id="e4ff1-133">Ke zvýšení úrovně čisticí vývojového prostředí, zvažte povolení obnovení balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-133">To promote a cleaner development environment, consider enabling NuGet Package Restore.</span></span> <span data-ttu-id="e4ff1-134">Další informace najdete v tématu [obnovení balíčků NuGet "](http://docs.nuget.org/consume/package-restore).</span><span class="sxs-lookup"><span data-stu-id="e4ff1-134">For more information, see [NuGet Package Restore"](http://docs.nuget.org/consume/package-restore).</span></span>
7. <span data-ttu-id="e4ff1-135">Přidat odkaz na **System.Configuration** sestavení.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-135">Add a reference to **System.Configuration** assembly.</span></span> <span data-ttu-id="e4ff1-136">Toto sestavení obsahuje System.Configuration. **ConfigurationManager** třídu, která se používá pro přístup k souborům konfigurace (například App.config).</span><span class="sxs-lookup"><span data-stu-id="e4ff1-136">This assembly contains the System.Configuration.**ConfigurationManager** class that is used to access configuration files (for example, App.config).</span></span>
   
    <span data-ttu-id="e4ff1-137">Přidání odkazů pomocí dialogu spravovat odkazy, klikněte pravým tlačítkem na název projektu v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-137">To add references using the Manage References dialog, right-click the project name in the Solution Explorer.</span></span> <span data-ttu-id="e4ff1-138">Pak vyberte Přidat a odkazy.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-138">Then, select Add and References.</span></span>
   
    <span data-ttu-id="e4ff1-139">Otevře se dialogové okno Spravovat odkazy.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-139">The Manage References dialog appears.</span></span>
8. <span data-ttu-id="e4ff1-140">V části sestavení rozhraní .NET framework najít a vybrat sestavení System.Configuration a klikněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-140">Under .NET framework assemblies, find and select the System.Configuration assembly and press OK.</span></span>
9. <span data-ttu-id="e4ff1-141">Otevřete soubor App.config a přidejte *appSettings* část do souboru.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-141">Open the App.config file and add an *appSettings* section to the file.</span></span>     
   
    <span data-ttu-id="e4ff1-142">Nastavte hodnoty, které jsou potřebné pro připojení k rozhraní API služby Media Services.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-142">Set the values that are needed to connect to the Media Services API.</span></span> <span data-ttu-id="e4ff1-143">Další informace najdete v tématu [přístup k Azure Media Services API pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="e4ff1-143">For more information, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

    <span data-ttu-id="e4ff1-144">Pokud používáte [ověření uživatele](media-services-use-aad-auth-to-access-ams-api.md#types-of-authentication) konfiguračního souboru bude pravděpodobně mít hodnoty pro doménu klienta Azure AD a koncového bodu rozhraní REST API pro AMS.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-144">If you are using [user authentication](media-services-use-aad-auth-to-access-ams-api.md#types-of-authentication) your config file will probably have values for your Azure AD tenant domain and the AMS REST API endpoint.</span></span>
    
    >[!Important]
    ><span data-ttu-id="e4ff1-145">Většina ukázek kódu v dokumentaci k Azure Media Services nastavit, použijte uživatele (interaktivní) typ ověřování pro připojení k rozhraní API pro AMS.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-145">Most code samples in the Azure Media Services documentation set, use a user (interactive) type of authentication to connect to the AMS API.</span></span> <span data-ttu-id="e4ff1-146">Tato metoda ověřování bude fungovat i pro správu nebo monitorování nativní aplikace: mobilní aplikace, aplikace pro Windows a konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-146">This authentication method will work well for management or monitoring native apps: mobile apps, Windows apps, and Console applications.</span></span> <span data-ttu-id="e4ff1-147">Tato metoda ověřování není vhodný pro server, webové služby, rozhraní API typu aplikací.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-147">This authentication method is not suitable for server, web services, APIs type of applications.</span></span>  <span data-ttu-id="e4ff1-148">Další informace najdete v tématu [přístup k rozhraní API AMS pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="e4ff1-148">For more information, see [Access the AMS API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span>

        <configuration>
        ...
            <appSettings>
              <add key="AADTenantDomain" value="YourAADTenantDomain" />
              <add key="MediaServiceRESTAPIEndpoint" value="YourRESTAPIEndpoint" />
            </appSettings>

        </configuration>

10. <span data-ttu-id="e4ff1-149">Následujícím kódem přepište existující příkazy **using** na začátku souboru Program.cs.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-149">Overwrite the existing **using** statements at the beginning of the Program.cs file with the following code.</span></span>
           
        using System;
        using System.Configuration;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Collections.Generic;
        using System.Linq;

<span data-ttu-id="e4ff1-150">V tomto okamžiku jste připravení začít s vývojem aplikací Media Services.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-150">At this point, you are ready to start developing a Media Services application.</span></span>    

## <a name="example"></a><span data-ttu-id="e4ff1-151">Příklad</span><span class="sxs-lookup"><span data-stu-id="e4ff1-151">Example</span></span>

<span data-ttu-id="e4ff1-152">Zde je malý příklad, který se připojuje k rozhraní API pro AMS a uvádí všechny dostupné procesory médií.</span><span class="sxs-lookup"><span data-stu-id="e4ff1-152">Here is a small example that connects to the AMS API and lists all available Media Processors.</span></span>
    
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

##<a name="next-steps"></a><span data-ttu-id="e4ff1-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e4ff1-153">Next steps</span></span>

<span data-ttu-id="e4ff1-154">Nyní [se můžete připojit k rozhraní API pro AMS](media-services-use-aad-auth-to-access-ams-api.md) a spusťte [vývoj](media-services-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e4ff1-154">Now [you can connect to the AMS API](media-services-use-aad-auth-to-access-ams-api.md) and start [developing](media-services-dotnet-get-started.md).</span></span>


## <a name="media-services-learning-paths"></a><span data-ttu-id="e4ff1-155">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="e4ff1-155">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="e4ff1-156">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="e4ff1-156">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

