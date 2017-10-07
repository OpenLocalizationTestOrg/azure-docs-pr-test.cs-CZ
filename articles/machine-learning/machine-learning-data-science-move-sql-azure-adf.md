---
title: "aaaMove data z tooSQL místní SQL Server Azure s Azure Data Factory | Microsoft Docs"
description: "Nastavte kanál ADF, která vytvoří dvě aktivity migrace dat, které společně přesun dat na každý den mezi databází na místě a v cloudu hello."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 36837c83-2015-48be-b850-c4346aa5ae8a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 7f7e78c7a84a259539221d3235b76bb5a3cf9866
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-on-premises-sql-server-toosql-azure-with-azure-data-factory"></a>Přesun dat z tooSQL serveru SQL místní Azure s Azure Data Factory
Toto téma ukazuje, jak hello toomove data ze tooa databáze systému SQL Server místní databáze SQL Azure přes Azure Blob Storage pomocí Azure Data Factory (ADF).

Tabulka, která shrnuje různé možnosti pro přesunutí dat tooan Azure SQL Database, najdete v části [přesunout data tooan Azure SQL Database pro Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).

## <a name="intro"></a>Úvod: Co je ADF a když měli použít toomigrate dat?
Azure Data Factory je plně spravovaná Cloudová datová integrační služba, která orchestruje a automatizuje hello přesouvání a transformaci dat. Hello klíče koncept v modelu ADF hello je kanálu. Kanál je logické seskupení aktivit, z nichž každý definuje hello akce tooperform na hello data obsažená v datových sadách. Propojené služby jsou použité toodefine hello informace potřebné pro vytváření dat tooconnect toohello datové prostředky.

S ADF se může skládat stávající služby zpracování dat do datových kanálů, které jsou vysoce dostupné a spravovaných v cloudu hello. Tyto kanály dat může být naplánované tooingest, Příprava, transformace, analyzovat a publikovat data a ADF spravuje a orchestruje hello komplexní dat a zpracování závislosti. Řešení může být hello rychle vytvořené a nasazené v cloudu, připojení stále více využívají místní a zdroje dat v cloudu.

Zvažte použití ADF:

* Pokud data potřebám toobe průběžně migrovat v hybridním scénáři, který přistupuje k i místní a cloudové prostředky
* Když hello dat je zpracován, nebo musí toobe upravené nebo mít obchodní logiku přidat tooit při migraci.

