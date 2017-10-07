---
title: "aaaAMQP 1.0 v příručce protokol Azure Service Bus a Event Hubs | Microsoft Docs"
description: "Průvodce tooexpressions protokolu a popis protokolu AMQP 1.0 v Azure Service Bus a Event Hubs"
services: service-bus-messaging,event-hubs
documentationcenter: .net
author: clemensv
manager: timlt
editor: 
ms.assetid: d2d3d540-8760-426a-ad10-d5128ce0ae24
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2017
ms.author: clemensv;hillaryc;sethm
ms.openlocfilehash: 882ce0fc84af11d9f61bc95dc3e4db0b67b2b020
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# V Průvodci Azure Service Bus a Event Hubs protokolu AMQP 1.0

Hello Advanced 1.0 protokol služby Řízení front zpráv je standardizovaná rámcovacích a přenos protokol asynchronně, bezpečně a spolehlivě přenos zpráv mezi dvěma účastníky. Je primární protokol hello Azure zasílání zpráv Service Bus a Azure Event Hubs. Obě služby také podporují protokol HTTPS. Hello proprietární SBMP protokol, který je také podporována je vyřazován považuje AMQP.

AMQP 1.0 je hello výsledek spolupráce široký odvětví, který shrnuta dodavatelé middleware, jako je Microsoft a Red Hat, s mnoha uživateli zasílání zpráv middleware, jako je například Chase Nováková JP představující oboru finančních služeb hello. Hello technické standardizace fórum o požadavky na protokol a rozšíření AMQP hello je OASIS a dosáhla formální schválení jako mezinárodní standard jako ISO/IEC 19494.

## Cíle

Tento článek stručně shrnuje hello základní koncepty hello protokolu AMQP 1.0 zasílání zpráv specifikace společně s malou sadu koncept rozšíření specifikace, které se aktuálně dokončují v technický výbor hello OASIS AMQP a vysvětluje, jak Azure Service Bus implementuje a je založený na těchto specifikací.

cílem Hello je pro všechny vývojáře pomocí jakékoli existující zásobníku klienta protokolu AMQP 1.0 na jakékoli platformě toobe možné toointeract s Azure Service Bus prostřednictvím protokolu AMQP 1.0.

Běžné pro obecné účely zásobníky protokolu AMQP 1.0, jako je například Apache kanálem nebo AMQP.NET Lite již implementovat všechny gesta jádra protokolu AMQP 1.0. Tyto základní gesta jsou někdy obklopeno vyšší úrovně rozhraní API; Apache kanálem i nabízí dva, hello imperativní API Messenger a hello reaktivní reaktoru rozhraní API.

V následující diskusní hello jsme předpokládají, že hello správu připojení AMQP, relací a odkazy a hello zpracování přenosů rámečkem a řízení toku jsou zpracovávány hello příslušných zásobníku (jako je například Apache kanálem-C) a nevyžadují mnohem, pokud existuje konkrétní pozornost vývojáři aplikace. Abstraktně předpokládáme hello existenci několik rozhraní API primitiv jako hello možnost tooconnect a toocreate určitou formu *odesílatele* a *příjemce* abstrakce objekty, které se pak mít některé tvar `send()`a `receive()` operace, v uvedeném pořadí.

Když hovoříte o pokročilých funkcí služby Azure Service Bus, jako je například procházení zpráva nebo správu relací, tyto funkce jsou vysvětlené v podmínkách AMQP, ale také jako vrstvený pseudo implementace nad tato abstrakce předpokládané rozhraní API.

## Co je AMQP?

AMQP je protokol rámcovacích a přenosu. Rámcovacích znamená, že poskytuje strukturu pro binární datové proudy, které toku v obou směrech připojení k síti. Struktura Hello poskytuje vymezení pro odlišné bloky dat, volá *rámce*, toobe vyměňují mezi hello připojené stranami. Možnosti přenosu Hello Ujistěte se, že obě strany komunikuje můžete vytvořit sdílené znalosti o při se přenáší rámce, i když přenosů musí být považováno za dokončené.

Na rozdíl od dříve vypršela platnost koncept verze vytváří pracovní skupinou hello AMQP používají několik zprostředkovatelé zprávu není protokol AMQP 1.0 konečné a standardizovaném hello pracovní skupiny stanovit hello přítomnost zprostředkovatele zpráv nebo žádné konkrétní topologie pro entity uvnitř zprostředkovatele zpráv.

Hello protokol lze použít pro symetrické komunikaci peer-to-peer, pro interakci s zprostředkovatelé zprávy, které podporují fronty a publikování a přihlášení k odběru entity, jak to dělá Azure Service Bus. Může taky sloužit pro interakci s infrastrukturu zasílání zpráv kde vzory hello interakce se liší od regulární fronty, stejně jako hello případě Azure Event Hubs. Centra událostí funguje podobně jako u front, když se události posílají tooit, ale pracuje jako služba sériové úložiště při události se načítají z. podobá poněkud páskové jednotky. Hello klient vybere posun do hello k dispozici datového proudu a pak je zpracovat všechny události z tohoto posunutí toohello nejnovější dostupné.

