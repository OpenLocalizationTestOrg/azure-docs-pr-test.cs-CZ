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
# <a name="use-c-with-mapreduce-streaming-on-hadoop-in-hdinsight"></a><span data-ttu-id="3e208-103">Použití jazyka C# s MapReduce, streamování systému Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e208-103">Use C# with MapReduce streaming on Hadoop in HDInsight</span></span>

<span data-ttu-id="3e208-104">Zjistěte, jak toouse C# toocreate MapReduce řešení v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3e208-104">Learn how toouse C# toocreate a MapReduce solution on HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3e208-105">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="3e208-105">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="3e208-106">Další informace najdete v tématu [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="3e208-106">For more information, see [HDInsight component versioning](hdinsight-component-versioning.md).</span></span>

<span data-ttu-id="3e208-107">Streamování Hadoop je nástroj, který vám umožní úloh MapReduce toorun pomocí skriptu nebo spustitelného souboru.</span><span class="sxs-lookup"><span data-stu-id="3e208-107">Hadoop streaming is a utility that allows you toorun MapReduce jobs using a script or executable.</span></span> <span data-ttu-id="3e208-108">V tomto příkladu je rozhraní .NET použité tooimplement hello mapper a reduktorem počet řešení aplikace word.</span><span class="sxs-lookup"><span data-stu-id="3e208-108">In this example, .NET is used tooimplement hello mapper and reducer for a word count solution.</span></span>

## <a name="net-on-hdinsight"></a><span data-ttu-id="3e208-109">Rozhraní .NET v HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e208-109">.NET on HDInsight</span></span>

