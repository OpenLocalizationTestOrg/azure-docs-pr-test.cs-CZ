---
title: "Kurz: Vytvoření kanálu s aktivitou kopírování pomocí rozhraní .NET API | Dokumentace Microsoftu"
description: "V tomto kurzu vytvoříte kanál Azure Data Factory s aktivitou kopírování pomocí rozhraní .NET API."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 58fc4007-b46d-4c8e-a279-cb9e479b3e2b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 52f72da54cdd80691e09d7453bf6730454c4089e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-net-api"></a>Kurz: Vytvoření kanálu s aktivitou kopírování pomocí rozhraní .NET API
> [!div class="op_single_selector"]
> * [Přehled a požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Šablona Azure Resource Manageru](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)

V tomto článku se dozvíte, jak toouse [.NET API](https://portal.azure.com) toocreate objekt pro vytváření dat s kanál, který kopíruje data z databáze Azure SQL tooan úložiště objektů blob v Azure. Pokud jste tooAzure nový objekt pro vytváření dat, pročtěte hello [Úvod tooAzure Data Factory](data-factory-introduction.md) článek před provedením tohoto kurzu.   

V tomto kurzu vytvoříte kanál s jednou aktivitou: aktivita kopírování. Aktivita kopírování Hello zkopíruje data z úložiště dat podporovaných podřízený tooa podporované datové úložiště. Seznam úložišť dat podporovaných jako zdroje a jímky najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Hello aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelné způsobem. Další informace o hello aktivitě kopírování najdete v tématu [aktivity přesunu dat](data-factory-data-movement-activities.md).

Kanál může obsahovat víc než jednu aktivitu. A dvě aktivity (spustit aktivitu po jiné) můžete řetězu nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit. Další informace naleznete, když přejdete na [více aktivit v kanálu](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline). 

> [!NOTE] 
> Úplnou dokumentaci k rozhraní .NET API pro datovou továrnu najdete v [referencích k rozhraní .NET API služby Data Factory](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1).
> 
> Hello datovém kanálu v tomto kurzu kopíruje data ze zdroje dat úložiště tooa cílového úložiště dat. Kurz týkající se jak tootransform dat pomocí Azure Data Factory najdete v části [kurz: sestavení dat tootransform kanálu pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).

## <a name="prerequisites"></a>Požadavky
* Projděte [přehled kurzu a předpoklady](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) tooget přehled kurzu hello a dokončení hello **požadavek** kroky.
* Visual Studio 2012 nebo 2013 nebo 2015
* Stáhněte sadu [Azure .NET SDK](http://azure.microsoft.com/downloads/) a nainstalujte ji.
* Azure Powershell Postupujte podle pokynů v [jak tooinstall a konfigurace prostředí Azure PowerShell](../powershell-install-configure.md) článek tooinstall prostředí Azure PowerShell ve vašem počítači. Používáte prostředí Azure PowerShell toocreate aplikaci Azure Active Directory.

### <a name="create-an-application-in-azure-active-directory"></a>Vytvoření aplikace v Azure Active Directory
Vytvoření aplikace Azure Active Directory, vytvořit objekt služby pro aplikaci hello a přiřaďte ho toohello **Data Factory Přispěvatel** role.

1. Spusťte **PowerShell**.
2. Spusťte následující příkaz hello a zadejte hello uživatelské jméno a heslo použít toosign v toohello portálu Azure.

    ```PowerShell
    Login-AzureRmAccount
    ```
3. Spusťte následující příkaz tooview hello všechny hello předplatná pro tento účet.

    ```PowerShell
    Get-AzureRmSubscription
    ```
4. Spusťte následující příkaz tooselect hello předplatné, které chcete toowork s hello. Nahraďte  **&lt;NameOfAzureSubscription** &gt; s názvem hello předplatného Azure.

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```

   > [!IMPORTANT]
   > Poznamenejte si **SubscriptionId** a **TenantId** z hello výstup tohoto příkazu.

5. Vytvořte skupinu prostředků Azure s názvem **ADFTutorialResourceGroup** tak, že spustíte následující příkaz v hello prostředí PowerShell hello.

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```

    Pokud skupina prostředků hello již existuje, zadejte zda tooupdate ho (Y) nebo jej zachovat jako (ne).

    Pokud používáte jiné skupině prostředků, musíte v tomto kurzu toouse hello název vaší skupiny prostředků místo skupiny ADFTutorialResourceGroup.
6. Vytvořte aplikaci Azure Active Directory.

    ```PowerShell
    $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"
    ```

    Pokud dojde k následující chybě hello, zadejte jinou adresu URL a znovu spusťte příkaz hello.
    
    ```PowerShell
    Another object with hello same value for property identifierUris already exists.
    ```
7. Vytvořte hello objekt služby AD.

    ```PowerShell
    New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    ```
8. Přidání služby hlavní toohello **Data Factory Přispěvatel** role.

    ```PowerShell
    New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    ```
9. Získání ID hello aplikace.

    ```PowerShell
    $azureAdApplication 
    ```
    Poznamenejte si ID aplikace hello (applicationID) z výstupu hello.

Z těchto kroků byste měli mít tyto čtyři hodnoty:

* ID tenanta
* ID předplatného
* ID aplikace
* Heslo (zadané v první příkaz hello)

## <a name="walkthrough"></a>Názorný postup
1. Pomocí sady Visual Studio 2012/2013/2015 vytvořte konzolovou aplikaci C# .NET.
   1. Spusťte **Visual Studio** 2012/2013/2015.
   2. Klikněte na tlačítko **soubor**, bod příliš**nový**a klikněte na tlačítko **projektu**.
   3. Rozbalte **Šablony** a vyberte **Visual C#**. V tomto názorném postupu použijete C#, ale mohli byste využít libovolný jazyk .NET.
   4. Vyberte **konzolové aplikace** hello seznamu typů projektu na hello správné.
   5. Zadejte **DataFactoryAPITestApp** pro hello název.
   6. Vyberte **C:\ADFGetStarted** pro hello umístění.
   7. Klikněte na tlačítko **OK** toocreate hello projektu.
2. Klikněte na tlačítko **nástroje**, bod příliš**Správce balíčků NuGet**a klikněte na tlačítko **Konzola správce balíčků**.
3. V hello **Konzola správce balíčků**, hello následující kroky:
   1. Spusťte následující příkaz tooinstall Data Factory balíček hello:`Install-Package Microsoft.Azure.Management.DataFactories`
   2. Spusťte následující příkaz tooinstall Azure Active Directory balíčku (použít Active Directory API v kódu hello) hello:`Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`
4. Přidejte následující hello **appSetttings** části toohello **App.config** souboru. Tato nastavení jsou používána ve hello Pomocná metoda: **GetAuthorizationHeader**.

    Nahraďte hodnoty pro **&lt;ID aplikace&gt;**, **&lt;Heslo&gt;**, **&lt;ID předplatného&gt;** a **&lt;ID tenanta&gt;** vlastními hodnotami.

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
            <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
            <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

            <add key="ApplicationId" value="your application ID" />
            <add key="Password" value="Password you used while creating hello AAD application" />
            <add key="SubscriptionId" value= "Subscription ID" />
            <add key="ActiveDirectoryTenantId" value="Tenant ID" />
        </appSettings>
    </configuration>
    ```

5. Přidejte následující hello **pomocí** příkazy toohello zdrojový soubor (Program.cs) v projektu hello.

    ```csharp
    using System.Configuration;
    using System.Collections.ObjectModel;
    using System.Threading;
    using System.Threading.Tasks;

    using Microsoft.Azure;
    using Microsoft.Azure.Management.DataFactories;
    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Common.Models;

    using Microsoft.IdentityModel.Clients.ActiveDirectory;

    ```

6. Přidejte následující kód, který vytvoří instanci hello **DataPipelineManagementClient** třída toohello **hlavní** metoda. Tento objekt toocreate použijete objekt pro vytváření dat, propojené služby, vstupní a výstupní datové sady a kanál. Tento objekt toomonitor řezy datové sady se také použít za běhu.

    ```csharp
    // create data factory management client
    string resourceGroupName = "ADFTutorialResourceGroup";
    string dataFactoryName = "APITutorialFactory";

    TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader().Result);

    Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);
    ```

   > [!IMPORTANT]
   > Nahraďte hodnotu hello **resourceGroupName** s hello název vaší skupiny prostředků Azure.
   >
   > Aktualizujte název hello data factory (dataFactoryName) toobe jedinečný. Název objektu pro vytváření dat hello musí být globálně jedinečný. V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.

7. Přidejte následující kód, který vytvoří hello **objekt pro vytváření dat** toohello **hlavní** metoda.

    ```csharp
    // create a data factory
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
    ```

    Objekt pro vytváření dat může mít jeden nebo víc kanálů. Kanál může obsahovat jednu nebo víc aktivit. Například aktivitu kopírování dat toocopy z tooa zdroje cílového úložiště dat a toorun aktivitu HDInsight Hive tootransform skriptu Hive vstupní data tooproduct výstupní data. Začneme vytvořením objektu pro vytváření dat hello v tomto kroku.
8. Přidejte následující kód, který vytvoří hello **propojená služba Azure Storage** toohello **hlavní** metoda.

   > [!IMPORTANT]
   > Položky **storageaccountname** a **accountkey** nahraďte názvem svého účtu Azure Storage a jeho klíčem.

    ```csharp
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
                    new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                )
            }
        }
    );
    ```

    Vytvoření propojených služeb v objektu pro vytváření toolink dat, data se ukládá a výpočetní služby toohello data factory. V tomto kurzu nebudete používat žádnou výpočetní službu jako je Azure HDInsight nebo Azure Data Lake Analytics. Budete používat dvě úložiště dat typu Azure Storage (zdroj) a Azure SQL Database (cíl). 

    Vytvoříte tedy dvě propojené služby s názvem AzureStorageLinkedService a AzureSqlLinkedService typu: AzureStorage a AzureSqlDatabase.  

    Hello AzureStorageLinkedService propojuje účet úložiště Azure toohello datovou továrnu. Tento účet úložiště se hello, jeden ve které jste vytvořili kontejner a objemu odeslaných dat hello jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
9. Přidejte následující kód, který vytvoří hello **propojená služba Azure SQL** toohello **hlavní** metoda.

   > [!IMPORTANT]
   > Položky **servername**, **databasename**, **username** a **password** nahraďte názvem serveru SQL Azure, názvem databáze, uživatelským jménem a heslem.

    ```csharp
    // create a linked service for output data store: Azure SQL Database
    Console.WriteLine("Creating Azure SQL Database linked service");
    client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new LinkedServiceCreateOrUpdateParameters()
        {
            LinkedService = new LinkedService()
            {
                Name = "AzureSqlLinkedService",
                Properties = new LinkedServiceProperties
                (
                    new AzureSqlDatabaseLinkedService("Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30")
                )
            }
        }
    );
    ```

    AzureSqlLinkedService propojuje službu Azure SQL database toohello datovou továrnu. Hello data, která se zkopírují z úložiště objektů blob hello jsou uložena v této databázi. Vytvořit hello tabulce emp v této databázi jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
10. Přidejte následující kód, který vytvoří hello **vstupní a výstupní datové sady** toohello **hlavní** metoda.

    ```csharp
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
                Structure = new List<DataElement>()
                {
                    new DataElement() { Name = "FirstName", Type = "String" },
                    new DataElement() { Name = "LastName", Type = "String" }
                },
                LinkedServiceName = "AzureStorageLinkedService",
                TypeProperties = new AzureBlobDataset()
                {
                    FolderPath = "adftutorial/",
                    FileName = "emp.txt"
                },
                External = true,
                Availability = new Availability()
                {
                    Frequency = SchedulePeriod.Hour,
                    Interval = 1,
                },

                Policy = new Policy()
                {
                    Validation = new ValidationPolicy()
                    {
                        MinimumRows = 1
                    }
                }
            }
        }
    });

    Console.WriteLine("Creating output dataset of type: Azure SQL");
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new DatasetCreateOrUpdateParameters()
        {
            Dataset = new Dataset()
            {
                Name = Dataset_Destination,
                Properties = new DatasetProperties()
                {
                    Structure = new List<DataElement>()
                    {
                        new DataElement() { Name = "FirstName", Type = "String" },
                        new DataElement() { Name = "LastName", Type = "String" }
                    },
                    LinkedServiceName = "AzureSqlLinkedService",
                    TypeProperties = new AzureSqlTableDataset()
                    {
                        TableName = "emp"
                    },
                    Availability = new Availability()
                    {
                        Frequency = SchedulePeriod.Hour,
                        Interval = 1,
                    },
                }
            }
        });
    ```
    
    V předchozím kroku hello jste vytvořili propojené služby toolink, účet úložiště Azure a Azure SQL database tooyour objekt pro vytváření dat. V tomto kroku definujete dvě datové sady s názvem InputDataset a OutputDataset, které představují vstupní a výstupní data, která jsou uložená v úložištích dat hello odkazuje AzureStorageLinkedService a AzureSqlLinkedService.

    Hello propojená služba úložiště Azure určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour účtu úložiště Azure. A datovou sadu hello vstupního objektu blob (InputDataset) určuje hello kontejneru a složce hello, který obsahuje vstupní data hello.  

    Podobně hello služby Azure SQL Database propojené určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour Azure SQL database. A hello výstupní datovou sadu tabulky SQL (OututDataset) určuje hello tabulky v hello databáze hello toowhich zkopírování dat z úložiště objektů blob hello.

    V tomto kroku vytvoříte datové sady s názvem InputDataset, který ukazuje soubor blob tooa (emp.txt) v kořenové složce hello kontejner objektů blob (adftutorial) v hello Azure Storage reprezentované hello AzureStorageLinkedService propojené služby. Pokud nechcete zadat hodnotu pro název souboru hello (nebo ji přeskočit), jsou data ze všech objektů BLOB ve složce vstupní hello zkopírovaný toohello cílový. V tomto kurzu zadejte hodnotu pro název souboru hello.    

    V tomto kroku vytvoříte výstupní datovou sadu s názvem **OutputDataset**. Tato datová sada body tooa tabulky SQL ve hello Azure SQL database reprezentované **AzureSqlLinkedService**.
11. Přidat hello následující kód, který **vytvoří a aktivuje kanálu** toohello **hlavní** metoda. V tomto kroku vytvoříte kanál s **aktivitou kopírování**, která používá **InputDataset** jako vstup a **OutputDataset** jako výstup.

    ```csharp
    // create a pipeline
    Console.WriteLine("Creating a pipeline");
    DateTime PipelineActivePeriodStartTime = new DateTime(2017, 5, 11, 0, 0, 0, 0, DateTimeKind.Utc);
    DateTime PipelineActivePeriodEndTime = new DateTime(2017, 5, 12, 0, 0, 0, 0, DateTimeKind.Utc);
    string PipelineName = "ADFTutorialPipeline";

    client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new PipelineCreateOrUpdateParameters()
        {
            Pipeline = new Pipeline()
            {
                Name = PipelineName,
                Properties = new PipelineProperties()
                {
                    Description = "Demo Pipeline for data transfer between blobs",

                    // Initial value for pipeline's active period. With this, you won't need tooset slice status
                    Start = PipelineActivePeriodStartTime,
                    End = PipelineActivePeriodEndTime,

                    Activities = new List<Activity>()
                    {
                        new Activity()
                        {
                            Name = "BlobToAzureSql",
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
                            TypeProperties = new CopyActivity()
                            {
                                Source = new BlobSource(),
                                Sink = new BlobSink()
                                {
                                    WriteBatchSize = 10000,
                                    WriteBatchTimeout = TimeSpan.FromMinutes(10)
                                }
                            }
                        }
                    }
                }
            }
        });
    ```

    Všimněte si hello následující body:
   
    - V části hello aktivit je jenom jedna aktivita jejichž **typ** je nastaven příliš**kopie**. Další informace o aktivitě kopírování hello najdete v tématu [aktivity přesunu dat](data-factory-data-movement-activities.md). V řešeních služby Data Factory můžete také použít [aktivity transformace dat](data-factory-data-transformation-activities.md).
    - Vstup aktivity hello nastaven příliš**InputDataset** a výstup hello aktivity je nastavený příliš**OutputDataset**. 
    - V hello **rámci typeProperties** části **BlobSource** je zadán jako typ zdroje hello a **SqlSink** je zadán jako typ jímky hello. Úplný seznam úložišť dat nepodporuje aktivity kopírování hello jako zdroje a jímky, najdete v části [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats). toolearn jak toouse konkrétní podporované datové úložiště jako zdroj/jímka, klikněte na odkaz hello v tabulce hello.  
   
    Výstupní datové sady v současné době je, jaké jednotky hello plán. V tomto kurzu výstupní datové sady je nakonfigurované tooproduce řez jednou za hodinu. Hello kanálu má čas spuštění a čas ukončení, které jsou od sebe, jeden den, což je 24 hodin. Proto 24 řezů výstupní datové sady vyprodukované hello kanálu.
12. Přidejte následující kód toohello hello **hlavní** metoda tooget hello stav datový řez ze hello výstupní datovou sadu. V této ukázce se očekává jenom jeden řez.

    ```csharp
    // Pulling status within a timeout threshold
    DateTime start = DateTime.Now;
    bool done = false;

    while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
    {
        Console.WriteLine("Pulling hello slice status");        
        // wait before hello next status check
        Thread.Sleep(1000 * 12);

        var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
            new DataSliceListParameters()
            {
                DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
            });

        foreach (DataSlice slice in datalistResponse.DataSlices)
        {
            if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
            {
                Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                done = true;
                break;
            }
            else
            {
                Console.WriteLine("Slice status is: {0}", slice.State);
            }
        }
    }
    ```

13. Přidejte následující kód tooget spustit podrobnosti datový řez toohello hello **hlavní** metoda.

    ```csharp
    Console.WriteLine("Getting run details of a data slice");

    // give it a few minutes for hello output slice toobe ready
    Console.WriteLine("\nGive it a few minutes for hello output slice toobe ready and press any key.");
    Console.ReadKey();

    var datasliceRunListResponse = client.DataSliceRuns.List(
            resourceGroupName,
            dataFactoryName,
            Dataset_Destination,
            new DataSliceRunListParameters()
            {
                DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
            }
        );

    foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
    {
        Console.WriteLine("Status: \t\t{0}", run.Status);
        Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
        Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
        Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
        Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
        Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
        Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
    }

    Console.WriteLine("\nPress any key tooexit.");
    Console.ReadKey();
    ```

14. Přidejte následující metodu helper používané hello hello **hlavní** metoda toohello **Program** třídy.

    > [!NOTE] 
    > Když je kopírujete a vkládáte hello následující kód, ujistěte se, že hello zkopírovaný kód je ve stejné úrovni jako hello metodu Main hello.

    ```csharp
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
    ```

15. V hello Průzkumníku řešení rozbalte projekt hello (DataFactoryAPITestApp), klikněte pravým tlačítkem na **odkazy**a klikněte na tlačítko **přidat odkaz na**. Zaškrtněte políčko pro sestavení **System.Configuration**. Pak klikněte na **OK**.
16. Vytvoření konzolové aplikace hello. Klikněte na tlačítko **sestavení** na hello nabídky a klikněte na **sestavit řešení**.
17. Potvrďte, že je alespoň jeden soubor v hello **adftutorial** kontejneru ve službě Azure blob storage. Pokud ne, vytvořte **Emp.txt** soubor v poznámkovém bloku s hello následující obsah a nahrajte ho toohello adftutorial kontejneru.

    ```
    John, Doe
    Jane, Doe
    ```
18. Hello ukázku spustit kliknutím **ladění** -> **spustit ladění** v nabídce hello. Až se zobrazí hello **získávání spustit podrobnosti datový řez**, počkejte několik minut a stiskněte klávesu **ENTER**.
19. Použití této datové továrně hello hello Azure portálu tooverify **APITutorialFactory** je vytvořena s hello artefakty následující:
   * Propojená služba: **LinkedService_AzureStorage**
   * Datová sada: **InputDataset** a **OutputDataset**.
   * Kanál: **PipelineBlobSample**
20. Ověřte, že hello dva záznamy zaměstnanců jsou vytvořeny v hello **emp** tabulky v hello zadat databázi Azure SQL.

## <a name="next-steps"></a>Další kroky
Úplnou dokumentaci k rozhraní .NET API pro datovou továrnu najdete v [referencích k rozhraní .NET API služby Data Factory](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1).

V tomto kurzu jste v operaci kopírování použili úložiště objektů blob jako zdrojové úložiště dat a databázi Azure SQL jako cílové úložiště dat. Hello následující tabulka obsahuje seznam úložiště dat, které jsou podporované jako zdroje a cíle pomocí aktivity kopírování hello: 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

toolearn o tom, jak toocopy data z datové úložiště, klikněte na odkaz hello hello úložiště dat v tabulce hello.

