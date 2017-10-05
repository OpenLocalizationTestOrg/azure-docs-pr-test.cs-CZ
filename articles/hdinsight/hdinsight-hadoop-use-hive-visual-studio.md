---
title: "Hive pomocí nástrojů Data Lake (Hadoop) pro Visual Studio – Azure HDInsight | Microsoft Docs"
description: "Další informace o použití nástroje Data Lake pro Visual Studio ke spouštění dotazů Apache Hive s Apache Hadoop v Azure HDInsight."
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
ms.openlocfilehash: 3411c59fee73aa2e26a05d70e1dae11cdfc865ff
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="run-hive-queries-using-the-data-lake-tools-for-visual-studio"></a><span data-ttu-id="cd7d2-103">Spouštění dotazů Hive pomocí nástroje Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd7d2-103">Run Hive queries using the Data Lake tools for Visual Studio</span></span>

<span data-ttu-id="cd7d2-104">Zjistěte, jak pomocí nástrojů Data Lake pro Visual Studio pro Apache Hive dotaz.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-104">Learn how to use the Data Lake tools for Visual Studio to query Apache Hive.</span></span> <span data-ttu-id="cd7d2-105">Nástroje Data Lake umožňují snadno vytvářet, odesílání a sledování dotazů Hive k systému Hadoop v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-105">The Data Lake tools allow you to easily create, submit, and monitor Hive queries to Hadoop on Azure HDInsight.</span></span>

## <span data-ttu-id="cd7d2-106"><a id="prereq"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="cd7d2-106"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="cd7d2-107">Cluster Azure HDInsight (Hadoop v HDInsight)</span><span class="sxs-lookup"><span data-stu-id="cd7d2-107">An Azure HDInsight (Hadoop on HDInsight) cluster</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="cd7d2-108">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-108">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="cd7d2-109">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="cd7d2-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="cd7d2-110">Visual Studio (jedna z následujících verzí):</span><span class="sxs-lookup"><span data-stu-id="cd7d2-110">Visual Studio (one of the following versions):</span></span>

    * <span data-ttu-id="cd7d2-111">Visual Studio 2013 Community/Professional/Premium/Ultimate s aktualizací 4</span><span class="sxs-lookup"><span data-stu-id="cd7d2-111">Visual Studio 2013 Community/Professional/Premium/Ultimate with Update 4</span></span>

    * <span data-ttu-id="cd7d2-112">Visual Studio 2015 (všechny edice)</span><span class="sxs-lookup"><span data-stu-id="cd7d2-112">Visual Studio 2015 (any edition)</span></span>

    * <span data-ttu-id="cd7d2-113">Visual Studio 2017 (všechny edice)</span><span class="sxs-lookup"><span data-stu-id="cd7d2-113">Visual Studio 2017 (any edition)</span></span>

* <span data-ttu-id="cd7d2-114">Nástroje HDInsight pro Visual Studio nebo nástroje Azure Data Lake pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-114">HDInsight tools for Visual Studio or Azure Data Lake tools for Visual Studio.</span></span> <span data-ttu-id="cd7d2-115">V tématu [začněte používat nástroje Visual Studio Hadoop pro HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md) informace o instalaci a konfiguraci nástroje.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-115">See [Get started using Visual Studio Hadoop tools for HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md) for information on installing and configuring the tools.</span></span>

## <span data-ttu-id="cd7d2-116"><a id="run"></a>Spouštění dotazů Hive pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd7d2-116"><a id="run"></a> Run Hive queries using the Visual Studio</span></span>

1. <span data-ttu-id="cd7d2-117">Otevřete **Visual Studio** a vyberte **nový** > **projektu** > **Azure Data Lake**  >   **HIVE** > **podregistru aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-117">Open **Visual Studio** and select **New** > **Project** > **Azure Data Lake** > **HIVE** > **Hive Application**.</span></span> <span data-ttu-id="cd7d2-118">Zadejte název pro tento projekt.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-118">Provide a name for this project.</span></span>

