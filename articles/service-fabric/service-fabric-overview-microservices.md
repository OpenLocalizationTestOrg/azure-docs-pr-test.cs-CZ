---
title: aaaIntroduction toomicroservices v Azure | Microsoft Docs
description: "Přehled vytváření cloudových aplikací s přístupem mikroslužeb Proč je důležité pro vývoj moderních aplikací a jak Azure Service Fabric poskytuje platformu tooachieve to."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: fae2be85-0ab4-4cd3-9d1f-e0d95fe1959b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/02/2017
ms.author: msfussell
ms.openlocfilehash: b11920b9105e7575390e8fcf0d1ef6ab3c632978
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="why-a-microservices-approach-toobuilding-applications"></a>Proč mikroslužeb přístupu toobuilding aplikace?
Jako vývojáři softwaru není nic nového v tom, jak jsme vezměte v úvahu řešení aplikace do součásti aplikace. Je hello centrální zlepší orientaci objektu, abstrakce softwaru a componentization. V současné době této factorization obvykle tootake hello formu třídy a rozhraní mezi sdílené knihovny a technologických vrstev. Vrstvený přístup je obvykle prováděné s back-end úložiště, střední vrstvu obchodní logiky a klientské uživatelské rozhraní (UI). Co *má* změnit přes hello posledních několika letech je, že jsme, jako vývojáři, jsou vytváření distribuované aplikace, které jsou pro hello cloud a řídí hello firmy.

Změna obchodních potřeb Hello jsou:

* Služba, která je vytvořená a funguje na škálování tooreach zákazníkům nové zeměpisné oblasti (například).
* Rychlejší doručování funkce a možnosti toobe možné toorespond toocustomer požadavky agilní způsobem.
* Vyšší náklady na tooreduce využití prostředků.

Mají vliv na těchto obchodních potřeb *jak* jsme sestavení aplikace.

Další informace o přístupu hello Azure toomicroservices, najdete v tématu [Mikroslužeb: revolution aplikace používá technologii hello cloudu](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/).

## <a name="monolithic-vs-microservice-design-approach"></a>Monolitický oproti přístup v rámci mikroslužbu návrhu
Všechny aplikace v průběhu času vyvíjejí. Tím, že je užitečné toopeople momentální úspěšné aplikace. Neúspěšná aplikace není momentální a nakonec jsou zastaralé. změní otázku Hello: kolik víte o své požadavky ještě dnes, a co bude budou v budoucí hello? Předpokládejme například že vytváříte vytváření sestav aplikace pro oddělení. Jste si jisti, že hello aplikace zůstává v rámci oboru hello vaší společnosti a že jsou krátkodobou hello sestavy. Zvoleného přístupu se liší od, Řekněme, vytváření služby, které nabízí videa obsahu tootens milionů zákazníků. 

V některých případech přístupu k něčemu out hello dveře jako testování konceptu je hello řízení faktoru, když víte, že aplikace hello můžete později přepracovali. Není moc bod v přepsání technici něco, který získá nikdy nepoužívali. To je obvykle engineering kompromis hello. Na druhé straně, hello, pokud společností mluvit o sestavení pro hello cloud, očekávání hello je růstu a využití. Hello problémem je, že nepředvídatelným růstu a škálování. Možnost tooprototype toobe rádi bychom znali rychle a také zároveň budete vědět, že jsme na cestu toodeal s budoucí úspěch. Toto je hello štíhlého spuštění přístup: sestavení, měřit, zjistěte a procházení.

Během hello letopočtu klient server jsme obdělávány toofocus na sestavení vrstvené aplikace pomocí konkrétní technologie v jednotlivých úrovních. termín Hello *monolitický* vyplývá aplikace pro tyto přístupy. rozhraní Hello obdělávány toobe mezi vrstvami hello a více úzce párované návrh byl použit mezi součástmi v jednotlivých vrstvách. Vývojáři určené a započítané třídy, které byly zkompilovány do knihovny a spojené do několika spustitelné soubory a knihovny DLL. 

