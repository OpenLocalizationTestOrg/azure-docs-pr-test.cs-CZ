---
title: aaaAzure integraci sady Android SDK Mobile Engagement
description: "Nejnovější aktualizace a postupy pro Android SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a5487793-1a12-4f6c-a1cf-587c5a671e6b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 4f79936ea0fa6102023dec2b4682032a4a81fa9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-engagement-on-android"></a>Jak tooIntegrate Engagement v systému Android
> [!div class="op_single_selector"]
> * [Univerzální pro Windows](mobile-engagement-windows-store-integrate-engagement.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-integrate-engagement.md)
> 
> 

Tento postup popisuje hello nejjednodušší způsob, jak tooactivate Engagement analýzy a monitorování funkce ve vaší aplikace Android.

> [!IMPORTANT]
> Vaše minimální úroveň rozhraní API systému Android SDK musí být 10 nebo vyšší (Android 2.3.3 nebo vyšší).
> 
> 

Hello následující kroky jsou že dostatek tooactivates hello protokolů je nutná sestava toocompute všechny statistické údaje týkající se uživatelů, relací, aktivity, dojde k chybě a Technicals. Hello protokolů je nutná sestava toocompute další statistiky jako události a chyby úlohy je třeba provést ručně pomocí rozhraní API Engagement hello (viz [jak toouse hello advanced Mobile Engagement označování rozhraní API v systémem Android](mobile-engagement-android-use-engagement-api.md) od těchto statistiky jsou závislé aplikace.

## <a name="embed-hello-engagement-sdk-and-service-into-your-android-project"></a>Vložení hello Engagement SDK a služby do vašeho projektu Android
Stažení hello sady SDK pro Android z [sem](https://aka.ms/vq9mfn) získat `mobile-engagement-VERSION.jar` a umístí je do hello `libs` složky vašeho projektu Android (vytvořit složky libs hello, pokud neexistuje ještě).

> [!IMPORTANT]
> Pokud vytvoříte balíčku aplikace s ProGuard, musíte tookeep některé třídy. Můžete použít následující fragment kódu konfigurace hello:
> 
> -Udržovat veřejná třída * rozšiřuje android.os.IInterface-zachovat {com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS – třída
> 
> <methods>; }
> 
> 

Zadejte připojovací řetězec Engagement volání hello následující metodu v aktivitě Spouštěče hello:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

Hello připojovací řetězec pro vaši aplikaci se zobrazí na portálu Azure.

* Pokud chybí, přidejte následující Android oprávnění hello (před hello `<application>` značka):
  
          <uses-permission android:name="android.permission.INTERNET"/>
          <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
* Přidejte následující části hello (mezi hello `<application>` a `</application>` značky):
  
          <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>Service"
            android:process=":Engagement"/>
* Změna `<Your application name>` hello název vaší aplikace.

> [!TIP]
> Hello `android:label` atribut vám umožní toochoose hello název hello zapojení služby jak ho uvidí koncoví uživatelé toohello v obrazovce "Spuštění služby" hello svůj telefon. Je doporučeno tooset tento atribut příliš`"<Your application name>Service"` (například `"AcmeFunGameService"`).
> 
> 

Zadání hello `android:process` atribut zajistí, že hello Engagement služba se spustí ve svém vlastním procesu (spuštěnými Engagement na stejný proces vaše aplikace bude vaše vlákna main/uživatelského rozhraní potenciálně pomaleji hello).

> [!NOTE]
> Kód, umístěte do `Application.onCreate()` a dalších zpětná volání aplikace bude spuštěna pro procesy všechny aplikace, včetně služby zapojení hello. Může mít nežádoucí vedlejší efekty (jako přiřazení nepotřebné paměti a vláken v procesu hello zapojení, duplicitní všesměrového vysílání příjemci nebo služby).
> 
> 

Pokud přepíšete `Application.onCreate()`, je doporučené tooadd hello následující fragment kódu na začátku hello vaší `Application.onCreate()` funkce:

             public void onCreate()
             {
               if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
                 return;

               ... Your code...
             }

Můžete provést hello samé pro `Application.onTerminate()`, `Application.onLowMemory()` a `Application.onConfigurationChanged(...)`.

Můžete také rozšířit `EngagementApplication` místo rozšíření `Application`: hello zpětného volání `Application.onCreate()` hello procesu kontroly a volání `Application.onApplicationProcessCreate()` jen pokud aktuální proces hello není hello jedné hostování hello zapojení služby, hello stejná pravidla platí pro Hello jiných zpětných volání.

## <a name="basic-reporting"></a>Vytváření základních sestav
### <a name="recommended-method-overload-your-activity-classes"></a>Doporučená metoda: přetížení vaší `Activity` třídy
V sestavě hello tooactivate pořadí všechny protokoly hello vyžadují zapojení toocompute uživatelů, relací, aktivity, dojde k chybě a technické statistiky máte právě toomake všechny vaše `*Activity` dílčí třídy dědí hello odpovídající `Engagement*Activity` třídy (například pokud vaše aktivita starší verze rozšiřuje `ListActivity`, zkontrolujte ji rozšiřuje `EngagementListActivity`).

**Bez Engagement:**

            package com.company.myapp;

            import android.app.Activity;
            import android.os.Bundle;

            public class MyApp extends Activity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

**S Engagement:**

            package com.company.myapp;

            import com.microsoft.azure.engagement.activity.EngagementActivity;
            import android.os.Bundle;

            public class MyApp extends EngagementActivity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

> [!IMPORTANT]
> Při použití `EngagementListActivity` nebo `EngagementExpandableListActivity`, ujistěte se, že žádném volání, příliš`requestWindowFeature(...);` je provedena před hello volání příliš`super.onCreate(...);`, jinak dojde k chybě.
> 
> 

Tyto třídy můžete najít v hello `src` složku a můžete je zkopírovat do projektu. Hello třídy jsou také v hello **JavaDoc**.

### <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternativní metoda: volání `startActivity()` a `endActivity()` ručně
Pokud nemůžete nebo nechcete, aby toooverload vaše `Activity` třídy, můžete místo toho začínat a končit vaše aktivity voláním `EngagementAgent`na metody přímo.

> [!IMPORTANT]
> Hello Android SDK nikdy nevolá hello `endActivity()` metoda, i když ukončení aplikace hello (v systému Android, jsou ve skutečnosti nikdy ukončit aplikace). Z toho důvodu je *vysoce* doporučená toocall hello `startActivity()` metoda v hello `onResume` zpětného volání z *všechny* vaše aktivity a hello `endActivity()` metoda v hello `onPause()` zpětné volání z *všechny* vašich aktivit. Toto je hello jediný způsob, jak toobe zda relací nebudou úniku. Pokud relace došlo k úniku, hello zapojení služby se nikdy odpojit od hello Engagement back-end (protože hello služby zůstane připojen tak dlouho, dokud relace čeká na vyřízení).
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

Tento příklad velmi podobné toohello `EngagementActivity` třídy a jeho variant, jejichž zdrojový kód je k dispozici v hello `src` složky.

## <a name="test"></a>Test
Nyní Ověřte prosím svoji integraci spuštěním mobilní aplikaci v emulátoru nebo zařízení a ověření, že registruje relaci na kartě monitorování hello.

Další části Hello jsou volitelné.

## <a name="location-reporting"></a>Rozšířené ohlašování polohy
Pokud chcete, aby oznámil toobe umístění, je třeba tooadd pár řádků konfigurace (mezi hello `<application>` a `</application>` značek).

### <a name="lazy-area-location-reporting"></a>Opožděné hlášení umístění oblasti
Opožděné hlášení umístění oblasti umožňuje tooreport hello zemi, oblast a polohu přidružené toodevices. Tento typ hlášení umístění používá jenom síťová umístění (na základě ID buňky nebo Wi-Fi). oblasti Hello zařízení se hlásí nejvíce jednou za relace. Hello GPS se nikdy nepoužívá, a proto tento typ umístění sestavy má jen v několika (toosay není žádná) dopad na baterie hello.

Hlášené oblasti jsou použité toocompute geografické Statistika týkající se uživatelů, relací, události a chyby. Může být také používány jako kritérium v kampaně Reach.

vytváření sestav, můžete provést pomocí nástroje Konfigurace hello výše v tomto postupu umístění opožděné oblasti tooenable:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Musíte taky tooadd hello, pokud chybí následující oprávnění:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Nebo můžete používat ``ACCESS_FINE_LOCATION`` pokud ji už používáte ve vaší aplikaci.

### <a name="real-time-location-reporting"></a>Hlášení polohy reálném čase.
Hlášení polohy reálném čase umožňuje tooreport hello šířky a délky, přidružené toodevices. Ve výchozím nastavení tento typ hlášení umístění používá jenom síťová umístění (na základě ID buňky nebo Wi-Fi) a vytváření sestav hello je aktivní pouze při spuštění aplikace hello v popředí (tj. během relace).

Umístění v reálném čase, jsou *není* používá toocompute statistiky. Jejich jediným účelem je použití hello tooallow geografického vymezení reálném čase \<monitorování geografických Reach-cílové skupiny zón\> kritérium v kampaně Reach.

vytváření sestav, můžete provést pomocí nástroje Konfigurace hello výše v tomto postupu umístění reálném čase tooenable:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Musíte taky tooadd hello, pokud chybí následující oprávnění:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Nebo můžete používat ``ACCESS_FINE_LOCATION`` pokud ji už používáte ve vaší aplikaci.

#### <a name="gps-based-reporting"></a>Na základě GPS při vytváření sestav
Ve výchozím nastavení hlášení polohy reálném čase pouze používá síťové umístění. použití hello tooenable GPS na základě umístění (což je mnohem přesnější), použijte objekt konfigurace hello:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Musíte taky tooadd hello, pokud chybí následující oprávnění:

            <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Vytváření sestav pozadí
Ve výchozím nastavení hlášení polohy reálném čase je aktivní pouze při spuštění aplikace hello v popředí (tj. během relace). vytváření sestav hello tooenable také v pozadí, použijte objekt konfigurace hello:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [!NOTE]
> Při spuštění aplikace hello v pozadí, jsou hlášeny pouze síťové umístění, i když jste povolili hello GPS.
> 
> 

Sestava umístění pozadí Hello se zastaví, pokud uživatel hello restartuje jeho zařízení, můžete přidat tento toomake automaticky restartuje při spuštění:

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

Musíte taky tooadd hello, pokud chybí následující oprávnění:

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

### <a name="android-m-permissions"></a>Android M oprávnění
Od verze Android M, některá oprávnění jsou spravovány v době běhu a vyžaduje schválení uživatelem.

Pokud cílíte na úrovni rozhraní API systému Android 23, bude Hello runtime oprávnění vypnuta ve výchozím nastavení pro nové instalace aplikace. V opačném případě se bude se ve výchozím nastavení zapnuta.

Hello uživatele můžete povolit nebo zakázat tato oprávnění z nabídky nastavení zařízení hello. Vypnutí oprávnění z nabídky systému ukončí procesy na pozadí aplikace hello, to je chování systému a nemá žádný vliv na schopnost tooreceive nabízení v pozadí.

V kontextu hello Mobile engagementu hello oprávnění, které vyžadují schválení za běhu jsou:

* `ACCESS_COARSE_LOCATION`
* `ACCESS_FINE_LOCATION`
* `WRITE_EXTERNAL_STORAGE`(jenom při cílení na úrovni rozhraní API systému Android 23 pro tato)

Hello externího úložiště se používá pouze pro funkce velký obrázek Reach. Pokud zjistíte, tato oprávnění toobe rušivý požádat uživatele, můžete jej odebrat Pokud se používá pouze pro Mobile Engagement, ale hello náklady na zakázání funkce velký obrázek.

Pro funkce hello umístění měli byste požádat o oprávnění toouser pomocí dialogu standardní systému. Pokud uživatel hello schválí, je třeba tootell ``EngagementAgent`` tootake, který změnit v úvahu v reálném čase (jinak změnu hello bude zpracován hello další čas hello uživatel spustí hello aplikaci).

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
         * Request location permission, but this won't explain why it is needed toohello user.
         * hello standard Android documentation explains with more details how toodisplay a rationale activity tooexplain hello user why hello permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of hello same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

        /* Only if you want tookeep features using external storage */
        if (checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.WRITE_EXTERNAL_STORAGE }, 1);
      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence hello request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

## <a name="advanced-reporting"></a>Rozšířená tvorba sestav
Případně, pokud chcete tooreport aplikace konkrétní události, chyb a úlohy, je nutné toouse hello Engagement rozhraní API prostřednictvím metody hello hello `EngagementAgent` třídy. Tato třída objektu může být retreived pomocí volání hello `EngagementAgent.getInstance()` statickou metodu.

Hello Engagement rozhraní API umožňuje toouse, všechny rozšířené možnosti Engagement a jak je popsané v hello tooUse rozhraní API Engagement v systému Android (jako v technické dokumentaci hello hello i `EngagementAgent` třídy).

## <a name="advanced-configuration-in-androidmanifestxml"></a>Pokročilá konfigurace (v AndroidManifest.xml)
### <a name="wake-locks"></a>Zámky probuzení
Pokud chcete, aby toobe jistotu, že statistiky se odesílají v reálném čase, při použití Wi-Fi nebo úvodní obrazovka je vypnutý, přidejte hello následující volitelné oprávnění:

            <uses-permission android:name="android.permission.WAKE_LOCK"/>

### <a name="crash-report"></a>Hlášení o selhání
Pokud chcete toodisable hlášení selhání, přidejte tuto (mezi hello `<application>` a `</application>` značky):

            <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Prahová hodnota shluku
Ve výchozím nastavení sestavy služby zapojení hello protokolů v reálném čase. Pokud vaše aplikace hlásí protokoly velmi často, je lepší toobuffer hello protokoly a tooreport je všechny najednou na pravidelné základní časové (tomu se říká hello "shluků režim"). toodo Ano, přidejte tuto (mezi hello `<application>` a `</application>` značky):

            <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Hello shluků režimu mírně zvýšit hello baterie životnosti ale má vliv na hello monitorování Engagement: všechny doba trvání relace a úlohy se prahová hodnota shluků zaokrouhlené toohello (tedy relací a úlohy kratší, než je prahová hodnota shluků hello nemusí být zobrazeny). Je doporučeno toouse práh shluků než 30000 (30s).

### <a name="session-timeout"></a>Časový limit relace
Ve výchozím nastavení relace je zakončeno 10s po skončení hello jeho poslední aktivita (který obvykle probíhá ve stisknutím hello domů nebo zálohování klíče, nastavení hello phone nečinnosti nebo přechod do jiné aplikace). Toto je tooavoid rozdělení relace každý uživatel hello čas ukončení a velmi rychle vrátit toohello aplikace (který může dojít při mu vyzvednutí bitovou kopii, zkontrolujte, zda oznámení atd.). Můžete chtít toomodify tento parametr. toodo Ano, přidejte tuto (mezi hello `<application>` a `</application>` značky):

            <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a>Zakázání zasílání zpráv protokolu
### <a name="using-a-method-call"></a>Pomocí volání metody
Pokud chcete Engagement toostop odesílání protokolů, můžete volat:

            EngagementAgent.getInstance(context).setEnabled(false);

Toto volání je trvalé: používá soubor sdílet předvolby.

Pokud je při volání této funkce aktivní zapojení, může trvat 1 minutu po dobu toostop služby hello. Ale nespustí hello služby všechny hello příštím spuštění aplikace hello.

Můžete povolit protokol reporting znovu voláním hello stejné fungovat s `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Integrace ve vašem vlastním`PreferenceActivity`
Namísto volání této funkce, můžete také integrovat toto nastavení přímo do existující `PreferenceActivity`.

Zapojení toouse vaše předvolby soubor (s požadovanou režim hello) můžete nakonfigurovat v hello `AndroidManifest.xml` soubor s `application meta-data`:

* Hello `engagement:agent:settings:name` klíč je použité toodefine hello název souboru sdílet předvolby hello.
* Hello `engagement:agent:settings:mode` klíč je použité toodefine hello režim souboru hello sdílet předvolby, měli byste použít hello stejný režim jako v vaší `PreferenceActivity`. Hello režimu, musí být předán jako číslo: Pokud používáte kombinaci konstantní příznaky ve vašem kódu, zkontrolujte hello celkové hodnoty.

Zapojení vždy použít hello `engagement:key` boolean klíč v souboru předvoleb hello ke správě tohoto nastavení.

Následující příklad Hello `AndroidManifest.xml` ukazuje hello výchozí hodnoty:

            <application>
                [...]
                <meta-data
                  android:name="engagement:agent:settings:name"
                  android:value="engagement.agent" />
                <meta-data
                  android:name="engagement:agent:settings:mode"
                  android:value="0" />

Potom můžete přidat `CheckBoxPreference` v vaše předvolby rozložení jako hello jeden následující:

            <CheckBoxPreference
              android:key="engagement:enabled"
              android:defaultValue="true"
              android:title="Use Engagement"
              android:summaryOn="Engagement is enabled."
              android:summaryOff="Engagement is disabled." />

<!-- URLs. -->
[Device API]: http://go.microsoft.com/?linkid=9876094
