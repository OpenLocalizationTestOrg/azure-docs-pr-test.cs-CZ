---
title: "sada Toolkit pro IntelliJ - ladění aplikace vzdáleně na HDInsight Spark aaaAzure | Microsoft Docs"
description: "Další informace o použití nástrojů HDInsight v Azure nástrojů IntelliJ tooremotely ladění aplikací spuštěných na clustery HDInsight Spark prostřednictvím sítě vpn."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 55fb454f-c7dc-46de-a978-e242e9a94f4c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: ad67d23bf609d0a7afb38b2acb110062f8b27b39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-toolkit-for-intellij-toodebug-applications-remotely-on-hdinsight-spark-through-vpn"></a><span data-ttu-id="e1ac2-103">Použijte sadu nástrojů Azure pro aplikace toodebug IntelliJ vzdáleně na HDInsight Spark prostřednictvím sítě VPN</span><span class="sxs-lookup"><span data-stu-id="e1ac2-103">Use Azure Toolkit for IntelliJ toodebug applications remotely on HDInsight Spark through VPN</span></span>

<span data-ttu-id="e1ac2-104">Doporučujeme, abyste hello způsob ladění spark applicaltion vzdáleně přes ssh.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-104">We recommend hello way of debugging spark applicaltion remotely through ssh.</span></span> <span data-ttu-id="e1ac2-105">Pokyny najdete v tématu [vzdálené ladění aplikací Spark v clusteru HDInsight pomocí sady Azure Toolkit pro IntelliJ prostřednictvím SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).</span><span class="sxs-lookup"><span data-stu-id="e1ac2-105">For instructions, see [Remotely debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).</span></span>

<span data-ttu-id="e1ac2-106">Tento článek obsahuje podrobné pokyny k jak toouse hello nástroje HDInsight v Azure nástrojů pro IntelliJ toosubmit úlohy Spark v HDInsight Spark clusteru a pak ho vzdáleně ladit ze stolního počítače.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-106">This article provides step-by-step guidance on how toouse hello HDInsight Tools in Azure Toolkit for IntelliJ toosubmit a Spark job on HDInsight Spark cluster and then debug it remotely from your desktop computer.</span></span> <span data-ttu-id="e1ac2-107">toodo Ano, je třeba provést následující postup vysoké úrovně hello:</span><span class="sxs-lookup"><span data-stu-id="e1ac2-107">toodo so, you must perform hello following high-level steps:</span></span>

