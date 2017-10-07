---
title: "předávání aaaAuto entity zasílání zpráv Azure Service Bus | Microsoft Docs"
description: "Jak toochain Service Bus fronty nebo fronty odběru tooanother nebo téma."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: f7060778-3421-402c-97c7-735dbf6a61e8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm
ms.openlocfilehash: af18273e4e2f81c5363eb830c7decf313afd8307
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="chaining-service-bus-entities-with-auto-forwarding"></a>Řetězení entit služby Service Bus s automatické přesměrování

Hello Service Bus *automatické přesměrování* funkce vám umožní toochain fronty nebo fronty odběru tooanother nebo téma, které je součástí hello stejný obor názvů. Pokud je povoleno automatické přesměrování, Service Bus automaticky odebere zprávy, které jsou umístěny v první fronty hello nebo předplatné (zdroj) a umístí je do hello druhý fronta nebo téma (cíl). Všimněte si, že je stále možné toosend zpráva toohello cílové entity přímo. Navíc není možné toochain dílčí fronta, například fronty nedoručených zpráv, tooanother fronta nebo téma.

## <a name="using-auto-forwarding"></a>Pomocí automatické přesměrování
Můžete povolit automatické přesměrování nastavení hello [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] nebo [SubscriptionDescription.ForwardTo] [ SubscriptionDescription.ForwardTo] Vlastnosti hello [QueueDescription] [ QueueDescription] nebo [SubscriptionDescription] [ SubscriptionDescription] objekty pro zdroj hello jako hello Následující příklad.

```csharp
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

Hello cílové entity, musí existovat ve hello dobu, kdy se vytvoří hello zdrojové entity. Pokud hello Cílová entita neexistuje, vrátí Service Bus výjimku, pokud kladené toocreate hello zdrojové entity.

Můžete použít automatické přesměrování tooscale na jednotlivé tématu. Service Bus omezení hello [počet předplatných na dané téma](service-bus-quotas.md) too2, 000. Další odběry zvládne vytvořením témata druhé úrovně. Všimněte si, že i v případě, že nejsou svázaná s hello může zvýšit omezení hello počtu předplatných, přidání druhou úroveň témat Service Bus hello celkovou propustnost téma.

![Scénáře automatické přesměrování][0]

Můžete také použít odesílatelé zpráv toodecouple automatické přesměrování z příjemců. Představte si třeba ERP systém, který se skládá ze tří modulů: pořadí zpracování, Správa inventáře a řízení vztahů se zákazníky. Každý z těchto modulů generuje zprávy, které jsou zařazených do fronty do příslušné téma. Alice a Bob jsou obchodní zástupce, kteří se chtějí všechny zprávy, které se týkají tootheir zákazníků. tooreceive ty zprávy, Alice a Bob vytvořit osobní fronty a odběru na každém hello ERP témat, která automaticky předávat všechny tootheir fronty zpráv.

![Scénáře automatické přesměrování][1]

Pokud Alice přejde na dovolenou, svůj osobní fronty, nikoli hello ERP tématu, zaplní. V tomto scénáři protože obchodním zástupcem neobdržel všechny zprávy žádné témat ERP hello někdy dosáhnout kvóty.

## <a name="auto-forwarding-considerations"></a>Důležité informace o automatické přesměrování

Pokud hello cílové entity shromažďuje příliš mnoho zpráv a překračuje kvótu hello nebo hello cílové entity je zakázaná, přidá hello zdrojové entitě hello zprávy tooits [frontu nedoručených zpráv](service-bus-dead-letter-queues.md) dokud není místa v cílovém umístění hello (nebo entita hello je znovu povolit). Tyto zprávy bude pokračovat toolive do fronty nedoručených zpráv hello, takže se musí explicitně zobrazí a jejich zpracování z fronty nedoručených zpráv hello.

Pokud řetězení společně jednotlivých témata tooobtain složené téma se mnoho odběrů, doporučuje se, že máte středně velkým počtem odběry tématu první úrovně hello a mnoho odběrů na témata hello druhé úrovně. Například první úrovně téma s 20 odběry, každý z nich zřetězené tooa téma druhé úrovně se 200 odběry, umožňuje vyšší propustnost než první úrovně téma se 200 předplatná každý zřetězené tooa téma druhé úrovně se 20 odběrů .

Service Bus bills jednu operaci pro každý přesměrovaná zpráva. Například odesílání zpráv tooa téma s 20 odběry, každý z nich nakonfigurován tooauto předání zprávy tooanother fronta nebo téma, se fakturuje jako 21 operace Pokud všechny odběry první úrovně obdrží kopii zprávy hello.

toocreate předplatné, které je zřetězené tooanother fronta nebo téma, musí mít hello Tvůrce odběru hello **spravovat** oprávnění na hello zdrojové i cílové entity hello. Odeslání zprávy toohello zdroj tématu vyžaduje pouze **odeslat** oprávnění k tématu zdroj hello.

## <a name="next-steps"></a>Další kroky

Podrobné informace o automatické přesměrování najdete v části hello následující odkazy na témata:

* [ForwardTo][QueueDescription.ForwardTo]
* [QueueDescription][QueueDescription]
* [SubscriptionDescription][SubscriptionDescription]

toolearn Další informace o vylepšení výkonu služby Service Bus, najdete v části 

* [Osvědčené postupy pro zlepšení výkonu pomocí zasílání zpráv Service Bus](service-bus-performance-improvements.md)
* [Segmentované entity zasílání zpráv][Partitioned messaging entities].

[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto#Microsoft_ServiceBus_Messaging_QueueDescription_ForwardTo
[SubscriptionDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.forwardto#Microsoft_ServiceBus_Messaging_SubscriptionDescription_ForwardTo
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[SubscriptionDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[0]: ./media/service-bus-auto-forwarding/IC628631.gif
[1]: ./media/service-bus-auto-forwarding/IC628632.gif
[Partitioned messaging entities]: service-bus-partitioning.md
