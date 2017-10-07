---
title: "aaaMove data z úložiště dat rozhraní ODBC | Microsoft Docs"
description: "Informace o tom, jak toomove dat od ODBC ukládá pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: ad70a598-c031-4339-a883-c6125403cb76
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: jingwang
ms.openlocfilehash: bf96e71da449313b6144bb194205c572d2ca2030
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-odbc-data-stores-using-azure-data-factory"></a>Přesun dat z rozhraní ODBC datová úložiště pomocí Azure Data Factory
Tento článek vysvětluje, jak uložit toouse hello aktivitu kopírování v Azure Data Factory toomove data z místními dat ODBC. Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.

Může kopírovat data z ODBC dat úložiště tooany podporované jímku dat úložiště. Seznam dat úložiště, které jsou podporované jako jímky pomocí aktivity kopírování hello, najdete v části hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky. Objekt pro vytváření dat se aktuálně podporuje pouze přesun Klientova úložiště dat tooother uložit data z dat ODBC, ale ne pro přesun dat z jiného úložiště dat úložiště tooan ODBC data. 

## <a name="enabling-connectivity"></a>Povolení připojení
Služba data Factory podporuje připojování zdroje ODBC tooon místní pomocí hello Brána pro správu dat. V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) toolearn článek o Brána pro správu dat a podrobné pokyny k nastavení hello brány. Použijte úložiště dat rozhraní ODBC tooconnect tooan aplikace hello brány i v případě, že je hostovaná ve virtuálním počítači Azure IaaS.

Hello brány můžete nainstalovat na stejný místní počítač nebo virtuální počítač Azure jako úložiště dat rozhraní ODBC hello hello hello. Doporučujeme však nainstalovat hello brány na samostatný počítač nebo Azure IaaS virtuálních počítačů tooavoid sporu prostředků a pro dosažení vyššího výkonu. Když instalujete na samostatný počítač hello brány, hello počítač měl být schopný tooaccess hello počítače s úložištěm dat rozhraní ODBC hello.

Vedle hello Brána pro správu dat musíte také ovladače ODBC hello tooinstall pro úložiště dat hello na počítači brány hello.

> [!NOTE]
> V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.

## <a name="getting-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště dat rozhraní ODBC pomocí různých nástrojů nebo rozhraní API.

Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.

Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**. V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování. 

Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello: 

1. Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.
2. Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování. 
3. Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup. 

Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello). Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.  Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy dat z úložiště dat rozhraní ODBC, naleznete v tématu [JSON příklad: kopírování dat z dat ODBC ukládání objektů Blob tooAzure](#json-example-copy-data-from-odbc-data-store-to-azure-blob) tohoto článku. 

Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou úložiště dat konkrétní tooODBC entity služby Data Factory používané toodefine:

## <a name="linked-service-properties"></a>Vlastnosti propojené služby
Hello následující tabulka obsahuje popis JSON elementy konkrétní tooODBC propojené služby.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| type |vlastnost typu Hello musí být nastavena na: **OnPremisesOdbc** |Ano |
| připojovací řetězec |šifrovat přihlašovací údaje bez přístupu část Hello hello připojovací řetězec a volitelné přihlašovacích údajů. Příklady v následující části hello. |Ano |
| přihlašovací údaje |Hello přístup pověření část hello připojovacího řetězce zadaného ve formátu ovladačem vlastnost hodnota. Příklad: "Uid =<user ID>; PWD =<password>; RefreshToken =<secret refresh token>; ". |Ne |
| authenticationType. |Typ ověřování používá úložiště dat rozhraní ODBC toohello tooconnect. Možné hodnoty jsou: anonymní a Basic. |Ano |
| uživatelské jméno |Pokud používáte základní ověřování, zadejte uživatelské jméno. |Ne |
| heslo |Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello. |Ne |
| gatewayName |Úložiště dat rozhraní ODBC toohello tooconnect měli použít název hello brány, kterou hello služba Data Factory. |Ano |

### <a name="using-basic-authentication"></a>Použití základního ověřování

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```
### <a name="using-basic-authentication-with-encrypted-credentials"></a>Základní ověřování pomocí zašifrované přihlašovací údaje
Můžete šifrovat přihlašovací údaje hello pomocí hello [New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) rutiny (1.0 verzi prostředí Azure PowerShell) nebo [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0.9 nebo starší verzi hello Azure PowerShell).  

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=myserver.database.windows.net; Database=TestDatabase;;EncryptedCredential=eyJDb25uZWN0...........................",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-anonymous-authentication"></a>Pomocí anonymní ověřování

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "connectionString": "Driver={SQL Server};Server={servername}.database.windows.net; Database=TestDatabase;",
            "credential": "UID={uid};PWD={pwd}",
            "gatewayName": "mygateway"
        }
    }
}
```


## <a name="dataset-properties"></a>Vlastnosti datové sady
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku. Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).

Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello. rámci typeProperties Hello část datové sady typ **RelationalTable** (která zahrnuje datovou sadu ODBC) má následující vlastnosti hello

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| tableName |Název tabulky hello v úložišti dat rozhraní ODBC hello. |Ano |

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku. Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.

Vlastnosti, které jsou k dispozici v hello **rámci typeProperties** části hello aktivit na hello se každý typ aktivity lišit podle druhé straně. Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.

Při aktivitě kopírování, pokud je zdroj typu **RelationalSource** (která zahrnuje rozhraní ODBC), hello následující vlastnosti jsou k dispozici v rámci typeProperties části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| query |Použijte data tooread hello vlastního dotazu. |Řetězec dotazu SQL. Příklad: vybrat * z MyTable. |Ano |


