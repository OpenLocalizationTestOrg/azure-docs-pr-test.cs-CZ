---
title: "pomocí sady SDK rozhraní .NET HDInsight - Azure úloh MapReduce aaaSubmit | Microsoft Docs"
description: "Zjistěte, jak toosubmit MapReduce úlohy tooAzure HDInsight Hadoop pomocí sady .NET SDK HDInsight."
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
ms.openlocfilehash: d00e31400b8fa47982c31d00bfdcdb304bcb0b59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a><span data-ttu-id="01f4c-103">Spuštění úloh MapReduce pomocí sady .NET SDK HDInsight</span><span class="sxs-lookup"><span data-stu-id="01f4c-103">Run MapReduce jobs using HDInsight .NET SDK</span></span>
[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

<span data-ttu-id="01f4c-104">Zjistěte, jak toosubmit MapReduce úlohy pomocí sady .NET SDK HDInsight.</span><span class="sxs-lookup"><span data-stu-id="01f4c-104">Learn how toosubmit MapReduce jobs using HDInsight .NET SDK.</span></span> <span data-ttu-id="01f4c-105">HDInsight clustery jsou součástí soubor jar se některé ukázky MapReduce.</span><span class="sxs-lookup"><span data-stu-id="01f4c-105">HDInsight clusters come with a jar file with some MapReduce samples.</span></span> <span data-ttu-id="01f4c-106">soubor jar Hello je */example/jars/hadoop-mapreduce-examples.jar*.</span><span class="sxs-lookup"><span data-stu-id="01f4c-106">hello jar file is */example/jars/hadoop-mapreduce-examples.jar*.</span></span>  <span data-ttu-id="01f4c-107">Jeden z ukázky hello je *wordcount*.</span><span class="sxs-lookup"><span data-stu-id="01f4c-107">One of hello samples is *wordcount*.</span></span> <span data-ttu-id="01f4c-108">Můžete vyvíjet C# konzole aplikace toosubmit úlohu wordcount.</span><span class="sxs-lookup"><span data-stu-id="01f4c-108">You develop a C# console application toosubmit a wordcount job.</span></span>  <span data-ttu-id="01f4c-109">Úloha Hello čte hello */example/data/gutenberg/davinci.txt* souboru a vydává hello výsledky příliš*/example/data/davinciwordcount*.</span><span class="sxs-lookup"><span data-stu-id="01f4c-109">hello job reads hello */example/data/gutenberg/davinci.txt* file, and outputs hello results too*/example/data/davinciwordcount*.</span></span>  <span data-ttu-id="01f4c-110">Pokud chcete aplikace hello toorerun, musí vyčistit hello výstupní složky.</span><span class="sxs-lookup"><span data-stu-id="01f4c-110">If you want toorerun hello application, you must clean up hello output folder.</span></span>

> [!NOTE]
> <span data-ttu-id="01f4c-111">Hello kroky v tomto článku je potřeba provést z klienta Windows.</span><span class="sxs-lookup"><span data-stu-id="01f4c-111">hello steps in this article must be performed from a Windows client.</span></span> <span data-ttu-id="01f4c-112">Informace o systému Linux, OS X nebo Unix klienta toowork pomocí Hive použijte selektor karta hello zobrazený na hello horní části článku hello.</span><span class="sxs-lookup"><span data-stu-id="01f4c-112">For information on using a Linux, OS X, or Unix client toowork with Hive, use hello tab selector shown on hello top of hello article.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="01f4c-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="01f4c-113">Prerequisites</span></span>
<span data-ttu-id="01f4c-114">Před zahájením tohoto článku, musíte mít hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="01f4c-114">Before you begin this article, you must have hello following items:</span></span>

* <span data-ttu-id="01f4c-115">**Cluster Hadoop v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="01f4c-115">**A Hadoop cluster in HDInsight**.</span></span> <span data-ttu-id="01f4c-116">V tématu [začít používat systémem Linux Hadoop v HDInsight](./hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="01f4c-116">See [Get started using Linux-based Hadoop in HDInsight](./hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
* <span data-ttu-id="01f4c-117">**Visual Studio 2013 nebo 2015 nebo 2017**.</span><span class="sxs-lookup"><span data-stu-id="01f4c-117">**Visual Studio 2013/2015/2017**.</span></span>

## <a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a><span data-ttu-id="01f4c-118">Odesílání úloh MapReduce pomocí sady .NET SDK HDInsight</span><span class="sxs-lookup"><span data-stu-id="01f4c-118">Submit MapReduce jobs using HDInsight .NET SDK</span></span>
<span data-ttu-id="01f4c-119">Hello HDInsight .NET SDK poskytuje klientské knihovny .NET, takže je jednodušší toowork s clustery HDInsight pomocí technologie .NET.</span><span class="sxs-lookup"><span data-stu-id="01f4c-119">hello HDInsight .NET SDK provides .NET client libraries, which makes it easier toowork with HDInsight clusters from .NET.</span></span> 

<span data-ttu-id="01f4c-120">**tooSubmit úlohy**</span><span class="sxs-lookup"><span data-stu-id="01f4c-120">**tooSubmit jobs**</span></span>

1. <span data-ttu-id="01f4c-121">Vytvořte konzolovou aplikaci C# v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01f4c-121">Create a C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="01f4c-122">Z konzoly Správce balíčků Nuget hello spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="01f4c-122">From hello Nuget Package Manager Console, run hello following command:</span></span>
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. <span data-ttu-id="01f4c-123">Použijte hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="01f4c-123">Use hello following code:</span></span>
   
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
                    System.Console.WriteLine("hello application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = existingClusterUsername, Password = existingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(existingClusterUri, clusterCredentials);
   
                    SubmitMRJob();
   
                    System.Console.WriteLine("Press ENTER toocontinue ...");
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
   
                    System.Console.WriteLine("Submitting hello MR job toohello cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitMapReduceJob(paras);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);
   
                    System.Console.WriteLine("Waiting for hello job completion ...");
   
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
                        // Create hello storage account object
                        CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=" + 
                            defaultStorageAccountName + 
                            ";AccountKey=" + defaultStorageAccountKey);
        
                        // Create hello blob client.
                        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        
                        // Retrieve reference tooa previously created container.
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
4. <span data-ttu-id="01f4c-124">Stiskněte klávesu **F5** toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="01f4c-124">Press **F5** toorun hello application.</span></span>

<span data-ttu-id="01f4c-125">toorun hello úlohu znovu, musíte změnit hello úlohy výstup název složky, v ukázce hello, je "/ Příklad/data/davinciwordcount".</span><span class="sxs-lookup"><span data-stu-id="01f4c-125">toorun hello job again, you must change hello job output folder name, in hello sample, it is "/example/data/davinciwordcount".</span></span>

<span data-ttu-id="01f4c-126">Po úspěšném dokončení úlohy hello aplikace hello vytiskne obsah hello hello výstupního souboru "část r-00000".</span><span class="sxs-lookup"><span data-stu-id="01f4c-126">When hello job completes successfully, hello application prints hello content of hello output file "part-r-00000".</span></span>

## <a name="next-steps"></a><span data-ttu-id="01f4c-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="01f4c-127">Next steps</span></span>
<span data-ttu-id="01f4c-128">V tomto článku jste se naučili několik způsobů toocreate clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="01f4c-128">In this article, you have learned several ways toocreate an HDInsight cluster.</span></span> <span data-ttu-id="01f4c-129">toolearn více, najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="01f4c-129">toolearn more, see hello following articles:</span></span>

* <span data-ttu-id="01f4c-130">Odeslání úlohy Hive, najdete v části [spouštění dotazů Hive pomocí sady .NET SDK HDInsight](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="01f4c-130">For submitting a Hive job, see [Run Hive queries using HDInsight .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span></span>
* <span data-ttu-id="01f4c-131">Vytváření clusterů HDInsight, naleznete v části [vytvořit systémem Linux Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="01f4c-131">For creating HDInsight clusters, see [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>
* <span data-ttu-id="01f4c-132">Správa clusterů HDInsight, naleznete v části [spravovat Hadoop clusterů v HDInsight](hdinsight-administer-use-portal-linux.md).</span><span class="sxs-lookup"><span data-stu-id="01f4c-132">For managing HDInsight clusters, see [Manage Hadoop clusters in HDInsight](hdinsight-administer-use-portal-linux.md).</span></span>
* <span data-ttu-id="01f4c-133">Učení hello SDK rozhraní .NET HDInsight, naleznete v části [referenční informace sady SDK rozhraní .NET HDInsight](https://msdn.microsoft.com/library/mt271028.aspx).</span><span class="sxs-lookup"><span data-stu-id="01f4c-133">For learning hello HDInsight .NET SDK, see [HDInsight .NET SDK reference](https://msdn.microsoft.com/library/mt271028.aspx).</span></span>
* <span data-ttu-id="01f4c-134">Pro neinteraktivní ověřování tooAzure najdete v tématu [vytvořit neinteraktivního ověřování aplikace .NET HDInsight](hdinsight-create-non-interactive-authentication-dotnet-applications.md).</span><span class="sxs-lookup"><span data-stu-id="01f4c-134">For non-interactive authenticate tooAzure, see [Create non-interactive authentication .NET HDInsight applications](hdinsight-create-non-interactive-authentication-dotnet-applications.md).</span></span>

