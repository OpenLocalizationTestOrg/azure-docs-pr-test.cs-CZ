---
title: "Správa .NET v1.x SDK pro Azure Stream Analytics | Microsoft Docs"
description: "Začínáme s .NET SDK služby Stream Analytics správy. Zjistěte, jak nastavit a spustit úlohy analytics. Vytvoření projektu, vstupy, výstupy a transformace."
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
ms.openlocfilehash: c75322ba53a447b8529023482945051caaf61bb2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="management-net-sdk-v1x-set-up-and-run-analytics-jobs-using-the-azure-stream-analytics-api-for-net"></a><span data-ttu-id="446ee-106">Správa .NET SDK v1.x: nastavit až a spouštění úloh analytics pomocí rozhraní API služby Azure Stream Analytics pro .NET</span><span class="sxs-lookup"><span data-stu-id="446ee-106">Management .NET SDK v1.x: Set up and run analytics jobs using the Azure Stream Analytics API for .NET</span></span>
<span data-ttu-id="446ee-107">Zjistěte, jak nastavit a spustit úlohy analytics pomocí rozhraní API služby Stream Analytics pro .NET pomocí .NET SDK služby správy.</span><span class="sxs-lookup"><span data-stu-id="446ee-107">Learn how to set up and run analytics jobs using the Stream Analytics API for .NET using the Management .NET SDK.</span></span> <span data-ttu-id="446ee-108">Nastavení projektu, vytvořte vstupní a výstupní zdrojů, transformace a spuštění a zastavení úloh.</span><span class="sxs-lookup"><span data-stu-id="446ee-108">Set up a project, create input and output sources, transformations, and start and stop jobs.</span></span> <span data-ttu-id="446ee-109">Pro úlohy analýzy může Streamovat data z úložiště objektů Blob nebo z centra událostí.</span><span class="sxs-lookup"><span data-stu-id="446ee-109">For your analytics jobs, you can stream data from Blob storage or from an event hub.</span></span>

