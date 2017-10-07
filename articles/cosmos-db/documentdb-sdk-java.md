---
title: "Azure Cosmos DB: DocumentDB Java rozhraní API, sadu SDK a prostředky | Microsoft Docs"
description: "Další informace o hello Java API a sady SDK, včetně data vydání, vyřazení dat a změny provedené mezi každou verzi hello Azure Cosmos databáze DocumentDB Java SDK."
services: cosmos-db
documentationcenter: java
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 7861cadf-2a05-471a-9925-0fec0599351b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 07/11/2017
ms.author: khdang
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8ef43ebeb7ae1bfc55512c4a7489c1b7930122d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-documentdb-java-sdk-release-notes-and-resources"></a>Azure Cosmos DB: DocumentDB Java SDK poznámky k verzi a prostředky
> [!div class="op_single_selector"]
> * [.NET](documentdb-sdk-dotnet.md)
> * [Informační kanál změnu rozhraní .NET](documentdb-sdk-dotnet-changefeed.md)
> * [.NET Core](documentdb-sdk-dotnet-core.md)
> * [Node.js](documentdb-sdk-node.md)
> * [Java](documentdb-sdk-java.md)
> * [Python](documentdb-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/documentdb/)
> * [Poskytovatel prostředků REST](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td>**Stažení sady SDK**</td><td>[Maven](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)</td></tr>

<tr><td>**Dokumentaci k rozhraní API**</td><td>[Referenční dokumentace rozhraní API Java](/java/api/com.microsoft.azure.documentdb)</td></tr>

<tr><td>**Přispívat tooSDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-java/)</td></tr>

<tr><td>**Začínáme**</td><td>[Začínáme s hello sady Java SDK](documentdb-java-get-started.md)</td></tr>

<tr><td>**Kurz vývoje webové aplikace**</td><td>[Vývoj webových aplikací s Azure Cosmos DB](documentdb-java-application.md)</td></tr>

<tr><td>**Aktuální podporovaný modul runtime**</td><td>[JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## <a name="release-notes"></a>Poznámky k verzi

### <a name="a-name11201120"></a><a name="1.12.0"/>1.12.0
* Rozdělí toorequest důležitých oprav chyb zpracování během oddílu.
* Oprava problému s hello silné a BoundedStaleness úrovně konzistence.

### <a name="a-name11101110"></a><a name="1.11.0"/>1.11.0
* Přidaná podpora pro novou úroveň konzistence volá ConsistentPrefix.
* Opravit chyby při čtení kolekce v režimu relace.

### <a name="a-name11001100"></a><a name="1.10.0"/>1.10.0
* Povolená podpora dělenou kolekci s jako málo jako odpovídající 2500 RU za sekundu a škálování v přírůstcích po 100 RU za sekundu.
* Pevná chyby ve hello nativní sestavení, které může způsobit výjimku NullRef v některé dotazy.

### <a name="a-name196196"></a><a name="1.9.6"/>1.9.6
* Vyřešený chyby v konfiguraci modulu hello dotazu, který může způsobit, že výjimky pro dotazy v režimu brány.
* Vyřešili několik chyb v kontejneru hello relace, který může způsobit výjimku "Vlastník prostředek nebyl nalezen" pro požadavky okamžitě po vytvoření kolekce.

### <a name="a-name195195"></a><a name="1.9.5"/>1.9.5
* Přidaná podpora pro dotazy agregace (COUNT, MIN, MAX, součet a průměr). V tématu [podporu agregace](documentdb-sql-query.md#Aggregates).
* Přidaná podpora pro změnu informačního kanálu.
* Byla přidána podpora pro informace o kvótě kolekce prostřednictvím RequestOptions.setPopulateQuotaInfo.
* Byla přidána podpora pro uloženou proceduru skriptu protokolování prostřednictvím RequestOptions.setScriptLoggingEnabled.
* Opravit chyby, kde dotazu v režimu DirectHttps může přestat reagovat při zjištění chyby omezení.
* Vyřešený chyby v režimu konzistence relace.
* Opravit chyby, které můžou způsobit výjimky NullReferenceException v HttpContext, když je vysoká rychlost požadavků.
* Vylepšený výkon DirectHttps režimu.

### <a name="a-name194194"></a><a name="1.9.4"/>1.9.4
* Podpora přidání jednoduchý klient založený na instancích proxy s rozhraním API ConnectionPolicy.setProxy().
* Přidání rozhraní API DocumentClient.close() tooproperly vypnutí DocumentClient instance.
* Výkon vylepšené dotazů v režimu přímého připojení odvozením hello plán dotazu z nativní sestavení hello místo hello brány.
* Nastavit FAIL_ON_UNKNOWN_PROPERTIES = false, takže uživatelé nepotřebují toodefine JsonIgnoreProperties v jejich POJO.
* Rozdělili protokolování toouse SLF4J.
* Vyřešili několik dalších chyb ve čtecím modulu konzistence.

### <a name="a-name193193"></a><a name="1.9.3"/>1.9.3
* Pevná chyby ve hello připojení správy tooprevent připojení nevracení v režimu přímého připojení.
* Vyřešený chyby v dotazu TOP hello, kde se může vyvolat NullReferenece výjimka.
* Lepší výkon snížením hello počet volání sítě pro vnitřní mezipaměti hello.
* Přidání stavový kód, aktivity a URI požadavku v DocumentClientException pro lepší řešení potíží.

### <a name="a-name192192"></a><a name="1.9.2"/>1.9.2
* Ve správě hello připojení pro stabilitu byl opraven problém.

### <a name="a-name191191"></a><a name="1.9.1"/>1.9.1
* Přidaná podpora pro úrovně konzistence BoundedStaleness.
* Byla přidána podpora pro přímé připojení k síti pro operace CRUD pro dělené kolekce.
* Opravit chyby v dotazování databáze s SQL.
* Chyby opraveny v mezipaměti relace hello kde token relace může být nastaveny nesprávně.

### <a name="a-name190190"></a><a name="1.9.0"/>1.9.0
* Přidaná podpora pro křížové paralelní dotazy oddílu.
* Byla přidána podpora pro horní nebo ORDER BY a dotazy pro dělené kolekce.
* Přidaná podpora pro silnou konzistenci.
* Byla přidána podpora pro název na základě požadavky při použití přímé připojení.
* Opravené toomake zůstanou ActivityId mezi všechny žádosti o opakování konzistentní.
* Pevné chyby související s toohello mezipaměť relace při opětovném vytváření kolekce se hello stejný název.
* Přidání mnohoúhelníku a datové typy LineString při zadávání kolekce indexování zásady pro geografického vymezení prostorových dotazů.
* Opravené problémy s Java dokumentace pro jazyk Java 1.8.

### <a name="a-name181181"></a><a name="1.8.1"/>1.8.1
* Pevná chyby ve PartitionKeyDefinitionMap toocache kolekce tvořené jedním oddílem a není zkontrolujte navíc načíst oddílu o klíč.
* Pevné opakování toonot chyb, je-li hodnotu klíče oddílu nesprávné.

### <a name="a-name180180"></a><a name="1.8.0"/>1.8.0
* Podpora přidání hello účty databáze více oblast.
* Přidaná podpora pro automatické opakování na omezenému požadavků s možností toocustomize hello maximální pokusy o opakování a maximum opakujte doba čekání.  Zobrazit RetryOptions a ConnectionPolicy.getRetryOptions().
* Nepoužívané IPartitionResolver na základě vlastního rozdělení kódu. Použijte dělené kolekce pro vyšší propustnost a úložiště.

### <a name="a-name171171"></a><a name="1.7.1"/>1.7.1
* Podpora zásad přidané opakování omezení.  

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
* Přidání čas toolive (TTL) podpory pro dokumenty.

### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0
* Implementovat [oddíly kolekce](partition-data.md) a [úrovně výkonu uživatelem definované](performance-levels.md).

### <a name="a-name151151"></a><a name="1.5.1"/>1.5.1
* Pevná chyby ve hodnoty hash toogenerate HashPartitionResolver v little endian toobe konzistentní s dalších sadách SDK.

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0
* Přidejte hodnotu Hash p & ro tooassist překladače oddílu s horizontálního dělení aplikacemi v rámci více oddílů.

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0
* Implementujte Upsert. Přidat nové metody upsertXXX toosupport Upsert funkce.
* Implementujte, na základě ID směrování. Žádné změny veřejné rozhraní API, všechny změny interní.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
* Verze přeskočen číslo verze toobring v zarovnání s dalších sadách SDK

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Podporuje geoprostorové indexu
* Ověří vlastnost id pro všechny prostředky. Identifikátory prostředků nesmí obsahovat?, /, #, \, znaků nebo končit mezerou.
* Přidá nový tooResourceResponse "průběh transformace index" záhlaví.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Implementuje zásady indexování V2

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA SDK

## <a name="release--retirement-dates"></a>Verze & vyřazení kalendářních dat
Microsoft bude poskytovat oznámení alespoň **dobu 12 měsíců** před vyřazením z provozu v pořadí toosmooth hello přechod tooa novější nebo v nepodporované verzi sady SDK.

Nové funkce a funkce a optimalizace, jsou přidány pouze aktuální toohello SDK a jako takový je, že jste vždy upgradu toohello nejnovější verze sady SDK, abyste co nejdříve.

Všechny žádosti o tooCosmos DB pomocí vyřazeno sady SDK budou odmítnuty službou hello.

> [!WARNING]
> Všechny verze hello DocumentDB SDK pro předchozí tooversion Java **1.0.0** vyřadí na **29. února 2016**.
> 
> 

<br/>

| Verze | Datum vydání | Datum vyřazení |
| --- | --- | --- |
| [1.12.0](#1.12.0) |11 července 2017 |--- |
| [1.11.0](#1.11.0) |10. května 2017 |--- |
| [1.10.0](#1.10.0) |11 března 2017 |--- |
| [1.9.6](#1.9.6) |21. února 2017 |--- |
| [1.9.5](#1.9.5) |31. ledna 2017 |--- |
| [1.9.4](#1.9.4) |24 od listopadu 2016 |--- |
| [1.9.3](#1.9.3) |30. října 2016 |--- |
| [1.9.2](#1.9.2) |28. října 2016 |--- |
| [1.9.1](#1.9.1) |26 října 2016 |--- |
| [1.9.0](#1.9.0) |03. října 2016 |--- |
| [1.8.1](#1.8.1) |30. června 2016 |--- |
| [1.8.0](#1.8.0) |14. června 2016 |--- |
| [1.7.1](#1.7.1) |30. dubna 2016 |--- |
| [1.7.0](#1.7.0) |27. dubna 2016 |--- |
| [1.6.0](#1.6.0) |29. března 2016 |--- |
| [1.5.1](#1.5.1) |31. prosince 2015 |--- |
| [1.5.0](#1.5.0) |04 prosince 2015 |--- |
| [1.4.0](#1.4.0) |05 říjen 2015 |--- |
| [1.3.0](#1.3.0) |05 říjen 2015 |--- |
| [1.2.0](#1.2.0) |05 srpen 2015 |--- |
| [1.1.0](#1.1.0) |09 července 2015 |--- |
| [1.0.1](#1.0.1) |12 květen 2015 |--- |
| [1.0.0](#1.0.0) |07 duben 2015 |--- |
| 0.9.5-prelease |09 března 2015 |29. února 2016 |
| 0.9.4-prelease |17 únor 2015 |29. února 2016 |
| 0.9.3-prelease |13. ledna 2015 |29. února 2016 |
| 0.9.2-prelease |19 prosince 2014 |29. února 2016 |
| 0.9.1-prelease |19 prosince 2014 |29. února 2016 |
| 0.9.0-prelease |10. prosince 2014 |29. února 2016 |

## <a name="faq"></a>Nejčastější dotazy
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Viz také
toolearn Další informace o Cosmos databáze, najdete v části [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) stránku služby.

