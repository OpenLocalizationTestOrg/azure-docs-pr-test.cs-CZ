---
title: "aaaProcess rozsáhlých datových sad, pomocí služby Data Factory a Batch | Microsoft Docs"
description: "Popisuje, jak kanálu tooprocess obrovské objemy dat v objektu pro vytváření dat Azure pomocí paralelní zpracování funkce Azure Batch."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 688b964b-51d0-4faa-91a7-26c7e3150868
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: 6788f02de555d2e9d6588cc990a39043866d7e97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="process-large-scale-datasets-using-data-factory-and-batch"></a>Zpracování rozsáhlých datových sad pomocí služeb Data Factory a Batch
Tento článek popisuje architekturu ukázkové řešení, které přesune a zpracuje rozsáhlých datových sad automatické a naplánované způsobem. Také poskytuje návod začátku do konce tooimplement hello řešení pomocí Azure Data Factory a Azure Batch.

Tento článek je delší než náš typické článek, protože obsahuje návod celé ukázkové řešení. Pokud jste nový tooBatch a objekt pro vytváření dat, můžete další informace o těchto službách a jak pracují společně. Pokud znáte něco o službách hello a jsou návrhu nebo architektury řešení, může soustředit jenom na hello [části architektura](#architecture-of-sample-solution) hello článku a pokud vyvíjíte prototypu nebo řešení, můžete také tootry out podrobné pokyny v hello [návod](#implementation-of-sample-solution). Doporučujeme komentář k tomuto obsahu a jeho použití.

Nejprve podíváme, jak služby Data Factory a Batch vám mohou pomoci s zpracování rozsáhlých datových sad v cloudu hello.     

## <a name="why-azure-batch"></a>Proč Azure Batch?
Azure Batch umožňuje efektivně v cloudu hello aplikace toorun rozsáhlé paralelní a vysoce výkonné výpočty (HPC). Je to služba platformy, která plánuje výpočetně náročných toorun ve spravované kolekci virtuálních počítačů, a dokáže automaticky škálovat výpočetní prostředky toomeet hello potřeby vašich úloh.

S hello služby Batch definujete výpočetní prostředky tooexecute aplikace paralelně a škálovaně. Můžete spustit na vyžádání nebo naplánované úlohy a není nutné toomanually vytvářet, konfigurovat a spravovat HPC cluster, jednotlivé virtuální počítače, virtuální sítě nebo komplexních úloh a úloh plánování infrastruktury.

V tématu hello následující články, pokud nejste obeznámeni s Azure Batch, protože pomáhá při pochopení hello architektura a implementace řešení hello popsané v tomto článku.   

* [Základy služby Azure Batch](../batch/batch-technical-overview.md)
* [Přehled funkcí Batch](../batch/batch-api-basics.md)

(volitelné) toolearn Další informace o Azure Batch, najdete v části hello [studijní Azure Batch](https://azure.microsoft.com/documentation/learning-paths/batch/).

## <a name="why-azure-data-factory"></a>Proč pro vytváření dat Azure?
Objekt pro vytváření dat je služba pro integraci dat založené na cloudu, která orchestruje a automatizuje hello přesouvání a transformaci dat. Pomocí služby Data Factory hello, můžete vytvořit spravované datové kanály, které přesun dat z místní a cloudové úložiště dat úložiště tooa centralizované data (například: Azure Blob Storage) a proces nebo transformace dat pomocí služeb, jako je například Azure HDInsight a Azure Machine Learning. Můžete také plánovat datové kanály toorun naplánované způsobem (hodinové, denní, týdenní, atd.) a monitorování a spravovat na problémy první pohled tooidentify a provést akci.

V tématu hello následující články, pokud nejste obeznámeni s Azure Data Factory, protože pomáhá při pochopení hello architektura a implementace řešení hello popsané v tomto článku.  

* [Úvod objektu pro vytváření dat Azure](data-factory-introduction.md)
* [Sestavit svůj první kanál dat](data-factory-build-your-first-pipeline.md)   

(volitelné) toolearn Další informace o Azure Data Factory najdete v části hello [studijní postup Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/).

## <a name="data-factory-and-batch-together"></a>Objekt pro vytváření dat a společně Batch
Objekt pro vytváření dat obsahuje zabudované aktivity, jako např. aktivity kopírování toocopy nebo přesunout data ze zdrojových dat úložiště tooa cílového úložiště dat a dat tooprocess Hive aktivity pomocí clusterů systému Hadoop (HDInsight) v Azure. V tématu [aktivit transformace dat](data-factory-data-transformation-activities.md) seznam aktivit transformace podporované.

Také umožňuje toomove nebo proces data s vlastní logikou jste toocreate vlastní aktivity rozhraní .NET a spusťte tyto aktivity v clusteru Azure HDInsight nebo na fondu Azure Batch virtuálních počítačů. Při použití služby Azure Batch, můžete nakonfigurovat fond hello tooauto škálování (Přidat nebo odebrat virtuální počítače založené na pracovním vytížení hello) podle vzorce zadáte.     

## <a name="architecture-of-sample-solution"></a>Architektura ukázkové řešení
I když hello architektury popsané v tomto článku je pro jednoduchým řešením, je důležité toocomplex scénáře, jako je riziko modelování finančních služeb, zpracování obrázků a vykreslování a Genomické analysis.

Hello diagram znázorňuje, 1) jak Data Factory orchestruje přesun dat a zpracování a 2) jak Azure Batch zpracovává hello data paralelní způsobem. Stažení a tisku hello diagram referenční (11 × 17 palců. nebo velikost A3): [orchestration HPC a data pomocí Azure Batch a objektu pro vytváření dat](http://go.microsoft.com/fwlink/?LinkId=717686).

[![Diagram zpracování velkých dat](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)

Hello následující seznam uvádí základní kroky procesu hello hello. Hello řešení zahrnuje kód a vysvětlení toobuild hello začátku do konce řešení.

1. **Nakonfigurovat fond výpočetních uzlů (VM) Azure Batch**. Můžete zadat hello počet uzlů a velikost každého uzlu.
2. **Vytvoření instance služby Azure Data Factory** nakonfigurovaný s entitami, které představují Azure blob storage, výpočetní služby Azure Batch, vstupní a výstupní data a pracovní postup nebo kanálu s aktivitami, které přesunout a transformovat data.
3. **Vytvořit vlastní .NET aktivitu v kanálu pro vytváření dat hello**. Aktivita Hello je uživatelského kódu, který běží na hello fondu Azure Batch.
4. **Ukládat velké množství vstupních dat jako objekty BLOB v úložišti Azure**. Data je rozdělena do logické řezy (obvykle čas).
5. **Objekt pro vytváření dat zkopíruje data, která zpracovává paralelně** toohello sekundárního umístění.
6. **Objekt pro vytváření dat běží hello vlastní aktivitu pomocí hello fondu přidělené dávkou**. Objekt pro vytváření dat mohou být aktivity současně. Každá aktivita zpracovává řez data. výsledky Hello jsou uloženy v úložišti Azure.
7. **Objekt pro vytváření dat přesune hello konečných výsledků tooa třetí umístění**, k distribuci prostřednictvím aplikace nebo pro další zpracování jiných nástrojů.

## <a name="implementation-of-sample-solution"></a>Implementace ukázkové řešení
Hello ukázkové řešení je záměrně jednoduchá a tooshow můžete jak toouse objekt pro vytváření dat a Batch společně tooprocess datové sady. řešení Hello jednoduše vrátí hello počet výskytů hledaný termín ("Microsoft") v vstupní soubory, které jsou uspořádány do časové řady. Vyprodukuje hello počet toooutput soubory.

**Čas**: Pokud jste obeznámení se základy používání služby Azure Data Factory a Batch, a mít dokončené hello požadavky uvedené níže, jsme odhadnout toto řešení trvá toocomplete 1 – 2 hodiny.

### <a name="prerequisites"></a>Požadavky
#### <a name="azure-subscription"></a>Předplatné Azure
Pokud nemáte předplatné Azure, můžete si během několika minut vytvořit Bezplatný zkušební účet. V tématu [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="azure-storage-account"></a>Účet služby Azure Storage
Používáte účet úložiště Azure pro ukládání dat hello v tomto kurzu. Pokud nemáte účet úložiště Azure, najdete v části [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account). Hello ukázkové řešení používá úložiště objektů blob.

#### <a name="azure-batch-account"></a>Účet Azure Batch
Vytvoření účtu Azure Batch pomocí hello [portál Azure](http://manage.windowsazure.com/). V tématu [vytvoření a Správa účtu Azure Batch](../batch/batch-account-create-portal.md). Všimněte si hello Azure Batch účet názvu a klíče účtu. Můžete také použít [New-AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) rutiny toocreate účtu Azure Batch. V tématu [Začínáme pomocí rutin prostředí PowerShell Azure Batch](../batch/batch-powershell-cmdlets-get-started.md) pro podrobné pokyny k použití této rutiny.

Hello ukázkové řešení používá Azure Batch (nepřímo přes kanál služby Azure Data Factory) tooprocess data paralelní způsobem ve fondu výpočetních uzlů (spravované kolekce virtuálních počítačů).

#### <a name="azure-batch-pool-of-virtual-machines-vms"></a>Azure Batch fondu virtuálních počítačů (VM)
Vytvoření **fondu Azure Batch** s minimálně 2 výpočetních uzlů.

1. V hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **Procházet** v levé nabídce text hello a klikněte na tlačítko **účty Batch**.
2. Vyberte vaše hello tooopen účet Azure Batch **účtu Batch** okno.
3. Klikněte na tlačítko **fondy** dlaždici.
4. V hello **fondy** okně klikněte na tlačítko Přidat na panelu nástrojů tooadd hello fondu.
   1. Zadejte ID fondu hello (**ID fondu**). Poznámka: hello **ID fondu hello**; je nutné při vytváření řešení Data Factory hello.
   2. Zadejte **Windows Server 2012 R2** pro nastavení hello řada operačního systému.
   3. Vyberte **cenovou úroveň uzlu**.
   4. Zadejte **2** jako hodnota pro hello **cíl vyhrazené** nastavení.
   5. Zadejte **2** jako hodnota pro hello **maximální počet úloh na uzel** nastavení.
   6. Klikněte na tlačítko **OK** toocreate hello fondu.

#### <a name="azure-storage-explorer"></a>Azure Storage Explorer
[Azure Storage Explorer 6 (nástroj)](https://azurestorageexplorer.codeplex.com/) nebo [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (z ClumsyLeaf softwaru). Pomocí těchto nástrojů pro kontroly a změna hello data ve vašich projektů Azure Storage, včetně protokolů hello aplikací hostovaných v cloudu.

1. Vytvořit kontejner s názvem **můj_kontejner** s přístupem k privátní (žádný anonymní přístup)
2. Pokud používáte **CloudXplorer**, vytvořte složky a podsložky hello strukturu:

   ![](./media/data-factory-data-processing-using-batch/image3.png)

   `Inputfolder`a `outputfolder` jsou nejvyšší úrovně složky v `mycontainer`. Hello `inputfolder` má podsložky razítka data a času (rrrr-MM-DD-HH).

   Pokud používáte **Azure Storage Explorer**, musíte v dalším kroku hello tooupload soubory s názvy: `inputfolder/2015-11-16-00/file.txt`, `inputfolder/2015-11-16-01/file.txt` a tak dále. Tento krok automaticky vytvoří hello složek.
3. Vytvořte textový soubor **soubor.txt** na počítači s obsahem, který má hello – klíčové slovo **Microsoft**. Například: "testovací vlastní aktivity Microsoft test vlastní aktivity Microsoft".
4. Nahrajte toohello souboru hello následující vstupní složky v Azure blob storage.

   ![](./media/data-factory-data-processing-using-batch/image4.png)

   Pokud používáte **Azure Storage Explorer**, nahrajte soubor hello **soubor.txt** příliš**můj_kontejner**. Klikněte na tlačítko **kopie** na hello nástrojů toocreate kopii hello objektů blob. V hello **kopírovat objekt Blob** dialogové okno, změna hello **název cílového objektu blob** příliš`inputfolder/2015-11-16-00/file.txt`. Opakujte tento krok toocreate `inputfolder/2015-11-16-01/file.txt`, `inputfolder/2015-11-16-02/file.txt`, `inputfolder/2015-11-16-03/file.txt`, `inputfolder/2015-11-16-04/file.txt` a tak dále. Tato akce automaticky vytvoří hello složek.
5. Vytvořte jiný kontejner s názvem: `customactivitycontainer`. Můžete nahrát hello vlastní aktivita zip souboru toothis kontejneru.

#### <a name="visual-studio"></a>Visual Studio
Nainstalujte sadu Microsoft Visual Studio 2012 nebo novější toocreate hello vlastní Batch aktivity toobe použít v hello řešení Data Factory.

### <a name="high-level-steps-toocreate-hello-solution"></a>Postup vysoké úrovně toocreate hello řešení
1. Vytvořte vlastní aktivity, která obsahuje logiku zpracování dat hello.
2. Vytvoření služby Azure data factory, která používá vlastní aktivity hello:

### <a name="create-hello-custom-activity"></a>Vytvořit vlastní aktivitu hello
Hello vlastní aktivity služby Data Factory je hello srdcem této ukázkové řešení. Hello ukázkové řešení používá Azure Batch toorun hello vlastní aktivity. V tématu [použít vlastní aktivity v kanálu Azure Data Factory](data-factory-use-custom-activities.md) pro vlastní aktivity toodevelop hello základní informace a použít je v Azure Data Factory kanálů.

toocreate .NET vlastní aktivity, které můžete použít v kanál služby Azure Data Factory, budete potřebovat toocreate **knihovna tříd rozhraní .NET** projektu s třídou, která implementuje **IDotNetActivity** rozhraní. Toto rozhraní obsahuje pouze jednu metodu: **Execute**. Tady je hello podpis metody hello:

```csharp
public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)
```

Hello metoda má několik klíčových součástí, je nutné, aby toounderstand.

* Metoda Hello přijímá čtyř parametrů:

  1. **linkedServices**. Vyčíslitelná seznam propojené služby, které odkazují vstupní a výstupní datové zdroje (například: Azure Blob Storage) toohello data factory. V této ukázce je pouze jeden propojené služby typu úložiště Azure pro vstup a výstup.
  2. **datové sady**. Toto je vyčíslitelná seznam datových sad. Můžete použít tento parametr tooget hello umístění a schémata definované vstupní a výstupní datové sady.
  3. **aktivita**. Tento parametr představuje hello aktuální výpočetní entitu – v takovém případě služby Azure Batch.
  4. **protokolovač**. Umožňuje protokolovacího nástroje Hello tento prostor pro psaní komentářů ladění jako hello "User" protokolu hello kanálu.
* Hello metoda vrátí slovník, který může být vlastní aktivity používané toochain společně v budoucnosti hello. Tato funkce není dosud implementována, takže vrátí prázdný slovník z metody hello.

#### <a name="procedure-create-hello-custom-activity"></a>Postup: Vytvoření vlastní aktivity hello
1. Vytvoření projektu knihovny tříd rozhraní .NET v sadě Visual Studio.

   1. Spusťte **sady Visual Studio 2012**/**2013 nebo 2015**.
   2. Klikněte na tlačítko **soubor**, bod příliš**nový**a klikněte na tlačítko **projektu**.
   3. Rozbalte položku **šablony**a vyberte **Visual C\#**. V tomto návodu můžete použít C\#, ale můžete použít libovolné .NET jazyk toodevelop hello vlastní aktivity.
   4. Vyberte **knihovny tříd** hello seznamu typů projektu na hello správné.
   5. Zadejte **MyDotNetActivity** pro hello **název**.
   6. Vyberte **C:\\ADF** pro hello **umístění**. Vytvořte složku hello **ADF** Pokud neexistuje.
   7. Klikněte na tlačítko **OK** toocreate hello projektu.
2. Klikněte na tlačítko **nástroje**, bod příliš**Správce balíčků NuGet**a klikněte na tlačítko **Konzola správce balíčků**.
3. V hello **Konzola správce balíčků**, spustit následující příkaz tooimport hello **Microsoft.Azure.Management.DataFactories**.

    ```powershell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. Import hello **Azure Storage** balíček NuGet v toohello projektu. Tento balíček je nutné, protože v této ukázce použijete rozhraní API úložiště objektů Blob hello.

    ```powershell
    Install-Package Azure.Storage
    ```
5. Přidejte následující hello **pomocí** direktivy toohello zdrojový soubor v projektu hello.

    ```csharp
    using System.IO;
    using System.Globalization;
    using System.Diagnostics;
    using System.Linq;
    
    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Runtime;
    
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```
6. Změnit název hello hello **obor názvů** příliš**MyDotNetActivityNS**.

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. Změňte název hello třídy hello příliš**MyDotNetActivity** a odvodí z hello **IDotNetActivity** rozhraní, jak je uvedeno níže.

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. Implementace (Přidat) hello **Execute** metoda hello **IDotNetActivity** rozhraní toohello **MyDotNetActivity** hello třídy a zkopírujte následující ukázka kódu toohello metoda. V tématu hello [spustit metodu](#execute-method) části vysvětlení pro hello logikou používanou v této metodě.

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
    
       // declare types for input and output data stores
       AzureStorageLinkedService inputLinkedService;
    
       Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
       foreach (LinkedService ls in linkedServices)
           logger.Write("linkedService.Name {0}", ls.Name);
    
       // using First method instead of Single since we are using hello same
       // Azure Storage linked service for input and output.
       inputLinkedService = linkedServices.First(
           linkedService =>
           linkedService.Name ==
           inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
           as AzureStorageLinkedService;
    
       string connectionString = inputLinkedService.ConnectionString; // toocreate an input storage client.
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
           // with hello data slice.
           //
           // definition of hello method is shown in hello next step.
           output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
       } while (continuationToken != null);
    
       // get hello output dataset using hello name of hello dataset matched tooa name in hello Activity output collection.
       Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    
       folderPath = GetFolderPath(outputDataset);
    
       logger.Write("Writing blob toohello folder: {0}", folderPath);
    
       // create a storage object for hello output blob.
       CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
       // write hello name of hello file.
       Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
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
9. Přidejte následující pomocná třída toohello metody hello. Tyto metody jsou vyvolány hello **Execute** metoda. Co je nejdůležitější – hello **Calculate** metoda izoluje hello kód, který iteruje v rámci jednotlivých objektů blob.

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
    
       AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
       if (blobDataset == null)
       {
           return null;
       }
    
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
    
       AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
       if (blobDataset == null)
       {
           return null;
       }
    
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
    Hello **GetFolderPath** metoda vrátí hello cesta toohello složky této hello datovou sadu body tooand hello **GetFileName** metoda vrátí hello název hello objektů blob nebo souboru, který hello body datovou sadu.

    ```csharp

    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
    ```

    Hello **Calculate** metoda vypočítá hello počet instancí – klíčové slovo **Microsoft** v hello vstupní soubory (objekty BLOB ve složce hello). hledaný termín Hello ("Microsoft") je pevně zakódovaná v kódu hello.

1. Kompilace projektu hello. Klikněte na tlačítko **sestavení** hello nabídky a klikněte na **sestavit řešení**.
2. Spusťte **Průzkumníka Windows**a přejděte příliš**bin\\ladění** nebo **bin\\verze** složku v závislosti na typu hello sestavení.
3. Vytvořte soubor zip **MyDotNetActivity.zip** obsahující všechny binární soubory hello v hello  **\\bin\\ladění** složky. Můžete chtít tooinclude hello MyDotNetActivity. **pdb** tak, aby získat další podrobnosti, jako je například číslo řádku v hello zdrojový kód, který způsobil hello problém, když dojde k chybě.

   ![](./media/data-factory-data-processing-using-batch/image5.png)
4. Nahrát **MyDotNetActivity.zip** jako kontejner objektů blob toohello objektů blob: `customactivitycontainer` v hello Azure blob storage této hello **StorageLinkedService** propojená služba v hello  **ADFTutorialDataFactory** používá. Vytvoření kontejneru objektů blob hello `customactivitycontainer` Pokud ještě neexistuje.

#### <a name="execute-method"></a>Execute – metoda
Tato část obsahuje další podrobnosti a poznámky o hello kódu v hello Metoda Execute.

1. Členové Hello iterace v rámci hello vstupní kolekce se nacházejí v hello [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) oboru názvů. Iterace v rámci kolekce objektů blob hello vyžaduje použití hello **BlobContinuationToken** třídy. V podstatě, je nutné použít DNT-při smyčky pomocí tokenu hello hello mechanismus pro ukončení smyčky hello. Další informace najdete v tématu [jak toouse úložiště Blob z .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Zobrazí se zde základní smyčka:

    ```csharp
    // Initialize hello continuation token.
    BlobContinuationToken continuationToken = null;
    do
    {
    // Get hello list of input blobs from hello input storage client object.
    BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
    
                         true,
                                   BlobListingDetails.Metadata,
                                   null,
                                   continuationToken,
                                   null,
                                   null);
    // Return a string derived from parsing each blob.
    
     output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
    } while (continuationToken != null);

    ```
   Naleznete v dokumentaci k hello hello [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) metoda podrobnosti.
2. Hello kód pro práci prostřednictvím hello sadu objektů BLOB logicky přejde v rámci hello udělat-při smyčky. V hello **Execute** metoda, proveďte hello-při smyčky předá hello seznam objektů BLOB s názvem metoda tooa **Calculate**. Hello metoda vrátí řetězec proměnné s názvem **výstup** tedy hello výsledek s vstupní prostřednictvím všech objektů BLOB hello v segmentu hello.

   Vrátí hello počtu výskytů hello hledaný termín (**Microsoft**) v objektu blob hello předán toohello **Calculate** metoda.

    ```csharp
    output += string.Format("{0} occurrences of hello search term \"{1}\" were found in hello file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
    ```
3. Jednou hello **Calculate** metoda provedla pracovní hello, se musí být napsané tooa nový objekt blob. Aby pro každou sadu objektů BLOB zpracovat nový objekt blob může být napsán s výsledky hello. Nový objekt blob tooa toowrite, první najít hello výstupní datovou sadu.

    ```csharp
    // Get hello output dataset using hello name of hello dataset matched tooa name in hello Activity output collection.
    Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    ```
4. Hello kód také volá metodu helper: **GetFolderPath** tooretrieve cesta ke složce hello (název kontejneru úložiště hello).

    ```csharp
    folderPath = GetFolderPath(outputDataset);
    ```
   Hello **GetFolderPath** přetypování hello datovou sadu objektu tooan AzureBlobDataSet, který má vlastnost s názvem FolderPath.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FolderPath;
    ```
5. Hello kód volání hello **GetFileName** metoda tooretrieve hello souboru name (název objektu blob). Kód Hello je podobné toohello výše cesta ke složce hello tooget kódu.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FileName;
    ```
6. Název Hello hello souboru je zapsán vytvořením objektu URI. Konstruktor URI Hello používá hello **BlobEndpoint** název kontejneru hello tooreturn vlastnost. název a cesta k souboru Hello složky se přidají identifikátor URI výstupního objektu blob tooconstruct hello.  

    ```csharp
    // Write hello name of hello file.
    Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    ```
7. byl proveden zápis Hello název souboru hello a nyní může zapisovat hello výstupní řetězec z hello **Calculate** metoda tooa nové blob:

    ```csharp
    // Create a blob and upload hello output text.
    CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
    logger.Write("Writing {0} toohello output blob", output);
    outputBlob.UploadText(output);
    ```

### <a name="create-hello-data-factory"></a>Vytvoření objektu pro vytváření dat hello
V hello [vytvořit vlastní aktivitu hello](#create-the-custom-activity) části jste vytvořili vlastní aktivity a soubor zip nahrané hello s binární soubory a hello PDB souboru tooan kontejner objektů blob v Azure. V této části vytvoříte Azure **objekt pro vytváření dat** s **kanálu** používající hello **vlastní aktivity**.

pro vlastní aktivity hello představuje hello objekty BLOB (soubory) ve složce vstupní hello Hello vstupní datové sady (`mycontainer\\inputfolder`) v úložišti objektů blob. Hello výstupní datovou sadu pro aktivity hello představuje objekty BLOB výstup hello v hello výstupní složce (`mycontainer\\outputfolder`) v úložišti objektů blob.

Vyřaďte jeden nebo více souborů ve složkách vstupní hello:

```
mycontainer -\> inputfolder
    2015-11-16-00
    2015-11-16-01
    2015-11-16-02
    2015-11-16-03
    2015-11-16-04
```

Například můžete vyřaďte jeden soubor (soubor.txt) s hello následující obsah do jednotlivých složek hello.

```
test custom activity Microsoft test custom activity Microsoft
```

Každé vstupní složky odpovídá tooa řez v Azure Data Factory i v případě, že složka hello obsahuje 2 nebo více souborů. Při zpracování kanálu hello se každý řez iteruje hello vlastní aktivity všech objektů BLOB hello v hello vstupní složky pro tento řez.

Uvidíte pět výstupní soubory s hello stejné obsahu. Například soubor výstup hello zpracování hello souboru ve složce hello 2015 11 16 00 má hello následující obsah:

```
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file.txt.
```

Pokud je vyřadit více souborů (soubor.txt, Soubor2.txt, file3.txt) s hello stejné toohello vstupní složky obsahu, najdete v části hello následující obsah ve výstupním souboru hello. Každé složky (2015-11-16-00 atd.) odpovídá tooa řez v této ukázce, i když hello složka obsahuje více vstupní soubory.

```csharp
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file.txt.
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file2.txt.
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file3.txt.
```

Hello výstupní soubor má tři řádky, jeden pro každý vstupní soubor (binární rozsáhlý objekt) ve složce hello přidružené řez hello (2015-11-16-00).

Úloha se vytvoří pro každou aktivitu spustit. V této ukázce je jenom jedna aktivita v kanálu hello. Při zpracování kanálu hello se řez hello vlastní aktivity spouští na Azure Batch tooprocess hello řez. Vzhledem k tomu, že existují pět řezy (každý řez může mít více objektů BLOB nebo soubor), nejsou pět úkoly vytvořené v Azure Batch. Když úloha běží na Batch, je ve skutečnosti hello vlastní aktivity se systémem.

Následující postup Hello poskytuje další podrobnosti.

#### <a name="step-1-create-hello-data-factory"></a>Krok 1: Vytvoření objektu pro vytváření dat hello
1. Po přihlášení toohello [portál Azure](https://portal.azure.com/), hello následující kroky:

   1. Klikněte na tlačítko **nový** v levé nabídce hello.
   2. Klikněte na tlačítko **Data + analýzy** v hello **nový** okno.
   3. Klikněte na tlačítko **Data Factory** na hello **analýzy dat** okno.
2. V hello **nový objekt pro vytváření dat** okno, zadejte **CustomActivityFactory** pro hello název. Hello název objektu pro vytváření dat Azure hello musí být globálně jedinečný. Pokud se zobrazí chyba hello: **název objektu pro vytváření dat "CustomActivityFactory" není k dispozici**, změňte hello název objektu pro vytváření dat hello (například **yournameCustomActivityFactory**) a zkuste vytvořit znovu.
3. Klikněte na tlačítko **název skupiny prostředků**a vyberte existující skupinu prostředků nebo vytvořte skupinu prostředků.
4. Ověřte, že používáte správné předplatné hello a oblast, kam chcete hello data factory toobe vytvořit.
5. Klikněte na tlačítko **vytvořit** na hello **nový objekt pro vytváření dat** okno.
6. Zobrazí hello objekt pro vytváření dat vytváří v hello **řídicí panel** z hello portálu Azure.
7. Po úspěšném vytvoření objektu pro vytváření dat hello, se zobrazí stránka objektu pro vytváření dat hello, se zobrazí hello obsah objektu pro vytváření dat hello.

   ![](./media/data-factory-data-processing-using-batch/image6.png)

#### <a name="step-2-create-linked-services"></a>Krok 2: Vytvoření propojených služeb
Propojené služby propojují úložiště dat nebo výpočetní služby tooan pro vytváření dat Azure. V tomto kroku propojíte vaše **Azure Storage** účtu a **Azure Batch** objekt pro vytváření dat účet tooyour.

#### <a name="create-azure-storage-linked-service"></a>Vytvoření propojené služby Azure Storage
1. Klikněte na tlačítko hello **vytvořit a nasadit** na hello dlaždici **DATA FACTORY** okno pro **CustomActivityFactory**. Zobrazí hello editoru služby Data Factory.
2. Klikněte na tlačítko **nové datové úložiště** na panelu příkazů hello a zvolte **úložiště Azure.** Měli byste vidět hello skript JSON pro vytvoření Azure Storage propojená služba v editoru hello.

   ![](./media/data-factory-data-processing-using-batch/image7.png)

3. Nahraďte **název účtu** hello název účtu úložiště Azure a **klíč účtu** s přístupovým klíčem hello hello účtu úložiště Azure. toolearn tooget úložiště přístupu klíče najdete v tématu [zobrazení, kopírování a opětovné vytváření přístupových klíčů úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-account).

4. Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello propojené služby.

   ![](./media/data-factory-data-processing-using-batch/image8.png)

#### <a name="create-azure-batch-linked-service"></a>Vytvoření služby Azure Batch propojené
V tomto kroku vytvoříte propojené služby pro vaše **Azure Batch** účtu, který je použité toorun hello objekt pro vytváření dat vlastní aktivity.

1. Klikněte na tlačítko **nový výpočet** na panelu příkazů hello a zvolte **Azure Batch.** Měli byste vidět hello skript JSON pro vytvoření Azure Batch propojená služba v editoru hello.
2. V hello skript JSON:

   1. Nahraďte **název účtu** s názvem hello účtu Azure Batch.
   2. Nahraďte **přístupový klíč** s přístupovým klíčem hello hello účtu Azure Batch.
   3. Zadejte ID hello hello fondu pro hello **poolName** vlastnost**.** Pro tuto vlastnost lze zadat buď název fondu nebo fondu ID.
   4. Zadejte hello batch identifikátor URI pro hello **batchUri** vlastnost JSON.

      > [!IMPORTANT]
      > Hello **URL** z hello **okně účtu Azure Batch** je ve formátu hello: \<accountname\>.\< oblast\>. batch.azure.com. Pro hello **batchUri** vlastnost hello JSON, budete potřebovat příliš**odebrat "název účtu."** z adresy URL hello. Příklad: `"batchUri": "https://eastus.batch.azure.com"`.
      >
      >

      ![](./media/data-factory-data-processing-using-batch/image9.png)

      Pro hello **poolName** vlastnost, můžete také zadat hello ID fondu hello místo názvu hello hello fondu.

      > [!NOTE]
      > Hello služba Data Factory nepodporuje možnost na vyžádání pro Azure Batch, stejně jako pro HDInsight. Vlastní fondu Azure Batch můžete použít pouze v objektu pro vytváření dat Azure.
      >
      >
   5. Zadejte **StorageLinkedService** pro hello **linkedServiceName** vlastnost. Tuto propojenou službu jste vytvořili v předchozím kroku hello. Toto úložiště se používá jako pracovní oblast pro soubory a protokoly.
3. Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello propojené služby.

#### <a name="step-3-create-datasets"></a>Krok 3: Vytvoření datové sady
V tomto kroku vytvoříte datové sady toorepresent vstupní a výstupní data.

#### <a name="create-input-dataset"></a>Vytvoření vstupní datové sady
1. V hello **Editor** hello objekt pro vytváření dat, klikněte na tlačítko **nová datová sada** na panelu nástrojů hello a klikněte na tlačítko **úložiště objektů Azure Blob** z rozevírací nabídky hello.
2. Nahraďte hello JSON v pravém podokně hello hello následujícím fragmentu kódu JSON:

    ```json
    {
       "name": "InputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
               "format": {
                   "type": "TextFormat"
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
           },
           "external": true,
           "policy": {}
       }
    }
    ```

    Vytvoření kanálu dále v tomto návodu se časem spuštění: 2015-11-16T00:00:00Z a koncový čas: 2015-11-16T05:00:00Z. Je naplánované tooproduce data **každou hodinu**, takže se 5 řezy vstupní a výstupní (mezi **00**: 00:00 -\> **05**: 00:00).

    Hello **frekvence** a **interval** hello vstupní datové sady je nastavený příliš**hodinu** a **1**, což znamená, že hello vstupní řez je k dispozici každou hodinu.

    Tady jsou hello času zahájení pro každý řez, která je reprezentována **SliceStart** systémové proměnné v hello výše fragmentu kódu JSON.

    | **Řez** | **Čas spuštění**          |
    |-----------|-------------------------|
    | 1         | 2015. 11 16T**00**: 00:00 |
    | 2         | 2015. 11 16T**01**: 00:00 |
    | 3         | 2015. 11 16T**02**: 00:00 |
    | 4         | 2015. 11 16T**03**: 00:00 |
    | 5         | 2015. 11 16T**04**: 00:00 |

    Hello **folderPath** se vypočítává pomocí hello rok, měsíc, den a hodina část času spuštění řezu hello (**SliceStart**). Zde je proto jak vstupní složky je namapované tooa řez.

    | **Řez** | **Čas spuštění**          | **Vstupní složky**  |
    |-----------|-------------------------|-------------------|
    | 1         | 2015. 11 16T**00**: 00:00 | 2015-11-16-**00** |
    | 2         | 2015. 11 16T**01**: 00:00 | 2015-11-16-**01** |
    | 3         | 2015. 11 16T**02**: 00:00 | 2015-11-16-**02** |
    | 4         | 2015. 11 16T**03**: 00:00 | 2015-11-16-**03** |
    | 5         | 2015. 11 16T**04**: 00:00 | 2015-11-16-**04** |

1. Klikněte na tlačítko **nasadit** hello toocreate panelu nástrojů a nasadit hello **InputDataset** tabulky.

#### <a name="create-output-dataset"></a>Vytvoření výstupní datové sady
V tomto kroku vytvoříte jinou datovou sadu typu AzureBlob toorepresent hello výstupní data.

1. V hello **Editor** hello objekt pro vytváření dat, klikněte na tlačítko **nová datová sada** na panelu nástrojů hello a klikněte na tlačítko **úložiště objektů Azure Blob** z rozevírací nabídky hello.
2. Nahraďte hello JSON v pravém podokně hello hello následujícím fragmentu kódu JSON:

    ```json
    {
       "name": "OutputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "fileName": "{slice}.txt",
               "folderPath": "mycontainer/outputfolder",
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

    Objekt blob nebo soubor výstupu se generuje pro každý vstupní řez. Zde je, jak je výstupní soubor s názvem pro každý řez. Všechny soubory výstup hello vygenerují na jednu výstupní složky: `mycontainer\\outputfolder`.

    | **Řez** | **Čas spuštění**          | **Výstupní soubor**       |
    |-----------|-------------------------|-----------------------|
    | 1         | 2015. 11 16T**00**: 00:00 | 2015-11-16 -**00. txt** |
    | 2         | 2015. 11 16T**01**: 00:00 | 2015-11-16 -**01. txt** |
    | 3         | 2015. 11 16T**02**: 00:00 | 2015-11-16 -**02. txt** |
    | 4         | 2015. 11 16T**03**: 00:00 | 2015-11-16 -**03. txt** |
    | 5         | 2015. 11 16T**04**: 00:00 | 2015-11-16 -**04. txt** |

    Mějte na paměti, že všechny soubory ve složce aplikace vstupní hello (například: 2015-11-16-00) jsou součástí řez se časem spuštění hello: 2015-11-16-00. Při zpracování této řezu se vlastní aktivity hello prohledává každý soubor a vytvoří řádek v souboru výstup hello s hello počtu výskytů hledaný termín ("Microsoft"). Pokud existují tři soubory ve složce hello 2015 11 16 00, se ve výstupním souboru hello tři řádky: 2015-11-16-00.txt.

1. Klikněte na tlačítko **nasadit** hello toocreate panelu nástrojů a nasadit hello **OutputDataset**.

#### <a name="step-4-create-and-run-hello-pipeline-with-custom-activity"></a>Krok 4: Vytvoření a spuštění kanálu hello s vlastní aktivity
V tomto kroku vytvoříte kanál s aktivitou jeden, hello vlastní aktivity, které jste vytvořili dříve.

> [!IMPORTANT]
> Pokud ještě jste neodeslali hello **soubor.txt** tooinput složek v kontejneru objektu blob hello tak učinit před vytvořením kanálu hello. Hello **isPaused** vlastnost nastavena toofalse v kanálu hello formát JSON, takže hello kanál okamžitě spustí jako hello **spustit** datum je v minulosti hello.
>
>

1. V editoru služby Data Factory hello, klikněte na **nový kanál** na panelu příkazů hello. Pokud se příkaz hello nezobrazí, klikněte na tlačítko **... (Tři tečky)**  toosee ho.
2. Nahraďte hello JSON v pravém podokně hello hello následující skript JSON:

    ```json
    {
       "name": "PipelineCustom",
       "properties": {
           "description": "Use custom activity",
           "activities": [
               {
                   "type": "DotNetActivity",
                   "typeProperties": {
                       "assemblyName": "MyDotNetActivity.dll",
                       "entryPoint": "MyDotNetActivityNS.MyDotNetActivity",
                       "packageLinkedService": "AzureStorageLinkedService",
                       "packageFile": "customactivitycontainer/MyDotNetActivity.zip"
                   },
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
                   "policy": {
                       "timeout": "00:30:00",
                       "concurrency": 5,
                       "retry": 3
                   },
                   "scheduler": {
                       "frequency": "Hour",
                       "interval": 1
                   },
                   "name": "MyDotNetActivity",
                   "linkedServiceName": "AzureBatchLinkedService"
               }
           ],
           "start": "2015-11-16T00:00:00Z",
           "end": "2015-11-16T05:00:00Z",
           "isPaused": false
      }
    }
    ```
   Všimněte si hello následující body:

   * Je jenom jedna aktivita v kanálu hello a který je typu: **DotNetActivity**.
   * **AssemblyName** nastavena toohello název hello knihovny DLL: **MyDotNetActivity.dll**.
   * **EntryPoint** je nastaven příliš**MyDotNetActivityNS.MyDotNetActivity**. Je v podstatě \<obor názvů\>.\< Název třídy\> ve vašem kódu.
   * **PackageLinkedService** je nastaven příliš**StorageLinkedService** který odkazuje toohello úložiště objektů blob, který obsahuje soubor zip hello vlastní aktivity. Pokud používáte jiné účty Azure Storage pro vstupní a výstupní soubory a soubor zip hello vlastní aktivitu, máte toocreate jiné Azure Storage propojené služby. Tento článek předpokládá, že používáte hello stejný účet úložiště Azure.
   * **PackageFile** je nastaven příliš**customactivitycontainer/MyDotNetActivity.zip**. Je ve formátu hello: \<containerforthezip\>/\<nameofthezip.zip\>.
   * vlastní aktivita Hello přijímá **InputDataset** jako vstup a **OutputDataset** jako výstup.
   * Hello **linkedServiceName** vlastnost vlastní aktivity hello ukazuje toohello **AzureBatchLinkedService**, která informuje Azure Data Factory hello vlastní aktivity musí toorun na Azure Batch.
   * Hello **souběžnosti** nastavení je důležité. Pokud používáte hello výchozí hodnotu, která je 1, i v případě, že máte 2 nebo více výpočetních uzlů ve fondu Azure Batch hello, hello řezy jsou zpracovávány jeden po druhém. Proto nejsou využívat výhod hello paralelní zpracování funkce Azure Batch. Pokud nastavíte **souběžnosti** tooa vyšší hodnota, například 2, znamená to, že dva řezy (odpovídá tootwo úlohy v Azure Batch) může zpracovat hello stejný čas, v takovém případě oba hello virtuální počítače v Azure Batch jsou využité fondu hello. Proto správně nastavte vlastnost souběžnosti hello.
   * Pouze jednu úlohu (řez) je spustit na virtuálním počítači v libovolném bodě ve výchozím nastavení. Hello důvod předpokládají, že ve výchozím nastavení, hello **maximální počet úloh na virtuální počítač** nastavena too1 fondu Azure Batch. V rámci požadavků, jste vytvořili fond s too2 sadu tuto vlastnost, tak dva řezy objekt pro vytváření dat může mít spuštěný na virtuálním počítači na hello stejnou dobu.

    -   **isPaused** vlastnost je ve výchozím nastavení toofalse. kanál Hello spustí hned v tomto příkladu, protože hello řezy spustit v posledních hello. Můžete nastavit tuto vlastnost tootrue toopause hello kanálu a nastavte ji zpět toofalse toorestart.

    -   Hello **spustit** čas a **end** časy jsou od sebe pět hodin a řezy vytváří každou hodinu, takže pět řezy vytváří hello kanálu.

1. Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello kanálu.

#### <a name="step-5-test-hello-pipeline"></a>Krok 5: Testování hello kanálu
V tomto kroku otestovat hello kanálu přetažením souborů do složky vstupní hello. Začneme s testování kanálu hello jeden soubor pro jeden vstupní složky.

1. V okně Data Factory hello v hello portálu Azure, klikněte na tlačítko **Diagram**.

   ![](./media/data-factory-data-processing-using-batch/image10.png)
2. V zobrazení diagramu hello, klikněte dvakrát na vstupní datové sady: **InputDataset**.

   ![](./media/data-factory-data-processing-using-batch/image11.png)
3. Měli byste vidět hello **InputDataset** okno s pěti všechny řezy připraven. Všimněte si hello **čas spuštění ŘEZU** a **ŘEZ KONCOVÝ čas** pro každý řez.

   ![](./media/data-factory-data-processing-using-batch/image12.png)
4. V hello **zobrazení diagramu**, klikněte na tlačítko **OutputDataset**.
5. Měli byste vidět, že hello pět Výstup řezy jsou ve stavu Připraveno hello, pokud již byl vytvořen.

   ![](./media/data-factory-data-processing-using-batch/image13.png)
6. Použití portálu Azure tooview hello **úlohy** přidružené hello **řezy** a zjistit, jaké virtuálního počítače byla spuštěna na každý řez. V tématu [integraci služby Data Factory a Batch](#data-factory-and-batch-integration) podrobnosti.
7. Měli byste vidět hello výstupní soubory v hello `outputfolder` z `mycontainer` ve vaší službě Azure blob storage.

   ![](./media/data-factory-data-processing-using-batch/image15.png)

   Měli byste vidět pět výstupní soubory, jeden pro každý vstupní řez. Každý z hello výstupu soubor by měl mít obsahu podobné toohello následující výstup:

    ```
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file.txt.
    ```
   Hello následující diagram znázorňuje, jak hello Data Factory řezy mapování tootasks ve službě Azure Batch. V tomto příkladu má řez spustit pouze jeden.

   ![](./media/data-factory-data-processing-using-batch/image16.png)
8. Nyní zkuste to prosím ještě s více soubory ve složce. Vytvoření souborů: **Soubor2.txt**, **file3.txt**, **file4.txt**, a **file5.txt** s hello stejný obsah jako soubor.txt ve složce hello: **2015-11-06-01**.
9. Ve složce výstup hello **odstranit** hello výstupního souboru: **2015 11 16 01.txt**.
10. Nyní v hello **OutputDataset** okno, klikněte pravým tlačítkem na hello řez s **čas spuštění ŘEZU** nastavit příliš**11/16/2015 01:00:00 AM**a klikněte na tlačítko **spustit**hello toorerun nebo zpětný-process řez. Řez hello teď má pět souborů místo jeden soubor.

    ![](./media/data-factory-data-processing-using-batch/image17.png)
11. Po spuštění řezu hello a její stav je **připraven**, ověřte obsah hello hello výstupního souboru pro tento řez (**2015 11 16 01.txt**) v hello `outputfolder` z `mycontainer` ve službě blob storage. Měla by existovat řádek pro každý soubor hello řez.

    ```
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file2.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file3.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file4.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file5.txt.
    ```

> [!NOTE]
> Pokud jste před dalším pokusem s pěti vstupní soubory neodstranila 2015 modulu hello výstupní soubor-11-16-01.txt, zobrazí jeden řádek z předchozí řez hello spustit a pěti řádcích z hello spuštění aktuálního řezu. Ve výchozím nastavení je hello obsah připojením toooutput soubor, pokud již existuje.
>
>

#### <a name="data-factory-and-batch-integration"></a>Integrace služby Data Factory a Batch
Hello služba Data Factory vytvoří úlohu ve službě Azure Batch s názvem hello: `adf-poolname:job-xxx`.

![Azure Data Factory - úlohy Batch](media/data-factory-data-processing-using-batch/data-factory-batch-jobs.png)

Úloha v úloze hello se vytvoří při každém spuštění aktivity řezu. Pokud existují 10 toobe připraven řezy, které se zpracovat, vytvoření 10 úkolů v úloze hello. Můžete mít více než jeden řez spouštět současně, pokud máte několika výpočetních uzlech ve fondu hello. Pokud hello maximální úkolů na výpočetní uzel je nastaven příliš > 1, může existovat více než jeden řezu systémem hello stejné výpočty.

V tomto příkladu jsou pět řezů, takže pět úlohy v Azure Batch. S hello **souběžnosti** nastavit příliš**5** v hello kanálu JSON v Azure Data Factory a **maximální počet úloh na virtuální počítač** nastavit příliš**2** ve službě Azure Batch fond s **2** virtuálních počítačů, hello úlohy spustí rychlou (zkontrolujte, zda počáteční a koncový čas pro úlohy).

Použít hello portálu tooview hello dávkové úlohy a její úkoly, které jsou přidruženy hello **řezy** a zjistit, jaké virtuálního počítače byla spuštěna na každý řez.

![Azure Data Factory - úlohy Batch](media/data-factory-data-processing-using-batch/data-factory-batch-job-tasks.png)

### <a name="debug-hello-pipeline"></a>Ladění hello kanálu
Ladění zahrnuje několik základních technik:

1. Pokud vstupní řez hello není nastaven příliš**připraven**, potvrďte, že vstupní složky struktura hello je správná a soubor.txt existuje ve vstupní složkách hello.

   ![](./media/data-factory-data-processing-using-batch/image3.png)
2. V hello **Execute** metoda vlastní aktivity, použijte hello **IActivityLogger** toolog informace o objektu, který vám pomůže vyřešit problémy. zprávy Hello přihlášení zobrazí v hello uživatele\_souboru protokolu 0.

   V hello **OutputDataset** okně klikněte na tlačítko hello řez toosee hello **datový ŘEZ** okno pro tento řez. Zobrazí **běh aktivit** pro tento řez. Měli byste vidět jednu aktivitu spustit pro řez hello. Pokud kliknete na tlačítko **spustit** v řádku nabídek hello můžete spustit jinou aktivitu spustit pro hello stejné řez.

   Po kliknutí na tlačítko hello aktivity při spuštění, se zobrazí hello **podrobnosti o spuštění aktivit** okno se seznamem souborů protokolu. Zobrazí zprávy zaznamenané v hello **uživatele\_0 protokolu** souboru. Pokud dojde k chybě, uvidíte tři běh aktivit protože hello počet opakování je nastavena too3 v kanálu nebo aktivity hello JSON. Po kliknutí na tlačítko hello aktivity při spuštění, zobrazí hello soubory protokolů, abyste si prošli tootroubleshoot hello chyby.

   ![](./media/data-factory-data-processing-using-batch/image18.png)

   V seznamu hello protokolových souborů, klikněte na tlačítko hello **uživatele 0.log**. V pravém panelu hello jsou výsledky hello pomocí hello **IActivityLogger.Write** metoda.

   ![](./media/data-factory-data-processing-using-batch/image19.png)

   Kontrola systému 0.log pro všechny chybové zprávy systému a výjimky.

    ```
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Loading assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Creating an instance of MyDotNetActivityNS.MyDotNetActivity from assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Executing Module
    
    Trace\_T\_D\_12/6/2015 1:43:38 AM\_T\_D\_\_T\_D\_Information\_T\_D\_0\_T\_D\_Activity e3817da0-d843-4c5c-85c6-40ba7424dce2 finished successfully
    ```
3. Zahrnout hello **PDB** souborů v souboru zip hello tak, aby podrobnosti o chybě hello informace, jako **zásobník volání** když dojde k chybě.
4. Všechny soubory v souboru zip hello hello hello vlastní aktivity musí být v hello **top úroveň** s ne v podsložkách.

   ![](./media/data-factory-data-processing-using-batch/image20.png)
5. Ujistěte se, že hello **assemblyName** (MyDotNetActivity.dll), **entryPoint** (MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer / MyDotNetActivity.zip), a **packageLinkedService** (měli toohello bodu Azure úložiště objektů blob obsahující soubor zip hello) jsou nastavené toocorrect hodnoty.
6. Pokud můžete opravit chyby a chcete řez hello tooreprocess, klikněte pravým tlačítkem na hello řez ve hello **OutputDataset** a klikněte na **spustit**.

   ![](./media/data-factory-data-processing-using-batch/image21.png)

   > [!NOTE]
   > Zobrazí **kontejneru** ve službě Azure Blob storage s názvem: `adfjobs`. Tento kontejner není automaticky odstraněn, ale můžete ji po dokončení testování řešení hello bezpečně odstranit. Podobně hello řešení Data Factory vytvoří Azure Batch **úlohy** s názvem: `adf-\<pool ID/name\>:job-0000000001`. Po dokončení testu hello řešení Pokud chcete, můžete odstranit tuto úlohu.
   >
   >
7. vlastní aktivity Hello nepoužívá hello **app.config** soubor ze svého balíčku. Proto pokud váš kód čte libovolné púřipojovací řetězce z konfiguračního souboru hello, ale nefunguje za běhu. Hello osvědčený postup, při použití Azure Batch je toohold všech tajných klíčů v **Azure KeyVault**, použijte keyvault hello hlavní tooprotect služeb na základě certifikátů a distribuovat fondu Batch tooAzure hello certifikátu. Hello vlastní aktivity .NET pak k dispozici tajné klíče z hello KeyVault za běhu. Toto řešení je obecný a můžete škálovat tooany typ tajný klíč, ne jenom připojovací řetězec.

    Je snazší alternativní řešení (ale není z hlediska): můžete vytvořit **propojená služba Azure SQL** s nastavení připojovacího řetězce, vytvořit datovou sadu, zda text hello používá propojené služby a řetězu hello datovou sadu jako fiktivní vstupní datové sady toohello vlastní aktivity .NET. Můžete pak přístup hello propojené služby připojovací řetězec v kódu hello vlastní aktivity a by bez problémů fungují za běhu.  

#### <a name="extend-hello-sample"></a>Rozšíření ukázka hello
Tato ukázka toolearn můžete rozšířit informace o funkcích Azure Data Factory a Azure Batch. Řezy tooprocess v jinou dobu rozsahu, například hello následující kroky:

1. Přidejte následující podsložky v hello hello `inputfolder`: 2015-11-16-05 2015-11-16-06 201-11-16-07, 2011-11-16-08, 2015-11-16-09 a umístěte vstupní soubory v těchto složkách. Změnit hello koncový čas pro kanál hello z `2015-11-16T05:00:00Z` příliš`2015-11-16T10:00:00Z`. V hello **zobrazení diagramu**, dvakrát klikněte na hello **InputDataset**a ověřte, zda text hello vstupní řezy jsou připravené. Klikněte dvakrát na **OuptutDataset** toosee hello stav výstup řezy. Pokud jsou ve stavu Připraveno, zkontrolujte hello výstupní složky pro hello výstupní soubory.
2. Zvýšení nebo snížení hello **souběžnosti** nastavení toounderstand jak ovlivňuje výkon hello řešení, zejména hello zpracování které proběhne Azure Batch. (Viz krok 4: vytvoření a spuštění kanálu hello Další informace o hello **souběžnosti** nastavení.)
3. Vytvoření fondu s vyšší nebo nižší **maximální počet úloh na virtuální počítač**. toouse hello nový fond, které jste vytvořili, hello aktualizace služby Azure Batch propojené v řešení Data Factory hello. (Viz krok 4: vytvoření a spuštění kanálu hello Další informace o hello **maximální počet úloh na virtuální počítač** nastavení.)
4. Vytvoření fondu Azure Batch s **škálování** funkce. Automatické škálování výpočetních uzlů ve fondu Azure Batch je hello dynamické přizpůsobení výpočetní výkon, které používá vaše aplikace. 

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
5. V řešení ukázka hello hello **Execute** metoda vyvolá hello **Calculate** metoda, která zpracuje vstupní data řez tooproduce řez výstupních dat. Můžete napsat vlastní metoda tooprocess vstupní data a nahraďte metodu tooyour volání volání metody Calculate hello v hello Metoda Execute.

### <a name="next-steps-consume-hello-data"></a>Další kroky: využívají hello data
Po při zpracování dat, budete moct pracovat s online nástroje, například **Microsoft Power BI**. Tady jsou odkazy toohelp pochopit Power BI a jak toouse ho v Azure:

* [Prozkoumejte datovou sadu v Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-data/)
* [Začínáme s Power BI Desktop hello](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/)
* [Aktualizovat data v Power BI](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/)
* [Azure a Power BI – základní – přehled](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)

## <a name="references"></a>Odkazy
* [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)

  * [Úvod tooAzure služba Data Factory](data-factory-introduction.md)
  * [Začínáme s Azure Data Factory](data-factory-build-your-first-pipeline.md)
  * [Použití vlastních aktivit v kanálu Azure Data Factory](data-factory-use-custom-activities.md)
* [Azure Batch](https://azure.microsoft.com/documentation/services/batch/)

  * [Základy služby Azure Batch](../batch/batch-technical-overview.md)
  * [Přehled funkcí Azure Batch](../batch/batch-api-basics.md)
  * [Vytvoření a Správa účtu Azure Batch na portálu Azure hello](../batch/batch-account-create-portal.md)
  * [Začínáme s Azure Batch Library pro .NET](../batch/batch-dotnet-get-started.md)

[batch-explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch-explorer-walkthrough]: http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx
