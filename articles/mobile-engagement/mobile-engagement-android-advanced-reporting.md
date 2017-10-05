---
title: "Rozšířené možnosti vytváření sestav pro Azure Mobile Engagement Android SDK"
description: "Popisuje, jak provést pokročilé vytváření sestav pro zachycení analytics pro Azure Mobile Engagement Android SDK"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 7da7abd5-19d6-4892-94d8-818e5424b2cd
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/10/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 2a1445afa2c2fca1a31ad9c012b9c8a917ebf65c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="advanced-reporting-with-engagement-on-android"></a><span data-ttu-id="f22fc-103">Pokročilé vytváření sestav s Engagement v systému Android</span><span class="sxs-lookup"><span data-stu-id="f22fc-103">Advanced Reporting with Engagement on Android</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f22fc-104">Univerzální platforma Windows</span><span class="sxs-lookup"><span data-stu-id="f22fc-104">Universal Windows</span></span>](mobile-engagement-windows-store-integrate-engagement.md)
> * [<span data-ttu-id="f22fc-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="f22fc-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="f22fc-106">iOS</span><span class="sxs-lookup"><span data-stu-id="f22fc-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="f22fc-107">Android</span><span class="sxs-lookup"><span data-stu-id="f22fc-107">Android</span></span>](mobile-engagement-android-advanced-reporting.md)
> 
> 

<span data-ttu-id="f22fc-108">Toto téma popisuje další scénáře vytváření sestav v aplikaci pro Android.</span><span class="sxs-lookup"><span data-stu-id="f22fc-108">This topic describes additional reporting scenarios in your Android application.</span></span> <span data-ttu-id="f22fc-109">Tyto možnosti můžete použít pro aplikace vytvořená v [Začínáme](mobile-engagement-android-get-started.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="f22fc-109">You can apply these options to the app created in the [Getting Started](mobile-engagement-android-get-started.md) tutorial.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f22fc-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f22fc-110">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

<span data-ttu-id="f22fc-111">Které jste dokončili kurz byl záměrně přímé a jednoduchý, ale jsou rozšířené možnosti, můžete si zvolit.</span><span class="sxs-lookup"><span data-stu-id="f22fc-111">The tutorial you completed was deliberately direct and simple, but there are advanced options you can choose.</span></span>

## <a name="modifying-your-activity-classes"></a><span data-ttu-id="f22fc-112">Úprava vaší `Activity` třídy</span><span class="sxs-lookup"><span data-stu-id="f22fc-112">Modifying your `Activity` classes</span></span>
<span data-ttu-id="f22fc-113">V [kurzu Začínáme](mobile-engagement-android-get-started.md), všechny jste museli provést aby byla vaše `*Activity` podtřídy dědí odpovídající `Engagement*Activity` třídy.</span><span class="sxs-lookup"><span data-stu-id="f22fc-113">In the [Getting Started tutorial](mobile-engagement-android-get-started.md), all you had to do was to make your `*Activity` subclasses inherit from the corresponding `Engagement*Activity` classes.</span></span> <span data-ttu-id="f22fc-114">Například, pokud vaše aktivita starší verze rozšířené `ListActivity`, by ho rozšířit provedete `EngagementListActivity`.</span><span class="sxs-lookup"><span data-stu-id="f22fc-114">For example, if your legacy activity extended `ListActivity`, you would make it extend `EngagementListActivity`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f22fc-115">Při použití `EngagementListActivity` nebo `EngagementExpandableListActivity`, ujistěte se, že všechny volat na `requestWindowFeature(...);` je provedena před volání `super.onCreate(...);`, jinak dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="f22fc-115">When using `EngagementListActivity` or `EngagementExpandableListActivity`, make sure any call to `requestWindowFeature(...);` is made before the call to `super.onCreate(...);`, otherwise a crash occurs.</span></span>
> 
> 

<span data-ttu-id="f22fc-116">Můžete najít v těchto tříd `src` složku a můžete je zkopírovat do projektu.</span><span class="sxs-lookup"><span data-stu-id="f22fc-116">You can find these classes in the `src` folder, and can copy them into your project.</span></span> <span data-ttu-id="f22fc-117">Třídy jsou i ve **JavaDoc**.</span><span class="sxs-lookup"><span data-stu-id="f22fc-117">The classes are also in the **JavaDoc**.</span></span>

## <a name="alternate-method-call-startactivity-and-endactivity-manually"></a><span data-ttu-id="f22fc-118">Alternativní metoda: volání `startActivity()` a `endActivity()` ručně</span><span class="sxs-lookup"><span data-stu-id="f22fc-118">Alternate method: call `startActivity()` and `endActivity()` manually</span></span>
<span data-ttu-id="f22fc-119">Pokud nemůžete nebo nechcete přetížení vaše `Activity` třídy, můžete místo toho začínat a končit vaše aktivity voláním `EngagementAgent`na metody přímo.</span><span class="sxs-lookup"><span data-stu-id="f22fc-119">If you cannot or do not want to overload your `Activity` classes, you can instead start and end your activities by calling the `EngagementAgent`'s methods directly.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f22fc-120">Nikdy nevolá SDK pro Android `endActivity()` metoda, i když ukončení aplikace (v systému Android, se nikdy ukončit aplikace).</span><span class="sxs-lookup"><span data-stu-id="f22fc-120">The Android SDK never calls the `endActivity()` method, even when the application is closed (on Android, applications are never closed).</span></span> <span data-ttu-id="f22fc-121">Z toho důvodu je *vysoce* doporučuje volat `startActivity()` metoda v `onResume` zpětného volání z *všechny* vaše aktivity a `endActivity()` metoda v `onPause()` zpětného volání z *všechny* vaše aktivity.</span><span class="sxs-lookup"><span data-stu-id="f22fc-121">Thus, it is *HIGHLY* recommended to call the `startActivity()` method in the `onResume` callback of *ALL* your activities, and the `endActivity()` method in the `onPause()` callback of *ALL* your activities.</span></span> <span data-ttu-id="f22fc-122">Toto je jediný způsob, jak Ujistěte se, že nejsou neuniknou relací.</span><span class="sxs-lookup"><span data-stu-id="f22fc-122">This is the only way to be sure that sessions are not leaked.</span></span> <span data-ttu-id="f22fc-123">Pokud relace došlo k úniku, Engagement služba nikdy odpojí z back-end Engagement (protože službu zůstane připojen tak dlouho, dokud relace čeká na vyřízení).</span><span class="sxs-lookup"><span data-stu-id="f22fc-123">If a session is leaked, the Engagement service never disconnects from the Engagement backend (since the service stays connected as long as a session is pending).</span></span>
> 
> 

<span data-ttu-id="f22fc-124">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="f22fc-124">Here is an example:</span></span>

    public class MyActivity extends Some3rdPartyActivity
    {
      @Override
      protected void onResume()
      {
        super.onResume();
        String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
        EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
      }

      @Override
      protected void onPause()
      {
        super.onPause();
        EngagementAgent.getInstance(this).endActivity();
      }
    }

<span data-ttu-id="f22fc-125">V tomto příkladu je podobná `EngagementActivity` třídy a jeho variant, jejichž zdrojový kód je k dispozici v `src` složky.</span><span class="sxs-lookup"><span data-stu-id="f22fc-125">This example is similar to the `EngagementActivity` class and its variants, whose source code is provided in the `src` folder.</span></span>

## <a name="using-applicationoncreate"></a><span data-ttu-id="f22fc-126">Pomocí Application.onCreate()</span><span class="sxs-lookup"><span data-stu-id="f22fc-126">Using Application.onCreate()</span></span>
<span data-ttu-id="f22fc-127">Kód, umístěte do `Application.onCreate()` a v jiné aplikaci zpětná volání spuštění pro procesy na všechny aplikace, včetně služby zapojení.</span><span class="sxs-lookup"><span data-stu-id="f22fc-127">Any code you place in `Application.onCreate()` and in other application callbacks is run for all your application's processes, including the Engagement service.</span></span> <span data-ttu-id="f22fc-128">Může mít nežádoucí vedlejší účinky, jako je přidělování nepotřebné paměti a vláken v procesu zapojení, nebo duplicitní všesměrového vysílání příjemci nebo službám.</span><span class="sxs-lookup"><span data-stu-id="f22fc-128">It may have unwanted side effects, like unneeded memory allocations and threads in the Engagement's process, or duplicate broadcast receivers or services.</span></span>

<span data-ttu-id="f22fc-129">Pokud přepíšete `Application.onCreate()`, doporučujeme, abyste přidáním následující fragment kódu na začátku vaší `Application.onCreate()` funkce:</span><span class="sxs-lookup"><span data-stu-id="f22fc-129">If you override `Application.onCreate()`, we recommend adding the following code snippet at the beginning of your `Application.onCreate()` function:</span></span>

     public void onCreate()
     {
       if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
         return;

       ... Your code...
     }

<span data-ttu-id="f22fc-130">Může stejnou věc udělat `Application.onTerminate()`, `Application.onLowMemory()`, a `Application.onConfigurationChanged(...)`.</span><span class="sxs-lookup"><span data-stu-id="f22fc-130">You can do the same thing for `Application.onTerminate()`, `Application.onLowMemory()`, and `Application.onConfigurationChanged(...)`.</span></span>

<span data-ttu-id="f22fc-131">Můžete také rozšířit `EngagementApplication` místo rozšíření `Application`: zpětné volání `Application.onCreate()` nemá procesu kontroly a volání `Application.onApplicationProcessCreate()` jen pokud aktuální proces není ta, který je hostitelem služby zapojení, stejná pravidla platit pro ostatní zpětných volání.</span><span class="sxs-lookup"><span data-stu-id="f22fc-131">You can also extend `EngagementApplication` instead of extending `Application`: the callback `Application.onCreate()` does the process check and calls `Application.onApplicationProcessCreate()` only if the current process is not the one hosting the Engagement service, the same rules apply for the other callbacks.</span></span>

## <a name="tags-in-the-androidmanifestxml-file"></a><span data-ttu-id="f22fc-132">Značky v souboru AndroidManifest.xml</span><span class="sxs-lookup"><span data-stu-id="f22fc-132">Tags in the AndroidManifest.xml file</span></span>
<span data-ttu-id="f22fc-133">Ve značce služby v souboru AndroidManifest.xml `android:label` atribut umožňuje zvolit název služby Engagement, jak se objevuje pro koncové uživatele na obrazovce "Spuštění služby" svůj telefon.</span><span class="sxs-lookup"><span data-stu-id="f22fc-133">In the service tag in the AndroidManifest.xml file, the `android:label` attribute allows you to choose the name of the Engagement service as it appears to end users in the "Running services" screen of their phone.</span></span> <span data-ttu-id="f22fc-134">Je vhodné nastavení tohoto atributu na `"<Your application name>Service"` (například `"AcmeFunGameService"`).</span><span class="sxs-lookup"><span data-stu-id="f22fc-134">We recommended setting this attribute to `"<Your application name>Service"` (for example, `"AcmeFunGameService"`).</span></span>

<span data-ttu-id="f22fc-135">Určení `android:process` atribut zajišťuje, že služba Engagement běží ve svém vlastním procesu (systémem Engagement v rámci jednoho procesu jako vaše aplikace provede potenciálně pomaleji vlákna vaší hlavní nebo uživatelského rozhraní).</span><span class="sxs-lookup"><span data-stu-id="f22fc-135">Specifying the `android:process` attribute ensures that the Engagement service runs in its own process (running Engagement in the same process as your application makes your main/UI thread potentially less responsive).</span></span>

## <a name="building-with-proguard"></a><span data-ttu-id="f22fc-136">Sestavování s použitím ProGuard</span><span class="sxs-lookup"><span data-stu-id="f22fc-136">Building with ProGuard</span></span>
<span data-ttu-id="f22fc-137">Pokud vytvoříte balíčku aplikace s ProGuard, budete muset zachovat některé třídy.</span><span class="sxs-lookup"><span data-stu-id="f22fc-137">If you build your application package with ProGuard, you need to keep some classes.</span></span> <span data-ttu-id="f22fc-138">Můžete použít následující fragment kódu konfigurace:</span><span class="sxs-lookup"><span data-stu-id="f22fc-138">You can use the following configuration snippet:</span></span>

    -keep public class * extends android.os.IInterface
    -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
    <methods>;
     }
