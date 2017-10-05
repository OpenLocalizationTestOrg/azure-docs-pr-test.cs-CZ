---
title: Integraci sady Windows Phone Silverlight Engagement SDK
description: "Postup při integraci Azure Mobile Engagement s aplikacemi pro Windows Phone Silverlight"
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
ms.openlocfilehash: 29b18aecff783cebf617995e2a19f16f0b68b51b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="windows-phone-silverlight-engagement-sdk-integration"></a><span data-ttu-id="deb59-103">Integraci sady Windows Phone Silverlight Engagement SDK</span><span class="sxs-lookup"><span data-stu-id="deb59-103">Windows Phone Silverlight Engagement SDK Integration</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="deb59-104">Univerzální pro Windows</span><span class="sxs-lookup"><span data-stu-id="deb59-104">Windows Universal</span></span>](mobile-engagement-windows-store-integrate-engagement.md) 
> * [<span data-ttu-id="deb59-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="deb59-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md) 
> * [<span data-ttu-id="deb59-106">iOS</span><span class="sxs-lookup"><span data-stu-id="deb59-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md) 
> * [<span data-ttu-id="deb59-107">Android</span><span class="sxs-lookup"><span data-stu-id="deb59-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md) 
> 
> 

<span data-ttu-id="deb59-108">Tento postup popisuje nejjednodušší způsob, jak aktivovat Azure Mobile Engagement analýzy a monitorování funkce v aplikaci Silverlight pro Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="deb59-108">This procedure describes the simplest way to activate Azure Mobile Engagement's Analytics and Monitoring functions in your Windows Phone Silverlight application.</span></span>

<span data-ttu-id="deb59-109">Následující kroky jsou dost aktivovat sestavy protokolů, které jsou potřebné k výpočtu všechny statistické údaje týkající se uživatelů, relací, aktivity, dojde k chybě a Technicals.</span><span class="sxs-lookup"><span data-stu-id="deb59-109">The following steps are enough to activate the report of logs needed to compute all statistics regarding Users, Sessions, Activities, Crashes and Technicals.</span></span> <span data-ttu-id="deb59-110">Sestavy protokolů, které jsou potřebné k výpočtu další statistiky, jako jsou události a chyby úlohy je třeba provést ručně pomocí rozhraní API Engagement (najdete v části [jak používat rozšířené Mobile Engagement označování rozhraní API v aplikaci Windows Phone Silverlight](mobile-engagement-windows-phone-use-engagement-api.md) níže) vzhledem k tomu, že tyto statistické údaje jsou závislé aplikace.</span><span class="sxs-lookup"><span data-stu-id="deb59-110">The report of logs needed to compute other statistics like Events, Errors and Jobs must be done manually using the Engagement API (see [How to use the advanced Mobile Engagement tagging API in your Windows Phone Silverlight app](mobile-engagement-windows-phone-use-engagement-api.md) below) since these statistics are application-dependent.</span></span>

## <a name="supported-versions"></a><span data-ttu-id="deb59-111">Podporované verze</span><span class="sxs-lookup"><span data-stu-id="deb59-111">Supported versions</span></span>
<span data-ttu-id="deb59-112">Mobile Engagement SDK pro Windows aplikace Silverlight lze pouze integrovat do aplikace cílený na:</span><span class="sxs-lookup"><span data-stu-id="deb59-112">The Mobile Engagement SDK for Windows Silverlight can only be integrated into applications targeting :</span></span>

* <span data-ttu-id="deb59-113">Windows Phone 8.0</span><span class="sxs-lookup"><span data-stu-id="deb59-113">Windows Phone 8.0</span></span>
* <span data-ttu-id="deb59-114">Windows Phone 8.1 Silverlight</span><span class="sxs-lookup"><span data-stu-id="deb59-114">Windows Phone 8.1 Silverlight</span></span>

> [!NOTE]
> <span data-ttu-id="deb59-115">Pokud je cílem Windows Phone 8.1 (bez Silverlight) můžete odkazovat [univerzální pro Windows postup integrace](mobile-engagement-windows-store-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="deb59-115">If you are targeting Windows Phone 8.1 (non-Silverlight) then refer to the [Windows Universal integration procedure](mobile-engagement-windows-store-integrate-engagement.md).</span></span>
> 
> 

## <a name="install-the-mobile-engagement-silverlight-sdk"></a><span data-ttu-id="deb59-116">Nainstalovat sadu SDK Mobile Engagement Silverlight</span><span class="sxs-lookup"><span data-stu-id="deb59-116">Install the Mobile Engagement Silverlight SDK</span></span>
<span data-ttu-id="deb59-117">Mobile Engagement SDK pro Windows aplikace Silverlight je k dispozici jako balíček Nuget s názvem *MicrosoftAzure.MobileEngagement*.</span><span class="sxs-lookup"><span data-stu-id="deb59-117">The Mobile Engagement SDK for Windows Silverlight is available as a Nuget package called *MicrosoftAzure.MobileEngagement*.</span></span> <span data-ttu-id="deb59-118">Můžete ho nainstalovat z Visual Studio Správce balíčků Nuget.</span><span class="sxs-lookup"><span data-stu-id="deb59-118">You can install it from the Visual Studio Nuget Package Manager.</span></span> 

## <a name="add-the-capabilities"></a><span data-ttu-id="deb59-119">Přidání funkcí</span><span class="sxs-lookup"><span data-stu-id="deb59-119">Add the capabilities</span></span>
<span data-ttu-id="deb59-120">Sady Engagement SDK potřebuje některé funkce sady Windows Phone Silverlight SDK správné fungování.</span><span class="sxs-lookup"><span data-stu-id="deb59-120">The Engagement SDK needs some capabilities of the Windows Phone Silverlight SDK in order to work properly.</span></span>

<span data-ttu-id="deb59-121">Otevřete váš `WMAppManifest.xml` souboru a ujistěte se, že tyto možnosti jsou deklarované v `Capabilities` panelu:</span><span class="sxs-lookup"><span data-stu-id="deb59-121">Open your `WMAppManifest.xml` file and be sure that the following capabilities are declared in the `Capabilities` panel:</span></span>

* `ID_CAP_NETWORKING`
* `ID_CAP_IDENTITY_DEVICE`

## <a name="initialize-the-engagement-sdk"></a><span data-ttu-id="deb59-122">Inicializace Engagement SDK</span><span class="sxs-lookup"><span data-stu-id="deb59-122">Initialize the Engagement SDK</span></span>
### <a name="engagement-configuration"></a><span data-ttu-id="deb59-123">Konfigurace zapojení</span><span class="sxs-lookup"><span data-stu-id="deb59-123">Engagement configuration</span></span>
<span data-ttu-id="deb59-124">Konfigurace zapojení je centralizovaná v `Resources\EngagementConfiguration.xml` souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="deb59-124">The Engagement configuration is centralized in the `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="deb59-125">Upravte tento soubor k určení:</span><span class="sxs-lookup"><span data-stu-id="deb59-125">Edit this file to specify :</span></span>

* <span data-ttu-id="deb59-126">Připojovací řetězec aplikací mezi značky `<connectionString>` a `<\connectionString>`.</span><span class="sxs-lookup"><span data-stu-id="deb59-126">Your application connection string between tags `<connectionString>` and `<\connectionString>`.</span></span>

<span data-ttu-id="deb59-127">Pokud chcete zadat za běhu, můžete volat metodu před Engagement inicializaci agenta:</span><span class="sxs-lookup"><span data-stu-id="deb59-127">If you want to specify it at runtime instead, you can call the following method before the Engagement agent initialization:</span></span>

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

<span data-ttu-id="deb59-128">Připojovací řetězec pro vaši aplikaci se zobrazí na portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="deb59-128">The connection string for your application is displayed on the Azure Classic Portal.</span></span>

### <a name="engagement-initialization"></a><span data-ttu-id="deb59-129">Inicializace zapojení</span><span class="sxs-lookup"><span data-stu-id="deb59-129">Engagement initialization</span></span>
<span data-ttu-id="deb59-130">Když vytvoříte nový projekt, `App.xaml.cs` se vygeneruje soubor.</span><span class="sxs-lookup"><span data-stu-id="deb59-130">When you create a new project, a `App.xaml.cs` file is generated.</span></span> <span data-ttu-id="deb59-131">Tato třída dědí z `Application` a obsahuje mnoho důležité metody.</span><span class="sxs-lookup"><span data-stu-id="deb59-131">This class inherits from `Application` and contains many important methods.</span></span> <span data-ttu-id="deb59-132">Je také použije k chybě při inicializaci sady Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="deb59-132">It will also be used to initialize the Engagement SDK.</span></span>

<span data-ttu-id="deb59-133">Změnit `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="deb59-133">Modify the `App.xaml.cs`:</span></span>

* <span data-ttu-id="deb59-134">Přidat do vaší `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="deb59-134">Add to your `using` statements :</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="deb59-135">Vložit `EngagementAgent.Instance.Init` v `Application_Launching` metoda:</span><span class="sxs-lookup"><span data-stu-id="deb59-135">Insert `EngagementAgent.Instance.Init` in the `Application_Launching` method :</span></span>
  
      private void Application_Launching(object sender, LaunchingEventArgs e)
      {
        EngagementAgent.Instance.Init();
      }
* <span data-ttu-id="deb59-136">Vložit `EngagementAgent.Instance.OnActivated` v `Application_Activated` metoda:</span><span class="sxs-lookup"><span data-stu-id="deb59-136">Insert `EngagementAgent.Instance.OnActivated` in the `Application_Activated` method :</span></span>
  
      private void Application_Activated(object sender, ActivatedEventArgs e)
      {
         EngagementAgent.Instance.OnActivated(e);
      }

> [!WARNING]
> <span data-ttu-id="deb59-137">Jsme důrazně bránit můžete přidat inicializace Engagement na jiném místě vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="deb59-137">We strongly discourage you to add the Engagement initialization in another place of your application.</span></span> <span data-ttu-id="deb59-138">Ale pozor, který `EngagementAgent.Instance.Init` metoda se spouští na vyhrazené vlákno a ne na vlákna uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="deb59-138">However, be aware that the `EngagementAgent.Instance.Init` method runs on a dedicated thread, and not on the UI thread.</span></span>
> 
> 

## <a name="basic-reporting"></a><span data-ttu-id="deb59-139">Vytváření základních sestav</span><span class="sxs-lookup"><span data-stu-id="deb59-139">Basic reporting</span></span>
### <a name="recommended-method--overload-your-phoneapplicationpage-classes"></a><span data-ttu-id="deb59-140">Doporučená metoda: přetížení vaší `PhoneApplicationPage` třídy</span><span class="sxs-lookup"><span data-stu-id="deb59-140">Recommended method : overload your `PhoneApplicationPage` classes</span></span>
<span data-ttu-id="deb59-141">Chcete-li aktivovat sestavu všechny protokoly, které vyžadují zapojení k výpočtu uživatelů, relací, aktivity, dojde k chybě a technické statistiky, můžete jednoduše provést všechny vaše `PhoneApplicationPage` dílčí třídy dědí `EngagementPage` třídy.</span><span class="sxs-lookup"><span data-stu-id="deb59-141">In order to activate the report of all the logs required by Engagement to compute Users, Sessions, Activities, Crashes and Technical statistics, you can simply make all your `PhoneApplicationPage` sub-classes inherit from the `EngagementPage` classes.</span></span>

<span data-ttu-id="deb59-142">Tady je příklad toho, jak to udělat pro stránku vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="deb59-142">Here is an example of how to do this for a page of your application.</span></span> <span data-ttu-id="deb59-143">Můžete to samé pro všechny stránky vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="deb59-143">You can do the same thing for all pages of your application.</span></span>

#### <a name="c-source-file"></a><span data-ttu-id="deb59-144">C# zdrojový soubor</span><span class="sxs-lookup"><span data-stu-id="deb59-144">C# Source file</span></span>
<span data-ttu-id="deb59-145">Upravit stránku `.xaml.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="deb59-145">Modify your page `.xaml.cs` file :</span></span>

* <span data-ttu-id="deb59-146">Přidat do vaší `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="deb59-146">Add to your `using` statements :</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="deb59-147">Nahraďte `PhoneApplicationPage` s `EngagementPage` :</span><span class="sxs-lookup"><span data-stu-id="deb59-147">Replace `PhoneApplicationPage` with `EngagementPage` :</span></span>

<span data-ttu-id="deb59-148">**Bez Engagement:**</span><span class="sxs-lookup"><span data-stu-id="deb59-148">**Without Engagement:**</span></span>

        namespace Example
        {
          public partial class ExamplePage : PhoneApplicationPage
          {
            [...]
          }
        }

<span data-ttu-id="deb59-149">**S Engagement:**</span><span class="sxs-lookup"><span data-stu-id="deb59-149">**With Engagement:**</span></span>

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [!WARNING]
> <span data-ttu-id="deb59-150">Pokud stránka dědí z `OnNavigatedTo` metody dávejte pozor, abyste mohli `base.OnNavigatedTo(e)` volání.</span><span class="sxs-lookup"><span data-stu-id="deb59-150">If your page inherits from the `OnNavigatedTo` method, be careful to let the `base.OnNavigatedTo(e)` call.</span></span> <span data-ttu-id="deb59-151">Jinak nebudou ohlášena aktivity.</span><span class="sxs-lookup"><span data-stu-id="deb59-151">Otherwise, the activity will not be reported.</span></span> <span data-ttu-id="deb59-152">Ve skutečnosti `EngagementPage` volá `StartActivity` uvnitř `OnNavigatedTo` metoda.</span><span class="sxs-lookup"><span data-stu-id="deb59-152">Indeed, the `EngagementPage` is calling `StartActivity` inside the `OnNavigatedTo` method.</span></span>
> 
> 

#### <a name="xaml-file"></a><span data-ttu-id="deb59-153">Souboru XAML</span><span class="sxs-lookup"><span data-stu-id="deb59-153">XAML file</span></span>
<span data-ttu-id="deb59-154">Upravit stránku `.xaml` souboru:</span><span class="sxs-lookup"><span data-stu-id="deb59-154">Modify your page `.xaml` file :</span></span>

* <span data-ttu-id="deb59-155">Přidejte do deklarací oborů názvů:</span><span class="sxs-lookup"><span data-stu-id="deb59-155">Add to your namespaces declarations :</span></span>
  
      xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
* <span data-ttu-id="deb59-156">Nahraďte `phone:PhoneApplicationPage` s `engagement:EngagementPage` :</span><span class="sxs-lookup"><span data-stu-id="deb59-156">Replace `phone:PhoneApplicationPage` with `engagement:EngagementPage` :</span></span>

<span data-ttu-id="deb59-157">**Bez Engagement:**</span><span class="sxs-lookup"><span data-stu-id="deb59-157">**Without Engagement:**</span></span>

        <phone:PhoneApplicationPage>
            <!-- layout -->
        </phone:PhoneApplicationPage>

<span data-ttu-id="deb59-158">**S Engagement:**</span><span class="sxs-lookup"><span data-stu-id="deb59-158">**With Engagement:**</span></span>

        <engagement:EngagementPage 
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP">

            <!-- layout -->
        </engagement:EngagementPage >

#### <a name="override-the-default-behavior"></a><span data-ttu-id="deb59-159">Přepsat výchozí chování</span><span class="sxs-lookup"><span data-stu-id="deb59-159">Override the default behavior</span></span>
<span data-ttu-id="deb59-160">Ve výchozím nastavení je název třídy stránky uvedená jako název aktivity, s žádné další.</span><span class="sxs-lookup"><span data-stu-id="deb59-160">By default, the class name of the page is reported as the activity name, with no extra.</span></span> <span data-ttu-id="deb59-161">Pokud třída používá příponou "Stránka", Engagement také odebere.</span><span class="sxs-lookup"><span data-stu-id="deb59-161">If the class uses the "Page" suffix, Engagement will also remove it.</span></span>

<span data-ttu-id="deb59-162">Pokud chcete přepsat výchozí chování pro název, jednoduše přidejte tuto kódu:</span><span class="sxs-lookup"><span data-stu-id="deb59-162">If you want to override the default behavior for the name, simply add this to your code:</span></span>

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
           /* your code */
           return "new name";
        }

<span data-ttu-id="deb59-163">Pokud chcete ohlásit některé doplňující informace s vaší aktivitou, můžete to přidat do kódu:</span><span class="sxs-lookup"><span data-stu-id="deb59-163">If you want to report some extra information with your activity, you can add this to your code:</span></span>

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
           /* your code */
           return extra;
        }

<span data-ttu-id="deb59-164">Tyto metody jsou volány prostřednictvím `OnNavigatedTo` metoda stránky.</span><span class="sxs-lookup"><span data-stu-id="deb59-164">These methods are called from within the `OnNavigatedTo` method of your page.</span></span>

### <a name="alternate-method-call-startactivity-manually"></a><span data-ttu-id="deb59-165">Alternativní metoda: volání `StartActivity()` ručně</span><span class="sxs-lookup"><span data-stu-id="deb59-165">Alternate method: call `StartActivity()` manually</span></span>
<span data-ttu-id="deb59-166">Pokud nemůžete nebo nechcete přetížení vaše `PhoneApplicationPage` třídy, můžete místo toho spustit vaše aktivity voláním `EngagementAgent` metody přímo.</span><span class="sxs-lookup"><span data-stu-id="deb59-166">If you cannot or do not want to overload your `PhoneApplicationPage` classes, you can instead start your activities by calling `EngagementAgent` methods directly.</span></span>

<span data-ttu-id="deb59-167">Doporučujeme, abyste volání `StartActivity` uvnitř vaší `OnNavigatedTo` metoda vaší PhoneApplicationPage.</span><span class="sxs-lookup"><span data-stu-id="deb59-167">We recommend calling `StartActivity` inside your `OnNavigatedTo` method of your PhoneApplicationPage.</span></span>

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
           base.OnNavigatedTo(e);
           EngagementAgent.Instance.StartActivity("MyPage");
        }

> [!IMPORTANT]
> <span data-ttu-id="deb59-168">Zajistěte, aby že správně ukončete svou relaci.</span><span class="sxs-lookup"><span data-stu-id="deb59-168">Ensure you end your session correctly.</span></span>
> 
> <span data-ttu-id="deb59-169">Sada SDK automaticky zavolá `EndActivity` metoda při ukončení aplikace.</span><span class="sxs-lookup"><span data-stu-id="deb59-169">The SDK automatically calls the `EndActivity` method when the application is closed.</span></span> <span data-ttu-id="deb59-170">Z toho důvodu je **vysoce** doporučuje volat `StartActivity` metoda vždy, když aktivita uživatele změnit a **nikdy** volání `EndActivity` metoda.</span><span class="sxs-lookup"><span data-stu-id="deb59-170">Thus, it is **HIGHLY** recommended to call the `StartActivity` method whenever the activity of the user change, and to **NEVER** call the `EndActivity` method.</span></span> <span data-ttu-id="deb59-171">Tato metoda odešle zprávu do serveru zapojení, že má aktuální uživatel opustil aplikace, a to ovlivní všechny protokoly aplikací.</span><span class="sxs-lookup"><span data-stu-id="deb59-171">This method sends a message to the Engagement server that the current user has left the application and this impacts all application logs.</span></span>
> 
> 

## <a name="advanced-reporting"></a><span data-ttu-id="deb59-172">Rozšířená tvorba sestav</span><span class="sxs-lookup"><span data-stu-id="deb59-172">Advanced reporting</span></span>
<span data-ttu-id="deb59-173">Volitelně můžete chtít sestav aplikace konkrétní události, chyb a úlohy, Uděláte to tak, použijte jiné metody v nalezen `EngagementAgent` třídy.</span><span class="sxs-lookup"><span data-stu-id="deb59-173">Optionally, you may want to report application specific events, errors and jobs, to do so, use the others methods found in the `EngagementAgent` class.</span></span> <span data-ttu-id="deb59-174">Rozhraní API Engagement umožňuje používat všechny rozšířené možnosti Engagement.</span><span class="sxs-lookup"><span data-stu-id="deb59-174">The Engagement API allows to use all of Engagement's advanced capabilities.</span></span>

<span data-ttu-id="deb59-175">Další informace najdete v tématu [jak používat rozšířené Mobile Engagement označování rozhraní API v aplikaci Windows Phone Silverlight](mobile-engagement-windows-phone-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="deb59-175">For further information, see [How to use the advanced Mobile Engagement tagging API in your Windows Phone Silverlight app](mobile-engagement-windows-phone-use-engagement-api.md).</span></span>

## <a name="advanced-configuration"></a><span data-ttu-id="deb59-176">Pokročilá konfigurace</span><span class="sxs-lookup"><span data-stu-id="deb59-176">Advanced configuration</span></span>
### <a name="disable-automatic-crash-reporting"></a><span data-ttu-id="deb59-177">Zakázat automatické hlášení chyb</span><span class="sxs-lookup"><span data-stu-id="deb59-177">Disable automatic crash reporting</span></span>
<span data-ttu-id="deb59-178">Můžete vypnout automatické hlášení funkci Engagement generování sestav.</span><span class="sxs-lookup"><span data-stu-id="deb59-178">You can disable the automatic crash reporting feature of Engagement.</span></span> <span data-ttu-id="deb59-179">Pak když dojde k neošetřené výjimce, Engagement nebude provádět žádné kroky.</span><span class="sxs-lookup"><span data-stu-id="deb59-179">Then, when an unhandled exception will occur, Engagement won't do anything.</span></span>

> [!WARNING]
> <span data-ttu-id="deb59-180">Pokud budete chtít tuto funkci zakázat, mějte na paměti, když dojde k nezpracované chybě ve vaší aplikaci, nepošle Engagement havárii **a** zavřít relaci a úlohy.</span><span class="sxs-lookup"><span data-stu-id="deb59-180">If you plan to disable this feature, be aware that when a unhandled crash will occur in your app, Engagement will not send the crash **AND** it will not close the session and jobs.</span></span>
> 
> 

<span data-ttu-id="deb59-181">Chcete-li zakázat automatické hlášení chyb, upravte konfiguraci. v závislosti na způsobu, jakým jste ji deklarovaný:</span><span class="sxs-lookup"><span data-stu-id="deb59-181">To disable automatic crash reporting, just customize your configuration depending on the way you declared it :</span></span>

#### <a name="from-engagementconfigurationxml-file"></a><span data-ttu-id="deb59-182">Z `EngagementConfiguration.xml` souboru</span><span class="sxs-lookup"><span data-stu-id="deb59-182">From `EngagementConfiguration.xml` file</span></span>
<span data-ttu-id="deb59-183">Nastavte Sestavy havárií na `false` mezi `<reportCrash>` a `</reportCrash>` značky.</span><span class="sxs-lookup"><span data-stu-id="deb59-183">Set report crash to `false` between `<reportCrash>` and `</reportCrash>` tags.</span></span>

#### <a name="from-engagementconfiguration-object-at-run-time"></a><span data-ttu-id="deb59-184">Z `EngagementConfiguration` objektu v době běhu</span><span class="sxs-lookup"><span data-stu-id="deb59-184">From `EngagementConfiguration` object at run time</span></span>
<span data-ttu-id="deb59-185">Nastavit na hodnotu false pomocí objektu EngagementConfiguration sestavy havárií.</span><span class="sxs-lookup"><span data-stu-id="deb59-185">Set report crash to false using your EngagementConfiguration object.</span></span>

        /* Engagement configuration. */

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration(); engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        /\* Disable Engagement crash reporting. \*/ engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a><span data-ttu-id="deb59-186">Režim shluku</span><span class="sxs-lookup"><span data-stu-id="deb59-186">Burst mode</span></span>
<span data-ttu-id="deb59-187">Ve výchozím nastavení sestavy služby zapojení protokolů v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="deb59-187">By default, the Engagement service reports logs in real time.</span></span> <span data-ttu-id="deb59-188">Pokud vaše aplikace hlásí protokoly velmi často, je lepší ukládat do vyrovnávací paměti protokoly a sestavy je všechny najednou pravidelné základní časové (to se nazývá "shluků režim").</span><span class="sxs-lookup"><span data-stu-id="deb59-188">If your application reports logs very frequently, it is better to buffer the logs and to report them all at once on a regular time base (this is called the “burst mode”).</span></span>

<span data-ttu-id="deb59-189">Chcete-li tak učinit, zavolejte metodu:</span><span class="sxs-lookup"><span data-stu-id="deb59-189">To do so, call the method:</span></span>

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

<span data-ttu-id="deb59-190">Argument je hodnota **milisekundách**.</span><span class="sxs-lookup"><span data-stu-id="deb59-190">The argument is a value in **milliseconds**.</span></span> <span data-ttu-id="deb59-191">Kdykoli Pokud chcete znovu aktivovat protokolování v reálném čase, stačí zavoláte metoda bez zadání parametru nebo s hodnotou 0.</span><span class="sxs-lookup"><span data-stu-id="deb59-191">At any time, if you want to reactivate the real-time logging, just call the method without any parameter, or with the 0 value.</span></span>

<span data-ttu-id="deb59-192">Režim shluků mírně zvýšit výdrž baterie, ale má vliv na zapojení monitorování: všechny dobu trvání relace a úlohy zaokrouhlen shluků prahovou hodnotu (tedy relací a úlohy kratší, než je prahová hodnota shluků nemusí být zobrazeny).</span><span class="sxs-lookup"><span data-stu-id="deb59-192">The burst mode slightly increase the battery life but has an impact on the Engagement Monitor: all sessions and jobs duration will be rounded to the burst threshold (thus, sessions and jobs shorter than the burst threshold may not be visible).</span></span> <span data-ttu-id="deb59-193">Doporučujeme používat práh shluků než 30000 (30s).</span><span class="sxs-lookup"><span data-stu-id="deb59-193">It is recommended to use a burst threshold no longer than 30000 (30s).</span></span> <span data-ttu-id="deb59-194">Budete muset mějte na paměti, uložené protokoly jsou omezeny na 300 položek.</span><span class="sxs-lookup"><span data-stu-id="deb59-194">You have to be aware that saved logs are limited to 300 items.</span></span> <span data-ttu-id="deb59-195">Pokud odesílání je příliš dlouhý. může dojít ke ztrátě některých protokoly.</span><span class="sxs-lookup"><span data-stu-id="deb59-195">If sending is too long you can lose some logs.</span></span>

> [!WARNING]
> <span data-ttu-id="deb59-196">Prahová hodnota shluků nelze nakonfigurovat na dřívější dobu než jedna sekunda.</span><span class="sxs-lookup"><span data-stu-id="deb59-196">The burst threshold cannot be configured to a period lesser than one second.</span></span> <span data-ttu-id="deb59-197">Pokud se pokusíte tím, že sada SDK zobrazí trasování se chyba a bude automaticky resetovat na výchozí hodnotu, který je nula sekund.</span><span class="sxs-lookup"><span data-stu-id="deb59-197">If you try to do so, the SDK will show a trace with the error and will automatically reset to the default value, that is, zero seconds.</span></span> <span data-ttu-id="deb59-198">Tím se aktivuje sady SDK k hlášení protokolů v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="deb59-198">This will trigger the SDK to report the logs in real-time.</span></span>
> 
> 

