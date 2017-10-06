---
title: "aaaService sběrnici asynchronní zasílání zpráv | Microsoft Docs"
description: "Popis asynchronní zasílání zpráv Azure Service Bus."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: f1435549-e1f2-40cb-a280-64ea07b39fc7
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: sethm
ms.openlocfilehash: 5ab6ddf052155a9dd975b413cfaf393119c1999d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="asynchronous-messaging-patterns-and-high-availability"></a>Asynchronní schémata zasílání zpráv a vysoká dostupnost

Asynchronní zasílání zpráv, můžou se implementovat v mnoha různými způsoby. Pomocí fronty, témata a odběry Azure Service Bus podporuje asynchronism prostřednictvím dopředného mechanismus a úložiště. V běžném provozu (synchronní) odesílat zprávy tooqueues a témat a příjem zpráv z fronty a odběry. Aplikace, které můžete psát závisí na těchto entitách se vždy k dispozici. Když se změní stav entity hello kvůli tooa z různých důvodů, je nutné způsob tooprovide omezenou schopnost entita, která může stát odpovědí na většinu potřeb.

Aplikace se obvykle používají asynchronní zasílání zpráv tooenable vzory pro různé scénáře komunikace. Můžete vytvořit aplikace, ve kterých klienti odesílají zprávy tooservices, i když není spuštěná hello. Pro aplikace, které mají shluky komunikace může pomoct fronty úrovně hello načíst tím, že poskytuje komunikační toobuffer místě. Nakonec můžete získat jednoduchý, ale efektivní zatížení vyrovnávání toodistribute zprávy mezi různými počítači.

V pořadí toomaintain dostupnosti všech těchto entitách vezměte v úvahu počet různé způsoby, ve kterém může vyskytovat tyto entity není k dispozici pro odolný systém zasílání zpráv. Obecně řečeno vidíme hello entity stát není k dispozici tooapplications, kterou jsme napsali v hello následující různými způsoby:

* Nelze toosend zprávy.
* Nelze tooreceive zprávy.
* Entity nelze toomanage (vytvořit, získat, aktualizovat nebo odstranit entity).
* Služba nemůže toocontact hello.

Pro každý z těchto chyb selhání různé režimy existovat umožňující pracovní tooperform toocontinue aplikace na určité úrovni snížené schopnosti. Například systém, který může odesílat zprávy, ale není přijímat můžete nadále získávat objednávek zákazníků, ale nemůže zpracovat tyto objednávky. Toto téma popisuje potenciální problémy, ke kterým dochází, a jak jsou omezeny těchto problémů. Service Bus zavedla několika způsoby zmírnění, které musí přihlásíte k odběru a toto téma taky popisuje pravidla pro hello použít tyto způsoby zmírnění výslovný souhlas hello.

## <a name="reliability-in-service-bus"></a>Spolehlivost v Service Bus
Existuje několik způsobů toohandle zprávu a entity problémy a jsou pokyny, kterými se řídí hello příslušné použití těchto způsoby zmírnění rizik. toounderstand hello pokyny, nejprve je potřeba pochopit, co může selhat v Service Bus. Z důvodu toohello návrhu Azure systémů všechny tyto problémy jsou často toobe krátkodobou. Na vysoké úrovni hello různé příčiny nedostupnosti zobrazit takto:

* Omezení z externího systému, na kterém závisí Service Bus. Omezení dojde na interakce s úložiště a výpočetní prostředky.
* Problém pro systém, na kterém závisí Service Bus. Například určitá část úložiště může dojít k potížím.
* Chyba služby Service Bus na jednom subsystému. V takovém případě výpočetním uzlu můžete získat v nekonzistentním stavu a restartování sám sebe, způsobuje všechny entity slouží tooload vyrovnávání tooother uzlů. To zase způsobit na krátkou dobu zpracování pomalé zprávy.
* Chyba služby Service Bus v rámci datového centra Azure. Jedná se o "závažnou chybu" během které hello systému nedostupný pro mnoho minut nebo několik hodin.

