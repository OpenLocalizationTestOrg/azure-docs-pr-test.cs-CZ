---
title: "Korelovat události v čase pomocí nástrojů Storm a HBase v HDInsight"
description: "Zjistěte, jak ke korelaci událostí, které přicházejí v různou dobu pomocí nástrojů Storm a HBase v HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 17d11479-2d02-4790-8d29-05fb38351479
ms.service: hdinsight
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/07/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 06630096383601e48e8f69f8553314cee42f5f3e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="correlate-events-that-arrive-at-different-times-using-storm-and-hbase"></a><span data-ttu-id="69ab1-103">Korelovat události, které přicházejí v různou dobu pomocí nástrojů Storm a HBase</span><span class="sxs-lookup"><span data-stu-id="69ab1-103">Correlate events that arrive at different times using Storm and HBase</span></span>

<span data-ttu-id="69ab1-104">Pomocí úložiště trvalé dat s Apache Storm mohou korelovat datové položky, které přicházejí v různých časech.</span><span class="sxs-lookup"><span data-stu-id="69ab1-104">By using a persistent data store with Apache Storm, you can correlate data entries that arrive at different times.</span></span> <span data-ttu-id="69ab1-105">Například propojení události přihlášení a odhlášení pro uživatelské relace vypočítat, jak dlouho už bylo relace.</span><span class="sxs-lookup"><span data-stu-id="69ab1-105">For example, linking login and logout events for a user session to calculate how long the session lasted.</span></span>

<span data-ttu-id="69ab1-106">V tomto dokumentu zjistěte, jak vytvořit základní topologie C# Storm, který sleduje události přihlášení a odhlášení pro relace uživatelů a vypočítá dobu trvání relace.</span><span class="sxs-lookup"><span data-stu-id="69ab1-106">In this document, you learn how to create a basic C# Storm topology that tracks login and logout events for user sessions, and calculates the duration of the session.</span></span> <span data-ttu-id="69ab1-107">Topologie používá HBase jako úložiště dat trvalé.</span><span class="sxs-lookup"><span data-stu-id="69ab1-107">The topology uses HBase as a persistent data store.</span></span> <span data-ttu-id="69ab1-108">HBase můžete také provádět batch dotazy na historická data k vytvoření další statistiky.</span><span class="sxs-lookup"><span data-stu-id="69ab1-108">HBase also allows you to perform batch queries on the historical data to produce additional insights.</span></span> <span data-ttu-id="69ab1-109">Například kolik uživatelských relací byly spuštěna, nebo skončila v konkrétním časovém období.</span><span class="sxs-lookup"><span data-stu-id="69ab1-109">For example, how many user sessions were started or ended during a specific time period.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="69ab1-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="69ab1-110">Prerequisites</span></span>

