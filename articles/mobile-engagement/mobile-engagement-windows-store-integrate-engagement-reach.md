---
title: "Univerzální aplikace Windows dosáhnout integraci sady SDK"
description: "Postup pro integraci Azure Mobile Engagement Reach univerzálních aplikací pro Windows"
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
ms.openlocfilehash: 9311e998e67d8d0d56da68fc9460df32ce7ce5a9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="windows-universal-apps-reach-sdk-integration"></a><span data-ttu-id="c1c6e-103">Univerzální aplikace Windows dosáhnout integraci sady SDK</span><span class="sxs-lookup"><span data-stu-id="c1c6e-103">Windows Universal Apps Reach SDK Integration</span></span>
<span data-ttu-id="c1c6e-104">Je třeba provést postup integrace popsaný v tématu [integraci sady Windows Universal Engagement SDK](mobile-engagement-windows-store-integrate-engagement.md) před těchto pokynů.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-104">You must follow the integration procedure described in the [Windows Universal Engagement SDK Integration](mobile-engagement-windows-store-integrate-engagement.md) before following this guide.</span></span>

## <a name="embed-the-engagement-reach-sdk-into-your-windows-universal-project"></a><span data-ttu-id="c1c6e-105">Vložení Engagement Reach SDK do projektu univerzální pro Windows</span><span class="sxs-lookup"><span data-stu-id="c1c6e-105">Embed the Engagement Reach SDK into your Windows Universal project</span></span>
<span data-ttu-id="c1c6e-106">Nemáte nic přidat.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-106">You do not have anything to add.</span></span> <span data-ttu-id="c1c6e-107">`EngagementReach`odkazy a prostředky jsou už ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-107">`EngagementReach` references and resources are already in your project.</span></span>

> [!TIP]
> <span data-ttu-id="c1c6e-108">Můžete přizpůsobit bitové kopie, které jsou umístěné v `Resources` složky projektu, zejména ikonu značky (této výchozí ikonu Engagement).</span><span class="sxs-lookup"><span data-stu-id="c1c6e-108">You can customize images located in the `Resources` folder of your project, especially the brand icon (that default to the Engagement icon).</span></span> <span data-ttu-id="c1c6e-109">Pro univerzální aplikace také můžete přesunout `Resources` složky v projektu sdíleného sdílet jeho obsah mezi aplikací, ale bude třeba ponechat `Resources\EngagementConfiguration.xml` souborů na výchozího umístění, jako je platforma závislé.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-109">On Universal Apps you can also move the `Resources` folder on your shared project to share its content between apps, but you will have to keep the `Resources\EngagementConfiguration.xml` file on its default location as it is platform dependent.</span></span>
> 
> 

## <a name="enable-the-windows-notification-service"></a><span data-ttu-id="c1c6e-110">Povolení služby oznámení Windows</span><span class="sxs-lookup"><span data-stu-id="c1c6e-110">Enable the Windows Notification Service</span></span>
### <a name="windows-8x-and-windows-phone-81-only"></a><span data-ttu-id="c1c6e-111">Windows 8.x a Windows Phone 8.1 pouze</span><span class="sxs-lookup"><span data-stu-id="c1c6e-111">Windows 8.x and Windows Phone 8.1 only</span></span>
<span data-ttu-id="c1c6e-112">Chcete-li použít **služby oznámení Windows** (označované jako WNS) ve vaší `Package.appxmanifest` souborů na `Application UI` klikněte na `All Image Assets` do pole levé robota.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-112">In order to use the **Windows Notification Service** (referred as WNS) in your `Package.appxmanifest` file on `Application UI` click on `All Image Assets` in the left bot box.</span></span> <span data-ttu-id="c1c6e-113">Na pravé straně pole v `Notifications`, změňte `toast capable` z `(not set)` k `(Yes)`.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-113">At the right of the box in `Notifications`, change `toast capable` from `(not set)` to `(Yes)`.</span></span>

### <a name="all-platforms"></a><span data-ttu-id="c1c6e-114">Všechny platformy</span><span class="sxs-lookup"><span data-stu-id="c1c6e-114">All platforms</span></span>
<span data-ttu-id="c1c6e-115">Budete muset synchronizovat vaší aplikaci účtu Microsoft a platforma pro zapojení.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-115">You need to synchronize your app to your Microsoft account and to the engagement platform.</span></span> <span data-ttu-id="c1c6e-116">Pro tento potřebujete vytvořit účet nebo se přihlásit [Centrum vývojářů pro windows](https://dev.windows.com).</span><span class="sxs-lookup"><span data-stu-id="c1c6e-116">For this you need to create an account or log on [windows dev center](https://dev.windows.com).</span></span> <span data-ttu-id="c1c6e-117">Poté vytvořte novou aplikaci a najít identifikátor SID a tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-117">After that create a new application and find the SID and secret key.</span></span> <span data-ttu-id="c1c6e-118">Na front-endu zapojení, přejděte na nastavení vaší aplikace v `native push` a vložte svoje přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-118">On the engagement frontend, go on your app setting in `native push` and paste your credentials.</span></span> <span data-ttu-id="c1c6e-119">Potom klikněte pravým tlačítkem na projekt, vyberte `store` a `Associate App with the Store...`.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-119">After that, right click on your project, select `store` and `Associate App with the Store...`.</span></span> <span data-ttu-id="c1c6e-120">Stačí vybrat aplikaci, které vytvoříte před k synchronizaci.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-120">You just need to select the application you have create before to synchronize it.</span></span>

## <a name="initialize-the-engagement-reach-sdk"></a><span data-ttu-id="c1c6e-121">Inicializace Engagement Reach SDK</span><span class="sxs-lookup"><span data-stu-id="c1c6e-121">Initialize the Engagement Reach SDK</span></span>
<span data-ttu-id="c1c6e-122">Změnit `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="c1c6e-122">Modify the `App.xaml.cs`:</span></span>

* <span data-ttu-id="c1c6e-123">Vložit `EngagementReach.Instance.Init` právě po `EngagementAgent.Instance.Init` ve vaší `InitEngagement` metoda:</span><span class="sxs-lookup"><span data-stu-id="c1c6e-123">Insert `EngagementReach.Instance.Init` just after `EngagementAgent.Instance.Init` in your `InitEngagement` method:</span></span>
  
      private void InitEngagement(IActivatedEventArgs e)
      {
        EngagementAgent.Instance.Init(e);
        EngagementReach.Instance.Init(e);
      }
  
  <span data-ttu-id="c1c6e-124">`EngagementReach.Instance.Init` Běží ve vyhrazené vlákno.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-124">The `EngagementReach.Instance.Init` runs in a dedicated thread.</span></span> <span data-ttu-id="c1c6e-125">Nemusíte dělat sami.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-125">You do not have to do it yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="c1c6e-126">Pokud používáte nabízená oznámení jinde v aplikaci, pak budete muset [sdílet kanál nabízené](#push-channel-sharing) s Engagement Reach.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-126">If you are using push notifications elsewhere in your application then you have to [share your push channel](#push-channel-sharing) with Engagement Reach.</span></span>
> 
> 

## <a name="integration"></a><span data-ttu-id="c1c6e-127">Integrace</span><span class="sxs-lookup"><span data-stu-id="c1c6e-127">Integration</span></span>
<span data-ttu-id="c1c6e-128">Engagement nabízí dva způsoby, jak do aplikace přidat Reach v aplikaci plakáty a vkládaná zobrazení pro oznámení a hlasování: integrace překrytí a ruční integraci webové zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-128">Engagement provides two ways to add the Reach in-app banners and interstitial views for announcements and polls in your application: the overlay integration and the web views manual integration.</span></span> <span data-ttu-id="c1c6e-129">Nesmí kombinovat obě přístup na stejné stránce.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-129">You should not combine both approach on the same page.</span></span>

<span data-ttu-id="c1c6e-130">Volba mezi dvěma integrace může shrnout takto:</span><span class="sxs-lookup"><span data-stu-id="c1c6e-130">The choice between the two integration could be summarized this way:</span></span>

* <span data-ttu-id="c1c6e-131">Můžete se rozhodnout integrace překrytí Pokud vaše stránky již dědí od agenta `EngagementPage`, bude stačit nahrazení `EngagementPage` podle `EngagementPageOverlay` a `xmlns:engagement="using:Microsoft.Azure.Engagement"` podle `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` na stránkách.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-131">You may choose the overlay integration if your pages already inherits from the Agent `EngagementPage`, it's just a matter of replacing `EngagementPage` by `EngagementPageOverlay` and `xmlns:engagement="using:Microsoft.Azure.Engagement"` by `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` in your pages.</span></span>
* <span data-ttu-id="c1c6e-132">Můžete se rozhodnout webové zobrazení ruční integrace Pokud chcete přesněji umístit dosáhnout uživatelského rozhraní v rámci vaší stránky, nebo pokud nechcete přidat jinou úroveň dědičnosti na stránky.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-132">You may choose the web views manual integration if you want to precisely place the Reach UI within your pages or if you don't want to add another inheritance level to your pages.</span></span> 

### <a name="overlay-integration"></a><span data-ttu-id="c1c6e-133">Integrace překrytí</span><span class="sxs-lookup"><span data-stu-id="c1c6e-133">Overlay integration</span></span>
<span data-ttu-id="c1c6e-134">Překrytí Engagement dynamicky přidá prvky uživatelského rozhraní slouží k zobrazení kampaně Reach v stránku.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-134">The Engagement overlay dynamically adds the UI elements used to display Reach campaigns in your page.</span></span> <span data-ttu-id="c1c6e-135">Pokud překrytí nebude vyhovovat rozložení byste měli zvážit webové zobrazení ruční integrace místo.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-135">If the overlay doesn't suit your layout you should consider the web views manual integration instead.</span></span>

<span data-ttu-id="c1c6e-136">V souboru změny XAML `EngagementPage` odkaz na`EngagementPageOverlay`</span><span class="sxs-lookup"><span data-stu-id="c1c6e-136">In your .xaml file change `EngagementPage` reference to `EngagementPageOverlay`</span></span>

* <span data-ttu-id="c1c6e-137">Přidejte do deklarací oborů názvů:</span><span class="sxs-lookup"><span data-stu-id="c1c6e-137">Add to your namespaces declarations:</span></span>
  
      xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"
* <span data-ttu-id="c1c6e-138">Nahraďte `engagement:EngagementPage` s `engagement:EngagementPageOverlay`:</span><span class="sxs-lookup"><span data-stu-id="c1c6e-138">Replace `engagement:EngagementPage` with `engagement:EngagementPageOverlay`:</span></span>

<span data-ttu-id="c1c6e-139">**S EngagementPage:**</span><span class="sxs-lookup"><span data-stu-id="c1c6e-139">**With EngagementPage:**</span></span>

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">

            <!-- Your layout -->
        </engagement:EngagementPage>

<span data-ttu-id="c1c6e-140">**S EngagementPageOverlay:**</span><span class="sxs-lookup"><span data-stu-id="c1c6e-140">**With EngagementPageOverlay:**</span></span>

        <engagement:EngagementPageOverlay 
            xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay">

            <!-- Your layout -->
        </engagement:EngagementPageOverlay>

<span data-ttu-id="c1c6e-141">V souboru .cs značky stránku v `EngagementPageOverlay` místo `EngagementPage` a import `Microsoft.Azure.Engagement.Overlay`.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-141">Then in your .cs file tag your page in `EngagementPageOverlay` instead of `EngagementPage` and import `Microsoft.Azure.Engagement.Overlay`.</span></span>

            using Microsoft.Azure.Engagement.Overlay;

* <span data-ttu-id="c1c6e-142">Nahraďte `EngagementPage` s `EngagementPageOverlay`:</span><span class="sxs-lookup"><span data-stu-id="c1c6e-142">Replace `EngagementPage` with `EngagementPageOverlay`:</span></span>

<span data-ttu-id="c1c6e-143">**S EngagementPage:**</span><span class="sxs-lookup"><span data-stu-id="c1c6e-143">**With EngagementPage:**</span></span>

            using Microsoft.Azure.Engagement;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPage
              {
                [...]
              }
            }

<span data-ttu-id="c1c6e-144">**S EngagementPageOverlay:**</span><span class="sxs-lookup"><span data-stu-id="c1c6e-144">**With EngagementPageOverlay:**</span></span>

            using Microsoft.Azure.Engagement.Overlay;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPageOverlay 
              {
                [...]
              }
            }


<span data-ttu-id="c1c6e-145">Přidá do překrytí Engagement `Grid` elementu na stránku skládá z vaší rozložení a dvě `WebView` prvky jeden pro informační zprávě a druhá pro vkládaná zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-145">The Engagement overlay adds a `Grid` element on top of your page composed of your layout and the two `WebView` elements one for the banner and the other one for the interstitial view.</span></span>

<span data-ttu-id="c1c6e-146">Můžete upravit přímo v překrytí elementy `EngagementPageOverlay.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-146">You can customize the overlay elements directly in the `EngagementPageOverlay.cs` file.</span></span>

