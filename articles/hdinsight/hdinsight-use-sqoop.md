---
title: "Spuštění úloh Apache Sqoop s Azure HDInsight (Hadoop) | Microsoft Docs"
description: "Další informace o použití prostředí Azure PowerShell z pracovní stanice Sqoop import a export mezi clusteru Hadoop a Azure SQL database."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 2fdcc6b7-6ad5-4397-a30b-e7e389b66c7a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 8e77153493b6f37f5f48116b86bad6b25a50d1a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-sqoop-with-hadoop-in-hdinsight"></a><span data-ttu-id="7cb63-103">Použití nástroje Sqoop se systémem Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="7cb63-103">Use Sqoop with Hadoop in HDInsight</span></span>
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

<span data-ttu-id="7cb63-104">Další informace o použití Sqoop v HDInsight k importu a exportu mezi HDInsight cluster a Azure SQL database nebo databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7cb63-104">Learn how to use Sqoop in HDInsight to import and export between HDInsight cluster and Azure SQL database or SQL Server database.</span></span>

<span data-ttu-id="7cb63-105">I když Hadoop je přirozené volbou pro zpracování nestrukturovaných a částečně strukturovaných dat, jako jsou protokoly a soubory, může také být potřeba zpracování strukturovaných dat, která je uložená v relačních databází.</span><span class="sxs-lookup"><span data-stu-id="7cb63-105">Although Hadoop is a natural choice for processing unstructured and semistructured data, such as logs and files, there may also be a need to process structured data that is stored in relational databases.</span></span>

<span data-ttu-id="7cb63-106">[Sqoop] [ sqoop-user-guide-1.4.4] je nástroj sloužící k přenosu dat mezi clusterů systému Hadoop a relačními databázemi.</span><span class="sxs-lookup"><span data-stu-id="7cb63-106">[Sqoop][sqoop-user-guide-1.4.4] is a tool designed to transfer data between Hadoop clusters and relational databases.</span></span> <span data-ttu-id="7cb63-107">Můžete ho pro import dat ze systému správy relačních databází (RDBMS), jako je SQL Server, MySQL a Oracle do systému souborů Hadoop distributed (HDFS), transformovat data v Hadoop pomocí MapReduce nebo Hive a poté exportujte data zpět do relační.</span><span class="sxs-lookup"><span data-stu-id="7cb63-107">You can use it to import data from a relational database management system (RDBMS) such as SQL Server, MySQL, or Oracle into the Hadoop distributed file system (HDFS), transform the data in Hadoop with MapReduce or Hive, and then export the data back into an RDBMS.</span></span> <span data-ttu-id="7cb63-108">V tomto kurzu použijete databázi systému SQL Server pro relační databázi.</span><span class="sxs-lookup"><span data-stu-id="7cb63-108">In this tutorial, you are using a SQL Server database for your relational database.</span></span>

