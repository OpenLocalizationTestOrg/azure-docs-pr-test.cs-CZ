---
title: "data aaaDistribute globálně pomocí Azure Cosmos DB | Microsoft Docs"
description: "Další informace o škálování planetu geografická replikace, převzetí služeb při selhání a data obnovení pomocí globální databáze z databáze Cosmos Azure, služby globálně distribuované, podstoupí model databáze."
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: ba5ad0cc-aa1f-4f40-aee9-3364af070725
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/14/2017
ms.author: arramac
ms.openlocfilehash: b50e8433dc7e70c54d68c4c2f99954a13f4951f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodistribute-data-globally-with-azure-cosmos-db"></a>Jak toodistribute dat globálně pomocí Azure Cosmos DB
Azure je všudypřítomný – má globální nároků přes 30 + zeměpisné oblasti a průběžně zvětšuje. S jeho po celém světě přítomnosti jedním z hello rozlišené možnosti, které Azure nabízí vývojářům tooits je hello možnost toobuild, nasazení a snadno spravovat globálně distribuované aplikace. 

[Azure Cosmos DB](../cosmos-db/introduction.md) je globálně distribuovaná databázová služba Microsoftu s více modely pro klíčové aplikace. Azure Cosmos DB poskytuje připraveného globální distribuční [elastické škálování propustnost a úložiště](../cosmos-db/partition-data.md) po celém světě, jednociferné milisekundu latence v hello 99th percentilu, [pět dobře definované úrovně konzistence ](consistency-levels.md)a zaručit vysoká dostupnost, všechny zálohovány pomocí [špičkový SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Azure Cosmos DB [automaticky indexuje data](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) aniž byste si toodeal se správou schéma a index. Zahrnuje více modelů a podporuje modely dokumentů, klíčových hodnot, grafů a sloupcových dat. Jako narodila cloudové služby je víceklientská a globální distribuci z hello pozadí pečlivě zkonstruován Azure Cosmos DB.

**Jedinou kolekci Azure Cosmos DB rozdělena na oddíly a distribuovaných nad několika oblastmi Azure**

![Azure Cosmos DB kolekce rozdělena na oddíly a distribuován do tří oblastí](./media/distribute-data-globally/global-apps.png)

Jak jsme se naučili při sestavování Azure Cosmos DB, přidání globální distribuční nelze chodím – nemůže být "přišroubovány on" na "jedné lokality" databázový systém. Možnosti Hello nabízí globálně distribuované databázi span nad rámec, tradiční zeměpisné katastrofě obnovení (Geo-DR) nabízí "jedné lokalitě" databází. Databáze jedné lokalitě nabídky Geo-zotavení po Havárii schopnosti jsou striktní podmnožinu globálně distribuované databáze. 

S připraveného globální distribuční databázi Cosmos Azure, vývojáři nemají toobuild vlastní generování uživatelského rozhraní replikace díky využití architektury buď Lambda vzor hello (například [AWS DynamoDB replikace](https://github.com/awslabs/dynamodb-cross-region-library/blob/master/README.md)) přes protokol databáze hello nebo nástrojem to "dvojité zápisy" nad několika oblastmi. Není doporučujeme tyto přístupy vzhledem k tomu, že je možné tooensure správnost takové přístupy a poskytují zvukové SLA. 

V tomto článku poskytujeme přehled možností globální distribuční databázi Cosmos Azure. Můžeme také popisují Azure Cosmos DB jedinečný přístup tooproviding komplexní SLA. 

## <a id="EnableGlobalDistribution"></a>Povolení připraveného globální distribuční
Azure Cosmos DB poskytuje hello následující možnosti tooenable tooeasily můžete zapsat planetu škálování aplikací. Tyto možnosti jsou dostupné prostřednictvím založenou na poskytovateli prostředků hello Azure Cosmos DB na [rozhraní REST API](https://docs.microsoft.com/rest/api/documentdbresourceprovider/) a také hello portálu Azure.

### <a id="RegionalPresence"></a>Všudypřítomná regionální přítomnosti 
Azure je neustále rostoucí jeho zeměpisné přítomnosti tak, že převedou [nové oblasti](https://azure.microsoft.com/regions/) online. Azure Cosmos DB je k dispozici ve všech oblastech nové Azure ve výchozím nastavení. To vám umožní tooassociate geografické oblasti s vaším účtem Azure Cosmos DB databáze také Azure otevře novou oblast hello pro firmy.

**Azure Cosmos DB je k dispozici ve všech oblastech Azure ve výchozím nastavení**

![Azure DB Cosmos k dispozici na všechny oblasti Azure](./media/distribute-data-globally/azure-regions.png)

### <a id="UnlimitedRegionsPerAccount"></a>Přidružení neomezený počet oblasti s vaším účtem Azure Cosmos DB databáze
Azure Cosmos DB vám umožní tooassociate libovolný počet oblastí Azure s vaší Azure DB Cosmos databáze účtu. Mimo omezení geografického vymezení (například Čína, Německo) neexistují žádná omezení počtu hello oblastí, které může být spojeno s vaším účtem databáze Azure Cosmos DB. Hello následující obrázek znázorňuje toospan účet nakonfigurovaný databáze nad 25 oblastmi Azure.  

**Klienta Azure Cosmos DB databáze účet STA 25 oblastí Azure**

![Účet databáze Azure Cosmos DB pokrývání uzlů 25 oblastí Azure](./media/distribute-data-globally/spanning-regions.png)

### <a id="PolicyBasedGeoFencing"></a>Na základě zásad geografického vymezení
Azure Cosmos DB je navrženou toohave možnosti geografického vymezení na základě zásad. Geografického vymezení je důležité součásti tooensure vedení a dodržování předpisů omezení dat a může zabránit přidružení v určité oblasti s vaším účtem. Příklady geografického vymezení zahrnují (ale nejsou omezeni), oborů globální distribuční toohello oblasti v rámci svrchovaných cloudu (například Čína a Německo), nebo hranici zdanění government (například Austrálie). zásady Hello jsou řízena pomocí metadat hello předplatného Azure.

### <a id="DynamicallyAddRegions"></a>Dynamicky přidávat a odebírat oblastí
Azure Cosmos DB vám umožní tooadd (přidružení) nebo odebrání (zrušit přidružení) oblasti tooyour databázového účtu v libovolném bodě v čase (viz [předchozí obrázek](#UnlimitedRegionsPerAccount)). Na základě replikaci dat mezi oddílů souběžně, Azure Cosmos DB zajišťuje, že při přechodu do režimu online novou oblast, Azure Cosmos DB k dispozici do 30 minut kdekoli v hello, world pro až too100 TBs. 

### <a id="FailoverPriorities"></a>Priorit převzetí služeb při selhání
přesné pořadí toocontrol regionální převzetí služeb při selhání, po výpadku více místní databázi Cosmos Azure vám umožní tooassociate hello priority toovarious oblasti spojené s účtem databáze hello (viz následující obrázek hello). Azure Cosmos DB zajistí, že pořadí hello automatické převzetí služeb při selhání dojde v pořadí podle priority hello, které zadáte. Další informace o místní převzetí služeb při selhání najdete v tématu [automatické regionální převzetí služeb při selhání pro kontinuitu podnikových procesů v Azure Cosmos DB](regional-failover.md).

**Klient Azure Cosmos databáze můžete nakonfigurovat pořadí priorit převzetí služeb při selhání hello (pravé podokno) pro oblasti, které jsou spojené s účtem databáze**

![Konfigurace priorit převzetí služeb při selhání v Azure Cosmos DB](./media/distribute-data-globally/failover-priorities.png)

### <a id="OfflineRegions"></a>Dynamicky převádět oblast "do režimu offline"
Azure Cosmos DB umožňuje tootake databáze účtu offline v určité oblasti a převeďte ho zpátky do online režimu později. Oblasti označené offline neúčastnit aktivně replikace a nejsou součástí hello pořadí převzetí služeb při selhání. To vám umožní toofreeze hello poslední známá image dobrý databáze v jednom z hello číst oblasti před zavedením potenciálně riziková upgraduje tooyour aplikace.

### <a id="ConsistencyLevels"></a>Více, dobře definovaný konzistence modely pro globální replikované databáze
Zpřístupní Azure Cosmos DB [více dobře definované úrovně konzistence](consistency-levels.md) zajišťoval SLA. Můžete vybrat konkrétní konzistence model (z hello seznam dostupných možností) v závislosti na hello zatížení/scénáře. 

### <a id="TunableConsistency"></a>Přizpůsobitelné konzistence pro globální replikované databáze
Azure Cosmos DB vám umožní přepsat tooprogrammatically a uvolnit hello výchozí konzistence výběru na základě žádosti za běhu. 

### <a id="DynamicallyConfigurableReadWriteRegions"></a>Dynamicky konfigurovat pro čtení a zápisu oblastí
Azure Cosmos DB umožňuje oblasti hello tooconfigure (přidružené k databázi hello) pro "číst", "zápisu" nebo "pro čtení a zápis" oblasti. 

### <a id="ElasticallyScaleThroughput"></a>Elasticky škálování propustnost mezi oblastmi Azure
Je možné Elasticky škálovat kolekci Azure Cosmos DB podle zřizování propustnost prostřednictvím kódu programu. Hello propustnost je použité tooall hello oblasti, kolekce hello je distribuován v.

### <a id="GeoLocalReadsAndWrites"></a>Geograficky místní čte a zapisuje
Hlavní výhoda Hello globálně distribuované databáze je toooffer s nízkou latencí přístup toohello data kdekoli v hello, world. Azure Cosmos DB nabízí nízkou latencí záruky na P99 pro různé operace databáze. Zajišťuje, že všechny operace čtení jsou směrované toohello nejbližší místní čtení oblast. slouží k tooserve požadavek čtení hello kvora místní toohello oblast, ve kterém se objeví hello číst; Hello totéž platí i toohello zápisy. Až po většinu repliky spolehlivě potvrdil hello zápisu místně, ale bez se ověřované vrácení na vzdálené repliky tooacknowledge hello zápisy, potvrdí se zápis. Jinak PUT, protokol hello replikace pro Azure Cosmos DB funguje v rámci hello předpoklad, že hello číst a zapisovat kvor jsou vždy místní toohello pro čtení a zápisu oblasti, v uvedeném pořadí, ve které hello požadavku.

### <a id="ManualFailover"></a>Ruční zahájení regionální převzetí služeb při selhání
Azure Cosmos DB vám umožní tootrigger hello převzetí služeb při selhání hello databáze účet toovalidate hello *ukončení tooend* dostupnosti vlastnosti celá aplikace hello (kromě hello databáze). Vzhledem k tomu, že oba hello zabezpečení a jsou zaručit liveness vlastnosti hello selhání zjišťování a vedoucí volba, Azure Cosmos DB zaručuje *nulové ztráty dat* operace klienta iniciované ruční převzetí služeb při selhání.

### <a id="AutomaticFailover"></a>Automatické převzetí služeb při selhání
Azure Cosmos DB podporuje automatické převzetí služeb při selhání v případě jeden nebo více regionální výpadků. Při selhání regionální Azure Cosmos DB udržuje latenci pro čtení, dostupnosti provozu, konzistence a propustnost SLA. Azure Cosmos DB obsahuje horní mez hello trvání toocomplete operaci automatické převzetí služeb při selhání. Toto je okno hello potenciální ztráty dat během výpadku regionální hello.

### <a id="GranularFailover"></a>Určená pro členitostí v různých převzetí služeb při selhání
Aktuálně hello automatickou a ruční převzetí služeb při selhání funkce jsou viditelné v hello členitost hello databázového účtu. Všimněte si, interně Azure DB Cosmos je navrženou toooffer *automatické* převzetí služeb při selhání na podrobnější databáze, kolekce nebo dokonce oddílu (kolekce vlastnící rozsah klíčů). 

### <a id="MultiHomingAPIs"></a>Více funkci rozhraní API v Azure Cosmos DB
Azure Cosmos DB vám umožní toointeract s databází hello buď pomocí logických (bez ohledu na oblast) nebo fyzické koncových bodů (specifické pro oblast). Použití logické koncové body zajišťuje, že aplikace hello můžete transparentně byly vícedomé v případě převzetí služeb při selhání. Dobrý den pozdější, fyzické koncových bodů, poskytují jemně odstupňovanou kontrolu toohello aplikace tooredirect čte a zapisuje toospecific oblasti.

Můžete najít informace o tom, jak načíst tooconfigure předvolby pro hello [DocumentDB API](../cosmos-db/tutorial-global-distribution-documentdb.md), [rozhraní Graph API](../cosmos-db/tutorial-global-distribution-graph.md), [tabulky API](../cosmos-db/tutorial-global-distribution-table.md), a [MongoDB API](../cosmos-db/tutorial-global-distribution-mongodb.md)v jejich odpovídajících propojené články.

### <a id="TransparentSchemaMigration"></a>Migrace databáze transparentní a konzistentní schéma a index 
Azure Cosmos DB je plně [bez ohledu na schéma](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). Hello jedinečný návrhu databázového stroje umožňuje tooautomatically a synchronně indexu všechny hello data, která ho ingestuje bez nutnosti žádné schéma nebo sekundární indexy, které od vás. Díky tomu můžete tooiterate globálně distribuované aplikace rychle bez starostí o migraci databáze schéma a index nebo koordinace aplikace s více fáze zavedení změn schématu uživatelům. Azure Cosmos DB zaručuje, že všechny zásady tooindexing změny explicitně které jste udělali nepovedou do snížení výkonu výkon nebo dostupnost.  

### <a id="ComprehensiveSLAs"></a>Komplexní SLA (kromě stejně vysokou dostupnost)
Jako služba globálně distribuovanou databázi, databázi Cosmos Azure nabízí dobře definovaný SLA pro **ztráty dat**, **dostupnosti**, **latence v P99**, **propustnost ** a **konzistence** hello databáze jako celek, bez ohledu na počet hello oblasti přidružené k databázi hello.  

## <a id="LatencyGuarantees"></a>Latence záruky
Hlavní výhoda Hello globálně distribuované databáze služby jako databázi Cosmos Azure je toooffer s nízkou latencí přístup tooyour data kdekoli v hello, world. Azure Cosmos DB nabízí zaručenou nízkou latencí v P99 pro různé operace databáze. Hello replikace protokol, který využívá Azure Cosmos DB zajistí, že hello databázové operace (v ideálním případě jak čte a zapisuje) jsou vždy prováděla hello oblast místní toothat hello klienta. Hello latence, které zahrnuje smlouvy SLA systému Azure Cosmos DB P99 pro operace čtení a zápisu (synchronně) indexované a dotazuje na různé velikosti požadavku a odpovědi. záruky Hello latence pro zápis zahrnují potvrzení trvanlivý většinu kvora v místním datacentru hello.

### <a id="LatencyAndConsistency"></a>Čekací doba na relaci s konzistence 
Pro globální Distribuovaná služba toooffer silné konzistence v globálně distribuované instalaci, je nutné toosynchronously replikace hello zápisy nebo synchronní provádět mezi oblastmi čtení – hello rychlosti světlým a hello vyžadují spolehlivost sítě WAN aby silnou konzistenci za následek vysoké latenci a nízkou dostupnost databázových operací. V pořadí toooffer zaručit nízkou latenci v P99 a 99.99 dostupnosti, proto musí hello služby použít asynchronní replikaci. Tato naopak vyžaduje hello služby musí také nabízí [dobře definovaný, volný konzistence choice(s)](consistency-levels.md) – slabší než silné (toooffer nízkou latenci a dostupnosti záruky) a v ideálním případě silnější než ("případné" konzistence toooffer intuitivní programovací model).

Azure Cosmos DB zajišťuje, že operace čtení nejsou požadované toocontact repliky napříč více oblastí toodeliver hello konkrétní konzistence úroveň záruky. Podobně zajišťuje, že operace zápisu nejsou zablokování při hello data se replikují přes všechny oblasti hello (tj. zápisy se asynchronně replikují přes oblasti). Pro účty databáze více oblasti jsou k dispozici více úrovních volný konzistence. 

### <a id="LatencyAndAvailability"></a>Čekací doba na relaci s dostupností 
Latence a dostupnost se sociálními hello hello stejné mince. V souvislosti se latence operace hello v stabilního stavu a dostupnosti hello stěně selhání. Z hlediska aplikace hello je pomalé běžící operace databáze nelze rozlišit z databáze, která není k dispozici. 

toodistinguish vysokou latencí z nedostupnosti Azure Cosmos DB obsahuje absolutní horní mez latence různé operace databáze. Pokud se operace hello databáze trvá déle než horní hranice toocomplete hello, Azure Cosmos DB vrátí vypršení časového limitu. Hello smlouva SLA o dostupnosti Azure Cosmos DB zajistí, že vypršení časových limitů hello počítají proti smlouva SLA o dostupnosti hello. 

### <a id="LatencyAndThroughput"></a>Čekací doba na relaci s propustností
Azure Cosmos DB neprovede můžete zvolit latence a propustnosti. Ctí hello SLA pro obě latence v P99 a zajišťovat hello propustnosti, kterou máte zřízen. 

## <a id="ConsistencyGuarantees"></a>Záruky konzistence
Při hello [silnou konzistenci modelu](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) je standard zlatý hello z programovatelnosti, přechodu na hello zvládnutí cena vysokou latencí (v stabilního stavu) a ztrátu dostupnosti (v hello vzhled chyb). 

Azure Cosmos DB nabízí dobře definovaný tooreason tooyou pro programovací model o konzistence replikovaná data. V pořadí tooenable jste toobuild vícedomé aplikace, modely konzistence hello vystavené Azure Cosmos DB nejsou vázané na navrženou toobe oblasti a není závislá na hello oblast, ze které se zpracovávají hello čtení a zápisu. 

Azure Cosmos DB konzistence SLA zaručuje, že 100 % požadavků na čtení bude vyhovovat záruku konzistence hello úroveň konzistence hello požadoval buď (hello výchozí úroveň konzistence na hello databázového účtu nebo hodnota hello přepsat na žádost hello ). Požadavek čtení považuje konzistence hello toohave splněny smlouvy SLA, pokud jsou splněny všechny hello konzistence záruky související s úrovní konzistence hello. Hello následující tabulka zaznamená hello konzistence záruky, které odpovídají úrovně konzistence toospecific, které nabízí Azure Cosmos DB.

**Konzistence záruky související s úrovní konzistence pro danou v Azure Cosmos DB**

<table>
    <tr>
        <td><strong>Úroveň konzistence</strong></td>
        <td><strong>Vlastnosti konzistence</strong></td>
        <td><strong>SLA</strong></td>
    </tr>
    <tr>
        <td rowspan="3">Relace</td>
        <td>Přečtěte si vlastní zápisu</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Monotónní pro čtení</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Konzistentní předpony</td>
        <td>100%</td>
    </tr>
    <tr>
        <td rowspan="3">Typu s ohraničenou prošlostí</td>
        <td>Monotónní čtení (v rámci oblasti)</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Konzistentní předpony</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Vázaný typu prošlostí &lt; tisíc, T</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Konzistentní předpony</td>
        <td>Konzistentní předpony</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Silné</td>
        <td>Linearizable</td>
        <td>100%</td>
    </tr>
</table>

### <a id="ConsistencyAndAvailability"></a>Relace je konzistence s dostupností
Hello [nemožností výsledek](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf) z hello [věta CAP](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf) prokáže, že je skutečně znemožňuje, aby tooremain systému hello k dispozici a nabídka linearizable konzistence hello stěně selhání. Služba Hello databáze, musíte zvolit toobe prohlášení CP nebo Asie a Tichomoří – CP systémy forgo dostupnosti považuje linearizable konzistence při hello Asie systémy forgo [linearizable konzistence](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) považuje dostupnosti. Azure Cosmos DB nikdy porušuje hello požadovanou úroveň konzistence, takže oficiálně je CP systému. Ale v praxi není konzistence všech nebo nic nabídky – jsou více modely dobře definovaný konzistence podél hello spektrum konzistence mezi linearizable a případnou konzistence. V Azure Cosmos DB, mají o tooidentify řadu hello zmírnit modely konzistence s skutečných použitelnosti a intuitivní programovací model. Azure Cosmos DB přejde hello konzistence dostupnosti kompromisy prostřednictvím nabídky 99.99 dostupnost SLA spolu s [více zmírnit ještě dobře definované úrovně konzistence](consistency-levels.md). 

### <a id="ConsistencyAndAvailability"></a>Relace je konzistence s latencí
Komplexnější varianta CAP byl navržený Prof. ADAM Abadi a se nazývá [PACELC](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf), který také účty pro latenci a konzistence kompromisy v stabilního stavu. Je uvedeno, že v stabilního stavu, musíte zvolit hello databázový systém mezi konzistencí a latenci. S více modely volný konzistence (zálohován asynchronní replikaci a místní pro čtení, zápisu kvor) Azure Cosmos DB zajišťuje, že všechny čtení a zápisu jsou místní toohello pro čtení a zápisu oblasti v uvedeném pořadí.  Díky tomu, že s nízkou latencí pro Azure Cosmos DB toooffer zaručuje v rámci oblasti hello úrovně konzistence hello.  

### <a id="ConsistencyAndThroughput"></a>Relace je konzistence s propustností
Vzhledem k tomu, že hello implementaci modelu konkrétní konzistence, závisí na volbu hello [kvora typ](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf), propustnost hello také se liší podle hello volbu konzistence. Například v Azure Cosmos DB, hello propustnost s důrazně konzistentní čtení je přibližně poloviční toothat nakonec byl konzistentní čtení. 
 
**Vztah čtení kapacity pro konkrétní konzistence úrovně v Azure Cosmos DB**

![Vztah mezi konzistencí a propustnosti](./media/distribute-data-globally/consistency-and-throughput.png)

## <a id="ThroughputGuarantees"></a>Propustnost záruky 
Azure Cosmos DB vám umožní tooscale propustnost (stejně jako, úložiště), Elasticky v různých oblastech v závislosti na vyžádání hello. 

**Jedinou kolekci Azure Cosmos DB rozděleného (mezi tři horizontálních oddílů) a poté distribuován do tří oblastí Azure**

![Azure Cosmos DB distribuované a kolekce rozdělena na oddíly](../cosmos-db/media/introduction/azure-cosmos-db-global-distribution.png)

Kolekci Azure Cosmos DB získá distribuované pomocí dvěma rozměry – v rámci oblasti a pak v oblastech. Zde je uveden postup: 

* V jedné oblasti kolekci Azure Cosmos DB škálovat na více systémů z hlediska prostředků oddíly. Každý oddíl prostředků spravuje sady klíčů a je důrazně konzistentní a vysokou dostupností na základě stavu počítače replikace mezi sadu replik. Azure Cosmos DB je prostředek řídí systém plně, kde oddílu prostředků zodpovídá za poskytování svůj díl propustnost pro hello rozpočtu tooit prostředky přidělené systému. Hello škálování kolekci Azure Cosmos DB je zcela transparentní – Azure Cosmos DB spravuje hello prostředků oddíly a rozdělí a sloučí se podle potřeby. 
* Všechny oddíly prostředků hello je poté distribuován nad několika oblastmi. Oddíly prostředků vlastnící hello stejnou sadu klíčů mezi různé oblasti formuláře oddíl sady (viz [předchozí obrázek](#ThroughputGuarantees)).  Oddíly prostředků v rámci sady oddílu jsou koordinované použití stavu počítače replikace napříč hello více oblastí. V závislosti na úrovni konzistence hello nakonfigurované se konfiguruje oddíly hello prostředků v rámci sady oddílu dynamicky pomocí topologiemi (například hvězdičky, sériově, stromu atd.). 

Azure Cosmos DB základě vysoce přizpůsobivém oddílu správu, Vyrovnávání zatížení a zásady správného řízení striktní prostředků, umožňuje tooelastically škálování propustnosti nad několika oblastmi Azure v kolekci Azure Cosmos DB. Změna propustnosti na kolekci je operace runtime v Azure Cosmos DB - jako s jiných databázových operací, které Azure Cosmos DB zaručuje hello absolutní horní mez latence pro vaši žádost o toochange hello propustnost. Jako příklad hello následující obrázek znázorňuje zákazníka kolekce pružně zajištěnou propustností (v rozsahu od 1 milion - 10M počet požadavků za sekundu rámci dvou oblastí) na základě poptávky hello.
 
**Kolekce zákazníka pružně zajištěnou propustností (1 milion - 10M požadavků za sekundu)**

![Azure Cosmos DB pružně zřízené propustnosti](./media/distribute-data-globally/elastic-throughput.png)

### <a id="ThroughputAndConsistency"></a>Propustnost je vztah s konzistence 
Stejné jako [konzistence na vztah s propustností](#ConsistencyAndThroughput).

### <a id="ThroughputAndAvailability"></a>Propustnost je vztah s dostupností
Azure Cosmos DB pokračuje toomaintain při provedení změn hello toohello propustnost jeho dostupnost. Azure Cosmos DB transparentně spravuje oddíly (například rozdělení, sloučení, operace klonování) a zajistí, že hello operations není snížit výkon nebo dostupnost, zatímco aplikace hello pružně zvyšuje nebo snižuje propustnost. 

## <a id="AvailabilityGuarantees"></a>Záruky dostupnosti
Azure Cosmos DB nabízí 99,99 % dostupnost smlouva SLA pro každou hello dat a řízení operací roviny. Jak je popsáno výše, zahrnují záruky dostupnosti Azure Cosmos DB absolutní horní mez na latenci pro každé operace roviny dat a řízení. záruky dostupnosti Hello jsou steadfast a s počtem hello oblastí nebo zeměpisné vzdálenost mezi oblastmi nemění. Záruky dostupnosti použít s ruční jak, automatické převzetí služeb při selhání. Azure Cosmos DB nabízí transparentní více funkci rozhraní API, která zajistěte, aby vaše aplikace umožňuje práci s logické koncových bodů a může transparentně směrovat hello požadavky toohello novou oblast v případě převzetí služeb při selhání. Vložit jinak, vaše aplikace nemusí toobe znovu nasazena na místní převzetí služeb při selhání a udržované hello dostupnost SLA.

### <a id="AvailabilityAndConsistency"></a>Relace na dostupnosti s konzistence, latence a propustnosti
Relace na dostupnosti s konzistence, latence a propustnosti je popsaná v [konzistence na vztah s dostupností](#ConsistencyAndAvailability), [na latenci vztah s dostupností](#LatencyAndAvailability) a [Propustnosti na vztah s dostupností](#ThroughputAndAvailability). 

## <a id="GuaranteesAgainstDataLoss"></a>Záruky a chování systému pro "ztrátě dat."
V Azure DB Cosmos každý oddíl (kolekce) je vysoké dostupnosti počtem replik, které jsou rozloženy domén selhání alespoň 10-20. Všech zápisů jsou synchronně a spolehlivě potvrdí podle většinu kvora replik před jejich jsou potvrzené toohello klienta. Asynchronní replikaci se použije s spolupráce mezi oddílů rozloženy více oblastí. Azure Cosmos DB zaručuje, že nedochází ke ztrátě dat spustil klienta ruční převzetí služeb při selhání. Při automatické převzetí služeb při selhání Azure Cosmos DB zaručí, horní mez hello nakonfigurované ohraničenou typu prošlostí interval na hello okno ztráty dat v rámci smlouvy SLA pro jeho.

## <a id="CustomerFacingSLAMetrics"></a>Metriky SLA zákazníkem
Azure Cosmos DB transparentně zpřístupní hello metriky propustnosti, latenci, konzistence a dostupnost. Tyto metriky jsou přístupné prostřednictvím kódu programu nebo přes hello portálu Azure (viz následující obrázek). Můžete také nastavit výstrahy na různé prahové hodnoty pomocí služby Azure Application Insights.
 
**Metriky konzistence, latenci, propustnost a dostupnost se transparentně dostupné tooeach klienta**

![Azure Cosmos DB zákazníka viditelné metrikách SLA](./media/distribute-data-globally/customer-slas.png)

## <a id="Next Steps"></a>Další kroky
* globální replikace tooimplement na pomocí účtu Azure Cosmos DB hello Azure portálu, najdete v tématu [jak tooperform Azure Cosmos DB globální databáze replikace pomocí portálu Azure hello](tutorial-global-distribution-documentdb.md).
* toolearn o tooimplement více hlavních architektury s Azure Cosmos DB, najdete v části [architektury více hlavní databázi s Azure Cosmos DB](multi-region-writers.md).
* toolearn Další informace o způsobu práce automatickou a ruční převzetí služeb při selhání v Azure Cosmos DB, najdete v [regionální převzetí služeb při selhání v Azure Cosmos DB](regional-failover.md).

## <a id="References"></a>Odkazy
1. Erica Brewer. [Směrem robustní distribuovaných systémů](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf)
2. Erica Brewer. [Zakončení později – 12 letech jak změnily hello pravidla](http://informatik.unibas.ch/fileadmin/Lectures/HS2012/CS341/workshops/reportsAndSlides/PresentationKevinUrban.pdf)
3. Gilbert, Lynch. - [Brewer & č. 39; s domněnek a vhodnosti konzistentní, k dispozici, Oddíl odolný vůči chybám webových služeb](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf)
4. ADAM Abadi. [Konzistence kompromisy v moderních distribuovaných systémů návrhu databáze](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf)
5. Martin Kleppmann. [Zastavte volání databáze prohlášení CP nebo Asie a Tichomoří](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html)
6. Petr Bailis a další. [Pravděpodobnosti typu s ohraničenou Prošlostí (PBS) pro praktické částečné kvor](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
7. Naor a vlny. [Zatížení, kapacity a dostupnosti v systémech kvora](http://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf)
8. Herlihy a výskytů. [Lineralizability: Správnost podmínku pro souběžné objekty](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf)
9. [Azure Cosmos DB SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/)
