---
title: "aaaCorrelate události v čase pomocí nástrojů Storm a HBase v HDInsight"
description: "Zjistěte, jak toocorrelate události, které přicházejí v různou dobu pomocí nástrojů Storm a HBase v HDInsight."
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
ms.openlocfilehash: f915eef77bbda5b02bfd02ad0b5a4923ff2f4f26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="correlate-events-that-arrive-at-different-times-using-storm-and-hbase"></a><span data-ttu-id="bfcf9-103">Korelovat události, které přicházejí v různou dobu pomocí nástrojů Storm a HBase</span><span class="sxs-lookup"><span data-stu-id="bfcf9-103">Correlate events that arrive at different times using Storm and HBase</span></span>

<span data-ttu-id="bfcf9-104">Pomocí úložiště trvalé dat s Apache Storm mohou korelovat datové položky, které přicházejí v různých časech.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-104">By using a persistent data store with Apache Storm, you can correlate data entries that arrive at different times.</span></span> <span data-ttu-id="bfcf9-105">Například propojení události přihlášení a odhlášení pro toocalculate relace uživatele, jak dlouho hello relace už bylo.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-105">For example, linking login and logout events for a user session toocalculate how long hello session lasted.</span></span>

<span data-ttu-id="bfcf9-106">V tomto dokumentu, zjistíte, jak toocreate základní topologie C# Storm, který sleduje události přihlášení a odhlášení pro relace uživatelů a vypočítá dobu trvání relace hello hello.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-106">In this document, you learn how toocreate a basic C# Storm topology that tracks login and logout events for user sessions, and calculates hello duration of hello session.</span></span> <span data-ttu-id="bfcf9-107">topologie Hello používá HBase jako úložiště dat trvalé.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-107">hello topology uses HBase as a persistent data store.</span></span> <span data-ttu-id="bfcf9-108">HBase můžete taky tooperform batch dotazy na další statistiky tooproduce hello historická data.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-108">HBase also allows you tooperform batch queries on hello historical data tooproduce additional insights.</span></span> <span data-ttu-id="bfcf9-109">Například kolik uživatelských relací byly spuštěna, nebo skončila v konkrétním časovém období.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-109">For example, how many user sessions were started or ended during a specific time period.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bfcf9-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bfcf9-110">Prerequisites</span></span>

* <span data-ttu-id="bfcf9-111">Visual Studio a hello nástroje HDInsight pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-111">Visual Studio and hello HDInsight tools for Visual Studio.</span></span> <span data-ttu-id="bfcf9-112">Další informace najdete v tématu [začít používat hello nástroje HDInsight pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="bfcf9-112">For more information, see [Get started using hello HDInsight tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

* <span data-ttu-id="bfcf9-113">Apache Storm v HDInsight clusteru (založené na Windows).</span><span class="sxs-lookup"><span data-stu-id="bfcf9-113">Apache Storm on HDInsight cluster (Windows-based).</span></span>

  > [!WARNING]
  > <span data-ttu-id="bfcf9-114">Když SCP.NET topologie jsou podporované u clusterů Storm se systémem Linux vytvořené po 10/28/2016, hello HBase SDK pro .NET balíčku k dispozici od 10/28/2016 nebude správně fungovat na HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="bfcf9-114">While SCP.NET topologies are supported on Linux-based Storm clusters created after 10/28/2016, hello HBase SDK for .NET package available as of 10/28/2016 does not work correctly on Linux-based HDInsight</span></span>

