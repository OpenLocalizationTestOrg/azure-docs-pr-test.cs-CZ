---
title: "Možnosti aaaAzure IoT Hub zařízení cloud | Microsoft Docs"
description: "Příručka vývojáře – pokyny k při odeslání zpráv typu zařízení cloud toouse, hlášen vlastnostech nebo soubor pro komunikaci typu cloud zařízení."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 979136db-c92d-4288-870c-f305e8777bdd
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: elioda
ms.openlocfilehash: 2caefc4f59e16ad28b0d8cf4b3bb627b4cba803b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="device-to-cloud-communications-guidance"></a>Komunikace zařízení cloud pokyny
Při odesílání informací z hello zařízení aplikaci toohello back-end řešení, IoT Hub zpřístupní tři možnosti:

* [Zprávy typu zařízení cloud] [ lnk-d2c] pro časové řady telemetrie a výstrahy.
* [Hlášené vlastnosti] [ lnk-twins] pro vytváření sestav informace o stavu zařízení například dostupné možnosti, podmínky nebo hello stavu dlouho běžící pracovních postupů. Například konfigurace a aktualizace softwaru.
* [Nahrávání souborů] [ lnk-fileupload] pro média soubory a velké telemetrie dávek odeslaný občas připojených zařízení a komprimované toosave šířky pásma.

Zde je podrobné porovnání hello různé možnosti komunikace zařízení cloud.

|  | Zprávy typu zařízení-cloud | Hlášené vlastnosti | Nahrávání souborů |
| ---- | ------- | ---------- | ---- |
| Scénář | Výstrahy a telemetrie časové řady. Například 256 KB senzor datových balíků odesílá každých 5 minut. | Podmínky a dostupné možnosti. Například hello aktuální zařízení režim připojení třeba mobilní síť nebo Wi-Fi. Synchronizace dlouho běžící pracovních postupů, jako je například konfigurace a aktualizací softwaru. | Mediálních souborů. Velké (obvykle komprimované) telemetrie dávek. |
| Ukládání a načítání | Dočasně ukládá IoT Hub až too7 dnů. Pouze sekvenční čtení. | Ukládá v hello dvojče zařízení IoT Hub. Dá načíst pomocí hello [IoT Hub dotazovací jazyk][lnk-query]. | Uložený v účtu Azure Storage zadaný uživatelem. |
| Velikost | Až too256 KB zprávy. | Maximální hlášené vlastnosti velikost je 8 KB. | Maximální velikost podporovaná technologií úložiště objektů Blob Azure. |
| frekvence | Vysoká. Další informace najdete v tématu [IoT Hub omezuje][lnk-quotas]. | Střední. Další informace najdete v tématu [IoT Hub omezuje][lnk-quotas]. | Nízká. Další informace najdete v tématu [IoT Hub omezuje][lnk-quotas]. |
| Protocol (Protokol) | K dispozici ve všech protokolů. | Aktuálně dostupná jenom při použití MQTT. | K dispozici při použití libovolný protokol, ale vyžaduje HTTP na hello zařízení. |

Je možné, že aplikace vyžaduje tooboth odeslat informace jako telemetrie časové řady nebo výstrahu a také toomake je k dispozici v dvojče zařízení hello. V tomto scénáři můžete vybrat jednu hello následující možnosti:

* Hello zařízení aplikace odešle zprávu typu zařízení cloud a změna vlastností sestavy.
* back-end Hello řešení můžete uložit hello informace ve dvojče zařízení hello značky, když obdrží uvítací zprávu.

Vzhledem k tomu, že zprávy typu zařízení cloud povolit mnohem vyšší propustnost než aktualizace twin zařízení, je někdy žádoucí tooavoid aktualizace hello zařízení twin pro každou zprávu typu zařízení cloud.


[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-fileupload]: iot-hub-devguide-file-upload.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