* <span data-ttu-id="69ab1-111">Visual Studio a nástroje HDInsight pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="69ab1-111">Visual Studio and the HDInsight tools for Visual Studio.</span></span> <span data-ttu-id="69ab1-112">Další informace najdete v tématu [Začínáme pomocí nástrojů HDInsight pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="69ab1-112">For more information, see [Get started using the HDInsight tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

* <span data-ttu-id="69ab1-113">Apache Storm v HDInsight clusteru (založené na Windows).</span><span class="sxs-lookup"><span data-stu-id="69ab1-113">Apache Storm on HDInsight cluster (Windows-based).</span></span>

  > [!WARNING]
  > <span data-ttu-id="69ab1-114">Když SCP.NET topologie jsou podporované u clusterů Storm se systémem Linux vytvořené po 10/28/2016, sada SDK HBase pro balíček .NET, které jsou k dispozici od 10/28/2016 nebude správně fungovat na HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="69ab1-114">While SCP.NET topologies are supported on Linux-based Storm clusters created after 10/28/2016, the HBase SDK for .NET package available as of 10/28/2016 does not work correctly on Linux-based HDInsight</span></span>

* <span data-ttu-id="69ab1-115">Apache HBase v clusteru HDInsight (Linux nebo na základě Windows).</span><span class="sxs-lookup"><span data-stu-id="69ab1-115">Apache HBase on HDInsight cluster (Linux or Windows-based).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="69ab1-116">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="69ab1-116">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="69ab1-117">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="69ab1-117">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="69ab1-118">[Java](https://java.com) 1.7 nebo vyšší na vašem vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="69ab1-118">[Java](https://java.com) 1.7 or greater on your development environment.</span></span> <span data-ttu-id="69ab1-119">Java se používá k balíčku topologii při odeslání do clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="69ab1-119">Java is used to package the topology when it is submitted to the HDInsight cluster.</span></span>

  * <span data-ttu-id="69ab1-120">**JAVA_HOME** proměnnou prostředí musí odkazovat na adresář, který obsahuje Java.</span><span class="sxs-lookup"><span data-stu-id="69ab1-120">The **JAVA_HOME** environment variable must point to the directory that contains Java.</span></span>
  * <span data-ttu-id="69ab1-121">**%JAVA_HOME%/bin** adresář se musí nacházet v cestě</span><span class="sxs-lookup"><span data-stu-id="69ab1-121">The **%JAVA_HOME%/bin** directory must be in the path</span></span>

## <a name="architecture"></a><span data-ttu-id="69ab1-122">Architektura</span><span class="sxs-lookup"><span data-stu-id="69ab1-122">Architecture</span></span>

![Diagram toku dat prostřednictvím topologie](./media/hdinsight-storm-correlation-topology/correlationtopology.png)

<span data-ttu-id="69ab1-124">Obecný identifikátor korelace událostí vyžaduje pro zdroje událostí.</span><span class="sxs-lookup"><span data-stu-id="69ab1-124">Correlating events requires a common identifier for the event source.</span></span> <span data-ttu-id="69ab1-125">For example ID uživatele, ID relace nebo další část dat, který je a) jedinečný a b) součástí v všechna data odesílaná do Storm.</span><span class="sxs-lookup"><span data-stu-id="69ab1-125">For example, a user ID, session ID, or other piece of data that is a) unique and b) included in all data sent to Storm.</span></span> <span data-ttu-id="69ab1-126">Tento příklad používá hodnotu GUID představují ID relace.</span><span class="sxs-lookup"><span data-stu-id="69ab1-126">This example uses a GUID value to represent a session ID.</span></span>

<span data-ttu-id="69ab1-127">V tomto příkladu se skládá ze dvou clustery HDInsight:</span><span class="sxs-lookup"><span data-stu-id="69ab1-127">This example consists of two HDInsight clusters:</span></span>

* <span data-ttu-id="69ab1-128">HBase: úložiště dat pro historických dat</span><span class="sxs-lookup"><span data-stu-id="69ab1-128">HBase: persistent data store for historical data</span></span>
* <span data-ttu-id="69ab1-129">Storm: používá k ingestování příchozích dat</span><span class="sxs-lookup"><span data-stu-id="69ab1-129">Storm: used to ingest incoming data</span></span>

<span data-ttu-id="69ab1-130">Data je náhodně generované topologie Storm a skládá se z následujících položek:</span><span class="sxs-lookup"><span data-stu-id="69ab1-130">The data is randomly generated by the Storm topology, and consists of the following items:</span></span>

* <span data-ttu-id="69ab1-131">ID relace: identifikátor GUID, který jednoznačně identifikuje každou relaci</span><span class="sxs-lookup"><span data-stu-id="69ab1-131">Session ID: a GUID that uniquely identifies each session</span></span>
* <span data-ttu-id="69ab1-132">Událost: počáteční nebo koncové události.</span><span class="sxs-lookup"><span data-stu-id="69ab1-132">Event: a START or END event.</span></span> <span data-ttu-id="69ab1-133">V tomto příkladu počáteční dochází vždy konce</span><span class="sxs-lookup"><span data-stu-id="69ab1-133">For this example, START always occurs before END</span></span>
* <span data-ttu-id="69ab1-134">Čas: čas události.</span><span class="sxs-lookup"><span data-stu-id="69ab1-134">Time: the time of the event.</span></span>