Hello protokol AMQP 1.0 je navrženou toobe rozšiřitelný, povolení další tooenhance specifikace jeho možnosti. znázornění Hello tři rozšíření specifikace popisovaný v tomto dokumentu. Pro komunikaci přes existující infrastrukturu HTTPS/Websocket, kde konfigurace hello nativní porty TCP protokolu AMQP může být obtížné specifikace vazby definuje jak toolayer AMQP přes objekty WebSockets. Pro interakci s hello infrastrukturu zasílání zpráv v požadavku nebo odpovědi definuje způsobem pro účely správy nebo tooprovide pokročilé funkce, specifikace správy AMQP hello hello vyžaduje základní interakce primitiv. Integrace modelu federované autorizace, hello AMQP deklarace identity na základě zabezpečení specifikace definuje jak tooassociate a obnovení tokeny autorizace, které jsou spojené s odkazy.

## Základní scénáře AMQP

Tato část vysvětluje hello základní použití protokolu AMQP 1.0 s Azure Service Bus, která zahrnuje vytváření připojení, relací a odkazy a přenosu tooand zprávy ze sběrnice entitami, jako je například fronty, témata a odběry.

Hello nejvíce autoritativní zdroj toolearn o tom, jak funguje AMQP je specifikace hello protokolu AMQP 1.0, ale hello specifikace byla zapsána tooprecisely Průvodce implementaci a ne tooteach hello protokolu. Tato část se zaměřuje na představení tolik terminologie podle potřeby pro popisující, jak Service Bus používá protokolu AMQP 1.0. Pro komplexnější tooAMQP úvod, jakož i širší diskuzi o protokolu AMQP 1.0, můžete zkontrolovat [tento kurz video][this video course].

### Připojení a relace

AMQP volání hello komunikaci programy *kontejnery*; tyto obsahovat *uzly*, které jsou hello komunikaci entity v těchto kontejnerech. Fronta může být taková uzlu. AMQP umožňuje multiplexní, takže jednoho připojení lze použít pro mnoho cesty komunikaci mezi uzly. například klientem aplikace můžete souběžně přijímat z jedné fronty a fronty pro odesílání tooanother přes hello stejné síťové připojení.

![][1]

Hello síťové připojení proto ukotvena hello kontejneru. Inicializuje hello kontejneru v roli klienta hello provedení kontejner tooa služby odchozí TCP soketu připojení v hello příjemce role, která naslouchá a přijímá příchozí připojení TCP. metoda handshake připojení Hello zahrnuje vyjednávání hello verzi protokolu, deklarace nebo vyjednávání hello použití Transport Level Security (TLS/SSL) a ověření typu handshake ověřování/autorizace v oboru hello připojení, která je založená na SASL.

Azure Service Bus vyžaduje použití protokolu TLS hello za všech okolností. Podporuje připojení přes TCP port 5671, jímž hello připojení TCP je nejdřív jako překryvný obrázek s TLS před vstupem hello AMQP protokol metody handshake a taky podporuje připojení přes TCP port 5672, při kterém hello server okamžitě nabízí povinné upgrade tooTLS připojení pomocí protokolu AMQP předepsané modelu hello. Vazba AMQP Websocket Hello Vytvoří tunel přes port TCP 443, který je pak ekvivalentní tooAMQP 5671 připojení.

Po nastavení připojení hello a TLS, Service Bus nabízí dvě možnosti mechanismus SASL:

* PROSTÝ SASL se běžně používá pro předávání uživatelské jméno a heslo serveru tooa přihlašovací údaje. Service Bus nemá účty, ale s názvem [pravidla zabezpečení sdíleného přístupu](service-bus-sas.md), který práva a jsou spojeny s klíčem. Hello název pravidla se používá jako hello uživatelské jméno a klíč hello (jak je text kódováním base64, pomocí) se používá jako hello heslo. Hello práv spojených s hello zvolené pravidlo řídí hello operace v hello propojení.
* ANONYMNÍ SASL se používá pro obcházení SASL autorizace při hello klienta chce toouse hello deklarace identity na základě zabezpečení (CBS) modelu, který je popsán později. Pomocí této možnosti připojení klienta lze navázat anonymně po krátkou dobu během které hello klienta mohou komunikovat pouze s koncovým bodem CBS hello se musí dokončit handshake hello modelu CBS.

Po navázání připojení pro přenos hello hello kontejnery, každý deklarovat hello maximální velikost rámce jsou ochotni toohandle a po časový limit nečinnosti se budete jednostranně odpojit, pokud není žádná aktivita na hello připojení.

Také se deklarovat, kolik souběžných kanály jsou podporovány. Kanál je přenos jednosměrný odchozí, virtuální cestu nad hello připojení. Relace trvá kanál z každé hello vzájemně propojena kontejnery tooform obousměrná komunikace cesty.

Relace mají model řízení toku na základě okno; Po vytvoření relace jednotlivých stran deklaruje, kolik snímků je ochotná tooaccept do jeho okna. Jak přenést technologie hello strany exchange rámce, rámce vyplnění, že zastavit okno a přenosy při zaplnění hello okno a dokud okno hello získá resetovat nebo rozšířit pomocí hello *toku performative* (*performative*je hello AMQP termín pro úroveň protokolu gesta vyměňují mezi hello dvě strany).

Tento model se systémem Windows je přibližně obdobná jako toohello TCP koncept řízení toku na základě okno, ale na úrovni relace hello uvnitř hello soketu. Hello protokolu koncept povolení pro více souběžných relací existuje tak, aby provoz s vysokou prioritou může rushed po omezenému normální provoz, jako je na express dráhy highway.

