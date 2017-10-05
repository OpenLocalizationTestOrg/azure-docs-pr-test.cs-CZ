---
title: "Odesílání úloh MapReduce pomocí sady SDK rozhraní .NET HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak k odesílání úloh MapReduce do Azure HDInsight Hadoop pomocí sady .NET SDK HDInsight."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: c85e44b0-85fd-4185-ad1c-c34a9fe5ef44
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: jgao
ms.openlocfilehash: 015435270c31bafea0ebf5303b459338755c1410
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a><span data-ttu-id="6cc2d-103">Spuštění úloh MapReduce pomocí sady .NET SDK HDInsight</span><span class="sxs-lookup"><span data-stu-id="6cc2d-103">Run MapReduce jobs using HDInsight .NET SDK</span></span>
[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

<span data-ttu-id="6cc2d-104">Zjistěte, jak k odesílání úloh MapReduce pomocí sady .NET SDK HDInsight.</span><span class="sxs-lookup"><span data-stu-id="6cc2d-104">Learn how to submit MapReduce jobs using HDInsight .NET SDK.</span></span> <span data-ttu-id="6cc2d-105">HDInsight clustery jsou součástí soubor jar se některé ukázky MapReduce.</span><span class="sxs-lookup"><span data-stu-id="6cc2d-105">HDInsight clusters come with a jar file with some MapReduce samples.</span></span> <span data-ttu-id="6cc2d-106">Je na soubor jar */example/jars/hadoop-mapreduce-examples.jar*.</span><span class="sxs-lookup"><span data-stu-id="6cc2d-106">The jar file is */example/jars/hadoop-mapreduce-examples.jar*.</span></span>  <span data-ttu-id="6cc2d-107">Jeden z ukázky je *wordcount*.</span><span class="sxs-lookup"><span data-stu-id="6cc2d-107">One of the samples is *wordcount*.</span></span> <span data-ttu-id="6cc2d-108">Můžete vyvíjet konzolovou aplikaci C# se odeslat úlohu wordcount.</span><span class="sxs-lookup"><span data-stu-id="6cc2d-108">You develop a C# console application to submit a wordcount job.</span></span>  <span data-ttu-id="6cc2d-109">Načte úlohu */example/data/gutenberg/davinci.txt* souboru a vrací výsledky do */example/data/davinciwordcount*.</span><span class="sxs-lookup"><span data-stu-id="6cc2d-109">The job reads the */example/data/gutenberg/davinci.txt* file, and outputs the results to */example/data/davinciwordcount*.</span></span>  <span data-ttu-id="6cc2d-110">Pokud chcete znovu spusťte aplikaci, musí vyčistit do výstupní složky.</span><span class="sxs-lookup"><span data-stu-id="6cc2d-110">If you want to rerun the application, you must clean up the output folder.</span></span>

> [!NOTE]
> <span data-ttu-id="6cc2d-111">Kroky v tomto článku je potřeba provést z klienta Windows.</span><span class="sxs-lookup"><span data-stu-id="6cc2d-111">The steps in this article must be performed from a Windows client.</span></span> <span data-ttu-id="6cc2d-112">Informace o používání Linux, OS X nebo Unix klienta pro práci s Hive použijte volič karty v horní článek ukazuje.</span><span class="sxs-lookup"><span data-stu-id="6cc2d-112">For information on using a Linux, OS X, or Unix client to work with Hive, use the tab selector shown on the top of the article.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="6cc2d-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6cc2d-113">Prerequisites</span></span>
<span data-ttu-id="6cc2d-114">Před zahájením tohoto článku, musíte mít následující položky:</span><span class="sxs-lookup"><span data-stu-id="6cc2d-114">Before you begin this article, you must have the following items:</span></span>

* <span data-ttu-id="6cc2d-115">**Cluster Hadoop v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="6cc2d-115">**A Hadoop cluster in HDInsight**.</span></span> <span data-ttu-id="6cc2d-116">V tématu [začít používat systémem Linux Hadoop v HDInsight](./hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="6cc2d-116">See [Get started using Linux-based Hadoop in HDInsight](./hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
* <span data-ttu-id="6cc2d-117">**Visual Studio 2013 nebo 2015 nebo 2017**.</span><span class="sxs-lookup"><span data-stu-id="6cc2d-117">**Visual Studio 2013/2015/2017**.</span></span>

## <a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a><span data-ttu-id="6cc2d-118">Odesílání úloh MapReduce pomocí sady .NET SDK HDInsight</span><span class="sxs-lookup"><span data-stu-id="6cc2d-118">Submit MapReduce jobs using HDInsight .NET SDK</span></span>
<span data-ttu-id="6cc2d-119">.NET SDK služby HDInsight poskytuje klientské knihovny .NET, která usnadňuje práci s clustery HDInsight pomocí technologie .NET.</span><span class="sxs-lookup"><span data-stu-id="6cc2d-119">The HDInsight .NET SDK provides .NET client libraries, which makes it easier to work with HDInsight clusters from .NET.</span></span> 

<span data-ttu-id="6cc2d-120">**K odesílání úloh**</span><span class="sxs-lookup"><span data-stu-id="6cc2d-120">**To Submit jobs**</span></span>

1. <span data-ttu-id="6cc2d-121">Vytvořte konzolovou aplikaci C# v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6cc2d-121">Create a C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="6cc2d-122">Z konzoly Správce balíčků Nuget spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6cc2d-122">From the Nuget Package Manager Console, run the following command:</span></span>
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. <span data-ttu-id="6cc2d-123">Použijte následující kód:</span><span class="sxs-lookup"><span data-stu-id="6cc2d-123">Use the following code:</span></span>
   
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;

        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;
   
                private const string existingClusterName = "<Your HDInsight Cluster Name>";
                private const string existingClusterUri = existingClusterName + ".azurehdinsight.net";
                private const string existingClusterUsername = "<Cluster Username>";
                private const string existingClusterPassword = "<Cluster User Password>";
   
                private const string defaultStorageAccountName = "<Default Storage Account Name>"; //<StorageAccountName>.blob.core.windows.net
                private const string defaultStorageAccountKey = "<Default Storage Account Key>";
                private const string defaultStorageContainerName = "<Default Blob Container Name>";

                private const string sourceFile = "/example/data/gutenberg/davinci.txt";  
                private const string outputFolder = "/example/data/davinciwordcount";
   
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = existingClusterUsername, Password = existingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(existingClusterUri, clusterCredentials);
   
                    SubmitMRJob();
   
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
   
                private static void SubmitMRJob()
                {
                    List<string> args = new List<string> { { "/example/data/gutenberg/davinci.txt" }, { "/example/data/davinciwordcount" } };
   
                    var paras = new MapReduceJobSubmissionParameters
                    {
                        JarFile = @"/example/jars/hadoop-mapreduce-examples.jar",
                        JarClass = "wordcount",
                        Arguments = args
                    };
   
                    System.Console.WriteLine("Submitting the MR job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitMapReduceJob(paras);
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
                    System.Console.WriteLine("Job output is: ");
                    var storageAccess = new AzureStorageAccess(defaultStorageAccountName, defaultStorageAccountKey,
                        defaultStorageContainerName);
        
                    if (jobDetail.ExitValue == 0)
                    {
                        // Create the storage account object
                        CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=" + 
                            defaultStorageAccountName + 
                            ";AccountKey=" + defaultStorageAccountKey);
        
                        // Create the blob client.
                        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        
                        // Retrieve reference to a previously created container.
                        CloudBlobContainer container = blobClient.GetContainerReference(defaultStorageContainerName);
        
                        CloudBlockBlob blockBlob = container.GetBlockBlobReference(outputFolder.Substring(1) + "/part-r-00000");
        
                        using (var stream = blockBlob.OpenRead())
                        {
                            using (StreamReader reader = new StreamReader(stream))
                            {
                                while (!reader.EndOfStream)
                                {
                                    System.Console.WriteLine(reader.ReadLine());
                                }
                            }
                        }
                    }
                    else
                    {
                        // fetch stderr output in case of failure
                        var output = _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); 
        
                        using (var reader = new StreamReader(output, Encoding.UTF8))
                        {
                            string value = reader.ReadToEnd();
                            System.Console.WriteLine(value);
                        }
        
                    }
                }
            }
        }