1. <span data-ttu-id="e1ac2-108">Vytvoření site-to-site nebo point-to-site virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-108">Create a site-to-site or point-to-site Azure Virtual Network.</span></span> <span data-ttu-id="e1ac2-109">Hello kroky v tomto dokumentu předpokládají, že používáte síť site-to-site.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-109">hello steps in this document assume that you use a site-to-site network.</span></span>
2. <span data-ttu-id="e1ac2-110">Vytvořte Spark cluster v Azure HDInsight, který je součástí hello site-to-site virtuální síť Azure.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-110">Create a Spark cluster in Azure HDInsight that is part of hello site-to-site Azure Virtual Network.</span></span>
3. <span data-ttu-id="e1ac2-111">Ověřte připojení hello mezi headnode hello clusteru a pracovní ploše.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-111">Verify hello connectivity between hello cluster headnode and your desktop.</span></span>
4. <span data-ttu-id="e1ac2-112">Vytvoření aplikace Scala v IntelliJ IDEA a nakonfigurovat ji pro vzdálené ladění.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-112">Create a Scala application in IntelliJ IDEA and configure it for remote debugging.</span></span>
5. <span data-ttu-id="e1ac2-113">Spuštění a ladění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-113">Run and debug hello application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e1ac2-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e1ac2-114">Prerequisites</span></span>
* <span data-ttu-id="e1ac2-115">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-115">An Azure subscription.</span></span> <span data-ttu-id="e1ac2-116">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="e1ac2-116">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="e1ac2-117">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-117">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="e1ac2-118">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="e1ac2-118">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
* <span data-ttu-id="e1ac2-119">Java Development kit Oracle.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-119">Oracle Java Development kit.</span></span> <span data-ttu-id="e1ac2-120">Můžete nainstalovat z [zde](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="e1ac2-120">You can install it from [here](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
* <span data-ttu-id="e1ac2-121">IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-121">IntelliJ IDEA.</span></span> <span data-ttu-id="e1ac2-122">Tento článek používá verzi 2017.1.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-122">This article uses version 2017.1.</span></span> <span data-ttu-id="e1ac2-123">Můžete nainstalovat z [zde](https://www.jetbrains.com/idea/download/).</span><span class="sxs-lookup"><span data-stu-id="e1ac2-123">You can install it from [here](https://www.jetbrains.com/idea/download/).</span></span>
* <span data-ttu-id="e1ac2-124">Nástroje HDInsight v Azure nástrojů pro IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-124">HDInsight Tools in Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="e1ac2-125">Nástroje HDInsight pro IntelliJ jsou k dispozici jako součást hello nástrojů Azure pro IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-125">HDInsight tools for IntelliJ are available as part of hello Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="e1ac2-126">Pokyny jak tooinstall hello nástrojů Azure, najdete v části [hello instalace nástrojů Azure pro IntelliJ](../azure-toolkit-for-intellij-installation.md).</span><span class="sxs-lookup"><span data-stu-id="e1ac2-126">For instructions on how tooinstall hello Azure Toolkit, see [Installing hello Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij-installation.md).</span></span>
* <span data-ttu-id="e1ac2-127">Přihlaste se k předplatnému Azure z IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-127">Log into your Azure Subscription from IntelliJ IDEA.</span></span> <span data-ttu-id="e1ac2-128">Postupujte podle pokynů hello [zde](hdinsight-apache-spark-intellij-tool-plugin.md).</span><span class="sxs-lookup"><span data-stu-id="e1ac2-128">Follow hello instructions [here](hdinsight-apache-spark-intellij-tool-plugin.md).</span></span>
* <span data-ttu-id="e1ac2-129">Při spouštění aplikací Spark Scala pro vzdálené ladění na počítači se systémem Windows, může získat výjimku, jak je popsáno v [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356) k tomu dojde kvůli chybějící tooa WinUtils.exe v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-129">While running Spark Scala application for remote debugging on a Windows computer, you might get an exception as explained in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356) that occurs due tooa missing WinUtils.exe on Windows.</span></span> <span data-ttu-id="e1ac2-130">toowork vyřešit tuto chybu, musíte [hello spustitelný soubor stáhnout odsud](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa umístění jako **C:\WinUtils\bin**.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-130">toowork around this error, you must [download hello executable from here](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa location like **C:\WinUtils\bin**.</span></span> <span data-ttu-id="e1ac2-131">Pak musíte přidat proměnnou prostředí **HADOOP_HOME** a nastavte hodnotu hello hello proměnné příliš**C\WinUtils**.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-131">You must then add an environment variable **HADOOP_HOME** and set hello value of hello variable too**C\WinUtils**.</span></span>

## <a name="step-1-create-an-azure-virtual-network"></a><span data-ttu-id="e1ac2-132">Krok 1: Vytvoření virtuální sítě Azure</span><span class="sxs-lookup"><span data-stu-id="e1ac2-132">Step 1: Create an Azure Virtual Network</span></span>
<span data-ttu-id="e1ac2-133">Postupujte podle pokynů hello z hello pod odkazy toocreate virtuální síť Azure a pak ověřte hello připojení mezi hello plochy a virtuální síti Azure.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-133">Follow hello instructions from hello below links toocreate an Azure Virtual Network and then verify hello connectivity between hello desktop and Azure Virtual Network.</span></span>

* [<span data-ttu-id="e1ac2-134">Vytvoření virtuální sítě s připojením VPN typu site-to-site pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e1ac2-134">Create a VNet with a site-to-site VPN connection using Azure Portal</span></span>](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [<span data-ttu-id="e1ac2-135">Vytvoření virtuální sítě s připojením VPN typu site-to-site pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="e1ac2-135">Create a VNet with a site-to-site VPN connection using PowerShell</span></span>](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* [<span data-ttu-id="e1ac2-136">Konfigurace připojení point-to-site tooa virtuální sítě pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="e1ac2-136">Configure a point-to-site connection tooa virtual network using PowerShell</span></span>](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

## <a name="step-2-create-an-hdinsight-spark-cluster"></a><span data-ttu-id="e1ac2-137">Krok 2: Vytvoření clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="e1ac2-137">Step 2: Create an HDInsight Spark cluster</span></span>
<span data-ttu-id="e1ac2-138">Měli byste také vytvořit cluster Apache Spark v Azure HDInsight, který je součástí hello virtuální síť Azure, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-138">You should also create an Apache Spark cluster on Azure HDInsight that is part of hello Azure Virtual Network that you created.</span></span> <span data-ttu-id="e1ac2-139">Použijte hello informace, které jsou k dispozici na [vytvořit systémem Linux clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="e1ac2-139">Use hello information available at [Create Linux-based clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span> <span data-ttu-id="e1ac2-140">Jako součást volitelné konfigurace vyberte hello virtuální síť Azure, kterou jste vytvořili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-140">As part of optional configuration, select hello Azure Virtual Network that you created in hello previous step.</span></span>

## <a name="step-3-verify-hello-connectivity-between-hello-cluster-headnode-and-your-desktop"></a><span data-ttu-id="e1ac2-141">Krok 3: Ověření hello připojení mezi headnode hello clusteru a pracovní plochy</span><span class="sxs-lookup"><span data-stu-id="e1ac2-141">Step 3: Verify hello connectivity between hello cluster headnode and your desktop</span></span>
1. <span data-ttu-id="e1ac2-142">Získáte IP adresu hello hello headnode.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-142">Get hello IP address of hello headnode.</span></span> <span data-ttu-id="e1ac2-143">Otevřete uživatelské rozhraní Ambari pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-143">Open Ambari UI for hello cluster.</span></span> <span data-ttu-id="e1ac2-144">V okně hello clusteru, klikněte na tlačítko **řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-144">From hello cluster blade, click **Dashboard**.</span></span>

    ![Hledání headnode IP adresy](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/launch-ambari-ui.png)
2. <span data-ttu-id="e1ac2-146">Z hello uživatelského rozhraní Ambari, v pravém horním rohu hello, klikněte na tlačítko **hostitele**.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-146">From hello Ambari UI, from hello top-right corner, click **Hosts**.</span></span>

    ![Hledání headnode IP adresy](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/ambari-hosts.png)
3. <span data-ttu-id="e1ac2-148">Zobrazí seznam headnodes, pracovní uzly a uzly zookeeper.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-148">You should see a list of headnodes, worker nodes, and zookeeper nodes.</span></span> <span data-ttu-id="e1ac2-149">Hello headnodes mít hello **hn*** předponu.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-149">hello headnodes have hello **hn*** prefix.</span></span> <span data-ttu-id="e1ac2-150">Klikněte na první headnode hello.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-150">Click hello first headnode.</span></span>

    ![Hledání headnode IP adresy](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/cluster-headnodes.png)
4. <span data-ttu-id="e1ac2-152">V dolní části hello hello stránky, které se otevře z hello **Souhrn** pole kopie hello IP adresu hello headnode a název hostitele hello.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-152">At hello bottom of hello page that opens, from hello **Summary** box, copy hello IP address of hello headnode and hello host name.</span></span>

    ![Hledání headnode IP adresy](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/headnode-ip-address.png)
5. <span data-ttu-id="e1ac2-154">Zahrnout hello IP adresu a název hostitele hello hello headnode toohello **hostitele** soubor na počítači hello z kam chcete toorun a vzdáleně ladit úlohy Spark hello.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-154">Include hello IP address and hello host name of hello headnode toohello **hosts** file on hello computer from where you want toorun and remotely debug hello Spark jobs.</span></span> <span data-ttu-id="e1ac2-155">To vám umožní toocommunicate s headnode hello pomocí hello IP adresu a také hello název hostitele.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-155">This will enable you toocommunicate with hello headnode using hello IP address as well as hello hostname.</span></span>

   1. <span data-ttu-id="e1ac2-156">Otevřete Poznámkový blok se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-156">Open a notepad with elevated permissions.</span></span> <span data-ttu-id="e1ac2-157">Z nabídky Soubor hello, klikněte na tlačítko **otevřete** a pak přejděte toohello umístění souboru hostitelů hello.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-157">From hello file menu, click **Open** and then navigate toohello location of hello hosts file.</span></span> <span data-ttu-id="e1ac2-158">Na počítači s Windows, je `C:\Windows\System32\Drivers\etc\hosts`.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-158">On a Windows computer, it is `C:\Windows\System32\Drivers\etc\hosts`.</span></span>
   2. <span data-ttu-id="e1ac2-159">Přidejte následující toohello hello **hostitele** souboru.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-159">Add hello following toohello **hosts** file.</span></span>

           # For headnode0
           192.xxx.xx.xx hn0-nitinp
           192.xxx.xx.xx hn0-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net

           # For headnode1
           192.xxx.xx.xx hn1-nitinp
           192.xxx.xx.xx hn1-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net
6. <span data-ttu-id="e1ac2-160">Z hello počítači, který je připojen toohello Azure Virtual Network, který je používán hello clusteru HDInsight ověřte, že může odeslat příkaz ping obou hello headnodes pomocí hello IP adresu a také hello název hostitele.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-160">From hello computer that you connected toohello Azure Virtual Network that is used by hello HDInsight cluster, verify that you can ping both hello headnodes using hello IP address as well as hello hostname.</span></span>
7. <span data-ttu-id="e1ac2-161">SSH do headnode hello clusteru pomocí pokynů hello [clusteru HDInsight tooan připojit pomocí protokolu SSH](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="e1ac2-161">SSH into hello cluster headnode using hello instructions at [Connect tooan HDInsight cluster using SSH](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span> <span data-ttu-id="e1ac2-162">Z clusteru headnode hello odeslat příkaz ping hello IP adresu hello stolní počítač.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-162">From hello cluster headnode, ping hello IP address of hello desktop computer.</span></span> <span data-ttu-id="e1ac2-163">Měli byste otestovat připojení tooboth hello IP adresy přiřazené toohello počítače, jeden pro hello síťové připojení a hello jiné pro hello virtuální síť Azure, která hello počítač je připojený k.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-163">You should test connectivity tooboth hello IP addresses assigned toohello computer, one for hello network connection and hello other for hello Azure Virtual Network that hello computer is connected to.</span></span>
8. <span data-ttu-id="e1ac2-164">Opakujte kroky hello pro hello také jiné headnode.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-164">Repeat hello steps for hello other headnode as well.</span></span>

## <a name="step-4-create-a-spark-scala-application-using-hello-hdinsight-tools-in-azure-toolkit-for-intellij-and-configure-it-for-remote-debugging"></a><span data-ttu-id="e1ac2-165">Krok 4: Vytvoření aplikace Spark Scala pomocí hello nástroje HDInsight pro IntelliJ v Azure nástrojů a nakonfigurovat ji pro vzdálené ladění</span><span class="sxs-lookup"><span data-stu-id="e1ac2-165">Step 4: Create a Spark Scala application using hello HDInsight Tools in Azure Toolkit for IntelliJ and configure it for remote debugging</span></span>
1. <span data-ttu-id="e1ac2-166">Spusťte IntelliJ IDEA a vytvoření nového projektu.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-166">Launch IntelliJ IDEA and create a new project.</span></span> <span data-ttu-id="e1ac2-167">V hello projektu dialogové okno Nový, zkontrolujte hello následující možnosti a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-167">In hello new project dialog box, make hello following choices, and then click **Next**.</span></span>

    ![Vytvoření aplikací Spark Scala](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-hdi-scala-app.png)

   * <span data-ttu-id="e1ac2-169">V levém podokně hello, vyberte **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-169">From hello left pane, select **HDInsight**.</span></span>
   * <span data-ttu-id="e1ac2-170">V pravém podokně hello vyberte **Spark v HDInsight (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-170">From hello right pane, select **Spark on HDInsight (Scala)**.</span></span>
   * <span data-ttu-id="e1ac2-171">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-171">Click **Next**.</span></span>
2. <span data-ttu-id="e1ac2-172">V dalším okně hello zadejte hello následujících podrobností projektu a pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-172">In hello next window, provide hello following project details, and then click **Finish**.</span></span>  
   - <span data-ttu-id="e1ac2-173">Zadejte název projektu a umístění projektu.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-173">Provide a project name and project location.</span></span>
   - <span data-ttu-id="e1ac2-174">Pro **SDK projektu**, použijte Java 1.8 pro cluster spark 2.x, Java 1.7 pro 1.x clusteru spark.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-174">For **Project SDK**, Use Java 1.8 for spark 2.x cluster, Java 1.7 for spark 1.x cluster.</span></span>
   - <span data-ttu-id="e1ac2-175">Pro **Spark verze**, Průvodce vytvořením projektu Scala integruje správnou verzi sady SDK Spark a Scala SDK.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-175">For **Spark Version**, Scala project creation wizard integrates proper version for Spark SDK and Scala SDK.</span></span> <span data-ttu-id="e1ac2-176">Pokud verze clusteru spark hello nižší 2.0, zvolte spark 1.x.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-176">If hello spark cluster version is lower 2.0, choose spark 1.x.</span></span> <span data-ttu-id="e1ac2-177">Jinak měli byste vybrat spark2.x.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-177">Otherwise, you should select spark2.x.</span></span> <span data-ttu-id="e1ac2-178">Tento příklad používá Spark2.0.2 (Scala 2.11.8).</span><span class="sxs-lookup"><span data-stu-id="e1ac2-178">This example uses Spark2.0.2(Scala 2.11.8).</span></span>
       <span data-ttu-id="e1ac2-179">![Vytvoření aplikací Spark Scala](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-scala-project-details.png)</span><span class="sxs-lookup"><span data-stu-id="e1ac2-179">![Create Spark Scala application](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-scala-project-details.png)</span></span>
  
3. <span data-ttu-id="e1ac2-180">Hello Spark projektu automaticky vytvoří artefakt.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-180">hello Spark project will automatically create an artifact for you.</span></span> <span data-ttu-id="e1ac2-181">toosee hello artefaktů, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-181">toosee hello artifact, follow these steps.</span></span>

   1. <span data-ttu-id="e1ac2-182">Z hello **soubor** nabídky, klikněte na tlačítko **strukturu projektu**.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-182">From hello **File** menu, click **Project Structure**.</span></span>
   2. <span data-ttu-id="e1ac2-183">V hello **strukturu projektu** dialogové okno, klikněte na tlačítko **artefakty** toosee hello výchozí artefaktů, který je vytvořen.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-183">In hello **Project Structure** dialog box, click **Artifacts** toosee hello default artifact that is created.</span></span>
   <span data-ttu-id="e1ac2-184">![Vytvoření JAR](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/default-artifact.png)</span><span class="sxs-lookup"><span data-stu-id="e1ac2-184">![Create JAR](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/default-artifact.png)</span></span>

      <span data-ttu-id="e1ac2-185">Můžete také vytvořit vlastní artefaktů bly kliknutím na hello  **+**  ikonu, vyznačené na obrázku hello výše.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-185">You can also create your own artifact bly clicking on hello **+** icon, highlighted in hello image above.</span></span>

4. <span data-ttu-id="e1ac2-186">Přidání knihovny tooyour projektu.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-186">Add libraries tooyour project.</span></span> <span data-ttu-id="e1ac2-187">Klikněte pravým tlačítkem na název projektu hello ve stromu projektu hello tooadd knihovny a pak klikněte na **otevřete nastavení modulu**.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-187">tooadd a library, right-click hello project name in hello project tree, and then click **Open Module Settings**.</span></span> <span data-ttu-id="e1ac2-188">V hello **strukturu projektu** dialogové okno, v levém podokně hello, klikněte na tlačítko **knihovny**, klikněte na symbol hello (+) a pak klikněte na tlačítko **z Maven**.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-188">In hello **Project Structure** dialog box, from hello left pane, click **Libraries**, click hello (+) symbol, and then click **From Maven**.</span></span>

    ![Přidání knihovny](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/add-library.png)

    <span data-ttu-id="e1ac2-190">V hello **knihovny stáhnout z úložiště Maven** dialogové okno, vyhledávání a přidejte následující knihovny hello.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-190">In hello **Download Library from Maven Repository** dialog box, search and add hello following libraries.</span></span>

   * `org.scalatest:scalatest_2.10:2.2.1`
   * `org.apache.hadoop:hadoop-azure:2.7.1`
5. <span data-ttu-id="e1ac2-191">Kopírování `yarn-site.xml` a `core-site.xml` z hello clusteru headnode a přidejte ji toohello projektu.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-191">Copy `yarn-site.xml` and `core-site.xml` from hello cluster headnode and add it toohello project.</span></span> <span data-ttu-id="e1ac2-192">Použijte následující příkazy toocopy hello soubory hello.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-192">Use hello following commands toocopy hello files.</span></span> <span data-ttu-id="e1ac2-193">Můžete použít [emulaci](https://cygwin.com/install.html) toorun hello následující `scp` příkazy toocopy hello soubory z clusteru headnodes hello.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-193">You can use [Cygwin](https://cygwin.com/install.html) toorun hello following `scp` commands toocopy hello files from hello cluster headnodes.</span></span>

        scp <ssh user name>@<headnode IP address or host name>://etc/hadoop/conf/core-site.xml .

    <span data-ttu-id="e1ac2-194">Protože jsme již přidali hello clusteru headnode IP adresy a názvy hostitelů fo hello souboru hostitelů na ploše hello, můžeme použít hello **spojovací bod služby** příkazy v hello následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-194">Because we already added hello cluster headnode IP address and hostnames fo hello hosts file on hello desktop, we can use hello **scp** commands in hello following manner.</span></span>

        scp sshuser@hn0-nitinp:/etc/hadoop/conf/core-site.xml .
        scp sshuser@hn0-nitinp:/etc/hadoop/conf/yarn-site.xml .

    <span data-ttu-id="e1ac2-195">Přidat tyto soubory tooyour projekt zkopírováním pod hello **/src** složky ve stromu vašeho projektu, například `<your project directory>\src`.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-195">Add these files tooyour project by copying them under hello **/src** folder in your project tree, for example `<your project directory>\src`.</span></span>
6. <span data-ttu-id="e1ac2-196">Aktualizace hello `core-site.xml` toomake hello následující změny.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-196">Update hello `core-site.xml` toomake hello following changes.</span></span>

   1. <span data-ttu-id="e1ac2-197">`core-site.xml`zahrnuje hello šifrované klíče toohello úložiště účet přidružený ke clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-197">`core-site.xml` includes hello encrypted key toohello storage account associated with hello cluster.</span></span> <span data-ttu-id="e1ac2-198">V hello `core-site.xml` , že jste přidali toohello projektu, nahraďte hello zašifrovaný klíč s hello skutečné úložiště klíč přidružený k hello výchozí účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-198">In hello `core-site.xml` that you added toohello project, replace hello encrypted key with hello actual storage key associated with hello default storage account.</span></span> <span data-ttu-id="e1ac2-199">V tématu [Správa přístupových klíčů úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="e1ac2-199">See [Manage your storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>

           <property>
                 <name>fs.azure.account.key.hdistoragecentral.blob.core.windows.net</name>
                 <value>access-key-associated-with-the-account</value>
           </property>
   2. <span data-ttu-id="e1ac2-200">Odebrat hello následujících položek z hello `core-site.xml`.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-200">Remove hello following entries from hello `core-site.xml`.</span></span>

           <property>
                 <name>fs.azure.account.keyprovider.hdistoragecentral.blob.core.windows.net</name>
                 <value>org.apache.hadoop.fs.azure.ShellDecryptionKeyProvider</value>
           </property>

           <property>
                 <name>fs.azure.shellkeyprovider.script</name>
                 <value>/usr/lib/python2.7/dist-packages/hdinsight_common/decrypt.sh</value>
           </property>

           <property>
                 <name>net.topology.script.file.name</name>
                 <value>/etc/hadoop/conf/topology_script.py</value>
           </property>
   3. <span data-ttu-id="e1ac2-201">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-201">Save hello file.</span></span>
7. <span data-ttu-id="e1ac2-202">Přidání hello hlavní třídy pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-202">Add hello Main class for your application.</span></span> <span data-ttu-id="e1ac2-203">Z hello **Project Exploreru**, klikněte pravým tlačítkem na **src**, bod příliš**nový**a potom klikněte na **Scala třída**.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-203">From hello **Project Explorer**, right-click **src**, point too**New**, and then click **Scala class**.</span></span>

    ![Přidejte zdrojový kód](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-spark-scala-code.png)
8. <span data-ttu-id="e1ac2-205">V hello **vytvořte novou třídu Scala** dialogovém okně zadejte název, **druhu** vyberte **objekt**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-205">In hello **Create New Scala Class** dialog box, provide a name, for **Kind** select **Object**, and then click **OK**.</span></span>

    ![Přidejte zdrojový kód](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-spark-scala-code-object.png)
9. <span data-ttu-id="e1ac2-207">V hello `MyClusterAppMain.scala` souboru, vložte následující kód hello.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-207">In hello `MyClusterAppMain.scala` file, paste hello following code.</span></span> <span data-ttu-id="e1ac2-208">Tento kód vytvoří hello Spark kontextu a spustí `executeJob` metoda z hello `SparkSample` objektu.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-208">This code creates hello Spark context and launches an `executeJob` method from hello `SparkSample` object.</span></span>

        import org.apache.spark.{SparkConf, SparkContext}

        object SparkSampleMain {
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("SparkSample")
                                      .set("spark.hadoop.validateOutputSpecs", "false")
            val sc = new SparkContext(conf)

            SparkSample.executeJob(sc,
                                   "wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv",
                                   "wasb:///HVACOut")
          }
        }

10. <span data-ttu-id="e1ac2-209">Opakujte kroky 8 a 9 výše tooadd názvem nového objektu Scala `SparkSample`.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-209">Repeat steps 8 and 9 above tooadd a new Scala object called `SparkSample`.</span></span> <span data-ttu-id="e1ac2-210">Třída toothis přidejte následující kód hello.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-210">toothis class add hello following code.</span></span> <span data-ttu-id="e1ac2-211">Tento kód čte hello data z hello HVAC.csv (k dispozici na všech clusterech HDInsight Spark), načte hello řádky, které mají pouze jednu číslici v hello sedmého sloupci hello sdíleného svazku clusteru a zapíše výstup hello příliš**/HVACOut** pod výchozí hello Kontejner úložiště pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-211">This code reads hello data from hello HVAC.csv (available on all HDInsight Spark clusters), retrieves hello rows that only have one digit in hello seventh column in hello CSV, and writes hello output too**/HVACOut** under hello default storage container for hello cluster.</span></span>

        import org.apache.spark.SparkContext

        object SparkSample {
         def executeJob (sc: SparkContext, input: String, output: String): Unit = {
           val rdd = sc.textFile(input)

           //find hello rows which have only one digit in hello 7th column in hello CSV
           val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)

           val s = sc.parallelize(rdd.take(5)).cartesian(rdd).count()
           println(s)

           rdd1.saveAsTextFile(output)
           //rdd1.collect().foreach(println)
         }
        }
11. <span data-ttu-id="e1ac2-212">Opakujte kroky 8 a 9 výše tooadd novou třídu s názvem `RemoteClusterDebugging`.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-212">Repeat steps 8 and 9 above tooadd a new class called `RemoteClusterDebugging`.</span></span> <span data-ttu-id="e1ac2-213">Tato třída implementuje hello Spark test framework, který slouží k ladění aplikací.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-213">This class implements hello Spark test framework that is used for debugging applications.</span></span> <span data-ttu-id="e1ac2-214">Přidejte následující kód toohello hello `RemoteClusterDebugging` třídy.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-214">Add hello following code toohello `RemoteClusterDebugging` class.</span></span>

        import org.apache.spark.{SparkConf, SparkContext}
        import org.scalatest.FunSuite

        class RemoteClusterDebugging extends FunSuite {

         test("Remote run") {
           val conf = new SparkConf().setAppName("SparkSample")
                                     .setMaster("yarn-client")
                                     .set("spark.yarn.am.extraJavaOptions", "-Dhdp.version=2.4")
                                     .set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")
                                     .setJars(Seq("""C:\workspace\IdeaProjects\MyClusterApp\out\artifacts\MyClusterApp_DefaultArtifact\default_artifact.jar"""))
                                     .set("spark.hadoop.validateOutputSpecs", "false")
           val sc = new SparkContext(conf)

           SparkSample.executeJob(sc,
             "wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv",
             "wasb:///HVACOut")
         }
        }

     <span data-ttu-id="e1ac2-215">Několik důležitých věcí toonote tady:</span><span class="sxs-lookup"><span data-stu-id="e1ac2-215">Couple of important things toonote here:</span></span>

   * <span data-ttu-id="e1ac2-216">Pro `.set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")`, zkontrolujte, zda je k dispozici v úložišti clusteru hello v zadané cestě hello hello Spark sestavení JAR.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-216">For `.set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")`, make sure hello Spark assembly JAR is available on hello cluster storage at hello specified path.</span></span>
   * <span data-ttu-id="e1ac2-217">Pro `setJars`, zadejte hello umístění, kde bude vytvořen jar artefaktů hello.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-217">For `setJars`, specify hello location where hello artifact jar will be created.</span></span> <span data-ttu-id="e1ac2-218">Obvykle je `<Your IntelliJ project directory>\out\<project name>_DefaultArtifact\default_artifact.jar`.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-218">Typically it is `<Your IntelliJ project directory>\out\<project name>_DefaultArtifact\default_artifact.jar`.</span></span>
12. <span data-ttu-id="e1ac2-219">V hello `RemoteClusterDebugging` třídy, klikněte pravým tlačítkem na hello `test` – klíčové slovo a vyberte **vytvořit konfigurace RemoteClusterDebugging**.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-219">In hello `RemoteClusterDebugging` class, right-click hello `test` keyword and select **Create RemoteClusterDebugging Configuration**.</span></span>

    ![Vytvoření konfigurace vzdáleného](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-remote-config.png)

13. <span data-ttu-id="e1ac2-221">V dialogovém okně hello, zadejte název pro konfiguraci hello a vyberte hello **testovací druh** jako **název testovacího**.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-221">In hello dialog box, provide a name for hello configuration, and select hello **Test kind** as **Test name**.</span></span> <span data-ttu-id="e1ac2-222">Nechat všechny ostatní hodnoty jako výchozí, klikněte na **použít**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-222">Leave all other values as default, click **Apply**, and then click **OK**.</span></span>

    ![Vytvoření konfigurace vzdáleného](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/provide-config-value.png)
14. <span data-ttu-id="e1ac2-224">Teď byste měli vidět **vzdálené spuštění** konfigurace rozevírací seznam v řádku nabídek hello.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-224">You should now see a **Remote Run** configuration drop-down in hello menu bar.</span></span>

    ![Vytvoření konfigurace vzdáleného](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/config-run.png)

## <a name="step-5-run-hello-application-in-debug-mode"></a><span data-ttu-id="e1ac2-226">Krok 5: Hello aplikaci spustit v režimu ladění</span><span class="sxs-lookup"><span data-stu-id="e1ac2-226">Step 5: Run hello application in debug mode</span></span>
1. <span data-ttu-id="e1ac2-227">Otevřete v projektu IntelliJ IDEA `SparkSample.scala` a vytvořit další rdd1 too'val zarážek '.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-227">In your IntelliJ IDEA project, open `SparkSample.scala` and create a breakpoint next too\`val rdd1'.</span></span> <span data-ttu-id="e1ac2-228">V místní nabídce hello pro vytvoření zarážku vyberte **řádku funkce executeJob**.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-228">In hello pop-up menu for creating a breakpoint, select **line in function executeJob**.</span></span>

    ![Přidat zarážky](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-breakpoint.png)
2. <span data-ttu-id="e1ac2-230">Klikněte na tlačítko hello **ladění spustit** tlačítko Další toohello **vzdálené spuštění** toostart rozevíracího seznamu configuration spuštěna aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-230">Click hello **Debug Run** button next toohello **Remote Run** configuration drop-down toostart running hello application.</span></span>

    ![Spuštění programu hello v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-run-mode.png)
3. <span data-ttu-id="e1ac2-232">Při spuštění programu hello dosáhne hello zarážek, měli byste vidět **ladicí program** ve spodním podokně hello.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-232">When hello program execution reaches hello breakpoint, you should see a **Debugger** tab in hello bottom pane.</span></span>

    ![Spuštění programu hello v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch.png)
4. <span data-ttu-id="e1ac2-234">Klikněte na tlačítko hello (**+**) ikona tooadd sledovat, jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-234">Click hello (**+**) icon tooadd a watch as shown in hello image below.</span></span>

    ![Spuštění programu hello v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch-variable.png)

    <span data-ttu-id="e1ac2-236">Sem, protože aplikace hello překročila před proměnnou hello `rdd1` byla vytvořena pomocí tohoto sledování, uvidíte, jaké jsou hello prvních 5 řádků v proměnné hello `rdd`.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-236">Here, because hello application broke before hello variable `rdd1` was created, using this watch we can see what are hello first 5 rows in hello variable `rdd`.</span></span> <span data-ttu-id="e1ac2-237">Stiskněte **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-237">Press **ENTER**.</span></span>

    ![Spuštění programu hello v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch-variable-value.png)

    <span data-ttu-id="e1ac2-239">Co se zobrazí v hello obrázku výše je, že v době běhu můžete dotazovat terrabytes dat a ladění jak postupuje vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-239">What you see in hello image above is that at runtime, you could query terrabytes of data and debug how your application progresses.</span></span> <span data-ttu-id="e1ac2-240">Například v hello výstup hello obrázku výše, uvidíte této hello první řádek výstupu hello je záhlaví.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-240">For example, in hello output shown in hello image above, you can see that hello first row of hello output is a header.</span></span> <span data-ttu-id="e1ac2-241">Řádek záhlaví tooskip hello vaší aplikace kódu můžete upravit na základě toho se v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-241">Based on this, you can modify your application code tooskip hello header row if required.</span></span>
5. <span data-ttu-id="e1ac2-242">Nyní můžete kliknout na hello **obnovit Program** tooproceed ikona s vaší aplikací spustit.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-242">You can now click hello **Resume Program** icon tooproceed with your application run.</span></span>

    ![Spuštění programu hello v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-continue-run.png)
6. <span data-ttu-id="e1ac2-244">Pokud aplikace hello úspěšně dokončí, měli byste vidět výstup jako následující hello.</span><span class="sxs-lookup"><span data-stu-id="e1ac2-244">If hello application completes successfully, you should see an output like hello following.</span></span>

    ![Spuštění programu hello v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-complete.png)

## <span data-ttu-id="e1ac2-246"><a name="seealso"></a>Viz také</span><span class="sxs-lookup"><span data-stu-id="e1ac2-246"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="e1ac2-247">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="e1ac2-247">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="demo"></a><span data-ttu-id="e1ac2-248">Ukázka</span><span class="sxs-lookup"><span data-stu-id="e1ac2-248">Demo</span></span>
* <span data-ttu-id="e1ac2-249">Vytvoření projektu Scala (Video): [vytvoření aplikací Spark Scala](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="e1ac2-249">Create Scala Project (Video): [Create Spark Scala Applications](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span></span>
* <span data-ttu-id="e1ac2-250">Vzdálené ladění (Video): [pomocí nástrojů Azure pro IntelliJ toodebug aplikací Spark vzdáleně na clusteru HDInsight](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="e1ac2-250">Remote Debug (Video): [Use Azure Toolkit for IntelliJ toodebug Spark applications remotely on HDInsight Cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span></span>

### <a name="scenarios"></a><span data-ttu-id="e1ac2-251">Scénáře</span><span class="sxs-lookup"><span data-stu-id="e1ac2-251">Scenarios</span></span>
* [<span data-ttu-id="e1ac2-252">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="e1ac2-252">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="e1ac2-253">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="e1ac2-253">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="e1ac2-254">Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight</span><span class="sxs-lookup"><span data-stu-id="e1ac2-254">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="e1ac2-255">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="e1ac2-255">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="e1ac2-256">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="e1ac2-256">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="e1ac2-257">Vytvoření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="e1ac2-257">Create and run applications</span></span>
* [<span data-ttu-id="e1ac2-258">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="e1ac2-258">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="e1ac2-259">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="e1ac2-259">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="e1ac2-260">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="e1ac2-260">Tools and extensions</span></span>
* [<span data-ttu-id="e1ac2-261">Používat nástroje HDInsight pro IntelliJ toocreate v nástrojů Azure a odesílání aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="e1ac2-261">Use HDInsight Tools in Azure Toolkit for IntelliJ toocreate and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="e1ac2-262">Použití nástrojů Azure pro IntelliJ toodebug aplikací Spark vzdáleně přes SSH</span><span class="sxs-lookup"><span data-stu-id="e1ac2-262">Use Azure Toolkit for IntelliJ toodebug Spark applications remotely through SSH</span></span>](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [<span data-ttu-id="e1ac2-263">Použití nástroje HDInsight pro IntelliJ s Hortonworks karanténě</span><span class="sxs-lookup"><span data-stu-id="e1ac2-263">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [<span data-ttu-id="e1ac2-264">Použití nástrojů HDInsight v Azure nástrojů Eclipse toocreate Spark aplikací</span><span class="sxs-lookup"><span data-stu-id="e1ac2-264">Use HDInsight Tools in Azure Toolkit for Eclipse toocreate Spark applications</span></span>](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [<span data-ttu-id="e1ac2-265">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="e1ac2-265">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="e1ac2-266">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="e1ac2-266">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="e1ac2-267">Použití externích balíčků s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="e1ac2-267">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="e1ac2-268">Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="e1ac2-268">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="e1ac2-269">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="e1ac2-269">Manage resources</span></span>
* [<span data-ttu-id="e1ac2-270">Správa prostředků hello cluster Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="e1ac2-270">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="e1ac2-271">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="e1ac2-271">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
