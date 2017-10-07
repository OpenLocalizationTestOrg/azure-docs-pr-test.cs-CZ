---
title: "aaaSecure nasazením Internet věcí | Microsoft Docs"
description: "Tento článek podrobnosti o tom, jak toosecure nasazení IoT"
services: 
suite: iot-suite
documentationcenter: 
author: YuriDio
manager: timlt
editor: 
ms.assetid: 95c23341-16b0-4954-b3f2-d2e82ab7b367
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: yurid
ms.openlocfilehash: befba8f2009279c2217dcd3496d529139134ec01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="secure-your-iot-deployment"></a>Zabezpečení nasazení IoT
Tento článek obsahuje hello další úroveň podrobností pro zabezpečení infrastruktury založené na Azure IoT Internet věcí (IoT) hello. Odkazuje tooimplementation úroveň podrobností pro konfiguraci a nasazení jednotlivých součástí. Poskytuje taky porovnání a možnosti mezi různé konkurenční metody.

Zabezpečení nasazení Azure IoT hello je možné rozdělit do hello následující tři oblasti zabezpečení:

* **Zabezpečení zařízení**: zabezpečení zařízení IoT hello při nasazení v divoký hello.
* **Zabezpečení připojení**: zajištění všechna data přenášená mezi hello zařízení IoT a IoT Hub je důvěrný a manipulací.
* **Zabezpečení cloudu**: poskytnutím prostředků toosecure dat prochází přes, a je uložen v cloudu hello.

![Tři oblasti zabezpečení][img-overview]

## <a name="secure-device-provisioning-and-authentication"></a>Zabezpečené zřizování zařízení a ověřování
Hello Azure IoT Suite zabezpečuje zařízení IoT pomocí hello následující dvě metody:

* Tím, že pro každé zařízení, které mohou být využívána hello zařízení toocommunicate s hello IoT Hub poskytuje jedinečnou identitu klíč (tokeny zabezpečení).
* Pomocí na zařízení [certifikát X.509] [ lnk-x509] a privátní klíče jako znamená tooauthenticate hello zařízení toohello IoT Hub. Tato metoda ověřování zajišťuje, že hello privátní klíč na zařízení hello nezná mimo hello zařízení kdykoli, poskytuje vyšší úroveň zabezpečení.

Metoda token zabezpečení Hello poskytuje ověření pro každé volání provedené hello zařízení tooIoT Centrum tím, že přidružíte hello symetrického klíče tooeach volání. Ověřování na základě X.509 umožňuje ověřování zařízení IoT ve fyzické vrstvě hello jako součást hello TLS připojení zařízení. Metoda na základě zabezpečení tokenu Hello můžete použít bez ověřování hello X.509, což je méně bezpečné vzor. Hello volba mezi hello dvě metody je primárně závisí podle míry budou zabezpečené hello ověřování zařízení potřebuje toobe a dostupnost zabezpečeného úložiště na zařízení hello (toostore hello privátní klíč bezpečně).

## <a name="iot-hub-security-tokens"></a>Tokeny zabezpečení IoT Hub
IoT Hub používá zabezpečení tokeny tooauthenticate zařízení a služeb tooavoid, odesílání klíčů v síti hello. Kromě toho mají omezenou dobu platnosti a obor tokeny zabezpečení. Sady SDK služby Azure IoT automaticky generovat tokeny bez nutnosti žádnou zvláštní konfiguraci. Některé scénáře, ale vyžaduje toogenerate hello uživatele a používá tokeny zabezpečení přímo. Mezi ně patří hello přímého použití hello MQTT, AMQP nebo HTTP ploch nebo hello implementace vzoru hello služby tokenů.

Další informace o struktuře hello hello token zabezpečení a jeho použití naleznete v hello následující články:

* [Struktura tokenu zabezpečení][lnk-security-tokens]
* [Pomocí SAS tokeny jako zařízení][lnk-sas-tokens]

