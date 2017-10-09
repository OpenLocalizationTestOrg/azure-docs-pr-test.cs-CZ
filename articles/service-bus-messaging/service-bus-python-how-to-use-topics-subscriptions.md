---
title: "témata týkající se aaaHow toouse Azure Service Bus s Pythonem | Microsoft Docs"
description: "Zjistěte, jak toouse Azure Service Bus témata a odběry z Pythonu."
services: service-bus-messaging
documentationcenter: python
author: sethmanheim
manager: timlt
editor: 
ms.assetid: c4f1d76c-7567-4b33-9193-3788f82934e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 1171cbe8061bb3d73e2ce92ecc0cf45afae37054
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-python"></a>Jak toouse Service Bus témata a odběry s Pythonem

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Tento článek popisuje, jak toouse Service Bus témat a odběrů. Hello ukázky jsou napsané v Pythonu a používají hello [balíček Azure Python SDK][Azure Python package]. Hello pokryté scénáře zahrnují **vytváření témat a odběrů**, **vytváření filtrů odběrů**, **odesílání zpráv tooa tématu**, **přijetí zprávy z odběru**, a **odstranění témat a odběrů**. Další informace o tématech a odběrech najdete v tématu hello [další kroky](#next-steps) části.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

> [!NOTE] 
> Pokud potřebujete tooinstall Python nebo hello [balíček Azure Python][Azure Python package], najdete v části hello [Průvodce instalací Python](../python-how-to-install.md).

## <a name="create-a-topic"></a>Vytvoření tématu
Hello **ServiceBusService** objekt vám umožní toowork s tématy. Přidejte následující hello v horní hello jakéhokoliv Python souboru, ve kterém chcete tooprogrammatically přístup sběrnice:

```python
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

Hello následující kód vytvoří **ServiceBusService** objektu. Nahraďte `mynamespace`, `sharedaccesskeyname`, a `sharedaccesskey` s vašeho oboru názvů skutečný název a klíč hodnotu klíče sdíleného přístupového podpisu (SAS).

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Hello hodnoty pro hello název klíče SAS a hodnotu můžete získat z hello [portál Azure][Azure portal].

```python
bus_service.create_topic('mytopic')
```

Hello `create_topic` metoda také podporuje další možnosti, které umožňují toooverride výchozí téma nastavení jako je například velikost tématu toolive nebo maximální doba zprávy. Hello následující příklad ilustruje hello tématu maximální velikost too5 GB a hodnotu času toolive (TTL), 1 minuta:

```python
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Vytvořte odběr
Odběry tootopics jsou také vytvořené pomocí hello **ServiceBusService** objektu. Odběry mají názvy a můžou mít volitelné filtry, které omezuje hello sadu zpráv doručených virtuální fronty odběru toohello.

> [!NOTE]
> Odběry jsou trvalé a bude pokračovat tooexist, dokud nebudou buď jejich nebo hello tématu toowhich jejich přihlášeni, jsou odstraněny.
> 
> 

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a>Vytvoření odběru s filtrem (MatchAll) výchozí hello
Hello **MatchAll** filtr je hello výchozí filtr, který se používá v případě, že při vytvoření nového předplatného je zadán žádný filtr. Když hello **MatchAll** filtr se používá, všechny zprávy publikované toohello tématu ukládány do virtuální fronty odběru hello. Hello následující příklad vytvoří odběr s názvem `AllMessages` a používá hello výchozí **MatchAll** filtru.

```python
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a>Vytvoření odběru s filtry
Můžete také definovat filtry, které umožňují toospecify příjem zpráv odeslaných tooa tématu by měla objeví v konkrétním odběru tématu.

je technologie Hello nejflexibilnější filtr, který odběry podporují **SqlFilter**, který implementuje je podmnožinou SQL92. Filtry SQL pracují hello vlastnosti hello zpráv, které jsou publikované toohello tématu. Další informace o hello výrazy, které lze použít s filtrem SQL najdete v tématu hello [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] syntaxe.

Filtry tooa odběru můžete přidat pomocí hello **vytvořit\_pravidlo** metoda hello **ServiceBusService** objektu. Tato metoda vám umožní tooadd nové filtry tooan existující předplatné.

> [!NOTE]
> Protože se automaticky použije výchozí filtr hello tooall nových předplatných, je nutné nejprve odebrat hello výchozí filtr nebo hello **MatchAll** přepíše ostatní filtry, můžete určit. Hello výchozí pravidla můžete odebrat pomocí hello `delete_rule` metoda hello **ServiceBusService** objektu.
> 
> 

Hello následující příklad vytvoří odběr s názvem `HighMessages` s **SqlFilter** který vybere jen zprávy, které mají vlastní `messagenumber` vlastnost větší než 3:

```python
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Podobně hello následující příklad vytvoří odběr s názvem `LowMessages` s **SqlFilter** který vybere jen zprávy, které mají `messagenumber` vlastnost menší než nebo rovna too3:

```python
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Teď, když je odeslána zpráva příliš`mytopic` vždy se dodá tooreceivers odběru toohello **AllMessages** odběr tématu a odběru selektivně tooreceivers toohello **HighMessages**  a **LowMessages** odběry témat (v závislosti na obsahu zprávy hello).

## <a name="send-messages-tooa-topic"></a>Odeslání zprávy tooa tématu
toosend tématu zpráva tooa Service Bus, vaše aplikace musí používat hello `send_topic_message` metoda hello **ServiceBusService** objektu.

Hello následující příklad ukazuje, jak testovací toosend pět zprávy příliš`mytopic`. Všimněte si, že hello `messagenumber` hodnotu vlastnosti každé zprávy se liší na hello iteraci smyčky hello (to určuje, které odběry ji přijmou):

```python
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

Témata Service Bus podporují maximální velikost zprávy 256 kB v hello [úrovně Standard](service-bus-premium-messaging.md) a 1 MB hello [úroveň Premium](service-bus-premium-messaging.md). Hello hlavičky, která zahrnuje hello standard a vlastnosti vlastní aplikace, může mít maximální velikost 64 KB. Neexistuje žádné omezení na hello počet zpráv držených v tématu, ale není na hello celková velikost hello zpráv držených v tématu. Velikost tématu se definuje při vytvoření, maximální limit je 5 GB. Další informace o kvótách najdete v tématu [Service Bus kvóty][Service Bus quotas].

## <a name="receive-messages-from-a-subscription"></a>Příjem zpráv z odběru
Přijímání zpráv z odběru pomocí hello `receive_subscription_message` metodu hello **ServiceBusService** objektu:

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

Zprávy jsou odstraněny z předplatného hello jako jejich přečtení při hello parametr `peek_lock` je nastaven příliš**False**. Můžete číst (funkce Náhled) a uzamčení uvítací zprávu bez odstranění z fronty hello parametrem hello nastavení `peek_lock` příliš**True**.

Hello chování čtení a odstranění uvítací zprávu jako součást hello přijímat operaci je hello nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat hello události selhání se zpráva nezpracuje. toounderstand, představte si třeba situaci v problémy, které příjemce hello hello přijímání požadavků a pak dojde k chybě před zpracováním ho. Protože Service Bus bude označena hello zprávu jako spotřebovávanou, pak když aplikace hello restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily uvítací zprávu, která byla spotřebované předchozí toohello havárií.

Pokud hello `peek_lock` parametr je nastaven příliš**True**, přijímat hello stane dvoufázového operace, takže je možné toosupport aplikace, které nemůžou tolerovat vynechání zpráv. Když Service Bus přijme požadavek, najde hello další zprávy toobe využívat, uzamkne ji tooprevent jinými spotřebiteli a vrátí ji toohello aplikace. Po hello aplikace dokončí zpracování zprávy hello (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze hello hello přijímat proces voláním `delete` metodu hello **zpráva** objektu. Hello `delete` metoda označí uvítací zprávu jako spotřebovávanou a odstraní ji z odběru hello.

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Jak toohandle aplikace spadne a nečitelných zpráv
Service Bus poskytuje funkce toohelp, který elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy. Pokud přijímající aplikace nemůže tooprocess hello zprávy z nějakého důvodu a potom ji můžete volat hello `unlock` metodu hello **zpráva** objektu. To bude způsobit, že Service Bus toounlock uvítací zprávu v rámci předplatného hello a nastavit jej jako dostupné toobe přijetí, buď pomocí hello stejné využívání aplikací nebo jinou spotřebitelskou aplikací.

Je také přidružené zpráva uzamčená v rámci předplatného hello vypršení časového limitu, a pokud aplikace hello selže tooprocess uvítací zprávu před hello zámku vyprší časový limit (například pokud hello aplikace spadne), pak uvítací zprávu odemkne Service Bus automaticky a je k dispozici toobe přijetí.

V hello událost, která hello aplikace spadne po zpracování uvítací zprávu, ale před hello `delete` metoda je volána, pak uvítací zprávu bude víckrát toohello aplikace odešle znovu. To se často označuje jako *zpracování nejméně jednou*, který je každá zpráva se zpracuje alespoň jednou, ale v určitých situacích hello může doručit víckrát. Pokud hello scénář nemůže tolerovat zpracování duplicitní, měli vývojáři aplikace přidat další logiku tootheir aplikace toohandle víckrát doručené zprávy. To se často opírá hello **MessageId** vlastnost hello zprávy, která zůstane konstantní mezi pokusy o doručení.

## <a name="delete-topics-and-subscriptions"></a>Odstranění témat a odběrů
Témata a odběry jsou trvalé a musí být explicitně odstranit buď prostřednictvím hello [portál Azure] [ Azure portal] nebo prostřednictvím kódu programu. Hello následující příklad ukazuje, jak s názvem toodelete hello tématu `mytopic`:

```python
bus_service.delete_topic('mytopic')
```

Odstraní téma také odstraní všechny odběry, které jsou registrovány hello tématu. Odběry se taky dají odstranit samostatně. Hello následující kód ukazuje, jak toodelete předplatné s názvem `HighMessages` z hello `mytopic` tématu:

```python
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello témat Service Bus, postupujte podle těchto odkazů toolearn Další.

* V tématu [fronty, témata a odběry][Queues, topics, and subscriptions].
* Reference pro [SqlFilter.SqlExpression][SqlFilter.SqlExpression].

[Azure portal]: https://portal.azure.com
[Azure Python package]: https://pypi.python.org/pypi/azure  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Service Bus quotas]: service-bus-quotas.md 
