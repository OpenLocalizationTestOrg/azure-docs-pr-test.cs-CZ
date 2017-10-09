---
title: "aaaRun dotazů Hive pomocí sady SDK rozhraní .NET HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toosubmit Hadoop úlohy tooAzure HDInsight Hadoop pomocí sady .NET SDK HDInsight."
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
ms.openlocfilehash: 11f07d90405d3e804774610e242813927df59a03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-using-hdinsight-net-sdk"></a>Spouštění dotazů Hive pomocí sady .NET SDK HDInsight
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Zjistěte, jak toosubmit Hive dotazy pomocí HDInsight .NET SDK. Můžete napsat dotaz Hive pro výpis tabulek Hive toosubmit programu C# a zobrazit výsledky hello.

> [!NOTE]
> Hello kroky v tomto článku je potřeba provést z klienta Windows. Informace o systému Linux, OS X nebo Unix klienta toowork pomocí Hive použijte selektor karta hello zobrazený na hello horní části článku hello.
> 
> 

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto článku, musíte mít hello následující položky:

* **Cluster Hadoop v HDInsight**. V tématu [začít používat systémem Linux Hadoop v HDInsight](./hdinsight-hadoop-linux-tutorial-get-started.md).
* **Visual Studio 2013 nebo 2015 nebo 2017**.

## <a name="submit-hive-queries-using-hdinsight-net-sdk"></a>Odesílání dotazů Hive pomocí sady .NET SDK HDInsight
Hello HDInsight .NET SDK poskytuje klientské knihovny .NET, takže je jednodušší toowork s clustery HDInsight pomocí technologie .NET. 

**tooSubmit úlohy**

1. Vytvořte konzolovou aplikaci C# v sadě Visual Studio.
2. Z konzoly Správce balíčků Nuget hello spusťte následující příkaz hello:
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. Použijte hello následující kód:

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
                    System.Console.WriteLine("hello application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
   
                    SubmitHiveJob();
   
                    System.Console.WriteLine("Press ENTER toocontinue ...");
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
   
                    System.Console.WriteLine("Submitting hello Hive job toohello cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
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
4. Stiskněte klávesu **F5** toorun hello aplikace.

výstup Hello hello aplikace musí být podobně jako:

![HDInsight Hadoop Hive výstup úlohy](./media/hdinsight-hadoop-use-hive-dotnet-sdk/hdinsight-hadoop-use-hive-net-sdk-output.png)

## <a name="next-steps"></a>Další kroky
V tomto článku jste se naučili několik způsobů toocreate clusteru služby HDInsight. toolearn více, najdete v části hello následující články:

* [Začínáme se službou Azure HDInsight][hdinsight-get-started]
* [Vytvoření clusterů systému Hadoop v HDInsight][hdinsight-provision]
* [Správa clusterů systému Hadoop v HDInsight pomocí hello portálu Azure](hdinsight-administer-use-management-portal.md)
* [Referenční informace sady SDK rozhraní .NET HDInsight](https://msdn.microsoft.com/library/mt271028.aspx)
* [Použití Pigu se službou HDInsight](hdinsight-use-pig.md)
* [Použití nástroje Sqoop s HDInsight](hdinsight-use-sqoop-mac-linux.md)
* [Vytvoření aplikací .NET HDInsight pro neinteraktivní ověřování](hdinsight-create-non-interactive-authentication-dotnet-applications.md)

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md


