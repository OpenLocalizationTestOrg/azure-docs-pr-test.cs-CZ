---
title: "aaaOverview základy Azure Service Bus | Microsoft Docs"
description: "Úvod toousing Service Bus tooconnect softwaru tooother aplikace Azure."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 12654cdd-82ab-4b95-b56f-08a5a8bbc6f9
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/15/2017
ms.author: sethm
ms.openlocfilehash: 1abd5cf310ef06ba35e1e2489a7c0a07e1797736
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-bus"></a>Azure Service Bus

Zda je aplikace nebo služba běží v cloudu hello nebo lokálně, často potřebuje toointeract s jinými aplikacemi nebo službami. tooprovide toodo široce užitečný způsob, jak tento, nabízí Microsoft Azure Service Bus. Tento článek vypadá na tuto technologii, které popisují, co je a proč byste měli toouse ho.

## <a name="service-bus-fundamentals"></a>Základy služby Service Bus

Různé situace potřebují různé styly komunikace. V některých případech odesílat a přijímat zprávy přes jednoduchou frontu je nejlepším řešením hello. V jiných situacích běžná fronta nestačí a je lepší použít frontu s publikováním a odběrem. V některých případech stačí jen navázat spojení mezi aplikacemi a fronty nejsou vůbec potřeba. Service Bus poskytuje všechny tři možnosti, povolení toointeract vaší aplikace v několika různými způsoby.

Service Bus je víceklientská Cloudová služba, která znamená, že služba hello je sdílet více uživatelů. Vytvoří každý uživatel, jako třeba vývojář aplikací *obor názvů*, pak definuje komunikační mechanizmy hello potřebné v daném oboru názvů. Na obrázku 1 je znázorněno, jak tato architektura vypadá.

![][1]

**Obrázek 1: Service Bus poskytuje víceklientské služby pro připojení aplikací přes hello cloud.**

V oboru názvů můžete použít jednu nebo víc instancí tří různých komunikačních mechanizmů, každý z nich spojuje aplikace jiným způsobem. Hello možnosti jsou:

* *Fronty*, které umožňují jednosměrnou komunikaci. Každá fronta slouží jako prostředník (někdy se tomu říká *zprostředkovatel*), který ukládá odeslané zprávy, dokud nedorazí k příjemci. Každou zprávu přijme jeden příjemce.
* *Témata*, která nabízejí jednosměrnou komunikaci pomocí *odběrů* – jedno téma může mít několik odběrů. Podobně jako u front je téma něco jako zprostředkovatel, ale každý odběr může volitelně používat pouze zprávy filtru tooreceive, které odpovídají konkrétním kritériím.
* *Předávání*, které umožňuje jednosměrnou komunikaci. Na rozdíl od front a témat se při předávání neukládají žádné zprávy; nejedná se o zprostředkování. Místo toho se zprávy jednoduše předají na toohello cílové aplikaci.

Když vytvoříte frontu, téma nebo předávání, musíte je pojmenovat. Tento název kombinace názvem vašeho oboru názvů, vytvoří jedinečný identifikátor pro objekt hello. Aplikace můžete zadat Tento název tooService sběrnice a pak vzájemně prostřednictvím této fronty, tématu nebo toocommunicate předávání. 

aplikace systému Windows toouse některý z těchto objektů v hello předávání scénáři, můžete použít Windows Communication Foundation (WCF). Tato služba se označuje jako [přenos WCF](../service-bus-relay/relay-what-is-it.md). Pro fronty a témata můžou aplikace Windows použít API pro přenos zpráv, které definuje služba Service Bus. toomake tyto objekty jednodušší toouse z aplikací systému Windows, Microsoft sady SDK pro jazyk Java, Node.js a další jazyky. Přístup k frontám a tématům se může získat i pomocí [REST API](/rest/api/servicebus/) přes HTTP(s). 

Je důležité toounderstand, který i v případě, že je služba Bus samotná běží v cloudu hello (to znamená v datových centrech společnosti Microsoft Azure), aplikace, které ho používají, můžou běžet kdekoli. Můžete použít Service Bus tooconnect aplikace běžící v Azure, například nebo aplikacemi spuštěnými ve svém vlastním datovém centru. Můžete ji použít i tooconnect aplikace běžící v Azure nebo jiné cloudové platformy místní aplikace nebo s tabletů a telefonů. Je dokonce tooconnect domácí spotřebiče, senzory a jiná zařízení tooa centrální aplikace nebo tooone jiné. Service Bus je komunikační mechanizmus v cloudu hello, který je přístupný prakticky odkudkoli. Způsob, jakým používáte závisí na to, jaké aplikace musí toodo.

## <a name="queues"></a>Fronty

Předpokládejme, že se že rozhodnete tooconnect dvě aplikace pomocí fronty Service Bus. Na obrázku 2 je taková situace.

![][2]

**Obrázek 2: Fronty Service Bus poskytují jednosměrné asynchronní řízení front zpráv.**

