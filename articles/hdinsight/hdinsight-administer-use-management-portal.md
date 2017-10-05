---
title: "Správa clusterů Hadoop založených na systému Windows v prostředí HDInsight pomocí portálu Azure | Microsoft Docs"
description: "Zjistěte, jak spravovat služby HDInsight. Vytvoření clusteru HDInsight, otevřete konzolu pro interaktivní JavaScript a otevřete konzolu pro příkaz Hadoop."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 9295a988-bd88-453a-8c8b-55fa103bf39c
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: f69fa4f838b22ccbb25186c08cac9744bb31c6d1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-windows-based-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a><span data-ttu-id="25bbd-104">Správa clusterů Hadoop založených na systému Windows v prostředí HDInsight pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="25bbd-104">Manage Windows-based Hadoop clusters in HDInsight by using the Azure portal</span></span>

<span data-ttu-id="25bbd-105">Pomocí [portál Azure][azure-portal], můžete vytvořit clustery se systémem Windows Hadoop v Azure HDInsight, změnit heslo uživatele Hadoop a povolit protokol RDP (Remote Desktop), aby příkaz Hadoop je k dispozici konzoly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="25bbd-105">Using the [Azure portal][azure-portal], you can create Windows-based Hadoop clusters in Azure HDInsight, change Hadoop user password, and enable Remote Desktop Protocol (RDP) so you can access the Hadoop command console on the cluster.</span></span>

<span data-ttu-id="25bbd-106">Informace v tomto článku se vztahují pouze na clustery HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="25bbd-106">The information in this article only applies to Window-based HDInsight clusters.</span></span> <span data-ttu-id="25bbd-107">Informace o správě clusterech se systémem Linux naleznete v tématu [Správa clusterů systému Hadoop v HDInsight pomocí portálu Azure](hdinsight-administer-use-portal-linux.md).</span><span class="sxs-lookup"><span data-stu-id="25bbd-107">For information on managing Linux-based clusters, see [Manage Hadoop clusters in HDInsight by using the Azure portal](hdinsight-administer-use-portal-linux.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="25bbd-108">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="25bbd-108">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="25bbd-109">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="25bbd-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="25bbd-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="25bbd-110">Prerequisites</span></span>

<span data-ttu-id="25bbd-111">Je nutné, abyste před zahájením tohoto článku měli tyto položky:</span><span class="sxs-lookup"><span data-stu-id="25bbd-111">Before you begin this article, you must have the following:</span></span>

* <span data-ttu-id="25bbd-112">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-112">**An Azure subscription**.</span></span> <span data-ttu-id="25bbd-113">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="25bbd-113">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="25bbd-114">**Účet služby Azure Storage** -clusteru HDInsight používá kontejner úložiště objektů Blob v Azure jako výchozí systém souborů.</span><span class="sxs-lookup"><span data-stu-id="25bbd-114">**Azure Storage account** - An HDInsight cluster uses an Azure Blob storage container as the default file system.</span></span> <span data-ttu-id="25bbd-115">Další informace o tom, jak Azure Blob storage nabízí jednotné prostředí s clustery HDInsight najdete v tématu [použití Azure Blob Storage s HDInsight](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="25bbd-115">For more information about how Azure Blob storage provides a seamless experience with HDInsight clusters, see [Use Azure Blob Storage with HDInsight](hdinsight-hadoop-use-blob-storage.md).</span></span> <span data-ttu-id="25bbd-116">Podrobnosti o vytvoření účtu Azure Storage najdete v tématu [postup vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="25bbd-116">For details on creating an Azure Storage account, see [How to Create a Storage Account](../storage/common/storage-create-storage-account.md).</span></span>

## <a name="open-the-portal"></a><span data-ttu-id="25bbd-117">Otevřete portál</span><span class="sxs-lookup"><span data-stu-id="25bbd-117">Open the Portal</span></span>
1. <span data-ttu-id="25bbd-118">Přihlaste se k [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="25bbd-118">Sign in to [https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="25bbd-119">Po otevření portálu můžete:</span><span class="sxs-lookup"><span data-stu-id="25bbd-119">After you open the portal, you can:</span></span>

   * <span data-ttu-id="25bbd-120">Klikněte na tlačítko **nový** v levé nabídce na vytvoření nového clusteru:</span><span class="sxs-lookup"><span data-stu-id="25bbd-120">Click **New** from the left menu to create a new cluster:</span></span>

       ![tlačítko Nový cluster HDInsight](./media/hdinsight-administer-use-management-portal/azure-portal-new-button.png)
   * <span data-ttu-id="25bbd-122">Klikněte na tlačítko **clustery HDInsight** v levé nabídce.</span><span class="sxs-lookup"><span data-stu-id="25bbd-122">Click **HDInsight Clusters** from the left menu.</span></span>

       ![Azure portálu tlačítko clusteru HDInsight](./media/hdinsight-administer-use-management-portal/azure-portal-hdinsight-button.png)

     <span data-ttu-id="25bbd-124">Pokud **HDInsight** nebude zobrazovat v nabídce vlevo, klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-124">If **HDInsight** doesn't appear in the left menu, click **Browse**.</span></span>

     ![Tlačítko Procházet clusteru Azure portálu](./media/hdinsight-administer-use-management-portal/azure-portal-browse-button.png)

## <a name="create-clusters"></a><span data-ttu-id="25bbd-126">Vytváření clusterů</span><span class="sxs-lookup"><span data-stu-id="25bbd-126">Create clusters</span></span>
<span data-ttu-id="25bbd-127">Postup vytvoření, pomocí portálu naleznete v tématu [Tvorba clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="25bbd-127">For the creation instructions using the Portal, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

<span data-ttu-id="25bbd-128">HDInsight funguje s komponentami široký rozsah Hadoop.</span><span class="sxs-lookup"><span data-stu-id="25bbd-128">HDInsight works with a wide range of Hadoop components.</span></span> <span data-ttu-id="25bbd-129">Seznam součástí, které byly ověřeny a podporovány, naleznete v části [jaká verze Hadoop je v Azure HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="25bbd-129">For the list of the components that have been verified and supported, see [What version of Hadoop is in Azure HDInsight](hdinsight-component-versioning.md).</span></span> <span data-ttu-id="25bbd-130">HDInsight můžete přizpůsobit pomocí jedné z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="25bbd-130">You can customize HDInsight by using one of the following options:</span></span>

* <span data-ttu-id="25bbd-131">Chcete-li spustit vlastní skripty, které můžete přizpůsobit cluster změnit konfiguraci clusteru nebo nainstalovat vlastní komponenty, například Giraph nebo Solr použití akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="25bbd-131">Use Script Action to run custom scripts that can customize a cluster to either change cluster configuration or install custom components such as Giraph or Solr.</span></span> <span data-ttu-id="25bbd-132">Další informace najdete v tématu [clusteru HDInsight přizpůsobit pomocí akce skriptu](hdinsight-hadoop-customize-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="25bbd-132">For more information, see [Customize HDInsight cluster using Script Action](hdinsight-hadoop-customize-cluster.md).</span></span>
* <span data-ttu-id="25bbd-133">Při vytváření clusteru použijte vlastní nastavení parametrů clusteru v rozhraní .NET SDK HDInsight nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="25bbd-133">Use the cluster customization parameters in the HDInsight .NET SDK or Azure PowerShell during cluster creation.</span></span> <span data-ttu-id="25bbd-134">Tyto změny konfigurace se pak zachovají prostřednictvím dobu životnosti clusteru a nemá vliv reimages uzlu clusteru, které platformy Azure pravidelně provádí za účelem údržby.</span><span class="sxs-lookup"><span data-stu-id="25bbd-134">These configuration changes are then preserved through the lifetime of the cluster and are not affected by cluster node reimages that Azure platform periodically performs for maintenance.</span></span> <span data-ttu-id="25bbd-135">Další informace o používání vlastního nastavení parametrů clusteru najdete v tématu [Tvorba clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="25bbd-135">For more information on using the cluster customization parameters, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span>
* <span data-ttu-id="25bbd-136">Některé nativní součásti Java, jako je Mahout a možností konfigurace, můžete spustit v clusteru jako souborů JAR.</span><span class="sxs-lookup"><span data-stu-id="25bbd-136">Some native Java components, like Mahout and Cascading, can be run on the cluster as JAR files.</span></span> <span data-ttu-id="25bbd-137">Tyto soubory JAR můžete distribuovat do úložiště objektů Blob v Azure a odeslat ke clusterům HDInsight prostřednictvím mechanismy odesílání úloh Hadoop.</span><span class="sxs-lookup"><span data-stu-id="25bbd-137">These JAR files can be distributed to Azure Blob storage, and submitted to HDInsight clusters through Hadoop job submission mechanisms.</span></span> <span data-ttu-id="25bbd-138">Další informace najdete v tématu [Hadoop odeslání úlohy prostřednictvím kódu programu](hdinsight-submit-hadoop-jobs-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="25bbd-138">For more information, see [Submit Hadoop jobs programmatically](hdinsight-submit-hadoop-jobs-programmatically.md).</span></span>

  > [!NOTE]
  > <span data-ttu-id="25bbd-139">Pokud máte problémy s nasazením souborů JAR ke clusterům HDInsight nebo volání souborů JAR na clusterech HDInsight, obraťte se na [Microsoft Support](https://azure.microsoft.com/support/options/).</span><span class="sxs-lookup"><span data-stu-id="25bbd-139">If you have issues deploying JAR files to HDInsight clusters or calling JAR files on HDInsight clusters, contact [Microsoft Support](https://azure.microsoft.com/support/options/).</span></span>
  >
  > <span data-ttu-id="25bbd-140">Kaskádových není podporována v prostředí HDInsight a nejsou vhodné pro systém Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="25bbd-140">Cascading is not supported by HDInsight, and is not eligible for Microsoft Support.</span></span> <span data-ttu-id="25bbd-141">Seznam podporovaných součásti, najdete v části [co je nového ve verzích clusterů poskytovaných v HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="25bbd-141">For lists of supported components, see [What's new in the cluster versions provided by HDInsight](hdinsight-component-versioning.md).</span></span>
  >
  >

<span data-ttu-id="25bbd-142">Instalace vlastních softwaru v clusteru pomocí připojení ke vzdálené ploše není podporována.</span><span class="sxs-lookup"><span data-stu-id="25bbd-142">Installation of custom software on the cluster by using Remote Desktop Connection is not supported.</span></span> <span data-ttu-id="25bbd-143">Neměli byste ukládat soubory na jednotkách hlavního uzlu, jak budou ztraceny, budete muset znovu vytvořit clustery.</span><span class="sxs-lookup"><span data-stu-id="25bbd-143">You should avoid storing any files on the drives of the head node, as they will be lost if you need to re-create the clusters.</span></span> <span data-ttu-id="25bbd-144">Doporučujeme ukládat soubory do úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="25bbd-144">We recommend storing files on Azure Blob storage.</span></span> <span data-ttu-id="25bbd-145">Úložiště objektů blob je trvalé.</span><span class="sxs-lookup"><span data-stu-id="25bbd-145">Blob storage is persistent.</span></span>

## <a name="list-and-show-clusters"></a><span data-ttu-id="25bbd-146">Seznam a zobrazit clustery</span><span class="sxs-lookup"><span data-stu-id="25bbd-146">List and show clusters</span></span>
1. <span data-ttu-id="25bbd-147">Přihlaste se k [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="25bbd-147">Sign in to [https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="25bbd-148">Klikněte na tlačítko **clustery HDInsight** v levé nabídce.</span><span class="sxs-lookup"><span data-stu-id="25bbd-148">Click **HDInsight Clusters** from the left menu.</span></span>
3. <span data-ttu-id="25bbd-149">Klikněte na název clusteru.</span><span class="sxs-lookup"><span data-stu-id="25bbd-149">Click the cluster name.</span></span> <span data-ttu-id="25bbd-150">Pokud je seznam clusteru dlouho, můžete použít možnosti filtrovat horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="25bbd-150">If the cluster list is long, you can use filter on the top of the page.</span></span>
4. <span data-ttu-id="25bbd-151">Dvakrát klikněte na cluster ze seznamu a zobrazit podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="25bbd-151">Double-click a cluster from the list to show the details.</span></span>

    <span data-ttu-id="25bbd-152">**Nabídky a essentials**:</span><span class="sxs-lookup"><span data-stu-id="25bbd-152">**Menu and essentials**:</span></span>

    ![Azure portálu essentials clusteru HDInsight](./media/hdinsight-administer-use-management-portal/hdinsight-essentials.png)

   * <span data-ttu-id="25bbd-154">Upravit nabídku, klikněte pravým tlačítkem na libovolné místo v nabídce a pak klikněte na tlačítko **přizpůsobit**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-154">To customize the menu, right-click anywhere on the menu, and then click **Customize**.</span></span>
   * <span data-ttu-id="25bbd-155">**Nastavení** a **všechna nastavení**: zobrazí **nastavení** okno pro cluster, který umožňuje zobrazit podrobné informace o konfiguraci pro cluster.</span><span class="sxs-lookup"><span data-stu-id="25bbd-155">**Settings** and **All Settings**: Displays the **Settings** blade for the cluster, which allows you to access detailed configuration information for the cluster.</span></span>
   * <span data-ttu-id="25bbd-156">**Řídicí panel**, **řídicí panel clusteru** a **adresy URL: Toto jsou všechny způsoby, jak přístup k řídicímu panelu clusteru, který je Ambari Web pro clustery se systémem Linux. -**Zabezpečené prostředí **: Zobrazí pokyny pro připojení ke clusteru pomocí připojení Secure Shell (SSH).</span><span class="sxs-lookup"><span data-stu-id="25bbd-156">**Dashboard**, **Cluster Dashboard** and **URL: These are all ways to access the cluster dashboard, which is Ambari Web for Linux-based clusters. -**Secure Shell**: Shows the instructions to connect to the cluster using Secure Shell (SSH) connection.</span></span>
   * <span data-ttu-id="25bbd-157">**Škálování clusteru**: umožňuje změnit počet uzlů pracovního procesu pro tento cluster.</span><span class="sxs-lookup"><span data-stu-id="25bbd-157">**Scale Cluster**: Allows you to change the number of worker nodes for this cluster.</span></span>
   * <span data-ttu-id="25bbd-158">**Odstranit**: Odstraní clusteru.</span><span class="sxs-lookup"><span data-stu-id="25bbd-158">**Delete**: Deletes the cluster.</span></span>
   * <span data-ttu-id="25bbd-159">**Rychlý Start**: Zobrazí informace, které vám pomohou začněte používat HDInsight.</span><span class="sxs-lookup"><span data-stu-id="25bbd-159">**Quickstart**: Displays information that will help you get started using HDInsight.</span></span>
   * <span data-ttu-id="25bbd-160">** Uživatele: Umožňuje nastavit oprávnění pro *portálu správy* tohoto clusteru pro ostatní uživatele ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="25bbd-160">**Users: Allows you to set permissions for *portal management* of this cluster for other users on your Azure subscription.</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="25bbd-161">To *pouze* ovlivňuje oprávnění a přístupová práva k tomuto clusteru na portálu Azure a nemá žádný vliv na který můžete připojit k nebo odesílání úloh do clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="25bbd-161">This *only* affects access and permissions to this cluster in the Azure portal, and has no effect on who can connect to or submit jobs to the HDInsight cluster.</span></span>
     >
     >
   * <span data-ttu-id="25bbd-162">**Značky**: značky umožňují nastavit páry klíč/hodnota k definování vlastní taxonomii cloudových služeb.</span><span class="sxs-lookup"><span data-stu-id="25bbd-162">**Tags**: Tags allow you to set key/value pairs to define a custom taxonomy of your cloud services.</span></span> <span data-ttu-id="25bbd-163">Můžete například vytvořit klíč s názvem **projektu**a potom používat běžné hodnotu pro všechny služby související s konkrétní projekt.</span><span class="sxs-lookup"><span data-stu-id="25bbd-163">For example, you may create a key named **project**, and then use a common value for all services associated with a specific project.</span></span>
   * <span data-ttu-id="25bbd-164">**Zobrazení Ambari**: odkazy na Ambari Web.</span><span class="sxs-lookup"><span data-stu-id="25bbd-164">**Ambari Views**: Links to Ambari Web.</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="25bbd-165">Ke správě služeb poskytovaných clusteru HDInsight, je nutné použít Ambari Web nebo Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="25bbd-165">To manage the services provided by the HDInsight cluster, you must use Ambari Web or the Ambari REST API.</span></span> <span data-ttu-id="25bbd-166">Další informace o používání Ambari najdete v tématu [Správa clusterů HDInsight pomocí Ambari](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="25bbd-166">For more information on using Ambari, see [Manage HDInsight clusters using Ambari](hdinsight-hadoop-manage-ambari.md).</span></span>
     >
     >

     <span data-ttu-id="25bbd-167">**Využití**:</span><span class="sxs-lookup"><span data-stu-id="25bbd-167">**Usage**:</span></span>

     ![Azure portálu použití clusteru HDInsight](./media/hdinsight-administer-use-management-portal/hdinsight-portal-cluster-usage.png)
5. <span data-ttu-id="25bbd-169">Klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-169">Click **Settings**.</span></span>

    ![Azure portálu použití clusteru HDInsight](./media/hdinsight-administer-use-management-portal/hdinsight.portal.cluster.settings.png)

   * <span data-ttu-id="25bbd-171">**Vlastnosti**: zobrazení vlastností clusteru.</span><span class="sxs-lookup"><span data-stu-id="25bbd-171">**Properties**: View the cluster properties.</span></span>
   * <span data-ttu-id="25bbd-172">**Identita AAD clusteru**:</span><span class="sxs-lookup"><span data-stu-id="25bbd-172">**Cluster AAD Identity**:</span></span>
   * <span data-ttu-id="25bbd-173">**Klíče k úložišti Azure**: Zobrazit výchozí účet úložiště a jeho klíčem.</span><span class="sxs-lookup"><span data-stu-id="25bbd-173">**Azure Storage Keys**: View the default storage account and its key.</span></span> <span data-ttu-id="25bbd-174">Účet úložiště je konfigurace během procesu vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="25bbd-174">The storage account is configuration during the cluster creation process.</span></span>
   * <span data-ttu-id="25bbd-175">**Cluster přihlášení**: Změňte clusteru HTTP uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="25bbd-175">**Cluster Login**: Change the cluster HTTP user name and password.</span></span>
   * <span data-ttu-id="25bbd-176">**Externí Metaúložiště**: Zobrazit metaúložiště Hive a Oozie.</span><span class="sxs-lookup"><span data-stu-id="25bbd-176">**External Metastores**: View the Hive and Oozie metastores.</span></span> <span data-ttu-id="25bbd-177">Metaúložiště se dá nakonfigurovat jenom během procesu vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="25bbd-177">The metastores can only be configured during the cluster creation process.</span></span>
   * <span data-ttu-id="25bbd-178">**Škálování clusteru**: Zvyšte a snižte počet uzlů pracovního procesu clusteru.</span><span class="sxs-lookup"><span data-stu-id="25bbd-178">**Scale Cluster**: Increase and decrease the number of cluster worker nodes.</span></span>
   * <span data-ttu-id="25bbd-179">**Vzdálená plocha**: Povolit a zakázat přístup ke vzdálené ploše (RDP) a nakonfigurovat uživatelské jméno protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="25bbd-179">**Remote Desktop**: Enable and disable remote desktop (RDP) access, and configure the RDP username.</span></span>  <span data-ttu-id="25bbd-180">Uživatelské jméno protokolu RDP musí být odlišný od uživatelské jméno protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="25bbd-180">The RDP user name must be different from the HTTP user name.</span></span>
   * <span data-ttu-id="25bbd-181">**Preferovaného partnera**:</span><span class="sxs-lookup"><span data-stu-id="25bbd-181">**Partner of Record**:</span></span>

     > [!NOTE]
     > <span data-ttu-id="25bbd-182">Toto je obecná seznam dostupných nastavení; Ne všechny z nich bude k dispozici pro všechny typy clusteru.</span><span class="sxs-lookup"><span data-stu-id="25bbd-182">This is a generic list of available settings; not all of them will be present for all cluster types.</span></span>
     >
     >
6. <span data-ttu-id="25bbd-183">Klikněte na tlačítko **vlastnosti**:</span><span class="sxs-lookup"><span data-stu-id="25bbd-183">Click **Properties**:</span></span>

    <span data-ttu-id="25bbd-184">V části vlastnosti jsou uvedeny následující:</span><span class="sxs-lookup"><span data-stu-id="25bbd-184">The properties section lists the following:</span></span>

   * <span data-ttu-id="25bbd-185">**Název hostitele**: název clusteru.</span><span class="sxs-lookup"><span data-stu-id="25bbd-185">**Hostname**: Cluster name.</span></span>
   * <span data-ttu-id="25bbd-186">**Adresa URL clusteru**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-186">**Cluster URL**.</span></span>
   * <span data-ttu-id="25bbd-187">**Stav**: zahrnují byl zrušen, přijata, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, provozní, spuštěna, chyba, odstraňování, odstranit, Timedout, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, ClusterCustomization CertRolloverQueued, ResizeQueued,</span><span class="sxs-lookup"><span data-stu-id="25bbd-187">**Status**: Include Aborted, Accepted, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, Operational, Running, Error, Deleting, Deleted, Timedout, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued, ClusterCustomization</span></span>
   * <span data-ttu-id="25bbd-188">**Oblast**: umístění Azure.</span><span class="sxs-lookup"><span data-stu-id="25bbd-188">**Region**: Azure location.</span></span> <span data-ttu-id="25bbd-189">Seznam podporovaných umístění Azure, najdete v článku **oblast** rozevírací pole se seznamem na [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="25bbd-189">For a list of supported Azure locations, see the **Region** dropdown list box on [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>
   * <span data-ttu-id="25bbd-190">**Data vytvořená**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-190">**Data created**.</span></span>
   * <span data-ttu-id="25bbd-191">**Operační systém**: buď **Windows** nebo **Linux**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-191">**Operating system**: Either **Windows** or **Linux**.</span></span>
   * <span data-ttu-id="25bbd-192">**Typ**: Hadoop, HBase, Storm, Spark.</span><span class="sxs-lookup"><span data-stu-id="25bbd-192">**Type**: Hadoop, HBase, Storm, Spark.</span></span>
   * <span data-ttu-id="25bbd-193">**Verze**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-193">**Version**.</span></span> <span data-ttu-id="25bbd-194">V tématu [HDInsight verze](hdinsight-component-versioning.md)</span><span class="sxs-lookup"><span data-stu-id="25bbd-194">See [HDInsight versions](hdinsight-component-versioning.md)</span></span>
   * <span data-ttu-id="25bbd-195">**Předplatné**: název odběru.</span><span class="sxs-lookup"><span data-stu-id="25bbd-195">**Subscription**: Subscription name.</span></span>
   * <span data-ttu-id="25bbd-196">**ID předplatného**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-196">**Subscription ID**.</span></span>
   * <span data-ttu-id="25bbd-197">**Primární zdroj dat**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-197">**Primary data source**.</span></span> <span data-ttu-id="25bbd-198">Účet úložiště objektů Blob v Azure použito jako výchozí systém souborů Hadoop.</span><span class="sxs-lookup"><span data-stu-id="25bbd-198">The Azure Blob storage account used as the default Hadoop file system.</span></span>
   * <span data-ttu-id="25bbd-199">**Pracovní uzly cenová úroveň**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-199">**Worker nodes pricing tier**.</span></span>
   * <span data-ttu-id="25bbd-200">**Hlavní uzel cenová úroveň**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-200">**Head node pricing tier**.</span></span>

## <a name="delete-clusters"></a><span data-ttu-id="25bbd-201">Odstranění clusterů</span><span class="sxs-lookup"><span data-stu-id="25bbd-201">Delete clusters</span></span>
<span data-ttu-id="25bbd-202">Odstranění clusteru se neodstraní výchozí účet úložiště nebo všechny propojené účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="25bbd-202">Delete a cluster will not delete the default storage account or any linked storage accounts.</span></span> <span data-ttu-id="25bbd-203">Clusteru můžete znovu vytvořit pomocí stejné účty úložiště a stejné metaúložiště.</span><span class="sxs-lookup"><span data-stu-id="25bbd-203">You can re-create the cluster by using the same storage accounts and the same metastores.</span></span>

1. <span data-ttu-id="25bbd-204">Přihlaste se k [portál][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="25bbd-204">Sign in to the [Portal][azure-portal].</span></span>
2. <span data-ttu-id="25bbd-205">Klikněte na tlačítko **Procházet vše** v levé nabídce klikněte na tlačítko **clustery HDInsight**, klikněte na název clusteru.</span><span class="sxs-lookup"><span data-stu-id="25bbd-205">Click **Browse All** from the left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="25bbd-206">Klikněte na tlačítko **odstranit** z hlavní nabídky a pak postupujte podle pokynů.</span><span class="sxs-lookup"><span data-stu-id="25bbd-206">Click **Delete** from the top menu, and then follow the instructions.</span></span>

<span data-ttu-id="25bbd-207">Viz také [pozastavení nebo vypnutí clustery](#pauseshut-down-clusters).</span><span class="sxs-lookup"><span data-stu-id="25bbd-207">See also [Pause/shut down clusters](#pauseshut-down-clusters).</span></span>

## <a name="scale-clusters"></a><span data-ttu-id="25bbd-208">Škálování clusterů</span><span class="sxs-lookup"><span data-stu-id="25bbd-208">Scale clusters</span></span>
<span data-ttu-id="25bbd-209">Funkce škálování clusteru umožňuje změnit počet uzlů pracovního procesu používá cluster, který běží v Azure HDInsight bez nutnosti znovu vytvořit cluster.</span><span class="sxs-lookup"><span data-stu-id="25bbd-209">The cluster scaling feature allows you to change the number of worker nodes used by a cluster that is running in Azure HDInsight without having to re-create the cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="25bbd-210">Pouze clustery s HDInsight verze 3.1.3 nebo vyšší nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="25bbd-210">Only clusters with HDInsight version 3.1.3 or higher are supported.</span></span> <span data-ttu-id="25bbd-211">Pokud si nejste jistí na verzi vašeho clusteru, můžete zkontrolovat stránku vlastností.</span><span class="sxs-lookup"><span data-stu-id="25bbd-211">If you are unsure of the version of your cluster, you can check the Properties page.</span></span>  <span data-ttu-id="25bbd-212">V tématu [seznamu a zobrazit clustery](#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="25bbd-212">See [List and show clusters](#list-and-show-clusters).</span></span>
>
>

<span data-ttu-id="25bbd-213">Dopad změny v počtu uzlů dat pro každý typ clusteru podporuje HDInsight:</span><span class="sxs-lookup"><span data-stu-id="25bbd-213">The impact of changing the number of data nodes for each type of cluster supported by HDInsight:</span></span>

* <span data-ttu-id="25bbd-214">Hadoop</span><span class="sxs-lookup"><span data-stu-id="25bbd-214">Hadoop</span></span>

    <span data-ttu-id="25bbd-215">Můžete bez problémů zvýšit počet uzlů pracovního procesu v clusteru Hadoop, který běží bez dopadu na všechny úlohy čekající na vyřízení nebo spuštěné.</span><span class="sxs-lookup"><span data-stu-id="25bbd-215">You can seamlessly increase the number of worker nodes in a Hadoop cluster that is running without impacting any pending or running jobs.</span></span> <span data-ttu-id="25bbd-216">Nové úlohy můžete také odeslány, když probíhá operace.</span><span class="sxs-lookup"><span data-stu-id="25bbd-216">New jobs can also be submitted while the operation is in progress.</span></span> <span data-ttu-id="25bbd-217">Selhání v rámci operace škálování pohodlné zpracování tak, aby cluster zůstane vždy ve funkčním stavu.</span><span class="sxs-lookup"><span data-stu-id="25bbd-217">Failures in a scaling operation are gracefully handled so that the cluster is always left in a functional state.</span></span>

    <span data-ttu-id="25bbd-218">Pokud se Hadoop cluster měřítko snížením počtu uzlů data, některé služby v clusteru restartovat.</span><span class="sxs-lookup"><span data-stu-id="25bbd-218">When a Hadoop cluster is scaled down by reducing the number of data nodes, some of the services in the cluster are restarted.</span></span> <span data-ttu-id="25bbd-219">To způsobí, že všechny spuštěné a čeká se na úlohy selhání po dokončení operace škálování.</span><span class="sxs-lookup"><span data-stu-id="25bbd-219">This causes all running and pending jobs to fail at the completion of the scaling operation.</span></span> <span data-ttu-id="25bbd-220">Můžete, ale odešlete znovu úloh po dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="25bbd-220">You can, however, resubmit the jobs once the operation is complete.</span></span>
* <span data-ttu-id="25bbd-221">HBase</span><span class="sxs-lookup"><span data-stu-id="25bbd-221">HBase</span></span>

    <span data-ttu-id="25bbd-222">Bezproblémově můžete přidávat nebo odebírat uzly do clusteru HBase, když je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="25bbd-222">You can seamlessly add or remove nodes to your HBase cluster while it is running.</span></span> <span data-ttu-id="25bbd-223">Místní servery jsou automaticky vyváženy během několika minut po dokončení operace škálování.</span><span class="sxs-lookup"><span data-stu-id="25bbd-223">Regional Servers are automatically balanced within a few minutes of completing the scaling operation.</span></span> <span data-ttu-id="25bbd-224">Protokolování do headnode clusteru a spuštěním následujících příkazů z okna příkazového řádku však můžete také ručně vyvážit místní servery:</span><span class="sxs-lookup"><span data-stu-id="25bbd-224">However, you can also manually balance the regional servers by logging into the headnode of cluster and running the following commands from a command prompt window:</span></span>

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

    <span data-ttu-id="25bbd-225">Další informace o používání prostředí HBase naleznete v tématu]</span><span class="sxs-lookup"><span data-stu-id="25bbd-225">For more information on using the HBase shell, see []</span></span>
* <span data-ttu-id="25bbd-226">Storm</span><span class="sxs-lookup"><span data-stu-id="25bbd-226">Storm</span></span>

    <span data-ttu-id="25bbd-227">Můžete bezproblémově přidávat nebo odebírat uzly dat do clusteru Storm, když je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="25bbd-227">You can seamlessly add or remove data nodes to your Storm cluster while it is running.</span></span> <span data-ttu-id="25bbd-228">Ale po úspěšném dokončení operace škálování, budete muset znovu vyvážit topologii.</span><span class="sxs-lookup"><span data-stu-id="25bbd-228">But after a successful completion of the scaling operation, you will need to rebalance the topology.</span></span>

    <span data-ttu-id="25bbd-229">Vyrovnává lze dosáhnout dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="25bbd-229">Rebalancing can be accomplished in two ways:</span></span>

  * <span data-ttu-id="25bbd-230">Storm webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="25bbd-230">Storm web UI</span></span>
  * <span data-ttu-id="25bbd-231">Nástroj pro rozhraní příkazového řádku (CLI)</span><span class="sxs-lookup"><span data-stu-id="25bbd-231">Command-line interface (CLI) tool</span></span>

    <span data-ttu-id="25bbd-232">Podrobnosti najdete [dokumentaci Apache Storm](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="25bbd-232">Please refer to the [Apache Storm documentation](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) for more details.</span></span>

    <span data-ttu-id="25bbd-233">Uživatelské rozhraní Storm webu je k dispozici v clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="25bbd-233">The Storm web UI is available on the HDInsight cluster:</span></span>

    ![Obnovte rovnováhu škálování HDInsight storm](./media/hdinsight-administer-use-management-portal/hdinsight-portal-scale-cluster-storm-rebalance.png)

    <span data-ttu-id="25bbd-235">Tady je příklad jak znovu vyvážit topologie Storm pomocí rozhraní příkazového řádku příkaz:</span><span class="sxs-lookup"><span data-stu-id="25bbd-235">Here is an example how to use the CLI command to rebalance the Storm topology:</span></span>

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

<span data-ttu-id="25bbd-236">**Škálování clusterů**</span><span class="sxs-lookup"><span data-stu-id="25bbd-236">**To scale clusters**</span></span>

1. <span data-ttu-id="25bbd-237">Přihlaste se k [portál][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="25bbd-237">Sign in to the [Portal][azure-portal].</span></span>
2. <span data-ttu-id="25bbd-238">Klikněte na tlačítko **Procházet vše** v levé nabídce klikněte na tlačítko **clustery HDInsight**, klikněte na název clusteru.</span><span class="sxs-lookup"><span data-stu-id="25bbd-238">Click **Browse All** from the left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="25bbd-239">Klikněte na tlačítko **nastavení** z horní nabídce a pak klikněte na tlačítko **škálování clusteru**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-239">Click **Settings** from the top menu, and then click **Scale Cluster**.</span></span>
4. <span data-ttu-id="25bbd-240">Zadejte **číslo pracovní uzly**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-240">Enter **Number of Worker nodes**.</span></span> <span data-ttu-id="25bbd-241">Limit počtu uzlů clusteru se liší podle předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="25bbd-241">The limit on the number of cluster node varies among Azure subscriptions.</span></span> <span data-ttu-id="25bbd-242">Obraťte se na podporu fakturace o navýšení limitu.</span><span class="sxs-lookup"><span data-stu-id="25bbd-242">You can contact billing support to increase the limit.</span></span>  <span data-ttu-id="25bbd-243">Informace o nákladech se projeví změny, které jste udělali počet uzlů.</span><span class="sxs-lookup"><span data-stu-id="25bbd-243">The cost information will reflect the changes you have made to the number of nodes.</span></span>

    ![HDInsight Hadoop HBase, Storm Spark škálování](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

## <a name="pauseshut-down-clusters"></a><span data-ttu-id="25bbd-245">Pozastavení nebo vypnutí clustery</span><span class="sxs-lookup"><span data-stu-id="25bbd-245">Pause/shut down clusters</span></span>
<span data-ttu-id="25bbd-246">Většina úloh Hadoop jsou dávkové úlohy, které jsou jen občas spustili.</span><span class="sxs-lookup"><span data-stu-id="25bbd-246">Most of Hadoop jobs are batch jobs that are only ran occasionally.</span></span> <span data-ttu-id="25bbd-247">Pro většinu clusterů systému Hadoop jsou velké období clusteru není použitá ke zpracování.</span><span class="sxs-lookup"><span data-stu-id="25bbd-247">For most Hadoop clusters, there are large periods of time that the cluster is not being used for processing.</span></span> <span data-ttu-id="25bbd-248">Pomocí HDInsight jsou vaše data uložena v Azure Storage, takže můžete clusteru bezpečně odstranit, pokud není používán.</span><span class="sxs-lookup"><span data-stu-id="25bbd-248">With HDInsight, your data is stored in Azure Storage, so you can safely delete a cluster when it is not in use.</span></span>
<span data-ttu-id="25bbd-249">Za cluster služby HDInsight se účtují poplatky, i když se nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="25bbd-249">You are also charged for an HDInsight cluster, even when it is not in use.</span></span> <span data-ttu-id="25bbd-250">Vzhledem k tomu, že poplatky za cluster představují několikanásobek poplatků za úložiště, dává ekonomický smysl odstraňovat clustery, které nejsou používány.</span><span class="sxs-lookup"><span data-stu-id="25bbd-250">Since the charges for the cluster are many times more than the charges for storage, it makes economic sense to delete clusters when they are not in use.</span></span>

<span data-ttu-id="25bbd-251">Existuje mnoho způsobů, které můžete naprogramovat proces:</span><span class="sxs-lookup"><span data-stu-id="25bbd-251">There are many ways you can program the process:</span></span>

* <span data-ttu-id="25bbd-252">Uživatel pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="25bbd-252">User Azure Data Factory.</span></span> <span data-ttu-id="25bbd-253">V tématu [propojená služba Azure HDInsight](../data-factory/data-factory-compute-linked-services.md) a [transformovat a analyzovat pomocí Azure Data Factory](../data-factory/data-factory-data-transformation-activities.md) pro HDInsight na vyžádání a samoobslužné definované propojené služby.</span><span class="sxs-lookup"><span data-stu-id="25bbd-253">See [Azure HDInsight Linked Service](../data-factory/data-factory-compute-linked-services.md) and [Transform and analyze using Azure Data Factory](../data-factory/data-factory-data-transformation-activities.md) for on-demand and self-defined HDInsight linked services.</span></span>
* <span data-ttu-id="25bbd-254">Použití Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="25bbd-254">Use Azure PowerShell.</span></span>  <span data-ttu-id="25bbd-255">V tématu [analyzovat data zpoždění letu](hdinsight-analyze-flight-delay-data.md).</span><span class="sxs-lookup"><span data-stu-id="25bbd-255">See [Analyze flight delay data](hdinsight-analyze-flight-delay-data.md).</span></span>
* <span data-ttu-id="25bbd-256">Použití Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="25bbd-256">Use Azure CLI.</span></span> <span data-ttu-id="25bbd-257">V tématu [Správa clusterů HDInsight pomocí rozhraní příkazového řádku Azure](hdinsight-administer-use-command-line.md).</span><span class="sxs-lookup"><span data-stu-id="25bbd-257">See [Manage HDInsight clusters using Azure CLI](hdinsight-administer-use-command-line.md).</span></span>
* <span data-ttu-id="25bbd-258">Použití sady .NET SDK HDInsight.</span><span class="sxs-lookup"><span data-stu-id="25bbd-258">Use HDInsight .NET SDK.</span></span> <span data-ttu-id="25bbd-259">V tématu [úloh Hadoop odeslání](hdinsight-submit-hadoop-jobs-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="25bbd-259">See [Submit Hadoop jobs](hdinsight-submit-hadoop-jobs-programmatically.md).</span></span>

<span data-ttu-id="25bbd-260">Informace o cenách najdete v části [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="25bbd-260">For the pricing information, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span> <span data-ttu-id="25bbd-261">Pokud chcete odstranit cluster z portálu, přečtěte si téma [odstranění clusterů](#delete-clusters)</span><span class="sxs-lookup"><span data-stu-id="25bbd-261">To delete a cluster from the Portal, see [Delete clusters](#delete-clusters)</span></span>

## <a name="change-cluster-username"></a><span data-ttu-id="25bbd-262">Uživatelské jméno změnit clusteru</span><span class="sxs-lookup"><span data-stu-id="25bbd-262">Change cluster username</span></span>
<span data-ttu-id="25bbd-263">Cluster služby HDInsight může mít dva uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="25bbd-263">An HDInsight cluster can have two user accounts.</span></span> <span data-ttu-id="25bbd-264">Uživatelský účet clusteru HDInsight se vytvoří během procesu vytváření.</span><span class="sxs-lookup"><span data-stu-id="25bbd-264">The HDInsight cluster user account is created during the creation process.</span></span> <span data-ttu-id="25bbd-265">Můžete také vytvořit uživatelský účet protokolu RDP pro přístup ke clusteru pomocí protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="25bbd-265">You can also create an RDP user account for accessing the cluster via RDP.</span></span> <span data-ttu-id="25bbd-266">V tématu [povolení vzdálené plochy](#connect-to-hdinsight-clusters-by-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="25bbd-266">See [Enable remote desktop](#connect-to-hdinsight-clusters-by-using-rdp).</span></span>

<span data-ttu-id="25bbd-267">**Chcete-li změnit HDInsight clusteru uživatelské jméno a heslo**</span><span class="sxs-lookup"><span data-stu-id="25bbd-267">**To change the HDInsight cluster user name and password**</span></span>

1. <span data-ttu-id="25bbd-268">Přihlaste se k [portál][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="25bbd-268">Sign in to the [Portal][azure-portal].</span></span>
2. <span data-ttu-id="25bbd-269">Klikněte na tlačítko **Procházet vše** v levé nabídce klikněte na tlačítko **clustery HDInsight**, klikněte na název clusteru.</span><span class="sxs-lookup"><span data-stu-id="25bbd-269">Click **Browse All** from the left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="25bbd-270">Klikněte na tlačítko **nastavení** z horní nabídce a pak klikněte na tlačítko **clusteru přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-270">Click **Settings** from the top menu, and then click **Cluster Login**.</span></span>
4. <span data-ttu-id="25bbd-271">Pokud **clusteru přihlášení** byla povolena, musíte kliknout na **zakázat**a potom klikněte na **povolit** před změnou uživatelského jména a hesla...</span><span class="sxs-lookup"><span data-stu-id="25bbd-271">If **Cluster login** has been enabled, you must click **Disable**, and then click **Enable** before you can change the username and password..</span></span>
5. <span data-ttu-id="25bbd-272">Změna **clusteru přihlašovací jméno** nebo **clusteru přihlašovacího hesla**a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-272">Change the **Cluster Login Name** and/or the **Cluster Login Password**, and then click **Save**.</span></span>

    ![HDInsight změnit clusteru uživatel uživatelské jméno heslo http uživatele](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="grantrevoke-access"></a><span data-ttu-id="25bbd-274">Udělení nebo odvolání přístupu</span><span class="sxs-lookup"><span data-stu-id="25bbd-274">Grant/revoke access</span></span>
<span data-ttu-id="25bbd-275">Clustery HDInsight mají následující webové služby HTTP (všechny tyto služby mají RESTful koncových bodů):</span><span class="sxs-lookup"><span data-stu-id="25bbd-275">HDInsight clusters have the following HTTP web services (all of these services have RESTful endpoints):</span></span>

* <span data-ttu-id="25bbd-276">ODBC</span><span class="sxs-lookup"><span data-stu-id="25bbd-276">ODBC</span></span>
* <span data-ttu-id="25bbd-277">JDBC</span><span class="sxs-lookup"><span data-stu-id="25bbd-277">JDBC</span></span>
* <span data-ttu-id="25bbd-278">Ambari</span><span class="sxs-lookup"><span data-stu-id="25bbd-278">Ambari</span></span>
* <span data-ttu-id="25bbd-279">Oozie</span><span class="sxs-lookup"><span data-stu-id="25bbd-279">Oozie</span></span>
* <span data-ttu-id="25bbd-280">Templeton</span><span class="sxs-lookup"><span data-stu-id="25bbd-280">Templeton</span></span>

<span data-ttu-id="25bbd-281">Ve výchozím nastavení jsou tyto služby oprávnění pro přístup.</span><span class="sxs-lookup"><span data-stu-id="25bbd-281">By default, these services are granted for access.</span></span> <span data-ttu-id="25bbd-282">Vám může odvolání nebo udělit přístup z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="25bbd-282">You can revoke/grant the access from the Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="25bbd-283">Pomocí udělení nebo odvolání přístupu, obnoví clusteru uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="25bbd-283">By granting/revoking the access, you will reset the cluster user name and password.</span></span>
>
>

<span data-ttu-id="25bbd-284">**K udělení nebo odvolání přístup protokolu HTTP webové služby**</span><span class="sxs-lookup"><span data-stu-id="25bbd-284">**To grant/revoke HTTP web services access**</span></span>

1. <span data-ttu-id="25bbd-285">Přihlaste se k [portál][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="25bbd-285">Sign in to the [Portal][azure-portal].</span></span>
2. <span data-ttu-id="25bbd-286">Klikněte na tlačítko **Procházet vše** v levé nabídce klikněte na tlačítko **clustery HDInsight**, klikněte na název clusteru.</span><span class="sxs-lookup"><span data-stu-id="25bbd-286">Click **Browse All** from the left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="25bbd-287">Klikněte na tlačítko **nastavení** z horní nabídce a pak klikněte na tlačítko **clusteru přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-287">Click **Settings** from the top menu, and then click **Cluster Login**.</span></span>
4. <span data-ttu-id="25bbd-288">Pokud **clusteru přihlášení** byla povolena, musíte kliknout na **zakázat**a potom klikněte na **povolit** před změnou uživatelského jména a hesla...</span><span class="sxs-lookup"><span data-stu-id="25bbd-288">If **Cluster login** has been enabled, you must click **Disable**, and then click **Enable** before you can change the username and password..</span></span>
5. <span data-ttu-id="25bbd-289">Pro **uživatelské jméno přihlášení clusteru** a **clusteru přihlašovacího hesla**, zadejte nové uživatelské jméno a heslo (v uvedeném pořadí) pro cluster.</span><span class="sxs-lookup"><span data-stu-id="25bbd-289">For **Cluster Login Username** and **Cluster Login Password**, enter the new user name and password (respectively) for the cluster.</span></span>
6. <span data-ttu-id="25bbd-290">Klikněte na **ULOŽIT**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-290">Click **SAVE**.</span></span>

    ![HDInsight celkové odebrat přístup protokolu http webové služby](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="find-the-default-storage-account"></a><span data-ttu-id="25bbd-292">Najít výchozí účet úložiště</span><span class="sxs-lookup"><span data-stu-id="25bbd-292">Find the default storage account</span></span>
<span data-ttu-id="25bbd-293">Každý cluster HDInsight má výchozí účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="25bbd-293">Each HDInsight cluster has a default storage account.</span></span> <span data-ttu-id="25bbd-294">Výchozí účet úložiště a jeho klíče pro cluster se zobrazí v části **nastavení**/**vlastnosti**/**klíčů k úložišti Azure**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-294">The default storage account and its keys for a cluster appears under **Settings**/**Properties**/**Azure Storage Keys**.</span></span> <span data-ttu-id="25bbd-295">V tématu [seznamu a zobrazit clustery](#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="25bbd-295">See [List and show clusters](#list-and-show-clusters).</span></span>

## <a name="find-the-resource-group"></a><span data-ttu-id="25bbd-296">Najít skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="25bbd-296">Find the resource group</span></span>
<span data-ttu-id="25bbd-297">V režimu Azure Resource Manager se vytvoří každý cluster HDInsight s skupinu prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="25bbd-297">In the Azure Resource Manager mode, each HDInsight cluster is created with an Azure resource group.</span></span> <span data-ttu-id="25bbd-298">Skupina prostředků Azure, který je součástí clusteru s podporou se zobrazí v:</span><span class="sxs-lookup"><span data-stu-id="25bbd-298">The Azure resource group that a cluster belongs to appears in:</span></span>

* <span data-ttu-id="25bbd-299">Seznam clusteru má **skupiny prostředků** sloupce.</span><span class="sxs-lookup"><span data-stu-id="25bbd-299">The cluster list has a **Resource Group** column.</span></span>
* <span data-ttu-id="25bbd-300">Cluster **základní** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="25bbd-300">Cluster **Essential** tile.</span></span>  

<span data-ttu-id="25bbd-301">V tématu [seznamu a zobrazit clustery](#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="25bbd-301">See [List and show clusters](#list-and-show-clusters).</span></span>

## <a name="open-hdinsight-query-console"></a><span data-ttu-id="25bbd-302">Otevřete konzolu HDInsight dotazu</span><span class="sxs-lookup"><span data-stu-id="25bbd-302">Open HDInsight Query console</span></span>
<span data-ttu-id="25bbd-303">Konzole dotazu HDInsight zahrnuje následující funkce:</span><span class="sxs-lookup"><span data-stu-id="25bbd-303">The HDInsight Query console includes the following features:</span></span>

* <span data-ttu-id="25bbd-304">**Hive Editor**: grafickým uživatelským rozhraním A webové rozhraní pro odesílání úloh Hive.</span><span class="sxs-lookup"><span data-stu-id="25bbd-304">**Hive Editor**: A GUI web interface for submitting Hive jobs.</span></span>  <span data-ttu-id="25bbd-305">V tématu [spouštění dotazů Hive pomocí konzole dotazu](hdinsight-hadoop-use-hive-query-console.md).</span><span class="sxs-lookup"><span data-stu-id="25bbd-305">See [Run Hive queries using the Query Console](hdinsight-hadoop-use-hive-query-console.md).</span></span>

    ![Editor portálu hive HDInsight](./media/hdinsight-administer-use-management-portal/hdinsight-hive-editor.png)
* <span data-ttu-id="25bbd-307">**Historie úlohy**: úloh Hadoop monitorování.</span><span class="sxs-lookup"><span data-stu-id="25bbd-307">**Job history**: Monitor Hadoop jobs.</span></span>  

    ![Historie úlohy portálu HDInsight](./media/hdinsight-administer-use-management-portal/hdinsight-job-history.png)

    <span data-ttu-id="25bbd-309">Klikněte na tlačítko **název dotazu** a zobrazit podrobnosti, včetně vlastnosti úlohy **dotaz úlohy**, a ** výstup úlohy.</span><span class="sxs-lookup"><span data-stu-id="25bbd-309">Click **Query Name** to show the details including Job properties, **Job Query**, and **Job Output.</span></span> <span data-ttu-id="25bbd-310">Můžete také stáhnout dotaz a výstup do pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="25bbd-310">You can also download both the query and the output to your workstation.</span></span>
* <span data-ttu-id="25bbd-311">**Soubor prohlížeče**: Procházet výchozí účet úložiště a propojené účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="25bbd-311">**File Browser**: Browse the default storage account and the linked storage accounts.</span></span>

    ![Procházet prohlížeče portálu souboru HDInsight](./media/hdinsight-administer-use-management-portal/hdinsight-file-browser.png)

    <span data-ttu-id="25bbd-313">Na tomto snímku  **<Account>**  typ označuje položku je účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="25bbd-313">On the screenshot, the **<Account>** type indicates the item is an Azure storage account.</span></span>  <span data-ttu-id="25bbd-314">Klikněte na název účtu a vyhledejte soubory.</span><span class="sxs-lookup"><span data-stu-id="25bbd-314">Click the account name to browse the files.</span></span>
* <span data-ttu-id="25bbd-315">**Uživatelské rozhraní Hadoop**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-315">**Hadoop UI**.</span></span>

    ![Portál HDInsight Hadoop uživatelského rozhraní](./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-ui.png)

    <span data-ttu-id="25bbd-317">Z **uživatelského rozhraní Hadoop*, můžete procházet soubory a zkontrolujte protokoly.</span><span class="sxs-lookup"><span data-stu-id="25bbd-317">From **Hadoop UI*, you can browse files, and check logs.</span></span>
* <span data-ttu-id="25bbd-318">**Yarn uživatelského rozhraní**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-318">**Yarn UI**.</span></span>

    ![Portál HDInsight YARN uživatelského rozhraní](./media/hdinsight-administer-use-management-portal/hdinsight-yarn-ui.png)

## <a name="run-hive-queries"></a><span data-ttu-id="25bbd-320">Spuštění dotazů Hive</span><span class="sxs-lookup"><span data-stu-id="25bbd-320">Run Hive queries</span></span>
<span data-ttu-id="25bbd-321">Chcete-li spustili úlohy Hive z portálu, klikněte na tlačítko **Hive Editor** v konzole nástroje HDInsight dotazu.</span><span class="sxs-lookup"><span data-stu-id="25bbd-321">To ran Hive jobs from the Portal, click **Hive Editor** in the HDInsight Query console.</span></span> <span data-ttu-id="25bbd-322">V tématu [otevřeného dotazu HDInsight konzoly](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="25bbd-322">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

## <a name="monitor-jobs"></a><span data-ttu-id="25bbd-323">Monitorování úloh</span><span class="sxs-lookup"><span data-stu-id="25bbd-323">Monitor jobs</span></span>
<span data-ttu-id="25bbd-324">Monitorování úloh z portálu, klikněte na tlačítko **historie úlohy** v konzole nástroje HDInsight dotazu.</span><span class="sxs-lookup"><span data-stu-id="25bbd-324">To monitor jobs from the Portal, click **Job History** in the HDInsight Query console.</span></span> <span data-ttu-id="25bbd-325">V tématu [otevřeného dotazu HDInsight konzoly](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="25bbd-325">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

## <a name="browse-files"></a><span data-ttu-id="25bbd-326">Procházet soubory</span><span class="sxs-lookup"><span data-stu-id="25bbd-326">Browse files</span></span>
<span data-ttu-id="25bbd-327">Chcete-li procházet soubory uložené na výchozí účet úložiště a účty propojené úložiště, klikněte na tlačítko **prohlížeč souborů** v konzole nástroje HDInsight dotazu.</span><span class="sxs-lookup"><span data-stu-id="25bbd-327">To browse files stored in the default storage account and the linked storage accounts, click **File Browser** in the HDInsight Query console.</span></span> <span data-ttu-id="25bbd-328">V tématu [otevřeného dotazu HDInsight konzoly](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="25bbd-328">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

<span data-ttu-id="25bbd-329">Můžete také **procházet systému souborů** nástroj z **uživatelského rozhraní Hadoop** v konzole nástroje HDInsight.</span><span class="sxs-lookup"><span data-stu-id="25bbd-329">You can also use the **Browse the file system** utility from the **Hadoop UI** in the HDInsight console.</span></span>  <span data-ttu-id="25bbd-330">V tématu [otevřeného dotazu HDInsight konzoly](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="25bbd-330">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

## <a name="monitor-cluster-usage"></a><span data-ttu-id="25bbd-331">Monitorování využití clusteru</span><span class="sxs-lookup"><span data-stu-id="25bbd-331">Monitor cluster usage</span></span>
<span data-ttu-id="25bbd-332">**Využití** části okna clusteru HDInsight se zobrazují informace o počet jader, které jsou k dispozici pro vaše předplatné pro použití s HDInsight, jakož i počet jader přidělené k tomuto clusteru a jak jsou přiděleny pro uzly v tomto clusteru.</span><span class="sxs-lookup"><span data-stu-id="25bbd-332">The **Usage** section of the HDInsight cluster blade displays information about the number of cores available to your subscription for use with HDInsight, as well as the number of cores allocated to this cluster and how they are allocated for the nodes within this cluster.</span></span> <span data-ttu-id="25bbd-333">V tématu [seznamu a zobrazit clustery](#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="25bbd-333">See [List and show clusters](#list-and-show-clusters).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="25bbd-334">Pokud chcete monitorovat služby poskytované clusteru HDInsight, musíte použít Ambari Web nebo Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="25bbd-334">To monitor the services provided by the HDInsight cluster, you must use Ambari Web or the Ambari REST API.</span></span> <span data-ttu-id="25bbd-335">Další informace o používání Ambari najdete v tématu [Správa clusterů HDInsight pomocí Ambari](hdinsight-hadoop-manage-ambari.md)</span><span class="sxs-lookup"><span data-stu-id="25bbd-335">For more information on using Ambari, see [Manage HDInsight clusters using Ambari](hdinsight-hadoop-manage-ambari.md)</span></span>
>
>

## <a name="open-hadoop-ui"></a><span data-ttu-id="25bbd-336">Otevřete uživatelské rozhraní pro Hadoop</span><span class="sxs-lookup"><span data-stu-id="25bbd-336">Open Hadoop UI</span></span>
<span data-ttu-id="25bbd-337">Monitorovat clusteru, procházet systému souborů a zkontrolujte protokoly, klikněte na tlačítko **uživatelského rozhraní Hadoop** v konzole nástroje HDInsight dotazu.</span><span class="sxs-lookup"><span data-stu-id="25bbd-337">To monitor the cluster, browse the file system, and check logs, click **Hadoop UI** in the HDInsight Query console.</span></span> <span data-ttu-id="25bbd-338">V tématu [otevřeného dotazu HDInsight konzoly](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="25bbd-338">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

## <a name="open-yarn-ui"></a><span data-ttu-id="25bbd-339">Otevřete Yarn uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="25bbd-339">Open Yarn UI</span></span>
<span data-ttu-id="25bbd-340">Chcete-li použít uživatelské rozhraní Yarn, klikněte na tlačítko **uživatelském rozhraní Yarn** v konzole nástroje HDInsight dotazu.</span><span class="sxs-lookup"><span data-stu-id="25bbd-340">To use Yarn user interface, click **Yarn UI** in the HDInsight Query console.</span></span> <span data-ttu-id="25bbd-341">V tématu [otevřeného dotazu HDInsight konzoly](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="25bbd-341">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

## <a name="connect-to-clusters-using-rdp"></a><span data-ttu-id="25bbd-342">Připojení ke clusterům pomocí protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="25bbd-342">Connect to clusters using RDP</span></span>
<span data-ttu-id="25bbd-343">Přihlašovací údaje pro cluster, který jste zadali při jeho vytváření poskytnout přístup ke službám v clusteru, ale nechcete samotného clusteru pomocí vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="25bbd-343">The credentials for the cluster that you provided at its creation give access to the services on the cluster, but not to the cluster itself through Remote Desktop.</span></span> <span data-ttu-id="25bbd-344">Přístup ke vzdálené ploše můžete zapnout při zřizování clusteru nebo po zřízení clusteru.</span><span class="sxs-lookup"><span data-stu-id="25bbd-344">You can turn on Remote Desktop access when you provision a cluster or after a cluster is provisioned.</span></span> <span data-ttu-id="25bbd-345">Pokyny k povolení funkce Vzdálená plocha při vytváření naleznete v tématu [clusteru HDInsight se vytvořit](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="25bbd-345">For the instructions about enabling Remote Desktop at creation, see [Create HDInsight cluster](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

<span data-ttu-id="25bbd-346">**Chcete-li povolit vzdálená plocha**</span><span class="sxs-lookup"><span data-stu-id="25bbd-346">**To enable Remote Desktop**</span></span>

1. <span data-ttu-id="25bbd-347">Přihlaste se k [portál][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="25bbd-347">Sign in to the [Portal][azure-portal].</span></span>
2. <span data-ttu-id="25bbd-348">Klikněte na tlačítko **Procházet vše** v levé nabídce klikněte na tlačítko **clustery HDInsight**, klikněte na název clusteru.</span><span class="sxs-lookup"><span data-stu-id="25bbd-348">Click **Browse All** from the left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="25bbd-349">Klikněte na tlačítko **nastavení** z horní nabídce a pak klikněte na tlačítko **vzdálené plochy**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-349">Click **Settings** from the top menu, and then click **Remote Desktop**.</span></span>
4. <span data-ttu-id="25bbd-350">Zadejte **vyprší dne**, **uživatelské jméno vzdálené plochy** a **heslo vzdálené plochy**a potom klikněte na **povolit**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-350">Enter **Expires On**, **Remote Desktop Username** and **Remote Desktop Password**, and then click **Enable**.</span></span>

    ![HDInsight povolit zakázat konfigurace vzdálené plochy](./media/hdinsight-administer-use-management-portal/hdinsight.portal.remote.desktop.png)

    <span data-ttu-id="25bbd-352">Výchozí hodnoty pro vyprší dne je za týden.</span><span class="sxs-lookup"><span data-stu-id="25bbd-352">The default values for Expires On is a week.</span></span>

   > [!NOTE]
   > <span data-ttu-id="25bbd-353">.NET SDK služby HDInsight můžete použít také k povolení vzdálené plochy na clusteru.</span><span class="sxs-lookup"><span data-stu-id="25bbd-353">You can also use the HDInsight .NET SDK to enable Remote Desktop on a cluster.</span></span> <span data-ttu-id="25bbd-354">Použití **EnableRdp** metodu klientského objektu HDInsight následujícím způsobem: **klienta. EnableRdp (název clusteru, místo, "rdpuser", "rdppassword", DateTime.Now.AddDays(6))**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-354">Use the **EnableRdp** method on the HDInsight client object in the following manner: **client.EnableRdp(clustername, location, "rdpuser", "rdppassword", DateTime.Now.AddDays(6))**.</span></span> <span data-ttu-id="25bbd-355">Podobně zakázání vzdálené plochy na clusteru, můžete použít **klienta. DisableRdp (název clusteru, umístění)**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-355">Similarly, to disable Remote Desktop on the cluster, you can use **client.DisableRdp(clustername, location)**.</span></span> <span data-ttu-id="25bbd-356">Další informace o těchto metodách v tématu [HDInsight .NET SDK – referenční informace](http://go.microsoft.com/fwlink/?LinkId=529017).</span><span class="sxs-lookup"><span data-stu-id="25bbd-356">For more information on these methods, see [HDInsight .NET SDK Reference](http://go.microsoft.com/fwlink/?LinkId=529017).</span></span> <span data-ttu-id="25bbd-357">Tuto možnost lze použít pouze pro clustery služby HDInsight v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="25bbd-357">This is applicable only for HDInsight clusters running on Windows.</span></span>
   >
   >

<span data-ttu-id="25bbd-358">**K připojení ke clusteru pomocí protokolu RDP**</span><span class="sxs-lookup"><span data-stu-id="25bbd-358">**To connect to a cluster by using RDP**</span></span>

1. <span data-ttu-id="25bbd-359">Přihlaste se k [portál][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="25bbd-359">Sign in to the [Portal][azure-portal].</span></span>
2. <span data-ttu-id="25bbd-360">Klikněte na tlačítko **Procházet vše** v levé nabídce klikněte na tlačítko **clustery HDInsight**, klikněte na název clusteru.</span><span class="sxs-lookup"><span data-stu-id="25bbd-360">Click **Browse All** from the left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="25bbd-361">Klikněte na tlačítko **nastavení** z horní nabídce a pak klikněte na tlačítko **vzdálené plochy**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-361">Click **Settings** from the top menu, and then click **Remote Desktop**.</span></span>
4. <span data-ttu-id="25bbd-362">Klikněte na tlačítko **Connect** a postupujte podle pokynů.</span><span class="sxs-lookup"><span data-stu-id="25bbd-362">Click **Connect** and follow the instructions.</span></span> <span data-ttu-id="25bbd-363">Pokud připojení je zakázat, musíte ji nejprve povolit.</span><span class="sxs-lookup"><span data-stu-id="25bbd-363">If Connect is disable, you must enable it first.</span></span> <span data-ttu-id="25bbd-364">Zajistěte, aby pomocí vzdálené plochy uživatele uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="25bbd-364">Make sure using the Remote Desktop user username and password.</span></span>  <span data-ttu-id="25bbd-365">Nelze použít přihlašovací údaje uživatele clusteru.</span><span class="sxs-lookup"><span data-stu-id="25bbd-365">You can't use the Cluster user credentials.</span></span>

## <a name="open-hadoop-command-line"></a><span data-ttu-id="25bbd-366">Otevřete příkazový řádek Hadoop</span><span class="sxs-lookup"><span data-stu-id="25bbd-366">Open Hadoop command line</span></span>
<span data-ttu-id="25bbd-367">Připojte se ke clusteru pomocí vzdálené plochy a použijte příkazový řádek Hadoop, musíte nejprve povolíte přístup ke vzdálené ploše do clusteru jak je popsáno v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="25bbd-367">To connect to the cluster by using Remote Desktop and use the Hadoop command line, you must first have enabled Remote Desktop access to the cluster as described in the previous section.</span></span>

<span data-ttu-id="25bbd-368">**Chcete-li otevřít příkazový řádek Hadoop**</span><span class="sxs-lookup"><span data-stu-id="25bbd-368">**To open a Hadoop command line**</span></span>

1. <span data-ttu-id="25bbd-369">Připojte se ke clusteru pomocí vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="25bbd-369">Connect to the cluster using Remote Desktop.</span></span>
2. <span data-ttu-id="25bbd-370">Z plochy, klikněte dvakrát na **Hadoop příkazového řádku**.</span><span class="sxs-lookup"><span data-stu-id="25bbd-370">From the desktop, double-click **Hadoop Command Line**.</span></span>

    <span data-ttu-id="25bbd-371">![HDI. HadoopCommandLine][image-hadoopcommandline]</span><span class="sxs-lookup"><span data-stu-id="25bbd-371">![HDI.HadoopCommandLine][image-hadoopcommandline]</span></span>

    <span data-ttu-id="25bbd-372">Další informace o Hadoop příkazů najdete v tématu [Hadoop příkazy odkaz](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CommandsManual.html).</span><span class="sxs-lookup"><span data-stu-id="25bbd-372">For more information on Hadoop commands, see [Hadoop commands reference](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CommandsManual.html).</span></span>

<span data-ttu-id="25bbd-373">Název složky v předchozím snímku obrazovky má číslo verze Hadoop vložených.</span><span class="sxs-lookup"><span data-stu-id="25bbd-373">In the previous screenshot, the folder name has the Hadoop version number embedded.</span></span> <span data-ttu-id="25bbd-374">Číslo verze lze změnit v závislosti na verzi Hadoop komponenty nainstalované v clusteru.</span><span class="sxs-lookup"><span data-stu-id="25bbd-374">The version number can changed based on the version of the Hadoop components installed on the cluster.</span></span> <span data-ttu-id="25bbd-375">Proměnné prostředí Hadoop můžete použít k odkazování na těchto složek.</span><span class="sxs-lookup"><span data-stu-id="25bbd-375">You can use Hadoop environment variables to refer to those folders.</span></span> <span data-ttu-id="25bbd-376">Například:</span><span class="sxs-lookup"><span data-stu-id="25bbd-376">For example:</span></span>

    cd %hadoop_home%
    cd %hive_home%
    cd %hbase_home%
    cd %pig_home%
    cd %sqoop_home%
    cd %hcatalog_home%

## <a name="next-steps"></a><span data-ttu-id="25bbd-377">Další kroky</span><span class="sxs-lookup"><span data-stu-id="25bbd-377">Next steps</span></span>
<span data-ttu-id="25bbd-378">V tomto článku jste se naučili postup vytvoření clusteru HDInsight pomocí portálu a otevřete nástroj příkazového řádku Hadoop.</span><span class="sxs-lookup"><span data-stu-id="25bbd-378">In this article, you have learned how to create an HDInsight cluster by using the Portal, and how to open the Hadoop command-line tool.</span></span> <span data-ttu-id="25bbd-379">Další informace naleznete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="25bbd-379">To learn more, see the following articles:</span></span>

* [<span data-ttu-id="25bbd-380">Spravovat HDInsight pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="25bbd-380">Administer HDInsight Using Azure PowerShell</span></span>](hdinsight-administer-use-powershell.md)
* [<span data-ttu-id="25bbd-381">Spravovat HDInsight pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="25bbd-381">Administer HDInsight Using Azure CLI</span></span>](hdinsight-administer-use-command-line.md)
* [<span data-ttu-id="25bbd-382">Vytvoření clusterů HDInsight</span><span class="sxs-lookup"><span data-stu-id="25bbd-382">Create HDInsight clusters</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
* [<span data-ttu-id="25bbd-383">Odesílání úloh Hadoop prostřednictvím kódu programu</span><span class="sxs-lookup"><span data-stu-id="25bbd-383">Submit Hadoop jobs programmatically</span></span>](hdinsight-submit-hadoop-jobs-programmatically.md)
* [<span data-ttu-id="25bbd-384">Začínáme s Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="25bbd-384">Get Started with Azure HDInsight</span></span>](hdinsight-hadoop-linux-tutorial-get-started.md)
* [<span data-ttu-id="25bbd-385">Jaká verze Hadoop je v Azure HDInsight?</span><span class="sxs-lookup"><span data-stu-id="25bbd-385">What version of Hadoop is in Azure HDInsight?</span></span>](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-command-line.png "Hadoop příkazového řádku"