Existují výhody toosuch monolitický návrhu přístup. Je často toodesign jednodušší a má rychlejší volání mezi součástmi, protože tyto volání je často přes meziprocesová komunikace (IPC). Navíc všechny testy jeden produkt, který obvykle toobe další osoby – zdroj efektivní. Hello nevýhodou je, že je úzkou párování mezi vrstvami vrstev a nemůže škálovat jednotlivých součástí. Pokud potřebujete tooperform opravy nebo upgrade, máte toowait pro ostatní toofinish jejich testování. Je obtížnější toobe agile.

Mikroslužeb adres tyto downsides a další úzce zarovnané s hello předcházející obchodní požadavky, ale mají také výhody a závazky. Hello mikroslužeb výhody každé z nich obvykle zapouzdří jednodušší obchodní funkce, které škálování směrem nahoru nebo dolů, testovací, nasadit a spravovat nezávisle. Jednou z důležité výhody mikroslužbu přístupu je, že týmy se řídí více podle obchodních scénářů než technologii, která hello vrstvený přístup podporuje. V praxi menší týmy vyvíjet mikroslužbu závislosti na scénáři zákazníka a používat žádné technologie, kterou si zvolí. 

Jinými slovy hello organizace nepotřebuje toostandardize technická toomaintain mikroslužbu aplikace. Jednotlivé týmy, vlastní služby můžete udělat, co má smysl pro ně podle tým odborných znalostí nebo co je nejvhodnější toosolve hello problém. V praxi sadu doporučené technologií, jako je například konkrétní NoSQL uložení nebo webové aplikační rozhraní, je vhodnější.

Nevýhodou Hello mikroslužeb dodává při správě hello zvýšit počet samostatné entity a práci s složitější nasazení a správa verzí. Síťový provoz mezi hello mikroslužeb zvyšuje i hello odpovídající síťovou latenci. Velké množství chatty, podrobné služby jsou recept na při důvodů výkonu. Bez nástroje toohelp zobrazit tyto závislosti, je pevný příliš "viz" hello celý systém. 

Ujistěte se, standardy přístup mikroslužbu hello fungovat podle schválení na to, jak toocommunicate a probíhá odolný vůči chybám pouze hello věcí musí ze služby, nikoli pevné smluv. Je důležité toodefine tyto kontrakty předem hello návrhu, protože services aktualizovat nezávisle na sobě navzájem. Další popis poprvé použit k návrhu, s přístupem mikroslužeb je "podrobných orientované na služby architektura (SOA)."

***V nejjednodušším hello mikroslužeb návrhu přístup je o odpojeného federace služeb, s tooeach nezávislé změny a dohodnuté standardy pro komunikaci.***

Jako další cloudové aplikace vytváří, osoby zjistíte, že tento rozložením hello celkové aplikace do služby nezávislé, zaměřené na scénář je lépe dlouhodobý přístup.

## <a name="comparison-between-application-development-approaches"></a>Porovnání mezi přístupy vývoj aplikací
![Vývoj aplikací platformy Service Fabric][Image1]

1) Monolitický aplikace obsahuje funkce specifické pro doménu a obvykle dělení funkční vrstvy, jako je web, obchodních a data.

2) Škálování monolitický aplikace klonováním na více serverů a virtuálních počítačů nebo kontejnerů.

3) Mikroslužbu aplikace odděluje funkce do samostatné služby menší.

4) Hello mikroslužeb přístup měřítka odhlašování nasazením na jednotlivé služby nezávisle, vytváření instancí těchto služeb mezi servery a virtuálních počítačů nebo kontejnerů.

