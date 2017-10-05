---
title: Windows Phone Silverlight Reach integraci sady SDK
description: "Postup při integraci Azure Mobile Engagement Reach s aplikacemi pro Windows Phone Silverlight"
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
ms.openlocfilehash: 0738f33df94d14fbb393bfaaf09e94c6560213cc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="windows-phone-silverlight-reach-sdk-integration"></a><span data-ttu-id="accd8-103">Windows Phone Silverlight Reach integraci sady SDK</span><span class="sxs-lookup"><span data-stu-id="accd8-103">Windows Phone Silverlight Reach SDK Integration</span></span>
<span data-ttu-id="accd8-104">Je třeba provést postup integrace popsaný v tématu [integraci sady Windows Phone Silverlight Engagement SDK](mobile-engagement-windows-phone-integrate-engagement.md) před těchto pokynů.</span><span class="sxs-lookup"><span data-stu-id="accd8-104">You must follow the integration procedure described in the [Windows Phone Silverlight Engagement SDK Integration](mobile-engagement-windows-phone-integrate-engagement.md) before following this guide.</span></span>

## <a name="embed-the-engagement-reach-sdk-into-your-windows-phone-silverlight-project"></a><span data-ttu-id="accd8-105">Vložení Engagement Reach SDK do projektu Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="accd8-105">Embed the Engagement Reach SDK into your Windows Phone Silverlight project</span></span>
<span data-ttu-id="accd8-106">Nemáte nic přidat.</span><span class="sxs-lookup"><span data-stu-id="accd8-106">You do not have anything to add.</span></span> <span data-ttu-id="accd8-107">`EngagementReach`odkazy a prostředky jsou už ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="accd8-107">`EngagementReach` references and resources are already in your project.</span></span>

> [!TIP]
> <span data-ttu-id="accd8-108">Můžete přizpůsobit bitové kopie, které jsou umístěné v `Resources` složky projektu, zejména ikonu značky (této výchozí ikonu Engagement).</span><span class="sxs-lookup"><span data-stu-id="accd8-108">You can customize images located in the `Resources` folder of your project, especially the brand icon (that default to the Engagement icon).</span></span>
> 
> 

## <a name="add-the-capabilities"></a><span data-ttu-id="accd8-109">Přidání funkcí</span><span class="sxs-lookup"><span data-stu-id="accd8-109">Add the capabilities</span></span>
<span data-ttu-id="accd8-110">Engagement Reach SDK potřebuje některé další funkce.</span><span class="sxs-lookup"><span data-stu-id="accd8-110">The Engagement Reach SDK needs some additional capabilities.</span></span>

<span data-ttu-id="accd8-111">Otevřete váš `WMAppManifest.xml` souboru a ujistěte se, že jsou deklarovány následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="accd8-111">Open your `WMAppManifest.xml` file and be sure that the following capabilities are declared:</span></span>

* `ID_CAP_PUSH_NOTIFICATION`
* `ID_CAP_WEBBROWSERCOMPONENT`

<span data-ttu-id="accd8-112">První z nich se používá služba MPNS povolit zobrazení oznámení informační zprávy.</span><span class="sxs-lookup"><span data-stu-id="accd8-112">The first one is used by the MPNS service to allow the display of toast notification.</span></span> <span data-ttu-id="accd8-113">Druhá slouží k vložení úlohu prohlížeče do sady SDK.</span><span class="sxs-lookup"><span data-stu-id="accd8-113">The second one is used to embed a browser task into the SDK.</span></span>

<span data-ttu-id="accd8-114">Upravit `WMAppManifest.xml` souboru a přidejte uvnitř `<Capabilities />` značky:</span><span class="sxs-lookup"><span data-stu-id="accd8-114">Edit the `WMAppManifest.xml` file and add inside the `<Capabilities />` tag :</span></span>

    <Capability Name="ID_CAP_PUSH_NOTIFICATION" />
    <Capability Name="ID_CAP_WEBBROWSERCOMPONENT" />

