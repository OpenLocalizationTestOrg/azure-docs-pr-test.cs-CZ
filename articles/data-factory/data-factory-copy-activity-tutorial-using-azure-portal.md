---
title: "Kurz: Vytvoření toocopy datový kanál Azure Data Factory (portál Azure) | Microsoft Docs"
description: "V tomto kurzu pomocí portálu Azure toocreate kanál služby Azure Data Factory s aktivitou kopírování toocopy dat z Azure SQL database tooan úložiště objektů blob v Azure."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: d9317652-0170-4fd3-b9b2-37711272162b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: fadd840fe9a15cd8831cdb25dccbd10ac42fa161
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-use-azure-portal-toocreate-a-data-factory-pipeline-toocopy-data"></a>Kurz: Použití portálu Azure toocreate toocopy datový kanál služby Data Factory 
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

V tomto článku se dozvíte, jak toouse [portál Azure](https://portal.azure.com) toocreate objekt pro vytváření dat s kanál, který kopíruje data z databáze Azure SQL tooan úložiště objektů blob v Azure. Pokud jste tooAzure nový objekt pro vytváření dat, pročtěte hello [Úvod tooAzure Data Factory](data-factory-introduction.md) článek před provedením tohoto kurzu.   

V tomto kurzu vytvoříte kanál s jednou aktivitou: aktivita kopírování. Aktivita kopírování Hello zkopíruje data z úložiště dat podporovaných podřízený tooa podporované datové úložiště. Seznam úložišť dat podporovaných jako zdroje a jímky najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Hello aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelné způsobem. Další informace o hello aktivitě kopírování najdete v tématu [aktivity přesunu dat](data-factory-data-movement-activities.md).

Kanál může obsahovat víc než jednu aktivitu. A dvě aktivity (spustit aktivitu po jiné) můžete řetězu nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit. Další informace naleznete, když přejdete na [více aktivit v kanálu](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline). 

> [!NOTE] 
> Hello datovém kanálu v tomto kurzu kopíruje data ze zdroje dat úložiště tooa cílového úložiště dat. Kurz týkající se jak tootransform dat pomocí Azure Data Factory najdete v části [kurz: sestavení dat tootransform kanálu pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).

## <a name="prerequisites"></a>Požadavky
Dokončení požadavky uvedené v hello [požadavky kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) článek před provedením tohoto kurzu.

## <a name="steps"></a>Kroky
Zde jsou hello kroky, které můžete provádět v rámci tohoto kurzu:

1. Vytvořte **datovou továrnu** Azure. V tomto kroku vytvoříte datovou továrnu s názvem ADFTutorialDataFactory. 
2. Vytvoření **propojené služby** v datové továrně hello. V tomto kroku vytvoříte dvě propojené služby typu: Azure Storage a Azure SQL Database. 
    
    Hello AzureStorageLinkedService propojuje účet úložiště Azure toohello datovou továrnu. Vytvořit kontejner a účet úložiště dat toothis odesílané jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

    AzureSqlLinkedService propojuje službu Azure SQL database toohello datovou továrnu. Hello data, která se zkopírují z úložiště objektů blob hello jsou uložena v této databázi. V této databázi jste v rámci [požadavků](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) vytvořili tabulku SQL.   
3. Vytvoření vstupní a výstupní **datové sady** v datové továrně hello.  
    
    Hello propojená služba úložiště Azure určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour účtu úložiště Azure. A určuje datovou sadu vstupního objektu blob hello hello kontejneru a složce hello, který obsahuje vstupní data hello.  

    Podobně hello služby Azure SQL Database propojené určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour Azure SQL database. A hello výstupní SQL tabulky datovou sadu Určuje, že se zkopíruje hello tabulky v hello databáze toowhich hello dat z úložiště objektů blob hello.
4. Vytvoření **kanálu** v datové továrně hello. V tomto kroku pomocí aktivity kopírování vytvoříte kanál.   
    
    Aktivita kopírování Hello zkopíruje data z objektu blob ve tabulku tooa úložiště objektů blob v Azure hello ve hello Azure SQL database. Můžete vytvořit aktivity kopírování v kanálu toocopy data z cílového všechny podporované zdrojové tooany podporována. Seznam podporovaných úložišť dat najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats). 
5. Monitorování kanálu hello. V tomto kroku budete **monitorování** hello řezy vstupní a výstupní datové sady pomocí portálu Azure. 

