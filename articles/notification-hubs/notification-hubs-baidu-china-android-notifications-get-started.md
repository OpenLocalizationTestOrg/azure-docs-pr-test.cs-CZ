---
title: "aaaGet začít s Azure Notification Hubs pomocí Baidu | Microsoft Docs"
description: "V tomto kurzu zjistíte, jak toouse Azure Notification Hubs toopush oznámení tooAndroid zařízení pomocí Baidu."
services: notification-hubs
documentationcenter: android
author: ysxu
manager: erikre
editor: 
ms.assetid: 23bde1ea-f978-43b2-9eeb-bfd7b9edc4c1
ms.service: notification-hubs
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: mobile-baidu
ms.workload: mobile
ms.date: 08/19/2016
ms.author: yuaxu
ms.openlocfilehash: 2767fdd3bb04674e7a531634237cc05cd8c21cb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-notification-hubs-using-baidu"></a>Začínáme s použitím Notification Hubs pomocí Baidu
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Přehled
Je cloudu baidu představuje čínskou cloudovou službu, které můžete použít toosend nabízená oznámení toomobile zařízení. Tato služba je užitečné v Číně, kde doručování nabízených oznámení, že tooAndroid je komplexní z důvodu hello přítomnosti různých obchodů s aplikacemi a nabízených služeb, kromě toohello dostupnosti zařízení Android, které nejsou obvykle připojené tooGCM (Google Cloud Messaging).

## <a name="prerequisites"></a>Požadavky
V tomto kurzu budete potřebovat:

* Sadu Android SDK (předpokládáme, že použijete Eclipse), kterou si můžete stáhnout z hello <a href="http://go.microsoft.com/fwlink/?LinkId=389797">lokality Android</a>
* [Mobile Services Android SDK]
* [Baidu Push Android SDK]

> [!NOTE]
> toocomplete tento kurz, musíte mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F).
> 
> 

## <a name="create-a-baidu-account"></a>Vytvořte účet Baidu
toouse Baidu, musíte mít účet Baidu. Pokud již účet máte, přihlaste se toohello [portál Baidu] a toohello další krok přeskočit. Jinak, najdete v části hello následující pokyny o tom, toocreate účet Baidu.  

1. Přejděte toohello [portál Baidu] a klikněte na tlačítko hello**登录**(**přihlášení**) odkaz. Klikněte na tlačítko**立即注册**toostart hello účet registraci.
   
   ![][1]
2. Zadejte hello požadované podrobnosti – telefon nebo e-mailovou adresu, heslo a ověřovací kód – a klikněte na tlačítko **registrace**.
   
   ![][2]
3. Vám bude zasláno e-mailovou toohello e-mailovou adresu, že jste zadali s tooactivate odkaz účet Baidu.
   
   ![][3]
4. Přihlaste tooyour e-mailový účet, otevřete aktivaci baidu hello a klikněte na tlačítko tooactivate odkaz hello aktivaci účtu Baidu.
   
   ![][4]

Po aktivaci účtu Baidu máte, přihlaste se toohello [portál Baidu].

## <a name="register-as-a-baidu-developer"></a>Zaregistrujte se jako vývojář Baidu
1. Jakmile jste byli přihlášeni toohello [portál Baidu], klikněte na tlačítko**更多 >>** (**Další**).
   
      ![][5]
2. Přejděte dolů v hello**站长与开发者服务 (správce webového serveru a služby pro vývojáře)** části a klikněte na tlačítko**百度开放云平台**(**Baidu otevřete Cloudová platforma**).
   
      ![][6]
3. Na další stránku hello, klikněte na tlačítko**开发者服务**(**služby pro vývojáře**) v pravém horním rohu hello.
   
      ![][7]
4. Na další stránku hello, klikněte na tlačítko**注册开发者**(**zaregistrován vývojáři**) z nabídky hello v pravém horním rohu hello.
   
      ![][8]
5. Zadejte své jméno, popis a číslo mobilního telefonu pro příjem ověřovací textové zprávy a pak klikněte na tlačítko **送验证码** (**Odeslat ověřovací kód**). Pro mezinárodní telefonní čísla budete potřebovat kód země hello tooenclose v závorkách. Například pro USA se bude jednat o číslo **(1) 1234567890**.
   
      ![][9]
6. Následně byste měli obdržet textovou zprávu s číslem ověření, jak je znázorněno v hello následující ukázka:
   
      ![][10]
7. Zadejte číslo ověření hello z uvítací zprávu v**验证码**(**potvrzovací kód**).
8. Nakonec dokončit registraci vývojáře hello přijetí smlouvy hello Baidu a kliknutím na**提交**(**odeslání**). Zobrazí se následující stránka při úspěšném dokončení registrace hello:
   
      ![][11]

## <a name="create-a-baidu-cloud-push-project"></a>Vytvořte projekt nabízených oznámení cloudu Baidu
Při vytváření projektu nabízených oznámení cloudu Baidu obdržíte své ID aplikace, klíč rozhraní API a tajný klíč.

1. Jakmile jste byli přihlášeni toohello [portál Baidu], klikněte na tlačítko**更多 >>** (**Další**).
   
      ![][5]
2. Přejděte dolů v hello**站长与开发者服务**(**správce webového serveru a služby pro vývojáře**) a klikněte na**百度开放云平台**(**Baidu otevřete Cloudová platforma**).
   
      ![][6]
3. Na další stránku hello, klikněte na tlačítko**开发者服务**(**služby pro vývojáře**) v pravém horním rohu hello.
   
      ![][7]
4. Na další stránku hello, klikněte na tlačítko**云推送**(**cloudu Push**) z hello**云服务**(**cloudové služby**) oddílu.
   
      ![][12]
5. Jakmile jste vývojář registrovaný, zobrazí**管理控制台**(**konzoly pro správu**) v horní nabídce hello. Klikněte na tlačítko **开发者服务管理** (**Správa služeb pro vývojáře**).
   
      ![][13]
6. Na další stránku hello, klikněte na tlačítko**创建工程**(**vytvoření projektu**).
   
      ![][14]
7. Zadejte název aplikace a klikněte na tlačítko **创建** (**Vytvořit**).
   
      ![][15]
8. Po úspěšném vytvoření projektu nabízených oznámení cloudu Baidu se zobrazí stránka s **AppID**, **klíčem rozhraní API** a **tajným klíčem**. Poznamenejte si klíč hello rozhraní API a tajný klíč, který použijeme později.
   
      ![][16]
9. Konfigurace projektu hello nabízená oznámení kliknutím**云推送**(**cloudu nabízené**) v levém podokně hello.
   
      ![][31]
10. Na další stránku hello, klikněte na tlačítko hello**推送设置**(**Push nastavení**) tlačítko.
    
    ![][32]  
11. Na stránce konfigurace hello, přidejte název hello balíčku, který budete používat v projektu Android v hello**应用包名**(**balíčku aplikace**) pole a pak klikněte na**保存设置**() **Uložit**).  
    
    ![][33]

Zobrazí hello**保存成功!** zpráva (**Úspěšně uloženo!**) .

## <a name="configure-your-notification-hub"></a>Konfigurace centra oznámení
1. Přihlaste se toohello [portálu Azure Classic]a potom klikněte na **+ nový** v hello dolní části obrazovky hello.
2. Klikněte na tlačítko **aplikační služby**, klikněte na tlačítko **Sběrnice**, klikněte na tlačítko **Notification Hubs** a pak klikněte na tlačítko **Rychle vytvořit**.
3. Zadejte název vaší **centra oznámení**, vyberte hello **oblast** a hello **Namespace** kde toto centrum oznámení se vytvoří a pak klikněte na tlačítko  **Vytvoření nového centra oznámení**.  
   
      ![][17]
4. Klikněte na tlačítko hello obor názvů, ve které jste vytvořili centrum oznámení a pak klikněte na **Notification Hubs** v horní části hello.
   
      ![][18]
5. Vyberte hello centra oznámení, který jste vytvořili a pak klikněte na tlačítko **konfigurace** hello hlavní nabídce.
   
      ![][19]
6. Projděte dolů toohello **nastavení oznámení baidu** a zadejte klíč hello rozhraní API a tajný klíč, který jste získali z konzoly Baidu hello dříve pro váš projekt nabízených oznámení cloudu Baidu. Klikněte na **Uložit**.
   
      ![][20]
7. Klikněte na tlačítko hello **řídicí panel** hello horní pro Centrum oznámení hello a pak klikněte **zobrazení připojovacího řetězce**.
   
      ![][21]
8. Poznamenejte si hello **DefaultListenSharedAccessSignature** a **DefaultFullSharedAccessSignature** z hello **informace o přístupovém připojení** okno.
   
    ![][22]

## <a name="connect-your-app-toohello-notification-hub"></a>Připojit vaše Centrum oznámení toohello aplikace
1. V Eclipse ADT vytvořte nový projekt Android (**Soubor** > **Nový** > **Projekt aplikace Android**).
   
    ![][23]
2. Zadejte **název aplikace** a ujistěte se, že hello **minimální požadované SDK** verze je nastaven příliš**rozhraní API 16: Android 4.1**.
   
    ![][24]
3. Klikněte na tlačítko **Další** a pokračovat, dokud hello následující hello průvodce **vytvořit aktivity** se zobrazí v okně. Ujistěte se, že **prázdné aktivity** je vybrané a nakonec vyberte možnost **Dokončit** toocreate nové aplikace pro Android.
   
    ![][25]
4. Ujistěte se, že hello **cíl sestavení projektu** nastavena správně.
   
    ![][26]
5. Stáhněte si soubor notification-hubs-0.4.jar hello ze hello **soubory** kartě hello [Notification-Hubs-Android-SDK na panelu koše](https://bintray.com/microsoftazuremobile/SDK/Notification-Hubs-Android-SDK/0.4). Přidejte soubor toohello hello **knihovny** složky projektu Eclipse a aktualizace hello *knihovny* složky.
6. Stáhněte a rozbalte hello [Baidu Push Android SDK], otevřete hello **knihovny** složku a potom kopie hello **pushservice-x.y.z** jar souborové služby a hello **armeabi**  &  **mips** složek v hello **knihovny** složky vaší aplikace Android.
7. Otevřete hello **AndroidManifest.xml** souboru systémem Android projekt a přidejte hello oprávnění, které jsou vyžadované Baidu SDK hello.
   
        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.WRITE_SETTINGS" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
        <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />
8. Přidat hello **android: name** vlastnost tooyour **aplikace** element v **AndroidManifest.xml**, ve které nahradíte *názevvašehoprojektu* (pro například **com.example.BaiduTest**). Ujistěte se, že tento název projektu odpovídá hello jeden, který jste nakonfigurovali v konzole Baidu hello.
   
        <application android:name="yourprojectname.DemoApplication"
9. Přidejte následující konfigurace v rámci elementu aplikace hello po hello hello **. MainActivity** prvku aktivity, nahraďte *názevvašehoprojektu* (například **com.example.BaiduTest**):
   
        <receiver android:name="yourprojectname.MyPushMessageReceiver">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
                <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
                <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
            </intent-filter>
        </receiver>
   
        <receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
            </intent-filter>
        </receiver>
   
        <receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.METHOD" />
                <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_REMOVED"/>
                <data android:scheme="package" />
            </intent-filter>
        </receiver>
   
        <service
            android:name="com.baidu.android.pushservice.PushService"
            android:exported="true"
            android:process=":bdservice_v1"  >
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
            </intent-filter>
        </service>
10. Přidejte novou třídu s názvem **ConfigurationSettings.java** toohello projektu.
    
     ![][28]
    
     ![][29]
11. Přidejte následující kód tooit hello:
    
        public class ConfigurationSettings {
                public static String API_KEY = "...";
                public static String NotificationHubName = "...";
                public static String NotificationHubConnectionString = "...";
            }
    
    Nastavte hodnotu hello **API_KEY** s co jste získali z projektu cloudu Baidu hello dříve, **NotificationHubName** s vaším jménem centra oznámení z hello portálu Azure Classic a  **NotificationHubConnectionString** pomocí DefaultListenSharedAccessSignature z portálu Azure Classic hello.
12. Přidejte novou třídu s názvem **DemoApplication.java**a přidejte následující kód tooit hello:
    
        import com.baidu.frontia.FrontiaApplication;
    
        public class DemoApplication extends FrontiaApplication {
            @Override
            public void onCreate() {
                super.onCreate();
            }
        }
13. Přidejte další novou třídu s názvem **MyPushMessageReceiver.java**a přidejte následující kód tooit hello. Je hello třídu, která zpracovává hello nabízená oznámení, které jsou přijaty ze serveru nabízených oznámení Baidu hello.
    
        import java.util.List;
        import android.content.Context;
        import android.os.AsyncTask;
        import android.util.Log;
        import com.baidu.frontia.api.FrontiaPushMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub;
    
        public class MyPushMessageReceiver extends FrontiaPushMessageReceiver {
            /** TAG tooLog */
            public static NotificationHub hub = null;
            public static String mChannelId, mUserId;
            public static final String TAG = MyPushMessageReceiver.class
                    .getSimpleName();
    
            @Override
            public void onBind(Context context, int errorCode, String appid,
                    String userId, String channelId, String requestId) {
                String responseString = "onBind errorCode=" + errorCode + " appid="
                        + appid + " userId=" + userId + " channelId=" + channelId
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
                mChannelId = channelId;
                mUserId = userId;
    
                try {
                    if (hub == null) {
                        hub = new NotificationHub(
                                ConfigurationSettings.NotificationHubName,
                                ConfigurationSettings.NotificationHubConnectionString,
                                context);
                        Log.i(TAG, "Notification hub initialized");
                    }
                } catch (Exception e) {
                   Log.e(TAG, e.getMessage());
                }
    
                registerWithNotificationHubs();
            }
    
            private void registerWithNotificationHubs() {
               new AsyncTask<Void, Void, Void>() {
                  @Override
                  protected Void doInBackground(Void... params) {
                     try {
                         hub.registerBaidu(mUserId, mChannelId);
                         Log.i(TAG, "Registered with Notification Hub - '"
                                 + ConfigurationSettings.NotificationHubName + "'"
                                 + " with UserId - '"
                                 + mUserId + "' and Channel Id - '"
                                 + mChannelId + "'");
                     } catch (Exception e) {
                         Log.e(TAG, e.getMessage());
                     }
                     return null;
                 }
               }.execute(null, null, null);
            }
    
            @Override
            public void onSetTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onSetTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }
    
            @Override
            public void onDelTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onDelTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }
    
            @Override
            public void onListTags(Context context, int errorCode, List<String> tags,
                    String requestId) {
                String responseString = "onListTags errorCode=" + errorCode + " tags="
                        + tags;
                Log.d(TAG, responseString);
            }
    
            @Override
            public void onUnbind(Context context, int errorCode, String requestId) {
                String responseString = "onUnbind errorCode=" + errorCode
                        + " requestId = " + requestId;
                Log.d(TAG, responseString);
            }
    
            @Override
            public void onNotificationClicked(Context context, String title,
                    String description, String customContentString) {
                String notifyString = "title=\"" + title + "\" description=\""
                        + description + "\" customContent=" + customContentString;
                Log.d(TAG, notifyString);
            }
    
            @Override
            public void onMessage(Context context, String message,
                    String customContentString) {
                String messageString = "message=\"" + message + "\" customContentString=" + customContentString;
                Log.d(TAG, messageString);
            }
        }
14. Otevřete **MainActivity.java**a přidejte následující toohello hello **onCreate** metoda:
    
            PushManager.startWork(getApplicationContext(),
                    PushConstants.LOGIN_TYPE_API_KEY, ConfigurationSettings.API_KEY);
15. Otevřete následující prohlášení importu v horní části hello hello:
    
            import com.baidu.android.pushservice.PushConstants;
            import com.baidu.android.pushservice.PushManager;

## <a name="send-notifications-tooyour-app"></a>Odeslat oznámení tooyour aplikace
Můžete rychle otestovat příjem oznámení ve vaší aplikaci odesláním oznámení hello [portál Azure](https://portal.azure.com/) pomocí hello **odeslat** tlačítko hello centra oznámení, jak je znázorněno v hello následující obrazovka:

![](./media/notification-hubs-baidu-get-started/notification-hub-test-send-baidu.png)

Nabízená oznámení se většinou posílají ve službě back-end, jako je služba Mobile Services, nebo v technologii ASP.NET pomocí kompatibilní knihovny. Pokud není žádná knihovna k dispozici pro back-end, můžete použít hello REST API přímo toosend zpráv s oznámením.

V tomto kurzu jsme dělat nic složitého a jednoduše předvedeme testování vaší klientské aplikace pomocí odesílání oznámení hello .NET SDK pro centra oznámení v konzolové aplikaci, místo back-end službu. Doporučujeme, abyste hello [použití centra oznámení toopush oznámení toousers](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) kurz jako hello další krok pro odesílání oznámení z backendu ASP.NET. Hello následující přístupy lze však použít pro zasílání oznámení:

* **Rozhraní REST**: oznámení můžete podporovat na jakékoli platformě back-end pomocí hello [rozhraní REST](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).
* **Microsoft Azure oznámení centra .NET SDK**: hello Správce balíčků Nuget pro Visual Studio, spusťte [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).
* **Node.js**: [jak toouse Notification Hubs z Node.js](notification-hubs-nodejs-push-notification-tutorial.md).
* **Mobile Apps**: pro příklad toosend oznámení z backendu Azure App Service Mobile Apps, které jsou integrovány v centrech oznámení najdete v části [přidat nabízená oznámení tooyour mobilní aplikace](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).
* **Java / PHP**: Příklad jak hello toosend oznámení pomocí rozhraní REST API, naleznete v části "jak toouse centra oznámení z Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

## <a name="optional-send-notifications-from-a-net-console-app"></a>(Volitelné) Odesílání oznámení z konzoly aplikace .NET.
V této části ukážeme odesílání oznámení pomocí konzolové aplikace .NET.

1. Vytvořte novou konzolovou aplikaci Visual C#:
   
    ![][30]
2. V okně konzoly Správce balíčků hello, nastavte hello **výchozí projekt** tooyour nové konzoly projekt aplikace a potom v okně konzoly hello, spusťte následující příkaz hello:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Tento pokyn přidá odkaz toohello SDK centra oznámení Azure pomocí hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">balíčku Microsoft.Azure.Notification Hubs NuGet</a>.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
3. Soubor otevřete hello **Program.cs** a přidejte následující hello pomocí příkazu:
   
        using Microsoft.Azure.NotificationHubs;
4. Ve vašem `Program` třídy, přidejte následující metodu hello a nahraďte *DefaultFullSharedAccessSignatureSASConnectionString* a *NotificationHubName* hello hodnotami, které máte.
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("DefaultFullSharedAccessSignatureSASConnectionString", "NotificationHubName");
            string message = "{\"title\":\"((Notification title))\",\"description\":\"Hello from Azure\"}";
            var result = await hub.SendBaiduNativeNotificationAsync(message);
        }