### <a name="web-views-manual-integration"></a><span data-ttu-id="c1c6e-147">Webové zobrazení ruční integrace</span><span class="sxs-lookup"><span data-stu-id="c1c6e-147">Web views manual integration</span></span>
<span data-ttu-id="c1c6e-148">Reach bude hledání svoje stránky pro dva `WebView` elementy odpovědná za zobrazování hlavičky a vkládaná zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-148">Reach will be searching your pages for the two `WebView` elements responsible for displaying the banner and the interstitial view.</span></span> <span data-ttu-id="c1c6e-149">Jediné, co musíte udělat, je přidání těchto dvou `WebView` elementy někde na stránkách, tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="c1c6e-149">The only thing you have to do is to add those two `WebView` elements somewhere in your pages, here is an example:</span></span>

    <Grid x:Name="engagementGrid">

      <!-- Your layout -->

      <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Stretch" VerticalAlignment="Top"/>
      <WebView x:Name="engagement_announcement_content" Visibility="Collapsed"  HorizontalAlignment="Stretch"  VerticalAlignment="Stretch"/>
    </Grid>


<span data-ttu-id="c1c6e-150">V tomto příkladu `WebView` elementy jsou roztažen tak, aby vyhovovaly jejich kontejneru, který automaticky je znovu velikostí v případě změna velikosti obrazovky otočení nebo okna.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-150">In this example the `WebView` elements are stretched to fit their container which automatically re-sizes them in case of screen rotation or window size change.</span></span>

> [!WARNING]
> <span data-ttu-id="c1c6e-151">Je důležité zachovat stejnou pojmenování `engagement_notification_content` a `engagement_announcement_content` pro `WebView` elementy.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-151">It is important to keep the same naming `engagement_notification_content` and `engagement_announcement_content` for the `WebView` elements.</span></span> <span data-ttu-id="c1c6e-152">Reach je identifikace je podle názvu.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-152">Reach is identifying them by their name.</span></span> 
> 
> 

## <a name="handle-datapush-optional"></a><span data-ttu-id="c1c6e-153">Popisovač datapush (volitelné)</span><span class="sxs-lookup"><span data-stu-id="c1c6e-153">Handle datapush (optional)</span></span>
<span data-ttu-id="c1c6e-154">Pokud chcete aplikace nebudou moct přijímat Reach datová oznámení, je nutné implementovat dvě události EngagementReach třídy:</span><span class="sxs-lookup"><span data-stu-id="c1c6e-154">If you want your application to be able to receive Reach data pushes, you have to implement two events of the EngagementReach class:</span></span>

<span data-ttu-id="c1c6e-155">V souboru App.xaml.cs v konstruktoru App() přidejte:</span><span class="sxs-lookup"><span data-stu-id="c1c6e-155">In App.xaml.cs in the App() constructor add:</span></span>

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

<span data-ttu-id="c1c6e-156">Uvidíte, že zpětné volání každá metoda vrátí logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-156">You can see that the callback of each method returns a boolean.</span></span> <span data-ttu-id="c1c6e-157">Zapojení odešle zpětnou vazbu k jeho back-end po odeslání nabízeného oznámení data.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-157">Engagement sends a feedback to its back-end after dispatching the data push.</span></span> <span data-ttu-id="c1c6e-158">Pokud vrátí hodnotu false, zpětné volání `exit` zpětné vazby bude odesílat.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-158">If the callback returns false, the `exit` feedback will be send.</span></span> <span data-ttu-id="c1c6e-159">Jinak bude `action`.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-159">Otherwise, it will be `action`.</span></span> <span data-ttu-id="c1c6e-160">Pokud je pro události, nastavené žádné zpětného volání `drop` zapojení se vrátí zpětnou vazbu.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-160">If no callback is set for the events, the `drop` feedback will be returned to Engagement.</span></span>

