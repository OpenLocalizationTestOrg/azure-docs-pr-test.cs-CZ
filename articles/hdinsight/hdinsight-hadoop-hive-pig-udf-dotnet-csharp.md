---
title: "Použití jazyka C# s Hive a Pig systému Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Další informace o použití jazyka C# uživatelsky definované funkce (UDF) s Hive a Pig streamování v Azure HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d83def76-12ad-4538-bb8e-3ba3542b7211
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 58e7af47be71c3e0389e5fb4641e124eb648494e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-c-user-defined-functions-with-hive-and-pig-streaming-on-hadoop-in-hdinsight"></a><span data-ttu-id="582ec-103">Uživatelem definované funkce jazyka C# pomocí Hive a Pig streamování systému Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="582ec-103">Use C# user-defined functions with Hive and Pig streaming on Hadoop in HDInsight</span></span>

<span data-ttu-id="582ec-104">Další informace o použití jazyka C# uživatelem definované funkce (UDF) s Apache Hive a Pig v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="582ec-104">Learn how to use C# user defined functions (UDF) with Apache Hive and Pig on HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="582ec-105">Kroky v tomto dokumentu pracovat s clustery HDInsight se systémem Linux i systému Windows.</span><span class="sxs-lookup"><span data-stu-id="582ec-105">The steps in this document work with both Linux-based and Windows-based HDInsight clusters.</span></span> <span data-ttu-id="582ec-106">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="582ec-106">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="582ec-107">Další informace najdete v tématu [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="582ec-107">For more information, see [HDInsight component versioning](hdinsight-component-versioning.md).</span></span>

<span data-ttu-id="582ec-108">Obě Hive a Pig můžete předat data do externí aplikace pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="582ec-108">Both Hive and Pig can pass data to external applications for processing.</span></span> <span data-ttu-id="582ec-109">Tento proces se označuje jako _streamování_.</span><span class="sxs-lookup"><span data-stu-id="582ec-109">This process is known as _streaming_.</span></span> <span data-ttu-id="582ec-110">Pokud používáte aplikační rozhraní .NET, data, je předaná do aplikace na stdin – a aplikace vrací výsledky do datového proudu STDOUT.</span><span class="sxs-lookup"><span data-stu-id="582ec-110">When using a .NET applciation, the data is passed to the application on STDIN, and the application returns the results on STDOUT.</span></span> <span data-ttu-id="582ec-111">Číst a zapisovat ze standardního vstupu a výstupu STDOUT, můžete použít `Console.ReadLine()` a `Console.WriteLine()` z konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="582ec-111">To read and write from STDIN and STDOUT, you can use `Console.ReadLine()` and `Console.WriteLine()` from a console application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="582ec-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="582ec-112">Prerequisites</span></span>

