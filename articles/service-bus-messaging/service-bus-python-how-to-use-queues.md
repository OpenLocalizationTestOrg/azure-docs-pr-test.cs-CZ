---
title: aaaHow toouse Azure Service Bus fronty s Pythonem | Microsoft Docs
description: "Zjistěte, jak fronty Azure Service Bus toouse z Pythonu."
services: service-bus-messaging
documentationcenter: python
author: sethmanheim
manager: timlt
editor: 
ms.assetid: b95ee5cd-3b31-459c-a7f3-cf8bcf77858b
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm;lmazuel
ms.openlocfilehash: bceb84d04ff3445c3087a9c246c583d6630f07af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-queues-with-python"></a>Jak toouse Service Bus fronty s Pythonem

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Tento článek popisuje, jak toouse fronty Service Bus. Hello ukázky jsou napsané v Pythonu a používají hello [balíček Python Azure Service Bus][Python Azure Service Bus package]. Hello pokryté scénáře zahrnují **vytváření front, odesílání a přijímání zpráv**, a **odstraňování front**.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

> [!NOTE]
> tooinstall Python nebo hello [balíček Python Azure Service Bus][Python Azure Service Bus package], najdete v části hello [Průvodce instalací Python](../python-how-to-install.md).
> 
> 

## <a name="create-a-queue"></a>Vytvoření fronty
Hello **ServiceBusService** objekt vám umožní toowork s fronty. Přidejte následující kód v horní hello jakéhokoliv Python souboru, ve kterém chcete tooprogrammatically přístup Service Bus hello:

```python
from azure.servicebus import ServiceBusService, Message, Queue
```

Hello následující kód vytvoří **ServiceBusService** objektu. Nahraďte `mynamespace`, `sharedaccesskeyname`, a `sharedaccesskey` se váš obor názvů, název klíče sdíleného přístupového podpisu (SAS) a hodnotou.

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Hello hodnoty hello název klíče SAS a hodnotu lze nalézt v hello [portál Azure] [ Azure portal] informace o připojení, nebo v sadě Visual Studio hello **vlastnosti** podokně při výběru Hello oboru názvů Service Bus v Průzkumníku serveru (jak je uvedeno v předchozí části hello).

```python
bus_service.create_queue('taskqueue')
```

Hello `create_queue` metoda také podporuje další možnosti, které umožňují toooverride výchozí nastavení fronty například čas toolive zprávy (TTL) nebo maximální velikost fronty. Hello následující příklad ilustruje hello fronty maximální velikost too5 GB a minutu too1 hodnotu TTL hello:

```python
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue('taskqueue', queue_options)
```

## <a name="send-messages-tooa-queue"></a>Odesílat zprávy fronty tooa
toosend fronty Service Bus zprávu tooa aplikace volá hello `send_queue_message` metodu hello **ServiceBusService** objektu.

Hello následující příklad ukazuje, jak toosend zkušební zprávu toohello frontu s názvem `taskqueue` pomocí `send_queue_message`:

```python
msg = Message(b'Test Message')
bus_service.send_queue_message('taskqueue', msg)
```

Fronty Service Bus podporují maximální velikost zprávy 256 kB v hello [úrovně Standard](service-bus-premium-messaging.md) a 1 MB hello [úroveň Premium](service-bus-premium-messaging.md). Hello hlavičky, která zahrnuje hello standard a vlastnosti vlastní aplikace, může mít maximální velikost 64 KB. Neexistuje žádné omezení na hello počet zpráv držených ve frontě, ale není na hello celková velikost hello zpráv držených ve frontě. Velikost fronty se definuje při vytvoření, maximální limit je 5 GB. Další informace o kvótách najdete v tématu [Service Bus kvóty][Service Bus quotas].

## <a name="receive-messages-from-a-queue"></a>Příjem zpráv z fronty
Přijímání zpráv z fronty pomocí hello `receive_queue_message` metodu hello **ServiceBusService** objektu:

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
print(msg.body)
```

Zprávy jsou odstraněny z fronty hello jako jejich přečtení při hello parametr `peek_lock` je nastaven příliš**False**. Můžete číst (funkce Náhled) a uzamčení uvítací zprávu bez odstranění z fronty hello parametrem hello nastavení `peek_lock` příliš**True**.

Hello chování čtení a odstranění uvítací zprávu jako součást hello přijímat operaci je hello nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat hello události selhání se zpráva nezpracuje. toounderstand, představte si třeba situaci v problémy, které příjemce hello hello přijímání požadavků a pak dojde k chybě před zpracováním ho. Protože Service Bus bude označena hello zprávu jako spotřebovávanou, pak když aplikace hello restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily uvítací zprávu, která byla spotřebované předchozí toohello havárií.

Pokud hello `peek_lock` parametr je nastaven příliš**True**, přijímat hello stane dvoufázového operace, takže je možné toosupport aplikace, které nemůžou tolerovat vynechání zpráv. Když Service Bus přijme požadavek, najde hello další zprávy toobe využívat, uzamkne ji tooprevent jinými spotřebiteli a vrátí ji toohello aplikace. Po hello aplikace dokončí zpracování zprávy hello (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze hello hello obdrží proces volání hello **odstranit** metodu hello **zpráva** objektu. Hello **odstranit** metoda bude označit uvítací zprávu jako spotřebovávanou a odeberte ji z fronty hello.

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Jak toohandle aplikace spadne a nečitelných zpráv
Service Bus poskytuje funkce toohelp, který elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy. Pokud přijímající aplikace nemůže tooprocess hello zprávy z nějakého důvodu a potom ji můžete volat hello **odemknutí** metodu hello **zpráva** objektu. To bude způsobit, že Service Bus toounlock uvítací zprávu ve frontě hello a nastavit jej jako dostupné toobe přijetí, buď pomocí hello stejné využívání aplikací nebo jinou spotřebitelskou aplikací.

Je také vypršení časového limitu přidružené zpráva uzamčená v rámci hello fronty, a pokud selže hello aplikace, které tooprocess hello zprávy před hello zámku vyprší časový limit (např. Pokud hello aplikace spadne), pak Service Bus se automatické odemknutí uvítací zprávu a nastavte jej k dispozici toobe přijetí.

V hello událost, která hello aplikace spadne po zpracování uvítací zprávu, ale před hello **odstranit** metoda je volána, pak uvítací zprávu bude víckrát toohello aplikace odešle znovu. To se často označuje jako **zpracování nejméně jednou**, který je každá zpráva se zpracuje alespoň jednou, ale v určitých situacích hello může doručit víckrát. Pokud hello scénář nemůže tolerovat zpracování duplicitní, měli vývojáři aplikace přidat další logiku tootheir aplikace toohandle víckrát doručené zprávy. To se často opírá hello **MessageId** vlastnost hello zprávy, která zůstane konstantní mezi pokusy o doručení.

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello front Service Bus, najdete v těchto článcích toolearn Další.

* [Fronty, témata a odběry][Queues, topics, and subscriptions]

[Azure portal]: https://portal.azure.com
[Python Azure Service Bus package]: https://pypi.python.org/pypi/azure-servicebus  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Service Bus quotas]: service-bus-quotas.md

