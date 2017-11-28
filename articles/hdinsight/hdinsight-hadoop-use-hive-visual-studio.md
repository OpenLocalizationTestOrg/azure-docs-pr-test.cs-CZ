---
title: "aaaHive pomocí nástrojů Data Lake (Hadoop) pro Visual Studio – Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak nástroje toouse hello Data Lake pro Visual Studio toorun Apache Hive dotazy s Apache Hadoop v Azure HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2b3e672a-1195-4fa5-afb7-b7b73937bfbe
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/07/2017
ms.author: larryfr
ms.openlocfilehash: dc76974c02cf68bcf701b2b155842c9e9c5cb988
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-using-hello-data-lake-tools-for-visual-studio"></a><span data-ttu-id="dfb07-103">Spouštění dotazů Hive pomocí hello nástrojů Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dfb07-103">Run Hive queries using hello Data Lake tools for Visual Studio</span></span>

<span data-ttu-id="dfb07-104">Zjistěte, jak nástroje toouse hello Data Lake pro Visual Studio tooquery Apache Hive.</span><span class="sxs-lookup"><span data-stu-id="dfb07-104">Learn how toouse hello Data Lake tools for Visual Studio tooquery Apache Hive.</span></span> <span data-ttu-id="dfb07-105">nástroje Data Lake Hello umožňují tooeasily vytvoření, odeslání a sledování tooHadoop dotazy Hive v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="dfb07-105">hello Data Lake tools allow you tooeasily create, submit, and monitor Hive queries tooHadoop on Azure HDInsight.</span></span>

## <span data-ttu-id="dfb07-106"><a id="prereq"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="dfb07-106"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="dfb07-107">Cluster Azure HDInsight (Hadoop v HDInsight)</span><span class="sxs-lookup"><span data-stu-id="dfb07-107">An Azure HDInsight (Hadoop on HDInsight) cluster</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="dfb07-108">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="dfb07-108">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="dfb07-109">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="dfb07-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="dfb07-110">Visual Studio (jeden hello následující verze):</span><span class="sxs-lookup"><span data-stu-id="dfb07-110">Visual Studio (one of hello following versions):</span></span>

    * <span data-ttu-id="dfb07-111">Visual Studio 2013 Community/Professional/Premium/Ultimate s aktualizací 4</span><span class="sxs-lookup"><span data-stu-id="dfb07-111">Visual Studio 2013 Community/Professional/Premium/Ultimate with Update 4</span></span>

    * <span data-ttu-id="dfb07-112">Visual Studio 2015 (všechny edice)</span><span class="sxs-lookup"><span data-stu-id="dfb07-112">Visual Studio 2015 (any edition)</span></span>

    * <span data-ttu-id="dfb07-113">Visual Studio 2017 (všechny edice)</span><span class="sxs-lookup"><span data-stu-id="dfb07-113">Visual Studio 2017 (any edition)</span></span>

* <span data-ttu-id="dfb07-114">Nástroje HDInsight pro Visual Studio nebo nástroje Azure Data Lake pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dfb07-114">HDInsight tools for Visual Studio or Azure Data Lake tools for Visual Studio.</span></span> <span data-ttu-id="dfb07-115">V tématu [začněte používat nástroje Visual Studio Hadoop pro HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md) informace o instalaci a konfiguraci nástroje hello.</span><span class="sxs-lookup"><span data-stu-id="dfb07-115">See [Get started using Visual Studio Hadoop tools for HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md) for information on installing and configuring hello tools.</span></span>

## <span data-ttu-id="dfb07-116"><a id="run"></a>Spouštění dotazů Hive pomocí hello Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dfb07-116"><a id="run"></a> Run Hive queries using hello Visual Studio</span></span>

1. <span data-ttu-id="dfb07-117">Otevřete **Visual Studio** a vyberte **nový** > **projektu** > **Azure Data Lake**  >   **HIVE** > **podregistru aplikace**.</span><span class="sxs-lookup"><span data-stu-id="dfb07-117">Open **Visual Studio** and select **New** > **Project** > **Azure Data Lake** > **HIVE** > **Hive Application**.</span></span> <span data-ttu-id="dfb07-118">Zadejte název pro tento projekt.</span><span class="sxs-lookup"><span data-stu-id="dfb07-118">Provide a name for this project.</span></span>

