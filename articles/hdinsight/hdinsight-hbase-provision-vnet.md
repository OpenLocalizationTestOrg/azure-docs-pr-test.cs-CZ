---
title: "aaaCreate HBase clusterů ve virtuální síti - Azure | Microsoft Docs"
description: "Začínáme používat HBase v Azure HDInsight. Zjistěte, jak toocreate HDInsight HBase clusterů ve virtuální síti Azure."
keywords: 
services: hdinsight,virtual-network
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 8de8e446-f818-4e61-8fad-e9d38421e80d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/17/2017
ms.author: jgao
ms.openlocfilehash: 097338a5a650bb607a9f6f9ddb59bb88d098b56f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-hbase-clusters-on-hdinsight-in-azure-virtual-network"></a><span data-ttu-id="7156f-104">Vytvořit clustery HBase v HDInsight v Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="7156f-104">Create HBase clusters on HDInsight in Azure Virtual Network</span></span>
<span data-ttu-id="7156f-105">Zjistěte, jak toocreate Azure HDInsight HBase clusterů v [Azure Virtual Network][1].</span><span class="sxs-lookup"><span data-stu-id="7156f-105">Learn how toocreate Azure HDInsight HBase clusters in an [Azure Virtual Network][1].</span></span>

<span data-ttu-id="7156f-106">Clustery HBase integrace virtuální sítě, může být nasazený toohello stejné virtuální sítě jako aplikace tak, že aplikace může komunikovat přímo s HBase.</span><span class="sxs-lookup"><span data-stu-id="7156f-106">With virtual network integration, HBase clusters can be deployed toohello same virtual network as your applications so that applications can communicate with HBase directly.</span></span> <span data-ttu-id="7156f-107">Hello výhody patří:</span><span class="sxs-lookup"><span data-stu-id="7156f-107">hello benefits include:</span></span>

* <span data-ttu-id="7156f-108">Přímé připojení hello webové aplikace toohello uzlů tohoto clusteru hello HBase, který umožňuje komunikaci prostřednictvím vzdálené procedury HBase Java volání rozhraní API (RPC).</span><span class="sxs-lookup"><span data-stu-id="7156f-108">Direct connectivity of hello web application toohello nodes of hello HBase cluster, which enables communication via HBase Java remote procedure call (RPC) APIs.</span></span>
* <span data-ttu-id="7156f-109">Zvýšení výkonu tak, že není provozu přejděte přes více bran a nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="7156f-109">Improved performance by not having your traffic go over multiple gateways and load-balancers.</span></span>
* <span data-ttu-id="7156f-110">Hello možnost tooprocess citlivé informace bezpečnější způsobem bez vystavení veřejný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="7156f-110">hello ability tooprocess sensitive information in a more secure manner without exposing a public endpoint.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="7156f-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7156f-111">Prerequisites</span></span>
<span data-ttu-id="7156f-112">Než začnete tento kurz, musíte mít hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="7156f-112">Before you begin this tutorial, you must have hello following items:</span></span>

* <span data-ttu-id="7156f-113">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="7156f-113">**An Azure subscription**.</span></span> <span data-ttu-id="7156f-114">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="7156f-114">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="7156f-115">**Pracovní stanice s prostředím Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="7156f-115">**A workstation with Azure PowerShell**.</span></span> <span data-ttu-id="7156f-116">V tématu [instalace a použití prostředí Azure PowerShell](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/).</span><span class="sxs-lookup"><span data-stu-id="7156f-116">See [Install and use Azure PowerShell](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/).</span></span>

