---
title: "aaaHow toouse centra oznámení s Pythonem"
description: "Zjistěte, jak toouse Azure Notification Hubs z Python back-end."
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 5640dd4a-a91e-4aa0-a833-93615bde49b4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: python
ms.devlang: php
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 21d5aaf7fc24c9936fac8e0a8de640c66c51ab0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-notification-hubs-from-python"></a>Jak toouse Notification Hubs z Pythonu
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Můžete ke všem funkcím centra oznámení z Java/PHP nebo Python nebo Ruby back-end pomocí rozhraní REST centra oznámení hello, jak je popsáno v tématu MSDN hello [rozhraní API REST centra oznámení](http://msdn.microsoft.com/library/dn223264.aspx).

> [!NOTE]
> To je ukázka odkazu implementace pro implementaci odešle oznámení hello v Pythonu a není hello oficiálně podporován Python SDK centra oznámení.
> 
> Tato ukázka je napsané v Pythonu 3.4.
> 
> 

V tomto tématu ukážeme postup:

* Vytvoření klienta REST centra oznámení funkcí v Pythonu.
* Odesílání oznámení pomocí hello Python rozhraní toohello rozhraní API REST centra oznámení. 
* Získá výpis hello požadavků a odpovědí HTTP REST pro ladění vzdělávací účely. 

Můžete postupovat podle hello [kurzu Začínáme Get](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) pro vaši mobilní platformu podle vlastní volby, implementace část hello back-end v Pythonu.

> [!NOTE]
> obor Hello ukázky hello je jenom omezené toosend oznámení a neprovádí žádné registrace správy.
> 
> 

## <a name="client-interface"></a>Rozhraní klienta
rozhraní Hello hlavní klienta může poskytnout hello stejných metod, které jsou k dispozici v hello [.NET SDK centra oznámení](http://msdn.microsoft.com/library/jj933431.aspx). To vám umožní toodirectly převede všechny hello kurzy a ukázky aktuálně k dispozici na tomto webu a přispěli hello komunity na hello Internetu.

Můžete najít všechny hello kód je k dispozici v hello [Python REST obálku ukázka].

Například toocreate klienta:

    isDebug = True
    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

toosend Windows připít oznámení:

    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

## <a name="implementation"></a>Implementace
Pokud jste ještě není, postupujte podle našich [kurzu Začínáme Get] až toohello poslední část, kde máte tooimplement hello back-end.

Všechny hello podrobnosti tooimplement úplné obálku REST naleznete na [MSDN](http://msdn.microsoft.com/library/dn530746.aspx). V této části jsme se popisují implementaci Python hello hello hlavní kroky požadované tooaccess koncové body REST centra oznámení a odesílání oznámení

1. Analyzovat hello připojovací řetězec
2. Vygenerování tokenu autorizace hello
3. Odeslání oznámení pomocí rozhraní HTTP REST API

### <a name="parse-hello-connection-string"></a>Analyzovat hello připojovací řetězec
Tady je hlavní třídy hello implementace hello klienta, jejichž konstruktor analyzuje hello připojovací řetězec:

    class NotificationHub:
        API_VERSION = "?api-version=2013-10"
        DEBUG_SEND = "&test"

        def __init__(self, connection_string=None, hub_name=None, debug=0):
            self.HubName = hub_name
            self.Debug = debug

            # Parse connection string
            parts = connection_string.split(';')
            if len(parts) != 3:
                raise Exception("Invalid ConnectionString.")

            for part in parts:
                if part.startswith('Endpoint'):
                    self.Endpoint = 'https' + part[11:]
                if part.startswith('SharedAccessKeyName'):
                    self.SasKeyName = part[20:]
                if part.startswith('SharedAccessKey'):
                    self.SasKeyValue = part[16:]


### <a name="create-security-token"></a>Vytvoření tokenu zabezpečení
Podrobnosti o Hello hello vytvoření tokenu zabezpečení jsou k dispozici [zde](http://msdn.microsoft.com/library/dn495627.aspx).
Hello následující metody mají toobe přidat toohello **NotificationHub** třída toocreate hello token podle hello identifikátoru URI aktuální žádosti hello a přihlašovací údaje hello extrahoval z hello připojovací řetězec.

    @staticmethod
    def get_expiry():
        # By default returns an expiration of 5 minutes (=300 seconds) from now
        return int(round(time.time() + 300))

    @staticmethod
    def encode_base64(data):
        return base64.b64encode(data)

    def sign_string(self, to_sign):
        key = self.SasKeyValue.encode('utf-8')
        to_sign = to_sign.encode('utf-8')
        signed_hmac_sha256 = hmac.HMAC(key, to_sign, hashlib.sha256)
        digest = signed_hmac_sha256.digest()
        encoded_digest = self.encode_base64(digest)
        return encoded_digest

    def generate_sas_token(self):
        target_uri = self.Endpoint + self.HubName
        my_uri = urllib.parse.quote(target_uri, '').lower()
        expiry = str(self.get_expiry())
        to_sign = my_uri + '\n' + expiry
        signature = urllib.parse.quote(self.sign_string(to_sign))
        auth_format = 'SharedAccessSignature sig={0}&se={1}&skn={2}&sr={3}'
        sas_token = auth_format.format(signature, expiry, self.SasKeyName, my_uri)
        return sas_token

### <a name="send-a-notification-using-http-rest-api"></a>Odeslání oznámení pomocí rozhraní HTTP REST API
První, umožňují použít definice třídy představující oznámení.

    class Notification:
        def __init__(self, notification_format=None, payload=None, debug=0):
            valid_formats = ['template', 'apple', 'gcm', 'windows', 'windowsphone', "adm", "baidu"]
            if not any(x in notification_format for x in valid_formats):
                raise Exception(
                    "Invalid Notification format. " +
                    "Must be one of hello following - 'template', 'apple', 'gcm', 'windows', 'windowsphone', 'adm', 'baidu'")

            self.format = notification_format
            self.payload = payload

            # array with keynames for headers
            # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
            # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry
            # in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
            self.headers = None

Tato třída je kontejner pro nativní oznámení textu nebo sadu vlastností v případě šablony oznámení, sadu hlaviček, který obsahuje formátu (nativní platforma nebo šablony) a vlastnosti specifické pro platformu (např. vlastnost Apple vypršení platnosti a hlavičky WNS).

Podrobnosti najdete toohello [dokumentaci rozhraní API REST centra oznámení](http://msdn.microsoft.com/library/dn495827.aspx) a hello formáty konkrétní oznámení platformy pro všechny hello možnosti, které jsou k dispozici.

Teď s touto třídou jsme může zapisovat hello odesílání oznámení metody uvnitř hello **NotificationHub** třídy.

    def make_http_request(self, url, payload, headers):
        parsed_url = urllib.parse.urlparse(url)
        connection = http.client.HTTPSConnection(parsed_url.hostname, parsed_url.port)

        if self.Debug > 0:
            connection.set_debuglevel(self.Debug)
            # adding this querystring parameter gets detailed information about hello PNS send notification outcome
            url += self.DEBUG_SEND
            print("--- REQUEST ---")
            print("URI: " + url)
            print("Headers: " + json.dumps(headers, sort_keys=True, indent=4, separators=(' ', ': ')))
            print("--- END REQUEST ---\n")

        connection.request('POST', url, payload, headers)
        response = connection.getresponse()

        if self.Debug > 0:
            # print out detailed response information for debugging purpose
            print("\n\n--- RESPONSE ---")
            print(str(response.status) + " " + response.reason)
            print(response.msg)
            print(response.read())
            print("--- END RESPONSE ---")

        elif response.status != 201:
            # Successful outcome of send message is HTTP 201 - Created
            raise Exception(
                "Error sending notification. Received HTTP code " + str(response.status) + " " + response.reason)

        connection.close()

    def send_notification(self, notification, tag_or_tag_expression=None):
        url = self.Endpoint + self.HubName + '/messages' + self.API_VERSION

        json_platforms = ['template', 'apple', 'gcm', 'adm', 'baidu']

        if any(x in notification.format for x in json_platforms):
            content_type = "application/json"
            payload_to_send = json.dumps(notification.payload)
        else:
            content_type = "application/xml"
            payload_to_send = notification.payload

        headers = {
            'Content-type': content_type,
            'Authorization': self.generate_sas_token(),
            'ServiceBusNotification-Format': notification.format
        }

        if isinstance(tag_or_tag_expression, set):
            tag_list = ' || '.join(tag_or_tag_expression)
        else:
            tag_list = tag_or_tag_expression

        # add hello tags/tag expressions toohello headers collection
        if tag_list != "":
            headers.update({'ServiceBusNotification-Tags': tag_list})

        # add any custom headers toohello headers collection that hello user may have added
        if notification.headers is not None:
            headers.update(notification.headers)

        self.make_http_request(url, payload_to_send, headers)

    def send_apple_notification(self, payload, tags=""):
        nh = Notification("apple", payload)
        self.send_notification(nh, tags)

    def send_gcm_notification(self, payload, tags=""):
        nh = Notification("gcm", payload)
        self.send_notification(nh, tags)

    def send_adm_notification(self, payload, tags=""):
        nh = Notification("adm", payload)
        self.send_notification(nh, tags)

    def send_baidu_notification(self, payload, tags=""):
        nh = Notification("baidu", payload)
        self.send_notification(nh, tags)

    def send_mpns_notification(self, payload, tags=""):
        nh = Notification("windowsphone", payload)

        if "<wp:Toast>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'toast', 'X-NotificationClass': '2'}
        elif "<wp:Tile>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'tile', 'X-NotificationClass': '1'}

        self.send_notification(nh, tags)

    def send_windows_notification(self, payload, tags=""):
        nh = Notification("windows", payload)

        if "<toast>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/toast'}
        elif "<tile>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/tile'}
        elif "<badge>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/badge'}

        self.send_notification(nh, tags)

    def send_template_notification(self, properties, tags=""):
        nh = Notification("template", properties)
        self.send_notification(nh, tags)

Hello výše metody odeslat HTTP POST požadavek toohello /messages koncový bod centra oznámení, s správné textu hello a hlavičky toosend hello oznámení.

### <a name="using-debug-property-tooenable-detailed-logging"></a>Pomocí ladění vlastnost tooenable podrobné protokolování
Povolení ladění vlastnost při inicializaci hello Centrum oznámení se zapsat podrobné protokolování informací o hello HTTP, že požadavek a odpověď výpisu, jakož i podrobné zprávy oznámení odeslat výsledek. Nedávno jsme přidali tato vlastnost s názvem [TestSend centra oznámení vlastnost](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) která vrací podrobné informace o hello výsledek odeslání oznámení. toouse ho - inicializovat pomocí hello následující:

    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

žádost odeslat oznámení centra Hello adresy URL protokolu HTTP získá spolu s "test" querystring v důsledku. 

## <a name="complete-tutorial"></a>Dokončení hello kurzu
Nyní můžete provést kurzu Začínáme hello zasláním oznámení hello z Python back-end.

Inicializace vašeho centra oznámení klienta (nahraďte hello připojovací řetězec a názvu centra podle pokynů v hello [kurzu Začínáme Get]):

    hub = NotificationHub("myConnectionString", "myNotificationHubName")

Pak přidejte kód odesílání hello v závislosti na svou cílovou platformu mobilních. Tato ukázka přidá také vyšší úrovně metody tooenable odesílání oznámení založené na platformě hello například send_windows_notification pro systém windows. send_apple_notification (pro apple) atd. 

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows Store a Windows Phone 8.1 (bez Silverlight)
    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 a 8.1 Silverlight
    hub.send_mpns_notification(toast)

### <a name="ios"></a>iOS
    alert_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_apple_notification(alert_payload)

### <a name="android"></a>Android
    gcm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_gcm_notification(gcm_payload)

### <a name="kindle-fire"></a>Kindle Fire
    adm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_adm_notification(adm_payload)

### <a name="baidu"></a>Baidu
    baidu_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_baidu_notification(baidu_payload)

Spuštěním kódu Python by měl vytvořit oznámení, které jsou na cílovém zařízení.

## <a name="examples"></a>Příklady:
### <a name="enabling-debug-property"></a>Povolení ladění vlastnost
Když povolíte příznak ladění při inicializaci hello NotificationHub, zobrazí se podrobné výpisu žádosti a odpovědi protokolu HTTP, jakož i NotificationOutcome jako hello následující, kde pochopit, jaké hlavičky protokolu HTTP se předávají v žádosti o hello a jaké HTTP byla přijata odpověď z hello Centrum oznámení:![][1]

Uvidíte, například podrobný výsledek centra oznámení 

* Když hello je úspěšně odeslána zpráva toohello službu nabízených oznámení. 
  
        <Outcome>hello Notification was successfully sent toohello Push Notification System</Outcome>
* Pokud nebyly nalezeny žádné cíle pro všechny nabízené oznámení pak budete pravděpodobně toosee hello následující hello odpovědi (což znamená, že neexistují žádné registrace pravděpodobně nalézt toodeliver hello oznámení, protože registrace hello měl některé Neshoda značek)
  
        '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'

### <a name="broadcast-toast-notification-toowindows"></a>Vysílání tooWindows oznámení informační zprávy
Všimněte si hello hlavičky, které získat odeslaná při odesílání oznámení klienta tooWindows informační zprávy všesměrového vysílání. 

    hub.send_windows_notification(wns_payload)

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a>Odeslat oznámení o určení značky (nebo výraz označení)
Hlavičky protokolu HTTP značky hello oznámení, který získá přidat toohello HTTP požadavku (v příkladu hello níže jsme odesílání hello oznámení pouze tooregistrations s odebranou datovou částí "sports")

    hub.send_windows_notification(wns_payload, "sports")

![][3]

### <a name="send-notification-specifying-multiple-tags"></a>Odeslat oznámení o určení více značek
Všimněte si, jak hlavičky protokolu HTTP značky hello změní, když jsou odesílány více značek. 

    tags = {'sports', 'politics'}
    hub.send_windows_notification(wns_payload, tags)

![][4]

### <a name="templated-notification"></a>Použitím šablon oznámení
Všimněte si, že hello změny záhlaví HTTP formátu a datovou část textu hello je odeslána jako část obsahu žádosti hello HTTP:

**Na straně klienta - registrované šablony**

        var template =
                        @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";

**Na straně serveru – odesílání hello datové části**

        template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
        hub.send_template_notification(template_payload)

![][5]

## <a name="next-steps"></a>Další kroky
V tomto tématu jsme vám ukázal, jak toocreate jednoduché Python REST klienta pro centra oznámení. Odsud můžete:

* Stáhnout hello úplné [Python REST obálku ukázka], která obsahuje všechny výše uvedený kód hello.
* Pokračujte ve čtení o centrech oznámení označování funkce v hello [novinkách kurzu]
* Pokračujte ve čtení o funkce šablon centra oznámení v hello [kurzu lokalizace zprávy]

<!-- URLs -->
[Python REST obálku ukázka]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python
[kurzu Začínáme Get]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[novinkách kurzu]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/
[kurzu lokalizace zprávy]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/

<!-- Images. -->
[1]: ./media/notification-hubs-python-backend-how-to/DetailedLoggingInfo.png
[2]: ./media/notification-hubs-python-backend-how-to/BroadcastScenario.png
[3]: ./media/notification-hubs-python-backend-how-to/SendWithOneTag.png
[4]: ./media/notification-hubs-python-backend-how-to/SendWithMultipleTags.png
[5]: ./media/notification-hubs-python-backend-how-to/TemplatedNotification.png

