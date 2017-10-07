---
title: "aaaSending nabízená oznámení tooAndroid pomocí Azure Notification Hubs a zasílání zpráv cloudu Firebase | Microsoft Docs"
description: "V tomto kurzu zjistíte, jak toouse Azure Notification Hubs a zasílání zpráv cloudu Firebase toopush oznámení tooAndroid zařízení."
services: notification-hubs
documentationcenter: android
keywords: "nabízená oznámení, nabízené oznámení, nabízené oznámení android, fcm, firebase cloud messaging"
author: ysxu
manager: erikre
editor: 
ms.assetid: 02298560-da61-4bbb-b07c-e79bd520e420
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: hero-article
ms.date: 07/14/2016
ms.author: yuaxu
ms.openlocfilehash: d2e57437ac7b0ef77abf048f991043620621e58d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sending-push-notifications-tooandroid-with-azure-notification-hubs"></a>Odesílání nabízených oznámení tooAndroid pomocí Azure Notification Hubs
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Přehled
> [!IMPORTANT]
> Toto téma popisuje nabízená oznámení ve službě Google Firebase Cloud Messaging (FCM). Pokud stále používáte zasílání zpráv cloudu Google (GCM), přečtěte si téma [odesílající nabízená oznámení tooAndroid pomocí Azure Notification Hubs a GCM](notification-hubs-android-push-notification-google-gcm-get-started.md).
> 
> 

Tento kurz ukazuje, jak toouse Azure Notification Hubs a zasílání zpráv cloudu Firebase toosend nabízená oznámení tooan aplikace pro Android.
Vytvoříte prázdnou aplikaci systému Android, která bude přijímat nabízená oznámení pomocí služby Firebase Cloud Messaging (FCM).

[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Hello dokončit kód v tomto kurzu lze stáhnout z webu GitHub [zde](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStartedFirebase).

## <a name="prerequisites"></a>Požadavky
> [!IMPORTANT]
> toocomplete tento kurz, musíte mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).
> 
> 

* Kromě toho aktivní účet Azure tooan uvedených výše, tento kurz vyžaduje hello nejnovější verze [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797).
* Android 2.3 nebo novější pro službu Firebase Cloud Messaging.
* Služba Firebase Cloud Messaging vyžaduje úložiště Google verze 27 nebo novější.
* Služby Google Play 9.0.2 nebo novější pro službu Firebase Cloud Messaging.
* Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy Notification Hubs pro Android Apps.

## <a name="create-a-new-android-studio-project"></a>Vytvoření nového projektu v Android Studiu
1. V nástroji Android Studio spusťte nový projekt Android Studio.
   
       ![Android Studio - new project](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-new-project.png)
2. Zvolte hello **telefon i Tablet** formuláři faktor a hello **minimální SDK** , které chcete toosupport. Pak klikněte na tlačítko **Další**.
   
       ![Android Studio - project creation workflow](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-choose-form-factor.png)
3. Zvolte **prázdná aktivita** hello hlavní aktivitu, klikněte na tlačítko **Další**a potom klikněte na **Dokončit**.

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Vytvoření projektu, který podporuje službu Firebase Cloud Messaging
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-new-notification-hub"></a>Konfigurace nového centra oznámení
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

