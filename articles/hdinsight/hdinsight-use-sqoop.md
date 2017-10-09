---
title: "aaaRun Apache Sqoop úlohy s Azure HDInsight (Hadoop) | Microsoft Docs"
description: "Zjistěte, jak toouse prostředí Azure PowerShell z pracovní stanice toorun Sqoop import a export mezi clusteru Hadoop a Azure SQL database."
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
ms.openlocfilehash: bdac507704937d77921c9c13d70aa2434c7e3be4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-sqoop-with-hadoop-in-hdinsight"></a><span data-ttu-id="38304-103">Použití nástroje Sqoop se systémem Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="38304-103">Use Sqoop with Hadoop in HDInsight</span></span>
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

<span data-ttu-id="38304-104">Zjistěte, jak toouse Sqoop v HDInsight tooimport a export mezi HDInsight cluster a Azure SQL database nebo databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="38304-104">Learn how toouse Sqoop in HDInsight tooimport and export between HDInsight cluster and Azure SQL database or SQL Server database.</span></span>

<span data-ttu-id="38304-105">I když Hadoop je přirozené volbou pro zpracování nestrukturovaných a částečně strukturovaných dat, jako jsou protokoly a soubory, může také existovat nutnost tooprocess strukturovaná data uložená v relačních databází.</span><span class="sxs-lookup"><span data-stu-id="38304-105">Although Hadoop is a natural choice for processing unstructured and semistructured data, such as logs and files, there may also be a need tooprocess structured data that is stored in relational databases.</span></span>

<span data-ttu-id="38304-106">[Sqoop] [ sqoop-user-guide-1.4.4] je tootransfer nástroj navržený dat mezi clusterů systému Hadoop a relačními databázemi.</span><span class="sxs-lookup"><span data-stu-id="38304-106">[Sqoop][sqoop-user-guide-1.4.4] is a tool designed tootransfer data between Hadoop clusters and relational databases.</span></span> <span data-ttu-id="38304-107">Můžete ji použít tooimport data ze systému správy relačních databází (RDBMS), jako je SQL Server, MySQL a Oracle do systému souborů Hadoop distributed hello (HDFS), transformovat hello data v Hadoop pomocí MapReduce nebo Hive a poté exportujte hello data zpět do RELAČNÍ.</span><span class="sxs-lookup"><span data-stu-id="38304-107">You can use it tooimport data from a relational database management system (RDBMS) such as SQL Server, MySQL, or Oracle into hello Hadoop distributed file system (HDFS), transform hello data in Hadoop with MapReduce or Hive, and then export hello data back into an RDBMS.</span></span> <span data-ttu-id="38304-108">V tomto kurzu použijete databázi systému SQL Server pro relační databázi.</span><span class="sxs-lookup"><span data-stu-id="38304-108">In this tutorial, you are using a SQL Server database for your relational database.</span></span>

<span data-ttu-id="38304-109">Sqoop verze, které jsou podporovány v clusterech prostředí HDInsight najdete v tématu [co je nového ve verzích clusterů hello poskytovaných v HDInsight?][hdinsight-versions]</span><span class="sxs-lookup"><span data-stu-id="38304-109">For Sqoop versions that are supported on HDInsight clusters, see [What's new in hello cluster versions provided by HDInsight?][hdinsight-versions]</span></span>

## <a name="understand-hello-scenario"></a><span data-ttu-id="38304-110">Pochopení hello scénář</span><span class="sxs-lookup"><span data-stu-id="38304-110">Understand hello scenario</span></span>

<span data-ttu-id="38304-111">HDInsight cluster se dodává s ukázková data.</span><span class="sxs-lookup"><span data-stu-id="38304-111">HDInsight cluster comes with some sample data.</span></span> <span data-ttu-id="38304-112">Pomocí následujících dvou vzorcích hello:</span><span class="sxs-lookup"><span data-stu-id="38304-112">You use hello following two samples:</span></span>

* <span data-ttu-id="38304-113">Soubor protokolu log4j, která se nachází v */example/data/sample.log*.</span><span class="sxs-lookup"><span data-stu-id="38304-113">A log4j log file, which is located at */example/data/sample.log*.</span></span> <span data-ttu-id="38304-114">Hello následující protokoly se extrahují z hello souboru:</span><span class="sxs-lookup"><span data-stu-id="38304-114">hello following logs are extracted from hello file:</span></span>
  
        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...
* <span data-ttu-id="38304-115">Hive tabulku s názvem *hivesampletable*, který odkazuje na hello datový soubor nacházející se v */hive/warehouse/hivesampletable*.</span><span class="sxs-lookup"><span data-stu-id="38304-115">A Hive table named *hivesampletable*, which references hello data file located at */hive/warehouse/hivesampletable*.</span></span> <span data-ttu-id="38304-116">Hello tabulka obsahuje některé data mobilních zařízení.</span><span class="sxs-lookup"><span data-stu-id="38304-116">hello table contains some mobile device data.</span></span> 
  
  | <span data-ttu-id="38304-117">Pole</span><span class="sxs-lookup"><span data-stu-id="38304-117">Field</span></span> | <span data-ttu-id="38304-118">Datový typ</span><span class="sxs-lookup"><span data-stu-id="38304-118">Data type</span></span> |
  | --- | --- |
  | <span data-ttu-id="38304-119">ClientID</span><span class="sxs-lookup"><span data-stu-id="38304-119">clientid</span></span> |<span data-ttu-id="38304-120">Řetězec</span><span class="sxs-lookup"><span data-stu-id="38304-120">string</span></span> |
  | <span data-ttu-id="38304-121">querytime</span><span class="sxs-lookup"><span data-stu-id="38304-121">querytime</span></span> |<span data-ttu-id="38304-122">Řetězec</span><span class="sxs-lookup"><span data-stu-id="38304-122">string</span></span> |
  | <span data-ttu-id="38304-123">trh</span><span class="sxs-lookup"><span data-stu-id="38304-123">market</span></span> |<span data-ttu-id="38304-124">Řetězec</span><span class="sxs-lookup"><span data-stu-id="38304-124">string</span></span> |
  | <span data-ttu-id="38304-125">deviceplatform</span><span class="sxs-lookup"><span data-stu-id="38304-125">deviceplatform</span></span> |<span data-ttu-id="38304-126">Řetězec</span><span class="sxs-lookup"><span data-stu-id="38304-126">string</span></span> |
  | <span data-ttu-id="38304-127">devicemake</span><span class="sxs-lookup"><span data-stu-id="38304-127">devicemake</span></span> |<span data-ttu-id="38304-128">Řetězec</span><span class="sxs-lookup"><span data-stu-id="38304-128">string</span></span> |
  | <span data-ttu-id="38304-129">devicemodel</span><span class="sxs-lookup"><span data-stu-id="38304-129">devicemodel</span></span> |<span data-ttu-id="38304-130">Řetězec</span><span class="sxs-lookup"><span data-stu-id="38304-130">string</span></span> |
  | <span data-ttu-id="38304-131">state</span><span class="sxs-lookup"><span data-stu-id="38304-131">state</span></span> |<span data-ttu-id="38304-132">Řetězec</span><span class="sxs-lookup"><span data-stu-id="38304-132">string</span></span> |
  | <span data-ttu-id="38304-133">Země</span><span class="sxs-lookup"><span data-stu-id="38304-133">country</span></span> |<span data-ttu-id="38304-134">Řetězec</span><span class="sxs-lookup"><span data-stu-id="38304-134">string</span></span> |
  | <span data-ttu-id="38304-135">querydwelltime</span><span class="sxs-lookup"><span data-stu-id="38304-135">querydwelltime</span></span> |<span data-ttu-id="38304-136">Double</span><span class="sxs-lookup"><span data-stu-id="38304-136">double</span></span> |
  | <span data-ttu-id="38304-137">ID relace</span><span class="sxs-lookup"><span data-stu-id="38304-137">sessionid</span></span> |<span data-ttu-id="38304-138">bigint</span><span class="sxs-lookup"><span data-stu-id="38304-138">bigint</span></span> |
  | <span data-ttu-id="38304-139">sessionpagevieworder</span><span class="sxs-lookup"><span data-stu-id="38304-139">sessionpagevieworder</span></span> |<span data-ttu-id="38304-140">bigint</span><span class="sxs-lookup"><span data-stu-id="38304-140">bigint</span></span> |

<span data-ttu-id="38304-141">Nejprve exportujete *sample.log* a *hivesampletable* toohello Azure SQL database nebo tooSQL serveru a pak import hello tabulku, která obsahuje data mobilních zařízení hello zálohování tooHDInsight pomocí hello následující cestu:</span><span class="sxs-lookup"><span data-stu-id="38304-141">First, you export *sample.log* and *hivesampletable* toohello Azure SQL database or tooSQL Server, and then import hello table that contains hello mobile device data back tooHDInsight by using hello following path:</span></span>

    /tutorials/usesqoop/importeddata

## <a name="create-cluster-and-sql-database"></a><span data-ttu-id="38304-142">Vytvoření clusteru a databáze SQL</span><span class="sxs-lookup"><span data-stu-id="38304-142">Create cluster and SQL database</span></span>
<span data-ttu-id="38304-143">Tato část uvádí, jak toocreate cluster, databáze SQL a hello SQL databáze, schémata pro spuštěné hello kurz používání hello portál Azure a šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="38304-143">This section shows you how toocreate a cluster, a SQL Database, and hello SQL database schemas for running hello tutorial using hello Azure portal and an Azure Resource Manager template.</span></span> <span data-ttu-id="38304-144">Hello šablony lze nalézt v [šablon Azure rychlý Start](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-sql-database/).</span><span class="sxs-lookup"><span data-stu-id="38304-144">hello template can be found in [Azure QuickStart Templates](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-sql-database/).</span></span> <span data-ttu-id="38304-145">šablony Resource Manageru Hello volá souboru bacpac balíček toodeploy hello tabulky schémata tooSQL databáze.</span><span class="sxs-lookup"><span data-stu-id="38304-145">hello Resource Manager template calls a bacpac package toodeploy hello table schemas tooSQL database.</span></span>  <span data-ttu-id="38304-146">Hello souboru bacpac balíčku se nachází v kontejneru veřejného objektu blob, https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac.</span><span class="sxs-lookup"><span data-stu-id="38304-146">hello bacpac package is located in a public blob container, https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac.</span></span> <span data-ttu-id="38304-147">Pokud chcete pro soubory souboru bacpac hello toouse kontejner privátní, použijte následující hodnoty v šabloně hello hello:</span><span class="sxs-lookup"><span data-stu-id="38304-147">If you want toouse a private container for hello bacpac files, use hello following values in hello template:</span></span>
   
        "storageKeyType": "Primary",
        "storageKey": "<TheAzureStorageAccountKey>",

<span data-ttu-id="38304-148">Pokud dáváte přednost toouse prostředí Azure PowerShell toocreate hello clusteru a hello SQL Database, najdete v části [příloha A](#appendix-a---a-powershell-sample).</span><span class="sxs-lookup"><span data-stu-id="38304-148">If you prefer toouse Azure PowerShell toocreate hello cluster and hello SQL Database, see [Appendix A](#appendix-a---a-powershell-sample).</span></span>

1. <span data-ttu-id="38304-149">Klikněte na tlačítko hello následující tooopen image šablony Resource Manageru v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="38304-149">Click hello following image tooopen a Resource Manager template in hello Azure portal.</span></span>         
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-sql-database%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-use-sqoop/deploy-to-azure.png" alt="Deploy tooAzure"></a>
   

2. <span data-ttu-id="38304-150">Zadejte hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="38304-150">Enter hello following properties:</span></span>

    - <span data-ttu-id="38304-151">**Předplatné**: Zadejte předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="38304-151">**Subscription**: Enter your Azure subscription.</span></span>
    - <span data-ttu-id="38304-152">**Skupina prostředků**: Vytvořte novou skupinu prostředků Azure, nebo vyberte existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="38304-152">**Resource Group**: Create a new Azure Resource Group, or select an existing Resource Group.</span></span>  <span data-ttu-id="38304-153">Skupina prostředků je pro účely správy.</span><span class="sxs-lookup"><span data-stu-id="38304-153">A Resource Group is for management purpose.</span></span>  <span data-ttu-id="38304-154">Je kontejner pro objekty.</span><span class="sxs-lookup"><span data-stu-id="38304-154">It is a container for objects.</span></span>
    - <span data-ttu-id="38304-155">**Umístění**: Vyberte oblast.</span><span class="sxs-lookup"><span data-stu-id="38304-155">**Location**: Select a region.</span></span>
    - <span data-ttu-id="38304-156">**Název clusteru**: Zadejte název pro hello Hadoop cluster.</span><span class="sxs-lookup"><span data-stu-id="38304-156">**ClusterName**: Enter a name for hello Hadoop cluster.</span></span>
    - <span data-ttu-id="38304-157">**Přihlašovací jméno a heslo clusteru**: hello výchozí přihlašovací jméno je admin.</span><span class="sxs-lookup"><span data-stu-id="38304-157">**Cluster login name and password**: hello default login name is admin.</span></span>
    - <span data-ttu-id="38304-158">**Uživatelské jméno a heslo SSH**.</span><span class="sxs-lookup"><span data-stu-id="38304-158">**SSH user name and password**.</span></span>
    - <span data-ttu-id="38304-159">**Databáze SQL serveru přihlašovací jméno a heslo**.</span><span class="sxs-lookup"><span data-stu-id="38304-159">**SQL database server login name and password**.</span></span>
    - <span data-ttu-id="38304-160">**_artifacts umístění**: používání hello výchozí hodnotu, pokud chcete, aby toouse backpac souboru v jiném umístění.</span><span class="sxs-lookup"><span data-stu-id="38304-160">**_artifacts Location**: Use hello default value unless you want toouse your own backpac file in a different location.</span></span>
    - <span data-ttu-id="38304-161">**_artifacts umístění Sas Token**: ponechat prázdné.</span><span class="sxs-lookup"><span data-stu-id="38304-161">**_artifacts Location Sas Token**: Leave it blank.</span></span>
    - <span data-ttu-id="38304-162">**Název souboru souboru Bacpac**: používání hello výchozí hodnotu, pokud chcete, aby toouse souboru backpac.</span><span class="sxs-lookup"><span data-stu-id="38304-162">**Bacpac File Name**: Use hello default value unless you want toouse your own backpac file.</span></span>
     
     <span data-ttu-id="38304-163">Hello následující hodnoty jsou pevně kódovaný v části proměnných hello:</span><span class="sxs-lookup"><span data-stu-id="38304-163">hello following values are hardcoded in hello variables section:</span></span>
     
     | <span data-ttu-id="38304-164">Výchozí název účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="38304-164">Default storage account name</span></span> | <span data-ttu-id="38304-165"><CluterName>úložiště</span><span class="sxs-lookup"><span data-stu-id="38304-165"><CluterName>store</span></span> |
     | --- | --- |
     | <span data-ttu-id="38304-166">Název serveru databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="38304-166">Azure SQL database server name</span></span> |<span data-ttu-id="38304-167"><ClusterName>DBServer</span><span class="sxs-lookup"><span data-stu-id="38304-167"><ClusterName>dbserver</span></span> |
     | <span data-ttu-id="38304-168">Název databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="38304-168">Azure SQL database name</span></span> |<span data-ttu-id="38304-169"><ClusterName>DB</span><span class="sxs-lookup"><span data-stu-id="38304-169"><ClusterName>db</span></span> |
     
     <span data-ttu-id="38304-170">Zapište tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="38304-170">Please write down these values.</span></span>  <span data-ttu-id="38304-171">Budete potřebovat později v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="38304-171">You need them later in hello tutorial.</span></span>

<span data-ttu-id="38304-172">3 klikněte na tlačítko **OK** toosave hello parametry.</span><span class="sxs-lookup"><span data-stu-id="38304-172">3.Click **OK** toosave hello parameters.</span></span>

<span data-ttu-id="38304-173">4. z hello **vlastní nasazení** okně klikněte na tlačítko **skupiny prostředků** rozevírací pole a pak klikněte na **nový** toocreate novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="38304-173">4.From hello **Custom deployment** blade, click **Resource group** dropdown box, and then click **New** toocreate a new resource group.</span></span> <span data-ttu-id="38304-174">Hello skupina prostředků je kontejner, který seskupuje hello cluster, účet závislého úložiště hello a další propojené prostředky.</span><span class="sxs-lookup"><span data-stu-id="38304-174">hello resource group is a container that groups hello cluster, hello dependent storage account and other linked resource.</span></span>

<span data-ttu-id="38304-175">5. Klikněte na tlačítko **Smluvní podmínky** a pak klikněte na tlačítko **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="38304-175">5.Click **Legal terms**, and then click **Create**.</span></span>

<span data-ttu-id="38304-176">6. Klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="38304-176">6.Click **Create**.</span></span> <span data-ttu-id="38304-177">Zobrazí se nová dlaždice s názvem odeslání nasazení pro šablonu nasazení.</span><span class="sxs-lookup"><span data-stu-id="38304-177">You see a new tile titled Submitting deployment for Template deployment.</span></span> <span data-ttu-id="38304-178">Trvá přibližně 20 minut toocreate hello clusteru a databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="38304-178">It takes about around 20 minutes toocreate hello cluster and SQL database.</span></span>

<span data-ttu-id="38304-179">Pokud se rozhodnete toouse existující databázi Azure SQL nebo Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="38304-179">If you choose toouse existing Azure SQL database or Microsoft SQL Server</span></span>

* <span data-ttu-id="38304-180">**Databáze SQL Azure**: musíte nakonfigurovat pravidlo brány firewall pro přístup k Azure SQL database serveru tooallow hello z pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="38304-180">**Azure SQL database**: You must configure a firewall rule for hello Azure SQL database server tooallow access from your workstation.</span></span> <span data-ttu-id="38304-181">Pokyny týkající se vytváření databáze Azure SQL a konfiguraci brány firewall hello najdete v tématu [začít používat Azure SQL database][sqldatabase-get-started].</span><span class="sxs-lookup"><span data-stu-id="38304-181">For instructions about creating an Azure SQL database and configuring hello firewall, see [Get started using Azure SQL database][sqldatabase-get-started].</span></span> 
  
  > [!NOTE]
  > <span data-ttu-id="38304-182">Ve výchozím nastavení Azure SQL database umožňuje připojení z Azure služby, jako je Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="38304-182">By default an Azure SQL database allows connections from Azure services, such as Azure HDInsight.</span></span> <span data-ttu-id="38304-183">Pokud toto nastavení brány firewall je zakázáno, je třeba tooenable z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="38304-183">If this firewall setting is disabled, you need tooenable it from hello Azure portal.</span></span> <span data-ttu-id="38304-184">Pokyny týkající se vytváření databáze Azure SQL a konfigurace pravidel brány firewall, najdete v části [vytvořit a nakonfigurovat databázi SQL][sqldatabase-create-configue].</span><span class="sxs-lookup"><span data-stu-id="38304-184">For instruction about creating an Azure SQL database and configuring firewall rules, see [Create and Configure SQL Database][sqldatabase-create-configue].</span></span>
  > 
  > 
* <span data-ttu-id="38304-185">**SQL Server**: Pokud je váš cluster HDInsight na hello stejné virtuální síti v Azure jako systém SQL Server, můžete použít kroky hello Tento článek tooimport a export dat tooa databáze SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="38304-185">**SQL Server**: If your HDInsight cluster is on hello same virtual network in Azure as SQL Server, you can use hello steps in this article tooimport and export data tooa SQL Server database.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="38304-186">HDInsight podporuje pouze na základě umístění virtuální sítě a aktuálně nefunguje s virtuálních sítích založených na skupinu vztahů.</span><span class="sxs-lookup"><span data-stu-id="38304-186">HDInsight supports only location-based virtual networks, and it does not currently work with affinity group-based virtual networks.</span></span>
  > 
  > 
  
  * <span data-ttu-id="38304-187">toocreate a konfigurace virtuální sítě, najdete v části [vytvoření virtuální sítě pomocí portálu Azure hello](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="38304-187">toocreate and configure a virtual network, see [Create a virtual network using hello Azure portal](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span></span>
    
    * <span data-ttu-id="38304-188">Pokud používáte systém SQL Server ve vašem datovém centru, je nutné nakonfigurovat hello virtuální síti jako *site-to-site* nebo *point-to-site*.</span><span class="sxs-lookup"><span data-stu-id="38304-188">When you are using SQL Server in your datacenter, you must configure hello virtual network as *site-to-site* or *point-to-site*.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="38304-189">Pro **point-to-site** virtuální sítě, SQL Server musí používat klienta VPN hello konfigurace aplikace, která je k dispozici z hello **řídicí panel** konfigurace virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="38304-189">For **point-to-site** virtual networks, SQL Server must be running hello VPN client configuration application, which is available from hello **Dashboard** of your Azure virtual network configuration.</span></span>
      > 
      > 
    * <span data-ttu-id="38304-190">Při použití systému SQL Server na virtuální počítač Azure, lze použít všechny konfigurace virtuální sítě, pokud hello virtuálního počítače, který je hostitelem SQL serveru je členem hello stejné virtuální síti jako HDInsight.</span><span class="sxs-lookup"><span data-stu-id="38304-190">When you are using SQL Server on an Azure virtual machine, any virtual network configuration can be used if hello virtual machine hosting SQL Server is a member of hello same virtual network as HDInsight.</span></span>
  * <span data-ttu-id="38304-191">toocreate clusteru služby HDInsight ve virtuální síti, najdete v části [vytvoření Hadoop clusterů v HDInsight pomocí vlastních možností](hdinsight-hadoop-provision-linux-clusters.md)</span><span class="sxs-lookup"><span data-stu-id="38304-191">toocreate an HDInsight cluster on a virtual network, see [Create Hadoop clusters in HDInsight using custom options](hdinsight-hadoop-provision-linux-clusters.md)</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="38304-192">SQL Server musíte také povolit ověřování.</span><span class="sxs-lookup"><span data-stu-id="38304-192">SQL Server must also allow authentication.</span></span> <span data-ttu-id="38304-193">Musíte použít SQL Server hello toocomplete přihlášení kroky v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="38304-193">You must use a SQL Server login toocomplete hello steps in this article.</span></span>
    > 
    > 

## <a name="run-sqoop-jobs"></a><span data-ttu-id="38304-194">Spuštění úloh Sqoop</span><span class="sxs-lookup"><span data-stu-id="38304-194">Run Sqoop jobs</span></span>
<span data-ttu-id="38304-195">HDInsight Sqoop úlohy můžete spustit pomocí různých metod.</span><span class="sxs-lookup"><span data-stu-id="38304-195">HDInsight can run Sqoop jobs by using a variety of methods.</span></span> <span data-ttu-id="38304-196">Pomocí hello následující toodecide tabulku, která metoda je pro vás nejvhodnější a potom postupujte podle hello odkaz návod.</span><span class="sxs-lookup"><span data-stu-id="38304-196">Use hello following table toodecide which method is right for you, then follow hello link for a walkthrough.</span></span>

| <span data-ttu-id="38304-197">**Použít** Pokud chcete...</span><span class="sxs-lookup"><span data-stu-id="38304-197">**Use this** if you want...</span></span> | <span data-ttu-id="38304-198">.. .an **interaktivní** prostředí</span><span class="sxs-lookup"><span data-stu-id="38304-198">...an **interactive** shell</span></span> | <span data-ttu-id="38304-199">... **batch** zpracování</span><span class="sxs-lookup"><span data-stu-id="38304-199">...**batch** processing</span></span> | <span data-ttu-id="38304-200">.. při to **clusteru operačního systému**</span><span class="sxs-lookup"><span data-stu-id="38304-200">...with this **cluster operating system**</span></span> | <span data-ttu-id="38304-201">.. .from to **klientský operační systém**</span><span class="sxs-lookup"><span data-stu-id="38304-201">...from this **client operating system**</span></span> |
|:--- |:---:|:---:|:--- |:--- |
| [<span data-ttu-id="38304-202">SSH</span><span class="sxs-lookup"><span data-stu-id="38304-202">SSH</span></span>](hdinsight-use-sqoop-mac-linux.md) |<span data-ttu-id="38304-203">✔</span><span class="sxs-lookup"><span data-stu-id="38304-203">✔</span></span> |<span data-ttu-id="38304-204">✔</span><span class="sxs-lookup"><span data-stu-id="38304-204">✔</span></span> |<span data-ttu-id="38304-205">Linux</span><span class="sxs-lookup"><span data-stu-id="38304-205">Linux</span></span> |<span data-ttu-id="38304-206">Linux, Unix, Mac OS X nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="38304-206">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="38304-207">Sada .NET SDK pro Hadoop</span><span class="sxs-lookup"><span data-stu-id="38304-207">.NET SDK for Hadoop</span></span>](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |&nbsp; |<span data-ttu-id="38304-208">✔</span><span class="sxs-lookup"><span data-stu-id="38304-208">✔</span></span> |<span data-ttu-id="38304-209">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="38304-209">Linux or Windows</span></span> |<span data-ttu-id="38304-210">Windows (prozatím)</span><span class="sxs-lookup"><span data-stu-id="38304-210">Windows (for now)</span></span> |
| [<span data-ttu-id="38304-211">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="38304-211">Azure PowerShell</span></span>](hdinsight-hadoop-use-sqoop-powershell.md) |&nbsp; |<span data-ttu-id="38304-212">✔</span><span class="sxs-lookup"><span data-stu-id="38304-212">✔</span></span> |<span data-ttu-id="38304-213">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="38304-213">Linux or Windows</span></span> |<span data-ttu-id="38304-214">Windows</span><span class="sxs-lookup"><span data-stu-id="38304-214">Windows</span></span> |

## <a name="limitations"></a><span data-ttu-id="38304-215">Omezení</span><span class="sxs-lookup"><span data-stu-id="38304-215">Limitations</span></span>
* <span data-ttu-id="38304-216">Hromadně export - s Linuxovým systémem HDInsight, hello Sqoop konektor používaný tooexport data tooMicrosoft systému SQL Server nebo Azure SQL Database v současné době nepodporuje hromadné vložení.</span><span class="sxs-lookup"><span data-stu-id="38304-216">Bulk export - With Linux-based HDInsight, hello Sqoop connector used tooexport data tooMicrosoft SQL Server or Azure SQL Database does not currently support bulk inserts.</span></span>
* <span data-ttu-id="38304-217">Dávkování - s HDInsight se systémem Linux, při použití hello `-batch` přepnout při vložení, Sqoop provádí více vloží místo dávkování operace insert hello.</span><span class="sxs-lookup"><span data-stu-id="38304-217">Batching - With Linux-based HDInsight, When using hello `-batch` switch when performing inserts, Sqoop performs multiple inserts instead of batching hello insert operations.</span></span>

## <a name="next-steps"></a><span data-ttu-id="38304-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="38304-218">Next steps</span></span>
<span data-ttu-id="38304-219">Nyní jste se naučili, jak toouse Sqoop.</span><span class="sxs-lookup"><span data-stu-id="38304-219">Now you have learned how toouse Sqoop.</span></span> <span data-ttu-id="38304-220">toolearn více, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="38304-220">toolearn more, see:</span></span>

* [<span data-ttu-id="38304-221">Použití Hivu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="38304-221">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="38304-222">Použití Pigu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="38304-222">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* <span data-ttu-id="38304-223">[Použijte Oozie s HDInsight][hdinsight-use-oozie]: použití Sqoop akce v pracovním postupu Oozie.</span><span class="sxs-lookup"><span data-stu-id="38304-223">[Use Oozie with HDInsight][hdinsight-use-oozie]: Use Sqoop action in an Oozie workflow.</span></span>
* <span data-ttu-id="38304-224">[Analýza dat zpoždění letu pomocí HDInsight][hdinsight-analyze-flight-data]: použití Hive letu tooanalyze zpoždění data a pak použijte Sqoop tooexport data tooan Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="38304-224">[Analyze flight delay data using HDInsight][hdinsight-analyze-flight-data]: Use Hive tooanalyze flight delay data, and then use Sqoop tooexport data tooan Azure SQL database.</span></span>
* <span data-ttu-id="38304-225">[Nahrání dat tooHDInsight][hdinsight-upload-data]: Najít další metody pro odesílání dat tooHDInsight/Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="38304-225">[Upload data tooHDInsight][hdinsight-upload-data]: Find other methods for uploading data tooHDInsight/Azure Blob storage.</span></span>

## <a name="appendix-a---a-powershell-sample"></a><span data-ttu-id="38304-226">Příloha A - ukázku prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="38304-226">Appendix A - a PowerShell sample</span></span>
<span data-ttu-id="38304-227">Ukázkové prostředí PowerShell Hello provádí hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="38304-227">hello PowerShell sample performs hello following steps:</span></span>

1. <span data-ttu-id="38304-228">Připojte tooAzure.</span><span class="sxs-lookup"><span data-stu-id="38304-228">Connect tooAzure.</span></span>
2. <span data-ttu-id="38304-229">Vytvořte skupinu prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="38304-229">Create an Azure resource group.</span></span> <span data-ttu-id="38304-230">Další informace najdete v tématu [použití Azure Powershellu s Azure Resource Manager](../powershell-azure-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="38304-230">For more information, see [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md)</span></span>
3. <span data-ttu-id="38304-231">Vytvoření serveru Azure SQL Database, Azure SQL database a dvě tabulky.</span><span class="sxs-lookup"><span data-stu-id="38304-231">Create an Azure SQL Database server, an Azure SQL database, and two tables.</span></span> 
   
    <span data-ttu-id="38304-232">Pokud místo toho používat SQL Server, použijte následující příkazy toocreate hello tabulky hello:</span><span class="sxs-lookup"><span data-stu-id="38304-232">If you use SQL Server instead, use hello following statements toocreate hello tables:</span></span>
   
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
   
    <span data-ttu-id="38304-233">Hello nejjednodušší způsob, jak tooexamine hello databáze a tabulky je toouse Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="38304-233">hello easiest way tooexamine hello database and tables is toouse Visual Studio.</span></span> <span data-ttu-id="38304-234">Hello databázového serveru a hello databáze může být prověřen pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="38304-234">hello database server and hello database can be examined using hello Azure portal.</span></span>
4. <span data-ttu-id="38304-235">Vytvoření clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="38304-235">Create an HDInsight cluster.</span></span>
   
    <span data-ttu-id="38304-236">tooexamine hello clusteru, můžete použít hello portál Azure nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="38304-236">tooexamine hello cluster, you can use hello Azure portal or Azure PowerShell.</span></span>
5. <span data-ttu-id="38304-237">Předběžně zpracovat hello zdrojového datového souboru.</span><span class="sxs-lookup"><span data-stu-id="38304-237">Pre-process hello source data file.</span></span>
   
    <span data-ttu-id="38304-238">V tomto kurzu můžete exportovat soubor protokolu log4j (soubor s oddělovači) a databázi Azure SQL tooan tabulku Hive.</span><span class="sxs-lookup"><span data-stu-id="38304-238">In this tutorial, you export a log4j log file (a delimited file) and a Hive table tooan Azure SQL database.</span></span> <span data-ttu-id="38304-239">Hello souboru s oddělovači se nazývá */example/data/sample.log*.</span><span class="sxs-lookup"><span data-stu-id="38304-239">hello delimited file is called */example/data/sample.log*.</span></span> <span data-ttu-id="38304-240">V kurzu hello jste viděli několik ukázky log4j protokolů.</span><span class="sxs-lookup"><span data-stu-id="38304-240">Earlier in hello tutorial, you saw a few samples of log4j logs.</span></span> <span data-ttu-id="38304-241">V souboru protokolu hello existují některé prázdné řádky a podobné toothese některé řádky:</span><span class="sxs-lookup"><span data-stu-id="38304-241">In hello log file, there are some empty lines and some lines similar toothese:</span></span>
   
        java.lang.Exception: 2012-02-03 20:11:35 SampleClass2 [FATAL] unrecoverable system problem at id 609774657
            at com.osa.mocklogger.MockLogger$2.run(MockLogger.java:83)
   
    <span data-ttu-id="38304-242">To je v pořádku pro další příklady, které používají tato data, ale před jsme můžete importovat do hello Azure SQL database nebo SQL Server jsme musíte odebrat tyto výjimky.</span><span class="sxs-lookup"><span data-stu-id="38304-242">This is fine for other examples that use this data, but we must remove these exceptions before we can import into hello Azure SQL database or SQL Server.</span></span> <span data-ttu-id="38304-243">Sqoop export se nezdaří, pokud je prázdný řetězec nebo čáry s méně než hello počet polí definovaných v tabulce databáze Azure SQL hello elementy.</span><span class="sxs-lookup"><span data-stu-id="38304-243">Sqoop export  fails if there is an empty string or a line with a fewer elements than hello number of fields defined in hello Azure SQL database table.</span></span> <span data-ttu-id="38304-244">Tabulka log4jlogs Hello má 7 řetězec typu pole.</span><span class="sxs-lookup"><span data-stu-id="38304-244">hello log4jlogs table has 7 string-type fields.</span></span>
   
    <span data-ttu-id="38304-245">Tento postup vytvoří nový soubor v clusteru hello: tutorials/usesqoop/data/sample.log.</span><span class="sxs-lookup"><span data-stu-id="38304-245">This procedure creates a new file on hello cluster: tutorials/usesqoop/data/sample.log.</span></span> <span data-ttu-id="38304-246">tooexamine hello upravené datový soubor, můžete použít hello portálu Azure, nástroji Průzkumník Azure Storage nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="38304-246">tooexamine hello modified data file, you can use hello Azure portal, an Azure Storage explorer tool, or Azure PowerShell.</span></span> <span data-ttu-id="38304-247">[Začínáme s HDInsight] [ hdinsight-get-started] má kód ukázkové pro použití prostředí Azure PowerShell toodownload soubor a zobrazit obsah souboru hello.</span><span class="sxs-lookup"><span data-stu-id="38304-247">[Get started with HDInsight][hdinsight-get-started] has a code sample for using Azure PowerShell toodownload a file and display hello file content.</span></span>
6. <span data-ttu-id="38304-248">Exportujte databáze Azure SQL data souboru toohello.</span><span class="sxs-lookup"><span data-stu-id="38304-248">Export a data file toohello Azure SQL database.</span></span>
   
    <span data-ttu-id="38304-249">zdrojový soubor Hello je tutorials/usesqoop/data/sample.log.</span><span class="sxs-lookup"><span data-stu-id="38304-249">hello source file is tutorials/usesqoop/data/sample.log.</span></span> <span data-ttu-id="38304-250">Tabulka Hello kde hello dat je exportovaný toois nazývá log4jlogs.</span><span class="sxs-lookup"><span data-stu-id="38304-250">hello table where hello data is exported toois called log4jlogs.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="38304-251">Než informace o připojovacím řetězci by měly fungovat hello kroky v této části pro Azure SQL database nebo SQL Server.</span><span class="sxs-lookup"><span data-stu-id="38304-251">Other than connection string information, hello steps in this section should work for an Azure SQL database or for SQL Server.</span></span> <span data-ttu-id="38304-252">Tyto kroky testovali pomocí hello následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="38304-252">These steps were tested by using hello following configuration:</span></span>
   > 
   > * <span data-ttu-id="38304-253">**Konfigurace point-to-site virtuální síť Azure**: virtuální síť připojená hello HDInsight clusteru tooa systému SQL Server v privátním datacentru.</span><span class="sxs-lookup"><span data-stu-id="38304-253">**Azure virtual network point-to-site configuration**: A virtual network connected hello HDInsight cluster tooa SQL Server in a private datacenter.</span></span> <span data-ttu-id="38304-254">V tématu [konfigurace VPN typu Point-to-Site v hello portálu pro správu](../vpn-gateway/vpn-gateway-point-to-site-create.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="38304-254">See [Configure a Point-to-Site VPN in hello Management Portal](../vpn-gateway/vpn-gateway-point-to-site-create.md) for more information.</span></span>
   > * <span data-ttu-id="38304-255">**Azure HDInsight 3.1**: najdete v části [vytvoření Hadoop clusterů v HDInsight pomocí vlastních možností](hdinsight-hadoop-provision-linux-clusters.md) informace o vytváření clusteru s podporou ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="38304-255">**Azure HDInsight 3.1**: See [Create Hadoop clusters in HDInsight using custom options](hdinsight-hadoop-provision-linux-clusters.md) for information about creating a cluster on a virtual network.</span></span>
   > * <span data-ttu-id="38304-256">**SQL Server 2014**: nakonfigurovali tooallow ověřování a spuštěné hello VPN klienta konfigurace balíčku tooconnect bezpečně toohello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="38304-256">**SQL Server 2014**: Configured tooallow authentication and running hello VPN client configuration package tooconnect securely toohello virtual network.</span></span>
   > 
   > 
7. <span data-ttu-id="38304-257">Exportujte databáze Azure SQL toohello tabulku Hive.</span><span class="sxs-lookup"><span data-stu-id="38304-257">Export a Hive table toohello Azure SQL database.</span></span>
8. <span data-ttu-id="38304-258">Importujte clusteru HDInsight toohello tabulky mobiledata hello.</span><span class="sxs-lookup"><span data-stu-id="38304-258">Import hello mobiledata table toohello HDInsight cluster.</span></span>
   
    <span data-ttu-id="38304-259">tooexamine hello upravené datový soubor, můžete použít hello portálu Azure, nástroji Průzkumník Azure Storage nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="38304-259">tooexamine hello modified data file, you can use hello Azure portal, an Azure Storage explorer tool, or Azure PowerShell.</span></span>  <span data-ttu-id="38304-260">[Začínáme s HDInsight] [ hdinsight-get-started] má kód ukázkové o použití prostředí Azure PowerShell toodownload soubor a zobrazit obsah souboru hello.</span><span class="sxs-lookup"><span data-stu-id="38304-260">[Get started with HDInsight][hdinsight-get-started] has a code sample about using Azure PowerShell toodownload a file and display hello file content.</span></span>

### <a name="hello-powershell-sample"></a><span data-ttu-id="38304-261">Ukázka Hello prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="38304-261">hello PowerShell sample</span></span>
    # Prepare an Azure SQL database toobe used by hello Sqoop tutorial

    #region - provide hello following values

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

    #region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
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

        #tooallow other Azure services tooaccess hello server add a firewall rule and set both hello StartIpAddress and EndIpAddress too0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription tooaccess hello server.
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
    Write-Host "Creating hello log4jlogs table and hello mobiledata table ..." -ForegroundColor Green

    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()

    # Create hello log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jTable
    $ret = $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateLog4jClusteredIndex
    $cmd.ExecuteNonQuery()

    # Create hello mobiledata table and index
    $cmd.CommandText = $cmdCreateMobileTable
    $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateMobileDataClusteredIndex
    $cmd.ExecuteNonQuery()

    $conn.close()

    #endregion


    #region - Create HDInsight cluster

    Write-Host "Creating hello HDInsight cluster and hello dependent services ..." -ForegroundColor Green

    # Create hello default storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS

    # Create hello default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 

    # Create hello HDInsight cluster
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

    # Validate hello cluster
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion

    #region - pre-process hello source file

    Write-Host "Preprocessing hello source file ..." -ForegroundColor Green

    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"

    # Define hello connection string
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

    # Create block blob objects referencing hello source and destination blob.
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)

    # Define a MemoryStream and a StreamReader for reading from hello source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)

    # Define a MemoryStream and a StreamWriter for writing into hello destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream

    # Pre-process hello source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")

        # remove hello "java.lang.Exception" from hello first element of hello array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList tooremove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)

            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }

        # remove hello lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }

    # Write toohello destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)

    #endregion

    #region - export a log file from hello cluster toohello SQL database

    Write-Host "Preprocessing hello source file ..." -ForegroundColor Green

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
