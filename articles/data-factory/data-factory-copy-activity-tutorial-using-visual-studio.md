---
title: "Kurz: Vytvoření kanálu s aktivitou kopírování pomocí sady Visual Studio | Dokumentace Microsoftu"
description: "V tomto kurzu vytvoříte kanál služby Azure Data Factory s aktivitou kopírování pomocí sady Visual Studio."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1751185b-ce0a-4ab2-a9c3-e37b4d149ca3
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: d99d8875807bab41f5122ab95a09f83f82923529
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-visual-studio"></a>Kurz: Vytvoření kanálu s aktivitou kopírování pomocí sady Visual Studio
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

V tomto článku se dozvíte, jak toouse hello Microsoft Visual Studio toocreate objekt pro vytváření dat s kanál, který kopíruje data z databáze Azure SQL tooan úložiště objektů blob v Azure. Pokud jste tooAzure nový objekt pro vytváření dat, pročtěte hello [Úvod tooAzure Data Factory](data-factory-introduction.md) článek před provedením tohoto kurzu.   

V tomto kurzu vytvoříte kanál s jednou aktivitou: aktivita kopírování. Aktivita kopírování Hello zkopíruje data z úložiště dat podporovaných podřízený tooa podporované datové úložiště. Seznam úložišť dat podporovaných jako zdroje a jímky najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Hello aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelné způsobem. Další informace o hello aktivitě kopírování najdete v tématu [aktivity přesunu dat](data-factory-data-movement-activities.md).

Kanál může obsahovat víc než jednu aktivitu. A dvě aktivity (spustit aktivitu po jiné) můžete řetězu nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit. Další informace naleznete, když přejdete na [více aktivit v kanálu](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).

> [!NOTE] 
> Hello datovém kanálu v tomto kurzu kopíruje data ze zdroje dat úložiště tooa cílového úložiště dat. Kurz týkající se jak tootransform dat pomocí Azure Data Factory najdete v části [kurz: sestavení dat tootransform kanálu pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).

