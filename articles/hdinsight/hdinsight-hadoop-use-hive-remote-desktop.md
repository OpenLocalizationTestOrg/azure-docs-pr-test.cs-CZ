---
title: "Použijte Hadoop Hive a Vzdálená plocha v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak se připojit ke clusteru Hadoop v HDInsight pomocí vzdálené plochy a pak spouštění dotazů Hive pomocí rozhraní příkazového řádku Hive."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 8c228e35-d58a-4f22-917a-1d20c9da89b4
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 187c7cb413b3707e58eea387857375053d267189
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a><span data-ttu-id="15c8e-103">Použijte Hive s Hadoop v HDInsight pomocí vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="15c8e-103">Use Hive with Hadoop on HDInsight with Remote Desktop</span></span>
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="15c8e-104">V tomto článku zjistěte, jak se připojit ke clusteru HDInsight pomocí vzdálené plochy a pak spouštění dotazů Hive pomocí Hive rozhraní příkazového řádku (CLI).</span><span class="sxs-lookup"><span data-stu-id="15c8e-104">In this article, you will learn how to connect to an HDInsight cluster by using Remote Desktop, and then run Hive queries by using the Hive Command-Line Interface (CLI).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="15c8e-105">Vzdálená plocha je dostupná pouze na clustery HDInsight, které používají Windows jako operační systém.</span><span class="sxs-lookup"><span data-stu-id="15c8e-105">Remote Desktop is only available on HDInsight clusters that use Windows as the operating system.</span></span> <span data-ttu-id="15c8e-106">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="15c8e-106">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="15c8e-107">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="15c8e-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="15c8e-108">HDInsight 3.4 nebo větší, projděte si téma [používání Hive s HDInsight a Beeline](hdinsight-hadoop-use-hive-beeline.md) informace o spouštění dotazů Hive přímo na clusteru z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="15c8e-108">For HDInsight 3.4 or greater, see [Use Hive with HDInsight and Beeline](hdinsight-hadoop-use-hive-beeline.md) for information on running Hive queries directly on the cluster from a command-line.</span></span>

## <span data-ttu-id="15c8e-109"><a id="prereq"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="15c8e-109"><a id="prereq"></a>Prerequisites</span></span>
<span data-ttu-id="15c8e-110">Pokud chcete provést kroky v tomto článku, budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="15c8e-110">To complete the steps in this article, you will need the following:</span></span>

* <span data-ttu-id="15c8e-111">Cluster HDInsight se systémem Windows (Hadoop v HDInsight)</span><span class="sxs-lookup"><span data-stu-id="15c8e-111">A Windows-based HDInsight (Hadoop on HDInsight) cluster</span></span>
* <span data-ttu-id="15c8e-112">Klientský počítač se systémem Windows 10, Windows 8 nebo Windows 7</span><span class="sxs-lookup"><span data-stu-id="15c8e-112">A client computer running Windows 10, Window 8, or Windows 7</span></span>