<span data-ttu-id="7cb63-109">Sqoop verze, které jsou podporovány v clusterech prostředí HDInsight najdete v tématu [co je nového ve verzích clusterů poskytovaných v HDInsight?][hdinsight-versions]</span><span class="sxs-lookup"><span data-stu-id="7cb63-109">For Sqoop versions that are supported on HDInsight clusters, see [What's new in the cluster versions provided by HDInsight?][hdinsight-versions]</span></span>

## <a name="understand-the-scenario"></a><span data-ttu-id="7cb63-110">Pochopit scénáře</span><span class="sxs-lookup"><span data-stu-id="7cb63-110">Understand the scenario</span></span>

<span data-ttu-id="7cb63-111">HDInsight cluster se dodává s ukázková data.</span><span class="sxs-lookup"><span data-stu-id="7cb63-111">HDInsight cluster comes with some sample data.</span></span> <span data-ttu-id="7cb63-112">Můžete použít následující dva ukázky:</span><span class="sxs-lookup"><span data-stu-id="7cb63-112">You use the following two samples:</span></span>

* <span data-ttu-id="7cb63-113">Soubor protokolu log4j, která se nachází v */example/data/sample.log*.</span><span class="sxs-lookup"><span data-stu-id="7cb63-113">A log4j log file, which is located at */example/data/sample.log*.</span></span> <span data-ttu-id="7cb63-114">Tyto protokoly jsou extrahovány ze souboru:</span><span class="sxs-lookup"><span data-stu-id="7cb63-114">The following logs are extracted from the file:</span></span>
  
        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...
* <span data-ttu-id="7cb63-115">Hive tabulku s názvem *hivesampletable*, který odkazuje na datový soubor nacházející se v */hive/warehouse/hivesampletable*.</span><span class="sxs-lookup"><span data-stu-id="7cb63-115">A Hive table named *hivesampletable*, which references the data file located at */hive/warehouse/hivesampletable*.</span></span> <span data-ttu-id="7cb63-116">Tabulka obsahuje některé data mobilních zařízení.</span><span class="sxs-lookup"><span data-stu-id="7cb63-116">The table contains some mobile device data.</span></span> 
  
  | <span data-ttu-id="7cb63-117">Pole</span><span class="sxs-lookup"><span data-stu-id="7cb63-117">Field</span></span> | <span data-ttu-id="7cb63-118">Datový typ</span><span class="sxs-lookup"><span data-stu-id="7cb63-118">Data type</span></span> |
  | --- | --- |
  | <span data-ttu-id="7cb63-119">ClientID</span><span class="sxs-lookup"><span data-stu-id="7cb63-119">clientid</span></span> |<span data-ttu-id="7cb63-120">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7cb63-120">string</span></span> |
  | <span data-ttu-id="7cb63-121">querytime</span><span class="sxs-lookup"><span data-stu-id="7cb63-121">querytime</span></span> |<span data-ttu-id="7cb63-122">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7cb63-122">string</span></span> |
  | <span data-ttu-id="7cb63-123">trh</span><span class="sxs-lookup"><span data-stu-id="7cb63-123">market</span></span> |<span data-ttu-id="7cb63-124">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7cb63-124">string</span></span> |
  | <span data-ttu-id="7cb63-125">deviceplatform</span><span class="sxs-lookup"><span data-stu-id="7cb63-125">deviceplatform</span></span> |<span data-ttu-id="7cb63-126">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7cb63-126">string</span></span> |
  | <span data-ttu-id="7cb63-127">devicemake</span><span class="sxs-lookup"><span data-stu-id="7cb63-127">devicemake</span></span> |<span data-ttu-id="7cb63-128">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7cb63-128">string</span></span> |
  | <span data-ttu-id="7cb63-129">devicemodel</span><span class="sxs-lookup"><span data-stu-id="7cb63-129">devicemodel</span></span> |<span data-ttu-id="7cb63-130">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7cb63-130">string</span></span> |
  | <span data-ttu-id="7cb63-131">state</span><span class="sxs-lookup"><span data-stu-id="7cb63-131">state</span></span> |<span data-ttu-id="7cb63-132">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7cb63-132">string</span></span> |
  | <span data-ttu-id="7cb63-133">Země</span><span class="sxs-lookup"><span data-stu-id="7cb63-133">country</span></span> |<span data-ttu-id="7cb63-134">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7cb63-134">string</span></span> |
  | <span data-ttu-id="7cb63-135">querydwelltime</span><span class="sxs-lookup"><span data-stu-id="7cb63-135">querydwelltime</span></span> |<span data-ttu-id="7cb63-136">Double</span><span class="sxs-lookup"><span data-stu-id="7cb63-136">double</span></span> |
  | <span data-ttu-id="7cb63-137">ID relace</span><span class="sxs-lookup"><span data-stu-id="7cb63-137">sessionid</span></span> |<span data-ttu-id="7cb63-138">bigint</span><span class="sxs-lookup"><span data-stu-id="7cb63-138">bigint</span></span> |
  | <span data-ttu-id="7cb63-139">sessionpagevieworder</span><span class="sxs-lookup"><span data-stu-id="7cb63-139">sessionpagevieworder</span></span> |<span data-ttu-id="7cb63-140">bigint</span><span class="sxs-lookup"><span data-stu-id="7cb63-140">bigint</span></span> |

<span data-ttu-id="7cb63-141">Nejprve exportujete *sample.log* a *hivesampletable* do Azure SQL database nebo SQL Server a pak import tabulka, která obsahuje data mobilních zařízení zpět do HDInsight pomocí následující cesty:</span><span class="sxs-lookup"><span data-stu-id="7cb63-141">First, you export *sample.log* and *hivesampletable* to the Azure SQL database or to SQL Server, and then import the table that contains the mobile device data back to HDInsight by using the following path:</span></span>

    /tutorials/usesqoop/importeddata

## <a name="create-cluster-and-sql-database"></a><span data-ttu-id="7cb63-142">Vytvoření clusteru a databáze SQL</span><span class="sxs-lookup"><span data-stu-id="7cb63-142">Create cluster and SQL database</span></span>
<span data-ttu-id="7cb63-143">V této části se dozvíte, jak vytvořit cluster, databáze SQL a schémata databáze SQL pro spuštění kurz pomocí portálu Azure a šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="7cb63-143">This section shows you how to create a cluster, a SQL Database, and the SQL database schemas for running the tutorial using the Azure portal and an Azure Resource Manager template.</span></span> <span data-ttu-id="7cb63-144">Šablony lze nalézt v [šablon Azure rychlý Start](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-sql-database/).</span><span class="sxs-lookup"><span data-stu-id="7cb63-144">The template can be found in [Azure QuickStart Templates](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-sql-database/).</span></span> <span data-ttu-id="7cb63-145">Šablony Resource Manageru volá souboru bacpac balíček pro nasazení schémata tabulek do databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="7cb63-145">The Resource Manager template calls a bacpac package to deploy the table schemas to SQL database.</span></span>  <span data-ttu-id="7cb63-146">Balíček souboru bacpac se nachází v kontejneru veřejného objektu blob, https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac.</span><span class="sxs-lookup"><span data-stu-id="7cb63-146">The bacpac package is located in a public blob container, https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac.</span></span> <span data-ttu-id="7cb63-147">Pokud chcete použít pro soubory souboru bacpac kontejner privátní, použijte následující hodnoty v šabloně:</span><span class="sxs-lookup"><span data-stu-id="7cb63-147">If you want to use a private container for the bacpac files, use the following values in the template:</span></span>
   
        "storageKeyType": "Primary",
        "storageKey": "<TheAzureStorageAccountKey>",

<span data-ttu-id="7cb63-148">Pokud chcete používat prostředí Azure PowerShell k vytvoření clusteru a databázi SQL, najdete v části [příloha A](#appendix-a---a-powershell-sample).</span><span class="sxs-lookup"><span data-stu-id="7cb63-148">If you prefer to use Azure PowerShell to create the cluster and the SQL Database, see [Appendix A](#appendix-a---a-powershell-sample).</span></span>

1. <span data-ttu-id="7cb63-149">Kliknutím na následující obrázek otevřete šablonu Resource Manageru na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7cb63-149">Click the following image to open a Resource Manager template in the Azure portal.</span></span>         
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-sql-database%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-use-sqoop/deploy-to-azure.png" alt="Deploy to Azure"></a>
   

2. <span data-ttu-id="7cb63-150">Zadejte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="7cb63-150">Enter the following properties:</span></span>

    - <span data-ttu-id="7cb63-151">**Předplatné**: Zadejte předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="7cb63-151">**Subscription**: Enter your Azure subscription.</span></span>
    - <span data-ttu-id="7cb63-152">**Skupina prostředků**: Vytvořte novou skupinu prostředků Azure, nebo vyberte existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="7cb63-152">**Resource Group**: Create a new Azure Resource Group, or select an existing Resource Group.</span></span>  <span data-ttu-id="7cb63-153">Skupina prostředků je pro účely správy.</span><span class="sxs-lookup"><span data-stu-id="7cb63-153">A Resource Group is for management purpose.</span></span>  <span data-ttu-id="7cb63-154">Je kontejner pro objekty.</span><span class="sxs-lookup"><span data-stu-id="7cb63-154">It is a container for objects.</span></span>
    - <span data-ttu-id="7cb63-155">**Umístění**: Vyberte oblast.</span><span class="sxs-lookup"><span data-stu-id="7cb63-155">**Location**: Select a region.</span></span>
    - <span data-ttu-id="7cb63-156">**Název clusteru**: Zadejte název pro Hadoop cluster.</span><span class="sxs-lookup"><span data-stu-id="7cb63-156">**ClusterName**: Enter a name for the Hadoop cluster.</span></span>
    - <span data-ttu-id="7cb63-157">**Přihlašovací jméno a heslo clusteru**: výchozí přihlašovací jméno je admin.</span><span class="sxs-lookup"><span data-stu-id="7cb63-157">**Cluster login name and password**: The default login name is admin.</span></span>
    - <span data-ttu-id="7cb63-158">**Uživatelské jméno a heslo SSH**.</span><span class="sxs-lookup"><span data-stu-id="7cb63-158">**SSH user name and password**.</span></span>
    - <span data-ttu-id="7cb63-159">**Databáze SQL serveru přihlašovací jméno a heslo**.</span><span class="sxs-lookup"><span data-stu-id="7cb63-159">**SQL database server login name and password**.</span></span>
    - <span data-ttu-id="7cb63-160">**_artifacts umístění**: použijte výchozí hodnotu, pokud chcete používat svůj vlastní soubor backpac v jiném umístění.</span><span class="sxs-lookup"><span data-stu-id="7cb63-160">**_artifacts Location**: Use the default value unless you want to use your own backpac file in a different location.</span></span>
    - <span data-ttu-id="7cb63-161">**_artifacts umístění Sas Token**: ponechat prázdné.</span><span class="sxs-lookup"><span data-stu-id="7cb63-161">**_artifacts Location Sas Token**: Leave it blank.</span></span>
    - <span data-ttu-id="7cb63-162">**Název souboru souboru Bacpac**: použijte výchozí hodnotu, pokud chcete používat svůj vlastní soubor backpac.</span><span class="sxs-lookup"><span data-stu-id="7cb63-162">**Bacpac File Name**: Use the default value unless you want to use your own backpac file.</span></span>
     
     <span data-ttu-id="7cb63-163">Následující hodnoty jsou pevně kódovaný v části proměnných:</span><span class="sxs-lookup"><span data-stu-id="7cb63-163">The following values are hardcoded in the variables section:</span></span>
     
     | <span data-ttu-id="7cb63-164">Výchozí název účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="7cb63-164">Default storage account name</span></span> | <span data-ttu-id="7cb63-165"><CluterName>úložiště</span><span class="sxs-lookup"><span data-stu-id="7cb63-165"><CluterName>store</span></span> |
     | --- | --- |
     | <span data-ttu-id="7cb63-166">Název serveru databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="7cb63-166">Azure SQL database server name</span></span> |<span data-ttu-id="7cb63-167"><ClusterName>DBServer</span><span class="sxs-lookup"><span data-stu-id="7cb63-167"><ClusterName>dbserver</span></span> |
     | <span data-ttu-id="7cb63-168">Název databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="7cb63-168">Azure SQL database name</span></span> |<span data-ttu-id="7cb63-169"><ClusterName>DB</span><span class="sxs-lookup"><span data-stu-id="7cb63-169"><ClusterName>db</span></span> |
     
     <span data-ttu-id="7cb63-170">Zapište tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7cb63-170">Please write down these values.</span></span>  <span data-ttu-id="7cb63-171">Budete je potřebovat později v kurzu.</span><span class="sxs-lookup"><span data-stu-id="7cb63-171">You need them later in the tutorial.</span></span>

<span data-ttu-id="7cb63-172">3. Klikněte na možnost **OK** a uložte parametry.</span><span class="sxs-lookup"><span data-stu-id="7cb63-172">3.Click **OK** to save the parameters.</span></span>

<span data-ttu-id="7cb63-173">4. Z okna **Vlastní nasazení** klikněte na rozevírací pole **Skupina prostředků** a pak klikněte na tlačítko **Nový** a vytvořte novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="7cb63-173">4.From the **Custom deployment** blade, click **Resource group** dropdown box, and then click **New** to create a new resource group.</span></span> <span data-ttu-id="7cb63-174">Skupina prostředků je kontejner, který seskupuje cluster, účet závislého úložiště a další propojené prostředky skupin.</span><span class="sxs-lookup"><span data-stu-id="7cb63-174">The resource group is a container that groups the cluster, the dependent storage account and other linked resource.</span></span>

<span data-ttu-id="7cb63-175">5. Klikněte na tlačítko **Smluvní podmínky** a pak klikněte na tlačítko **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7cb63-175">5.Click **Legal terms**, and then click **Create**.</span></span>

<span data-ttu-id="7cb63-176">6. Klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7cb63-176">6.Click **Create**.</span></span> <span data-ttu-id="7cb63-177">Zobrazí se nová dlaždice s názvem odeslání nasazení pro šablonu nasazení.</span><span class="sxs-lookup"><span data-stu-id="7cb63-177">You see a new tile titled Submitting deployment for Template deployment.</span></span> <span data-ttu-id="7cb63-178">Vytvoření clusteru a databáze SQL trvá přibližně 20 minut.</span><span class="sxs-lookup"><span data-stu-id="7cb63-178">It takes about around 20 minutes to create the cluster and SQL database.</span></span>

<span data-ttu-id="7cb63-179">Pokud chcete použít existující databázi Azure SQL nebo Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="7cb63-179">If you choose to use existing Azure SQL database or Microsoft SQL Server</span></span>

* <span data-ttu-id="7cb63-180">**Databáze SQL Azure**: musíte nakonfigurovat pravidlo brány firewall pro server databáze Azure SQL pro povolení přístupu z pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="7cb63-180">**Azure SQL database**: You must configure a firewall rule for the Azure SQL database server to allow access from your workstation.</span></span> <span data-ttu-id="7cb63-181">Pokyny týkající se vytváření databáze Azure SQL a konfiguraci brány firewall najdete v tématu [začít používat Azure SQL database][sqldatabase-get-started].</span><span class="sxs-lookup"><span data-stu-id="7cb63-181">For instructions about creating an Azure SQL database and configuring the firewall, see [Get started using Azure SQL database][sqldatabase-get-started].</span></span> 
  
  > [!NOTE]
  > <span data-ttu-id="7cb63-182">Ve výchozím nastavení Azure SQL database umožňuje připojení z Azure služby, jako je Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="7cb63-182">By default an Azure SQL database allows connections from Azure services, such as Azure HDInsight.</span></span> <span data-ttu-id="7cb63-183">Pokud toto nastavení brány firewall je zakázáno, musíte ji povolit z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7cb63-183">If this firewall setting is disabled, you need to enable it from the Azure portal.</span></span> <span data-ttu-id="7cb63-184">Pokyny týkající se vytváření databáze Azure SQL a konfigurace pravidel brány firewall, najdete v části [vytvořit a nakonfigurovat databázi SQL][sqldatabase-create-configue].</span><span class="sxs-lookup"><span data-stu-id="7cb63-184">For instruction about creating an Azure SQL database and configuring firewall rules, see [Create and Configure SQL Database][sqldatabase-create-configue].</span></span>
  > 
  > 
* <span data-ttu-id="7cb63-185">**SQL Server**: Pokud váš cluster HDInsight je ve stejné virtuální síti v Azure jako systém SQL Server, můžete použít kroky v tomto článku pro import a export dat k databázi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7cb63-185">**SQL Server**: If your HDInsight cluster is on the same virtual network in Azure as SQL Server, you can use the steps in this article to import and export data to a SQL Server database.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="7cb63-186">HDInsight podporuje pouze na základě umístění virtuální sítě a aktuálně nefunguje s virtuálních sítích založených na skupinu vztahů.</span><span class="sxs-lookup"><span data-stu-id="7cb63-186">HDInsight supports only location-based virtual networks, and it does not currently work with affinity group-based virtual networks.</span></span>
  > 
  > 
  
  * <span data-ttu-id="7cb63-187">Vytvoření a konfigurace virtuální sítě najdete v tématu [vytvoření virtuální sítě pomocí portálu Azure](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="7cb63-187">To create and configure a virtual network, see [Create a virtual network using the Azure portal](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span></span>
    
    * <span data-ttu-id="7cb63-188">Pokud používáte systém SQL Server ve vašem datovém centru, je nutné nakonfigurovat virtuální síti jako *site-to-site* nebo *point-to-site*.</span><span class="sxs-lookup"><span data-stu-id="7cb63-188">When you are using SQL Server in your datacenter, you must configure the virtual network as *site-to-site* or *point-to-site*.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="7cb63-189">Pro **point-to-site** virtuální sítě, SQL Server musí používat klienta VPN konfigurace aplikace, která je k dispozici z **řídicí panel** konfigurace virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="7cb63-189">For **point-to-site** virtual networks, SQL Server must be running the VPN client configuration application, which is available from the **Dashboard** of your Azure virtual network configuration.</span></span>
      > 
      > 
    * <span data-ttu-id="7cb63-190">Při použití systému SQL Server na virtuální počítač Azure, lze použít žádnou konfiguraci virtuální sítě, pokud je virtuální počítač, který je hostitelem SQL serveru členem stejné virtuální síti jako HDInsight.</span><span class="sxs-lookup"><span data-stu-id="7cb63-190">When you are using SQL Server on an Azure virtual machine, any virtual network configuration can be used if the virtual machine hosting SQL Server is a member of the same virtual network as HDInsight.</span></span>
  * <span data-ttu-id="7cb63-191">Vytvoření clusteru HDInsight ve virtuální síti naleznete v tématu [vytvoření Hadoop clusterů v HDInsight pomocí vlastních možností](hdinsight-hadoop-provision-linux-clusters.md)</span><span class="sxs-lookup"><span data-stu-id="7cb63-191">To create an HDInsight cluster on a virtual network, see [Create Hadoop clusters in HDInsight using custom options](hdinsight-hadoop-provision-linux-clusters.md)</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="7cb63-192">SQL Server musíte také povolit ověřování.</span><span class="sxs-lookup"><span data-stu-id="7cb63-192">SQL Server must also allow authentication.</span></span> <span data-ttu-id="7cb63-193">Pokud chcete provést kroky v tomto článku, je nutné použít přihlášení systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7cb63-193">You must use a SQL Server login to complete the steps in this article.</span></span>
    > 
    > 

## <a name="run-sqoop-jobs"></a><span data-ttu-id="7cb63-194">Spuštění úloh Sqoop</span><span class="sxs-lookup"><span data-stu-id="7cb63-194">Run Sqoop jobs</span></span>
<span data-ttu-id="7cb63-195">HDInsight Sqoop úlohy můžete spustit pomocí různých metod.</span><span class="sxs-lookup"><span data-stu-id="7cb63-195">HDInsight can run Sqoop jobs by using a variety of methods.</span></span> <span data-ttu-id="7cb63-196">Následující tabulku použijte k rozhodování, jakou metodu je pro vás nejvhodnější a potom klepněte na odkaz návod.</span><span class="sxs-lookup"><span data-stu-id="7cb63-196">Use the following table to decide which method is right for you, then follow the link for a walkthrough.</span></span>

| <span data-ttu-id="7cb63-197">**Použít** Pokud chcete...</span><span class="sxs-lookup"><span data-stu-id="7cb63-197">**Use this** if you want...</span></span> | <span data-ttu-id="7cb63-198">.. .an **interaktivní** prostředí</span><span class="sxs-lookup"><span data-stu-id="7cb63-198">...an **interactive** shell</span></span> | <span data-ttu-id="7cb63-199">... **batch** zpracování</span><span class="sxs-lookup"><span data-stu-id="7cb63-199">...**batch** processing</span></span> | <span data-ttu-id="7cb63-200">.. při to **clusteru operačního systému**</span><span class="sxs-lookup"><span data-stu-id="7cb63-200">...with this **cluster operating system**</span></span> | <span data-ttu-id="7cb63-201">.. .from to **klientský operační systém**</span><span class="sxs-lookup"><span data-stu-id="7cb63-201">...from this **client operating system**</span></span> |
|:--- |:---:|:---:|:--- |:--- |
| [<span data-ttu-id="7cb63-202">SSH</span><span class="sxs-lookup"><span data-stu-id="7cb63-202">SSH</span></span>](hdinsight-use-sqoop-mac-linux.md) |<span data-ttu-id="7cb63-203">✔</span><span class="sxs-lookup"><span data-stu-id="7cb63-203">✔</span></span> |<span data-ttu-id="7cb63-204">✔</span><span class="sxs-lookup"><span data-stu-id="7cb63-204">✔</span></span> |<span data-ttu-id="7cb63-205">Linux</span><span class="sxs-lookup"><span data-stu-id="7cb63-205">Linux</span></span> |<span data-ttu-id="7cb63-206">Linux, Unix, Mac OS X nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="7cb63-206">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="7cb63-207">Sada .NET SDK pro Hadoop</span><span class="sxs-lookup"><span data-stu-id="7cb63-207">.NET SDK for Hadoop</span></span>](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |&nbsp; |<span data-ttu-id="7cb63-208">✔</span><span class="sxs-lookup"><span data-stu-id="7cb63-208">✔</span></span> |<span data-ttu-id="7cb63-209">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="7cb63-209">Linux or Windows</span></span> |<span data-ttu-id="7cb63-210">Windows (prozatím)</span><span class="sxs-lookup"><span data-stu-id="7cb63-210">Windows (for now)</span></span> |
| [<span data-ttu-id="7cb63-211">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7cb63-211">Azure PowerShell</span></span>](hdinsight-hadoop-use-sqoop-powershell.md) |&nbsp; |<span data-ttu-id="7cb63-212">✔</span><span class="sxs-lookup"><span data-stu-id="7cb63-212">✔</span></span> |<span data-ttu-id="7cb63-213">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="7cb63-213">Linux or Windows</span></span> |<span data-ttu-id="7cb63-214">Windows</span><span class="sxs-lookup"><span data-stu-id="7cb63-214">Windows</span></span> |

## <a name="limitations"></a><span data-ttu-id="7cb63-215">Omezení</span><span class="sxs-lookup"><span data-stu-id="7cb63-215">Limitations</span></span>
* <span data-ttu-id="7cb63-216">Hromadné export - s Linuxovým systémem HDInsight, Sqoop konektor umožňuje exportovat data do systému Microsoft SQL Server nebo Azure SQL Database v současné době nepodporuje hromadné vložení.</span><span class="sxs-lookup"><span data-stu-id="7cb63-216">Bulk export - With Linux-based HDInsight, the Sqoop connector used to export data to Microsoft SQL Server or Azure SQL Database does not currently support bulk inserts.</span></span>
* <span data-ttu-id="7cb63-217">Dávkování - s HDInsight se systémem Linux, při použití `-batch` přepnout při vložení, Sqoop provádí více vloží místo dávkování operace insert.</span><span class="sxs-lookup"><span data-stu-id="7cb63-217">Batching - With Linux-based HDInsight, When using the `-batch` switch when performing inserts, Sqoop performs multiple inserts instead of batching the insert operations.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7cb63-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7cb63-218">Next steps</span></span>
<span data-ttu-id="7cb63-219">Nyní jste se naučili postup použití nástroje Sqoop.</span><span class="sxs-lookup"><span data-stu-id="7cb63-219">Now you have learned how to use Sqoop.</span></span> <span data-ttu-id="7cb63-220">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="7cb63-220">To learn more, see:</span></span>

* [<span data-ttu-id="7cb63-221">Použití Hivu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="7cb63-221">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="7cb63-222">Použití Pigu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="7cb63-222">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* <span data-ttu-id="7cb63-223">[Použijte Oozie s HDInsight][hdinsight-use-oozie]: použití Sqoop akce v pracovním postupu Oozie.</span><span class="sxs-lookup"><span data-stu-id="7cb63-223">[Use Oozie with HDInsight][hdinsight-use-oozie]: Use Sqoop action in an Oozie workflow.</span></span>
* <span data-ttu-id="7cb63-224">[Analýza dat zpoždění letu pomocí HDInsight][hdinsight-analyze-flight-data]: použití Hive k analýze letu zpoždění dat a pak pomocí Sqoop exportovat data do Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="7cb63-224">[Analyze flight delay data using HDInsight][hdinsight-analyze-flight-data]: Use Hive to analyze flight delay data, and then use Sqoop to export data to an Azure SQL database.</span></span>
* <span data-ttu-id="7cb63-225">[Nahrání dat do HDInsight][hdinsight-upload-data]: Najít další metody pro odesílání dat do HDInsight nebo Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="7cb63-225">[Upload data to HDInsight][hdinsight-upload-data]: Find other methods for uploading data to HDInsight/Azure Blob storage.</span></span>

## <a name="appendix-a---a-powershell-sample"></a><span data-ttu-id="7cb63-226">Příloha A - ukázku prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="7cb63-226">Appendix A - a PowerShell sample</span></span>
<span data-ttu-id="7cb63-227">Ukázku v prostředí PowerShell provede následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7cb63-227">The PowerShell sample performs the following steps:</span></span>

1. <span data-ttu-id="7cb63-228">Připojte k Azure.</span><span class="sxs-lookup"><span data-stu-id="7cb63-228">Connect to Azure.</span></span>
2. <span data-ttu-id="7cb63-229">Vytvořte skupinu prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="7cb63-229">Create an Azure resource group.</span></span> <span data-ttu-id="7cb63-230">Další informace najdete v tématu [použití Azure Powershellu s Azure Resource Manager](../powershell-azure-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="7cb63-230">For more information, see [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md)</span></span>
3. <span data-ttu-id="7cb63-231">Vytvoření serveru Azure SQL Database, Azure SQL database a dvě tabulky.</span><span class="sxs-lookup"><span data-stu-id="7cb63-231">Create an Azure SQL Database server, an Azure SQL database, and two tables.</span></span> 
   
    <span data-ttu-id="7cb63-232">Pokud místo toho používat SQL Server, použijte následující příkazy k vytvoření tabulky:</span><span class="sxs-lookup"><span data-stu-id="7cb63-232">If you use SQL Server instead, use the following statements to create the tables:</span></span>
   
        CREATE TABLE [dbo].[log4jlogs](
         [t1] [nvarchar](50),
         [t2] [nvarchar](50),
         [t3] [nvarchar](50),
         [t4] [nvarchar](50),
         [t5] [nvarchar](50),
         [t6] [nvarchar](50),
         [t7] [nvarchar](50))
   
        CREATE TABLE [dbo].[mobiledata](
         [clientid] [nvarchar](50),
         [querytime] [nvarchar](50),
         [market] [nvarchar](50),
         [deviceplatform] [nvarchar](50),
         [devicemake] [nvarchar](50),
         [devicemodel] [nvarchar](50),
         [state] [nvarchar](50),
         [country] [nvarchar](50),
         [querydwelltime] [float],
         [sessionid] [bigint],
         [sessionpagevieworder][bigint])
   
    <span data-ttu-id="7cb63-233">Nejjednodušší způsob, jak podívejte se na databáze a tabulky je pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7cb63-233">The easiest way to examine the database and tables is to use Visual Studio.</span></span> <span data-ttu-id="7cb63-234">Databázový server a databáze může být prověřen pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7cb63-234">The database server and the database can be examined using the Azure portal.</span></span>
4. <span data-ttu-id="7cb63-235">Vytvoření clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="7cb63-235">Create an HDInsight cluster.</span></span>
   
    <span data-ttu-id="7cb63-236">K prozkoumání clusteru, můžete portál Azure nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7cb63-236">To examine the cluster, you can use the Azure portal or Azure PowerShell.</span></span>
5. <span data-ttu-id="7cb63-237">Předběžně zpracovat zdrojového datového souboru.</span><span class="sxs-lookup"><span data-stu-id="7cb63-237">Pre-process the source data file.</span></span>
   
    <span data-ttu-id="7cb63-238">V tomto kurzu můžete exportovat soubor protokolu log4j (soubor s oddělovači) a tabulku Hive do Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="7cb63-238">In this tutorial, you export a log4j log file (a delimited file) and a Hive table to an Azure SQL database.</span></span> <span data-ttu-id="7cb63-239">Souboru s oddělovači se nazývá */example/data/sample.log*.</span><span class="sxs-lookup"><span data-stu-id="7cb63-239">The delimited file is called */example/data/sample.log*.</span></span> <span data-ttu-id="7cb63-240">V tomto kurzu jste viděli několik ukázky log4j protokolů.</span><span class="sxs-lookup"><span data-stu-id="7cb63-240">Earlier in the tutorial, you saw a few samples of log4j logs.</span></span> <span data-ttu-id="7cb63-241">V souboru protokolu existují některé prázdné řádky a některé řádky vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="7cb63-241">In the log file, there are some empty lines and some lines similar to these:</span></span>
   
        java.lang.Exception: 2012-02-03 20:11:35 SampleClass2 [FATAL] unrecoverable system problem at id 609774657
            at com.osa.mocklogger.MockLogger$2.run(MockLogger.java:83)
   
    <span data-ttu-id="7cb63-242">To je v pořádku pro další příklady, které používají tato data, ale před jsme můžete importovat do Azure SQL database nebo SQL Server jsme musíte odebrat tyto výjimky.</span><span class="sxs-lookup"><span data-stu-id="7cb63-242">This is fine for other examples that use this data, but we must remove these exceptions before we can import into the Azure SQL database or SQL Server.</span></span> <span data-ttu-id="7cb63-243">Sqoop export se nezdaří, pokud je prázdný řetězec nebo čáry s méně elementů než počet elementů pole definovaná v tabulce databáze Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="7cb63-243">Sqoop export  fails if there is an empty string or a line with a fewer elements than the number of fields defined in the Azure SQL database table.</span></span> <span data-ttu-id="7cb63-244">Tabulka log4jlogs má 7 řetězec typu pole.</span><span class="sxs-lookup"><span data-stu-id="7cb63-244">The log4jlogs table has 7 string-type fields.</span></span>
   
    <span data-ttu-id="7cb63-245">Tento postup vytvoří nový soubor v clusteru: tutorials/usesqoop/data/sample.log.</span><span class="sxs-lookup"><span data-stu-id="7cb63-245">This procedure creates a new file on the cluster: tutorials/usesqoop/data/sample.log.</span></span> <span data-ttu-id="7cb63-246">Pro zjištění změny datového souboru, můžete portál Azure, nástroji Průzkumník Azure Storage nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7cb63-246">To examine the modified data file, you can use the Azure portal, an Azure Storage explorer tool, or Azure PowerShell.</span></span> <span data-ttu-id="7cb63-247">[Začínáme s HDInsight] [ hdinsight-get-started] má ukázka kódu pro použití Azure PowerShell k stažení souboru a zobrazit obsah souboru.</span><span class="sxs-lookup"><span data-stu-id="7cb63-247">[Get started with HDInsight][hdinsight-get-started] has a code sample for using Azure PowerShell to download a file and display the file content.</span></span>
6. <span data-ttu-id="7cb63-248">Exportujte soubor dat databáze Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="7cb63-248">Export a data file to the Azure SQL database.</span></span>
   
    <span data-ttu-id="7cb63-249">Zdrojový soubor je tutorials/usesqoop/data/sample.log.</span><span class="sxs-lookup"><span data-stu-id="7cb63-249">The source file is tutorials/usesqoop/data/sample.log.</span></span> <span data-ttu-id="7cb63-250">V tabulce, kde se data se exportují do nazývá log4jlogs.</span><span class="sxs-lookup"><span data-stu-id="7cb63-250">The table where the data is exported to is called log4jlogs.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="7cb63-251">Než informace o připojovacím řetězci by měl pracovní postup v této části pro Azure SQL database nebo SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7cb63-251">Other than connection string information, the steps in this section should work for an Azure SQL database or for SQL Server.</span></span> <span data-ttu-id="7cb63-252">Tyto kroky testovali pomocí následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="7cb63-252">These steps were tested by using the following configuration:</span></span>
   > 
   > * <span data-ttu-id="7cb63-253">**Konfigurace point-to-site virtuální síť Azure**: virtuální sítě připojen k serveru SQL Server v privátním datacentru clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="7cb63-253">**Azure virtual network point-to-site configuration**: A virtual network connected the HDInsight cluster to a SQL Server in a private datacenter.</span></span> <span data-ttu-id="7cb63-254">V tématu [konfigurace VPN typu Point-to-Site v portálu pro správu](../vpn-gateway/vpn-gateway-point-to-site-create.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="7cb63-254">See [Configure a Point-to-Site VPN in the Management Portal](../vpn-gateway/vpn-gateway-point-to-site-create.md) for more information.</span></span>
   > * <span data-ttu-id="7cb63-255">**Azure HDInsight 3.1**: najdete v části [vytvoření Hadoop clusterů v HDInsight pomocí vlastních možností](hdinsight-hadoop-provision-linux-clusters.md) informace o vytváření clusteru s podporou ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="7cb63-255">**Azure HDInsight 3.1**: See [Create Hadoop clusters in HDInsight using custom options](hdinsight-hadoop-provision-linux-clusters.md) for information about creating a cluster on a virtual network.</span></span>
   > * <span data-ttu-id="7cb63-256">**SQL Server 2014**: nakonfigurovaná tak, aby povolit ověřování a spouštění klienta VPN konfigurační balíček se bezpečně připojit k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="7cb63-256">**SQL Server 2014**: Configured to allow authentication and running the VPN client configuration package to connect securely to the virtual network.</span></span>
   > 
   > 
7. <span data-ttu-id="7cb63-257">Exportujte tabulky Hive k databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="7cb63-257">Export a Hive table to the Azure SQL database.</span></span>
8. <span data-ttu-id="7cb63-258">Importujte tabulky mobiledata ke clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="7cb63-258">Import the mobiledata table to the HDInsight cluster.</span></span>
   
    <span data-ttu-id="7cb63-259">Pro zjištění změny datového souboru, můžete portál Azure, nástroji Průzkumník Azure Storage nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7cb63-259">To examine the modified data file, you can use the Azure portal, an Azure Storage explorer tool, or Azure PowerShell.</span></span>  <span data-ttu-id="7cb63-260">[Začínáme s HDInsight] [ hdinsight-get-started] má ukázka kódu o použití prostředí Azure PowerShell k stažení souboru a zobrazit obsah souboru.</span><span class="sxs-lookup"><span data-stu-id="7cb63-260">[Get started with HDInsight][hdinsight-get-started] has a code sample about using Azure PowerShell to download a file and display the file content.</span></span>

### <a name="the-powershell-sample"></a><span data-ttu-id="7cb63-261">Ukázku v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="7cb63-261">The PowerShell sample</span></span>
    # Prepare an Azure SQL database to be used by the Sqoop tutorial

    #region - provide the following values

    $subscriptionID = "<Enter your Azure Subscription ID>"

    $sqlDatabaseLogin = "<Enter a SQL Database Login name>" #SQL Database server login
    $sqlDatabasePassword = "<Enter a Password>"

    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"

    # used for creating Azure service names
    $nameToken = "<Enter an alias>" 
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion

    #region - variables

    # Resource group variables
    $resourceGroupName = $namePrefix + "rg"
    $location = "East US 2" # used by all Azure services defined in this tutorial

    # SQL database varialbes
    $sqlDatabaseServerName = $namePrefix + "sqldbserver"
    $sqlDatabaseName = $namePrefix + "sqldb"
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $sqlDatabaseMaxSizeGB = 10

    # Used for retrieving external IP address and creating firewall rules
    $ipAddressRestService = "http://bot.whatismyipaddress.com"
    $fireWallRuleName = "UseSqoop"

    # Used for creating tables and clustered indexes
    $cmdCreateLog4jTable = "CREATE TABLE [dbo].[log4jlogs](
        [t1] [nvarchar](50),
        [t2] [nvarchar](50),
        [t3] [nvarchar](50),
        [t4] [nvarchar](50),
        [t5] [nvarchar](50),
        [t6] [nvarchar](50),
        [t7] [nvarchar](50))"

    $cmdCreateLog4jClusteredIndex = "CREATE CLUSTERED INDEX log4jlogs_clustered_index on log4jlogs(t1)"

    $cmdCreateMobileTable = " CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder][bigint])"

    $cmdCreateMobileDataClusteredIndex = "CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)"

    # HDInsight variables
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    #region - Create Azure resouce group
    Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    }
    #endregion

    #region - Create Azure SQL database server
    Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green

        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)

        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $credential `
                                    -Location $location).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan

        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-workstation" `
            -StartIpAddress $workstationIPAddress `
            -EndIpAddress $workstationIPAddress

        #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription to access the server.
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-Azureservices" `
            -StartIpAddress "0.0.0.0" `
            -EndIpAddress "0.0.0.0"
    }

    #endregion

    #region - Create and validate Azure SQL database
    Write-Host "`nCreating an Azure SQL database ..." -ForegroundColor Green

    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }

    #endregion

    #region - Create tables
    Write-Host "Creating the log4jlogs table and the mobiledata table ..." -ForegroundColor Green

    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()

    # Create the log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jTable
    $ret = $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateLog4jClusteredIndex
    $cmd.ExecuteNonQuery()

    # Create the mobiledata table and index
    $cmd.CommandText = $cmdCreateMobileTable
    $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateMobileDataClusteredIndex
    $cmd.ExecuteNonQuery()

    $conn.close()

    #endregion


    #region - Create HDInsight cluster

    Write-Host "Creating the HDInsight cluster and the dependent services ..." -ForegroundColor Green

    # Create the default storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS

    # Create the default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 

    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -Location $location `
        -ClusterType Hadoop `
        -OSType Windows `
        -ClusterSizeInNodes 2 `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultBlobContainerName 

    # Validate the cluster
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion

    #region - pre-process the source file

    Write-Host "Preprocessing the source file ..." -ForegroundColor Green

    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"

    # Define the connection string
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

    # Create block blob objects referencing the source and destination blob.
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)

    # Define a MemoryStream and a StreamReader for reading from the source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)

    # Define a MemoryStream and a StreamWriter for writing into the destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream

    # Pre-process the source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")

        # remove the "java.lang.Exception" from the first element of the array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList to remove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)

            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }

        # remove the lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }

    # Write to the destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)

    #endregion

    #region - export a log file from the cluster to the SQL database

    Write-Host "Preprocessing the source file ..." -ForegroundColor Green

    $tableName_log4j = "log4jlogs"

    # Connection string for Azure SQL Database.
    # Comment if using SQL Server
    $connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    # Connection string for SQL Server.
    # Uncomment if using SQL Server.
    #$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"

    $exportDir_log4j = "/tutorials/usesqoop/data"

    # Submit a Sqoop job
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId

    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput

    #endregion

    #region - export a Hive table

    $tableName_mobile = "mobiledata"
    $exportDir_mobile = "/hive/warehouse/hivesampletable"

    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_mobile --export-dir $exportDir_mobile --fields-terminated-by \t -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId

    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError

    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput

    #endregion

    #region - import a database

    $targetDir_mobile = "/tutorials/usesqoop/importeddata/"

    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "import --connect $connectionString --table $tableName_mobile --target-dir $targetDir_mobile --fields-terminated-by \t --lines-terminated-by \n -m 1"

    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId

    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError

    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput

    #endregion



[azure-management-portal]: https://portal.azure.com/

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database/sql-database-get-started.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
