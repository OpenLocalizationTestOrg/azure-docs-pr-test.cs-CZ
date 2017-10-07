---
title: "aaaAzure Cosmos DB nejčastější dotazy | Microsoft Docs"
description: "Získejte odpovědi toofrequently kladené dotazy týkající se Azure Cosmos databáze, databáze globálně distribuované, více modelu služby. Další informace o kapacitě a úrovně výkonu a škálování."
keywords: "Otázky k databázi, často kladené otázky, documentdb, azure, Microsoft azure"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: b68d1831-35f9-443d-a0ac-dad0c89f245b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: mimig
ms.openlocfilehash: 59e047d9acd8ac05facc96655747d7495a45317a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-faq"></a>Nejčastější dotazy k Azure Cosmos DB
## <a name="azure-cosmos-db-fundamentals"></a>Základy Azure Cosmos DB
### <a name="what-is-azure-cosmos-db"></a>Co je Azure Cosmos DB?
Azure Cosmos DB je globálně replikované databáze, více modelu služba, která nabízí bohaté dotazování přes data bez schémat, pomáhá poskytovat Konfigurovatelný a spolehlivý výkon a umožňuje rychlý vývoj. Ho všechny dosáhnete přes spravovanou platformu, který je zálohovaný díky hello power a dosahem Microsoft Azure. 

Azure Cosmos DB je hello správným řešením pro webové, mobilní a herní, a aplikace či aplikace IoT při předvídatelnou propustnost, vysokou dostupnost, nízkou latenci a bez schémat datového modelu jsou klíčové požadavky. Poskytuje flexibilitu schémat a bohaté indexování a zahrnuje podporu transakcí několika dokumentů s integrovaným JavaScriptem. 

