---
title: "aplikace Azure Service Bus aaaInsulating proti výpadkům a havárií | Microsoft Docs"
description: "Popisuje postupy můžete použít tooprotect aplikace pro potenciální výpadek služby Service Bus."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: tysonn
ms.assetid: fd9fa8ab-f4c4-43f7-974f-c876df1614d4
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/12/2017
ms.author: sethm
ms.openlocfilehash: 349b4968456c9f15375753d83495246f5a3ddfdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-insulating-applications-against-service-bus-outages-and-disasters"></a>Osvědčené postupy pro izolační aplikace proti výpadkům Service Bus a havárií
Kritické aplikace musí nepřetržitě, fungovat i v hello přítomnost neplánované výpadky nebo havárie. Toto téma popisuje postupy můžete použít aplikace Service Bus tooprotect pro potenciální výpadek služby nebo po havárii.

Výpadek je definován jako dočasné nedostupnosti hello Azure Service Bus. výpadek Hello může ovlivnit některé součásti Service Bus, jako je zasílání zpráv úložiště nebo celého datového centra i hello. Po napravení problému hello Service Bus opět k dispozici. Výpadek obvykle nezpůsobí ztrátě zpráv nebo jiná data. Je například selhání součásti hello nedostupnosti konkrétní úložišti pro přenos zpráv. Příkladem výpadku celou datacenter je výpadku napájení hello datacenter nebo vadný datacenter síťový přepínač. Výpadek můžete poslední z pár minut tooa několik dní.

Havárie je definován jako trvalé ztrátě hello jednotky škálování služby Service Bus nebo datacenter. Hello datacenter může nebo nemusí opět k dispozici. Havárie obvykle způsobí ztrátu některé nebo všechny zprávy nebo jiná data. Příklady havárie se ještě efektivněji, zahlcení nebo zemětřesení.

## <a name="current-architecture"></a>Aktuální architektura
Service Bus používá více zasílání zpráv ukládá toostore zpráv, které jsou odeslány tooqueues nebo témata. Bez oddílů fronta nebo téma je přiřazen tooone úložiště pro zasílání zpráv. Pokud toto úložiště zasílání zpráv není k dispozici, všechny operace v tomto fronta nebo téma se nezdaří.

Všechny služby Service Bus entit pro zasílání zpráv (fronty, témata, předávání) se nacházejí v oboru názvů služby, který je přidružen k datacentru. Service Bus neumožňuje automatické geografická replikace dat, k tomu ani obor názvů toospan několik datových center.

## <a name="protecting-against-acs-outages"></a>Ochrana proti výpadkům služby ACS
Pokud používáte přihlašovací údaje služby ACS a služby ACS není k dispozici, klienti už můžou získat tokeny. Klienti, kteří mají token v době hello ACS ocitne mimo provoz můžete pokračovat toouse Service Bus, dokud nevyprší platnost tokenů hello. životnost tokenu výchozí Hello je 3 hodiny.

tooprotect proti výpadkům služby ACS používala tokeny sdíleného přístupového podpisu (SAS). V takovém případě hello klient se ověří přímo službou Service Bus podepsáním samoobslužné minted token s tajným klíčem. Volání tooACS se už nevyžadují. Další informace o tokeny SAS najdete v tématu [ověření sběrnice][Service Bus authentication].

## <a name="protecting-queues-and-topics-against-messaging-store-failures"></a>Ochrana fronty a témata proti selhání úložiště pro zasílání zpráv
Bez oddílů fronta nebo téma je přiřazen tooone úložiště pro zasílání zpráv. Pokud toto úložiště zasílání zpráv není k dispozici, všechny operace v tomto fronta nebo téma se nezdaří. A rozdělena na oddíly na hello fronta druhé straně, se skládá z více fragmentů. Každý fragment je uložen v jiném úložišti zasílání zpráv. Odeslání zprávy tooa oddílů fronta nebo téma sběrnice přiřadí hello tooone zprávy z fragmentů hello. Pokud hello odpovídající úložišti pro přenos zpráv není k dispozici, zapíše Service Bus hello tooa různých fragmentu zprávy, pokud je to možné. Další informace o dělené entity najdete v tématu [segmentované entity zasílání zpráv][Partitioned messaging entities].