> [!NOTE]
> termín Hello **úložiště** může zahrnovat Azure Storage a SQL Azure.
> 
> 

Service Bus obsahuje počet jejich zmírnění pro tyto problémy. Hello následující části popisují jednotlivé problémy a jejich odpovídajících jejich zmírnění.

### <a name="throttling"></a>Omezování
Službou Service Bus omezování umožňuje správu míra spolupráci zprávy. Každý uzel jednotlivé služby Service Bus je umístěno mnoho entit. Každý z těchto entit vytváří požadavky na hello systému z hlediska využití procesoru, paměti, úložiště a jiné omezující vlastnosti. Když některé z těchto omezující vlastnosti zjistí využití který překračuje definované prahové hodnoty, Service Bus můžete odepřít daného požadavku. volající Hello obdrží [ServerBusyException] [ ServerBusyException] a opakování za 10 sekund.

Zmírnění dopadů musí kód hello hello Chyba čtení a zastaví všechny opakování uvítací zprávu alespoň 10 sekund. Vzhledem k tomu, že hello chybě může dojít mezi částí aplikace hello zákazníka, očekává se, že každého jednotlivého nezávisle provede hello logika opakovaných pokusů. Kód Hello může snížit pravděpodobnost hello omezené tím, že umožňuje vytváření oddílů na se fronta nebo téma.

### <a name="issue-for-an-azure-dependency"></a>Problém pro Azure závislostí
Ostatní součásti v rámci Azure občas může mít problémy služby. Například pokud probíhá upgrade systému, který používá Service Bus, daný systém může dočasně mají omezenou možnosti. toowork kolem těchto typů problémů, Service Bus pravidelně prověří a implementuje způsoby zmírnění rizik. Vedlejší efekty tyto způsoby zmírnění zobrazí. Například toohandle přechodná problémů s úložištěm, Service Bus implementuje systém, který umožňuje konzistentně toowork operace odesílání zpráv. Kvůli toohello povaha zmírnění hello odeslané zprávy může trvat až minut tooappear too15 v hello vliv fronty nebo předplatné a připravený pro operace příjmu. Obecně řečeno většiny entit nebude k tomuto problému dojde. Však zadána hello počet entit v Service Bus v rámci Azure tohoto omezení je někdy potřebné pro malou zákazníků služby Service Bus.

### <a name="service-bus-failure-on-a-single-subsystem"></a>Service Bus selhání v jednom subsystému
S libovolnou aplikací okolností může způsobit interní součást Service Bus toobecome nekonzistentní. Když Service Bus to zjistí, shromažďuje data z aplikace tooaid hello při diagnostikování, co se stalo. Jakmile se hello data shromažďují, aplikace hello je restartována v tooreturn pokusu o jeho tooa konzistentním stavu. Tento proces probíhá poměrně rychle a jsou mnohem kratší výsledky v entitě zobrazování toobe není k dispozici pro až tooa několik minut, i když je typické dobu mimo provoz.

V těchto případech hello klientská aplikace generuje [System.TimeoutException] [ System.TimeoutException] nebo [MessagingException] [ MessagingException] výjimka. Service Bus obsahuje ke zmírnění tohoto problému v hello formu logika opakovaných pokusů automatizované klienta. Jakmile doba opakování hello vyčerpá a uvítací zprávu doručeno, můžete prozkoumat pomocí dalších funkcí, jako třeba [spárovat obory názvů][paired namespaces]. Obory názvů spárované mít další upozornění, které jsou popsané v tomto článku.

### <a name="failure-of-service-bus-within-an-azure-datacenter"></a>Selhání v rámci datového centra Azure Service Bus
selhání nasazení upgradu služby Service Bus nebo závislé systému je Hello nejpravděpodobnější důvod selhání v datovém centru Azure. Jak platformy hello používá, co hello pravděpodobnost selhání tohoto typu. Selhání datacenter může také vyskytnout z důvodů, které zahrnují následující hello:

* Elektrický výpadek (napájení a generování power zmizí).
* Připojení (internet zalomení mezi klienty a Azure).

V obou případech havárie přírodních nebo syntetická způsobilo problém hello. toowork řešení a ujistěte se, že může i dál posílat zprávy, můžete použít [spárovat obory názvů] [ paired namespaces] tooenable zprávy toobe odeslané tooa druhý umístění, zatímco primární umístění hello se provádí v pořádku znovu. Další informace najdete v tématu [osvědčené postupy pro izolační aplikace proti výpadkům Service Bus a havárií][Best practices for insulating applications against Service Bus outages and disasters].

## <a name="paired-namespaces"></a>Spárované obory názvů
Hello [spárovat obory názvů] [ paired namespaces] funkce podporuje scénáře, ve kterém entity služby sběrnice nebo nasazení v rámci datového centra k dispozici. Pokud k této události dochází zřídka, distribuovaných systémů stále musí být připraveny toohandle nejhorší scénářích případů. Obvykle tato událost se stane, protože některé elementu, na kterém závisí Service Bus dochází krátkodobé problém. dostupnost aplikace toomaintain během výpadku uživatelé sběrnice můžete použít dva samostatné obory názvů, pokud možno v samostatných datových centrech, toohost jejich entit pro zasílání zpráv. Hello zbytek Tato část používá hello následující terminologií:

* Primární obor názvů: hello obor názvů, se kterým komunikuje aplikace pro odesílání a příjmu operace.
* Sekundární obor názvů: hello obor názvů, který funguje jako zálohování toohello primární obor názvů. Aplikace logiky nekomunikuje s Tento obor názvů.
* Převzetí služeb při selhání interval: hello množství času tooaccept normální selhání než aplikace hello přepíná z oboru názvů hello primární obor názvů toohello sekundární.

Spárovat obory názvů podporu *odeslat dostupnosti*. Modul Send dostupnosti zachovává hello možnost toosend zprávy. dostupnost odesílání toouse, aplikace musí splňovat hello následující požadavky:

1. Zprávy jsou přijímány pouze z primární názvů hello.
2. Zprávy odeslané tooa danou fronta nebo téma může dorazí mimo pořadí.
3. Zprávy v rámci relace může být dodány ve správném pořadí. Toto je zalomení z normální funkce relací. To znamená, že vaše aplikace používá zprávy skupinu toologically relací.
4. Stav relace je zachováno pouze v oboru názvů primární hello.
5. primární fronty Hello můžete uvést do režimu online a začít přijímat zprávy před sekundární fronty hello nabízí všechny zprávy do fronty primární hello.

Hello následující oddíly popisují hello rozhraní API, jak jsou implementované hello rozhraní API a ukazuje ukázkový kód, který používá funkce hello. Všimněte si, že jsou fakturace důsledky spojené s touto funkcí.

### <a name="hello-messagingfactorypairnamespaceasync-api"></a>Hello MessagingFactory.PairNamespaceAsync rozhraní API
Funkce obory názvů spárované Hello zahrnuje hello [PairNamespaceAsync] [ PairNamespaceAsync] metodu hello [Microsoft.ServiceBus.Messaging.MessagingFactory] [ Microsoft.ServiceBus.Messaging.MessagingFactory] třídy:

```csharp
public Task PairNamespaceAsync(PairedNamespaceOptions options);
```

Po dokončení úloh hello párování hello obor názvů je také dokončení a připravené tooact při pro žádné [MessageReceiver][MessageReceiver], [QueueClient] [ QueueClient], nebo [TopicClient] [ TopicClient] vytvořené pomocí hello [MessagingFactory] [ MessagingFactory] instance. [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions] [ Microsoft.ServiceBus.Messaging.PairedNamespaceOptions] je hello základní třídu pro hello různých typů spojení, které jsou k dispozici [MessagingFactory] [ MessagingFactory] objektu. V současné době hello jen odvozené třídy je jednu s názvem [SendAvailabilityPairedNamespaceOptions][SendAvailabilityPairedNamespaceOptions], který implementuje hello odesílání dostupnosti požadavky. [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] obsahuje sadu konstruktory, které sestavení na sobě navzájem. Prohlížení hello konstruktor s hello většinu parametrů, je můžete porozumět chování hello hello další konstruktory.

```csharp
public SendAvailabilityPairedNamespaceOptions(
    NamespaceManager secondaryNamespaceManager,
    MessagingFactory messagingFactory,
    int backlogQueueCount,
    TimeSpan failoverInterval,
    bool enableSyphon)
```

Tyto parametry mají následující významy hello:

* *secondaryNamespaceManager*: inicializovali [NamespaceManager] [ NamespaceManager] instance pro obor názvů sekundární hello této hello [PairNamespaceAsync] [ PairNamespaceAsync] metoda může používat tooset až hello sekundární oboru názvů. obor názvů správce Hello je použité tooobtain hello seznam front v oboru názvů hello a toomake se, že existují nevyřízené položky fronty hello vyžaduje. Pokud tyto fronty nejsou k dispozici, jejich vytvoření. [NamespaceManager] [ NamespaceManager] vyžaduje hello možnost toocreate token s hello **spravovat** deklarací identity.
* *messagingFactory*: hello [MessagingFactory] [ MessagingFactory] instance pro obor názvů sekundární hello. Hello [MessagingFactory] [ MessagingFactory] objekt je použité toosend a v případě hello [EnableSyphon] [ EnableSyphon] vlastnost je nastavena příliš**true** , přijímat zprávy z front hello nevyřízených položek.
* *backlogQueueCount*: hello počet nevyřízených položek fronty toocreate. Tato hodnota musí být alespoň 1. Při odesílání zprávy toohello nevyřízených položek, jednu z těchto front náhodně vybere. Pokud nastavíte hodnotu too1 hello, můžete použít někdy tento pouze jedna fronta. Pokud k tomu dojde a hello jeden nevyřízené položky fronty generuje chyby, klient hello není možné tootry různých nevyřízené položky fronty a může selhat toosend zprávu. Doporučujeme tuto hodnotu toosome větší hodnotu a výchozí hello hodnota too10 nastavení. Můžete změnit tato tooa vyšší nebo nižší hodnotu v závislosti na množství dat, které vaše aplikace odešle za den. Každá fronta nevyřízené položky mohou být uloženy až too5 GB zpráv.
* *failoverInterval*: hello množství času, během kterého bude přijímat chyby na primární obor názvů hello před přepnutím jednu entitu přes toohello sekundární oboru názvů. Na základě entity entity dojde k převzetí služeb při selhání. Entity v jeden obor názvů často za provozu v různých uzlech v rámci služby Service Bus. Selhání v jedné entity neznamená selhání v jiném. Tuto hodnotu můžete nastavit příliš[System.TimeSpan.Zero] [ System.TimeSpan.Zero] toofailover toohello sekundární okamžitě po selhání vaše první, pouze dočasné. Selhání, které aktivují časovače hello převzetí služeb při selhání jsou [MessagingException] [ MessagingException] v které hello [IsTransient] [ IsTransient] vlastnost je nastavena hodnota false, nebo [System.TimeoutException][System.TimeoutException]. Ostatní výjimky, jako například [UnauthorizedAccessException] [ UnauthorizedAccessException] nezpůsobí převzetí služeb při selhání, protože označují, že klient hello není správně nakonfigurovaná. A [ServerBusyException] [ ServerBusyException] nemá příčina převzetí služeb při selhání protože správné vzor hello je toowait 10 sekund, potom uvítací zprávu znovu odešlete.
* *enableSyphon*: Určuje, že tato konkrétní párování by také syphon zprávy z primární názvů back toohello hello sekundární oboru názvů. Obecně platí, aplikace, které odesílají zprávy měli nastavit hodnotu příliš**false**; aplikace, které přijímají zprávy musí tato hodnota nastavená příliš**true**. Hello důvodem je, že často, existují méně příjemci zprávy než odesílatelé zpráv. V závislosti na hello počet příjemců můžete toohave jednu aplikaci instance popisovače hello Trativod povinností. Použití mnoha příjemci má fakturační důsledky pro každou frontu nevyřízených položek.

toouse hello kódu, vytvořit primární [MessagingFactory] [ MessagingFactory] instance, sekundární [MessagingFactory] [ MessagingFactory] instance, sekundární databázi [NamespaceManager] [ NamespaceManager] instance a [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] instance. Hello volání může být stejně jednoduché jako hello následující:

```csharp
SendAvailabilityPairedNamespaceOptions sendAvailabilityOptions = new SendAvailabilityPairedNamespaceOptions(secondaryNamespaceManager, secondary);
primary.PairNamespaceAsync(sendAvailabilityOptions).Wait();
```

Když hello Úloha vrácená hello [PairNamespaceAsync] [ PairNamespaceAsync] metoda dokončí, vše, co je nastavit a připravené toouse. Předtím, než se vrátí hello úloh, pravděpodobně nebyly dokončeny, všechny práce na pozadí hello, které jsou nezbytné pro hello párování toowork vpravo. V důsledku toho by neměl spustit, odesílání zpráv, dokud vrátí hello úloh. Pokud došlo k jeho selhání, třeba chybná přihlašovací údaje nebo selhání toocreate hello nevyřízené položky fronty, budou tyto výjimky vyvolány po dokončení úlohy hello. Po hello úkolů vrátí, ověřte, zda byly hello fronty najít nebo vytvořit tak, že prověří hello [BacklogQueueCount] [ BacklogQueueCount] vlastnost na vaše [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] instance. Pro hello předcházející kód, který operaci vypadat takto:

```csharp
if (sendAvailabilityOptions.BacklogQueueCount < 1)
{
    // Handle case where no queues were created.
}
```

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello asynchronní zasílání zpráv ve sběrnici Service Bus, přečtěte si další informace o [spárovat obory názvů][paired namespaces].

[ServerBusyException]: /dotnet/api/microsoft.servicebus.messaging.serverbusyexception
[System.TimeoutException]: https://msdn.microsoft.com/library/system.timeoutexception.aspx
[MessagingException]: /dotnet/api/microsoft.servicebus.messaging.messagingexception
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Microsoft.ServiceBus.Messaging.MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[MessageReceiver]: /dotnet/api/microsoft.servicebus.messaging.messagereceiver
[QueueClient]: /dotnet/api/microsoft.servicebus.messaging.queueclient
[TopicClient]: /dotnet/api/microsoft.servicebus.messaging.topicclient
[Microsoft.ServiceBus.Messaging.PairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.pairednamespaceoptions
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[SendAvailabilityPairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[NamespaceManager]: /dotnet/api/microsoft.servicebus.namespacemanager
[PairNamespaceAsync]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_PairNamespaceAsync_Microsoft_ServiceBus_Messaging_PairedNamespaceOptions_
[EnableSyphon]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions#Microsoft_ServiceBus_Messaging_SendAvailabilityPairedNamespaceOptions_EnableSyphon
[System.TimeSpan.Zero]: https://msdn.microsoft.com/library/system.timespan.zero.aspx
[IsTransient]: /dotnet/api/microsoft.servicebus.messaging.messagingexception#Microsoft_ServiceBus_Messaging_MessagingException_IsTransient
[UnauthorizedAccessException]: https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx
[BacklogQueueCount]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions?redirectedfrom=MSDN#Microsoft_ServiceBus_Messaging_SendAvailabilityPairedNamespaceOptions_BacklogQueueCount
[paired namespaces]: service-bus-paired-namespaces.md