Hello proces je prostý: odesílatel odešle zprávu tooa fronty Service Bus a příjemce převezme zprávy později. Je možné, aby fronta měla pouze jednoho příjemce, jak je znázorněno na obrázku 2. Nebo více aplikací může číst z hello stejné fronty. V druhé situaci hello každou zprávu přečte jen jeden příjemce. Pro službu přetypování byste měli raději použít téma.

Každá zpráva má dvě části: skupinu vlastností ve formě dvojice klíč+hodnota a tělo zprávy. datová část Hello může být binární, text nebo i XML. Jak používají závisí na jaké aplikace se pokouší toodo. Aplikace odešle zprávu o poslední prodej může obsahovat třeba hello vlastnosti **Seller = "Ava"** a **velikost = 10 000**. tělo zprávy Hello může obsahovat naskenovaný systému podepsané smlouvy hello prodej, nebo pokud není k dispozici, zůstat prázdné.

Příjemce může zprávu načíst z fronty Service Bus dvěma různými způsoby. Hello první možnost se jmenuje  *[ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode)*, odstraní zprávu z fronty hello a okamžitě ji odstraní. Tato možnost je jednoduchá, ale pokud hello příjemce spadne, než se dokončí zpracování zprávy hello, dojde ke ztrátě uvítací zprávu. Proto je byla odebrána z fronty hello, ani žádný jiný příjemce k němu přístup. 

Druhá možnost se Hello  *[PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode)*, je určen toohelp se tento problém. Jako **ReceiveAndDelete**, **PeekLock** čtení odebere zprávu z fronty hello. Uvítací zprávu, nedojde ale k odstranění. Místo toho zamkne uvítací zprávu, což neviditelná tooother příjemců, pak čeká na jednu ze tří událostí:

* Pokud příjemce procesy hello hello zprávy úspěšně, zavolá [Complete()](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete), a odstraní fronty hello uvítací zprávu. 
* Pokud se hello příjemce rozhodne, že nelze zpracovat zprávu hello úspěšně, zavolá [Abandon()](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon). fronty Hello pak hello zámku odebere uvítací zprávu a je k dispozici tooother příjemci.
* Volá-li hello příjemce ani jeden z těchto metod v nastaveném časovém intervalu (ve výchozím nastavení 60 sekund), hello fronta předpokládá, že hello příjemce selhal. V takovém případě se chová jako kdyby hello příjemce zavolal **Abandon**, provedení hello zprávy k dispozici tooother příjemci.

Všimněte si, co se tu může stát: hello stejná zpráva se může dodat dvakrát, případně tootwo různým příjemcům. Aplikace, které používají fronty Service Bus, na tuto možnost musí být připravené. toomake detekce duplicitních jednodušší, každá zpráva má jedinečnou [MessageID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) vlastnost, která ve výchozím nastavení hello zůstává stejný bez ohledu na to, jak často uvítací zprávu je pro čtení z fronty. 

Fronty jsou užitečné v mnoha situacích. Umožňují aplikacím toocommunicate i v případě, že nejsou spuštěné ve hello stejný čas, to se hodí především pro dávkové a mobilní aplikace. Fronta s několika příjemci taky poskytuje automatické vyvažování zátěže, protože odeslané zprávy se rozdělují mezi jednotlivé příjemce.

## <a name="topics"></a>Témata

Je užitečná, protože se jedná o, fronty nejsou vždy hello správné řešení. Někdy je lepší použít témata Service Bus. Na obrázku 3 je taková situace.

![][3]

**Obrázek 3: V závislosti na hello filtru může odběratelská aplikace, může přijímat, některé nebo všechny zprávy hello odeslaných tooa tématu Service Bus.**

A *tématu* je podobný ve frontě tooa mnoha způsoby. Odesílatelé odeslání zprávy tooa téma v hello stejným způsobem, kteří odesílají zprávy tooa fronty, a tyto vzhled zprávy hello stejné jako u front. Hello rozdílem je, že témata umožňují každé přijímající aplikace toocreate vlastní *předplatné* definováním *filtru*. Odběratel pak uvidí jen hello zprávy, které odpovídají použitému filtru. Na obrázku 3 je například téma se třemi odběrateli a každý z nich používá vlastní filtr.

* Odběratel 1 přijímá jen zprávy, které obsahují vlastnost hello *Seller = "Ava"*.
* Odběratel 2 přijímá zprávy, které obsahují vlastnost hello *Seller = "Ruby"* a/nebo mají *velikost* vlastnost, jehož hodnota je vyšší než 100 000. Možná je Ruby manažerka prodeje hello, takže chce toosee svoje prodeje a všechny velké prodeje bez ohledu na to, čí jsou.
* Odběratel 3 má nastavený filtr příliš*True*, což znamená, že přijímá všechny zprávy. Například tato aplikace může mít starost udržování auditní stopy a proto je nutné toosee všechny zprávy hello.

