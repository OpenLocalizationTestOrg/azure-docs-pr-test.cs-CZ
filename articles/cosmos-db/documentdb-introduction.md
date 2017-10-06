---
title: "Azure Cosmos DB: Rozhraní DocumentDB API | Dokumentace Microsoftu"
description: "Zjistěte, jak můžete použít Azure Cosmos DB toostore a dotaz ohromné objemy dokumentů JSON s nízkou latencí pomocí SQL a JavaScript."
keywords: "json databáze, databáze dokumentu"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 686cdd2b-704a-4488-921e-8eefb70d5c63
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/22/2017
ms.author: mimig
ms.openlocfilehash: c96dfa5e2685782a99d2bcaf992f88dd2bef920b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-cosmos-db-documentdb-api"></a>Úvod tooAzure Cosmos DB: DocumentDB rozhraní API

[Azure Cosmos DB](introduction.md) je globálně distribuovaná databázová služba Microsoftu s více modely pro klíčové aplikace. Poskytuje Azure Cosmos DB [globální distribuční klíč](distribute-data-globally.md), [elastické škálování propustnost a úložiště](partition-data.md) po celém světě, jednociferné milisekundu latence v hello 99th percentilu, [pět dobře definované úrovně konzistence](consistency-levels.md)a zaručit vysoká dostupnost, všechny zálohovány pomocí [špičkový SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Azure Cosmos DB [automaticky indexuje data](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) aniž byste si toodeal se správou schéma a index. Zahrnuje více modelů a podporuje modely dokumentů, klíčových hodnot, grafů a sloupcových dat. 

![Rozhraní Azure DocumentDB API](./media/documentdb-introduction/cosmosdb-documentdb.png) 

S hello rozhraní API DocumentDB, databázi Cosmos Azure poskytuje bohaté a známý [možnosti dotazů SQL](documentdb-sql-query.md) s trvale nízké latence přes data JSON bez schématu. V tomto článku jsme poskytovat přehled o hello Azure Cosmos DB na rozhraní API DocumentDB, a jak můžete ho použít toostore ohromné objemy dat JSON, dotaz je v rámci pořadí latence v milisekundách a snadno momentální hello schématu. 

## <a name="what-capabilities-and-key-features-does-azure-cosmos-db-offer"></a>Jaké schopnosti a klíčové funkce nabízí databáze Azure Cosmos?
Azure DB Cosmos, prostřednictvím hello DocumentDB API nabízí hello následující klíčové funkce a výhody:

* **Elasticky škálovatelná propustnost a úložiště:** snadno škálovat nahoru i dolů vaší toomeet databáze JSON vaší aplikace potřebuje. Data se ukládají na discích SSD (solid-state drive), které nabízí nízkou a předvídatelnou latenci. Azure Cosmos DB podporuje kontejnery pro ukládání dat JSON, se říká kolekce, které je možné škálovat toovirtually neomezené velikosti úložiště a zřízené propustnosti. S růstem vaší aplikace je možné službu Azure Cosmos DB bezproblémově elasticky škálovat s předvídatelným výkonem. 


* **Replikace více oblast:** Azure Cosmos DB transparentně replikuje vaše datové oblasti tooall jste spojené s vaším účtem Azure Cosmos DB umožňuje toodevelop aplikace, které vyžadují globální přístup toodata současně kompromisy mezi konzistencí, dostupnosti a výkonu, všechny odpovídající záruky. Azure Cosmos DB poskytuje transparentní regionální převzetí služeb při selhání s více funkci rozhraní API a hello možnost tooelastically škálování propustnost a úložiště v rámci hello zeměkouli. Další informace najdete v tématu [Globální distribuce dat pomocí služby Azure Cosmos DB](distribute-data-globally.md).

* **Ad hoc dotazy se známou syntaxí SQL:** Můžete ukládat heterogenní dokumenty JSON a dotazovat se na ně pomocí známé syntaxe SQL. Azure Cosmos DB využívá vysoce souběžnou zámek volné, protokolu strukturovaná indexování index tooautomatically technologie všechen obsah dokumentů. Díky bohaté dotazy v reálném čase bez pomocné parametry hello nutné toospecify schématu, sekundární indexy nebo zobrazení. Další informace najdete v tématu [Dotazování služby Azure Cosmos DB](documentdb-sql-query.md). 
* **Spuštění JavaScriptu uvnitř databáze hello:** Express logiku aplikace jako uložené procedury, triggery a uživatelem definované funkce (UDF) prostřednictvím standardního JavaScriptu. To umožňuje vaší aplikace logiky toooperate přes data bez obav o hello neshody mezi hello aplikací a schématem databáze hello. Hello rozhraní API DocumentDB poskytuje spouštění logiky aplikace JavaScript přímo v databázovém stroji hello z úplné transakcí. Hello těsná integrace JavaScriptu umožňuje provádění hello INSERT, REPLACE, odstranění a vyberte operací z Javascriptového programu jako izolované transakce. Další informace najdete v tématu [DocumentDB – Programování na straně serveru](programming.md).

* **Přizpůsobitelné úrovně konzistence:** vyberte jednu z pěti dobře definované konzistence úrovně tooachieve optimálního poměru mezi konzistencí a výkonem. Pro dotazy a operace čtení nabízí služba Azure Cosmos DB pět různých úrovní konzistence: silná, omezená neaktuálnost, relace, konzistentní předpona a konečný výsledek. Tyto úrovně konzistence podrobné, dobře definované povolit toomake zvolit vhodný poměr mezi konzistencí, dostupností a latencí. Další informace v [pomocí konzistence úrovně toomaximize dostupnosti a výkonu](consistency-levels.md).

* **Plně spravovaná:** vyloučit prostředky hello nutné toomanage databáze a počítačů. Jako plně spravovaná služba Microsoft Azure můžete provést není nutné toomanage virtuální počítače, nasazení a konfigurace softwaru, spravovat škálování nebo řešit komplexní datové vrstvě upgrady. Každá databáze je automaticky zálohována a chráněna proti selháním v dané oblasti. Můžete snadno přidat účet Azure Cosmos DB a zřídit kapacitu podle potřeby, což vám toofocus ve vaší aplikaci, ne na provoz a správu databáze. 

* **Otevřené řešení:** Pomocí existujících dovedností a nástrojů můžete začít rychle. Programové ošetření hello rozhraní API DocumentDB je jednoduché, srozumitelné a nevyžaduje nové nástroje, tooadopt ani vyhovovat toocustom rozšíření tooJSON nebo JavaScript. Přístup ke všem funkcím hello databáze, včetně CRUD, dotazování a zpracování JavaScriptu přes jednoduché rozhraní RESTful HTTP. Hello rozhraní API DocumentDB pracuje s existujícími formáty, jazyky a standardy zároveň nabízí vysokou hodnotu je hodnotné schopnosti databáze.

* **Automatické indexování:** ve výchozím nastavení, Azure Cosmos DB automaticky indexuje všechny dokumenty hello v databázi hello a nepodporuje očekávat ani nevyžaduje žádné schéma nebo vytváření sekundárních indexů. Nechcete tooindex všechno? Buďte bez obav, můžete také [výslovně nesouhlasit s používáním cest v souborech JSON](indexing-policies.md).

* **Změna kanálu podporu:** změnu kanálu poskytuje seřazený seznam dokumenty v kolekci Azure Cosmos DB v hello pořadí, ve kterém byly upraveny. Tento informační kanál lze použít toolisten pro toodata změny v datech tooreplicate pořadí, aktivovat volání rozhraní API nebo provádět aktualizace zpracování datového proudu. Informační kanál změn je automaticky povolené a snadno toouse: [Další informace o změnit informačního kanálu](https://docs.microsoft.com/azure/cosmos-db/change-feed). 

## <a name="data-management"></a>Jak budete spravovat data s hello rozhraní API DocumentDB?
Hello DocumentDB API pomáhá spravovat JSON data prostřednictvím dobře definovaných databázových prostředků. Tyto prostředky se pro zachování vysoké dostupnosti replikují a je možné je jedinečně adresovat pomocí logického identifikátoru URI. Hello rozhraní API DocumentDB nabízí že jednoduchý HTTP na základě programovací model RESTful pro všechny prostředky. 


účet databáze Azure Cosmos DB Hello je jedinečný obor názvů, který poskytuje přístup k tooAzure Cosmos DB. Před vytvořením databázového účtu, musíte mít předplatné Azure, která umožňuje přístup k různým službám Azure tooa. 

Všechny prostředky ve službě Azure Cosmos DB jsou modelovány a ukládány jako dokumenty JSON. Prostředky se spravují jako položky, kterými jsou dokumenty JSON obsahující metadata, a jako kanály, jimiž jsou kolekce položek. Sady položek se uchovávají ve svých příslušných kanálech.

Hello obrázek níže znázorňuje hello vztahy mezi prostředky Azure Cosmos DB hello:

![Hello hierarchický vztah mezi prostředky v Azure Cosmos DB][1] 

Databázový účet se skládá ze sady databází, kde každá obsahuje několik kolekcí, z nichž každá pak může obsahovat uložené procedury, triggery, funkce UDF, dokumenty a související přílohy. Databáze také přiřazeni uživatelé. každý má sadu oprávnění tooaccess různé další kolekce, uložené procedury, triggery, UDF, dokumenty nebo přílohy. Databáze, uživatelé, oprávnění a kolekce jsou systémem definované prostředky s dobře známými schématy – dokumenty, uložené procedury, triggery, funkce UDF a přílohy obsahují libovolný, uživatelem definovaný obsah JSON.  

> [!NOTE]
> Protože hello DocumentDB rozhraní API byla dříve dostupná jako hello služby Azure DocumentDB, můžete pokračovat v tooprovision, sledovat a spravovat účty vytvořené prostřednictvím hello rozhraní API REST pro správu prostředků Azure, nebo pomocí nástroje hello Azure DocumentDB nebo Azure Cosmos DB názvy prostředků. Zcela zaměnitelným významem používáme názvy hello k odkazování toohello rozhraní API služby Azure DocumentDB. 

## <a name="develop"></a>Jak můžete vyvíjet aplikace s hello rozhraní API DocumentDB?

Azure Cosmos DB zveřejňuje prostředky přes hello rozhraní REST API, kterou lze volat v jakémkoli jazyce schopném zasílat požadavky HTTP/HTTPS. Kromě toho jsme nabízí programovací knihovny pro několik oblíbených jazyků pro hello DocumentDB rozhraní API. Hello klienta knihovny zjednodušují mnoho aspektů práce s hello API řeší podrobnosti, jako je například ukládání adres do mezipaměti, správu výjimek, automatické opakování pokusů a tak dále. Knihovny jsou aktuálně dostupné pro hello následující jazyky a platformy:  

| Ke stažení | Dokumentace |
| --- | --- |
| [.NET SDK](http://go.microsoft.com/fwlink/?LinkID=402989) |[Knihovna .NET](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) |
| [Node.js SDK](http://go.microsoft.com/fwlink/?LinkID=402990) |[Knihovna Node.js](http://azure.github.io/azure-documentdb-node/) |
| [Java SDK](http://go.microsoft.com/fwlink/?LinkID=402380) |[Knihovna Java](/java/api/com.microsoft.azure.documentdb) |
| [JavaScript SDK](http://go.microsoft.com/fwlink/?LinkID=402991) |[Knihovna JavaScript](http://azure.github.io/azure-documentdb-js/) |
| neuvedeno |[Sada JavaScript SDK na straně serveru](http://azure.github.io/azure-documentdb-js-server/) |
| [Python SDK](https://pypi.python.org/pypi/pydocumentdb) |[Knihovna Python](http://azure.github.io/azure-documentdb-python/) |
| neuvedeno | [Rozhraní API pro MongoDB](mongodb-introduction.md)


Pomocí hello [emulátoru DB Cosmos Azure](local-emulator.md), můžete vývoj a testování vaší aplikace místně pomocí hello DocumentDB API bez vytváření předplatného Azure nebo nákladům. Až budete spokojeni s jak funguje aplikaci v emulátoru hello, můžete přepnout toousing účet Azure Cosmos DB v cloudu hello.

Nad rámec basic vytvářet, číst, aktualizovat a odstranit operace, hello rozhraní API DocumentDB poskytuje bohaté rozhraní SQL k získávání dokumentů JSON a podpora protokolu spouštění logiky aplikace JavaScript z transakcí na straně serveru. provádění rozhraní Hello dotazů a skriptů jsou k dispozici prostřednictvím všech knihoven platforem a také hello rozhraní REST API. 

### <a name="sql-query"></a>Dotaz SQL
Hello rozhraní API DocumentDB podporuje dotazování dokumentů pomocí jazyka SQL, který se zobrazuje v hello JavaScript typ systému a výrazy s podporou relačních, hierarchických a prostorových dotazů. dotazovací jazyk DocumentDB Hello je dokumentů JSON tooquery jednoduché, ale výkonné rozhraní. Hello jazyk podporuje podmnožinu gramatiky ANSI SQL a přidává těsnou integraci objekt jazyka JavaScript, pole, vytvářením objektů a volání funkce. Hello rozhraní API DocumentDB poskytuje svůj model dotazování bez explicitního schématu nebo parametrů indexování od vývojáře hello.

Uživatelem definované funkce (UDF) můžete zaregistrována hello DocumentDB rozhraní API a odkazuje v rámci příkazu jazyka SQL, a tím rozšíření hello gramatika toosupport vlastní logiky aplikace. Tyto funkce se píší jako Javascriptové programy a provést v rámci hello databáze. 

Pro vývojáře .NET hello rozhraní API DocumentDB [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx) nabízí i poskytovatele dotazů LINQ. 

### <a name="transactions-and-javascript-execution"></a>Transakce a spouštění JavaScriptu
Hello rozhraní API DocumentDB umožňuje toowrite logiku aplikace jako pojmenované programy vytvořené zcela v JavaScriptu. Tyto programy se registrují ke kolekci a můžou provádět databázové operace na hello dokumenty v dané kolekci. JavaScript je možné zaregistrovat ke spouštění jako trigger, uloženou proceduru nebo uživatelem definovanou funkci. Triggery a uložené procedury lze vytvořit, číst, aktualizovat a odstraňovat dokumenty, zatímco uživatelem definované funkce spouštět jako součást logiky provádění dotazu hello bez kolekce toohello přístup pro zápis.

Spouštění JavaScriptu v rámci hello Cosmos DB je Modelováno hello konceptů podporovaných relačními databázovými systémy. JavaScript zde slouží jako moderní náhrada Transact-SQL. Všechna logika JavaScriptu se spouští v ambientní transakci ACID s izolací snímku. Hello průběhu vykonávání Pokud hello JavaScript vyvolá výjimku, potom hello celá transakce zrušena.

## <a name="are-there-any-online-courses-on-azure-cosmos-db"></a>Existují nějaké online kurzy pro službu Azure Cosmos DB?

Ano, existuje kurz [Microsoft Virtual Academy](https://mva.microsoft.com/en-US/training-courses/azure-documentdb-planetscale-nosql-16847) pro Azure DocumentDB. 

>[!VIDEO https://mva.microsoft.com/en-US/training-courses-embed/azure-documentdb-planetscale-nosql-16847]
>
>

## <a name="next-steps"></a>Další kroky
Máte už účet Azure? Pak můžete se službou Azure Cosmos DB začít pomocí našich [Rychlých startů](../cosmos-db/create-documentdb-dotnet.md), které vás provedou vytvořením účtu a začátky práce se službou Cosmos DB.

[1]: ./media/documentdb-introduction/json-database-resources1.png