2. <span data-ttu-id="cd7d2-119">Otevřete **Script.hql** souboru, který je vytvořen s tímto projektu a vložte následující příkazy HiveQL:</span><span class="sxs-lookup"><span data-stu-id="cd7d2-119">Open the **Script.hql** file that is created with this project, and paste in the following HiveQL statements:</span></span>

   ```hiveql
   set hive.execution.engine=tez;
   DROP TABLE log4jLogs;
   CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
   ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
   STORED AS TEXTFILE LOCATION '/example/data/';
   SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND  INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
   ```

    <span data-ttu-id="cd7d2-120">Tyto příkazy provádět následující akce:</span><span class="sxs-lookup"><span data-stu-id="cd7d2-120">These statements perform the following actions:</span></span>

   * <span data-ttu-id="cd7d2-121">`DROP TABLE`: Pokud existuje v tabulce, tento příkaz se odstraní.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-121">`DROP TABLE`: If the table exists, this statement deletes it.</span></span>

   * <span data-ttu-id="cd7d2-122">`CREATE EXTERNAL TABLE`: Vytvoří novou tabulku "externí" v Hive.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-122">`CREATE EXTERNAL TABLE`: Creates a new 'external' table in Hive.</span></span> <span data-ttu-id="cd7d2-123">Externí tabulky pouze uložit definici tabulky Hive (data je ponechán v původním umístění).</span><span class="sxs-lookup"><span data-stu-id="cd7d2-123">External tables only store the table definition in Hive (the data is left in the original location).</span></span>

     > [!NOTE]
     > <span data-ttu-id="cd7d2-124">Externí tabulky by měl být použit při očekáváte, že v základních datech aktualizovat externího zdroje.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-124">External tables should be used when you expect the underlying data to be updated by an external source.</span></span> <span data-ttu-id="cd7d2-125">Například MapReduce úlohy nebo služby Azure.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-125">For example, a MapReduce job or Azure service.</span></span>
     >
     > <span data-ttu-id="cd7d2-126">Vyřazení externí tabulku nemá **není** odstranit data, jenom definici tabulky.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-126">Dropping an external table does **not** delete the data, only the table definition.</span></span>

   * <span data-ttu-id="cd7d2-127">`ROW FORMAT`: Určuje způsob formátování data Hive.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-127">`ROW FORMAT`: Tells Hive how the data is formatted.</span></span> <span data-ttu-id="cd7d2-128">V takovém případě polí v každém protokolu jsou oddělené mezerou.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-128">In this case, the fields in each log are separated by a space.</span></span>

   * <span data-ttu-id="cd7d2-129">`STORED AS TEXTFILE LOCATION`: Informuje Hive se uloží data (například/datový adresář) a která je uložena jako text.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-129">`STORED AS TEXTFILE LOCATION`: Tells Hive where the data is stored (the example/data directory) and that it is stored as text.</span></span>

   * <span data-ttu-id="cd7d2-130">`SELECT`: Vyberte počet všech řádků kde sloupec `t4` obsahuje hodnotu `[ERROR]`.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-130">`SELECT`: Select a count of all rows where column `t4` contains the value `[ERROR]`.</span></span> <span data-ttu-id="cd7d2-131">Tento příkaz vrátí hodnotu `3` vzhledem k tomu, že existují tři řádky, které obsahují tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-131">This statement returns a value of `3` because there are three rows that contain this value.</span></span>

   * <span data-ttu-id="cd7d2-132">`INPUT__FILE__NAME LIKE '%.log'`-Informuje Hive, jsme by měl vrátit pouze data ze souborů končící na. log.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-132">`INPUT__FILE__NAME LIKE '%.log'` - Tells Hive that we should only return data from files ending in .log.</span></span> <span data-ttu-id="cd7d2-133">Tuto klauzuli omezuje hledání sample.log soubor, který obsahuje data.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-133">This clause restricts the search to the sample.log file that contains the data.</span></span>

3. <span data-ttu-id="cd7d2-134">Na panelu nástrojů vyberte **clusteru HDInsight** , kterou chcete použít pro tento dotaz.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-134">From the toolbar, select the **HDInsight Cluster** that you want to use for this query.</span></span> <span data-ttu-id="cd7d2-135">Vyberte **odeslání** spustit příkazy jako úlohy Hive.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-135">Select **Submit** to run the statements as a Hive job.</span></span>

   ![Odeslání panelu](./media/hdinsight-hadoop-use-hive-visual-studio/toolbar.png)

4. <span data-ttu-id="cd7d2-137">**Souhrn úlohy Hive** se zobrazí a zobrazí informace o probíhající úloze.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-137">The **Hive Job Summary** appears and displays information about the running job.</span></span> <span data-ttu-id="cd7d2-138">Použití **aktualizovat** odkaz aktualizovat informace o úloze, dokud **stav úlohy** změny **dokončeno**.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-138">Use the **Refresh** link to refresh the job information, until the **Job Status** changes to **Completed**.</span></span>

   ![Souhrn úlohy zobrazení dokončené úlohy](./media/hdinsight-hadoop-use-hive-visual-studio/jobsummary.png)

5. <span data-ttu-id="cd7d2-140">Použití **výstup úlohy** odkaz zobrazte výstup této úlohy.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-140">Use the **Job Output** link to view the output of this job.</span></span> <span data-ttu-id="cd7d2-141">Zobrazuje `[ERROR] 3`, což je hodnoty vrácené tímto dotazem.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-141">It displays `[ERROR] 3`, which is the value returned by this query.</span></span>

6. <span data-ttu-id="cd7d2-142">Můžete také spustit dotazů Hive bez vytvoření projektu.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-142">You can also run Hive queries without creating a project.</span></span> <span data-ttu-id="cd7d2-143">Pomocí **Průzkumníka serveru**, rozbalte položku **Azure** > **HDInsight**, klikněte pravým tlačítkem na serveru HDInsight a pak vyberte **napsat dotaz Hive** .</span><span class="sxs-lookup"><span data-stu-id="cd7d2-143">Using **Server Explorer**, expand **Azure** > **HDInsight**, right-click your HDInsight server, and then select **Write a Hive Query**.</span></span>