## <a name="json-example-copy-data-from-odbc-data-store-tooazure-blob"></a>Příklad JSON: kopírování dat z dat ODBC ukládání tooAzure objektů Blob
Tento příklad obsahuje definice JSON, můžete použít toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ukazuje, jak toocopy data z ODBC zdroje tooan Azure Blob Storage. Data však mohou být zkopírovaný tooany hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.

Ukázka Hello má hello následující entity objektu pro vytváření dat:

1. Propojené služby typu [OnPremisesOdbc](#linked-service-properties).
2. Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](#dataset-properties).
4. Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Ukázka Hello zkopíruje data z výsledku dotazu v objektu blob úložiště tooa ODBC dat každou hodinu. Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.

Jako první krok nastavte Brána pro správu dat hello. Hello pokyny jsou v hello [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.

**ODBC propojená služba** tento příklad používá hello základní ověřování. V tématu [ODBC propojená služba](#linked-service-properties) části pro různé typy ověřování můžete použít.

```json
{
    "name": "OnPremOdbcLinkedService",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```

**Propojená služba Azure Storage**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
    "type": "AzureStorage",
    "typeProperties": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
    }
}
```

**Vstupní datové sady rozhraní ODBC**

Ukázka Hello předpokládá jste vytvořili tabulku "MyTable" v databázi ODBC a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady.

Nastavení "externí": "PRAVDA" informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

```json
{
    "name": "ODBCDataSet",
    "properties": {
        "published": false,
        "type": "RelationalTable",
        "linkedServiceName": "OnPremOdbcLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
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
    "name": "AzureBlobOdbcDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/odbc/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
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
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```


**Aktivita kopírování v kanálu s zdroje ODBC (RelationalSource) a podřízený objekt Blob (BlobSink)**

Hello kanál obsahuje aktivitu kopírování, je nakonfigurovaná toouse tyto vstupní a výstupní datové sady a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**RelationalSource** a **podřízený** je typ nastaven příliš**BlobSink**. Dotaz SQL Hello zadaný pro hello **dotazu** vlastnost vybere hello data v hello za hodinu toocopy.

```json
{
    "name": "CopyODBCToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "OdbcDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOdbcDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "OdbcToBlob"
            }
        ],
        "start": "2016-06-01T18:00:00Z",
        "end": "2016-06-01T19:00:00Z"
    }
}
```
### <a name="type-mapping-for-odbc"></a>Mapování typu pro rozhraní ODBC
Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello dvoustupňový přístup následující:

1. Převod z typu too.NET typy nativní zdroje
2. Převést typ jímky toonative typ rozhraní .NET

Při přesouvání dat z úložiště dat rozhraní ODBC, ODBC datové typy jsou typy namapované too.NET, jak je uvedeno v hello [mapování datového typu ODBC](https://msdn.microsoft.com/library/cc668763.aspx) tématu.

## <a name="map-source-toosink-columns"></a>Mapování zdrojových toosink sloupců
toolearn o mapování sloupců v toocolumns datové sady zdroje v datové sadě jímka, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Opakovatelných číst z relační zdrojů
Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy. V Azure Data Factory může řez znovu ručně. Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě. Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit. V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="ge-historian-store"></a>GE Historian úložiště
Vytvoření toolink ODBC propojené služby [GE Proficy Historian (teď GE Historian)](http://www.geautomation.com/products/proficy-historian) úložiště dat tooan pro vytváření dat Azure, jak ukazuje následující příklad hello:

```json
{
    "name": "HistorianLinkedService",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "connectionString": "DSN=<name of hello GE Historian store>",
            "gatewayName": "<gateway name>",
            "authenticationType": "Basic",
            "userName": "<user name>",
            "password": "<password>"
        }
    }
}
```

Nainstalovat brána pro správu dat na místním počítači a zaregistrujte bránu hello hello portálu. Hello brány je nainstalovaná v místním počítači používá hello ovladač ODBC pro GE Historian tooconnect toohello GE Historian úložišti. Proto nainstalujte ovladač hello, pokud ještě není nainstalovaná na počítači brány hello. V tématu [povolení připojení](#enabling-connectivity) podrobnosti.

Než použijete hello GE Historian ukládat v řešení pro vytváření dat, ověřte, zda hello brány může připojit toohello data store pomocí pokynů v další části hello.

Přečíst článek hello od začátku hello podrobnější přehled použití dat ODBC se ukládá jako zdrojové úložiště dat v operaci kopírování.  

## <a name="troubleshoot-connectivity-issues"></a>Řešení potíží s problémy s připojením
potíže s připojením tootroubleshoot, použijte hello **diagnostiky** kartě **Správce konfigurace brány pro správu dat**.

1. Spusťte **Správce konfigurace brány pro správu dat**. Buď můžete spustit "C:\Program Files\Microsoft Data správy Gateway\1.0\Shared\ConfigManager.exe" přímo (nebo) vyhledávání pro **brány** toofind odkaz příliš**Brána pro správu dat** aplikace, jak ukazuje následující obrázek hello.

    ![Hledání brány](./media/data-factory-odbc-connector/search-gateway.png)
2. Přepínač toohello **diagnostiky** kartě.

    ![Diagnostiku brány](./media/data-factory-odbc-connector/data-factory-gateway-diagnostics.png)
3. Vyberte hello **typ** dat uložit (propojené služby).
4. Zadejte **ověřování** a zadejte **pověření** (nebo) zadejte **připojovací řetězec** , je použít tooconnect toohello datové úložiště.
5. Klikněte na tlačítko **testovací připojení** tootest hello připojení toohello úložišti.

## <a name="performance-and-tuning"></a>Výkon a ladění
V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.