<span data-ttu-id="69ab1-135">Tato data jsou zpracovány a uložená v HBase.</span><span class="sxs-lookup"><span data-stu-id="69ab1-135">This data is processed and stored in HBase.</span></span>

### <a name="storm-topology"></a><span data-ttu-id="69ab1-136">Topologie Storm</span><span class="sxs-lookup"><span data-stu-id="69ab1-136">Storm topology</span></span>

<span data-ttu-id="69ab1-137">Když se spustí relaci, **spustit** událostí je přijatých topologie a do HBase.</span><span class="sxs-lookup"><span data-stu-id="69ab1-137">When a session starts, a **START** event is received by the topology and logged to HBase.</span></span> <span data-ttu-id="69ab1-138">Při **END** události, načte topologii **spustit** událostí a vypočítá dobu mezi dvěma událostmi.</span><span class="sxs-lookup"><span data-stu-id="69ab1-138">When an **END** event is received, the topology retrieves the **START** event and calculates the time between the two events.</span></span> <span data-ttu-id="69ab1-139">To **doba trvání** hodnoty jsou pak uloženy v HBase spolu s **END** informací o události.</span><span class="sxs-lookup"><span data-stu-id="69ab1-139">This **Duration** value is then stored in HBase along with the **END** event information.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="69ab1-140">Při této topologii ukazuje základní vzor, produkční řešení by potřeba provést návrhu pro následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="69ab1-140">While this topology demonstrates the basic pattern, a production solution would need to take design for the following scenarios:</span></span>
>
> * <span data-ttu-id="69ab1-141">Příchozích mimo pořadí událostí</span><span class="sxs-lookup"><span data-stu-id="69ab1-141">Events arriving out of order</span></span>
> * <span data-ttu-id="69ab1-142">Duplicitní události</span><span class="sxs-lookup"><span data-stu-id="69ab1-142">Duplicate events</span></span>
> * <span data-ttu-id="69ab1-143">Vyřazené události</span><span class="sxs-lookup"><span data-stu-id="69ab1-143">Dropped events</span></span>

<span data-ttu-id="69ab1-144">Ukázková topologie se skládá z následujících součástí:</span><span class="sxs-lookup"><span data-stu-id="69ab1-144">The sample topology is composed of the following components:</span></span>

* <span data-ttu-id="69ab1-145">Session.cs: simuluje uživatelské relace vytvořením ID náhodných relace, počáteční čas a jak dlouho trvá relace.</span><span class="sxs-lookup"><span data-stu-id="69ab1-145">Session.cs: simulates a user session by creating a random session ID, start time, and how long the session lasts.</span></span>

* <span data-ttu-id="69ab1-146">Spout.cs: Vytvoří 100 relací, vysílá události spuštění, čeká náhodných časový limit pro každou relaci a potom vydá KONCOVÁ událost.</span><span class="sxs-lookup"><span data-stu-id="69ab1-146">Spout.cs: creates 100 sessions, emits a START event, waits the random timeout for each session, and then emits an END event.</span></span> <span data-ttu-id="69ab1-147">Potom recykluje skončila generovat nové relace.</span><span class="sxs-lookup"><span data-stu-id="69ab1-147">Then recycles ended sessions to generate new ones.</span></span>

* <span data-ttu-id="69ab1-148">HBaseLookupBolt.cs: používá k vyhledání informací o relaci z HBase ID relace.</span><span class="sxs-lookup"><span data-stu-id="69ab1-148">HBaseLookupBolt.cs: uses the session ID to look up session information from HBase.</span></span> <span data-ttu-id="69ab1-149">Při zpracování KONCOVÁ událost najde odpovídající události spuštění a vypočítá dobu trvání relace.</span><span class="sxs-lookup"><span data-stu-id="69ab1-149">When an END event is processed, it finds the corresponding START event and calculates the duration of the session.</span></span>

* <span data-ttu-id="69ab1-150">HBaseBolt.cs: Ukládá informace o HBase.</span><span class="sxs-lookup"><span data-stu-id="69ab1-150">HBaseBolt.cs: Stores information into HBase.</span></span>

