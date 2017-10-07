---
title: "aaaSQL aktivity uložené procedury serveru"
description: "Zjistěte, jak je možné používat tooinvoke aktivity uložené procedury aplikace SQL Server hello uloženou proceduru v databázi SQL Azure nebo Azure SQL Data Warehouse z objektu pro vytváření dat kanál."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: spelluru
ms.openlocfilehash: 9116f80eefc59d95e866b2ba1de2feb1bdc4b1d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sql-server-stored-procedure-activity"></a>Systému SQL Server uložené procedury aktivity
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Aktivita Hive](data-factory-hive-activity.md) 
> * [Pig aktivity](data-factory-pig-activity.md)
> * [Činnost MapReduce](data-factory-map-reduce.md)
> * [Streamované aktivitě Hadoop](data-factory-hadoop-streaming-activity.md)
> * [Spark aktivity](data-factory-spark.md)
> * [Aktivita Provedení dávky služby Machine Learning](data-factory-azure-ml-batch-execution-activity.md)
> * [Aktivita Aktualizace prostředků služby Machine Learning](data-factory-azure-ml-update-resource-activity.md)
> * [Aktivita Uložená procedura](data-factory-stored-proc-activity.md)
> * [Aktivita U-SQL služby Data Lake Analytics](data-factory-usql-activity.md)
> * [Vlastní aktivity rozhraní .NET](data-factory-use-custom-activities.md)

## <a name="overview"></a>Přehled
Pomocí aktivit transformace dat v datové továrně [kanálu](data-factory-create-pipelines.md) tootransform a proces nezpracovaná data do předpovědi a statistiky. Hello aktivity uložené procedury patří mezi hello aktivit transformace, které podporuje služby Data Factory. Tento článek vychází hello [aktivit transformace dat](data-factory-data-transformation-activities.md) článek, který poskytne Obecné přehled o transformaci dat a aktivit transformace hello podporované ve službě Data Factory.

Můžete použít tooinvoke hello aktivity uložené procedury uložené procedury v jednom z hello následující data ukládá ve vaší podnikové síti nebo na virtuální počítač Azure (VM): 

- Azure SQL Database
- Azure SQL Data Warehouse
- Databáze systému SQL Server.  Pokud používáte systém SQL Server, nainstalujte Brána pro správu dat na stejný počítač, hostitele hello databáze nebo na samostatný počítač, který má přístup k databázi toohello hello. Brána pro správu dat je, že komponenty, která se připojuje data způsobem, zabezpečení a správě zdroje na místní nebo na virtuální počítač Azure s cloudovými službami. V tématu [Brána pro správu dat](data-factory-data-management-gateway.md) článku.

> [!IMPORTANT]
> Při kopírování dat do Azure SQL Database nebo SQL Server, můžete nakonfigurovat hello **SqlSink** v tooinvoke kopie aktivity uložené procedury pomocí hello **sqlWriterStoredProcedureName** vlastnost. Další informace najdete v tématu [vyvolat uloženou proceduru z aktivity kopírování](data-factory-invoke-stored-procedure-from-copy-activity.md). Podrobnosti o hello vlastnosti, viz následující články konektor: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [systému SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties). Volání uložené procedury při kopírování dat do Azure SQL Data Warehouse pomocí aktivity kopírování není podporována. Ale hello uložené procedury aktivity tooinvoke uloženou proceduru můžete použít v SQL Data Warehouse. 
>  
> Při kopírování dat z Azure SQL Database nebo SQL Server nebo Azure SQL Data Warehouse, můžete nakonfigurovat **SqlSource** v tooinvoke aktivity kopírování dat ze zdrojové databáze hello pomocí hello tooread uložené procedury  **sqlReaderStoredProcedureName** vlastnost. Další informace najdete v tématu hello následující články konektor: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [systému SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)          


