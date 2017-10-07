---
title: "aaaWindows integrace sady SDK dosáhnout Phone Silverlight"
description: Jak tooIntegrate Azure Mobile Engagement Reach s aplikacemi pro Windows Phone Silverlight
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: d3516a6b-db9f-4cdb-a475-4148edf81af1
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 09c8767216e11963c5c600755ab8d4d11cd92034
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="windows-phone-silverlight-reach-sdk-integration"></a><span data-ttu-id="31260-103">Windows Phone Silverlight Reach integraci sady SDK</span><span class="sxs-lookup"><span data-stu-id="31260-103">Windows Phone Silverlight Reach SDK Integration</span></span>
<span data-ttu-id="31260-104">Je třeba postupovat podle hello integrace postup popsaný v hello [integraci sady Windows Phone Silverlight Engagement SDK](mobile-engagement-windows-phone-integrate-engagement.md) před těchto pokynů.</span><span class="sxs-lookup"><span data-stu-id="31260-104">You must follow hello integration procedure described in hello [Windows Phone Silverlight Engagement SDK Integration](mobile-engagement-windows-phone-integrate-engagement.md) before following this guide.</span></span>

## <a name="embed-hello-engagement-reach-sdk-into-your-windows-phone-silverlight-project"></a><span data-ttu-id="31260-105">Vložení hello Engagement Reach SDK do projektu Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="31260-105">Embed hello Engagement Reach SDK into your Windows Phone Silverlight project</span></span>
<span data-ttu-id="31260-106">Nemáte nic tooadd.</span><span class="sxs-lookup"><span data-stu-id="31260-106">You do not have anything tooadd.</span></span> <span data-ttu-id="31260-107">`EngagementReach`odkazy a prostředky jsou už ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="31260-107">`EngagementReach` references and resources are already in your project.</span></span>

> [!TIP]
> <span data-ttu-id="31260-108">Můžete přizpůsobit bitové kopie, které jsou umístěné v hello `Resources` složky projektu, zejména hello brand ikona (této výchozí toohello Engagement).</span><span class="sxs-lookup"><span data-stu-id="31260-108">You can customize images located in hello `Resources` folder of your project, especially hello brand icon (that default toohello Engagement icon).</span></span>
> 
> 

## <a name="add-hello-capabilities"></a><span data-ttu-id="31260-109">Přidání možností hello</span><span class="sxs-lookup"><span data-stu-id="31260-109">Add hello capabilities</span></span>
<span data-ttu-id="31260-110">Hello Engagement Reach SDK potřebuje některé další funkce.</span><span class="sxs-lookup"><span data-stu-id="31260-110">hello Engagement Reach SDK needs some additional capabilities.</span></span>

<span data-ttu-id="31260-111">Otevřete váš `WMAppManifest.xml` soubor a zkontrolujte, zda jsou deklarovány této hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="31260-111">Open your `WMAppManifest.xml` file and be sure that hello following capabilities are declared:</span></span>

* `ID_CAP_PUSH_NOTIFICATION`
* `ID_CAP_WEBBROWSERCOMPONENT`

<span data-ttu-id="31260-112">Hello nejprve jeden používá hello MPNS služby tooallow hello zobrazení informační zpráva.</span><span class="sxs-lookup"><span data-stu-id="31260-112">hello first one is used by hello MPNS service tooallow hello display of toast notification.</span></span> <span data-ttu-id="31260-113">Hello druhá je použité tooembed úlohu prohlížeče do hello SDK.</span><span class="sxs-lookup"><span data-stu-id="31260-113">hello second one is used tooembed a browser task into hello SDK.</span></span>

<span data-ttu-id="31260-114">Upravit hello `WMAppManifest.xml` souboru a přidejte uvnitř hello `<Capabilities />` značky:</span><span class="sxs-lookup"><span data-stu-id="31260-114">Edit hello `WMAppManifest.xml` file and add inside hello `<Capabilities />` tag :</span></span>

    <Capability Name="ID_CAP_PUSH_NOTIFICATION" />
    <Capability Name="ID_CAP_WEBBROWSERCOMPONENT" />

## <a name="enable-hello-microsoft-push-notification-service"></a><span data-ttu-id="31260-115">Povolit hello Microsoft službu nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="31260-115">Enable hello Microsoft Push Notification Service</span></span>
<span data-ttu-id="31260-116">V pořadí toouse hello **Microsoft službu nabízených oznámení** (označované jako MPNS) vaší `WMAppManifest.xml` soubor musí mít `<App />` značku s `Publisher` atribut nastavit toohello název projektu.</span><span class="sxs-lookup"><span data-stu-id="31260-116">In order toouse hello **Microsoft Push Notification Service** (referred as MPNS) your `WMAppManifest.xml` file must have an `<App />` tag with a `Publisher` attribute set toohello name of your project.</span></span>

## <a name="initialize-hello-engagement-reach-sdk"></a><span data-ttu-id="31260-117">Inicializace hello Engagement Reach SDK</span><span class="sxs-lookup"><span data-stu-id="31260-117">Initialize hello Engagement Reach SDK</span></span>
### <a name="engagement-configuration"></a><span data-ttu-id="31260-118">Konfigurace zapojení</span><span class="sxs-lookup"><span data-stu-id="31260-118">Engagement configuration</span></span>
<span data-ttu-id="31260-119">Konfigurace Engagement Hello je centralizovaná v hello `Resources\EngagementConfiguration.xml` souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="31260-119">hello Engagement configuration is centralized in hello `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="31260-120">Úpravy konfigurace reach toospecify souboru:</span><span class="sxs-lookup"><span data-stu-id="31260-120">Edit this file toospecify reach configuration :</span></span>

* <span data-ttu-id="31260-121">*Volitelné*, zda je aktivována hello nativního nabízení (MPNS) nebo není mezi `<enableNativePush>` a `</enableNativePush>` značky, (`true` ve výchozím nastavení).</span><span class="sxs-lookup"><span data-stu-id="31260-121">*Optional*, indicate whether hello native push (MPNS) is activated or not between `<enableNativePush>` and `</enableNativePush>` tags, (`true` by default).</span></span>
* <span data-ttu-id="31260-122">*Volitelné*, označuje název hello hello nabízené kanál mezi `<channelName>` a `</channelName>` značky, poskytují hello stejné, že vaše aplikace může aktuálně použít nebo hodnotu nevyplňujte.</span><span class="sxs-lookup"><span data-stu-id="31260-122">*Optional*, indicate hello name of hello push channel between `<channelName>` and `</channelName>` tags, provide hello same that your application may currently use or leave it empty.</span></span>

<span data-ttu-id="31260-123">Pokud chcete, aby ho za běhu místo toho můžete volat hello následující toospecify metoda před inicializací agenta hello Engagement:</span><span class="sxs-lookup"><span data-stu-id="31260-123">If you want toospecify it at runtime instead, you can call hello following method before hello Engagement agent initialization :</span></span>

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    engagementConfiguration.Reach.EnableNativePush = true;                  
    /* [Optional] whether hello native push (MPNS) is activated or not. */

    engagementConfiguration.Reach.ChannelName = "YOUR_PUSH_CHANNEL_NAME";   
    /* [Optional] Provide hello same channel name that your application may currently use. */

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

> [!TIP]
> <span data-ttu-id="31260-124">Můžete zadat název hello hello MPNS nabízené kanál svojí aplikace.</span><span class="sxs-lookup"><span data-stu-id="31260-124">You can specify hello name of hello MPNS push channel of your application.</span></span> <span data-ttu-id="31260-125">Ve výchozím nastavení vytvoří Engagement název založený na hello appId.</span><span class="sxs-lookup"><span data-stu-id="31260-125">By default, Engagement creates a name based on hello appId.</span></span> <span data-ttu-id="31260-126">Máte bez nutnosti toospecify hello názvu sami, s výjimkou Pokud máte v plánu toouse hello nabízené kanál mimo zapojení.</span><span class="sxs-lookup"><span data-stu-id="31260-126">You have no need toospecify hello name yourself, except if you plan toouse hello push channel outside of Engagement.</span></span>
> 
> 

### <a name="engagement-initialization"></a><span data-ttu-id="31260-127">Inicializace zapojení</span><span class="sxs-lookup"><span data-stu-id="31260-127">Engagement initialization</span></span>
<span data-ttu-id="31260-128">Upravit hello `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="31260-128">Modify hello `App.xaml.cs`:</span></span>

