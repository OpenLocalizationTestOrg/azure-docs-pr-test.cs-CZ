---
title: "aaaRun Apache Pig úlohy pomocí .NET SDK pro Hadoop - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toouse hello .NET SDK pro tooHadoop o toosubmit Pig úloh Hadoop v HDInsight."
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
ms.openlocfilehash: 1d4ceebd7c168372d23fe29a088f04676686de30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-pig-jobs-using-hello-net-sdk-for-hadoop-in-hdinsight"></a><span data-ttu-id="1e439-103">Spuštění úlohy Pig pomocí hello .NET SDK pro Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="1e439-103">Run Pig jobs using hello .NET SDK for Hadoop in HDInsight</span></span>

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="1e439-104">Zjistěte, jak toouse hello .NET SDK pro Hadoop toosubmit Apache Pig úlohy tooHadoop v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1e439-104">Learn how toouse hello .NET SDK for Hadoop toosubmit Apache Pig jobs tooHadoop on Azure HDInsight.</span></span>

<span data-ttu-id="1e439-105">Hello HDInsight .NET SDK poskytuje klienta knihovny .NET, které umožňuje snazší toowork s clustery HDInsight pomocí technologie .NET.</span><span class="sxs-lookup"><span data-stu-id="1e439-105">hello HDInsight .NET SDK provides .NET client libraries that makes it easier toowork with HDInsight clusters from .NET.</span></span> <span data-ttu-id="1e439-106">Pig umožňuje operace, MapReduce toocreate podle modelování řadu transformace dat.</span><span class="sxs-lookup"><span data-stu-id="1e439-106">Pig allows you toocreate MapReduce operations by modeling a series of data transformations.</span></span> <span data-ttu-id="1e439-107">V tomto dokumentu zjistíte, jak toosubmit aplikace toouse základní C# Pig úlohy tooan clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1e439-107">In this document, you learn how toouse a basic C# application toosubmit a Pig job tooan HDInsight cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1e439-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1e439-108">Prerequisites</span></span>

<span data-ttu-id="1e439-109">toocomplete hello kroky v tomto článku, budete potřebovat následující hello.</span><span class="sxs-lookup"><span data-stu-id="1e439-109">toocomplete hello steps in this article, you need hello following.</span></span>

* <span data-ttu-id="1e439-110">Cluster Azure HDInsight (Hadoop v HDInsight) (buď Windows nebo systémem Linux).</span><span class="sxs-lookup"><span data-stu-id="1e439-110">An Azure HDInsight (Hadoop on HDInsight) cluster (either Windows or Linux-based).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="1e439-111">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="1e439-111">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="1e439-112">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="1e439-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="1e439-113">Visual Studio 2012, 2013, 2015 nebo 2017.</span><span class="sxs-lookup"><span data-stu-id="1e439-113">Visual Studio 2012, 2013, 2015 or 2017.</span></span>

## <a name="create-hello-application"></a><span data-ttu-id="1e439-114">Vytvoření aplikace hello</span><span class="sxs-lookup"><span data-stu-id="1e439-114">Create hello application</span></span>

<span data-ttu-id="1e439-115">Hello HDInsight .NET SDK poskytuje klientské knihovny .NET, takže je jednodušší toowork s clustery HDInsight pomocí technologie .NET.</span><span class="sxs-lookup"><span data-stu-id="1e439-115">hello HDInsight .NET SDK provides .NET client libraries, which makes it easier toowork with HDInsight clusters from .NET.</span></span>

1. <span data-ttu-id="1e439-116">Z hello **soubor** nabídky v sadě Visual Studio, vyberte **nový** a pak vyberte **projektu**.</span><span class="sxs-lookup"><span data-stu-id="1e439-116">From hello **File** menu in Visual Studio, select **New** and then select **Project**.</span></span>

