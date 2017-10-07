---
title: "Průvodce aaaDeveloper pro Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře pro službu Azure IoT Hub Hello zahrnuje diskusí koncových bodů, zabezpečení, hello registru identit, správu zařízení, přímé metody, dvojčata zařízení, nahrávání souborů, úlohy, hello dotazovacího jazyka pro IoT Hub a zasílání zpráv."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: d534ff9d-2de5-4995-bb2d-84a02693cb2e
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: 2d3f18399e4cef6f9c4850a5caeb170a2d091a35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-hub-developer-guide"></a>Příručka vývojáře pro službu Azure IoT Hub

Azure IoT Hub je plně spravovaná služba, která pomáhá povolit spolehlivou a zabezpečenou obousměrnou komunikaci mezi miliony zařízení a back-end řešení.

Azure IoT Hub nabízí:

* Zabezpečená komunikace pomocí přihlašovacích údajů na zařízení zabezpečení a řízení přístupu.
* Více možností velkého rozsahu komunikace zařízení cloud a cloud zařízení.
* Dotazovatelné úložiště informace o stavu licence vázané na zařízení a metadata.
* Připojení snadno zařízení s knihovny zařízení pro hello Nejoblíbenější jazyky a platformy.

Tato příručka vývojáře IoT Hub obsahuje hello následující články:

* [Komunikace zařízení cloud pokyny] [ lnk-d2c-guidance] pomůže vám zvolit zpráv typu zařízení cloud, dvojče zařízení hlášené vlastností a nahrávání souborů.
* [Cloud zařízení komunikace pokyny] [ lnk-c2d-guidance] pomůže vám zvolit přímé metody dvojče zařízení požadované vlastnosti a zprávy typu cloud zařízení.
* [Zařízení cloud a cloud zařízení zasílání zpráv službou IoT Hub] [ devguide-messaging] popisuje hello zasílání zpráv funkce (zařízení cloud a z cloudu do zařízení), které IoT Hub zpřístupní.
  * [Odesílání zpráv typu zařízení cloud tooIoT rozbočovače][devguide-messages-d2c].
  * [Číst zprávy typu zařízení cloud ze vestavěným koncovým bodem hello][devguide-builtin].
  * [Použít vlastní koncové body a pravidla směrování pro zprávy typu zařízení cloud][devguide-custom].
  * [Odesílání zpráv typu cloud zařízení ze služby IoT Hub][devguide-messages-c2d].
  * [Vytvoření a čtení zpráv služby IoT Hub][devguide-format].
* [Odeslání souborů ze zařízení] [ devguide-upload] popisuje, jak mohou odesílat soubory ze zařízení. Hello článku také obsahuje informace o oblastech, jako je hello, že proces odesílání hello oznámení můžete odesílat.
* [Správa identit zařízení IoT hub] [ devguide-identities] popisuje, jaké informace IoT hub úložiště registru identit a jak můžete používat a upravit ho.
* [Řízení přístupu tooIoT rozbočovače] [ devguide-security] popisuje hello zabezpečení modelu použitému toogrant tooIoT rozbočovače funkce přístupu pro zařízení a součásti cloudu. Hello článek obsahuje informace o používání tokeny a certifikáty X.509 a podrobnosti o hello oprávnění, které můžete udělit.
* [Používání stavu toosynchronize dvojčata zařízení a konfigurací] [ devguide-device-twins] popisuje hello *dvojče zařízení* koncept a hello funkce zpřístupňuje například synchronizace zařízení s jeho zařízení Twin. Hello článek obsahuje informace o hello data uložená v dvojče zařízení.
* [Volání metody přímé na zařízení] [ devguide-directmethods] popisuje životního cyklu hello přímé metody, informace o jak v zařízení z vaší aplikace a popisovač hello back-end metody tooinvoke přímá metoda ve vašem zařízení.
* [Plánování úloh na několika zařízeních] [ devguide-jobs] popisuje, jak lze naplánovat úlohy na několika zařízeních. Hello článek popisuje, jak toosubmit úlohy provádět úkoly, jako je spuštění přímé metody, aktualizace zařízení pomocí dvojče zařízení. Také popisuje, jak tooquery hello stav úlohy.
* [Reference – volba komunikační protokol] [ devguide-protocol] popisuje hello komunikační protokoly, podporuje komunikaci zařízení IoT Hub a seznamy hello porty, které by se mělo otevřít.
* [Odkaz – koncové body centra IoT] [ devguide-endpoints] popisuje hello různých koncových bodů, které každý IoT hub zpřístupní pro operace runtime a správy. článek Hello také popisuje, jak můžete vytvořit další koncové body ve službě IoT hub a jak toouse pole brány tooenable zařízení připojení tooyour IoT Hub koncových bodů v nestandardním scénáře.
* [Referenční dokumentace - IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv] [ devguide-query] popisuje této služby IoT Hub dotazovací jazyk, který vám umožní tooretrieve informace z vašeho centra o úlohách a dvojčata zařízení.
* [Referenční dokumentace - kvót a omezování] [ devguide-quotas] shrnuje hello kvóty nastavené v hello služby IoT Hub a hello omezení chování toosee můžete očekávat, když překročí kvótu.
* [Reference – ceny] [ devguide-pricing] obsahuje obecné informace o různých SKU a cenách služby IoT Hub a podrobnosti o tom, jak hello různé funkce IoT Hub se měří jako zprávy službou IoT Hub.
* [Referenční dokumentace - zařízení a služby sady SDK] [ devguide-sdks] seznamy hello SDK služby Azure IoT můžete použít toodevelop zařízení a služby aplikace, které spolupracují se službou IoT hub. Hello článek obsahuje odkazy tooonline rozhraní API dokumentaci.
* [Odkaz – Podpora IoT Hub MQTT] [ devguide-mqtt] poskytuje podrobné informace o tom, jak IoT Hub podporuje protokol MQTT hello. Hello článek popisuje podporu hello hello MQTT protokol předdefinované toohello SDK služby Azure IoT a obsahuje informace o použití protokolu MQTT hello přímo.
* [Glosář] [ devguide-glossary] seznam běžných termínů spojených se IoT Hub.

[devguide-messaging]: iot-hub-devguide-messaging.md
[devguide-upload]: iot-hub-devguide-file-upload.md
[devguide-identities]: iot-hub-devguide-identity-registry.md
[devguide-security]: iot-hub-devguide-security.md
[devguide-device-twins]: iot-hub-devguide-device-twins.md
[devguide-directmethods]: iot-hub-devguide-direct-methods.md
[devguide-jobs]: iot-hub-devguide-jobs.md
[devguide-endpoints]: iot-hub-devguide-endpoints.md
[devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[devguide-query]: iot-hub-devguide-query-language.md
[devguide-sdks]: iot-hub-devguide-sdks.md
[devguide-mqtt]: iot-hub-mqtt-support.md
[devguide-glossary]: iot-hub-devguide-glossary.md
[devguide-pricing]: iot-hub-devguide-pricing.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[devguide-messages-d2c]: iot-hub-devguide-messages-d2c.md
[devguide-builtin]: iot-hub-devguide-messages-read-builtin.md
[devguide-custom]: iot-hub-devguide-messages-read-custom.md
[devguide-messages-c2d]: iot-hub-devguide-messages-c2d.md
[devguide-format]: iot-hub-devguide-messages-construct.md
[devguide-protocol]: iot-hub-devguide-protocols.md
