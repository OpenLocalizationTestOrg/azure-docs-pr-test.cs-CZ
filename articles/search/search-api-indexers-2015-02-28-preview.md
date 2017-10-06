---
title: "Indexer operací (rozhraní API REST služby vyhledávání systému Azure: 2015-02-28-Preview) | Microsoft Docs"
description: "Indexer operací (rozhraní API REST služby vyhledávání systému Azure: 2015-02-28-Preview)"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 42b5f85c-8304-4bc7-8e1e-a9c76b8ca25a
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: eugenesh
ms.openlocfilehash: 4dd9591072b44eeabae6eac1182b4eea10fd4a22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="indexer-operations-azure-search-service-rest-api-2015-02-28-preview"></a>Indexer operací (rozhraní API REST služby vyhledávání systému Azure: 2015-02-28-Preview)
> [!NOTE]
> Tento článek popisuje indexery v hello [2015-02-28-Preview REST API](search-api-2015-02-28-preview.md). Tato verze rozhraní API přidá verze preview indexer Azure Blob Storage s extrakce dokumentu a indexer Azure Table Storage a dalších vylepšení. Hello rozhraní API podporuje také všeobecně dostupná indexery (GA), včetně indexery pro databáze SQL Azure, SQL Server na virtuálních počítačích Azure a Azure Cosmos DB.
> 
> 

## <a name="overview"></a>Přehled
Vyhledávání systému Azure můžete integrovat přímo některé běžné zdroje dat, odebrání hello nutné toowrite kód tooindex vaše data. tooset si tuto dobu, můžete volat rozhraní API služby Azure Search toocreate hello a spravovat **indexery** a **zdroje dat**. 

**Indexer** je na prostředek, který připojí zdroje dat s indexy vyhledávání cíl. Indexer se používá v hello následující způsoby: 

* Proveďte jednorázové kopii dat toopopulate hello indexu.
* Synchronizujte indexu se změnami ve zdroji dat hello podle plánu. plán Hello je součástí definice indexer hello.
* Volání na vyžádání tooupdate index podle potřeby. 

