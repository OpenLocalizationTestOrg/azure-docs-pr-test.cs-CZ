---
title: "Použití jazyka C# s MapReduce systému Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Další informace o použití jazyka C# k vytvoření řešení MapReduce s Hadoop v prostředí Azure HDInsight."
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
ms.openlocfilehash: adb454e56378a800c671614735aec78b6851aeb2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-c-with-mapreduce-streaming-on-hadoop-in-hdinsight"></a><span data-ttu-id="a23ff-103">Použití jazyka C# s MapReduce, streamování systému Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a23ff-103">Use C# with MapReduce streaming on Hadoop in HDInsight</span></span>

<span data-ttu-id="a23ff-104">Další informace o použití jazyka C# k vytvoření řešení MapReduce v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a23ff-104">Learn how to use C# to create a MapReduce solution on HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a23ff-105">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="a23ff-105">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="a23ff-106">Další informace najdete v tématu [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="a23ff-106">For more information, see [HDInsight component versioning](hdinsight-component-versioning.md).</span></span>

<span data-ttu-id="a23ff-107">Streamování Hadoop je nástroj, který umožňuje spouštění úloh MapReduce pomocí skriptu nebo spustitelného souboru.</span><span class="sxs-lookup"><span data-stu-id="a23ff-107">Hadoop streaming is a utility that allows you to run MapReduce jobs using a script or executable.</span></span> <span data-ttu-id="a23ff-108">V tomto příkladu se používá .NET pro implementaci mapper a reduktorem počet řešení aplikace word.</span><span class="sxs-lookup"><span data-stu-id="a23ff-108">In this example, .NET is used to implement the mapper and reducer for a word count solution.</span></span>

## <a name="net-on-hdinsight"></a><span data-ttu-id="a23ff-109">Rozhraní .NET v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a23ff-109">.NET on HDInsight</span></span>