## <a name="protecting-against-datacenter-outages-or-disasters"></a>Ochrana proti výpadkům datacenter nebo havárie
tooallow převzetí služeb při selhání dvou Datacenter, můžete vytvořit obor názvů služby Service Bus v každé datové centrum. Například hello oboru názvů služby Service Bus **contosoPrimary.servicebus.windows.net** může nacházet v oblasti USA, Severní a střední hello a **contosoSecondary.servicebus.windows.net**může nacházet v oblasti USA – jih a střední hello. Pokud Service Bus entity pro zasílání zpráv musí zůstat přístupný v hello přítomnost výpadku datového centra, můžete vytvořit dané entity v oba obory názvů.

Další informace najdete v tématu hello "Selhání v rámci datového centra Azure Service Bus" kapitoly [asynchronní vzory a vysoká dostupnost pro zasílání zpráv][Asynchronous messaging patterns and high availability].

## <a name="protecting-relay-endpoints-against-datacenter-outages-or-disasters"></a>Ochranu koncových bodů předávání proti výpadkům datacenter nebo havárie
Geografická replikace koncových bodů předávání umožňuje služba, která zveřejňuje toobe koncový bod předávání, která je dostupná v hello přítomnost výpadků služby Service Bus. tooachieve geografická replikace, hello služby musíte vytvořit dva koncové body předávání v různých oborech názvů. obory názvů Hello se musí nacházet v různých datových centrech a dva koncové body hello musí mít odlišné názvy. Například můžete v části dostupný primární koncový bod **contosoPrimary.servicebus.windows.net/myPrimaryService**při jeho protějšku sekundární dostupný v části **contosoSecondary.servicebus.windows.net /mySecondaryService**.

Služba Hello pak naslouchá na obou koncových bodů a klient vyvolat hello služby přes koncový bod. Klientská aplikace náhodně vybere jeden z hello předávací jako hello primární koncový bod a odešle jeho žádost toohello aktivní koncový bod. Pokud hello operace selže s kódem chyby, tato chyba znamená, že tohoto koncového bodu předávání hello není k dispozici. aplikace Hello otevře zálohování koncový bod kanálu toohello a znovu vydá požadavek hello. V tomto bodě hello zálohování koncové body a hello active přepínač role: hello klientská aplikace považovat hello staré aktivní koncový bod toobe hello nový koncový bod zálohování a hello staré zálohování koncový bod toobe hello nové aktivní koncový bod. Pokud obě odeslat operace selže, zůstanou nezměněny hello role hello dvě entity a vrátí se chyba.

Hello [geografická replikace s sběrnice zpráv přes předávací službu] [ Geo-replication with Service Bus relayed Messages] příklad ukazuje, jak předává tooreplicate.

## <a name="protecting-queues-and-topics-against-datacenter-outages-or-disasters"></a>Ochrana fronty a témata proti výpadkům datacenter nebo havárie
tooachieve odolnost proti výpadkům datového centra při pomocí zprostředkovaného zasílání zpráv, Service Bus podporuje dva přístupy: *active* a *pasivní* replikace. Pro každý přístup Pokud se daný fronta nebo téma musí zůstat přístupný v hello přítomnost výpadku datového centra můžete vytvořit ji v oba obory názvů. Obě entit může mít hello stejný název. Například dostupný primární fronty pod **contosoPrimary.servicebus.windows.net/myQueue**při jeho protějšku sekundární dostupný v části **contosoSecondary.servicebus.windows.net/myQueue**.

Pokud hello aplikace nevyžaduje komunikaci trvalé odesílatele k příjemce, můžete implementovat aplikace hello trvanlivý klienta fronty tooprevent ztrátě a tooshield hello odesílatele zprávy z jakékoli přechodné chyby Service Bus.

## <a name="active-replication"></a>Replikace služby Active
Replikace služby Active používá entity v oba obory názvů pro všechny operace. Libovolného klienta, který odešle zprávu odešle dvě kopie hello stejnou zprávu. Hello první kopie se odesílá toohello primární entity (například **contosoPrimary.servicebus.windows.net/sales**), a druhé kopie uvítací zprávu hello je odeslána toohello sekundární entity (například  **contosoSecondary.servicebus.windows.net/sales**).

Klient obdrží z obou front zpráv. procesy příjemce Hello hello prvního kopírování zpráv a je potlačeno hello druhé kopie. duplicitní zprávy toosuppress, odesílatel hello musí označit každou zprávu s jedinečným identifikátorem. Obě kopie hello zprávy musí být označené hello stejný identifikátor. Můžete použít hello [BrokeredMessage.MessageId] [ BrokeredMessage.MessageId] nebo [BrokeredMessage.Label] [ BrokeredMessage.Label] vlastnosti, nebo vlastní vlastnost tootag hello zpráva. seznam zpráv, které již přijal musí zachovat Hello příjemce.

