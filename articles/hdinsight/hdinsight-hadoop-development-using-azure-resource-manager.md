---
title: "Migrace do nástroje Správce prostředků Azure pro HDInsight | Microsoft Docs"
description: "Postup migrace na vývojové nástroje Azure Resource Manageru pro clustery služby HDInsight na"
services: hdinsight
editor: cgronlun
manager: jhubbard
author: nitinme
documentationcenter: 
ms.assetid: 05efedb5-6456-4552-87ff-156d77fbe2e1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 708d22b9ce53d4dbc07c6bcde0c46dcd238291bb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="migrating-to-azure-resource-manager-based-development-tools-for-hdinsight-clusters"></a><span data-ttu-id="8dbf3-103">Migrace na Azure Resource Manager vývojové nástroje založené pro clustery služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="8dbf3-103">Migrating to Azure Resource Manager-based development tools for HDInsight clusters</span></span>

<span data-ttu-id="8dbf3-104">HDInsight je místo na základě Azure Service Manager ASM nástroje začne pro HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-104">HDInsight is deprecating Azure Service Manager (ASM)-based tools for HDInsight.</span></span> <span data-ttu-id="8dbf3-105">Pokud používáte prostředí Azure PowerShell, rozhraní příkazového řádku Azure nebo sady SDK rozhraní .NET HDInsight pro práci s clustery HDInsight, můžete se doporučuje používat založené na Azure Resource Manager ARM verze prostředí PowerShell, rozhraní příkazového řádku a .NET SDK do budoucna.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-105">If you have been using Azure PowerShell, Azure CLI, or the HDInsight .NET SDK to work with HDInsight clusters, you are encouraged to use the Azure Resource Manager (ARM)-based versions of PowerShell, CLI, and .NET SDK going forward.</span></span> <span data-ttu-id="8dbf3-106">Tento článek obsahuje ukazatele na tom, jak migrovat do nového přístupu založené na ARM.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-106">This article provides pointers on how to migrate to the new ARM-based approach.</span></span> <span data-ttu-id="8dbf3-107">Bez ohledu na příslušném Tento článek také ukazuje out rozdíly mezi ASM a ARM přístupy pro HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-107">Wherever applicable, this article also points out the differences between the ASM and ARM approaches for HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8dbf3-108">Podpora pro ASM na základě prostředí PowerShell, rozhraní příkazového řádku, a .NET SDK se ukončí na **1. ledna 2017**.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-108">The support for ASM based PowerShell, CLI, and .NET SDK will discontinue on **January 1, 2017**.</span></span>
> 
> 

## <a name="migrating-azure-cli-to-azure-resource-manager"></a><span data-ttu-id="8dbf3-109">Migrace rozhraní příkazového řádku Azure do Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="8dbf3-109">Migrating Azure CLI to Azure Resource Manager</span></span>
<span data-ttu-id="8dbf3-110">Azure CLI nyní výchozí do režimu Azure Resource Manager (ARM), pokud provádíte upgrade z předchozí instalace; v takovém případě budete možná muset použít `azure config mode arm` příkaz Přepnout do režimu ARM.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-110">The Azure CLI now defaults to Azure Resource Manager (ARM) mode, unless you are upgrading from a previous installation; in this case, you may need to use the `azure config mode arm` command to switch to ARM mode.</span></span>

<span data-ttu-id="8dbf3-111">Základní příkazy, které rozhraní příkazového řádku Azure k dispozici pro práci s HDInsight pomocí Azure Service Management (ASM) jsou stejné, když pomocí modelu ARM; ale některé parametry a přepínače může mít nové názvy a existuje mnoho nových parametrů při použití ARM.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-111">The basic commands that the Azure CLI provided to work with HDInsight using Azure Service Management (ASM) are the same when using ARM; however some parameters and switches may have new names, and there are many new parameters available when using ARM.</span></span> <span data-ttu-id="8dbf3-112">Například můžete nyní použít `azure hdinsight cluster create` k určení virtuální síť Azure, který by měl být vytvořen v clusteru, nebo Hive a informace metaúložiště Oozie.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-112">For example, you can now use `azure hdinsight cluster create` to specify the Azure Virtual Network that a cluster should be created in, or Hive and Oozie metastore information.</span></span>

<span data-ttu-id="8dbf3-113">Základní příkazy pro práci s HDInsight prostřednictvím Správce Azure Resource Manager jsou:</span><span class="sxs-lookup"><span data-stu-id="8dbf3-113">Basic commands for working with HDInsight through Azure Resource Manager are:</span></span>

* <span data-ttu-id="8dbf3-114">`azure hdinsight cluster create`-Vytvoří nový cluster HDInsight</span><span class="sxs-lookup"><span data-stu-id="8dbf3-114">`azure hdinsight cluster create` - creates a new HDInsight cluster</span></span>
* <span data-ttu-id="8dbf3-115">`azure hdinsight cluster delete`-Odstraní stávající cluster HDInsight</span><span class="sxs-lookup"><span data-stu-id="8dbf3-115">`azure hdinsight cluster delete` - deletes an existing HDInsight cluster</span></span>
* <span data-ttu-id="8dbf3-116">`azure hdinsight cluster show`– Zobrazí informace o stávajícího clusteru</span><span class="sxs-lookup"><span data-stu-id="8dbf3-116">`azure hdinsight cluster show` - display information about an existing cluster</span></span>
* <span data-ttu-id="8dbf3-117">`azure hdinsight cluster list`-obsahuje seznam clustery HDInsight pro vaše předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="8dbf3-117">`azure hdinsight cluster list` - lists HDInsight clusters for your Azure subscription</span></span>

<span data-ttu-id="8dbf3-118">Použití `-h` přepínač tak, aby kontrolovat parametry a přepínače, které jsou k dispozici u každého příkazu.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-118">Use the `-h` switch to inspect the parameters and switches available for each command.</span></span>

### <a name="new-commands"></a><span data-ttu-id="8dbf3-119">Nové příkazy</span><span class="sxs-lookup"><span data-stu-id="8dbf3-119">New commands</span></span>
<span data-ttu-id="8dbf3-120">Nové příkazy, které jsou k dispozici s Azure Resource Manager jsou:</span><span class="sxs-lookup"><span data-stu-id="8dbf3-120">New commands available with Azure Resource Manager are:</span></span>

