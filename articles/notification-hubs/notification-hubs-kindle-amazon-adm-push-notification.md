---
title: "aaaGet začít s Azure Notification Hubs pro aplikace Kindle | Microsoft Docs"
description: "V tomto kurzu zjistíte, jak Azure Notification Hubs toosend toouse nabízená oznámení aplikace Kindle tooa."
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 346fc8e5-294b-4e4f-9f27-7a82d9626e93
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-kindle
ms.devlang: Java
ms.topic: hero-article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 7c28d64372cd2d90bab9cd9bf818d333f3478f7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-notification-hubs-for-kindle-apps"></a>Začínáme s použitím Notification Hubs pro aplikace Kindle
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Přehled
Tento kurz ukazuje, jak Azure Notification Hubs toosend toouse nabízená oznámení aplikace Kindle tooa.
Vytvoříte prázdnou aplikaci systému Kindle, která bude přijímat nabízená oznámení pomocí služby ADM (Amazon Device Messaging).

## <a name="prerequisites"></a>Požadavky
Tento kurz vyžaduje hello následující:

* Hello Android SDK (předpokládáme, že použijete Eclipse) získejte z hello <a href="http://go.microsoft.com/fwlink/?LinkId=389797">lokality Android</a>.
* Postupujte podle kroků hello v <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">nastavení vašeho vývojového prostředí</a> tooset vývojového prostředí pro Kindle.

## <a name="add-a-new-app-toohello-developer-portal"></a>Přidat nový portál pro vývojáře aplikací toohello
1. Nejprve vytvořte aplikaci v hello [portálu pro vývojáře Amazon].
   
    ![][0]
2. Kopírování hello **klíč aplikace**.
   
    ![][1]
3. Hello portálu klikněte hello název vaší aplikace a pak klikněte na hello **zasílání zpráv zařízení** kartě.
   
    ![][2]
4. Klikněte na tlačítko **Vytvoření nového profilu zabezpečení** a pak vytvořte nový profil zabezpečení (například **profil zabezpečení TestAdm**). Potom klikněte na **Uložit**.
   
    ![][3]
5. Klikněte na tlačítko **profily zabezpečení** profil tooview hello zabezpečení, kterou jste právě vytvořili. Kopírování hello **ID klienta** a **tajný klíč klienta** hodnoty pro pozdější použití.
   
    ![][4]

## <a name="create-an-api-key"></a>Vytvořte klíč rozhraní API
1. Otevřete příkazový řádek s oprávněními správce.
2. Přejděte toohello složky sady SDK pro Android.
3. Zadejte hello následující příkaz:
   
        keytool -list -v -alias androiddebugkey -keystore ./debug.keystore
   
    ![][5]
4. Pro hello **úložiště klíčů** heslo, typ **android**.
5. Kopírování hello **MD5** otisků prstů.
6. Zpět v portálu pro vývojáře hello na hello **zasílání zpráv** , klikněte na **Android/Kindle** a zadejte název hello hello balíčku pro vaši aplikaci (například **com.sample.notificationhubtest**) a hello **MD5** hodnotu a pak klikněte na **vygenerovat klíč rozhraní API**.

## <a name="add-credentials-toohello-hub"></a>Přidat přihlašovací údaje toohello rozbočovače
Hello portálu přidejte hello klienta tajný klíč a klient ID toohello **konfigurace** centra oznámení.

## <a name="set-up-your-application"></a>Nastavte aplikaci
> [!NOTE]
> Při vytváření aplikace použijte alespoň 17 úrovní rozhraní API.
> 
> 

Přidání projektu Eclipse tooyour knihovny ADM hello:

1. knihovny ADM hello tooobtain [stáhnout hello SDK]. Extrahujte soubor zip sady SDK hello.
2. V Eclipse klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **Vlastnosti**. Vyberte **cesta sestavení Java** na hello vlevo a pak vyberte hello ** knihovny ** karty v horní části hello. Klikněte na tlačítko **přidat externí Jar**a vyberte hello soubor `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` z hello adresáře, do kterého jste extrahovali Amazon SDK hello.
3. Stáhněte si hello SDK NotificationHubs pro Android (odkaz).
4. Rozbalte balíček hello a pak přetáhněte soubor hello `notification-hubs-sdk.jar` do hello `libs` složky v prostředí Eclipse.

Upravte manifest toosupport aplikace ADM:

1. Přidáte obor názvů Amazon hello v hello kořenového prvku manifestu:

        xmlns:amazon="http://schemas.amazon.com/apk/res/android"

1. Přidejte oprávnění jako první prvek hello v rámci prvku manifestu hello. SUBSTITUTE **[název vašeho balíčku]** s balíčkem hello používá toocreate vaší aplikace.
   
        <permission
         android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
         android:protectionLevel="signature" />
   
        <uses-permission android:name="android.permission.INTERNET"/>
   
        <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />
   
        <!-- This permission allows your app access tooreceive push notifications
        from ADM. -->
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />
   
        <!-- ADM uses WAKE_LOCK tookeep hello processor from sleeping when a message is received. -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />
2. Vložte následující prvek jako první podřízený prvek aplikace hello hello hello. Mějte na paměti, toosubstitute **[svůj název služby]** s hello názvem vaší obslužné rutiny zpráv ADM, které vytvoříte v další části hello (včetně hello balíček) a nahraďte **[název vašeho balíčku]** s hello Název balíčku, pomocí kterého jste vytvořili vaší aplikaci.
   
        <amazon:enable-feature
              android:name="com.amazon.device.messaging"
                     android:required="true"/>
        <service
            android:name="[YOUR SERVICE NAME]"
            android:exported="false" />
   
        <receiver
            android:name="[YOUR SERVICE NAME]$Receiver" />
   
            <!-- This permission ensures that only ADM can send your app registration broadcasts. -->
            android:permission="com.amazon.device.messaging.permission.SEND" >
   
            <!-- toointeract with ADM, your app must listen for hello following intents. -->
            <intent-filter>
          <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
          <action android:name="com.amazon.device.messaging.intent.RECEIVE" />
   
          <!-- Replace hello name in hello category tag with your app's package name. -->
          <category android:name="[YOUR PACKAGE NAME]" />
            </intent-filter>
        </receiver>

## <a name="create-your-adm-message-handler"></a>Vytvořte obslužnou rutinu zpráv ADM
1. Vytvořte novou třídu, která dědí z `com.amazon.device.messaging.ADMMessageHandlerBase` a pojmenujte ji `MyADMMessageHandler`, jak ukazuje následující obrázek hello:
   
    ![][6]
2. Přidejte následující hello `import` příkazy:
   
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.support.v4.app.NotificationCompat;
        import com.amazon.device.messaging.ADMMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub
3. Přidejte následující kód ve třídě hello, kterou jste vytvořili hello. Mějte na paměti, toosubstitute hello, rozbočovače název a připojovací řetězec (naslouchání):
   
        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
          private static NotificationHub hub;
        public static NotificationHub getNotificationHub(Context context) {
            Log.v("com.wa.hellokindlefire", "getNotificationHub");
            if (hub == null) {
                hub = new NotificationHub("[hub name]", "[listen connection string]", context);
            }
            return hub;
        }
   
        public MyADMMessageHandler() {
                super("MyADMMessageHandler");
            }
   
            public static class Receiver extends ADMMessageReceiver
            {
                public Receiver()
                {
                    super(MyADMMessageHandler.class);
                }
            }
   
            private void sendNotification(String msg) {
                Context ctx = getApplicationContext();
   
                mNotificationManager = (NotificationManager)
                    ctx.getSystemService(Context.NOTIFICATION_SERVICE);
   
            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                  new Intent(ctx, MainActivity.class), 0);
   
            NotificationCompat.Builder mBuilder =
                  new NotificationCompat.Builder(ctx)
                  .setSmallIcon(R.mipmap.ic_launcher)
                  .setContentTitle("Notification Hub Demo")
                  .setStyle(new NotificationCompat.BigTextStyle()
                         .bigText(msg))
                  .setContentText(msg);
   
             mBuilder.setContentIntent(contentIntent);
             mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
        }
4. Přidejte následující kód toohello hello `OnMessage()` metoda:
   
        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);
5. Přidejte následující kód toohello hello `OnRegistered` metoda:
   
            try {
        getNotificationHub(getApplicationContext()).register(registrationId);
            } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
            }
6. Přidejte následující kód toohello hello `OnUnregistered` metoda:
   
         try {
             getNotificationHub(getApplicationContext()).unregister();
         } catch (Exception e) {
             Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
         }
7. V hello `MainActivity` metody přidat hello následující příkaz importu:
   
        import com.amazon.device.messaging.ADM;
8. Přidejte následující kód na konci hello hello hello `OnCreate` metoda:
   
        final ADM adm = new ADM(this);
        if (adm.getRegistrationId() == null)
        {
           adm.startRegister();
        } else {
            new AsyncTask() {
                  @Override
                  protected Object doInBackground(Object... params) {
                     try {                         MyADMMessageHandler.getNotificationHub(getApplicationContext()).register(adm.getRegistrationId());
                     } catch (Exception e) {
                         Log.e("com.wa.hellokindlefire", "Failed registration with hub", e);
                         return e;
                     }
                     return null;
                 }
               }.execute(null, null, null);
        }

## <a name="add-your-api-key-tooyour-app"></a>Přidání klíče tooyour aplikace API
1. V nástroji Eclipse vytvořte nový soubor s názvem **api_key.txt** v hello majetku adresáře projektu.
2. Otevřete hello soubor a zkopírujte hello klíč rozhraní API generovaný na portálu pro vývojáře Amazon hello.

## <a name="run-hello-app"></a>Spuštění aplikace hello
1. Spusťte emulátor hello.
2. V emulátoru hello prstem z horní hello a klikněte na tlačítko **nastavení**a potom klikněte na **Můj účet** a zaregistrujte se pomocí platného účtu Amazon.
3. V nástroji Eclipse spusťte aplikaci hello.

> [!NOTE]
> Pokud dojde k potížím, zkontrolujte hello čas emulátoru hello (nebo zařízení). Hello časová hodnota musí být přesná. toochange hello čas emulátoru Kindle hello, můžete spustit následující příkaz z adresáře nástrojů platformy Android SDK hello:
> 
> 

        adb shell  date -s "yyyymmdd.hhmmss"

## <a name="send-a-message"></a>Odeslat zprávu
toosend zprávy pomocí .NET:

        static void Main(string[] args)
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

            hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
        }

![][7]

<!-- URLs. -->
[portálu pro vývojáře Amazon]: https://developer.amazon.com/home.html
[Stáhnout hello SDK]: https://developer.amazon.com/public/resources/development-tools/sdk

[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png
