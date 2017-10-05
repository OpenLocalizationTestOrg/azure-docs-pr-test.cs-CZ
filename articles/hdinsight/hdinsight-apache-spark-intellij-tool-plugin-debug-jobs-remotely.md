---
title: "Azure nástrojů pro IntelliJ - ladění aplikace vzdáleně na HDInsight Spark | Microsoft Docs"
description: "Další informace o použití nástrojů HDInsight v Azure nástrojů pro IntelliJ pro vzdálené ladění aplikací běžících na HDInsight Spark clusterů prostřednictvím sítě vpn."
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
ms.openlocfilehash: 5ce282aac94d0f22ea587cbe4005819310e23b1f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-toolkit-for-intellij-to-debug-applications-remotely-on-hdinsight-spark-through-vpn"></a><span data-ttu-id="aba36-103">Použití nástrojů Azure pro IntelliJ k ladění aplikací vzdáleně na HDInsight Spark prostřednictvím sítě VPN</span><span class="sxs-lookup"><span data-stu-id="aba36-103">Use Azure Toolkit for IntelliJ to debug applications remotely on HDInsight Spark through VPN</span></span>

<span data-ttu-id="aba36-104">Doporučujeme, abyste způsob ladění spark applicaltion vzdáleně přes ssh.</span><span class="sxs-lookup"><span data-stu-id="aba36-104">We recommend the way of debugging spark applicaltion remotely through ssh.</span></span> <span data-ttu-id="aba36-105">Pokyny najdete v tématu [vzdálené ladění aplikací Spark v clusteru HDInsight pomocí sady Azure Toolkit pro IntelliJ prostřednictvím SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).</span><span class="sxs-lookup"><span data-stu-id="aba36-105">For instructions, see [Remotely debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).</span></span>

<span data-ttu-id="aba36-106">Tento článek obsahuje podrobné pokyny o tom, jak používat nástroje HDInsight v Azure nástrojů pro IntelliJ odeslat úlohu Spark v HDInsight Spark clusteru a pak ho vzdáleně ladit ze stolního počítače.</span><span class="sxs-lookup"><span data-stu-id="aba36-106">This article provides step-by-step guidance on how to use the HDInsight Tools in Azure Toolkit for IntelliJ to submit a Spark job on HDInsight Spark cluster and then debug it remotely from your desktop computer.</span></span> <span data-ttu-id="aba36-107">Chcete-li to provést, musíte provést následující postup vysoké úrovně:</span><span class="sxs-lookup"><span data-stu-id="aba36-107">To do so, you must perform the following high-level steps:</span></span>