7. <span data-ttu-id="cd7d2-144">V **temp.hql** dokument, který se zobrazí, přidejte následující příkazy HiveQL:</span><span class="sxs-lookup"><span data-stu-id="cd7d2-144">In the **temp.hql** document that appears, add the following HiveQL statements:</span></span>

   ```hiveql
   set hive.execution.engine=tez;
   CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
   INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
   ```

    <span data-ttu-id="cd7d2-145">Tyto příkazy provádět následující akce:</span><span class="sxs-lookup"><span data-stu-id="cd7d2-145">These statements perform the following actions:</span></span>

   * <span data-ttu-id="cd7d2-146">`CREATE TABLE IF NOT EXISTS`: Vytvoří tabulku, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-146">`CREATE TABLE IF NOT EXISTS`: Creates a table if it does not already exist.</span></span> <span data-ttu-id="cd7d2-147">Protože `EXTERNAL` – klíčové slovo není, tento příkaz vytvoří interní tabulku.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-147">Because the `EXTERNAL` keyword is not used, this statement creates an internal table.</span></span> <span data-ttu-id="cd7d2-148">Interní tabulky jsou uložena v datovém skladu Hive a jsou spravovány nástrojem Hive.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-148">Internal tables are stored in the Hive data warehouse and are managed by Hive.</span></span>

     > [!NOTE]
     > <span data-ttu-id="cd7d2-149">Na rozdíl od `EXTERNAL` tabulky, vyřazení interní tabulku taky odstraní zadaná data.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-149">Unlike `EXTERNAL` tables, dropping an internal table also deletes the underlying data.</span></span>

   * <span data-ttu-id="cd7d2-150">`STORED AS ORC`: Ukládá data ve sloupcovém formátu (ORC) optimalizované řádek.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-150">`STORED AS ORC`: Stores the data in optimized row columnar (ORC) format.</span></span> <span data-ttu-id="cd7d2-151">ORC je vysoce optimalizovaný a efektivní formát pro ukládání dat Hive.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-151">ORC is a highly optimized and efficient format for storing Hive data.</span></span>

   * <span data-ttu-id="cd7d2-152">`INSERT OVERWRITE ... SELECT`: Vybere řádky z `log4jLogs` tabulku, která obsahují `[ERROR]`, pak vkládá data do `errorLogs` tabulky.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-152">`INSERT OVERWRITE ... SELECT`: Selects rows from the `log4jLogs` table that contain `[ERROR]`, then inserts the data into the `errorLogs` table.</span></span>

8. <span data-ttu-id="cd7d2-153">Na panelu nástrojů vyberte **odeslání** chcete spustit úlohu.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-153">From the toolbar, select **Submit** to run the job.</span></span> <span data-ttu-id="cd7d2-154">Použití **stav úlohy** k určení, zda byla úloha úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-154">Use the **Job Status** to determine that the job has completed successfully.</span></span>

9. <span data-ttu-id="cd7d2-155">Chcete-li ověřit, zda úloha vytvořena v tabulce, použijte **Průzkumníka serveru** a rozbalte **Azure** > **HDInsight** > clusteru HDInsight >  **Hive databáze** > **výchozí**.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-155">To verify that the job created the table, use **Server Explorer** and expand **Azure** > **HDInsight** > your HDInsight cluster > **Hive Databases** > **default**.</span></span> <span data-ttu-id="cd7d2-156">**ErrorLogs** tabulky a **log4jLogs** tabulce jsou uvedeny.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-156">The **errorLogs** table and the **log4jLogs** table are listed.</span></span>

## <span data-ttu-id="cd7d2-157"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="cd7d2-157"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="cd7d2-158">Jak vidíte, nástroje HDInsight pro Visual Studio poskytují snadný způsob, jak pracovat s dotazy Hive v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="cd7d2-158">As you can see, the HDInsight tools for Visual Studio provide an easy way to work with Hive queries on HDInsight.</span></span>

<span data-ttu-id="cd7d2-159">Obecné informace o Hive v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="cd7d2-159">For general information about Hive in HDInsight:</span></span>

* [<span data-ttu-id="cd7d2-160">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="cd7d2-160">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="cd7d2-161">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="cd7d2-161">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="cd7d2-162">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="cd7d2-162">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

* [<span data-ttu-id="cd7d2-163">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="cd7d2-163">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="cd7d2-164">Další informace o nástrojích HDInsight pro Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="cd7d2-164">For more information about the HDInsight tools for Visual Studio:</span></span>

* [<span data-ttu-id="cd7d2-165">Začínáme s nástroje HDInsight pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd7d2-165">Getting started with HDInsight tools for Visual Studio</span></span>](hdinsight-hadoop-visual-studio-tools-get-started.md)

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
