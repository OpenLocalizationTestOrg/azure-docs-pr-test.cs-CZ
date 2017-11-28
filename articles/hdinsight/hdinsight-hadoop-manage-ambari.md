---
title: "Sledování a správě Azure HDInsight pomocí Ambari webového uživatelského rozhraní | Microsoft Docs"
description: "Další informace o použití Ambari ke sledování a správě clusterů HDInsight se systémem Linux. V tomto dokumentu zjistěte, jak pomocí uživatelského rozhraní Ambari Web součástí clusterů HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 4787f3cc-a650-4dc3-9d96-a19a67aad046
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: dc0f9ff030f70985dad0f3b74ba0ee3dda1d9f4b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="manage-hdinsight-clusters-by-using-the-ambari-web-ui"></a><span data-ttu-id="d964f-104">Správa clusterů HDInsight pomocí Ambari webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="d964f-104">Manage HDInsight clusters by using the Ambari Web UI</span></span>

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

<span data-ttu-id="d964f-105">Apache Ambari zjednodušuje správu a sledování clusteru Hadoop tím, že poskytuje snadno použít webového uživatelského rozhraní a rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="d964f-105">Apache Ambari simplifies the management and monitoring of a Hadoop cluster by providing an easy to use web UI and REST API.</span></span> <span data-ttu-id="d964f-106">Ambari je zahrnuta v clusterech HDInsight se systémem Linux a slouží ke sledování clusteru a udělat změny konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d964f-106">Ambari is included on Linux-based HDInsight clusters, and is used to monitor the cluster and make configuration changes.</span></span>

<span data-ttu-id="d964f-107">V tomto dokumentu zjistěte, jak pomocí uživatelského rozhraní Ambari Web pomocí clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d964f-107">In this document, you learn how to use the Ambari Web UI with an HDInsight cluster.</span></span>

## <span data-ttu-id="d964f-108"><a id="whatis"></a>Co je Ambari?</span><span class="sxs-lookup"><span data-stu-id="d964f-108"><a id="whatis"></a>What is Ambari?</span></span>

<span data-ttu-id="d964f-109">[Apache Ambari](http://ambari.apache.org) zjednodušuje správu Hadoop, tím, že poskytuje snadno použitelné webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d964f-109">[Apache Ambari](http://ambari.apache.org) simplifies Hadoop management by providing an easy-to-use web UI.</span></span> <span data-ttu-id="d964f-110">Můžete použít Ambari vytvářet, spravovat a monitorovat clusterů systému Hadoop.</span><span class="sxs-lookup"><span data-stu-id="d964f-110">You can use Ambari create, manage, and monitor Hadoop clusters.</span></span> <span data-ttu-id="d964f-111">Vývojářům můžete integrovat tyto funkce do svých aplikací pomocí [Ambari REST API](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span><span class="sxs-lookup"><span data-stu-id="d964f-111">Developers can integrate these capabilities into their applications by using the [Ambari REST APIs](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span></span>

<span data-ttu-id="d964f-112">Webové uživatelské rozhraní Ambari je dostupné ve výchozím nastavení s clustery HDInsight, které používají operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="d964f-112">The Ambari Web UI is provided by default with HDInsight clusters that use the Linux operating system.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d964f-113">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="d964f-113">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="d964f-114">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="d964f-114">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> 

## <a name="connectivity"></a><span data-ttu-id="d964f-115">Připojení</span><span class="sxs-lookup"><span data-stu-id="d964f-115">Connectivity</span></span>

<span data-ttu-id="d964f-116">Webové uživatelské rozhraní Ambari je k dispozici v clusteru HDInsight na HTTPS://CLUSTERNAME.azurehdidnsight.net, kde **CLUSTERNAME** je název clusteru.</span><span class="sxs-lookup"><span data-stu-id="d964f-116">The Ambari Web UI is available on your HDInsight cluster at HTTPS://CLUSTERNAME.azurehdidnsight.net, where **CLUSTERNAME** is the name of your cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d964f-117">Připojení k Ambari v HDInsight vyžaduje protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d964f-117">Connecting to Ambari on HDInsight requires HTTPS.</span></span> <span data-ttu-id="d964f-118">Po zobrazení výzvy k ověření, použijte název účtu správce a heslo, které jste zadali při vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="d964f-118">When prompted for authentication, use the admin account name and password you provided when the cluster was created.</span></span>

## <a name="ssh-tunnel-proxy"></a><span data-ttu-id="d964f-119">Tunelové propojení SSH (proxy)</span><span class="sxs-lookup"><span data-stu-id="d964f-119">SSH tunnel (proxy)</span></span>

<span data-ttu-id="d964f-120">Při Ambari pro váš cluster je přístupná přímo přes Internet, nejsou některé odkazy z webové uživatelské rozhraní Ambari, (například za účelem jako JobTracker) zveřejněné na Internetu.</span><span class="sxs-lookup"><span data-stu-id="d964f-120">While Ambari for your cluster is accessible directly over the Internet, some links from the Ambari Web UI (such as to the JobTracker) are not exposed on the internet.</span></span> <span data-ttu-id="d964f-121">Pro přístup k těmto službám, je nutné vytvořit tunelového propojení SSH.</span><span class="sxs-lookup"><span data-stu-id="d964f-121">To access these services, you must create an SSH tunnel.</span></span> <span data-ttu-id="d964f-122">Další informace najdete v tématu [používání tunelového propojení SSH s HDInsight](hdinsight-linux-ambari-ssh-tunnel.md).</span><span class="sxs-lookup"><span data-stu-id="d964f-122">For more information, see [Use SSH Tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md).</span></span>

## <a name="ambari-web-ui"></a><span data-ttu-id="d964f-123">Webovému uživatelskému rozhraní Ambari</span><span class="sxs-lookup"><span data-stu-id="d964f-123">Ambari Web UI</span></span>

<span data-ttu-id="d964f-124">Při připojování k webové uživatelské rozhraní Ambari, zobrazí se výzva k ověření na stránku.</span><span class="sxs-lookup"><span data-stu-id="d964f-124">When connecting to the Ambari Web UI, you are prompted to authenticate to the page.</span></span> <span data-ttu-id="d964f-125">Pomocí Správce clusteru (výchozí správce) a heslo použité při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="d964f-125">Use the cluster admin user (default Admin) and password you used during cluster creation.</span></span>

<span data-ttu-id="d964f-126">Po otevření stránky, poznamenejte si panelu nahoře.</span><span class="sxs-lookup"><span data-stu-id="d964f-126">When the page opens, note the bar at the top.</span></span> <span data-ttu-id="d964f-127">Tento panel obsahuje následující informace a ovládací prvky:</span><span class="sxs-lookup"><span data-stu-id="d964f-127">This bar contains the following information and controls:</span></span>

![ambari nav](./media/hdinsight-hadoop-manage-ambari/ambari-nav.png)

* <span data-ttu-id="d964f-129">**Ambari logo** -otevře řídicí panel, který můžete použít ke sledování clusteru.</span><span class="sxs-lookup"><span data-stu-id="d964f-129">**Ambari logo** - Opens the dashboard, which can be used to monitor the cluster.</span></span>

* <span data-ttu-id="d964f-130">**Název ops # clusteru** -zobrazí počet probíhajících operacích Ambari.</span><span class="sxs-lookup"><span data-stu-id="d964f-130">**Cluster name # ops** - Displays the number of ongoing Ambari operations.</span></span> <span data-ttu-id="d964f-131">Výběr názvu clusteru nebo **# ops** zobrazí seznam operace na pozadí.</span><span class="sxs-lookup"><span data-stu-id="d964f-131">Selecting the cluster name or **# ops** displays a list of background operations.</span></span>

* <span data-ttu-id="d964f-132">**výstrahy #** -zobrazí varování nebo kritické výstrahy, pokud existuje, které pro cluster.</span><span class="sxs-lookup"><span data-stu-id="d964f-132">**# alerts** - Displays warnings or critical alerts, if any, for the cluster.</span></span>

* <span data-ttu-id="d964f-133">**Řídicí panel** -zobrazí řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="d964f-133">**Dashboard** - Displays the dashboard.</span></span>

* <span data-ttu-id="d964f-134">**Služby** – informace a nastavení konfigurace pro služby v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d964f-134">**Services** - Information and configuration settings for the services in the cluster.</span></span>

* <span data-ttu-id="d964f-135">**Hostitelé** – informace a konfigurace nastavení pro uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d964f-135">**Hosts** - Information and configuration settings for the nodes in the cluster.</span></span>

* <span data-ttu-id="d964f-136">**Výstrahy** -protokolu informace, upozornění a kritické výstrahy.</span><span class="sxs-lookup"><span data-stu-id="d964f-136">**Alerts** - A log of information, warnings, and critical alerts.</span></span>

* <span data-ttu-id="d964f-137">**Správce** -softwaru zásobníku nebo služby, které jsou nainstalované v clusteru, informace o účtu služby a zabezpečení protokolu Kerberos.</span><span class="sxs-lookup"><span data-stu-id="d964f-137">**Admin** - Software stack/services that are installed on the cluster, service account information, and Kerberos security.</span></span>

* <span data-ttu-id="d964f-138">**Tlačítko Správce** -Ambari správy, uživatelská nastavení a odhlášení.</span><span class="sxs-lookup"><span data-stu-id="d964f-138">**Admin button** - Ambari management, user settings, and logout.</span></span>

## <a name="monitoring"></a><span data-ttu-id="d964f-139">Monitorování</span><span class="sxs-lookup"><span data-stu-id="d964f-139">Monitoring</span></span>

### <a name="alerts"></a><span data-ttu-id="d964f-140">Výstrahy</span><span class="sxs-lookup"><span data-stu-id="d964f-140">Alerts</span></span>

<span data-ttu-id="d964f-141">Následující seznam obsahuje společné výstrahy stavy, které jsou používané Ambari:</span><span class="sxs-lookup"><span data-stu-id="d964f-141">The following list contains the common alert statuses used by Ambari:</span></span>

* <span data-ttu-id="d964f-142">**OK**</span><span class="sxs-lookup"><span data-stu-id="d964f-142">**OK**</span></span>
* <span data-ttu-id="d964f-143">**Upozornění**</span><span class="sxs-lookup"><span data-stu-id="d964f-143">**Warning**</span></span>
* <span data-ttu-id="d964f-144">**KRITICKÉ**</span><span class="sxs-lookup"><span data-stu-id="d964f-144">**CRITICAL**</span></span>
* <span data-ttu-id="d964f-145">**NEZNÁMÝ**</span><span class="sxs-lookup"><span data-stu-id="d964f-145">**UNKNOWN**</span></span>

<span data-ttu-id="d964f-146">Výstrahy jiné než **OK** způsobit, že **# výstrahy** položku v horní části stránky a zobrazuje počet výstrah.</span><span class="sxs-lookup"><span data-stu-id="d964f-146">Alerts other than **OK** cause the **# alerts** entry at the top of the page to display the number of alerts.</span></span> <span data-ttu-id="d964f-147">Výběrem této položky zobrazí výstrahy a jejich stav.</span><span class="sxs-lookup"><span data-stu-id="d964f-147">Selecting this entry displays the alerts and their status.</span></span>

<span data-ttu-id="d964f-148">Výstrahy jsou uspořádány do několik výchozích skupin, které si můžete prohlížet **výstrahy** stránky.</span><span class="sxs-lookup"><span data-stu-id="d964f-148">Alerts are organized into several default groups, which can be viewed from the **Alerts** page.</span></span>

![stránka výstrah](./media/hdinsight-hadoop-manage-ambari/alerts.png)

<span data-ttu-id="d964f-150">Skupiny můžete spravovat pomocí **akce** nabídky a výběrem **Spravovat výstrahy skupiny**.</span><span class="sxs-lookup"><span data-stu-id="d964f-150">You can manage the groups by using the **Actions** menu and selecting **Manage Alert Groups**.</span></span>

![Dialogové okno upozornění skupin spravovat](./media/hdinsight-hadoop-manage-ambari/manage-alerts.png)

<span data-ttu-id="d964f-152">Můžete také spravovat výstrahy metody a vytvořit oznámení o výstrahách z **akce** nabídky výběrem __Spravovat výstrahy oznámení__.</span><span class="sxs-lookup"><span data-stu-id="d964f-152">You can also manage alerting methods, and create alert notifications from the **Actions** menu by selecting __Manage Alert Notifications__.</span></span> <span data-ttu-id="d964f-153">Zobrazí se všechny aktuální oznámení.</span><span class="sxs-lookup"><span data-stu-id="d964f-153">Any current notifications are displayed.</span></span> <span data-ttu-id="d964f-154">Odsud můžete také vytvořit oznámení.</span><span class="sxs-lookup"><span data-stu-id="d964f-154">You can also create notifications from here.</span></span> <span data-ttu-id="d964f-155">Může být zasláno oznámení **e-MAILU** nebo **SNMP** dojít, když konkrétní závažnost výstrahy nebo kombinace.</span><span class="sxs-lookup"><span data-stu-id="d964f-155">Notifications can be sent via **EMAIL** or **SNMP** when specific alert/severity combinations occur.</span></span> <span data-ttu-id="d964f-156">Například můžete odesílat e-mailové zprávy při jejich výstrahy v **YARN výchozí** skupina nastavení **kritický**.</span><span class="sxs-lookup"><span data-stu-id="d964f-156">For example, you can send an email message when any of the alerts in the **YARN Default** group is set to **Critical**.</span></span>

![Vytvoření dialogového okna výstrah](./media/hdinsight-hadoop-manage-ambari/create-alert-notification.png)

<span data-ttu-id="d964f-158">Nakonec výběr __spravovat nastavení upozornění__ z __akce__ nabídky můžete nastavit počet opakování výstrahy se musí vyskytovat předtím, než odešle oznámení.</span><span class="sxs-lookup"><span data-stu-id="d964f-158">Finally, selecting __Manage Alert Settings__ from the __Actions__ menu allows you to set the number of times an alert must occur before a notification is sent.</span></span> <span data-ttu-id="d964f-159">Toto nastavení lze zabránit oznámení pro přechodné chyby.</span><span class="sxs-lookup"><span data-stu-id="d964f-159">This setting can be used to prevent notifications for transient errors.</span></span>

### <a name="cluster"></a><span data-ttu-id="d964f-160">Cluster</span><span class="sxs-lookup"><span data-stu-id="d964f-160">Cluster</span></span>

<span data-ttu-id="d964f-161">**Metriky** řídicího panelu obsahuje řadu pomůcek, které usnadňují monitorovat stav clusteru na první pohled.</span><span class="sxs-lookup"><span data-stu-id="d964f-161">The **Metrics** tab of the dashboard contains a series of widgets that make it easy to monitor the status of your cluster at a glance.</span></span> <span data-ttu-id="d964f-162">Několik widgetů, například **využití procesoru**, poskytují další informace, při kliknutí na.</span><span class="sxs-lookup"><span data-stu-id="d964f-162">Several widgets, such as **CPU Usage**, provide additional information when clicked.</span></span>

![řídicí panel se metriky](./media/hdinsight-hadoop-manage-ambari/metrics.png)

<span data-ttu-id="d964f-164">**Heatmaps** karta zobrazuje jako barevnou heatmaps, budete z zelená na červený metriky.</span><span class="sxs-lookup"><span data-stu-id="d964f-164">The **Heatmaps** tab displays metrics as colored heatmaps, going from green to red.</span></span>

![řídicí panel se heatmaps](./media/hdinsight-hadoop-manage-ambari/heatmap.png)

<span data-ttu-id="d964f-166">Další informace o uzly v clusteru, vyberte **hostitele**.</span><span class="sxs-lookup"><span data-stu-id="d964f-166">For more information on the nodes within the cluster, select **Hosts**.</span></span> <span data-ttu-id="d964f-167">Pak vyberte konkrétní uzlu, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="d964f-167">Then select the specific node you are interested in.</span></span>

![Podrobnosti o hostiteli](./media/hdinsight-hadoop-manage-ambari/host-details.png)

### <a name="services"></a><span data-ttu-id="d964f-169">Služby</span><span class="sxs-lookup"><span data-stu-id="d964f-169">Services</span></span>

<span data-ttu-id="d964f-170">**Služby** bočním panelu na řídicí panel poskytuje rychlý přehled o stavu služeb spuštěných v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d964f-170">The **Services** sidebar on the dashboard provides quick insight into the status of the services running on the cluster.</span></span> <span data-ttu-id="d964f-171">Jsou různé ikony slouží k určení stavu nebo akce, které by měla být provedena.</span><span class="sxs-lookup"><span data-stu-id="d964f-171">Various icons are used to indicate status or actions that should be taken.</span></span> <span data-ttu-id="d964f-172">Symbol žlutý recyklaci se například zobrazí, pokud služba vyžaduje provedení recyklace.</span><span class="sxs-lookup"><span data-stu-id="d964f-172">For example, a yellow recycle symbol is displayed if a service needs to be recycled.</span></span>

![straně řádku služeb](./media/hdinsight-hadoop-manage-ambari/service-bar.png)

> [!NOTE]
> <span data-ttu-id="d964f-174">Služby, zobrazí se liší mezi typy clusteru HDInsight a verze.</span><span class="sxs-lookup"><span data-stu-id="d964f-174">The services displayed differ between HDInsight cluster types and versions.</span></span> <span data-ttu-id="d964f-175">Služby zobrazí tady může být jiný než služby zobrazené pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="d964f-175">The services displayed here may be different than the services displayed for your cluster.</span></span>

<span data-ttu-id="d964f-176">Výběr služby zobrazí podrobnější informace ve službě.</span><span class="sxs-lookup"><span data-stu-id="d964f-176">Selecting a service displays more detailed information on the service.</span></span>

![souhrnné informace o služby](./media/hdinsight-hadoop-manage-ambari/service-details.png)

#### <a name="quick-links"></a><span data-ttu-id="d964f-178">Rychlé odkazy</span><span class="sxs-lookup"><span data-stu-id="d964f-178">Quick links</span></span>

<span data-ttu-id="d964f-179">Zobrazení některé služby **rychlé odkazy** odkaz v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="d964f-179">Some services display a **Quick Links** link at the top of the page.</span></span> <span data-ttu-id="d964f-180">To lze použít pro přístup k webové specifickou pro službu uživatelská rozhraní, jako například:</span><span class="sxs-lookup"><span data-stu-id="d964f-180">This can be used to access service-specific web UIs, such as:</span></span>

* <span data-ttu-id="d964f-181">**Historie úlohy** -historie úlohy MapReduce.</span><span class="sxs-lookup"><span data-stu-id="d964f-181">**Job History** - MapReduce job history.</span></span>
* <span data-ttu-id="d964f-182">**Správce prostředků** -YARN ResourceManager uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d964f-182">**Resource Manager** - YARN ResourceManager UI.</span></span>
* <span data-ttu-id="d964f-183">**NameNode** -Hadoop Distributed soubor System (HDFS) NameNode uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d964f-183">**NameNode** - Hadoop Distributed File System (HDFS) NameNode UI.</span></span>
* <span data-ttu-id="d964f-184">**Uživatelské rozhraní webu Oozie** -Oozie uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d964f-184">**Oozie Web UI** - Oozie UI.</span></span>

<span data-ttu-id="d964f-185">V prohlížeči, který zobrazí stránku vybrané výběrem libovolné z těchto odkazů otevře novou kartu.</span><span class="sxs-lookup"><span data-stu-id="d964f-185">Selecting any of these links opens a new tab in your browser, which displays the selected page.</span></span>

> [!NOTE]
> <span data-ttu-id="d964f-186">Výběr **rychlé odkazy** záznam pro službu může vrátit chyba "server nebyl nalezen".</span><span class="sxs-lookup"><span data-stu-id="d964f-186">Selecting the **Quick Links** entry for a service may return a "server not found" error.</span></span> <span data-ttu-id="d964f-187">Pokud dojde k této chybě, musíte použít tunelového propojení SSH při použití **rychlé odkazy** položka pro tuto službu.</span><span class="sxs-lookup"><span data-stu-id="d964f-187">If you encounter this error, you must use an SSH tunnel when using the **Quick Links** entry for this service.</span></span> <span data-ttu-id="d964f-188">Informace najdete v tématu [používání tunelového propojení SSH s HDInsight](hdinsight-linux-ambari-ssh-tunnel.md)</span><span class="sxs-lookup"><span data-stu-id="d964f-188">For information, see [Use SSH Tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md)</span></span>

## <a name="management"></a><span data-ttu-id="d964f-189">Správa</span><span class="sxs-lookup"><span data-stu-id="d964f-189">Management</span></span>

### <a name="ambari-users-groups-and-permissions"></a><span data-ttu-id="d964f-190">Ambari uživatele, skupiny a oprávnění</span><span class="sxs-lookup"><span data-stu-id="d964f-190">Ambari users, groups, and permissions</span></span>

<span data-ttu-id="d964f-191">Práce se uživatelé, skupiny a oprávnění jsou podporovány při použití [připojený k doméně](hdinsight-domain-joined-introduction.md) clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d964f-191">Working with users, groups, and permissions are supported when using a [domain joined](hdinsight-domain-joined-introduction.md) HDInsight cluster.</span></span> <span data-ttu-id="d964f-192">Informace o používání rozhraní Ambari správy na cluster připojený k doméně, najdete v části [Správa clusterů HDInsight připojený k doméně](hdinsight-domain-joined-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d964f-192">For information on using the Ambari Management UI on a domain-joined cluster, see [Manage domain-joined HDInsight clusters](hdinsight-domain-joined-introduction.md).</span></span>

> [!WARNING]
> <span data-ttu-id="d964f-193">Neměňte heslo Ambari sledovací zařízení (hdinsightwatchdog) v clusteru HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="d964f-193">Do not change the password of the Ambari watchdog (hdinsightwatchdog) on your Linux-based HDInsight cluster.</span></span> <span data-ttu-id="d964f-194">Změna hesla se dělí možnost pomocí skriptových akcí nebo provádět operace škálování k vašemu clusteru.</span><span class="sxs-lookup"><span data-stu-id="d964f-194">Changing the password breaks the ability to use script actions or perform scaling operations with your cluster.</span></span>

### <a name="hosts"></a><span data-ttu-id="d964f-195">Hostitelé</span><span class="sxs-lookup"><span data-stu-id="d964f-195">Hosts</span></span>

<span data-ttu-id="d964f-196">**Hostitele** stránka obsahuje seznam všech hostitelích v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d964f-196">The **Hosts** page lists all hosts in the cluster.</span></span> <span data-ttu-id="d964f-197">Ke správě hostitelů, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="d964f-197">To manage hosts, follow these steps.</span></span>

![stránka hostitele](./media/hdinsight-hadoop-manage-ambari/hosts.png)

> [!NOTE]
> <span data-ttu-id="d964f-199">Přidání, vyřazení z provozu a recommissioning hostitele nepoužívejte s clustery HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d964f-199">Adding, decommissioning, and recommissioning a host should not be used with HDInsight clusters.</span></span>

1. <span data-ttu-id="d964f-200">Vyberte hostitele, který chcete spravovat.</span><span class="sxs-lookup"><span data-stu-id="d964f-200">Select the host that you wish to manage.</span></span>

2. <span data-ttu-id="d964f-201">Použití **akce** nabídce vyberte akci, kterou chcete provést:</span><span class="sxs-lookup"><span data-stu-id="d964f-201">Use the **Actions** menu to select the action that you wish to perform:</span></span>

   * <span data-ttu-id="d964f-202">**Spustit všechny součásti** – spustit všechny součásti na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="d964f-202">**Start all components** - Start all components on the host.</span></span>

   * <span data-ttu-id="d964f-203">**Zastavit všechny součásti** -ukončit všechny součásti na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="d964f-203">**Stop all components** - Stop all components on the host.</span></span>

   * <span data-ttu-id="d964f-204">**Restartujte všechny součásti** – zastavení a spuštění všech součástí na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="d964f-204">**Restart all components** - Stop and start all components on the host.</span></span>

   * <span data-ttu-id="d964f-205">**Zapnout režim údržby** -potlačí výstrah pro hostitele.</span><span class="sxs-lookup"><span data-stu-id="d964f-205">**Turn on maintenance mode** - Suppresses alerts for the host.</span></span> <span data-ttu-id="d964f-206">Tento režim by měl povolit, pokud jsou provádění akcí, které generují výstrahy.</span><span class="sxs-lookup"><span data-stu-id="d964f-206">This mode should be enabled if you are performing actions that generate alerts.</span></span> <span data-ttu-id="d964f-207">Například ukončení a opětovném spuštění služby.</span><span class="sxs-lookup"><span data-stu-id="d964f-207">For example, stopping and starting a service.</span></span>

   * <span data-ttu-id="d964f-208">**Zapnutí režimu údržby** -vrátí hostitele do normální výstrahy.</span><span class="sxs-lookup"><span data-stu-id="d964f-208">**Turn off maintenance mode** - Returns the host to normal alerting.</span></span>

   * <span data-ttu-id="d964f-209">**Zastavit** -zastaví DataNode nebo NodeManagers na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="d964f-209">**Stop** - Stops DataNode or NodeManagers on the host.</span></span>

   * <span data-ttu-id="d964f-210">**Spustit** -spustí DataNode nebo NodeManagers na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="d964f-210">**Start** - Starts DataNode or NodeManagers on the host.</span></span>

   * <span data-ttu-id="d964f-211">**Restartujte** -zastaví a spustí DataNode nebo NodeManagers na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="d964f-211">**Restart** - Stops and starts DataNode or NodeManagers on the host.</span></span>

   * <span data-ttu-id="d964f-212">**Vyřadit z provozu** – odebere hostitele z clusteru.</span><span class="sxs-lookup"><span data-stu-id="d964f-212">**Decommission** - Removes a host from the cluster.</span></span>

     > [!NOTE]
     > <span data-ttu-id="d964f-213">Nepoužívejte tuto akci v clusterech prostředí HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d964f-213">Do not use this action on HDInsight clusters.</span></span>

   * <span data-ttu-id="d964f-214">**Recommission** -přidá dříve vyřazení hostitele do clusteru.</span><span class="sxs-lookup"><span data-stu-id="d964f-214">**Recommission** - Adds a previously decommissioned host to the cluster.</span></span>

     > [!NOTE]
     > <span data-ttu-id="d964f-215">Nepoužívejte tuto akci v clusterech prostředí HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d964f-215">Do not use this action on HDInsight clusters.</span></span>

### <span data-ttu-id="d964f-216"><a id="service"></a>Služby</span><span class="sxs-lookup"><span data-stu-id="d964f-216"><a id="service"></a>Services</span></span>

<span data-ttu-id="d964f-217">Z **řídicí panel** nebo **služby** stránky, použijte **akce** tlačítko v dolní části seznamu služeb zastavení a spuštění všech služeb.</span><span class="sxs-lookup"><span data-stu-id="d964f-217">From the **Dashboard** or **Services** page, use the **Actions** button at the bottom of the list of services to stop and start all services.</span></span>

![Akce služby](./media/hdinsight-hadoop-manage-ambari/service-actions.png)

> [!WARNING]
> <span data-ttu-id="d964f-219">Při **přidat službu** je uveden v této nabídce neměl by být použit na přidání služeb na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d964f-219">While **Add Service** is listed in this menu, it should not be used to add services to the HDInsight cluster.</span></span> <span data-ttu-id="d964f-220">Nové služby musí být přidaní pomocí akce skriptu při zřizování clusteru.</span><span class="sxs-lookup"><span data-stu-id="d964f-220">New services should be added using a Script Action during cluster provisioning.</span></span> <span data-ttu-id="d964f-221">Další informace o použití akce skriptu najdete v tématu [HDInsight přizpůsobit clustery pomocí akcí skriptů](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="d964f-221">For more information on using Script Actions, see [Customize HDInsight clusters using Script Actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

<span data-ttu-id="d964f-222">Když **akce** tlačítko můžete restartovat všechny služby, často chcete spustit, zastavit nebo restartovat určitou službu.</span><span class="sxs-lookup"><span data-stu-id="d964f-222">While the **Actions** button can restart all services, often you want to start, stop, or restart a specific service.</span></span> <span data-ttu-id="d964f-223">Použijte následující postup k provádění akcí na jednotlivé služby:</span><span class="sxs-lookup"><span data-stu-id="d964f-223">Use the following steps to perform actions on an individual service:</span></span>

1. <span data-ttu-id="d964f-224">Z **řídicí panel** nebo **služby** vyberte možnost služby.</span><span class="sxs-lookup"><span data-stu-id="d964f-224">From the **Dashboard** or **Services** page, select a service.</span></span>

2. <span data-ttu-id="d964f-225">Z horní části **Souhrn** pomocí **služby akce** tlačítko a vyberte akci provést.</span><span class="sxs-lookup"><span data-stu-id="d964f-225">From the top of the **Summary** tab, use the **Service Actions** button and select the action to take.</span></span> <span data-ttu-id="d964f-226">To restartuje službu na všech uzlech.</span><span class="sxs-lookup"><span data-stu-id="d964f-226">This restarts the service on all nodes.</span></span>

    ![Akce služby](./media/hdinsight-hadoop-manage-ambari/individual-service-actions.png)

   > [!NOTE]
   > <span data-ttu-id="d964f-228">Restartování některé služby spuštěného clusteru může generovat výstrahy.</span><span class="sxs-lookup"><span data-stu-id="d964f-228">Restarting some services while the cluster is running may generate alerts.</span></span> <span data-ttu-id="d964f-229">Abyste se vyhnuli výstrahy, můžete použít **služby akce** tlačítko umožníte **režimu údržby** pro službu před provedením restartování.</span><span class="sxs-lookup"><span data-stu-id="d964f-229">To avoid alerts, you can use the **Service Actions** button to enable **Maintenance mode** for the service before performing the restart.</span></span>

3. <span data-ttu-id="d964f-230">Jakmile akci byla vybrána, **# op** položku v horní části stránky přírůstcích zobrazíte probíhající operaci na pozadí.</span><span class="sxs-lookup"><span data-stu-id="d964f-230">Once an action has been selected, the **# op** entry at the top of the page increments to show that a background operation is occurring.</span></span> <span data-ttu-id="d964f-231">Pokud nakonfigurované pro zobrazení, zobrazí se seznam operace na pozadí.</span><span class="sxs-lookup"><span data-stu-id="d964f-231">If configured to display, the list of background operations is displayed.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d964f-232">Pokud jste povolili **režimu údržby** pro službu, nezapomeňte ho vypnout pomocí **služby akce** tlačítko po dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="d964f-232">If you enabled **Maintenance mode** for the service, remember to disable it by using the **Service Actions** button once the operation has finished.</span></span>

<span data-ttu-id="d964f-233">Ke konfiguraci služby, použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d964f-233">To configure a service, use the following steps:</span></span>

1. <span data-ttu-id="d964f-234">Z **řídicí panel** nebo **služby** vyberte možnost služby.</span><span class="sxs-lookup"><span data-stu-id="d964f-234">From the **Dashboard** or **Services** page, select a service.</span></span>

2. <span data-ttu-id="d964f-235">Vyberte **konfigurací** kartě.</span><span class="sxs-lookup"><span data-stu-id="d964f-235">Select the **Configs** tab.</span></span> <span data-ttu-id="d964f-236">Zobrazí se aktuální konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d964f-236">The current configuration is displayed.</span></span> <span data-ttu-id="d964f-237">Zobrazí se také seznam předchozí konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d964f-237">A list of previous configurations is also displayed.</span></span>

    ![Konfigurace](./media/hdinsight-hadoop-manage-ambari/service-configs.png)

3. <span data-ttu-id="d964f-239">Používání polí zobrazí konfiguraci upravit, a potom vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d964f-239">Use the fields displayed to modify the configuration, and then select **Save**.</span></span> <span data-ttu-id="d964f-240">Nebo vyberte předchozí konfiguraci a pak vyberte **změnit na aktuální** se vrátit zpět předchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="d964f-240">Or select a previous configuration and then select **Make current** to roll back to the previous settings.</span></span>

## <a name="ambari-views"></a><span data-ttu-id="d964f-241">Zobrazení Ambari</span><span class="sxs-lookup"><span data-stu-id="d964f-241">Ambari views</span></span>

<span data-ttu-id="d964f-242">Zobrazení Ambari umožňují vývojářům připojte prvky uživatelského rozhraní pomocí webové uživatelské rozhraní Ambari [Framework zobrazení Ambari](https://cwiki.apache.org/confluence/display/AMBARI/Views).</span><span class="sxs-lookup"><span data-stu-id="d964f-242">Ambari Views allow developers to plug UI elements into the Ambari Web UI using the [Ambari Views Framework](https://cwiki.apache.org/confluence/display/AMBARI/Views).</span></span> <span data-ttu-id="d964f-243">HDInsight poskytuje následující zobrazení s typy clusteru Hadoop:</span><span class="sxs-lookup"><span data-stu-id="d964f-243">HDInsight provides the following views with Hadoop cluster types:</span></span>

* <span data-ttu-id="d964f-244">Správce front yarn: správce front poskytuje jednoduché uživatelského rozhraní pro zobrazení a úprava fronty YARN.</span><span class="sxs-lookup"><span data-stu-id="d964f-244">Yarn Queue Manager: The queue manager provides a simple UI for viewing and modifying YARN queues.</span></span>

* <span data-ttu-id="d964f-245">Zobrazení Hive: Zobrazení Hive umožňuje spouštět dotazy Hive přímo z webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="d964f-245">Hive View: The Hive View allows you to run Hive queries directly from your web browser.</span></span> <span data-ttu-id="d964f-246">Můžete uložit dotazy, zobrazení výsledků, uložte výsledky do úložiště clusteru nebo stáhnout výsledky do místního systému.</span><span class="sxs-lookup"><span data-stu-id="d964f-246">You can save queries, view results, save results to the cluster storage, or download results to your local system.</span></span> <span data-ttu-id="d964f-247">Další informace o používání Hive zobrazení najdete v tématu [použijte zobrazení Hive s HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).</span><span class="sxs-lookup"><span data-stu-id="d964f-247">For more information on using Hive Views, see [Use Hive Views with HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).</span></span>

* <span data-ttu-id="d964f-248">Zobrazení tez: Tez zobrazení umožňuje lépe pochopit a optimalizovat úlohy.</span><span class="sxs-lookup"><span data-stu-id="d964f-248">Tez View: The Tez View allows you to better understand and optimize jobs.</span></span> <span data-ttu-id="d964f-249">Můžete zobrazit informace na tom, jak jsou vykonány úlohách Tez a jaké prostředky jsou používány.</span><span class="sxs-lookup"><span data-stu-id="d964f-249">You can view information on how Tez jobs are executed and what resources are used.</span></span>
