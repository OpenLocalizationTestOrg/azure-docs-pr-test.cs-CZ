---
title: "aaaRun Sqoop úloh pomocí rozhraní .NET a HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak HDInsight .NET SDK toorun toouse Sqoop import a export mezi clusteru Hadoop a Azure SQL database."
keywords: "sqoop úlohy"
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 87bacd13-7775-4b71-91da-161cb6224a96
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: afa0a78ba5e5d89c04ba7be4b58dd24aea4f39ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-sqoop-jobs-using-net-sdk-for-hadoop-in-hdinsight"></a>Spuštění úloh Sqoop pro Hadoop v HDInsight pomocí sady .NET SDK
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Zjistěte, jak HDInsight .NET SDK toorun toouse Sqoop úlohy v prostředí HDInsight tooimport a exportovat mezi HDInsight cluster a Azure SQL database nebo databáze systému SQL Server.

> [!NOTE]
> Hello kroky v tomto článku lze použít s buď Windows nebo Linux cluster HDInsight; ale tyto kroky fungovat pouze z klienta Windows. Používejte selektor karta hello na hello horní části tohoto článku toochoose jiné metody.
> 
> 

### <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte mít hello následující položky:

* **Cluster Hadoop v HDInsight**. V tématu [vytvoření clusteru a databáze SQL](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="use-sqoop-on-hdinsight-clusters-using-net-sdk"></a>Použití nástroje Sqoop clustery prostředí HDInsight pomocí sady .NET SDK
Hello HDInsight .NET SDK poskytuje klientské knihovny .NET, takže je jednodušší toowork s clustery HDInsight pomocí technologie .NET. V této části vytvoříte C# konzole aplikace tooexport hello hivesampletable toohello tabulka databáze SQL, kterou jste vytvořili dříve v tomto kurzu.

## <a name="submit-a-sqoop-job"></a>Odeslání úlohy Sqoop

1. Vytvořte konzolovou aplikaci C# v sadě Visual Studio.
2. Z hello Konzola správce balíčků Visual Studio spusťte následující balíček Nuget příkaz tooimport hello hello.
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. Použijte následující kód v souboru Program.cs hello hello:
   
        using System.Collections.Generic;
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
   
                static void Main(string[] args)
                {
                    System.Console.WriteLine("hello application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
   
                    SubmitSqoopJob();
   
                    System.Console.WriteLine("Press ENTER toocontinue ...");
                    System.Console.ReadLine();
                }
   
                private static void SubmitSqoopJob()
                {
                    var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                    var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                    var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                    var sqlDatabaseDatabaseName = "<DatabaseName>";
   
                    var tableName = "<TableName>";
                    var exportDir = "/tutorials/usesqoop/data";
   
                    // Connection string for using Azure SQL Database.
                    // Comment if using SQL Server
                    var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                    // Connection string for using SQL Server.
                    // Uncomment if using SQL Server
                    //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
   
                    var parameters = new SqoopJobSubmissionParameters
                    {
                        Files = new List<string> { "/user/oozie/share/lib/sqoop/sqljdbc41.jar" }, // This line is required for Linux-based cluster.
                        Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                    };
   
                    System.Console.WriteLine("Submitting hello Sqoop job toohello cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that hello response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating hello response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }
4. Stiskněte klávesu **F5** toorun programu hello. 

## <a name="limitations"></a>Omezení
* Hromadně export - s Linuxovým systémem HDInsight, hello Sqoop konektor používaný tooexport data tooMicrosoft systému SQL Server nebo Azure SQL Database v současné době nepodporuje hromadné vložení.
* Dávkování - s HDInsight se systémem Linux, při použití hello `-batch` přepnout při vložení, Sqoop provádí více vloží místo dávkování operace insert hello.

## <a name="next-steps"></a>Další kroky
Nyní jste se naučili, jak toouse Sqoop. toolearn více, najdete v části:

* [Použijte Oozie s HDInsight](hdinsight-use-oozie.md): použití Sqoop akce v pracovním postupu Oozie.
* [Analýza dat zpoždění letu pomocí HDInsight](hdinsight-analyze-flight-delay-data.md): použití Hive letu tooanalyze zpoždění data a pak použijte Sqoop tooexport data tooan Azure SQL database.
* [Nahrání dat tooHDInsight](hdinsight-upload-data.md): Najít další metody pro odesílání dat tooHDInsight/Azure Blob storage.

