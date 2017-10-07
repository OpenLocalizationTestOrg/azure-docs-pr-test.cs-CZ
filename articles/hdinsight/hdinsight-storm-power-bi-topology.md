---
title: aaaUse Apache Storm s Power BI - Azure HDInsight | Microsoft Docs
description: "Vytvoření sestavy Power BI pomocí dat z topologie C# spuštěná na clusteru serveru Apache Storm v HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 36fe3b9c-5232-4464-8d75-95403b6da7a1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/31/2017
ms.author: larryfr
ms.openlocfilehash: 194cd8220bd60475af1d64a110e4b23ef92e1bc8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-power-bi-toovisualize-data-from-an-apache-storm-topology"></a><span data-ttu-id="edef0-103">Použít data toovisualize Power BI od topologie Apache Storm</span><span class="sxs-lookup"><span data-stu-id="edef0-103">Use Power BI toovisualize data from an Apache Storm topology</span></span>

<span data-ttu-id="edef0-104">Power BI umožňuje toovisually zobrazí data jako sestavy.</span><span class="sxs-lookup"><span data-stu-id="edef0-104">Power BI allows you toovisually display data as reports.</span></span> <span data-ttu-id="edef0-105">Tento dokument obsahuje příklad toouse Apache Storm v HDInsight toogenerate data pro Power BI.</span><span class="sxs-lookup"><span data-stu-id="edef0-105">This document provides an example of how toouse Apache Storm on HDInsight toogenerate data for Power BI.</span></span>

