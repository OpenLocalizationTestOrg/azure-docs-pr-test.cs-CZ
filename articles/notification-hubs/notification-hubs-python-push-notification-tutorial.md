---
title: "Jak používat centra oznámení s Pythonem"
description: "Naučte se používat Azure Notification Hubs z Python back-end."
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
ms.openlocfilehash: 9ceedb9940759427fc8cec74a1307e42472563a6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-notification-hubs-from-python"></a><span data-ttu-id="11261-103">Jak používat centra oznámení z Pythonu</span><span class="sxs-lookup"><span data-stu-id="11261-103">How to use Notification Hubs from Python</span></span>
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

<span data-ttu-id="11261-104">Můžete ke všem funkcím centra oznámení z Java/PHP nebo Python nebo Ruby back-end pomocí rozhraní REST centra oznámení, jak je popsáno v tématu MSDN [rozhraní API REST centra oznámení](http://msdn.microsoft.com/library/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="11261-104">You can access all Notification Hubs features from a Java/PHP/Python/Ruby back-end using the Notification Hub REST interface as described in the MSDN topic [Notification Hubs REST APIs](http://msdn.microsoft.com/library/dn223264.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="11261-105">To je ukázka odkazu implementace pro implementaci odešle oznámení v Pythonu a není oficiálně podporované Python SDK centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="11261-105">This is a sample reference implementation for implementing the notification sends in Python and is not the officially supported Notifications Hub Python SDK.</span></span>
> 
> <span data-ttu-id="11261-106">Tato ukázka je napsané v Pythonu 3.4.</span><span class="sxs-lookup"><span data-stu-id="11261-106">This sample is written using Python 3.4.</span></span>
> 
> 

<span data-ttu-id="11261-107">V tomto tématu ukážeme postup:</span><span class="sxs-lookup"><span data-stu-id="11261-107">In this topic we show how to:</span></span>

* <span data-ttu-id="11261-108">Vytvoření klienta REST centra oznámení funkcí v Pythonu.</span><span class="sxs-lookup"><span data-stu-id="11261-108">Build a REST client for Notification Hubs features in Python.</span></span>
* <span data-ttu-id="11261-109">Odesílání oznámení pomocí rozhraní Python rozhraní API REST centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="11261-109">Send notifications using the Python interface to the Notification Hub REST APIs.</span></span> 
* <span data-ttu-id="11261-110">Získá výpis požadavků a odpovědí HTTP REST pro ladění vzdělávací účely.</span><span class="sxs-lookup"><span data-stu-id="11261-110">Get a dump of the HTTP REST request/response for debugging/educational purpose.</span></span> 

<span data-ttu-id="11261-111">Můžete provést [kurzu Začínáme Get](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) pro vaši mobilní platformu podle volby implementace části back-end v Pythonu.</span><span class="sxs-lookup"><span data-stu-id="11261-111">You can follow the [Get started tutorial](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) for your mobile platform of choice, implementing the back-end portion in Python.</span></span>

> [!NOTE]
> <span data-ttu-id="11261-112">Obor vzorku je omezena pouze k odesílání oznámení a neprovádí žádné registrace správy.</span><span class="sxs-lookup"><span data-stu-id="11261-112">The scope of the sample is only limited to send notifications and it doesn't do any registration management.</span></span>
> 
> 

## <a name="client-interface"></a><span data-ttu-id="11261-113">Rozhraní klienta</span><span class="sxs-lookup"><span data-stu-id="11261-113">Client interface</span></span>
<span data-ttu-id="11261-114">Rozhraní hlavní klienta může poskytovat stejné metody, které jsou k dispozici v [.NET SDK centra oznámení](http://msdn.microsoft.com/library/jj933431.aspx).</span><span class="sxs-lookup"><span data-stu-id="11261-114">The main client interface can provide the same methods that are available in the [.NET Notification Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx).</span></span> <span data-ttu-id="11261-115">To vám umožní přímo převést všechny výukové programy a ukázky aktuálně k dispozici na tomto webu a přispěli komunity na Internetu.</span><span class="sxs-lookup"><span data-stu-id="11261-115">This will allow you to directly translate all the tutorials and samples currently available on this site, and contributed by the community on the internet.</span></span>

<span data-ttu-id="11261-116">K dispozici v kódu lze najít [Python REST obálku ukázka].</span><span class="sxs-lookup"><span data-stu-id="11261-116">You can find all the code available in the [Python REST wrapper sample].</span></span>

<span data-ttu-id="11261-117">Chcete-li například vytvořit klienta:</span><span class="sxs-lookup"><span data-stu-id="11261-117">For example, to create a client:</span></span>

    isDebug = True
    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

<span data-ttu-id="11261-118">K odeslání oznámení s informační zprávou Windows:</span><span class="sxs-lookup"><span data-stu-id="11261-118">To send a Windows toast notification:</span></span>

    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

## <a name="implementation"></a><span data-ttu-id="11261-119">Implementace</span><span class="sxs-lookup"><span data-stu-id="11261-119">Implementation</span></span>
<span data-ttu-id="11261-120">Pokud jste ještě není, postupujte podle našich [kurzu Začínáme Get] až na poslední část, kde je nutné implementovat back-end.</span><span class="sxs-lookup"><span data-stu-id="11261-120">If you did not already, please follow our [Get started tutorial] up to the last section where you have to implement the back-end.</span></span>

<span data-ttu-id="11261-121">Všechny podrobnosti implementace úplné obálku REST naleznete na [MSDN](http://msdn.microsoft.com/library/dn530746.aspx).</span><span class="sxs-lookup"><span data-stu-id="11261-121">All the details to implement a full REST wrapper can be found on [MSDN](http://msdn.microsoft.com/library/dn530746.aspx).</span></span> <span data-ttu-id="11261-122">V této části jsme se popisují implementaci Python hlavní kroky potřebné pro přístup k koncové body REST centra oznámení a odesílání oznámení</span><span class="sxs-lookup"><span data-stu-id="11261-122">In this section we will describe the Python implementation of the main steps required to access Notification Hubs REST endpoints and send notifications</span></span>

1. <span data-ttu-id="11261-123">Analýza připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="11261-123">Parse the connection string</span></span>
2. <span data-ttu-id="11261-124">Vygenerování tokenu autorizace</span><span class="sxs-lookup"><span data-stu-id="11261-124">Generate the authorization token</span></span>
3. <span data-ttu-id="11261-125">Odeslání oznámení pomocí rozhraní HTTP REST API</span><span class="sxs-lookup"><span data-stu-id="11261-125">Send a notification using HTTP REST API</span></span>

### <a name="parse-the-connection-string"></a><span data-ttu-id="11261-126">Analýza připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="11261-126">Parse the connection string</span></span>
<span data-ttu-id="11261-127">Tady je hlavní třída implementace klienta, jejichž konstruktor analyzuje připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="11261-127">Here is the main class implementing the client, whose constructor parses the connection string:</span></span>

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


### <a name="create-security-token"></a><span data-ttu-id="11261-128">Vytvoření tokenu zabezpečení</span><span class="sxs-lookup"><span data-stu-id="11261-128">Create security token</span></span>
<span data-ttu-id="11261-129">Podrobnosti o vytvoření tokenu zabezpečení jsou k dispozici [zde](http://msdn.microsoft.com/library/dn495627.aspx).</span><span class="sxs-lookup"><span data-stu-id="11261-129">The details of the security token creation are available [here](http://msdn.microsoft.com/library/dn495627.aspx).</span></span>
<span data-ttu-id="11261-130">Následující metody, mají být přidán do **NotificationHub** v identifikátoru URI aktuální žádosti a přihlašovací údaje extrahovat z připojovacího řetězce na základě třídy k vytvoření tohoto tokenu.</span><span class="sxs-lookup"><span data-stu-id="11261-130">The following methods have to be added to the **NotificationHub** class to create the token based on the URI of the current request and the credentials extracted from the connection string.</span></span>

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

### <a name="send-a-notification-using-http-rest-api"></a><span data-ttu-id="11261-131">Odeslání oznámení pomocí rozhraní HTTP REST API</span><span class="sxs-lookup"><span data-stu-id="11261-131">Send a notification using HTTP REST API</span></span>
<span data-ttu-id="11261-132">První, umožňují použít definice třídy představující oznámení.</span><span class="sxs-lookup"><span data-stu-id="11261-132">First, let use define a class representing a notification.</span></span>

    class Notification:
        def __init__(self, notification_format=None, payload=None, debug=0):
            valid_formats = ['template', 'apple', 'gcm', 'windows', 'windowsphone', "adm", "baidu"]
            if not any(x in notification_format for x in valid_formats):
                raise Exception(
                    "Invalid Notification format. " +
                    "Must be one of the following - 'template', 'apple', 'gcm', 'windows', 'windowsphone', 'adm', 'baidu'")

            self.format = notification_format
            self.payload = payload

            # array with keynames for headers
            # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
            # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry
            # in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
            self.headers = None

<span data-ttu-id="11261-133">Tato třída je kontejner pro nativní oznámení textu nebo sadu vlastností v případě šablony oznámení, sadu hlaviček, který obsahuje formátu (nativní platforma nebo šablony) a vlastnosti specifické pro platformu (např. vlastnost Apple vypršení platnosti a hlavičky WNS).</span><span class="sxs-lookup"><span data-stu-id="11261-133">This class is a container for a native notification body or a set of properties in case of a template notification, a set of headers which contains format (native platform or template) and platform-specific properties (like Apple expiration property and WNS headers).</span></span>

<span data-ttu-id="11261-134">Naleznete [dokumentaci rozhraní API REST centra oznámení](http://msdn.microsoft.com/library/dn495827.aspx) a na konkrétní oznámení platformách formátů pro všechny možnosti, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="11261-134">Please refer to the [Notification Hubs REST APIs documentation](http://msdn.microsoft.com/library/dn495827.aspx) and the specific notification platforms' formats for all the options available.</span></span>

<span data-ttu-id="11261-135">Teď s touto třídou jsme může zapisovat odesílání oznámení metody uvnitř **NotificationHub** třídy.</span><span class="sxs-lookup"><span data-stu-id="11261-135">Now with this class, we can write the send notification methods inside of the **NotificationHub** class.</span></span>

    def make_http_request(self, url, payload, headers):
        parsed_url = urllib.parse.urlparse(url)
        connection = http.client.HTTPSConnection(parsed_url.hostname, parsed_url.port)

        if self.Debug > 0:
            connection.set_debuglevel(self.Debug)
            # adding this querystring parameter gets detailed information about the PNS send notification outcome
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

        # add the tags/tag expressions to the headers collection
        if tag_list != "":
            headers.update({'ServiceBusNotification-Tags': tag_list})

        # add any custom headers to the headers collection that the user may have added
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

<span data-ttu-id="11261-136">Výše uvedené metody odeslat požadavek HTTP POST koncovému bodu /messages centra oznámení, s správné textu a hlavičky k odesílání oznámení.</span><span class="sxs-lookup"><span data-stu-id="11261-136">The above methods send an HTTP POST request to the /messages endpoint of your notification hub, with the correct body and headers to send the notification.</span></span>

### <a name="using-debug-property-to-enable-detailed-logging"></a><span data-ttu-id="11261-137">Pomocí vlastnosti ladění povolit podrobné protokolování</span><span class="sxs-lookup"><span data-stu-id="11261-137">Using debug property to enable detailed logging</span></span>
<span data-ttu-id="11261-138">Povolení ladění vlastnost při inicializaci centra oznámení se zapsat podrobné protokolování informace o požadavku HTTP a odpovědi výpisu, jakož i podrobné zprávy oznámení odeslat výsledek.</span><span class="sxs-lookup"><span data-stu-id="11261-138">Enabling debug property while initializing the Notification Hub will write out detailed logging information about the HTTP request and response dump as well as detailed Notification message send outcome.</span></span> <span data-ttu-id="11261-139">Nedávno jsme přidali tato vlastnost s názvem [TestSend centra oznámení vlastnost](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) která vrací podrobné informace o výsledek odeslání oznámení.</span><span class="sxs-lookup"><span data-stu-id="11261-139">We recently added this property called [Notification Hubs TestSend property](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) which returns detailed information about the notification send outcome.</span></span> <span data-ttu-id="11261-140">Pro použití - inicializujte pomocí tohoto vzorce:</span><span class="sxs-lookup"><span data-stu-id="11261-140">To use it - initialize using the following:</span></span>

    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

<span data-ttu-id="11261-141">Žádost o odeslání oznámení centra adresy URL protokolu HTTP získá spolu s "test" querystring v důsledku.</span><span class="sxs-lookup"><span data-stu-id="11261-141">The Notification Hub Send request HTTP URL gets appended with a "test" querystring as a result.</span></span> 

## <span data-ttu-id="11261-142"><a name="complete-tutorial"></a>Dokončení tohoto kurzu</span><span class="sxs-lookup"><span data-stu-id="11261-142"><a name="complete-tutorial"></a>Complete the tutorial</span></span>
<span data-ttu-id="11261-143">Nyní můžete dokončit kurz Začínáme zasláním oznámení z back-end Python.</span><span class="sxs-lookup"><span data-stu-id="11261-143">Now you can complete the Get Started tutorial by sending the notification from a Python back-end.</span></span>

<span data-ttu-id="11261-144">Inicializace vašeho centra oznámení klienta (nahraďte název připojovacího řetězce a centra podle pokynů v [kurzu Začínáme Get]):</span><span class="sxs-lookup"><span data-stu-id="11261-144">Initialize your Notification Hubs client (substitute the connection string and hub name as instructed in the [Get started tutorial]):</span></span>

    hub = NotificationHub("myConnectionString", "myNotificationHubName")

<span data-ttu-id="11261-145">Pak přidejte kód odeslat v závislosti na svou cílovou platformu mobilních.</span><span class="sxs-lookup"><span data-stu-id="11261-145">Then add the send code depending on your target mobile platform.</span></span> <span data-ttu-id="11261-146">Tato ukázka přidá také vyšší úrovně metody k povolení odesílání oznámení založené na platformě například send_windows_notification pro systém windows. send_apple_notification (pro apple) atd.</span><span class="sxs-lookup"><span data-stu-id="11261-146">This sample also adds higher level methods to enable sending notifications based on the platform e.g. send_windows_notification for windows; send_apple_notification (for apple) etc.</span></span> 

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a><span data-ttu-id="11261-147">Windows Store a Windows Phone 8.1 (bez Silverlight)</span><span class="sxs-lookup"><span data-stu-id="11261-147">Windows Store and Windows Phone 8.1 (non-Silverlight)</span></span>
    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

### <a name="windows-phone-80-and-81-silverlight"></a><span data-ttu-id="11261-148">Windows Phone 8.0 a 8.1 Silverlight</span><span class="sxs-lookup"><span data-stu-id="11261-148">Windows Phone 8.0 and 8.1 Silverlight</span></span>
    hub.send_mpns_notification(toast)

### <a name="ios"></a><span data-ttu-id="11261-149">iOS</span><span class="sxs-lookup"><span data-stu-id="11261-149">iOS</span></span>
    alert_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_apple_notification(alert_payload)

### <a name="android"></a><span data-ttu-id="11261-150">Android</span><span class="sxs-lookup"><span data-stu-id="11261-150">Android</span></span>
    gcm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_gcm_notification(gcm_payload)

### <a name="kindle-fire"></a><span data-ttu-id="11261-151">Kindle Fire</span><span class="sxs-lookup"><span data-stu-id="11261-151">Kindle Fire</span></span>
    adm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_adm_notification(adm_payload)

### <a name="baidu"></a><span data-ttu-id="11261-152">Baidu</span><span class="sxs-lookup"><span data-stu-id="11261-152">Baidu</span></span>
    baidu_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_baidu_notification(baidu_payload)

<span data-ttu-id="11261-153">Spuštěním kódu Python by měl vytvořit oznámení, které jsou na cílovém zařízení.</span><span class="sxs-lookup"><span data-stu-id="11261-153">Running your Python code should produce a notification appearing on your target device.</span></span>

## <a name="examples"></a><span data-ttu-id="11261-154">Příklady:</span><span class="sxs-lookup"><span data-stu-id="11261-154">Examples:</span></span>
### <a name="enabling-debug-property"></a><span data-ttu-id="11261-155">Povolení ladění vlastnost</span><span class="sxs-lookup"><span data-stu-id="11261-155">Enabling debug property</span></span>
<span data-ttu-id="11261-156">Když povolíte příznak ladění při inicializaci NotificationHub, zobrazí se podrobné požadavku HTTP a odpovědi výpisu, jakož i NotificationOutcome takto kde pochopit, jaké hlavičky protokolu HTTP se předávají v požadavku a jaké odpověď HTTP byla přijata z centra oznámení:![][1]</span><span class="sxs-lookup"><span data-stu-id="11261-156">When you enable debug flag while initializing the NotificationHub then you will see detailed HTTP request and response dump as well as NotificationOutcome like the following where you can understand what HTTP headers are passed in the request and what HTTP response was received from the Notification Hub: ![][1]</span></span>

<span data-ttu-id="11261-157">Uvidíte, například podrobný výsledek centra oznámení</span><span class="sxs-lookup"><span data-stu-id="11261-157">You will see detailed Notification Hub result e.g.</span></span> 

* <span data-ttu-id="11261-158">Když zprávu úspěšně odeslat na službu nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="11261-158">when the message is successfully sent to the Push Notification Service.</span></span> 
  
        <Outcome>The Notification was successfully sent to the Push Notification System</Outcome>
* <span data-ttu-id="11261-159">Pokud neexistují žádné cíle najít pro všechny nabízené oznámení potom pravděpodobně chcete vidět následující informace v odpovědi (což znamená, že neexistují žádné registrace nalezen pravděpodobně doručit oznámení, protože registrace měl některé neodpovídající značky)</span><span class="sxs-lookup"><span data-stu-id="11261-159">If there were no targets found for any push notification then you are likely going to see the following in the response (which indicates that there were no registrations found to deliver the notification probably because the registrations had some mismatched tags)</span></span>
  
        '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'

### <a name="broadcast-toast-notification-to-windows"></a><span data-ttu-id="11261-160">Vysílání informační zpráva do systému Windows</span><span class="sxs-lookup"><span data-stu-id="11261-160">Broadcast toast notification to Windows</span></span>
<span data-ttu-id="11261-161">Všimněte si seznam hlaviček, které získat odeslaná při odesílání oznámení s informační zprávou všesměrového vysílání pro klienta systému Windows.</span><span class="sxs-lookup"><span data-stu-id="11261-161">Notice the headers that get sent out when you are sending a broadcast toast notification to Windows client.</span></span> 

    hub.send_windows_notification(wns_payload)

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a><span data-ttu-id="11261-162">Odeslat oznámení o určení značky (nebo výraz označení)</span><span class="sxs-lookup"><span data-stu-id="11261-162">Send notification specifying a tag (or tag expression)</span></span>
<span data-ttu-id="11261-163">Všimněte si hlavičku HTTP značky, který získá přidán do požadavku HTTP (v následujícím příkladu jsme odesílání oznámení pouze na registrací s odebranou datovou částí "sports")</span><span class="sxs-lookup"><span data-stu-id="11261-163">Notice the Tags HTTP header which gets added to the HTTP request (in the example below, we are sending the notification only to registrations with 'sports' payload)</span></span>

    hub.send_windows_notification(wns_payload, "sports")

![][3]

### <a name="send-notification-specifying-multiple-tags"></a><span data-ttu-id="11261-164">Odeslat oznámení o určení více značek</span><span class="sxs-lookup"><span data-stu-id="11261-164">Send notification specifying multiple tags</span></span>
<span data-ttu-id="11261-165">Všimněte si, jak hlavičku HTTP značky změní, když jsou odesílány více značek.</span><span class="sxs-lookup"><span data-stu-id="11261-165">Notice how the Tags HTTP header changes when multiple tags are sent.</span></span> 

    tags = {'sports', 'politics'}
    hub.send_windows_notification(wns_payload, tags)

![][4]

### <a name="templated-notification"></a><span data-ttu-id="11261-166">Použitím šablon oznámení</span><span class="sxs-lookup"><span data-stu-id="11261-166">Templated notification</span></span>
<span data-ttu-id="11261-167">Všimněte si, že změní hlavičku formátu HTTP a text datové části je odeslána jako část obsahu žádosti HTTP:</span><span class="sxs-lookup"><span data-stu-id="11261-167">Notice that the Format HTTP header changes and the payload body is sent as part of the HTTP request body:</span></span>

<span data-ttu-id="11261-168">**Na straně klienta - registrované šablony**</span><span class="sxs-lookup"><span data-stu-id="11261-168">**Client side - registered template**</span></span>

        var template =
                        @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";

<span data-ttu-id="11261-169">**Na straně serveru – odesílání datové části**</span><span class="sxs-lookup"><span data-stu-id="11261-169">**Server side - sending the payload**</span></span>

        template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
        hub.send_template_notification(template_payload)

![][5]

## <a name="next-steps"></a><span data-ttu-id="11261-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="11261-170">Next Steps</span></span>
<span data-ttu-id="11261-171">V tomto tématu jsme vám ukázal, jak vytvořit jednoduché klienta Python REST pro centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="11261-171">In this topic we showed how to create a simple Python REST client for Notification Hubs.</span></span> <span data-ttu-id="11261-172">Odsud můžete:</span><span class="sxs-lookup"><span data-stu-id="11261-172">From here you can:</span></span>

* <span data-ttu-id="11261-173">Stáhnout kompletní [Python REST obálku ukázka], která obsahuje všechny výše uvedený kód.</span><span class="sxs-lookup"><span data-stu-id="11261-173">Download the full [Python REST wrapper sample], which contains all the code above.</span></span>
* <span data-ttu-id="11261-174">Pokračujte ve čtení o označování funkce v centrech oznámení [novinkách kurzu]</span><span class="sxs-lookup"><span data-stu-id="11261-174">Continue learning about Notification Hubs tagging feature in the [Breaking News tutorial]</span></span>
* <span data-ttu-id="11261-175">Pokračujte ve čtení o funkce šablon centra oznámení v [kurzu lokalizace zprávy]</span><span class="sxs-lookup"><span data-stu-id="11261-175">Continue learning about Notification Hubs Templates feature in the [Localizing News tutorial]</span></span>

<!-- URLs -->
<span data-ttu-id="11261-176">[Python REST obálku ukázka]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python</span><span class="sxs-lookup"><span data-stu-id="11261-176">[Python REST wrapper sample]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python</span></span>
<span data-ttu-id="11261-177">[kurzu Začínáme Get]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/</span><span class="sxs-lookup"><span data-stu-id="11261-177">[Get started tutorial]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/</span></span>
<span data-ttu-id="11261-178">[novinkách kurzu]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/</span><span class="sxs-lookup"><span data-stu-id="11261-178">[Breaking News tutorial]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/</span></span>
<span data-ttu-id="11261-179">[kurzu lokalizace zprávy]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/</span><span class="sxs-lookup"><span data-stu-id="11261-179">[Localizing News tutorial]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/</span></span>

<!-- Images. -->
[1]: ./media/notification-hubs-python-backend-how-to/DetailedLoggingInfo.png
[2]: ./media/notification-hubs-python-backend-how-to/BroadcastScenario.png
[3]: ./media/notification-hubs-python-backend-how-to/SendWithOneTag.png
[4]: ./media/notification-hubs-python-backend-how-to/SendWithMultipleTags.png
[5]: ./media/notification-hubs-python-backend-how-to/TemplatedNotification.png

