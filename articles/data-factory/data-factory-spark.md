---
title: aaaInvoke Spark programy z Azure Data Factory | Microsoft Docs
description: "Zjistěte, jak tooinvoke Spark programy z objektu pro vytváření dat Azure pomocí hello činnost MapReduce."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: fd98931c-cab5-4d66-97cb-4c947861255c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: f88943ece7ee3d21dedbd857609f1b2713b62741
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="invoke-spark-programs-from-azure-data-factory-pipelines"></a>Vyvolání Spark programy z kanálů služby Azure Data Factory

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

## <a name="introduction"></a>Úvod
Aktivita Spark je jedním z hello [aktivit transformace dat](data-factory-data-transformation-activities.md) podporovaných službou Azure Data Factory. Tato aktivita se spustí hello zadaný program Spark na cluster Apache Spark v Azure HDInsight.    

> [!IMPORTANT]
> - Aktivita Spark nepodporuje clustery HDInsight Spark, které používají Azure Data Lake Store jako primární úložiště.
> - Aktivita Spark podporuje pouze existující (vlastní) clustery HDInsight Spark. HDInsight propojené služby na vyžádání nepodporuje.

## <a name="walkthrough-create-a-pipeline-with-spark-activity"></a>Návod: vytvoření kanálu s aktivitou Spark
Tady jsou toocreate hello obvyklé kroky pro vytváření dat kanál s aktivitou Spark.  

1. Vytvoření datové továrny
2. Vytvoření Azure Storage, propojené služby toolink vašeho úložiště Azure, která souvisí s datovou továrnu toohello clusteru HDInsight Spark.     
2. Vytvořte Azure HDInsight propojené služby toolink váš cluster Apache Spark v Azure HDInsight toohello data factory.
3. Vytvořte datovou sadu, která odkazuje toohello propojenou službu úložiště Azure. V současné době je třeba zadat datovou sadu výstupů pro aktivitu i v případě, že neexistuje žádný výstup se vytváří.  
4. Vytvoření kanálu s aktivitou Spark, která odkazuje toohello propojené služby HDInsight vytvořené v #. 2. Aktivita Hello je nakonfigurovaný s hello datovou sadu, kterou jste vytvořili v předchozím kroku hello jako výstupní datové. Hello výstupní datová sada, jaké jednotky hello plán (hodinový, denní, atd.). Proto je nutné zadat hello výstupní datovou sadu, i když hello aktivita nevytváří skutečně výstup.

