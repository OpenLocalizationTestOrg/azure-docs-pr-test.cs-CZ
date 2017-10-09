---
title: aaaAzure integraci sady Android SDK Mobile Engagement
description: "Nejnovější aktualizace a postupy pro Android SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 9ec3fab3-35ec-458e-bf41-6cdd69e3fa44
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 06/27/2016
ms.author: piyushjo
ms.openlocfilehash: 4ab6143771bdc0758a548abb529d6bde98fc0e4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-engagement-reach-on-android"></a>Jak tooIntegrate Engagement Reach pro Android
> [!IMPORTANT]
> Postupujte podle hello integrace postup popsaný v hello jak tooIntegrate Engagement v systému Android dokumentu před těchto pokynů.
> 
> 

## <a name="standard-integration"></a>Standardní integrace

Zkopírujte soubory prostředků Reach z hello SDK do projektu:

* Zkopírujte soubory hello z hello `res/layout` složky doručit s hello SDK do hello `res/layout` složky vaší aplikace.
* Zkopírujte soubory hello z hello `res/drawable` složky doručit s hello SDK do hello `res/drawable` složky vaší aplikace.

Upravit vaše `AndroidManifest.xml` souboru:

* Přidejte následující části hello (mezi hello `<application>` a `</application>` značky):
  
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
          <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
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
* Potřebujete tato oprávnění tooreplay systémová oznámení, které nebyly použity při spuštění (jinak zůstanou na disku, ale už se nezobrazí, skutečně máte tooinclude to).
  
          <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
* Určení ikony pro oznámení (v aplikaci a systému ty, které jsou i) používané kopírování a úpravy hello následující části (mezi hello `<application>` a `</application>` značky):
  
          <meta-data android:name="engagement:reach:notification:icon" android:value="<name_of_icon_WITHOUT_file_extension_and_WITHOUT_'@drawable/'>" />

> [!IMPORTANT]
> Tato část se **povinné** Pokud máte v úmyslu používat systémová oznámení při vytváření kampaně Reach. Android zabrání systémová oznámení bez ikony se zobrazí. Takže pokud není v této části vaši koncoví uživatelé nebudou moct tooreceive je.
> 
> 

