---
title: aaaMove tooand dat z SQL serveru | Microsoft Docs
description: "Další informace o způsobu toomove data do nebo ze systému SQL Server databáze tedy místní nebo virtuální počítač Azure pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 864ece28-93b5-4309-9873-b095bbe6fedd
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jingwang
ms.openlocfilehash: f0cccf56a670e62ec893d75052a81eb26d562050
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-sql-server-on-premises-or-on-iaas-azure-vm-using-azure-data-factory"></a>Přesunutí dat tooand z místní SQL Server nebo na IaaS (virtuální počítač Azure) pomocí Azure Data Factory
Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data z databáze SQL Server na místě. Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello. 

## <a name="supported-scenarios"></a>Podporované scénáře
Může kopírovat data **z databáze systému SQL Server** toohello následující úložišť dat:

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

Data můžete zkopírovat z hello následující úložišť dat **databáze systému SQL Server tooa**:

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="supported-sql-server-versions"></a>Podporované verze systému SQL Server
Tato podpora konektoru systému SQL Server kopírování dat z / toohello následující verze instance hostovaná místně nebo v Azure IaaS pomocí ověřování SQL a ověřování systému Windows: SQL Server 2016, SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008, SQL Server 2005

## <a name="enabling-connectivity"></a>Povolení připojení
Koncepty Hello a kroky potřebné pro připojení s SQL serveru hostované místně nebo v Azure IaaS (infrastruktura jako služba) virtuálních počítačů jsou hello stejné. V obou případech musíte toouse Brána pro správu dat pro připojení k síti.

V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) toolearn článek o Brána pro správu dat a podrobné pokyny k nastavení hello brány. Nastavení instance brány je nezbytný předpoklad pro připojení k systému SQL Server.

Zatímco můžete bránu nainstalovat hello stejné na místní počítač nebo cloudové instance virtuálního počítače jako hello systému SQL Server pro lepší výkon, doporučujeme vám nainstalovat na samostatné počítače. S hello brány a SQL Server na samostatné počítače snižuje kolize prostředků.

## <a name="getting-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování, který přesouvá data z databáze SQL serveru místní pomocí různých nástrojů nebo rozhraní API.

Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.

Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**. V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování. 

Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello: 