Následující postup používá Hello hello aktivity uložené procedury v kanálu tooinvoke uloženou proceduru v databázi Azure SQL. 

## <a name="walkthrough"></a>Názorný postup
### <a name="sample-table-and-stored-procedure"></a>Ukázkové tabulky a uložené procedury
1. Vytvořte následující hello **tabulky** ve vaší databázi SQL Azure pomocí SQL Server Management Studio nebo jakýkoli jiný nástroj, který jste obeznámeni s. sloupec razítko_data_a_času Hello je hello datum a čas, kdy se vygeneruje odpovídající ID hello.

    ```SQL
    CREATE TABLE dbo.sampletable
    (
        Id uniqueidentifier,
        datetimestamp nvarchar(127)
    )
    GO

    CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
    GO
    ```
    ID je jedinečný hello identifikovat a hello razítko_data_a_času sloupec je hello datum a čas, kdy se vygeneruje odpovídající ID hello.
    
    ![Ukázková data](./media/data-factory-stored-proc-activity/sample-data.png)

    V této ukázce hello uložená procedura je v databázi SQL Azure. Pokud hello uložené procedury je do Azure SQL Data Warehouse a databáze systému SQL Server, je podobný hello přístup. Pro databázi systému SQL Server, musíte nainstalovat [Brána pro správu dat](data-factory-data-management-gateway.md).
2. Vytvořte následující hello **uložené procedury** který vkládá data v toohello **sampletable**.

    ```SQL
    CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
    AS

    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime)
    END
    ```

   > [!IMPORTANT]
   > **Název** a **velká a malá písmena** z hello parametr (hodnota DateTime v tomto příkladu) musí odpovídat parametr zadaný v kanálu nebo aktivity hello JSON. V hello Definice uloženou proceduru, ujistěte se, že  **@**  slouží jako předpona pro parametr hello.