Azure Service Bus aktuálně používá přesně jednu relaci pro každé připojení. Hello maximální velikost na rámce Service Bus je 262 144 bajtů (256 kB) pro standardní Service Bus a Event Hubs. Je 1 048 576 (1 MB) pro Service Bus Premium. Service Bus nepředstavuje žádné konkrétní windows omezení úrovně relace, ale resetování hello okno pravidelně jako součást řízení toku na úrovni propojení (viz [hello další části](#links)).

Připojení, kanálů a relací jsou dočasné. Sbalí hello základní připojení, připojení, musí být obnovila tunelu TLS, SASL autorizační kontext a relací.

### Odkazy

AMQP přenosu zpráv přes odkazy. Odkaz je cesta ke komunikaci vytvořených v relaci, která umožňuje přenášení zpráv v jednom směru; vyjednávání stav Hello přenos je nad hello odkaz a obousměrné mezi hello připojené stranami.

![][2]

Odkazy můžete vytvořit buď kontejneru existující relaci, takže je AMQP liší od mnoha jiné protokoly, včetně HTTP a MQTT, kde je spuštění hello přenosy a přenos cestu exkluzivní oprávnění hello strany vytváření a kdykoliv Hello připojení soketu.

odkaz inicializaci kontejneru Hello požádá hello opačným kontejneru tooaccept odkaz a zvolí roli odesílatele nebo příjemce. Proto buď kontejneru můžete zahájit vytvořením jednosměrný nebo cesty obousměrnou komunikaci s hello pozdější modelován jako dvojice odkazů.

Odkazy jsou s názvem a související s uzly. Jak jsme uvedli v hello začátku, jsou uzly hello komunikaci entity uvnitř kontejneru.

Uzlu v Service Bus, je přímo ekvivalentní tooa frontu, téma, předplatné nebo dílčí fronta nedoručených zpráv z fronty nebo předplatné. Název uzlu Hello používá v AMQP je proto hello relativní název entity hello uvnitř oboru názvů Service Bus hello. Pokud je název fronty **Moje_fronta**, který je také jeho název uzlu AMQP. Odběr tématu následuje hello rozhraní API HTTP konvencí se rozděleny na kolekci prostředků "odběry" a proto odběr **sub** nebo téma **mytopic** má název uzlu AMQP hello **mytopic nebo předplatných nebo dílčí**.

Hello připojujícího se klienta je taky požadované toouse název místního uzlu pro vytváření odkazů. Service Bus není doporučený o tyto názvy a nebude je interpretovat. Zásobníky klienta protokolu AMQP 1.0 obecně používat schéma tooassure, že jsou tyto názvy dočasných uzlů jedinečné v oboru hello hello klienta.

### Přenosy

Po vytvoření odkazu zprávy lze přenášet prostřednictvím tohoto připojení. V protokolu AMQP, je přenos provést s gesto explicitní protocol (hello *přenos* performative), přesune zprávu z odesílatele tooreceiver přes propojení. Přenos je dokončena ho po "vyrovnání", což znamená, že mají obě strany navázat sdílené pochopení hello výsledek přenos.

![][3]

V nejjednodušším případě hello můžete zvolit hello odesílatele zprávy toosend "předem vyrovnány,", což znamená, že hello klienta není zájem o hello výsledek a příjemce hello neposkytuje žádné zpětnou vazbu o hello výsledek operace hello. Tento režim je podporována Service Bus na úrovni protokolu AMQP hello, ale nejsou viditelné v žádném z rozhraní API klienta hello.

Hello regulární případ je, že zprávy odesílány nevyrovnaná a hello příjemce potom označuje přijetí nebo odmítnutí pomocí hello *dispozice* performative. Odmítání nastane, když příjemce hello nemůže přijmout uvítací zprávu z jakéhokoli důvodu a hello odmítání zpráva obsahuje informace o hello důvod, což je strukturu chyba definované AMQP. Pokud zprávy odmítnuto z důvodu chyby toointernal uvnitř Service Bus, služba hello vrací doplňující informace uvnitř této struktury, které je možné pro zajištění diagnostiky pracovníky toosupport pomocných parametrů, pokud jsou vyplnění žádosti o podporu. Další informace o chybách dozvíte později.

Speciální formu odmítání je hello *vydané* stavu, což znamená, že příjemce hello nemá žádné technické námitky toohello přenos, ale také žádné zájem o vyrovnání hello přenosu. Zda případ existuje, například když zprávu doručuje tooa Service Bus klienta a klient hello zvolí příliš "abandon" uvítací zprávu protože hello pracovní vyplývající z zpracování uvítací zprávu; nelze provést Hello doručení zpráv, samotné není poškozeno. Varianta tento stav je hello *upravil* stavu, který umožňuje změny toohello zprávu jako jeho vydání. Tento stav není v současné době používán Service Bus.

Hello AMQP 1.0 – specifikace definuje další dispozice stavu názvem *přijata*, který pomáhá konkrétně toohandle odkaz obnovení. Obnovení odkaz umožňuje rekonstrukce hello stav odkazu a všechny čekající dodávky nad nové připojení a relace, kdy došlo ke ztrátě hello předchozí připojení a relace.

Service Bus nepodporuje obnovení odkaz; Pokud klient hello ztratí hello připojení tooService sběrnice nevyrovnaná zpráva přenosu čekající, přenos zpráv dojde ke ztrátě a hello klienta musíte znovu připojit, znovu vytvořit odkaz hello a opakujte hello přenosu.

Jako takový Service Bus a Event Hubs podporující "alespoň jednou" přenos kde hello odesílatele zprávy hello byla uložena a přijata si být jistí, ale nepodporují přenosy v hello AMQP úrovni, kde by systém hello pokusit o toorecover "přesně jedno" Hello odkaz a pokračovat toonegotiate hello doručení stavu tooavoid duplikace hello přenos zpráv.

odešle, toocompensate pro možné duplicitní Service Bus podporuje detekce duplicitních jako volitelná funkce v fronty a témata. Detekce duplicitních záznamů hello identifikátory zpráv všech příchozích zpráv během uživatelem definované časového okna a potom bezobslužně rušilo všechny zprávy odeslané s hello stejné identifikátory zpráv této stejného časového období.

### Řízení toku

Kromě toho řízení toku na úrovni relace toohello modelu, který dřív popsané, každý odkaz má svou vlastní model řízení toku. Řízení toku na úrovni relace chrání hello kontejneru z s toohandle příliš mnoho rámce v po řízení toku na úrovni propojení vloží aplikace hello stará o tom, kolik zpráv se chce toohandle pomocí odkazu a kdy.

![][4]

Na propojení, přenos může pouze dojít v případě hello odesílatele má dost *propojit platební*. Platební odkaz je čítač nastaven hello příjemce pomocí hello *toku* performative, která má obor tooa odkaz. Jakmile hello odesílatele je přiřazena platební odkaz, pokusí toouse až tento platební podle doručování zpráv. Každý snižuje doručení zprávy hello zbývající kredit odkaz o 1. Při použití platební odkaz hello dodávky zastavit.

Když Service Bus je v roli hello příjemce, okamžitě poskytuje hello odesílatele s dostatečným odkaz platební tak, aby okamžitě odesílání zpráv. Jako odkaz platební se používá, Service Bus v určitých intervalech odesílá *toku* performative toohello odesílatele tooupdate zůstatek Dal hello odkaz.

Service Bus v roli hello odesílatele, odešle zprávy toouse až žádný kredit zbývající odkaz.

"Přijímat" volání na úrovni hello API překládá do *toku* performative odesílány tooService sběrnice klientem hello a Service Bus spotřebuje, který nejprve úvěrového podle trvá hello k dispozici, odemčený zpráv z fronty hello, uzamčení, a přenáší ho. Pokud není žádná snadno dostupné pro doručení, žádný kredit zbývající podle všech propojení navázat s konkrétní entity zůstává zaznamenané v pořadí podle příchodem, zprávy jsou zamčené a jakmile budou k dispozici, toouse přenést všechny zbývající kredit.

Hello zámku na zprávu vydání hello přenos po vyrovnání do jednoho z terminálu stavů hello *přijata*, *odmítl*, nebo *vydané*. uvítací zprávu odebere ze služby Service Bus je stavu terminálu hello *přijata*. Zůstane v Service Bus a doručuje toohello Další příjemce, když přenos hello žádné hello dosáhne jiných stavů. Service Bus automaticky přesune uvítací zprávu do fronty nedoručených zpráv hello entity, když se dosáhne povolený pro entitu hello kvůli verzemi nebo zamítnutí toorepeated hello doručení maximální počet.

I když hello API pro Service Bus nezveřejňují přímo tato možnost dnes, můžete klienta protokolu AMQP nižší úrovně použít hello odkaz platební model tooturn hello "stylu pro vyžádání obsahu" interakci vydávat jednu jednotku kredit pro každý požadavek na přijetí do modelu "push stylu" Po vydání velký počet odkaz kredity a pak přijímat zprávy, jakmile budou k dispozici bez další interakce. Prostřednictvím hello je podporované nabízené [MessagingFactory.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_PrefetchCount) nebo [MessageReceiver.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_PrefetchCount) nastavení vlastností. Když jsou nula, klienta AMQP hello použit jako platební odkaz hello.

V tomto kontextu jeho důležité toounderstand který hello hodiny pro vypršení platnosti hello hello zámku na uvítací zprávu uvnitř hello entity spustí, když hello zprávy jsou převzaty z hello entity, pokud není uvítací zprávu umístí hello přenosu. Vždy, když klient hello určuje připravenosti tooreceive zprávy vydáním platební odkaz, je proto očekávané toobe aktivně vyžádání zprávy v síti hello a být připravené toohandle je. V opačném případě zámek zprávy hello mohla vypršet platnost před i doručuje uvítací zprávu. Hello použitím řízení toku platební odkaz přímo odrážet hello okamžitou připravenosti toodeal s dostupné zpráv odeslaných toohello příjemce.

Souhrnně hello následující části obsahují schéma přehled performative toku hello během různých interakce rozhraní API. Každá část obsahuje jiné logické operace. Některé z těchto interakcí může být "opožděné," což znamená, že se může provést pouze v případě potřeby. Vytvoření odesílatele zprávy nemusí způsobit interakce sítě, dokud první zprávu hello se odesílá nebo se požadovaná.

Hello šipky v následující tabulce hello zobrazují směr toku performative hello.

#### Vytvoření příjemce zprávu

| Klient | Service Bus |
| --- | --- |
| --> připojit ()<br/>název = {název odkazu}<br/>zpracování = {číselné popisovač}<br/>role =**příjemce**,<br/>Zdroj = {název entity}<br/>cíl = {id klienta odkaz}<br/>) |Klient připojí tooentity jako příjemce |
| Service Bus odpovědi připojení ukončení hello propojení |<--připojení ()<br/>název = {název odkazu}<br/>zpracování = {číselné popisovač}<br/>role =**odesílatele**,<br/>Zdroj = {název entity}<br/>cíl = {id klienta odkaz}<br/>) |

