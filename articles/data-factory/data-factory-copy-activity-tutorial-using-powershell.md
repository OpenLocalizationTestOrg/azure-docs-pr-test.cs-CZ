---
title: "Kurz: Vytvoření kanálu toomove dat pomocí Azure PowerShell | Microsoft Docs"
description: "V tomto kurzu vytvoříte kanál služby Azure Data Factory s aktivitou kopírování pomocí Azure PowerShellu."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 71087349-9365-4e95-9847-170658216ed8
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 21406d7dfaa0c555b2538fbb52839d761c140fc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-data-factory-pipeline-that-moves-data-by-using-azure-powershell"></a>Kurz: Vytvoření kanálu Data Factory pro přesouvání dat pomocí Azure PowerShellu
> [!div class="op_single_selector"]
> * [Přehled a požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Šablona Azure Resource Manageru](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
>
>

V tomto článku se dozvíte, jak toocreate toouse prostředí PowerShell objekt pro vytváření dat s kanál, který kopíruje data z databáze Azure SQL tooan úložiště objektů blob v Azure. Pokud jste tooAzure nový objekt pro vytváření dat, pročtěte hello [Úvod tooAzure Data Factory](data-factory-introduction.md) článek před provedením tohoto kurzu.   

V tomto kurzu vytvoříte kanál s jednou aktivitou: aktivita kopírování. Aktivita kopírování Hello zkopíruje data z úložiště dat podporovaných podřízený tooa podporované datové úložiště. Seznam úložišť dat podporovaných jako zdroje a jímky najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Hello aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelné způsobem. Další informace o hello aktivitě kopírování najdete v tématu [aktivity přesunu dat](data-factory-data-movement-activities.md).

Kanál může obsahovat víc než jednu aktivitu. A dvě aktivity (spustit aktivitu po jiné) můžete řetězu nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit. Další informace naleznete, když přejdete na [více aktivit v kanálu](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).

> [!NOTE]
> Tento článek nepopisuje všechny rutiny služby Data Factory hello. Úplnou dokumentaci k těmto rutinám najdete v článku [Referenční informace o rutinách služby Data Factory](/powershell/module/azurerm.datafactories).
> 
> Hello datovém kanálu v tomto kurzu kopíruje data ze zdroje dat úložiště tooa cílového úložiště dat. Kurz týkající se jak tootransform dat pomocí Azure Data Factory najdete v části [kurz: sestavení dat tootransform kanálu pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).

## <a name="prerequisites"></a>Požadavky
- Dokončení požadavky uvedené v hello [požadavky kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) článku.
- Nainstalujte **Azure PowerShell**. Postupujte podle pokynů hello v [jak tooinstall a konfigurace prostředí Azure PowerShell](../powershell-install-configure.md).

## <a name="steps"></a>Kroky
Zde jsou hello kroky, které můžete provádět v rámci tohoto kurzu:

1. Vytvořte **datovou továrnu** Azure. V tomto kroku vytvoříte datovou továrnu s názvem ADFTutorialDataFactoryPSH. 
2. Vytvoření **propojené služby** v datové továrně hello. V tomto kroku vytvoříte dvě propojené služby typu: Azure Storage a Azure SQL Database. 
    
    Hello AzureStorageLinkedService propojuje účet úložiště Azure toohello datovou továrnu. Vytvořit kontejner a účet úložiště dat toothis odesílané jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

    AzureSqlLinkedService propojuje službu Azure SQL database toohello datovou továrnu. Hello data, která se zkopírují z úložiště objektů blob hello jsou uložena v této databázi. V této databázi jste v rámci [požadavků](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) vytvořili tabulku SQL.   
3. Vytvoření vstupní a výstupní **datové sady** v datové továrně hello.  
    
    Hello propojená služba úložiště Azure určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour účtu úložiště Azure. A určuje datovou sadu vstupního objektu blob hello hello kontejneru a složce hello, který obsahuje vstupní data hello.  

    Podobně hello služby Azure SQL Database propojené určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour Azure SQL database. A hello výstupní SQL tabulky datovou sadu Určuje, že se zkopíruje hello tabulky v hello databáze toowhich hello dat z úložiště objektů blob hello.
4. Vytvoření **kanálu** v datové továrně hello. V tomto kroku pomocí aktivity kopírování vytvoříte kanál.   
    
    Aktivita kopírování Hello zkopíruje data z objektu blob ve tabulku tooa úložiště objektů blob v Azure hello ve hello Azure SQL database. Můžete vytvořit aktivity kopírování v kanálu toocopy data z cílového všechny podporované zdrojové tooany podporována. Seznam podporovaných úložišť dat najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats). 