<span data-ttu-id="446ee-110">Najdete v článku [správu referenční dokumentaci rozhraní API služby Stream Analytics pro .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span><span class="sxs-lookup"><span data-stu-id="446ee-110">See the [management reference documentation for the Stream Analytics API for .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span></span>

<span data-ttu-id="446ee-111">Azure Stream Analytics je plně spravovaná služba poskytuje zpracování událostí s nízkou latencí, vysoce dostupná, škálovatelná, komplexní přes streamování dat v cloudu.</span><span class="sxs-lookup"><span data-stu-id="446ee-111">Azure Stream Analytics is a fully managed service providing low-latency, highly available, scalable, complex event processing over streaming data in the cloud.</span></span> <span data-ttu-id="446ee-112">Stream Analytics umožňuje zákazníkům nastavit úloh streamování k analýze datové proudy a umožňuje, aby jednotka téměř analýzu v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="446ee-112">Stream Analytics enables customers to set up streaming jobs to analyze data streams, and allows them to drive near real-time analytics.</span></span>  

> [!NOTE]
> <span data-ttu-id="446ee-113">Ukázkový kód v tomto článku se pořád používají starší verze (1.x) verzi .NET SDK služby Azure Stream Analytics správy.</span><span class="sxs-lookup"><span data-stu-id="446ee-113">Sample code in this article still uses legacy (1.x) version of Azure Stream Analytics Management .NET SDK.</span></span> <span data-ttu-id="446ee-114">Ukázku kódu pomocí aktuální verze sady SDK, najdete v tématu [používat sadu .NET SDK správy pro Stream Analytics](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-dotnet-management-sdk).</span><span class="sxs-lookup"><span data-stu-id="446ee-114">For sample code using the up-to-date SDK version, please see [Use the Management .NET SDK for Stream Analytics](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-dotnet-management-sdk).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="446ee-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="446ee-115">Prerequisites</span></span>
<span data-ttu-id="446ee-116">Je nutné, abyste před zahájením tohoto článku měli tyto položky:</span><span class="sxs-lookup"><span data-stu-id="446ee-116">Before you begin this article, you must have the following:</span></span>

* <span data-ttu-id="446ee-117">Nainstalujte Visual Studio 2017 nebo 2015.</span><span class="sxs-lookup"><span data-stu-id="446ee-117">Install Visual Studio 2017 or 2015.</span></span>
* <span data-ttu-id="446ee-118">Stáhněte a nainstalujte [Azure .NET SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="446ee-118">Download and install [Azure .NET SDK](https://azure.microsoft.com/downloads/).</span></span>
* <span data-ttu-id="446ee-119">Vytvořte skupinu prostředků Azure v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="446ee-119">Create an Azure Resource Group in your subscription.</span></span> <span data-ttu-id="446ee-120">Toto je ukázkový skript prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="446ee-120">The following is a sample Azure PowerShell script.</span></span> <span data-ttu-id="446ee-121">Prostředí Azure PowerShell informace najdete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview);</span><span class="sxs-lookup"><span data-stu-id="446ee-121">For Azure PowerShell information, see [Install and configure Azure PowerShell](/powershell/azure/overview);</span></span>  

        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered to the subscription, remove the remark symbol (#) to run the Register-AzureRMProvider cmdlet to register the provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


* <span data-ttu-id="446ee-122">Nastavení služby vstupní zdroj a cíl výstupu používat.</span><span class="sxs-lookup"><span data-stu-id="446ee-122">Set up an input source and output target to use.</span></span> <span data-ttu-id="446ee-123">Další pokyny najdete v tématu [vstupy přidat](stream-analytics-add-inputs.md) nastavit ukázka vstup a [přidejte výstupy](stream-analytics-add-outputs.md) nastavit ukázkový výstup.</span><span class="sxs-lookup"><span data-stu-id="446ee-123">For further instructions see [Add Inputs](stream-analytics-add-inputs.md) to set up a sample input and [Add Outputs](stream-analytics-add-outputs.md) to set up a sample output.</span></span>

## <a name="set-up-a-project"></a><span data-ttu-id="446ee-124">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="446ee-124">Set up a project</span></span>
<span data-ttu-id="446ee-125">Chcete-li vytvořit úlohu analytics použít rozhraní API analýzy datového proudu pro platformu .NET, nejprve nastavte projekt.</span><span class="sxs-lookup"><span data-stu-id="446ee-125">To create an analytics job use the Stream Analytics API for .NET, first set up your project.</span></span>

1. <span data-ttu-id="446ee-126">Vytvořte konzolovou aplikaci Visual Studio C# .NET.</span><span class="sxs-lookup"><span data-stu-id="446ee-126">Create a Visual Studio C# .NET console application.</span></span>
2. <span data-ttu-id="446ee-127">V konzole Správce balíčků spusťte následující příkazy instalace balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="446ee-127">In the Package Manager Console, run the following commands to install the NuGet packages.</span></span> <span data-ttu-id="446ee-128">První z nich je Azure Stream Analytics správu .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="446ee-128">The first one is the Azure Stream Analytics Management .NET SDK.</span></span> <span data-ttu-id="446ee-129">Druhá je klient Azure Active Directory, který se použije pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="446ee-129">The second one is the Azure Active Directory client that will be used for authentication.</span></span>
   
        Install-Package Microsoft.Azure.Management.StreamAnalytics -Version 1.8.3
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.28.4
3. <span data-ttu-id="446ee-130">Přidejte následující **appSettings** část do souboru App.config:</span><span class="sxs-lookup"><span data-stu-id="446ee-130">Add the following **appSettings** section to the App.config file:</span></span>
   
        <appSettings>
          <!--CSM Prod related values-->
          <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
          <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
          <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
          <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION" />
          <add key="ActiveDirectoryTenantId" value="YOU TENANT ID" />
        </appSettings>

    <span data-ttu-id="446ee-131">Nahraďte hodnoty pro **SubscriptionId** a **ActiveDirectoryTenantId** s Azure ID předplatného a klienta.</span><span class="sxs-lookup"><span data-stu-id="446ee-131">Replace values for **SubscriptionId** and **ActiveDirectoryTenantId** with your Azure subscription and tenant IDs.</span></span> <span data-ttu-id="446ee-132">Tyto hodnoty můžete získat spuštěním následující rutiny prostředí Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="446ee-132">You can get these values by running the following Azure PowerShell cmdlet:</span></span>

        Get-AzureAccount

4. <span data-ttu-id="446ee-133">Do souboru .csproj přidejte následující odkaz:</span><span class="sxs-lookup"><span data-stu-id="446ee-133">Add the following reference in your .csproj file:</span></span>

        <Reference Include="System.Configuration" />

1. <span data-ttu-id="446ee-134">Přidejte následující **pomocí** příkazy ke zdrojovému souboru (Program.cs) v projektu:</span><span class="sxs-lookup"><span data-stu-id="446ee-134">Add the following **using** statements to the source file (Program.cs) in the project:</span></span>
   
        using System;
        using System.Configuration;
        using System.Threading.Tasks;
        
        using Microsoft.Azure;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
2. <span data-ttu-id="446ee-135">Přidejte Pomocná metoda ověřování:</span><span class="sxs-lookup"><span data-stu-id="446ee-135">Add an authentication helper method:</span></span>

   ```   
   private static async Task<string> GetAuthorizationHeader()
   {
       var context = new AuthenticationContext(
           ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
           ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

        AuthenticationResult result = await context.AcquireTokenASync(
           resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
           clientId: ConfigurationManager.AppSettings["AsaClientId"],
           redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
           promptBehavior: PromptBehavior.Always);

        if (result != null)
            return result.AccessToken;

       throw new InvalidOperationException("Failed to acquire token");
   }
   ```  

## <a name="create-a-stream-analytics-management-client"></a><span data-ttu-id="446ee-136">Vytvoření klienta správy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="446ee-136">Create a Stream Analytics management client</span></span>
<span data-ttu-id="446ee-137">A **StreamAnalyticsManagementClient** objekt umožňuje spravovat úlohy a úlohy součástí, např. vstupní, výstupní a transformace.</span><span class="sxs-lookup"><span data-stu-id="446ee-137">A **StreamAnalyticsManagementClient** object allows you to manage the job and the job components, such as input, output, and transformation.</span></span>

<span data-ttu-id="446ee-138">Přidejte následující kód do začátku **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="446ee-138">Add the following code to the beginning of the **Main** method:</span></span>

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";
    string streamAnalyticsInputName = "<YOUR JOB INPUT NAME>";
    string streamAnalyticsOutputName = "<YOUR JOB OUTPUT NAME>";
    string streamAnalyticsTransformationName = "<YOUR JOB TRANSFORMATION NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
        ConfigurationManager.AppSettings["SubscriptionId"],
        GetAuthorizationHeader().Result);

    // Create Stream Analytics management client
    StreamAnalyticsManagementClient client = new StreamAnalyticsManagementClient(aadTokenCredentials);

<span data-ttu-id="446ee-139">**ResourceGroupName** hodnota proměnné by měl být stejný jako název skupiny prostředků vytvořené nebo zachyceny v požadovaných předchozích kroků.</span><span class="sxs-lookup"><span data-stu-id="446ee-139">The **resourceGroupName** variable's value should be the same as the name of the resource group you created or picked in the prerequisite steps.</span></span>

<span data-ttu-id="446ee-140">Automatizovat aspekt úlohy vytvoření přihlašovacích údajů prezentace, najdete v tématu [ověřování hlavní název služby pomocí Azure Resource Manageru](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="446ee-140">To automate the credential presentation aspect of job creation, refer to [Authenticating a service principal with Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span></span>

<span data-ttu-id="446ee-141">Zbývající části tohoto článku předpokládá, že tento kód je na začátku **hlavní** metoda.</span><span class="sxs-lookup"><span data-stu-id="446ee-141">The remaining sections of this article assume that this code is at the beginning of the **Main** method.</span></span>

## <a name="create-a-stream-analytics-job"></a><span data-ttu-id="446ee-142">Vytvoření úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="446ee-142">Create a Stream Analytics job</span></span>
<span data-ttu-id="446ee-143">Následující kód vytvoří úlohu služby Stream Analytics v rámci skupiny prostředků, který jste definovali.</span><span class="sxs-lookup"><span data-stu-id="446ee-143">The following code creates a Stream Analytics job under the resource group that you have defined.</span></span> <span data-ttu-id="446ee-144">Do úlohy budou později přidat vstupní, výstupní a transformace.</span><span class="sxs-lookup"><span data-stu-id="446ee-144">You will add an input, output, and transformation to the job later.</span></span>

    // Create a Stream Analytics job
    JobCreateOrUpdateParameters jobCreateParameters = new JobCreateOrUpdateParameters()
    {
        Job = new Job()
        {
            Name = streamAnalyticsJobName,
            Location = "<LOCATION>",
            Properties = new JobProperties()
            {
                EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Adjust,
                Sku = new Sku()
                {
                    Name = "Standard"
                }
            }
        }
    };

    JobCreateOrUpdateResponse jobCreateResponse = client.StreamingJobs.CreateOrUpdate(resourceGroupName, jobCreateParameters);


## <a name="create-a-stream-analytics-input-source"></a><span data-ttu-id="446ee-145">Vytvoření vstupní zdroj Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="446ee-145">Create a Stream Analytics input source</span></span>
<span data-ttu-id="446ee-146">Následující kód vytvoří Stream Analytics vstupní zdroj s typ vstupního zdroje blob a serializace sdíleného svazku clusteru.</span><span class="sxs-lookup"><span data-stu-id="446ee-146">The following code creates a Stream Analytics input source with the blob input source type and CSV serialization.</span></span> <span data-ttu-id="446ee-147">Chcete-li vytvořit vstupní zdroje centra událostí, použijte **EventHubStreamInputDataSource** místo **BlobStreamInputDataSource**.</span><span class="sxs-lookup"><span data-stu-id="446ee-147">To create an event hub input source, use **EventHubStreamInputDataSource** instead of **BlobStreamInputDataSource**.</span></span> <span data-ttu-id="446ee-148">Podobně můžete přizpůsobit serializace typ vstupního zdroje.</span><span class="sxs-lookup"><span data-stu-id="446ee-148">Similarly, you can customize the serialization type of the input source.</span></span>

    // Create a Stream Analytics input source
    InputCreateOrUpdateParameters jobInputCreateParameters = new InputCreateOrUpdateParameters()
    {
        Input = new Input()
        {
            Name = streamAnalyticsInputName,
            Properties = new StreamInputProperties()
            {
                Serialization = new CsvSerialization
                {
                    Properties = new CsvSerializationProperties
                    {
                        Encoding = "UTF8",
                        FieldDelimiter = ","
                    }
                },
                DataSource = new BlobStreamInputDataSource
                {
                    Properties = new BlobStreamInputDataSourceProperties
                    {
                        StorageAccounts = new StorageAccount[]
                        {
                            new StorageAccount()
                            {
                                AccountName = "<YOUR STORAGE ACCOUNT NAME>",
                                AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
                            }
                        },
                        Container = "samples",
                        PathPattern = ""
                    }
                }
            }
        }
    };

    InputCreateOrUpdateResponse inputCreateResponse =
        client.Inputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobInputCreateParameters);

<span data-ttu-id="446ee-149">Vstupní zdroje, ať už z úložiště objektů Blob nebo centra událostí, jsou svázané s určité úlohy.</span><span class="sxs-lookup"><span data-stu-id="446ee-149">Input sources, whether from Blob storage or an event hub, are tied to a specific job.</span></span> <span data-ttu-id="446ee-150">Pokud chcete použít stejný vstupní zdroj pro různé úlohy, musí volat metodu znovu a zadejte jiný název úlohy.</span><span class="sxs-lookup"><span data-stu-id="446ee-150">To use the same input source for different jobs, you must call the method again and specify a different job name.</span></span>

## <a name="test-a-stream-analytics-input-source"></a><span data-ttu-id="446ee-151">Testování vstupního zdroje Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="446ee-151">Test a Stream Analytics input source</span></span>
<span data-ttu-id="446ee-152">**TestConnection** Metoda testuje, jestli úloha Stream Analytics se může připojit ke zdroji vstupní stejně jako další aspekty, které jsou specifické pro daný typ vstupního zdroje.</span><span class="sxs-lookup"><span data-stu-id="446ee-152">The **TestConnection** method tests whether the Stream Analytics job is able to connect to the input source as well as other aspects specific to the input source type.</span></span> <span data-ttu-id="446ee-153">Například v objektu blob vstupního zdroje, které jste vytvořili v předchozím kroku, metoda zkontroluje, že název účtu úložiště a pár klíčů slouží k připojení k účtu úložiště a také zkontrolujte, jestli zadaný kontejner existuje.</span><span class="sxs-lookup"><span data-stu-id="446ee-153">For example, in the blob input source you created in an earlier step, the method will check that the Storage account name and key pair can be used to connect to the Storage account as well as check that the specified container exists.</span></span>

    // Test input source connection
    DataSourceTestConnectionResponse inputTestResponse =
        client.Inputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsInputName);

## <a name="create-a-stream-analytics-output-target"></a><span data-ttu-id="446ee-154">Vytvořit cíl výstupu Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="446ee-154">Create a Stream Analytics output target</span></span>
<span data-ttu-id="446ee-155">Vytvoření cíl výstupu je velmi podobné vytváření vstupní zdroj Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="446ee-155">Creating an output target is very similar to creating a Stream Analytics input source.</span></span> <span data-ttu-id="446ee-156">Jako vstupní zdrojů jsou svázané cíle výstup do konkrétní úlohy.</span><span class="sxs-lookup"><span data-stu-id="446ee-156">Like input sources, output targets are tied to a specific job.</span></span> <span data-ttu-id="446ee-157">Pokud chcete použít stejný cíl výstupu pro různé úlohy, musí volat metodu znovu a zadejte jiný název úlohy.</span><span class="sxs-lookup"><span data-stu-id="446ee-157">To use the same output target for different jobs, you must call the method again and specify a different job name.</span></span>

<span data-ttu-id="446ee-158">Následující kód vytvoří cíl výstupu (Azure SQL database).</span><span class="sxs-lookup"><span data-stu-id="446ee-158">The following code creates an output target (Azure SQL database).</span></span> <span data-ttu-id="446ee-159">Můžete upravit cíl výstupní datový typ nebo typ serializace.</span><span class="sxs-lookup"><span data-stu-id="446ee-159">You can customize the output target's data type and/or serialization type.</span></span>

    // Create a Stream Analytics output target
    OutputCreateOrUpdateParameters jobOutputCreateParameters = new OutputCreateOrUpdateParameters()
    {
        Output = new Output()
        {
            Name = streamAnalyticsOutputName,
            Properties = new OutputProperties()
            {
                DataSource = new SqlAzureOutputDataSource()
                {
                    Properties = new SqlAzureOutputDataSourceProperties()
                    {
                        Server = "<YOUR DATABASE SERVER NAME>",
                        Database = "<YOUR DATABASE NAME>",
                        User = "<YOUR DATABASE LOGIN>",
                        Password = "<YOUR DATABASE LOGIN PASSWORD>",
                        Table = "<YOUR DATABASE TABLE NAME>"
                    }
                }
            }
        }
    };

    OutputCreateOrUpdateResponse outputCreateResponse =
        client.Outputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobOutputCreateParameters);

## <a name="test-a-stream-analytics-output-target"></a><span data-ttu-id="446ee-160">Testování cíl výstupu Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="446ee-160">Test a Stream Analytics output target</span></span>
<span data-ttu-id="446ee-161">Cíl výstupu Stream Analytics má také **TestConnection** metoda pro testování připojení.</span><span class="sxs-lookup"><span data-stu-id="446ee-161">A Stream Analytics output target also has the **TestConnection** method for testing connections.</span></span>

    // Test output target connection
    DataSourceTestConnectionResponse outputTestResponse =
        client.Outputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsOutputName);

## <a name="create-a-stream-analytics-transformation"></a><span data-ttu-id="446ee-162">Vytvořte transformaci Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="446ee-162">Create a Stream Analytics transformation</span></span>
<span data-ttu-id="446ee-163">Následující kód vytvoří transformaci Stream Analytics s dotaz "vybrat * z vstup" a určuje přidělit jednu jednotku streamování pro úlohu služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="446ee-163">The following code creates a Stream Analytics transformation with the query "select * from Input" and specifies to allocate one streaming unit for the Stream Analytics job.</span></span> <span data-ttu-id="446ee-164">Další informace o úpravě jednotek streamování, najdete v části [úlohy škálování Azure Stream Analytics](stream-analytics-scale-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="446ee-164">For more information on adjusting streaming units, see [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md).</span></span>

    // Create a Stream Analytics transformation
    TransformationCreateOrUpdateParameters transformationCreateParameters = new TransformationCreateOrUpdateParameters()
    {
        Transformation = new Transformation()
        {
            Name = streamAnalyticsTransformationName,
            Properties = new TransformationProperties()
            {
                StreamingUnits = 1,
                Query = "select * from Input"
            }
        }
    };

    var transformationCreateResp =
        client.Transformations.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, transformationCreateParameters);

<span data-ttu-id="446ee-165">Jako vstup a výstup je vázaný na konkrétní úlohy Stream Analytics, který byl vytvořen v rámci také transformace.</span><span class="sxs-lookup"><span data-stu-id="446ee-165">Like input and output, a transformation is also tied to the specific Stream Analytics job it was created under.</span></span>

## <a name="start-a-stream-analytics-job"></a><span data-ttu-id="446ee-166">Spustit úlohu služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="446ee-166">Start a Stream Analytics job</span></span>
<span data-ttu-id="446ee-167">Po vytvoření úlohy služby Stream Analytics a jeho input(s), výstupem a transformace, můžete spustit úlohu voláním **spustit** metoda.</span><span class="sxs-lookup"><span data-stu-id="446ee-167">After creating a Stream Analytics job and its input(s), output(s), and transformation, you can start the job by calling the **Start** method.</span></span>

<span data-ttu-id="446ee-168">Následující ukázkový kód spustí úlohu služby Stream Analytics s časem zahájení vlastní výstup nastavena na 12 prosinec 2012, 12:12:12 UTC:</span><span class="sxs-lookup"><span data-stu-id="446ee-168">The following sample code starts a Stream Analytics job with a custom output start time set to December 12, 2012, 12:12:12 UTC:</span></span>

    // Start a Stream Analytics job
    JobStartParameters jobStartParameters = new JobStartParameters
    {
        OutputStartMode = OutputStartMode.CustomTime,
        OutputStartTime = new DateTime(2012, 12, 12, 0, 0, 0, DateTimeKind.Utc)
    };

    LongRunningOperationResponse jobStartResponse = client.StreamingJobs.Start(resourceGroupName, streamAnalyticsJobName, jobStartParameters);

## <a name="stop-a-stream-analytics-job"></a><span data-ttu-id="446ee-169">Zastavení úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="446ee-169">Stop a Stream Analytics job</span></span>
<span data-ttu-id="446ee-170">Spuštěná úloha Stream Analytics můžete zastavit voláním **Zastavit** metoda.</span><span class="sxs-lookup"><span data-stu-id="446ee-170">You can stop a running Stream Analytics job by calling the **Stop** method.</span></span>

    // Stop a Stream Analytics job
    LongRunningOperationResponse jobStopResponse = client.StreamingJobs.Stop(resourceGroupName, streamAnalyticsJobName);

## <a name="delete-a-stream-analytics-job"></a><span data-ttu-id="446ee-171">Odstranit úlohu služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="446ee-171">Delete a Stream Analytics job</span></span>
<span data-ttu-id="446ee-172">**Odstranit** metoda odstraní úlohu, jakož i základní dílčí prostředků, včetně input(s), výstupem a transformaci úlohy.</span><span class="sxs-lookup"><span data-stu-id="446ee-172">The **Delete** method will delete the job as well as the underlying sub-resources, including input(s), output(s), and transformation of the job.</span></span>

    // Delete a Stream Analytics job
    LongRunningOperationResponse jobDeleteResponse = client.StreamingJobs.Delete(resourceGroupName, streamAnalyticsJobName);

## <a name="get-support"></a><span data-ttu-id="446ee-173">Získat podporu</span><span class="sxs-lookup"><span data-stu-id="446ee-173">Get support</span></span>
<span data-ttu-id="446ee-174">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="446ee-174">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="446ee-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="446ee-175">Next steps</span></span>
<span data-ttu-id="446ee-176">Když jste se naučili základy používání služby pomocí sady .NET SDK k vytváření a spouštění úloh analytics.</span><span class="sxs-lookup"><span data-stu-id="446ee-176">You've learned the basics of using a .NET SDK to create and run analytics jobs.</span></span> <span data-ttu-id="446ee-177">Další informace najdete v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="446ee-177">To learn more, see the following:</span></span>

* [<span data-ttu-id="446ee-178">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="446ee-178">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="446ee-179">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="446ee-179">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="446ee-180">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="446ee-180">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* <span data-ttu-id="446ee-181">[Správa Azure Stream Analytics sady .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span><span class="sxs-lookup"><span data-stu-id="446ee-181">[Azure Stream Analytics Management .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span></span>
* [<span data-ttu-id="446ee-182">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="446ee-182">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="446ee-183">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="446ee-183">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

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
