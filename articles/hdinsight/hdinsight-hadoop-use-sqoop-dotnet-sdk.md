---
title: "Spuštění úloh Sqoop pomocí rozhraní .NET a HDInsight - Azure | Microsoft Docs"
description: "Naučte se používat sadu .NET SDK HDInsight Sqoop import a export mezi clusteru Hadoop a Azure SQL database."
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
ms.openlocfilehash: c95641fc6d20e2911e007d1974b9e2c2398b3133
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="run-sqoop-jobs-using-net-sdk-for-hadoop-in-hdinsight"></a><span data-ttu-id="a6f7b-104">Spuštění úloh Sqoop pro Hadoop v HDInsight pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="a6f7b-104">Run Sqoop jobs using .NET SDK for Hadoop in HDInsight</span></span>
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

<span data-ttu-id="a6f7b-105">Naučte se používat sadu .NET SDK HDInsight ke spuštění úloh Sqoop v HDInsight k importu a exportu mezi HDInsight cluster a Azure SQL database nebo databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a6f7b-105">Learn how to use HDInsight .NET SDK to run Sqoop jobs in HDInsight to import and export between HDInsight cluster and Azure SQL database or SQL Server database.</span></span>

> [!NOTE]
> <span data-ttu-id="a6f7b-106">Kroky v tomto článku lze použít s buď Windows nebo Linux cluster HDInsight; ale tyto kroky fungovat pouze z klienta Windows.</span><span class="sxs-lookup"><span data-stu-id="a6f7b-106">The steps in this article can be used with either a Windows-based or Linux-based HDInsight cluster; however, these steps only work from a Windows client.</span></span> <span data-ttu-id="a6f7b-107">Zvolte jiné metody pomocí volič karty nahoře v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="a6f7b-107">Use the tab selector on the top of this article to choose other methods.</span></span>
> 
> 

### <a name="prerequisites"></a><span data-ttu-id="a6f7b-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a6f7b-108">Prerequisites</span></span>
<span data-ttu-id="a6f7b-109">Před zahájením tohoto kurzu musíte mít tyto položky:</span><span class="sxs-lookup"><span data-stu-id="a6f7b-109">Before you begin this tutorial, you must have the following items:</span></span>

