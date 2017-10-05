---
title: "Vytvořit clustery HBase ve virtuální síti - Azure | Microsoft Docs"
description: "Začínáme používat HBase v Azure HDInsight. Naučte se vytvářet clustery HDInsight HBase v Azure Virtual Network."
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
ms.openlocfilehash: 668bd494ce3274188af56cf7d6253cec7af9abbc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-hbase-clusters-on-hdinsight-in-azure-virtual-network"></a><span data-ttu-id="53dfe-104">Vytvořit clustery HBase v HDInsight v Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="53dfe-104">Create HBase clusters on HDInsight in Azure Virtual Network</span></span>
<span data-ttu-id="53dfe-105">Naučte se vytvářet clustery Azure HDInsight HBase v [Azure Virtual Network][1].</span><span class="sxs-lookup"><span data-stu-id="53dfe-105">Learn how to create Azure HDInsight HBase clusters in an [Azure Virtual Network][1].</span></span>

<span data-ttu-id="53dfe-106">Integrace virtuální sítě může být nasazena clustery HBase na stejné virtuální síti jako aplikace, aby aplikace přímo komunikovat s HBase.</span><span class="sxs-lookup"><span data-stu-id="53dfe-106">With virtual network integration, HBase clusters can be deployed to the same virtual network as your applications so that applications can communicate with HBase directly.</span></span> <span data-ttu-id="53dfe-107">Nabízí například tyto výhody:</span><span class="sxs-lookup"><span data-stu-id="53dfe-107">The benefits include:</span></span>

* <span data-ttu-id="53dfe-108">Přímé připojení k webové aplikace do uzlů clusteru HBase, který umožňuje komunikaci prostřednictvím vzdálené procedury HBase Java volání rozhraní API (RPC).</span><span class="sxs-lookup"><span data-stu-id="53dfe-108">Direct connectivity of the web application to the nodes of the HBase cluster, which enables communication via HBase Java remote procedure call (RPC) APIs.</span></span>
* <span data-ttu-id="53dfe-109">Zvýšení výkonu tak, že není provozu přejděte přes více bran a nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="53dfe-109">Improved performance by not having your traffic go over multiple gateways and load-balancers.</span></span>
* <span data-ttu-id="53dfe-110">Schopnost zpracovávat citlivé informace bezpečnější způsobem bez vystavení veřejný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="53dfe-110">The ability to process sensitive information in a more secure manner without exposing a public endpoint.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="53dfe-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="53dfe-111">Prerequisites</span></span>
<span data-ttu-id="53dfe-112">Před zahájením tohoto kurzu musíte mít tyto položky:</span><span class="sxs-lookup"><span data-stu-id="53dfe-112">Before you begin this tutorial, you must have the following items:</span></span>

* <span data-ttu-id="53dfe-113">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="53dfe-113">**An Azure subscription**.</span></span> <span data-ttu-id="53dfe-114">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="53dfe-114">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="53dfe-115">**Pracovní stanice s prostředím Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="53dfe-115">**A workstation with Azure PowerShell**.</span></span> <span data-ttu-id="53dfe-116">V tématu [instalace a použití prostředí Azure PowerShell](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/).</span><span class="sxs-lookup"><span data-stu-id="53dfe-116">See [Install and use Azure PowerShell](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/).</span></span>

## <a name="create-hbase-cluster-into-virtual-network"></a><span data-ttu-id="53dfe-117">Vytvoření clusteru HBase do virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="53dfe-117">Create HBase cluster into virtual network</span></span>
<span data-ttu-id="53dfe-118">V této části vytvoříte clusteru s podporou systémem Linux HBase s závislý účet Azure Storage virtuální síti Azure pomocí [šablony Azure Resource Manageru](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="53dfe-118">In this section, you create a Linux-based HBase cluster with the dependent Azure Storage account in an Azure virtual network using an [Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span> <span data-ttu-id="53dfe-119">Další metody vytváření clusterů a Principy nastavení, najdete v tématu [Tvorba clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="53dfe-119">For other cluster creation methods and understanding the settings, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span> <span data-ttu-id="53dfe-120">Další informace o používání šablonu při vytváření clusterů systému Hadoop v HDInsight najdete v tématu [vytvoření Hadoop clusterů v HDInsight pomocí šablony Azure Resource Manager](hdinsight-hadoop-create-windows-clusters-arm-templates.md)</span><span class="sxs-lookup"><span data-stu-id="53dfe-120">For more information about using a template to create Hadoop clusters in HDInsight, see [Create Hadoop clusters in HDInsight using Azure Resource Manager templates](hdinsight-hadoop-create-windows-clusters-arm-templates.md)</span></span>

> [!NOTE]
> <span data-ttu-id="53dfe-121">Některé vlastnosti jsou pevně zakódovaná do šablony.</span><span class="sxs-lookup"><span data-stu-id="53dfe-121">Some properties are hard-coded into the template.</span></span> <span data-ttu-id="53dfe-122">Například:</span><span class="sxs-lookup"><span data-stu-id="53dfe-122">For example:</span></span>
>
> * <span data-ttu-id="53dfe-123">**Umístění**: východní USA 2</span><span class="sxs-lookup"><span data-stu-id="53dfe-123">**Location**: East US 2</span></span>
> * <span data-ttu-id="53dfe-124">**Verze clusteru:** 3.5</span><span class="sxs-lookup"><span data-stu-id="53dfe-124">**Cluster version**: 3.5</span></span>
> * <span data-ttu-id="53dfe-125">**Počet uzlů pracovního procesu clusteru**: 2</span><span class="sxs-lookup"><span data-stu-id="53dfe-125">**Cluster worker node count**: 2</span></span>
> * <span data-ttu-id="53dfe-126">**Výchozí účet úložiště**: jedinečné řetězce</span><span class="sxs-lookup"><span data-stu-id="53dfe-126">**Default storage account**: a unique string</span></span>
> * <span data-ttu-id="53dfe-127">**Název virtuální sítě**: &lt;název clusteru >-virtuální síť</span><span class="sxs-lookup"><span data-stu-id="53dfe-127">**Virtual network name**: &lt;Cluster Name>-vnet</span></span>
> * <span data-ttu-id="53dfe-128">**Adresní prostor virtuální sítě**: 10.0.0.0/16</span><span class="sxs-lookup"><span data-stu-id="53dfe-128">**Virtual network address space**: 10.0.0.0/16</span></span>
> * <span data-ttu-id="53dfe-129">**Název podsítě**: subnet1</span><span class="sxs-lookup"><span data-stu-id="53dfe-129">**Subnet name**: subnet1</span></span>
> * <span data-ttu-id="53dfe-130">**Rozsah adres podsítě**: 10.0.0.0/24</span><span class="sxs-lookup"><span data-stu-id="53dfe-130">**Subnet address range**: 10.0.0.0/24</span></span>
>
> <span data-ttu-id="53dfe-131">&lt;Název clusteru > se nahradí název clusteru, poskytují při použití šablony.</span><span class="sxs-lookup"><span data-stu-id="53dfe-131">&lt;Cluster Name> is replaced with the cluster name you provide when using the template.</span></span>
>
>

1. <span data-ttu-id="53dfe-132">Kliknutím na následující obrázek otevřete šablonu na portálu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="53dfe-132">Click the following image to open the template in the Azure portal.</span></span> <span data-ttu-id="53dfe-133">Šablona se nachází v [šablon Azure rychlý Start](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-linux-vnet/).</span><span class="sxs-lookup"><span data-stu-id="53dfe-133">The template is located in [Azure QuickStart Templates](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-linux-vnet/).</span></span>

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-provision-vnet/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. <span data-ttu-id="53dfe-134">Z **vlastní nasazení** okno, zadejte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="53dfe-134">From the **Custom deployment** blade, enter the following properties:</span></span>

   * <span data-ttu-id="53dfe-135">**Předplatné**: Vyberte předplatné Azure použité k vytvoření clusteru HDInsight, závislý účet úložiště a virtuální síť Azure.</span><span class="sxs-lookup"><span data-stu-id="53dfe-135">**Subscription**: Select an Azure subscription used to create the HDInsight cluster, the dependent Storage account and the Azure virtual network.</span></span>
   * <span data-ttu-id="53dfe-136">**Skupina prostředků**: vyberte **vytvořit nový**a zadejte nový název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="53dfe-136">**Resource group**: Select **Create new**, and specify a new resource group name.</span></span>
   * <span data-ttu-id="53dfe-137">**Umístění:** Vyberte umístění pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="53dfe-137">**Location**: Select a location for the resource group.</span></span>
   * <span data-ttu-id="53dfe-138">**Název clusteru**: Zadejte název pro cluster Hadoop, který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="53dfe-138">**ClusterName**: Enter a name for the Hadoop cluster to be created.</span></span>
   * <span data-ttu-id="53dfe-139">**Přihlašovací jméno a heslo clusteru**: výchozí přihlašovací jméno je **admin**.</span><span class="sxs-lookup"><span data-stu-id="53dfe-139">**Cluster login name and password**: The default login name is **admin**.</span></span>
   * <span data-ttu-id="53dfe-140">**Uživatelské jméno a heslo SSH**: výchozí uživatelské jméno **sshuser**.</span><span class="sxs-lookup"><span data-stu-id="53dfe-140">**SSH username and password**: The default username is **sshuser**.</span></span>  <span data-ttu-id="53dfe-141">Můžete ho změnit.</span><span class="sxs-lookup"><span data-stu-id="53dfe-141">You can rename it.</span></span>
   * <span data-ttu-id="53dfe-142">**Souhlasím s podmínek a ujednání uvedené výše**: (vyberte)</span><span class="sxs-lookup"><span data-stu-id="53dfe-142">**I agree to the terms and the conditions stated above**: (Select)</span></span>
3. <span data-ttu-id="53dfe-143">Klikněte na **Koupit**.</span><span class="sxs-lookup"><span data-stu-id="53dfe-143">Click **Purchase**.</span></span> <span data-ttu-id="53dfe-144">Vytvoření clusteru trvá přibližně 20 minut.</span><span class="sxs-lookup"><span data-stu-id="53dfe-144">It takes about around 20 minutes to create a cluster.</span></span> <span data-ttu-id="53dfe-145">Jakmile je vytvořen cluster, můžete kliknout na okně clusteru na portálu a otevřete jej.</span><span class="sxs-lookup"><span data-stu-id="53dfe-145">Once the cluster is created, you can click the cluster blade in the portal to open it.</span></span>

<span data-ttu-id="53dfe-146">Po dokončení tohoto kurzu můžete cluster odstranit.</span><span class="sxs-lookup"><span data-stu-id="53dfe-146">After you complete the tutorial, you might want to delete the cluster.</span></span> <span data-ttu-id="53dfe-147">Pomocí HDInsight jsou vaše data uložena v Azure Storage, takže můžete clusteru bezpečně odstranit, pokud není používán.</span><span class="sxs-lookup"><span data-stu-id="53dfe-147">With HDInsight, your data is stored in Azure Storage, so you can safely delete a cluster when it is not in use.</span></span> <span data-ttu-id="53dfe-148">Za cluster služby HDInsight se účtují poplatky, i když se nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="53dfe-148">You are also charged for an HDInsight cluster, even when it is not in use.</span></span> <span data-ttu-id="53dfe-149">Vzhledem k tomu, že poplatky za cluster představují několikanásobek poplatků za úložiště, dává ekonomický smysl odstraňovat clustery, které nejsou používány.</span><span class="sxs-lookup"><span data-stu-id="53dfe-149">Since the charges for the cluster are many times more than the charges for storage, it makes economic sense to delete clusters when they are not in use.</span></span> <span data-ttu-id="53dfe-150">Postup odstranění clusteru naleznete v tématu [Správa clusterů systému Hadoop v HDInsight pomocí portálu Azure](hdinsight-administer-use-management-portal.md#delete-clusters).</span><span class="sxs-lookup"><span data-stu-id="53dfe-150">For the instructions of deleting a cluster, see [Manage Hadoop clusters in HDInsight by using the Azure portal](hdinsight-administer-use-management-portal.md#delete-clusters).</span></span>

<span data-ttu-id="53dfe-151">Pokud chcete začít pracovat s nového clusteru HBase, můžete použít postupy v nalezen [Začínáme používat HBase s Hadoop v HDInsight](hdinsight-hbase-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="53dfe-151">To begin working with your new HBase cluster, you can use the procedures found in [Get started using HBase with Hadoop in HDInsight](hdinsight-hbase-tutorial-get-started.md).</span></span>

## <a name="connect-to-the-hbase-cluster-using-hbase-java-rpc-apis"></a><span data-ttu-id="53dfe-152">Připojte se ke clusteru HBase pomocí rozhraní API RPC Java HBase</span><span class="sxs-lookup"><span data-stu-id="53dfe-152">Connect to the HBase cluster using HBase Java RPC APIs</span></span>
1. <span data-ttu-id="53dfe-153">Vytváření infrastruktury jako služby (IaaS) virtuální počítač do stejné virtuální síti Azure a stejné podsíti.</span><span class="sxs-lookup"><span data-stu-id="53dfe-153">Create an infrastructure as a service (IaaS) virtual machine into the same Azure virtual network and the same subnet.</span></span> <span data-ttu-id="53dfe-154">Pokyny pro vytvoření nového virtuálního počítače IaaS, najdete v části [vytvořit virtuální počítač spuštěný Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="53dfe-154">For instructions on creating a new IaaS virtual machine, see [Create a Virtual Machine Running Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md).</span></span> <span data-ttu-id="53dfe-155">Když postupovat podle kroků v tomto dokumentu, musíte použít následující hodnoty pro konfiguraci sítě:</span><span class="sxs-lookup"><span data-stu-id="53dfe-155">When following the steps in this document, you must use the following values for the Network configuration:</span></span>

   * <span data-ttu-id="53dfe-156">**Virtuální síť**: &lt;název clusteru >-virtuální síť</span><span class="sxs-lookup"><span data-stu-id="53dfe-156">**Virtual network**: &lt;Cluster name>-vnet</span></span>
   * <span data-ttu-id="53dfe-157">**Podsíť**: subnet1</span><span class="sxs-lookup"><span data-stu-id="53dfe-157">**Subnet**: subnet1</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="53dfe-158">Nahraďte &lt;název clusteru > s názvem, který jste použili při vytváření clusteru HDInsight v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="53dfe-158">Replace &lt;Cluster name> with the name you used when creating the HDInsight cluster in previous steps.</span></span>
   >
   >

   <span data-ttu-id="53dfe-159">Pomocí těchto hodnot, virtuální počítač je umístěn ve stejné virtuální síť a podsíť jako HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="53dfe-159">Using these values, the virtual machine is placed in the same virtual network and subnet as the HDInsight cluster.</span></span> <span data-ttu-id="53dfe-160">Tato konfigurace umožňuje, aby spolu navzájem přímo komunikovat.</span><span class="sxs-lookup"><span data-stu-id="53dfe-160">This configuration allows them to directly communicate with each other.</span></span> <span data-ttu-id="53dfe-161">Existuje způsob vytvoření clusteru HDInsight pomocí prázdné hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="53dfe-161">There is a way to create an HDInsight cluster with an empty edge node.</span></span> <span data-ttu-id="53dfe-162">Hraničního uzlu slouží ke správě clusteru.</span><span class="sxs-lookup"><span data-stu-id="53dfe-162">The edge node can be used to manage the cluster.</span></span>  <span data-ttu-id="53dfe-163">Další informace najdete v tématu [použít prázdný edge uzly v HDInsight](hdinsight-apps-use-edge-node.md).</span><span class="sxs-lookup"><span data-stu-id="53dfe-163">For more information, see [Use empty edge nodes in HDInsight](hdinsight-apps-use-edge-node.md).</span></span>

2. <span data-ttu-id="53dfe-164">Při použití aplikace v jazyce Java se vzdáleně připojit k HBase, musíte použít plně kvalifikovaný název domény (FQDN).</span><span class="sxs-lookup"><span data-stu-id="53dfe-164">When using a Java application to connect to HBase remotely, you must use the fully qualified domain name (FQDN).</span></span> <span data-ttu-id="53dfe-165">Určí, musíte si příponu DNS specifickou pro připojení clusteru HBase.</span><span class="sxs-lookup"><span data-stu-id="53dfe-165">To determine this, you must get the connection-specific DNS suffix of the HBase cluster.</span></span> <span data-ttu-id="53dfe-166">K tomu, můžete použít jednu z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="53dfe-166">To do that, you can use one of the following methods:</span></span>

   * <span data-ttu-id="53dfe-167">Volat pomocí Ambari pomocí webového prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="53dfe-167">Use a Web browser to make an Ambari call:</span></span>

     <span data-ttu-id="53dfe-168">Přejděte na https://&lt;ClusterName >.azurehdinsight.net/api/v1/clusters/&lt;ClusterName > / hostitelem? minimal_response = true.</span><span class="sxs-lookup"><span data-stu-id="53dfe-168">Browse to https://&lt;ClusterName>.azurehdinsight.net/api/v1/clusters/&lt;ClusterName>/hosts?minimal_response=true.</span></span> <span data-ttu-id="53dfe-169">Se změní soubor JSON s přípony DNS.</span><span class="sxs-lookup"><span data-stu-id="53dfe-169">It turns a JSON file with the DNS suffixes.</span></span>
   * <span data-ttu-id="53dfe-170">Použijte Ambari web:</span><span class="sxs-lookup"><span data-stu-id="53dfe-170">Use the Ambari website:</span></span>

     1. <span data-ttu-id="53dfe-171">Přejděte na https://&lt;ClusterName >. azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="53dfe-171">Browse to  https://&lt;ClusterName>.azurehdinsight.net.</span></span>
     2. <span data-ttu-id="53dfe-172">Klikněte na tlačítko **hostitele** v hlavní nabídce.</span><span class="sxs-lookup"><span data-stu-id="53dfe-172">Click **Hosts** from the top menu.</span></span>
   * <span data-ttu-id="53dfe-173">Použití Curl volání REST:</span><span class="sxs-lookup"><span data-stu-id="53dfe-173">Use Curl to make REST calls:</span></span>

    ```bash
        curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest
    ```

     <span data-ttu-id="53dfe-174">V datech JavaScript Object Notation (JSON) vrátil vyhledejte položku "název_hostitele".</span><span class="sxs-lookup"><span data-stu-id="53dfe-174">In the JavaScript Object Notation (JSON) data returned, find the "host_name" entry.</span></span> <span data-ttu-id="53dfe-175">Obsahuje plně kvalifikovaný název domény pro uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="53dfe-175">It contains the FQDN for the nodes in the cluster.</span></span> <span data-ttu-id="53dfe-176">Například:</span><span class="sxs-lookup"><span data-stu-id="53dfe-176">For example:</span></span>

         ...
         "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
         ...

     <span data-ttu-id="53dfe-177">Část začínající název clusteru pro název domény je příponu DNS.</span><span class="sxs-lookup"><span data-stu-id="53dfe-177">The portion of the domain name beginning with the cluster name is the DNS suffix.</span></span> <span data-ttu-id="53dfe-178">Například mycluster.b1.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="53dfe-178">For example, mycluster.b1.cloudapp.net.</span></span>
   * <span data-ttu-id="53dfe-179">Použití Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="53dfe-179">Use Azure PowerShell</span></span>

     <span data-ttu-id="53dfe-180">Pomocí následujícího skriptu prostředí Azure PowerShell k registraci **Get-ClusterDetail** funkce, které lze použít k vrácení příponu DNS:</span><span class="sxs-lookup"><span data-stu-id="53dfe-180">Use the following Azure PowerShell script to register the **Get-ClusterDetail** function, which can be used to return the DNS suffix:</span></span>

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
            Displays information to facilitate an HDInsight cluster-to-cluster scenario within the same virtual network.
            .Description
            This command shows the following 4 properties of an HDInsight cluster:
            1. ZookeeperQuorum (supports only HBase type cluster)
                Shows the value of HBase property "hbase.zookeeper.quorum".
            2. ZookeeperClientPort (supports only HBase type cluster)
                Shows the value of HBase property "hbase.zookeeper.property.clientPort".
            3. HBaseRestServers (supports only HBase type cluster)
                Shows a list of host FQDNs that run the HBase REST server.
            4. FQDNSuffix (supports all cluster types)
                Shows the FQDN suffix of hosts in the cluster.
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
            This command shows the value of HBase property "hbase.zookeeper.quorum".
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
            This command shows the value of HBase property "hbase.zookeeper.property.clientPort".
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
            This command shows a list of host FQDNs that run the HBase REST server.
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
            This command shows the FQDN suffix of hosts in the cluster.
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

     <span data-ttu-id="53dfe-181">Po spuštění skriptu prostředí Azure PowerShell, použijte následující příkaz vrátit příponu DNS pomocí **Get-ClusterDetail** funkce.</span><span class="sxs-lookup"><span data-stu-id="53dfe-181">After running the Azure PowerShell script, use the following command to return the DNS suffix by using the **Get-ClusterDetail** function.</span></span> <span data-ttu-id="53dfe-182">Při použití tohoto příkazu, zadejte název clusteru HDInsight HBase, správce jméno a heslo správce.</span><span class="sxs-lookup"><span data-stu-id="53dfe-182">Specify your HDInsight HBase cluster name, admin name, and admin password when using this command.</span></span>

    ```powershell
        Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>
    ```

     <span data-ttu-id="53dfe-183">Tento příkaz vrátí příponu DNS.</span><span class="sxs-lookup"><span data-stu-id="53dfe-183">This command returns the DNS suffix.</span></span> <span data-ttu-id="53dfe-184">Například **yourclustername.b4.internal.cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="53dfe-184">For example, **yourclustername.b4.internal.cloudapp.net**.</span></span>


<!--
3.    Change the primary DNS suffix configuration of the virtual machine. This enables the virtual machine to automatically resolve the host name of the HBase cluster without explicit specification of the suffix. For example, the *workernode0* host name will be correctly resolved to workernode0 of the HBase cluster.

    To make the configuration change:

    1. RDP into the virtual machine.
    2. Open **Local Group Policy Editor**. The executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** to the value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix][img-primary-dns-suffix]
    4. Click **OK**.
    5. Reboot the virtual machine.
-->

<span data-ttu-id="53dfe-185">Chcete-li ověřit, že virtuální počítač může komunikovat s clusterem HBase, použijte příkaz `ping headnode0.<dns suffix>` z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="53dfe-185">To verify that the virtual machine can communicate with the HBase cluster, use the command `ping headnode0.<dns suffix>` from the virtual machine.</span></span> <span data-ttu-id="53dfe-186">Například headnode0.mycluster.b1.cloudapp.net příkazu ping.</span><span class="sxs-lookup"><span data-stu-id="53dfe-186">For example, ping headnode0.mycluster.b1.cloudapp.net.</span></span>

<span data-ttu-id="53dfe-187">Tyto informace používat v aplikaci Java, můžete podle kroků v [použít Maven k sestavení aplikací Java, které používají HBase s HDInsight (Hadoop)](hdinsight-hbase-build-java-maven.md) k vytvoření aplikace.</span><span class="sxs-lookup"><span data-stu-id="53dfe-187">To use this information in a Java application, you can follow the steps in [Use Maven to build Java applications that use HBase with HDInsight (Hadoop)](hdinsight-hbase-build-java-maven.md) to create an application.</span></span> <span data-ttu-id="53dfe-188">Chcete-li mít aplikaci připojit ke vzdálenému serveru HBase, změňte **hbase-site.xml** soubor v tomto příkladu má použít plně kvalifikovaný název domény pro Zookeeper.</span><span class="sxs-lookup"><span data-stu-id="53dfe-188">To have the application connect to a remote HBase server, modify the **hbase-site.xml** file in this example to use the FQDN for Zookeeper.</span></span> <span data-ttu-id="53dfe-189">Například:</span><span class="sxs-lookup"><span data-stu-id="53dfe-189">For example:</span></span>

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [!NOTE]
> <span data-ttu-id="53dfe-190">Další informace o překladu názvů v Azure najdete v části virtuální sítě, včetně toho, jak používat vlastní server DNS [rozlišení DNS (Name)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span><span class="sxs-lookup"><span data-stu-id="53dfe-190">For more information about name resolution in Azure virtual networks, including how to use your own DNS server, see [Name Resolution (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="53dfe-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="53dfe-191">Next steps</span></span>
<span data-ttu-id="53dfe-192">V tomto kurzu jste zjistili, jak vytvořit HBase cluster.</span><span class="sxs-lookup"><span data-stu-id="53dfe-192">In this tutorial, you learned how to create an HBase cluster.</span></span> <span data-ttu-id="53dfe-193">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="53dfe-193">To learn more, see:</span></span>

* [<span data-ttu-id="53dfe-194">Začínáme s HDInsight</span><span class="sxs-lookup"><span data-stu-id="53dfe-194">Get started with HDInsight</span></span>](hdinsight-hadoop-linux-tutorial-get-started.md)
* [<span data-ttu-id="53dfe-195">Použít prázdný edge uzly v HDInsight</span><span class="sxs-lookup"><span data-stu-id="53dfe-195">Use empty edge nodes in HDInsight</span></span>](hdinsight-apps-use-edge-node.md)
* [<span data-ttu-id="53dfe-196">Konfigurace replikace HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="53dfe-196">Configure HBase replication in HDInsight</span></span>](hdinsight-hbase-replication.md)
* [<span data-ttu-id="53dfe-197">Vytvoření clusterů systému Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="53dfe-197">Create Hadoop clusters in HDInsight</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
* [<span data-ttu-id="53dfe-198">Začínáme používat HBase s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="53dfe-198">Get started using HBase with Hadoop in HDInsight</span></span>](hdinsight-hbase-tutorial-get-started.md)
* [<span data-ttu-id="53dfe-199">Analýza sentimentu Twitter s HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="53dfe-199">Analyze Twitter sentiment with HBase in HDInsight</span></span>](hdinsight-hbase-analyze-twitter-sentiment.md)
* <span data-ttu-id="53dfe-200">[Přehled virtuálních sítí][vnet-overview]</span><span class="sxs-lookup"><span data-stu-id="53dfe-200">[Virtual Network Overview][vnet-overview]</span></span>

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
[img-provision-cluster-page1]: ./media/hdinsight-hbase-provision-vnet/hbasewizard1.png "Podrobnosti o zřizování pro nový cluster HBase"
[img-provision-cluster-page5]: ./media/hdinsight-hbase-provision-vnet/hbasewizard5.png "Použití akce skriptu k přizpůsobení HBase cluster"

[azure-preview-portal]: https://portal.azure.com
