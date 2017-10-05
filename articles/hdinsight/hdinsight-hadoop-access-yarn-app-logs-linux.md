---
title: "Protokoly aplikací Hadoop YARN přístup na HDInsight se systémem Linux - Azure | Microsoft Docs"
description: "Zjistěte, jak pro přístup k protokoly YARN aplikací na clusteru HDInsight se systémem Linux (Hadoop) pomocí příkazového řádku a webový prohlížeč."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 3ec08d20-4f19-4a8e-ac86-639c04d2f12e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: fbbbddc47f24a46eac9bc64d4420ee8429ed4ad1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="access-yarn-application-logs-on-linux-based-hdinsight"></a><span data-ttu-id="2b581-103">Protokoly YARN aplikace přístup na HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="2b581-103">Access YARN application logs on Linux-based HDInsight</span></span>

<span data-ttu-id="2b581-104">Zjistěte, jak získat přístup v protokolech aplikací YARN (ještě jiný prostředek Vyjednavač) v clusteru Hadoop v prostředí Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="2b581-104">Learn how to access the logs for YARN (Yet Another Resource Negotiator) applications on a Hadoop cluster in Azure HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2b581-105">Kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux.</span><span class="sxs-lookup"><span data-stu-id="2b581-105">The steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="2b581-106">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="2b581-106">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="2b581-107">Další informace najdete v tématu [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="2b581-107">For more information, see [HDInsight component versioning](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="2b581-108"><a name="YARNTimelineServer"></a>YARN časová osa serveru</span><span class="sxs-lookup"><span data-stu-id="2b581-108"><a name="YARNTimelineServer"></a>YARN Timeline Server</span></span>

<span data-ttu-id="2b581-109">[YARN časová osa serveru](http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html) poskytuje obecné informace o dokončené aplikace a informace o konkrétní rozhraní aplikaci prostřednictvím dvou různých rozhraní.</span><span class="sxs-lookup"><span data-stu-id="2b581-109">The [YARN Timeline Server](http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html) provides generic information on completed applications and framework-specific application information through two different interfaces.</span></span> <span data-ttu-id="2b581-110">Zejména:</span><span class="sxs-lookup"><span data-stu-id="2b581-110">Specifically:</span></span>

* <span data-ttu-id="2b581-111">Ukládání a načítání informací o obecná aplikace v clusterech prostředí HDInsight byl povolený s verzí 3.1.1.374 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="2b581-111">Storage and retrieval of generic application information on HDInsight clusters has been enabled with version 3.1.1.374 or higher.</span></span>
* <span data-ttu-id="2b581-112">Součást aplikace konkrétní rozhraní informace časová osa serveru není aktuálně k dispozici v clusterech prostředí HDInsight.</span><span class="sxs-lookup"><span data-stu-id="2b581-112">The framework-specific application information component of the Timeline Server is not currently available on HDInsight clusters.</span></span>

<span data-ttu-id="2b581-113">Obecné informace o aplikacích v patří následující typy dat:</span><span class="sxs-lookup"><span data-stu-id="2b581-113">Generic information on applications includes the following type of data:</span></span>

* <span data-ttu-id="2b581-114">ID aplikace, jedinečný identifikátor aplikace</span><span class="sxs-lookup"><span data-stu-id="2b581-114">The application ID, a unique identifier of an application</span></span>
* <span data-ttu-id="2b581-115">Uživatel, který aplikaci</span><span class="sxs-lookup"><span data-stu-id="2b581-115">The user who started the application</span></span>
* <span data-ttu-id="2b581-116">Informace o dokončení aplikace provedených pokusů</span><span class="sxs-lookup"><span data-stu-id="2b581-116">Information on attempts made to complete the application</span></span>
* <span data-ttu-id="2b581-117">Kontejnery používané jakýkoliv pokus o dané aplikaci</span><span class="sxs-lookup"><span data-stu-id="2b581-117">The containers used by any given application attempt</span></span>

## <span data-ttu-id="2b581-118"><a name="YARNAppsAndLogs"></a>Protokoly YARN aplikací a</span><span class="sxs-lookup"><span data-stu-id="2b581-118"><a name="YARNAppsAndLogs"></a>YARN applications and logs</span></span>