* <span data-ttu-id="bfcf9-115">Apache HBase v clusteru HDInsight (Linux nebo na základě Windows).</span><span class="sxs-lookup"><span data-stu-id="bfcf9-115">Apache HBase on HDInsight cluster (Linux or Windows-based).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="bfcf9-116">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-116">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="bfcf9-117">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="bfcf9-117">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="bfcf9-118">[Java](https://java.com) 1.7 nebo vyšší na vašem vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-118">[Java](https://java.com) 1.7 or greater on your development environment.</span></span> <span data-ttu-id="bfcf9-119">Java je použité toopackage hello topologie, pokud je odeslaná toohello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-119">Java is used toopackage hello topology when it is submitted toohello HDInsight cluster.</span></span>

  * <span data-ttu-id="bfcf9-120">Hello **JAVA_HOME** prostředí proměnné musí bodu toohello adresář, který obsahuje Java.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-120">hello **JAVA_HOME** environment variable must point toohello directory that contains Java.</span></span>
  * <span data-ttu-id="bfcf9-121">Hello **%JAVA_HOME%/bin** adresář se musí nacházet v cestě hello</span><span class="sxs-lookup"><span data-stu-id="bfcf9-121">hello **%JAVA_HOME%/bin** directory must be in hello path</span></span>

## <a name="architecture"></a><span data-ttu-id="bfcf9-122">Architektura</span><span class="sxs-lookup"><span data-stu-id="bfcf9-122">Architecture</span></span>

![Diagram toku dat hello prostřednictvím hello topologie](./media/hdinsight-storm-correlation-topology/correlationtopology.png)

<span data-ttu-id="bfcf9-124">Obecný identifikátor korelace událostí vyžaduje pro zdroj události hello.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-124">Correlating events requires a common identifier for hello event source.</span></span> <span data-ttu-id="bfcf9-125">For example ID uživatele, ID relace nebo další část dat, je (a) jedinečný a b) zahrnuli do všech dat odesílaných tooStorm.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-125">For example, a user ID, session ID, or other piece of data that is a) unique and b) included in all data sent tooStorm.</span></span> <span data-ttu-id="bfcf9-126">Tento příklad používá toorepresent hodnota GUID ID relace.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-126">This example uses a GUID value toorepresent a session ID.</span></span>

<span data-ttu-id="bfcf9-127">V tomto příkladu se skládá ze dvou clustery HDInsight:</span><span class="sxs-lookup"><span data-stu-id="bfcf9-127">This example consists of two HDInsight clusters:</span></span>

* <span data-ttu-id="bfcf9-128">HBase: úložiště dat pro historických dat</span><span class="sxs-lookup"><span data-stu-id="bfcf9-128">HBase: persistent data store for historical data</span></span>
* <span data-ttu-id="bfcf9-129">Storm: používá tooingest příchozích dat</span><span class="sxs-lookup"><span data-stu-id="bfcf9-129">Storm: used tooingest incoming data</span></span>

<span data-ttu-id="bfcf9-130">Hello data je náhodně generované topologie Storm hello a se skládá z hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="bfcf9-130">hello data is randomly generated by hello Storm topology, and consists of hello following items:</span></span>

* <span data-ttu-id="bfcf9-131">ID relace: identifikátor GUID, který jednoznačně identifikuje každou relaci</span><span class="sxs-lookup"><span data-stu-id="bfcf9-131">Session ID: a GUID that uniquely identifies each session</span></span>
* <span data-ttu-id="bfcf9-132">Událost: počáteční nebo koncové události.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-132">Event: a START or END event.</span></span> <span data-ttu-id="bfcf9-133">V tomto příkladu počáteční dochází vždy konce</span><span class="sxs-lookup"><span data-stu-id="bfcf9-133">For this example, START always occurs before END</span></span>
* <span data-ttu-id="bfcf9-134">Čas: čas hello hello událostí.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-134">Time: hello time of hello event.</span></span>

<span data-ttu-id="bfcf9-135">Tato data jsou zpracovány a uložená v HBase.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-135">This data is processed and stored in HBase.</span></span>

### <a name="storm-topology"></a><span data-ttu-id="bfcf9-136">Topologie Storm</span><span class="sxs-lookup"><span data-stu-id="bfcf9-136">Storm topology</span></span>

<span data-ttu-id="bfcf9-137">Když se spustí relaci, **spustit** událost přijme hello topologie a protokolovat tooHBase.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-137">When a session starts, a **START** event is received by hello topology and logged tooHBase.</span></span> <span data-ttu-id="bfcf9-138">Když **END** události, hello topologie načte hello **spustit** událostí a vypočítá hello čas mezi událostmi hello dva.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-138">When an **END** event is received, hello topology retrieves hello **START** event and calculates hello time between hello two events.</span></span> <span data-ttu-id="bfcf9-139">To **doba trvání** hodnoty jsou pak uloženy v HBase společně s hello **END** informací o události.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-139">This **Duration** value is then stored in HBase along with hello **END** event information.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bfcf9-140">Zatímco tato topologie znázorňuje hello základní vzor, produkční řešení potřebovat tootake návrhu pro hello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="bfcf9-140">While this topology demonstrates hello basic pattern, a production solution would need tootake design for hello following scenarios:</span></span>
>
> * <span data-ttu-id="bfcf9-141">Příchozích mimo pořadí událostí</span><span class="sxs-lookup"><span data-stu-id="bfcf9-141">Events arriving out of order</span></span>
> * <span data-ttu-id="bfcf9-142">Duplicitní události</span><span class="sxs-lookup"><span data-stu-id="bfcf9-142">Duplicate events</span></span>
> * <span data-ttu-id="bfcf9-143">Vyřazené události</span><span class="sxs-lookup"><span data-stu-id="bfcf9-143">Dropped events</span></span>

