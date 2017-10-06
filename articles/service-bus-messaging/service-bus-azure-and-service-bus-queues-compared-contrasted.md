---
title: "fronty úložiště aaaAzure a fronty Service Bus - porovnání a rozdíl od aktualizovaného | Microsoft Docs"
description: "Analyzuje rozdíly a podobnosti mezi dvěma typy front, které nabízí Azure."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: tysonn
ms.assetid: f07301dc-ca9b-465c-bd5b-a0f99bab606b
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/07/2017
ms.author: sethm
ms.openlocfilehash: f8b915e73ea3c82d823a96bf23c8c9e24c96aa42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storage-queues-and-service-bus-queues---compared-and-contrasted"></a>Fronty úložiště a fronty Service Bus - porovnání a na rozdíl od aktualizovaného
Tento článek analyzuje hello rozdíly a podobnosti mezi hello dva typy front, které nabízí Microsoft Azure ještě dnes: fronty úložiště a fronty Service Bus. Pomocí těchto informací můžete porovnat a kontrastu hello příslušné technologie a být schopný toomake více informované rozhodnutí o řešení, které nejlépe vyhovuje vašim potřebám.

## <a name="introduction"></a>Úvod
Azure podporuje dva typy mechanismů fronty: **fronty úložiště** a **fronty Service Bus**.