<span data-ttu-id="2b581-119">YARN podporuje více programovacích modelů (MapReduce právě jeden z nich) tím, že odpojí Správa prostředků z plánování/monitorování aplikací.</span><span class="sxs-lookup"><span data-stu-id="2b581-119">YARN supports multiple programming models (MapReduce being one of them) by decoupling resource management from application scheduling/monitoring.</span></span> <span data-ttu-id="2b581-120">YARN používá globální konfiguraci *ResourceManager* (RM) uzlu na pracovním *NodeManagers* (NMs) a každou aplikaci *ApplicationMasters* (AMs).</span><span class="sxs-lookup"><span data-stu-id="2b581-120">YARN uses a global *ResourceManager* (RM), per-worker-node *NodeManagers* (NMs), and per-application *ApplicationMasters* (AMs).</span></span> <span data-ttu-id="2b581-121">Každou aplikaci AM vyjedná prostředků (procesoru, paměti, disku, sítě) pro spuštění aplikace s RM.</span><span class="sxs-lookup"><span data-stu-id="2b581-121">The per-application AM negotiates resources (CPU, memory, disk, network) for running your application with the RM.</span></span> <span data-ttu-id="2b581-122">Správce prostředků funguje s NMs udělení těchto prostředků, které jsou poskytovány jako *kontejnery*.</span><span class="sxs-lookup"><span data-stu-id="2b581-122">The RM works with NMs to grant these resources, which are granted as *containers*.</span></span> <span data-ttu-id="2b581-123">AM zodpovídá za sledování postupu kontejnery přiřazen RM.</span><span class="sxs-lookup"><span data-stu-id="2b581-123">The AM is responsible for tracking the progress of the containers assigned to it by the RM.</span></span> <span data-ttu-id="2b581-124">Aplikace může vyžadovat mnoho kontejnerů v závislosti na povaze aplikace.</span><span class="sxs-lookup"><span data-stu-id="2b581-124">An application may require many containers depending on the nature of the application.</span></span>

<span data-ttu-id="2b581-125">Každá aplikace může obsahovat více *pokusy o aplikace*.</span><span class="sxs-lookup"><span data-stu-id="2b581-125">Each application may consist of multiple *application attempts*.</span></span> <span data-ttu-id="2b581-126">Pokud aplikace selže, může pokus jako nový pokus o.</span><span class="sxs-lookup"><span data-stu-id="2b581-126">If an application fails, it may be retried as a new attempt.</span></span> <span data-ttu-id="2b581-127">Každý pokus o spuštění v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="2b581-127">Each attempt runs in a container.</span></span> <span data-ttu-id="2b581-128">V tom smyslu kontejner poskytuje kontext pro základní jednotkou úlohy prováděné YARN aplikace.</span><span class="sxs-lookup"><span data-stu-id="2b581-128">In a sense, a container provides the context for basic unit of work performed by a YARN application.</span></span> <span data-ttu-id="2b581-129">Všechny práci, kterou se provádí v kontextu kontejneru se provádí na jednoho pracovního uzlu, na kterém byl přidělen kontejneru.</span><span class="sxs-lookup"><span data-stu-id="2b581-129">All work that is done within the context of a container is performed on the single worker node on which the container was allocated.</span></span> <span data-ttu-id="2b581-130">V tématu [YARN koncepty] [ YARN-concepts] pro odkaz na další.</span><span class="sxs-lookup"><span data-stu-id="2b581-130">See [YARN Concepts][YARN-concepts] for further reference.</span></span>

<span data-ttu-id="2b581-131">Protokoly aplikací (a protokoly přidružené kontejneru) jsou kritické v ladění problematické aplikací Hadoop.</span><span class="sxs-lookup"><span data-stu-id="2b581-131">Application logs (and the associated container logs) are critical in debugging problematic Hadoop applications.</span></span> <span data-ttu-id="2b581-132">YARN poskytuje dobrý rozhraní pro shromažďování, agregace a ukládání protokolů aplikace pomocí [protokolu agregace] [ log-aggregation] funkce.</span><span class="sxs-lookup"><span data-stu-id="2b581-132">YARN provides a nice framework for collecting, aggregating, and storing application logs with the [Log Aggregation][log-aggregation] feature.</span></span> <span data-ttu-id="2b581-133">Díky funkci agregace protokolu přístupem protokoly aplikací více deterministický.</span><span class="sxs-lookup"><span data-stu-id="2b581-133">The Log Aggregation feature makes accessing application logs more deterministic.</span></span> <span data-ttu-id="2b581-134">Ho slučuje protokolů v rámci všech kontejnerů v pracovním uzlu a ukládá je jako jeden soubor protokolu agregované pro pracovního uzlu.</span><span class="sxs-lookup"><span data-stu-id="2b581-134">It aggregates logs across all containers on a worker node and stores them as one aggregated log file per worker node.</span></span> <span data-ttu-id="2b581-135">V protokolu jsou uloženy na výchozí systém souborů po dokončení aplikace.</span><span class="sxs-lookup"><span data-stu-id="2b581-135">The log is stored on the default file system after an application finishes.</span></span> <span data-ttu-id="2b581-136">Vaše aplikace může používat stovkami nebo tisíci kontejnery, ale protokoly pro všechny kontejnery spustit na jednom pracovního uzlu agregovány vždy pro jeden soubor.</span><span class="sxs-lookup"><span data-stu-id="2b581-136">Your application may use hundreds or thousands of containers, but logs for all containers run on a single worker node are always aggregated to a single file.</span></span> <span data-ttu-id="2b581-137">Proto není pouze 1 protokolu za pracovního uzlu používá vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="2b581-137">So there is only 1 log per worker node used by your application.</span></span> <span data-ttu-id="2b581-138">Agregace protokolu je povoleno ve výchozím nastavení v clusterech HDInsight verze 3.0 nebo novějším.</span><span class="sxs-lookup"><span data-stu-id="2b581-138">Log Aggregation is enabled by default on HDInsight clusters version 3.0 and above.</span></span> <span data-ttu-id="2b581-139">Agregované protokoly jsou umístěny ve výchozím nastavení úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="2b581-139">Aggregated logs are located in default storage for the cluster.</span></span> <span data-ttu-id="2b581-140">Následující cesta je cesta HDFS do protokolů:</span><span class="sxs-lookup"><span data-stu-id="2b581-140">The following path is the HDFS path to the logs:</span></span>

    /app-logs/<user>/logs/<applicationId>

