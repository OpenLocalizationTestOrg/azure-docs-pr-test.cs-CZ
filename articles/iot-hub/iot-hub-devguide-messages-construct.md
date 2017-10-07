---
title: "Formát zprávy aaaUnderstand Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře - popisuje hello formátu a očekávaný obsah zprávy IoT Hub."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3fc5f1a3-3711-4611-9897-d4db079b4250
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: 3b1567e47bc32f70c0c252996648c4035ae81243
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-read-iot-hub-messages"></a>Vytvoření a čtení zpráv služby IoT Hub

toosupport bezproblémové interoperabilita mezi protokoly, IoT Hub definuje běžné formát zprávy pro všechny protokoly zařízení přístupem. Používá se tento formát zprávy pro [zařízení cloud] [ lnk-d2c] a [cloud zařízení] [ lnk-c2d] zprávy. [IoT Hub zpráva] [ lnk-messaging] se skládá z:

* Sadu *vlastnosti systému*. Vlastnosti, které Centrum IoT interpretuje nebo nastaví. Tato sada je předem určený.
* Sadu *vlastnosti aplikace*. Slovník vlastností řetězce, které hello aplikace můžete definovat a získat přístup, a to bez nutnosti tělo zprávy toodeserialize hello. IoT Hub nikdy upravuje tyto vlastnosti.
* Neprůhledné binární text.

Názvy a hodnoty vlastností mohou obsahovat pouze alfanumerické znaky ASCII, plus ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}`` při můžete:

* Odesílání zpráv typu zařízení cloud pomocí protokolu HTTP hello.
* Odesílání zpráv typu cloud zařízení.

Další informace o tom, tooencode a dekódování zprávy odeslané pomocí různých protokolů, najdete v části [SDK služby Azure IoT][lnk-sdks].

Hello následující tabulka uvádí hello sadu vlastností systému ve zprávách služby IoT Hub.

| Vlastnost | Popis |
| --- | --- |
| messageId |Identifikátor uživatele nastavit hello zprávě použité pro vzory požadavku a odpovědi. Formát: Malá a velká písmena řetězec (až too128 znaků) z alfanumerických znaků ASCII 7bitového + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| Pořadové číslo |Číslo (jedinečný pro každé zařízení fronty) přiřazené službou IoT Hub tooeach cloud zařízení zpráv. |
| příliš|Cíl zadaný v [Cloud-zařízení] [ lnk-c2d] zprávy. |
| ExpiryTimeUtc |Datum a čas vypršení platnosti zprávy. |
| EnqueuedTime |Datum a čas hello [Cloud-zařízení] [ lnk-c2d] zpráva byla přijata službou IoT Hub. |
| CorrelationId |Vlastnosti řetězce v zprávu odpovědi, která obvykle obsahuje hello MessageId hello požadavku v vzory požadavku a odpovědi. |
| ID uživatele |ID použít toospecify hello původ zprávy. Když zprávy generované IoT Hub, se nastaví příliš`{iot hub name}`. |
| Potvrzení |Generátor zpráva zpětnou vazbu. Tato vlastnost se používá v zprávy typu cloud zařízení toorequest IoT Hub toogenerate zpětnou vazbu zprávu v důsledku hello spotřeby uvítací zprávu hello zařízení. Možné hodnoty: **žádné** (výchozí): je vygenerována žádná zpráva zpětnou vazbu, **kladné**: Pokud uvítací zprávu byl dokončen, se zobrazí zpráva zpětné vazby **záporné**: přijímat zpráva zpětné vazby, pokud platnost zprávy hello (nebo bylo dosaženo maximální doručení počet) bez jeho dokončení hello zařízení nebo **úplné**: kladné a záporné. Další informace najdete v tématu [zprávy zpětné vazby][lnk-feedback]. |
| ConnectionDeviceId |ID nastavit službou IoT Hub na zpráv typu zařízení cloud. Obsahuje hello **deviceId** hello zařízení, která odeslána zpráva hello. |
| ConnectionDeviceGenerationId |ID nastavit službou IoT Hub na zpráv typu zařízení cloud. Obsahuje hello **generationId** (dle [vlastnosti identity zařízení][lnk-device-properties]) hello zařízení, která odeslána zpráva hello. |
| ConnectionAuthMethod |Metoda ověřování, nastavit službou IoT Hub na zpráv typu zařízení cloud. Tato vlastnost obsahuje informace o hello ověřování metodu použitou tooauthenticate hello zařízení odesílání zprávy hello. Další informace najdete v tématu [zařízení toocloud ochranu proti falšování][lnk-antispoofing]. |

## <a name="message-size"></a>Velikost zpráv

IoT Hub měří velikost zprávy způsobem, bez ohledu na protokol, vzhledem k tomu jenom skutečné datové části hello. Hello velikost v bajtech se počítá jako součet hello hello následující:

* velikost textu Hello v bajtech.
* Hello velikost v bajtech všech hello hodnot vlastností hello zpráv systému.
* Hello velikost v bajtech všechny názvy vlastností uživatele a hodnoty.

Názvy a hodnoty vlastností jsou omezené tooASCII znaky, takže hello délka řetězce hello rovná hello velikost v bajtech.

## <a name="next-steps"></a>Další kroky

Informace o omezení velikosti zpráv v centru IoT najdete v tématu [IoT Hub kvót a omezování][lnk-quotas].

toolearn jak toocreate a čtení zpráv služby IoT Hub v různých programovacích jazyků, najdete v části hello [Začínáme] [ lnk-get-started] kurzy.

[lnk-messaging]: iot-hub-devguide-messaging.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-get-started]: iot-hub-get-started.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-feedback]: iot-hub-devguide-messages-c2d.md#message-feedback
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-antispoofing]: iot-hub-devguide-messages-d2c.md#anti-spoofing-properties
