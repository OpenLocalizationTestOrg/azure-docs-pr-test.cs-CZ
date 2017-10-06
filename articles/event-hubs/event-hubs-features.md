---
title: "Přehled funkce aaaAzure Event Hubs | Microsoft Docs"
description: "Přehled a podrobnosti o funkcích Azure Event Hubs"
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2017
ms.author: sethm
ms.openlocfilehash: 8094e48abc8455ed725d4d5d3f9895f431441e2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-features-overview"></a>Přehled funkcí služby Event Hubs

Azure Event Hubs je škálovatelnou událostí zpracování služba, která ingestuje a zpracovává velké objemy události a data, s nízkou latencí a vysokou spolehlivostí. V tématu [co je Služba Event Hubs?](event-hubs-what-is-event-hubs.md) souhrnné informace služby hello.

Tento článek vychází hello informace v hello [přehled](event-hubs-what-is-event-hubs.md)a poskytuje implementace a technické podrobnosti o Event Hubs komponenty a funkce.

## <a name="event-publishers"></a>Zdroje událostí

Každá entita, která odešle centra událostí tooan dat je producent událostí nebo *vydavatel události*. Zdroje událostí mohou publikovat události pomocí protokolu HTTPS nebo AMQP 1.0. Zdroje událostí použít tooidentify tokenu sdíleného přístupového podpisu (SAS) sami tooan Centrum událostí a může mít jedinečnou identitu nebo používat společný token SAS.

### <a name="publishing-an-event"></a>Publikování události