* <span data-ttu-id="69ab1-151">TypeHelper.cs: Pomáhá s převod typů při čtení / zápisu do HBase.</span><span class="sxs-lookup"><span data-stu-id="69ab1-151">TypeHelper.cs: Helps with type conversion when reading from/writing to HBase.</span></span>

### <a name="hbase-schema"></a><span data-ttu-id="69ab1-152">Schéma HBase</span><span class="sxs-lookup"><span data-stu-id="69ab1-152">HBase schema</span></span>

<span data-ttu-id="69ab1-153">V HBase jsou data uložena v tabulce s následující schéma nebo nastavení:</span><span class="sxs-lookup"><span data-stu-id="69ab1-153">In HBase, the data is stored in a table with the following schema/settings:</span></span>

* <span data-ttu-id="69ab1-154">Klíč řádku: relace ID slouží jako klíč pro řádky v této tabulce.</span><span class="sxs-lookup"><span data-stu-id="69ab1-154">Row key: the session ID is used as the key for rows in this table.</span></span>

* <span data-ttu-id="69ab1-155">Rodin sloupců: název rodiny je 'CR'.</span><span class="sxs-lookup"><span data-stu-id="69ab1-155">Column family: the family name is 'cf'.</span></span> <span data-ttu-id="69ab1-156">Jsou uložené v této rodině sloupce:</span><span class="sxs-lookup"><span data-stu-id="69ab1-156">Columns stored in this family are:</span></span>

  * <span data-ttu-id="69ab1-157">událost: počáteční nebo KONCOVÝ.</span><span class="sxs-lookup"><span data-stu-id="69ab1-157">event: START or END.</span></span>

  * <span data-ttu-id="69ab1-158">čas: čas v milisekundách, kdy došlo k události.</span><span class="sxs-lookup"><span data-stu-id="69ab1-158">time: the time in milliseconds that the event occurred.</span></span>

  * <span data-ttu-id="69ab1-159">Doba trvání: délka mezi počáteční a koncové události.</span><span class="sxs-lookup"><span data-stu-id="69ab1-159">duration: the length between START and END event.</span></span>

* <span data-ttu-id="69ab1-160">VERZE: rodiny 'CR' nastavena na uchování 5 verzích každý řádek.</span><span class="sxs-lookup"><span data-stu-id="69ab1-160">VERSIONS: the 'cf' family is set to retain 5 versions of each row.</span></span>

  > [!NOTE]
  > <span data-ttu-id="69ab1-161">Verze jsou protokolu předchozí hodnot uložených pro klíč specifickým řádkem.</span><span class="sxs-lookup"><span data-stu-id="69ab1-161">Versions are a log of previous values stored for a specific row key.</span></span> <span data-ttu-id="69ab1-162">Ve výchozím nastavení HBase pouze vrací hodnotu pro nejnovější verzi řádek.</span><span class="sxs-lookup"><span data-stu-id="69ab1-162">By default, HBase only returns the value for the most recent version of a row.</span></span> <span data-ttu-id="69ab1-163">V takovém případě se používá pro stejný řádek pro každou verzi řádek je identifikována hodnotu časového razítka všechny události (START a END.).</span><span class="sxs-lookup"><span data-stu-id="69ab1-163">In this case, the same row is used for all events (START, END.) each version of a row is identified by the timestamp value.</span></span> <span data-ttu-id="69ab1-164">Verze poskytují Historický přehled události evidované pro konkrétní ID.</span><span class="sxs-lookup"><span data-stu-id="69ab1-164">Versions provide a historical view of events logged for a specific ID.</span></span>

## <a name="download-the-project"></a><span data-ttu-id="69ab1-165">Stažení projektu</span><span class="sxs-lookup"><span data-stu-id="69ab1-165">Download the project</span></span>

<span data-ttu-id="69ab1-166">Ukázkový projekt si můžete stáhnout z [https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation](https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation).</span><span class="sxs-lookup"><span data-stu-id="69ab1-166">The sample project can be downloaded from [https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation](https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation).</span></span>

<span data-ttu-id="69ab1-167">Tento soubor ke stažení obsahuje následující projekty C#:</span><span class="sxs-lookup"><span data-stu-id="69ab1-167">This download contains the following C# projects:</span></span>

