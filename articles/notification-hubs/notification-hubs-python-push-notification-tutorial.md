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
# <a name="how-toouse-notification-hubs-from-python"></a><span data-ttu-id="2c1c8-103">Jak toouse Notification Hubs z Pythonu</span><span class="sxs-lookup"><span data-stu-id="2c1c8-103">How toouse Notification Hubs from Python</span></span>
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

<span data-ttu-id="2c1c8-104">Můžete ke všem funkcím centra oznámení z Java/PHP nebo Python nebo Ruby back-end pomocí rozhraní REST centra oznámení hello, jak je popsáno v tématu MSDN hello [rozhraní API REST centra oznámení](http://msdn.microsoft.com/library/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="2c1c8-104">You can access all Notification Hubs features from a Java/PHP/Python/Ruby back-end using hello Notification Hub REST interface as described in hello MSDN topic [Notification Hubs REST APIs](http://msdn.microsoft.com/library/dn223264.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="2c1c8-105">To je ukázka odkazu implementace pro implementaci odešle oznámení hello v Pythonu a není hello oficiálně podporován Python SDK centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-105">This is a sample reference implementation for implementing hello notification sends in Python and is not hello officially supported Notifications Hub Python SDK.</span></span>
> 
> <span data-ttu-id="2c1c8-106">Tato ukázka je napsané v Pythonu 3.4.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-106">This sample is written using Python 3.4.</span></span>
> 
> 

<span data-ttu-id="2c1c8-107">V tomto tématu ukážeme postup:</span><span class="sxs-lookup"><span data-stu-id="2c1c8-107">In this topic we show how to:</span></span>

* <span data-ttu-id="2c1c8-108">Vytvoření klienta REST centra oznámení funkcí v Pythonu.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-108">Build a REST client for Notification Hubs features in Python.</span></span>
* <span data-ttu-id="2c1c8-109">Odesílání oznámení pomocí hello Python rozhraní toohello rozhraní API REST centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-109">Send notifications using hello Python interface toohello Notification Hub REST APIs.</span></span> 
* <span data-ttu-id="2c1c8-110">Získá výpis hello požadavků a odpovědí HTTP REST pro ladění vzdělávací účely.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-110">Get a dump of hello HTTP REST request/response for debugging/educational purpose.</span></span> 

<span data-ttu-id="2c1c8-111">Můžete postupovat podle hello [kurzu Začínáme Get](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) pro vaši mobilní platformu podle vlastní volby, implementace část hello back-end v Pythonu.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-111">You can follow hello [Get started tutorial](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) for your mobile platform of choice, implementing hello back-end portion in Python.</span></span>

> [!NOTE]
> <span data-ttu-id="2c1c8-112">obor Hello ukázky hello je jenom omezené toosend oznámení a neprovádí žádné registrace správy.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-112">hello scope of hello sample is only limited toosend notifications and it doesn't do any registration management.</span></span>
> 
> 

## <a name="client-interface"></a><span data-ttu-id="2c1c8-113">Rozhraní klienta</span><span class="sxs-lookup"><span data-stu-id="2c1c8-113">Client interface</span></span>
<span data-ttu-id="2c1c8-114">rozhraní Hello hlavní klienta může poskytnout hello stejných metod, které jsou k dispozici v hello [.NET SDK centra oznámení](http://msdn.microsoft.com/library/jj933431.aspx).</span><span class="sxs-lookup"><span data-stu-id="2c1c8-114">hello main client interface can provide hello same methods that are available in hello [.NET Notification Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx).</span></span> <span data-ttu-id="2c1c8-115">To vám umožní toodirectly převede všechny hello kurzy a ukázky aktuálně k dispozici na tomto webu a přispěli hello komunity na hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-115">This will allow you toodirectly translate all hello tutorials and samples currently available on this site, and contributed by hello community on hello internet.</span></span>

<span data-ttu-id="2c1c8-116">Můžete najít všechny hello kód je k dispozici v hello [Python REST obálku ukázka].</span><span class="sxs-lookup"><span data-stu-id="2c1c8-116">You can find all hello code available in hello [Python REST wrapper sample].</span></span>

<span data-ttu-id="2c1c8-117">Například toocreate klienta:</span><span class="sxs-lookup"><span data-stu-id="2c1c8-117">For example, toocreate a client:</span></span>

    isDebug = True
    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

<span data-ttu-id="2c1c8-118">toosend Windows připít oznámení:</span><span class="sxs-lookup"><span data-stu-id="2c1c8-118">toosend a Windows toast notification:</span></span>

    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

## <a name="implementation"></a><span data-ttu-id="2c1c8-119">Implementace</span><span class="sxs-lookup"><span data-stu-id="2c1c8-119">Implementation</span></span>
<span data-ttu-id="2c1c8-120">Pokud jste ještě není, postupujte podle našich [kurzu Začínáme Get] až toohello poslední část, kde máte tooimplement hello back-end.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-120">If you did not already, please follow our [Get started tutorial] up toohello last section where you have tooimplement hello back-end.</span></span>

<span data-ttu-id="2c1c8-121">Všechny hello podrobnosti tooimplement úplné obálku REST naleznete na [MSDN](http://msdn.microsoft.com/library/dn530746.aspx).</span><span class="sxs-lookup"><span data-stu-id="2c1c8-121">All hello details tooimplement a full REST wrapper can be found on [MSDN](http://msdn.microsoft.com/library/dn530746.aspx).</span></span> <span data-ttu-id="2c1c8-122">V této části jsme se popisují implementaci Python hello hello hlavní kroky požadované tooaccess koncové body REST centra oznámení a odesílání oznámení</span><span class="sxs-lookup"><span data-stu-id="2c1c8-122">In this section we will describe hello Python implementation of hello main steps required tooaccess Notification Hubs REST endpoints and send notifications</span></span>

1. <span data-ttu-id="2c1c8-123">Analyzovat hello připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="2c1c8-123">Parse hello connection string</span></span>
2. <span data-ttu-id="2c1c8-124">Vygenerování tokenu autorizace hello</span><span class="sxs-lookup"><span data-stu-id="2c1c8-124">Generate hello authorization token</span></span>
3. <span data-ttu-id="2c1c8-125">Odeslání oznámení pomocí rozhraní HTTP REST API</span><span class="sxs-lookup"><span data-stu-id="2c1c8-125">Send a notification using HTTP REST API</span></span>

### <a name="parse-hello-connection-string"></a><span data-ttu-id="2c1c8-126">Analyzovat hello připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="2c1c8-126">Parse hello connection string</span></span>
<span data-ttu-id="2c1c8-127">Tady je hlavní třídy hello implementace hello klienta, jejichž konstruktor analyzuje hello připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="2c1c8-127">Here is hello main class implementing hello client, whose constructor parses hello connection string:</span></span>

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


### <a name="create-security-token"></a><span data-ttu-id="2c1c8-128">Vytvoření tokenu zabezpečení</span><span class="sxs-lookup"><span data-stu-id="2c1c8-128">Create security token</span></span>
<span data-ttu-id="2c1c8-129">Podrobnosti o Hello hello vytvoření tokenu zabezpečení jsou k dispozici [zde](http://msdn.microsoft.com/library/dn495627.aspx).</span><span class="sxs-lookup"><span data-stu-id="2c1c8-129">hello details of hello security token creation are available [here](http://msdn.microsoft.com/library/dn495627.aspx).</span></span>
<span data-ttu-id="2c1c8-130">Hello následující metody mají toobe přidat toohello **NotificationHub** třída toocreate hello token podle hello identifikátoru URI aktuální žádosti hello a přihlašovací údaje hello extrahoval z hello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-130">hello following methods have toobe added toohello **NotificationHub** class toocreate hello token based on hello URI of hello current request and hello credentials extracted from hello connection string.</span></span>

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

### <a name="send-a-notification-using-http-rest-api"></a><span data-ttu-id="2c1c8-131">Odeslání oznámení pomocí rozhraní HTTP REST API</span><span class="sxs-lookup"><span data-stu-id="2c1c8-131">Send a notification using HTTP REST API</span></span>
<span data-ttu-id="2c1c8-132">První, umožňují použít definice třídy představující oznámení.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-132">First, let use define a class representing a notification.</span></span>

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

<span data-ttu-id="2c1c8-133">Tato třída je kontejner pro nativní oznámení textu nebo sadu vlastností v případě šablony oznámení, sadu hlaviček, který obsahuje formátu (nativní platforma nebo šablony) a vlastnosti specifické pro platformu (např. vlastnost Apple vypršení platnosti a hlavičky WNS).</span><span class="sxs-lookup"><span data-stu-id="2c1c8-133">This class is a container for a native notification body or a set of properties in case of a template notification, a set of headers which contains format (native platform or template) and platform-specific properties (like Apple expiration property and WNS headers).</span></span>

<span data-ttu-id="2c1c8-134">Podrobnosti najdete toohello [dokumentaci rozhraní API REST centra oznámení](http://msdn.microsoft.com/library/dn495827.aspx) a hello formáty konkrétní oznámení platformy pro všechny hello možnosti, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-134">Please refer toohello [Notification Hubs REST APIs documentation](http://msdn.microsoft.com/library/dn495827.aspx) and hello specific notification platforms' formats for all hello options available.</span></span>

<span data-ttu-id="2c1c8-135">Teď s touto třídou jsme může zapisovat hello odesílání oznámení metody uvnitř hello **NotificationHub** třídy.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-135">Now with this class, we can write hello send notification methods inside of hello **NotificationHub** class.</span></span>

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

<span data-ttu-id="2c1c8-136">Hello výše metody odeslat HTTP POST požadavek toohello /messages koncový bod centra oznámení, s správné textu hello a hlavičky toosend hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-136">hello above methods send an HTTP POST request toohello /messages endpoint of your notification hub, with hello correct body and headers toosend hello notification.</span></span>

### <a name="using-debug-property-tooenable-detailed-logging"></a><span data-ttu-id="2c1c8-137">Pomocí ladění vlastnost tooenable podrobné protokolování</span><span class="sxs-lookup"><span data-stu-id="2c1c8-137">Using debug property tooenable detailed logging</span></span>
<span data-ttu-id="2c1c8-138">Povolení ladění vlastnost při inicializaci hello Centrum oznámení se zapsat podrobné protokolování informací o hello HTTP, že požadavek a odpověď výpisu, jakož i podrobné zprávy oznámení odeslat výsledek.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-138">Enabling debug property while initializing hello Notification Hub will write out detailed logging information about hello HTTP request and response dump as well as detailed Notification message send outcome.</span></span> <span data-ttu-id="2c1c8-139">Nedávno jsme přidali tato vlastnost s názvem [TestSend centra oznámení vlastnost](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) která vrací podrobné informace o hello výsledek odeslání oznámení.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-139">We recently added this property called [Notification Hubs TestSend property](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) which returns detailed information about hello notification send outcome.</span></span> <span data-ttu-id="2c1c8-140">toouse ho - inicializovat pomocí hello následující:</span><span class="sxs-lookup"><span data-stu-id="2c1c8-140">toouse it - initialize using hello following:</span></span>

    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

<span data-ttu-id="2c1c8-141">žádost odeslat oznámení centra Hello adresy URL protokolu HTTP získá spolu s "test" querystring v důsledku.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-141">hello Notification Hub Send request HTTP URL gets appended with a "test" querystring as a result.</span></span> 

## <span data-ttu-id="2c1c8-142"><a name="complete-tutorial"></a>Dokončení hello kurzu</span><span class="sxs-lookup"><span data-stu-id="2c1c8-142"><a name="complete-tutorial"></a>Complete hello tutorial</span></span>
<span data-ttu-id="2c1c8-143">Nyní můžete provést kurzu Začínáme hello zasláním oznámení hello z Python back-end.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-143">Now you can complete hello Get Started tutorial by sending hello notification from a Python back-end.</span></span>

<span data-ttu-id="2c1c8-144">Inicializace vašeho centra oznámení klienta (nahraďte hello připojovací řetězec a názvu centra podle pokynů v hello [kurzu Začínáme Get]):</span><span class="sxs-lookup"><span data-stu-id="2c1c8-144">Initialize your Notification Hubs client (substitute hello connection string and hub name as instructed in hello [Get started tutorial]):</span></span>

    hub = NotificationHub("myConnectionString", "myNotificationHubName")

<span data-ttu-id="2c1c8-145">Pak přidejte kód odesílání hello v závislosti na svou cílovou platformu mobilních.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-145">Then add hello send code depending on your target mobile platform.</span></span> <span data-ttu-id="2c1c8-146">Tato ukázka přidá také vyšší úrovně metody tooenable odesílání oznámení založené na platformě hello například send_windows_notification pro systém windows. send_apple_notification (pro apple) atd.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-146">This sample also adds higher level methods tooenable sending notifications based on hello platform e.g. send_windows_notification for windows; send_apple_notification (for apple) etc.</span></span> 

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a><span data-ttu-id="2c1c8-147">Windows Store a Windows Phone 8.1 (bez Silverlight)</span><span class="sxs-lookup"><span data-stu-id="2c1c8-147">Windows Store and Windows Phone 8.1 (non-Silverlight)</span></span>
    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

### <a name="windows-phone-80-and-81-silverlight"></a><span data-ttu-id="2c1c8-148">Windows Phone 8.0 a 8.1 Silverlight</span><span class="sxs-lookup"><span data-stu-id="2c1c8-148">Windows Phone 8.0 and 8.1 Silverlight</span></span>
    hub.send_mpns_notification(toast)

### <a name="ios"></a><span data-ttu-id="2c1c8-149">iOS</span><span class="sxs-lookup"><span data-stu-id="2c1c8-149">iOS</span></span>
    alert_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_apple_notification(alert_payload)

### <a name="android"></a><span data-ttu-id="2c1c8-150">Android</span><span class="sxs-lookup"><span data-stu-id="2c1c8-150">Android</span></span>
    gcm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_gcm_notification(gcm_payload)

### <a name="kindle-fire"></a><span data-ttu-id="2c1c8-151">Kindle Fire</span><span class="sxs-lookup"><span data-stu-id="2c1c8-151">Kindle Fire</span></span>
    adm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_adm_notification(adm_payload)

### <a name="baidu"></a><span data-ttu-id="2c1c8-152">Baidu</span><span class="sxs-lookup"><span data-stu-id="2c1c8-152">Baidu</span></span>
    baidu_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_baidu_notification(baidu_payload)

<span data-ttu-id="2c1c8-153">Spuštěním kódu Python by měl vytvořit oznámení, které jsou na cílovém zařízení.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-153">Running your Python code should produce a notification appearing on your target device.</span></span>

## <a name="examples"></a><span data-ttu-id="2c1c8-154">Příklady:</span><span class="sxs-lookup"><span data-stu-id="2c1c8-154">Examples:</span></span>
### <a name="enabling-debug-property"></a><span data-ttu-id="2c1c8-155">Povolení ladění vlastnost</span><span class="sxs-lookup"><span data-stu-id="2c1c8-155">Enabling debug property</span></span>
<span data-ttu-id="2c1c8-156">Když povolíte příznak ladění při inicializaci hello NotificationHub, zobrazí se podrobné výpisu žádosti a odpovědi protokolu HTTP, jakož i NotificationOutcome jako hello následující, kde pochopit, jaké hlavičky protokolu HTTP se předávají v žádosti o hello a jaké HTTP byla přijata odpověď z hello Centrum oznámení:![][1]</span><span class="sxs-lookup"><span data-stu-id="2c1c8-156">When you enable debug flag while initializing hello NotificationHub then you will see detailed HTTP request and response dump as well as NotificationOutcome like hello following where you can understand what HTTP headers are passed in hello request and what HTTP response was received from hello Notification Hub: ![][1]</span></span>

<span data-ttu-id="2c1c8-157">Uvidíte, například podrobný výsledek centra oznámení</span><span class="sxs-lookup"><span data-stu-id="2c1c8-157">You will see detailed Notification Hub result e.g.</span></span> 

* <span data-ttu-id="2c1c8-158">Když hello je úspěšně odeslána zpráva toohello službu nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-158">when hello message is successfully sent toohello Push Notification Service.</span></span> 
  
        <Outcome>hello Notification was successfully sent toohello Push Notification System</Outcome>
* <span data-ttu-id="2c1c8-159">Pokud nebyly nalezeny žádné cíle pro všechny nabízené oznámení pak budete pravděpodobně toosee hello následující hello odpovědi (což znamená, že neexistují žádné registrace pravděpodobně nalézt toodeliver hello oznámení, protože registrace hello měl některé Neshoda značek)</span><span class="sxs-lookup"><span data-stu-id="2c1c8-159">If there were no targets found for any push notification then you are likely going toosee hello following in hello response (which indicates that there were no registrations found toodeliver hello notification probably because hello registrations had some mismatched tags)</span></span>
  
        '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'

### <a name="broadcast-toast-notification-toowindows"></a><span data-ttu-id="2c1c8-160">Vysílání tooWindows oznámení informační zprávy</span><span class="sxs-lookup"><span data-stu-id="2c1c8-160">Broadcast toast notification tooWindows</span></span>
<span data-ttu-id="2c1c8-161">Všimněte si hello hlavičky, které získat odeslaná při odesílání oznámení klienta tooWindows informační zprávy všesměrového vysílání.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-161">Notice hello headers that get sent out when you are sending a broadcast toast notification tooWindows client.</span></span> 

    hub.send_windows_notification(wns_payload)

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a><span data-ttu-id="2c1c8-162">Odeslat oznámení o určení značky (nebo výraz označení)</span><span class="sxs-lookup"><span data-stu-id="2c1c8-162">Send notification specifying a tag (or tag expression)</span></span>
<span data-ttu-id="2c1c8-163">Hlavičky protokolu HTTP značky hello oznámení, který získá přidat toohello HTTP požadavku (v příkladu hello níže jsme odesílání hello oznámení pouze tooregistrations s odebranou datovou částí "sports")</span><span class="sxs-lookup"><span data-stu-id="2c1c8-163">Notice hello Tags HTTP header which gets added toohello HTTP request (in hello example below, we are sending hello notification only tooregistrations with 'sports' payload)</span></span>

    hub.send_windows_notification(wns_payload, "sports")

![][3]

### <a name="send-notification-specifying-multiple-tags"></a><span data-ttu-id="2c1c8-164">Odeslat oznámení o určení více značek</span><span class="sxs-lookup"><span data-stu-id="2c1c8-164">Send notification specifying multiple tags</span></span>
<span data-ttu-id="2c1c8-165">Všimněte si, jak hlavičky protokolu HTTP značky hello změní, když jsou odesílány více značek.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-165">Notice how hello Tags HTTP header changes when multiple tags are sent.</span></span> 

    tags = {'sports', 'politics'}
    hub.send_windows_notification(wns_payload, tags)

![][4]

### <a name="templated-notification"></a><span data-ttu-id="2c1c8-166">Použitím šablon oznámení</span><span class="sxs-lookup"><span data-stu-id="2c1c8-166">Templated notification</span></span>
<span data-ttu-id="2c1c8-167">Všimněte si, že hello změny záhlaví HTTP formátu a datovou část textu hello je odeslána jako část obsahu žádosti hello HTTP:</span><span class="sxs-lookup"><span data-stu-id="2c1c8-167">Notice that hello Format HTTP header changes and hello payload body is sent as part of hello HTTP request body:</span></span>

<span data-ttu-id="2c1c8-168">**Na straně klienta - registrované šablony**</span><span class="sxs-lookup"><span data-stu-id="2c1c8-168">**Client side - registered template**</span></span>

        var template =
                        @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";

<span data-ttu-id="2c1c8-169">**Na straně serveru – odesílání hello datové části**</span><span class="sxs-lookup"><span data-stu-id="2c1c8-169">**Server side - sending hello payload**</span></span>

        template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
        hub.send_template_notification(template_payload)

![][5]

## <a name="next-steps"></a><span data-ttu-id="2c1c8-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2c1c8-170">Next Steps</span></span>
<span data-ttu-id="2c1c8-171">V tomto tématu jsme vám ukázal, jak toocreate jednoduché Python REST klienta pro centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-171">In this topic we showed how toocreate a simple Python REST client for Notification Hubs.</span></span> <span data-ttu-id="2c1c8-172">Odsud můžete:</span><span class="sxs-lookup"><span data-stu-id="2c1c8-172">From here you can:</span></span>

* <span data-ttu-id="2c1c8-173">Stáhnout hello úplné [Python REST obálku ukázka], která obsahuje všechny výše uvedený kód hello.</span><span class="sxs-lookup"><span data-stu-id="2c1c8-173">Download hello full [Python REST wrapper sample], which contains all hello code above.</span></span>
* <span data-ttu-id="2c1c8-174">Pokračujte ve čtení o centrech oznámení označování funkce v hello [novinkách kurzu]</span><span class="sxs-lookup"><span data-stu-id="2c1c8-174">Continue learning about Notification Hubs tagging feature in hello [Breaking News tutorial]</span></span>
* <span data-ttu-id="2c1c8-175">Pokračujte ve čtení o funkce šablon centra oznámení v hello [kurzu lokalizace zprávy]</span><span class="sxs-lookup"><span data-stu-id="2c1c8-175">Continue learning about Notification Hubs Templates feature in hello [Localizing News tutorial]</span></span>

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