> [!WARNING]
> <span data-ttu-id="c1c6e-161">Zapojení není schopný přijímat násobky názory pro datová oznámení.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-161">Engagement is not able to receive multiples feedbacks for a data push.</span></span> <span data-ttu-id="c1c6e-162">Pokud budete chtít nastavit několik obslužné rutiny na události, uvědomte si, zda bude poslední odpovídají zpětné vazby jeden odeslána.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-162">If you plan to set several handlers on an event, be aware that the feedback will correspond to the last one sent.</span></span> <span data-ttu-id="c1c6e-163">V takovém případě doporučujeme vždy vrací stejnou hodnotu, abyste nemuseli matoucí zpětnou vazbu na front-endu.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-163">In this case, we recommend to always returns the same value to avoid having confusing feedback on the front-end.</span></span>
> 
> 

## <a name="customize-ui-optional"></a><span data-ttu-id="c1c6e-164">Přizpůsobení uživatelského rozhraní (volitelné)</span><span class="sxs-lookup"><span data-stu-id="c1c6e-164">Customize UI (optional)</span></span>
### <a name="first-step"></a><span data-ttu-id="c1c6e-165">Prvním krokem</span><span class="sxs-lookup"><span data-stu-id="c1c6e-165">First step</span></span>
<span data-ttu-id="c1c6e-166">Můžeme vám umožňují přizpůsobit reach uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-166">We allow you to customize the reach UI.</span></span>

<span data-ttu-id="c1c6e-167">Uděláte to tak, budete muset vytvořit podtřídou třídy `EngagementReachHandler` třídy.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-167">To do so, you have to create a subclass of the `EngagementReachHandler` class.</span></span>

<span data-ttu-id="c1c6e-168">**Ukázkový kód:**</span><span class="sxs-lookup"><span data-stu-id="c1c6e-168">**Sample Code :**</span></span>

            using Microsoft.Azure.Engagement;

            namespace Example
            {
              internal class ExampleReachHandler : EngagementReachHandler
              {
               // Override EngagementReachHandler methods depending on your needs
              }
            }

<span data-ttu-id="c1c6e-169">Potom nastavte obsah `EngagementReach.Instance.Handler` pole s vaší vlastní objekt v vaše `App.xaml.cs` třídy v rámci `App()` metoda.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-169">Then, set the content of the `EngagementReach.Instance.Handler` field with your custom object in your `App.xaml.cs` class within the `App()` method.</span></span>

<span data-ttu-id="c1c6e-170">**Ukázkový kód:**</span><span class="sxs-lookup"><span data-stu-id="c1c6e-170">**Sample Code :**</span></span>

            protected override void OnLaunched(LaunchActivatedEventArgs args)
            {
              // your app initialization 
              EngagementReach.Instance.Handler = new ExampleReachHandler();
              // Engagement Agent and Reach initialization
            }

> [!NOTE]
> <span data-ttu-id="c1c6e-171">Ve výchozím nastavení používá Engagement jejich vlastní implementaci `EngagementReachHandler`.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-171">By default, Engagement uses its own implementation of `EngagementReachHandler`.</span></span>
> <span data-ttu-id="c1c6e-172">Nemusíte vytvářet vlastní, a pokud tak učiníte, nemusíte přepsat každou metodu.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-172">You don't have to create your own, and if you do so, you don't have to override every method.</span></span> <span data-ttu-id="c1c6e-173">Výchozí chování je výběr základní objekt zapojení.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-173">The default behavior is to select the Engagement base object.</span></span>
> 
> 

### <a name="web-view"></a><span data-ttu-id="c1c6e-174">Webové zobrazení</span><span class="sxs-lookup"><span data-stu-id="c1c6e-174">Web View</span></span>
<span data-ttu-id="c1c6e-175">Ve výchozím nastavení použije Reach vložené prostředky knihovny DLL k zobrazení oznámení a stránky.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-175">By default, Reach will use the embedded resources of the DLL to display the notifications and pages.</span></span>

