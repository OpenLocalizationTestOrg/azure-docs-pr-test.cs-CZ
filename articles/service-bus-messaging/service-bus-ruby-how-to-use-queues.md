---
title: aaaHow toouse Azure Service Bus fronty s Ruby | Microsoft Docs
description: "Zjistěte, jak fronty toouse Service Bus v Azure. Ukázky kódu jsou vytvořeny v Ruby."
services: service-bus-messaging
documentationcenter: ruby
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 0a11eab2-823f-4cc7-842b-fbbe0f953751
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 7270154583f98e3372e82efbb967ea7a5acd1686
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-queues-with-ruby"></a>Jak toouse Service Bus fronty s Ruby

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Tato příručka popisuje, jak toouse fronty Service Bus. Hello ukázky jsou napsané v Ruby a používají hello Azure gem. Hello pokryté scénáře zahrnují **vytváření front, odesílání a přijímání zpráv**, a **odstraňování front**. Další informace o fronty Service Bus, najdete v části hello [další kroky](#next-steps) části.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]
   
[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="how-toocreate-a-queue"></a>Jak toocreate fronty
Hello **Azure::ServiceBusService** objekt vám umožní toowork s fronty. toocreate fronty, použijte hello `create_queue()` metoda. Hello následující příklad vytvoří frontu nebo vytiskne případné chyby.

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  queue = azure_service_bus_service.create_queue("test-queue")
rescue
  puts $!
end
```

Můžete také předat **Azure::ServiceBus::Queue** objektu s další možnosti, což vám umožní toooverride hello výchozí nastavení fronty, jako je například velikost fronty toolive nebo maximální doba zprávy. Hello následující příklad ukazuje, jak tooset hello fronty maximální velikost too5 GB a čas toolive too1 minutu:

```ruby
queue = Azure::ServiceBus::Queue.new("test-queue")
queue.max_size_in_megabytes = 5120
queue.default_message_time_to_live = "PT1M"

queue = azure_service_bus_service.create_queue(queue)
```

## <a name="how-toosend-messages-tooa-queue"></a>Jak toosend tooa fronta zpráv
toosend fronty Service Bus zprávu tooa aplikace volá hello `send_queue_message()` metodu hello **Azure::ServiceBusService** objektu. Zprávy odeslané příliš (a přijaté z) jsou fronty služby Service Bus **Azure::ServiceBus::BrokeredMessage** objekty a mají sadu standardních vlastností (jako například `label` a `time_to_live`), slovník, který je použité toohold vlastní vlastnosti specifické pro aplikace a tělo s libovolnými aplikačními daty. Aplikace můžete nastavit hello těla zprávy hello předáním řetězcovou hodnotu jako uvítací zprávu a jakékoliv nezbytné vlastnosti, standardní se naplní výchozí hodnoty.

Hello následující příklad ukazuje, jak toosend zkušební zprávu toohello frontu s názvem `test-queue` pomocí `send_queue_message()`:

```ruby
message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
message.correlation_id = "test-correlation-id"
azure_service_bus_service.send_queue_message("test-queue", message)
```

Fronty Service Bus podporují maximální velikost zprávy 256 kB v hello [úrovně Standard](service-bus-premium-messaging.md) a 1 MB hello [úroveň Premium](service-bus-premium-messaging.md). Hello hlavičky, která zahrnuje hello standard a vlastnosti vlastní aplikace, může mít maximální velikost 64 KB. Neexistuje žádné omezení na hello počet zpráv držených ve frontě, ale není na hello celková velikost hello zpráv držených ve frontě. Velikost fronty se definuje při vytvoření, maximální limit je 5 GB.

## <a name="how-tooreceive-messages-from-a-queue"></a>Jak tooreceive zpráv z fronty
Přijímání zpráv z fronty pomocí hello `receive_queue_message()` metodu hello **Azure::ServiceBusService** objektu. Zprávy jsou ve výchozím nastavení, přečtěte si a uzamčení bez odstraňuje z fronty hello. Však můžete odstranit zprávy z fronty hello číst nastavení hello `:peek_lock` možnost příliš**false**.

výchozí chování Hello díky hello čtení a odstraňování dvoufázová operace, takže také je možné toosupport aplikace, které nemůžou tolerovat vynechání zpráv. Když Service Bus přijme požadavek, najde hello další zprávy toobe využívat, uzamkne ji tooprevent jinými spotřebiteli a vrátí ji toohello aplikace. Po hello aplikace dokončí zpracování zprávy hello (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze hello hello přijímat proces voláním `delete_queue_message()` metoda a poskytování toobe hello zprávu odstranit jako parametr. Hello `delete_queue_message()` metoda bude označit uvítací zprávu jako spotřebovávanou a odeberte ji z fronty hello.

Pokud hello `:peek_lock` parametr je nastaven příliš**false**, čtení a odstranění uvítací zprávu stane hello nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat není zpracování zprávy ve hello události došlo k chybě. toounderstand, představte si třeba situaci v problémy, které příjemce hello hello přijímání požadavků a pak dojde k chybě před zpracováním ho. Vzhledem k tomu, že Service Bus označila uvítací zprávu jako spotřebovávanou, když aplikace hello restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily uvítací zprávu, která byla spotřebované předchozí toohello havárií.

Hello následující příklad ukazuje, jak se proces a tooreceive zprávy, pomocí `receive_queue_message()`. Příklad Hello nejprve přijme a odstraní zprávu pomocí `:peek_lock` nastavit příliš**false**, obdrží další zprávu a pak odstraní hello zprávu pomocí `delete_queue_message()`:

```ruby
message = azure_service_bus_service.receive_queue_message("test-queue",
  { :peek_lock => false })
message = azure_service_bus_service.receive_queue_message("test-queue")
azure_service_bus_service.delete_queue_message(message)
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Jak toohandle aplikace spadne a nečitelných zpráv
Service Bus poskytuje funkce toohelp, který elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy. Pokud přijímající aplikace nemůže tooprocess hello zprávy z nějakého důvodu a potom ji můžete volat hello `unlock_queue_message()` metodu hello **Azure::ServiceBusService** objektu. Toto volání příčiny hello toounlock sběrnice zpráv ve frontě hello a nastavit jej jako dostupné toobe přijetí, buď pomocí hello stejné využívání aplikací nebo jinou spotřebitelskou aplikací.

Je také vypršení časového limitu přidružené zpráva uzamčená v rámci hello fronty, a pokud aplikace hello selže tooprocess uvítací zprávu před hello zámku vyprší časový limit (například pokud hello aplikace spadne), pak uvítací zprávu odemkne Service Bus automaticky a je k dispozici toobe přijetí.

V hello událost, která hello aplikace spadne po zpracování uvítací zprávu, ale před hello `delete_queue_message()` metoda je volána, pak uvítací zprávu je víckrát toohello aplikace odešle znovu. Tento proces se často nazývá *zpracování nejméně jednou*; to znamená, že každá zpráva se zpracuje alespoň jednou, ale v některých situacích hello může doručit víckrát. Pokud hello scénář nemůže tolerovat zpracování duplicitní, měli vývojáři aplikace přidat další logiku tootheir aplikace toohandle víckrát doručené zprávy. To se často opírá hello `message_id` vlastnost hello zprávy, která zůstává konstantní mezi pokusy o doručení.

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello front Service Bus, postupujte podle těchto odkazů toolearn Další.

* Přehled [fronty, témata a odběry](service-bus-queues-topics-subscriptions.md).
* Navštivte hello [Azure SDK pro Ruby](https://github.com/Azure/azure-sdk-for-ruby) úložišti na Githubu.

Porovnání mezi fronty Azure Service Bus hello popsané v tomto článku a fronty Azure, které jsou popsané v hello [jak toouse úložiště Queue z Ruby](../storage/queues/storage-ruby-how-to-use-queue-storage.md) článku najdete v tématu [fronty Azure a Azure fronty služby Service Bus - porovnání a kontrastní](service-bus-azure-and-service-bus-queues-compared-contrasted.md)