* Pokud vytvoříte kampaně s systémová oznámení pomocí velký obrázek, potřebujete následující oprávnění hello tooadd (po hello `</application>` značka) Pokud chybí:
  
          <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
          <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
  
  * V systému Android M a pokud aplikace cílí úroveň rozhraní API systému Android 23 nebo vyšší ``WRITE_EXTERNAL_STORAGE`` oprávnění vyžaduje schválení uživatelem. Přečtěte si prosím [v této části](mobile-engagement-android-integrate-engagement.md#android-m-permissions).
* Pro systémová oznámení, které můžete také zadat v hello dosáhnout kampaň, pokud by se prstence nebo zavibrovat hello zařízení. Pro něj toowork, máte jistotu deklarovaný hello následující oprávnění toomake (po hello `</application>` značka):
  
          <uses-permission android:name="android.permission.VIBRATE" />
  
  Bez tohoto oprávnění zabrání Android systémových oznámení se zobrazí v případě, že je zaškrtnuté hello prstenec nebo hello zavibrovat možnost v manažer kampaně Reach hello.

## <a name="native-push"></a>Nativního nabízení
Teď, když jste nakonfigurovali modul Reach, je nutné tooconfigure nativního nabízení toobe možné tooreceive hello kampaně v zařízení hello.

V systému Android podporujeme dvě služby:

* Google Play zařízení: použití [Google Cloud Messaging] podle následující hello [jak tooIntegrate GCM s Engagement průvodce](mobile-engagement-android-gcm-integrate.md) průvodce.
* Zařízení Amazon: použití [Amazon Device Messaging] podle následující hello [jak tooIntegrate ADM s Engagement průvodce](mobile-engagement-android-adm-integrate.md) průvodce.

Pokud chcete, aby tootarget Amazon a webu Google Play zařízení, jeho možných toohave všechno uvnitř 1 AndroidManifest.xml/APK pro vývoj. Ale při odesílání tooAmazon, pokud se najdou GCM kódu může zamítnutí vaší aplikace.

V takovém případě byste měli používat více APKs.

**Aplikace je nyní připraven tooreceive a zobrazení kampaně reach!**

## <a name="how-toohandle-data-push"></a>Jak nabízená toohandle dat
### <a name="integration"></a>Integrace
Pokud chcete toobe vaší aplikací mít tooreceive Reach data nabízených oznámení, je třeba toocreate podtřídou třídy `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` a odkazujete v hello `AndroidManifest.xml` souboru (mezi hello `<application>` nebo `</application>` značky):

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

Pak můžete přepsat hello `onDataPushStringReceived` a `onDataPushBase64Received` zpětných volání. Zde naleznete příklad:

            public class MyDataPushReceiver extends EngagementReachDataPushReceiver
            {
              @Override
              protected Boolean onDataPushStringReceived(Context context, String category, String body)
              {
                Log.d("tmp", "String data push message received: " + body);
                return true;
              }

              @Override
              protected Boolean onDataPushBase64Received(Context context, String category, byte[] decodedBody, String encodedBody)
              {
                Log.d("tmp", "Base64 data push message received: " + encodedBody);
                // Do something useful with decodedBody like updating an image view
                return true;
              }
            }

### <a name="category"></a>Kategorie
Hello kategorie parametr je volitelný při vytváření kampaň nabízených Data a umožňuje že vám toofilter data nabízených oznámení. To je užitečné, pokud máte několik všesměrového vysílání příjemci zpracování různé typy dat nabízených oznámení, nebo pokud chcete různé typy toopush z `Base64` dat a chcete tooidentify jejich typů před analýzou je.

### <a name="callbacks-return-parameter"></a>Návratový parametr zpětných volání.
Tady jsou některé pokyny tooproperly popisovač hello návratový parametr `onDataPushStringReceived` a `onDataPushBase64Received`:

* Všesměrového vysílání příjemce by měla vrátit `null` v hello zpětného volání, pokud nebude vědět, jak nabízená toohandle data. Měli byste použít hello kategorie toodetermine zda přijímač všesměrového vysílání by měla řídit hello datová oznámení, nebo ne.
* Jeden z hello všesměrového vysílání příjemce by měla vrátit `true` v hello zpětného volání, pokud ji přijímá hello datová oznámení.
* Jeden z hello všesměrového vysílání příjemce by měla vrátit `false` v hello zpětného volání, pokud rozpozná hello datová oznámení, ale zahodí z jakéhokoliv důvodu. Například vrátit `false` při hello přijal data jsou neplatná.
* Pokud jeden vysílání příjemce vrátí `true` při jiné jeden vrátí `false` pro hello stejné datová oznámení, hello chování není definován, měli byste nikdy udělat.

Hello návratový typ se používá pouze pro hello Reach statistiky:

* `Replied`se zvýší, pokud jeden z všesměrového vysílání příjemci hello vrácen buď `true` nebo `false`.
* `Actioned`se zvýší, pouze v případě, že jeden z hello vysílání příjemci vrátil `true`.

## <a name="how-toocustomize-campaigns"></a>Jak toocustomize kampaně
toocustomize kampaní, můžete upravit rozložení hello součástí hello Reach SDK.

Měli byste zachovat všechny identifikátory hello použité v hello rozložení a hello typy hello zobrazení, která použijte identifikátor, hlavně pro zobrazení textu zobrazení bitové kopie a zachovat. Některá zobrazení jsou právě používané toohide nebo zobrazit oblastí, jejich typ může být změněn. Pokud máte v úmyslu toochange hello typ zobrazení v hello zadat rozložení zkontrolujte hello zdrojového kódu.

### <a name="notifications"></a>Oznámení
Existují dva typy oznámení: systém a v aplikaci oznámení, které používají různé rozložení soubory.

#### <a name="system-notifications"></a>Systémová oznámení
Systémová oznámení toocustomize potřebujete toouse hello **kategorie**. Můžete přeskočit příliš[kategorie](#categories).

#### <a name="in-app-notifications"></a>Oznámení v aplikaci
Ve výchozím nastavení, oznámení v aplikaci je zobrazení, které je dynamicky přidané toohello aktuální aktivity uživatelské rozhraní Děkujeme toohello Android metoda `addContentView()`. Tomu se říká překrytí oznámení. Překryvy oznámení jsou skvělý pro rychlé integraci, protože nevyžadují toomodify jste žádné rozložení v aplikaci.

Vzhled hello toomodify vaše překryvy oznámení, jednoduše soubor můžete upravit hello `engagement_notification_area.xml` tooyour potřebuje.

> [!NOTE]
> soubor Hello `engagement_notification_overlay.xml` je ten, který je použité toocreate hello oznámení překrytí, obsahuje soubor hello `engagement_notification_area.xml`. Můžete taky přizpůsobit ho toosuit vašim potřebám (například pro umístění hello oznamovací oblasti v rámci překrytí hello).
> 
> 

##### <a name="include-notification-layout-as-part-of-an-activity-layout"></a>Zahrnout rozložení oznámení jako součást rozložení aktivity
Překryvy se výborně hodí pro rychlé integrace, ale můžete bylo nepraktické nebo mít vedlejší účinky ve zvláštních případech. Hello překrytí systému lze přizpůsobit na úrovni aktivity, takže je easy tooprevent vedlejší účinky pro speciální aktivity.

Můžete rozhodnout tooinclude naše rozložení oznámení ve vaší stávající toohello Děkujeme rozložení Android **zahrnují** příkaz. Hello tady je příklad upravené `ListActivity` rozložení obsahující jenom `ListView`.

**Před integraci sady Engagement:**

            <?xml version="1.0" encoding="utf-8"?>
            <ListView
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@android:id/list"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent" />

**Po integraci sady Engagement:**

            <?xml version="1.0" encoding="utf-8"?>
            <LinearLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <ListView
                android:id="@android:id/list"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:layout_weight="1" />

              <include layout="@layout/engagement_notification_area" />

            </LinearLayout>

V tomto příkladu jsme přidali nadřazený kontejner, protože původní rozložení hello používá zobrazení seznamu jako nejvyšší úrovně element hello. Jsme přidali i `android:layout_weight="1"` toobe možné tooadd nakonfigurované níže zobrazení seznamu zobrazení `android:layout_height="fill_parent"`.

Hello Engagement Reach SDK automaticky rozpozná, že rozložení oznámení hello je součástí této aktivity a nepřidá překrytí pro tuto aktivitu.

> [!TIP]
> Pokud používáte ve vaší aplikaci ListActivity, viditelné překrytí Reach zabrání reagující tooclicked položky v zobrazení seznamu hello už. Jedná se o známý problém. toowork tento problém vyřešit, doporučujeme vám tooembed hello oznámení rozložení v rozložení vlastní seznam aktivitu jako v předchozím příkladu hello.
> 
> 

##### <a name="disabling-application-notification-per-activity"></a>Zakázat oznámení aplikace na aktivitu
Pokud nechcete, aby hello překrytí toobe přidány tooyour aktivity, a pokud nezadáte hello oznámení rozložení v vlastní rozložení, můžete zakázat hello překrytí pro tuto aktivitu v hello `AndroidManifest.xml` přidáním `meta-data` části stejně jako v nástroji následující hello Příklad:

            <activity android:name="SplashScreenActivity">
              <meta-data android:name="engagement:notification:overlay" android:value="false"/>
            </activity>

#### <a name="categories"></a>Kategorie
Při úpravě hello zadat rozložení upravíte vzhled hello všechna oznámení. Kategorie povolit, že jste toodefine, které se různé cílové hledá oznámení (pravděpodobně chování). Kategorie lze při vytváření kampaně Reach. Mějte na paměti, že kategorie vám také umožní přizpůsobit oznámení a hlasování, který je popsán dále v tomto dokumentu.

tooregister kategorie obslužnou rutinu pro oznámení, musíte tooadd volání při inicializaci aplikace hello.

> [!IMPORTANT]
> Přečtěte si upozornění hello o atribut android: proces hello \<android-sdk-engagement-process\> v hello jak tooIntegrate Engagement na Android tématu než budete pokračovat.
> 
> 

Hello následující příklad předpokládá potvrzeny hello předchozího upozornění a použít podtřídou třídy `EngagementApplication`:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), "myCategory");
              }
            }

