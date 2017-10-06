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
# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a>Spuštění úloh MapReduce pomocí sady .NET SDK HDInsight
[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Zjistěte, jak toosubmit MapReduce úlohy pomocí sady .NET SDK HDInsight. HDInsight clustery jsou součástí soubor jar se některé ukázky MapReduce. soubor jar Hello je */example/jars/hadoop-mapreduce-examples.jar*.  Jeden z ukázky hello je *wordcount*. Můžete vyvíjet C# konzole aplikace toosubmit úlohu wordcount.  Úloha Hello čte hello */example/data/gutenberg/davinci.txt* souboru a vydává hello výsledky příliš*/example/data/davinciwordcount*.  Pokud chcete aplikace hello toorerun, musí vyčistit hello výstupní složky.

> [!NOTE]
> Hello kroky v tomto článku je potřeba provést z klienta Windows. Informace o systému Linux, OS X nebo Unix klienta toowork pomocí Hive použijte selektor karta hello zobrazený na hello horní části článku hello.
> 
> 

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto článku, musíte mít hello následující položky:

* **Cluster Hadoop v HDInsight**. V tématu [začít používat systémem Linux Hadoop v HDInsight](./hdinsight-hadoop-linux-tutorial-get-started.md).
* **Visual Studio 2013 nebo 2015 nebo 2017**.

## <a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a>Odesílání úloh MapReduce pomocí sady .NET SDK HDInsight
Hello HDInsight .NET SDK poskytuje klientské knihovny .NET, takže je jednodušší toowork s clustery HDInsight pomocí technologie .NET. 

**tooSubmit úlohy**

1. Vytvořte konzolovou aplikaci C# v sadě Visual Studio.
2. Z konzoly Správce balíčků Nuget hello spusťte následující příkaz hello:
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. Použijte hello následující kód:
   
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
4. Stiskněte klávesu **F5** toorun hello aplikace.

toorun hello úlohu znovu, musíte změnit hello úlohy výstup název složky, v ukázce hello, je "/ Příklad/data/davinciwordcount".

Po úspěšném dokončení úlohy hello aplikace hello vytiskne obsah hello hello výstupního souboru "část r-00000".

## <a name="next-steps"></a>Další kroky
V tomto článku jste se naučili několik způsobů toocreate clusteru služby HDInsight. toolearn více, najdete v části hello následující články:

* Odeslání úlohy Hive, najdete v části [spouštění dotazů Hive pomocí sady .NET SDK HDInsight](hdinsight-hadoop-use-hive-dotnet-sdk.md).
* Vytváření clusterů HDInsight, naleznete v části [vytvořit systémem Linux Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
* Správa clusterů HDInsight, naleznete v části [spravovat Hadoop clusterů v HDInsight](hdinsight-administer-use-portal-linux.md).
* Učení hello SDK rozhraní .NET HDInsight, naleznete v části [referenční informace sady SDK rozhraní .NET HDInsight](https://msdn.microsoft.com/library/mt271028.aspx).
* Pro neinteraktivní ověřování tooAzure najdete v tématu [vytvořit neinteraktivního ověřování aplikace .NET HDInsight](hdinsight-create-non-interactive-authentication-dotnet-applications.md).