## <a name="create-hbase-cluster-into-virtual-network"></a><span data-ttu-id="7156f-117">Vytvoření clusteru HBase do virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="7156f-117">Create HBase cluster into virtual network</span></span>
<span data-ttu-id="7156f-118">V této části vytvoříte clusteru s podporou systémem Linux HBase s hello závislý účet Azure Storage virtuální síti Azure pomocí [šablony Azure Resource Manageru](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="7156f-118">In this section, you create a Linux-based HBase cluster with hello dependent Azure Storage account in an Azure virtual network using an [Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span> <span data-ttu-id="7156f-119">Další metody vytváření clusterů a principy hello nastavení, najdete v tématu [Tvorba clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="7156f-119">For other cluster creation methods and understanding hello settings, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span> <span data-ttu-id="7156f-120">Další informace o použití šablony toocreate Hadoop clusterů v HDInsight, najdete v části [vytvoření Hadoop clusterů v HDInsight pomocí šablony Azure Resource Manager](hdinsight-hadoop-create-windows-clusters-arm-templates.md)</span><span class="sxs-lookup"><span data-stu-id="7156f-120">For more information about using a template toocreate Hadoop clusters in HDInsight, see [Create Hadoop clusters in HDInsight using Azure Resource Manager templates](hdinsight-hadoop-create-windows-clusters-arm-templates.md)</span></span>

> [!NOTE]
> <span data-ttu-id="7156f-121">Některé vlastnosti jsou pevně zakódovaná do šablony hello.</span><span class="sxs-lookup"><span data-stu-id="7156f-121">Some properties are hard-coded into hello template.</span></span> <span data-ttu-id="7156f-122">Například:</span><span class="sxs-lookup"><span data-stu-id="7156f-122">For example:</span></span>
>
> * <span data-ttu-id="7156f-123">**Umístění**: východní USA 2</span><span class="sxs-lookup"><span data-stu-id="7156f-123">**Location**: East US 2</span></span>
> * <span data-ttu-id="7156f-124">**Verze clusteru:** 3.5</span><span class="sxs-lookup"><span data-stu-id="7156f-124">**Cluster version**: 3.5</span></span>
> * <span data-ttu-id="7156f-125">**Počet uzlů pracovního procesu clusteru**: 2</span><span class="sxs-lookup"><span data-stu-id="7156f-125">**Cluster worker node count**: 2</span></span>
> * <span data-ttu-id="7156f-126">**Výchozí účet úložiště**: jedinečné řetězce</span><span class="sxs-lookup"><span data-stu-id="7156f-126">**Default storage account**: a unique string</span></span>
> * <span data-ttu-id="7156f-127">**Název virtuální sítě**: &lt;název clusteru >-virtuální síť</span><span class="sxs-lookup"><span data-stu-id="7156f-127">**Virtual network name**: &lt;Cluster Name>-vnet</span></span>
> * <span data-ttu-id="7156f-128">**Adresní prostor virtuální sítě**: 10.0.0.0/16</span><span class="sxs-lookup"><span data-stu-id="7156f-128">**Virtual network address space**: 10.0.0.0/16</span></span>
> * <span data-ttu-id="7156f-129">**Název podsítě**: subnet1</span><span class="sxs-lookup"><span data-stu-id="7156f-129">**Subnet name**: subnet1</span></span>
> * <span data-ttu-id="7156f-130">**Rozsah adres podsítě**: 10.0.0.0/24</span><span class="sxs-lookup"><span data-stu-id="7156f-130">**Subnet address range**: 10.0.0.0/24</span></span>
>
> <span data-ttu-id="7156f-131">&lt;Název clusteru > se nahradí název clusteru hello poskytují při použití šablony hello.</span><span class="sxs-lookup"><span data-stu-id="7156f-131">&lt;Cluster Name> is replaced with hello cluster name you provide when using hello template.</span></span>
>
>

1. <span data-ttu-id="7156f-132">Klikněte na tlačítko hello následující šablony hello tooopen bitové kopie v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7156f-132">Click hello following image tooopen hello template in hello Azure portal.</span></span> <span data-ttu-id="7156f-133">Šablona Hello se nachází v [šablon Azure rychlý Start](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-linux-vnet/).</span><span class="sxs-lookup"><span data-stu-id="7156f-133">hello template is located in [Azure QuickStart Templates](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-linux-vnet/).</span></span>

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-provision-vnet/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. <span data-ttu-id="7156f-134">Z hello **vlastní nasazení** okno, zadejte hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="7156f-134">From hello **Custom deployment** blade, enter hello following properties:</span></span>

   * <span data-ttu-id="7156f-135">**Předplatné**: Vyberte předplatné Azure použité clusteru HDInsight hello toocreate, hello závislý účet úložiště a hello virtuální síť Azure.</span><span class="sxs-lookup"><span data-stu-id="7156f-135">**Subscription**: Select an Azure subscription used toocreate hello HDInsight cluster, hello dependent Storage account and hello Azure virtual network.</span></span>
   * <span data-ttu-id="7156f-136">**Skupina prostředků**: vyberte **vytvořit nový**a zadejte nový název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="7156f-136">**Resource group**: Select **Create new**, and specify a new resource group name.</span></span>
   * <span data-ttu-id="7156f-137">**Umístění**: Vyberte umístění pro skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="7156f-137">**Location**: Select a location for hello resource group.</span></span>
   * <span data-ttu-id="7156f-138">**Název clusteru**: Zadejte název toobe clusteru Hadoop hello vytvořili.</span><span class="sxs-lookup"><span data-stu-id="7156f-138">**ClusterName**: Enter a name for hello Hadoop cluster toobe created.</span></span>
   * <span data-ttu-id="7156f-139">**Přihlašovací jméno a heslo clusteru**: hello výchozí přihlašovací jméno je **správce**.</span><span class="sxs-lookup"><span data-stu-id="7156f-139">**Cluster login name and password**: hello default login name is **admin**.</span></span>
   * <span data-ttu-id="7156f-140">**SSH uživatelské jméno a heslo**: výchozí uživatelské jméno hello **sshuser**.</span><span class="sxs-lookup"><span data-stu-id="7156f-140">**SSH username and password**: hello default username is **sshuser**.</span></span>  <span data-ttu-id="7156f-141">Můžete ho změnit.</span><span class="sxs-lookup"><span data-stu-id="7156f-141">You can rename it.</span></span>
   * <span data-ttu-id="7156f-142">**Souhlasím toohello podmínky a ujednání hello výše uvedených**: (vyberte)</span><span class="sxs-lookup"><span data-stu-id="7156f-142">**I agree toohello terms and hello conditions stated above**: (Select)</span></span>
3. <span data-ttu-id="7156f-143">Klikněte na **Koupit**.</span><span class="sxs-lookup"><span data-stu-id="7156f-143">Click **Purchase**.</span></span> <span data-ttu-id="7156f-144">Trvá přibližně 20 minut toocreate cluster.</span><span class="sxs-lookup"><span data-stu-id="7156f-144">It takes about around 20 minutes toocreate a cluster.</span></span> <span data-ttu-id="7156f-145">Po vytvoření clusteru hello můžete kliknout na okno hello clusteru v portálu tooopen hello ho.</span><span class="sxs-lookup"><span data-stu-id="7156f-145">Once hello cluster is created, you can click hello cluster blade in hello portal tooopen it.</span></span>

<span data-ttu-id="7156f-146">Po dokončení kurzu hello můžete chtít toodelete hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="7156f-146">After you complete hello tutorial, you might want toodelete hello cluster.</span></span> <span data-ttu-id="7156f-147">Pomocí HDInsight jsou vaše data uložena v Azure Storage, takže můžete clusteru bezpečně odstranit, pokud není používán.</span><span class="sxs-lookup"><span data-stu-id="7156f-147">With HDInsight, your data is stored in Azure Storage, so you can safely delete a cluster when it is not in use.</span></span> <span data-ttu-id="7156f-148">Za cluster služby HDInsight se účtují poplatky, i když se nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="7156f-148">You are also charged for an HDInsight cluster, even when it is not in use.</span></span> <span data-ttu-id="7156f-149">Vzhledem k tomu, že hello poplatky za hello clusteru jsou mnohokrát víc než hello poplatky za úložiště, dává ekonomický smysl clustery toodelete které nejsou používána.</span><span class="sxs-lookup"><span data-stu-id="7156f-149">Since hello charges for hello cluster are many times more than hello charges for storage, it makes economic sense toodelete clusters when they are not in use.</span></span> <span data-ttu-id="7156f-150">Pokyny hello odstranění clusteru najdete v tématu [hello spravovat Hadoop clusterů v HDInsight pomocí portálu Azure](hdinsight-administer-use-management-portal.md#delete-clusters).</span><span class="sxs-lookup"><span data-stu-id="7156f-150">For hello instructions of deleting a cluster, see [Manage Hadoop clusters in HDInsight by using hello Azure portal](hdinsight-administer-use-management-portal.md#delete-clusters).</span></span>

<span data-ttu-id="7156f-151">toobegin práce s nového clusteru HBase, můžete použít postupy hello najít v [Začínáme používat HBase s Hadoop v HDInsight](hdinsight-hbase-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7156f-151">toobegin working with your new HBase cluster, you can use hello procedures found in [Get started using HBase with Hadoop in HDInsight](hdinsight-hbase-tutorial-get-started.md).</span></span>

## <a name="connect-toohello-hbase-cluster-using-hbase-java-rpc-apis"></a><span data-ttu-id="7156f-152">Připojte toohello cluster HBase pomocí rozhraní API RPC Java HBase</span><span class="sxs-lookup"><span data-stu-id="7156f-152">Connect toohello HBase cluster using HBase Java RPC APIs</span></span>
1. <span data-ttu-id="7156f-153">Vytváření infrastruktury jako služby (IaaS) virtuální počítač do hello stejnou virtuální síť Azure a hello stejné podsíti.</span><span class="sxs-lookup"><span data-stu-id="7156f-153">Create an infrastructure as a service (IaaS) virtual machine into hello same Azure virtual network and hello same subnet.</span></span> <span data-ttu-id="7156f-154">Pokyny pro vytvoření nového virtuálního počítače IaaS, najdete v části [vytvořit virtuální počítač spuštěný Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="7156f-154">For instructions on creating a new IaaS virtual machine, see [Create a Virtual Machine Running Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md).</span></span> <span data-ttu-id="7156f-155">Pokud následující hello kroky v tomto dokumentu, je nutné použít následující hodnoty pro konfiguraci sítě hello hello:</span><span class="sxs-lookup"><span data-stu-id="7156f-155">When following hello steps in this document, you must use hello following values for hello Network configuration:</span></span>

   * <span data-ttu-id="7156f-156">**Virtuální síť**: &lt;název clusteru >-virtuální síť</span><span class="sxs-lookup"><span data-stu-id="7156f-156">**Virtual network**: &lt;Cluster name>-vnet</span></span>
   * <span data-ttu-id="7156f-157">**Podsíť**: subnet1</span><span class="sxs-lookup"><span data-stu-id="7156f-157">**Subnet**: subnet1</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="7156f-158">Nahraďte &lt;název clusteru > s názvem hello jste použili při vytváření clusteru HDInsight hello v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="7156f-158">Replace &lt;Cluster name> with hello name you used when creating hello HDInsight cluster in previous steps.</span></span>
   >
   >

   <span data-ttu-id="7156f-159">Pomocí těchto hodnot, hello je umístěn virtuální počítač v hello stejné virtuální síť a podsíť jako hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="7156f-159">Using these values, hello virtual machine is placed in hello same virtual network and subnet as hello HDInsight cluster.</span></span> <span data-ttu-id="7156f-160">Tato konfigurace umožňuje jejich toodirectly vzájemně komunikovat.</span><span class="sxs-lookup"><span data-stu-id="7156f-160">This configuration allows them toodirectly communicate with each other.</span></span> <span data-ttu-id="7156f-161">Neexistuje způsob, jak toocreate clusteru HDInsight bez prázdný hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="7156f-161">There is a way toocreate an HDInsight cluster with an empty edge node.</span></span> <span data-ttu-id="7156f-162">Hello hraniční uzel může být použité toomanage hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="7156f-162">hello edge node can be used toomanage hello cluster.</span></span>  <span data-ttu-id="7156f-163">Další informace najdete v tématu [použít prázdný edge uzly v HDInsight](hdinsight-apps-use-edge-node.md).</span><span class="sxs-lookup"><span data-stu-id="7156f-163">For more information, see [Use empty edge nodes in HDInsight](hdinsight-apps-use-edge-node.md).</span></span>

2. <span data-ttu-id="7156f-164">Při vzdáleném použití tooHBase tooconnect aplikace Java, musíte použít hello plně kvalifikovaný název domény (FQDN).</span><span class="sxs-lookup"><span data-stu-id="7156f-164">When using a Java application tooconnect tooHBase remotely, you must use hello fully qualified domain name (FQDN).</span></span> <span data-ttu-id="7156f-165">toodetermine, musíte získat hello příponu DNS specifickou pro připojení clusteru HBase hello.</span><span class="sxs-lookup"><span data-stu-id="7156f-165">toodetermine this, you must get hello connection-specific DNS suffix of hello HBase cluster.</span></span> <span data-ttu-id="7156f-166">toodo, můžete použít jednu z následujících metod hello:</span><span class="sxs-lookup"><span data-stu-id="7156f-166">toodo that, you can use one of hello following methods:</span></span>

   * <span data-ttu-id="7156f-167">Použití webové prohlížeče toomake volání rozhraní Ambari:</span><span class="sxs-lookup"><span data-stu-id="7156f-167">Use a Web browser toomake an Ambari call:</span></span>

     <span data-ttu-id="7156f-168">Procházet toohttps: / /&lt;ClusterName >.azurehdinsight.net/api/v1/clusters/&lt;ClusterName > / hostitelem? minimal_response = true.</span><span class="sxs-lookup"><span data-stu-id="7156f-168">Browse toohttps://&lt;ClusterName>.azurehdinsight.net/api/v1/clusters/&lt;ClusterName>/hosts?minimal_response=true.</span></span> <span data-ttu-id="7156f-169">Se změní soubor JSON s hello přípony DNS.</span><span class="sxs-lookup"><span data-stu-id="7156f-169">It turns a JSON file with hello DNS suffixes.</span></span>
   * <span data-ttu-id="7156f-170">Použití webu Ambari hello:</span><span class="sxs-lookup"><span data-stu-id="7156f-170">Use hello Ambari website:</span></span>

     1. <span data-ttu-id="7156f-171">Procházet příliš https://&lt;ClusterName >. azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="7156f-171">Browse too https://&lt;ClusterName>.azurehdinsight.net.</span></span>
     2. <span data-ttu-id="7156f-172">Klikněte na tlačítko **hostitele** hello hlavní nabídce.</span><span class="sxs-lookup"><span data-stu-id="7156f-172">Click **Hosts** from hello top menu.</span></span>
   * <span data-ttu-id="7156f-173">Použijte volání REST toomake Curl:</span><span class="sxs-lookup"><span data-stu-id="7156f-173">Use Curl toomake REST calls:</span></span>

    ```bash
        curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest
    ```

     <span data-ttu-id="7156f-174">V hello JavaScript Object Notation (JSON) data vrácená vyhledejte položku "název_hostitele" hello.</span><span class="sxs-lookup"><span data-stu-id="7156f-174">In hello JavaScript Object Notation (JSON) data returned, find hello "host_name" entry.</span></span> <span data-ttu-id="7156f-175">Obsahuje hello plně kvalifikovaný název domény pro hello uzly v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="7156f-175">It contains hello FQDN for hello nodes in hello cluster.</span></span> <span data-ttu-id="7156f-176">Například:</span><span class="sxs-lookup"><span data-stu-id="7156f-176">For example:</span></span>

         ...
         "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
         ...

     <span data-ttu-id="7156f-177">část Hello hello domény název začíná název clusteru hello je hello příponu DNS.</span><span class="sxs-lookup"><span data-stu-id="7156f-177">hello portion of hello domain name beginning with hello cluster name is hello DNS suffix.</span></span> <span data-ttu-id="7156f-178">Například mycluster.b1.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="7156f-178">For example, mycluster.b1.cloudapp.net.</span></span>
   * <span data-ttu-id="7156f-179">Použití Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="7156f-179">Use Azure PowerShell</span></span>

     <span data-ttu-id="7156f-180">Použití hello následující hello tooregister skriptu prostředí Azure PowerShell **Get-ClusterDetail** funkce, které lze použít tooreturn příponu DNS hello:</span><span class="sxs-lookup"><span data-stu-id="7156f-180">Use hello following Azure PowerShell script tooregister hello **Get-ClusterDetail** function, which can be used tooreturn hello DNS suffix:</span></span>

    ```powershell
        function Get-ClusterDetail(
            [String]
            [Parameter( Position=0, Mandatory=$true )]
            $ClusterDnsName,
            [String]
            [Parameter( Position=1, Mandatory=$true )]
            $Username,
            [String]
            [Parameter( Position=2, Mandatory=$true )]
            $Password,
            [String]
            [Parameter( Position=3, Mandatory=$true )]
            $PropertyName
            )
        {
        <#
            .SYNOPSIS
            Displays information toofacilitate an HDInsight cluster-to-cluster scenario within hello same virtual network.
            .Description
            This command shows hello following 4 properties of an HDInsight cluster:
            1. ZookeeperQuorum (supports only HBase type cluster)
                Shows hello value of HBase property "hbase.zookeeper.quorum".
            2. ZookeeperClientPort (supports only HBase type cluster)
                Shows hello value of HBase property "hbase.zookeeper.property.clientPort".
            3. HBaseRestServers (supports only HBase type cluster)
                Shows a list of host FQDNs that run hello HBase REST server.
            4. FQDNSuffix (supports all cluster types)
                Shows hello FQDN suffix of hosts in hello cluster.
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
            This command shows hello value of HBase property "hbase.zookeeper.quorum".
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
            This command shows hello value of HBase property "hbase.zookeeper.property.clientPort".
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
            This command shows a list of host FQDNs that run hello HBase REST server.
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
            This command shows hello FQDN suffix of hosts in hello cluster.
        #>

            $DnsSuffix = ".azurehdinsight.net"

            $ClusterFQDN = $ClusterDnsName + $DnsSuffix
            $webclient = new-object System.Net.WebClient
            $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

            if($PropertyName -eq "ZookeeperQuorum")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
            }
            if($PropertyName -eq "ZookeeperClientPort")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.property.clientPort"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                Write-host $JsonObject.items[0].properties.'hbase.zookeeper.property.clientPort'
            }
            if($PropertyName -eq "HBaseRestServers")
            {
                $Url1 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.rest.port"
                $Response1 = $webclient.DownloadString($Url1)
                $JsonObject1 = $Response1 | ConvertFrom-Json
                $PortNumber = $JsonObject1.items[0].properties.'hbase.rest.port'

                $Url2 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/hbase/components/hbrest"
                $Response2 = $webclient.DownloadString($Url2)
                $JsonObject2 = $Response2 | ConvertFrom-Json
                foreach ($host_component in $JsonObject2.host_components)
                {
                    $ConnectionString = $host_component.HostRoles.host_name + ":" + $PortNumber
                    Write-host $ConnectionString
                }
            }
            if($PropertyName -eq "FQDNSuffix")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/YARN/components/RESOURCEMANAGER"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                $FQDN = $JsonObject.host_components[0].HostRoles.host_name
                $pos = $FQDN.IndexOf(".")
                $Suffix = $FQDN.Substring($pos + 1)
                Write-host $Suffix
            }
        }
    ```

     <span data-ttu-id="7156f-181">Po spouštění skriptu prostředí Azure PowerShell text hello, použijte hello následující příkaz příponu DNS hello tooreturn pomocí hello **Get-ClusterDetail** funkce.</span><span class="sxs-lookup"><span data-stu-id="7156f-181">After running hello Azure PowerShell script, use hello following command tooreturn hello DNS suffix by using hello **Get-ClusterDetail** function.</span></span> <span data-ttu-id="7156f-182">Při použití tohoto příkazu, zadejte název clusteru HDInsight HBase, správce jméno a heslo správce.</span><span class="sxs-lookup"><span data-stu-id="7156f-182">Specify your HDInsight HBase cluster name, admin name, and admin password when using this command.</span></span>

    ```powershell
        Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>
    ```

     <span data-ttu-id="7156f-183">Tento příkaz vrátí hello příponu DNS.</span><span class="sxs-lookup"><span data-stu-id="7156f-183">This command returns hello DNS suffix.</span></span> <span data-ttu-id="7156f-184">Například **yourclustername.b4.internal.cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="7156f-184">For example, **yourclustername.b4.internal.cloudapp.net**.</span></span>


<!--
3.    Change hello primary DNS suffix configuration of hello virtual machine. This enables hello virtual machine tooautomatically resolve hello host name of hello HBase cluster without explicit specification of hello suffix. For example, hello *workernode0* host name will be correctly resolved tooworkernode0 of hello HBase cluster.

    toomake hello configuration change:

    1. RDP into hello virtual machine.
    2. Open **Local Group Policy Editor**. hello executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** toohello value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix][img-primary-dns-suffix]
    4. Click **OK**.
    5. Reboot hello virtual machine.
-->

<span data-ttu-id="7156f-185">tooverify, který hello virtuální počítač může komunikovat s hello HBase cluster, použijte příkaz hello `ping headnode0.<dns suffix>` z hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7156f-185">tooverify that hello virtual machine can communicate with hello HBase cluster, use hello command `ping headnode0.<dns suffix>` from hello virtual machine.</span></span> <span data-ttu-id="7156f-186">Například headnode0.mycluster.b1.cloudapp.net příkazu ping.</span><span class="sxs-lookup"><span data-stu-id="7156f-186">For example, ping headnode0.mycluster.b1.cloudapp.net.</span></span>

<span data-ttu-id="7156f-187">toouse tyto informace v aplikaci Java, můžete provést kroky hello v [aplikací Java toobuild použít Maven, které používají HBase s HDInsight (Hadoop)](hdinsight-hbase-build-java-maven.md) toocreate aplikace.</span><span class="sxs-lookup"><span data-stu-id="7156f-187">toouse this information in a Java application, you can follow hello steps in [Use Maven toobuild Java applications that use HBase with HDInsight (Hadoop)](hdinsight-hbase-build-java-maven.md) toocreate an application.</span></span> <span data-ttu-id="7156f-188">aplikace hello toohave připojení vzdáleného serveru HBase tooa, změňte hello **hbase-site.xml** soubor v této hello toouse příklad plně kvalifikovaný název domény pro Zookeeper.</span><span class="sxs-lookup"><span data-stu-id="7156f-188">toohave hello application connect tooa remote HBase server, modify hello **hbase-site.xml** file in this example toouse hello FQDN for Zookeeper.</span></span> <span data-ttu-id="7156f-189">Například:</span><span class="sxs-lookup"><span data-stu-id="7156f-189">For example:</span></span>

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [!NOTE]
> <span data-ttu-id="7156f-190">Další informace o překladu názvů v Azure virtuální sítě včetně jak toouse vlastního serveru DNS, najdete v části [rozlišení DNS (Name)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span><span class="sxs-lookup"><span data-stu-id="7156f-190">For more information about name resolution in Azure virtual networks, including how toouse your own DNS server, see [Name Resolution (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="7156f-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7156f-191">Next steps</span></span>
<span data-ttu-id="7156f-192">V tomto kurzu jste se naučili jak toocreate HBase cluster.</span><span class="sxs-lookup"><span data-stu-id="7156f-192">In this tutorial, you learned how toocreate an HBase cluster.</span></span> <span data-ttu-id="7156f-193">toolearn více, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="7156f-193">toolearn more, see:</span></span>

* [<span data-ttu-id="7156f-194">Začínáme s HDInsight</span><span class="sxs-lookup"><span data-stu-id="7156f-194">Get started with HDInsight</span></span>](hdinsight-hadoop-linux-tutorial-get-started.md)
* [<span data-ttu-id="7156f-195">Použít prázdný edge uzly v HDInsight</span><span class="sxs-lookup"><span data-stu-id="7156f-195">Use empty edge nodes in HDInsight</span></span>](hdinsight-apps-use-edge-node.md)
* [<span data-ttu-id="7156f-196">Konfigurace replikace HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="7156f-196">Configure HBase replication in HDInsight</span></span>](hdinsight-hbase-replication.md)
* [<span data-ttu-id="7156f-197">Vytvoření clusterů systému Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="7156f-197">Create Hadoop clusters in HDInsight</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
* [<span data-ttu-id="7156f-198">Začínáme používat HBase s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="7156f-198">Get started using HBase with Hadoop in HDInsight</span></span>](hdinsight-hbase-tutorial-get-started.md)
* [<span data-ttu-id="7156f-199">Analýza sentimentu Twitter s HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="7156f-199">Analyze Twitter sentiment with HBase in HDInsight</span></span>](hdinsight-hbase-analyze-twitter-sentiment.md)
* <span data-ttu-id="7156f-200">[Přehled virtuálních sítí][vnet-overview]</span><span class="sxs-lookup"><span data-stu-id="7156f-200">[Virtual Network Overview][vnet-overview]</span></span>

[1]: http://azure.microsoft.com/services/virtual-network/
[2]: http://technet.microsoft.com/library/ee176961.aspx
[3]: http://technet.microsoft.com/library/hh847889.aspx

[hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[vnet-overview]: ../virtual-network/virtual-networks-overview.md
[vm-create]: ../virtual-machines/virtual-machines-windows-hero-tutorial.md

[azure-portal]: https://portal.azure.com
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx


[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter


[powershell-install]: /powershell/azureps-cmdlets-docs


[hdinsight-customize-cluster]: hdinsight-hadoop-customize-cluster.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-DNS.md

[img-dns-surffix]: ./media/hdinsight-hbase-provision-vnet/DNSSuffix.png
[img-primary-dns-suffix]: ./media/hdinsight-hbase-provision-vnet/PrimaryDNSSuffix.png
[img-provision-cluster-page1]: ./media/hdinsight-hbase-provision-vnet/hbasewizard1.png "Podrobnosti o zřizování pro nový cluster HBase hello"
[img-provision-cluster-page5]: ./media/hdinsight-hbase-provision-vnet/hbasewizard5.png "Pomocí akce skriptu toocustomize HBase cluster"

[azure-preview-portal]: https://portal.azure.com