**Fronty úložiště**, které jsou součástí hello [úložiště Azure](https://azure.microsoft.com/services/storage/) infrastruktury, funkce jednoduché rozhraní založené na REST GET nebo PUT/funkce Náhled, poskytuje spolehlivé, trvalé zasílání zpráv v rámci a mezi službami.

**Fronty služby Service Bus** jsou součástí širší [zasílání zpráv Azure](https://azure.microsoft.com/services/service-bus/) infrastruktury, který podporuje služby Řízení front a také publikování a přihlášení k odběru a další pokročilé integrace vzory. Další informace o Service Bus fronty nebo témata nebo předplatných najdete v tématu hello [Přehled služby Service Bus](service-bus-messaging-overview.md).

Přestože obě služby Řízení front technologie existují současně, fronty úložiště zavedených Zaprvé, jako mechanismus vyhrazené fronty úložiště postavená na služby úložiště Azure. Fronty služby Service Bus je postavená na hello širší "zasílání zpráv" infrastruktury určené toointegrate aplikací nebo součástí aplikace, které může span více komunikační protokoly, kontrakty dat, vztah důvěryhodnosti domén nebo prostředí sítě.

## <a name="technology-selection-considerations"></a>Důležité informace o výběru technologie
Fronty úložiště a fronty Service Bus jsou implementace hello zprávy služby Řízení front služby aktuálně nabízené na Microsoft Azure. Každý má sadu mírně odlišné funkce, což znamená, vyberte jednu nebo jiné hello nebo používat, v závislosti na hello požadavky vaší konkrétní řešení nebo obchodní nebo technické problému, který se řešení.

Při určování služby Řízení front technologii, která vyhovuje hello účel daného řešení, zvažte následující doporučení hello řešení architekty a vývojáře. Další podrobnosti najdete v tématu hello další části.

Jako řešení architekt nebo vývojáře **měli byste zvážit použití fronty úložiště** při:

* Aplikace musí uložit více než 80 GB zpráv ve frontě, kde hello zprávy mají životnost kratší než 7 dní.
* Vaše aplikace chce tootrack průběh pro zpracování zprávy uvnitř hello fronty. To je užitečné, pokud dojde k chybě pracovního procesu hello zpracování zprávy. Následné pracovní pak můžete použít tento toocontinue informace z kde předchozí pracovní hello skončil.
* Vyžadujete protokoly serveru straně všech hello transakce prováděné vůči vaší front.

Jako řešení architekt nebo vývojáře **měli byste zvážit použití fronty Service Bus** při:

* Řešení musí být schopný tooreceive zpráv bez nutnosti toopoll hello fronty. Službou Service Bus toho lze dosáhnout pomocí hello použití hello dlouho dotazování přijímat operace pomocí protokolu TCP protokolů hello, které podporuje Service Bus.
* Řešení vyžaduje hello fronty tooprovide zaručenou první in-first-out (FIFO) seřazené doručení.
* Chcete symetrický prostředí v Azure a v systému Windows Server (privátní cloud). Další informace najdete v tématu [sběrnice služby pro Windows Server](https://msdn.microsoft.com/library/dn282144.aspx).
* Řešení musí být schopný toosupport automatické duplicitní zjišťování.
* Chcete, aby vaše aplikace tooprocess zprávy jako paralelní dlouhodobé datové proudy (zprávy jsou spojené s pomocí hello datový proud a [SessionId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sessionid?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId) vlastnost na uvítací zprávu). V tomto modelu každý uzel ve využívání aplikací hello bojuje pro datové proudy, jako názvem na rozdíl od toomessages. Pokud datový proud je zadána tooa využívání uzlu hello uzlu můžete zkontrolovat stav hello stav datového proudu aplikace hello použití transakcí.
* Řešení vyžaduje transakční chování a nedělitelnost při odesílání nebo přijímání více zpráv z fronty.
* Hello time to live (TTL) vlastnosti specifické pro aplikaci zatížení hello může být vyšší než období hello 7 dnů.
* Vaše aplikace zpracovává zprávy, které může být vyšší než 64 KB, ale bude nejspíš neobjeví přístup hello omezení 256 KB.
* Práce s tooprovide požadavek toohello modelu přístupu na základě role fronty a různá práva nebo oprávnění pro odesílateli a příjemci.
* Velikost vašeho fronty nebude růst větší než 80 GB.
* Chcete toouse hello protokolu AMQP 1.0 založených na standardech zasílání zpráv protokolu. Další informace o protokolu AMQP najdete v tématu [přehled AMQP Service Bus](service-bus-amqp-overview.md).
* Představ při případné migraci z fronty na základě typu point-to-point komunikace tooa vzorce výměny zpráv umožňující bezproblémovou integraci další příjemci (odběratelé), z nichž každý přijímá nezávislé kopie některých nebo všech odesílat zprávy fronty toohello. Hello pozdější odkazuje toohello publikování a přihlášení k odběru schopností nativně poskytované služby Service Bus.
* Řešení zasílání zpráv musí být schopný toosupport hello "na většinu-jedno" doručení záruku hello nevyžaduje jste toobuild hello další infrastrukturu součásti.
* Chcete vytvořit možné toopublish toobe a využívat dávky zprávy.

## <a name="comparing-storage-queues-and-service-bus-queues"></a>Porovnání fronty úložiště a fronty Service Bus
Hello tabulky v hello následující oddíly poskytují možnost logického seskupování fronty funkcí a umožňují porovnat na první pohled, hello možnosti dostupné v fronty úložiště a fronty Service Bus.

## <a name="foundational-capabilities"></a>Základní možnosti
Tato část porovná některé z možností hello základní front zpráv poskytuje úložiště fronty a fronty Service Bus.

| Kritérií porovnání | Fronty úložiště | Fronty služby Service Bus |
| --- | --- | --- |
| Řazení záruku |**Ne** <br/><br>Další informace najdete v tématu hello první si poznamenejte hello části "Další informace".</br> |**Ano - First-In-First-Out (FIFO)**<br/><br>(prostřednictvím hello použití relací pro zasílání zpráv) |
| Záruky doručení |**V aspoň jednou** |**V aspoň jednou**<br/><br/>**Jednou na většinu** |
| Podpora atomické operace |**Ne** |**Ano**<br/><br/> |
| Přijímat chování |**Bez blokování**<br/><br/>(dokončení okamžitě pokud se nenajde žádné nové zprávy) |**Blokování s nebo bez časového limitu**<br/><br/>(nabízí dlouhé dotazování nebo hello ["Comet technika"](http://go.microsoft.com/fwlink/?LinkId=613759))<br/><br/>**Bez blokování**<br/><br/>(prostřednictvím hello použití rozhraní .NET spravované rozhraní API pouze) |
| Rozhraní API nabízené stylu |**Ne** |**Ano**<br/><br/>[OnMessage](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessage#Microsoft_ServiceBus_Messaging_QueueClient_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__) a **OnMessage** relací .NET API. |
| Zobrazí režim |**Funkce Náhled & zapůjčení** |**Funkce Náhled & uzamčení**<br/><br/>**Přijímat & Odstranit** |
| Režim výhradní přístup |**Na základě zapůjčení** |**Na základě zámku** |
| Doba trvání zapůjčení/zámku |**30 sekund (výchozí)**<br/><br/>**7 dní (maximum)** (můžete obnovit nebo verzi zapůjčení zpráv pomocí hello [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) rozhraní API.) |**60 sekund (výchozí)**<br/><br/>Můžete obnovit pomocí hello zámek zprávy [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) rozhraní API. |
| Zapůjčení/uzamčení přesnost |**Úroveň zprávy**<br/><br/>(každá zpráva může mít hodnotu jinou vypršení časového limitu, která může aktualizovat podle potřeby při zpracování uvítací zprávu pomocí hello [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) rozhraní API) |**Úroveň fronty**<br/><br/>(každá fronta má tooall přesnost použita uzamčení jeho zpráv, ale můžete obnovit pomocí hello zámku hello [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) rozhraní API.) |
| Zpracovat v dávce přijímat |**Ano**<br/><br/>(explicitně určit počet zpráv při načítání zpráv až tooa maximálně 32 zprávy) |**Ano**<br/><br/>(implicitně povolení vlastnost před načtením nebo explicitně prostřednictvím hello použít transakcí) |
| Dávkové odeslání |**Ne** |**Ano**<br/><br/>(prostřednictvím hello použití transakcí nebo dávkování na straně klienta) |

### <a name="additional-information"></a>Další informace
* Zprávy do front úložiště jsou obvykle first-in-first-out, ale v některých případech může být mimo pořadí; například dobu trvání časového limitu viditelnost zpráva vypršení platnosti (například v důsledku chyb během zpracování klientské aplikace). Když vyprší časový limit viditelnosti hello, hello zpráva se zobrazí na hello fronty pro jiný pracovní toodequeue znova ho. V tomto okamžiku může umístit nově viditelné uvítací zprávu ve frontě hello (znovu toobe vyjmutou) po zprávu, která byla původně zařazených do fronty za ním.
* Hello zaručit, že FIFO vzor v fronty Service Bus vyžaduje použití hello relací zasílání zpráv. V hello událost, která hello aplikace dojde k chybě při zpracování zprávy přijaté v hello **prohlížet & Zamknout** režim, hello příštím příjemce fronta přijímá relaci zasílání zpráv, bude začínat hello selhání zpráva po jeho Time to live (TTL) období vyprší platnost.
* Fronty úložiště jsou navrženou toosupport standardní služby Řízení front scénáře, jako je aplikace součástí tooincrease škálovatelnost a odolnost proti selhání oddělení, zatížení vyrovnávání a pracovní postupy procesů sestavování.
* Fronty Service Bus podporují hello *v aspoň jednou* záruku doručení. Kromě toho hello *jednou na většinu* sémantického může být podporován pomocí stav aplikace hello toostore stavu relace pomocí transakce tooatomically přijímat zprávy a aktualizovat stav relace hello.
* Fronty úložiště zadat programovací model jednotné a konzistentní napříč fronty, tabulky a objekty BLOB – pro vývojáře i pro operace týmy.
* Fronty služby Service Bus poskytuje podporu pro místní transakce v kontextu hello jedné frontě.
* Hello **přijetí a odstranění** režim podporovaný Service Bus poskytuje hello možnost tooreduce hello počet operací (a související náklady) pro zasílání zpráv za zajištění snížený doručení.
* Fronty úložiště poskytoval zapůjčení s hello možnost tooextend hello zapůjčení pro zprávy. To umožňuje pracovníkům hello toomaintain krátké zapůjčení na zprávy. Proto pokud dojde k chybě pracovní, uvítací zprávu může rychle zpracovat znovu jinému pracovnímu procesu. Kromě toho pracovní můžete rozšířit zapůjčení hello na zprávu, pokud je doba delší, než aktuální hello pronájmu tooprocess.
* Fronty úložiště nabízejí viditelnost vypršení časového limitu, můžete nastavit při zařazování hello nebo vyřazení zprávy. Kromě toho zprávu můžete aktualizovat pomocí různých zapůjčení hodnoty při spuštění a aktualizace různé hodnoty mezi zprávy v hello stejné fronty. Překročení časového limitu zámku Service Bus jsou definovány v metadatech fronty hello; ale můžete obnovit hello zámku tak, že volání hello [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) metoda.
* maximální časový limit Hello blokování přijímat operace v fronty Service Bus je 24 dní. Na základě REST vypršení časových limitů však mít maximální hodnotu 55 sekund.
* Dávkování na straně klienta poskytovaný umožňuje Service Bus fronty klienta toobatch více zpráv do jednoho odeslání operace. Dávkování je k dispozici pouze pro operace asynchronní odesílání.
* Funkce jako je například mezní hodnoty 200 TB hello front úložiště (více při virtualizaci účty) a neomezená fronty vám ideální platformu pro poskytovatele SaaS.
* Zadejte fronty úložiště flexibilní a původce delegovaný mechanismu řízení přístupu.

## <a name="advanced-capabilities"></a>Rozšířené možnosti
Tato část porovná pokročilých funkcí poskytovaných fronty úložiště a fronty Service Bus.

| Kritérií porovnání | Fronty úložiště | Fronty služby Service Bus |
| --- | --- | --- |
| Doručení naplánované |**Ano** |**Ano** |
| Automatické mrtvou lettering |**Ne** |**Ano** |
| Zvýšení hodnoty time to live fronty |**Ano**<br/><br/>(prostřednictvím místní aktualizace limit viditelnosti) |**Ano**<br/><br/>(poskytované prostřednictvím vyhrazené funkce rozhraní API) |
| Zacházení s nezpracovatelnými podpora zpráv |**Ano** |**Ano** |
| Aktualizace na místě |**Ano** |**Ano** |
| Protokol transakce na straně serveru |**Ano** |**Ne** |
| Metriky úložiště |**Ano**<br/><br/>**Minutu metriky**: poskytuje metriky v reálném čase pro dostupnost, TPS, rozhraní API volat počty, počty chyb a další v reálném čase (agregovat za minutu a jsou uvedeny během několika minut od co se právě stalo v produkčním prostředí. Další informace najdete v tématu [o Storage Analytics Metrics](/rest/api/storageservices/fileservices/About-Storage-Analytics-Metrics). |**Ano**<br/><br/>(hromadné dotazy voláním [GetQueues](/dotnet/api/microsoft.servicebus.namespacemanager.getqueues#Microsoft_ServiceBus_NamespaceManager_GetQueues)) |
| Stav správy |**Ne** |**Ano**<br/><br/>[Microsoft.ServiceBus.Messaging.EntityStatus.Active](/dotnet/api/microsoft.servicebus.messaging.entitystatus.active), [Microsoft.ServiceBus.Messaging.EntityStatus.Disabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus.disabled), [Microsoft.ServiceBus.Messaging.EntityStatus.SendDisabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus.senddisabled), [Microsoft.ServiceBus.Messaging.EntityStatus.ReceiveDisabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus.receivedisabled) |
| Automatické předávání zpráv |**Ne** |**Ano** |
| Vyprázdnění fronty – funkce |**Ano** |**Ne** |
| Zpráva skupiny |**Ne** |**Ano**<br/><br/>(prostřednictvím hello použití relací pro zasílání zpráv) |
| Stav aplikace na zprávy skupinu |**Ne** |**Ano** |
| Detekce duplicitních |**Ne** |**Ano**<br/><br/>(konfigurovat na straně odesílatele hello) |
| Procházení skupin zpráv |**Ne** |**Ano** |
| Načítání zpráv relací podle ID |**Ne** |**Ano** |

### <a name="additional-information"></a>Další informace
* Obě služby Řízení front technologie povolit zpráva toobe, naplánován pro odeslání na později.
* Automatické přeposílání fronty umožňuje tisíce fronty tooauto dopředného jejich zprávy tooa jedné fronty, ze které přijímající aplikace hello využívá uvítací zprávu. Můžete použít tento mechanismus tooachieve zabezpečení, tok řízení a izolovat úložiště mezi každou zprávu vydavatele.
* Fronty úložiště poskytuje podporu pro aktualizaci obsahu zprávy. Tuto funkci můžete použít pro zachování informací o stavu a průběhu přírůstkové aktualizace do zprávy hello tak, aby může být zpracována z hello poslední známé kontrolního bodu, místo od začátku. Pomocí front Service Bus, můžete povolit hello stejné scénář prostřednictvím hello použití relací zprávy. Relace umožňují toosave a načíst stav zpracování aplikace hello (pomocí [setstate –](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_) a [GetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate#Microsoft_ServiceBus_Messaging_MessageSession_GetState)).
* [Mrtvých písmem](service-bus-dead-letter-queues.md), která je jen nepodporuje fronty Service Bus, může být užitečná pro izolace zprávy, které nelze úspěšně zpracovat hello přijímající aplikace nebo když zprávy nelze cílového umístění v důsledku tooan platnost Vlastnost Time to live (TTL). Hodnota TTL Hello Určuje, jak dlouho zůstane zprávu ve frontě hello. Službou Service Bus bude uvítací zprávu fronty speciální přesunutý tooa názvem $DeadLetterQueue, když vyprší doba TTL hello.
* toofind "poškozených" zpráv do front úložiště, když vyřazení k aplikaci hello zpráva prozkoumá hello  **[DequeueCount](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueuemessage.dequeuecount.aspx)**  vlastnost uvítací zprávu. Pokud **DequeueCount** je větší než danou prahovou hodnotu, aplikace hello přesune hello zpráva tooan definované aplikací "nedoručených zpráv" fronty.
* Fronty úložiště povolit tooobtain podrobný protokol všechny hello transakce prováděné vůči hello fronty, jakož i agregovat metriky. Obě tyto možnosti jsou užitečné pro ladění a pochopení, jak vaše aplikace používá fronty úložiště. Také se hodí pro vaše aplikace optimalizace výkonu a snížení nákladů hello použití fronty.
* Koncept Hello "zpráva relací" Service Bus podporuje umožňuje zprávy, které patří tooa určité logické skupiny toobe přidružené k dané příjemce, který pak vytvoří relace jako spřažení mezi zprávy a jejich odpovídajících příjemci. Můžete ho povolit rozšířené funkce v Service Bus pomocí nastavení hello [SessionID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sessionid#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId) vlastnost ve zprávě. Příjemce potom naslouchat na ID konkrétní relace a přijímat zprávy, které sdílejí hello zadaný identifikátor relace.
* Odebere duplicitní zprávy odeslané tooa fronta nebo téma, na základě hodnoty hello hello zprostředkovatele Hello duplikace detekce funkcí podporovaných fronty Service Bus automaticky [MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) vlastnost.

## <a name="capacity-and-quotas"></a>Kapacity a kvót
Tato část porovná fronty úložiště a fronty Service Bus z perspektivy hello [kapacity a kvót](service-bus-quotas.md) , uplatnit.

| Kritérií porovnání | Fronty úložiště | Fronty služby Service Bus |
| --- | --- | --- |
| Maximální velikost fronty |**500 TB**<br/><br/>(omezený tooa [jednotné kapacitě účtu úložiště](../storage/common/storage-introduction.md#queue-storage)) |**1 GB too80 GB**<br/><br/>(definován při vytvoření fronty a [povolení dělení](service-bus-partitioning.md) – najdete v části "Další informace" hello) |
| Maximální velikost zprávy |**64 KB**<br/><br/>(48 KB při použití **Base64** kódování)<br/><br/>Azure podporuje velké zprávy kombinací fronty a objekty BLOB – v tomto okamžiku můžete zařadit do too200GB pro jednu položku. |**256 KB** nebo **1 MB**<br/><br/>(včetně záhlaví a text, velikost maximální záhlaví: 64 KB).<br/><br/>Závisí na hello [vrstvy služby](service-bus-premium-messaging.md). |
| Maximální hodnota TTL zprávy |**7 dní** |**`TimeSpan.Max`** |
| Maximální počet front |**Unlimited** |**10,000**<br/><br/>(na obor názvů služby, může být zvýšena) |
| Maximální počet souběžných klientů |**Unlimited** |**Unlimited**<br/><br/>(100 souběžných připojení omezení se vztahuje pouze komunikace na základě protokolu tooTCP) |

### <a name="additional-information"></a>Další informace
* Service Bus vynucuje omezení velikosti fronty. Hello maximální velikost fronty je zadána při vytvoření fronty hello a může mít hodnotu mezi 1 a 80 GB. Pokud je dosaženo hodnota velikosti fronty hello nastavit při vytváření hello fronty, další příchozí zprávy budou odmítnuty a dostane výjimku hello volání kódu. Další informace o kvótách v Service Bus, najdete v části [Service Bus kvóty](service-bus-quotas.md).
* V hello [úrovně Standard](service-bus-premium-messaging.md), vytvořením front Service Bus na 1, 2, 3, 4 nebo 5 GB velikosti (hello výchozí hodnota je 1 GB). Ve vrstvě hello Premium můžete vytvořit fronty až do velikosti too80 GB. Ve verzi Standard úroveň, s dělení povolené (což je výchozí hello), Service Bus vytvoří 16 oddíly pro každý GB je zadat. Jako takový, když vytvoříte frontu, který je 5 GB velikost, s 16 oddíly hello maximální velikost fronty stane (5 * 16) = 80 GB. Zobrazí maximální velikost hello oddílů fronta nebo téma na základě jeho položku na hello [portál Azure][Azure portal]. V úrovni Premium hello pouze 2 oddílech se vytvoří každou frontu.
* Fronty úložiště, pokud hello obsah zprávy hello není bezpečné XML a pak musí být **Base64** kódování. Pokud jste **Base64**-kódování uvítací zprávu, datové části hello uživatele může být až too48 KB, místo 64 KB.
* Pomocí front Service Bus, každá zpráva uložený ve frontě se skládá ze dvou částí: hlavičku a text. Celková velikost zpráv hello Hello nesmí překročit uvítací zprávu maximální velikost podporovaná ve vrstvě služeb hello.
* Pokud klienti komunikují pomocí front Service Bus přes hello protokolu TCP, hello maximální počet souběžných připojení tooa jedné fronty Service Bus je omezená too100. Toto číslo je sdílena mezi odesílateli a příjemci. Pokud je dosaženo této kvóty, odeslání dalších žádostí o další připojení se odmítne a dostane výjimku hello volání kódu. Toto omezení není vynucená pro klienty připojující se toohello fronty pomocí rozhraní API založené na REST.
* Pokud potřebujete více než 10 000 front v jeden obor názvů Service Bus, můžete obraťte se na tým podpory Azure hello a požádejte o zvýšení limitu. tooscale překračuje 10 000 front se Service Bus, můžete také vytvořit další obory názvů pomocí hello [portál Azure][Azure portal].

## <a name="management-and-operations"></a>Operace a Správa
Tato část obsahuje porovnání funkcí správy hello poskytované fronty úložiště a fronty Service Bus.

| Kritérií porovnání | Fronty úložiště | Fronty Service Bus |
| --- | --- | --- |
| Protokol pro správu |**REST přes protokol HTTP nebo HTTPS** |**REST přes protokol HTTPS** |
| Protokol modulu runtime |**REST přes protokol HTTP nebo HTTPS** |**REST přes protokol HTTPS**<br/><br/>**AMQP 1.0 Standard (TCP with TLS)** |
| .NET API |**Ano**<br/><br/>(Klient úložiště .NET API) |**Ano**<br/><br/>(.NET sběrnice rozhraní API) |
| Nativní C++ |**Ano** |**Ano** |
| Java API |**Ano** |**Ano** |
| ROZHRANÍ API PHP |**Ano** |**Ano** |
| Rozhraní API Node.js |**Ano** |**Ano** |
| Podpora libovolný metadat |**Ano** |**Ne** |
| Pravidla pojmenování fronty |**Až too63 znaků.**<br/><br/>(Písmena v název fronty musí být malá písmena.) |**Až too260 znaků.**<br/><br/>(Fronty cesty a názvy jsou velká a malá písmena.) |
| Get – funkce délka fronty |**Ano**<br/><br/>(Přibližnou hodnotu, pokud zprávy vyprší za hello TTL bez odstraňuje.) |**Ano**<br/><br/>(Přesný, v okamžiku hodnota). |
| Prohlížení – funkce |**Ano** |**Ano** |

### <a name="additional-information"></a>Další informace
* Fronty úložiště poskytuje podporu pro libovolné atributy, které můžou být použité toohello popis fronty, hello tvar dvojice název/hodnota.
* Obě technologie fronty nabízejí možnost toopeek hello zprávu bez nutnosti toolock it, které mohou být užitečné při implementaci nástroj Prohlížeč/explorer fronty.
* Hello Service Bus .NET pro zprostředkované zasílání zpráv rozhraní API využívají plně duplexní připojení TCP pro zlepšení výkonu při porovnání tooREST přes protokol HTTP, a podporují standardní protokol hello protokolu AMQP 1.0.
* Názvy front úložiště může být 3 až 63 znaků dlouhý, může obsahovat malá písmena, číslice a pomlčky. Další informace najdete v tématu [pojmenování front a Metadata](/rest/api/storageservices/fileservices/Naming-Queues-and-Metadata).
* Názvy fronty Service Bus může být až too260 znaků a musí mít méně přísná pravidla pojmenování. Názvy fronty Service Bus může obsahovat písmena, číslice, tečky, pomlčky a podtržítka.

## <a name="authentication-and-authorization"></a>Ověřování a autorizace
Tato část popisuje hello ověřování a autorizace funkcí podporovaných fronty úložiště a fronty Service Bus.

| Kritérií porovnání | Fronty úložiště | Fronty služby Service Bus |
| --- | --- | --- |
| Authentication |**Symetrický klíč** |**Symetrický klíč** |
| Model zabezpečení |Delegovaný přístup prostřednictvím tokeny SAS. |SAS |
| Zprostředkovatel federaci identit |**Ne** |**Ano** |

### <a name="additional-information"></a>Další informace
* Každý požadavek tooeither služby Řízení front technologie Dobrý den, musí být ověřeny. Anonymní přístup veřejné fronty nejsou podporovány. Pomocí [SAS](service-bus-sas.md), tento scénář lze vyřešit tak, že publikování pouze pro zápis SAS, SAS jen pro čtení nebo i SAS úplnému přístupu.
* Hello schéma ověřování zadaný úložiště fronty zahrnuje použití hello symetrický klíč, který je na základě hodnoty hash ověřování kódu metoda HMAC (Message), počítaný s algoritmem hello SHA-256 a kódovaná jako **Base64** řetězec. Další informace o příslušných protokol hello najdete v tématu [ověřování pro služby úložiště Azure hello](/rest/api/storageservices/fileservices/Authentication-for-the-Azure-Storage-Services). Fronty služby Service Bus podporují podobné model pomocí symetrických klíčů. Další informace najdete v tématu [ověřování sdíleného přístupového podpisu službou Service Bus](service-bus-sas.md).

## <a name="conclusion"></a>Závěr
Podle získat lepší představu o hello dvě technologie, budete se moct toomake více informované rozhodnutí, na kterém fronty toouse technologie a při. Hello rozhodnutí o při toouse fronty úložiště nebo Service Bus fronty jasně závisí na počtu faktorů. Tyto faktory mohou výraznou závisí na hello konkrétním potřebám vaší aplikace a jeho architektura. Pokud už vaše aplikace používá hello základní funkce Microsoft Azure, dáte možná přednost toochoose front úložiště, zvlášť pokud vyžadují základní komunikace a zasílání zpráv mezi služby nebo nutnost fronty, které může být větší než 80 GB velikost.

Protože fronty Service Bus poskytují počet pokročilé funkce, jako je například relace, transakce, duplicitní detekce, automatické zpráv lettering a odolná funkce pbulikovat/odebírat, mohou být upřednostňovanou volbou Pokud vytváříte hybridním aplikace nebo pokud vaše aplikace, jinak hodnota vyžaduje tyto funkce.

## <a name="next-steps"></a>Další kroky
Hello následující články poskytují další pokyny a informace o použití úložiště fronty nebo fronty Service Bus.

* [Začínáme s frontami služby Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Jak tooUse hello fronty úložiště služby](../storage/queues/storage-dotnet-how-to-use-queues.md)
* [Osvědčené postupy pro zlepšení výkonu pomocí služby Service Bus zprostředkované zasílání zpráv](service-bus-performance-improvements.md)
* [Představení front a témat v Azure Service Bus (příspěvek na blogu)](http://www.code-magazine.com/article.aspx?quickid=1112041)
* [Hello tooService Příručka pro vývojáře sběrnice](http://www.cloudcasts.net/devguide/Default.aspx?id=11030)
* [Použití hello služba Řízení front v Azure](http://www.developerfusion.com/article/120197/using-the-queuing-service-in-windows-azure/)

[Azure portal]: https://portal.azure.com