> [!NOTE]
> <span data-ttu-id="edef0-106">Hello kroky v tomto dokumentu spoléhají na vývojového prostředí systému Windows pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="edef0-106">hello steps in this document rely on a Windows development environment with Visual Studio.</span></span> <span data-ttu-id="edef0-107">zkompilovaný projekt Hello může být odeslaná tooa clusteru HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="edef0-107">hello compiled project can be submitted tooa Linux-based HDInsight cluster.</span></span> <span data-ttu-id="edef0-108">Pouze clustery se systémem Linux vytvořené po 10/28/2016 podporu SCP.NET topologie.</span><span class="sxs-lookup"><span data-stu-id="edef0-108">Only Linux-based clusters created after 10/28/2016 support SCP.NET topologies.</span></span>
>
> <span data-ttu-id="edef0-109">toouse topologie C# s clusterem systémem Linux, hello aktualizace balíčku Microsoft.SCP.Net.SDK NuGet použité ve vašem projektu tooversion 0.10.0.6 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="edef0-109">toouse a C# topology with a Linux-based cluster, update hello Microsoft.SCP.Net.SDK NuGet package used by your project tooversion 0.10.0.6 or higher.</span></span> <span data-ttu-id="edef0-110">Hello verzi balíčku hello se taky musí shodovat hello hlavní verzi Storm v HDInsight nainstalována.</span><span class="sxs-lookup"><span data-stu-id="edef0-110">hello version of hello package must also match hello major version of Storm installed on HDInsight.</span></span> <span data-ttu-id="edef0-111">Například Storm ve službě HDInsight verze 3.3 a 3.4 používá Storm 0.10.x, zatímco HDInsight 3.5 používá Storm 1.0.x.</span><span class="sxs-lookup"><span data-stu-id="edef0-111">For example, Storm on HDInsight versions 3.3 and 3.4 use Storm version 0.10.x, while HDInsight 3.5 uses Storm 1.0.x.</span></span>
>
> <span data-ttu-id="edef0-112">C# topologií v clusterech se systémem Linux musí používat rozhraní .NET 4.5 a používat Mono toorun na clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="edef0-112">C# topologies on Linux-based clusters must use .NET 4.5, and use Mono toorun on hello HDInsight cluster.</span></span> <span data-ttu-id="edef0-113">Většina věcí fungovat.</span><span class="sxs-lookup"><span data-stu-id="edef0-113">Most things work.</span></span> <span data-ttu-id="edef0-114">Nicméně byste měli zkontrolovat hello [Mono kompatibility](http://www.mono-project.com/docs/about-mono/compatibility/) dokumentu potenciální nekompatibility.</span><span class="sxs-lookup"><span data-stu-id="edef0-114">However you should check hello [Mono Compatibility](http://www.mono-project.com/docs/about-mono/compatibility/) document for potential incompatibilities.</span></span>
>
> <span data-ttu-id="edef0-115">Verzi Javy tohoto projektu, který funguje s HDInsight se systémem Linux nebo systému Windows, najdete v části [zpracovat události z Azure Event Hubs se Storm v HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).</span><span class="sxs-lookup"><span data-stu-id="edef0-115">For a Java version of this project, which works with Linux-based or Windows-based HDInsight, see [Process events from Azure Event Hubs with Storm on HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="edef0-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="edef0-116">Prerequisites</span></span>

* <span data-ttu-id="edef0-117">Uživatele služby Azure Active Directory s [Power BI](https://powerbi.com) přístup.</span><span class="sxs-lookup"><span data-stu-id="edef0-117">An Azure Active Directory user with [Power BI](https://powerbi.com) access.</span></span>
* <span data-ttu-id="edef0-118">Cluster služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="edef0-118">An HDInsight cluster.</span></span> <span data-ttu-id="edef0-119">Další informace najdete v tématu [Začínáme se Storm v HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="edef0-119">For more information, see [Get started with Storm on HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="edef0-120">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="edef0-120">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="edef0-121">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="edef0-121">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="edef0-122">Visual Studio (jeden hello následující verze)</span><span class="sxs-lookup"><span data-stu-id="edef0-122">Visual Studio (one of hello following versions)</span></span>

  * <span data-ttu-id="edef0-123">Visual Studio 2012 s [aktualizací 4](http://www.microsoft.com/download/details.aspx?id=39305)</span><span class="sxs-lookup"><span data-stu-id="edef0-123">Visual Studio 2012 with [update 4](http://www.microsoft.com/download/details.aspx?id=39305)</span></span>
  * <span data-ttu-id="edef0-124">Visual Studio 2013 s [aktualizací 4](http://www.microsoft.com/download/details.aspx?id=44921) nebo [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?linkid=517284&clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="edef0-124">Visual Studio 2013 with [update 4](http://www.microsoft.com/download/details.aspx?id=44921) or [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?linkid=517284&clcid=0x409)</span></span>
  * [<span data-ttu-id="edef0-125">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="edef0-125">Visual Studio 2015</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
  * <span data-ttu-id="edef0-126">Visual Studio 2017 (všechny edice)</span><span class="sxs-lookup"><span data-stu-id="edef0-126">Visual Studio 2017 (any edition)</span></span>

* <span data-ttu-id="edef0-127">Hello nástroje HDInsight pro Visual Studio: najdete v části [začít používat hello nástroje HDInsight pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) informace o informace o instalaci.</span><span class="sxs-lookup"><span data-stu-id="edef0-127">hello HDInsight Tools for Visual Studio: See [Get started using hello HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) for information on installation information.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="edef0-128">Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="edef0-128">How it works</span></span>

<span data-ttu-id="edef0-129">Tento příklad obsahuje topologie C# Storm, který náhodně generuje data protokolu Internetové informační služby (IIS).</span><span class="sxs-lookup"><span data-stu-id="edef0-129">This example contains a C# Storm topology that randomly generates Internet Information Services (IIS) log data.</span></span> <span data-ttu-id="edef0-130">Tato data se pak zapíše tooa SQL Database a zde je použité toogenerate sestavy v Power BI.</span><span class="sxs-lookup"><span data-stu-id="edef0-130">This data is then written tooa SQL Database, and from there it is used toogenerate reports in Power BI.</span></span>

<span data-ttu-id="edef0-131">Hello následující soubory implementace hello hlavní funkce tohoto příkladu:</span><span class="sxs-lookup"><span data-stu-id="edef0-131">hello following files implement hello main functionality of this example:</span></span>

* <span data-ttu-id="edef0-132">**SqlAzureBolt.cs**: zapisuje informace o vytvořil v tooSQL topologie Storm hello databáze.</span><span class="sxs-lookup"><span data-stu-id="edef0-132">**SqlAzureBolt.cs**: Writes information produced in hello Storm topology tooSQL Database.</span></span>
* <span data-ttu-id="edef0-133">**IISLogsTable.sql**: hello Transact-SQL příkazy používají toogenerate hello databázi, která hello data jsou uložena v.</span><span class="sxs-lookup"><span data-stu-id="edef0-133">**IISLogsTable.sql**: hello Transact-SQL statements used toogenerate hello database that hello data is stored in.</span></span>

> [!WARNING]
> <span data-ttu-id="edef0-134">Před spuštěním hello topologie clusteru HDInsight vytvořte hello tabulky v databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="edef0-134">Create hello table in SQL Database before starting hello topology on your HDInsight cluster.</span></span>

## <a name="download-hello-example"></a><span data-ttu-id="edef0-135">Stáhnout hello – ukázka</span><span class="sxs-lookup"><span data-stu-id="edef0-135">Download hello example</span></span>

<span data-ttu-id="edef0-136">Stáhnout hello [HDInsight C# Storm Power BI příklad](https://github.com/Azure-Samples/hdinsight-dotnet-storm-powerbi).</span><span class="sxs-lookup"><span data-stu-id="edef0-136">Download hello [HDInsight C# Storm Power BI example](https://github.com/Azure-Samples/hdinsight-dotnet-storm-powerbi).</span></span> <span data-ttu-id="edef0-137">toodownload, buď rozvětvení nebo klonování pomocí [git](http://git-scm.com/), nebo použijte hello **Stáhnout** odkaz toodownload .zip hello archivu.</span><span class="sxs-lookup"><span data-stu-id="edef0-137">toodownload it, either fork/clone it using [git](http://git-scm.com/), or use hello **Download** link toodownload a .zip of hello archive.</span></span>

## <a name="create-a-database"></a><span data-ttu-id="edef0-138">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="edef0-138">Create a database</span></span>

1. <span data-ttu-id="edef0-139">toocreate databázi, použijte postup hello v hello [kurz k SQL Database](../sql-database/sql-database-get-started.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="edef0-139">toocreate a database, use hello steps in hello [SQL Database tutorial](../sql-database/sql-database-get-started.md) document.</span></span>

2. <span data-ttu-id="edef0-140">Připojit databáze toohello pomocí následující hello kroky hello [připojit tooa SQL Database pomocí sady Visual Studio](../sql-database/sql-database-connect-query.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="edef0-140">Connect toohello database by following hello steps in hello [Connect tooa SQL Database with Visual Studio](../sql-database/sql-database-connect-query.md) document.</span></span>

3. <span data-ttu-id="edef0-141">V Průzkumníku objektů, klikněte pravým tlačítkem na hello databáze a vyberte **nový dotaz**.</span><span class="sxs-lookup"><span data-stu-id="edef0-141">In Object Explorer, right-click hello database and select  **New Query**.</span></span> <span data-ttu-id="edef0-142">Vložte obsah hello hello **IISLogsTable.sql** zahrnutý v hello stáhli projektu do okna dotazu hello a potom pomocí kombinace kláves Ctrl + Shift + E tooexecute hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="edef0-142">Paste hello contents of hello **IISLogsTable.sql** file included in hello downloaded project into hello query window, and then use Ctrl + Shift + E tooexecute hello query.</span></span> <span data-ttu-id="edef0-143">Měli byste obdržet zprávu, která hello příkazy byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="edef0-143">You should receive a message that hello commands completed successfully.</span></span>

## <a name="configure-hello-sample"></a><span data-ttu-id="edef0-144">Konfigurace ukázka hello</span><span class="sxs-lookup"><span data-stu-id="edef0-144">Configure hello sample</span></span>

1. <span data-ttu-id="edef0-145">Z hello [portál Azure](https://portal.azure.com), vyberte svou databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="edef0-145">From hello [Azure portal](https://portal.azure.com), select your SQL database.</span></span> <span data-ttu-id="edef0-146">Z hello **Essentials** části hello SQL databáze okna, vyberte **zobrazit databázové připojovací řetězce**.</span><span class="sxs-lookup"><span data-stu-id="edef0-146">From hello **Essentials** section of hello SQL database blade, select **Show database connection strings**.</span></span> <span data-ttu-id="edef0-147">Hello seznamu, které se zobrazí, zkopírujte hello **ADO.NET (ověřování systému SQL)** informace.</span><span class="sxs-lookup"><span data-stu-id="edef0-147">From hello list that appears, copy hello **ADO.NET (SQL authentication)** information.</span></span>

2. <span data-ttu-id="edef0-148">Ukázka hello otevřete v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="edef0-148">Open hello sample in Visual Studio.</span></span> <span data-ttu-id="edef0-149">Z **Průzkumníku řešení**, otevřete hello **App.config** souboru a pak vyhledejte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="edef0-149">From **Solution Explorer**, open hello **App.config** file, and then find hello following entry:</span></span>

        <add key="SqlAzureConnectionString" value="##TOBEFILLED##" />

    <span data-ttu-id="edef0-150">Nahraďte hello **TOBEFILLED ##** hodnotu s připojovací řetězec databáze hello zkopírovali v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="edef0-150">Replace hello **##TOBEFILLED##** value with hello database connection string copied in hello previous step.</span></span> <span data-ttu-id="edef0-151">Nahraďte **{vaší\_uživatelské jméno}** a **{vaší\_heslo}** s hello uživatelské jméno a heslo pro databázi hello.</span><span class="sxs-lookup"><span data-stu-id="edef0-151">Replace **{your\_username}** and **{your\_password}** with hello username and password for hello database.</span></span>

3. <span data-ttu-id="edef0-152">Uložte a zavřete soubory hello.</span><span class="sxs-lookup"><span data-stu-id="edef0-152">Save and close hello files.</span></span>

## <a name="deploy-hello-sample"></a><span data-ttu-id="edef0-153">Nasazení ukázkové hello</span><span class="sxs-lookup"><span data-stu-id="edef0-153">Deploy hello sample</span></span>

1. <span data-ttu-id="edef0-154">Z **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **StormToSQL** projektu a vyberte **odeslání tooStorm v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="edef0-154">From **Solution Explorer**, right-click hello **StormToSQL** project and select **Submit tooStorm on HDInsight**.</span></span> <span data-ttu-id="edef0-155">Vyberte hello clusteru HDInsight z hello **Storm Cluster** dialogu rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="edef0-155">Select hello HDInsight cluster from hello **Storm Cluster** dropdown dialog.</span></span>

   > [!NOTE]
   > <span data-ttu-id="edef0-156">To může trvat několik sekund, než hello **Storm Cluster** toopopulate rozevírací seznam s názvy serverů.</span><span class="sxs-lookup"><span data-stu-id="edef0-156">It may take a few seconds for hello **Storm Cluster** dropdown toopopulate with server names.</span></span>
   >
   > <span data-ttu-id="edef0-157">Pokud se zobrazí výzva, zadejte hello přihlašovací údaje pro vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="edef0-157">If prompted, enter hello login credentials for your Azure subscription.</span></span> <span data-ttu-id="edef0-158">Pokud máte více než jedno předplatné, přihlaste se toohello, která obsahuje váš cluster Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="edef0-158">If you have more than one subscription, log in toohello one that contains your Storm on HDInsight cluster.</span></span>

2. <span data-ttu-id="edef0-159">Při odeslání hello topologie hello __topologie prohlížeč__ se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="edef0-159">When hello topology has been submitted, hello __Topology Viewer__ appears.</span></span> <span data-ttu-id="edef0-160">tooview tento topologie, vyberte hello SqlAzureWriterTopology položky ze seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="edef0-160">tooview this topology, select hello SqlAzureWriterTopology entry from hello list.</span></span>

    ![Hello topologie, topologie hello vybrané](./media/hdinsight-storm-power-bi-topology/topologyview.png)

    <span data-ttu-id="edef0-162">Můžete použít tyto informace zobrazit toosee na topologii hello nebo dvakrát klikněte na položku (například hello SqlAzureBolt) toosee informace tooa konkrétní součást v topologii hello.</span><span class="sxs-lookup"><span data-stu-id="edef0-162">You can use this view toosee information on hello topology, or double-click an entry (such as hello SqlAzureBolt) toosee information specific tooa component in hello topology.</span></span>

3. <span data-ttu-id="edef0-163">Po hello má topologie spustili několik minut, okno dotazu SQL návratový toohello jste použili toocreate hello databáze.</span><span class="sxs-lookup"><span data-stu-id="edef0-163">After hello topology has ran for a few minutes, return toohello SQL query window you used toocreate hello database.</span></span> <span data-ttu-id="edef0-164">Nahraďte existující příkazy hello hello následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="edef0-164">Replace hello existing statements with hello following query:</span></span>

        select * from iislogs;

    <span data-ttu-id="edef0-165">Pomocí kombinace kláves Ctrl + Shift + E tooexecute hello dotazu a mělo by se zobrazit výsledky podobné toohello následující data:</span><span class="sxs-lookup"><span data-stu-id="edef0-165">Use Ctrl + Shift + E tooexecute hello query, and you should receive results similar toohello following data:</span></span>

        1    2016-05-27 17:57:14.797    255.255.255.255    /bar    GET    200
        2    2016-05-27 17:57:14.843    127.0.0.1    /spam/eggs    POST    500
        3    2016-05-27 17:57:14.850    123.123.123.123    /eggs    DELETE    200
        4    2016-05-27 17:57:14.853    127.0.0.1    /foo    POST    404
        5    2016-05-27 17:57:14.853    10.9.8.7    /bar    GET    200
        6    2016-05-27 17:57:14.857    192.168.1.1    /spam    DELETE    200

    <span data-ttu-id="edef0-166">Tato data byla zapsána od topologie Storm hello.</span><span class="sxs-lookup"><span data-stu-id="edef0-166">This data has been written from hello Storm topology.</span></span>

## <a name="create-a-report"></a><span data-ttu-id="edef0-167">Vytvoření sestavy</span><span class="sxs-lookup"><span data-stu-id="edef0-167">Create a report</span></span>

1. <span data-ttu-id="edef0-168">Připojit toohello [konektor Azure SQL Database](https://app.powerbi.com/getdata/bigdata/azure-sql-database-with-live-connect) pro Power BI.</span><span class="sxs-lookup"><span data-stu-id="edef0-168">Connect toohello [Azure SQL Database connector](https://app.powerbi.com/getdata/bigdata/azure-sql-database-with-live-connect) for Power BI.</span></span> 

2. <span data-ttu-id="edef0-169">V rámci **databáze**, vyberte **získat**.</span><span class="sxs-lookup"><span data-stu-id="edef0-169">Within **Databases**, select **Get**.</span></span>

3. <span data-ttu-id="edef0-170">Vyberte **Azure SQL Database**a potom vyberte **Connect**.</span><span class="sxs-lookup"><span data-stu-id="edef0-170">Select **Azure SQL Database**, and then select **Connect**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="edef0-171">Můžete být vyzváni, toocontinue Power BI Desktop toodownload hello.</span><span class="sxs-lookup"><span data-stu-id="edef0-171">You may be asked toodownload hello Power BI Desktop toocontinue.</span></span> <span data-ttu-id="edef0-172">Pokud ano, použijte následující postup tooconnect hello:</span><span class="sxs-lookup"><span data-stu-id="edef0-172">If so, use hello following steps tooconnect:</span></span>
    >
    > 1. <span data-ttu-id="edef0-173">Otevřít Power BI Desktop a vyberte __načíst Data__.</span><span class="sxs-lookup"><span data-stu-id="edef0-173">Open Power BI Desktop and select __Get Data__.</span></span>
    > <span data-ttu-id="edef0-174">Vyberte 2 __Azure__a potom __Azure SQL database__.</span><span class="sxs-lookup"><span data-stu-id="edef0-174">2  Select __Azure__, and then __Azure SQL database__.</span></span>

4. <span data-ttu-id="edef0-175">Zadejte hello informace tooconnect tooyour Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="edef0-175">Enter hello information tooconnect tooyour Azure SQL Database.</span></span> <span data-ttu-id="edef0-176">Tyto informace můžete najít návštěvou hello [portál Azure](https://portal.azure.com) a vyberete databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="edef0-176">You can find this information by visiting hello [Azure portal](https://portal.azure.com) and selecting your SQL database.</span></span>

   > [!NOTE]
   > <span data-ttu-id="edef0-177">Interval obnovování hello a vlastní filtry lze také nastavit pomocí **povolit rozšířené možnosti** ze hello připojit dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="edef0-177">You can also set hello refresh interval and custom filters by using **Enable Advanced Options** from hello connect dialog.</span></span>

5. <span data-ttu-id="edef0-178">Po připojení, zobrazí se nová datová sada s hello stejný název jako databáze hello je připojen k.</span><span class="sxs-lookup"><span data-stu-id="edef0-178">After you've connected, you will see a new dataset with hello same name as hello database you connected to.</span></span> <span data-ttu-id="edef0-179">Vyberte toobegin datovou sadu hello navrhování sestavy.</span><span class="sxs-lookup"><span data-stu-id="edef0-179">Select hello dataset toobegin designing a report.</span></span>

6. <span data-ttu-id="edef0-180">Z **pole**, rozbalte položku hello **IISLOGS** položku.</span><span class="sxs-lookup"><span data-stu-id="edef0-180">From **Fields**, expand hello **IISLOGS** entry.</span></span> <span data-ttu-id="edef0-181">toocreate sestavy, seznamy hello URI vyplývá, zaškrtněte políčko hello pro **modul URISTEM**.</span><span class="sxs-lookup"><span data-stu-id="edef0-181">toocreate a report that lists hello URI stems, select hello checkbox for **URISTEM**.</span></span>

    ![Vytvoření sestavy](./media/hdinsight-storm-power-bi-topology/createreport.png)

7. <span data-ttu-id="edef0-183">V dalším kroku přetáhněte **metoda** toohello sestavy.</span><span class="sxs-lookup"><span data-stu-id="edef0-183">Next, drag **METHOD** toohello report.</span></span> <span data-ttu-id="edef0-184">vyplývá Hello sestavy aktualizace toolist hello a hello odpovídající metoda HTTP pro hello požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="edef0-184">hello report updates toolist hello stems and hello corresponding HTTP method used for hello HTTP request.</span></span>

    ![Přidání dat hello – metoda](./media/hdinsight-storm-power-bi-topology/uristemandmethod.png)

8. <span data-ttu-id="edef0-186">Z hello **vizualizace** sloupce, vyberte hello **pole** ikonu a potom vyberte hello šipka dolů vedle příliš**metoda** v hello **hodnoty**části.</span><span class="sxs-lookup"><span data-stu-id="edef0-186">From hello **Visualizations** column, select hello **Fields** icon, and then select hello down arrow next too**METHOD** in hello **Values** section.</span></span> <span data-ttu-id="edef0-187">Vyberte toodisplay přistupoval počet jak často identifikátoru URI **počet**.</span><span class="sxs-lookup"><span data-stu-id="edef0-187">toodisplay a count of how many times a URI has been accessed, select **Count**.</span></span>

    ![Změna počtu tooa metod](./media/hdinsight-storm-power-bi-topology/count.png)

9. <span data-ttu-id="edef0-189">Potom vyberte hello **skládaný sloupcový graf** toochange, jak se zobrazují informace hello.</span><span class="sxs-lookup"><span data-stu-id="edef0-189">Next, select hello **Stacked column chart** toochange how hello information is displayed.</span></span>

    ![Změna tooa skládaný graf](./media/hdinsight-storm-power-bi-topology/stackedcolumn.png)

10. <span data-ttu-id="edef0-191">toosave hello sestavu, vyberte **Uložit** a zadejte název sestavy hello.</span><span class="sxs-lookup"><span data-stu-id="edef0-191">toosave hello report, select **Save** and enter a name for hello report.</span></span>

## <a name="stop-hello-topology"></a><span data-ttu-id="edef0-192">Zastavení topologie hello</span><span class="sxs-lookup"><span data-stu-id="edef0-192">Stop hello topology</span></span>

<span data-ttu-id="edef0-193">topologie Hello pokračuje toorun, dokud nejde zastavit ani odstranit hello Storm v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="edef0-193">hello topology continues toorun until you stop it or delete hello Storm on HDInsight cluster.</span></span> <span data-ttu-id="edef0-194">toostop hello topologie, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="edef0-194">toostop hello topology, perform hello following steps:</span></span>

1. <span data-ttu-id="edef0-195">V sadě Visual Studio vraťte toohello topologie prohlížeč a vyberte topologii hello.</span><span class="sxs-lookup"><span data-stu-id="edef0-195">In Visual Studio, return toohello topology viewer and select hello topology.</span></span>

2. <span data-ttu-id="edef0-196">Vyberte hello **Kill** tlačítko toostop hello topologie.</span><span class="sxs-lookup"><span data-stu-id="edef0-196">Select hello **Kill** button toostop hello topology.</span></span>

    ![Příkaz kill tlačítko na topologii hello souhrn](./media/hdinsight-storm-power-bi-topology/killtopology.png)

## <a name="delete-your-cluster"></a><span data-ttu-id="edef0-198">Odstranění clusteru</span><span class="sxs-lookup"><span data-stu-id="edef0-198">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="edef0-199">Další kroky</span><span class="sxs-lookup"><span data-stu-id="edef0-199">Next steps</span></span>

<span data-ttu-id="edef0-200">V tomto dokumentu jste zjistili, jak toosend dat z tooSQL topologie Storm databáze, pak vizualizovat data hello pomocí Power BI.</span><span class="sxs-lookup"><span data-stu-id="edef0-200">In this document, you learned how toosend data from a Storm topology tooSQL Database, then visualize hello data using Power BI.</span></span> <span data-ttu-id="edef0-201">Informace o tom toowork s jinými Azure technologiemi používat Storm v HDInsight, najdete v části hello následujícím dokumentu:</span><span class="sxs-lookup"><span data-stu-id="edef0-201">For information on how toowork with other Azure technologies using Storm on HDInsight, see hello following document:</span></span>

* [<span data-ttu-id="edef0-202">Příklad topologií pro Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="edef0-202">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)
