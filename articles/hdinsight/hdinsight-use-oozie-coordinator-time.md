---
title: "Použít časové coordinator Hadoop Oozie v HDInsight | Microsoft Docs"
description: "V prostředí HDInsight, Cloudová služba velkých dat pomocí Hadoop Oozie coordinator založené na čase. Zjistěte, jak definovat pracovní postupy Oozie a koordinátoři a odesílání úloh."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00c3a395-d51a-44ff-af2d-1f116c4b1c83
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 600a70c74a16e2601a874f804ac2e8382c8bfa90
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-time-based-oozie-coordinator-with-hadoop-in-hdinsight-to-define-workflows-and-coordinate-jobs"></a><span data-ttu-id="786fe-104">Pomocí nástroje Oozie coordinator založené na čase s Hadoop v HDInsight můžete definovat pracovní postupy a koordinovat úlohy</span><span class="sxs-lookup"><span data-stu-id="786fe-104">Use time-based Oozie coordinator with Hadoop in HDInsight to define workflows and coordinate jobs</span></span>
<span data-ttu-id="786fe-105">V tomto článku se dozvíte, jak definovat pracovní postupy a koordinátoři a spouštění koordinátor úlohy, na základě času.</span><span class="sxs-lookup"><span data-stu-id="786fe-105">In this article, you'll learn how to define workflows and coordinators, and how to trigger the coordinator jobs, based on time.</span></span> <span data-ttu-id="786fe-106">Je užitečné projít [Oozie použití s HDInsight] [ hdinsight-use-oozie] před přečtěte si tento článek.</span><span class="sxs-lookup"><span data-stu-id="786fe-106">It is helpful to go through [Use Oozie with HDInsight][hdinsight-use-oozie] before you read this article.</span></span> <span data-ttu-id="786fe-107">Kromě Oozie můžete také naplánovat úlohy pomocí Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="786fe-107">In addition to Oozie, you can also schedule jobs using Azure Data Factory.</span></span> <span data-ttu-id="786fe-108">Další služby Azure Data Factory najdete v tématu [použijte Pig a Hive pomocí služby Data Factory](../data-factory/data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="786fe-108">To learn Azure Data Factory, see [Use Pig and Hive with Data Factory](../data-factory/data-factory-data-transformation-activities.md).</span></span>

> [!NOTE]
> <span data-ttu-id="786fe-109">Tento článek vyžaduje cluster HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="786fe-109">This article requires a Windows-based HDInsight cluster.</span></span> <span data-ttu-id="786fe-110">Informace o používání Oozie, včetně úloh založené na čase, v clusteru se systémem Linux naleznete v části [Oozie použití se systémem Hadoop k definování a spuštění workflowu v HDInsight se systémem Linux](hdinsight-use-oozie-linux-mac.md)</span><span class="sxs-lookup"><span data-stu-id="786fe-110">For information on using Oozie, including time-based jobs, on a Linux-based cluster, see [Use Oozie with Hadoop to define and run a workflow on Linux-based HDInsight](hdinsight-use-oozie-linux-mac.md)</span></span>

## <a name="what-is-oozie"></a><span data-ttu-id="786fe-111">Co je Oozie</span><span class="sxs-lookup"><span data-stu-id="786fe-111">What is Oozie</span></span>
<span data-ttu-id="786fe-112">Apache Oozie je pracovní postup nebo koordinaci systém, který spravuje úloh Hadoop.</span><span class="sxs-lookup"><span data-stu-id="786fe-112">Apache Oozie is a workflow/coordination system that manages Hadoop jobs.</span></span> <span data-ttu-id="786fe-113">Je integrován do zásobníku Hadoop, a podporuje úloh Hadoop pro Apache MapReduce, Apache Pig, Apache Hive a Apache Sqoop.</span><span class="sxs-lookup"><span data-stu-id="786fe-113">It is integrated with the Hadoop stack, and it supports Hadoop jobs for Apache MapReduce, Apache Pig, Apache Hive, and Apache Sqoop.</span></span> <span data-ttu-id="786fe-114">Můžete se také používají k plánování úloh, které jsou specifické pro systém, jako jsou programy v jazyce Java nebo skripty prostředí.</span><span class="sxs-lookup"><span data-stu-id="786fe-114">It can also be used to schedule jobs that are specific to a system, such as Java programs or shell scripts.</span></span>

<span data-ttu-id="786fe-115">Následující obrázek ukazuje pracovní postup, který implementujete:</span><span class="sxs-lookup"><span data-stu-id="786fe-115">The following image shows the workflow you will implement:</span></span>

![Diagram pracovního postupu][img-workflow-diagram]

<span data-ttu-id="786fe-117">Pracovní postup obsahuje dvě akce:</span><span class="sxs-lookup"><span data-stu-id="786fe-117">The workflow contains two actions:</span></span>

1. <span data-ttu-id="786fe-118">Akce Hive spouští skript HiveQL k určení počtu výskytů každý typ úroveň protokolu v souboru protokolu log4j.</span><span class="sxs-lookup"><span data-stu-id="786fe-118">A Hive action runs a HiveQL script to count the occurrences of each log-level type in a log4j log file.</span></span> <span data-ttu-id="786fe-119">Každý log4j protokolový soubor obsahuje řádku pole, která obsahuje pole [úroveň protokolu], které chcete zobrazit typ a závažnost, například:</span><span class="sxs-lookup"><span data-stu-id="786fe-119">Each log4j log consists of a line of fields that contains a [LOG LEVEL] field to show the type and the severity, for example:</span></span>

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    <span data-ttu-id="786fe-120">Výstup skriptu Hive je podobná:</span><span class="sxs-lookup"><span data-stu-id="786fe-120">The Hive script output is similar to:</span></span>

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    <span data-ttu-id="786fe-121">Další informace o Hivu najdete v tématu [Použití Hivu se službou HDInsight][hdinsight-use-hive].</span><span class="sxs-lookup"><span data-stu-id="786fe-121">For more information about Hive, see [Use Hive with HDInsight][hdinsight-use-hive].</span></span>
2. <span data-ttu-id="786fe-122">Akce Sqoop Exportuje výstup akce HiveQL do tabulky v databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="786fe-122">A Sqoop action exports the HiveQL action output to a table in an Azure SQL database.</span></span> <span data-ttu-id="786fe-123">Další informace o Sqoop najdete v tématu [Sqoop použití s HDInsight][hdinsight-use-sqoop].</span><span class="sxs-lookup"><span data-stu-id="786fe-123">For more information about Sqoop, see [Use Sqoop with HDInsight][hdinsight-use-sqoop].</span></span>

> [!NOTE]
> <span data-ttu-id="786fe-124">Podporované verze Oozie v clusterech prostředí HDInsight najdete v tématu [co je nového ve verzích clusterů poskytovaných v HDInsight?] [hdinsight-versions].</span><span class="sxs-lookup"><span data-stu-id="786fe-124">For supported Oozie versions on HDInsight clusters, see [What's new in the cluster versions provided by HDInsight?][hdinsight-versions].</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="786fe-125">Požadavky</span><span class="sxs-lookup"><span data-stu-id="786fe-125">Prerequisites</span></span>
<span data-ttu-id="786fe-126">Je nutné, abyste před zahájením tohoto kurzu měli tyto položky:</span><span class="sxs-lookup"><span data-stu-id="786fe-126">Before you begin this tutorial, you must have the following:</span></span>

* <span data-ttu-id="786fe-127">**Pracovní stanice s prostředím Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="786fe-127">**A workstation with Azure PowerShell**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="786fe-128">Podpora prostředí Azure PowerShell pro správu prostředků služby HDInsight pomocí Azure Service Manageru je **zastaralá** a 1. ledna 2017 dojde k jejímu odebrání.</span><span class="sxs-lookup"><span data-stu-id="786fe-128">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and will be removed by January 1, 2017.</span></span> <span data-ttu-id="786fe-129">Kroky v tomto dokumentu používají nové rutiny služby HDInsight, které pracují s Azure Resource Managerem.</span><span class="sxs-lookup"><span data-stu-id="786fe-129">The steps in this document use the new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="786fe-130">Podle postupu v tématu [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) si nainstalujte nejnovější verzi prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="786fe-130">Please follow the steps in [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) to install the latest version of Azure PowerShell.</span></span> <span data-ttu-id="786fe-131">Pokud máte skripty, které je potřeba upravit tak, aby používaly nové rutiny, které pracují s nástrojem Azure Resource Manager, najdete další informace v tématu [Migrace na vývojové nástroje založené na Azure Resource Manageru pro clustery služby HDInsight](hdinsight-hadoop-development-using-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="786fe-131">If you have scripts that need to be modified to use the new cmdlets that work with Azure Resource Manager, see [Migrating to Azure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

* <span data-ttu-id="786fe-132">**Cluster služby HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="786fe-132">**An HDInsight cluster**.</span></span> <span data-ttu-id="786fe-133">Informace o vytváření clusteru služby HDInsight najdete v tématu [Tvorba clusterů HDInsight][hdinsight-provision], nebo [Začínáme s HDInsight][hdinsight-get-started].</span><span class="sxs-lookup"><span data-stu-id="786fe-133">For information about creating an HDInsight cluster, see [Create HDInsight clusters][hdinsight-provision], or [Get started with HDInsight][hdinsight-get-started].</span></span> <span data-ttu-id="786fe-134">Následující data, která mají absolvovat kurz budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="786fe-134">You will need the following data to go through the tutorial:</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="786fe-135">Vlastnost clusteru</span><span class="sxs-lookup"><span data-stu-id="786fe-135">Cluster property</span></span></th><th><span data-ttu-id="786fe-136">Název proměnné prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="786fe-136">Windows PowerShell variable name</span></span></th><th><span data-ttu-id="786fe-137">Hodnota</span><span class="sxs-lookup"><span data-stu-id="786fe-137">Value</span></span></th><th><span data-ttu-id="786fe-138">Popis</span><span class="sxs-lookup"><span data-stu-id="786fe-138">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="786fe-139">Název clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="786fe-139">HDInsight cluster name</span></span></td><td><span data-ttu-id="786fe-140">$clusterName</span><span class="sxs-lookup"><span data-stu-id="786fe-140">$clusterName</span></span></td><td></td><td><span data-ttu-id="786fe-141">Cluster HDInsight, na kterém budete spouštět v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="786fe-141">The HDInsight cluster on which you will run this tutorial.</span></span></td></tr>
    <tr><td><span data-ttu-id="786fe-142">Uživatelské jméno clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="786fe-142">HDInsight cluster username</span></span></td><td><span data-ttu-id="786fe-143">$clusterUsername</span><span class="sxs-lookup"><span data-stu-id="786fe-143">$clusterUsername</span></span></td><td></td><td><span data-ttu-id="786fe-144">Uživatelské jméno clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="786fe-144">The HDInsight cluster user name.</span></span> </td></tr>
    <tr><td><span data-ttu-id="786fe-145">Heslo uživatele clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="786fe-145">HDInsight cluster user password</span></span> </td><td><span data-ttu-id="786fe-146">$clusterPassword</span><span class="sxs-lookup"><span data-stu-id="786fe-146">$clusterPassword</span></span></td><td></td><td><span data-ttu-id="786fe-147">Heslo uživatele clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="786fe-147">The HDInsight cluster user password.</span></span></td></tr>
    <tr><td><span data-ttu-id="786fe-148">Název účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="786fe-148">Azure storage account name</span></span></td><td><span data-ttu-id="786fe-149">$storageAccountName</span><span class="sxs-lookup"><span data-stu-id="786fe-149">$storageAccountName</span></span></td><td></td><td><span data-ttu-id="786fe-150">Účet služby Azure Storage k dispozici ke clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="786fe-150">An Azure Storage account available to the HDInsight cluster.</span></span> <span data-ttu-id="786fe-151">V tomto kurzu použijte výchozí účet úložiště, který jste zadali během procesu zřizování clusteru.</span><span class="sxs-lookup"><span data-stu-id="786fe-151">For this tutorial, use the default storage account that you specified during the cluster provision process.</span></span></td></tr>
    <tr><td><span data-ttu-id="786fe-152">Název kontejneru Azure Blob</span><span class="sxs-lookup"><span data-stu-id="786fe-152">Azure Blob container name</span></span></td><td><span data-ttu-id="786fe-153">$containerName</span><span class="sxs-lookup"><span data-stu-id="786fe-153">$containerName</span></span></td><td></td><td><span data-ttu-id="786fe-154">V tomto příkladu použijte kontejner úložiště objektů Blob v Azure, který se používá pro výchozí systém souborů clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="786fe-154">For this example, use the Azure Blob storage container that is used for the default HDInsight cluster file system.</span></span> <span data-ttu-id="786fe-155">Ve výchozím nastavení má stejný název jako HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="786fe-155">By default, it has the same name as the HDInsight cluster.</span></span></td></tr>
    </table><span data-ttu-id="786fe-156">
* **Azure SQL database**.</span><span class="sxs-lookup"><span data-stu-id="786fe-156">
* **An Azure SQL database**.</span></span> <span data-ttu-id="786fe-157">Je nutné nakonfigurovat pravidlo brány firewall pro server databáze SQL pro povolení přístupu z pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="786fe-157">You must configure a firewall rule for the SQL Database server to allow access from your workstation.</span></span> <span data-ttu-id="786fe-158">Pokyny týkající se vytváření databáze Azure SQL a konfiguraci brány firewall najdete v tématu [začít používat Azure SQL database] [databáze_sql get-started].</span><span class="sxs-lookup"><span data-stu-id="786fe-158">For instructions about creating an Azure SQL database and configuring the firewall, see [Get started using Azure SQL database][sqldatabase-get-started].</span></span> <span data-ttu-id="786fe-159">Tento článek obsahuje skript prostředí Windows PowerShell pro vytvoření tabulky databáze Azure SQL, které potřebujete pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="786fe-159">This article provides a Windows PowerShell script for creating the Azure SQL database table that you need for this tutorial.</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="786fe-160">Vlastnost databáze SQL</span><span class="sxs-lookup"><span data-stu-id="786fe-160">SQL database property</span></span></th><th><span data-ttu-id="786fe-161">Název proměnné prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="786fe-161">Windows PowerShell variable name</span></span></th><th><span data-ttu-id="786fe-162">Hodnota</span><span class="sxs-lookup"><span data-stu-id="786fe-162">Value</span></span></th><th><span data-ttu-id="786fe-163">Popis</span><span class="sxs-lookup"><span data-stu-id="786fe-163">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="786fe-164">Název databáze serveru SQL</span><span class="sxs-lookup"><span data-stu-id="786fe-164">SQL database server name</span></span></td><td><span data-ttu-id="786fe-165">$sqlDatabaseServer</span><span class="sxs-lookup"><span data-stu-id="786fe-165">$sqlDatabaseServer</span></span></td><td></td><td><span data-ttu-id="786fe-166">Databáze SQL server, ke kterému bude Sqoop exportovat data.</span><span class="sxs-lookup"><span data-stu-id="786fe-166">The SQL database server to which Sqoop will export data.</span></span> </td></tr>
    <tr><td><span data-ttu-id="786fe-167">Přihlašovací jméno SQL databáze</span><span class="sxs-lookup"><span data-stu-id="786fe-167">SQL database login name</span></span></td><td><span data-ttu-id="786fe-168">$sqlDatabaseLogin</span><span class="sxs-lookup"><span data-stu-id="786fe-168">$sqlDatabaseLogin</span></span></td><td></td><td><span data-ttu-id="786fe-169">Přihlašovací jméno SQL Database.</span><span class="sxs-lookup"><span data-stu-id="786fe-169">SQL Database login name.</span></span></td></tr>
    <tr><td><span data-ttu-id="786fe-170">Heslo pro přihlášení databáze SQL</span><span class="sxs-lookup"><span data-stu-id="786fe-170">SQL database login password</span></span></td><td><span data-ttu-id="786fe-171">$sqlDatabaseLoginPassword</span><span class="sxs-lookup"><span data-stu-id="786fe-171">$sqlDatabaseLoginPassword</span></span></td><td></td><td><span data-ttu-id="786fe-172">Heslo přihlášení k databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="786fe-172">SQL Database login password.</span></span></td></tr>
    <tr><td><span data-ttu-id="786fe-173">Název databáze SQL</span><span class="sxs-lookup"><span data-stu-id="786fe-173">SQL database name</span></span></td><td><span data-ttu-id="786fe-174">$sqlDatabaseName</span><span class="sxs-lookup"><span data-stu-id="786fe-174">$sqlDatabaseName</span></span></td><td></td><td><span data-ttu-id="786fe-175">Azure SQL database, ke kterému bude Sqoop exportovat data.</span><span class="sxs-lookup"><span data-stu-id="786fe-175">The Azure SQL database to which Sqoop will export data.</span></span> </td></tr>
    </table>

  > [!NOTE]
  > <span data-ttu-id="786fe-176">Ve výchozím nastavení Azure SQL database umožňuje připojení z Azure služby, jako je Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="786fe-176">By default an Azure SQL database allows connections from Azure Services, such as Azure HDInsight.</span></span> <span data-ttu-id="786fe-177">Pokud toto nastavení brány firewall je zakázáno, musíte ji povolit z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="786fe-177">If this firewall setting is disabled, you must enable it from the Azure Portal.</span></span> <span data-ttu-id="786fe-178">Pokyny týkající se vytváření databáze SQL a konfigurace pravidel brány firewall, najdete v části [vytvořit a nakonfigurovat databázi SQL][sqldatabase-get-started].</span><span class="sxs-lookup"><span data-stu-id="786fe-178">For instruction about creating a SQL Database and configuring firewall rules, see [Create and Configure SQL Database][sqldatabase-get-started].</span></span>

> [!NOTE]
> <span data-ttu-id="786fe-179">Vyplňování hodnoty v tabulkách.</span><span class="sxs-lookup"><span data-stu-id="786fe-179">Fill-in the values in the tables.</span></span> <span data-ttu-id="786fe-180">Je užitečné při procházení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="786fe-180">It will be helpful for going through this tutorial.</span></span>

## <a name="define-oozie-workflow-and-the-related-hiveql-script"></a><span data-ttu-id="786fe-181">Definovat Oozie pracovního postupu a související skript HiveQL</span><span class="sxs-lookup"><span data-stu-id="786fe-181">Define Oozie workflow and the related HiveQL script</span></span>
<span data-ttu-id="786fe-182">Definice Oozie pracovní postupy jsou zapsány ve hPDL (jazyka definice proces XML).</span><span class="sxs-lookup"><span data-stu-id="786fe-182">Oozie workflows definitions are written in hPDL (an XML process definition language).</span></span> <span data-ttu-id="786fe-183">Výchozí název souboru pracovního postupu je *workflow.xml*.</span><span class="sxs-lookup"><span data-stu-id="786fe-183">The default workflow file name is *workflow.xml*.</span></span>  <span data-ttu-id="786fe-184">Budete uložte místně soubor pracovního postupu a nasadíte ho do clusteru HDInsight pomocí prostředí Azure PowerShell později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="786fe-184">You will save the workflow file locally, and then deploy it to the HDInsight cluster by using Azure PowerShell later in this tutorial.</span></span>

<span data-ttu-id="786fe-185">Akce Hive v pracovním postupu volá soubor skriptu HiveQL.</span><span class="sxs-lookup"><span data-stu-id="786fe-185">The Hive action in the workflow calls a HiveQL script file.</span></span> <span data-ttu-id="786fe-186">Tento soubor skriptu obsahuje tři příkazy HiveQL:</span><span class="sxs-lookup"><span data-stu-id="786fe-186">This script file contains three HiveQL statements:</span></span>

1. <span data-ttu-id="786fe-187">**Příkaz DROP TABLE** odstraní tabulku Hive log4j, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="786fe-187">**The DROP TABLE statement** deletes the log4j Hive table if it exists.</span></span>
2. <span data-ttu-id="786fe-188">**Příkaz CREATE TABLE** vytvoří log4j externí tabulku Hive, který odkazuje na umístění souboru protokolu log4j;</span><span class="sxs-lookup"><span data-stu-id="786fe-188">**The CREATE TABLE statement** creates a log4j Hive external table, which points to the location of the log4j log file;</span></span>
3. <span data-ttu-id="786fe-189">**Umístění souboru protokolu log4j**.</span><span class="sxs-lookup"><span data-stu-id="786fe-189">**The location of the log4j log file**.</span></span> <span data-ttu-id="786fe-190">Oddělovač polí je ",".</span><span class="sxs-lookup"><span data-stu-id="786fe-190">The field delimiter is ",".</span></span> <span data-ttu-id="786fe-191">Oddělovač řádku výchozí je "\n".</span><span class="sxs-lookup"><span data-stu-id="786fe-191">The default line delimiter is "\n".</span></span> <span data-ttu-id="786fe-192">Externí tabulku Hive se používá předejdete datového souboru odebírán z původního umístění, v případě, že chcete spustit Oozie workflow vícekrát.</span><span class="sxs-lookup"><span data-stu-id="786fe-192">A Hive external table is used to avoid the data file being removed from the original location, in case you want to run the Oozie workflow multiple times.</span></span>
4. <span data-ttu-id="786fe-193">**Příkaz INSERT PŘEPSAT** počtu výskytů každý typ úroveň protokolu z tabulky Hive log4j a uloží výstup do umístění úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="786fe-193">**The INSERT OVERWRITE statement** counts the occurrences of each log-level type from the log4j Hive table, and it saves the output to an Azure Blob storage location.</span></span>

> [!NOTE]
> <span data-ttu-id="786fe-194">Je známý problém cesta Hive.</span><span class="sxs-lookup"><span data-stu-id="786fe-194">There is a known Hive path issue.</span></span> <span data-ttu-id="786fe-195">Budete spouštět na tento problém při odesílání úlohu Oozie.</span><span class="sxs-lookup"><span data-stu-id="786fe-195">You will run into this problem when submitting an Oozie job.</span></span> <span data-ttu-id="786fe-196">Pokyny k nápravě problému naleznete na webu TechNet Wiki: [HDInsight Hive Chyba: nelze přejmenovat][technetwiki-hive-error].</span><span class="sxs-lookup"><span data-stu-id="786fe-196">The instructions for fixing the issue can be found on the TechNet Wiki: [HDInsight Hive error: Unable to rename][technetwiki-hive-error].</span></span>

<span data-ttu-id="786fe-197">**Zadat soubor skriptu HiveQL k volání v tomto pracovním postupu**</span><span class="sxs-lookup"><span data-stu-id="786fe-197">**To define the HiveQL script file to be called by the workflow**</span></span>

1. <span data-ttu-id="786fe-198">Vytvořte textový soubor s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="786fe-198">Create a text file with the following content:</span></span>

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    <span data-ttu-id="786fe-199">Existují tři proměnné používané ve skriptu:</span><span class="sxs-lookup"><span data-stu-id="786fe-199">There are three variables used in the script:</span></span>

   * <span data-ttu-id="786fe-200">${hiveTableName}</span><span class="sxs-lookup"><span data-stu-id="786fe-200">${hiveTableName}</span></span>
   * <span data-ttu-id="786fe-201">${hiveDataFolder}</span><span class="sxs-lookup"><span data-stu-id="786fe-201">${hiveDataFolder}</span></span>
   * <span data-ttu-id="786fe-202">${hiveOutputFolder}</span><span class="sxs-lookup"><span data-stu-id="786fe-202">${hiveOutputFolder}</span></span>

     <span data-ttu-id="786fe-203">Soubor definice pracovního postupu (workflow.xml v tomto kurzu) předá tyto hodnoty tento skript HiveQL v době běhu.</span><span class="sxs-lookup"><span data-stu-id="786fe-203">The workflow definition file (workflow.xml in this tutorial) will pass these values to this HiveQL script at run time.</span></span>
2. <span data-ttu-id="786fe-204">Uložte soubor jako **C:\Tutorials\UseOozie\useooziewf.hql** pomocí kódování ANSI (ASCII).</span><span class="sxs-lookup"><span data-stu-id="786fe-204">Save the file as **C:\Tutorials\UseOozie\useooziewf.hql** by using ANSI (ASCII) encoding.</span></span> <span data-ttu-id="786fe-205">(Použijte Poznámkový blok, pokud textového editoru neposkytuje tuto možnost.) Tento soubor skriptu nasadí do clusteru HDInsight později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="786fe-205">(Use Notepad if your text editor doesn't provide this option.) This script file will be deployed to the HDInsight cluster later in the tutorial.</span></span>

<span data-ttu-id="786fe-206">**Chcete-li definovat pracovní postup**</span><span class="sxs-lookup"><span data-stu-id="786fe-206">**To define a workflow**</span></span>

1. <span data-ttu-id="786fe-207">Vytvořte textový soubor s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="786fe-207">Create a text file with the following content:</span></span>

    ```xml
    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start to = "RunHiveScript"/>

        <action name="RunHiveScript">
            <hive xmlns="uri:oozie:hive-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.job.queue.name</name>
                        <value>${queueName}</value>
                    </property>
                </configuration>
                <script>${hiveScript}</script>
                <param>hiveTableName=${hiveTableName}</param>
                <param>hiveDataFolder=${hiveDataFolder}</param>
                <param>hiveOutputFolder=${hiveOutputFolder}</param>
            </hive>
            <ok to="RunSqoopExport"/>
            <error to="fail"/>
        </action>

        <action name="RunSqoopExport">
            <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.compress.map.output</name>
                        <value>true</value>
                    </property>
                </configuration>
            <arg>export</arg>
            <arg>--connect</arg>
            <arg>${sqlDatabaseConnectionString}</arg>
            <arg>--table</arg>
            <arg>${sqlDatabaseTableName}</arg>
            <arg>--export-dir</arg>
            <arg>${hiveOutputFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\001"</arg>
            </sqoop>
            <ok to="end"/>
            <error to="fail"/>
        </action>

        <kill name="fail">
            <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
        </kill>

        <end name="end"/>
    </workflow-app>
    ```

    <span data-ttu-id="786fe-208">Existují dvě akce, které jsou definovány v pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="786fe-208">There are two actions defined in the workflow.</span></span> <span data-ttu-id="786fe-209">Tato akce spuštění *RunHiveScript*.</span><span class="sxs-lookup"><span data-stu-id="786fe-209">The start-to action is *RunHiveScript*.</span></span> <span data-ttu-id="786fe-210">Pokud se spustí akce *OK*, je další akce *RunSqoopExport*.</span><span class="sxs-lookup"><span data-stu-id="786fe-210">If the action runs *OK*, the next action is *RunSqoopExport*.</span></span>

    <span data-ttu-id="786fe-211">RunHiveScript má několik proměnné.</span><span class="sxs-lookup"><span data-stu-id="786fe-211">The RunHiveScript has several variables.</span></span> <span data-ttu-id="786fe-212">Při odesílání úlohy Oozie z pracovní stanice pomocí prostředí Azure PowerShell, projdou hodnoty.</span><span class="sxs-lookup"><span data-stu-id="786fe-212">You will pass the values when you submit the Oozie job from your workstation by using Azure PowerShell.</span></span>

    <span data-ttu-id="786fe-213">Proměnné pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="786fe-213">Workflow variables</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="786fe-214">Proměnné pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="786fe-214">Workflow variables</span></span></th><th><span data-ttu-id="786fe-215">Popis</span><span class="sxs-lookup"><span data-stu-id="786fe-215">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="786fe-216">${jobTracker}</span><span class="sxs-lookup"><span data-stu-id="786fe-216">${jobTracker}</span></span></td><td><span data-ttu-id="786fe-217">Zadejte adresu URL ke sledovacímu modulu úlohy Hadoop.</span><span class="sxs-lookup"><span data-stu-id="786fe-217">Specify the URL of the Hadoop job tracker.</span></span> <span data-ttu-id="786fe-218">Použití <strong>jobtrackerhost:9010</strong> v HDInsight clusteru verze 3.0 a 2.0.</span><span class="sxs-lookup"><span data-stu-id="786fe-218">Use <strong>jobtrackerhost:9010</strong> on HDInsight cluster version 3.0 and 2.0.</span></span></td></tr>
    <tr><td><span data-ttu-id="786fe-219">${nameNode}</span><span class="sxs-lookup"><span data-stu-id="786fe-219">${nameNode}</span></span></td><td><span data-ttu-id="786fe-220">Zadejte adresu URL Hadoop název uzlu.</span><span class="sxs-lookup"><span data-stu-id="786fe-220">Specify the URL of the Hadoop name node.</span></span> <span data-ttu-id="786fe-221">Použít výchozí wasb systému souborů: / / adres, například <i>wasb: / /&lt;containerName&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</span><span class="sxs-lookup"><span data-stu-id="786fe-221">Use the default file system wasb:// address, for example, <i>wasb://&lt;containerName&gt;@&lt;storageAccountName&gt;.blob.core.windows.net</i>.</span></span></td></tr>
    <tr><td><span data-ttu-id="786fe-222">${queueName}</span><span class="sxs-lookup"><span data-stu-id="786fe-222">${queueName}</span></span></td><td><span data-ttu-id="786fe-223">Určuje název fronty, který bude úloha odeslána.</span><span class="sxs-lookup"><span data-stu-id="786fe-223">Specifies the queue name that the job will be submitted to.</span></span> <span data-ttu-id="786fe-224">Použití <strong>výchozí</strong>.</span><span class="sxs-lookup"><span data-stu-id="786fe-224">Use <strong>default</strong>.</span></span></td></tr>
    </table>

    <span data-ttu-id="786fe-225">Proměnné akcí v Hive</span><span class="sxs-lookup"><span data-stu-id="786fe-225">Hive action variables</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="786fe-226">Hive proměnné akce</span><span class="sxs-lookup"><span data-stu-id="786fe-226">Hive action variable</span></span></th><th><span data-ttu-id="786fe-227">Popis</span><span class="sxs-lookup"><span data-stu-id="786fe-227">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="786fe-228">${hiveDataFolder}</span><span class="sxs-lookup"><span data-stu-id="786fe-228">${hiveDataFolder}</span></span></td><td><span data-ttu-id="786fe-229">Zdrojový adresář pro příkaz Hive Create Table.</span><span class="sxs-lookup"><span data-stu-id="786fe-229">The source directory for the Hive Create Table command.</span></span></td></tr>
    <tr><td><span data-ttu-id="786fe-230">${hiveOutputFolder}</span><span class="sxs-lookup"><span data-stu-id="786fe-230">${hiveOutputFolder}</span></span></td><td><span data-ttu-id="786fe-231">Výstupní složky pro příkaz INSERT PŘEPSAT.</span><span class="sxs-lookup"><span data-stu-id="786fe-231">The output folder for the INSERT OVERWRITE statement.</span></span></td></tr>
    <tr><td><span data-ttu-id="786fe-232">${hiveTableName}</span><span class="sxs-lookup"><span data-stu-id="786fe-232">${hiveTableName}</span></span></td><td><span data-ttu-id="786fe-233">Název tabulky Hive, která odkazuje na log4j datových souborů.</span><span class="sxs-lookup"><span data-stu-id="786fe-233">The name of the Hive table that references the log4j data files.</span></span></td></tr>
    </table>

    <span data-ttu-id="786fe-234">Proměnné akcí v Sqoop</span><span class="sxs-lookup"><span data-stu-id="786fe-234">Sqoop action variables</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="786fe-235">Sqoop proměnné akce</span><span class="sxs-lookup"><span data-stu-id="786fe-235">Sqoop action variable</span></span></th><th><span data-ttu-id="786fe-236">Popis</span><span class="sxs-lookup"><span data-stu-id="786fe-236">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="786fe-237">${sqlDatabaseConnectionString}</span><span class="sxs-lookup"><span data-stu-id="786fe-237">${sqlDatabaseConnectionString}</span></span></td><td><span data-ttu-id="786fe-238">Připojovací řetězec databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="786fe-238">SQL Database connection string.</span></span></td></tr>
    <tr><td><span data-ttu-id="786fe-239">${sqlDatabaseTableName}</span><span class="sxs-lookup"><span data-stu-id="786fe-239">${sqlDatabaseTableName}</span></span></td><td><span data-ttu-id="786fe-240">Tabulka databáze Azure SQL do kterého se budou data exportovat.</span><span class="sxs-lookup"><span data-stu-id="786fe-240">The Azure SQL database table to where the data will be exported.</span></span></td></tr>
    <tr><td><span data-ttu-id="786fe-241">${hiveOutputFolder}</span><span class="sxs-lookup"><span data-stu-id="786fe-241">${hiveOutputFolder}</span></span></td><td><span data-ttu-id="786fe-242">Výstupní složky pro příkaz Hive vložit PŘEPSAT.</span><span class="sxs-lookup"><span data-stu-id="786fe-242">The output folder for the Hive INSERT OVERWRITE statement.</span></span> <span data-ttu-id="786fe-243">Toto je stejné složce, Sqoop export (export-dir).</span><span class="sxs-lookup"><span data-stu-id="786fe-243">This is the same folder for the Sqoop export (export-dir).</span></span></td></tr>
    </table>

    <span data-ttu-id="786fe-244">Další informace o pracovním postupu Oozie a pomocí akce pracovního postupu najdete v tématu [dokumentaci Apache Oozie 4.0] [ apache-oozie-400] (u clusteru HDInsight verze 3.0) nebo [dokumentaci Apache Oozie 3.3.2] [ apache-oozie-332] (u clusteru HDInsight verze 2.1).</span><span class="sxs-lookup"><span data-stu-id="786fe-244">For more information about Oozie workflow and using the workflow actions, see [Apache Oozie 4.0 documentation][apache-oozie-400] (for HDInsight cluster version 3.0) or [Apache Oozie 3.3.2 documentation][apache-oozie-332] (for HDInsight cluster version 2.1).</span></span>

1. <span data-ttu-id="786fe-245">Uložte soubor jako **C:\Tutorials\UseOozie\workflow.xml** pomocí kódování ANSI (ASCII).</span><span class="sxs-lookup"><span data-stu-id="786fe-245">Save the file as **C:\Tutorials\UseOozie\workflow.xml** by using ANSI (ASCII) encoding.</span></span> <span data-ttu-id="786fe-246">(Použijte Poznámkový blok, pokud textového editoru neposkytuje tuto možnost.)</span><span class="sxs-lookup"><span data-stu-id="786fe-246">(Use Notepad if your text editor doesn't provide this option.)</span></span>

<span data-ttu-id="786fe-247">**Chcete-li definovat coordinator**</span><span class="sxs-lookup"><span data-stu-id="786fe-247">**To define coordinator**</span></span>

1. <span data-ttu-id="786fe-248">Vytvořte textový soubor s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="786fe-248">Create a text file with the following content:</span></span>

    ```xml
    <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
        <action>
            <workflow>
                <app-path>${wfPath}</app-path>
            </workflow>
        </action>
    </coordinator-app>
    ```

    <span data-ttu-id="786fe-249">Existují pět proměnné používané v definičním souboru:</span><span class="sxs-lookup"><span data-stu-id="786fe-249">There are five variables used in the definition file:</span></span>

   | <span data-ttu-id="786fe-250">Proměnná</span><span class="sxs-lookup"><span data-stu-id="786fe-250">Variable</span></span> | <span data-ttu-id="786fe-251">Popis</span><span class="sxs-lookup"><span data-stu-id="786fe-251">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="786fe-252">${coordFrequency}</span><span class="sxs-lookup"><span data-stu-id="786fe-252">${coordFrequency}</span></span> |<span data-ttu-id="786fe-253">Čas pozastavení úlohy.</span><span class="sxs-lookup"><span data-stu-id="786fe-253">Job pause time.</span></span> <span data-ttu-id="786fe-254">Frekvence je vždy vyjádřené v minutách.</span><span class="sxs-lookup"><span data-stu-id="786fe-254">Frequency is always expressed in minutes.</span></span> |
   | <span data-ttu-id="786fe-255">${coordStart}</span><span class="sxs-lookup"><span data-stu-id="786fe-255">${coordStart}</span></span> |<span data-ttu-id="786fe-256">Čas spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="786fe-256">Job start time.</span></span> |
   | <span data-ttu-id="786fe-257">${coordEnd}</span><span class="sxs-lookup"><span data-stu-id="786fe-257">${coordEnd}</span></span> |<span data-ttu-id="786fe-258">Čas ukončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="786fe-258">Job end time.</span></span> |
   | <span data-ttu-id="786fe-259">${coordTimezone}</span><span class="sxs-lookup"><span data-stu-id="786fe-259">${coordTimezone}</span></span> |<span data-ttu-id="786fe-260">Oozie zpracovává koordinátor úlohy v pevné časové pásmo s žádné letní čas (obvykle vyjádřený pomocí UTC).</span><span class="sxs-lookup"><span data-stu-id="786fe-260">Oozie processes coordinator jobs in a fixed time zone with no daylight saving time (typically represented by using UTC).</span></span> <span data-ttu-id="786fe-261">Toto časové pásmo se označuje jako "Oozie zpracování časové pásmo."</span><span class="sxs-lookup"><span data-stu-id="786fe-261">This time zone is referred as the "Oozie processing timezone."</span></span> |
   | <span data-ttu-id="786fe-262">${wfPath}</span><span class="sxs-lookup"><span data-stu-id="786fe-262">${wfPath}</span></span> |<span data-ttu-id="786fe-263">Cesta pro workflow.xml.</span><span class="sxs-lookup"><span data-stu-id="786fe-263">The path for the workflow.xml.</span></span>  <span data-ttu-id="786fe-264">Pokud název souboru pracovního postupu není výchozí název souboru (workflow.xml), je nutné zadat.</span><span class="sxs-lookup"><span data-stu-id="786fe-264">If the workflow file name is not the default file name (workflow.xml), you must specify it.</span></span> |
2. <span data-ttu-id="786fe-265">Uložte soubor jako **C:\Tutorials\UseOozie\coordinator.xml** pomocí kódování ANSI (ASCII).</span><span class="sxs-lookup"><span data-stu-id="786fe-265">Save the file as **C:\Tutorials\UseOozie\coordinator.xml** by using the ANSI (ASCII) encoding.</span></span> <span data-ttu-id="786fe-266">(Použijte Poznámkový blok, pokud textového editoru neposkytuje tuto možnost.)</span><span class="sxs-lookup"><span data-stu-id="786fe-266">(Use Notepad if your text editor doesn't provide this option.)</span></span>

## <a name="deploy-the-oozie-project-and-prepare-the-tutorial"></a><span data-ttu-id="786fe-267">Nasazení projektu Oozie a připravte kurz</span><span class="sxs-lookup"><span data-stu-id="786fe-267">Deploy the Oozie project and prepare the tutorial</span></span>
<span data-ttu-id="786fe-268">Bude spustit skript prostředí Azure PowerShell k těmto činnostem:</span><span class="sxs-lookup"><span data-stu-id="786fe-268">You will run an Azure PowerShell script to perform the following:</span></span>

* <span data-ttu-id="786fe-269">Zkopírujte skript HiveQL (useoozie.hql) do úložiště objektů Blob v Azure, wasb:///tutorials/useoozie/useoozie.hql.</span><span class="sxs-lookup"><span data-stu-id="786fe-269">Copy the HiveQL script (useoozie.hql) to Azure Blob storage, wasb:///tutorials/useoozie/useoozie.hql.</span></span>
* <span data-ttu-id="786fe-270">Zkopírujte workflow.xml wasb:///tutorials/useoozie/workflow.xml.</span><span class="sxs-lookup"><span data-stu-id="786fe-270">Copy workflow.xml to wasb:///tutorials/useoozie/workflow.xml.</span></span>
* <span data-ttu-id="786fe-271">Zkopírujte coordinator.xml wasb:///tutorials/useoozie/coordinator.xml.</span><span class="sxs-lookup"><span data-stu-id="786fe-271">Copy coordinator.xml to wasb:///tutorials/useoozie/coordinator.xml.</span></span>
* <span data-ttu-id="786fe-272">Zkopírujte datový soubor (nebo example/data/sample.log) k wasb:///tutorials/useoozie/data/sample.log.</span><span class="sxs-lookup"><span data-stu-id="786fe-272">Copy the data file (/example/data/sample.log) to wasb:///tutorials/useoozie/data/sample.log.</span></span>
* <span data-ttu-id="786fe-273">Vytvoření tabulky databáze Azure SQL pro ukládání dat export Sqoop.</span><span class="sxs-lookup"><span data-stu-id="786fe-273">Create an Azure SQL database table for storing Sqoop export data.</span></span> <span data-ttu-id="786fe-274">Název tabulky je *log4jLogCount*.</span><span class="sxs-lookup"><span data-stu-id="786fe-274">The table name is *log4jLogCount*.</span></span>

<span data-ttu-id="786fe-275">**Pochopení úložiště HDInsight**</span><span class="sxs-lookup"><span data-stu-id="786fe-275">**Understand HDInsight storage**</span></span>

<span data-ttu-id="786fe-276">HDInsight používá úložiště objektů Blob v Azure pro úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="786fe-276">HDInsight uses Azure Blob storage for data storage.</span></span> <span data-ttu-id="786fe-277">wasb: / / je implementace systému souborů Hadoop distributed (HDFS) v Azure Blob storage společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="786fe-277">wasb:// is Microsoft's implementation of the Hadoop distributed file system (HDFS) in Azure Blob storage.</span></span> <span data-ttu-id="786fe-278">Další informace najdete v tématu [použití Azure Blob storage s HDInsight][hdinsight-storage].</span><span class="sxs-lookup"><span data-stu-id="786fe-278">For more information, see [Use Azure Blob storage with HDInsight][hdinsight-storage].</span></span>

<span data-ttu-id="786fe-279">Při zřizování clusteru služby HDInsight, účet úložiště objektů Blob v Azure a konkrétní kontejner z daného účtu je určený jako výchozí systém souborů, jako v HDFS.</span><span class="sxs-lookup"><span data-stu-id="786fe-279">When you provision an HDInsight cluster, an Azure Blob storage account and a specific container from that account is designated as the default file system, like in HDFS.</span></span> <span data-ttu-id="786fe-280">Kromě tohoto účtu úložiště můžete přidat další účty úložiště ze stejného předplatného Azure nebo z různých předplatných Azure během procesu zřizování.</span><span class="sxs-lookup"><span data-stu-id="786fe-280">In addition to this storage account, you can add additional storage accounts from the same Azure subscription or from different Azure subscriptions during the provisioning process.</span></span> <span data-ttu-id="786fe-281">Pokyny o přidání dalších účtů úložiště najdete v tématu [zřizování clusterů HDInsight][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="786fe-281">For instructions about adding additional storage accounts, see [Provision HDInsight clusters][hdinsight-provision].</span></span> <span data-ttu-id="786fe-282">Pro zjednodušení skript prostředí PowerShell Azure používá v tomto kurzu, všechny soubory jsou uložené ve výchozím kontejneru systému soubor nacházející se v */kurzy/useoozie*.</span><span class="sxs-lookup"><span data-stu-id="786fe-282">To simplify the Azure PowerShell script used in this tutorial, all of the files are stored in the default file system container located at */tutorials/useoozie*.</span></span> <span data-ttu-id="786fe-283">Ve výchozím nastavení tento kontejner má stejný název jako název clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="786fe-283">By default, this container has the same name as the HDInsight cluster name.</span></span>
<span data-ttu-id="786fe-284">Syntaxe je:</span><span class="sxs-lookup"><span data-stu-id="786fe-284">The syntax is:</span></span>

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [!NOTE]
> <span data-ttu-id="786fe-285">Pouze *wasb: / /* syntaxe je podporován v clusteru HDInsight verze 3.0.</span><span class="sxs-lookup"><span data-stu-id="786fe-285">Only the *wasb://* syntax is supported in HDInsight cluster version 3.0.</span></span> <span data-ttu-id="786fe-286">Starší *asv: / /* syntaxe je podporován v HDInsight 2.1 a 1.6 clustery, ale není podporována v clusterech HDInsight 3.0.</span><span class="sxs-lookup"><span data-stu-id="786fe-286">The older *asv://* syntax is supported in HDInsight 2.1 and 1.6 clusters, but it is not supported in HDInsight 3.0 clusters.</span></span>
>
> <span data-ttu-id="786fe-287">Wasb: / / cesta je virtuální cesta.</span><span class="sxs-lookup"><span data-stu-id="786fe-287">The wasb:// path is a virtual path.</span></span> <span data-ttu-id="786fe-288">Další informace najdete v části [použití Azure Blob storage s HDInsight][hdinsight-storage].</span><span class="sxs-lookup"><span data-stu-id="786fe-288">For more information see [Use Azure Blob storage with HDInsight][hdinsight-storage].</span></span>

<span data-ttu-id="786fe-289">Soubor, který je uložený ve výchozím kontejneru systému souborů je přístupný z HDInsight pomocí některé z následujících identifikátory URI (používám workflow.xml jako příklad):</span><span class="sxs-lookup"><span data-stu-id="786fe-289">A file that is stored in the default file system container can be accessed from HDInsight by using any of the following URIs (I am using workflow.xml as an example):</span></span>

    wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasb:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

<span data-ttu-id="786fe-290">Pokud chcete získat přístup k souboru přímo z účtu úložiště, je název objektu blob pro soubor:</span><span class="sxs-lookup"><span data-stu-id="786fe-290">If you want to access the file directly from the storage account, the blob name for the file is:</span></span>

    tutorials/useoozie/workflow.xml

<span data-ttu-id="786fe-291">**Pochopení interních a externích tabulek Hive**</span><span class="sxs-lookup"><span data-stu-id="786fe-291">**Understand Hive internal and external tables**</span></span>

<span data-ttu-id="786fe-292">Existuje několik věcí, které potřebujete vědět o vnitřních a vnějších tabulek Hive:</span><span class="sxs-lookup"><span data-stu-id="786fe-292">There are a few things you need to know about Hive internal and external tables:</span></span>

* <span data-ttu-id="786fe-293">Příkaz CREATE TABLE vytvoří interní tabulku, také známé jako spravované tabulku.</span><span class="sxs-lookup"><span data-stu-id="786fe-293">The CREATE TABLE command creates an internal table, also known as a managed table.</span></span> <span data-ttu-id="786fe-294">Datový soubor se musí nacházet ve výchozím kontejneru.</span><span class="sxs-lookup"><span data-stu-id="786fe-294">The data file must be located in the default container.</span></span>
* <span data-ttu-id="786fe-295">Příkaz CREATE TABLE přesune soubor dat /hive/skladu /<TableName> složky ve výchozím kontejneru.</span><span class="sxs-lookup"><span data-stu-id="786fe-295">The CREATE TABLE command moves the data file to the /hive/warehouse/<TableName> folder in the default container.</span></span>
* <span data-ttu-id="786fe-296">Příkaz CREATE TABLE externí vytvoří externí tabulku.</span><span class="sxs-lookup"><span data-stu-id="786fe-296">The CREATE EXTERNAL TABLE command creates an external table.</span></span> <span data-ttu-id="786fe-297">Datový soubor může být umístěn mimo výchozí kontejner.</span><span class="sxs-lookup"><span data-stu-id="786fe-297">The data file can be located outside the default container.</span></span>
* <span data-ttu-id="786fe-298">Příkaz CREATE TABLE externí nepřesouvá datového souboru.</span><span class="sxs-lookup"><span data-stu-id="786fe-298">The CREATE EXTERNAL TABLE command does not move the data file.</span></span>
* <span data-ttu-id="786fe-299">Příkaz CREATE TABLE externí neumožňuje všechny podsložky složky, která je zadána v klauzuli umístění.</span><span class="sxs-lookup"><span data-stu-id="786fe-299">The CREATE EXTERNAL TABLE command doesn't allow any subfolders under the folder that is specified in the LOCATION clause.</span></span> <span data-ttu-id="786fe-300">To je důvod, proč tento kurz vytvoří kopii souboru sample.log.</span><span class="sxs-lookup"><span data-stu-id="786fe-300">This is the reason why the tutorial makes a copy of the sample.log file.</span></span>

<span data-ttu-id="786fe-301">Další informace najdete v tématu [HDInsight: Hive interní a externí tabulky ÚVOD][cindygross-hive-tables].</span><span class="sxs-lookup"><span data-stu-id="786fe-301">For more information, see [HDInsight: Hive Internal and External Tables Intro][cindygross-hive-tables].</span></span>

<span data-ttu-id="786fe-302">**Pro přípravu kurzu**</span><span class="sxs-lookup"><span data-stu-id="786fe-302">**To prepare the tutorial**</span></span>

1. <span data-ttu-id="786fe-303">Otevřete Windows PowerShell ISE (na obrazovce Start systému Windows 8 zadejte **PowerShell_ISE**a potom klikněte na **Windows PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="786fe-303">Open the Windows PowerShell ISE (in the Windows 8 Start screen, type **PowerShell_ISE**, and then click **Windows PowerShell ISE**.</span></span> <span data-ttu-id="786fe-304">Další informace najdete v tématu [spuštění prostředí Windows PowerShell ve Windows 8 a Windows][powershell-start]).</span><span class="sxs-lookup"><span data-stu-id="786fe-304">For more information, see [Start Windows PowerShell on Windows 8 and Windows][powershell-start]).</span></span>
2. <span data-ttu-id="786fe-305">V dolním podokně spusťte následující příkaz pro připojení k předplatnému Azure:</span><span class="sxs-lookup"><span data-stu-id="786fe-305">In the bottom pane, run the following command to connect to your Azure subscription:</span></span>

    ```powershell
    Add-AzureAccount
    ```

    <span data-ttu-id="786fe-306">Zobrazí se výzva k zadání přihlašovacích údajů účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="786fe-306">You will be prompted to enter your Azure account credentials.</span></span> <span data-ttu-id="786fe-307">Tato metoda přidávání připojení předplatného vyprší, a po 12 hodinách, budete muset znovu spustit rutinu.</span><span class="sxs-lookup"><span data-stu-id="786fe-307">This method of adding a subscription connection times out, and after 12 hours, you will have to run the cmdlet again.</span></span>

   > [!NOTE]
   > <span data-ttu-id="786fe-308">Pokud máte víc předplatných Azure a výchozí předplatné není ta, kterou chcete použít, použijte <strong>Select-AzureSubscription</strong> rutiny vyberte předplatné.</span><span class="sxs-lookup"><span data-stu-id="786fe-308">If you have multiple Azure subscriptions and the default subscription is not the one you want to use, use the <strong>Select-AzureSubscription</strong> cmdlet to select a subscription.</span></span>

3. <span data-ttu-id="786fe-309">Zkopírujte následující skript do podokna Skript a potom nastavte prvních šesti proměnné:</span><span class="sxs-lookup"><span data-stu-id="786fe-309">Copy the following script into the script pane, and then set the first six variables:</span></span>

    ```powershell
    # WASB variables
    $storageAccountName = "<StorageAccountName>"
    $containerName = "<BlobStorageContainerName>"

    # SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    # Oozie files for the tutorial
    $hiveQLScript = "C:\Tutorials\UseOozie\useooziewf.hql"
    $workflowDefinition = "C:\Tutorials\UseOozie\workflow.xml"
    $coordDefinition =  "C:\Tutorials\UseOozie\coordinator.xml"

    # WASB folder for storing the Oozie tutorial files.
    $destFolder = "tutorials/useoozie"  # Do NOT use the long path here
    ```

    <span data-ttu-id="786fe-310">Další popis proměnných najdete v tématu [požadavky](#prerequisites) v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="786fe-310">For more descriptions of the variables, see the [Prerequisites](#prerequisites) section in this tutorial.</span></span>

4. <span data-ttu-id="786fe-311">Připojte následující skript, v podokně skriptu:</span><span class="sxs-lookup"><span data-stu-id="786fe-311">Append the following to the script in the script pane:</span></span>

    ```powershell
    # Create a storage context object
    $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

    function uploadOozieFiles()
    {
        Write-Host "Copy HiveQL script, workflow definition and coordinator definition ..." -ForegroundColor Green
        Set-AzureStorageBlobContent -File $hiveQLScript -Container $containerName -Blob "$destFolder/useooziewf.hql" -Context $destContext
        Set-AzureStorageBlobContent -File $workflowDefinition -Container $containerName -Blob "$destFolder/workflow.xml" -Context $destContext
        Set-AzureStorageBlobContent -File $coordDefinition -Container $containerName -Blob "$destFolder/coordinator.xml" -Context $destContext
    }

    function prepareHiveDataFile()
    {
        Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green
        Start-CopyAzureStorageBlob -SrcContainer $containerName -SrcBlob "example/data/sample.log" -Context $destContext -DestContainer $containerName -destBlob "$destFolder/data/sample.log" -DestContext $destContext
    }

    function prepareSQLDatabase()
    {
        # SQL query string for creating log4jLogsCount table
        $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                [Level] [nvarchar](10) NOT NULL,
                [Total] float,
            CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
            (
            [Level] ASC
            )
            )"

        #Create the log4jLogsCount table
        Write-Host "Create Log4jLogsCount table ..." -ForegroundColor Green
        $conn = New-Object System.Data.SqlClient.SqlConnection
        $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
        $conn.open()
        $cmd = New-Object System.Data.SqlClient.SqlCommand
        $cmd.connection = $conn
        $cmd.commandtext = $cmdCreateLog4jCountTable
        $cmd.executenonquery()

        $conn.close()
    }

    # upload workflow.xml, coordinator.xml, and ooziewf.hql
    uploadOozieFiles;

    # make a copy of example/data/sample.log to example/data/log4j/sample.log
    prepareHiveDataFile;

    # create log4jlogsCount table on SQL database
    prepareSQLDatabase;
    ```

5. <span data-ttu-id="786fe-312">Klikněte na tlačítko **spustit skript** nebo stiskněte klávesu **F5** pro spuštění skriptu.</span><span class="sxs-lookup"><span data-stu-id="786fe-312">Click **Run Script** or press **F5** to run the script.</span></span> <span data-ttu-id="786fe-313">Výstup bude vypadat podobně jako:</span><span class="sxs-lookup"><span data-stu-id="786fe-313">The output will be similar to:</span></span>

    ![Kurz přípravy výstup][img-preparation-output]

## <a name="run-the-oozie-project"></a><span data-ttu-id="786fe-315">Spusťte projekt Oozie</span><span class="sxs-lookup"><span data-stu-id="786fe-315">Run the Oozie project</span></span>
<span data-ttu-id="786fe-316">Prostředí Azure PowerShell aktuálně neposkytuje žádné rutiny pro definování Oozie úloh.</span><span class="sxs-lookup"><span data-stu-id="786fe-316">Azure PowerShell currently doesn't provide any cmdlets for defining Oozie jobs.</span></span> <span data-ttu-id="786fe-317">Můžete použít **Invoke-RestMethod** rutiny k vyvolání Oozie webové služby.</span><span class="sxs-lookup"><span data-stu-id="786fe-317">You can use the **Invoke-RestMethod** cmdlet to invoke Oozie web services.</span></span> <span data-ttu-id="786fe-318">Oozie webového rozhraní API služby je JSON rozhraní HTTP REST API.</span><span class="sxs-lookup"><span data-stu-id="786fe-318">The Oozie web services API is a HTTP REST JSON API.</span></span> <span data-ttu-id="786fe-319">Další informace o rozhraní API Oozie webových služeb najdete v tématu [dokumentaci Apache Oozie 4.0] [ apache-oozie-400] (u clusteru HDInsight verze 3.0) nebo [dokumentaci Apache Oozie 3.3.2] [ apache-oozie-332] (u clusteru HDInsight verze 2.1).</span><span class="sxs-lookup"><span data-stu-id="786fe-319">For more information about the Oozie web services API, see [Apache Oozie 4.0 documentation][apache-oozie-400] (for HDInsight cluster version 3.0) or [Apache Oozie 3.3.2 documentation][apache-oozie-332] (for HDInsight cluster version 2.1).</span></span>

<span data-ttu-id="786fe-320">**Odeslat úlohu Oozie**</span><span class="sxs-lookup"><span data-stu-id="786fe-320">**To submit an Oozie job**</span></span>

1. <span data-ttu-id="786fe-321">Otevřete Windows PowerShell ISE (na obrazovce Start systému Windows 8, zadejte **PowerShell_ISE**a potom klikněte na **Windows PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="786fe-321">Open the Windows PowerShell ISE (in Windows 8 Start screen, type **PowerShell_ISE**, and then click **Windows PowerShell ISE**.</span></span> <span data-ttu-id="786fe-322">Další informace najdete v tématu [spuštění prostředí Windows PowerShell ve Windows 8 a Windows][powershell-start]).</span><span class="sxs-lookup"><span data-stu-id="786fe-322">For more information, see [Start Windows PowerShell on Windows 8 and Windows][powershell-start]).</span></span>
2. <span data-ttu-id="786fe-323">Zkopírujte následující skript do podokna Skript a potom nastavte nejprve čtrnáct proměnné (však přeskočit **$storageUri**).</span><span class="sxs-lookup"><span data-stu-id="786fe-323">Copy the following script into the script pane, and then set the first fourteen variables (however, skip **$storageUri**).</span></span>

    ```powershell
    #HDInsight cluster variables
    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterUserPassword>"

    #Azure Blob storage (WASB) variables
    $storageAccountName = "<StorageAccountName>"
    $storageContainerName = "<BlobContainerName>"
    $storageUri="wasb://$storageContainerName@$storageAccountName.blob.core.windows.net"

    #Azure SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "<SQLDatabaseloginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"

    #Oozie WF/coordinator variables
    $coordStart = "2014-03-21T13:45Z"
    $coordEnd = "2014-03-21T13:45Z"
    $coordFrequency = "1440"    # in minutes, 24h x 60m = 1440m
    $coordTimezone = "UTC"    #UTC/GMT

    $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
    $waitTimeBetweenOozieJobStatusCheck=10

    #Hive action variables
    $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
    $hiveTableName = "log4jlogs"
    $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
    $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

    #Sqoop action variables
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServer.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServer;password=$sqlDatabaseLoginPassword;database=$sqlDatabaseName"
    $sqlDatabaseTableName = "log4jLogsCount"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)
    ```

    <span data-ttu-id="786fe-324">Další popis proměnných najdete v tématu [požadavky](#prerequisites) v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="786fe-324">For more descriptions of the variables, see the [Prerequisites](#prerequisites) section in this tutorial.</span></span>

    <span data-ttu-id="786fe-325">$coordstart a $coordend jsou pracovního postupu počáteční a koncový čas.</span><span class="sxs-lookup"><span data-stu-id="786fe-325">$coordstart and $coordend are the workflow starting and ending time.</span></span> <span data-ttu-id="786fe-326">Chcete-li zjistit čas UTC nebo GMT, hledejte "čas utc" na vyhledávače bing.com. Jak často v minutách, kterou chcete spustit pracovní postup je $coordFrequency.</span><span class="sxs-lookup"><span data-stu-id="786fe-326">To find out the UTC/GMT time, search "utc time" on bing.com. The $coordFrequency is how often in minutes you want to run the workflow.</span></span>
3. <span data-ttu-id="786fe-327">Připojte následující skript.</span><span class="sxs-lookup"><span data-stu-id="786fe-327">Append the following to the script.</span></span> <span data-ttu-id="786fe-328">Tato část definuje Oozie datové části:</span><span class="sxs-lookup"><span data-stu-id="786fe-328">This part defines the Oozie payload:</span></span>

    ```powershell
    #OoziePayload used for Oozie web service submission
    $OoziePayload =  @"
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>

        <property>
            <name>nameNode</name>
            <value>$storageUrI</value>
        </property>

        <property>
            <name>jobTracker</name>
            <value>jobtrackerhost:9010</value>
        </property>

        <property>
            <name>queueName</name>
            <value>default</value>
        </property>

        <property>
            <name>oozie.use.system.libpath</name>
            <value>true</value>
        </property>

        <property>
            <name>oozie.coord.application.path</name>
            <value>$oozieWFPath</value>
        </property>

        <property>
            <name>wfPath</name>
            <value>$oozieWFPath</value>
        </property>

        <property>
            <name>coordStart</name>
            <value>$coordStart</value>
        </property>

        <property>
            <name>coordEnd</name>
            <value>$coordEnd</value>
        </property>

        <property>
            <name>coordFrequency</name>
            <value>$coordFrequency</value>
        </property>

        <property>
            <name>coordTimezone</name>
            <value>$coordTimezone</value>
        </property>

        <property>
            <name>hiveScript</name>
            <value>$hiveScript</value>
        </property>

        <property>
            <name>hiveTableName</name>
            <value>$hiveTableName</value>
        </property>

        <property>
            <name>hiveDataFolder</name>
            <value>$hiveDataFolder</value>
        </property>

        <property>
            <name>hiveOutputFolder</name>
            <value>$hiveOutputFolder</value>
        </property>

        <property>
            <name>sqlDatabaseConnectionString</name>
            <value>&quot;$sqlDatabaseConnectionString&quot;</value>
        </property>

        <property>
            <name>sqlDatabaseTableName</name>
            <value>$SQLDatabaseTableName</value>
        </property>

        <property>
            <name>user.name</name>
            <value>admin</value>
        </property>

    </configuration>
    "@
    ```

   > [!NOTE]
   > <span data-ttu-id="786fe-329">Hlavní rozdíl ve srovnání se souborem datové části odeslání pracovního postupu je proměnná **oozie.coord.application.path**.</span><span class="sxs-lookup"><span data-stu-id="786fe-329">The major difference compared to the workflow submission payload file is the variable **oozie.coord.application.path**.</span></span> <span data-ttu-id="786fe-330">Při odesílání úlohy pracovního postupu použijete **oozie.wf.application.path** místo.</span><span class="sxs-lookup"><span data-stu-id="786fe-330">When you submit a workflow job, you use **oozie.wf.application.path** instead.</span></span>

4. <span data-ttu-id="786fe-331">Připojte následující skript.</span><span class="sxs-lookup"><span data-stu-id="786fe-331">Append the following to the script.</span></span> <span data-ttu-id="786fe-332">Tato část kontroluje stav Oozie webové služby:</span><span class="sxs-lookup"><span data-stu-id="786fe-332">This part checks the Oozie web service status:</span></span>

    ```powershell
    function checkOozieServerStatus()
    {
        Write-Host "Checking Oozie server status..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/admin/status"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds -OutVariable $OozieServerStatus

        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $oozieServerSatus = $jsonResponse[0].("systemMode")
        Write-Host "Oozie server status is $oozieServerSatus..."

        if($oozieServerSatus -notmatch "NORMAL")
        {
            Write-Host "Oozie server status is $oozieServerSatus...cannot submit Oozie jobs. Check the server status and re-run the job."
            exit 1
        }
    }
    ```

5. <span data-ttu-id="786fe-333">Připojte následující skript.</span><span class="sxs-lookup"><span data-stu-id="786fe-333">Append the following to the script.</span></span> <span data-ttu-id="786fe-334">Tato část vytvoří úlohu Oozie:</span><span class="sxs-lookup"><span data-stu-id="786fe-334">This part creates an Oozie job:</span></span>

    ```powershell
    function createOozieJob()
    {
        # create Oozie job
        Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
        Write-Host "`n--------`n$OoziePayload`n--------"
        $clusterUriCreateJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
        $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $creds -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName -debug -Verbose

        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $oozieJobId = $jsonResponse[0].("id")
        Write-Host "Oozie job id is $oozieJobId..."

        return $oozieJobId
    }
    ```

   > [!NOTE]
   > <span data-ttu-id="786fe-335">Při odesílání úlohy pracovního postupu, je nutné provést volání spustíte úlohu po vytvoření úlohy jiné webové služby.</span><span class="sxs-lookup"><span data-stu-id="786fe-335">When you submit a workflow job, you must make another web service call to start the job after the job is created.</span></span> <span data-ttu-id="786fe-336">V takovém případě se aktivuje úlohu koordinátora čas.</span><span class="sxs-lookup"><span data-stu-id="786fe-336">In this case, the coordinator job is triggered by time.</span></span> <span data-ttu-id="786fe-337">Úloha se spustí automaticky.</span><span class="sxs-lookup"><span data-stu-id="786fe-337">The job will start automatically.</span></span>

6. <span data-ttu-id="786fe-338">Připojte následující skript.</span><span class="sxs-lookup"><span data-stu-id="786fe-338">Append the following to the script.</span></span> <span data-ttu-id="786fe-339">Tato část kontroluje stav úlohy Oozie:</span><span class="sxs-lookup"><span data-stu-id="786fe-339">This part checks the Oozie job status:</span></span>

    ```powershell
    function checkOozieJobStatus($oozieJobId)
    {
        # get job status
        Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
        Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

        Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
        $clusterUriGetJobStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $JobStatus = $jsonResponse[0].("status")

        while($JobStatus -notmatch "SUCCEEDED|KILLED")
        {
            Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
            Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $JobStatus = $jsonResponse[0].("status")
        }

        Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!"
        if($JobStatus -notmatch "SUCCEEDED")
        {
            Write-Host "Check logs at http://headnode0:9014/cluster for detais."
            exit -1
        }
    }
    ```

7. <span data-ttu-id="786fe-340">(Volitelné) Připojte následující skript.</span><span class="sxs-lookup"><span data-stu-id="786fe-340">(Optional) Append the following to the script.</span></span>

    ```powershell
    function listOozieJobs()
    {
        Write-Host "Listing Oozie jobs..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds

        write-host "Job ID                                   App Name        Status      Started                         Ended"
        write-host "----------------------------------------------------------------------------------------------------------------------------------"
        foreach($job in $response.workflows)
        {
            Write-Host $job.id "`t" $job.appName "`t" $job.status "`t" $job.startTime "`t" $job.endTime
        }
    }

    function ShowOozieJobLog($oozieJobId)
    {
        Write-Host "Showing Oozie job info..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/$oozieJobId" + "?show=log"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds
        write-host $response
    }

    function killOozieJob($oozieJobId)
    {
        Write-Host "Killing the Oozie job $oozieJobId..." -ForegroundColor Green
        $clusterUriStartJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=kill" #Valid values for the 'action' parameter are 'start', 'suspend', 'resume', 'kill', 'dryrun', 'rerun', and 'change'.
        $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $creds | Format-Table -HideTableHeaders -debug
    }
    ```

8. <span data-ttu-id="786fe-341">Připojte následující skript:</span><span class="sxs-lookup"><span data-stu-id="786fe-341">Append the following to the script:</span></span>

    ```powershell
    checkOozieServerStatus
    # listOozieJobs
    $oozieJobId = createOozieJob($oozieJobId)
    checkOozieJobStatus($oozieJobId)
    # ShowOozieJobLog($oozieJobId)
    # killOozieJob($oozieJobId)
    ```

    <span data-ttu-id="786fe-342">Pokud chcete spustit další funkce, odeberte znak #.</span><span class="sxs-lookup"><span data-stu-id="786fe-342">Remove the # signs if you want to run the additional functions.</span></span>
9. <span data-ttu-id="786fe-343">Pokud váš clusteru HDinsight verze 2.1, nahraďte "https://$clusterName.azurehdinsight.net:443/oozie/v2/" s "https://$clusterName.azurehdinsight.net:443/oozie/v1/".</span><span class="sxs-lookup"><span data-stu-id="786fe-343">If your HDinsight cluster is version 2.1, replace "https://$clusterName.azurehdinsight.net:443/oozie/v2/" with "https://$clusterName.azurehdinsight.net:443/oozie/v1/".</span></span> <span data-ttu-id="786fe-344">Verze clusteru HDInsight 2.1 nemá podporuje verze 2 webové služby.</span><span class="sxs-lookup"><span data-stu-id="786fe-344">HDInsight cluster version 2.1 does not supports version 2 of the web services.</span></span>
10. <span data-ttu-id="786fe-345">Klikněte na tlačítko **spustit skript** nebo stiskněte klávesu **F5** pro spuštění skriptu.</span><span class="sxs-lookup"><span data-stu-id="786fe-345">Click **Run Script** or press **F5** to run the script.</span></span> <span data-ttu-id="786fe-346">Výstup bude vypadat podobně jako:</span><span class="sxs-lookup"><span data-stu-id="786fe-346">The output will be similar to:</span></span>

     ![Kurz spustit výstup pracovního postupu][img-runworkflow-output]
11. <span data-ttu-id="786fe-348">Připojte k vaší databázi SQL zobrazíte exportovaná data.</span><span class="sxs-lookup"><span data-stu-id="786fe-348">Connect to your SQL Database to see the exported data.</span></span>

<span data-ttu-id="786fe-349">**Zkontrolujte protokol chyb úlohy**</span><span class="sxs-lookup"><span data-stu-id="786fe-349">**To check the job error log**</span></span>

<span data-ttu-id="786fe-350">Chcete-li vyřešit pracovního postupu, Oozie souboru protokolu najdete tady C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log z headnode clusteru.</span><span class="sxs-lookup"><span data-stu-id="786fe-350">To troubleshoot a workflow, the Oozie log file can be found at C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log from the cluster headnode.</span></span> <span data-ttu-id="786fe-351">Informace o protokolu RDP, najdete v části [clusterů HDInsight správa pomocí portálu Azure][hdinsight-admin-portal].</span><span class="sxs-lookup"><span data-stu-id="786fe-351">For information on RDP, see [Administering HDInsight clusters using the Azure portal][hdinsight-admin-portal].</span></span>

<span data-ttu-id="786fe-352">**Spusťte znovu tohoto kurzu**</span><span class="sxs-lookup"><span data-stu-id="786fe-352">**To rerun the tutorial**</span></span>

<span data-ttu-id="786fe-353">Chcete-li znovu spustit pracovní postup, musíte provést následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="786fe-353">To rerun the workflow, you must perform the following tasks:</span></span>

* <span data-ttu-id="786fe-354">Odstraňte výstupní soubor skriptu Hive.</span><span class="sxs-lookup"><span data-stu-id="786fe-354">Delete the Hive script output file.</span></span>
* <span data-ttu-id="786fe-355">Odstraníte data v tabulce log4jLogsCount.</span><span class="sxs-lookup"><span data-stu-id="786fe-355">Delete the data in the log4jLogsCount table.</span></span>

<span data-ttu-id="786fe-356">Tady je ukázkový skript prostředí Windows PowerShell, který můžete použít:</span><span class="sxs-lookup"><span data-stu-id="786fe-356">Here is a sample Windows PowerShell script that you can use:</span></span>

```powershell
$storageAccountName = "<AzureStorageAccountName>"
$containerName = "<ContainerName>"

#SQL database variables
$sqlDatabaseServer = "<SQLDatabaseServerName>"
$sqlDatabaseLogin = "<SQLDatabaseLoginName>"
$sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>"
$sqlDatabaseName = "<SQLDatabaseName>"
$sqlDatabaseTableName = "log4jLogsCount"

Write-host "Delete the Hive script output file ..." -ForegroundColor Green
$storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
$destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
$conn = New-Object System.Data.SqlClient.SqlConnection
$conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
$conn.open()
$cmd = New-Object System.Data.SqlClient.SqlCommand
$cmd.connection = $conn
$cmd.commandtext = "delete from $sqlDatabaseTableName"
$cmd.executenonquery()

$conn.close()
```

## <a name="next-steps"></a><span data-ttu-id="786fe-357">Další kroky</span><span class="sxs-lookup"><span data-stu-id="786fe-357">Next steps</span></span>
<span data-ttu-id="786fe-358">V tomto kurzu jste se dozvěděli, jak definovat pracovním postupu Oozie a Oozie coordinator a jak spustit úlohu Oozie coordinator pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="786fe-358">In this tutorial, you learned how to define an Oozie workflow and an Oozie coordinator, and how to run an Oozie coordinator job by using Azure PowerShell.</span></span> <span data-ttu-id="786fe-359">Další informace naleznete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="786fe-359">To learn more, see the following articles:</span></span>

* <span data-ttu-id="786fe-360">[Začínáme s HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="786fe-360">[Get started with HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="786fe-361">[Používat úložiště objektů Azure Blob s HDInsight][hdinsight-storage]</span><span class="sxs-lookup"><span data-stu-id="786fe-361">[Use Azure Blob storage with HDInsight][hdinsight-storage]</span></span>
* <span data-ttu-id="786fe-362">[Spravovat HDInsight pomocí prostředí Azure PowerShell][hdinsight-admin-powershell]</span><span class="sxs-lookup"><span data-stu-id="786fe-362">[Administer HDInsight by using Azure PowerShell][hdinsight-admin-powershell]</span></span>
* <span data-ttu-id="786fe-363">[Nahrání dat do služby HDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="786fe-363">[Upload data to HDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="786fe-364">[Použití nástroje Sqoop s HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="786fe-364">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>
* <span data-ttu-id="786fe-365">[Použití Hivu se službou HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="786fe-365">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="786fe-366">[Použití Pigu se službou HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="786fe-366">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="786fe-367">[Vývoj aplikací Java MapReduce pro HDInsight][hdinsight-develop-java-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="786fe-367">[Develop Java MapReduce programs for HDInsight][hdinsight-develop-java-mapreduce]</span></span>

[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-develop-java-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.RunCoord.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