Navrhování s mikroslužbu přístup není všelék pro všechny projekty, ale lépe zarovnat hello obchodním cílům popsané výše. Počínaje monolitický přístup může být přijatelné, pokud víte, že budete mít hello možnost toorework hello kód později do návrhu mikroslužeb. Běžně začínat monolitický aplikace a pomalu jej rozdělte ve fázích, počínaje hello funkčním oblastem, které je třeba toobe více škálovatelné nebo agile.

toosummarize, hello mikroslužbu přístup je toocompose aplikace z mnoha malé služeb. Hello služby spuštěny v kontejnerech, které jsou nasazeny v clusteru s podporou počítačů. Menší týmy vyvíjet služba, která se zaměřuje na scénáři nezávisle, verze, nasazení a testování škálovat jednotlivé služby, takže můžete rozvíjet celá aplikace hello.

## <a name="what-is-a-microservice"></a>Co je mikroslužbu?
Existují různé definice mikroslužeb. Pokud hledáte hello Internet, najdete mnoho užitečné zdroje, které poskytují vlastní hlediska a definice. Ale většinu hello následující vlastnosti mikroslužeb jsou široce mezi:

* Zapouzdření zákazník nebo obchodní scénáře. Co je hello problému, který se řešení?
* Vyvinuté malé technickému týmu.
* Napsané v žádném programovací jazyk a použít libovolnou architekturu.
* Sestávají z kódu a (volitelně) stavu obě tyto položky jsou nezávisle verzí, nasazení a škálovat.
* Pracovat s další mikroslužeb přes dobře definované rozhraní a protokoly.
* Jedinečné názvy (URL) použili tooresolve jejich umístění.
* Zůstala konzistentní a k dispozici v hello přítomnost chyb.

Je možné vytvořit tyto charakteristiky do shrnutí:

***Mikroslužbu aplikace se skládají z malých, nezávisle na nástroji verzí a škálovatelné zaměřený na zákazníky služby, které vzájemně komunikovat pomocí standardních protokolů s dobře definované rozhraní.***

Jsme popsaná hello první dva body v předcházející části hello a rozbalení na a vysvětlení, teď hello ostatní.

### <a name="written-in-any-programming-language-and-use-any-framework"></a>Napsané v žádném programovací jazyk a použít libovolnou architekturu
Vývojáři jsme musí být volné toochoose jazyk nebo rozhraní, které chceme, v závislosti na našich dovednosti nebo potřeby hello hello služby. V některých služeb může být hodnota hello výkonu výhod C++ nad všemi else. V jiných služeb může být nejdůležitější hello snadné spravovaný vývoj v jazyce C# nebo Java. V některých případech může být nutné toouse knihovnu konkrétní partnerskou technologie úložiště dat, nebo způsobem vystavení hello tooclients služby.

Po výběru technologie pocházet toohello provozní nebo správu životního cyklu a škálování služby hello.

### <a name="allows-code-and-state-toobe-independently-versioned-deployed-and-scaled"></a>Umožňuje toobe kód a stav nasazení a škálovat nezávisle verzí
Ale zvolíte toowrite mikroslužeb, hello kód a volitelně hello stavu by měl nezávisle nasazení, upgrade a škálování. To je ve skutečnosti jedním z hello těžší toosolve problémy, protože pochází dolů tooyour výběr technologií. Pro škálování, je pochopení, jak toopartition (nebo komplikovaného skládání architektury) i hello kód a stavu náročné. Při hello kód a stavu použijte samostatné technologií, které je dnes běžné, třeba hello skriptů nasazení pro vaše mikroslužbu toobe možné toocope s škálování je i. To je taky o její agilnost a flexibilitu, takže některé hello mikroslužeb upgradem bez nutnosti tooupgrade všechny najednou.

Vrácení toohello monolitický versus mikroslužbu přístup na chvíli hello následující diagram znázorňuje hello rozdíly v hello přístup toostoring stavu.

#### <a name="state-storage-between-application-styles"></a>Stav úložiště mezi aplikací styly
![Úložiště stavu platformy Service Fabric][Image2]