Hello `MyNotifier` objekt je implementace hello hello oznámení kategorie obslužné rutiny. Jedná se buď implementaci hello `EngagementNotifier` rozhraní nebo dílčí třídu hello výchozí implementace: `EngagementDefaultNotifier`.

Všimněte si, že hello téhož oznamovatele dokáže zpracovat několika kategorií, zaregistrujte je takto:

            reachAgent.registerNotifier(new MyNotifier(this), "myCategory", "myAnotherCategory");

tooreplace hello výchozí kategorie implementace implementaci jako můžete zaregistrovat v hello následující ukázka:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), Intent.CATEGORY_DEFAULT); // "android.intent.category.DEFAULT"
              }
            }

aktuální kategorie Hello použitá v obslužné rutině se předá jako parametr v většinu metod, můžete přepsat v `EngagementDefaultNotifier`.

Je předán buď jako `String` parametr nebo nepřímo v `EngagementReachContent` objekt, který má `getCategory()` metoda.

Můžete změnit většinu proces vytváření oznámení hello opětovná definice metody na `EngagementDefaultNotifier`pro pokročilejší přizpůsobení myslíte, že volné tootake podívejte se v technické dokumentaci hello a hello zdrojového kódu.

##### <a name="in-app-notifications"></a>Oznámení v aplikaci
Pokud chcete toouse alternativní rozložení pro určitou kategorii, můžete to implementovat jako hello následující ukázka:

            public class MyNotifier extends EngagementDefaultNotifier
            {
              public MyNotifier(Context context)
              {
                super(context);
              }

              @Override
              protected int getOverlayLayoutId(String category)
              {
                return R.layout.my_notification_overlay;
              }


              @Override
              public Integer getOverlayViewId(String category)
              {
                return R.id.my_notification_overlay;
              }

              @Override
              public Integer getInAppAreaId(String category)
              {
                return R.id.my_notification_area;
              }
            }

