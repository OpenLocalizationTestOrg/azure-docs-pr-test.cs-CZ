---
title: "úrovně aaaConsistency v Azure Cosmos DB | Microsoft Docs"
description: "Azure Cosmos DB má pět konzistence úrovně toohelp vyrovnávání případné konzistence, dostupností a latencí kompromis."
keywords: "konzistence typu případné azure cosmos databáze, azure, Microsoft azure"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: cgronlun
documentationcenter: 
ms.assetid: 3fe51cfa-a889-4a4a-b320-16bf871fe74c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: mimig
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ac399c229d0856cd811bc81568536e519af3300f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tunable-data-consistency-levels-in-azure-cosmos-db"></a>Data přizpůsobitelné úrovně konzistence v Azure Cosmos DB
Azure Cosmos DB slouží z hello pozadí s globální distribuce v paměti pro každý datový model. Je navrženou toooffer předvídatelný s nízkou latencí záruky, smlouvy SLA 99,99 % dostupnost a více dobře definovaný zmírnit modely konzistence. V současné době Azure Cosmos DB poskytuje pět úrovně konzistence: silnou, s ohraničenou odolností, založenou relace, konzistentní Předpona a případnou. 

Kromě **silné** a **konzistence typu případné** modely běžně nabízí distribuovaných databází Azure Cosmos DB nabízí tři další modely pečlivě kódované a operationalized konzistence a ověřila jejich užitečnost proti skutečných případy použití. Jedná se o hello **ohraničenou typu prošlostí**, **relace**, a **konzistentní předponu** úrovně konzistence. Kolektivně tyto úrovně konzistence pět povolit toomake dobře odůvodněnou kompromis mezi konzistencí, dostupností a latencí. 

## <a name="distributed-databases-and-consistency"></a>Distribuovaná databáze a konzistence
Komerčně distribuované databáze spadají do dvou kategorií: databáze, které vůbec nenabízejí řádně definované osvědčené volby konzistence, a databáze, které nabízejí dvě extrémně programovatelné volby (silná vs. nahodilá konzistence). 