#### Vytvoření odesílatele zprávy

| Klient | Service Bus |
| --- | --- |
| --> připojit ()<br/>název = {název odkazu}<br/>zpracování = {číselné popisovač}<br/>role =**odesílatele**,<br/>Zdroj = {id klienta odkaz},<br/>cíl = {název entity}<br/>) |Žádná akce |
| Žádná akce |<--připojení ()<br/>název = {název odkazu}<br/>zpracování = {číselné popisovač}<br/>role =**příjemce**,<br/>Zdroj = {id klienta odkaz},<br/>cíl = {název entity}<br/>) |

#### Vytvoření odesílatele zprávy (chyba)

| Klient | Service Bus |
| --- | --- |
| --> připojit ()<br/>název = {název odkazu}<br/>zpracování = {číselné popisovač}<br/>role =**odesílatele**,<br/>Zdroj = {id klienta odkaz},<br/>cíl = {název entity}<br/>) |Žádná akce |
| Žádná akce |<--připojení ()<br/>název = {název odkazu}<br/>zpracování = {číselné popisovač}<br/>role =**příjemce**,<br/>Zdroj = null,<br/>cíl = null<br/>)<br/><br/><--odpojit ()<br/>zpracování = {číselné popisovač}<br/>Uzavřený =**true**,<br/>chyba = {{chyba info}<br/>) |