**Příklad `my_notification_overlay.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <RelativeLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/my_notification_overlay"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <include layout="@layout/my_notification_area" />

            </RelativeLayout>

Jak vidíte, se liší od standardní hello jeden identifikátor zobrazit překrytí hello. Je důležité, aby každé rozložení použít jedinečný identifikátor pro překryvy.

**Příklad `my_notification_area.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <merge
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <RelativeLayout
                android:id="@+id/my_notification_area"
                android:layout_width="fill_parent"
                android:layout_height="64dp"
                android:layout_alignParentTop="true"
                android:background="#B000">

                <LinearLayout
                  android:orientation="horizontal"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent"
                  android:gravity="center_vertical">

                  <ImageView
                    android:id="@+id/engagement_notification_icon"
                    android:layout_width="48dp"
                    android:layout_height="48dp" />

                  <LinearLayout
                    android:id="@+id/engagement_notification_text"
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent"
                    android:layout_weight="1"
                    android:gravity="center_vertical">

                    <TextView
                      android:id="@+id/engagement_notification_title"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:singleLine="true"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Medium" />

                    <TextView
                      android:id="@+id/engagement_notification_message"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:maxLines="2"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Small" />

                  </LinearLayout>

                  <ImageView
                    android:id="@+id/engagement_notification_image"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:adjustViewBounds="true" />

                  <ImageButton
                    android:id="@+id/engagement_notification_close_area"
                    android:visibility="invisible"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:src="@android:drawable/btn_dialog"
                    android:background="#0F00" />

                </LinearLayout>

                <ImageButton
                  android:id="@+id/engagement_notification_close"
                  android:layout_width="wrap_content"
                  android:layout_height="fill_parent"
                  android:layout_alignParentRight="true"
                  android:src="@android:drawable/btn_dialog"
                  android:background="#0F00" />

              </RelativeLayout>

            </merge>

