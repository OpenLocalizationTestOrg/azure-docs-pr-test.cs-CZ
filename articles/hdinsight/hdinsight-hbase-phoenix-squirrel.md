---
title: "aaaUse Apache Phoenix a SQuirreL s HDInsight se systémem Windows Azure | Microsoft Docs"
description: "Zjistěte, jak toouse Apache Phoenix v HDInsight a jak tooinstall a konfigurace SQuirreL na clusteru pracovní stanice tooconnect tooan HBase v HDInsight."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 1a756e98-75c1-44cd-a178-c5119683b7b7
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 147ac35fa882fd1bedbc5361ac804c36a4d56de1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-phoenix-and-squirrel-with-windows-based-hbase-clusters-in-hdinsight"></a><span data-ttu-id="448ce-103">Použití nástrojů Apache Phoenix a SQuirreL s clustery založené na Windows HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="448ce-103">Use Apache Phoenix and SQuirreL with Windows-based HBase clusters in HDInsight</span></span>
<span data-ttu-id="448ce-104">Zjistěte, jak toouse [Apache Phoenix](http://phoenix.apache.org/) v HDInsight a jak tooinstall a konfigurace SQuirreL na clusteru pracovní stanice tooconnect tooan HBase v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="448ce-104">Learn how toouse [Apache Phoenix](http://phoenix.apache.org/) in HDInsight, and how tooinstall and configure SQuirreL on your workstation tooconnect tooan HBase cluster in HDInsight.</span></span> <span data-ttu-id="448ce-105">Další informace o Phoenix najdete v tématu [Phoenix za 15 minut nebo méně](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html).</span><span class="sxs-lookup"><span data-stu-id="448ce-105">For more information about Phoenix, see [Phoenix in 15 minutes or less](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html).</span></span> <span data-ttu-id="448ce-106">Hello gramatika Phoenix, najdete v části [Phoenix gramatika](http://phoenix.apache.org/language/index.html).</span><span class="sxs-lookup"><span data-stu-id="448ce-106">For hello Phoenix grammar, see [Phoenix Grammar](http://phoenix.apache.org/language/index.html).</span></span>

> [!NOTE]
> <span data-ttu-id="448ce-107">Hello Phoenix informace o verzi v HDInsight, naleznete v části [co je nového ve verzích clusterů systému Hadoop hello poskytovaných v HDInsight?](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="448ce-107">For hello Phoenix version information in HDInsight, see [What's new in hello Hadoop cluster versions provided by HDInsight?](hdinsight-component-versioning.md).</span></span>
>

> [!IMPORTANT]
> <span data-ttu-id="448ce-108">Hello kroky v této dokumentu funguje jenom pro clustery HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="448ce-108">hello steps in this document only work for Windows-based HDInsight clusters.</span></span> <span data-ttu-id="448ce-109">HDInsight je k dispozici pouze v systému Windows verze nižší než HDInsight 3.4.</span><span class="sxs-lookup"><span data-stu-id="448ce-109">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="448ce-110">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="448ce-110">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="448ce-111">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="448ce-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="448ce-112">Informace o používání Phoenix na HDInsight se systémem Linux najdete v tématu [clusterů použití nástrojů Apache Phoenix se systémem Linux HBase v HDInsight](hdinsight-hbase-phoenix-squirrel-linux.md).</span><span class="sxs-lookup"><span data-stu-id="448ce-112">For information on using Phoenix on Linux-based HDInsight, see [Use Apache Phoenix with Linux-based HBase clusters in HDInsight](hdinsight-hbase-phoenix-squirrel-linux.md).</span></span>
>



## <a name="use-sqlline"></a><span data-ttu-id="448ce-113">Použití SQLLine</span><span class="sxs-lookup"><span data-stu-id="448ce-113">Use SQLLine</span></span>
<span data-ttu-id="448ce-114">[SQLLine](http://sqlline.sourceforge.net/) je tooexecute nástroj příkazového řádku SQL.</span><span class="sxs-lookup"><span data-stu-id="448ce-114">[SQLLine](http://sqlline.sourceforge.net/) is a command line utility tooexecute SQL.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="448ce-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="448ce-115">Prerequisites</span></span>
<span data-ttu-id="448ce-116">Než budete moci použít SQLLine, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="448ce-116">Before you can use SQLLine, you must have hello following:</span></span>

* <span data-ttu-id="448ce-117">**Cluster HBase v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="448ce-117">**A HBase cluster in HDInsight**.</span></span> <span data-ttu-id="448ce-118">Informace o zřizování HBase clusteru najdete v tématu [začít pracovat s Apache HBase v HDInsight][hdinsight-hbase-get-started].</span><span class="sxs-lookup"><span data-stu-id="448ce-118">For information on provision HBase cluster, see [Get started with Apache HBase in HDInsight][hdinsight-hbase-get-started].</span></span>
* <span data-ttu-id="448ce-119">**Připojit cluster HBase toohello prostřednictvím protokolu vzdálené plochy hello**.</span><span class="sxs-lookup"><span data-stu-id="448ce-119">**Connect toohello HBase cluster via hello remote desktop protocol**.</span></span> <span data-ttu-id="448ce-120">Pokyny najdete v tématu [Správa clusterů systému Hadoop v HDInsight pomocí portálu Azure Classic hello][hdinsight-manage-portal].</span><span class="sxs-lookup"><span data-stu-id="448ce-120">For instructions, see [Manage Hadoop clusters in HDInsight by using hello Azure Classic Portal][hdinsight-manage-portal].</span></span>

<span data-ttu-id="448ce-121">**toofind se název hostitele hello**</span><span class="sxs-lookup"><span data-stu-id="448ce-121">**toofind out hello host name**</span></span>

1. <span data-ttu-id="448ce-122">Otevřete **Hadoop příkazového řádku** z plochy hello.</span><span class="sxs-lookup"><span data-stu-id="448ce-122">Open **Hadoop Command Line** from hello desktop.</span></span>
2. <span data-ttu-id="448ce-123">Spusťte následující příkaz tooget hello DNS přípona hello:</span><span class="sxs-lookup"><span data-stu-id="448ce-123">Run hello following command tooget hello DNS suffix:</span></span>

        ipconfig

    <span data-ttu-id="448ce-124">Zapište **příponu DNS specifickou pro připojení**.</span><span class="sxs-lookup"><span data-stu-id="448ce-124">Write down **Connection-specific DNS Suffix**.</span></span> <span data-ttu-id="448ce-125">Například *myhbasecluster.f5.internal.cloudapp.net*.</span><span class="sxs-lookup"><span data-stu-id="448ce-125">For example, *myhbasecluster.f5.internal.cloudapp.net*.</span></span> <span data-ttu-id="448ce-126">Když připojíte tooan HBase cluster, budete potřebovat tooconnect tooone z hello Zookeepers pomocí plně kvalifikovaného názvu domény.</span><span class="sxs-lookup"><span data-stu-id="448ce-126">When you connect tooan HBase cluster, you will need tooconnect tooone of hello Zookeepers using FQDN.</span></span> <span data-ttu-id="448ce-127">Každý cluster HDInsight má 3 Zookeepers.</span><span class="sxs-lookup"><span data-stu-id="448ce-127">Each HDInsight cluster has 3 Zookeepers.</span></span> <span data-ttu-id="448ce-128">Jsou *zookeeper0*, *zookeeper1*, a *zookeeper2*.</span><span class="sxs-lookup"><span data-stu-id="448ce-128">They are *zookeeper0*, *zookeeper1*, and *zookeeper2*.</span></span> <span data-ttu-id="448ce-129">Hello plně kvalifikovaný název domény bude vypadat přibližně jako *zookeeper2.myhbasecluster.f5.internal.cloudapp.net*.</span><span class="sxs-lookup"><span data-stu-id="448ce-129">hello FQDN will be something like *zookeeper2.myhbasecluster.f5.internal.cloudapp.net*.</span></span>

<span data-ttu-id="448ce-130">**toouse SQLLine**</span><span class="sxs-lookup"><span data-stu-id="448ce-130">**toouse SQLLine**</span></span>

1. <span data-ttu-id="448ce-131">Otevřete **Hadoop příkazového řádku** z plochy hello.</span><span class="sxs-lookup"><span data-stu-id="448ce-131">Open **Hadoop Command Line** from hello desktop.</span></span>
2. <span data-ttu-id="448ce-132">Spusťte následující příkazy tooopen SQLLine hello:</span><span class="sxs-lookup"><span data-stu-id="448ce-132">Run hello following commands tooopen SQLLine:</span></span>

        cd %phoenix_home%\bin
        sqlline.py [hello FQDN of one of hello Zookeepers]

    ![HDInsight hbase phoenix sqlline][hdinsight-hbase-phoenix-sqlline]

    <span data-ttu-id="448ce-134">příkazy Hello používá v ukázce hello:</span><span class="sxs-lookup"><span data-stu-id="448ce-134">hello commands used in hello sample:</span></span>

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

<span data-ttu-id="448ce-135">Další informace najdete v tématu [SQLLine ruční](http://sqlline.sourceforge.net/#manual) a [Phoenix gramatika](http://phoenix.apache.org/language/index.html).</span><span class="sxs-lookup"><span data-stu-id="448ce-135">For more information, see [SQLLine manual](http://sqlline.sourceforge.net/#manual) and [Phoenix Grammar](http://phoenix.apache.org/language/index.html).</span></span>

## <a name="use-squirrel"></a><span data-ttu-id="448ce-136">Použití SQuirreL</span><span class="sxs-lookup"><span data-stu-id="448ce-136">Use SQuirreL</span></span>
<span data-ttu-id="448ce-137">[SQuirreL klienta SQL](http://squirrel-sql.sourceforge.net/) je grafické Java program, který umožní tooview hello struktura databáze kompatibilní JDBC, procházet hello data v tabulkách, vydávat příkazy SQL atd. Dá se použít tooconnect tooApache Phoenix v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="448ce-137">[SQuirreL SQL Client](http://squirrel-sql.sourceforge.net/) is a graphical Java program that will allow you tooview hello structure of a JDBC compliant database, browse hello data in tables, issue SQL commands etc. It can be used tooconnect tooApache Phoenix on HDInsight.</span></span>

<span data-ttu-id="448ce-138">V této části se dozvíte, jak tooinstall a konfigurace SQuirreL na vaše pracovní stanice tooconnect tooan cluster HBase v HDInsight prostřednictvím sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="448ce-138">This section shows you how tooinstall and configure SQuirreL on your workstation tooconnect tooan HBase cluster in HDInsight via VPN.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="448ce-139">Požadavky</span><span class="sxs-lookup"><span data-stu-id="448ce-139">Prerequisites</span></span>
<span data-ttu-id="448ce-140">Než budete postupovat hello postupy, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="448ce-140">Before following hello procedures, you must have hello following:</span></span>

* <span data-ttu-id="448ce-141">HBase cluster nasadit tooan virtuální síť Azure s virtuálním počítačem DNS.</span><span class="sxs-lookup"><span data-stu-id="448ce-141">An HBase cluster deployed tooan Azure virtual network with a DNS virtual machine.</span></span>  <span data-ttu-id="448ce-142">Pokyny najdete v tématu [vytvořte HBase clusterů ve virtuální síti Azure][hdinsight-hbase-provision-vnet].</span><span class="sxs-lookup"><span data-stu-id="448ce-142">For instructions, see [Create HBase clusters on Azure Virtual Network][hdinsight-hbase-provision-vnet].</span></span>

* <span data-ttu-id="448ce-143">Získáte přípona DNS podle připojení clusteru cluster HBase hello.</span><span class="sxs-lookup"><span data-stu-id="448ce-143">Get hello HBase cluster cluster Connection-specific DNS suffix.</span></span> <span data-ttu-id="448ce-144">tooget ho RDP do hello clusteru, a poté spusťte příkaz IPConfig.</span><span class="sxs-lookup"><span data-stu-id="448ce-144">tooget it, RDP into hello cluster, and then run IPConfig.</span></span>  <span data-ttu-id="448ce-145">přípona DNS Hello je podobná:</span><span class="sxs-lookup"><span data-stu-id="448ce-145">hello DNS suffix is similar to:</span></span>

        myhbase.b7.internal.cloudapp.net
* <span data-ttu-id="448ce-146">Stáhněte a nainstalujte [Microsoft Visual Studio Express pro Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx) na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="448ce-146">Download and install [Microsoft Visual Studio Express for Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx) on your workstation.</span></span> <span data-ttu-id="448ce-147">Je nutné makecert z balíčku toocreate hello svůj certifikát.</span><span class="sxs-lookup"><span data-stu-id="448ce-147">You will need makecert from hello package toocreate your certificate.</span></span>  
* <span data-ttu-id="448ce-148">Stáhněte a nainstalujte [prostředí Java Runtime](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html) na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="448ce-148">Download and install [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html) on your workstation.</span></span>  <span data-ttu-id="448ce-149">SQuirreL SQL klientská verze 3.0 nebo vyšší vyžaduje prostředí JRE 1.6 nebo vyšší verze.</span><span class="sxs-lookup"><span data-stu-id="448ce-149">SQuirreL SQL client version 3.0 and higher requires JRE version 1.6 or higher.</span></span>  

### <a name="configure-a-point-to-site-vpn-connection-toohello-azure-virtual-network"></a><span data-ttu-id="448ce-150">Konfigurace toohello připojení Point-to-Site VPN virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="448ce-150">Configure a Point-to-Site VPN connection toohello Azure virtual network</span></span>
<span data-ttu-id="448ce-151">Související se situací konfiguraci připojení point-to-site VPN jsou 3 kroky:</span><span class="sxs-lookup"><span data-stu-id="448ce-151">There are 3 steps involved configuring a point-to-site VPN connection:</span></span>

1. [<span data-ttu-id="448ce-152">Konfigurace virtuální sítě a brány dynamického směrování</span><span class="sxs-lookup"><span data-stu-id="448ce-152">Configure a virtual network and a dynamic routing gateway</span></span>](#Configure-a-virtual-network-and-a-dynamic-routing-gateway)
2. [<span data-ttu-id="448ce-153">Vytvoření certifikáty</span><span class="sxs-lookup"><span data-stu-id="448ce-153">Create your certificates</span></span>](#Create-your-certificates)
3. [<span data-ttu-id="448ce-154">Konfigurace klienta VPN</span><span class="sxs-lookup"><span data-stu-id="448ce-154">Configure your VPN client</span></span>](#Configure-your-VPN-client)

<span data-ttu-id="448ce-155">V tématu [konfigurace tooan připojení Point-to-Site VPN Azure Virtual Network](../vpn-gateway/vpn-gateway-point-to-site-create.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="448ce-155">See [Configure a Point-to-Site VPN connection tooan Azure Virtual Network](../vpn-gateway/vpn-gateway-point-to-site-create.md) for more information.</span></span>

#### <a name="configure-a-virtual-network-and-a-dynamic-routing-gateway"></a><span data-ttu-id="448ce-156">Konfigurace virtuální sítě a brány dynamického směrování</span><span class="sxs-lookup"><span data-stu-id="448ce-156">Configure a virtual network and a dynamic routing gateway</span></span>
<span data-ttu-id="448ce-157">Zajistit, když máte zřízenou cluster HBase v virtuální sítě Azure (viz hello požadavky pro tento oddíl).</span><span class="sxs-lookup"><span data-stu-id="448ce-157">Assure you have provisioned an HBase cluster in an Azure virtual network (see hello prerequisites for this section).</span></span> <span data-ttu-id="448ce-158">dalším krokem Hello je tooconfigure připojení point-to-site.</span><span class="sxs-lookup"><span data-stu-id="448ce-158">hello next step is tooconfigure a point-to-site connection.</span></span>

<span data-ttu-id="448ce-159">**připojení point-to-site tooconfigure hello**</span><span class="sxs-lookup"><span data-stu-id="448ce-159">**tooconfigure hello point-to-site connectivity**</span></span>

1. <span data-ttu-id="448ce-160">Přihlaste se toohello [portálu Azure Classic][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="448ce-160">Sign in toohello [Azure Classic Portal][azure-portal].</span></span>
2. <span data-ttu-id="448ce-161">Na levé straně hello, klikněte na tlačítko **sítě**.</span><span class="sxs-lookup"><span data-stu-id="448ce-161">On hello left, click **NETWORKS**.</span></span>
3. <span data-ttu-id="448ce-162">Klikněte na virtuální síť hello jste vytvořili (v tématu [HBase zřizování clusterů ve virtuální síti Azure][hdinsight-hbase-provision-vnet]).</span><span class="sxs-lookup"><span data-stu-id="448ce-162">Click hello virtual network you have created (see [Provision HBase clusters on Azure Virtual Network][hdinsight-hbase-provision-vnet]).</span></span>
4. <span data-ttu-id="448ce-163">Klikněte na tlačítko **konfigurace** shora hello.</span><span class="sxs-lookup"><span data-stu-id="448ce-163">Click **CONFIGURE** from hello top.</span></span>
5. <span data-ttu-id="448ce-164">V hello **připojení point-to-site** vyberte **konfiguraci připojení point-to-site**.</span><span class="sxs-lookup"><span data-stu-id="448ce-164">In hello **point-to-site connectivity** section, select **Configure point-to-site connectivity**.</span></span>
6. <span data-ttu-id="448ce-165">Konfigurace **počáteční IP** a **CIDR** toospecify hello IP adresu rozsahu, ze kterého klienti sítě VPN obdrží IP adres při připojení.</span><span class="sxs-lookup"><span data-stu-id="448ce-165">Configure **STARTING IP** and **CIDR** toospecify hello IP address range from which your VPN clients will receive an IP address when connected.</span></span> <span data-ttu-id="448ce-166">rozsah Hello se nesmí překrývat s žádným z rozsahů ve vaší místní sítě a virtuální síť Azure, které se připojují k hello hello.</span><span class="sxs-lookup"><span data-stu-id="448ce-166">hello range cannot overlap with any of hello ranges located on your on-premises network and hello Azure virtual network you will be connecting to.</span></span> <span data-ttu-id="448ce-167">Například.</span><span class="sxs-lookup"><span data-stu-id="448ce-167">For example.</span></span> <span data-ttu-id="448ce-168">Pokud jste vybrali 10.0.0.0/20 hello virtuální sítě, můžete vybrat 10.1.0.0/24 pro hello klienta adresní prostor.</span><span class="sxs-lookup"><span data-stu-id="448ce-168">if you selected 10.0.0.0/20 for hello virtual network, you can select 10.1.0.0/24 for hello client address space.</span></span> <span data-ttu-id="448ce-169">V tématu hello [připojeníPoint-To-Site] [ vnet-point-to-site-connectivity] stránka Další informace.</span><span class="sxs-lookup"><span data-stu-id="448ce-169">See hello [Point-To-Site Connectivity][vnet-point-to-site-connectivity] page for more information.</span></span>
7. <span data-ttu-id="448ce-170">V části prostory adres hello virtuální sítě, klikněte na tlačítko **přidat podsíť brány**.</span><span class="sxs-lookup"><span data-stu-id="448ce-170">In hello virtual network address spaces section, click **add gateway subnet**.</span></span>
8. <span data-ttu-id="448ce-171">Klikněte na tlačítko **Uložit** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="448ce-171">Click **SAVE** on hello bottom of hello page.</span></span>
9. <span data-ttu-id="448ce-172">Klikněte na tlačítko **Ano** tooconfirm hello změnu.</span><span class="sxs-lookup"><span data-stu-id="448ce-172">Click **YES** tooconfirm hello change.</span></span> <span data-ttu-id="448ce-173">Počkejte, dokud hello systém dokončil provádění hello změnit než budete pokračovat dalším postupu toohello.</span><span class="sxs-lookup"><span data-stu-id="448ce-173">Wait until hello system has finished making hello change before you proceed toohello next procedure.</span></span>

<span data-ttu-id="448ce-174">**toocreate brány dynamického směrování**</span><span class="sxs-lookup"><span data-stu-id="448ce-174">**toocreate a dynamic routing gateway**</span></span>

1. <span data-ttu-id="448ce-175">Z portálu Azure Classic hello, klikněte na tlačítko **řídicí panel** z hello horní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="448ce-175">From hello Azure Classic Portal, click **DASHBOARD** from hello top of hello page.</span></span>
2. <span data-ttu-id="448ce-176">Klikněte na tlačítko **vytvořit BRÁNU** hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="448ce-176">Click **CREATE GATEWAY** from hello bottom of hello page.</span></span>
3. <span data-ttu-id="448ce-177">Klikněte na tlačítko **Ano** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="448ce-177">Click **YES** tooconfirm.</span></span> <span data-ttu-id="448ce-178">Počkejte na vytvoření brány hello.</span><span class="sxs-lookup"><span data-stu-id="448ce-178">Wait until hello gateway is created.</span></span>
4. <span data-ttu-id="448ce-179">Klikněte na tlačítko **řídicí panel** shora hello.</span><span class="sxs-lookup"><span data-stu-id="448ce-179">Click **DASHBOARD** from hello top.</span></span>  <span data-ttu-id="448ce-180">Zobrazí se visual diagram hello virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="448ce-180">You will see a visual diagram of hello virtual network:</span></span>

    ![Virtuální diagram point-to-site virtuální síť Azure][img-vnet-diagram]

    <span data-ttu-id="448ce-182">Hello diagram znázorňuje 0 připojení klientů.</span><span class="sxs-lookup"><span data-stu-id="448ce-182">hello diagram shows 0 client connections.</span></span> <span data-ttu-id="448ce-183">Když provedete toohello připojení virtuální sítě, počet hello bude aktualizované tooone.</span><span class="sxs-lookup"><span data-stu-id="448ce-183">After you make a connection toohello virtual network, hello number will be updated tooone.</span></span>

#### <a name="create-your-certificates"></a><span data-ttu-id="448ce-184">Vytvoření certifikáty</span><span class="sxs-lookup"><span data-stu-id="448ce-184">Create your certificates</span></span>
<span data-ttu-id="448ce-185">Jedním ze způsobů toocreate certifikát X.509 je pomocí hello nástroje vytvoření certifikátu (makecert.exe), která se dodává s [Microsoft Visual Studio Express pro Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="448ce-185">One way toocreate an X.509 certificate is by using hello Certificate Creation Tool (makecert.exe) that comes with [Microsoft Visual Studio Express for Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx).</span></span>

<span data-ttu-id="448ce-186">**toocreate certifikát podepsaný svým držitelem kořenové**</span><span class="sxs-lookup"><span data-stu-id="448ce-186">**toocreate a self-signed root certificate**</span></span>

1. <span data-ttu-id="448ce-187">Pracovní stanice otevřete okno příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="448ce-187">From your workstation, open a command prompt window.</span></span>
2. <span data-ttu-id="448ce-188">Přejděte toohello složka nástrojů pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="448ce-188">Navigate toohello Visual Studio tools folder.</span></span>
3. <span data-ttu-id="448ce-189">Následující příkaz v následujícím příkladu hello Hello vytvoříte a instalace kořenového certifikátu v úložišti osobních certifikátů hello na pracovní stanici a také vytvořit odpovídající soubor .cer, že později nahrajete toohello portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="448ce-189">hello following command in hello example below will create and install a root certificate in hello Personal certificate store on your workstation and also create a corresponding .cer file that you’ll later upload toohello Azure Classic Portal.</span></span>

        makecert -sky exchange -r -n "CN=HBaseVnetVPNRootCertificate" -pe -a sha1 -len 2048 -ss My "C:\Users\JohnDole\Desktop\HBaseVNetVPNRootCertificate.cer"

    <span data-ttu-id="448ce-190">Změnit toohello adresář, který chcete hello toobe soubor .cer umístěny v, kde HBaseVnetVPNRootCertificate je název hello chcete toouse hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="448ce-190">Change toohello directory that you want hello .cer file toobe located in, where HBaseVnetVPNRootCertificate is hello name that you want toouse for hello certificate.</span></span>

    <span data-ttu-id="448ce-191">Neuzavírejte hello příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="448ce-191">Don't close hello command prompt.</span></span>  <span data-ttu-id="448ce-192">Budete ho potřebovat v dalším postupu hello.</span><span class="sxs-lookup"><span data-stu-id="448ce-192">You will need it in hello next procedure.</span></span>

   > [!NOTE]
   > <span data-ttu-id="448ce-193">Vzhledem k tomu, že jste vytvořili kořenový certifikát, ze kterého se budou generovat klientské certifikáty, může tooexport společně s jeho privátní klíč tohoto certifikátu a uložte ho tooa bezpečného umístění, kde ho bude možné obnovit.</span><span class="sxs-lookup"><span data-stu-id="448ce-193">Because you have created a root certificate from which client certificates will be generated, you may want tooexport this certificate along with its private key and save it tooa safe location where it may be recovered.</span></span>
   >
   >

<span data-ttu-id="448ce-194">**toocreate certifikát klienta**</span><span class="sxs-lookup"><span data-stu-id="448ce-194">**toocreate a client certificate**</span></span>

* <span data-ttu-id="448ce-195">Z hello stejné příkazového řádku (má toobe na hello stejný počítač, kde jste vytvořili hello kořenový certifikát.</span><span class="sxs-lookup"><span data-stu-id="448ce-195">From hello same command prompt (It has toobe on hello same computer where you created hello root certificate.</span></span> <span data-ttu-id="448ce-196">Hello klientský certifikát musí být vygenerovaný z hello kořenového certifikátu), spusťte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="448ce-196">hello client certificate must be generated from hello root certificate), run hello following command:</span></span>

          makecert.exe -n "CN=HBaseVnetVPNClientCertificate" -pe -sky exchange -m 96 -ss My -in "HBaseVnetVPNRootCertificate" -is my -a sha1

    <span data-ttu-id="448ce-197">HBaseVnetVPNRootCertificate je název kořenového certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="448ce-197">HBaseVnetVPNRootCertificate is hello root certificate name.</span></span>  <span data-ttu-id="448ce-198">Obsahuje název toomatch hello kořenového certifikátu.</span><span class="sxs-lookup"><span data-stu-id="448ce-198">It has toomatch hello root certificate name.</span></span>  

    <span data-ttu-id="448ce-199">Hello kořenový certifikát a certifikát klienta hello jsou uloženy v úložišti osobních certifikátů v počítači.</span><span class="sxs-lookup"><span data-stu-id="448ce-199">Both hello root certificate and hello client certificate are stored in your Personal certificate store on your computer.</span></span> <span data-ttu-id="448ce-200">Použijte certmgr.msc tooverify.</span><span class="sxs-lookup"><span data-stu-id="448ce-200">Use certmgr.msc tooverify.</span></span>

    ![Certifikát sítě VPN point-to-site virtuální síť Azure][img-certificate]

    <span data-ttu-id="448ce-202">Klientský certifikát musí nainstalovat na každém počítači, které chcete tooconnect toohello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="448ce-202">A client certificate must be installed on each computer that you want tooconnect toohello virtual network.</span></span> <span data-ttu-id="448ce-203">Doporučujeme vám vytvořit jedinečný klientský certifikát pro každý počítač, které chcete tooconnect toohello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="448ce-203">We recommend that you create unique client certificates for each computer that you want tooconnect toohello virtual network.</span></span> <span data-ttu-id="448ce-204">tooexport hello klientské certifikáty pomocí certmgr.msc.</span><span class="sxs-lookup"><span data-stu-id="448ce-204">tooexport hello client certificates, use certmgr.msc.</span></span>

<span data-ttu-id="448ce-205">**tooupload hello kořenový certifikát toohello portálu Azure Classic**</span><span class="sxs-lookup"><span data-stu-id="448ce-205">**tooupload hello root certificate toohello Azure Classic Portal**</span></span>

1. <span data-ttu-id="448ce-206">Z portálu Azure Classic hello, klikněte na tlačítko **sítě** na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="448ce-206">From hello Azure Classic Portal, click **NETWORK** on hello left.</span></span>
2. <span data-ttu-id="448ce-207">Klikněte na tlačítko hello virtuální síť, kde je nasazen HBase cluster na.</span><span class="sxs-lookup"><span data-stu-id="448ce-207">Click hello virtual network where your HBase cluster is deployed to.</span></span>
3. <span data-ttu-id="448ce-208">Klikněte na tlačítko **certifikáty** shora hello.</span><span class="sxs-lookup"><span data-stu-id="448ce-208">Click **CERTIFICATES** from hello top.</span></span>
4. <span data-ttu-id="448ce-209">Klikněte na tlačítko **NAHRÁT** z hello dolů a zadejte soubor kořenového certifikátu hello jste vytvořili v postupu hello před posledním.</span><span class="sxs-lookup"><span data-stu-id="448ce-209">Click **UPLOAD** from hello bottom, and specify hello root certificate file you have created in hello procedure before last.</span></span> <span data-ttu-id="448ce-210">Počkejte, dokud hello certifikát nebyl importován.</span><span class="sxs-lookup"><span data-stu-id="448ce-210">Wait until hello certificate got imported.</span></span>
5. <span data-ttu-id="448ce-211">Klikněte na tlačítko **řídicí panel** hello nahoře.</span><span class="sxs-lookup"><span data-stu-id="448ce-211">Click **DASHBOARD** on hello top.</span></span>  <span data-ttu-id="448ce-212">Hello virtuální diagram zobrazuje stav hello.</span><span class="sxs-lookup"><span data-stu-id="448ce-212">hello virtual diagram shows hello status.</span></span>

#### <a name="configure-your-vpn-client"></a><span data-ttu-id="448ce-213">Konfigurace klienta VPN</span><span class="sxs-lookup"><span data-stu-id="448ce-213">Configure your VPN client</span></span>
<span data-ttu-id="448ce-214">**toodownload a nainstalujte hello balíčku klienta VPN**</span><span class="sxs-lookup"><span data-stu-id="448ce-214">**toodownload and install hello client VPN package**</span></span>

1. <span data-ttu-id="448ce-215">Na stránce řídicího PANELU hello hello virtuální sítě, v části rychlého přehledu hello, klikněte na možnost **hello stažení balíčku klienta VPN 64-bit** nebo **hello stažení balíčku klienta VPN 32-bit** na základě vaší verze operačního systému pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="448ce-215">From hello DASHBOARD page of hello virtual network, in hello quick glance section, click either **Download hello 64-bit Client VPN Package** or **Download hello 32-bit Client VPN Package** based on your workstation OS version.</span></span>
2. <span data-ttu-id="448ce-216">Klikněte na tlačítko **spustit** tooinstall hello balíčku.</span><span class="sxs-lookup"><span data-stu-id="448ce-216">Click **Run** tooinstall hello package.</span></span>
3. <span data-ttu-id="448ce-217">Na řádku hello zabezpečení, klikněte na tlačítko **Další informace o**a potom klikněte na **přesto spustit**.</span><span class="sxs-lookup"><span data-stu-id="448ce-217">At hello security prompt, click **More info**, and then click **Run anyway**.</span></span>
4. <span data-ttu-id="448ce-218">Klikněte na tlačítko **Ano** dvakrát.</span><span class="sxs-lookup"><span data-stu-id="448ce-218">Click **Yes** twice.</span></span>

<span data-ttu-id="448ce-219">**tooconnect tooVPN**</span><span class="sxs-lookup"><span data-stu-id="448ce-219">**tooconnect tooVPN**</span></span>

1. <span data-ttu-id="448ce-220">Na ploše hello pracovní stanice klikněte na ikonu sítě hello na hlavním panelu hello.</span><span class="sxs-lookup"><span data-stu-id="448ce-220">On hello desktop of your workstation, click hello Networks icon on hello Task bar.</span></span> <span data-ttu-id="448ce-221">Zobrazí se připojení VPN s názvem vaší virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="448ce-221">You shall see a VPN connection with your virtual network name.</span></span>
2. <span data-ttu-id="448ce-222">Klikněte na název připojení VPN hello.</span><span class="sxs-lookup"><span data-stu-id="448ce-222">Click hello VPN connection name.</span></span>
3. <span data-ttu-id="448ce-223">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="448ce-223">Click **Connect**.</span></span>

<span data-ttu-id="448ce-224">**tootest hello VPN připojení a domény překlad**</span><span class="sxs-lookup"><span data-stu-id="448ce-224">**tootest hello VPN connection and domain name resolution**</span></span>

* <span data-ttu-id="448ce-225">Z hello pracovní stanice, otevřete příkazový řádek a otestování příkazem ping jeden hello následující názvy zadané hello HBase cluster přípona DNS je myhbase.b7.internal.cloudapp.net:</span><span class="sxs-lookup"><span data-stu-id="448ce-225">From hello workstation, open a command prompt and ping one of hello following names given hello HBase cluster's DNS suffix is myhbase.b7.internal.cloudapp.net:</span></span>

        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        headnode0.myhbase.b7.internal.cloudapp.net
        headnode1.myhbase.b7.internal.cloudapp.net
        workernode0.myhbase.b7.internal.cloudapp.net

### <a name="install-and-configure-squirrel-on-your-workstation"></a><span data-ttu-id="448ce-226">Instalace a konfigurace SQuirreL na pracovní stanici</span><span class="sxs-lookup"><span data-stu-id="448ce-226">Install and configure SQuirreL on your workstation</span></span>
<span data-ttu-id="448ce-227">**tooinstall SQuirreL**</span><span class="sxs-lookup"><span data-stu-id="448ce-227">**tooinstall SQuirreL**</span></span>

1. <span data-ttu-id="448ce-228">Stáhněte si soubor jar klienta hello SQuirreL SQL z [http://squirrel-sql.sourceforge.net/#installation](http://squirrel-sql.sourceforge.net/#installation).</span><span class="sxs-lookup"><span data-stu-id="448ce-228">Download hello SQuirreL SQL client jar file from [http://squirrel-sql.sourceforge.net/#installation](http://squirrel-sql.sourceforge.net/#installation).</span></span>
2. <span data-ttu-id="448ce-229">Soubor jar hello otevření a spuštění.</span><span class="sxs-lookup"><span data-stu-id="448ce-229">Open/run hello jar file.</span></span> <span data-ttu-id="448ce-230">Vyžaduje hello [prostředí Java Runtime](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html).</span><span class="sxs-lookup"><span data-stu-id="448ce-230">It requires hello [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html).</span></span>
3. <span data-ttu-id="448ce-231">Klikněte na tlačítko **Další** dvakrát.</span><span class="sxs-lookup"><span data-stu-id="448ce-231">Click **Next** twice.</span></span>
4. <span data-ttu-id="448ce-232">Zadejte cestu, kde máte hello oprávnění zapisovat a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="448ce-232">Specify a path where you have hello write permission, and then click **Next**.</span></span>

  > [!NOTE]
  > <span data-ttu-id="448ce-233">Hello výchozí instalační složka je ve složce C:\Program Files\squirrel-sql-3.6 hello.</span><span class="sxs-lookup"><span data-stu-id="448ce-233">hello default installation folder is in hello C:\Program Files\squirrel-sql-3.6 folder.</span></span>  <span data-ttu-id="448ce-234">V cestě toothis toowrite pořadí instalační program hello musejí mít oprávnění hello správce.</span><span class="sxs-lookup"><span data-stu-id="448ce-234">In order toowrite toothis path, hello installer must be granted hello administrator privilege.</span></span> <span data-ttu-id="448ce-235">Otevřete příkazový řádek jako správce, přejděte na tooJava složka bin a spusťte:</span><span class="sxs-lookup"><span data-stu-id="448ce-235">You can open a command prompt as administrator, navigate tooJava's bin folder, and then run:</span></span>
  >
  >     <span data-ttu-id="448ce-236">Java.exe-jar [hello cesta soubor jar SQuirreL hello]</span><span class="sxs-lookup"><span data-stu-id="448ce-236">java.exe -jar [hello path of hello SQuirreL jar file]</span></span>
5. <span data-ttu-id="448ce-237">Klikněte na tlačítko **OK** tooconfirm vytváření hello cílový adresář.</span><span class="sxs-lookup"><span data-stu-id="448ce-237">Click **OK** tooconfirm creating hello target directory.</span></span>
6. <span data-ttu-id="448ce-238">Hello výchozí nastavení je tooinstall hello základní a standardní balíčky.</span><span class="sxs-lookup"><span data-stu-id="448ce-238">hello default setting is tooinstall hello Base and Standard packages.</span></span>  <span data-ttu-id="448ce-239">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="448ce-239">Click **Next**.</span></span>
7. <span data-ttu-id="448ce-240">Klikněte na tlačítko **Další** dvakrát a potom klikněte na **provádí**.</span><span class="sxs-lookup"><span data-stu-id="448ce-240">Click **Next** twice, and then click **Done**.</span></span>

<span data-ttu-id="448ce-241">**tooinstall hello Phoenix ovladačů**</span><span class="sxs-lookup"><span data-stu-id="448ce-241">**tooinstall hello Phoenix driver**</span></span>

<span data-ttu-id="448ce-242">soubor jar ovladače phoenix Hello se nachází na hello HBase cluster.</span><span class="sxs-lookup"><span data-stu-id="448ce-242">hello phoenix driver jar file is located on hello HBase cluster.</span></span> <span data-ttu-id="448ce-243">Cesta Hello je podobné následující toohello podle verze hello:</span><span class="sxs-lookup"><span data-stu-id="448ce-243">hello path is similar toohello following based on hello versions:</span></span>

    C:\apps\dist\phoenix-4.0.0.2.1.11.0-2316\phoenix-4.0.0.2.1.11.0-2316-client.jar
<span data-ttu-id="448ce-244">Je třeba toocopy ho tooyour pracovní stanice v rámci hello [Instalační složka SQuirreL] / lib cestu.</span><span class="sxs-lookup"><span data-stu-id="448ce-244">You need toocopy it tooyour workstation under hello [SQuirreL installation folder]/lib path.</span></span>  <span data-ttu-id="448ce-245">Hello nejjednodušší způsob je tooRDP do hello clusteru a potom použijte soubor kopírování a vkládání (CTRL + C a CTRL + V) toocopy ho tooyour pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="448ce-245">hello easiest way is tooRDP into hello cluster, and then use file copy/paste (CTRL+C and CTRL+V) toocopy it tooyour workstation.</span></span>

<span data-ttu-id="448ce-246">**tooadd tooSQuirreL Phoenix ovladačů**</span><span class="sxs-lookup"><span data-stu-id="448ce-246">**tooadd a Phoenix driver tooSQuirreL**</span></span>

1. <span data-ttu-id="448ce-247">Otevřete SQuirreL SQL klienta z pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="448ce-247">Open SQuirreL SQL Client from your workstation.</span></span>
2. <span data-ttu-id="448ce-248">Klikněte na tlačítko hello **ovladač** karty na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="448ce-248">Click hello **Driver** tab on hello left.</span></span>
3. <span data-ttu-id="448ce-249">Z hello **ovladače** nabídky, klikněte na tlačítko **nový ovladač**.</span><span class="sxs-lookup"><span data-stu-id="448ce-249">From hello **Drivers** menu, click **New Driver**.</span></span>
4. <span data-ttu-id="448ce-250">Zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="448ce-250">Enter hello following information:</span></span>

   * <span data-ttu-id="448ce-251">**Název**: Phoenix</span><span class="sxs-lookup"><span data-stu-id="448ce-251">**Name**: Phoenix</span></span>
   * <span data-ttu-id="448ce-252">**Příklad adresy URL**: jdbc:phoenix:zookeeper2.contoso-hbase-eu.f5.internal.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="448ce-252">**Example URL**: jdbc:phoenix:zookeeper2.contoso-hbase-eu.f5.internal.cloudapp.net</span></span>
   * <span data-ttu-id="448ce-253">**Název třídy**: org.apache.phoenix.jdbc.PhoenixDriver</span><span class="sxs-lookup"><span data-stu-id="448ce-253">**Class Name**: org.apache.phoenix.jdbc.PhoenixDriver</span></span>

     > [!WARNING]
     > <span data-ttu-id="448ce-254">Uživatel všechny malá písmena v hello Příklad adresy URL.</span><span class="sxs-lookup"><span data-stu-id="448ce-254">User all lower case in hello Example URL.</span></span> <span data-ttu-id="448ce-255">Můžete použít jejich úplné zookeeper kvora v případě, že jeden z nich je vypnutý.</span><span class="sxs-lookup"><span data-stu-id="448ce-255">You can use they full zookeeper quorum in case one of them is down.</span></span>  <span data-ttu-id="448ce-256">názvy hostitelů Hello jsou zookeeper0, zookeeper1 a zookeeper2.</span><span class="sxs-lookup"><span data-stu-id="448ce-256">hello hostnames are zookeeper0, zookeeper1, and zookeeper2.</span></span>
     >
     >

     ![HDInsight HBase Phoenix SQuirreL ovladačů][img-squirrel-driver]
5. <span data-ttu-id="448ce-258">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="448ce-258">Click **OK**.</span></span>

<span data-ttu-id="448ce-259">**toocreate alias toohello HBase cluster**</span><span class="sxs-lookup"><span data-stu-id="448ce-259">**toocreate an alias toohello HBase cluster**</span></span>

1. <span data-ttu-id="448ce-260">Z SQuirreL, klikněte na tlačítko hello **aliasy** karty na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="448ce-260">From SQuirreL, Click hello **Aliases** tab on hello left.</span></span>
2. <span data-ttu-id="448ce-261">Z hello **aliasy** nabídky, klikněte na tlačítko **nový Alias**.</span><span class="sxs-lookup"><span data-stu-id="448ce-261">From hello **Aliases** menu, click **New Alias**.</span></span>
3. <span data-ttu-id="448ce-262">Zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="448ce-262">Enter hello following information:</span></span>

   * <span data-ttu-id="448ce-263">**Název**: hello název clusteru HBase hello nebo libovolný název, který preferujete.</span><span class="sxs-lookup"><span data-stu-id="448ce-263">**Name**: hello name of hello HBase cluster or any name you prefer.</span></span>
   * <span data-ttu-id="448ce-264">**Ovladač**: Phoenix.</span><span class="sxs-lookup"><span data-stu-id="448ce-264">**Driver**: Phoenix.</span></span>  <span data-ttu-id="448ce-265">Toto musí odpovídat názvu ovladače hello, kterou jste vytvořili v posledním postupu hello.</span><span class="sxs-lookup"><span data-stu-id="448ce-265">This must match hello driver name you created in hello last procedure.</span></span>
   * <span data-ttu-id="448ce-266">**Adresa URL**: hello adresa URL je zkopírována z konfiguraci ovladačů.</span><span class="sxs-lookup"><span data-stu-id="448ce-266">**URL**: hello URL is copied from your driver configuration.</span></span> <span data-ttu-id="448ce-267">Ujistěte se, že toouser všechny malá písmena.</span><span class="sxs-lookup"><span data-stu-id="448ce-267">Make sure toouser all lower case.</span></span>
   * <span data-ttu-id="448ce-268">**Uživatelské jméno**: může být jakýkoli text.</span><span class="sxs-lookup"><span data-stu-id="448ce-268">**User name**: It can be any text.</span></span>  <span data-ttu-id="448ce-269">Vzhledem k tomu, že používáte připojení VPN, hello uživatelské jméno se nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="448ce-269">Because you use VPN connectivity here, hello user name is not used at all.</span></span>
   * <span data-ttu-id="448ce-270">**Heslo**: může být jakýkoli text.</span><span class="sxs-lookup"><span data-stu-id="448ce-270">**Password**: It can be any text.</span></span>

     ![HDInsight HBase Phoenix SQuirreL ovladačů][img-squirrel-alias]
4. <span data-ttu-id="448ce-272">Klikněte na tlačítko **Test**.</span><span class="sxs-lookup"><span data-stu-id="448ce-272">Click **Test**.</span></span>
5. <span data-ttu-id="448ce-273">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="448ce-273">Click **Connect**.</span></span> <span data-ttu-id="448ce-274">Pokud se provede hello připojení, SQuirreL vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="448ce-274">When it makes hello connection, SQuirreL looks like:</span></span>

    ![HBase Phoenix SQuirreL][img-squirrel]

<span data-ttu-id="448ce-276">**toorun testu**</span><span class="sxs-lookup"><span data-stu-id="448ce-276">**toorun a test**</span></span>

1. <span data-ttu-id="448ce-277">Klikněte na tlačítko hello **SQL** kartě právo další toohello **objekty** kartě.</span><span class="sxs-lookup"><span data-stu-id="448ce-277">Click hello **SQL** tab right next toohello **Objects** tab.</span></span>
2. <span data-ttu-id="448ce-278">Zkopírujte a vložte následující kód hello:</span><span class="sxs-lookup"><span data-stu-id="448ce-278">Copy and paste hello following code:</span></span>

        CREATE TABLE IF NOT EXISTS us_population (state CHAR(2) NOT NULL, city VARCHAR NOT NULL, population BIGINT  CONSTRAINT my_pk PRIMARY KEY (state, city))
3. <span data-ttu-id="448ce-279">Klikněte na tlačítko Spustit hello.</span><span class="sxs-lookup"><span data-stu-id="448ce-279">Click hello run button.</span></span>

    ![HBase Phoenix SQuirreL][img-squirrel-sql]
4. <span data-ttu-id="448ce-281">Přepnout zpět toohello **objekty** kartě.</span><span class="sxs-lookup"><span data-stu-id="448ce-281">Switch back toohello **Objects** tab.</span></span>
5. <span data-ttu-id="448ce-282">Rozbalte název aliasu hello a potom rozbalte **tabulky**.</span><span class="sxs-lookup"><span data-stu-id="448ce-282">Expand hello alias name, and then expand **TABLE**.</span></span>  <span data-ttu-id="448ce-283">Zobrazí se nové tabulky hello uvedený v seznamu.</span><span class="sxs-lookup"><span data-stu-id="448ce-283">You shall see hello new table listed under.</span></span>

## <a name="next-steps"></a><span data-ttu-id="448ce-284">Další kroky</span><span class="sxs-lookup"><span data-stu-id="448ce-284">Next steps</span></span>
<span data-ttu-id="448ce-285">V tomto článku jste se naučili jak toouse Apache Phoenix v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="448ce-285">In this article, you have learned how toouse Apache Phoenix in HDInsight.</span></span>  <span data-ttu-id="448ce-286">Další, najdete v části toolearn</span><span class="sxs-lookup"><span data-stu-id="448ce-286">toolearn more, see</span></span>

* <span data-ttu-id="448ce-287">[Přehled HBase ve službě HDInsight][hdinsight-hbase-overview]: HBase je NoSQL open source databáze Apache postavená na systému Hadoop, která poskytuje náhodný přístup a silnou konzistenci pro velké objemy nestrukturovaných a částečně strukturovaných dat.</span><span class="sxs-lookup"><span data-stu-id="448ce-287">[HDInsight HBase overview][hdinsight-hbase-overview]: HBase is an Apache, open-source, NoSQL database built on Hadoop that provides random access and strong consistency for large amounts of unstructured and semistructured data.</span></span>
* <span data-ttu-id="448ce-288">[Zřídit clustery HBase v Azure Virtual Network][hdinsight-hbase-provision-vnet]: clustery HBase integrace virtuální sítě, může být nasazený toohello stejné virtuální sítě jako aplikace tak, že aplikace může komunikovat s HBase přímo.</span><span class="sxs-lookup"><span data-stu-id="448ce-288">[Provision HBase clusters on Azure Virtual Network][hdinsight-hbase-provision-vnet]: With virtual network integration, HBase clusters can be deployed toohello same virtual network as your applications so that applications can communicate with HBase directly.</span></span>
* <span data-ttu-id="448ce-289">[Konfigurace replikace HBase v HDInsight](hdinsight-hbase-replication.md): Zjistěte, jak tooconfigure HBase replikace mezi dvěma datových centrech Azure.</span><span class="sxs-lookup"><span data-stu-id="448ce-289">[Configure HBase replication in HDInsight](hdinsight-hbase-replication.md): Learn how tooconfigure HBase replication across two Azure datacenters.</span></span>
* <span data-ttu-id="448ce-290">[Analýza sentimentu Twitter s HBase v HDInsight][hbase-twitter-sentiment]: Zjistěte, jak v reálném čase toodo [postojích analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) velkých objemů dat pomocí HBase v clusteru Hadoop v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="448ce-290">[Analyze Twitter sentiment with HBase in HDInsight][hbase-twitter-sentiment]: Learn how toodo real-time [sentiment analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) of big data by using HBase in a Hadoop cluster in HDInsight.</span></span>

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png
