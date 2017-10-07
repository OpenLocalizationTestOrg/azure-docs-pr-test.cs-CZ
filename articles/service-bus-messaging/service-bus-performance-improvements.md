---
title: "aaaBest postupy pro zlepšení výkonu pomocí Azure Service Bus | Microsoft Docs"
description: "Popisuje, jak toouse Service Bus toooptimize výkon při výměně zprostředkované zprávy."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e756c15d-31fc-45c0-8df4-0bca0da10bb2
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/10/2017
ms.author: sethm
ms.openlocfilehash: 52764d227757cbb11246675878933f21685817f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-performance-improvements-using-service-bus-messaging"></a>Osvědčené postupy pro zlepšení výkonu pomocí zasílání zpráv Service Bus

Tento článek popisuje, jak toouse [zasílání zpráv Azure Service Bus](https://azure.microsoft.com/services/service-bus/) toooptimize výkon při výměně zprostředkované zprávy. první část Hello Toto téma popisuje hello různé mechanismy, které jsou nabízeny toohelp zvýšení výkonu. Druhá část Hello obsahuje pokyny k jak hello toouse Service Bus tak, že můžete nabídnout nejlepší výkon v daném scénáři.

Hello termín "client" v tomto tématu se vztahuje tooany entity, který přistupuje k Service Bus. Klient může trvat hello role odesílatele nebo příjemce. Hello termín "sender" se používá pro Service Bus fronta nebo téma klienta, který odesílá zprávy fronty Service Bus tooa nebo téma. Hello termín "příjemce" odkazuje tooa Service Bus fronty nebo předplatného klienta, který přijímá zprávy z fronty Service Bus nebo předplatné.

Tyto části seznámí několik konceptů, že Service Bus používá toohelp výkonu.

## <a name="protocols"></a>Protokoly
Service Bus umožňuje toosend klientů a přijímat zprávy přes jeden ze tří protokolů:

1. Pokročilé zpráv služby Řízení front Protocol (AMQP)
2. Sběrnice zpráv protokolu (SBMP)
3. HTTP

Protokoly AMQP a SBMP jsou efektivnější, protože udržují hello připojení tooService sběrnice, dokud existuje objekt pro vytváření zpráv hello. Také implementuje dávkování a prefetching. Pokud není výslovně uvedeno, veškerý obsah v tomto tématu se předpokládá použití hello AMQP nebo SBMP.

## <a name="reusing-factories-and-clients"></a>Opětovné použití objektů Factory a klientů
Service Bus klient objekty, jako například [QueueClient] [ QueueClient] nebo [MessageSender][MessageSender], jsou vytvořeny pomocí [ MessagingFactory] [ MessagingFactory] objekt, který také poskytuje interní správu připojení. Zasílání zpráv objekty Factory nebo fronty, témata a předplatné klientů by neměl zavřít po odeslat zprávu a pak hello další zprávu odešlete znovu vytvořit. Zavřením instanci messagingfactory odstraní hello připojení toohello sběrnice služby a je vytvořeno nové připojení, když znovu vytvořit objekt pro vytváření hello. Navazování připojení je náročná operace, se můžete vyhnout znovu pomocí hello stejný objekt pro vytváření a klienta objekty pro více operací. Můžete bezpečně hello [QueueClient] [ QueueClient] objekt pro odesílání zpráv z více vláken a souběžných asynchronní operace. 

## <a name="concurrent-operations"></a>Souběžných operací
Provádění operace (odesílání, příjem, odstranit, apod) nějakou dobu trvá. Tentokrát zahrnuje hello zpracování operace hello podle hello služby Service Bus v přidání toohello latenci dotazů a odpovědí hello hello. tooincrease hello počet operací za čas, operace musí být spuštěn současně. Můžete provést několika různými způsoby:

* **Asynchronní operace**: hello klienta plány operations provedením asynchronní operace. Další požadavek Hello je spuštěn před dokončením hello předchozí požadavek. Hello tady je příklad operace asynchronní odesílání:
  
 ```csharp
  BrokeredMessage m1 = new BrokeredMessage(body);
  BrokeredMessage m2 = new BrokeredMessage(body);
  
  Task send1 = queueClient.SendAsync(m1).ContinueWith((t) => 
    {
      Console.WriteLine("Sent message #1");
    });
  Task send2 = queueClient.SendAsync(m2).ContinueWith((t) => 
    {
      Console.WriteLine("Sent message #2");
    });
  Task.WaitAll(send1, send2);
  Console.WriteLine("All messages sent");
  ```
  
  Toto je příklad asynchronní příjem operace:
  
  ```csharp
  Task receive1 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
  Task receive2 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
  
  Task.WaitAll(receive1, receive2);
  Console.WriteLine("All messages received");
  
  async void ProcessReceivedMessage(Task<BrokeredMessage> t)
  {
    BrokeredMessage m = t.Result;
    Console.WriteLine("{0} received", m.Label);
    await m.CompleteAsync();
    Console.WriteLine("{0} complete", m.Label);
  }
  ```
* **Více objektů Factory**: Všichni klienti (odesílatelé v přidání tooreceivers), které jsou vytvořené pomocí hello stejný objekt pro vytváření sdílené složky jednoho připojení TCP. Hello zprávy maximální propustnost je omezena hello počet operací, které můžete přejít přes toto připojení TCP. Hello propustnost, kterou lze získat pomocí jednoho factory se výrazně liší podle doby odezvy TCP a velikost zprávy. tooobtain počty vyšší propustnost, byste měli používat více objektů Factory zasílání zpráv.

## <a name="receive-mode"></a>Zobrazí režim
Při vytvoření fronty nebo předplatného klienta, můžete určit režim příjmu: *funkce Náhled zámku* nebo *přijetí a odstranění*. Výchozí Hello přijímat režimu je [PeekLock][PeekLock]. Při fungování v tomto režimu, hello klienta odešle požadavek tooreceive zprávu ze sběrnice. Po přijetí uvítací zprávu klienta hello odešle zprávu požadavku toocomplete hello.

Při nastavení hello přijímat režimu příliš[ReceiveAndDelete][ReceiveAndDelete], oba kroky se zkombinují v jedné žádosti. Tím se zkracuje hello celkový počet operací a může zvýšit hello celkovou propustnost zpráv. Tato výkonnější kompromisnímu hello riziko ztráty zprávy.

Service Bus nepodporuje transakce pro operace přijímat a odstranění. Kromě toho jsou požadovány pro všechny scénáře, ve které hello klienta chce toodefer sémantiku funkce Náhled uzamčení nebo [nedoručených zpráv](service-bus-dead-letter-queues.md) zprávu.

## <a name="client-side-batching"></a>Dávkování na straně klienta
Dávkování na straně klienta umožňuje fronta nebo téma klienta toodelay hello odesílání zpráv pro určitou dobu. Pokud klient hello odešle další zprávy během tohoto období, přenáší zprávy hello v jedné dávce. Dávkování na straně klienta rovněž způsobí, že fronty nebo předplatného klienta toobatch více **Complete** požadavky do jedné žádosti. Dávkování je dostupná jenom pro asynchronní **odeslat** a **Complete** operace. Synchronní operace jsou okamžitě se odešlou toohello služby Service Bus. Dávkování dojít k funkce Náhled nebo přijímat operace, ani dávkování dojde k do klientů.

Ve výchozím nastavení používá klienta s intervalem batch 20ms. Hello batch interval můžete změnit nastavení hello [BatchFlushInterval] [ BatchFlushInterval] vlastnost před vytvořením hello messagingfactory. Toto nastavení ovlivní všechny klienty, které jsou vytvořené pomocí tento objekt pro vytváření.

dávkování toodisable, nastavte hello [BatchFlushInterval] [ BatchFlushInterval] vlastnost příliš**TimeSpan.Zero**. Například:

```csharp
MessagingFactorySettings mfs = new MessagingFactorySettings();
mfs.TokenProvider = tokenProvider;
mfs.NetMessagingTransportSettings.BatchFlushInterval = TimeSpan.FromSeconds(0.05);
MessagingFactory messagingFactory = MessagingFactory.Create(namespaceUri, mfs);
```

Dávkování nemá vliv na hello počet operací, fakturovatelný zasílání zpráv a je dostupná jenom pro hello protokolu klienta služby Service Bus. Hello protokolu HTTP nepodporuje dávkování.

## <a name="batching-store-access"></a>Dávkování přístup k úložišti
tooincrease propustnost hello fronty, tématu nebo předplatného, Service Bus dávek více zpráv při zápisu tooits interní úložiště. Pokud je povoleno na se fronta nebo téma, zápis zpráv do úložiště hello se zpracovat v dávce. Pokud je povoleno na fronty nebo předplatného, odstranění zprávy z hello úložiště bude zpracovat v dávce. Pokud je povolený přístup dávkové úložiště pro entitu, Service Bus zpozdí operace zápisu úložiště o dané entity podle až too20ms. Další úložiště operace, ke kterým došlo během tohoto intervalu jsou přidány toohello dávky. Zpracovat v dávce ovlivňuje pouze přístup k úložišti **odeslat** a **Complete** operace; přijímat operace neovlivní. Dávkové úložiště přístup je vlastnost u entity. Dávkování dochází mezi všechny entity, které umožňují přístup dávkové úložiště.

Při vytváření nové fronty, tématu nebo předplatného, je ve výchozím nastavení povolen přístup dávkové úložiště. toodisable zpracovat v dávce přístup k úložišti, sada hello [EnableBatchedOperations] [ EnableBatchedOperations] vlastnost příliš**false** před vytvořením hello entity. Například:

```csharp
QueueDescription qd = new QueueDescription();
qd.EnableBatchedOperations = false;
Queue q = namespaceManager.CreateQueue(qd);
```

Dávkové úložiště přístup nemá vliv na hello počet operací, fakturovatelný zasílání zpráv a je vlastností fronty, tématu nebo předplatného. Je nezávislý na hello přijímat režim a hello protokol, který se používá mezi klientem a hello služby Service Bus.

## <a name="prefetching"></a>Prefetching
Prefetching umožňuje hello fronty nebo předplatného klienta tooload další zprávy ze služby hello při provádění operace příjmu. Klient Hello ukládá tyto zprávy v místní mezipaměti. Hello velikost mezipaměti hello je dáno hello [QueueClient.PrefetchCount] [ QueueClient.PrefetchCount] nebo [SubscriptionClient.PrefetchCount] [ SubscriptionClient.PrefetchCount] Vlastnosti. Každého klienta, který umožňuje prefetching udržuje vlastní mezipaměť. Mezipaměť není sdílet mezi klienty. Pokud klient hello zahájí operace příjmu a jeho mezipaměť je prázdná, hello služba přenáší dávku zpráv. Hello velikost dávky hello rovná hello velikost mezipaměti hello nebo 256 KB, podle toho, která je menší. Pokud klient hello zahájí operace příjmu a hello mezipaměti obsahuje zprávu, zpráva hello je převzat ze hello mezipaměti.

Když je prefetched zprávu, hello služby zámky hello prefetched zpráva. Tímto způsobem, nelze přijímat zprávy prefetched hello jiný příjemce. Pokud příjemce hello nelze dokončit uvítací zprávu, než vyprší platnost hello zámku, změní na uvítací zprávu k dispozici tooother příjemci. Hello prefetched kopii uvítací zprávu zůstává v mezipaměti hello. Hello příjemce, který využívá hello platnost mezipaměti kopie obdrží výjimku, když se pokusí toocomplete této zprávě. Ve výchozím nastavení zámek zprávy hello vyprší po 60 sekund. Tato hodnota může být rozšířené too5 minut. tooprevent hello spotřebu zprávy s vypršenou platností, velikost mezipaměti hello by měl vždycky být menší než hello počet zpráv, které mohou být využívány službou klienta v rámci hello je interval časového limitu zámku.

Pokud používáte hello výchozí zámku vypršení platnosti 60 sekund, správné hodnoty pro [SubscriptionClient.PrefetchCount] [ SubscriptionClient.PrefetchCount] je 20krát hello maximální počet zpracovaných položek všechny přijímačů objektu pro vytváření hello. Například objekt factory vytvoří 3 příjemci a každý příjemce může zpracovat až too10 zpráv za sekundu. Hello předběžné načtení počet nesmí být delší než 20 × 3 × 10 = 600. Ve výchozím nastavení [QueueClient.PrefetchCount] [ QueueClient.PrefetchCount] je sada too0, což znamená, že žádné další zprávy jsou načtených ze služby hello.

Prefetching zprávy o zvýšení hello celkovou propustnost pro fronty nebo předplatného, protože snižuje celkový počet operace zpráv nebo odezev hello. Načítání hello první zprávu, ale bude trvat déle (z důvodu toohello zvětšit velikost zprávy). Přijímání zpráv prefetched bude rychlejší, protože tyto zprávy již byly staženy klientem hello.

Hello time to live (TTL) vlastnosti zprávy, které se kontroluje server hello ve hello dobu, kdy hello server odešle hello zpráva toohello klienta. Klient Hello nekontroluje vlastnost TTL hello zprávy při příjmu zprávy hello. Místo toho může být přijata zpráva hello i v případě TTL dané zprávy hello byla úspěšná, zatímco uvítací zprávu se ukládá do mezipaměti klienta hello.

Prefetching nemá vliv na hello počet operací, fakturovatelný zasílání zpráv a je dostupná jenom pro hello protokolu klienta služby Service Bus. Hello protokolu HTTP nepodporuje prefetching. Prefetching je k dispozici pro synchronní a asynchronní operace příjmu.

## <a name="express-queues-and-topics"></a>Express front a témat

Expresní entity povolte vysoké propustnosti a menší latence scénáře a jsou podporovány pouze v hello zasílání zpráv úrovně Standard. Entity vytvořené v [Premium obory názvů](service-bus-premium-messaging.md) nepodporují expresní možnost hello. S expresní entity Pokud je odeslána zpráva tooa fronta nebo téma, hello zprávy se neuloží okamžitě v úložišti pro přenos zpráv hello. Místo toho do mezipaměti v paměti. Pokud zpráva zůstává ve frontě hello na několik sekund, je zapsán automaticky toostable úložiště, tedy chránit před ztrátou kvůli výpadku tooan. Zápis uvítací zprávu do mezipaměti paměti zvyšuje propustnost a snižuje latence, protože neexistuje žádný přístup toostable úložiště hello čas hello zprávy se odesílají. Zprávy, které se spotřebovávají během několika sekund nezapisují toohello úložiště pro zasílání zpráv. Hello následující ukázka vytvoří express tématu.

```csharp
TopicDescription td = new TopicDescription(TopicName);
td.EnableExpress = true;
namespaceManager.CreateTopic(td);
```

Pokud expresní entity tooan je odeslána zpráva obsahující důležité informace, které nesmí být ztraceny, hello odesílatele můžete vynutit Service Bus tooimmediately zachovat hello zpráva toostable úložiště podle nastavení hello [ForcePersistence] [ ForcePersistence] vlastnost příliš**true**.

> [!NOTE]
> Expresní entity nepodporují transakce.

## <a name="use-of-partitioned-queues-or-topics"></a>Použití oddílů fronty nebo témata
Service Bus používá interně hello stejný uzel a zasílání zpráv tooprocess úložiště a ukládat všechny zprávy pro entity přenosu zpráv (fronty nebo tématu). Oddílů fronta nebo téma, na hello je druhé straně, rozdělené mezi více uzly a úložiště pro zasílání zpráv. Oddílů fronty a témata nejen poskytne vyšší výkon než regulární fronty a témata, budou také vykazovat vyšší dostupnosti. toocreate dělené entity, sada hello [enablepartitioning je] [ EnablePartitioning] vlastnost příliš**true**, jak ukazuje následující příklad hello. Další informace o dělené entity najdete v tématu [segmentované entity zasílání zpráv][Partitioned messaging entities].

```csharp
// Create partitioned queue.
QueueDescription qd = new QueueDescription(QueueName);
qd.EnablePartitioning = true;
namespaceManager.CreateQueue(qd);
```

## <a name="use-of-multiple-queues"></a>Použití více front

Pokud není možné toouse oddílů fronta nebo téma nebo hello očekává, že zatížení nemůže zpracovávat jeden oddílů fronta nebo téma, musíte použít více entit pro zasílání zpráv. Pokud používáte více entit, vytvořte vyhrazený klienta pro každou entitu, místo použití hello stejné klienta pro všechny entity.

## <a name="development-and-testing-features"></a>Vývoj a testování funkcí

Service Bus má jednu funkci, který je používán speciálně pro vývoj který **by měl být nikdy použit ve konfigurace produkční**: [TopicDescription.EnableFilteringMessagesBeforePublishing][].

Pokud nová pravidla nebo filtry přidají toohello téma, můžete použít [TopicDescription.EnableFilteringMessagesBeforePublishing][] tooverify, který hello nové výraz filtru funguje podle očekávání.

## <a name="scenarios"></a>Scénáře
Hello následující části popisují typické scénáře zasílání zpráv a popisují nastavení hello preferované Service Bus. Propustnosti jsou klasifikovány jako malé (méně než 1 zpráv za sekundu), střední (1 zpráv za sekundu nebo větší, ale méně než 100 zpráv za sekundu) a vysokou (100 zprávy/druhý nebo vyšší). Hello počet klientů jsou klasifikovány jako malá (5 nebo méně), střední (víc než 5 ale menší než nebo rovna too20) a velké (více než 20).

### <a name="high-throughput-queue"></a>Vysokou propustností fronty
Cíl: Maximalizujte propustnost hello jedné frontě. počet Hello odesílateli a příjemci je malá.

* Oddílů frontu použijte pro lepší výkon a dostupnost.
* tooincrease hello celkové míra odesílání do fronty hello, použijte více odesílatelé toocreate objekty pro vytváření zpráv. Pro každý odesílatele použijte asynchronní operace nebo více vláken.
* tooincrease hello celkovou rychlost příjmu z fronty hello, použijte několika příjemců toocreate objekty pro vytváření zpráv.
* Použijte výhod tootake asynchronních operací dávkování na straně klienta.
* Nastavit hello dávkování intervalu too50ms tooreduce hello počet přenosů protokolu klienta služby Service Bus. Pokud jsou použity více odesílatelé, zvýšit hello dávkování too100ms intervalu.
* Ponechte povolen přístup dávkové úložiště. Tím se zvyšuje hello celkovou rychlost, jakou může být zprávy zapisovány do fronty hello.
* Nastavte hello předběžné načtení počet too20 hello míry maximální zpracování všech příjemci objekt pro vytváření. Tím se snižuje počet hello přenosy protokolu klienta služby Service Bus.

### <a name="multiple-high-throughput-queues"></a>Více vysokou propustností fronty
Cíl: Maximalizujte celkovou propustnost více front. propustnost Hello jednotlivé fronty je střední nebo vysokou.

tooobtain maximální propustnost napříč více front, použít nastavení hello uvedených toomaximize hello propustnost jedné frontě. Kromě toho použijte různé objekty Factory toocreate klienty, kteří odesílat nebo přijímat z jiné fronty.

### <a name="low-latency-queue"></a>Fronty s nízkou latencí
Cíl: Minimalizaci latence začátku do konce se fronta nebo téma. počet Hello odesílateli a příjemci je malá. propustnost Hello hello fronty je malá nebo střední.

* Oddílů frontu použijte pro lepší dostupnost.
* Zakážete dávkování na straně klienta. Hello klienta, ihned odešle zprávu.
* Zakážete přístup dávkové úložiště. Služba Hello okamžitě zapíše úložiště toohello hello zpráv.
* Pokud používáte jednoho klienta, nastavte hello předběžné načtení počet too20 časy hello zpracování míra hello příjemce. Pokud více zpráv přijaty ve frontě hello v hello stejný čas, hello Service Bus klientský protokol odesílá je vše na hello stejnou dobu. Když hello klient obdrží hello další zprávu, zpráva již v místní mezipaměti hello. Hello mezipaměti musí být malé.
* Pokud používáte více klientů, nastavte too0 počet předběžné načtení hello. Díky tomu mohou hello druhý klienta přijímat hello druhou zprávu při první klient hello stále zpracovává hello první zprávu.

### <a name="queue-with-a-large-number-of-senders"></a>Fronty s velkým počtem uživatelů
Cíl: Maximalizujte propustnost se fronta nebo téma s velkým počtem uživatelů. Každý odesílatel odešle zprávy střední míru. Hello počet příjemců je malá.

Service Bus umožňuje až tooa souběžných připojení too1000 entity pro zasílání zpráv (nebo 5000 pomocí protokolu AMQP). Tento limit je vyžadována na úrovni oboru názvů hello a fronty, témata nebo odběry jsou omezená podle hello maximální počet souběžných připojení na obor názvů. Pro fronty je toto číslo sdílet mezi odesílateli a příjemci. Pokud jsou požadovány pro odesílatelé všechna připojení 1000, měli byste nahradit hello fronty téma a v rámci jednoho předplatného. Téma přijme až too1000 souběžných připojení z odesílatelé, zatímco hello předplatné přijímá další 1 000 souběžných připojení z příjemců. Pokud více než 1 000 souběžných odesílatelé, by měli poslat hello odesílatelé toohello zpráv služby Service Bus protokol prostřednictvím protokolu HTTP.

propustnost toomaximize hello následující:

* Oddílů frontu použijte pro lepší výkon a dostupnost.
* Pokud každý odesílatele se nachází v jiném procesu, použijte pouze jeden objekt factory pro procesy.
* Použijte výhod tootake asynchronních operací dávkování na straně klienta.
* Použijte výchozí hello dávkování interval 20ms tooreduce hello počtu přenosy protokolu klienta služby Service Bus.
* Ponechte povolen přístup dávkové úložiště. Tím se zvyšuje hello celkovou rychlost, jakou může být zprávy zapisovány do hello fronta nebo téma.
* Nastavte hello předběžné načtení počet too20 hello míry maximální zpracování všech příjemci objekt pro vytváření. Tím se snižuje počet hello přenosy protokolu klienta služby Service Bus.

### <a name="queue-with-a-large-number-of-receivers"></a>Fronty s velkým počtem příjemci
Cíl: Maximalizovat hello přijímat míra fronty nebo předplatné s velký počet příjemců. Každý příjemce přijímá zprávy střední rychlostí. Hello počet odesílatelé je malá.

Service Bus umožňuje až too1000 souběžných připojení tooan entity. Pokud fronty vyžaduje více než 1 000 příjemci, měli byste nahradit hello fronty téma a více předplatných. Každý odběr může podporovat až too1000 souběžných připojení. Příjemci Alternativně můžete přistupovat hello frontě prostřednictvím hello protokolu HTTP.

propustnost toomaximize hello následující:

* Oddílů frontu použijte pro lepší výkon a dostupnost.
* Pokud každý příjemce se nachází v jiném procesu, použijte pouze jeden objekt factory pro procesy.
* Příjemci můžete použít synchronní nebo asynchronní operace. Zadaný hello mírný přijímat počet jednotlivé příjemce, dávkování na straně klienta dokončení požadavku nemá vliv na propustnost příjemce.
* Ponechte povolen přístup dávkové úložiště. Tím se snižuje hello celkové zatížení hello entity. Také snižuje hello celková rychlost, jakou může být zprávy zapisovány do hello fronta nebo téma.
* Nastavte hello předběžné načtení počet tooa malé hodnotu (například PrefetchCount = 10). To brání tomu, aby příjemci nečinnosti, zatímco ostatní příjemce máte velké množství zpráv do mezipaměti.

### <a name="topic-with-a-small-number-of-subscriptions"></a>Téma se malý počet odběrů
Cíl: Maximalizujte propustnost hello tématu s malým počtem odběry. Zprávu přijme mnoho odběrů, což znamená, že hello kombinaci přijímat míra nad všechny odběry je větší než míra odesílání hello. Hello počet odesílatelé je malá. Hello počet příjemců na předplatné je malá.

propustnost toomaximize hello následující:

* Použijte oddílů tématu pro lepší výkon a dostupnost.
* tooincrease hello celkové míra odesílání do tématu hello, použijte více odesílatelé toocreate objekty pro vytváření zpráv. Pro každý odesílatele použijte asynchronní operace nebo více vláken.
* tooincrease hello celkovou rychlost příjmu z odběru, použijte několika příjemců toocreate objekty pro vytváření zpráv. Pro každý příjemce použijte asynchronní operace nebo více vláken.
* Použijte výhod tootake asynchronních operací dávkování na straně klienta.
* Použijte výchozí hello dávkování interval 20ms tooreduce hello počtu přenosy protokolu klienta služby Service Bus.
* Ponechte povolen přístup dávkové úložiště. Tím se zvyšuje hello celkovou rychlost, jakou může být zprávy zapisovány do tématu hello.
* Nastavte hello předběžné načtení počet too20 hello míry maximální zpracování všech příjemci objekt pro vytváření. Tím se snižuje počet hello přenosy protokolu klienta služby Service Bus.

### <a name="topic-with-a-large-number-of-subscriptions"></a>Téma se velký počet odběrů
Cíl: Maximalizujte propustnost hello téma se velký počet předplatných. Zprávu přijme mnoho odběrů, což znamená, že hello kombinaci přijímat míra přes všechny odběry je mnohem větší než míra odesílání hello. Hello počet odesílatelé je malá. Hello počet příjemců na předplatné je malá.

Témata s velkým počtem odběry obvykle odhalí nízkou celkovou propustnost všechny zprávy jsou směrované tooall odběry. To je způsobeno hello skutečnost, že každou zprávu přijme mnohokrát, a všechny zprávy, které jsou obsaženy v tématu a všechny její odběry jsou uložené ve hello stejné úložiště. Předpokládá se, že hello počet odesílatelé a počet příjemců na předplatné je malá. Service Bus podporuje až too2 000 odběry na téma.

propustnost toomaximize hello následující:

* Použijte oddílů tématu pro lepší výkon a dostupnost.
* Použijte výhod tootake asynchronních operací dávkování na straně klienta.
* Použijte výchozí hello dávkování interval 20ms tooreduce hello počtu přenosy protokolu klienta služby Service Bus.
* Ponechte povolen přístup dávkové úložiště. Tím se zvyšuje hello celkovou rychlost, jakou může být zprávy zapisovány do tématu hello.
* Nastavte hello předběžné načtení počet too20 časy hello očekává přijímat míry v sekundách. Tím se snižuje počet hello přenosy protokolu klienta služby Service Bus.

## <a name="next-steps"></a>Další kroky
toolearn Další informace o optimalizaci výkonu služby Service Bus, najdete v části [segmentované entity zasílání zpráv][Partitioned messaging entities].

[QueueClient]: /dotnet/api/microsoft.servicebus.messaging.queueclient
[MessageSender]: /dotnet/api/microsoft.servicebus.messaging.messagesender
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[PeekLock]: /dotnet/api/microsoft.servicebus.messaging.receivemode
[ReceiveAndDelete]: /dotnet/api/microsoft.servicebus.messaging.receivemode
[BatchFlushInterval]: /dotnet/api/microsoft.servicebus.messaging.netmessagingtransportsettings.batchflushinterval#Microsoft_ServiceBus_Messaging_NetMessagingTransportSettings_BatchFlushInterval
[EnableBatchedOperations]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablebatchedoperations#Microsoft_ServiceBus_Messaging_QueueDescription_EnableBatchedOperations
[QueueClient.PrefetchCount]: /dotnet/api/microsoft.servicebus.messaging.queueclient.prefetchcount#Microsoft_ServiceBus_Messaging_QueueClient_PrefetchCount
[SubscriptionClient.PrefetchCount]: /dotnet/api/microsoft.servicebus.messaging.subscriptionclient.prefetchcount#Microsoft_ServiceBus_Messaging_SubscriptionClient_PrefetchCount
[ForcePersistence]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage.forcepersistence#Microsoft_ServiceBus_Messaging_BrokeredMessage_ForcePersistence
[EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning#Microsoft_ServiceBus_Messaging_QueueDescription_EnablePartitioning
[Partitioned messaging entities]: service-bus-partitioning.md
[TopicDescription.EnableFilteringMessagesBeforePublishing]: /dotnet/api/microsoft.servicebus.messaging.topicdescription.enablefilteringmessagesbeforepublishing#Microsoft_ServiceBus_Messaging_TopicDescription_EnableFilteringMessagesBeforePublishing
