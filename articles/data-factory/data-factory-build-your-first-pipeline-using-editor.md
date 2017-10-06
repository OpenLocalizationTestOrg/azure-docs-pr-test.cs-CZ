---
title: "aaaBuild vaše první objekt pro vytváření dat (portál Azure) | Microsoft Docs"
description: "V tomto kurzu vytvoříte pomocí editoru služby Data Factory v hello Azure portal ukázkový kanál Azure Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: d5b14e9e-e358-45be-943c-5297435d402d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: fc80776001b181a59c04d80d2e05c20b107a63f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-portal"></a>Kurz: Sestavení prvního objektu pro vytváření dat Azure pomocí webu Azure Portal
> [!div class="op_single_selector"]
> * [Přehled a požadavky](data-factory-build-your-first-pipeline.md)
> * [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Šablona Resource Manageru](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)


V tomto článku se dozvíte, jak toouse [portál Azure](https://portal.azure.com/) toocreate první objekt pro vytváření dat Azure. kurz hello toodo pomocí jiných nástrojů nebo sady SDK, vyberte jednu z možností hello hello rozevíracího seznamu. 

Hello kanálu v tomto kurzu má jednu aktivitu: **aktivitu HDInsight Hive**. Tato aktivita spouští skript hive v clusteru Azure HDInsight, transformací vstupní data tooproduce výstupní data. Hello kanálu není naplánované toorun po měsíci mezi hello zadaný počáteční a koncový čas. 

> [!NOTE]
> Hello datovém kanálu v tomto kurzu transformuje vstupní data tooproduce výstupní data. Kurz týkající se jak toocopy dat pomocí Azure Data Factory najdete v části [kurz: kopírování dat z úložiště objektů Blob tooSQL databáze](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> Kanál může obsahovat víc než jednu aktivitu. A dvě aktivity (spustit aktivitu po jiné) můžete řetězu nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit. Další informace najdete v tématu [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).

## <a name="prerequisites"></a>Požadavky
1. Pročtěte [přehled kurzu](data-factory-build-your-first-pipeline.md) článek a dokončení hello **požadavek** kroky.
2. Tento článek neposkytuje koncepční přehled hello služby Azure Data Factory. Doporučujeme projít si [Úvod tooAzure Data Factory](data-factory-introduction.md) článku podrobnější přehled služby hello.  

## <a name="create-data-factory"></a>Vytvoření objektu pro vytváření dat
Objekt pro vytváření dat může mít jeden nebo víc kanálů. Kanál může obsahovat jednu nebo víc aktivit. Například aktivitu kopírování dat toocopy z tooa zdroje cílového úložiště dat a toorun aktivitu HDInsight Hive tootransform skriptu Hive vstupní data tooproduct výstupní data. Začneme vytvořením objektu pro vytváření dat hello v tomto kroku.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Klikněte na tlačítko **nový** v levé nabídce hello, klikněte na tlačítko **Data + analýzy**a klikněte na tlačítko **Data Factory**.

   ![Okno Vytvořit](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)
3. V hello **nový objekt pro vytváření dat** okno, zadejte **GetStartedDF** pro hello název.

   ![Okno Nový objekt pro vytváření dat](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

   > [!IMPORTANT]
   > musí být Hello název objektu pro vytváření dat Azure hello **globálně jedinečný**. Pokud se zobrazí chyba hello: **název objektu pro vytváření dat "GetStartedDF" není k dispozici**. Změňte hello název objektu pro vytváření dat hello (třeba na Váš_název_getstarteddf) a zkuste to znovu. V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.
   >
   > Hello název objektu pro vytváření dat hello může být registrován jako **DNS** název v budoucnosti hello a proto pak bude veřejně viditelný.
   >
   >
4. Vyberte hello **předplatného Azure** místo hello data factory toobe vytvořili.
5. Vyberte existující **skupinu prostředků** nebo vytvořte novou. Hello kurzu vytvořte skupinu prostředků s názvem: **ADFGetStartedRG**.
6. Vyberte hello **umístění** hello data Factory. V rozevíracím seznamu hello jsou uvedeny pouze oblasti nepodporuje hello služba Data Factory.
7. Vyberte **Pin toodashboard**. 
8. Klikněte na tlačítko **vytvořit** na hello **nový objekt pro vytváření dat** okno.

   > [!IMPORTANT]
   > instance služby Data Factory toocreate, musíte být členem skupiny hello [Přispěvatel objekt pro vytváření dat](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role na úrovni předplatného nebo prostředků skupiny hello.
   >
   >
7. Na řídicím panelu hello, uvidíte následující hello dlaždici se stavem: nasazení služby data factory.    

   ![Stav vytváření objektu pro vytváření dat](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
8. Blahopřejeme! Úspěšně jste vytvořili první objekt pro vytváření dat. Po úspěšném vytvoření objektu pro vytváření dat hello, se zobrazí stránka objektu pro vytváření dat hello, se zobrazí hello obsah objektu pro vytváření dat hello.     

    ![Okno Objekt pro vytváření dat](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

Před vytvořením kanálu v hello data factory, je nutné toocreate několik entit služby Data Factory nejdřív. Nejdřív vytvoříte propojené služby toolink dat úložiště nebo výpočtů tooyour data ukládat, definujete vstupní a výstupní datové sady toorepresent vstupní a výstupní data v propojených úložištích dat a poté vytvořit hello kanálu s aktivitou, která používá tyto datové sady.

## <a name="create-linked-services"></a>Vytvoření propojených služeb
V tomto kroku propojíte účtu úložiště Azure a služby na vyžádání Azure HDInsight clusteru tooyour data factory. blokování účtu Azure Storage Hello hello vstupní a výstupní data pro kanál hello v této ukázce. Hello propojené služby HDInsight je použité toorun skriptu Hive určeného v hello aktivitu hello kanál v této ukázce. Určit, co [úložiště dat](data-factory-data-movement-activities.md)/[výpočetní služby](data-factory-compute-linked-services.md) se používají ve vašem scénáři a propojit tyto služby toohello objekt pro vytváření dat vytvořením propojených služeb.  

### <a name="create-azure-storage-linked-service"></a>Vytvoření propojené služby Azure Storage
V tomto kroku propojíte datovou továrnu tooyour účet úložiště Azure. V tomto kurzu použijete hello stejný účet úložiště Azure toostore vstupní a výstupní data a hello HQL soubor skriptu.

1. Klikněte na tlačítko **vytvořit a nasadit** na hello **DATA FACTORY** okno pro **GetStartedDF**. Měli byste vidět hello editoru služby Data Factory.

   ![Dlaždice Vytvořit a nasadit](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2. Klikněte na **Nové datové úložiště** a zvolte **Účet Azure**.

   ![Nové úložiště dat – Azure Storage – nabídka](./media/data-factory-build-your-first-pipeline-using-editor/new-data-store-azure-storage-menu.png)
3. Měli byste vidět hello skript JSON pro vytvoření Azure Storage propojená služba v editoru hello.

   ![Propojená služba Azure Storage](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. Nahraďte **název účtu** hello název účtu úložiště Azure a **klíč účtu** s přístupovým klíčem hello hello účtu úložiště Azure. toolearn jak přístup tooget úložiště klíčů najdete v tématu hello informace o tom, jak tooview, kopírování a opětovné vytváření úložiště přístupové klíče v [spravovat váš účet úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-account).
5. Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello propojené služby.

    ![Tlačítko Nasadit](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

   Po hello propojené služby nasazení úspěšně hello **koncept-1** by měl zmizet okně a zobrazí **AzureStorageLinkedService** v hello stromovém zobrazení na levé straně hello.

    ![Propojená služba úložiště v nabídce](./media/data-factory-build-your-first-pipeline-using-editor/StorageLinkedServiceInTree.png)    

### <a name="create-azure-hdinsight-linked-service"></a>Vytvoření propojené služby Azure HDInsight
V tomto kroku propojíte služby na vyžádání HDInsight clusteru tooyour data factory. Hello HDInsight cluster je automaticky vytvoří za běhu a až dokončí zpracování a nečinnosti hello zadaného množství času se odstraní.

1. V hello **editoru služby Data Factory**, klikněte na tlačítko **... Další**, klikněte na **Nový výpočet** a vyberte **Cluster HDInsight na vyžádání**.

    ![Nový výpočet](./media/data-factory-build-your-first-pipeline-using-editor/new-compute-menu.png)
2. Zkopírujte a vložte následující fragment kódu toohello hello **koncept-1** okno. Hello fragmentu kódu JSON popisuje vlastnosti hello, které jsou používané toocreate hello HDInsight clusteru na vyžádání.

    ```JSON
    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }
    ```

    Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:

   | Vlastnost | Popis |
   |:--- |:--- |
   | ClusterSize |Určuje velikost hello hello clusteru HDInsight. |
   | TimeToLive | Nastavení této hello nečinnosti pro hello HDInsight cluster, než se odstraní. |
   | linkedServiceName | Určuje účet úložiště hello, který je použité toostore hello protokoly, které jsou generovány nástrojem HDInsight. |

    Všimněte si hello následující body:

   * Hello Data Factory vytvoří **systémem Linux** cluster HDInsight s hello JSON. Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).
   * Místo clusteru HDInsight na vyžádání můžete použít také **vlastní cluster HDInsight**. Podrobnosti najdete v tématu [Propojená služba HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).
   * Vytvoří Hello HDInsight cluster **výchozí kontejner** v úložišti objektů blob hello jste zadali v hello JSON (**linkedServiceName**). HDInsight neprovede odstranění tohoto kontejneru při odstranění clusteru hello. Toto chování je záměrné. Díky propojené službě HDInsight na vyžádání se cluster HDInsight vytvoří pokaždé, když je zpracován určitý řez, pokud neexistuje aktivní cluster (**timeToLive**). Hello clusteru je automaticky odstraněna po dokončení zpracování hello.

       Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů. Pokud pro řešení potíží s hello úloh je nepotřebujete, můžete toodelete je úložiště hello tooreduce náklady. vzor podle Hello názvy těchto kontejnerů: "adf**název_vašeho_objektu_pro_vytváření_dat**-**linkedservicename**- razítko_data_a_času". Pomocí nástrojů, jako [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete kontejnery ve vaší službě Azure blob storage.

     Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).
3. Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello propojené služby.

    ![Nasazení propojené služby HDInsight na vyžádání](./media/data-factory-build-your-first-pipeline-using-editor/ondemand-hdinsight-deploy.png)
4. Zkontrolujte, jestli obě **AzureStorageLinkedService** a **HDInsightOnDemandLinkedService** v hello stromovém zobrazení na levé straně hello.

    ![Zobrazení stromu s propojenými službami](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)

## <a name="create-datasets"></a>Vytvoření datových sad
V tomto kroku vytvoříte datové sady toorepresent hello vstupní a výstupní data pro zpracování Hive. Tyto datové sady odkazují toohello **AzureStorageLinkedService** jste vytvořili dříve v tomto kurzu. Hello propojené služby body tooan účet služby Azure Storage a datové sady zadejte kontejner, složku a název souboru v úložišti hello, který obsahuje vstupní a výstupní data.   

### <a name="create-input-dataset"></a>Vytvoření vstupní datové sady
1. V hello **editoru služby Data Factory**, klikněte na tlačítko **... Další** na panelu příkazů hello, klikněte na tlačítko **nová datová sada**a vyberte **úložiště objektů Azure Blob**.

    ![Nová datová sada](./media/data-factory-build-your-first-pipeline-using-editor/new-data-set.png)
2. Zkopírujte a vložte následující fragment kódu toohello koncept-1 okno hello. V hello fragmentu kódu JSON vytváříte datovou sadu s názvem **AzureBlobInput** , která představuje vstupní data pro aktivitu v kanálu hello. Kromě toho určíte, zda text hello vstupní data nachází v kontejneru objektů blob hello s názvem **adfgetstarted** a hello složku s názvem **inputdata**.

    ```JSON
    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "input.log",
                "folderPath": "adfgetstarted/inputdata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }
    ```
    Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:

   | Vlastnost | Popis |
   |:--- |:--- |
   | type |Hello vlastnost Typ nastavena příliš**AzureBlob** protože data se nachází v Azure blob storage. |
   | linkedServiceName |Odkazuje toohello **AzureStorageLinkedService** jste vytvořili dříve. |
   | folderPath | Určuje objekt blob hello **kontejneru** a hello **složky** , který obsahuje vstupní objekty BLOB. | 
   | fileName |Tato vlastnost je nepovinná. Pokud ji vynecháte, jsou vybrány všechny soubory hello z hello folderPath. V tomto kurzu pouze hello **input.log** zpracování. |
   | type |soubory protokolu Hello jsou v textovém formátu, takže používáme **TextFormat**. |
   | columnDelimiter |sloupce v souborech protokolů hello oddělené **čárku (`,`)** |
   | frequency/interval |frekvence nastavená příliš**měsíc** a interval je **1**, což znamená, že hello vstupní řezy jsou k dispozici každý měsíc. |
   | external | Tato vlastnost nastavena příliš**true** Pokud hello vstupní data nevygenerovala pomocí tohoto kanálu. V tomto kurzu není soubor input.log hello generována tohoto kanálu, abyste nám nastavit vlastnost tootrue hello. |

    Další informace o těchto vlastnostech JSON najdete v článku [konektor Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).
3. Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello nově vytvořený datovou sadu. Měli byste vidět hello datovou sadu v hello stromového zobrazení na levé straně hello.

### <a name="create-output-dataset"></a>Vytvoření výstupní datové sady
Teď vytvořte hello výstupní datovou sadu toorepresent hello výstupní data ve hello úložiště objektů Blob v Azure.

1. V hello **editoru služby Data Factory**, klikněte na tlačítko **... Další** na panelu příkazů hello, klikněte na tlačítko **nová datová sada**a vyberte **úložiště objektů Azure Blob**.  
2. Zkopírujte a vložte následující fragment kódu toohello koncept-1 okno hello. V hello fragmentu kódu JSON vytváříte datovou sadu s názvem **AzureBlobOutput**a určení hello struktura hello dat, která je vytvořena pomocí skriptu Hive hello. Kromě toho určíte, že hello výsledky ukládat do kontejneru objektů blob hello s názvem **adfgetstarted** a hello složku s názvem **partitioneddata**. Hello **dostupnosti** část určuje, že hello výstupní sada vytváří jednou měsíčně.

    ```JSON
    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
          "folderPath": "adfgetstarted/partitioneddata",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "availability": {
          "frequency": "Month",
          "interval": 1
        }
      }
    }
    ```
    V tématu **vytvoření vstupní datové sady hello** části popisy těchto vlastností. Sady nenajdete vlastnost external hello u výstupní datové jako hello datovou sadu vytváří služba Data Factory hello.
3. Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello nově vytvořený datovou sadu.
4. Ověřte, že tuto datovou sadu hello byla úspěšně vytvořena.

    ![Zobrazení stromu s propojenými službami](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-data-set.png)

## <a name="create-pipeline"></a>Vytvoření kanálu
V tomto kroku vytvoříte svůj první kanál s aktivitou **HDInsightHive**. Vstupní řez je dostupný jednou měsíčně (frekvence: Month, interval: 1), výstupní řez se vytváří jednou měsíčně a vlastnost scheduler hello aktivity hello nastavena také toomonthly. nastavení Hello hello výstupní datovou sadu a hello aktivitu Plánovač se musí shodovat. Výstupní datové sady v současné době je, jaké jednotky hello plánu, takže je nutné vytvořit datovou sadu výstupů i v případě, že hello aktivita nevytváří žádný výstup. Pokud aktivita hello neberou žádný vstup, můžete přeskočit vytvoření vstupní datové sady hello. na konci hello v této části jsou vysvětleny Hello vlastnosti používané ve hello následující JSON.

1. V hello **editoru služby Data Factory**, klikněte na tlačítko **třemi tečkami (...) Další příkazy** a pak klikněte na **nový kanál**.

    ![Tlačítko Nový kanál](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. Zkopírujte a vložte následující fragment kódu toohello koncept-1 okno hello.

   > [!IMPORTANT]
   > Nahraďte **storageaccountname** hello název účtu úložiště v hello JSON.
   >
   >

    ```JSON
    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService",
                        "defines": {
                            "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                            "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "policy": {
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "name": "RunSampleHiveActivity",
                    "linkedServiceName": "HDInsightOnDemandLinkedService"
                }
            ],
            "start": "2017-07-01T00:00:00Z",
            "end": "2017-07-02T00:00:00Z",
            "isPaused": false
        }
    }
    ```

    V hello fragmentu kódu JSON vytváříte kanál sestávající z jediné aktivity, který používá Hive tooprocess Data v clusteru HDInsight.

    soubor skriptu Hive Hello **partitionweblogs.hql**, je uložený v účtu úložiště Azure hello (určeného hello scriptLinkedService, nazývá **AzureStorageLinkedService**) a v  **skript** složky v kontejneru hello **adfgetstarted**.

    Hello **definuje** část se nastavení používané toospecify hello běhového prostředí, které se předávají toohello skriptu hive jako konfigurační hodnoty Hive (např. ${hiveconf: inputtable}, ${určuje}).

    Hello **spustit** a **end** vlastnosti kanálu hello určuje hello aktivní období kanálu hello.

    V kódu JSON aktivity hello určujete, že hello skript Hive běžet ve výpočetní hello určeného hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.

   > [!NOTE]
   > V části "JSON kanálu" v [kanály a aktivity v Azure Data Factory](data-factory-create-pipelines.md) podrobnosti o vlastnostech JSON použitých v příkladu hello.
   >
   >
3. Potvrďte hello následující:

   1. **Input.log** soubor existuje v hello **inputdata** složky hello **adfgetstarted** kontejneru v hello úložiště objektů blob v Azure
   2. **partitionweblogs.hql** soubor existuje v hello **skriptu** složky hello **adfgetstarted** kontejneru v hello úložiště objektů blob Azure. Předpokladem pro dokončení hello kroky hello [přehled kurzu](data-factory-build-your-first-pipeline.md) Pokud nevidíte tyto soubory.
   3. Potvrďte, že nahrazen **storageaccountname** hello název účtu úložiště v hello kanálu JSON.
4. Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello kanálu. Od hello **spustit** a **end** jsou nastavené v posledních hello a **isPaused** je sada toofalse, hello kanál (aktivita v kanálu hello) spustí hned po nasazení.
5. Zkontrolujte, jestli kanál hello v zobrazení stromu hello.

    ![Zobrazení stromu s kanálem](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
6. Úspěšně jste vytvořili první kanál, blahopřejeme!

## <a name="monitor-pipeline"></a>Monitorování kanálu
### <a name="monitor-pipeline-using-diagram-view"></a>Monitorování kanálu pomocí Zobrazení diagramu
1. Klikněte na tlačítko **X** okna editoru služby Data Factory tooclose toonavigate zpět toohello okno objekt pro vytváření dat a klikněte na tlačítko **Diagram**.

    ![Dlaždice Diagram](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
2. V zobrazení diagramu hello zobrazí se přehled hello kanálů a datové sady použité v tomto kurzu.

    ![Zobrazení diagramu](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png)
3. tooview všechny aktivity v kanálu hello, klikněte pravým tlačítkem na kanál v hello diagram a klikněte na Otevřít kanál.

    ![Nabídka Otevřít kanál](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-menu.png)
4. Zkontrolujte, jestli aktivita HDInsightHive hello v kanálu hello.

    ![Zobrazení Otevřít kanál](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-view.png)

    toonavigate zpět toohello předchozího zobrazení, klikněte na tlačítko **objekt pro vytváření dat** v nabídce hello s popisem cesty v horní části hello.
5. V hello **zobrazení diagramu**, dvakrát klikněte na datovou sadu hello **AzureBlobInput**. Potvrďte, že hello řez je v **připraven** stavu. Může trvat několik minut, než se řez tooshow hello ve stavu Připraveno. Pokud neodehrává se po určité době čekat na, zobrazit, pokud máte hello vstupní soubor (input.log) umístit do hello správném kontejneru (adfgetstarted) a složce (inputdata).

   ![Vstupní řez ve stavu Připraveno](./media/data-factory-build-your-first-pipeline-using-editor/input-slice-ready.png)
6. Klikněte na tlačítko **X** tooclose **AzureBlobInput** okno.
7. V hello **zobrazení diagramu**, dvakrát klikněte na datovou sadu hello **AzureBlobOutput**. Uvidíte, že hello řez, který se právě zpracovává.

   ![Datová sada](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
8. Po dokončení zpracování uvidíte hello řez ve **připraven** stavu.

   ![Datová sada](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png)  

   > [!IMPORTANT]
   > Vytváření clusteru HDInsight na vyžádání většinou nějakou dobu trvá (přibližně 20 minut). Proto očekávat hello kanálu trvat příliš **přibližně 30 minut** tooprocess hello řez.
   >
   >

9. Pokud je řez hello v **připraven** stavu, zkontrolujte hello **partitioneddata** složky v hello **adfgetstarted** kontejneru ve službě blob storage hello výstupní data.  

   ![Výstupní data](./media/data-factory-build-your-first-pipeline-using-editor/three-ouptut-files.png)
10. Klikněte na tlačítko hello řez toosee podrobnosti o jeho **datový řez** okno.

   ![Podrobnosti o datovém řezu](./media/data-factory-build-your-first-pipeline-using-editor/data-slice-details.png)  
11. Klikněte na aktivitu spustit v hello **aktivita spuštěna seznamu** toosee podrobnosti o aktivitě spustit (Hive aktivitu v tomto scénáři) **podrobnosti o spuštění aktivit** okno.   

   ![Podrobnosti o spuštění aktivit](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-blade.png)    

   Ze souborů protokolů hello se zobrazí informace o stavu a hello dotaz Hive, která byla spuštěna. Tyto protokoly jsou užitečné při řešení potíží.
   Další podrobnosti najdete v článku [Monitorování a správa kanálů pomocí oken Azure Portal](data-factory-monitor-manage-pipelines.md).

> [!IMPORTANT]
> vstupní soubor Hello se po úspěšném zpracování řezu hello se odstraní. Chcete-li řez hello toorerun nebo znovu hello kurzu, tedy nahrajte hello vstupní soubor (input.log) toohello složky inputdata hello kontejneru adfgetstarted.
>
>

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Monitorování kanálu pomocí aplikace pro monitorování a správu
Také můžete použít monitorování a Správa aplikací toomonitor vašeho kanálů. Podrobnosti o použití této aplikace najdete v tématu [Monitorování a správa kanálů služby Azure Data Factory pomocí aplikace pro monitorování a správu](data-factory-monitor-manage-app.md).

1. Klikněte na tlačítko **monitorování a správa** na domovskou stránku hello objektu pro vytváření dat dlaždici.

    ![Dlaždice Monitorování a správa](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-tile.png)
2. Měla by se zobrazit **aplikace pro monitorování a správu**. Změna hello **počáteční čas** a **čas ukončení** toomatch spuštění a ukončení vašeho kanálu a klikněte na tlačítko **použít**.

    ![Aplikace pro monitorování a správu](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-app.png)
3. Vyberte okno s aktivity v hello **aktivity Windows** seznamu toosee podrobnosti o něm.

    ![Podrobnosti o okně aktivity](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-details.png)

## <a name="summary"></a>Souhrn
V tomto kurzu jste vytvořili Azure data factory tooprocess data pomocí skriptu Hive v clusteru HDInsight hadoop. Jste použili hello editoru služby Data Factory v hello Azure portálu toodo hello následující kroky:  

1. Vytvořili jste **objekt pro vytváření dat** Azure.
2. Vytvořili jste dvě **propojené služby**:
   1. **Úložiště Azure** propojené služby toolink služby Azure blob storage, který obsahuje vstupní/výstupní soubory toohello data factory.
   2. **Azure HDInsight** na vyžádání propojené služby toolink služby na vyžádání HDInsight Hadoop clusteru toohello data factory. Azure Data Factory vytvoří HDInsight Hadoop clusteru za běhu tooprocess vstupní data a výstupní data produktu.
3. Vytvořili jste dvě **datové sady**, které popisují vstupní a výstupní data aktivity HDInsight Hive v kanálu hello.
4. Vytvořili jste **kanál** s aktivitou **HDInsight Hive**.

## <a name="next-steps"></a>Další kroky
V tomto článku jste vytvořili kanál s aktivitou transformace (aktivita HDInsight), která v clusteru HDInsight na vyžádání spouští skript Hive. jak zjistit, toouse toocopy aktivity kopírování dat z tooAzure objektů Blob v Azure SQL, toosee [kurz: kopírování dat ze objektů blob v Azure tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="see-also"></a>Viz také
| Téma | Popis |
|:--- |:--- |
| [Kanály](data-factory-create-pipelines.md) |Tento článek vám pomůže pochopit kanály a aktivity v Azure Data Factory a jak toouse je tooconstruct začátku do konce řízené daty pracovní postupy pro vaší situaci nebo podniku. |
| [Datové sady](data-factory-create-datasets.md) |Tento článek vám pomůže pochopit datové sady ve službě Azure Data Factory. |
| [Plánování a provádění](data-factory-scheduling-and-execution.md) |Tento článek vysvětluje aspekty plánování a provádění hello aplikačního modelu služby Azure Data Factory. |
| [Monitorování a správa kanálů pomocí monitorovací aplikace](data-factory-monitor-manage-app.md) |Tento článek popisuje, jak toomonitor, spravovat a ladit kanály pomocí hello monitorování a správu aplikací. |