## <a name="enable-the-microsoft-push-notification-service"></a><span data-ttu-id="accd8-115">Povolit službu Microsoft Push Notification Service</span><span class="sxs-lookup"><span data-stu-id="accd8-115">Enable the Microsoft Push Notification Service</span></span>
<span data-ttu-id="accd8-116">Chcete-li použít **Microsoft službu nabízených oznámení** (označované jako MPNS) vaší `WMAppManifest.xml` soubor musí mít `<App />` značku s `Publisher` atributu nastavena na název projektu.</span><span class="sxs-lookup"><span data-stu-id="accd8-116">In order to use the **Microsoft Push Notification Service** (referred as MPNS) your `WMAppManifest.xml` file must have an `<App />` tag with a `Publisher` attribute set to the name of your project.</span></span>

## <a name="initialize-the-engagement-reach-sdk"></a><span data-ttu-id="accd8-117">Inicializace Engagement Reach SDK</span><span class="sxs-lookup"><span data-stu-id="accd8-117">Initialize the Engagement Reach SDK</span></span>
### <a name="engagement-configuration"></a><span data-ttu-id="accd8-118">Konfigurace zapojení</span><span class="sxs-lookup"><span data-stu-id="accd8-118">Engagement configuration</span></span>
<span data-ttu-id="accd8-119">Konfigurace zapojení je centralizovaná v `Resources\EngagementConfiguration.xml` souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="accd8-119">The Engagement configuration is centralized in the `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="accd8-120">Upravte tento soubor k určení reach konfigurace:</span><span class="sxs-lookup"><span data-stu-id="accd8-120">Edit this file to specify reach configuration :</span></span>

* <span data-ttu-id="accd8-121">*Volitelné*, zda je aktivována nativního nabízení (MPNS) nebo není mezi `<enableNativePush>` a `</enableNativePush>` značky, (`true` ve výchozím nastavení).</span><span class="sxs-lookup"><span data-stu-id="accd8-121">*Optional*, indicate whether the native push (MPNS) is activated or not between `<enableNativePush>` and `</enableNativePush>` tags, (`true` by default).</span></span>
* <span data-ttu-id="accd8-122">*Volitelné*, označení názvu nabízené kanál mezi `<channelName>` a `</channelName>` značky, poskytují stejné, vaše aplikace může aktuálně použít nebo hodnotu nevyplňujte.</span><span class="sxs-lookup"><span data-stu-id="accd8-122">*Optional*, indicate the name of the push channel between `<channelName>` and `</channelName>` tags, provide the same that your application may currently use or leave it empty.</span></span>

<span data-ttu-id="accd8-123">Pokud chcete zadat za běhu, můžete volat metodu před Engagement inicializaci agenta:</span><span class="sxs-lookup"><span data-stu-id="accd8-123">If you want to specify it at runtime instead, you can call the following method before the Engagement agent initialization :</span></span>

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    engagementConfiguration.Reach.EnableNativePush = true;                  
    /* [Optional] whether the native push (MPNS) is activated or not. */

    engagementConfiguration.Reach.ChannelName = "YOUR_PUSH_CHANNEL_NAME";   
    /* [Optional] Provide the same channel name that your application may currently use. */

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

> [!TIP]
> <span data-ttu-id="accd8-124">Můžete zadat název kanálu nabízené MPNS vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="accd8-124">You can specify the name of the MPNS push channel of your application.</span></span> <span data-ttu-id="accd8-125">Ve výchozím nastavení vytvoří název založený na appId zapojení.</span><span class="sxs-lookup"><span data-stu-id="accd8-125">By default, Engagement creates a name based on the appId.</span></span> <span data-ttu-id="accd8-126">Nemáte žádné potřeba zadat název sami, s výjimkou Pokud plánujete použít kanál nabízené mimo zapojení.</span><span class="sxs-lookup"><span data-stu-id="accd8-126">You have no need to specify the name yourself, except if you plan to use the push channel outside of Engagement.</span></span>
> 
> 

### <a name="engagement-initialization"></a><span data-ttu-id="accd8-127">Inicializace zapojení</span><span class="sxs-lookup"><span data-stu-id="accd8-127">Engagement initialization</span></span>
<span data-ttu-id="accd8-128">Změnit `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="accd8-128">Modify the `App.xaml.cs`:</span></span>

