---
title: "Připojení aplikace Excel k systému Hadoop pomocí ovladače ODBC Hive - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak nastavit a používat ovladač Microsoft Hive ODBC pro aplikaci Excel k dotazování na data v clusterech HDInsight z aplikace Microsoft Excel."
keywords: "v aplikaci excel hadoop, hive v aplikaci excel, hive rozhraní odbc"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
tags: azure-portal
editor: cgronlun
ms.assetid: a7665a14-0211-4521-b3e7-3b26e8029cc0
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/22/2017
ms.author: jgao
ms.openlocfilehash: 7818093e42c34ee671a035cde783a6622fb2a798
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="connect-excel-to-hadoop-in-azure-hdinsight-with-the-microsoft-hive-odbc-driver"></a><span data-ttu-id="2018d-104">Připojení aplikace Excel k systému Hadoop v prostředí Azure HDInsight pomocí ovladače Microsoft Hive ODBC</span><span class="sxs-lookup"><span data-stu-id="2018d-104">Connect Excel to Hadoop in Azure HDInsight with the Microsoft Hive ODBC driver</span></span>

[!INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

<span data-ttu-id="2018d-105">Řešení velkých objemů dat společnosti Microsoft se integruje s clustery Apache Hadoop, které jsou nasazené v Azure Hdinsightu součásti Microsoft Business Intelligence (BI).</span><span class="sxs-lookup"><span data-stu-id="2018d-105">Microsoft's Big Data solution integrates Microsoft Business Intelligence (BI) components with Apache Hadoop clusters that have been deployed by the Azure HDInsight.</span></span> <span data-ttu-id="2018d-106">Příklad této integrace je schopnost připojení aplikace Excel do datového skladu Hive clusteru Hadoop v HDInsight pomocí ovladače Microsoft Hive připojení ODBC (Open Database).</span><span class="sxs-lookup"><span data-stu-id="2018d-106">An example of this integration is the ability to connect Excel to the Hive data warehouse of a Hadoop cluster in HDInsight using the Microsoft Hive Open Database Connectivity (ODBC) Driver.</span></span>

<span data-ttu-id="2018d-107">Také je možné připojit data související s clusteru služby HDInsight a dalších zdrojů dat, včetně jiných clusterů systému Hadoop (bez HDInsight), z aplikace Excel pomocí doplňku Microsoft Power Query pro Excel.</span><span class="sxs-lookup"><span data-stu-id="2018d-107">It is also possible to connect the data associated with an HDInsight cluster and other data sources, including other (non-HDInsight) Hadoop clusters, from Excel using the Microsoft Power Query add-in for Excel.</span></span> <span data-ttu-id="2018d-108">Informace o instalaci a používání doplňku Power Query najdete v tématu [připojení aplikace Excel do prostředí HDInsight pomocí doplňku Power Query][hdinsight-power-query].</span><span class="sxs-lookup"><span data-stu-id="2018d-108">For information on installing and using Power Query, see [Connect Excel to HDInsight with Power Query][hdinsight-power-query].</span></span>

> [!NOTE]
> <span data-ttu-id="2018d-109">Zatímco kroky v tomto článku lze použít s clusterem Linux nebo HDInsight se systémem Windows, je vyžadována pro klientské pracovní stanice systému Windows.</span><span class="sxs-lookup"><span data-stu-id="2018d-109">While the steps in this article can be used with either a Linux or Windows-based HDInsight cluster, Windows is required for the client workstation.</span></span>
> 
> 

<span data-ttu-id="2018d-110">**Požadavky**:</span><span class="sxs-lookup"><span data-stu-id="2018d-110">**Prerequisites**:</span></span>

<span data-ttu-id="2018d-111">Před zahájením tohoto článku, musíte mít následující položky:</span><span class="sxs-lookup"><span data-stu-id="2018d-111">Before you begin this article, you must have the following items:</span></span>

* <span data-ttu-id="2018d-112">**Cluster služby HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="2018d-112">**An HDInsight cluster**.</span></span> <span data-ttu-id="2018d-113">Chcete-li vytvořit, přečtěte si téma [Začínáme s Azure HDInsight][hdinsight-get-started].</span><span class="sxs-lookup"><span data-stu-id="2018d-113">To create one, see [Get started with Azure HDInsight][hdinsight-get-started].</span></span>
* <span data-ttu-id="2018d-114">**Pracovní stanice** Office 2013 Professional Plus, Office 365 Pro Plus, samostatné aplikace Excel 2013 nebo Office 2010 Professional Plus.</span><span class="sxs-lookup"><span data-stu-id="2018d-114">**A workstation** with Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 Standalone, or Office 2010 Professional Plus.</span></span>

## <a name="install-microsoft-hive-odbc-driver"></a><span data-ttu-id="2018d-115">Instalaci ovladače Microsoft Hive ODBC</span><span class="sxs-lookup"><span data-stu-id="2018d-115">Install Microsoft Hive ODBC driver</span></span>
<span data-ttu-id="2018d-116">Stáhnout a nainstalovat ovladače ODBC Microsoft Hive z [Download Center][hive-odbc-driver-download].</span><span class="sxs-lookup"><span data-stu-id="2018d-116">Download and install Microsoft Hive ODBC Driver from the [Download Center][hive-odbc-driver-download].</span></span>

<span data-ttu-id="2018d-117">Tento ovladač lze nainstalovat na 32bitové nebo 64bitové verze Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 a Windows Server 2012.</span><span class="sxs-lookup"><span data-stu-id="2018d-117">This driver can be installed on 32-bit or 64-bit versions of Windows 7, Windows 8, Windows 10, Windows Server 2008 R2, and Windows Server 2012.</span></span> <span data-ttu-id="2018d-118">Ovladač umožňuje připojení k Azure HDInsight (verze 1.6 nebo novější) a emulátoru HDInsight Azure (v.1.0.0.0 a novější).</span><span class="sxs-lookup"><span data-stu-id="2018d-118">The driver allows connection to Azure HDInsight (version 1.6 and later) and Azure HDInsight Emulator (v.1.0.0.0 and later).</span></span> <span data-ttu-id="2018d-119">Nainstalujete verzi, která odpovídá verzi aplikace, kde používáte ovladač ODBC.</span><span class="sxs-lookup"><span data-stu-id="2018d-119">You shall install the version that matches the version of the application where you use the ODBC driver.</span></span> <span data-ttu-id="2018d-120">V tomto kurzu se používá ovladače z aplikace Office Excel.</span><span class="sxs-lookup"><span data-stu-id="2018d-120">For this tutorial, the driver is used from Office Excel.</span></span>

## <a name="create-hive-odbc-data-source"></a><span data-ttu-id="2018d-121">Vytvoření zdroje dat Hive ODBC</span><span class="sxs-lookup"><span data-stu-id="2018d-121">Create Hive ODBC data source</span></span>
<span data-ttu-id="2018d-122">Následující kroky ukazují, jak vytvořit zdroj dat ODBC Hive.</span><span class="sxs-lookup"><span data-stu-id="2018d-122">The following steps show you how to create a Hive ODBC Data Source.</span></span>

1. <span data-ttu-id="2018d-123">Ze systému Windows 8 nebo Windows 10, stiskněte klávesu Windows k otevření obrazovky Start a potom zadejte **zdroje dat**.</span><span class="sxs-lookup"><span data-stu-id="2018d-123">From Windows 8 or Windows 10, press the Windows key to open the Start screen, and then type **data sources**.</span></span>
2. <span data-ttu-id="2018d-124">Klikněte na tlačítko **nastavení zdroje dat ODBC (32 bitů)** nebo **nastavení zdroje dat ODBC (64 bitů)** v závislosti na vaší verzi Office.</span><span class="sxs-lookup"><span data-stu-id="2018d-124">Click **Set up ODBC Data sources (32-bit)** or **Set up ODBC Data Sources (64-bit)** depending on your Office version.</span></span> <span data-ttu-id="2018d-125">Pokud používáte systém Windows 7, vyberte **zdroje dat ODBC (32 bitů)** nebo **zdroje dat ODBC (64 bitů)** z **nástroje pro správu**.</span><span class="sxs-lookup"><span data-stu-id="2018d-125">If you are using Windows 7, choose **ODBC Data Sources (32 bit)** or **ODBC Data Sources (64 bit)** from **Administrative Tools**.</span></span> <span data-ttu-id="2018d-126">Zobrazí se **správce zdrojů dat ODBC** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2018d-126">You shall see the **ODBC Data Source Administrator** dialog.</span></span>
   
    <span data-ttu-id="2018d-127">![Správce zdrojů dat ODBC](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png "konfigurace názvu DSN pomocí Správce zdrojů dat ODBC")</span><span class="sxs-lookup"><span data-stu-id="2018d-127">![OBDC data source administrator](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png "Configure a DSN using ODBC Data Source Administrator")</span></span>

3. <span data-ttu-id="2018d-128">Z DNS uživatele, klikněte na tlačítko **přidat** otevřete **vytvořit nový zdroj dat** průvodce.</span><span class="sxs-lookup"><span data-stu-id="2018d-128">From User DNS, click **Add** to open the **Create New Data Source** wizard.</span></span>
4. <span data-ttu-id="2018d-129">Vyberte **ovladače ODBC Microsoft Hive**a potom klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="2018d-129">Select **Microsoft Hive ODBC Driver**, and then click **Finish**.</span></span> <span data-ttu-id="2018d-130">Zobrazí se **Microsoft DNS ovladače ODBC Hive instalace** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2018d-130">You shall see the **Microsoft Hive ODBC Driver DNS Setup** dialog.</span></span>
5. <span data-ttu-id="2018d-131">Zadejte nebo vyberte tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="2018d-131">Type or select the following values:</span></span>
   
   | <span data-ttu-id="2018d-132">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="2018d-132">Property</span></span> | <span data-ttu-id="2018d-133">Popis</span><span class="sxs-lookup"><span data-stu-id="2018d-133">Description</span></span> |
   | --- | --- |
   |  <span data-ttu-id="2018d-134">Název zdroje dat</span><span class="sxs-lookup"><span data-stu-id="2018d-134">Data Source Name</span></span> |<span data-ttu-id="2018d-135">Zadejte název zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="2018d-135">Give a name to your data source</span></span> |
   |  <span data-ttu-id="2018d-136">Hostitel</span><span class="sxs-lookup"><span data-stu-id="2018d-136">Host</span></span> |<span data-ttu-id="2018d-137">Zadejte &lt;název_clusteru_HDInsight>.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="2018d-137">Enter &lt;HDInsightClusterName>.azurehdinsight.net.</span></span> <span data-ttu-id="2018d-138">Například mujHDICluster.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="2018d-138">For example, myHDICluster.azurehdinsight.net</span></span> |
   |  <span data-ttu-id="2018d-139">Port</span><span class="sxs-lookup"><span data-stu-id="2018d-139">Port</span></span> |<span data-ttu-id="2018d-140">Použijte <strong>443</strong>.</span><span class="sxs-lookup"><span data-stu-id="2018d-140">Use <strong>443</strong>.</span></span> <span data-ttu-id="2018d-141">(Tento port se změnil z 563 na 443.)</span><span class="sxs-lookup"><span data-stu-id="2018d-141">(This port has been changed from 563 to 443.)</span></span> |
   |  <span data-ttu-id="2018d-142">Databáze</span><span class="sxs-lookup"><span data-stu-id="2018d-142">Database</span></span> |<span data-ttu-id="2018d-143">Použijte <strong>Výchozí</strong>.</span><span class="sxs-lookup"><span data-stu-id="2018d-143">Use <strong>Default</strong>.</span></span> |
   |  <span data-ttu-id="2018d-144">Mechanismus</span><span class="sxs-lookup"><span data-stu-id="2018d-144">Mechanism</span></span> |<span data-ttu-id="2018d-145">Vyberte <strong>Služba Azure HDInsight</strong></span><span class="sxs-lookup"><span data-stu-id="2018d-145">Select <strong>Azure HDInsight Service</strong></span></span> |
   |  <span data-ttu-id="2018d-146">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="2018d-146">User Name</span></span> |<span data-ttu-id="2018d-147">Zadejte uživatelské jméno uživatele ve HTTP clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="2018d-147">Enter HDInsight cluster HTTP user username.</span></span> <span data-ttu-id="2018d-148">Výchozí uživatelské jméno <strong>admin</strong>.</span><span class="sxs-lookup"><span data-stu-id="2018d-148">The default username is <strong>admin</strong>.</span></span> |
   |  <span data-ttu-id="2018d-149">Heslo</span><span class="sxs-lookup"><span data-stu-id="2018d-149">Password</span></span> |<span data-ttu-id="2018d-150">Zadejte heslo uživatele clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="2018d-150">Enter HDInsight cluster user password.</span></span> |
   
    </table>
   
    <span data-ttu-id="2018d-151">Existují některé důležité parametry zajímat když kliknete na tlačítko **pokročilé možnosti**:</span><span class="sxs-lookup"><span data-stu-id="2018d-151">There are some important parameters to be aware of when you click **Advanced Options**:</span></span>
   
   | <span data-ttu-id="2018d-152">Parametr</span><span class="sxs-lookup"><span data-stu-id="2018d-152">Parameter</span></span> | <span data-ttu-id="2018d-153">Popis</span><span class="sxs-lookup"><span data-stu-id="2018d-153">Description</span></span> |
   | --- | --- |
   |  <span data-ttu-id="2018d-154">Použít nativní dotaz</span><span class="sxs-lookup"><span data-stu-id="2018d-154">Use Native Query</span></span> |<span data-ttu-id="2018d-155">Pokud je zaškrtnuto, ovladač ODBC není pokusí převést TSQL HiveQL.</span><span class="sxs-lookup"><span data-stu-id="2018d-155">When it is selected, the ODBC driver does NOT try to convert TSQL into HiveQL.</span></span> <span data-ttu-id="2018d-156">Musí se použít pouze v případě, že jste se, že jste odeslali čistý příkazy HiveQL 100 %.</span><span class="sxs-lookup"><span data-stu-id="2018d-156">You shall use it only if you are 100% sure you are submitting pure HiveQL statements.</span></span> <span data-ttu-id="2018d-157">Při připojování k systému SQL Server nebo Azure SQL Database, by měl nechte nezaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="2018d-157">When connecting to SQL Server or Azure SQL Database, you should leave it unchecked.</span></span> |
   |  <span data-ttu-id="2018d-158">Počet získaných za blok řádků</span><span class="sxs-lookup"><span data-stu-id="2018d-158">Rows fetched per block</span></span> |<span data-ttu-id="2018d-159">Při načítání velkému počtu záznamů, optimalizace pro tento parametr může být nutné zajistit optimální přínos.</span><span class="sxs-lookup"><span data-stu-id="2018d-159">When fetching a large number of records, tuning this parameter may be required to ensure optimal performances.</span></span> |
   |  <span data-ttu-id="2018d-160">Výchozí délka sloupce řetězec, délka binárního sloupce, měřítko Decimal sloupce</span><span class="sxs-lookup"><span data-stu-id="2018d-160">Default string column length, Binary column length, Decimal column scale</span></span> |<span data-ttu-id="2018d-161">Datový typ délky a přesnosti může mít vliv na tom, jak je vrácená data.</span><span class="sxs-lookup"><span data-stu-id="2018d-161">The data type lengths and precisions may affect how data is returned.</span></span> <span data-ttu-id="2018d-162">Způsobí nesprávné informace o který se má vrátit z důvodu ztráty přesnost nebo zkrácení.</span><span class="sxs-lookup"><span data-stu-id="2018d-162">They cause incorrect information to be returned due to loss of precision and/or truncation.</span></span> |

    <span data-ttu-id="2018d-163">![Rozšířené možnosti](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png "DSN rozšířené možnosti konfigurace")</span><span class="sxs-lookup"><span data-stu-id="2018d-163">![Advanced options](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png "Advanced DSN configuration options")</span></span>

1. <span data-ttu-id="2018d-164">Klikněte na tlačítko **testování** zdroj dat otestovat.</span><span class="sxs-lookup"><span data-stu-id="2018d-164">Click **Test** to test the data source.</span></span> <span data-ttu-id="2018d-165">Zdroj dat správně nakonfigurovaný, zobrazuje *testy byla úspěšně DOKONČENA.!*.</span><span class="sxs-lookup"><span data-stu-id="2018d-165">When the data source is configured correctly, it shows *TESTS COMPLETED SUCCESSFULLY!*.</span></span>
2. <span data-ttu-id="2018d-166">Klikněte na tlačítko **OK** zavřete dialogové okno Test.</span><span class="sxs-lookup"><span data-stu-id="2018d-166">Click **OK** to close the Test dialog.</span></span> <span data-ttu-id="2018d-167">Nový zdroj dat je uvedeno v **správce zdrojů dat ODBC**.</span><span class="sxs-lookup"><span data-stu-id="2018d-167">The new data source shall be listed on the **ODBC Data Source Administrator**.</span></span>
3. <span data-ttu-id="2018d-168">Klikněte na tlačítko **OK** ukončíte průvodce.</span><span class="sxs-lookup"><span data-stu-id="2018d-168">Click **OK** to exit the wizard.</span></span>

## <a name="import-data-into-excel-from-hdinsight"></a><span data-ttu-id="2018d-169">Import dat do Excelu ze služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="2018d-169">Import data into Excel from HDInsight</span></span>
<span data-ttu-id="2018d-170">Následující kroky popisují způsob, jak můžete importovat data z tabulky Hive do sešitu aplikace Excel pomocí zdroje dat ODBC, kterou jste vytvořili v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="2018d-170">The following steps describe the way to import data from a Hive table into an Excel workbook using the ODBC data source that you created in the previous section.</span></span>

1. <span data-ttu-id="2018d-171">V Excelu otevřete nový nebo existující sešit.</span><span class="sxs-lookup"><span data-stu-id="2018d-171">Open a new or existing workbook in Excel.</span></span>
2. <span data-ttu-id="2018d-172">Z **Data** , klikněte na **načíst Data**, klikněte na tlačítko **z jiných zdrojů**a potom klikněte na **z rozhraní ODBC** ke spuštění **dat Průvodce připojením**.</span><span class="sxs-lookup"><span data-stu-id="2018d-172">From the **Data** tab, click **Get Data**, click **From Other Sources**, and then click **From ODBC** to launch the **Data Connection Wizard**.</span></span>
   
    <span data-ttu-id="2018d-173">![Průvodce připojením Open data](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png "Open data Průvodce připojením")</span><span class="sxs-lookup"><span data-stu-id="2018d-173">![Open data connection wizard](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png "Open data connection wizard")</span></span>
4. <span data-ttu-id="2018d-174">Vyberte název zdroje dat, který jste vytvořili v poslední části a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="2018d-174">Select the data source name that you created in the last section, and then click **OK**.</span></span>
5. <span data-ttu-id="2018d-175">Zadejte uživatelské jméno Hadoop (výchozí název je správce) a heslo a pak klikněte na tlačítko **Connect**.</span><span class="sxs-lookup"><span data-stu-id="2018d-175">Enter Hadoop user name (the default name is admin) and the password, and then click **Connect**.</span></span>
6. <span data-ttu-id="2018d-176">Navigátor, rozbalte **HIVE**, rozbalte položku **výchozí**, klikněte na tlačítko **hivesampletable**a potom klikněte na **zatížení**.</span><span class="sxs-lookup"><span data-stu-id="2018d-176">On Navigator, expand **HIVE**, expand **default**, click **hivesampletable**, and then click **Load**.</span></span> <span data-ttu-id="2018d-177">Import dat do Excelu trvá několik sekund.</span><span class="sxs-lookup"><span data-stu-id="2018d-177">It takes a few seconds before data gets imported to Excel.</span></span>

    <span data-ttu-id="2018d-178">![Navigátor HDInsight Hive ODBC](./media/hdinsight-connect-excel-hive-ODBC-driver/hdinsight.hive.odbc.navigator.png "Open data Průvodce připojením")</span><span class="sxs-lookup"><span data-stu-id="2018d-178">![HDInsight Hive ODBC navigator](./media/hdinsight-connect-excel-hive-ODBC-driver/hdinsight.hive.odbc.navigator.png "Open data connection wizard")</span></span>


## <a name="next-steps"></a><span data-ttu-id="2018d-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2018d-179">Next steps</span></span>
<span data-ttu-id="2018d-180">V tomto článku jste zjistili, jak používat ovladač Microsoft Hive ODBC k načtení dat ze služby HDInsight do aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="2018d-180">In this article, you learned how to use the Microsoft Hive ODBC driver to retrieve data from the HDInsight Service into Excel.</span></span> <span data-ttu-id="2018d-181">Podobně můžete načíst data ze služby HDInsight do databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="2018d-181">Similarly, you can retrieve data from the HDInsight Service into SQL Database.</span></span> <span data-ttu-id="2018d-182">Je také možné nahrát data do služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="2018d-182">It is also possible to upload data into an HDInsight Service.</span></span> <span data-ttu-id="2018d-183">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="2018d-183">To learn more, see:</span></span>

* <span data-ttu-id="2018d-184">[Analýza dat zpoždění letu pomocí HDInsight][hdinsight-analyze-flight-data]</span><span class="sxs-lookup"><span data-stu-id="2018d-184">[Analyze flight delay data using HDInsight][hdinsight-analyze-flight-data]</span></span>
* <span data-ttu-id="2018d-185">[Nahrání dat do HDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="2018d-185">[Upload Data to HDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="2018d-186">[Použití nástroje Sqoop s HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="2018d-186">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698