Hello bývalé zatížení aplikace vývojářům minutia jejich protokoly replikace a očekává je toomake obtížné kompromisy mezi konzistencí, dostupností, latence a propustnosti. Hello pozdější převádí toochoose přetížení, mezi dvěma hranicemi hello. Navzdory hello celou řadu výzkum a návrhy pro více než 50 konzistence modely hello komunity distribuovanou databázi nebylo možné toocommercialize úrovně konzistence nad rámec silné a případnou konzistence. Cosmos DB umožňuje vývojářům toochoose mezi pěti modely dobře definovaný konzistence podél hello konzistence spektrum – nejsilnější, ohraničenou odolností, založenou [relace](http://dl.acm.org/citation.cfm?id=383631), konzistentní předponu a případnou. 

![Azure Cosmos DB nabízí více, dobře definované (volný) konzistence toochoose modely z](./media/consistency-levels/five-consistency-levels.png)

Hello následující tabulka znázorňuje hello konkrétní záruky, které poskytuje každou úroveň konzistence.
 
**Úrovně konzistence a záruky**

| Úrovně konzistence | Záruky |
| --- | --- |
| Silné | Linearizovatelnost |
| Omezená neaktuálnost | Konzistentní předpona Prodleva čtení mezi zápisy podle předpon k nebo intervalu t |
| Relace   | Konzistentní předpona Monotónní čtení, monotónní zápisy, čtení zápisů, zápisy po čtení |
| Konzistentní předpona | Aktualizace vrátil jsou některé předpony všechny aktualizace hello, s žádné mezery |
| Nahodilé  | Čtení mimo pořadí |

Můžete nakonfigurovat úroveň konzistence výchozí hello na vašem účtu Cosmos DB (a později přepsat hello konzistence na konkrétního požadavku pro čtení). Interně hello výchozí konzistence úroveň platí toodata v rámci sady hello oddílů, které může span oblasti. O 73 % naše tenanty použijte konzistence typu relace a 20 % raději typu s ohraničenou prošlostí. Jsme pozorovat, že přibližně 3 % naše zákazníky experimentovat s různé úrovně konzistence původně před spuštěním na konkrétní konzistence volba pro svou aplikaci. Také pozorovat, že pouze 2 % naše tenanty přepsat úrovně konzistence na základě žádosti. 

V databázi Cosmos čtení zpracovat v relaci konzistentní předponu a konzistence typu případné jsou dvakrát jako levných jako čtení s konzistence typu silné nebo ohraničenou prošlostí. Cosmos DB má špičkové komplexní SLA 99,99 %, včetně záruk konzistence společně s dostupnosti, propustnosti a latence. Můžeme využívat [linearizability kontrolu](http://dl.acm.org/citation.cfm?id=1806634), který nepřetržitě funguje přes naše služby telemetrie a zveřejněno sestavy žádné tooyou narušení konzistence. Pro ohraničenou odolností, založenou jsme monitorování a sestavy, kterou trvalo veškerá porušení zásad a t meze. Pro všechny pět úrovní volný konzistence jsme také sestavy hello [metrika pravděpodobnosti typu s ohraničenou prošlostí](http://dl.acm.org/citation.cfm?id=2212359) tooyou přímo.  

## <a name="scope-of-consistency"></a>Rozsah konzistence
Hello členitost konzistence je žádost o oboru tooa jednoho uživatele. Žádost o zápis může odpovídat tooan insert, replace, upsert a odstraňovat transakce. Stejně jako u zápisu a transakce čtení nebo dotazu je také požadavek vymezená tooa jednoho uživatele. Hello uživatel může být požadované toopaginate přes velkým sadu výsledků dotazu, pokrývání uzlů více oddílů, ale každý přečíst transakce je jednostránkové vymezená tooa a zpracování z v rámci jednoho oddílu.

## <a name="consistency-levels"></a>Úrovně konzistentnosti
Výchozí úroveň konzistence můžete nakonfigurovat na vašem účtu databáze, která se použije tooall kolekcí (a databází) v rámci účtu Cosmos DB. Ve výchozím nastavení všechny čtení a dotazy vydaný pro hello uživatelem definované prostředků, použijte hello výchozí konzistence úroveň zadaný na hello databázového účtu. Můžete uvolnit úroveň konzistence hello specifického pro čtení nebo dotazu požadavku, použití v každé z hello podporuje rozhraní API. Existují pět typy podporované hello Azure Cosmos DB replikace protokolem úrovně konzistence, které poskytují zrušte kompromis mezi záruky konkrétní konzistence a výkon, jak je popsáno v této části.

**Silná**: 

* Nabízí silnou konzistenci [linearizability](https://aphyr.com/posts/313-strong-consistency-models) záruku s hello čte zaručenou tooreturn hello nejnovější verzi položky. 
* Silnou konzistenci zaručuje, že zápis se zobrazí po jeho je trvale potvrzený hello většinu kvora replik. Zápis je buď synchronně trvale potvrzený hello primární i hello kvora sekundárních databází nebo byl přerušen. Čtení vždy potvrdí se většinou hello číst kvora, klient nikdy uvidí zápisu nepotvrzené nebo jeho část a je vždy zaručit tooread hello nejnovější potvrzené zápisu. 
* Azure Cosmos DB účty, které jsou nakonfigurované toouse silnou konzistenci nelze přiřadit více než jedné oblasti Azure pomocí svého účtu Azure Cosmos DB. 
* Hello náklady na operace čtení (z hlediska [požadované jednotky](request-units.md) spotřebované) se silnou konzistenci je vyšší než relaci a případnou, ale hello stejné jako typu s ohraničenou prošlostí.

**Vázaný typu prošlostí**: 

* Konzistence typu s ohraničenou prošlostí zaručuje, že hello čtení může funkce lag za zápisy podle maximálně *tisíc* verze nebo předpony položky nebo *t* časovém intervalu. 
* Proto pokud výběr ohraničenou typu prošlostí, hello "typu prošlostí" je možné nakonfigurovat dvěma způsoby: číslo verze *tisíc* hello položky, pomocí kterého hello čtení funkce lag za hello zápisů a hello časový interval *t* 
* Vázaný typu prošlostí nabízí celkový globální pořadí s výjimkou v rámci hello "typu prošlostí okno." Hello monotónní číst záruky existuje v rámci oblasti i mimo hello "typu prošlostí okno." 
* Typu s ohraničenou prošlostí poskytuje silnější záruku konzistence než relace nebo konzistence typu případné. Pro globálně distribuované aplikace doporučujeme že použít typu s ohraničenou prošlostí pro scénáře, kde by jako toohave silnou konzistenci, ale také chcete 99,99 % dostupnost a s nízkou latencí. 
* Azure Cosmos DB účty, které jsou nakonfigurovány s konzistence typu s ohraničenou prošlostí můžete přidružit libovolný počet oblastí Azure pomocí svého účtu Azure Cosmos DB. 
* Hello náklady na operace čtení (z hlediska RUs spotřebované) s typu s ohraničenou prošlostí je vyšší než relace a konzistence typu případné, ale hello stejné jako silnou konzistenci.

**Relace**: 

* Na rozdíl od modely globální konzistence hello nabízené úrovně konzistence typu silné a ohraničenou prošlostí je konzistence typu relace vymezená tooa relaci klienta. 
* Konzistence typu relace je ideální pro všechny scénáře, kde je zahrnuta relaci zařízení nebo uživatele, protože zaručuje monotónní čtení, monotónní zápisů a čtení zaručuje vlastní zápisy (RYW). 
* Konzistence typu relace poskytuje předvídatelnou konzistenci pro relaci a maximální propustnost čtení současně nabízí zápisy s nejnižší latencí hello a čtení. 
* Azure Cosmos DB účty, které jsou nakonfigurovány s konzistence typu relace můžete přidružit libovolný počet oblastí Azure pomocí svého účtu Azure Cosmos DB. 
* Hello náklady na operace čtení (z hlediska RUs spotřebované) s úroveň konzistence je menší než typu silné a ohraničenou prošlostí, ale více než případné konzistence relace

<a id="consistent-prefix"></a>
**Konzistentní předponu**: 

* Konzistentní předponu zaručuje, že při absenci dalších zápisů hello repliky v rámci skupiny hello konvergovat. 
* Konzistentní předponu zaručuje, že čtení nikdy neuvidí mimo pořadí zápisy. Pokud zápisy se prováděly v pořadí hello `A, B, C`, pak klient uvidí buď `A`, `A,B`, nebo `A,B,C`, ale nikdy mimo pořadí jako `A,C` nebo `B,A,C`.
* Azure Cosmos DB účty, které jsou nakonfigurovány s konzistentní předponu konzistence můžete přidružit libovolný počet oblastí Azure pomocí svého účtu Azure Cosmos DB. 

**Závěrečné**: 

* Konzistence typu případné zaručuje, že při absenci dalších zápisů hello repliky v rámci skupiny hello konvergovat. 
* Konzistence typu případné je hello nejslabších formu konzistence, kde může klient získat hello hodnoty, které jsou starší než hello ty, které jsou sice zaznamenala před.
* Konzistence typu případné poskytuje konzistence čtení hello nejslabších ale nabízí hello nejnižší latenci pro čtení i zápisy.
* Azure Cosmos DB účty, které jsou nakonfigurovány s konzistence typu případné můžete přidružit libovolný počet oblastí Azure pomocí svého účtu Azure Cosmos DB. 
* Hello náklady na operace čtení (z hlediska RUs spotřebované) s konzistence typu případné hello úroveň je hello nejnižší všech úrovní konzistence hello Azure Cosmos DB.

## <a name="configuring-hello-default-consistency-level"></a>Nastavování úrovně konzistence výchozí hello
1. V hello [portál Azure](https://portal.azure.com/), v hello vlevo, klikněte na **Azure Cosmos DB**.
2. V hello **Azure Cosmos DB** okně, vyberte hello databáze účet toomodify.
3. V okně účtu hello, klikněte na tlačítko **výchozí konzistence**.
4. V hello **výchozí konzistence** , vyberte hello novou úroveň konzistence a klikněte na **Uložit**.
   
    ![Snímek obrazovky zvýraznění hello ikonu nastavení a vstupního výchozí konzistence](./media/consistency-levels/database-consistency-level-1.png)

## <a name="consistency-levels-for-queries"></a>Úrovně konzistence pro dotazy
Ve výchozím nastavení pro uživatelem definované prostředky úroveň konzistence hello pro dotazy je hello stejné jako hello úroveň konzistence pro čtení. Ve výchozím nastavení hello aktualizace indexu synchronně na každý insert, replace nebo odstranit kontejner položky toohello Cosmos DB. Díky této může hello dotazy toohonor hello stejnou úroveň konzistence jako bod čtení. Při Azure Cosmos DB je optimalizovaná pro zápis a podporuje dlouhodobě svazky zápisy, synchronní indexu údržby a poskytování konzistentní dotazů, můžete nakonfigurovat určité kolekce tooupdate svého indexu líné. Opožděné indexování další zvyšuje výkon hello zápisu a je ideální pro hromadné přijímání scénáře při zatížení je primárně náročné na čtení.  

| Indexování režimu | Čtení | Dotazy |
| --- | --- | --- |
| Konzistentní (výchozí) |Vyberte z typu silné a ohraničenou prošlostí, relace, konzistentní předpony nebo případné |Vyberte, ze silného typu s ohraničenou prošlostí, relaci nebo případné |
| Opožděné |Vyberte z typu silné a ohraničenou prošlostí, relace, konzistentní předpony nebo případné |Nahodilé |
| Žádný |Vyberte z typu silné a ohraničenou prošlostí, relace, konzistentní předpony nebo případné |Neuvedeno |

Jako s požadavky na čtení, můžete snížit úroveň konzistence hello požadavku specifického dotazu v každé rozhraní API.

## <a name="next-steps"></a>Další kroky
Pokud chcete toodo více čtení o úrovně konzistence a kompromisy, doporučujeme hello následující prostředky:

* Doug Terry. Replikovaná Data konzistence vysvětlené prostřednictvím baseballové (video).   
  [https://www.YouTube.com/Watch?v=gluIh8zd26I](https://www.youtube.com/watch?v=gluIh8zd26I)
* Doug Terry. Replikovaná Data konzistence vysvětlené prostřednictvím baseballové.   
  [http://Research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.PDF](http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf)
* Doug Terry. Relace záruky pro slabě konzistentní replikovaná Data.   
  [http://DL.ACM.org/CITATION.cfm?ID=383631](http://dl.acm.org/citation.cfm?id=383631)
* ADAM Abadi. Konzistence kompromisy moderní distribuované návrhu databáze systémy: Zakončení je jenom část scénáře hello ".   
  [http://Computer.org/CSDL/mags/co/2012/02/mco2012020037-ABS.HTML](http://computer.org/csdl/mags/co/2012/02/mco2012020037-abs.html)
* Petr Bailis, Shivaram Venkataraman, Michael J. tw, Joseph M. Hellerstein, Stoica uchování. Pravděpodobnosti ohraničenou typu Prošlostí (PBS) pro praktické částečné kvor.   
  [http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.PDF](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
* Wernerovi Vogels. Závěrečné konzistentní – kdykoli znovu spustit.    
  [http://allthingsdistributed.com/2008/12/eventually_consistent.HTML](http://allthingsdistributed.com/2008/12/eventually_consistent.html)
* Moni Naor, Avishai vlny, hello zatížení, kapacity a dostupnosti z kvora systémy SIAM deníku na výpočetní, v.27 n.2, p.423-447, duben 1998.
  [http://epubs.siam.org/DOI/Abs/10.1137/S0097539795281232](http://epubs.siam.org/doi/abs/10.1137/S0097539795281232)
* Sebastianu Burckhardt, Jan Dern, Macanal Musuvathi, Royi Tan, řada produktů: nástroj úplný a automatické linearizability pro kontrolu, řízení hello 2010 ACM SIGPLAN konference na programovací jazyk návrhu a implementace, června 05-10, 2010, Toronto, Ontario, Kanada [doi > 10.1145/1806596.1806634] [http://dl.acm.org/citation.cfm?id=1806634](http://dl.acm.org/citation.cfm?id=1806634)
* Petr Bailis, Shivaram Venkataraman, Michael J. tw, Joseph M. Hellerstein, uchování Stoica Probabilistically ohraničenou typu prošlostí pro praktické částečné kvor řízení hello VLDB dotační, v.5 n.8, p.776-787, duben 2012 [http:// DL.ACM.org/CITATION.cfm?ID=2212359](http://dl.acm.org/citation.cfm?id=2212359)
