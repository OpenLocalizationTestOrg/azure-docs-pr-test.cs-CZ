---
title: "Prostřednictvím kódu programu Sledování úloh v Stream Analytics | Microsoft Docs"
description: "Naučte se monitorovat prostřednictvím kódu programu úlohy Stream Analytics vytvořené pomocí rozhraní REST API, Azure SDK nebo prostředí PowerShell."
keywords: "monitorování .net, monitorování úloh monitorování aplikace"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 2ec02cc9-4ca5-4a25-ae60-c44be9ad4835
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: samacha
ms.openlocfilehash: 7e9d2f6f03fd539c59b105108fb46697bcd60f1c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="programmatically-create-a-stream-analytics-job-monitor"></a>Vytváření monitorování úlohy Stream Analytics prostřednictvím kódu programu

Tento článek ukazuje, jak povolit monitorování v rámci úlohy Stream Analytics. Úlohy analýzy datového proudu, které jsou vytvořené pomocí rozhraní REST API, Azure SDK nebo prostředí PowerShell nemají povoleno ve výchozím nastavení monitorování. Můžete ručně ji povolit na portálu Azure přejděte na stránku úlohy monitorování a kliknutím na tlačítko Povolit nebo tento proces můžete automatizovat pomocí kroků v tomto článku. Data sledování se zobrazí v oblasti metriky portál Azure pro vaše úloha Stream Analytics.

## <a name="prerequisites"></a>Požadavky

Před zahájením tohoto procesu, musíte mít následující:

* Visual Studio 2017 nebo 2015
* [Azure .NET SDK](https://azure.microsoft.com/downloads/) staženy a nainstalovány
* Existující úloha Stream Analytics, který musí mít povoleno monitorování

## <a name="create-a-project"></a>Vytvoření projektu

1. Vytvořte konzolovou aplikaci Visual Studio C# .NET.
2. V konzole Správce balíčků spusťte následující příkazy instalace balíčků NuGet. První z nich je Azure Stream Analytics správu .NET SDK. Druhá je Azure SDK pro monitorování, který se použije k povolení monitorování. Poslední je klient Azure Active Directory, který se použije pro ověřování.
   
   ```
   Install-Package Microsoft.Azure.Management.StreamAnalytics
   Install-Package Microsoft.Azure.Insights -Pre
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```
3. Přidejte následující oddíl appSettings do souboru App.config.
   
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
   Nahraďte hodnoty pro *SubscriptionId* a *ActiveDirectoryTenantId* s Azure ID předplatného a klienta. Tyto hodnoty můžete získat spuštěním následující rutiny prostředí PowerShell:
   
   ```
   Get-AzureAccount
   ```
4. Přidejte následující příkazy ke zdrojovému souboru (Program.cs) v projektu.
   
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
5. Přidejte metodu helper ověřování.
   
     Veřejné statické řetězec GetAuthorizationHeader()
   
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
     }

## <a name="create-management-clients"></a>Vytvoření klientů pro správu

Následující kód nastaví nezbytné proměnné a správy klientů.

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

## <a name="enable-monitoring-for-an-existing-stream-analytics-job"></a>Povolit monitorování pro existující úlohy Stream Analytics

Následující kód umožňuje monitorování pro **existující** úlohy služby Stream Analytics. První část kód provede požadavek GET službu Stream Analytics k načtení informací o konkrétní úloze Stream Analytics. Použije *Id* vlastnost (získané z požadavek GET) jako parametr pro metodu Put ve druhé polovině kódu, který odesílá PUT žádost o službě Statistika povolení monitorování pro úlohu služby Stream Analytics.

>[!WARNING]
>Pokud jste dříve povolili monitorování pro různé úlohy služby Stream Analytics, buď prostřednictvím portálu Azure nebo prostřednictvím kódu programu níže uvedeného kódu, **doporučujeme zadat stejný název účtu úložiště, který jste použili, když jste povolili dříve monitorování.**
> 
> Účet úložiště souvisí oblast, které jste vytvořili vaší úloze Stream Analytics, není určený speciálně pro úlohy sám sebe.
> 
> Všechny služby Stream Analytics úlohy (a všechny ostatní prostředky služby Azure) v této oblasti stejné sdílet tento účet úložiště pro ukládání dat monitorování. Pokud zadáte jiný účet úložiště, se může způsobit nečekané vedlejší účinky v sledování jiné úlohy Stream Analytics nebo jiných prostředků Azure.
> 
> Název účtu úložiště, který můžete použít k nahrazení `<YOUR STORAGE ACCOUNT NAME>` v následujícím kódu by měla být účet úložiště, který je ve stejném předplatném jako úlohu služby Stream Analytics, který chcete povolit monitorování.
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



## <a name="get-support"></a>Získat podporu

Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky

* [Úvod do služby Azure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

