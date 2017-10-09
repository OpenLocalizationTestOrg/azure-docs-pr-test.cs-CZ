---
title: "ověřování služby Service Bus aaaAzure s podpisy sdíleného přístupu | Microsoft Docs"
description: "Přehled ověřování Service Bus pomocí sdílené přístupové podpisy přehled, podrobnosti o ověřování SAS s Azure Service Bus."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: 773bb11720384d7245820b56dc25b8e064ffa746
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-authentication-with-shared-access-signatures"></a>Ověřování služby Service Bus s podpisy sdíleného přístupu

*Sdílené přístupové podpisy* (SAS) jsou hello primární zabezpečení mechanismus pro zasílání zpráv Service Bus. Tento článek popisuje SAS, jak fungují a jak toouse je způsobem, bez ohledu na platformu.

Ověření SAS umožňuje aplikacím tooauthenticate tooService sběrnice pomocí přístupový klíč nakonfigurované na oboru názvů hello nebo na hello zasílání zpráv entity (fronta nebo téma) pomocí konkrétní práva, která jsou přidružená. Pak můžete použít tento klíč toogenerate token SAS, který můžou klienti zase použít tooauthenticate tooService sběrnice.

Podpora ověřování SAS je součástí hello Azure SDK verze 2.0 nebo novější.

## <a name="overview-of-sas"></a>Přehled SAS

Sdílené přístupové podpisy jsou mechanismus ověřování na základě zabezpečeného hodnoty hash SHA-256 nebo identifikátory URI. Přidružení zabezpečení je velmi výkonný mechanismus, který je používán všechny služby Service Bus. Při skutečném použití SAS má dvě součásti: *sdílené zásady přístupu* a *sdílený přístupový podpis* (často říká *tokenu*).

Ověřování SAS v Service Bus zahrnuje konfiguraci hello kryptografického klíče s přidružená práva na prostředku služby Service Bus. Prostředky sběrnice klienti deklarace identity přístup tooService prezentací SAS token. Tento token se skládá z identifikátoru URI přistupuje hello prostředku a vypršela platnost podepsané hello nakonfigurovaný klíč.

