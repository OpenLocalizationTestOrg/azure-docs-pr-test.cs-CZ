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
# <a name="use-c-user-defined-functions-with-hive-and-pig-streaming-on-hadoop-in-hdinsight"></a>Uživatelem definované funkce jazyka C# pomocí Hive a Pig streamování systému Hadoop v HDInsight

Zjistěte, jak toouse C# uživatele definovaných funkcí (UDF) s Apache Hive a Pig v HDInsight.

> [!IMPORTANT]
> Hello kroky v tomto dokumentu pracovat s clustery HDInsight se systémem Linux i systému Windows. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md).

Obě Hive a Pig můžete předat data tooexternal aplikací pro zpracování. Tento proces se označuje jako _streamování_. Při použití aplikační rozhraní .NET, hello data předávána toohello aplikace na stdin – a aplikace hello vrátí hello výsledky do datového proudu STDOUT. tooread a zápisu ze standardního vstupu a výstupu STDOUT, můžete použít `Console.ReadLine()` a `Console.WriteLine()` z konzolové aplikace.

## <a name="prerequisites"></a>Požadavky

* Znalost zápis a sestavování kód C#, která je cílena rozhraní .NET Framework 4.5.

    * Ať IDE, které chcete použijte. Doporučujeme, abyste [Visual Studio](https://www.visualstudio.com/vs) 2015, 2017, nebo [Visual Studio Code](https://code.visualstudio.com/). Hello kroky v tomto dokumentu Visual Studio 2017.

* Tooupload .exe způsob soubory toohello clusteru a spuštění úlohy Pig a Hive. Doporučujeme, abyste hello nástrojů Data Lake pro Visual Studio, prostředí Azure PowerShell a rozhraní příkazového řádku Azure. Hello kroky v tomto dokumentu pomocí hello nástrojů Data Lake pro Visual Studio tooupload hello soubory a spusťte dotaz Hive příklad hello.

    Informace o dalších dotazů Hive toorun způsoby a úlohy Pig najdete v části hello následující dokumenty:

    * [Použijte Apache Hive s HDInsight](hdinsight-use-hive.md)

    * [Použijte Apache Pig s HDInsight](hdinsight-use-pig.md)

* Hadoop v clusteru HDInsight. Další informace týkající se vytvoření clusteru najdete v tématu [vytvoření clusteru HDInsight](hdinsight-provision-clusters.md).

## <a name="net-on-hdinsight"></a>Rozhraní .NET v HDInsight

* __HDInsight se systémem Linux__ clusterů pomocí [Mono (https://mono-project.com)](https://mono-project.com) toorun aplikací .NET. Monofonní verze 4.2.1 je součástí HDInsight verze 3.5.

    Další informace o Mono kompatibilitu s verzí rozhraní .NET Framework naleznete v tématu [Mono kompatibility](http://www.mono-project.com/docs/about-mono/compatibility/).

    toouse na konkrétní verzi Mono, najdete v části hello [instalace nebo aktualizace Mono](hdinsight-hadoop-install-mono.md) dokumentu.

* __HDInsight se systémem Windows__ clustery pomocí aplikace .NET toorun hello Microsoft .NET CLR.

Další informace o verzi hello hello rozhraní .NET framework a Mono součástí verzích HDInsight naleznete v tématu [HDInsight verze součástí](hdinsight-component-versioning.md).

## <a name="create-hello-c-projects"></a>Vytvoření hello C\# projekty

### <a name="hive-udf"></a>Hive UDF

1. Otevřete Visual Studio a vytvořte řešení. Typ projektu hello vyberte **konzolovou aplikaci (rozhraní .NET Framework)**a název hello nový projekt **HiveCSharp**.

    > [!IMPORTANT]
    > Vyberte __rozhraní .NET Framework 4.5__ Pokud používáte cluster HDInsight se systémem Linux. Další informace o Mono kompatibilitu s verzí rozhraní .NET Framework naleznete v tématu [Mono kompatibility](http://www.mono-project.com/docs/about-mono/compatibility/).

2. Nahraďte obsah hello **Program.cs** s hello následující:

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

3. Sestavení projektu hello.

### <a name="pig-udf"></a>Pig UDF

1. Otevřete Visual Studio a vytvořte řešení. Typ projektu hello vyberte **konzolové aplikace**a název hello nový projekt **PigUDF**.

2. Nahraďte obsah hello hello **Program.cs** soubor s hello následující kód:

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

    Tato aplikace analyzuje řádky hello odeslaný Pig a přeformátovat řádky, které začínají `java.lang.Exception`.

3. Uložit **Program.cs**a následně vytvořit projekt hello.

## <a name="upload-toostorage"></a>Nahrát toostorage

1. V sadě Visual Studio otevřete **Průzkumníka serveru**.

2. Rozbalte položku **Azure** a pak rozbalte **HDInsight**.

3. Po zobrazení výzvy zadejte své přihlašovací údaje předplatného Azure a pak klikněte na tlačítko **přihlásit**.

4. Rozbalte cluster HDInsight hello, který chcete toodeploy této aplikace. Položku s textem hello __(výchozí účet úložiště)__ je uveden.

    ![Průzkumník serveru zobrazující hello účet úložiště pro hello cluster](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

    * Pokud tuto položku lze rozšířit, kterou používáte __účet úložiště Azure__ jako výchozí úložiště pro hello cluster. soubory hello tooview na hello výchozí úložiště pro hello cluster, rozbalte položku hello a potom dvakrát klikněte na hello __(výchozí kontejner)__.

    * Pokud tuto položku nelze rozšířit, používáte __Azure Data Lake Store__ jako hello výchozí úložiště pro hello cluster. soubory hello tooview na hello výchozí úložiště pro hello cluster, dvakrát klikněte na hello __(výchozí účet úložiště)__ položku.

6. soubory .exe tooupload hello, použijte jednu z následujících metod hello:

    * Pokud se používá __účet úložiště Azure__, klikněte na ikonu hello nahrávání a vyhledejte toohello **bin\debug** složku pro hello **HiveCSharp** projektu. Nakonec vyberte hello **HiveCSharp.exe** souboru a klikněte na tlačítko **Ok**.

        ![Nahrajte ikonu](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/upload.png)
    
    * Pokud používáte __Azure Data Lake Store__, klikněte pravým tlačítkem myši na prázdnou oblast v seznamu hello souboru a pak vyberte __nahrát__. Nakonec vyberte hello **HiveCSharp.exe** souboru a klikněte na tlačítko **otevřete**.

    Jednou hello __HiveCSharp.exe__ nahrávání dokončí, proces odesílání opakování hello hello __PigUDF.exe__ souboru.

## <a name="run-a-hive-query"></a>Spuštění dotazu Hive

1. V sadě Visual Studio otevřete **Průzkumníka serveru**.

2. Rozbalte položku **Azure** a pak rozbalte **HDInsight**.

3. Klikněte pravým tlačítkem na hello clusteru, že jste nasadili hello **HiveCSharp** do aplikace a pak vyberte **napsat dotaz Hive**.

4. Použijte následující text pro dotaz Hive hello hello:

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
    > Zrušením komentáře u hello `add file` příkaz, který odpovídá typu hello výchozí úložiště použitého pro váš cluster.

    Tento dotaz vybere hello `clientid`, `devicemake`, a `devicemodel` pole z `hivesampletable`a předá hello pole toohello HiveCSharp.exe aplikace. dotaz Hello očekává tři pole tooreturn se aplikace hello, které jsou uloženy jako `clientid`, `phoneLabel`, a `phoneHash`. dotaz Hello také očekává toofind HiveCSharp.exe hello kořenové hello výchozí kontejner úložiště.

5. Klikněte na tlačítko **odeslání** toosubmit hello úlohy toohello HDInsight clusteru. Hello **Souhrn úlohy Hive** otevře se okno.

6. Klikněte na tlačítko **aktualizovat** toorefresh hello souhrnné dokud **stav úlohy** změní příliš**dokončeno**. výstup úlohy hello tooview, klikněte na tlačítko **výstup úlohy**.

## <a name="run-a-pig-job"></a>Spustit úlohu Pig

1. Použijte jednu z následujících clusteru HDInsight tooyour tooconnect metody hello:

    * Pokud používáte __systémem Linux__ HDInsight clusteru, použijte SSH. Například, `ssh sshuser@mycluster-ssh.azurehdinsight.net`. Další informace najdete v tématu [withHDInsight použití SSH](hdinsight-hadoop-linux-use-ssh-unix.md)
    
    * Pokud používáte __založené na Windows__ clusteru HDInsight, [clusteru toohello připojit pomocí vzdálené plochy](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)

2. Použijte jeden hello následující příkaz toostart hello Pig příkazového řádku:

        pig

    > [!IMPORTANT]
    > Pokud používáte cluster systému Windows, použijte hello místo následující příkazy:
    > ```
    > cd %PIG_HOME%
    > bin\pig
    > ```

    A `grunt>` se zobrazí výzva.

3. Zadejte následující toorun Pig úlohu, která používá aplikace rozhraní .NET Framework hello hello:

        DEFINE streamer `PigUDF.exe` CACHE('/PigUDF.exe');
        LOGS = LOAD '/example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = STREAM LOG through streamer as (col1, col2, col3, col4, col5);
        DUMP DETAILS;

    Hello `DEFINE` příkaz vytvoří zástupce `streamer` pro hello pigudf.exe aplikace, a `CACHE` načte z výchozí úložiště pro hello cluster. Později `streamer` se používá s hello `STREAM` operátor tooprocess hello jednotlivých řádků obsažená v protokolu a návratové hello data jako řadu sloupců.

    > [!NOTE]
    > název aplikace Hello, který se používá pro streamování, musí být uzavřena do hello \` (backtick) znak při alias, a "(v jednoduchých uvozovkách) při použití s `SHIP`.

4. Po zadání hello poslední řádek, začněte hello úlohy. Vrátí výstup podobný toohello následující text:

        (2012-02-03 20:11:56 SampleClass5 [WARN] problem finding id 1358451042 - java.lang.Exception)
        (2012-02-03 20:11:56 SampleClass5 [DEBUG] detail for id 1976092771)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1317358561)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1737534798)
        (2012-02-03 20:11:56 SampleClass7 [DEBUG] detail for id 1475865947)

## <a name="next-steps"></a>Další kroky

V tomto dokumentu jste se naučili jak toouse aplikace rozhraní .NET Framework z Hive a Pig v HDInsight. Pokud chcete toolearn jak zjistit, toouse Python s Hive a Pig, [Python použití Hive a Pig v HDInsight](hdinsight-python.md).

Další způsoby toouse Pig a Hive a toolearn o použití prostředí MapReduce najdete v části hello následující dokumenty:

* [Použití Hivu se službou HDInsight](hdinsight-use-hive.md)
* [Použití Pigu se službou HDInsight](hdinsight-use-pig.md)
* [Používání nástroje MapReduce s HDInsight](hdinsight-use-mapreduce.md)