Každé centrum IoT má [registru identit] [ lnk-identity-registry] , může být prostředky na zařízení použité toocreate v hello služby, jako je například fronty, která obsahuje neukládají zprávy typu cloud zařízení a toohello tooallow přístup zařízení přístupem koncových bodů. Hello registru identit služby IoT Hub poskytuje zabezpečené úložiště identit zařízení a zabezpečení klíčů pro řešení. Jednotlivé nebo skupiny identit zařízení mohou být přidány tooan povolit seznam nebo seznamy bloku, povolení plnou kontrolu nad přístup k zařízení. Hello následující články poskytují další podrobnosti o hello struktura registru identit hello a podporované operace.

[IoT Hub podporuje protokoly, například MQTT, AMQP a HTTP][lnk-protocols]. Každý z těchto protokolů jinak použijte tokeny zabezpečení z tooIoT zařízení IoT hello rozbočovače:

* AMQP: SASL prostý a založené na deklaracích AMQP zabezpečení ({policyName}@sas.root. { iothubName} v případě hello tokenů úrovni centra IoT; {deviceId} v případě zařízení obor tokeny).
* MQTT: Připojení paketu používá {deviceId} jako hello {ClientId}, {IoThubhostname} / {deviceId} v hello **uživatelské jméno** pole a SAS token v hello **heslo** pole.
* HTTP: Je platný token v hlavičce autorizace požadavku hello.

Registr identit služby IoT Hub může být použité tooconfigure podle zařízení zabezpečovací přihlašovací údaje a řízení přístupu. Však řešení IoT již má významné investice v [vlastní zařízení identity registru nebo ověřování schématu][lnk-custom-auth], lze ji integrovat do existující infrastruktury s centrem IoT vytvořením služby tokenů.

### <a name="x509-certificate-based-device-authentication"></a>Ověřování zařízení na základě certifikátu X.509
Hello použití [zařízení na základě certifikátu X.509] [ lnk-protocols] a jeho přidružený privátní a veřejné klíče dvojice povoluje další ověřování ve fyzické vrstvě hello. privátní klíč Hello je bezpečně uloženo na hello zařízení a není zjistitelný mimo hello zařízení. certifikát X.509 Hello obsahuje informace o hello zařízení, jako je například ID zařízení a další podrobnosti organizace. Podpis certifikátu hello je generována pomocí hello privátní klíč.

Zřizování toku vysoké úrovně zařízení:

* Přidružte identifikátor tooa fyzického zařízení s – identitu zařízení a/nebo zařízení přidružených toohello certifikát X.509 během zařízení výrobní nebo uvedení do provozu.
* Vytvořte záznam odpovídající identity ve IoT Hub – identitu zařízení a informace o přidružených zařízení v registru identit služby IoT Hub hello.
* Kryptografický otisk certifikátu X.509 bezpečně uložte v registru identit služby IoT Hub.

### <a name="root-certificate-on-device"></a>Kořenový certifikát na zařízení
Při navazování připojení TLS zabezpečené službou IoT Hub, ověřuje hello zařízení IoT pomocí kořenový certifikát, který je součástí hello zařízení SDK služby IoT Hub. Pro klienta hello C sady SDK hello certifikát se nachází ve složce hello "\\c\\certifikátů" v kořenovém úložišti hello hello. I když tyto kořenových certifikátů je dlouhodobé, jsou stále může vyprší nebo odvolat. Pokud neexistuje žádný způsob aktualizace hello certifikát na zařízení hello hello, že zařízení nemusí být možné připojit toosubsequently toohello IoT Hub (nebo jiné cloudové služby). Toto riziko bude efektivně snížilo s znamená tooupdate hello kořenový certifikát po nasazení zařízení IoT hello.

## <a name="securing-hello-connection"></a>Zabezpečení připojení hello
Připojení k Internetu mezi hello zařízení IoT a IoT Hub, je zabezpečena pomocí hello standard zabezpečení TLS (Transport Layer). Azure IoT podporuje [TLS 1.2][lnk-tls12], TLS 1.1 a TLS 1.0, v tomto pořadí. Podpora pro protokol TLS 1.0 je k dispozici pouze z důvodů zpětné kompatibility. Vzhledem k tomu, že poskytuje nejvyšší zabezpečení hello se doporučuje toouse TLS 1.2.

