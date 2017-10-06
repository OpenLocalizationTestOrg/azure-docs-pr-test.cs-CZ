---
title: "aaaAzure Přehled služby IoT Hub | Microsoft Docs"
description: "Přehled hello služby Azure IoT Hub: co je IoT Hub, připojení zařízení, pro internet věcí komunikačních schémat, bran a vzor hello komunikace s asistencí služby"
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d0b46868a1ec9e13c8f107b90269c5307f4ba27c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-hello-azure-iot-hub-service"></a>Přehled služby Azure IoT Hub hello

Vítejte tooAzure IoT Hub. Tento článek obsahuje přehled služby Azure IoT Hub a popisuje, proč byste měli používat tuto službu tooimplement do řešení pro Internet věcí (IoT). Azure IoT Hub je plně spravovaná služba, která umožňuje spolehlivou a zabezpečenou obousměrnou komunikaci mezi miliony zařízení IoT a back-endem řešení. Azure IoT Hub:

* Nabízí několik možností komunikace zařízení-cloud a cloud-zařízení, včetně jednosměrného zasílání zpráv, přenosu souborů a metod požadavek-odpověď.
* Poskytuje integrovanou deklarativní zpráva směrování tooother služby Azure.
* Poskytuje dotazovatelné úložiště metadat zařízení a synchronizované stavové informace.
* Umožňuje zabezpečit komunikaci a řízení přístupu pomocí klíčů specifických pro jednotlivá zařízení a certifikátů X.509.
* Zajišťuje rozsáhlé monitorování událostí připojení zařízení a správy identity zařízení.
* Zahrnuje knihovny zařízení pro hello Nejoblíbenější jazyky a platformy.

článek Hello [srovnání služeb IoT Hub a Event Hubs] [ lnk-compare] popisuje hello hlavní rozdíly mezi těmito dvěma službami a zvýrazňuje hello výhody používání služby IoT Hub ve vašich vlastních IoT řešení.

Další informace o tom, jak Azure a IoT Hub pomoc se zabezpečením řešení IoT najdete v tématu [zabezpečení Internetu věcí z hello pozadí][lnk-security-ground-up].

![Služba Azure IoT Hub jako cloudová brána v řešení internetu věcí][img-architecture]

> [!NOTE]
> Podrobné informace o architektuře IoT najdete v části hello [referenční architektura IoT Microsoft Azure][lnk-refarch].

## <a name="iot-device-connectivity-challenges"></a>Výzvy spojené s připojením zařízení IoT

IoT Hub a hello knihovny zařízení vám pomůže toomeet hello výzvám, které představuje tooreliably a bezpečné připojení zařízení toohello back-end řešení. Zařízení IoT:

* Jsou často vestavěnými systémy bez lidské obsluhy.
* Můžou se nacházet ve vzdálených umístěních, kam je fyzický přístup nákladný.
* Můžou být dostupná jenom prostřednictvím back-end hello řešení.
* Můžou mít omezené prostředky pro napájení a zpracování.
* Můžou mít přerušované, pomalé nebo nákladné síťové připojení.
* Může být nutné toouse aplikace vlastní, vlastní nebo průmyslové protokoly.
* Můžou být vytvořená pomocí rozsáhlé sady oblíbených hardwarových a softwarových platforem.

Kromě výše uvedených požadavků toohello, jakékoli řešení IoT musí zajistit také škálování, zabezpečení a spolehlivost. Hello výslednou sadu požadavků na připojení je obtížné tooimplement při pomocí tradičních technologií, jako jsou webové kontejnery a zprostředkovatelé zasílání zpráv.

## <a name="why-use-azure-iot-hub"></a>Proč používat Azure IoT Hub?

Kromě toho sada bohaté tooa [zařízení cloud] [ lnk-d2c-guidance] a [cloud zařízení] [ lnk-c2d-guidance] možnosti komunikace, včetně zasílání zpráv, souborů přenosy a metody požadavku a odpovědi, Azure IoT Hub adresy hello problémy s připojením zařízení v hello následující způsoby:

