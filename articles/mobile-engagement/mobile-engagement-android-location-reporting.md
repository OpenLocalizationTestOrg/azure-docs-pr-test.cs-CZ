---
title: "aaaLocation vytváření sestav pro Azure Mobile Engagement Android SDK"
description: "Popisuje, jak umístění tooconfigure vytváření sestav pro Azure Mobile Engagement Android SDK"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6cab5ed1-b767-46ac-9f0b-48a4e249d88c
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: c2cb097df2a77bee2d56ffe9509dc116548db408
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="location-reporting-for-azure-mobile-engagement-android-sdk"></a>Umístění vytváření sestav pro Azure Mobile Engagement Android SDK
> [!div class="op_single_selector"]
> * [Android](mobile-engagement-android-integrate-engagement.md)
> 
> 

Toto téma popisuje, jak umístění toodo vytváření sestav pro aplikace pro Android.

## <a name="prerequisites"></a>Požadavky
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="location-reporting"></a>Rozšířené ohlašování polohy
Pokud chcete, aby oznámil toobe umístění, je třeba tooadd pár řádků konfigurace (mezi hello `<application>` a `</application>` značek).

### <a name="lazy-area-location-reporting"></a>Opožděné hlášení umístění oblasti
Opožděné hlášení umístění oblasti umožňuje vytváření sestav hello zemi, oblast a polohu přidružené k zařízení. Tento typ hlášení umístění používá jenom síťová umístění (na základě ID buňky nebo Wi-Fi). oblasti Hello zařízení se hlásí nejvíce jednou za relace. Hello GPS se nikdy nepoužívá, a proto tento typ umístění sestavy má nízkou dopad na baterie hello.

Hlášené oblasti jsou použité toocompute geografické Statistika týkající se uživatelů, relací, události a chyby. Může být také používány jako kritérium v kampaně Reach.

Umístění opožděné oblasti vytváření sestav pomocí nástroje Konfigurace hello výše v tomto postupu povolíte:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Budete také potřebovat toospecify oprávnění umístění. Tento kód používá ``COARSE`` oprávnění:

    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Pokud vaše aplikace vyžaduje, můžete použít ``ACCESS_FINE_LOCATION`` místo.

### <a name="real-time-location-reporting"></a>Hlášení polohy v reálném čase
Hlášení polohy v reálném čase umožňuje vytváření sestav hello šířky a délky, které jsou přidružené k zařízení. Ve výchozím nastavení používá tento typ hlášení polohy jenom umístění v síti, na základě ID buňky nebo Wi-Fi. Hello reporting je aktivní jenom v případě hello aplikace spuštěná v popředí (například během relace).

V reálném čase umístění jsou *není* používá toocompute statistiky. Jejich jediným účelem je použití hello tooallow v reálném čase geografického vymezení \<monitorování geografických Reach-cílové skupiny zón\> kritérium v kampaně Reach.

umístění v reálném čase tooenable vytváření sestav, přidejte řádek z kódu toowhere nastavíte hello Engagement připojovací řetězec v aktivitě Spouštěče hello. výsledek Hello vypadá hello následující:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

        You also need toospecify a location permission. This code uses ``COARSE`` permission:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

        If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.

#### <a name="gps-based-reporting"></a>Na základě GPS při vytváření sestav
Ve výchozím nastavení hlášení polohy v reálném čase pouze používá síťové umístění. použití hello tooenable na základě GPS umístění, které jsou mnohem přesnější, použijte objekt konfigurace hello:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Musíte taky tooadd hello, pokud chybí následující oprávnění:

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Vytváření sestav pozadí
Ve výchozím nastavení hlášení polohy v reálném čase je aktivní pouze při spuštění aplikace hello v popředí (například během relace). hello tooenable také vytváření sestav v pozadí, použijte tento objekt konfigurace:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [!NOTE]
> Při spuštění aplikace hello v pozadí, jsou hlášeny pouze na základě síťové umístění, i když jste povolili hello GPS.
> 
> 

Pokud uživatel hello restartuje jejich zařízení, je zastavena hello pozadí umístění sestavy. toomake automaticky restartuje při spuštění, přidejte tento kód.

    <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
           android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
        </intent-filter>
    </receiver>

Musíte taky tooadd hello, pokud chybí následující oprávnění:

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

## <a name="android-m-permissions"></a>Android M oprávnění
Od verze Android M, některá oprávnění jsou spravovány v době běhu a vyžadují schválení uživatele.

Pokud cílíte na úrovni rozhraní API systému Android 23, oprávnění hello runtime jsou ve výchozím nastavení vypnuté pro nové instalace aplikace. Jinak jsou zapnuty ve výchozím nastavení.

Vám může povolit nebo zakázat tato oprávnění z nabídky nastavení zařízení hello. Vypnutí oprávnění z nabídky systému hello ukončí procesy na pozadí hello hello aplikace, která je chování systému a nemá žádný vliv na schopnost tooreceive nabízení v pozadí.

V kontextu hello Mobile Engagement umístění reporting hello oprávnění, které vyžadují schválení za běhu jsou:

* `ACCESS_COARSE_LOCATION`
* `ACCESS_FINE_LOCATION`

Požádat o oprávnění z hello uživatele pomocí dialogu standardní systému. Pokud uživatel hello schválí, řekněte ``EngagementAgent`` tootake, který změnit v úvahu v reálném čase. V opačném případě změnu hello je zpracovaná hello další čas hello uživatel spustí hello aplikace.

Tady je toouse ukázkový kód v aktivitě toorequest oprávnění aplikací a k dopředného hello, pokud kladné příliš``EngagementAgent``:

    @Override
    public void onCreate(Bundle savedInstanceState)
    {
      /* Other code... */

      /* Request permissions */
      requestPermissions();
    }

    @TargetApi(Build.VERSION_CODES.M)
    private void requestPermissions()
    {
      /* Avoid crashing if not on Android M */
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M)
      {
        /*
         * Request location permission, but this doesn't explain why it is needed toohello user.
         * hello standard Android documentation explains with more details how toodisplay a rationale activity tooexplain hello user why hello permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of hello same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence hello request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }
