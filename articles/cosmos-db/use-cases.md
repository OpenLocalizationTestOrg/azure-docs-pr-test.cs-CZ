---
title: "aaaCommon použít případy a scénáře pro Azure Cosmos DB | Microsoft Docs"
description: "Další informace o hello top 5 případy používání pro Azure Cosmos DB: uživatel vygeneruje obsah, protokolování událostí, data katalogu, předvolby data uživatele a Internetu věcí (IoT)."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: eca68a58-1a8c-4851-8cf8-6e4d2b889905
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: mimig
ms.openlocfilehash: 115a418e7b18d5033c4d7e64591bf302aea0190b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="common-azure-cosmos-db-use-cases"></a>Běžné případy použití Azure Cosmos DB
Tento článek obsahuje přehled několik běžné případy použití pro Azure Cosmos DB.  Hello doporučení v tomto článku sloužit jako výchozí bod, když budete vyvíjet aplikace s Cosmos DB.   

Po přečtení tohoto článku, budete moct tooanswer hello následující otázky: 

* Jaké jsou hello běžné případy použití pro databázi Cosmos Azure?
* Jaké jsou výhody hello používání Azure Cosmos DB pro maloobchodní aplikace?
* Jaké jsou výhody hello používání Azure Cosmos DB jako úložiště dat pro Internet věcí (IoT) systémy?
* Jaké jsou výhody hello používání Azure Cosmos DB pro webové a mobilní aplikace?

## <a name="introduction"></a>Úvod
[Azure Cosmos DB](../cosmos-db/introduction.md) je globálně distribuované databáze služby společnosti Microsoft. Hello je služba navrženou tooallow zákazníci tooelastically (a nezávisle) škálování propustnost a úložiště napříč libovolný počet zeměpisné oblasti. Azure Cosmos DB je první služba globálně distribuované databáze hello v hello trhu informací o dnešku toooffer komplexní [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/cosmos-db/) zahrnující propustnosti, latenci, dostupnosti a konzistence. 

Projekt Azure Cosmos DB Hello spustit v 2011 jako "Projektu Florencii" tooaddress vývojáře problémové body, které se týkají velkých internetových aplikací v Microsoft. Sledování, zda tyto problémy není jedinečný tooMicrosoft aplikace, jsme se rozhodli toomake Azure Cosmos DB všeobecně dostupná tooexternal vývojáři v 2015 ve formátu hello [Azure DocumentDB](https://azure.microsoft.com/blog/documentdb-moving-to-general-availability/). Služba Hello je interně používáno ubiquitously v rámci společnosti Microsoft a je jedním z hello nejrychlejší rostoucí služby používá Azure vývojáři externě. 

Azure Cosmos DB je globální databáze distribuované, více modelu, která se používá v celé řadě aplikací a případy použití. Je vhodné použít pro žádné [bez serveru](http://azure.com/serverless) aplikace, která musí nízkou pořadí milisekundu odezvy a musí tooscale rychle a globálně. Podporuje několik datových modelech (klíč hodnota, dokumenty, grafy a sloupcové) a přístup k mnoha rozhraní API pro data, včetně [MongoDB](mongodb-introduction.md), [DocumentDB SQL](documentdb-introduction.md), [Gremlin](graph-introduction.md), a [tabulky Azure](table-introduction.md) nativně a rozšiřitelné způsobem. 

Hello Následují některé atributy Azure Cosmos DB, které dobře hodí pro vysoce výkonné aplikace s globální ctižádost.

* Azure Cosmos DB nativně oddíly data pro vysokou dostupnost a škálovatelnost. Azure Cosmos DB nabízí záruky 99,99 % dostupnost, propustnost, nízkou latenci a konzistence.
* Azure Cosmos DB má úložiště SSD zálohovaný s nízkou latencí pořadí milisekundu odezvy.
* Azure Cosmos DB je podpora pro úrovně konzistence jako předpona případné a konzistentní, relace a typu ohraničenou prošlostí umožňuje úplnou flexibilitu a nízké náklady na výkon poměr. Žádná databáze služba nabízí tolik flexibilitu jako databázi Cosmos Azure v úrovně konzistence. 
* Azure Cosmos DB má flexibilní cenový model dat popisný, který měřidla úložiště a propustnost nezávisle.
* Azure Cosmos DB – model vyhrazenou propustností umožňuje toothink z hlediska počet čtení/zápisu místo procesoru nebo paměti nebo IOPs hello základní hardware.
* Azure Cosmos DB návrhu umožňuje škálovat toomassive požadavek svazky v hello pořadí bilión požadavků za den.

Tyto atributy jsou užitečné v webové, mobilní a herní a aplikace IoT, které musíte nízkou odezvy a musí toohandle masivní objemy čtení a zápisy.

## <a name="iot-and-telematics"></a>IoT a telematika
Případy použití IoT běžně některé vzory v tom, jak se ingestování, proces, sdílet a ukládat data.  Tyto systémy nejprve tooingest shluky dat ze senzorů zařízení různých národních prostředí. Tyto systémy dále zpracovávat a analyzovat streamování přehledy v reálném čase tooderive data. Hello data je potom archivovat toocold úložiště pro analýzu batch. Microsoft Azure nabízí bohaté služby, které lze použít pro IoT používat případech včetně Azure Cosmos DB, Azure Event Hubs, Azure Stream Analytics, centra oznámení Azure, Azure Machine Learning, Azure HDInsight a PowerBI. 

![Azure referenční architektura Cosmos DB IoT](./media/use-cases/iot.png)

Shluky dat můžete konzumaci Azure Event Hubs jako nabízí přijímání dat Vysoká propustnost s nízkou latencí. Požity data, která potřebují toobe zpracování pro přehledy v reálném čase může být funneled tooAzure Stream Analytics pro analýzu v reálném čase. Data je možné načíst do Azure Cosmos DB pro zadávání dotazů ad hoc. Po načtení dat hello do Azure Cosmos DB hello data jsou připravena toobe dotaz. Kromě toho nová data a data tooexisting změny lze číst na změnu informačního kanálu. Změna kanálu je trvalé, připojit jen protokolu, která ukládá změny tooCosmos DB kolekce v sekvenčním pořadí. Dobrý den, všechna data nebo pouze změny toodata v Azure Cosmos DB lze použít jako referenční data v rámci analýzy v reálném čase. Data navíc další může být vylepšení a zpracovává připojování tooHDInsight Azure Cosmos DB dat pro úlohy Pig, Hive nebo mapy nebo snižte.  Přesnějších dat je pak načten back tooAzure Cosmos DB pro vytváření sestav.   

Ukázka řešení IoT pomocí Azure Cosmos DB, EventHubs a Storm, najdete v části hello [hdinsight-storm příklady úložišti na Githubu](https://github.com/hdinsight/hdinsight-storm-examples/).

Další informace o Azure nabídky pro IoT najdete v tématu [hello vytvořit si Internet věcí](http://www.microsoft.com/server-cloud/internet-of-things.aspx). 

## <a name="retail-and-marketing"></a>Prodej a marketing
Azure Cosmos DB se hojně používá v platformy společnosti Microsoft vlastní elektronické obchodování, se systémem hello Windows Store nebo XBox Live. Je je také použít v odvětví prodejní hello k ukládání dat katalogu a pro událost sourcing v pořadí zpracování kanály.

Scénáře použití data CATALOG zahrnuje ukládání a dotazování sadu atributů pro entity, jako je například osob, míst a produkty. Některé příklady data katalogu jsou uživatelské účty, katalogy produktů, registrech zařízení IoT a faktury systémů materiálů. Atributy pro tato data se může lišit a přes požadavky na čas toofit aplikace, můžete změnit.

Zvažte například katalogu produktů pro dodavatele automobilu částí. Všechny části může mít svůj vlastní atributy v toohello přidání běžné se atributy, které sdílejí všechny části. Atributy pro určitou část kromě toho můžete změnit hello následující roku, kdy je vydán nový model. Azure Cosmos DB podporuje flexibilní schémata a hierarchických dat, a proto se skvěle hodí pro ukládání dat katalogu produktů.

![Azure Cosmos DB prodejní katalogu referenční architektura](./media/use-cases/product-catalog.png)

Azure Cosmos DB se často používá pro událost sourcing toopower událostí řízené architektury pomocí jeho [změnit informačního kanálu](change-feed.md) funkce. informační kanál změnu Hello poskytuje podřízené mikroslužeb hello možnost tooreliably a přírůstkově vloží pro čtení a tooan Azure Cosmos DB provedeny aktualizace (například pořadí událostí). Tuto funkci lze využít tooprovide trvalé událostí uložit jako zprostředkovatel zprávy událostí změny stavu a jednotky pořadí zpracování pracovního postupu mezi mnoha mikroslužeb (které může být implementováno jako [bez serveru Azure Functions](http://azure.com/serverless)).

![Azure DB Cosmos řazení kanálu referenční architektura](./media/use-cases/event-sourcing.png)

Kromě toho data uložená v Azure Cosmos DB lze integrovat s HDInsight pro analýzu velkých objemů dat pomocí Apache Spark úlohy. Podrobnosti na hello konektor Spark pro Azure Cosmos DB najdete v tématu [spustit úlohu Spark s Cosmos databáze a HDInsight](spark-connector.md).

## <a name="gaming"></a>Hraní her
Hello databázové vrstvy je zásadní součástí herní aplikace. Moderní hry provádět grafické zpracování na klientech mobilních nebo konzoly, ale závisí na toodeliver cloudu hello přizpůsobit a přizpůsobený obsah jako statistiky ve hře, sociální média integrace a nejlepších výsledků tabulky. Hry často vyžadují jedním milisekundu latenci pro čtení a zapíše tooprovide nestačí, aby ve hře prostředí. Herní databáze potřebuje toobe rychlé a být schopný toohandle masivní špičky v požadavků během nové herní spustí a aktualizace funkcí.

Používá Azure Cosmos DB hry jako [hello proti neaktivní: Ne Man krajině](https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/) podle [další hry](http://www.nextgames.com/), a [bylo 5: vychovatelům](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Azure Cosmos DB poskytuje následující hello výhody toogame vývojáři:

* Azure Cosmos DB umožňuje výkonu toobe škálovat nahoru nebo dolů, pružně. To umožňuje hry toohandle aktualizaci profilu a statistiky z desítek toomillions souběžných hráči tím, že jednoho volání rozhraní API.
* Podporuje Azure Cosmos DB milisekundu čte a zapisuje toohelp vyhnout žádné pomalou průběhu hry.
* Automatické indexování Azure Cosmos DB umožňuje filtrování více různých vlastností v reálném čase, například najít přehrávače podle jejich interní player ID nebo jejich GameCenter, Facebook, Google ID nebo dotaz na základě player členství v sdružení. To je možné bez vytváření složitých indexování nebo infrastrukturu horizontálního dělení.
* Sociální funkce včetně ve hře chatovací zprávy, player sdružení členství ve skupinách, výzvy dokončit, nejlepších výsledků tabulky a sociálních grafy jsou jednodušší tooimplement se schématem, flexibilní.
* Azure DB Cosmos jako spravované platformy jako služba (PaaS) vyžaduje minimální instalaci a správu pracovních tooallow pro rychlé iterace a snížit čas toomarket.

![Azure Cosmos DB herní referenční architektura](./media/use-cases/gaming.png)

## <a name="web-and-mobile-applications"></a>Webové a mobilní aplikace
Azure Cosmos DB se často používá v rámci webové a mobilní aplikace a je dobře hodí pro modelování sociálních interakce integrace služeb třetích stran a pro vytváření bohatě přizpůsobené prostředí. Hello Cosmos DB sady SDK může být použité sestavení bohaté iOS a Android aplikací pomocí Oblíbené hello [Xamarin framework](mobile-apps-with-xamarin.md).  

### <a name="social-applications"></a>Sociální aplikace
Běžně používá pro Azure Cosmos DB toostore a dotaz uživatelem generovaný obsah (UGC) je pro webové, mobilní a sociálních médií aplikace. Některé příklady UGC jsou relace konverzace, tweetů, příspěvcích na blogu, hodnocení a komentáře. Hello UGC v sociálních sítích aplikace je často blend volného formátu textu, vlastností, značky a vztahy, které nejsou ohraničené pevné struktura. Obsah, jako je například konverzace, komentáře a příspěvky můžete uloženy v Cosmos databázi bez nutnosti transformace nebo komplexní objekt toorelational mapování vrstvy.  Vlastnosti dat lze přidat nebo změnit snadno toomatch požadavky, protože vývojáři iterace v kódu aplikace hello, takže povýšení rychlý vývoj.  

Aplikace, které integraci se sociálními sítěmi třetích stran musí odpovídat toochanging schémata z těchto sítí. Jako data se automaticky indexovaný ve výchozím nastavení v Cosmos DB, data jsou připravena toobe dotaz kdykoli. Proto tyto aplikace mají hello flexibilitu tooretrieve projekce podle jejich potřeb příslušné.

Mnoho aplikací sociálních hello spusťte v globálním měřítku a mohou vykazovat vzorce nepředvídatelné. Flexibilita při škálování hello úložiště dat je důležité jako hello aplikační vrstvu škáluje toomatch využití vyžádání.  Můžete škálovat tak, že přidáte další data oddíly pod účtem Cosmos DB.  Kromě toho můžete také vytvořit další účty Cosmos DB nad několika oblastmi. Dostupnost služby oblast Cosmos DB, najdete v části [oblasti Azure](https://azure.microsoft.com/regions/#services).

![Azure Cosmos DB webové aplikace referenční architektura](./media/use-cases/apps-with-global-reach.png)

### <a name="personalization"></a>Přizpůsobení
V současné době moderních aplikací mít komplexní zobrazení a prostředí. Toto jsou obvykle dynamická, stravovací toouser předvolby nebo atmosféru a značka potřebám. Proto aplikace potřebují mít tooretrieve přizpůsobených nastavení toobe efektivně toorender prvky uživatelského rozhraní a prostředí rychle. 

Formát JSON, formátu nepodporuje Cosmos databáze, je efektivní formátu toorepresent uživatelského rozhraní rozložení dat, protože není pouze jednoduché, ale také můžete snadno interpretovat JavaScript. Cosmos DB nabízí přizpůsobitelné úrovně konzistence umožňující rychlé čtení s nízkou latencí a zápisu. Proto ukládání uživatelského rozhraní rozložení dat včetně individuální nastavení, jako dokumenty JSON v Cosmos DB je efektivní znamená tooget tato data napříč hello přenosu.

![Azure Cosmos DB webové aplikace referenční architektura](./media/use-cases/personalization.png)

## <a name="next-steps"></a>Další kroky
tooget začít s Azure Cosmos DB, postupujte podle našich [rychlé zahájení](create-documentdb-dotnet.md), který vás provede procesem vytvoření účtu a úvod do Cosmos DB. 

Nebo, pokud vás zajímají další informace o zákazníky používající Cosmos DB tooread, hello následující scénáře zákazníků jsou k dispozici:

* [Jet.com](https://jet.com). Elektronické obchodování challenger očí hello nejvyšší pozici, běží v cloudu Microsoft hello, využívá Cosmos DB v globálním měřítku.
* [Asos.com](http://www.asos.com/). Asos.com je British online způsobem a kosmetické úložiště. Především zaměřené na malí dospělí, Asos prodává přes 850 značky, jakož i vlastní rozsah oblečení a příslušenství.
* [Toyota](https://www.toyota.com/). Toyota Motor Corporation je japonské automobilu výrobcem. Toyota využít Cosmos DB pro globální aplikaci IoT.
* [Citrix](https://customers.microsoft.com/story/citrix). Citrix sama vyvinula jednotného přihlašování řešení pomocí Azure Service Fabric a Azure Cosmos DB
* [TEXA](https://customers.microsoft.com/story/texaspa) na TEXA revoluční řešení IoT pro vlastníky vehicle pomáhá šetřit čas, peníze, plynu – a případně je umístěn.
* [Na Domino Pizza](https://www.dominos.com). Na Domino Pizza Inc. je American pizza restaurace řetězec.
* [Ovládací prvky Janíček](http://www.johnsoncontrols.com). Ovládací prvky Janíček je globální smíšené technologie a více průmyslových vedoucí obsluhující celou řadu zákazníků ve víc než 150 zemích.
* [Microsoft Windows, univerzální úložiště, Azure IoT Hub, Xbox Live a dalším službám internetových](https://azure.microsoft.com/blog/how-azure-documentdb-planet-scale-nosql-helps-run-microsoft-s-own-businesses/). Jak Microsoft sestavení enormně škálovatelné služby Azure Cosmos DB.
* [Tým Microsoft Data a analýzy](https://customers.microsoft.com/story/microsoftdataandanalytics). Společnosti Microsoft Data a analýzy tým dosahuje velkých objemů dat kolekce planetu škálování s Azure Cosmos DB
* [Sulekha.com](https://customers.microsoft.com/story/sulekha-uses-azure-documentdb-to-connect-customers-and-businesses-across-india). Sulekha využívá Azure Cosmos DB tooconnect zákazníků a firmy v Indii.
* [NewOrbit](https://customers.microsoft.com/story/neworbit-takes-flight-with-azure-documentdb). NewOrbit trvá letu s Azure Cosmos DB.
* [Affinio](https://customers.microsoft.com/doclink/affinio-switches-from-aws-to-azure-documentdb-to-harness-social-data-at-scale). Affinio přepíná z AWS tooAzure Cosmos DB tooharness sociální data ve velkém měřítku.
* [Další hry](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/). Hello vycházkové zpráv: Ne Man demokratická krajině herní soars příliš č. 1 nepodporuje Azure Cosmos DB.
* [Bylo](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Tom, jak bylo 5 implementují sociálních hraní her pomocí Azure Cosmos DB.
* [Galerie Analytics Cortana](https://azure.microsoft.com/blog/cortana-analytics-gallery-a-scalable-community-site-built-on-azure-documentdb/). Cortana Analytics Galerie - lokalitu škálovatelné komunity vytvořené v Azure Cosmos DB.
* [Uloženy](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18602). Úvodní integrátor poskytuje globální přehledy mezinárodní podniky v minutách s flexibilní cloudové technologie.
* [Příspěvky republika](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18639). Přidání intelligence toohello zprávy tooprovide informací s účelem aktivnější občanů. 
* [Mezinárodní skupině](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18653). Pro konzistentní barvu v celém světě hello většiny hlavních značek zapněte tooSGS. A skupině změní tooAzure.
* [Telenor](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18608). Neoprávněných Telenor používá toomove hello cloudu s hello rychlosti spuštění. 
* [XOMNI](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18667). úložiště Hello hello budoucí běží na rychlé vyhledávání a hello snadno toku dat
* [Nucleo](https://customers.microsoft.com/story/azure-based-software-platform-breaks-down-barriers-bet). Platforma softwaru na základě Azure rozpis překážek mezi podnikům a zákazníků
* [Weka](https://customers.microsoft.com/story/weka-smart-fridge-improves-vaccine-management-so-more-people-can-be-protected-against-diseases). Inteligentní ledničky weka zlepšuje správu očkovací tak lidem se dají chránit proti nákaz
* [Oranžové Tribes](https://customers.microsoft.com/story/theres-more-to-that-food-app-than-meets-the-eye-or-the-mouth). Existuje více toothat jídlo aplikace než splňuje hello oka nebo hello úst.
* [Skutečné Madridu](https://customers.microsoft.com/story/real-madrid-brings-the-stadium-closer-to-450-million-f). Skutečné Madridu přináší hello stadium blíže too450 mil. ventilátory kolem hello zeměkouli s hello cloudu Microsoftu.
* [Tuku](https://customers.microsoft.com/story/tuku-makes-car-buying-fun-with-help-from-azure-services). TUKU díky car kdybyste kupovali fun pomoci ze služby Azure