* <span data-ttu-id="a6f7b-110">**Cluster Hadoop v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="a6f7b-110">**A Hadoop cluster in HDInsight**.</span></span> <span data-ttu-id="a6f7b-111">V tématu [vytvoření clusteru a databáze SQL](hdinsight-use-sqoop.md#create-cluster-and-sql-database).</span><span class="sxs-lookup"><span data-stu-id="a6f7b-111">See [Create cluster and SQL database](hdinsight-use-sqoop.md#create-cluster-and-sql-database).</span></span>

## <a name="use-sqoop-on-hdinsight-clusters-using-net-sdk"></a><span data-ttu-id="a6f7b-112">Použití nástroje Sqoop clustery prostředí HDInsight pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="a6f7b-112">Use Sqoop on HDInsight clusters using .NET SDK</span></span>
<span data-ttu-id="a6f7b-113">.NET SDK služby HDInsight poskytuje klientské knihovny .NET, která usnadňuje práci s clustery HDInsight pomocí technologie .NET.</span><span class="sxs-lookup"><span data-stu-id="a6f7b-113">The HDInsight .NET SDK provides .NET client libraries, which makes it easier to work with HDInsight clusters from .NET.</span></span> <span data-ttu-id="a6f7b-114">V této části vytvoříte konzolovou aplikaci C# pro export hivesampletable do tabulky databáze SQL, kterou jste vytvořili dříve v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="a6f7b-114">In this section, you create a C# console application to export the hivesampletable to the SQL Database table you created earlier in this tutorial.</span></span>

## <a name="submit-a-sqoop-job"></a><span data-ttu-id="a6f7b-115">Odeslání úlohy Sqoop</span><span class="sxs-lookup"><span data-stu-id="a6f7b-115">Submit a Sqoop job</span></span>

1. <span data-ttu-id="a6f7b-116">Vytvořte konzolovou aplikaci C# v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a6f7b-116">Create a C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="a6f7b-117">Z konzoly Správce balíčků Visual Studio spusťte následující příkaz pro import balíčku Nuget.</span><span class="sxs-lookup"><span data-stu-id="a6f7b-117">From the Visual Studio Package Manager Console, run the following Nuget command to import the package.</span></span>
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. <span data-ttu-id="a6f7b-118">Použijte následující kód v souboru Program.cs:</span><span class="sxs-lookup"><span data-stu-id="a6f7b-118">Use the following code in the Program.cs file:</span></span>
   
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
                    System.Console.WriteLine("The application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
   
                    SubmitSqoopJob();
   
                    System.Console.WriteLine("Press ENTER to continue ...");
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
   
                    System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }
4. <span data-ttu-id="a6f7b-119">Stiskněte klávesu **F5** ke spuštění programu.</span><span class="sxs-lookup"><span data-stu-id="a6f7b-119">Press **F5** to run the program.</span></span> 

## <a name="limitations"></a><span data-ttu-id="a6f7b-120">Omezení</span><span class="sxs-lookup"><span data-stu-id="a6f7b-120">Limitations</span></span>
* <span data-ttu-id="a6f7b-121">Hromadné export - s Linuxovým systémem HDInsight, Sqoop konektor umožňuje exportovat data do systému Microsoft SQL Server nebo Azure SQL Database v současné době nepodporuje hromadné vložení.</span><span class="sxs-lookup"><span data-stu-id="a6f7b-121">Bulk export - With Linux-based HDInsight, the Sqoop connector used to export data to Microsoft SQL Server or Azure SQL Database does not currently support bulk inserts.</span></span>
* <span data-ttu-id="a6f7b-122">Dávkování - s HDInsight se systémem Linux, při použití `-batch` přepnout při vložení, Sqoop provádí více vloží místo dávkování operace insert.</span><span class="sxs-lookup"><span data-stu-id="a6f7b-122">Batching - With Linux-based HDInsight, When using the `-batch` switch when performing inserts, Sqoop performs multiple inserts instead of batching the insert operations.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a6f7b-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a6f7b-123">Next steps</span></span>
<span data-ttu-id="a6f7b-124">Nyní jste se naučili postup použití nástroje Sqoop.</span><span class="sxs-lookup"><span data-stu-id="a6f7b-124">Now you have learned how to use Sqoop.</span></span> <span data-ttu-id="a6f7b-125">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="a6f7b-125">To learn more, see:</span></span>

* <span data-ttu-id="a6f7b-126">[Použijte Oozie s HDInsight](hdinsight-use-oozie.md): použití Sqoop akce v pracovním postupu Oozie.</span><span class="sxs-lookup"><span data-stu-id="a6f7b-126">[Use Oozie with HDInsight](hdinsight-use-oozie.md): Use Sqoop action in an Oozie workflow.</span></span>
* <span data-ttu-id="a6f7b-127">[Analýza dat zpoždění letu pomocí HDInsight](hdinsight-analyze-flight-delay-data.md): použití Hive k analýze letu zpoždění dat a pak pomocí Sqoop exportovat data do Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="a6f7b-127">[Analyze flight delay data using HDInsight](hdinsight-analyze-flight-delay-data.md): Use Hive to analyze flight delay data, and then use Sqoop to export data to an Azure SQL database.</span></span>
* <span data-ttu-id="a6f7b-128">[Nahrání dat do HDInsight](hdinsight-upload-data.md): Najít další metody pro odesílání dat do HDInsight nebo Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="a6f7b-128">[Upload data to HDInsight](hdinsight-upload-data.md): Find other methods for uploading data to HDInsight/Azure Blob storage.</span></span>