<span data-ttu-id="2b581-141">V cestě `user` je jméno uživatele, který aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2b581-141">In the path, `user` is the name of the user who started the application.</span></span> <span data-ttu-id="2b581-142">`applicationId` Je jedinečný identifikátor přiřadila YARN RM. k aplikaci</span><span class="sxs-lookup"><span data-stu-id="2b581-142">The `applicationId` is the unique identifier assigned to an application by the YARN RM.</span></span>

<span data-ttu-id="2b581-143">Agregované protokoly nejsou přímo čitelný, jako jsou zapsány [TFile][T-file], [binární formát] [ binary-format] indexovat pomocí kontejneru.</span><span class="sxs-lookup"><span data-stu-id="2b581-143">The aggregated logs are not directly readable, as they are written in a [TFile][T-file], [binary format][binary-format] indexed by container.</span></span> <span data-ttu-id="2b581-144">Chcete-li zobrazit tyto protokoly jako prostý text pro aplikace nebo kontejnerů, které vás zajímají použijte protokoly YARN ResourceManager nebo nástrojů příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="2b581-144">Use the YARN ResourceManager logs or CLI tools to view these logs as plain text for applications or containers of interest.</span></span>

## <a name="yarn-cli-tools"></a><span data-ttu-id="2b581-145">YARN rozhraní příkazového řádku nástroje</span><span class="sxs-lookup"><span data-stu-id="2b581-145">YARN CLI tools</span></span>

<span data-ttu-id="2b581-146">Při použití nástroje pro YARN rozhraní příkazového řádku, je nutné se nejprve připojit ke clusteru HDInsight pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="2b581-146">To use the YARN CLI tools, you must first connect to the HDInsight cluster using SSH.</span></span> <span data-ttu-id="2b581-147">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="2b581-147">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="2b581-148">Tyto protokoly můžete zobrazit jako prostý text spuštěním jednoho z následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="2b581-148">You can view these logs as plain text by running one of the following commands:</span></span>

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>

<span data-ttu-id="2b581-149">Zadejte &lt;applicationId >, &lt;uživatel kdo spuštění--application >, &lt;identifikátor containerId >, a &lt;adresa pracovní node > informace při spuštění těchto příkazů.</span><span class="sxs-lookup"><span data-stu-id="2b581-149">Specify the &lt;applicationId>, &lt;user-who-started-the-application>, &lt;containerId>, and &lt;worker-node-address> information when running these commands.</span></span>

## <a name="yarn-resourcemanager-ui"></a><span data-ttu-id="2b581-150">YARN ResourceManager uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="2b581-150">YARN ResourceManager UI</span></span>

<span data-ttu-id="2b581-151">Rozhraní YARN ResourceManager běží na clusteru headnode.</span><span class="sxs-lookup"><span data-stu-id="2b581-151">The YARN ResourceManager UI runs on the cluster headnode.</span></span> <span data-ttu-id="2b581-152">Je přístupný prostřednictvím webového uživatelského rozhraní Ambari.</span><span class="sxs-lookup"><span data-stu-id="2b581-152">It is accessed through the Ambari web UI.</span></span> <span data-ttu-id="2b581-153">Pokud chcete zobrazit protokoly YARN použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2b581-153">Use the following steps to view the YARN logs:</span></span>

1. <span data-ttu-id="2b581-154">Ve webovém prohlížeči přejděte do https://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="2b581-154">In your web browser, navigate to https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="2b581-155">Nahraďte název clusteru s názvem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="2b581-155">Replace CLUSTERNAME with the name of your HDInsight cluster.</span></span>
2. <span data-ttu-id="2b581-156">V seznamu služeb na levé straně vyberte **YARN**.</span><span class="sxs-lookup"><span data-stu-id="2b581-156">From the list of services on the left, select **YARN**.</span></span>

    ![Yarn služby vybrané](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnservice.png)
3. <span data-ttu-id="2b581-158">Z **rychlé odkazy** rozevíracího seznamu, vyberte jeden z hlavních uzlech clusteru a pak vyberte **ResourceManager protokolu**.</span><span class="sxs-lookup"><span data-stu-id="2b581-158">From the **Quick Links** dropdown, select one of the cluster head nodes and then select **ResourceManager Log**.</span></span>

    ![Yarn rychlé odkazy](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnquicklinks.png)

    <span data-ttu-id="2b581-160">Zobrazí se seznam odkazů na protokoly YARN.</span><span class="sxs-lookup"><span data-stu-id="2b581-160">You are presented with a list of links to YARN logs.</span></span>

[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