<span data-ttu-id="bfcf9-144">Ukázková topologie Hello se skládá z hello následující součásti:</span><span class="sxs-lookup"><span data-stu-id="bfcf9-144">hello sample topology is composed of hello following components:</span></span>

* <span data-ttu-id="bfcf9-145">Session.cs: simuluje vytvořením ID náhodných relace, počáteční čas a jak dlouho hello relace vydrží uživatelské relace.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-145">Session.cs: simulates a user session by creating a random session ID, start time, and how long hello session lasts.</span></span>

* <span data-ttu-id="bfcf9-146">Spout.cs: Vytvoří 100 relací, vysílá události spuštění, čeká hello náhodných vypršení časového limitu pro každou relaci a potom vydá KONCOVÁ událost.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-146">Spout.cs: creates 100 sessions, emits a START event, waits hello random timeout for each session, and then emits an END event.</span></span> <span data-ttu-id="bfcf9-147">Potom recykluje skončila relací toogenerate nové.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-147">Then recycles ended sessions toogenerate new ones.</span></span>

* <span data-ttu-id="bfcf9-148">HBaseLookupBolt.cs: používá toolook ID relace hello relace informací z HBase.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-148">HBaseLookupBolt.cs: uses hello session ID toolook up session information from HBase.</span></span> <span data-ttu-id="bfcf9-149">Při zpracování KONCOVÁ událost najde hello odpovídající počáteční událost a vypočítá dobu trvání relace hello hello.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-149">When an END event is processed, it finds hello corresponding START event and calculates hello duration of hello session.</span></span>

* <span data-ttu-id="bfcf9-150">HBaseBolt.cs: Ukládá informace o HBase.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-150">HBaseBolt.cs: Stores information into HBase.</span></span>

* <span data-ttu-id="bfcf9-151">TypeHelper.cs: Pomáhá s převod typů při čtení / zápis tooHBase.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-151">TypeHelper.cs: Helps with type conversion when reading from/writing tooHBase.</span></span>

### <a name="hbase-schema"></a><span data-ttu-id="bfcf9-152">Schéma HBase</span><span class="sxs-lookup"><span data-stu-id="bfcf9-152">HBase schema</span></span>

<span data-ttu-id="bfcf9-153">V HBase hello data jsou uložena v tabulce s hello následující schéma nebo nastavení:</span><span class="sxs-lookup"><span data-stu-id="bfcf9-153">In HBase, hello data is stored in a table with hello following schema/settings:</span></span>

* <span data-ttu-id="bfcf9-154">Klíč řádku: hello relace ID slouží jako klíč hello řádků v této tabulce.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-154">Row key: hello session ID is used as hello key for rows in this table.</span></span>

* <span data-ttu-id="bfcf9-155">Rodin sloupců: název rodiny hello je 'CR'.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-155">Column family: hello family name is 'cf'.</span></span> <span data-ttu-id="bfcf9-156">Jsou uložené v této rodině sloupce:</span><span class="sxs-lookup"><span data-stu-id="bfcf9-156">Columns stored in this family are:</span></span>

  * <span data-ttu-id="bfcf9-157">událost: počáteční nebo KONCOVÝ.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-157">event: START or END.</span></span>

  * <span data-ttu-id="bfcf9-158">čas: hello čas v milisekundách, po jejímž hello událostí došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-158">time: hello time in milliseconds that hello event occurred.</span></span>

  * <span data-ttu-id="bfcf9-159">Doba trvání: hello délka mezi počáteční a koncové události.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-159">duration: hello length between START and END event.</span></span>

