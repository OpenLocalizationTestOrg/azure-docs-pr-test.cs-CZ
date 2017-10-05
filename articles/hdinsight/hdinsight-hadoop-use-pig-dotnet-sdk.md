---
title: "Spuštění úlohy Apache Pig pomocí .NET SDK pro Hadoop - Azure HDInsight | Microsoft Docs"
description: "Naučte se používat sadu .NET SDK pro Hadoop úlohy Pig do Hadoop v HDInsight."
services: hdinsight
documentationcenter: .net
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: fa11d49a-328c-47e7-b16d-e7ed2a453195
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/15/2017
ms.author: larryfr
ms.openlocfilehash: e40d152821b36852c447d5a3adfd39114edbbace
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="run-pig-jobs-using-the-net-sdk-for-hadoop-in-hdinsight"></a><span data-ttu-id="7e806-103">Spuštění úlohy Pig pomocí sady .NET SDK pro Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="7e806-103">Run Pig jobs using the .NET SDK for Hadoop in HDInsight</span></span>

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="7e806-104">Naučte se používat sadu .NET SDK pro Hadoop úlohy Apache Pig do Hadoop v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="7e806-104">Learn how to use the .NET SDK for Hadoop to submit Apache Pig jobs to Hadoop on Azure HDInsight.</span></span>

<span data-ttu-id="7e806-105">.NET SDK služby HDInsight poskytuje klientské knihovny .NET, které usnadňuje práci s clustery HDInsight z rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="7e806-105">The HDInsight .NET SDK provides .NET client libraries that makes it easier to work with HDInsight clusters from .NET.</span></span> <span data-ttu-id="7e806-106">Pig umožňuje modelování řadu transformace dat vytvořit MapReduce operations.</span><span class="sxs-lookup"><span data-stu-id="7e806-106">Pig allows you to create MapReduce operations by modeling a series of data transformations.</span></span> <span data-ttu-id="7e806-107">V tomto dokumentu zjistěte, jak používat základní aplikace C# se odeslat úlohu Pig do clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="7e806-107">In this document, you learn how to use a basic C# application to submit a Pig job to an HDInsight cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7e806-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7e806-108">Prerequisites</span></span>

<span data-ttu-id="7e806-109">Pokud chcete provést kroky v tomto článku, budete potřebovat následující.</span><span class="sxs-lookup"><span data-stu-id="7e806-109">To complete the steps in this article, you need the following.</span></span>

* <span data-ttu-id="7e806-110">Cluster Azure HDInsight (Hadoop v HDInsight) (buď Windows nebo systémem Linux).</span><span class="sxs-lookup"><span data-stu-id="7e806-110">An Azure HDInsight (Hadoop on HDInsight) cluster (either Windows or Linux-based).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="7e806-111">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="7e806-111">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="7e806-112">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="7e806-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="7e806-113">Visual Studio 2012, 2013, 2015 nebo 2017.</span><span class="sxs-lookup"><span data-stu-id="7e806-113">Visual Studio 2012, 2013, 2015 or 2017.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="7e806-114">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="7e806-114">Create the application</span></span>

<span data-ttu-id="7e806-115">.NET SDK služby HDInsight poskytuje klientské knihovny .NET, která usnadňuje práci s clustery HDInsight pomocí technologie .NET.</span><span class="sxs-lookup"><span data-stu-id="7e806-115">The HDInsight .NET SDK provides .NET client libraries, which makes it easier to work with HDInsight clusters from .NET.</span></span>

1. <span data-ttu-id="7e806-116">Z **soubor** nabídky v sadě Visual Studio, vyberte **nový** a pak vyberte **projektu**.</span><span class="sxs-lookup"><span data-stu-id="7e806-116">From the **File** menu in Visual Studio, select **New** and then select **Project**.</span></span>

