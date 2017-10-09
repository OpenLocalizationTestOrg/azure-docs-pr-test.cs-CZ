---
title: "aaaMonitor a spravovat Azure HDInsight pomocí Ambari webového uživatelského rozhraní | Microsoft Docs"
description: "Zjistěte, jak toouse Ambari toomonitor a Správa clusterů HDInsight se systémem Linux. V tomto dokumentu zjistíte, jak toouse hello webové uživatelské rozhraní Ambari součástí HDInsight clustery."
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
ms.openlocfilehash: d422c40e63345d7054839a625e115c50dad040f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hdinsight-clusters-by-using-hello-ambari-web-ui"></a><span data-ttu-id="1de64-104">Správa clusterů HDInsight pomocí hello webové uživatelské rozhraní Ambari</span><span class="sxs-lookup"><span data-stu-id="1de64-104">Manage HDInsight clusters by using hello Ambari Web UI</span></span>

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

<span data-ttu-id="1de64-105">Apache Ambari zjednodušuje hello Správa a sledování clusteru Hadoop tím, že poskytuje snadno toouse webového uživatelského rozhraní a REST API.</span><span class="sxs-lookup"><span data-stu-id="1de64-105">Apache Ambari simplifies hello management and monitoring of a Hadoop cluster by providing an easy toouse web UI and REST API.</span></span> <span data-ttu-id="1de64-106">Ambari je zahrnutý v clusterech HDInsight se systémem Linux a je použité toomonitor hello clusteru a ujistěte se změny konfigurace.</span><span class="sxs-lookup"><span data-stu-id="1de64-106">Ambari is included on Linux-based HDInsight clusters, and is used toomonitor hello cluster and make configuration changes.</span></span>

<span data-ttu-id="1de64-107">V tomto dokumentu zjistíte, jak toouse hello webové uživatelské rozhraní Ambari pomocí clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1de64-107">In this document, you learn how toouse hello Ambari Web UI with an HDInsight cluster.</span></span>

## <span data-ttu-id="1de64-108"><a id="whatis"></a>Co je Ambari?</span><span class="sxs-lookup"><span data-stu-id="1de64-108"><a id="whatis"></a>What is Ambari?</span></span>