* <span data-ttu-id="582ec-113">Znalost zápis a sestavování kód C#, která je cílena rozhraní .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="582ec-113">A familiarity with writing and building C# code that targets .NET Framework 4.5.</span></span>

    * <span data-ttu-id="582ec-114">Ať IDE, které chcete použijte.</span><span class="sxs-lookup"><span data-stu-id="582ec-114">Use whatever IDE you want.</span></span> <span data-ttu-id="582ec-115">Doporučujeme, abyste [Visual Studio](https://www.visualstudio.com/vs) 2015, 2017, nebo [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="582ec-115">We recommend [Visual Studio](https://www.visualstudio.com/vs) 2015, 2017, or [Visual Studio Code](https://code.visualstudio.com/).</span></span> <span data-ttu-id="582ec-116">Kroky v tomto dokumentu používají Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="582ec-116">The steps in this document use Visual Studio 2017.</span></span>

* <span data-ttu-id="582ec-117">Způsob, jak nahrát soubory .exe do clusteru a spuštění úlohy Pig a Hive.</span><span class="sxs-lookup"><span data-stu-id="582ec-117">A way to upload .exe files to the cluster and run Pig and Hive jobs.</span></span> <span data-ttu-id="582ec-118">Doporučujeme, abyste nástrojů Data Lake pro Visual Studio, prostředí Azure PowerShell a rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="582ec-118">We recommend the Data Lake Tools for Visual Studio, Azure PowerShell and Azure CLI.</span></span> <span data-ttu-id="582ec-119">Kroky v tomto dokumentu nástroje Data Lake pro Visual Studio a odesílat soubory, spusťte v příkladu Hive dotaz.</span><span class="sxs-lookup"><span data-stu-id="582ec-119">The steps in this document use the Data Lake Tools for Visual Studio to upload the files and run the example Hive query.</span></span>

    <span data-ttu-id="582ec-120">Informace o jiných způsobech spouštění Hive dotazy a úlohy Pig, najdete v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="582ec-120">For information on other ways to run Hive queries and Pig jobs, see the following documents:</span></span>

    * [<span data-ttu-id="582ec-121">Použijte Apache Hive s HDInsight</span><span class="sxs-lookup"><span data-stu-id="582ec-121">Use Apache Hive with HDInsight</span></span>](hdinsight-use-hive.md)

    * [<span data-ttu-id="582ec-122">Použijte Apache Pig s HDInsight</span><span class="sxs-lookup"><span data-stu-id="582ec-122">Use Apache Pig with HDInsight</span></span>](hdinsight-use-pig.md)

* <span data-ttu-id="582ec-123">Hadoop v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="582ec-123">A Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="582ec-124">Další informace týkající se vytvoření clusteru najdete v tématu [vytvoření clusteru HDInsight](hdinsight-provision-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="582ec-124">For more information on creating a cluster, see [Create an HDInsight cluster](hdinsight-provision-clusters.md).</span></span>

## <a name="net-on-hdinsight"></a><span data-ttu-id="582ec-125">Rozhraní .NET v HDInsight</span><span class="sxs-lookup"><span data-stu-id="582ec-125">.NET on HDInsight</span></span>

* <span data-ttu-id="582ec-126">__HDInsight se systémem Linux__ clusterů pomocí [Mono (https://mono-project.com)](https://mono-project.com) ke spouštění aplikací .NET.</span><span class="sxs-lookup"><span data-stu-id="582ec-126">__Linux-based HDInsight__ clusters using [Mono (https://mono-project.com)](https://mono-project.com) to run .NET applications.</span></span> <span data-ttu-id="582ec-127">Monofonní verze 4.2.1 je součástí HDInsight verze 3.5.</span><span class="sxs-lookup"><span data-stu-id="582ec-127">Mono version 4.2.1 is included with HDInsight version 3.5.</span></span>

    <span data-ttu-id="582ec-128">Další informace o Mono kompatibilitu s verzí rozhraní .NET Framework naleznete v tématu [Mono kompatibility](http://www.mono-project.com/docs/about-mono/compatibility/).</span><span class="sxs-lookup"><span data-stu-id="582ec-128">For more information on Mono compatibility with .NET Framework versions, see [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/).</span></span>

    <span data-ttu-id="582ec-129">Použít konkrétní verzi Mono, najdete v článku [instalace nebo aktualizace Mono](hdinsight-hadoop-install-mono.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="582ec-129">To use a specific version of Mono, see the [Install or update Mono](hdinsight-hadoop-install-mono.md) document.</span></span>

* <span data-ttu-id="582ec-130">__HDInsight se systémem Windows__ clustery pomocí rozhraní Microsoft .NET CLR ke spouštění aplikací .NET.</span><span class="sxs-lookup"><span data-stu-id="582ec-130">__Windows-based HDInsight__ clusters use the Microsoft .NET CLR to run .NET applications.</span></span>

<span data-ttu-id="582ec-131">Další informace o verzi rozhraní .NET framework a Mono součástí verzích HDInsight naleznete v tématu [HDInsight verze součástí](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="582ec-131">For more information on the version of the .NET framework and Mono included with HDInsight versions, see [HDInsight component versions](hdinsight-component-versioning.md).</span></span>

## <a name="create-the-c-projects"></a><span data-ttu-id="582ec-132">Vytvoření C\# projekty</span><span class="sxs-lookup"><span data-stu-id="582ec-132">Create the C\# projects</span></span>

### <a name="hive-udf"></a><span data-ttu-id="582ec-133">Hive UDF</span><span class="sxs-lookup"><span data-stu-id="582ec-133">Hive UDF</span></span>

1. <span data-ttu-id="582ec-134">Otevřete Visual Studio a vytvořte řešení.</span><span class="sxs-lookup"><span data-stu-id="582ec-134">Open Visual Studio and create a solution.</span></span> <span data-ttu-id="582ec-135">Typ projektu vyberte **konzolovou aplikaci (rozhraní .NET Framework)**a název nového projektu **HiveCSharp**.</span><span class="sxs-lookup"><span data-stu-id="582ec-135">For the project type, select **Console App (.NET Framework)**, and name the new project **HiveCSharp**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="582ec-136">Vyberte __rozhraní .NET Framework 4.5__ Pokud používáte cluster HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="582ec-136">Select __.NET Framework 4.5__ if you are using a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="582ec-137">Další informace o Mono kompatibilitu s verzí rozhraní .NET Framework naleznete v tématu [Mono kompatibility](http://www.mono-project.com/docs/about-mono/compatibility/).</span><span class="sxs-lookup"><span data-stu-id="582ec-137">For more information on Mono compatibility with .NET Framework versions, see [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/).</span></span>

2. <span data-ttu-id="582ec-138">Nahraďte obsah **Program.cs** následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="582ec-138">Replace the contents of **Program.cs** with the following:</span></span>

    ```csharp
    using System;
    using System.Security.Cryptography;
    using System.Text;
    using System.Threading.Tasks;

    namespace HiveCSharp
    {
        class Program
        {
            static void Main(string[] args)
            {
                string line;
                // Read stdin in a loop
                while ((line = Console.ReadLine()) != null)
                {
                    // Parse the string, trimming line feeds
                    // and splitting fields at tabs
                    line = line.TrimEnd('\n');
                    string[] field = line.Split('\t');
                    string phoneLabel = field[1] + ' ' + field[2];
                    // Emit new data to stdout, delimited by tabs
                    Console.WriteLine("{0}\t{1}\t{2}", field[0], phoneLabel, GetMD5Hash(phoneLabel));
                }
            }
            /// <summary>
            /// Returns an MD5 hash for the given string
            /// </summary>
            /// <param name="input">string value</param>
            /// <returns>an MD5 hash</returns>
            static string GetMD5Hash(string input)
            {
                // Step 1, calculate MD5 hash from input
                MD5 md5 = System.Security.Cryptography.MD5.Create();
                byte[] inputBytes = System.Text.Encoding.ASCII.GetBytes(input);
                byte[] hash = md5.ComputeHash(inputBytes);

                // Step 2, convert byte array to hex string
                StringBuilder sb = new StringBuilder();
                for (int i = 0; i < hash.Length; i++)
                {
                    sb.Append(hash[i].ToString("x2"));
                }
                return sb.ToString();
            }
        }
    }
    ```

3. <span data-ttu-id="582ec-139">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="582ec-139">Build the project.</span></span>

### <a name="pig-udf"></a><span data-ttu-id="582ec-140">Pig UDF</span><span class="sxs-lookup"><span data-stu-id="582ec-140">Pig UDF</span></span>

1. <span data-ttu-id="582ec-141">Otevřete Visual Studio a vytvořte řešení.</span><span class="sxs-lookup"><span data-stu-id="582ec-141">Open Visual Studio and create a solution.</span></span> <span data-ttu-id="582ec-142">Typ projektu vyberte **konzolové aplikace**a název nového projektu **PigUDF**.</span><span class="sxs-lookup"><span data-stu-id="582ec-142">For the project type, select **Console Application**, and name the new project **PigUDF**.</span></span>

2. <span data-ttu-id="582ec-143">Nahraďte obsah **Program.cs** soubor s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="582ec-143">Replace the contents of the **Program.cs** file with the following code:</span></span>

    ```csharp
    using System;

    namespace PigUDF
    {
        class Program
        {
            static void Main(string[] args)
            {
                string line;
                // Read stdin in a loop
                while ((line = Console.ReadLine()) != null)
                {
                    // Fix formatting on lines that begin with an exception
                    if(line.StartsWith("java.lang.Exception"))
                    {
                        // Trim the error info off the beginning and add a note to the end of the line
                        line = line.Remove(0, 21) + " - java.lang.Exception";
                    }
                    // Split the fields apart at tab characters
                    string[] field = line.Split('\t');
                    // Put fields back together for writing
                    Console.WriteLine(String.Join("\t",field));
                }
            }
        }
    }
    ```

    <span data-ttu-id="582ec-144">Tato aplikace analyzuje řádky odeslané z Pig a přeformátovat řádků, které začínají `java.lang.Exception`.</span><span class="sxs-lookup"><span data-stu-id="582ec-144">This application parses the lines sent from Pig, and reformat lines that begin with `java.lang.Exception`.</span></span>

3. <span data-ttu-id="582ec-145">Uložit **Program.cs**a potom sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="582ec-145">Save **Program.cs**, and then build the project.</span></span>

## <a name="upload-to-storage"></a><span data-ttu-id="582ec-146">Nahrání do úložiště</span><span class="sxs-lookup"><span data-stu-id="582ec-146">Upload to storage</span></span>

1. <span data-ttu-id="582ec-147">V sadě Visual Studio otevřete **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="582ec-147">In Visual Studio, open **Server Explorer**.</span></span>

2. <span data-ttu-id="582ec-148">Rozbalte položku **Azure** a pak rozbalte **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="582ec-148">Expand **Azure**, and then expand **HDInsight**.</span></span>

3. <span data-ttu-id="582ec-149">Po zobrazení výzvy zadejte své přihlašovací údaje předplatného Azure a pak klikněte na tlačítko **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="582ec-149">If prompted, enter your Azure subscription credentials, and then click **Sign In**.</span></span>

4. <span data-ttu-id="582ec-150">Rozbalte položku, kterou chcete nasadit tuto aplikaci do clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="582ec-150">Expand the HDInsight cluster that you wish to deploy this application to.</span></span> <span data-ttu-id="582ec-151">Položku s textem __(výchozí účet úložiště)__ je uveden.</span><span class="sxs-lookup"><span data-stu-id="582ec-151">An entry with the text __(Default Storage Account)__ is listed.</span></span>

    ![Průzkumník serveru zobrazující účet úložiště pro cluster](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

    * <span data-ttu-id="582ec-153">Pokud tuto položku lze rozšířit, kterou používáte __účet úložiště Azure__ jako výchozí úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="582ec-153">If this entry can be expanded, you are using an __Azure Storage Account__ as default storage for the cluster.</span></span> <span data-ttu-id="582ec-154">Chcete-li zobrazit soubory na výchozí úložiště pro cluster, rozbalte položku a dvakrát __(výchozí kontejner)__.</span><span class="sxs-lookup"><span data-stu-id="582ec-154">To view the files on the default storage for the cluster, expand the entry and then double-click the __(Default Container)__.</span></span>

    * <span data-ttu-id="582ec-155">Pokud tuto položku nelze rozšířit, používáte __Azure Data Lake Store__ jako výchozí úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="582ec-155">If this entry cannot be expanded, you are using __Azure Data Lake Store__ as the default storage for the cluster.</span></span> <span data-ttu-id="582ec-156">Chcete-li zobrazit soubory na výchozí úložiště pro cluster, dvakrát klikněte na __(výchozí účet úložiště)__ položku.</span><span class="sxs-lookup"><span data-stu-id="582ec-156">To view the files on the default storage for the cluster, double-click the __(Default Storage Account)__ entry.</span></span>

6. <span data-ttu-id="582ec-157">Pokud chcete nahrát soubory .exe, použijte jednu z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="582ec-157">To upload the .exe files, use one of the following methods:</span></span>

    * <span data-ttu-id="582ec-158">Pokud se používá __účet úložiště Azure__, klikněte na ikonu nahrávání a potom vyhledejte **bin\debug** složku pro **HiveCSharp** projektu.</span><span class="sxs-lookup"><span data-stu-id="582ec-158">If using an __Azure Storage Account__, click the upload icon, and then browse to the **bin\debug** folder for the **HiveCSharp** project.</span></span> <span data-ttu-id="582ec-159">Nakonec vyberte **HiveCSharp.exe** souboru a klikněte na tlačítko **Ok**.</span><span class="sxs-lookup"><span data-stu-id="582ec-159">Finally, select the **HiveCSharp.exe** file and click **Ok**.</span></span>

        ![Nahrajte ikonu](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/upload.png)
    
    * <span data-ttu-id="582ec-161">Pokud používáte __Azure Data Lake Store__, klikněte pravým tlačítkem myši na prázdnou oblast v seznamu souboru a pak vyberte __nahrát__.</span><span class="sxs-lookup"><span data-stu-id="582ec-161">If using __Azure Data Lake Store__, right-click an empty area in the file listing, and then select __Upload__.</span></span> <span data-ttu-id="582ec-162">Nakonec vyberte **HiveCSharp.exe** souboru a klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="582ec-162">Finally, select the **HiveCSharp.exe** file and click **Open**.</span></span>

    <span data-ttu-id="582ec-163">Jednou __HiveCSharp.exe__ odesílání skončí, opakujte tento postup nahrání pro __PigUDF.exe__ souboru.</span><span class="sxs-lookup"><span data-stu-id="582ec-163">Once the __HiveCSharp.exe__ upload has finished, repeat the upload process for the __PigUDF.exe__ file.</span></span>

## <a name="run-a-hive-query"></a><span data-ttu-id="582ec-164">Spuštění dotazu Hive</span><span class="sxs-lookup"><span data-stu-id="582ec-164">Run a Hive query</span></span>

1. <span data-ttu-id="582ec-165">V sadě Visual Studio otevřete **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="582ec-165">In Visual Studio, open **Server Explorer**.</span></span>

2. <span data-ttu-id="582ec-166">Rozbalte položku **Azure** a pak rozbalte **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="582ec-166">Expand **Azure**, and then expand **HDInsight**.</span></span>

3. <span data-ttu-id="582ec-167">Pravým tlačítkem na cluster, který jste nasadili **HiveCSharp** do aplikace a pak vyberte **napsat dotaz Hive**.</span><span class="sxs-lookup"><span data-stu-id="582ec-167">Right-click the cluster that you deployed the **HiveCSharp** application to, and then select **Write a Hive Query**.</span></span>

4. <span data-ttu-id="582ec-168">Použijte následující text pro dotaz Hive:</span><span class="sxs-lookup"><span data-stu-id="582ec-168">Use the following text for the Hive query:</span></span>

    ```hiveql
    -- Uncomment the following if you are using Azure Storage
    -- add file wasb:///HiveCSharp.exe;
    -- Uncomment the following if you are using Azure Data Lake Store
    -- add file adl:///HiveCSharp.exe;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'HiveCSharp.exe' AS
    (clientid string, phoneLabel string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="582ec-169">Zrušením komentáře u `add file` příkaz, který odpovídá typu použitého výchozí úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="582ec-169">Uncomment the `add file` statement that matches the type of default storage used for your cluster.</span></span>

    <span data-ttu-id="582ec-170">Tento dotaz vybere `clientid`, `devicemake`, a `devicemodel` pole z `hivesampletable`a předá pole HiveCSharp.exe aplikaci.</span><span class="sxs-lookup"><span data-stu-id="582ec-170">This query selects the `clientid`, `devicemake`, and `devicemodel` fields from `hivesampletable`, and passes the fields to the HiveCSharp.exe application.</span></span> <span data-ttu-id="582ec-171">Dotaz očekává aplikaci, aby vracela tři pole, které jsou uloženy jako `clientid`, `phoneLabel`, a `phoneHash`.</span><span class="sxs-lookup"><span data-stu-id="582ec-171">The query expects the application to return three fields, which are stored as `clientid`, `phoneLabel`, and `phoneHash`.</span></span> <span data-ttu-id="582ec-172">Dotaz se také očekává, že HiveCSharp.exe najít v kořenovém kontejneru výchozí úložiště.</span><span class="sxs-lookup"><span data-stu-id="582ec-172">The query also expects to find HiveCSharp.exe in the root of the default storage container.</span></span>

5. <span data-ttu-id="582ec-173">Klikněte na tlačítko **odeslání** se odeslat úlohu do clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="582ec-173">Click **Submit** to submit the job to the HDInsight cluster.</span></span> <span data-ttu-id="582ec-174">**Souhrn úlohy Hive** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="582ec-174">The **Hive Job Summary** window opens.</span></span>

6. <span data-ttu-id="582ec-175">Klikněte na tlačítko **aktualizovat** aktualizovat souhrn dokud **stav úlohy** změny **dokončeno**.</span><span class="sxs-lookup"><span data-stu-id="582ec-175">Click **Refresh** to refresh the summary until **Job Status** changes to **Completed**.</span></span> <span data-ttu-id="582ec-176">Chcete-li zobrazit výstup úlohy, klikněte na tlačítko **výstup úlohy**.</span><span class="sxs-lookup"><span data-stu-id="582ec-176">To view the job output, click **Job Output**.</span></span>

## <a name="run-a-pig-job"></a><span data-ttu-id="582ec-177">Spustit úlohu Pig</span><span class="sxs-lookup"><span data-stu-id="582ec-177">Run a Pig job</span></span>

1. <span data-ttu-id="582ec-178">Pro připojení ke svému clusteru HDInsight, použijte jednu z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="582ec-178">Use one of the following methods to connect to your HDInsight cluster:</span></span>

    * <span data-ttu-id="582ec-179">Pokud používáte __systémem Linux__ HDInsight clusteru, použijte SSH.</span><span class="sxs-lookup"><span data-stu-id="582ec-179">If you are using a __Linux-based__ HDInsight cluster, use SSH.</span></span> <span data-ttu-id="582ec-180">Například, `ssh sshuser@mycluster-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="582ec-180">For example, `ssh sshuser@mycluster-ssh.azurehdinsight.net`.</span></span> <span data-ttu-id="582ec-181">Další informace najdete v tématu [withHDInsight použití SSH](hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="582ec-181">For more information, see [Use SSH withHDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>
    
    * <span data-ttu-id="582ec-182">Pokud používáte __založené na Windows__ clusteru HDInsight, [připojit ke clusteru pomocí vzdálené plochy](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)</span><span class="sxs-lookup"><span data-stu-id="582ec-182">If you are using a __Windows-based__ HDInsight cluster, [Connect to the cluster using Remote Desktop](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)</span></span>

2. <span data-ttu-id="582ec-183">Pomocí jedné následujícího příkazu spusťte Pig příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="582ec-183">Use one the following command to start the Pig command line:</span></span>

        pig

    > [!IMPORTANT]
    > <span data-ttu-id="582ec-184">Pokud používáte cluster systému Windows, použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="582ec-184">If you are using a Windows-based cluster, use the following commands instead:</span></span>
    > ```
    > cd %PIG_HOME%
    > bin\pig
    > ```

    <span data-ttu-id="582ec-185">A `grunt>` se zobrazí výzva.</span><span class="sxs-lookup"><span data-stu-id="582ec-185">A `grunt>` prompt is displayed.</span></span>

3. <span data-ttu-id="582ec-186">Zadejte následující příkaz pro spuštění úlohy Pig, která používá aplikace rozhraní .NET Framework:</span><span class="sxs-lookup"><span data-stu-id="582ec-186">Enter the following to run a Pig job that uses the .NET Framework application:</span></span>

        DEFINE streamer `PigUDF.exe` CACHE('/PigUDF.exe');
        LOGS = LOAD '/example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = STREAM LOG through streamer as (col1, col2, col3, col4, col5);
        DUMP DETAILS;

    <span data-ttu-id="582ec-187">`DEFINE` Příkaz vytvoří zástupce `streamer` pro aplikace pigudf.exe a `CACHE` načte z výchozí úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="582ec-187">The `DEFINE` statement creates an alias of `streamer` for the pigudf.exe applications, and `CACHE` loads it from default storage for the cluster.</span></span> <span data-ttu-id="582ec-188">Později `streamer` se používá s `STREAM` operátor ke zpracování jedné řádků obsažená v protokolu a vrátí data jako řadu sloupců.</span><span class="sxs-lookup"><span data-stu-id="582ec-188">Later, `streamer` is used with the `STREAM` operator to process the single lines contained in LOG and return the data as a series of columns.</span></span>

    > [!NOTE]
    > <span data-ttu-id="582ec-189">Název aplikace, který se používá pro streamování, musí být uzavřena do \` (backtick) při znak alias, a "(jednoduchou uvozovku) při použití s `SHIP\`.</span><span class="sxs-lookup"><span data-stu-id="582ec-189">The application name that is used for streaming must be surrounded by the \` (backtick) character when aliased, and ' (single quote) when used with `SHIP\`.</span></span>

4. <span data-ttu-id="582ec-190">Po zadání poslední řádek, by měla spustit úlohu.</span><span class="sxs-lookup"><span data-stu-id="582ec-190">After entering the last line, the job should start.</span></span> <span data-ttu-id="582ec-191">Vrátí výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="582ec-191">It returns output similar to the following text:</span></span>

        (2012-02-03 20:11:56 SampleClass5 [WARN] problem finding id 1358451042 - java.lang.Exception)
        (2012-02-03 20:11:56 SampleClass5 [DEBUG] detail for id 1976092771)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1317358561)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1737534798)
        (2012-02-03 20:11:56 SampleClass7 [DEBUG] detail for id 1475865947)

## <a name="next-steps"></a><span data-ttu-id="582ec-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="582ec-192">Next steps</span></span>

<span data-ttu-id="582ec-193">V tomto dokumentu jste se naučili, jak používat rozhraní .NET Framework aplikace z Hive a Pig v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="582ec-193">In this document, you have learned how to use a .NET Framework application from Hive and Pig on HDInsight.</span></span> <span data-ttu-id="582ec-194">Pokud chcete další informace o použití Python s Hive a Pig, přečtěte si téma [použít Python s Hive a Pig v HDInsight](hdinsight-python.md).</span><span class="sxs-lookup"><span data-stu-id="582ec-194">If you would like to learn how to use Python with Hive and Pig, see [Use Python with Hive and Pig in HDInsight](hdinsight-python.md).</span></span>

<span data-ttu-id="582ec-195">Pro jiné způsoby používání Pig a Hive a další informace o použití prostředí MapReduce najdete v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="582ec-195">For other ways to use Pig and Hive, and to learn about using MapReduce, see the following documents:</span></span>

* [<span data-ttu-id="582ec-196">Použití Hivu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="582ec-196">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="582ec-197">Použití Pigu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="582ec-197">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="582ec-198">Používání nástroje MapReduce s HDInsight</span><span class="sxs-lookup"><span data-stu-id="582ec-198">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)
