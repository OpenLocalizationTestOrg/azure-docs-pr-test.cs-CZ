---
title: "aaaUnderstand zabezpečení Azure IoT Hub | Microsoft Docs"
description: "Průvodce - vývojáře přístupu toocontrol tooIoT rozbočovače pro aplikace pro zařízení a back-end aplikace. Obsahuje informace o tokeny zabezpečení a podporu pro certifikáty X.509."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 45631e70-865b-4e06-bb1d-aae1175a52ba
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 717726328a6bb5c5c334a123d0abfed711b2c3b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="control-access-tooiot-hub"></a>Řízení přístupu tooIoT rozbočovače

Tento článek popisuje možnosti hello pro zabezpečení služby IoT hub. IoT Hub používá *oprávnění* toogrant koncový bod centra IoT přístup tooeach. Oprávnění omezit hello přístup tooan IoT hub závislosti na funkcích.

Tento článek popisuje:

* Hello jiná oprávnění, můžete udělit nebo back-end aplikačním tooaccess tooa zařízení služby IoT hub.
* Hello proces a hello tokeny ověřování používá tooverify oprávnění.
* Jak tooscope pověření toolimit přístup k toospecific prostředkům.
* Podpora služby IoT Hub pro certifikáty X.509.
* Mechanismy ověřování vlastní zařízení, které používají existující registrech identity zařízení nebo schémat ověřování.

### <a name="when-toouse"></a>Když toouse

Musí mít příslušná oprávnění tooaccess žádné koncové body centra IoT hello. Zařízení musí obsahovat třeba token obsahující zabezpečovací pověření společně s každou zprávu, že odešle tooIoT rozbočovače.

## <a name="access-control-and-permissions"></a>Řízení přístupu a oprávnění

