---
title: "metodách směrování provozu Traffic Manager - aaaAzure | Microsoft Docs"
description: "To vám pomůže pochopit metody směrování hello jiný přenos používaný správcem provoz články"
services: traffic-manager
documentationcenter: 
author: KumudD
manager: timlt
editor: 
ms.assetid: db1efbf6-6762-4c7a-ac99-675d4eeb54d0
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/13/2017
ms.author: kumud
ms.openlocfilehash: b3eeca63ab5f2b9cd4a3a6b6a8fd3e40059e32b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="traffic-manager-routing-methods"></a>Metody směrování Traffic Manageru

Azure Traffic Manager podporuje čtyři směrování provozu metody toodetermine jak tooroute síťový provoz toohello různé koncové body služby. Správce provozu se vztahuje hello směrování provozu metoda tooeach dotaz DNS, které obdrží. Metoda směrování provozu Hello Určuje, kterému koncovému bodu, vrátí se v odpovědi DNS hello.

Jsou k dispozici v Traffic Manageru čtyři metody směrování provozu:

* **[Priorita](#priority):** vyberte **s prioritou** při toouse koncový bod primární služba pro veškerý provoz a zadejte zálohování v případě, že hello primární nebo hello zálohování koncových bodů nejsou k dispozici.
* **[Vážená](#weighted):** vyberte **vážená** když chcete toodistribute provoz mezi sadu koncových bodů, buď rovnoměrně nebo podle tooweights, které definujete.
* **[Výkon](#performance):** vyberte **výkonu** Pokud máte koncové body v různých zeměpisných místech a chcete, aby koncoví uživatelé toouse hello "nejbližší" endpoint z hlediska hello nejnižší latenci sítě.
* **[Zeměpisná](#geographic):** vyberte **geografické** tak, aby uživatelé směrovanou toospecific koncových bodů (Azure, externí nebo vnořené) na základě které zeměpisné umístění svého dotazu DNS pochází z. To umožňuje Traffic Manager zákazníci tooenable scénáře, kde je důležité zároveň budete vědět, geografické oblasti uživatele a směrování je založeno na tomto. Mezi příklady patří soulad s pověřeními suverenity dat, lokalizace obsah & uživatelské prostředí a měření provoz z různých oblastech.

Všechny profily Traffic Manager zahrnují monitorování stavu koncový bod a koncový bod automatické převzetí služeb při selhání. Další informace najdete v tématu [Traffic Manager koncového bodu monitorování](traffic-manager-monitoring.md). Jeden profil Traffic Manageru můžete použít pouze jednu metodu směrování provozu. Kdykoli můžete vybrat metodu směrování různých provozu pro svůj profil. Změny se použijí během jedné minuty a započítány jsou nedojde k žádnému výpadku. Metody směrování provozu lze spojovat pomocí vnořené profily Traffic Manageru. Vnoření umožňuje sofistikovaná a flexibilní směrování provozu konfigurace, které splňují hello potřebám větší a složité aplikace. Další informace najdete v tématu [vnořené profily Traffic Manageru](traffic-manager-nested-profiles.md).

## <a name = "priority"></a>Metoda směrování provozu s prioritou

Často organizace chce tooprovide spolehlivost pro své služby nasazením jednu nebo více zálohování služeb v případě, že jejich primární služba ocitne mimo provoz. Metoda směrování provozu "Priority" Hello umožňuje Azure zákazníků tooeasily implementaci tohoto vzoru převzetí služeb při selhání.

![Azure Traffic Manager, Priority, směrování provozu – metoda][1]

Hello profil služby Traffic Manager obsahuje seznam koncových bodů služby seřazený podle priority. Ve výchozím nastavení odešle Traffic Manager všechny přenosy toohello primární (nejvyšší priorita) koncového bodu. Pokud hello primární koncový bod není k dispozici, směrování Traffic Manager hello druhý koncový bod toohello provoz. Pokud oba hello primární a sekundární koncových bodů nejsou k dispozici, provoz hello přejde toohello třetí, a tak dále. Dostupnost hello koncového bodu je založena na stav hello nakonfigurované (povolit nebo zakázat) a hello monitorování probíhající koncového bodu.

### <a name="configuring-endpoints"></a>Konfigurace koncových bodů

S Azure Resource Manager, můžete nakonfigurovat Priorita koncového bodu hello explicitně pomocí vlastnosti "priority" hello pro každý koncový bod. Tato vlastnost je hodnotu v rozmezí 1 až 1000. Nižší hodnoty představují vyšší prioritu. Koncové body nemohou sdílet hodnoty priority. Nastavení vlastnosti hello je volitelné. Když tento parametr vynechán, použije se výchozí prioritu podle pořadí hello koncový bod.

##<a name = "weighted"></a>Vážené směrování provozu – metoda
směrování provozu "Vážená" Hello metoda vám umožní toodistribute provoz rovnoměrně nebo toouse předem definované vyvážení.

![Azure Traffic Manager 'Vyvážené' směrování provozu – metoda][2]

Hello vyvážené směrování provozu metoda přiřaďte tooeach koncový bod váhy v konfiguraci profilu Traffic Manageru hello. Váha Hello je celé číslo od 1 too1000. Tento parametr je volitelný. Pokud tento parametr vynechán, provoz správce používá výchozí váhu '1'.

Pro každé přijaté dotazy DNS Traffic Manager náhodně zvolí koncový bod k dispozici. pravděpodobnost Hello podle uvážení koncového bodu je založená na hello váhu přiřazené tooall koncové body k dispozici. Pomocí hello mezi všechny koncové body výsledky v distribuce přenosů i vážené stejné. Pomocí vyšší nebo nižší váhu na konkrétním koncovým bodům způsobí, že tyto koncové body toobe více nebo méně často, vrátí se v odpovědi DNS hello.

Metoda Hello vážené umožňuje některé užitečné scénáře:

* Aplikace postupného upgradu: přidělte procento nový koncový bod provoz tooroute tooa a postupně zvyšovat hello provoz přes čas too100 %.
* Aplikace migrace tooAzure: vytvoření profilu s Azure a externími koncovými body. Upravte hello váhu hello koncové body koncové tooprefer nové body hello.
* Cloud bursting pro dodatečnou kapacitu: rychle rozbalte položku místní nasazení do cloudu hello umístěním za profil Traffic Manageru. Pokud budete potřebovat víc kapacity v cloudu hello, můžete přidat nebo povolit další koncové body a určit, jaké části přenosů přejde tooeach koncový bod.

Hello portálu Azure Resource Manager podporuje hello konfiguraci směrování vyvážené provozu.  Můžete nakonfigurovat váhu používat hello verze správce prostředků Azure PowerShell, rozhraní příkazového řádku a hello rozhraní REST API.

Je důležité toounderstand, odpovědí DNS jsou uložené v mezipaměti klienty a servery DNS rekurzivní hello, které hello klientům používat tooresolve názvy DNS. Toto použití mezipaměti může mít vliv na provoz vyvážené distribuce. Po velký počet hello klienty a servery DNS rekurzivní distribuce přenosů funguje podle očekávání. Ale při malém počtu hello klientů nebo serverů DNS rekurzivní ukládání do mezipaměti může výrazně zkreslit distribuce přenosů hello.

Běžné případy použití patří:

* Vývoj a testování prostředí
* Komunikace aplikace – aplikace
* Aplikace zaměřené na úzké uživatele – základní hodnotu, která sdílejí společné rekurzivní infrastruktury služby DNS (například zaměstnanci společnosti připojení prostřednictvím proxy serveru)

Tyto účinky ukládání do mezipaměti DNS jsou běžné tooall provozu na základě DNS směrování systémů, nejen Azure Traffic Manager. V některých případech může explicitně vymazání hello mezipaměť DNS zadejte alternativní řešení. V jiných případech může být vhodnější alternativní metodu směrování provozu.

## <a name = "performance"></a>Metoda směrování provozu výkonu

Nasazení koncové body ve dvou nebo více umístění mezi hello zeměkouli může zvýšit rychlost odezvy hello mnoho aplikací pomocí směrování provozu toohello umístění, které je 'nejbližší' tooyou. Tuto možnost nabízí Hello metodu směrování provozu 'Výkonu'.

![Azure Traffic Manager, výkon, směrování provozu – metoda][3]

Hello 'nejbližší' koncový bod není nutně nejbližší měřený podle zeměpisného vzdálenost. Místo toho hello metodu směrování provozu, výkon, určuje hello nejbližší koncového bodu na základě měření latence sítě. Správce provozu udržuje Internetu latence tabulky tootrack hello doba odezvy mezi rozsahů IP adres a každé datové centrum Azure.

Správce provozu vyhledává hello zdrojové IP adresy hello příchozích požadavků na DNS v hello Internet latence tabulky. Správce provozu zvolí koncový bod k dispozici v hello datové centrum Azure, který má nejnižší latenci hello pro tento rozsah IP adres a potom vrátí tohoto koncového bodu v hello odpověď DNS.

Jak je popsáno v [jak Traffic Manager funguje](traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager neobdrží dotazy DNS přímo z klientů. Místo toho dotazy DNS pocházet ze služby DNS rekurzivní hello, že jsou klienti hello nakonfigurované toouse. Proto hello IP adresa použitá toodetermine hello 'nejbližší' koncový bod není IP adresa klienta hello, ale je služba DNS rekurzivní hello hello IP adresu. Tato IP adresa v praxi, je dobré proxy pro klienta hello.


Traffic Manager pravidelně, které aktualizace hello Internet latence tabulky tooaccount změny v hello globální Internetu a nové oblastech Azure. Ale výkonu aplikací se liší podle v reálném čase rozdíly v zatížení napříč hello Internetu. Směrování provozu výkonu nesleduje zatížení koncového bodu dané služby. Ale pokud koncový bod stane nedostupným, Traffic Manager nezahrnuje ho v odpovědi na dotazy DNS.


Toonote body:

* Pokud váš profil obsahuje několik koncových bodů v hello stejné oblasti Azure, pak Traffic Manager rovnoměrně rozděluje zatížení mezi hello dostupných koncových bodů v této oblasti. Pokud dáváte přednost distribuce různých přenosů v rámci oblasti, můžete použít [vnořené profily Traffic Manageru](traffic-manager-nested-profiles.md).
* Pokud všechny povolené koncové body v hello nejbližší oblast Azure jsou sníženou, Traffic Manager přesune toohello provoz koncovými body v hello další nejbližší oblast Azure. Pokud chcete toodefine pořadí upřednostňované převzetí služeb při selhání, použijte [vnořené profily Traffic Manageru](traffic-manager-nested-profiles.md).
* Pokud používáte metodu směrování provozu hello výkonu externí koncové body nebo vnořené koncových bodů, je potřeba toospecify hello umístění těchto koncových bodů. Zvolte hello oblast Azure nejbližší tooyour nasazení. Těchto lokalit jsou hodnoty hello nepodporuje hello Internet latence tabulky.
* Hello algoritmus, který vybere hello koncový bod je deterministická. Opakovat dotazy DNS z hello stejného klienta jsou směrované toohello stejný koncový bod. Obvykle se klienti používat servery DNS jiné rekurzivní cestách. Hello klienta může být směrované tooa jinému koncovému bodu. Směrování může být ovlivněn aktualizací toohello Internet latence tabulky. Proto hello výkonu metodu směrování provozu není zaručeno, že klient je vždy směrovány toohello stejný koncový bod.
* Při změně hello Internet latence tabulky, můžete si všimnout, že někteří klienti jsou řízené tooa jinému koncovému bodu. Tato změna směrování je přesnější podle aktuálního data latence. Tyto aktualizace jsou nezbytné toomaintain hello přesnost výkonu-směrování provozu jako hello průběžně zpracovaní Internetu.

## <a name = "geographic"></a>Zeměpisná metodu směrování provozu

Profily Traffic Manageru může být nakonfigurované toouse hello Geographic metodu směrování, aby uživatelé jsou směrované toospecific koncových bodů (Azure, externí nebo vnořené) podle které zeměpisné umístění svého dotazu DNS pochází z. To umožňuje Traffic Manager zákazníci tooenable scénáře, kde je důležité zároveň budete vědět, geografické oblasti uživatele a směrování je založeno na tomto. Mezi příklady patří soulad s pověřeními suverenity dat, lokalizace obsah & uživatelské prostředí a měření provoz z různých oblastech.
Pokud profil je nakonfigurována pro zeměpisnou směrování, každý koncový bod přidružené, profil musí toohave sadu tooit zeměpisné oblasti přiřazen. Může být geografické oblasti s následující úrovní členitosti 
- World – libovolné oblasti
- Místní seskupení – například Afrika, Střední východ, Austrálie/Tichomoří atd. 
- Země nebo oblast – například Irsku Peru, Hongkong – zvláštní správní oblast ČLR atd. 
- Kraj – například kalifornské USA, Austrálie – Queensland, Kanada Alberta atd. (Poznámka: Tato úroveň členitost je podporována pouze pro stavy nebo provincie Austrálie, Kanada, UK a USA).

Jakmile oblasti nebo sadu oblastí je přiřazena tooan koncový bod, získá všechny žádosti z těchto oblastí směrované jenom toothat koncový bod. Traffic Manager používá hello zdrojové IP adresy hello DNS dotazu toodetermine hello oblasti, ze kterého uživatel se dotazuje z – to je obvykle hello IP adresu místního Překladač DNS hello provádění dotazu hello jménem uživatele hello.  

![Azure Traffic Manager, geografické' směrování provozu – metoda](./media/traffic-manager-routing-methods/geographic.png)

Traffic Manager načte hello zdrojovou IP adresu hello dotazu DNS a rozhodne, které geografické oblasti je pocházející z. Pak vypadá toosee, pokud je koncový bod, který má tato tooit geografické oblasti namapované. Spustí toto vyhledávání na úrovni členitosti nejnižší hello (Kraj kde by je podporován, jinak v úrovně země nebo oblast hello) a klient se přepne všechny způsob hello nejvyšší úroveň toohello, což je **World**. Pomocí této funkce traversal je určený jako koncový bod tooreturn hello v odpovědi na dotaz hello nalezena Hello první shodu. Pokud je vrácen odpovídající pomocí typu koncový bod vnořené, koncový bod v rámci této podřízené profilu, založené na jeho metody směrování. Hello následující body jsou příslušné toothis chování:

- Geografické oblasti může být namapované pouze endpoint tooone v profilu Traffic Manageru, když je typ směrování hello geografické směrování. To zajišťuje, že směrování uživatelů je deterministická a zákazníků můžete povolit scénáře, které vyžadují jednoznačným geografické hranice.
- Pokud uživatele oblast dodává do dvou různých koncových bodů zeměpisné mapování, Traffic Manager vybere hello koncový bod s nejnižší rozlišením hello a nebere v úvahu směrování žádostí z této oblasti toohello jiný koncový bod. Představte si třeba profil typ geografické směrování s dva koncové body - koncovém bodě 1 a Endpoint2. Je nakonfigurovaný koncovém bodě 1 tooreceive provoz z Irsku a Endpoint2 je nakonfigurovaný tooreceive provoz z Evropa. Pokud požadavek pochází z Irská, je vždy směrované tooEndpoint1.
- Vzhledem k tomu, že v oblasti lze mapovat pouze tooone koncový bod, vrátí ji Traffic Manager bez ohledu na to, jestli je koncový bod hello v pořádku nebo ne.

    >[!IMPORTANT]
    >Důrazně doporučujeme, aby zákazníci, kteří používají metody směrování geografické hello, přidružte ji k koncové hello vnořené typu body, které má podřízené profily obsahující alespoň dva koncové body v každém.
- Pokud je nalezena shoda koncový bod a tohoto koncového bodu je v hello **Zastaveno** stavu Traffic Manager vrátí odpověď NODATA. Žádné další hledání v takovém případě se provádí vyšší nahoru v hierarchii hello geografické oblasti. Toto chování platí i pro typy vnořených koncových bodů při hello podřízené profilu je v hello **Zastaveno** nebo **zakázané** stavu.
- Pokud se zobrazí koncový bod **zakázané** stav, nebude zahrnutý v oblasti hello odpovídající procesu. Toto chování platí i pro typy vnořených koncových bodů při hello koncový bod je v hello **zakázané** stavu.
- Pokud dotaz pochází z zeměpisnou oblast, která nemá žádné mapování v tomto profilu, Traffic Manager vrátí odpověď NODATA. Proto důrazně doporučujeme, aby zákazníci používat zeměpisné směrování s jeden koncový bod, v ideálním případě tohoto typu vnořené s alespoň dva koncové body v rámci profilu podřízené hello, s hello oblast **World** přiřazené tooit. To také zajistí, zpracovává všechny IP adresy, které nejsou namapovány tooa oblast.

Jak je popsáno v [jak Traffic Manager funguje](traffic-manager-how-traffic-manager-works.md), Traffic Manager neobdrží dotazy DNS přímo z klientů. Místo toho dotazy DNS pocházet ze služby DNS rekurzivní hello, že jsou klienti hello nakonfigurované toouse. Proto hello IP adresa použitá toodetermine hello oblast není hello IP adresa klienta, ale je služba DNS rekurzivní hello hello IP adresu. Tato IP adresa v praxi, je dobré proxy pro klienta hello.


## <a name="next-steps"></a>Další kroky

Zjistěte, jak pomocí aplikací s vysokou dostupností toodevelop [monitorování koncového bodu Traffic Manager](traffic-manager-monitoring.md)

Zjistěte, jak příliš[vytvořit profil správce provozu](traffic-manager-create-profile.md)

<!--Image references-->
[1]: ./media/traffic-manager-routing-methods/priority.png
[2]: ./media/traffic-manager-routing-methods/weighted.png
[3]: ./media/traffic-manager-routing-methods/performance.png