2. <span data-ttu-id="dfb07-119">Otevřete hello **Script.hql** souboru, který je vytvořen s tímto projektu a vložte následující příkazy HiveQL hello:</span><span class="sxs-lookup"><span data-stu-id="dfb07-119">Open hello **Script.hql** file that is created with this project, and paste in hello following HiveQL statements:</span></span>

   ```hiveql
   set hive.execution.engine=tez;
   DROP TABLE log4jLogs;
   CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
   ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
   STORED AS TEXTFILE LOCATION '/example/data/';
   SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND  INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
   ```

    <span data-ttu-id="dfb07-120">Tyto příkazy provádět hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="dfb07-120">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="dfb07-121">`DROP TABLE`: Pokud hello tabulka existuje, tento příkaz se odstraní.</span><span class="sxs-lookup"><span data-stu-id="dfb07-121">`DROP TABLE`: If hello table exists, this statement deletes it.</span></span>

   * <span data-ttu-id="dfb07-122">`CREATE EXTERNAL TABLE`: Vytvoří novou tabulku "externí" v Hive.</span><span class="sxs-lookup"><span data-stu-id="dfb07-122">`CREATE EXTERNAL TABLE`: Creates a new 'external' table in Hive.</span></span> <span data-ttu-id="dfb07-123">Externí tabulky pouze uložit definici tabulky hello Hive (hello dat je ponecháno v původním umístění hello).</span><span class="sxs-lookup"><span data-stu-id="dfb07-123">External tables only store hello table definition in Hive (hello data is left in hello original location).</span></span>

     > [!NOTE]
     > <span data-ttu-id="dfb07-124">Externí tabulky by měl použít, pokud očekáváte, že hello základní data toobe aktualizovat externího zdroje.</span><span class="sxs-lookup"><span data-stu-id="dfb07-124">External tables should be used when you expect hello underlying data toobe updated by an external source.</span></span> <span data-ttu-id="dfb07-125">Například MapReduce úlohy nebo služby Azure.</span><span class="sxs-lookup"><span data-stu-id="dfb07-125">For example, a MapReduce job or Azure service.</span></span>
     >
     > <span data-ttu-id="dfb07-126">Vyřazení externí tabulku nemá **není** odstranit hello data, jenom hello definici tabulky.</span><span class="sxs-lookup"><span data-stu-id="dfb07-126">Dropping an external table does **not** delete hello data, only hello table definition.</span></span>

   * <span data-ttu-id="dfb07-127">`ROW FORMAT`: Určuje způsob formátování hello data Hive.</span><span class="sxs-lookup"><span data-stu-id="dfb07-127">`ROW FORMAT`: Tells Hive how hello data is formatted.</span></span> <span data-ttu-id="dfb07-128">V takovém případě hello pole v každém protokolu jsou oddělené mezerou.</span><span class="sxs-lookup"><span data-stu-id="dfb07-128">In this case, hello fields in each log are separated by a space.</span></span>

   * <span data-ttu-id="dfb07-129">`STORED AS TEXTFILE LOCATION`: Informuje Hive kde hello data se ukládají (adresář hello příklad nebo data) a která je uložena jako text.</span><span class="sxs-lookup"><span data-stu-id="dfb07-129">`STORED AS TEXTFILE LOCATION`: Tells Hive where hello data is stored (hello example/data directory) and that it is stored as text.</span></span>

   * <span data-ttu-id="dfb07-130">`SELECT`: Vyberte počet všech řádků kde sloupec `t4` obsahuje hodnotu hello `[ERROR]`.</span><span class="sxs-lookup"><span data-stu-id="dfb07-130">`SELECT`: Select a count of all rows where column `t4` contains hello value `[ERROR]`.</span></span> <span data-ttu-id="dfb07-131">Tento příkaz vrátí hodnotu `3` vzhledem k tomu, že existují tři řádky, které obsahují tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dfb07-131">This statement returns a value of `3` because there are three rows that contain this value.</span></span>

   * <span data-ttu-id="dfb07-132">`INPUT__FILE__NAME LIKE '%.log'`-Informuje Hive, jsme by měl vrátit pouze data ze souborů končící na. log.</span><span class="sxs-lookup"><span data-stu-id="dfb07-132">`INPUT__FILE__NAME LIKE '%.log'` - Tells Hive that we should only return data from files ending in .log.</span></span> <span data-ttu-id="dfb07-133">Tuto klauzuli omezuje hello vyhledávání toohello sample.log soubor, který obsahuje hello data.</span><span class="sxs-lookup"><span data-stu-id="dfb07-133">This clause restricts hello search toohello sample.log file that contains hello data.</span></span>