<span data-ttu-id="c1c6e-176">K poskytování úplné přizpůsobení možnosti používáme pouze zobrazení webové stránky.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-176">To provide a full customization possibilities we only use web view.</span></span> <span data-ttu-id="c1c6e-177">Pokud chcete přizpůsobit rozložení, potlačí přímo soubory zdrojů `EngagementAnnouncement.html` a `EngagementNotification.html`.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-177">If you want to customize layouts, override directly the resources files `EngagementAnnouncement.html` and `EngagementNotification.html`.</span></span> <span data-ttu-id="c1c6e-178">Zapojení musí všechny kód v `<body></body>` správné fungování.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-178">Engagement needs all code in `<body></body>` to run correctly.</span></span> <span data-ttu-id="c1c6e-179">Ale můžete přidat značky vnější `engagement_webview_area`.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-179">But you can add tag outer `engagement_webview_area`.</span></span>

<span data-ttu-id="c1c6e-180">Můžete ale použít vlastní prostředky.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-180">However, you can decide to use your own resources.</span></span>

<span data-ttu-id="c1c6e-181">Můžete přepsat `EngagementReachHandler` metody v vaše podtřída říct Engagement používat vaše rozložení, ale pečlivě vložených mechanismus engagement:</span><span class="sxs-lookup"><span data-stu-id="c1c6e-181">You can override `EngagementReachHandler` methods in your subclass to tell Engagement to use your layouts, but take care to embedded the engagement mechanism:</span></span>

<span data-ttu-id="c1c6e-182">**Ukázkový kód:**</span><span class="sxs-lookup"><span data-stu-id="c1c6e-182">**Sample Code :**</span></span>

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


<span data-ttu-id="c1c6e-183">Ve výchozím nastavení je AnnouncementHTML `ms-appx-web:///Resources/EngagementAnnouncement.html`.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-183">By default, AnnouncementHTML is `ms-appx-web:///Resources/EngagementAnnouncement.html`.</span></span> <span data-ttu-id="c1c6e-184">Reprezentuje soubor html, který návrh obsah nabízené zprávy (Text oznámení, webové anoucement a dotazování oznámení).</span><span class="sxs-lookup"><span data-stu-id="c1c6e-184">It represents the html file that design the content of a push message (Text announcement, Web anoucement and Poll announcement).</span></span> <span data-ttu-id="c1c6e-185">Je AnnouncementName `engagement_announcement_content`.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-185">AnnouncementName is `engagement_announcement_content`.</span></span> <span data-ttu-id="c1c6e-186">Je název webového zobrazení návrhu ve vašem xaml – stránka.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-186">It is the name of the webview design in your xaml page.</span></span>

<span data-ttu-id="c1c6e-187">Je NotfificationHTML `ms-appx-web:///Resources/EngagementNotification.html`.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-187">NotfificationHTML is `ms-appx-web:///Resources/EngagementNotification.html`.</span></span> <span data-ttu-id="c1c6e-188">Reprezentuje soubor html, který navrhnout oznámení nabízená zpráva.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-188">It represents the html file that design the notification of a push message.</span></span> <span data-ttu-id="c1c6e-189">Je NotfificationName `engagement_notification_content`.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-189">NotfificationName is `engagement_notification_content`.</span></span> <span data-ttu-id="c1c6e-190">Je název webového zobrazení návrhu ve vašem xaml – stránka.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-190">It is the name of the webview design in your xaml page.</span></span>

### <a name="customization"></a><span data-ttu-id="c1c6e-191">Přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="c1c6e-191">Customization</span></span>
<span data-ttu-id="c1c6e-192">Můžete přizpůsobit oznámení a oznámení, že má webové zobrazení, je-li zachovat Engagement objektu.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-192">You can customize notification and announcement web view has you want if you preserve Engagement object.</span></span> <span data-ttu-id="c1c6e-193">Opatrní této webové zobrazení objekt je popsán třikrát - poprvé ve vašem jazyce xaml, ještě jednou v souboru .cs v metodě "setwebview()" a třetí čas v souboru html.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-193">Take care that webview object is described three times - the first time in your xaml, second time in your .cs file in the "setwebview()" method, and third time in the html file.</span></span>