1. Vytvoření **objekt pro vytváření dat**. Objekt pro vytváření dat může obsahovat jeden nebo víc kanálů. 
2. Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory. Například pokud data kopírujete tooan databáze systému SQL Server úložiště objektů blob v Azure, vytvoříte dvě propojené služby toolink databáze systému SQL Server a účet úložiště Azure tooyour služby data factory. Vlastnosti propojené služby, které jsou specifické tooSQL databáze serveru, najdete v části [propojené vlastnosti služby](#linked-service-properties) části. 
3. Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování. V příkladu hello uvedených v posledním kroku hello vytvořit tabulku datovou sadu toospecify hello SQL v databázi systému SQL Server, který obsahuje vstupní data hello. Vytvoření kontejneru objektů blob hello toospecify s jinou datovou sadu a hello složky, která obsahuje hello data zkopírovaných z hello databáze systému SQL Server. Vlastnosti datové sady, které jsou konkrétní tooSQL databáze serveru, najdete v části [vlastnosti datové sady](#dataset-properties) části.
4. Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup. V příkladu hello již bylo zmíněno dříve použijete SqlSource jako zdroj a BlobSink jako jímku pro aktivitu kopírování hello. Podobně pokud zkopírujete z Azure Blob Storage tooSQL databáze serveru, použijte BlobSource a SqlSink v aktivitě kopírování hello. Vlastnosti aktivity kopírování, které jsou specifické tooSQL databáze serveru, najdete v části [zkopírovat vlastnosti aktivity](#copy-activity-properties) části. Podrobnosti o způsobu toouse úložiště dat jako zdroj nebo jímka klikněte na tlačítko hello odkaz v předchozí části hello pro data store. 

Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello). Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.  Ukázky s definicemi JSON entit služby Data Factory, které jsou používané toocopy data do nebo z místní databáze systému SQL Server naleznete v části [JSON příklady](#json-examples-for-copying-data-from-and-to-sql-server) tohoto článku. 

Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooSQL serveru: 

## <a name="linked-service-properties"></a>Vlastnosti propojené služby
Vytvoření propojené služby typu **onpremisessqlserver** toolink služby místní systém SQL Server databáze tooa data factory. Hello následující tabulka obsahuje popis služby SQL serveru propojená konkrétní tooon místní elementy JSON.

Hello následující tabulka obsahuje popis pro konkrétní tooSQL elementy JSON serveru propojené služby.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| type |vlastnost typu Hello by měla být nastavena na: **onpremisessqlserver**. |Ano |
| připojovací řetězec |Zadejte požadované informace connectionString tooconnect toohello místní databáze SQL serveru pomocí ověřování SQL nebo ověřování systému Windows. |Ano |
| gatewayName |Název hello brány, kterou služba Data Factory hello měli používat toohello tooconnect, místní databázi systému SQL Server. |Ano |
| uživatelské jméno |Zadejte uživatelské jméno, pokud používáte ověřování systému Windows. Příklad: **domainname\\uživatelské jméno**. |Ne |
| heslo |Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello. |Ne |

Můžete šifrovat přihlašovací údaje pomocí hello **New-AzureRmDataFactoryEncryptValue** rutiny a použijte je v hello připojovací řetězec, jak ukazuje následující příklad hello (**EncryptedCredential** vlastnost):  

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

### <a name="samples"></a>Ukázky
**JSON pro pomocí ověřování SQL.**

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties":
    {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
**JSON pro použití ověřování systému Windows**

Brána pro správu dat se zosobnit hello zadaný uživatel účet tooconnect toohello místní databázi systému SQL Server. 

```json
{
     "Name": " MyOnPremisesSQLDB",
     "Properties":
     {
         "type": "OnPremisesSqlServer",
         "typeProperties": {
             "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
             "username": "<domain\\username>",
             "password": "<password>",
             "gatewayName": "<gateway name>"
        }
     }
}
```

## <a name="dataset-properties"></a>Vlastnosti datové sady
V hello ukázky jste použili datovou sadu typu **SqlServerTable** toorepresent tabulky v databázi systému SQL Server.  

Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku. Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (SQL Server, objektů blob v Azure, Azure table atd.).

část rámci typeProperties Hello se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello. Hello **rámci typeProperties** části pro datovou sadu hello typu **SqlServerTable** má hello následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| tableName |Název hello tabulku nebo zobrazení v hello instance databáze SQL serveru, který propojená služba odkazuje. |Ano |

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit
Pokud přesouváte data z databáze systému SQL Server, nastavíte typ zdroje hello v aktivitě kopírování hello příliš**SqlSource**. Podobně pokud přesouváte databázi systému SQL Server tooa dat, nastavíte typ jímky hello v aktivitě kopírování hello příliš**SqlSink**. Tato část obsahuje seznam vlastností, které jsou podporované SqlSource a SqlSink.

Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku. Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.

> [!NOTE]
> Hello aktivity kopírování přijímá pouze jeden vstup a vytváří jenom jeden výstup.

Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části hello hello aktivity se liší podle každý typ aktivity. Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.

### <a name="sqlsource"></a>SqlSource
Pokud zdroj v aktivitě kopírování je typu **SqlSource**, hello následující vlastnosti jsou k dispozici v **rámci typeProperties** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| sqlReaderQuery |Použijte data tooread hello vlastního dotazu. |Řetězec dotazu SQL. Příklad: vybrat * z MyTable. Může odkazovat více tabulek z databáze hello odkazuje hello vstupní datové sady. Pokud není zadaný, hello příkaz jazyka SQL, která se provedla: Vyberte možnost z MyTable. |Ne |
| sqlReaderStoredProcedureName |Název hello uložené procedury, která čte data z hello zdrojové tabulky. |Název hello uložené procedury. Hello poslední příkaz jazyka SQL musí být příkaz SELECT v hello uložené procedury. |Ne |
| storedProcedureParameters |Parametry pro hello uložené procedury. |Páry název/hodnota. Názvy a malá a velká písmena parametry musí odpovídat názvům hello a malá a velká písmena parametry hello uložené procedury. |Ne |

Pokud hello **sqlReaderQuery** je zadán pro hello SqlSource, hello aktivity kopírování spouští tento dotaz hello databáze systému SQL Server zdrojová tooget hello data.

Alternativně můžete zadat uložené procedury zadáním hello **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud hello uložená procedura používá parametry).

Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, hello sloupce definovaný v oddílu Struktura hello jsou použité toobuild dotaz select toorun proti hello databáze systému SQL Server. Pokud definice datové sady hello nemá hello struktura, vyberou se všechny sloupce z tabulky hello.

> [!NOTE]
> Při použití **sqlReaderStoredProcedureName**, stále potřebujete toospecify hodnotu pro hello **tableName** vlastnost v datové sadě hello JSON. Neexistují žádné ověření, ale adresovat této tabulky.

### <a name="sqlsink"></a>SqlSink
**SqlSink** podporuje hello následující vlastnosti:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| writeBatchTimeout |Doba pro toocomplete operaci vložení dávky hello Počkejte, než vyprší časový limit. |Časový interval<br/><br/> Příklad: "00: 30:00" (30 minut). |Ne |
| writeBatchSize |Pokud velikost vyrovnávací paměti hello dosáhne writeBatchSize vkládá data do tabulky SQL hello. |Celé číslo (počet řádků) |Ne (výchozí: 10000) |
| sqlWriterCleanupScript |Zadejte dotaz pro aktivitu kopírování tooexecute tak, aby se vyčistit data určitý řez. Další informace najdete v tématu [opakovatelných kopie](#repeatable-copy) části. |Příkaz dotazu. |Ne |
| sliceIdentifierColumnName |Zadejte název sloupce pro aktivitu kopírování toofill s identifikátorem automaticky generovány řez, což je použité tooclean data určitý řez, pokud znovu spustit. Další informace najdete v tématu [opakovatelných kopie](#repeatable-copy) části. |Název sloupce sloupce s datovým typem binary(32). |Ne |
| sqlWriterStoredProcedureName |Název hello uložené procedury upserts (aktualizace nebo vložení) dat do cílové tabulky hello. |Název hello uložené procedury. |Ne |
| storedProcedureParameters |Parametry pro hello uložené procedury. |Páry název/hodnota. Názvy a malá a velká písmena parametry musí odpovídat názvům hello a malá a velká písmena parametry hello uložené procedury. |Ne |
| sqlWriterTableType |Zadejte toobe název typu tabulky používán hello uložené procedury. Aktivita kopírování zpřístupní přesouvání dat hello v dočasné tabulce s tímto typem tabulky. Uložená procedura kódu můžete pak sloučit data hello kopírovány s existujícími daty. |Zadejte název tabulky. |Ne |


## <a name="json-examples-for-copying-data-from-and-toosql-server"></a>Příklady JSON ke kopírování dat z a tooSQL serveru
Hello následující příklady poskytují definice JSON ukázka používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Dobrý den, jak následující ukázky zobrazit toocopy tooand dat z SQL serveru a Azure Blob Storage. Nicméně je možné zkopírovat data **přímo** ze všech zdrojů tooany z hello jímky uvádí [zde](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.     

## <a name="example-copy-data-from-sql-server-tooazure-blob"></a>Příklad: Kopírování dat z SQL serveru tooAzure objektů Blob
Následující ukázka ukazuje Hello:

1. Propojené služby typu [onpremisessqlserver](#linked-service-properties).
2. Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Vstup [datovou sadu](data-factory-create-datasets.md) typu [SqlServerTable](#dataset-properties).
4. Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Hello [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [SqlSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Ukázka Hello zkopíruje data časové řady z tabulky tooan SQL Server objektů blob v Azure každou hodinu. Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.

Jako první krok nastavte Brána pro správu dat hello. Hello pokyny jsou v hello [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.

**Služba SQL Server propojené**
```json
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```
**Objekt Blob propojená služba Azure storage**

```json
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Vstupní datové sady SQL Server**

Ukázka Hello předpokládá jste vytvořili tabulku "MyTable" v systému SQL Server a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady. Můžete dotazovat přes více tabulek v rámci hello stejnou databázi pomocí jednu datovou sadu, ale jedné tabulky musí být použita pro typeProperty tableName hello datovou sadu.

Nastavení "externí": "PRAVDA" informuje služba Data Factory tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

```json
{
  "name": "SqlServerInput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
**Výstupní datovou sadu objektů Blob v Azure**

Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1). Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány. Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**Kanál s aktivitou kopírování**

Hello kanál obsahuje aktivitu kopírování, je nakonfigurovaná toouse tyto vstupní a výstupní datové sady a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**SqlSource** a **podřízený** je typ nastaven příliš**BlobSink**. Dotaz SQL Hello zadaný pro hello **SqlReaderQuery** vlastnost vybere hello data v hello za hodinu toocopy.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2016-06-01T18:00:00",
    "end":"2016-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "SqlServertoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": " SqlServerInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
     ]
   }
}
```
V tomto příkladu **sqlReaderQuery** pro hello SqlSource je zadána. Hello aktivity kopírování spouští tento dotaz hello data hello tooget zdrojové databáze systému SQL Server. Alternativně můžete zadat uložené procedury zadáním hello **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud hello uložená procedura používá parametry). Hello sqlReaderQuery může odkazovat více tabulek v rámci hello databáze odkazuje hello vstupní datové sady. Není omezený tooonly hello tabulky nastavena jako hello typeProperty tableName datové sady.

Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, hello sloupce definovaný v oddílu Struktura hello jsou použité toobuild dotaz select toorun proti hello databáze systému SQL Server. Pokud definice datové sady hello nemá hello struktura, vyberou se všechny sloupce z tabulky hello.

V tématu hello [zdroje Sql](#sqlsource) části a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) hello seznam vlastnostech podporovaných zprostředkovatelem SqlSource a BlobSink.

## <a name="example-copy-data-from-azure-blob-toosql-server"></a>Příklad: Kopírování dat z Azure Blob tooSQL serveru
Následující ukázka ukazuje Hello:

1. Hello propojené služby typu [onpremisessqlserver](#linked-service-properties).
2. Hello propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Výstup [datovou sadu](data-factory-create-datasets.md) typu [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).
5. Hello [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) a [SqlSink](#sql-server-copy-activity-type-properties).

Ukázka Hello zkopíruje data časové řady z tabulky serveru SQL Server tooa objektů blob v Azure každou hodinu. Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.

**Služba SQL Server propojené**

```json
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```
**Objekt Blob propojená služba Azure storage**

```json
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Azure vstupní datovou sadu objektu Blob**

Data je převzata z nového objektu blob každou hodinu (frekvence: hodiny, interval: 1). název a cesta k souboru složky Hello pro objekt blob hello se vyhodnocují dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány. Cesta ke složce Hello používá rok, měsíc a den součástí hello počáteční čas a název souboru používá hello hodinu součástí hello počáteční čas. "externí": "PRAVDA" nastavení informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
**Výstupní datové sady SQL Server**

Ukázka Hello zkopíruje data tooa tabulku s názvem "MyTable" v systému SQL Server. Vytvoření tabulky hello v systému SQL Server s hello stejný počet sloupců, podle očekávání toocontain soubor Blob CSV hello. Přidávání řádků tabulky toohello každou hodinu.

```json
{
  "name": "SqlServerOutput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**Kanál s aktivitou kopírování**

Hello kanál obsahuje aktivitu kopírování, je nakonfigurovaná toouse tyto vstupní a výstupní datové sady a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**BlobSource** a **podřízený** je typ nastaven příliš**SqlSink**.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQL",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": " SqlServerOutput "
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
          },
          "sink": {
            "type": "SqlSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
      ]
   }
}
```

## <a name="troubleshooting-connection-issues"></a>Odstraňování potíží s připojením
1. Konfigurace vzdáleného připojení tooaccept systému SQL Server. Spusťte **SQL Server Management Studio**, klikněte pravým tlačítkem na **server**a klikněte na tlačítko **vlastnosti**. Vyberte **připojení** ze seznamu hello a zkontrolujte **povolit vzdálená připojení toohello server**.

    ![Povolit vzdálená připojení](./media/data-factory-sqlserver-connector/AllowRemoteConnections.png)

    V tématu [konfigurace vzdáleného přístupu hello možnosti konfigurace serveru](https://msdn.microsoft.com/library/ms191464.aspx) podrobné pokyny.
2. Spusťte **Správce konfigurace systému SQL Server**. Rozbalte položku **konfigurace sítě serveru SQL Server** pro hello instance je chcete a vyberte **protokoly pro MSSQLSERVER**. Měli byste vidět protokolů v pravém podokně hello. Povolte protokol TCP/IP kliknutím pravým tlačítkem na **TCP/IP** a kliknutím na **povolit**.

    ![Povolte protokol TCP/IP](./media/data-factory-sqlserver-connector/EnableTCPProptocol.png)

    V tématu [povolit nebo zakázat síťový protokol serveru](https://msdn.microsoft.com/library/ms191294.aspx) podrobnosti a alternativní způsoby povolení protokolu TCP/IP.
3. Ve stejném okně hello, dvakrát klikněte na **TCP/IP** toolaunch **vlastností protokolu TCP/IP** okno.
4. Přepínač toohello **IP adresy** kartě. Projděte dolů toosee **IPAll** části. Zapište hello ** TCP Port ** (výchozí hodnota je **1433**).
5. Vytvoření **pravidlo pro hello brány Windows Firewall** pro příchozí provoz tooallow hello počítače pomocí tohoto portu.  
6. **Ověření připojení**: tooconnect toohello pomocí plně kvalifikovaného názvu SQL serveru použít SQL Server Management Studio z jiný počítač. Například: "<machine>.<domain>. Corp.<company>.com, 1433. "

   > [!IMPORTANT]

   > V tématu [přesun dat mezi místní zdroje a hello cloudu s Brána pro správu dat](data-factory-move-data-between-onprem-and-cloud.md) podrobné informace.
   >
   > V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.
   >
   >


## <a name="identity-columns-in-hello-target-database"></a>Sloupce identity v hello cílová databáze
Tato část poskytuje příklad, který kopíruje data ze zdrojové tabulky s žádné identity sloupec tooa cílové tabulky se sloupcem identity.

**Zdrojová tabulka:**

```sql
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```
**Cílové tabulky:**

```sql
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```

Všimněte si, že hello cílová tabulka obsahuje sloupec identity.

**Definice JSON datové sady zdroje**

```json
{
    "name": "SampleSource",
    "properties": {
        "published": false,
        "type": " SqlServerTable",
        "linkedServiceName": "TestIdentitySQL",
        "typeProperties": {
            "tableName": "SourceTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```
**Cílový definici JSON datové sady**

```json
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "TestIdentitySQLSource",
        "typeProperties": {
            "tableName": "TargetTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }
}
```

Všimněte si, že jako zdrojové a cílové tabulky jiné schéma (cíl má sloupec s identitou). V tomto scénáři budete potřebovat toospecify **struktura** vlastnost v definici datové sady cíl hello, který neobsahuje sloupec identity hello.

## <a name="invoke-stored-procedure-from-sql-sink"></a>Volání uložené procedury jímku SQL
V tématu [vyvolat uloženou proceduru SQL jímka v aktivitě kopírování](data-factory-invoke-stored-procedure-from-copy-activity.md) článku příklad volání uložené procedury z jímku SQL při aktivitě kopírování kanálu.

## <a name="type-mapping-for-sql-server"></a>Mapování typu pro SQL server
Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku hello aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello následující přístup krok 2:

1. Převod z typu too.NET typy nativní zdroje
2. Převést typ jímky toonative typ rozhraní .NET

Při přesunutí dat příliš & ze serveru SQL server hello se používají následující mapování z typu too.NET typ SQL a naopak.

mapování Hello je stejný jako hello mapování datového typu aplikace SQL Server pro technologii ADO.NET.

| Typ databázového stroje SQL Server | Typ rozhraní .NET framework |
| --- | --- |
| bigint |Int64 |
| Binární |Byte] |
| Bit |Logická hodnota |
| Char |Řetězec, Char] |
| Datum |Data a času |
| Data a času |Data a času |
| datetime2 |Data a času |
| Datový typ DateTimeOffset |Datový typ DateTimeOffset |
| Decimal |Decimal |
| Atribut FILESTREAM (varbinary(max)) |Byte] |
| Plovoucí desetinná čárka |Double |
| Bitové kopie |Byte] |
| celá čísla |Int32 |
| peníze |Decimal |
| nchar |Řetězec, Char] |
| ntext |Řetězec, Char] |
| číselné |Decimal |
| nvarchar |Řetězec, Char] |
| skutečné |Jeden |
| ROWVERSION |Byte] |
| smalldatetime |Data a času |
| smallint |Int16 |
| Smallmoney |Decimal |
| SQL_VARIANT |Objekt * |
| Text |Řetězec, Char] |
| time |Časový interval |
| časové razítko |Byte] |
| tinyint |Bajtů |
| Typ UniqueIdentifier |Identifikátor GUID |
| varbinary |Byte] |
| varchar |Řetězec, Char] |
| xml |XML |

## <a name="mapping-source-toosink-columns"></a>Mapování zdrojové toosink sloupce
toomap sloupce z toocolumns datové sady zdroje z podřízený datové sady, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-copy"></a>Opakovatelných kopie
Při kopírování dat tooSQL databáze serveru, aktivity kopírování hello připojí tabulky jímky toohello data ve výchozím nastavení. Místo toho najdete v části tooperform UPSERT [tooSqlSink opakovatelných zápisu](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) článku. 

Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy. V Azure Data Factory může řez znovu ručně. Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě. Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit. V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Výkon a ladění
V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.