Jako v případě front můžou Odběratelé tématu tooa načítat zprávy způsobem buď [ReceiveAndDelete a PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode). Na rozdíl od front se ale do jedné zprávy odeslané tooa tématu lze přijímat pomocí více předplatných. Tento přístup, označovaného jako *publikování a odběr* (nebo *pub nebo sub*), je užitečné v případě, že více aplikací jsou zajímá hello stejné zprávy. Definováním hello vhodný filtr může klepnout každého odběratele do jenom hello část, je nutné toosee hello datového proudu zpráv.

## <a name="relays"></a>Předávání

Fronty i témata nabízejí jednosměrnou asynchronní komunikaci přes zprostředkovatele. Zprávy proudí jen jedním směrem a mezi odesílateli a příjemci není žádné přímé spojení. Co když ale toto připojení nechcete? Předpokládejme, že vaše aplikace potřebují tooboth odesílat a přijímat zprávy, nebo možná má přímé propojení mezi nimi a nepotřebujete zprostředkovatele toostore zprávy. tooaddress scénáře, jako je to, Service Bus poskytuje *předává*, jak ukazuje obrázek 4.

![][4]

**Obrázek 4: Předávání přes Service Bus nabízí synchronní obousměrnou komunikaci mezi aplikacemi.**

Hello napadne otázka tooask asi je toto: Proč ji používat? I když není zapotřebí fronty, proč nutit aplikace komunikovat přes cloudovou službu a nikoli jen komunikovaly přímo? odpověď Hello je, že rozhovoru přímo může být obtížnější než si myslíte.

Předpokládejme, že chcete tooconnect dva místních aplikací, které obě běží ve firemních datových centrech. Každá z těchto aplikací je za firewallem a každé datové centrum pravděpodobně používá překládání adres (NAT). Brána firewall blokuje Hello příchozích dat na všech ale několik portů a NAT znamená, že hello počítače, které jednotlivé aplikace běží na nemá pevnou IP adresu, která můžete dosáhnout přímo z mimo hello datového centra. Bez pomoci specializovaných nástrojů tyto aplikace připojující se přes hello veřejný internet jen těžko.

Service Bus relay může pomoci. toocommunicate obousměrně s předáváním, každá aplikace navázat odchozí připojení TCP se Service Bus a udržuje ho otevřené. Veškerá komunikace mezi dvěma aplikacemi hello přenáší přes tato spojení. Protože se obě spojení vytvořila ze uvnitř hello datacenter hello brána firewall umožňuje příchozí provoz tooeach aplikace bez otevřít nové porty. Tento přístup také obchází problém hello NAT, protože každá aplikace má koncový bod konzistentní v cloudu hello hello komunikace. Výměnou dat přes předávací hello hello aplikace můžou vyhnout hello problémy, které by jinak takovou komunikaci výrazně ztěžovaly. 

předává toouse Service Bus, aplikací závisí na hello Windows Communication Foundation (WCF). Service Bus poskytuje vazby WCF, které ulehčují toointeract aplikací systému Windows přes předávací službu. Aplikace, které už WCF používají obvykle můžete určit jednu z těchto vazeb pak komunikovat tooeach jiných přes předávací službu. Na rozdíl od front a témat ale komunikace přes předávací službu z aplikací na jiné platformě než Windows vyžaduje určitou míru programátorského úsilí, protože nejsou k dispozici žádné standardizované knihovny.

A na rozdíl od front a témat taky aplikace nevytvářejí explicitně předávací službu. Místo toho když aplikace, která tooreceive zprávy, které vytvoří připojení TCP se Service Bus, přenos se vytvoří automaticky. Když hello připojení se ukončí, hello předávací služba odstraní. tooenable předávání hello toofind aplikaci vytvořili konkrétním posluchačem, Service Bus poskytuje registr, který umožňuje aplikacím toolocate konkrétní předávací službu podle názvu.

Předávací služby jsou správným řešením hello když potřebujete přímou komunikaci mezi aplikacemi. Řekněme třeba, že systém pro rezervaci letenek běží na lokálním datovém centru, ke kterému musí mít přístup prodejní místa, mobilní zařízení a další počítače. Aplikace běžící v těchto systémech může spoléhají na předávací služby Service Bus v cloudu toocommunicate hello, kde může být spuštěn.

## <a name="summary"></a>Souhrn

Propojení aplikací vždy patřilo budování kompletních řešení a hello rozsah scénáře, které vyžadují aplikace a služby toocommunicate mezi sebou nastaven tooincrease víc aplikací a zařízení jsou připojené toohello Internetu. Poskytnutím cloudové technologie k dosažení komunikaci prostřednictvím fronty, témata a předávací službu Service Bus cílem je toomake jednodušší tooimplement tuto základní funkci a více zpřístupnil.

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy hello Azure Service Bus, postupujte podle těchto odkazů toolearn Další.

* Jak toouse [fronty Service Bus](service-bus-dotnet-get-started-with-queues.md)
* Jak toouse [témat sběrnice Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)
* Jak toouse [předávání přes Service Bus](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md)
* [Ukázky služby Service Bus](service-bus-samples.md)

[1]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_01_architecture.png
[2]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_02_queues.png
[3]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[4]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_04_relay.png