***Hello monolitický přístup na levé straně hello má jedné databáze a úrovně konkrétní technologie.***

***Hello mikroslužeb přístup na pravém hello má graf vzájemně propojena mikroslužeb, kde stav je obvykle vymezená toohello mikroslužbu a se používají různé technologie.***

V rámci monolitický přístupu obvykle aplikace hello používá jednu databázi. Hello výhodou je, že se jedná o jediného umístění, takže je easy toodeploy. Jednotlivé komponenty může mít jednu tabulku toostore její stav. Týmy potřebovat samostatný stav toostrictly, který je výzvu. Nevyhnutelnou existují temptations tooadd nové sloupce tooan stávající zákazník tabulky, nezadávejte spojení mezi tabulkami a vytvoření závislosti ve vrstvě úložiště hello. Jakmile k tomu dojde, nelze škálovat jednotlivých součástí. 

V hello mikroslužeb přístupu každá služba spravuje a ukládá svůj stav. Každá služba je zodpovědná za škálování kód a stavu požadavky hello společně toomeet hello služby. Nevýhodou je, že pokud je potřeba toocreate zobrazení nebo dotazy, dat aplikace, budete potřebovat tooquery v obchodech různorodých stavu. To je obvykle vyřeší tak, že samostatné mikroslužbu, který vytvoří zobrazení v kolekci mikroslužeb. Pokud potřebujete více režimu dotazů na hello data tooperform, každý mikroslužby zvažte zápis jeho tooa datového skladu služba dat pro offline analýzu.

Správa verzí je konkrétní toohello nasazené verzi mikroslužbu, že několik různých verzí, nasazení a spuštění vedle sebe. Správa verzí adresy hello scénáře, kde na novější verzi mikroslužbu během upgradu se nezdaří a musí tooroll back tooan starší verze. Hello jiné scénáře pro správu verzí provádí A/B-style testování, kde různé uživatele zaznamenat různé verze služby hello. Například je běžné tooupgrade mikroslužbu pro konkrétní sadu zákazníci tootest nové funkce před distribucí další široce. Po správu životního cyklu mikroslužeb tento teď přináší nám toocommunication mezi nimi.

### <a name="interacts-with-other-microservices-over-well-defined-interfaces-and-protocols"></a>Komunikuje s další mikroslužeb přes dobře definované rozhraní a protokoly
Toto téma vyžaduje málo pozornost tady, protože rozsáhlou dokumentaci o architektuře orientované na služby, který byl publikován přes hello posledních 10 letech popisuje komunikačních schémat. Obecně platí používá komunikace služby REST přístup s protokoly HTTP a TCP a XML nebo JSON jako formát serializace hello. Z hlediska rozhraní je o přijetí hello přístup v rámci webové návrhu. Ale nic zabraňuje pomocí binární protokoly nebo vlastní formáty dat. Připravte pro osoby toohave těžší čas pomocí vaší mikroslužeb, pokud jsou zveřejněno k dispozici.

### <a name="has-a-unique-name-url-used-tooresolve-its-location"></a>Jedinečný název (URL) použila tooresolve jeho umístění
Mějte na paměti, jak jsme zachovat varování, že přístup mikroslužbu hello je jako hello web? Jako hello web vaše mikroslužbu musí toobe adresovatelné vždy, když je spuštěná. Pokud přemýšlíte o počítačích a které z nich je spuštěna konkrétní mikroslužbu, přejděte věcí chybný rychle. 

V hello stejný způsobem této DNS řeší konkrétní adresu URL tooa konkrétní počítač, váš mikroslužbu musí toohave jedinečný název tak, aby jeho aktuálního umístění zjistitelný. Mikroslužeb potřebovat adresovatelné názvy, díky kterým hello infrastrukturu, která běží nezávisle. Z toho vyplývá, že existuje interakci mezi nasazení služby a jak se zjistí, protože je potřeba toobe registru služby. Stejným způsobem Pokud se počítači nezdaří, hello registru služby musí zjistíte kde hello služba je nyní spuštěna. 

