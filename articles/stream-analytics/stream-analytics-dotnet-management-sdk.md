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
# <a name="management-net-sdk-set-up-and-run-analytics-jobs-using-hello-azure-stream-analytics-api-for-net"></a><span data-ttu-id="0fbe9-106">Správa .NET SDK: Nastavení a spuštění úloh analytics pomocí rozhraní API služby Azure Stream Analytics hello pro .NET</span><span class="sxs-lookup"><span data-stu-id="0fbe9-106">Management .NET SDK: Set up and run analytics jobs using hello Azure Stream Analytics API for .NET</span></span>
<span data-ttu-id="0fbe9-107">Zjistěte, jak tooset nahoru a spuštění analytics úloh pomocí hello Stream Analytics rozhraní API pro .NET pomocí hello Management .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-107">Learn how tooset up and run analytics jobs using hello Stream Analytics API for .NET using hello Management .NET SDK.</span></span> <span data-ttu-id="0fbe9-108">Nastavení projektu, vytvořte vstupní a výstupní zdrojů, transformace a spuštění a zastavení úloh.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-108">Set up a project, create input and output sources, transformations, and start and stop jobs.</span></span> <span data-ttu-id="0fbe9-109">Pro úlohy analýzy může Streamovat data z úložiště objektů Blob nebo z centra událostí.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-109">For your analytics jobs, you can stream data from Blob storage or from an event hub.</span></span>

<span data-ttu-id="0fbe9-110">V tématu hello [správu referenční dokumentaci k nástroji pro hello Stream Analytics rozhraní API pro .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span><span class="sxs-lookup"><span data-stu-id="0fbe9-110">See hello [management reference documentation for hello Stream Analytics API for .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span></span>

<span data-ttu-id="0fbe9-111">Azure Stream Analytics je plně spravovaná služba poskytuje zpracování událostí s nízkou latencí, vysoce dostupná, škálovatelná, komplexní přes streamování dat v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-111">Azure Stream Analytics is a fully managed service providing low-latency, highly available, scalable, complex event processing over streaming data in hello cloud.</span></span> <span data-ttu-id="0fbe9-112">Stream Analytics umožňuje zákazníkům tooset až streamování úlohy tooanalyze datové proudy a umožňuje jim toodrive téměř analýzu v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-112">Stream Analytics enables customers tooset up streaming jobs tooanalyze data streams, and allows them toodrive near real-time analytics.</span></span>  

> [!NOTE]
> <span data-ttu-id="0fbe9-113">Aktualizovali jsme hello ukázkový kód v tomto článku s verzí v2.x .NET SDK služby Azure Stream Analytics správy.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-113">We have updated hello sample code in this article with Azure Stream Analytics Management .NET SDK v2.x version.</span></span> <span data-ttu-id="0fbe9-114">Pro ukázku kódu pomocí hello používá verze sady SDK lagecy (1.x), najdete v tématu [použít v1.x hello Management .NET SDK pro Stream Analytics](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-dotnet-management-sdk-v1).</span><span class="sxs-lookup"><span data-stu-id="0fbe9-114">For sample code using hello uses lagecy (1.x) SDK version, please see [Use hello Management .NET SDK v1.x for Stream Analytics](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-dotnet-management-sdk-v1).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0fbe9-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0fbe9-115">Prerequisites</span></span>
<span data-ttu-id="0fbe9-116">Před zahájením tohoto článku, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="0fbe9-116">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="0fbe9-117">Nainstalujte Visual Studio 2017 nebo 2015.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-117">Install Visual Studio 2017 or 2015.</span></span>
* <span data-ttu-id="0fbe9-118">Stáhněte a nainstalujte [Azure .NET SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="0fbe9-118">Download and install [Azure .NET SDK](https://azure.microsoft.com/downloads/).</span></span>
* <span data-ttu-id="0fbe9-119">Vytvořte skupinu prostředků Azure v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-119">Create an Azure Resource Group in your subscription.</span></span> <span data-ttu-id="0fbe9-120">Následující Hello je ukázkový skript prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-120">hello following is a sample Azure PowerShell script.</span></span> <span data-ttu-id="0fbe9-121">Prostředí Azure PowerShell informace najdete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview);</span><span class="sxs-lookup"><span data-stu-id="0fbe9-121">For Azure PowerShell information, see [Install and configure Azure PowerShell](/powershell/azure/overview);</span></span>  

        # Log in tooyour Azure account
        Add-AzureAccount

        # Select hello Azure subscription you want toouse toocreate hello resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered toohello subscription, remove hello remark symbol (#) toorun hello Register-AzureRMProvider cmdlet tooregister hello provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


* <span data-ttu-id="0fbe9-122">Nastavit vstupní zdroj a cíl toouse výstupní.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-122">Set up an input source and output target toouse.</span></span> <span data-ttu-id="0fbe9-123">Další pokyny najdete v tématu [vstupy přidat](stream-analytics-add-inputs.md) tooset si ukázkový vstup a [přidejte výstupy](stream-analytics-add-outputs.md) tooset si ukázkový výstup.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-123">For further instructions see [Add Inputs](stream-analytics-add-inputs.md) tooset up a sample input and [Add Outputs](stream-analytics-add-outputs.md) tooset up a sample output.</span></span>

## <a name="set-up-a-project"></a><span data-ttu-id="0fbe9-124">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="0fbe9-124">Set up a project</span></span>
<span data-ttu-id="0fbe9-125">toocreate úlohu analytics použijte hello Stream Analytics rozhraní API pro platformu .NET, nejprve nastavte projekt.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-125">toocreate an analytics job use hello Stream Analytics API for .NET, first set up your project.</span></span>

1. <span data-ttu-id="0fbe9-126">Vytvořte konzolovou aplikaci Visual Studio C# .NET.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-126">Create a Visual Studio C# .NET console application.</span></span>
2. <span data-ttu-id="0fbe9-127">V hello Konzola správce balíčků hello spusťte následující příkazy balíčky NuGet tooinstall hello.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-127">In hello Package Manager Console, run hello following commands tooinstall hello NuGet packages.</span></span> <span data-ttu-id="0fbe9-128">Hello nejprve jeden je hello .NET SDK služby Azure Stream Analytics správy.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-128">hello first one is hello Azure Stream Analytics Management .NET SDK.</span></span> <span data-ttu-id="0fbe9-129">Hello druhá je pro ověřování klienta Azure.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-129">hello second one is for Azure client authentication.</span></span>
   
        Install-Package Microsoft.Azure.Management.StreamAnalytics -Version 2.0.0
        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.3.1
3. <span data-ttu-id="0fbe9-130">Přidejte následující hello **appSettings** souboru App.config toohello části:</span><span class="sxs-lookup"><span data-stu-id="0fbe9-130">Add hello following **appSettings** section toohello App.config file:</span></span>
   
        <appSettings>
          <add key="ClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR SUBSCRIPTION ID" />
          <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
        </appSettings>

    <span data-ttu-id="0fbe9-131">Nahraďte hodnoty pro **SubscriptionId** a **ActiveDirectoryTenantId** s Azure ID předplatného a klienta.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-131">Replace values for **SubscriptionId** and **ActiveDirectoryTenantId** with your Azure subscription and tenant IDs.</span></span> <span data-ttu-id="0fbe9-132">Tyto hodnoty můžete získat spuštěním následující rutiny Azure Powershellu hello:</span><span class="sxs-lookup"><span data-stu-id="0fbe9-132">You can get these values by running hello following Azure PowerShell cmdlet:</span></span>

        Get-AzureAccount

4. <span data-ttu-id="0fbe9-133">Hello následující odkaz v souboru .csproj přidejte:</span><span class="sxs-lookup"><span data-stu-id="0fbe9-133">Add hello following reference in your .csproj file:</span></span>

        <Reference Include="System.Configuration" />

5. <span data-ttu-id="0fbe9-134">Přidejte následující hello **pomocí** příkazy toohello zdrojový soubor (Program.cs) v projektu hello:</span><span class="sxs-lookup"><span data-stu-id="0fbe9-134">Add hello following **using** statements toohello source file (Program.cs) in hello project:</span></span>
   
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.Threading;
        using System.Threading.Tasks;
        
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Rest;
6. <span data-ttu-id="0fbe9-135">Přidejte Pomocná metoda ověřování:</span><span class="sxs-lookup"><span data-stu-id="0fbe9-135">Add an authentication helper method:</span></span>

   ```
   private static async Task<ServiceClientCredentials> GetCredentials()
   {
       var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(ConfigurationManager.AppSettings["ClientId"], new Uri("urn:ietf:wg:oauth:2.0:oob"));
       ServiceClientCredentials credentials = await UserTokenProvider.LoginWithPromptAsync(ConfigurationManager.AppSettings["ActiveDirectoryTenantId"], activeDirectoryClientSettings);
       
       return credentials;
    }
   ```

## <a name="create-a-stream-analytics-management-client"></a><span data-ttu-id="0fbe9-136">Vytvoření klienta správy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="0fbe9-136">Create a Stream Analytics management client</span></span>
<span data-ttu-id="0fbe9-137">A **StreamAnalyticsManagementClient** objekt vám umožní toomanage hello úlohy a hello úlohy komponenty, například vstupní, výstupní a transformace.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-137">A **StreamAnalyticsManagementClient** object allows you toomanage hello job and hello job components, such as input, output, and transformation.</span></span>

<span data-ttu-id="0fbe9-138">Přidejte následující kód toohello začátku hello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="0fbe9-138">Add hello following code toohello beginning of hello **Main** method:</span></span>

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

<span data-ttu-id="0fbe9-139">Hello **resourceGroupName** hodnota proměnné by měl být stejný jako název hello hello prostředku skupiny, kterou jste vytvořili nebo zachyceny v požadovaných krocích hello hello.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-139">hello **resourceGroupName** variable's value should be hello same as hello name of hello resource group you created or picked in hello prerequisite steps.</span></span>

<span data-ttu-id="0fbe9-140">tooautomate hello pověření prezentace aspekt úlohy vytvoření odkazovat příliš[ověřování hlavní název služby pomocí Azure Resource Manageru](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="0fbe9-140">tooautomate hello credential presentation aspect of job creation, refer too[Authenticating a service principal with Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span></span>

<span data-ttu-id="0fbe9-141">Hello zbývající části tohoto článku předpokládá, že tento kód je na začátku hello hello **hlavní** metoda.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-141">hello remaining sections of this article assume that this code is at hello beginning of hello **Main** method.</span></span>

## <a name="create-a-stream-analytics-job"></a><span data-ttu-id="0fbe9-142">Vytvoření úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="0fbe9-142">Create a Stream Analytics job</span></span>
<span data-ttu-id="0fbe9-143">Hello následující kód vytvoří úlohu služby Stream Analytics v rámci skupiny prostředků hello, který jste definovali.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-143">hello following code creates a Stream Analytics job under hello resource group that you have defined.</span></span> <span data-ttu-id="0fbe9-144">Úlohu toohello vstupní, výstupní a transformaci přidáte později.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-144">You will add an input, output, and transformation toohello job later.</span></span>

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

## <a name="create-a-stream-analytics-input-source"></a><span data-ttu-id="0fbe9-145">Vytvoření vstupní zdroj Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="0fbe9-145">Create a Stream Analytics input source</span></span>
<span data-ttu-id="0fbe9-146">Hello následující kód vytvoří Stream Analytics vstupní zdroj s typ vstupního zdroje blob hello a serializace sdíleného svazku clusteru.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-146">hello following code creates a Stream Analytics input source with hello blob input source type and CSV serialization.</span></span> <span data-ttu-id="0fbe9-147">použít toocreate centra událostí vstupního zdroje, **EventHubStreamInputDataSource** místo **BlobStreamInputDataSource**.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-147">toocreate an event hub input source, use **EventHubStreamInputDataSource** instead of **BlobStreamInputDataSource**.</span></span> <span data-ttu-id="0fbe9-148">Podobně můžete přizpůsobit hello serializace typu hello vstupní zdroj.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-148">Similarly, you can customize hello serialization type of hello input source.</span></span>

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

<span data-ttu-id="0fbe9-149">Vstupního zdroje z úložiště objektů Blob nebo centra událostí, jsou vázané tooa určité úlohy.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-149">Input sources, whether from Blob storage or an event hub, are tied tooa specific job.</span></span> <span data-ttu-id="0fbe9-150">toouse hello stejný vstupní zdroj pro různé úlohy, je nutné volat metodu hello znovu a zadejte jiný název úlohy.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-150">toouse hello same input source for different jobs, you must call hello method again and specify a different job name.</span></span>

## <a name="test-a-stream-analytics-input-source"></a><span data-ttu-id="0fbe9-151">Testování vstupního zdroje Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="0fbe9-151">Test a Stream Analytics input source</span></span>
<span data-ttu-id="0fbe9-152">Hello **TestConnection** metoda testy, zda úloha Stream Analytics hello není možné tooconnect toohello vstupní zdroj, jakož i další aspekty konkrétní toohello vstupní typ zdroje.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-152">hello **TestConnection** method tests whether hello Stream Analytics job is able tooconnect toohello input source as well as other aspects specific toohello input source type.</span></span> <span data-ttu-id="0fbe9-153">Například v hello vstupní zdroj objektů blob, kterou jste vytvořili v předchozím kroku, hello metoda zkontroluje, že hello název účtu úložiště a pár klíčů může být použité tooconnect toohello účet úložiště, stejně jako zkontrolujte, zda že tento hello zadaný kontejner existuje.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-153">For example, in hello blob input source you created in an earlier step, hello method will check that hello Storage account name and key pair can be used tooconnect toohello Storage account as well as check that hello specified container exists.</span></span>

   ```
   // Test hello connection toohello input
   ResourceTestStatus testInputResult = streamAnalyticsManagementClient.Inputs.Test(resourceGroupName, streamingJobName, inputName);
   ```

## <a name="create-a-stream-analytics-output-target"></a><span data-ttu-id="0fbe9-154">Vytvořit cíl výstupu Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="0fbe9-154">Create a Stream Analytics output target</span></span>
<span data-ttu-id="0fbe9-155">Vytvoření cíl výstupu je velmi podobné toocreating vstupní zdroj Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-155">Creating an output target is very similar toocreating a Stream Analytics input source.</span></span> <span data-ttu-id="0fbe9-156">Podobně jako vstupní zdrojů výstup cíle jsou vázané tooa určité úlohy.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-156">Like input sources, output targets are tied tooa specific job.</span></span> <span data-ttu-id="0fbe9-157">toouse hello stejný cíl výstupu pro různé úlohy, je nutné volat metodu hello znovu a zadejte jiný název úlohy.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-157">toouse hello same output target for different jobs, you must call hello method again and specify a different job name.</span></span>

<span data-ttu-id="0fbe9-158">Hello následující kód vytvoří cíl výstupu (Azure SQL database).</span><span class="sxs-lookup"><span data-stu-id="0fbe9-158">hello following code creates an output target (Azure SQL database).</span></span> <span data-ttu-id="0fbe9-159">Můžete upravit cíl výstup hello datový typ nebo typ serializace.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-159">You can customize hello output target's data type and/or serialization type.</span></span>

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

## <a name="test-a-stream-analytics-output-target"></a><span data-ttu-id="0fbe9-160">Testování cíl výstupu Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="0fbe9-160">Test a Stream Analytics output target</span></span>
<span data-ttu-id="0fbe9-161">Cíl výstupu Stream Analytics má také hello **TestConnection** metoda pro testování připojení.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-161">A Stream Analytics output target also has hello **TestConnection** method for testing connections.</span></span>

   ```
   // Test hello connection toohello output
   ResourceTestStatus testOutputResult = streamAnalyticsManagementClient.Outputs.Test(resourceGroupName, streamingJobName, outputName);
   ```

## <a name="create-a-stream-analytics-transformation"></a><span data-ttu-id="0fbe9-162">Vytvořte transformaci Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="0fbe9-162">Create a Stream Analytics transformation</span></span>
<span data-ttu-id="0fbe9-163">Hello následující kód vytvoří transformaci Stream Analytics s dotazem hello "vybrat * z vstup" a určuje tooallocate jednu jednotku streamování pro úlohy služby Stream Analytics hello.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-163">hello following code creates a Stream Analytics transformation with hello query "select * from Input" and specifies tooallocate one streaming unit for hello Stream Analytics job.</span></span> <span data-ttu-id="0fbe9-164">Další informace o úpravě jednotek streamování, najdete v části [úlohy škálování Azure Stream Analytics](stream-analytics-scale-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="0fbe9-164">For more information on adjusting streaming units, see [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md).</span></span>

   ```
   // Create a transformation
   Transformation transformation = new Transformation()
   {
       Query = "Select Id, Name from <your input name>", // '<your input name>' should be replaced with hello value you put for hello 'inputName' variable above or in a previous step
       StreamingUnits = 1
   };
   Transformation createTransformationResult = streamAnalyticsManagementClient.Transformations.CreateOrReplace(transformation, resourceGroupName, streamingJobName, transformationName);
   ```

<span data-ttu-id="0fbe9-165">Jako vstup a výstup transformace je také vázanou toohello konkrétní úlohy služby Stream Analytics, který byl vytvořen v části.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-165">Like input and output, a transformation is also tied toohello specific Stream Analytics job it was created under.</span></span>

## <a name="start-a-stream-analytics-job"></a><span data-ttu-id="0fbe9-166">Spustit úlohu služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="0fbe9-166">Start a Stream Analytics job</span></span>
<span data-ttu-id="0fbe9-167">Po vytvoření úlohy služby Stream Analytics a jeho input(s), výstupem a transformace, můžete spustit úlohy hello volání hello **spustit** metoda.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-167">After creating a Stream Analytics job and its input(s), output(s), and transformation, you can start hello job by calling hello **Start** method.</span></span>

<span data-ttu-id="0fbe9-168">Hello následující vzorový kód spustí úlohu služby Stream Analytics s vlastní výstup počáteční čas sadu tooDecember 12, 2012, 12:12:12 UTC:</span><span class="sxs-lookup"><span data-stu-id="0fbe9-168">hello following sample code starts a Stream Analytics job with a custom output start time set tooDecember 12, 2012, 12:12:12 UTC:</span></span>

   ```
   // Start a streaming job
   StartStreamingJobParameters startStreamingJobParameters = new StartStreamingJobParameters()
   {
       OutputStartMode = OutputStartMode.CustomTime,
       OutputStartTime = new DateTime(2012, 12, 12, 12, 12, 12, DateTimeKind.Utc)
   };
   streamAnalyticsManagementClient.StreamingJobs.Start(resourceGroupName, streamingJobName, startStreamingJobParameters);
   ```

## <a name="stop-a-stream-analytics-job"></a><span data-ttu-id="0fbe9-169">Zastavení úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="0fbe9-169">Stop a Stream Analytics job</span></span>
<span data-ttu-id="0fbe9-170">Spuštěná úloha Stream Analytics můžete zastavit pomocí volání hello **Zastavit** metoda.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-170">You can stop a running Stream Analytics job by calling hello **Stop** method.</span></span>

   ```
   // Stop a streaming job
   streamAnalyticsManagementClient.StreamingJobs.Stop(resourceGroupName, streamingJobName);
   ```

## <a name="delete-a-stream-analytics-job"></a><span data-ttu-id="0fbe9-171">Odstranit úlohu služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="0fbe9-171">Delete a Stream Analytics job</span></span>
<span data-ttu-id="0fbe9-172">Hello **odstranit** metoda odstraní hello úlohy a také hello základní dílčí prostředků, včetně input(s), výstupem a transformaci hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-172">hello **Delete** method will delete hello job as well as hello underlying sub-resources, including input(s), output(s), and transformation of hello job.</span></span>

   ```
   // Delete a streaming job
   streamAnalyticsManagementClient.StreamingJobs.Delete(resourceGroupName, streamingJobName);
   ```

## <a name="get-support"></a><span data-ttu-id="0fbe9-173">Získat podporu</span><span class="sxs-lookup"><span data-stu-id="0fbe9-173">Get support</span></span>
<span data-ttu-id="0fbe9-174">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="0fbe9-174">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0fbe9-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0fbe9-175">Next steps</span></span>
<span data-ttu-id="0fbe9-176">Jste se naučili základy hello pomocí .NET SDK toocreate a spusťte úlohy analytics.</span><span class="sxs-lookup"><span data-stu-id="0fbe9-176">You've learned hello basics of using a .NET SDK toocreate and run analytics jobs.</span></span> <span data-ttu-id="0fbe9-177">toolearn více, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="0fbe9-177">toolearn more, see hello following:</span></span>

* [<span data-ttu-id="0fbe9-178">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="0fbe9-178">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="0fbe9-179">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="0fbe9-179">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="0fbe9-180">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="0fbe9-180">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* <span data-ttu-id="0fbe9-181">[Správa Azure Stream Analytics sady .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span><span class="sxs-lookup"><span data-stu-id="0fbe9-181">[Azure Stream Analytics Management .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span></span>
* [<span data-ttu-id="0fbe9-182">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="0fbe9-182">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="0fbe9-183">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="0fbe9-183">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

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