<span data-ttu-id="3e208-110">__HDInsight se systémem Linux__ clusterů použijte [Mono (https://mono-project.com)](https://mono-project.com) toorun aplikací .NET.</span><span class="sxs-lookup"><span data-stu-id="3e208-110">__Linux-based HDInsight__ clusters use [Mono (https://mono-project.com)](https://mono-project.com) toorun .NET applications.</span></span> <span data-ttu-id="3e208-111">Monofonní verze 4.2.1 je součástí HDInsight verze 3.5.</span><span class="sxs-lookup"><span data-stu-id="3e208-111">Mono version 4.2.1 is included with HDInsight version 3.5.</span></span> <span data-ttu-id="3e208-112">Další informace o verzi hello Mono zahrnuté do HDInsight naleznete v tématu [HDInsight verze součástí](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="3e208-112">For more information on hello version of Mono included with HDInsight, see [HDInsight component versions](hdinsight-component-versioning.md).</span></span> <span data-ttu-id="3e208-113">toouse na konkrétní verzi Mono, najdete v části hello [instalace nebo aktualizace Mono](hdinsight-hadoop-install-mono.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="3e208-113">toouse a specific version of Mono, see hello [Install or update Mono](hdinsight-hadoop-install-mono.md) document.</span></span>

<span data-ttu-id="3e208-114">Další informace o Mono kompatibilitu s verzí rozhraní .NET Framework naleznete v tématu [Mono kompatibility](http://www.mono-project.com/docs/about-mono/compatibility/).</span><span class="sxs-lookup"><span data-stu-id="3e208-114">For more information on Mono compatibility with .NET Framework versions, see [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/).</span></span>

## <a name="how-hadoop-streaming-works"></a><span data-ttu-id="3e208-115">Jak funguje streamování Hadoop</span><span class="sxs-lookup"><span data-stu-id="3e208-115">How Hadoop streaming works</span></span>

<span data-ttu-id="3e208-116">Základní proces Hello používá pro streamování v tomto dokumentu je následující:</span><span class="sxs-lookup"><span data-stu-id="3e208-116">hello basic process used for streaming in this document is as follows:</span></span>

1. <span data-ttu-id="3e208-117">Hadoop předá na stdin – toohello Mapovač dat (mapper.exe v tomto příkladu).</span><span class="sxs-lookup"><span data-stu-id="3e208-117">Hadoop passes data toohello mapper (mapper.exe in this example) on STDIN.</span></span>
2. <span data-ttu-id="3e208-118">Mapovač Hello zpracovává hello dat a vysílá tooSTDOUT páry klíč/hodnota oddělený tabulátory.</span><span class="sxs-lookup"><span data-stu-id="3e208-118">hello mapper processes hello data, and emits tab-delimited key/value pairs tooSTDOUT.</span></span>
3. <span data-ttu-id="3e208-119">výstup Hello je číst Hadoop a poté předá stdin – reduktorem toohello (reducer.exe v tomto příkladu).</span><span class="sxs-lookup"><span data-stu-id="3e208-119">hello output is read by Hadoop, and then passed toohello reducer (reducer.exe in this example) on STDIN.</span></span>
4. <span data-ttu-id="3e208-120">Hello reduktorem čte páry klíč – hodnota hello oddělený tabulátory, zpracovává hello data a potom vydá hello výsledek jako dvojice klíč/hodnota oddělený tabulátory do datového proudu STDOUT.</span><span class="sxs-lookup"><span data-stu-id="3e208-120">hello reducer reads hello tab-delimited key/value pairs, processes hello data, and then emits hello result as tab-delimited key/value pairs on STDOUT.</span></span>
5. <span data-ttu-id="3e208-121">výstup Hello bude číst Hadoop a zapisovat toohello výstupního adresáře.</span><span class="sxs-lookup"><span data-stu-id="3e208-121">hello output is read by Hadoop and written toohello output directory.</span></span>

<span data-ttu-id="3e208-122">Další informace o streamování najdete v tématu [Hadoop streamování (https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html)](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).</span><span class="sxs-lookup"><span data-stu-id="3e208-122">For more information on streaming, see [Hadoop Streaming (https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html)](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3e208-123">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3e208-123">Prerequisites</span></span>

* <span data-ttu-id="3e208-124">Znalost zápis a sestavování kód C#, která je cílena rozhraní .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="3e208-124">A familiarity with writing and building C# code that targets .NET Framework 4.5.</span></span> <span data-ttu-id="3e208-125">Hello kroky v tomto dokumentu Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="3e208-125">hello steps in this document use Visual Studio 2017.</span></span>

* <span data-ttu-id="3e208-126">Způsob tooupload .exe soubory toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="3e208-126">A way tooupload .exe files toohello cluster.</span></span> <span data-ttu-id="3e208-127">Hello kroky v tomto dokumentu používají hello nástrojů Data Lake pro Visual Studio tooupload hello soubory tooprimary úložiště pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="3e208-127">hello steps in this document use hello Data Lake Tools for Visual Studio tooupload hello files tooprimary storage for hello cluster.</span></span>

* <span data-ttu-id="3e208-128">Prostředí Azure PowerShell nebo SSH klienta.</span><span class="sxs-lookup"><span data-stu-id="3e208-128">Azure PowerShell or a SSH client.</span></span>

* <span data-ttu-id="3e208-129">Hadoop v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3e208-129">A Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="3e208-130">Další informace týkající se vytvoření clusteru najdete v tématu [vytvoření clusteru HDInsight](hdinsight-provision-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="3e208-130">For more information on creating a cluster, see [Create an HDInsight cluster](hdinsight-provision-clusters.md).</span></span>

## <a name="create-hello-mapper"></a><span data-ttu-id="3e208-131">Vytvořit mapování hello</span><span class="sxs-lookup"><span data-stu-id="3e208-131">Create hello mapper</span></span>

<span data-ttu-id="3e208-132">V sadě Visual Studio vytvořte novou __Konzolová aplikace__ s názvem __mapper__.</span><span class="sxs-lookup"><span data-stu-id="3e208-132">In Visual Studio, create a new __Console application__ named __mapper__.</span></span> <span data-ttu-id="3e208-133">Použijte následující kód aplikace hello hello:</span><span class="sxs-lookup"><span data-stu-id="3e208-133">Use hello following code for hello application:</span></span>

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

<span data-ttu-id="3e208-134">Po vytvoření aplikace hello sestavení ho tooproduce hello `/bin/Debug/mapper.exe` soubor v adresáři projektu hello.</span><span class="sxs-lookup"><span data-stu-id="3e208-134">After creating hello application, build it tooproduce hello `/bin/Debug/mapper.exe` file in hello project directory.</span></span>

## <a name="create-hello-reducer"></a><span data-ttu-id="3e208-135">Vytvoření reduktorem hello</span><span class="sxs-lookup"><span data-stu-id="3e208-135">Create hello reducer</span></span>

<span data-ttu-id="3e208-136">V sadě Visual Studio vytvořte novou __Konzolová aplikace__ s názvem __reduktorem__.</span><span class="sxs-lookup"><span data-stu-id="3e208-136">In Visual Studio, create a new __Console application__ named __reducer__.</span></span> <span data-ttu-id="3e208-137">Použijte následující kód aplikace hello hello:</span><span class="sxs-lookup"><span data-stu-id="3e208-137">Use hello following code for hello application:</span></span>

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

<span data-ttu-id="3e208-138">Po vytvoření aplikace hello sestavení ho tooproduce hello `/bin/Debug/reducer.exe` soubor v adresáři projektu hello.</span><span class="sxs-lookup"><span data-stu-id="3e208-138">After creating hello application, build it tooproduce hello `/bin/Debug/reducer.exe` file in hello project directory.</span></span>

## <a name="upload-toostorage"></a><span data-ttu-id="3e208-139">Nahrát toostorage</span><span class="sxs-lookup"><span data-stu-id="3e208-139">Upload toostorage</span></span>

1. <span data-ttu-id="3e208-140">V sadě Visual Studio otevřete **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="3e208-140">In Visual Studio, open **Server Explorer**.</span></span>

2. <span data-ttu-id="3e208-141">Rozbalte položku **Azure** a pak rozbalte **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="3e208-141">Expand **Azure**, and then expand **HDInsight**.</span></span>

3. <span data-ttu-id="3e208-142">Po zobrazení výzvy zadejte své přihlašovací údaje předplatného Azure a pak klikněte na tlačítko **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="3e208-142">If prompted, enter your Azure subscription credentials, and then click **Sign In**.</span></span>

4. <span data-ttu-id="3e208-143">Rozbalte cluster HDInsight hello, který chcete toodeploy této aplikace.</span><span class="sxs-lookup"><span data-stu-id="3e208-143">Expand hello HDInsight cluster that you wish toodeploy this application to.</span></span> <span data-ttu-id="3e208-144">Položku s textem hello __(výchozí účet úložiště)__ je uveden.</span><span class="sxs-lookup"><span data-stu-id="3e208-144">An entry with hello text __(Default Storage Account)__ is listed.</span></span>

    ![Průzkumník serveru zobrazující hello účet úložiště pro hello cluster](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

    * <span data-ttu-id="3e208-146">Pokud tuto položku lze rozšířit, kterou používáte __účet úložiště Azure__ jako výchozí úložiště pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="3e208-146">If this entry can be expanded, you are using an __Azure Storage Account__ as default storage for hello cluster.</span></span> <span data-ttu-id="3e208-147">soubory hello tooview na hello výchozí úložiště pro hello cluster, rozbalte položku hello a potom dvakrát klikněte na hello __(výchozí kontejner)__.</span><span class="sxs-lookup"><span data-stu-id="3e208-147">tooview hello files on hello default storage for hello cluster, expand hello entry and then double-click hello __(Default Container)__.</span></span>

    * <span data-ttu-id="3e208-148">Pokud tuto položku nelze rozšířit, používáte __Azure Data Lake Store__ jako hello výchozí úložiště pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="3e208-148">If this entry cannot be expanded, you are using __Azure Data Lake Store__ as hello default storage for hello cluster.</span></span> <span data-ttu-id="3e208-149">soubory hello tooview na hello výchozí úložiště pro hello cluster, dvakrát klikněte na hello __(výchozí účet úložiště)__ položku.</span><span class="sxs-lookup"><span data-stu-id="3e208-149">tooview hello files on hello default storage for hello cluster, double-click hello __(Default Storage Account)__ entry.</span></span>

5. <span data-ttu-id="3e208-150">soubory .exe tooupload hello, použijte jednu z následujících metod hello:</span><span class="sxs-lookup"><span data-stu-id="3e208-150">tooupload hello .exe files, use one of hello following methods:</span></span>

    * <span data-ttu-id="3e208-151">Pokud se používá __účet úložiště Azure__, klikněte na ikonu hello nahrávání a vyhledejte toohello **bin\debug** složku pro hello **mapper** projektu.</span><span class="sxs-lookup"><span data-stu-id="3e208-151">If using an __Azure Storage Account__, click hello upload icon, and then browse toohello **bin\debug** folder for hello **mapper** project.</span></span> <span data-ttu-id="3e208-152">Nakonec vyberte hello **mapper.exe** souboru a klikněte na tlačítko **Ok**.</span><span class="sxs-lookup"><span data-stu-id="3e208-152">Finally, select hello **mapper.exe** file and click **Ok**.</span></span>

        ![Nahrajte ikonu](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/upload.png)
    
    * <span data-ttu-id="3e208-154">Pokud používáte __Azure Data Lake Store__, klikněte pravým tlačítkem myši na prázdnou oblast v seznamu hello souboru a pak vyberte __nahrát__.</span><span class="sxs-lookup"><span data-stu-id="3e208-154">If using __Azure Data Lake Store__, right-click an empty area in hello file listing, and then select __Upload__.</span></span> <span data-ttu-id="3e208-155">Nakonec vyberte hello **mapper.exe** souboru a klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="3e208-155">Finally, select hello **mapper.exe** file and click **Open**.</span></span>

    <span data-ttu-id="3e208-156">Jednou hello __mapper.exe__ nahrávání dokončí, proces odesílání opakování hello hello __reducer.exe__ souboru.</span><span class="sxs-lookup"><span data-stu-id="3e208-156">Once hello __mapper.exe__ upload has finished, repeat hello upload process for hello __reducer.exe__ file.</span></span>

## <a name="run-a-job-using-an-ssh-session"></a><span data-ttu-id="3e208-157">Spustit úlohu: pomocí relace SSH</span><span class="sxs-lookup"><span data-stu-id="3e208-157">Run a job: Using an SSH session</span></span>

1. <span data-ttu-id="3e208-158">Použití clusteru HDInsight toohello tooconnect SSH.</span><span class="sxs-lookup"><span data-stu-id="3e208-158">Use SSH tooconnect toohello HDInsight cluster.</span></span> <span data-ttu-id="3e208-159">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="3e208-159">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="3e208-160">Použijte jednu z následujících úlohu MapReduce hello toostart příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="3e208-160">Use one of hello following command toostart hello MapReduce job:</span></span>

    * <span data-ttu-id="3e208-161">Pokud používáte __Data Lake Store__ jako výchozí úložiště:</span><span class="sxs-lookup"><span data-stu-id="3e208-161">If using __Data Lake Store__ as default storage:</span></span>

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files adl:///mapper.exe,adl:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```
    
    * <span data-ttu-id="3e208-162">Pokud používáte __Azure Storage__ jako výchozí úložiště:</span><span class="sxs-lookup"><span data-stu-id="3e208-162">If using __Azure Storage__ as default storage:</span></span>

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files wasb:///mapper.exe,wasb:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```

    <span data-ttu-id="3e208-163">Hello následující seznam popisuje, jaké jsou jednotlivé parametry:</span><span class="sxs-lookup"><span data-stu-id="3e208-163">hello following list describes what each parameter does:</span></span>

    * <span data-ttu-id="3e208-164">`hadoop-streaming.jar`: soubor jar hello, který obsahuje hello streamování MapReduce funkce.</span><span class="sxs-lookup"><span data-stu-id="3e208-164">`hadoop-streaming.jar`: hello jar file that contains hello streaming MapReduce functionality.</span></span>
    * <span data-ttu-id="3e208-165">`-files`: Přidá hello `mapper.exe` a `reducer.exe` soubory toothis úlohy.</span><span class="sxs-lookup"><span data-stu-id="3e208-165">`-files`: Adds hello `mapper.exe` and `reducer.exe` files toothis job.</span></span> <span data-ttu-id="3e208-166">Hello `adl:///` nebo `wasb:///` před každý soubor hello cesta toohello kořenovém výchozí úložiště pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="3e208-166">hello `adl:///` or `wasb:///` before each file is hello path toohello root of default storage for hello cluster.</span></span>
    * <span data-ttu-id="3e208-167">`-mapper`: Určuje soubor, který implementuje hello mapper.</span><span class="sxs-lookup"><span data-stu-id="3e208-167">`-mapper`: Specifies which file implements hello mapper.</span></span>
    * <span data-ttu-id="3e208-168">`-reducer`: Určuje soubor, který implementuje reduktorem hello.</span><span class="sxs-lookup"><span data-stu-id="3e208-168">`-reducer`: Specifies which file implements hello reducer.</span></span>
    * <span data-ttu-id="3e208-169">`-input`: hello vstupní data.</span><span class="sxs-lookup"><span data-stu-id="3e208-169">`-input`: hello input data.</span></span>
    * <span data-ttu-id="3e208-170">`-output`: hello výstupního adresáře.</span><span class="sxs-lookup"><span data-stu-id="3e208-170">`-output`: hello output directory.</span></span>

3. <span data-ttu-id="3e208-171">Po dokončení úlohy MapReduce hello použijte hello následující tooview hello výsledky:</span><span class="sxs-lookup"><span data-stu-id="3e208-171">Once hello MapReduce job completes, use hello following tooview hello results:</span></span>

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    <span data-ttu-id="3e208-172">Hello následující text je příklad hello dat vrácených tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="3e208-172">hello following text is an example of hello data returned by this command:</span></span>

        you     1128
        young   38
        younger 1
        youngest        1
        your    338
        yours   4
        yourself        34
        yourselves      3
        youth   17

## <a name="run-a-job-using-powershell"></a><span data-ttu-id="3e208-173">Spustit úlohu: pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="3e208-173">Run a job: Using PowerShell</span></span>

<span data-ttu-id="3e208-174">Pomocí následujících toorun skript prostředí PowerShell úlohu MapReduce hello a stahování hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="3e208-174">Use hello following PowerShell script toorun a MapReduce job and download hello results.</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/use-csharp-mapreduce/use-csharp-mapreduce.ps1?range=5-87)]

<span data-ttu-id="3e208-175">Tento skript vyzve k zadání hello clusteru přihlašovací účet jméno a heslo, společně s názvem clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="3e208-175">This script prompts you for hello cluster login account name and password, along with hello HDInsight cluster name.</span></span> <span data-ttu-id="3e208-176">Po dokončení úlohy hello výstup hello je stažené toohello `output.txt` je soubor v hello directory hello skript spustili z.</span><span class="sxs-lookup"><span data-stu-id="3e208-176">Once hello job completes, hello output is downloaded toohello `output.txt` file in hello directory hello script is ran from.</span></span> <span data-ttu-id="3e208-177">Hello následující text je příkladem hello data v hello `output.txt` souboru:</span><span class="sxs-lookup"><span data-stu-id="3e208-177">hello following text is an example of hello data in hello `output.txt` file:</span></span>

    you     1128
    young   38
    younger 1
    youngest        1
    your    338
    yours   4
    yourself        34
    yourselves      3
    youth   17

## <a name="next-steps"></a><span data-ttu-id="3e208-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3e208-178">Next steps</span></span>

<span data-ttu-id="3e208-179">Další informace o používání MapReduce s HDInsight naleznete v tématu [používání MapReduce s HDInsight](hdinsight-use-mapreduce.md).</span><span class="sxs-lookup"><span data-stu-id="3e208-179">For more information on using MapReduce with HDInsight, see [Use MapReduce with HDInsight](hdinsight-use-mapreduce.md).</span></span>

<span data-ttu-id="3e208-180">Informace o použití jazyka C# s Hive a Pig najdete v tématu [uživatelsky definované funkce jazyka C# pomocí Hive a Pig](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="3e208-180">For information on using C# with Hive and Pig, see [Use a C# user defined function with Hive and Pig](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md).</span></span>

<span data-ttu-id="3e208-181">Informace o použití jazyka C# se Storm v HDInsight naleznete v tématu [vývoj C# topologií pro Storm v HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="3e208-181">For information on using C# with Storm on HDInsight, see [Develop C# topologies for Storm on HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>