1. <span data-ttu-id="aba36-108">Vytvoření site-to-site nebo point-to-site virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="aba36-108">Create a site-to-site or point-to-site Azure Virtual Network.</span></span> <span data-ttu-id="aba36-109">Kroky v tomto dokumentu předpokládají, že používáte síť site-to-site.</span><span class="sxs-lookup"><span data-stu-id="aba36-109">The steps in this document assume that you use a site-to-site network.</span></span>
2. <span data-ttu-id="aba36-110">Vytvořte Spark cluster v Azure HDInsight, který je součástí virtuální sítě Azure site-to-site.</span><span class="sxs-lookup"><span data-stu-id="aba36-110">Create a Spark cluster in Azure HDInsight that is part of the site-to-site Azure Virtual Network.</span></span>
3. <span data-ttu-id="aba36-111">Zkontrolujte připojení mezi headnode clusteru a pracovní ploše.</span><span class="sxs-lookup"><span data-stu-id="aba36-111">Verify the connectivity between the cluster headnode and your desktop.</span></span>
4. <span data-ttu-id="aba36-112">Vytvoření aplikace Scala v IntelliJ IDEA a nakonfigurovat ji pro vzdálené ladění.</span><span class="sxs-lookup"><span data-stu-id="aba36-112">Create a Scala application in IntelliJ IDEA and configure it for remote debugging.</span></span>
5. <span data-ttu-id="aba36-113">Spuštění a ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="aba36-113">Run and debug the application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aba36-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="aba36-114">Prerequisites</span></span>
* <span data-ttu-id="aba36-115">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="aba36-115">An Azure subscription.</span></span> <span data-ttu-id="aba36-116">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="aba36-116">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="aba36-117">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="aba36-117">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="aba36-118">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="aba36-118">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
* <span data-ttu-id="aba36-119">Java Development kit Oracle.</span><span class="sxs-lookup"><span data-stu-id="aba36-119">Oracle Java Development kit.</span></span> <span data-ttu-id="aba36-120">Můžete nainstalovat z [zde](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="aba36-120">You can install it from [here](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
* <span data-ttu-id="aba36-121">IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="aba36-121">IntelliJ IDEA.</span></span> <span data-ttu-id="aba36-122">Tento článek používá verzi 2017.1.</span><span class="sxs-lookup"><span data-stu-id="aba36-122">This article uses version 2017.1.</span></span> <span data-ttu-id="aba36-123">Můžete nainstalovat z [zde](https://www.jetbrains.com/idea/download/).</span><span class="sxs-lookup"><span data-stu-id="aba36-123">You can install it from [here](https://www.jetbrains.com/idea/download/).</span></span>
* <span data-ttu-id="aba36-124">Nástroje HDInsight v Azure nástrojů pro IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="aba36-124">HDInsight Tools in Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="aba36-125">Nástroje HDInsight pro IntelliJ jsou k dispozici jako součást sady nástrojů Azure pro IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="aba36-125">HDInsight tools for IntelliJ are available as part of the Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="aba36-126">Pokyny k instalaci sady nástrojů Azure najdete v tématu [instalace sady Toolkit Azure pro IntelliJ](../azure-toolkit-for-intellij-installation.md).</span><span class="sxs-lookup"><span data-stu-id="aba36-126">For instructions on how to install the Azure Toolkit, see [Installing the Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij-installation.md).</span></span>
* <span data-ttu-id="aba36-127">Přihlaste se k předplatnému Azure z IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="aba36-127">Log into your Azure Subscription from IntelliJ IDEA.</span></span> <span data-ttu-id="aba36-128">Postupujte podle pokynů [zde](hdinsight-apache-spark-intellij-tool-plugin.md).</span><span class="sxs-lookup"><span data-stu-id="aba36-128">Follow the instructions [here](hdinsight-apache-spark-intellij-tool-plugin.md).</span></span>
* <span data-ttu-id="aba36-129">Při spouštění aplikací Spark Scala pro vzdálené ladění na počítači se systémem Windows, může získat výjimku, jak je popsáno v [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356) k tomu dojde z důvodu chybějící WinUtils.exe v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="aba36-129">While running Spark Scala application for remote debugging on a Windows computer, you might get an exception as explained in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356) that occurs due to a missing WinUtils.exe on Windows.</span></span> <span data-ttu-id="aba36-130">Chcete-li tuto chybu vyřešit, je potřeba [spustitelný soubor stáhnout odsud](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) do umístění, jako je **C:\WinUtils\bin**.</span><span class="sxs-lookup"><span data-stu-id="aba36-130">To work around this error, you must [download the executable from here](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) to a location like **C:\WinUtils\bin**.</span></span> <span data-ttu-id="aba36-131">Pak musíte přidat proměnnou prostředí **HADOOP_HOME** a nastavte hodnotu proměnné na **C\WinUtils**.</span><span class="sxs-lookup"><span data-stu-id="aba36-131">You must then add an environment variable **HADOOP_HOME** and set the value of the variable to **C\WinUtils**.</span></span>

## <a name="step-1-create-an-azure-virtual-network"></a><span data-ttu-id="aba36-132">Krok 1: Vytvoření virtuální sítě Azure</span><span class="sxs-lookup"><span data-stu-id="aba36-132">Step 1: Create an Azure Virtual Network</span></span>
<span data-ttu-id="aba36-133">Postupujte podle pokynů z následujících odkazů můžete vytvořit virtuální síť Azure a pak ověřte připojení mezi desktopem a virtuální síti Azure.</span><span class="sxs-lookup"><span data-stu-id="aba36-133">Follow the instructions from the below links to create an Azure Virtual Network and then verify the connectivity between the desktop and Azure Virtual Network.</span></span>

* [<span data-ttu-id="aba36-134">Vytvoření virtuální sítě s připojením VPN typu site-to-site pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="aba36-134">Create a VNet with a site-to-site VPN connection using Azure Portal</span></span>](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [<span data-ttu-id="aba36-135">Vytvoření virtuální sítě s připojením VPN typu site-to-site pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="aba36-135">Create a VNet with a site-to-site VPN connection using PowerShell</span></span>](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* [<span data-ttu-id="aba36-136">Konfigurace připojení typu point-to-site k virtuální síti pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="aba36-136">Configure a point-to-site connection to a virtual network using PowerShell</span></span>](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

## <a name="step-2-create-an-hdinsight-spark-cluster"></a><span data-ttu-id="aba36-137">Krok 2: Vytvoření clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="aba36-137">Step 2: Create an HDInsight Spark cluster</span></span>
<span data-ttu-id="aba36-138">Měli byste také vytvořit cluster Apache Spark v Azure HDInsight, který je součástí virtuální sítě Azure, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="aba36-138">You should also create an Apache Spark cluster on Azure HDInsight that is part of the Azure Virtual Network that you created.</span></span> <span data-ttu-id="aba36-139">Použijte informace k dispozici na [vytvořit systémem Linux clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="aba36-139">Use the information available at [Create Linux-based clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span> <span data-ttu-id="aba36-140">Jako součást volitelné konfigurace vyberte virtuální síť Azure, kterou jste vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="aba36-140">As part of optional configuration, select the Azure Virtual Network that you created in the previous step.</span></span>

## <a name="step-3-verify-the-connectivity-between-the-cluster-headnode-and-your-desktop"></a><span data-ttu-id="aba36-141">Krok 3: Ověření připojení mezi headnode clusteru a pracovní plochy</span><span class="sxs-lookup"><span data-stu-id="aba36-141">Step 3: Verify the connectivity between the cluster headnode and your desktop</span></span>
1. <span data-ttu-id="aba36-142">Získáte IP adresu headnode.</span><span class="sxs-lookup"><span data-stu-id="aba36-142">Get the IP address of the headnode.</span></span> <span data-ttu-id="aba36-143">Otevřete uživatelské rozhraní Ambari pro cluster.</span><span class="sxs-lookup"><span data-stu-id="aba36-143">Open Ambari UI for the cluster.</span></span> <span data-ttu-id="aba36-144">V okně clusteru, klikněte na tlačítko **řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="aba36-144">From the cluster blade, click **Dashboard**.</span></span>

    ![Hledání headnode IP adresy](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/launch-ambari-ui.png)
2. <span data-ttu-id="aba36-146">V uživatelském rozhraní Ambari, v pravém horním rohu, klikněte na **hostitele**.</span><span class="sxs-lookup"><span data-stu-id="aba36-146">From the Ambari UI, from the top-right corner, click **Hosts**.</span></span>

    ![Hledání headnode IP adresy](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/ambari-hosts.png)
3. <span data-ttu-id="aba36-148">Zobrazí seznam headnodes, pracovní uzly a uzly zookeeper.</span><span class="sxs-lookup"><span data-stu-id="aba36-148">You should see a list of headnodes, worker nodes, and zookeeper nodes.</span></span> <span data-ttu-id="aba36-149">Máte headnodes **hn*** předponu.</span><span class="sxs-lookup"><span data-stu-id="aba36-149">The headnodes have the **hn*** prefix.</span></span> <span data-ttu-id="aba36-150">Klikněte na první headnode.</span><span class="sxs-lookup"><span data-stu-id="aba36-150">Click the first headnode.</span></span>

    ![Hledání headnode IP adresy](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/cluster-headnodes.png)
4. <span data-ttu-id="aba36-152">V dolní části stránky, které se otevře z **Souhrn** pole, zkopírovat IP adresu headnode a název hostitele.</span><span class="sxs-lookup"><span data-stu-id="aba36-152">At the bottom of the page that opens, from the **Summary** box, copy the IP address of the headnode and the host name.</span></span>

    ![Hledání headnode IP adresy](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/headnode-ip-address.png)
5. <span data-ttu-id="aba36-154">Zahrnují IP adresu a název hostitele headnode k **hostitele** soubor v počítači ze kterých chcete spustit a vzdáleně ladit úlohy Spark.</span><span class="sxs-lookup"><span data-stu-id="aba36-154">Include the IP address and the host name of the headnode to the **hosts** file on the computer from where you want to run and remotely debug the Spark jobs.</span></span> <span data-ttu-id="aba36-155">To vám umožní komunikovat s headnode používat IP adresu, stejně jako název hostitele.</span><span class="sxs-lookup"><span data-stu-id="aba36-155">This will enable you to communicate with the headnode using the IP address as well as the hostname.</span></span>

   1. <span data-ttu-id="aba36-156">Otevřete Poznámkový blok se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="aba36-156">Open a notepad with elevated permissions.</span></span> <span data-ttu-id="aba36-157">V nabídce Soubor klikněte na tlačítko **otevřete** a pak přejděte do umístění souboru hostitelů.</span><span class="sxs-lookup"><span data-stu-id="aba36-157">From the file menu, click **Open** and then navigate to the location of the hosts file.</span></span> <span data-ttu-id="aba36-158">Na počítači s Windows, je `C:\Windows\System32\Drivers\etc\hosts`.</span><span class="sxs-lookup"><span data-stu-id="aba36-158">On a Windows computer, it is `C:\Windows\System32\Drivers\etc\hosts`.</span></span>
   2. <span data-ttu-id="aba36-159">Přidejte následující **hostitele** souboru.</span><span class="sxs-lookup"><span data-stu-id="aba36-159">Add the following to the **hosts** file.</span></span>

           # For headnode0
           192.xxx.xx.xx hn0-nitinp
           192.xxx.xx.xx hn0-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net

           # For headnode1
           192.xxx.xx.xx hn1-nitinp
           192.xxx.xx.xx hn1-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net
6. <span data-ttu-id="aba36-160">Z počítače, který jste připojeni k virtuální síti Azure, který je používán clusteru HDInsight ověřte, že může odeslat příkaz ping i headnodes používat IP adresu, stejně jako název hostitele.</span><span class="sxs-lookup"><span data-stu-id="aba36-160">From the computer that you connected to the Azure Virtual Network that is used by the HDInsight cluster, verify that you can ping both the headnodes using the IP address as well as the hostname.</span></span>
7. <span data-ttu-id="aba36-161">SSH do clusteru headnode, postupujte podle pokynů v [připojit ke clusteru HDInsight pomocí protokolu SSH](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="aba36-161">SSH into the cluster headnode using the instructions at [Connect to an HDInsight cluster using SSH](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span> <span data-ttu-id="aba36-162">Z clusteru headnode příkazem ping otestovat adresu IP stolní počítač.</span><span class="sxs-lookup"><span data-stu-id="aba36-162">From the cluster headnode, ping the IP address of the desktop computer.</span></span> <span data-ttu-id="aba36-163">Měli byste otestovat připojení k obě IP adresy přiřazené do počítače, jeden pro síťové připojení a druhou pro virtuální síť Azure, která je počítač připojen k.</span><span class="sxs-lookup"><span data-stu-id="aba36-163">You should test connectivity to both the IP addresses assigned to the computer, one for the network connection and the other for the Azure Virtual Network that the computer is connected to.</span></span>
8. <span data-ttu-id="aba36-164">Opakujte kroky pro ostatní headnode také.</span><span class="sxs-lookup"><span data-stu-id="aba36-164">Repeat the steps for the other headnode as well.</span></span>

## <a name="step-4-create-a-spark-scala-application-using-the-hdinsight-tools-in-azure-toolkit-for-intellij-and-configure-it-for-remote-debugging"></a><span data-ttu-id="aba36-165">Krok 4: Vytvoření Spark Scala aplikace pomocí nástrojů HDInsight pro IntelliJ v nástrojů Azure a nakonfigurovat ji pro vzdálené ladění</span><span class="sxs-lookup"><span data-stu-id="aba36-165">Step 4: Create a Spark Scala application using the HDInsight Tools in Azure Toolkit for IntelliJ and configure it for remote debugging</span></span>
1. <span data-ttu-id="aba36-166">Spusťte IntelliJ IDEA a vytvoření nového projektu.</span><span class="sxs-lookup"><span data-stu-id="aba36-166">Launch IntelliJ IDEA and create a new project.</span></span> <span data-ttu-id="aba36-167">V dialogovém okně Nový projekt vyberte následující možnosti a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="aba36-167">In the new project dialog box, make the following choices, and then click **Next**.</span></span>

    ![Vytvoření aplikací Spark Scala](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-hdi-scala-app.png)

   * <span data-ttu-id="aba36-169">V levém podokně vyberte **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="aba36-169">From the left pane, select **HDInsight**.</span></span>
   * <span data-ttu-id="aba36-170">V pravém podokně vyberte **Spark v HDInsight (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="aba36-170">From the right pane, select **Spark on HDInsight (Scala)**.</span></span>
   * <span data-ttu-id="aba36-171">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="aba36-171">Click **Next**.</span></span>
2. <span data-ttu-id="aba36-172">V okně Další zadejte následující podrobnosti projektu a pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="aba36-172">In the next window, provide the following project details, and then click **Finish**.</span></span>  
   - <span data-ttu-id="aba36-173">Zadejte název projektu a umístění projektu.</span><span class="sxs-lookup"><span data-stu-id="aba36-173">Provide a project name and project location.</span></span>
   - <span data-ttu-id="aba36-174">Pro **SDK projektu**, použijte Java 1.8 pro cluster spark 2.x, Java 1.7 pro 1.x clusteru spark.</span><span class="sxs-lookup"><span data-stu-id="aba36-174">For **Project SDK**, Use Java 1.8 for spark 2.x cluster, Java 1.7 for spark 1.x cluster.</span></span>
   - <span data-ttu-id="aba36-175">Pro **Spark verze**, Průvodce vytvořením projektu Scala integruje správnou verzi sady SDK Spark a Scala SDK.</span><span class="sxs-lookup"><span data-stu-id="aba36-175">For **Spark Version**, Scala project creation wizard integrates proper version for Spark SDK and Scala SDK.</span></span> <span data-ttu-id="aba36-176">Pokud je verze clusteru spark nižší 2.0, zvolte spark 1.x.</span><span class="sxs-lookup"><span data-stu-id="aba36-176">If the spark cluster version is lower 2.0, choose spark 1.x.</span></span> <span data-ttu-id="aba36-177">Jinak měli byste vybrat spark2.x.</span><span class="sxs-lookup"><span data-stu-id="aba36-177">Otherwise, you should select spark2.x.</span></span> <span data-ttu-id="aba36-178">Tento příklad používá Spark2.0.2 (Scala 2.11.8).</span><span class="sxs-lookup"><span data-stu-id="aba36-178">This example uses Spark2.0.2(Scala 2.11.8).</span></span>
       <span data-ttu-id="aba36-179">![Vytvoření aplikací Spark Scala](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-scala-project-details.png)</span><span class="sxs-lookup"><span data-stu-id="aba36-179">![Create Spark Scala application](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-scala-project-details.png)</span></span>
  
3. <span data-ttu-id="aba36-180">Projekt Spark automaticky vytvoří artefakt.</span><span class="sxs-lookup"><span data-stu-id="aba36-180">The Spark project will automatically create an artifact for you.</span></span> <span data-ttu-id="aba36-181">Informace o artefaktu, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="aba36-181">To see the artifact, follow these steps.</span></span>

   1. <span data-ttu-id="aba36-182">Z **soubor** nabídky, klikněte na tlačítko **strukturu projektu**.</span><span class="sxs-lookup"><span data-stu-id="aba36-182">From the **File** menu, click **Project Structure**.</span></span>
   2. <span data-ttu-id="aba36-183">V **strukturu projektu** dialogové okno, klikněte na tlačítko **artefakty** zobrazíte výchozí artefaktu, který je vytvořen.</span><span class="sxs-lookup"><span data-stu-id="aba36-183">In the **Project Structure** dialog box, click **Artifacts** to see the default artifact that is created.</span></span>
   <span data-ttu-id="aba36-184">![Vytvoření JAR](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/default-artifact.png)</span><span class="sxs-lookup"><span data-stu-id="aba36-184">![Create JAR](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/default-artifact.png)</span></span>

      <span data-ttu-id="aba36-185">Můžete také vytvořit vlastní artefaktů bly kliknutím na  **+**  ikonu, vyznačené na obrázku výše.</span><span class="sxs-lookup"><span data-stu-id="aba36-185">You can also create your own artifact bly clicking on the **+** icon, highlighted in the image above.</span></span>

4. <span data-ttu-id="aba36-186">Do projektu přidejte knihovny.</span><span class="sxs-lookup"><span data-stu-id="aba36-186">Add libraries to your project.</span></span> <span data-ttu-id="aba36-187">Pokud chcete přidat do knihovny, klikněte pravým tlačítkem na název projektu ve stromové struktuře projektu a pak klikněte na tlačítko **otevřete nastavení modulu**.</span><span class="sxs-lookup"><span data-stu-id="aba36-187">To add a library, right-click the project name in the project tree, and then click **Open Module Settings**.</span></span> <span data-ttu-id="aba36-188">V **strukturu projektu** dialogové okno, v levém podokně klikněte na tlačítko **knihovny**, klikněte na tlačítko (+) symbolů a potom klikněte na **z Maven**.</span><span class="sxs-lookup"><span data-stu-id="aba36-188">In the **Project Structure** dialog box, from the left pane, click **Libraries**, click the (+) symbol, and then click **From Maven**.</span></span>

    ![Přidání knihovny](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/add-library.png)

    <span data-ttu-id="aba36-190">V **knihovny stáhnout z úložiště Maven** dialogové okno, vyhledávání a přidejte následující knihovny.</span><span class="sxs-lookup"><span data-stu-id="aba36-190">In the **Download Library from Maven Repository** dialog box, search and add the following libraries.</span></span>

   * `org.scalatest:scalatest_2.10:2.2.1`
   * `org.apache.hadoop:hadoop-azure:2.7.1`
5. <span data-ttu-id="aba36-191">Kopírování `yarn-site.xml` a `core-site.xml` z headnode clusteru a přidejte ji do projektu.</span><span class="sxs-lookup"><span data-stu-id="aba36-191">Copy `yarn-site.xml` and `core-site.xml` from the cluster headnode and add it to the project.</span></span> <span data-ttu-id="aba36-192">Použijte následující příkazy pro kopírování souborů.</span><span class="sxs-lookup"><span data-stu-id="aba36-192">Use the following commands to copy the files.</span></span> <span data-ttu-id="aba36-193">Můžete použít [emulaci](https://cygwin.com/install.html) spustit následující `scp` příkazy pro kopírování souborů z headnodes clusteru.</span><span class="sxs-lookup"><span data-stu-id="aba36-193">You can use [Cygwin](https://cygwin.com/install.html) to run the following `scp` commands to copy the files from the cluster headnodes.</span></span>

        scp <ssh user name>@<headnode IP address or host name>://etc/hadoop/conf/core-site.xml .

    <span data-ttu-id="aba36-194">Protože jsme již přidána clusteru headnode IP adresy a názvy hostitelů fo souboru hostitelů na ploše, můžeme použít **spojovací bod služby** příkazy následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="aba36-194">Because we already added the cluster headnode IP address and hostnames fo the hosts file on the desktop, we can use the **scp** commands in the following manner.</span></span>

        scp sshuser@hn0-nitinp:/etc/hadoop/conf/core-site.xml .
        scp sshuser@hn0-nitinp:/etc/hadoop/conf/yarn-site.xml .

    <span data-ttu-id="aba36-195">Do projektu přidejte tyto soubory tak, že je v části zkopírujete **/src** složky ve stromu vašeho projektu, například `<your project directory>\src`.</span><span class="sxs-lookup"><span data-stu-id="aba36-195">Add these files to your project by copying them under the **/src** folder in your project tree, for example `<your project directory>\src`.</span></span>
6. <span data-ttu-id="aba36-196">Aktualizace `core-site.xml` provést následující změny.</span><span class="sxs-lookup"><span data-stu-id="aba36-196">Update the `core-site.xml` to make the following changes.</span></span>

   1. <span data-ttu-id="aba36-197">`core-site.xml`obsahuje šifrovaný klíč k účtu úložiště, který je přidružen ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="aba36-197">`core-site.xml` includes the encrypted key to the storage account associated with the cluster.</span></span> <span data-ttu-id="aba36-198">V `core-site.xml` , že jste přidali do projektu, nahraďte skutečným úložiště klíč zašifrovaný klíč přidružený výchozí účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="aba36-198">In the `core-site.xml` that you added to the project, replace the encrypted key with the actual storage key associated with the default storage account.</span></span> <span data-ttu-id="aba36-199">V tématu [Správa přístupových klíčů úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="aba36-199">See [Manage your storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>

           <property>
                 <name>fs.azure.account.key.hdistoragecentral.blob.core.windows.net</name>
                 <value>access-key-associated-with-the-account</value>
           </property>
   2. <span data-ttu-id="aba36-200">Odebrat následující položky z `core-site.xml`.</span><span class="sxs-lookup"><span data-stu-id="aba36-200">Remove the following entries from the `core-site.xml`.</span></span>

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
   3. <span data-ttu-id="aba36-201">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="aba36-201">Save the file.</span></span>
7. <span data-ttu-id="aba36-202">Přidání třídy hlavní pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="aba36-202">Add the Main class for your application.</span></span> <span data-ttu-id="aba36-203">Z **Project Exploreru**, klikněte pravým tlačítkem na **src**, přejděte na **nový**a potom klikněte na **Scala třída**.</span><span class="sxs-lookup"><span data-stu-id="aba36-203">From the **Project Explorer**, right-click **src**, point to **New**, and then click **Scala class**.</span></span>

    ![Přidejte zdrojový kód](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-spark-scala-code.png)
8. <span data-ttu-id="aba36-205">V **vytvořte novou třídu Scala** dialogovém okně zadejte název, **druhu** vyberte **objekt**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="aba36-205">In the **Create New Scala Class** dialog box, provide a name, for **Kind** select **Object**, and then click **OK**.</span></span>

    ![Přidejte zdrojový kód](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-spark-scala-code-object.png)
9. <span data-ttu-id="aba36-207">V `MyClusterAppMain.scala` souboru, vložte následující kód.</span><span class="sxs-lookup"><span data-stu-id="aba36-207">In the `MyClusterAppMain.scala` file, paste the following code.</span></span> <span data-ttu-id="aba36-208">Tento kód vytvoří Spark kontextu a spustí `executeJob` metoda z `SparkSample` objektu.</span><span class="sxs-lookup"><span data-stu-id="aba36-208">This code creates the Spark context and launches an `executeJob` method from the `SparkSample` object.</span></span>

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

10. <span data-ttu-id="aba36-209">Opakujte kroky 8 a 9 výše a přidejte nový objekt Scala názvem `SparkSample`.</span><span class="sxs-lookup"><span data-stu-id="aba36-209">Repeat steps 8 and 9 above to add a new Scala object called `SparkSample`.</span></span> <span data-ttu-id="aba36-210">Pro tuto třídu přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="aba36-210">To this class add the following code.</span></span> <span data-ttu-id="aba36-211">Tento kód čte data z HVAC.csv (k dispozici na všech clusterech HDInsight Spark), načte řádky, které mají pouze jednu číslici ve sloupci sedmého ve sdíleném svazku clusteru a zapíše výstup do **/HVACOut** v kontejneru výchozí úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="aba36-211">This code reads the data from the HVAC.csv (available on all HDInsight Spark clusters), retrieves the rows that only have one digit in the seventh column in the CSV, and writes the output to **/HVACOut** under the default storage container for the cluster.</span></span>

        import org.apache.spark.SparkContext

        object SparkSample {
         def executeJob (sc: SparkContext, input: String, output: String): Unit = {
           val rdd = sc.textFile(input)

           //find the rows which have only one digit in the 7th column in the CSV
           val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)

           val s = sc.parallelize(rdd.take(5)).cartesian(rdd).count()
           println(s)

           rdd1.saveAsTextFile(output)
           //rdd1.collect().foreach(println)
         }
        }
11. <span data-ttu-id="aba36-212">Opakujte kroky 8 a 9 výše a přidejte novou třídu s názvem `RemoteClusterDebugging`.</span><span class="sxs-lookup"><span data-stu-id="aba36-212">Repeat steps 8 and 9 above to add a new class called `RemoteClusterDebugging`.</span></span> <span data-ttu-id="aba36-213">Tato třída implementuje rozhraní Spark test, který slouží k ladění aplikací.</span><span class="sxs-lookup"><span data-stu-id="aba36-213">This class implements the Spark test framework that is used for debugging applications.</span></span> <span data-ttu-id="aba36-214">Přidejte následující kód, který `RemoteClusterDebugging` třídy.</span><span class="sxs-lookup"><span data-stu-id="aba36-214">Add the following code to the `RemoteClusterDebugging` class.</span></span>

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

     <span data-ttu-id="aba36-215">Několik důležitých věcí si zde:</span><span class="sxs-lookup"><span data-stu-id="aba36-215">Couple of important things to note here:</span></span>

   * <span data-ttu-id="aba36-216">Pro `.set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")`, zkontrolujte, zda je sestavení Spark JAR k dispozici v úložišti clusteru v zadané cestě.</span><span class="sxs-lookup"><span data-stu-id="aba36-216">For `.set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")`, make sure the Spark assembly JAR is available on the cluster storage at the specified path.</span></span>
   * <span data-ttu-id="aba36-217">Pro `setJars`, zadejte umístění, kde bude vytvořen jar artefaktů.</span><span class="sxs-lookup"><span data-stu-id="aba36-217">For `setJars`, specify the location where the artifact jar will be created.</span></span> <span data-ttu-id="aba36-218">Obvykle je `<Your IntelliJ project directory>\out\<project name>_DefaultArtifact\default_artifact.jar`.</span><span class="sxs-lookup"><span data-stu-id="aba36-218">Typically it is `<Your IntelliJ project directory>\out\<project name>_DefaultArtifact\default_artifact.jar`.</span></span>
12. <span data-ttu-id="aba36-219">V `RemoteClusterDebugging` třídy, klikněte pravým tlačítkem myši `test` – klíčové slovo a vyberte **vytvořit konfigurace RemoteClusterDebugging**.</span><span class="sxs-lookup"><span data-stu-id="aba36-219">In the `RemoteClusterDebugging` class, right-click the `test` keyword and select **Create RemoteClusterDebugging Configuration**.</span></span>

    ![Vytvoření konfigurace vzdáleného](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-remote-config.png)

13. <span data-ttu-id="aba36-221">V dialogovém okně zadejte název pro konfiguraci a vyberte **testovací druh** jako **název testovacího**.</span><span class="sxs-lookup"><span data-stu-id="aba36-221">In the dialog box, provide a name for the configuration, and select the **Test kind** as **Test name**.</span></span> <span data-ttu-id="aba36-222">Nechat všechny ostatní hodnoty jako výchozí, klikněte na **použít**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="aba36-222">Leave all other values as default, click **Apply**, and then click **OK**.</span></span>

    ![Vytvoření konfigurace vzdáleného](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/provide-config-value.png)
14. <span data-ttu-id="aba36-224">Teď byste měli vidět **vzdálené spuštění** konfigurace rozevírací seznam v panelu nabídek.</span><span class="sxs-lookup"><span data-stu-id="aba36-224">You should now see a **Remote Run** configuration drop-down in the menu bar.</span></span>

    ![Vytvoření konfigurace vzdáleného](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/config-run.png)

## <a name="step-5-run-the-application-in-debug-mode"></a><span data-ttu-id="aba36-226">Krok 5: Spuštění aplikace v režimu ladění</span><span class="sxs-lookup"><span data-stu-id="aba36-226">Step 5: Run the application in debug mode</span></span>
1. <span data-ttu-id="aba36-227">Otevřete v projektu IntelliJ IDEA `SparkSample.scala` a vytvořit zarážku vedle 'val rdd1'.</span><span class="sxs-lookup"><span data-stu-id="aba36-227">In your IntelliJ IDEA project, open `SparkSample.scala` and create a breakpoint next to \`val rdd1'.</span></span> <span data-ttu-id="aba36-228">V nabídce automaticky otevírané okno pro vytvoření zarážku vyberte **řádku funkce executeJob**.</span><span class="sxs-lookup"><span data-stu-id="aba36-228">In the pop-up menu for creating a breakpoint, select **line in function executeJob**.</span></span>

    ![Přidat zarážky](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-breakpoint.png)
2. <span data-ttu-id="aba36-230">Klikněte na tlačítko **ladění spustit** vedle položky **vzdálené spuštění** konfigurace rozevíracího seznamu spustit aplikace.</span><span class="sxs-lookup"><span data-stu-id="aba36-230">Click the **Debug Run** button next to the **Remote Run** configuration drop-down to start running the application.</span></span>

    ![Spusťte program v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-run-mode.png)
3. <span data-ttu-id="aba36-232">Při spuštění programu dosáhne zarážky, měli byste vidět **ladicí program** karty v dolním podokně.</span><span class="sxs-lookup"><span data-stu-id="aba36-232">When the program execution reaches the breakpoint, you should see a **Debugger** tab in the bottom pane.</span></span>

    ![Spusťte program v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch.png)
4. <span data-ttu-id="aba36-234">Klikněte na (**+**) ikonu, čímž přidáte sledovat, jak je znázorněno na obrázku níže.</span><span class="sxs-lookup"><span data-stu-id="aba36-234">Click the (**+**) icon to add a watch as shown in the image below.</span></span>

    ![Spusťte program v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch-variable.png)

    <span data-ttu-id="aba36-236">Sem, protože aplikace překročila před proměnnou `rdd1` byla vytvořena pomocí tohoto sledování, uvidíte, jaké jsou prvních 5 řádky v proměnné `rdd`.</span><span class="sxs-lookup"><span data-stu-id="aba36-236">Here, because the application broke before the variable `rdd1` was created, using this watch we can see what are the first 5 rows in the variable `rdd`.</span></span> <span data-ttu-id="aba36-237">Stiskněte **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="aba36-237">Press **ENTER**.</span></span>

    ![Spusťte program v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch-variable-value.png)

    <span data-ttu-id="aba36-239">Co se zobrazí v na obrázku výše je, že v době běhu můžete dotazovat terrabytes dat a ladění jak postupuje vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="aba36-239">What you see in the image above is that at runtime, you could query terrabytes of data and debug how your application progresses.</span></span> <span data-ttu-id="aba36-240">Ve výstupu znázorněno na obrázku výše, například uvidíte, že první řádek výstupu je záhlaví.</span><span class="sxs-lookup"><span data-stu-id="aba36-240">For example, in the output shown in the image above, you can see that the first row of the output is a header.</span></span> <span data-ttu-id="aba36-241">Na základě, můžete upravit kód aplikace tak, aby přeskočil řádek záhlaví, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="aba36-241">Based on this, you can modify your application code to skip the header row if required.</span></span>
5. <span data-ttu-id="aba36-242">Nyní můžete kliknout na **obnovit Program** ikonu pokračujte aplikace spustit.</span><span class="sxs-lookup"><span data-stu-id="aba36-242">You can now click the **Resume Program** icon to proceed with your application run.</span></span>

    ![Spusťte program v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-continue-run.png)
6. <span data-ttu-id="aba36-244">Pokud se aplikace úspěšně dokončí, měli byste vidět výstup jako následující.</span><span class="sxs-lookup"><span data-stu-id="aba36-244">If the application completes successfully, you should see an output like the following.</span></span>

    ![Spusťte program v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-complete.png)

## <span data-ttu-id="aba36-246"><a name="seealso"></a>Viz také</span><span class="sxs-lookup"><span data-stu-id="aba36-246"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="aba36-247">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="aba36-247">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="demo"></a><span data-ttu-id="aba36-248">Ukázka</span><span class="sxs-lookup"><span data-stu-id="aba36-248">Demo</span></span>
* <span data-ttu-id="aba36-249">Vytvoření projektu Scala (Video): [vytvoření aplikací Spark Scala](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="aba36-249">Create Scala Project (Video): [Create Spark Scala Applications](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span></span>
* <span data-ttu-id="aba36-250">Vzdálené ladění (Video): [pomocí nástrojů Azure pro IntelliJ k ladění aplikací Spark vzdáleně na clusteru HDInsight](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="aba36-250">Remote Debug (Video): [Use Azure Toolkit for IntelliJ to debug Spark applications remotely on HDInsight Cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span></span>

### <a name="scenarios"></a><span data-ttu-id="aba36-251">Scénáře</span><span class="sxs-lookup"><span data-stu-id="aba36-251">Scenarios</span></span>
* [<span data-ttu-id="aba36-252">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="aba36-252">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="aba36-253">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="aba36-253">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="aba36-254">Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin</span><span class="sxs-lookup"><span data-stu-id="aba36-254">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="aba36-255">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="aba36-255">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="aba36-256">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="aba36-256">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="aba36-257">Vytvoření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="aba36-257">Create and run applications</span></span>
* [<span data-ttu-id="aba36-258">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="aba36-258">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="aba36-259">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="aba36-259">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="aba36-260">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="aba36-260">Tools and extensions</span></span>
* [<span data-ttu-id="aba36-261">Pomocí nástrojů HDInsight v Azure nástrojů pro IntelliJ můžete vytvořit a odesílání aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="aba36-261">Use HDInsight Tools in Azure Toolkit for IntelliJ to create and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="aba36-262">Použití nástrojů Azure pro IntelliJ k ladění aplikací Spark vzdáleně přes SSH</span><span class="sxs-lookup"><span data-stu-id="aba36-262">Use Azure Toolkit for IntelliJ to debug Spark applications remotely through SSH</span></span>](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [<span data-ttu-id="aba36-263">Použití nástroje HDInsight pro IntelliJ s Hortonworks karanténě</span><span class="sxs-lookup"><span data-stu-id="aba36-263">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [<span data-ttu-id="aba36-264">Používat nástroje HDInsight pro vytvoření aplikací Spark v Azure nástrojů pro Eclipse</span><span class="sxs-lookup"><span data-stu-id="aba36-264">Use HDInsight Tools in Azure Toolkit for Eclipse to create Spark applications</span></span>](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [<span data-ttu-id="aba36-265">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="aba36-265">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="aba36-266">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="aba36-266">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="aba36-267">Použití externích balíčků s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="aba36-267">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="aba36-268">Instalace Jupyteru do počítače a připojení ke clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="aba36-268">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="aba36-269">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="aba36-269">Manage resources</span></span>
* [<span data-ttu-id="aba36-270">Správa prostředků v clusteru Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="aba36-270">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="aba36-271">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="aba36-271">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