To nám přináší další téma toohello: odolnost a konzistence.

### <a name="remains-consistent-and-available-in-hello-presence-of-failures"></a>Zůstane konzistentní a k dispozici v hello přítomnost chyb
Práci s neočekávanými chybami je jedním z hello nejtěžší toosolve problémy, zejména v distribuovaného systému. Velká část hello kód, který jsme zapsat jako vývojáři je zpracování výjimek, a to je také kde hello většinu času je věnovaný testování. problém Hello je složitější než psaní kódu toohandle selhání. Co se stane, když selže hello počítač se spuštěným systémem hello mikroslužbu? Pouze potřebujete toodetect toto selhání mikroslužbu (pevné potíže na vlastní), ale něco musíte taky toorestart vaše mikroslužby. 

Mikroslužbu potřebuje odolné toofailures toobe a restartujte často na jiném počítači, z důvodů dostupnosti. To také obsahuje dolů toohello stavu, který byl uložen jménem hello mikroslužbu, kde hello mikroslužbu můžete obnovit tento stav z, a jestli mikroslužbu hello je možné toorestart úspěšně. Jinými slovy je potřeba, toobe odolnost v výpočetní hello (hello procesu restartování), a také odolnost proti hello stavu nebo data (žádná data ztrátě a hello data konzistentní).

Při další scénáře, jako je například při selhání dojít během upgradu aplikace jsou kombinovaných Hello problémy odolnosti. Hello mikroslužbu, práce s hello nasazení systému, nepotřebuje toorecover. Je také nutné toothen rozhodněte, zda lze pokračovat toomove dopředného toohello novější verzi nebo místo toho vrácení předchozí verze toomaintain tooa konzistentním stavu. Otázky, jako jestli dostatek počítače jsou k dispozici tookeep postoupíte a jak toorecover předchozích verzích hello mikroslužbu potřebovat toobe považována za. To vyžaduje hello mikroslužbu tooemit stavu informace toobe možné toomake tato rozhodnutí.

### <a name="reports-health-and-diagnostics"></a>Sestavy stav a Diagnostika
To nemusí připadat zřejmé, bývá často přehlížen, ale mikroslužbu musí nahlásit jeho stav a Diagnostika. Jinak se jen málo informací z hlediska operace. Korelace diagnostických událostí mezi sadu nezávislých služeb a práci s zkosí hodiny počítače, které toomake smysl pro hello – pořadí událostí je náročné. V hello stejným způsobem jako interakci s mikroslužbu přes protokoly dohodnuté a data formátů, existuje ukáže potřebu standardizace v tom, jak toolog stavu a diagnostických událostí, které nakonec skončili v události úložiště pro dotazování a zobrazení. V rámci mikroslužeb přístupu je klíč, na formát jeden protokolování shodnou různé týmy. Je potřeba toobe diagnostických událostí konzistentní přístup tooviewing v hello aplikaci jako celek.

Stav se liší od diagnostiky. Stav je o hello mikroslužbu reporting její aktuální stav tootake příslušné akce. Dobrým příkladem pracuje s dostupností toomaintain mechanismy pro upgrade a nasazení. I když služba může být aktuálně v chybném stavu z důvodu tooa procesu havárií nebo restartování počítače, může být služba hello stále funkční. Hello poslední věcí, kterou je třeba je toomake tento nejhorší provedení upgradu. Hello nejlepším postupem je nejdřív toodo šetření, nebo můžete nastavit dobu toorecover mikroslužbu hello. Stav události z mikroslužbu Pomozte nám informovaně rozhodnout a v důsledku toho vám pomohou vytvořit samoopravitelné služby.