Azure IoT Suite podporuje hello následující šifrovací sady, v tomto pořadí.

| Šifrovací sada | Délka |
| --- | --- |
| Protokol TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA384 secp384r1 ECDH (0xc028) (ekvalizéru FS 7680 bits RSA) |256 |
| Protokol TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0xc027) ECDH secp256r1 (ekvalizéru FS 3072 bits RSA) |128 |
| Protokol TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1 (ekvalizéru FS 7680 bits RSA) |256 |
| Protokol TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1 (ekvalizéru FS 3072 bits RSA) |128 |
| Protokol TLS\_RSA\_WITH\_AES\_256\_GCM\_SHA384 (0x9d) |256 |
| Protokol TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256 (0x9c) |128 |
| Protokol TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA256 (0x3d) |256 |
| Protokol TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0x3c) |128 |
| Protokol TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA (0x35) |256 |
| Protokol TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA (0x2f) |128 |
| Protokol TLS\_RSA\_WITH\_3DES\_EDE\_CBC\_SHA (0xa) |112 |

## <a name="securing-hello-cloud"></a>Zabezpečení cloudu hello
Azure IoT Hub umožňuje definice [zásad řízení přístupu] [ lnk-protocols] pro každý klíč zabezpečení. Používá hello následující sadu oprávnění toogrant přístup tooeach koncových bodů služby IoT Hub. Oprávnění omezit přístup tooan hello, IoT Hub v závislosti na funkcích.

* **RegistryRead**. Registr identit toohello uděluje přístup pro čtení. Další informace najdete v tématu [registru identit][lnk-identity-registry].
* **RegistryReadWrite**. Uděluje oprávnění ke čtení a zápisu toohello registru identit. Další informace najdete v tématu [registru identit][lnk-identity-registry].
* **ServiceConnect**. Uděluje přístup ke komunikaci služby směřujících toocloud a sledování koncových bodů. Například udělí oprávnění tooback-end cloudu zpráv typu zařízení cloud services tooreceive, odesílání zpráv typu cloud zařízení a načíst hello odpovídající doručení potvrzování.
* **DeviceConnect**. Uděluje přístup k toodevice přístupem koncových bodů. Například uděluje oprávnění zpráv typu zařízení cloud toosend a příjem zpráv typu cloud zařízení. Toto oprávnění je používán zařízení.

Existují dva způsoby tooobtain **DeviceConnect** oprávnění službou IoT Hub s [tokeny zabezpečení][lnk-sas-tokens]: pomocí klíče identity zařízení nebo sdílený přístupový klíč. Kromě toho je důležité toonote zveřejněného všechny funkce, které jsou přístupné ze zařízení záměrné u koncových bodů s předponou `/devices/{deviceId}`.

[Součásti služby můžete generovat jenom tokeny zabezpečení] [ lnk-service-tokens] pomocí sdílené zásady přístupu udělení hello příslušná oprávnění.

Azure IoT Hub a dalším službám, které může být součástí řešení hello povolit správu uživatele, kteří používají hello Azure Active Directory.

Data ve službě Azure IoT Hub požity mohou být spotřebovávána různých služeb, jako je Azure Stream Analytics a úložiště objektů blob Azure. Tyto služby umožňují přístup pro správu. Další informace o těchto službách a k dispozici následující možnosti:

