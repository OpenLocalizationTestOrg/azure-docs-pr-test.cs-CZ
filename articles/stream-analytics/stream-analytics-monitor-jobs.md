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
# <a name="programmatically-create-a-stream-analytics-job-monitor"></a>Vytváření monitorování úlohy Stream Analytics prostřednictvím kódu programu

Tento článek ukazuje, jak tooenable monitorování u úlohy Stream Analytics. Úlohy analýzy datového proudu, které jsou vytvořené pomocí rozhraní REST API, Azure SDK nebo prostředí PowerShell nemají povoleno ve výchozím nastavení monitorování. Můžete ručně ji povolit v hello portálu Azure tak, že přejdete na stránku toohello úlohy monitorování a hello kliknutím na tlačítko Povolit nebo tento proces můžete automatizovat pomocí následujících kroků hello v tomto článku. Hello dat monitorování se zobrazí v oblasti metriky hello hello portál Azure pro vaše úloha Stream Analytics.

## <a name="prerequisites"></a>Požadavky

Před zahájením tohoto procesu, musíte mít následující hello:

* Visual Studio 2017 nebo 2015
* [Azure .NET SDK](https://azure.microsoft.com/downloads/) staženy a nainstalovány
* Existující úlohy Stream Analytics vyžadující toohave monitoring je povolena.

## <a name="create-a-project"></a>Vytvoření projektu

1. Vytvořte konzolovou aplikaci Visual Studio C# .NET.
2. V hello Konzola správce balíčků hello spusťte následující příkazy balíčky NuGet tooinstall hello. Hello nejprve jeden je hello .NET SDK služby Azure Stream Analytics správy. Hello druhá je hello Azure SDK monitorování, který se použije tooenable monitorování. Hello poslední jedním je hello klienta Azure Active Directory, který se použije pro ověřování.
   
   ```
   Install-Package Microsoft.Azure.Management.StreamAnalytics
   Install-Package Microsoft.Azure.Insights -Pre
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```
3. Přidejte následující soubor App.config toohello oddílu appSettings hello.
   
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
   Nahraďte hodnoty pro *SubscriptionId* a *ActiveDirectoryTenantId* s Azure ID předplatného a klienta. Tyto hodnoty můžete získat spuštěním následující rutiny prostředí PowerShell hello:
   
   ```
   Get-AzureAccount
   ```
4. Přidejte následující hello pomocí příkazů toohello zdrojový soubor (Program.cs) v projektu hello.
   
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
   
             throw new InvalidOperationException("Failed tooacquire token");
     }

## <a name="create-management-clients"></a>Vytvoření klientů pro správu

Hello následující kód bude nastavení hello nezbytné proměnné a správy klientů.

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

Hello následující kód povolí monitorování **existující** úlohy služby Stream Analytics. první část Hello hello kód provede požadavek GET hello Stream Analytics služby tooretrieve informace o konkrétní úloze Stream Analytics hello. Používá hello *Id* vlastnost (získané z požadavek GET hello) jako parametr pro hello metodu Put v hello druhé polovině tématu hello kód, který odešle požadavek PUT toohello Statistika služby tooenable monitorování hello Stream Analytics úloha.

>[!WARNING]
>Pokud jste dříve povolili monitorování pro různé úlohy služby Stream Analytics, buď prostřednictvím hello portál Azure nebo prostřednictvím kódu programu hello níže uvedeného kódu, **doporučujeme zadat hello stejný název účtu úložiště, můžete použít, když jste dřív povolené monitorování.**
> 
> účet úložiště Hello je propojené toohello oblast, který jste vytvořili vaší úloze Stream Analytics v není konkrétně úlohy toohello sám sebe.
> 
> Všechny služby Stream Analytics úlohy (a všechny ostatní prostředky služby Azure) v této oblasti stejné sdílet tento toostore účet úložiště dat monitorování. Pokud zadáte jiný účet úložiště, se může způsobit nečekané vedlejší účinky v hello sledování jiné úlohy Stream Analytics nebo jiných prostředků Azure.
> 
> název účtu úložiště Hello použít tooreplace `<YOUR STORAGE ACCOUNT NAME>` hello následující kód musí být účet úložiště, který je v hello stejnému předplatnému jako úlohy služby Stream Analytics hello, který chcete povolit monitorování.
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

* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

