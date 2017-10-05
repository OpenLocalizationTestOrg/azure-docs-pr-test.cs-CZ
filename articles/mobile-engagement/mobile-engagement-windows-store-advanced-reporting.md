---
title: "Pokročilé vytváření sestav s MobileApps Engagement univerzální pro Windows"
description: "Postup pro integraci Azure Mobile Engagement univerzálních aplikací pro Windows"
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
ms.openlocfilehash: feac309db1ffce0945012e293bfc1df417aed876
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="advanced-reporting-with-the-windows-universal-apps-engagement-sdk"></a><span data-ttu-id="03539-103">Pokročilé vytváření sestav s Engagement univerzálních aplikací pro Windows SDK</span><span class="sxs-lookup"><span data-stu-id="03539-103">Advanced Reporting with the Windows Universal Apps Engagement SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="03539-104">Univerzální platforma Windows</span><span class="sxs-lookup"><span data-stu-id="03539-104">Universal Windows</span></span>](mobile-engagement-windows-store-advanced-reporting.md)
> * [<span data-ttu-id="03539-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="03539-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="03539-106">iOS</span><span class="sxs-lookup"><span data-stu-id="03539-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="03539-107">Android</span><span class="sxs-lookup"><span data-stu-id="03539-107">Android</span></span>](mobile-engagement-android-advanced-reporting.md)
> 
> 

<span data-ttu-id="03539-108">Toto téma popisuje další scénáře vytváření sestav v aplikaci univerzální pro Windows.</span><span class="sxs-lookup"><span data-stu-id="03539-108">This topic describes additional reporting scenarios in your Windows Universal application.</span></span> <span data-ttu-id="03539-109">Mezi tyto scénáře patří možnosti, které můžete použít pro vytvořené v aplikaci [Začínáme](mobile-engagement-windows-store-dotnet-get-started.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="03539-109">These scenarios include options that you can choose to apply to the app created in the [Getting Started](mobile-engagement-windows-store-dotnet-get-started.md) tutorial.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="03539-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="03539-110">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

<span data-ttu-id="03539-111">Před zahájením tohoto kurzu, je třeba nejprve provést [Začínáme](mobile-engagement-windows-store-dotnet-get-started.md) kurz, který je úmyslně přímé a jednoduché.</span><span class="sxs-lookup"><span data-stu-id="03539-111">Before starting this tutorial, you must first complete the [Getting Started](mobile-engagement-windows-store-dotnet-get-started.md) tutorial, which is deliberately direct and simple.</span></span> <span data-ttu-id="03539-112">Tento kurz se zaměřuje na další možnosti, které můžete vybrat z.</span><span class="sxs-lookup"><span data-stu-id="03539-112">This tutorial covers additional options you can choose from.</span></span>

## <a name="specifying-engagement-configuration-at-runtime"></a><span data-ttu-id="03539-113">Určení konfigurace engagement za běhu</span><span class="sxs-lookup"><span data-stu-id="03539-113">Specifying engagement configuration at runtime</span></span>
<span data-ttu-id="03539-114">Konfigurace zapojení je centralizovaná v `Resources\EngagementConfiguration.xml` souboru projektu, který je tam, kde byl zadán v [Začínáme](mobile-engagement-windows-store-dotnet-get-started.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="03539-114">The Engagement configuration is centralized in the `Resources\EngagementConfiguration.xml` file of your project, which is where it was specified in the [Getting Started](mobile-engagement-windows-store-dotnet-get-started.md) topic.</span></span>

<span data-ttu-id="03539-115">Ale můžete je také zadat v době běhu: můžete volat metodu před Engagement inicializaci agenta:</span><span class="sxs-lookup"><span data-stu-id="03539-115">But you can also specify it at runtime: you can call the following method before the Engagement agent initialization:</span></span>

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);



## <a name="recommended-method-overload-your-page-classes"></a><span data-ttu-id="03539-116">Doporučená metoda: přetížení vaší `Page` třídy</span><span class="sxs-lookup"><span data-stu-id="03539-116">Recommended method: overload your `Page` classes</span></span>
<span data-ttu-id="03539-117">Pokud chcete aktivovat reporting všechny protokoly, které vyžadují zapojení k výpočtu uživatelů, relací, aktivity, dojde k chybě a technické statistiky, zkontrolujte všechny vaše `Page` dílčí třídy dědí `EngagementPage` třídy.</span><span class="sxs-lookup"><span data-stu-id="03539-117">To activate the reporting of all the logs required by Engagement to compute Users, Sessions, Activities, Crashes and Technical statistics, make all your `Page` sub-classes inherit from the `EngagementPage` classes.</span></span>

<span data-ttu-id="03539-118">Tady je příklad pro stránku vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="03539-118">Here is an example for a page of your application.</span></span> <span data-ttu-id="03539-119">Můžete to samé pro všechny stránky vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="03539-119">You can do the same thing for all pages of your application.</span></span>

### <a name="c-source-file"></a><span data-ttu-id="03539-120">C# zdrojový soubor</span><span class="sxs-lookup"><span data-stu-id="03539-120">C# Source file</span></span>
<span data-ttu-id="03539-121">Upravit stránku `.xaml.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="03539-121">Modify your page `.xaml.cs` file:</span></span>

* <span data-ttu-id="03539-122">Přidat do vaší `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="03539-122">Add to your `using` statements:</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="03539-123">Nahraďte `Page` s `EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="03539-123">Replace `Page` with `EngagementPage`:</span></span>

<span data-ttu-id="03539-124">**Bez Engagement:**</span><span class="sxs-lookup"><span data-stu-id="03539-124">**Without Engagement:**</span></span>

        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

<span data-ttu-id="03539-125">**S Engagement:**</span><span class="sxs-lookup"><span data-stu-id="03539-125">**With Engagement:**</span></span>

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage
          {
            [...]
          }
        }

> [!IMPORTANT]
> <span data-ttu-id="03539-126">Pokud stránka přepíše metodu `OnNavigatedTo`, nezapomeňte volat `base.OnNavigatedTo(e)`.</span><span class="sxs-lookup"><span data-stu-id="03539-126">If your page overrides the `OnNavigatedTo` method, be sure to call `base.OnNavigatedTo(e)`.</span></span> <span data-ttu-id="03539-127">Jinak, není hlášena aktivity ( `EngagementPage` volání `StartActivity` uvnitř jeho `OnNavigatedTo` metoda).</span><span class="sxs-lookup"><span data-stu-id="03539-127">Otherwise, the activity is not be reported (the `EngagementPage` calls `StartActivity` inside its `OnNavigatedTo` method).</span></span>
> 
> 

### <a name="xaml-file"></a><span data-ttu-id="03539-128">Souboru XAML</span><span class="sxs-lookup"><span data-stu-id="03539-128">XAML file</span></span>
<span data-ttu-id="03539-129">Upravit stránku `.xaml` souboru:</span><span class="sxs-lookup"><span data-stu-id="03539-129">Modify your page `.xaml` file:</span></span>

* <span data-ttu-id="03539-130">Přidejte do deklarací oborů názvů:</span><span class="sxs-lookup"><span data-stu-id="03539-130">Add to your namespaces declarations:</span></span>
  
      xmlns:engagement="using:Microsoft.Azure.Engagement"
* <span data-ttu-id="03539-131">Nahraďte `Page` s `engagement:EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="03539-131">Replace `Page` with `engagement:EngagementPage`:</span></span>

<span data-ttu-id="03539-132">**Bez Engagement:**</span><span class="sxs-lookup"><span data-stu-id="03539-132">**Without Engagement:**</span></span>

        <Page>
            <!-- layout -->
            ...
        </Page>

<span data-ttu-id="03539-133">**S Engagement:**</span><span class="sxs-lookup"><span data-stu-id="03539-133">**With Engagement:**</span></span>

        <engagement:EngagementPage
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

### <a name="override-the-default-behaviour"></a><span data-ttu-id="03539-134">Přepsat výchozí chování</span><span class="sxs-lookup"><span data-stu-id="03539-134">Override the default behaviour</span></span>
<span data-ttu-id="03539-135">Ve výchozím nastavení je název třídy stránky uvedená jako název aktivity, s žádné další.</span><span class="sxs-lookup"><span data-stu-id="03539-135">By default, the class name of the page is reported as the activity name, with no extra.</span></span> <span data-ttu-id="03539-136">Pokud třída používá příponou "Stránka", Engagement jeho odstranění.</span><span class="sxs-lookup"><span data-stu-id="03539-136">If the class uses the "Page" suffix, Engagement removes it.</span></span>

<span data-ttu-id="03539-137">Pokud chcete přepsat výchozí chování pro název, přidejte tento kód:</span><span class="sxs-lookup"><span data-stu-id="03539-137">To override the default behavior for the name, add this code:</span></span>

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

<span data-ttu-id="03539-138">Sestavy doplňující informace s vaší aktivitou, přidejte tento kód:</span><span class="sxs-lookup"><span data-stu-id="03539-138">To report extra information with your activity, add this code:</span></span>

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

<span data-ttu-id="03539-139">Tyto metody jsou volány prostřednictvím `OnNavigatedTo` metoda stránky.</span><span class="sxs-lookup"><span data-stu-id="03539-139">These methods are called from within the `OnNavigatedTo` method of your page.</span></span>

### <a name="alternate-method-call-startactivity-manually"></a><span data-ttu-id="03539-140">Alternativní metoda: volání `StartActivity()` ručně</span><span class="sxs-lookup"><span data-stu-id="03539-140">Alternate method: call `StartActivity()` manually</span></span>
<span data-ttu-id="03539-141">Pokud nemůžete nebo nechcete přetížení vaše `Page` třídy, můžete místo toho spustit vaše aktivity voláním `EngagementAgent` metody přímo.</span><span class="sxs-lookup"><span data-stu-id="03539-141">If you cannot or do not want to overload your `Page` classes, you can instead start your activities by calling `EngagementAgent` methods directly.</span></span>

<span data-ttu-id="03539-142">Doporučujeme, abyste volání `StartActivity` uvnitř vaší `OnNavigatedTo` metoda stránky.</span><span class="sxs-lookup"><span data-stu-id="03539-142">We recommend calling `StartActivity` inside your `OnNavigatedTo` method of your Page.</span></span>

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [!IMPORTANT]
> <span data-ttu-id="03539-143">Zajistěte, aby že správně ukončete svou relaci.</span><span class="sxs-lookup"><span data-stu-id="03539-143">Ensure you end your session correctly.</span></span>
> 
> <span data-ttu-id="03539-144">Sady Windows Universal SDK automaticky zavolá `EndActivity` metoda při ukončení aplikace.</span><span class="sxs-lookup"><span data-stu-id="03539-144">The Windows Universal SDK automatically calls the `EndActivity` method when the application is closed.</span></span> <span data-ttu-id="03539-145">Z toho důvodu je **vysoce** doporučuje volat `StartActivity` metoda vždy, když aktivita uživatele změnit a **nikdy** volání `EndActivity` metoda.</span><span class="sxs-lookup"><span data-stu-id="03539-145">Thus, it is **HIGHLY** recommended to call the `StartActivity` method whenever the activity of the user change, and to **NEVER** call the `EndActivity` method.</span></span> <span data-ttu-id="03539-146">Tato metoda Engagement server oznámení, že má aktuální uživatel opustil aplikaci, které ovlivní všechny protokoly aplikací.</span><span class="sxs-lookup"><span data-stu-id="03539-146">This method notifies the Engagement server that the current user has left the application, which will impact all application logs.</span></span>
> 
> 

## <a name="advanced-reporting"></a><span data-ttu-id="03539-147">Rozšířená tvorba sestav</span><span class="sxs-lookup"><span data-stu-id="03539-147">Advanced reporting</span></span>
<span data-ttu-id="03539-148">Volitelně můžete chtít sestavu, události specifické pro aplikace, chyb a úlohy, Uděláte to tak, použít jiné metody v nalezen `EngagementAgent` třídy.</span><span class="sxs-lookup"><span data-stu-id="03539-148">Optionally, you may want to report application-specific events, errors and jobs, to do so, use the others methods found in the `EngagementAgent` class.</span></span> <span data-ttu-id="03539-149">Rozhraní API Engagement umožňuje použití pokročilých funkcí pro všechny zapojení.</span><span class="sxs-lookup"><span data-stu-id="03539-149">The Engagement API allows use of all Engagement's advanced capabilities.</span></span>

<span data-ttu-id="03539-150">Další informace najdete v tématu [jak používat rozšířené Mobile Engagement označování rozhraní API v aplikaci Windows Universal](mobile-engagement-windows-store-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="03539-150">For further information, see [How to use the advanced Mobile Engagement tagging API in your Windows Universal app](mobile-engagement-windows-store-use-engagement-api.md).</span></span>