Podpis sdíleného přístupu autorizační pravidla můžete konfigurovat v Service Bus [předává](service-bus-fundamentals-hybrid-solutions.md#relays), [fronty](service-bus-fundamentals-hybrid-solutions.md#queues), a [témata](service-bus-fundamentals-hybrid-solutions.md#topics).

SAS ověřování používá hello následující prvky:

* [Sdílený přístup autorizační pravidlo](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule): volitelný sekundární klíč, název klíče a přidružená práva A 256 bitů primární kryptografický klíč ve formátu Base64 reprezentace (kolekce *naslouchání*, *odeslat*, nebo *spravovat* oprávnění).
* [Sdílený přístupový podpis](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) tokenu: vygenerování pomocí hello HMAC SHA256 prostředků řetězce, který se skládá z hello URI hello prostředku, který je přístupný a vypršela platnost, se hello kryptografickým klíčem. podpis Hello a další prvky popsané v následující části hello jsou formátovány do tokenu SAS text řetězec tooform hello.

## <a name="shared-access-policy"></a>Zásada sdíleného přístupu

Důležité toounderstand o tokenu SAS se začne zásadám. Pro každou zásadu rozhodnete na tři údaje: **název**, **oboru**, a **oprávnění**. Hello **název** je právě toto; jedinečný název v rámci tohoto oboru. Hello obor je snadné dostatečně: jeho hello identifikátor URI prostředku hello nejistá. V případě oboru názvů Service Bus hello obor je hello plně kvalifikovaný název domény (FQDN), jako například `https://<yournamespace>.servicebus.windows.net/`.

Hello dostupná oprávnění pro zásady jsou z velké části není potřeba vysvětlovat:

* Odeslat
* Naslouchání
* Spravovat

Po vytvoření zásady hello je přiřazen *primární klíč* a *sekundární klíč*. Toto jsou kryptograficky silnou klíče. Nemusíte je ztratí nebo je úniku – vždy budete mít k dispozici v hello [portál Azure][Azure portal]. Můžete použít buď hello generované klíčů a jejich lze vygenerovat kdykoli. Ale pokud chcete znovu vygenerovat nebo změnit hello primární klíč v zásadách hello, budou všechny sdílené přístupové podpisy na ní neplatné.

Když vytvoříte obor názvů sběrnice, zásady se automaticky vytvoří pro hello celý obor názvů s názvem **RootManageSharedAccessKey**, a tato zásada nemá všechna oprávnění. Nemáte přihlásíte jako **kořenové**, takže nemusíte tuto zásadu použít, pokud dobře důvody. Můžete vytvořit další zásady hello **konfigurace** kartě pro obor názvů hello hello portálu. Je důležité toonote, který úrovně jediného stromu v Service Bus (obor názvů, fronty atd.) může mít pouze až tooit too12 zásady připojené.

## <a name="configuration-for-shared-access-signature-authentication"></a>Konfigurace pro ověřování sdíleného přístupového podpisu
Můžete nakonfigurovat hello [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) pravidlo na obory názvů Service Bus, fronty a témata. Konfigurace [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) na Service Bus se aktuálně nepodporuje předplatné, ale můžete použít pravidla nakonfigurované na oboru názvů nebo téma toosubscriptions toosecure přístup. Pracovní vzorku, který znázorňuje tento postup, najdete v části hello [pomocí sdíleného přístupového podpisu (SAS) ověřování pomocí předplatných Service Bus](http://code.msdn.microsoft.com/Using-Shared-Access-e605b37c) ukázka.

Nesmí být delší než 12 tato pravidla lze konfigurovat v oboru názvů Service Bus, fronta nebo téma. Pravidla, které jsou nakonfigurované na oboru názvů Service Bus platit tooall entity v daném oboru názvů.

![SAS](./media/service-bus-sas/service-bus-namespace.png)

Na tomto obrázku hello *manageRuleNS*, *sendRuleNS*, a *listenRuleNS* autorizační pravidla použít tooboth fronta F1 a téma T1, při *listenRuleQ*  a *sendRuleQ* použít pouze tooqueue Otázka č. 1 a *sendRuleT* platí pouze tootopic T1.

Hello klíče parametry [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) jsou následující:

| Parametr | Popis |
| --- | --- |
| *KeyName* |Řetězec, který popisuje hello autorizační pravidlo. |
| *PrimaryKey* |Kódováním base64 256 bitů primární klíč pro přihlašování a ověřování tokenu SAS hello. |
| *Sekundární klíč* |Kódováním base64 256 bitů sekundární klíč pro přihlašování a ověřování tokenu SAS hello. |
| *AccessRights* |Seznam přístupová práva udělují hello autorizační pravidlo. Tato práva, může být jakékoli kolekce naslouchání, odeslání a Správa práv. |

Při zřízení oboru názvů Service Bus [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule), s [KeyName](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_KeyName) nastavit příliš**RootManageSharedAccessKey**, se vytvoří ve výchozím nastavení.

## <a name="generate-a-shared-access-signature-token"></a>Vygenerování sdíleného přístupového podpisu (token)

samotnými zásadami Hello není hello přístupový token pro Service Bus. Je hello objekt, ze které hello se generuje přístupový token - pomocí buď hello primární nebo sekundární klíč. Libovolného klienta, který má přístup toohello podpisový klíč zadaný v autorizační pravidlo hello sdíleného přístupu můžete vygenerovat hello SAS token. Hello token je generován tím, že pečlivě vytvoří řetězec ve formátu hello:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

Kde `signature-string` je hodnota hash SHA-256 hello hello oboru hello tokenu (**oboru** jak je popsáno v předchozí části hello) s Line FEED připojí a čas vypršení platnosti (v sekundách od hello epoch: `00:00:00 UTC` na 1. ledna pod hodnotou 1970). 

> [!NOTE]
> tooavoid čas krátké vypršení platnosti tokenu, je doporučeno zakódovat hodnota času vypršení platnosti hello jako celé číslo bez znaménka alespoň 32-bit, nebo pokud možno celé číslo dlouho (64 bitů).  
> 
> 

Hodnota hash Hello vypadá podobně jako toohello následující pseudo kód a vrátí 32 bajtů.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

Hello-rozdělí hodnoty jsou v hello **SharedAccessSignature** řetězec tak, aby hello příjemce můžete vypočítat hodnotu hash hello s hello stejnými parametry, toobe se, že vrací hello stejného výsledku. Hello URI Určuje obor hello a název klíče hello identifikuje hello zásad toobe používá toocompute hello hash. To je důležité z hlediska zabezpečení. Pokud hello podpis neodpovídá vypočítá které hello příjemce (Service Bus), byl odepřen přístup. V tomto okamžiku může být tohoto odesílatele hello měl přístupový klíč toohello a musí být udělena práva hello určená v zásadách hello.

Všimněte si, že mají používat hello kódovaný identifikátor URI pro tuto operaci. identifikátor URI prostředku Hello je hello je požadována úplný identifikátor URI hello přístup toowhich prostředků služby Service Bus. Například `http://<namespace>.servicebus.windows.net/<entityPath>` nebo `sb://<namespace>.servicebus.windows.net/<entityPath>`, tj `http://contoso.servicebus.windows.net/contosoTopics/T1/Subscriptions/S3`.

Hello sdíleného přístupu autorizační pravidlo se používá k podepisování musí být nakonfigurované na entitu hello zadanou tento identifikátor URI, nebo pomocí jedné z jejích hierarchické nadřazených tříd. Například `http://contoso.servicebus.windows.net/contosoTopics/T1` nebo `http://contoso.servicebus.windows.net` v předchozím příkladu hello.

SAS token je platná pro všechny prostředky pod hello `<resourceURI>` použít v hello `signature-string`.

Hello [KeyName](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_KeyName) v hello SAS token odkazuje toohello **keyName** hello sdíleného přístupu autorizační pravidla používají toogenerate hello token.

Hello *URL kódovaný resourceURI* musí být hello stejný jako identifikátor URI použitý v hello řetězec přihlášení během hello výpočtu podpisu hello hello. Měla by být [kódováním procent](https://msdn.microsoft.com/library/4fkewx0t.aspx).

Doporučujeme pravidelně obnovovat hello klíče v hello [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) objektu. Aplikace by měly používat obecně hello [PrimaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_PrimaryKey) toogenerate SAS token. Při opakovaném generování klíče hello, měli byste nahradit hello [sekundární klíč](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_SecondaryKey) s hello původního primárního klíče a vygenerovat nový klíč jako hello nový primární klíč. To vám umožní toocontinue pomocí tokeny k ověřování, které byly vydány s hello starý primární klíč a ještě, nevypršela jejich platnost.

Pokud dojde k narušení klíč a budete mít toorevoke hello klíčů, můžete obnovit obou hello [PrimaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_PrimaryKey) a hello [sekundární klíč](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_SecondaryKey) z [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule), nahraďte je s novými klíči. Tento postup zruší platnost všech tokenů podepsané hello staré klíče.

## <a name="how-toouse-shared-access-signature-authentication-with-service-bus"></a>Jak toouse sdíleného přístupového podpisu ověřování službou Service Bus

Hello následující scénáře patří konfigurace autorizačních pravidel, generování tokenů SAS a autorizací klienta.

Pro úplnou pracovní ukázkové aplikace Service Bus, která znázorňuje hello konfiguraci a použití SAS autorizace, najdete v části [sdíleného přístupového podpisu ověřování službou Service Bus](http://code.msdn.microsoft.com/Shared-Access-Signature-0a88adf8). Související příklad, který znázorňuje použití hello SAS autorizačních pravidel, která je nakonfigurovaná na obory názvů nebo témata sběrnice odběrů toosecure je k dispozici zde: [pomocí sdíleného přístupového podpisu (SAS) ověřování pomocí předplatných Service Bus ](http://code.msdn.microsoft.com/Using-Shared-Access-e605b37c).

## <a name="access-shared-access-authorization-rules-on-a-namespace"></a>Pravidla přístupu k autorizaci sdíleného přístupu u oboru názvů

Operace na kořen oboru názvů Service Bus hello vyžadují ověřování pomocí certifikátu. Je potřeba načíst certifikát pro správu pro vaše předplatné Azure. tooupload certifikát pro správu, postupujte podle kroků hello [sem](../cloud-services/cloud-services-configure-ssl-certificate-portal.md#step-3-upload-a-certificate), pomocí hello [portál Azure][Azure portal]. Další informace o certifikátech správy Azure najdete v tématu hello [přehled Azure certifikátů](../cloud-services/cloud-services-certs-create.md#what-are-management-certificates).

Hello koncový bod pro přístup ke sdíleným přístupem autorizační pravidla na obor názvů sběrnice je následující:

```http
https://management.core.windows.net/{subscriptionId}/services/ServiceBus/namespaces/{namespace}/AuthorizationRules/
```

toocreate [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) objekt v oboru názvů Service Bus, provést operaci POST na tento koncový bod s informace o pravidle hello serializovanou jako XML nebo JSON. Například:

```csharp
// Base address for accessing authorization rules on a namespace
string baseAddress = @"https://management.core.windows.net/<subscriptionId>/services/ServiceBus/namespaces/<namespace>/AuthorizationRules/";

// Configure authorization rule with base64-encoded 256-bit key and Send rights
var sendRule = new SharedAccessAuthorizationRule("contosoSendAll",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Send });

// Operations on hello Service Bus namespace root require certificate authentication.
WebRequestHandler handler = new WebRequestHandler
{
    ClientCertificateOptions = ClientCertificateOption.Manual
};
// Access hello management certificate by subject name
handler.ClientCertificates.Add(GetCertificate(<certificateSN>));

HttpClient httpClient = new HttpClient(handler)
{
    BaseAddress = new Uri(baseAddress)
};
httpClient.DefaultRequestHeaders.Accept.Add(
    new MediaTypeWithQualityHeaderValue("application/json"));
httpClient.DefaultRequestHeaders.Add("x-ms-version", "2015-01-01");

// Execute a POST operation on hello baseAddress above toocreate an auth rule
var postResult = httpClient.PostAsJsonAsync("", sendRule).Result;
```

Podobně použijte operaci GET na hello koncový bod tooread hello autorizační pravidla nakonfigurované na oboru názvů hello.

tooupdate nebo odstranění konkrétní ověřovací pravidlo, použijte následující koncový bod hello:

```http
https://management.core.windows.net/{subscriptionId}/services/ServiceBus/namespaces/{namespace}/AuthorizationRules/{KeyName}
```

## <a name="access-shared-access-authorization-rules-on-an-entity"></a>Přístup sdíleného přístupu autorizační pravidla s entitou

Dostanete [Microsoft.ServiceBus.Messaging.SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) objekt nakonfigurované na fronty sběrnice nebo téma prostřednictvím hello [AuthorizationRules](/dotnet/api/microsoft.servicebus.messaging.authorizationrules) kolekce v hello odpovídající [QueueDescription](/dotnet/api/microsoft.servicebus.messaging.queuedescription) nebo [TopicDescription](/dotnet/api/microsoft.servicebus.messaging.topicdescription).

Hello následující kód ukazuje, jak tooadd autorizační pravidla pro frontu.

```csharp
// Create an instance of NamespaceManager for hello operation
NamespaceManager nsm = NamespaceManager.CreateFromConnectionString(
    <connectionString> );
QueueDescription qd = new QueueDescription( <qPath> );

// Create a rule with send rights with keyName as "contosoQSendKey"
// and add it toohello queue description.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoSendKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Send }));

// Create a rule with listen rights with keyName as "contosoQListenKey"
// and add it toohello queue description.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoQListenKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Listen }));

// Create a rule with manage rights with keyName as "contosoQManageKey"
// and add it toohello queue description.
// A rule with manage rights must also have send and receive rights.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoQManageKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] {AccessRights.Manage, AccessRights.Listen, AccessRights.Send }));

// Create hello queue.
nsm.CreateQueue(qd);
```

## <a name="use-shared-access-signature-authorization"></a>Použití autorizačních sdíleného přístupového podpisu

Aplikace pomocí knihovny Service Bus .NET hello hello .NET SDK služby Azure můžete použít autorizace SAS prostřednictvím hello [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) třídy. Hello následující kód ukazuje použití hello hello zprostředkovatele tokenu toosend zprávy tooa fronty Service Bus.

```csharp
Uri runtimeUri = ServiceBusEnvironment.CreateServiceUri("sb",
    <yourServiceNamespace>, string.Empty);
MessagingFactory mf = MessagingFactory.Create(runtimeUri,
    TokenProvider.CreateSharedAccessSignatureTokenProvider(keyName, key));
QueueClient sendClient = mf.CreateQueueClient(qPath);

//Sending hello message tooqueue.
BrokeredMessage helloMessage = new BrokeredMessage("Hello, Service Bus!");
helloMessage.MessageId = "SAS-Sample-Message";
sendClient.Send(helloMessage);
```

Aplikace můžete také použít SAS pro ověřování pomocí SAS připojovací řetězec v metodách, které přijímají připojovací řetězce.

Všimněte si, že autorizace SAS toouse s předávací služby Service Bus, můžete použít klíče SAS, které jsou nakonfigurované na oboru názvů Service Bus hello. Pokud vytvoříte relé explicitně na obor názvů hello ([NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) s [RelayDescription](/dotnet/api/microsoft.servicebus.messaging.relaydescription)) objektu, budete moct nastavit pravidla SAS hello jen pro tento předávání. toouse autorizace SAS s odběry služby Service Bus, můžete nakonfigurovat na obor názvů sběrnice nebo téma klíče SAS.

## <a name="use-hello-shared-access-signature-at-http-level"></a>Použití hello sdílený přístupový podpis (na úrovni protokolu HTTP)

Teď, když víte, jak toocreate podpisy sdíleného přístupu pro všechny entity v Service Bus, jste připraveni tooperform HTTP POST:

```http
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
``` 

Pamatujte si, že se tento postup funguje pro vše. Můžete vytvořit SAS pro fronty, tématu nebo předplatného. 

Pokud dáváte odesílatele nebo klienta SAS token, nemají hello klíč přímo a jejich nelze vrátit hello hash tooobtain ho. Jako takový budete mít kontrolu nad co přístupem a jak dlouho. Tooremember důležité je, pokud změníte hello primární klíč v zásadách hello, bude zrušena žádné sdílené přístupové podpisy vytvořit z něj.

## <a name="use-hello-shared-access-signature-at-amqp-level"></a>Použití hello sdílený přístupový podpis (na úrovni AMQP)

V předchozí části hello jste viděli, jak toouse hello token SAS s požadavek HTTP POST pro odesílání dat toohello Service Bus. Jak už víte, můžete přístup k Service Bus pomocí hello Advanced zpráv služby Řízení front protokol (AMQP), je hello preferované protokol toouse z důvodů výkonu v řadě scénářů. Hello využití tokenu SAS s AMQP je popsané v dokumentu hello [AMQP Claim-Based zabezpečení verze 1.0](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc) který je v pracovní koncept od 2013 ale dobře nepodporuje Azure ještě dnes.

Před zahájením toosend data tooService sběrnice, musíte odeslat hello vydavatel tokenu SAS hello uvnitř do AMQP zpráva tooa dobře definovaný AMQP uzlu s názvem **$cbs** (můžete ho zobrazovat jako "speciální" fronty používané hello služby tooacquire a ověřit, zda všechny Hello tokeny SAS). Vydavatel Hello musíte zadat hello **ReplyTo** pole uvnitř uvítací zprávu AMQP; Toto je uzel hello, ve které hello služby reaguje toohello vydavatele s hello výsledek ověření tokenu hello (jednoduché požadavek nebo odpověď vzor mezi Vydavatel a služby). Tento uzel odpověď se vytvoří "na hello chodu," o "dynamické vytvoření vzdáleném uzlu" a Mluvte podle specifikace hello protokolu AMQP 1.0. Po zkontrolování, že tento hello SAS token je platný, můžete hello vydavatele vpřed a toosend data toohello službu spustit.

Hello následující kroky ukazují, jak toosend hello tokenu SAS s použitím hello protokolu AMQP [AMQP.Net Lite](https://github.com/Azure/amqpnetlite) knihovny. To je užitečné, pokud nemůžete použít hello oficiální Service Bus SDK (například na WinRT, rozhraní .net Compact Framework, rozhraní .net Micro Framework a Mono) vývoj v jazyce C\#. Tato knihovna je samozřejmě užitečné toohelp pochopit, jak založené na deklaracích identity zabezpečení funguje na úrovni hello AMQP, jako jste viděli, jak to funguje na úrovni hello HTTP (s HTTP POST požadavku a hello SAS token odeslaný uvnitř hello "" hlavičku ověřování). Pokud nepotřebujete takové hluboké znalosti o AMQP, můžete použít hello oficiální Service Bus SDK s rozhraní .net Framework aplikace, které bude to pro vás.

### <a name="c35"></a>C&#35;

```csharp
/// <summary>
/// Send claim-based security (CBS) token
/// </summary>
/// <param name="shareAccessSignature">Shared access signature (token) toosend</param>
private bool PutCbsToken(Connection connection, string sasToken)
{
    bool result = true;
    Session session = new Session(connection);

    string cbsClientAddress = "cbs-client-reply-to";
    var cbsSender = new SenderLink(session, "cbs-sender", "$cbs");
    var cbsReceiver = new ReceiverLink(session, cbsClientAddress, "$cbs");

    // construct hello put-token message
    var request = new Message(sasToken);
    request.Properties = new Properties();
    request.Properties.MessageId = Guid.NewGuid().ToString();
    request.Properties.ReplyTo = cbsClientAddress;
    request.ApplicationProperties = new ApplicationProperties();
    request.ApplicationProperties["operation"] = "put-token";
    request.ApplicationProperties["type"] = "servicebus.windows.net:sastoken";
    request.ApplicationProperties["name"] = Fx.Format("amqp://{0}/{1}", sbNamespace, entity);
    cbsSender.Send(request);

    // receive hello response
    var response = cbsReceiver.Receive();
    if (response == null || response.Properties == null || response.ApplicationProperties == null)
    {
        result = false;
    }
    else
    {
        int statusCode = (int)response.ApplicationProperties["status-code"];
        if (statusCode != (int)HttpStatusCode.Accepted && statusCode != (int)HttpStatusCode.OK)
        {
            result = false;
        }
    }

    // hello sender/receiver may be kept open for refreshing tokens
    cbsSender.Close();
    cbsReceiver.Close();
    session.Close();

    return result;
}
```

Hello `PutCbsToken()` metoda přijímá hello *připojení* (instance třídy připojení AMQP jako poskytované hello [AMQP .NET Lite knihovny](https://github.com/Azure/amqpnetlite)) představující služby toohello připojení TCP hello a hello *sasToken* parametr, který je toosend tokenu SAS hello. 

> [!NOTE]
> Je důležité, aby hello připojení je vytvořena s **SASL ověřovací mechanismus nastavit tooEXTERNAL** (a ne hello prostý výchozí uživatelské jméno a heslo použít, pokud nepotřebujete toosend hello SAS token).
> 
> 

V dalším kroku hello vydavatele vytvoří dva odkazy AMQP pro hello SAS token odesílání a příjmu odpovědi hello (výsledek ověření tokenu hello) ze služby hello.

Hello AMQP zpráva obsahuje sadu vlastností a další informace než jednoduché zprávy. Hello SAS token je hello těla zprávy hello (pomocí jeho konstruktoru). Hello **"ReplyTo"** vlastnost je nastavena na název uzlu toohello pro příjem výsledek ověření hello na odkaz hello příjemce (pokud, můžete změnit její název chcete a bude nutné vytvořit dynamicky službou hello). Hello poslední tři aplikace nebo vlastní vlastnosti jsou používány hello služby tooindicate jaké operace má tooexecute. Jak je popsáno ve hello CBS návrhu specifikace, musí být hello **název operace** hello ("put-token"), **typ tokenu** (v tomto případě "servicebus.windows.net:sastoken") a hello **" název"cílová skupina hello** toowhich hello token platí (hello celý entity).

Po odeslání tokenu SAS hello na odkaz hello odesílatele, musí hello vydavatele čtení hello odpovědět na odkaz hello příjemce. odpověď Hello je jednoduchý AMQP zprávu s vlastností aplikace s názvem **"kód stavu"** obsahující hello stejné hodnoty jako stavový kód HTTP.

## <a name="rights-required-for-service-bus-operations"></a>Práva potřebná pro operace služby Service Bus

Hello následující tabulka uvádí hello přístupová práva potřebná pro různé operace na prostředcích služby Service Bus.

| Operace | Požadované deklarace identity | Deklarace oboru |
| --- | --- | --- |
| **Namespace** | | |
| Konfigurovat autorizační pravidlo na obor názvů |Spravovat |Každou adresu, obor názvů |
| **Služba registru** | | |
| Zobrazení výčtu privátní zásady |Spravovat |Každou adresu, obor názvů |
| Zahájit naslouchání na obor názvů |Naslouchání |Každou adresu, obor názvů |
| Posílat zprávy tooa naslouchání v oboru názvů |Odeslat |Každou adresu, obor názvů |
| **Fronty** | | |
| Vytvoření fronty |Spravovat |Každou adresu, obor názvů |
| Odstranění fronty |Spravovat |Každou adresu, platný fronty |
| Zobrazení výčtu fronty |Spravovat |$Prostředky nebo fronty |
| Získat hello popis fronty |Spravovat |Každou adresu, platný fronty |
| Konfigurovat autorizační pravidlo pro frontu |Spravovat |Každou adresu, platný fronty |
| Odeslat do fronty toohello |Odeslat |Každou adresu, platný fronty |
| Příjem zpráv z fronty |Naslouchání |Každou adresu, platný fronty |
| Zrušte nebo dokončení zprávy po přijetí uvítací zprávu v režimu zamknutí funkce Náhled |Naslouchání |Každou adresu, platný fronty |
| Odložení zprávu pro pozdější načtení |Naslouchání |Každou adresu, platný fronty |
| Tato zpráva |Naslouchání |Každou adresu, platný fronty |
| Zjištění stavu hello související s relací fronty zpráv |Naslouchání |Každou adresu, platný fronty |
| Nastavit stav hello související s relací fronty zpráv |Naslouchání |Každou adresu, platný fronty |
| **Téma** | | |
| Vytvoření tématu |Spravovat |Každou adresu, obor názvů |
| Odstranit téma |Spravovat |Každou adresu, platný tématu |
| Zobrazení výčtu témata |Spravovat |$Prostředky nebo témata |
| Získání popisu tématu hello |Spravovat |Každou adresu, platný tématu |
| Konfigurovat autorizační pravidlo pro téma |Spravovat |Každou adresu, platný tématu |
| Odeslat toohello tématu |Odeslat |Každou adresu, platný tématu |
| **Předplatné** | | |
| Vytvoření odběru |Spravovat |Každou adresu, obor názvů |
| Odstranit odběr |Spravovat |.. /myTopic/Subscriptions/mySubscription |
| Zobrazení výčtu odběrů |Spravovat |.. / myTopic/odběrů |
| Získat předplatné popis |Spravovat |.. /myTopic/Subscriptions/mySubscription |
| Zrušte nebo dokončení zprávy po přijetí uvítací zprávu v režimu zamknutí funkce Náhled |Naslouchání |.. /myTopic/Subscriptions/mySubscription |
| Odložení zprávu pro pozdější načtení |Naslouchání |.. /myTopic/Subscriptions/mySubscription |
| Tato zpráva |Naslouchání |.. /myTopic/Subscriptions/mySubscription |
| Zjištění stavu hello přidružené k tématu relace |Naslouchání |.. /myTopic/Subscriptions/mySubscription |
| Nastavit stav hello přidružené k tématu relace |Naslouchání |.. /myTopic/Subscriptions/mySubscription |
| **Pravidla** | | |
| Vytvoření pravidla |Spravovat |.. /myTopic/Subscriptions/mySubscription |
| Odstranění pravidla |Spravovat |.. /myTopic/Subscriptions/mySubscription |
| Zobrazení výčtu pravidel |Spravovat nebo naslouchání |.. /myTopic/Subscriptions/mySubscription/Rules 

## <a name="next-steps"></a>Další kroky

toolearn Další informace o zasílání zpráv Service Bus, najdete v následujících tématech hello.

* [Základy služby Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Fronty, témata a odběry služby Service Bus](service-bus-queues-topics-subscriptions.md)
* [Jak toouse fronty Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Jak toouse Service Bus témat a odběrů](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[Azure portal]: https://portal.azure.com