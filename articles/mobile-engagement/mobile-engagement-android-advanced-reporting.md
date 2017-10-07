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
# <a name="advanced-reporting-with-engagement-on-android"></a>Pokročilé vytváření sestav s Engagement v systému Android
> [!div class="op_single_selector"]
> * [Univerzální platforma Windows](mobile-engagement-windows-store-integrate-engagement.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-advanced-reporting.md)
> 
> 

Toto téma popisuje další scénáře vytváření sestav v aplikaci pro Android. Můžete použít tyto možnosti toohello aplikace vytvořená v hello [Začínáme](mobile-engagement-android-get-started.md) kurzu.

## <a name="prerequisites"></a>Požadavky
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

Hello kurzu, které jste dokončili byla úmyslně direct a jednoduchý, ale jsou rozšířené možnosti, můžete si zvolit.

## <a name="modifying-your-activity-classes"></a>Úprava vaší `Activity` třídy
V hello [kurzu Začínáme](mobile-engagement-android-get-started.md), všechny jste měli toodo byla toomake vaše `*Activity` podtřídy dědí hello odpovídající `Engagement*Activity` třídy. Například, pokud vaše aktivita starší verze rozšířené `ListActivity`, by ho rozšířit provedete `EngagementListActivity`.

> [!IMPORTANT]
> Při použití `EngagementListActivity` nebo `EngagementExpandableListActivity`, ujistěte se, že žádném volání, příliš`requestWindowFeature(...);` je provedena před hello volání příliš`super.onCreate(...);`, jinak dojde k chybě.
> 
> 

Tyto třídy můžete najít v hello `src` složku a můžete je zkopírovat do projektu. Hello třídy jsou také v hello **JavaDoc**.

## <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternativní metoda: volání `startActivity()` a `endActivity()` ručně
Pokud nemůžete nebo nechcete, aby toooverload vaše `Activity` třídy, můžete místo toho zahájení a ukončení vaše aktivity voláním hello `EngagementAgent`na metody přímo.

> [!IMPORTANT]
> Hello Android SDK nikdy nevolá hello `endActivity()` metoda, i když ukončení aplikace hello (v systému Android, se nikdy ukončit aplikace). Z toho důvodu je *vysoce* doporučená toocall hello `startActivity()` metoda v hello `onResume` zpětného volání z *všechny* vaše aktivity a hello `endActivity()` metoda v hello `onPause()` zpětné volání z *všechny* vašich aktivit. Toto je hello pouze způsob toobe jistotu, že nejsou neuniknou relací. Pokud relace došlo k úniku, odpojí od hello Engagement back-end nikdy hello zapojení služby (od služby hello zůstane připojen tak dlouho, dokud relace čeká na vyřízení).
> 
> 

Zde naleznete příklad:

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

Tato ukázka je podobné toohello `EngagementActivity` třídy a jeho variant, jejichž zdrojový kód je k dispozici v hello `src` složky.

## <a name="using-applicationoncreate"></a>Pomocí Application.onCreate()
Kód, umístěte do `Application.onCreate()` a v jiné aplikaci zpětná volání spouští se pro všechny aplikace procesů, včetně služby zapojení hello. Může mít nežádoucí vedlejší účinky, jako je přidělování nepotřebné paměti a vláken v procesu hello Engagement, nebo duplicitní vysílání příjemci nebo služby.

Pokud přepíšete `Application.onCreate()`, doporučujeme, abyste přidáním hello následující fragment kódu na začátku hello vaší `Application.onCreate()` funkce:

     public void onCreate()
     {
       if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
         return;

       ... Your code...
     }

Můžete provést hello samé pro `Application.onTerminate()`, `Application.onLowMemory()`, a `Application.onConfigurationChanged(...)`.

Můžete také rozšířit `EngagementApplication` místo rozšíření `Application`: hello zpětného volání `Application.onCreate()` hello procesu kontroly a volání `Application.onApplicationProcessCreate()` jen pokud aktuální proces hello není hello jedné hostování hello zapojení služby, hello stejná pravidla platí pro Hello jiných zpětných volání.

## <a name="tags-in-hello-androidmanifestxml-file"></a>Značky v souboru AndroidManifest.xml hello
Ve značce hello služby v souboru AndroidManifest.xml hello, hello `android:label` atribut vám umožní toochoose hello název hello zapojení služby jak se zobrazuje uživatelům tooend v obrazovce "Spuštění služby" hello svůj telefon. Je vhodné nastavení tohoto atributu příliš`"<Your application name>Service"` (například `"AcmeFunGameService"`).

Zadání hello `android:process` atribut zajistí, že hello Engagement service běží ve svém vlastním procesu (spouštění Engagement v hello stejný proces vaše aplikace provede potenciálně pomaleji vlákna vaší hlavní nebo uživatelského rozhraní).

## <a name="building-with-proguard"></a>Sestavování s použitím ProGuard
Pokud vytvoříte balíčku aplikace s ProGuard, musíte tookeep některé třídy. Můžete použít následující fragment kódu konfigurace hello:

    -keep public class * extends android.os.IInterface
    -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
    <methods>;
     }
