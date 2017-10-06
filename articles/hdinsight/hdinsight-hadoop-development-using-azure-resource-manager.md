---
title: "aaaMigrate tooAzure Resource Manager nástroje pro HDInsight | Microsoft Docs"
description: "Jak nástroje toomigrate tooAzure vývoj Resource Manageru pro clustery HDInsight"
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
ms.openlocfilehash: c087ae63d2544e5badae6be9c258f783aa92e2ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrating-tooazure-resource-manager-based-development-tools-for-hdinsight-clusters"></a><span data-ttu-id="b3805-103">Migrace tooAzure Resource Manager vývojové nástroje založené pro clustery služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="b3805-103">Migrating tooAzure Resource Manager-based development tools for HDInsight clusters</span></span>

<span data-ttu-id="b3805-104">HDInsight je místo na základě Azure Service Manager ASM nástroje začne pro HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b3805-104">HDInsight is deprecating Azure Service Manager (ASM)-based tools for HDInsight.</span></span> <span data-ttu-id="b3805-105">Pokud používáte prostředí Azure PowerShell, rozhraní příkazového řádku Azure nebo hello SDK rozhraní .NET HDInsight toowork s clustery HDInsight, jste podporovali toouse hello založené na Azure Resource Manager ARM verze prostředí PowerShell, rozhraní příkazového řádku a .NET SDK do budoucna.</span><span class="sxs-lookup"><span data-stu-id="b3805-105">If you have been using Azure PowerShell, Azure CLI, or hello HDInsight .NET SDK toowork with HDInsight clusters, you are encouraged toouse hello Azure Resource Manager (ARM)-based versions of PowerShell, CLI, and .NET SDK going forward.</span></span> <span data-ttu-id="b3805-106">Tento článek obsahuje ukazatele toomigrate toohello nový založené na ARM přístup.</span><span class="sxs-lookup"><span data-stu-id="b3805-106">This article provides pointers on how toomigrate toohello new ARM-based approach.</span></span> <span data-ttu-id="b3805-107">Pokud je to možné, v tomto článku také upozornění na hello rozdíly mezi hello ASM a ARM přístupy pro HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b3805-107">Wherever applicable, this article also points out hello differences between hello ASM and ARM approaches for HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b3805-108">Podpora Hello ASM na základě prostředí PowerShell, rozhraní příkazového řádku, a .NET SDK se ukončí na **1. ledna 2017**.</span><span class="sxs-lookup"><span data-stu-id="b3805-108">hello support for ASM based PowerShell, CLI, and .NET SDK will discontinue on **January 1, 2017**.</span></span>
> 
> 

## <a name="migrating-azure-cli-tooazure-resource-manager"></a><span data-ttu-id="b3805-109">Migrace tooAzure rozhraní příkazového řádku Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b3805-109">Migrating Azure CLI tooAzure Resource Manager</span></span>
<span data-ttu-id="b3805-110">Hello příkazového řádku Azure CLI nyní výchozí tooAzure režimu Resource Manager (ARM), pokud provádíte upgrade z předchozí instalace; v takovém případě musíte toouse hello `azure config mode arm` příkaz tooswitch tooARM režimu.</span><span class="sxs-lookup"><span data-stu-id="b3805-110">hello Azure CLI now defaults tooAzure Resource Manager (ARM) mode, unless you are upgrading from a previous installation; in this case, you may need toouse hello `azure config mode arm` command tooswitch tooARM mode.</span></span>

<span data-ttu-id="b3805-111">základní příkazy Hello této hello rozhraní příkazového řádku Azure poskytuje toowork s HDInsight pomocí Azure Service Management (ASM) jsou hello stejné při použití ARM; ale některé parametry a přepínače může mít nové názvy a existuje mnoho nových parametrů při použití ARM.</span><span class="sxs-lookup"><span data-stu-id="b3805-111">hello basic commands that hello Azure CLI provided toowork with HDInsight using Azure Service Management (ASM) are hello same when using ARM; however some parameters and switches may have new names, and there are many new parameters available when using ARM.</span></span> <span data-ttu-id="b3805-112">Například můžete nyní použít `azure hdinsight cluster create` toospecify hello virtuální sítě Azure, který by měl být vytvořen v clusteru, nebo Hive a informace metaúložiště Oozie.</span><span class="sxs-lookup"><span data-stu-id="b3805-112">For example, you can now use `azure hdinsight cluster create` toospecify hello Azure Virtual Network that a cluster should be created in, or Hive and Oozie metastore information.</span></span>

<span data-ttu-id="b3805-113">Základní příkazy pro práci s HDInsight prostřednictvím Správce Azure Resource Manager jsou:</span><span class="sxs-lookup"><span data-stu-id="b3805-113">Basic commands for working with HDInsight through Azure Resource Manager are:</span></span>

* <span data-ttu-id="b3805-114">`azure hdinsight cluster create`-Vytvoří nový cluster HDInsight</span><span class="sxs-lookup"><span data-stu-id="b3805-114">`azure hdinsight cluster create` - creates a new HDInsight cluster</span></span>
* <span data-ttu-id="b3805-115">`azure hdinsight cluster delete`-Odstraní stávající cluster HDInsight</span><span class="sxs-lookup"><span data-stu-id="b3805-115">`azure hdinsight cluster delete` - deletes an existing HDInsight cluster</span></span>
* <span data-ttu-id="b3805-116">`azure hdinsight cluster show`– Zobrazí informace o stávajícího clusteru</span><span class="sxs-lookup"><span data-stu-id="b3805-116">`azure hdinsight cluster show` - display information about an existing cluster</span></span>
* <span data-ttu-id="b3805-117">`azure hdinsight cluster list`-obsahuje seznam clustery HDInsight pro vaše předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="b3805-117">`azure hdinsight cluster list` - lists HDInsight clusters for your Azure subscription</span></span>

<span data-ttu-id="b3805-118">Použití hello `-h` přepínač tooinspect hello parametry a přepínače, které jsou k dispozici u každého příkazu.</span><span class="sxs-lookup"><span data-stu-id="b3805-118">Use hello `-h` switch tooinspect hello parameters and switches available for each command.</span></span>

### <a name="new-commands"></a><span data-ttu-id="b3805-119">Nové příkazy</span><span class="sxs-lookup"><span data-stu-id="b3805-119">New commands</span></span>
<span data-ttu-id="b3805-120">Nové příkazy, které jsou k dispozici s Azure Resource Manager jsou:</span><span class="sxs-lookup"><span data-stu-id="b3805-120">New commands available with Azure Resource Manager are:</span></span>