2. <span data-ttu-id="7e806-117">Pro nový projekt zadejte nebo vyberte tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="7e806-117">For the new project, type or select the following values:</span></span>

   | <span data-ttu-id="7e806-118">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="7e806-118">Property</span></span> | <span data-ttu-id="7e806-119">Hodnota</span><span class="sxs-lookup"><span data-stu-id="7e806-119">Value</span></span> |
   | ------ | ------ |
   | <span data-ttu-id="7e806-120">Kategorie</span><span class="sxs-lookup"><span data-stu-id="7e806-120">Category</span></span> | <span data-ttu-id="7e806-121">Šablony/Visual C#/Windows</span><span class="sxs-lookup"><span data-stu-id="7e806-121">Templates/Visual C#/Windows</span></span> |
   | <span data-ttu-id="7e806-122">Šablona</span><span class="sxs-lookup"><span data-stu-id="7e806-122">Template</span></span> | <span data-ttu-id="7e806-123">Konzolová aplikace</span><span class="sxs-lookup"><span data-stu-id="7e806-123">Console Application</span></span> |
   | <span data-ttu-id="7e806-124">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="7e806-124">Name</span></span> | <span data-ttu-id="7e806-125">SubmitPigJob</span><span class="sxs-lookup"><span data-stu-id="7e806-125">SubmitPigJob</span></span> |

3. <span data-ttu-id="7e806-126">Kliknutím na tlačítko **OK** vytvořte projekt.</span><span class="sxs-lookup"><span data-stu-id="7e806-126">Click **OK** to create the project.</span></span>

4. <span data-ttu-id="7e806-127">Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven** nebo **Správce balíčků Nuget**a potom vyberte **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="7e806-127">From the **Tools** menu, select **Library Package Manager** or **Nuget Package Manager**, and then select **Package Manager Console**.</span></span>

5. <span data-ttu-id="7e806-128">K instalaci balíčků .NET SDK, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7e806-128">To install the .NET SDK packages, use the following command:</span></span>

        Install-Package Microsoft.Azure.Management.HDInsight.Job

6. <span data-ttu-id="7e806-129">V Průzkumníku řešení poklikejte na **Program.cs** ho otevřete.</span><span class="sxs-lookup"><span data-stu-id="7e806-129">From Solution Explorer, double-click **Program.cs** to open it.</span></span> <span data-ttu-id="7e806-130">Nahraďte stávající kód následující.</span><span class="sxs-lookup"><span data-stu-id="7e806-130">Replace the existing code with the following.</span></span>

    ```csharp
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

                SubmitPigJob();

                System.Console.WriteLine("Press ENTER to continue ...");
                System.Console.ReadLine();
            }

            private static void SubmitPigJob()
            {
                var parameters = new PigJobSubmissionParameters
                {
                    Query = @"LOGS = LOAD '/example/data/sample.log';
                                LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
                                FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
                                GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
                                FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
                                RESULT = order FREQUENCIES by COUNT desc;
                                DUMP RESULT;"
                };

                System.Console.WriteLine("Submitting the Pig job to the cluster...");
                var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                System.Console.WriteLine("Validating that the response is as expected...");
                System.Console.WriteLine("Response status code is " + response.StatusCode);
                System.Console.WriteLine("Validating the response object...");
                System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
            }
        }
    }
    ```

7. <span data-ttu-id="7e806-131">Chcete-li spustit aplikaci, stiskněte **F5**.</span><span class="sxs-lookup"><span data-stu-id="7e806-131">To start the application, press **F5**.</span></span>

8. <span data-ttu-id="7e806-132">Pokud chcete aplikaci ukončit, stiskněte **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="7e806-132">To exit the application, press **ENTER**.</span></span>

## <a name="summary"></a><span data-ttu-id="7e806-133">Souhrn</span><span class="sxs-lookup"><span data-stu-id="7e806-133">Summary</span></span>

<span data-ttu-id="7e806-134">Jak vidíte, sady .NET SDK pro Hadoop umožňuje vytvářet aplikace .NET, které úlohy Pig do clusteru služby HDInsight a monitorovat stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="7e806-134">As you can see, the .NET SDK for Hadoop allows you to create .NET applications that submit Pig jobs to an HDInsight cluster, and monitor the job status.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7e806-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7e806-135">Next steps</span></span>

<span data-ttu-id="7e806-136">Informace o Pig v HDInsight, naleznete v části [použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md).</span><span class="sxs-lookup"><span data-stu-id="7e806-136">For information on Pig in HDInsight, see [Use Pig with Hadoop on HDInsight](hdinsight-use-pig.md).</span></span>

<span data-ttu-id="7e806-137">Další informace o používání Hadoop v HDInsight najdete v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="7e806-137">For more information on using Hadoop on HDInsight, see the following documents:</span></span>

* [<span data-ttu-id="7e806-138">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="7e806-138">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="7e806-139">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="7e806-139">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/
