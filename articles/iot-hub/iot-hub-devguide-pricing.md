---
title: "ceny služby Azure IoT Hub aaaUnderstand | Microsoft Docs"
description: "Příručka vývojáře - informace o jak měření a ceny funguje službou IoT Hub, včetně šlo příklady."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 1ac90923-1edf-4134-bbd4-77fee9b68d24
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2016
ms.author: elioda
ms.openlocfilehash: e294c0b7f483e042ca3f63e93c14e0c2d773ae7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-hub-pricing-information"></a>Informace o cenách Azure IoT Hub

[Azure IoT Hub ceny] [ lnk-pricing] obsahuje hello obecné informace o různých SKU a ceny pro centra IoT. Tento článek obsahuje další podrobnosti o tom, jak hello různé funkce IoT Hub se měří jako zprávy službou IoT Hub.

## <a name="charges-per-operation"></a>Poplatky za operace

| Operace | Informace o fakturaci | 
| --------- | ------------------- |
| Operace registru identit <br/> (vytvořit, získat, seznam, aktualizace, odstranění) | Není účtován. |
| Zprávy typu zařízení-cloud | Úspěšně odeslané zprávy budou účtovat v bloky 4 KB na příjem příchozích dat do služby IoT Hub, například, že zprávu 6KB je účtován 2 zprávy. |
| Zprávy typu cloud zařízení | Budou se účtovat úspěšně odeslaných zpráv v 4 KB bloků, například zprávu 6 KB je účtován 2 zprávy. |
| Nahrávání souborů | Soubor přenos tooAzure úložiště není – měření podle objemu službou IoT Hub. Zprávy pro zahájení a ukončení přenosu souborů budou účtovat podle messaged měřené v přírůstcích po 4 KB. Například soubor 10 MB je účtován dvě zprávy v přidání toohello Azure Storage náklady. |
| Přímé metody | Metoda úspěšné požadavky budou účtovat v bloky 4 KB, odpovědi s neprázdný subjekty účtované v 4 KB jako další zprávy. Požadavky toodisconnected zařízení budou účtovat jako zprávy v bloky 4 KB. Například metoda subjektu 6 KB, jejímž výsledkem odpověď se žádný text ze zařízení hello účtován jako dvě zprávy. Metoda s 6 KB textu, jejímž výsledkem 1 KB odezvu hello zařízení účtován jako dvě zprávy pro požadavek hello plus další zprávu pro odpověď hello. |
| Čtení dvojčat zařízení | Dvojče zařízení přečte z hello zařízení a řešení hello zpět end účtujeme jako zprávy v bloky o velikosti 512 bajtů. Například čtení twin 6 KB zařízení je účtován jako 12 zprávy. |
| Aktualizace dvojče zařízení (značky a vlastnosti) | Aktualizace zařízení twin hello zařízení a zařízení hello budou účtovat jako zprávy v bloky o velikosti 512 bajtů. Například čtení twin 6 KB zařízení je účtován jako 12 zprávy. |
| Dotazy twin zařízení | Dotazy budou účtovat jako zprávy v závislosti na velikosti výsledek hello v bloky o velikosti 512 bajtů. |
| Operace úloh <br/> (vytvoření, aktualizace, výpis, odstranění) | Není účtován. |
| Operace úlohy na zařízení | Budou se účtovat normální úlohy operací (např. aktualizace twin zařízení a metody). Úlohu výsledkem volání metody 1000 s požadavky 1 KB a prázdný text odpovědi je pro instanci účtovat 1000 zprávy. |

> [!NOTE]
> Všech velikostí se vypočítávají zvažování hello velikost datové části v bajtech (protokol rámcovacích bude ignorován). V případě zprávy (která mají vlastnosti a text) hello velikost je počítaný způsobem, bez ohledu na protokol, jak je popsáno v hello [IoT Hub – Příručka vývojáře pro zasílání zpráv][lnk-message-size].

## <a name="example-1"></a>Příklad #1

Zařízení odesílá jednu zprávu typu zařízení cloud 1 KB za minutu tooIoT rozbočovače, který je pak přečte Azure Stream Analytics. back-end Hello řešení volá metodu (s odebranou datovou částí 512 bajtů) na zařízení hello každých 10 minut tootrigger určité akce. Hello zařízení odpovídá toohello metoda s tím výsledkem 200 bajtů.

Hello zařízení spotřebovává 1 zpráva * 60 minut * 24 hodin = 1440 zprávy na každý den pro hello zpráv typu zařízení cloud a 2 požadavku a odpovědi * 6krát za hodinu * 24 hodin = 288 zprávy pro metody hello celkem. 1728 zpráv za den.

## <a name="example-2"></a>Příklad #2

Zařízení odesílá jednu zprávu 100 KB zařízení cloud každou hodinu. Aktualizuje také jeho dvojče zařízení s datových částí 1 KB každé 4 hodiny. řešení Hello zpět končit jednou za den, dvojče zařízení 14 KB hello čtení a aktualizuje se konfigurace toochange datových částí 512 bajtů.

Hello zařízení spotřebovává 25 zprávy (100KB nebo 4KB) * 24 hodin pro zprávy typu zařízení cloud, plus 1 zpráva * 6krát na každý den pro aktualizace zařízení twin, celkem 156 zpráv za den.
využívá dvojče zařízení hello tooread 28 zprávy (14KB/0,5 KB), plus 1 zpráva tooupdate zprostředkovatele Hello back-end řešení je celkem 29 zprávy.

Celkem hello zařízení a back-end hello řešení využívat 185 zpráv za den.


[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-message-size]: iot-hub-devguide-messages-construct.md
