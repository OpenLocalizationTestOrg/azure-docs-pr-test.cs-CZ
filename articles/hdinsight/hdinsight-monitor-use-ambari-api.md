---
title: "aaaMonitor clusterů systému Hadoop v HDInsight pomocí hello Ambari API - Azure | Microsoft Docs"
description: "Použijte hello Apache Ambari API pro vytvoření, správě a monitoringu clusterů systému Hadoop. Operátor intuitivní nástroje a rozhraní API skrýt hello složitost systému Hadoop."
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
ms.openlocfilehash: d61a8aae5ddfcd7d44f2e4cc899e0a4da5e5fdcc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-hadoop-clusters-in-hdinsight-using-hello-ambari-api"></a><span data-ttu-id="10abf-104">Monitorování clusterů systému Hadoop v HDInsight pomocí Ambari API hello</span><span class="sxs-lookup"><span data-stu-id="10abf-104">Monitor Hadoop clusters in HDInsight using hello Ambari API</span></span>
<span data-ttu-id="10abf-105">Zjistěte, jak toomonitor HDInsight clustery pomocí rozhraní Ambari API.</span><span class="sxs-lookup"><span data-stu-id="10abf-105">Learn how toomonitor HDInsight clusters by using Ambari APIs.</span></span>

> [!NOTE]
> <span data-ttu-id="10abf-106">Hello informace v tomto článku je určen pro clustery HDInsight se systémem Windows, které poskytují jen pro čtení verzi hello Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="10abf-106">hello information in this article is primarily for Windows-based HDInsight clusters, which provide a read-only version of hello Ambari REST API.</span></span> <span data-ttu-id="10abf-107">V clusterech se systémem Linux naleznete v části [clusterů systému Hadoop spravovat pomocí nástroje Ambari](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="10abf-107">For Linux-based clusters, see [Manage Hadoop clusters using Ambari](hdinsight-hadoop-manage-ambari.md).</span></span>
> 
> 

## <a name="what-is-ambari"></a><span data-ttu-id="10abf-108">Co je Ambari?</span><span class="sxs-lookup"><span data-stu-id="10abf-108">What is Ambari?</span></span>
<span data-ttu-id="10abf-109">[Apache Ambari] [ ambari-home] se používá pro zřizování, správě a monitoringu clusterů systému Apache Hadoop.</span><span class="sxs-lookup"><span data-stu-id="10abf-109">[Apache Ambari][ambari-home] is used for provisioning, managing, and monitoring Apache Hadoop clusters.</span></span> <span data-ttu-id="10abf-110">Obsahuje intuitivní nástrojů pro operátory a výkonnou sadu rozhraní API, které překonávají složitost hello systému Hadoop a zjednodušují operace hello s clustery.</span><span class="sxs-lookup"><span data-stu-id="10abf-110">It includes an intuitive collection of operator tools and a robust set of APIs that hide hello complexity of Hadoop, simplifying hello operation of clusters.</span></span> <span data-ttu-id="10abf-111">Další informace o hello rozhraní API najdete v tématu [referenční dokumentace rozhraní API Ambari][ambari-api-reference].</span><span class="sxs-lookup"><span data-stu-id="10abf-111">For more information about hello APIs, see [Ambari API Reference][ambari-api-reference].</span></span> 

<span data-ttu-id="10abf-112">HDInsight aktuálně podporuje pouze hello Ambari funkce monitorování.</span><span class="sxs-lookup"><span data-stu-id="10abf-112">HDInsight currently supports only hello Ambari monitoring feature.</span></span> <span data-ttu-id="10abf-113">Clustery HDInsight verze 3.0 a 2.1 podporuje Ambari API 1.0.</span><span class="sxs-lookup"><span data-stu-id="10abf-113">Ambari API 1.0 is supported by HDInsight version 3.0 and 2.1 clusters.</span></span> <span data-ttu-id="10abf-114">Tento článek se týká přístupu k rozhraní API Ambari v clusterech HDInsight verze 3.1 nebo 2.1.</span><span class="sxs-lookup"><span data-stu-id="10abf-114">This article covers accessing Ambari APIs on HDInsight version 3.1 and 2.1 clusters.</span></span> <span data-ttu-id="10abf-115">Hello spočívá hlavní rozdíl mezi hello dva je, že některé součásti hello mít změněn s hello zavedením nové funkce (například hello Server historie úloh).</span><span class="sxs-lookup"><span data-stu-id="10abf-115">hello key difference between hello two is that some of hello components have changed with hello introduction of new capabilities (such as hello Job History Server).</span></span> 

<span data-ttu-id="10abf-116">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="10abf-116">**Prerequisites**</span></span>

<span data-ttu-id="10abf-117">Než začnete tento kurz, musíte mít hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="10abf-117">Before you begin this tutorial, you must have hello following items:</span></span>

* <span data-ttu-id="10abf-118">**Pracovní stanice s prostředím Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="10abf-118">**A workstation with Azure PowerShell**.</span></span>
* <span data-ttu-id="10abf-119">(Volitelné) [cURL][curl].</span><span class="sxs-lookup"><span data-stu-id="10abf-119">(Optional) [cURL][curl].</span></span> <span data-ttu-id="10abf-120">tooinstall, najdete v části [cURL verze a soubory ke stažení][curl-download].</span><span class="sxs-lookup"><span data-stu-id="10abf-120">tooinstall it, see [cURL Releases and Downloads][curl-download].</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="10abf-121">Když použijte příkaz cURL hello v systému Windows, použijte dvojité uvozovky značky místo jednoduchých uvozovek pro hodnoty možnosti hello.</span><span class="sxs-lookup"><span data-stu-id="10abf-121">When use hello cURL command in Windows, use double-quotation marks instead of single-quotation marks for hello option values.</span></span>
  > 
  > 
* <span data-ttu-id="10abf-122">**Cluster Azure HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="10abf-122">**An Azure HDInsight cluster**.</span></span> <span data-ttu-id="10abf-123">Pokyny o zřizování clusteru najdete v tématu [začněte používat HDInsight] [ hdinsight-get-started] nebo [zřizování clusterů HDInsight][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="10abf-123">For instructions about cluster provisioning, see [Get started using HDInsight][hdinsight-get-started] or [Provision HDInsight clusters][hdinsight-provision].</span></span> <span data-ttu-id="10abf-124">Hello následující toogo data prostřednictvím hello kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="10abf-124">You need hello following data toogo through hello tutorial:</span></span>
  
  | <span data-ttu-id="10abf-125">Vlastnost clusteru</span><span class="sxs-lookup"><span data-stu-id="10abf-125">Cluster property</span></span> | <span data-ttu-id="10abf-126">Název proměnné Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="10abf-126">Azure PowerShell variable name</span></span> | <span data-ttu-id="10abf-127">Hodnota</span><span class="sxs-lookup"><span data-stu-id="10abf-127">Value</span></span> | <span data-ttu-id="10abf-128">Popis</span><span class="sxs-lookup"><span data-stu-id="10abf-128">Description</span></span> |
  | --- | --- | --- | --- |
  |   <span data-ttu-id="10abf-129">Název clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="10abf-129">HDInsight cluster name</span></span> |<span data-ttu-id="10abf-130">$clusterName</span><span class="sxs-lookup"><span data-stu-id="10abf-130">$clusterName</span></span> | |<span data-ttu-id="10abf-131">Hello název clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="10abf-131">hello name of your HDInsight cluster.</span></span> |
  |   <span data-ttu-id="10abf-132">Uživatelské jméno clusteru</span><span class="sxs-lookup"><span data-stu-id="10abf-132">Cluster username</span></span> |<span data-ttu-id="10abf-133">$clusterUsername</span><span class="sxs-lookup"><span data-stu-id="10abf-133">$clusterUsername</span></span> | |<span data-ttu-id="10abf-134">Vytvoření clusteru hello zadané clusteru uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="10abf-134">Cluster user name specified when hello cluster was created.</span></span> |
  |   <span data-ttu-id="10abf-135">Heslo clusteru</span><span class="sxs-lookup"><span data-stu-id="10abf-135">Cluster password</span></span> |<span data-ttu-id="10abf-136">$clusterPassword</span><span class="sxs-lookup"><span data-stu-id="10abf-136">$clusterPassword</span></span> | |<span data-ttu-id="10abf-137">Heslo uživatele clusteru.</span><span class="sxs-lookup"><span data-stu-id="10abf-137">Cluster user password.</span></span> |

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


## <a name="jump-start"></a><span data-ttu-id="10abf-138">Rychle začít s prací</span><span class="sxs-lookup"><span data-stu-id="10abf-138">Jump-start</span></span>
<span data-ttu-id="10abf-139">Existuje několik způsobů toouse Ambari toomonitor HDInsight clustery.</span><span class="sxs-lookup"><span data-stu-id="10abf-139">There are several ways toouse Ambari toomonitor HDInsight clusters.</span></span>

<span data-ttu-id="10abf-140">**Použití Azure Powershellu**</span><span class="sxs-lookup"><span data-stu-id="10abf-140">**Use Azure PowerShell**</span></span>

<span data-ttu-id="10abf-141">Hello následující skript prostředí Azure PowerShell získá hello informací o sledování úloh MapReduce *v clusteru služby HDInsight 3.5.*</span><span class="sxs-lookup"><span data-stu-id="10abf-141">hello following Azure PowerShell script gets hello MapReduce job tracker information *in an HDInsight 3.5 cluster.*</span></span>  <span data-ttu-id="10abf-142">Hello klíčovým rozdílem je, že jsme pro vyžádání obsahu tyto podrobnosti ze služby YARN hello (nikoli MapReduce).</span><span class="sxs-lookup"><span data-stu-id="10abf-142">hello key difference is that we pull these details from hello YARN service (rather than MapReduce).</span></span>

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName/services/YARN/components/RESOURCEMANAGER"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

<span data-ttu-id="10abf-143">Následující skript prostředí PowerShell Hello získá hello informací o sledování úloh MapReduce *v clusteru služby HDInsight 2.1*:</span><span class="sxs-lookup"><span data-stu-id="10abf-143">hello following PowerShell script gets hello MapReduce job tracker information *in an HDInsight 2.1 cluster*:</span></span>

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

<span data-ttu-id="10abf-144">výstup Hello je:</span><span class="sxs-lookup"><span data-stu-id="10abf-144">hello output is:</span></span>

![Výstup Jobtracker][img-jobtracker-output]

<span data-ttu-id="10abf-146">**Použití cURL**</span><span class="sxs-lookup"><span data-stu-id="10abf-146">**Use cURL**</span></span>

<span data-ttu-id="10abf-147">Hello následující příklad získá informace o clusteru pomocí cURL:</span><span class="sxs-lookup"><span data-stu-id="10abf-147">hello following example gets cluster information by using cURL:</span></span>

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

<span data-ttu-id="10abf-148">výstup Hello je:</span><span class="sxs-lookup"><span data-stu-id="10abf-148">hello output is:</span></span>

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

<span data-ttu-id="10abf-149">**Pro verzi 8/10/2014 hello**:</span><span class="sxs-lookup"><span data-stu-id="10abf-149">**For hello 10/8/2014 release**:</span></span>

<span data-ttu-id="10abf-150">Při použití hello Ambari koncový bod, "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}" hello *název_hostitele* pole Vrátí hello plně kvalifikovaný název domény (FQDN) uzlu hello místo názvu hostitele hello.</span><span class="sxs-lookup"><span data-stu-id="10abf-150">When using hello Ambari endpoint, "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", hello *host_name* field returns hello fully qualified domain name (FQDN) of hello node instead of hello host name.</span></span> <span data-ttu-id="10abf-151">Před hello verze 8/10/2014, v tomto příkladu vrátil jednoduše "**headnode0**".</span><span class="sxs-lookup"><span data-stu-id="10abf-151">Before hello 10/8/2014 release, this example returned simply "**headnode0**".</span></span> <span data-ttu-id="10abf-152">Po vydání hello 8/10/2014, získáte hello plně kvalifikovaný název domény "**headnode0. { ClusterDNS} gt; .azurehdinsight .net**", jak je uvedeno v předchozím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="10abf-152">After hello 10/8/2014 release, you get hello FQDN "**headnode0.{ClusterDNS}.azurehdinsight.net**", as shown in hello previous example.</span></span> <span data-ttu-id="10abf-153">Tato změna byla požadovaná toofacilitate scénáře, kde lze nasadit více typů clusteru (například HBase a Hadoop) v jednu virtuální síť (VNET).</span><span class="sxs-lookup"><span data-stu-id="10abf-153">This change was required toofacilitate scenarios where multiple cluster types (such as HBase and Hadoop) can be deployed in one virtual network (VNET).</span></span> <span data-ttu-id="10abf-154">K tomu dojde, například při použití HBase jako back-end platformy pro Hadoop.</span><span class="sxs-lookup"><span data-stu-id="10abf-154">This happens, for example, when using HBase as a back-end platform for Hadoop.</span></span>

## <a name="ambari-monitoring-apis"></a><span data-ttu-id="10abf-155">Ambari monitorování rozhraní API</span><span class="sxs-lookup"><span data-stu-id="10abf-155">Ambari monitoring APIs</span></span>
<span data-ttu-id="10abf-156">Hello následující tabulka uvádí některé z hello nejběžnější Ambari monitorování volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="10abf-156">hello following table lists some of hello most common Ambari monitoring API calls.</span></span> <span data-ttu-id="10abf-157">Další informace o hello rozhraní API najdete v tématu [referenční dokumentace rozhraní API Ambari][ambari-api-reference].</span><span class="sxs-lookup"><span data-stu-id="10abf-157">For more information about hello API, see [Ambari API Reference][ambari-api-reference].</span></span>

| <span data-ttu-id="10abf-158">Volání rozhraní API monitorování</span><span class="sxs-lookup"><span data-stu-id="10abf-158">Monitor API call</span></span> | <span data-ttu-id="10abf-159">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="10abf-159">URI</span></span> | <span data-ttu-id="10abf-160">Popis</span><span class="sxs-lookup"><span data-stu-id="10abf-160">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="10abf-161">Získat clustery</span><span class="sxs-lookup"><span data-stu-id="10abf-161">Get clusters</span></span> |`/api/v1/clusters` | |
| <span data-ttu-id="10abf-162">Získání informací o clusteru.</span><span class="sxs-lookup"><span data-stu-id="10abf-162">Get cluster info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net` |<span data-ttu-id="10abf-163">clustery, služby, hostitelé</span><span class="sxs-lookup"><span data-stu-id="10abf-163">clusters, services, hosts</span></span> |
| <span data-ttu-id="10abf-164">Získat služby</span><span class="sxs-lookup"><span data-stu-id="10abf-164">Get services</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services` |<span data-ttu-id="10abf-165">Mezi služby patří: hdfs, mapreduce</span><span class="sxs-lookup"><span data-stu-id="10abf-165">Services include: hdfs, mapreduce</span></span> |
| <span data-ttu-id="10abf-166">Získání informací o služby.</span><span class="sxs-lookup"><span data-stu-id="10abf-166">Get services info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>` | |
| <span data-ttu-id="10abf-167">Získat součásti služby</span><span class="sxs-lookup"><span data-stu-id="10abf-167">Get service components</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components` |<span data-ttu-id="10abf-168">HDFS: namenode, datanodeMapReduce: jobtracker; tasktracker</span><span class="sxs-lookup"><span data-stu-id="10abf-168">HDFS: namenode, datanodeMapReduce: jobtracker; tasktracker</span></span> |
| <span data-ttu-id="10abf-169">Získání informací o součásti.</span><span class="sxs-lookup"><span data-stu-id="10abf-169">Get component info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>` |<span data-ttu-id="10abf-170">ServiceComponentInfo, metriky, hostitele součásti</span><span class="sxs-lookup"><span data-stu-id="10abf-170">ServiceComponentInfo, host-components, metrics</span></span> |
| <span data-ttu-id="10abf-171">Získat hostitele</span><span class="sxs-lookup"><span data-stu-id="10abf-171">Get hosts</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts` |<span data-ttu-id="10abf-172">headnode0 workernode0</span><span class="sxs-lookup"><span data-stu-id="10abf-172">headnode0, workernode0</span></span> |
| <span data-ttu-id="10abf-173">Získání informací o hostiteli.</span><span class="sxs-lookup"><span data-stu-id="10abf-173">Get host info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>` | |
| <span data-ttu-id="10abf-174">Získat komponenty hostitele</span><span class="sxs-lookup"><span data-stu-id="10abf-174">Get host components</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components` |<span data-ttu-id="10abf-175">namenode, resourcemanager</span><span class="sxs-lookup"><span data-stu-id="10abf-175">namenode, resourcemanager</span></span> |
| <span data-ttu-id="10abf-176">Získáte informace o součást hostitele.</span><span class="sxs-lookup"><span data-stu-id="10abf-176">Get host component info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>` |<span data-ttu-id="10abf-177">HostRoles, součást, hostitele, metriky</span><span class="sxs-lookup"><span data-stu-id="10abf-177">HostRoles, component, host, metrics</span></span> |
| <span data-ttu-id="10abf-178">Získání konfigurace</span><span class="sxs-lookup"><span data-stu-id="10abf-178">Get configurations</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations` |<span data-ttu-id="10abf-179">Konfigurace typů: základní site, hdfs-site, mapred-site, hive-site</span><span class="sxs-lookup"><span data-stu-id="10abf-179">Config types: core-site, hdfs-site, mapred-site, hive-site</span></span> |
| <span data-ttu-id="10abf-180">Získáte informace o konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="10abf-180">Get configuration info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>` |<span data-ttu-id="10abf-181">Konfigurace typů: základní site, hdfs-site, mapred-site, hive-site</span><span class="sxs-lookup"><span data-stu-id="10abf-181">Config types: core-site, hdfs-site, mapred-site, hive-site</span></span> |

## <a name="next-steps"></a><span data-ttu-id="10abf-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="10abf-182">Next Steps</span></span>
<span data-ttu-id="10abf-183">Nyní jste se naučili, jak se volá toouse Ambari API monitorování.</span><span class="sxs-lookup"><span data-stu-id="10abf-183">Now you have learned how toouse Ambari monitoring API calls.</span></span> <span data-ttu-id="10abf-184">toolearn více, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="10abf-184">toolearn more, see:</span></span>

* <span data-ttu-id="10abf-185">[Správa clusterů HDInsight pomocí hello portálu Azure][hdinsight-admin-portal]</span><span class="sxs-lookup"><span data-stu-id="10abf-185">[Manage HDInsight clusters using hello Azure portal][hdinsight-admin-portal]</span></span>
* <span data-ttu-id="10abf-186">[Správa clusterů HDInsight pomocí prostředí Azure PowerShell][hdinsight-admin-powershell]</span><span class="sxs-lookup"><span data-stu-id="10abf-186">[Manage HDInsight clusters using Azure PowerShell][hdinsight-admin-powershell]</span></span>
* <span data-ttu-id="10abf-187">[Správa clusterů HDInsight pomocí rozhraní příkazového řádku][hdinsight-admin-cli]</span><span class="sxs-lookup"><span data-stu-id="10abf-187">[Manage HDInsight clusters using command-line interface][hdinsight-admin-cli]</span></span>
* <span data-ttu-id="10abf-188">[Dokumentace prostředí HDInsight][hdinsight-documentation]</span><span class="sxs-lookup"><span data-stu-id="10abf-188">[HDInsight documentation][hdinsight-documentation]</span></span>
* <span data-ttu-id="10abf-189">[Začínáme s HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="10abf-189">[Get started with HDInsight][hdinsight-get-started]</span></span>

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