Další databáze otázky, odpovědi a pokyny pro nasazení a používání této služby najdete v tématu hello [stránce dokumentace Azure Cosmos DB](https://azure.microsoft.com/documentation/services/cosmos-db/).

### <a name="what-happened-toodocumentdb"></a>Jaké happened tooDocumentDB?
Hello rozhraní API DocumentDB je jedním z hello podporované rozhraní API a datové modely pro Azure Cosmos DB. Kromě toho Azure Cosmos DB podporuje můžete pomocí rozhraní Graph API (Preview), rozhraní API tabulky (Preview) a rozhraní API MongoDB. Další informace najdete v tématu [dotazy zákazníků DocumentDB](#moving-to-cosmos-db).

### <a name="how-do-i-get-toomy-documentdb-account-in-hello-azure-portal"></a>Jak získat toomy účet DocumentDB v hello portál Azure?
V hello portálu Azure klikněte na ikonu Azure Cosmos DB hello v levém podokně hello. Pokud jste měli účet DocumentDB před, nyní máte účet Azure Cosmos DB s fakturace tooyour žádné změny.

### <a name="what-are-hello-typical-use-cases-for-azure-cosmos-db"></a>Jaké jsou hello typické případy použití pro databázi Cosmos Azure?
Azure Cosmos DB je dobrou volbou pro nové webové, mobilní a herní, a je důležité, aplikace či aplikace IoT automatické škálování, předvídatelný výkon, rychlé pořadí doby odezvy v milisekundách kde hello možnost tooquery přes data bez schémat. Azure Cosmos DB různě konfigurovat toorapid vývoji a podpoře hello nepřetržitých iterací modelů dat aplikace. Aplikace, které spravují uživatelem generovaný obsah a data jsou [běžné případy použití pro Azure Cosmos DB](use-cases.md). 

### <a name="how-does-azure-cosmos-db-offer-predictable-performance"></a>Jak Azure Cosmos DB nabízí předvídatelný výkon?
A [jednotek žádosti](request-units.md) (RU) je hello mírou propustnosti v Azure Cosmos DB. Propustnost 1 RU odpovídá propustnosti toohello hello GET u 1 KB dokumentu. Všechny operace v Azure DB Cosmos, včetně čtení, zápisu, dotazů SQL a spuštění uložených procedur, má deterministickou RU hodnotu, která je založena na hello propustnosti vyžaduje toocomplete hello operaci. Namísto přemýšlení o procesoru, vstupně-výstupní operace a paměti a jak každá ovlivňují propustnost aplikace, si můžete představit jako jednu míru RU.

Je možné rezervovat každý kontejner Azure Cosmos DB zajištěnou propustností z hlediska RUs propustnost za sekundu. Pro aplikace jakéhokoli rozsahu můžete srovnávací test jednotlivých požadavků toomeasure jejich hodnoty RU a zřídit celkem hello toohandle kontejneru jednotek žádosti ze všech požadavků. Můžete taky škálovat nebo snižovat propustnost vaší kontejneru jako hello potřebám vaší aplikace měnícím. Další informace o jednotkách žádosti a nápovědu pro určení vašeho kontejneru potřebuje, najdete v části [odhadnout požadavky propustnost](request-units.md#estimating-throughput-needs) a zkuste to hello [propustnost kalkulačky](https://www.documentdb.com/capacityplanner). termín Hello *kontejneru* zde označují toorefers tooa DocumentDB API kolekce, grafu rozhraní Graph API, rozhraní API MongoDB kolekce a tabulka API tabulky. 

### <a name="how-does-azure-cosmos-db-support-various-data-models-such-as-keyvalue-columnar-document-and-graph"></a>Jak Azure Cosmos DB podporuje různé datové modely, například klíč/hodnota, sloupcové, dokument a graf?

Klíč hodnota (tabulky), sloupcové, dokumentů a data grafu modely jsou všechny nativně podporované kvůli ARS hello (atomů, záznamy a pořadí) návrhu tohoto Azure DB Cosmos je založený na. Atomů, záznamy a pořadí lze snadno namapované a předpokládané toovarious datové modely. Hello rozhraní API pro podmnožinu modely jsou k dispozici pravé nyní (DocumentDB, MongoDB, tabulky a rozhraní Graph API) a ostatní konkrétní tooadditional datové modely, budou k dispozici v budoucnu hello.

Azure Cosmos DB má schéma lhostejné indexování modul schopná automaticky indexování všechny hello data, která ho ingestuje bez nutnosti žádné schéma nebo sekundární indexy z hello developer. modul Hello spoléhá na sadu rozvržení logické indexu (obráceným, sloupcové, stromu), které oddělit hello rozložení úložiště z hello index a zpracování subsystémů dotazů. Cosmos DB také obsahuje sadu přenosu protokolů a rozhraní API toosupport možnost hello extensible způsobem a překládat je efektivně toohello základní data modelu (1) hello logické index rozložení a (2) díky tomu jednoznačně schopný zajistit podporu více datové modely nativně .

### <a name="is-azure-cosmos-db-hipaa-compliant"></a>Je kompatibilní s Azure Cosmos DB HIPAA?
Ano, je kompatibilní se standardem HIPAA Azure Cosmos DB. HIPAA zavádí požadavky pro hello použití, zveřejnění a ochranu jednotlivě identifikovatelných zdravotních informací. Další informace najdete v tématu hello [Microsoft Trust Center](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### <a name="what-are-hello-storage-limits-of-azure-cosmos-db"></a>Jaká jsou omezení úložiště hello Azure Cosmos DB?
Neexistuje žádné omezení toohello celkovou velikost dat, která kontejner můžete pojmout v Azure Cosmos DB.

### <a name="what-are-hello-throughput-limits-of-azure-cosmos-db"></a>Jaká jsou omezení propustnosti hello Azure Cosmos DB?
Neexistuje žádné omezení toohello celkovou velikost propustnosti, kterou kontejner může podporovat v Azure Cosmos DB. Hello klíče je vhodné je toodistribute vaši úlohu přibližně rovnoměrně mezi dostatečně velký počet klíčů oddílů.

### <a name="how-much-does-azure-cosmos-db-cost"></a>Kolik Azure Cosmos DB stojí?
Podrobnosti najdete toohello [podrobnosti o cenách Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/) stránky. Poplatky za používání služby Azure Cosmos DB jsou určeny hello počet zřízené kontejnerů, hello počet hodin hello kontejnery byly online a hello zřízené propustnosti pro každý kontejner. termín Hello *kontejnery* zde označují kolekce DocumentDB API toohello, grafu rozhraní Graph API, rozhraní API MongoDB kolekce a tabulka API tabulky. 

### <a name="is-a-free-account-available"></a>Je k dispozici bezplatný účet?
Pokud jste nový tooAzure, můžete si zaregistrovat [bezplatný účet Azure](https://azure.microsoft.com/free/), kterým získáte 30 dnů a a všechny tootry platební hello služby Azure. Pokud máte předplatné sady Visual Studio, jste také vhodné pro [volné kredity Azure](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) toouse pro libovolnou službu Azure. 

Můžete taky hello [emulátoru DB Cosmos Azure](local-emulator.md) toodevelop a testování vaší aplikace místně pro free, bez vytváření předplatného Azure. Až budete spokojeni s jak aplikace funguje v hello emulátoru DB Cosmos Azure, můžete přepnout toousing účet Azure Cosmos DB v cloudu hello.

### <a name="how-can-i-get-additional-help-with-azure-cosmos-db"></a>Jak můžete získat další pomoc s Azure Cosmos DB?
Pokud potřebujete pomoc, oslovení toous na [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-cosmosdb) nebo hello [fórum MSDN](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=AzureDocumentDB), nebo naplánovat očima chat s technickému týmu Azure Cosmos DB hello zasláním e-mailu příliš[ askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com). 

## <a name="set-up-azure-cosmos-db"></a>Nastavení Azure Cosmos DB
### <a name="how-do-i-sign-up-for-azure-cosmos-db"></a>Jak přihlásím Azure Cosmos DB?
Azure Cosmos DB je k dispozici v hello portálu Azure. První zaregistrujte si předplatné Azure. Poté, co jste se přihlásili, můžete přidat rozhraní API DocumentDB, rozhraní Graph API (Preview), rozhraní API tabulky rozhraní API (Preview) nebo MongoDB účet tooyour předplatného Azure.

### <a name="what-is-a-master-key"></a>Co je hlavní klíč?
Hlavní klíč je token tooaccess zabezpečení všechny prostředky pro účet. Uživatelé, kteří mají klíč hello mít oprávnění ke čtení a zápisu tooall prostředky v účtu databáze hello. Při distribuci hlavního klíče buďte opatrní. Hello primární hlavní klíč nebo sekundární hlavní klíč jsou k dispozici na hello **klíče** okno hello [portál Azure][azure-portal]. Další informace o klíčích najdete v tématu [zobrazení, kopírování a opětovné vytváření přístupových klíčů](manage-account.md#keys).

### <a name="what-are-hello-regions-that-preferredlocations-can-be-set-to"></a>Jaké jsou hello oblasti, které může být nastavena PreferredLocations? 
Hello PreferredLocations hodnotu lze nastavit tooany hello Azure oblasti, ve kterých je k dispozici Cosmos DB. Seznam dostupných oblastí najdete v tématu [oblastí Azure](https://azure.microsoft.com/regions/).

### <a name="is-there-anything-i-should-be-aware-of-when-distributing-data-across-hello-world-via-hello-azure-datacenters"></a>Je všechno, co bych vědět, když rozděluje data mezi hello, world prostřednictvím hello datových centrech Azure? 
Azure Cosmos DB nachází přes všechny oblasti Azure, jako zadaný v hello [oblastí Azure](https://azure.microsoft.com/regions/) stránky. Vzhledem k tomu, že je hello základní služby, má každý nový datacenter Azure Cosmos DB přítomnosti. 

Když nastavíte oblast, mějte na paměti, že Azure Cosmos DB respektuje suverénní a government cloudy. To znamená pokud vytvoříte účet v svrchovaných oblasti, nelze replikovat mimo danou svrchovaných oblast. Podobně nelze povolit replikaci do jiných umístění svrchovaných z mimo účtu. 

## <a name="develop-against-hello-documentdb-api"></a>Vývoj proti hello DocumentDB rozhraní API

### <a name="how-do-i-start-developing-against-hello-documentdb-api"></a>Jak spustit vývoj proti hello rozhraní API DocumentDB?
Rozhraní API služby Microsoft DocumentDB je k dispozici v hello [portál Azure][azure-portal]. Nejprve musíte zaregistrovat předplatné Azure. Jakmile si zaregistrujete předplatné Azure, můžete přidat rozhraní API DocumentDB kontejneru tooyour předplatného Azure. Pokyny k přidání účtu Azure Cosmos DB najdete v tématu [vytvoření účtu Azure Cosmos DB databáze](create-documentdb-dotnet.md#create-account). Pokud jste měli účet DocumentDB v posledních hello, máte nyní účet Azure Cosmos DB. 

Pro .NET, Python, Node.js, JavaScript a Javu jsou k dispozici sady [SDK](documentdb-sdk-dotnet.md). Vývojáři mohou použít také hello [RESTful HTTP API](/rest/api/documentdb/) toointeract s prostředky Azure Cosmos DB z různých platforem a jazyků.

### <a name="can-i-access-some-ready-made-samples-tooget-a-head-start"></a>Můžete získat přístup některé připravených ukázky tooget začít?
Ukázky pro hello rozhraní API DocumentDB [.NET](documentdb-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md), a [Python](documentdb-python-samples.md) sady SDK jsou k dispozici na Githubu.


### <a name="does-hello-documentdb-api-database-support-schema-free-data"></a>Podporuje hello data bez schémat podporu rozhraní API DocumentDB databáze?
Ano, hello rozhraní API DocumentDB umožňuje aplikacím toostore libovolné dokumenty JSON bez schématu definic nebo parametrů. Data jsou okamžitě k dispozici pro dotaz přes rozhraní dotazů SQL databáze Azure Cosmos hello.  

### <a name="does-hello-documentdb-api-support-acid-transactions"></a>Podporuje hello rozhraní API DocumentDB transakce ACID?
Ano, hello rozhraní API DocumentDB podporuje transakce mezi dokumenty vyjádřené jako JavaScript uložených procedur a aktivačních událostí. Transakce jsou obor tooa v každé kolekci jednoho oddílu a spouštěny sémantice ACID jako "všechny nebo nic," izolované od ostatních souběžně spuštěných požadavků kód a uživatele. Pokud jsou výjimky vyvolány prostřednictvím hello serverové spouštění kódu aplikace JavaScript, hello celá transakce bude vrácena zpět. Další informace o transakcích najdete v tématu [databáze programu transakce](programming.md#database-program-transactions).

### <a name="what-is-a-collection"></a>Co je kolekce?
Kolekce je skupina dokumentů a jejich přidružené logiky Javascriptové aplikace. Kolekce je fakturovatelná entita, kde hello [náklady](performance-levels.md) je dáno hello propustnost a použít úložiště. Kolekce může mít rozsah jeden nebo více oddílů nebo serverů a lze je škálovat toohandle prakticky neomezené objemy úložišť a propustnosti.

Kolekce jsou také entitami fakturace hello pro Azure Cosmos DB. Každá kolekce se fakturuje každou hodinu, založená na hello zřízené propustnosti a využitého prostoru úložiště. Další informace najdete v tématu [Azure Cosmos DB ceny](https://azure.microsoft.com/pricing/details/cosmos-db/). 

### <a name="how-do-i-create-a-database"></a>Jak vytvořím databázi?
Databáze můžete vytvořit pomocí hello [portál Azure](https://portal.azure.com), jak je popsáno v [přidat kolekci](create-documentdb-dotnet.md#create-collection), jeden z hello [SDK služby Azure Cosmos DB](documentdb-sdk-dotnet.md), nebo hello [rozhraní REST API](/rest/api/documentdb/). 

### <a name="how-do-i-set-up-users-and-permissions"></a>Jak nastavím uživatele a oprávnění?
Uživatele a oprávnění můžete vytvořit pomocí jedné z hello [Cosmos DB rozhraní API sady SDK](documentdb-sdk-dotnet.md) nebo hello [rozhraní REST API](/rest/api/documentdb/).  

### <a name="does-hello-documentdb-api-support-sql"></a>Podporuje hello rozhraní API DocumentDB SQL?
Hello dotazovací jazyk SQL je vylepšená podmnožina dotazovacích funkcí hello podporovaný SQL. Hello dotazovací jazyk Azure Cosmos DB SQL nabízí bohaté hierarchické a relační operátory a rozšiřitelnost prostřednictvím bázi jazyka JavaScript, uživatelsky definované funkce (UDF). Gramatika JSON umožňuje modelování dokumentů JSON ve formě stromů ve s popiskem uzly, které jsou používány hello Azure Cosmos DB automatických technikách indexování a hello dialektu dotazování SQL v Azure Cosmos DB. Informace o používání gramatiky SQL najdete v tématu hello [QueryDocumentDB] [ query] článku.

### <a name="does-hello-documentdb-api-support-sql-aggregation-functions"></a>Podporuje hello funkce agregace SQL podporu rozhraní API DocumentDB?
Hello rozhraní API DocumentDB podporuje agregace s nízkou latencí v jakémkoli měřítku prostřednictvím agregační funkce `COUNT`, `MIN`, `MAX`, `AVG`, a `SUM` prostřednictvím hello gramatiky SQL. Další informace najdete v tématu [agregační funkce](documentdb-sql-query.md#Aggregates).

### <a name="how-does-hello-documentdb-api-provide-concurrency"></a>Jak hello rozhraní API DocumentDB zajišťuje souběžnost?
Hello rozhraní API DocumentDB podporuje optimistické řízení souběžného (přístupu OCC) prostřednictvím značek entit HTTP, neboli Etagů. Každý prostředek DocumentDB API má ETag a hello značka ETag je nastavený na hello server pokaždé, když je aktualizován dokument. všechny zprávy odpovědi jsou součástí hlavičku ETag Hello a hello aktuální hodnotu. Značky etag binárním rozsáhlým lze použít s hello If-Match záhlaví tooallow hello serveru toodecide, zda by měly být aktualizovány prostředku. Hello hodnota If-Match je hello ETag hodnota toobe kontrolovat. Pokud hello hodnota ETag odpovídá server hello hodnota ETag, hello prostředků se aktualizuje. Pokud již není hello ETag aktuální, hello server odmítne hello operaci s "HTTP 412 Precondition selhání" kód odpovědi. Hello klienta pak znovu načte hello prostředků tooacquire hello aktuální hodnota ETag pro prostředek hello. Kromě toho značky etag binárním rozsáhlým lze použít s toodetermine hlavička If-None-Match hello, jestli je potřeba znovu načtěte prostředku.

toouse optimistickou metodu souběžného v rozhraní .NET, použijte hello [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) třídy. Ukázku .NET naleznete v části [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) v hello DocumentManagement ukázce na Githubu.

### <a name="how-do-i-perform-transactions-in-hello-documentdb-api"></a>Jak se provádí transakce v hello rozhraní API DocumentDB?
Hello rozhraní API DocumentDB podporuje transakce integrované do jazyka prostřednictvím JavaScript uložených procedur a aktivačních událostí. Všechny databázové operace ve skriptech se spouští v izolaci snímku. Pokud je kolekci s jedním oddílem, provádění hello je kolekce vymezená toohello. Pokud hello kolekce rozdělena na oddíly, provádění hello je vymezená toodocuments s hello stejnou hodnotu klíč oddílu v kolekci hello. Snímek hello verzí dokumentů (Etagy) je provést při spuštění hello hello transakce a potvrzené pouze v případě, že hello skript uspěje. Pokud hello JavaScript vyvolá chybu, hello transakce je vrácena zpět. Další informace najdete v tématu [programování v jazyce JavaScript na straně serveru pro databázi Azure Cosmos](programming.md).

### <a name="how-can-i-bulk-insert-documents-into-cosmos-db"></a>Jak můžete I hromadného vložení dokumenty do Cosmos DB?
Vám může hromadného vložení dokumenty do Azure Cosmos DB v některém ze dvou způsobů:

* Hello nástroj pro migraci dat, jak je popsáno v [nástroj pro migraci databáze pro databázi Azure Cosmos](import-data.md).
* Uložené procedury, jak je popsáno v [programování v jazyce JavaScript na straně serveru pro databázi Azure Cosmos](programming.md).

### <a name="does-hello-documentdb-api-support-resource-link-caching"></a>Podporuje hello rozhraní API DocumentDB ukládání do mezipaměti odkaz prostředků?
Ano, protože Azure Cosmos DB je služba RESTful, odkazy na zdroje jsou neměnné a mohou být uloženy v mezipaměti. Klienti DocumentDB API můžete zadat hlavičku "If-None-Match" pro čtení pro všechny prostředků jako dokumentu nebo kolekci a pak aktualizujte své místní kopie po změně verze serveru hello.

### <a name="is-a-local-instance-of-documentdb-api-available"></a>Je k dispozici místní instanci rozhraní API DocumentDB?
Ano. Hello [emulátoru DB Cosmos Azure](local-emulator.md) poskytuje zachováním emulace Dobrý den služby Cosmos DB. Podporuje funkce, které jsou identické tooAzure Cosmos databáze, včetně podpory pro vytváření a dotazování dokumentů JSON, zřizování a škálování kolekce a provádění uložené procedury a triggery. Můžete vyvíjet a testovat aplikace pomocí hello emulátoru DB Cosmos Azure a nasadit je tooAzure v globálním měřítku tím, že konfigurací jedné změnit toohello koncového bodu připojení pro Azure Cosmos DB.

## <a name="develop-against-hello-api-for-mongodb"></a>Vývoj proti hello rozhraní API pro MongoDB
### <a name="what-is-hello-azure-cosmos-db-api-for-mongodb"></a>Co je hello rozhraní API služby Azure Cosmos DB pro MongoDB?
Hello rozhraní API služby Azure Cosmos DB pro MongoDB je vrstvu kompatibility, která umožňuje aplikacím tooeasily a transparentně komunikovat s hello nativní Azure Cosmos DB databázový stroj pomocí existující, podporované komunity Apache MongoDB rozhraní API a ovladačů. Vývojáři teď můžete použít existující MongoDB nástroj řetězy dovednosti a znalosti toobuild aplikace, které využít výhod Azure Cosmos DB. Vývojáři těžit z hello jedinečné schopnosti Azure Cosmos DB, mezi které patří automatické indexování, zálohování údržby, smlouvy o úrovni finančně zálohovány služeb (SLA) a tak dále.

### <a name="how-do-i-connect-toomy-api-for-mongodb-database"></a>Postup připojení toomy rozhraní API pro databázi MongoDB?
Hello nejrychlejší způsob, jak tooconnect toohello rozhraní API služby Azure Cosmos DB pro MongoDB je toohead přes toohello [portál Azure](https://portal.azure.com). Přejděte tooyour účtu a klikněte v levém navigačním nabídce hello **rychlý Start**. Rychlý Start je hello nejlepší způsob, jak tooget kód fragmenty tooconnect tooyour databáze. 

Azure Cosmos DB vynucuje vysokými nároky na zabezpečení a standardy. Účty Azure Cosmos DB vyžadovat ověřování a zabezpečenou komunikaci prostřednictvím protokolu SSL, takže je zda toouse TLSv1.2.

Další informace najdete v tématu [připojit tooyour rozhraní API pro databázi MongoDB](connect-mongodb-account.md).

### <a name="are-there-additional-error-codes-for-an-api-for-mongodb-database"></a>Existují další kódy chyb pro rozhraní API pro databázi MongoDB?
Hello MongoDB rozhraní API v přidání toohello běžné MongoDB kódy chyb, má svou vlastní specifické chybové kódy:


| Chyba               | Kód  | Popis  | Řešení  |
|---------------------|-------|--------------|-----------|
| TooManyRequests     | 16500 | Celkový počet jednotek žádosti použití Hello překročilo četnost hello zřízené požadavek jednotky pro kolekci hello a byla omezena. | Zvažte škálování hello propustnost kolekce hello z hello portál Azure nebo opakováním. |
| ExceededMemoryLimit | 16501 | Jako víceklientské služby překročil hello operaci přidělení paměti hello klienta. | Snížit rozsah hello hello operace prostřednictvím konkrétnější kritéria dotazu nebo se obraťte na podporu hello [portál Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). <br><br>Příklad:  *&nbsp; &nbsp; &nbsp; &nbsp;db.getCollection('users').aggregate ([<br> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;{$match: {název: "Andy"}}, <br> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;{$sort: {stáří: -1} }<br>&nbsp;&nbsp;&nbsp;&nbsp;])*) |

## <a name="develop-with-hello-table-api-preview"></a>Vývoj s hello tabulky rozhraní API (Preview)

### <a name="terms"></a>Výrazy 
Hello Azure Cosmos DB: tabulka rozhraní API (Preview) odkazuje toohello premium nabídky pomocí Azure Cosmos DB podpora tabulky oznamují na 2017 sestavení. 

Standardní tabulka Hello SDK je hello existující tabulku úložiště Azure SDK. 

### <a name="how-can-i-use-hello-new-table-api-preview-offering"></a>Jak můžete použít hello novou nabídku tabulky rozhraní API (Preview)? 
Hello rozhraní API služby Azure Cosmos DB tabulky je k dispozici v hello [portál Azure][azure-portal]. Nejprve musíte zaregistrovat předplatné Azure. Poté, co jste se přihlásili, můžete přidat rozhraní API služby Azure Cosmos DB tabulky účet tooyour předplatného Azure a pak přidejte účet tooyour tabulky. 

Během období preview hello Pokud [sady SDK](../cosmos-db/table-sdk-dotnet.md) jsou k dispozici pro rozhraní .NET, můžete začít používat provedením hello [tabulky API](../cosmos-db/create-table-dotnet.md) úvodní článek.

### <a name="do-i-need-a-new-sdk-toouse-hello-table-api-preview"></a>Je nutné nové hello toouse SDK API tabulky (Preview)? 
Ano, hello [tabulku úložiště Premium Windows Azure (Preview) SDK](https://www.nuget.org/packages/WindowsAzure.Storage-PremiumTable) je k dispozici na NuGet. Další informace jsou k dispozici na hello [Azure Cosmos DB tabulky .NET API: stažení a poznámky k verzi](https://github.com/Microsoft/azure-docs-pr/cosmos-db/table-sdk-dotnet.md) stránky. 

### <a name="how-do-i-provide-feedback-about-hello-sdk-or-bugs"></a>Jak poskytují zpětnou vazbu o hello SDK nebo oznámení chyb?
Můžete sdílet svůj názor v některém z následujících způsobů hello:

* [Uservoice](https://feedback.azure.com/forums/599062-azure-cosmos-db-table-api)
* [Fórum MSDN](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=AzureDocumentDB)
* [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-cosmosdb)

### <a name="what-is-hello-connection-string-that-i-need-toouse-tooconnect-toohello-table-api-preview"></a>Co je hello připojovací řetězec, potřebuji toouse tooconnect toohello tabulky rozhraní API (Preview)?
Hello připojovací řetězec je:
```
DefaultEndpointsProtocol=https;AccountName=<AccountNamefromCosmos DB;AccountKey=<FromKeysPaneofCosmosDB>;TableEndpoint=https://<AccountNameFromDocumentDB>.documents.azure.com
```
Ze stránky klíče hello hello portálu Azure můžete získat hello připojovací řetězec. 

### <a name="how-do-i-override-hello-config-settings-for-hello-request-options-in-hello-new-table-api-preview"></a>Jak přepsat nastavení souboru config hello hello požadavek možnosti v nové tabulce API hello (Preview)?
Informace o nastavení konfigurace najdete v tématu [možnosti Azure Cosmos DB](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). Hello nastavení můžete změnit přidáním tooapp.config v hello appSettings v aplikaci klienta hello.

    <appSettings>
        <add key="TableConsistencyLevel" value="Eventual|Strong|Session|BoundedStaleness|ConsistentPrefix"/>
        <add key="TableThroughput" value="<PositiveIntegerValue"/>
        <add key="TableIndexingPolicy" value="<jsonindexdefn>"/>
        <add key="TableUseGatewayMode" value="True|False"/>
        <add key="TablePreferredLocations" value="Location1|Location2|Location3|Location4>"/>....
    </appSettings>


### <a name="are-there-any-changes-for-customers-who-are-using-hello-existing-standard-table-sdk"></a>Jsou pro zákazníky, kteří používají hello existující standardní tabulky SDK všechny změny?
Žádné. Neexistují žádné změny pro existující nebo nové zákazníky, kteří používají hello existující standardní tabulky SDK. 

### <a name="how-do-i-view-table-data-that-is-stored-in-azure-cosmos-db-for-use-with-hello-table-api-review"></a>Zobrazení tabulky data, která je uložená v Azure Cosmos DB pro použití s hello tabulky rozhraní API (review) 
Můžete použít hello Azure portálu toobrowse hello data. Můžete také použít hello kódu rozhraní API tabulky (Preview) nebo uvedené v další odpovědí hello nástroje hello. 

### <a name="which-tools-work-with-hello-table-api-preview"></a>Které nástroje pro práci s hello tabulky rozhraní API (Preview)? 
Můžete použít hello starší verze aplikace Azure Explorer (0.8.9).

Nástroje s tootake hello flexibilitu, které může podporovat dříve připojovací řetězec ve formátu hello zadaný hello nové tabulky rozhraní API (Preview). Seznam nástrojů tabulce je uvedený na hello [Azure Storage Client Tools](../storage/common/storage-explorers.md) stránky. 

### <a name="do-powershell-or-azure-cli-work-with-hello-new-table-api-preview"></a>Prostředí PowerShell nebo rozhraní příkazového řádku Azure fungují s hello nové tabulky rozhraní API (Preview)?
Plánujeme podporu tooadd pro prostředí PowerShell a rozhraní příkazového řádku Azure pro tabulku rozhraní API (Preview). 

### <a name="is-hello-concurrency-on-operations-controlled"></a>Je hello souběžnosti na operace řízené?
Ano, optimistickou metodu souběžného je k dispozici prostřednictvím použití hello mechanismus hello značka ETag. 

### <a name="is-hello-odata-query-model-supported-for-entities"></a>Se hello model dotazů OData podporuje pro entity? 
Ano, podporuje hello tabulky rozhraní API (Preview) dotazu OData a dotazů LINQ. 

### <a name="can-i-connect-toohello-standard-azure-table-and-hello-new-premium-table-api-preview-side-by-side-in-hello-same-application"></a>Můžete připojit toohello standardní tabulky Azure a hello nové premium tabulky rozhraní API (Preview) vedle sebe v hello stejnou aplikaci? 
Ano, se můžete připojit pomocí vytváření dvě samostatné instance hello CloudTableClient, každý odkazující tooits vlastní identifikátor URI prostřednictvím hello připojovací řetězec.

### <a name="how-do-i-migrate-an-existing-azure-table-storage-application-toothis-new-offering"></a>Jak mohu migrovat existující Azure Table storage aplikace toothis nové nabídky?
tootake výhod hello nová tabulka API nabídka na stávající úložiště dat v tabulce, obraťte se na [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com). 

### <a name="what-is-hello-roadmap-for-this-service-and-when-will-you-offer-other-standard-table-api-functionality"></a>Co je hello plán pro tuto službu a při vám nabídne další standardní funkce rozhraní API tabulky?
Plánujeme tooadd Podpora SAS tokeny, ServiceContext, statistiky, klienta straně šifrování, analýzu a další funkce, jako jsme pokračovat směrem k Všeobecné Vám může vaše názory na [Uservoice](https://feedback.azure.com/forums/599062-azure-cosmos-db-table-api). 

### <a name="how-is-expansion-of-hello-storage-size-done-for-this-service-if-for-example-i-start-with-n-gb-of-data-and-my-data-will-grow-too1-tb-over-time"></a>Jak je rozšíření velikost úložiště hello provést pro tuto službu, je-li například spustit s  *n*  GB dat a data se zvýší too1 TB časem? 
Azure Cosmos DB je navrženou tooprovide neomezené úložiště prostřednictvím použití hello vodorovné škálování. Hello služby můžete sledovat a efektivně zvýšit úložiště. 

### <a name="how-do-i-monitor-hello-table-api-preview-offering"></a>Jak se monitorování nabídky hello tabulky rozhraní API (Preview)?
Můžete použít hello tabulky rozhraní API (Preview) **metriky** žádostí podokně toomonitor a využití úložiště. 

### <a name="how-do-i-calculate-hello-throughput-i-require"></a>Jak vypočítat hello propustnost, které můžu vyžadovat?
Můžete použít hello kapacity odhadu toocalculate hello TableThroughput, které je nutné pro operace hello. Další informace najdete v tématu [jednotky žádosti odhad a úložiště dat](https://www.documentdb.com/capacityplanner). Obecně platí můžete představují vaší entity jako JSON a zadejte hello čísla pro operace. 

### <a name="can-i-use-hello-new-table-api-preview-sdk-locally-with-hello-emulator"></a>Můžete použít hello nové tabulky rozhraní API (Preview) SDK místně pomocí emulátoru hello?
Ano, můžete použít hello tabulky rozhraní API (Preview) s místní emulátoru hello při použití hello novou sadu SDK. Nový emulátor toodownload, přejděte příliš[hello použití emulátoru DB Cosmos Azure pro místní vývoj a testování](local-emulator.md). Hello StorageConnectionString hodnota v souboru app.config musí toobe:

```
DefaultEndpointsProtocol=https;AccountName=localhost;AccountKey=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==;TableEndpoint=https://localhost:8081`. 
```

### <a name="can-my-existing-application-work-with-hello-table-api-preview"></a>Můžete Moje existující aplikace pracovat hello tabulky rozhraní API (Preview)? 
Hello útoku hello nové tabulky rozhraní API (Preview) je kompatibilní s hello existující standardní tabulky Azure SDK napříč hello vytvořit, odstranit, aktualizovat a dotaz operace v hello .NET API. Zkontrolujte, zda máte klíč řádku, protože hello tabulky rozhraní API (Preview) vyžaduje klíč oddílu a klíč řádku. Také plánujeme tooadd další podporu SDK jako budeme pokračovat směrem k GA této nabídky služeb.

### <a name="do-i-need-toomigrate-my-existing-azure-table-based-applications-toohello-new-sdk-if-i-do-not-want-toouse-hello-table-api-preview-features"></a>Potřebuji toomigrate mé existující aplikace Azure na základě tabulky toohello novou sadu SDK, pokud chcete toouse hello tabulky rozhraní API (Preview) funkce?
Ne, můžete vytvořit a používat existující prostředky standardní tabulky bez přerušení jakéhokoli druhu. Ale pokud nepoužijete hello nové tabulky rozhraní API (Preview), můžete nemohou využívat hello automatické indexu, možnost Další konzistence hello nebo globální distribuce. 

### <a name="how-do-i-add-replication-of-hello-data-in-hello-premium-table-api-preview-across-multiple-regions-of-azure"></a>Jak přidat replikace hello dat na úrovni premium hello tabulky rozhraní API (Preview) nad několika oblastmi Azure?
Můžete použít portál Azure Cosmos DB hello [nastavení globální replikace](tutorial-global-distribution-documentdb.md#portal) tooadd oblasti, které jsou vhodné pro vaši aplikaci. toodevelop globálně distribuované aplikace, měli byste také přidat aplikaci s hello PreferredLocation informace sady toohello místní oblast pro zajištění nízkou latenci pro čtení. 

### <a name="how-do-i-change-hello-primary-write-region-for-hello-account-in-hello-premium-table-api-preview"></a>Změna hello primární zápisu oblast pro účet hello v hello premium tabulky rozhraní API (Preview)
Můžete použít hello Azure Cosmos DB globální replikace portálu podokně tooadd oblast a potom převzít toohello požadované oblasti. Pokyny najdete v tématu [vývoj s více oblast Azure Cosmos DB účty](regional-failover.md). 

### <a name="how-do-i-configure-my-preferred-read-regions-for-low-latency-when-i-distribute-my-data"></a>Jak lze nakonfigurovat Moje upřednostňované čtení oblasti pro s nízkou latencí při I distribuci svá data? 
toohelp číst z místního umístění hello, použijte hello PreferredLocation klíč v souboru app.config hello. Pro existující aplikace hello tabulky rozhraní API (Preview) vrátí chybu, pokud je nastavená LocationMode. Tento kód odeberte, protože hello premium tabulky rozhraní API (Preview) převezme tyto informace ze souboru app.config hello. Další informace najdete v tématu [možnosti Azure Cosmos DB](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="how-should-i-think-about-consistency-levels-in-hello-table-api-preview"></a>Jak by měla myslíte o úrovně konzistence v hello tabulky rozhraní API (Preview)? 
Azure Cosmos DB poskytuje dobře odůvodněnou kompromis mezi konzistencí, dostupností a latencí. Azure Cosmos DB nabízí pět úrovní konzistence tooTable vývojáři rozhraní API (Preview), abyste mohli vyberte model hello správné konzistence na úrovni tabulky hello a provádět požadavky na jednotlivé při dotazování na hello data. Když se klient připojí, může však určovat úrovní konzistence. Můžete změnit úroveň hello prostřednictvím hello app.config nastavení pro hodnotu hello hello TableConsistencyLevel klíče. 

Hello tabulky rozhraní API (Preview) nabízí nízkou latencí čte s "načíst vlastní zápisy" s konzistence typu relace jako výchozí hello. Další informace najdete v tématu [úrovně konzistence](consistency-levels.md). 

Ve výchozím nastavení Azure Table storage poskytuje silnou konzistenci v rámci oblasti a Eventual konzistenci hello sekundárního umístění. 

### <a name="does-azure-cosmos-db-offer-more-consistency-levels-than-standard-tables"></a>Nabízí Azure Cosmos DB další úrovně konzistence než standardní tabulky?
Ano, informace o jak toobenefit z hello distribuované povaha Azure Cosmos DB najdete v tématu [úrovně konzistence](consistency-levels.md). Protože záruky jsou k dispozici pro úrovně konzistence hello, můžete je používat s jistotou. Další informace najdete v tématu [možnosti Azure Cosmos DB](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="when-global-distribution-is-enabled-how-long-does-it-take-tooreplicate-hello-data"></a>Pokud je povoleno globální distribuční, jak dlouho trvá tooreplicate hello dat?
Potvrdit data hello spolehlivě v místní oblast hello jsme push hello datové oblasti tooother okamžitě v řádu milisekundách. Tato replikace je závislá pouze na hello odezvy doba hello datového centra. toolearn Další informace o schopnosti hello globální distribuční databáze Cosmos Azure, najdete v části [Cosmos databázi Azure: Služba globálně distribuované databáze v Azure](distribute-data-globally.md).

### <a name="can-hello-read-request-consistency-level-be-changed"></a>Můžete změnit úroveň konzistence hello požadavků na čtení?
S Azure Cosmos databáze můžete nastavit úroveň konzistence hello na úrovni kontejneru hello (pro tabulku hello). Pomocí hello SDK můžete změnit úroveň hello tím, že poskytuje hello hodnotu pro klíč TableConsistencyLevel v souboru app.config hello. Hello možné hodnoty jsou: silného typu s ohraničenou Prošlostí, relace, konzistentní předpony a Eventual. Další informace najdete v tématu [úrovně konzistence přizpůsobitelné dat v Azure Cosmos DB](consistency-levels.md). klíče nápad Hello je, že nelze definovat hello požadavek konzistence úrovni na větší než hello nastavení pro tabulku hello. Například nelze nastavit hello úroveň konzistence pro tabulku hello v Eventual a úroveň konzistence hello požadavek na silné. 

### <a name="how-does-hello-premium-table-api-preview-account-handle-failover-if-a-region-goes-down"></a>Jak hello premium tabulky rozhraní API (Preview) účet zpracovávat převzetí služeb při selhání případě oblast výpadku? 
Hello premium vychází z hello globálně distribuované platformu Azure Cosmos DB jazyků tabulky rozhraní API (Preview). tooensure, že vaše aplikace může tolerovat výpadků datacenter, povolte alespoň jeden další oblast pro účet hello v portálu Azure Cosmos DB hello [vývoj s více oblast Azure Cosmos DB účty](regional-failover.md). Můžete nastavit prioritu hello hello oblasti pomocí portálu hello [vývoj s více oblast Azure Cosmos DB účty](regional-failover.md). 

Můžete přidat jako v mnoha oblastech jako pro účet hello a řídit, kde můžete převzít tooby poskytování prioritu převzetí služeb při selhání. Kurz, toouse hello databáze, musíte tooprovide aplikace existuje příliš. Pokud tak učiníte, nebudou vaši zákazníci se setkávají výpadku. Hello klienta SDK se automaticky homing. To znamená může zjistit hello oblast, která je mimo provoz a automaticky převzata toohello novou oblast.

### <a name="is-hello-premium-table-api-preview-enabled-for-backups"></a>Je povolen hello premium tabulky rozhraní API (Preview) pro zálohování?
Ano, hello premium tabulky rozhraní API (Preview) vychází z jazyků platformy hello Azure Cosmos databáze pro zálohování. Zálohy jsou vytvářeny automaticky. Další informace najdete v tématu [Online zálohování a obnovení s Azure Cosmos DB](online-backup-and-restore.md).

 
### <a name="does-hello-table-api-preview-index-all-attributes-of-an-entity-by-default"></a>Hello tabulky rozhraní API (Preview) všechny atributy indexu entity ve výchozím nastavení?
Ano, jsou všechny atributy entity indexovaný ve výchozím nastavení. Další informace najdete v tématu [Cosmos databázi Azure: indexování zásady](indexing-policies.md). 

### <a name="does-this-mean-i-do-not-have-toocreate-multiple-indexes-toosatisfy-hello-queries"></a>Znamená to nemá nemám toocreate více indexů toosatisfy hello dotazů? 
Ano, Azure Cosmos DB poskytuje automatické indexování všechny atributy bez jakékoli definice schématu. Tato automatizace uvolní vývojáři toofocus na aplikace hello ne na vytvoření indexu a správu. Další informace najdete v tématu [Cosmos databázi Azure: indexování zásady](indexing-policies.md).

### <a name="can-i-change-hello-indexing-policy"></a>Můžete změnit zásady indexování hello?
Ano, můžete změnit zásady indexování hello tím, že poskytuje hello definici indexu. Další informace najdete v tématu [možnosti Azure Cosmos DB](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). Je třeba tooproperly kódování a vyhnuli hello nastavení. 

Ve formátu json řetězce v souboru app.config hello:
```
{
  "indexingMode": "consistent",
  "automatic": true,
  "includedPaths": [
    {
      "path": "/somepath",
      "indexes": [
        {
          "kind": "Range",
          "dataType": "Number",
          "precision": -1
        },
        {
          "kind": "Range",
          "dataType": "String",
          "precision": -1
        } 
      ]
    }
  ],
  "excludedPaths": 
[
 {
      "path": "/anotherpath"
 }
]
}
```

### <a name="azure-cosmos-db-as-a-platform-seems-toohave-lot-of-capabilities-such-as-sorting-aggregates-hierarchy-and-other-functionality-will-you-be-adding-these-capabilities-toohello-table-api"></a>Azure DB Cosmos jako platformu zdá se, že toohave spoustu možností, jako je například řazení, agregace, hierarchie a další funkce. Můžete přidávat tyto možnosti toohello API tabulky? 
Ve verzi preview, hello API tabulka poskytuje hello stejné funkce jako Azure Table storage dotazu. Azure Cosmos DB také podporuje řazení, agregace, geoprostorové dotazu, hierarchie a širokou škálu integrované funkce. Poskytujeme další funkce v hello API tabulky v aktualizaci budoucí služby. Další informace najdete v tématu [dotazy SQL pro rozhraní API služby Azure Cosmos databáze DocumentDB](../documentdb/documentdb-sql-query.md).
 
### <a name="when-should-i-change-tablethroughput-for-hello-table-api-preview"></a>Kdy je třeba změnit TableThroughput pro hello tabulky rozhraní API (Preview)?
Měli byste změnit TableThroughput, pokud platí některá z hello následující podmínky:
* Provádění extrakce, transformace a načítání (ETL) dat, nebo chcete tooupload velké množství dat v krátkém čase. 
* Budete potřebovat další propustnost z kontejneru hello v back-end hello. Například můžete zjistit, že hello používá propustnost je větší než hello zřízené propustnosti a jste jsou získávání omezené. Další informace najdete v tématu [sadu propustnost pro Azure Cosmos DB kontejnery](set-throughput.md).

### <a name="can-i-scale-up-or-scale-down-hello-throughput-of-my-table-api-preview-table"></a>Je možné škálovat nebo snižovat hello propustnost tabulka tabulky rozhraní API (Preview)? 
Ano, můžete portál Azure Cosmos DB hello škálování podokně tooscale hello propustnost. Další informace najdete v tématu [sadu propustnost](set-throughput.md).

### <a name="is-a-default-tablethroughput-set-for-newly-provisioned-tables"></a>Je výchozí pro nově zřízeného tabulky nastavit TableThroughput?
Ano, pokud hello TableThroughput prostřednictvím app.config nepotlačí a nepoužívejte kontejner předem vytvořené v Azure Cosmos DB, hello service vytvoří tabulku s propustností 400.
 
### <a name="is-there-any-change-of-pricing-for-existing-customers-of-hello-standard-table-api"></a>Je k dispozici žádné změně ceny pro stávající zákazníky služby hello standardní tabulky API?
Žádné. Neexistuje žádná změna v ceny pro stávající zákazníky služby standardní API tabulky. 

### <a name="how-is-hello-price-calculated-for-hello-table-api-preview"></a>Výpočtu hello ceny pro hello tabulky rozhraní API (Preview) 
Hello cena závisí na hello přidělené TableThroughput. 

### <a name="how-do-i-handle-any-throttling-on-hello-tables-in-table-api-preview-offering"></a>Jak pracovat, žádné omezení na hello tabulky v tabulce rozhraní API (Preview) nabízí? 
Pokud rychlost požadavků hello překročí kapacitu hello hello zřízené propustnosti pro základní kontejner hello, obdržíte chybu a hello SDK bude opakovat hello volání použitím zásady opakování hello.

### <a name="why-do-i-need-toochoose-a-throughput-apart-from-partitionkey-and-rowkey-tootake-advantage-of-hello-premium-table-api-preview-offering-of-azure-cosmos-db"></a>Proč musím toochoose propustnost kromě PartitionKey a RowKey tootake výhod hello premium tabulky rozhraní API (Preview) nabídky Azure Cosmos databáze?
Azure Cosmos DB nastaví výchozí propustnost pro váš kontejner, pokud nezadáte v souboru app.config hello. 

Azure Cosmos DB poskytuje záruku výkonu a latencí a s horní meze operaci. Tato záruka je možné, pokud modul hello můžete vynutit zásady správného řízení na operace hello klienta. Nastavení TableThroughput zajistí, že dostanete hello zaručit propustnosti a latence, protože platforma hello si vyhrazuje tato kapacita a zaručí provozní úspěch. 

Pomocí specifikace propustnost hello můžete pružně změnit toobenefit z hello sezónnosti vaší aplikace, podle potřeb hello propustnost a ušetřili náklady.

### <a name="azure-storage-sdk-has-been-very-inexpensive-for-me-because-i-pay-only-toostore-hello-data-and-i-rarely-query-hello-new-azure-cosmos-db-offering-seems-toobe-charging-me-even-though-i-have-not-performed-a-single-transaction-or-stored-anything-can-you-please-explain"></a>Azure SDK úložiště bylo pro mě nejlepší, velmi málo, protože I platit pouze data hello toostore a zřídka dotazu. Nová nabídka Azure Cosmos DB Hello zdá se, že toobe mi poplatků, i když I nebyly provést jedné transakci nebo nic uložené. Můžete je vysvětlete?

Azure Cosmos DB je navrženou toobe globálně distribuované, na základě smlouvy SLA systému se záruky dostupnosti, latence a propustnosti. Při rezervaci propustnost v Azure Cosmos DB tak, aby zajistil, na rozdíl od hello propustnost jiných systémů. Azure Cosmos DB poskytuje další funkce, které zákazníci požadovali, jako je například sekundární indexy a globální distribuci. Během období preview hello poskytujeme model optimalizované propustnosti a nakonec plánujeme tooprovide toomeet optimalizace úložiště modelu, naše zákazníky potřebám. 

### <a name="i-never-get-a-quota-full-notification-indicating-that-a-partition-is-full-when-i-ingest-data-into-table-storage-with-hello-table-api-preview-i-do-get-this-message-is-this-offering-limiting-me-and-forcing-me-toochange-my-existing-application"></a>Nikdy zobrazí oznámení "kvóta úplné" (což znamená, že oddíl je úplná) při načítání dat do úložiště tabulek. S hello tabulky rozhraní API (Preview) zobrazí se tato zpráva. Tato nabídka je omezení mi a nenutí toochange Moje existující aplikace?

Azure Cosmos DB je systém na základě smlouvy o úrovni služeb, který poskytuje neomezené škálování záruky latence, propustnost, dostupnosti a konzistence. tooensure zaručit premium výkon, zajistěte, aby byla velikost dat a index spravovat a škálovatelnost. Hello 10 GB limit počtu hello entit nebo počet položek na klíč oddílu je tooensure, můžeme poskytnout vynikající výkon vyhledávání a dotazů. tooensure, která je škálovatelná aplikace dobře i pro Azure Storage, doporučujeme vám *není* vytvořit aktivní oddíl ukládání všechny informace v jednom oddílu a dotazování ho. 

### <a name="so-partitionkey-and-rowkey-are-still-required-with-hello-new-table-api-preview"></a>Takže PartitionKey a RowKey se stále vyžadují s hello nové tabulky rozhraní API (Preview)? 
Ano. Protože hello útoku hello tabulky rozhraní API (Preview) je podobné toothat hello úložiště Table SDK, klíč oddílu hello poskytuje model efektivní způsob toodistribute hello data. Hello klíč řádku je jedinečný v rámci oddílu. Hello toobe klíče potřebám řádek přítomen a nemůže mít hodnotu null jako v hello standardní SDK. Délka Hello RowKey je 255 bajtů a hello délka PartitionKey je 100 bajtů (brzy toobe vyšší too1 KB). 

### <a name="what-are-hello-error-messages-for-hello-table-api-preview"></a>Jaké jsou hello chybové zprávy pro hello tabulky rozhraní API (Preview)?
Vzhledem k tomu, že tato předběžná verze je kompatibilní s tabulkou standardní hello, bude většina chyb hello mapovat toohello chyby z tabulky Standardní hello. 

### <a name="why-do-i-get-throttled-when-i-try-toocreate-lot-of-tables-one-after-another-in-hello-table-api-preview"></a>Proč I získat omezeny při toocreate mnoho tabulek jedna po druhé v hello tabulky rozhraní API (Preview)?
Azure Cosmos DB je systém na základě smlouvy o úrovni služeb, který poskytuje latence, propustnost, dostupnosti a konzistence záruky. Protože je zřízená systému, vyhrazuje prostředky tooguarantee tyto požadavky. Hello rychlé rychlost vytváření tabulek je zjištěna a omezení. Doporučujeme podívat hello rychlost vytváření tabulek a snížit tooless než 5 za minutu. Pamatujte na to, že hello tabulky rozhraní API (Preview) je zřízená systém. Hello chvíli můžete zřídit, bude zahájena toopay pro ni. 

## <a name="develop-against-hello-graph-api-preview"></a>Vývoj proti hello rozhraní Graph API (Preview)
### <a name="how-can-i-apply-hello-functionality-of-graph-api-preview-tooazure-cosmos-db"></a>Tom, jak můžete použít funkce hello rozhraní Graph API (Preview) tooAzure Cosmos DB?
Můžete použít rozšíření knihovny tooapply hello funkce rozhraní Graph API (Preview). V této knihovně se nazývá grafy Microsoft Azure a je k dispozici na NuGet. 

### <a name="it-looks-like-you-support-hello-gremlin-graph-traversal-language-do-you-plan-tooadd-more-forms-of-query"></a>Zdá se, podporují hello Gremlin grafu traversal jazyk. Máte v plánu tooadd další formuláře dotazu?
Ano, plánujeme tooadd další mechanismy pro dotaz v budoucnu hello. 

### <a name="how-can-i-use-hello-new-graph-api-preview-offering"></a>Jak můžete použít hello novou nabídku rozhraní Graph API (Preview)? 
Začínáme, dokončení hello tooget [rozhraní Graph API](../cosmos-db/create-graph-dotnet.md) úvodní článek.

<a id="moving-to-cosmos-db"></a>
## <a name="questions-from-documentdb-customers"></a>Dotazy zákazníků DocumentDB
### <a name="why-are-you-moving-tooazure-cosmos-db"></a>Proč se přesouvání tooAzure Cosmos DB? 

Azure Cosmos DB je další velký nárůst hello ve globálně distribuované databáze cloudu v odpovídajícím měřítku. Jako zákazník DocumentDB máte teď přístup toohello revoluční systém a možnosti, které nabízí Azure Cosmos DB.

Azure Cosmos DB spustit jako "Projektu Florencii" v 2010 tooaddress hello problémové body, jimž vývojáři při vytváření aplikace ve velkém měřítku uvnitř společnosti Microsoft. výzvy Hello vytváření globálně distribuované aplikace nejsou jedinečné tooMicrosoft, proto jsme provedli hello první generace této technologie k dispozici v 2015 tooAzure vývojáři hello tvar Azure DocumentDB. 

Od tohoto okamžiku jsme přidali nové funkce a zavedla významná nové funkce. Azure Cosmos DB je výsledek hello. Jako součást této verze, DocumentDB zákazníků s jejich daty, automaticky a bezproblémově budou Azure Cosmos DB zákazníků. Tyto funkce jsou v oblastech hello hello základní databázový stroj, jakož i globální distribuce, elastickou škálovatelnost a špičkový, komplexní SLA. Konkrétně jsme vyvinuly hello Azure Cosmos DB databáze tooefficiently mapa oblíbených datové modely, typ systémy a rozhraní API toohello základní datový model databáze Azure Cosmos. 

Hello aktuální projevem vývojáře přístupem Tato práce je nová podpora hello [Gremlin](../cosmos-db/graph-introduction.md) a [tabulky úložiště rozhraní API](../cosmos-db/table-introduction.md). A jedná se jenom hello začátku. Plánujeme tooadd dalších oblíbených rozhraní API a novější datové modely v čase, s další vylepšení výkonu a úložiště v globálním měřítku. 

Je důležité toopoint si tento hello DocumentDB [SQL dialekt](../documentdb/documentdb-sql-query.md) vždy pouze jeden z hello bylo mnoho rozhraní API této hello může podporovat základní Azure DB Cosmos. Pro vývojáře, kteří používají plně spravovaná služba, například Azure Cosmos DB, hello pouze rozhraní toohello služba je hello rozhraní API, které jsou vystaveny službou hello. Nic skutečně změní pro stávající zákazníky služby DocumentDB. V Azure DB Cosmos získáte přesně hello, které nabízí stejnou SQL API této DocumentDB. A teď (a v budoucích hello) můžete přístup k dalším funkcím dříve nedostupné 

Jiné projevem naše trvalá práce je hello rozšířené základ pro globální a elastické škálovatelnost propustnost a úložiště. Provedli jsme několik vylepšení základní toohello globální distribuční subsystému. Jedním z hello hello konzistentní předpony konzistence model, který umožňuje celkový pět modely dobře definovaný konzistence je mnoho takové funkce vývojáře přístupem. Vydá jsme řadu zajímavějšího funkcí, jako se pro dospělé. 

### <a name="what-do-i-need-toodo-tooensure-that-my-documentdb-resources-continue-toorun-on-azure-cosmos-db"></a>Co dělat, je nutné, aby Moje prostředky DocumentDB pokračovat v Azure Cosmos DB toorun tooensure toodo?

Je nutné toomake žádné změny vůbec. Vaše prostředky DocumentDB jsou teď prostředky Azure Cosmos DB a došlo bez přerušení služby hello při přesunutí došlo k chybě.

### <a name="what-changes-do-i-need-toomake-for-my-app-toowork-with-azure-cosmos-db"></a>Co dělat změny potřebuji toomake pro Moje aplikace toowork s Azure Cosmos DB?

Neexistují žádné toomake změny. Názvy tříd, obory názvů a NuGet balíček nezměnily. Jako vždy doporučujeme, aby byl váš sad SDK si toodate tootake výhod hello nejnovější funkce a vylepšení. 

### <a name="whats-changed-in-hello-azure-portal"></a>Co se změnilo v hello portál Azure?

DocumentDB již nadále nebude zobrazovat na portálu hello jako služby Azure. Místo ní je nová ikona Azure Cosmos DB, jak ukazuje následující obrázek hello. Všechny kolekce jsou k dispozici, protože se nacházely před a je možné škálovat propustnosti, změna úrovně konzistence a sledování smluv SLA. Vylepšené možnosti Hello Data Explorer (Preview). Teď můžete zobrazit a upravovat dokumenty, vytvářet a spouštět dotazy a pracovat s uložené procedury, triggery a UDF z jedné stránky, jak ukazuje následující obrázek hello: 

![okno Azure Cosmos DB kolekce Hello](./media/faq/cosmos-db-data-explorer.png)

### <a name="are-there-changes-toopricing"></a>Existují toopricing změny?

Ne, hello náklady na kterém běží aplikace v Azure Cosmos DB hello stejné před.

### <a name="are-there-changes-toohello-slas"></a>Existují změny toohello SLA?

Ne, hello SLA pro dostupnost, konzistence, latence a propustnosti jsou beze změny a jsou stále zobrazuje hello portálu. Další informace najdete v tématu [SLA pro Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/).
   
![Aplikace úkolů s ukázkovými daty](./media/faq/azure-cosmosdb-portal-metrics-slas.png)


[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md
