---
title: "v prostředí HDInsight založené na čase aaaUse Hadoop Oozie coordinator | Microsoft Docs"
description: "V prostředí HDInsight, Cloudová služba velkých dat pomocí Hadoop Oozie coordinator založené na čase. Zjistěte, jak toodefine Oozie pracovní postupy a koordinátory a odesílání úloh."
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
ms.openlocfilehash: aecbb5ee94a4234d1a7768bdb6de2a33508b1e4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-time-based-oozie-coordinator-with-hadoop-in-hdinsight-toodefine-workflows-and-coordinate-jobs"></a><span data-ttu-id="33818-104">Použití založené na čase Oozie coordinator se systémem Hadoop v HDInsight toodefine pracovních a koordinovat úlohy</span><span class="sxs-lookup"><span data-stu-id="33818-104">Use time-based Oozie coordinator with Hadoop in HDInsight toodefine workflows and coordinate jobs</span></span>
<span data-ttu-id="33818-105">V tomto článku se dozvíte, jak toodefine pracovní postupy a koordinátoři a jak tootrigger hello koordinátor úlohy podle času.</span><span class="sxs-lookup"><span data-stu-id="33818-105">In this article, you'll learn how toodefine workflows and coordinators, and how tootrigger hello coordinator jobs, based on time.</span></span> <span data-ttu-id="33818-106">Je užitečné toogo prostřednictvím [Oozie použití s HDInsight] [ hdinsight-use-oozie] před přečtěte si tento článek.</span><span class="sxs-lookup"><span data-stu-id="33818-106">It is helpful toogo through [Use Oozie with HDInsight][hdinsight-use-oozie] before you read this article.</span></span> <span data-ttu-id="33818-107">Kromě toho tooOozie, můžete také naplánovat úlohy pomocí Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="33818-107">In addition tooOozie, you can also schedule jobs using Azure Data Factory.</span></span> <span data-ttu-id="33818-108">toolearn Azure Data Factory najdete v části [použijte Pig a Hive pomocí služby Data Factory](../data-factory/data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="33818-108">toolearn Azure Data Factory, see [Use Pig and Hive with Data Factory](../data-factory/data-factory-data-transformation-activities.md).</span></span>

> [!NOTE]
> <span data-ttu-id="33818-109">Tento článek vyžaduje cluster HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="33818-109">This article requires a Windows-based HDInsight cluster.</span></span> <span data-ttu-id="33818-110">Informace o používání Oozie, včetně úloh založené na čase, v clusteru se systémem Linux naleznete v části [Oozie použití s Hadoop toodefine a spuštění pracovního postupu na HDInsight se systémem Linux](hdinsight-use-oozie-linux-mac.md)</span><span class="sxs-lookup"><span data-stu-id="33818-110">For information on using Oozie, including time-based jobs, on a Linux-based cluster, see [Use Oozie with Hadoop toodefine and run a workflow on Linux-based HDInsight](hdinsight-use-oozie-linux-mac.md)</span></span>

## <a name="what-is-oozie"></a><span data-ttu-id="33818-111">Co je Oozie</span><span class="sxs-lookup"><span data-stu-id="33818-111">What is Oozie</span></span>
<span data-ttu-id="33818-112">Apache Oozie je pracovní postup nebo koordinaci systém, který spravuje úloh Hadoop.</span><span class="sxs-lookup"><span data-stu-id="33818-112">Apache Oozie is a workflow/coordination system that manages Hadoop jobs.</span></span> <span data-ttu-id="33818-113">Je integrován se hello zásobníku Hadoop a podporuje úloh Hadoop pro Apache MapReduce, Apache Pig, Apache Hive a Apache Sqoop.</span><span class="sxs-lookup"><span data-stu-id="33818-113">It is integrated with hello Hadoop stack, and it supports Hadoop jobs for Apache MapReduce, Apache Pig, Apache Hive, and Apache Sqoop.</span></span> <span data-ttu-id="33818-114">Lze také použít tooschedule úlohy, které jsou specifické tooa systému, například programy v jazyce Java nebo skripty prostředí.</span><span class="sxs-lookup"><span data-stu-id="33818-114">It can also be used tooschedule jobs that are specific tooa system, such as Java programs or shell scripts.</span></span>

<span data-ttu-id="33818-115">Hello následující obrázek ukazuje pracovní postup hello, který implementujete:</span><span class="sxs-lookup"><span data-stu-id="33818-115">hello following image shows hello workflow you will implement:</span></span>

![Diagram pracovního postupu][img-workflow-diagram]

<span data-ttu-id="33818-117">pracovní postup Hello obsahuje dvě akce:</span><span class="sxs-lookup"><span data-stu-id="33818-117">hello workflow contains two actions:</span></span>

1. <span data-ttu-id="33818-118">Akce Hive spouští hello toocount skript HiveQL výskyty každý typ úroveň protokolu v souboru protokolu log4j.</span><span class="sxs-lookup"><span data-stu-id="33818-118">A Hive action runs a HiveQL script toocount hello occurrences of each log-level type in a log4j log file.</span></span> <span data-ttu-id="33818-119">Každý log4j protokol se skládá z řádku pole, které obsahuje [úroveň protokolu] pole tooshow hello typu a hello závažnost, například:</span><span class="sxs-lookup"><span data-stu-id="33818-119">Each log4j log consists of a line of fields that contains a [LOG LEVEL] field tooshow hello type and hello severity, for example:</span></span>

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    <span data-ttu-id="33818-120">je podobná Hello výstup skriptu Hive:</span><span class="sxs-lookup"><span data-stu-id="33818-120">hello Hive script output is similar to:</span></span>

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    <span data-ttu-id="33818-121">Další informace o Hivu najdete v tématu [Použití Hivu se službou HDInsight][hdinsight-use-hive].</span><span class="sxs-lookup"><span data-stu-id="33818-121">For more information about Hive, see [Use Hive with HDInsight][hdinsight-use-hive].</span></span>
2. <span data-ttu-id="33818-122">Akce Sqoop exportuje hello HiveQL akce výstupní tooa tabulku v databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="33818-122">A Sqoop action exports hello HiveQL action output tooa table in an Azure SQL database.</span></span> <span data-ttu-id="33818-123">Další informace o Sqoop najdete v tématu [Sqoop použití s HDInsight][hdinsight-use-sqoop].</span><span class="sxs-lookup"><span data-stu-id="33818-123">For more information about Sqoop, see [Use Sqoop with HDInsight][hdinsight-use-sqoop].</span></span>

> [!NOTE]
> <span data-ttu-id="33818-124">Podporované verze Oozie v clusterech prostředí HDInsight najdete v tématu [co je nového ve verzích clusterů hello poskytovaných v HDInsight?] [hdinsight-versions].</span><span class="sxs-lookup"><span data-stu-id="33818-124">For supported Oozie versions on HDInsight clusters, see [What's new in hello cluster versions provided by HDInsight?][hdinsight-versions].</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="33818-125">Požadavky</span><span class="sxs-lookup"><span data-stu-id="33818-125">Prerequisites</span></span>
<span data-ttu-id="33818-126">Než začnete tento kurz, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="33818-126">Before you begin this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="33818-127">**Pracovní stanice s prostředím Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="33818-127">**A workstation with Azure PowerShell**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="33818-128">Podpora prostředí Azure PowerShell pro správu prostředků služby HDInsight pomocí Azure Service Manageru je **zastaralá** a 1. ledna 2017 dojde k jejímu odebrání.</span><span class="sxs-lookup"><span data-stu-id="33818-128">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and will be removed by January 1, 2017.</span></span> <span data-ttu-id="33818-129">kroky Hello, v tento dokument použít hello nové rutiny služby HDInsight, které fungují s Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="33818-129">hello steps in this document use hello new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="33818-130">Postupujte podle kroků hello v [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello nejnovější verzi prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="33818-130">Please follow hello steps in [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="33818-131">Pokud máte skripty, že toobe potřeba upravit hello toouse nové se rutiny, které pracují s Azure Resource Managerem najdete v tématu [tooAzure migrace založené na správci prostředků vývoj nástroje pro clustery služby HDInsight](hdinsight-hadoop-development-using-azure-resource-manager.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="33818-131">If you have scripts that need toobe modified toouse hello new cmdlets that work with Azure Resource Manager, see [Migrating tooAzure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

* <span data-ttu-id="33818-132">**Cluster služby HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="33818-132">**An HDInsight cluster**.</span></span> <span data-ttu-id="33818-133">Informace o vytváření clusteru služby HDInsight najdete v tématu [Tvorba clusterů HDInsight][hdinsight-provision], nebo [Začínáme s HDInsight][hdinsight-get-started].</span><span class="sxs-lookup"><span data-stu-id="33818-133">For information about creating an HDInsight cluster, see [Create HDInsight clusters][hdinsight-provision], or [Get started with HDInsight][hdinsight-get-started].</span></span> <span data-ttu-id="33818-134">Hello následující toogo data prostřednictvím hello kurzu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="33818-134">You will need hello following data toogo through hello tutorial:</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="33818-135">Vlastnost clusteru</span><span class="sxs-lookup"><span data-stu-id="33818-135">Cluster property</span></span></th><th><span data-ttu-id="33818-136">Název proměnné prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="33818-136">Windows PowerShell variable name</span></span></th><th><span data-ttu-id="33818-137">Hodnota</span><span class="sxs-lookup"><span data-stu-id="33818-137">Value</span></span></th><th><span data-ttu-id="33818-138">Popis</span><span class="sxs-lookup"><span data-stu-id="33818-138">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="33818-139">Název clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="33818-139">HDInsight cluster name</span></span></td><td><span data-ttu-id="33818-140">$clusterName</span><span class="sxs-lookup"><span data-stu-id="33818-140">$clusterName</span></span></td><td></td><td><span data-ttu-id="33818-141">cluster HDInsight Hello, na kterém budete spouštět v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="33818-141">hello HDInsight cluster on which you will run this tutorial.</span></span></td></tr>
    <tr><td><span data-ttu-id="33818-142">Uživatelské jméno clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="33818-142">HDInsight cluster username</span></span></td><td><span data-ttu-id="33818-143">$clusterUsername</span><span class="sxs-lookup"><span data-stu-id="33818-143">$clusterUsername</span></span></td><td></td><td><span data-ttu-id="33818-144">Hello HDInsight clusteru uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="33818-144">hello HDInsight cluster user name.</span></span> </td></tr>
    <tr><td><span data-ttu-id="33818-145">Heslo uživatele clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="33818-145">HDInsight cluster user password</span></span> </td><td><span data-ttu-id="33818-146">$clusterPassword</span><span class="sxs-lookup"><span data-stu-id="33818-146">$clusterPassword</span></span></td><td></td><td><span data-ttu-id="33818-147">Hello heslo uživatele clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="33818-147">hello HDInsight cluster user password.</span></span></td></tr>
    <tr><td><span data-ttu-id="33818-148">Název účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="33818-148">Azure storage account name</span></span></td><td><span data-ttu-id="33818-149">$storageAccountName</span><span class="sxs-lookup"><span data-stu-id="33818-149">$storageAccountName</span></span></td><td></td><td><span data-ttu-id="33818-150">Azure Storage účet k dispozici toohello clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="33818-150">An Azure Storage account available toohello HDInsight cluster.</span></span> <span data-ttu-id="33818-151">V tomto kurzu použijte hello výchozí úložiště účet, který jste zadali během procesu zřizování clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="33818-151">For this tutorial, use hello default storage account that you specified during hello cluster provision process.</span></span></td></tr>
    <tr><td><span data-ttu-id="33818-152">Název kontejneru Azure Blob</span><span class="sxs-lookup"><span data-stu-id="33818-152">Azure Blob container name</span></span></td><td><span data-ttu-id="33818-153">$containerName</span><span class="sxs-lookup"><span data-stu-id="33818-153">$containerName</span></span></td><td></td><td><span data-ttu-id="33818-154">V tomto příkladu použijte hello kontejner úložiště objektů Blob v Azure, který se používá pro hello výchozí systém souborů clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="33818-154">For this example, use hello Azure Blob storage container that is used for hello default HDInsight cluster file system.</span></span> <span data-ttu-id="33818-155">Ve výchozím nastavení má stejný název jako hello HDInsight cluster hello.</span><span class="sxs-lookup"><span data-stu-id="33818-155">By default, it has hello same name as hello HDInsight cluster.</span></span></td></tr>
    </table><span data-ttu-id="33818-156">
* **Azure SQL database**.</span><span class="sxs-lookup"><span data-stu-id="33818-156">
* **An Azure SQL database**.</span></span> <span data-ttu-id="33818-157">Je nutné nakonfigurovat pravidlo brány firewall pro přístup k databázi SQL serveru tooallow hello z pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="33818-157">You must configure a firewall rule for hello SQL Database server tooallow access from your workstation.</span></span> <span data-ttu-id="33818-158">Pokyny týkající se vytváření databáze Azure SQL a konfiguraci brány firewall hello najdete v tématu [začít používat Azure SQL database] [databáze_sql get-started].</span><span class="sxs-lookup"><span data-stu-id="33818-158">For instructions about creating an Azure SQL database and configuring hello firewall, see [Get started using Azure SQL database][sqldatabase-get-started].</span></span> <span data-ttu-id="33818-159">Tento článek obsahuje skript prostředí Windows PowerShell pro vytvoření tabulky databáze Azure SQL hello, které potřebujete pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="33818-159">This article provides a Windows PowerShell script for creating hello Azure SQL database table that you need for this tutorial.</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="33818-160">Vlastnost databáze SQL</span><span class="sxs-lookup"><span data-stu-id="33818-160">SQL database property</span></span></th><th><span data-ttu-id="33818-161">Název proměnné prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="33818-161">Windows PowerShell variable name</span></span></th><th><span data-ttu-id="33818-162">Hodnota</span><span class="sxs-lookup"><span data-stu-id="33818-162">Value</span></span></th><th><span data-ttu-id="33818-163">Popis</span><span class="sxs-lookup"><span data-stu-id="33818-163">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="33818-164">Název databáze serveru SQL</span><span class="sxs-lookup"><span data-stu-id="33818-164">SQL database server name</span></span></td><td><span data-ttu-id="33818-165">$sqlDatabaseServer</span><span class="sxs-lookup"><span data-stu-id="33818-165">$sqlDatabaseServer</span></span></td><td></td><td><span data-ttu-id="33818-166">Hello SQL databáze serveru toowhich Sqoop bude exportovat data.</span><span class="sxs-lookup"><span data-stu-id="33818-166">hello SQL database server toowhich Sqoop will export data.</span></span> </td></tr>
    <tr><td><span data-ttu-id="33818-167">Přihlašovací jméno SQL databáze</span><span class="sxs-lookup"><span data-stu-id="33818-167">SQL database login name</span></span></td><td><span data-ttu-id="33818-168">$sqlDatabaseLogin</span><span class="sxs-lookup"><span data-stu-id="33818-168">$sqlDatabaseLogin</span></span></td><td></td><td><span data-ttu-id="33818-169">Přihlašovací jméno SQL Database.</span><span class="sxs-lookup"><span data-stu-id="33818-169">SQL Database login name.</span></span></td></tr>
    <tr><td><span data-ttu-id="33818-170">Heslo pro přihlášení databáze SQL</span><span class="sxs-lookup"><span data-stu-id="33818-170">SQL database login password</span></span></td><td><span data-ttu-id="33818-171">$sqlDatabaseLoginPassword</span><span class="sxs-lookup"><span data-stu-id="33818-171">$sqlDatabaseLoginPassword</span></span></td><td></td><td><span data-ttu-id="33818-172">Heslo přihlášení k databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="33818-172">SQL Database login password.</span></span></td></tr>
    <tr><td><span data-ttu-id="33818-173">Název databáze SQL</span><span class="sxs-lookup"><span data-stu-id="33818-173">SQL database name</span></span></td><td><span data-ttu-id="33818-174">$sqlDatabaseName</span><span class="sxs-lookup"><span data-stu-id="33818-174">$sqlDatabaseName</span></span></td><td></td><td><span data-ttu-id="33818-175">Hello Azure SQL database toowhich Sqoop bude exportovat data.</span><span class="sxs-lookup"><span data-stu-id="33818-175">hello Azure SQL database toowhich Sqoop will export data.</span></span> </td></tr>
    </table>

  > [!NOTE]
  > <span data-ttu-id="33818-176">Ve výchozím nastavení Azure SQL database umožňuje připojení z Azure služby, jako je Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="33818-176">By default an Azure SQL database allows connections from Azure Services, such as Azure HDInsight.</span></span> <span data-ttu-id="33818-177">Pokud toto nastavení brány firewall je zakázáno, musíte ji povolit z portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="33818-177">If this firewall setting is disabled, you must enable it from hello Azure Portal.</span></span> <span data-ttu-id="33818-178">Pokyny týkající se vytváření databáze SQL a konfigurace pravidel brány firewall, najdete v části [vytvořit a nakonfigurovat databázi SQL][sqldatabase-get-started].</span><span class="sxs-lookup"><span data-stu-id="33818-178">For instruction about creating a SQL Database and configuring firewall rules, see [Create and Configure SQL Database][sqldatabase-get-started].</span></span>

> [!NOTE]
> <span data-ttu-id="33818-179">Vyplňování hello hodnoty v tabulkách hello.</span><span class="sxs-lookup"><span data-stu-id="33818-179">Fill-in hello values in hello tables.</span></span> <span data-ttu-id="33818-180">Je užitečné při procházení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="33818-180">It will be helpful for going through this tutorial.</span></span>

## <a name="define-oozie-workflow-and-hello-related-hiveql-script"></a><span data-ttu-id="33818-181">Definice pracovního postupu Oozie a hello související skript HiveQL</span><span class="sxs-lookup"><span data-stu-id="33818-181">Define Oozie workflow and hello related HiveQL script</span></span>
<span data-ttu-id="33818-182">Definice Oozie pracovní postupy jsou zapsány ve hPDL (jazyka definice proces XML).</span><span class="sxs-lookup"><span data-stu-id="33818-182">Oozie workflows definitions are written in hPDL (an XML process definition language).</span></span> <span data-ttu-id="33818-183">Hello výchozí název souboru pracovního postupu je *workflow.xml*.</span><span class="sxs-lookup"><span data-stu-id="33818-183">hello default workflow file name is *workflow.xml*.</span></span>  <span data-ttu-id="33818-184">Budete uložte místně soubor hello pracovního postupu a poté ji nasadit toohello clusteru HDInsight pomocí prostředí Azure PowerShell později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="33818-184">You will save hello workflow file locally, and then deploy it toohello HDInsight cluster by using Azure PowerShell later in this tutorial.</span></span>

<span data-ttu-id="33818-185">Hello Hive akce v pracovním postupu hello volá soubor skriptu HiveQL.</span><span class="sxs-lookup"><span data-stu-id="33818-185">hello Hive action in hello workflow calls a HiveQL script file.</span></span> <span data-ttu-id="33818-186">Tento soubor skriptu obsahuje tři příkazy HiveQL:</span><span class="sxs-lookup"><span data-stu-id="33818-186">This script file contains three HiveQL statements:</span></span>

1. <span data-ttu-id="33818-187">**příkaz DROP TABLE Hello** odstranění hello tabulku Hive log4j, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="33818-187">**hello DROP TABLE statement** deletes hello log4j Hive table if it exists.</span></span>
2. <span data-ttu-id="33818-188">**příkaz CREATE TABLE Hello** vytvoří log4j externí tabulku Hive, který ukazuje toohello umístění souboru protokolu log4j hello;</span><span class="sxs-lookup"><span data-stu-id="33818-188">**hello CREATE TABLE statement** creates a log4j Hive external table, which points toohello location of hello log4j log file;</span></span>
3. <span data-ttu-id="33818-189">**umístění souboru protokolu log4j hello Hello**.</span><span class="sxs-lookup"><span data-stu-id="33818-189">**hello location of hello log4j log file**.</span></span> <span data-ttu-id="33818-190">Oddělovač polí Hello je ",".</span><span class="sxs-lookup"><span data-stu-id="33818-190">hello field delimiter is ",".</span></span> <span data-ttu-id="33818-191">oddělovač řádku výchozí Hello je "\n".</span><span class="sxs-lookup"><span data-stu-id="33818-191">hello default line delimiter is "\n".</span></span> <span data-ttu-id="33818-192">Externí tabulku Hive je použité tooavoid hello datový soubor odebírán z hello původního umístění, v případě, že má toorun hello Oozie pracovní vícekrát.</span><span class="sxs-lookup"><span data-stu-id="33818-192">A Hive external table is used tooavoid hello data file being removed from hello original location, in case you want toorun hello Oozie workflow multiple times.</span></span>
4. <span data-ttu-id="33818-193">**Hello vložit PŘEPSAT příkaz** počty hello výskyty každý typ úroveň protokolu z hello log4j tabulku Hive a ukládá umístění úložiště objektů Blob v Azure tooan výstup hello.</span><span class="sxs-lookup"><span data-stu-id="33818-193">**hello INSERT OVERWRITE statement** counts hello occurrences of each log-level type from hello log4j Hive table, and it saves hello output tooan Azure Blob storage location.</span></span>

> [!NOTE]
> <span data-ttu-id="33818-194">Je známý problém cesta Hive.</span><span class="sxs-lookup"><span data-stu-id="33818-194">There is a known Hive path issue.</span></span> <span data-ttu-id="33818-195">Budete spouštět na tento problém při odesílání úlohu Oozie.</span><span class="sxs-lookup"><span data-stu-id="33818-195">You will run into this problem when submitting an Oozie job.</span></span> <span data-ttu-id="33818-196">Hello pokyny k opravě problému hello najdete na webu TechNet Wiki hello: [HDInsight Hive Chyba: nelze toorename][technetwiki-hive-error].</span><span class="sxs-lookup"><span data-stu-id="33818-196">hello instructions for fixing hello issue can be found on hello TechNet Wiki: [HDInsight Hive error: Unable toorename][technetwiki-hive-error].</span></span>

<span data-ttu-id="33818-197">**toodefine hello HiveQL skriptu souboru toobe volá pracovní postup hello**</span><span class="sxs-lookup"><span data-stu-id="33818-197">**toodefine hello HiveQL script file toobe called by hello workflow**</span></span>

1. <span data-ttu-id="33818-198">Vytvořte textový soubor s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="33818-198">Create a text file with hello following content:</span></span>

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    <span data-ttu-id="33818-199">Existují tři proměnné používané ve skriptu hello:</span><span class="sxs-lookup"><span data-stu-id="33818-199">There are three variables used in hello script:</span></span>

   * <span data-ttu-id="33818-200">${hiveTableName}</span><span class="sxs-lookup"><span data-stu-id="33818-200">${hiveTableName}</span></span>
   * <span data-ttu-id="33818-201">${hiveDataFolder}</span><span class="sxs-lookup"><span data-stu-id="33818-201">${hiveDataFolder}</span></span>
   * <span data-ttu-id="33818-202">${hiveOutputFolder}</span><span class="sxs-lookup"><span data-stu-id="33818-202">${hiveOutputFolder}</span></span>

     <span data-ttu-id="33818-203">Soubor definice pracovního postupu Hello (workflow.xml v tomto kurzu) předá tyto hodnoty toothis skript HiveQL v době běhu.</span><span class="sxs-lookup"><span data-stu-id="33818-203">hello workflow definition file (workflow.xml in this tutorial) will pass these values toothis HiveQL script at run time.</span></span>
2. <span data-ttu-id="33818-204">Uložte soubor hello jako **C:\Tutorials\UseOozie\useooziewf.hql** pomocí kódování ANSI (ASCII).</span><span class="sxs-lookup"><span data-stu-id="33818-204">Save hello file as **C:\Tutorials\UseOozie\useooziewf.hql** by using ANSI (ASCII) encoding.</span></span> <span data-ttu-id="33818-205">(Použijte Poznámkový blok, pokud textového editoru neposkytuje tuto možnost.) Tento soubor skriptu bude cluster HDInsight nasazené toohello později v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="33818-205">(Use Notepad if your text editor doesn't provide this option.) This script file will be deployed toohello HDInsight cluster later in hello tutorial.</span></span>

<span data-ttu-id="33818-206">**toodefine pracovního postupu**</span><span class="sxs-lookup"><span data-stu-id="33818-206">**toodefine a workflow**</span></span>

1. <span data-ttu-id="33818-207">Vytvořte textový soubor s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="33818-207">Create a text file with hello following content:</span></span>

    ```xml
    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start too= "RunHiveScript"/>

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

    <span data-ttu-id="33818-208">Existují dvě akce definované v pracovním postupu hello.</span><span class="sxs-lookup"><span data-stu-id="33818-208">There are two actions defined in hello workflow.</span></span> <span data-ttu-id="33818-209">je Hello start-tooaction *RunHiveScript*.</span><span class="sxs-lookup"><span data-stu-id="33818-209">hello start-tooaction is *RunHiveScript*.</span></span> <span data-ttu-id="33818-210">Pokud spuštění akce hello *OK*, je další akce hello *RunSqoopExport*.</span><span class="sxs-lookup"><span data-stu-id="33818-210">If hello action runs *OK*, hello next action is *RunSqoopExport*.</span></span>

    <span data-ttu-id="33818-211">Hello RunHiveScript má několik proměnné.</span><span class="sxs-lookup"><span data-stu-id="33818-211">hello RunHiveScript has several variables.</span></span> <span data-ttu-id="33818-212">Při odesílání úlohy Oozie hello z pracovní stanice pomocí prostředí Azure PowerShell, projdou hello hodnoty.</span><span class="sxs-lookup"><span data-stu-id="33818-212">You will pass hello values when you submit hello Oozie job from your workstation by using Azure PowerShell.</span></span>

    <span data-ttu-id="33818-213">Proměnné pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="33818-213">Workflow variables</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="33818-214">Proměnné pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="33818-214">Workflow variables</span></span></th><th><span data-ttu-id="33818-215">Popis</span><span class="sxs-lookup"><span data-stu-id="33818-215">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="33818-216">${jobTracker}</span><span class="sxs-lookup"><span data-stu-id="33818-216">${jobTracker}</span></span></td><td><span data-ttu-id="33818-217">Zadejte adresu URL hello sledovací modul úlohy Hadoop hello.</span><span class="sxs-lookup"><span data-stu-id="33818-217">Specify hello URL of hello Hadoop job tracker.</span></span> <span data-ttu-id="33818-218">Použití <strong>jobtrackerhost:9010</strong> v HDInsight clusteru verze 3.0 a 2.0.</span><span class="sxs-lookup"><span data-stu-id="33818-218">Use <strong>jobtrackerhost:9010</strong> on HDInsight cluster version 3.0 and 2.0.</span></span></td></tr>
    <tr><td><span data-ttu-id="33818-219">${nameNode}</span><span class="sxs-lookup"><span data-stu-id="33818-219">${nameNode}</span></span></td><td><span data-ttu-id="33818-220">Zadejte adresu URL hello hello Hadoop název uzlu.</span><span class="sxs-lookup"><span data-stu-id="33818-220">Specify hello URL of hello Hadoop name node.</span></span> <span data-ttu-id="33818-221">Použít hello výchozí soubor systému wasb: / / adres, například <i>wasb: / /&lt;containerName&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</span><span class="sxs-lookup"><span data-stu-id="33818-221">Use hello default file system wasb:// address, for example, <i>wasb://&lt;containerName&gt;@&lt;storageAccountName&gt;.blob.core.windows.net</i>.</span></span></td></tr>
    <tr><td><span data-ttu-id="33818-222">${queueName}</span><span class="sxs-lookup"><span data-stu-id="33818-222">${queueName}</span></span></td><td><span data-ttu-id="33818-223">Určuje, že bude odeslána hello název fronty, který hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="33818-223">Specifies hello queue name that hello job will be submitted to.</span></span> <span data-ttu-id="33818-224">Použití <strong>výchozí</strong>.</span><span class="sxs-lookup"><span data-stu-id="33818-224">Use <strong>default</strong>.</span></span></td></tr>
    </table>

    <span data-ttu-id="33818-225">Proměnné akcí v Hive</span><span class="sxs-lookup"><span data-stu-id="33818-225">Hive action variables</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="33818-226">Hive proměnné akce</span><span class="sxs-lookup"><span data-stu-id="33818-226">Hive action variable</span></span></th><th><span data-ttu-id="33818-227">Popis</span><span class="sxs-lookup"><span data-stu-id="33818-227">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="33818-228">${hiveDataFolder}</span><span class="sxs-lookup"><span data-stu-id="33818-228">${hiveDataFolder}</span></span></td><td><span data-ttu-id="33818-229">Hello zdrojový adresář pro hello příkaz Hive Create Table.</span><span class="sxs-lookup"><span data-stu-id="33818-229">hello source directory for hello Hive Create Table command.</span></span></td></tr>
    <tr><td><span data-ttu-id="33818-230">${hiveOutputFolder}</span><span class="sxs-lookup"><span data-stu-id="33818-230">${hiveOutputFolder}</span></span></td><td><span data-ttu-id="33818-231">Hello výstupní složky pro hello příkaz INSERT PŘEPSAT.</span><span class="sxs-lookup"><span data-stu-id="33818-231">hello output folder for hello INSERT OVERWRITE statement.</span></span></td></tr>
    <tr><td><span data-ttu-id="33818-232">${hiveTableName}</span><span class="sxs-lookup"><span data-stu-id="33818-232">${hiveTableName}</span></span></td><td><span data-ttu-id="33818-233">Název Hello hello tabulku Hive, který odkazuje na hello log4j datových souborů.</span><span class="sxs-lookup"><span data-stu-id="33818-233">hello name of hello Hive table that references hello log4j data files.</span></span></td></tr>
    </table>

    <span data-ttu-id="33818-234">Proměnné akcí v Sqoop</span><span class="sxs-lookup"><span data-stu-id="33818-234">Sqoop action variables</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="33818-235">Sqoop proměnné akce</span><span class="sxs-lookup"><span data-stu-id="33818-235">Sqoop action variable</span></span></th><th><span data-ttu-id="33818-236">Popis</span><span class="sxs-lookup"><span data-stu-id="33818-236">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="33818-237">${sqlDatabaseConnectionString}</span><span class="sxs-lookup"><span data-stu-id="33818-237">${sqlDatabaseConnectionString}</span></span></td><td><span data-ttu-id="33818-238">Připojovací řetězec databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="33818-238">SQL Database connection string.</span></span></td></tr>
    <tr><td><span data-ttu-id="33818-239">${sqlDatabaseTableName}</span><span class="sxs-lookup"><span data-stu-id="33818-239">${sqlDatabaseTableName}</span></span></td><td><span data-ttu-id="33818-240">exportován Hello data hello toowhere tabulky v databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="33818-240">hello Azure SQL database table toowhere hello data will be exported.</span></span></td></tr>
    <tr><td><span data-ttu-id="33818-241">${hiveOutputFolder}</span><span class="sxs-lookup"><span data-stu-id="33818-241">${hiveOutputFolder}</span></span></td><td><span data-ttu-id="33818-242">Hello výstupní složky pro hello Hive vložit PŘEPSAT příkaz.</span><span class="sxs-lookup"><span data-stu-id="33818-242">hello output folder for hello Hive INSERT OVERWRITE statement.</span></span> <span data-ttu-id="33818-243">Toto je hello stejné složce, hello Sqoop export (export-dir).</span><span class="sxs-lookup"><span data-stu-id="33818-243">This is hello same folder for hello Sqoop export (export-dir).</span></span></td></tr>
    </table>

    <span data-ttu-id="33818-244">Další informace o pracovním postupu Oozie a použití hello akce pracovního postupu najdete v tématu [dokumentaci Apache Oozie 4.0] [ apache-oozie-400] (u clusteru HDInsight verze 3.0) nebo [Apache Oozie 3.3.2 dokumentace] [ apache-oozie-332] (u clusteru HDInsight verze 2.1).</span><span class="sxs-lookup"><span data-stu-id="33818-244">For more information about Oozie workflow and using hello workflow actions, see [Apache Oozie 4.0 documentation][apache-oozie-400] (for HDInsight cluster version 3.0) or [Apache Oozie 3.3.2 documentation][apache-oozie-332] (for HDInsight cluster version 2.1).</span></span>

1. <span data-ttu-id="33818-245">Uložte soubor hello jako **C:\Tutorials\UseOozie\workflow.xml** pomocí kódování ANSI (ASCII).</span><span class="sxs-lookup"><span data-stu-id="33818-245">Save hello file as **C:\Tutorials\UseOozie\workflow.xml** by using ANSI (ASCII) encoding.</span></span> <span data-ttu-id="33818-246">(Použijte Poznámkový blok, pokud textového editoru neposkytuje tuto možnost.)</span><span class="sxs-lookup"><span data-stu-id="33818-246">(Use Notepad if your text editor doesn't provide this option.)</span></span>

<span data-ttu-id="33818-247">**toodefine coordinator**</span><span class="sxs-lookup"><span data-stu-id="33818-247">**toodefine coordinator**</span></span>

1. <span data-ttu-id="33818-248">Vytvořte textový soubor s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="33818-248">Create a text file with hello following content:</span></span>

    ```xml
    <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
        <action>
            <workflow>
                <app-path>${wfPath}</app-path>
            </workflow>
        </action>
    </coordinator-app>
    ```

    <span data-ttu-id="33818-249">Existují pět proměnné používané v souboru definice hello:</span><span class="sxs-lookup"><span data-stu-id="33818-249">There are five variables used in hello definition file:</span></span>

   | <span data-ttu-id="33818-250">Proměnná</span><span class="sxs-lookup"><span data-stu-id="33818-250">Variable</span></span> | <span data-ttu-id="33818-251">Popis</span><span class="sxs-lookup"><span data-stu-id="33818-251">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="33818-252">${coordFrequency}</span><span class="sxs-lookup"><span data-stu-id="33818-252">${coordFrequency}</span></span> |<span data-ttu-id="33818-253">Čas pozastavení úlohy.</span><span class="sxs-lookup"><span data-stu-id="33818-253">Job pause time.</span></span> <span data-ttu-id="33818-254">Frekvence je vždy vyjádřené v minutách.</span><span class="sxs-lookup"><span data-stu-id="33818-254">Frequency is always expressed in minutes.</span></span> |
   | <span data-ttu-id="33818-255">${coordStart}</span><span class="sxs-lookup"><span data-stu-id="33818-255">${coordStart}</span></span> |<span data-ttu-id="33818-256">Čas spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="33818-256">Job start time.</span></span> |
   | <span data-ttu-id="33818-257">${coordEnd}</span><span class="sxs-lookup"><span data-stu-id="33818-257">${coordEnd}</span></span> |<span data-ttu-id="33818-258">Čas ukončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="33818-258">Job end time.</span></span> |
   | <span data-ttu-id="33818-259">${coordTimezone}</span><span class="sxs-lookup"><span data-stu-id="33818-259">${coordTimezone}</span></span> |<span data-ttu-id="33818-260">Oozie zpracovává koordinátor úlohy v pevné časové pásmo s žádné letní čas (obvykle vyjádřený pomocí UTC).</span><span class="sxs-lookup"><span data-stu-id="33818-260">Oozie processes coordinator jobs in a fixed time zone with no daylight saving time (typically represented by using UTC).</span></span> <span data-ttu-id="33818-261">Toto časové pásmo se označuje jako hello "Oozie zpracování časové pásmo."</span><span class="sxs-lookup"><span data-stu-id="33818-261">This time zone is referred as hello "Oozie processing timezone."</span></span> |
   | <span data-ttu-id="33818-262">${wfPath}</span><span class="sxs-lookup"><span data-stu-id="33818-262">${wfPath}</span></span> |<span data-ttu-id="33818-263">Hello cesta pro hello workflow.xml.</span><span class="sxs-lookup"><span data-stu-id="33818-263">hello path for hello workflow.xml.</span></span>  <span data-ttu-id="33818-264">Pokud název souboru pracovního postupu hello není hello výchozí název souboru (workflow.xml), je nutné zadat.</span><span class="sxs-lookup"><span data-stu-id="33818-264">If hello workflow file name is not hello default file name (workflow.xml), you must specify it.</span></span> |
2. <span data-ttu-id="33818-265">Uložte soubor hello jako **C:\Tutorials\UseOozie\coordinator.xml** pomocí kódování ANSI (ASCII) hello.</span><span class="sxs-lookup"><span data-stu-id="33818-265">Save hello file as **C:\Tutorials\UseOozie\coordinator.xml** by using hello ANSI (ASCII) encoding.</span></span> <span data-ttu-id="33818-266">(Použijte Poznámkový blok, pokud textového editoru neposkytuje tuto možnost.)</span><span class="sxs-lookup"><span data-stu-id="33818-266">(Use Notepad if your text editor doesn't provide this option.)</span></span>

## <a name="deploy-hello-oozie-project-and-prepare-hello-tutorial"></a><span data-ttu-id="33818-267">Nasazení projektu Oozie hello a připravte hello kurzu</span><span class="sxs-lookup"><span data-stu-id="33818-267">Deploy hello Oozie project and prepare hello tutorial</span></span>
<span data-ttu-id="33818-268">Budete spouštět prostředí Azure PowerShell skriptu tooperform hello následující:</span><span class="sxs-lookup"><span data-stu-id="33818-268">You will run an Azure PowerShell script tooperform hello following:</span></span>

* <span data-ttu-id="33818-269">Kopírování hello úložiště objektů Blob tooAzure HiveQL skriptu (useoozie.hql), wasb:///tutorials/useoozie/useoozie.hql.</span><span class="sxs-lookup"><span data-stu-id="33818-269">Copy hello HiveQL script (useoozie.hql) tooAzure Blob storage, wasb:///tutorials/useoozie/useoozie.hql.</span></span>
* <span data-ttu-id="33818-270">Zkopírujte workflow.xml toowasb:///tutorials/useoozie/workflow.xml.</span><span class="sxs-lookup"><span data-stu-id="33818-270">Copy workflow.xml toowasb:///tutorials/useoozie/workflow.xml.</span></span>
* <span data-ttu-id="33818-271">Zkopírujte coordinator.xml toowasb:///tutorials/useoozie/coordinator.xml.</span><span class="sxs-lookup"><span data-stu-id="33818-271">Copy coordinator.xml toowasb:///tutorials/useoozie/coordinator.xml.</span></span>
* <span data-ttu-id="33818-272">Kopírování hello datového souboru (/ example/data/sample.log) toowasb:///tutorials/useoozie/data/sample.log.</span><span class="sxs-lookup"><span data-stu-id="33818-272">Copy hello data file (/example/data/sample.log) toowasb:///tutorials/useoozie/data/sample.log.</span></span>
* <span data-ttu-id="33818-273">Vytvoření tabulky databáze Azure SQL pro ukládání dat export Sqoop.</span><span class="sxs-lookup"><span data-stu-id="33818-273">Create an Azure SQL database table for storing Sqoop export data.</span></span> <span data-ttu-id="33818-274">Název tabulky Hello je *log4jLogCount*.</span><span class="sxs-lookup"><span data-stu-id="33818-274">hello table name is *log4jLogCount*.</span></span>

<span data-ttu-id="33818-275">**Pochopení úložiště HDInsight**</span><span class="sxs-lookup"><span data-stu-id="33818-275">**Understand HDInsight storage**</span></span>

<span data-ttu-id="33818-276">HDInsight používá úložiště objektů Blob v Azure pro úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="33818-276">HDInsight uses Azure Blob storage for data storage.</span></span> <span data-ttu-id="33818-277">wasb: / / je implementace systému souborů Hadoop distributed (HDFS) hello v Azure Blob storage společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="33818-277">wasb:// is Microsoft's implementation of hello Hadoop distributed file system (HDFS) in Azure Blob storage.</span></span> <span data-ttu-id="33818-278">Další informace najdete v tématu [použití Azure Blob storage s HDInsight][hdinsight-storage].</span><span class="sxs-lookup"><span data-stu-id="33818-278">For more information, see [Use Azure Blob storage with HDInsight][hdinsight-storage].</span></span>

<span data-ttu-id="33818-279">Při zřizování clusteru služby HDInsight, účet úložiště objektů Blob v Azure a konkrétní kontejner z daného účtu je určený jako hello výchozí systém souborů, jako v HDFS.</span><span class="sxs-lookup"><span data-stu-id="33818-279">When you provision an HDInsight cluster, an Azure Blob storage account and a specific container from that account is designated as hello default file system, like in HDFS.</span></span> <span data-ttu-id="33818-280">Kromě toho toothis účet úložiště, můžete přidat další úložiště účtů z hello stejné předplatné Azure nebo z různých předplatných Azure během procesu zřizování hello.</span><span class="sxs-lookup"><span data-stu-id="33818-280">In addition toothis storage account, you can add additional storage accounts from hello same Azure subscription or from different Azure subscriptions during hello provisioning process.</span></span> <span data-ttu-id="33818-281">Pokyny o přidání dalších účtů úložiště najdete v tématu [zřizování clusterů HDInsight][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="33818-281">For instructions about adding additional storage accounts, see [Provision HDInsight clusters][hdinsight-provision].</span></span> <span data-ttu-id="33818-282">skript prostředí PowerShell Azure hello toosimplify použili v tomto kurzu, všechny soubory se ukládají do kontejneru systému souboru výchozí hello hello umístěné v */kurzy/useoozie*.</span><span class="sxs-lookup"><span data-stu-id="33818-282">toosimplify hello Azure PowerShell script used in this tutorial, all of hello files are stored in hello default file system container located at */tutorials/useoozie*.</span></span> <span data-ttu-id="33818-283">Ve výchozím nastavení má tento kontejner hello stejný název jako název clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="33818-283">By default, this container has hello same name as hello HDInsight cluster name.</span></span>
<span data-ttu-id="33818-284">Hello syntaxe je:</span><span class="sxs-lookup"><span data-stu-id="33818-284">hello syntax is:</span></span>

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [!NOTE]
> <span data-ttu-id="33818-285">Pouze hello *wasb: / /* syntaxe je podporován v clusteru HDInsight verze 3.0.</span><span class="sxs-lookup"><span data-stu-id="33818-285">Only hello *wasb://* syntax is supported in HDInsight cluster version 3.0.</span></span> <span data-ttu-id="33818-286">Hello starší *asv: / /* syntaxe je podporován v HDInsight 2.1 a 1.6 clustery, ale není podporována v clusterech HDInsight 3.0.</span><span class="sxs-lookup"><span data-stu-id="33818-286">hello older *asv://* syntax is supported in HDInsight 2.1 and 1.6 clusters, but it is not supported in HDInsight 3.0 clusters.</span></span>
>
> <span data-ttu-id="33818-287">Hello wasb: / / cesta je virtuální cesta.</span><span class="sxs-lookup"><span data-stu-id="33818-287">hello wasb:// path is a virtual path.</span></span> <span data-ttu-id="33818-288">Další informace najdete v části [použití Azure Blob storage s HDInsight][hdinsight-storage].</span><span class="sxs-lookup"><span data-stu-id="33818-288">For more information see [Use Azure Blob storage with HDInsight][hdinsight-storage].</span></span>

<span data-ttu-id="33818-289">Soubor, který je uložen v kontejneru systému souboru výchozí hello je přístupná z prostředí HDInsight pomocí některé z hello následující identifikátory URI (používám workflow.xml jako příklad):</span><span class="sxs-lookup"><span data-stu-id="33818-289">A file that is stored in hello default file system container can be accessed from HDInsight by using any of hello following URIs (I am using workflow.xml as an example):</span></span>

    wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasb:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

<span data-ttu-id="33818-290">Pokud chcete soubor hello tooaccess přímo z účtu úložiště hello, název objektu blob hello hello souboru je:</span><span class="sxs-lookup"><span data-stu-id="33818-290">If you want tooaccess hello file directly from hello storage account, hello blob name for hello file is:</span></span>

    tutorials/useoozie/workflow.xml

<span data-ttu-id="33818-291">**Pochopení interních a externích tabulek Hive**</span><span class="sxs-lookup"><span data-stu-id="33818-291">**Understand Hive internal and external tables**</span></span>

<span data-ttu-id="33818-292">Existuje několik věcí, které je třeba tooknow o vnitřních a vnějších tabulek Hive:</span><span class="sxs-lookup"><span data-stu-id="33818-292">There are a few things you need tooknow about Hive internal and external tables:</span></span>

* <span data-ttu-id="33818-293">příkaz CREATE TABLE Hello vytvoří interní tabulku, také známé jako spravované tabulku.</span><span class="sxs-lookup"><span data-stu-id="33818-293">hello CREATE TABLE command creates an internal table, also known as a managed table.</span></span> <span data-ttu-id="33818-294">Hello datového souboru se musí nacházet v kontejneru výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="33818-294">hello data file must be located in hello default container.</span></span>
* <span data-ttu-id="33818-295">příkaz CREATE TABLE Hello přesune hello data soubortoohello/hive/skladu/<TableName> složky v hello výchozí kontejner.</span><span class="sxs-lookup"><span data-stu-id="33818-295">hello CREATE TABLE command moves hello data file toohello /hive/warehouse/<TableName> folder in hello default container.</span></span>
* <span data-ttu-id="33818-296">Hello vytvoření externí tabulky příkaz vytvoří externí tabulku.</span><span class="sxs-lookup"><span data-stu-id="33818-296">hello CREATE EXTERNAL TABLE command creates an external table.</span></span> <span data-ttu-id="33818-297">Hello datového souboru může být umístěn mimo výchozí kontejner hello.</span><span class="sxs-lookup"><span data-stu-id="33818-297">hello data file can be located outside hello default container.</span></span>
* <span data-ttu-id="33818-298">příkaz CREATE TABLE externí Hello nepřesouvá hello datový soubor.</span><span class="sxs-lookup"><span data-stu-id="33818-298">hello CREATE EXTERNAL TABLE command does not move hello data file.</span></span>
* <span data-ttu-id="33818-299">příkaz CREATE TABLE externí Hello neumožňuje všechny podsložky hello složky, která je zadána v klauzuli umístění hello.</span><span class="sxs-lookup"><span data-stu-id="33818-299">hello CREATE EXTERNAL TABLE command doesn't allow any subfolders under hello folder that is specified in hello LOCATION clause.</span></span> <span data-ttu-id="33818-300">Toto je hello důvod, proč hello kurzu vytvoří kopii souboru sample.log hello.</span><span class="sxs-lookup"><span data-stu-id="33818-300">This is hello reason why hello tutorial makes a copy of hello sample.log file.</span></span>

<span data-ttu-id="33818-301">Další informace najdete v tématu [HDInsight: Hive interní a externí tabulky ÚVOD][cindygross-hive-tables].</span><span class="sxs-lookup"><span data-stu-id="33818-301">For more information, see [HDInsight: Hive Internal and External Tables Intro][cindygross-hive-tables].</span></span>

<span data-ttu-id="33818-302">**kurz tooprepare hello**</span><span class="sxs-lookup"><span data-stu-id="33818-302">**tooprepare hello tutorial**</span></span>

1. <span data-ttu-id="33818-303">Otevřete hello Windows PowerShell ISE (na obrazovce Start systému Windows 8 hello zadejte **PowerShell_ISE**a potom klikněte na **Windows PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="33818-303">Open hello Windows PowerShell ISE (in hello Windows 8 Start screen, type **PowerShell_ISE**, and then click **Windows PowerShell ISE**.</span></span> <span data-ttu-id="33818-304">Další informace najdete v tématu [spuštění prostředí Windows PowerShell ve Windows 8 a Windows][powershell-start]).</span><span class="sxs-lookup"><span data-stu-id="33818-304">For more information, see [Start Windows PowerShell on Windows 8 and Windows][powershell-start]).</span></span>
2. <span data-ttu-id="33818-305">V dolním podokně hello spusťte následující příkaz tooconnect tooyour předplatného Azure hello:</span><span class="sxs-lookup"><span data-stu-id="33818-305">In hello bottom pane, run hello following command tooconnect tooyour Azure subscription:</span></span>

    ```powershell
    Add-AzureAccount
    ```

    <span data-ttu-id="33818-306">Můžete se výzvami tooenter přihlašovací údaje účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="33818-306">You will be prompted tooenter your Azure account credentials.</span></span> <span data-ttu-id="33818-307">Tato metoda přidávání připojení předplatného vyprší, a po 12 hodinách, bude třeba toorun hello rutinu znovu.</span><span class="sxs-lookup"><span data-stu-id="33818-307">This method of adding a subscription connection times out, and after 12 hours, you will have toorun hello cmdlet again.</span></span>

   > [!NOTE]
   > <span data-ttu-id="33818-308">Pokud máte více předplatných Azure a předplatné výchozí hello není hello ten, který chcete toouse, použijte hello <strong>Select-AzureSubscription</strong> rutiny tooselect předplatné.</span><span class="sxs-lookup"><span data-stu-id="33818-308">If you have multiple Azure subscriptions and hello default subscription is not hello one you want toouse, use hello <strong>Select-AzureSubscription</strong> cmdlet tooselect a subscription.</span></span>

3. <span data-ttu-id="33818-309">Zkopírujte následující skript do podokno skriptu hello hello a pak nastavte hello prvních šesti proměnné:</span><span class="sxs-lookup"><span data-stu-id="33818-309">Copy hello following script into hello script pane, and then set hello first six variables:</span></span>

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

    # Oozie files for hello tutorial
    $hiveQLScript = "C:\Tutorials\UseOozie\useooziewf.hql"
    $workflowDefinition = "C:\Tutorials\UseOozie\workflow.xml"
    $coordDefinition =  "C:\Tutorials\UseOozie\coordinator.xml"

    # WASB folder for storing hello Oozie tutorial files.
    $destFolder = "tutorials/useoozie"  # Do NOT use hello long path here
    ```

    <span data-ttu-id="33818-310">Další popis hello proměnných najdete v tématu hello [požadavky](#prerequisites) v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="33818-310">For more descriptions of hello variables, see hello [Prerequisites](#prerequisites) section in this tutorial.</span></span>

4. <span data-ttu-id="33818-311">Připojte hello následující skript toohello v hello skript:</span><span class="sxs-lookup"><span data-stu-id="33818-311">Append hello following toohello script in hello script pane:</span></span>

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
        Write-Host "Make a copy of hello sample.log file ... " -ForegroundColor Green
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

        #Create hello log4jLogsCount table
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

    # make a copy of example/data/sample.log tooexample/data/log4j/sample.log
    prepareHiveDataFile;

    # create log4jlogsCount table on SQL database
    prepareSQLDatabase;
    ```

5. <span data-ttu-id="33818-312">Klikněte na tlačítko **spustit skript** nebo stiskněte klávesu **F5** toorun hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="33818-312">Click **Run Script** or press **F5** toorun hello script.</span></span> <span data-ttu-id="33818-313">Hello výstup bude vypadat podobně jako:</span><span class="sxs-lookup"><span data-stu-id="33818-313">hello output will be similar to:</span></span>

    ![Kurz přípravy výstup][img-preparation-output]

## <a name="run-hello-oozie-project"></a><span data-ttu-id="33818-315">Spusťte projekt Oozie hello</span><span class="sxs-lookup"><span data-stu-id="33818-315">Run hello Oozie project</span></span>
<span data-ttu-id="33818-316">Prostředí Azure PowerShell aktuálně neposkytuje žádné rutiny pro definování Oozie úloh.</span><span class="sxs-lookup"><span data-stu-id="33818-316">Azure PowerShell currently doesn't provide any cmdlets for defining Oozie jobs.</span></span> <span data-ttu-id="33818-317">Můžete použít hello **Invoke-RestMethod** rutiny tooinvoke Oozie webové služby.</span><span class="sxs-lookup"><span data-stu-id="33818-317">You can use hello **Invoke-RestMethod** cmdlet tooinvoke Oozie web services.</span></span> <span data-ttu-id="33818-318">rozhraní API webových služeb Oozie Hello je JSON rozhraní HTTP REST API.</span><span class="sxs-lookup"><span data-stu-id="33818-318">hello Oozie web services API is a HTTP REST JSON API.</span></span> <span data-ttu-id="33818-319">Další informace o rozhraní API hello Oozie webových služeb najdete v tématu [dokumentaci Apache Oozie 4.0] [ apache-oozie-400] (u clusteru HDInsight verze 3.0) nebo [dokumentaci Apache Oozie 3.3.2] [ apache-oozie-332] (u clusteru HDInsight verze 2.1).</span><span class="sxs-lookup"><span data-stu-id="33818-319">For more information about hello Oozie web services API, see [Apache Oozie 4.0 documentation][apache-oozie-400] (for HDInsight cluster version 3.0) or [Apache Oozie 3.3.2 documentation][apache-oozie-332] (for HDInsight cluster version 2.1).</span></span>

<span data-ttu-id="33818-320">**toosubmit úlohu Oozie**</span><span class="sxs-lookup"><span data-stu-id="33818-320">**toosubmit an Oozie job**</span></span>

1. <span data-ttu-id="33818-321">Otevřete hello Windows PowerShell ISE (na obrazovce Start systému Windows 8, zadejte **PowerShell_ISE**a potom klikněte na **Windows PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="33818-321">Open hello Windows PowerShell ISE (in Windows 8 Start screen, type **PowerShell_ISE**, and then click **Windows PowerShell ISE**.</span></span> <span data-ttu-id="33818-322">Další informace najdete v tématu [spuštění prostředí Windows PowerShell ve Windows 8 a Windows][powershell-start]).</span><span class="sxs-lookup"><span data-stu-id="33818-322">For more information, see [Start Windows PowerShell on Windows 8 and Windows][powershell-start]).</span></span>
2. <span data-ttu-id="33818-323">Kopírování hello následující skript do hello podokno skriptu a pak sadu hello nejprve čtrnáct proměnné (však přeskočit **$storageUri**).</span><span class="sxs-lookup"><span data-stu-id="33818-323">Copy hello following script into hello script pane, and then set hello first fourteen variables (however, skip **$storageUri**).</span></span>

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

    $oozieWFPath="$storageUri/tutorials/useoozie"  # hello default name is workflow.xml. And you don't need toospecify hello file name.
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

    <span data-ttu-id="33818-324">Další popis hello proměnných najdete v tématu hello [požadavky](#prerequisites) v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="33818-324">For more descriptions of hello variables, see hello [Prerequisites](#prerequisites) section in this tutorial.</span></span>

    <span data-ttu-id="33818-325">$coordstart a $coordend jsou hello pracovního postupu počáteční a koncový čas.</span><span class="sxs-lookup"><span data-stu-id="33818-325">$coordstart and $coordend are hello workflow starting and ending time.</span></span> <span data-ttu-id="33818-326">toofind na čas UTC nebo GMT hello hledání "čas utc" na vyhledávače bing.com. Hello $coordFrequency se jak často má toorun hello pracovní minut.</span><span class="sxs-lookup"><span data-stu-id="33818-326">toofind out hello UTC/GMT time, search "utc time" on bing.com. hello $coordFrequency is how often in minutes you want toorun hello workflow.</span></span>
3. <span data-ttu-id="33818-327">Připojí hello následující toohello skriptu.</span><span class="sxs-lookup"><span data-stu-id="33818-327">Append hello following toohello script.</span></span> <span data-ttu-id="33818-328">Tato část definuje datovou část Oozie hello:</span><span class="sxs-lookup"><span data-stu-id="33818-328">This part defines hello Oozie payload:</span></span>

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
   > <span data-ttu-id="33818-329">Hello hlavní rozdíl oproti toohello pracovní postup odesílání datové části souboru je hello proměnná **oozie.coord.application.path**.</span><span class="sxs-lookup"><span data-stu-id="33818-329">hello major difference compared toohello workflow submission payload file is hello variable **oozie.coord.application.path**.</span></span> <span data-ttu-id="33818-330">Při odesílání úlohy pracovního postupu použijete **oozie.wf.application.path** místo.</span><span class="sxs-lookup"><span data-stu-id="33818-330">When you submit a workflow job, you use **oozie.wf.application.path** instead.</span></span>

4. <span data-ttu-id="33818-331">Připojí hello následující toohello skriptu.</span><span class="sxs-lookup"><span data-stu-id="33818-331">Append hello following toohello script.</span></span> <span data-ttu-id="33818-332">Tato část kontroluje stav hello Oozie webové služby:</span><span class="sxs-lookup"><span data-stu-id="33818-332">This part checks hello Oozie web service status:</span></span>

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
            Write-Host "Oozie server status is $oozieServerSatus...cannot submit Oozie jobs. Check hello server status and re-run hello job."
            exit 1
        }
    }
    ```

5. <span data-ttu-id="33818-333">Připojí hello následující toohello skriptu.</span><span class="sxs-lookup"><span data-stu-id="33818-333">Append hello following toohello script.</span></span> <span data-ttu-id="33818-334">Tato část vytvoří úlohu Oozie:</span><span class="sxs-lookup"><span data-stu-id="33818-334">This part creates an Oozie job:</span></span>

    ```powershell
    function createOozieJob()
    {
        # create Oozie job
        Write-Host "Sending hello following Payload toohello cluster:" -ForegroundColor Green
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
   > <span data-ttu-id="33818-335">Při odesílání úlohy pracovního postupu, musíte provést další webové služby volání toostart hello úloha po vytvoření úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="33818-335">When you submit a workflow job, you must make another web service call toostart hello job after hello job is created.</span></span> <span data-ttu-id="33818-336">V takovém případě je spuštěna úloha coordinator hello podle času.</span><span class="sxs-lookup"><span data-stu-id="33818-336">In this case, hello coordinator job is triggered by time.</span></span> <span data-ttu-id="33818-337">Hello úlohy se spustí automaticky.</span><span class="sxs-lookup"><span data-stu-id="33818-337">hello job will start automatically.</span></span>

6. <span data-ttu-id="33818-338">Připojí hello následující toohello skriptu.</span><span class="sxs-lookup"><span data-stu-id="33818-338">Append hello following toohello script.</span></span> <span data-ttu-id="33818-339">Tato část kontroluje stav úlohy Oozie hello:</span><span class="sxs-lookup"><span data-stu-id="33818-339">This part checks hello Oozie job status:</span></span>

    ```powershell
    function checkOozieJobStatus($oozieJobId)
    {
        # get job status
        Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until hello job metadata is populated in hello Oozie metastore..." -ForegroundColor Green
        Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

        Write-Host "Getting job status and waiting for hello job toocomplete..." -ForegroundColor Green
        $clusterUriGetJobStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $JobStatus = $jsonResponse[0].("status")

        while($JobStatus -notmatch "SUCCEEDED|KILLED")
        {
            Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for hello job toocomplete..."
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

7. <span data-ttu-id="33818-340">(Volitelné) Připojí hello následující toohello skriptu.</span><span class="sxs-lookup"><span data-stu-id="33818-340">(Optional) Append hello following toohello script.</span></span>

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
        Write-Host "Killing hello Oozie job $oozieJobId..." -ForegroundColor Green
        $clusterUriStartJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=kill" #Valid values for hello 'action' parameter are 'start', 'suspend', 'resume', 'kill', 'dryrun', 'rerun', and 'change'.
        $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $creds | Format-Table -HideTableHeaders -debug
    }
    ```

8. <span data-ttu-id="33818-341">Připojte hello následující toohello skriptu:</span><span class="sxs-lookup"><span data-stu-id="33818-341">Append hello following toohello script:</span></span>

    ```powershell
    checkOozieServerStatus
    # listOozieJobs
    $oozieJobId = createOozieJob($oozieJobId)
    checkOozieJobStatus($oozieJobId)
    # ShowOozieJobLog($oozieJobId)
    # killOozieJob($oozieJobId)
    ```

    <span data-ttu-id="33818-342">Pokud chcete, aby toorun hello další funkce, odeberte znak # hello.</span><span class="sxs-lookup"><span data-stu-id="33818-342">Remove hello # signs if you want toorun hello additional functions.</span></span>
9. <span data-ttu-id="33818-343">Pokud váš clusteru HDinsight verze 2.1, nahraďte "https://$clusterName.azurehdinsight.net:443/oozie/v2/" s "https://$clusterName.azurehdinsight.net:443/oozie/v1/".</span><span class="sxs-lookup"><span data-stu-id="33818-343">If your HDinsight cluster is version 2.1, replace "https://$clusterName.azurehdinsight.net:443/oozie/v2/" with "https://$clusterName.azurehdinsight.net:443/oozie/v1/".</span></span> <span data-ttu-id="33818-344">Verze clusteru HDInsight 2.1 nemá podporuje verze 2 hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="33818-344">HDInsight cluster version 2.1 does not supports version 2 of hello web services.</span></span>
10. <span data-ttu-id="33818-345">Klikněte na tlačítko **spustit skript** nebo stiskněte klávesu **F5** toorun hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="33818-345">Click **Run Script** or press **F5** toorun hello script.</span></span> <span data-ttu-id="33818-346">Hello výstup bude vypadat podobně jako:</span><span class="sxs-lookup"><span data-stu-id="33818-346">hello output will be similar to:</span></span>

     ![Kurz spustit výstup pracovního postupu][img-runworkflow-output]
11. <span data-ttu-id="33818-348">Připojte tooyour SQL Database toosee hello exportovat data.</span><span class="sxs-lookup"><span data-stu-id="33818-348">Connect tooyour SQL Database toosee hello exported data.</span></span>

<span data-ttu-id="33818-349">**Protokol chyb úlohy toocheck hello**</span><span class="sxs-lookup"><span data-stu-id="33818-349">**toocheck hello job error log**</span></span>

<span data-ttu-id="33818-350">tootroubleshoot pracovního postupu, soubor protokolu Oozie hello naleznete na C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log z clusteru headnode hello.</span><span class="sxs-lookup"><span data-stu-id="33818-350">tootroubleshoot a workflow, hello Oozie log file can be found at C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log from hello cluster headnode.</span></span> <span data-ttu-id="33818-351">Informace o protokolu RDP, najdete v části [hello clusterů HDInsight správa pomocí portálu Azure][hdinsight-admin-portal].</span><span class="sxs-lookup"><span data-stu-id="33818-351">For information on RDP, see [Administering HDInsight clusters using hello Azure portal][hdinsight-admin-portal].</span></span>

<span data-ttu-id="33818-352">**kurz toorerun hello**</span><span class="sxs-lookup"><span data-stu-id="33818-352">**toorerun hello tutorial**</span></span>

<span data-ttu-id="33818-353">pracovní postup hello toorerun, je třeba provést hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="33818-353">toorerun hello workflow, you must perform hello following tasks:</span></span>

* <span data-ttu-id="33818-354">Odstraňte soubor výstup skriptu Hive hello.</span><span class="sxs-lookup"><span data-stu-id="33818-354">Delete hello Hive script output file.</span></span>
* <span data-ttu-id="33818-355">Odstraňte hello data v tabulce log4jLogsCount hello.</span><span class="sxs-lookup"><span data-stu-id="33818-355">Delete hello data in hello log4jLogsCount table.</span></span>

<span data-ttu-id="33818-356">Tady je ukázkový skript prostředí Windows PowerShell, který můžete použít:</span><span class="sxs-lookup"><span data-stu-id="33818-356">Here is a sample Windows PowerShell script that you can use:</span></span>

```powershell
$storageAccountName = "<AzureStorageAccountName>"
$containerName = "<ContainerName>"

#SQL database variables
$sqlDatabaseServer = "<SQLDatabaseServerName>"
$sqlDatabaseLogin = "<SQLDatabaseLoginName>"
$sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>"
$sqlDatabaseName = "<SQLDatabaseName>"
$sqlDatabaseTableName = "log4jLogsCount"

Write-host "Delete hello Hive script output file ..." -ForegroundColor Green
$storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
$destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

Write-host "Delete all hello records from hello log4jLogsCount table ..." -ForegroundColor Green
$conn = New-Object System.Data.SqlClient.SqlConnection
$conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
$conn.open()
$cmd = New-Object System.Data.SqlClient.SqlCommand
$cmd.connection = $conn
$cmd.commandtext = "delete from $sqlDatabaseTableName"
$cmd.executenonquery()

$conn.close()
```

## <a name="next-steps"></a><span data-ttu-id="33818-357">Další kroky</span><span class="sxs-lookup"><span data-stu-id="33818-357">Next steps</span></span>
<span data-ttu-id="33818-358">V tomto kurzu jste se naučili jak toodefine pracovním postupu Oozie a Oozie coordinator, a jak toorun Oozie coordinator úlohy pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="33818-358">In this tutorial, you learned how toodefine an Oozie workflow and an Oozie coordinator, and how toorun an Oozie coordinator job by using Azure PowerShell.</span></span> <span data-ttu-id="33818-359">toolearn více, najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="33818-359">toolearn more, see hello following articles:</span></span>

* <span data-ttu-id="33818-360">[Začínáme s HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="33818-360">[Get started with HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="33818-361">[Používat úložiště objektů Azure Blob s HDInsight][hdinsight-storage]</span><span class="sxs-lookup"><span data-stu-id="33818-361">[Use Azure Blob storage with HDInsight][hdinsight-storage]</span></span>
* <span data-ttu-id="33818-362">[Spravovat HDInsight pomocí prostředí Azure PowerShell][hdinsight-admin-powershell]</span><span class="sxs-lookup"><span data-stu-id="33818-362">[Administer HDInsight by using Azure PowerShell][hdinsight-admin-powershell]</span></span>
* <span data-ttu-id="33818-363">[Nahrání dat tooHDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="33818-363">[Upload data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="33818-364">[Použití nástroje Sqoop s HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="33818-364">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>
* <span data-ttu-id="33818-365">[Použití Hivu se službou HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="33818-365">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="33818-366">[Použití Pigu se službou HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="33818-366">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="33818-367">[Vývoj aplikací Java MapReduce pro HDInsight][hdinsight-develop-java-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="33818-367">[Develop Java MapReduce programs for HDInsight][hdinsight-develop-java-mapreduce]</span></span>

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