Jak vidíte, se liší od standardní hello jeden hello oznámení oblasti zobrazení identifikátor. Je důležité, že každé rozložení používá jedinečný identifikátor pro oznamovací oblasti.

Tento jednoduchý příklad kategorie umožňuje aplikace (nebo v aplikaci) oznámení zobrazí v horní části hello obrazovky hello. Jsme nezměnila hello standardní identifikátory používané v oznamovací oblasti hello sám sebe.

Pokud chcete, aby toochange, že máte tooredefine hello `EngagementDefaultNotifier.prepareInAppArea` metoda. Je doporučeno toolook v hello technické dokumentace a zdrojový kód hello `EngagementNotifier` a `EngagementDefaultNotifier` Pokud chcete tato úroveň rozšířené úpravy.

##### <a name="system-notifications"></a>Systémová oznámení
Tím, že rozšíří `EngagementDefaultNotifier`, můžete přepsat `onNotificationPrepared` tooalter hello oznámení, že byl připraven výchozí implementací hello.

Například:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content)
              throws RuntimeException
            {
              if ("ongoing".equals(content.getCategory()))
                notification.flags |= Notification.FLAG_ONGOING_EVENT;
              return true;
            }

Tento příklad vytvoří systémové oznámení pro obsah, se zobrazuje jako probíhající událost v případě, že se používá kategorie "probíhající" hello.

Pokud chcete, aby toobuild hello `Notification` objektu od začátku, můžete se vrátit `false` toohello metoda a volání `notify` sami na hello `NotificationManager`. V takovém případě je důležité, aby byl `contentIntent`, `deleteIntent` a hello používá identifikátor oznámení `EngagementReachReceiver`.

Tady je správný příklad takových implementace:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content) throws RuntimeException
            {
              /* Required fields */
              NotificationCompat.Builder builder = new NotificationCompat.Builder(mContext)
                .setSmallIcon(notification.icon)              // icon is mandatory
                .setContentIntent(notification.contentIntent) // keep content intent
                .setDeleteIntent(notification.deleteIntent);  // keep delete intent

              /* Your customization */
              // builder.set...

              /* Dismiss option can be managed only after build */
              Notification myNotification = builder.build();
              if (!content.isNotificationCloseable())
                myNotification.flags |= Notification.FLAG_NO_CLEAR;

              /* Notify here instead of super class */
              NotificationManager manager = (NotificationManager) mContext.getSystemService(Context.NOTIFICATION_SERVICE);
              manager.notify(getNotificationId(content), myNotification); // notice hello call tooget hello right identifier

              /* Return false, we notify ourselves */
              return false;
            }

##### <a name="notification-only-announcements"></a>Oznámení pouze oznámení
Hello správu hello kliknutím na oznámení pouze oznámení lze přizpůsobit přepsáním `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` toomodify hello připravený `Intent`. Pomocí této metody můžete tootune hello příznaky snadno.

Například tooadd hello `SINGLE_TOP` příznak:

            @Override
            protected Intent onNotifAnnouncementIntentPrepared(EngagementNotifAnnouncement notifAnnouncement,
              Intent intent)
            {
              intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
              return intent;
            }

Pro starší verze zapojení uživatelů Pamatujte, že systémová oznámení bez akce URL teď spustí aplikace hello Pokud byl v pozadí, takže tato metoda může být volána s hlášení bez adresa URL akce. Měli byste zvážit, když přizpůsobení hello záměr.

Můžete taky implementovat `EngagementNotifier.executeNotifAnnouncementAction` od začátku.

