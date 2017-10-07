---
title: "aaaWindows Universal pokročilé vytváření sestav s MobileApps zapojení"
description: "Jak tooIntegrate Azure Mobile Engagement s univerzálních aplikací pro Windows"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: ea5030bf-73ac-49b7-bc3e-c25fc10e945a
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 20968f238ef7ae9dc0b8bb6dac3fb8bdb9bc3a10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-reporting-with-hello-windows-universal-apps-engagement-sdk"></a><span data-ttu-id="36224-103">Pokročilé vytváření sestav s hello Engagement SDK univerzální aplikace Windows</span><span class="sxs-lookup"><span data-stu-id="36224-103">Advanced Reporting with hello Windows Universal Apps Engagement SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="36224-104">Univerzální platforma Windows</span><span class="sxs-lookup"><span data-stu-id="36224-104">Universal Windows</span></span>](mobile-engagement-windows-store-advanced-reporting.md)
> * [<span data-ttu-id="36224-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="36224-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="36224-106">iOS</span><span class="sxs-lookup"><span data-stu-id="36224-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="36224-107">Android</span><span class="sxs-lookup"><span data-stu-id="36224-107">Android</span></span>](mobile-engagement-android-advanced-reporting.md)
> 
> 

<span data-ttu-id="36224-108">Toto téma popisuje další scénáře vytváření sestav v aplikaci univerzální pro Windows.</span><span class="sxs-lookup"><span data-stu-id="36224-108">This topic describes additional reporting scenarios in your Windows Universal application.</span></span> <span data-ttu-id="36224-109">Mezi tyto scénáře patří možnosti, které můžete tooapply toohello aplikace vytvořená v hello [Začínáme](mobile-engagement-windows-store-dotnet-get-started.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="36224-109">These scenarios include options that you can choose tooapply toohello app created in hello [Getting Started](mobile-engagement-windows-store-dotnet-get-started.md) tutorial.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="36224-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="36224-110">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

<span data-ttu-id="36224-111">Před zahájením tohoto kurzu, musíte nejdřív dokončit hello [Začínáme](mobile-engagement-windows-store-dotnet-get-started.md) kurz, který je úmyslně přímé a jednoduché.</span><span class="sxs-lookup"><span data-stu-id="36224-111">Before starting this tutorial, you must first complete hello [Getting Started](mobile-engagement-windows-store-dotnet-get-started.md) tutorial, which is deliberately direct and simple.</span></span> <span data-ttu-id="36224-112">Tento kurz se zaměřuje na další možnosti, které můžete vybrat z.</span><span class="sxs-lookup"><span data-stu-id="36224-112">This tutorial covers additional options you can choose from.</span></span>

## <a name="specifying-engagement-configuration-at-runtime"></a><span data-ttu-id="36224-113">Určení konfigurace engagement za běhu</span><span class="sxs-lookup"><span data-stu-id="36224-113">Specifying engagement configuration at runtime</span></span>
<span data-ttu-id="36224-114">Konfigurace Engagement Hello je centralizovaná v hello `Resources\EngagementConfiguration.xml` souboru projektu, který je tam, kde byl zadán v hello [Začínáme](mobile-engagement-windows-store-dotnet-get-started.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="36224-114">hello Engagement configuration is centralized in hello `Resources\EngagementConfiguration.xml` file of your project, which is where it was specified in hello [Getting Started](mobile-engagement-windows-store-dotnet-get-started.md) topic.</span></span>

<span data-ttu-id="36224-115">Ale můžete je také zadat v době běhu: můžete volat hello následující metody před inicializaci agenta hello Engagement:</span><span class="sxs-lookup"><span data-stu-id="36224-115">But you can also specify it at runtime: you can call hello following method before hello Engagement agent initialization:</span></span>

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set hello Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);



## <a name="recommended-method-overload-your-page-classes"></a><span data-ttu-id="36224-116">Doporučená metoda: přetížení vaší `Page` třídy</span><span class="sxs-lookup"><span data-stu-id="36224-116">Recommended method: overload your `Page` classes</span></span>
<span data-ttu-id="36224-117">vytváření sestav hello tooactivate všechny protokoly hello vyžadují zapojení toocompute uživatelů, relací, aktivity, dojde k chybě a technické statistiky, ujistěte se, všechna vaše `Page` dílčí třídy dědí hello `EngagementPage` třídy.</span><span class="sxs-lookup"><span data-stu-id="36224-117">tooactivate hello reporting of all hello logs required by Engagement toocompute Users, Sessions, Activities, Crashes and Technical statistics, make all your `Page` sub-classes inherit from hello `EngagementPage` classes.</span></span>

<span data-ttu-id="36224-118">Tady je příklad pro stránku vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="36224-118">Here is an example for a page of your application.</span></span> <span data-ttu-id="36224-119">Můžete provést hello samé pro všechny stránky vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="36224-119">You can do hello same thing for all pages of your application.</span></span>

### <a name="c-source-file"></a><span data-ttu-id="36224-120">C# zdrojový soubor</span><span class="sxs-lookup"><span data-stu-id="36224-120">C# Source file</span></span>
<span data-ttu-id="36224-121">Upravit stránku `.xaml.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="36224-121">Modify your page `.xaml.cs` file:</span></span>

* <span data-ttu-id="36224-122">Přidat tooyour `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="36224-122">Add tooyour `using` statements:</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="36224-123">Nahraďte `Page` s `EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="36224-123">Replace `Page` with `EngagementPage`:</span></span>

<span data-ttu-id="36224-124">**Bez Engagement:**</span><span class="sxs-lookup"><span data-stu-id="36224-124">**Without Engagement:**</span></span>

        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

<span data-ttu-id="36224-125">**S Engagement:**</span><span class="sxs-lookup"><span data-stu-id="36224-125">**With Engagement:**</span></span>

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage
          {
            [...]
          }
        }

> [!IMPORTANT]
> <span data-ttu-id="36224-126">Pokud stránka přepíše hello `OnNavigatedTo` metoda, že toocall být `base.OnNavigatedTo(e)`.</span><span class="sxs-lookup"><span data-stu-id="36224-126">If your page overrides hello `OnNavigatedTo` method, be sure toocall `base.OnNavigatedTo(e)`.</span></span> <span data-ttu-id="36224-127">Jinak, není hlášena hello aktivity (hello `EngagementPage` volání `StartActivity` uvnitř jeho `OnNavigatedTo` metoda).</span><span class="sxs-lookup"><span data-stu-id="36224-127">Otherwise, hello activity is not be reported (hello `EngagementPage` calls `StartActivity` inside its `OnNavigatedTo` method).</span></span>
> 
> 

### <a name="xaml-file"></a><span data-ttu-id="36224-128">Souboru XAML</span><span class="sxs-lookup"><span data-stu-id="36224-128">XAML file</span></span>
<span data-ttu-id="36224-129">Upravit stránku `.xaml` souboru:</span><span class="sxs-lookup"><span data-stu-id="36224-129">Modify your page `.xaml` file:</span></span>

* <span data-ttu-id="36224-130">Přidejte tooyour deklarací oborů názvů:</span><span class="sxs-lookup"><span data-stu-id="36224-130">Add tooyour namespaces declarations:</span></span>
  
      xmlns:engagement="using:Microsoft.Azure.Engagement"
* <span data-ttu-id="36224-131">Nahraďte `Page` s `engagement:EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="36224-131">Replace `Page` with `engagement:EngagementPage`:</span></span>

<span data-ttu-id="36224-132">**Bez Engagement:**</span><span class="sxs-lookup"><span data-stu-id="36224-132">**Without Engagement:**</span></span>

        <Page>
            <!-- layout -->
            ...
        </Page>

<span data-ttu-id="36224-133">**S Engagement:**</span><span class="sxs-lookup"><span data-stu-id="36224-133">**With Engagement:**</span></span>

        <engagement:EngagementPage
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

### <a name="override-hello-default-behaviour"></a><span data-ttu-id="36224-134">Přepsat výchozí chování hello</span><span class="sxs-lookup"><span data-stu-id="36224-134">Override hello default behaviour</span></span>
<span data-ttu-id="36224-135">Ve výchozím nastavení je název třídy hello stránku hello uvedená jako název aktivity hello a žádné navíc.</span><span class="sxs-lookup"><span data-stu-id="36224-135">By default, hello class name of hello page is reported as hello activity name, with no extra.</span></span> <span data-ttu-id="36224-136">Pokud třída hello používá příponu "Stránka" hello, Engagement jeho odstranění.</span><span class="sxs-lookup"><span data-stu-id="36224-136">If hello class uses hello "Page" suffix, Engagement removes it.</span></span>

<span data-ttu-id="36224-137">toooverride hello výchozí chování pro název hello přidejte tento kód:</span><span class="sxs-lookup"><span data-stu-id="36224-137">toooverride hello default behavior for hello name, add this code:</span></span>

        // in hello .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

<span data-ttu-id="36224-138">tooreport doplňující informace s vaší aktivitou, přidejte tento kód:</span><span class="sxs-lookup"><span data-stu-id="36224-138">tooreport extra information with your activity, add this code:</span></span>

        // in hello .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

<span data-ttu-id="36224-139">Tyto metody jsou volat v rámci hello `OnNavigatedTo` metoda stránky.</span><span class="sxs-lookup"><span data-stu-id="36224-139">These methods are called from within hello `OnNavigatedTo` method of your page.</span></span>

### <a name="alternate-method-call-startactivity-manually"></a><span data-ttu-id="36224-140">Alternativní metoda: volání `StartActivity()` ručně</span><span class="sxs-lookup"><span data-stu-id="36224-140">Alternate method: call `StartActivity()` manually</span></span>
<span data-ttu-id="36224-141">Pokud nemůžete nebo nechcete, aby toooverload vaše `Page` třídy, můžete místo toho spustit vaše aktivity voláním `EngagementAgent` metody přímo.</span><span class="sxs-lookup"><span data-stu-id="36224-141">If you cannot or do not want toooverload your `Page` classes, you can instead start your activities by calling `EngagementAgent` methods directly.</span></span>

<span data-ttu-id="36224-142">Doporučujeme, abyste volání `StartActivity` uvnitř vaší `OnNavigatedTo` metoda stránky.</span><span class="sxs-lookup"><span data-stu-id="36224-142">We recommend calling `StartActivity` inside your `OnNavigatedTo` method of your Page.</span></span>

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [!IMPORTANT]
> <span data-ttu-id="36224-143">Zajistěte, aby že správně ukončete svou relaci.</span><span class="sxs-lookup"><span data-stu-id="36224-143">Ensure you end your session correctly.</span></span>
> 
> <span data-ttu-id="36224-144">Hello sady Windows Universal SDK automaticky volá hello `EndActivity` metoda při zavření aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="36224-144">hello Windows Universal SDK automatically calls hello `EndActivity` method when hello application is closed.</span></span> <span data-ttu-id="36224-145">Z toho důvodu je **vysoce** doporučená toocall hello `StartActivity` metoda vždy, když aktivita hello hello uživatele změnit a příliš**nikdy** volání hello `EndActivity` metoda.</span><span class="sxs-lookup"><span data-stu-id="36224-145">Thus, it is **HIGHLY** recommended toocall hello `StartActivity` method whenever hello activity of hello user change, and too**NEVER** call hello `EndActivity` method.</span></span> <span data-ttu-id="36224-146">Tato metoda hello Engagement server oznámení, že tento aktuální uživatel hello opustil hello aplikaci, které ovlivní všechny protokoly aplikací.</span><span class="sxs-lookup"><span data-stu-id="36224-146">This method notifies hello Engagement server that hello current user has left hello application, which will impact all application logs.</span></span>
> 
> 

## <a name="advanced-reporting"></a><span data-ttu-id="36224-147">Rozšířená tvorba sestav</span><span class="sxs-lookup"><span data-stu-id="36224-147">Advanced reporting</span></span>
<span data-ttu-id="36224-148">Volitelně můžete tooreport události specifické pro aplikaci, chyb a úlohy, toodo tak, použijte hello další metody nalezeny v hello `EngagementAgent` třídy.</span><span class="sxs-lookup"><span data-stu-id="36224-148">Optionally, you may want tooreport application-specific events, errors and jobs, toodo so, use hello others methods found in hello `EngagementAgent` class.</span></span> <span data-ttu-id="36224-149">Hello Engagement API umožňuje použití pokročilých funkcí pro všechny zapojení.</span><span class="sxs-lookup"><span data-stu-id="36224-149">hello Engagement API allows use of all Engagement's advanced capabilities.</span></span>

<span data-ttu-id="36224-150">Další informace najdete v tématu [jak toouse hello advanced označování rozhraní API v aplikaci Windows Universal Mobile Engagement](mobile-engagement-windows-store-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="36224-150">For further information, see [How toouse hello advanced Mobile Engagement tagging API in your Windows Universal app](mobile-engagement-windows-store-use-engagement-api.md).</span></span>