* <span data-ttu-id="69ab1-168">CorrelationTopology: Topologie C# Storm, který náhodně vysílá počáteční a koncové události pro relace uživatelů.</span><span class="sxs-lookup"><span data-stu-id="69ab1-168">CorrelationTopology: C# Storm topology that randomly emits start and end events for user sessions.</span></span> <span data-ttu-id="69ab1-169">Každý relace trvá od 1 do 5 minut.</span><span class="sxs-lookup"><span data-stu-id="69ab1-169">Each session lasts between 1 and 5 minutes.</span></span>

* <span data-ttu-id="69ab1-170">SessionInfo: C# konzolovou aplikaci, která vytvoří tabulku HBase a poskytuje příklady dotazů k vrácení informací o datech uložených relací.</span><span class="sxs-lookup"><span data-stu-id="69ab1-170">SessionInfo: C# console application that creates the HBase table, and provides example queries to return information about stored session data.</span></span>

## <a name="create-the-table"></a><span data-ttu-id="69ab1-171">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="69ab1-171">Create the table</span></span>

1. <span data-ttu-id="69ab1-172">Otevřete **SessionInfo** projektu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="69ab1-172">Open the **SessionInfo** project in Visual Studio.</span></span>

2. <span data-ttu-id="69ab1-173">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **SessionInfo** projektu a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="69ab1-173">In **Solution Explorer**, right-click the **SessionInfo** project and select **Properties**.</span></span>

    ![Snímek obrazovky nabídky s vlastnostmi vybrané](./media/hdinsight-storm-correlation-topology/selectproperties.png)

3. <span data-ttu-id="69ab1-175">Vyberte **nastavení**, nastavte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="69ab1-175">Select **Settings**, then set the following values:</span></span>

   * <span data-ttu-id="69ab1-176">HBaseClusterURL: adresa URL pro váš cluster HBase.</span><span class="sxs-lookup"><span data-stu-id="69ab1-176">HBaseClusterURL: the URL to your HBase cluster.</span></span> <span data-ttu-id="69ab1-177">Například https://myhbasecluster.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="69ab1-177">For example, https://myhbasecluster.azurehdinsight.net.</span></span>

   * <span data-ttu-id="69ab1-178">HBaseClusterUserName: Správce/HTTP uživatelský účet pro váš cluster</span><span class="sxs-lookup"><span data-stu-id="69ab1-178">HBaseClusterUserName: the admin/HTTP user account for your cluster</span></span>

   * <span data-ttu-id="69ab1-179">HBaseClusterPassword: heslo pro uživatelský účet správce nebo HTTP</span><span class="sxs-lookup"><span data-stu-id="69ab1-179">HBaseClusterPassword: the password for the admin/HTTP user account</span></span>

   * <span data-ttu-id="69ab1-180">HBaseTableName: název tabulku, kterou chcete použít v tomto příkladu</span><span class="sxs-lookup"><span data-stu-id="69ab1-180">HBaseTableName: the name of the table to use with this example</span></span>

   * <span data-ttu-id="69ab1-181">HBaseTableColumnFamily: Rodiny název sloupce</span><span class="sxs-lookup"><span data-stu-id="69ab1-181">HBaseTableColumnFamily: The column family name</span></span>

     ![Obrázek dialogového okna nastavení](./media/hdinsight-storm-correlation-topology/settings.png)

4. <span data-ttu-id="69ab1-183">Spusťte řešení.</span><span class="sxs-lookup"><span data-stu-id="69ab1-183">Run the solution.</span></span> <span data-ttu-id="69ab1-184">Po zobrazení výzvy vyberte "c" klíč, který chcete vytvořit cluster HBase v tabulce.</span><span class="sxs-lookup"><span data-stu-id="69ab1-184">When prompted, select the 'c' key to create the table on your HBase cluster.</span></span>

## <a name="build-and-deploy-the-storm-topology"></a><span data-ttu-id="69ab1-185">Vytváření a nasazování topologie Storm</span><span class="sxs-lookup"><span data-stu-id="69ab1-185">Build and deploy the Storm topology</span></span>