### <a name="prerequisites"></a>Požadavky
1. Vytvořit **účet úložiště pro obecné účely Azure** podle pokynů v Průvodci hello: [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account).  
2. Vytvořit **cluster Apache Spark v Azure HDInsight** podle pokynů v kurzu hello: [cluster vytvořit Apache Spark v Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Přidružte hello účtu úložiště Azure, kterou jste vytvořili v kroku #1 se tento cluster.  
3. Stáhnout a revidovat soubor skriptu jazyka python hello **test.py** nacházející se v: [https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py).  
3.  Nahrát **test.py** toohello **pyFiles** složky v hello **adfspark** kontejneru ve službě Azure Blob storage. Pokud neexistuje, vytvořte hello kontejneru a složce hello.

### <a name="create-data-factory"></a>Vytvoření objektu pro vytváření dat
Začneme vytvořením objektu pro vytváření dat hello v tomto kroku.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Klikněte na tlačítko **nový** v levé nabídce hello, klikněte na tlačítko **Data + analýzy**a klikněte na tlačítko **Data Factory**.
3. V hello **nový objekt pro vytváření dat** okno, zadejte **SparkDF** pro hello název.

   > [!IMPORTANT]
   > musí být Hello název objektu pro vytváření dat Azure hello **globálně jedinečný**. Pokud se zobrazí chyba hello: **název objektu pro vytváření dat "SparkDF" není k dispozici**. Změňte hello název objektu pro vytváření dat hello (například yournameSparkDFdate a zkuste to znovu. V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.   
4. Vyberte hello **předplatného Azure** místo hello data factory toobe vytvořili.
5. Vyberte existující **skupiny prostředků** nebo vytvořte skupinu prostředků Azure.
6. Vyberte **Pin toodashboard** možnost.  
6. Klikněte na tlačítko **vytvořit** na hello **nový objekt pro vytváření dat** okno.

   > [!IMPORTANT]
   > instance služby Data Factory toocreate, musíte být členem skupiny hello [Přispěvatel objekt pro vytváření dat](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role na úrovni předplatného nebo prostředků skupiny hello.
7. Zobrazí hello objekt pro vytváření dat vytváří v hello **řídicí panel** z hello portál Azure následujícím způsobem:   
8. Po úspěšném vytvoření objektu pro vytváření dat hello, se zobrazí stránka objektu pro vytváření dat hello, se zobrazí hello obsah objektu pro vytváření dat hello. Pokud se stránka objektu pro vytváření dat hello nezobrazí, klikněte na dlaždici hello objektu pro vytváření dat na řídicím panelu hello.

    ![Okno Objekt pro vytváření dat](./media/data-factory-spark/data-factory-blade.png)

### <a name="create-linked-services"></a>Vytvoření propojených služeb
V tomto kroku vytvoříte dvě propojené služby, toolink jednu datovou továrnu tooyour clusteru Spark a hello jiných toolink datovou továrnu tooyour úložiště Azure.  

#### <a name="create-azure-storage-linked-service"></a>Vytvoření propojené služby Azure Storage
V tomto kroku propojíte datovou továrnu tooyour účet úložiště Azure. Datové sady, které vytvoříte v kroku později v tomto názorném postupu odkazuje toothis propojené služby. Hello propojené služby HDInsight, kterou definujete v dalším kroku hello příliš odkazuje toothis propojené služby.  

1. Klikněte na tlačítko **vytvořit a nasadit** na hello **Data Factory** okno objektu pro vytváření dat. Měli byste vidět hello editoru služby Data Factory.
2. Klikněte na **Nové datové úložiště** a zvolte **Účet Azure**.

   ![Nové úložiště dat – Azure Storage – nabídka](./media/data-factory-spark/new-data-store-azure-storage-menu.png)
3. Měli byste vidět hello **skript JSON** pro vytvoření Azure Storage propojená služba v editoru hello.

   ![Propojená služba Azure Storage](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. Nahraďte **název účtu** a **klíč účtu** hello názvem a přístupovým klíčem k účtu úložiště Azure. toolearn jak přístup tooget úložiště klíčů najdete v tématu hello informace o tom, jak tooview, kopírování a opětovné vytváření úložiště přístupové klíče v [spravovat váš účet úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-account).
5. toodeploy hello propojené služby, klikněte na tlačítko **nasadit** na panelu příkazů hello. Po hello propojené služby nasazení úspěšně hello **koncept-1** by měl zmizet okně a zobrazí **AzureStorageLinkedService** v hello stromovém zobrazení na levé straně hello.

#### <a name="create-hdinsight-linked-service"></a>Vytvoření propojené služby HDInsight
V tomto kroku vytvoříte toolink Azure HDInsight propojené služby clusteru HDInsight Spark toohello datovou továrnu. Hello HDInsight cluster je použité toorun hello Spark program zadaný v aktivitě Spark hello hello kanálu v této ukázce.  

1. Pokud tlačítko nevidíte, klikněte na panelu nástrojů na **... Další** na hello nástrojů, klikněte na tlačítko **nový výpočet**a potom klikněte na **clusteru HDInsight**.

    ![Vytvoření propojené služby HDInsight](media/data-factory-spark/new-hdinsight-linked-service.png)
2. Zkopírujte a vložte následující fragment kódu toohello hello **koncept-1** okno. V editoru JSON hello hello následující kroky:
    1. Zadejte hello **URI** hello HDInsight Spark clusteru. Například: `https://<sparkclustername>.azurehdinsight.net/`.
    2. Zadejte název hello hello **uživatele** kdo má cluster Spark toohello přístup.
    3. Zadejte hello **heslo** pro uživatele.
    4. Zadejte hello **propojená služba Azure Storage** který je přidružen hello clusteru HDInsight Spark. V tomto příkladu je: **AzureStorageLinkedService**.

    ```json
    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<sparkclustername>.azurehdinsight.net/",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }
    ```

    > [!IMPORTANT]
    > - Aktivita Spark nepodporuje clustery HDInsight Spark, které používají Azure Data Lake Store jako primární úložiště.
    > - Aktivita Spark podporuje pouze existující (vlastní) clusteru HDInsight Spark. HDInsight propojené služby na vyžádání nepodporuje.

    V tématu [propojená služba HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) podrobnosti o hello HDInsight propojené služby.
3.  toodeploy hello propojené služby, klikněte na tlačítko **nasadit** na panelu příkazů hello.  

### <a name="create-output-dataset"></a>Vytvoření výstupní datové sady
Hello výstupní datová sada, jaké jednotky hello plán (hodinový, denní, atd.). Proto je nutné zadat výstupní datovou sadu aktivity hello spark v kanálu hello, i když aktivita hello skutečně nevytváří žádný výstup. Určení vstupní datové sady pro aktivitu hello je volitelné.

1. V hello **editoru služby Data Factory**, klikněte na tlačítko **... Další** na panelu příkazů hello, klikněte na tlačítko **nová datová sada**a vyberte **úložiště objektů Azure Blob**.  
2. Zkopírujte a vložte následující fragment kódu toohello koncept-1 okno hello. Hello fragmentu kódu JSON Určuje datovou sadu s názvem **OutputDataset**. Kromě toho určíte, že hello výsledky ukládat do kontejneru objektů blob hello s názvem **adfspark** a hello složku s názvem **pyFiles výstupní**. Jak už bylo zmíněno dříve, je tato datová sada fiktivní datová sada. Hello Spark program v tomto příkladu nevytváří žádný výstup. Hello **dostupnosti** část určuje, že hello výstupní sada vytváří jednou denně.  

    ```json
    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "sparkoutput.txt",
                "folderPath": "adfspark/pyFiles/output",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }
    ```
3. toodeploy hello datovou sadu, klikněte na tlačítko **nasadit** na panelu příkazů hello.


### <a name="create-pipeline"></a>Vytvoření kanálu
V tomto kroku vytvoříte kanál s **HDInsightSpark** aktivity. Výstupní datové sady v současné době je, jaké jednotky hello plánu, takže je nutné vytvořit datovou sadu výstupů i v případě, že hello aktivita nevytváří žádný výstup. Pokud aktivita hello neberou žádný vstup, můžete přeskočit vytvoření vstupní datové sady hello. Proto žádné vstupní datové sady je zadána v tomto příkladu.

1. V hello **editoru služby Data Factory**, klikněte na tlačítko **... Další** na panelu příkazů text hello a potom klikněte na **nový kanál**.
2. Nahraďte hello skript v okně hello koncept-1 hello následující skript:

    ```json
    {
        "name": "SparkPipeline",
        "properties": {
            "activities": [
                {
                    "type": "HDInsightSpark",
                    "typeProperties": {
                        "rootPath": "adfspark\\pyFiles",
                        "entryFilePath": "test.py",
                        "getDebugInfo": "Always"
                    },
                    "outputs": [
                        {
                            "name": "OutputDataset"
                        }
                    ],
                    "name": "MySparkActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2017-02-05T00:00:00Z",
            "end": "2017-02-06T00:00:00Z"
        }
    }
    ```
    Všimněte si hello následující body:
    - Hello **typ** vlastnost je nastavena příliš**HDInsightSpark**.
    - Hello **rootPath** je nastaven příliš**adfspark\\pyFiles** kde adfspark je kontejner objektů Blob Azure hello a pyFiles je dobře složka v tomto kontejneru. V tomto příkladu je hello Azure Blob Storage hello ten, který je přidružen hello Spark cluster. Můžete nahrát soubor tooa hello jiného úložiště Azure. Pokud tak učiníte, vytváření Azure Storage, propojené služby toolink tohoto úložiště účet toohello dat. Potom zadejte název hello hello propojené služby jako hodnotu pro hello **sparkJobLinkedService** vlastnost. V tématu [vlastnosti aktivity Spark](#spark-activity-properties) podrobnosti o této vlastnosti a dalších vlastností nepodporuje hello Spark aktivity.  
    - Hello **entryFilePath** nastavena toohello **test.py**, což je soubor python hello.
    - Hello **getdebuginfo –** vlastnost je nastavena příliš**vždy**, což znamená, že soubory protokolu hello jsou vždy vygeneruje (úspěch nebo neúspěch).

        > [!IMPORTANT]
        > Doporučujeme, abyste tuto vlastnost příliš nenastavujte`Always` v produkčním prostředí Pokud řešíte problém.
    - Hello **výstupy** obsahuje jednu výstupní datovou sadu. Je třeba zadat datovou sadu výstupů i v případě, že hello spark program nevytváří žádný výstup. plán hello Hello výstupní datovou sadu jednotky pro kanál hello (hodinový, denní, atd.).  

        Podrobnosti o vlastnostech hello podporovaných aktivitou Spark najdete v tématu [Spark vlastnosti aktivity](#spark-activity-properties) části.
3. toodeploy hello kanálu, klikněte na tlačítko **nasadit** na panelu příkazů hello.

### <a name="monitor-pipeline"></a>Monitorování kanálu
1. Klikněte na tlačítko **X** oken editoru služby Data Factory tooclose a toonavigate zpět toohello objekt pro vytváření dat domovské stránky. Klikněte na tlačítko **monitorování a správa** toolaunch hello monitorování aplikací na jiné kartě.

    ![Dlaždice monitorování a Správa](media/data-factory-spark/monitor-and-manage-tile.png)
2. Změna hello **počáteční čas** filtru v horní části hello příliš**2/1/2017**a klikněte na tlačítko **použít**.
3. Jak je pouze jeden den mezi hello spuštění (2017-02-01) a ukončení (2017-02-02) hello kanálu, měli byste vidět jenom jeden interval aktivity. Potvrďte, že hello datový řez je v **připraven** stavu.

    ![Monitorování kanálu hello](media/data-factory-spark/monitor-and-manage-app.png)    
4. Vyberte hello **okně aktivita** toosee podrobnosti o hello aktivity při spuštění. Pokud dojde k chybě, zobrazí podrobnosti o něm v pravém podokně hello.

### <a name="verify-hello-results"></a>Zkontrolujte výsledky hello

1. Spusťte **Poznámkový blok Jupyter** pro váš cluster HDInsight Spark přechodem na: https://CLUSTERNAME.azurehdinsight.net/jupyter. Můžete také spustit řídicí panel clusteru pro váš cluster HDInsight Spark a poté spusťte **Poznámkový blok Jupyter**.
2. Klikněte na tlačítko **nový** -> **PySpark** toostart nový poznámkový blok.

    ![Nový poznámkový blok Jupyter](media/data-factory-spark/jupyter-new-book.png)
3. Spuštění hello následující příkaz kopírování/vkládání textu hello a stisknutím klávesy **SHIFT + ENTER** na konci hello druhý příkaz hello.  

    ```sql
    %%sql

    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```
4. Zkontrolujte, jestli hello data z tabulky TVK hello:  

    ![Výsledky dotazu Jupyter](media/data-factory-spark/jupyter-notebook-results.png)

V tématu [spuštění dotazů Spark SQL](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md#run-a-hive-query-using-spark-sql) části Podrobné pokyny. 

### <a name="troubleshooting"></a>Řešení potíží
Vzhledem k tomu, že nastavíte **getdebuginfo –** příliš**vždy**, uvidíte **protokolu** podsložky v hello **pyFiles** složky v kontejnerech objektů Blob Azure. Další podrobnosti najdete v souboru protokolu Hello hello do složky protokolů. Tento soubor protokolu je obzvláště užitečná, když dojde k chybě. V produkčním prostředí, může být vhodné tooset je příliš**selhání**.

Pro další informace o řešení, hello následující kroky:


1. Přejděte příliš`https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster`.

    ![Uživatelské rozhraní YARN aplikace](media/data-factory-spark/yarnui-application.png)  
2. Klikněte na tlačítko **protokoly** pro jednu hello spustit pokusy.

    ![Stránka aplikace](media/data-factory-spark/yarn-applications.png)
3. Měli byste vidět další informace o chybě v protokolu stránku hello.

    ![Chyba protokolu](media/data-factory-spark/yarnui-application-error.png)

Hello následující části obsahují informace o cluster Apache Spark toouse entity služby Data Factory a aktivita Spark v datové továrně.

## <a name="spark-activity-properties"></a>Vlastnosti aktivity Spark
Tady je hello ukázka definici JSON kanálu s aktivitou Spark:    

```json
{
    "name": "SparkPipeline",
    "properties": {
        "activities": [
            {
                "type": "HDInsightSpark",
                "typeProperties": {
                    "rootPath": "adfspark\\pyFiles",
                    "entryFilePath": "test.py",
                    "arguments": [ "arg1", "arg2" ],
                    "sparkConfig": {
                        "spark.python.worker.memory": "512m"
                    }
                    "getDebugInfo": "Always"
                },
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "MySparkActivity",
                "description": "This activity invokes hello Spark program",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-01T00:00:00Z",
        "end": "2017-02-02T00:00:00Z"
    }
}
```

Hello následující tabulka popisuje hello vlastnostech JSON použitých ve hello definici JSON:

| Vlastnost | Popis | Požaduje se |
| -------- | ----------- | -------- |
| jméno | Název hello aktivity v kanálu hello. | Ano |
| description | Text popisující jaké aktivity hello nepodporuje. | Ne |
| type | Tato vlastnost musí být nastavená tooHDInsightSpark. | Ano |
| linkedServiceName | Název hello HDInsight propojené služby, na které hello Spark program se spustí. | Ano |
| rootPath | kontejner objektů Blob Azure Hello a složky, která obsahuje soubor Spark hello. Název souboru Hello je malá a velká písmena. | Ano |
| entryFilePath | Relativní cesta toohello kořenové složky hello Spark nebo balíček kódu. | Ano |
| Název třídy | Hlavní třídy aplikace Java/Spark | Ne |
| Argumenty | Seznam argumentů příkazového řádku toohello Spark programu. | Ne |
| proxyUser | Spark program hello tooimpersonate tooexecute Hello uživatelského účtu | Ne |
| sparkConfig | Zadejte hodnoty pro vlastnosti konfigurace Spark uvedených v tématu hello: [Spark konfigurace – vlastnosti aplikace](https://spark.apache.org/docs/latest/configuration.html#available-properties). | Ne |
| getdebuginfo – | Určuje, když jsou soubory protokolu Spark hello zkopírovaný toohello úložiště Azure používá HDInsight cluster (nebo) zadaný ve sparkJobLinkedService. Povolené hodnoty: None, vždy nebo selhání. Výchozí hodnota: žádné. | Ne |
| sparkJobLinkedService | Hello Azure Storage propojená služba, která obsahuje soubor úlohy Spark hello, závislosti a protokoly.  Pokud hodnotu pro tuto vlastnost nezadáte, použije se hello úložiště přidruženého k clusteru HDInsight. | Ne |

## <a name="folder-structure"></a>Struktura složek
Hello Spark aktivity nepodporuje skriptu v řádku jako Pig a do aktivity Hive. Spark úlohy jsou také více extensible než úlohy Pig nebo Hive. Pro úlohy Spark, můžete zadat více závislostí, jako jar balíčky (umístěné v jazyce java hello cesty pro třídy), soubory python (umístit na hello PYTHONPATH) a všechny další soubory.

Vytvořte hello následující strukturu složek v hello úložiště objektů Azure Blob odkazuje hello propojené služby HDInsight. Potom obsah nahrajete závislé soubory toohello odpovídající podsložek hello kořenové složky, která je reprezentována **entryFilePath**. Můžete například nahrajte python soubory toohello pyFiles podsložky a soubory jar toohello JAR podsložky hello kořenové složky. Služba Data Factory v době běhu očekává hello následující strukturu složek v hello úložiště objektů Blob v Azure:     

| Cesta | Popis | Požaduje se | Typ |
| ---- | ----------- | -------- | ---- |
| . | Kořenová cesta Hello hello úlohy Spark v hello propojená služba úložiště    | Ano | Složka |
| &lt;uživatelem definované&gt; | Cesta Hello odkazující toohello vstupní soubor úlohy Spark hello | Ano | File |
| . / jars | Všechny soubory v této složce se nahrála a umístit na cestě třídy java hello hello clusteru | Ne | Složka |
| . / pyFiles | Všechny soubory v této složce se nahrála a umístit na hello PYTHONPATH hello clusteru | Ne | Složka |
| . / soubory | Všechny soubory v této složce se nahrála a umístit na vykonavatele pracovní adresář | Ne | Složka |
| . / archivy | Všechny soubory v této složce nekomprimované | Ne | Složka |
| . / protokoly | Hello složku, kde jsou uloženy protokoly z clusteru Spark hello.| Ne | Složka |

Tady je příklad pro úložiště, který obsahuje dva soubory úlohy Spark v Azure Blob Storage odkazuje hello propojené služby HDInsight hello.

```
SparkJob1
    main.jar
    files
        input1.txt
        input2.txt
    jars
        package1.jar
        package2.jar
    logs

SparkJob2
    main.py
    pyFiles
        scrip1.py
        script2.py
    logs
```
