---
title: aaaAzure integraci sady Android SDK Mobile Engagement
description: "Nejnovější aktualizace a postupy pro Android SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 11618586-c709-49ca-bcd8-745323ff1af6
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: df5c82812fe0a242eaa5df8c906030237215b7eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-procedures"></a>Postupy upgradu
Pokud již máte integrovanou starší verze naše sady SDK do své aplikace, musíte tooconsider hello následující body při upgradu hello SDK.

Toofollow může mít několik postupů, pokud provedena několik verzí hello SDK. Například pokud migrujete z 1.4.0 too1.6.0 máte toofirst postupujte podle hello "z 1.4.0 too1.5.0" postup pak hello "z 1.5.0 too1.6.0" postup.

Ať hello verze upgradujete, budete mít tooreplace hello `mobile-engagement-VERSION.jar` s hello nový.

## <a name="from-420-too421"></a>Z 4.2.0 too4.2.1
Tento krok lze provést ve skutečnosti na libovolnou verzi systému hello SDK, je zvýšení zabezpečení při integraci Reach aktivity.

Nyní byste měli přidat `exported="false"` tooall Reach aktivity.

Reach aktivity by teď měl vypadat takto vaše `AndroidManifest.xml`:

            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>

## <a name="from-400-too410"></a>Z 4.0.0 too4.1.0
Hello SDK nyní popisovač nové oprávnění modelu ze systému Android M.