Hello [geografická replikace se Service Bus zprostředkované zprávy] [ Geo-replication with Service Bus Brokered Messages] příklad znázorňuje active replikaci entity pro zasílání zpráv.

> [!NOTE]
> replikace služby active přístup Hello zdvojnásobí hello počet operací, proto tento přístup může vést toohigher náklady.
> 
> 

## <a name="passive-replication"></a>Pasivní replikace
V případě selhání bez hello pasivní replikace používá jenom jeden hello dvě entit pro zasílání zpráv. Klient odešle hello zpráva toohello active entity. Pokud operace hello u hello active entity selže s kódem chyby, která určuje, že active entity hello hostitele může být k dispozici datacenter hello, hello klient odešle kopii hello zpráva toohello zálohování entity. V tomto bodě hello aktivní a entity zálohování hello přepínač role: hello odesílání klienta považuje hello staré active entity toobe hello nové zálohování entity a hello staré zálohování entita je hello nové aktivní entity. Pokud obě odeslat operace selže, zůstanou nezměněny hello role hello dvě entity a vrátí se chyba.

Klient obdrží z obou front zpráv. Protože je pravděpodobné, že hello příjemce obdrží dvě kopie hello stejné zprávy, hello příjemce musí potlačení duplicitních zpráv. Můžete potlačit duplicit v hello stejným způsobem, jak popisuje pro replikaci active.

Obecně platí je levnější než replikace služby active pasivní replikace, protože ve většině případů je prováděna pouze jednu operaci. Latence, propustnosti a peněžní náklady jsou identické toohello nereplikované scénář.

Při použití pasivní replikace, v hello následující scénáře zprávy můžou ke ztrátě nebo přijímat dvakrát:

* **Zpráva zpoždění nebo ztrátě**: předpokládají, že hello sender úspěšně odeslal primární fronta zpráv m1 toohello a pak nedostupný hello fronty než hello příjemce obdrží m1. Hello odesílatel odešle sekundární fronty následnou zprávu m2 toohello. Pokud hello primární fronta je dočasně nedostupný, hello příjemce obdrží m1 po hello fronty opět k dispozici. V případě havárie hello příjemce obdržet nikdy m1.
* **Duplicitní příjem**: předpokládá, že hello odesílatel odešle zpráva m toohello primární fronty. Service Bus úspěšně zpracuje m, ale selže toosend odpověď. Po hello odeslat operaci časový limit, hello odesílatel odešle identické kopii m toohello sekundární fronty. Pokud příjemce hello je možné tooreceive hello první kopie m před primární fronty hello nedostupný, hello příjemce obdrží obě kopie na přibližně hello m stejnou dobu. Pokud příjemce hello není možné tooreceive hello první kopii m před primární fronty hello nedostupný, hello příjemce původně přijímá pouze hello druhé kopie m, ale pak obdrží druhé kopie m, když primární fronty hello je dostupná.

Hello [geografická replikace se Service Bus zprostředkované zprávy] [ Geo-replication with Service Bus Brokered Messages] příklad znázorňuje pasivní replikaci entity pro zasílání zpráv.

## <a name="next-steps"></a>Další kroky
toolearn Další informace o zotavení po havárii, najdete v těchto článcích:

* [Kontinuita podnikových procesů Azure SQL Database][Azure SQL Database Business Continuity]
* [Návrh odolný aplikací pro Azure.][Azure resiliency technical guidance]

[Service Bus Authentication]: service-bus-authentication-and-authorization.md
[Partitioned messaging entities]: service-bus-partitioning.md
[Asynchronous messaging patterns and high availability]: service-bus-async-messaging.md#failure-of-service-bus-within-an-azure-datacenter
[Geo-replication with Service Bus Relayed Messages]: http://code.msdn.microsoft.com/Geo-replication-with-16dbfecd
[BrokeredMessage.MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId
[BrokeredMessage.Label]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label
[Geo-replication with Service Bus Brokered Messages]: http://code.msdn.microsoft.com/Geo-replication-with-f5688664
[Azure SQL Database Business Continuity]: ../sql-database/sql-database-business-continuity.md
[Azure resiliency technical guidance]: /azure/architecture/resiliency
