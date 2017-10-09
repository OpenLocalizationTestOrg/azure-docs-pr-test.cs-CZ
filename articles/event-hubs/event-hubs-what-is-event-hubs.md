---
title: "aaaWhat je Azure Event Hubs a proč ji používat | Microsoft Docs"
description: "Přehled a úvod tooAzure Event Hubs - cloudového škálovatelného přijímání telemetrie z webů, aplikací a zařízení"
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: sethm; babanisa
ms.openlocfilehash: f6199a2e5bee8506f529b6f561234d79f9c8d465
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-event-hubs"></a>Co je služba Event Hubs?

Azure Event Hubs je vysoce škálovatelná platforma pro streamování dat a služba pro ingestování událostí, která je schopná přijmout a zpracovat miliony událostí za sekundu. Služba Event Hubs dokáže zpracovávat a ukládat události, data nebo telemetrické údaje produkované distribuovaným softwarem a zařízeními. Data se odešlou tooan centra událostí, lze je transformovat a uložit pomocí jakékoli zprostředkovatele datové analýzy v reálném čase nebo adaptérů pro dávkování či ukládání. S hello možnost tooprovide [publikování a odebírání](https://msdn.microsoft.com/library/aa560414.aspx) s nízkou latencí a v masivním měřítku, Event Hubs slouží jako hello "na doběhu" pro velká Data.

## <a name="why-use-event-hubs"></a>Proč používat Event Hubs

Možnosti zpracování telemetrických údajů a událostí, které služba Event Hubs nabízí, jsou zvláště užitečné pro:

* instrumentaci aplikací
* zpracování činnosti nebo pracovního postupu koncového uživatele
* scénáře související s internetem věcí (IoT)

Služba Event Hubs například umožňuje sledování chování v mobilních aplikacích, informace o datových přenosech z webových farem, zachycení událostí v hrách na konzolách nebo shromažďování telemetrických údajů z průmyslových strojů, připojených vozidel či jiných zařízení.

## <a name="azure-event-hubs-overview"></a>Přehled služby Azure Event Hubs

Vítejte v architekturách řešení hraje Služba Event Hubs běžně roli je hello "přední dveře" pro kanál událostí, často říká *přijímač událostí*. Přijímač událostí je komponenta nebo služba, která se nachází mezi zdroji událostí a příjemci událostí toodecouple hello produkce datového proudu událostí od spotřeby těchto události hello. Hello následující obrázek znázorňuje tuto architekturu:

![Event Hubs](./media/event-hubs-what-is-event-hubs/event_hubs_full_pipeline.png)

Služba Event Hubs poskytuje možnost zpracovávat datové proudy zpráv. Její charakteristiky ji ale odlišují od tradičních podnikových způsobů zasílání zpráv. Možnosti služby Event Hubs jsou vycházejí ze scénářů zpracování událostí a vysoké propustnosti. Takto se liší od služby Event Hubs [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) zasílání zpráv a neimplementuje některé možnosti hello, které jsou k dispozici pro [zasílání zpráv Service Bus](/azure/service-bus-messaging/) entitami, jako je například témata.

## <a name="event-hubs-features"></a>Funkce Event Hubs

Centra událostí obsahuje hello následující klíčové prvky:

- [**Producenti/zdroje událostí**](event-hubs-features.md#event-publishers): Entita, která odesílá data tooan události rozbočovače. Událost se publikuje prostřednictvím AMQP 1.0 nebo HTTPS.
- [**Zaznamenat**](event-hubs-features.md#capture): umožňuje toocapture Event Hubs, streamování dat a uloží jej v účtu úložiště objektů Blob v Azure.
- [**Oddíly**](event-hubs-features.md#partitions): hello umožňuje každý příjemce tooonly číst z konkrétní podmnožinu nebo oddíl datového proudu událostí.
- [**Tokeny SAS**](event-hubs-features.md#sas-tokens): identifikuje a ověřuje vydavatel události hello.
- [**Příjemci událostí:**](event-hubs-features.md#event-consumers) Entita, která čte data událostí z centra událostí. Příjemci událostí se připojují prostřednictvím protokolu AMQP 1.0. 
- [**Skupiny příjemců**](event-hubs-features.md#consumer-groups): poskytuje každý více využívání aplikace s oddělená zobrazení datového proudu událostí hello, povolení těchto příjemci tooact nezávisle.
- [**Jednotky propustnosti:**](event-hubs-features.md#capacity) Předem zakoupené jednotky kapacity. Škálování jednoho oddílu dovoluje maximálně jednu jednotku propustnosti.

Technické podrobnosti o těchto a dalších funkcí služby Event Hubs najdete v tématu hello [přehled funkcí služby Event Hubs](event-hubs-features.md). 

## <a name="next-steps"></a>Další kroky

Podrobné informace o cenách služby Event Hubs naleznete v článku o [cenách služby Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

Další informace o službě Event Hubs najdete na adrese hello následující odkazy:

* Úvodní [Kurz služby Event Hubs](event-hubs-dotnet-standard-getstarted-send.md)
* [Nejčastější dotazy k Event Hubs](event-hubs-faq.md)
* [Ukázkové aplikace, které používají službu Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)
 
 