#### Příjemce nebo odesílatele zprávy zavřít

| Klient | Service Bus |
| --- | --- |
| --> odpojit ()<br/>zpracování = {číselné popisovač}<br/>Uzavřený =**true**<br/>) |Žádná akce |
| Žádná akce |<--odpojit ()<br/>zpracování = {číselné popisovač}<br/>Uzavřený =**true**<br/>) |

#### Odeslat (úspěch)

| Klient | Service Bus |
| --- | --- |
| přenos (--><br/>doručení id = {číselné popisovač}<br/>doručení tag = {binární popisovač}<br/>Vyrovnané =**false**,, více =**false**,<br/>Stav =**null**,<br/>Obnovit =**false**<br/>) |Žádná akce |
| Žádná akce |<--(dispozice<br/>role = příjemce,<br/>první = {doručení id},<br/>poslední = {doručení id},<br/>Vyrovnané =**true**,<br/>Stav =**přijato**<br/>) |

#### Odeslat (chyba)

| Klient | Service Bus |
| --- | --- |
| přenos (--><br/>doručení id = {číselné popisovač}<br/>doručení tag = {binární popisovač}<br/>Vyrovnané =**false**,, více =**false**,<br/>Stav =**null**,<br/>Obnovit =**false**<br/>) |Žádná akce |
| Žádná akce |<--(dispozice<br/>role = příjemce,<br/>první = {doručení id},<br/>poslední = {doručení id},<br/>Vyrovnané =**true**,<br/>Stav =**odmítl**()<br/>chyba = {{chyba info}<br/>)<br/>) |

#### Přijmout

| Klient | Service Bus |
| --- | --- |
| tok (--><br/>odkaz platební = 1<br/>) |Žádná akce |
| Žádná akce |< přenosu ()<br/>doručení id = {číselné popisovač}<br/>doručení tag = {binární popisovač}<br/>Vyrovnané =**false**,<br/>více =**false**,<br/>Stav =**null**,<br/>Obnovit =**false**<br/>) |
| dispozice (--><br/>role =**příjemce**,<br/>první = {doručení id},<br/>poslední = {doručení id},<br/>Vyrovnané =**true**,<br/>Stav =**přijato**<br/>) |Žádná akce |

#### Více zpráva zobrazí

| Klient | Service Bus |
| --- | --- |
| tok (--><br/>odkaz platební = 3<br/>) |Žádná akce |
| Žádná akce |< přenosu ()<br/>doručení id = {číselné popisovač}<br/>doručení tag = {binární popisovač}<br/>Vyrovnané =**false**,<br/>více =**false**,<br/>Stav =**null**,<br/>Obnovit =**false**<br/>) |
| Žádná akce |< přenosu ()<br/>doručení id = {číselné popisovač + 1},<br/>doručení tag = {binární popisovač}<br/>Vyrovnané =**false**,<br/>více =**false**,<br/>Stav =**null**,<br/>Obnovit =**false**<br/>) |
| Žádná akce |< přenosu ()<br/>doručení id = {číselné popisovač + 2},<br/>doručení tag = {binární popisovač}<br/>Vyrovnané =**false**,<br/>více =**false**,<br/>Stav =**null**,<br/>Obnovit =**false**<br/>) |
| dispozice (--><br/>role = příjemce,<br/>první = {doručení id},<br/>poslední = {id doručení + 2}<br/>Vyrovnané =**true**,<br/>Stav =**přijato**<br/>) |Žádná akce |