ADF umožňuje hello plánování a sledování úloh pomocí jednoduché skripty JSON, které spravují hello přesouvání dat v pravidelných intervalech. ADF má také další funkce, například podporu pro komplexní operace. Další informace o ADF, naleznete v dokumentaci k hello v [Azure Data Factory (ADF)](https://azure.microsoft.com/services/data-factory/).

## <a name="scenario"></a>Hello scénář
Nemůžeme nastavit kanál ADF, která vytvoří dvě aktivity migrace dat. Společně se přesouvat data na každý den mezi místní databázi SQL a Azure SQL Database v cloudu hello. jsou dvě aktivity Hello:

* kopírování dat z databáze tooan systému SQL Server na místní účet Azure Blob Storage
* kopírování dat z tooan účet Azure Blob Storage hello Azure SQL Database.

> [!NOTE]
> Hello postupy zde byly upraveny, z hello podrobnější kurzu poskytované hello ADF team: [přesun dat mezi místní zdroje a cloudu s Brána pro správu dat](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) odkazuje toohello příslušných částech tohoto tématu jsou k dispozici v případě nutnosti.
>
>

## <a name="prereqs"></a>Požadavky
Tento kurz předpokládá, že máte:

* **Předplatné**. Pokud nemáte předplatné, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
* **Účtu úložiště Azure**. Používáte účet úložiště Azure pro ukládání dat hello v tomto kurzu. Pokud nemáte účet úložiště Azure, najdete v části hello [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) článku. Po vytvoření účtu úložiště hello musíte tooobtain hello účet, který klíč používá tooaccess hello úložiště. V tématu [Správa přístupových klíčů úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).
* Přístup k tooan **Azure SQL Database**. Pokud je potřeba nastavit Azure SQL Database, hello tpoic [Začínáme se službou Microsoft Azure SQL Database ](../sql-database/sql-database-get-started.md) obsahuje informace o tooprovision novou instanci třídy Azure SQL Database.
* Nainstalovaný a nakonfigurovaný **prostředí Azure PowerShell** místně. Pokyny najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

> [!NOTE]
> Tento postup používá hello [portál Azure](https://portal.azure.com/).
>
>

## <a name="upload-data"></a>Nahrávání hello data tooyour místní systém SQL Server
Používáme hello [datovou sadu NYC taxíkem](http://chriswhong.com/open-data/foil_nyc_taxi/) proces migrace toodemonstrate hello. Hello NYC taxíkem datová sada k dispozici, jak je uvedeno v tomto příspěvku v úložišti objektů blob v Azure [NYC taxíkem Data](http://www.andresmh.com/nyctaxitrips/). Hello dat má dva soubory, hello trip_data.csv souboru, který obsahuje podrobnosti o cestě, a hello trip_far.csv soubor, který obsahuje podrobnosti o tarif hello placené pro každou cestu. Ukázka a popis tyto soubory jsou uvedeny v [NYC taxíkem služebních cest datovou sadu popis](machine-learning-data-science-process-sql-walkthrough.md#dataset).

Můžete přizpůsobit hello postup uvedený v tomto poli tooa sadu svoje vlastní data, nebo postupujte podle kroků hello, jak je popsáno pomocí hello NYC taxíkem datovou sadu. tooupload hello NYC taxíkem datové sady do místní databáze systému SQL Server, postupujte podle uvedených v postupu hello [hromadně importovat Data do databáze serveru SQL](machine-learning-data-science-process-sql-walkthrough.md#dbload). Tyto pokyny jsou pro systém SQL Server na virtuální počítač Azure, ale hello hello postup pro odesílání toohello místní SQL Server je stejný.

## <a name="create-adf"></a>Vytvoření služby Azure Data Factory
Pokyny pro vytvoření nové Azure Data Factory a skupině prostředků v hello Hello [portál Azure](https://portal.azure.com/) jsou k dispozici [vytvoření služby Azure Data Factory](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#create-data-factory). Název hello novou instanci ADF *adfdsp* a skupinu prostředků, název hello vytvořili *adfdsprg*.

## <a name="install-and-configure-up-hello-data-management-gateway"></a>Instalace a konfigurace až hello Brána pro správu dat
tooenable kanály toowork objekt pro vytváření dat Azure s SQL serverem místní, je nutné tooadd jej jako objekt pro vytváření toohello dat propojené služby. toocreate a propojené služby SQL serveru místní, musíte:

* Stáhněte a nainstalujte Brána pro správu dat na místním počítači hello.
* konfiguraci hello propojené služby pro brány hello toouse zdroje dat pro místní hello.

Hello Brána pro správu dat serializuje a deserializuje hello zdroj a jímka data na hello počítači, který je hostitelem.

Pokyny k instalaci a informace o Brána pro správu dat najdete v tématu [přesun dat mezi místní zdroje a cloudu s Brána pro správu dat](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)

## <a name="adflinkedservices"></a>Vytvoření propojené služby tooconnect toohello datových prostředků
Propojená služba definuje hello informace potřebné pro Azure Data Factory tooconnect tooa datový prostředek. Hello podrobný postup pro vytvoření propojené služby je k dispozici v [vytvoření propojených služeb](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-linked-services).

Máme tři zdroje v tomto scénáři, pro které jsou potřeba propojené služby.

1. [Propojené služby pro místní systém SQL Server](#adf-linked-service-onprem-sql)
2. [Propojené služby pro Azure Blob Storage](#adf-linked-service-blob-store)
3. [Propojené služby pro databázi Azure SQL](#adf-linked-service-azure-sql)

### <a name="adf-linked-service-onprem-sql"></a>Propojené služby pro místní databázi systému SQL Server
toocreate hello propojené služby pro hello místní systém SQL Server:

* Klikněte na tlačítko hello **Data Store** v hello ADF cílová stránka na portálu Azure Classic
* Vyberte **SQL** a zadejte hello *uživatelské jméno* a *heslo* přihlašovací údaje pro hello místní systém SQL Server. Je třeba tooenter hello servername jako **servername plně kvalifikovaný název instance zpětné lomítko (servername\instancename)**. Název hello propojená služba *adfonpremsql*.

### <a name="adf-linked-service-blob-store"></a>Propojené služby pro objekt Blob
toocreate hello propojené služby pro účet Azure Blob Storage hello:

* Klikněte na tlačítko hello **Data Store** v hello ADF cílová stránka na portálu Azure Classic
* Vyberte **účet úložiště Azure**
* Zadejte název klíče a kontejneru Azure Blob Storage účtu hello. Název hello propojenou službu *adfds*.

### <a name="adf-linked-service-azure-sql"></a>Propojené služby pro databázi Azure SQL
toocreate hello propojené služby pro hello Azure SQL Database:

* Klikněte na tlačítko hello **Data Store** v hello ADF cílová stránka na portálu Azure Classic
* Vyberte **Azure SQL** a zadejte hello *uživatelské jméno* a *heslo* přihlašovací údaje pro hello Azure SQL Database. Hello *uživatelské jméno* musí být zadány jako  *user@servername* .   

## <a name="adf-tables"></a>Definovat a vytvářet tabulky toospecify jak tooaccess hello datové sady
Vytváření tabulek, která ke specifikaci hello struktura, umístění a dostupnost datové sady hello hello následující postupy založené na skriptech. Soubory JSON jsou použité toodefine hello tabulky. Další informace o struktuře hello těchto souborů naleznete v tématu [datové sady](../data-factory/data-factory-create-datasets.md).

> [!NOTE]
> Musí provést hello `Add-AzureAccount` rutiny před provedením hello [New-AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) pro provedení příkazu hello je vybrán tooconfirm rutiny, která hello právo předplatné. Dokumentaci této rutiny najdete v tématu [Add-AzureAccount](/powershell/module/azure/add-azureaccount?view=azuresmps-3.7.0).
>
>

Hello na základě JSON definice v tabulkách hello použijte hello následující názvy:

* Hello **název tabulky** v hello místní SQL server je *nyctaxi_data*
* Hello **název kontejneru** v hello Azure Blob Storage je účet *containername*  

Tři definice tabulek jsou potřeba pro tento kanál ADF:

1. [Místní tabulky SQL](#adf-table-onprem-sql)
2. [Tabulka objektů BLOB](#adf-table-blob-store)
3. [SQL Azure Table](#adf-table-azure-sql)

> [!NOTE]
> Tyto postupy použijte toodefine prostředí Azure PowerShell a vytvořit hello ADF aktivity. Ale tyto úkoly lze provést také pomocí hello portálu Azure. Podrobnosti najdete v tématu [vytvoření datových sad](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-datasets).
>
>

### <a name="adf-table-onprem-sql"></a>Místní tabulky SQL
definice tabulky Hello hello místního systému SQL Server je uveden v následující soubor JSON hello:

        {
            "name": "OnPremSQLTable",
            "properties":
            {
                "location":
                {
                "type": "OnPremisesSqlServerTableLocation",
                "tableName": "nyctaxi_data",
                "linkedServiceName": "adfonpremsql"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1,   
                "waitOnExternal":
                {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
                }

                }
            }
        }

názvy sloupců Hello nebyly zde zahrnuty. Můžete se na názvy sloupců hello zahrnutím je zde dílčí vyberte (pro podrobnosti zkontrolujte hello [ADF dokumentaci](../data-factory/data-factory-data-movement-activities.md) tématu.

Zkopírujte definici JSON hello hello tabulky do souboru s názvem *onpremtabledef.json* souboru a uložte ho tooa označuje umístění (zde předpokládá, že toobe *C:\temp\onpremtabledef.json*). Vytváření tabulek hello v ADF s hello následující rutiny Azure Powershellu:

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


### <a name="adf-table-blob-store"></a>Tabulka objektů BLOB
Definice tabulky hello pro hello výstupní umístění objektu blob je v následující hello (mapuje hello požity dat z objektu blob tooAzure místní):

        {
            "name": "OutputBlobTable",
            "properties":
            {
                "location":
                {
                "type": "AzureBlobLocation",
                "folderPath": "containername",
                "format":
                {
                "type": "TextFormat",
                "columnDelimiter": "\t"
                },
                "linkedServiceName": "adfds"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1
                }
            }
        }

Zkopírujte definici JSON hello hello tabulky do souboru s názvem *bloboutputtabledef.json* souboru a uložte ho tooa označuje umístění (zde předpokládá, že toobe *C:\temp\bloboutputtabledef.json*). Vytváření tabulek hello v ADF s hello následující rutiny Azure Powershellu:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json  

### <a name="adf-table-azure-sq"></a>SQL Azure Table
Definice pro hello tabulku pro výstup SQL Azure hello se následující hello (toto schéma mapuje hello dat pocházejících z objektu blob hello):

    {
        "name": "OutputSQLAzureTable",
        "properties":
        {
            "structure":
            [
                { "name": "column1", type": "String"},
                { "name": "column2", type": "String"}                
            ],
            "location":
            {
                "type": "AzureSqlTableLocation",
                "tableName": "your_db_name",
                "linkedServiceName": "adfdssqlazure_linked_servicename"
            },
            "availability":
            {
                "frequency": "Day",
                "interval": 1            
            }
        }
    }

Zkopírujte definici JSON hello hello tabulky do souboru s názvem *AzureSqlTable.json* souboru a uložte ho tooa označuje umístění (zde předpokládá, že toobe *C:\temp\AzureSqlTable.json*). Vytváření tabulek hello v ADF s hello následující rutiny Azure Powershellu:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json  


## <a name="adf-pipeline"></a>Definovat a vytvořit kanál hello
Zadejte hello aktivity, které patří toohello kanálu a vytvoření kanálu hello s hello následující postupy založené na skriptech. Soubor JSON je použité toodefine hello kanálu vlastnosti.

* Hello skript předpokládá, že hello **název kanálu** je *AMLDSProcessPipeline*.
* Všimněte si také, že nastaví hello periodicity toobe kanálu hello provést na každý den základ a použití hello výchozí doba provádění úlohy hello (12: 00 UTC).

> [!NOTE]
> Hello následující postupy použijte toodefine prostředí Azure PowerShell a vytvoření kanálu hello ADF. Ale tento úkol můžete udělat taky pomocí portálu Azure. Podrobnosti najdete v tématu [vytvořit kanál](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-pipeline).
>
>

Pomocí definice tabulky hello uvedeny dříve, definice kanálu hello hello ADF, je zadána následovně:

        {
            "name": "AMLDSProcessPipeline",
            "properties":
            {
                "description" : "This pipeline has one Copy activity that copies data from an on-premises SQL tooAzure blob",
                 "activities":
                [
                    {
                        "name": "CopyFromSQLtoBlob",
                        "description": "Copy data from on-premises SQL server tooblob",     
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OnPremSQLTable"} ],
                        "outputs": [ {"name": "OutputBlobTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "SqlSource",
                                "sqlReaderQuery": "select * from nyctaxi_data"
                            },
                            "sink":
                            {
                                "type": "BlobSink"
                            }   
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 0,
                            "timeout": "01:00:00"
                        }       

                     },

                    {
                        "name": "CopyFromBlobtoSQLAzure",
                        "description": "Push data tooSql Azure",        
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OutputBlobTable"} ],
                        "outputs": [ {"name": "OutputSQLAzureTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "BlobSource"
                            },
                            "sink":
                            {
                                "type": "SqlSink",
                                "WriteBatchTimeout": "00:5:00",                
                            }            
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 2,
                            "timeout": "02:00:00"
                        }
                     }
                ]
            }
        }

Zkopírujte tuto definici JSON kanálu hello do souboru s názvem *pipelinedef.json* souboru a uložte ho tooa označuje umístění (zde předpokládá, že toobe *C:\temp\pipelinedef.json*). Vytvoření kanálu hello v ADF s hello následující rutiny Azure Powershellu:

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json

Zkontrolujte, jestli můžete hello kanálu na hello ADF v hello portálu Azure Classic zobrazí jako následující (při kliknutí na tlačítko hello diagram)

![ADF kanálu](media/machine-learning-data-science-move-sql-azure-adf/DJP1kji.png)

## <a name="adf-pipeline-start"></a>Hello spuštění kanálu
Hello kanálu můžete spustit nyní pomocí hello následující příkaz:

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

Hello *počátečním* a *koncovým datem* hodnoty parametrů potřebovat toobe nahradí skutečnými daty hello, mezi kterými chcete toorun hello kanálu.

Jakmile hello kanálu provede, musí být schopný toosee hello data se zobrazí v kontejneru hello vybraného pro objekt blob hello, jeden soubor za den.

Všimněte si, že jsme nebyly využít hello funkce poskytované službou ADF toopipe data postupně. Další informace o tom, jak toodo tato a další funkce poskytované verzí ADF, najdete v části hello [ADF dokumentaci](https://azure.microsoft.com/services/data-factory/).