Můžete udělit [oprávnění](#iot-hub-permissions) v hello následující způsoby:

* **IoT hub úrovni sdílených zásad přístupu**. Zásady sdíleného přístupu můžete udělit libovolnou kombinaci [oprávnění](#iot-hub-permissions). Zásady můžete definovat v hello [portál Azure][lnk-management-portal], nebo programově pomocí hello [zprostředkovatele prostředků služby IoT Hub rozhraní REST API][lnk-resource-provider-apis]. Nově vytvořený IoT hub má hello následující výchozí zásady:

  * **iothubowner**: zásada se všechna oprávnění.
  * **Služba**: zásada se **ServiceConnect** oprávnění.
  * **zařízení**: zásada se **DeviceConnect** oprávnění.
  * **registryRead**: zásada se **RegistryRead** oprávnění.
  * **registryReadWrite**: zásada se **RegistryRead** a RegistryWrite oprávnění.
  * **Podle zařízení zabezpečovací pověření**. Každé centrum IoT obsahuje [registru identit][lnk-identity-registry]. Pro každé zařízení v registru této identity můžete nakonfigurovat zabezpečovací pověření, která udělují **DeviceConnect** oprávnění oboru toohello koncové body odpovídající zařízení.

Například v typického řešení IoT:

* komponenty správy zařízení Hello používá hello *registryReadWrite* zásad.
* součástí procesoru událostí Hello používá hello *služby* zásad.
* Hello spuštění zařízení obchodní logiky součást používá hello *služby* zásad.
* Jednotlivých zařízení se připojují přes přihlašovací údaje uložené v registru identit služby IoT hub hello.

> [!NOTE]
> V tématu [oprávnění](#iot-hub-permissions) podrobné informace.

## <a name="authentication"></a>Authentication

Azure IoT Hub uděluje přístup tooendpoints kontrolou token proti hello sdílené zásady přístupu a identit registru zabezpečovací pověření.

Zabezpečovací přihlašovací údaje, jako jsou symetrického klíče, se nikdy odešlou přes přenosu hello.

> [!NOTE]
> Hello poskytovatel prostředků Azure IoT Hub je zabezpečená vašeho předplatného Azure, jako jsou všechny zprostředkovatele v hello [Azure Resource Manager][lnk-azure-resource-manager].

Další informace o tom, najdete v části tooconstruct a použití tokenů zabezpečení [tokeny zabezpečení služby IoT Hub][lnk-sas-tokens].

### <a name="protocol-specifics"></a>Protokol podrobností

Každý podporovaný protokol, například MQTT, AMQP a HTTP, je určena k přenosu tokeny různými způsoby.

Při použití MQTT, hello připojení paketu má hello deviceId jako hello ClientId, {iothubhostname} / {deviceId} v hello pole pro uživatelské jméno a do pole heslo hello tokenu SAS. {iothubhostname} by měl být hello úplné CName hello IoT hub (například contoso.azure-devices.net).

Při použití [AMQP][lnk-amqp], podporuje IoT Hub [SASL prostý] [ lnk-sasl-plain] a [AMQP deklarace identity – zabezpečení na základě-] [ lnk-cbs].

Pokud používáte AMQP deklarace identity – zabezpečení na základě-, určuje hello standardní jak tootransmit tyto tokeny.

SASL prostý text hello **uživatelské jméno** může být:

* `{policyName}@sas.root.{iothubName}`Pokud se používá tokeny úrovni centra IoT.
* `{deviceId}@sas.{iothubname}`Pokud používáte zařízení obor tokeny.

V obou případech pole hesla hello obsahuje hello token, jak je popsáno v [tokeny zabezpečení služby IoT Hub][lnk-sas-tokens].

HTTP implementuje ověřování zahrnutím platný token v hello **autorizace** hlavičky žádosti.

#### <a name="example"></a>Příklad

Uživatelské jméno (DeviceId je malá a velká písmena):`iothubname.azure-devices.net/DeviceId`

Heslo (Generovat SAS token s hello [explorer zařízení] [ lnk-device-explorer] nástroj):`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`

> [!NOTE]
> Hello [SDK služby Azure IoT] [ lnk-sdks] automaticky generovat tokeny při připojování toohello služby. V některých případech nepodporují hello SDK služby Azure IoT všechny protokoly hello nebo všechny metody ověřování hello.

### <a name="special-considerations-for-sasl-plain"></a>Zvláštní upozornění pro SASL prostý

Při použití SASL prostý s AMQP, klient připojení tooan IoT hub můžete použít jeden token pro každé připojení TCP. Když vyprší platnost tokenu hello, hello připojení TCP odpojí od hello služby a aktivuje o obnovení. Toto chování, když není problematické pro back-end aplikačním je poškození aplikace zařízení pro hello následujících důvodů:

* Brány obvykle jménem mnoho zařízení připojit. Pokud používáte SASL prostý, mají toocreate odlišné připojení TCP pro každé zařízení připojení tooan IoT hub. Tento scénář také výrazně zvyšuje hello spotřeba energie a síťových prostředků a hello latence každé připojení zařízení.
* Zařízení s omezenými zdroji jsou negativně ovlivněn pomocí hello vyšší prostředky tooreconnect po každém vypršení platnosti tokenu.

## <a name="scope-iot-hub-level-credentials"></a>Přihlašovací údaje úrovni centra IoT oboru

Můžete určit obor zásad zabezpečení na úrovni centra IoT tak, že vytvoříte tokeny pomocí identifikátoru URI prostředku s omezeným přístupem. Například hello koncový bod toosend zařízení cloud zprávy ze zařízení je **/devices/ {deviceId} / zprávy/události**. Můžete taky zásady sdíleného přístupu úrovně rozbočovače IoT s **DeviceConnect** oprávnění toosign token je jejichž resourceURI **/devices/ {deviceId}**. Tento postup vytvoří token, který je pouze použitelné toosend zprávy jménem zařízení **deviceId**.

Tento mechanismus je podobné toohello [zásad vydavatele Event Hubs][lnk-event-hubs-publisher-policy]a umožní vám tooimplement vlastní metody ověřování.

## <a name="security-tokens"></a>Tokeny zabezpečení

Zabezpečení služby IoT Hub používá tokeny tooauthenticate zařízení a služeb tooavoid odesílání klíčů na hello přenosu. Kromě toho mají omezenou dobu platnosti a obor tokeny zabezpečení. [Sady SDK služby Azure IoT] [ lnk-sdks] automaticky generovat tokeny bez nutnosti žádnou zvláštní konfiguraci. Některé scénáře vyžadují toogenerate a používat přímo tokeny zabezpečení. Mezi tyto scénáře patří:

* Hello přímého použití ploch hello MQTT, AMQP a HTTP.
* Hello implementace vzoru hello služba tokenu, jak je popsáno v [ověřování zařízení vlastní][lnk-custom-auth].

Centrum IoT také umožňuje zařízení tooauthenticate službou IoT Hub pomocí [certifikáty X.509][lnk-x509].

### <a name="security-token-structure"></a>Struktura tokenu zabezpečení

Použijte zabezpečení tokeny toogrant ohraničenou čas přístup toodevices a služby toospecific funkci v IoT Hub. tooget autorizace tooconnect tooIoT rozbočovače, zařízení a služeb musí odesílat tokeny zabezpečení, které jsou podepsané sdíleného přístupu nebo symetrického klíče. Tyto klíče jsou uloženy s identitu zařízení v registru identit hello.

Token podepsán sdílený přístupový klíč uděluje přístup tooall hello funkcí spojených s oprávnění zásad hello sdíleného přístupu. Token podepsaný pomocí identity zařízení symetrického klíče pouze uděluje hello **DeviceConnect** oprávnění pro hello přidružené identitu zařízení.

token zabezpečení Hello má hello následující formát:

`SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

Zde jsou hello očekávaných hodnot:

| Hodnota | Popis |
| --- | --- |
| {podpis} |Řetězec podpisu HMAC SHA256 formuláře hello: `{URL-encoded-resourceURI} + "\n" + expiry`. **Důležité**: klíč hello je dekódovat z formátu base64 a použít jako výpočetní klíče tooperform hello HMAC SHA256. |
| {resourceURI} |Předpony identifikátoru URI (podle segmentu) hello koncových bodů, které lze přistupovat pomocí tohoto tokenu, počínaje název hostitele centra IoT hello (žádné protocol). Například `myHub.azure-devices.net/devices/device1`. |
| {vypršení platnosti} |Řetězce UTF8 pro počet sekund od hello epoch 00:00:00 UTC na 1. ledna pod hodnotou 1970. |
| {Adresu URL-kódovaný resourceURI} |Nižší případ kódování URL prostředku malá hello identifikátor URI |
| {policyName} |Název Hello hello sdílené toowhich zásady přístupu, který odkazuje tento token. Chybějící Pokud hello token odkazuje toodevice registru přihlašovací údaje. |

**Poznámka: na předponě**: předpony identifikátoru URI hello se počítá podle segmentu a ne serverem znak. Například `/a/b` předpona pro `/a/b/c` , ale ne pro `/a/bc`.

Hello následující fragment kódu Node.js ukazuje funkci s názvem **generateSasToken** , výpočtů hello tokenu z hello vstupy `resourceUri, signingKey, policyName, expiresInMins`. Další části Hello podrobnosti, jak tooinitialize hello různé vstupy pro různé token hello případy použití.

```nodejs
var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
    resourceUri = encodeURIComponent(resourceUri);

    // Set expiration in seconds
    var expires = (Date.now() / 1000) + expiresInMins * 60;
    expires = Math.ceil(expires);
    var toSign = resourceUri + '\n' + expires;

    // Use crypto
    var hmac = crypto.createHmac('sha256', new Buffer(signingKey, 'base64'));
    hmac.update(toSign);
    var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

    // Construct autorization string
    var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
    + base64UriEncoded + "&se=" + expires;
    if (policyName) token += "&skn="+policyName;
    return token;
};
```

Ke srovnání hello ekvivalentní toogenerate kód Python, je token zabezpečení:

```python
from base64 import b64encode, b64decode
from hashlib import sha256
from time import time
from urllib import quote_plus, urlencode
from hmac import HMAC

def generate_sas_token(uri, key, policy_name, expiry=3600):
    ttl = time() + expiry
    sign_key = "%s\n%d" % ((quote_plus(uri)), int(ttl))
    print sign_key
    signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())

    rawtoken = {
        'sr' :  uri,
        'sig': signature,
        'se' : str(int(ttl))
    }

    if policy_name is not None:
        rawtoken['skn'] = policy_name

    return 'SharedAccessSignature ' + urlencode(rawtoken)
```

> [!NOTE]
> Vzhledem k tomu, že hello doba platnosti tokenu hello se ověří na počítačích IoT Hub, musí být vypnuté hello hello hodiny hello počítače, který generuje hello token minimální.

### <a name="use-sas-tokens-in-a-device-app"></a>Použití tokeny SAS v aplikaci pro zařízení

Existují dva způsoby tooobtain **DeviceConnect** oprávnění službou IoT Hub s tokeny zabezpečení: pomocí [zařízení symetrického klíče z registru identit hello](#use-a-symmetric-key-in-the-identity-registry), nebo použijte [sdílený přístupový klíč](#use-a-shared-access-policy).

Mějte na paměti, že všechny funkce, které jsou přístupné ze zařízení je zveřejněný prostřednictvím návrhu na koncové body s předponou `/devices/{deviceId}`.

> [!IMPORTANT]
> Hello jediným způsobem, že IoT Hub ověřuje určité zařízení používá hello zařízení identity symetrického klíče. V případech, když se zásada sdíleného přístupu použité tooaccess funkce zařízení, musí řešení hello zvažte hello součástí vydání tokenu zabezpečení hello důvěryhodné komponentu.

Hello zařízení přístupem koncových bodů jsou (bez ohledu na protokol hello):

| Koncový bod | Funkce |
| --- | --- |
| `{iot hub host name}/devices/{deviceId}/messages/events` |Odesílání zpráv typu zařízení cloud. |
| `{iot hub host name}/devices/{deviceId}/devicebound` |Příjem zpráv typu cloud zařízení. |

### <a name="use-a-symmetric-key-in-hello-identity-registry"></a>Použít symetrického klíče v registru identit hello

Pokud používáte identitu zařízení symetrického klíče toogenerate token, hello policyName (`skn`) element hello tokenu je vynechán.

Například token vytvořen tooaccess všechny funkce zařízení by měl mít hello následující parametry:

* identifikátor URI prostředku: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* podpisový klíč: žádné symetrický klíč pro hello `{device id}` identitu,
* žádný název zásady
* kdykoli vypršení platnosti.

Příkladem použití předchozích funkce Node.js hello může být:

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var deviceKey ="...";

var token = generateSasToken(endpoint, deviceKey, null, 60);
```

Hello výsledek, který uděluje přístup tooall funkce pro device1, bude:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697`

> [!NOTE]
> Ho je možné toogenerate token SAS pomocí rozhraní .NET hello [explorer zařízení] [ lnk-device-explorer] nástroje nebo hello různé platformy, na základě uzlu [iothub-explorer] [ lnk-iothub-explorer] nástroj příkazového řádku.

### <a name="use-a-shared-access-policy"></a>Použijte zásady sdíleného přístupu

Při vytváření tokenu z zásady sdíleného přístupu, nastavení hello `skn` název pole toohello hello zásad. Tato zásada musí udělit hello **DeviceConnect** oprávnění.

Hello dva základní scénáře pomocí funkce zařízení tooaccess zásady sdíleného přístupu jsou:

* [cloudové brány protokolu][lnk-endpoints],
* [token služby] [ lnk-custom-auth] používá schémat tooimplement vlastní ověřování.

Od hello zásady sdíleného přístupu může potenciálně udělit přístup tooconnect jako jakékoli zařízení, že je důležité toouse hello správný identifikátor URI prostředku při vytváření tokenů zabezpečení. Toto nastavení je obzvláště důležité pro token služby, které mají tooscope hello tokenu tooa konkrétní zařízení pomocí hello identifikátor URI. Tento bod je méně relevantní pro protokol brány jako jejich jsou již jehož prostřednictvím je umožněn přenos pro všechna zařízení.

Jako příklad tokenu služby pomocí hello předem vytvořené sdílené zásady přístupu volat **zařízení** by vytvořit token s hello následující parametry:

* identifikátor URI prostředku: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* podpisový klíč: jeden z klíčů hello Dobrý den `device` zásad
* Název zásady: `device`,
* kdykoli vypršení platnosti.

Příkladem použití předchozích funkce Node.js hello může být:

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

Hello výsledek, který uděluje přístup tooall funkce pro device1, bude:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device`

Brána protokolu může používat stejný token pro všechna zařízení, jednoduše nastavení hello identifikátor URI příliš hello`myhub.azure-devices.net/devices`.

### <a name="use-security-tokens-from-service-components"></a>Použít tokeny zabezpečení ze součásti služby

Součásti služby můžete vygenerovat pouze tokeny zabezpečení pomocí udělení hello příslušná oprávnění, jak je popsáno dříve zásady sdíleného přístupu.

Tady je funkce služby hello zveřejněné na koncové body hello:

| Koncový bod | Funkce |
| --- | --- |
| `{iot hub host name}/devices` |Vytvoření, aktualizace, získání a odstranění identit zařízení. |
| `{iot hub host name}/messages/events` |Příjem zpráv typu zařízení cloud. |
| `{iot hub host name}/servicebound/feedback` |Přijímat zpětnou vazbu pro zprávy typu cloud zařízení. |
| `{iot hub host name}/devicebound` |Odesílání zpráv typu cloud zařízení. |

Jako příklad služby generování pomocí hello předem vytvořené sdílené zásady přístupu volat **registryRead** by vytvořit token s hello následující parametry:

* identifikátor URI prostředku: `{IoT hub name}.azure-devices.net/devices`,
* podpisový klíč: jeden z klíčů hello Dobrý den `registryRead` zásad
* Název zásady: `registryRead`,
* kdykoli vypršení platnosti.

```nodejs
var endpoint ="myhub.azure-devices.net/devices";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

Hello výsledek, který by udělit přístup tooread všechny identity zařízení, bude:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead`

## <a name="supported-x509-certificates"></a>Podporované certifikáty X.509

Službou IoT Hub můžete použít všechny tooauthenticate certifikát X.509 zařízení. Certifikáty patří:

* **Existující certifikát X.509**. Zařízení již pravděpodobně certifikát X.509 s ním spojená. Hello zařízení můžete použít tento certifikát tooauthenticate službou IoT Hub.
* **A samoobslužné generované a X-509 certifikátu podepsaného svým držitelem**. Výrobce zařízení nebo interní nástroje pro nasazení můžete generovat tyto certifikáty a ukládat hello odpovídající privátní klíč (a certifikátu) na zařízení hello. Můžete použít nástroje, jako [OpenSSL] [ lnk-openssl] a [Windows SelfSignedCertificate] [ lnk-selfsigned] nástroj pro tento účel.
* **Certifikační Autority podepsané certifikát X.509**. tooidentify zařízení a pro ověření službou IoT Hub, můžete použít certifikát X.509, generovány a podepsaný pomocí certifikační autority (CA). IoT Hub pouze ověřuje, že hello kryptografický otisk uvedené odpovídá hello nakonfigurovaný kryptografický otisk. IotHub nelze ověřit řetěz certifikátů hello.

Zařízení může použít certifikát X.509 nebo token zabezpečení pro ověřování, ale ne obojí.

### <a name="register-an-x509-certificate-for-a-device"></a>Registrovat certifikát X.509 pro zařízení.

Hello [sady SDK služby Azure IoT pro jazyk C#] [ lnk-service-sdk] (verze 1.0.8+) podporuje registraci zařízení, které používá certifikátu X.509. certifikát pro ověřování. Jiná rozhraní API, jako je například importu a exportu zařízení také podporují certifikáty X.509.

### <a name="c-support"></a>C\# podpory

Hello **RegistryManager** třída poskytuje programový způsob tooregister zařízení. Konkrétně hello **AddDeviceAsync** a **UpdateDeviceAsync** metody umožňují tooregister a aktualizujte zařízení v registru identit služby IoT Hub hello. Tyto dvě metody přijímají **zařízení** instance jako vstup. Hello **zařízení** třída zahrnuje **ověřování** vlastnost, která vám umožní toospecify primární a sekundární X.509 kryptografické otisky certifikátu. Kryptografický otisk Hello představuje hodnotu hash SHA-1 certifikátu X.509 hello (uložené pomocí binární kódování DER). Máte možnost hello zadání primární kryptografický otisk nebo sekundární kryptografický otisk nebo obojí. Primární a sekundární kryptografické otisky jsou podporované toohandle certifikát výměny scénáře.

> [!NOTE]
> IoT Hub nevyžaduje ani hello celý certifikát X.509, pouze kryptografický otisk hello uložit.

Zde je ukázka C\# code zařízení pomocí certifikátu X.509 tooregister fragment kódu:

```csharp
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "921BC9694ADEB8929D4F7FE4B9A3A6DE58B0790B"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-certificate-during-run-time-operations"></a>Použít certifikát X.509 během spuštění operací

Hello [zařízení Azure IoT SDK pro .NET] [ lnk-client-sdk] (verze 1.0.11+) podporuje hello certifikáty X.509.

### <a name="c-support"></a>C\# podpory

Hello třída **DeviceAuthenticationWithX509Certificate** podporuje hello vytvoření **DeviceClient** instance, používá certifikát X.509. certifikát X.509 Hello musí být ve formátu PFX (také nazývané PKCS #12) hello, která zahrnuje hello privátní klíč.

Zde je fragment kódu ukázka:

```csharp
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a>Ověřování vlastní zařízení

Můžete použít hello IoT Hub [registru identit] [ lnk-identity-registry] tooconfigure podle zařízení zabezpečovací přihlašovací údaje a řízení přístupu pomocí [tokeny] [ lnk-sas-tokens] . Pokud řešení IoT už má vlastní identitu registru nebo ověřování schématu, zvažte vytvoření *token služby* toointegrate Tato infrastruktura službou IoT Hub. Tímto způsobem můžete další funkce IoT ve vašem řešení.

Token služby je vlastní Cloudová služba. Používá služby IoT Hub *sdílené zásady přístupu* s **DeviceConnect** oprávnění toocreate *obor zařízení* tokeny. Tyto tokeny umožňují zařízení tooconnect tooyour IoT hub.

![Kroky hello služby tokenů vzor][img-tokenservice]

Zde jsou hlavní kroky hello hello služby tokenů vzoru:

1. Vytvoření služby IoT Hub sdílených zásad přístupu s **DeviceConnect** oprávnění pro službu IoT hub. Tyto zásady můžete vytvořit v hello [portál Azure] [ lnk-management-portal] nebo prostřednictvím kódu programu. token služba Hello používá tento tokeny hello toosign zásad, který vytváří.
1. Když se zařízení potřebuje tooaccess služby IoT hub, vyžádá podepsaný token ze služby tokenu. Hello zařízení můžete ověřit pomocí vaše vlastní identitu registru nebo ověřování schématu toodetermine hello identitu zařízení, že služba tokenu hello používá toocreate hello token.
1. Služba tokenu Hello vrátí token. Hello token je vytvořená pomocí `/devices/{deviceId}` jako `resourceURI`, s `deviceId` jako zařízení hello ověřovaného. Služba tokenu Hello používá hello sdíleného přístupu zásad tooconstruct hello token.
1. zařízení Hello používá hello token přímo službou hello IoT hub.

> [!NOTE]
> Můžete použít třídu .NET hello [SharedAccessSignatureBuilder] [ lnk-dotnet-sas] nebo hello třída jazyka Java [IotHubServiceSasToken] [ lnk-java-sas] toocreate token ve službě tokenu.

Služba tokenu Hello můžete nastavit vypršení platnosti tokenu hello podle potřeby. Když vyprší platnost tokenu hello, servery hello IoT hub hello připojení zařízení. Potom hello zařízení musíte požádat o nový token od služby tokenů hello. Čas vypršení platnosti krátký zvyšuje zatížení hello hello zařízení a služby tokenů hello.

Pro zařízení tooconnect tooyour rozbočovače, je třeba stále přidat ji toohello registru identit služby IoT Hub – Přestože hello zařízení používá token a ne tooconnect klíče o zařízení. Proto můžete pokračovat v řízení přístupu podle zařízení toouse povolením nebo zakázáním identit zařízení ve hello [registru identit][lnk-identity-registry]. Tento přístup snižuje rizika spojená hello tokeny pomocí vypršení platnosti dlouhou dobu.

### <a name="comparison-with-a-custom-gateway"></a>Porovnání s vlastní bránu

vzor služby tokenů Hello je, že hello doporučená tooimplement způsob schéma vlastní identitu registru nebo ověřování službou IoT Hub. Tento vzor je doporučená, protože IoT Hub pokračuje toohandle většina hello řešení provozu. Ale pokud schéma vlastní ověřování hello tak vzájemně propojeny s protokolem hello, možná budete potřebovat *vlastní brány* tooprocess všechny hello provoz. Příkladem takové situaci je pomocí[zabezpečení TLS (Transport Layer) a předsdílených klíčů (PSKs)][lnk-tls-psk]. Další informace najdete v tématu hello [brány protokolu] [ lnk-protocols] tématu.

## <a name="reference-topics"></a>Témata odkazů:

Hello následující odkazy na témata poskytují další informace o řízení přístupu tooyour IoT hub.

## <a name="iot-hub-permissions"></a>IoT Hub oprávnění

Hello následující tabulka uvádí hello oprávnění můžete použít Centrum IoT tooyour toocontrol přístup.

| Oprávnění | Poznámky |
| --- | --- |
| **RegistryRead** |Registr identit toohello uděluje přístup pro čtení. Další informace najdete v tématu [registru identit][lnk-identity-registry]. <br/>Toto oprávnění je používán back-end cloudové služby. |
| **RegistryReadWrite** |Uděluje oprávnění ke čtení a zápisu toohello registru identit. Další informace najdete v tématu [registru identit][lnk-identity-registry]. <br/>Toto oprávnění je používán back-end cloudové služby. |
| **ServiceConnect** |Uděluje přístup ke komunikaci služby směřujících toocloud a sledování koncových bodů. <br/>Uděluje oprávnění tooreceive zařízení cloud zprávy, odesílání zpráv typu cloud zařízení a načtení hello odpovídající potvrzování doručení. <br/>Uděluje oprávnění tooretrieve doručení potvrzení pro soubor odešle. <br/>Uděluje oprávnění tooaccess zařízení dvojčata tooupdate značky a požadované vlastnosti načíst hlášené vlastnosti a spouštět dotazy. <br/>Toto oprávnění je používán back-end cloudové služby. |
| **DeviceConnect** |Uděluje přístup k toodevice přístupem koncových bodů. <br/>Uděluje oprávnění toosend zařízení cloud zpráv a příjem zpráv typu cloud zařízení. <br/>Uděluje oprávnění tooperform nahrávání souborů ze zařízení. <br/>Uděluje oprávnění tooreceive zařízení twin potřeby vlastnost oznámení a aktualizace zařízení twin hlášené vlastnosti. <br/>Uděluje oprávnění tooperform soubor odešle. <br/>Toto oprávnění je používán zařízení. |

## <a name="additional-reference-material"></a>Odkaz na další materiály

Další témata referenční příručka vývojáře pro službu IoT Hub hello patří:

* [Koncové body centra IoT] [ lnk-endpoints] popisuje hello různých koncových bodů, které každý IoT hub zpřístupní pro spuštění a management operace.
* [Omezování a kvóty] [ lnk-quotas] popisuje hello kvót a omezování chování, které se vztahují toohello služby IoT Hub.
* [Azure IoT zařízení a služby sady SDK] [ lnk-sdks] seznamy hello různé jazykové sady SDK můžete použít při vývoji aplikace zařízení a služby, které interakci s centrem IoT.
* [IoT Hub dotazovací jazyk] [ lnk-query] popisuje hello dotazovací jazyk tooretrieve informace ze služby IoT Hub o úlohách a dvojčata zařízení můžete použít.
* [Podpora IoT Hub MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře služby IoT Hub pro protokol MQTT hello.

## <a name="next-steps"></a>Další kroky

Nyní jste se naučili, jak toocontrol přístup IoT Hub, může být zájem o hello následující témata Průvodce vývojáře IoT Hub:

* [Používání stavu toosynchronize dvojčata zařízení a konfigurací][lnk-devguide-device-twins]
* [Volání metody přímé na zařízení][lnk-devguide-directmethods]
* [Plánování úloh na několika zařízeních][lnk-devguide-jobs]

Pokud chcete tootry některé z hello konceptů popsaných v tomto článku, může být zájem o hello následující kurzy IoT Hub:

* [Začínáme s Azure IoT Hub][lnk-getstarted-tutorial]
* [Jak toosend cloud zařízení zprávy s centrem IoT][lnk-c2d-tutorial]
* [Jak tooprocess zpráv typu zařízení cloud IoT Hub][lnk-d2c-tutorial]

<!-- links and images -->

[img-tokenservice]: ./media/iot-hub-devguide-security/tokenservice.png
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-openssl]: https://www.openssl.org/
[lnk-selfsigned]: https://technet.microsoft.com/library/hh848633

[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-sas-tokens]: iot-hub-devguide-security.md#security-tokens
[lnk-amqp]: https://www.amqp.org/
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab
[lnk-management-portal]: https://portal.azure.com
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-dotnet-sas]: https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx
[lnk-java-sas]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service.auth._iot_hub_service_sas_token
[lnk-tls-psk]: https://tools.ietf.org/html/rfc4279
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-custom-auth]: iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: iot-hub-devguide-security.md#supported-x509-certificates
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-service-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/service
[lnk-client-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