Událost můžete publikovat prostřednictvím protokolu AMQP 1.0 nebo HTTPS. Event Hubs poskytuje [knihovny klienta a třídy](event-hubs-dotnet-framework-api-overview.md) pro publikování události tooan Centrum událostí z klientů .NET. Pro jiné moduly runtime a platformy můžete použít libovolného klienta protokolu AMQP 1.0, například [Apache Qpid](http://qpid.apache.org/). Události můžete publikovat samostatně nebo v dávce. Jedna publikace (instance dat události) je omezena limitem 256 KB – bez ohledu na to, jestli se jedná o jedinou událost nebo dávku (batch). Publikování události větší než tato prahová hodnota dojde k chybě. Je vhodné, ne o oddíly v Centru událostí hello vydavatelů toobe a zadejte tooonly *klíč oddílu* (dostupné v další části hello), nebo svoji identitu prostřednictvím tokenu SAS.

Hello volba toouse AMQP nebo HTTPS, je konkrétní toohello scénáři použití. Protokol AMQP vyžaduje hello zřízení vytvoření trvalého obousměrného soketu v přidání tootransport zabezpečení na úrovni (TLS) nebo SSL/TLS. AMQP má vyšší náklady na síť při inicializaci hello relace, ale HTTPS vyžaduje další SSL režie pro každý požadavek. AMQP má pro často používané zdroje vyšší výkon.

![Event Hubs](./media/event-hubs-features/partition_keys.png)

Služba Event Hubs zajišťuje, že všechny události, které sdílejí hodnotu klíče oddílu se dodávají v pořadí a toohello stejné oddílu. Pokud se klíče oddílů používají společně se zásadami zdroje, pak hello identity vydavatele hello a hello hodnotu klíče oddílu hello se musí shodovat. V opačném případě dojde k chybě.

### <a name="publisher-policy"></a>Zásady zdroje

Služba Event Hubs umožňuje podrobnou kontrolu nad zdroji událostí prostřednictvím *zásad zdroje*. Spuštění funkcí navržených toofacilitate velkého počtu zdroje nezávislé událostí jsou zásady vydavatele. Společně se zásadami zdroje každý vydavatel používá svůj vlastní jedinečný identifikátor při publikování události tooan se v Centru událostí pomocí hello následující mechanismus:

```
//[my namespace].servicebus.windows.net/[event hub name]/publishers/[my publisher name]
```

Nemáte názvy vydavatelů toocreate dopředu, ale musí odpovídat tokenu SAS hello používá při publikování události, v pořadí tooensure nezávislé vydavatele identity. Při použití zásad zdroje, hello **PartitionKey** toohello název vydavatele je nastavena hodnota. toowork správně, musí tyto hodnoty odpovídat.

## <a name="capture"></a>Zachycování

[Zaznamenat centra událostí](event-hubs-capture-overview.md) vám umožní hello zachycení tooautomatically streamování dat ve službě Event Hubs a uložte ho tooyour výběru účtu úložiště Blob nebo účet služby Azure Data Lake. Můžete povolit sběr dat ze hello portál Azure a zadat minimální velikost a časové okno tooperform hello zachycení. Použití funkce Capture centra událostí, zadáte vlastní účet Azure Blob Storage a kontejner, nebo účet služby Azure Data Lake, což je použité toostore hello zachycená data. Zachycená data jsou zapsána ve formátu Apache Avro hello.

## <a name="partitions"></a>Oddíly

Event Hubs poskytuje datový proud prostřednictvím schéma oddílů ve kterém každý příjemce četl pouze konkrétní podmnožinu nebo oddíl datového proudu zpráv hello zpráv. Toto schéma umožňuje vodorovné škálování zpracování událostí a poskytuje další funkce zaměřené na datový proud, které nejsou ve frontách a tématech k dispozici.

Oddíl je seřazená posloupnost událostí, která se nachází v centru událostí. Jako novější události dorazí, budou přidány toohello konec této posloupnosti. Oddíl si lze představit jako „protokol transakcí“.

![Event Hubs](./media/event-hubs-features/partition.png)

Služba Event Hubs uchovává data pro dobu nakonfigurované uchování, která platí pro všechny oddíly v Centru událostí hello. Události mizí na základě času, nemůžete je explicitně odstranit. Vzhledem k tomu, že oddíly jsou nezávislé a obsahují vlastní posloupnost dat, často se jejich velikost mění různým tempem.

![Event Hubs](./media/event-hubs-features/multiple_partitions.png)

Hello počet oddílů je zadána při vytváření a musí být mezi 2 a 32. počet oddílů Hello není změnit, takže byste měli zvážit dlouhodobé škálování při nastavování počet oddílů. Oddíly jsou data organizace mechanismus, který má vztah toohello podřízené paralelismus vyžadují přijímací aplikace. Hello počet oddílů v Centru událostí přímo souvisí toohello počet souběžných čtenářů očekáváte, že toohave. Kontaktujte tým služby Event Hubs hello můžete zvýšit hello počet oddílů nad rámec 32.

Při oddíly identifikovat a mohou být odesílány toodirectly, odesílání přímo tooa oddílu se nedoporučuje. Místo toho můžete použít konstrukce vyšší úrovně počínaje hello [vydavatel události](#event-publishers) a [kapacity](#capacity) oddíly. 

Oddíly jsou vyplněny posloupností dat událostí, která obsahuje hello textu hello události, uživatelem definované vlastnosti kontejneru objektů a dat a metadata, jako je například její posun v oddílu hello a pořadí v posloupnosti datového proudu hello.

Další informace o oddílech a hello kompromis mezi dostupnost a spolehlivost najdete v tématu hello [Průvodce programováním pro službu Event Hubs](event-hubs-programming-guide.md#partition-key) a hello [dostupnosti a konzistence v Event Hubs](event-hubs-availability-and-consistency.md) článku .

### <a name="partition-key"></a>Klíč oddílu

Můžete použít [klíč oddílu](event-hubs-programming-guide.md#partition-key) toomap příchozích dat událostí do konkrétních oddílů pro účel hello dat organizace. Hello klíč oddílu je hodnota zadaná odesílatelem předaná do centra událostí. Zpracovává se pomocí statické hašovací funkce, které vytvoří přiřazení k oddílu hello. Pokud při publikování události nezadáte klíč oddílu, použije se přiřazení metodou kruhového dotazování.

Vydavatel události Hello známa pouze svůj klíč oddílu není hello oddílu toowhich hello události publikují. Díky tomuto oddělení klíče a oddílu insulates hello odesílatele z nutnosti tooknow příliš mnoho o zpracování příjmu dat hello. Na zařízení nebo uživatel jedinečné identity díky vhodným klíčem oddílu, ale další atributy, například geography může být také použít toogroup souvisejících událostí do jednoho oddílu.

## <a name="sas-tokens"></a>Tokeny SAS

Služba Event Hubs využívá *sdílené přístupové podpisy*, které jsou k dispozici na úrovni hello obor názvů a události rozbočovače. Token SAS, který se generuje z klíče SAS, je adresa URL, která je zašifrovaná pomocí hašovacího algoritmu SHA a zakódovaná ve specifickém formátu. Pomocí názvu hello hello klíče (zásady) a hello token, Event Hubs znovu vygenerovat hello hash a umožnit jejich ověřování hello odesílatele. Za normálních okolností se tokeny SAS pro zdroje událostí vytváří jen s oprávněními pro **odesílání** na konkrétní centrum událostí. Tento mechanismus SAS token adresa URL je hello základ pro identifikaci zdroje byla zavedená v zásadách zdroje hello. Další informace o práci s tokenem SAS naleznete v tématu o [ověřování u služby Service Bus pomocí sdíleného přístupového podpisu](../service-bus-messaging/service-bus-sas.md).

## <a name="event-consumers"></a>Příjemci událostí

*Příjemcem událostí* je každá entita, která čte data událostí z centra událostí. Všichni příjemci Event Hubs připojení prostřednictvím relace protokolu AMQP 1.0 hello a události se doručují prostřednictvím hello relace, jakmile budou k dispozici. dostupnost dat nemusí Hello klienta toopoll.

### <a name="consumer-groups"></a>Skupiny příjemců

Hello mechanismus publikování/přihlášení k odběru služby Event Hubs je povolená díky *skupiny příjemců*. Skupina příjemců je zobrazení (stavu, pozice nebo posunu) celého centra událostí. Skupiny příjemců povolit různým přijímajícím aplikacím tooeach oddělená zobrazení datového proudu událostí hello a datový proud hello tooread nezávisle vlastním tempem a s vlastními posun.

V architektuře zpracování datového proudu každý aplikace pro příjem dat znamená zároveň tooa skupiny příjemců. Pokud chcete toowrite událostí data toolong termín úložiště, aplikace zapisovače úložiště je skupiny příjemců. Komplexní zpracování událostí zase může provádět jiná, samostatná skupina příjemců. K oddílům můžete přistupovat pouze prostřednictvím skupiny příjemců. Může být maximálně 5 souběžných čtenářů na oddíl na skupinu uživatelů; ale **se doporučuje, zda je na oddíl na skupinu spotřebitelů pouze jeden aktivní příjemce**. V Centru událostí je vždy jedna výchozí skupina příjemců a můžete vytvořit skupiny příjemců too20 pro centra událostí úrovně Standard.

Hello Následují příklady zápisu identifikátoru URI skupiny příjemců hello:

```http
//[my namespace].servicebus.windows.net/[event hub name]/[Consumer Group #1]
//[my namespace].servicebus.windows.net/[event hub name]/[Consumer Group #2]
```

Hello následující obrázek znázorňuje architekturu zpracování datového proudu Event Hubs hello:

![Event Hubs](./media/event-hubs-features/event_hubs_architecture.png)

### <a name="stream-offsets"></a>Posuny datového proudu

*Posun* je hello pozice události v rámci oddílu. Posun si můžete představit jako kurzor na straně klienta. Hello posun je bajt číslování hello události. Tento posun umožňuje toospecify příjemce (čtenáři) události bod v proudu událostí hello, ze kterého chtějí toobegin čtení událostí. Můžete zadat posun hello podobě časového razítka nebo hodnoty posunu. Příjemci knihovny zodpovídají za uložení svých hodnot posunu mimo hello služby Event Hubs. Každá událost v rámci oddílu zahrnuje posun.

![Event Hubs](./media/event-hubs-features/partition_offset.png)

### <a name="checkpointing"></a>Vytváření kontrolních bodů

*Vytváření kontrolních bodů* je proces, pomocí kterého čtenáři označují nebo potvrzují svou pozici v rámci posloupnosti událostí v oddílu. Vytváření kontrolních bodů je zodpovědností hello hello příjemce a probíhá na bázi oddílů v rámci skupiny příjemců. Tato odpovědnost znamená, že pro každé skupině příjemců každý Čtenář oddílu musí udržovat přehled o své aktuální pozici v datovém proudu událostí hello a může informovat službu hello, když bude považovat datový proud hello dokončení.

Pokud čtečka z oddílu odpojí, při opětovném připojení začne číst od kontrolního bodu hello, který dříve zaslal hello poslední Čtenář daného oddílu z této skupiny příjemců. Když hello Čtenář připojí, předá tento posunutí toohello události rozbočovače toospecify hello umístění na které toostart čtení. Tímto způsobem můžete vytváření kontrolních bodů tooboth označování událostí jako "dokončeno" aplikací pro příjem dat a dojde k tooprovide odolnosti pokud převzetí služeb při selhání čtenářů spuštěných na různé počítače. Je možné tooreturn tooolder dat tak, že určíte nižší posun od kontrolního bodu. Díky tomuto mechanismu umožňuje vytváření kontrolních bodů nejen obnovu při selhání, ale i opakované přehrání datového proudu.

### <a name="common-consumer-tasks"></a>Běžné úlohy příjemce

Všichni příjemci Event Hubs připojení přes relaci protokolu AMQP 1.0, stav obousměrný komunikační kanál. Každý oddíl má relaci protokolu AMQP 1.0, která usnadňuje transport událostí oddělených oddílem hello.

#### <a name="connect-tooa-partition"></a>Připojit tooa oddílu

Při připojování toopartitions, je běžný postup toouse leasing mechanismus toocoordinate čtečky připojení toospecific oddíly. Tímto způsobem je možné u každého oddílu ve příjemce skupiny toohave pouze jednoho aktivního čtenáře. Vytváření kontrolních bodů, leasing a správu čtečky jednodušší pomocí hello [EventProcessorHost](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost) třída pro klienty .NET. Hello Event Processor Host je inteligentní agent příjemce.

#### <a name="read-events"></a>Čtení událostí

Po otevření odkaz a relace AMQP 1.0 u konkrétního oddílu se události doručují toohello protokolu AMQP 1.0 klienta podle hello služby Event Hubs. Tento mechanismus doručení umožňuje vyšší propustnost a nižší latenci než mechanismy založené na operaci Pull, jako je například metoda GET protokolu HTTP. Události se posílají toohello klienta, každá instance dat události obsahuje důležitá metadata, jako hello posun a číslo posloupnosti, které jsou používané toofacilitate vytváření kontrolních bodů v posloupnosti událostí hello.

Data události:
* Posun
* Pořadové číslo
* Tělo
* Uživatelské vlastnosti
* Systémové vlastnosti

Je vaše odpovědnosti toomanage hello posun.

## <a name="capacity"></a>Kapacita

Služba Event Hubs je vysoce škálovatelná paralelní architektura a nejsou tooconsider několik klíčových faktorů při dimenzování a škálování.

### <a name="throughput-units"></a>Jednotky propustnosti

kapacita propustnosti Hello Event Hubs řídí *jednotky propustnosti*. Jednotky propustnosti jsou předem zakoupené jednotky kapacity. Jedna jednotka propustnosti zahrnuje následující kapacitu hello:

* Příchozí data: až too1 MB za sekundu nebo 1000 událostí za sekundu (nastane dříve)
* Odchozí data: až too2 MB za sekundu

Nad rámec hello kapacitu hello zakoupené jednotky propustnosti, je omezen příjem příchozích dat a [ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) je vrácen. Odchozí nevytváří takové výjimky, ale je stále omezené toohello kapacita hello zakoupené jednotky propustnosti. Je-li přijímat publikování výjimky související s frekvencí nebo budoucnu očekáváte větší objem odchozích dat toosee, být toocheck se, kolik jednotek propustnosti jste zakoupili pro obor názvů hello. Můžete spravovat jednotky propustnosti na hello **škálování** okno hello obory názvů v hello [portál Azure](https://portal.azure.com). Můžete také spravovat jednotky propustnosti programově pomocí hello [rozhraní API centra událostí](event-hubs-api-overview.md).

Jednotky propustnosti se kupují předem a účtují se po hodinách. Zakoupené jednotky propustnosti se účtují minimálně za jednu hodinu. Jednotky propustnosti too20 můžete zakoupili pro obor názvů Event Hubs a jsou sdíleny ve všech centrech událostí v oboru názvů hello.

Další jednotky propustnosti lze zakoupit v blocích po 20 too100 jednotky propustnosti, že se obrátíte na podporu Azure. Navíc si můžete taky zakoupit bloky, které obsahují sto jednotek propustnosti.

Doporučujeme vám, že můžete vyrovnávat propustnost jednotky a oddíly, které tooachieve optimálního škálování. Škálování jednoho oddílu dovoluje maximálně jednu jednotku propustnosti. Hello počet jednotek propustnosti by měl být menší než nebo rovna toohello počet oddílů v Centru událostí.

Informace o cenách podrobné Event Hubs, najdete v části [cenách služby Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="next-steps"></a>Další kroky

Další informace o službě Event Hubs najdete na adrese hello následující odkazy:

* Úvodní [kurz služby Event Hubs][Event Hubs tutorial]
* [Průvodce programováním pro službu Event Hubs](event-hubs-programming-guide.md)
* [Dostupnost a konzistence ve službě Event Hubs](event-hubs-availability-and-consistency.md)
* [Nejčastější dotazy k Event Hubs](event-hubs-faq.md)
* [Ukázky centra událostí][]

[Event Hubs tutorial]: event-hubs-dotnet-standard-getstarted-send.md
[Ukázky centra událostí]: https://github.com/Azure/azure-event-hubs/tree/master/samples
