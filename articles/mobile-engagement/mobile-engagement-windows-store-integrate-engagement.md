---
title: "aaaWindows Universal integraci sady SDK zapojení aplikace"
description: "Jak tooIntegrate Azure Mobile Engagement s univerzálních aplikací pro Windows"
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
ms.openlocfilehash: 18543798099c233dbe55cc387ba0216e369c8596
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="windows-universal-apps-engagement-sdk-integration"></a><span data-ttu-id="438b9-103">Integraci sady Engagement SDK univerzálních aplikací pro Windows</span><span class="sxs-lookup"><span data-stu-id="438b9-103">Windows Universal Apps Engagement SDK Integration</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="438b9-104">Univerzální platforma Windows</span><span class="sxs-lookup"><span data-stu-id="438b9-104">Universal Windows</span></span>](mobile-engagement-windows-store-integrate-engagement.md) 
> * [<span data-ttu-id="438b9-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="438b9-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md) 
> * [<span data-ttu-id="438b9-106">iOS</span><span class="sxs-lookup"><span data-stu-id="438b9-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md) 
> * [<span data-ttu-id="438b9-107">Android</span><span class="sxs-lookup"><span data-stu-id="438b9-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md) 
> 
> 

<span data-ttu-id="438b9-108">Tento postup popisuje analýzy a monitorování funkce v aplikaci univerzální pro Windows hello nejjednodušší způsob, jak tooactivate Engagement.</span><span class="sxs-lookup"><span data-stu-id="438b9-108">This procedure describes hello simplest way tooactivate Engagement's Analytics and Monitoring functions in your Windows Universal application.</span></span>

<span data-ttu-id="438b9-109">Hello následující kroky jsou že dostatek tooactivate hello protokolů je nutná sestava toocompute všechny statistické údaje týkající se uživatelů, relací, aktivity, dojde k chybě a Technicals.</span><span class="sxs-lookup"><span data-stu-id="438b9-109">hello following steps are enough tooactivate hello report of logs needed toocompute all statistics regarding Users, Sessions, Activities, Crashes and Technicals.</span></span> <span data-ttu-id="438b9-110">Hello protokolů je nutná sestava toocompute další statistiky jako události a chyby úlohy je třeba provést ručně pomocí hello Engagement rozhraní API (v tématu [jak toouse hello advanced označování rozhraní API v aplikaci Windows Universal Mobile Engagement](mobile-engagement-windows-store-use-engagement-api.md) od Tyto statistické údaje jsou závislé aplikace.</span><span class="sxs-lookup"><span data-stu-id="438b9-110">hello report of logs needed toocompute other statistics like Events, Errors and Jobs must be done manually using hello Engagement API (see [How toouse hello advanced Mobile Engagement tagging API in your Windows Universal app](mobile-engagement-windows-store-use-engagement-api.md) since these statistics are application dependent.</span></span>

## <a name="supported-versions"></a><span data-ttu-id="438b9-111">Podporované verze</span><span class="sxs-lookup"><span data-stu-id="438b9-111">Supported versions</span></span>
<span data-ttu-id="438b9-112">Mobile Engagement SDK pro univerzální aplikace pro Windows Hello lze pouze integrovat do prostředí Windows Runtime a aplikací pro univerzální platformu Windows:</span><span class="sxs-lookup"><span data-stu-id="438b9-112">hello Mobile Engagement SDK for Windows Universal Apps can only be integrated into Windows Runtime and Universal Windows Platform applications targeting :</span></span>

* <span data-ttu-id="438b9-113">Windows 8</span><span class="sxs-lookup"><span data-stu-id="438b9-113">Windows 8</span></span>
* <span data-ttu-id="438b9-114">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="438b9-114">Windows 8.1</span></span>
* <span data-ttu-id="438b9-115">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="438b9-115">Windows Phone 8.1</span></span>
* <span data-ttu-id="438b9-116">Windows 10 (desktop a mobile rodiny)</span><span class="sxs-lookup"><span data-stu-id="438b9-116">Windows 10 (desktop and mobile families)</span></span>

> [!NOTE]
> <span data-ttu-id="438b9-117">Pokud jsou cílem Windows Phone Silverlight pak odkazovat toohello [postup integrace Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="438b9-117">If you are targeting Windows Phone Silverlight then refer toohello [Windows Phone Silverlight integration procedure](mobile-engagement-windows-phone-integrate-engagement.md).</span></span>
> 
> 

## <a name="install-hello-mobile-engagement-universal-apps-sdk"></a><span data-ttu-id="438b9-118">Nainstalujte hello Universal SDK aplikace Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="438b9-118">Install hello Mobile Engagement Universal Apps SDK</span></span>
### <a name="all-platforms"></a><span data-ttu-id="438b9-119">Všechny platformy</span><span class="sxs-lookup"><span data-stu-id="438b9-119">All platforms</span></span>
<span data-ttu-id="438b9-120">Mobile Engagement SDK pro univerzální aplikace pro Windows Hello je k dispozici jako balíček Nuget s názvem *MicrosoftAzure.MobileEngagement*.</span><span class="sxs-lookup"><span data-stu-id="438b9-120">hello Mobile Engagement SDK for Windows Universal App is available as a Nuget package called *MicrosoftAzure.MobileEngagement*.</span></span> <span data-ttu-id="438b9-121">Můžete ji nainstalovat ze hello Správce balíčků Nuget Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="438b9-121">You can install it from hello Visual Studio Nuget Package Manager.</span></span>

### <a name="windows-8x-and-windows-phone-81"></a><span data-ttu-id="438b9-122">Windows 8.x a Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="438b9-122">Windows 8.x and Windows Phone 8.1</span></span>
<span data-ttu-id="438b9-123">NuGet automaticky nasadí hello prostředků sady SDK v hello `Resources` složku v kořenovém adresáři projektu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="438b9-123">NuGet automatically deploys hello SDK resources in hello `Resources` folder at hello root of your application project.</span></span>

### <a name="windows-10-universal-windows-platform-applications"></a><span data-ttu-id="438b9-124">Aplikace Windows 10 univerzální platformu Windows</span><span class="sxs-lookup"><span data-stu-id="438b9-124">Windows 10 Universal Windows Platform applications</span></span>
<span data-ttu-id="438b9-125">NuGet automaticky Nenasazuje hello SDK prostředky v aplikaci UWP ještě.</span><span class="sxs-lookup"><span data-stu-id="438b9-125">NuGet does not automatically deploy hello SDK resources in your UWP application yet.</span></span> <span data-ttu-id="438b9-126">Je nutné ji ručně až prostředky nasazení je znovu zavedena v NuGet toodo:</span><span class="sxs-lookup"><span data-stu-id="438b9-126">You have toodo it manually until resources deployment is reintroduced in NuGet:</span></span>

1. <span data-ttu-id="438b9-127">Otevřete váš Průzkumníka souborů.</span><span class="sxs-lookup"><span data-stu-id="438b9-127">Open your File Explorer.</span></span>
2. <span data-ttu-id="438b9-128">Přejděte toohello následující umístění (**x.x.x** je hello verzi Engagement instalujete): *% USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\\*  *x.x.x**\\content\win81*</span><span class="sxs-lookup"><span data-stu-id="438b9-128">Navigate toohello following location (**x.x.x** is hello version of Engagement you are installing): *%USERPROFILE%\\.nuget\packages\MicrosoftAzure.MobileEngagement\\**x.x.x**\\content\win81*</span></span>
3. <span data-ttu-id="438b9-129">Přetažení hello **prostředky** složku z hello souboru explorer toohello kořenového adresáře projektu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="438b9-129">Drag and drop hello **Resources** folder from hello file explorer toohello root of your project in Visual Studio.</span></span>
4. <span data-ttu-id="438b9-130">V sadě Visual Studio vyberte projekt a aktivovat hello **zobrazit všechny soubory** ikonu nad hello **Průzkumníku řešení**.</span><span class="sxs-lookup"><span data-stu-id="438b9-130">In Visual Studio select your project and activate hello **Show All files** icon on top of hello **Solution Explorer**.</span></span>
5. <span data-ttu-id="438b9-131">Některé soubory nejsou zahrnuty v projektu hello.</span><span class="sxs-lookup"><span data-stu-id="438b9-131">Some files are not included in hello project.</span></span> <span data-ttu-id="438b9-132">tooimport je najednou klikněte pravým tlačítkem na hello **prostředky** složky, **vyloučit z projektu** pak jiné klikněte pravým tlačítkem na hello **prostředky** složky, **zahrnout v projektu** toore-zahrnovat celou složku hello.</span><span class="sxs-lookup"><span data-stu-id="438b9-132">tooimport them at once right click on hello **Resources** folder, **Exclude from project** then another right click on hello **Resources** folder, **Include in project** toore-include hello whole folder.</span></span> <span data-ttu-id="438b9-133">Všechny soubory z hello **prostředky** složky jsou teď součástí projektu.</span><span class="sxs-lookup"><span data-stu-id="438b9-133">All files from hello **Resources** folder are now included in your project.</span></span>

## <a name="add-hello-capabilities"></a><span data-ttu-id="438b9-134">Přidání možností hello</span><span class="sxs-lookup"><span data-stu-id="438b9-134">Add hello capabilities</span></span>
<span data-ttu-id="438b9-135">Některé možnosti hello Windows SDK v pořadí toowork musí Hello Engagement SDK správně.</span><span class="sxs-lookup"><span data-stu-id="438b9-135">hello Engagement SDK needs some capabilities of hello Windows SDK in order toowork properly.</span></span>

<span data-ttu-id="438b9-136">Otevřete váš `Package.appxmanifest` soubor a zkontrolujte, zda jsou deklarovány této hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="438b9-136">Open your `Package.appxmanifest` file and be sure that hello following capabilities are declared:</span></span>

* `Internet (Client)`

## <a name="initialize-hello-engagement-sdk"></a><span data-ttu-id="438b9-137">Inicializace hello Engagement SDK</span><span class="sxs-lookup"><span data-stu-id="438b9-137">Initialize hello Engagement SDK</span></span>
### <a name="engagement-configuration"></a><span data-ttu-id="438b9-138">Konfigurace zapojení</span><span class="sxs-lookup"><span data-stu-id="438b9-138">Engagement configuration</span></span>
<span data-ttu-id="438b9-139">Konfigurace Engagement Hello je centralizovaná v hello `Resources\EngagementConfiguration.xml` souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="438b9-139">hello Engagement configuration is centralized in hello `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="438b9-140">Upravte tento soubor toospecify:</span><span class="sxs-lookup"><span data-stu-id="438b9-140">Edit this file toospecify:</span></span>

* <span data-ttu-id="438b9-141">Připojovací řetězec aplikací mezi značky `<connectionString>` a `<\connectionString>`.</span><span class="sxs-lookup"><span data-stu-id="438b9-141">Your application connection string between tags `<connectionString>` and `<\connectionString>`.</span></span>

<span data-ttu-id="438b9-142">Pokud chcete, aby ho za běhu místo toho můžete volat hello následující toospecify metoda před inicializací agenta hello Engagement:</span><span class="sxs-lookup"><span data-stu-id="438b9-142">If you want toospecify it at runtime instead, you can call hello following method before hello Engagement agent initialization:</span></span>

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set hello Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

<span data-ttu-id="438b9-143">Hello připojovací řetězec pro vaši aplikaci se zobrazí na portálu Azure Classic hello.</span><span class="sxs-lookup"><span data-stu-id="438b9-143">hello connection string for your application is displayed on hello Azure Classic Portal.</span></span>

### <a name="engagement-initialization"></a><span data-ttu-id="438b9-144">Inicializace zapojení</span><span class="sxs-lookup"><span data-stu-id="438b9-144">Engagement initialization</span></span>
<span data-ttu-id="438b9-145">Když vytvoříte nový projekt, `App.xaml.cs` se vygeneruje soubor.</span><span class="sxs-lookup"><span data-stu-id="438b9-145">When you create a new project, a `App.xaml.cs` file is generated.</span></span> <span data-ttu-id="438b9-146">Tato třída dědí z `Application` a obsahuje mnoho důležité metody.</span><span class="sxs-lookup"><span data-stu-id="438b9-146">This class inherits from `Application` and contains many important methods.</span></span> <span data-ttu-id="438b9-147">Bude také použít tooinitialize hello Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="438b9-147">It will also be used tooinitialize hello Engagement SDK.</span></span>

<span data-ttu-id="438b9-148">Upravit hello `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="438b9-148">Modify hello `App.xaml.cs`:</span></span>

* <span data-ttu-id="438b9-149">Přidat tooyour `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="438b9-149">Add tooyour `using` statements:</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="438b9-150">Definujte metoda tooshare hello Engagement inicializace jednou pro všechna volání:</span><span class="sxs-lookup"><span data-stu-id="438b9-150">Define a method tooshare hello Engagement initialization once for all calls:</span></span>
  
      private void InitEngagement(IActivatedEventArgs e)
      {
        EngagementAgent.Instance.Init(e);
  
        // or
  
        EngagementAgent.Instance.Init(e, engagementConfiguration);
      }
* <span data-ttu-id="438b9-151">Volání `InitEngagement` v hello `OnLaunched` metoda:</span><span class="sxs-lookup"><span data-stu-id="438b9-151">Call `InitEngagement` in hello `OnLaunched` method:</span></span>
  
      protected override void OnLaunched(LaunchActivatedEventArgs e)
      {
        InitEngagement(e);
      }
* <span data-ttu-id="438b9-152">Při spuštění vaší aplikace pomocí vlastní schéma jiná aplikace nebo hello příkazového řádku pak hello `OnActivated` metoda je volána.</span><span class="sxs-lookup"><span data-stu-id="438b9-152">When your application is launched using a custom scheme, another application or hello command line then hello `OnActivated` method is called.</span></span> <span data-ttu-id="438b9-153">Musíte taky tooinitiate hello Engagement SDK, když je aktivován vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="438b9-153">You also need tooinitiate hello Engagement SDK when your app is activated.</span></span> <span data-ttu-id="438b9-154">toodo tedy přepsat `OnActivated` metoda:</span><span class="sxs-lookup"><span data-stu-id="438b9-154">toodo so, override `OnActivated` method:</span></span>
  
      protected override void OnActivated(IActivatedEventArgs args)
      {
        InitEngagement(args);
      }

> [!IMPORTANT]
> <span data-ttu-id="438b9-155">Jsme důrazně bránit vám tooadd hello Engagement inicializace na jiném místě vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="438b9-155">We strongly discourage you tooadd hello Engagement initialization in another place of your application.</span></span>
> 
> 

## <a name="basic-reporting"></a><span data-ttu-id="438b9-156">Vytváření základních sestav</span><span class="sxs-lookup"><span data-stu-id="438b9-156">Basic reporting</span></span>
### <a name="recommended-method-overload-your-page-classes"></a><span data-ttu-id="438b9-157">Doporučená metoda: přetížení vaší `Page` třídy</span><span class="sxs-lookup"><span data-stu-id="438b9-157">Recommended method: overload your `Page` classes</span></span>
<span data-ttu-id="438b9-158">V pořadí tooactivate hello sestavu všechny protokoly hello vyžadují zapojení toocompute uživatelů, relací, aktivity, dojde k chybě a technické statistiky, můžete jednoduše provést všechny vaše `Page` dílčí třídy dědí hello `EngagementPage` třídy.</span><span class="sxs-lookup"><span data-stu-id="438b9-158">In order tooactivate hello report of all hello logs required by Engagement toocompute Users, Sessions, Activities, Crashes and Technical statistics, you can simply make all your `Page` sub-classes inherit from hello `EngagementPage` classes.</span></span>

<span data-ttu-id="438b9-159">Tady je příklad toho, jak toodo to pro stránku vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="438b9-159">Here is an example of how toodo this for a page of your application.</span></span> <span data-ttu-id="438b9-160">Můžete provést hello samé pro všechny stránky vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="438b9-160">You can do hello same thing for all pages of your application.</span></span>

#### <a name="c-source-file"></a><span data-ttu-id="438b9-161">C# zdrojový soubor</span><span class="sxs-lookup"><span data-stu-id="438b9-161">C# Source file</span></span>
<span data-ttu-id="438b9-162">Upravit stránku `.xaml.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="438b9-162">Modify your page `.xaml.cs` file:</span></span>

* <span data-ttu-id="438b9-163">Přidat tooyour `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="438b9-163">Add tooyour `using` statements:</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="438b9-164">Nahraďte `Page` s `EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="438b9-164">Replace `Page` with `EngagementPage`:</span></span>

<span data-ttu-id="438b9-165">**Bez Engagement:**</span><span class="sxs-lookup"><span data-stu-id="438b9-165">**Without Engagement:**</span></span>

        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

<span data-ttu-id="438b9-166">**S Engagement:**</span><span class="sxs-lookup"><span data-stu-id="438b9-166">**With Engagement:**</span></span>

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [!IMPORTANT]
> <span data-ttu-id="438b9-167">Pokud stránka přepíše hello `OnNavigatedTo` metoda, že toocall být `base.OnNavigatedTo(e)`.</span><span class="sxs-lookup"><span data-stu-id="438b9-167">If your page overrides hello `OnNavigatedTo` method, be sure toocall `base.OnNavigatedTo(e)`.</span></span> <span data-ttu-id="438b9-168">V opačném hello aktivita nebude hlášena (hello `EngagementPage` volání `StartActivity` uvnitř jeho `OnNavigatedTo` metoda).</span><span class="sxs-lookup"><span data-stu-id="438b9-168">Otherwise,  hello activity will not be reported (hello `EngagementPage` calls `StartActivity` inside its `OnNavigatedTo` method).</span></span>
> 
> 

#### <a name="xaml-file"></a><span data-ttu-id="438b9-169">Souboru XAML</span><span class="sxs-lookup"><span data-stu-id="438b9-169">XAML file</span></span>
<span data-ttu-id="438b9-170">Upravit stránku `.xaml` souboru:</span><span class="sxs-lookup"><span data-stu-id="438b9-170">Modify your page `.xaml` file:</span></span>

* <span data-ttu-id="438b9-171">Přidejte tooyour deklarací oborů názvů:</span><span class="sxs-lookup"><span data-stu-id="438b9-171">Add tooyour namespaces declarations:</span></span>
  
      xmlns:engagement="using:Microsoft.Azure.Engagement"
* <span data-ttu-id="438b9-172">Nahraďte `Page` s `engagement:EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="438b9-172">Replace `Page` with `engagement:EngagementPage`:</span></span>

<span data-ttu-id="438b9-173">**Bez Engagement:**</span><span class="sxs-lookup"><span data-stu-id="438b9-173">**Without Engagement:**</span></span>

        <Page>
            <!-- layout -->
            ...
        </Page>

<span data-ttu-id="438b9-174">**S Engagement:**</span><span class="sxs-lookup"><span data-stu-id="438b9-174">**With Engagement:**</span></span>

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

#### <a name="override-hello-default-behaviour"></a><span data-ttu-id="438b9-175">Přepsat výchozí chování hello</span><span class="sxs-lookup"><span data-stu-id="438b9-175">Override hello default behaviour</span></span>
<span data-ttu-id="438b9-176">Ve výchozím nastavení je název třídy hello stránku hello uvedená jako název aktivity hello a žádné navíc.</span><span class="sxs-lookup"><span data-stu-id="438b9-176">By default, hello class name of hello page is reported as hello activity name, with no extra.</span></span> <span data-ttu-id="438b9-177">Pokud třída hello používá příponu "Stránka" hello, Engagement také odebere.</span><span class="sxs-lookup"><span data-stu-id="438b9-177">If hello class uses hello "Page" suffix, Engagement will also remove it.</span></span>

<span data-ttu-id="438b9-178">Pokud chcete toooverride hello výchozí chování pro název hello, jednoduše přidejte tento kód tooyour:</span><span class="sxs-lookup"><span data-stu-id="438b9-178">If you want toooverride hello default behaviour for hello name, simply add this tooyour code:</span></span>

        // in hello .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

<span data-ttu-id="438b9-179">Pokud chcete, tooreport některé další – informace s vaší aktivitou, můžete přidat tento kód tooyour:</span><span class="sxs-lookup"><span data-stu-id="438b9-179">If you want tooreport some extra informations with your activity, you can add this tooyour code:</span></span>

        // in hello .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

<span data-ttu-id="438b9-180">Tyto metody jsou volat v rámci hello `OnNavigatedTo` metoda stránky.</span><span class="sxs-lookup"><span data-stu-id="438b9-180">These methods are called from within hello `OnNavigatedTo` method of your page.</span></span>

### <a name="alternate-method-call-startactivity-manually"></a><span data-ttu-id="438b9-181">Alternativní metoda: volání `StartActivity()` ručně</span><span class="sxs-lookup"><span data-stu-id="438b9-181">Alternate method: call `StartActivity()` manually</span></span>
<span data-ttu-id="438b9-182">Pokud nemůžete nebo nechcete, aby toooverload vaše `Page` třídy, můžete místo toho spustit vaše aktivity voláním `EngagementAgent` metody přímo.</span><span class="sxs-lookup"><span data-stu-id="438b9-182">If you cannot or do not want toooverload your `Page` classes, you can instead start your activities by calling `EngagementAgent` methods directly.</span></span>

<span data-ttu-id="438b9-183">Doporučujeme, abyste toocall `StartActivity` uvnitř vaší `OnNavigatedTo` metoda stránky.</span><span class="sxs-lookup"><span data-stu-id="438b9-183">We recommend toocall `StartActivity` inside your `OnNavigatedTo` method of your Page.</span></span>

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [!IMPORTANT]
> <span data-ttu-id="438b9-184">Zajistěte, aby že správně ukončete svou relaci.</span><span class="sxs-lookup"><span data-stu-id="438b9-184">Ensure you end your session correctly.</span></span>
> 
> <span data-ttu-id="438b9-185">Hello sady Windows Universal SDK automaticky volá hello `EndActivity` metoda při zavření aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="438b9-185">hello Windows Universal SDK automatically calls hello `EndActivity` method when hello application is closed.</span></span> <span data-ttu-id="438b9-186">Z toho důvodu je **vysoce** doporučená toocall hello `StartActivity` metoda vždy, když aktivita hello hello uživatele změnit a příliš**nikdy** volání hello `EndActivity` metoda, tato metoda odesílá tooEngagement Server, že má aktuální uživatel aplikace hello ponechejte, budou ovlivňuje všechny protokoly aplikací.</span><span class="sxs-lookup"><span data-stu-id="438b9-186">Thus, it is **HIGHLY** recommended toocall hello `StartActivity` method whenever hello activity of hello user change, and too**NEVER** call hello `EndActivity` method, this method sends tooEngagement server that current user has leave hello application, this will impacts all application logs.</span></span>
> 
> 

## <a name="advanced-reporting"></a><span data-ttu-id="438b9-187">Rozšířená tvorba sestav</span><span class="sxs-lookup"><span data-stu-id="438b9-187">Advanced reporting</span></span>
<span data-ttu-id="438b9-188">Volitelně můžete tooreport aplikace konkrétní události, chyb a úlohy, toodo tak, použijte hello další metody nalezeny v hello `EngagementAgent` třídy.</span><span class="sxs-lookup"><span data-stu-id="438b9-188">Optionally, you may want tooreport application specific events, errors and jobs, toodo so, use hello others methods found in hello `EngagementAgent` class.</span></span> <span data-ttu-id="438b9-189">Hello Engagement API umožňuje toouse všechny rozšířené možnosti Engagement.</span><span class="sxs-lookup"><span data-stu-id="438b9-189">hello Engagement API allows toouse all of Engagement's advanced capabilities.</span></span>

<span data-ttu-id="438b9-190">Další informace najdete v tématu [jak toouse hello advanced označování rozhraní API v aplikaci Windows Universal Mobile Engagement](mobile-engagement-windows-store-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="438b9-190">For further information, see [How toouse hello advanced Mobile Engagement tagging API in your Windows Universal app](mobile-engagement-windows-store-use-engagement-api.md).</span></span>

## <a name="advanced-configuration"></a><span data-ttu-id="438b9-191">Pokročilá konfigurace</span><span class="sxs-lookup"><span data-stu-id="438b9-191">Advanced configuration</span></span>
### <a name="disable-automatic-crash-reporting"></a><span data-ttu-id="438b9-192">Zakázat automatické hlášení chyb</span><span class="sxs-lookup"><span data-stu-id="438b9-192">Disable automatic crash reporting</span></span>
<span data-ttu-id="438b9-193">Můžete vypnout automatické hlášení hello funkci Engagement generování sestav.</span><span class="sxs-lookup"><span data-stu-id="438b9-193">You can disable hello automatic crash reporting feature of Engagement.</span></span> <span data-ttu-id="438b9-194">Pak když dojde k neošetřené výjimce, Engagement nebude provádět žádné kroky.</span><span class="sxs-lookup"><span data-stu-id="438b9-194">Then, when an unhandled exception will occur, Engagement won't do anything.</span></span>

> [!WARNING]
> <span data-ttu-id="438b9-195">Pokud máte v plánu toodisable tuto funkci, mějte na paměti, když dojde k nezpracované chybě ve vaší aplikaci, nepošle Engagement hello havárií **a** nebude zavřete hello relace a úlohy.</span><span class="sxs-lookup"><span data-stu-id="438b9-195">If you plan toodisable this feature, be aware that when a unhandled crash will occur in your app, Engagement will not send hello crash **AND** will not close hello session and jobs.</span></span>
> 
> 

<span data-ttu-id="438b9-196">Automatické hlášení toodisable vytváření sestav, stačí upravit konfiguraci v závislosti na způsob hello deklarujete objekt:</span><span class="sxs-lookup"><span data-stu-id="438b9-196">toodisable automatic crash reporting, just customize your configuration depending on hello way you declared it:</span></span>

#### <a name="from-engagementconfigurationxml-file"></a><span data-ttu-id="438b9-197">Z `EngagementConfiguration.xml` souboru</span><span class="sxs-lookup"><span data-stu-id="438b9-197">From `EngagementConfiguration.xml` file</span></span>
<span data-ttu-id="438b9-198">Nastavit sestavy zhroutí příliš`false` mezi `<reportCrash>` a `</reportCrash>` značky.</span><span class="sxs-lookup"><span data-stu-id="438b9-198">Set report crash too`false` between `<reportCrash>` and `</reportCrash>` tags.</span></span>

#### <a name="from-engagementconfiguration-object-at-run-time"></a><span data-ttu-id="438b9-199">Z `EngagementConfiguration` objektu v době běhu</span><span class="sxs-lookup"><span data-stu-id="438b9-199">From `EngagementConfiguration` object at run time</span></span>
<span data-ttu-id="438b9-200">Nastavit toofalse havárií sestavy pomocí EngagementConfiguration objektu.</span><span class="sxs-lookup"><span data-stu-id="438b9-200">Set report crash toofalse using your EngagementConfiguration object.</span></span>

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a><span data-ttu-id="438b9-201">Režim shluku</span><span class="sxs-lookup"><span data-stu-id="438b9-201">Burst mode</span></span>
<span data-ttu-id="438b9-202">Ve výchozím nastavení sestavy služby zapojení hello protokolů v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="438b9-202">By default, hello Engagement service reports logs in real time.</span></span> <span data-ttu-id="438b9-203">Pokud vaše aplikace hlásí protokoly velmi často, je lepší toobuffer hello protokoly a tooreport je všechny najednou na pravidelné základní časové (tomu se říká hello "shluků režim").</span><span class="sxs-lookup"><span data-stu-id="438b9-203">If your application reports logs very frequently, it is better toobuffer hello logs and tooreport them all at once on a regular time base (this is called hello “burst mode”).</span></span>

<span data-ttu-id="438b9-204">toodo tedy volat metodu hello:</span><span class="sxs-lookup"><span data-stu-id="438b9-204">toodo so, call hello method:</span></span>

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

<span data-ttu-id="438b9-205">Hello argument je hodnota **milisekundách**.</span><span class="sxs-lookup"><span data-stu-id="438b9-205">hello argument is a value in **milliseconds**.</span></span> <span data-ttu-id="438b9-206">Kdykoli Pokud chcete protokolování v reálném čase hello tooreactivate, stačí zavoláte metoda hello bez zadání parametru nebo s hodnotou hello 0.</span><span class="sxs-lookup"><span data-stu-id="438b9-206">At any time, if you want tooreactivate hello real-time logging, just call hello method without any parameter, or with hello 0 value.</span></span>

<span data-ttu-id="438b9-207">Hello shluků režimu mírně zvýšit hello baterie životnosti ale má vliv na hello monitorování Engagement: všechny doba trvání relace a úlohy se prahová hodnota shluků zaokrouhlené toohello (tedy relací a úlohy kratší, než je prahová hodnota shluků hello nemusí být zobrazeny).</span><span class="sxs-lookup"><span data-stu-id="438b9-207">hello burst mode slightly increase hello battery life but has an impact on hello Engagement Monitor: all sessions and jobs duration will be rounded toohello burst threshold (thus, sessions and jobs shorter than hello burst threshold may not be visible).</span></span> <span data-ttu-id="438b9-208">Je doporučeno toouse práh shluků než 30000 (30s).</span><span class="sxs-lookup"><span data-stu-id="438b9-208">It is recommended toouse a burst threshold no longer than 30000 (30s).</span></span> <span data-ttu-id="438b9-209">Máte toobe vědět, které uložené protokoly jsou omezené too300 položky.</span><span class="sxs-lookup"><span data-stu-id="438b9-209">You have toobe aware that saved logs are limited too300 items.</span></span> <span data-ttu-id="438b9-210">Pokud odesílání je příliš dlouhý. může dojít ke ztrátě některých protokoly.</span><span class="sxs-lookup"><span data-stu-id="438b9-210">If sending is too long you can lose some logs.</span></span>

> [!WARNING]
> <span data-ttu-id="438b9-211">Prahová hodnota shluků Hello nelze konfigurovat tooa období menší než hodnotami 1.</span><span class="sxs-lookup"><span data-stu-id="438b9-211">hello burst threshold cannot be configured tooa period lesser than 1s.</span></span> <span data-ttu-id="438b9-212">Pokud se pokusíte toodo tak, hello SDK bude zobrazit trasování s chybou hello a bude automaticky toohello výchozí hodnotu, tedy 0s obnovit.</span><span class="sxs-lookup"><span data-stu-id="438b9-212">If you try toodo so, hello SDK will show a trace with hello error and will automatically reset toohello default value, i.e., 0s.</span></span> <span data-ttu-id="438b9-213">Tím se aktivuje hello SDK tooreport hello protokolů v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="438b9-213">This will trigger hello SDK tooreport hello logs in real-time.</span></span>
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview

