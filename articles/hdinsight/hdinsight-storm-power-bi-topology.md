---
title: "Použít Apache Storm s Power BI - Azure HDInsight | Microsoft Docs"
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
ms.openlocfilehash: 36487c0c34e5a4bb955bbc15c8c96b9e838aeb44
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-power-bi-to-visualize-data-from-an-apache-storm-topology"></a><span data-ttu-id="a1c76-103">Pomocí Power BI vizualizovat data od topologie Apache Storm</span><span class="sxs-lookup"><span data-stu-id="a1c76-103">Use Power BI to visualize data from an Apache Storm topology</span></span>

<span data-ttu-id="a1c76-104">Power BI umožňuje vizuálně zobrazit data jako sestavy.</span><span class="sxs-lookup"><span data-stu-id="a1c76-104">Power BI allows you to visually display data as reports.</span></span> <span data-ttu-id="a1c76-105">Tento dokument obsahuje příklady použití Apache Storm v HDInsight pro generování dat pro Power BI.</span><span class="sxs-lookup"><span data-stu-id="a1c76-105">This document provides an example of how to use Apache Storm on HDInsight to generate data for Power BI.</span></span>

> [!NOTE]
> <span data-ttu-id="a1c76-106">Kroky v tomto dokumentu se spoléhají na vývojového prostředí systému Windows pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a1c76-106">The steps in this document rely on a Windows development environment with Visual Studio.</span></span> <span data-ttu-id="a1c76-107">Zkompilovaný projekt můžete odeslat ke clusteru HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="a1c76-107">The compiled project can be submitted to a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="a1c76-108">Pouze clustery se systémem Linux vytvořené po 10/28/2016 podporu SCP.NET topologie.</span><span class="sxs-lookup"><span data-stu-id="a1c76-108">Only Linux-based clusters created after 10/28/2016 support SCP.NET topologies.</span></span>
>
> <span data-ttu-id="a1c76-109">Pokud chcete používat topologie C# s clusterem se systémem Linux, aktualizujte balíček Microsoft.SCP.Net.SDK NuGet používaný v projektu na verzi 0.10.0.6 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="a1c76-109">To use a C# topology with a Linux-based cluster, update the Microsoft.SCP.Net.SDK NuGet package used by your project to version 0.10.0.6 or higher.</span></span> <span data-ttu-id="a1c76-110">Verze balíčku se zároveň musí shodovat s hlavní verzí Stormu nainstalovanou ve službě HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a1c76-110">The version of the package must also match the major version of Storm installed on HDInsight.</span></span> <span data-ttu-id="a1c76-111">Například Storm ve službě HDInsight verze 3.3 a 3.4 používá Storm 0.10.x, zatímco HDInsight 3.5 používá Storm 1.0.x.</span><span class="sxs-lookup"><span data-stu-id="a1c76-111">For example, Storm on HDInsight versions 3.3 and 3.4 use Storm version 0.10.x, while HDInsight 3.5 uses Storm 1.0.x.</span></span>
>
> <span data-ttu-id="a1c76-112">Topologie jazyka C# v clusterech založených na Linuxu musí používat technologii .NET 4.5. a pro spuštění v clusteru HDInsight musí používat Mono.</span><span class="sxs-lookup"><span data-stu-id="a1c76-112">C# topologies on Linux-based clusters must use .NET 4.5, and use Mono to run on the HDInsight cluster.</span></span> <span data-ttu-id="a1c76-113">Většina věcí fungovat.</span><span class="sxs-lookup"><span data-stu-id="a1c76-113">Most things work.</span></span> <span data-ttu-id="a1c76-114">Nicméně byste měli zkontrolovat [Mono kompatibility](http://www.mono-project.com/docs/about-mono/compatibility/) dokumentu potenciální nekompatibility.</span><span class="sxs-lookup"><span data-stu-id="a1c76-114">However you should check the [Mono Compatibility](http://www.mono-project.com/docs/about-mono/compatibility/) document for potential incompatibilities.</span></span>
>
> <span data-ttu-id="a1c76-115">Verzi Javy tohoto projektu, který funguje s HDInsight se systémem Linux nebo systému Windows, najdete v části [zpracovat události z Azure Event Hubs se Storm v HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).</span><span class="sxs-lookup"><span data-stu-id="a1c76-115">For a Java version of this project, which works with Linux-based or Windows-based HDInsight, see [Process events from Azure Event Hubs with Storm on HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a1c76-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a1c76-116">Prerequisites</span></span>

* <span data-ttu-id="a1c76-117">Uživatele služby Azure Active Directory s [Power BI](https://powerbi.com) přístup.</span><span class="sxs-lookup"><span data-stu-id="a1c76-117">An Azure Active Directory user with [Power BI](https://powerbi.com) access.</span></span>
* <span data-ttu-id="a1c76-118">Cluster služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a1c76-118">An HDInsight cluster.</span></span> <span data-ttu-id="a1c76-119">Další informace najdete v tématu [Začínáme se Storm v HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="a1c76-119">For more information, see [Get started with Storm on HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="a1c76-120">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="a1c76-120">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="a1c76-121">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="a1c76-121">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="a1c76-122">Visual Studio (jedna z následujících verzí)</span><span class="sxs-lookup"><span data-stu-id="a1c76-122">Visual Studio (one of the following versions)</span></span>

  * <span data-ttu-id="a1c76-123">Visual Studio 2012 s [aktualizací 4](http://www.microsoft.com/download/details.aspx?id=39305)</span><span class="sxs-lookup"><span data-stu-id="a1c76-123">Visual Studio 2012 with [update 4](http://www.microsoft.com/download/details.aspx?id=39305)</span></span>
  * <span data-ttu-id="a1c76-124">Visual Studio 2013 s [aktualizací 4](http://www.microsoft.com/download/details.aspx?id=44921) nebo [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?linkid=517284&clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="a1c76-124">Visual Studio 2013 with [update 4](http://www.microsoft.com/download/details.aspx?id=44921) or [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?linkid=517284&clcid=0x409)</span></span>
  * [<span data-ttu-id="a1c76-125">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="a1c76-125">Visual Studio 2015</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
  * <span data-ttu-id="a1c76-126">Visual Studio 2017 (všechny edice)</span><span class="sxs-lookup"><span data-stu-id="a1c76-126">Visual Studio 2017 (any edition)</span></span>

* <span data-ttu-id="a1c76-127">Nástroje HDInsight pro Visual Studio: najdete v části [Začínáme pomocí nástrojů HDInsight pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) informace o informace o instalaci.</span><span class="sxs-lookup"><span data-stu-id="a1c76-127">The HDInsight Tools for Visual Studio: See [Get started using the HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) for information on installation information.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="a1c76-128">Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="a1c76-128">How it works</span></span>

<span data-ttu-id="a1c76-129">Tento příklad obsahuje topologie C# Storm, který náhodně generuje data protokolu Internetové informační služby (IIS).</span><span class="sxs-lookup"><span data-stu-id="a1c76-129">This example contains a C# Storm topology that randomly generates Internet Information Services (IIS) log data.</span></span> <span data-ttu-id="a1c76-130">Tato data pak zapsána do databáze SQL a z ní se používá ke generování sestav ve službě Power BI.</span><span class="sxs-lookup"><span data-stu-id="a1c76-130">This data is then written to a SQL Database, and from there it is used to generate reports in Power BI.</span></span>

<span data-ttu-id="a1c76-131">Následující soubory implementovat hlavní funkce tohoto příkladu:</span><span class="sxs-lookup"><span data-stu-id="a1c76-131">The following files implement the main functionality of this example:</span></span>

* <span data-ttu-id="a1c76-132">**SqlAzureBolt.cs**: zapisuje informace o vytvořil v topologii Storm k databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="a1c76-132">**SqlAzureBolt.cs**: Writes information produced in the Storm topology to SQL Database.</span></span>
* <span data-ttu-id="a1c76-133">**IISLogsTable.sql**: příkazy jazyka Transact-SQL sloužící ke generování data uložená v databázi.</span><span class="sxs-lookup"><span data-stu-id="a1c76-133">**IISLogsTable.sql**: The Transact-SQL statements used to generate the database that the data is stored in.</span></span>

> [!WARNING]
> <span data-ttu-id="a1c76-134">Vytvoření tabulky v databázi SQL před zahájením topologii v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a1c76-134">Create the table in SQL Database before starting the topology on your HDInsight cluster.</span></span>

## <a name="download-the-example"></a><span data-ttu-id="a1c76-135">Stáhněte si příklad</span><span class="sxs-lookup"><span data-stu-id="a1c76-135">Download the example</span></span>

<span data-ttu-id="a1c76-136">Stažení [HDInsight C# Storm Power BI příklad](https://github.com/Azure-Samples/hdinsight-dotnet-storm-powerbi).</span><span class="sxs-lookup"><span data-stu-id="a1c76-136">Download the [HDInsight C# Storm Power BI example](https://github.com/Azure-Samples/hdinsight-dotnet-storm-powerbi).</span></span> <span data-ttu-id="a1c76-137">Se stáhne, buď rozvětvení nebo klonování pomocí [git](http://git-scm.com/), nebo použijte **Stáhnout** odkaz ke stažení .zip archivu.</span><span class="sxs-lookup"><span data-stu-id="a1c76-137">To download it, either fork/clone it using [git](http://git-scm.com/), or use the **Download** link to download a .zip of the archive.</span></span>

## <a name="create-a-database"></a><span data-ttu-id="a1c76-138">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="a1c76-138">Create a database</span></span>

1. <span data-ttu-id="a1c76-139">Chcete-li vytvořit databázi, použijte postup v [kurz k SQL Database](../sql-database/sql-database-get-started.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a1c76-139">To create a database, use the steps in the [SQL Database tutorial](../sql-database/sql-database-get-started.md) document.</span></span>

2. <span data-ttu-id="a1c76-140">Připojit k databázi pomocí kroků v [připojit k SQL Database pomocí sady Visual Studio](../sql-database/sql-database-connect-query.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a1c76-140">Connect to the database by following the steps in the [Connect to a SQL Database with Visual Studio](../sql-database/sql-database-connect-query.md) document.</span></span>

3. <span data-ttu-id="a1c76-141">V Průzkumníku objektů, klikněte pravým tlačítkem na databázi a vyberte **nový dotaz**.</span><span class="sxs-lookup"><span data-stu-id="a1c76-141">In Object Explorer, right-click the database and select  **New Query**.</span></span> <span data-ttu-id="a1c76-142">Umožňuje vložit obsah **IISLogsTable.sql** souboru součástí staženého projektu do okna dotazu a potom pomocí kombinace kláves Ctrl + Shift + E provést dotaz.</span><span class="sxs-lookup"><span data-stu-id="a1c76-142">Paste the contents of the **IISLogsTable.sql** file included in the downloaded project into the query window, and then use Ctrl + Shift + E to execute the query.</span></span> <span data-ttu-id="a1c76-143">Měli byste obdržet zprávu, že tyto příkazy úspěšně provedly.</span><span class="sxs-lookup"><span data-stu-id="a1c76-143">You should receive a message that the commands completed successfully.</span></span>

## <a name="configure-the-sample"></a><span data-ttu-id="a1c76-144">Ukázka konfigurace</span><span class="sxs-lookup"><span data-stu-id="a1c76-144">Configure the sample</span></span>

1. <span data-ttu-id="a1c76-145">Z [portál Azure](https://portal.azure.com), vyberte svou databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="a1c76-145">From the [Azure portal](https://portal.azure.com), select your SQL database.</span></span> <span data-ttu-id="a1c76-146">Z **Essentials** část SQL databáze okně vyberte **zobrazit databázové připojovací řetězce**.</span><span class="sxs-lookup"><span data-stu-id="a1c76-146">From the **Essentials** section of the SQL database blade, select **Show database connection strings**.</span></span> <span data-ttu-id="a1c76-147">Ze seznamu, který se zobrazí, zkopírujte **ADO.NET (ověřování systému SQL)** informace.</span><span class="sxs-lookup"><span data-stu-id="a1c76-147">From the list that appears, copy the **ADO.NET (SQL authentication)** information.</span></span>

2. <span data-ttu-id="a1c76-148">Otevřete ukázku v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a1c76-148">Open the sample in Visual Studio.</span></span> <span data-ttu-id="a1c76-149">Z **Průzkumníku řešení**, otevřete **App.config** souboru a pak vyhledejte následující položky:</span><span class="sxs-lookup"><span data-stu-id="a1c76-149">From **Solution Explorer**, open the **App.config** file, and then find the following entry:</span></span>

        <add key="SqlAzureConnectionString" value="##TOBEFILLED##" />

    <span data-ttu-id="a1c76-150">Nahraďte **TOBEFILLED ##** hodnotu s připojovací řetězec databáze zkopírovali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="a1c76-150">Replace the **##TOBEFILLED##** value with the database connection string copied in the previous step.</span></span> <span data-ttu-id="a1c76-151">Nahraďte **{vaší\_uživatelské jméno}** a **{vaší\_heslo}** pomocí uživatelského jména a hesla pro databázi.</span><span class="sxs-lookup"><span data-stu-id="a1c76-151">Replace **{your\_username}** and **{your\_password}** with the username and password for the database.</span></span>

3. <span data-ttu-id="a1c76-152">Uložte a zavřete soubory.</span><span class="sxs-lookup"><span data-stu-id="a1c76-152">Save and close the files.</span></span>

## <a name="deploy-the-sample"></a><span data-ttu-id="a1c76-153">Ukázka nasazení</span><span class="sxs-lookup"><span data-stu-id="a1c76-153">Deploy the sample</span></span>

1. <span data-ttu-id="a1c76-154">Z **Průzkumníku řešení**, klikněte pravým tlačítkem myši **StormToSQL** projektu a vyberte **odeslání do Storm v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="a1c76-154">From **Solution Explorer**, right-click the **StormToSQL** project and select **Submit to Storm on HDInsight**.</span></span> <span data-ttu-id="a1c76-155">Vyberte cluster HDInsight z **Storm Cluster** dialogu rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="a1c76-155">Select the HDInsight cluster from the **Storm Cluster** dropdown dialog.</span></span>

   > [!NOTE]
   > <span data-ttu-id="a1c76-156">To může trvat několik sekund **Storm Cluster** rozevírací k naplnění s názvy serverů.</span><span class="sxs-lookup"><span data-stu-id="a1c76-156">It may take a few seconds for the **Storm Cluster** dropdown to populate with server names.</span></span>
   >
   > <span data-ttu-id="a1c76-157">Pokud se zobrazí výzva, zadejte přihlašovací údaje pro vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="a1c76-157">If prompted, enter the login credentials for your Azure subscription.</span></span> <span data-ttu-id="a1c76-158">Pokud máte více než jedno předplatné, přihlaste se k ta, která obsahuje váš cluster Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a1c76-158">If you have more than one subscription, log in to the one that contains your Storm on HDInsight cluster.</span></span>

2. <span data-ttu-id="a1c76-159">Při odeslání topologii __topologie prohlížeč__ se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="a1c76-159">When the topology has been submitted, the __Topology Viewer__ appears.</span></span> <span data-ttu-id="a1c76-160">Pokud chcete zobrazit tato topologie, vyberte položku SqlAzureWriterTopology ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="a1c76-160">To view this topology, select the SqlAzureWriterTopology entry from the list.</span></span>

    ![Topologie, topologie vybrané](./media/hdinsight-storm-power-bi-topology/topologyview.png)

    <span data-ttu-id="a1c76-162">Můžete v tomto zobrazení můžete zobrazit informace o topologii nebo dvakrát klikněte na položku (například SqlAzureBolt) Chcete-li zobrazit informace specifické pro komponentu v topologii.</span><span class="sxs-lookup"><span data-stu-id="a1c76-162">You can use this view to see information on the topology, or double-click an entry (such as the SqlAzureBolt) to see information specific to a component in the topology.</span></span>

3. <span data-ttu-id="a1c76-163">Poté, co má topologii spustili několik minut, vraťte se do okna dotazu SQL, které jste použili k vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="a1c76-163">After the topology has ran for a few minutes, return to the SQL query window you used to create the database.</span></span> <span data-ttu-id="a1c76-164">Nahraďte existující příkazy následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="a1c76-164">Replace the existing statements with the following query:</span></span>

        select * from iislogs;

    <span data-ttu-id="a1c76-165">Pomocí kombinace kláves Ctrl + Shift + E spustit dotaz a by měl zobrazit výsledky podobná následující data:</span><span class="sxs-lookup"><span data-stu-id="a1c76-165">Use Ctrl + Shift + E to execute the query, and you should receive results similar to the following data:</span></span>

        1    2016-05-27 17:57:14.797    255.255.255.255    /bar    GET    200
        2    2016-05-27 17:57:14.843    127.0.0.1    /spam/eggs    POST    500
        3    2016-05-27 17:57:14.850    123.123.123.123    /eggs    DELETE    200
        4    2016-05-27 17:57:14.853    127.0.0.1    /foo    POST    404
        5    2016-05-27 17:57:14.853    10.9.8.7    /bar    GET    200
        6    2016-05-27 17:57:14.857    192.168.1.1    /spam    DELETE    200

    <span data-ttu-id="a1c76-166">Tato data byla zapsána od topologie Storm.</span><span class="sxs-lookup"><span data-stu-id="a1c76-166">This data has been written from the Storm topology.</span></span>

## <a name="create-a-report"></a><span data-ttu-id="a1c76-167">Vytvoření sestavy</span><span class="sxs-lookup"><span data-stu-id="a1c76-167">Create a report</span></span>

1. <span data-ttu-id="a1c76-168">Připojení k [konektor Azure SQL Database](https://app.powerbi.com/getdata/bigdata/azure-sql-database-with-live-connect) pro Power BI.</span><span class="sxs-lookup"><span data-stu-id="a1c76-168">Connect to the [Azure SQL Database connector](https://app.powerbi.com/getdata/bigdata/azure-sql-database-with-live-connect) for Power BI.</span></span> 

2. <span data-ttu-id="a1c76-169">V rámci **databáze**, vyberte **získat**.</span><span class="sxs-lookup"><span data-stu-id="a1c76-169">Within **Databases**, select **Get**.</span></span>

3. <span data-ttu-id="a1c76-170">Vyberte **Azure SQL Database**a potom vyberte **Connect**.</span><span class="sxs-lookup"><span data-stu-id="a1c76-170">Select **Azure SQL Database**, and then select **Connect**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a1c76-171">Můžete být vyzváni, abyste stáhnout Power BI Desktop pokračujte.</span><span class="sxs-lookup"><span data-stu-id="a1c76-171">You may be asked to download the Power BI Desktop to continue.</span></span> <span data-ttu-id="a1c76-172">Pokud ano, pro připojení použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a1c76-172">If so, use the following steps to connect:</span></span>
    >
    > 1. <span data-ttu-id="a1c76-173">Otevřít Power BI Desktop a vyberte __načíst Data__.</span><span class="sxs-lookup"><span data-stu-id="a1c76-173">Open Power BI Desktop and select __Get Data__.</span></span>
    > <span data-ttu-id="a1c76-174">Vyberte 2 __Azure__a potom __Azure SQL database__.</span><span class="sxs-lookup"><span data-stu-id="a1c76-174">2  Select __Azure__, and then __Azure SQL database__.</span></span>

4. <span data-ttu-id="a1c76-175">Zadejte informace pro připojení k vaší databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="a1c76-175">Enter the information to connect to your Azure SQL Database.</span></span> <span data-ttu-id="a1c76-176">Tyto informace můžete najít navštivte stránky [portál Azure](https://portal.azure.com) a vyberete databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="a1c76-176">You can find this information by visiting the [Azure portal](https://portal.azure.com) and selecting your SQL database.</span></span>

   > [!NOTE]
   > <span data-ttu-id="a1c76-177">Interval obnovování a vlastní filtry lze také nastavit pomocí **povolit rozšířené možnosti** z dialogového okna připojení.</span><span class="sxs-lookup"><span data-stu-id="a1c76-177">You can also set the refresh interval and custom filters by using **Enable Advanced Options** from the connect dialog.</span></span>

5. <span data-ttu-id="a1c76-178">Po připojení, zobrazí se nová datová sada se stejným názvem jako databázi, ke kterému jste připojení.</span><span class="sxs-lookup"><span data-stu-id="a1c76-178">After you've connected, you will see a new dataset with the same name as the database you connected to.</span></span> <span data-ttu-id="a1c76-179">Vyberte datovou sadu zahájíte navrhování sestavy.</span><span class="sxs-lookup"><span data-stu-id="a1c76-179">Select the dataset to begin designing a report.</span></span>

6. <span data-ttu-id="a1c76-180">Z **pole**, rozbalte **IISLOGS** položku.</span><span class="sxs-lookup"><span data-stu-id="a1c76-180">From **Fields**, expand the **IISLOGS** entry.</span></span> <span data-ttu-id="a1c76-181">Pokud chcete vytvořit sestavu obsahující seznam stonky identifikátor URI, zaškrtněte políčko **modul URISTEM**.</span><span class="sxs-lookup"><span data-stu-id="a1c76-181">To create a report that lists the URI stems, select the checkbox for **URISTEM**.</span></span>

    ![Vytvoření sestavy](./media/hdinsight-storm-power-bi-topology/createreport.png)

7. <span data-ttu-id="a1c76-183">V dalším kroku přetáhněte **metoda** do sestavy.</span><span class="sxs-lookup"><span data-stu-id="a1c76-183">Next, drag **METHOD** to the report.</span></span> <span data-ttu-id="a1c76-184">Sestavy aktualizací k zobrazení seznamu stonky a odpovídající metodu HTTP použitou pro požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="a1c76-184">The report updates to list the stems and the corresponding HTTP method used for the HTTP request.</span></span>

    ![Přidání dat – metoda](./media/hdinsight-storm-power-bi-topology/uristemandmethod.png)

8. <span data-ttu-id="a1c76-186">Z **vizualizace** sloupce, vyberte **pole** ikonu a potom vyberte na šipku dolů vedle **metoda** v **hodnoty** části.</span><span class="sxs-lookup"><span data-stu-id="a1c76-186">From the **Visualizations** column, select the **Fields** icon, and then select the down arrow next to **METHOD** in the **Values** section.</span></span> <span data-ttu-id="a1c76-187">Chcete-li zobrazit počet kolikrát přistupoval identifikátor URI, vyberte **počet**.</span><span class="sxs-lookup"><span data-stu-id="a1c76-187">To display a count of how many times a URI has been accessed, select **Count**.</span></span>

    ![Změna na počet metody](./media/hdinsight-storm-power-bi-topology/count.png)

9. <span data-ttu-id="a1c76-189">Potom vyberte **skládaný sloupcový graf** změnit, jak se zobrazují informace.</span><span class="sxs-lookup"><span data-stu-id="a1c76-189">Next, select the **Stacked column chart** to change how the information is displayed.</span></span>

    ![Změna na skládaný graf](./media/hdinsight-storm-power-bi-topology/stackedcolumn.png)

10. <span data-ttu-id="a1c76-191">Chcete-li uložit sestavu, vyberte **Uložit** a zadejte název pro sestavu.</span><span class="sxs-lookup"><span data-stu-id="a1c76-191">To save the report, select **Save** and enter a name for the report.</span></span>

## <a name="stop-the-topology"></a><span data-ttu-id="a1c76-192">Zastavení topologie</span><span class="sxs-lookup"><span data-stu-id="a1c76-192">Stop the topology</span></span>

<span data-ttu-id="a1c76-193">Topologie i nadále spustit, dokud nejde zastavit ani odstranit Storm v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a1c76-193">The topology continues to run until you stop it or delete the Storm on HDInsight cluster.</span></span> <span data-ttu-id="a1c76-194">K zastavení topologie, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a1c76-194">To stop the topology, perform the following steps:</span></span>

1. <span data-ttu-id="a1c76-195">V sadě Visual Studio vraťte do prohlížeče topologie a vyberte topologii.</span><span class="sxs-lookup"><span data-stu-id="a1c76-195">In Visual Studio, return to the topology viewer and select the topology.</span></span>

2. <span data-ttu-id="a1c76-196">Vyberte **Kill** tlačítko zastavení topologie.</span><span class="sxs-lookup"><span data-stu-id="a1c76-196">Select the **Kill** button to stop the topology.</span></span>

    ![Příkaz kill tlačítko na topologii souhrn](./media/hdinsight-storm-power-bi-topology/killtopology.png)

## <a name="delete-your-cluster"></a><span data-ttu-id="a1c76-198">Odstranění clusteru</span><span class="sxs-lookup"><span data-stu-id="a1c76-198">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="a1c76-199">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a1c76-199">Next steps</span></span>

<span data-ttu-id="a1c76-200">V tomto dokumentu jste zjistili, jak odesílat data do databáze SQL z topologie Storm a pak vizualizovat data pomocí Power BI.</span><span class="sxs-lookup"><span data-stu-id="a1c76-200">In this document, you learned how to send data from a Storm topology to SQL Database, then visualize the data using Power BI.</span></span> <span data-ttu-id="a1c76-201">Informace o tom, jak pracovat s jinými Azure technologiemi používat Storm v HDInsight najdete v následujícím dokumentu:</span><span class="sxs-lookup"><span data-stu-id="a1c76-201">For information on how to work with other Azure technologies using Storm on HDInsight, see the following document:</span></span>

* [<span data-ttu-id="a1c76-202">Příklad topologií pro Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a1c76-202">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)
