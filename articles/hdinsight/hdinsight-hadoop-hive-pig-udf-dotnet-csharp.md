---
title: "aaaUse C# s Hive a Pig systému Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse C# uživatelem definované funkce (UDF) s Hive a Pig streamování v Azure HDInsight."
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
ms.openlocfilehash: dd35409766f2dafe4d8050c3f9bc351949473ad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-c-user-defined-functions-with-hive-and-pig-streaming-on-hadoop-in-hdinsight"></a><span data-ttu-id="692fe-103">Uživatelem definované funkce jazyka C# pomocí Hive a Pig streamování systému Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="692fe-103">Use C# user-defined functions with Hive and Pig streaming on Hadoop in HDInsight</span></span>

<span data-ttu-id="692fe-104">Zjistěte, jak toouse C# uživatele definovaných funkcí (UDF) s Apache Hive a Pig v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="692fe-104">Learn how toouse C# user defined functions (UDF) with Apache Hive and Pig on HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="692fe-105">Hello kroky v tomto dokumentu pracovat s clustery HDInsight se systémem Linux i systému Windows.</span><span class="sxs-lookup"><span data-stu-id="692fe-105">hello steps in this document work with both Linux-based and Windows-based HDInsight clusters.</span></span> <span data-ttu-id="692fe-106">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="692fe-106">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="692fe-107">Další informace najdete v tématu [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="692fe-107">For more information, see [HDInsight component versioning](hdinsight-component-versioning.md).</span></span>

<span data-ttu-id="692fe-108">Obě Hive a Pig můžete předat data tooexternal aplikací pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="692fe-108">Both Hive and Pig can pass data tooexternal applications for processing.</span></span> <span data-ttu-id="692fe-109">Tento proces se označuje jako _streamování_.</span><span class="sxs-lookup"><span data-stu-id="692fe-109">This process is known as _streaming_.</span></span> <span data-ttu-id="692fe-110">Při použití aplikační rozhraní .NET, hello data předávána toohello aplikace na stdin – a aplikace hello vrátí hello výsledky do datového proudu STDOUT.</span><span class="sxs-lookup"><span data-stu-id="692fe-110">When using a .NET applciation, hello data is passed toohello application on STDIN, and hello application returns hello results on STDOUT.</span></span> <span data-ttu-id="692fe-111">tooread a zápisu ze standardního vstupu a výstupu STDOUT, můžete použít `Console.ReadLine()` a `Console.WriteLine()` z konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="692fe-111">tooread and write from STDIN and STDOUT, you can use `Console.ReadLine()` and `Console.WriteLine()` from a console application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="692fe-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="692fe-112">Prerequisites</span></span>