## <span data-ttu-id="15c8e-113"><a id="connect"></a>Připojit pomocí vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="15c8e-113"><a id="connect"></a>Connect with Remote Desktop</span></span>
<span data-ttu-id="15c8e-114">Povolení vzdálené plochy pro HDInsight cluster a pak připojit pomocí pokynů uvedených v [připojení ke clusterům HDInsight pomocí protokolu RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="15c8e-114">Enable Remote Desktop for the HDInsight cluster, then connect to it by following the instructions at [Connect to HDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>

## <span data-ttu-id="15c8e-115"><a id="hive"></a>Použijte příkaz Hive</span><span class="sxs-lookup"><span data-stu-id="15c8e-115"><a id="hive"></a>Use the Hive command</span></span>
<span data-ttu-id="15c8e-116">Když připojíte k ploše pro HDInsight cluster, použijte následující postup pro práci s Hive:</span><span class="sxs-lookup"><span data-stu-id="15c8e-116">When you have connected to the desktop for the HDInsight cluster, use the following steps to work with Hive:</span></span>

1. <span data-ttu-id="15c8e-117">Z plochy HDInsight, spusťte **Hadoop příkazového řádku**.</span><span class="sxs-lookup"><span data-stu-id="15c8e-117">From the HDInsight desktop, start the **Hadoop Command Line**.</span></span>
2. <span data-ttu-id="15c8e-118">Zadejte následující příkaz, který spouští rozhraní příkazového řádku Hive:</span><span class="sxs-lookup"><span data-stu-id="15c8e-118">Enter the following command to start the Hive CLI:</span></span>

        %hive_home%\bin\hive

    <span data-ttu-id="15c8e-119">Pokud bylo zahájeno rozhraní příkazového řádku, zobrazí se řádku Hive rozhraní příkazového řádku: `hive>`.</span><span class="sxs-lookup"><span data-stu-id="15c8e-119">When the CLI has started, you will see the Hive CLI prompt: `hive>`.</span></span>
3. <span data-ttu-id="15c8e-120">Pomocí rozhraní příkazového řádku, zadejte následující příkazy, čímž umožňuje vytvořit novou tabulku s názvem **log4jLogs** pomocí ukázkových dat:</span><span class="sxs-lookup"><span data-stu-id="15c8e-120">Using the CLI, enter the following statements to create a new table named **log4jLogs** using sample data:</span></span>

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    <span data-ttu-id="15c8e-121">Tyto příkazy provádět následující akce:</span><span class="sxs-lookup"><span data-stu-id="15c8e-121">These statements perform the following actions:</span></span>

   * <span data-ttu-id="15c8e-122">**VYŘAĎTE tabulku**: Odstraní tabulku a datový soubor, pokud tabulka již existuje.</span><span class="sxs-lookup"><span data-stu-id="15c8e-122">**DROP TABLE**: Deletes the table and the data file if the table already exists.</span></span>
   * <span data-ttu-id="15c8e-123">**Vytvoření externí tabulky**: vytvoří novou tabulku "externí" v Hive.</span><span class="sxs-lookup"><span data-stu-id="15c8e-123">**CREATE EXTERNAL TABLE**: Creates a new 'external' table in Hive.</span></span> <span data-ttu-id="15c8e-124">Externí tabulky uložit pouze definici tabulky Hive (data je ponechán v původním umístění).</span><span class="sxs-lookup"><span data-stu-id="15c8e-124">External tables store only the table definition in Hive (the data is left in the original location).</span></span>

     > [!NOTE]
     > <span data-ttu-id="15c8e-125">Externí tabulky by měl být použit při očekáváte, že v základních datech aktualizovat externího zdroje (například procesu nahrávání automatizované dat), nebo jiná operace MapReduce, ale chcete, aby dotazy Hive používat nejnovější data.</span><span class="sxs-lookup"><span data-stu-id="15c8e-125">External tables should be used when you expect the underlying data to be updated by an external source (such as an automated data upload process) or by another MapReduce operation, but you always want Hive queries to use the latest data.</span></span>
     >
     > <span data-ttu-id="15c8e-126">Vyřazení externí tabulku nemá **není** odstranit data, jenom definici tabulky.</span><span class="sxs-lookup"><span data-stu-id="15c8e-126">Dropping an external table does **not** delete the data, only the table definition.</span></span>
     >
     >
   * <span data-ttu-id="15c8e-127">**Řádek formátu**: informuje Hive formátování data.</span><span class="sxs-lookup"><span data-stu-id="15c8e-127">**ROW FORMAT**: Tells Hive how the data is formatted.</span></span> <span data-ttu-id="15c8e-128">V takovém případě polí v každém protokolu jsou oddělené mezerou.</span><span class="sxs-lookup"><span data-stu-id="15c8e-128">In this case, the fields in each log are separated by a space.</span></span>
   * <span data-ttu-id="15c8e-129">**ULOŽENÉ umístění textový soubor AS**: informuje Hive, kde je data uložená (adresář, příklad nebo data) a která je uložena jako text.</span><span class="sxs-lookup"><span data-stu-id="15c8e-129">**STORED AS TEXTFILE LOCATION**: Tells Hive where the data is stored (the example/data directory) and that it is stored as text.</span></span>
   * <span data-ttu-id="15c8e-130">**Vyberte**: počet všech řádků vybere kde sloupec **t4** obsahuje hodnotu **[Chyba]**.</span><span class="sxs-lookup"><span data-stu-id="15c8e-130">**SELECT**: Selects a count of all rows where column **t4** contains the value **[ERROR]**.</span></span> <span data-ttu-id="15c8e-131">To by měla vrátit hodnotu **3** vzhledem k tomu, že existují tři řádky, které obsahují tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="15c8e-131">This should return a value of **3** because there are three rows that contain this value.</span></span>
   * <span data-ttu-id="15c8e-132">**INPUT__FILE__NAME jako '%.log'** -informuje Hive, který jsme by měl vrátit pouze data ze souborů končící na. log.</span><span class="sxs-lookup"><span data-stu-id="15c8e-132">**INPUT__FILE__NAME LIKE '%.log'** - Tells Hive that we should only return data from files ending in .log.</span></span> <span data-ttu-id="15c8e-133">To brání hledání k sample.log souboru, který obsahuje data a udržuje ho z vrací data z jiných příkladu datové soubory, které neodpovídají schématu, které jsme definovali.</span><span class="sxs-lookup"><span data-stu-id="15c8e-133">This restricts the search to the sample.log file that contains the data, and keeps it from returning data from other example data files that do not match the schema we defined.</span></span>
4. <span data-ttu-id="15c8e-134">Použít následující příkazy k vytvoření nové "interní" tabulku s názvem **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="15c8e-134">Use the following statements to create a new 'internal' table named **errorLogs**:</span></span>

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    <span data-ttu-id="15c8e-135">Tyto příkazy provádět následující akce:</span><span class="sxs-lookup"><span data-stu-id="15c8e-135">These statements perform the following actions:</span></span>

   * <span data-ttu-id="15c8e-136">**Vytvoření tabulka není v případě existuje**: vytvoří tabulku, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="15c8e-136">**CREATE TABLE IF NOT EXISTS**: Creates a table if it does not already exist.</span></span> <span data-ttu-id="15c8e-137">Protože **externí** – klíčové slovo nepoužívá, jde o interní tabulku, která je uložena v datovém skladu Hive a je zcela spravuje Hive.</span><span class="sxs-lookup"><span data-stu-id="15c8e-137">Because the **EXTERNAL** keyword is not used, this is an internal table, which is stored in the Hive data warehouse and is managed completely by Hive.</span></span>

     > [!NOTE]
     > <span data-ttu-id="15c8e-138">Na rozdíl od **externí** tabulky, vyřazení interní tabulku taky odstraní zadaná data.</span><span class="sxs-lookup"><span data-stu-id="15c8e-138">Unlike **EXTERNAL** tables, dropping an internal table also deletes the underlying data.</span></span>
     >
     >
   * <span data-ttu-id="15c8e-139">**ULOŽENÉ ORC AS**: ukládá data ve sloupcovém formátu (ORC) optimalizované řádek.</span><span class="sxs-lookup"><span data-stu-id="15c8e-139">**STORED AS ORC**: Stores the data in optimized row columnar (ORC) format.</span></span> <span data-ttu-id="15c8e-140">Toto je vysoce optimalizovaný a efektivní formát pro ukládání dat Hive.</span><span class="sxs-lookup"><span data-stu-id="15c8e-140">This is a highly optimized and efficient format for storing Hive data.</span></span>
   * <span data-ttu-id="15c8e-141">**PŘEPSAT INSERT... Vyberte**: vybere řádky z **log4jLogs** tabulku, která obsahují **[Chyba]**, pak vkládá data do **errorLogs** tabulky.</span><span class="sxs-lookup"><span data-stu-id="15c8e-141">**INSERT OVERWRITE ... SELECT**: Selects rows from the **log4jLogs** table that contain **[ERROR]**, then inserts the data into the **errorLogs** table.</span></span>

     <span data-ttu-id="15c8e-142">Chcete-li ověřit, že pouze řádky, které obsahují **[Chyba]** ve sloupci t4 byly uloženy do **errorLogs** tabulky, použijte následující příkaz a vrátí všechny řádky z **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="15c8e-142">To verify that only rows that contain **[ERROR]** in column t4 were stored to the **errorLogs** table, use the following statement to return all the rows from **errorLogs**:</span></span>

       <span data-ttu-id="15c8e-143">Vybrat * z errorLogs;</span><span class="sxs-lookup"><span data-stu-id="15c8e-143">SELECT * from errorLogs;</span></span>

     <span data-ttu-id="15c8e-144">Tří řádků dat má být vrácen, všechny obsahující **[Chyba]** v t4 sloupce.</span><span class="sxs-lookup"><span data-stu-id="15c8e-144">Three rows of data should be returned, all containing **[ERROR]** in column t4.</span></span>

## <span data-ttu-id="15c8e-145"><a id="summary"></a>Shrnutí</span><span class="sxs-lookup"><span data-stu-id="15c8e-145"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="15c8e-146">Jak je vidět příkaz Hive poskytuje snadný způsob, jak interaktivně spouštění dotazů Hive v clusteru HDInsight, monitorovat stav úlohy a načíst výstup.</span><span class="sxs-lookup"><span data-stu-id="15c8e-146">As you can see, the the Hive command provides an easy way to interactively run Hive queries on an HDInsight cluster, monitor the job status, and retrieve the output.</span></span>

## <span data-ttu-id="15c8e-147"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="15c8e-147"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="15c8e-148">Obecné informace o Hive v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="15c8e-148">For general information about Hive in HDInsight:</span></span>

* [<span data-ttu-id="15c8e-149">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="15c8e-149">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="15c8e-150">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="15c8e-150">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="15c8e-151">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="15c8e-151">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="15c8e-152">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="15c8e-152">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="15c8e-153">Pokud používáte s Hive Tez, najdete v následujících dokumentech pro ladění informace:</span><span class="sxs-lookup"><span data-stu-id="15c8e-153">If you are using Tez with Hive, see the following documents for debugging information:</span></span>

* [<span data-ttu-id="15c8e-154">Pomocí uživatelského rozhraní Tez na HDInsight se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="15c8e-154">Use the Tez UI on Windows-based HDInsight</span></span>](hdinsight-debug-tez-ui.md)
* [<span data-ttu-id="15c8e-155">Použití zobrazení Ambari Tez na HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="15c8e-155">Use the Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

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





[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx
