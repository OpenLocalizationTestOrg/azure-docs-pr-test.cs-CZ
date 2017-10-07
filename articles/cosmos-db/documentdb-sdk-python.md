---
title: "aaaAzure Cosmos DB Python rozhraní API, sadu SDK a prostředky | Microsoft Docs"
description: "Další informace o hello rozhraní API jazyka Python a sady SDK, včetně data vydání, vyřazení dat a změny provedené mezi každou verzi hello Azure Cosmos DB Python SDK."
services: cosmos-db
documentationcenter: python
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 3ac344a9-b2fa-4a3f-a4cc-02d287e05469
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/24/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1a164b72d2bd819de87df0229357b82e2177af2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-python-sdk-release-notes-and-resources"></a>Azure Python Cosmos DB SDK: Poznámky k verzi a prostředky
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

<tr><td>**Stáhněte si sadu SDK**</td><td>[Úložiště PyPI](https://pypi.python.org/pypi/pydocumentdb)</td></tr>

<tr><td>**Dokumentaci k rozhraní API**</td><td>[Referenční dokumentace rozhraní API jazyka Python](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.html)</td></tr>

<tr><td>**Pokyny k instalaci sady SDK**</td><td>[Pokyny k instalaci Python SDK](http://azure.github.io/azure-documentdb-python/)</td></tr>

<tr><td>**Přispívat tooSDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-python)</td></tr>

<tr><td>**Začínáme**</td><td>[Začínáme s hello Python SDK](documentdb-python-application.md)</td></tr>

<tr><td>**Aktuální podporované platformy**</td><td>[Python 2.7](https://www.python.org/downloads/) a [Python 3.5](https://www.python.org/downloads/)</td></tr>
</table></br>

## <a name="release-notes"></a>Poznámky k verzi
### <a name="a-name220220"></a><a name="2.2.0"/>2.2.0
* Přidaná podpora pro novou úroveň konzistence volá ConsistentPrefix.


### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0
* Přidaná podpora pro dotazy agregace (COUNT, MIN, MAX, součet a průměr).
* Přidat možnost pro zákaz protokolu SSL ověření při spuštění emulátoru DB Cosmos.
* Odebrat závislé požadavky modulu toobe přesně 2.10.0 hello omezením.
* Snížena minimální propustnosti na dělené kolekce z 10,100 RU/s too2500 RU/s.
* Přidaná podpora pro povolení protokolování skriptu během provádění uložené procedury.
* Verze rozhraní REST API bumped příliš ' 2017-01-19' v této verzi.

### <a name="a-name201201"></a><a name="2.0.1"/>2.0.1
* Redakční změny provedené toodocumentation komentáře.

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0
* Přidaná podpora pro Python 3.5.
* Přidaná podpora pro sdružování připojení pomocí modulu požadavky.
* Přidaná podpora pro konzistence typu relace.
* Byla přidána podpora pro dotazy na nejvyšší nebo ORDERBY pro dělené kolekce.

### <a name="a-name190190"></a><a name="1.9.0"/>1.9.0
* Podpora zásad přidané opakování omezenému požadavky. (Omezenému požadavky obdrží žádost o míra příliš velký výjimka, kód chyby 429.) Ve výchozím nastavení Azure Cosmos DB opakuje devětkrát pro každý požadavek vyskytne kód chyby 429, aby byla dodržena hello retryAfter čas v hlavičku odpovědi hello. Časový interval opakování pevné lze nyní nastavit jako součást hello RetryOptions vlastnost u objektu ConnectionPolicy hello Pokud chcete, aby tooignore hello retryAfter čas vrácená serverem mezi opakovanými pokusy hello. Azure Cosmos DB nyní čeká maximálně 30 sekund pro každý požadavek, který je omezené (bez ohledu na počet opakování) a vrátí odpověď hello s kódem chyby 429. Tento čas může být také elementem v hello RetryOptions vlastnost ConnectionPolicy objektu.
* Cosmos DB nyní vrátí x-ms omezení--počet opakování a x-ms-throttle-retry-wait-time-ms jako hello hlavičky odpovědi v každé žádosti toodenote hello omezení počtu a hello cummulative čas požadavku hello čekali mezi opakovanými pokusy hello zkuste provést znovu.
* Odebrané hello RetryPolicy třídy a odpovídající vlastnosti hello (retry_policy) vystavený pro třídu document_client hello a místo toho zavedl třídu RetryOptions vystavení hello RetryOptions vlastnost ConnectionPolicy třídu, která lze použít toooverride Některé hello výchozí možnosti opakování.

### <a name="a-name180180"></a><a name="1.8.0"/>1.8.0
* Podpora přidání hello účty databáze více oblast.

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
* Přidání hello podporu pro funkci tooLive(TTL) čas pro dokumenty.

### <a name="a-name161161"></a><a name="1.6.1"/>1.6.1
* Opravy chyb souvisejících s tooserver straně dělení tooallow speciální znaky v cestě klíč oddílu.

### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0
* Implementovat [oddíly kolekce](partition-data.md) a [úrovně výkonu uživatelem definované](performance-levels.md). 

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0
* Přidejte hodnotu Hash p & ro tooassist překladače oddílu s horizontálního dělení aplikacemi v rámci více oddílů.

### <a name="a-name142142"></a><a name="1.4.2"/>1.4.2
* Implementujte Upsert. Přidat nové metody UpsertXXX toosupport Upsert funkce.
* Implementujte, na základě ID směrování. Žádné změny veřejné rozhraní API, všechny změny interní.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Podporuje geoprostorové index.
* Ověří vlastnost id pro všechny prostředky. Identifikátory prostředků nesmí obsahovat?, /, #, \, znaků nebo končit mezerou.
* Přidá nový tooResourceResponse "průběh transformace index" záhlaví.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Implementuje zásady indexování V2.

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1
* Podporuje pro připojení proxy serveru.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA SDK.

## <a name="release--retirement-dates"></a>Verze & vyřazení kalendářních dat
Microsoft bude poskytovat oznámení alespoň **dobu 12 měsíců** před vyřazením z provozu v pořadí toosmooth hello přechod tooa novější nebo v nepodporované verzi sady SDK.

Nové funkce a funkce a optimalizace, jsou přidány pouze aktuální toohello SDK a jako takový je, že jste vždy upgradu toohello nejnovější verze sady SDK, abyste co nejdříve. 

Všechny žádosti o tooCosmos DB pomocí vyřazeno sady SDK budou odmítnuty službou hello.

> [!WARNING]
> Všechny verze hello Azure DocumentDB SDK pro Python předchozí tooversion **1.0.0** vyřadí na **29. února 2016**. 
> 
> 

<br/>

| Verze | Datum vydání | Datum vyřazení |
| --- | --- | --- |
| [2.2.0](#2.2.0) |10. května 2017 |--- |
| [2.1.0](#2.1.0) |01 může 2017 |--- |
| [2.0.1](#2.0.1) |30. října 2016 |--- |
| [2.0.0](#2.0.0) |29. září 2016 |--- |
| [1.9.0](#1.9.0) |07 července 2016 |--- |
| [1.8.0](#1.8.0) |14. června 2016 |--- |
| [1.7.0](#1.7.0) |26. dubna 2016 |--- |
| [1.6.1](#1.6.1) |08. dubna 2016 |--- |
| [1.6.0](#1.6.0) |29. března 2016 |--- |
| [1.5.0](#1.5.0) |03 leden 2016 |--- |
| [1.4.2](#1.4.2) |06 říjen 2015 |--- |
| [1.4.1](#1.4.1) |06 říjen 2015 |--- |
| [1.2.0](#1.2.0) |06 srpen 2015 |--- |
| [1.1.0](#1.1.0) |09 července 2015 |--- |
| [1.0.1](#1.0.1) |25 květen 2015 |--- |
| [1.0.0](#1.0.0) |07 duben 2015 |--- |
| 0.9.4-prelease |14 leden 2015 |29. února 2016 |
| 0.9.3-prelease |09 prosince 2014 |29. února 2016 |
| 0.9.2-prelease |25 listopadu 2014 |29. února 2016 |
| 0.9.1-prelease |23. září 2014 |29. února 2016 |
| 0.9.0-prelease |21 srpen 2014 |29. února 2016 |

## <a name="faq"></a>Nejčastější dotazy
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Viz také
toolearn Další informace o Cosmos databáze, najdete v části [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) stránku služby. 