## <a name="service-fabric-as-a-microservices-platform"></a>Service Fabric jako platformu mikroslužeb
Azure Service Fabric této služby z přechod společností Microsoft z dodáváme produkty pole, které byly obvykle monolitický ve stylu, toodelivering služby. Hello prostředí sestavování a provoz velké služby, například Azure SQL Database a Azure Cosmos databáze, ve tvaru Service Fabric. Platforma Hello vyvinuly časem více služeb přijatá ho. Je důležité Service Fabric měl toorun nejen v Azure, ale i v samostatných nasazeních systému Windows Server.

***cílem Hello Service Fabric je toosolve hello pevný problémy při vytváření a spouštění služby a efektivně využívat prostředky infrastruktury tak, aby týmy můžete řešení obchodních problémů mikroslužeb přístup.***

Service Fabric nabízí tři obecné oblasti toohelp vytvářet aplikace, které používají mikroslužeb přístup:

* Platforma, která poskytuje služby toodeploy systému, upgradu, zjišťovat a restartujte služby se nezdařila, zjišťování služby, směrování zpráv, Správa stavu a sledování stavu. Tyto systémové služby platí povolit mnoho společných vlastností hello mikroslužeb popsaných výše.
* Možnost toodeploy aplikace buď běží v kontejnerech nebo jako zpracuje. Service Fabric je kontejner a procesu orchestrator.
* Rozhraní API pro programování produktivní, toohelp vytvářet aplikace jako mikroslužeb: [ASP.NET Core, Reliable Actors a spolehlivé služby](service-fabric-choose-framework.md). Můžete použít všechny toobuild kódu vaší mikroslužby. Ale tato rozhraní API proveďte úlohy hello více přehledné a jejich integraci s platformou hello na podrobnější úrovni. Tímto způsobem, například stav a Diagnostika informace můžete získat, nebo můžete využít integrovanou vysokou dostupnost.

***Service Fabric nerozlišuje na tom, jak sestavit služby a všechny technologii můžete použít. Však poskytuje integrované programovací rozhraní API, aby byla jednodušší toobuild mikroslužeb.***

### <a name="migrating-existing-applications-tooservice-fabric"></a>Migrace existujících aplikací tooService prostředků infrastruktury
Klíče přístup tooService prostředků infrastruktury je tooreuse existující kód, který lze potom modernized s novou mikroslužeb. Existují pět fází tooapplication modernizace a může spuštění a zastavení na kterémkoli z fází hello. Toto jsou;

1) Trvat tradiční monolitický aplikace
2) Navýšení a posunutí – použijte kontejnery nebo hosta spustitelné soubory toohost existující kód v Service Fabric.
3) Modernizace - nové mikroslužeb přidat společně se stávající kontejnerizované kód. 
4) Inovacemi. Zajistěte – rozdělit hello monolitický založený výhradně na potřeba mikroslužeb.
5) Převede na mikroslužeb - hello transformace existující monolitický aplikace nebo vytváření nových greenfield aplikací.

![Migrace tooMicroservices][Image3]

Je důležité tooemphasis znovu, můžete je **spuštění a zastavení na kterémkoli z těchto fází**, nemůžete se budeme nuceni toomoved toohello další fáze. Nyní Podíváme se na příklady pro každou z těchto fází.

**Navýšení a posunutí** – jsou zrušení velkého počtu společností a přejdou existující monolitický aplikace do kontejnerů toofor dvou důvodů, proč;

- Snížení nákladů z důvodu tooconsolidation a odebrání stávající hardware nebo spuštění aplikací v vyšší hustotu. 
- Konzistentní nasazení kontrakt mezi vývoj a provoz.

Snížení nákladů, jsou srozumitelné a v rámci Microsoftu velkého počtu existujících aplikací probíhá kontejnerizované jednoduše toomillions dolarů. Těžší tooevaluate, ale stejně důležitá je konzistentní nasazení. Zobrazuje, že vývojáři mohou stále být technologie hello volné toochoose této sady je, ale hello operace bude pouze přijmout toodeploy jediný způsob a správě těchto aplikací. Ho nebude hello operací s toodeal s hello složitosti mnoha různých technologií nebo vynucení vývojáři tooonly zvolte některé formáty. V podstatě každá aplikace je kontejnerizované do bitové kopie samostatná nasazení.