* <span data-ttu-id="8dbf3-121">`azure hdinsight cluster resize`-dynamicky změní počet uzlů pracovního procesu v clusteru</span><span class="sxs-lookup"><span data-stu-id="8dbf3-121">`azure hdinsight cluster resize` - dynamically changes the number of worker nodes in the cluster</span></span>
* <span data-ttu-id="8dbf3-122">`azure hdinsight cluster enable-http-access`– Umožňuje HTTPs přístup ke clusteru (na ve výchozím nastavení)</span><span class="sxs-lookup"><span data-stu-id="8dbf3-122">`azure hdinsight cluster enable-http-access` - enables HTTPs access to the cluster (on by default)</span></span>
* <span data-ttu-id="8dbf3-123">`azure hdinsight cluster disable-http-access`– Zakáže HTTPs přístup ke clusteru</span><span class="sxs-lookup"><span data-stu-id="8dbf3-123">`azure hdinsight cluster disable-http-access` - disables HTTPs access to the cluster</span></span>
* <span data-ttu-id="8dbf3-124">`azure hdinsight script-action`-obsahuje příkazy pro vytváření nebo správu akcí skriptů v clusteru</span><span class="sxs-lookup"><span data-stu-id="8dbf3-124">`azure hdinsight script-action` - provides commands for creating/managing Script Actions on a cluster</span></span>
* <span data-ttu-id="8dbf3-125">`azure hdinsight config`-poskytuje příkazů pro vytvoření konfiguračního souboru, který lze použít s `hdinsight cluster create` příkaz poskytnout informace o konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-125">`azure hdinsight config` - provides commands for creating a configuration file that can be used with the `hdinsight cluster create` command to provide configuration information.</span></span>

### <a name="deprecated-commands"></a><span data-ttu-id="8dbf3-126">Nepoužívané příkazy</span><span class="sxs-lookup"><span data-stu-id="8dbf3-126">Deprecated commands</span></span>
<span data-ttu-id="8dbf3-127">Pokud použijete `azure hdinsight job` příkazy k odesílání úloh do clusteru HDInsight, tyto nejsou k dispozici prostřednictvím příkazy ARM.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-127">If you use the `azure hdinsight job` commands to submit jobs to your HDInsight cluster, these are not available through the ARM commands.</span></span> <span data-ttu-id="8dbf3-128">Pokud potřebujete prostřednictvím kódu programu odesílání úloh do HDInsight z skriptů, měli byste místo toho použít rozhraní REST API poskytovaných v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-128">If you need to programmatically submit jobs to HDInsight from scripts, you should instead use the REST APIs provided by HDInsight.</span></span> <span data-ttu-id="8dbf3-129">Další informace o odesílání úloh pomocí rozhraní REST API najdete v následujících dokumentech.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-129">For more information on submitting jobs using REST APIs, see the following documents.</span></span>

* [<span data-ttu-id="8dbf3-130">Spuštění úloh MapReduce s Hadoop v HDInsight pomocí cURL</span><span class="sxs-lookup"><span data-stu-id="8dbf3-130">Run MapReduce jobs with Hadoop on HDInsight using cURL</span></span>](hdinsight-hadoop-use-mapreduce-curl.md)
* [<span data-ttu-id="8dbf3-131">Spouštění dotazů Hive se systémem Hadoop v HDInsight pomocí cURL</span><span class="sxs-lookup"><span data-stu-id="8dbf3-131">Run Hive queries with Hadoop on HDInsight using cURL</span></span>](hdinsight-hadoop-use-hive-curl.md)
* [<span data-ttu-id="8dbf3-132">Spuštění úlohy Pig s Hadoop v HDInsight pomocí cURL</span><span class="sxs-lookup"><span data-stu-id="8dbf3-132">Run Pig jobs with Hadoop on HDInsight using cURL</span></span>](hdinsight-hadoop-use-pig-curl.md)

<span data-ttu-id="8dbf3-133">Informace o jiných způsobech spustit MapReduce, Hive a vepřových interaktivně, najdete v části [používání MapReduce s Hadoop v HDInsight](hdinsight-use-mapreduce.md), [používání Hive s Hadoop v HDInsight](hdinsight-use-hive.md), a [použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md).</span><span class="sxs-lookup"><span data-stu-id="8dbf3-133">For information on other ways to run MapReduce, Hive, and Pig interactively, see [Use MapReduce with Hadoop on HDInsight](hdinsight-use-mapreduce.md), [Use Hive with Hadoop on HDInsight](hdinsight-use-hive.md), and [Use Pig with Hadoop on HDInsight](hdinsight-use-pig.md).</span></span>

### <a name="examples"></a><span data-ttu-id="8dbf3-134">Příklady</span><span class="sxs-lookup"><span data-stu-id="8dbf3-134">Examples</span></span>
<span data-ttu-id="8dbf3-135">**Vytvoření clusteru**</span><span class="sxs-lookup"><span data-stu-id="8dbf3-135">**Creating a cluster**</span></span>

* <span data-ttu-id="8dbf3-136">Příkaz staré (ASM)-`azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`</span><span class="sxs-lookup"><span data-stu-id="8dbf3-136">Old command (ASM) - `azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`</span></span>
* <span data-ttu-id="8dbf3-137">Nový příkaz (ARM)-`azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`</span><span class="sxs-lookup"><span data-stu-id="8dbf3-137">New command (ARM) - `azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`</span></span>

<span data-ttu-id="8dbf3-138">**Odstranění clusteru**</span><span class="sxs-lookup"><span data-stu-id="8dbf3-138">**Deleting a cluster**</span></span>

* <span data-ttu-id="8dbf3-139">Příkaz staré (ASM)-`azure hdinsight cluster delete myhdicluster`</span><span class="sxs-lookup"><span data-stu-id="8dbf3-139">Old command (ASM) - `azure hdinsight cluster delete myhdicluster`</span></span>
* <span data-ttu-id="8dbf3-140">Nový příkaz (ARM)-`azure hdinsight cluster delete mycluster -g myresourcegroup`</span><span class="sxs-lookup"><span data-stu-id="8dbf3-140">New command (ARM) - `azure hdinsight cluster delete mycluster -g myresourcegroup`</span></span>

<span data-ttu-id="8dbf3-141">**Seznam clustery**</span><span class="sxs-lookup"><span data-stu-id="8dbf3-141">**List clusters**</span></span>

* <span data-ttu-id="8dbf3-142">Příkaz staré (ASM)-`azure hdinsight cluster list`</span><span class="sxs-lookup"><span data-stu-id="8dbf3-142">Old command (ASM) - `azure hdinsight cluster list`</span></span>
* <span data-ttu-id="8dbf3-143">Nový příkaz (ARM)-`azure hdinsight cluster list`</span><span class="sxs-lookup"><span data-stu-id="8dbf3-143">New command (ARM) - `azure hdinsight cluster list`</span></span>

> [!NOTE]
> <span data-ttu-id="8dbf3-144">Pro příkaz seznamu zadání skupiny prostředků pomocí `-g` vrátí pouze clustery v zadaná skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-144">For the list command, specifying the resource group using `-g` will return only the clusters in the specified resource group.</span></span>
> 
> 

<span data-ttu-id="8dbf3-145">**Zobrazit informace o clusteru**</span><span class="sxs-lookup"><span data-stu-id="8dbf3-145">**Show cluster information**</span></span>

* <span data-ttu-id="8dbf3-146">Příkaz staré (ASM)-`azure hdinsight cluster show myhdicluster`</span><span class="sxs-lookup"><span data-stu-id="8dbf3-146">Old command (ASM) - `azure hdinsight cluster show myhdicluster`</span></span>
* <span data-ttu-id="8dbf3-147">Nový příkaz (ARM)-`azure hdinsight cluster show myhdicluster -g myresourcegroup`</span><span class="sxs-lookup"><span data-stu-id="8dbf3-147">New command (ARM) - `azure hdinsight cluster show myhdicluster -g myresourcegroup`</span></span>

## <a name="migrating-azure-powershell-to-azure-resource-manager"></a><span data-ttu-id="8dbf3-148">Migrace prostředí Azure PowerShell pro Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="8dbf3-148">Migrating Azure PowerShell to Azure Resource Manager</span></span>
<span data-ttu-id="8dbf3-149">Obecné informace o prostředí Azure PowerShell v režimu Azure Resource Manager (ARM) najdete v [použití Azure Powershellu s Azure Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="8dbf3-149">The general information about Azure PowerShell in the Azure Resource Manager (ARM) mode can be found at [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="8dbf3-150">Rutiny Azure PowerShell ARM může být nainstalovaná-souběžného s rutinami ASM.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-150">The Azure PowerShell ARM cmdlets can be installed side-by-side with the ASM cmdlets.</span></span> <span data-ttu-id="8dbf3-151">Rutiny ze dvou režimů dají rozlišovat podle jejich názvy.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-151">The cmdlets from the two modes can be distinguished by their names.</span></span>  <span data-ttu-id="8dbf3-152">Má režimu ARM *AzureRmHDInsight* v porovnání se názvy rutin *AzureHDInsight* v režimu ASM.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-152">The ARM mode has *AzureRmHDInsight* in the cmdlet names comparing to *AzureHDInsight* in the ASM mode.</span></span>  <span data-ttu-id="8dbf3-153">Například *New-AzureRmHDInsightCluster* vs. *Nové AzureHDInsightCluster*.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-153">For example, *New-AzureRmHDInsightCluster* vs. *New-AzureHDInsightCluster*.</span></span> <span data-ttu-id="8dbf3-154">Názvy zprávy mohou mít parametry a přepínače, existuje mnoho nových parametrů k dispozici při použití a ARM.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-154">Parameters and switches may have news names, and there are many new parameters available when using ARM.</span></span>  <span data-ttu-id="8dbf3-155">Například několik rutin vyžadovat nového přepínače názvem *- ResourceGroupName*.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-155">For example, several cmdlets require a new switch called *-ResourceGroupName*.</span></span> 

<span data-ttu-id="8dbf3-156">Před použitím rutin HDInsight, musíte připojit k účtu Azure a vytvořit novou skupinu prostředků:</span><span class="sxs-lookup"><span data-stu-id="8dbf3-156">Before you can use the HDInsight cmdlets, you must connect to your Azure account, and create a new resource group:</span></span>

* <span data-ttu-id="8dbf3-157">Login-AzureRmAccount nebo [vyberte AzureRmProfile](https://msdn.microsoft.com/library/mt619310.aspx).</span><span class="sxs-lookup"><span data-stu-id="8dbf3-157">Login-AzureRmAccount or [Select-AzureRmProfile](https://msdn.microsoft.com/library/mt619310.aspx).</span></span> <span data-ttu-id="8dbf3-158">V tématu [ověřování hlavní název služby pomocí Azure Resource Manageru](../azure-resource-manager/resource-group-authenticate-service-principal.md)</span><span class="sxs-lookup"><span data-stu-id="8dbf3-158">See [Authenticating a service principal with Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md)</span></span>
* [<span data-ttu-id="8dbf3-159">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="8dbf3-159">New-AzureRmResourceGroup</span></span>](https://msdn.microsoft.com/library/mt603739.aspx)

### <a name="renamed-cmdlets"></a><span data-ttu-id="8dbf3-160">Přejmenovat rutiny</span><span class="sxs-lookup"><span data-stu-id="8dbf3-160">Renamed cmdlets</span></span>
<span data-ttu-id="8dbf3-161">Seznam rutin HDInsight ASM v konzole Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="8dbf3-161">To list the HDInsight ASM cmdlets in Windows PowerShell console:</span></span>

    help *azurermhdinsight*

<span data-ttu-id="8dbf3-162">Následující tabulka uvádí jejich názvy v režimu ARM a ASM rutiny:</span><span class="sxs-lookup"><span data-stu-id="8dbf3-162">The following table lists the ASM cmdlets and their names in the ARM mode:</span></span>

| <span data-ttu-id="8dbf3-163">Rutiny ASM</span><span class="sxs-lookup"><span data-stu-id="8dbf3-163">ASM cmdlets</span></span> | <span data-ttu-id="8dbf3-164">Rutiny ARM</span><span class="sxs-lookup"><span data-stu-id="8dbf3-164">ARM cmdlets</span></span> |
| --- | --- |
| <span data-ttu-id="8dbf3-165">Add-AzureHDInsightConfigValues</span><span class="sxs-lookup"><span data-stu-id="8dbf3-165">Add-AzureHDInsightConfigValues</span></span> |[<span data-ttu-id="8dbf3-166">Přidat AzureRmHDInsightConfigValues</span><span class="sxs-lookup"><span data-stu-id="8dbf3-166">Add-AzureRmHDInsightConfigValues</span></span>](https://msdn.microsoft.com/library/mt603530.aspx) |
| <span data-ttu-id="8dbf3-167">Add-AzureHDInsightMetastore</span><span class="sxs-lookup"><span data-stu-id="8dbf3-167">Add-AzureHDInsightMetastore</span></span> |[<span data-ttu-id="8dbf3-168">Přidat AzureRmHDInsightMetastore</span><span class="sxs-lookup"><span data-stu-id="8dbf3-168">Add-AzureRmHDInsightMetastore</span></span>](https://msdn.microsoft.com/library/mt603670.aspx) |
| <span data-ttu-id="8dbf3-169">Add-AzureHDInsightScriptAction</span><span class="sxs-lookup"><span data-stu-id="8dbf3-169">Add-AzureHDInsightScriptAction</span></span> |[<span data-ttu-id="8dbf3-170">Přidat AzureRmHDInsightScriptAction</span><span class="sxs-lookup"><span data-stu-id="8dbf3-170">Add-AzureRmHDInsightScriptAction</span></span>](https://msdn.microsoft.com/library/mt603527.aspx) |
| <span data-ttu-id="8dbf3-171">Add-AzureHDInsightStorage</span><span class="sxs-lookup"><span data-stu-id="8dbf3-171">Add-AzureHDInsightStorage</span></span> |[<span data-ttu-id="8dbf3-172">Přidat AzureRmHDInsightStorage</span><span class="sxs-lookup"><span data-stu-id="8dbf3-172">Add-AzureRmHDInsightStorage</span></span>](https://msdn.microsoft.com/library/mt619445.aspx) |
| <span data-ttu-id="8dbf3-173">Get-AzureHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="8dbf3-173">Get-AzureHDInsightCluster</span></span> |[<span data-ttu-id="8dbf3-174">Get-AzureRmHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="8dbf3-174">Get-AzureRmHDInsightCluster</span></span>](https://msdn.microsoft.com/library/mt619371.aspx) |
| <span data-ttu-id="8dbf3-175">Get-AzureHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="8dbf3-175">Get-AzureHDInsightJob</span></span> |[<span data-ttu-id="8dbf3-176">Get-AzureRmHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="8dbf3-176">Get-AzureRmHDInsightJob</span></span>](https://msdn.microsoft.com/library/mt603590.aspx) |
| <span data-ttu-id="8dbf3-177">Get-AzureHDInsightJobOutput</span><span class="sxs-lookup"><span data-stu-id="8dbf3-177">Get-AzureHDInsightJobOutput</span></span> |[<span data-ttu-id="8dbf3-178">Get-AzureRmHDInsightJobOutput</span><span class="sxs-lookup"><span data-stu-id="8dbf3-178">Get-AzureRmHDInsightJobOutput</span></span>](https://msdn.microsoft.com/library/mt603793.aspx) |
| <span data-ttu-id="8dbf3-179">Get-AzureHDInsightProperties</span><span class="sxs-lookup"><span data-stu-id="8dbf3-179">Get-AzureHDInsightProperties</span></span> |[<span data-ttu-id="8dbf3-180">Get-AzureRmHDInsightProperties</span><span class="sxs-lookup"><span data-stu-id="8dbf3-180">Get-AzureRmHDInsightProperties</span></span>](https://msdn.microsoft.com/library/mt603546.aspx) |
| <span data-ttu-id="8dbf3-181">Grant-AzureHDInsightHttpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="8dbf3-181">Grant-AzureHDInsightHttpServicesAccess</span></span> |[<span data-ttu-id="8dbf3-182">Udělení AzureRmHDInsightHttpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="8dbf3-182">Grant-AzureRmHDInsightHttpServicesAccess</span></span>](https://msdn.microsoft.com/library/mt619407.aspx) |
| <span data-ttu-id="8dbf3-183">Grant-AzureHdinsightRdpAccess</span><span class="sxs-lookup"><span data-stu-id="8dbf3-183">Grant-AzureHdinsightRdpAccess</span></span> |[<span data-ttu-id="8dbf3-184">Udělení AzureRmHDInsightRdpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="8dbf3-184">Grant-AzureRmHDInsightRdpServicesAccess</span></span>](https://msdn.microsoft.com/library/mt603717.aspx) |
| <span data-ttu-id="8dbf3-185">Invoke-AzureHDInsightHiveJob</span><span class="sxs-lookup"><span data-stu-id="8dbf3-185">Invoke-AzureHDInsightHiveJob</span></span> |[<span data-ttu-id="8dbf3-186">Vyvolání AzureRmHDInsightHiveJob</span><span class="sxs-lookup"><span data-stu-id="8dbf3-186">Invoke-AzureRmHDInsightHiveJob</span></span>](https://msdn.microsoft.com/library/mt603593.aspx) |
| <span data-ttu-id="8dbf3-187">New-AzureHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="8dbf3-187">New-AzureHDInsightCluster</span></span> |[<span data-ttu-id="8dbf3-188">Nové AzureRmHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="8dbf3-188">New-AzureRmHDInsightCluster</span></span>](https://msdn.microsoft.com/library/mt619331.aspx) |
| <span data-ttu-id="8dbf3-189">New-AzureHDInsightClusterConfig</span><span class="sxs-lookup"><span data-stu-id="8dbf3-189">New-AzureHDInsightClusterConfig</span></span> |[<span data-ttu-id="8dbf3-190">Nové AzureRmHDInsightClusterConfig</span><span class="sxs-lookup"><span data-stu-id="8dbf3-190">New-AzureRmHDInsightClusterConfig</span></span>](https://msdn.microsoft.com/library/mt603700.aspx) |
| <span data-ttu-id="8dbf3-191">New-AzureHDInsightHiveJobDefinition</span><span class="sxs-lookup"><span data-stu-id="8dbf3-191">New-AzureHDInsightHiveJobDefinition</span></span> |[<span data-ttu-id="8dbf3-192">Nové AzureRmHDInsightHiveJobDefinition</span><span class="sxs-lookup"><span data-stu-id="8dbf3-192">New-AzureRmHDInsightHiveJobDefinition</span></span>](https://msdn.microsoft.com/library/mt619448.aspx) |
| <span data-ttu-id="8dbf3-193">New-AzureHDInsightMapReduceJobDefinition</span><span class="sxs-lookup"><span data-stu-id="8dbf3-193">New-AzureHDInsightMapReduceJobDefinition</span></span> |[<span data-ttu-id="8dbf3-194">Nové AzureRmHDInsightMapReduceJobDefinition</span><span class="sxs-lookup"><span data-stu-id="8dbf3-194">New-AzureRmHDInsightMapReduceJobDefinition</span></span>](https://msdn.microsoft.com/library/mt603626.aspx) |
| <span data-ttu-id="8dbf3-195">New-AzureHDInsightPigJobDefinition</span><span class="sxs-lookup"><span data-stu-id="8dbf3-195">New-AzureHDInsightPigJobDefinition</span></span> |[<span data-ttu-id="8dbf3-196">Nové AzureRmHDInsightPigJobDefinition</span><span class="sxs-lookup"><span data-stu-id="8dbf3-196">New-AzureRmHDInsightPigJobDefinition</span></span>](https://msdn.microsoft.com/library/mt603671.aspx) |
| <span data-ttu-id="8dbf3-197">New-AzureHDInsightSqoopJobDefinition</span><span class="sxs-lookup"><span data-stu-id="8dbf3-197">New-AzureHDInsightSqoopJobDefinition</span></span> |[<span data-ttu-id="8dbf3-198">Nové AzureRmHDInsightSqoopJobDefinition</span><span class="sxs-lookup"><span data-stu-id="8dbf3-198">New-AzureRmHDInsightSqoopJobDefinition</span></span>](https://msdn.microsoft.com/library/mt608551.aspx) |
| <span data-ttu-id="8dbf3-199">New-AzureHDInsightStreamingMapReduceJobDefinition</span><span class="sxs-lookup"><span data-stu-id="8dbf3-199">New-AzureHDInsightStreamingMapReduceJobDefinition</span></span> |[<span data-ttu-id="8dbf3-200">Nové AzureRmHDInsightStreamingMapReduceJobDefinition</span><span class="sxs-lookup"><span data-stu-id="8dbf3-200">New-AzureRmHDInsightStreamingMapReduceJobDefinition</span></span>](https://msdn.microsoft.com/library/mt603626.aspx) |
| <span data-ttu-id="8dbf3-201">Remove-AzureHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="8dbf3-201">Remove-AzureHDInsightCluster</span></span> |[<span data-ttu-id="8dbf3-202">Odebrat AzureRmHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="8dbf3-202">Remove-AzureRmHDInsightCluster</span></span>](https://msdn.microsoft.com/library/mt619431.aspx) |
| <span data-ttu-id="8dbf3-203">Revoke-AzureHDInsightHttpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="8dbf3-203">Revoke-AzureHDInsightHttpServicesAccess</span></span> |[<span data-ttu-id="8dbf3-204">AzureRmHDInsightHttpServicesAccess odvolání.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-204">Revoke-AzureRmHDInsightHttpServicesAccess</span></span>](https://msdn.microsoft.com/library/mt619375.aspx) |
| <span data-ttu-id="8dbf3-205">Revoke-AzureHdinsightRdpAccess</span><span class="sxs-lookup"><span data-stu-id="8dbf3-205">Revoke-AzureHdinsightRdpAccess</span></span> |[<span data-ttu-id="8dbf3-206">AzureRmHDInsightRdpServicesAccess odvolání.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-206">Revoke-AzureRmHDInsightRdpServicesAccess</span></span>](https://msdn.microsoft.com/library/mt603523.aspx) |
| <span data-ttu-id="8dbf3-207">Set-AzureHDInsightClusterSize</span><span class="sxs-lookup"><span data-stu-id="8dbf3-207">Set-AzureHDInsightClusterSize</span></span> |[<span data-ttu-id="8dbf3-208">Set-AzureRmHDInsightClusterSize</span><span class="sxs-lookup"><span data-stu-id="8dbf3-208">Set-AzureRmHDInsightClusterSize</span></span>](https://msdn.microsoft.com/library/mt603513.aspx) |
| <span data-ttu-id="8dbf3-209">Set-AzureHDInsightDefaultStorage</span><span class="sxs-lookup"><span data-stu-id="8dbf3-209">Set-AzureHDInsightDefaultStorage</span></span> |[<span data-ttu-id="8dbf3-210">Set-AzureRmHDInsightDefaultStorage</span><span class="sxs-lookup"><span data-stu-id="8dbf3-210">Set-AzureRmHDInsightDefaultStorage</span></span>](https://msdn.microsoft.com/library/mt603486.aspx) |
| <span data-ttu-id="8dbf3-211">Start-AzureHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="8dbf3-211">Start-AzureHDInsightJob</span></span> |[<span data-ttu-id="8dbf3-212">Počáteční AzureRmHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="8dbf3-212">Start-AzureRmHDInsightJob</span></span>](https://msdn.microsoft.com/library/mt603798.aspx) |
| <span data-ttu-id="8dbf3-213">Stop-AzureHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="8dbf3-213">Stop-AzureHDInsightJob</span></span> |[<span data-ttu-id="8dbf3-214">Stop-AzureRmHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="8dbf3-214">Stop-AzureRmHDInsightJob</span></span>](https://msdn.microsoft.com/library/mt619424.aspx) |
| <span data-ttu-id="8dbf3-215">Use-AzureHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="8dbf3-215">Use-AzureHDInsightCluster</span></span> |[<span data-ttu-id="8dbf3-216">Použití AzureRmHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="8dbf3-216">Use-AzureRmHDInsightCluster</span></span>](https://msdn.microsoft.com/library/mt619442.aspx) |
| <span data-ttu-id="8dbf3-217">Wait-AzureHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="8dbf3-217">Wait-AzureHDInsightJob</span></span> |[<span data-ttu-id="8dbf3-218">Počkejte AzureRmHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="8dbf3-218">Wait-AzureRmHDInsightJob</span></span>](https://msdn.microsoft.com/library/mt603834.aspx) |

### <a name="new-cmdlets"></a><span data-ttu-id="8dbf3-219">Nové rutiny</span><span class="sxs-lookup"><span data-stu-id="8dbf3-219">New cmdlets</span></span>
<span data-ttu-id="8dbf3-220">Níže jsou nové rutiny, které jsou dostupné jenom v režimu ARM.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-220">The following are the new cmdlets that are only available in the ARM mode.</span></span> 

<span data-ttu-id="8dbf3-221">**Akce skriptu související rutiny:**</span><span class="sxs-lookup"><span data-stu-id="8dbf3-221">**Script action related cmdlets:**</span></span>

* <span data-ttu-id="8dbf3-222">**Get-AzureRmHDInsightPersistedScriptAction**: získá akcí trvalého skriptu pro cluster a zobrazí je v chronologickém pořadí nebo získá podrobnosti pro akci zadaný trvalého skriptu.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-222">**Get-AzureRmHDInsightPersistedScriptAction**: Gets the persisted script actions for a cluster and lists them in chronological order, or gets details for a specified persisted script action.</span></span> 
* <span data-ttu-id="8dbf3-223">**Get-AzureRmHDInsightScriptActionHistory**: získá historii akcí skriptu pro cluster a seznamů v obráceném chronologickém pořadí nebo získá podrobnosti akce dříve spuštění skriptu.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-223">**Get-AzureRmHDInsightScriptActionHistory**: Gets the script action history for a cluster and lists it in reverse chronological order, or gets details of a previously executed script action.</span></span> 
* <span data-ttu-id="8dbf3-224">**Odebrat AzureRmHDInsightPersistedScriptAction**: Odebere akcí trvalého skriptu z clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-224">**Remove-AzureRmHDInsightPersistedScriptAction**: Removes a persisted script action from an HDInsight cluster.</span></span>
* <span data-ttu-id="8dbf3-225">**Set-AzureRmHDInsightPersistedScriptAction**: Nastaví akce dříve spuštění skriptu jako akcí trvalého skriptu.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-225">**Set-AzureRmHDInsightPersistedScriptAction**: Sets a previously executed script action to be a persisted script action.</span></span>
* <span data-ttu-id="8dbf3-226">**Odeslání AzureRmHDInsightScriptAction**: Odešle nové akce skriptu do clusteru Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-226">**Submit-AzureRmHDInsightScriptAction**: Submits a new script action to an Azure HDInsight cluster.</span></span> 

<span data-ttu-id="8dbf3-227">Využití Další informace najdete v tématu [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="8dbf3-227">For additional usage information, see [Customize Linux-based HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

<span data-ttu-id="8dbf3-228">**Clsuter identity související rutiny:**</span><span class="sxs-lookup"><span data-stu-id="8dbf3-228">**Clsuter identity related cmdlets:**</span></span>

* <span data-ttu-id="8dbf3-229">**Přidat AzureRmHDInsightClusterIdentity**: Přidá identitu clusteru do objekt konfigurace clusteru tak, aby clusteru HDInsight můžete přístup k Azure Data Lake úložiště.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-229">**Add-AzureRmHDInsightClusterIdentity**: Adds a cluster identity to a cluster configuration object so that the HDInsight cluster can access Azure Data Lake Stores.</span></span> <span data-ttu-id="8dbf3-230">V tématu [vytvoření clusteru HDInsight s Data Lake Store pomocí Azure PowerShell](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="8dbf3-230">See [Create an HDInsight cluster with Data Lake Store using Azure PowerShell](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).</span></span>

### <a name="examples"></a><span data-ttu-id="8dbf3-231">Příklady</span><span class="sxs-lookup"><span data-stu-id="8dbf3-231">Examples</span></span>
<span data-ttu-id="8dbf3-232">**Vytvoření clusteru**</span><span class="sxs-lookup"><span data-stu-id="8dbf3-232">**Create cluster**</span></span>

<span data-ttu-id="8dbf3-233">Příkaz staré (ASM):</span><span class="sxs-lookup"><span data-stu-id="8dbf3-233">Old command (ASM):</span></span> 

    New-AzureHDInsightCluster `
        -Name $clusterName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainerName $containerName `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -Credential $httpCredential `
        -SshCredential $sshCredential

<span data-ttu-id="8dbf3-234">Nový příkaz (ARM):</span><span class="sxs-lookup"><span data-stu-id="8dbf3-234">New command (ARM):</span></span>

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainer $containerName  `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -HttpCredential $httpcredentials `
        -SshCredential $sshCredentials


<span data-ttu-id="8dbf3-235">**Odstranění clusteru**</span><span class="sxs-lookup"><span data-stu-id="8dbf3-235">**Delete cluster**</span></span>

<span data-ttu-id="8dbf3-236">Příkaz staré (ASM):</span><span class="sxs-lookup"><span data-stu-id="8dbf3-236">Old command (ASM):</span></span>

    Remove-AzureHDInsightCluster -name $clusterName 

<span data-ttu-id="8dbf3-237">Nový příkaz (ARM):</span><span class="sxs-lookup"><span data-stu-id="8dbf3-237">New command (ARM):</span></span>

    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName 

<span data-ttu-id="8dbf3-238">**Seznam clusteru**</span><span class="sxs-lookup"><span data-stu-id="8dbf3-238">**List cluster**</span></span>

<span data-ttu-id="8dbf3-239">Příkaz staré (ASM):</span><span class="sxs-lookup"><span data-stu-id="8dbf3-239">Old command (ASM):</span></span>

    Get-AzureHDInsightCluster

<span data-ttu-id="8dbf3-240">Nový příkaz (ARM):</span><span class="sxs-lookup"><span data-stu-id="8dbf3-240">New command (ARM):</span></span>

    Get-AzureRmHDInsightCluster 

<span data-ttu-id="8dbf3-241">**Zobrazit clusteru**</span><span class="sxs-lookup"><span data-stu-id="8dbf3-241">**Show cluster**</span></span>

<span data-ttu-id="8dbf3-242">Příkaz staré (ASM):</span><span class="sxs-lookup"><span data-stu-id="8dbf3-242">Old command (ASM):</span></span>

    Get-AzureHDInsightCluster -Name $clusterName

<span data-ttu-id="8dbf3-243">Nový příkaz (ARM):</span><span class="sxs-lookup"><span data-stu-id="8dbf3-243">New command (ARM):</span></span>

    Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -clusterName $clusterName


#### <a name="other-samples"></a><span data-ttu-id="8dbf3-244">Další ukázky</span><span class="sxs-lookup"><span data-stu-id="8dbf3-244">Other samples</span></span>
* [<span data-ttu-id="8dbf3-245">Vytvoření clusterů HDInsight</span><span class="sxs-lookup"><span data-stu-id="8dbf3-245">Create HDInsight clusters</span></span>](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [<span data-ttu-id="8dbf3-246">Odeslání úloh Hive</span><span class="sxs-lookup"><span data-stu-id="8dbf3-246">Submit Hive jobs</span></span>](hdinsight-hadoop-use-hive-powershell.md)
* [<span data-ttu-id="8dbf3-247">Odeslání úlohy Pig</span><span class="sxs-lookup"><span data-stu-id="8dbf3-247">Submit Pig jobs</span></span>](hdinsight-hadoop-use-pig-powershell.md)
* [<span data-ttu-id="8dbf3-248">Odesílání úloh Sqoop</span><span class="sxs-lookup"><span data-stu-id="8dbf3-248">Submit Sqoop jobs</span></span>](hdinsight-hadoop-use-sqoop-powershell.md)

## <a name="migrating-to-the-arm-based-hdinsight-net-sdk"></a><span data-ttu-id="8dbf3-249">Migrace na bázi ARM HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="8dbf3-249">Migrating to the ARM-based HDInsight .NET SDK</span></span>
<span data-ttu-id="8dbf3-250">Azure Service Management základě [(ASM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt416619.aspx) je nyní zastaralý.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-250">The Azure Service Management-based [(ASM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt416619.aspx) is now deprecated.</span></span> <span data-ttu-id="8dbf3-251">Doporučujeme používat správu prostředků Azure prostřednictvím [(ARM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt271028.aspx).</span><span class="sxs-lookup"><span data-stu-id="8dbf3-251">You are encouraged to use the Azure Resource Management-based [(ARM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt271028.aspx).</span></span> <span data-ttu-id="8dbf3-252">Následující balíčky HDInsight se systémem ASM jsou zastaralá.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-252">The following ASM-based HDInsight packages are being deprecated.</span></span>

* `Microsoft.WindowsAzure.Management.HDInsight`
* `Microsoft.Hadoop.Client`

<span data-ttu-id="8dbf3-253">Tato část obsahuje odkazy na další informace o tom, jak provádět určité úlohy pomocí sady SDK založené na ARM.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-253">This section provides pointers to more information on how to perform certain tasks using the ARM-based SDK.</span></span>

| <span data-ttu-id="8dbf3-254">Postup... pomocí sady SDK HDInsight založené na ARM</span><span class="sxs-lookup"><span data-stu-id="8dbf3-254">How to... using the ARM-based HDInsight SDK</span></span> | <span data-ttu-id="8dbf3-255">Odkazy</span><span class="sxs-lookup"><span data-stu-id="8dbf3-255">Links</span></span> |
| --- | --- |
| <span data-ttu-id="8dbf3-256">Vytvoření clusterů HDInsight pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="8dbf3-256">Create HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="8dbf3-257">V tématu [Tvorba clusterů HDInsight pomocí sady .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="8dbf3-257">See [Create HDInsight clusters using .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span></span> |
| <span data-ttu-id="8dbf3-258">Přizpůsobení clusteru pomocí akce skriptu pomocí .NET SDK</span><span class="sxs-lookup"><span data-stu-id="8dbf3-258">Customize a cluster using Script Action with .NET SDK</span></span> |<span data-ttu-id="8dbf3-259">V tématu [HDInsight Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action)</span><span class="sxs-lookup"><span data-stu-id="8dbf3-259">See [Customize HDInsight Linux clusters using Script Action](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action)</span></span> |
| <span data-ttu-id="8dbf3-260">Ověření aplikací interaktivně pomocí .NET SDK služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8dbf3-260">Authenticate applications interactively using Azure Active Directory with .NET SDK</span></span> |<span data-ttu-id="8dbf3-261">V tématu [spouštění dotazů Hive pomocí sady .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="8dbf3-261">See [Run Hive queries using .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span></span> <span data-ttu-id="8dbf3-262">Fragment kódu v tomto článku používá interaktivní ověřování přístup.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-262">The code snippet in this article uses the interactive authentication approach.</span></span> |
| <span data-ttu-id="8dbf3-263">Ověření aplikací interaktivně pomocí .NET SDK služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8dbf3-263">Authenticate applications non-interactively using Azure Active Directory with .NET SDK</span></span> |<span data-ttu-id="8dbf3-264">V tématu [vytvořit neinteraktivní aplikace pro HDInsight](hdinsight-create-non-interactive-authentication-dotnet-applications.md)</span><span class="sxs-lookup"><span data-stu-id="8dbf3-264">See [Create non-interactive applications for HDInsight](hdinsight-create-non-interactive-authentication-dotnet-applications.md)</span></span> |
| <span data-ttu-id="8dbf3-265">Odeslání úlohy Hive pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="8dbf3-265">Submit a Hive job using .NET SDK</span></span> |<span data-ttu-id="8dbf3-266">V tématu [úlohy odeslání Hive](hdinsight-hadoop-use-hive-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="8dbf3-266">See [Submit Hive jobs](hdinsight-hadoop-use-hive-dotnet-sdk.md)</span></span> |
| <span data-ttu-id="8dbf3-267">Odeslat úlohu Pig pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="8dbf3-267">Submit a Pig job using .NET SDK</span></span> |<span data-ttu-id="8dbf3-268">V tématu [úlohy odeslání Pig](hdinsight-hadoop-use-pig-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="8dbf3-268">See [Submit Pig jobs](hdinsight-hadoop-use-pig-dotnet-sdk.md)</span></span> |
| <span data-ttu-id="8dbf3-269">Odeslání úlohy Sqoop pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="8dbf3-269">Submit a Sqoop job using .NET SDK</span></span> |<span data-ttu-id="8dbf3-270">V tématu [Sqoop odeslání úlohy](hdinsight-hadoop-use-sqoop-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="8dbf3-270">See [Submit Sqoop jobs](hdinsight-hadoop-use-sqoop-dotnet-sdk.md)</span></span> |
| <span data-ttu-id="8dbf3-271">Seznam clusterů HDInsight pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="8dbf3-271">List HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="8dbf3-272">V tématu [clusterů HDInsight seznamu](hdinsight-administer-use-dotnet-sdk.md#list-clusters)</span><span class="sxs-lookup"><span data-stu-id="8dbf3-272">See [List HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#list-clusters)</span></span> |
| <span data-ttu-id="8dbf3-273">Škálování clusterů HDInsight pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="8dbf3-273">Scale HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="8dbf3-274">V tématu [clusterů HDInsight škálování](hdinsight-administer-use-dotnet-sdk.md#scale-clusters)</span><span class="sxs-lookup"><span data-stu-id="8dbf3-274">See [Scale HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#scale-clusters)</span></span> |
| <span data-ttu-id="8dbf3-275">Udělení nebo odvolání přístupu ke clusterům HDInsight pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="8dbf3-275">Grant/revoke access to HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="8dbf3-276">V tématu [udělení nebo odvolání přístupu ke clusterům HDInsight](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access)</span><span class="sxs-lookup"><span data-stu-id="8dbf3-276">See [Grant/revoke access to HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access)</span></span> |
| <span data-ttu-id="8dbf3-277">Aktualizovat pověření uživatele HTTP pro clustery služby HDInsight pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="8dbf3-277">Update HTTP user credentials for HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="8dbf3-278">V tématu [přihlašovací údaje uživatele HTTP aktualizace pro clustery služby HDInsight](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials)</span><span class="sxs-lookup"><span data-stu-id="8dbf3-278">See [Update HTTP user credentials for HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials)</span></span> |
| <span data-ttu-id="8dbf3-279">Najít výchozí účet úložiště pro clustery služby HDInsight pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="8dbf3-279">Find the default storage account for HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="8dbf3-280">V tématu [najít výchozí účet úložiště pro clustery HDInsight](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account)</span><span class="sxs-lookup"><span data-stu-id="8dbf3-280">See [Find the default storage account for HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account)</span></span> |
| <span data-ttu-id="8dbf3-281">Odstranění clusterů HDInsight pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="8dbf3-281">Delete HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="8dbf3-282">V tématu [clusterů HDInsight odstranit](hdinsight-administer-use-dotnet-sdk.md#delete-clusters)</span><span class="sxs-lookup"><span data-stu-id="8dbf3-282">See [Delete HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#delete-clusters)</span></span> |

### <a name="examples"></a><span data-ttu-id="8dbf3-283">Příklady</span><span class="sxs-lookup"><span data-stu-id="8dbf3-283">Examples</span></span>
<span data-ttu-id="8dbf3-284">Tady jsou některé příklady na to, jak je operace provést pomocí sady SDK na základě ASM a fragmentu kódu ekvivalentní pro sadu SDK založené na ARM.</span><span class="sxs-lookup"><span data-stu-id="8dbf3-284">Following are some examples on how an operation is performed using the ASM-based SDK and the equivalent code snippet for the ARM-based SDK.</span></span>

<span data-ttu-id="8dbf3-285">**Vytvoření klienta CRUD clusteru**</span><span class="sxs-lookup"><span data-stu-id="8dbf3-285">**Creating a cluster CRUD client**</span></span>

* <span data-ttu-id="8dbf3-286">Příkaz staré (ASM)</span><span class="sxs-lookup"><span data-stu-id="8dbf3-286">Old command (ASM)</span></span>
  
        //Certificate auth
        //This logs the application in using a subscription administration certificate, which is not offered in Azure Resource Manager (ARM)
  
        const string subid = "454467d4-60ca-4dfd-a556-216eeeeeeee1";
        var cred = new HDInsightCertificateCredential(new Guid(subid), new X509Certificate2(@"path\to\certificate.cer"));
        var client = HDInsightClient.Connect(cred);
* <span data-ttu-id="8dbf3-287">Nový příkaz (ARM) (hlavní autorizace Service)</span><span class="sxs-lookup"><span data-stu-id="8dbf3-287">New command (ARM) (Service principal authorization)</span></span>
  
        //Service principal auth
        //This will log the application in as itself, rather than on behalf of a specific user.
        //For details, including how to set up the application, see:
        //   https://azure.microsoft.com/en-us/documentation/articles/hdinsight-create-non-interactive-authentication-dotnet-applications/
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);
* <span data-ttu-id="8dbf3-288">Nový příkaz (ARM) (autorizace uživatelů)</span><span class="sxs-lookup"><span data-stu-id="8dbf3-288">New command (ARM) (User authorization)</span></span>
  
        //User auth
        //This will log the application in on behalf of the user.
        //The end-user will see a login popup.
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.User, Id = username };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, AuthenticationFactory.CommonAdTenant, password, ShowDialog.Auto).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);

<span data-ttu-id="8dbf3-289">**Vytvoření clusteru**</span><span class="sxs-lookup"><span data-stu-id="8dbf3-289">**Creating a cluster**</span></span>

* <span data-ttu-id="8dbf3-290">Příkaz staré (ASM)</span><span class="sxs-lookup"><span data-stu-id="8dbf3-290">Old command (ASM)</span></span>
  
        var clusterInfo = new ClusterCreateParameters
                    {
                        Name = dnsName,
                        DefaultStorageAccountKey = key,
                        DefaultStorageContainer = defaultStorageContainer,
                        DefaultStorageAccountName = storageAccountDnsName,
                        ClusterSizeInNodes = 1,
                        ClusterType = type,
                        Location = "West US",
                        UserName = "admin",
                        Password = "*******",
                        Version = version,
                        HeadNodeSize = NodeVMSize.Large,
                    };
        clusterInfo.CoreConfiguration.Add(new KeyValuePair<string, string>("config1", "value1"));
        client.CreateCluster(clusterInfo);
* <span data-ttu-id="8dbf3-291">Nový příkaz (ARM)</span><span class="sxs-lookup"><span data-stu-id="8dbf3-291">New command (ARM)</span></span>
  
        var clusterCreateParameters = new ClusterCreateParameters
            {
                Location = "West US",
                ClusterType = "Hadoop",
                Version = "3.1",
                OSType = OSType.Linux,
                DefaultStorageAccountName = "mystorage.blob.core.windows.net",
                DefaultStorageAccountKey =
                    "O9EQvp3A3AjXq/W27rst1GQfLllhp0gUeiUUn2D8zX2lU3taiXSSfqkZlcPv+nQcYUxYw==",
                UserName = "hadoopuser",
                Password = "*******",
                HeadNodeSize = "ExtraLarge",
                RdpUsername = "hdirp",
                RdpPassword = ""*******",
                RdpAccessExpiry = new DateTime(2025, 3, 1),
                ClusterSizeInNodes = 5
            };
        var coreConfigs = new Dictionary<string, string> {{"config1", "value1"}};
        clusterCreateParameters.Configurations.Add(ConfigurationKey.CoreSite, coreConfigs);

<span data-ttu-id="8dbf3-292">**Povolíte přístup protokolu HTTP**</span><span class="sxs-lookup"><span data-stu-id="8dbf3-292">**Enabling HTTP access**</span></span>

* <span data-ttu-id="8dbf3-293">Příkaz staré (ASM)</span><span class="sxs-lookup"><span data-stu-id="8dbf3-293">Old command (ASM)</span></span>
  
        client.EnableHttp(dnsName, "West US", "admin", "*******");
* <span data-ttu-id="8dbf3-294">Nový příkaz (ARM)</span><span class="sxs-lookup"><span data-stu-id="8dbf3-294">New command (ARM)</span></span>
  
        var httpParams = new HttpSettingsParameters
        {
               HttpUserEnabled = true,
               HttpUsername = "admin",
               HttpPassword = "*******",
        };
        client.Clusters.ConfigureHttpSettings(resourceGroup, dnsname, httpParams);

<span data-ttu-id="8dbf3-295">**Odstranění clusteru**</span><span class="sxs-lookup"><span data-stu-id="8dbf3-295">**Deleting a cluster**</span></span>

* <span data-ttu-id="8dbf3-296">Příkaz staré (ASM)</span><span class="sxs-lookup"><span data-stu-id="8dbf3-296">Old command (ASM)</span></span>
  
        client.DeleteCluster(dnsName);
* <span data-ttu-id="8dbf3-297">Nový příkaz (ARM)</span><span class="sxs-lookup"><span data-stu-id="8dbf3-297">New command (ARM)</span></span>
  
        client.Clusters.Delete(resourceGroup, dnsname);

