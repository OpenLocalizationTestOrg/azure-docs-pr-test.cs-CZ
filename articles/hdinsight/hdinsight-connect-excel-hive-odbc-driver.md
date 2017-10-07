---
title: "aaaConnect tooHadoop Excel s hello ovladače ODBC Hive - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak tooset nahoru a používání hello ovladače Microsoft Hive ODBC pro aplikaci Excel tooquery data v clusterech HDInsight z aplikace Microsoft Excel."
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
ms.openlocfilehash: f01f89e7d4203c739d56079dc589fc11f4aa2174
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-excel-toohadoop-in-azure-hdinsight-with-hello-microsoft-hive-odbc-driver"></a><span data-ttu-id="3ed50-104">Připojení aplikace Excel tooHadoop v Azure HDInsight pomocí ovladače Microsoft Hive ODBC hello</span><span class="sxs-lookup"><span data-stu-id="3ed50-104">Connect Excel tooHadoop in Azure HDInsight with hello Microsoft Hive ODBC driver</span></span>

[!INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

<span data-ttu-id="3ed50-105">Řešení velkých objemů dat společnosti Microsoft se integruje s clustery systému Apache Hadoop nasazených hello Azure HDInsight součásti Microsoft Business Intelligence (BI).</span><span class="sxs-lookup"><span data-stu-id="3ed50-105">Microsoft's Big Data solution integrates Microsoft Business Intelligence (BI) components with Apache Hadoop clusters that have been deployed by hello Azure HDInsight.</span></span> <span data-ttu-id="3ed50-106">Příklad této integrace je hello možnost tooconnect Excel toohello Hive datový sklad clusteru Hadoop v HDInsight pomocí hello ovladač Microsoft Hive připojení ODBC (Open Database).</span><span class="sxs-lookup"><span data-stu-id="3ed50-106">An example of this integration is hello ability tooconnect Excel toohello Hive data warehouse of a Hadoop cluster in HDInsight using hello Microsoft Hive Open Database Connectivity (ODBC) Driver.</span></span>

<span data-ttu-id="3ed50-107">Je také možné tooconnect hello data přidružená k clusteru služby HDInsight a jiných zdrojů dat, včetně dalších (bez HDInsight) clusterů systému Hadoop, z Excelu pomocí hello doplňku Microsoft Power Query pro Excel.</span><span class="sxs-lookup"><span data-stu-id="3ed50-107">It is also possible tooconnect hello data associated with an HDInsight cluster and other data sources, including other (non-HDInsight) Hadoop clusters, from Excel using hello Microsoft Power Query add-in for Excel.</span></span> <span data-ttu-id="3ed50-108">Informace o instalaci a používání doplňku Power Query najdete v tématu [tooHDInsight připojení aplikace Excel pomocí doplňku Power Query][hdinsight-power-query].</span><span class="sxs-lookup"><span data-stu-id="3ed50-108">For information on installing and using Power Query, see [Connect Excel tooHDInsight with Power Query][hdinsight-power-query].</span></span>

> [!NOTE]
> <span data-ttu-id="3ed50-109">Během hello kroky tomto článku lze použít s buď Linux nebo clusteru HDInsight se systémem Windows, Windows se vyžaduje pro hello klientské pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="3ed50-109">While hello steps in this article can be used with either a Linux or Windows-based HDInsight cluster, Windows is required for hello client workstation.</span></span>
> 
> 

<span data-ttu-id="3ed50-110">**Požadavky**:</span><span class="sxs-lookup"><span data-stu-id="3ed50-110">**Prerequisites**:</span></span>

<span data-ttu-id="3ed50-111">Před zahájením tohoto článku, musíte mít hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="3ed50-111">Before you begin this article, you must have hello following items:</span></span>

* <span data-ttu-id="3ed50-112">**Cluster služby HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="3ed50-112">**An HDInsight cluster**.</span></span> <span data-ttu-id="3ed50-113">toocreate, najdete v části [Začínáme s Azure HDInsight][hdinsight-get-started].</span><span class="sxs-lookup"><span data-stu-id="3ed50-113">toocreate one, see [Get started with Azure HDInsight][hdinsight-get-started].</span></span>
* <span data-ttu-id="3ed50-114">**Pracovní stanice** Office 2013 Professional Plus, Office 365 Pro Plus, samostatné aplikace Excel 2013 nebo Office 2010 Professional Plus.</span><span class="sxs-lookup"><span data-stu-id="3ed50-114">**A workstation** with Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 Standalone, or Office 2010 Professional Plus.</span></span>

## <a name="install-microsoft-hive-odbc-driver"></a><span data-ttu-id="3ed50-115">Instalaci ovladače Microsoft Hive ODBC</span><span class="sxs-lookup"><span data-stu-id="3ed50-115">Install Microsoft Hive ODBC driver</span></span>
<span data-ttu-id="3ed50-116">Stáhnout a nainstalovat ovladače ODBC Microsoft Hive ze hello [Download Center][hive-odbc-driver-download].</span><span class="sxs-lookup"><span data-stu-id="3ed50-116">Download and install Microsoft Hive ODBC Driver from hello [Download Center][hive-odbc-driver-download].</span></span>

<span data-ttu-id="3ed50-117">Tento ovladač lze nainstalovat na 32bitové nebo 64bitové verze Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 a Windows Server 2012.</span><span class="sxs-lookup"><span data-stu-id="3ed50-117">This driver can be installed on 32-bit or 64-bit versions of Windows 7, Windows 8, Windows 10, Windows Server 2008 R2, and Windows Server 2012.</span></span> <span data-ttu-id="3ed50-118">ovladač Hello umožňuje připojení tooAzure HDInsight (verze 1.6 nebo novější) a emulátoru HDInsight Azure (v.1.0.0.0 a novější).</span><span class="sxs-lookup"><span data-stu-id="3ed50-118">hello driver allows connection tooAzure HDInsight (version 1.6 and later) and Azure HDInsight Emulator (v.1.0.0.0 and later).</span></span> <span data-ttu-id="3ed50-119">Nainstalujete hello verze, která odpovídá hello verze aplikace hello kde pomocí ovladače ODBC hello.</span><span class="sxs-lookup"><span data-stu-id="3ed50-119">You shall install hello version that matches hello version of hello application where you use hello ODBC driver.</span></span> <span data-ttu-id="3ed50-120">V tomto kurzu se používá hello ovladače z aplikace Office Excel.</span><span class="sxs-lookup"><span data-stu-id="3ed50-120">For this tutorial, hello driver is used from Office Excel.</span></span>

## <a name="create-hive-odbc-data-source"></a><span data-ttu-id="3ed50-121">Vytvoření zdroje dat Hive ODBC</span><span class="sxs-lookup"><span data-stu-id="3ed50-121">Create Hive ODBC data source</span></span>
<span data-ttu-id="3ed50-122">Hello následující kroky ukazují, jak toocreate Hive zdroje dat ODBC.</span><span class="sxs-lookup"><span data-stu-id="3ed50-122">hello following steps show you how toocreate a Hive ODBC Data Source.</span></span>

1. <span data-ttu-id="3ed50-123">Ze systému Windows 8 nebo Windows 10, stiskněte hello Windows klíče tooopen hello úvodní obrazovce a potom zadejte **zdroje dat**.</span><span class="sxs-lookup"><span data-stu-id="3ed50-123">From Windows 8 or Windows 10, press hello Windows key tooopen hello Start screen, and then type **data sources**.</span></span>
2. <span data-ttu-id="3ed50-124">Klikněte na tlačítko **nastavení zdroje dat ODBC (32 bitů)** nebo **nastavení zdroje dat ODBC (64 bitů)** v závislosti na vaší verzi Office.</span><span class="sxs-lookup"><span data-stu-id="3ed50-124">Click **Set up ODBC Data sources (32-bit)** or **Set up ODBC Data Sources (64-bit)** depending on your Office version.</span></span> <span data-ttu-id="3ed50-125">Pokud používáte systém Windows 7, vyberte **zdroje dat ODBC (32 bitů)** nebo **zdroje dat ODBC (64 bitů)** z **nástroje pro správu**.</span><span class="sxs-lookup"><span data-stu-id="3ed50-125">If you are using Windows 7, choose **ODBC Data Sources (32 bit)** or **ODBC Data Sources (64 bit)** from **Administrative Tools**.</span></span> <span data-ttu-id="3ed50-126">Zobrazí se hello **správce zdrojů dat ODBC** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3ed50-126">You shall see hello **ODBC Data Source Administrator** dialog.</span></span>
   
    <span data-ttu-id="3ed50-127">![Správce zdrojů dat ODBC](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png "konfigurace názvu DSN pomocí Správce zdrojů dat ODBC")</span><span class="sxs-lookup"><span data-stu-id="3ed50-127">![OBDC data source administrator](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png "Configure a DSN using ODBC Data Source Administrator")</span></span>

3. <span data-ttu-id="3ed50-128">Z DNS uživatele, klikněte na tlačítko **přidat** tooopen hello **vytvořit nový zdroj dat** průvodce.</span><span class="sxs-lookup"><span data-stu-id="3ed50-128">From User DNS, click **Add** tooopen hello **Create New Data Source** wizard.</span></span>
4. <span data-ttu-id="3ed50-129">Vyberte **ovladače ODBC Microsoft Hive**a potom klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="3ed50-129">Select **Microsoft Hive ODBC Driver**, and then click **Finish**.</span></span> <span data-ttu-id="3ed50-130">Zobrazí se hello **Microsoft DNS ovladače ODBC Hive instalace** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3ed50-130">You shall see hello **Microsoft Hive ODBC Driver DNS Setup** dialog.</span></span>
5. <span data-ttu-id="3ed50-131">Zadejte nebo vyberte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="3ed50-131">Type or select hello following values:</span></span>
   
   | <span data-ttu-id="3ed50-132">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3ed50-132">Property</span></span> | <span data-ttu-id="3ed50-133">Popis</span><span class="sxs-lookup"><span data-stu-id="3ed50-133">Description</span></span> |
   | --- | --- |
   |  <span data-ttu-id="3ed50-134">Název zdroje dat</span><span class="sxs-lookup"><span data-stu-id="3ed50-134">Data Source Name</span></span> |<span data-ttu-id="3ed50-135">Zadejte název zdroje dat tooyour</span><span class="sxs-lookup"><span data-stu-id="3ed50-135">Give a name tooyour data source</span></span> |
   |  <span data-ttu-id="3ed50-136">Hostitel</span><span class="sxs-lookup"><span data-stu-id="3ed50-136">Host</span></span> |<span data-ttu-id="3ed50-137">Zadejte &lt;název_clusteru_HDInsight>.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="3ed50-137">Enter &lt;HDInsightClusterName>.azurehdinsight.net.</span></span> <span data-ttu-id="3ed50-138">Například mujHDICluster.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="3ed50-138">For example, myHDICluster.azurehdinsight.net</span></span> |
   |  <span data-ttu-id="3ed50-139">Port</span><span class="sxs-lookup"><span data-stu-id="3ed50-139">Port</span></span> |<span data-ttu-id="3ed50-140">Použijte <strong>443</strong>.</span><span class="sxs-lookup"><span data-stu-id="3ed50-140">Use <strong>443</strong>.</span></span> <span data-ttu-id="3ed50-141">(Tento port se změnil z 563 too443.)</span><span class="sxs-lookup"><span data-stu-id="3ed50-141">(This port has been changed from 563 too443.)</span></span> |
   |  <span data-ttu-id="3ed50-142">Databáze</span><span class="sxs-lookup"><span data-stu-id="3ed50-142">Database</span></span> |<span data-ttu-id="3ed50-143">Použijte <strong>Výchozí</strong>.</span><span class="sxs-lookup"><span data-stu-id="3ed50-143">Use <strong>Default</strong>.</span></span> |
   |  <span data-ttu-id="3ed50-144">Mechanismus</span><span class="sxs-lookup"><span data-stu-id="3ed50-144">Mechanism</span></span> |<span data-ttu-id="3ed50-145">Vyberte <strong>Služba Azure HDInsight</strong></span><span class="sxs-lookup"><span data-stu-id="3ed50-145">Select <strong>Azure HDInsight Service</strong></span></span> |
   |  <span data-ttu-id="3ed50-146">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="3ed50-146">User Name</span></span> |<span data-ttu-id="3ed50-147">Zadejte uživatelské jméno uživatele ve HTTP clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3ed50-147">Enter HDInsight cluster HTTP user username.</span></span> <span data-ttu-id="3ed50-148">výchozí uživatelské jméno Hello <strong>správce</strong>.</span><span class="sxs-lookup"><span data-stu-id="3ed50-148">hello default username is <strong>admin</strong>.</span></span> |
   |  <span data-ttu-id="3ed50-149">Heslo</span><span class="sxs-lookup"><span data-stu-id="3ed50-149">Password</span></span> |<span data-ttu-id="3ed50-150">Zadejte heslo uživatele clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3ed50-150">Enter HDInsight cluster user password.</span></span> |
   
    </table>
   
    <span data-ttu-id="3ed50-151">Existují některé důležité parametry toobe vědět, když kliknete na tlačítko **pokročilé možnosti**:</span><span class="sxs-lookup"><span data-stu-id="3ed50-151">There are some important parameters toobe aware of when you click **Advanced Options**:</span></span>
   
   | <span data-ttu-id="3ed50-152">Parametr</span><span class="sxs-lookup"><span data-stu-id="3ed50-152">Parameter</span></span> | <span data-ttu-id="3ed50-153">Popis</span><span class="sxs-lookup"><span data-stu-id="3ed50-153">Description</span></span> |
   | --- | --- |
   |  <span data-ttu-id="3ed50-154">Použít nativní dotaz</span><span class="sxs-lookup"><span data-stu-id="3ed50-154">Use Native Query</span></span> |<span data-ttu-id="3ed50-155">Pokud je zaškrtnuto, ovladač ODBC hello nezkusí tooconvert TSQL do HiveQL.</span><span class="sxs-lookup"><span data-stu-id="3ed50-155">When it is selected, hello ODBC driver does NOT try tooconvert TSQL into HiveQL.</span></span> <span data-ttu-id="3ed50-156">Musí se použít pouze v případě, že jste se, že jste odeslali čistý příkazy HiveQL 100 %.</span><span class="sxs-lookup"><span data-stu-id="3ed50-156">You shall use it only if you are 100% sure you are submitting pure HiveQL statements.</span></span> <span data-ttu-id="3ed50-157">Při připojování tooSQL serveru nebo v databázi SQL Azure, by měl nechte nezaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="3ed50-157">When connecting tooSQL Server or Azure SQL Database, you should leave it unchecked.</span></span> |
   |  <span data-ttu-id="3ed50-158">Počet získaných za blok řádků</span><span class="sxs-lookup"><span data-stu-id="3ed50-158">Rows fetched per block</span></span> |<span data-ttu-id="3ed50-159">Při načítání velkému počtu záznamů, optimalizace pro tento parametr může být požadované tooensure přínos optimální.</span><span class="sxs-lookup"><span data-stu-id="3ed50-159">When fetching a large number of records, tuning this parameter may be required tooensure optimal performances.</span></span> |
   |  <span data-ttu-id="3ed50-160">Výchozí délka sloupce řetězec, délka binárního sloupce, měřítko Decimal sloupce</span><span class="sxs-lookup"><span data-stu-id="3ed50-160">Default string column length, Binary column length, Decimal column scale</span></span> |<span data-ttu-id="3ed50-161">Hello datový typ délky a přesnosti může mít vliv na tom, jak je vrácená data.</span><span class="sxs-lookup"><span data-stu-id="3ed50-161">hello data type lengths and precisions may affect how data is returned.</span></span> <span data-ttu-id="3ed50-162">Způsobí nesprávné informace o toobe vrácená z důvodu tooloss přesnost nebo zkrácení.</span><span class="sxs-lookup"><span data-stu-id="3ed50-162">They cause incorrect information toobe returned due tooloss of precision and/or truncation.</span></span> |

    <span data-ttu-id="3ed50-163">![Rozšířené možnosti](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png "DSN rozšířené možnosti konfigurace")</span><span class="sxs-lookup"><span data-stu-id="3ed50-163">![Advanced options](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png "Advanced DSN configuration options")</span></span>

1. <span data-ttu-id="3ed50-164">Klikněte na tlačítko **Test** zdroj dat tootest hello.</span><span class="sxs-lookup"><span data-stu-id="3ed50-164">Click **Test** tootest hello data source.</span></span> <span data-ttu-id="3ed50-165">Zdroj dat hello je správně nakonfigurovaný, zobrazuje *testy byla úspěšně DOKONČENA.!*.</span><span class="sxs-lookup"><span data-stu-id="3ed50-165">When hello data source is configured correctly, it shows *TESTS COMPLETED SUCCESSFULLY!*.</span></span>
2. <span data-ttu-id="3ed50-166">Klikněte na tlačítko **OK** tooclose hello Test – dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3ed50-166">Click **OK** tooclose hello Test dialog.</span></span> <span data-ttu-id="3ed50-167">Hello nový zdroj dat je uvedeno v hello **správce zdrojů dat ODBC**.</span><span class="sxs-lookup"><span data-stu-id="3ed50-167">hello new data source shall be listed on hello **ODBC Data Source Administrator**.</span></span>
3. <span data-ttu-id="3ed50-168">Klikněte na tlačítko **OK** tooexit hello průvodce.</span><span class="sxs-lookup"><span data-stu-id="3ed50-168">Click **OK** tooexit hello wizard.</span></span>

## <a name="import-data-into-excel-from-hdinsight"></a><span data-ttu-id="3ed50-169">Import dat do Excelu ze služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="3ed50-169">Import data into Excel from HDInsight</span></span>
<span data-ttu-id="3ed50-170">Hello následující kroky popisují hello způsob tooimport data z tabulky Hive do sešitu aplikace Excel pomocí hello zdroje dat ODBC, který jste vytvořili v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="3ed50-170">hello following steps describe hello way tooimport data from a Hive table into an Excel workbook using hello ODBC data source that you created in hello previous section.</span></span>

1. <span data-ttu-id="3ed50-171">V Excelu otevřete nový nebo existující sešit.</span><span class="sxs-lookup"><span data-stu-id="3ed50-171">Open a new or existing workbook in Excel.</span></span>
2. <span data-ttu-id="3ed50-172">Z hello **Data** , klikněte na **načíst Data**, klikněte na tlačítko **z jiných zdrojů**a potom klikněte na **z rozhraní ODBC** toolaunch hello  **Průvodce datovým připojením**.</span><span class="sxs-lookup"><span data-stu-id="3ed50-172">From hello **Data** tab, click **Get Data**, click **From Other Sources**, and then click **From ODBC** toolaunch hello **Data Connection Wizard**.</span></span>
   
    <span data-ttu-id="3ed50-173">![Průvodce připojením Open data](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png "Open data Průvodce připojením")</span><span class="sxs-lookup"><span data-stu-id="3ed50-173">![Open data connection wizard](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png "Open data connection wizard")</span></span>
4. <span data-ttu-id="3ed50-174">Název, který jste vytvořili v poslední části hello zdroje dat vyberte hello a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="3ed50-174">Select hello data source name that you created in hello last section, and then click **OK**.</span></span>
5. <span data-ttu-id="3ed50-175">Zadejte uživatelské jméno Hadoop (hello výchozí název je správce) a hello heslo a pak klikněte na tlačítko **Connect**.</span><span class="sxs-lookup"><span data-stu-id="3ed50-175">Enter Hadoop user name (hello default name is admin) and hello password, and then click **Connect**.</span></span>
6. <span data-ttu-id="3ed50-176">Navigátor, rozbalte **HIVE**, rozbalte položku **výchozí**, klikněte na tlačítko **hivesampletable**a potom klikněte na **zatížení**.</span><span class="sxs-lookup"><span data-stu-id="3ed50-176">On Navigator, expand **HIVE**, expand **default**, click **hivesampletable**, and then click **Load**.</span></span> <span data-ttu-id="3ed50-177">Jak dlouho trvá několik sekund, než získá importované tooExcel data.</span><span class="sxs-lookup"><span data-stu-id="3ed50-177">It takes a few seconds before data gets imported tooExcel.</span></span>

    <span data-ttu-id="3ed50-178">![Navigátor HDInsight Hive ODBC](./media/hdinsight-connect-excel-hive-ODBC-driver/hdinsight.hive.odbc.navigator.png "Open data Průvodce připojením")</span><span class="sxs-lookup"><span data-stu-id="3ed50-178">![HDInsight Hive ODBC navigator](./media/hdinsight-connect-excel-hive-ODBC-driver/hdinsight.hive.odbc.navigator.png "Open data connection wizard")</span></span>


## <a name="next-steps"></a><span data-ttu-id="3ed50-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3ed50-179">Next steps</span></span>
<span data-ttu-id="3ed50-180">V tomto článku jste se dozvěděli, jak toouse hello data tooretrieve ovladače Microsoft Hive ODBC z hello služba HDInsight do aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="3ed50-180">In this article, you learned how toouse hello Microsoft Hive ODBC driver tooretrieve data from hello HDInsight Service into Excel.</span></span> <span data-ttu-id="3ed50-181">Podobně můžete data načíst z hello služba HDInsight do databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="3ed50-181">Similarly, you can retrieve data from hello HDInsight Service into SQL Database.</span></span> <span data-ttu-id="3ed50-182">Je také možné tooupload data do služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3ed50-182">It is also possible tooupload data into an HDInsight Service.</span></span> <span data-ttu-id="3ed50-183">toolearn více, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="3ed50-183">toolearn more, see:</span></span>

* <span data-ttu-id="3ed50-184">[Analýza dat zpoždění letu pomocí HDInsight][hdinsight-analyze-flight-data]</span><span class="sxs-lookup"><span data-stu-id="3ed50-184">[Analyze flight delay data using HDInsight][hdinsight-analyze-flight-data]</span></span>
* <span data-ttu-id="3ed50-185">[Nahrání dat tooHDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="3ed50-185">[Upload Data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="3ed50-186">[Použití nástroje Sqoop s HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="3ed50-186">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698