<span data-ttu-id="a23ff-110">__HDInsight se systémem Linux__ clusterů použijte [Mono (https://mono-project.com)](https://mono-project.com) ke spouštění aplikací .NET.</span><span class="sxs-lookup"><span data-stu-id="a23ff-110">__Linux-based HDInsight__ clusters use [Mono (https://mono-project.com)](https://mono-project.com) to run .NET applications.</span></span> <span data-ttu-id="a23ff-111">Monofonní verze 4.2.1 je součástí HDInsight verze 3.5.</span><span class="sxs-lookup"><span data-stu-id="a23ff-111">Mono version 4.2.1 is included with HDInsight version 3.5.</span></span> <span data-ttu-id="a23ff-112">Další informace o verzi Mono zahrnuté do HDInsight naleznete v tématu [HDInsight verze součástí](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="a23ff-112">For more information on the version of Mono included with HDInsight, see [HDInsight component versions](hdinsight-component-versioning.md).</span></span> <span data-ttu-id="a23ff-113">Použít konkrétní verzi Mono, najdete v článku [instalace nebo aktualizace Mono](hdinsight-hadoop-install-mono.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a23ff-113">To use a specific version of Mono, see the [Install or update Mono](hdinsight-hadoop-install-mono.md) document.</span></span>

<span data-ttu-id="a23ff-114">Další informace o Mono kompatibilitu s verzí rozhraní .NET Framework naleznete v tématu [Mono kompatibility](http://www.mono-project.com/docs/about-mono/compatibility/).</span><span class="sxs-lookup"><span data-stu-id="a23ff-114">For more information on Mono compatibility with .NET Framework versions, see [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/).</span></span>

## <a name="how-hadoop-streaming-works"></a><span data-ttu-id="a23ff-115">Jak funguje streamování Hadoop</span><span class="sxs-lookup"><span data-stu-id="a23ff-115">How Hadoop streaming works</span></span>

<span data-ttu-id="a23ff-116">Základní proces použít pro streamování v tomto dokumentu je následující:</span><span class="sxs-lookup"><span data-stu-id="a23ff-116">The basic process used for streaming in this document is as follows:</span></span>

1. <span data-ttu-id="a23ff-117">Hadoop předává na stdin – data mapper (mapper.exe v tomto příkladu).</span><span class="sxs-lookup"><span data-stu-id="a23ff-117">Hadoop passes data to the mapper (mapper.exe in this example) on STDIN.</span></span>
2. <span data-ttu-id="a23ff-118">Mapper zpracovává dat a vysílá páry oddělený tabulátory klíč/hodnota pro STDOUT.</span><span class="sxs-lookup"><span data-stu-id="a23ff-118">The mapper processes the data, and emits tab-delimited key/value pairs to STDOUT.</span></span>
3. <span data-ttu-id="a23ff-119">Výstup bude číst Hadoop a následně předán do reduktorem (reducer.exe v tomto příkladu) na STDIN.</span><span class="sxs-lookup"><span data-stu-id="a23ff-119">The output is read by Hadoop, and then passed to the reducer (reducer.exe in this example) on STDIN.</span></span>
4. <span data-ttu-id="a23ff-120">Reduktorem čte páry klíč/hodnota oddělený tabulátory, zpracuje data a potom vydá výsledek jako dvojice klíč/hodnota oddělený tabulátory do datového proudu STDOUT.</span><span class="sxs-lookup"><span data-stu-id="a23ff-120">The reducer reads the tab-delimited key/value pairs, processes the data, and then emits the result as tab-delimited key/value pairs on STDOUT.</span></span>
5. <span data-ttu-id="a23ff-121">Výstup bude Hadoop číst a zapisovat do výstupního adresáře.</span><span class="sxs-lookup"><span data-stu-id="a23ff-121">The output is read by Hadoop and written to the output directory.</span></span>

<span data-ttu-id="a23ff-122">Další informace o streamování najdete v tématu [Hadoop streamování (https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html)](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).</span><span class="sxs-lookup"><span data-stu-id="a23ff-122">For more information on streaming, see [Hadoop Streaming (https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html)](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a23ff-123">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a23ff-123">Prerequisites</span></span>

* <span data-ttu-id="a23ff-124">Znalost zápis a sestavování kód C#, která je cílena rozhraní .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="a23ff-124">A familiarity with writing and building C# code that targets .NET Framework 4.5.</span></span> <span data-ttu-id="a23ff-125">Kroky v tomto dokumentu používají Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="a23ff-125">The steps in this document use Visual Studio 2017.</span></span>

* <span data-ttu-id="a23ff-126">Způsob, jak nahrát soubory .exe do clusteru.</span><span class="sxs-lookup"><span data-stu-id="a23ff-126">A way to upload .exe files to the cluster.</span></span> <span data-ttu-id="a23ff-127">Kroky v tomto dokumentu použít nástroje Data Lake pro Visual Studio k nahrání souborů do primárního úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="a23ff-127">The steps in this document use the Data Lake Tools for Visual Studio to upload the files to primary storage for the cluster.</span></span>

* <span data-ttu-id="a23ff-128">Prostředí Azure PowerShell nebo SSH klienta.</span><span class="sxs-lookup"><span data-stu-id="a23ff-128">Azure PowerShell or a SSH client.</span></span>

* <span data-ttu-id="a23ff-129">Hadoop v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a23ff-129">A Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="a23ff-130">Další informace týkající se vytvoření clusteru najdete v tématu [vytvoření clusteru HDInsight](hdinsight-provision-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="a23ff-130">For more information on creating a cluster, see [Create an HDInsight cluster](hdinsight-provision-clusters.md).</span></span>

## <a name="create-the-mapper"></a><span data-ttu-id="a23ff-131">Vytvořte mapper</span><span class="sxs-lookup"><span data-stu-id="a23ff-131">Create the mapper</span></span>

<span data-ttu-id="a23ff-132">V sadě Visual Studio vytvořte novou __Konzolová aplikace__ s názvem __mapper__.</span><span class="sxs-lookup"><span data-stu-id="a23ff-132">In Visual Studio, create a new __Console application__ named __mapper__.</span></span> <span data-ttu-id="a23ff-133">Použijte následující kód pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="a23ff-133">Use the following code for the application:</span></span>

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
            //Hadoop passes data to the mapper on STDIN
            while((line = Console.ReadLine()) != null)
            {
                // We only want words, so strip out punctuation, numbers, etc.
                var onlyText = Regex.Replace(line, @"\.|;|:|,|[0-9]|'", "");
                // Split at whitespace.
                var words = Regex.Matches(onlyText, @"[\w]+");
                // Loop over the words
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

<span data-ttu-id="a23ff-134">Po vytvoření aplikace, sestavte jej k vytvoření `/bin/Debug/mapper.exe` soubor v adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="a23ff-134">After creating the application, build it to produce the `/bin/Debug/mapper.exe` file in the project directory.</span></span>

## <a name="create-the-reducer"></a><span data-ttu-id="a23ff-135">Vytvořte reduktorem</span><span class="sxs-lookup"><span data-stu-id="a23ff-135">Create the reducer</span></span>

<span data-ttu-id="a23ff-136">V sadě Visual Studio vytvořte novou __Konzolová aplikace__ s názvem __reduktorem__.</span><span class="sxs-lookup"><span data-stu-id="a23ff-136">In Visual Studio, create a new __Console application__ named __reducer__.</span></span> <span data-ttu-id="a23ff-137">Použijte následující kód pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="a23ff-137">Use the following code for the application:</span></span>

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
                // Get the word
                string word = sArr[0];
                // Get the count
                int count = Convert.ToInt32(sArr[1]);

                //Do we already have a count for the word?
                if(words.ContainsKey(word))
                {
                    //If so, increment the count
                    words[word] += count;
                } else
                {
                    //Add the key to the collection
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

<span data-ttu-id="a23ff-138">Po vytvoření aplikace, sestavte jej k vytvoření `/bin/Debug/reducer.exe` soubor v adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="a23ff-138">After creating the application, build it to produce the `/bin/Debug/reducer.exe` file in the project directory.</span></span>

## <a name="upload-to-storage"></a><span data-ttu-id="a23ff-139">Nahrání do úložiště</span><span class="sxs-lookup"><span data-stu-id="a23ff-139">Upload to storage</span></span>

1. <span data-ttu-id="a23ff-140">V sadě Visual Studio otevřete **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="a23ff-140">In Visual Studio, open **Server Explorer**.</span></span>

2. <span data-ttu-id="a23ff-141">Rozbalte položku **Azure** a pak rozbalte **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="a23ff-141">Expand **Azure**, and then expand **HDInsight**.</span></span>

3. <span data-ttu-id="a23ff-142">Po zobrazení výzvy zadejte své přihlašovací údaje předplatného Azure a pak klikněte na tlačítko **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="a23ff-142">If prompted, enter your Azure subscription credentials, and then click **Sign In**.</span></span>

4. <span data-ttu-id="a23ff-143">Rozbalte položku, kterou chcete nasadit tuto aplikaci do clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a23ff-143">Expand the HDInsight cluster that you wish to deploy this application to.</span></span> <span data-ttu-id="a23ff-144">Položku s textem __(výchozí účet úložiště)__ je uveden.</span><span class="sxs-lookup"><span data-stu-id="a23ff-144">An entry with the text __(Default Storage Account)__ is listed.</span></span>

    ![Průzkumník serveru zobrazující účet úložiště pro cluster](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

    * <span data-ttu-id="a23ff-146">Pokud tuto položku lze rozšířit, kterou používáte __účet úložiště Azure__ jako výchozí úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="a23ff-146">If this entry can be expanded, you are using an __Azure Storage Account__ as default storage for the cluster.</span></span> <span data-ttu-id="a23ff-147">Chcete-li zobrazit soubory na výchozí úložiště pro cluster, rozbalte položku a dvakrát __(výchozí kontejner)__.</span><span class="sxs-lookup"><span data-stu-id="a23ff-147">To view the files on the default storage for the cluster, expand the entry and then double-click the __(Default Container)__.</span></span>

    * <span data-ttu-id="a23ff-148">Pokud tuto položku nelze rozšířit, používáte __Azure Data Lake Store__ jako výchozí úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="a23ff-148">If this entry cannot be expanded, you are using __Azure Data Lake Store__ as the default storage for the cluster.</span></span> <span data-ttu-id="a23ff-149">Chcete-li zobrazit soubory na výchozí úložiště pro cluster, dvakrát klikněte na __(výchozí účet úložiště)__ položku.</span><span class="sxs-lookup"><span data-stu-id="a23ff-149">To view the files on the default storage for the cluster, double-click the __(Default Storage Account)__ entry.</span></span>

5. <span data-ttu-id="a23ff-150">Pokud chcete nahrát soubory .exe, použijte jednu z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="a23ff-150">To upload the .exe files, use one of the following methods:</span></span>

    * <span data-ttu-id="a23ff-151">Pokud se používá __účet úložiště Azure__, klikněte na ikonu nahrávání a potom vyhledejte **bin\debug** složku pro **mapper** projektu.</span><span class="sxs-lookup"><span data-stu-id="a23ff-151">If using an __Azure Storage Account__, click the upload icon, and then browse to the **bin\debug** folder for the **mapper** project.</span></span> <span data-ttu-id="a23ff-152">Nakonec vyberte **mapper.exe** souboru a klikněte na tlačítko **Ok**.</span><span class="sxs-lookup"><span data-stu-id="a23ff-152">Finally, select the **mapper.exe** file and click **Ok**.</span></span>

        ![Nahrajte ikonu](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/upload.png)
    
    * <span data-ttu-id="a23ff-154">Pokud používáte __Azure Data Lake Store__, klikněte pravým tlačítkem myši na prázdnou oblast v seznamu souboru a pak vyberte __nahrát__.</span><span class="sxs-lookup"><span data-stu-id="a23ff-154">If using __Azure Data Lake Store__, right-click an empty area in the file listing, and then select __Upload__.</span></span> <span data-ttu-id="a23ff-155">Nakonec vyberte **mapper.exe** souboru a klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="a23ff-155">Finally, select the **mapper.exe** file and click **Open**.</span></span>

    <span data-ttu-id="a23ff-156">Jednou __mapper.exe__ odesílání skončí, opakujte tento postup nahrání pro __reducer.exe__ souboru.</span><span class="sxs-lookup"><span data-stu-id="a23ff-156">Once the __mapper.exe__ upload has finished, repeat the upload process for the __reducer.exe__ file.</span></span>

## <a name="run-a-job-using-an-ssh-session"></a><span data-ttu-id="a23ff-157">Spustit úlohu: pomocí relace SSH</span><span class="sxs-lookup"><span data-stu-id="a23ff-157">Run a job: Using an SSH session</span></span>

1. <span data-ttu-id="a23ff-158">Použití SSH se připojit ke clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a23ff-158">Use SSH to connect to the HDInsight cluster.</span></span> <span data-ttu-id="a23ff-159">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="a23ff-159">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="a23ff-160">Použijte jednu z následujícího příkazu spustíte úlohu MapReduce:</span><span class="sxs-lookup"><span data-stu-id="a23ff-160">Use one of the following command to start the MapReduce job:</span></span>

    * <span data-ttu-id="a23ff-161">Pokud používáte __Data Lake Store__ jako výchozí úložiště:</span><span class="sxs-lookup"><span data-stu-id="a23ff-161">If using __Data Lake Store__ as default storage:</span></span>

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files adl:///mapper.exe,adl:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```
    
    * <span data-ttu-id="a23ff-162">Pokud používáte __Azure Storage__ jako výchozí úložiště:</span><span class="sxs-lookup"><span data-stu-id="a23ff-162">If using __Azure Storage__ as default storage:</span></span>

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files wasb:///mapper.exe,wasb:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```

    <span data-ttu-id="a23ff-163">Následující seznam popisuje, jaké jsou jednotlivé parametry:</span><span class="sxs-lookup"><span data-stu-id="a23ff-163">The following list describes what each parameter does:</span></span>

    * <span data-ttu-id="a23ff-164">`hadoop-streaming.jar`: Na soubor jar, který obsahuje funkce datových proudů MapReduce.</span><span class="sxs-lookup"><span data-stu-id="a23ff-164">`hadoop-streaming.jar`: The jar file that contains the streaming MapReduce functionality.</span></span>
    * <span data-ttu-id="a23ff-165">`-files`: Přidá `mapper.exe` a `reducer.exe` soubory do této úlohy.</span><span class="sxs-lookup"><span data-stu-id="a23ff-165">`-files`: Adds the `mapper.exe` and `reducer.exe` files to this job.</span></span> <span data-ttu-id="a23ff-166">`adl:///` Nebo `wasb:///` před každý soubor je cesta ke kořenovému adresáři výchozí úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="a23ff-166">The `adl:///` or `wasb:///` before each file is the path to the root of default storage for the cluster.</span></span>
    * <span data-ttu-id="a23ff-167">`-mapper`: Určuje soubor, který implementuje mapper.</span><span class="sxs-lookup"><span data-stu-id="a23ff-167">`-mapper`: Specifies which file implements the mapper.</span></span>
    * <span data-ttu-id="a23ff-168">`-reducer`: Určuje soubor, který implementuje reduktorem.</span><span class="sxs-lookup"><span data-stu-id="a23ff-168">`-reducer`: Specifies which file implements the reducer.</span></span>
    * <span data-ttu-id="a23ff-169">`-input`: Vstupní data.</span><span class="sxs-lookup"><span data-stu-id="a23ff-169">`-input`: The input data.</span></span>
    * <span data-ttu-id="a23ff-170">`-output`: Výstupního adresáře.</span><span class="sxs-lookup"><span data-stu-id="a23ff-170">`-output`: The output directory.</span></span>

3. <span data-ttu-id="a23ff-171">Po dokončení úlohy MapReduce, použijte následující postupy pro zobrazení výsledků:</span><span class="sxs-lookup"><span data-stu-id="a23ff-171">Once the MapReduce job completes, use the following to view the results:</span></span>

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    <span data-ttu-id="a23ff-172">Tento text je příklad dat vrácených tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="a23ff-172">The following text is an example of the data returned by this command:</span></span>

        you     1128
        young   38
        younger 1
        youngest        1
        your    338
        yours   4
        yourself        34
        yourselves      3
        youth   17

## <a name="run-a-job-using-powershell"></a><span data-ttu-id="a23ff-173">Spustit úlohu: pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="a23ff-173">Run a job: Using PowerShell</span></span>

<span data-ttu-id="a23ff-174">Pomocí následujícího skriptu prostředí PowerShell a spusťte úlohu MapReduce stažení výsledků.</span><span class="sxs-lookup"><span data-stu-id="a23ff-174">Use the following PowerShell script to run a MapReduce job and download the results.</span></span>

<span data-ttu-id="a23ff-175">[!code-powershell[hlavní](../../powershell_scripts/hdinsight/use-csharp-mapreduce/use-csharp-mapreduce.ps1?range=5-87)]</span><span class="sxs-lookup"><span data-stu-id="a23ff-175">[!code-powershell[main](../../powershell_scripts/hdinsight/use-csharp-mapreduce/use-csharp-mapreduce.ps1?range=5-87)]</span></span>

<span data-ttu-id="a23ff-176">Tento skript vyzve k zadání názvu clusteru přihlášení účtu a heslo, společně s názvem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a23ff-176">This script prompts you for the cluster login account name and password, along with the HDInsight cluster name.</span></span> <span data-ttu-id="a23ff-177">Po dokončení úlohy, výstup se stáhne do `output.txt` soubor v adresáři je skript spustili z.</span><span class="sxs-lookup"><span data-stu-id="a23ff-177">Once the job completes, the output is downloaded to the `output.txt` file in the directory the script is ran from.</span></span> <span data-ttu-id="a23ff-178">Tento text je příklad dat v `output.txt` souboru:</span><span class="sxs-lookup"><span data-stu-id="a23ff-178">The following text is an example of the data in the `output.txt` file:</span></span>

    you     1128
    young   38
    younger 1
    youngest        1
    your    338
    yours   4
    yourself        34
    yourselves      3
    youth   17

## <a name="next-steps"></a><span data-ttu-id="a23ff-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a23ff-179">Next steps</span></span>

<span data-ttu-id="a23ff-180">Další informace o používání MapReduce s HDInsight naleznete v tématu [používání MapReduce s HDInsight](hdinsight-use-mapreduce.md).</span><span class="sxs-lookup"><span data-stu-id="a23ff-180">For more information on using MapReduce with HDInsight, see [Use MapReduce with HDInsight](hdinsight-use-mapreduce.md).</span></span>

<span data-ttu-id="a23ff-181">Informace o použití jazyka C# s Hive a Pig najdete v tématu [uživatelsky definované funkce jazyka C# pomocí Hive a Pig](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="a23ff-181">For information on using C# with Hive and Pig, see [Use a C# user defined function with Hive and Pig](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md).</span></span>

<span data-ttu-id="a23ff-182">Informace o použití jazyka C# se Storm v HDInsight naleznete v tématu [vývoj C# topologií pro Storm v HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="a23ff-182">For information on using C# with Storm on HDInsight, see [Develop C# topologies for Storm on HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>