* <span data-ttu-id="692fe-113">Znalost zápis a sestavování kód C#, která je cílena rozhraní .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="692fe-113">A familiarity with writing and building C# code that targets .NET Framework 4.5.</span></span>

    * <span data-ttu-id="692fe-114">Ať IDE, které chcete použijte.</span><span class="sxs-lookup"><span data-stu-id="692fe-114">Use whatever IDE you want.</span></span> <span data-ttu-id="692fe-115">Doporučujeme, abyste [Visual Studio](https://www.visualstudio.com/vs) 2015, 2017, nebo [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="692fe-115">We recommend [Visual Studio](https://www.visualstudio.com/vs) 2015, 2017, or [Visual Studio Code](https://code.visualstudio.com/).</span></span> <span data-ttu-id="692fe-116">Hello kroky v tomto dokumentu Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="692fe-116">hello steps in this document use Visual Studio 2017.</span></span>

* <span data-ttu-id="692fe-117">Tooupload .exe způsob soubory toohello clusteru a spuštění úlohy Pig a Hive.</span><span class="sxs-lookup"><span data-stu-id="692fe-117">A way tooupload .exe files toohello cluster and run Pig and Hive jobs.</span></span> <span data-ttu-id="692fe-118">Doporučujeme, abyste hello nástrojů Data Lake pro Visual Studio, prostředí Azure PowerShell a rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="692fe-118">We recommend hello Data Lake Tools for Visual Studio, Azure PowerShell and Azure CLI.</span></span> <span data-ttu-id="692fe-119">Hello kroky v tomto dokumentu pomocí hello nástrojů Data Lake pro Visual Studio tooupload hello soubory a spusťte dotaz Hive příklad hello.</span><span class="sxs-lookup"><span data-stu-id="692fe-119">hello steps in this document use hello Data Lake Tools for Visual Studio tooupload hello files and run hello example Hive query.</span></span>

    <span data-ttu-id="692fe-120">Informace o dalších dotazů Hive toorun způsoby a úlohy Pig najdete v části hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="692fe-120">For information on other ways toorun Hive queries and Pig jobs, see hello following documents:</span></span>

    * [<span data-ttu-id="692fe-121">Použijte Apache Hive s HDInsight</span><span class="sxs-lookup"><span data-stu-id="692fe-121">Use Apache Hive with HDInsight</span></span>](hdinsight-use-hive.md)

    * [<span data-ttu-id="692fe-122">Použijte Apache Pig s HDInsight</span><span class="sxs-lookup"><span data-stu-id="692fe-122">Use Apache Pig with HDInsight</span></span>](hdinsight-use-pig.md)

* <span data-ttu-id="692fe-123">Hadoop v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="692fe-123">A Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="692fe-124">Další informace týkající se vytvoření clusteru najdete v tématu [vytvoření clusteru HDInsight](hdinsight-provision-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="692fe-124">For more information on creating a cluster, see [Create an HDInsight cluster](hdinsight-provision-clusters.md).</span></span>

## <a name="net-on-hdinsight"></a><span data-ttu-id="692fe-125">Rozhraní .NET v HDInsight</span><span class="sxs-lookup"><span data-stu-id="692fe-125">.NET on HDInsight</span></span>

* <span data-ttu-id="692fe-126">__HDInsight se systémem Linux__ clusterů pomocí [Mono (https://mono-project.com)](https://mono-project.com) toorun aplikací .NET.</span><span class="sxs-lookup"><span data-stu-id="692fe-126">__Linux-based HDInsight__ clusters using [Mono (https://mono-project.com)](https://mono-project.com) toorun .NET applications.</span></span> <span data-ttu-id="692fe-127">Monofonní verze 4.2.1 je součástí HDInsight verze 3.5.</span><span class="sxs-lookup"><span data-stu-id="692fe-127">Mono version 4.2.1 is included with HDInsight version 3.5.</span></span>

    <span data-ttu-id="692fe-128">Další informace o Mono kompatibilitu s verzí rozhraní .NET Framework naleznete v tématu [Mono kompatibility](http://www.mono-project.com/docs/about-mono/compatibility/).</span><span class="sxs-lookup"><span data-stu-id="692fe-128">For more information on Mono compatibility with .NET Framework versions, see [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/).</span></span>

    <span data-ttu-id="692fe-129">toouse na konkrétní verzi Mono, najdete v části hello [instalace nebo aktualizace Mono](hdinsight-hadoop-install-mono.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="692fe-129">toouse a specific version of Mono, see hello [Install or update Mono](hdinsight-hadoop-install-mono.md) document.</span></span>

* <span data-ttu-id="692fe-130">__HDInsight se systémem Windows__ clustery pomocí aplikace .NET toorun hello Microsoft .NET CLR.</span><span class="sxs-lookup"><span data-stu-id="692fe-130">__Windows-based HDInsight__ clusters use hello Microsoft .NET CLR toorun .NET applications.</span></span>

<span data-ttu-id="692fe-131">Další informace o verzi hello hello rozhraní .NET framework a Mono součástí verzích HDInsight naleznete v tématu [HDInsight verze součástí](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="692fe-131">For more information on hello version of hello .NET framework and Mono included with HDInsight versions, see [HDInsight component versions](hdinsight-component-versioning.md).</span></span>

## <a name="create-hello-c-projects"></a><span data-ttu-id="692fe-132">Vytvoření hello C\# projekty</span><span class="sxs-lookup"><span data-stu-id="692fe-132">Create hello C\# projects</span></span>

### <a name="hive-udf"></a><span data-ttu-id="692fe-133">Hive UDF</span><span class="sxs-lookup"><span data-stu-id="692fe-133">Hive UDF</span></span>

1. <span data-ttu-id="692fe-134">Otevřete Visual Studio a vytvořte řešení.</span><span class="sxs-lookup"><span data-stu-id="692fe-134">Open Visual Studio and create a solution.</span></span> <span data-ttu-id="692fe-135">Typ projektu hello vyberte **konzolovou aplikaci (rozhraní .NET Framework)**a název hello nový projekt **HiveCSharp**.</span><span class="sxs-lookup"><span data-stu-id="692fe-135">For hello project type, select **Console App (.NET Framework)**, and name hello new project **HiveCSharp**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="692fe-136">Vyberte __rozhraní .NET Framework 4.5__ Pokud používáte cluster HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="692fe-136">Select __.NET Framework 4.5__ if you are using a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="692fe-137">Další informace o Mono kompatibilitu s verzí rozhraní .NET Framework naleznete v tématu [Mono kompatibility](http://www.mono-project.com/docs/about-mono/compatibility/).</span><span class="sxs-lookup"><span data-stu-id="692fe-137">For more information on Mono compatibility with .NET Framework versions, see [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/).</span></span>

2. <span data-ttu-id="692fe-138">Nahraďte obsah hello **Program.cs** s hello následující:</span><span class="sxs-lookup"><span data-stu-id="692fe-138">Replace hello contents of **Program.cs** with hello following:</span></span>

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
                    // Parse hello string, trimming line feeds
                    // and splitting fields at tabs
                    line = line.TrimEnd('\n');
                    string[] field = line.Split('\t');
                    string phoneLabel = field[1] + ' ' + field[2];
                    // Emit new data toostdout, delimited by tabs
                    Console.WriteLine("{0}\t{1}\t{2}", field[0], phoneLabel, GetMD5Hash(phoneLabel));
                }
            }
            /// <summary>
            /// Returns an MD5 hash for hello given string
            /// </summary>
            /// <param name="input">string value</param>
            /// <returns>an MD5 hash</returns>
            static string GetMD5Hash(string input)
            {
                // Step 1, calculate MD5 hash from input
                MD5 md5 = System.Security.Cryptography.MD5.Create();
                byte[] inputBytes = System.Text.Encoding.ASCII.GetBytes(input);
                byte[] hash = md5.ComputeHash(inputBytes);

                // Step 2, convert byte array toohex string
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

3. <span data-ttu-id="692fe-139">Sestavení projektu hello.</span><span class="sxs-lookup"><span data-stu-id="692fe-139">Build hello project.</span></span>

### <a name="pig-udf"></a><span data-ttu-id="692fe-140">Pig UDF</span><span class="sxs-lookup"><span data-stu-id="692fe-140">Pig UDF</span></span>

1. <span data-ttu-id="692fe-141">Otevřete Visual Studio a vytvořte řešení.</span><span class="sxs-lookup"><span data-stu-id="692fe-141">Open Visual Studio and create a solution.</span></span> <span data-ttu-id="692fe-142">Typ projektu hello vyberte **konzolové aplikace**a název hello nový projekt **PigUDF**.</span><span class="sxs-lookup"><span data-stu-id="692fe-142">For hello project type, select **Console Application**, and name hello new project **PigUDF**.</span></span>

2. <span data-ttu-id="692fe-143">Nahraďte obsah hello hello **Program.cs** soubor s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="692fe-143">Replace hello contents of hello **Program.cs** file with hello following code:</span></span>

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
                        // Trim hello error info off hello beginning and add a note toohello end of hello line
                        line = line.Remove(0, 21) + " - java.lang.Exception";
                    }
                    // Split hello fields apart at tab characters
                    string[] field = line.Split('\t');
                    // Put fields back together for writing
                    Console.WriteLine(String.Join("\t",field));
                }
            }
        }
    }
    ```

    <span data-ttu-id="692fe-144">Tato aplikace analyzuje řádky hello odeslaný Pig a přeformátovat řádky, které začínají `java.lang.Exception`.</span><span class="sxs-lookup"><span data-stu-id="692fe-144">This application parses hello lines sent from Pig, and reformat lines that begin with `java.lang.Exception`.</span></span>

3. <span data-ttu-id="692fe-145">Uložit **Program.cs**a následně vytvořit projekt hello.</span><span class="sxs-lookup"><span data-stu-id="692fe-145">Save **Program.cs**, and then build hello project.</span></span>

## <a name="upload-toostorage"></a><span data-ttu-id="692fe-146">Nahrát toostorage</span><span class="sxs-lookup"><span data-stu-id="692fe-146">Upload toostorage</span></span>

1. <span data-ttu-id="692fe-147">V sadě Visual Studio otevřete **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="692fe-147">In Visual Studio, open **Server Explorer**.</span></span>

2. <span data-ttu-id="692fe-148">Rozbalte položku **Azure** a pak rozbalte **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="692fe-148">Expand **Azure**, and then expand **HDInsight**.</span></span>

3. <span data-ttu-id="692fe-149">Po zobrazení výzvy zadejte své přihlašovací údaje předplatného Azure a pak klikněte na tlačítko **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="692fe-149">If prompted, enter your Azure subscription credentials, and then click **Sign In**.</span></span>

4. <span data-ttu-id="692fe-150">Rozbalte cluster HDInsight hello, který chcete toodeploy této aplikace.</span><span class="sxs-lookup"><span data-stu-id="692fe-150">Expand hello HDInsight cluster that you wish toodeploy this application to.</span></span> <span data-ttu-id="692fe-151">Položku s textem hello __(výchozí účet úložiště)__ je uveden.</span><span class="sxs-lookup"><span data-stu-id="692fe-151">An entry with hello text __(Default Storage Account)__ is listed.</span></span>

    ![Průzkumník serveru zobrazující hello účet úložiště pro hello cluster](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

    * <span data-ttu-id="692fe-153">Pokud tuto položku lze rozšířit, kterou používáte __účet úložiště Azure__ jako výchozí úložiště pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="692fe-153">If this entry can be expanded, you are using an __Azure Storage Account__ as default storage for hello cluster.</span></span> <span data-ttu-id="692fe-154">soubory hello tooview na hello výchozí úložiště pro hello cluster, rozbalte položku hello a potom dvakrát klikněte na hello __(výchozí kontejner)__.</span><span class="sxs-lookup"><span data-stu-id="692fe-154">tooview hello files on hello default storage for hello cluster, expand hello entry and then double-click hello __(Default Container)__.</span></span>

    * <span data-ttu-id="692fe-155">Pokud tuto položku nelze rozšířit, používáte __Azure Data Lake Store__ jako hello výchozí úložiště pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="692fe-155">If this entry cannot be expanded, you are using __Azure Data Lake Store__ as hello default storage for hello cluster.</span></span> <span data-ttu-id="692fe-156">soubory hello tooview na hello výchozí úložiště pro hello cluster, dvakrát klikněte na hello __(výchozí účet úložiště)__ položku.</span><span class="sxs-lookup"><span data-stu-id="692fe-156">tooview hello files on hello default storage for hello cluster, double-click hello __(Default Storage Account)__ entry.</span></span>

6. <span data-ttu-id="692fe-157">soubory .exe tooupload hello, použijte jednu z následujících metod hello:</span><span class="sxs-lookup"><span data-stu-id="692fe-157">tooupload hello .exe files, use one of hello following methods:</span></span>

    * <span data-ttu-id="692fe-158">Pokud se používá __účet úložiště Azure__, klikněte na ikonu hello nahrávání a vyhledejte toohello **bin\debug** složku pro hello **HiveCSharp** projektu.</span><span class="sxs-lookup"><span data-stu-id="692fe-158">If using an __Azure Storage Account__, click hello upload icon, and then browse toohello **bin\debug** folder for hello **HiveCSharp** project.</span></span> <span data-ttu-id="692fe-159">Nakonec vyberte hello **HiveCSharp.exe** souboru a klikněte na tlačítko **Ok**.</span><span class="sxs-lookup"><span data-stu-id="692fe-159">Finally, select hello **HiveCSharp.exe** file and click **Ok**.</span></span>

        ![Nahrajte ikonu](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/upload.png)
    
    * <span data-ttu-id="692fe-161">Pokud používáte __Azure Data Lake Store__, klikněte pravým tlačítkem myši na prázdnou oblast v seznamu hello souboru a pak vyberte __nahrát__.</span><span class="sxs-lookup"><span data-stu-id="692fe-161">If using __Azure Data Lake Store__, right-click an empty area in hello file listing, and then select __Upload__.</span></span> <span data-ttu-id="692fe-162">Nakonec vyberte hello **HiveCSharp.exe** souboru a klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="692fe-162">Finally, select hello **HiveCSharp.exe** file and click **Open**.</span></span>

    <span data-ttu-id="692fe-163">Jednou hello __HiveCSharp.exe__ nahrávání dokončí, proces odesílání opakování hello hello __PigUDF.exe__ souboru.</span><span class="sxs-lookup"><span data-stu-id="692fe-163">Once hello __HiveCSharp.exe__ upload has finished, repeat hello upload process for hello __PigUDF.exe__ file.</span></span>

## <a name="run-a-hive-query"></a><span data-ttu-id="692fe-164">Spuštění dotazu Hive</span><span class="sxs-lookup"><span data-stu-id="692fe-164">Run a Hive query</span></span>

1. <span data-ttu-id="692fe-165">V sadě Visual Studio otevřete **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="692fe-165">In Visual Studio, open **Server Explorer**.</span></span>

2. <span data-ttu-id="692fe-166">Rozbalte položku **Azure** a pak rozbalte **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="692fe-166">Expand **Azure**, and then expand **HDInsight**.</span></span>

3. <span data-ttu-id="692fe-167">Klikněte pravým tlačítkem na hello clusteru, že jste nasadili hello **HiveCSharp** do aplikace a pak vyberte **napsat dotaz Hive**.</span><span class="sxs-lookup"><span data-stu-id="692fe-167">Right-click hello cluster that you deployed hello **HiveCSharp** application to, and then select **Write a Hive Query**.</span></span>

4. <span data-ttu-id="692fe-168">Použijte následující text pro dotaz Hive hello hello:</span><span class="sxs-lookup"><span data-stu-id="692fe-168">Use hello following text for hello Hive query:</span></span>

    ```hiveql
    -- Uncomment hello following if you are using Azure Storage
    -- add file wasb:///HiveCSharp.exe;
    -- Uncomment hello following if you are using Azure Data Lake Store
    -- add file adl:///HiveCSharp.exe;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'HiveCSharp.exe' AS
    (clientid string, phoneLabel string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="692fe-169">Zrušením komentáře u hello `add file` příkaz, který odpovídá typu hello výchozí úložiště použitého pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="692fe-169">Uncomment hello `add file` statement that matches hello type of default storage used for your cluster.</span></span>

    <span data-ttu-id="692fe-170">Tento dotaz vybere hello `clientid`, `devicemake`, a `devicemodel` pole z `hivesampletable`a předá hello pole toohello HiveCSharp.exe aplikace.</span><span class="sxs-lookup"><span data-stu-id="692fe-170">This query selects hello `clientid`, `devicemake`, and `devicemodel` fields from `hivesampletable`, and passes hello fields toohello HiveCSharp.exe application.</span></span> <span data-ttu-id="692fe-171">dotaz Hello očekává tři pole tooreturn se aplikace hello, které jsou uloženy jako `clientid`, `phoneLabel`, a `phoneHash`.</span><span class="sxs-lookup"><span data-stu-id="692fe-171">hello query expects hello application tooreturn three fields, which are stored as `clientid`, `phoneLabel`, and `phoneHash`.</span></span> <span data-ttu-id="692fe-172">dotaz Hello také očekává toofind HiveCSharp.exe hello kořenové hello výchozí kontejner úložiště.</span><span class="sxs-lookup"><span data-stu-id="692fe-172">hello query also expects toofind HiveCSharp.exe in hello root of hello default storage container.</span></span>

5. <span data-ttu-id="692fe-173">Klikněte na tlačítko **odeslání** toosubmit hello úlohy toohello HDInsight clusteru.</span><span class="sxs-lookup"><span data-stu-id="692fe-173">Click **Submit** toosubmit hello job toohello HDInsight cluster.</span></span> <span data-ttu-id="692fe-174">Hello **Souhrn úlohy Hive** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="692fe-174">hello **Hive Job Summary** window opens.</span></span>

6. <span data-ttu-id="692fe-175">Klikněte na tlačítko **aktualizovat** toorefresh hello souhrnné dokud **stav úlohy** změní příliš**dokončeno**.</span><span class="sxs-lookup"><span data-stu-id="692fe-175">Click **Refresh** toorefresh hello summary until **Job Status** changes too**Completed**.</span></span> <span data-ttu-id="692fe-176">výstup úlohy hello tooview, klikněte na tlačítko **výstup úlohy**.</span><span class="sxs-lookup"><span data-stu-id="692fe-176">tooview hello job output, click **Job Output**.</span></span>

## <a name="run-a-pig-job"></a><span data-ttu-id="692fe-177">Spustit úlohu Pig</span><span class="sxs-lookup"><span data-stu-id="692fe-177">Run a Pig job</span></span>

1. <span data-ttu-id="692fe-178">Použijte jednu z následujících clusteru HDInsight tooyour tooconnect metody hello:</span><span class="sxs-lookup"><span data-stu-id="692fe-178">Use one of hello following methods tooconnect tooyour HDInsight cluster:</span></span>

    * <span data-ttu-id="692fe-179">Pokud používáte __systémem Linux__ HDInsight clusteru, použijte SSH.</span><span class="sxs-lookup"><span data-stu-id="692fe-179">If you are using a __Linux-based__ HDInsight cluster, use SSH.</span></span> <span data-ttu-id="692fe-180">Například, `ssh sshuser@mycluster-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="692fe-180">For example, `ssh sshuser@mycluster-ssh.azurehdinsight.net`.</span></span> <span data-ttu-id="692fe-181">Další informace najdete v tématu [withHDInsight použití SSH](hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="692fe-181">For more information, see [Use SSH withHDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>
    
    * <span data-ttu-id="692fe-182">Pokud používáte __založené na Windows__ clusteru HDInsight, [clusteru toohello připojit pomocí vzdálené plochy](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)</span><span class="sxs-lookup"><span data-stu-id="692fe-182">If you are using a __Windows-based__ HDInsight cluster, [Connect toohello cluster using Remote Desktop](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)</span></span>

2. <span data-ttu-id="692fe-183">Použijte jeden hello následující příkaz toostart hello Pig příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="692fe-183">Use one hello following command toostart hello Pig command line:</span></span>

        pig

    > [!IMPORTANT]
    > <span data-ttu-id="692fe-184">Pokud používáte cluster systému Windows, použijte hello místo následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="692fe-184">If you are using a Windows-based cluster, use hello following commands instead:</span></span>
    > ```
    > cd %PIG_HOME%
    > bin\pig
    > ```

    <span data-ttu-id="692fe-185">A `grunt>` se zobrazí výzva.</span><span class="sxs-lookup"><span data-stu-id="692fe-185">A `grunt>` prompt is displayed.</span></span>

3. <span data-ttu-id="692fe-186">Zadejte následující toorun Pig úlohu, která používá aplikace rozhraní .NET Framework hello hello:</span><span class="sxs-lookup"><span data-stu-id="692fe-186">Enter hello following toorun a Pig job that uses hello .NET Framework application:</span></span>

        DEFINE streamer `PigUDF.exe` CACHE('/PigUDF.exe');
        LOGS = LOAD '/example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = STREAM LOG through streamer as (col1, col2, col3, col4, col5);
        DUMP DETAILS;

    <span data-ttu-id="692fe-187">Hello `DEFINE` příkaz vytvoří zástupce `streamer` pro hello pigudf.exe aplikace, a `CACHE` načte z výchozí úložiště pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="692fe-187">hello `DEFINE` statement creates an alias of `streamer` for hello pigudf.exe applications, and `CACHE` loads it from default storage for hello cluster.</span></span> <span data-ttu-id="692fe-188">Později `streamer` se používá s hello `STREAM` operátor tooprocess hello jednotlivých řádků obsažená v protokolu a návratové hello data jako řadu sloupců.</span><span class="sxs-lookup"><span data-stu-id="692fe-188">Later, `streamer` is used with hello `STREAM` operator tooprocess hello single lines contained in LOG and return hello data as a series of columns.</span></span>

    > [!NOTE]
    > <span data-ttu-id="692fe-189">název aplikace Hello, který se používá pro streamování, musí být uzavřena do hello \` (backtick) znak při alias, a "(v jednoduchých uvozovkách) při použití s `SHIP\`.</span><span class="sxs-lookup"><span data-stu-id="692fe-189">hello application name that is used for streaming must be surrounded by hello \` (backtick) character when aliased, and ' (single quote) when used with `SHIP\`.</span></span>

4. <span data-ttu-id="692fe-190">Po zadání hello poslední řádek, začněte hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="692fe-190">After entering hello last line, hello job should start.</span></span> <span data-ttu-id="692fe-191">Vrátí výstup podobný toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="692fe-191">It returns output similar toohello following text:</span></span>

        (2012-02-03 20:11:56 SampleClass5 [WARN] problem finding id 1358451042 - java.lang.Exception)
        (2012-02-03 20:11:56 SampleClass5 [DEBUG] detail for id 1976092771)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1317358561)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1737534798)
        (2012-02-03 20:11:56 SampleClass7 [DEBUG] detail for id 1475865947)

## <a name="next-steps"></a><span data-ttu-id="692fe-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="692fe-192">Next steps</span></span>

<span data-ttu-id="692fe-193">V tomto dokumentu jste se naučili jak toouse aplikace rozhraní .NET Framework z Hive a Pig v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="692fe-193">In this document, you have learned how toouse a .NET Framework application from Hive and Pig on HDInsight.</span></span> <span data-ttu-id="692fe-194">Pokud chcete toolearn jak zjistit, toouse Python s Hive a Pig, [Python použití Hive a Pig v HDInsight](hdinsight-python.md).</span><span class="sxs-lookup"><span data-stu-id="692fe-194">If you would like toolearn how toouse Python with Hive and Pig, see [Use Python with Hive and Pig in HDInsight](hdinsight-python.md).</span></span>

<span data-ttu-id="692fe-195">Další způsoby toouse Pig a Hive a toolearn o použití prostředí MapReduce najdete v části hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="692fe-195">For other ways toouse Pig and Hive, and toolearn about using MapReduce, see hello following documents:</span></span>

* [<span data-ttu-id="692fe-196">Použití Hivu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="692fe-196">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="692fe-197">Použití Pigu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="692fe-197">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="692fe-198">Používání nástroje MapReduce s HDInsight</span><span class="sxs-lookup"><span data-stu-id="692fe-198">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)