### Zprávy

Hello následující oddíly popisují vlastnosti z hello standardní AMQP zpráva oddíly, které jsou používány Service Bus a jak jsou mapovány toohello sadu rozhraní API služby Service Bus.

#### záhlaví

| Název pole | Využití | Název rozhraní API |
| --- | --- | --- |
| trvanlivý |- |- |
| Priorita |- |- |
| Hodnota TTL |Čas toolive pro tuto zprávu |[TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive) |
| první nabyvatel |- |- |
| Počet doručení |- |[DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeliveryCount) |

#### properties

| Název pole | Využití | Název rozhraní API |
| --- | --- | --- |
| id zprávy |Definované aplikací, vlastní identifikátor této zprávy. Použít pro vyhledávání duplicit. |[MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) |
| id uživatele |Identifikátor uživatele definované aplikací, není interpretovat Service Bus. |Není k dispozici prostřednictvím hello rozhraní API služby Service Bus. |
| příliš|Identifikátor cílové definované aplikací, není interpretovat Service Bus. |[K](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_To) |
| Předmět |Identifikátor účel definované aplikací zprávy není interpretovat Service Bus. |[Popisek](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) |
| odpověď příliš|Indikátor definované aplikací odpovědi path není interpretovat Service Bus. |[ReplyTo](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ReplyTo) |
| id korelace |Identifikátor korelace definované aplikací, není interpretovat Service Bus. |[CorrelationId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_CorrelationId) |
| Typ obsahu |Definované aplikací ukazatel typu obsahu pro text hello, není interpretovat Service Bus. |[Typ obsahu](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ContentType) |
| kódování obsahu |Definované aplikací kódování obsahu indikátor pro text hello, není interpretovat Service Bus. |Není k dispozici prostřednictvím hello rozhraní API služby Service Bus. |
| absolutní čas vypršení platnosti |Deklaruje, na které absolutní rychlých hello uplynutí platnost zprávy vyprší. Na vstupu (záhlaví TTL pozorovanou), ignorována autoritativní na výstup. |[ExpiresAtUtc](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ExpiresAtUtc) |
| čas vytvoření |Deklaruje, na které čas hello byla zpráva vytvořena. Která nepoužívá Service Bus |Není k dispozici prostřednictvím hello rozhraní API služby Service Bus. |
| id skupiny |Identifikátor definované aplikací pro související sadu zpráv. Použít pro Service Bus relace. |[ID relace](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId) |
| skupiny pořadí |Čítač identifikace hello relativní pořadové číslo zprávy hello v relaci. Ignorovat služby Service Bus. |Není k dispozici prostřednictvím hello rozhraní API služby Service Bus. |
| odpověď na skupiny id |- |[ReplyToSessionId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ReplyToSessionId) |

## Pokročilé funkce služby Service Bus

Tato část obsahuje rozšířené možnosti služby Azure Service Bus, které jsou založeny na tooAMQP rozšíření koncept, aktuálně vyvíjených v hello OASIS technický výbor pro AMQP. Service Bus implementuje hello nejnovější verze tyto koncepty a přijímá změny zavedené jako tyto koncepty dosáhne standardní stavu.

> [!NOTE]
> Service Bus zasílání zpráv, pokročilé operace jsou podporovány prostřednictvím vzor požadavků a odpovědí. Podrobnosti o Hello tyto operace jsou popsány v dokumentu hello [protokolu AMQP 1.0 v Service Bus: na základě požadavku odpověď operace](service-bus-amqp-request-response.md).
> 
> 

### AMQP správy

Specifikace správy AMQP Hello je hello první rozšíření koncept hello zde popsané. Tato specifikace definuje sadu jako vrstva nad hello protokolu AMQP gesta protokolu, které umožňují správu interakce s hello zasílání zpráv infrastruktury prostřednictvím protokolu AMQP. Specifikace Hello definuje obecná operací, jako *vytvořit*, *číst*, *aktualizace*, a *odstranit* pro správu entity uvnitř infrastrukturu zasílání zpráv a sadu operace dotazů.

Všechny tyto gesta vyžadovat interakci požadavků a odpovědí mezi klientem hello a hello infrastrukturu zasílání zpráv, a proto hello specifikace definuje, jak toomodel interakce vzor nad AMQP: hello klient připojí toohello zasílání zpráv infrastruktury, inicializuje relaci a potom vytvoří pár odkazy. Na jeden odkaz hello klienta funguje jako odesílatele a na jiných hello funguje jako příjemce, proto vytvoří pár odkazy, které může fungovat jako obousměrný kanál.