1. <span data-ttu-id="69ab1-186">Otevřete **CorrelationTopology** řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="69ab1-186">Open the **CorrelationTopology** solution in Visual Studio.</span></span>

2. <span data-ttu-id="69ab1-187">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **CorrelationTopology** projektu a vyberte možnost Vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="69ab1-187">In **Solution Explorer**, right-click the **CorrelationTopology** project and select properties.</span></span>

3. <span data-ttu-id="69ab1-188">V okně vlastností vyberte **nastavení** a zadejte hodnoty konfigurace pro tento projekt.</span><span class="sxs-lookup"><span data-stu-id="69ab1-188">In the properties window, select **Settings** and enter the configuration values for this project.</span></span> <span data-ttu-id="69ab1-189">První 5 jsou stejné hodnoty používané **SessionInfo** projektu:</span><span class="sxs-lookup"><span data-stu-id="69ab1-189">The first 5 are the same values used by the **SessionInfo** project:</span></span>

   * <span data-ttu-id="69ab1-190">HBaseClusterURL: adresa URL pro váš cluster HBase.</span><span class="sxs-lookup"><span data-stu-id="69ab1-190">HBaseClusterURL: the URL to your HBase cluster.</span></span> <span data-ttu-id="69ab1-191">Například https://myhbasecluster.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="69ab1-191">For example, https://myhbasecluster.azurehdinsight.net.</span></span>

   * <span data-ttu-id="69ab1-192">HBaseClusterUserName: Správce/HTTP uživatelský účet pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="69ab1-192">HBaseClusterUserName: the admin/HTTP user account for your cluster.</span></span>

   * <span data-ttu-id="69ab1-193">HBaseClusterPassword: heslo pro uživatelský účet správce nebo HTTP.</span><span class="sxs-lookup"><span data-stu-id="69ab1-193">HBaseClusterPassword: the password for the admin/HTTP user account.</span></span>

   * <span data-ttu-id="69ab1-194">HBaseTableName: název tabulku, kterou chcete použít v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="69ab1-194">HBaseTableName: the name of the table to use with this example.</span></span> <span data-ttu-id="69ab1-195">Tato hodnota je stejný název tabulky jako použité v SessionInfo projektu.</span><span class="sxs-lookup"><span data-stu-id="69ab1-195">This value is the same table name as used in the SessionInfo project.</span></span>

   * <span data-ttu-id="69ab1-196">HBaseTableColumnFamily: Sloupec Název rodiny.</span><span class="sxs-lookup"><span data-stu-id="69ab1-196">HBaseTableColumnFamily: The column family name.</span></span> <span data-ttu-id="69ab1-197">Tato hodnota se stejným názvem rodiny sloupec jako použité v SessionInfo projektu.</span><span class="sxs-lookup"><span data-stu-id="69ab1-197">This value is the same column family name as used in the SessionInfo project.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="69ab1-198">Neměňte HBaseTableColumnNames, jako výchozí hodnoty jsou názvy používané **SessionInfo** načíst data.</span><span class="sxs-lookup"><span data-stu-id="69ab1-198">Do not change the HBaseTableColumnNames, as the defaults are the names used by **SessionInfo** to retrieve data.</span></span>

4. <span data-ttu-id="69ab1-199">Uložte vlastnosti a sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="69ab1-199">Save the properties, then build the project.</span></span>

5. <span data-ttu-id="69ab1-200">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **odeslání do Storm v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="69ab1-200">In **Solution Explorer**, right-click the project and select **Submit to Storm on HDInsight**.</span></span> <span data-ttu-id="69ab1-201">Pokud se zobrazí výzva, zadejte přihlašovací údaje pro vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="69ab1-201">If prompted, enter the credentials for your Azure subscription.</span></span>

   ![Obrázek odeslání na položku nabídky storm](./media/hdinsight-storm-correlation-topology/submittostorm.png)

