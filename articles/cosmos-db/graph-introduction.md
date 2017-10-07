---
title: "Úvod tooAzure Cosmos DB Graph API | Microsoft Docs"
description: "Zjistěte, jak můžete použít Azure Cosmos DB toostore, dotazů a křížovou masivní grafy s nízkou latencí pomocí hello hello Gremlin grafu dotazovací jazyk Apache TinkerPop."
services: cosmos-db
author: dennyglee
documentationcenter: 
ms.assetid: b916644c-4f28-4964-95fe-681faa6d6e08
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/21/2017
ms.author: denlee
ms.openlocfilehash: a4e79a4aa27976966baf0554928026177991ff69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-cosmos-db-graph-api"></a>Úvod tooAzure Cosmos DB: rozhraní Graph API

[Azure Cosmos DB](introduction.md) je hello globálně distribuované a více modelech databáze služby společnosti Microsoft pro kritické aplikace. Poskytuje Azure Cosmos DB [globální distribuční klíč](distribute-data-globally.md), [elastické škálování propustnost a úložiště](partition-data.md) po celém světě, jednociferné milisekundu latence v hello 99th percentilu, [pět dobře definované úrovně konzistence](consistency-levels.md)a zaručit vysoká dostupnost, které jsou zajišťované pomocí [špičkový SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Azure Cosmos DB [automaticky indexuje data](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) aniž byste si toodeal se správou schéma a index. Zahrnuje více modelů a podporuje modely dokumentů, klíčových hodnot, grafů a sloupcových dat.

![Gremlin grafu a Azure Cosmos DB](./media/graph-introduction/graph-gremlin.png)

Hello Azure Cosmos DB Graph API poskytuje:

- Graf modelování
- Procházení rozhraní API
- Globální distribuční klíč
- Elastické škálování úložiště a propustnost s méně než 10 ms, přečtěte si latenci a méně než 15 ms v 99th percentilu hello
- Automatické indexování s dostupností rychlých dotazu
- Přizpůsobitelné úrovně konzistence
- Komplexní SLA 99,99 % dostupnost včetně

tooquery Azure Cosmos DB, můžete použít hello [Apache TinkerPop](http://tinkerpop.apache.org) graf traversal jazyk [Gremlin](http://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps), nebo jiné systémy TinkerPop kompatibilní grafu jako [Apache Spark, GraphX](spark-connector-graph.md).

Tento článek obsahuje přehled hello Azure Cosmos DB Graph API a vysvětluje, jak můžete použít ho toostore masivní grafy s až miliardy vrcholy a okrajů. Se můžete dotazovat hello grafy s latencí milisekundu a snadno momentální hello grafu strukturu a schéma.

## <a name="graph-database"></a>Graf databáze
Data jak se objevuje v reálném světě hello je samozřejmě připojený. Modelování tradiční dat se zaměřuje na entity. Pro mnoho aplikací, je také nutné toomodel nebo toomodel entity a vztahy přirozeně.

A [grafu](http://mathworld.wolfram.com/Graph.html) je struktura, která se skládá z [vrcholy](http://mathworld.wolfram.com/GraphVertex.html) a [okraje](http://mathworld.wolfram.com/GraphEdge.html). Vrcholy a okrajů může mít libovolný počet vlastností. Vrcholy označují diskrétní objekty, jako je osoba, místo nebo událost. Okraje označují vztahy mezi vrcholy. Například uživatel může vědět jinou osobou, být zahrnut v události a nedávno v umístění. Vlastnosti express informace o hello vrcholy a okrajů. Příklad vlastnosti zahrnují vrchol, který má název, stáří a okraj, který má časovým razítkem a váhu. Více oficiálně tento model se označuje jako [vlastnost grafu](http://tinkerpop.apache.org/docs/current/reference/#intro). Azure Cosmos DB podporuje model grafu vlastnost hello.

Například následující hello ukázkové graf znázorňuje vztahy mezi osoby, mobilní zařízení, zájmů a operační systémy.

![Zobrazuje osob, zařízení a zájmů ukázkové databáze](./media/graph-introduction/sample-graph.png)

Grafy jsou užitečné toounderstand široké škály datových sad v vědecké účely, technologie a business. Graf databáze umožňují modelu a ukládání grafů přirozeně a efektivně, což je užitečné díky pro mnoho scénářů. Graf databáze jsou obvykle databáze NoSQL, protože tyto použití případů často také nutné flexibilitu schémat a rychlé iterací.

Grafy nabízejí nové a výkonné data modelování techniku. Ale tuto skutečnost sám o sobě není dostatečná toouse důvod databázi grafu. Pro mnoho případy použití a vzorů, které zahrnují traversals grafu grafy překonat tradiční databáze SQL a NoSQL podle pořadí podle velikosti. Tento rozdíl ve výkonu je rozšířena další při procházení více než jedna relace, jako je friend z friend.

Hello rychlé traversals, které poskytují grafu databází můžete kombinovat s algoritmy grafu, nejprve v úrovni vyhledávání, hledání spektra první a na Dijkstra algoritmus, toosolve problémy v různých doménách sociálních sítí, správy obsahu, geoprostorové, a doporučení.

## <a name="planet-scale-graphs-with-azure-cosmos-db"></a>Grafy planetu škálování s Azure Cosmos DB
Azure Cosmos DB je plně spravovaná grafu databáze, která nabízí globální distribuční elastické škálování úložiště a propustnost, automatické indexování a dotaz, přizpůsobitelné úrovně konzistence a podporu pro hello TinkerPop standard.  

![Azure Cosmos DB – architektura grafu](./media/graph-introduction/cosmosdb-graph-architecture.png)

Azure Cosmos DB nabízí následující hello rozlišené možnosti srovnání tooother grafu databáze hello trhu:

* Elasticky škálovatelná propustnost a úložiště

 Grafy v reálném světě hello potřebovat tooscale nad rámec hello kapacity jednoho serveru. S Azure Cosmos databáze je možné škálovat vaše grafy bezproblémově více serverů. Také je možné škálovat hello propustnost vaší grafu nezávisle podle vaší vzorů přístupu. Azure Cosmos DB podporuje grafu databáze, které je možné škálovat toovirtually neomezené velikosti úložiště a zřízené propustnosti.

* Replikace více oblast

 Azure Cosmos DB transparentně replikuje vaší oblasti tooall dat grafu, které jste spojené s vaším účtem. Replikace umožňuje toodevelop aplikace, které vyžadují toodata globální přístup. V oblasti hello konzistence, dostupnosti, výkonu a odpovídající záruky jsou kompromisy. Azure Cosmos DB poskytuje transparentní regionální převzetí služeb při selhání s více funkci rozhraní API. Propustnost a úložiště můžete Elasticky škálovat napříč hello zeměkouli.

* Rychlé dotazy a traversals pomocí známé syntaxe pro Gremlin

 Ukládat heterogenní vrcholy a okrajů a dotazování pomocí známé syntaxe Gremlin těchto dokumentů. Azure Cosmos DB využívá vysoce souběžnou uvolnění zámku, strukturovaná protokolu indexování technologie tooautomatically indexem veškerý obsah. Tato funkce umožňuje bohaté dotazy v reálném čase a traversals bez hello potřebovat toospecify parametry schématu, sekundární indexy nebo zobrazení. Další informace v [grafy dotazu pomocí Gremlin](gremlin-support.md).

* Plně spravovaná

 Azure Cosmos DB eliminuje prostředky hello nutné toomanage databáze a počítačů. Jako plně spravovaná služba Microsoft Azure můžete provést není nutné toomanage virtuální počítače, nasazení a konfigurace softwaru, spravovat škálování nebo řešit komplexní datové vrstvě upgrady. Každý grafu je automaticky zálohovat a chránit proti místní selhání. Můžete snadno přidat účet Azure Cosmos DB a zřídit kapacitu podle potřeby tak, aby se můžete soustředit na svou aplikaci, ne provoz a správu databáze.

* Automatické indexování

 Ve výchozím nastavení Azure Cosmos DB automaticky indexuje všechny vlastnosti hello v rámci uzlů a hrany v grafu hello a nebudou očekávat a nevyžaduje žádné schéma nebo vytváření sekundárních indexů.

* Kompatibilita s Apache TinkerPop

 Azure Cosmos DB nativně podporuje hello open source Apache TinkerPop standard a umožňuje integraci s jinými systémy povoleno TinkerPop grafu. Ano, můžete snadno migrovat z jiné databáze grafů, jako je Titan nebo Neo4j, nebo pomocí graf analýzy architektury, jako je Azure Cosmos DB [Apache Spark GraphX](spark-connector-graph.md).

* Přizpůsobitelné úrovně konzistence

 Vyberte z pěti konzistence dobře definované úrovně tooachieve optimálního poměru mezi konzistencí a výkonem. Pro dotazy a operace čtení nabízí služba Azure Cosmos DB pět různých úrovní konzistence: silná, omezená neaktuálnost, relace, konzistentní předpona a konečný výsledek. Tyto úrovně konzistence podrobné, dobře definované povolit toomake zvukové kompromisy mezi konzistence, dostupností a latencí. Další informace v [pomocí konzistence úrovně toomaximize dostupnosti a výkonu v DocumentDB](consistency-levels.md).

Azure Cosmos DB také můžete použít více modelů, jako je dokument a graf, uvnitř hello stejné kontejnery nebo databáze. Můžete použít dat grafu dokumentu kolekce toostore node souběžně s dokumenty. Můžete použít obě dotazy SQL přes JSON a Gremlin dotazy tooquery hello stejná data jako graf.

## <a name="getting-started"></a>Začínáme
Můžete použít hello rozhraní příkazového řádku Azure (CLI) Azure Powershell nebo hello portál Azure s podporou pro účty Azure Cosmos DB toocreate rozhraní API grafu. Po vytvoření účtů hello portál Azure poskytuje koncového bodu služby, jako je třeba `https://<youraccount>.graphs.azure.com`, která poskytuje front-end protokolu WebSocket pro Gremlin. Můžete nakonfigurovat vaše TinkerPop kompatibilní nástroje, jako například hello [Gremin konzoly](http://tinkerpop.apache.org/docs/current/reference/#gremlin-console), tooconnect toothis koncový bod a sestavení aplikace v jazyce Java, Node.js nebo všechny Gremlin ovladače klienta.

Hello následující tabulka uvádí oblíbených Gremlin ovladače, které můžete použít pro Azure Cosmos DB:

| Ke stažení | Dokumentace |
| --- | --- |
| [Java](https://mvnrepository.com/artifact/com.tinkerpop.gremlin/gremlin-java) |[Gremlin JavaDoc](http://tinkerpop.apache.org/javadocs/current/full/) |
| [Node.js](https://www.npmjs.com/package/gremlin) |[Gremlin JavaScript na Githubu](https://github.com/jbmusso/gremlin-javascript) |
| [Gremlin konzoly](https://tinkerpop.apache.org/downloads.html) |[TinkerPop dokumentace](http://tinkerpop.apache.org/docs/current/reference/#gremlin-console) |

Azure Cosmos DB také poskytuje knihovny .NET, který má Gremlin rozšiřující metody nad hello [SDK služby Azure Cosmos DB](documentdb-sdk-dotnet.md) prostřednictvím balíčku NuGet. Tato knihovna nabízí Gremlin serveru "v rámci procesu", které můžete použít tooconnect přímo tooDocumentDB data oddíly.

| Ke stažení | Dokumentace |
| --- | --- |
| [.NET](https://www.nuget.org/packages/Microsoft.Azure.Graphs/) |[Microsoft.Azure.Graphs](https://msdn.microsoft.com/library/azure/dn948556.aspx) |

Pomocí hello [emulátoru DB Cosmos Azure](local-emulator.md), můžete použít rozhraní Graph API toodevelop hello a otestovat místně bez vytváření předplatného Azure nebo nákladům. Až budete spokojeni s jak aplikace funguje v hello emulátoru, můžete přepnout toousing účet Azure Cosmos DB v cloudu hello.

## <a name="scenarios-for-graph-support-of-azure-cosmos-db"></a>Scénáře pro graf podpoře Azure Cosmos DB
Tady je několik scénářů, kde je možné grafu podpoře Azure Cosmos DB:

* Sociálních sítí

 Kombinací data o vašich zákazníků a jejich interakce s jinými lidmi, můžete vyvíjet přizpůsobené prostředí, předpovědi chování zákazníků nebo spojení uživatelů s ostatními s podobnými zájmy. Azure Cosmos DB můžou být použité toomanage sociálních sítí a sledovat předvolby zákazníka a data.

* Doporučení moduly

 Tento scénář se často používá v odvětví prodejní hello. Kombinací informace o produktech, uživatelů a interakce s uživatelem, jako je nákup, procházení nebo hodnocení položku, můžete vytvořit vlastní doporučení. Hello s nízkou latencí, elastické škálování a nativní podpora grafu Azure Cosmos databáze je ideální pro modelování tyto interakce.

* Geoprostorové

 Mnoho aplikací v telecommunications, logistiky a plánování cesty potřebovat toofind umístění v rámci oblast zájmu nebo vyhledejte hello nejkratší nebo optimální směrování mezi dvěma umístěními. Azure Cosmos DB je přirozené přizpůsobit pro tyto problémy.

* Internet věcí

 Hello sítě a připojení mezi modelován jako graf zařízení IoT můžete sestavit lépe porozumět hello stav zařízení a prostředky a zjistěte, jak změny v jedné části hello sítě může potenciálně ovlivnit další části.

## <a name="next-steps"></a>Další kroky
toolearn Další informace o podpoře grafu v Azure Cosmos DB, najdete v části:

* Začínáme s hello [Azure Cosmos DB grafu kurzu](create-graph-dotnet.md).
* Další informace o tom příliš[dotaz grafy v Azure DB Cosmos pomocí Gremlin](gremlin-support.md).
