---
title: "Integraci sady Engagement SDK univerzálních aplikací pro Windows"
description: "Postup pro integraci Azure Mobile Engagement univerzálních aplikací pro Windows"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 71236b68-5ebd-44aa-8c82-c7ca8098ea05
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 898160814304fa8ec65622056a77ca9d4caf2c99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="windows-universal-apps-engagement-sdk-integration"></a><span data-ttu-id="89de7-103">Integraci sady Engagement SDK univerzálních aplikací pro Windows</span><span class="sxs-lookup"><span data-stu-id="89de7-103">Windows Universal Apps Engagement SDK Integration</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="89de7-104">Univerzální platforma Windows</span><span class="sxs-lookup"><span data-stu-id="89de7-104">Universal Windows</span></span>](mobile-engagement-windows-store-integrate-engagement.md) 
> * [<span data-ttu-id="89de7-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="89de7-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md) 
> * [<span data-ttu-id="89de7-106">iOS</span><span class="sxs-lookup"><span data-stu-id="89de7-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md) 
> * [<span data-ttu-id="89de7-107">Android</span><span class="sxs-lookup"><span data-stu-id="89de7-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md) 
> 
> 

<span data-ttu-id="89de7-108">Tento postup popisuje nejjednodušší způsob, jak aktivovat Engagement analýzy a monitorování funkce v aplikaci univerzální pro Windows.</span><span class="sxs-lookup"><span data-stu-id="89de7-108">This procedure describes the simplest way to activate Engagement's Analytics and Monitoring functions in your Windows Universal application.</span></span>

<span data-ttu-id="89de7-109">Následující kroky jsou dost aktivovat sestavy protokolů, které jsou potřebné k výpočtu všechny statistické údaje týkající se uživatelů, relací, aktivity, dojde k chybě a Technicals.</span><span class="sxs-lookup"><span data-stu-id="89de7-109">The following steps are enough to activate the report of logs needed to compute all statistics regarding Users, Sessions, Activities, Crashes and Technicals.</span></span> <span data-ttu-id="89de7-110">Sestava protokolů, které jsou potřebné k výpočtu další statistiky, jako jsou události a chyby úlohy je třeba provést ručně pomocí rozhraní API Engagement (najdete v části [jak používat rozšířené Mobile Engagement označování rozhraní API v aplikaci Windows Universal](mobile-engagement-windows-store-use-engagement-api.md) vzhledem k tomu, že tyto statistické údaje jsou závislé aplikace.</span><span class="sxs-lookup"><span data-stu-id="89de7-110">The report of logs needed to compute other statistics like Events, Errors and Jobs must be done manually using the Engagement API (see [How to use the advanced Mobile Engagement tagging API in your Windows Universal app](mobile-engagement-windows-store-use-engagement-api.md) since these statistics are application dependent.</span></span>

## <a name="supported-versions"></a><span data-ttu-id="89de7-111">Podporované verze</span><span class="sxs-lookup"><span data-stu-id="89de7-111">Supported versions</span></span>
<span data-ttu-id="89de7-112">Mobile Engagement SDK pro Windows Universal aplikace lze pouze integrovat do prostředí Windows Runtime a aplikací pro univerzální platformu Windows:</span><span class="sxs-lookup"><span data-stu-id="89de7-112">The Mobile Engagement SDK for Windows Universal Apps can only be integrated into Windows Runtime and Universal Windows Platform applications targeting :</span></span>

* <span data-ttu-id="89de7-113">Windows 8</span><span class="sxs-lookup"><span data-stu-id="89de7-113">Windows 8</span></span>
* <span data-ttu-id="89de7-114">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="89de7-114">Windows 8.1</span></span>
* <span data-ttu-id="89de7-115">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="89de7-115">Windows Phone 8.1</span></span>
* <span data-ttu-id="89de7-116">Windows 10 (desktop a mobile rodiny)</span><span class="sxs-lookup"><span data-stu-id="89de7-116">Windows 10 (desktop and mobile families)</span></span>

> [!NOTE]
> <span data-ttu-id="89de7-117">Pokud jsou cílem Windows Phone Silverlight pak odkazovat [postup integrace Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="89de7-117">If you are targeting Windows Phone Silverlight then refer to the [Windows Phone Silverlight integration procedure](mobile-engagement-windows-phone-integrate-engagement.md).</span></span>
> 
> 

## <a name="install-the-mobile-engagement-universal-apps-sdk"></a><span data-ttu-id="89de7-118">Nainstalovat sadu SDK Mobile Engagement univerzální aplikace</span><span class="sxs-lookup"><span data-stu-id="89de7-118">Install the Mobile Engagement Universal Apps SDK</span></span>
### <a name="all-platforms"></a><span data-ttu-id="89de7-119">Všechny platformy</span><span class="sxs-lookup"><span data-stu-id="89de7-119">All platforms</span></span>
<span data-ttu-id="89de7-120">Mobile Engagement SDK pro univerzální aplikace Windows je k dispozici jako balíček Nuget s názvem *MicrosoftAzure.MobileEngagement*.</span><span class="sxs-lookup"><span data-stu-id="89de7-120">The Mobile Engagement SDK for Windows Universal App is available as a Nuget package called *MicrosoftAzure.MobileEngagement*.</span></span> <span data-ttu-id="89de7-121">Můžete ho nainstalovat z Visual Studio Správce balíčků Nuget.</span><span class="sxs-lookup"><span data-stu-id="89de7-121">You can install it from the Visual Studio Nuget Package Manager.</span></span>

### <a name="windows-8x-and-windows-phone-81"></a><span data-ttu-id="89de7-122">Windows 8.x a Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="89de7-122">Windows 8.x and Windows Phone 8.1</span></span>
<span data-ttu-id="89de7-123">NuGet automaticky nasadí prostředků sady SDK v `Resources` složku v kořenovém adresáři projektu aplikace.</span><span class="sxs-lookup"><span data-stu-id="89de7-123">NuGet automatically deploys the SDK resources in the `Resources` folder at the root of your application project.</span></span>

### <a name="windows-10-universal-windows-platform-applications"></a><span data-ttu-id="89de7-124">Aplikace Windows 10 univerzální platformu Windows</span><span class="sxs-lookup"><span data-stu-id="89de7-124">Windows 10 Universal Windows Platform applications</span></span>
<span data-ttu-id="89de7-125">NuGet automaticky Nenasazuje prostředků sady SDK v aplikaci UWP ještě.</span><span class="sxs-lookup"><span data-stu-id="89de7-125">NuGet does not automatically deploy the SDK resources in your UWP application yet.</span></span> <span data-ttu-id="89de7-126">Je nutné provést ručně dokud prostředky nasazení je znovu zavedena v NuGet:</span><span class="sxs-lookup"><span data-stu-id="89de7-126">You have to do it manually until resources deployment is reintroduced in NuGet:</span></span>

1. <span data-ttu-id="89de7-127">Otevřete váš Průzkumníka souborů.</span><span class="sxs-lookup"><span data-stu-id="89de7-127">Open your File Explorer.</span></span>
2. <span data-ttu-id="89de7-128">Přejděte do následujícího umístění (**x.x.x** je verze Engagement instalujete): *% USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\\**x.x.x**\\content\win81*</span><span class="sxs-lookup"><span data-stu-id="89de7-128">Navigate to the following location (**x.x.x** is the version of Engagement you are installing): *%USERPROFILE%\\.nuget\packages\MicrosoftAzure.MobileEngagement\\**x.x.x**\\content\win81*</span></span>
3. <span data-ttu-id="89de7-129">Přetáhnout myší **prostředky** složky v Průzkumníku souborů do kořenového adresáře projektu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="89de7-129">Drag and drop the **Resources** folder from the file explorer to the root of your project in Visual Studio.</span></span>
4. <span data-ttu-id="89de7-130">V sadě Visual Studio vyberte projekt a aktivovat **zobrazit všechny soubory** ikonu na **Průzkumníku řešení**.</span><span class="sxs-lookup"><span data-stu-id="89de7-130">In Visual Studio select your project and activate the **Show All files** icon on top of the **Solution Explorer**.</span></span>
5. <span data-ttu-id="89de7-131">Některé soubory nejsou zahrnuty v projektu.</span><span class="sxs-lookup"><span data-stu-id="89de7-131">Some files are not included in the project.</span></span> <span data-ttu-id="89de7-132">K importu je najednou klikněte pravým tlačítkem na **prostředky** složky, **vyloučit z projektu** pak jiné klikněte pravým tlačítkem na **prostředky** složky, **zahrnout do projektu** znovu zahrnout celou složku.</span><span class="sxs-lookup"><span data-stu-id="89de7-132">To import them at once right click on the **Resources** folder, **Exclude from project** then another right click on the **Resources** folder, **Include in project** to re-include the whole folder.</span></span> <span data-ttu-id="89de7-133">Všechny soubory z **prostředky** složky jsou teď součástí projektu.</span><span class="sxs-lookup"><span data-stu-id="89de7-133">All files from the **Resources** folder are now included in your project.</span></span>

## <a name="add-the-capabilities"></a><span data-ttu-id="89de7-134">Přidání funkcí</span><span class="sxs-lookup"><span data-stu-id="89de7-134">Add the capabilities</span></span>
<span data-ttu-id="89de7-135">Sady Engagement SDK potřebuje některé funkce sady Windows SDK správné fungování.</span><span class="sxs-lookup"><span data-stu-id="89de7-135">The Engagement SDK needs some capabilities of the Windows SDK in order to work properly.</span></span>

<span data-ttu-id="89de7-136">Otevřete váš `Package.appxmanifest` souboru a ujistěte se, že jsou deklarovány následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="89de7-136">Open your `Package.appxmanifest` file and be sure that the following capabilities are declared:</span></span>

* `Internet (Client)`

## <a name="initialize-the-engagement-sdk"></a><span data-ttu-id="89de7-137">Inicializace Engagement SDK</span><span class="sxs-lookup"><span data-stu-id="89de7-137">Initialize the Engagement SDK</span></span>
### <a name="engagement-configuration"></a><span data-ttu-id="89de7-138">Konfigurace zapojení</span><span class="sxs-lookup"><span data-stu-id="89de7-138">Engagement configuration</span></span>
<span data-ttu-id="89de7-139">Konfigurace zapojení je centralizovaná v `Resources\EngagementConfiguration.xml` souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="89de7-139">The Engagement configuration is centralized in the `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="89de7-140">Upravte tento soubor k určení:</span><span class="sxs-lookup"><span data-stu-id="89de7-140">Edit this file to specify:</span></span>

* <span data-ttu-id="89de7-141">Připojovací řetězec aplikací mezi značky `<connectionString>` a `<\connectionString>`.</span><span class="sxs-lookup"><span data-stu-id="89de7-141">Your application connection string between tags `<connectionString>` and `<\connectionString>`.</span></span>

<span data-ttu-id="89de7-142">Pokud chcete zadat za běhu, můžete volat metodu před Engagement inicializaci agenta:</span><span class="sxs-lookup"><span data-stu-id="89de7-142">If you want to specify it at runtime instead, you can call the following method before the Engagement agent initialization:</span></span>

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

<span data-ttu-id="89de7-143">Připojovací řetězec pro vaši aplikaci se zobrazí na portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="89de7-143">The connection string for your application is displayed on the Azure Classic Portal.</span></span>

### <a name="engagement-initialization"></a><span data-ttu-id="89de7-144">Inicializace zapojení</span><span class="sxs-lookup"><span data-stu-id="89de7-144">Engagement initialization</span></span>
<span data-ttu-id="89de7-145">Když vytvoříte nový projekt, `App.xaml.cs` se vygeneruje soubor.</span><span class="sxs-lookup"><span data-stu-id="89de7-145">When you create a new project, a `App.xaml.cs` file is generated.</span></span> <span data-ttu-id="89de7-146">Tato třída dědí z `Application` a obsahuje mnoho důležité metody.</span><span class="sxs-lookup"><span data-stu-id="89de7-146">This class inherits from `Application` and contains many important methods.</span></span> <span data-ttu-id="89de7-147">Je také použije k chybě při inicializaci sady Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="89de7-147">It will also be used to initialize the Engagement SDK.</span></span>

<span data-ttu-id="89de7-148">Změnit `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="89de7-148">Modify the `App.xaml.cs`:</span></span>

* <span data-ttu-id="89de7-149">Přidat do vaší `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="89de7-149">Add to your `using` statements:</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="89de7-150">Zadejte způsob, jak sdílet inicializace Engagement jednou pro všechna volání:</span><span class="sxs-lookup"><span data-stu-id="89de7-150">Define a method to share the Engagement initialization once for all calls:</span></span>
  
      private void InitEngagement(IActivatedEventArgs e)
      {
        EngagementAgent.Instance.Init(e);
  
        // or
  
        EngagementAgent.Instance.Init(e, engagementConfiguration);
      }
* <span data-ttu-id="89de7-151">Volání `InitEngagement` v `OnLaunched` metoda:</span><span class="sxs-lookup"><span data-stu-id="89de7-151">Call `InitEngagement` in the `OnLaunched` method:</span></span>
  
      protected override void OnLaunched(LaunchActivatedEventArgs e)
      {
        InitEngagement(e);
      }
* <span data-ttu-id="89de7-152">Když je aplikace spuštěna pomocí vlastní schéma, jiná aplikace nebo příkazového řádku potom `OnActivated` metoda je volána.</span><span class="sxs-lookup"><span data-stu-id="89de7-152">When your application is launched using a custom scheme, another application or the command line then the `OnActivated` method is called.</span></span> <span data-ttu-id="89de7-153">Musíte také zahájit sady Engagement SDK, když je aktivován vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="89de7-153">You also need to initiate the Engagement SDK when your app is activated.</span></span> <span data-ttu-id="89de7-154">Chcete-li to provést, přepište `OnActivated` metoda:</span><span class="sxs-lookup"><span data-stu-id="89de7-154">To do so, override `OnActivated` method:</span></span>
  
      protected override void OnActivated(IActivatedEventArgs args)
      {
        InitEngagement(args);
      }

> [!IMPORTANT]
> <span data-ttu-id="89de7-155">Jsme důrazně bránit můžete přidat inicializace Engagement na jiném místě vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="89de7-155">We strongly discourage you to add the Engagement initialization in another place of your application.</span></span>
> 
> 

## <a name="basic-reporting"></a><span data-ttu-id="89de7-156">Vytváření základních sestav</span><span class="sxs-lookup"><span data-stu-id="89de7-156">Basic reporting</span></span>
### <a name="recommended-method-overload-your-page-classes"></a><span data-ttu-id="89de7-157">Doporučená metoda: přetížení vaší `Page` třídy</span><span class="sxs-lookup"><span data-stu-id="89de7-157">Recommended method: overload your `Page` classes</span></span>
<span data-ttu-id="89de7-158">Chcete-li aktivovat sestavu všechny protokoly, které vyžadují zapojení k výpočtu uživatelů, relací, aktivity, dojde k chybě a technické statistiky, můžete jednoduše provést všechny vaše `Page` dílčí třídy dědí `EngagementPage` třídy.</span><span class="sxs-lookup"><span data-stu-id="89de7-158">In order to activate the report of all the logs required by Engagement to compute Users, Sessions, Activities, Crashes and Technical statistics, you can simply make all your `Page` sub-classes inherit from the `EngagementPage` classes.</span></span>

<span data-ttu-id="89de7-159">Tady je příklad toho, jak to udělat pro stránku vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="89de7-159">Here is an example of how to do this for a page of your application.</span></span> <span data-ttu-id="89de7-160">Můžete to samé pro všechny stránky vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="89de7-160">You can do the same thing for all pages of your application.</span></span>

#### <a name="c-source-file"></a><span data-ttu-id="89de7-161">C# zdrojový soubor</span><span class="sxs-lookup"><span data-stu-id="89de7-161">C# Source file</span></span>
<span data-ttu-id="89de7-162">Upravit stránku `.xaml.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="89de7-162">Modify your page `.xaml.cs` file:</span></span>

* <span data-ttu-id="89de7-163">Přidat do vaší `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="89de7-163">Add to your `using` statements:</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="89de7-164">Nahraďte `Page` s `EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="89de7-164">Replace `Page` with `EngagementPage`:</span></span>

<span data-ttu-id="89de7-165">**Bez Engagement:**</span><span class="sxs-lookup"><span data-stu-id="89de7-165">**Without Engagement:**</span></span>

        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

<span data-ttu-id="89de7-166">**S Engagement:**</span><span class="sxs-lookup"><span data-stu-id="89de7-166">**With Engagement:**</span></span>

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [!IMPORTANT]
> <span data-ttu-id="89de7-167">Pokud stránka přepíše metodu `OnNavigatedTo`, nezapomeňte volat `base.OnNavigatedTo(e)`.</span><span class="sxs-lookup"><span data-stu-id="89de7-167">If your page overrides the `OnNavigatedTo` method, be sure to call `base.OnNavigatedTo(e)`.</span></span> <span data-ttu-id="89de7-168">Jinak aktivita nebude hlášena (`EngagementPage` volá `StartActivity` uvnitř své metody `OnNavigatedTo`).</span><span class="sxs-lookup"><span data-stu-id="89de7-168">Otherwise,  the activity will not be reported (the `EngagementPage` calls `StartActivity` inside its `OnNavigatedTo` method).</span></span>
> 
> 

#### <a name="xaml-file"></a><span data-ttu-id="89de7-169">Souboru XAML</span><span class="sxs-lookup"><span data-stu-id="89de7-169">XAML file</span></span>
<span data-ttu-id="89de7-170">Upravit stránku `.xaml` souboru:</span><span class="sxs-lookup"><span data-stu-id="89de7-170">Modify your page `.xaml` file:</span></span>

* <span data-ttu-id="89de7-171">Přidejte do deklarací oborů názvů:</span><span class="sxs-lookup"><span data-stu-id="89de7-171">Add to your namespaces declarations:</span></span>
  
      xmlns:engagement="using:Microsoft.Azure.Engagement"
* <span data-ttu-id="89de7-172">Nahraďte `Page` s `engagement:EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="89de7-172">Replace `Page` with `engagement:EngagementPage`:</span></span>

<span data-ttu-id="89de7-173">**Bez Engagement:**</span><span class="sxs-lookup"><span data-stu-id="89de7-173">**Without Engagement:**</span></span>

        <Page>
            <!-- layout -->
            ...
        </Page>

<span data-ttu-id="89de7-174">**S Engagement:**</span><span class="sxs-lookup"><span data-stu-id="89de7-174">**With Engagement:**</span></span>

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

#### <a name="override-the-default-behaviour"></a><span data-ttu-id="89de7-175">Přepsat výchozí chování</span><span class="sxs-lookup"><span data-stu-id="89de7-175">Override the default behaviour</span></span>
<span data-ttu-id="89de7-176">Ve výchozím nastavení je název třídy stránky uvedená jako název aktivity, s žádné další.</span><span class="sxs-lookup"><span data-stu-id="89de7-176">By default, the class name of the page is reported as the activity name, with no extra.</span></span> <span data-ttu-id="89de7-177">Pokud třída používá příponou "Stránka", Engagement také odebere.</span><span class="sxs-lookup"><span data-stu-id="89de7-177">If the class uses the "Page" suffix, Engagement will also remove it.</span></span>

<span data-ttu-id="89de7-178">Pokud chcete přepsat výchozí chování pro název, jednoduše přidejte tuto kódu:</span><span class="sxs-lookup"><span data-stu-id="89de7-178">If you want to override the default behaviour for the name, simply add this to your code:</span></span>

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

<span data-ttu-id="89de7-179">Pokud chcete ohlásit některé další – informace s vaší aktivitou, můžete to přidat do kódu:</span><span class="sxs-lookup"><span data-stu-id="89de7-179">If you want to report some extra informations with your activity, you can add this to your code:</span></span>

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

<span data-ttu-id="89de7-180">Tyto metody jsou volány prostřednictvím `OnNavigatedTo` metoda stránky.</span><span class="sxs-lookup"><span data-stu-id="89de7-180">These methods are called from within the `OnNavigatedTo` method of your page.</span></span>

### <a name="alternate-method-call-startactivity-manually"></a><span data-ttu-id="89de7-181">Alternativní metoda: volání `StartActivity()` ručně</span><span class="sxs-lookup"><span data-stu-id="89de7-181">Alternate method: call `StartActivity()` manually</span></span>
<span data-ttu-id="89de7-182">Pokud nemůžete nebo nechcete přetížení vaše `Page` třídy, můžete místo toho spustit vaše aktivity voláním `EngagementAgent` metody přímo.</span><span class="sxs-lookup"><span data-stu-id="89de7-182">If you cannot or do not want to overload your `Page` classes, you can instead start your activities by calling `EngagementAgent` methods directly.</span></span>

<span data-ttu-id="89de7-183">Doporučujeme, abyste k volání `StartActivity` uvnitř vaší `OnNavigatedTo` metoda stránky.</span><span class="sxs-lookup"><span data-stu-id="89de7-183">We recommend to call `StartActivity` inside your `OnNavigatedTo` method of your Page.</span></span>

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [!IMPORTANT]
> <span data-ttu-id="89de7-184">Zajistěte, aby že správně ukončete svou relaci.</span><span class="sxs-lookup"><span data-stu-id="89de7-184">Ensure you end your session correctly.</span></span>
> 
> <span data-ttu-id="89de7-185">Sady Windows Universal SDK automaticky zavolá `EndActivity` metoda při ukončení aplikace.</span><span class="sxs-lookup"><span data-stu-id="89de7-185">The Windows Universal SDK automatically calls the `EndActivity` method when the application is closed.</span></span> <span data-ttu-id="89de7-186">Z toho důvodu je **vysoce** doporučuje volat `StartActivity` metoda vždy, když aktivita uživatele změnit a **nikdy** volání `EndActivity` metodu, tato metoda odešle na server zapojení, že aktuální uživatel má nechat aplikaci, budou ovlivňuje všechny protokoly aplikací.</span><span class="sxs-lookup"><span data-stu-id="89de7-186">Thus, it is **HIGHLY** recommended to call the `StartActivity` method whenever the activity of the user change, and to **NEVER** call the `EndActivity` method, this method sends to Engagement server that current user has leave the application, this will impacts all application logs.</span></span>
> 
> 

## <a name="advanced-reporting"></a><span data-ttu-id="89de7-187">Rozšířená tvorba sestav</span><span class="sxs-lookup"><span data-stu-id="89de7-187">Advanced reporting</span></span>
<span data-ttu-id="89de7-188">Volitelně můžete chtít sestav aplikace konkrétní události, chyb a úlohy, Uděláte to tak, použijte jiné metody v nalezen `EngagementAgent` třídy.</span><span class="sxs-lookup"><span data-stu-id="89de7-188">Optionally, you may want to report application specific events, errors and jobs, to do so, use the others methods found in the `EngagementAgent` class.</span></span> <span data-ttu-id="89de7-189">Rozhraní API Engagement umožňuje používat všechny rozšířené možnosti Engagement.</span><span class="sxs-lookup"><span data-stu-id="89de7-189">The Engagement API allows to use all of Engagement's advanced capabilities.</span></span>

<span data-ttu-id="89de7-190">Další informace najdete v tématu [jak používat rozšířené Mobile Engagement označování rozhraní API v aplikaci Windows Universal](mobile-engagement-windows-store-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="89de7-190">For further information, see [How to use the advanced Mobile Engagement tagging API in your Windows Universal app](mobile-engagement-windows-store-use-engagement-api.md).</span></span>

## <a name="advanced-configuration"></a><span data-ttu-id="89de7-191">Pokročilá konfigurace</span><span class="sxs-lookup"><span data-stu-id="89de7-191">Advanced configuration</span></span>
### <a name="disable-automatic-crash-reporting"></a><span data-ttu-id="89de7-192">Zakázat automatické hlášení chyb</span><span class="sxs-lookup"><span data-stu-id="89de7-192">Disable automatic crash reporting</span></span>
<span data-ttu-id="89de7-193">Můžete vypnout automatické hlášení funkci Engagement generování sestav.</span><span class="sxs-lookup"><span data-stu-id="89de7-193">You can disable the automatic crash reporting feature of Engagement.</span></span> <span data-ttu-id="89de7-194">Pak když dojde k neošetřené výjimce, Engagement nebude provádět žádné kroky.</span><span class="sxs-lookup"><span data-stu-id="89de7-194">Then, when an unhandled exception will occur, Engagement won't do anything.</span></span>

> [!WARNING]
> <span data-ttu-id="89de7-195">Pokud budete chtít tuto funkci zakázat, mějte na paměti, když dojde k nezpracované chybě ve vaší aplikaci, nepošle Engagement havárii **a** nebude zavřete relaci a úlohy.</span><span class="sxs-lookup"><span data-stu-id="89de7-195">If you plan to disable this feature, be aware that when a unhandled crash will occur in your app, Engagement will not send the crash **AND** will not close the session and jobs.</span></span>
> 
> 

<span data-ttu-id="89de7-196">Chcete-li zakázat automatické hlášení chyb, upravte konfiguraci. v závislosti na způsobu, jakým jste ji deklarovaný:</span><span class="sxs-lookup"><span data-stu-id="89de7-196">To disable automatic crash reporting, just customize your configuration depending on the way you declared it:</span></span>

#### <a name="from-engagementconfigurationxml-file"></a><span data-ttu-id="89de7-197">Z `EngagementConfiguration.xml` souboru</span><span class="sxs-lookup"><span data-stu-id="89de7-197">From `EngagementConfiguration.xml` file</span></span>
<span data-ttu-id="89de7-198">Nastavte Sestavy havárií na `false` mezi `<reportCrash>` a `</reportCrash>` značky.</span><span class="sxs-lookup"><span data-stu-id="89de7-198">Set report crash to `false` between `<reportCrash>` and `</reportCrash>` tags.</span></span>

#### <a name="from-engagementconfiguration-object-at-run-time"></a><span data-ttu-id="89de7-199">Z `EngagementConfiguration` objektu v době běhu</span><span class="sxs-lookup"><span data-stu-id="89de7-199">From `EngagementConfiguration` object at run time</span></span>
<span data-ttu-id="89de7-200">Nastavit na hodnotu false pomocí objektu EngagementConfiguration sestavy havárií.</span><span class="sxs-lookup"><span data-stu-id="89de7-200">Set report crash to false using your EngagementConfiguration object.</span></span>

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a><span data-ttu-id="89de7-201">Režim shluku</span><span class="sxs-lookup"><span data-stu-id="89de7-201">Burst mode</span></span>
<span data-ttu-id="89de7-202">Ve výchozím nastavení sestavy služby zapojení protokolů v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="89de7-202">By default, the Engagement service reports logs in real time.</span></span> <span data-ttu-id="89de7-203">Pokud vaše aplikace hlásí protokoly velmi často, je lepší ukládat do vyrovnávací paměti protokoly a sestavy je všechny najednou pravidelné základní časové (to se nazývá "shluků režim").</span><span class="sxs-lookup"><span data-stu-id="89de7-203">If your application reports logs very frequently, it is better to buffer the logs and to report them all at once on a regular time base (this is called the “burst mode”).</span></span>

<span data-ttu-id="89de7-204">Chcete-li tak učinit, zavolejte metodu:</span><span class="sxs-lookup"><span data-stu-id="89de7-204">To do so, call the method:</span></span>

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

<span data-ttu-id="89de7-205">Argument je hodnota **milisekundách**.</span><span class="sxs-lookup"><span data-stu-id="89de7-205">The argument is a value in **milliseconds**.</span></span> <span data-ttu-id="89de7-206">Kdykoli Pokud chcete znovu aktivovat protokolování v reálném čase, stačí zavoláte metoda bez zadání parametru nebo s hodnotou 0.</span><span class="sxs-lookup"><span data-stu-id="89de7-206">At any time, if you want to reactivate the real-time logging, just call the method without any parameter, or with the 0 value.</span></span>

<span data-ttu-id="89de7-207">Režim shluků mírně zvýšit výdrž baterie, ale má vliv na zapojení monitorování: všechny dobu trvání relace a úlohy zaokrouhlen shluků prahovou hodnotu (tedy relací a úlohy kratší, než je prahová hodnota shluků nemusí být zobrazeny).</span><span class="sxs-lookup"><span data-stu-id="89de7-207">The burst mode slightly increase the battery life but has an impact on the Engagement Monitor: all sessions and jobs duration will be rounded to the burst threshold (thus, sessions and jobs shorter than the burst threshold may not be visible).</span></span> <span data-ttu-id="89de7-208">Doporučujeme používat práh shluků než 30000 (30s).</span><span class="sxs-lookup"><span data-stu-id="89de7-208">It is recommended to use a burst threshold no longer than 30000 (30s).</span></span> <span data-ttu-id="89de7-209">Budete muset mějte na paměti, uložené protokoly jsou omezeny na 300 položek.</span><span class="sxs-lookup"><span data-stu-id="89de7-209">You have to be aware that saved logs are limited to 300 items.</span></span> <span data-ttu-id="89de7-210">Pokud odesílání je příliš dlouhý. může dojít ke ztrátě některých protokoly.</span><span class="sxs-lookup"><span data-stu-id="89de7-210">If sending is too long you can lose some logs.</span></span>

> [!WARNING]
> <span data-ttu-id="89de7-211">Prahová hodnota shluků nelze nakonfigurovat na dobu, která je menší než hodnotami 1.</span><span class="sxs-lookup"><span data-stu-id="89de7-211">The burst threshold cannot be configured to a period lesser than 1s.</span></span> <span data-ttu-id="89de7-212">Pokud se pokusíte učinit, sady SDK se zobrazí trasování došlo k chybě a automaticky obnoví na výchozí hodnotu, tedy 0s.</span><span class="sxs-lookup"><span data-stu-id="89de7-212">If you try to do so, the SDK will show a trace with the error and will automatically reset to the default value, i.e., 0s.</span></span> <span data-ttu-id="89de7-213">Tím se aktivuje sady SDK k hlášení protokolů v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="89de7-213">This will trigger the SDK to report the logs in real-time.</span></span>
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview

