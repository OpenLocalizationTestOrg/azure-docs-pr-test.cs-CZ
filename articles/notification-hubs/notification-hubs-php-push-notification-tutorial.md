---
title: "aaaHow toouse centra oznámení s PHP"
description: "Zjistěte, jak toouse Azure Notification Hubs z PHP back-end."
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 0156f994-96d0-4878-b07b-49b7be4fd856
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: php
ms.devlang: php
ms.topic: article
ms.date: 06/07/2016
ms.author: yuaxu
ms.openlocfilehash: 6cd426286a684006a07867fcf44a8ff71be7efa8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-notification-hubs-from-php"></a>Jak toouse Notification Hubs z PHP
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Můžete ke všem funkcím centra oznámení z Java/PHP nebo Ruby back-end pomocí rozhraní REST centra oznámení hello, jak je popsáno v tématu MSDN hello [rozhraní API REST centra oznámení](http://msdn.microsoft.com/library/dn223264.aspx).

V tomto tématu ukážeme postup:

* Vytvoření klienta REST pro funkce Notification Hubs v jazyce PHP;
* Postupujte podle hello [kurzu Začínáme Get](notification-hubs-ios-apple-push-notification-apns-get-started.md) pro vaši mobilní platformu podle vlastní volby, implementace část hello back-end v jazyce PHP.

## <a name="client-interface"></a>Rozhraní klienta
rozhraní Hello hlavní klienta může poskytnout hello stejných metod, které jsou k dispozici v hello [.NET SDK centra oznámení](http://msdn.microsoft.com/library/jj933431.aspx), bude možné toodirectly převede všechny hello kurzy a ukázky aktuálně k dispozici na tomto webu a přispěli hello komunity na hello Internetu.

Můžete najít všechny hello kód je k dispozici v hello [PHP REST obálku ukázka].

Například toocreate klienta:

    $hub = new NotificationHub("connection string", "hubname");    

toosend nativní oznámení iOS:

    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);

## <a name="implementation"></a>Implementace
Pokud jste ještě není, postupujte podle našich [kurzu Začínáme Get] až toohello poslední část, kde máte tooimplement hello back-end.
Navíc pokud chcete, můžete použít hello kód z hello [PHP REST obálku ukázka] , přejděte přímo toohello [hello dokončení kurzu](#complete-tutorial) části.

Všechny hello podrobnosti tooimplement úplné obálku REST naleznete na [MSDN](http://msdn.microsoft.com/library/dn530746.aspx). V této části jsme se popisují implementaci PHP hello hello hlavní kroky požadované tooaccess koncové body REST centra oznámení:

1. Analyzovat hello připojovací řetězec
2. Vygenerování tokenu autorizace hello
3. Provádění volání hello HTTP

### <a name="parse-hello-connection-string"></a>Analyzovat hello připojovací řetězec
Tady je hello hlavní třídy implementující hello klienta, jejichž konstruktor, který analyzuje hello připojovací řetězec:

    class NotificationHub {
        const API_VERSION = "?api-version=2013-10";

        private $endpoint;
        private $hubPath;
        private $sasKeyName;
        private $sasKeyValue;

        function __construct($connectionString, $hubPath) {
            $this->hubPath = $hubPath;

            $this->parseConnectionString($connectionString);
        }

        private function parseConnectionString($connectionString) {
            $parts = explode(";", $connectionString);
            if (sizeof($parts) != 3) {
                throw new Exception("Error parsing connection string: " . $connectionString);
            }

            foreach ($parts as $part) {
                if (strpos($part, "Endpoint") === 0) {
                    $this->endpoint = "https" . substr($part, 11);
                } else if (strpos($part, "SharedAccessKeyName") === 0) {
                    $this->sasKeyName = substr($part, 20);
                } else if (strpos($part, "SharedAccessKey") === 0) {
                    $this->sasKeyValue = substr($part, 16);
                }
            }
        }
    }


### <a name="create-security-token"></a>Vytvoření tokenu zabezpečení
Podrobnosti o Hello hello vytvoření tokenu zabezpečení jsou k dispozici [zde](http://msdn.microsoft.com/library/dn495627.aspx).
Hello následující metoda má toobe přidat toohello **NotificationHub** třída toocreate hello token podle hello identifikátoru URI aktuální žádosti hello a přihlašovací údaje hello extrahoval z hello připojovací řetězec.

    private function generateSasToken($uri) {
        $targetUri = strtolower(rawurlencode(strtolower($uri)));

        $expires = time();
        $expiresInMins = 60;
        $expires = $expires + $expiresInMins * 60;
        $toSign = $targetUri . "\n" . $expires;

        $signature = rawurlencode(base64_encode(hash_hmac('sha256', $toSign, $this->sasKeyValue, TRUE)));

        $token = "SharedAccessSignature sr=" . $targetUri . "&sig="
                    . $signature . "&se=" . $expires . "&skn=" . $this->sasKeyName;

        return $token;
    }

### <a name="send-a-notification"></a>Odeslat oznámení
Dejte nám nejdřív definice třídy představující oznámení.

    class Notification {
        public $format;
        public $payload;

        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        public $headers;

        function __construct($format, $payload) {
            if (!in_array($format, ["template", "apple", "windows", "gcm", "windowsphone"])) {
                throw new Exception('Invalid format: ' . $format);
            }

            $this->format = $format;
            $this->payload = $payload;
        }
    }

Tato třída je kontejner pro nativní oznámení text, nebo sadu vlastností v případě šablony oznámení a sadu hlaviček, který obsahuje formátu (nativní platforma nebo šablony) a vlastnosti specifické pro platformu (např. vlastnost Apple vypršení platnosti a WNS hlavičky).

Podrobnosti najdete toohello [dokumentaci rozhraní API REST centra oznámení](http://msdn.microsoft.com/library/dn495827.aspx) a hello formáty konkrétní oznámení platformy pro všechny hello možnosti, které jsou k dispozici.

Díky této třídy, jsme nyní můžete napsat hello odesílání oznámení metody uvnitř hello **NotificationHub** třídy.

    public function sendNotification($notification, $tagsOrTagExpression="") {
        if (is_array($tagsOrTagExpression)) {
            $tagExpression = implode(" || ", $tagsOrTagExpression);
        } else {
            $tagExpression = $tagsOrTagExpression;
        }

        # build uri
        $uri = $this->endpoint . $this->hubPath . "/messages" . NotificationHub::API_VERSION;
        $ch = curl_init($uri);

        if (in_array($notification->format, ["template", "apple", "gcm"])) {
            $contentType = "application/json";
        } else {
            $contentType = "application/xml";
        }

        $token = $this->generateSasToken($uri);

        $headers = [
            'Authorization: '.$token,
            'Content-Type: '.$contentType,
            'ServiceBusNotification-Format: '.$notification->format
        ];

        if ("" !== $tagExpression) {
            $headers[] = 'ServiceBusNotification-Tags: '.$tagExpression;
        }

        # add headers for other platforms
        if (is_array($notification->headers)) {
            $headers = array_merge($headers, $notification->headers);
        }

        curl_setopt_array($ch, array(
            CURLOPT_POST => TRUE,
            CURLOPT_RETURNTRANSFER => TRUE,
            CURLOPT_SSL_VERIFYPEER => FALSE,
            CURLOPT_HTTPHEADER => $headers,
            CURLOPT_POSTFIELDS => $notification->payload
        ));

        // Send hello request
        $response = curl_exec($ch);

        // Check for errors
        if($response === FALSE){
            throw new Exception(curl_error($ch));
        }

        $info = curl_getinfo($ch);

        if ($info['http_code'] <> 201) {
            throw new Exception('Error sending notificaiton: '. $info['http_code'] . ' msg: ' . $response);
        }
    } 

Hello výše metody odeslat HTTP POST požadavek toohello /messages koncový bod centra oznámení, s správné textu hello a hlavičky toosend hello oznámení.

## <a name="complete-tutorial"></a>Dokončení hello kurzu
Nyní můžete provést kurzu Začínáme hello odesláním hello oznámení z PHP back-end.

Inicializace vašeho centra oznámení klienta (nahraďte hello připojovací řetězec a názvu centra podle pokynů v hello [kurzu Začínáme Get]):

    $hub = new NotificationHub("connection string", "hubname");    

Pak přidejte kód odesílání hello v závislosti na svou cílovou platformu mobilních.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows Store a Windows Phone 8.1 (bez Silverlight)
    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);

### <a name="ios"></a>iOS
    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);

### <a name="android"></a>Android
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("gcm", $message);
    $hub->sendNotification($notification, null);

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 a 8.1 Silverlight
    $toast = '<?xml version="1.0" encoding="utf-8"?>' .
                '<wp:Notification xmlns:wp="WPNotification">' .
                   '<wp:Toast>' .
                        '<wp:Text1>Hello from PHP!</wp:Text1>' .
                   '</wp:Toast> ' .
                '</wp:Notification>';
    $notification = new Notification("windowsphone", $toast);
    $notification->headers[] = 'X-WindowsPhone-Target : toast';
    $notification->headers[] = 'X-NotificationClass : 2';
    $hub->sendNotification($notification, null);


### <a name="kindle-fire"></a>Kindle Fire
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);

Spuštěním kódu PHP by měl vytvořit nyní oznámení, které jsou na cílovém zařízení.

## <a name="next-steps"></a>Další kroky
V tomto tématu jsme vám ukázal, jak toocreate jednoduché Java REST klienta pro centra oznámení. Odsud můžete:

* Stáhnout hello úplné [PHP REST obálku ukázka], která obsahuje všechny výše uvedený kód hello.
* Pokračujte ve čtení o centrech oznámení označování funkce v hello [novinkách kurzu]
* Další informace o nabízené oznámení tooindividual uživatelé v [upozornění uživatelů kurzu]

Další informace najdete v tématu taky hello [středisku pro vývojáře PHP](/develop/php/).

[PHP REST obálku ukázka]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[kurzu Začínáme Get]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/