Pokud používáte umístění funkcí nebo oznámení velký obrázek přečtěte [v této části](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

Kromě toho toohello nový model oprávnění, teď podporujeme konfiguraci funkcí umístění za běhu.
Snažíme se stále kompatibilní s hello manifestu parametry pro umístění, ale je nyní zastaralý. Konfigurace modulu runtime toouse, odeberte hello následující části z vaší ``AndroidManifest.xml``:

    <meta-data
      android:name="engagement:locationReport:lazyArea"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:background"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:fine"
      android:value="true"/>

a číst [Tato aktualizuje postup](mobile-engagement-android-integrate-engagement.md#location-reporting) konfigurace modulu runtime toouse místo.

## <a name="from-300-too400"></a>Z 3.0.0 too4.0.0
### <a name="native-push"></a>Nativního nabízení
Nativního nabízení (GCM/ADM) se teď také používá pro oznámení v aplikaci, musíte nakonfigurovat přihlašovací údaje nativního nabízení hello pro jakýkoli typ nabízené kampaně.

Není-li již postupujte [tento postup](mobile-engagement-android-integrate-engagement-reach.md#native-push).

### <a name="androidmanifestxml"></a>AndroidManifest.xml
Integrace reach byl změněn v ``AndroidManifest.xml``.

Nahraďte toto:

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>

Od společnosti

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>
    <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
      <intent-filter>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
      </intent-filter>
    </receiver>

Je to pravděpodobně načítání obrazovky teď kliknutím na oznámení (s text nebo webového obsahu) nebo hlasování.
Máte tooadd to pro tyto toowork kampaně v 4.0.0:

    <activity
      android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity"
      android:theme="@android:style/Theme.Dialog">
      <intent-filter>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
        <category android:name="android.intent.category.DEFAULT"/>
      </intent-filter>
    </activity>

### <a name="resources"></a>Zdroje
Vložení nové hello `res/layout/engagement_loading.xml` souboru do projektu.

## <a name="from-240-too300"></a>Z 2.4.0 too3.0.0
Hello následující text popisuje, jak toomigrate integraci sady SDK z hello Capptain služby nabízených Capptain SAS do aplikace používá technologii Azure Mobile Engagement. Pokud provádíte migraci ze starší verze, informujte hello Capptain webu toomigrate too2.4.0 nejprve a potom použijte hello následující postup.

> [!IMPORTANT]
> Capptain Mobile Engagement není hello stejné služby a hello postup vypsáni níže pouze označuje, jak toomigrate hello klientskou aplikaci. Migrace hello SDK v aplikaci hello se NEPROVÁDÍ migraci dat ze sady Mobile Engagement pro toohello hello Capptain servery.
> 
> 

### <a name="jar-file"></a>Soubor JAR
Nahraďte `capptain.jar` podle `mobile-engagement-VERSION.jar` ve vaší `libs` složky.

### <a name="resource-files"></a>Soubory prostředků
Každý soubor prostředků, který jsme zadali (s předponou podle `capptain_`) toobe nahradili hello nové (s předponou `engagement_`).

Pokud jste si přizpůsobili těchto souborů, máte toore-použít vlastní na hello nové soubory, **všechny identifikátory hello v souborech prostředků hello také přejmenovaná**.

### <a name="application-id"></a>ID aplikace
Teď používá Engagement SDK identifikátorů připojovacího řetězce tooconfigure hello například identifikátor aplikace hello.

Máte toouse `EngagementAgent.init` metoda aktivitou Spouštěče takto:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

Hello připojovací řetězec pro vaši aplikaci se zobrazí na portálu Azure.

Odeberte všechny volání příliš`CapptainAgent.configure` jako `EngagementAgent.init` nahrazuje dané metody.

Hello `appId` již jde konfigurovat pomocí `AndroidManifest.xml`.

Odeberte z této části vaší `AndroidManifest.xml` pokud:

            <meta-data android:name="capptain:appId" android:value="<YOUR_APPID>"/>

### <a name="java-api"></a>Java API
Každé volání tooany třída jazyka Java naše SDK má toobe přejmenovat; například `CapptainAgent.getInstance(this)` musí být přejmenován `EngagementAgent.getInstance(this)`, `extends CapptainActivity` musí být přejmenován `extends EngagementActivity` atd...

Pokud byly integrovány s výchozí agenta předvoleb soubory, hello výchozí název souboru je nyní `engagement.agent` a hello je klíč `engagement:agent`.

Při vytváření webové oznámení, hello vazač Javascript je nyní `engagementReachContent`.

### <a name="androidmanifestxml"></a>AndroidManifest.xml
Mnoho změn došlo k dispozici, není sdíleny hello služby a spoustu příjemci už nejsou exportovatelný.

deklarace Hello služby je teď jednodušší; Odeberte hello záměrné filtru a všechny metadat je uvnitř a přidejte `exportable=false`.

Plus všechno, co je přejmenován toouse zapojení.

Teď vypadá takto:

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

Když chcete tooenable protokolů testování, hello metadata byla nyní přesunout toohello aplikace značky a bylo přejmenováno:

            <application>

              <meta-data android:name="engagement:log:test" android:value="true" />

              <service/>

            </application>

Všechny ostatní metadata právě přejmenovaná, zde je úplný seznam hello (samozřejmě přejmenovat pouze hello ty, které jsou použijete):

            <meta-data
              android:name="engagement:reportCrash"
              android:value="true"/>
            <meta-data
              android:name="engagement:sessionTimeout"
              android:value="10000"/>
            <meta-data
              android:name="engagement:burstThreshold"
              android:value="0"/>
            <meta-data
              android:name="engagement:connection:delay"
              android:value="0"/>
            <meta-data
              android:name="engagement:locationReport:lazyArea"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:background"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:fine"
              android:value="false"/>
            <meta-data
              android:name="engagement:agent:settings:name"
              android:value="engagement.agent"/>
            <meta-data
              android:name="engagement:agent:settings:mode"
              android:value="0"/>
            <meta-data
              android:name="engagement:gcm:sender"
              android:value="<YOUR_PROJECT_NUMBER>\n"/>
            <meta-data
              android:name="engagement:adm:register"
              android:value="true"/>
            <meta-data
              android:name="engagement:reach:notification:icon"
              android:value="<DRAWABLE_NAME_WITHOUT_EXTENSION>"/>

            <activity android:name="SomeActivityWithoutReachOverlay">
              <meta-data
                android:name="engagement:notification:overlay"
                android:value="false"/>
            </activity>

Sledování Google Play a SmartAd byla odebrána ze sady SDK právě máte tooremove to bez nahrazení:

            <meta-data 
                android:name="capptain:track:installReferrerForwardList"
                android:value="com.class1,com.class2"/>
            <meta-data
                android:name="capptain:track:adservers"
                android:value="smartad" />

Hello Reach aktivity jsou nyní deklarované takto:

            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/plain"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/html"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>

Pokud máte vlastní aktivity Reach, potřebujete jenom toochange hello záměrné akce toomatch buď `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` nebo `com.microsoft.azure.engagement.reach.intent.action.POLL`.

Hello všesměrového vysílání příjemci přejmenovaná plus nyní přidáme `exported=false`. Tady je hello úplný seznam příjemců hello nové specifikace hello (samozřejmě přejmenovat pouze hello ty, které jsou použijete):

            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
              android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver
              android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver"
              android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
                <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
              </intent-filter>
            </receiver>

            <receiver
              android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
              android:permission="com.amazon.device.messaging.permission.SEND">
              <intent-filter>
                <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
                <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementConnectionReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.CONNECTED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.DISCONNECTED"/>
              </intent-filter>
            </receiver>

            <receiver
              android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementMessageReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.MESSAGE"/>
              </intent-filter>
            </receiver>

Sledování příjemce byl odebrán, takže budete mít tooremove v této části:

          <receiver android:name="com.ubikod.capptain.android.sdk.track.CapptainTrackReceiver">
            <intent-filter>
              <action android:name="com.ubikod.capptain.intent.action.APPID_GOT" />
              <!-- possibly <action android:name="com.android.vending.INSTALL_REFERRER" /> -->
            </intent-filter>
          </receiver>

Všimněte si, že hello prohlášení o implementaci hello vysílání příjemce **EngagementMessageReceiver** došlo ke změně v hello `AndroidManifest.xml`. To je proto toosend hello rozhraní API a odebrat libovolný protokolu XMPP zprávy z libovolné protokolu XMPP entit a hello toosend rozhraní API a příjem zpráv mezi zařízeními byly odebrány. Proto máte také toodelete hello následující zpětná volání z vaší **EngagementMessageReceiver** implementace:

            protected void onDeviceMessageReceived(android.content.Context context, java.lang.String deviceId, java.lang.String payload)

a

            protected void onXMPPMessageReceived(android.content.Context context, android.os.Bundle message)

potom odstraňte v žádném volání, **EngagementAgent** pro:

            sendMessageToDevice(java.lang.String deviceId, java.lang.String payload, java.lang.String packageName)

a

            sendXMPPMessage(android.os.Bundle msg)

### <a name="proguard"></a>Proguard
Proguard konfigurace může být ovlivněno rebranding hello pravidla jsou nyní vyhledávání jako:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }

            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
              <methods>;
            }

