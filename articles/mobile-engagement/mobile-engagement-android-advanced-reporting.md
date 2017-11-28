---
title: "aaaAdvanced možnosti zasílání zpráv o Azure Mobile Engagement Android SDK"
description: "Popisuje, jak toodo pokročilou analýzu reporting toocapture pro Azure Mobile Engagement Android SDK"
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
ms.openlocfilehash: 5c8f4ea36c54715f4e09fd43c96132c15019a71b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-reporting-with-engagement-on-android"></a><span data-ttu-id="85511-103">Pokročilé vytváření sestav s Engagement v systému Android</span><span class="sxs-lookup"><span data-stu-id="85511-103">Advanced Reporting with Engagement on Android</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="85511-104">Univerzální platforma Windows</span><span class="sxs-lookup"><span data-stu-id="85511-104">Universal Windows</span></span>](mobile-engagement-windows-store-integrate-engagement.md)
> * [<span data-ttu-id="85511-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="85511-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="85511-106">iOS</span><span class="sxs-lookup"><span data-stu-id="85511-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="85511-107">Android</span><span class="sxs-lookup"><span data-stu-id="85511-107">Android</span></span>](mobile-engagement-android-advanced-reporting.md)
> 
> 

<span data-ttu-id="85511-108">Toto téma popisuje další scénáře vytváření sestav v aplikaci pro Android.</span><span class="sxs-lookup"><span data-stu-id="85511-108">This topic describes additional reporting scenarios in your Android application.</span></span> <span data-ttu-id="85511-109">Můžete použít tyto možnosti toohello aplikace vytvořená v hello [Začínáme](mobile-engagement-android-get-started.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="85511-109">You can apply these options toohello app created in hello [Getting Started](mobile-engagement-android-get-started.md) tutorial.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="85511-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="85511-110">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

<span data-ttu-id="85511-111">Hello kurzu, které jste dokončili byla úmyslně direct a jednoduchý, ale jsou rozšířené možnosti, můžete si zvolit.</span><span class="sxs-lookup"><span data-stu-id="85511-111">hello tutorial you completed was deliberately direct and simple, but there are advanced options you can choose.</span></span>

## <a name="modifying-your-activity-classes"></a><span data-ttu-id="85511-112">Úprava vaší `Activity` třídy</span><span class="sxs-lookup"><span data-stu-id="85511-112">Modifying your `Activity` classes</span></span>
<span data-ttu-id="85511-113">V hello [kurzu Začínáme](mobile-engagement-android-get-started.md), všechny jste měli toodo byla toomake vaše `*Activity` podtřídy dědí hello odpovídající `Engagement*Activity` třídy.</span><span class="sxs-lookup"><span data-stu-id="85511-113">In hello [Getting Started tutorial](mobile-engagement-android-get-started.md), all you had toodo was toomake your `*Activity` subclasses inherit from hello corresponding `Engagement*Activity` classes.</span></span> <span data-ttu-id="85511-114">Například, pokud vaše aktivita starší verze rozšířené `ListActivity`, by ho rozšířit provedete `EngagementListActivity`.</span><span class="sxs-lookup"><span data-stu-id="85511-114">For example, if your legacy activity extended `ListActivity`, you would make it extend `EngagementListActivity`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="85511-115">Při použití `EngagementListActivity` nebo `EngagementExpandableListActivity`, ujistěte se, že žádném volání, příliš`requestWindowFeature(...);` je provedena před hello volání příliš`super.onCreate(...);`, jinak dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="85511-115">When using `EngagementListActivity` or `EngagementExpandableListActivity`, make sure any call too`requestWindowFeature(...);` is made before hello call too`super.onCreate(...);`, otherwise a crash occurs.</span></span>
> 
> 

<span data-ttu-id="85511-116">Tyto třídy můžete najít v hello `src` složku a můžete je zkopírovat do projektu.</span><span class="sxs-lookup"><span data-stu-id="85511-116">You can find these classes in hello `src` folder, and can copy them into your project.</span></span> <span data-ttu-id="85511-117">Hello třídy jsou také v hello **JavaDoc**.</span><span class="sxs-lookup"><span data-stu-id="85511-117">hello classes are also in hello **JavaDoc**.</span></span>

## <a name="alternate-method-call-startactivity-and-endactivity-manually"></a><span data-ttu-id="85511-118">Alternativní metoda: volání `startActivity()` a `endActivity()` ručně</span><span class="sxs-lookup"><span data-stu-id="85511-118">Alternate method: call `startActivity()` and `endActivity()` manually</span></span>
<span data-ttu-id="85511-119">Pokud nemůžete nebo nechcete, aby toooverload vaše `Activity` třídy, můžete místo toho zahájení a ukončení vaše aktivity voláním hello `EngagementAgent`na metody přímo.</span><span class="sxs-lookup"><span data-stu-id="85511-119">If you cannot or do not want toooverload your `Activity` classes, you can instead start and end your activities by calling hello `EngagementAgent`'s methods directly.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="85511-120">Hello Android SDK nikdy nevolá hello `endActivity()` metoda, i když ukončení aplikace hello (v systému Android, se nikdy ukončit aplikace).</span><span class="sxs-lookup"><span data-stu-id="85511-120">hello Android SDK never calls hello `endActivity()` method, even when hello application is closed (on Android, applications are never closed).</span></span> <span data-ttu-id="85511-121">Z toho důvodu je *vysoce* doporučená toocall hello `startActivity()` metoda v hello `onResume` zpětného volání z *všechny* vaše aktivity a hello `endActivity()` metoda v hello `onPause()` zpětné volání z *všechny* vašich aktivit.</span><span class="sxs-lookup"><span data-stu-id="85511-121">Thus, it is *HIGHLY* recommended toocall hello `startActivity()` method in hello `onResume` callback of *ALL* your activities, and hello `endActivity()` method in hello `onPause()` callback of *ALL* your activities.</span></span> <span data-ttu-id="85511-122">Toto je hello pouze způsob toobe jistotu, že nejsou neuniknou relací.</span><span class="sxs-lookup"><span data-stu-id="85511-122">This is hello only way toobe sure that sessions are not leaked.</span></span> <span data-ttu-id="85511-123">Pokud relace došlo k úniku, odpojí od hello Engagement back-end nikdy hello zapojení služby (od služby hello zůstane připojen tak dlouho, dokud relace čeká na vyřízení).</span><span class="sxs-lookup"><span data-stu-id="85511-123">If a session is leaked, hello Engagement service never disconnects from hello Engagement backend (since hello service stays connected as long as a session is pending).</span></span>
> 
> 

<span data-ttu-id="85511-124">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="85511-124">Here is an example:</span></span>

    public class MyActivity extends Some3rdPartyActivity
    {
      @Override
      protected void onResume()
      {
        super.onResume();
        String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at hello end.
        EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
      }

      @Override
      protected void onPause()
      {
        super.onPause();
        EngagementAgent.getInstance(this).endActivity();
      }
    }

<span data-ttu-id="85511-125">Tato ukázka je podobné toohello `EngagementActivity` třídy a jeho variant, jejichž zdrojový kód je k dispozici v hello `src` složky.</span><span class="sxs-lookup"><span data-stu-id="85511-125">This example is similar toohello `EngagementActivity` class and its variants, whose source code is provided in hello `src` folder.</span></span>

## <a name="using-applicationoncreate"></a><span data-ttu-id="85511-126">Pomocí Application.onCreate()</span><span class="sxs-lookup"><span data-stu-id="85511-126">Using Application.onCreate()</span></span>
<span data-ttu-id="85511-127">Kód, umístěte do `Application.onCreate()` a v jiné aplikaci zpětná volání spouští se pro všechny aplikace procesů, včetně služby zapojení hello.</span><span class="sxs-lookup"><span data-stu-id="85511-127">Any code you place in `Application.onCreate()` and in other application callbacks is run for all your application's processes, including hello Engagement service.</span></span> <span data-ttu-id="85511-128">Může mít nežádoucí vedlejší účinky, jako je přidělování nepotřebné paměti a vláken v procesu hello Engagement, nebo duplicitní vysílání příjemci nebo služby.</span><span class="sxs-lookup"><span data-stu-id="85511-128">It may have unwanted side effects, like unneeded memory allocations and threads in hello Engagement's process, or duplicate broadcast receivers or services.</span></span>

<span data-ttu-id="85511-129">Pokud přepíšete `Application.onCreate()`, doporučujeme, abyste přidáním hello následující fragment kódu na začátku hello vaší `Application.onCreate()` funkce:</span><span class="sxs-lookup"><span data-stu-id="85511-129">If you override `Application.onCreate()`, we recommend adding hello following code snippet at hello beginning of your `Application.onCreate()` function:</span></span>

     public void onCreate()
     {
       if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
         return;

       ... Your code...
     }

<span data-ttu-id="85511-130">Můžete provést hello samé pro `Application.onTerminate()`, `Application.onLowMemory()`, a `Application.onConfigurationChanged(...)`.</span><span class="sxs-lookup"><span data-stu-id="85511-130">You can do hello same thing for `Application.onTerminate()`, `Application.onLowMemory()`, and `Application.onConfigurationChanged(...)`.</span></span>

<span data-ttu-id="85511-131">Můžete také rozšířit `EngagementApplication` místo rozšíření `Application`: hello zpětného volání `Application.onCreate()` hello procesu kontroly a volání `Application.onApplicationProcessCreate()` jen pokud aktuální proces hello není hello jedné hostování hello zapojení služby, hello stejná pravidla platí pro Hello jiných zpětných volání.</span><span class="sxs-lookup"><span data-stu-id="85511-131">You can also extend `EngagementApplication` instead of extending `Application`: hello callback `Application.onCreate()` does hello process check and calls `Application.onApplicationProcessCreate()` only if hello current process is not hello one hosting hello Engagement service, hello same rules apply for hello other callbacks.</span></span>

## <a name="tags-in-hello-androidmanifestxml-file"></a><span data-ttu-id="85511-132">Značky v souboru AndroidManifest.xml hello</span><span class="sxs-lookup"><span data-stu-id="85511-132">Tags in hello AndroidManifest.xml file</span></span>
<span data-ttu-id="85511-133">Ve značce hello služby v souboru AndroidManifest.xml hello, hello `android:label` atribut vám umožní toochoose hello název hello zapojení služby jak se zobrazuje uživatelům tooend v obrazovce "Spuštění služby" hello svůj telefon.</span><span class="sxs-lookup"><span data-stu-id="85511-133">In hello service tag in hello AndroidManifest.xml file, hello `android:label` attribute allows you toochoose hello name of hello Engagement service as it appears tooend users in hello "Running services" screen of their phone.</span></span> <span data-ttu-id="85511-134">Je vhodné nastavení tohoto atributu příliš`"<Your application name>Service"` (například `"AcmeFunGameService"`).</span><span class="sxs-lookup"><span data-stu-id="85511-134">We recommended setting this attribute too`"<Your application name>Service"` (for example, `"AcmeFunGameService"`).</span></span>

<span data-ttu-id="85511-135">Zadání hello `android:process` atribut zajistí, že hello Engagement service běží ve svém vlastním procesu (spouštění Engagement v hello stejný proces vaše aplikace provede potenciálně pomaleji vlákna vaší hlavní nebo uživatelského rozhraní).</span><span class="sxs-lookup"><span data-stu-id="85511-135">Specifying hello `android:process` attribute ensures that hello Engagement service runs in its own process (running Engagement in hello same process as your application makes your main/UI thread potentially less responsive).</span></span>

## <a name="building-with-proguard"></a><span data-ttu-id="85511-136">Sestavování s použitím ProGuard</span><span class="sxs-lookup"><span data-stu-id="85511-136">Building with ProGuard</span></span>
<span data-ttu-id="85511-137">Pokud vytvoříte balíčku aplikace s ProGuard, musíte tookeep některé třídy.</span><span class="sxs-lookup"><span data-stu-id="85511-137">If you build your application package with ProGuard, you need tookeep some classes.</span></span> <span data-ttu-id="85511-138">Můžete použít následující fragment kódu konfigurace hello:</span><span class="sxs-lookup"><span data-stu-id="85511-138">You can use hello following configuration snippet:</span></span>

    -keep public class * extends android.os.IInterface
    -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
    <methods>;
     }