* <span data-ttu-id="bfcf9-160">VERZE: hello 'CR' rodiny nastavena tooretain 5 verzích každý řádek.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-160">VERSIONS: hello 'cf' family is set tooretain 5 versions of each row.</span></span>

  > [!NOTE]
  > <span data-ttu-id="bfcf9-161">Verze jsou protokolu předchozí hodnot uložených pro klíč specifickým řádkem.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-161">Versions are a log of previous values stored for a specific row key.</span></span> <span data-ttu-id="bfcf9-162">Ve výchozím nastavení HBase pouze vrací hodnotu hello hello nejnovější verze řádku.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-162">By default, HBase only returns hello value for hello most recent version of a row.</span></span> <span data-ttu-id="bfcf9-163">V takovém případě hello stejný řádek se používá pro každou verzi řádek je identifikována hodnotu časového razítka hello všechny události (START a END.).</span><span class="sxs-lookup"><span data-stu-id="bfcf9-163">In this case, hello same row is used for all events (START, END.) each version of a row is identified by hello timestamp value.</span></span> <span data-ttu-id="bfcf9-164">Verze poskytují Historický přehled události evidované pro konkrétní ID.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-164">Versions provide a historical view of events logged for a specific ID.</span></span>

## <a name="download-hello-project"></a><span data-ttu-id="bfcf9-165">Stáhněte si projekt hello</span><span class="sxs-lookup"><span data-stu-id="bfcf9-165">Download hello project</span></span>

<span data-ttu-id="bfcf9-166">Ukázkový projekt Hello si můžete stáhnout z [https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation](https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation).</span><span class="sxs-lookup"><span data-stu-id="bfcf9-166">hello sample project can be downloaded from [https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation](https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation).</span></span>

<span data-ttu-id="bfcf9-167">Tento soubor ke stažení obsahuje hello následující projektů C#:</span><span class="sxs-lookup"><span data-stu-id="bfcf9-167">This download contains hello following C# projects:</span></span>

* <span data-ttu-id="bfcf9-168">CorrelationTopology: Topologie C# Storm, který náhodně vysílá počáteční a koncové události pro relace uživatelů.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-168">CorrelationTopology: C# Storm topology that randomly emits start and end events for user sessions.</span></span> <span data-ttu-id="bfcf9-169">Každý relace trvá od 1 do 5 minut.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-169">Each session lasts between 1 and 5 minutes.</span></span>

* <span data-ttu-id="bfcf9-170">SessionInfo: C# konzolovou aplikaci, která vytvoří tabulku HBase hello a poskytuje příklad dotazy tooreturn informace o datech uložených relací.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-170">SessionInfo: C# console application that creates hello HBase table, and provides example queries tooreturn information about stored session data.</span></span>

## <a name="create-hello-table"></a><span data-ttu-id="bfcf9-171">Vytvoření tabulky hello</span><span class="sxs-lookup"><span data-stu-id="bfcf9-171">Create hello table</span></span>

1. <span data-ttu-id="bfcf9-172">Otevřete hello **SessionInfo** projektu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-172">Open hello **SessionInfo** project in Visual Studio.</span></span>

2. <span data-ttu-id="bfcf9-173">V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **SessionInfo** projektu a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-173">In **Solution Explorer**, right-click hello **SessionInfo** project and select **Properties**.</span></span>

    ![Snímek obrazovky nabídky s vlastnostmi vybrané](./media/hdinsight-storm-correlation-topology/selectproperties.png)

