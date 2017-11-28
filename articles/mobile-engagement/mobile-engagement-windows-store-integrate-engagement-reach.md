---
title: "aaaWindows univerzální aplikace dosáhnout integrace sady SDK"
description: "Jak tooIntegrate Azure Mobile Engagement Reach s univerzálních aplikací pro Windows"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a31ca1d6-856f-4aec-898a-07969ae5f7ec
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: af311c65940014083333853875a00173b8d6783e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="windows-universal-apps-reach-sdk-integration"></a><span data-ttu-id="cd96a-103">Univerzální aplikace Windows dosáhnout integraci sady SDK</span><span class="sxs-lookup"><span data-stu-id="cd96a-103">Windows Universal Apps Reach SDK Integration</span></span>
<span data-ttu-id="cd96a-104">Je třeba postupovat podle hello integrace postup popsaný v hello [integraci sady Windows Universal Engagement SDK](mobile-engagement-windows-store-integrate-engagement.md) před těchto pokynů.</span><span class="sxs-lookup"><span data-stu-id="cd96a-104">You must follow hello integration procedure described in hello [Windows Universal Engagement SDK Integration](mobile-engagement-windows-store-integrate-engagement.md) before following this guide.</span></span>

## <a name="embed-hello-engagement-reach-sdk-into-your-windows-universal-project"></a><span data-ttu-id="cd96a-105">Vložení hello Engagement Reach SDK do projektu univerzální pro Windows</span><span class="sxs-lookup"><span data-stu-id="cd96a-105">Embed hello Engagement Reach SDK into your Windows Universal project</span></span>
<span data-ttu-id="cd96a-106">Nemáte nic tooadd.</span><span class="sxs-lookup"><span data-stu-id="cd96a-106">You do not have anything tooadd.</span></span> <span data-ttu-id="cd96a-107">`EngagementReach`odkazy a prostředky jsou už ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="cd96a-107">`EngagementReach` references and resources are already in your project.</span></span>

> [!TIP]
> <span data-ttu-id="cd96a-108">Můžete přizpůsobit bitové kopie, které jsou umístěné v hello `Resources` složky projektu, zejména hello brand ikona (této výchozí toohello Engagement).</span><span class="sxs-lookup"><span data-stu-id="cd96a-108">You can customize images located in hello `Resources` folder of your project, especially hello brand icon (that default toohello Engagement icon).</span></span> <span data-ttu-id="cd96a-109">Pro univerzální aplikace také můžete přesunout hello `Resources` složky na vaše sdílený projekt tooshare jeho obsah mezi aplikací, ale bude mít tookeep hello `Resources\EngagementConfiguration.xml` souborů na výchozího umístění, jako je platforma závislé.</span><span class="sxs-lookup"><span data-stu-id="cd96a-109">On Universal Apps you can also move hello `Resources` folder on your shared project tooshare its content between apps, but you will have tookeep hello `Resources\EngagementConfiguration.xml` file on its default location as it is platform dependent.</span></span>
> 
> 

## <a name="enable-hello-windows-notification-service"></a><span data-ttu-id="cd96a-110">Povolit hello služby oznámení Windows</span><span class="sxs-lookup"><span data-stu-id="cd96a-110">Enable hello Windows Notification Service</span></span>
### <a name="windows-8x-and-windows-phone-81-only"></a><span data-ttu-id="cd96a-111">Windows 8.x a Windows Phone 8.1 pouze</span><span class="sxs-lookup"><span data-stu-id="cd96a-111">Windows 8.x and Windows Phone 8.1 only</span></span>
<span data-ttu-id="cd96a-112">V pořadí toouse hello **služby oznámení Windows** (označované jako WNS) ve vaší `Package.appxmanifest` souborů na `Application UI` klikněte na `All Image Assets` pole levé robota hello.</span><span class="sxs-lookup"><span data-stu-id="cd96a-112">In order toouse hello **Windows Notification Service** (referred as WNS) in your `Package.appxmanifest` file on `Application UI` click on `All Image Assets` in hello left bot box.</span></span> <span data-ttu-id="cd96a-113">V hello napravo od hello `Notifications`, změňte `toast capable` z `(not set)` příliš`(Yes)`.</span><span class="sxs-lookup"><span data-stu-id="cd96a-113">At hello right of hello box in `Notifications`, change `toast capable` from `(not set)` too`(Yes)`.</span></span>

### <a name="all-platforms"></a><span data-ttu-id="cd96a-114">Všechny platformy</span><span class="sxs-lookup"><span data-stu-id="cd96a-114">All platforms</span></span>
<span data-ttu-id="cd96a-115">Je nutné toosynchronize vaší aplikace tooyour Microsoft účet a toohello platforma pro zapojení.</span><span class="sxs-lookup"><span data-stu-id="cd96a-115">You need toosynchronize your app tooyour Microsoft account and toohello engagement platform.</span></span> <span data-ttu-id="cd96a-116">Pro tuto potřebovat toocreate účet nebo přihlásit [Centrum vývojářů pro windows](https://dev.windows.com).</span><span class="sxs-lookup"><span data-stu-id="cd96a-116">For this you need toocreate an account or log on [windows dev center](https://dev.windows.com).</span></span> <span data-ttu-id="cd96a-117">Po, vytvořte novou aplikaci a najít hello SID a tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="cd96a-117">After that create a new application and find hello SID and secret key.</span></span> <span data-ttu-id="cd96a-118">Na hello engagement front-endu, přejděte na nastavení vaší aplikace v `native push` a vložte svoje přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="cd96a-118">On hello engagement frontend, go on your app setting in `native push` and paste your credentials.</span></span> <span data-ttu-id="cd96a-119">Potom klikněte pravým tlačítkem na projekt, vyberte `store` a `Associate App with hello Store...`.</span><span class="sxs-lookup"><span data-stu-id="cd96a-119">After that, right click on your project, select `store` and `Associate App with hello Store...`.</span></span> <span data-ttu-id="cd96a-120">Stačí tooselect hello aplikace musíte mít před toosynchronize ji vytvořit.</span><span class="sxs-lookup"><span data-stu-id="cd96a-120">You just need tooselect hello application you have create before toosynchronize it.</span></span>

## <a name="initialize-hello-engagement-reach-sdk"></a><span data-ttu-id="cd96a-121">Inicializace hello Engagement Reach SDK</span><span class="sxs-lookup"><span data-stu-id="cd96a-121">Initialize hello Engagement Reach SDK</span></span>
<span data-ttu-id="cd96a-122">Upravit hello `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="cd96a-122">Modify hello `App.xaml.cs`:</span></span>

* <span data-ttu-id="cd96a-123">Vložit `EngagementReach.Instance.Init` právě po `EngagementAgent.Instance.Init` ve vaší `InitEngagement` metoda:</span><span class="sxs-lookup"><span data-stu-id="cd96a-123">Insert `EngagementReach.Instance.Init` just after `EngagementAgent.Instance.Init` in your `InitEngagement` method:</span></span>
  
      private void InitEngagement(IActivatedEventArgs e)
      {
        EngagementAgent.Instance.Init(e);
        EngagementReach.Instance.Init(e);
      }
  
  <span data-ttu-id="cd96a-124">Hello `EngagementReach.Instance.Init` běží ve vyhrazené vlákno.</span><span class="sxs-lookup"><span data-stu-id="cd96a-124">hello `EngagementReach.Instance.Init` runs in a dedicated thread.</span></span> <span data-ttu-id="cd96a-125">Nemáte toodo ho sami.</span><span class="sxs-lookup"><span data-stu-id="cd96a-125">You do not have toodo it yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="cd96a-126">Pokud používáte nabízená oznámení jinde v aplikaci, pak máte příliš[sdílet kanál nabízené](#push-channel-sharing) s Engagement Reach.</span><span class="sxs-lookup"><span data-stu-id="cd96a-126">If you are using push notifications elsewhere in your application then you have too[share your push channel](#push-channel-sharing) with Engagement Reach.</span></span>
> 
> 

## <a name="integration"></a><span data-ttu-id="cd96a-127">Integrace</span><span class="sxs-lookup"><span data-stu-id="cd96a-127">Integration</span></span>
<span data-ttu-id="cd96a-128">Engagement nabízí dva způsoby, jak tooadd hello Reach v aplikaci plakáty a vkládaná zobrazení pro oznámení a dotazuje se ve vaší aplikaci: hello překrytí, integrace a hello webové zobrazení ruční integrace.</span><span class="sxs-lookup"><span data-stu-id="cd96a-128">Engagement provides two ways tooadd hello Reach in-app banners and interstitial views for announcements and polls in your application: hello overlay integration and hello web views manual integration.</span></span> <span data-ttu-id="cd96a-129">Nesmí kombinovat obě přístup na hello stejné stránce.</span><span class="sxs-lookup"><span data-stu-id="cd96a-129">You should not combine both approach on hello same page.</span></span>

<span data-ttu-id="cd96a-130">Volba Hello mezi dvěma integrace hello může shrnout takto:</span><span class="sxs-lookup"><span data-stu-id="cd96a-130">hello choice between hello two integration could be summarized this way:</span></span>

* <span data-ttu-id="cd96a-131">Můžete rozhodnout integrace překrytí hello, pokud vaše stránky již dědí z hello agenta `EngagementPage`, bude stačit nahrazení `EngagementPage` podle `EngagementPageOverlay` a `xmlns:engagement="using:Microsoft.Azure.Engagement"` podle `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` na stránkách.</span><span class="sxs-lookup"><span data-stu-id="cd96a-131">You may choose hello overlay integration if your pages already inherits from hello Agent `EngagementPage`, it's just a matter of replacing `EngagementPage` by `EngagementPageOverlay` and `xmlns:engagement="using:Microsoft.Azure.Engagement"` by `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` in your pages.</span></span>
* <span data-ttu-id="cd96a-132">Můžete se rozhodnout hello webové zobrazení ruční integrace Pokud chcete tooprecisely místní hello dosáhnout uživatelského rozhraní v rámci vaší stránky, nebo pokud nechcete, aby tooadd jiné stránky úrovně tooyour dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="cd96a-132">You may choose hello web views manual integration if you want tooprecisely place hello Reach UI within your pages or if you don't want tooadd another inheritance level tooyour pages.</span></span> 

### <a name="overlay-integration"></a><span data-ttu-id="cd96a-133">Integrace překrytí</span><span class="sxs-lookup"><span data-stu-id="cd96a-133">Overlay integration</span></span>
<span data-ttu-id="cd96a-134">Hello Engagement překrytí dynamicky přidá prvky uživatelského rozhraní hello použitou kampaně Reach toodisplay v stránku.</span><span class="sxs-lookup"><span data-stu-id="cd96a-134">hello Engagement overlay dynamically adds hello UI elements used toodisplay Reach campaigns in your page.</span></span> <span data-ttu-id="cd96a-135">Pokud není hello překrytí vyhovovaly vaší rozložení byste měli zvážit hello webové zobrazení ruční integrace místo.</span><span class="sxs-lookup"><span data-stu-id="cd96a-135">If hello overlay doesn't suit your layout you should consider hello web views manual integration instead.</span></span>

<span data-ttu-id="cd96a-136">V souboru změny XAML `EngagementPage` příliš reference`EngagementPageOverlay`</span><span class="sxs-lookup"><span data-stu-id="cd96a-136">In your .xaml file change `EngagementPage` reference too`EngagementPageOverlay`</span></span>

* <span data-ttu-id="cd96a-137">Přidejte tooyour deklarací oborů názvů:</span><span class="sxs-lookup"><span data-stu-id="cd96a-137">Add tooyour namespaces declarations:</span></span>
  
      xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"
* <span data-ttu-id="cd96a-138">Nahraďte `engagement:EngagementPage` s `engagement:EngagementPageOverlay`:</span><span class="sxs-lookup"><span data-stu-id="cd96a-138">Replace `engagement:EngagementPage` with `engagement:EngagementPageOverlay`:</span></span>

<span data-ttu-id="cd96a-139">**S EngagementPage:**</span><span class="sxs-lookup"><span data-stu-id="cd96a-139">**With EngagementPage:**</span></span>

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">

            <!-- Your layout -->
        </engagement:EngagementPage>

<span data-ttu-id="cd96a-140">**S EngagementPageOverlay:**</span><span class="sxs-lookup"><span data-stu-id="cd96a-140">**With EngagementPageOverlay:**</span></span>

        <engagement:EngagementPageOverlay 
            xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay">

            <!-- Your layout -->
        </engagement:EngagementPageOverlay>

<span data-ttu-id="cd96a-141">V souboru .cs značky stránku v `EngagementPageOverlay` místo `EngagementPage` a import `Microsoft.Azure.Engagement.Overlay`.</span><span class="sxs-lookup"><span data-stu-id="cd96a-141">Then in your .cs file tag your page in `EngagementPageOverlay` instead of `EngagementPage` and import `Microsoft.Azure.Engagement.Overlay`.</span></span>

            using Microsoft.Azure.Engagement.Overlay;

* <span data-ttu-id="cd96a-142">Nahraďte `EngagementPage` s `EngagementPageOverlay`:</span><span class="sxs-lookup"><span data-stu-id="cd96a-142">Replace `EngagementPage` with `EngagementPageOverlay`:</span></span>

<span data-ttu-id="cd96a-143">**S EngagementPage:**</span><span class="sxs-lookup"><span data-stu-id="cd96a-143">**With EngagementPage:**</span></span>

            using Microsoft.Azure.Engagement;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPage
              {
                [...]
              }
            }

<span data-ttu-id="cd96a-144">**S EngagementPageOverlay:**</span><span class="sxs-lookup"><span data-stu-id="cd96a-144">**With EngagementPageOverlay:**</span></span>

            using Microsoft.Azure.Engagement.Overlay;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPageOverlay 
              {
                [...]
              }
            }


<span data-ttu-id="cd96a-145">Přidá technologie Hello Engagement překrytí `Grid` elementu na stránku tvořený rozložení a hello dva `WebView` prvky jeden pro hello banner a hello další jeden pro zobrazení vkládaná hello.</span><span class="sxs-lookup"><span data-stu-id="cd96a-145">hello Engagement overlay adds a `Grid` element on top of your page composed of your layout and hello two `WebView` elements one for hello banner and hello other one for hello interstitial view.</span></span>

<span data-ttu-id="cd96a-146">Můžete přizpůsobit hello překrytí elementů přímo v hello `EngagementPageOverlay.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="cd96a-146">You can customize hello overlay elements directly in hello `EngagementPageOverlay.cs` file.</span></span>

### <a name="web-views-manual-integration"></a><span data-ttu-id="cd96a-147">Webové zobrazení ruční integrace</span><span class="sxs-lookup"><span data-stu-id="cd96a-147">Web views manual integration</span></span>
<span data-ttu-id="cd96a-148">Reach bude hledání svoje stránky pro hello dva `WebView` elementy odpovědná za zobrazování hello banner a hello vkládaná zobrazení.</span><span class="sxs-lookup"><span data-stu-id="cd96a-148">Reach will be searching your pages for hello two `WebView` elements responsible for displaying hello banner and hello interstitial view.</span></span> <span data-ttu-id="cd96a-149">Dobrý den, co máte toodo je tooadd pouze tyto dvě `WebView` elementy někde na stránkách, tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="cd96a-149">hello only thing you have toodo is tooadd those two `WebView` elements somewhere in your pages, here is an example:</span></span>

    <Grid x:Name="engagementGrid">

      <!-- Your layout -->

      <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Stretch" VerticalAlignment="Top"/>
      <WebView x:Name="engagement_announcement_content" Visibility="Collapsed"  HorizontalAlignment="Stretch"  VerticalAlignment="Stretch"/>
    </Grid>


<span data-ttu-id="cd96a-150">V tento příklad hello `WebView` elementy jsou roztažené toofit jejich kontejneru, který automaticky je znovu velikostí v případě změna velikosti obrazovky otočení nebo okna.</span><span class="sxs-lookup"><span data-stu-id="cd96a-150">In this example hello `WebView` elements are stretched toofit their container which automatically re-sizes them in case of screen rotation or window size change.</span></span>

> [!WARNING]
> <span data-ttu-id="cd96a-151">Je důležité tookeep hello stejné názvy `engagement_notification_content` a `engagement_announcement_content` pro hello `WebView` elementy.</span><span class="sxs-lookup"><span data-stu-id="cd96a-151">It is important tookeep hello same naming `engagement_notification_content` and `engagement_announcement_content` for hello `WebView` elements.</span></span> <span data-ttu-id="cd96a-152">Reach je identifikace je podle názvu.</span><span class="sxs-lookup"><span data-stu-id="cd96a-152">Reach is identifying them by their name.</span></span> 
> 
> 

## <a name="handle-datapush-optional"></a><span data-ttu-id="cd96a-153">Popisovač datapush (volitelné)</span><span class="sxs-lookup"><span data-stu-id="cd96a-153">Handle datapush (optional)</span></span>
<span data-ttu-id="cd96a-154">Pokud chcete, aby vaše aplikace toobe možné tooreceive Reach datová oznámení, máte dvě události tooimplement hello EngagementReach třídy:</span><span class="sxs-lookup"><span data-stu-id="cd96a-154">If you want your application toobe able tooreceive Reach data pushes, you have tooimplement two events of hello EngagementReach class:</span></span>

<span data-ttu-id="cd96a-155">V souboru App.xaml.cs v konstruktoru App() hello přidejte:</span><span class="sxs-lookup"><span data-stu-id="cd96a-155">In App.xaml.cs in hello App() constructor add:</span></span>

            EngagementReach.Instance.DataPushStringReceived += (body) =>
            {
              Debug.WriteLine("String data push message received: " + body);
              return true;
            };

            EngagementReach.Instance.DataPushBase64Received += (decodedBody, encodedBody) =>
            {
              Debug.WriteLine("Base64 data push message received: " + encodedBody);
              // Do something useful with decodedBody like updating an image view
              return true;
            };

<span data-ttu-id="cd96a-156">Uvidíte, že hello zpětné volání z každé metoda vrátí logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cd96a-156">You can see that hello callback of each method returns a boolean.</span></span> <span data-ttu-id="cd96a-157">Zapojení odešle zpětné vazby tooits back-end po odeslání hello datová oznámení.</span><span class="sxs-lookup"><span data-stu-id="cd96a-157">Engagement sends a feedback tooits back-end after dispatching hello data push.</span></span> <span data-ttu-id="cd96a-158">Pokud zpětné volání hello vrací hodnotu false, hello `exit` zpětné vazby bude odesílat.</span><span class="sxs-lookup"><span data-stu-id="cd96a-158">If hello callback returns false, hello `exit` feedback will be send.</span></span> <span data-ttu-id="cd96a-159">Jinak bude `action`.</span><span class="sxs-lookup"><span data-stu-id="cd96a-159">Otherwise, it will be `action`.</span></span> <span data-ttu-id="cd96a-160">Pokud je pro události hello nastavené žádné zpětné volání, hello `drop` tooEngagement bude vrácen zpětnou vazbu.</span><span class="sxs-lookup"><span data-stu-id="cd96a-160">If no callback is set for hello events, hello `drop` feedback will be returned tooEngagement.</span></span>

> [!WARNING]
> <span data-ttu-id="cd96a-161">Zapojení není možné tooreceive násobky názory pro datová oznámení.</span><span class="sxs-lookup"><span data-stu-id="cd96a-161">Engagement is not able tooreceive multiples feedbacks for a data push.</span></span> <span data-ttu-id="cd96a-162">Pokud máte v plánu tooset několik obslužné rutiny na události, mějte na paměti, že zpětnou vazbu hello bude odpovídat toohello naposledy odeslána.</span><span class="sxs-lookup"><span data-stu-id="cd96a-162">If you plan tooset several handlers on an event, be aware that hello feedback will correspond toohello last one sent.</span></span> <span data-ttu-id="cd96a-163">V takovém případě doporučujeme tooalways vrátí hello stejnou hodnotu tooavoid s matoucí zpětnou vazbu o hello front-endu.</span><span class="sxs-lookup"><span data-stu-id="cd96a-163">In this case, we recommend tooalways returns hello same value tooavoid having confusing feedback on hello front-end.</span></span>
> 
> 

## <a name="customize-ui-optional"></a><span data-ttu-id="cd96a-164">Přizpůsobení uživatelského rozhraní (volitelné)</span><span class="sxs-lookup"><span data-stu-id="cd96a-164">Customize UI (optional)</span></span>
### <a name="first-step"></a><span data-ttu-id="cd96a-165">Prvním krokem</span><span class="sxs-lookup"><span data-stu-id="cd96a-165">First step</span></span>
<span data-ttu-id="cd96a-166">Můžeme vám umožňují toocustomize hello reach uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cd96a-166">We allow you toocustomize hello reach UI.</span></span>

<span data-ttu-id="cd96a-167">toodo tedy máte toocreate, podtřídou třídy hello `EngagementReachHandler` třídy.</span><span class="sxs-lookup"><span data-stu-id="cd96a-167">toodo so, you have toocreate a subclass of hello `EngagementReachHandler` class.</span></span>

<span data-ttu-id="cd96a-168">**Ukázkový kód:**</span><span class="sxs-lookup"><span data-stu-id="cd96a-168">**Sample Code :**</span></span>

            using Microsoft.Azure.Engagement;

            namespace Example
            {
              internal class ExampleReachHandler : EngagementReachHandler
              {
               // Override EngagementReachHandler methods depending on your needs
              }
            }

<span data-ttu-id="cd96a-169">Potom nastavte hello obsah hello `EngagementReach.Instance.Handler` pole s vaší vlastní objekt v vaše `App.xaml.cs` třídu v rámci hello `App()` metoda.</span><span class="sxs-lookup"><span data-stu-id="cd96a-169">Then, set hello content of hello `EngagementReach.Instance.Handler` field with your custom object in your `App.xaml.cs` class within hello `App()` method.</span></span>

<span data-ttu-id="cd96a-170">**Ukázkový kód:**</span><span class="sxs-lookup"><span data-stu-id="cd96a-170">**Sample Code :**</span></span>

            protected override void OnLaunched(LaunchActivatedEventArgs args)
            {
              // your app initialization 
              EngagementReach.Instance.Handler = new ExampleReachHandler();
              // Engagement Agent and Reach initialization
            }

> [!NOTE]
> <span data-ttu-id="cd96a-171">Ve výchozím nastavení používá Engagement jejich vlastní implementaci `EngagementReachHandler`.</span><span class="sxs-lookup"><span data-stu-id="cd96a-171">By default, Engagement uses its own implementation of `EngagementReachHandler`.</span></span>
> <span data-ttu-id="cd96a-172">Nemáte toocreate vlastní, a pokud tak učiníte, nemáte toooverride každou metodu.</span><span class="sxs-lookup"><span data-stu-id="cd96a-172">You don't have toocreate your own, and if you do so, you don't have toooverride every method.</span></span> <span data-ttu-id="cd96a-173">Hello výchozí chování je základní objekt Engagement tooselect hello.</span><span class="sxs-lookup"><span data-stu-id="cd96a-173">hello default behavior is tooselect hello Engagement base object.</span></span>
> 
> 

### <a name="web-view"></a><span data-ttu-id="cd96a-174">Webové zobrazení</span><span class="sxs-lookup"><span data-stu-id="cd96a-174">Web View</span></span>
<span data-ttu-id="cd96a-175">Ve výchozím nastavení použije Reach hello vložených prostředků hello DLL toodisplay hello oznámení a stránky.</span><span class="sxs-lookup"><span data-stu-id="cd96a-175">By default, Reach will use hello embedded resources of hello DLL toodisplay hello notifications and pages.</span></span>

<span data-ttu-id="cd96a-176">tooprovide úplné možnosti přizpůsobení používáme pouze zobrazení webové stránky.</span><span class="sxs-lookup"><span data-stu-id="cd96a-176">tooprovide a full customization possibilities we only use web view.</span></span> <span data-ttu-id="cd96a-177">Pokud chcete toocustomize rozložení, přepsat přímo soubory zdrojů hello `EngagementAnnouncement.html` a `EngagementNotification.html`.</span><span class="sxs-lookup"><span data-stu-id="cd96a-177">If you want toocustomize layouts, override directly hello resources files `EngagementAnnouncement.html` and `EngagementNotification.html`.</span></span> <span data-ttu-id="cd96a-178">Zapojení musí všechny kód v `<body></body>` toorun správně.</span><span class="sxs-lookup"><span data-stu-id="cd96a-178">Engagement needs all code in `<body></body>` toorun correctly.</span></span> <span data-ttu-id="cd96a-179">Ale můžete přidat značky vnější `engagement_webview_area`.</span><span class="sxs-lookup"><span data-stu-id="cd96a-179">But you can add tag outer `engagement_webview_area`.</span></span>

<span data-ttu-id="cd96a-180">Můžete však rozhodnout toouse vaše vlastní prostředky.</span><span class="sxs-lookup"><span data-stu-id="cd96a-180">However, you can decide toouse your own resources.</span></span>

<span data-ttu-id="cd96a-181">Můžete přepsat `EngagementReachHandler` metody ve vaší podtřídami tootell Engagement toouse rozložení, ale trvat pozor tooembedded hello engagement mechanismus:</span><span class="sxs-lookup"><span data-stu-id="cd96a-181">You can override `EngagementReachHandler` methods in your subclass tootell Engagement toouse your layouts, but take care tooembedded hello engagement mechanism:</span></span>

<span data-ttu-id="cd96a-182">**Ukázkový kód:**</span><span class="sxs-lookup"><span data-stu-id="cd96a-182">**Sample Code :**</span></span>

            // In your subclass of EngagementReachHandler

            public override string GetAnnouncementHTML()
            {
              return base.GetAnnouncementHTML();
            }
            public override string GetAnnouncementName()
            {
              return base.GetAnnouncementName();
            }
            public override string GetNotfificationHTML()
            {
              return base.GetNotfificationHTML();
            }
            public override string GetNotfificationName()
            {
              return base.GetNotfificationName();
            }


<span data-ttu-id="cd96a-183">Ve výchozím nastavení je AnnouncementHTML `ms-appx-web:///Resources/EngagementAnnouncement.html`.</span><span class="sxs-lookup"><span data-stu-id="cd96a-183">By default, AnnouncementHTML is `ms-appx-web:///Resources/EngagementAnnouncement.html`.</span></span> <span data-ttu-id="cd96a-184">Reprezentuje hello souboru html, který návrh hello obsah nabízená zpráva (Text oznámení, webové anoucement a dotazování oznámení).</span><span class="sxs-lookup"><span data-stu-id="cd96a-184">It represents hello html file that design hello content of a push message (Text announcement, Web anoucement and Poll announcement).</span></span> <span data-ttu-id="cd96a-185">Je AnnouncementName `engagement_announcement_content`.</span><span class="sxs-lookup"><span data-stu-id="cd96a-185">AnnouncementName is `engagement_announcement_content`.</span></span> <span data-ttu-id="cd96a-186">Je název hello hello webové zobrazení návrhu v stránku xaml.</span><span class="sxs-lookup"><span data-stu-id="cd96a-186">It is hello name of hello webview design in your xaml page.</span></span>

<span data-ttu-id="cd96a-187">Je NotfificationHTML `ms-appx-web:///Resources/EngagementNotification.html`.</span><span class="sxs-lookup"><span data-stu-id="cd96a-187">NotfificationHTML is `ms-appx-web:///Resources/EngagementNotification.html`.</span></span> <span data-ttu-id="cd96a-188">Reprezentuje hello souboru html, který návrh hello oznámení o nabízené zprávy.</span><span class="sxs-lookup"><span data-stu-id="cd96a-188">It represents hello html file that design hello notification of a push message.</span></span> <span data-ttu-id="cd96a-189">Je NotfificationName `engagement_notification_content`.</span><span class="sxs-lookup"><span data-stu-id="cd96a-189">NotfificationName is `engagement_notification_content`.</span></span> <span data-ttu-id="cd96a-190">Je název hello hello webové zobrazení návrhu v stránku xaml.</span><span class="sxs-lookup"><span data-stu-id="cd96a-190">It is hello name of hello webview design in your xaml page.</span></span>

### <a name="customization"></a><span data-ttu-id="cd96a-191">Přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="cd96a-191">Customization</span></span>
<span data-ttu-id="cd96a-192">Můžete přizpůsobit oznámení a oznámení, že má webové zobrazení, je-li zachovat Engagement objektu.</span><span class="sxs-lookup"><span data-stu-id="cd96a-192">You can customize notification and announcement web view has you want if you preserve Engagement object.</span></span> <span data-ttu-id="cd96a-193">Ujistěte se, že objekt webové zobrazení je popsáno třikrát – hello poprvé ve vašem jazyce xaml, druhý čas v souboru .cs v metodě "setwebview()" hello a třetí čas v souboru html hello.</span><span class="sxs-lookup"><span data-stu-id="cd96a-193">Take care that webview object is described three times - hello first time in your xaml, second time in your .cs file in hello "setwebview()" method, and third time in hello html file.</span></span>

* <span data-ttu-id="cd96a-194">Ve vašem xaml popisují hello aktuální grafické rozložení webového zobrazení součásti.</span><span class="sxs-lookup"><span data-stu-id="cd96a-194">In your xaml you describe hello current graphical layout webview component.</span></span>
* <span data-ttu-id="cd96a-195">V souboru .cs můžete definovat "setwebview()", ve kterém můžete nastavit hello dimenze hello dva webové zobrazení (oznámení, oznámení).</span><span class="sxs-lookup"><span data-stu-id="cd96a-195">In your .cs file you can define "setwebview()" in which you set hello dimension of hello two webview (notification, announcement).</span></span> <span data-ttu-id="cd96a-196">Je velmi účinné při aplikace hello je změna velikosti.</span><span class="sxs-lookup"><span data-stu-id="cd96a-196">It is very effective when hello application is resizing.</span></span>
* <span data-ttu-id="cd96a-197">V souboru html Engagement hello jsme popisují hello webové zobrazení obsahu, návrhu a hello pozic prvky mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="cd96a-197">In hello Engagement html file we describe hello webview content, design and hello elements positions between each other.</span></span>

### <a name="launch-message"></a><span data-ttu-id="cd96a-198">Spusťte zprávu</span><span class="sxs-lookup"><span data-stu-id="cd96a-198">Launch message</span></span>
<span data-ttu-id="cd96a-199">Když uživatel klikne na systémové oznámení (oznámení), Engagement spustí aplikace hello, obsah hello zatížení hello nabízené zprávy a zobrazit stránku hello hello odpovídající kampaně.</span><span class="sxs-lookup"><span data-stu-id="cd96a-199">When a user clicks on a system notification (a toast), Engagement launches hello application, load hello content of hello push messages, and display hello page for hello corresponding campaign.</span></span>

<span data-ttu-id="cd96a-200">Dochází ke zpoždění mezi hello spuštění aplikace a hello zobrazení hello hello stránky (v závislosti na rychlosti sítě hello).</span><span class="sxs-lookup"><span data-stu-id="cd96a-200">There is a delay between hello launch of hello application and hello display of hello page (depending on hello speed of your network).</span></span>

<span data-ttu-id="cd96a-201">tooindicate toohello uživatele, který se načítá něco, by měl poskytovat vizuální informace, jako je indikátor průběhu nebo indikátor průběhu.</span><span class="sxs-lookup"><span data-stu-id="cd96a-201">tooindicate toohello user that something is loading, you should provide a visual information, like a progress bar or a progress indicator.</span></span> <span data-ttu-id="cd96a-202">Zapojení nemůže zpracovat samostatně, ale poskytuje několik obslužné rutiny pro vás.</span><span class="sxs-lookup"><span data-stu-id="cd96a-202">Engagement cannot handle that itself, but provides a few handlers for you.</span></span>

<span data-ttu-id="cd96a-203">Přidejte tooimplement hello zpětného volání v souboru App.xaml.cs v "Veřejné App() {}":</span><span class="sxs-lookup"><span data-stu-id="cd96a-203">tooimplement hello callback, in App.xaml.cs in "Public App(){}" add:</span></span>

            /* hello application has launched and hello content is loading.
             * You should display an indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };

            /* hello application has finished loading hello content and hello page
             * is about toobe displayed.
             * You should hide hello indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };

            /* hello content has been loaded, but an error has occurred.
             * You can provide an information toohello user.
             * You should hide hello indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

<span data-ttu-id="cd96a-204">Zpětné volání hello můžete nastavit vaše metoda "Veřejné App() {}" vaší `App.xaml.cs` soubor, pokud možno před hello `EngagementReach.Instance.Init()` volání.</span><span class="sxs-lookup"><span data-stu-id="cd96a-204">You can set hello callback in your "Public App(){}" method of your `App.xaml.cs` file, preferably before hello `EngagementReach.Instance.Init()` call.</span></span>

> [!TIP]
> <span data-ttu-id="cd96a-205">Každý obslužná rutina je volána službou hello vlákna uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cd96a-205">Each handler is called by hello UI Thread.</span></span> <span data-ttu-id="cd96a-206">Při použití a MessageBox nebo něco uživatelského rozhraní související nemáte tooworry.</span><span class="sxs-lookup"><span data-stu-id="cd96a-206">You do not have tooworry when using a MessageBox or something UI-related.</span></span>
> 
> 

## <span data-ttu-id="cd96a-207"><a id="push-channel-sharing"></a>Push sdílení kanálu</span><span class="sxs-lookup"><span data-stu-id="cd96a-207"><a id="push-channel-sharing"></a> Push channel sharing</span></span>
<span data-ttu-id="cd96a-208">Pokud používáte nabízená oznámení pro jiný účel ve vaší aplikaci máte toouse hello nabízené kanál sdílení funkce hello Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="cd96a-208">If you are using push notifications for another purpose in your application then you have toouse hello push channel sharing feature of hello Engagement SDK.</span></span> <span data-ttu-id="cd96a-209">Toto je nabízené tooavoid vynechán.</span><span class="sxs-lookup"><span data-stu-id="cd96a-209">This is tooavoid missed push.</span></span>

* <span data-ttu-id="cd96a-210">Můžete zadat vlastní nabízené kanál toohello Engagement Reach inicializace.</span><span class="sxs-lookup"><span data-stu-id="cd96a-210">You can provide your own push channel toohello Engagement Reach initialization.</span></span> <span data-ttu-id="cd96a-211">Hello SDK použije místo požaduje novou.</span><span class="sxs-lookup"><span data-stu-id="cd96a-211">hello SDK will use it instead of requesting a new one.</span></span>

<span data-ttu-id="cd96a-212">Aktualizace hello Engagement Reach inicializace pomocí nabízených kanál v hello `InitEngagement` metoda z hello `App.xaml.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="cd96a-212">Update hello Engagement Reach initialization with your push channel in hello `InitEngagement` method from hello `App.xaml.cs` file:</span></span>

    /* Your own push channel logic... */
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    /*...Engagement initialization */
    EngagementAgent.Instance.Init(e);
    EngagementReach.Instance.Init(e,pushChannel);

* <span data-ttu-id="cd96a-213">Případně pokud chcete tooconsume hello push kanál po inicializaci Reach hello pak můžete nastavit zpětné volání na Engagement Reach tooget hello nabízené kanálu po jejím vytvoření hello SDK.</span><span class="sxs-lookup"><span data-stu-id="cd96a-213">Alternatively, if you just want tooconsume hello push channel after hello Reach initialization then you can set a callback on Engagement Reach tooget hello push channel once it is created by hello SDK.</span></span>

<span data-ttu-id="cd96a-214">Nastavit vaše zpětné volání v jakémkoli místě **po** hello Reach inicializace:</span><span class="sxs-lookup"><span data-stu-id="cd96a-214">Set your callback at any place **after** hello Reach initialization :</span></span>

    /* Set action on hello SDK push channel. */
    EngagementReach.Instance.SetActionOnPushChannel((PushNotificationChannel channel) => 
    {
      /* hello forwarded channel can be null if its creation fails for any reason. */
      if (channel != null)
      {
        /* Your own push channel logic... */
      });
    }

## <a name="custom-scheme-tip"></a><span data-ttu-id="cd96a-215">Tip pro vlastní schéma</span><span class="sxs-lookup"><span data-stu-id="cd96a-215">Custom scheme tip</span></span>
<span data-ttu-id="cd96a-216">Poskytujeme použití vlastní schéma.</span><span class="sxs-lookup"><span data-stu-id="cd96a-216">We provide custom scheme use.</span></span> <span data-ttu-id="cd96a-217">Můžete odeslat jiný typ identifikátoru URI z toobe front-endu zapojení použít v aplikaci zapojení.</span><span class="sxs-lookup"><span data-stu-id="cd96a-217">You can send different type of URI from engagement frontend toobe used in your engagement application.</span></span> <span data-ttu-id="cd96a-218">Výchozí schéma jako `http, ftp, ...` jsou spravovat v systému Windows, bude okno řádku by šlo žádná výchozí aplikace nainstalované v zařízení.</span><span class="sxs-lookup"><span data-stu-id="cd96a-218">Default scheme like `http, ftp, ...` are manage by Windows, a window will prompt if they are no default application installed on device.</span></span> <span data-ttu-id="cd96a-219">Můžete také vytvořit své vlastní schéma pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cd96a-219">You can also create your own custom scheme for your application.</span></span>

<span data-ttu-id="cd96a-220">Hello jednoduchý způsob tooset vlastní schéma v aplikaci je tooopen vaší `Package.appxmanifest` přejděte v `Declarations` panelu.</span><span class="sxs-lookup"><span data-stu-id="cd96a-220">hello simple way tooset a custom scheme in your application is tooopen your `Package.appxmanifest` go in `Declarations` panel.</span></span> <span data-ttu-id="cd96a-221">Vyberte `Protocol` v hello posuňte se k dispozici deklarace pole a přidejte ji.</span><span class="sxs-lookup"><span data-stu-id="cd96a-221">Select `Protocol` in hello Available Declarations scroll box and add it.</span></span> <span data-ttu-id="cd96a-222">Upravit hello `Name` pole s nový protokol požadovaný název.</span><span class="sxs-lookup"><span data-stu-id="cd96a-222">Edit hello `Name` field with your new protocol desired name.</span></span>

<span data-ttu-id="cd96a-223">Nyní toouse tento protokol upravit vaše `App.xaml.cs` s hello `OnActivated` metoda a nezapomeňte také zde tooinitialize engagement:</span><span class="sxs-lookup"><span data-stu-id="cd96a-223">Now toouse this protocol, edit your `App.xaml.cs` with hello `OnActivated` method, and don't forget tooinitialize engagement here also:</span></span>

            /// <summary>
            /// Enter point when app his called by another way than user click
            /// </summary>
            /// <param name="args">Activation args</param>
            protected override void OnActivated(IActivatedEventArgs args)
            {
              /* Init engagement like it was launch by a custom uri scheme */
              EngagementAgent.Instance.Init(args);
              EngagementReach.Instance.Init(args);

              //TODO design action toodo when app is launch

              #region Custom scheme use
              if (args.Kind == ActivationKind.Protocol)
              {
                ProtocolActivatedEventArgs myProtocol = (ProtocolActivatedEventArgs)args;

                if (myProtocol.Uri.Scheme.Equals("protocolName"))
                {
                  string path = myProtocol.Uri.AbsolutePath;
                }
              }
              #endregion