5. Monitorování kanálu hello. V tomto kroku budete **monitorování** hello řezy vstupní a výstupní datové sady pomocí prostředí PowerShell.

## <a name="create-a-data-factory"></a>Vytvoření datové továrny
> [!IMPORTANT]
> Dokončení [předpoklady pro kurz hello](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) Pokud jste tak již neučinili.   

Objekt pro vytváření dat může mít jeden nebo víc kanálů. Kanál může obsahovat jednu nebo víc aktivit. Například aktivitu kopírování dat toocopy z tooa zdroje cílového úložiště dat a toorun aktivitu HDInsight Hive tootransform skriptu Hive vstupní data tooproduct výstupní data. Začneme vytvořením objektu pro vytváření dat hello v tomto kroku.

1. Spusťte **PowerShell**. Nechte prostředí Azure PowerShell otevřené až hello konce tohoto kurzu. Pokud zavřete a znovu otevřete, toorun hello příkazů je třeba znovu.

    Spusťte následující příkaz hello a zadejte hello uživatelské jméno a heslo použít toosign v toohello portálu Azure:

    ```PowerShell
    Login-AzureRmAccount
    ```   
   
    Spusťte následující příkaz tooview hello všechny hello předplatná pro tento účet:

    ```PowerShell
    Get-AzureRmSubscription
    ```

    Spusťte následující příkaz tooselect hello předplatné, které chcete toowork s hello. Nahraďte  **&lt;NameOfAzureSubscription** &gt; s názvem hello předplatného Azure:

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```
2. Vytvořte skupinu prostředků Azure s názvem **ADFTutorialResourceGroup** spuštěním hello následující příkaz:

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
    
    Některé hello kroky v tomto kurzu vychází z předpokladu, že používáte hello skupinu prostředků s názvem **ADFTutorialResourceGroup**. Pokud používáte jiné skupině prostředků, je nutné toouse ho místo skupiny ADFTutorialResourceGroup v tomto kurzu.
3. Spustit hello **New-AzureRmDataFactory** rutiny toocreate objekt pro vytváření dat s názvem **ADFTutorialDataFactoryPSH**:  

    ```PowerShell
    $df=New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH –Location "West US"
    ```
    Tento název už nejspíš není volný. Proto zkontrolujte hello název objektu pro vytváření dat hello jedinečný přidáním předpony nebo přípony (například: ADFTutorialDataFactoryPSH05152017) a znovu spusťte příkaz hello.  

Všimněte si hello následující body:

* Hello název objektu pro vytváření dat Azure hello musí být globálně jedinečný. Pokud se zobrazí následující chyba hello, změňte název hello (například na název Váš_název_adftutorialdatafactorypsh). Při provádění postupů v tomto kurzu potom tento název používejte místo názvu ADFTutorialFactoryPSH. Informace o artefaktech služby Data Factory najdete v tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md).

    ```
    Data factory name “ADFTutorialDataFactoryPSH” is not available
    ```
* instance služby Data Factory toocreate, musí být Přispěvatel nebo správce hello předplatného Azure.
* Hello název objektu pro vytváření dat hello může zaregistrovat jako název DNS v hello budoucí a proto pak bude veřejně viditelný.
* Můžete obdržet hello následující chybě: "**toto předplatné není registrované toouse oboru názvů Microsoft.DataFactory.**" Udělejte jednu z následujících hello a zkuste znovu publikovat:

  * V prostředí Azure PowerShell spusťte následující příkaz tooregister hello Data Factory zprostředkovatele hello:

    ```PowerShell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

    Spusťte následující příkaz tooconfirm hello této hello zaregistrovat poskytovatele služby Data Factory:

    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * Přihlášení pomocí hello předplatného Azure toohello [portál Azure](https://portal.azure.com). Okno objekt pro vytváření dat tooa přejít, nebo vytvořte objekt pro vytváření dat v hello portál Azure. Tato akce automaticky registruje zprostředkovatele hello za vás.

## <a name="create-linked-services"></a>Vytvoření propojených služeb
Vytvoření propojených služeb v objektu pro vytváření toolink dat, data se ukládá a výpočetní služby toohello data factory. V tomto kurzu nebudete používat žádnou výpočetní službu jako je Azure HDInsight nebo Azure Data Lake Analytics. Budete používat dvě úložiště dat typu Azure Storage (zdroj) a Azure SQL Database (cíl). 

Vytvoříte tedy dvě propojené služby s názvem AzureStorageLinkedService a AzureSqlLinkedService typu: AzureStorage a AzureSqlDatabase.  

Hello AzureStorageLinkedService propojuje účet úložiště Azure toohello datovou továrnu. Tento účet úložiště se hello, jeden ve které jste vytvořili kontejner a objemu odeslaných dat hello jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

AzureSqlLinkedService propojuje službu Azure SQL database toohello datovou továrnu. Hello data, která se zkopírují z úložiště objektů blob hello jsou uložena v této databázi. Vytvořit hello tabulce emp v této databázi jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

### <a name="create-a-linked-service-for-an-azure-storage-account"></a>Vytvoření propojené služby pro účet úložiště Azure
V tomto kroku propojíte datovou továrnu tooyour účet úložiště Azure.

1. Vytvořte soubor JSON s názvem **AzureStorageLinkedService.json** v **C:\ADFGetStartedPSH** složku s hello následující obsah: (vytvořit hello složka ADFGetStartedPSH, pokud ještě neexistuje.)

    > [!IMPORTANT]
    > Nahraďte &lt;accountname&gt; a &lt;accountkey&gt; zadejte název a klíč účtu úložiště Azure před uložením souboru hello. 

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
2. V **prostředí Azure PowerShell**, přepínač toohello **ADFGetStartedPSH** složky.
4. Spustit hello **New-AzureRmDataFactoryLinkedService** rutiny toocreate hello propojené službě: **AzureStorageLinkedService**. Tato rutina a jiné rutiny služby Data Factory používané v tomto kurzu vyžaduje, aby vám toopass hodnoty pro hello **ResourceGroupName** a **DataFactoryName** parametry. Alternativně můžete předat objekt DataFactory hello vrácený hello rutiny New-AzureRmDataFactory, aniž by museli zadávat ResourceGroupName a DataFactoryName pokaždé, když spustíte rutinu. 

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\AzureStorageLinkedService.json
    ```
    Tady je ukázkový výstup hello:

    ```
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.LinkedServiceProperties
    ProvisioningState : Succeeded
    ``` 

    Druhý způsob vytváření Tato propojená služba je název skupiny prostředků toospecify a název objektu pro vytváření dat místo zadání objekt DataFactory hello.  

    ```PowerShell
    New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName <Name of your data factory> -File .\AzureStorageLinkedService.json
    ```

### <a name="create-a-linked-service-for-an-azure-sql-database"></a>Vytvoření propojené služby pro Azure SQL Database
V tomto kroku propojíte datovou továrnu tooyour databáze Azure SQL.

1. Vytvořte soubor JSON s názvem AzureSqlLinkedService.json ve složce C:\ADFGetStartedPSH s hello následující obsah:

    > [!IMPORTANT]
    > Položky &lt;název_serveru&gt;, &lt;název_databáze&gt;, &lt;username@servername&gt; a &lt;heslo&gt; nahraďte názvem serveru SQL Azure, názvem databáze, uživatelským účtem a heslem.
    
    ```json
    {
        "name": "AzureSqlLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": "Server=tcp:<server>.database.windows.net,1433;Database=<databasename>;User ID=<user>@<server>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        }
     }
    ```
2. Spusťte následující příkaz toocreate hello propojené služby:

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\AzureSqlLinkedService.json
    ```
    
    Tady je ukázkový výstup hello:

    ```
    LinkedServiceName : AzureSqlLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.LinkedServiceProperties
    ProvisioningState : Succeeded
    ```

   Potvrďte, že **povolit přístup k službám tooAzure** je zapnuté nastavení pro server databáze SQL. tooverify a zapnout ho, hello následující kroky:

    1. Přihlaste se toohello [portálu Azure](https://portal.azure.com)
    2. Klikněte na tlačítko **další služby >** na levé straně hello a klikněte na **servery SQL** v hello **databáze** kategorie.
    3. V seznamu hello systému SQL Server, vyberte svůj server.
    4. V okně SQL serveru text hello, klikněte na tlačítko **zobrazit nastavení brány firewall** odkaz.
    5. V hello **nastavení brány Firewall** okně klikněte na tlačítko **ON** pro **povolit přístup k službám tooAzure**.
    6. Klikněte na tlačítko **Uložit** na panelu nástrojů hello. 

## <a name="create-datasets"></a>Vytvoření datových sad
V předchozím kroku hello jste vytvořili propojené služby toolink, účet úložiště Azure a Azure SQL database tooyour objekt pro vytváření dat. V tomto kroku definujete dvě datové sady s názvem InputDataset a OutputDataset, které představují vstupní a výstupní data, která jsou uložená v úložištích dat hello odkazuje AzureStorageLinkedService a AzureSqlLinkedService.

Hello propojená služba úložiště Azure určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour účtu úložiště Azure. A datovou sadu hello vstupního objektu blob (InputDataset) určuje hello kontejneru a složce hello, který obsahuje vstupní data hello.  

Podobně hello služby Azure SQL Database propojené určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour Azure SQL database. A hello výstupní datovou sadu tabulky SQL (OututDataset) určuje hello tabulky v hello databáze hello toowhich zkopírování dat z úložiště objektů blob hello. 

### <a name="create-an-input-dataset"></a>Vytvoření vstupní datové sady
V tomto kroku vytvoříte datové sady s názvem InputDataset, který ukazuje soubor blob tooa (emp.txt) v kořenové složce hello kontejner objektů blob (adftutorial) v hello Azure Storage reprezentované hello AzureStorageLinkedService propojené služby. Pokud nechcete zadat hodnotu pro název souboru hello (nebo ji přeskočit), jsou data ze všech objektů BLOB ve složce vstupní hello zkopírovaný toohello cílový. V tomto kurzu zadejte hodnotu pro název souboru hello.  

1. Vytvořte soubor JSON s názvem **InputDataset.json** v hello **C:\ADFGetStartedPSH** složku s hello následující obsah:

    ```json
    {
        "name": "InputDataset",
        "properties": {
            "structure": [
                {
                    "name": "FirstName",
                    "type": "String"
                },
                {
                    "name": "LastName",
                    "type": "String"
                }
            ],
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "emp.txt",
                "folderPath": "adftutorial/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
     }
    ```

    Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:

    | Vlastnost | Popis |
    |:--- |:--- |
    | type | Hello vlastnost Typ nastavena příliš**AzureBlob** protože data se nachází v Azure blob storage. |
    | linkedServiceName | Odkazuje toohello **AzureStorageLinkedService** kterou jste vytvořili dříve. |
    | folderPath | Určuje objekt blob hello **kontejneru** a hello **složky** , který obsahuje vstupní objekty BLOB. V tomto kurzu adftutorial hello kontejner objektů blob, složka hello kořenové složky. | 
    | fileName | Tato vlastnost je nepovinná. Pokud ji vynecháte, jsou vybrány všechny soubory z hello folderPath. V tomto kurzu **emp.txt** zadaný pro hello název souboru, takže pouze tento soubor je převzata pro zpracování. |
    | format -> type |vstupní soubor Hello je ve formátu textu hello, takže použijeme **TextFormat**. |
    | columnDelimiter | Hello sloupců ve vstupním souboru hello oddělené **čárku (`,`)**. |
    | frequency/interval | je příliš nastavena frekvence Hello**hodinu** a interval je nastaven příliš**1**, což znamená, že hello vstupní řezy jsou dostupné **každou hodinu**. Jinými slovy, hello služba Data Factory hledá vstupní data každou hodinu v kořenové složce hello kontejner objektů blob (**adftutorial**) jste zadali. Hledá hello dat v rámci hello kanálu počáteční a koncové dobu, není před nebo po této doby.  |
    | external | Tato vlastnost nastavena příliš**true** Pokud hello data nevygenerovala pomocí tohoto kanálu. Hello vstupní data v tomto kurzu je v souboru emp.txt hello, který není generované tímto kanálem, abyste nám nastavit tuto vlastnost tootrue. |

    Další informace o těchto vlastnostech JSON najdete v článku [konektor Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).
2. Spusťte následující příkaz datovou sadu služby Data Factory hello toocreate hello.

    ```PowerShell  
    New-AzureRmDataFactoryDataset $df -File .\InputDataset.json
    ```
    Tady je ukázkový výstup hello:

    ```
    DatasetName       : InputDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Availability      : Microsoft.Azure.Management.DataFactories.Common.Models.Availability
    Location          : Microsoft.Azure.Management.DataFactories.Models.AzureBlobDataset
    Policy            : Microsoft.Azure.Management.DataFactories.Common.Models.Policy
    Structure         : {FirstName, LastName}
    Properties        : Microsoft.Azure.Management.DataFactories.Models.DatasetProperties
    ProvisioningState : Succeeded
    ```

### <a name="create-an-output-dataset"></a>Vytvoření výstupní datové sady
V této části hello kroku vytvoříte výstupní datovou sadu s názvem **OutputDataset**. Tato datová sada body tooa tabulky SQL ve hello Azure SQL database reprezentované **AzureSqlLinkedService**. 

1. Vytvořte soubor JSON s názvem **OutputDataset.json** v hello **C:\ADFGetStartedPSH** složku s hello následující obsah:

    ```json
    {
        "name": "OutputDataset",
        "properties": {
            "structure": [
                {
                    "name": "FirstName",
                    "type": "String"
                },
                {
                    "name": "LastName",
                    "type": "String"
                }
            ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "emp"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```

    Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:

    | Vlastnost | Popis |
    |:--- |:--- |
    | type | Hello vlastnost Typ nastavena příliš**AzureSqlTable** protože data jsou zkopírované tooa tabulky v databázi Azure SQL. |
    | linkedServiceName | Odkazuje toohello **AzureSqlLinkedService** kterou jste vytvořili dříve. |
    | tableName | Zadaný hello **tabulky** toowhich hello data budou zkopírována. | 
    | frequency/interval | Hello je nastavena frekvence příliš**hodinu** a interval je **1**, což znamená, že vytváří řezy výstup hello **každou hodinu** mezi hello kanálu počáteční a koncové krát, ne dřív nebo Po této doby.  |

    Existují tři sloupce – **ID**, **FirstName**, a **LastName** – v tabulce emp hello v databázi hello. ID je sloupec identity, takže je nutné pouze toospecify **FirstName** a **LastName** sem.

    Další informace o těchto vlastnostech JSON najdete v článku [konektor Azure SQL](data-factory-azure-sql-connector.md#dataset-properties).
2. Spusťte následující příkaz toocreate hello data factory dataset hello.

    ```PowerShell   
    New-AzureRmDataFactoryDataset $df -File .\OutputDataset.json
    ```

    Tady je ukázkový výstup hello:

    ```
    DatasetName       : OutputDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Availability      : Microsoft.Azure.Management.DataFactories.Common.Models.Availability
    Location          : Microsoft.Azure.Management.DataFactories.Models.AzureSqlTableDataset
    Policy            :
    Structure         : {FirstName, LastName}
    Properties        : Microsoft.Azure.Management.DataFactories.Models.DatasetProperties
    ProvisioningState : Succeeded
    ```

## <a name="create-a-pipeline"></a>Vytvoření kanálu
V tomto kroku vytvoříte kanál s **aktivitou kopírování**, která používá **InputDataset** jako vstup a **OutputDataset** jako výstup.

Výstupní datové sady v současné době je, jaké jednotky hello plán. V tomto kurzu výstupní datové sady je nakonfigurované tooproduce řez jednou za hodinu. Hello kanálu má čas spuštění a čas ukončení, které jsou od sebe, jeden den, což je 24 hodin. Proto 24 řezů výstupní datové sady vyprodukované hello kanálu. 


1. Vytvořte soubor JSON s názvem **C:\adfgetstartedpsh** v hello **C:\ADFGetStartedPSH** složku s hello následující obsah:

    ```json   
    {
      "name": "ADFTutorialPipeline",
      "properties": {
        "description": "Copy data from a blob tooAzure SQL table",
        "activities": [
          {
            "name": "CopyFromBlobToSQL",
            "type": "Copy",
            "inputs": [
              {
                "name": "InputDataset"
              }
            ],
            "outputs": [
              {
                "name": "OutputDataset"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "SqlSink",
                "writeBatchSize": 10000,
                "writeBatchTimeout": "60:00:00"
              }
            },
            "Policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
        ],
        "start": "2017-05-11T00:00:00Z",
        "end": "2017-05-12T00:00:00Z"
      }
    } 
    ```
    Všimněte si hello následující body:
   
    - V části hello aktivit je jenom jedna aktivita jejichž **typ** je nastaven příliš**kopie**. Další informace o aktivitě kopírování hello najdete v tématu [aktivity přesunu dat](data-factory-data-movement-activities.md). V řešeních služby Data Factory můžete také použít [aktivity transformace dat](data-factory-data-transformation-activities.md).
    - Vstup aktivity hello nastaven příliš**InputDataset** a výstup hello aktivity je nastavený příliš**OutputDataset**. 
    - V hello **rámci typeProperties** části **BlobSource** je zadán jako typ zdroje hello a **SqlSink** je zadán jako typ jímky hello. Úplný seznam úložišť dat nepodporuje aktivity kopírování hello jako zdroje a jímky, najdete v části [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats). toolearn jak toouse konkrétní podporované datové úložiště jako zdroj/jímka, klikněte na odkaz hello v tabulce hello.  
     
    Nahraďte hodnotu hello hello **spustit** vlastnost s hello aktuální den a **end** hodnotu s hello další den. Můžete zadat jenom část data hello a přeskočit část času hello hello datum a čas. Například "2016-02-03", který je ekvivalentní příliš "2016-02-03T00:00:00Z"
     
    Počáteční a koncové hodnoty data a času musí být ve [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601). Například: 2016-10-14T16:32:41Z. Hello **end** čas je nepovinný, ale používáme v tomto kurzu. 
     
    Pokud nezadáte hodnotu hello **end** vlastnost, vypočítá se jako "**start + 48 hodin**". bez omezení, zadejte toorun hello kanálu **9999-09-09** hello hodnotu hello **end** vlastnost.
     
    V předchozím příkladu hello jsou 24 datových řezů jako se vytvářejí každou hodinu.

    Popisy vlastností JSON použitých v definici kanálu najdete v článku [Vytvoření kanálů](data-factory-create-pipelines.md). Popisy vlastností JSON použitých v definici aktivity kopírování najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md). Popisy vlastností JSON podporovaných zdrojem BlobSource najdete v článku [Konektor Azure Blob](data-factory-azure-blob-connector.md). Popisy vlastností JSON podporovaných jímkou SqlSink najdete v článku [Konektor Azure SQL Database](data-factory-azure-sql-connector.md).
2. Spusťte následující příkaz toocreate hello data factory tabulka hello.

    ```PowerShell   
    New-AzureRmDataFactoryPipeline $df -File .\ADFTutorialPipeline.json
    ```

    Tady je ukázkový výstup hello: 

    ```
    PipelineName      : ADFTutorialPipeline
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.PipelinePropertie
    ProvisioningState : Succeeded
    ```

**Blahopřejeme!** Úspěšně jste vytvořili objekt pro vytváření dat Azure s kanálu toocopy dat z Azure SQL database tooan úložiště objektů blob v Azure. 

## <a name="monitor-hello-pipeline"></a>Monitorování kanálu hello
V tomto kroku budete pomocí prostředí Azure PowerShell toomonitor co se děje v objektu pro vytváření dat Azure.

1. Nahraďte &lt;DataFactoryName&gt; hello název objektu pro vytváření dat a spusťte **Get-AzureRmDataFactory**a přiřaďte výstup tooa hello proměnné $df.

    ```PowerShell  
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name <DataFactoryName>
    ```

    Například:
    ```PowerShell
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH0516
    ```
    
    Potom spusťte tiskové hello obsah hello toosee $df následující výstup: 
    
    ```
    PS C:\ADFGetStartedPSH> $df
    
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DataFactoryId     : 6f194b34-03b3-49ab-8f03-9f8a7b9d3e30
    ResourceGroupName : ADFTutorialResourceGroup
    Location          : West US
    Tags              : {}
    Properties        : Microsoft.Azure.Management.DataFactories.Models.DataFactoryProperties
    ProvisioningState : Succeeded
    ```
2. Spustit **Get-AzureRmDataFactorySlice** tooget podrobnosti o všech řezech tabulky hello **OutputDataset**, což je hello výstupní datovou sadu hello kanálu.  

    ```PowerShell   
    Get-AzureRmDataFactorySlice $df -DatasetName OutputDataset -StartDateTime 2017-05-11T00:00:00Z
    ```

   Toto nastavení by měl odpovídat hello **spustit** hodnota v kódu JSON kanálu hello. Měli byste vidět 24 řezů, pro každou hodinu od 12: 00 aktuálního dne too12 hello mě hello další den.

   Tady jsou tři ukázkové řezy z výstupu hello: 

    ``` 
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DatasetName       : OutputDataset
    Start             : 5/11/2017 11:00:00 PM
    End               : 5/12/2017 12:00:00 AM
    RetryCount        : 0
    State             : Ready
    SubState          :
    LatencyStatus     :
    LongRetryCount    : 0

    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DatasetName       : OutputDataset
    Start             : 5/11/2017 9:00:00 PM
    End               : 5/11/2017 10:00:00 PM
    RetryCount        : 0
    State             : InProgress
    SubState          :
    LatencyStatus     :
    LongRetryCount    : 0   

    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DatasetName       : OutputDataset
    Start             : 5/11/2017 8:00:00 PM
    End               : 5/11/2017 9:00:00 PM
    RetryCount        : 0
    State             : Waiting
    SubState          : ConcurrencyLimit
    LatencyStatus     :
    LongRetryCount    : 0
    ```
3. Spustit **Get-AzureRmDataFactoryRun** tooget hello podrobnosti aktivity běží **konkrétní** řez. Zkopírujte hodnotu data a času hello z hello výstup hello předchozí příkaz toospecify hello hodnotu pro parametr StartDateTime hello. 

    ```PowerShell  
    Get-AzureRmDataFactoryRun $df -DatasetName OutputDataset -StartDateTime "5/11/2017 09:00:00 PM"
    ```

   Tady je ukázkový výstup hello: 

    ```
    Id                  : c0ddbd75-d0c7-4816-a775-704bbd7c7eab_636301332000000000_636301368000000000_OutputDataset
    ResourceGroupName   : ADFTutorialResourceGroup
    DataFactoryName     : ADFTutorialDataFactoryPSH0516
    DatasetName         : OutputDataset
    ProcessingStartTime : 5/16/2017 8:00:33 PM
    ProcessingEndTime   : 5/16/2017 8:01:36 PM
    PercentComplete     : 100
    DataSliceStart      : 5/11/2017 9:00:00 PM
    DataSliceEnd        : 5/11/2017 10:00:00 PM
    Status              : Succeeded
    Timestamp           : 5/16/2017 8:00:33 PM
    RetryAttempt        : 0
    Properties          : {}
    ErrorMessage        :
    ActivityName        : CopyFromBlobToSQL
    PipelineName        : ADFTutorialPipeline
    Type                : Copy  
    ```

Úplnou dokumentaci o rutinách služby Data Factory najdete v článku [Referenční informace o rutinách služby Data Factory](/powershell/module/azurerm.datafactories).

## <a name="summary"></a>Souhrn
V tomto kurzu jste vytvořili Azure data factory toocopy dat z Azure SQL database tooan objektů blob v Azure. Můžete použít PowerShell toocreate hello objekt pro vytváření dat, propojených služeb, datových sad a kanálu. Zde jsou hello kroků, které jste provedli v tomto kurzu:  

1. Vytvořili jste **objekt pro vytváření dat** Azure.
2. Vytvořili jste **propojené služby**:

   a. **Azure Storage** propojené služby toolink účtu úložiště Azure, který obsahuje vstupní data.     
   b. **Azure SQL** propojené služby toolink vaší databázi SQL, který obsahuje hello výstupní data.
3. Vytvořili jste **datové sady**, které popisují vstupní data a výstupní data pro kanály.
4. Vytvoření **kanálu** s **aktivity kopírování**, s **BlobSource** jako zdroj hello a **SqlSink** jako jímku hello.

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste v operaci kopírování použili úložiště objektů blob jako zdrojové úložiště dat a databázi Azure SQL jako cílové úložiště dat. Hello následující tabulka obsahuje seznam úložiště dat, které jsou podporované jako zdroje a cíle pomocí aktivity kopírování hello: 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

toolearn o tom, jak toocopy data z datové úložiště, klikněte na odkaz hello hello úložiště dat v tabulce hello. 