* **Dvojče zařízení**. Pomocí [dvojčat zařízení][lnk-twins] můžete ukládat, synchronizovat a dotazovat se na metadata a stav zařízení. Dvojčata zařízení jsou dokumenty JSON, které obsahují informace o stavu zařízení (metadata, konfigurace a podmínky). IoT Hub trvá dvojče zařízení pro každé zařízení připojení tooIoT rozbočovače.

* **Ověřování podle zařízení a zabezpečené připojení**. Každému zařízení vlastní můžete zřídit [klíč zabezpečení] [ lnk-devguide-security] tooenable ho tooconnect tooIoT rozbočovače. Hello [registru identit služby IoT Hub] [ lnk-devguide-identityregistry] ukládá v řešení identity a klíče zařízení. Back-end řešení můžete přidat tooallow jednotlivá zařízení nebo odepřít seznamy tooenable úplnou kontrolu nad přístup k zařízení.

* **Trasy zařízení cloud zprávy tooAzure služeb na základě pravidel, deklarativní**. IoT Hub umožňuje vám zprávy toodefine trasy založené na směrování toocontrol pravidla, kde vaše Centrum posílání zpráv typu zařízení cloud. Pravidla směrování nevyžadují toowrite je jakýkoli kód a může trvat hello místo dispečerů vlastní po přijímání zpráv.

* **Sledování operací připojení zařízení**. O událostech připojení zařízení a operacích správy identity zařízení můžete dostávat podrobné protokoly operací. Tato možnost monitorování umožňuje vaší IoT řešení tooidentify problémy s připojením, jako jsou zařízení, které se pokusí tooconnect pomocí nesprávných přihlašovacích údajů, posílat zprávy příliš často, nebo zamítnout všechny zprávy typu cloud zařízení.

* **Rozsáhlá sada knihoven zařízení**. [Sady SDK pro zařízení Azure IoT][lnk-device-sdks] jsou dostupné a podporované pro různé jazyky a platformy – pro řadu distribucí systému Linux, systém Windows a operační systémy v reálném čase. Sady SDK pro zařízení Azure IoT také podporují spravované jazyky,  jako je C#, Java či JavaScript.

* **Protokoly a rozšiřitelnost IoT**. Pokud vaše řešení nemůže používat knihovny zařízení hello, IoT Hub zpřístupní veřejný protokol, který umožňuje protokoly zařízení toonatively použití hello MQTT v3.1.1, HTTP 1.1 nebo AMQP 1.0 protokoly. Můžete také rozšířit toosupport IoT Hub pro vlastní protokoly. provedete:

  * Vytvořením brány pole s [Azure IoT Edge] [ lnk-iot-edge] která převede váš vlastní protokol tooone Dobrý den, porozuměl jsem jim tří protokolů službou IoT Hub.
  * Přizpůsobení hello [brány protokolu Azure IoT][protocol-gateway], komponentu s otevřeným zdrojem, který běží v cloudu hello.

* **Škálování**. Azure IoT Hub lze škálovat toomillions současně připojených zařízení a miliony událostí za sekundu.

## <a name="gateways"></a>Brány

Se brána v řešení IoT obvykle buď [brány protokolu] [ lnk-iotedge] který je nasazen v cloudu hello nebo [brána pole] [ lnk-field-gateway] tedy nasadit místně ve vašich zařízeních. Brána protokolu provádí překlad protokolu, například MQTT tooAMQP. Brána pole můžete spustit analytics hello hranu, rozhodnutí dobou tooreduce latenci, poskytovat zařízením služby správy, vynucovat zabezpečení a ochrany osobních údajů omezení a také provést překlad protokolu. Oba typy brány fungují jako prostředníci mezi vaším zařízením a službou IoT Hub.

Brána pole se od jednoduchého zařízení pro směrování provozu (například zařízení pro překládání adres nebo brány firewall) liší, protože při správě přístupu a informačního toku ve vašem řešení obvykle hraje aktivní roli.

Řešení může zahrnovat brány protokolu i pole.

## <a name="how-does-iot-hub-work"></a>Jak služba IoT Hub funguje?

Azure IoT Hub implementuje hello [komunikace s asistencí služby] [ lnk-service-assisted-pattern] vzor toomediate hello interakce mezi zařízeními a řešení back-end. cílem Hello komunikace s asistencí služby je tooestablish důvěryhodné, obousměrné komunikační trasy mezi řídicím systémem, jako je například IoT Hub a zařízeními pro zvláštní účely, které jsou nasazeny v nedůvěryhodných fyzického místa na disku. Hello schéma zavádí následující zásady hello:

* Zabezpečení má přednost před všemi dalšími možnostmi.

* Zařízení nepřijímají nevyžádané informace o síti. Zařízení navazuje všechna připojení a vytváří trasy jako pouze odchozí. Pro zařízení tooreceive příkaz z back-end hello řešení hello musí pravidelně inicializovat připojení toocheck pro všechny čekající příkazy tooprocess.

* Zařízení musí připojit pouze tooor vytvořit trasy toowell známé služby, se kterými mají partnerský vztah, jako je například IoT Hub.

* Hello komunikační trasa mezi zařízením a službou nebo mezi zařízením a bránou je zabezpečená na hello aplikačního protokolu.

* Autorizace a ověření na úrovni systému jsou založeny na identifikaci jednotlivých zařízení. Díky tomu jsou přístupové údaje a povolení téměř okamžitě odvolatelné.

* Obousměrnou komunikaci pro zařízení, která se připojují k kvůli toopower nebo připojení obavy zjednodušuje tím, že příkazy a oznámení zařízení, dokud se zařízení připojí tooreceive je. IoT Hub uchovává fronty zařízení specifické pro odesílané příkazy hello.

* Datové části aplikací se zabezpečují samostatně za účelem chráněného přenosu dat prostřednictvím bran tooa konkrétní službu.

Hello mobilní průmysl využil hello komunikace s asistencí služby vzor v obrovském měřítku tooimplement nabízených oznámení služeb, jako [služeb nabízených oznámení Windows][lnk-wns], [Google Cloud Messaging][lnk-google-messaging], a [Apple Push Notification Service][lnk-apple-push].

IoT Hub se podporuje přes cestu veřejného partnerského vztahu ExpressRoute.

## <a name="next-steps"></a>Další kroky

toolearn jak toosend zpráv ze zařízení a přijímat z IoT Hub, a také jak směruje zprávy tooconfigure, najdete v části [odesílat a přijímat zprávy službou IoT Hub][lnk-send-messages].

toolearn způsobu IoT Hub umožňuje správu zařízení založených na standardech pro tooremotely můžete spravovat, konfigurovat a aktualizovat vaše zařízení, najdete v [přehled správy zařízení s centrem IoT][lnk-device-management].

tooimplement klientským aplikacím na širokou škálu zařízení hardwarových platforem a operačních systémů, můžete použít sady SDK pro zařízení Azure IoT hello. sady SDK pro zařízení Hello zahrnují knihovny, které usnadňují odesílání telemetrie tooan IoT hub a příjem cloud zařízení zpráv. Při použití sady SDK, hello zařízení můžete vybírat z různých síťových protokolů toocommunicate službou IoT Hub. toolearn více, viz hello [informace o zařízení sady SDK][lnk-device-sdks].

tooget spuštění psaní nějaký kód a spuštění některých ukázek najdete v části hello [Začínáme se službou IoT Hub] [ lnk-get-started] kurzu.

[img-architecture]: media/iot-hub-what-is-iot-hub/hubarchitecture.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[protocol-gateway]: https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md
[lnk-service-assisted-pattern]: http://blogs.msdn.com/b/clemensv/archive/2014/02/10/service-assisted-communication-for-connected-devices.aspx "Service Assisted Communication (Komunikace s asistencí služby), příspěvek na blogu od Clemense Vasterse"
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-iotedge]: iot-hub-protocol-gateway.md
[lnk-field-gateway]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-devguide-identityregistry]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-wns]: https://msdn.microsoft.com/library/windows/apps/mt187203.aspx
[lnk-google-messaging]: https://developers.google.com/cloud-messaging/
[lnk-apple-push]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-iot-edge]: https://github.com/Azure/iot-edge
[lnk-send-messages]: iot-hub-devguide-messaging.md
[lnk-device-management]: iot-hub-device-management-overview.md

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[lnk-security-ground-up]: iot-hub-security-ground-up.md
