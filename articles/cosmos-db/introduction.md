---
title: aaaIntroduction tooAzure Cosmos DB | Microsoft Docs
description: "Přečtěte si něco o službě Azure Cosmos DB. Tato globálně distribuovaná databáze s více modely je navržena s ohledem na nízkou latenci, elastickou škálovatelnost a vysokou dostupnost."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: a855183f-34d4-49cc-9609-1478e465c3b7
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/14/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: f2acbe99f425b2f07a62bbbb4324aa48f1037481
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="welcome-tooazure-cosmos-db"></a>Vítejte tooAzure Cosmos DB

Azure Cosmos DB je globálně distribuovaná databáze Microsoftu pro více modelů. Díky hello kliknutím na tlačítko Azure Cosmos DB vám umožní tooelastically a nezávisle škálování propustnost a úložiště napříč libovolný počet zeměpisné oblasti Azure. Nabízí záruku propustnosti, latence, dostupnosti a konzistence s kompletními [smlouvami o úrovni služeb](https://aka.ms/acdbsla) (SLA), které někdy nenabízejí žádné jiné databázové služby.

![Azure Cosmos DB je celosvětově distribuovaná databázová služba od Microsoftu, která nabízí elastické horizontální navýšení kapacity, zaručenou nízkou latenci, pět modelů konzistence a kompletní zaručené smlouvy SLA](./media/introduction/azure-cosmos-db.png)

## <a name="solutions-that-benefit-from-azure-cosmos-db"></a>Řešení, kterým služba Azure Cosmos DB přináší výhody

Všechny [webové, mobilní, hry a aplikace či aplikace IoT](use-cases.md) vyžadující toohandle masivní objemy čtení a zápisy na [globální](distribute-data-globally.md) škálování s nízkou odezvy pro různá data, bude mít užitek z Azure Cosmos DB [zaručit](https://azure.microsoft.com/support/legal/sla/cosmos-db/) dostupnost, Vysoká propustnost, nízkou latenci a přizpůsobitelné konzistence.

## <a name="key-capabilities"></a>Klíčové funkce
Jako služba globálně distribuovanou databázi databázi Cosmos Azure poskytuje následující možnosti toohelp vytvářet škálovatelné a vysoce přizpůsobivém aplikace hello:

* **Globální distribuce na klíč**
    * Můžete [distribuci dat](distribute-data-globally.md) tooany počet [oblastí Azure](https://azure.microsoft.com/regions/), s hello [klikněte na tlačítka s](tutorial-global-distribution-documentdb.md). To umožňuje vám tooput vaše data, kde uživatelé jsou, zajištění hello nejnižší možnou latenci tooyour zákazníků. 
    * Pomocí Azure Cosmos DB více funkci rozhraní API, vždy aplikace hello ví, kde je hello nejbližší oblast a odešle požadavky toohello nejbližší datového centra. Všechny tyto je možné bez změny konfigurace, nastavení vaší oblasti zápisu a mnoho pro čtení oblastech, jako má a hello zbývající bude zpracován za vás.

* **Více datových modelů a oblíbená rozhraní API pro přístup k datům a dotazování na ně**
    * Hello atom sekvence záznamů (ARS) na základě datový model, který Azure Cosmos DB je založený na nativně podporuje více datové modely, včetně, ale bez omezení toodocument, grafu, klíč hodnota, tabulka a sloupcovém datové modely.
    * Rozhraní API pro hello následující datové modely jsou podporovány pomocí sady SDK k dispozici v několika jazycích:
        * [Rozhraní DocumentDB API](documentdb-introduction.md)
        * [Rozhraní MongoDB API](mongodb-introduction.md)
        * [Rozhraní Table API](table-introduction.md)
        * [Rozhraní Graph (Gremlin) API](graph-introduction.md)
        * Další datové modely se připravují. 

* **Elastické škálování propustnosti a úložiště na vyžádání, celosvětově**
    * Snadno škálujte propustnost databáze v přírůstcích po [sekundách](request-units.md), přičemž to můžete kdykoli změnit. 
    * Škálování velikost úložiště [transparentně a automaticky](partition-data.md) toohandle požadavků velikost teď a navždy.

* **Vytváření aplikací s rychlou odezvou a vysokou důležitostí pro chod firmy**
    * Azure Cosmos DB zaručuje začátku do konce s nízkou latencí v hello 99th percentilu tooits zákazníků. 
    * Pro typické 1 KB položku Cosmos DB zaručuje začátku do konce latence čtení v části 10 ms a indexované zápisy na hello 99th percentilu, v části 15 ms hello uvnitř stejné oblasti Azure. Střední latence Hello se výrazně nižší (v části 5 ms).

* **Zajištění neustálé dostupnosti**
    * 99,99% dostupnost v jedné oblasti.
    * Nasazení tooany počet [oblastí Azure](https://azure.microsoft.com/regions) vyšší dostupnosti.
    * [Simulujte selhání](regional-failover.md) jedné nebo více oblastí se zárukou nulových ztrát dat. 

* **Zápis globálně distribuované aplikace hello pravým způsob**
    * Pět [konzistence modely](consistency-levels.md) modely poskytují spektrum silnou konzistenci SQL jako všechny hello konzistence typu případné způsobem jako tooNoSQL a každý věc mezi nimi. 
  
* **Záruka vrácení peněz**
    * Data budou k dispozici rychle, jinak dostanete zpátky peníze. 
    * [Smlouvy o úrovni služeb](https://aka.ms/acdbsla) jsou zárukou dostupnosti, latence, propustnosti a konzistence. 

* **Není potřeba správa schématu/indexu databáze**
    * Už si nemusíte dělat starosti se zajištěním synchronizace databázového schématu a indexů ve schématu aplikace. Schéma není zapotřebí. 
    * Azure Cosmos DB databázový stroj je plně vázané na schéma – ji automaticky indexuje všechny údaje, které hello ingestuje bez nutnosti žádné schéma nebo indexy a slouží neuvěřitelně rychlou dotazy. 

* **Nízké náklady na vlastnictví**
    * Pětkrát tooten [větší finanční efektivita](https://aka.ms/cosmos-db-tco-paper) než nespravované řešení.
    * Třikrát levnější než DynamoDB.

## <a name="capability-comparison"></a>Porovnání schopností

Azure Cosmos DB poskytuje nejlepší možnosti hello relačních a nerelačních databází.

| Možnosti | Relační databáze   | Nerelační databáze (NoSQL) |    Azure Cosmos DB |
| --- | --- | --- | --- |
| Globální distribuce | Ne | Ne | Ano, distribuce na klíč ve více než 30 oblastech s rozhraními API pro vícenásobné navádění|
| Horizontální škálování | Ne | Ano | Ano, můžete nezávisle škálovat úložiště a propustnost | 
| Záruky latence | Ne | Ano | Ano, 99 % čtení do 10 ms a zápisů do 15 ms | 
| Vysoká dostupnost | Ne | Ano | Ano, služba Cosmos DB je vždycky aktivní, má kompromisy PACELC a poskytuje možnosti automatického a ručního převzetí služeb při selhání|
| Datový model + API | Relační + SQL | Více modelů + OSS API | Více modelů + SQL + OSS API (další už brzy) |
| Smlouvy SLA | Ano | Ne | Ano, komplexní smlouvy SLA pro latenci, propustnost, konzistenci a dostupnost |


## <a name="next-steps"></a>Další kroky
Začínáme se službou Azure Cosmos DB s využitím jedné ze čtyř šablon Rychlý start:

* [Začínáme s rozhraním API DocumentDB služby Azure Cosmos DB](create-documentdb-dotnet.md)
* [Začínáme s rozhraním API MongoDB služby Azure Cosmos DB](create-mongodb-nodejs.md)
* [Začínáme s rozhraním API Graph služby Azure Cosmos DB](create-graph-dotnet.md)
* [Začínáme s rozhraním API Table služby Azure Cosmos DB](create-table-dotnet.md)