* [Azure Cosmos DB][lnk-docdb]: škálovatelné a plně indexované databázová služba pro částečně strukturovaná data, která spravuje metadata pro zařízení hello zřídíte, jako je například atributy, konfiguraci a vlastnosti zabezpečení. Cosmos DB nabízí vysoce výkonné a vysokou propustností zpracování, bez ohledu na schéma indexování dat a bohaté rozhraní SQL.
* [Azure Stream Analytics][lnk-asa]: streamu v reálném čase zpracování hello cloudu, který umožňuje vám toorapidly vývoji a nasazení nízkými náklady analytics řešení toouncover přehledy v reálném čase ze zařízení, senzorů, infrastruktury a aplikace. Hello data z tato plně spravovaná služba můžete škálovat tooany svazku, zatímco stále dosahuje vysoké propustnosti, s nízkou latencí a odolnost proti chybám.
* [Azure App Services][lnk-appservices]: cloudové platformy toobuild výkonné webové a mobilní aplikace, které se připojují toodata kdekoli; v cloudu hello nebo místně. Vytvářejte poutavé mobilní aplikace pro iOS, Android a Windows. Integrate váš Software jako služba (SaaS) a podnikové aplikace s toodozens připojení se na pole cloudových služeb a podnikové aplikace. Kód v váš oblíbený jazyk a IDE toobuild webové aplikace (.NET, Node.js, PHP, Python nebo Java) a rozhraní API rychlejší než kdy dřív.
* [Služba Logic Apps][lnk-logicapps]: hello funkce Logic Apps služby Azure App Service pomáhá integrovat IoT řešení tooyour existující-obchodní systémy a automatizovat pracovní postupy. Služba Logic Apps umožňuje vývojářům toodesign pracovních, které začínají spouštěčem událostí a provádějí sérii kroků – pravidla a akce, které pomocí výkonné konektory toointegrate své obchodní procesy. Služba Logic Apps nabízí připojení se na pole tooa velká ekosystém SaaS, cloudových a místních aplikací.
* [Úložiště objektů blob Azure][lnk-blob]: spolehlivé a ekonomické cloudové úložiště pro data hello, že vaše zařízení odesílají toohello cloudu.

## <a name="conclusion"></a>Závěr
Tento článek obsahuje přehled implementace úroveň podrobností pro navrhování a nasazení infrastruktury IoT pomocí Azure IoT. Konfigurace jednotlivých součástí toobe zabezpečené je klíč v zabezpečení hello celé infrastruktury IoT. k dispozici v Azure IoT rozhodnutích při návrhu Hello zadejte určité úrovně flexibilitu a možnost výběru; Každý výběr však může mít vliv na zabezpečení. Doporučuje se každý z těchto možností vyhodnotí vyhodnoťte riziko/náklady.

## <a name="see-also"></a>Viz také
Můžete také prozkoumat některé hello další funkce a možnosti hello předkonfigurovaná řešení IoT Suite:

* [Přehled řešení předkonfigurované prediktivní údržby][lnk-predictive-overview]
* [Nejčastější dotazy k sadě IoT Suite][lnk-faq]

Další informace o zabezpečení služby IoT Hub v [řízení přístupu tooIoT rozbočovače] [ lnk-devguide-security] v hello Příručka vývojáře pro službu IoT Hub.


[img-overview]: media/iot-suite-security-deployment/overview.png

[lnk-security-tokens]: ../iot-hub/iot-hub-devguide-security.md#security-token-structure
[lnk-sas-tokens]: ../iot-hub/iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-protocols]: ../iot-hub/iot-hub-devguide-security.md
[lnk-custom-auth]: ../iot-hub/iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: http://www.itu.int/rec/T-REC-X.509-201210-I/en
[lnk-tls12]: https://tools.ietf.org/html/rfc5246
[lnk-service-tokens]: ../iot-hub/iot-hub-devguide-security.md#use-security-tokens-from-service-components
[lnk-docdb]: https://azure.microsoft.com/services/documentdb/
[lnk-asa]: https://azure.microsoft.com/services/stream-analytics/
[lnk-appservices]: https://azure.microsoft.com/services/app-service/
[lnk-logicapps]: https://azure.microsoft.com/services/app-service/logic/
[lnk-blob]: https://azure.microsoft.com/services/storage/

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-devguide-security]: ../iot-hub/iot-hub-devguide-security.md