6. <span data-ttu-id="69ab1-203">V **odeslání topologie** dialogovém okně, vyberte cluster Storm, který chcete nasadit tuto topologii.</span><span class="sxs-lookup"><span data-stu-id="69ab1-203">In the **Submit Topology** dialog, select the Storm cluster you want to deploy this topology to.</span></span>

   > [!NOTE]
   > <span data-ttu-id="69ab1-204">Při prvním odeslání topologii, může trvat několik sekund načíst název clusterům HDInsight.</span><span class="sxs-lookup"><span data-stu-id="69ab1-204">The first time you submit a topology, it may take a few seconds to retrieve the name of your HDInsight clusters.</span></span>

7. <span data-ttu-id="69ab1-205">Jakmile topologii byl nahrán a odeslaných do clusteru, **zobrazení topologie Storm** otevře a zobrazí spuštěné topologie.</span><span class="sxs-lookup"><span data-stu-id="69ab1-205">Once the topology has been uploaded and submitted to the cluster, the **Storm Topology View** opens and displays the running topology.</span></span> <span data-ttu-id="69ab1-206">Chcete-li obnovit data, vyberte **CorrelationTopology** a použijte tlačítko Aktualizovat v horní pravé části stránky.</span><span class="sxs-lookup"><span data-stu-id="69ab1-206">To refresh the data, select the **CorrelationTopology** and use the refresh button at the top right of the page.</span></span>

   ![Obrázek topologie zobrazení](./media/hdinsight-storm-correlation-topology/topologyview.png)

   <span data-ttu-id="69ab1-208">Když začne topologii generování dat, je hodnota v **Emitted** sloupec přírůstcích.</span><span class="sxs-lookup"><span data-stu-id="69ab1-208">When the topology begins generating data, the value in the **Emitted** column increments.</span></span>

   > [!NOTE]
   > <span data-ttu-id="69ab1-209">Pokud **zobrazení topologie Storm** neobsahuje automaticky otevře, otevřete ho pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="69ab1-209">If the **Storm Topology View** does not open automatically, use the following steps to open it:</span></span>
   >
   > 1. <span data-ttu-id="69ab1-210">V **Průzkumníku řešení**, rozbalte položku **Azure**a potom rozbalte **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="69ab1-210">In **Solution Explorer**, expand **Azure**, and then expand **HDInsight**.</span></span>
   > 2. <span data-ttu-id="69ab1-211">Klikněte pravým tlačítkem na cluster Storm, který topologie běží na a pak vyberte **topologie Storm zobrazení**</span><span class="sxs-lookup"><span data-stu-id="69ab1-211">Right-click the Storm cluster that the topology is running on, and then select **View Storm Topologies**</span></span>

## <a name="query-the-data"></a><span data-ttu-id="69ab1-212">Dotaz na data</span><span class="sxs-lookup"><span data-stu-id="69ab1-212">Query the data</span></span>

<span data-ttu-id="69ab1-213">Po data byla vygenerované, použijte následující postup se dotázat na údaje.</span><span class="sxs-lookup"><span data-stu-id="69ab1-213">Once data has been emitted, use the following steps to query the data.</span></span>

1. <span data-ttu-id="69ab1-214">Vraťte se na **SessionInfo** projektu.</span><span class="sxs-lookup"><span data-stu-id="69ab1-214">Return to the **SessionInfo** project.</span></span> <span data-ttu-id="69ab1-215">Pokud není spuštěná, spusťte novou instanci.</span><span class="sxs-lookup"><span data-stu-id="69ab1-215">If not running, start a new instance of it.</span></span>