4. <span data-ttu-id="6cc2d-124">Stisknutím klávesy **F5** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6cc2d-124">Press **F5** to run the application.</span></span>

<span data-ttu-id="6cc2d-125">Chcete-li úlohu znovu spustit, musíte změnit název složky výstup úlohy, v ukázce je "/ Příklad/data/davinciwordcount".</span><span class="sxs-lookup"><span data-stu-id="6cc2d-125">To run the job again, you must change the job output folder name, in the sample, it is "/example/data/davinciwordcount".</span></span>

<span data-ttu-id="6cc2d-126">Po úspěšném dokončení úlohy aplikace vytiskne obsah výstupní soubor "část r-00000".</span><span class="sxs-lookup"><span data-stu-id="6cc2d-126">When the job completes successfully, the application prints the content of the output file "part-r-00000".</span></span>

## <a name="next-steps"></a><span data-ttu-id="6cc2d-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6cc2d-127">Next steps</span></span>
<span data-ttu-id="6cc2d-128">V tomto článku jste se naučili několik způsobů, jak vytvořit cluster služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="6cc2d-128">In this article, you have learned several ways to create an HDInsight cluster.</span></span> <span data-ttu-id="6cc2d-129">Další informace naleznete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="6cc2d-129">To learn more, see the following articles:</span></span>

* <span data-ttu-id="6cc2d-130">Odeslání úlohy Hive, najdete v části [spouštění dotazů Hive pomocí sady .NET SDK HDInsight](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="6cc2d-130">For submitting a Hive job, see [Run Hive queries using HDInsight .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span></span>
* <span data-ttu-id="6cc2d-131">Vytváření clusterů HDInsight, naleznete v části [vytvořit systémem Linux Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="6cc2d-131">For creating HDInsight clusters, see [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>
* <span data-ttu-id="6cc2d-132">Správa clusterů HDInsight, naleznete v části [spravovat Hadoop clusterů v HDInsight](hdinsight-administer-use-portal-linux.md).</span><span class="sxs-lookup"><span data-stu-id="6cc2d-132">For managing HDInsight clusters, see [Manage Hadoop clusters in HDInsight](hdinsight-administer-use-portal-linux.md).</span></span>
* <span data-ttu-id="6cc2d-133">Učení .NET SDK služby HDInsight, naleznete v části [referenční informace sady SDK rozhraní .NET HDInsight](https://msdn.microsoft.com/library/mt271028.aspx).</span><span class="sxs-lookup"><span data-stu-id="6cc2d-133">For learning the HDInsight .NET SDK, see [HDInsight .NET SDK reference](https://msdn.microsoft.com/library/mt271028.aspx).</span></span>
* <span data-ttu-id="6cc2d-134">Pro neinteraktivní ověřování v Azure najdete v tématu [vytvořit neinteraktivního ověřování aplikace .NET HDInsight](hdinsight-create-non-interactive-authentication-dotnet-applications.md).</span><span class="sxs-lookup"><span data-stu-id="6cc2d-134">For non-interactive authenticate to Azure, see [Create non-interactive authentication .NET HDInsight applications](hdinsight-create-non-interactive-authentication-dotnet-applications.md).</span></span>