3. <span data-ttu-id="bfcf9-175">Vyberte **nastavení**, pak sada hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="bfcf9-175">Select **Settings**, then set hello following values:</span></span>

   * <span data-ttu-id="bfcf9-176">HBaseClusterURL: hello URL tooyour HBase clusteru.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-176">HBaseClusterURL: hello URL tooyour HBase cluster.</span></span> <span data-ttu-id="bfcf9-177">Například https://myhbasecluster.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-177">For example, https://myhbasecluster.azurehdinsight.net.</span></span>

   * <span data-ttu-id="bfcf9-178">HBaseClusterUserName: hello správce/HTTP uživatelský účet pro váš cluster</span><span class="sxs-lookup"><span data-stu-id="bfcf9-178">HBaseClusterUserName: hello admin/HTTP user account for your cluster</span></span>

   * <span data-ttu-id="bfcf9-179">HBaseClusterPassword: hello heslo pro uživatelský účet správce nebo HTTP hello</span><span class="sxs-lookup"><span data-stu-id="bfcf9-179">HBaseClusterPassword: hello password for hello admin/HTTP user account</span></span>

   * <span data-ttu-id="bfcf9-180">HBaseTableName: název hello toouse tabulky hello tento příklad</span><span class="sxs-lookup"><span data-stu-id="bfcf9-180">HBaseTableName: hello name of hello table toouse with this example</span></span>

   * <span data-ttu-id="bfcf9-181">HBaseTableColumnFamily: název řady sloupce hello</span><span class="sxs-lookup"><span data-stu-id="bfcf9-181">HBaseTableColumnFamily: hello column family name</span></span>

     ![Obrázek dialogového okna nastavení](./media/hdinsight-storm-correlation-topology/settings.png)

4. <span data-ttu-id="bfcf9-183">Spuštění hello řešení.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-183">Run hello solution.</span></span> <span data-ttu-id="bfcf9-184">Po zobrazení výzvy vyberte hello "c" klíče toocreate hello tabulku v clusteru HBase.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-184">When prompted, select hello 'c' key toocreate hello table on your HBase cluster.</span></span>

## <a name="build-and-deploy-hello-storm-topology"></a><span data-ttu-id="bfcf9-185">Vytváření a nasazování topologie Storm hello</span><span class="sxs-lookup"><span data-stu-id="bfcf9-185">Build and deploy hello Storm topology</span></span>

1. <span data-ttu-id="bfcf9-186">Otevřete hello **CorrelationTopology** řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-186">Open hello **CorrelationTopology** solution in Visual Studio.</span></span>

2. <span data-ttu-id="bfcf9-187">V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **CorrelationTopology** projektu a vyberte možnost Vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-187">In **Solution Explorer**, right-click hello **CorrelationTopology** project and select properties.</span></span>

3. <span data-ttu-id="bfcf9-188">V okně vlastností hello vyberte **nastavení** a zadejte hodnoty konfigurace hello pro tento projekt.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-188">In hello properties window, select **Settings** and enter hello configuration values for this project.</span></span> <span data-ttu-id="bfcf9-189">Hello první 5 jsou hello používá stejné hodnoty hello **SessionInfo** projektu:</span><span class="sxs-lookup"><span data-stu-id="bfcf9-189">hello first 5 are hello same values used by hello **SessionInfo** project:</span></span>

   * <span data-ttu-id="bfcf9-190">HBaseClusterURL: hello URL tooyour HBase clusteru.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-190">HBaseClusterURL: hello URL tooyour HBase cluster.</span></span> <span data-ttu-id="bfcf9-191">Například https://myhbasecluster.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-191">For example, https://myhbasecluster.azurehdinsight.net.</span></span>

   * <span data-ttu-id="bfcf9-192">HBaseClusterUserName: hello správce/HTTP uživatelský účet pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-192">HBaseClusterUserName: hello admin/HTTP user account for your cluster.</span></span>

   * <span data-ttu-id="bfcf9-193">HBaseClusterPassword: hello heslo pro uživatelský účet správce nebo HTTP hello.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-193">HBaseClusterPassword: hello password for hello admin/HTTP user account.</span></span>

   * <span data-ttu-id="bfcf9-194">HBaseTableName: název hello hello toouse tabulky se v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-194">HBaseTableName: hello name of hello table toouse with this example.</span></span> <span data-ttu-id="bfcf9-195">Tato hodnota je hello stejný název tabulky v rámci projektu SessionInfo hello.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-195">This value is hello same table name as used in hello SessionInfo project.</span></span>

   * <span data-ttu-id="bfcf9-196">HBaseTableColumnFamily: název řady sloupce hello.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-196">HBaseTableColumnFamily: hello column family name.</span></span> <span data-ttu-id="bfcf9-197">Tato hodnota je hello stejný název rodiny sloupec v rámci projektu SessionInfo hello.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-197">This value is hello same column family name as used in hello SessionInfo project.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="bfcf9-198">Neměňte hello HBaseTableColumnNames, jako výchozí hodnoty hello jsou názvy hello používá **SessionInfo** tooretrieve data.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-198">Do not change hello HBaseTableColumnNames, as hello defaults are hello names used by **SessionInfo** tooretrieve data.</span></span>