<span data-ttu-id="1de64-109">[Apache Ambari](http://ambari.apache.org) zjednodušuje správu Hadoop, tím, že poskytuje snadno použitelné webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1de64-109">[Apache Ambari](http://ambari.apache.org) simplifies Hadoop management by providing an easy-to-use web UI.</span></span> <span data-ttu-id="1de64-110">Můžete použít Ambari vytvářet, spravovat a monitorovat clusterů systému Hadoop.</span><span class="sxs-lookup"><span data-stu-id="1de64-110">You can use Ambari create, manage, and monitor Hadoop clusters.</span></span> <span data-ttu-id="1de64-111">Vývojářům můžete integrovat tyto funkce do svých aplikací s použitím hello [Ambari REST API](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span><span class="sxs-lookup"><span data-stu-id="1de64-111">Developers can integrate these capabilities into their applications by using hello [Ambari REST APIs](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span></span>

<span data-ttu-id="1de64-112">Hello webové uživatelské rozhraní Ambari je dostupné ve výchozím nastavení s clustery HDInsight, které používají operační systém Linux hello.</span><span class="sxs-lookup"><span data-stu-id="1de64-112">hello Ambari Web UI is provided by default with HDInsight clusters that use hello Linux operating system.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1de64-113">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="1de64-113">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="1de64-114">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="1de64-114">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> 

## <a name="connectivity"></a><span data-ttu-id="1de64-115">Připojení</span><span class="sxs-lookup"><span data-stu-id="1de64-115">Connectivity</span></span>

<span data-ttu-id="1de64-116">Hello webové uživatelské rozhraní Ambari je k dispozici v clusteru HDInsight na HTTPS://CLUSTERNAME.azurehdidnsight.net, kde **CLUSTERNAME** je hello názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="1de64-116">hello Ambari Web UI is available on your HDInsight cluster at HTTPS://CLUSTERNAME.azurehdidnsight.net, where **CLUSTERNAME** is hello name of your cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1de64-117">Připojení tooAmbari v HDInsight vyžaduje protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1de64-117">Connecting tooAmbari on HDInsight requires HTTPS.</span></span> <span data-ttu-id="1de64-118">Po zobrazení výzvy k ověření, použijte název účtu správce hello a heslo, které jste zadali při vytvoření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="1de64-118">When prompted for authentication, use hello admin account name and password you provided when hello cluster was created.</span></span>

## <a name="ssh-tunnel-proxy"></a><span data-ttu-id="1de64-119">Tunelové propojení SSH (proxy)</span><span class="sxs-lookup"><span data-stu-id="1de64-119">SSH tunnel (proxy)</span></span>

<span data-ttu-id="1de64-120">Při Ambari pro váš cluster je přístupný přímo prostřednictvím hello Internet, hello některé odkazy z hello webové uživatelské rozhraní Ambari (například toohello JobTracker) se nezobrazí na Internetu.</span><span class="sxs-lookup"><span data-stu-id="1de64-120">While Ambari for your cluster is accessible directly over hello Internet, some links from hello Ambari Web UI (such as toohello JobTracker) are not exposed on hello internet.</span></span> <span data-ttu-id="1de64-121">tooaccess těchto služeb, je nutné vytvořit tunelového propojení SSH.</span><span class="sxs-lookup"><span data-stu-id="1de64-121">tooaccess these services, you must create an SSH tunnel.</span></span> <span data-ttu-id="1de64-122">Další informace najdete v tématu [používání tunelového propojení SSH s HDInsight](hdinsight-linux-ambari-ssh-tunnel.md).</span><span class="sxs-lookup"><span data-stu-id="1de64-122">For more information, see [Use SSH Tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md).</span></span>

## <a name="ambari-web-ui"></a><span data-ttu-id="1de64-123">Webovému uživatelskému rozhraní Ambari</span><span class="sxs-lookup"><span data-stu-id="1de64-123">Ambari Web UI</span></span>

<span data-ttu-id="1de64-124">Při připojování toohello webové uživatelské rozhraní Ambari, jste výzvami tooauthenticate toohello stránky.</span><span class="sxs-lookup"><span data-stu-id="1de64-124">When connecting toohello Ambari Web UI, you are prompted tooauthenticate toohello page.</span></span> <span data-ttu-id="1de64-125">Použijte hello uživatel s oprávněními správce clusteru (výchozí správce) a heslo použité při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="1de64-125">Use hello cluster admin user (default Admin) and password you used during cluster creation.</span></span>

<span data-ttu-id="1de64-126">Po otevření stránky hello Poznámka: hello panelu v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="1de64-126">When hello page opens, note hello bar at hello top.</span></span> <span data-ttu-id="1de64-127">Tento panel obsahuje hello následující informace a ovládací prvky:</span><span class="sxs-lookup"><span data-stu-id="1de64-127">This bar contains hello following information and controls:</span></span>

![ambari nav](./media/hdinsight-hadoop-manage-ambari/ambari-nav.png)

* <span data-ttu-id="1de64-129">**Ambari logo** – řídicí panel hello otevře, který lze použít toomonitor hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="1de64-129">**Ambari logo** - Opens hello dashboard, which can be used toomonitor hello cluster.</span></span>

* <span data-ttu-id="1de64-130">**Název ops # clusteru** -zobrazí hello počet probíhajících operacích Ambari.</span><span class="sxs-lookup"><span data-stu-id="1de64-130">**Cluster name # ops** - Displays hello number of ongoing Ambari operations.</span></span> <span data-ttu-id="1de64-131">Výběr názvu clusteru hello nebo **# ops** zobrazí seznam operace na pozadí.</span><span class="sxs-lookup"><span data-stu-id="1de64-131">Selecting hello cluster name or **# ops** displays a list of background operations.</span></span>

* <span data-ttu-id="1de64-132">**výstrahy #** -zobrazí varování nebo kritické výstrahy, pokud existuje, které pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="1de64-132">**# alerts** - Displays warnings or critical alerts, if any, for hello cluster.</span></span>

* <span data-ttu-id="1de64-133">**Řídicí panel** -zobrazí hello řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="1de64-133">**Dashboard** - Displays hello dashboard.</span></span>

* <span data-ttu-id="1de64-134">**Služby** – informace a konfigurace nastavení pro hello služby v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="1de64-134">**Services** - Information and configuration settings for hello services in hello cluster.</span></span>

* <span data-ttu-id="1de64-135">**Hostitelé** – informace a konfigurace nastavení pro hello uzly v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="1de64-135">**Hosts** - Information and configuration settings for hello nodes in hello cluster.</span></span>

* <span data-ttu-id="1de64-136">**Výstrahy** -protokolu informace, upozornění a kritické výstrahy.</span><span class="sxs-lookup"><span data-stu-id="1de64-136">**Alerts** - A log of information, warnings, and critical alerts.</span></span>

* <span data-ttu-id="1de64-137">**Správce** -softwaru zásobníku nebo služby, které jsou nainstalované na hello clusteru, informace o účtu služby a zabezpečení protokolu Kerberos.</span><span class="sxs-lookup"><span data-stu-id="1de64-137">**Admin** - Software stack/services that are installed on hello cluster, service account information, and Kerberos security.</span></span>

* <span data-ttu-id="1de64-138">**Tlačítko Správce** -Ambari správy, uživatelská nastavení a odhlášení.</span><span class="sxs-lookup"><span data-stu-id="1de64-138">**Admin button** - Ambari management, user settings, and logout.</span></span>

## <a name="monitoring"></a><span data-ttu-id="1de64-139">Monitorování</span><span class="sxs-lookup"><span data-stu-id="1de64-139">Monitoring</span></span>

### <a name="alerts"></a><span data-ttu-id="1de64-140">Výstrahy</span><span class="sxs-lookup"><span data-stu-id="1de64-140">Alerts</span></span>

<span data-ttu-id="1de64-141">Hello následující seznam obsahuje společné výstrahy stavy hello používá Ambari:</span><span class="sxs-lookup"><span data-stu-id="1de64-141">hello following list contains hello common alert statuses used by Ambari:</span></span>

* <span data-ttu-id="1de64-142">**OK**</span><span class="sxs-lookup"><span data-stu-id="1de64-142">**OK**</span></span>
* <span data-ttu-id="1de64-143">**Upozornění**</span><span class="sxs-lookup"><span data-stu-id="1de64-143">**Warning**</span></span>
* <span data-ttu-id="1de64-144">**KRITICKÉ**</span><span class="sxs-lookup"><span data-stu-id="1de64-144">**CRITICAL**</span></span>
* <span data-ttu-id="1de64-145">**NEZNÁMÝ**</span><span class="sxs-lookup"><span data-stu-id="1de64-145">**UNKNOWN**</span></span>

<span data-ttu-id="1de64-146">Výstrahy jiné než **OK** způsobit hello **# výstrahy** položku v horní části hello hello stránky toodisplay hello počtu výstrah.</span><span class="sxs-lookup"><span data-stu-id="1de64-146">Alerts other than **OK** cause hello **# alerts** entry at hello top of hello page toodisplay hello number of alerts.</span></span> <span data-ttu-id="1de64-147">Výběrem této položky zobrazí hello výstrahy a jejich stav.</span><span class="sxs-lookup"><span data-stu-id="1de64-147">Selecting this entry displays hello alerts and their status.</span></span>

<span data-ttu-id="1de64-148">Výstrahy jsou uspořádány do několik výchozích skupin, které lze zobrazit z hello **výstrahy** stránky.</span><span class="sxs-lookup"><span data-stu-id="1de64-148">Alerts are organized into several default groups, which can be viewed from hello **Alerts** page.</span></span>

![stránka výstrah](./media/hdinsight-hadoop-manage-ambari/alerts.png)

<span data-ttu-id="1de64-150">Hello skupin můžete spravovat pomocí hello **akce** nabídky a výběrem **Spravovat výstrahy skupiny**.</span><span class="sxs-lookup"><span data-stu-id="1de64-150">You can manage hello groups by using hello **Actions** menu and selecting **Manage Alert Groups**.</span></span>

![Dialogové okno upozornění skupin spravovat](./media/hdinsight-hadoop-manage-ambari/manage-alerts.png)

<span data-ttu-id="1de64-152">Můžete také spravovat výstrahy metody a vytvořit oznámení o výstrahách z hello **akce** nabídky výběrem __Spravovat výstrahy oznámení__.</span><span class="sxs-lookup"><span data-stu-id="1de64-152">You can also manage alerting methods, and create alert notifications from hello **Actions** menu by selecting __Manage Alert Notifications__.</span></span> <span data-ttu-id="1de64-153">Zobrazí se všechny aktuální oznámení.</span><span class="sxs-lookup"><span data-stu-id="1de64-153">Any current notifications are displayed.</span></span> <span data-ttu-id="1de64-154">Odsud můžete také vytvořit oznámení.</span><span class="sxs-lookup"><span data-stu-id="1de64-154">You can also create notifications from here.</span></span> <span data-ttu-id="1de64-155">Může být zasláno oznámení **e-MAILU** nebo **SNMP** dojít, když konkrétní závažnost výstrahy nebo kombinace.</span><span class="sxs-lookup"><span data-stu-id="1de64-155">Notifications can be sent via **EMAIL** or **SNMP** when specific alert/severity combinations occur.</span></span> <span data-ttu-id="1de64-156">Například můžete odesílat e-mailové zprávy při jejich hello výstrahy v hello **YARN výchozí** skupiny je nastaven příliš**kritický**.</span><span class="sxs-lookup"><span data-stu-id="1de64-156">For example, you can send an email message when any of hello alerts in hello **YARN Default** group is set too**Critical**.</span></span>

![Vytvoření dialogového okna výstrah](./media/hdinsight-hadoop-manage-ambari/create-alert-notification.png)

<span data-ttu-id="1de64-158">Nakonec výběr __spravovat nastavení upozornění__ z hello __akce__ nabídky vám umožní tooset hello počet opakování výstrahy se musí vyskytovat předtím, než odešle oznámení.</span><span class="sxs-lookup"><span data-stu-id="1de64-158">Finally, selecting __Manage Alert Settings__ from hello __Actions__ menu allows you tooset hello number of times an alert must occur before a notification is sent.</span></span> <span data-ttu-id="1de64-159">Toto nastavení může být použité tooprevent oznámení pro přechodné chyby.</span><span class="sxs-lookup"><span data-stu-id="1de64-159">This setting can be used tooprevent notifications for transient errors.</span></span>

### <a name="cluster"></a><span data-ttu-id="1de64-160">Cluster</span><span class="sxs-lookup"><span data-stu-id="1de64-160">Cluster</span></span>

<span data-ttu-id="1de64-161">Hello **metriky** karta hello řídicí panel obsahuje řadu pomůcek, které umožňují snadno toomonitor hello stav clusteru na první pohled.</span><span class="sxs-lookup"><span data-stu-id="1de64-161">hello **Metrics** tab of hello dashboard contains a series of widgets that make it easy toomonitor hello status of your cluster at a glance.</span></span> <span data-ttu-id="1de64-162">Několik widgetů, například **využití procesoru**, poskytují další informace, při kliknutí na.</span><span class="sxs-lookup"><span data-stu-id="1de64-162">Several widgets, such as **CPU Usage**, provide additional information when clicked.</span></span>

![řídicí panel se metriky](./media/hdinsight-hadoop-manage-ambari/metrics.png)

<span data-ttu-id="1de64-164">Hello **Heatmaps** karta zobrazuje jako barevnou heatmaps, budete z zelená toored metriky.</span><span class="sxs-lookup"><span data-stu-id="1de64-164">hello **Heatmaps** tab displays metrics as colored heatmaps, going from green toored.</span></span>

![řídicí panel se heatmaps](./media/hdinsight-hadoop-manage-ambari/heatmap.png)

<span data-ttu-id="1de64-166">Další informace o hello uzlů v clusteru hello vyberte **hostitele**.</span><span class="sxs-lookup"><span data-stu-id="1de64-166">For more information on hello nodes within hello cluster, select **Hosts**.</span></span> <span data-ttu-id="1de64-167">Pak vyberte hello konkrétním uzlu, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="1de64-167">Then select hello specific node you are interested in.</span></span>

![Podrobnosti o hostiteli](./media/hdinsight-hadoop-manage-ambari/host-details.png)

### <a name="services"></a><span data-ttu-id="1de64-169">Služby</span><span class="sxs-lookup"><span data-stu-id="1de64-169">Services</span></span>

<span data-ttu-id="1de64-170">Hello **služby** bočním panelu na hello řídicí panel poskytuje rychlý přehled o stavu hello hello služeb spuštěných v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="1de64-170">hello **Services** sidebar on hello dashboard provides quick insight into hello status of hello services running on hello cluster.</span></span> <span data-ttu-id="1de64-171">Jsou různé ikony používané tooindicate stavu nebo akce, které by měla být provedena.</span><span class="sxs-lookup"><span data-stu-id="1de64-171">Various icons are used tooindicate status or actions that should be taken.</span></span> <span data-ttu-id="1de64-172">Například symbol žlutý recyklaci se zobrazí, pokud služba potřebuje toobe recyklují.</span><span class="sxs-lookup"><span data-stu-id="1de64-172">For example, a yellow recycle symbol is displayed if a service needs toobe recycled.</span></span>

![straně řádku služeb](./media/hdinsight-hadoop-manage-ambari/service-bar.png)

> [!NOTE]
> <span data-ttu-id="1de64-174">Hello služeb, zobrazí se liší mezi typy clusteru HDInsight a verze.</span><span class="sxs-lookup"><span data-stu-id="1de64-174">hello services displayed differ between HDInsight cluster types and versions.</span></span> <span data-ttu-id="1de64-175">Hello služby zobrazí tady může být jiný než services hello zobrazené u vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="1de64-175">hello services displayed here may be different than hello services displayed for your cluster.</span></span>

<span data-ttu-id="1de64-176">Výběr služby zobrazí podrobnější informace ve službě hello.</span><span class="sxs-lookup"><span data-stu-id="1de64-176">Selecting a service displays more detailed information on hello service.</span></span>

![souhrnné informace o služby](./media/hdinsight-hadoop-manage-ambari/service-details.png)

#### <a name="quick-links"></a><span data-ttu-id="1de64-178">Rychlé odkazy</span><span class="sxs-lookup"><span data-stu-id="1de64-178">Quick links</span></span>

<span data-ttu-id="1de64-179">Zobrazení některé služby **rychlé odkazy** odkaz hello horní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="1de64-179">Some services display a **Quick Links** link at hello top of hello page.</span></span> <span data-ttu-id="1de64-180">To může být použité tooaccess specifickou pro službu web uživatelská rozhraní, jako například:</span><span class="sxs-lookup"><span data-stu-id="1de64-180">This can be used tooaccess service-specific web UIs, such as:</span></span>

* <span data-ttu-id="1de64-181">**Historie úlohy** -historie úlohy MapReduce.</span><span class="sxs-lookup"><span data-stu-id="1de64-181">**Job History** - MapReduce job history.</span></span>
* <span data-ttu-id="1de64-182">**Správce prostředků** -YARN ResourceManager uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1de64-182">**Resource Manager** - YARN ResourceManager UI.</span></span>
* <span data-ttu-id="1de64-183">**NameNode** -Hadoop Distributed soubor System (HDFS) NameNode uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1de64-183">**NameNode** - Hadoop Distributed File System (HDFS) NameNode UI.</span></span>
* <span data-ttu-id="1de64-184">**Uživatelské rozhraní webu Oozie** -Oozie uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1de64-184">**Oozie Web UI** - Oozie UI.</span></span>

<span data-ttu-id="1de64-185">V prohlížeči, který zobrazí stránku hello vybrané výběrem libovolné z těchto odkazů otevře novou kartu.</span><span class="sxs-lookup"><span data-stu-id="1de64-185">Selecting any of these links opens a new tab in your browser, which displays hello selected page.</span></span>

> [!NOTE]
> <span data-ttu-id="1de64-186">Výběr hello **rychlé odkazy** záznam pro službu může vrátit chyba "server nebyl nalezen".</span><span class="sxs-lookup"><span data-stu-id="1de64-186">Selecting hello **Quick Links** entry for a service may return a "server not found" error.</span></span> <span data-ttu-id="1de64-187">Pokud dojde k této chybě, musíte použít tunelového propojení SSH při použití hello **rychlé odkazy** položka pro tuto službu.</span><span class="sxs-lookup"><span data-stu-id="1de64-187">If you encounter this error, you must use an SSH tunnel when using hello **Quick Links** entry for this service.</span></span> <span data-ttu-id="1de64-188">Informace najdete v tématu [používání tunelového propojení SSH s HDInsight](hdinsight-linux-ambari-ssh-tunnel.md)</span><span class="sxs-lookup"><span data-stu-id="1de64-188">For information, see [Use SSH Tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md)</span></span>

## <a name="management"></a><span data-ttu-id="1de64-189">Správa</span><span class="sxs-lookup"><span data-stu-id="1de64-189">Management</span></span>

### <a name="ambari-users-groups-and-permissions"></a><span data-ttu-id="1de64-190">Ambari uživatele, skupiny a oprávnění</span><span class="sxs-lookup"><span data-stu-id="1de64-190">Ambari users, groups, and permissions</span></span>

<span data-ttu-id="1de64-191">Práce se uživatelé, skupiny a oprávnění jsou podporovány při použití [připojený k doméně](hdinsight-domain-joined-introduction.md) clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1de64-191">Working with users, groups, and permissions are supported when using a [domain joined](hdinsight-domain-joined-introduction.md) HDInsight cluster.</span></span> <span data-ttu-id="1de64-192">Informace o používání hello uživatelského rozhraní Ambari správy na cluster připojený k doméně, najdete v části [Správa clusterů HDInsight připojený k doméně](hdinsight-domain-joined-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1de64-192">For information on using hello Ambari Management UI on a domain-joined cluster, see [Manage domain-joined HDInsight clusters](hdinsight-domain-joined-introduction.md).</span></span>

> [!WARNING]
> <span data-ttu-id="1de64-193">Neměňte heslo hello sledovací hello Ambari zařízení (hdinsightwatchdog) v clusteru HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="1de64-193">Do not change hello password of hello Ambari watchdog (hdinsightwatchdog) on your Linux-based HDInsight cluster.</span></span> <span data-ttu-id="1de64-194">Změna hesla zalomení hello hello možnost toouse skript akce nebo provádět operace škálování k vašemu clusteru.</span><span class="sxs-lookup"><span data-stu-id="1de64-194">Changing hello password breaks hello ability toouse script actions or perform scaling operations with your cluster.</span></span>

### <a name="hosts"></a><span data-ttu-id="1de64-195">Hostitelé</span><span class="sxs-lookup"><span data-stu-id="1de64-195">Hosts</span></span>

<span data-ttu-id="1de64-196">Hello **hostitele** stránka obsahuje seznam všech hostitelích v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="1de64-196">hello **Hosts** page lists all hosts in hello cluster.</span></span> <span data-ttu-id="1de64-197">toomanage hostitele, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="1de64-197">toomanage hosts, follow these steps.</span></span>

![stránka hostitele](./media/hdinsight-hadoop-manage-ambari/hosts.png)

> [!NOTE]
> <span data-ttu-id="1de64-199">Přidání, vyřazení z provozu a recommissioning hostitele nepoužívejte s clustery HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1de64-199">Adding, decommissioning, and recommissioning a host should not be used with HDInsight clusters.</span></span>

1. <span data-ttu-id="1de64-200">Vyberte hostitele hello chcete toomanage.</span><span class="sxs-lookup"><span data-stu-id="1de64-200">Select hello host that you wish toomanage.</span></span>

2. <span data-ttu-id="1de64-201">Použití hello **akce** nabídky tooselect hello akce chcete tooperform:</span><span class="sxs-lookup"><span data-stu-id="1de64-201">Use hello **Actions** menu tooselect hello action that you wish tooperform:</span></span>

   * <span data-ttu-id="1de64-202">**Spustit všechny součásti** – spustit všechny součásti hello hostiteli.</span><span class="sxs-lookup"><span data-stu-id="1de64-202">**Start all components** - Start all components on hello host.</span></span>

   * <span data-ttu-id="1de64-203">**Zastavit všechny součásti** -ukončit všechny součásti na hostiteli hello.</span><span class="sxs-lookup"><span data-stu-id="1de64-203">**Stop all components** - Stop all components on hello host.</span></span>

   * <span data-ttu-id="1de64-204">**Restartujte všechny součásti** – zastavení a spuštění všech součástí na hostiteli hello.</span><span class="sxs-lookup"><span data-stu-id="1de64-204">**Restart all components** - Stop and start all components on hello host.</span></span>

   * <span data-ttu-id="1de64-205">**Zapnout režim údržby** -potlačí výstrah pro hostitele hello.</span><span class="sxs-lookup"><span data-stu-id="1de64-205">**Turn on maintenance mode** - Suppresses alerts for hello host.</span></span> <span data-ttu-id="1de64-206">Tento režim by měl povolit, pokud jsou provádění akcí, které generují výstrahy.</span><span class="sxs-lookup"><span data-stu-id="1de64-206">This mode should be enabled if you are performing actions that generate alerts.</span></span> <span data-ttu-id="1de64-207">Například ukončení a opětovném spuštění služby.</span><span class="sxs-lookup"><span data-stu-id="1de64-207">For example, stopping and starting a service.</span></span>

   * <span data-ttu-id="1de64-208">**Zapnutí režimu údržby** – vrátí text hello hostitele toonormal výstrahy.</span><span class="sxs-lookup"><span data-stu-id="1de64-208">**Turn off maintenance mode** - Returns hello host toonormal alerting.</span></span>

   * <span data-ttu-id="1de64-209">**Zastavit** -zastaví DataNode nebo NodeManagers na hostiteli hello.</span><span class="sxs-lookup"><span data-stu-id="1de64-209">**Stop** - Stops DataNode or NodeManagers on hello host.</span></span>

   * <span data-ttu-id="1de64-210">**Spustit** -spustí DataNode nebo NodeManagers na hostiteli hello.</span><span class="sxs-lookup"><span data-stu-id="1de64-210">**Start** - Starts DataNode or NodeManagers on hello host.</span></span>

   * <span data-ttu-id="1de64-211">**Restartujte** -zastaví a spustí DataNode nebo NodeManagers na hostiteli hello.</span><span class="sxs-lookup"><span data-stu-id="1de64-211">**Restart** - Stops and starts DataNode or NodeManagers on hello host.</span></span>

   * <span data-ttu-id="1de64-212">**Vyřadit z provozu** – odebere hostitele z clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="1de64-212">**Decommission** - Removes a host from hello cluster.</span></span>

     > [!NOTE]
     > <span data-ttu-id="1de64-213">Nepoužívejte tuto akci v clusterech prostředí HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1de64-213">Do not use this action on HDInsight clusters.</span></span>

   * <span data-ttu-id="1de64-214">**Recommission** -přidá toohello dříve vyřazení hostitelského clusteru.</span><span class="sxs-lookup"><span data-stu-id="1de64-214">**Recommission** - Adds a previously decommissioned host toohello cluster.</span></span>

     > [!NOTE]
     > <span data-ttu-id="1de64-215">Nepoužívejte tuto akci v clusterech prostředí HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1de64-215">Do not use this action on HDInsight clusters.</span></span>

### <span data-ttu-id="1de64-216"><a id="service"></a>Služby</span><span class="sxs-lookup"><span data-stu-id="1de64-216"><a id="service"></a>Services</span></span>

<span data-ttu-id="1de64-217">Z hello **řídicí panel** nebo **služby** stránky, použijte hello **akce** tlačítko dole hello hello seznam služeb toostop a spuštění všech služeb.</span><span class="sxs-lookup"><span data-stu-id="1de64-217">From hello **Dashboard** or **Services** page, use hello **Actions** button at hello bottom of hello list of services toostop and start all services.</span></span>

![Akce služby](./media/hdinsight-hadoop-manage-ambari/service-actions.png)

> [!WARNING]
> <span data-ttu-id="1de64-219">Při **přidat službu** je uveden v této nabídce, neměl by být clusteru HDInsight toohello tooadd používaných služeb.</span><span class="sxs-lookup"><span data-stu-id="1de64-219">While **Add Service** is listed in this menu, it should not be used tooadd services toohello HDInsight cluster.</span></span> <span data-ttu-id="1de64-220">Nové služby musí být přidaní pomocí akce skriptu při zřizování clusteru.</span><span class="sxs-lookup"><span data-stu-id="1de64-220">New services should be added using a Script Action during cluster provisioning.</span></span> <span data-ttu-id="1de64-221">Další informace o použití akce skriptu najdete v tématu [HDInsight přizpůsobit clustery pomocí akcí skriptů](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="1de64-221">For more information on using Script Actions, see [Customize HDInsight clusters using Script Actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

<span data-ttu-id="1de64-222">Při hello **akce** tlačítko můžete restartovat všechny služby, často se má toostart, zastavit nebo restartovat určitou službu.</span><span class="sxs-lookup"><span data-stu-id="1de64-222">While hello **Actions** button can restart all services, often you want toostart, stop, or restart a specific service.</span></span> <span data-ttu-id="1de64-223">Použijte následující postup tooperform akce na jednotlivé služby hello:</span><span class="sxs-lookup"><span data-stu-id="1de64-223">Use hello following steps tooperform actions on an individual service:</span></span>

1. <span data-ttu-id="1de64-224">Z hello **řídicí panel** nebo **služby** vyberte možnost služby.</span><span class="sxs-lookup"><span data-stu-id="1de64-224">From hello **Dashboard** or **Services** page, select a service.</span></span>

2. <span data-ttu-id="1de64-225">Z hello horní části hello **Souhrn** pomocí hello **služby akce** tlačítkem a vyberte hello tootake akce.</span><span class="sxs-lookup"><span data-stu-id="1de64-225">From hello top of hello **Summary** tab, use hello **Service Actions** button and select hello action tootake.</span></span> <span data-ttu-id="1de64-226">Tím dojde k restartování služby hello na všech uzlech.</span><span class="sxs-lookup"><span data-stu-id="1de64-226">This restarts hello service on all nodes.</span></span>

    ![Akce služby](./media/hdinsight-hadoop-manage-ambari/individual-service-actions.png)

   > [!NOTE]
   > <span data-ttu-id="1de64-228">Restartování některé služby, když běží hello clusteru může generovat výstrahy.</span><span class="sxs-lookup"><span data-stu-id="1de64-228">Restarting some services while hello cluster is running may generate alerts.</span></span> <span data-ttu-id="1de64-229">tooavoid výstrahy, můžete použít hello **služby akce** tlačítko tooenable **režimu údržby** služby hello před provedením hello restartování.</span><span class="sxs-lookup"><span data-stu-id="1de64-229">tooavoid alerts, you can use hello **Service Actions** button tooenable **Maintenance mode** for hello service before performing hello restart.</span></span>

3. <span data-ttu-id="1de64-230">Jakmile akci byla vybrána, hello **# op** položka hello horní části hello stránky přírůstcích tooshow, ke kterému dochází operaci na pozadí.</span><span class="sxs-lookup"><span data-stu-id="1de64-230">Once an action has been selected, hello **# op** entry at hello top of hello page increments tooshow that a background operation is occurring.</span></span> <span data-ttu-id="1de64-231">Nakonfigurované toodisplay, zobrazí se seznam hello operace na pozadí.</span><span class="sxs-lookup"><span data-stu-id="1de64-231">If configured toodisplay, hello list of background operations is displayed.</span></span>

   > [!NOTE]
   > <span data-ttu-id="1de64-232">Pokud jste povolili **režimu údržby** hello služby, mějte na paměti, toodisable ho pomocí hello **služby akce** tlačítko po dokončení operace hello.</span><span class="sxs-lookup"><span data-stu-id="1de64-232">If you enabled **Maintenance mode** for hello service, remember toodisable it by using hello **Service Actions** button once hello operation has finished.</span></span>

<span data-ttu-id="1de64-233">tooconfigure služby, použijte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1de64-233">tooconfigure a service, use hello following steps:</span></span>

1. <span data-ttu-id="1de64-234">Z hello **řídicí panel** nebo **služby** vyberte možnost služby.</span><span class="sxs-lookup"><span data-stu-id="1de64-234">From hello **Dashboard** or **Services** page, select a service.</span></span>

2. <span data-ttu-id="1de64-235">Vyberte hello **konfigurací** se zobrazí kartu hello aktuální konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="1de64-235">Select hello **Configs** tab. hello current configuration is displayed.</span></span> <span data-ttu-id="1de64-236">Zobrazí se také seznam předchozí konfigurace.</span><span class="sxs-lookup"><span data-stu-id="1de64-236">A list of previous configurations is also displayed.</span></span>

    ![Konfigurace](./media/hdinsight-hadoop-manage-ambari/service-configs.png)

3. <span data-ttu-id="1de64-238">Použití hello pole zobrazí toomodify hello konfigurace a pak vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="1de64-238">Use hello fields displayed toomodify hello configuration, and then select **Save**.</span></span> <span data-ttu-id="1de64-239">Nebo vyberte předchozí konfiguraci a pak vyberte **změnit na aktuální** tooroll zpět toohello předchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="1de64-239">Or select a previous configuration and then select **Make current** tooroll back toohello previous settings.</span></span>

## <a name="ambari-views"></a><span data-ttu-id="1de64-240">Zobrazení Ambari</span><span class="sxs-lookup"><span data-stu-id="1de64-240">Ambari views</span></span>

<span data-ttu-id="1de64-241">Zobrazení Ambari umožňují vývojářům prvky uživatelského rozhraní tooplug do hello webové uživatelské rozhraní Ambari pomocí hello [Framework zobrazení Ambari](https://cwiki.apache.org/confluence/display/AMBARI/Views).</span><span class="sxs-lookup"><span data-stu-id="1de64-241">Ambari Views allow developers tooplug UI elements into hello Ambari Web UI using hello [Ambari Views Framework](https://cwiki.apache.org/confluence/display/AMBARI/Views).</span></span> <span data-ttu-id="1de64-242">HDInsight poskytuje hello následující zobrazení s typy clusteru Hadoop:</span><span class="sxs-lookup"><span data-stu-id="1de64-242">HDInsight provides hello following views with Hadoop cluster types:</span></span>

* <span data-ttu-id="1de64-243">Správce front yarn: správce front hello poskytuje jednoduché uživatelského rozhraní pro zobrazení a úprava fronty YARN.</span><span class="sxs-lookup"><span data-stu-id="1de64-243">Yarn Queue Manager: hello queue manager provides a simple UI for viewing and modifying YARN queues.</span></span>

* <span data-ttu-id="1de64-244">Zobrazení Hive: hello zobrazení Hive můžete toorun dotazů Hive přímo z webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1de64-244">Hive View: hello Hive View allows you toorun Hive queries directly from your web browser.</span></span> <span data-ttu-id="1de64-245">Můžete uložit dotazy, zobrazení výsledků, uložte výsledky úložiště clusteru toohello nebo stáhnout výsledky tooyour místní systém.</span><span class="sxs-lookup"><span data-stu-id="1de64-245">You can save queries, view results, save results toohello cluster storage, or download results tooyour local system.</span></span> <span data-ttu-id="1de64-246">Další informace o používání Hive zobrazení najdete v tématu [použijte zobrazení Hive s HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).</span><span class="sxs-lookup"><span data-stu-id="1de64-246">For more information on using Hive Views, see [Use Hive Views with HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).</span></span>

* <span data-ttu-id="1de64-247">Zobrazení tez: hello Tez zobrazení můžete toobetter pochopit a optimalizovat úlohy.</span><span class="sxs-lookup"><span data-stu-id="1de64-247">Tez View: hello Tez View allows you toobetter understand and optimize jobs.</span></span> <span data-ttu-id="1de64-248">Můžete zobrazit informace na tom, jak jsou vykonány úlohách Tez a jaké prostředky jsou používány.</span><span class="sxs-lookup"><span data-stu-id="1de64-248">You can view information on how Tez jobs are executed and what resources are used.</span></span>
