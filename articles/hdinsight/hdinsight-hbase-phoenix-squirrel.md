---
title: "Použití Apache Phoenix a SQuirreL s Azure HDInsight se systémem Windows | Microsoft Docs"
description: "Zjistěte, jak používat Apache Phoenix v HDInsight a jak nainstalovat a nakonfigurovat SQuirreL na pracovní stanici pro připojení k cluster HBase v HDInsight."
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
ms.openlocfilehash: 024b70df99fdefa1598225ebb1fbfee85ea375d0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-apache-phoenix-and-squirrel-with-windows-based-hbase-clusters-in-hdinsight"></a><span data-ttu-id="176d0-103">Použití nástrojů Apache Phoenix a SQuirreL s clustery založené na Windows HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="176d0-103">Use Apache Phoenix and SQuirreL with Windows-based HBase clusters in HDInsight</span></span>
<span data-ttu-id="176d0-104">Další informace o použití [Apache Phoenix](http://phoenix.apache.org/) v HDInsight a jak nainstalovat a nakonfigurovat SQuirreL na pracovní stanici pro připojení k cluster HBase v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="176d0-104">Learn how to use [Apache Phoenix](http://phoenix.apache.org/) in HDInsight, and how to install and configure SQuirreL on your workstation to connect to an HBase cluster in HDInsight.</span></span> <span data-ttu-id="176d0-105">Další informace o Phoenix najdete v tématu [Phoenix za 15 minut nebo méně](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html).</span><span class="sxs-lookup"><span data-stu-id="176d0-105">For more information about Phoenix, see [Phoenix in 15 minutes or less](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html).</span></span> <span data-ttu-id="176d0-106">Gramatika Phoenix, najdete v části [Phoenix gramatika](http://phoenix.apache.org/language/index.html).</span><span class="sxs-lookup"><span data-stu-id="176d0-106">For the Phoenix grammar, see [Phoenix Grammar](http://phoenix.apache.org/language/index.html).</span></span>

> [!NOTE]
> <span data-ttu-id="176d0-107">Informace o verzi Phoenix v HDInsight, naleznete v části [co je nového ve verzích clusterů systému Hadoop poskytovaných prostředím HDInsight?](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="176d0-107">For the Phoenix version information in HDInsight, see [What's new in the Hadoop cluster versions provided by HDInsight?](hdinsight-component-versioning.md).</span></span>
>

> [!IMPORTANT]
> <span data-ttu-id="176d0-108">Kroky v tomto dokumentu fungovat pouze pro clustery HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="176d0-108">The steps in this document only work for Windows-based HDInsight clusters.</span></span> <span data-ttu-id="176d0-109">HDInsight je k dispozici pouze v systému Windows verze nižší než HDInsight 3.4.</span><span class="sxs-lookup"><span data-stu-id="176d0-109">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="176d0-110">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="176d0-110">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="176d0-111">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="176d0-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="176d0-112">Informace o používání Phoenix na HDInsight se systémem Linux najdete v tématu [clusterů použití nástrojů Apache Phoenix se systémem Linux HBase v HDInsight](hdinsight-hbase-phoenix-squirrel-linux.md).</span><span class="sxs-lookup"><span data-stu-id="176d0-112">For information on using Phoenix on Linux-based HDInsight, see [Use Apache Phoenix with Linux-based HBase clusters in HDInsight](hdinsight-hbase-phoenix-squirrel-linux.md).</span></span>
>



## <a name="use-sqlline"></a><span data-ttu-id="176d0-113">Použití SQLLine</span><span class="sxs-lookup"><span data-stu-id="176d0-113">Use SQLLine</span></span>
<span data-ttu-id="176d0-114">[SQLLine](http://sqlline.sourceforge.net/) je nástroj příkazového řádku pro provedení SQL.</span><span class="sxs-lookup"><span data-stu-id="176d0-114">[SQLLine](http://sqlline.sourceforge.net/) is a command line utility to execute SQL.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="176d0-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="176d0-115">Prerequisites</span></span>
<span data-ttu-id="176d0-116">Než budete moci použít SQLLine, musíte mít následující:</span><span class="sxs-lookup"><span data-stu-id="176d0-116">Before you can use SQLLine, you must have the following:</span></span>

* <span data-ttu-id="176d0-117">**Cluster HBase v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="176d0-117">**A HBase cluster in HDInsight**.</span></span> <span data-ttu-id="176d0-118">Informace o zřizování HBase clusteru najdete v tématu [začít pracovat s Apache HBase v HDInsight][hdinsight-hbase-get-started].</span><span class="sxs-lookup"><span data-stu-id="176d0-118">For information on provision HBase cluster, see [Get started with Apache HBase in HDInsight][hdinsight-hbase-get-started].</span></span>
* <span data-ttu-id="176d0-119">**Připojte se ke clusteru HBase pomocí protokolu vzdálené plochy**.</span><span class="sxs-lookup"><span data-stu-id="176d0-119">**Connect to the HBase cluster via the remote desktop protocol**.</span></span> <span data-ttu-id="176d0-120">Pokyny najdete v tématu [Správa clusterů systému Hadoop v HDInsight pomocí portálu Azure Classic][hdinsight-manage-portal].</span><span class="sxs-lookup"><span data-stu-id="176d0-120">For instructions, see [Manage Hadoop clusters in HDInsight by using the Azure Classic Portal][hdinsight-manage-portal].</span></span>

<span data-ttu-id="176d0-121">**Chcete-li zjistit název hostitele**</span><span class="sxs-lookup"><span data-stu-id="176d0-121">**To find out the host name**</span></span>

1. <span data-ttu-id="176d0-122">Otevřete **Hadoop příkazového řádku** z plochy.</span><span class="sxs-lookup"><span data-stu-id="176d0-122">Open **Hadoop Command Line** from the desktop.</span></span>
2. <span data-ttu-id="176d0-123">Spusťte následující příkaz k získání příponu DNS:</span><span class="sxs-lookup"><span data-stu-id="176d0-123">Run the following command to get the DNS suffix:</span></span>

        ipconfig

    <span data-ttu-id="176d0-124">Zapište **příponu DNS specifickou pro připojení**.</span><span class="sxs-lookup"><span data-stu-id="176d0-124">Write down **Connection-specific DNS Suffix**.</span></span> <span data-ttu-id="176d0-125">Například *myhbasecluster.f5.internal.cloudapp.net*.</span><span class="sxs-lookup"><span data-stu-id="176d0-125">For example, *myhbasecluster.f5.internal.cloudapp.net*.</span></span> <span data-ttu-id="176d0-126">Jakmile se připojíte ke clusteru HBase, musíte připojit k Zookeepers pomocí plně kvalifikovaného názvu domény.</span><span class="sxs-lookup"><span data-stu-id="176d0-126">When you connect to an HBase cluster, you will need to connect to one of the Zookeepers using FQDN.</span></span> <span data-ttu-id="176d0-127">Každý cluster HDInsight má 3 Zookeepers.</span><span class="sxs-lookup"><span data-stu-id="176d0-127">Each HDInsight cluster has 3 Zookeepers.</span></span> <span data-ttu-id="176d0-128">Jsou *zookeeper0*, *zookeeper1*, a *zookeeper2*.</span><span class="sxs-lookup"><span data-stu-id="176d0-128">They are *zookeeper0*, *zookeeper1*, and *zookeeper2*.</span></span> <span data-ttu-id="176d0-129">Plně kvalifikovaný název domény se něco podobného jako *zookeeper2.myhbasecluster.f5.internal.cloudapp.net*.</span><span class="sxs-lookup"><span data-stu-id="176d0-129">The FQDN will be something like *zookeeper2.myhbasecluster.f5.internal.cloudapp.net*.</span></span>

<span data-ttu-id="176d0-130">**Chcete-li použít SQLLine**</span><span class="sxs-lookup"><span data-stu-id="176d0-130">**To use SQLLine**</span></span>

1. <span data-ttu-id="176d0-131">Otevřete **Hadoop příkazového řádku** z plochy.</span><span class="sxs-lookup"><span data-stu-id="176d0-131">Open **Hadoop Command Line** from the desktop.</span></span>
2. <span data-ttu-id="176d0-132">Spusťte následující příkazy otevřete SQLLine:</span><span class="sxs-lookup"><span data-stu-id="176d0-132">Run the following commands to open SQLLine:</span></span>

        cd %phoenix_home%\bin
        sqlline.py [The FQDN of one of the Zookeepers]

    ![HDInsight hbase phoenix sqlline][hdinsight-hbase-phoenix-sqlline]

    <span data-ttu-id="176d0-134">Funkce použité v ukázce:</span><span class="sxs-lookup"><span data-stu-id="176d0-134">The commands used in the sample:</span></span>

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

<span data-ttu-id="176d0-135">Další informace najdete v tématu [SQLLine ruční](http://sqlline.sourceforge.net/#manual) a [Phoenix gramatika](http://phoenix.apache.org/language/index.html).</span><span class="sxs-lookup"><span data-stu-id="176d0-135">For more information, see [SQLLine manual](http://sqlline.sourceforge.net/#manual) and [Phoenix Grammar](http://phoenix.apache.org/language/index.html).</span></span>

## <a name="use-squirrel"></a><span data-ttu-id="176d0-136">Použití SQuirreL</span><span class="sxs-lookup"><span data-stu-id="176d0-136">Use SQuirreL</span></span>
<span data-ttu-id="176d0-137">[SQuirreL klienta SQL](http://squirrel-sql.sourceforge.net/) je grafické Java program, který vám umožní zobrazit struktura databáze kompatibilní JDBC, procházet data v tabulkách, vydávat příkazy SQL atd. Slouží k připojení k serveru Apache Phoenix v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="176d0-137">[SQuirreL SQL Client](http://squirrel-sql.sourceforge.net/) is a graphical Java program that will allow you to view the structure of a JDBC compliant database, browse the data in tables, issue SQL commands etc. It can be used to connect to Apache Phoenix on HDInsight.</span></span>

<span data-ttu-id="176d0-138">V této části se dozvíte, jak nainstalovat a nakonfigurovat SQuirreL na pracovní stanici se připojit ke clusteru HBase v HDInsight prostřednictvím sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="176d0-138">This section shows you how to install and configure SQuirreL on your workstation to connect to an HBase cluster in HDInsight via VPN.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="176d0-139">Požadavky</span><span class="sxs-lookup"><span data-stu-id="176d0-139">Prerequisites</span></span>
<span data-ttu-id="176d0-140">Než budete postupovat podle pokynů, musí mít následující:</span><span class="sxs-lookup"><span data-stu-id="176d0-140">Before following the procedures, you must have the following:</span></span>

* <span data-ttu-id="176d0-141">Cluster HBase nasadit do virtuální sítě Azure s virtuálním počítačem DNS.</span><span class="sxs-lookup"><span data-stu-id="176d0-141">An HBase cluster deployed to an Azure virtual network with a DNS virtual machine.</span></span>  <span data-ttu-id="176d0-142">Pokyny najdete v tématu [vytvořte HBase clusterů ve virtuální síti Azure][hdinsight-hbase-provision-vnet].</span><span class="sxs-lookup"><span data-stu-id="176d0-142">For instructions, see [Create HBase clusters on Azure Virtual Network][hdinsight-hbase-provision-vnet].</span></span>

* <span data-ttu-id="176d0-143">Získáte přípona DNS podle připojení clusteru cluster HBase.</span><span class="sxs-lookup"><span data-stu-id="176d0-143">Get the HBase cluster cluster Connection-specific DNS suffix.</span></span> <span data-ttu-id="176d0-144">Chcete-li ho RDP do clusteru a poté spusťte příkaz IPConfig.</span><span class="sxs-lookup"><span data-stu-id="176d0-144">To get it, RDP into the cluster, and then run IPConfig.</span></span>  <span data-ttu-id="176d0-145">Přípona DNS je stejný jako:</span><span class="sxs-lookup"><span data-stu-id="176d0-145">The DNS suffix is similar to:</span></span>

        myhbase.b7.internal.cloudapp.net
* <span data-ttu-id="176d0-146">Stáhněte a nainstalujte [Microsoft Visual Studio Express pro Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx) na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="176d0-146">Download and install [Microsoft Visual Studio Express for Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx) on your workstation.</span></span> <span data-ttu-id="176d0-147">Budete potřebovat makecert z balíček, který chcete vytvořit svůj certifikát.</span><span class="sxs-lookup"><span data-stu-id="176d0-147">You will need makecert from the package to create your certificate.</span></span>  
* <span data-ttu-id="176d0-148">Stáhněte a nainstalujte [prostředí Java Runtime](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html) na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="176d0-148">Download and install [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html) on your workstation.</span></span>  <span data-ttu-id="176d0-149">SQuirreL SQL klientská verze 3.0 nebo vyšší vyžaduje prostředí JRE 1.6 nebo vyšší verze.</span><span class="sxs-lookup"><span data-stu-id="176d0-149">SQuirreL SQL client version 3.0 and higher requires JRE version 1.6 or higher.</span></span>  

### <a name="configure-a-point-to-site-vpn-connection-to-the-azure-virtual-network"></a><span data-ttu-id="176d0-150">Konfigurace připojení typu Point-to-Site VPN do virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="176d0-150">Configure a Point-to-Site VPN connection to the Azure virtual network</span></span>
<span data-ttu-id="176d0-151">Související se situací konfiguraci připojení point-to-site VPN jsou 3 kroky:</span><span class="sxs-lookup"><span data-stu-id="176d0-151">There are 3 steps involved configuring a point-to-site VPN connection:</span></span>

1. [<span data-ttu-id="176d0-152">Konfigurace virtuální sítě a brány dynamického směrování</span><span class="sxs-lookup"><span data-stu-id="176d0-152">Configure a virtual network and a dynamic routing gateway</span></span>](#Configure-a-virtual-network-and-a-dynamic-routing-gateway)
2. [<span data-ttu-id="176d0-153">Vytvoření certifikáty</span><span class="sxs-lookup"><span data-stu-id="176d0-153">Create your certificates</span></span>](#Create-your-certificates)
3. [<span data-ttu-id="176d0-154">Konfigurace klienta VPN</span><span class="sxs-lookup"><span data-stu-id="176d0-154">Configure your VPN client</span></span>](#Configure-your-VPN-client)

<span data-ttu-id="176d0-155">V tématu [konfigurace připojení typu Point-to-Site VPN k virtuální síti Azure](../vpn-gateway/vpn-gateway-point-to-site-create.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="176d0-155">See [Configure a Point-to-Site VPN connection to an Azure Virtual Network](../vpn-gateway/vpn-gateway-point-to-site-create.md) for more information.</span></span>

#### <a name="configure-a-virtual-network-and-a-dynamic-routing-gateway"></a><span data-ttu-id="176d0-156">Konfigurace virtuální sítě a brány dynamického směrování</span><span class="sxs-lookup"><span data-stu-id="176d0-156">Configure a virtual network and a dynamic routing gateway</span></span>
<span data-ttu-id="176d0-157">Zajistit, když máte zřízenou cluster HBase v virtuální sítě Azure (viz požadavky na této části).</span><span class="sxs-lookup"><span data-stu-id="176d0-157">Assure you have provisioned an HBase cluster in an Azure virtual network (see the prerequisites for this section).</span></span> <span data-ttu-id="176d0-158">Dalším krokem je konfigurace připojení typu point-to-site.</span><span class="sxs-lookup"><span data-stu-id="176d0-158">The next step is to configure a point-to-site connection.</span></span>

<span data-ttu-id="176d0-159">**Ke konfiguraci připojení point-to-site**</span><span class="sxs-lookup"><span data-stu-id="176d0-159">**To configure the point-to-site connectivity**</span></span>

1. <span data-ttu-id="176d0-160">Přihlaste se k [portál Azure Classic][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="176d0-160">Sign in to the [Azure Classic Portal][azure-portal].</span></span>
2. <span data-ttu-id="176d0-161">Na levé straně klikněte na tlačítko **sítě**.</span><span class="sxs-lookup"><span data-stu-id="176d0-161">On the left, click **NETWORKS**.</span></span>
3. <span data-ttu-id="176d0-162">Klikněte na virtuální síť, které jste vytvořili (viz [HBase zřizování clusterů ve virtuální síti Azure][hdinsight-hbase-provision-vnet]).</span><span class="sxs-lookup"><span data-stu-id="176d0-162">Click the virtual network you have created (see [Provision HBase clusters on Azure Virtual Network][hdinsight-hbase-provision-vnet]).</span></span>
4. <span data-ttu-id="176d0-163">Klikněte na tlačítko **konfigurace** shora.</span><span class="sxs-lookup"><span data-stu-id="176d0-163">Click **CONFIGURE** from the top.</span></span>
5. <span data-ttu-id="176d0-164">V **připojení point-to-site** vyberte **konfiguraci připojení point-to-site**.</span><span class="sxs-lookup"><span data-stu-id="176d0-164">In the **point-to-site connectivity** section, select **Configure point-to-site connectivity**.</span></span>
6. <span data-ttu-id="176d0-165">Konfigurace **počáteční IP** a **CIDR** k určení rozsahu IP adres, ze kterého klienti sítě VPN obdrží IP adresu při připojení.</span><span class="sxs-lookup"><span data-stu-id="176d0-165">Configure **STARTING IP** and **CIDR** to specify the IP address range from which your VPN clients will receive an IP address when connected.</span></span> <span data-ttu-id="176d0-166">Rozsah se nesmí překrývat s žádným z rozsahů ve vaší místní sítí a virtuální síť Azure, které se budou připojovat k.</span><span class="sxs-lookup"><span data-stu-id="176d0-166">The range cannot overlap with any of the ranges located on your on-premises network and the Azure virtual network you will be connecting to.</span></span> <span data-ttu-id="176d0-167">Například.</span><span class="sxs-lookup"><span data-stu-id="176d0-167">For example.</span></span> <span data-ttu-id="176d0-168">Pokud jste vybrali 10.0.0.0/20 pro virtuální síť, můžete vybrat 10.1.0.0/24 pro klienta adresní prostor.</span><span class="sxs-lookup"><span data-stu-id="176d0-168">if you selected 10.0.0.0/20 for the virtual network, you can select 10.1.0.0/24 for the client address space.</span></span> <span data-ttu-id="176d0-169">Najdete v článku [připojeníPoint-To-Site] [ vnet-point-to-site-connectivity] stránka Další informace.</span><span class="sxs-lookup"><span data-stu-id="176d0-169">See the [Point-To-Site Connectivity][vnet-point-to-site-connectivity] page for more information.</span></span>
7. <span data-ttu-id="176d0-170">V části prostory adres virtuální sítě, klikněte na tlačítko **přidat podsíť brány**.</span><span class="sxs-lookup"><span data-stu-id="176d0-170">In the virtual network address spaces section, click **add gateway subnet**.</span></span>
8. <span data-ttu-id="176d0-171">Klikněte na tlačítko **Uložit** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="176d0-171">Click **SAVE** on the bottom of the page.</span></span>
9. <span data-ttu-id="176d0-172">Klikněte na tlačítko **Ano** pro potvrzení změny.</span><span class="sxs-lookup"><span data-stu-id="176d0-172">Click **YES** to confirm the change.</span></span> <span data-ttu-id="176d0-173">Počkejte, dokud systém dokončil provádění změn, než budete pokračovat k dalšímu postupu.</span><span class="sxs-lookup"><span data-stu-id="176d0-173">Wait until the system has finished making the change before you proceed to the next procedure.</span></span>

<span data-ttu-id="176d0-174">**K vytvoření brány dynamického směrování**</span><span class="sxs-lookup"><span data-stu-id="176d0-174">**To create a dynamic routing gateway**</span></span>

1. <span data-ttu-id="176d0-175">Z klasického portálu Azure klikněte na tlačítko **řídicí panel** z horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="176d0-175">From the Azure Classic Portal, click **DASHBOARD** from the top of the page.</span></span>
2. <span data-ttu-id="176d0-176">Klikněte na tlačítko **vytvořit BRÁNU** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="176d0-176">Click **CREATE GATEWAY** from the bottom of the page.</span></span>
3. <span data-ttu-id="176d0-177">Klikněte na tlačítko **Ano** k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="176d0-177">Click **YES** to confirm.</span></span> <span data-ttu-id="176d0-178">Počkejte na vytvoření brány.</span><span class="sxs-lookup"><span data-stu-id="176d0-178">Wait until the gateway is created.</span></span>
4. <span data-ttu-id="176d0-179">Klikněte na tlačítko **řídicí panel** shora.</span><span class="sxs-lookup"><span data-stu-id="176d0-179">Click **DASHBOARD** from the top.</span></span>  <span data-ttu-id="176d0-180">Zobrazí se visual diagram virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="176d0-180">You will see a visual diagram of the virtual network:</span></span>

    ![Virtuální diagram point-to-site virtuální síť Azure][img-vnet-diagram]

    <span data-ttu-id="176d0-182">Diagram znázorňuje 0 připojení klientů.</span><span class="sxs-lookup"><span data-stu-id="176d0-182">The diagram shows 0 client connections.</span></span> <span data-ttu-id="176d0-183">Když provedete připojení k virtuální síti, číslo se aktualizují na jednu.</span><span class="sxs-lookup"><span data-stu-id="176d0-183">After you make a connection to the virtual network, the number will be updated to one.</span></span>

#### <a name="create-your-certificates"></a><span data-ttu-id="176d0-184">Vytvoření certifikáty</span><span class="sxs-lookup"><span data-stu-id="176d0-184">Create your certificates</span></span>
<span data-ttu-id="176d0-185">Jeden způsob, jak vytvořit certifikát X.509 je pomocí nástroje vytvoření certifikátu (makecert.exe), která se dodává s [Microsoft Visual Studio Express pro Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="176d0-185">One way to create an X.509 certificate is by using the Certificate Creation Tool (makecert.exe) that comes with [Microsoft Visual Studio Express for Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx).</span></span>

<span data-ttu-id="176d0-186">**Vytvořit certifikát podepsaný svým držitelem kořenové**</span><span class="sxs-lookup"><span data-stu-id="176d0-186">**To create a self-signed root certificate**</span></span>

1. <span data-ttu-id="176d0-187">Pracovní stanice otevřete okno příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="176d0-187">From your workstation, open a command prompt window.</span></span>
2. <span data-ttu-id="176d0-188">Přejděte do složky nástroje Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="176d0-188">Navigate to the Visual Studio tools folder.</span></span>
3. <span data-ttu-id="176d0-189">Následující příkaz v následujícím příkladu vytvoříte a instalace kořenového certifikátu v úložišti osobních certifikátů na pracovní stanici a také vytvořit odpovídající soubor .cer, který později nahrajete do portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="176d0-189">The following command in the example below will create and install a root certificate in the Personal certificate store on your workstation and also create a corresponding .cer file that you’ll later upload to the Azure Classic Portal.</span></span>

        makecert -sky exchange -r -n "CN=HBaseVnetVPNRootCertificate" -pe -a sha1 -len 2048 -ss My "C:\Users\JohnDole\Desktop\HBaseVNetVPNRootCertificate.cer"

    <span data-ttu-id="176d0-190">Přejděte do adresáře, který chcete soubor .cer umístěný v, kde HBaseVnetVPNRootCertificate je název, který chcete použít pro certifikát.</span><span class="sxs-lookup"><span data-stu-id="176d0-190">Change to the directory that you want the .cer file to be located in, where HBaseVnetVPNRootCertificate is the name that you want to use for the certificate.</span></span>

    <span data-ttu-id="176d0-191">Nemáte zavřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="176d0-191">Don't close the command prompt.</span></span>  <span data-ttu-id="176d0-192">Budete ho potřebovat v dalším postupu.</span><span class="sxs-lookup"><span data-stu-id="176d0-192">You will need it in the next procedure.</span></span>

   > [!NOTE]
   > <span data-ttu-id="176d0-193">Vzhledem k tomu, že jste vytvořili kořenový certifikát, ze kterého se budou generovat klientské certifikáty, měli byste tento certifikát exportovat i s jeho privátním klíčem, a uložit jej na bezpečné místo, odkud ho bude možné obnovit.</span><span class="sxs-lookup"><span data-stu-id="176d0-193">Because you have created a root certificate from which client certificates will be generated, you may want to export this certificate along with its private key and save it to a safe location where it may be recovered.</span></span>
   >
   >

<span data-ttu-id="176d0-194">**Vytvoření certifikátu klienta**</span><span class="sxs-lookup"><span data-stu-id="176d0-194">**To create a client certificate**</span></span>

* <span data-ttu-id="176d0-195">Při použití stejné příkazového řádku (musí být na stejném počítači, kde jste vytvořili kořenový certifikát.</span><span class="sxs-lookup"><span data-stu-id="176d0-195">From the same command prompt (It has to be on the same computer where you created the root certificate.</span></span> <span data-ttu-id="176d0-196">Klientský certifikát musí být vygenerovaný z kořenového certifikátu), spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="176d0-196">The client certificate must be generated from the root certificate), run the following command:</span></span>

          makecert.exe -n "CN=HBaseVnetVPNClientCertificate" -pe -sky exchange -m 96 -ss My -in "HBaseVnetVPNRootCertificate" -is my -a sha1

    <span data-ttu-id="176d0-197">HBaseVnetVPNRootCertificate je název kořenového certifikátu.</span><span class="sxs-lookup"><span data-stu-id="176d0-197">HBaseVnetVPNRootCertificate is the root certificate name.</span></span>  <span data-ttu-id="176d0-198">Musí odpovídat názvu kořenový certifikát.</span><span class="sxs-lookup"><span data-stu-id="176d0-198">It has to match the root certificate name.</span></span>  

    <span data-ttu-id="176d0-199">Kořenový certifikát a certifikát klienta jsou uložené v úložišti osobních certifikátů v počítači.</span><span class="sxs-lookup"><span data-stu-id="176d0-199">Both the root certificate and the client certificate are stored in your Personal certificate store on your computer.</span></span> <span data-ttu-id="176d0-200">Certmgr.msc použijte k ověření.</span><span class="sxs-lookup"><span data-stu-id="176d0-200">Use certmgr.msc to verify.</span></span>

    ![Certifikát sítě VPN point-to-site virtuální síť Azure][img-certificate]

    <span data-ttu-id="176d0-202">Klientský certifikát musí být nainstalován na každém počítači, který chcete připojit k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="176d0-202">A client certificate must be installed on each computer that you want to connect to the virtual network.</span></span> <span data-ttu-id="176d0-203">Doporučujeme vám vytvořit jedinečný klientský certifikát pro každý počítač, který chcete připojit k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="176d0-203">We recommend that you create unique client certificates for each computer that you want to connect to the virtual network.</span></span> <span data-ttu-id="176d0-204">Chcete-li exportovat klientské certifikáty, použijte certmgr.msc.</span><span class="sxs-lookup"><span data-stu-id="176d0-204">To export the client certificates, use certmgr.msc.</span></span>

<span data-ttu-id="176d0-205">**Nahrát kořenový certifikát na portálu Azure Classic**</span><span class="sxs-lookup"><span data-stu-id="176d0-205">**To upload the root certificate to the Azure Classic Portal**</span></span>

1. <span data-ttu-id="176d0-206">Z klasického portálu Azure klikněte na tlačítko **sítě** na levé straně.</span><span class="sxs-lookup"><span data-stu-id="176d0-206">From the Azure Classic Portal, click **NETWORK** on the left.</span></span>
2. <span data-ttu-id="176d0-207">Klikněte na virtuální síť, kde je nasazen HBase cluster na.</span><span class="sxs-lookup"><span data-stu-id="176d0-207">Click the virtual network where your HBase cluster is deployed to.</span></span>
3. <span data-ttu-id="176d0-208">Klikněte na tlačítko **certifikáty** shora.</span><span class="sxs-lookup"><span data-stu-id="176d0-208">Click **CERTIFICATES** from the top.</span></span>
4. <span data-ttu-id="176d0-209">Klikněte na tlačítko **NAHRÁT** z dolní části a zadejte soubor kořenového certifikátu jste vytvořili v postupu před posledním.</span><span class="sxs-lookup"><span data-stu-id="176d0-209">Click **UPLOAD** from the bottom, and specify the root certificate file you have created in the procedure before last.</span></span> <span data-ttu-id="176d0-210">Počkejte, dokud získali certifikát importován.</span><span class="sxs-lookup"><span data-stu-id="176d0-210">Wait until the certificate got imported.</span></span>
5. <span data-ttu-id="176d0-211">Klikněte na tlačítko **řídicí panel** nahoře.</span><span class="sxs-lookup"><span data-stu-id="176d0-211">Click **DASHBOARD** on the top.</span></span>  <span data-ttu-id="176d0-212">Virtuální diagram zobrazuje stav.</span><span class="sxs-lookup"><span data-stu-id="176d0-212">The virtual diagram shows the status.</span></span>

#### <a name="configure-your-vpn-client"></a><span data-ttu-id="176d0-213">Konfigurace klienta VPN</span><span class="sxs-lookup"><span data-stu-id="176d0-213">Configure your VPN client</span></span>
<span data-ttu-id="176d0-214">**Ke stažení a instalaci balíčku klienta VPN**</span><span class="sxs-lookup"><span data-stu-id="176d0-214">**To download and install the client VPN package**</span></span>

1. <span data-ttu-id="176d0-215">Na stránce řídicího PANELU ve virtuální síti, v části rychlého přehledu, klikněte na možnost **stažení balíčku klienta VPN 64-bit** nebo **stažení balíčku klienta VPN 32-bit** založené na pracovní stanici operačního systému verze.</span><span class="sxs-lookup"><span data-stu-id="176d0-215">From the DASHBOARD page of the virtual network, in the quick glance section, click either **Download the 64-bit Client VPN Package** or **Download the 32-bit Client VPN Package** based on your workstation OS version.</span></span>
2. <span data-ttu-id="176d0-216">Klikněte na tlačítko **spustit** k instalaci balíčku.</span><span class="sxs-lookup"><span data-stu-id="176d0-216">Click **Run** to install the package.</span></span>
3. <span data-ttu-id="176d0-217">Do příkazového řádku zabezpečení klikněte na tlačítko **více informací o**a potom klikněte na **přesto spustit**.</span><span class="sxs-lookup"><span data-stu-id="176d0-217">At the security prompt, click **More info**, and then click **Run anyway**.</span></span>
4. <span data-ttu-id="176d0-218">Klikněte na tlačítko **Ano** dvakrát.</span><span class="sxs-lookup"><span data-stu-id="176d0-218">Click **Yes** twice.</span></span>

<span data-ttu-id="176d0-219">**Pro připojení k síti VPN**</span><span class="sxs-lookup"><span data-stu-id="176d0-219">**To connect to VPN**</span></span>

1. <span data-ttu-id="176d0-220">Na ploše pracovní stanice klikněte na ikonu sítě na hlavním panelu úloh.</span><span class="sxs-lookup"><span data-stu-id="176d0-220">On the desktop of your workstation, click the Networks icon on the Task bar.</span></span> <span data-ttu-id="176d0-221">Zobrazí se připojení VPN s názvem vaší virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="176d0-221">You shall see a VPN connection with your virtual network name.</span></span>
2. <span data-ttu-id="176d0-222">Klikněte na název připojení VPN.</span><span class="sxs-lookup"><span data-stu-id="176d0-222">Click the VPN connection name.</span></span>
3. <span data-ttu-id="176d0-223">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="176d0-223">Click **Connect**.</span></span>

<span data-ttu-id="176d0-224">**K testování VPN připojení a domény překlad IP adresy**</span><span class="sxs-lookup"><span data-stu-id="176d0-224">**To test the VPN connection and domain name resolution**</span></span>

* <span data-ttu-id="176d0-225">Z pracovní stanice, otevřete příkazový řádek a ping, jeden z následujících názvů zadána přípona DNS HBase cluster je myhbase.b7.internal.cloudapp.net:</span><span class="sxs-lookup"><span data-stu-id="176d0-225">From the workstation, open a command prompt and ping one of the following names given the HBase cluster's DNS suffix is myhbase.b7.internal.cloudapp.net:</span></span>

        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        headnode0.myhbase.b7.internal.cloudapp.net
        headnode1.myhbase.b7.internal.cloudapp.net
        workernode0.myhbase.b7.internal.cloudapp.net

### <a name="install-and-configure-squirrel-on-your-workstation"></a><span data-ttu-id="176d0-226">Instalace a konfigurace SQuirreL na pracovní stanici</span><span class="sxs-lookup"><span data-stu-id="176d0-226">Install and configure SQuirreL on your workstation</span></span>
<span data-ttu-id="176d0-227">**Chcete-li nainstalovat SQuirreL**</span><span class="sxs-lookup"><span data-stu-id="176d0-227">**To install SQuirreL**</span></span>

1. <span data-ttu-id="176d0-228">Stáhněte si soubor jar klienta SQuirreL SQL z [http://squirrel-sql.sourceforge.net/#installation](http://squirrel-sql.sourceforge.net/#installation).</span><span class="sxs-lookup"><span data-stu-id="176d0-228">Download the SQuirreL SQL client jar file from [http://squirrel-sql.sourceforge.net/#installation](http://squirrel-sql.sourceforge.net/#installation).</span></span>
2. <span data-ttu-id="176d0-229">Otevření a spuštění na soubor jar.</span><span class="sxs-lookup"><span data-stu-id="176d0-229">Open/run the jar file.</span></span> <span data-ttu-id="176d0-230">Vyžaduje [prostředí Java Runtime](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html).</span><span class="sxs-lookup"><span data-stu-id="176d0-230">It requires the [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html).</span></span>
3. <span data-ttu-id="176d0-231">Klikněte na tlačítko **Další** dvakrát.</span><span class="sxs-lookup"><span data-stu-id="176d0-231">Click **Next** twice.</span></span>
4. <span data-ttu-id="176d0-232">Zadejte cestu, kde máte oprávnění k zápisu a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="176d0-232">Specify a path where you have the write permission, and then click **Next**.</span></span>

  > [!NOTE]
  > <span data-ttu-id="176d0-233">Výchozí instalační složka je ve složce C:\Program Files\squirrel-sql-3.6.</span><span class="sxs-lookup"><span data-stu-id="176d0-233">The default installation folder is in the C:\Program Files\squirrel-sql-3.6 folder.</span></span>  <span data-ttu-id="176d0-234">Aby bylo možné zapsat do této cesty, instalační program musí mít oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="176d0-234">In order to write to this path, the installer must be granted the administrator privilege.</span></span> <span data-ttu-id="176d0-235">Otevřete příkazový řádek jako správce, přejděte do složky Bin přejít na jazyce Java a spusťte:</span><span class="sxs-lookup"><span data-stu-id="176d0-235">You can open a command prompt as administrator, navigate to Java's bin folder, and then run:</span></span>
  >
  >     <span data-ttu-id="176d0-236">Java.exe-jar [cestu na soubor jar SQuirreL]</span><span class="sxs-lookup"><span data-stu-id="176d0-236">java.exe -jar [the path of the SQuirreL jar file]</span></span>
5. <span data-ttu-id="176d0-237">Klikněte na tlačítko **OK** pro potvrzení vytvoření cílový adresář.</span><span class="sxs-lookup"><span data-stu-id="176d0-237">Click **OK** to confirm creating the target directory.</span></span>
6. <span data-ttu-id="176d0-238">Výchozí nastavení je instalace základní a standardní balíčky.</span><span class="sxs-lookup"><span data-stu-id="176d0-238">The default setting is to install the Base and Standard packages.</span></span>  <span data-ttu-id="176d0-239">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="176d0-239">Click **Next**.</span></span>
7. <span data-ttu-id="176d0-240">Klikněte na tlačítko **Další** dvakrát a potom klikněte na **provádí**.</span><span class="sxs-lookup"><span data-stu-id="176d0-240">Click **Next** twice, and then click **Done**.</span></span>

<span data-ttu-id="176d0-241">**K instalaci ovladačů Phoenix**</span><span class="sxs-lookup"><span data-stu-id="176d0-241">**To install the Phoenix driver**</span></span>

<span data-ttu-id="176d0-242">Soubor jar phoenix ovladačů se nachází v clusteru HBase.</span><span class="sxs-lookup"><span data-stu-id="176d0-242">The phoenix driver jar file is located on the HBase cluster.</span></span> <span data-ttu-id="176d0-243">Cesta je podobný následujícímu podle verze:</span><span class="sxs-lookup"><span data-stu-id="176d0-243">The path is similar to the following based on the versions:</span></span>

    C:\apps\dist\phoenix-4.0.0.2.1.11.0-2316\phoenix-4.0.0.2.1.11.0-2316-client.jar
<span data-ttu-id="176d0-244">Je potřeba ho zkopírujte do pracovní stanice v [Instalační složka SQuirreL] / lib cestu.</span><span class="sxs-lookup"><span data-stu-id="176d0-244">You need to copy it to your workstation under the [SQuirreL installation folder]/lib path.</span></span>  <span data-ttu-id="176d0-245">Nejjednodušší způsob je pro připojení RDP do clusteru a potom pomocí souboru kopírování a vkládání (CTRL + C a CTRL + V) a zkopírujte ho do pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="176d0-245">The easiest way is to RDP into the cluster, and then use file copy/paste (CTRL+C and CTRL+V) to copy it to your workstation.</span></span>

<span data-ttu-id="176d0-246">**Chcete-li přidat ovladač Phoenix SQuirreL**</span><span class="sxs-lookup"><span data-stu-id="176d0-246">**To add a Phoenix driver to SQuirreL**</span></span>

1. <span data-ttu-id="176d0-247">Otevřete SQuirreL SQL klienta z pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="176d0-247">Open SQuirreL SQL Client from your workstation.</span></span>
2. <span data-ttu-id="176d0-248">Klikněte **ovladač** karty na levé straně.</span><span class="sxs-lookup"><span data-stu-id="176d0-248">Click the **Driver** tab on the left.</span></span>
3. <span data-ttu-id="176d0-249">Z **ovladače** nabídky, klikněte na tlačítko **nový ovladač**.</span><span class="sxs-lookup"><span data-stu-id="176d0-249">From the **Drivers** menu, click **New Driver**.</span></span>
4. <span data-ttu-id="176d0-250">Zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="176d0-250">Enter the following information:</span></span>

   * <span data-ttu-id="176d0-251">**Název**: Phoenix</span><span class="sxs-lookup"><span data-stu-id="176d0-251">**Name**: Phoenix</span></span>
   * <span data-ttu-id="176d0-252">**Příklad adresy URL**: jdbc:phoenix:zookeeper2.contoso-hbase-eu.f5.internal.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="176d0-252">**Example URL**: jdbc:phoenix:zookeeper2.contoso-hbase-eu.f5.internal.cloudapp.net</span></span>
   * <span data-ttu-id="176d0-253">**Název třídy**: org.apache.phoenix.jdbc.PhoenixDriver</span><span class="sxs-lookup"><span data-stu-id="176d0-253">**Class Name**: org.apache.phoenix.jdbc.PhoenixDriver</span></span>

     > [!WARNING]
     > <span data-ttu-id="176d0-254">Uživatel v adrese URL příklad všechny malá písmena.</span><span class="sxs-lookup"><span data-stu-id="176d0-254">User all lower case in the Example URL.</span></span> <span data-ttu-id="176d0-255">Můžete použít jejich úplné zookeeper kvora v případě, že jeden z nich je vypnutý.</span><span class="sxs-lookup"><span data-stu-id="176d0-255">You can use they full zookeeper quorum in case one of them is down.</span></span>  <span data-ttu-id="176d0-256">Názvy hostitelů jsou zookeeper0, zookeeper1 a zookeeper2.</span><span class="sxs-lookup"><span data-stu-id="176d0-256">The hostnames are zookeeper0, zookeeper1, and zookeeper2.</span></span>
     >
     >

     ![HDInsight HBase Phoenix SQuirreL ovladačů][img-squirrel-driver]
5. <span data-ttu-id="176d0-258">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="176d0-258">Click **OK**.</span></span>

<span data-ttu-id="176d0-259">**Chcete-li vytvořit alias do clusteru HBase**</span><span class="sxs-lookup"><span data-stu-id="176d0-259">**To create an alias to the HBase cluster**</span></span>

1. <span data-ttu-id="176d0-260">SQuirreL, klikněte **aliasy** karty na levé straně.</span><span class="sxs-lookup"><span data-stu-id="176d0-260">From SQuirreL, Click the **Aliases** tab on the left.</span></span>
2. <span data-ttu-id="176d0-261">Z **aliasy** nabídky, klikněte na tlačítko **nový Alias**.</span><span class="sxs-lookup"><span data-stu-id="176d0-261">From the **Aliases** menu, click **New Alias**.</span></span>
3. <span data-ttu-id="176d0-262">Zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="176d0-262">Enter the following information:</span></span>

   * <span data-ttu-id="176d0-263">**Název**: název clusteru HBase nebo libovolný název, který preferujete.</span><span class="sxs-lookup"><span data-stu-id="176d0-263">**Name**: The name of the HBase cluster or any name you prefer.</span></span>
   * <span data-ttu-id="176d0-264">**Ovladač**: Phoenix.</span><span class="sxs-lookup"><span data-stu-id="176d0-264">**Driver**: Phoenix.</span></span>  <span data-ttu-id="176d0-265">Toto musí odpovídat názvu ovladače, kterou jste vytvořili v posledním postupu.</span><span class="sxs-lookup"><span data-stu-id="176d0-265">This must match the driver name you created in the last procedure.</span></span>
   * <span data-ttu-id="176d0-266">**Adresa URL**: adresa URL se zkopíruje z konfiguraci ovladačů.</span><span class="sxs-lookup"><span data-stu-id="176d0-266">**URL**: The URL is copied from your driver configuration.</span></span> <span data-ttu-id="176d0-267">Zkontrolujte, zda uživatel všechny malá písmena.</span><span class="sxs-lookup"><span data-stu-id="176d0-267">Make sure to user all lower case.</span></span>
   * <span data-ttu-id="176d0-268">**Uživatelské jméno**: může být jakýkoli text.</span><span class="sxs-lookup"><span data-stu-id="176d0-268">**User name**: It can be any text.</span></span>  <span data-ttu-id="176d0-269">Vzhledem k tomu, že používáte připojení VPN, uživatelské jméno se nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="176d0-269">Because you use VPN connectivity here, the user name is not used at all.</span></span>
   * <span data-ttu-id="176d0-270">**Heslo**: může být jakýkoli text.</span><span class="sxs-lookup"><span data-stu-id="176d0-270">**Password**: It can be any text.</span></span>

     ![HDInsight HBase Phoenix SQuirreL ovladačů][img-squirrel-alias]
4. <span data-ttu-id="176d0-272">Klikněte na tlačítko **Test**.</span><span class="sxs-lookup"><span data-stu-id="176d0-272">Click **Test**.</span></span>
5. <span data-ttu-id="176d0-273">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="176d0-273">Click **Connect**.</span></span> <span data-ttu-id="176d0-274">Pokud se provede připojení, SQuirreL vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="176d0-274">When it makes the connection, SQuirreL looks like:</span></span>

    ![HBase Phoenix SQuirreL][img-squirrel]

<span data-ttu-id="176d0-276">**Při spuštění testu**</span><span class="sxs-lookup"><span data-stu-id="176d0-276">**To run a test**</span></span>

1. <span data-ttu-id="176d0-277">Klikněte na tlačítko **SQL** kartě právo vedle **objekty** kartě.</span><span class="sxs-lookup"><span data-stu-id="176d0-277">Click the **SQL** tab right next to the **Objects** tab.</span></span>
2. <span data-ttu-id="176d0-278">Zkopírujte a vložte následující kód:</span><span class="sxs-lookup"><span data-stu-id="176d0-278">Copy and paste the following code:</span></span>

        CREATE TABLE IF NOT EXISTS us_population (state CHAR(2) NOT NULL, city VARCHAR NOT NULL, population BIGINT  CONSTRAINT my_pk PRIMARY KEY (state, city))
3. <span data-ttu-id="176d0-279">Klikněte na tlačítko spustit.</span><span class="sxs-lookup"><span data-stu-id="176d0-279">Click the run button.</span></span>

    ![HBase Phoenix SQuirreL][img-squirrel-sql]
4. <span data-ttu-id="176d0-281">Přepnout zpět **objekty** kartě.</span><span class="sxs-lookup"><span data-stu-id="176d0-281">Switch back to the **Objects** tab.</span></span>
5. <span data-ttu-id="176d0-282">Rozbalte název aliasu a poté rozbalte **tabulky**.</span><span class="sxs-lookup"><span data-stu-id="176d0-282">Expand the alias name, and then expand **TABLE**.</span></span>  <span data-ttu-id="176d0-283">Zobrazí se nové tabulky uvedené v části.</span><span class="sxs-lookup"><span data-stu-id="176d0-283">You shall see the new table listed under.</span></span>

## <a name="next-steps"></a><span data-ttu-id="176d0-284">Další kroky</span><span class="sxs-lookup"><span data-stu-id="176d0-284">Next steps</span></span>
<span data-ttu-id="176d0-285">V tomto článku jste se naučili, jak používat Apache Phoenix v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="176d0-285">In this article, you have learned how to use Apache Phoenix in HDInsight.</span></span>  <span data-ttu-id="176d0-286">Další informace najdete v tématu</span><span class="sxs-lookup"><span data-stu-id="176d0-286">To learn more, see</span></span>

* <span data-ttu-id="176d0-287">[Přehled HBase ve službě HDInsight][hdinsight-hbase-overview]: HBase je NoSQL open source databáze Apache postavená na systému Hadoop, která poskytuje náhodný přístup a silnou konzistenci pro velké objemy nestrukturovaných a částečně strukturovaných dat.</span><span class="sxs-lookup"><span data-stu-id="176d0-287">[HDInsight HBase overview][hdinsight-hbase-overview]: HBase is an Apache, open-source, NoSQL database built on Hadoop that provides random access and strong consistency for large amounts of unstructured and semistructured data.</span></span>
* <span data-ttu-id="176d0-288">[Zřídit clustery HBase v Azure Virtual Network][hdinsight-hbase-provision-vnet]: integrace virtuální sítě, clustery HBase můžete nasadit do stejné virtuální síti jako aplikace tak, aby aplikace může komunikovat přímo s HBase.</span><span class="sxs-lookup"><span data-stu-id="176d0-288">[Provision HBase clusters on Azure Virtual Network][hdinsight-hbase-provision-vnet]: With virtual network integration, HBase clusters can be deployed to the same virtual network as your applications so that applications can communicate with HBase directly.</span></span>
* <span data-ttu-id="176d0-289">[Konfigurace replikace HBase v HDInsight](hdinsight-hbase-replication.md): Naučte se konfigurovat replikace HBase v rámci dvou datových centrech Azure.</span><span class="sxs-lookup"><span data-stu-id="176d0-289">[Configure HBase replication in HDInsight](hdinsight-hbase-replication.md): Learn how to configure HBase replication across two Azure datacenters.</span></span>
* <span data-ttu-id="176d0-290">[Analýza sentimentu Twitter s HBase v HDInsight][hbase-twitter-sentiment]: Další informace o postupu v reálném čase [postojích analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) velkých objemů dat pomocí HBase v clusteru Hadoop v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="176d0-290">[Analyze Twitter sentiment with HBase in HDInsight][hbase-twitter-sentiment]: Learn how to do real-time [sentiment analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) of big data by using HBase in a Hadoop cluster in HDInsight.</span></span>

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