* <span data-ttu-id="accd8-129">Přidat do vaší `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="accd8-129">Add to your `using` statements :</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="accd8-130">Vložit `EngagementReach.Instance.Init` právě po `EngagementAgent.Instance.Init` v `Application_Launching` :</span><span class="sxs-lookup"><span data-stu-id="accd8-130">Insert `EngagementReach.Instance.Init` just after `EngagementAgent.Instance.Init` in `Application_Launching` :</span></span>
  
      private void Application_Launching(object sender, LaunchingEventArgs e)
      {
         EngagementAgent.Instance.Init();
         EngagementReach.Instance.Init();
      }
* <span data-ttu-id="accd8-131">Vložit `EngagementReach.Instance.OnActivated` v `Application_Activated` metoda:</span><span class="sxs-lookup"><span data-stu-id="accd8-131">Insert `EngagementReach.Instance.OnActivated` in the `Application_Activated` method :</span></span>
  
      private void Application_Activated(object sender, ActivatedEventArgs e)
      {
         EngagementAgent.Instance.OnActivated(e);
         EngagementReach.Instance.OnActivated(e);
      }

> [!IMPORTANT]
> <span data-ttu-id="accd8-132">`EngagementReach.Instance.Init` Běží ve vyhrazené vlákno.</span><span class="sxs-lookup"><span data-stu-id="accd8-132">The `EngagementReach.Instance.Init` runs in a dedicated thread.</span></span> <span data-ttu-id="accd8-133">Nemusíte dělat sami.</span><span class="sxs-lookup"><span data-stu-id="accd8-133">You do not have to do it yourself.</span></span>
> 
> 

## <a name="app-store-submission-considerations"></a><span data-ttu-id="accd8-134">Aspekty odeslání úložiště aplikací</span><span class="sxs-lookup"><span data-stu-id="accd8-134">App store submission considerations</span></span>
<span data-ttu-id="accd8-135">Microsoft ukládá některá pravidla při používání nabízených oznámení:</span><span class="sxs-lookup"><span data-stu-id="accd8-135">Microsoft imposes some rules when using the push notifications:</span></span>

<span data-ttu-id="accd8-136">Z Microsoft [zásady aplikací] části 2.9, dokumentace:</span><span class="sxs-lookup"><span data-stu-id="accd8-136">From the Microsoft [Application Policies] documentation, section 2.9:</span></span>

1) <span data-ttu-id="accd8-137">Musíte požádat uživatele tak, aby přijímal přijímat nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="accd8-137">You must ask the user to accept to receive push notifications.</span></span> <span data-ttu-id="accd8-138">Potom si v nastaveních, přidejte způsob, jak zakázat nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="accd8-138">Then, in your settings, add a way to disable the push notifications.</span></span>

<span data-ttu-id="accd8-139">Objekt EngagementReach nabízí dvě metody pro správu opt souhlas/nesouhlas, `EnableNativePush()` a `DisableNativePush()`.</span><span class="sxs-lookup"><span data-stu-id="accd8-139">The EngagementReach object provides two methods to manage the opt-in/opt-out, `EnableNativePush()` and `DisableNativePush()`.</span></span> <span data-ttu-id="accd8-140">Možnost může například vytvořit v nastavení s přepínání zakázat nebo povolit MPNS.</span><span class="sxs-lookup"><span data-stu-id="accd8-140">You could, for example, create an option in the settings with a toggle to disable or enable MPNS.</span></span>

<span data-ttu-id="accd8-141">Můžete také rozhodnout deaktivovat MPNS prostřednictvím konfigurace Engagement\<windows phone-sdk-reach konfigurace\>.</span><span class="sxs-lookup"><span data-stu-id="accd8-141">You can also decide to deactivate MPNS through the Engagement configuration\<windows-phone-sdk-reach-configuration\>.</span></span>

> <span data-ttu-id="accd8-142">2.9.1) aplikace musí nejprve popisují oznámení, musíte zadat a **získat express oprávnění (výslovný souhlas)**, a **mechanismus, pomocí kterého uživatel může vyjádření výslovného nesouhlasu s nabízená oznámení musíte zadat**.</span><span class="sxs-lookup"><span data-stu-id="accd8-142">2.9.1) The application must first describe the notifications to be provided and **obtain the user’s express permission (opt-in)**, and **must provide a mechanism through which the user can opt out of receiving push notifications**.</span></span> <span data-ttu-id="accd8-143">Všechna oznámení zadaný pomocí Microsoft službu nabízených oznámení musí být v souladu s uživateli poskytnutý popis a musí dodržovat všechny příslušné [zásady aplikací] [ Content Policies] a [další požadavky pro konkrétní typy aplikací].</span><span class="sxs-lookup"><span data-stu-id="accd8-143">All notifications provided using the Microsoft Push Notification Service must be consistent with the description provided to the user and must comply with all applicable [Application Policies][Content Policies] and [Additional Requirements for Specific Application Types].</span></span>
> 
> 

2) <span data-ttu-id="accd8-144">Neměli byste používat příliš mnoho nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="accd8-144">You should not use too many push notifications.</span></span> <span data-ttu-id="accd8-145">Zapojení bude zpracovávat oznámení za vás.</span><span class="sxs-lookup"><span data-stu-id="accd8-145">Engagement will handle notifications for you.</span></span>

> <span data-ttu-id="accd8-146">2.9.2) aplikace a jeho použití programu Microsoft službu nabízených oznámení nesmí nadměrně použít dostatečnou kapacitu sítě nebo šířka pásma Microsoft službu nabízených oznámení, nebo jinak neoprávněně nebyli Windows Phone nebo jiné zařízení Microsoft nebo služby s nadměrné nabízená oznámení, jak stanoví společnost Microsoft dle svého uvážení a musí poškodit nebo narušit žádné sítě, Microsoft nebo servery nebo serverech třetích stran nebo sítě připojené k Microsoft službu nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="accd8-146">2.9.2) The application and its use of the Microsoft Push Notification Service must not excessively use network capacity or bandwidth of the Microsoft Push Notification Service, or otherwise unduly burden a Windows Phone or other Microsoft device or service with excessive push notifications, as determined by Microsoft in its reasonable discretion, and must not harm or interfere with any Microsoft networks or servers or any third party servers or networks connected to the Microsoft Push Notification Service.</span></span>
> 
> 

3) <span data-ttu-id="accd8-147">Nespoléhejte na MPNS k odeslání informací o criticals.</span><span class="sxs-lookup"><span data-stu-id="accd8-147">Do not rely on MPNS to send criticals information.</span></span> <span data-ttu-id="accd8-148">Zapojení používá MPNS, takže toto pravidlo platí také pro kampaně vytvářet v zapojení front-endu.</span><span class="sxs-lookup"><span data-stu-id="accd8-148">Engagement uses MPNS, so this rule also applies for the campaigns created inside the Engagement front-end.</span></span>

> <span data-ttu-id="accd8-149">Microsoft službu nabízených oznámení 2.9.3) nemusí být používá k odesílání oznámení, které jsou zvláště důležité nebo jinak mohou ovlivnit záležitosti životnosti nebo úmrtí, včetně bez omezení kritické oznámení souvisejících s lékařské zařízení nebo podmínku.</span><span class="sxs-lookup"><span data-stu-id="accd8-149">2.9.3) The Microsoft Push Notification Service may not be used to send notifications that are mission critical or otherwise could affect matters of life or death, including without limitation critical notifications related to a medical device or condition.</span></span> <span data-ttu-id="accd8-150">MICROSOFT VÝSLOVNĚ ZŘÍKÁ JAKÉKOLI ZÁRUKY, POUŽITÍ MICROSOFT NABÍZENÁ OZNÁMENÍ SLUŽBY NEBO DORUČOVÁNÍ OZNÁMENÍ O SLUŽBÁCH MICROSOFT NABÍZENÁ OZNÁMENÍ BUDE BEZ PŘERUŠENÍ, BEZ CHYB, NEBO JINAK ZÁRUKU, ŽE DOJDE K NA ZÁKLADĚ V REÁLNÉM ČASE.</span><span class="sxs-lookup"><span data-stu-id="accd8-150">MICROSOFT EXPRESSLY DISCLAIMS ANY WARRANTIES THAT THE USE OF THE MICROSOFT PUSH NOTIFICATION SERVICE OR DELIVERY OF MICROSOFT PUSH NOTIFICATION SERVICE NOTIFICATIONS WILL BE UNINTERRUPTED, ERROR FREE, OR OTHERWISE GUARANTEED TO OCCUR ON A REAL-TIME BASIS.</span></span>
> 
> 

<span data-ttu-id="accd8-151">**Nemůžeme zaručit, že vaše aplikace předá proces ověření Pokud nerespektují tato doporučení.**</span><span class="sxs-lookup"><span data-stu-id="accd8-151">**We cannot guarantee that your application will pass the validation process if you do not respect these recommendations.**</span></span>

## <a name="handle-data-push-optional"></a><span data-ttu-id="accd8-152">Zpracování datová oznámení (volitelné)</span><span class="sxs-lookup"><span data-stu-id="accd8-152">Handle data push (optional)</span></span>
<span data-ttu-id="accd8-153">Pokud chcete aplikace nebudou moct přijímat Reach datová oznámení, je nutné implementovat dvě události EngagementReach třídy:</span><span class="sxs-lookup"><span data-stu-id="accd8-153">If you want your application to be able to receive Reach data pushes, you have to implement two events of the EngagementReach class:</span></span>

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

<span data-ttu-id="accd8-154">Uvidíte, že zpětné volání každá metoda vrátí logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="accd8-154">You can see that the callback of each method returns a boolean.</span></span> <span data-ttu-id="accd8-155">Zapojení odešle zpětnou vazbu k jeho back-end po odeslání nabízeného oznámení data.</span><span class="sxs-lookup"><span data-stu-id="accd8-155">Engagement sends a feedback to its back-end after dispatching the data push.</span></span> <span data-ttu-id="accd8-156">Pokud vrátí hodnotu false, zpětné volání `exit` zpětné vazby bude odesílat.</span><span class="sxs-lookup"><span data-stu-id="accd8-156">If the callback returns false, the `exit` feedback will be send.</span></span> <span data-ttu-id="accd8-157">Jinak bude `action`.</span><span class="sxs-lookup"><span data-stu-id="accd8-157">Otherwise, it will be `action`.</span></span> <span data-ttu-id="accd8-158">Pokud je pro události, nastavené žádné zpětného volání `drop` zapojení se vrátí zpětnou vazbu.</span><span class="sxs-lookup"><span data-stu-id="accd8-158">If no callback is set for the events, the `drop` feedback will be returned to Engagement.</span></span>

> [!WARNING]
> <span data-ttu-id="accd8-159">Zapojení není schopný přijímat násobky názory pro datová oznámení.</span><span class="sxs-lookup"><span data-stu-id="accd8-159">Engagement is not able to receive multiples feedbacks for a data push.</span></span> <span data-ttu-id="accd8-160">Pokud budete chtít nastavit několik obslužné rutiny na události, uvědomte si, zda bude poslední odpovídají zpětné vazby jeden odeslána.</span><span class="sxs-lookup"><span data-stu-id="accd8-160">If you plan to set several handlers on an event, be aware that the feedback will correspond to the last one sent.</span></span> <span data-ttu-id="accd8-161">V takovém případě doporučujeme vždy vrací stejnou hodnotu, abyste nemuseli matoucí zpětnou vazbu na front-endu.</span><span class="sxs-lookup"><span data-stu-id="accd8-161">In this case, we recommend to always returns the same value to avoid having confusing feedback on the front-end.</span></span>
> 
> 

## <a name="customize-ui-optional"></a><span data-ttu-id="accd8-162">Přizpůsobení uživatelského rozhraní (volitelné)</span><span class="sxs-lookup"><span data-stu-id="accd8-162">Customize UI (optional)</span></span>
### <a name="first-step"></a><span data-ttu-id="accd8-163">Prvním krokem</span><span class="sxs-lookup"><span data-stu-id="accd8-163">First step</span></span>
<span data-ttu-id="accd8-164">Můžeme vám umožňují přizpůsobit reach uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="accd8-164">We allow you to customize the reach UI.</span></span>

<span data-ttu-id="accd8-165">Uděláte to tak, budete muset vytvořit podtřídou třídy `EngagementReachHandler` třídy.</span><span class="sxs-lookup"><span data-stu-id="accd8-165">To do so, you have to create a subclass of the `EngagementReachHandler` class.</span></span>

<span data-ttu-id="accd8-166">**Ukázkový kód:**</span><span class="sxs-lookup"><span data-stu-id="accd8-166">**Sample Code :**</span></span>

    using Microsoft.Azure.Engagement;

    namespace Example
    {
       internal class ExampleReachHandler : EngagementReachHandler
       {
          // Override EngagementReachHandler methods depending on your needs
       }
    }

<span data-ttu-id="accd8-167">Potom nastavte obsah `EngagementReach.Instance.Handler` pole s vaší vlastní objekt v vaše `App.xaml.cs` třídy v rámci `Application_Launching` metoda.</span><span class="sxs-lookup"><span data-stu-id="accd8-167">Then, set the content of the `EngagementReach.Instance.Handler` field with your custom object in your `App.xaml.cs` class within the `Application_Launching` method.</span></span>

<span data-ttu-id="accd8-168">**Ukázkový kód:**</span><span class="sxs-lookup"><span data-stu-id="accd8-168">**Sample Code :**</span></span>

    private void Application_Launching(object sender, LaunchingEventArgs e)
    {
       // your app initialization 
       EngagementReach.Instance.Handler = new ExampleReachHandler();
       // Engagement Agent and Reach initialization
    }

> [!NOTE]
> <span data-ttu-id="accd8-169">Ve výchozím nastavení používá Engagement jejich vlastní implementaci `EngagementReachHandler`.</span><span class="sxs-lookup"><span data-stu-id="accd8-169">By default, Engagement uses its own implementation of `EngagementReachHandler`.</span></span> <span data-ttu-id="accd8-170">Nemusíte vytvářet vlastní, a pokud tak učiníte, nemusíte přepsat každou metodu.</span><span class="sxs-lookup"><span data-stu-id="accd8-170">You don't have to create your own, and if you do so, you don't have to override every method.</span></span> <span data-ttu-id="accd8-171">Výchozí chování je výběr základní objekt zapojení.</span><span class="sxs-lookup"><span data-stu-id="accd8-171">The default behavior is to select the Engagement base object.</span></span>
> 
> 

### <a name="layouts"></a><span data-ttu-id="accd8-172">Rozložení</span><span class="sxs-lookup"><span data-stu-id="accd8-172">Layouts</span></span>
<span data-ttu-id="accd8-173">Ve výchozím nastavení použije Reach vložené prostředky knihovny DLL k zobrazení oznámení a stránky.</span><span class="sxs-lookup"><span data-stu-id="accd8-173">By default, Reach will use the embedded resources of the DLL to display the notifications and pages.</span></span>

<span data-ttu-id="accd8-174">Můžete však použít vlastní prostředky tak, aby odrážela vaší značkou v těchto součástí.</span><span class="sxs-lookup"><span data-stu-id="accd8-174">However, you can decide to use your own resources to reflect your brand in these components.</span></span>

<span data-ttu-id="accd8-175">Můžete přepsat `EngagementReachHandler` metody v vaše podtřída říct Engagement používat vaše rozložení:</span><span class="sxs-lookup"><span data-stu-id="accd8-175">You can override `EngagementReachHandler` methods in your subclass to tell Engagement to use your layouts :</span></span>

<span data-ttu-id="accd8-176">**Ukázkový kód:**</span><span class="sxs-lookup"><span data-stu-id="accd8-176">**Sample Code :**</span></span>

    // In your subclass of EngagementReachHandler

    public override string GetTextViewAnnouncementUri()
    {
       // return the path of your own xaml
    }

    public override string GetWebViewAnnouncementUri()
    {
       // return the path of your own xaml
    }

    public override string GetPollUri()
    {
       // return the path of your own xaml
    }

    public override EngagementNotificationView CreateNotification(EngagementNotificationViewModel viewModel)
    {
       // return a new instance of your own notification
    }

> [!TIP]
> <span data-ttu-id="accd8-177">`CreateNotification` Metoda může vrátit hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="accd8-177">The `CreateNotification` method can return null.</span></span> <span data-ttu-id="accd8-178">Oznámení se nezobrazí a budou vynechána kampaně reach.</span><span class="sxs-lookup"><span data-stu-id="accd8-178">The notification won't be displayed and the reach campaign will be dropped.</span></span>
> 
> 

<span data-ttu-id="accd8-179">Pro zjednodušení implementace rozložení, poskytujeme také vlastní xaml, který může sloužit jako základ pro váš kód.</span><span class="sxs-lookup"><span data-stu-id="accd8-179">To simplify your layout implementation, we also provide our own xaml which can serve as a basis for your code.</span></span> <span data-ttu-id="accd8-180">Se nacházejí v archivu Engagement SDK (/ src/reach /).</span><span class="sxs-lookup"><span data-stu-id="accd8-180">They are located in the Engagement SDK archive (/src/reach/).</span></span>

> [!WARNING]
> <span data-ttu-id="accd8-181">Zdroje, které poskytujeme jsou přesně stejnou ty, které používáme.</span><span class="sxs-lookup"><span data-stu-id="accd8-181">The sources that we provide are the exact same ones that we use.</span></span> <span data-ttu-id="accd8-182">Takže pokud chcete změnit přímo, nezapomeňte změnit obor názvů a název.</span><span class="sxs-lookup"><span data-stu-id="accd8-182">So if you want to modify them directly, don't forget to change the namespace and the name.</span></span>
> 
> 

### <a name="notification-position"></a><span data-ttu-id="accd8-183">Pozice oznámení</span><span class="sxs-lookup"><span data-stu-id="accd8-183">Notification position</span></span>
<span data-ttu-id="accd8-184">Ve výchozím nastavení zobrazí se ve spodní levé straně aplikace oznámení v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="accd8-184">By default, an in-app notification is displayed at the bottom left side of the application.</span></span> <span data-ttu-id="accd8-185">Toto chování můžete změnit přepsání `GetNotificationPosition` metodu `EngagementReachHandler` objektu.</span><span class="sxs-lookup"><span data-stu-id="accd8-185">You can change this behavior by overriding the `GetNotificationPosition` method of the `EngagementReachHandler` object.</span></span>

    // In your subclass of EngagementReachHandler

    public override EngagementReachHandler.NotificationPosition GetNotificationPosition(EngagementNotificationViewModel viewModel)
    {
       // return a value of the EngagementReachHandler.NotificationPosition enum (TOP or BOTTOM)
    }

<span data-ttu-id="accd8-186">V současné době můžete zvolit, jestli `BOTTOM` (výchozí) a `TOP` pozic.</span><span class="sxs-lookup"><span data-stu-id="accd8-186">Currently, you can choose between the `BOTTOM` (default) and `TOP` positions.</span></span>

### <a name="launch-message"></a><span data-ttu-id="accd8-187">Spusťte zprávu</span><span class="sxs-lookup"><span data-stu-id="accd8-187">Launch message</span></span>
<span data-ttu-id="accd8-188">Když uživatel klikne na systémové oznámení (oznámení), Engagement spuštění aplikace, obsah zprávy nabízené načíst a zobrazit stránku pro odpovídající kampaně.</span><span class="sxs-lookup"><span data-stu-id="accd8-188">When a user clicks on a system notification (a toast), Engagement launches the app, load the content of the push messages, and display the page for the corresponding campaign.</span></span>

<span data-ttu-id="accd8-189">Dochází ke zpoždění mezi spuštění aplikace a zobrazení stránky (v závislosti na rychlosti sítě).</span><span class="sxs-lookup"><span data-stu-id="accd8-189">There is a delay between the launch of the application and the display of the page (depending on the speed of your network).</span></span>

<span data-ttu-id="accd8-190">Chcete-li uživatele upozornit, načítání něco, by měl poskytovat vizuální informace, jako je indikátor průběhu nebo indikátor průběhu.</span><span class="sxs-lookup"><span data-stu-id="accd8-190">To indicate to the user that something is loading, you should provide a visual information, like a progress bar or a progress indicator.</span></span> <span data-ttu-id="accd8-191">Zapojení nemůže zpracovat samostatně, ale poskytuje několik obslužné rutiny pro vás.</span><span class="sxs-lookup"><span data-stu-id="accd8-191">Engagement cannot handle that itself, but provides a few handlers for you.</span></span>

<span data-ttu-id="accd8-192">Chcete-li implementovat zpětné volání, proveďte:</span><span class="sxs-lookup"><span data-stu-id="accd8-192">To implement the callback, do:</span></span>

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

<span data-ttu-id="accd8-193">Zpětné volání můžete nastavit v vaše `Application_Launching` metodu vaše `App.xaml.cs` soubor, pokud možno před `EngagementReach.Instance.Init()` volání.</span><span class="sxs-lookup"><span data-stu-id="accd8-193">You can set the callback in your `Application_Launching` method of your `App.xaml.cs` file, preferably before the `EngagementReach.Instance.Init()` call.</span></span>

> [!TIP]
> <span data-ttu-id="accd8-194">Každý obslužná rutina je volána službou vlákna uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="accd8-194">Each handler is called by the UI Thread.</span></span> <span data-ttu-id="accd8-195">Nemusíte si dělat starosti při použití a MessageBox nebo něco související uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="accd8-195">You do not have to worry when using a MessageBox or something UI-related.</span></span>
> 
> 

<span data-ttu-id="accd8-196">[zásady aplikací]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx</span><span class="sxs-lookup"><span data-stu-id="accd8-196">[Application Policies]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx</span></span>
[Content Policies]:http://msdn.microsoft.com/library/windows/apps/hh184842(v=vs.105).aspx
<span data-ttu-id="accd8-197">[další požadavky pro konkrétní typy aplikací]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx</span><span class="sxs-lookup"><span data-stu-id="accd8-197">[Additional Requirements for Specific Application Types]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx</span></span>