* <span data-ttu-id="b3805-121">`azure hdinsight cluster resize`-dynamicky změny hello počet uzlů pracovního procesu v clusteru hello</span><span class="sxs-lookup"><span data-stu-id="b3805-121">`azure hdinsight cluster resize` - dynamically changes hello number of worker nodes in hello cluster</span></span>
* <span data-ttu-id="b3805-122">`azure hdinsight cluster enable-http-access`– umožňuje clusteru toohello přístup HTTPs (na ve výchozím nastavení)</span><span class="sxs-lookup"><span data-stu-id="b3805-122">`azure hdinsight cluster enable-http-access` - enables HTTPs access toohello cluster (on by default)</span></span>
* <span data-ttu-id="b3805-123">`azure hdinsight cluster disable-http-access`– Zakáže clusteru toohello přístup HTTPs</span><span class="sxs-lookup"><span data-stu-id="b3805-123">`azure hdinsight cluster disable-http-access` - disables HTTPs access toohello cluster</span></span>
* <span data-ttu-id="b3805-124">`azure hdinsight script-action`-obsahuje příkazy pro vytváření nebo správu akcí skriptů v clusteru</span><span class="sxs-lookup"><span data-stu-id="b3805-124">`azure hdinsight script-action` - provides commands for creating/managing Script Actions on a cluster</span></span>
* <span data-ttu-id="b3805-125">`azure hdinsight config`-poskytuje příkazů pro vytvoření konfiguračního souboru, který lze použít s hello `hdinsight cluster create` příkaz tooprovide informace o konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="b3805-125">`azure hdinsight config` - provides commands for creating a configuration file that can be used with hello `hdinsight cluster create` command tooprovide configuration information.</span></span>

### <a name="deprecated-commands"></a><span data-ttu-id="b3805-126">Nepoužívané příkazy</span><span class="sxs-lookup"><span data-stu-id="b3805-126">Deprecated commands</span></span>
<span data-ttu-id="b3805-127">Pokud používáte hello `azure hdinsight job` příkazy toosubmit úlohy tooyour HDInsight clusteru, tyto nejsou k dispozici prostřednictvím hello příkazy ARM.</span><span class="sxs-lookup"><span data-stu-id="b3805-127">If you use hello `azure hdinsight job` commands toosubmit jobs tooyour HDInsight cluster, these are not available through hello ARM commands.</span></span> <span data-ttu-id="b3805-128">Pokud potřebujete tooprogrammatically odeslání úlohy tooHDInsight z skriptů, měli byste místo toho použít hello rozhraní REST API poskytovaných v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b3805-128">If you need tooprogrammatically submit jobs tooHDInsight from scripts, you should instead use hello REST APIs provided by HDInsight.</span></span> <span data-ttu-id="b3805-129">Další informace o odesílání úloh pomocí rozhraní REST API najdete v tématu hello následující dokumenty.</span><span class="sxs-lookup"><span data-stu-id="b3805-129">For more information on submitting jobs using REST APIs, see hello following documents.</span></span>

* [<span data-ttu-id="b3805-130">Spuštění úloh MapReduce s Hadoop v HDInsight pomocí cURL</span><span class="sxs-lookup"><span data-stu-id="b3805-130">Run MapReduce jobs with Hadoop on HDInsight using cURL</span></span>](hdinsight-hadoop-use-mapreduce-curl.md)
* [<span data-ttu-id="b3805-131">Spouštění dotazů Hive se systémem Hadoop v HDInsight pomocí cURL</span><span class="sxs-lookup"><span data-stu-id="b3805-131">Run Hive queries with Hadoop on HDInsight using cURL</span></span>](hdinsight-hadoop-use-hive-curl.md)
* [<span data-ttu-id="b3805-132">Spuštění úlohy Pig s Hadoop v HDInsight pomocí cURL</span><span class="sxs-lookup"><span data-stu-id="b3805-132">Run Pig jobs with Hadoop on HDInsight using cURL</span></span>](hdinsight-hadoop-use-pig-curl.md)

<span data-ttu-id="b3805-133">Informace o jiných způsobech toorun MapReduce, Hive a vepřových interaktivně, najdete v části [používání MapReduce s Hadoop v HDInsight](hdinsight-use-mapreduce.md), [používání Hive s Hadoop v HDInsight](hdinsight-use-hive.md), a [použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md).</span><span class="sxs-lookup"><span data-stu-id="b3805-133">For information on other ways toorun MapReduce, Hive, and Pig interactively, see [Use MapReduce with Hadoop on HDInsight](hdinsight-use-mapreduce.md), [Use Hive with Hadoop on HDInsight](hdinsight-use-hive.md), and [Use Pig with Hadoop on HDInsight](hdinsight-use-pig.md).</span></span>

### <a name="examples"></a><span data-ttu-id="b3805-134">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3805-134">Examples</span></span>
<span data-ttu-id="b3805-135">**Vytvoření clusteru**</span><span class="sxs-lookup"><span data-stu-id="b3805-135">**Creating a cluster**</span></span>

* <span data-ttu-id="b3805-136">Příkaz staré (ASM)-`azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`</span><span class="sxs-lookup"><span data-stu-id="b3805-136">Old command (ASM) - `azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`</span></span>
* <span data-ttu-id="b3805-137">Nový příkaz (ARM)-`azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`</span><span class="sxs-lookup"><span data-stu-id="b3805-137">New command (ARM) - `azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`</span></span>

<span data-ttu-id="b3805-138">**Odstranění clusteru**</span><span class="sxs-lookup"><span data-stu-id="b3805-138">**Deleting a cluster**</span></span>

* <span data-ttu-id="b3805-139">Příkaz staré (ASM)-`azure hdinsight cluster delete myhdicluster`</span><span class="sxs-lookup"><span data-stu-id="b3805-139">Old command (ASM) - `azure hdinsight cluster delete myhdicluster`</span></span>
* <span data-ttu-id="b3805-140">Nový příkaz (ARM)-`azure hdinsight cluster delete mycluster -g myresourcegroup`</span><span class="sxs-lookup"><span data-stu-id="b3805-140">New command (ARM) - `azure hdinsight cluster delete mycluster -g myresourcegroup`</span></span>

