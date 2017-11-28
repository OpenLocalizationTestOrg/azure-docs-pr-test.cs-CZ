---
title: "Sledování úloh aaaProgrammatically v Stream Analytics | Microsoft Docs"
description: "Zjistěte, jak tooprogrammatically monitorování úlohy Stream Analytics vytvořené pomocí rozhraní REST API, Azure SDK nebo prostředí PowerShell."
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
ms.openlocfilehash: 44a9c29c2161ee81ea76ece4646a8691bf5d5b48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="programmatically-create-a-stream-analytics-job-monitor"></a><span data-ttu-id="3acbb-104">Vytváření monitorování úlohy Stream Analytics prostřednictvím kódu programu</span><span class="sxs-lookup"><span data-stu-id="3acbb-104">Programmatically create a Stream Analytics job monitor</span></span>

<span data-ttu-id="3acbb-105">Tento článek ukazuje, jak tooenable monitorování u úlohy Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="3acbb-105">This article demonstrates how tooenable monitoring for a Stream Analytics job.</span></span> <span data-ttu-id="3acbb-106">Úlohy analýzy datového proudu, které jsou vytvořené pomocí rozhraní REST API, Azure SDK nebo prostředí PowerShell nemají povoleno ve výchozím nastavení monitorování.</span><span class="sxs-lookup"><span data-stu-id="3acbb-106">Stream Analytics jobs that are created via REST APIs, Azure SDK, or PowerShell do not have monitoring enabled by default.</span></span> <span data-ttu-id="3acbb-107">Můžete ručně ji povolit v hello portálu Azure tak, že přejdete na stránku toohello úlohy monitorování a hello kliknutím na tlačítko Povolit nebo tento proces můžete automatizovat pomocí následujících kroků hello v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="3acbb-107">You can manually enable it in hello Azure portal by going toohello job’s Monitor page and clicking hello Enable button or you can automate this process by following hello steps in this article.</span></span> <span data-ttu-id="3acbb-108">Hello dat monitorování se zobrazí v oblasti metriky hello hello portál Azure pro vaše úloha Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="3acbb-108">hello monitoring data will show up in hello Metrics area of hello Azure portal for your Stream Analytics job.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3acbb-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3acbb-109">Prerequisites</span></span>

<span data-ttu-id="3acbb-110">Před zahájením tohoto procesu, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="3acbb-110">Before you begin this process, you must have hello following:</span></span>

* <span data-ttu-id="3acbb-111">Visual Studio 2017 nebo 2015</span><span class="sxs-lookup"><span data-stu-id="3acbb-111">Visual Studio 2017 or 2015</span></span>
* <span data-ttu-id="3acbb-112">[Azure .NET SDK](https://azure.microsoft.com/downloads/) staženy a nainstalovány</span><span class="sxs-lookup"><span data-stu-id="3acbb-112">[Azure .NET SDK](https://azure.microsoft.com/downloads/) downloaded and installed</span></span>
* <span data-ttu-id="3acbb-113">Existující úlohy Stream Analytics vyžadující toohave monitoring je povolena.</span><span class="sxs-lookup"><span data-stu-id="3acbb-113">An existing Stream Analytics job that needs toohave monitoring enabled</span></span>

## <a name="create-a-project"></a><span data-ttu-id="3acbb-114">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="3acbb-114">Create a project</span></span>

1. <span data-ttu-id="3acbb-115">Vytvořte konzolovou aplikaci Visual Studio C# .NET.</span><span class="sxs-lookup"><span data-stu-id="3acbb-115">Create a Visual Studio C# .NET console application.</span></span>
2. <span data-ttu-id="3acbb-116">V hello Konzola správce balíčků hello spusťte následující příkazy balíčky NuGet tooinstall hello.</span><span class="sxs-lookup"><span data-stu-id="3acbb-116">In hello Package Manager Console, run hello following commands tooinstall hello NuGet packages.</span></span> <span data-ttu-id="3acbb-117">Hello nejprve jeden je hello .NET SDK služby Azure Stream Analytics správy.</span><span class="sxs-lookup"><span data-stu-id="3acbb-117">hello first one is hello Azure Stream Analytics Management .NET SDK.</span></span> <span data-ttu-id="3acbb-118">Hello druhá je hello Azure SDK monitorování, který se použije tooenable monitorování.</span><span class="sxs-lookup"><span data-stu-id="3acbb-118">hello second one is hello Azure Monitor SDK that will be used tooenable monitoring.</span></span> <span data-ttu-id="3acbb-119">Hello poslední jedním je hello klienta Azure Active Directory, který se použije pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="3acbb-119">hello last one is hello Azure Active Directory client that will be used for authentication.</span></span>
   
   ```
   Install-Package Microsoft.Azure.Management.StreamAnalytics
   Install-Package Microsoft.Azure.Insights -Pre
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```
3. <span data-ttu-id="3acbb-120">Přidejte následující soubor App.config toohello oddílu appSettings hello.</span><span class="sxs-lookup"><span data-stu-id="3acbb-120">Add hello following appSettings section toohello App.config file.</span></span>
   
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
   <span data-ttu-id="3acbb-121">Nahraďte hodnoty pro *SubscriptionId* a *ActiveDirectoryTenantId* s Azure ID předplatného a klienta.</span><span class="sxs-lookup"><span data-stu-id="3acbb-121">Replace values for *SubscriptionId* and *ActiveDirectoryTenantId* with your Azure subscription and tenant IDs.</span></span> <span data-ttu-id="3acbb-122">Tyto hodnoty můžete získat spuštěním následující rutiny prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="3acbb-122">You can get these values by running hello following PowerShell cmdlet:</span></span>
   
   ```
   Get-AzureAccount
   ```
4. <span data-ttu-id="3acbb-123">Přidejte následující hello pomocí příkazů toohello zdrojový soubor (Program.cs) v projektu hello.</span><span class="sxs-lookup"><span data-stu-id="3acbb-123">Add hello following using statements toohello source file (Program.cs) in hello project.</span></span>
   
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
5. <span data-ttu-id="3acbb-124">Přidejte metodu helper ověřování.</span><span class="sxs-lookup"><span data-stu-id="3acbb-124">Add an authentication helper method.</span></span>
   
     <span data-ttu-id="3acbb-125">Veřejné statické řetězec GetAuthorizationHeader()</span><span class="sxs-lookup"><span data-stu-id="3acbb-125">public static string GetAuthorizationHeader()</span></span>
   
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
   
             throw new InvalidOperationException("Failed tooacquire token");
     <span data-ttu-id="3acbb-126">}</span><span class="sxs-lookup"><span data-stu-id="3acbb-126">}</span></span>

## <a name="create-management-clients"></a><span data-ttu-id="3acbb-127">Vytvoření klientů pro správu</span><span class="sxs-lookup"><span data-stu-id="3acbb-127">Create management clients</span></span>

<span data-ttu-id="3acbb-128">Hello následující kód bude nastavení hello nezbytné proměnné a správy klientů.</span><span class="sxs-lookup"><span data-stu-id="3acbb-128">hello following code will set up hello necessary variables and management clients.</span></span>

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

## <a name="enable-monitoring-for-an-existing-stream-analytics-job"></a><span data-ttu-id="3acbb-129">Povolit monitorování pro existující úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="3acbb-129">Enable monitoring for an existing Stream Analytics job</span></span>

<span data-ttu-id="3acbb-130">Hello následující kód povolí monitorování **existující** úlohy služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="3acbb-130">hello following code enables monitoring for an **existing** Stream Analytics job.</span></span> <span data-ttu-id="3acbb-131">první část Hello hello kód provede požadavek GET hello Stream Analytics služby tooretrieve informace o konkrétní úloze Stream Analytics hello.</span><span class="sxs-lookup"><span data-stu-id="3acbb-131">hello first part of hello code performs a GET request against hello Stream Analytics service tooretrieve information about hello particular Stream Analytics job.</span></span> <span data-ttu-id="3acbb-132">Používá hello *Id* vlastnost (získané z požadavek GET hello) jako parametr pro hello metodu Put v hello druhé polovině tématu hello kód, který odešle požadavek PUT toohello Statistika služby tooenable monitorování hello Stream Analytics úloha.</span><span class="sxs-lookup"><span data-stu-id="3acbb-132">It uses hello *Id* property (retrieved from hello GET request) as a parameter for hello Put method in hello second half of hello code, which sends a PUT request toohello Insights service tooenable monitoring for hello Stream Analytics job.</span></span>

>[!WARNING]
><span data-ttu-id="3acbb-133">Pokud jste dříve povolili monitorování pro různé úlohy služby Stream Analytics, buď prostřednictvím hello portál Azure nebo prostřednictvím kódu programu hello níže uvedeného kódu, **doporučujeme zadat hello stejný název účtu úložiště, můžete použít, když jste dřív povolené monitorování.**</span><span class="sxs-lookup"><span data-stu-id="3acbb-133">If you have previously enabled monitoring for a different Stream Analytics job, either through hello Azure portal or programmatically via hello below code, **we recommend that you provide hello same storage account name that you used when you previously enabled monitoring.**</span></span>
> 
> <span data-ttu-id="3acbb-134">účet úložiště Hello je propojené toohello oblast, který jste vytvořili vaší úloze Stream Analytics v není konkrétně úlohy toohello sám sebe.</span><span class="sxs-lookup"><span data-stu-id="3acbb-134">hello storage account is linked toohello region that you created your Stream Analytics job in, not specifically toohello job itself.</span></span>
> 
> <span data-ttu-id="3acbb-135">Všechny služby Stream Analytics úlohy (a všechny ostatní prostředky služby Azure) v této oblasti stejné sdílet tento toostore účet úložiště dat monitorování.</span><span class="sxs-lookup"><span data-stu-id="3acbb-135">All Stream Analytics jobs (and all other Azure resources) in that same region share this storage account toostore monitoring data.</span></span> <span data-ttu-id="3acbb-136">Pokud zadáte jiný účet úložiště, se může způsobit nečekané vedlejší účinky v hello sledování jiné úlohy Stream Analytics nebo jiných prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="3acbb-136">If you provide a different storage account, it might cause unintended side effects in hello monitoring of your other Stream Analytics jobs or other Azure resources.</span></span>
> 
> <span data-ttu-id="3acbb-137">název účtu úložiště Hello použít tooreplace `<YOUR STORAGE ACCOUNT NAME>` hello následující kód musí být účet úložiště, který je v hello stejnému předplatnému jako úlohy služby Stream Analytics hello, který chcete povolit monitorování.</span><span class="sxs-lookup"><span data-stu-id="3acbb-137">hello storage account name that you use tooreplace `<YOUR STORAGE ACCOUNT NAME>` in hello following code should be a storage account that is in hello same subscription as hello Stream Analytics job that you are enabling monitoring for.</span></span>
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



## <a name="get-support"></a><span data-ttu-id="3acbb-138">Získat podporu</span><span class="sxs-lookup"><span data-stu-id="3acbb-138">Get support</span></span>

<span data-ttu-id="3acbb-139">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="3acbb-139">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3acbb-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3acbb-140">Next steps</span></span>

* [<span data-ttu-id="3acbb-141">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="3acbb-141">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="3acbb-142">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="3acbb-142">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="3acbb-143">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="3acbb-143">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="3acbb-144">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="3acbb-144">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="3acbb-145">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="3acbb-145">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