* <span data-ttu-id="c1c6e-194">Ve vašem xaml popisují aktuální webové zobrazení součást grafické rozložení.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-194">In your xaml you describe the current graphical layout webview component.</span></span>
* <span data-ttu-id="c1c6e-195">V souboru .cs můžete definovat "setwebview()", ve kterém můžete nastavit dimenzi dva webové zobrazení (oznámení, oznámení).</span><span class="sxs-lookup"><span data-stu-id="c1c6e-195">In your .cs file you can define "setwebview()" in which you set the dimension of the two webview (notification, announcement).</span></span> <span data-ttu-id="c1c6e-196">Je velmi účinné při aplikace je změna velikosti.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-196">It is very effective when the application is resizing.</span></span>
* <span data-ttu-id="c1c6e-197">V souboru html Engagement jsme popisují webové zobrazení obsahu, návrhu a pozice prvky mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-197">In the Engagement html file we describe the webview content, design and the elements positions between each other.</span></span>

### <a name="launch-message"></a><span data-ttu-id="c1c6e-198">Spusťte zprávu</span><span class="sxs-lookup"><span data-stu-id="c1c6e-198">Launch message</span></span>
<span data-ttu-id="c1c6e-199">Když uživatel klikne na systémové oznámení (oznámení), Engagement spustí aplikaci, načíst obsah nabízená oznámení a zobrazení stránky pro odpovídající kampaně.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-199">When a user clicks on a system notification (a toast), Engagement launches the application, load the content of the push messages, and display the page for the corresponding campaign.</span></span>

<span data-ttu-id="c1c6e-200">Dochází ke zpoždění mezi spuštění aplikace a zobrazení stránky (v závislosti na rychlosti sítě).</span><span class="sxs-lookup"><span data-stu-id="c1c6e-200">There is a delay between the launch of the application and the display of the page (depending on the speed of your network).</span></span>

<span data-ttu-id="c1c6e-201">Chcete-li uživatele upozornit, načítání něco, by měl poskytovat vizuální informace, jako je indikátor průběhu nebo indikátor průběhu.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-201">To indicate to the user that something is loading, you should provide a visual information, like a progress bar or a progress indicator.</span></span> <span data-ttu-id="c1c6e-202">Zapojení nemůže zpracovat samostatně, ale poskytuje několik obslužné rutiny pro vás.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-202">Engagement cannot handle that itself, but provides a few handlers for you.</span></span>

<span data-ttu-id="c1c6e-203">Chcete-li implementovat zpětné volání, v souboru App.xaml.cs v "Veřejné App() {}" přidáte:</span><span class="sxs-lookup"><span data-stu-id="c1c6e-203">To implement the callback, in App.xaml.cs in "Public App(){}" add:</span></span>

            /* The application has launched and the content is loading.
             * You should display an indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };

            /* The application has finished loading the content and the page
             * is about to be displayed.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };

            /* The content has been loaded, but an error has occurred.
             * You can provide an information to the user.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

<span data-ttu-id="c1c6e-204">Zpětné volání můžete nastavit vaše metoda "Veřejné App() {}" vaší `App.xaml.cs` soubor, pokud možno před `EngagementReach.Instance.Init()` volání.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-204">You can set the callback in your "Public App(){}" method of your `App.xaml.cs` file, preferably before the `EngagementReach.Instance.Init()` call.</span></span>

> [!TIP]
> <span data-ttu-id="c1c6e-205">Každý obslužná rutina je volána službou vlákna uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-205">Each handler is called by the UI Thread.</span></span> <span data-ttu-id="c1c6e-206">Nemusíte si dělat starosti při použití a MessageBox nebo něco související uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-206">You do not have to worry when using a MessageBox or something UI-related.</span></span>
> 
> 

## <span data-ttu-id="c1c6e-207"><a id="push-channel-sharing"></a>Push sdílení kanálu</span><span class="sxs-lookup"><span data-stu-id="c1c6e-207"><a id="push-channel-sharing"></a> Push channel sharing</span></span>
<span data-ttu-id="c1c6e-208">Pokud používáte nabízená oznámení pro jiný účel ve vaší aplikaci budete muset použít nabízenou kanál sdílení funkce sady Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-208">If you are using push notifications for another purpose in your application then you have to use the push channel sharing feature of the Engagement SDK.</span></span> <span data-ttu-id="c1c6e-209">Tím se vyhnete zmeškaných push.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-209">This is to avoid missed push.</span></span>

* <span data-ttu-id="c1c6e-210">Můžete zadat vlastní kanál nabízené k inicializaci Engagement Reach.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-210">You can provide your own push channel to the Engagement Reach initialization.</span></span> <span data-ttu-id="c1c6e-211">Sada SDK použije místo požaduje novou.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-211">The SDK will use it instead of requesting a new one.</span></span>

<span data-ttu-id="c1c6e-212">Aktualizovat inicializace Engagement Reach s nabízené kanál v `InitEngagement` metoda z `App.xaml.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="c1c6e-212">Update the Engagement Reach initialization with your push channel in the `InitEngagement` method from the `App.xaml.cs` file:</span></span>

    /* Your own push channel logic... */
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    /*...Engagement initialization */
    EngagementAgent.Instance.Init(e);
    EngagementReach.Instance.Init(e,pushChannel);

