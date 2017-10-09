---
title: aaaManagement .NET SDK pro Azure Stream Analytics | Microsoft Docs
description: "Začínáme s .NET SDK služby Stream Analytics správy. Zjistěte, jak tooset až a spustit úlohy analytics. Vytvoření projektu, vstupy, výstupy a transformace."
keywords: ".NET SDK, analýzy rozhraní API"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 5e93de87-0c6f-4f4b-be98-08d63f832897
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/06/2017
ms.author: jeffstok
ms.openlocfilehash: 507c11938bc5bf2249a2e41f6bcc076db8ead3f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="management-net-sdk-set-up-and-run-analytics-jobs-using-hello-azure-stream-analytics-api-for-net"></a>Správa .NET SDK: Nastavení a spuštění úloh analytics pomocí rozhraní API služby Azure Stream Analytics hello pro .NET
Zjistěte, jak tooset nahoru a spuštění analytics úloh pomocí hello Stream Analytics rozhraní API pro .NET pomocí hello Management .NET SDK. Nastavení projektu, vytvořte vstupní a výstupní zdrojů, transformace a spuštění a zastavení úloh. Pro úlohy analýzy může Streamovat data z úložiště objektů Blob nebo z centra událostí.

V tématu hello [správu referenční dokumentaci k nástroji pro hello Stream Analytics rozhraní API pro .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).

Azure Stream Analytics je plně spravovaná služba poskytuje zpracování událostí s nízkou latencí, vysoce dostupná, škálovatelná, komplexní přes streamování dat v cloudu hello. Stream Analytics umožňuje zákazníkům tooset až streamování úlohy tooanalyze datové proudy a umožňuje jim toodrive téměř analýzu v reálném čase.  

> [!NOTE]
> Aktualizovali jsme hello ukázkový kód v tomto článku s verzí v2.x .NET SDK služby Azure Stream Analytics správy. Pro ukázku kódu pomocí hello používá verze sady SDK lagecy (1.x), najdete v tématu [použít v1.x hello Management .NET SDK pro Stream Analytics](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-dotnet-management-sdk-v1).

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto článku, musíte mít následující hello:

* Nainstalujte Visual Studio 2017 nebo 2015.
* Stáhněte a nainstalujte [Azure .NET SDK](https://azure.microsoft.com/downloads/).
* Vytvořte skupinu prostředků Azure v rámci vašeho předplatného. Následující Hello je ukázkový skript prostředí Azure PowerShell. Prostředí Azure PowerShell informace najdete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview);  

        # Log in tooyour Azure account
        Add-AzureAccount

        # Select hello Azure subscription you want toouse toocreate hello resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered toohello subscription, remove hello remark symbol (#) toorun hello Register-AzureRMProvider cmdlet tooregister hello provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


* Nastavit vstupní zdroj a cíl toouse výstupní. Další pokyny najdete v tématu [vstupy přidat](stream-analytics-add-inputs.md) tooset si ukázkový vstup a [přidejte výstupy](stream-analytics-add-outputs.md) tooset si ukázkový výstup.

## <a name="set-up-a-project"></a>Nastavení projektu
toocreate úlohu analytics použijte hello Stream Analytics rozhraní API pro platformu .NET, nejprve nastavte projekt.

1. Vytvořte konzolovou aplikaci Visual Studio C# .NET.
2. V hello Konzola správce balíčků hello spusťte následující příkazy balíčky NuGet tooinstall hello. Hello nejprve jeden je hello .NET SDK služby Azure Stream Analytics správy. Hello druhá je pro ověřování klienta Azure.
   
        Install-Package Microsoft.Azure.Management.StreamAnalytics -Version 2.0.0
        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.3.1
3. Přidejte následující hello **appSettings** souboru App.config toohello části:
   
        <appSettings>
          <add key="ClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR SUBSCRIPTION ID" />
          <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
        </appSettings>

    Nahraďte hodnoty pro **SubscriptionId** a **ActiveDirectoryTenantId** s Azure ID předplatného a klienta. Tyto hodnoty můžete získat spuštěním následující rutiny Azure Powershellu hello:

        Get-AzureAccount

4. Hello následující odkaz v souboru .csproj přidejte:

        <Reference Include="System.Configuration" />

5. Přidejte následující hello **pomocí** příkazy toohello zdrojový soubor (Program.cs) v projektu hello:
   
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.Threading;
        using System.Threading.Tasks;
        
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Rest;
6. Přidejte Pomocná metoda ověřování:

   ```
   private static async Task<ServiceClientCredentials> GetCredentials()
   {
       var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(ConfigurationManager.AppSettings["ClientId"], new Uri("urn:ietf:wg:oauth:2.0:oob"));
       ServiceClientCredentials credentials = await UserTokenProvider.LoginWithPromptAsync(ConfigurationManager.AppSettings["ActiveDirectoryTenantId"], activeDirectoryClientSettings);
       
       return credentials;
    }
   ```

