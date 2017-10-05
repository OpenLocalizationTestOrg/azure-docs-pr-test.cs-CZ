---
title: "Instalace vlastních aplikací Hadoop v Azure HDInsight | Dokumentace Microsoftu"
description: "Informace o instalaci aplikací HDInsight na aplikace HDInsight."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: e556b29c-8176-4bc5-a90b-aa01abfd3aee
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: ebec29dea9f5dc1767f47a53d9da03347a51de28
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="install-custom-hadoop-applications-on-azure-hdinsight"></a><span data-ttu-id="af453-103">Instalace vlastních aplikací Hadoop v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="af453-103">Install custom Hadoop applications on Azure HDInsight</span></span>

<span data-ttu-id="af453-104">V tomto článku se dozvíte, jak v Azure HDInsight nainstalovat aplikaci Hadoop, která nebyla publikována na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="af453-104">In this article, you will learn how to install a Hadoop application on Azure HDInsight, which has not been published to the Azure portal.</span></span> <span data-ttu-id="af453-105">Aplikace, kterou v tomto článku nainstalujete, se jmenuje [Hue](http://gethue.com/).</span><span class="sxs-lookup"><span data-stu-id="af453-105">The application you will install in this article is [Hue](http://gethue.com/).</span></span>

<span data-ttu-id="af453-106">Aplikace HDInsight je aplikace, kterou uživatelé mohou nainstalovat na clusteru HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="af453-106">An HDInsight application is an application that users can install on a Linux-based HDInsight cluster.</span></span>  <span data-ttu-id="af453-107">Tyto aplikace mohou být vytvořeny společností Microsoft, nezávislými dodavateli softwaru (ISV) nebo vámi samotnými.</span><span class="sxs-lookup"><span data-stu-id="af453-107">These applications can be developed by Microsoft, independent software vendors (ISV) or by yourself.</span></span>  

<span data-ttu-id="af453-108">Další související články:</span><span class="sxs-lookup"><span data-stu-id="af453-108">Other related articles:</span></span>

* <span data-ttu-id="af453-109">[Instalace aplikací HDInsight](hdinsight-apps-install-applications.md): Naučte se instalovat aplikace HDInsight do svých clusterů.</span><span class="sxs-lookup"><span data-stu-id="af453-109">[Install HDInsight applications](hdinsight-apps-install-applications.md): Learn how to install an HDInsight application to your clusters.</span></span>
* <span data-ttu-id="af453-110">[Publikování aplikací HDInsight](hdinsight-apps-publish-applications.md): Zjistěte, jak publikovat vlastní aplikace HDInsight do obchodu Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="af453-110">[Publish HDInsight applications](hdinsight-apps-publish-applications.md): Learn how to publish your custom HDInsight applications to Azure Marketplace.</span></span>
* <span data-ttu-id="af453-111">[MSDN: Instalace aplikace HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Další informace jak definovat aplikace HDInsight.</span><span class="sxs-lookup"><span data-stu-id="af453-111">[MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx): Learn how to define HDInsight applications.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="af453-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="af453-112">Prerequisites</span></span>
<span data-ttu-id="af453-113">Pokud chcete instalovat aplikace HDInsight na stávající cluster HDInsight, musí mít cluster služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="af453-113">If you want to install HDInsight applications on an existing HDInsight cluster, you must have an HDInsight cluster.</span></span> <span data-ttu-id="af453-114">Chcete-li jeden vytvořit, prostudujte si část [Tvorba clusterů](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).</span><span class="sxs-lookup"><span data-stu-id="af453-114">To create one, see [Create clusters](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).</span></span> <span data-ttu-id="af453-115">Aplikace HDInsight můžete také nainstalovat při vytváření clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="af453-115">You can also install HDInsight applications when you create an HDInsight cluster.</span></span>

## <a name="install-hdinsight-applications"></a><span data-ttu-id="af453-116">Instalace aplikací HDInsight</span><span class="sxs-lookup"><span data-stu-id="af453-116">Install HDInsight applications</span></span>
<span data-ttu-id="af453-117">Aplikace HDInsight lze nainstalovat při vytvoření clusteru nebo do existujícího clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="af453-117">HDInsight applications can be installed when you create a cluster or to an existing HDInsight cluster.</span></span> <span data-ttu-id="af453-118">Postup definování šablon Azure Resource Manageru, viz část [MSDN: instalace aplikace HDInsight](https://msdn.microsoft.com/library/mt706515.aspx).</span><span class="sxs-lookup"><span data-stu-id="af453-118">For defining Azure Resource Manager templates, see [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx).</span></span>

<span data-ttu-id="af453-119">Soubory potřebné pro nasazení této aplikace (Hue):</span><span class="sxs-lookup"><span data-stu-id="af453-119">The files needed for deploying this application (Hue):</span></span>

* <span data-ttu-id="af453-120">[azuredeploy.JSON](https://github.com/hdinsight/Iaas-Applications/blob/master/Hue/azuredeploy.json): Šablona Resource Manageru pro instalaci aplikace HDInsight.</span><span class="sxs-lookup"><span data-stu-id="af453-120">[azuredeploy.json](https://github.com/hdinsight/Iaas-Applications/blob/master/Hue/azuredeploy.json): The Resource Manager template for installing HDInsight application.</span></span> <span data-ttu-id="af453-121">Viz [MSDN: instalace aplikace HDInsight](https://msdn.microsoft.com/library/mt706515.aspx) pro vývoj vlastní šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="af453-121">See [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx) for developing your own Resource Manager template.</span></span>
* <span data-ttu-id="af453-122">[hue-install_v0.sh](https://github.com/hdinsight/Iaas-Applications/blob/master/Hue/scripts/Hue-install_v0.sh): Akce skriptu volaná šablonou Resource Manageru pro konfiguraci hraničního uzlu.</span><span class="sxs-lookup"><span data-stu-id="af453-122">[hue-install_v0.sh](https://github.com/hdinsight/Iaas-Applications/blob/master/Hue/scripts/Hue-install_v0.sh): The Script action being called by the Resource Manager template for configuring the edge node.</span></span>
* <span data-ttu-id="af453-123">[hue-binaries.tgz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/hue-binaries-14-04.tgz): Binární soubor hue volaný ze souboru hui-install_v0.sh.</span><span class="sxs-lookup"><span data-stu-id="af453-123">[hue-binaries.tgz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/hue-binaries-14-04.tgz): The hue binary file being called from hui-install_v0.sh.</span></span>
* <span data-ttu-id="af453-124">[hue-binaries-14-04.tgz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/hue-binaries-14-04.tgz): Binární soubor hue volaný ze souboru hui-install_v0.sh.</span><span class="sxs-lookup"><span data-stu-id="af453-124">[hue-binaries-14-04.tgz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/hue-binaries-14-04.tgz): The hue binary file being called from hui-install_v0.sh.</span></span>
* <span data-ttu-id="af453-125">[webwasb tomcat.tar.gz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/webwasb-tomcat.tar.gz): Ukázková webová aplikace (Tomcat) volaná ze souboru hui-install_v0.sh.</span><span class="sxs-lookup"><span data-stu-id="af453-125">[webwasb-tomcat.tar.gz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/webwasb-tomcat.tar.gz): A sample web application (Tomcat) being called from hui-install_v0.sh.</span></span>

<span data-ttu-id="af453-126">**Postup instalace aplikace Hue do stávajícího clusteru HDInsight**</span><span class="sxs-lookup"><span data-stu-id="af453-126">**To install Hue to an existing HDInsight cluster**</span></span>

1. <span data-ttu-id="af453-127">Klikněte na následující obrázek pro přihlášení do Azure a otevřete šablonu Resource Manageru na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="af453-127">Click the following image to sign in to Azure and open the Resource Manager template in the Azure Portal.</span></span>

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2FIaas-Applications%2Fmaster%2FHue%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-install-custom-applications/deploy-to-azure.png" alt="Deploy to Azure"></a>

    <span data-ttu-id="af453-128">Toto tlačítko otevře šablonu Resource Manageru na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="af453-128">This button opens a Resource Manager template on the Azure portal.</span></span>  <span data-ttu-id="af453-129">Šablony Resource Manageru se nachází na adrese [https://github.com/hdinsight/Iaas-Applications/tree/master/Hue](https://github.com/hdinsight/Iaas-Applications/tree/master/Hue).</span><span class="sxs-lookup"><span data-stu-id="af453-129">The Resource Manager template is located at [https://github.com/hdinsight/Iaas-Applications/tree/master/Hue](https://github.com/hdinsight/Iaas-Applications/tree/master/Hue).</span></span>  <span data-ttu-id="af453-130">Další informace o zápisu této šablony Resource Manageru naleznete v části [MSDN: instalace aplikace HDInsight](https://msdn.microsoft.com/library/mt706515.aspx).</span><span class="sxs-lookup"><span data-stu-id="af453-130">To learn how to write this Resource Manager template, see [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx).</span></span>
2. <span data-ttu-id="af453-131">Z okna **Parametry** zadejte následující údaje:</span><span class="sxs-lookup"><span data-stu-id="af453-131">From the **Parameters** blade, enter the following:</span></span>

   * <span data-ttu-id="af453-132">**Název clusteru**: Zadejte název clusteru, do kterého chcete aplikaci nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="af453-132">**ClusterName**: Enter the name of the cluster where you want to install the application.</span></span> <span data-ttu-id="af453-133">Tento cluster musí být existující cluster.</span><span class="sxs-lookup"><span data-stu-id="af453-133">This cluster must be an existing cluster.</span></span>
3. <span data-ttu-id="af453-134">Klikněte na možnost **OK** a uložte parametry.</span><span class="sxs-lookup"><span data-stu-id="af453-134">Click **OK** to save the parameters.</span></span>
4. <span data-ttu-id="af453-135">Z okna **Vlastní nasazení** zadejte **Skupinu prostředků**.</span><span class="sxs-lookup"><span data-stu-id="af453-135">From the **Custom deployment** blade, enter **Resource group**.</span></span>  <span data-ttu-id="af453-136">Skupina prostředků je kontejner, který seskupuje cluster, účet závislého úložiště a další prostředky.</span><span class="sxs-lookup"><span data-stu-id="af453-136">The resource group is a container that groups the cluster, the dependent storage account and other resources.</span></span> <span data-ttu-id="af453-137">Je nezbytná k použití stejné skupiny prostředků jako cluster.</span><span class="sxs-lookup"><span data-stu-id="af453-137">It is required to use the same resource group as the cluster.</span></span>
5. <span data-ttu-id="af453-138">Klikněte na tlačítko **Smluvní podmínky** a pak klikněte na tlačítko **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="af453-138">Click **Legal terms**, and then click **Create**.</span></span>
6. <span data-ttu-id="af453-139">Ověřte zaškrtnutí políčka **Kód pin pro řídicí panel** a pak klikněte na tlačítko **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="af453-139">Verify the **Pin to dashboard** checkbox is selected, and then click **Create**.</span></span> <span data-ttu-id="af453-140">Stav instalace můžete zobrazit z dlaždice připnuté k řídicímu panelu portálu a portálu oznámení (kliknutím na ikonu zvonku v horní části portálu).</span><span class="sxs-lookup"><span data-stu-id="af453-140">You can see the installation status from the tile pinned to the portal dashboard and the portal notification (click the bell icon on the top of the portal).</span></span>  <span data-ttu-id="af453-141">Instalace aplikace trvá přibližně 10 minut.</span><span class="sxs-lookup"><span data-stu-id="af453-141">It takes about 10 minutes to install the application.</span></span>

<span data-ttu-id="af453-142">**Postup instalace aplikace Hue při vytváření clusteru**</span><span class="sxs-lookup"><span data-stu-id="af453-142">**To install Hue while creating a cluster**</span></span>

1. <span data-ttu-id="af453-143">Klikněte na následující obrázek pro přihlášení do Azure a otevřete šablonu Resource Manageru na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="af453-143">Click the following image to sign in to Azure and open the Resource Manager template in the Azure Portal.</span></span>

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhdinsightapps%2Fcreate-linux-based-hadoop-cluster-in-hdinsight.json" target="_blank"><img src="./media/hdinsight-apps-install-custom-applications/deploy-to-azure.png" alt="Deploy to Azure"></a>

    <span data-ttu-id="af453-144">Toto tlačítko otevře šablonu Resource Manageru na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="af453-144">This button opens a Resource Manager template on the Azure portal.</span></span>  <span data-ttu-id="af453-145">Šablona Resource Manageru je umístěna na adrese [https://hditutorialdata.blob.core.windows.net/hdinsightapps/create-linux-based-hadoop-cluster-in-hdinsight.json](https://hditutorialdata.blob.core.windows.net/hdinsightapps/create-linux-based-hadoop-cluster-in-hdinsight.json).</span><span class="sxs-lookup"><span data-stu-id="af453-145">The Resource Manager template is located at [https://hditutorialdata.blob.core.windows.net/hdinsightapps/create-linux-based-hadoop-cluster-in-hdinsight.json](https://hditutorialdata.blob.core.windows.net/hdinsightapps/create-linux-based-hadoop-cluster-in-hdinsight.json).</span></span>  <span data-ttu-id="af453-146">Další informace o zápisu této šablony Resource Manageru naleznete v části [MSDN: instalace aplikace HDInsight](https://msdn.microsoft.com/library/mt706515.aspx).</span><span class="sxs-lookup"><span data-stu-id="af453-146">To learn how to write this Resource Manager template, see [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx).</span></span>
2. <span data-ttu-id="af453-147">Pro vytvoření clusteru a instalaci aplikace Hue postupujte podle pokynů.</span><span class="sxs-lookup"><span data-stu-id="af453-147">Follow the instruction to create cluster and install Hue.</span></span> <span data-ttu-id="af453-148">Další informace o vytváření clusterů HDInsight naleznete v tématu [Vytváření clusterů Hadoop založených na Linuxu v nástroji HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="af453-148">For more information on creating HDInsight clusters, see [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

<span data-ttu-id="af453-149">Kromě portálu Azure můžete pro volání šablon Resource Manageru použít také [Azure PowerShell](hdinsight-hadoop-create-linux-clusters-arm-templates.md#deploy-with-powershell) a [Azure CLI](hdinsight-hadoop-create-linux-clusters-arm-templates.md#deploy-with-cli).</span><span class="sxs-lookup"><span data-stu-id="af453-149">In addition to the Azure portal, you can also use [Azure PowerShell](hdinsight-hadoop-create-linux-clusters-arm-templates.md#deploy-with-powershell) and [Azure CLI](hdinsight-hadoop-create-linux-clusters-arm-templates.md#deploy-with-cli) to call Resource Manager templates.</span></span>

## <a name="validate-the-installation"></a><span data-ttu-id="af453-150">Ověření instalace</span><span class="sxs-lookup"><span data-stu-id="af453-150">Validate the installation</span></span>
<span data-ttu-id="af453-151">Stav aplikace můžete zkontrolovat na portálu Azure a ověřit tak instalaci aplikace.</span><span class="sxs-lookup"><span data-stu-id="af453-151">You can check the application status on the Azure portal to validate the application installation.</span></span> <span data-ttu-id="af453-152">Kromě toho můžete také ověřit, zda všechny koncové body HTTP vychází dle očekávání a webovou stránku, pokud existuje:</span><span class="sxs-lookup"><span data-stu-id="af453-152">In addition, you can also validate all HTTP endpoints came up as expected and the webpage if there is one:</span></span>

<span data-ttu-id="af453-153">**Postup otevření portál Hue**</span><span class="sxs-lookup"><span data-stu-id="af453-153">**To open the Hue portal**</span></span>

1. <span data-ttu-id="af453-154">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="af453-154">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="af453-155">Klikněte na tlačítko **Clustery HDInsight** v levé nabídce.</span><span class="sxs-lookup"><span data-stu-id="af453-155">Click **HDInsight Clusters** in the left menu.</span></span>  <span data-ttu-id="af453-156">Pokud ho nevidíte, klikněte na tlačítko **Procházet** a pak klikněte na tlačítko **Clustery HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="af453-156">If you don't see it, click **Browse**, and then click **HDInsight Clusters**.</span></span>
3. <span data-ttu-id="af453-157">Klikněte na cluster, kde je nainstalovaná aplikace.</span><span class="sxs-lookup"><span data-stu-id="af453-157">Click the cluster where you installed the application.</span></span>
4. <span data-ttu-id="af453-158">Z okna **Nastavení** klikněte na tlačítko **Aplikace** pod kategorií **Obecné**.</span><span class="sxs-lookup"><span data-stu-id="af453-158">From the **Settings** blade, click **Applications** under the **General** category.</span></span> <span data-ttu-id="af453-159">Měli byste vidět položku **hue** uvedenou v okně **Nainstalované aplikace**.</span><span class="sxs-lookup"><span data-stu-id="af453-159">You shall see **hue** listed in the **Installed Apps** blade.</span></span>
5. <span data-ttu-id="af453-160">Klikněte na položku **hue** na seznamu a zobrazte seznam vlastností.</span><span class="sxs-lookup"><span data-stu-id="af453-160">Click **hue** from the list to list the properties.</span></span>  
6. <span data-ttu-id="af453-161">Klikněte na odkaz webové stránky a ověřte webovou stránku; koncový bod HTTP, otevřete v prohlížeči a ověřte uživatelské rozhraní webu Hue, otevřete koncový bod SSH pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="af453-161">Click the Webpage link to validate the website; open the HTTP endpoint in a browser to validate the Hue web UI, open the SSH endpoint using SSH.</span></span> <span data-ttu-id="af453-162">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="af453-162">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <a name="troubleshoot-the-installation"></a><span data-ttu-id="af453-163">Řešení potíží instalace</span><span class="sxs-lookup"><span data-stu-id="af453-163">Troubleshoot the installation</span></span>
<span data-ttu-id="af453-164">Stav instalace aplikace můžete zkontrolovat na portálu oznámení (kliknutím na ikonu zvonku v horní části portálu).</span><span class="sxs-lookup"><span data-stu-id="af453-164">You can check the application installation status from the portal notification (Click the bell icon on the top of the portal).</span></span>

<span data-ttu-id="af453-165">Pokud se instalace aplikace nezdařila, můžete zobrazit chybové zprávy a informace ladění ze 3 míst:</span><span class="sxs-lookup"><span data-stu-id="af453-165">If an application installation failed, you can see the error messages and debug information from 3 places:</span></span>

* <span data-ttu-id="af453-166">Aplikace HDInsight: obecné informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="af453-166">HDInsight Applications: general error information.</span></span>

    <span data-ttu-id="af453-167">Otevřete cluster z portálu a v okně Nastavení klikněte na tlačítko Aplikace:</span><span class="sxs-lookup"><span data-stu-id="af453-167">Open the cluster from the portal, and click Applications from the Settings blade:</span></span>

    ![hdinsight aplikace aplikace chyba instalace](./media/hdinsight-apps-install-applications/hdinsight-apps-error.png)
* <span data-ttu-id="af453-169">Akce skriptu HDInsight: Pokud chybová zpráva aplikace HDInsight značí selhání akce skriptu, zobrazí se další podrobnosti o selhání skriptu v podokně Akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="af453-169">HDInsight script action: If the HDInsight Applications' error message indicates a script action failure, more details about the script failure will be presented in the script actions pane.</span></span>

    <span data-ttu-id="af453-170">V okně Nastavení klikněte na tlačítko Akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="af453-170">Click Script Action from the Settings blade.</span></span> <span data-ttu-id="af453-171">V historii akcí skriptu se zobrazí chybové zprávy</span><span class="sxs-lookup"><span data-stu-id="af453-171">Script action history shows the error messages</span></span>

    ![hdinsight aplikace skript akce chyba](./media/hdinsight-apps-install-applications/hdinsight-apps-script-action-error.png)
* <span data-ttu-id="af453-173">Webové uživatelské rozhraní Ambari: Pokud byl příčinou selhání instalační skript, použijte webové uživatelské rozhraní Ambari ke kontrole úplných protokolů týkajících se skriptů instalace.</span><span class="sxs-lookup"><span data-stu-id="af453-173">Ambari Web UI: If the install script was the cause of the failure, use Ambari Web UI to check full logs about the install scripts.</span></span>

    <span data-ttu-id="af453-174">Další informace naleznete v tématu [Poradce při potížích](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="af453-174">For more information, see [Troubleshooting](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting).</span></span>

## <a name="remove-hdinsight-applications"></a><span data-ttu-id="af453-175">Odstranění aplikací HDInsight</span><span class="sxs-lookup"><span data-stu-id="af453-175">Remove HDInsight applications</span></span>
<span data-ttu-id="af453-176">Existuje několik způsobů jak odstranit aplikace HDInsight.</span><span class="sxs-lookup"><span data-stu-id="af453-176">There are several ways to delete HDInsight applications.</span></span>

### <a name="use-portal"></a><span data-ttu-id="af453-177">Použít portál</span><span class="sxs-lookup"><span data-stu-id="af453-177">Use portal</span></span>
<span data-ttu-id="af453-178">**Postup odebrání aplikace pomocí portálu**</span><span class="sxs-lookup"><span data-stu-id="af453-178">**To remove an application using the portal**</span></span>

1. <span data-ttu-id="af453-179">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="af453-179">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="af453-180">Klikněte na tlačítko **Clustery HDInsight** v levé nabídce.</span><span class="sxs-lookup"><span data-stu-id="af453-180">Click **HDInsight Clusters** in the left menu.</span></span>  <span data-ttu-id="af453-181">Pokud ho nevidíte, klikněte na tlačítko **Procházet** a pak klikněte na tlačítko **Clustery HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="af453-181">If you don't see it, click **Browse**, and then click **HDInsight Clusters**.</span></span>
3. <span data-ttu-id="af453-182">Klikněte na cluster, kde je nainstalovaná aplikace.</span><span class="sxs-lookup"><span data-stu-id="af453-182">Click the cluster where you installed the application.</span></span>
4. <span data-ttu-id="af453-183">Z okna **Nastavení** klikněte na tlačítko **Aplikace** pod kategorií **Obecné**.</span><span class="sxs-lookup"><span data-stu-id="af453-183">From the **Settings** blade, click **Applications** under the **General** category.</span></span> <span data-ttu-id="af453-184">Zobrazí se seznam nainstalovaných aplikací.</span><span class="sxs-lookup"><span data-stu-id="af453-184">You shall see a list of installed application.</span></span> <span data-ttu-id="af453-185">Pro toto školení je položka **hue** uvedena v okně **Nainstalované aplikace**.</span><span class="sxs-lookup"><span data-stu-id="af453-185">For this tutorial, **hue** listed in the **Installed Apps** blade.</span></span>
5. <span data-ttu-id="af453-186">Klikněte pravým tlačítkem na aplikaci, kterou chcete odebrat, a pak klikněte na tlačítko **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="af453-186">Right-click the application you want to remove, and then click **Delete**.</span></span>
6. <span data-ttu-id="af453-187">Pro potvrzení klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="af453-187">Click **Yes** to confirm.</span></span>

<span data-ttu-id="af453-188">Z portálu můžete také odstranit cluster nebo odstranit skupinu prostředků, která obsahuje aplikaci.</span><span class="sxs-lookup"><span data-stu-id="af453-188">From the portal, you can also delete the cluster or delete the resource group which contains the application.</span></span>

### <a name="use-azure-powershell"></a><span data-ttu-id="af453-189">Použití Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="af453-189">Use Azure PowerShell</span></span>
<span data-ttu-id="af453-190">Pomocí Azure PowerShell můžete odstranit cluster nebo skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="af453-190">Using Azure PowerShell, you can delete the cluster or delete the resource group.</span></span> <span data-ttu-id="af453-191">Viz téma [Odstranění clusterů pomocí Azure PowerShell](hdinsight-administer-use-powershell.md#delete-clusters).</span><span class="sxs-lookup"><span data-stu-id="af453-191">See [Delete clusters by using Azure PowerShell](hdinsight-administer-use-powershell.md#delete-clusters).</span></span>

### <a name="use-azure-cli"></a><span data-ttu-id="af453-192">Použití Azure CLI</span><span class="sxs-lookup"><span data-stu-id="af453-192">Use Azure CLI</span></span>
<span data-ttu-id="af453-193">Pomocí Azure CLI můžete odstranit cluster nebo skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="af453-193">Using Azure CLI, you can delete the cluster or delete the resource group.</span></span> <span data-ttu-id="af453-194">Viz téma [Odstranění clusterů pomocí Azure CLI](hdinsight-administer-use-command-line.md#delete-clusters).</span><span class="sxs-lookup"><span data-stu-id="af453-194">See [Delete clusters by using Azure CLI](hdinsight-administer-use-command-line.md#delete-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="af453-195">Další kroky</span><span class="sxs-lookup"><span data-stu-id="af453-195">Next steps</span></span>
* <span data-ttu-id="af453-196">[MSDN: Instalace aplikace HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): další informace jak vyvíjet šablony Resource Manageru pro nasazení aplikací HDInsight.</span><span class="sxs-lookup"><span data-stu-id="af453-196">[MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx): learn how to develop Resource Manager templates for deploying HDInsight applications.</span></span>
* <span data-ttu-id="af453-197">[Instalace aplikací HDInsight](hdinsight-apps-install-applications.md): Naučte se instalovat aplikace HDInsight do svých clusterů.</span><span class="sxs-lookup"><span data-stu-id="af453-197">[Install HDInsight applications](hdinsight-apps-install-applications.md): Learn how to install an HDInsight application to your clusters.</span></span>
* <span data-ttu-id="af453-198">[Publikování aplikací HDInsight](hdinsight-apps-publish-applications.md): Zjistěte, jak publikovat vlastní aplikace HDInsight do obchodu Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="af453-198">[Publish HDInsight applications](hdinsight-apps-publish-applications.md): Learn how to publish your custom HDInsight applications to Azure Marketplace.</span></span>
* <span data-ttu-id="af453-199">[Přizpůsobení clusterů HDInsight v systému Linux pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md): další informace o použití akce skriptu k instalaci dalších aplikací.</span><span class="sxs-lookup"><span data-stu-id="af453-199">[Customize Linux-based HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md): learn how to use Script Action to install additional applications.</span></span>
* <span data-ttu-id="af453-200">[Vytváření clusterů Hadoop na systému Linux v HDInsight pomocí šablon Resource Manageru](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Zjistěte, jak voláním šablon Resource Manageru vytvoříte clustery HDInsight.</span><span class="sxs-lookup"><span data-stu-id="af453-200">[Create Linux-based Hadoop clusters in HDInsight using Resource Manager templates](hdinsight-hadoop-create-linux-clusters-arm-templates.md): learn how to call Resource Manager templates to create HDInsight clusters.</span></span>
* <span data-ttu-id="af453-201">[Použití prázdných hraničních uzlů v HDInsight](hdinsight-apps-use-edge-node.md): Zjistěte, jak lze pomocí prázdných hraničních uzlů přistupovat ke clusteru HDInsight, testovat aplikace HDInsight a hostovat aplikace HDInsight.</span><span class="sxs-lookup"><span data-stu-id="af453-201">[Use empty edge nodes in HDInsight](hdinsight-apps-use-edge-node.md): learn how to use an empty edge node for accessing HDInsight cluster, testing HDInsight applications, and hosting HDInsight applications.</span></span>