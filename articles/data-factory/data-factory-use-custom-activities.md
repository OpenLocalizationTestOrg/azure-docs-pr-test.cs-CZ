---
title: "aaaUse vlastní aktivity v kanálu Azure Data Factory"
description: "Zjistěte, jak toocreate vlastní aktivity a použijte je v kanál služby Azure Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 8dd7ba14-15d2-4fd9-9ada-0b2c684327e9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: 23e33727b2160541ab40938ffd911fdd484b3daa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-custom-activities-in-an-azure-data-factory-pipeline"></a>Použití vlastních aktivit v kanálu Azure Data Factory

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

Existují dva typy aktivit, které můžete použít v kanál služby Azure Data Factory.

- [Aktivity přesunu dat](data-factory-data-movement-activities.md) toomove dat mezi [podporovanými úložišti dat zdroj a jímka](data-factory-data-movement-activities.md#supported-data-stores-and-formats).
- [Aktivity transformace dat](data-factory-data-transformation-activities.md) tootransform dat pomocí výpočetních služeb například Azure HDInsight, Azure Batch a Azure Machine Learning. 

Vytvoření toomove data do nebo z úložiště dat, které objekt pro vytváření dat nepodporuje, **vlastní aktivity** s vlastní data přesun logiku a použití hello aktivitu v kanálu. Podobně tootransform nebo zpracování dat způsobem, který není podporován službou Data Factory vytvořit vlastní aktivity s vlastní logikou transformaci dat a použít hello aktivity v kanálu. 

Můžete nakonfigurovat vlastní aktivity toorun na **Azure Batch** fondu virtuálních počítačů nebo systémem Windows **Azure HDInsight** clusteru. Pokud používáte Azure Batch, můžete použít pouze existujícího fondu Azure Batch. Vzhledem k tomu, při použití HDInsight, můžete použít stávající cluster HDInsight nebo clusteru, který se automaticky vytvoří pro můžete na vyžádání za běhu.  

Hello následující názorný postup obsahuje podrobné pokyny pro vytváření vlastních aktivit rozhraní .NET a používání hello vlastní aktivity v kanálu. návod Hello používá **Azure Batch** propojené služby. toouse Azure HDInsight propojená služba místo, vytvoření propojené služby typu **HDInsight** (vlastní cluster HDInsight) nebo **HDInsightOnDemand** (Data Factory vytvoří cluster služby HDInsight na vyžádání). Potom nakonfigurujte vlastní aktivity toouse hello propojené služby HDInsight. V tématu [použití Azure HDInsight propojené služby](#use-hdinsight-compute-service) část Podrobnosti o použití Azure HDInsight toorun hello vlastní aktivity.

> [!IMPORTANT]
> - vlastní aktivity .NET Hello spustit pouze na clusterech HDInsight se systémem Windows. Alternativní řešení pro toto omezení je toouse hello mapy snížit aktivity toorun vlastní Java kód na clusteru HDInsight se systémem Linux. Další možností je toouse fondu Azure Batch virtuální počítače toorun vlastních aktivit místo použití clusteru HDInsight.
> - Není možné toouse Brána pro správu dat z vlastní aktivity tooaccess místní datové zdroje. V současné době [Brána pro správu dat](data-factory-data-management-gateway.md) podporuje pouze aktivity kopírování hello a aktivity uložené procedury v datové továrně.   

## <a name="walkthrough-create-a-custom-activity"></a>Návod: vytvoření vlastní aktivity
### <a name="prerequisites"></a>Požadavky
* Visual Studio 2012/2013 nebo 2015
* Stáhněte sadu [Azure .NET SDK](https://azure.microsoft.com/downloads/) a nainstalujte ji.

### <a name="azure-batch-prerequisites"></a>Požadavky Azure Batch
V Průvodci hello spustíte vlastní aktivitu .NET pomocí Azure Batch jako výpočetní prostředek. **Azure Batch** je platforma služby pro spuštění rozsáhlé paralelní a vysoce výkonné aplikace (HPC) efektivně v hello cloud computing. Azure Batch plány toorun výpočetně náročných na spravované **kolekci virtuálních počítačů**, a dokáže automaticky škálovat výpočetní prostředky toomeet hello potřeby vašich úloh. V tématu [základy Azure Batch] [ batch-technical-overview] článku podrobnější přehled služby Azure Batch hello.

Hello kurzu vytvoříte účet Azure Batch se fondu virtuálních počítačů. Zde jsou kroky hello:

1. Vytvoření **účtu Azure Batch** pomocí hello [portál Azure](http://portal.azure.com). V tématu [vytvoření a Správa účtu Azure Batch] [ batch-create-account] pokyny najdete v článku.
2. Poznamenejte si název účtu Azure Batch hello, klíč účtu, identifikátor URI a název fondu. Musíte je toocreate služby Azure Batch propojený.
    1. Na domovské stránce hello pro účet Azure Batch, uvidíte **URL** v hello následující formát: `https://myaccount.westus.batch.azure.com`. V tomto příkladu **stránku Můj účet** je název hello hello účtu Azure Batch. Identifikátor URI, které můžete použít v definici hello propojené služby je adresa URL hello bez názvu hello hello účtu. Například: `https://<region>.batch.azure.com`.
    2. Klikněte na tlačítko **klíče** v levé nabídce hello a kopírování hello **primární přístupový klíč**.
    3. Klikněte na existující fond, toouse **fondy** v nabídce hello a zapište hello **ID** hello fondu. Pokud nemáte existující fond, přesuňte toohello další krok.     
2. Vytvoření **fondu Azure Batch**.

   1. V hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **Procházet** v levé nabídce text hello a klikněte na tlačítko **účty Batch**.
   2. Vyberte vaše hello tooopen účet Azure Batch **účtu Batch** okno.
   3. Klikněte na tlačítko **fondy** dlaždici.
   4. V hello **fondy** okně klikněte na tlačítko Přidat na panelu nástrojů tooadd hello fondu.
      1. Zadejte ID fondu hello (ID fondu). Poznámka: hello **ID fondu hello**; je nutné při vytváření řešení Data Factory hello.
      2. Zadejte **Windows Server 2012 R2** pro nastavení hello řada operačního systému.
      3. Vyberte **cenovou úroveň uzlu**.
      4. Zadejte **2** jako hodnota pro hello **cíl vyhrazené** nastavení.
      5. Zadejte **2** jako hodnota pro hello **maximální počet úloh na uzel** nastavení.
   5. Klikněte na tlačítko **OK** toocreate hello fondu.
   6. Zapište hello **ID** hello fondu. 



### <a name="high-level-steps"></a>Postup vysoké úrovně
Zde jsou hello dva základní kroky, které můžete provádět v rámci tohoto návodu: 

1. Vytvořte vlastní aktivitu, která obsahuje jednoduché datové transformaci/zpracování logiky.
2. Vytvořte objekt pro vytváření dat Azure s kanál, který používá vlastní aktivity hello.

### <a name="create-a-custom-activity"></a>Vytvoření vlastní aktivity
Vytvoření toocreate .NET vlastní aktivitu, **knihovna tříd rozhraní .NET** projektu s třídou, která implementuje **IDotNetActivity** rozhraní. Toto rozhraní obsahuje pouze jednu metodu: [Execute](https://msdn.microsoft.com/library/azure/mt603945.aspx) a jeho podpis je:

```csharp
public IDictionary<string, string> Execute(
        IEnumerable<LinkedService> linkedServices,
        IEnumerable<Dataset> datasets,
        Activity activity,
        IActivityLogger logger)
```


Metoda Hello přijímá čtyř parametrů:

- **linkedServices**. Tato vlastnost je vyčíslitelná seznam úložiště dat propojené služby odkazuje vstupní a výstupní datové sady pro aktivitu hello.   
- **datové sady**. Tato vlastnost je vyčíslitelná seznam vstupní a výstupní datové sady pro aktivitu hello. Můžete použít tento parametr tooget hello umístění a schémata definované vstupní a výstupní datové sady.
- **aktivita**. Tato vlastnost představuje hello aktuální aktivity. Dá se použít tooaccess rozšířené vlastnosti související s hello vlastní aktivity. V tématu [přístup rozšířené vlastnosti](#access-extended-properties) podrobnosti.
- **protokolovač**. Tento objekt umožňuje zapisovat ladění komentáře tento prostor v protokolu uživatele hello hello kanálu.

Hello metoda vrátí slovník, který může být vlastní aktivity používané toochain společně v budoucnosti hello. Tato funkce není dosud implementována, takže vrátí prázdný slovník z metody hello.  

### <a name="procedure"></a>Postup
1. Vytvoření **knihovna tříd rozhraní .NET** projektu.
   <ol type="a">
     <li>Spusťte <b>Visual Studio 2017</b> nebo <b>sady Visual Studio 2015</b> nebo <b>Visual Studio 2013</b> nebo <b>sady Visual Studio 2012</b>.</li>
     <li>Klikněte na tlačítko <b>soubor</b>, bod příliš<b>nový</b>a klikněte na tlačítko <b>projektu</b>.</li>
     <li>Rozbalte <b>Šablony</b> a vyberte <b>Visual C#</b>. V tomto návodu použijete C#, ale můžete použít libovolné .NET jazyk toodevelop hello vlastní aktivity.</li>
     <li>Vyberte <b>knihovny tříd</b> hello seznamu typů projektu na hello správné. V VS 2017, zvolte <b>knihovny tříd (rozhraní .NET Framework)</b> </li>
     <li>Zadejte <b>MyDotNetActivity</b> pro hello <b>název</b>.</li>
     <li>Vyberte <b>C:\ADFGetStarted</b> pro hello <b>umístění</b>.</li>
     <li>Klikněte na tlačítko <b>OK</b> toocreate hello projektu.</li>
   </ol>
2.Klikněte na tlačítko **nástroje**, bod příliš**Správce balíčků NuGet**a klikněte na tlačítko **Konzola správce balíčků**.
3. V hello Konzola správce balíčků, spustit následující příkaz tooimport hello **Microsoft.Azure.Management.DataFactories**.

    ```PowerShell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. Import hello **Azure Storage** balíček NuGet v toohello projektu.

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    > [!IMPORTANT]
    > Spouštěč služby Data Factory vyžaduje hello 4.3 verzi WindowsAzure.Storage. Pokud přidáte odkaz tooa novější verze sestavení Azure Storage ve vašem projektu vlastní aktivitu, zobrazí chybu když hello aktivita provede. Chyba hello tooresolve, najdete v části [izolace domény aplikace](#appdomain-isolation) části. 
5. Přidejte následující hello **pomocí** příkazy toohello zdrojový soubor v projektu hello.

    ```csharp

    // Comment these lines if using VS 2017
    using System.IO;
    using System.Globalization;
    using System.Diagnostics;
    using System.Linq;
    // --------------------

    // Comment these lines if using <= VS 2015
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    // ---------------------

    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Runtime;

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```
6. Změnit název hello hello **obor názvů** příliš**MyDotNetActivityNS**.

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. Změňte název hello třídy hello příliš**MyDotNetActivity** a odvodí z hello **IDotNetActivity** rozhraní, jak je znázorněno v následujícím fragmentu kódu hello:

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. Implementace (Přidat) hello **Execute** metoda hello **IDotNetActivity** rozhraní toohello **MyDotNetActivity** hello třídy a zkopírujte následující ukázka kódu toohello metoda.

    Hello následující příklad vrátí hello počet výskytů hello hledaný termín ("Microsoft") v jednotlivých objektů blob přidružený datový řez.

    ```csharp
    /// <summary>
    /// Execute method is hello only method of IDotNetActivity interface you must implement.
    /// In this sample, hello method invokes hello Calculate method tooperform hello core logic.  
    /// </summary>
    
    public IDictionary<string, string> Execute(
        IEnumerable<LinkedService> linkedServices,
        IEnumerable<Dataset> datasets,
        Activity activity,
        IActivityLogger logger)
    {
        // get extended properties defined in activity JSON definition
        // (for example: SliceStart)
        DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
        string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];
    
        // toolog information, use hello logger object
        // log all extended properties            
        IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
        logger.Write("Logging extended properties if any...");
        foreach (KeyValuePair<string, string> entry in extendedProperties)
        {
            logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
        }
    
        // linked service for input and output data stores
        // in this example, same storage is used for both input/output
        AzureStorageLinkedService inputLinkedService;

        // get hello input dataset
        Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
        // declare variables toohold type properties of input/output datasets
        AzureBlobDataset inputTypeProperties, outputTypeProperties;
        
        // get type properties from hello dataset object
        inputTypeProperties = inputDataset.Properties.TypeProperties as AzureBlobDataset;
    
        // log linked services passed in linkedServices parameter
        // you will see two linked services of type: AzureStorage
        // one for input dataset and hello other for output dataset 
        foreach (LinkedService ls in linkedServices)
            logger.Write("linkedService.Name {0}", ls.Name);
    
        // get hello first Azure Storate linked service from linkedServices object
        // using First method instead of Single since we are using hello same
        // Azure Storage linked service for input and output.
        inputLinkedService = linkedServices.First(
            linkedService =>
            linkedService.Name ==
            inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
            as AzureStorageLinkedService;
    
        // get hello connection string in hello linked service
        string connectionString = inputLinkedService.ConnectionString;
    
        // get hello folder path from hello input dataset definition
        string folderPath = GetFolderPath(inputDataset);
        string output = string.Empty; // for use later.
    
        // create storage client for input. Pass hello connection string.
        CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
        CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();
    
        // initialize hello continuation token before using it in hello do-while loop.
        BlobContinuationToken continuationToken = null;
        do
        {   // get hello list of input blobs from hello input storage client object.
            BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                     true,
                                     BlobListingDetails.Metadata,
                                     null,
                                     continuationToken,
                                     null,
                                     null);
    
            // Calculate method returns hello number of occurrences of
            // hello search term (“Microsoft”) in each blob associated
               // with hello data slice. definition of hello method is shown in hello next step.
    
            output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
        } while (continuationToken != null);
    
        // get hello output dataset using hello name of hello dataset matched tooa name in hello Activity output collection.
        Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

        // get type properties for hello output dataset
        outputTypeProperties = outputDataset.Properties.TypeProperties as AzureBlobDataset;
    
        // get hello folder path from hello output dataset definition
        folderPath = GetFolderPath(outputDataset);

        // log hello output folder path 
        logger.Write("Writing blob toohello folder: {0}", folderPath);
    
        // create a storage object for hello output blob.
        CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
        // write hello name of hello file.
        Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
        // log hello output file name
        logger.Write("output blob URI: {0}", outputBlobUri.ToString());

        // create a blob and upload hello output text.
        CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
        logger.Write("Writing {0} toohello output blob", output);
        outputBlob.UploadText(output);
    
        // hello dictionary can be used toochain custom activities together in hello future.
        // This feature is not implemented yet, so just return an empty dictionary.  
    
        return new Dictionary<string, string>();
    }
    ```
9. Přidejte následující metody helper hello: 

    ```csharp
    /// <summary>
    /// Gets hello folderPath value from hello input/output dataset.
    /// </summary>
    
    private static string GetFolderPath(Dataset dataArtifact)
    {
        if (dataArtifact == null || dataArtifact.Properties == null)
        {
            return null;
        }

        // get type properties of hello dataset 
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }
    
        // return hello folder path found in hello type properties
        return blobDataset.FolderPath;
    }
    
    /// <summary>
    /// Gets hello fileName value from hello input/output dataset.   
    /// </summary>
    
    private static string GetFileName(Dataset dataArtifact)
    {
        if (dataArtifact == null || dataArtifact.Properties == null)
        {
            return null;
        }
    
        // get type properties of hello dataset
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }
    
        // return hello blob/file name in hello type properties
        return blobDataset.FileName;
    }
    
    /// <summary>
    /// Iterates through each blob (file) in hello folder, counts hello number of instances of search term in hello file,
    /// and prepares hello output text that is written toohello output blob.
    /// </summary>
    
    public static string Calculate(BlobResultSegment Bresult, IActivityLogger logger, string folderPath, ref BlobContinuationToken token, string searchTerm)
    {
        string output = string.Empty;
        logger.Write("number of blobs found: {0}", Bresult.Results.Count<IListBlobItem>());
        foreach (IListBlobItem listBlobItem in Bresult.Results)
        {
            CloudBlockBlob inputBlob = listBlobItem as CloudBlockBlob;
            if ((inputBlob != null) && (inputBlob.Name.IndexOf("$$$.$$$") == -1))
            {
                string blobText = inputBlob.DownloadText(Encoding.ASCII, null, null, null);
                logger.Write("input blob text: {0}", blobText);
                string[] source = blobText.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' }, StringSplitOptions.RemoveEmptyEntries);
                var matchQuery = from word in source
                                 where word.ToLowerInvariant() == searchTerm.ToLowerInvariant()
                                 select word;
                int wordCount = matchQuery.Count();
                output += string.Format("{0} occurrences(s) of hello search term \"{1}\" were found in hello file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
            }
        }
        return output;
    }
    ```

    Hello GetFolderPath metoda vrátí hello cesta toohello složky, která hello datová sada odkazuje tooand hello GetFileName metoda vrátí hello název hello objektů blob nebo souboru, který hello body datovou sadu. Pokud jste havefolderPath definuje pomocí proměnných, například {Year}, {Month}, {Day} atd., hello metoda vrátí hello řetězec, jako je bez jejich nahrazení s hodnotami modulu runtime. V tématu [přístup rozšířené vlastnosti](#access-extended-properties) část Podrobnosti o přístup k SliceStart, SliceEnd atd.    

    ```JSON
    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "adftutorial/inputfolder/",
    ```

    Hello Calculate metoda vypočítá hello počet instancí – klíčové slovo Microsoft v hello vstupní soubory (objekty BLOB ve složce hello). hledaný termín Hello ("Microsoft") je pevně zakódovaná v kódu hello.
10. Kompilace projektu hello. Klikněte na tlačítko **sestavení** hello nabídky a klikněte na **sestavit řešení**.

    > [!IMPORTANT]
    > Nastavit 4.5.2 verzi rozhraní .NET Framework jako hello cílové rozhraní pro svůj projekt: klikněte pravým tlačítkem na projekt hello a klikněte na tlačítko **vlastnosti** tooset hello cílové rozhraní. Objekt pro vytváření dat nepodporuje vlastní aktivity, které jsou novější než 4.5.2 zkompilovat proti verze rozhraní .NET Framework.

11. Spusťte **Průzkumníka Windows**a přejděte příliš**bin\debug** nebo **bin\release** složku v závislosti na typu hello sestavení.
12. Vytvořte soubor zip **MyDotNetActivity.zip** obsahující všechny binární soubory hello v hello <project folder>\bin\Debug složka. Zahrnout hello **MyDotNetActivity.pdb** tak, aby získat další podrobnosti, jako je například číslo řádku v hello zdrojového kódu, která způsobila problém hello, pokud došlo k chybě. 

    > [!IMPORTANT]
    > Všechny soubory v souboru zip hello hello hello vlastní aktivity musí být v hello **top úroveň** s žádné podsložek.

    ![Binární výstupní soubory](./media/data-factory-use-custom-activities/Binaries.png)
14. Vytvořte kontejner objektů blob s názvem **customactivitycontainer** Pokud ještě neexistuje. 
15. Nahrát MyDotNetActivity.zip jako customactivitycontainer toohello objektů blob v **pro obecné účely** úložiště objektů blob v Azure (úložiště objektů Blob není za provozu nebo nástrojů), který odkazuje AzureStorageLinkedService.  

> [!IMPORTANT]
> Pokud přidáte toto rozhraní .NET aktivity projektu tooa řešení v sadě Visual Studio, který obsahuje projekt Data Factory a přidat odkaz na too.NET aktivity projekt z projektu aplikace hello objekt pro vytváření dat, není nutné tooperform hello poslední dva kroky ručně vytváření soubor zip Hello a pak ho nahrát toohello úložiště objektů blob v Azure pro obecné účely. Když publikujete entity služby Data Factory pomocí sady Visual Studio, tyto kroky automaticky provádí proces publikování hello. Další informace najdete v tématu [projekt Data Factory v sadě Visual Studio](#data-factory-project-in-visual-studio) části.

## <a name="create-a-pipeline-with-custom-activity"></a>Vytvoření kanálu s vlastní aktivity
Vytvoření vlastní aktivity a nahrát soubor zip hello se binární soubory tooa kontejneru objektů blob v **pro obecné účely** účet úložiště Azure. V této části vytvoříte objekt pro vytváření dat Azure s kanál, který používá vlastní aktivity hello.

vstupní datové sady Hello vlastní aktivity hello představuje objekty BLOB (soubory) ve složce customactivityinput hello adftutorial kontejneru v úložiště objektů blob hello. Hello výstupní datovou sadu aktivity hello představuje výstupní objekty BLOB ve složce customactivityoutput hello adftutorial kontejneru v úložiště objektů blob hello.

Vytvoření **soubor.txt** soubor s hello následující obsahu a nahrajte ho příliš**customactivityinput** složky hello **adftutorial** kontejneru. Vytvoření kontejneru adftutorial hello, pokud ho ještě neexistuje. 

```
test custom activity Microsoft test custom activity Microsoft
```

vstupní složky Hello odpovídá tooa řez v Azure Data Factory i v případě, že složka hello obsahuje dvě nebo více souborů. Při zpracování kanálu hello se každý řez iteruje hello vlastní aktivity všech objektů BLOB hello v hello vstupní složky pro tento řez.

Uvidíte, že jednu výstupní soubor s ve složce adftutorial\customactivityoutput hello s jedním nebo více řádky (stejný jako počet objektů BLOB ve složce vstupní hello):

```
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2016-11-16-00/file.txt.
```


Zde jsou hello kroky, které můžete provést v této části:

1. Vytvoření **objekt pro vytváření dat**.
2. Vytvoření **propojené služby** fondu Azure Batch hello virtuálních počítačů, na které hello vlastní aktivity spouští a hello Azure Storage, který obsahuje objekty BLOB vstupu a výstupu hello.
3. Vytvoření vstupní a výstupní **datové sady** které představují vstupní a výstupní hello vlastní aktivity.
4. Vytvoření **kanálu** používající hello vlastní aktivity.

> [!NOTE]
> Vytvoření hello **soubor.txt** a nahrajte ho tooa kontejner objektů blob, pokud jste tak již neučinili. Najdete v pokynech v předcházející části hello.   

### <a name="step-1-create-hello-data-factory"></a>Krok 1: Vytvoření objektu pro vytváření dat hello
1. Po přihlášení toohello portálu Azure, hello následující kroky:
   1. Klikněte na tlačítko **nový** v levé nabídce hello.
   2. Klikněte na tlačítko **Data + analýzy** v hello **nový** okno.
   3. Klikněte na tlačítko **Data Factory** na hello **analýzy dat** okno.
   
    ![Nové nabídky Azure Data Factory](media/data-factory-use-custom-activities/new-azure-data-factory-menu.png)
2. V hello **nový objekt pro vytváření dat** okno, zadejte **CustomActivityFactory** pro hello název. Hello název objektu pro vytváření dat Azure hello musí být globálně jedinečný. Pokud se zobrazí chyba hello: **název objektu pro vytváření dat "CustomActivityFactory" není k dispozici**, změňte hello název objektu pro vytváření dat hello (například **yournameCustomActivityFactory**) a zkuste vytvořit znovu.

    ![Nové okno Azure Data Factory](media/data-factory-use-custom-activities/new-azure-data-factory-blade.png)
3. Klikněte na tlačítko **název skupiny prostředků**a vyberte existující skupinu prostředků nebo vytvořte skupinu prostředků.
4. Ověřte, že používáte správné hello **předplatné** a **oblast** místo hello data factory toobe vytvořili.
5. Klikněte na tlačítko **vytvořit** na hello **nový objekt pro vytváření dat** okno.
6. Zobrazí hello objekt pro vytváření dat vytváří v hello **řídicí panel** z hello portálu Azure.
7. Po úspěšném vytvoření objektu pro vytváření dat hello, zobrazí okno hello objekt pro vytváření dat, se zobrazí hello obsah objektu pro vytváření dat hello.
    
    ![Okno Objekt pro vytváření dat](media/data-factory-use-custom-activities/data-factory-blade.png)

### <a name="step-2-create-linked-services"></a>Krok 2: Vytvoření propojených služeb
Propojené služby propojují úložiště dat nebo výpočetní služby tooan pro vytváření dat Azure. V tomto kroku propojíte svůj účet úložiště Azure a účet Azure Batch tooyour služby data factory.

#### <a name="create-azure-storage-linked-service"></a>Vytvoření propojené služby Azure Storage
1. Klikněte na tlačítko hello **vytvořit a nasadit** na hello dlaždici **DATA FACTORY** okno pro **CustomActivityFactory**. Zobrazí hello editoru služby Data Factory.
2. Klikněte na tlačítko **nové datové úložiště** na panelu příkazů hello a zvolte **úložiště Azure**. Měli byste vidět hello skript JSON pro vytvoření Azure Storage propojená služba v editoru hello.
    
    ![Nové datové úložiště - Azure Storage](media/data-factory-use-custom-activities/new-data-store-menu.png)
3. Nahraďte `<accountname>` s názvem účtu úložiště Azure a `<accountkey>` s přístupovým klíčem hello účtu úložiště Azure. toolearn tooget úložiště přístupu klíče najdete v tématu [zobrazení, kopírování a opětovné vytváření přístupových klíčů úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-account).

    ![Úložiště Azure líbilo služby](media/data-factory-use-custom-activities/azure-storage-linked-service.png)
4. Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello propojené služby.

#### <a name="create-azure-batch-linked-service"></a>Vytvoření služby Azure Batch propojené
1. V editoru služby Data Factory hello, klikněte na **... Další** na panelu příkazů hello, klikněte na tlačítko **nový výpočet**a potom vyberte **Azure Batch** nabídce hello.

    ![Nový výpočet - Azure Batch](media/data-factory-use-custom-activities/new-azure-compute-batch.png)
2. Ujistěte se, hello následující skript JSON toohello změny:

   1. Zadejte název účtu Azure Batch pro hello **accountName** vlastnost. Hello **URL** z hello **okně účtu Azure Batch** je ve formátu hello: `http://accountname.region.batch.azure.com`. Pro hello **batchUri** vlastnost hello JSON, bude nutné tooremove `accountname.` z adresy URL a použijte hello hello `accountname` pro hello `accountName` vlastnost JSON.
   2. Zadejte klíč účtu Azure Batch hello pro hello **accessKey** vlastnost.
   3. Zadejte název hello hello fondu, které jste vytvořili v rámci požadavků pro hello **poolName** vlastnost. Můžete také zadat hello ID fondu hello místo názvu hello hello fondu.
   4. Zadejte identifikátor URI Azure Batch pro hello **batchUri** vlastnost. Příklad: `https://westus.batch.azure.com`.  
   5. Zadejte hello **AzureStorageLinkedService** pro hello **linkedServiceName** vlastnost.

        ```json
        {
         "name": "AzureBatchLinkedService",
         "properties": {
           "type": "AzureBatch",
           "typeProperties": {
             "accountName": "myazurebatchaccount",
             "batchUri": "https://westus.batch.azure.com",
             "accessKey": "<yourbatchaccountkey>",
             "poolName": "myazurebatchpool",
             "linkedServiceName": "AzureStorageLinkedService"
           }
         }
        }
        ```

       Pro hello **poolName** vlastnost, můžete také zadat hello ID fondu hello místo názvu hello hello fondu.

      > [!IMPORTANT]
      > Hello služba Data Factory nepodporuje možnost na vyžádání pro Azure Batch, stejně jako pro HDInsight. Vlastní fondu Azure Batch můžete použít pouze v objektu pro vytváření dat Azure.   
    

### <a name="step-3-create-datasets"></a>Krok 3: Vytvoření datové sady
V tomto kroku vytvoříte datové sady toorepresent vstupní a výstupní data.

#### <a name="create-input-dataset"></a>Vytvoření vstupní datové sady
1. V hello **Editor** hello objekt pro vytváření dat, klikněte na tlačítko **... Další** na panelu příkazů hello, klikněte na tlačítko **nová datová sada**a potom vyberte **úložiště objektů Azure Blob** z rozevírací nabídky hello.
2. Nahraďte hello JSON v pravém podokně hello hello následujícím fragmentu kódu JSON:

    ```json
    {
     "name": "InputDataset",
     "properties": {
         "type": "AzureBlob",
         "linkedServiceName": "AzureStorageLinkedService",
         "typeProperties": {
             "folderPath": "adftutorial/customactivityinput/",
             "format": {
                 "type": "TextFormat"
             }
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

   Vytvoření kanálu dále v tomto návodu se časem spuštění: 2016-11-16T00:00:00Z a koncový čas: 2016-11-16T05:00:00Z. Je naplánované tooproduce dat každou hodinu, takže se pět řezy vstupní a výstupní (mezi **00**: -> 00:00 **05**: 00:00).

   Hello **frekvence** a **interval** hello vstupní datové sady je nastavený příliš**hodinu** a **1**, což znamená, že hello vstupní řez je k dispozici každou hodinu. V této ukázce je hello stejný soubor (soubor.txt) v hello intputfolder.

   Tady jsou hello času zahájení pro každý řez, která je reprezentována SliceStart systémové proměnné v hello výše fragmentu kódu JSON.
3. Klikněte na tlačítko **nasadit** hello toocreate panelu nástrojů a nasadit hello **InputDataset**. Zkontrolujte, jestli hello **úspěšné vytvoření tabulky** zprávu na hello záhlaví hello Editor.

#### <a name="create-an-output-dataset"></a>Vytvoření výstupní datové sady
1. V hello **editoru služby Data Factory**, klikněte na tlačítko **... Další** na panelu příkazů hello, klikněte na tlačítko **nová datová sada**a potom vyberte **úložiště objektů Azure Blob**.
2. Nahraďte hello skript JSON v pravém podokně hello hello následující skript JSON:

    ```JSON
    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "{slice}.txt",
                "folderPath": "adftutorial/customactivityoutput/",
                "partitionedBy": [
                    {
                        "name": "slice",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy-MM-dd-HH"
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

     Výstupní umístění je **adftutorial/customactivityoutput/** a název výstupního souboru je rrrr MM-dd-HH.txt, kde je rrrr MM-dd HH hello rok, měsíc, datum a hodinu hello řez se vytváří. V tématu [referenční informace pro vývojáře] [ adf-developer-reference] podrobnosti.

    Objekt blob nebo soubor výstupu se generuje pro každý vstupní řez. Zde je, jak je výstupní soubor s názvem pro každý řez. Všechny soubory výstup hello vygenerují na jednu výstupní složky: **adftutorial\customactivityoutput**.

   | Řez | Čas spuštění | Výstupní soubor |
   |:--- |:--- |:--- |
   | 1 |2016-11-16T00:00:00 |2016 11 16 00.txt |
   | 2 |2016-11-16T01:00:00 |2016 11 16 01.txt |
   | 3 |2016-11-16T02:00:00 |2016 11 16 02.txt |
   | 4 |2016-11-16T03:00:00 |2016 11 16 03.txt |
   | 5 |2016-11-16T04:00:00 |2016 11 16 04.txt |

    Mějte na paměti, že všechny soubory hello ve vstupní složky jsou součástí řez s časy zahájení hello uvedených výše. Při zpracování této řezu se vlastní aktivity hello prohledává každý soubor a vytvoří řádek v souboru výstup hello s hello počtu výskytů hledaný termín ("Microsoft"). Pokud existují tři soubory v hello inputfolder, jsou tři řádky v hello výstupní soubor pro každou hodinové řez: 2016-11-16-00.txt 2016-11-16:01:00:00.txt atd.
3. toodeploy hello **OutputDataset**, klikněte na tlačítko **nasadit** na panelu příkazů hello.

### <a name="create-and-run-a-pipeline-that-uses-hello-custom-activity"></a>Vytvoření a spuštění kanálu, který používá vlastní aktivity hello
1. V editoru služby Data Factory hello, klikněte na **... Další**a potom vyberte **nový kanál** na panelu příkazů hello. 
2. Nahraďte hello JSON v pravém podokně hello hello následující skript JSON:

    ```JSON
    {
      "name": "ADFTutorialPipelineCustom",
      "properties": {
        "description": "Use custom activity",
        "activities": [
          {
            "Name": "MyDotNetActivity",
            "Type": "DotNetActivity",
            "Inputs": [
              {
                "Name": "InputDataset"
              }
            ],
            "Outputs": [
              {
                "Name": "OutputDataset"
              }
            ],
            "LinkedServiceName": "AzureBatchLinkedService",
            "typeProperties": {
              "AssemblyName": "MyDotNetActivity.dll",
              "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
              "PackageLinkedService": "AzureStorageLinkedService",
              "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
              "extendedProperties": {
                "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
              }
            },
            "Policy": {
              "Concurrency": 2,
              "ExecutionPriorityOrder": "OldestFirst",
              "Retry": 3,
              "Timeout": "00:30:00",
              "Delay": "00:00:00"
            }
          }
        ],
        "start": "2016-11-16T00:00:00Z",
        "end": "2016-11-16T05:00:00Z",
        "isPaused": false
      }
    }
    ```

    Všimněte si hello následující body:

   * **Concurrency** je nastaven příliš**2** tak, aby dva řezy jsou zpracovávány paralelně 2 virtuální počítače ve fondu Azure Batch hello.
   * V části aktivity hello je jedna aktivita a je typu: **DotNetActivity**.
   * **AssemblyName** nastavena toohello název hello knihovny DLL: **MyDotnetActivity.dll**.
   * **EntryPoint** je nastaven příliš**MyDotNetActivityNS.MyDotNetActivity**.
   * **PackageLinkedService** je nastaven příliš**AzureStorageLinkedService** který odkazuje toohello úložiště objektů blob, který obsahuje soubor zip hello vlastní aktivity. Pokud používáte různé účty Azure Storage pro vstupní a výstupní soubory a hello soubor zip vlastní aktivitu, vytvoříte jiné propojené služby Azure Storage. Tento článek předpokládá, že používáte hello stejný účet úložiště Azure.
   * **PackageFile** je nastaven příliš**customactivitycontainer/MyDotNetActivity.zip**. Je ve formátu hello: containerforthezip/nameofthezip.zip.
   * vlastní aktivita Hello přijímá **InputDataset** jako vstup a **OutputDataset** jako výstup.
   * Vlastnost linkedServiceName Hello vlastní aktivity hello ukazuje toohello **AzureBatchLinkedService**, která informuje Azure Data Factory hello vlastní aktivity musí toorun na virtuálních počítačích Azure Batch.
   * **isPaused** vlastnost je nastavena příliš**false** ve výchozím nastavení. kanál Hello spustí hned v tomto příkladu, protože hello řezy spustit v posledních hello. Můžete nastavit tuto vlastnost tootrue toopause hello kanálu a nastavte ji zpět toofalse toorestart.
   * Hello **spustit** čas a **end** časy jsou **pět** hodiny od sebe a řezy vytváří každou hodinu, takže pět řezy vytváří hello kanálu.
3. toodeploy hello kanálu, klikněte na tlačítko **nasadit** na panelu příkazů hello.

### <a name="monitor-hello-pipeline"></a>Monitorování kanálu hello
1. V okně Data Factory hello v hello portálu Azure, klikněte na tlačítko **Diagram**.

    ![Dlaždice Diagram](./media/data-factory-use-custom-activities/DataFactoryBlade.png)
2. V hello zobrazení diagramu klikněte teď hello OutputDataset.

    ![Zobrazení diagramu](./media/data-factory-use-custom-activities/diagram.png)
3. Měli byste vidět, že hello pět Výstup řezy jsou ve stavu Připraveno hello. Pokud nejsou ve stavu Připraveno hello, nebyly byla vypracována ještě. 

   ![Výstup řezy](./media/data-factory-use-custom-activities/OutputSlices.png)
4. Ověřte, že jsou generovány hello výstupní soubory v úložišti objektů blob hello v hello **adftutorial** kontejneru.

   ![výstup z vlastní aktivity][image-data-factory-ouput-from-custom-activity]
5. Pokud otevřete hello výstupního souboru, měli byste vidět, že hello výstup podobný toohello následující výstup:

    ```
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2016-11-16-00/file.txt.
    ```
6. Použití hello [portál Azure] [ azure-preview-portal] nebo toomonitor rutin prostředí Azure PowerShell vaše služby data factory, kanálů a datové sady. Zobrazí se zprávy z hello **ActivityLogger** v kódu hello hello vlastní aktivity v protokolech hello (konkrétně uživatel 0.log), které si můžete stáhnout z portálu hello nebo pomocí rutin.

   ![stažení protokolů z vlastní aktivity][image-data-factory-download-logs-from-custom-activity]

V tématu [monitorování a Správa kanálů](data-factory-monitor-manage-pipelines.md) podrobné pokyny pro monitorování datových sad a kanálů.      

## <a name="data-factory-project-in-visual-studio"></a>Projekt data Factory v sadě Visual Studio  
Můžete vytvořit a publikovat entity služby Data Factory pomocí sady Visual Studio místo použití portálu Azure. Podrobné informace o vytváření a publikování entit služby Data Factory pomocí sady Visual Studio, najdete v části [sestavit svůj první kanál pomocí sady Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) a [kopírování dat z Azure Blob tooAzure SQL](data-factory-copy-activity-tutorial-using-visual-studio.md) články.

Hello následující další kroky, pokud vytváříte projekt Data Factory v sadě Visual Studio:
 
1. Přidejte toohello projekt Data Factory hello řešení sady Visual Studio, které obsahuje hello vlastní aktivity projektu. 
2. Přidáte odkaz na toohello .NET aktivity projekt z projektu pro vytváření dat hello. Klikněte pravým tlačítkem na projekt Data Factory, přejděte příliš**přidat**a potom klikněte na **odkaz**. 
3. V hello **přidat odkaz na** dialogové okno, vyberte hello **MyDotNetActivity** projektu a klikněte na tlačítko **OK**.
4. Sestavení a publikování řešení hello.

    > [!IMPORTANT]
    > Když publikujete entity služby Data Factory, soubor zip je je automaticky vytvořen a je kontejner objektů blob nahrané toohello: customactivitycontainer. Pokud kontejner objektů blob hello neexistuje, je vytvořeno automaticky příliš.  


## <a name="data-factory-and-batch-integration"></a>Integrace služby Data Factory a Batch
Hello služba Data Factory vytvoří úlohu ve službě Azure Batch s názvem hello: **adf poolname: xxx úlohy**. Klikněte na tlačítko **úlohy** hello levé nabídce. 

![Azure Data Factory - úlohy Batch](media/data-factory-use-custom-activities/data-factory-batch-jobs.png)

Úloha se vytvoří při každém spuštění aktivity řezu. Pokud existují pět toobe připraven řezy, které se zpracovat, jsou v této úloze vytvořit pět úloh. Pokud existují několika výpočetních uzlech v hello fondu Batch, dva nebo více řezů může běžet paralelně. Pokud hello maximální úkolů na výpočetní uzel je nastaven příliš > 1, můžete také máte více než jeden řez systémem hello stejné výpočty.

![Azure Data Factory - úlohy Batch](media/data-factory-use-custom-activities/data-factory-batch-job-tasks.png)

Hello následující diagram znázorňuje hello vztah mezi Azure Data Factory a dávkové úlohy.

![Objekt pro vytváření dat & Batch](./media/data-factory-use-custom-activities/DataFactoryAndBatch.png)

## <a name="troubleshoot-failures"></a>Řešení chyb
Řešení potíží se skládá z několika základní techniky:

1. Pokud se zobrazí následující chyba hello, můžete pomocí úložiště objektů blob aktivní/nástrojů místo použití úložiště objektů blob v Azure pro obecné účely. Nahrát tooa soubor zip hello **účet úložiště pro obecné účely Azure**. 
 
    ```
    Error in Activity: Job encountered scheduling error. Code: BlobDownloadMiscError Category: ServerError Message: Miscellaneous error encountered while downloading one of hello specified Azure Blob(s).
    ``` 
2. Pokud se zobrazí následující chyba hello, ověřte, že hello název třídy hello v hello CS odpovídá hello zadaný název souboru je pro hello **EntryPoint** vlastnost v kódu JSON kanálu hello. V návodu hello je název třídy hello: MyDotNetActivity a hello EntryPoint v hello JSON je: MyDotNetActivityNS. **MyDotNetActivity**.

    ```
    MyDotNetActivity assembly does not exist or doesn't implement hello type Microsoft.DataFactories.Runtime.IDotNetActivity properly
    ```

   Pokud se názvy hello shodují, potvrďte, že všechny binární soubory hello jsou v hello **kořenové složky** hello souboru zip. To znamená když otevřete soubor zip hello, měli byste vidět všechny soubory hello hello kořenové složky, není ve všech podsložek.   
3. Pokud vstupní řez hello není nastaven příliš**připraven**, potvrďte správnost hello vstupní složky struktura a **soubor.txt** existuje ve vstupní složkách hello.
3. V hello **Execute** metoda vlastní aktivity, použijte hello **IActivityLogger** toolog informace o objektu, který vám pomůže vyřešit problémy. zprávy Hello přihlášení zobrazí v souborech protokolů uživatele hello (jeden nebo více souborů s názvem: uživatel 0.log, 1.log uživatele, uživatel 2.log atd.).

   V hello **OutputDataset** okně klikněte na tlačítko hello řez toosee hello **datový ŘEZ** okno pro tento řez. Zobrazí **běh aktivit** pro tento řez. Měli byste vidět jednu aktivitu spustit pro řez hello. Pokud kliknete na tlačítko spustit v hello příkazového řádku, můžete spustit jinou aktivitu spustit pro hello stejné řez.

   Po kliknutí na tlačítko hello aktivity při spuštění, se zobrazí hello **podrobnosti o spuštění aktivit** okno se seznamem souborů protokolu. Zobrazí zprávy zaznamenané v souboru user_0.log hello. Pokud dojde k chybě, uvidíte tři běh aktivit protože hello počet opakování je nastavena too3 v kanálu nebo aktivity hello JSON. Po kliknutí na tlačítko hello aktivity při spuštění, zobrazí hello soubory protokolů, abyste si prošli tootroubleshoot hello chyby.

   V seznamu hello protokolových souborů, klikněte na tlačítko hello **uživatele 0.log**. V pravém panelu hello jsou výsledky hello pomocí hello **IActivityLogger.Write** metoda. Pokud nevidíte všechny zprávy, zkontrolujte, zda máte další soubory protokolu s názvem: user_1.log, user_2.log atd. Kód hello nebyla úspěšná, jinak hodnota po hello posledního přihlášení zpráva.

   Kromě toho zkontrolujte **systému 0.log** pro všechny chybové zprávy systému a výjimky.
4. Zahrnout hello **PDB** souborů v souboru zip hello tak, aby podrobnosti o chybě hello informace, jako **zásobník volání** když dojde k chybě.
5. Všechny soubory v souboru zip hello hello hello vlastní aktivity musí být v hello **top úroveň** s žádné podsložek.
6. Ujistěte se, že hello **assemblyName** (MyDotNetActivity.dll), **entryPoint**(MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer / MyDotNetActivity.zip), a **packageLinkedService** (by měla odkazovat toohello **pro obecné účely**úložiště objektů blob v Azure, která obsahuje soubor zip hello) jsou nastavené toocorrect hodnoty.
7. Pokud můžete opravit chyby a chcete řez hello tooreprocess, klikněte pravým tlačítkem na hello řez ve hello **OutputDataset** a klikněte na **spustit**.
8. Pokud se zobrazí následující chyba hello, používáte hello Azure Storage balíčku verze > 4.3.0. Spouštěč služby Data Factory vyžaduje hello 4.3 verzi WindowsAzure.Storage. V tématu [izolace domény aplikace](#appdomain-isolation) části pro řešení, pokud je nutné použít hello novější verze sestavení Azure Storage. 

    ```
    Error in Activity: Unknown error in module: System.Reflection.TargetInvocationException: Exception has been thrown by hello target of an invocation. ---> System.TypeLoadException: Could not load type 'Microsoft.WindowsAzure.Storage.Blob.CloudBlob' from assembly 'Microsoft.WindowsAzure.Storage, Version=4.3.0.0, Culture=neutral, 
    ```

    Pokud používáte hello 4.3.0 verzi balíčku Azure Storage, odeberte hello existující odkaz tooAzure úložiště balíčku verze > 4.3.0. Potom spusťte následující příkaz z konzoly Správce balíčků NuGet hello. 

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    Sestavení projektu hello. Odstraňte Azure.Storage sestavením verze > 4.3.0 ze složky bin\Debug hello. Vytvořte soubor zip s binární soubory a souboru PDB hello. Nahraďte původní soubor zip hello touto v kontejneru objektů blob hello (customactivitycontainer). Spusťte znovu hello řezy, které se nezdařilo (klikněte pravým tlačítkem na řez a klikněte na tlačítko Spustit).   
8. vlastní aktivity Hello nepoužívá hello **app.config** soubor ze svého balíčku. Proto pokud váš kód čte libovolné púřipojovací řetězce z konfiguračního souboru hello, ale nefunguje za běhu. Hello osvědčený postup, při použití Azure Batch je toohold všech tajných klíčů v **Azure KeyVault**, používat služeb na základě certifikátu hlavní tooprotect hello **keyvault**a distribuovat certifikát hello tooAzure fondu služby Batch. Hello vlastní aktivity .NET pak k dispozici tajné klíče z hello KeyVault za běhu. Toto řešení je obecný řešení a můžete škálovat tooany typ tajný klíč, ne jenom připojovací řetězec.

   Je snazší alternativní řešení (ale není z hlediska): můžete vytvořit **propojená služba Azure SQL** s nastavení připojovacího řetězce, vytvořit datovou sadu, zda text hello používá propojené služby a řetězu hello datovou sadu jako fiktivní vstupní datové sady toohello vlastní aktivity .NET. Můžete pak přístup hello propojené služby připojovací řetězec v kódu hello vlastní aktivity.  

## <a name="update-custom-activity"></a>Aktualizovat vlastní aktivitu
Pokud aktualizujete hello kód hello vlastní aktivity, sestavte jej a nahrajte hello soubor zip, který obsahuje nové úložiště objektů blob toohello binární soubory.

## <a name="appdomain-isolation"></a>Izolace domény aplikace
Najdete v části [mezi ukázka AppDomain](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) který ukazuje, jak toocreate vlastní aktivitu, která není omezené tooassembly verze používané Spouštěče hello objekt pro vytváření dat (Příklad: WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x atd.).

## <a name="access-extended-properties"></a>Přístup rozšířené vlastnosti
V kódu JSON aktivity hello můžou deklarovat rozšířených vlastností, jak je znázorněno v hello následující ukázka:

```JSON
"typeProperties": {
  "AssemblyName": "MyDotNetActivity.dll",
  "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
  "PackageLinkedService": "AzureStorageLinkedService",
  "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
  "extendedProperties": {
    "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))",
    "DataFactoryName": "CustomActivityFactory"
  }
},
```


V příkladu hello existují dvě rozšířené vlastnosti: **SliceStart** a **DataFactoryName**. Hodnota Hello SliceStart vychází hello SliceStart systémové proměnné. V tématu [systémové proměnné](data-factory-functions-variables.md) seznam podporovaných systémové proměnné. Hodnota Hello DataFactoryName je pevně tooCustomActivityFactory.

tooaccess tyto rozšířené vlastnosti v hello **Execute** metodu, pomocí kódu podobné toohello následující kód:

```csharp
// tooget extended properties (for example: SliceStart)
DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];

// toolog all extended properties                               
IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
logger.Write("Logging extended properties if any...");
foreach (KeyValuePair<string, string> entry in extendedProperties)
{
    logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
}
```

## <a name="auto-scaling-of-azure-batch"></a>Automatické škálování služby Azure Batch
Můžete také vytvořit fond Azure Batch s **škálování** funkce. Můžete například vytvořit fondu azure batch s 0 vyhrazených virtuálních počítačích a vzorec škálování na základě hello počtu úkolů čekajících na zpracování. 

jednoduchý vzorec Hello dosahuje hello následující chování: při počátečním vytvoření fondu hello začíná 1 virtuální počítač. Metrika $PendingTasks definuje hello počet úloh v spuštěná + aktivní (zařazených do fronty) stavu.  Vzorec Hello najde hello průměrný počet úkolů čekajících na zpracování v hello posledních 180 sekund a nastaví TargetDedicated odpovídajícím způsobem. Zajišťuje, že TargetDedicated nikdy překročí 25 virtuálních počítačů. Tak, jako jsou odeslány nové úkoly, fondu automaticky zvětšování a jako dokončení úkolů, virtuální počítače stane volné jeden po druhém a automatické škálování hello zmenšuje těchto virtuálních počítačů. startingNumberOfVMs a maxNumberofVMs může být upravenou tooyour potřebám.

Vzorec škálování:

``` 
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
```

V tématu [automatické škálování výpočetních uzlů ve fondu Azure Batch](../batch/batch-automatic-scaling.md) podrobnosti.

Pokud fond hello používá výchozí hello [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), hello služba Batch může trvat 15 až 30 minut tooprepare hello virtuálních počítačů před spuštěním hello vlastní aktivity.  Pokud fond hello používá jiný autoScaleEvaluationInterval, může trvat hello služba Batch autoScaleEvaluationInterval + 10 minut.

## <a name="use-hdinsight-compute-service"></a>Pomocí HDInsight výpočetní služby
V Průvodci hello používá Azure Batch výpočetní toorun hello vlastní aktivity. Můžete také použít vlastní cluster HDInsight se systémem Windows nebo mít objekt pro vytváření dat vytvořit na vyžádání clusteru HDInsight se systémem Windows a mít hello vlastní aktivity při spuštění v clusteru HDInsight hello. Tady jsou hlavní kroky hello pomocí clusteru služby HDInsight.

> [!IMPORTANT]
> vlastní aktivity .NET Hello spustit pouze na clusterech HDInsight se systémem Windows. Alternativní řešení pro toto omezení je toouse hello mapy snížit aktivity toorun vlastní Java kód na clusteru HDInsight se systémem Linux. Další možností je toouse fondu Azure Batch virtuální počítače toorun vlastních aktivit místo použití clusteru HDInsight.
 

1. Vytvoření služby Azure HDInsight propojený.   
2. Použití HDInsight propojená služba místě **AzureBatchLinkedService** v hello kanálu JSON.

Pokud chcete, aby tootest její hello návod, změna **spustit** a **end** časy pro kanál hello tak, aby hello scénář s hello služby Azure HDInsight můžete otestovat.

#### <a name="create-azure-hdinsight-linked-service"></a>Vytvoření propojené služby Azure HDInsight
Hello služby Azure Data Factory podporuje vytváření clusteru na vyžádání a použít ho tooprocess vstupní tooproduce výstupní data. Můžete také vlastní cluster tooperform hello stejné. Pokud používáte cluster HDInsight na vyžádání, získá pro každý řez vytvořit cluster. Vzhledem k tomu, pokud chcete použít vlastní cluster HDInsight, hello cluster je připraven hello tooprocess řez okamžitě. Proto když používáte cluster na vyžádání, nemusíte vidět hello výstupní data co když použijete vlastní cluster.

> [!NOTE]
> V době běhu instance aktivity .NET funguje pouze na jeden uzel pracovního procesu v clusteru HDInsight hello; nelze ji škálovat toorun ve více uzlech. Více instancí .NET aktivity může běžet paralelně na různých uzlech clusteru HDInsight hello.
>
>

##### <a name="toouse-an-on-demand-hdinsight-cluster"></a>toouse clusteru HDInsight na vyžádání
1. V hello **portál Azure**, klikněte na tlačítko **vytvořit a nasadit** domovské stránce objektu pro vytváření dat hello.
2. V editoru služby Data Factory hello, klikněte na **nový výpočet** z příkazu hello panelu a vyberte možnost **clusteru HDInsight na vyžádání** nabídce hello.
3. Ujistěte se, hello následující skript JSON toohello změny:

   1. Pro hello **parametr clusterSize** vlastnost, zadejte velikost hello hello clusteru HDInsight.
   2. Pro hello **timeToLive** vlastnost, zadejte, jak dlouho hello zákazníka může být nečinnosti, než se odstraní.
   3. Pro hello **verze** vlastnost, zadejte verzi HDInsight hello chcete toouse. Pokud vyloučíte tuto vlastnost, použije se nejnovější verze hello.  
   4. Pro hello **linkedServiceName**, zadejte **AzureStorageLinkedService**.

        ```JSON
        {
           "name": "HDInsightOnDemandLinkedService",
           "properties": {
               "type": "HDInsightOnDemand",
               "typeProperties": {
                   "clusterSize": 4,
                   "timeToLive": "00:05:00",
                   "osType": "Windows",
                   "linkedServiceName": "AzureStorageLinkedService",
               }
           }
        }
        ```

    > [!IMPORTANT]
    > vlastní aktivity .NET Hello spustit pouze na clusterech HDInsight se systémem Windows. Alternativní řešení pro toto omezení je toouse hello mapy snížit aktivity toorun vlastní Java kód na clusteru HDInsight se systémem Linux. Další možností je toouse fondu Azure Batch virtuální počítače toorun vlastních aktivit místo použití clusteru HDInsight.

4. Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello propojené služby.

##### <a name="toouse-your-own-hdinsight-cluster"></a>toouse vlastní cluster HDInsight:
1. V hello **portál Azure**, klikněte na tlačítko **vytvořit a nasadit** domovské stránce objektu pro vytváření dat hello.
2. V hello **editoru služby Data Factory**, klikněte na tlačítko **nový výpočet** z příkazu hello panelu a vyberte možnost **clusteru HDInsight** nabídce hello.
3. Ujistěte se, hello následující skript JSON toohello změny:

   1. Pro hello **clusterUri** vlastnost, zadejte adresu URL hello pro vaše prostředí HDInsight. Příklad: https://<clustername>.azurehdinsight.net/     
   2. Pro hello **uživatelské jméno** vlastnost, zadejte uživatelské jméno hello, který má cluster HDInsight toohello přístup.
   3. Pro hello **heslo** vlastnost, zadejte heslo hello hello uživatele.
   4. Pro hello **LinkedServiceName** vlastnost, zadejte **AzureStorageLinkedService**.
4. Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello propojené služby.

V tématu [výpočetní propojené služby](data-factory-compute-linked-services.md) podrobnosti.

V hello **kanálu JSON**, použití HDInsight (na vyžádání nebo vlastní) propojené služby:

```JSON
{
  "name": "ADFTutorialPipelineCustom",
  "properties": {
    "description": "Use custom activity",
    "activities": [
      {
        "Name": "MyDotNetActivity",
        "Type": "DotNetActivity",
        "Inputs": [
          {
            "Name": "InputDataset"
          }
        ],
        "Outputs": [
          {
            "Name": "OutputDataset"
          }
        ],
        "LinkedServiceName": "HDInsightOnDemandLinkedService",
        "typeProperties": {
          "AssemblyName": "MyDotNetActivity.dll",
          "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
          "PackageLinkedService": "AzureStorageLinkedService",
          "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
          "extendedProperties": {
            "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
          }
        },
        "Policy": {
          "Concurrency": 2,
          "ExecutionPriorityOrder": "OldestFirst",
          "Retry": 3,
          "Timeout": "00:30:00",
          "Delay": "00:00:00"
        }
      }
    ],
    "start": "2016-11-16T00:00:00Z",
    "end": "2016-11-16T05:00:00Z",
    "isPaused": false
  }
}
```

## <a name="create-a-custom-activity-by-using-net-sdk"></a>Vytvoření vlastní aktivity pomocí .NET SDK
V Průvodci hello v tomto článku vytvořit objekt pro vytváření dat kanál, který používá vlastní aktivity hello pomocí hello portálu Azure. Hello následující kód ukazuje, jak toocreate hello objekt pro vytváření dat pomocí .NET SDK. Můžete najít další podrobnosti o použití sady SDK tooprogrammatically vytvořit kanály v hello [vytvoření kanálu s aktivitou kopírování pomocí rozhraní API .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md) článku. 

```csharp
using System;
using System.Configuration;
using System.Collections.ObjectModel;
using System.Threading;
using System.Threading.Tasks;

using Microsoft.Azure;
using Microsoft.Azure.Management.DataFactories;
using Microsoft.Azure.Management.DataFactories.Models;
using Microsoft.Azure.Management.DataFactories.Common.Models;

using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Collections.Generic;

namespace DataFactoryAPITestApp
{
    class Program
    {
        static void Main(string[] args)
        {
            // create data factory management client

            // TODO: replace ADFTutorialResourceGroup with hello name of your resource group.
            string resourceGroupName = "ADFTutorialResourceGroup";

            // TODO: replace APITutorialFactory with a name that is globally unique. For example: APITutorialFactory04212017
            string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
                ConfigurationManager.AppSettings["SubscriptionId"],
                GetAuthorizationHeader().Result);

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

            Console.WriteLine("Creating a data factory");
            client.DataFactories.CreateOrUpdate(resourceGroupName,
                new DataFactoryCreateOrUpdateParameters()
                {
                    DataFactory = new DataFactory()
                    {
                        Name = dataFactoryName,
                        Location = "westus",
                        Properties = new DataFactoryProperties()
                    }
                }
            );

            // create a linked service for input data store: Azure Storage
            Console.WriteLine("Creating Azure Storage linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureStorageLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            // TODO: Replace <accountname> and <accountkey> with name and key of your Azure Storage account.
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );

            // create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure Batch linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureBatchLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            // TODO: replace <batchaccountname> and <yourbatchaccountkey> with name and key of your Azure Batch account
                            new AzureBatchLinkedService("<batchaccountname>", "https://westus.batch.azure.com", "<yourbatchaccountkey>", "myazurebatchpool", "AzureStorageLinkedService")
                        )
                    }
                }
            );

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "InputDataset";
            string Dataset_Destination = "OutputDataset";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,

                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Source,
                        Properties = new DatasetProperties()
                        {
                            LinkedServiceName = "AzureStorageLinkedService",
                            TypeProperties = new AzureBlobDataset()
                            {
                                FolderPath = "adftutorial/customactivityinput/",
                                Format = new TextFormat()
                            },
                            External = true,
                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },

                            Policy = new Policy() { }
                        }
                    }
                });

            Console.WriteLine("Creating output dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Destination,
                        Properties = new DatasetProperties()
                        {
                            LinkedServiceName = "AzureStorageLinkedService",
                            TypeProperties = new AzureBlobDataset()
                            {
                                FileName = "{slice}.txt",
                                FolderPath = "adftutorial/customactivityoutput/",
                                PartitionedBy = new List<Partition>()
                                {
                                    new Partition()
                                    {
                                        Name = "slice",
                                        Value = new DateTimePartitionValue()
                                        {
                                            Date = "SliceStart",
                                            Format = "yyyy-MM-dd-HH"
                                        }
                                    }
                                }
                            },
                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },
                        }
                    }
                });

            Console.WriteLine("Creating a custom activity pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2017, 3, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipelineCustom";

            client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new PipelineCreateOrUpdateParameters()
                {
                    Pipeline = new Pipeline()
                    {
                        Name = PipelineName,
                        Properties = new PipelineProperties()
                        {
                            Description = "Use custom activity",

                            // Initial value for pipeline's active period. With this, you won't need tooset slice status
                            Start = PipelineActivePeriodStartTime,
                            End = PipelineActivePeriodEndTime,
                            IsPaused = false,

                            Activities = new List<Activity>()
                            {
                                new Activity()
                                {
                                    Name = "MyDotNetActivity",
                                    Inputs = new List<ActivityInput>()
                                    {
                                        new ActivityInput() {
                                            Name = Dataset_Source
                                        }
                                    },
                                    Outputs = new List<ActivityOutput>()
                                    {
                                        new ActivityOutput()
                                        {
                                            Name = Dataset_Destination
                                        }
                                    },
                                    LinkedServiceName = "AzureBatchLinkedService",
                                    TypeProperties = new DotNetActivity()
                                    {
                                        AssemblyName = "MyDotNetActivity.dll",
                                        EntryPoint = "MyDotNetActivityNS.MyDotNetActivity",
                                        PackageLinkedService = "AzureStorageLinkedService",
                                        PackageFile = "customactivitycontainer/MyDotNetActivity.zip",
                                        ExtendedProperties = new Dictionary<string, string>()
                                        {
                                            { "SliceStart", "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"}
                                        }
                                    },
                                    Policy = new ActivityPolicy()
                                    {
                                        Concurrency = 2,
                                        ExecutionPriorityOrder = "OldestFirst",
                                        Retry = 3,
                                        Timeout = new TimeSpan(0,0,30,0),
                                        Delay = new TimeSpan()
                                    }
                                }
                            }
                        }
                    }
                });
        }

        public static async Task<string> GetAuthorizationHeader()
        {
            AuthenticationContext context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
            ClientCredential credential = new ClientCredential(
                ConfigurationManager.AppSettings["ApplicationId"],
                ConfigurationManager.AppSettings["Password"]);
            AuthenticationResult result = await context.AcquireTokenAsync(
                resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                clientCredential: credential);

            if (result != null)
                return result.AccessToken;

            throw new InvalidOperationException("Failed tooacquire token");
        }
    }
}
```

## <a name="debug-custom-activity-in-visual-studio"></a>Ladění vlastní aktivity v sadě Visual Studio
Hello [Azure Data Factory - místní prostředí](https://github.com/gbrueckl/Azure.DataFactory.LocalEnvironment) ukázce na Githubu zahrnuje nástroj, který vám umožní toodebug vlastní .NET aktivity v sadě Visual Studio.  


## <a name="sample-custom-activities-on-github"></a>Ukázka vlastních aktivit na Githubu
| Ukázka | Jaké vlastní aktivita provede |
| --- | --- |
| [Stažení programu HTTP Data](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample). |Stáhne data koncový bod HTTP tooAzure Blob Storage pomocí vlastní C# aktivitu v datové továrně. |
| [Ukázka postojích Analysis služby Twitter.](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |Vyvolá modelu Azure ML a proveďte analýzu postojích, vyhodnocování, předpovědi atd. |
| [Spustit skript jazyka R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). |Vyvolá skript jazyka R spuštěním RScript.exe, který již má nainstalované R na clusteru HDInsight. |
| [Mezi domény aplikace .NET aktivity](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) |Používá jiný sestavení verze z těch, které jsou používané Spouštěče hello objekt pro vytváření dat |
| [Zpracování modelu v Azure Analysis Services](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/AzureAnalysisServicesProcessSample) |  Znovu zpracuje model v Azure Analysis Services. |

[batch-net-library]: ../batch/batch-dotnet-get-started.md
[batch-create-account]: ../batch/batch-account-create-portal.md
[batch-technical-overview]: ../batch/batch-technical-overview.md
[batch-get-started]: ../batch/batch-dotnet-get-started.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[data-factory-introduction]: data-factory-introduction.md
[azure-powershell-install]: https://github.com/Azure/azure-sdk-tools/releases


[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456

[new-azure-batch-account]: https://msdn.microsoft.com/library/mt125880.aspx
[new-azure-batch-pool]: https://msdn.microsoft.com/library/mt125936.aspx
[azure-batch-blog]: http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx

[nuget-package]: http://go.microsoft.com/fwlink/?LinkId=517478
[adf-developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[azure-preview-portal]: https://portal.azure.com/

[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[hivewalkthrough]: data-factory-data-transformation-activities.md

[image-data-factory-ouput-from-custom-activity]: ./media/data-factory-use-custom-activities/OutputFilesFromCustomActivity.png

[image-data-factory-download-logs-from-custom-activity]: ./media/data-factory-use-custom-activities/DownloadLogsFromCustomActivity.png
