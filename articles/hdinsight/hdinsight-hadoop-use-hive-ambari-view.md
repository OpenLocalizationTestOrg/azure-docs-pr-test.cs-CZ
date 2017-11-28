---
title: "Zobrazení Ambari toowork aaaUse s Hive v HDInsight (Hadoop) - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello zobrazení Hive z vaší webové prohlížeče toosubmit Hive dotazy. Hello zobrazení Hive je součástí hello poskytnuté webové uživatelské rozhraní Ambari k vašemu clusteru HDInsight se systémem Linux."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1abe9104-f4b2-41b9-9161-abbc43de8294
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: f9a77b652e70d34a0ff9165fbb8c2e16d3401ae0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-hive-view-with-hadoop-in-hdinsight"></a><span data-ttu-id="c34fc-104">Použít hello zobrazení Hive se systémem Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="c34fc-104">Use hello Hive View with Hadoop in HDInsight</span></span>

[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="c34fc-105">Zjistěte, jak toorun Hive dotazuje pomocí zobrazení Ambari Hive.</span><span class="sxs-lookup"><span data-stu-id="c34fc-105">Learn how toorun Hive queries using Ambari Hive View.</span></span> <span data-ttu-id="c34fc-106">Ambari je správy a monitorování nástroj s clustery HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="c34fc-106">Ambari is a management and monitoring utility provided with Linux-based HDInsight clusters.</span></span> <span data-ttu-id="c34fc-107">Jedna z funkcí hello poskytované prostřednictvím Ambari je webového uživatelského rozhraní, které můžou být použité toorun dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="c34fc-107">One of hello features provided through Ambari is a Web UI that can be used toorun Hive queries.</span></span>

> [!NOTE]
> <span data-ttu-id="c34fc-108">Ambari má mnoho možností, které nejsou popsané v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="c34fc-108">Ambari has many capabilities that are not discussed in this document.</span></span> <span data-ttu-id="c34fc-109">Další informace najdete v tématu [Správa clusterů HDInsight pomocí hello webové uživatelské rozhraní Ambari](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="c34fc-109">For more information, see [Manage HDInsight clusters by using hello Ambari Web UI](hdinsight-hadoop-manage-ambari.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c34fc-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c34fc-110">Prerequisites</span></span>

* <span data-ttu-id="c34fc-111">Cluster HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="c34fc-111">A Linux-based HDInsight cluster.</span></span> <span data-ttu-id="c34fc-112">Informace týkající se vytvoření clusteru najdete v tématu [Začínáme s HDInsight se systémem Linux](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c34fc-112">For information on creating cluster, see [Get started with Linux-based HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c34fc-113">Hello kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux.</span><span class="sxs-lookup"><span data-stu-id="c34fc-113">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="c34fc-114">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="c34fc-114">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="c34fc-115">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="c34fc-115">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="open-hello-hive-view"></a><span data-ttu-id="c34fc-116">Otevření zobrazení Hive hello</span><span class="sxs-lookup"><span data-stu-id="c34fc-116">Open hello Hive view</span></span>

<span data-ttu-id="c34fc-117">Zobrazení Ambari můžete z portálu Azure; hello Vyberte HDInsight cluster a pak vyberte **zobrazení Ambari** z hello **rychlé odkazy** části.</span><span class="sxs-lookup"><span data-stu-id="c34fc-117">You can Ambari Views from hello Azure portal; select your HDInsight cluster and then select **Ambari Views** from hello **Quick Links** section.</span></span>

![Rychlé odkazy části portálu hello](./media/hdinsight-hadoop-use-hive-ambari-view/quicklinks.png)

<span data-ttu-id="c34fc-119">Ze seznamu hello zobrazení, vyberte hello __zobrazení Hive__.</span><span class="sxs-lookup"><span data-stu-id="c34fc-119">From hello list of views, select hello __Hive View__.</span></span>

![Hello vybrané zobrazení Hive](./media/hdinsight-hadoop-use-hive-ambari-view/select-hive-view.png)

> [!NOTE]
> <span data-ttu-id="c34fc-121">Při přístupu k Ambari, jste výzvami tooauthenticate toohello lokality.</span><span class="sxs-lookup"><span data-stu-id="c34fc-121">When accessing Ambari, you are prompted tooauthenticate toohello site.</span></span> <span data-ttu-id="c34fc-122">Zadejte Dobrý den, správce (výchozí `admin`) účet jméno a heslo použité při vytváření hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="c34fc-122">Enter hello admin (default `admin`) account name and password you used when creating hello cluster.</span></span>

<span data-ttu-id="c34fc-123">Měli byste vidět stránky podobné toohello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="c34fc-123">You should see a page similar toohello following image:</span></span>

![Obrázek hello listu dotazu pro zobrazení Hive hello](./media/hdinsight-hadoop-use-hive-ambari-view/ambari-hive-view.png)

## <span data-ttu-id="c34fc-125"><a name="hivequery"></a>Spuštění dotazu</span><span class="sxs-lookup"><span data-stu-id="c34fc-125"><a name="hivequery"></a>Run a query</span></span>

<span data-ttu-id="c34fc-126">toorun hive dotaz, použít následující kroky ze zobrazení Hive hello hello.</span><span class="sxs-lookup"><span data-stu-id="c34fc-126">toorun a hive query, use hello following steps from hello Hive view.</span></span>

1. <span data-ttu-id="c34fc-127">Z hello __dotazu__ kartě, vložte následující příkazy HiveQL do listu hello hello:</span><span class="sxs-lookup"><span data-stu-id="c34fc-127">From hello __Query__ tab, paste hello following HiveQL statements into hello worksheet:</span></span>

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS cnt FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;
    ```

    <span data-ttu-id="c34fc-128">Tyto příkazy provádět hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="c34fc-128">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="c34fc-129">`DROP TABLE`-Odstraní se hello tabulky a hello datový soubor, v případě, že hello tabulka již existuje.</span><span class="sxs-lookup"><span data-stu-id="c34fc-129">`DROP TABLE` - Deletes hello table and hello data file, in case hello table already exists.</span></span>

   * <span data-ttu-id="c34fc-130">`CREATE EXTERNAL TABLE`-Vytvoří novou tabulku "externí" v Hive.</span><span class="sxs-lookup"><span data-stu-id="c34fc-130">`CREATE EXTERNAL TABLE` - Creates a new "external" table in Hive.</span></span>
   <span data-ttu-id="c34fc-131">Externí tabulky uložit jenom hello Definice tabulky Hive.</span><span class="sxs-lookup"><span data-stu-id="c34fc-131">External tables store only hello table definition in Hive.</span></span> <span data-ttu-id="c34fc-132">Hello data je ponechán v hello původního umístění.</span><span class="sxs-lookup"><span data-stu-id="c34fc-132">hello data is left in hello original location.</span></span>

   * <span data-ttu-id="c34fc-133">`ROW FORMAT`-Hello data formátování.</span><span class="sxs-lookup"><span data-stu-id="c34fc-133">`ROW FORMAT` - How hello data is formatted.</span></span> <span data-ttu-id="c34fc-134">V takovém případě hello pole v každém protokolu jsou oddělené mezerou.</span><span class="sxs-lookup"><span data-stu-id="c34fc-134">In this case, hello fields in each log are separated by a space.</span></span>

   * <span data-ttu-id="c34fc-135">`STORED AS TEXTFILE LOCATION`-Hello je ukládat data a že je uloženo jako text.</span><span class="sxs-lookup"><span data-stu-id="c34fc-135">`STORED AS TEXTFILE LOCATION` - Where hello data is stored, and that it is stored as text.</span></span>

   * <span data-ttu-id="c34fc-136">`SELECT`-Vybere počet všech řádků, kde t4 sloupec obsahuje hodnotu hello [Chyba].</span><span class="sxs-lookup"><span data-stu-id="c34fc-136">`SELECT` - Selects a count of all rows where column t4 contains hello value [ERROR].</span></span>

     > [!NOTE]
     > <span data-ttu-id="c34fc-137">Externí tabulky by měl použít, pokud očekáváte, že hello základní data toobe aktualizovat externího zdroje.</span><span class="sxs-lookup"><span data-stu-id="c34fc-137">External tables should be used when you expect hello underlying data toobe updated by an external source.</span></span> <span data-ttu-id="c34fc-138">Například automatické nahrání dat je proces, nebo jiná operace MapReduce.</span><span class="sxs-lookup"><span data-stu-id="c34fc-138">For example, an automated data upload process, or by another MapReduce operation.</span></span> <span data-ttu-id="c34fc-139">Vyřazení externí tabulku nemá *není* odstranit hello data, jenom hello definici tabulky.</span><span class="sxs-lookup"><span data-stu-id="c34fc-139">Dropping an external table does *not* delete hello data, only hello table definition.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="c34fc-140">Nechte hello __databáze__ výběr v __výchozí__.</span><span class="sxs-lookup"><span data-stu-id="c34fc-140">Leave hello __Database__ selection at __default__.</span></span> <span data-ttu-id="c34fc-141">Hello příklady v tomto dokumentu používají hello výchozí databázi, která je zahrnutá v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c34fc-141">hello examples in this document use hello default database included with HDInsight.</span></span>

2. <span data-ttu-id="c34fc-142">toostart hello dotaz, použít hello **Execute** tlačítko pod hello listu.</span><span class="sxs-lookup"><span data-stu-id="c34fc-142">toostart hello query, use hello **Execute** button below hello worksheet.</span></span> <span data-ttu-id="c34fc-143">Změní oranžová a hello text se změní příliš**Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="c34fc-143">It turns orange and hello text changes too**Stop**.</span></span>

3. <span data-ttu-id="c34fc-144">Po dokončení dotazu hello hello **výsledky** karta zobrazuje výsledky hello hello operace.</span><span class="sxs-lookup"><span data-stu-id="c34fc-144">Once hello query has finished, hello **Results** tab displays hello results of hello operation.</span></span> <span data-ttu-id="c34fc-145">Následující text Hello je hello výsledek dotazu hello:</span><span class="sxs-lookup"><span data-stu-id="c34fc-145">hello following text is hello result of hello query:</span></span>

        sev       cnt
        [ERROR]   3

    <span data-ttu-id="c34fc-146">Hello **protokoly** karta může být použité tooview hello protokolování informací o vytvořených hello úlohou.</span><span class="sxs-lookup"><span data-stu-id="c34fc-146">hello **Logs** tab can be used tooview hello logging information created by hello job.</span></span>

   > [!TIP]
   > <span data-ttu-id="c34fc-147">Hello **výsledky uložit** dialogové okno rozevíracího seznamu ve hello horní pravé hello **výsledky zpracování dotazu** části vám umožní toodownload nebo uložení výsledků.</span><span class="sxs-lookup"><span data-stu-id="c34fc-147">hello **Save results** drop-down dialog in hello upper left of hello **Query Process Results** section allows you toodownload or save results.</span></span>

4. <span data-ttu-id="c34fc-148">Vyberte hello první čtyři řádky tohoto dotazu, pak vyberte **Execute**.</span><span class="sxs-lookup"><span data-stu-id="c34fc-148">Select hello first four lines of this query, then select **Execute**.</span></span> <span data-ttu-id="c34fc-149">Všimněte si, že po dokončení úlohy hello neexistují žádné výsledky.</span><span class="sxs-lookup"><span data-stu-id="c34fc-149">Notice that there are no results when hello job completes.</span></span> <span data-ttu-id="c34fc-150">Pomocí hello **Execute** tlačítko, pokud je součástí hello dotazu je vybrána pouze spustí hello vybrané příkazy.</span><span class="sxs-lookup"><span data-stu-id="c34fc-150">Using hello **Execute** button when part of hello query is selected only runs hello selected statements.</span></span> <span data-ttu-id="c34fc-151">V takovém případě hello výběr nezahrnuli hello poslední příkaz, který načte řádky z tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="c34fc-151">In this case, hello selection didn't include hello final statement that retrieves rows from hello table.</span></span> <span data-ttu-id="c34fc-152">Pokud vyberte právě daného řádku a pomocí **Execute**, měli byste vidět hello očekávané výsledky.</span><span class="sxs-lookup"><span data-stu-id="c34fc-152">If you select just that line and use **Execute**, you should see hello expected results.</span></span>

5. <span data-ttu-id="c34fc-153">tooadd listu, použijte hello **nový list** tlačítko dole hello hello **Editor dotazů**.</span><span class="sxs-lookup"><span data-stu-id="c34fc-153">tooadd a worksheet, use hello **New Worksheet** button at hello bottom of hello **Query Editor**.</span></span> <span data-ttu-id="c34fc-154">V novém listu hello zadejte následující příkazy HiveQL hello:</span><span class="sxs-lookup"><span data-stu-id="c34fc-154">In hello new worksheet, enter hello following HiveQL statements:</span></span>

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';
    ```

  <span data-ttu-id="c34fc-155">Tyto příkazy provádět hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="c34fc-155">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="c34fc-156">**Vytvoření tabulka není v případě existuje** -vytvoří tabulku, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="c34fc-156">**CREATE TABLE IF NOT EXISTS** - Creates a table, if it does not already exist.</span></span> <span data-ttu-id="c34fc-157">Od hello **externí** – klíčové slovo nepoužívá, k vytvoření interní tabulky.</span><span class="sxs-lookup"><span data-stu-id="c34fc-157">Since hello **EXTERNAL** keyword is not used, an internal table is created.</span></span> <span data-ttu-id="c34fc-158">Interní tabulku je uložena v datovém skladu Hive hello a je zcela spravuje Hive.</span><span class="sxs-lookup"><span data-stu-id="c34fc-158">An internal table is stored in hello Hive data warehouse and is managed completely by Hive.</span></span> <span data-ttu-id="c34fc-159">Na rozdíl od externích tabulek se odstranit interní tabulku odstraní hello základní data.</span><span class="sxs-lookup"><span data-stu-id="c34fc-159">Unlike external tables, dropping an internal table deletes hello underlying data as well.</span></span>

   * <span data-ttu-id="c34fc-160">**ULOŽENÉ ORC AS** – ukládá hello data ve formátu optimalizované řádek sloupcovém (ORC).</span><span class="sxs-lookup"><span data-stu-id="c34fc-160">**STORED AS ORC** - Stores hello data in Optimized Row Columnar (ORC) format.</span></span> <span data-ttu-id="c34fc-161">ORC je vysoce optimalizovaný a efektivní formát pro ukládání dat Hive.</span><span class="sxs-lookup"><span data-stu-id="c34fc-161">ORC is a highly optimized and efficient format for storing Hive data.</span></span>

   * <span data-ttu-id="c34fc-162">**PŘEPSAT INSERT... Vyberte** -vybere řádky z hello **log4jLogs** tabulku, která obsahují `[ERROR]`, a poté vloží hello data do hello **errorLogs** tabulky.</span><span class="sxs-lookup"><span data-stu-id="c34fc-162">**INSERT OVERWRITE ... SELECT** - Selects rows from hello **log4jLogs** table that contain `[ERROR]`, and then inserts hello data into hello **errorLogs** table.</span></span>

     <span data-ttu-id="c34fc-163">Použití hello **Execute** tlačítko toorun tento dotaz.</span><span class="sxs-lookup"><span data-stu-id="c34fc-163">Use hello **Execute** button toorun this query.</span></span> <span data-ttu-id="c34fc-164">Hello **výsledky** karta neobsahuje žádné informace, pokud hello dotaz vrátí nulový počet řádků.</span><span class="sxs-lookup"><span data-stu-id="c34fc-164">hello **Results** tab does not contain any information when hello query returns zero rows.</span></span> <span data-ttu-id="c34fc-165">Stav Hello by měl zobrazit jako **úspěšné** po dokončení dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="c34fc-165">hello status should show as **SUCCEEDED** once hello query completes.</span></span>

### <a name="visual-explain"></a><span data-ttu-id="c34fc-166">Vysvětlují Visual</span><span class="sxs-lookup"><span data-stu-id="c34fc-166">Visual explain</span></span>

<span data-ttu-id="c34fc-167">toodisplay vizualizaci hello plán dotazu, vyberte hello **Visual vysvětlují** kartě níže hello listu.</span><span class="sxs-lookup"><span data-stu-id="c34fc-167">toodisplay a visualization of hello query plan, select hello **Visual Explain** tab below hello worksheet.</span></span>

<span data-ttu-id="c34fc-168">Hello **Visual vysvětlují** hello dotazu může být užitečné pro porozumění hello toku složitých dotazů.</span><span class="sxs-lookup"><span data-stu-id="c34fc-168">hello **Visual Explain** view of hello query can be helpful in understanding hello flow of complex queries.</span></span> <span data-ttu-id="c34fc-169">Textové ekvivalent v tomto zobrazení můžete zobrazit pomocí hello **vysvětlit** tlačítka na hello editoru dotazů.</span><span class="sxs-lookup"><span data-stu-id="c34fc-169">You can view a textual equivalent of this view by using hello **Explain** button in hello Query Editor.</span></span>

### <a name="tez-ui"></a><span data-ttu-id="c34fc-170">Tez uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="c34fc-170">Tez UI</span></span>

<span data-ttu-id="c34fc-171">toodisplay hello Tez uživatelského rozhraní pro hello dotaz, vyberte hello **Tez** kartě níže hello listu.</span><span class="sxs-lookup"><span data-stu-id="c34fc-171">toodisplay hello Tez UI for hello query, select hello **Tez** tab below hello worksheet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c34fc-172">Tez je nepoužívá tooresolve všechny dotazy.</span><span class="sxs-lookup"><span data-stu-id="c34fc-172">Tez is not used tooresolve all queries.</span></span> <span data-ttu-id="c34fc-173">Mnoho dotazů, bez použití Tez lze vyřešit.</span><span class="sxs-lookup"><span data-stu-id="c34fc-173">Many queries can be resolved without using Tez.</span></span> 

<span data-ttu-id="c34fc-174">Pokud Tez použité tooresolve hello dotazu, zobrazí se hello řízené Acyklické grafu (DAG).</span><span class="sxs-lookup"><span data-stu-id="c34fc-174">If Tez was used tooresolve hello query, hello Directed Acyclic Graph (DAG) is displayed.</span></span> <span data-ttu-id="c34fc-175">Pokud chcete tooview hello DAG pro dotazy, které jste spustili v posledních hello nebo ladění hello Tez procesu, použijte hello [Tez zobrazení](hdinsight-debug-ambari-tez-view.md) místo.</span><span class="sxs-lookup"><span data-stu-id="c34fc-175">If you want tooview hello DAG for queries you've ran in hello past, or debug hello Tez process, use hello [Tez View](hdinsight-debug-ambari-tez-view.md) instead.</span></span>

## <a name="view-job-history"></a><span data-ttu-id="c34fc-176">Zobrazení historie úlohy</span><span class="sxs-lookup"><span data-stu-id="c34fc-176">View job history</span></span>

<span data-ttu-id="c34fc-177">Hello __úlohy__ karta se zobrazuje historie dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="c34fc-177">hello __Jobs__ tab displays a history of Hive queries.</span></span>

![Obrázek hello historie úlohy](./media/hdinsight-hadoop-use-hive-ambari-view/job-history.png)

## <a name="database-tables"></a><span data-ttu-id="c34fc-179">Databázové tabulky</span><span class="sxs-lookup"><span data-stu-id="c34fc-179">Database tables</span></span>

<span data-ttu-id="c34fc-180">Můžete použít hello __tabulky__ kartě toowork u tabulek v rámci databáze Hive.</span><span class="sxs-lookup"><span data-stu-id="c34fc-180">You can use hello __Tables__ tab toowork with tables within a Hive database.</span></span>

![Obrázek kartu tabulky hello](./media/hdinsight-hadoop-use-hive-ambari-view/tables.png)

## <a name="saved-queries"></a><span data-ttu-id="c34fc-182">Uložené dotazy</span><span class="sxs-lookup"><span data-stu-id="c34fc-182">Saved queries</span></span>

<span data-ttu-id="c34fc-183">Z karty hello dotazu můžete volitelně uložit dotazy.</span><span class="sxs-lookup"><span data-stu-id="c34fc-183">From hello Query tab, you can optionally save queries.</span></span> <span data-ttu-id="c34fc-184">Po uložení, můžete opakovaně použít dotaz hello z hello __uložené dotazy__ kartě.</span><span class="sxs-lookup"><span data-stu-id="c34fc-184">Once saved, you can reuse hello query from hello __Saved Queries__ tab.</span></span>

![Obrázek karty uložené dotazy](./media/hdinsight-hadoop-use-hive-ambari-view/saved-queries.png)

## <a name="user-defined-functions"></a><span data-ttu-id="c34fc-186">Uživatelem definované funkce</span><span class="sxs-lookup"><span data-stu-id="c34fc-186">User-defined functions</span></span>

<span data-ttu-id="c34fc-187">Hive lze také rozšířit pomocí uživatelem definovaných funkcí (UDF).</span><span class="sxs-lookup"><span data-stu-id="c34fc-187">Hive can also be extended through user-defined functions (UDF).</span></span> <span data-ttu-id="c34fc-188">Uživatelem definovanou FUNKCI umožňuje tooimplement funkce nebo logiky, která není snadno modelován v HiveQL.</span><span class="sxs-lookup"><span data-stu-id="c34fc-188">A UDF allows you tooimplement functionality or logic that isn't easily modeled in HiveQL.</span></span>

<span data-ttu-id="c34fc-189">Hello UDF karta hello horní části hello zobrazení Hive můžete toodeclare a uložte sadu UDF.</span><span class="sxs-lookup"><span data-stu-id="c34fc-189">hello UDF tab at hello top of hello Hive View allows you toodeclare and save a set of UDFs.</span></span> <span data-ttu-id="c34fc-190">Tyto funkce lze použít s hello **Editor dotazů**.</span><span class="sxs-lookup"><span data-stu-id="c34fc-190">These UDFs can be used with hello **Query Editor**.</span></span>

![Obrázek karty UDF](./media/hdinsight-hadoop-use-hive-ambari-view/user-defined-functions.png)

<span data-ttu-id="c34fc-192">Po přidání UDF toohello zobrazení Hive **vložit UDF** dole hello hello se zobrazí tlačítko **Editor dotazů**.</span><span class="sxs-lookup"><span data-stu-id="c34fc-192">Once you have added a UDF toohello Hive View, an **Insert udfs** button appears at hello bottom of hello **Query Editor**.</span></span> <span data-ttu-id="c34fc-193">Výběrem této položky se zobrazí rozevírací seznam hello UDF definované v hello zobrazení Hive.</span><span class="sxs-lookup"><span data-stu-id="c34fc-193">Selecting this entry displays a drop-down list of hello UDFs defined in hello Hive View.</span></span> <span data-ttu-id="c34fc-194">Výběr UDF přidá HiveQL příkazy tooyour dotazu tooenable hello UDF.</span><span class="sxs-lookup"><span data-stu-id="c34fc-194">Selecting a UDF adds HiveQL statements tooyour query tooenable hello UDF.</span></span>

<span data-ttu-id="c34fc-195">Například, pokud jste definovali uživatelem definovanou FUNKCI hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="c34fc-195">For example, if you have defined a UDF with hello following properties:</span></span>

* <span data-ttu-id="c34fc-196">Název prostředku: myudfs</span><span class="sxs-lookup"><span data-stu-id="c34fc-196">Resource name: myudfs</span></span>

* <span data-ttu-id="c34fc-197">Cesta prostředku: /myudfs.jar</span><span class="sxs-lookup"><span data-stu-id="c34fc-197">Resource path: /myudfs.jar</span></span>

* <span data-ttu-id="c34fc-198">Název UDF: myawesomeudf</span><span class="sxs-lookup"><span data-stu-id="c34fc-198">UDF name: myawesomeudf</span></span>

* <span data-ttu-id="c34fc-199">Název třídy UDF: com.myudfs.Awesome</span><span class="sxs-lookup"><span data-stu-id="c34fc-199">UDF class name: com.myudfs.Awesome</span></span>

<span data-ttu-id="c34fc-200">Pomocí hello **vložit UDF** tlačítko zobrazí položka s názvem **myudfs**, s jinou rozevíracího seznamu pro každý UDF definované pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="c34fc-200">Using hello **Insert udfs** button displays an entry named **myudfs**, with another drop-down for each UDF defined for that resource.</span></span> <span data-ttu-id="c34fc-201">V takovém případě **myawesomeudf**.</span><span class="sxs-lookup"><span data-stu-id="c34fc-201">In this case, **myawesomeudf**.</span></span> <span data-ttu-id="c34fc-202">Výběrem této položky přidáte hello následující toohello začátku hello dotazu:</span><span class="sxs-lookup"><span data-stu-id="c34fc-202">Selecting this entry adds hello following toohello beginning of hello query:</span></span>

```hiveql
add jar /myudfs.jar;
create temporary function myawesomeudf as 'com.myudfs.Awesome';
```

<span data-ttu-id="c34fc-203">Potom můžete hello UDF v dotazu.</span><span class="sxs-lookup"><span data-stu-id="c34fc-203">You can then use hello UDF in your query.</span></span> <span data-ttu-id="c34fc-204">Například, `SELECT myawesomeudf(name) FROM people;`.</span><span class="sxs-lookup"><span data-stu-id="c34fc-204">For example, `SELECT myawesomeudf(name) FROM people;`.</span></span>

<span data-ttu-id="c34fc-205">Další informace o používání funkce UDF s Hive v HDInsight naleznete v tématu hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="c34fc-205">For more information on using UDFs with Hive on HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="c34fc-206">Python pomocí Hive a Pig v HDInsight</span><span class="sxs-lookup"><span data-stu-id="c34fc-206">Using Python with Hive and Pig in HDInsight</span></span>](hdinsight-python.md)
* [<span data-ttu-id="c34fc-207">Jak tooadd vlastní tooHDInsight Hive UDF</span><span class="sxs-lookup"><span data-stu-id="c34fc-207">How tooadd a custom Hive UDF tooHDInsight</span></span>](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

## <a name="hive-settings"></a><span data-ttu-id="c34fc-208">Nastavení Hive</span><span class="sxs-lookup"><span data-stu-id="c34fc-208">Hive settings</span></span>

<span data-ttu-id="c34fc-209">Nastavení může být použité toochange různá nastavení Hive.</span><span class="sxs-lookup"><span data-stu-id="c34fc-209">Settings can be used toochange various Hive settings.</span></span> <span data-ttu-id="c34fc-210">Například změna hello spouštěcímu modulu pro Hive z tooMapReduce Tez (hello výchozí nastavení).</span><span class="sxs-lookup"><span data-stu-id="c34fc-210">For example, changing hello execution engine for Hive from Tez (hello default) tooMapReduce.</span></span>

## <span data-ttu-id="c34fc-211"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="c34fc-211"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="c34fc-212">Obecné informace o Hive v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="c34fc-212">For general information on Hive in HDInsight:</span></span>

* [<span data-ttu-id="c34fc-213">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="c34fc-213">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="c34fc-214">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="c34fc-214">For information on other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="c34fc-215">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="c34fc-215">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="c34fc-216">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="c34fc-216">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
