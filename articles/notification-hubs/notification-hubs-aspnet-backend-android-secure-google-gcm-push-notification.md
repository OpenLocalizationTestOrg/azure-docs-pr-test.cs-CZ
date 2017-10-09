---
title: "aaaSending zabezpečení nabízená oznámení pomocí Azure Notification Hubs"
description: "Zjistěte, jak toosend zabezpečené nabízená oznámení tooan aplikace pro Android z Azure. Ukázky kódu jsou vytvořeny v jazyce Java a C#."
documentationcenter: android
keywords: "nabízená oznámení, nabízená oznámení, nabízené zprávy, android nabízená oznámení"
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: daf3de1c-f6a9-43c4-8165-a76bfaa70893
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: d07943c4691ed07acb987086228ef565e6281d57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sending-secure-push-notifications-with-azure-notification-hubs"></a>Odesílání zabezpečené nabízených oznámení pomocí Azure Notification Hubs
> [!div class="op_single_selector"]
> * [Univerzální pro Windows](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a>Přehled
> [!IMPORTANT]
> toocomplete tento kurz, musíte mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).
> 
> 

Podpora nabízená oznámení v Microsoft Azure umožňuje tooaccess infrastruktury zpráva snadno použitelnou, multiplatformní a upraveným nabízená, což výrazně zjednodušuje hello implementace nabízených oznámení spotřebních a podnikových aplikací pro mobilní platformy.

Z důvodu omezení tooregulatory nebo zabezpečení někdy aplikace může být vhodné tooinclude něco v hello oznámení, kterou nelze přenést prostřednictvím infrastrukturu pro hello standardní nabízená oznámení. Tento kurz popisuje, jak tooachieve hello stejné prostředí posíláním důvěrných informací o prostřednictvím zabezpečeného a ověřené připojení mezi hello klienta zařízení se systémem Android a back-end aplikace hello.

Na vysoké úrovni tok hello vypadá takto:

1. back-end Hello aplikace:
   * Zabezpečení datové úložiště v databázi back-end.
   * Odešle ID hello tato oznámení toohello zařízení se systémem Android (zabezpečené nebudou odeslány žádné informace).
2. aplikace Hello na hello zařízení při přijetí oznámení hello:
   * zařízení se systémem Android Hello kontaktuje hello back-end žádajícího hello zabezpečené datové části.
   * Hello aplikace můžete zobrazit datové části hello jako upozornění na hello zařízení.

Je důležité, že toonote, v předchozím toku hello (a v tomto kurzu) předpokládáme, že hello zařízení ukládá ověřovací token do místního úložiště, po přihlášení uživatele hello. Zaručí se tím úplně jednoduché prostředí, protože hello zařízení můžete načíst pomocí tohoto tokenu zabezpečení datové hello oznámení. Pokud vaše aplikace nejsou uložené ověřovací tokeny na hello zařízení nebo pokud tyto tokeny můžete vypršela platnost, by měla aplikace hello zařízení při přijetí nabízeného oznámení hello zobrazit obecné oznámení výzvy hello uživatele toolaunch hello aplikace. aplikace Hello pak ověřuje uživatele hello a ukazuje datová část oznámení hello.

Tento kurz ukazuje, jak toosend zabezpečené nabízená oznámení. Vychází hello [upozornění uživatelů](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) kurzu, takže byste měli dokončit hello kroky v tomto kurzu nejprve Pokud jste tak ještě neučinili.

> [!NOTE]
> V tomto kurzu se předpokládá, že jste vytvořili a nakonfigurovali vaše Centrum oznámení, jak je popsáno v [Začínáme s Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-hello-android-project"></a>Upravit hello projekt pro Android
Teď, když upravit vaše aplikace právě hello back-end toosend *id* nabízená oznámení, máte toochange vaší aplikace pro Android toohandle oznámení a zpětných volání váš back-end tooretrieve hello zabezpečit zprávy toobe zobrazí.
tooachieve tohoto cíle, máte jistotu, že zná svoji aplikaci pro Android toomake jak tooauthenticate samotné vaší back-end, pokud obdrží hello nabízená oznámení.

Nyní jsme upraví hello *přihlášení* tok v pořadí toosave hello ověřování hodnota v hlavičce hello sdílet předvolby vaší aplikace. Podobá mechanismy lze použít toostore žádné ověřovací token (např. tokenů OAuth), který hello aplikace bude mít toouse bez nutnosti přihlašovací údaje uživatele.

1. V projektu aplikace pro Android, přidejte následující konstanty hello horní části hello hello **MainActivity** třídy:
   
        public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
        public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";
2. Stále v hello **MainActivity** třídy, aktualizaci hello `getAuthorizationHeader()` hello toocontain metoda následující kód:
   
        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);
   
            SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();
   
            return basicAuthHeader;
        }
3. Přidejte následující hello `import` příkazy hello horní části hello **MainActivity** souboru:
   
        import android.content.SharedPreferences;

Nyní změníme hello obslužná rutina, která je volána, když je obdržena hello oznámení.

1. V hello **MyHandler** třída změnit hello `OnReceive()` toocontain metoda:
   
        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String secureMessageId = bundle.getString("secureId");
            retrieveNotification(secureMessageId);
        }
2. Pak přidejte hello `retrieveNotification()` metoda, nahraďte zástupný symbol hello `{back-end endpoint}` s koncovým bodem back-end hello získali při nasazování back-end:
   
        private void retrieveNotification(final String secureMessageId) {
            SharedPreferences sp = ctx.getSharedPreferences(MainActivity.NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            final String authorizationHeader = sp.getString(MainActivity.AUTHORIZATION_HEADER_PROPERTY, null);
   
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        HttpUriRequest request = new HttpGet("{back-end endpoint}/api/notifications/"+secureMessageId);
                        request.addHeader("Authorization", "Basic "+authorizationHeader);
                        HttpResponse response = new DefaultHttpClient().execute(request);
                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            Log.e("MainActivity", "Error retrieving secure notification" + response.getStatusLine().getStatusCode());
                            throw new RuntimeException("Error retrieving secure notification");
                        }
                        String secureNotificationJSON = EntityUtils.toString(response.getEntity());
                        JSONObject secureNotification = new JSONObject(secureNotificationJSON);
                        sendNotification(secureNotification.getString("Payload"));
                    } catch (Exception e) {
                        Log.e("MainActivity", "Failed tooretrieve secure notification - " + e.getMessage());
                        return e;
                    }
                    return null;
                }
            }.execute(null, null, null);
        }

Tato metoda volá hello oznámení tooretrieve back-end aplikace obsahu pomocí hello přihlašovací údaje uložené v hello sdílet předvolby a zobrazí jako normální oznámení. Hello oznámení vypadá uživatele aplikace toohello úplně stejně jako ostatní nabízených oznámení.

Všimněte si, že je vhodnější toohandle hello případech chybějící vlastnost hlavičky ověřování nebo odmítání podle hello back-end. Hello konkrétní zpracování těchto případech závisí hlavně na cílové činnost koncového uživatele. Jednou z možností je toodisplay oznámení s obecné výzvu hello uživatele tooauthenticate tooretrieve hello skutečné oznámení.

## <a name="run-hello-application"></a>Spustit hello aplikace
toorun hello aplikace, hello následující:

1. Zajistěte, aby **AppBackend** je nasazené v Azure. Pokud používáte Visual Studio, spusťte hello **AppBackend** aplikace webového rozhraní API. Zobrazí se webová stránka ASP.NET.
2. V prostředí Eclipse hello aplikace spusťte na fyzických Android zařízení nebo hello emulátor.
3. V hello aplikace pro Android uživatelského rozhraní, zadejte uživatelské jméno a heslo. Mohou to být libovolný řetězec, ale musí být hello stejnou hodnotu.
4. V hello aplikace pro Android uživatelského rozhraní, klikněte na **přihlásit**. Pak klikněte na tlačítko **odeslat nabízené**.