3. <span data-ttu-id="dfb07-134">Hello nástrojů vyberte hello **clusteru HDInsight** chcete toouse pro tento dotaz.</span><span class="sxs-lookup"><span data-stu-id="dfb07-134">From hello toolbar, select hello **HDInsight Cluster** that you want toouse for this query.</span></span> <span data-ttu-id="dfb07-135">Vyberte **odeslání** toorun hello příkazy jako úlohy Hive.</span><span class="sxs-lookup"><span data-stu-id="dfb07-135">Select **Submit** toorun hello statements as a Hive job.</span></span>

   ![Odeslání panelu](./media/hdinsight-hadoop-use-hive-visual-studio/toolbar.png)

4. <span data-ttu-id="dfb07-137">Hello **Souhrn úlohy Hive** se zobrazí a zobrazí informace o hello spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="dfb07-137">hello **Hive Job Summary** appears and displays information about hello running job.</span></span> <span data-ttu-id="dfb07-138">Použití hello **aktualizovat** odkaz toorefresh hello informace o úlohách, dokud hello **stav úlohy** změní příliš**dokončeno**.</span><span class="sxs-lookup"><span data-stu-id="dfb07-138">Use hello **Refresh** link toorefresh hello job information, until hello **Job Status** changes too**Completed**.</span></span>

   ![Souhrn úlohy zobrazení dokončené úlohy](./media/hdinsight-hadoop-use-hive-visual-studio/jobsummary.png)

5. <span data-ttu-id="dfb07-140">Použití hello **výstup úlohy** odkaz výstup hello tooview této úlohy.</span><span class="sxs-lookup"><span data-stu-id="dfb07-140">Use hello **Job Output** link tooview hello output of this job.</span></span> <span data-ttu-id="dfb07-141">Zobrazuje `[ERROR] 3`, což je hello hodnoty vrácené tímto dotazem.</span><span class="sxs-lookup"><span data-stu-id="dfb07-141">It displays `[ERROR] 3`, which is hello value returned by this query.</span></span>

6. <span data-ttu-id="dfb07-142">Můžete také spustit dotazů Hive bez vytvoření projektu.</span><span class="sxs-lookup"><span data-stu-id="dfb07-142">You can also run Hive queries without creating a project.</span></span> <span data-ttu-id="dfb07-143">Pomocí **Průzkumníka serveru**, rozbalte položku **Azure** > **HDInsight**, klikněte pravým tlačítkem na serveru HDInsight a pak vyberte **napsat dotaz Hive** .</span><span class="sxs-lookup"><span data-stu-id="dfb07-143">Using **Server Explorer**, expand **Azure** > **HDInsight**, right-click your HDInsight server, and then select **Write a Hive Query**.</span></span>

