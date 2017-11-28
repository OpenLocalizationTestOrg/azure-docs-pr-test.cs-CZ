---
title: aaaWindows integraci sady Engagement SDK Phone Silverlight
description: Jak tooIntegrate Azure Mobile Engagement s aplikacemi pro Windows Phone Silverlight
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 447fea8d-f4e3-4ad4-8ec0-8e3cf1ad3ab0
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: f65683a62e5256cea469a3a73d99ade4331cb6bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="windows-phone-silverlight-engagement-sdk-integration"></a><span data-ttu-id="9c23f-103">Integraci sady Windows Phone Silverlight Engagement SDK</span><span class="sxs-lookup"><span data-stu-id="9c23f-103">Windows Phone Silverlight Engagement SDK Integration</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9c23f-104">Univerzální pro Windows</span><span class="sxs-lookup"><span data-stu-id="9c23f-104">Windows Universal</span></span>](mobile-engagement-windows-store-integrate-engagement.md) 
> * [<span data-ttu-id="9c23f-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="9c23f-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md) 
> * [<span data-ttu-id="9c23f-106">iOS</span><span class="sxs-lookup"><span data-stu-id="9c23f-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md) 
> * [<span data-ttu-id="9c23f-107">Android</span><span class="sxs-lookup"><span data-stu-id="9c23f-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md) 
> 
> 

<span data-ttu-id="9c23f-108">Tento postup popisuje hello nejjednodušší způsob, jak tooactivate Azure Mobile Engagement analýzy a monitorování funkce v aplikaci Silverlight pro Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="9c23f-108">This procedure describes hello simplest way tooactivate Azure Mobile Engagement's Analytics and Monitoring functions in your Windows Phone Silverlight application.</span></span>

<span data-ttu-id="9c23f-109">Hello následující kroky jsou že dostatek tooactivate hello protokolů je nutná sestava toocompute všechny statistické údaje týkající se uživatelů, relací, aktivity, dojde k chybě a Technicals.</span><span class="sxs-lookup"><span data-stu-id="9c23f-109">hello following steps are enough tooactivate hello report of logs needed toocompute all statistics regarding Users, Sessions, Activities, Crashes and Technicals.</span></span> <span data-ttu-id="9c23f-110">Hello protokolů je nutná sestava toocompute další statistiky jako události a chyby úlohy je třeba provést ručně pomocí hello Engagement rozhraní API (v tématu [jak toouse hello advanced Mobile Engagement označování rozhraní API v aplikaci Windows Phone Silverlight](mobile-engagement-windows-phone-use-engagement-api.md) dole) vzhledem k tomu, že tyto statistické údaje jsou závislé aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c23f-110">hello report of logs needed toocompute other statistics like Events, Errors and Jobs must be done manually using hello Engagement API (see [How toouse hello advanced Mobile Engagement tagging API in your Windows Phone Silverlight app](mobile-engagement-windows-phone-use-engagement-api.md) below) since these statistics are application-dependent.</span></span>

## <a name="supported-versions"></a><span data-ttu-id="9c23f-111">Podporované verze</span><span class="sxs-lookup"><span data-stu-id="9c23f-111">Supported versions</span></span>
<span data-ttu-id="9c23f-112">Mobile Engagement SDK pro Silverlight pro Windows Hello lze pouze integrovat do aplikace cílený na:</span><span class="sxs-lookup"><span data-stu-id="9c23f-112">hello Mobile Engagement SDK for Windows Silverlight can only be integrated into applications targeting :</span></span>

* <span data-ttu-id="9c23f-113">Windows Phone 8.0</span><span class="sxs-lookup"><span data-stu-id="9c23f-113">Windows Phone 8.0</span></span>
* <span data-ttu-id="9c23f-114">Windows Phone 8.1 Silverlight</span><span class="sxs-lookup"><span data-stu-id="9c23f-114">Windows Phone 8.1 Silverlight</span></span>

> [!NOTE]
> <span data-ttu-id="9c23f-115">Pokud jste se cílení na Windows Phone 8.1 (bez Silverlight), pak odkazovat toohello [univerzální pro Windows postup integrace](mobile-engagement-windows-store-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="9c23f-115">If you are targeting Windows Phone 8.1 (non-Silverlight) then refer toohello [Windows Universal integration procedure](mobile-engagement-windows-store-integrate-engagement.md).</span></span>
> 
> 

## <a name="install-hello-mobile-engagement-silverlight-sdk"></a><span data-ttu-id="9c23f-116">Nainstalujte hello Mobile Engagement Silverlight SDK</span><span class="sxs-lookup"><span data-stu-id="9c23f-116">Install hello Mobile Engagement Silverlight SDK</span></span>
<span data-ttu-id="9c23f-117">Mobile Engagement SDK pro Silverlight pro Windows Hello je k dispozici jako balíček Nuget s názvem *MicrosoftAzure.MobileEngagement*.</span><span class="sxs-lookup"><span data-stu-id="9c23f-117">hello Mobile Engagement SDK for Windows Silverlight is available as a Nuget package called *MicrosoftAzure.MobileEngagement*.</span></span> <span data-ttu-id="9c23f-118">Můžete ji nainstalovat ze hello Správce balíčků Nuget Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9c23f-118">You can install it from hello Visual Studio Nuget Package Manager.</span></span> 

## <a name="add-hello-capabilities"></a><span data-ttu-id="9c23f-119">Přidání možností hello</span><span class="sxs-lookup"><span data-stu-id="9c23f-119">Add hello capabilities</span></span>
<span data-ttu-id="9c23f-120">Některé možnosti hello Windows Phone Silverlight SDK v pořadí toowork musí Hello Engagement SDK správně.</span><span class="sxs-lookup"><span data-stu-id="9c23f-120">hello Engagement SDK needs some capabilities of hello Windows Phone Silverlight SDK in order toowork properly.</span></span>

<span data-ttu-id="9c23f-121">Otevřete váš `WMAppManifest.xml` souboru a ujistěte se, že hello následující funkce jsou deklarované v hello `Capabilities` panelu:</span><span class="sxs-lookup"><span data-stu-id="9c23f-121">Open your `WMAppManifest.xml` file and be sure that hello following capabilities are declared in hello `Capabilities` panel:</span></span>

* `ID_CAP_NETWORKING`
* `ID_CAP_IDENTITY_DEVICE`

## <a name="initialize-hello-engagement-sdk"></a><span data-ttu-id="9c23f-122">Inicializace hello Engagement SDK</span><span class="sxs-lookup"><span data-stu-id="9c23f-122">Initialize hello Engagement SDK</span></span>
### <a name="engagement-configuration"></a><span data-ttu-id="9c23f-123">Konfigurace zapojení</span><span class="sxs-lookup"><span data-stu-id="9c23f-123">Engagement configuration</span></span>
<span data-ttu-id="9c23f-124">Konfigurace Engagement Hello je centralizovaná v hello `Resources\EngagementConfiguration.xml` souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="9c23f-124">hello Engagement configuration is centralized in hello `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="9c23f-125">Upravte tento soubor toospecify:</span><span class="sxs-lookup"><span data-stu-id="9c23f-125">Edit this file toospecify :</span></span>

* <span data-ttu-id="9c23f-126">Připojovací řetězec aplikací mezi značky `<connectionString>` a `<\connectionString>`.</span><span class="sxs-lookup"><span data-stu-id="9c23f-126">Your application connection string between tags `<connectionString>` and `<\connectionString>`.</span></span>

<span data-ttu-id="9c23f-127">Pokud chcete, aby ho za běhu místo toho můžete volat hello následující toospecify metoda před inicializací agenta hello Engagement:</span><span class="sxs-lookup"><span data-stu-id="9c23f-127">If you want toospecify it at runtime instead, you can call hello following method before hello Engagement agent initialization:</span></span>

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

<span data-ttu-id="9c23f-128">Hello připojovací řetězec pro vaši aplikaci se zobrazí na portálu Azure Classic hello.</span><span class="sxs-lookup"><span data-stu-id="9c23f-128">hello connection string for your application is displayed on hello Azure Classic Portal.</span></span>

### <a name="engagement-initialization"></a><span data-ttu-id="9c23f-129">Inicializace zapojení</span><span class="sxs-lookup"><span data-stu-id="9c23f-129">Engagement initialization</span></span>
<span data-ttu-id="9c23f-130">Když vytvoříte nový projekt, `App.xaml.cs` se vygeneruje soubor.</span><span class="sxs-lookup"><span data-stu-id="9c23f-130">When you create a new project, a `App.xaml.cs` file is generated.</span></span> <span data-ttu-id="9c23f-131">Tato třída dědí z `Application` a obsahuje mnoho důležité metody.</span><span class="sxs-lookup"><span data-stu-id="9c23f-131">This class inherits from `Application` and contains many important methods.</span></span> <span data-ttu-id="9c23f-132">Bude také použít tooinitialize hello Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="9c23f-132">It will also be used tooinitialize hello Engagement SDK.</span></span>

<span data-ttu-id="9c23f-133">Upravit hello `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="9c23f-133">Modify hello `App.xaml.cs`:</span></span>

* <span data-ttu-id="9c23f-134">Přidat tooyour `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="9c23f-134">Add tooyour `using` statements :</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="9c23f-135">Vložit `EngagementAgent.Instance.Init` v hello `Application_Launching` metoda:</span><span class="sxs-lookup"><span data-stu-id="9c23f-135">Insert `EngagementAgent.Instance.Init` in hello `Application_Launching` method :</span></span>
  
      private void Application_Launching(object sender, LaunchingEventArgs e)
      {
        EngagementAgent.Instance.Init();
      }
* <span data-ttu-id="9c23f-136">Vložit `EngagementAgent.Instance.OnActivated` v hello `Application_Activated` metoda:</span><span class="sxs-lookup"><span data-stu-id="9c23f-136">Insert `EngagementAgent.Instance.OnActivated` in hello `Application_Activated` method :</span></span>
  
      private void Application_Activated(object sender, ActivatedEventArgs e)
      {
         EngagementAgent.Instance.OnActivated(e);
      }

> [!WARNING]
> <span data-ttu-id="9c23f-137">Jsme důrazně bránit vám tooadd hello Engagement inicializace na jiném místě vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c23f-137">We strongly discourage you tooadd hello Engagement initialization in another place of your application.</span></span> <span data-ttu-id="9c23f-138">Ale, uvědomte si, že hello `EngagementAgent.Instance.Init` metoda se spouští na vyhrazené vlákno a ne na hello vlákna uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9c23f-138">However, be aware that hello `EngagementAgent.Instance.Init` method runs on a dedicated thread, and not on hello UI thread.</span></span>
> 
> 

## <a name="basic-reporting"></a><span data-ttu-id="9c23f-139">Vytváření základních sestav</span><span class="sxs-lookup"><span data-stu-id="9c23f-139">Basic reporting</span></span>
### <a name="recommended-method--overload-your-phoneapplicationpage-classes"></a><span data-ttu-id="9c23f-140">Doporučená metoda: přetížení vaší `PhoneApplicationPage` třídy</span><span class="sxs-lookup"><span data-stu-id="9c23f-140">Recommended method : overload your `PhoneApplicationPage` classes</span></span>
<span data-ttu-id="9c23f-141">V pořadí tooactivate hello sestavu všechny protokoly hello vyžadují zapojení toocompute uživatelů, relací, aktivity, dojde k chybě a technické statistiky, můžete jednoduše provést všechny vaše `PhoneApplicationPage` dílčí třídy dědí hello `EngagementPage` třídy.</span><span class="sxs-lookup"><span data-stu-id="9c23f-141">In order tooactivate hello report of all hello logs required by Engagement toocompute Users, Sessions, Activities, Crashes and Technical statistics, you can simply make all your `PhoneApplicationPage` sub-classes inherit from hello `EngagementPage` classes.</span></span>

<span data-ttu-id="9c23f-142">Tady je příklad toho, jak toodo to pro stránku vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c23f-142">Here is an example of how toodo this for a page of your application.</span></span> <span data-ttu-id="9c23f-143">Můžete provést hello samé pro všechny stránky vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c23f-143">You can do hello same thing for all pages of your application.</span></span>

#### <a name="c-source-file"></a><span data-ttu-id="9c23f-144">C# zdrojový soubor</span><span class="sxs-lookup"><span data-stu-id="9c23f-144">C# Source file</span></span>
<span data-ttu-id="9c23f-145">Upravit stránku `.xaml.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="9c23f-145">Modify your page `.xaml.cs` file :</span></span>

* <span data-ttu-id="9c23f-146">Přidat tooyour `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="9c23f-146">Add tooyour `using` statements :</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="9c23f-147">Nahraďte `PhoneApplicationPage` s `EngagementPage` :</span><span class="sxs-lookup"><span data-stu-id="9c23f-147">Replace `PhoneApplicationPage` with `EngagementPage` :</span></span>

<span data-ttu-id="9c23f-148">**Bez Engagement:**</span><span class="sxs-lookup"><span data-stu-id="9c23f-148">**Without Engagement:**</span></span>

        namespace Example
        {
          public partial class ExamplePage : PhoneApplicationPage
          {
            [...]
          }
        }

<span data-ttu-id="9c23f-149">**S Engagement:**</span><span class="sxs-lookup"><span data-stu-id="9c23f-149">**With Engagement:**</span></span>

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [!WARNING]
> <span data-ttu-id="9c23f-150">Pokud stránka dědí z hello `OnNavigatedTo` metoda, být opatrní toolet hello `base.OnNavigatedTo(e)` volání.</span><span class="sxs-lookup"><span data-stu-id="9c23f-150">If your page inherits from hello `OnNavigatedTo` method, be careful toolet hello `base.OnNavigatedTo(e)` call.</span></span> <span data-ttu-id="9c23f-151">V opačném hello aktivita nebude hlášena.</span><span class="sxs-lookup"><span data-stu-id="9c23f-151">Otherwise, hello activity will not be reported.</span></span> <span data-ttu-id="9c23f-152">Ve skutečnosti hello `EngagementPage` volá `StartActivity` uvnitř hello `OnNavigatedTo` metoda.</span><span class="sxs-lookup"><span data-stu-id="9c23f-152">Indeed, hello `EngagementPage` is calling `StartActivity` inside hello `OnNavigatedTo` method.</span></span>
> 
> 

#### <a name="xaml-file"></a><span data-ttu-id="9c23f-153">Souboru XAML</span><span class="sxs-lookup"><span data-stu-id="9c23f-153">XAML file</span></span>
<span data-ttu-id="9c23f-154">Upravit stránku `.xaml` souboru:</span><span class="sxs-lookup"><span data-stu-id="9c23f-154">Modify your page `.xaml` file :</span></span>

* <span data-ttu-id="9c23f-155">Přidejte tooyour deklarací oborů názvů:</span><span class="sxs-lookup"><span data-stu-id="9c23f-155">Add tooyour namespaces declarations :</span></span>
  
      xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
* <span data-ttu-id="9c23f-156">Nahraďte `phone:PhoneApplicationPage` s `engagement:EngagementPage` :</span><span class="sxs-lookup"><span data-stu-id="9c23f-156">Replace `phone:PhoneApplicationPage` with `engagement:EngagementPage` :</span></span>

<span data-ttu-id="9c23f-157">**Bez Engagement:**</span><span class="sxs-lookup"><span data-stu-id="9c23f-157">**Without Engagement:**</span></span>

        <phone:PhoneApplicationPage>
            <!-- layout -->
        </phone:PhoneApplicationPage>

<span data-ttu-id="9c23f-158">**S Engagement:**</span><span class="sxs-lookup"><span data-stu-id="9c23f-158">**With Engagement:**</span></span>

        <engagement:EngagementPage 
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP">

            <!-- layout -->
        </engagement:EngagementPage >

#### <a name="override-hello-default-behavior"></a><span data-ttu-id="9c23f-159">Přepsat výchozí chování hello</span><span class="sxs-lookup"><span data-stu-id="9c23f-159">Override hello default behavior</span></span>
<span data-ttu-id="9c23f-160">Ve výchozím nastavení je název třídy hello stránku hello uvedená jako název aktivity hello a žádné navíc.</span><span class="sxs-lookup"><span data-stu-id="9c23f-160">By default, hello class name of hello page is reported as hello activity name, with no extra.</span></span> <span data-ttu-id="9c23f-161">Pokud třída hello používá příponu "Stránka" hello, Engagement také odebere.</span><span class="sxs-lookup"><span data-stu-id="9c23f-161">If hello class uses hello "Page" suffix, Engagement will also remove it.</span></span>

<span data-ttu-id="9c23f-162">Pokud chcete toooverride hello výchozí chování pro název hello, jednoduše přidejte tento kód tooyour:</span><span class="sxs-lookup"><span data-stu-id="9c23f-162">If you want toooverride hello default behavior for hello name, simply add this tooyour code:</span></span>

        // in hello .xaml.cs file
        protected override string GetEngagementPageName()
        {
           /* your code */
           return "new name";
        }

<span data-ttu-id="9c23f-163">Pokud chcete, tooreport některé doplňující informace s vaší aktivitou, můžete přidat tento kód tooyour:</span><span class="sxs-lookup"><span data-stu-id="9c23f-163">If you want tooreport some extra information with your activity, you can add this tooyour code:</span></span>

        // in hello .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
           /* your code */
           return extra;
        }

<span data-ttu-id="9c23f-164">Tyto metody jsou volat v rámci hello `OnNavigatedTo` metoda stránky.</span><span class="sxs-lookup"><span data-stu-id="9c23f-164">These methods are called from within hello `OnNavigatedTo` method of your page.</span></span>

### <a name="alternate-method-call-startactivity-manually"></a><span data-ttu-id="9c23f-165">Alternativní metoda: volání `StartActivity()` ručně</span><span class="sxs-lookup"><span data-stu-id="9c23f-165">Alternate method: call `StartActivity()` manually</span></span>
<span data-ttu-id="9c23f-166">Pokud nemůžete nebo nechcete, aby toooverload vaše `PhoneApplicationPage` třídy, můžete místo toho spustit vaše aktivity voláním `EngagementAgent` metody přímo.</span><span class="sxs-lookup"><span data-stu-id="9c23f-166">If you cannot or do not want toooverload your `PhoneApplicationPage` classes, you can instead start your activities by calling `EngagementAgent` methods directly.</span></span>

<span data-ttu-id="9c23f-167">Doporučujeme, abyste volání `StartActivity` uvnitř vaší `OnNavigatedTo` metoda vaší PhoneApplicationPage.</span><span class="sxs-lookup"><span data-stu-id="9c23f-167">We recommend calling `StartActivity` inside your `OnNavigatedTo` method of your PhoneApplicationPage.</span></span>

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
           base.OnNavigatedTo(e);
           EngagementAgent.Instance.StartActivity("MyPage");
        }

> [!IMPORTANT]
> <span data-ttu-id="9c23f-168">Zajistěte, aby že správně ukončete svou relaci.</span><span class="sxs-lookup"><span data-stu-id="9c23f-168">Ensure you end your session correctly.</span></span>
> 
> <span data-ttu-id="9c23f-169">Hello SDK automaticky volá hello `EndActivity` metoda při zavření aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="9c23f-169">hello SDK automatically calls hello `EndActivity` method when hello application is closed.</span></span> <span data-ttu-id="9c23f-170">Z toho důvodu je **vysoce** doporučená toocall hello `StartActivity` metoda vždy, když aktivita hello hello uživatele změnit a příliš**nikdy** volání hello `EndActivity` metoda.</span><span class="sxs-lookup"><span data-stu-id="9c23f-170">Thus, it is **HIGHLY** recommended toocall hello `StartActivity` method whenever hello activity of hello user change, and too**NEVER** call hello `EndActivity` method.</span></span> <span data-ttu-id="9c23f-171">Tato metoda odesílá server Engagement zpráv toohello, že aktuální uživatel hello opustil hello aplikace, a to ovlivní všechny protokoly aplikací.</span><span class="sxs-lookup"><span data-stu-id="9c23f-171">This method sends a message toohello Engagement server that hello current user has left hello application and this impacts all application logs.</span></span>
> 
> 

## <a name="advanced-reporting"></a><span data-ttu-id="9c23f-172">Rozšířená tvorba sestav</span><span class="sxs-lookup"><span data-stu-id="9c23f-172">Advanced reporting</span></span>
<span data-ttu-id="9c23f-173">Volitelně můžete tooreport aplikace konkrétní události, chyb a úlohy, toodo tak, použijte hello další metody nalezeny v hello `EngagementAgent` třídy.</span><span class="sxs-lookup"><span data-stu-id="9c23f-173">Optionally, you may want tooreport application specific events, errors and jobs, toodo so, use hello others methods found in hello `EngagementAgent` class.</span></span> <span data-ttu-id="9c23f-174">Hello Engagement API umožňuje toouse všechny rozšířené možnosti Engagement.</span><span class="sxs-lookup"><span data-stu-id="9c23f-174">hello Engagement API allows toouse all of Engagement's advanced capabilities.</span></span>

<span data-ttu-id="9c23f-175">Další informace najdete v tématu [jak toouse hello advanced Mobile Engagement označování rozhraní API v aplikaci Windows Phone Silverlight](mobile-engagement-windows-phone-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="9c23f-175">For further information, see [How toouse hello advanced Mobile Engagement tagging API in your Windows Phone Silverlight app](mobile-engagement-windows-phone-use-engagement-api.md).</span></span>

## <a name="advanced-configuration"></a><span data-ttu-id="9c23f-176">Pokročilá konfigurace</span><span class="sxs-lookup"><span data-stu-id="9c23f-176">Advanced configuration</span></span>
### <a name="disable-automatic-crash-reporting"></a><span data-ttu-id="9c23f-177">Zakázat automatické hlášení chyb</span><span class="sxs-lookup"><span data-stu-id="9c23f-177">Disable automatic crash reporting</span></span>
<span data-ttu-id="9c23f-178">Můžete vypnout automatické hlášení hello funkci Engagement generování sestav.</span><span class="sxs-lookup"><span data-stu-id="9c23f-178">You can disable hello automatic crash reporting feature of Engagement.</span></span> <span data-ttu-id="9c23f-179">Pak když dojde k neošetřené výjimce, Engagement nebude provádět žádné kroky.</span><span class="sxs-lookup"><span data-stu-id="9c23f-179">Then, when an unhandled exception will occur, Engagement won't do anything.</span></span>

> [!WARNING]
> <span data-ttu-id="9c23f-180">Pokud máte v plánu toodisable tuto funkci, mějte na paměti, když dojde k nezpracované chybě ve vaší aplikaci, nepošle Engagement hello havárií **a** zavřít hello relace a úlohy.</span><span class="sxs-lookup"><span data-stu-id="9c23f-180">If you plan toodisable this feature, be aware that when a unhandled crash will occur in your app, Engagement will not send hello crash **AND** it will not close hello session and jobs.</span></span>
> 
> 

<span data-ttu-id="9c23f-181">Automatické hlášení toodisable vytváření sestav, stačí upravit konfiguraci v závislosti na způsob hello deklarujete objekt:</span><span class="sxs-lookup"><span data-stu-id="9c23f-181">toodisable automatic crash reporting, just customize your configuration depending on hello way you declared it :</span></span>

#### <a name="from-engagementconfigurationxml-file"></a><span data-ttu-id="9c23f-182">Z `EngagementConfiguration.xml` souboru</span><span class="sxs-lookup"><span data-stu-id="9c23f-182">From `EngagementConfiguration.xml` file</span></span>
<span data-ttu-id="9c23f-183">Nastavit sestavy zhroutí příliš`false` mezi `<reportCrash>` a `</reportCrash>` značky.</span><span class="sxs-lookup"><span data-stu-id="9c23f-183">Set report crash too`false` between `<reportCrash>` and `</reportCrash>` tags.</span></span>

#### <a name="from-engagementconfiguration-object-at-run-time"></a><span data-ttu-id="9c23f-184">Z `EngagementConfiguration` objektu v době běhu</span><span class="sxs-lookup"><span data-stu-id="9c23f-184">From `EngagementConfiguration` object at run time</span></span>
<span data-ttu-id="9c23f-185">Nastavit toofalse havárií sestavy pomocí EngagementConfiguration objektu.</span><span class="sxs-lookup"><span data-stu-id="9c23f-185">Set report crash toofalse using your EngagementConfiguration object.</span></span>

        /* Engagement configuration. */

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration(); engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        /\* Disable Engagement crash reporting. \*/ engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a><span data-ttu-id="9c23f-186">Režim shluku</span><span class="sxs-lookup"><span data-stu-id="9c23f-186">Burst mode</span></span>
<span data-ttu-id="9c23f-187">Ve výchozím nastavení sestavy služby zapojení hello protokolů v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="9c23f-187">By default, hello Engagement service reports logs in real time.</span></span> <span data-ttu-id="9c23f-188">Pokud vaše aplikace hlásí protokoly velmi často, je lepší toobuffer hello protokoly a tooreport je všechny najednou na pravidelné základní časové (tomu se říká hello "shluků režim").</span><span class="sxs-lookup"><span data-stu-id="9c23f-188">If your application reports logs very frequently, it is better toobuffer hello logs and tooreport them all at once on a regular time base (this is called hello “burst mode”).</span></span>

<span data-ttu-id="9c23f-189">toodo tedy volat metodu hello:</span><span class="sxs-lookup"><span data-stu-id="9c23f-189">toodo so, call hello method:</span></span>

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

<span data-ttu-id="9c23f-190">Hello argument je hodnota **milisekundách**.</span><span class="sxs-lookup"><span data-stu-id="9c23f-190">hello argument is a value in **milliseconds**.</span></span> <span data-ttu-id="9c23f-191">Kdykoli Pokud chcete protokolování v reálném čase hello tooreactivate, stačí zavoláte metoda hello bez zadání parametru nebo s hodnotou hello 0.</span><span class="sxs-lookup"><span data-stu-id="9c23f-191">At any time, if you want tooreactivate hello real-time logging, just call hello method without any parameter, or with hello 0 value.</span></span>

<span data-ttu-id="9c23f-192">Hello shluků režimu mírně zvýšit hello baterie životnosti ale má vliv na hello monitorování Engagement: všechny doba trvání relace a úlohy se prahová hodnota shluků zaokrouhlené toohello (tedy relací a úlohy kratší, než je prahová hodnota shluků hello nemusí být zobrazeny).</span><span class="sxs-lookup"><span data-stu-id="9c23f-192">hello burst mode slightly increase hello battery life but has an impact on hello Engagement Monitor: all sessions and jobs duration will be rounded toohello burst threshold (thus, sessions and jobs shorter than hello burst threshold may not be visible).</span></span> <span data-ttu-id="9c23f-193">Je doporučeno toouse práh shluků než 30000 (30s).</span><span class="sxs-lookup"><span data-stu-id="9c23f-193">It is recommended toouse a burst threshold no longer than 30000 (30s).</span></span> <span data-ttu-id="9c23f-194">Máte toobe vědět, které uložené protokoly jsou omezené too300 položky.</span><span class="sxs-lookup"><span data-stu-id="9c23f-194">You have toobe aware that saved logs are limited too300 items.</span></span> <span data-ttu-id="9c23f-195">Pokud odesílání je příliš dlouhý. může dojít ke ztrátě některých protokoly.</span><span class="sxs-lookup"><span data-stu-id="9c23f-195">If sending is too long you can lose some logs.</span></span>

> [!WARNING]
> <span data-ttu-id="9c23f-196">Hello shluků mezní hodnotu nelze konfigurovat tooa období menší než jedna sekunda.</span><span class="sxs-lookup"><span data-stu-id="9c23f-196">hello burst threshold cannot be configured tooa period lesser than one second.</span></span> <span data-ttu-id="9c23f-197">Pokud se pokusíte toodo tak, hello SDK bude zobrazit trasování s chybou hello a bude automaticky resetovat toohello výchozí hodnota, který je nula sekund.</span><span class="sxs-lookup"><span data-stu-id="9c23f-197">If you try toodo so, hello SDK will show a trace with hello error and will automatically reset toohello default value, that is, zero seconds.</span></span> <span data-ttu-id="9c23f-198">Tím se aktivuje hello SDK tooreport hello protokolů v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="9c23f-198">This will trigger hello SDK tooreport hello logs in real-time.</span></span>
> 
> 

