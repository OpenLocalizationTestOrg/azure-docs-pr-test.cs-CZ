---
title: "aaaIndexing úložiště tabulek Azure s Azure Search | Microsoft Docs"
description: "Zjistěte, jak tooindex data uložená ve službě Azure Table storage s Azure Search"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 1cc27411-d0cc-40ed-8aed-c7cb9ab402b9
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/10/2017
ms.author: eugenesh
ms.openlocfilehash: abb01a4d807cede310246b34775d8059fceb4456
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="index-azure-table-storage-with-azure-search"></a>Index úložiště tabulek Azure s Azure Search
Tento článek ukazuje, jak toouse Azure Search tooindex data uložená ve službě Azure Table storage.

## <a name="set-up-azure-table-storage-indexing"></a>Nastavení Azure Table storage indexování

Indexer Azure Table storage můžete nastavit pomocí těchto prostředků:

* [Azure Portal](https://ms.portal.azure.com)
* Služba Azure Search [rozhraní REST API](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* Služba Azure Search [sady .NET SDK](https://aka.ms/search-sdk)

Zde ukážeme hello toku pomocí hello REST API. 

### <a name="step-1-create-a-datasource"></a>Krok 1: Vytvoření zdroje dat

Určuje zdroj dat, které tooindex dat, hello vyžadované přihlašovací údaje tooaccess hello dat a hello zásady, které umožňují tooefficiently Azure Search identifikovat změny v datech hello.

Pro tabulku indexování, musí mít zdroj dat hello hello následující vlastnosti:

- **název** je jedinečný název hello hello zdroje dat v rámci služby search.
- **typ** musí být `azuretable`.
- **přihlašovací údaje** parametr obsahuje hello připojovací řetězce k účtu úložiště. V tématu hello [zadejte přihlašovací údaje](#Credentials) podrobnosti.
- **kontejner** nastaví hello název tabulky a volitelné dotazů.
    - Zadejte název tabulky hello pomocí hello `name` parametr.
    - Volitelně můžete zadat dotaz pomocí hello `query` parametr. 

> [!IMPORTANT] 
> Kdykoli je to možné, použijte filtr na PartitionKey pro dosažení vyššího výkonu. Další dotaz nemá prohledání úplnou tabulky, výsledkem je nízký výkon pro rozsáhlé tabulky. V tématu hello [faktory ovlivňující výkon](#Performance) části.


toocreate zdroj dat:

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-table", "query" : "PartitionKey eq '123'" }
    }   

Další informace o hello vytvořit zdroj dat rozhraní API najdete v tématu [vytvořit zdroj dat](https://docs.microsoft.com/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>
#### <a name="ways-toospecify-credentials"></a>Způsoby toospecify pověření ####

Můžete zadat přihlašovací údaje hello hello tabulky v jednom z těchto způsobů: 

- **Úplný přístup připojovací řetězce k účtu úložiště**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>` získáte hello připojovací řetězec z hello portálu Azure tak, že budete toohello **okně účtu úložiště** > **nastavení**   >  **Klíče** (pro účty úložiště classic) nebo **nastavení** > **přístupové klíče** (pro prostředků Azure Účty správce úložiště).
- **Účet úložiště sdíleného přístupu podpis připojovací řetězec**: `TableEndpoint=https://<your account>.table.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<hello signature>&spr=https&se=<hello validity end time>&srt=co&ss=t&sp=rl` hello sdílený přístupový podpis by měl mít hello seznamu a oprávnění ke čtení v kontejnery (tabulky v tomto případě) a objekty (řádky tabulky).
-  **Tabulka sdílený přístupový podpis**: `ContainerSharedAccessUri=https://<your storage account>.table.core.windows.net/<table name>?tn=<table name>&sv=2016-05-31&sig=<hello signature>&se=<hello validity end time>&sp=r` hello sdílený přístupový podpis musí mít oprávnění dotazu (čtení) v tabulce hello.

Další informace o sdílené úložiště přístup podpisy, najdete v části [pomocí sdílené přístupové podpisy](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Pokud používáte sdílený přístupový podpis pověření, budete potřebovat přihlašovací údaje zdroje hello tooupdate pravidelně s prodlouženou platností podpisy tooprevent vypršením jejich platnosti. Pokud přihlašovací údaje sdílený přístupový podpis vyprší, hello indexer nezdaří a zobrazí se chybová zpráva příliš "zadané v připojovacím řetězci hello přihlašovací údaje jsou neplatné nebo vypršela platnost."  

### <a name="step-2-create-an-index"></a>Krok 2: Vytvoření indexu
Hello index určuje hello pole v dokumentu, hello atributy a jiné vytvoří tento tvar hello vyhledáváním.

toocreate index:

    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "key", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "SomeColumnInMyTable", "type": "Edm.String", "searchable": true }
          ]
    }

Další informace o vytváření indexů, najdete v části [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index).

### <a name="step-3-create-an-indexer"></a>Krok 3: Vytvořením indexeru.
Indexer zdroje dat se připojí pomocí cílový index vyhledávání a poskytuje plánu aktualizace dat hello tooautomate. 

Po vytvoření hello index a zdroj dat, jste připravené toocreate hello indexer:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "table-indexer",
      "dataSourceName" : "table-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Indexer se spustí každé dvě hodiny. (hello interval plánování je nastavena příliš "PT2H".) toorun indexer každých 30 minut nastavit hello interval příliš "PT30M". Hello nejkratší podporovaný interval je pět minut. plán Hello je volitelná. Pokud tento parametr vynechán, indexer se spustí pouze jednou, když je vytvořena. Indexer však můžete spustit na vyžádání kdykoli.   

Další informace o hello Indexer vytvořit rozhraní API najdete v tématu [vytvořit Indexer](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

## <a name="deal-with-different-field-names"></a>Řešení s názvy různých polí
V některých případech se liší od názvů vlastností hello v tabulce hello názvy polí v existující index. Názvy vlastností hello toomap pro pole mapování z názvů polí toohello hello tabulku můžete použít v indexu vyhledávání. toolearn Další informace o mapování polí, najdete v části [mapování polí indexer Azure Search přemostění hello rozdíly mezi zdroje dat a vyhledávání indexů](search-indexer-field-mappings.md).

## <a name="handle-document-keys"></a>Popisovač dokumentu klíče
Ve službě Azure Search je klíč dokumentu hello jednoznačně identifikuje dokumentu. Každý index hledání musí mít přesně jeden klíčové pole typu `Edm.String`. Hello klíčové pole je povinné pro každý dokument, který je přidáván toohello index. (Ve skutečnosti jeho hello pouze povinné pole.)

Vzhledem k tomu, že řádky tabulky složené klíče, vygeneruje Azure Search syntetické pole s názvem `Key` , je tvořen oddílu klíč a řádek klíčové hodnoty. Například pokud řádek je klíč oddílu je `PK1` a RowKey `RK1`, pak hello `Key` hodnotu pole je `PK1RK1`.

> [!NOTE]
> Hello `Key` hodnota může obsahovat znaky, které jsou v dokumentu klíče, jako je například pomlčky neplatné. Neplatné znaky můžete řešit pomocí hello `base64Encode` [pole mapování funkce](search-indexer-field-mappings.md#base64EncodeFunction). Pokud to uděláte, mějte na paměti, tooalso použijte adresu URL bezpečných Base64 kódování při předávání dokumentu klíče v rozhraní API volá například vyhledávání.
>
>

## <a name="incremental-indexing-and-deletion-detection"></a>Přírůstkové indexování a odstranění duplicit
Při nastavování tabulka indexer toorun podle plánu, ho Reindexuje pouze nové nebo aktualizované řádky určeného řádku `Timestamp` hodnotu. Nemáte toospecify zásady detekce změn. Přírůstkové indexování se povolí automaticky.

tooindicate určitá dokumenty musí být odstraněn z hello indexu můžete použít strategie obnovitelného odstranění. Místo odstraněním řádku, přidejte vlastnost tooindicate, který se má odstranit a nastavit zásadu obnovitelného odstranění detekce na zdroji dat, hello. Například následující zásady hello zvažuje, je-li vlastnost hello řádek odstranění řádku `IsDeleted` s hodnotou hello `"true"`:

    PUT https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "table name", "query" : "<query>" },
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }   

<a name="Performance"></a>
## <a name="performance-considerations"></a>Otázky výkonu

Ve výchozím nastavení používá Azure Search hello následující filtr dotazu: `Timestamp >= HighWaterMarkValue`. Vzhledem k tomu tabulky Azure nemáte sekundárního indexu na hello `Timestamp` pole, tento typ dotazu vyžaduje prohledání úplnou tabulky a je proto pomalé pro rozsáhlé tabulky.


Tady jsou dvě možné přístupy k zlepšení výkonu indexování tabulky. Oba tyto přístupy spoléhají na použití tabulky oddílů: 

- Pokud vaše data můžou být dělené přirozeně do několika oddílů rozsahy, vytvořte zdroj dat a odpovídající indexer pro každý oddíl rozsah. Každý indexer teď má tooprocess pouze konkrétní oddíl rozsah, což vede k lepší výkon dotazů. Pokud má hello data, která potřebují toobe indexované malý počet pevné oddílů, ještě lepší: každý indexer pouze nemá kontrolu oddílu. Například toocreate zdroj dat pro zpracování rozsah oddílu s klíče z `000` příliš`100`, použít dotaz takto: 
    ```
    "container" : { "name" : "my-table", "query" : "PartitionKey ge '000' and PartitionKey lt '100' " }
    ```

- Pokud vaše data je rozdělena na oddíly podle času (například můžete vytvořit nový oddíl každý den nebo týden), zvažte hello následující postup: 
    - Použít dotaz formuláře hello: `(PartitionKey ge <TimeStamp>) and (other filters)`. 
    - Indexer průběh sledovat pomocí [získání rozhraní indexeru stav API](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status)a pravidelně aktualizovat hello `<TimeStamp>` podmínku hello dotazu na základě hello nejnovější úspěšné horní mez hodnoty. 
    - Tento přístup Pokud potřebujete tootrigger novou dokončení indexaci je nutné tooreset hello zdroje dat dotazu v přidání tooresetting hello indexer. 


## <a name="help-us-make-azure-search-better"></a>Pomozte nám vylepšit Azure Search
Pokud máte žádosti o funkce nebo vylepšení nápady, odesílat je na našem [UserVoice lokality](https://feedback.azure.com/forums/263029-azure-search/).