5. Přidejte následující řádky do hello vaše **hlavní** metoda:
   
         SendNotificationAsync();
         Console.ReadLine();

## <a name="test-your-app"></a>Testování aplikace
tootest této aplikace pomocí skutečného telefonu, jednoduše připojte hello phone tooyour počítači pomocí kabelu USB. Tato akce načte aplikaci do phone hello připojen.

Klikněte na této aplikaci pomocí hello emulátoru na horním nástrojů Eclipse hello tootest **spustit**a pak vyberte svou aplikaci: začne hello emulátoru, zatížení, a spustí hello aplikace.

aplikace Hello načte hello "userId" a "channelId" ze hello služby nabízených oznámení Baidu a zaregistruje hello centra oznámení.

toosend testovací oznámení, můžete použít hello ladění kartě hello portálu Azure Classic. Pokud jste vytvořili hello konzolové aplikace .NET pro Visual Studio, stačí stisknout klávesu F5 klíč hello v sadě Visual Studio toorun hello aplikaci. aplikace Hello odešle oznámení, že se zobrazí v horní oznamovací oblasti hello emulátoru nebo zařízení.

<!-- Images. -->
[1]: ./media/notification-hubs-baidu-get-started/BaiduRegistration.png
[2]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationInput.png
[3]: ./media/notification-hubs-baidu-get-started/BaiduConfirmation.png
[4]: ./media/notification-hubs-baidu-get-started/BaiduActivationEmail.png
[5]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationMore.png
[6]: ./media/notification-hubs-baidu-get-started/BaiduOpenCloudPlatform.png
[7]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperServices.png
[8]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperRegistration.png
[9]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationInput.png
[10]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationConfirmation.png
[11]: ./media/notification-hubs-baidu-get-started/BaiduDevConfirmationFinal.png
[12]: ./media/notification-hubs-baidu-get-started/BaiduCloudPush.png
[13]: ./media/notification-hubs-baidu-get-started/BaiduDevSvcMgmt.png
[14]: ./media/notification-hubs-baidu-get-started/BaiduCreateProject.png
[15]: ./media/notification-hubs-baidu-get-started/BaiduCreateProjectInput.png
[16]: ./media/notification-hubs-baidu-get-started/BaiduProjectKeys.png
[17]: ./media/notification-hubs-baidu-get-started/AzureNHCreation.png
[18]: ./media/notification-hubs-baidu-get-started/NotificationHubs.png
[19]: ./media/notification-hubs-baidu-get-started/NotificationHubsConfigure.png
[20]: ./media/notification-hubs-baidu-get-started/NotificationHubBaiduConfigure.png
[21]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionStringView.png
[22]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionString.png
[23]: ./media/notification-hubs-baidu-get-started/EclipseNewProject.png
[24]: ./media/notification-hubs-baidu-get-started/EclipseProjectCreation.png
[25]: ./media/notification-hubs-baidu-get-started/EclipseBlankActivity.png
[26]: ./media/notification-hubs-baidu-get-started/EclipseProjectBuildProperty.png
[27]: ./media/notification-hubs-baidu-get-started/EclipseBaiduReferences.png
[28]: ./media/notification-hubs-baidu-get-started/EclipseNewClass.png
[29]: ./media/notification-hubs-baidu-get-started/EclipseConfigSettingsClass.png
[30]: ./media/notification-hubs-baidu-get-started/ConsoleProject.png
[31]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig1.png
[32]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig2.png
[33]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig3.png

<!-- URLs. -->
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Baidu Push Android SDK]: http://developer.baidu.com/wiki/index.php?title=docs/cplat/push/sdk/clientsdk
[portálu Azure Classic]: https://manage.windowsazure.com/
[portál Baidu]: http://www.baidu.com/