## <a name="create-data-factory"></a>Vytvoření objektu pro vytváření dat
> [!IMPORTANT]
> Dokončení [předpoklady pro kurz hello](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) Pokud jste tak již neučinili.   

Objekt pro vytváření dat může mít jeden nebo víc kanálů. Kanál může obsahovat jednu nebo víc aktivit. Například aktivitu kopírování dat toocopy z tooa zdroje cílového úložiště dat a toorun aktivitu HDInsight Hive tootransform skriptu Hive vstupní data tooproduct výstupní data. Začneme vytvořením objektu pro vytváření dat hello v tomto kroku.

1. Po přihlášení toohello [portál Azure](https://portal.azure.com/), klikněte na tlačítko **nový** v levé nabídce hello, klikněte na tlačítko **Data + analýzy**a klikněte na tlačítko **Data Factory**. 
   
   ![Nový -> Objekt pro vytváření dat](./media/data-factory-copy-activity-tutorial-using-azure-portal/NewDataFactoryMenu.png)    
2. V hello **nový objekt pro vytváření dat** okno:
   
   1. Zadejte **ADFTutorialDataFactory** pro hello **název**. 
      
         ![Okno Nový objekt pro vytváření dat](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-new-data-factory.png)
      
       musí být Hello název objektu pro vytváření dat Azure hello **globálně jedinečný**. Pokud se zobrazí následující chyba hello, změňte název hello hello objekt pro vytváření dat (třeba na Váš_název_adftutorialdatafactory) a zkuste to znovu. V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.
      
           Data factory name “ADFTutorialDataFactory” is not available  
      
       ![Název objektu pro vytváření dat není k dispozici](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-not-available.png)
   2. Vyberte vaši Azure **předplatné** ve kterém chcete objekt pro vytváření dat toocreate hello. 
   3. Pro hello **skupiny prostředků**, proveďte jednu z následujících kroků hello:
      
      - Vyberte **použít existující**a hello rozevíracím seznamu vyberte existující skupinu prostředků. 
      - Vyberte **vytvořit nový**a zadejte hello název skupiny prostředků.   
         
          Některé hello kroky v tomto kurzu vychází z předpokladu, že použijete název hello: **ADFTutorialResourceGroup** pro skupinu prostředků hello. toolearn o skupinách prostředků najdete v části [pomocí prostředků skupiny prostředků Azure toomanage](../azure-resource-manager/resource-group-overview.md).  
   4. Vyberte hello **umístění** hello data Factory. V rozevíracím seznamu hello jsou uvedeny pouze oblasti nepodporuje hello služba Data Factory.
   5. Vyberte **Pin toodashboard**.     
   6. Klikněte na možnost **Vytvořit**.
      
      > [!IMPORTANT]
      > instance služby Data Factory toocreate, musíte být členem skupiny hello [Přispěvatel objekt pro vytváření dat](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role na úrovni předplatného nebo prostředků skupiny hello.
      > 
      > Hello název objektu pro vytváření dat hello možné zaregistrovat jako název DNS v hello budoucí a proto pak bude veřejně viditelný.                
      > 
      > 
3. Na řídicím panelu hello, uvidíte následující hello dlaždici se stavem: **nasazení pro vytváření dat**. 

    ![nasazování dlaždice datové továrny](media/data-factory-copy-activity-tutorial-using-azure-portal/deploying-data-factory.png)
4. Po dokončení vytvoření hello uvidíte hello **Data Factory** jak je uvedeno v bitové kopii hello.
   
   ![Domovská stránka objektu pro vytváření dat](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-home-page.png)

## <a name="create-linked-services"></a>Vytvoření propojených služeb
Vytvoření propojených služeb v objektu pro vytváření toolink dat, data se ukládá a výpočetní služby toohello data factory. V tomto kurzu nebudete používat žádnou výpočetní službu jako je Azure HDInsight nebo Azure Data Lake Analytics. Budete používat dvě úložiště dat typu Azure Storage (zdroj) a Azure SQL Database (cíl). 

Vytvoříte tedy dvě propojené služby s názvem AzureStorageLinkedService a AzureSqlLinkedService typu: AzureStorage a AzureSqlDatabase.  

Hello AzureStorageLinkedService propojuje účet úložiště Azure toohello datovou továrnu. Tento účet úložiště se hello, jeden ve které jste vytvořili kontejner a objemu odeslaných dat hello jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

AzureSqlLinkedService propojuje službu Azure SQL database toohello datovou továrnu. Hello data, která se zkopírují z úložiště objektů blob hello jsou uložena v této databázi. Vytvořit hello tabulce emp v této databázi jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).  

### <a name="create-azure-storage-linked-service"></a>Vytvoření propojené služby Azure Storage
V tomto kroku propojíte datovou továrnu tooyour účet úložiště Azure. Zadejte název hello a klíč účtu úložiště Azure v této části.  

1. V hello **Data Factory** okně klikněte na tlačítko **vytvořit a nasadit** dlaždici.
   
   ![Dlaždice Vytvořit a nasadit](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-author-deploy-tile.png) 
2. Zobrazí hello **editoru služby Data Factory** jak ukazuje následující obrázek hello: 

    ![Data Factory Editor](./media/data-factory-copy-activity-tutorial-using-azure-portal/data-factory-editor.png)
3. V editoru hello, klikněte na tlačítko **nové datové úložiště** tlačítka na panelu nástrojů hello a vyberte **úložiště Azure** z rozevírací nabídky hello. Měli byste vidět hello šablona JSON pro vytvoření služby Azure storage, propojené v pravém podokně hello. 
   
    ![Tlačítko Nové datové úložiště v editoru](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-newdatastore-button.png)    
3. Nahraďte `<accountname>` a `<accountkey>` hodnotami hello účet účet a název klíče pro účet úložiště Azure. 
   
    ![Editor – Blob Storage – JSON](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-json.png)    
4. Klikněte na tlačítko **nasadit** na panelu nástrojů hello. Měli byste vidět hello nasazené **AzureStorageLinkedService** nyní ve stromu hello zobrazit. 
   
    ![Editor – Blob Storage – nasazení](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-deploy.png)

    Další informace o vlastnostech JSON v definici hello propojené služby najdete v tématu [konektor Azure Blob Storage](data-factory-azure-blob-connector.md#linked-service-properties) článku.

### <a name="create-a-linked-service-for-hello-azure-sql-database"></a>Vytvoření propojené služby pro hello Azure SQL Database
V tomto kroku propojíte datovou továrnu tooyour databáze Azure SQL. Zadejte název serveru Azure SQL hello, název databáze, uživatelské jméno a heslo uživatele v této části. 

1. V hello **editoru služby Data Factory**, klikněte na tlačítko **nové datové úložiště** tlačítka na panelu nástrojů hello a vyberte **Azure SQL Database** z rozevírací nabídky hello. Měli byste vidět hello šablona JSON pro vytvoření hello propojená služba Azure SQL v pravém podokně hello.
2. Hodnoty `<servername>`, `<databasename>`, `<username>@<servername>` a `<password>` nahraďte názvy serveru Azure SQL, databáze, uživatelského účtu a heslem. 
3. Klikněte na tlačítko **nasadit** hello toocreate panelu nástrojů a nasadit hello **AzureSqlLinkedService**.
4. Zkontrolujte, jestli **AzureSqlLinkedService** v zobrazení stromu hello pod **propojené služby**.  

    Další informace o těchto vlastnostech JSON najdete v článku [Konektor služby Azure SQL Database](data-factory-azure-sql-connector.md#linked-service-properties).

## <a name="create-datasets"></a>Vytvoření datových sad
V předchozím kroku hello jste vytvořili propojené služby toolink, účet úložiště Azure a Azure SQL database tooyour objekt pro vytváření dat. V tomto kroku definujete dvě datové sady s názvem InputDataset a OutputDataset, které představují vstupní a výstupní data, která jsou uložená v úložištích dat hello odkazuje AzureStorageLinkedService a AzureSqlLinkedService.

Hello propojená služba úložiště Azure určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour účtu úložiště Azure. A datovou sadu hello vstupního objektu blob (InputDataset) určuje hello kontejneru a složce hello, který obsahuje vstupní data hello.  

Podobně hello služby Azure SQL Database propojené určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour Azure SQL database. A hello výstupní datovou sadu tabulky SQL (OututDataset) určuje hello tabulky v hello databáze hello toowhich zkopírování dat z úložiště objektů blob hello. 

### <a name="create-input-dataset"></a>Vytvoření vstupní datové sady
V tomto kroku vytvoříte datové sady s názvem InputDataset, který ukazuje soubor blob tooa (emp.txt) v kořenové složce hello kontejner objektů blob (adftutorial) v hello Azure Storage reprezentované hello AzureStorageLinkedService propojené služby. Pokud nechcete zadat hodnotu pro název souboru hello (nebo ji přeskočit), jsou data ze všech objektů BLOB ve složce vstupní hello zkopírovaný toohello cílový. V tomto kurzu zadejte hodnotu pro název souboru hello. 

1. V hello **Editor** hello objekt pro vytváření dat, klikněte na tlačítko **... Další**, klikněte na tlačítko **nová datová sada**a klikněte na tlačítko **úložiště objektů Azure Blob** z rozevírací nabídky hello. 
   
    ![Nabídka Nová datová sada](./media/data-factory-copy-activity-tutorial-using-azure-portal/new-dataset-menu.png)
2. Nahraďte kód JSON v pravém podokně hello s hello následujícím fragmentu kódu JSON: 
   
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
          "folderPath": "adftutorial/",
          "fileName": "emp.txt",
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
3. Klikněte na tlačítko **nasadit** hello toocreate panelu nástrojů a nasadit hello **InputDataset** datovou sadu. Zkontrolujte, jestli hello **InputDataset** v zobrazení stromu hello.

### <a name="create-output-dataset"></a>Vytvoření výstupní datové sady
Hello služby Azure SQL Database propojené určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour Azure SQL database. Hello výstupní SQL tabulky datové sady (OututDataset) v tomto kroku vytvoříte Určuje, že se zkopíruje hello tabulky v hello databáze toowhich hello dat z úložiště objektů blob hello.

1. V hello **Editor** hello objekt pro vytváření dat, klikněte na tlačítko **... Další**, klikněte na tlačítko **nová datová sada**a klikněte na tlačítko **Azure SQL** z rozevírací nabídky hello. 
2. Nahraďte kód JSON v pravém podokně hello s hello následujícím fragmentu kódu JSON:

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
3. Klikněte na tlačítko **nasadit** hello toocreate panelu nástrojů a nasadit hello **OutputDataset** datovou sadu. Zkontrolujte, jestli hello **OutputDataset** v zobrazení stromu hello pod **datové sady**. 

## <a name="create-pipeline"></a>Vytvoření kanálu
V tomto kroku vytvoříte kanál s **aktivitou kopírování**, která používá **InputDataset** jako vstup a **OutputDataset** jako výstup.

Výstupní datové sady v současné době je, jaké jednotky hello plán. V tomto kurzu výstupní datové sady je nakonfigurované tooproduce řez jednou za hodinu. Hello kanálu má čas spuštění a čas ukončení, které jsou od sebe, jeden den, což je 24 hodin. Proto 24 řezů výstupní datové sady vyprodukované hello kanálu. 

1. V hello **Editor** hello objekt pro vytváření dat, klikněte na tlačítko **... Další** a poté na **Nový kanál**. Alternativně můžete můžete kliknout pravým tlačítkem **kanály** v zobrazení stromu hello a klikněte na **nový kanál**.
2. Nahraďte kód JSON v pravém podokně hello s hello následujícím fragmentu kódu JSON: 

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
    - Počáteční a koncové hodnoty data a času musí být ve [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601). Například: 2016-10-14T16:32:41Z. Hello **end** čas je nepovinný, ale používáme v tomto kurzu. Pokud nezadáte hodnotu hello **end** vlastnost, vypočítá se jako "**start + 48 hodin**". bez omezení, zadejte toorun hello kanálu **9999-09-09** hello hodnotu hello **end** vlastnost.
     
    V předchozím příkladu hello jsou 24 datových řezů jako se vytvářejí každou hodinu.

    Popisy vlastností JSON použitých v definici kanálu najdete v článku [Vytvoření kanálů](data-factory-create-pipelines.md). Popisy vlastností JSON použitých v definici aktivity kopírování najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md). Popisy vlastností JSON podporovaných zdrojem BlobSource najdete v článku [Konektor Azure Blob](data-factory-azure-blob-connector.md). Popisy vlastností JSON podporovaných jímkou SqlSink najdete v článku [Konektor Azure SQL Database](data-factory-azure-sql-connector.md).
3. Klikněte na tlačítko **nasadit** hello toocreate panelu nástrojů a nasadit hello **ADFTutorialPipeline**. Zkontrolujte, jestli kanál hello v zobrazení stromu hello. 
4. Nyní zavřete hello **Editor** okno kliknutím **X**. Klikněte na tlačítko **X** znovu toosee hello **Data Factory** domovské stránce hello **ADFTutorialDataFactory**.

**Blahopřejeme!** Úspěšně jste vytvořili objekt pro vytváření dat Azure s kanálu toocopy dat z Azure SQL database tooan úložiště objektů blob v Azure. 


## <a name="monitor-pipeline"></a>Monitorování kanálu
V tomto kroku použijete hello Azure portálu toomonitor, co se děje v objektu pro vytváření dat Azure.    

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Monitorování kanálu pomocí aplikace pro monitorování a správu
Hello následující kroky ukazují, jak toomonitor kanálů v datové továrně pomocí hello monitorování a Správa aplikací: 

1. Klikněte na tlačítko **monitorování a správa** na domovskou stránku hello objektu pro vytváření dat dlaždici.
   
    ![Dlaždice Monitorování a správa](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-manage-tile.png) 
2. Na samostatné kartě by se měla zobrazit **aplikace pro monitorování a správu**. 

    > [!NOTE]
    > Pokud se zobrazí, že hello webový prohlížeč zasekl ve fázi "autorizace …", proveďte jednu z následujících hello: zrušte hello **blokovat soubory cookie třetích stran a data lokality** políčko (nebo) vytvořte výjimku pro **login.microsoftonline.com**a akci opakujte tooopen hello aplikace.

    ![Aplikace pro monitorování a správu](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-and-manage-app.png)
3. Změna hello **počáteční čas** a **čas ukončení** tooinclude spusťte (2017-05-11) a ukončení (2017-05-12) vašeho kanálu a klikněte na **použít**.     
3. Zobrazí hello **aktivity windows** spojené s každou hodinu mezi kanálu počáteční a koncové časy hello seznamu v prostředním podokně hello. 
4. toosee podrobnosti o aktivity okně vyberte hello okno aktivity v hello **aktivity Windows** seznamu. 
    ![Podrobnosti o okně aktivity](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-window-details.png)

    V Průzkumníku okno aktivity na hello správné, uvidíte, že hello řezy až toohello aktuální čas UTC (20:12:00) jsou zpracovány (v zelenou barvu). ještě nejsou zpracovány řezy Hello 8-9 PM, PM 9 10, 10 11 PM, 23: 00 - 12: 00.

    Hello **pokusy o** části v pravém podokně poskytuje informace o hello aktivity při spuštění pro hello datový řez hello. V případě, že došlo k chybě, poskytuje podrobnosti o chybě hello. Například pokud hello vstupní složky nebo kontejner neexistuje a hello řez zpracování selže, se zobrazí chybová zpráva s oznámením, že kontejner hello nebo složka neexistuje.

    ![Pokusy spuštění aktivit](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-run-attempts.png) 
4. Spusťte **SQL Server Management Studio**, připojit toohello Azure SQL Database a ověřte, že se v toohello vložily řádky hello **emp** tabulky v databázi hello.
    
    ![Výsledky dotazu SQL](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png)

Podrobnosti o použití této aplikace najdete v tématu [Monitorování a správa kanálů služby Azure Data Factory pomocí aplikace pro monitorování a správu](data-factory-monitor-manage-app.md).

### <a name="monitor-pipeline-using-diagram-view"></a>Monitorování kanálu pomocí Zobrazení diagramu
Můžete také monitorování datových kanálů pomocí zobrazení diagramu hello.  

1. V hello **Data Factory** okně klikněte na tlačítko **Diagram**.
   
    ![Okno objekt pro vytváření dat – dlaždice Diagram](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactoryblade-diagramtile.png)
2. Měli byste vidět hello diagram podobné toohello následující bitové kopie: 
   
    ![Zobrazení diagramu](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-diagram-blade.png)  
5. V zobrazení diagramu hello, klikněte dvakrát na **InputDataset** toosee řezy pro datovou sadu hello.  
   
    ![Datové sady s vybranou sadou InputDataset](./media/data-factory-copy-activity-tutorial-using-azure-portal/DataSetsWithInputDatasetFromBlobSelected.png)   
5. Klikněte na tlačítko **Další informace naleznete v** odkaz toosee všechny datové řezy hello. Uvidíte 24 hodinových řezů mezi časem spuštění a ukončení. 
   
    ![Všechny vstupní datové řezy](./media/data-factory-copy-activity-tutorial-using-azure-portal/all-input-slices.png)  
   
    Všimněte si, že všechny hello datové řezy až toohello aktuální čas UTC jsou **připraven** protože hello **emp.txt** soubor v kontejneru objektů blob hello existuje celou dobu hello: **adftutorial\input**. Hello řezy pro budoucí časy hello ve stavu Připraveno ještě nejsou. Potvrďte, že žádné řezy nezobrazují v hello **poslední době selhaly řezy** části dolnímu hello.
6. Hello diagram zobrazení (nebo) diagram hello levém toosee posuňte zobrazení najdete v oknech hello zavřít, dokud. Potom dvakrát klikněte na **OutputDataset**. 
8. Klikněte na tlačítko **Další informace naleznete v** odkaz na hello **tabulky** okně **OutputDataset** toosee všechny hello řezy.

    ![okno datové řezy](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslices-blade.png) 
9. Všimněte si, že všechny hello řezy až toohello aktuální čas UTC přesunout z **čekající na zpracování** Stav = > **v průběhu** ==> **připraven** stavu. Hello řezy z posledních hello (před aktuálním časem) se zpracovávají z nejnovější toooldest ve výchozím nastavení. Například pokud hello aktuální čas UTC 8:12:00, hello 19: 00 - 8 PM zpracování řezu se před hello 18: 00 – 19: 00 řez. Hello 20: 00 – 21: 00 zpracování řezu se na konci hello hello časový interval ve výchozím nastavení, které následuje po 21: 00.  
10. Klikněte na libovolný datový řez ze seznamu hello a měli byste vidět hello **datový řez** okno. Část dat spojená s oknem aktivity se nazývá řez. Řez může tvořit jeden soubor nebo více souborů.  
    
     ![okno datový řez](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslice-blade.png)
    
     Pokud není hello řez v hello **připraven** stavu, můžete zobrazit upstreamové datové řezy hello, které nejsou připravené a které blokují hello aktuálního řezu v hello **Upstreamové datové řezy, které nejsou připraveny** seznamu.
11. V hello **datový ŘEZ** okno, měli byste vidět všechna spuštění aktivit v seznamu hello dolnímu hello. Klikněte na tlačítko **aktivity při spuštění** toosee hello **podrobnosti o spuštění aktivit** okno. 
    
    ![Podrobnosti o spuštění aktivit](./media/data-factory-copy-activity-tutorial-using-azure-portal/ActivityRunDetails.png)

    V tomto okně uvidíte jak dlouho hello kopírování trvalo, jakou propustnost je, jak velký počet bajtů dat byly pro čtení a čas zahájení napsané, spuštění, spuštění koncový čas atd.  
12. Klikněte na tlačítko **X** tooclose všech oken hello dokud vrátit toohello domovského okna pro hello **ADFTutorialDataFactory**.
13. (volitelné) klikněte na hello **datové sady** dlaždici nebo **kanály** dlaždice tooget hello okna jste viděli hello předchozích kroků. 
14. Spusťte **SQL Server Management Studio**, připojit toohello Azure SQL Database a ověřte, že se v toohello vložily řádky hello **emp** tabulky v databázi hello.
    
    ![Výsledky dotazu SQL](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png)


## <a name="summary"></a>Souhrn
V tomto kurzu jste vytvořili Azure data factory toocopy dat z Azure SQL database tooan objektů blob v Azure. Můžete použít hello Azure portálu toocreate hello objekt pro vytváření dat, propojených služeb, datových sad a kanálu. Zde jsou hello kroků, které jste provedli v tomto kurzu:  

1. Vytvořili jste **objekt pro vytváření dat** Azure.
2. Vytvořili jste **propojené služby**:
   1. **Azure Storage** propojené služby toolink účtu úložiště Azure, který obsahuje vstupní data.     
   2. **Azure SQL** propojené toolink služby Azure SQL database, který obsahuje hello výstupní data. 
3. Vytvořili jste **datové sady**, které popisují vstupní data a výstupní data pro kanály.
4. Vytvořili jste **kanál** s **aktivitou kopírování**, která má jako zdroj **BlobSource** a jako jímku **SqlSink**.  

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste v operaci kopírování použili úložiště objektů blob jako zdrojové úložiště dat a databázi Azure SQL jako cílové úložiště dat. Hello následující tabulka obsahuje seznam úložiště dat, které jsou podporované jako zdroje a cíle pomocí aktivity kopírování hello: 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

toolearn o tom, jak toocopy data z datové úložiště, klikněte na odkaz hello hello úložiště dat v tabulce hello.