Mnoho organizací zastavit sem. Už mají hello výhod kontejnery a Service Fabric nabízí prostředí pro kompletní správu hello z nasazení, upgrady, Správa verzí, odvolání, stav monitorování atd.

**Modernizace** -je hello přidání nových služeb společně se stávající kontejnerizované kód. Pokud chcete toowrite nový kód, je nejlepší toodecide tootake malé kroky dolů hello mikroslužeb cesta. To může přidávat nové koncový bod REST API nebo nové obchodní logiku. Tímto způsobem, spusťte na cestu hello mikroslužeb nové sestavení a postup vývoje a jejich nasazení.

**Inovacemi. Zajistěte** -mějte na paměti tyto původní změna obchodních potřeb při spuštění hello tohoto článku, které řídí přístup mikroslužeb hello? V této fázi hello je rozhodnutí, jsou tyto aplikace aktuální toomy situaci a pokud ano, potřebuji toostart rozdělení hello monolitu nebo innovating. Je zde například když se databáze stane kritický bod zpracování, protože je používán jako fronty pracovního postupu. Jako číslo hello pracovního postupu žádostí o zvýšení hello pracovních potřeb toobe distribuován pro škálování. Proto pro tento konkrétním hello aplikace, která není škálování, nebo můžete potřebovat tooupdate více často, to se rozdělí na mikroslužbu a inovacemi. Zajistěte. 

**Převede na mikroslužeb** – to je, kde vaše aplikace je plně složený z (nebo rozložená do) mikroslužeb. Zde tooreach, které jste vytvořili hello mikroslužeb cesty. Můžete spustit zde, ale toodo to bez toohelp platformy mikroslužeb je důležité investice. 

### <a name="are-microservices-right-for-my-application"></a>Jsou mikroslužeb pro Moje aplikace?
Podle potřeby. Jsme došlo se, že jako více týmů v aplikaci Microsoft začal toobuild pro cloud hello firmy z důvodů, kolika z nich uskutečňovat hello výhody tak mikroslužbu jako přístup. Bing, například má byla vývoj mikroslužeb ve vyhledávání let. Pro jiné týmy hello mikroslužeb přístup byl nový. Týmy nalezen, že byly pevné problémy toosolve mimo jejich oblasti základní sílu. Z tohoto důvodu Service Fabric získávají trakční hello technologií výběru pro vytváření služeb.

cílem Hello Service Fabric je tooreduce hello složitosti vytváření aplikací s přístupem mikroslužbu, tak, že nemáte toogo prostřednictvím jako mnoho nákladných redesigns. Začněte v malém rozsahu, škálovat v případě potřeby, přestat používat služby, přidat nové a momentální s zákazníka využití je hello přístup. Jsme také vědět, že existuje mnoho dalších problémů ještě toobe vyřeší toomake mikroslužeb více srozumitelné pro Většina vývojářů. Kontejnery a programovací model objektu actor hello jsou příklady malé kroky v tomto směru a jsme si jisti, že budou další inovace vznikat toomake to snazší.
 
<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->

## <a name="next-steps"></a>Další kroky
* [Přehled terminologie Service Fabric](service-fabric-technical-overview.md)
* [Mikroslužeb: Aplikaci revolution používá technologii hello cloudu](https://azure.microsoft.com/en-us/blog/microservices-an-application-revolution-powered-by-the-cloud/)

[Image1]: media/service-fabric-overview-microservices/monolithic-vs-micro.png
[Image2]: media/service-fabric-overview-microservices/statemonolithic-vs-micro.png
[Image3]: media/service-fabric-overview-microservices/microservices-migration.png