##### <a name="notification-life-cycle"></a>Oznámení životního cyklu
Při použití hello výchozí kategorie, se nazývají některé metody životní cyklus na hello `EngagementReachInteractiveContent` objektu tooreport statistiky a aktualizace hello kampaň stavu:

* Když hello oznámení se zobrazí v aplikaci nebo umístit do hello stavového řádku, hello `displayNotification` metoda je volána (který sestavy statistik) podle `EngagementReachAgent` Pokud `handleNotification` vrátí `true`.
* Pokud se zavře hello oznámení, hello `exitNotification` metoda je volána, statistiky se použije v hlášení a další kampaně lze nyní zpracovat.
* Po kliknutí na oznámení hello `actionNotification` je volána, statistiky se použije v hlášení a spuštění hello přidružené záměr.

Pokud vaši implementaci `EngagementNotifier` přeskočení hello výchozí chování, máte tyto metody životního cyklu pro toocall samotnými. Hello následující příklady ilustrují některých případech, kde přeskočí hello výchozí chování:

* Nerozšíříte `EngagementDefaultNotifier`, například implementována kategorie zpracování od začátku.
* Pro systémová oznámení overrode hello `onNotificationPrepared` a můžete změnit `contentIntent` nebo `deleteIntent` v hello `Notification` objektu.
* Oznámení v aplikaci, overrode `prepareInAppArea`, že toomap alespoň `actionNotification` tooone U.I ovládacích prvků.

> [!NOTE]
> Pokud `handleNotification` vyvolá výjimku, hello obsahu se odstraní a `dropContent` je volána. To je uvedený v statistiky a další kampaně lze nyní zpracovat.
> 
> 

### <a name="announcements-and-polls"></a>Oznámení a hlasování
#### <a name="layouts"></a>Rozložení
Můžete upravit hello `engagement_text_announcement.xml`, `engagement_web_announcement.xml` a `engagement_poll.xml` soubory toocustomize textu oznámení, oznámení webové a hlasování.

Tyto soubory sdílet dvě běžné rozložení pro oblast hello nadpisu a hello tlačítko oblasti. Hello rozložení pro nadpis hello `engagement_content_title.xml` a používá hello eponymous drawable soubor pro pozadí hello. Hello rozložení pro tlačítek akce a ukončení hello je `engagement_button_bar.xml` a používá hello eponymous drawable soubor pro pozadí hello.

V hlasování, hello otázku rozložení a jejich možnosti jsou dynamicky zvětšený pomocí několikrát hello `engagement_question.xml` rozložení souboru hello otázky a hello `engagement_choice.xml` souboru hello volby.

#### <a name="categories"></a>Kategorie
##### <a name="alternate-layouts"></a>Alternativní rozložení
Jako oznámení může být hello kampaň kategorie používané toohave alternativní rozložení pro oznámení a hlasování.

Například toocreate kategorii pro text oznámení, můžete rozšířit `EngagementTextAnnouncementActivity` a odkazovat na hello `AndroidManifest.xml` souboru:

            <activity android:name="com.your_company.MyCustomTextAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>

Všimněte si této kategorie hello v hello záměr filtr se používá toomake hello rozdíl oproti hello výchozí oznámení aktivita.

Hello Reach SDK používá hello záměrné systému tooresolve hello vpravo aktivity pro určitou kategorii a jeho spadne zpět na výchozí kategorie hello Pokud hello řešení se nezdařilo.

Pak máte tooimplement `MyCustomTextAnnouncementActivity`, pokud jste právě má toochange hello rozložení (ale zachovat stejné zobrazit identifikátory hello), máte právě toodefine hello třídy, jako třeba v hello následující ukázka:

            public class MyCustomTextAnnouncementActivity extends EngagementTextAnnouncementActivity
            {
              @Override
              protected String getLayoutName()
              {
                return "my_text_announcement";  // tell super class toouse R.layout.my_text_announcement
              }
            }

kategorie výchozí hello tooreplace textu oznámení, jednoduše nahradit `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` podle vaší implementace.

