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
ms.date: 11/08/2017
ms.author: larryfr
ms.openlocfilehash: c828a7b63e70669ed38ecea898442a3978e67ba7
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/09/2017
---
# <a name="run-pig-jobs-using-the-net-sdk-for-hadoop-in-hdinsight"></a>Spuštění úlohy Pig pomocí sady .NET SDK pro Hadoop v HDInsight

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Naučte se používat sadu .NET SDK pro Hadoop úlohy Apache Pig do Hadoop v Azure HDInsight.

.NET SDK služby HDInsight poskytuje klientské knihovny .NET, které usnadňuje práci s clustery HDInsight z rozhraní .NET. Pig umožňuje modelování řadu transformace dat vytvořit MapReduce operations. V tomto dokumentu zjistěte, jak používat základní aplikace C# se odeslat úlohu Pig do clusteru HDInsight.

## <a name="prerequisites"></a>Požadavky

Pokud chcete provést kroky v tomto článku, budete potřebovat následující.

* Cluster Azure HDInsight (Hadoop v HDInsight) (buď Windows nebo systémem Linux).

  > [!IMPORTANT]
  > HDInsight od verze 3.4 výše používá výhradně operační systém Linux. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Visual Studio 2012, 2013, 2015 nebo 2017.

## <a name="create-the-application"></a>Vytvoření aplikace

.NET SDK služby HDInsight poskytuje klientské knihovny .NET, která usnadňuje práci s clustery HDInsight pomocí technologie .NET.

1. Z **soubor** nabídky v sadě Visual Studio, vyberte **nový** a pak vyberte **projektu**.

2. Pro nový projekt zadejte nebo vyberte tyto hodnoty:

   | Vlastnost | Hodnota |
   | ------ | ------ |
   | Kategorie | Šablony/Visual C#/Windows |
   | Šablona | Konzolová aplikace |
   | Name (Název) | SubmitPigJob |

3. Kliknutím na tlačítko **OK** vytvořte projekt.

4. Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven** nebo **Správce balíčků NuGet**a potom vyberte **Konzola správce balíčků**.

5. K instalaci balíčků .NET SDK, použijte následující příkaz:

        Install-Package Microsoft.Azure.Management.HDInsight.Job

6. V Průzkumníku řešení poklikejte na **Program.cs** ho otevřete. Nahraďte stávající kód následující.

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

7. Chcete-li spustit aplikaci, stiskněte **F5**.

8. Pokud chcete aplikaci ukončit, stiskněte **ENTER**.

## <a name="next-steps"></a>Další kroky

Informace o Pig v HDInsight, naleznete v části [použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md).

Další informace o používání Hadoop v HDInsight najdete v následujících dokumentech:

* [Použijte Hive s Hadoop v HDInsight](hdinsight-use-hive.md)
* [Používání nástroje MapReduce s Hadoop v HDInsight](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/
