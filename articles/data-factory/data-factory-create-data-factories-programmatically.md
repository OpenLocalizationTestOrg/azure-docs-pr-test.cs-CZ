---
title: "aaaCreate datových kanálů pomocí sady Azure .NET SDK | Microsoft Docs"
description: "Zjistěte, jak tooprogrammatically vytvořit, sledovat a spravovat objekty pro vytváření dat Azure pomocí sady SDK Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: b0a357be-3040-4789-831e-0d0a32a0bda5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 190b5f99edbb3c27e1e8efb8990b9e601b22458f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-monitor-and-manage-azure-data-factories-using-azure-data-factory-net-sdk"></a>Vytvoření, sledovat a spravovat Azure data Factory pomocí .NET SDK služby Azure Data Factory
## <a name="overview"></a>Přehled
Můžete vytvořit, sledovat a spravovat Azure data Factory programově pomocí .NET SDK služby Data Factory. Tento článek obsahuje návod, který může sledovat toocreate ukázkové aplikace konzoly .NET, která vytvoří a sleduje služby data factory. 

> [!NOTE]
> Tento článek nepopisuje všechny hello Data Factory .NET API. V tématu [Data Factory .NET API – referenční informace](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1) úplnou dokumentaci o .NET API pro Data Factory. 

## <a name="prerequisites"></a>Požadavky
* Visual Studio 2012 nebo 2013 nebo 2015
* Stáhněte a nainstalujte [Azure .NET SDK](http://azure.microsoft.com/downloads/).
* Azure Powershell Postupujte podle pokynů v [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článek tooinstall prostředí Azure PowerShell ve vašem počítači. Používáte prostředí Azure PowerShell toocreate aplikaci Azure Active Directory.

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
    $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFDotNetWalkthroughApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfdotnetwalkthroughapp.org/example" -Password "Pass@word1"
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
V Průvodci hello vytvoříte objekt pro vytváření dat kanál, který obsahuje aktivitu kopírování. Hello aktivity kopírování kopíruje data ze složky ve složce tooanother úložiště objektů blob v Azure aplikace hello stejné úložiště objektů blob. 

Hello aktivita kopírování provádí přesun dat hello v Azure Data Factory. Hello aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelné způsobem. V tématu [aktivity přesunu dat](data-factory-data-movement-activities.md) článku podrobnosti o aktivitě kopírování hello.

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
4. Nahraďte obsah hello **App.config** soubor v projektu hello s hello následující obsah: 
    
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
5. V souboru App.Config hello aktualizujte hodnoty pro  **&lt;ID aplikace&gt;**,  **&lt;heslo&gt;**,  **&lt; ID předplatného&gt;**, a  **&lt;ID klienta&gt;**  vlastními hodnotami.
6. Přidejte následující hello **pomocí** příkazy toohello **Program.cs** soubor v projektu hello.

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

    //IMPORTANT: specify hello name of Azure resource group here
    string resourceGroupName = "ADFTutorialResourceGroup";

    //IMPORTANT: hello name of hello data factory must be globally unique.
    // Therefore, update this value. For example:APITutorialFactory05122017
    string dataFactoryName = "APITutorialFactory";

    TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader().Result);

    Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);
    ```

   > [!IMPORTANT]
   > Nahraďte hodnotu hello **resourceGroupName** s hello název vaší skupiny prostředků Azure. Můžete vytvořit skupinu prostředků pomocí hello [New-AzureResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) rutiny.
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
9. Přidejte následující kód, který vytvoří hello **vstupní a výstupní datové sady** toohello **hlavní** metoda.

    Hello **FolderPath** hello vstupního objektu blob je nastavený příliš**adftutorial /** kde **adftutorial** je název hello hello kontejneru ve službě blob storage. Pokud tento kontejner ve službě Azure blob storage neexistuje, vytvořit kontejner s tímto názvem: **adftutorial** a nahrát textový soubor toohello kontejner.

    výstup Hello FolderPath pro hello objektu blob je nastavena na: **adftutorial/apifactoryoutput / {řezu}** kde **řez** je hodnota dynamicky počítané na základě hello **SliceStart**(spustit datum a čas každý řez.)

    ```csharp
    // create input and output datasets
    Console.WriteLine("Creating input and output datasets");
    string Dataset_Source = "DatasetBlobSource";
    string Dataset_Destination = "DatasetBlobDestination";
    
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
                    FolderPath = "adftutorial/apifactoryoutput/{Slice}",
                    PartitionedBy = new Collection<Partition>()
                    {
                        new Partition()
                        {
                            Name = "Slice",
                            Value = new DateTimePartitionValue()
                            {
                                Date = "SliceStart",
                                Format = "yyyyMMdd-HH"
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
    ```
10. Přidat hello následující kód, který **vytvoří a aktivuje kanálu** toohello **hlavní** metoda. Tento kanál má aktivitu **CopyActivity**, která jako zdroj používá **BlobSource** a jako jímku používá **BlobSink**.

    Hello aktivita kopírování provádí přesun dat hello v Azure Data Factory. Hello aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelné způsobem. V tématu [aktivity přesunu dat](data-factory-data-movement-activities.md) článku podrobnosti o aktivitě kopírování hello.

    ```csharp
    // create a pipeline
    Console.WriteLine("Creating a pipeline");
    DateTime PipelineActivePeriodStartTime = new DateTime(2014, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
    DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
    string PipelineName = "PipelineBlobSample";
    
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
                        Name = "BlobToBlob",
                        Inputs = new List<ActivityInput>()
                        {
                            new ActivityInput()
                {
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
    
                },
            }
        }
    });
    ```
12. Přidejte následující kód toohello hello **hlavní** metoda tooget hello stav datový řez ze hello výstupní datovou sadu. Není v této ukázce se očekává pouze jeden řez.

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
13. **(volitelné)**  Přidat hello následující kód tooget spustit podrobnosti datový řez toohello **hlavní** metoda.

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
        });
    
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
14. Přidejte následující metodu helper používané hello hello **hlavní** metoda toohello **Program** třídy. Tato metoda otevře dialogové okno, který umožňuje zadat **uživatelské jméno** a **heslo** použijete toolog tooAzure portálu.

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

15. V Průzkumníku řešení hello, rozbalte projekt hello: **DataFactoryAPITestApp**, klikněte pravým tlačítkem na **odkazy**a klikněte na tlačítko **přidat odkaz na**. Zaškrtněte políčko u `System.Configuration` sestavení a klikněte na tlačítko **OK**.
15. Vytvoření konzolové aplikace hello. Klikněte na tlačítko **sestavení** na hello nabídky a klikněte na **sestavit řešení**.
16. Potvrďte, že existuje alespoň jeden soubor hello adftutorial kontejneru ve službě Azure blob storage. Pokud ne, vytvořte soubor Emp.txt v poznámkovém bloku s hello následující obsahu a nahrajte ho toohello adftutorial kontejneru.

    ```
    John, Doe
    Jane, Doe
    ```
17. Hello ukázku spustit kliknutím **ladění** -> **spustit ladění** v nabídce hello. Až se zobrazí hello **získávání spustit podrobnosti datový řez**, počkejte několik minut a stiskněte klávesu **ENTER**.
18. Použití této datové továrně hello hello Azure portálu tooverify **APITutorialFactory** je vytvořena s hello artefakty následující:
    * Propojená služba: **AzureStorageLinkedService**
    * Datová sada: **DatasetBlobSource** a **DatasetBlobDestination**.
    * Kanál: **PipelineBlobSample**
19. Ověřte, že výstupní soubor je vytvořen v hello **apifactoryoutput** složky v hello **adftutorial** kontejneru.

## <a name="get-a-list-of-failed-data-slices"></a>Získat seznam neúspěšných datové řezy 

```csharp
// Parse hello resource path
var ResourceGroupName = "ADFTutorialResourceGroup";
var DataFactoryName = "DataFactoryAPITestApp";

var parameters = new ActivityWindowsByDataFactoryListParameters(ResourceGroupName, DataFactoryName);
parameters.WindowState = "Failed";
var response = dataFactoryManagementClient.ActivityWindows.List(parameters);
do
{
    foreach (var activityWindow in response.ActivityWindowListResponseValue.ActivityWindows)
    {
        var row = string.Join(
            "\t",
            activityWindow.WindowStart.ToString(),
            activityWindow.WindowEnd.ToString(),
            activityWindow.RunStart.ToString(),
            activityWindow.RunEnd.ToString(),
            activityWindow.DataFactoryName,
            activityWindow.PipelineName,
            activityWindow.ActivityName,
            string.Join(",", activityWindow.OutputDatasets));
        Console.WriteLine(row);
    }

    if (response.NextLink != null)
    {
        response = dataFactoryManagementClient.ActivityWindows.ListNext(response.NextLink, parameters);
    }
    else
    {
        response = null;
    }
}
while (response != null);
```

## <a name="next-steps"></a>Další kroky
Viz následující ukázka pro vytvoření kanálu pomocí sady .NET SDK, který kopíruje data z Azure blob storage tooan Azure SQL database hello: 

- [Vytvoření kanálu toocopy dat z úložiště objektů Blob tooSQL databáze](data-factory-copy-activity-tutorial-using-dotnet-api.md)