| Logický provoz | Klient | Service Bus |
| --- | --- | --- |
| Vytvořit žádost o odpovědi cestu |--> připojit ()<br/>název = {*název odkazu*},<br/>zpracování = {*číselné popisovač*},<br/>role =**odesílatele**,<br/>Zdroj =**null**,<br/>target = "Správa myentity / $"<br/>) |Žádná akce |
| Vytvořit žádost o odpovědi cestu |Žádná akce |\<--připojení ()<br/>název = {*název odkazu*},<br/>zpracování = {*číselné popisovač*},<br/>role =**příjemce**,<br/>Zdroj = null,<br/>target = "myentity"<br/>) |
| Vytvořit žádost o odpovědi cestu |--> připojit ()<br/>název = {*název odkazu*},<br/>zpracování = {*číselné popisovač*},<br/>role =**příjemce**,<br/>zdroj = "Správa myentity / $",<br/>target = "myclient$ id"<br/>) | |
| Vytvořit žádost o odpovědi cestu |Žádná akce |\<--připojení ()<br/>název = {*název odkazu*},<br/>zpracování = {*číselné popisovač*},<br/>role =**odesílatele**,<br/>zdroj = "myentity",<br/>target = "myclient$ id"<br/>) |

S tuto dvojici odkazy na místě, hello požadavků a odpovědí implementace je jednoduchá: žádost je zpráva odeslána tooan entity uvnitř hello infrastrukturu zasílání zpráv, která funguje s technologií tento vzor. V této zprávě požadavku, hello *Komu* pole hello *vlastnosti* oddíl je nastavit toohello *cíl* identifikátor hello odkaz, na které toodeliver hello odpovědi. Hello zpracování entity zpracuje požadavek hello a pak doručí hello odpověď přes hello propojit, jehož *cíl* identifikátor odpovídá hello uvedené *Komu* identifikátor.

Hello vzor samozřejmě vyžaduje, že kontejneru hello klienta a identifikátor hello klientem generovaná pro cíl odpovědi hello jsou jedinečné napříč všech klientů a z bezpečnostních důvodů se taky obtížné toopredict.

Hello výměny zpráv použít pro protokol hello management a všech ostatních protokolů této hello pomocí stejného vzoru dojít na úrovni aplikace hello; nedefinují nové gesta úrovni protokolu AMQP. Který je úmyslné, takže aplikace mohou využít okamžitou výhod těchto rozšíření s kompatibilní zásobníky protokolu AMQP 1.0.

Service Bus neimplementuje aktuálně žádnou z funkcí základní hello hello správu specifikace, ale vzor požadavků a odpovědí hello definované specifikací správu hello je základní pro funkci hello deklarace identity na základě zabezpečení a téměř všechny Rozšířené možnosti, které jsou popsané v následující části hello Hello.

### Ověření na základě deklarace identity

Koncept specifikace AMQP deklarace identity na základě autorizace (CBS) Hello vychází hello správu specifikace požadavků a odpovědí vzor a popisuje, jak toouse federovaný tokeny zabezpečení s AMQP zobecněný model.

model zabezpečení výchozí Hello AMQP popsané v úvodu hello je založený na SASL a integruje se službou hello metoda handshake připojení protokolu AMQP. Pomocí SASL má hello výhody, která poskytuje rozšiřitelnou model pro kterou se definovaly sadu mechanismy z libovolného protokolu, který dříve leans na SASL můžete využívat. Mezi tyto mechanismy jsou pro přenos uživatelská jména a hesla, zabezpečení na úrovni tooTLS toobind "Externí", "ANONYMNÍ" tooexpress hello absenci explicitní ověřování nebo autorizace a mnoha rozličných další mechanismy, které umožňují "PLAIN" předávání ověřování nebo autorizace přihlašovací údaje nebo tokenů.

Integrace SASL na AMQP má dva nevýhody:

* Všechny přihlašovací údaje a tokeny jsou toohello vymezená připojení. Infrastrukturu zasílání zpráv může být vhodné tooprovide rozlišené řízení přístupu na základě za entity; například povolení hello nosiče tokenu toosend tooqueue A ale tooqueue B. S hello autorizační kontext ukotvena hello připojení není možné toouse jednoho připojení a ještě použít jiný přístupových tokenů pro fronty A a B. fronty
* Přístupové tokeny jsou obvykle platné pouze po omezenou dobu. Tato platnosti vyžaduje hello uživatele tooperiodically znovu načíst tokeny a poskytuje možnost toohello vydavatel tokenu toorefuse vydání nový token, pokud došlo ke změně oprávnění pro přístup k hello uživatele. AMQP připojení může trvat dlouhou dobu. Hello SASL model jenom poskytuje prvního tooset token v době připojení, což znamená, že buď hello infrastrukturu zasílání zpráv má toodisconnect hello klienta při hello tokenu vyprší platnost, nebo vyžaduje tooaccept hello riziko povolení nepřetržitou komunikaci s Klient kdo má přístupová práva může mít odvolaný v mezičase hello.

Hello AMQP CBS specifikace implementované Service Bus, umožňuje elegantní alternativní řešení pro obě tyto problémy: umožňuje klientům tooassociate přístupové tokeny s každým uzlem a tooupdate ty tokeny před vypršením jejich platnosti, aniž by to ovlivnilo hello tok zpráv.

Správa virtuálního uzlu s názvem definuje CBS *$cbs*, toobe poskytuje infrastrukturu zasílání zpráv hello. uzlu správy Hello přijímá tokeny jménem jiných uzlů v hello infrastruktury zasílání zpráv.

Gesto protokol Hello je požadavek nebo odpověď exchange jako definované specifikací správu hello. Aby znamená hello klient vytvoří pár propojení s hello *$cbs* uzel a potom předává požadavek na hello odchozí propojení a potom počká hello odpověď na hello příchozí propojení.

zpráva požadavku Hello má následující vlastnosti aplikace hello:

| Klíč | Nepovinné | Typ hodnoty | Hodnota obsahu |
| --- | --- | --- | --- |
| operace |Ne |Řetězec |**PUT tokenu** |
| type |Ne |Řetězec |Typ Hello hello token, provádí požadavek put. |
| jméno |Ne |Řetězec |Hello "cílová skupina" toowhich hello token platí. |
| vypršení platnosti |Ano |časové razítko |čas vypršení platnosti Hello hello tokenu. |

Hello *název* vlastnost identifikuje hello entity, které hello musí být token přidruženy k. V Service Bus je je hello cesta toohello fronta nebo téma/odběr. Hello *typ* vlastnost určuje typ tokenu hello:

| Typ tokenu | Popis tokenu | Typ těla zprávy | Poznámky |
| --- | --- | --- | --- |
| amqp:jwt |Webového tokenu JSON (JWT) |AMQP hodnota (string) |Ještě není k dispozici. |
| amqp:swt |Jednoduchého webového tokenu (SWT) |AMQP hodnota (string) |Podporováno pouze pro SWT tokeny vydané službou AAD/ACS |
| servicebus.Windows.NET:sastoken |Token SAS sběrnice služby |AMQP hodnota (string) |- |

Tokeny udělit práva. Service Bus, že zná tři základní práva: "Odeslat" umožňuje odesílání, příjem umožňuje "Naslouchání" a "Manage" umožňuje manipulace s těmito entity. SWT tokeny vydané službou AAD/ACS explicitně zahrnovat tato práva jako deklarace identity. Tokeny SAS sběrnice služby naleznete toorules nakonfigurované na oboru názvů hello nebo entity, a tato pravidla jsou nakonfigurované s právy. Podpisový hello token s hello klíč přidružený k této pravidlo umožňuje proto hello tokenu express hello příslušných práv. token Hello přidružené entitu s využitím *put token* hello povolí připojení klienta toointeract s entitou hello za hello tokenu práva. Odkaz certifikovaného klienta hello hello *odesílatele* role vyžaduje právo; hello "Odeslat", s ohledem na hello *příjemce* role vyžaduje práva "Naslouchat" hello.

Hello zpráva s odpovědí obsahuje následující hello *vlastnosti aplikace* hodnoty

| Klíč | Nepovinné | Typ hodnoty | Hodnota obsahu |
| --- | --- | --- | --- |
| Stavový kód |Ne |celá čísla |Kód odpovědi HTTP **[RFC2616]**. |
| Popis stavu |Ano |Řetězec |Popis stavu hello. |

Hello klienta můžete volat *put token* opakovaně a pro všechny entity v hello infrastrukturu zasílání zpráv. Hello tokeny jsou vymezená toohello aktuálního klienta a ukotvena hello aktuální připojení, server hello význam zahodí všechny tokeny, zachované při připojení hello zahodí.

aktuální implementace sběrnice Hello CBS umožňuje pouze ve spojení s hello metoda SASL "ANONYMNÍ." Připojení protokolem SSL/TLS, musí existovat vždy předchozí toohello SASL handshake.

Hello ANONYMNÍ mechanismus musí podporovat proto hello vybrali klienta protokolu AMQP 1.0. Anonymní přístup znamená, že hello metoda handshake počáteční připojení, včetně vytváření hello počáteční relace se stane bez zároveň budete vědět, kdo je vytvoření hello připojení služby Service Bus.

Po vytvoření připojení hello a relace připojení hello odkazy toohello *$cbs* uzel a odesílání hello *put token* žádosti jsou hello jen povolená operace. Platný token musí být nastavena úspěšně pomocí *put token* požadavku pro některé entity uzel v rámci 20 sekund po hello připojení, v opačném případě hello jednostranně odpojení prostředím Service Bus.

pro udržování přehledu o vypršení platnosti tokenu následně zodpovídá Hello klienta. Když vyprší platnost tokenu, Service Bus neprodleně zahodí všechny odkazy na příslušné entity toohello hello připojení. tooprevent se hello klienta můžete nahradit hello token pro uzel hello novou kdykoli prostřednictvím hello virtuální *$cbs* uzlu správy s hello stejné *put token* gesty a bez získání v hello způsob hello datové části provoz tohoto toky na jiné odkazy.

## Další kroky

Další informace o protokolu AMQP, toolearn najdete na adrese hello následující odkazy:

* [Přehled protokolu AMQP Service Bus]
* [Podpora protokolu AMQP 1.0 témata a fronty Service Bus rozdělena na oddíly]
* [AMQP v Service Bus pro systém Windows Server]

[this video course]: https://www.youtube.com/playlist?list=PLmE4bZU0qx-wAP02i0I7PJWvDWoCytEjD
[1]: ./media/service-bus-amqp-protocol-guide/amqp1.png
[2]: ./media/service-bus-amqp-protocol-guide/amqp2.png
[3]: ./media/service-bus-amqp-protocol-guide/amqp3.png
[4]: ./media/service-bus-amqp-protocol-guide/amqp4.png

[Přehled protokolu AMQP Service Bus]: service-bus-amqp-overview.md
[Podpora protokolu AMQP 1.0 témata a fronty Service Bus rozdělena na oddíly]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[AMQP v Service Bus pro systém Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
