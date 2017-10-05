---
title: "Úvod do Azure Cosmos DB Graph API | Microsoft Docs"
description: "Zjistěte, jak lze pomocí Azure Cosmos DB ukládání, dotazování a procházení masivní grafy s nízkou latencí pomocí jazyk dotazu grafu Gremlin Apache TinkerPop."
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
ms.openlocfilehash: fb02fdf7f0f0a244fbf370e5f76fac2da2ef9f9e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="introduction-to-azure-cosmos-db-graph-api"></a>Úvod do Azure Cosmos DB: Graph API

[Azure Cosmos DB](introduction.md) je služba databáze globálně distribuované a více modelech od společnosti Microsoft pro kritické aplikace. Poskytuje Azure Cosmos DB [globální distribuční klíč](distribute-data-globally.md), [elastické škálování propustnost a úložiště](partition-data.md) po celém světě, jednociferné milisekundu latence v 99th percentilu [pět dobře definované úrovně konzistence](consistency-levels.md)a zaručit vysoká dostupnost, které jsou zajišťované pomocí [špičkový SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Azure Cosmos DB [automaticky indexuje data](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), aniž by vyžadovala zapojení správy schémat a indexů. Zahrnuje více modelů a podporuje modely dokumentů, klíčových hodnot, grafů a sloupcových dat.

![Gremlin grafu a Azure Cosmos DB](./media/graph-introduction/graph-gremlin.png)

Poskytuje rozhraní API služby Azure DB Cosmos grafu:

- Graf modelování
- Procházení rozhraní API
- Globální distribuční klíč
- Elastické škálování úložiště a propustnost s méně než 10 ms, přečtěte si latenci a méně než 15 ms v 99th percentilu
- Automatické indexování s dostupností rychlých dotazu
- Přizpůsobitelné úrovně konzistence
- Komplexní SLA 99,99 % dostupnost včetně

K dotazování databáze Cosmos Azure, můžete použít [Apache TinkerPop](http://tinkerpop.apache.org) graf traversal jazyk [Gremlin](http://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps), nebo jiné systémy TinkerPop kompatibilní grafu jako [Apache Spark GraphX](spark-connector-graph.md).

Tento článek obsahuje přehled Azure Cosmos DB Graph API a vysvětluje, jak můžete použít k ukládání velkých grafy s až miliardy vrcholy a okrajů. Se můžete dotazovat grafy s latencí milisekundu a snadno momentální grafu strukturu a schéma.

## <a name="graph-database"></a>Graf databáze
Data jak se objevuje v reálném světě se přirozeně připojen. Modelování tradiční dat se zaměřuje na entity. Pro mnoho aplikací je také zapotřebí model nebo model entity a vztahy přirozeně.

A [grafu](http://mathworld.wolfram.com/Graph.html) je struktura, která se skládá z [vrcholy](http://mathworld.wolfram.com/GraphVertex.html) a [okraje](http://mathworld.wolfram.com/GraphEdge.html). Vrcholy a okrajů může mít libovolný počet vlastností. Vrcholy označují diskrétní objekty, jako je osoba, místo nebo událost. Okraje označují vztahy mezi vrcholy. Například uživatel může vědět jinou osobou, být zahrnut v události a nedávno v umístění. Vlastnosti express informace o vrcholy a okrajů. Příklad vlastnosti zahrnují vrchol, který má název, stáří a okraj, který má časovým razítkem a váhu. Více oficiálně tento model se označuje jako [vlastnost grafu](http://tinkerpop.apache.org/docs/current/reference/#intro). Azure Cosmos DB podporuje model vlastnost grafu.

Například následující ukázkový graf znázorňuje vztahy mezi osoby, mobilní zařízení, zájmů a operační systémy.

![Zobrazuje osob, zařízení a zájmů ukázkové databáze](./media/graph-introduction/sample-graph.png)

Grafy jsou užitečné zjistit široké škály datových sad v vědecké účely, technologie a business. Graf databáze umožňují modelu a ukládání grafů přirozeně a efektivně, což je užitečné díky pro mnoho scénářů. Graf databáze jsou obvykle databáze NoSQL, protože tyto použití případů často také nutné flexibilitu schémat a rychlé iterací.

Grafy nabízejí nové a výkonné data modelování techniku. Ale tuto skutečnost sám o sobě není dostatečná důvod k použití databáze grafu. Pro mnoho případy použití a vzorů, které zahrnují traversals grafu grafy překonat tradiční databáze SQL a NoSQL podle pořadí podle velikosti. Tento rozdíl ve výkonu je rozšířena další při procházení více než jedna relace, jako je friend z friend.

Rychlé traversals, které poskytují grafu databází můžete kombinovat s algoritmy grafu jako hloubka první vyhledávání, hledání spektra první a Dijkstra je algoritmus, k řešení problémů v různých doménách jako sociálních sítí, obsahu, správu, geoprostorové a doporučení.

## <a name="planet-scale-graphs-with-azure-cosmos-db"></a>Grafy planetu škálování s Azure Cosmos DB
Azure Cosmos DB je plně spravovaná grafu databáze, která nabízí globální distribuční elastické škálování úložiště a propustnost, automatické indexování a dotaz, přizpůsobitelné úrovně konzistence a podpora pro standardní TinkerPop.  

![Azure Cosmos DB – architektura grafu](./media/graph-introduction/cosmosdb-graph-architecture.png)

Azure Cosmos DB nabízí následující možnosti odlišné ve srovnání s jiných databází grafu na trhu:

* Elasticky škálovatelná propustnost a úložiště

 Grafy ve skutečnosti muset škálování nad rámec kapacitu jeden server. S Azure Cosmos databáze je možné škálovat vaše grafy bezproblémově více serverů. Také je možné škálovat propustnost vaší grafu nezávisle podle vaší vzorů přístupu. Azure Cosmos DB podporuje grafu databáze, které je možné škálovat na prakticky neomezené velikosti úložiště a zřízené propustnosti.

* Replikace více oblast

 Azure Cosmos DB transparentně replikuje data grafu do všech oblastech, které jste spojené s vaším účtem. Replikace umožňuje vyvíjet aplikace, které vyžadují globální přístup k datům. V oblasti konzistence, dostupnost a výkon a odpovídající záruky jsou kompromisy. Azure Cosmos DB poskytuje transparentní regionální převzetí služeb při selhání s více funkci rozhraní API. Po celém světě můžete Elasticky škálovat propustnost a úložiště.

* Rychlé dotazy a traversals pomocí známé syntaxe pro Gremlin

 Ukládat heterogenní vrcholy a okrajů a dotazování pomocí známé syntaxe Gremlin těchto dokumentů. Azure Cosmos DB využívá vysoce souběžnou uvolnění zámku, strukturovaná protokolu technologii indexování automaticky indexuje všechny obsah. Tato funkce umožňuje bohaté dotazy v reálném čase a traversals bez potřeba specifikovat parametry schématu, sekundární indexy nebo zobrazení. Další informace v [grafy dotazu pomocí Gremlin](gremlin-support.md).

* Plně spravovaná

 Azure Cosmos DB eliminuje potřeba spravovat prostředky databáze a počítačů. Plně spravovaná služba Microsoft Azure a není nutné spravovat virtuální počítače, nasadit a konfigurovat software, spravovat škálování nebo řešit komplexní datové vrstvě upgrady. Každý grafu je automaticky zálohovat a chránit proti místní selhání. Můžete snadno přidat účet Azure Cosmos DB a zřídit kapacitu podle potřeby tak, aby se můžete soustředit na svou aplikaci, ne provoz a správu databáze.

* Automatické indexování

 Ve výchozím nastavení Azure Cosmos DB automaticky indexuje všechny vlastnosti v rámci uzlů a okraje v grafu a nebudou očekávat a nevyžaduje žádné schéma nebo vytváření sekundárních indexů.

* Kompatibilita s Apache TinkerPop

 Azure Cosmos DB nativně podporuje standardní open source Apache TinkerPop a umožňuje integraci s jinými systémy povoleno TinkerPop grafu. Ano, můžete snadno migrovat z jiné databáze grafů, jako je Titan nebo Neo4j, nebo pomocí graf analýzy architektury, jako je Azure Cosmos DB [Apache Spark GraphX](spark-connector-graph.md).

* Přizpůsobitelné úrovně konzistence

 Vyberte z pěti dobře definované úrovně konzistence pro dosažení optimálního poměru mezi konzistencí a výkonem. Pro dotazy a operace čtení nabízí služba Azure Cosmos DB pět různých úrovní konzistence: silná, omezená neaktuálnost, relace, konzistentní předpona a konečný výsledek. Tyto úrovně konzistence podrobné, dobře definované umožňují zvukové kompromisy mezi konzistence, dostupností a latencí. Další informace najdete v tématu o [využití úrovní konzistence pro maximalizaci dostupnosti a výkonu v DocumentDB](consistency-levels.md).

Azure Cosmos DB také můžete použít více modelů, jako je dokument a graf, v rámci stejné kontejnery nebo databáze. Kolekce dokumentů můžete použít k ukládání dat grafu node souběžně s dokumenty. Dotazy SQL za JSON a Gremlin dotazy můžete použít k dotazování stejná data jako graf.

## <a name="getting-started"></a>Začínáme
Můžete rozhraní příkazového řádku Azure (CLI), Azure Powershell nebo portálu Azure s podporou pro rozhraní graph API k vytváření účtů Azure Cosmos DB. Po vytvoření účtů, portál Azure poskytuje koncového bodu služby, jako je třeba `https://<youraccount>.graphs.azure.com`, která poskytuje front-end protokolu WebSocket pro Gremlin. Můžete nakonfigurovat vaše TinkerPop kompatibilní nástroje, jako je třeba [Gremin konzoly](http://tinkerpop.apache.org/docs/current/reference/#gremlin-console), k připojení k tomuto koncovému bodu a vytvářet aplikace v jazyce Java, Node.js nebo všechny Gremlin ovladače klienta.

V následující tabulce jsou uvedeny oblíbených Gremlin ovladače, které můžete použít pro Azure Cosmos DB:

| Ke stažení | Dokumentace |
| --- | --- |
| [Java](https://mvnrepository.com/artifact/com.tinkerpop.gremlin/gremlin-java) |[Gremlin JavaDoc](http://tinkerpop.apache.org/javadocs/current/full/) |
| [Node.js](https://www.npmjs.com/package/gremlin) |[Gremlin JavaScript na Githubu](https://github.com/jbmusso/gremlin-javascript) |
| [Gremlin konzoly](https://tinkerpop.apache.org/downloads.html) |[TinkerPop dokumentace](http://tinkerpop.apache.org/docs/current/reference/#gremlin-console) |

Také poskytuje knihovny .NET, který má Gremlin rozšiřující metody na Azure Cosmos DB [SDK služby Azure Cosmos DB](documentdb-sdk-dotnet.md) prostřednictvím balíčku NuGet. Tato knihovna nabízí server Gremlin služby "v rámci procesu", který můžete použít pro připojení přímo k DocumentDB data oddíly.

| Ke stažení | Dokumentace |
| --- | --- |
| [.NET](https://www.nuget.org/packages/Microsoft.Azure.Graphs/) |[Microsoft.Azure.Graphs](https://msdn.microsoft.com/library/azure/dn948556.aspx) |

Pomocí [emulátoru DB Cosmos Azure](local-emulator.md), můžete použít rozhraní Graph API pro vývoj a testování místně bez vytváření předplatného Azure nebo nákladům. Až budete spokojeni s jak funguje aplikaci v emulátoru, můžete přejít k používání účtu Azure Cosmos DB v cloudu.

## <a name="scenarios-for-graph-support-of-azure-cosmos-db"></a>Scénáře pro graf podpoře Azure Cosmos DB
Tady je několik scénářů, kde je možné grafu podpoře Azure Cosmos DB:

* Sociálních sítí

 Kombinací data o vašich zákazníků a jejich interakce s jinými lidmi, můžete vyvíjet přizpůsobené prostředí, předpovědi chování zákazníků nebo spojení uživatelů s ostatními s podobnými zájmy. Azure Cosmos DB slouží ke správě sociálních sítí a sledovat předvolby zákazníka a data.

* Doporučení moduly

 Tento scénář se často používá v prodejní odvětví. Kombinací informace o produktech, uživatelů a interakce s uživatelem, jako je nákup, procházení nebo hodnocení položku, můžete vytvořit vlastní doporučení. Nízká latence, elastické škálování a nativní podpora grafu Azure Cosmos databáze je ideální pro modelování tyto interakce.

* Geoprostorové

 Mnoho aplikací v telecommunications, logistiky a plánování cesty muset vyhledejte umístění, které vás zajímají v rámci oblasti nebo vyhledat nejkratší nebo optimální směrování mezi dvěma umístěními. Azure Cosmos DB je přirozené přizpůsobit pro tyto problémy.

* Internet věcí

 Sítě a připojení mezi modelován jako graf zařízení IoT můžete sestavit lépe porozumět stav zařízení a prostředky a zjistěte, jak změny v jedné části sítě může potenciálně ovlivnit další části.

## <a name="next-steps"></a>Další kroky
Další informace o podpoře grafu v Azure Cosmos DB najdete v tématu:

* Začínáme s [Azure Cosmos DB grafu kurzu](create-graph-dotnet.md).
* Další informace o tom, jak [dotaz grafy v Azure DB Cosmos pomocí Gremlin](gremlin-support.md).
