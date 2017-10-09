---
title: "data aaaMove – Brána pro správu dat | Microsoft Docs"
description: "Nastavení dat brány toomove dat mezi místními a hello cloudu. Použijte Brána pro správu dat v Azure Data Factory toomove data."
keywords: "Brána dat, integraci dat. přesun dat, přihlašovací údaje brány"
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: 7bf6d8fd-04b5-499d-bd19-eff217aa4a9c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: abnarain
ms.openlocfilehash: 314341c142d5260c785b7e82081774f044450e81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-between-on-premises-sources-and-hello-cloud-with-data-management-gateway"></a>Přesun dat mezi místní zdroje a hello cloudu s Brána pro správu dat
Tento článek obsahuje přehled dat integrace mezi místním úložištím dat a cloudové úložiště dat pomocí služby Data Factory. Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek a další data factory základní koncepty články: [datové sady](data-factory-create-datasets.md) a [kanály](data-factory-create-pipelines.md).

## <a name="data-management-gateway"></a>Brána správy dat
Brána pro správu dat je nutné nainstalovat na vaše místní počítač tooenable přesun dat z úložiště služby místní data. Hello gateway se dá nainstalovat na stejný počítač jako úložiště dat hello nebo na jiném počítači, dokud hello gateway bude moct připojit úložiště dat toohello hello.

> [!IMPORTANT]
> V tématu [Brána pro správu dat](data-factory-data-management-gateway.md) podrobnosti o Brána pro správu dat najdete v článku. 

Hello následující postup vám ukáže, jak toocreate objekt pro vytváření dat s kanál, který přesouvá data z místního **systému SQL Server** databáze tooan úložiště objektů blob Azure. Jako součást hello návod nainstalujte a nakonfigurujte hello Brána pro správu dat na počítači.

## <a name="walkthrough-copy-on-premises-data-toocloud"></a>Návod: zkopírování toocloud místní data
V tomto návodu hello následující kroky: 

1. Vytvoření datové továrny
2. Brána pro správu dat vytvořte. 
3. Vytvoření propojených služeb pro úložiště dat zdroj a jímka.
4. Vytvoření datových sad toorepresent vstupní a výstupní data.
5. Vytvoření kanálu s kopírovat data hello toomove aktivity.

## <a name="prerequisites-for-hello-tutorial"></a>Předpoklady pro kurz hello
Než začnete Tento názorný postup, musíte mít hello následující požadavky:

* **Předplatné Azure**.  Pokud nemáte předplatné, můžete si během několika minut bezplatně vytvořit zkušební účet. V tématu hello [bezplatné zkušební verze](http://azure.microsoft.com/pricing/free-trial/) článku.
* **Účet úložiště Azure**. Použít úložiště objektů blob hello jako **cílové/jímka** úložiště dat v tomto kurzu. Pokud nemáte účet úložiště Azure, najdete v části hello [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) najdete v článku kroky toocreate jeden.
* **SQL Server**. V tomto kurzu použijete místní databázi SQL Serveru jako **zdrojové** úložiště dat. 

## <a name="create-data-factory"></a>Vytvoření objektu pro vytváření dat
V tomto kroku použijete hello Azure portálu toocreate instanci objektu pro vytváření dat Azure s názvem **ADFTutorialOnPremDF**.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko **+ nový**, klikněte na tlačítko **Intelligence + analýzy**a klikněte na tlačítko **Data Factory**.

   ![Nový -> Objekt pro vytváření dat](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)  
3. V hello **nový objekt pro vytváření dat** zadejte **ADFTutorialOnPremDF** pro hello název.

    ![Přidat tooStartboard](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

   > [!IMPORTANT]
   > Hello název objektu pro vytváření dat Azure hello musí být globálně jedinečný. Pokud se zobrazí chyba hello: **název objektu pro vytváření dat "ADFTutorialOnPremDF" není k dispozici**, změňte hello název objektu pro vytváření dat hello (například yournameADFTutorialOnPremDF) a zkuste to znovu. Tento název používejte místo ADFTutorialOnPremDF při provádění zbývající kroky v tomto kurzu.
   >
   > Hello název objektu pro vytváření dat hello může být registrován jako **DNS** název v budoucnosti hello a proto pak bude veřejně viditelný.
   >
   >
4. Vyberte hello **předplatného Azure** místo hello data factory toobe vytvořili.
5. Vyberte existující **skupinu prostředků** nebo vytvořte novou. Hello kurzu vytvořte skupinu prostředků s názvem: **ADFTutorialResourceGroup**.
6. Klikněte na tlačítko **vytvořit** na hello **nový objekt pro vytváření dat** stránky.

   > [!IMPORTANT]
   > instance služby Data Factory toocreate, musíte být členem skupiny hello [Přispěvatel objekt pro vytváření dat](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role na úrovni předplatného nebo prostředků skupiny hello.
   >
   >
7. Po dokončení vytvoření se zobrazí hello **Data Factory** stránky, jak ukazuje následující obrázek hello:

   ![Data Factory domovské stránky](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

## <a name="create-gateway"></a>Vytvoření brány
1. V hello **Data Factory** klikněte na tlačítko **autora a nasazení** dlaždice toolaunch hello **Editor** hello data Factory.

    ![Dlaždice Vytvořit a nasadit](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png)
2. V editoru služby Data Factory hello, klikněte na **... Další** hello panelu nástrojů a pak klikněte na tlačítko **novou bránu dat**. Alternativně můžete můžete kliknout pravým tlačítkem **brány Data Gateways** v hello stromové zobrazení a klikněte na tlačítko **novou bránu dat**.

   ![Nová brána dat na panelu nástrojů](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
3. V hello **vytvořit** zadejte **adftutorialgateway** pro hello **název**a klikněte na tlačítko **OK**.     

    ![Vytvoření brány stránky](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)

    > [!NOTE]
    > V tomto návodu vytvoříte logické brány hello jenom jeden uzel (místní počítač Windows). Brána pro správu dat můžete škálovat tím, že přidružíte více místní počítače s bránou hello. Je možné škálovat až zvýšením počtu úloh přesun dat, které můžou běžet souběžně na uzlu. Tato funkce je také k dispozici pro logické brány se jeden uzel. V tématu [škálování Brána pro správu dat v Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) článku.  
4. V hello **konfigurace** klikněte na tlačítko **nainstalovat přímo na tomto počítači**. Tato akce stáhne hello instalační balíček brány hello, nainstaluje, konfiguruje a zaregistruje hello bránu v počítači hello.  

   > [!NOTE]
   > Použijte Internet Explorer nebo Microsoft ClickOnce kompatibilní webového prohlížeče.
   >
   > Pokud používáte Chrome, přejděte toohello [internetového obchodu Chrome](https://chrome.google.com/webstore/), hledání with – klíčové slovo "ClickOnce", vyberte jedno z rozšíření hello ClickOnce a nainstalujte ji.
   >
   > Hello stejné pro Firefox (instalace doplňku). Klikněte na tlačítko **otevřít nabídku** tlačítka na panelu nástrojů hello (**tři vodorovné čáry** v pravém horním rohu hello), klikněte na tlačítko **doplňky**, hledání with – klíčové slovo "ClickOnce", vyberte jednu z hello ClickOnce rozšíření a nainstalujte ji.    
   >
   >

    ![Brána - konfigurace stránky](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

    Tímto způsobem je nejjednodušší způsob (jedním kliknutím) toodownload hello, nainstalovat, nakonfigurovat a zaregistrovat bránu hello v jednom kroku jeden. Můžete zobrazit hello **Správce konfigurace brány pro správu dat společnosti Microsoft** je aplikace nainstalována na váš počítač. Můžete také vyhledat hello spustitelné **ConfigManager.exe** ve složce hello: **C:\Program Files\Microsoft Data správy Gateway\2.0\Shared**.

    Můžete také stáhnout a nainstalovat bránu ručně pomocí hello odkazů na této stránce a zaregistrovat ji pomocí klíče hello ukazuje hello **nový klíč** textové pole.

    V tématu [Brána pro správu dat](data-factory-data-management-gateway.md) článek pro všechny hello podrobnosti o hello brány.

   > [!NOTE]
   > Musíte mít oprávnění správce na místním počítači tooinstall hello a nakonfigurovat hello Brána pro správu dat úspěšně. Můžete přidat další uživatele toohello **Data Management Gateway Users** místní skupiny Windows. Hello členy této skupiny můžete použít hello Správce konfigurace brány pro správu dat nástroj tooconfigure hello brány.
   >
   >
5. Počkejte několik minut, nebo počkejte, až se zobrazí následující zprávu oznámení hello:

    ![Úspěšná instalace brány](./media/data-factory-move-data-between-onprem-and-cloud/gateway-install-success.png)
6. Spusťte **Správce konfigurace brány pro správu dat** aplikace ve vašem počítači. V hello **vyhledávání** zadejte **Brána pro správu dat** tooaccess tento nástroj. Můžete také vyhledat hello spustitelné **ConfigManager.exe** ve složce hello: **C:\Program Files\Microsoft Data správy Gateway\2.0\Shared**

    ![Správce konfigurace brány](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)
7. Zkontrolujte, jestli `adftutorialgateway is connected toohello cloud service` zprávy. Hello stavovém řádku zobrazuje dolní hello **připojené toohello Cloudová služba** spolu s **zelená značka zaškrtnutí**.

    Na hello **Domů** kartě, můžete provést také hello následující operace:

   * **Zaregistrovat** bránu pomocí klíče z portálu Azure pomocí tlačítko Registrovat hello hello.
   * **Zastavit** hello Data Management Gateway hostitele Service spuštěná na počítači brány.
   * **Naplánovat aktualizace** toobe nainstalovaný v určitém čase hello dne.
   * Zobrazení při hello brány **poslední aktualizace**.
   * Zadejte čas, kdy se dá nainstalovat bránu toohello aktualizace.
8. Přepínač toohello **nastavení** kartě hello certifikát uvedený v hello **certifikát** část se přihlašovací údaje použité tooencrypt/dešifrování pro hello místní datové úložiště, které zadáte na portál hello. (volitelné) Klikněte na tlačítko **změnu** toouse svůj vlastní certifikát místo. Ve výchozím nastavení používá brána hello hello certifikát, který je automaticky vygenerována třídou hello služba Data Factory.

    ![Konfigurace certifikátu brány](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)

    Můžete také provést následující akce v hello hello **nastavení** karty:

   * Zobrazení nebo export hello certifikátu používanému službou hello brány.
   * Změnit koncový bod HTTPS hello používá hello brány.    
   * Nastavte toobe proxy serveru HTTP používaný hello brány.     
9. (volitelné) Přepínač toohello **diagnostiky** kartě, zkontrolujte hello **zapnout podrobné protokolování** možnost, pokud chcete tooenable podrobné protokolování, které můžete použít tootroubleshoot všechny problémy s bránou hello. Hello informace o protokolování naleznete v **Prohlížeč událostí** pod **protokoly aplikací a služeb** -> **Brána pro správu dat** uzlu.

    ![Karta Diagnostika](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

    Můžete také provést následující akce v hello hello **diagnostiky** karty:

   * Použití **Test připojení** části tooan místní zdroje dat pomocí hello brány.
   * Klikněte na tlačítko **zobrazit protokoly** toosee hello Brána pro správu dat protokolu v okně prohlížeče událostí.
   * Klikněte na tlačítko **odeslat protokoly** tooupload soubor zip s protokoly posledních sedmi dnech tooMicrosoft toofacilitate problémů v vaše.
10. Na hello **diagnostiky** na kartě hello **Test připojení** vyberte **SqlServer** hello hello datový typ úložiště, zadejte název hello hello databázového serveru, název Hello databázi, zadejte typ ověřování, zadejte uživatelské jméno a heslo a klikněte na tlačítko **Test** tootest jestli hello gateway bude moct připojit toohello databáze.
11. Přepínač toohello webového prohlížeče a v hello **portál Azure**, klikněte na tlačítko **OK** na hello **konfigurace** stránky a pak na hello **novou bránu dat** stránka.
12. Měli byste vidět **adftutorialgateway** pod **brány Data Gateways** hello stromové zobrazení na levé straně hello.  Pokud kliknete na tlačítko ji, měli byste vidět spojenou hello JSON.

## <a name="create-linked-services"></a>Vytvoření propojených služeb
V tomto kroku vytvoříte dvě propojené služby: **AzureStorageLinkedService** a **SqlServerLinkedService**. Hello **SqlServerLinkedService** odkazy místní databázi systému SQL Server a hello **AzureStorageLinkedService** propojená služba odkazuje služby data factory toohello úložiště objektů blob v Azure. Vytvoření kanálu dále v tomto návodu, který kopíruje data z úložiště objektů blob v Azure toohello databáze serveru SQL Server pro místní hello.

#### <a name="add-a-linked-service-tooan-on-premises-sql-server-database"></a>Přidejte databázi SQL serveru propojená služba tooan na místě
1. V hello **editoru služby Data Factory**, klikněte na tlačítko **nové datové úložiště** na panelu nástrojů hello a vyberte **systému SQL Server**.

   ![Nová SQL serveru propojená služba](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png)
2. V hello **JSON editor** na hello správné, hello následující kroky:

   1. Pro hello **gatewayName**, zadejte **adftutorialgateway**.    
   2. V hello **connectionString**, hello následující kroky:    

      1. Pro **servername**, zadejte název hello hello serveru, který je hostitelem databáze systému SQL Server hello.
      2. Pro **databasename**, zadejte název hello hello databáze.
      3. Klikněte na tlačítko **šifrovat** tlačítka na panelu nástrojů hello. Zobrazí aplikace hello správce přihlašovacích údajů.

         ![Pověření správce aplikace](./media/data-factory-move-data-between-onprem-and-cloud/credentials-manager-application.png)
      4. V hello **nastavení pověření** dialogové okno, zadejte typ ověřování, uživatelské jméno a heslo a klikněte na tlačítko **OK**. Pokud hello připojení úspěšné, hello šifrovat přihlašovací údaje uloží do hello JSON a zavře dialogové okno hello.
      5. Zavřete kartu hello prázdný prohlížeče, který spustit dialogové okno hello, pokud není uzavřený automaticky a získat zpátky toohello karta s hello portálu Azure.

         Na počítači gateway hello tyto přihlašovací údaje jsou **šifrované** pomocí certifikátu, který hello Data Factory služby vlastní. Pokud chcete toouse hello certifikát, který je přidružen hello Brána pro správu dat na místo, najdete v části [nastavit pověření bezpečně](#set-credentials-and-security).    
   3. Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello systému SQL Server propojené služby. Měli byste vidět hello propojené služby v zobrazení stromu hello.

      ![Služba SQL Server propojené v zobrazení stromu hello](./media/data-factory-move-data-between-onprem-and-cloud/sql-linked-service-in-tree-view.png)    

#### <a name="add-a-linked-service-for-an-azure-storage-account"></a>Přidat propojené služby pro účet úložiště Azure
1. V hello **editoru služby Data Factory**, klikněte na tlačítko **nové datové úložiště** na panelu příkazů hello a klikněte na tlačítko **úložiště Azure**.
2. Zadejte hello název účtu úložiště Azure pro hello **název účtu**.
3. Zadejte hello klíč pro účet úložiště Azure pro hello **klíč účtu**.
4. Klikněte na tlačítko **nasadit** toodeploy hello **AzureStorageLinkedService**.

## <a name="create-datasets"></a>Vytvoření datových sad
V tomto kroku vytvoření vstupní a výstupní datové sady, které představují vstupní a výstupní data pro operace kopírování hello (databáze systému SQL Server pro místní = > úložiště objektů blob v Azure). Před vytvořením datové sady, hello následující kroky (podrobný postup odpovídá seznamu hello):

* Vytvoří tabulku s názvem **emp** v hello jste přidali jako objekt propojené služby pro vytváření dat toohello databáze systému SQL Server a vložte několik ukázkových položky do tabulce hello.
* Vytvořte kontejner objektů blob s názvem **adftutorial** v hello Azure blob, které jste přidali jako objekt propojené služby pro vytváření dat toohello účet úložiště.

### <a name="prepare-on-premises-sql-server-for-hello-tutorial"></a>Příprava na místním serveru SQL pro kurz hello
1. V databázi hello jste určili pro hello místního SQL serveru propojená služba (**SqlServerLinkedService**), použijte následující SQL skriptu toocreate hello hello **emp** tabulky v databázi hello.

    ```SQL   
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
        CONSTRAINT PK_emp PRIMARY KEY (ID)
    )
    GO
    ```
2. Některé ukázkové vložte do tabulky hello:

    ```SQL
    INSERT INTO emp VALUES ('John', 'Doe')
    INSERT INTO emp VALUES ('Jane', 'Doe')
    ```

### <a name="create-input-dataset"></a>Vytvoření vstupní datové sady

1. V hello **editoru služby Data Factory**, klikněte na tlačítko **... Další**, klikněte na tlačítko **nová datová sada** na panelu příkazů text hello a klikněte na tlačítko **tabulky serveru SQL Server**.
2. Nahraďte hello JSON v pravém podokně hello hello následující text:

    ```JSON   
    {        
        "name": "EmpOnPremSQLTable",
        "properties": {
            "type": "SqlServerTable",
            "linkedServiceName": "SqlServerLinkedService",
            "typeProperties": {
                "tableName": "emp"
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
   Všimněte si hello následující body:

   * **typ** je nastaven příliš**SqlServerTable**.
   * **Název tabulky** je nastaven příliš**emp**.
   * **linkedServiceName** je nastaven příliš**SqlServerLinkedService** (jste vytvořili tuto propojenou službu dříve v tomto návodu.).
   * Pro vstupní datovou sadu, která není generované jiný kanál v Azure Data Factory, je potřeba nastavit **externí** příliš**true**. Ji označuje, že jsou vstupní data hello vytvořené externí toohello služby Azure Data Factory. Volitelně můžete zadat všechny zásady externí data pomocí hello **externalData** element v hello **zásad** části.    

   V tématu [přesun dat do nebo ze systému SQL Server](data-factory-sqlserver-connector.md) podrobnosti o vlastnostech formátu JSON.
3. Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello datovou sadu.  

### <a name="create-output-dataset"></a>Vytvoření výstupní datové sady

1. V hello **editoru služby Data Factory**, klikněte na tlačítko **nová datová sada** na panelu příkazů text hello a klikněte na tlačítko **úložiště objektů Azure Blob**.
2. Nahraďte hello JSON v pravém podokně hello hello následující text:

    ```JSON   
    {
        "name": "OutputBlobTable",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "adftutorial/outfromonpremdf",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
     }
    ```   
   Všimněte si hello následující body:

   * **typ** je nastaven příliš**AzureBlob**.
   * **linkedServiceName** je nastaven příliš**AzureStorageLinkedService** (tuto propojenou službu měl vytvořili v kroku 2).
   * **folderPath** je nastaven příliš**adftutorial/outfromonpremdf** kde outfromonpremdf je složka hello v kontejneru adftutorial hello. Vytvoření hello **adftutorial** kontejner, pokud ještě neexistuje.
   * Hello **dostupnosti** je nastaven příliš**každou hodinu** (**frekvence** nastavit příliš**hodinu** a **interval** nastavit také **1**).  Služba Data Factory Hello generuje řez výstupních dat každou hodinu v hello **emp** tabulky v hello Azure SQL Database.

   Pokud nezadáte **fileName** pro **výstupní tabulku**, hello generované soubory v hello **folderPath** se pojmenují podle hello formátu: Data.<Guid>. TXT (například:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

   tooset **folderPath** a **fileName** dynamicky podle hello **SliceStart** čas, použijte vlastnost partitionedBy hello. V následujícím příkladu hello folderPath používá rok, měsíc a den z hello SliceStart (čas zahájení zpracování řezu hello) a fileName používá hodinu z hello SliceStart. Například, pokud je řez vytvářen v 2014-10-20T08:00:00, hello folderName je nastavená toowikidatagateway/wikisampledataout/2014/10/20 a hello fileName je nastavená too08.csv.

    ```JSON
    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy":
    [

        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
    ],
    ```

    V tématu [přesun dat do/z Azure Blob Storage](data-factory-azure-blob-connector.md) podrobnosti o vlastnostech formátu JSON.
3. Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello datovou sadu. Zkontrolujte, jestli obou hello datové sady v zobrazení stromu hello.  

## <a name="create-pipeline"></a>Vytvoření kanálu
V tomto kroku vytvoříte **kanálu** s jedním **aktivity kopírování** používající **EmpOnPremSQLTable** jako vstup a **OutputBlobTable** jako výstup.

1. V editoru služby Data Factory, klikněte na tlačítko **... Další** a poté na **Nový kanál**.
2. Nahraďte hello JSON v pravém podokně hello hello následující text:    

    ```JSON   
     {
         "name": "ADFTutorialPipelineOnPrem",
         "properties": {
         "description": "This pipeline has one Copy activity that copies data from an on-prem SQL tooAzure blob",
         "activities": [
           {
             "name": "CopyFromSQLtoBlob",
             "description": "Copy data from on-prem SQL server tooblob",
             "type": "Copy",
             "inputs": [
               {
                 "name": "EmpOnPremSQLTable"
               }
             ],
             "outputs": [
               {
                 "name": "OutputBlobTable"
               }
             ],
             "typeProperties": {
               "source": {
                 "type": "SqlSource",
                 "sqlReaderQuery": "select * from emp"
               },
               "sink": {
                 "type": "BlobSink"
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
         "start": "2016-07-05T00:00:00Z",
         "end": "2016-07-06T00:00:00Z",
         "isPaused": false
       }
     }
    ```   
   > [!IMPORTANT]
   > Nahraďte hodnotu hello hello **spustit** vlastnost s hello aktuální den a **end** hodnotu s hello další den.
   >
   >

   Všimněte si hello následující body:

   * V části aktivity hello se pouze aktivity jejichž **typ** je nastaven příliš**kopie**.
   * **Vstup** hello aktivity je nastavený příliš**EmpOnPremSQLTable** a **výstup** hello aktivity je nastavený příliš**OutputBlobTable**.
   * V hello **rámci typeProperties** části **SqlSource** je zadán jako hello **typ zdroje** a ** BlobSink ** je zadán jako hello **jímky typ**.
   * Dotaz SQL `select * from emp` je zadán pro hello **sqlReaderQuery** vlastnost **SqlSource**.

   Počáteční a koncové hodnoty data a času musí být ve [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601). Například: 2014-10-14T16:32:41Z. Hello **end** čas je nepovinný, ale používáme v tomto kurzu.

   Pokud nezadáte hodnotu hello **end** vlastnost, vypočítá se jako "**start + 48 hodin**". bez omezení, zadejte toorun hello kanálu **9/9/9999** hello hodnotu hello **end** vlastnost.

   Definování hello doby trvání v které hello se zpracovávají datové řezy podle hello **dostupnosti** vlastnosti, které byly definovány pro každé datové sady Azure Data Factory.

   V příkladu hello je 24 datových řezů jak se vytvářejí každou hodinu.        
3. Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello datovou sadu (tabulka je obdélníková datová sada). Potvrďte, že hello kanálu se zobrazí v zobrazení stromu hello pod **kanály** uzlu.  
4. Nyní, klikněte na tlačítko **X** dvakrát tooclose hello stránky tooget back toohello **Data Factory** stránku hello **ADFTutorialOnPremDF**.

**Blahopřejeme!** Úspěšně jste vytvořili objekt pro vytváření dat Azure, propojené služby, počet datových sad a kanálu a naplánované hello kanálu.

#### <a name="view-hello-data-factory-in-a-diagram-view"></a>Zobrazení hello objekt pro vytváření dat v zobrazení diagramu.
1. V hello **portál Azure**, klikněte na tlačítko **Diagram** na domovské stránce hello hello dlaždici **ADFTutorialOnPremDF** služby data factory. :

    ![Diagram odkaz](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)
2. Měli byste vidět hello diagram podobné toohello následující bitové kopie:

    ![Zobrazení diagramu](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

    Přiblížení, oddálení, zvětšení too100 %, toofit přiblížení, automaticky pozice kanálů a datové sady a zobrazení informací o rodokmenu (zvýrazní nadřazené a podřízené položky vybraných položek).  Poklepáním na vlastnosti toosee objektu (vstupní a výstupní datové sady nebo kanál) pro ni.

## <a name="monitor-pipeline"></a>Monitorování kanálu
V tomto kroku použijete hello Azure portálu toomonitor, co se děje v objektu pro vytváření dat Azure. Můžete také použít PowerShell rutiny toomonitor datových sad a kanálů. Podrobnosti o monitorování najdete v tématu [monitorování a Správa kanálů](data-factory-monitor-manage-pipelines.md).

1. V diagramu hello, klikněte dvakrát na **EmpOnPremSQLTable**.  

    ![EmpOnPremSQLTable řezy](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)
2. Všimněte si, že všechny hello datové řezy až jsou v **připraven** stavu, protože doba trvání kanálu hello (čas spuštění doba tooend) je v minulosti hello. Je také protože vložení hello dat v databázi systému SQL Server hello a existuje celou dobu hello. Potvrďte, že žádné řezy nezobrazují v hello **problémové řezy** části dolnímu hello. Klikněte na všech řezech hello tooview **najdete v části více** dole hello hello seznam řezů.
3. Nyní v hello **datové sady** klikněte na tlačítko **OutputBlobTable**.

    ![OputputBlobTable řezy](./media/data-factory-move-data-between-onprem-and-cloud/OutputBlobTableSlicesBlade.png)
4. Klikněte na libovolný datový řez ze seznamu hello a měli byste vidět hello **datový řez** stránky. Uvidíte, že aktivita spuštěna hello řez. Zobrazí jenom jedna aktivita obvykle spustit.  

    ![Okno datový řez](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

    Pokud není hello řez v hello **připraven** stavu, můžete zobrazit upstreamové datové řezy hello, které nejsou připravené a které blokují hello aktuálního řezu v hello **Upstreamové datové řezy, které nejsou připraveny** seznamu.
5. Klikněte na tlačítko hello **aktivity při spuštění** hello seznamu v dolním toosee hello **podrobnosti o spuštění aktivit**.

   ![Stránka Podrobnosti o spuštění aktivity](./media/data-factory-move-data-between-onprem-and-cloud/ActivityRunDetailsBlade.png)

   Zobrazí se informace a, jako je propustnost, doba trvání a brány hello používá tootransfer hello data.
6. Klikněte na tlačítko **X** tooclose všechny dokud hello stránky
7. získat zpět toohello domovskou stránku pro hello **ADFTutorialOnPremDF**.
8. (volitelné) Klikněte na tlačítko **kanály**, klikněte na tlačítko **ADFTutorialOnPremDF**a procházení vstupní tabulky (**Potřebováno**) nebo výstupní datové sady (**vyprodukováno**).
9. Pomocí nástrojů, jako [Microsoft Storage Explorer](http://storageexplorer.com/) tooverify vytvořený objekt blob nebo soubor pro každou hodinu.

   ![Azure Storage Explorer](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## <a name="next-steps"></a>Další kroky
* V tématu [Brána pro správu dat](data-factory-data-management-gateway.md) článek pro všechny podrobnosti o hello Brána pro správu dat hello.
* V tématu [kopírování dat z Azure Blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) toolearn o jak toouse aktivity kopírování toomove data ze zdrojových dat úložiště tooa podřízený data.