&emsp;&emsp;6. V hello **nastavení** okno centra oznámení, vyberte **služby oznámení** a potom **Google (GCM)**. Zadejte klíč serveru FCM hello jste zkopírovali dříve z hello [Firebase konzoly](https://firebase.google.com/console/) a klikněte na tlačítko **Uložit**.

&emsp;&emsp;![Azure Notification Hubs – Google (GCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-gcm-api.png)

Vaše centrum oznámení je nyní nakonfigurované toowork s Firebase cloudu Messagin a máte hello připojovací řetězce tooboth registraci vaší aplikace tooreceive a odesílání nabízených oznámení.

## <a id="connecting-app"></a>Připojit vaše Centrum oznámení toohello aplikace
### <a name="add-google-play-services-toohello-project"></a>Přidání projektu toohello služby Google Play
[!INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

### <a name="adding-azure-notification-hubs-libraries"></a>Přidání knihoven Azure Notification Hubs
1. V hello `Build.Gradle` souboru hello **aplikace**, přidejte následující řádky do hello hello **závislosti** části.
   
        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
2. Přidejte následující úložiště po hello hello **závislosti** části.
   
        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }

### <a name="updating-hello-androidmanifestxml"></a>Aktualizace hello AndroidManifest.xml.
1. toosupport FCM, musíme implementovat Instance ID naslouchací proces služby v našem kódu, který se používá příliš[získání registrace tokenů](https://firebase.google.com/docs/cloud-messaging/android/client#sample-register) pomocí [rozhraní API Google FirebaseInstanceId](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId). V tomto kurzu pojmenujeme třídu hello `MyInstanceIDService`. 
   
    Přidejte následující služby definice toohello souboru AndroidManifest.xml uvnitř hello hello `<application>` značky. 
   
        <service android:name=".MyInstanceIDService">
            <intent-filter>
                <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
            </intent-filter>
        </service>
2. Po obdržení tokenu registrace FCM z hello FirebaseInstanceId rozhraní API, budeme ji používat příliš[zaregistrovat hello centra oznámení Azure](notification-hubs-push-notification-registration-management.md). Tato registrace podpoříme pomocí pozadí hello `IntentService` s názvem `RegistrationIntentService`. Tato služba bude také odpovědná za aktualizaci registračního tokenu FCM.
   
    Přidejte následující služby definice toohello souboru AndroidManifest.xml uvnitř hello hello `<application>` značky. 
   
        <service
            android:name=".RegistrationIntentService"
            android:exported="false">
        </service>
3. Také definujeme tooreceive oznámení příjemce. Přidejte následující příjemce definice toohello souboru AndroidManifest.xml uvnitř hello hello `<application>` značky. Nahraďte hello `<your package>` zástupný symbol hello vaší skutečného názvu balíčku zobrazeného v horní hello části hello `AndroidManifest.xml` souboru.
   
        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>
4. Přidání oprávnění níže hello související s hello následující nezbytné FCM `</application>` značky. Ujistěte se, že tooreplace `<your package>` s hello názvu balíčku zobrazeného v horní hello části hello `AndroidManifest.xml` souboru.
   
    Další informace o těchto oprávnění najdete v tématu [nastavení klientské aplikace GCM pro Android](https://developers.google.com/cloud-messaging/android/client#manifest) a [migrace klientské aplikace GCM pro Android tooFirebase Cloud Messaging](https://developers.google.com/cloud-messaging/android/android-migrate-fcm#remove_the_permissions_required_by_gcm).
   
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />

### <a name="adding-code"></a>Přidání kódu
1. V zobrazení projektu hello, rozbalte položku **aplikace** > **src** > **hlavní** > **java**. Klikněte pravým tlačítkem na váš balíček ve složce **java**, klikněte na tlačítko **Nový** a pak klikněte na tlačítko **třída jazyka Java**. Přidejte novou třídu s názvem `NotificationSettings`. 
   
    ![Android Studio – nová třída Java](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hub-android-new-class.png)
   
    Zajistěte, aby tooupdate hello tyto tři zástupné symboly v následujícím kódu pro hello hello `NotificationSettings` třídy:
   
   * **ID odesílatele**: hello Id odesílatele, které jste získali dříve v hello **Cloud Messaging** kartě nastavení projektu v hello [Firebase konzoly](https://firebase.google.com/console/).
   * **HubListenConnectionString**: hello **DefaultListenAccessSignature** připojovací řetězec pro vaše centrum. Tento připojovací řetězec můžete zkopírovat kliknutím **zásady přístupu** na hello **nastavení** rozbočovače na hello [portálu Azure].
   * **HubName**: použití hello název centra oznámení, který se zobrazí v centru okna hello hello [portálu Azure].
     
     Kód `NotificationSettings`:
     
       public class NotificationSettings {
     
           public static String SenderId = "<Your project number>";
           public static String HubName = "<Your HubName>";
           public static String HubListenConnectionString = "<Enter your DefaultListenSharedAccessSignature connection string>";
       }
2. Pomocí kroků hello výše, přidejte další novou třídu s názvem `MyInstanceIDService`. Toto bude naše implementace služby procesu naslouchání Instance ID.
   
    Hello kód pro tuto třídu bude volat naše `IntentService` příliš[tokenu obnovení hello FCM](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) hello pozadí.
   
        import android.content.Intent;
        import android.util.Log;
        import com.google.firebase.iid.FirebaseInstanceIdService;

        public class MyInstanceIDService extends FirebaseInstanceIdService {

            private static final String TAG = "MyInstanceIDService";

            @Override
            public void onTokenRefresh() {

                Log.d(TAG, "Refreshing GCM Registration Token");

                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        };


1. Přidejte jiný nový projekt tooyour třída s názvem `RegistrationIntentService`. To bude hello implementace pro naše `IntentService` která zpracuje [aktualizuje token FCM hello](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) a [registrace centra oznámení hello](notification-hubs-push-notification-registration-management.md).
   
    Použijte následující kód pro tuto třídu hello.
   
        import android.app.IntentService;
        import android.content.Intent;
        import android.content.SharedPreferences;
        import android.preference.PreferenceManager;
        import android.util.Log;        
        import com.google.firebase.iid.FirebaseInstanceId;
        import com.microsoft.windowsazure.messaging.NotificationHub;
   
        public class RegistrationIntentService extends IntentService {
   
            private static final String TAG = "RegIntentService";
   
            private NotificationHub hub;
   
            public RegistrationIntentService() {
                super(TAG);
            }
   
            @Override
            protected void onHandleIntent(Intent intent) {
   
                SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
                String resultString = null;
                String regID = null;
                String storedToken = null;
   
                try {
                    String FCM_token = FirebaseInstanceId.getInstance().getToken();
                    Log.d(TAG, "FCM Registration Token: " + FCM_token);
   
                    // Storing hello registration id that indicates whether hello generated token has been
                    // sent tooyour server. If it is not stored, send hello token tooyour server,
                    // otherwise your server should have already received hello token.
                    if (((regID=sharedPreferences.getString("registrationID", null)) == null)){
   
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "Attempting a new registration with NH using FCM token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
   
                        // If you want toouse tags...
                        // Refer too: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
   
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
   
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
   
                    // Check if hello token may have been compromised and needs refreshing.
                    else if ((storedToken=sharedPreferences.getString("FCMtoken", "")) != FCM_token) {
   
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "NH Registration refreshing with token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
   
                        // If you want toouse tags...
                        // Refer too: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
   
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
   
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
   
                    else {
                        resultString = "Previously Registered Successfully - RegId : " + regID;
                    }
                } catch (Exception e) {
                    Log.e(TAG, resultString="Failed toocomplete registration", e);
                    // If an exception happens while fetching hello new token or updating our registration data
                    // on a third-party server, this ensures that we'll attempt hello update at a later time.
                }
   
                // Notify UI that registration has completed.
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(resultString);
                }
            }
        }
2. Ve vaší `MainActivity` třídy, přidejte následující hello `import` příkazů výše hello třídy deklarace.
   
        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.content.Intent;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;
3. Přidejte následující soukromé členy v horní části hello třídy hello hello. Použijeme tyto [zkontrolovat hello dostupnost služeb Google Play dle doporučení Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).
   
        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private static final String TAG = "MainActivity";
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;
4. Ve vašem `MainActivity` třídy, přidejte následující metodu toohello dostupnost služeb Google Play hello. 
   
        /**
         * Check hello device toomake sure it has hello Google Play Services APK. If
         * it doesn't, display a dialog that allows users toodownload hello APK from
         * hello Google Play Store or enable it in hello device's system settings.
         */
        private boolean checkPlayServices() {
            GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
            int resultCode = apiAvailability.isGooglePlayServicesAvailable(this);
            if (resultCode != ConnectionResult.SUCCESS) {
                if (apiAvailability.isUserResolvableError(resultCode)) {
                    apiAvailability.getErrorDialog(this, resultCode, PLAY_SERVICES_RESOLUTION_REQUEST)
                            .show();
                } else {
                    Log.i(TAG, "This device is not supported by Google Play Services.");
                    ToastNotify("This device is not supported by Google Play Services.");
                    finish();
                }
                return false;
            }
            return true;
        }
5. Ve vašem `MainActivity` třídy, přidejte následující kód, který zkontrolujte služby Google Play před voláním hello vaše `IntentService` tooget FCM registrační token a registrace pomocí centra oznámení.
   
        public void registerWithNotificationHubs()
        {
            if (checkPlayServices()) {
                // Start IntentService tooregister this application with FCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }
6. V hello `OnCreate` metoda hello `MainActivity` třídy, přidejte následující kód toostart hello registraci při vytvoření aktivity hello.
   
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }
7. Přidejte tyto další metody toohello `MainActivity` tooverify aplikace stavu a stavu sestavy ve vaší aplikaci.
   
        @Override
        protected void onStart() {
            super.onStart();
            isVisible = true;
        }
   
        @Override
        protected void onPause() {
            super.onPause();
            isVisible = false;
        }
   
        @Override
        protected void onResume() {
            super.onResume();
            isVisible = true;
        }
   
        @Override
        protected void onStop() {
            super.onStop();
            isVisible = false;
        }
   
        public void ToastNotify(final String notificationMessage) {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(MainActivity.this, notificationMessage, Toast.LENGTH_LONG).show();
                    TextView helloText = (TextView) findViewById(R.id.text_hello);
                    helloText.setText(notificationMessage);
                }
            });
        }
8. Hello `ToastNotify` metoda používá hello *"Hello, World"* `TextView` řízení tooreport stavu a oznámení trvale v aplikaci hello. Do rozložení activity_main.xml přidejte následující id pro ovládací prvek hello.
   
       android:id="@+id/text_hello"
9. Další přidáme podtřídy pro naše příjemce, které jsme definovali v hello AndroidManifest.xml. Přidejte jiný nový projekt tooyour třída s názvem `MyHandler`.
10. Přidejte následující příkazy pro import hello horní části hello `MyHandler.java`:
    
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.media.RingtoneManager;
        import android.net.Uri;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;
11. Přidejte následující kód pro hello hello `MyHandler` třídy, takže je podtřídou třídy `com.microsoft.windowsazure.notifications.NotificationsHandler`.
    
    Tento kód přepíše hello `OnReceive` proto hello obslužná rutina nahlásila oznámení, které jsou přijaty. Hello obslužná rutina také odesílá správci oznámení Android hello nabízená oznámení toohello pomocí hello `sendNotification()` metoda. Hello `sendNotification()` metoda se má provést, když není hello aplikace spuštěna a není přijato oznámení.
    
        public class MyHandler extends NotificationsHandler {
            public static final int NOTIFICATION_ID = 1;
            private NotificationManager mNotificationManager;
            NotificationCompat.Builder builder;
            Context ctx;
    
            @Override
            public void onReceive(Context context, Bundle bundle) {
                ctx = context;
                String nhMessage = bundle.getString("message");
                sendNotification(nhMessage);
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(nhMessage);
                }
            }
    
            private void sendNotification(String msg) {
    
                Intent intent = new Intent(ctx, MainActivity.class);
                intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
    
                mNotificationManager = (NotificationManager)
                        ctx.getSystemService(Context.NOTIFICATION_SERVICE);
    
                PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                        intent, PendingIntent.FLAG_ONE_SHOT);
    
                Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
                NotificationCompat.Builder mBuilder =
                        new NotificationCompat.Builder(ctx)
                                .setSmallIcon(R.mipmap.ic_launcher)
                                .setContentTitle("Notification Hub Demo")
                                .setStyle(new NotificationCompat.BigTextStyle()
                                        .bigText(msg))
                                .setSound(defaultSoundUri)
                                .setContentText(msg);
    
                mBuilder.setContentIntent(contentIntent);
                mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
            }
        }
12. V Android Studio na řádku nabídek hello, klikněte na tlačítko **sestavení** > **znovu sestavit projekt** toomake se, že jsou ve vašem kódu nenachází žádné chyby.
13. Spuštění aplikace hello na vaše zařízení a ověřte, zda že zaregistruje úspěšně hello centra oznámení. 
    
    > [!NOTE]
    > Registrace může při spuštění počáteční hello neúspěšné, dokud nebude hello `onTokenRefresh()` je volána metoda instance Id služby. aktualizace Hello musí inicializovat úspěšnou registraci hello centra oznámení.
    > 
    > 

## <a name="sending-push-notifications"></a>Odeslání nabízených oznámení
Můžete otestovat přijímání nabízených oznámení ve vaší aplikaci jejich odesláním prostřednictvím hello [portálu Azure] -vyhledejte hello **Poradce při potížích s** kapitoly hello okno centra, jak je uvedeno níže.

![Azure Notification Hubs – testovací odeslání](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-test-send.png)

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-directly-from-hello-app"></a>(Volitelné) Odesílání nabízených oznámení přímo z aplikace hello
> [!IMPORTANT]
> Tady je příklad odesílání oznámení z klienta aplikace hello se poskytuje pro učení pouze pro účely. Vzhledem k tomu, že to bude vyžadovat hello `DefaultFullSharedAccessSignature` toobe v hello klientskou aplikaci, zpřístupňuje riziko toohello centra oznámení, že uživatel může získat oznámení toosend neoprávněného přístupu tooyour klientů.
> 
> 

Za normálních okolností byste odesílali oznámení pomocí serveru backend. V některých případech můžete chtít toobe možné toosend nabízených oznámení přímo z klientské aplikace hello. Tato část vysvětluje, jak toosend oznámení z klienta hello pomocí hello [API služby REST centra oznámení Azure](https://msdn.microsoft.com/library/azure/dn223264.aspx).

1. V zobrazení projektu Android Studio rozbalte možnost **App** > **src** > **main** > **res** > **layout**. Otevřete hello `activity_main.xml` rozložení souboru a klikněte na tlačítko hello **Text** kartě obsah textu hello tooupdate hello souboru. Aktualizujte jej s hello kódu níže, který přidává nové `Button` a `EditText` ovládací prvky pro zasílání oznámení centra oznámení toohello zprávy oznámení. Přidejte tento kód v dolní části hello, těsně před `</RelativeLayout>`.
   
        <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/send_button"
        android:id="@+id/sendbutton"
        android:layout_centerVertical="true"
        android:layout_centerHorizontal="true"
        android:onClick="sendNotificationButtonOnClick" />
   
        <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/editTextNotificationMessage"
        android:layout_above="@+id/sendbutton"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="42dp"
        android:hint="@string/notification_message_hint" />
2. V zobrazení projektu Android Studio rozbalte **App** > **src** > **main** > **res** > **values**. Otevřete hello `strings.xml` souboru a přidejte hello řetězcové hodnoty, které jsou odkazovány pomocí nového hello `Button` a `EditText` ovládací prvky. Přidejte tyto dolnímu hello hello souboru, těsně před `</resources>`.
   
        <string name="send_button">Send Notification</string>
        <string name="notification_message_hint">Enter notification message text</string>
3. Ve vaší `NotificationSetting.java` soubor, přidejte následující nastavení toohello hello `NotificationSettings` třídy.
   
    Aktualizace `HubFullAccess` s hello **DefaultFullSharedAccessSignature** připojovací řetězec pro vaše centrum. Tento připojovací řetězec lze kopírovat z hello [portálu Azure] kliknutím **zásady přístupu** na hello **nastavení** okna pro vaše Centrum oznámení.
   
        public static String HubFullAccess = "<Enter Your DefaultFullSharedAccessSignature Connection string>";
4. Ve vaší `MainActivity.java` soubor, přidejte následující hello `import` příkazy výše hello `MainActivity` třídy.
   
        import java.io.BufferedOutputStream;
        import java.io.BufferedReader;
        import java.io.InputStreamReader;
        import java.io.OutputStream;
        import java.net.HttpURLConnection;
        import java.net.URL;
        import java.net.URLEncoder;
        import javax.crypto.Mac;
        import javax.crypto.spec.SecretKeySpec;
        import android.util.Base64;
        import android.view.View;
        import android.widget.EditText;
5. Ve vaší `MainActivity.java` soubor, přidejte následující členy hello horní části hello hello `MainActivity` třídy.    
   
        private String HubEndpoint = null;
        private String HubSasKeyName = null;
        private String HubSasKeyValue = null;
6. Centrum POST požadavek toosend zprávy tooyour oznámení, musíte vytvořit tokenu tooauthenticate softwaru přístupový podpis (SaS). K tomu je potřeba analýza hello klíčová data z hello připojovací řetězec a pak vytvořit hello tokenu SaS, jak je uvedeno v hello [běžné koncepty](http://msdn.microsoft.com/library/azure/dn495627.aspx) odkazu k REST API. Hello následující kód představuje příklad implementace.
   
    V `MainActivity.java`, přidejte následující metodu toohello hello `MainActivity` třídy tooparse připojovací řetězec.
   
        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx
         * tooparse hello connection string so a SaS authentication token can be
         * constructed.
         *
         * @param connectionString This must be hello DefaultFullSharedAccess connection
         *                         string for this example.
         */
        private void ParseConnectionString(String connectionString)
        {
            String[] parts = connectionString.split(";");
            if (parts.length != 3)
                throw new RuntimeException("Error parsing connection string: "
                        + connectionString);
   
            for (int i = 0; i < parts.length; i++) {
                if (parts[i].startsWith("Endpoint")) {
                    this.HubEndpoint = "https" + parts[i].substring(11);
                } else if (parts[i].startsWith("SharedAccessKeyName")) {
                    this.HubSasKeyName = parts[i].substring(20);
                } else if (parts[i].startsWith("SharedAccessKey")) {
                    this.HubSasKeyValue = parts[i].substring(16);
                }
            }
        }
7. V `MainActivity.java`, přidejte následující metodu toohello hello `MainActivity` třída toocreate ověřovacího tokenu SaS.
   
        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx to
         * construct a SaS token from hello access key tooauthenticate a request.
         *
         * @param uri hello unencoded resource URI string for this operation. hello resource
         *            URI is hello full URI of hello Service Bus resource toowhich access is
         *            claimed. For example,
         *            "http://<namespace>.servicebus.windows.net/<hubName>"
         */
        private String generateSasToken(String uri) {
   
            String targetUri;
            String token = null;
            try {
                targetUri = URLEncoder
                        .encode(uri.toString().toLowerCase(), "UTF-8")
                        .toLowerCase();
   
                long expiresOnDate = System.currentTimeMillis();
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60 * 1000;
                long expires = expiresOnDate / 1000;
                String toSign = targetUri + "\n" + expires;
   
                // Get an hmac_sha1 key from hello raw key bytes
                byte[] keyBytes = HubSasKeyValue.getBytes("UTF-8");
                SecretKeySpec signingKey = new SecretKeySpec(keyBytes, "HmacSHA256");
   
                // Get an hmac_sha1 Mac instance and initialize with hello signing key
                Mac mac = Mac.getInstance("HmacSHA256");
                mac.init(signingKey);
   
                // Compute hello hmac on input data bytes
                byte[] rawHmac = mac.doFinal(toSign.getBytes("UTF-8"));
   
                // Using android.util.Base64 for Android Studio instead of
                // Apache commons codec
                String signature = URLEncoder.encode(
                        Base64.encodeToString(rawHmac, Base64.NO_WRAP).toString(), "UTF-8");
   
                // Construct authorization string
                token = "SharedAccessSignature sr=" + targetUri + "&sig="
                        + signature + "&se=" + expires + "&skn=" + HubSasKeyName;
            } catch (Exception e) {
                if (isVisible) {
                    ToastNotify("Exception Generating SaS : " + e.getMessage().toString());
                }
            }
   
            return token;
        }
8. V `MainActivity.java`, přidejte následující metodu toohello hello `MainActivity` třída toohandle hello **odeslat oznámení** tlačítko a odesílání nabízených oznámení hello hello zpráva toohello centra pomocí předdefinovaného REST API.
   
        /**
         * Send Notification button click handler. This method parses the
         * DefaultFullSharedAccess connection string and generates a SaS token. The
         * token is added toohello Authorization header on hello POST request toothe
         * notification hub. hello text in hello editTextNotificationMessage control
         * is added as hello JSON body for hello request tooadd a GCM message toohello hub.
         *
         * @param v
         */
        public void sendNotificationButtonOnClick(View v) {
            EditText notificationText = (EditText) findViewById(R.id.editTextNotificationMessage);
            final String json = "{\"data\":{\"message\":\"" + notificationText.getText().toString() + "\"}}";
   
            new Thread()
            {
                public void run()
                {
                    try
                    {
                        // Based on reference documentation...
                        // http://msdn.microsoft.com/library/azure/dn223273.aspx
                        ParseConnectionString(NotificationSettings.HubFullAccess);
                        URL url = new URL(HubEndpoint + NotificationSettings.HubName +
                                "/messages/?api-version=2015-01");
   
                        HttpURLConnection urlConnection = (HttpURLConnection)url.openConnection();
   
                        try {
                            // POST request
                            urlConnection.setDoOutput(true);
   
                            // Authenticate hello POST request with hello SaS token
                            urlConnection.setRequestProperty("Authorization", 
                                generateSasToken(url.toString()));
   
                            // Notification format should be GCM
                            urlConnection.setRequestProperty("ServiceBusNotification-Format", "gcm");
   
                            // Include any tags
                            // Example below targets 3 specific tags
                            // Refer too: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                            // urlConnection.setRequestProperty("ServiceBusNotification-Tags", 
                            //        "tag1 || tag2 || tag3");
   
                            // Send notification message
                            urlConnection.setFixedLengthStreamingMode(json.length());
                            OutputStream bodyStream = new BufferedOutputStream(urlConnection.getOutputStream());
                            bodyStream.write(json.getBytes());
                            bodyStream.close();
   
                            // Get reponse
                            urlConnection.connect();
                            int responseCode = urlConnection.getResponseCode();
                            if ((responseCode != 200) && (responseCode != 201)) {
                                BufferedReader br = new BufferedReader(new InputStreamReader((urlConnection.getErrorStream())));
                                String line;
                                StringBuilder builder = new StringBuilder("Send Notification returned " +
                                        responseCode + " : ")  ;
                                while ((line = br.readLine()) != null) {
                                    builder.append(line);
                                }
   
                                ToastNotify(builder.toString());
                            }
                        } finally {
                            urlConnection.disconnect();
                        }
                    }
                    catch(Exception e)
                    {
                        if (isVisible) {
                            ToastNotify("Exception Sending Notification : " + e.getMessage().toString());
                        }
                    }
                }
            }.start();
        }

## <a name="testing-your-app"></a>Testování vaší aplikace
#### <a name="push-notifications-in-hello-emulator"></a>Nabízená oznámení v emulátoru hello
Pokud chcete, aby tootest nabízená oznámení uvnitř emulátoru, ujistěte se, že bitová kopie emulátoru podporuje úroveň rozhraní Google API hello, kterou jste zvolili pro vaši aplikaci. Pokud bitová kopie nepodporuje nativní rozhraní Google API, budete mít s hello **služby\_není\_dostupné** výjimka.

Kromě toho toohello výše, zkontrolujte, že jste přidali vaší tooyour účet Google spuštěný emulátoru pod **nastavení** > **účty**. Jinak, může vaše pokusy o tooregister s GCM výsledkem hello **ověřování\_se nezdařilo** výjimka.

#### <a name="running-hello-application"></a>Spuštění aplikace hello
1. Spuštění aplikace hello a Všimněte si, že hello ID registrace hlášené pro úspěšnou registraci.
   
       ![Testing on Android - Channel registration](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-registered.png)
2. Zadejte toobe zpráv oznámení odeslaných tooall zařízení Android, která byla zaregistrovaná hello rozbočovače.
   
       ![Testing on Android - sending a message](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-set-message.png)
3. Stiskněte tlačítko **Odeslat oznámení**. Zobrazí všechna zařízení, které aplikace běžet hello `AlertDialog` instance s hello nabízená oznámení. Zařízení, které nemají hello aplikace spuštěná, ale byla dříve registrována pro nabízená oznámení se zobrazí oznámení v hello správci oznámení Android. Ta lze zobrazit potažením dolů z levého horního rohu hello.
   
       ![Testing on Android - notifications](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-received-message.png)

## <a name="next-steps"></a>Další kroky
Doporučujeme, abyste hello [použití centra oznámení toopush oznámení toousers] kurz jako další krok hello. Zobrazí se jak toosend oznámení z back-end ASP.NET pomocí značky tootarget konkrétním uživatelům.

Pokud chcete toosegment uživatele podle zájmových skupin, podívejte se na hello [toosend použití centra oznámení nejnovější zprávy přes] kurzu.

toolearn další obecné informace o centrech oznámení naleznete v našem [pokyny centra oznámení].

<!-- Images. -->



<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Azure Classic Portal]: https://manage.windowsazure.com/
[pokyny centra oznámení]: notification-hubs-push-notification-overview.md
[použití centra oznámení toopush oznámení toousers]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[toosend použití centra oznámení nejnovější zprávy přes]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[portálu Azure]: https://portal.azure.com