4. <span data-ttu-id="bfcf9-199">Uložit hello vlastnosti a potom sestavení projektu hello.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-199">Save hello properties, then build hello project.</span></span>

5. <span data-ttu-id="bfcf9-200">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a vyberte **odeslání tooStorm v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-200">In **Solution Explorer**, right-click hello project and select **Submit tooStorm on HDInsight**.</span></span> <span data-ttu-id="bfcf9-201">Pokud se zobrazí výzva, zadejte přihlašovací údaje hello vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-201">If prompted, enter hello credentials for your Azure subscription.</span></span>

   ![Obrázek hello odeslání toostorm položky nabídky](./media/hdinsight-storm-correlation-topology/submittostorm.png)

6. <span data-ttu-id="bfcf9-203">V hello **odeslání topologie** dialogové okno, chcete toodeploy Tato topologie do clusteru Storm vyberte hello.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-203">In hello **Submit Topology** dialog, select hello Storm cluster you want toodeploy this topology to.</span></span>

   > [!NOTE]
   > <span data-ttu-id="bfcf9-204">Hello poprvé odeslání topologii, může trvat několik sekund tooretrieve hello název clusterům HDInsight.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-204">hello first time you submit a topology, it may take a few seconds tooretrieve hello name of your HDInsight clusters.</span></span>

7. <span data-ttu-id="bfcf9-205">Jakmile hello topologie už nahrávat a Odeslaná toohello clusteru, hello **zobrazení topologie Storm** otevře a zobrazí hello spuštěná topologie.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-205">Once hello topology has been uploaded and submitted toohello cluster, hello **Storm Topology View** opens and displays hello running topology.</span></span> <span data-ttu-id="bfcf9-206">toorefresh hello data, vyberte hello **CorrelationTopology** a použijte tlačítko Aktualizovat hello v hello top pravé části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-206">toorefresh hello data, select hello **CorrelationTopology** and use hello refresh button at hello top right of hello page.</span></span>

   ![Obrázek zobrazení topologie hello](./media/hdinsight-storm-correlation-topology/topologyview.png)

   <span data-ttu-id="bfcf9-208">Po zahájení generování dat hello topologie hello hodnota v hello **Emitted** sloupec přírůstcích.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-208">When hello topology begins generating data, hello value in hello **Emitted** column increments.</span></span>

   > [!NOTE]
   > <span data-ttu-id="bfcf9-209">Pokud hello **zobrazení topologie Storm** neobsahuje automaticky otevře, použijte hello následující tooopen kroky:</span><span class="sxs-lookup"><span data-stu-id="bfcf9-209">If hello **Storm Topology View** does not open automatically, use hello following steps tooopen it:</span></span>
   >
   > 1. <span data-ttu-id="bfcf9-210">V **Průzkumníku řešení**, rozbalte položku **Azure**a potom rozbalte **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-210">In **Solution Explorer**, expand **Azure**, and then expand **HDInsight**.</span></span>
   > 2. <span data-ttu-id="bfcf9-211">Klikněte pravým tlačítkem na cluster Storm hello, který hello topologie je spuštěný a pak vyberte **topologie Storm zobrazení**</span><span class="sxs-lookup"><span data-stu-id="bfcf9-211">Right-click hello Storm cluster that hello topology is running on, and then select **View Storm Topologies**</span></span>

## <a name="query-hello-data"></a><span data-ttu-id="bfcf9-212">Dotaz na data hello</span><span class="sxs-lookup"><span data-stu-id="bfcf9-212">Query hello data</span></span>

<span data-ttu-id="bfcf9-213">Jakmile data byla vygenerované, použijte následující kroky tooquery hello data hello.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-213">Once data has been emitted, use hello following steps tooquery hello data.</span></span>

1. <span data-ttu-id="bfcf9-214">Vrátí toohello **SessionInfo** projektu.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-214">Return toohello **SessionInfo** project.</span></span> <span data-ttu-id="bfcf9-215">Pokud není spuštěná, spusťte novou instanci.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-215">If not running, start a new instance of it.</span></span>

