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
# <a name="run-pig-jobs-using-hello-net-sdk-for-hadoop-in-hdinsight"></a>Spuštění úlohy Pig pomocí hello .NET SDK pro Hadoop v HDInsight

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Zjistěte, jak toouse hello .NET SDK pro Hadoop toosubmit Apache Pig úlohy tooHadoop v Azure HDInsight.

Hello HDInsight .NET SDK poskytuje klienta knihovny .NET, které umožňuje snazší toowork s clustery HDInsight pomocí technologie .NET. Pig umožňuje operace, MapReduce toocreate podle modelování řadu transformace dat. V tomto dokumentu zjistíte, jak toosubmit aplikace toouse základní C# Pig úlohy tooan clusteru HDInsight.

## <a name="prerequisites"></a>Požadavky

toocomplete hello kroky v tomto článku, budete potřebovat následující hello.

* Cluster Azure HDInsight (Hadoop v HDInsight) (buď Windows nebo systémem Linux).

  > [!IMPORTANT]
  > Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Visual Studio 2012, 2013, 2015 nebo 2017.

## <a name="create-hello-application"></a>Vytvoření aplikace hello

Hello HDInsight .NET SDK poskytuje klientské knihovny .NET, takže je jednodušší toowork s clustery HDInsight pomocí technologie .NET.

1. Z hello **soubor** nabídky v sadě Visual Studio, vyberte **nový** a pak vyberte **projektu**.

2. Pro nový projekt hello typu nebo vyberte hello následující hodnoty:

   | Vlastnost | Hodnota |
   | ------ | ------ |
   | Kategorie | Šablony/Visual C#/Windows |
   | Šablona | Konzolová aplikace |
   | Name (Název) | SubmitPigJob |

3. Klikněte na tlačítko **OK** toocreate hello projektu.

4. Z hello **nástroje** nabídce vyberte možnost **Správce balíčků knihoven** nebo **Správce balíčků Nuget**a potom vyberte **Konzola správce balíčků**.

5. tooinstall hello .NET SDK balíčků, použijte následující příkaz hello:

        Install-Package Microsoft.Azure.Management.HDInsight.Job

6. V Průzkumníku řešení poklikejte na **Program.cs** tooopen ho. Nahraďte stávající kód hello následující hello.

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

7. aplikace hello toostart, stiskněte klávesu **F5**.

8. aplikace hello tooexit, stiskněte klávesu **ENTER**.

## <a name="summary"></a>Souhrn

Jak vidíte, hello .NET SDK pro Hadoop vám umožní toocreate aplikace .NET, které odešlete clusteru HDInsight tooan úlohy Pig a sledovat stav úlohy hello.

## <a name="next-steps"></a>Další kroky

Informace o Pig v HDInsight, naleznete v části [použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md).

Další informace o použití Hadoop v HDInsight najdete v části hello následující dokumenty:

* [Použijte Hive s Hadoop v HDInsight](hdinsight-use-hive.md)
* [Používání nástroje MapReduce s Hadoop v HDInsight](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/