## <a name="create-a-stream-analytics-management-client"></a>Vytvoření klienta správy Stream Analytics
A **StreamAnalyticsManagementClient** objekt vám umožní toomanage hello úlohy a hello úlohy komponenty, například vstupní, výstupní a transformace.

Přidejte následující kód toohello začátku hello hello **hlavní** metoda:

   ```
    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamingJobName = "<YOUR STREAMING JOB NAME>";
    string inputName = "<YOUR JOB INPUT NAME>";
    string transformationName = "<YOUR JOB TRANSFORMATION NAME>";
    string outputName = "<YOUR JOB OUTPUT NAME>";
    
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    
    // Get credentials
    ServiceClientCredentials credentials = GetCredentials().Result;
    
    // Create Stream Analytics management client
    StreamAnalyticsManagementClient streamAnalyticsManagementClient = new StreamAnalyticsManagementClient(credentials)
    {
        SubscriptionId = ConfigurationManager.AppSettings["SubscriptionId"]
    };
   ```

Hello **resourceGroupName** hodnota proměnné by měl být stejný jako název hello hello prostředku skupiny, kterou jste vytvořili nebo zachyceny v požadovaných krocích hello hello.

tooautomate hello pověření prezentace aspekt úlohy vytvoření odkazovat příliš[ověřování hlavní název služby pomocí Azure Resource Manageru](../azure-resource-manager/resource-group-authenticate-service-principal.md).

Hello zbývající části tohoto článku předpokládá, že tento kód je na začátku hello hello **hlavní** metoda.

## <a name="create-a-stream-analytics-job"></a>Vytvoření úlohy Stream Analytics
Hello následující kód vytvoří úlohu služby Stream Analytics v rámci skupiny prostředků hello, který jste definovali. Úlohu toohello vstupní, výstupní a transformaci přidáte později.

   ```
   // Create a streaming job
   StreamingJob streamingJob = new StreamingJob()
   {
       Tags = new Dictionary<string, string>()
       {
           { "Origin", ".NET SDK" },
           { "ReasonCreated", "Getting started tutorial" }
       },
       Location = "West US",
       EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Drop,
       EventsOutOfOrderMaxDelayInSeconds = 5,
       EventsLateArrivalMaxDelayInSeconds = 16,
       OutputErrorPolicy = OutputErrorPolicy.Drop,
       DataLocale = "en-US",
       CompatibilityLevel = CompatibilityLevel.OneFullStopZero,
       Sku = new Sku()
       {
           Name = SkuName.Standard
       }
   };
   StreamingJob createStreamingJobResult = streamAnalyticsManagementClient.StreamingJobs.CreateOrReplace(streamingJob, resourceGroupName, streamingJobName);
   ```

## <a name="create-a-stream-analytics-input-source"></a>Vytvoření vstupní zdroj Stream Analytics
Hello následující kód vytvoří Stream Analytics vstupní zdroj s typ vstupního zdroje blob hello a serializace sdíleného svazku clusteru. použít toocreate centra událostí vstupního zdroje, **EventHubStreamInputDataSource** místo **BlobStreamInputDataSource**. Podobně můžete přizpůsobit hello serializace typu hello vstupní zdroj.

   ```
   // Create an input
   StorageAccount storageAccount = new StorageAccount()
   {
       AccountName = "<YOUR STORAGE ACCOUNT NAME>",
       AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
   };
   Input input = new Input()
   {
       Properties = new StreamInputProperties()
       {
           Serialization = new CsvSerialization()
           {
               FieldDelimiter = ",",
               Encoding = Encoding.UTF8
           },
           Datasource = new BlobStreamInputDataSource()
           {
               StorageAccounts = new[] { storageAccount },
               Container = "<YOUR STORAGE BLOB CONTAINER>",
               PathPattern = "{date}/{time}",
               DateFormat = "yyyy/MM/dd",
               TimeFormat = "HH",
               SourcePartitionCount = 16
           }
       }
   };
   Input createInputResult = streamAnalyticsManagementClient.Inputs.CreateOrReplace(input, resourceGroupName, streamingJobName, inputName);
   ```

Vstupního zdroje z úložiště objektů Blob nebo centra událostí, jsou vázané tooa určité úlohy. toouse hello stejný vstupní zdroj pro různé úlohy, je nutné volat metodu hello znovu a zadejte jiný název úlohy.

## <a name="test-a-stream-analytics-input-source"></a>Testování vstupního zdroje Stream Analytics
Hello **TestConnection** metoda testy, zda úloha Stream Analytics hello není možné tooconnect toohello vstupní zdroj, jakož i další aspekty konkrétní toohello vstupní typ zdroje. Například v hello vstupní zdroj objektů blob, kterou jste vytvořili v předchozím kroku, hello metoda zkontroluje, že hello název účtu úložiště a pár klíčů může být použité tooconnect toohello účet úložiště, stejně jako zkontrolujte, zda že tento hello zadaný kontejner existuje.

   ```
   // Test hello connection toohello input
   ResourceTestStatus testInputResult = streamAnalyticsManagementClient.Inputs.Test(resourceGroupName, streamingJobName, inputName);
   ```

## <a name="create-a-stream-analytics-output-target"></a>Vytvořit cíl výstupu Stream Analytics
Vytvoření cíl výstupu je velmi podobné toocreating vstupní zdroj Stream Analytics. Podobně jako vstupní zdrojů výstup cíle jsou vázané tooa určité úlohy. toouse hello stejný cíl výstupu pro různé úlohy, je nutné volat metodu hello znovu a zadejte jiný název úlohy.

Hello následující kód vytvoří cíl výstupu (Azure SQL database). Můžete upravit cíl výstup hello datový typ nebo typ serializace.

   ```
   // Create an output
   Output output = new Output()
   {
       Datasource = new AzureSqlDatabaseOutputDataSource()
       {
           Server = "<YOUR DATABASE SERVER NAME>",
           Database = "<YOUR DATABASE NAME>",
           User = "<YOUR DATABASE LOGIN>",
           Password = "<YOUR DATABASE LOGIN PASSWORD>",
           Table = "<YOUR DATABASE TABLE NAME>"
       }
   };
   Output createOutputResult = streamAnalyticsManagementClient.Outputs.CreateOrReplace(output, resourceGroupName, streamingJobName, outputName);
   ```

## <a name="test-a-stream-analytics-output-target"></a>Testování cíl výstupu Stream Analytics
Cíl výstupu Stream Analytics má také hello **TestConnection** metoda pro testování připojení.

   ```
   // Test hello connection toohello output
   ResourceTestStatus testOutputResult = streamAnalyticsManagementClient.Outputs.Test(resourceGroupName, streamingJobName, outputName);
   ```

## <a name="create-a-stream-analytics-transformation"></a>Vytvořte transformaci Stream Analytics
Hello následující kód vytvoří transformaci Stream Analytics s dotazem hello "vybrat * z vstup" a určuje tooallocate jednu jednotku streamování pro úlohy služby Stream Analytics hello. Další informace o úpravě jednotek streamování, najdete v části [úlohy škálování Azure Stream Analytics](stream-analytics-scale-jobs.md).

   ```
   // Create a transformation
   Transformation transformation = new Transformation()
   {
       Query = "Select Id, Name from <your input name>", // '<your input name>' should be replaced with hello value you put for hello 'inputName' variable above or in a previous step
       StreamingUnits = 1
   };
   Transformation createTransformationResult = streamAnalyticsManagementClient.Transformations.CreateOrReplace(transformation, resourceGroupName, streamingJobName, transformationName);
   ```

Jako vstup a výstup transformace je také vázanou toohello konkrétní úlohy služby Stream Analytics, který byl vytvořen v části.

## <a name="start-a-stream-analytics-job"></a>Spustit úlohu služby Stream Analytics
Po vytvoření úlohy služby Stream Analytics a jeho input(s), výstupem a transformace, můžete spustit úlohy hello volání hello **spustit** metoda.

Hello následující vzorový kód spustí úlohu služby Stream Analytics s vlastní výstup počáteční čas sadu tooDecember 12, 2012, 12:12:12 UTC:

   ```
   // Start a streaming job
   StartStreamingJobParameters startStreamingJobParameters = new StartStreamingJobParameters()
   {
       OutputStartMode = OutputStartMode.CustomTime,
       OutputStartTime = new DateTime(2012, 12, 12, 12, 12, 12, DateTimeKind.Utc)
   };
   streamAnalyticsManagementClient.StreamingJobs.Start(resourceGroupName, streamingJobName, startStreamingJobParameters);
   ```

## <a name="stop-a-stream-analytics-job"></a>Zastavení úlohy Stream Analytics
Spuštěná úloha Stream Analytics můžete zastavit pomocí volání hello **Zastavit** metoda.

   ```
   // Stop a streaming job
   streamAnalyticsManagementClient.StreamingJobs.Stop(resourceGroupName, streamingJobName);
   ```

## <a name="delete-a-stream-analytics-job"></a>Odstranit úlohu služby Stream Analytics
Hello **odstranit** metoda odstraní hello úlohy a také hello základní dílčí prostředků, včetně input(s), výstupem a transformaci hello úlohy.

   ```
   // Delete a streaming job
   streamAnalyticsManagementClient.StreamingJobs.Delete(resourceGroupName, streamingJobName);
   ```

## <a name="get-support"></a>Získat podporu
Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky
Jste se naučili základy hello pomocí .NET SDK toocreate a spusťte úlohy analytics. toolearn více, najdete v části hello následující:

* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Správa Azure Stream Analytics sady .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[azure.blob.storage]: http://azure.microsoft.com/documentation/services/storage/
[azure.blob.storage.use]: http://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/

[azure.event.hubs]: http://azure.microsoft.com/services/event-hubs/
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.forum]: http://go.microsoft.com/fwlink/?LinkId=512151

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.developer.guide]: stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
