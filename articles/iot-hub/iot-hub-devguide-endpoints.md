---
title: "Koncové body Azure IoT Hub aaaUnderstand | Microsoft Docs"
description: "Příručka vývojáře – referenční informace o IoT Hub směřujících zařízení a služby přístupem koncových bodů."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 57ba52ae-19c6-43e4-bc6c-d8a5c2476e95
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 8647f15d2f2a050ad5799ea82f4d2d46db0dbec1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reference---iot-hub-endpoints"></a>Odkaz – koncové body centra IoT

## <a name="iot-hub-names"></a>Názvy IoT Hub

Můžete najít název hello hello IoT hub, který je hostitelem koncové body hello portálu na hello **přehled** okno. Ve výchozím nastavení, vypadá hello název DNS služby IoT hub: `{your iot hub name}.azure-devices.net`.

Můžete použít Azure DNS toocreate vlastní název DNS pro službu IoT hub. Další informace najdete v tématu [nastavení vlastní domény tooprovide použití Azure DNS pro službu Azure](../dns/dns-custom-domain.md#azure-iot).

## <a name="list-of-built-in-iot-hub-endpoints"></a>Seznam předdefinovaných koncové body centra IoT

Azure IoT Hub je víceklientské služby, který zveřejňuje jeho funkce toovarious aktéři. Hello následující diagram znázorňuje hello různých koncových bodů, které IoT Hub zpřístupní.

![Koncové body IoT Hubu][img-endpoints]

Hello následující seznam popisuje hello koncové body:

* **Poskytovatel prostředků**. Hello IoT Hub prostředků zprostředkovatel poskytuje [Azure Resource Manager] [ lnk-arm] rozhraní. Toto rozhraní umožňuje toocreate vlastníky předplatného Azure a odstraňte centra IoT a tooupdate vlastností centra IoT. Vlastnosti služby IoT Hub řídí [zásad zabezpečení na úrovni centra][lnk-accesscontrol], nikoli řízení přístupu toodevice a funkční nastavení pro zasílání zpráv typu cloud zařízení a zařízení cloud. Hello zprostředkovatele prostředků služby IoT Hub můžete také příliš[exportovat identit zařízení][lnk-importexport].
* **Správa identit zařízení**. Každé centrum IoT zpřístupňuje řadu identit zařízení toomanage koncové body HTTP REST (vytvořit, získat, aktualizovat a odstranit). [Identit zařízení] [ lnk-device-identities] se používají pro řízení přístupu a ověřování a zařízení.
* **Správa zařízení twin**. Každý IoT hub zpřístupní sadu tooquery koncový bod HTTP REST a aktualizace service směřujících [dvojčata zařízení] [ lnk-twins] (aktualizace značky a vlastnosti).
* **Úlohy správy**. Každé centrum IoT zpřístupňuje řadu tooquery koncový bod HTTP REST služby přístupem a spravovat [úlohy][lnk-jobs].
* **Koncové body zařízení**. Pro každé zařízení v registru identit hello IoT Hub zpřístupní sada koncových bodů:

  * *Odesílání zpráv typu zařízení cloud*. Zařízení používá tento koncový bod příliš[odesílání zpráv typu zařízení cloud][lnk-d2c].
  * *Příjem zpráv typu cloud zařízení*. Zařízení používá tento koncový bod tooreceive cílem [zprávy typu cloud zařízení][lnk-c2d].
  * *Zahájení nahrávání souborů*. Zařízení používá tento koncový bod tooreceive identifikátor URI SAS úložiště Azure ze služby IoT Hub příliš[nahrát soubor][lnk-upload].
  * *Načíst a aktualizovat vlastnosti twin zařízení*. Zařízení používá tento koncový bod tooaccess jeho [dvojče zařízení][lnk-twins]na vlastnosti.
  * *Přijímat požadavky přímá metoda*. Zařízení používá tento koncový bod toolisten pro [přímá metoda][lnk-methods]na požadavky.

    Tyto koncové body jsou vystavené pomocí [protokoly MQTT v3.1.1][lnk-mqtt], HTTP 1.1 a [protokolu AMQP 1.0] [ lnk-amqp] protokoly. Je také k dispozici prostřednictvím protokolu AMQP [Websocket] [ lnk-websockets] na portu 443.

    Hello koncové body metody a dvojčata zařízení jsou k dispozici pouze při použití hello [protokoly MQTT v3.1.1] [ lnk-mqtt] protokolu.

* **Koncové body služby**. Každý IoT hub zpřístupní sada koncových bodů pro toocommunicate back-end vašeho řešení ve vašich zařízeních. S jednou výjimkou těchto koncových bodů jsou přístupné pouze pomocí hello [AMQP] [ lnk-amqp] protokolu. koncový bod volání metoda Hello vystavený přes hello protokolu HTTP.
  
  * *Příjem zpráv typu zařízení cloud*. Tento koncový bod je kompatibilní s [Azure Event Hubs][lnk-event-hubs]. Back-end služby můžete použít tooread hello [zpráv typu zařízení cloud] [ lnk-d2c] poslal zařízení. Ve službě IoT hub můžete vytvořit vlastní koncové body v přidání toothis vestavěným koncovým bodem.
  * *Zprávy typu cloud zařízení odesílat a přijímat potvrzování doručení*. Tyto koncové body povolit toosend back-end vašeho řešení spolehlivé [zprávy typu cloud zařízení][lnk-c2d], a tooreceive hello odpovídající potvrzení o doručení nebo vypršení platnosti.
  * *Přijímat oznámení souboru*. Tento koncový bod zasílání zpráv umožňuje tooreceive oznámení o při zařízení úspěšně odeslat soubor. 
  * *Přímé volání metody*. Tento koncový bod umožňuje tooinvoke back-end službu [přímá metoda] [ lnk-methods] na zařízení.
  * *Zobrazí operace sledování událostí*. Tento koncový bod vám umožní tooreceive operace sledování událostí, pokud vaše IoT hub byl nakonfigurován tooemit je. Další informace najdete v tématu [IoT Hub operations monitorování][lnk-operations-mon].

Hello [SDK služby Azure IoT] [ lnk-sdks] článek popisuje různé způsoby tooaccess hello těchto koncových bodů.

Všechny koncové body centra IoT pomocí hello [TLS] [ lnk-tls] protokol a žádný koncový bod je někdy zveřejněné na bez šifrování nezabezpečené kanály.

## <a name="custom-endpoints"></a>Vlastní koncové body

Můžete propojit stávající služby Azure v vaše předplatné tooyour IoT hub tooact jako koncové body pro směrování zpráv. Tyto koncové body fungovat jako koncové body služby a jsou použity jako jímky pro směrování zpráv. Zařízení nemůže zapisovat přímo toohello další koncové body. toolearn Další informace o směrování zpráv, najdete v položce Příručka vývojáře hello na [odesílání a přijímání zpráv službou IoT hub][lnk-devguide-messaging].

IoT Hub aktuálně podporuje hello následující služby Azure jako další koncové body:

* Event Hubs
* Fronty služby Service Bus
* Témata služby Service Bus

IoT Hub pro směrování toowork zpráva potřebuje koncové body služby toothese přístup pro zápis. Pokud nakonfigurujete koncové body prostřednictvím hello portálu Azure, se přidají hello potřebná oprávnění pro vás. Zajistěte, aby že konfigurace vaší služby toosupport hello očekává propustnost. Při první konfiguraci řešení IoT, můžete potřebovat toomonitor další koncové body a průběhu hello skutečné zátěže.

Pokud zpráva odpovídá víc tras, aby všechny bodu toohello stejný koncový bod služby IoT Hub zajišťuje koncový bod toothat zprávy pouze jednou. Proto můžete udělat není nutné tooconfigure odstranění duplicit na fronty sběrnice nebo téma. Do oddílů front oddílu spřažení zaručuje, řazení zpráv.

> [!NOTE]
> Fronty sběrnice a témata použít jako koncové body centra IoT nesmí mít **relací** nebo **duplicitní detekce** povolena. Pokud některá z těchto možností jsou povolené, hello koncový bod se zobrazí jako **Unreachable** v hello portálu Azure.

Omezení hello hello počet koncových bodů můžete přidat, naleznete v části [kvóty a omezení][lnk-devguide-quotas].

## <a name="field-gateways"></a>Pole brány

V řešení IoT *brána pole* nachází mezi zařízeními a vaše koncové body centra IoT. Je obvykle nachází zavřít tooyour zařízení. Vaše zařízení komunikují přímo s brána pole hello přes protokol podporovaný v zařízeních hello. Brána pole Hello připojí tooan koncový bod centra IoT pomocí protokolu, který je podporován službou IoT Hub. Brána pole může být vyhrazené hardwarové zařízení nebo počítač úsporného režimu softwarem vlastní brány.

Můžete použít [Azure IoT Edge] [ lnk-iot-edge] tooimplement brána pole. Okraj IoT nabízí funkce, jako je multiplexní komunikaci od více zařízení do hello stejné připojení služby IoT Hub.

## <a name="next-steps"></a>Další kroky

Zahrnout další referenční témata v této příručce pro vývojáře IoT Hub:

* [IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv][lnk-devguide-query]
* [Kvóty a omezení][lnk-devguide-quotas]
* [Podpora MQTT centra IoT][lnk-devguide-mqtt]

[lnk-iot-edge]: https://github.com/Azure/iot-edge

[img-endpoints]: ./media/iot-hub-devguide-endpoints/endpoints.png
[lnk-amqp]: https://www.amqp.org/
[lnk-mqtt]: http://mqtt.org/
[lnk-websockets]: https://tools.ietf.org/html/rfc6455
[lnk-arm]: ../azure-resource-manager/resource-group-overview.md
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/

[lnk-tls]: https://tools.ietf.org/html/rfc5246


[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-accesscontrol]: iot-hub-devguide-security.md#access-control-and-permissions
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-device-identities]: iot-hub-devguide-identity-registry.md
[lnk-upload]: iot-hub-devguide-file-upload.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md

[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[lnk-operations-mon]: iot-hub-operations-monitoring.md