2. <span data-ttu-id="1e439-117">Pro nový projekt hello typu nebo vyberte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="1e439-117">For hello new project, type or select hello following values:</span></span>

   | <span data-ttu-id="1e439-118">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="1e439-118">Property</span></span> | <span data-ttu-id="1e439-119">Hodnota</span><span class="sxs-lookup"><span data-stu-id="1e439-119">Value</span></span> |
   | ------ | ------ |
   | <span data-ttu-id="1e439-120">Kategorie</span><span class="sxs-lookup"><span data-stu-id="1e439-120">Category</span></span> | <span data-ttu-id="1e439-121">Šablony/Visual C#/Windows</span><span class="sxs-lookup"><span data-stu-id="1e439-121">Templates/Visual C#/Windows</span></span> |
   | <span data-ttu-id="1e439-122">Šablona</span><span class="sxs-lookup"><span data-stu-id="1e439-122">Template</span></span> | <span data-ttu-id="1e439-123">Konzolová aplikace</span><span class="sxs-lookup"><span data-stu-id="1e439-123">Console Application</span></span> |
   | <span data-ttu-id="1e439-124">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="1e439-124">Name</span></span> | <span data-ttu-id="1e439-125">SubmitPigJob</span><span class="sxs-lookup"><span data-stu-id="1e439-125">SubmitPigJob</span></span> |

3. <span data-ttu-id="1e439-126">Klikněte na tlačítko **OK** toocreate hello projektu.</span><span class="sxs-lookup"><span data-stu-id="1e439-126">Click **OK** toocreate hello project.</span></span>

4. <span data-ttu-id="1e439-127">Z hello **nástroje** nabídce vyberte možnost **Správce balíčků knihoven** nebo **Správce balíčků Nuget**a potom vyberte **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="1e439-127">From hello **Tools** menu, select **Library Package Manager** or **Nuget Package Manager**, and then select **Package Manager Console**.</span></span>

5. <span data-ttu-id="1e439-128">tooinstall hello .NET SDK balíčků, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="1e439-128">tooinstall hello .NET SDK packages, use hello following command:</span></span>

        Install-Package Microsoft.Azure.Management.HDInsight.Job

6. <span data-ttu-id="1e439-129">V Průzkumníku řešení poklikejte na **Program.cs** tooopen ho.</span><span class="sxs-lookup"><span data-stu-id="1e439-129">From Solution Explorer, double-click **Program.cs** tooopen it.</span></span> <span data-ttu-id="1e439-130">Nahraďte stávající kód hello následující hello.</span><span class="sxs-lookup"><span data-stu-id="1e439-130">Replace hello existing code with hello following.</span></span>

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
                System.Console.WriteLine("hello application is running ...");

                var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                SubmitPigJob();

                System.Console.WriteLine("Press ENTER toocontinue ...");
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

                System.Console.WriteLine("Submitting hello Pig job toohello cluster...");
                var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                System.Console.WriteLine("Validating that hello response is as expected...");
                System.Console.WriteLine("Response status code is " + response.StatusCode);
                System.Console.WriteLine("Validating hello response object...");
                System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
            }
        }
    }
    ```

7. <span data-ttu-id="1e439-131">aplikace hello toostart, stiskněte klávesu **F5**.</span><span class="sxs-lookup"><span data-stu-id="1e439-131">toostart hello application, press **F5**.</span></span>

8. <span data-ttu-id="1e439-132">aplikace hello tooexit, stiskněte klávesu **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="1e439-132">tooexit hello application, press **ENTER**.</span></span>

## <a name="summary"></a><span data-ttu-id="1e439-133">Souhrn</span><span class="sxs-lookup"><span data-stu-id="1e439-133">Summary</span></span>

<span data-ttu-id="1e439-134">Jak vidíte, hello .NET SDK pro Hadoop vám umožní toocreate aplikace .NET, které odešlete clusteru HDInsight tooan úlohy Pig a sledovat stav úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="1e439-134">As you can see, hello .NET SDK for Hadoop allows you toocreate .NET applications that submit Pig jobs tooan HDInsight cluster, and monitor hello job status.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1e439-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1e439-135">Next steps</span></span>

<span data-ttu-id="1e439-136">Informace o Pig v HDInsight, naleznete v části [použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md).</span><span class="sxs-lookup"><span data-stu-id="1e439-136">For information on Pig in HDInsight, see [Use Pig with Hadoop on HDInsight](hdinsight-use-pig.md).</span></span>

<span data-ttu-id="1e439-137">Další informace o použití Hadoop v HDInsight najdete v části hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="1e439-137">For more information on using Hadoop on HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="1e439-138">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="1e439-138">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="1e439-139">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="1e439-139">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/