* <span data-ttu-id="31260-129">Přidat tooyour `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="31260-129">Add tooyour `using` statements :</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="31260-130">Vložit `EngagementReach.Instance.Init` právě po `EngagementAgent.Instance.Init` v `Application_Launching` :</span><span class="sxs-lookup"><span data-stu-id="31260-130">Insert `EngagementReach.Instance.Init` just after `EngagementAgent.Instance.Init` in `Application_Launching` :</span></span>
  
      private void Application_Launching(object sender, LaunchingEventArgs e)
      {
         EngagementAgent.Instance.Init();
         EngagementReach.Instance.Init();
      }
* <span data-ttu-id="31260-131">Vložit `EngagementReach.Instance.OnActivated` v hello `Application_Activated` metoda:</span><span class="sxs-lookup"><span data-stu-id="31260-131">Insert `EngagementReach.Instance.OnActivated` in hello `Application_Activated` method :</span></span>
  
      private void Application_Activated(object sender, ActivatedEventArgs e)
      {
         EngagementAgent.Instance.OnActivated(e);
         EngagementReach.Instance.OnActivated(e);
      }

> [!IMPORTANT]
> <span data-ttu-id="31260-132">Hello `EngagementReach.Instance.Init` běží ve vyhrazené vlákno.</span><span class="sxs-lookup"><span data-stu-id="31260-132">hello `EngagementReach.Instance.Init` runs in a dedicated thread.</span></span> <span data-ttu-id="31260-133">Nemáte toodo ho sami.</span><span class="sxs-lookup"><span data-stu-id="31260-133">You do not have toodo it yourself.</span></span>
> 
> 

## <a name="app-store-submission-considerations"></a><span data-ttu-id="31260-134">Aspekty odeslání úložiště aplikací</span><span class="sxs-lookup"><span data-stu-id="31260-134">App store submission considerations</span></span>
<span data-ttu-id="31260-135">Microsoft ukládá některá pravidla při použití hello nabízených oznámení:</span><span class="sxs-lookup"><span data-stu-id="31260-135">Microsoft imposes some rules when using hello push notifications:</span></span>

<span data-ttu-id="31260-136">Z hello Microsoft [zásady aplikací] části 2.9, dokumentace:</span><span class="sxs-lookup"><span data-stu-id="31260-136">From hello Microsoft [Application Policies] documentation, section 2.9:</span></span>

1) <span data-ttu-id="31260-137">Musíte požádat hello uživatele tooaccept tooreceive nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="31260-137">You must ask hello user tooaccept tooreceive push notifications.</span></span> <span data-ttu-id="31260-138">Potom si v nastaveních, přidejte způsob toodisable hello nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="31260-138">Then, in your settings, add a way toodisable hello push notifications.</span></span>

<span data-ttu-id="31260-139">objekt EngagementReach Hello nabízí dvě metody toomanage hello opt v nebo odhlášení, `EnableNativePush()` a `DisableNativePush()`.</span><span class="sxs-lookup"><span data-stu-id="31260-139">hello EngagementReach object provides two methods toomanage hello opt-in/opt-out, `EnableNativePush()` and `DisableNativePush()`.</span></span> <span data-ttu-id="31260-140">Může například vytvořit možnost v nastavení hello s toodisable přepínač nebo povolit MPNS.</span><span class="sxs-lookup"><span data-stu-id="31260-140">You could, for example, create an option in hello settings with a toggle toodisable or enable MPNS.</span></span>

<span data-ttu-id="31260-141">Můžete také rozhodnout toodeactivate MPNS prostřednictvím konfigurace Engagement hello\<windows phone-sdk-reach konfigurace\>.</span><span class="sxs-lookup"><span data-stu-id="31260-141">You can also decide toodeactivate MPNS through hello Engagement configuration\<windows-phone-sdk-reach-configuration\>.</span></span>

> <span data-ttu-id="31260-142">2.9.1) aplikace hello musí nejprve popisují toobe oznámení hello zadaný a **získat express oprávnění uživatele hello (výslovný souhlas)**, a **musí nabízejí mechanismus, pomocí které hello uživatele můžete vyjádření výslovného nesouhlasu s přijetím nabízená oznámení**.</span><span class="sxs-lookup"><span data-stu-id="31260-142">2.9.1) hello application must first describe hello notifications toobe provided and **obtain hello user’s express permission (opt-in)**, and **must provide a mechanism through which hello user can opt out of receiving push notifications**.</span></span> <span data-ttu-id="31260-143">Všechna oznámení zadaný pomocí hello Microsoft službu nabízených oznámení musí být v souladu s hello poskytnutý popis toohello uživatele a musí dodržovat všechny příslušné [zásady aplikací] [ Content Policies]a [další požadavky pro konkrétní aplikaci typy].</span><span class="sxs-lookup"><span data-stu-id="31260-143">All notifications provided using hello Microsoft Push Notification Service must be consistent with hello description provided toohello user and must comply with all applicable [Application Policies][Content Policies] and [Additional Requirements for Specific Application Types].</span></span>
> 
> 

2) <span data-ttu-id="31260-144">Neměli byste používat příliš mnoho nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="31260-144">You should not use too many push notifications.</span></span> <span data-ttu-id="31260-145">Zapojení bude zpracovávat oznámení za vás.</span><span class="sxs-lookup"><span data-stu-id="31260-145">Engagement will handle notifications for you.</span></span>

> <span data-ttu-id="31260-146">2.9.2) hello aplikace a jeho použití programu hello Microsoft službu nabízených oznámení musí není nadměrně pomocí dostatečnou kapacitu sítě nebo šířka pásma hello Microsoft službu nabízených oznámení nebo jinak neoprávněně nebyli Windows Phone nebo jiných Microsoft zařízení nebo službu. s nadměrné nabízená oznámení, jak dle svého uvážení určí společnost Microsoft a nesmí poškodit nebo narušit žádné sítě, Microsoft nebo servery nebo serverech třetích stran nebo sítě připojené toohello Microsoft službu nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="31260-146">2.9.2) hello application and its use of hello Microsoft Push Notification Service must not excessively use network capacity or bandwidth of hello Microsoft Push Notification Service, or otherwise unduly burden a Windows Phone or other Microsoft device or service with excessive push notifications, as determined by Microsoft in its reasonable discretion, and must not harm or interfere with any Microsoft networks or servers or any third party servers or networks connected toohello Microsoft Push Notification Service.</span></span>
> 
> 

3) <span data-ttu-id="31260-147">Nespoléhejte na MPNS toosend criticals informace.</span><span class="sxs-lookup"><span data-stu-id="31260-147">Do not rely on MPNS toosend criticals information.</span></span> <span data-ttu-id="31260-148">Zapojení používá MPNS, takže toto pravidlo platí také pro hello kampaně vytvářet v hello Engagement front-endu.</span><span class="sxs-lookup"><span data-stu-id="31260-148">Engagement uses MPNS, so this rule also applies for hello campaigns created inside hello Engagement front-end.</span></span>

> <span data-ttu-id="31260-149">2.9.3) hello Microsoft službu nabízených oznámení nemusí být použité toosend oznámení, která jsou mise kritické nebo jinak mohou ovlivnit záležitosti životnosti nebo smrti, včetně bez omezení kritické oznámení související tooa lékařské zařízení nebo podmínku.</span><span class="sxs-lookup"><span data-stu-id="31260-149">2.9.3) hello Microsoft Push Notification Service may not be used toosend notifications that are mission critical or otherwise could affect matters of life or death, including without limitation critical notifications related tooa medical device or condition.</span></span> <span data-ttu-id="31260-150">MICROSOFT výslovně zříká ANY záruky, hello použití z hello MICROSOFT NABÍZENÁ oznámení služby nebo doručení z MICROSOFT NABÍZENÁ oznámení služby oznámení bude být bez přerušení, chyba volné nebo jinak zaručit tooOCCUR základ v reálném čase A ON.</span><span class="sxs-lookup"><span data-stu-id="31260-150">MICROSOFT EXPRESSLY DISCLAIMS ANY WARRANTIES THAT hello USE OF hello MICROSOFT PUSH NOTIFICATION SERVICE OR DELIVERY OF MICROSOFT PUSH NOTIFICATION SERVICE NOTIFICATIONS WILL BE UNINTERRUPTED, ERROR FREE, OR OTHERWISE GUARANTEED tooOCCUR ON A REAL-TIME BASIS.</span></span>
> 
> 

<span data-ttu-id="31260-151">**Nemůžeme zaručit aplikace předá proces ověření hello Pokud nerespektují tato doporučení.**</span><span class="sxs-lookup"><span data-stu-id="31260-151">**We cannot guarantee that your application will pass hello validation process if you do not respect these recommendations.**</span></span>

## <a name="handle-data-push-optional"></a><span data-ttu-id="31260-152">Zpracování datová oznámení (volitelné)</span><span class="sxs-lookup"><span data-stu-id="31260-152">Handle data push (optional)</span></span>
<span data-ttu-id="31260-153">Pokud chcete, aby vaše aplikace toobe možné tooreceive Reach datová oznámení, máte dvě události tooimplement hello EngagementReach třídy:</span><span class="sxs-lookup"><span data-stu-id="31260-153">If you want your application toobe able tooreceive Reach data pushes, you have tooimplement two events of hello EngagementReach class:</span></span>

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

<span data-ttu-id="31260-154">Uvidíte, že hello zpětné volání z každé metoda vrátí logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="31260-154">You can see that hello callback of each method returns a boolean.</span></span> <span data-ttu-id="31260-155">Zapojení odešle zpětné vazby tooits back-end po odeslání hello datová oznámení.</span><span class="sxs-lookup"><span data-stu-id="31260-155">Engagement sends a feedback tooits back-end after dispatching hello data push.</span></span> <span data-ttu-id="31260-156">Pokud zpětné volání hello vrací hodnotu false, hello `exit` zpětné vazby bude odesílat.</span><span class="sxs-lookup"><span data-stu-id="31260-156">If hello callback returns false, hello `exit` feedback will be send.</span></span> <span data-ttu-id="31260-157">Jinak bude `action`.</span><span class="sxs-lookup"><span data-stu-id="31260-157">Otherwise, it will be `action`.</span></span> <span data-ttu-id="31260-158">Pokud je pro události hello nastavené žádné zpětné volání, hello `drop` tooEngagement bude vrácen zpětnou vazbu.</span><span class="sxs-lookup"><span data-stu-id="31260-158">If no callback is set for hello events, hello `drop` feedback will be returned tooEngagement.</span></span>

> [!WARNING]
> <span data-ttu-id="31260-159">Zapojení není možné tooreceive násobky názory pro datová oznámení.</span><span class="sxs-lookup"><span data-stu-id="31260-159">Engagement is not able tooreceive multiples feedbacks for a data push.</span></span> <span data-ttu-id="31260-160">Pokud máte v plánu tooset několik obslužné rutiny na události, mějte na paměti, že zpětnou vazbu hello bude odpovídat toohello naposledy odeslána.</span><span class="sxs-lookup"><span data-stu-id="31260-160">If you plan tooset several handlers on an event, be aware that hello feedback will correspond toohello last one sent.</span></span> <span data-ttu-id="31260-161">V takovém případě doporučujeme tooalways vrátí hello stejnou hodnotu tooavoid s matoucí zpětnou vazbu o hello front-endu.</span><span class="sxs-lookup"><span data-stu-id="31260-161">In this case, we recommend tooalways returns hello same value tooavoid having confusing feedback on hello front-end.</span></span>
> 
> 

## <a name="customize-ui-optional"></a><span data-ttu-id="31260-162">Přizpůsobení uživatelského rozhraní (volitelné)</span><span class="sxs-lookup"><span data-stu-id="31260-162">Customize UI (optional)</span></span>
### <a name="first-step"></a><span data-ttu-id="31260-163">Prvním krokem</span><span class="sxs-lookup"><span data-stu-id="31260-163">First step</span></span>
<span data-ttu-id="31260-164">Můžeme vám umožňují toocustomize hello reach uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="31260-164">We allow you toocustomize hello reach UI.</span></span>

<span data-ttu-id="31260-165">toodo tedy máte toocreate, podtřídou třídy hello `EngagementReachHandler` třídy.</span><span class="sxs-lookup"><span data-stu-id="31260-165">toodo so, you have toocreate a subclass of hello `EngagementReachHandler` class.</span></span>

<span data-ttu-id="31260-166">**Ukázkový kód:**</span><span class="sxs-lookup"><span data-stu-id="31260-166">**Sample Code :**</span></span>

    using Microsoft.Azure.Engagement;

    namespace Example
    {
       internal class ExampleReachHandler : EngagementReachHandler
       {
          // Override EngagementReachHandler methods depending on your needs
       }
    }

<span data-ttu-id="31260-167">Potom nastavte hello obsah hello `EngagementReach.Instance.Handler` pole s vaší vlastní objekt v vaše `App.xaml.cs` třídu v rámci hello `Application_Launching` metoda.</span><span class="sxs-lookup"><span data-stu-id="31260-167">Then, set hello content of hello `EngagementReach.Instance.Handler` field with your custom object in your `App.xaml.cs` class within hello `Application_Launching` method.</span></span>

<span data-ttu-id="31260-168">**Ukázkový kód:**</span><span class="sxs-lookup"><span data-stu-id="31260-168">**Sample Code :**</span></span>

    private void Application_Launching(object sender, LaunchingEventArgs e)
    {
       // your app initialization 
       EngagementReach.Instance.Handler = new ExampleReachHandler();
       // Engagement Agent and Reach initialization
    }

> [!NOTE]
> <span data-ttu-id="31260-169">Ve výchozím nastavení používá Engagement jejich vlastní implementaci `EngagementReachHandler`.</span><span class="sxs-lookup"><span data-stu-id="31260-169">By default, Engagement uses its own implementation of `EngagementReachHandler`.</span></span> <span data-ttu-id="31260-170">Nemáte toocreate vlastní, a pokud tak učiníte, nemáte toooverride každou metodu.</span><span class="sxs-lookup"><span data-stu-id="31260-170">You don't have toocreate your own, and if you do so, you don't have toooverride every method.</span></span> <span data-ttu-id="31260-171">Hello výchozí chování je základní objekt Engagement tooselect hello.</span><span class="sxs-lookup"><span data-stu-id="31260-171">hello default behavior is tooselect hello Engagement base object.</span></span>
> 
> 

### <a name="layouts"></a><span data-ttu-id="31260-172">Rozložení</span><span class="sxs-lookup"><span data-stu-id="31260-172">Layouts</span></span>
<span data-ttu-id="31260-173">Ve výchozím nastavení použije Reach hello vložených prostředků hello DLL toodisplay hello oznámení a stránky.</span><span class="sxs-lookup"><span data-stu-id="31260-173">By default, Reach will use hello embedded resources of hello DLL toodisplay hello notifications and pages.</span></span>

<span data-ttu-id="31260-174">Ale můžete rozhodnout toouse, vlastní prostředky tooreflect vaší značkou v těchto součástí.</span><span class="sxs-lookup"><span data-stu-id="31260-174">However, you can decide toouse your own resources tooreflect your brand in these components.</span></span>

<span data-ttu-id="31260-175">Můžete přepsat `EngagementReachHandler` metody ve vaší podtřídami tootell Engagement toouse vaše rozložení:</span><span class="sxs-lookup"><span data-stu-id="31260-175">You can override `EngagementReachHandler` methods in your subclass tootell Engagement toouse your layouts :</span></span>

<span data-ttu-id="31260-176">**Ukázkový kód:**</span><span class="sxs-lookup"><span data-stu-id="31260-176">**Sample Code :**</span></span>

    // In your subclass of EngagementReachHandler

    public override string GetTextViewAnnouncementUri()
    {
       // return hello path of your own xaml
    }

    public override string GetWebViewAnnouncementUri()
    {
       // return hello path of your own xaml
    }

    public override string GetPollUri()
    {
       // return hello path of your own xaml
    }

    public override EngagementNotificationView CreateNotification(EngagementNotificationViewModel viewModel)
    {
       // return a new instance of your own notification
    }

> [!TIP]
> <span data-ttu-id="31260-177">Hello `CreateNotification` metoda může vrátit hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="31260-177">hello `CreateNotification` method can return null.</span></span> <span data-ttu-id="31260-178">Hello oznámení se nezobrazí a kampaně reach hello se zahodí.</span><span class="sxs-lookup"><span data-stu-id="31260-178">hello notification won't be displayed and hello reach campaign will be dropped.</span></span>
> 
> 

<span data-ttu-id="31260-179">toosimplify implementaci rozložení poskytujeme také vlastní xaml, který může sloužit jako základ pro váš kód.</span><span class="sxs-lookup"><span data-stu-id="31260-179">toosimplify your layout implementation, we also provide our own xaml which can serve as a basis for your code.</span></span> <span data-ttu-id="31260-180">Se nacházejí v archivu Engagement SDK hello (nebo src/reach /).</span><span class="sxs-lookup"><span data-stu-id="31260-180">They are located in hello Engagement SDK archive (/src/reach/).</span></span>

> [!WARNING]
> <span data-ttu-id="31260-181">Hello zdroje, které poskytujeme jsou hello přesně stejnou ty, které používáme.</span><span class="sxs-lookup"><span data-stu-id="31260-181">hello sources that we provide are hello exact same ones that we use.</span></span> <span data-ttu-id="31260-182">Takže pokud chcete toomodify je přímo, nemáte zapomněli toochange hello obor názvů a hello název.</span><span class="sxs-lookup"><span data-stu-id="31260-182">So if you want toomodify them directly, don't forget toochange hello namespace and hello name.</span></span>
> 
> 

### <a name="notification-position"></a><span data-ttu-id="31260-183">Pozice oznámení</span><span class="sxs-lookup"><span data-stu-id="31260-183">Notification position</span></span>
<span data-ttu-id="31260-184">Ve výchozím nastavení v hello spodní části hello aplikace se zobrazí oznámení v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="31260-184">By default, an in-app notification is displayed at hello bottom left side of hello application.</span></span> <span data-ttu-id="31260-185">Toto chování můžete změnit přepsání hello `GetNotificationPosition` metoda hello `EngagementReachHandler` objektu.</span><span class="sxs-lookup"><span data-stu-id="31260-185">You can change this behavior by overriding hello `GetNotificationPosition` method of hello `EngagementReachHandler` object.</span></span>

    // In your subclass of EngagementReachHandler

    public override EngagementReachHandler.NotificationPosition GetNotificationPosition(EngagementNotificationViewModel viewModel)
    {
       // return a value of hello EngagementReachHandler.NotificationPosition enum (TOP or BOTTOM)
    }

<span data-ttu-id="31260-186">V současné době můžete zvolit hello `BOTTOM` (výchozí) a `TOP` pozic.</span><span class="sxs-lookup"><span data-stu-id="31260-186">Currently, you can choose between hello `BOTTOM` (default) and `TOP` positions.</span></span>

### <a name="launch-message"></a><span data-ttu-id="31260-187">Spusťte zprávu</span><span class="sxs-lookup"><span data-stu-id="31260-187">Launch message</span></span>
<span data-ttu-id="31260-188">Když uživatel klikne na systémové oznámení (oznámení), spustí Engagement hello aplikace, obsah hello zatížení hello nabízené zprávy a zobrazit stránku hello hello odpovídající kampaně.</span><span class="sxs-lookup"><span data-stu-id="31260-188">When a user clicks on a system notification (a toast), Engagement launches hello app, load hello content of hello push messages, and display hello page for hello corresponding campaign.</span></span>

<span data-ttu-id="31260-189">Dochází ke zpoždění mezi hello spuštění aplikace a hello zobrazení hello hello stránky (v závislosti na rychlosti sítě hello).</span><span class="sxs-lookup"><span data-stu-id="31260-189">There is a delay between hello launch of hello application and hello display of hello page (depending on hello speed of your network).</span></span>

<span data-ttu-id="31260-190">tooindicate toohello uživatele, který se načítá něco, by měl poskytovat vizuální informace, jako je indikátor průběhu nebo indikátor průběhu.</span><span class="sxs-lookup"><span data-stu-id="31260-190">tooindicate toohello user that something is loading, you should provide a visual information, like a progress bar or a progress indicator.</span></span> <span data-ttu-id="31260-191">Zapojení nemůže zpracovat samostatně, ale poskytuje několik obslužné rutiny pro vás.</span><span class="sxs-lookup"><span data-stu-id="31260-191">Engagement cannot handle that itself, but provides a few handlers for you.</span></span>

<span data-ttu-id="31260-192">tooimplement hello zpětného volání, proveďte:</span><span class="sxs-lookup"><span data-stu-id="31260-192">tooimplement hello callback, do:</span></span>

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

<span data-ttu-id="31260-193">Zpětné volání hello můžete nastavit v vaše `Application_Launching` metodu vaše `App.xaml.cs` soubor, pokud možno před hello `EngagementReach.Instance.Init()` volání.</span><span class="sxs-lookup"><span data-stu-id="31260-193">You can set hello callback in your `Application_Launching` method of your `App.xaml.cs` file, preferably before hello `EngagementReach.Instance.Init()` call.</span></span>

> [!TIP]
> <span data-ttu-id="31260-194">Každý obslužná rutina je volána službou hello vlákna uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="31260-194">Each handler is called by hello UI Thread.</span></span> <span data-ttu-id="31260-195">Při použití a MessageBox nebo něco uživatelského rozhraní související nemáte tooworry.</span><span class="sxs-lookup"><span data-stu-id="31260-195">You do not have tooworry when using a MessageBox or something UI-related.</span></span>
> 
> 

[zásady aplikací]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx
[Content Policies]:http://msdn.microsoft.com/library/windows/apps/hh184842(v=vs.105).aspx
[další požadavky pro konkrétní aplikaci typy]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx

