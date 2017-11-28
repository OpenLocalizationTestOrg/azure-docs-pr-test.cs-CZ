---
title: "Spouštění dotazů Hive pomocí sady SDK rozhraní .NET HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak k odesílání úloh Hadoop do HDInsight Hadoop Azure pomocí sady .NET SDK HDInsight."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 4e291890-f8b4-426c-b5e8-d4fd512ff042
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: jgao
ms.openlocfilehash: 7b1a5f7ea3b2bda438727dc75a85557ea7930280
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="run-hive-queries-using-hdinsight-net-sdk"></a><span data-ttu-id="51233-103">Spouštění dotazů Hive pomocí sady .NET SDK HDInsight</span><span class="sxs-lookup"><span data-stu-id="51233-103">Run Hive queries using HDInsight .NET SDK</span></span>
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="51233-104">Zjistěte, jak k odesílání dotazů Hive pomocí sady .NET SDK HDInsight.</span><span class="sxs-lookup"><span data-stu-id="51233-104">Learn how to submit Hive queries using HDInsight .NET SDK.</span></span> <span data-ttu-id="51233-105">Zápis programu C# k odesílání dotazů Hive pro výpis tabulek Hive a zobrazit výsledky.</span><span class="sxs-lookup"><span data-stu-id="51233-105">You write a C# program to submit a Hive query for listing Hive tables, and display the results.</span></span>

> [!NOTE]
> <span data-ttu-id="51233-106">Kroky v tomto článku je potřeba provést z klienta Windows.</span><span class="sxs-lookup"><span data-stu-id="51233-106">The steps in this article must be performed from a Windows client.</span></span> <span data-ttu-id="51233-107">Informace o používání Linux, OS X nebo Unix klienta pro práci s Hive použijte volič karty v horní článek ukazuje.</span><span class="sxs-lookup"><span data-stu-id="51233-107">For information on using a Linux, OS X, or Unix client to work with Hive, use the tab selector shown on the top of the article.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="51233-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="51233-108">Prerequisites</span></span>
<span data-ttu-id="51233-109">Před zahájením tohoto článku, musíte mít následující položky:</span><span class="sxs-lookup"><span data-stu-id="51233-109">Before you begin this article, you must have the following items:</span></span>

* <span data-ttu-id="51233-110">**Cluster Hadoop v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="51233-110">**A Hadoop cluster in HDInsight**.</span></span> <span data-ttu-id="51233-111">V tématu [začít používat systémem Linux Hadoop v HDInsight](./hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="51233-111">See [Get started using Linux-based Hadoop in HDInsight](./hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
* <span data-ttu-id="51233-112">**Visual Studio 2013 nebo 2015 nebo 2017**.</span><span class="sxs-lookup"><span data-stu-id="51233-112">**Visual Studio 2013/2015/2017**.</span></span>

## <a name="submit-hive-queries-using-hdinsight-net-sdk"></a><span data-ttu-id="51233-113">Odesílání dotazů Hive pomocí sady .NET SDK HDInsight</span><span class="sxs-lookup"><span data-stu-id="51233-113">Submit Hive queries using HDInsight .NET SDK</span></span>
<span data-ttu-id="51233-114">.NET SDK služby HDInsight poskytuje klientské knihovny .NET, která usnadňuje práci s clustery HDInsight pomocí technologie .NET.</span><span class="sxs-lookup"><span data-stu-id="51233-114">The HDInsight .NET SDK provides .NET client libraries, which makes it easier to work with HDInsight clusters from .NET.</span></span> 

<span data-ttu-id="51233-115">**K odesílání úloh**</span><span class="sxs-lookup"><span data-stu-id="51233-115">**To Submit jobs**</span></span>

1. <span data-ttu-id="51233-116">Vytvořte konzolovou aplikaci C# v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="51233-116">Create a C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="51233-117">Z konzoly Správce balíčků Nuget spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="51233-117">From the Nuget Package Manager Console, run the following command:</span></span>
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. <span data-ttu-id="51233-118">Použijte následující kód:</span><span class="sxs-lookup"><span data-stu-id="51233-118">Use the following code:</span></span>

    ```csharp
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
   
        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;
   
                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";
   
                private const string DefaultStorageAccountName = "<Default Storage Account Name>";
                private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
                private const string DefaultStorageContainerName = "<Default Blob Container Name>";
   
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
   
                    SubmitHiveJob();
   
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
   
                private static void SubmitHiveJob()
                {
                    Dictionary<string, string> defines = new Dictionary<string, string> { { "hive.execution.engine", "tez" }, { "hive.exec.reducers.max", "1" } };
                    List<string> args = new List<string> { { "argA" }, { "argB" } };
                    var parameters = new HiveJobSubmissionParameters
                    {
                        Query = "SHOW TABLES",
                        Defines = defines,
                        Arguments = args
                    };
   
                    System.Console.WriteLine("Submitting the Hive job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);
   
                    System.Console.WriteLine("Waiting for the job completion ...");
   
                    // Wait for job completion
                    var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    while (!jobDetail.Status.JobComplete)
                    {
                        Thread.Sleep(1000);
                        jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    }
   
                    // Get job output
                    var storageAccess = new AzureStorageAccess(DefaultStorageAccountName, DefaultStorageAccountKey,
                        DefaultStorageContainerName);
                    var output = (jobDetail.ExitValue == 0)
                        ? _hdiJobManagementClient.JobManagement.GetJobOutput(jobId, storageAccess) // fetch stdout output in case of success
                        : _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); // fetch stderr output in case of failure
   
                    System.Console.WriteLine("Job output is: ");
   
                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
                }
            }
        }
    ```
4. <span data-ttu-id="51233-119">Stisknutím klávesy **F5** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="51233-119">Press **F5** to run the application.</span></span>

<span data-ttu-id="51233-120">Výstup aplikace musí být podobně jako:</span><span class="sxs-lookup"><span data-stu-id="51233-120">The output of the application shall be similar to:</span></span>

![HDInsight Hadoop Hive výstup úlohy](./media/hdinsight-hadoop-use-hive-dotnet-sdk/hdinsight-hadoop-use-hive-net-sdk-output.png)

## <a name="next-steps"></a><span data-ttu-id="51233-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="51233-122">Next steps</span></span>
<span data-ttu-id="51233-123">V tomto článku jste se naučili několik způsobů, jak vytvořit cluster služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="51233-123">In this article, you have learned several ways to create an HDInsight cluster.</span></span> <span data-ttu-id="51233-124">Další informace naleznete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="51233-124">To learn more, see the following articles:</span></span>

* <span data-ttu-id="51233-125">[Začínáme se službou Azure HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="51233-125">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="51233-126">[Vytvoření clusterů systému Hadoop v HDInsight][hdinsight-provision]</span><span class="sxs-lookup"><span data-stu-id="51233-126">[Create Hadoop clusters in HDInsight][hdinsight-provision]</span></span>
* [<span data-ttu-id="51233-127">Správa clusterů systému Hadoop v HDInsight pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="51233-127">Manage Hadoop clusters in HDInsight by using the Azure portal</span></span>](hdinsight-administer-use-management-portal.md)
* [<span data-ttu-id="51233-128">Referenční informace sady SDK rozhraní .NET HDInsight</span><span class="sxs-lookup"><span data-stu-id="51233-128">HDInsight .NET SDK reference</span></span>](https://msdn.microsoft.com/library/mt271028.aspx)
* [<span data-ttu-id="51233-129">Použití Pigu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="51233-129">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="51233-130">Použití nástroje Sqoop s HDInsight</span><span class="sxs-lookup"><span data-stu-id="51233-130">Use Sqoop with HDInsight</span></span>](hdinsight-use-sqoop-mac-linux.md)
* [<span data-ttu-id="51233-131">Vytvoření aplikací .NET HDInsight pro neinteraktivní ověřování</span><span class="sxs-lookup"><span data-stu-id="51233-131">Create non-interactive authentication .NET HDInsight applications</span></span>](hdinsight-create-non-interactive-authentication-dotnet-applications.md)

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md