2. <span data-ttu-id="69ab1-216">Po zobrazení výzvy vyberte **s** k vyhledání počáteční událostí.</span><span class="sxs-lookup"><span data-stu-id="69ab1-216">When prompted, select **s** to search for START event.</span></span> <span data-ttu-id="69ab1-217">Zobrazí se výzva k zadání počáteční a koncový čas definovat časové rozmezí - se vrátí jenom události mezi těmito časy.</span><span class="sxs-lookup"><span data-stu-id="69ab1-217">You are prompted to enter a start and end time to define a time range - only events between these two times are returned.</span></span>

    <span data-ttu-id="69ab1-218">Při zadávání počáteční a koncový čas, použijte následující formát: hh: mm a "" m"nebo 'pm'.</span><span class="sxs-lookup"><span data-stu-id="69ab1-218">Use the following format when entering the start and end times: HH:MM and 'am' or 'pm'.</span></span> <span data-ttu-id="69ab1-219">Například 23:20:00.</span><span class="sxs-lookup"><span data-stu-id="69ab1-219">For example, 11:20pm.</span></span>

    <span data-ttu-id="69ab1-220">Pokud chcete vrátit protokolované události, použijte čas zahájení z před nasazená topologie Storm a koncový čas z nyní.</span><span class="sxs-lookup"><span data-stu-id="69ab1-220">To return logged events, use a start time from before the Storm topology was deployed, and an end time of now.</span></span> <span data-ttu-id="69ab1-221">Vracených dat obsahuje položky podobné následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="69ab1-221">The return data contains entries similar to the following text:</span></span>

        Session e6992b3e-79be-4991-afcf-5cb47dd1c81c started at 6/5/2015 6:10:15 PM. Timestamp = 1433527820737

<span data-ttu-id="69ab1-222">Hledání koncové události funguje stejně jako počáteční události.</span><span class="sxs-lookup"><span data-stu-id="69ab1-222">Searching for END events works the same as START events.</span></span> <span data-ttu-id="69ab1-223">Ale koncové události jsou generovány, náhodně rozmezí 1 až 5 minut po události spuštění.</span><span class="sxs-lookup"><span data-stu-id="69ab1-223">However, END events are generated randomly between 1 and 5 minutes after the START event.</span></span> <span data-ttu-id="69ab1-224">Možná budete muset zkuste několik časových rozsahů najít koncové události.</span><span class="sxs-lookup"><span data-stu-id="69ab1-224">You may have to try a few time ranges to find the END events.</span></span> <span data-ttu-id="69ab1-225">KONCOVÉ události obsahovat také trvání relace - rozdíl mezi čas události počáteční a KONCOVÝ čas události.</span><span class="sxs-lookup"><span data-stu-id="69ab1-225">END events also contain the duration of the session - the difference between the START event time and END event time.</span></span> <span data-ttu-id="69ab1-226">Tady je příklad dat pro koncové události:</span><span class="sxs-lookup"><span data-stu-id="69ab1-226">Here is an example of data for END events:</span></span>

    Session fc9fa8e6-6892-4073-93b3-a587040d892e lasted 2 minutes, and ended at 6/5/2015 6:12:15 PM

> [!NOTE]
> <span data-ttu-id="69ab1-227">Hodnoty času, kterou zadáte, jsou v místním čase, je čas vrácených dotazem ve standardu UTC.</span><span class="sxs-lookup"><span data-stu-id="69ab1-227">While the time values you enter are in local time, the time returned from the query is in UTC.</span></span>

## <a name="stop-the-topology"></a><span data-ttu-id="69ab1-228">Zastavení topologie</span><span class="sxs-lookup"><span data-stu-id="69ab1-228">Stop the topology</span></span>

<span data-ttu-id="69ab1-229">Pokud budete chtít zastavit topologii, vraťte se do **CorrelationTopology** projektu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="69ab1-229">When you are ready to stop the topology, return to the **CorrelationTopology** project in Visual Studio.</span></span> <span data-ttu-id="69ab1-230">V **zobrazení topologie Storm**, vyberte topologii a potom pomocí **Kill** tlačítka v horní části zobrazení topologie.</span><span class="sxs-lookup"><span data-stu-id="69ab1-230">In the **Storm Topology View**, select the topology and then use the **Kill** button at the top of the topology view.</span></span>

## <a name="delete-your-cluster"></a><span data-ttu-id="69ab1-231">Odstranění clusteru</span><span class="sxs-lookup"><span data-stu-id="69ab1-231">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="69ab1-232">Další kroky</span><span class="sxs-lookup"><span data-stu-id="69ab1-232">Next steps</span></span>

<span data-ttu-id="69ab1-233">Další příklady Storm naleznete v tématu [příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="69ab1-233">For more Storm examples, see [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md).</span></span>