<span data-ttu-id="b3805-141">**Seznam clustery**</span><span class="sxs-lookup"><span data-stu-id="b3805-141">**List clusters**</span></span>

* <span data-ttu-id="b3805-142">Příkaz staré (ASM)-`azure hdinsight cluster list`</span><span class="sxs-lookup"><span data-stu-id="b3805-142">Old command (ASM) - `azure hdinsight cluster list`</span></span>
* <span data-ttu-id="b3805-143">Nový příkaz (ARM)-`azure hdinsight cluster list`</span><span class="sxs-lookup"><span data-stu-id="b3805-143">New command (ARM) - `azure hdinsight cluster list`</span></span>

> [!NOTE]
> <span data-ttu-id="b3805-144">Pro příkaz seznamu hello, zadání hello skupinu prostředků pomocí `-g` vrátí pouze clustery hello v hello zadaná skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="b3805-144">For hello list command, specifying hello resource group using `-g` will return only hello clusters in hello specified resource group.</span></span>
> 
> 

<span data-ttu-id="b3805-145">**Zobrazit informace o clusteru**</span><span class="sxs-lookup"><span data-stu-id="b3805-145">**Show cluster information**</span></span>

* <span data-ttu-id="b3805-146">Příkaz staré (ASM)-`azure hdinsight cluster show myhdicluster`</span><span class="sxs-lookup"><span data-stu-id="b3805-146">Old command (ASM) - `azure hdinsight cluster show myhdicluster`</span></span>
* <span data-ttu-id="b3805-147">Nový příkaz (ARM)-`azure hdinsight cluster show myhdicluster -g myresourcegroup`</span><span class="sxs-lookup"><span data-stu-id="b3805-147">New command (ARM) - `azure hdinsight cluster show myhdicluster -g myresourcegroup`</span></span>