7. <span data-ttu-id="dfb07-144">V hello **temp.hql** dokument, který se zobrazí, přidejte následující příkazy HiveQL hello:</span><span class="sxs-lookup"><span data-stu-id="dfb07-144">In hello **temp.hql** document that appears, add hello following HiveQL statements:</span></span>

   ```hiveql
   set hive.execution.engine=tez;
   CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
   INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
   ```

    <span data-ttu-id="dfb07-145">Tyto příkazy provádět hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="dfb07-145">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="dfb07-146">`CREATE TABLE IF NOT EXISTS`: Vytvoří tabulku, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="dfb07-146">`CREATE TABLE IF NOT EXISTS`: Creates a table if it does not already exist.</span></span> <span data-ttu-id="dfb07-147">Protože hello `EXTERNAL` – klíčové slovo není, tento příkaz vytvoří interní tabulku.</span><span class="sxs-lookup"><span data-stu-id="dfb07-147">Because hello `EXTERNAL` keyword is not used, this statement creates an internal table.</span></span> <span data-ttu-id="dfb07-148">Interní tabulky jsou uložené v hello Hive datového skladu a jsou spravovány nástrojem Hive.</span><span class="sxs-lookup"><span data-stu-id="dfb07-148">Internal tables are stored in hello Hive data warehouse and are managed by Hive.</span></span>

     > [!NOTE]
     > <span data-ttu-id="dfb07-149">Na rozdíl od `EXTERNAL` tabulky, vyřazení interní tabulku taky odstraní hello základní data.</span><span class="sxs-lookup"><span data-stu-id="dfb07-149">Unlike `EXTERNAL` tables, dropping an internal table also deletes hello underlying data.</span></span>

   * <span data-ttu-id="dfb07-150">`STORED AS ORC`: Hello úložiště dat ve sloupcovém formátu (ORC) optimalizované řádek.</span><span class="sxs-lookup"><span data-stu-id="dfb07-150">`STORED AS ORC`: Stores hello data in optimized row columnar (ORC) format.</span></span> <span data-ttu-id="dfb07-151">ORC je vysoce optimalizovaný a efektivní formát pro ukládání dat Hive.</span><span class="sxs-lookup"><span data-stu-id="dfb07-151">ORC is a highly optimized and efficient format for storing Hive data.</span></span>

   * <span data-ttu-id="dfb07-152">`INSERT OVERWRITE ... SELECT`: Vybere řádky z hello `log4jLogs` tabulku, která obsahují `[ERROR]`, pak vloží hello data do hello `errorLogs` tabulky.</span><span class="sxs-lookup"><span data-stu-id="dfb07-152">`INSERT OVERWRITE ... SELECT`: Selects rows from hello `log4jLogs` table that contain `[ERROR]`, then inserts hello data into hello `errorLogs` table.</span></span>

8. <span data-ttu-id="dfb07-153">Hello nástrojů vyberte **odeslání** toorun hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="dfb07-153">From hello toolbar, select **Submit** toorun hello job.</span></span> <span data-ttu-id="dfb07-154">Použití hello **stav úlohy** toodetermine tuto úlohu hello byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="dfb07-154">Use hello **Job Status** toodetermine that hello job has completed successfully.</span></span>

9. <span data-ttu-id="dfb07-155">tooverify, který hello úlohy vytvoření hello tabulky, použijte **Průzkumníka serveru** a rozbalte **Azure** > **HDInsight** > clusteru HDInsight > **Databáze hive** > **výchozí**.</span><span class="sxs-lookup"><span data-stu-id="dfb07-155">tooverify that hello job created hello table, use **Server Explorer** and expand **Azure** > **HDInsight** > your HDInsight cluster > **Hive Databases** > **default**.</span></span> <span data-ttu-id="dfb07-156">Hello **errorLogs** tabulky a hello **log4jLogs** tabulce jsou uvedeny.</span><span class="sxs-lookup"><span data-stu-id="dfb07-156">hello **errorLogs** table and hello **log4jLogs** table are listed.</span></span>

## <span data-ttu-id="dfb07-157"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="dfb07-157"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="dfb07-158">Jak vidíte, zadejte hello nástroje HDInsight pro Visual Studio snadno toowork s dotazy Hive v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="dfb07-158">As you can see, hello HDInsight tools for Visual Studio provide an easy way toowork with Hive queries on HDInsight.</span></span>

<span data-ttu-id="dfb07-159">Obecné informace o Hive v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="dfb07-159">For general information about Hive in HDInsight:</span></span>

* [<span data-ttu-id="dfb07-160">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="dfb07-160">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="dfb07-161">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="dfb07-161">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="dfb07-162">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="dfb07-162">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

* [<span data-ttu-id="dfb07-163">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="dfb07-163">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="dfb07-164">Další informace o hello nástroje HDInsight pro Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="dfb07-164">For more information about hello HDInsight tools for Visual Studio:</span></span>

* [<span data-ttu-id="dfb07-165">Začínáme s nástroje HDInsight pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dfb07-165">Getting started with HDInsight tools for Visual Studio</span></span>](hdinsight-hadoop-visual-studio-tools-get-started.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png
