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
# <a name="how-toouse-notification-hubs-from-php"></a><span data-ttu-id="4e710-103">Jak toouse Notification Hubs z PHP</span><span class="sxs-lookup"><span data-stu-id="4e710-103">How toouse Notification Hubs from PHP</span></span>
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

<span data-ttu-id="4e710-104">Můžete ke všem funkcím centra oznámení z Java/PHP nebo Ruby back-end pomocí rozhraní REST centra oznámení hello, jak je popsáno v tématu MSDN hello [rozhraní API REST centra oznámení](http://msdn.microsoft.com/library/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="4e710-104">You can access all Notification Hubs features from a Java/PHP/Ruby back-end using hello Notification Hub REST interface as described in hello MSDN topic [Notification Hubs REST APIs](http://msdn.microsoft.com/library/dn223264.aspx).</span></span>

<span data-ttu-id="4e710-105">V tomto tématu ukážeme postup:</span><span class="sxs-lookup"><span data-stu-id="4e710-105">In this topic we show how to:</span></span>

* <span data-ttu-id="4e710-106">Vytvoření klienta REST pro funkce Notification Hubs v jazyce PHP;</span><span class="sxs-lookup"><span data-stu-id="4e710-106">Build a REST client for Notification Hubs features in PHP;</span></span>
* <span data-ttu-id="4e710-107">Postupujte podle hello [kurzu Začínáme Get](notification-hubs-ios-apple-push-notification-apns-get-started.md) pro vaši mobilní platformu podle vlastní volby, implementace část hello back-end v jazyce PHP.</span><span class="sxs-lookup"><span data-stu-id="4e710-107">Follow hello [Get started tutorial](notification-hubs-ios-apple-push-notification-apns-get-started.md) for your mobile platform of choice, implementing hello back-end portion in PHP.</span></span>

## <a name="client-interface"></a><span data-ttu-id="4e710-108">Rozhraní klienta</span><span class="sxs-lookup"><span data-stu-id="4e710-108">Client interface</span></span>
<span data-ttu-id="4e710-109">rozhraní Hello hlavní klienta může poskytnout hello stejných metod, které jsou k dispozici v hello [.NET SDK centra oznámení](http://msdn.microsoft.com/library/jj933431.aspx), bude možné toodirectly převede všechny hello kurzy a ukázky aktuálně k dispozici na tomto webu a přispěli hello komunity na hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="4e710-109">hello main client interface can provide hello same methods that are available in hello [.NET Notification Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx), this will allow you toodirectly translate all hello tutorials and samples currently available on this site, and contributed by hello community on hello internet.</span></span>

<span data-ttu-id="4e710-110">Můžete najít všechny hello kód je k dispozici v hello [PHP REST obálku ukázka].</span><span class="sxs-lookup"><span data-stu-id="4e710-110">You can find all hello code available in hello [PHP REST wrapper sample].</span></span>

<span data-ttu-id="4e710-111">Například toocreate klienta:</span><span class="sxs-lookup"><span data-stu-id="4e710-111">For example, toocreate a client:</span></span>

    $hub = new NotificationHub("connection string", "hubname");    

<span data-ttu-id="4e710-112">toosend nativní oznámení iOS:</span><span class="sxs-lookup"><span data-stu-id="4e710-112">toosend an iOS native notification:</span></span>

    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);

## <a name="implementation"></a><span data-ttu-id="4e710-113">Implementace</span><span class="sxs-lookup"><span data-stu-id="4e710-113">Implementation</span></span>
<span data-ttu-id="4e710-114">Pokud jste ještě není, postupujte podle našich [kurzu Začínáme Get] až toohello poslední část, kde máte tooimplement hello back-end.</span><span class="sxs-lookup"><span data-stu-id="4e710-114">If you did not already, please follow our [Get started tutorial] up toohello last section where you have tooimplement hello back-end.</span></span>
<span data-ttu-id="4e710-115">Navíc pokud chcete, můžete použít hello kód z hello [PHP REST obálku ukázka] , přejděte přímo toohello [hello dokončení kurzu](#complete-tutorial) části.</span><span class="sxs-lookup"><span data-stu-id="4e710-115">Also, if you want you can use hello code from hello [PHP REST wrapper sample] and go directly toohello [Complete hello tutorial](#complete-tutorial) section.</span></span>

<span data-ttu-id="4e710-116">Všechny hello podrobnosti tooimplement úplné obálku REST naleznete na [MSDN](http://msdn.microsoft.com/library/dn530746.aspx).</span><span class="sxs-lookup"><span data-stu-id="4e710-116">All hello details tooimplement a full REST wrapper can be found on [MSDN](http://msdn.microsoft.com/library/dn530746.aspx).</span></span> <span data-ttu-id="4e710-117">V této části jsme se popisují implementaci PHP hello hello hlavní kroky požadované tooaccess koncové body REST centra oznámení:</span><span class="sxs-lookup"><span data-stu-id="4e710-117">In this section we will describe hello PHP implementation of hello main steps required tooaccess Notification Hubs REST endpoints:</span></span>

1. <span data-ttu-id="4e710-118">Analyzovat hello připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="4e710-118">Parse hello connection string</span></span>
2. <span data-ttu-id="4e710-119">Vygenerování tokenu autorizace hello</span><span class="sxs-lookup"><span data-stu-id="4e710-119">Generate hello authorization token</span></span>
3. <span data-ttu-id="4e710-120">Provádění volání hello HTTP</span><span class="sxs-lookup"><span data-stu-id="4e710-120">Perform hello HTTP call</span></span>

### <a name="parse-hello-connection-string"></a><span data-ttu-id="4e710-121">Analyzovat hello připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="4e710-121">Parse hello connection string</span></span>
<span data-ttu-id="4e710-122">Tady je hello hlavní třídy implementující hello klienta, jejichž konstruktor, který analyzuje hello připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="4e710-122">Here is hello main class implementing hello client, whose constructor that parses hello connection string:</span></span>

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


### <a name="create-security-token"></a><span data-ttu-id="4e710-123">Vytvoření tokenu zabezpečení</span><span class="sxs-lookup"><span data-stu-id="4e710-123">Create security token</span></span>
<span data-ttu-id="4e710-124">Podrobnosti o Hello hello vytvoření tokenu zabezpečení jsou k dispozici [zde](http://msdn.microsoft.com/library/dn495627.aspx).</span><span class="sxs-lookup"><span data-stu-id="4e710-124">hello details of hello security token creation are available [here](http://msdn.microsoft.com/library/dn495627.aspx).</span></span>
<span data-ttu-id="4e710-125">Hello následující metoda má toobe přidat toohello **NotificationHub** třída toocreate hello token podle hello identifikátoru URI aktuální žádosti hello a přihlašovací údaje hello extrahoval z hello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="4e710-125">hello following method has toobe added toohello **NotificationHub** class toocreate hello token based on hello URI of hello current request and hello credentials extracted from hello connection string.</span></span>

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

### <a name="send-a-notification"></a><span data-ttu-id="4e710-126">Odeslat oznámení</span><span class="sxs-lookup"><span data-stu-id="4e710-126">Send a notification</span></span>
<span data-ttu-id="4e710-127">Dejte nám nejdřív definice třídy představující oznámení.</span><span class="sxs-lookup"><span data-stu-id="4e710-127">First, let us define a class representing a notification.</span></span>

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

<span data-ttu-id="4e710-128">Tato třída je kontejner pro nativní oznámení text, nebo sadu vlastností v případě šablony oznámení a sadu hlaviček, který obsahuje formátu (nativní platforma nebo šablony) a vlastnosti specifické pro platformu (např. vlastnost Apple vypršení platnosti a WNS hlavičky).</span><span class="sxs-lookup"><span data-stu-id="4e710-128">This class is a container for a native notification body, or a set of properties on case of a template notification, and a set of headers which contains format (native platform or template) and platform-specific properties (like Apple expiration property and WNS headers).</span></span>

<span data-ttu-id="4e710-129">Podrobnosti najdete toohello [dokumentaci rozhraní API REST centra oznámení](http://msdn.microsoft.com/library/dn495827.aspx) a hello formáty konkrétní oznámení platformy pro všechny hello možnosti, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="4e710-129">Please refer toohello [Notification Hubs REST APIs documentation](http://msdn.microsoft.com/library/dn495827.aspx) and hello specific notification platforms' formats for all hello options available.</span></span>

<span data-ttu-id="4e710-130">Díky této třídy, jsme nyní můžete napsat hello odesílání oznámení metody uvnitř hello **NotificationHub** třídy.</span><span class="sxs-lookup"><span data-stu-id="4e710-130">Armed with this class, we can now write hello send notification methods inside of hello **NotificationHub** class.</span></span>

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

<span data-ttu-id="4e710-131">Hello výše metody odeslat HTTP POST požadavek toohello /messages koncový bod centra oznámení, s správné textu hello a hlavičky toosend hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="4e710-131">hello above methods send an HTTP POST request toohello /messages endpoint of your notification hub, with hello correct body and headers toosend hello notification.</span></span>

## <span data-ttu-id="4e710-132"><a name="complete-tutorial"></a>Dokončení hello kurzu</span><span class="sxs-lookup"><span data-stu-id="4e710-132"><a name="complete-tutorial"></a>Complete hello tutorial</span></span>
<span data-ttu-id="4e710-133">Nyní můžete provést kurzu Začínáme hello odesláním hello oznámení z PHP back-end.</span><span class="sxs-lookup"><span data-stu-id="4e710-133">Now you can complete hello Get Started tutorial by sending hello notification from a PHP back-end.</span></span>

<span data-ttu-id="4e710-134">Inicializace vašeho centra oznámení klienta (nahraďte hello připojovací řetězec a názvu centra podle pokynů v hello [kurzu Začínáme Get]):</span><span class="sxs-lookup"><span data-stu-id="4e710-134">Initialize your Notification Hubs client (substitute hello connection string and hub name as instructed in hello [Get started tutorial]):</span></span>

    $hub = new NotificationHub("connection string", "hubname");    

<span data-ttu-id="4e710-135">Pak přidejte kód odesílání hello v závislosti na svou cílovou platformu mobilních.</span><span class="sxs-lookup"><span data-stu-id="4e710-135">Then add hello send code depending on your target mobile platform.</span></span>

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a><span data-ttu-id="4e710-136">Windows Store a Windows Phone 8.1 (bez Silverlight)</span><span class="sxs-lookup"><span data-stu-id="4e710-136">Windows Store and Windows Phone 8.1 (non-Silverlight)</span></span>
    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);

### <a name="ios"></a><span data-ttu-id="4e710-137">iOS</span><span class="sxs-lookup"><span data-stu-id="4e710-137">iOS</span></span>
    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);

### <a name="android"></a><span data-ttu-id="4e710-138">Android</span><span class="sxs-lookup"><span data-stu-id="4e710-138">Android</span></span>
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("gcm", $message);
    $hub->sendNotification($notification, null);

### <a name="windows-phone-80-and-81-silverlight"></a><span data-ttu-id="4e710-139">Windows Phone 8.0 a 8.1 Silverlight</span><span class="sxs-lookup"><span data-stu-id="4e710-139">Windows Phone 8.0 and 8.1 Silverlight</span></span>
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


### <a name="kindle-fire"></a><span data-ttu-id="4e710-140">Kindle Fire</span><span class="sxs-lookup"><span data-stu-id="4e710-140">Kindle Fire</span></span>
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);

<span data-ttu-id="4e710-141">Spuštěním kódu PHP by měl vytvořit nyní oznámení, které jsou na cílovém zařízení.</span><span class="sxs-lookup"><span data-stu-id="4e710-141">Running your PHP code should produce now a notification appearing on your target device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4e710-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4e710-142">Next Steps</span></span>
<span data-ttu-id="4e710-143">V tomto tématu jsme vám ukázal, jak toocreate jednoduché Java REST klienta pro centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="4e710-143">In this topic we showed how toocreate a simple Java REST client for Notification Hubs.</span></span> <span data-ttu-id="4e710-144">Odsud můžete:</span><span class="sxs-lookup"><span data-stu-id="4e710-144">From here you can:</span></span>

* <span data-ttu-id="4e710-145">Stáhnout hello úplné [PHP REST obálku ukázka], která obsahuje všechny výše uvedený kód hello.</span><span class="sxs-lookup"><span data-stu-id="4e710-145">Download hello full [PHP REST wrapper sample], which contains all hello code above.</span></span>
* <span data-ttu-id="4e710-146">Pokračujte ve čtení o centrech oznámení označování funkce v hello [novinkách kurzu]</span><span class="sxs-lookup"><span data-stu-id="4e710-146">Continue learning about Notification Hubs tagging feature in hello [Breaking News tutorial]</span></span>
* <span data-ttu-id="4e710-147">Další informace o nabízené oznámení tooindividual uživatelé v [upozornění uživatelů kurzu]</span><span class="sxs-lookup"><span data-stu-id="4e710-147">Learn about pushing notifications tooindividual users in [Notify Users tutorial]</span></span>

<span data-ttu-id="4e710-148">Další informace najdete v tématu taky hello [středisku pro vývojáře PHP](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="4e710-148">For more information, see also hello [PHP Developer Center](/develop/php/).</span></span>

[PHP REST obálku ukázka]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[kurzu Začínáme Get]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/