**Indexer** je užitečné, když má index tooan pravidelné aktualizace. Můžete nastavit plán vložené jako součást definice indexeru, nebo ji spustit na vyžádání pomocí [spustit Indexer](#RunIndexer). 

A **zdroj dat** Určuje, jaká data musí toobe indexované, přihlašovací údaje tooaccess hello data a zásady tooenable Azure Search tooefficiently identifikovat změny v datech hello (například upravit nebo odstranit řádky v tabulce databáze). Je definována jako prostředek nezávisle tak, aby ji můžete použít několik indexerů.

Hello následující zdroje dat se aktuálně podporují:

* **Databáze Azure SQL** a **systému SQL Server na virtuálních počítačích Azure**. Cílové návodu, najdete v části [v tomto článku](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md). 
* **Azure Cosmos DB**. Cílové návodu, najdete v části [v tomto článku](search-howto-index-documentdb.md). 
* **Azure Blob Storage**, včetně dokumentů hello následující formáty: PDF, Microsoft Office (DOCX/DOC, XSLX nebo XLS, PPTX/PPT, MSG), HTML, XML, ZIP a prostý text soubory (včetně JSON). Cílové návodu, najdete v části [v tomto článku](search-howto-indexing-azure-blob-storage.md).
* **Azure Table Storage**. Cílové návodu, najdete v části [v tomto článku](search-howto-indexing-azure-tables.md).

Jsme chtěli přidání podpory pro další datové zdroje v budoucnu hello. toohelp nám nastavit priority rozhodování, zadejte svůj názor na hello [fóru pro zpětnou vazbu Azure Search](http://feedback.azure.com/forums/263029-azure-search).

V tématu [omezení služby](search-limits-quotas-capacity.md) pro maximální omezuje související prostředky zdroj tooindexer a data.

## <a name="typical-usage-flow"></a>Tok typickému využití
Můžete vytvořit a spravovat indexery a zdroje dat prostřednictvím jednoduchých požadavků HTTP (POST, GET, PUT, DELETE) proti danou `data source` nebo `indexer` prostředků.

Nastavení automatické indexování se obvykle čtyři krocích:

1. Identifikujte hello zdroj dat, který obsahuje hello data, která potřebuje toobe indexovat. Uvědomte si, že Azure Search nemusí podporovat všechny hello datových typů ve zdroji dat. V tématu [podporované datové typy](https://msdn.microsoft.com/library/azure/dn798938.aspx) seznam hello.
2. Vytvoření indexu Azure Search, jejichž schéma je kompatibilní s datovým zdrojem.
3. Vytvoření zdroje dat Azure Search, jak je popsáno v [vytvořit zdroj dat](#CreateDataSource).
4. Vytvoření indexer Azure Search, jak je popsáno [vytvořit Indexer](#CreateIndexer).

Měli byste naplánovat na vytváření jeden indexeru pro každou kombinaci cílový index a datové zdroje. Může mít několik indexerů zápisu do hello stejný index a můžete znovu použít hello stejný zdroj dat pro několik indexerů. Indexer ale může využívat jenom jeden zdroj dat najednou a může zapisovat jenom jeden index tooa. 

Po vytvoření indexer, je možné načíst jeho stav spuštění pomocí hello [získání stavu Indexer](#GetIndexerStatus) operaci. Také můžete kdykoli spustit indexer (místo nebo přidání toorunning ho pravidelně podle plánu) pomocí hello [spustit Indexer](#RunIndexer) operaci.

<!-- MSDN has 2 art files plus a API topic link list -->


## <a name="create-data-source"></a>Vytvoření zdroje dat
Ve službě Azure Search zdroj dat se používá s indexery, poskytuje cílový index informace připojení hello aktualizace ad hoc nebo naplánovanou dat. Můžete vytvořit nový zdroj dat v rámci služby Azure Search pomocí požadavku HTTP POST.

    POST https://[service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Alternativně můžete použít PUT a zadejte název zdroje dat hello na hello identifikátor URI. Pokud zdroj dat hello neexistuje, bude vytvořen.

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]

> [!NOTE]
> maximální počet zdrojů dat, které jsou povolené Hello se liší podle cenové úrovně. Hello bezplatná služba umožňuje až too3 datové zdroje. Standardní služby umožňuje 50 datové zdroje. V tématu [omezení služby](search-limits-quotas-capacity.md) podrobnosti.
> 
> 

**Požadavek**

Je požadován pro všechny žádosti o služby protokol HTTPS. Hello **vytvořit zdroj dat** požadavek lze sestavit pomocí Metoda POST nebo PUT metody. Při použití POST, je nutné zadat název zdroje dat v textu žádosti hello společně s definice zdroje dat hello. Pomocí PUT název hello je součástí adresy URL hello. Pokud zdroj dat hello neexistuje, vytvoří se. Pokud již existuje, je aktualizovaná toohello novou definici. 

Název zdroje dat Hello musí být malá písmena, začínat písmenem nebo číslicí, mít žádné lomítka nebo tečky a být kratší než 128 znaků. Po spuštění hello název zdroje dat s písmenem nebo číslicí, může obsahovat hello zbytek hello název jakékoli písmeno, čísla a pomlčky, dokud nejsou po sobě jdoucí pomlčky hello. V tématu [pravidla po pojmenování](https://msdn.microsoft.com/library/azure/dn857353.aspx) podrobnosti.

Hello `api-version` je vyžadován. aktuální verze Hello je `2015-02-28`.

**Hlavičky požadavku**

Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi. 

* `Content-Type`: Vyžaduje se. Tuto možnost nastavíte příliš`application/json`
* `api-key`: Vyžaduje se. Hello `api-key` je použité tooauthenticate hello požadavek tooyour službu vyhledávání. Je řetězcová hodnota, jedinečné tooyour služby. Hello **vytvořit zdroj dat** musí zahrnovat požadavek `api-key` záhlaví nastavit klíč správce tooyour (jako klíč dotazu názvem na rozdíl od tooa). 

Budete také potřebovat hello služby název tooconstruct hello adrese URL žádosti. Můžete získat i hello název služby a `api-key` z řídicího panelu služby v hello [portálu Azure](https://portal.azure.com/). V tématu [vytvořte službu vyhledávání v portálu hello](search-create-service-portal.md) nápovědu navigace stránky.

<a name="CreateDataSourceRequestSyntax"></a>
**Syntaxe požadavku textu**

text Hello hello žádosti obsahuje definici zdroje dat, která zahrnuje typ zdroje dat hello, přihlašovací údaje tooread hello data a také volitelnými daty detekce změn a změnit detekce odstranění dat, které zásady, které jsou používané tooefficiently identifikovat nebo Odstraněná data ve zdroji dat hello při použití s pravidelně naplánované indexer. 

Syntaxe Hello strukturování datová část požadavku hello je následující. Ukázková žádost je k dispozici další na v tomto tématu.

    { 
        "name" : "Required for POST, optional for PUT. hello name of hello data source",
        "description" : "Optional. Anything you want, or nothing at all",
        "type" : "Required. Must be one of 'azuresql', 'documentdb', 'azureblob', or 'azuretable'",
        "credentials" : { "connectionString" : "Required. Connection string for your data source" },
        "container" : { "name" : "Required. hello name of hello table, collection, or blob container you wish tooindex" },
        "dataChangeDetectionPolicy" : { Optional. See below for details }, 
        "dataDeletionDetectionPolicy" : { Optional. See below for details }
    }

Žádost obsahuje hello následující vlastnosti: 

* `name`: Vyžaduje se. Hello název zdroje dat hello. Název zdroje dat musí pouze obsahovat malá písmena, číslice nebo pomlčky, nesmí začínat ani končit pomlčky a musí mít omezený too128 znaků.
* `description`: Volitelný popis. 
* `type`: Vyžaduje se. Musí být jedna z hello podporované typy zdrojů dat:
  * `azuresql`-Azure SQL Database nebo SQL Server na virtuálních počítačích Azure
  * `documentdb`-DB azure Cosmos
  * `azureblob`-Azure Blob Storage
  * `azuretable`-Azure Table Storage
* `credentials`:
  * Hello požadované `connectionString` vlastnost určuje hello připojovací řetězec pro zdroj dat hello. Formát Hello hello připojovacího řetězce, závisí na typu zdroje dat hello: 
    * Pro Azure SQL jde hello obvykle připojovací řetězec SQL serveru. Pokud používáte hello Azure portálu tooretrieve hello připojovací řetězec, použijte hello `ADO.NET connection string` možnost.
    * Pro Azure Cosmos DB, musí být hello připojovací řetězec ve formátu hello: `"AccountEndpoint=https://[your account name].documents.azure.com;AccountKey=[your account key];Database=[your database id]"`. Všechny hello hodnoty jsou povinné. Můžete je najít v hello [portál Azure](https://portal.azure.com/).  
    * U objektů Blob v Azure a Table Storage je to hello připojovací řetězce k účtu úložiště. Formát Hello je popsán [zde](https://azure.microsoft.com/documentation/articles/storage-configure-connection-string/). Vyžaduje se protokol koncový bod HTTPS.  
* `container`, požadované: Určuje tooindex hello dat pomocí hello `name` a `query` vlastnosti: 
  * `name`, požadované:
    * Azure SQL: Určuje hello tabulku nebo zobrazení. Můžete použít kvalifikovaný schématu názvy, například `[dbo].[mytable]`.
    * DocumentDB: Určuje kolekci hello. 
    * Azure Blob Storage: Určuje hello kontejner úložiště.
    * Azure Table Storage: Určuje název hello hello tabulky. 
  * `query`, volitelné:
    * DocumentDB: umožňuje toospecify dotaz, který vyrovná libovolné rozložení dokumentu JSON do plochých schéma, které mohou indexu Azure Search.  
    * Azure Blob Storage: umožňuje toospecify virtuální složky v rámci kontejneru objektů blob hello. Například cesta blobu `mycontainer/documents/blob.pdf`, `documents` lze použít jako virtuální složky hello.
    * Azure Table Storage: umožňuje toospecify dotazu, filtry hello sadu řádků toobe importovat.
    * Azure SQL: dotaz není podporovaný. Pokud potřebujete tuto funkci, prosím hlasovat pro [tento návrh](https://feedback.azure.com/forums/263029-azure-search/suggestions/9893490-support-user-provided-query-in-sql-indexer)
* Hello volitelné `dataChangeDetectionPolicy` a `dataDeletionDetectionPolicy` vlastnosti jsou popsány níže.

<a name="DataChangeDetectionPolicies"></a>
**Zásady detekce změn dat**

účel Hello dat. změnit zásady detekce je tooefficiently identifikaci položek změněná data. Podporované zásady lišit v závislosti na typu zdroje dat hello. Následující oddíly popisují každou zásadu. 

***Zásady detekce změn horní meze*** 

Tuto zásadu používejte, pokud zdroj dat obsahuje sloupec nebo vlastnost, která splňuje hello následující kritéria:

* Vloží všechny zadejte hodnotu pro sloupec hello. 
* Všechny položky tooan aktualizace také změnit hodnotu hello hello sloupce. 
* Hello hodnotu v tomto sloupci se zvyšuje s každé změně.
* Dotazy, které používají podobné toohello následující klauzule filtru `WHERE [High Water Mark Column] > [Current High Water Mark Value]` mohou být provedeny efektivně.

Například při použití Azure SQL datových zdrojů, indexované `rowversion` sloupec je hello ideální volbou pro použití s zásadám hello horní mez. 

Tyto zásady můžete nastavit následujícím způsobem:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
    } 

Pokud používáte Azure Cosmos DB zdroje dat, je nutné použít hello `_ts` vlastnost poskytované Azure Cosmos DB. 

Při použití zdrojů dat objektů Blob v Azure, Azure Search automaticky používá horní meze změnit zásady detekce založené na objekt blob poslední úpravy časové razítko; nepotřebujete toospecify tato zásada sami.   

***Zásady detekce změn s integrací SQL***

Pokud vaše databáze SQL podporuje [sledování změn](https://msdn.microsoft.com/library/bb933875.aspx), doporučujeme používat SQL integrované změnit způsob sledování. Tato zásada umožňuje hello nejúčinnější sledování změn a umožňuje Azure Search tooidentify odstranit řádky bez nutnosti toohave sloupec explicitní "obnovitelného odstranění" ve schématu.

Integrované sledování změn je podporované počínaje hello následující verze databáze systému SQL Server: 

* SQL Server 2008 R2, pokud používáte systém SQL Server na virtuálních počítačích Azure.
* Azure SQL Database verze 12, pokud používáte Azure SQL Database.  

Když pomocí integrované sledování změn SQL zásad, nezadávejte zásadami detekce odstraňování oddělení dat – tato zásada má integrovanou podporu pro identifikaci odstranit řádky. 

Tuto zásadu lze použít pouze s tabulkami; nelze použít se zobrazeními. Je nutné tooenable sledování změn pro hello tabulku, kterou používáte, abyste mohli používat tuto zásadu. V tématu [povolení a zakázání sledování změn](https://msdn.microsoft.com/library/bb964713.aspx) pokyny.    

Při vytváření struktury hello **vytvořit zdroj dat** požádat, SQL integrované zásady sledování změn, můžete nastavit následujícím způsobem:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
    }

<a name="DataDeletionDetectionPolicies"></a>
**Zásady detekce odstranění dat**

účelem Hello zásady detekce odstranění dat je tooefficiently identifikaci položek odstraněná data. V současné době hello podporovány pouze zásady je hello `Soft Delete` zásady, které umožňuje identifikaci odstraněné položky na základě hodnoty hello `soft delete` sloupec nebo vlastnost ve zdroji dat hello. Tyto zásady můžete nastavit následujícím způsobem:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "hello column that specifies whether a row was deleted", 
        "softDeleteMarkerValue" : "hello value that identifies a row as deleted" 
    }

> [!NOTE]
> Jsou podporovány pouze sloupce s řetězec, celé číslo nebo logické hodnoty. Hodnota použitá jako Hello `softDeleteMarkerValue` musí být řetězec, i když hello odpovídající sloupec obsahuje celá čísla nebo logické hodnoty. Například pokud hello hodnotu, která se zobrazí ve zdroji dat je 1, použijte `"1"` jako hello `softDeleteMarkerValue`.    
> 
> 

<a name="CreateDataSourceRequestExamples"></a>
**Příklady text žádosti**

Pokud máte v úmyslu toouse hello zdroj dat s indexer, který spouští podle plánu, tento příklad ukazuje, jak změnit toospecify a odstranění zásady detekce: 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy", "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }

Pokud máte v úmyslu pouze toouse hello zdroj dat pro jednorázové kopii hello dat, lze vynechat hello zásady:

    { 
        "name" : "asqldatasource",
        "description" : "anything you want, or nothing at all",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" }
    } 

**Odpověď**

Pro úspěšné žádosti: 201 – vytvořeno. 

<a name="UpdateDataSource"></a>

## <a name="update-data-source"></a>Aktualizovat zdroj dat
Můžete aktualizovat stávajícího zdroje dat pomocí požadavek HTTP PUT. Zadáte název hello tooupdate zdroje dat hello v identifikátoru URI žádosti hello:

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Hello `api-version` je vyžadován. aktuální verze Hello je `2015-02-28`. [Azure verze rozhraní API služby Search](https://msdn.microsoft.com/library/azure/dn864560.aspx) má podrobnosti a další informace o alternativní verze.

Hello `api-key` musí být klíče správce (jako klíč dotazu názvem na rozdíl od tooa). Najdete v části ověřování toohello v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn více informací o klíči. [Vytvořte službu vyhledávání v portálu hello](search-create-service-portal.md) vysvětluje, jak používat adresu URL služby hello tooget a klíčové vlastnosti v žádosti o hello.

**Požadavek**

Hello syntaxe tělo žádosti je hello stejné jako u [vytvořit zdroj dat požadavky](#CreateDataSourceRequestSyntax).

U stávajícího zdroje dat nelze aktualizovat některé vlastnosti. Například nelze změnit typ hello stávajícího zdroje dat.  

Pokud nechcete, aby toochange hello připojovací řetězec pro stávajícího zdroje dat, můžete zadat hello literálu `<unchanged>` pro hello připojovací řetězec. To je užitečné v situacích, kdy potřebují tooupdate zdroj dat, ale nemáte pohodlný přístup toohello připojovací řetězec, protože je to citlivým z hlediska zabezpečení dat.

**Odpověď**

Pro úspěšné žádosti: 201 – vytvořeno Pokud nový zdroj dat byl vytvořený a 204 ne obsahu Pokud stávajícího zdroje dat byla aktualizována.

<a name="ListDataSource"></a>

## <a name="list-data-sources"></a>Seznam zdrojů dat
Hello **zdroje dat seznamu** operace vrátí seznam hodnot hello zdroje dat ve službě Azure Search. 

    GET https://[service name].search.windows.net/datasources?api-version=[api-version]
    api-key: [admin key]

Hello `api-version` je vyžadován. aktuální verze Hello je `2015-02-28`. 

Hello `api-key` musí být klíče správce (jako klíč dotazu názvem na rozdíl od tooa). Najdete v části ověřování toohello v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn více informací o klíči. [Vytvořte službu vyhledávání v portálu hello](search-create-service-portal.md) vysvětluje, jak používat adresu URL služby hello tooget a klíčové vlastnosti v žádosti o hello.

**Odpověď**

Pro úspěšné žádosti: 200 OK.

Tady je odpovědi na příkladu:

    {
      "value" : [
        {
          "name": "datasource1",
          "type": "azuresql",
          ... other data source properties
        }]
    }

Všimněte si, že můžete filtrovat hello odpovědi dolů toojust hello vlastnosti, které vás zajímají. Například pokud chcete pouze seznam názvy zdrojů dat, použijte hello OData `$select` dotazu možnost:

    GET /datasources?api-version=205-02-28&$select=name

V takovém případě hello odpověď z hello výše příklad by měly vypadat následovně: 

    {
      "value" : [ { "name": "datasource1" }, ... ]
    }

Toto je užitečné toosave šířky pásma, pokud máte spoustu dalších zdrojů dat ve vyhledávací službě.

<a name="GetDataSource"></a>

## <a name="get-data-source"></a>Získat zdroj dat
Hello **získat zdroj dat** operaci získá hello definice zdroje dat z Azure Search.

    GET https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

Hello `api-version` je vyžadován. aktuální verze Hello je `2015-02-28`. 

Hello `api-key` musí být klíče správce (jako klíč dotazu názvem na rozdíl od tooa). Najdete v části ověřování toohello v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn více informací o klíči. [Vytvořte službu vyhledávání v portálu hello](search-create-service-portal.md) vysvětluje, jak používat adresu URL služby hello tooget a klíčové vlastnosti v žádosti o hello.

**Odpověď**

Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.

odpověď Hello je podobné tooexamples v [požadavky příklad vytvoření zdroje dat](#CreateDataSourceRequestExamples): 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName" : "IsDeleted", 
            "softDeleteMarkerValue" : "true" }
    }

> [!NOTE]
> Nenastavujte hello `Accept` hlavička požadavku příliš`application/json;odata.metadata=none` při volající toto rozhraní API jako tak způsobí, že `@odata.type` atribut toobe vynechaný hello odpovědi a nebude možné toodifferentiate mezi změny dat a data zjišťování odstranění zásady různých typů. 
> 
> 

<a name="DeleteDataSource"></a>

## <a name="delete-data-source"></a>Odstranit zdroj dat
Hello **odstranit zdroj dat** operace odebere zdroj dat ze služby Azure Search.

    DELETE https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

> [!NOTE]
> Pokud žádné indexery odkazovat hello zdroj dat, který odstraňujete, se provede operaci odstranění hello. Ale tyto indexery přejde do chybového stavu při jeho příštím spuštění.  
> 
> 

Hello `api-version` je vyžadován. aktuální verze Hello je `2015-02-28`. 

Hello `api-key` musí být klíče správce (jako klíč dotazu názvem na rozdíl od tooa). Najdete v části ověřování toohello v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn více informací o klíči. [Vytvořte službu vyhledávání v portálu hello](search-create-service-portal.md) vysvětluje, jak používat adresu URL služby hello tooget a klíčové vlastnosti v žádosti o hello.

**Odpověď**

: Stav 204 ne obsahu je vrácen kód pro úspěšné odpovědi.

<a name="CreateIndexer"></a>

## <a name="create-indexer"></a>Vytvoření indexeru
Můžete vytvořit nový indexer v rámci služby Azure Search pomocí požadavku HTTP POST.

    POST https://[service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Alternativně můžete použít PUT a zadejte název zdroje dat hello na hello identifikátor URI. Pokud zdroj dat hello neexistuje, bude vytvořen.

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]

> [!NOTE]
> maximální počet indexery povolené Hello se liší podle cenové úrovně. bezplatné služby Hello umožňuje až too3 indexery. Standardní služby umožňuje 50 indexery. V tématu [omezení služby](search-limits-quotas-capacity.md) podrobnosti.
> 
> 

Hello `api-version` je vyžadován. aktuální verze Hello je `2015-02-28`. 

Hello `api-key` musí být klíče správce (jako klíč dotazu názvem na rozdíl od tooa). Najdete v části ověřování toohello v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn více informací o klíči. [Vytvořte službu vyhledávání v portálu hello](search-create-service-portal.md) vysvětluje, jak používat adresu URL služby hello tooget a klíčové vlastnosti v žádosti o hello.

<a name="CreateIndexerRequestSyntax"></a>
**Syntaxe požadavku textu**

text Hello hello žádosti obsahuje definici indexer, který určuje hello zdroj dat a hello cílový index pro indexování, jakož i volitelné indexování plán a parametry. 

Syntaxe Hello strukturování datová část požadavku hello je následující. Ukázková žádost je k dispozici další na v tomto tématu.

    { 
        "name" : "Required for POST, optional for PUT. hello name of hello indexer",
        "description" : "Optional. Anything you want, or null",
        "dataSourceName" : "Required. hello name of an existing data source",
        "targetIndexName" : "Required. hello name of an existing index",
        "schedule" : { Optional. See Indexing Schedule below. },
        "parameters" : { Optional. See Indexing Parameters below. },
        "fieldMappings" : { Optional. See Field Mappings below. },
        "disabled" : Optional boolean value indicating whether hello indexer is disabled. False by default.  
    }

**Plán indexeru**

Indexer Volitelně můžete zadat plán. Pokud plán je k dispozici, hello indexer bude pravidelně spouštět podle plánu. Plán má hello následující atributy:

* `interval`: Vyžaduje se. Doba trvání hodnotu, která určuje interval nebo období pro indexer se spustí. Hello Nejmenší povolený interval je 5 minut; Hello nejdelší je jeden den. Musí být naformátovaná jako hodnota "hodnoty doby podle" XSD (omezená podmnožina [ISO 8601 trvání](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) hodnotu). vzor Hello: `"P[nD][T[nH][nM]]"`. Příklady: `PT15M` pro každých 15 minut, `PT2H` pro každé 2 hodiny. 
* `startTime`: Vyžaduje se. Datetime UTC, když hello indexer by se měl spustit systémem. 

**Indexer parametry**

Indexer Volitelně můžete zadat několik parametrů, které ovlivňují své chování. Všechny hello parametry jsou volitelné.  

* `maxFailedItems`: hello počet položek, které může selhat toobe indexované kterého se spuštění indexeru považuje za selhání. Výchozí hodnota je 0. Vrátí informace o neúspěšné položky hello [získání stavu Indexer](#GetIndexerStatus) operaci. 
* `maxFailedItemsPerBatch`: hello počet položek, které může selhat toobe indexované v každé dávce, kterého se spuštění indexeru považuje za selhání. Výchozí hodnota je 0.
* `base64EncodeKeys`: Určuje, zda dokument klíče budou kódování base-64. Vyhledávání systému Azure vynucuje omezení znaků, které můžou být v klíči dokumentu. Hello hodnoty v zdrojová data však může obsahovat znaky, které jsou neplatné. Pokud je nutné tooindex například hodnoty jako dokument klíčů, můžete tento příznak nastavit tootrue. Výchozí hodnota je `false`.
* `batchSize`: Určuje hello počet položek, které čtou ze zdroje dat hello a indexované jako jeden batch v pořadí tooimprove výkonu. Výchozí Hello závisí na typu zdroje dat hello: je 1 000 pro Azure SQL a Azure Cosmos DB a 10 pro Azure Blob Storage.

**Mapování polí**

Pole mapování toomap název pole můžete použít v hello jiné pole Název tooa zdroje dat v hello cílový index. Představte si třeba zdrojová tabulka s polem `_id`. Služba Azure Search neumožňuje pole název začíná podtržítkem, takže hello pole musí být přejmenován. To lze provést pomocí hello `fieldMappings` vlastnost indexeru hello následujícím způsobem: 

    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 

Můžete určit více mapování polí: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "_id", "targetFieldName" : "id" },
        { "sourceFieldName" : "_timestamp", "targetFieldName" : "timestamp" },
     ]

Zdrojové a cílové názvy polí jsou velká a malá písmena.

<a name="FieldMappingFunctions"></a>
***Funkce mapování polí***

Mapování polí může být také hodnoty polí použitých tootransform zdroje pomocí *mapování funkce*.

V současné době podporuje pouze jeden tyto funkce: `jsonArrayToStringCollection`. Analyzuje pole, které obsahuje řetězec formátovaný jako pole JSON do pole Collection(Edm.String) v hello cílový index. Je určený pro použití se službou Azure SQL indexer na konkrétní vzhledem k tomu, že SQL nemá datový typ nativní kolekce. Můžete použít takto: 

    "fieldMappings" : [ { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } } ] 

Například pokud hello zdrojové pole obsahuje řetězec hello `["red", "white", "blue"]`, pak hello cílové pole typu `Collection(Edm.String)` vyplní hodnotami hello tři `"red"`, `"white"` a `"blue"`.

Všimněte si, že hello `targetFieldName` vlastnost je volitelná; Pokud vynecháno, hello `sourceFieldName` hodnota se používá. 

<a name="CreateIndexerRequestExamples"></a>
**Příklady text žádosti**

Hello následující příklad vytvoří indexer, který kopíruje data z tabulky hello odkazuje hello `ordersds` zdroje dat toohello `orders` index podle plánu, který začíná na 1 ledna 2015 UTC a spouští každou hodinu. Každé vyvolání indexeru bude úspěšné, je-li více než 5 položek selhání toobe indexované v každé dávce, a maximálně 10 položek nezdaří toobe indexované celkem. 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }

**Odpověď**

Pro úspěšné žádosti 201 – vytvořeno.

<a name="UpdateIndexer"></a>

## <a name="update-indexer"></a>Aktualizovat Indexer
Můžete aktualizovat existujícího indexeru pomocí požadavek HTTP PUT. Zadáte název hello hello indexer tooupdate v identifikátoru URI žádosti hello:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Hello `api-version` je vyžadován. aktuální verze Hello je `2015-02-28`. 

Hello `api-key` musí být klíče správce (jako klíč dotazu názvem na rozdíl od tooa). Najdete v části ověřování toohello v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn více informací o klíči. [Vytvořte službu vyhledávání v portálu hello](search-create-service-portal.md) vysvětluje, jak používat adresu URL služby hello tooget a klíčové vlastnosti v žádosti o hello.

**Požadavek**

Hello syntaxe tělo žádosti je hello stejné jako u [vytvořit Indexer požadavky](#CreateIndexerRequestSyntax).

**Odpověď**

Pro úspěšné žádosti: 201 – vytvořeno Pokud byl nový indexer vytvořil a 204 ne obsahu Pokud existujícího indexeru byla aktualizována.

<a name="ListIndexers"></a>

## <a name="list-indexers"></a>Seznam indexery
Hello **seznamu indexery** operace vrátí hello seznam indexery ve službě Azure Search. 

    GET https://[service name].search.windows.net/indexers?api-version=[api-version]
    api-key: [admin key]


Hello `api-version` je vyžadován. verze preview Hello je `2015-02-28-Preview`. [Správa verzí Azure Search](https://msdn.microsoft.com/library/azure/dn864560.aspx) má podrobnosti a další informace o alternativní verze.

Hello `api-key` musí být klíče správce (jako klíč dotazu názvem na rozdíl od tooa). Najdete v části ověřování toohello v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn více informací o klíči. [Vytvořte službu vyhledávání v portálu hello](search-create-service-portal.md) vysvětluje, jak používat adresu URL služby hello tooget a klíčové vlastnosti v žádosti o hello.

**Odpověď**

Pro úspěšné žádosti: 200 OK.

Tady je odpovědi na příkladu:

    {
      "value" : [
      {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        ... other indexer properties
      }]
    }

Všimněte si, že můžete filtrovat hello odpovědi dolů toojust hello vlastnosti, které vás zajímají. Například pokud chcete pouze seznam názvů indexer, použijte hello OData `$select` dotazu možnost:

    GET /indexers?api-version=2014-10-20-Preview&$select=name

V takovém případě hello odpověď z hello výše příklad by měly vypadat následovně: 

    {
      "value" : [ { "name": "myindexer" } ]
    }

Toto je užitečné toosave šířky pásma, pokud máte spoustu indexery ve službě vyhledávání.

<a name="GetIndexer"></a>

## <a name="get-indexer"></a>Získat indexeru
Hello **získat Indexer** operaci získá hello indexer definice z Azure Search.

    GET https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

Hello `api-version` je vyžadován. verze preview Hello je `2015-02-28-Preview`. 

Hello `api-key` musí být klíče správce (jako klíč dotazu názvem na rozdíl od tooa). Najdete v části ověřování toohello v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn více informací o klíči. [Vytvořte službu vyhledávání v portálu hello](search-create-service-portal.md) vysvětluje, jak používat adresu URL služby hello tooget a klíčové vlastnosti v žádosti o hello.

**Odpověď**

Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.

odpověď Hello je podobné tooexamples v [požadavky příklad vytvoření Indexer](#CreateIndexerRequestExamples): 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }


<a name="DeleteIndexer"></a>

## <a name="delete-indexer"></a>Odstranit Indexer
Hello **odstranit Indexer** operace odebere indexer ze služby Azure Search.

    DELETE https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

Při odstranění indexer hello indexer spuštěních probíhá v daném čase spustí toocompletion, ale žádné další spuštěních bude naplánována s. Toouse pokusů, které neexistující indexer bude mít za následek stavový kód protokolu HTTP 404 nebyl nalezen. 

Hello `api-version` je vyžadován. verze preview Hello je `2015-02-28-Preview`. 

Hello `api-key` musí být klíče správce (jako klíč dotazu názvem na rozdíl od tooa). Najdete v části ověřování toohello v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn více informací o klíči. [Vytvořte službu vyhledávání v portálu hello](search-create-service-portal.md) vysvětluje, jak používat adresu URL služby hello tooget a klíčové vlastnosti v žádosti o hello.

**Odpověď**

: Stav 204 ne obsahu je vrácen kód pro úspěšné odpovědi.

<a name="RunIndexer"></a>

## <a name="run-indexer"></a>Spustit Indexer
V přidání toorunning pravidelně podle plánu, mohou být vyvolány indexer na vyžádání prostřednictvím hello **spustit Indexer** operace: 

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [admin key]

Hello `api-version` je vyžadován. verze preview Hello je `2015-02-28-Preview`. 

Hello `api-key` musí být klíče správce (jako klíč dotazu názvem na rozdíl od tooa). Najdete v části ověřování toohello v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn více informací o klíči. [Vytvořte službu vyhledávání v portálu hello](search-create-service-portal.md) vysvětluje, jak používat adresu URL služby hello tooget a klíčové vlastnosti v žádosti o hello.

**Odpověď**

Stavový kód: vrácen 202 platných pro úspěšné odpovědi.

<a name="GetIndexerStatus"></a>

## <a name="get-indexer-status"></a>Získat stav indexeru
Hello **získání stavu Indexer** operace načte hello historii aktuální stav a spuštění indexeru: 

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [admin key]


Hello `api-version` je vyžadován. verze preview Hello je `2015-02-28-Preview`. 

Hello `api-key` musí být klíče správce (jako klíč dotazu názvem na rozdíl od tooa). Najdete v části ověřování toohello v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn více informací o klíči. [Vytvořte službu vyhledávání v portálu hello](search-create-service-portal.md) vysvětluje, jak používat adresu URL služby hello tooget a klíčové vlastnosti v žádosti o hello.

**Odpověď**

Stavový kód: 200 OK pro úspěšné odpovědi.

text odpovědi Hello obsahuje informace o celkový stav indexer, hello posledního vyvolání indexeru, jakož i hello historii poslední indexer volání (pokud existuje). 

Ukázkový text odpovědi vypadá takto: 

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

**Stav indexeru**

Indexer stav může být jedna z hello následující hodnoty:

* `running`Označuje, že tento indexer hello běží normálně. Všimněte si, že některé hello indexer spuštěních pravděpodobně stále nedaří, tak, aby byl vhodné toocheck hello `lastResult` také vlastnost. 
* `error`Označuje, že tento indexer hello došlo k chybě, která nemůže být vyřešen bez lidského zásahu. Například přihlašovací údaje zdroje dat hello platnost vypršela, nebo hello schématu zdroje dat hello nebo hello cílový index se změnilo v ukončování způsobem. 

**Výsledek spuštění indexeru**

Výsledek spuštění indexeru obsahuje informace o provádění jedné indexer. Nejnovější výsledek Hello je prezentované jako hello `lastResult` vlastnost hello indexer stavu. Další poslední výsledky, pokud je k dispozici, se vrátí jako hello `executionHistory` vlastnost hello indexer stavu. 

Výsledek spuštění indexeru obsahuje hello následující vlastnosti: 

* `status`: hello stav spuštění. V tématu [stav spuštění indexeru](#IndexerExecutionStatus) níže podrobnosti. 
* `errorMessage`: chybovou zprávu pro selhání spuštění. 
* `startTime`: hello čas v UTC při spuštění tohoto spuštění.
* `endTime`: hello čas v UTC při spuštění tohoto skončila. Tato hodnota není nastavená, pokud provádění hello stále probíhá.
* `errors`: pole chyb na úrovni položek, pokud existuje. Každá položka obsahuje klíč dokumentu (`key` vlastnosti) a chybovou zprávou (`errorMessage` vlastnost). 
* `itemsProcessed`: počet položky zdroje dat (například řádky tabulky), které hello tooindex indexer pokus při spuštění tohoto hello. 
* `itemsFailed`: počet položek, které došlo k chybě během spuštění tohoto hello.  
* `initialTrackingState`: vždy `null` pro první spuštění indexeru hello na zdroj dat hello používaný není povoleno nebo pokud změny dat hello sledování zásad. Pokud je tato zásada je povoleno, v dalších spuštěních tato hodnota označuje hello první (nejnižší) sledování změn hodnotu zpracovává spuštění tohoto. 
* `finalTrackingState`: vždy `null` Pokud změny zásad sledování hello dat není povoleno pro zdroj dat hello používá. Označuje, jinak hodnota hello nejnovější (nejvyšší) sledování změn hodnota úspěšně zpracoval spuštění tohoto. 

<a name="IndexerExecutionStatus"></a>
**Stav spuštění indexeru**

Stav spuštění indexeru zaznamená hello stav spuštění jedné indexer. Může mít hello následující hodnoty:

* `success`Označuje, že spuštění indexeru hello byla úspěšně dokončena.
* `inProgress`Označuje, že spuštění indexeru hello je v průběhu. 
* `transientFailure`Označuje, že se nezdařilo spuštění indexeru. V tématu `errorMessage` vlastnost podrobnosti. Hello selhání může nebo nemusí vyžadovat lidského zásahu toofix – například opravě nekompatibility schématu mezi zdrojem dat hello a hello cílový index vyžaduje akce uživatele, když s prodlevou zdroj dočasná data, která nemá. Indexer volání bude pokračovat podle plánu, pokud je k dispozici. 
* `persistentFailure`Označuje, že tento indexer hello selhal způsobem, který vyžaduje lidského zásahu. Naplánované indexer spuštěních zastaví. Po vyřešení problému hello, použijte resetovat Indexer API toorestart hello naplánované spuštění. 
* `reset`Označuje, že tento indexer hello resetoval tooReset volání rozhraní API Indexer (viz níže). 

<a name="ResetIndexer"></a>

## <a name="reset-indexer"></a>Resetovat Indexer
Hello **resetovat Indexer** operace obnoví stavy, které jsou přidružené k hello indexer sledování změn hello. To vám umožní tootrigger od úplného začátku přeindexování (například pokud došlo ke změně vašeho schématu zdroje dat), nebo zásady detekce toochange hello data změn pro zdroj dat přidružený hello indexer.   

    POST https://[service name].search.windows.net/indexers/[indexer name]/reset?api-version=[api-version]
    api-key: [admin key]

Hello `api-version` je vyžadován. verze preview Hello je `2015-02-28-Preview`. 

Hello `api-key` musí být klíče správce (jako klíč dotazu názvem na rozdíl od tooa). Najdete v části ověřování toohello v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn více informací o klíči. [Vytvořte službu vyhledávání v portálu hello](search-create-service-portal.md) vysvětluje, jak používat adresu URL služby hello tooget a klíčové vlastnosti v žádosti o hello.

**Odpověď**

Stavový kód: 204 žádný obsah pro úspěšné odpovědi.

## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>Mapování mezi SQL datové typy a typy dat vyhledávání systému Azure
<table style="font-size:12">
<tr>
<td>Datový typ SQL.</td>    
<td>Cílový index povolené typy polí</td>
<td>Poznámky</td>
</tr>
<tr>
<td>Bit</td>
<td>Edm.Boolean Edm.String</td>
<td></td>
</tr>
<tr>
<td>int, smallint, tinyint</td>
<td>Edm.String Edm.Int32, Edm.Int64,</td>
<td></td>
</tr>
<tr>
<td>bigint</td>
<td>Edm.Int64 Edm.String</td>
<td></td>
</tr>
<tr>
<td>skutečné, float</td>
<td>Edm.Double Edm.String</td>
<td></td>
</tr>
<tr>
<td>Smallmoney peníze<br>Decimal<br>číselné
</td>
<td>Edm.String</td>
<td>Vyhledávání systému Azure nepodporuje převod decimal typy do Edm.Double, protože by to ztratit přesnost
</td>
</tr>
<tr>
<td>Char, nchar, varchar, nvarchar</td>
<td>Edm.String<br/>Collection(Edm.String)</td>
<td>V tématu [funkce mapování polí](#FieldMappingFunctions) v tomto dokumentu podrobnosti o tom tootransform sloupec řetězce do Collection(Edm.String)</td>
</tr>
<tr>
<td>smalldatetime, datetime, datetime2, date, datetimeoffset</td>
<td>Edm.DateTimeOffset Edm.String</td>
<td></td>
</tr>
<tr>
<td>uniqueidentifer</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>Geography</td>
<td>Edm.GeographyPoint</td>
<td>Jsou podporovány pouze geography instance typu bodu s SRID 4326 (což je výchozí hello)</td>
</tr>
<tr>
<td>ROWVERSION</td>
<td>Není k dispozici</td>
<td>Verze řádku sloupce nelze uložit do indexu vyhledávání hello, ale mohou být použity pro sledování změn</td>
</tr>
<tr>
<td>čas, časový interval<br>binary, varbinary, image,<br>XML, geometry, typy CLR</td>
<td>Není k dispozici</td>
<td>Nepodporuje se</td>
</tr>
</table>

## <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>Mapování mezi JSON datové typy a typy dat vyhledávání systému Azure
<table style="font-size:12">
<tr>
<td>JSON datového typu</td>    
<td>Cílový index povolené typy polí</td>
<td>Poznámky</td>
</tr>
<tr>
<td>BOOL</td>
<td>Edm.Boolean Edm.String</td>
<td></td>
</tr>
<tr>
<td>Integrální čísla</td>
<td>Edm.String Edm.Int32, Edm.Int64,</td>
<td></td>
</tr>
<tr>
<td>Čísla s plovoucí desetinnou čárkou</td>
<td>Edm.Double Edm.String</td>
<td></td>
</tr>
<tr>
<td>Řetězec</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>pole primitivní typy, například ["a", "b", "c"]</td>
<td>Collection(Edm.String)</td>
<td></td>
</tr>
<tr>
<td>Řetězce, které vypadají podobně jako kalendářní data</td>
<td>Edm.DateTimeOffset Edm.String</td>
<td></td>
</tr>
<tr>
<td>GeoJSON bodu objekty</td>
<td>Edm.GeographyPoint</td>
<td>GeoJSON body jsou objekty JSON v hello následující formát: {"typ": "Místo", "coordinates": [dlouhý a lat]} </td>
</tr>
<tr>
<td>Jiné objekty JSON</td>
<td>Není k dispozici</td>
<td>Nepodporuje; Vyhledávání systému Azure aktuálně podporuje jenom primitivní typy a kolekcí řetězců</td>
</tr>
</table>