## <a name="migrating-azure-powershell-tooazure-resource-manager"></a><span data-ttu-id="b3805-148">Migrace tooAzure prostředí PowerShell Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b3805-148">Migrating Azure PowerShell tooAzure Resource Manager</span></span>
<span data-ttu-id="b3805-149">Hello obecné informace o prostředí Azure PowerShell v režimu Azure Resource Manager (ARM) hello lze najít na [použití Azure Powershellu s Azure Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="b3805-149">hello general information about Azure PowerShell in hello Azure Resource Manager (ARM) mode can be found at [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="b3805-150">Hello rutiny Azure PowerShell ARM může být nainstalovaná-souběžného s hello ASM rutiny.</span><span class="sxs-lookup"><span data-stu-id="b3805-150">hello Azure PowerShell ARM cmdlets can be installed side-by-side with hello ASM cmdlets.</span></span> <span data-ttu-id="b3805-151">rutiny Hello ze dvou režimů hello dají rozlišovat podle jejich názvy.</span><span class="sxs-lookup"><span data-stu-id="b3805-151">hello cmdlets from hello two modes can be distinguished by their names.</span></span>  <span data-ttu-id="b3805-152">má režimu ARM Hello *AzureRmHDInsight* v názvech rutiny hello porovnávání příliš*AzureHDInsight* v režimu ASM hello.</span><span class="sxs-lookup"><span data-stu-id="b3805-152">hello ARM mode has *AzureRmHDInsight* in hello cmdlet names comparing too*AzureHDInsight* in hello ASM mode.</span></span>  <span data-ttu-id="b3805-153">Například *New-AzureRmHDInsightCluster* vs. *Nové AzureHDInsightCluster*.</span><span class="sxs-lookup"><span data-stu-id="b3805-153">For example, *New-AzureRmHDInsightCluster* vs. *New-AzureHDInsightCluster*.</span></span> <span data-ttu-id="b3805-154">Názvy zprávy mohou mít parametry a přepínače, existuje mnoho nových parametrů k dispozici při použití a ARM.</span><span class="sxs-lookup"><span data-stu-id="b3805-154">Parameters and switches may have news names, and there are many new parameters available when using ARM.</span></span>  <span data-ttu-id="b3805-155">Například několik rutin vyžadovat nového přepínače názvem *- ResourceGroupName*.</span><span class="sxs-lookup"><span data-stu-id="b3805-155">For example, several cmdlets require a new switch called *-ResourceGroupName*.</span></span> 

<span data-ttu-id="b3805-156">Před použitím rutin HDInsight hello, musíte připojit tooyour účet Azure a vytvořit novou skupinu prostředků:</span><span class="sxs-lookup"><span data-stu-id="b3805-156">Before you can use hello HDInsight cmdlets, you must connect tooyour Azure account, and create a new resource group:</span></span>

* <span data-ttu-id="b3805-157">Login-AzureRmAccount nebo [vyberte AzureRmProfile](https://msdn.microsoft.com/library/mt619310.aspx).</span><span class="sxs-lookup"><span data-stu-id="b3805-157">Login-AzureRmAccount or [Select-AzureRmProfile](https://msdn.microsoft.com/library/mt619310.aspx).</span></span> <span data-ttu-id="b3805-158">V tématu [ověřování hlavní název služby pomocí Azure Resource Manageru](../azure-resource-manager/resource-group-authenticate-service-principal.md)</span><span class="sxs-lookup"><span data-stu-id="b3805-158">See [Authenticating a service principal with Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md)</span></span>
* [<span data-ttu-id="b3805-159">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="b3805-159">New-AzureRmResourceGroup</span></span>](https://msdn.microsoft.com/library/mt603739.aspx)

### <a name="renamed-cmdlets"></a><span data-ttu-id="b3805-160">Přejmenovat rutiny</span><span class="sxs-lookup"><span data-stu-id="b3805-160">Renamed cmdlets</span></span>
<span data-ttu-id="b3805-161">toolist hello HDInsight ASM rutiny v konzole Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="b3805-161">toolist hello HDInsight ASM cmdlets in Windows PowerShell console:</span></span>

    help *azurermhdinsight*

<span data-ttu-id="b3805-162">Hello následující tabulka uvádí hello ASM rutiny a jejich názvy v režimu ARM hello:</span><span class="sxs-lookup"><span data-stu-id="b3805-162">hello following table lists hello ASM cmdlets and their names in hello ARM mode:</span></span>

| <span data-ttu-id="b3805-163">Rutiny ASM</span><span class="sxs-lookup"><span data-stu-id="b3805-163">ASM cmdlets</span></span> | <span data-ttu-id="b3805-164">Rutiny ARM</span><span class="sxs-lookup"><span data-stu-id="b3805-164">ARM cmdlets</span></span> |
| --- | --- |
| <span data-ttu-id="b3805-165">Add-AzureHDInsightConfigValues</span><span class="sxs-lookup"><span data-stu-id="b3805-165">Add-AzureHDInsightConfigValues</span></span> |[<span data-ttu-id="b3805-166">Přidat AzureRmHDInsightConfigValues</span><span class="sxs-lookup"><span data-stu-id="b3805-166">Add-AzureRmHDInsightConfigValues</span></span>](https://msdn.microsoft.com/library/mt603530.aspx) |
| <span data-ttu-id="b3805-167">Add-AzureHDInsightMetastore</span><span class="sxs-lookup"><span data-stu-id="b3805-167">Add-AzureHDInsightMetastore</span></span> |[<span data-ttu-id="b3805-168">Přidat AzureRmHDInsightMetastore</span><span class="sxs-lookup"><span data-stu-id="b3805-168">Add-AzureRmHDInsightMetastore</span></span>](https://msdn.microsoft.com/library/mt603670.aspx) |
| <span data-ttu-id="b3805-169">Add-AzureHDInsightScriptAction</span><span class="sxs-lookup"><span data-stu-id="b3805-169">Add-AzureHDInsightScriptAction</span></span> |[<span data-ttu-id="b3805-170">Přidat AzureRmHDInsightScriptAction</span><span class="sxs-lookup"><span data-stu-id="b3805-170">Add-AzureRmHDInsightScriptAction</span></span>](https://msdn.microsoft.com/library/mt603527.aspx) |
| <span data-ttu-id="b3805-171">Add-AzureHDInsightStorage</span><span class="sxs-lookup"><span data-stu-id="b3805-171">Add-AzureHDInsightStorage</span></span> |[<span data-ttu-id="b3805-172">Přidat AzureRmHDInsightStorage</span><span class="sxs-lookup"><span data-stu-id="b3805-172">Add-AzureRmHDInsightStorage</span></span>](https://msdn.microsoft.com/library/mt619445.aspx) |
| <span data-ttu-id="b3805-173">Get-AzureHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="b3805-173">Get-AzureHDInsightCluster</span></span> |[<span data-ttu-id="b3805-174">Get-AzureRmHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="b3805-174">Get-AzureRmHDInsightCluster</span></span>](https://msdn.microsoft.com/library/mt619371.aspx) |
| <span data-ttu-id="b3805-175">Get-AzureHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="b3805-175">Get-AzureHDInsightJob</span></span> |[<span data-ttu-id="b3805-176">Get-AzureRmHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="b3805-176">Get-AzureRmHDInsightJob</span></span>](https://msdn.microsoft.com/library/mt603590.aspx) |
| <span data-ttu-id="b3805-177">Get-AzureHDInsightJobOutput</span><span class="sxs-lookup"><span data-stu-id="b3805-177">Get-AzureHDInsightJobOutput</span></span> |[<span data-ttu-id="b3805-178">Get-AzureRmHDInsightJobOutput</span><span class="sxs-lookup"><span data-stu-id="b3805-178">Get-AzureRmHDInsightJobOutput</span></span>](https://msdn.microsoft.com/library/mt603793.aspx) |
| <span data-ttu-id="b3805-179">Get-AzureHDInsightProperties</span><span class="sxs-lookup"><span data-stu-id="b3805-179">Get-AzureHDInsightProperties</span></span> |[<span data-ttu-id="b3805-180">Get-AzureRmHDInsightProperties</span><span class="sxs-lookup"><span data-stu-id="b3805-180">Get-AzureRmHDInsightProperties</span></span>](https://msdn.microsoft.com/library/mt603546.aspx) |
| <span data-ttu-id="b3805-181">Grant-AzureHDInsightHttpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="b3805-181">Grant-AzureHDInsightHttpServicesAccess</span></span> |[<span data-ttu-id="b3805-182">Udělení AzureRmHDInsightHttpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="b3805-182">Grant-AzureRmHDInsightHttpServicesAccess</span></span>](https://msdn.microsoft.com/library/mt619407.aspx) |
| <span data-ttu-id="b3805-183">Grant-AzureHdinsightRdpAccess</span><span class="sxs-lookup"><span data-stu-id="b3805-183">Grant-AzureHdinsightRdpAccess</span></span> |[<span data-ttu-id="b3805-184">Udělení AzureRmHDInsightRdpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="b3805-184">Grant-AzureRmHDInsightRdpServicesAccess</span></span>](https://msdn.microsoft.com/library/mt603717.aspx) |
| <span data-ttu-id="b3805-185">Invoke-AzureHDInsightHiveJob</span><span class="sxs-lookup"><span data-stu-id="b3805-185">Invoke-AzureHDInsightHiveJob</span></span> |[<span data-ttu-id="b3805-186">Vyvolání AzureRmHDInsightHiveJob</span><span class="sxs-lookup"><span data-stu-id="b3805-186">Invoke-AzureRmHDInsightHiveJob</span></span>](https://msdn.microsoft.com/library/mt603593.aspx) |
| <span data-ttu-id="b3805-187">New-AzureHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="b3805-187">New-AzureHDInsightCluster</span></span> |[<span data-ttu-id="b3805-188">Nové AzureRmHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="b3805-188">New-AzureRmHDInsightCluster</span></span>](https://msdn.microsoft.com/library/mt619331.aspx) |
| <span data-ttu-id="b3805-189">New-AzureHDInsightClusterConfig</span><span class="sxs-lookup"><span data-stu-id="b3805-189">New-AzureHDInsightClusterConfig</span></span> |[<span data-ttu-id="b3805-190">Nové AzureRmHDInsightClusterConfig</span><span class="sxs-lookup"><span data-stu-id="b3805-190">New-AzureRmHDInsightClusterConfig</span></span>](https://msdn.microsoft.com/library/mt603700.aspx) |
| <span data-ttu-id="b3805-191">New-AzureHDInsightHiveJobDefinition</span><span class="sxs-lookup"><span data-stu-id="b3805-191">New-AzureHDInsightHiveJobDefinition</span></span> |[<span data-ttu-id="b3805-192">Nové AzureRmHDInsightHiveJobDefinition</span><span class="sxs-lookup"><span data-stu-id="b3805-192">New-AzureRmHDInsightHiveJobDefinition</span></span>](https://msdn.microsoft.com/library/mt619448.aspx) |
| <span data-ttu-id="b3805-193">New-AzureHDInsightMapReduceJobDefinition</span><span class="sxs-lookup"><span data-stu-id="b3805-193">New-AzureHDInsightMapReduceJobDefinition</span></span> |[<span data-ttu-id="b3805-194">Nové AzureRmHDInsightMapReduceJobDefinition</span><span class="sxs-lookup"><span data-stu-id="b3805-194">New-AzureRmHDInsightMapReduceJobDefinition</span></span>](https://msdn.microsoft.com/library/mt603626.aspx) |
| <span data-ttu-id="b3805-195">New-AzureHDInsightPigJobDefinition</span><span class="sxs-lookup"><span data-stu-id="b3805-195">New-AzureHDInsightPigJobDefinition</span></span> |[<span data-ttu-id="b3805-196">Nové AzureRmHDInsightPigJobDefinition</span><span class="sxs-lookup"><span data-stu-id="b3805-196">New-AzureRmHDInsightPigJobDefinition</span></span>](https://msdn.microsoft.com/library/mt603671.aspx) |
| <span data-ttu-id="b3805-197">New-AzureHDInsightSqoopJobDefinition</span><span class="sxs-lookup"><span data-stu-id="b3805-197">New-AzureHDInsightSqoopJobDefinition</span></span> |[<span data-ttu-id="b3805-198">Nové AzureRmHDInsightSqoopJobDefinition</span><span class="sxs-lookup"><span data-stu-id="b3805-198">New-AzureRmHDInsightSqoopJobDefinition</span></span>](https://msdn.microsoft.com/library/mt608551.aspx) |
| <span data-ttu-id="b3805-199">New-AzureHDInsightStreamingMapReduceJobDefinition</span><span class="sxs-lookup"><span data-stu-id="b3805-199">New-AzureHDInsightStreamingMapReduceJobDefinition</span></span> |[<span data-ttu-id="b3805-200">Nové AzureRmHDInsightStreamingMapReduceJobDefinition</span><span class="sxs-lookup"><span data-stu-id="b3805-200">New-AzureRmHDInsightStreamingMapReduceJobDefinition</span></span>](https://msdn.microsoft.com/library/mt603626.aspx) |
| <span data-ttu-id="b3805-201">Remove-AzureHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="b3805-201">Remove-AzureHDInsightCluster</span></span> |[<span data-ttu-id="b3805-202">Odebrat AzureRmHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="b3805-202">Remove-AzureRmHDInsightCluster</span></span>](https://msdn.microsoft.com/library/mt619431.aspx) |
| <span data-ttu-id="b3805-203">Revoke-AzureHDInsightHttpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="b3805-203">Revoke-AzureHDInsightHttpServicesAccess</span></span> |[<span data-ttu-id="b3805-204">AzureRmHDInsightHttpServicesAccess odvolání.</span><span class="sxs-lookup"><span data-stu-id="b3805-204">Revoke-AzureRmHDInsightHttpServicesAccess</span></span>](https://msdn.microsoft.com/library/mt619375.aspx) |
| <span data-ttu-id="b3805-205">Revoke-AzureHdinsightRdpAccess</span><span class="sxs-lookup"><span data-stu-id="b3805-205">Revoke-AzureHdinsightRdpAccess</span></span> |[<span data-ttu-id="b3805-206">AzureRmHDInsightRdpServicesAccess odvolání.</span><span class="sxs-lookup"><span data-stu-id="b3805-206">Revoke-AzureRmHDInsightRdpServicesAccess</span></span>](https://msdn.microsoft.com/library/mt603523.aspx) |
| <span data-ttu-id="b3805-207">Set-AzureHDInsightClusterSize</span><span class="sxs-lookup"><span data-stu-id="b3805-207">Set-AzureHDInsightClusterSize</span></span> |[<span data-ttu-id="b3805-208">Set-AzureRmHDInsightClusterSize</span><span class="sxs-lookup"><span data-stu-id="b3805-208">Set-AzureRmHDInsightClusterSize</span></span>](https://msdn.microsoft.com/library/mt603513.aspx) |
| <span data-ttu-id="b3805-209">Set-AzureHDInsightDefaultStorage</span><span class="sxs-lookup"><span data-stu-id="b3805-209">Set-AzureHDInsightDefaultStorage</span></span> |[<span data-ttu-id="b3805-210">Set-AzureRmHDInsightDefaultStorage</span><span class="sxs-lookup"><span data-stu-id="b3805-210">Set-AzureRmHDInsightDefaultStorage</span></span>](https://msdn.microsoft.com/library/mt603486.aspx) |
| <span data-ttu-id="b3805-211">Start-AzureHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="b3805-211">Start-AzureHDInsightJob</span></span> |[<span data-ttu-id="b3805-212">Počáteční AzureRmHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="b3805-212">Start-AzureRmHDInsightJob</span></span>](https://msdn.microsoft.com/library/mt603798.aspx) |
| <span data-ttu-id="b3805-213">Stop-AzureHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="b3805-213">Stop-AzureHDInsightJob</span></span> |[<span data-ttu-id="b3805-214">Stop-AzureRmHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="b3805-214">Stop-AzureRmHDInsightJob</span></span>](https://msdn.microsoft.com/library/mt619424.aspx) |
| <span data-ttu-id="b3805-215">Use-AzureHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="b3805-215">Use-AzureHDInsightCluster</span></span> |[<span data-ttu-id="b3805-216">Použití AzureRmHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="b3805-216">Use-AzureRmHDInsightCluster</span></span>](https://msdn.microsoft.com/library/mt619442.aspx) |
| <span data-ttu-id="b3805-217">Wait-AzureHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="b3805-217">Wait-AzureHDInsightJob</span></span> |[<span data-ttu-id="b3805-218">Počkejte AzureRmHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="b3805-218">Wait-AzureRmHDInsightJob</span></span>](https://msdn.microsoft.com/library/mt603834.aspx) |

### <a name="new-cmdlets"></a><span data-ttu-id="b3805-219">Nové rutiny</span><span class="sxs-lookup"><span data-stu-id="b3805-219">New cmdlets</span></span>
<span data-ttu-id="b3805-220">Hello následují hello nové rutiny, které jsou dostupné jenom v režimu ARM hello.</span><span class="sxs-lookup"><span data-stu-id="b3805-220">hello following are hello new cmdlets that are only available in hello ARM mode.</span></span> 

<span data-ttu-id="b3805-221">**Akce skriptu související rutiny:**</span><span class="sxs-lookup"><span data-stu-id="b3805-221">**Script action related cmdlets:**</span></span>

* <span data-ttu-id="b3805-222">**Get-AzureRmHDInsightPersistedScriptAction**: hello získá trvalé akce skriptu pro cluster s podporou a zobrazí je v chronologickém pořadí nebo získá podrobnosti pro akci zadaný trvalého skriptu.</span><span class="sxs-lookup"><span data-stu-id="b3805-222">**Get-AzureRmHDInsightPersistedScriptAction**: Gets hello persisted script actions for a cluster and lists them in chronological order, or gets details for a specified persisted script action.</span></span> 
* <span data-ttu-id="b3805-223">**Get-AzureRmHDInsightScriptActionHistory**: získá hello v historii akcí skriptu pro cluster a seznamů v obráceném chronologickém pořadí nebo získá podrobnosti akce dříve spuštění skriptu.</span><span class="sxs-lookup"><span data-stu-id="b3805-223">**Get-AzureRmHDInsightScriptActionHistory**: Gets hello script action history for a cluster and lists it in reverse chronological order, or gets details of a previously executed script action.</span></span> 
* <span data-ttu-id="b3805-224">**Odebrat AzureRmHDInsightPersistedScriptAction**: Odebere akcí trvalého skriptu z clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b3805-224">**Remove-AzureRmHDInsightPersistedScriptAction**: Removes a persisted script action from an HDInsight cluster.</span></span>
* <span data-ttu-id="b3805-225">**Set-AzureRmHDInsightPersistedScriptAction**: Nastaví dříve spustit skript akce toobe akcí trvalého skriptu.</span><span class="sxs-lookup"><span data-stu-id="b3805-225">**Set-AzureRmHDInsightPersistedScriptAction**: Sets a previously executed script action toobe a persisted script action.</span></span>
* <span data-ttu-id="b3805-226">**Odeslání AzureRmHDInsightScriptAction**: odešle nový cluster Azure HDInsight akce tooan skriptu.</span><span class="sxs-lookup"><span data-stu-id="b3805-226">**Submit-AzureRmHDInsightScriptAction**: Submits a new script action tooan Azure HDInsight cluster.</span></span> 

<span data-ttu-id="b3805-227">Využití Další informace najdete v tématu [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="b3805-227">For additional usage information, see [Customize Linux-based HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

<span data-ttu-id="b3805-228">**Clsuter identity související rutiny:**</span><span class="sxs-lookup"><span data-stu-id="b3805-228">**Clsuter identity related cmdlets:**</span></span>

* <span data-ttu-id="b3805-229">**Přidat AzureRmHDInsightClusterIdentity**: Přidá objekt konfigurace clusteru tooa clusteru identity tak, aby hello clusteru HDInsight můžete přístup k Azure Data Lake úložiště.</span><span class="sxs-lookup"><span data-stu-id="b3805-229">**Add-AzureRmHDInsightClusterIdentity**: Adds a cluster identity tooa cluster configuration object so that hello HDInsight cluster can access Azure Data Lake Stores.</span></span> <span data-ttu-id="b3805-230">V tématu [vytvoření clusteru HDInsight s Data Lake Store pomocí Azure PowerShell](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="b3805-230">See [Create an HDInsight cluster with Data Lake Store using Azure PowerShell](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).</span></span>

### <a name="examples"></a><span data-ttu-id="b3805-231">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3805-231">Examples</span></span>
<span data-ttu-id="b3805-232">**Vytvoření clusteru**</span><span class="sxs-lookup"><span data-stu-id="b3805-232">**Create cluster**</span></span>

<span data-ttu-id="b3805-233">Příkaz staré (ASM):</span><span class="sxs-lookup"><span data-stu-id="b3805-233">Old command (ASM):</span></span> 

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

<span data-ttu-id="b3805-234">Nový příkaz (ARM):</span><span class="sxs-lookup"><span data-stu-id="b3805-234">New command (ARM):</span></span>

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


<span data-ttu-id="b3805-235">**Odstranění clusteru**</span><span class="sxs-lookup"><span data-stu-id="b3805-235">**Delete cluster**</span></span>

<span data-ttu-id="b3805-236">Příkaz staré (ASM):</span><span class="sxs-lookup"><span data-stu-id="b3805-236">Old command (ASM):</span></span>

    Remove-AzureHDInsightCluster -name $clusterName 

<span data-ttu-id="b3805-237">Nový příkaz (ARM):</span><span class="sxs-lookup"><span data-stu-id="b3805-237">New command (ARM):</span></span>

    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName 

<span data-ttu-id="b3805-238">**Seznam clusteru**</span><span class="sxs-lookup"><span data-stu-id="b3805-238">**List cluster**</span></span>

<span data-ttu-id="b3805-239">Příkaz staré (ASM):</span><span class="sxs-lookup"><span data-stu-id="b3805-239">Old command (ASM):</span></span>

    Get-AzureHDInsightCluster

<span data-ttu-id="b3805-240">Nový příkaz (ARM):</span><span class="sxs-lookup"><span data-stu-id="b3805-240">New command (ARM):</span></span>

    Get-AzureRmHDInsightCluster 

<span data-ttu-id="b3805-241">**Zobrazit clusteru**</span><span class="sxs-lookup"><span data-stu-id="b3805-241">**Show cluster**</span></span>

<span data-ttu-id="b3805-242">Příkaz staré (ASM):</span><span class="sxs-lookup"><span data-stu-id="b3805-242">Old command (ASM):</span></span>

    Get-AzureHDInsightCluster -Name $clusterName

<span data-ttu-id="b3805-243">Nový příkaz (ARM):</span><span class="sxs-lookup"><span data-stu-id="b3805-243">New command (ARM):</span></span>

    Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -clusterName $clusterName


#### <a name="other-samples"></a><span data-ttu-id="b3805-244">Další ukázky</span><span class="sxs-lookup"><span data-stu-id="b3805-244">Other samples</span></span>
* [<span data-ttu-id="b3805-245">Vytvoření clusterů HDInsight</span><span class="sxs-lookup"><span data-stu-id="b3805-245">Create HDInsight clusters</span></span>](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [<span data-ttu-id="b3805-246">Odeslání úloh Hive</span><span class="sxs-lookup"><span data-stu-id="b3805-246">Submit Hive jobs</span></span>](hdinsight-hadoop-use-hive-powershell.md)
* [<span data-ttu-id="b3805-247">Odeslání úlohy Pig</span><span class="sxs-lookup"><span data-stu-id="b3805-247">Submit Pig jobs</span></span>](hdinsight-hadoop-use-pig-powershell.md)
* [<span data-ttu-id="b3805-248">Odesílání úloh Sqoop</span><span class="sxs-lookup"><span data-stu-id="b3805-248">Submit Sqoop jobs</span></span>](hdinsight-hadoop-use-sqoop-powershell.md)

## <a name="migrating-toohello-arm-based-hdinsight-net-sdk"></a><span data-ttu-id="b3805-249">Migrace toohello založené na ARM HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="b3805-249">Migrating toohello ARM-based HDInsight .NET SDK</span></span>
<span data-ttu-id="b3805-250">Dobrý den, na základě Azure Service Management [(ASM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt416619.aspx) je nyní zastaralý.</span><span class="sxs-lookup"><span data-stu-id="b3805-250">hello Azure Service Management-based [(ASM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt416619.aspx) is now deprecated.</span></span> <span data-ttu-id="b3805-251">Jsou podporovali toouse hello založený na správě prostředků Azure [(ARM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt271028.aspx).</span><span class="sxs-lookup"><span data-stu-id="b3805-251">You are encouraged toouse hello Azure Resource Management-based [(ARM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt271028.aspx).</span></span> <span data-ttu-id="b3805-252">Hello následující HDInsight se systémem ASM balíčky jsou zastaralá.</span><span class="sxs-lookup"><span data-stu-id="b3805-252">hello following ASM-based HDInsight packages are being deprecated.</span></span>

* `Microsoft.WindowsAzure.Management.HDInsight`
* `Microsoft.Hadoop.Client`

<span data-ttu-id="b3805-253">Tato část obsahuje ukazatele toomore informace o tom, tooperform určité úlohy pomocí hello založené na ARM SDK.</span><span class="sxs-lookup"><span data-stu-id="b3805-253">This section provides pointers toomore information on how tooperform certain tasks using hello ARM-based SDK.</span></span>

| <span data-ttu-id="b3805-254">Postup... pomocí hello SDK HDInsight založené na ARM</span><span class="sxs-lookup"><span data-stu-id="b3805-254">How to... using hello ARM-based HDInsight SDK</span></span> | <span data-ttu-id="b3805-255">Odkazy</span><span class="sxs-lookup"><span data-stu-id="b3805-255">Links</span></span> |
| --- | --- |
| <span data-ttu-id="b3805-256">Vytvoření clusterů HDInsight pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="b3805-256">Create HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="b3805-257">V tématu [Tvorba clusterů HDInsight pomocí sady .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="b3805-257">See [Create HDInsight clusters using .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span></span> |
| <span data-ttu-id="b3805-258">Přizpůsobení clusteru pomocí akce skriptu pomocí .NET SDK</span><span class="sxs-lookup"><span data-stu-id="b3805-258">Customize a cluster using Script Action with .NET SDK</span></span> |<span data-ttu-id="b3805-259">V tématu [HDInsight Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action)</span><span class="sxs-lookup"><span data-stu-id="b3805-259">See [Customize HDInsight Linux clusters using Script Action](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action)</span></span> |
| <span data-ttu-id="b3805-260">Ověření aplikací interaktivně pomocí .NET SDK služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b3805-260">Authenticate applications interactively using Azure Active Directory with .NET SDK</span></span> |<span data-ttu-id="b3805-261">V tématu [spouštění dotazů Hive pomocí sady .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="b3805-261">See [Run Hive queries using .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span></span> <span data-ttu-id="b3805-262">Hello fragment kódu v tomto článku používá hello interaktivního ověřování přístup.</span><span class="sxs-lookup"><span data-stu-id="b3805-262">hello code snippet in this article uses hello interactive authentication approach.</span></span> |
| <span data-ttu-id="b3805-263">Ověření aplikací interaktivně pomocí .NET SDK služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b3805-263">Authenticate applications non-interactively using Azure Active Directory with .NET SDK</span></span> |<span data-ttu-id="b3805-264">V tématu [vytvořit neinteraktivní aplikace pro HDInsight](hdinsight-create-non-interactive-authentication-dotnet-applications.md)</span><span class="sxs-lookup"><span data-stu-id="b3805-264">See [Create non-interactive applications for HDInsight](hdinsight-create-non-interactive-authentication-dotnet-applications.md)</span></span> |
| <span data-ttu-id="b3805-265">Odeslání úlohy Hive pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="b3805-265">Submit a Hive job using .NET SDK</span></span> |<span data-ttu-id="b3805-266">V tématu [úlohy odeslání Hive](hdinsight-hadoop-use-hive-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="b3805-266">See [Submit Hive jobs](hdinsight-hadoop-use-hive-dotnet-sdk.md)</span></span> |
| <span data-ttu-id="b3805-267">Odeslat úlohu Pig pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="b3805-267">Submit a Pig job using .NET SDK</span></span> |<span data-ttu-id="b3805-268">V tématu [úlohy odeslání Pig](hdinsight-hadoop-use-pig-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="b3805-268">See [Submit Pig jobs](hdinsight-hadoop-use-pig-dotnet-sdk.md)</span></span> |
| <span data-ttu-id="b3805-269">Odeslání úlohy Sqoop pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="b3805-269">Submit a Sqoop job using .NET SDK</span></span> |<span data-ttu-id="b3805-270">V tématu [Sqoop odeslání úlohy](hdinsight-hadoop-use-sqoop-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="b3805-270">See [Submit Sqoop jobs](hdinsight-hadoop-use-sqoop-dotnet-sdk.md)</span></span> |
| <span data-ttu-id="b3805-271">Seznam clusterů HDInsight pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="b3805-271">List HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="b3805-272">V tématu [clusterů HDInsight seznamu](hdinsight-administer-use-dotnet-sdk.md#list-clusters)</span><span class="sxs-lookup"><span data-stu-id="b3805-272">See [List HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#list-clusters)</span></span> |
| <span data-ttu-id="b3805-273">Škálování clusterů HDInsight pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="b3805-273">Scale HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="b3805-274">V tématu [clusterů HDInsight škálování](hdinsight-administer-use-dotnet-sdk.md#scale-clusters)</span><span class="sxs-lookup"><span data-stu-id="b3805-274">See [Scale HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#scale-clusters)</span></span> |
| <span data-ttu-id="b3805-275">Udělení nebo odvolání přístupu tooHDInsight clustery pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="b3805-275">Grant/revoke access tooHDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="b3805-276">V tématu [udělení nebo odvolání přístupu tooHDInsight clustery](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access)</span><span class="sxs-lookup"><span data-stu-id="b3805-276">See [Grant/revoke access tooHDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access)</span></span> |
| <span data-ttu-id="b3805-277">Aktualizovat pověření uživatele HTTP pro clustery služby HDInsight pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="b3805-277">Update HTTP user credentials for HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="b3805-278">V tématu [přihlašovací údaje uživatele HTTP aktualizace pro clustery služby HDInsight](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials)</span><span class="sxs-lookup"><span data-stu-id="b3805-278">See [Update HTTP user credentials for HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials)</span></span> |
| <span data-ttu-id="b3805-279">Najít hello výchozí účet úložiště pro clustery služby HDInsight pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="b3805-279">Find hello default storage account for HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="b3805-280">V tématu [najít hello výchozí účet úložiště pro clustery služby HDInsight](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account)</span><span class="sxs-lookup"><span data-stu-id="b3805-280">See [Find hello default storage account for HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account)</span></span> |
| <span data-ttu-id="b3805-281">Odstranění clusterů HDInsight pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="b3805-281">Delete HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="b3805-282">V tématu [clusterů HDInsight odstranit](hdinsight-administer-use-dotnet-sdk.md#delete-clusters)</span><span class="sxs-lookup"><span data-stu-id="b3805-282">See [Delete HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#delete-clusters)</span></span> |

### <a name="examples"></a><span data-ttu-id="b3805-283">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3805-283">Examples</span></span>
<span data-ttu-id="b3805-284">Tady jsou některé příklady na to, jak je operace používá hello na základě ASM SDK a fragmentu kódu ekvivalentní hello hello založené na ARM SDK.</span><span class="sxs-lookup"><span data-stu-id="b3805-284">Following are some examples on how an operation is performed using hello ASM-based SDK and hello equivalent code snippet for hello ARM-based SDK.</span></span>

<span data-ttu-id="b3805-285">**Vytvoření klienta CRUD clusteru**</span><span class="sxs-lookup"><span data-stu-id="b3805-285">**Creating a cluster CRUD client**</span></span>

* <span data-ttu-id="b3805-286">Příkaz staré (ASM)</span><span class="sxs-lookup"><span data-stu-id="b3805-286">Old command (ASM)</span></span>
  
        //Certificate auth
        //This logs hello application in using a subscription administration certificate, which is not offered in Azure Resource Manager (ARM)
  
        const string subid = "454467d4-60ca-4dfd-a556-216eeeeeeee1";
        var cred = new HDInsightCertificateCredential(new Guid(subid), new X509Certificate2(@"path\to\certificate.cer"));
        var client = HDInsightClient.Connect(cred);
* <span data-ttu-id="b3805-287">Nový příkaz (ARM) (hlavní autorizace Service)</span><span class="sxs-lookup"><span data-stu-id="b3805-287">New command (ARM) (Service principal authorization)</span></span>
  
        //Service principal auth
        //This will log hello application in as itself, rather than on behalf of a specific user.
        //For details, including how tooset up hello application, see:
        //   https://azure.microsoft.com/en-us/documentation/articles/hdinsight-create-non-interactive-authentication-dotnet-applications/
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);
* <span data-ttu-id="b3805-288">Nový příkaz (ARM) (autorizace uživatelů)</span><span class="sxs-lookup"><span data-stu-id="b3805-288">New command (ARM) (User authorization)</span></span>
  
        //User auth
        //This will log hello application in on behalf of hello user.
        //hello end-user will see a login popup.
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.User, Id = username };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, AuthenticationFactory.CommonAdTenant, password, ShowDialog.Auto).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);

<span data-ttu-id="b3805-289">**Vytvoření clusteru**</span><span class="sxs-lookup"><span data-stu-id="b3805-289">**Creating a cluster**</span></span>

* <span data-ttu-id="b3805-290">Příkaz staré (ASM)</span><span class="sxs-lookup"><span data-stu-id="b3805-290">Old command (ASM)</span></span>
  
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
* <span data-ttu-id="b3805-291">Nový příkaz (ARM)</span><span class="sxs-lookup"><span data-stu-id="b3805-291">New command (ARM)</span></span>
  
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

<span data-ttu-id="b3805-292">**Povolíte přístup protokolu HTTP**</span><span class="sxs-lookup"><span data-stu-id="b3805-292">**Enabling HTTP access**</span></span>

* <span data-ttu-id="b3805-293">Příkaz staré (ASM)</span><span class="sxs-lookup"><span data-stu-id="b3805-293">Old command (ASM)</span></span>
  
        client.EnableHttp(dnsName, "West US", "admin", "*******");
* <span data-ttu-id="b3805-294">Nový příkaz (ARM)</span><span class="sxs-lookup"><span data-stu-id="b3805-294">New command (ARM)</span></span>
  
        var httpParams = new HttpSettingsParameters
        {
               HttpUserEnabled = true,
               HttpUsername = "admin",
               HttpPassword = "*******",
        };
        client.Clusters.ConfigureHttpSettings(resourceGroup, dnsname, httpParams);

<span data-ttu-id="b3805-295">**Odstranění clusteru**</span><span class="sxs-lookup"><span data-stu-id="b3805-295">**Deleting a cluster**</span></span>

* <span data-ttu-id="b3805-296">Příkaz staré (ASM)</span><span class="sxs-lookup"><span data-stu-id="b3805-296">Old command (ASM)</span></span>
  
        client.DeleteCluster(dnsName);
* <span data-ttu-id="b3805-297">Nový příkaz (ARM)</span><span class="sxs-lookup"><span data-stu-id="b3805-297">New command (ARM)</span></span>
  
        client.Clusters.Delete(resourceGroup, dnsname);