Podobně lze přizpůsobit web oznámení a hlasování.

Pro web oznámení můžete rozšířit `EngagementWebAnnouncementActivity` a deklarovat vaše aktivity v hello `AndroidManifest.xml` stejně jako v nástroji hello následující ukázka:

            <activity android:name="com.your_company.MyCustomWebAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/html" />    <!-- only difference with text announcements in hello intent is hello data mime type -->
              </intent-filter>
            </activity>

Pro dotazování můžete rozšířit `EngagementPollActivity` a deklarovat vaší v hello `AndroidManifest.xml` stejně jako v nástroji hello následující ukázka:

            <activity android:name="com.your_company.MyCustomPollActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="my_category" />
              </intent-filter>
            </activity>

##### <a name="implementation-from-scratch"></a>Implementace od začátku
Kategorie můžete implementovat vaše aktivity oznámení (a dotazování) bez rozšíření mezi hello `Engagement*Activity` třídy poskytované hello Reach SDK. To je užitečné, například pokud budete chtít toodefine rozložení, který nepoužívá hello stejné zobrazení jako standardní rozložení hello.

Jako přizpůsobení pokročilé oznámení se doporučuje toolook v hello zdrojový kód standardní implementace hello.

Tady jsou některé věci tookeep pamatovat: Reach spustí aktivita hello konkrétní záměrem (odpovídající záměrné filtr toohello) plus další parametr, což je identifikátor obsahu hello.

To můžete provést hello tooretrieve obsahu se objekt, který obsahovat hello pole, které jste zadali při vytváření hello kampaně na hello webu můžete:

            public class MyCustomTextAnnouncement extends EngagementActivity
            {
              private EngagementAnnouncement mContent;

              @Override
              protected void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);

                /* Get content */
                mContent = EngagementReachAgent.getInstance(this).getContent(getIntent());
                if (mContent == null)
                {
                  /* If problem with content, exit */
                  finish();
                  return;
                }

                setContentView(R.layout.my_text_announcement);

                /* Configure views by querying fields on mContent */
                // ...
              }
            }

Pro statistiku, byste měli hlásit hello obsah se zobrazí v hello `onResume` událostí:

            @Override
            protected void onResume()
            {
             /* Mark hello content displayed */
             mContent.displayContent(this);
             super.onResume();
            }

Potom toocall buď nezapomeňte `actionContent(this)` nebo `exitContent(this)` v obsahu objektu hello před hello aktivity přejde do pozadí.

Pokud nemůžete volat buď `actionContent` nebo `exitContent`, statistiky se neodešlou (tj. žádné analýzy kampaň hello) a další je nejdůležitější – hello další kampaně nebudete upozorněni, až po restartování procesu aplikace hello.

Orientace nebo jiné změny konfigurace můžete provést hello kód složité toodetermine, zda hello aktivity přejde do pozadí nebo Ne, hello díky standardní implementace zda obsah hello hlášení jako opustil Pokud hello uživatel opustí hello aktivity, (buď Stisknutím `HOME` nebo `BACK`), ale ne v případě změny hello orientace.

Tady je hello zajímavé součást hello implementace:

            @Override
            protected void onUserLeaveHint()
            {
              finish();
            }

            @Override
            protected void onPause()
            {
              if (isFinishing() && mContent != null)
              {
                /*
                 * Exit content on exit, this is has no effect if another process method has already been
                 * called so we don't have toocheck anything here.
                 */
                mContent.exitContent(this);
              }
              super.onPause();
            }

Jak můžete vidět, pokud jste volali metodu `actionContent(this)` pak dokončení aktivity hello `exitContent(this)` lze bezpečně volat bez nutnosti nijak neprojeví.

[here]:http://developer.android.com/tools/extras/support-library.html#Downloading
[Google Cloud Messaging]:http://developer.android.com/guide/google/gcm/index.html
[Amazon Device Messaging]:https://developer.amazon.com/sdk/adm.html