* <span data-ttu-id="c1c6e-213">Případně pokud chcete využívat kanál nabízené po inicializaci Reach pak můžete nastavení zpětné volání na Engagement Reach získat kanál nabízené jednou vytváří se sadou SDK.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-213">Alternatively, if you just want to consume the push channel after the Reach initialization then you can set a callback on Engagement Reach to get the push channel once it is created by the SDK.</span></span>

<span data-ttu-id="c1c6e-214">Nastavit vaše zpětné volání v jakémkoli místě **po** inicializace Reach:</span><span class="sxs-lookup"><span data-stu-id="c1c6e-214">Set your callback at any place **after** the Reach initialization :</span></span>

    /* Set action on the SDK push channel. */
    EngagementReach.Instance.SetActionOnPushChannel((PushNotificationChannel channel) => 
    {
      /* The forwarded channel can be null if its creation fails for any reason. */
      if (channel != null)
      {
        /* Your own push channel logic... */
      });
    }

## <a name="custom-scheme-tip"></a><span data-ttu-id="c1c6e-215">Tip pro vlastní schéma</span><span class="sxs-lookup"><span data-stu-id="c1c6e-215">Custom scheme tip</span></span>
<span data-ttu-id="c1c6e-216">Poskytujeme použití vlastní schéma.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-216">We provide custom scheme use.</span></span> <span data-ttu-id="c1c6e-217">Můžete odeslat jiný typ identifikátoru URI z front-endu engagement mají být použity v zapojení aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-217">You can send different type of URI from engagement frontend to be used in your engagement application.</span></span> <span data-ttu-id="c1c6e-218">Výchozí schéma jako `http, ftp, ...` jsou spravovat v systému Windows, bude okno řádku by šlo žádná výchozí aplikace nainstalované v zařízení.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-218">Default scheme like `http, ftp, ...` are manage by Windows, a window will prompt if they are no default application installed on device.</span></span> <span data-ttu-id="c1c6e-219">Můžete také vytvořit své vlastní schéma pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-219">You can also create your own custom scheme for your application.</span></span>

<span data-ttu-id="c1c6e-220">Jednoduchý způsob, jak nastavit vlastní schéma v aplikaci je otevřete váš `Package.appxmanifest` přejděte v `Declarations` panelu.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-220">The simple way to set a custom scheme in your application is to open your `Package.appxmanifest` go in `Declarations` panel.</span></span> <span data-ttu-id="c1c6e-221">Vyberte `Protocol` v dostupné deklarace posuňte pole a přidejte ji.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-221">Select `Protocol` in the Available Declarations scroll box and add it.</span></span> <span data-ttu-id="c1c6e-222">Upravit `Name` pole s nový protokol požadovaný název.</span><span class="sxs-lookup"><span data-stu-id="c1c6e-222">Edit the `Name` field with your new protocol desired name.</span></span>

<span data-ttu-id="c1c6e-223">Nyní Pokud chcete použít tento protokol, upravte vaše `App.xaml.cs` s `OnActivated` metoda a nezapomeňte také inicializovat engagement zde:</span><span class="sxs-lookup"><span data-stu-id="c1c6e-223">Now to use this protocol, edit your `App.xaml.cs` with the `OnActivated` method, and don't forget to initialize engagement here also:</span></span>

            /// <summary>
            /// Enter point when app his called by another way than user click
            /// </summary>
            /// <param name="args">Activation args</param>
            protected override void OnActivated(IActivatedEventArgs args)
            {
              /* Init engagement like it was launch by a custom uri scheme */
              EngagementAgent.Instance.Init(args);
              EngagementReach.Instance.Init(args);

              //TODO design action to do when app is launch

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