### <a name="create-a-data-factory"></a>Vytvoření datové továrny
1. Přihlaste se příliš[portál Azure](https://portal.azure.com/).
2. Klikněte na tlačítko **nový** v levé nabídce hello, klikněte na tlačítko **Intelligence + analýzy**a klikněte na tlačítko **Data Factory**.

    ![Nový objekt pro vytváření dat](media/data-factory-stored-proc-activity/new-data-factory.png)    
3. V hello **nový objekt pro vytváření dat** okno, zadejte **SProcDF** pro hello název. Azure Data Factory názvů **globálně jedinečný**. Je nutné tooprefix hello název objektu pro vytváření dat hello s názvem, tooenable hello úspěšném vytvoření objektu pro vytváření hello.

   ![Nový objekt pro vytváření dat](media/data-factory-stored-proc-activity/new-data-factory-blade.png)         
4. Vyberte vaše **předplatné**.
5. Pro **skupiny prostředků**, proveďte jednu z následujících kroků hello:
   1. Klikněte na tlačítko **vytvořit nový** a zadejte název pro skupinu prostředků hello.
   2. Klikněte na tlačítko **použít existující** a vyberte existující skupinu prostředků.  
6. Vyberte hello **umístění** hello data Factory.
7. Vyberte **Pin toodashboard** , aby mohli zobrazit objektu pro vytváření dat hello na řídicím panelu hello příštím přihlášení.
8. Klikněte na tlačítko **vytvořit** na hello **nový objekt pro vytváření dat** okno.
9. Zobrazí hello objekt pro vytváření dat vytváří v hello **řídicí panel** z hello portálu Azure. Po úspěšném vytvoření objektu pro vytváření dat hello, se zobrazí stránka objektu pro vytváření dat hello, se zobrazí hello obsah objektu pro vytváření dat hello.

   ![Domovská stránka objektu pro vytváření dat](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a>Vytvoření služby Azure SQL propojené
Po vytvoření objektu pro vytváření dat hello, vytváření služby SQL Azure, propojené, který odkazuje na databázi Azure SQL, který obsahuje hello sampletable tabulky a sp_sample uložené procedury, objekt pro vytváření dat tooyour.

1. Klikněte na tlačítko **vytvořit a nasadit** na hello **Data Factory** okno pro **SProcDF** toolaunch hello editoru služby Data Factory.
2. Klikněte na tlačítko **nové datové úložiště** na panelu příkazů hello a zvolte **Azure SQL Database**. Měli byste vidět, že hello skript JSON pro vytvoření Azure SQL propojená služba v editoru hello.

   ![Nové úložiště dat](media/data-factory-stored-proc-activity/new-data-store.png)
3. V hello skript JSON proveďte hello následující změny:

   1. Nahraďte `<servername>` s názvem hello serveru Azure SQL Database.
   2. Nahraďte `<databasename>` s hello databáze, ve které jste vytvořili tabulku hello a hello uložené procedury.
   3. Nahraďte `<username@servername>` s hello uživatelský účet, který má přístup toohello databáze.
   4. Nahraďte `<password>` s hello heslo pro uživatelský účet hello.

      ![Nové úložiště dat](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
4. toodeploy hello propojené služby, klikněte na tlačítko **nasadit** na panelu příkazů hello. Zkontrolujte, jestli hello AzureSqlLinkedService ve stromu hello zobrazení na levé straně hello.

    ![zobrazení stromu s propojené služby](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a>Vytvoření výstupní datové sady
Je nutné zadat, že výstupní datovou sadu aktivity uložené procedury i v případě, že hello uložené procedury neobsahuje žádná data. Je to způsobeno hello jeho výstupní datovou sadu, která řídí hello plán hello aktivity (jak často hello aktivita spuštěna - hodinové, denní, atd.). Hello výstupní datovou sadu musí používat **propojená služba** který odkazuje tooan databáze SQL Azure nebo Azure SQL Data Warehouse nebo databázi SQL Server, ve kterém chcete hello toorun uložené procedury. Hello výstupní datovou sadu může sloužit jako výsledek hello toopass způsob hello uložené procedury pro následné zpracování pomocí jiné aktivity ([řetězení aktivity](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) v kanálu hello. Ale objekt pro vytváření dat nelze zapsat automaticky hello výstupní datové sady toothis uložené procedury. Je, že hello této tabulky SQL tooa zápisů, které hello výstupní datová sada odkazuje na uložené procedury. V některých případech může být hello výstupní datovou sadu **fiktivní datovou sadu** (datovou sadu, která ukazuje tooa tabulku, která skutečně neobsahuje výstup hello uložené procedury). Tato fiktivní datová sada se používá jen toospecify hello plánu pro spuštěnou hello uložené procedury aktivity. 

1. Pokud tlačítko nevidíte, klikněte na panelu nástrojů na **... Další** na hello nástrojů, klikněte na tlačítko **nová datová sada**a klikněte na tlačítko **Azure SQL**. **Nová datová sada** na příkaz hello panelu a vyberte možnost **Azure SQL**.

    ![zobrazení stromu s propojené služby](media/data-factory-stored-proc-activity/new-dataset.png)
2. Zkopírujte a vložte následující skript JSON v editoru JSON toohello hello.

    ```JSON
    {                
        "name": "sprocsampleout",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "sampletable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```
3. toodeploy hello datovou sadu, klikněte na tlačítko **nasadit** na panelu příkazů hello. Zkontrolujte, jestli hello datovou sadu v zobrazení stromu hello.

    ![Zobrazení stromu s propojenými službami](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a>Vytvoření kanálu s aktivitou SqlServerStoredProcedure
Teď umožňuje vytvoření kanálu s aktivitou uložené procedury. 

Všimněte si hello následující vlastnosti: 

- Hello **typ** vlastnost je nastavena příliš**SqlServerStoredProcedure**. 
- Hello **storedProcedureName** v typu vlastnosti nastavena příliš**sp_sample** (název hello uložené procedury).
- Hello **storedProcedureParameters** část obsahuje jeden parametr s názvem **hodnoty DataTime**. Název a malá a velká písmena hello parametr ve formátu JSON musí odpovídat názvu hello a velká a malá písmena hello parametru v definici hello uložené procedury. Pokud je třeba předat hodnotu null pro parametr, použijte syntaxi hello: `"param1": null` (všechny malá písmena).
 
1. Pokud tlačítko nevidíte, klikněte na panelu nástrojů na **... Další** na panelu příkazů hello a klikněte na tlačítko **nový kanál**.
2. Hello zkopírujte a vložte následující fragment kódu JSON:   

    ```JSON
    {
        "name": "SprocActivitySamplePipeline",
        "properties": {
            "activities": [
                {
                    "type": "SqlServerStoredProcedure",
                    "typeProperties": {
                        "storedProcedureName": "sp_sample",
                        "storedProcedureParameters": {
                            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                        }
                    },
                    "outputs": [
                        {
                            "name": "sprocsampleout"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "SprocActivitySample"
                }
            ],
             "start": "2017-04-02T00:00:00Z",
             "end": "2017-04-02T05:00:00Z",
            "isPaused": false
        }
    }
    ```
3. toodeploy hello kanálu, klikněte na tlačítko **nasadit** na panelu nástrojů hello.  

### <a name="monitor-hello-pipeline"></a>Monitorování kanálu hello
1. Klikněte na tlačítko **X** okna editoru služby Data Factory tooclose toonavigate zpět toohello okno objekt pro vytváření dat a klikněte na tlačítko **Diagram**.

    ![Dlaždice diagram](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
2. V hello **zobrazení diagramu**, zobrazí se přehled hello kanálů a datové sady použité v tomto kurzu.

    ![Dlaždice diagram](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
3. V hello zobrazení diagramu, klikněte dvakrát na datovou sadu hello `sprocsampleout`. Zobrazí hello řezy ve stavu Připraveno. Měla by existovat pět řezy protože řez se vytvářejí každou hodinu mezi hello počáteční a koncový čas z hello JSON.

    ![Dlaždice diagram](media/data-factory-stored-proc-activity/data-factory-slices.png)
4. Pokud je řez v **připraven** stavu, spusťte `select * from sampletable` dotaz hello tooverify databáze Azure SQL, který hello data byla vložena v tabulce toohello hello uložené procedury.

   ![výstupní data](./media/data-factory-stored-proc-activity/output.png)

   V tématu [monitorování hello kanálu](data-factory-monitor-manage-pipelines.md) podrobné informace o monitorování kanálů služby Azure Data Factory.  


## <a name="specify-an-input-dataset"></a>Zadejte vstupní datové sady
Aktivita uložené procedury v návodu hello nemá žádné vstupní datové sady. Pokud zadáte vstupní datové sady, hello aktivity uložené procedury nespustí dokud hello řezy vstupní datové sady je k dispozici (ve stavu Připraveno). Hello datová sada může být externí datová sada (který není vyprodukované jinou aktivitu v hello stejné kanálu) nebo interní datovou sadu, která je vytvořený nadřízenou aktivitou (hello aktivity, která se spouští před této aktivity). Můžete určit více vstupní datové sady pro aktivity hello uložené procedury. Pokud tak učiníte, hello aktivity uložené procedury spustí jenom v případě, že všechny řezy hello vstupní datové sady jsou k dispozici (ve stavu Připraveno). vstupní datové sady Hello nelze zpracovat v hello uložené procedury jako parametr. Je pouze použité toocheck hello závislostí před počáteční hello uložené procedury aktivity.

## <a name="chaining-with-other-activities"></a>Řetězení s ostatními aktivitami
Pokud chcete toochain nadřazeného aktivitu se tato aktivita, zadejte jako vstup této aktivity hello výstup hello nadřazené aktivity. Pokud tak učiníte, hello aktivity uložené procedury nelze spustit až do dokončení hello nadřazeného aktivity a hello výstupní datovou sadu nadřízené činnosti hello je k dispozici (ve stavu Připraveno). Výstupní datové sady z více činností, nadřazeného můžete zadat jako vstupní datové sady hello uložené procedury aktivity. Když to uděláte tak hello uložené procedury aktivity spustí pouze v případě, že všechny řezy hello vstupní datové sady jsou k dispozici.  

V následujícím příkladu hello, je hello výstup aktivity kopírování hello: OutputDataset, což je vstup hello uložené procedury aktivity. Proto hello aktivity uložené procedury nelze spustit až do dokončení aktivity kopírování hello a hello OutputDataset řez je k dispozici (ve stavu Připraveno). Pokud zadáte více vstupní datové sady, hello aktivity uložené procedury nelze spustit až do všech řezech hello vstupní datové sady jsou k dispozici (ve stavu Připraveno). vstupní datové sady Hello nelze použít přímo jako aktivita parametry toohello uložené procedury. 

Další informace o řetězení aktivit najdete v tématu [více aktivit v kanálu](data-factory-create-pipelines.md#multiple-activities-in-a-pipeline)

```json
{

    "name": "ADFTutorialPipeline",
    "properties": {
        "description": "Copy data from a blob tooblob",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [ { "name": "InputDataset" } ],
                "outputs": [ { "name": "OutputDataset" } ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst"
                },
                "name": "CopyFromBlobToSQL"
            },
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "SPSproc"
                },
                "inputs": [ { "name": "OutputDataset" } ],
                "outputs": [ { "name": "SQLOutputDataset" } ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "RunStoredProcedure"
            }

        ],
        "start": "2017-04-12T00:00:00Z",
        "end": "2017-04-13T00:00:00Z",
        "isPaused": false,
    }
}
```

Podobně toolink hello úložiště aktivita procedury s **podřízené aktivity** (hello aktivity, které spustí po dokončení technologie hello aktivity uložené procedury), zadejte hello výstupní datovou sadu aktivity hello uložené procedury jako vstup hello podřízené aktivity v kanálu hello.

> [!IMPORTANT]
> Při kopírování dat do Azure SQL Database nebo SQL Server, můžete nakonfigurovat hello **SqlSink** v tooinvoke kopie aktivity uložené procedury pomocí hello **sqlWriterStoredProcedureName** vlastnost. Další informace najdete v tématu [vyvolat uloženou proceduru z aktivity kopírování](data-factory-invoke-stored-procedure-from-copy-activity.md). Podrobnosti o hello vlastnosti najdete v tématu hello následující články konektor: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [systému SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).
>  
> Při kopírování dat z Azure SQL Database nebo SQL Server nebo Azure SQL Data Warehouse, můžete nakonfigurovat **SqlSource** v tooinvoke aktivity kopírování dat ze zdrojové databáze hello pomocí hello tooread uložené procedury  **sqlReaderStoredProcedureName** vlastnost. Další informace najdete v tématu hello následující články konektor: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [systému SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)          

## <a name="json-format"></a>Formát JSON
Tady je hello formátu JSON pro definování aktivity uložené procedury:

```JSON
{
    "name": "SQLSPROCActivity",
    "description": "description",
    "type": "SqlServerStoredProcedure",
    "inputs":  [ { "name": "inputtable"  } ],
    "outputs":  [ { "name": "outputtable" } ],
    "typeProperties":
    {
        "storedProcedureName": "<name of hello stored procedure>",
        "storedProcedureParameters":  
        {
            "param1": "param1Value"
            …
        }
    }
}
```

Hello následující tabulka popisuje tyto vlastnosti JSON:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| jméno | Název aktivity hello |Ano |
| description |Popisuje, jaké aktivity hello se používá pro text |Ne |
| type | Musí být nastavena na: **SqlServerStoredProcedure** | Ano |
| Vstupy | Volitelné. Pokud zadáte vstupní datové sady, musí být k dispozici (v 'Připravený' stav) pro hello uložené procedury aktivity toorun. vstupní datové sady Hello nelze zpracovat v hello uložené procedury jako parametr. Je pouze použité toocheck hello závislostí před počáteční hello uložené procedury aktivity. |Ne |
| Výstupy | Je nutné zadat výstupní datovou sadu aktivity uložené procedury. Výstupní datové sady určuje hello **plán** pro hello uložené procedury aktivity (každou hodinu, týdně, měsíčně, atd.). <br/><br/>Hello výstupní datovou sadu musí používat **propojená služba** který odkazuje tooan databáze SQL Azure nebo Azure SQL Data Warehouse nebo databázi SQL Server, ve kterém chcete hello toorun uložené procedury. <br/><br/>Hello výstupní datovou sadu může sloužit jako výsledek hello toopass způsob hello uložené procedury pro následné zpracování pomocí jiné aktivity ([řetězení aktivity](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) v kanálu hello. Ale objekt pro vytváření dat nelze zapsat automaticky hello výstupní datové sady toothis uložené procedury. Je, že hello této tabulky SQL tooa zápisů, které hello výstupní datová sada odkazuje na uložené procedury. <br/><br/>V některých případech může být hello výstupní datovou sadu **fiktivní datovou sadu**, který se používá jen toospecify hello plánu pro spuštěnou hello uložené procedury aktivity. |Ano |
| storedProcedureName |Zadejte název hello hello uložené procedury v hello Azure SQL database nebo databáze Azure SQL Data Warehouse nebo SQL Server, která je reprezentována hello propojené služby, která hello používá výstupní tabulka. |Ano |
| storedProcedureParameters |Zadejte hodnoty pro parametry uložené procedury. Pokud potřebujete toopass hodnotu null pro parametr, použijte syntaxi hello: "param1": null (všechny malá písmena). Viz následující ukázka toolearn o používání této vlastnosti hello. |Ne |

## <a name="passing-a-static-value"></a>Předávání na statické hodnoty
Nyní Pojďme zvažte přidání jiný sloupec s názvem 'Scénář' v tabulce hello obsahující statická hodnota volána 'Dokumentu ukázka'.

![Ukázková data 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

**Tabulka:**

```SQL
CREATE TABLE dbo.sampletable2
(
    Id uniqueidentifier,
    datetimestamp nvarchar(127),
    scenario nvarchar(127)
)
GO

CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable2(Id);
```

**Uložené procedury:**

```SQL
CREATE PROCEDURE sp_sample2 @DateTime nvarchar(127) , @Scenario nvarchar(127)

AS

BEGIN
    INSERT INTO [sampletable2]
    VALUES (newid(), @DateTime, @Scenario)
END
```

Nyní, předat hello **scénář** hodnotu parametru a hello z hello uložené procedury aktivity. Hello **rámci typeProperties** část v hello předchozích ukázkových zdá hello následující fragment kódu:

```JSON
"typeProperties":
{
    "storedProcedureName": "sp_sample",
    "storedProcedureParameters":
    {
        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
        "Scenario": "Document sample"
    }
}
```

**Datové sady objektu pro vytváření dat:**

```JSON
{
    "name": "sprocsampleout2",
    "properties": {
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "sampletable2"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Kanál služby Data Factory**

```JSON
{
    "name": "SprocActivitySamplePipeline2",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_sample2",
                    "storedProcedureParameters": {
                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
                        "Scenario": "Document sample"
                    }
                },
                "outputs": [
                    {
                        "name": "sprocsampleout2"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "SprocActivitySample"
            }
        ],
        "start": "2016-10-02T00:00:00Z",
        "end": "2016-10-02T05:00:00Z"
    }
}
```