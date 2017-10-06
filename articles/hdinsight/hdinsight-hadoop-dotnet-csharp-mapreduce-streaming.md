---
title: "aaaUse C# s použitím prostředí MapReduce systému Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak řešení MapReduce toocreate toouse C# s Hadoop v Azure HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d83def76-12ad-4538-bb8e-3ba3542b7211
ms.custom: hdinsightactive
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: dd8b684e74155bc1a37d4ab8d6f9033276ef5aa3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-c-with-mapreduce-streaming-on-hadoop-in-hdinsight"></a>Použití jazyka C# s MapReduce, streamování systému Hadoop v HDInsight

Zjistěte, jak toouse C# toocreate MapReduce řešení v HDInsight.

> [!IMPORTANT]
> Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md).

Streamování Hadoop je nástroj, který vám umožní úloh MapReduce toorun pomocí skriptu nebo spustitelného souboru. V tomto příkladu je rozhraní .NET použité tooimplement hello mapper a reduktorem počet řešení aplikace word.

## <a name="net-on-hdinsight"></a>Rozhraní .NET v HDInsight

__HDInsight se systémem Linux__ clusterů použijte [Mono (https://mono-project.com)](https://mono-project.com) toorun aplikací .NET. Monofonní verze 4.2.1 je součástí HDInsight verze 3.5. Další informace o verzi hello Mono zahrnuté do HDInsight naleznete v tématu [HDInsight verze součástí](hdinsight-component-versioning.md). toouse na konkrétní verzi Mono, najdete v části hello [instalace nebo aktualizace Mono](hdinsight-hadoop-install-mono.md) dokumentu.

Další informace o Mono kompatibilitu s verzí rozhraní .NET Framework naleznete v tématu [Mono kompatibility](http://www.mono-project.com/docs/about-mono/compatibility/).

## <a name="how-hadoop-streaming-works"></a>Jak funguje streamování Hadoop

Základní proces Hello používá pro streamování v tomto dokumentu je následující:

1. Hadoop předá na stdin – toohello Mapovač dat (mapper.exe v tomto příkladu).
2. Mapovač Hello zpracovává hello dat a vysílá tooSTDOUT páry klíč/hodnota oddělený tabulátory.
3. výstup Hello je číst Hadoop a poté předá stdin – reduktorem toohello (reducer.exe v tomto příkladu).
4. Hello reduktorem čte páry klíč – hodnota hello oddělený tabulátory, zpracovává hello data a potom vydá hello výsledek jako dvojice klíč/hodnota oddělený tabulátory do datového proudu STDOUT.
5. výstup Hello bude číst Hadoop a zapisovat toohello výstupního adresáře.

Další informace o streamování najdete v tématu [Hadoop streamování (https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html)](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).

## <a name="prerequisites"></a>Požadavky

* Znalost zápis a sestavování kód C#, která je cílena rozhraní .NET Framework 4.5. Hello kroky v tomto dokumentu Visual Studio 2017.

* Způsob tooupload .exe soubory toohello clusteru. Hello kroky v tomto dokumentu používají hello nástrojů Data Lake pro Visual Studio tooupload hello soubory tooprimary úložiště pro hello cluster.

* Prostředí Azure PowerShell nebo SSH klienta.

* Hadoop v clusteru HDInsight. Další informace týkající se vytvoření clusteru najdete v tématu [vytvoření clusteru HDInsight](hdinsight-provision-clusters.md).

## <a name="create-hello-mapper"></a>Vytvořit mapování hello

V sadě Visual Studio vytvořte novou __Konzolová aplikace__ s názvem __mapper__. Použijte následující kód aplikace hello hello:

```csharp
using System;
using System.Text.RegularExpressions;

namespace mapper
{
    class Program
    {
        static void Main(string[] args)
        {
            string line;
            //Hadoop passes data toohello mapper on STDIN
            while((line = Console.ReadLine()) != null)
            {
                // We only want words, so strip out punctuation, numbers, etc.
                var onlyText = Regex.Replace(line, @"\.|;|:|,|[0-9]|'", "");
                // Split at whitespace.
                var words = Regex.Matches(onlyText, @"[\w]+");
                // Loop over hello words
                foreach(var word in words)
                {
                    //Emit tab-delimited key/value pairs.
                    //In this case, a word and a count of 1.
                    Console.WriteLine("{0}\t1",word);
                }
            }
        }
    }
}
```

Po vytvoření aplikace hello sestavení ho tooproduce hello `/bin/Debug/mapper.exe` soubor v adresáři projektu hello.

## <a name="create-hello-reducer"></a>Vytvoření reduktorem hello

V sadě Visual Studio vytvořte novou __Konzolová aplikace__ s názvem __reduktorem__. Použijte následující kód aplikace hello hello:

```csharp
using System;
using System.Collections.Generic;

namespace reducer
{
    class Program
    {
        static void Main(string[] args)
        {
            //Dictionary for holding a count of words
            Dictionary<string, int> words = new Dictionary<string, int>();

            string line;
            //Read from STDIN
            while ((line = Console.ReadLine()) != null)
            {
                // Data from Hadoop is tab-delimited key/value pairs
                var sArr = line.Split('\t');
                // Get hello word
                string word = sArr[0];
                // Get hello count
                int count = Convert.ToInt32(sArr[1]);

                //Do we already have a count for hello word?
                if(words.ContainsKey(word))
                {
                    //If so, increment hello count
                    words[word] += count;
                } else
                {
                    //Add hello key toohello collection
                    words.Add(word, count);
                }
            }
            //Finally, emit each word and count
            foreach (var word in words)
            {
                //Emit tab-delimited key/value pairs.
                //In this case, a word and a count of 1.
                Console.WriteLine("{0}\t{1}", word.Key, word.Value);
            }
        }
    }
}
```

Po vytvoření aplikace hello sestavení ho tooproduce hello `/bin/Debug/reducer.exe` soubor v adresáři projektu hello.

## <a name="upload-toostorage"></a>Nahrát toostorage

1. V sadě Visual Studio otevřete **Průzkumníka serveru**.

2. Rozbalte položku **Azure** a pak rozbalte **HDInsight**.

3. Po zobrazení výzvy zadejte své přihlašovací údaje předplatného Azure a pak klikněte na tlačítko **přihlásit**.

4. Rozbalte cluster HDInsight hello, který chcete toodeploy této aplikace. Položku s textem hello __(výchozí účet úložiště)__ je uveden.

    ![Průzkumník serveru zobrazující hello účet úložiště pro hello cluster](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

    * Pokud tuto položku lze rozšířit, kterou používáte __účet úložiště Azure__ jako výchozí úložiště pro hello cluster. soubory hello tooview na hello výchozí úložiště pro hello cluster, rozbalte položku hello a potom dvakrát klikněte na hello __(výchozí kontejner)__.

    * Pokud tuto položku nelze rozšířit, používáte __Azure Data Lake Store__ jako hello výchozí úložiště pro hello cluster. soubory hello tooview na hello výchozí úložiště pro hello cluster, dvakrát klikněte na hello __(výchozí účet úložiště)__ položku.

5. soubory .exe tooupload hello, použijte jednu z následujících metod hello:

    * Pokud se používá __účet úložiště Azure__, klikněte na ikonu hello nahrávání a vyhledejte toohello **bin\debug** složku pro hello **mapper** projektu. Nakonec vyberte hello **mapper.exe** souboru a klikněte na tlačítko **Ok**.

        ![Nahrajte ikonu](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/upload.png)
    
    * Pokud používáte __Azure Data Lake Store__, klikněte pravým tlačítkem myši na prázdnou oblast v seznamu hello souboru a pak vyberte __nahrát__. Nakonec vyberte hello **mapper.exe** souboru a klikněte na tlačítko **otevřete**.

    Jednou hello __mapper.exe__ nahrávání dokončí, proces odesílání opakování hello hello __reducer.exe__ souboru.

## <a name="run-a-job-using-an-ssh-session"></a>Spustit úlohu: pomocí relace SSH

1. Použití clusteru HDInsight toohello tooconnect SSH. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Použijte jednu z následujících úlohu MapReduce hello toostart příkaz hello:

    * Pokud používáte __Data Lake Store__ jako výchozí úložiště:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files adl:///mapper.exe,adl:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```
    
    * Pokud používáte __Azure Storage__ jako výchozí úložiště:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files wasb:///mapper.exe,wasb:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```

    Hello následující seznam popisuje, jaké jsou jednotlivé parametry:

    * `hadoop-streaming.jar`: soubor jar hello, který obsahuje hello streamování MapReduce funkce.
    * `-files`: Přidá hello `mapper.exe` a `reducer.exe` soubory toothis úlohy. Hello `adl:///` nebo `wasb:///` před každý soubor hello cesta toohello kořenovém výchozí úložiště pro hello cluster.
    * `-mapper`: Určuje soubor, který implementuje hello mapper.
    * `-reducer`: Určuje soubor, který implementuje reduktorem hello.
    * `-input`: hello vstupní data.
    * `-output`: hello výstupního adresáře.

3. Po dokončení úlohy MapReduce hello použijte hello následující tooview hello výsledky:

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    Hello následující text je příklad hello dat vrácených tento příkaz:

        you     1128
        young   38
        younger 1
        youngest        1
        your    338
        yours   4
        yourself        34
        yourselves      3
        youth   17

## <a name="run-a-job-using-powershell"></a>Spustit úlohu: pomocí prostředí PowerShell

Pomocí následujících toorun skript prostředí PowerShell úlohu MapReduce hello a stahování hello výsledky.

[!code-powershell[main](../../powershell_scripts/hdinsight/use-csharp-mapreduce/use-csharp-mapreduce.ps1?range=5-87)]

Tento skript vyzve k zadání hello clusteru přihlašovací účet jméno a heslo, společně s názvem clusteru HDInsight hello. Po dokončení úlohy hello výstup hello je stažené toohello `output.txt` je soubor v hello directory hello skript spustili z. Hello následující text je příkladem hello data v hello `output.txt` souboru:

    you     1128
    young   38
    younger 1
    youngest        1
    your    338
    yours   4
    yourself        34
    yourselves      3
    youth   17

## <a name="next-steps"></a>Další kroky

Další informace o používání MapReduce s HDInsight naleznete v tématu [používání MapReduce s HDInsight](hdinsight-use-mapreduce.md).

Informace o použití jazyka C# s Hive a Pig najdete v tématu [uživatelsky definované funkce jazyka C# pomocí Hive a Pig](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md).

Informace o použití jazyka C# se Storm v HDInsight naleznete v tématu [vývoj C# topologií pro Storm v HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).