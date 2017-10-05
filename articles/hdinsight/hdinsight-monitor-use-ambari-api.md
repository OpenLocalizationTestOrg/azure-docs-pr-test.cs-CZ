---
title: "Monitorování clusterů systému Hadoop v HDInsight pomocí nástroje Ambari API - Azure | Microsoft Docs"
description: "Použití rozhraní Apache Ambari API pro vytvoření, správě a monitoringu clusterů systému Hadoop. Operátor intuitivní nástroje a rozhraní API překonávají složitost systému Hadoop."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
editor: cgronlun
manager: jhubbard
ms.assetid: 052135b3-d497-4acc-92ff-71cee49356ff
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: b6fc2098027690eb76b69b1427f0e9541b8a7a69
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-hadoop-clusters-in-hdinsight-using-the-ambari-api"></a><span data-ttu-id="e2565-104">Sledování clusterů Hadoop ve službě HDInsight pomocí rozhraní API Ambari</span><span class="sxs-lookup"><span data-stu-id="e2565-104">Monitor Hadoop clusters in HDInsight using the Ambari API</span></span>
<span data-ttu-id="e2565-105">Naučte se monitorovat clusterů HDInsight pomocí Ambari API.</span><span class="sxs-lookup"><span data-stu-id="e2565-105">Learn how to monitor HDInsight clusters by using Ambari APIs.</span></span>

> [!NOTE]
> <span data-ttu-id="e2565-106">Informace v tomto článku je určen pro clustery HDInsight se systémem Windows, které poskytují jen pro čtení verzi Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="e2565-106">The information in this article is primarily for Windows-based HDInsight clusters, which provide a read-only version of the Ambari REST API.</span></span> <span data-ttu-id="e2565-107">V clusterech se systémem Linux naleznete v části [clusterů systému Hadoop spravovat pomocí nástroje Ambari](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="e2565-107">For Linux-based clusters, see [Manage Hadoop clusters using Ambari](hdinsight-hadoop-manage-ambari.md).</span></span>
> 
> 

## <a name="what-is-ambari"></a><span data-ttu-id="e2565-108">Co je Ambari?</span><span class="sxs-lookup"><span data-stu-id="e2565-108">What is Ambari?</span></span>
<span data-ttu-id="e2565-109">[Apache Ambari] [ ambari-home] se používá pro zřizování, správě a monitoringu clusterů systému Apache Hadoop.</span><span class="sxs-lookup"><span data-stu-id="e2565-109">[Apache Ambari][ambari-home] is used for provisioning, managing, and monitoring Apache Hadoop clusters.</span></span> <span data-ttu-id="e2565-110">Obsahuje intuitivní sadu nástrojů pro operátory a výkonnou sadu rozhraní API, které překonávají složitost systému Hadoop a zjednodušují operace s clustery.</span><span class="sxs-lookup"><span data-stu-id="e2565-110">It includes an intuitive collection of operator tools and a robust set of APIs that hide the complexity of Hadoop, simplifying the operation of clusters.</span></span> <span data-ttu-id="e2565-111">Další informace o rozhraních API najdete v tématu [referenční dokumentace rozhraní API Ambari][ambari-api-reference].</span><span class="sxs-lookup"><span data-stu-id="e2565-111">For more information about the APIs, see [Ambari API Reference][ambari-api-reference].</span></span> 

<span data-ttu-id="e2565-112">HDInsight aktuálně podporuje pouze monitorování funkce Ambari.</span><span class="sxs-lookup"><span data-stu-id="e2565-112">HDInsight currently supports only the Ambari monitoring feature.</span></span> <span data-ttu-id="e2565-113">Clustery HDInsight verze 3.0 a 2.1 podporuje Ambari API 1.0.</span><span class="sxs-lookup"><span data-stu-id="e2565-113">Ambari API 1.0 is supported by HDInsight version 3.0 and 2.1 clusters.</span></span> <span data-ttu-id="e2565-114">Tento článek se týká přístupu k rozhraní API Ambari v clusterech HDInsight verze 3.1 nebo 2.1.</span><span class="sxs-lookup"><span data-stu-id="e2565-114">This article covers accessing Ambari APIs on HDInsight version 3.1 and 2.1 clusters.</span></span> <span data-ttu-id="e2565-115">Klíčovým rozdílem mezi těmito dvěma je, že některé součásti změnily se zavedením nové funkce (například Server historie úlohy).</span><span class="sxs-lookup"><span data-stu-id="e2565-115">The key difference between the two is that some of the components have changed with the introduction of new capabilities (such as the Job History Server).</span></span> 

<span data-ttu-id="e2565-116">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="e2565-116">**Prerequisites**</span></span>

<span data-ttu-id="e2565-117">Před zahájením tohoto kurzu musíte mít tyto položky:</span><span class="sxs-lookup"><span data-stu-id="e2565-117">Before you begin this tutorial, you must have the following items:</span></span>

* <span data-ttu-id="e2565-118">**Pracovní stanice s prostředím Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="e2565-118">**A workstation with Azure PowerShell**.</span></span>
* <span data-ttu-id="e2565-119">(Volitelné) [cURL][curl].</span><span class="sxs-lookup"><span data-stu-id="e2565-119">(Optional) [cURL][curl].</span></span> <span data-ttu-id="e2565-120">Ho Pokud chcete nainstalovat, najdete v části [cURL verze a soubory ke stažení][curl-download].</span><span class="sxs-lookup"><span data-stu-id="e2565-120">To install it, see [cURL Releases and Downloads][curl-download].</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="e2565-121">Když v systému Windows, použijte dvojité uvozovky značky místo jednoduchých uvozovek pro hodnoty možnosti použít příkaz cURL.</span><span class="sxs-lookup"><span data-stu-id="e2565-121">When use the cURL command in Windows, use double-quotation marks instead of single-quotation marks for the option values.</span></span>
  > 
  > 
* <span data-ttu-id="e2565-122">**Cluster Azure HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="e2565-122">**An Azure HDInsight cluster**.</span></span> <span data-ttu-id="e2565-123">Pokyny o zřizování clusteru najdete v tématu [začněte používat HDInsight] [ hdinsight-get-started] nebo [zřizování clusterů HDInsight][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="e2565-123">For instructions about cluster provisioning, see [Get started using HDInsight][hdinsight-get-started] or [Provision HDInsight clusters][hdinsight-provision].</span></span> <span data-ttu-id="e2565-124">Je třeba projít tento kurz následující data:</span><span class="sxs-lookup"><span data-stu-id="e2565-124">You need the following data to go through the tutorial:</span></span>
  
  | <span data-ttu-id="e2565-125">Vlastnost clusteru</span><span class="sxs-lookup"><span data-stu-id="e2565-125">Cluster property</span></span> | <span data-ttu-id="e2565-126">Název proměnné Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="e2565-126">Azure PowerShell variable name</span></span> | <span data-ttu-id="e2565-127">Hodnota</span><span class="sxs-lookup"><span data-stu-id="e2565-127">Value</span></span> | <span data-ttu-id="e2565-128">Popis</span><span class="sxs-lookup"><span data-stu-id="e2565-128">Description</span></span> |
  | --- | --- | --- | --- |
  |   <span data-ttu-id="e2565-129">Název clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="e2565-129">HDInsight cluster name</span></span> |<span data-ttu-id="e2565-130">$clusterName</span><span class="sxs-lookup"><span data-stu-id="e2565-130">$clusterName</span></span> | |<span data-ttu-id="e2565-131">Název clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e2565-131">The name of your HDInsight cluster.</span></span> |
  |   <span data-ttu-id="e2565-132">Uživatelské jméno clusteru</span><span class="sxs-lookup"><span data-stu-id="e2565-132">Cluster username</span></span> |<span data-ttu-id="e2565-133">$clusterUsername</span><span class="sxs-lookup"><span data-stu-id="e2565-133">$clusterUsername</span></span> | |<span data-ttu-id="e2565-134">Vytvoření clusteru zadané clusteru uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="e2565-134">Cluster user name specified when the cluster was created.</span></span> |
  |   <span data-ttu-id="e2565-135">Heslo clusteru</span><span class="sxs-lookup"><span data-stu-id="e2565-135">Cluster password</span></span> |<span data-ttu-id="e2565-136">$clusterPassword</span><span class="sxs-lookup"><span data-stu-id="e2565-136">$clusterPassword</span></span> | |<span data-ttu-id="e2565-137">Heslo uživatele clusteru.</span><span class="sxs-lookup"><span data-stu-id="e2565-137">Cluster user password.</span></span> |

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


## <a name="jump-start"></a><span data-ttu-id="e2565-138">Rychle začít s prací</span><span class="sxs-lookup"><span data-stu-id="e2565-138">Jump-start</span></span>
<span data-ttu-id="e2565-139">Existuje několik způsobů monitorování clusterů HDInsight pomocí Ambari.</span><span class="sxs-lookup"><span data-stu-id="e2565-139">There are several ways to use Ambari to monitor HDInsight clusters.</span></span>

<span data-ttu-id="e2565-140">**Použití Azure Powershellu**</span><span class="sxs-lookup"><span data-stu-id="e2565-140">**Use Azure PowerShell**</span></span>

<span data-ttu-id="e2565-141">Následující skript prostředí Azure PowerShell získá informace o sledování úlohy MapReduce *v clusteru služby HDInsight 3.5.*</span><span class="sxs-lookup"><span data-stu-id="e2565-141">The following Azure PowerShell script gets the MapReduce job tracker information *in an HDInsight 3.5 cluster.*</span></span>  <span data-ttu-id="e2565-142">Klíčovým rozdílem je, že jsme pro vyžádání obsahu tyto podrobnosti z YARN služby (nikoli MapReduce).</span><span class="sxs-lookup"><span data-stu-id="e2565-142">The key difference is that we pull these details from the YARN service (rather than MapReduce).</span></span>

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName/services/YARN/components/RESOURCEMANAGER"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

<span data-ttu-id="e2565-143">Následující skript prostředí PowerShell získá informace o sledování úloh MapReduce *v clusteru služby HDInsight 2.1*:</span><span class="sxs-lookup"><span data-stu-id="e2565-143">The following PowerShell script gets the MapReduce job tracker information *in an HDInsight 2.1 cluster*:</span></span>

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

<span data-ttu-id="e2565-144">Výstup je:</span><span class="sxs-lookup"><span data-stu-id="e2565-144">The output is:</span></span>

![Výstup Jobtracker][img-jobtracker-output]

<span data-ttu-id="e2565-146">**Použití cURL**</span><span class="sxs-lookup"><span data-stu-id="e2565-146">**Use cURL**</span></span>

<span data-ttu-id="e2565-147">Následující příklad získá informace o clusteru pomocí cURL:</span><span class="sxs-lookup"><span data-stu-id="e2565-147">The following example gets cluster information by using cURL:</span></span>

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

<span data-ttu-id="e2565-148">Výstup je:</span><span class="sxs-lookup"><span data-stu-id="e2565-148">The output is:</span></span>

    {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/",
     "Clusters":{"cluster_name":"hdi0211v2.azurehdinsight.net","version":"2.1.3.0.432823"},
     "services"[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/hdfs",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"hdfs"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/mapreduce",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"mapreduce"}}],
     "hosts":[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/headnode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/workernode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0.{ClusterDNS}.azurehdinsight.net"}}]}

<span data-ttu-id="e2565-149">**Pro tuto verzi 8/10/2014**:</span><span class="sxs-lookup"><span data-stu-id="e2565-149">**For the 10/8/2014 release**:</span></span>

<span data-ttu-id="e2565-150">Při použití Ambari koncového bodu, "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}" *název_hostitele* pole Vrátí plně kvalifikovaný název domény (FQDN) uzlu místo názvu hostitele.</span><span class="sxs-lookup"><span data-stu-id="e2565-150">When using the Ambari endpoint, "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", the *host_name* field returns the fully qualified domain name (FQDN) of the node instead of the host name.</span></span> <span data-ttu-id="e2565-151">Před vydáním 8/10/2014, v tomto příkladu vrátil jednoduše "**headnode0**".</span><span class="sxs-lookup"><span data-stu-id="e2565-151">Before the 10/8/2014 release, this example returned simply "**headnode0**".</span></span> <span data-ttu-id="e2565-152">Po vydání 8/10/2014 získat plně kvalifikovaný název domény "**headnode0. { ClusterDNS} gt; .azurehdinsight .net**", jak je uvedeno v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="e2565-152">After the 10/8/2014 release, you get the FQDN "**headnode0.{ClusterDNS}.azurehdinsight.net**", as shown in the previous example.</span></span> <span data-ttu-id="e2565-153">Tato změna vyžaduje usnadnění scénáře, kde je možné nasadit více typů clusteru (například HBase a Hadoop) v jednu virtuální síť (VNET).</span><span class="sxs-lookup"><span data-stu-id="e2565-153">This change was required to facilitate scenarios where multiple cluster types (such as HBase and Hadoop) can be deployed in one virtual network (VNET).</span></span> <span data-ttu-id="e2565-154">K tomu dojde, například při použití HBase jako back-end platformy pro Hadoop.</span><span class="sxs-lookup"><span data-stu-id="e2565-154">This happens, for example, when using HBase as a back-end platform for Hadoop.</span></span>

## <a name="ambari-monitoring-apis"></a><span data-ttu-id="e2565-155">Ambari monitorování rozhraní API</span><span class="sxs-lookup"><span data-stu-id="e2565-155">Ambari monitoring APIs</span></span>
<span data-ttu-id="e2565-156">Následující tabulka uvádí některé z nejběžnějších Ambari monitorování volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e2565-156">The following table lists some of the most common Ambari monitoring API calls.</span></span> <span data-ttu-id="e2565-157">Další informace o rozhraní API najdete v tématu [referenční dokumentace rozhraní API Ambari][ambari-api-reference].</span><span class="sxs-lookup"><span data-stu-id="e2565-157">For more information about the API, see [Ambari API Reference][ambari-api-reference].</span></span>

| <span data-ttu-id="e2565-158">Volání rozhraní API monitorování</span><span class="sxs-lookup"><span data-stu-id="e2565-158">Monitor API call</span></span> | <span data-ttu-id="e2565-159">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="e2565-159">URI</span></span> | <span data-ttu-id="e2565-160">Popis</span><span class="sxs-lookup"><span data-stu-id="e2565-160">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e2565-161">Získat clustery</span><span class="sxs-lookup"><span data-stu-id="e2565-161">Get clusters</span></span> |`/api/v1/clusters` | |
| <span data-ttu-id="e2565-162">Získání informací o clusteru.</span><span class="sxs-lookup"><span data-stu-id="e2565-162">Get cluster info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net` |<span data-ttu-id="e2565-163">clustery, služby, hostitelé</span><span class="sxs-lookup"><span data-stu-id="e2565-163">clusters, services, hosts</span></span> |
| <span data-ttu-id="e2565-164">Získat služby</span><span class="sxs-lookup"><span data-stu-id="e2565-164">Get services</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services` |<span data-ttu-id="e2565-165">Mezi služby patří: hdfs, mapreduce</span><span class="sxs-lookup"><span data-stu-id="e2565-165">Services include: hdfs, mapreduce</span></span> |
| <span data-ttu-id="e2565-166">Získání informací o služby.</span><span class="sxs-lookup"><span data-stu-id="e2565-166">Get services info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>` | |
| <span data-ttu-id="e2565-167">Získat součásti služby</span><span class="sxs-lookup"><span data-stu-id="e2565-167">Get service components</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components` |<span data-ttu-id="e2565-168">HDFS: namenode, datanodeMapReduce: jobtracker; tasktracker</span><span class="sxs-lookup"><span data-stu-id="e2565-168">HDFS: namenode, datanodeMapReduce: jobtracker; tasktracker</span></span> |
| <span data-ttu-id="e2565-169">Získání informací o součásti.</span><span class="sxs-lookup"><span data-stu-id="e2565-169">Get component info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>` |<span data-ttu-id="e2565-170">ServiceComponentInfo, metriky, hostitele součásti</span><span class="sxs-lookup"><span data-stu-id="e2565-170">ServiceComponentInfo, host-components, metrics</span></span> |
| <span data-ttu-id="e2565-171">Získat hostitele</span><span class="sxs-lookup"><span data-stu-id="e2565-171">Get hosts</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts` |<span data-ttu-id="e2565-172">headnode0 workernode0</span><span class="sxs-lookup"><span data-stu-id="e2565-172">headnode0, workernode0</span></span> |
| <span data-ttu-id="e2565-173">Získání informací o hostiteli.</span><span class="sxs-lookup"><span data-stu-id="e2565-173">Get host info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>` | |
| <span data-ttu-id="e2565-174">Získat komponenty hostitele</span><span class="sxs-lookup"><span data-stu-id="e2565-174">Get host components</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components` |<span data-ttu-id="e2565-175">namenode, resourcemanager</span><span class="sxs-lookup"><span data-stu-id="e2565-175">namenode, resourcemanager</span></span> |
| <span data-ttu-id="e2565-176">Získáte informace o součást hostitele.</span><span class="sxs-lookup"><span data-stu-id="e2565-176">Get host component info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>` |<span data-ttu-id="e2565-177">HostRoles, součást, hostitele, metriky</span><span class="sxs-lookup"><span data-stu-id="e2565-177">HostRoles, component, host, metrics</span></span> |
| <span data-ttu-id="e2565-178">Získání konfigurace</span><span class="sxs-lookup"><span data-stu-id="e2565-178">Get configurations</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations` |<span data-ttu-id="e2565-179">Konfigurace typů: základní site, hdfs-site, mapred-site, hive-site</span><span class="sxs-lookup"><span data-stu-id="e2565-179">Config types: core-site, hdfs-site, mapred-site, hive-site</span></span> |
| <span data-ttu-id="e2565-180">Získáte informace o konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="e2565-180">Get configuration info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>` |<span data-ttu-id="e2565-181">Konfigurace typů: základní site, hdfs-site, mapred-site, hive-site</span><span class="sxs-lookup"><span data-stu-id="e2565-181">Config types: core-site, hdfs-site, mapred-site, hive-site</span></span> |

## <a name="next-steps"></a><span data-ttu-id="e2565-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e2565-182">Next Steps</span></span>
<span data-ttu-id="e2565-183">Nyní jste se naučili použití Ambari monitorování volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e2565-183">Now you have learned how to use Ambari monitoring API calls.</span></span> <span data-ttu-id="e2565-184">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="e2565-184">To learn more, see:</span></span>

* <span data-ttu-id="e2565-185">[Správa clusterů HDInsight pomocí portálu Azure][hdinsight-admin-portal]</span><span class="sxs-lookup"><span data-stu-id="e2565-185">[Manage HDInsight clusters using the Azure portal][hdinsight-admin-portal]</span></span>
* <span data-ttu-id="e2565-186">[Správa clusterů HDInsight pomocí prostředí Azure PowerShell][hdinsight-admin-powershell]</span><span class="sxs-lookup"><span data-stu-id="e2565-186">[Manage HDInsight clusters using Azure PowerShell][hdinsight-admin-powershell]</span></span>
* <span data-ttu-id="e2565-187">[Správa clusterů HDInsight pomocí rozhraní příkazového řádku][hdinsight-admin-cli]</span><span class="sxs-lookup"><span data-stu-id="e2565-187">[Manage HDInsight clusters using command-line interface][hdinsight-admin-cli]</span></span>
* <span data-ttu-id="e2565-188">[Dokumentace prostředí HDInsight][hdinsight-documentation]</span><span class="sxs-lookup"><span data-stu-id="e2565-188">[HDInsight documentation][hdinsight-documentation]</span></span>
* <span data-ttu-id="e2565-189">[Začínáme s HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="e2565-189">[Get started with HDInsight][hdinsight-get-started]</span></span>

[ambari-home]: http://ambari.apache.org/
[ambari-api-reference]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[microsoft-hadoop-SDK]: http://hadoopsdk.codeplex.com/wikipage?title=Ambari%20Monitoring%20Client

[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-documentation]: https://docs.microsoft.com/azure/hdinsight/
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md

[img-jobtracker-output]: ./media/hdinsight-monitor-use-ambari-api/hdi.ambari.monitor.jobtracker.output.png