## <a name="prerequisites"></a>Požadavky
1. Pročtěte [přehled kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) článek a dokončení hello **požadavek** kroky.       
2. instance služby Data Factory toocreate, musíte být členem skupiny hello [Přispěvatel objekt pro vytváření dat](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role na úrovni předplatného nebo prostředků skupiny hello.
3. Musíte mít nainstalované ve vašem počítači hello tyto položky: 
   * Visual Studio 2013 nebo Visual Studio 2015.
   * Stáhněte si sadu Azure SDK pro Visual Studio 2013 nebo Visual Studio 2015. Přejděte příliš[stránku položek ke stažení Azure](https://azure.microsoft.com/downloads/) a klikněte na tlačítko **VS 2013** nebo **VS 2015** v hello **.NET** části.
   * Stáhnout hello nejnovější modul plug-in Azure Data Factory pro Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) nebo [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Můžete také aktualizovat modul plug-in hello provedením následujících kroků hello: hello nabídce klikněte na **nástroje** -> **rozšíření a aktualizace** -> **Online**  ->  **Galerie sady visual Studio** -> **Microsoft Azure Data Factory Tools for Visual Studio** -> **aktualizace**.

## <a name="steps"></a>Kroky
Zde jsou hello kroky, které můžete provádět v rámci tohoto kurzu:

1. Vytvoření **propojené služby** v datové továrně hello. V tomto kroku vytvoříte dvě propojené služby typu: Azure Storage a Azure SQL Database. 
    
    Hello AzureStorageLinkedService propojuje účet úložiště Azure toohello datovou továrnu. Vytvořit kontejner a účet úložiště dat toothis odesílané jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

    AzureSqlLinkedService propojuje službu Azure SQL database toohello datovou továrnu. Hello data, která se zkopírují z úložiště objektů blob hello jsou uložena v této databázi. V této databázi jste v rámci [požadavků](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) vytvořili tabulku SQL.     
2. Vytvoření vstupní a výstupní **datové sady** v datové továrně hello.  
    
    Hello propojená služba úložiště Azure určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour účtu úložiště Azure. A určuje datovou sadu vstupního objektu blob hello hello kontejneru a složce hello, který obsahuje vstupní data hello.  

    Podobně hello služby Azure SQL Database propojené určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour Azure SQL database. A hello výstupní SQL tabulky datovou sadu Určuje, že se zkopíruje hello tabulky v hello databáze toowhich hello dat z úložiště objektů blob hello.
3. Vytvoření **kanálu** v datové továrně hello. V tomto kroku pomocí aktivity kopírování vytvoříte kanál.   
    
    Aktivita kopírování Hello zkopíruje data z objektu blob ve tabulku tooa úložiště objektů blob v Azure hello ve hello Azure SQL database. Můžete vytvořit aktivity kopírování v kanálu toocopy data z cílového všechny podporované zdrojové tooany podporována. Seznam podporovaných úložišť dat najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats). 
4. Vytvořte **datovou továrnu** Azure, když nasazujete entity služby Data Factory (propojené služby, datové sady / tabulky a kanály). 

## <a name="create-visual-studio-project"></a>Vytvoření projektu v sadě Visual Studio
1. Spusťte **Visual Studio 2015**. Klikněte na tlačítko **soubor**, bod příliš**nový**a klikněte na tlačítko **projektu**. Měli byste vidět hello **nový projekt** dialogové okno.  
2. V hello **nový projekt** dialogové okno, vyberte hello **DataFactory** šablonu a klikněte na **prázdný projekt Data Factory**.  
   
    ![Dialogové okno Nový projekt](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)
3. Zadejte název hello hello projektu, umístění pro hello řešení a název hello řešení a pak klikněte na tlačítko **OK**.
   
    ![Průzkumník řešení](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png)    

## <a name="create-linked-services"></a>Vytvoření propojených služeb
Vytvoření propojených služeb v objektu pro vytváření toolink dat, data se ukládá a výpočetní služby toohello data factory. V tomto kurzu nebudete používat žádnou výpočetní službu jako je Azure HDInsight nebo Azure Data Lake Analytics. Budete používat dvě úložiště dat typu Azure Storage (zdroj) a Azure SQL Database (cíl). 

Proto vytvoříte dvě propojené služby typu: AzureStorage a AzureSqlDatabase.  

Hello Azure Storage propojená služba propojuje účet úložiště Azure toohello datovou továrnu. Tento účet úložiště se hello, jeden ve které jste vytvořili kontejner a objemu odeslaných dat hello jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

Azure SQL propojená služba propojuje toohello datovou továrnu Azure SQL database. Hello data, která se zkopírují z úložiště objektů blob hello jsou uložena v této databázi. Vytvořit hello tabulce emp v této databázi jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Propojené služby propojují úložiště dat nebo výpočetní služby tooan pro vytváření dat Azure. V tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pro všechny hello zdroje a jímky nepodporuje hello aktivity kopírování. V tématu [výpočetní propojené služby](data-factory-compute-linked-services.md) hello seznam výpočetní služby podporovaných službou Data Factory. V tomto kurzu žádné výpočetní služby nepoužijete. 

### <a name="create-hello-azure-storage-linked-service"></a>Vytvoření hello propojené služby Azure Storage
1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **propojené služby**, bod příliš**přidat**a klikněte na tlačítko **novou položku**.      
2. V hello **přidat novou položku** dialogové okno, vyberte **propojená služba úložiště Azure** hello seznamu a klikněte na **přidat**. 
   
    ![Nová propojená služba](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
3. Nahraďte `<accountname>` a `<accountkey>`* s hello název účtu úložiště Azure a jeho klíčem. 
   
    ![Propojená služba Azure Storage](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)
4. Uložit hello **AzureStorageLinkedService1.json** souboru.

    Další informace o vlastnostech JSON v definici hello propojené služby najdete v tématu [konektor Azure Blob Storage](data-factory-azure-blob-connector.md#linked-service-properties) článku.

### <a name="create-hello-azure-sql-linked-service"></a>Vytvoření hello propojená služba Azure SQL
1. Klikněte pravým tlačítkem na **propojené služby** uzel v hello **Průzkumníku řešení** znovu bodu příliš**přidat**a klikněte na tlačítko **novou položku**. 
2. Tentokrát vyberte **Propojená služba Azure SQL** a klikněte na **Přidat**. 
3. V hello **AzureSqlLinkedService1.json soubor**, nahraďte `<servername>`, `<databasename>`, `<username@servername>`, a `<password>` s názvy serveru Azure SQL, databáze, uživatelský účet a heslo.    
4. Uložit hello **AzureSqlLinkedService1.json** souboru. 
    
    Další informace o těchto vlastnostech JSON najdete v článku [Konektor služby Azure SQL Database](data-factory-azure-sql-connector.md#linked-service-properties).


## <a name="create-datasets"></a>Vytvoření datových sad
V předchozím kroku hello jste vytvořili propojené služby toolink, účet úložiště Azure a Azure SQL database tooyour objekt pro vytváření dat. V tomto kroku definujete dvě datové sady s názvem InputDataset a OutputDataset, které představují vstupní a výstupní data, která jsou uložená v úložištích dat hello odkazuje AzureStorageLinkedService1 a AzureSqlLinkedService1.

Hello propojená služba úložiště Azure určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour účtu úložiště Azure. A datovou sadu hello vstupního objektu blob (InputDataset) určuje hello kontejneru a složce hello, který obsahuje vstupní data hello.  

Podobně hello služby Azure SQL Database propojené určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour Azure SQL database. A hello výstupní datovou sadu tabulky SQL (OututDataset) určuje hello tabulky v hello databáze hello toowhich zkopírování dat z úložiště objektů blob hello. 

### <a name="create-input-dataset"></a>Vytvoření vstupní datové sady
V tomto kroku vytvoříte datové sady s názvem InputDataset, který ukazuje soubor blob tooa (emp.txt) v kořenové složce hello kontejner objektů blob (adftutorial) v hello Azure Storage reprezentované hello AzureStorageLinkedService1 propojené služby. Pokud nechcete zadat hodnotu pro název souboru hello (nebo ji přeskočit), jsou data ze všech objektů BLOB ve složce vstupní hello zkopírovaný toohello cílový. V tomto kurzu zadejte hodnotu pro název souboru hello. 

Zde je použít hello termín "tabulky" místo "datové sady". Tabulka je obdélníková datová sada a je jediným typem datové sady, které jsou nyní podporovány hello. 

1. Klikněte pravým tlačítkem na **tabulky** v hello **Průzkumníku řešení**, bod příliš**přidat**a klikněte na tlačítko **novou položku**.
2. V hello **přidat novou položku** dialogové okno, vyberte **objektů Blob v Azure**a klikněte na tlačítko **přidat**.   
3. Nahraďte hello JSON text hello následující text a uložte hello **AzureBlobLocation1.json** souboru. 

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
      "linkedServiceName": "AzureStorageLinkedService1",
      "typeProperties": {
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

### <a name="create-output-dataset"></a>Vytvoření výstupní datové sady
V tomto kroku vytvoříte výstupní datovou sadu s názvem **OutputDataset**. Tato datová sada body tooa tabulky SQL ve hello Azure SQL database reprezentované **AzureSqlLinkedService1**. 

1. Klikněte pravým tlačítkem na **tabulky** v hello **Průzkumníku řešení** znovu bodu příliš**přidat**a klikněte na tlačítko **novou položku**.
2. V hello **přidat novou položku** dialogové okno, vyberte **Azure SQL**a klikněte na tlačítko **přidat**. 
3. Nahraďte hello JSON text hello následující JSON a uložte hello **AzureSqlTableLocation1.json** souboru.

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
       "linkedServiceName": "AzureSqlLinkedService1",
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

## <a name="create-pipeline"></a>Vytvoření kanálu
V tomto kroku vytvoříte kanál s **aktivitou kopírování**, která používá **InputDataset** jako vstup a **OutputDataset** jako výstup.

Výstupní datové sady v současné době je, jaké jednotky hello plán. V tomto kurzu výstupní datové sady je nakonfigurované tooproduce řez jednou za hodinu. Hello kanálu má čas spuštění a čas ukončení, které jsou od sebe, jeden den, což je 24 hodin. Proto 24 řezů výstupní datové sady vyprodukované hello kanálu. 

1. Klikněte pravým tlačítkem na **kanály** v hello **Průzkumníku řešení**, bod příliš**přidat**a klikněte na tlačítko **novou položku**.  
2. Vyberte **kanál kopírování dat** v hello **přidat novou položku** dialogové okno a klikněte na tlačítko **přidat**. 
3. Hello JSON nahraďte hello následující JSON a uložte hello **CopyActivity1.json** souboru.

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
             "style": "StartOfInterval",
             "retry": 0,
             "timeout": "01:00:00"
           }
         }
       ],
       "start": "2017-05-11T00:00:00Z",
       "end": "2017-05-12T00:00:00Z",
       "isPaused": false
     }
    }
    ```   
    - V části hello aktivit je jenom jedna aktivita jejichž **typ** je nastaven příliš**kopie**. Další informace o aktivitě kopírování hello najdete v tématu [aktivity přesunu dat](data-factory-data-movement-activities.md). V řešeních služby Data Factory můžete také použít [aktivity transformace dat](data-factory-data-transformation-activities.md).
    - Vstup aktivity hello nastaven příliš**InputDataset** a výstup hello aktivity je nastavený příliš**OutputDataset**. 
    - V hello **rámci typeProperties** části **BlobSource** je zadán jako typ zdroje hello a **SqlSink** je zadán jako typ jímky hello. Úplný seznam úložišť dat nepodporuje aktivity kopírování hello jako zdroje a jímky, najdete v části [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats). toolearn jak toouse konkrétní podporované datové úložiště jako zdroj/jímka, klikněte na odkaz hello v tabulce hello.  
     
    Nahraďte hodnotu hello hello **spustit** vlastnost s hello aktuální den a **end** hodnotu s hello další den. Můžete zadat jenom část data hello a přeskočit část času hello hello datum a čas. Například "2016-02-03", který je ekvivalentní příliš "2016-02-03T00:00:00Z"
     
    Počáteční a koncové hodnoty data a času musí být ve [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601). Například: 2016-10-14T16:32:41Z. Hello **end** čas je nepovinný, ale používáme v tomto kurzu. 
     
    Pokud nezadáte hodnotu hello **end** vlastnost, vypočítá se jako "**start + 48 hodin**". bez omezení, zadejte toorun hello kanálu **9999-09-09** hello hodnotu hello **end** vlastnost.
     
    V předchozím příkladu hello jsou 24 datových řezů jako se vytvářejí každou hodinu.

    Popisy vlastností JSON použitých v definici kanálu najdete v článku [Vytvoření kanálů](data-factory-create-pipelines.md). Popisy vlastností JSON použitých v definici aktivity kopírování najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md). Popisy vlastností JSON podporovaných zdrojem BlobSource najdete v článku [Konektor Azure Blob](data-factory-azure-blob-connector.md). Popisy vlastností JSON podporovaných jímkou SqlSink najdete v článku [Konektor Azure SQL Database](data-factory-azure-sql-connector.md).

## <a name="publishdeploy-data-factory-entities"></a>Publikování/nasazení entit služby Data Factory
V tomto kroku publikujete entity služby Data Factory (propojené služby, datové sady a kanál), které jste vytvořili dříve. Můžete také určit, že název hello hello nové data factory toobe vytvořili toohold tyto entity.  

1. Klikněte pravým tlačítkem na projekt v Průzkumníku řešení hello a klikněte na tlačítko **publikovat**. 
2. Pokud se zobrazí **přihlášení tooyour účtu Microsoft** dialogové okno, zadejte svoje přihlašovací údaje pro hello účet, který má předplatné Azure a klikněte na tlačítko **přihlášení**.
3. Měli byste vidět hello následující dialogové okno:
   
   ![Dialogové okno publikování](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)
4. Na stránce objektu pro vytváření dat konfigurace hello hello následující kroky: 
   
   1. Vyberte možnost **Create New Data Factory** (Vytvořit nový objekt pro vytváření dat).
   2. Zadejte **VSTutorialFactory** jako **Název**.  
      
      > [!IMPORTANT]
      > Hello název objektu pro vytváření dat Azure hello musí být globálně jedinečný. Pokud se při publikování zobrazí chybu o hello název objektu pro vytváření dat, změňte název hello hello objekt pro vytváření dat (třeba na Váš_název_vstutorialfactory) a zkuste publikování znovu. V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.        
      > 
      > 
   3. Vyberte předplatné Azure pro hello **předplatné** pole.
      
      > [!IMPORTANT]
      > Pokud se nezobrazí žádné předplatné. Ujistěte se, že jste se přihlásili pomocí účtu, který je správce nebo spolusprávce předplatného hello.  
      > 
      > 
   4. Vyberte hello **skupiny prostředků** pro hello data factory toobe vytvořili. 
   5. Vyberte hello **oblast** hello data Factory. V rozevíracím seznamu hello jsou uvedeny pouze oblasti nepodporuje hello služba Data Factory.
   6. Klikněte na tlačítko **Další** tooswitch toohello **publikování položky** stránky.
      
       ![Stránka konfigurace služby Data Factory](media/data-factory-copy-activity-tutorial-using-visual-studio/configure-data-factory-page.png)   
5. V hello **publikování položky** stránky, ujistěte se, že všechny hello datové továrny entity jsou vybrané a klikněte na tlačítko **Další** tooswitch toohello **Souhrn** stránky.
   
   ![Stránka publikování položek](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-items-page.png)     
6. Zkontrolujte hello shrnutí a klikněte na **Další** toostart hello nasazení proces a zobrazení hello **stav nasazení**.
   
   ![Stránka souhrnu publikování](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-summary-page.png)
7. V hello **stav nasazení** stránky, měli byste vidět hello stav procesu nasazení hello. Po dokončení hello nasazení, klikněte na tlačítko Dokončit.
 
   ![Stránka stavu nasazení](media/data-factory-copy-activity-tutorial-using-visual-studio/deployment-status.png)

Všimněte si hello následující body: 

* Pokud se zobrazí chyba hello: "Toto předplatné není registrované toouse oboru názvů Microsoft.DataFactory", proveďte jednu z následujících hello a zkuste znovu publikovat: 
  
  * V prostředí Azure PowerShell spusťte následující příkaz tooregister hello Data Factory zprostředkovatele hello. 

    ```PowerShell    
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
    Můžete spustit následující příkaz tooconfirm hello této hello zaregistrovat poskytovatele služby Data Factory. 
    
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * Přihlášení pomocí hello předplatného Azure hello [portál Azure](https://portal.azure.com) a přejděte tooa okno objekt pro vytváření dat (nebo) vytvořte objekt pro vytváření dat v hello portálu Azure. Tato akce automaticky registruje zprostředkovatele hello za vás.
* Hello název objektu pro vytváření dat hello možné zaregistrovat jako název DNS v hello budoucí a proto pak bude veřejně viditelný.

> [!IMPORTANT]
> instance služby Data Factory toocreate, je třeba toobe správce nebo spolusprávce předplatného Azure Dobrý den

## <a name="monitor-pipeline"></a>Monitorování kanálu
Přejděte toohello Domovská stránka objektu pro vytváření dat:

1. Přihlaste se příliš[portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko **další služby** hello levé nabídce a klikněte na tlačítko **datové továrny**.

    ![Procházet -> Datové továrny](media/data-factory-copy-activity-tutorial-using-visual-studio/browse-data-factories.png)
3. Začněte psát název vaší služby data factory hello.

    ![Název datové továrny](media/data-factory-copy-activity-tutorial-using-visual-studio/enter-data-factory-name.png) 
4. Klikněte na datovou továrnu hello výsledky seznamu toosee hello domovské stránce objektu pro vytváření dat.

    ![Domovská stránka objektu pro vytváření dat](media/data-factory-copy-activity-tutorial-using-visual-studio/data-factory-home-page.png)
5. Postupujte podle pokynů v [monitorování datových sad a kanálu](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) toomonitor hello kanálu a datových sad v tomto kurzu jste vytvořili. V současné době Visual Studio monitorování kanálů Data Factory nepodporuje. 

## <a name="summary"></a>Souhrn
V tomto kurzu jste vytvořili Azure data factory toocopy dat z Azure SQL database tooan objektů blob v Azure. Můžete použít Visual Studio toocreate hello objekt pro vytváření dat, propojených služeb, datových sad a kanálu. Zde jsou hello kroků, které jste provedli v tomto kurzu:  

1. Vytvořili jste **objekt pro vytváření dat** Azure.
2. Vytvořili jste **propojené služby**:
   1. **Azure Storage** propojené služby toolink účtu úložiště Azure, který obsahuje vstupní data.     
   2. **Azure SQL** propojené toolink služby Azure SQL database, který obsahuje hello výstupní data. 
3. Vytvořili jste **datové sady**, které popisují vstupní data a výstupní data pro kanály.
4. Vytvořili jste **kanál** s **aktivitou kopírování**, která má jako zdroj **BlobSource** a jako jímku **SqlSink**. 

toosee jak zjistit, toouse tootransform data aktivity HDInsight Hive pomocí clusteru Azure HDInsight [ kurz: vytvoření vaší první dat tootransform kanálu pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).

Dvě aktivity (spustit aktivitu po jiné) můžete zřetězené nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit. Podrobné informace najdete v tématu s popisem [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md). 

## <a name="view-all-data-factories-in-server-explorer"></a>Zobrazení všech datových továren v Průzkumníku serveru
Tato část popisuje, jak toouse hello Průzkumníka serveru v sadě Visual Studio tooview všechny datové továrny hello ve vašem předplatném Azure a vytvořit projekt sady Visual Studio založený na existujícím objektu pro vytváření dat. 

1. V **Visual Studio**, klikněte na tlačítko **zobrazení** hello nabídce a klikněte na tlačítko **Průzkumníka serveru**.
2. V okně hello Průzkumníka serveru rozbalte **Azure** a rozbalte **Data Factory**. Pokud se zobrazí **přihlášení tooVisual Studio**, zadejte hello **účet** přidružené k vašemu předplatnému Azure a klikněte na tlačítko **pokračovat**. Zadejte **heslo** a klikněte na **Přihlásit**. Visual Studio se pokusí tooget informace o všechny objekty pro vytváření dat Azure v rámci vašeho předplatného. Zobrazí hello stav této operace v hello **seznam úkolů objekt pro vytváření dat** okno.

    ![Průzkumník serveru](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)

## <a name="create-a-visual-studio-project-for-an-existing-data-factory"></a>Vytvoření projektu ve Visual Studiu pro existující datovou továrnu

- Klikněte pravým tlačítkem na objekt pro vytváření dat v Průzkumníku serveru a vyberte **tooNew Export Data Factory Project** toocreate projekt sady Visual Studio podle existující datovou továrnu.

    ![Export data factory tooa VS projekt](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

## <a name="update-data-factory-tools-for-visual-studio"></a>Aktualizace nástrojů služby Data Factory pro Visual Studio
tooupdate Azure Data Factory tools pro Visual Studio, hello následující kroky:

1. Klikněte na tlačítko **nástroje** na hello nabídku a vyberte **rozšíření a aktualizace**. 
2. Vyberte **aktualizace** v levém podokně hello a pak vyberte **Galerie sady Visual Studio**.
3. Vyberte možnost **Azure Data Factory tools for Visual Studio** (Nástroje služby Azure Data Factory pro Visual Studio) a klikněte na **Aktualizovat**. Pokud se tato položka nezobrazí, už máte nejnovější verzi nástroje hello hello. 

## <a name="use-configuration-files"></a>Použití konfiguračních souborů
Konfigurační soubory v sadě Visual Studio tooconfigure vlastnosti můžete použít pro propojených služeb/tabulek/kanálů pro různá prostředí.

Vezměte v úvahu následující definici JSON pro služby Azure Storage, propojené hello. toospecify **connectionString** s různými hodnotami pro accountname a accountkey pro různá prostředí (Dev/testovací/produkční) toowhich hello nasazujete entity služby Data Factory. Provedete to tak, že pro každé prostředí použijete samostatný konfigurační soubor.

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

### <a name="add-a-configuration-file"></a>Přidání konfiguračního souboru
Přidejte konfigurační soubor pro každé prostředí provedením následujících kroků hello:   

1. Klikněte pravým tlačítkem na projekt Data Factory hello v řešení sady Visual Studio, přejděte příliš**přidat**a klikněte na tlačítko **novou položku**.
2. Vyberte **konfigurace** hello seznamu nainstalovaných šablon vlevo hello vyberte **konfigurační soubor**, zadejte **název** pro konfiguraci hello souboru a klikněte na tlačítko **Přidat**.

    ![Přidání konfiguračního souboru](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. Přidejte konfigurační parametry a jejich hodnoty v hello následující formát:

    ```json
    {
        "$schema": "http://datafactories.schema.management.azure.com/vsschemas/V1/Microsoft.DataFactory.Config.json",
        "AzureStorageLinkedService1": [
            {
                "name": "$.properties.typeProperties.connectionString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        ],
        "AzureSqlLinkedService1": [
            {
                "name": "$.properties.typeProperties.connectionString",
                "value":  "Server=tcp:spsqlserver.database.windows.net,1433;Database=spsqldb;User ID=spelluru;Password=Sowmya123;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        ]
    }
    ```

    Tento příklad umožňuje nakonfigurovat vlastnost connectionString propojené služby Azure Storage a propojené služby Azure SQL. Všimněte si, že je hello syntaxe pro určení názvu [JsonPath](http://goessner.net/articles/JsonPath/).   

    Pokud má kód JSON vlastnost, která má pole hodnot, jak je znázorněno v následujícím kódu hello:  

    ```json
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
    ```

    Nakonfigurujte vlastnosti, jak je znázorněno v následující soubor konfigurace (použijte indexování od nuly) hello:

    ```json
    {
        "name": "$.properties.structure[0].name",
        "value": "FirstName"
    }
    {
        "name": "$.properties.structure[0].type",
        "value": "String"
    }
    {
        "name": "$.properties.structure[1].name",
        "value": "LastName"
    }
    {
        "name": "$.properties.structure[1].type",
        "value": "String"
    }
    ```

### <a name="property-names-with-spaces"></a>Názvy vlastností s mezerami
Pokud název vlastnosti obsahuje mezery, použijte hranaté závorky, jak ukazuje následující příklad (název databázového serveru) hello:

```json
 {
     "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
     "value": "MyAsqlServer.database.windows.net"
 }
```

### <a name="deploy-solution-using-a-configuration"></a>Nasazení řešení pomocí konfigurace
Když v sadě VS publikujete entity služby Azure Data Factory, abyste mohli zadat konfiguraci hello chcete toouse pro danou operaci publikování.

toopublish entity v projektu Azure Data Factory pomocí konfiguračního souboru:   

1. Klikněte pravým tlačítkem na projekt Data Factory a klikněte na tlačítko **publikovat** toosee hello **publikování položky** dialogové okno.
2. Vyberte existující datovou továrnu nebo zadejte hodnoty pro vytvoření služby data factory na hello **objekt pro vytváření dat konfigurace** a klikněte na tlačítko **Další**.   
3. Na hello **publikování položky** stránky: zobrazí rozevírací seznam s dostupnými konfiguracemi pro hello **výběr konfigurace nasazení** pole.

    ![Výběr konfiguračního souboru](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)
4. Vyberte hello **konfigurační soubor** , že chcete vytvořit toouse a klikněte na tlačítko **Další**.
5. Zkontrolujte, jestli hello název souboru JSON v hello **Souhrn** a klikněte na tlačítko **Další**.
6. Klikněte na tlačítko **Dokončit** po dokončení operace nasazení hello.

Když nasadíte, hello hodnoty z konfiguračního souboru hello jsou použité tooset hodnoty vlastností v souborech JSON hello před hello entity jsou nasazené tooAzure služba Data Factory.   

## <a name="use-azure-key-vault"></a>Použití Azure Key Vault
Není vhodné a často proti zabezpečení zásady toocommit citlivých dat jako úložiště kódu toohello řetězce připojení. V tématu [zabezpečeného publikování ADF](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) ukázku na Githubu toolearn o ukládání citlivých informací v Azure Key Vault a jeho použití při publikování entit služby Data Factory. Hello zabezpečeného publikování rozšíření pro Visual Studio umožňuje toobe hello tajné klíče uložené v Key Vault a pouze odkazy toothem jsou určené v propojené služby nebo konfigurace nasazení. Tyto odkazy se překládají při publikování tooAzure entity služby Data Factory. Tyto soubory pak lze potvrdit toosource úložiště bez vystavení všech tajných klíčů.


## <a name="next-steps"></a>Další kroky
V tomto kurzu jste v operaci kopírování použili úložiště objektů blob jako zdrojové úložiště dat a databázi Azure SQL jako cílové úložiště dat. Hello následující tabulka obsahuje seznam úložiště dat, které jsou podporované jako zdroje a cíle pomocí aktivity kopírování hello: 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

toolearn o tom, jak toocopy data z datové úložiště, klikněte na odkaz hello hello úložiště dat v tabulce hello.