2. <span data-ttu-id="bfcf9-216">Po zobrazení výzvy vyberte **s** toosearch pro spuštění událost.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-216">When prompted, select **s** toosearch for START event.</span></span> <span data-ttu-id="bfcf9-217">Jsou výzvami tooenter spuštění a ukončení toodefine čas časové rozmezí - se vrátí jenom události mezi těmito časy.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-217">You are prompted tooenter a start and end time toodefine a time range - only events between these two times are returned.</span></span>

    <span data-ttu-id="bfcf9-218">Použijte následující hello formátu při zadávání hello spuštění a ukončení: hh: mm a "" m"nebo 'pm'.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-218">Use hello following format when entering hello start and end times: HH:MM and 'am' or 'pm'.</span></span> <span data-ttu-id="bfcf9-219">Například 23:20:00.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-219">For example, 11:20pm.</span></span>

    <span data-ttu-id="bfcf9-220">tooreturn protokolují události, použijte čas zahájení z před hello nasazená topologie Storm a koncový čas z nyní.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-220">tooreturn logged events, use a start time from before hello Storm topology was deployed, and an end time of now.</span></span> <span data-ttu-id="bfcf9-221">Hello vracených dat obsahuje položky podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="bfcf9-221">hello return data contains entries similar toohello following text:</span></span>

        Session e6992b3e-79be-4991-afcf-5cb47dd1c81c started at 6/5/2015 6:10:15 PM. Timestamp = 1433527820737

<span data-ttu-id="bfcf9-222">Hledání koncové události funguje hello stejné jako počáteční události.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-222">Searching for END events works hello same as START events.</span></span> <span data-ttu-id="bfcf9-223">Ale koncové události jsou generovány, náhodně rozmezí 1 až 5 minut po události spuštění hello.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-223">However, END events are generated randomly between 1 and 5 minutes after hello START event.</span></span> <span data-ttu-id="bfcf9-224">Tootry může mít několik čas rozsahy toofind hello koncové události.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-224">You may have tootry a few time ranges toofind hello END events.</span></span> <span data-ttu-id="bfcf9-225">KONCOVÉ události obsahovat také hello trvání relace hello - hello rozdíl mezi čas události hello počáteční a KONCOVÝ čas události.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-225">END events also contain hello duration of hello session - hello difference between hello START event time and END event time.</span></span> <span data-ttu-id="bfcf9-226">Tady je příklad dat pro koncové události:</span><span class="sxs-lookup"><span data-stu-id="bfcf9-226">Here is an example of data for END events:</span></span>

    Session fc9fa8e6-6892-4073-93b3-a587040d892e lasted 2 minutes, and ended at 6/5/2015 6:12:15 PM

> [!NOTE]
> <span data-ttu-id="bfcf9-227">Hello časové hodnoty, které zadáte, jsou v místním čase, je čas hello vrácená z dotazu hello ve standardu UTC.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-227">While hello time values you enter are in local time, hello time returned from hello query is in UTC.</span></span>

## <a name="stop-hello-topology"></a><span data-ttu-id="bfcf9-228">Zastavení topologie hello</span><span class="sxs-lookup"><span data-stu-id="bfcf9-228">Stop hello topology</span></span>

<span data-ttu-id="bfcf9-229">Pokud jste připravené toostop hello topologie, vrátí toohello **CorrelationTopology** projektu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-229">When you are ready toostop hello topology, return toohello **CorrelationTopology** project in Visual Studio.</span></span> <span data-ttu-id="bfcf9-230">V hello **zobrazení topologie Storm**, vyberte hello topologie a potom pomocí hello **Kill** tlačítka v horní části hello hello topologie zobrazení.</span><span class="sxs-lookup"><span data-stu-id="bfcf9-230">In hello **Storm Topology View**, select hello topology and then use hello **Kill** button at hello top of hello topology view.</span></span>

## <a name="delete-your-cluster"></a><span data-ttu-id="bfcf9-231">Odstranění clusteru</span><span class="sxs-lookup"><span data-stu-id="bfcf9-231">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="bfcf9-232">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bfcf9-232">Next steps</span></span>

<span data-ttu-id="bfcf9-233">Další příklady Storm naleznete v tématu [příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="bfcf9-233">For more Storm examples, see [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md).</span></span>
