---
title: "témata Service Bus toouse aaaHow (Ruby) | Microsoft Docs"
description: "Zjistěte, jak toouse Service Bus témat a odběrů v Azure. Ukázky kódu jsou napsané pro poznámky Ruby aplikace."
services: service-bus-messaging
documentationcenter: ruby
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 3ef2295e-7c5f-4c54-a13b-a69c8045d4b6
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 236d6495825e68e336c23e1b500d0764ee512e49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-ruby"></a>Jak toouse Service Bus témata a odběry s Ruby
 
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Tento článek popisuje, jak toouse Service Bus témata a odběry z Ruby aplikací. Hello pokryté scénáře zahrnují **vytváření témat a odběrů, vytváření filtrů odběrů, odesílání zpráv** tooa tématu **přijímání zpráv z odběru**, a  **Odstranění témat a odběrů**. Další informace o témat a odběrů, najdete v části hello [další kroky](#next-steps) části.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="create-a-topic"></a>Vytvoření tématu
Hello **Azure::ServiceBusService** objekt vám umožní toowork s tématy. Hello následující kód vytvoří **Azure::ServiceBusService** objektu. toocreate téma, použijte hello `create_topic()` metoda. Hello následující ukázka vytvoří téma nebo vytiskne hello chyby, pokud jsou k dispozici.

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

Můžete také předat **Azure::ServiceBus::Topic** objekt s další možnosti, které umožňují toooverride výchozí téma nastavení jako je například velikost fronty toolive nebo maximální doba zprávy. Hello následující příklad ukazuje nastavení fronty hello maximální velikosti too5 GB a času toolive too1 minutu:

```ruby
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a>Vytvořte odběr
Odběry témat taky jsou vytvořeny pomocí hello **Azure::ServiceBusService** objektu. Odběry mají názvy a můžou mít volitelné filtry, které omezuje hello sadu zpráv doručených virtuální fronty odběru toohello.

Odběry jsou trvalé a bude pokračovat tooexist, dokud nebudou buď jejich nebo hello téma jejich jsou přidruženy, budou odstraněny. Pokud vaše aplikace obsahuje logiku toocreate předplatné, musí nejprve zkontrolujte, zda hello předplatné už existuje pomocí metody getSubscription hello.

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a>Vytvoření odběru s filtrem (MatchAll) výchozí hello
Hello **MatchAll** filtr je hello výchozí filtr, který se používá v případě, že při vytvoření nového předplatného je zadán žádný filtr. Když hello **MatchAll** filtr se používá, všechny zprávy publikované toohello tématu ukládány do virtuální fronty odběru hello. Hello následující příklad vytvoří odběr s názvem "všechna zprávy" a používá hello výchozí **MatchAll** filtru.

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a>Vytvoření odběru s filtry
Můžete také definovat filtry, které umožňují toospecify příjem zpráv odeslaných tooa tématu by měl zobrazit do konkrétní předplatné.

Hello nejflexibilnější filtr, který odběry podporují je hello **Azure::ServiceBus::SqlFilter**, který implementuje je podmnožinou SQL92. Filtry SQL pracují hello vlastnosti hello zpráv, které jsou publikované toohello tématu. Další podrobnosti o hello výrazy, které lze použít s filtrem SQL, projděte si hello [SqlFilter](service-bus-messaging-sql-filter.md) syntaxe.

Filtry tooa odběru můžete přidat pomocí hello `create_rule()` metoda hello **Azure::ServiceBusService** objektu. Tato metoda umožňuje tooadd nové filtry tooan existující předplatné.

Vzhledem k tomu, že se automaticky použije výchozí filtr hello tooall nových předplatných, musíte nejprve odebrat hello výchozí filtr, nebo hello **MatchAll** přepíše ostatní filtry, můžete určit. Hello výchozí pravidla můžete odebrat pomocí hello `delete_rule()` metodu hello **Azure::ServiceBusService** objektu.

Hello následující příklad vytvoří odběr s názvem "vysoce zprávy" **Azure::ServiceBus::SqlFilter** který vybere jen zprávy, které mají vlastní `message_number` vlastnost větší než 3:

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "high-messages")
azure_service_bus_service.delete_rule("test-topic", "high-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("high-messages-rule")
rule.topic = "test-topic"
rule.subscription = "high-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number > 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Podobně hello následující příklad vytvoří odběr s názvem `low-messages` s **Azure::ServiceBus::SqlFilter** který vybere jen zprávy, které mají `message_number` vlastnost menší než nebo rovna too3:

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "low-messages")
azure_service_bus_service.delete_rule("test-topic", "low-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("low-messages-rule")
rule.topic = "test-topic"
rule.subscription = "low-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number <= 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Když je nyní odeslána zpráva příliš`test-topic`, je vždycky být toohello doručené tooreceivers odběru `all-messages` odběr tématu a odběru selektivně tooreceivers toohello `high-messages` a `low-messages` (odběry tématu v závislosti na obsahu zprávy hello).

## <a name="send-messages-tooa-topic"></a>Odeslání zprávy tooa tématu
toosend tématu zpráva tooa Service Bus, vaše aplikace musí používat hello `send_topic_message()` metodu hello **Azure::ServiceBusService** objektu. Zprávy odeslané témata tooService Bus jsou instance třídy hello **Azure::ServiceBus::BrokeredMessage** objekty. **Azure::ServiceBus::BrokeredMessage** objekty mají sadu standardních vlastností (jako například `label` a `time_to_live`), slovník, který je použité toohold vlastní vlastnosti specifické pro aplikace a tělo dat řetězce. Aplikace může nastavit hello těla zprávy hello předáním toohello hodnotu řetězec `send_topic_message()` metoda a všechny nezbytné standardní vlastnosti vyplní výchozí hodnoty.

Hello následující příklad ukazuje, jak testovací toosend pět zprávy příliš`test-topic`. Všimněte si, že hello `message_number` hodnotu vlastní vlastnosti každé zprávy se liší na hello iteraci smyčky hello (to určuje, jaké předplatné přijetí):

```ruby
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

Témata Service Bus podporují maximální velikost zprávy 256 kB v hello [úrovně Standard](service-bus-premium-messaging.md) a 1 MB hello [úroveň Premium](service-bus-premium-messaging.md). Hello hlavičky, která zahrnuje hello standard a vlastnosti vlastní aplikace, může mít maximální velikost 64 KB. Neexistuje žádné omezení na hello počet zpráv držených v tématu, ale není na hello celková velikost hello zpráv držených v tématu. Velikost tématu se definuje při vytvoření, maximální limit je 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Příjem zpráv z odběru
Přijímání zpráv z odběru pomocí hello `receive_subscription_message()` metodu hello **Azure::ServiceBusService** objektu. Ve výchozím nastavení zprávy jsou read(peak) a uzamčení bez odstranění z předplatného hello. Můžete číst a odstraňte uvítací zprávu z předplatného hello nastavení hello `peek_lock` možnost příliš**false**.

výchozí chování Hello díky hello čtení a odstraňování dvoufázová operace, takže také je možné toosupport aplikace, které nemůžou tolerovat vynechání zpráv. Když Service Bus přijme požadavek, najde hello další zprávy toobe využívat, uzamkne ji tooprevent jinými spotřebiteli a vrátí ji toohello aplikace. Po hello aplikace dokončí zpracování zprávy hello (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze hello hello přijímat proces voláním `delete_subscription_message()` metoda a poskytování toobe hello zprávu odstranit jako parametr. Hello `delete_subscription_message()` metoda bude označit uvítací zprávu jako spotřebovávanou a jeho odebrání ze hello předplatného.

Pokud hello `:peek_lock` parametr je nastaven příliš**false**, čtení a odstranění uvítací zprávu stane hello nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat není zpracování zprávy ve hello události došlo k chybě. toounderstand, představte si třeba situaci v problémy, které příjemce hello hello přijímání požadavků a pak dojde k chybě před zpracováním ho. Protože Service Bus bude označena hello zprávu jako spotřebovávanou, pak když aplikace hello restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily uvítací zprávu, která byla spotřebované předchozí toohello havárií.

Hello následující příklad ukazuje, jak můžete obdržet zprávy a zpracování pomocí `receive_subscription_message()`. Příklad Hello nejprve přijme a odstraní zprávu z hello `low-messages` předplatné pomocí `:peek_lock` nastavit příliš**false**, pak obdrží další zprávu ze hello `high-messages` a poté odstraní hello zprávu pomocí `delete_subscription_message()`:

```ruby
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Jak toohandle aplikace spadne a nečitelných zpráv
Service Bus poskytuje funkce toohelp, který elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy. Pokud přijímající aplikace nemůže tooprocess hello zprávy z nějakého důvodu a potom ji můžete volat hello `unlock_subscription_message()` metodu hello **Azure::ServiceBusService** objektu. Tato příčiny hello toounlock sběrnice zpráv v rámci předplatného hello a nastavit jej jako dostupné toobe přijetí, buď pomocí hello stejné využívání aplikací nebo jinou spotřebitelskou aplikací.

Je také přidružené zpráva uzamčená v rámci předplatného hello vypršení časového limitu, a pokud aplikace hello selže tooprocess uvítací zprávu před hello zámku vyprší časový limit (například pokud hello aplikace spadne), pak Service Bus odemknutím uvítací zprávu automaticky a nastavit jej jako dostupné toobe přijetí.

V hello událost, která hello aplikace spadne po zpracování uvítací zprávu, ale před hello `delete_subscription_message()` metoda je volána, pak uvítací zprávu je víckrát toohello aplikace odešle znovu. To se často označuje jako *zpracování nejméně jednou*; to znamená, že každá zpráva se zpracuje alespoň jednou, ale v některých situacích hello může doručit víckrát. Pokud hello scénář nemůže tolerovat zpracování duplicitní, měli vývojáři aplikace přidat další logiku tootheir aplikace toohandle víckrát doručené zprávy. Tato logika se často opírá hello `message_id` vlastnost hello zprávy, která zůstane konstantní mezi pokusy o doručení.

## <a name="delete-topics-and-subscriptions"></a>Odstranění témat a odběrů
Témata a odběry jsou trvalé a musí být explicitně odstranit buď prostřednictvím hello [portál Azure] [ Azure portal] nebo prostřednictvím kódu programu. Hello Příklad dole ukazuje, jak s názvem toodelete hello tématu `test-topic`.

```ruby
azure_service_bus_service.delete_topic("test-topic")
```

Odstraní téma také odstraní všechny odběry, které jsou registrovány hello tématu. Odběry se taky dají odstranit samostatně. Hello následující kód ukazuje, jak s názvem odběru hello toodelete `high-messages` z hello `test-topic` tématu:

```ruby
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello témat Service Bus, postupujte podle těchto odkazů toolearn Další.

* V tématu [fronty, témata a odběry](service-bus-queues-topics-subscriptions.md).
* Reference pro API pro [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter).
* Navštivte hello [Azure SDK pro Ruby](https://github.com/Azure/azure-sdk-for-ruby) úložišti na Githubu.

[Azure portal]: https://portal.azure.com
