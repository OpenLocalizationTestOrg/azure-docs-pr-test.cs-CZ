---
title: "Prostřednictvím kódu programu Sledování úloh v Stream Analytics | Microsoft Docs"
description: "Naučte se monitorovat prostřednictvím kódu programu úlohy Stream Analytics vytvořené pomocí rozhraní REST API, Azure SDK nebo prostředí PowerShell."
keywords: "monitorování .net, monitorování úloh monitorování aplikace"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 2ec02cc9-4ca5-4a25-ae60-c44be9ad4835
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 0d39e77316a03a705586af3ba970a7be1208ec85
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="programmatically-create-a-stream-analytics-job-monitor"></a><span data-ttu-id="92c65-104">Vytváření monitorování úlohy Stream Analytics prostřednictvím kódu programu</span><span class="sxs-lookup"><span data-stu-id="92c65-104">Programmatically create a Stream Analytics job monitor</span></span>

<span data-ttu-id="92c65-105">Tento článek ukazuje, jak povolit monitorování v rámci úlohy Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="92c65-105">This article demonstrates how to enable monitoring for a Stream Analytics job.</span></span> <span data-ttu-id="92c65-106">Úlohy analýzy datového proudu, které jsou vytvořené pomocí rozhraní REST API, Azure SDK nebo prostředí PowerShell nemají povoleno ve výchozím nastavení monitorování.</span><span class="sxs-lookup"><span data-stu-id="92c65-106">Stream Analytics jobs that are created via REST APIs, Azure SDK, or PowerShell do not have monitoring enabled by default.</span></span> <span data-ttu-id="92c65-107">Můžete ručně ji povolit na portálu Azure přejděte na stránku úlohy monitorování a kliknutím na tlačítko Povolit nebo tento proces můžete automatizovat pomocí kroků v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="92c65-107">You can manually enable it in the Azure portal by going to the job’s Monitor page and clicking the Enable button or you can automate this process by following the steps in this article.</span></span> <span data-ttu-id="92c65-108">Data sledování se zobrazí v oblasti metriky portál Azure pro vaše úloha Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="92c65-108">The monitoring data will show up in the Metrics area of the Azure portal for your Stream Analytics job.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="92c65-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="92c65-109">Prerequisites</span></span>

<span data-ttu-id="92c65-110">Před zahájením tohoto procesu, musíte mít následující:</span><span class="sxs-lookup"><span data-stu-id="92c65-110">Before you begin this process, you must have the following:</span></span>

* <span data-ttu-id="92c65-111">Visual Studio 2017 nebo 2015</span><span class="sxs-lookup"><span data-stu-id="92c65-111">Visual Studio 2017 or 2015</span></span>
* <span data-ttu-id="92c65-112">[Azure .NET SDK](https://azure.microsoft.com/downloads/) staženy a nainstalovány</span><span class="sxs-lookup"><span data-stu-id="92c65-112">[Azure .NET SDK](https://azure.microsoft.com/downloads/) downloaded and installed</span></span>
* <span data-ttu-id="92c65-113">Existující úloha Stream Analytics, který musí mít povoleno monitorování</span><span class="sxs-lookup"><span data-stu-id="92c65-113">An existing Stream Analytics job that needs to have monitoring enabled</span></span>

## <a name="create-a-project"></a><span data-ttu-id="92c65-114">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="92c65-114">Create a project</span></span>

1. <span data-ttu-id="92c65-115">Vytvořte konzolovou aplikaci Visual Studio C# .NET.</span><span class="sxs-lookup"><span data-stu-id="92c65-115">Create a Visual Studio C# .NET console application.</span></span>
2. <span data-ttu-id="92c65-116">V konzole Správce balíčků spusťte následující příkazy instalace balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="92c65-116">In the Package Manager Console, run the following commands to install the NuGet packages.</span></span> <span data-ttu-id="92c65-117">První z nich je Azure Stream Analytics správu .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="92c65-117">The first one is the Azure Stream Analytics Management .NET SDK.</span></span> <span data-ttu-id="92c65-118">Druhá je Azure SDK pro monitorování, který se použije k povolení monitorování.</span><span class="sxs-lookup"><span data-stu-id="92c65-118">The second one is the Azure Monitor SDK that will be used to enable monitoring.</span></span> <span data-ttu-id="92c65-119">Poslední je klient Azure Active Directory, který se použije pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="92c65-119">The last one is the Azure Active Directory client that will be used for authentication.</span></span>
   
   ```
   Install-Package Microsoft.Azure.Management.StreamAnalytics
   Install-Package Microsoft.Azure.Insights -Pre
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```
3. <span data-ttu-id="92c65-120">Přidejte následující oddíl appSettings do souboru App.config.</span><span class="sxs-lookup"><span data-stu-id="92c65-120">Add the following appSettings section to the App.config file.</span></span>
   
   ```
   <appSettings>
     <!--CSM Prod related values-->
     <add key="ResourceGroupName" value="RESOURCE GROUP NAME" />
     <add key="JobName" value="YOUR JOB NAME" />
     <add key="StorageAccountName" value="YOUR STORAGE ACCOUNT"/>
     <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
     <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
     <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
     <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
     <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
     <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION ID" />
     <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
   </appSettings>
   ```
   <span data-ttu-id="92c65-121">Nahraďte hodnoty pro *SubscriptionId* a *ActiveDirectoryTenantId* s Azure ID předplatného a klienta.</span><span class="sxs-lookup"><span data-stu-id="92c65-121">Replace values for *SubscriptionId* and *ActiveDirectoryTenantId* with your Azure subscription and tenant IDs.</span></span> <span data-ttu-id="92c65-122">Tyto hodnoty můžete získat spuštěním následující rutiny prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="92c65-122">You can get these values by running the following PowerShell cmdlet:</span></span>
   
   ```
   Get-AzureAccount
   ```
4. <span data-ttu-id="92c65-123">Přidejte následující příkazy ke zdrojovému souboru (Program.cs) v projektu.</span><span class="sxs-lookup"><span data-stu-id="92c65-123">Add the following using statements to the source file (Program.cs) in the project.</span></span>
   
   ```
     using System;
     using System.Configuration;
     using System.Threading;
     using Microsoft.Azure;
     using Microsoft.Azure.Management.Insights;
     using Microsoft.Azure.Management.Insights.Models;
     using Microsoft.Azure.Management.StreamAnalytics;
     using Microsoft.Azure.Management.StreamAnalytics.Models;
     using Microsoft.IdentityModel.Clients.ActiveDirectory;
   ```
5. <span data-ttu-id="92c65-124">Přidejte metodu helper ověřování.</span><span class="sxs-lookup"><span data-stu-id="92c65-124">Add an authentication helper method.</span></span>
   
     <span data-ttu-id="92c65-125">Veřejné statické řetězec GetAuthorizationHeader()</span><span class="sxs-lookup"><span data-stu-id="92c65-125">public static string GetAuthorizationHeader()</span></span>
   
         {
             AuthenticationResult result = null;
             var thread = new Thread(() =>
             {
                 try
                 {
                     var context = new AuthenticationContext(
                         ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
                         ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
   
                     result = context.AcquireToken(
                         resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                         clientId: ConfigurationManager.AppSettings["AsaClientId"],
                         redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                         promptBehavior: PromptBehavior.Always);
                 }
                 catch (Exception threadEx)
                 {
                     Console.WriteLine(threadEx.Message);
                 }
             });
   
             thread.SetApartmentState(ApartmentState.STA);
             thread.Name = "AcquireTokenThread";
             thread.Start();
             thread.Join();
   
             if (result != null)
             {
                 return result.AccessToken;
             }
   
             throw new InvalidOperationException("Failed to acquire token");
     <span data-ttu-id="92c65-126">}</span><span class="sxs-lookup"><span data-stu-id="92c65-126">}</span></span>

## <a name="create-management-clients"></a><span data-ttu-id="92c65-127">Vytvoření klientů pro správu</span><span class="sxs-lookup"><span data-stu-id="92c65-127">Create management clients</span></span>

<span data-ttu-id="92c65-128">Následující kód nastaví nezbytné proměnné a správy klientů.</span><span class="sxs-lookup"><span data-stu-id="92c65-128">The following code will set up the necessary variables and management clients.</span></span>

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    Uri resourceManagerUri = new
    Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    // Create Stream Analytics and Insights management client
    StreamAnalyticsManagementClient streamAnalyticsClient = new
    StreamAnalyticsManagementClient(aadTokenCredentials, resourceManagerUri);
    InsightsManagementClient insightsClient = new
    InsightsManagementClient(aadTokenCredentials, resourceManagerUri);

## <a name="enable-monitoring-for-an-existing-stream-analytics-job"></a><span data-ttu-id="92c65-129">Povolit monitorování pro existující úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="92c65-129">Enable monitoring for an existing Stream Analytics job</span></span>

<span data-ttu-id="92c65-130">Následující kód umožňuje monitorování pro **existující** úlohy služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="92c65-130">The following code enables monitoring for an **existing** Stream Analytics job.</span></span> <span data-ttu-id="92c65-131">První část kód provede požadavek GET službu Stream Analytics k načtení informací o konkrétní úloze Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="92c65-131">The first part of the code performs a GET request against the Stream Analytics service to retrieve information about the particular Stream Analytics job.</span></span> <span data-ttu-id="92c65-132">Použije *Id* vlastnost (získané z požadavek GET) jako parametr pro metodu Put ve druhé polovině kódu, který odesílá PUT žádost o službě Statistika povolení monitorování pro úlohu služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="92c65-132">It uses the *Id* property (retrieved from the GET request) as a parameter for the Put method in the second half of the code, which sends a PUT request to the Insights service to enable monitoring for the Stream Analytics job.</span></span>

>[!WARNING]
><span data-ttu-id="92c65-133">Pokud jste dříve povolili monitorování pro různé úlohy služby Stream Analytics, buď prostřednictvím portálu Azure nebo prostřednictvím kódu programu níže uvedeného kódu, **doporučujeme zadat stejný název účtu úložiště, který jste použili, když jste povolili dříve monitorování.**</span><span class="sxs-lookup"><span data-stu-id="92c65-133">If you have previously enabled monitoring for a different Stream Analytics job, either through the Azure portal or programmatically via the below code, **we recommend that you provide the same storage account name that you used when you previously enabled monitoring.**</span></span>
> 
> <span data-ttu-id="92c65-134">Účet úložiště souvisí oblast, které jste vytvořili vaší úloze Stream Analytics, není určený speciálně pro úlohy sám sebe.</span><span class="sxs-lookup"><span data-stu-id="92c65-134">The storage account is linked to the region that you created your Stream Analytics job in, not specifically to the job itself.</span></span>
> 
> <span data-ttu-id="92c65-135">Všechny služby Stream Analytics úlohy (a všechny ostatní prostředky služby Azure) v této oblasti stejné sdílet tento účet úložiště pro ukládání dat monitorování.</span><span class="sxs-lookup"><span data-stu-id="92c65-135">All Stream Analytics jobs (and all other Azure resources) in that same region share this storage account to store monitoring data.</span></span> <span data-ttu-id="92c65-136">Pokud zadáte jiný účet úložiště, se může způsobit nečekané vedlejší účinky v sledování jiné úlohy Stream Analytics nebo jiných prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="92c65-136">If you provide a different storage account, it might cause unintended side effects in the monitoring of your other Stream Analytics jobs or other Azure resources.</span></span>
> 
> <span data-ttu-id="92c65-137">Název účtu úložiště, který můžete použít k nahrazení `<YOUR STORAGE ACCOUNT NAME>` v následujícím kódu by měla být účet úložiště, který je ve stejném předplatném jako úlohu služby Stream Analytics, který chcete povolit monitorování.</span><span class="sxs-lookup"><span data-stu-id="92c65-137">The storage account name that you use to replace `<YOUR STORAGE ACCOUNT NAME>` in the following code should be a storage account that is in the same subscription as the Stream Analytics job that you are enabling monitoring for.</span></span>
> 
> 

    // Get an existing Stream Analytics job
    JobGetParameters jobGetParameters = new JobGetParameters()
    {
        PropertiesToExpand = "inputs,transformation,outputs"
    };
    JobGetResponse jobGetResponse = streamAnalyticsClient.StreamingJobs.Get(resourceGroupName, streamAnalyticsJobName, jobGetParameters);

    // Enable monitoring
    ServiceDiagnosticSettingsPutParameters insightPutParameters = new ServiceDiagnosticSettingsPutParameters()
    {
            Properties = new ServiceDiagnosticSettings()
            {
                StorageAccountName = "<YOUR STORAGE ACCOUNT NAME>"
            }
    };
    insightsClient.ServiceDiagnosticSettingsOperations.Put(jobGetResponse.Job.Id, insightPutParameters);



## <a name="get-support"></a><span data-ttu-id="92c65-138">Získat podporu</span><span class="sxs-lookup"><span data-stu-id="92c65-138">Get support</span></span>

<span data-ttu-id="92c65-139">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="92c65-139">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="92c65-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="92c65-140">Next steps</span></span>

* [<span data-ttu-id="92c65-141">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="92c65-141">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="92c65-142">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="92c65-142">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="92c65-143">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="92c65-143">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="92c65-144">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="92c65-144">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="92c65-145">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="92c65-145">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

