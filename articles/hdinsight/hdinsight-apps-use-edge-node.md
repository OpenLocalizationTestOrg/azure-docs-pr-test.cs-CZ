---
title: "Použít prázdný edge uzly na clustery systému Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Postup přidání prázdný hraniční uzel clusteru služby HDInsight, který může být použit jako klient a pak testovacího nebo hostitele aplikace HDInsight."
services: hdinsight
editor: cgronlun
manager: jhubbard
author: mumian
tags: azure-portal
documentationcenter: 
ms.assetid: cdc7d1b4-15d7-4d4d-a13f-c7d3a694b4fb
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: jgao
ms.openlocfilehash: e21dabcc6999b1f1047d334e782f723d0c03c2cb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-empty-edge-nodes-on-hadoop-clusters-in-hdinsight"></a><span data-ttu-id="cb24d-103">Použít prázdný edge uzly na clustery systému Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="cb24d-103">Use empty edge nodes on Hadoop clusters in HDInsight</span></span>

<span data-ttu-id="cb24d-104">Informace o postupu přidání prázdný hraniční uzel do clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="cb24d-104">Learn how to add an empty edge node to an HDInsight cluster.</span></span> <span data-ttu-id="cb24d-105">Prázdný hraniční uzel je virtuální počítač s Linuxem pomocí stejných nástrojů klient nainstalován a nakonfigurován jako headnodes, ale žádné služby Hadoop systémem.</span><span class="sxs-lookup"><span data-stu-id="cb24d-105">An empty edge node is a Linux virtual machine with the same client tools installed and configured as in the headnodes, but with no Hadoop services running.</span></span> <span data-ttu-id="cb24d-106">Hraničního uzlu můžete použít pro přístup ke clusteru, testování vaší klientské aplikace a hostování vaší klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="cb24d-106">You can use the edge node for accessing the cluster, testing your client applications, and hosting your client applications.</span></span> 

<span data-ttu-id="cb24d-107">Prázdný hraniční uzel můžete přidat do existujícího clusteru HDInsight, do nového clusteru při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="cb24d-107">You can add an empty edge node to an existing HDInsight cluster, to a new cluster when you create the cluster.</span></span> <span data-ttu-id="cb24d-108">Přidání prázdný hraniční uzel se provádí pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="cb24d-108">Adding an empty edge node is done using Azure Resource Manager template.</span></span>  <span data-ttu-id="cb24d-109">Následující příklad ukazuje, jak se provádí pomocí šablony:</span><span class="sxs-lookup"><span data-stu-id="cb24d-109">The following sample demonstrates how it is done using a template:</span></span>

    "resources": [
        {
            "name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
            "type": "Microsoft.HDInsight/clusters/applications",
            "apiVersion": "2015-03-01-preview",
            "dependsOn": [ "[concat('Microsoft.HDInsight/clusters/',parameters('clusterName'))]" ],
            "properties": {
                "marketPlaceIdentifier": "EmptyNode",
                "computeProfile": {
                    "roles": [{
                        "name": "edgenode",
                        "targetInstanceCount": 1,
                        "hardwareProfile": {
                            "vmSize": "Standard_D3"
                        }
                    }]
                },
                "installScriptActions": [{
                    "name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
                    "uri": "[parameters('installScriptAction')]",
                    "roles": ["edgenode"]
                }],
                "uninstallScriptActions": [],
                "httpsEndpoints": [],
                "applicationType": "CustomApplication"
            }
        }
    ],

<span data-ttu-id="cb24d-110">Jak je znázorněno v ukázce, můžete volitelně volat [skript akce](hdinsight-hadoop-customize-cluster-linux.md) provést další konfiguraci, například při instalaci [Apache Hue](hdinsight-hadoop-hue-linux.md) v uzlu edge.</span><span class="sxs-lookup"><span data-stu-id="cb24d-110">As shown in the sample, you can optionally call a [script action](hdinsight-hadoop-customize-cluster-linux.md) to perform additional configuration, such as installing [Apache Hue](hdinsight-hadoop-hue-linux.md) in the edge node.</span></span> <span data-ttu-id="cb24d-111">Skript akce skriptu musí být veřejně přístupné na webu.</span><span class="sxs-lookup"><span data-stu-id="cb24d-111">The script action script must be publicly accessible on the web.</span></span>  <span data-ttu-id="cb24d-112">Pokud tento skript je uložený v úložišti Azure, použijte například veřejné kontejnery nebo veřejné objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="cb24d-112">For example, if the script is stored in Azure storage, use either public containers or public blobs.</span></span>

<span data-ttu-id="cb24d-113">Velikost virtuálního počítače uzlu edge musí splňovat požadavky HDInsight clusteru pracovní uzel virtuální počítač velikost.</span><span class="sxs-lookup"><span data-stu-id="cb24d-113">The edge node virtual machine size must meet the HDInsight cluster worker node vm size requirements.</span></span> <span data-ttu-id="cb24d-114">Doporučené pracovní uzel velikosti virtuálních počítačů, najdete v části [vytvoření Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span><span class="sxs-lookup"><span data-stu-id="cb24d-114">For the recommended worker node vm sizes, see [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span></span>

<span data-ttu-id="cb24d-115">Po vytvoření hraniční uzel, můžete připojit k uzlu edge pomocí protokolu SSH a spuštění nástrojů pro klientský přístup ke clusteru Hadoop v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="cb24d-115">After you have created an edge node, you can connect to the edge node using SSH, and run client tools to access the Hadoop cluster in HDInsight.</span></span>

> [!WARNING] 
> <span data-ttu-id="cb24d-116">Pomocí prázdné hraniční uzel s HDInsight je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="cb24d-116">Using an empty edge node with HDInsight is currently in preview.</span></span> <span data-ttu-id="cb24d-117">Vlastní komponenty, které jsou nainstalovány na uzlu edge získání vyvineme podpory společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="cb24d-117">Custom components that are installed on the edge node receive commercially reasonable support from Microsoft.</span></span> <span data-ttu-id="cb24d-118">Může to způsobit řešení problémů, na které narazíte.</span><span class="sxs-lookup"><span data-stu-id="cb24d-118">This might result in resolving problems you encounter.</span></span> <span data-ttu-id="cb24d-119">Nebo může označovat komunitní zdroje pro další pomoc.</span><span class="sxs-lookup"><span data-stu-id="cb24d-119">Or you may be referred to community resources for further assistance.</span></span> <span data-ttu-id="cb24d-120">Tady jsou některé z nejvíc aktivní lokality pro získání nápovědy od komunity:</span><span class="sxs-lookup"><span data-stu-id="cb24d-120">The following are some of the most active sites for getting help from the community:</span></span>
>
> * [<span data-ttu-id="cb24d-121">Fórum MSDN pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="cb24d-121">MSDN forum for HDInsight</span></span>](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight)
> * <span data-ttu-id="cb24d-122">[http://stackoverflow.com](http://stackoverflow.com).</span><span class="sxs-lookup"><span data-stu-id="cb24d-122">[http://stackoverflow.com](http://stackoverflow.com).</span></span>
>
> <span data-ttu-id="cb24d-123">Pokud používáte technologie Apache, bude pravděpodobně možné najít pomoc prostřednictvím Apache projektu lokalit na [http://apache.org](http://apache.org), například [Hadoop](http://hadoop.apache.org/) lokality.</span><span class="sxs-lookup"><span data-stu-id="cb24d-123">If you are using an Apache technology, you may be able to find assistance through the Apache project sites on [http://apache.org](http://apache.org), such as the [Hadoop](http://hadoop.apache.org/) site.</span></span>

## <a name="add-an-edge-node-to-an-existing-cluster"></a><span data-ttu-id="cb24d-124">Přidat k existujícímu clusteru hraniční uzel</span><span class="sxs-lookup"><span data-stu-id="cb24d-124">Add an edge node to an existing cluster</span></span>
<span data-ttu-id="cb24d-125">V této části použijte šablonu Resource Manager přidat hraniční uzel do existujícího clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="cb24d-125">In this section, you use a Resource Manager template to add an edge node to an existing HDInsight cluster.</span></span>  <span data-ttu-id="cb24d-126">Šablony Resource Manageru naleznete v [Githubu](https://azure.microsoft.com/en-us/resources/templates/101-hdinsight-linux-add-edge-node/).</span><span class="sxs-lookup"><span data-stu-id="cb24d-126">The Resource Manager template can be found in [GitHub](https://azure.microsoft.com/en-us/resources/templates/101-hdinsight-linux-add-edge-node/).</span></span> <span data-ttu-id="cb24d-127">Šablony Resource Manageru volá akci skriptu nacházející se v https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-add-edge-node/scripts/EmptyNodeSetup.sh. Skript nebude provádět žádné akce.</span><span class="sxs-lookup"><span data-stu-id="cb24d-127">The Resource Manager template calls a script action located at https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-add-edge-node/scripts/EmptyNodeSetup.sh. The script doesn't perform any actions.</span></span>  <span data-ttu-id="cb24d-128">Je k předvedení volání akce skriptu z šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="cb24d-128">It is to demonstrate calling script action from a Resource Manager template.</span></span>

<span data-ttu-id="cb24d-129">**Chcete-li přidat prázdný hraniční uzel do existujícího clusteru**</span><span class="sxs-lookup"><span data-stu-id="cb24d-129">**To add an empty edge node to an existing cluster**</span></span>

1. <span data-ttu-id="cb24d-130">Pokud ještě nemáte, vytvořte cluster služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="cb24d-130">Create an HDInsight cluster if you don't have one yet.</span></span>  <span data-ttu-id="cb24d-131">V tématu [kurz Hadoopu: začněte s Hadoop v HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="cb24d-131">See [Hadoop tutorial: Get started using Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
2. <span data-ttu-id="cb24d-132">Kliknutím na následující obrázek otevřete šablonu Azure Resource Manageru na portálu Azure a přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="cb24d-132">Click the following image to sign in to Azure and open the Azure Resource Manager template in the Azure portal.</span></span> 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-add-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy to Azure"></a>
3. <span data-ttu-id="cb24d-133">Nakonfigurujte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="cb24d-133">Configure the following properties:</span></span>
   
   * <span data-ttu-id="cb24d-134">**Předplatné**: Vyberte předplatné Azure, použít pro vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="cb24d-134">**Subscription**: Select an Azure subscription used for creating the cluster.</span></span>
   * <span data-ttu-id="cb24d-135">**Skupina prostředků**: Vyberte skupinu prostředků, použít pro existující cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="cb24d-135">**Resource group**: Select the resource group used for the existing HDInsight cluster.</span></span>
   * <span data-ttu-id="cb24d-136">**Umístění**: Vyberte umístění existujícího clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="cb24d-136">**Location**: Select the location of the existing HDInsight cluster.</span></span>
   * <span data-ttu-id="cb24d-137">**Název clusteru**: Zadejte název existujícího clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="cb24d-137">**Cluster Name**: Enter the name of an existing HDInsight cluster.</span></span>
   * <span data-ttu-id="cb24d-138">**Okraj velikost uzlu**: vyberte jednu z velikosti virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="cb24d-138">**Edge Node Size**: Select one of the VM sizes.</span></span> <span data-ttu-id="cb24d-139">Velikost virtuálního počítače musí splňovat požadavky velikost pracovního procesu uzlu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cb24d-139">The vm size must meet the worker node vm size requirements.</span></span> <span data-ttu-id="cb24d-140">Doporučené pracovní uzel velikosti virtuálních počítačů, najdete v části [vytvoření Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span><span class="sxs-lookup"><span data-stu-id="cb24d-140">For the recommended worker node vm sizes, see [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span></span>
   * <span data-ttu-id="cb24d-141">**Okraj uzlu předpony**: výchozí hodnota je **nové**.</span><span class="sxs-lookup"><span data-stu-id="cb24d-141">**Edge Node Prefix**: The default value is **new**.</span></span>  <span data-ttu-id="cb24d-142">Pomocí výchozí hodnoty, je název uzlu edge **nové edgenode**.</span><span class="sxs-lookup"><span data-stu-id="cb24d-142">Using the default value, the edge node name is **new-edgenode**.</span></span>  <span data-ttu-id="cb24d-143">Můžete upravit předponu z portálu.</span><span class="sxs-lookup"><span data-stu-id="cb24d-143">You can customize the prefix from the portal.</span></span> <span data-ttu-id="cb24d-144">Můžete také upravit úplný název ze šablony.</span><span class="sxs-lookup"><span data-stu-id="cb24d-144">You can also customize the full name from the template.</span></span>

4. <span data-ttu-id="cb24d-145">Zkontrolujte **souhlasím s podmínkami a ujednáními výše uvedených**a potom klikněte na **nákupu** vytvořit hraničního uzlu.</span><span class="sxs-lookup"><span data-stu-id="cb24d-145">Check **I agree to the terms and conditions stated above**, and then click  **Purchase** to create the edge node.</span></span>

>[!IMPORTANT]
> <span data-ttu-id="cb24d-146">Ujistěte se, že vyberte skupinu prostředků Azure pro existující cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="cb24d-146">Make sure to select the Azure resource group for the existing HDInsight cluster.</span></span>  <span data-ttu-id="cb24d-147">Jinak zobrazí chybová zpráva "nelze provést požadovanou operaci na vnořeného prostředku.</span><span class="sxs-lookup"><span data-stu-id="cb24d-147">Otherwise, you get the error message "Can not perform requested operation on nested resource.</span></span> <span data-ttu-id="cb24d-148">Nadřazený prostředek '&lt;ClusterName >' nebyl nalezen. "</span><span class="sxs-lookup"><span data-stu-id="cb24d-148">Parent resource '&lt;ClusterName>' not found."</span></span>

## <a name="add-an-edge-node-when-creating-a-cluster"></a><span data-ttu-id="cb24d-149">Přidat hraniční uzel, při vytváření clusteru</span><span class="sxs-lookup"><span data-stu-id="cb24d-149">Add an edge node when creating a cluster</span></span>
<span data-ttu-id="cb24d-150">V této části použijte k vytvoření clusteru HDInsight s hraniční uzel, na šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="cb24d-150">In this section, you use a Resource Manager template to create HDInsight cluster with an edge node.</span></span>  <span data-ttu-id="cb24d-151">Šablony Resource Manageru najdete v [galerii šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates/101-hdinsight-linux-with-edge-node/).</span><span class="sxs-lookup"><span data-stu-id="cb24d-151">The Resource Manager template can be found in the [Azure QuickStart Templates gallery](https://azure.microsoft.com/documentation/templates/101-hdinsight-linux-with-edge-node/).</span></span> <span data-ttu-id="cb24d-152">Šablony Resource Manageru volá akci skriptu nacházející se v https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-with-edge-node/scripts/EmptyNodeSetup.sh. Skript nebude provádět žádné akce.</span><span class="sxs-lookup"><span data-stu-id="cb24d-152">The Resource Manager template calls a script action located at https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-with-edge-node/scripts/EmptyNodeSetup.sh. The script doesn't perform any actions.</span></span>  <span data-ttu-id="cb24d-153">Je k předvedení volání akce skriptu z šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="cb24d-153">It is to demonstrate calling script action from a Resource Manager template.</span></span>

<span data-ttu-id="cb24d-154">**Chcete-li přidat prázdný hraniční uzel do existujícího clusteru**</span><span class="sxs-lookup"><span data-stu-id="cb24d-154">**To add an empty edge node to an existing cluster**</span></span>

1. <span data-ttu-id="cb24d-155">Pokud ještě nemáte, vytvořte cluster služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="cb24d-155">Create an HDInsight cluster if you don't have one yet.</span></span>  <span data-ttu-id="cb24d-156">V tématu [začněte s Hadoop v HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="cb24d-156">See [Get started using Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
2. <span data-ttu-id="cb24d-157">Kliknutím na následující obrázek otevřete šablonu Azure Resource Manageru na portálu Azure a přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="cb24d-157">Click the following image to sign in to Azure and open the Azure Resource Manager template in the Azure portal.</span></span> 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy to Azure"></a>
3. <span data-ttu-id="cb24d-158">Nakonfigurujte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="cb24d-158">Configure the following properties:</span></span>
   
   * <span data-ttu-id="cb24d-159">**Předplatné**: Vyberte předplatné Azure, použít pro vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="cb24d-159">**Subscription**: Select an Azure subscription used for creating the cluster.</span></span>
   * <span data-ttu-id="cb24d-160">**Skupina prostředků**: Vytvořte novou skupinu prostředků používat pro cluster.</span><span class="sxs-lookup"><span data-stu-id="cb24d-160">**Resource group**: Create a new resource group used for the cluster.</span></span>
   * <span data-ttu-id="cb24d-161">**Umístění:** Vyberte umístění pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="cb24d-161">**Location**: Select a location for the resource group.</span></span>
   * <span data-ttu-id="cb24d-162">**Název clusteru**: Zadejte název pro nový cluster vytvořit.</span><span class="sxs-lookup"><span data-stu-id="cb24d-162">**Cluster Name**: Enter a name for the new cluster to create.</span></span>
   * <span data-ttu-id="cb24d-163">**Uživatelské jméno přihlášení clusteru**: Zadejte uživatelské jméno Hadoop HTTP.</span><span class="sxs-lookup"><span data-stu-id="cb24d-163">**Cluster Login User Name**: Enter the Hadoop HTTP user name.</span></span>  <span data-ttu-id="cb24d-164">Výchozí název je **správce**.</span><span class="sxs-lookup"><span data-stu-id="cb24d-164">The default name is **admin**.</span></span>
   * <span data-ttu-id="cb24d-165">**Heslo pro přihlášení clusteru**: Zadejte heslo k uživatelskému Hadoop HTTP.</span><span class="sxs-lookup"><span data-stu-id="cb24d-165">**Cluster Login Password**: Enter the Hadoop HTTP user password.</span></span>
   * <span data-ttu-id="cb24d-166">**SSH uživatelské jméno**: Zadejte uživatelské jméno SSH.</span><span class="sxs-lookup"><span data-stu-id="cb24d-166">**Ssh User Name**: Enter the SSH user name.</span></span> <span data-ttu-id="cb24d-167">Výchozí název je **sshuser**.</span><span class="sxs-lookup"><span data-stu-id="cb24d-167">The default name is **sshuser**.</span></span>
   * <span data-ttu-id="cb24d-168">**SSH heslo**: Zadejte heslo uživatele SSH.</span><span class="sxs-lookup"><span data-stu-id="cb24d-168">**Ssh Password**: Enter the SSH user password.</span></span>
   * <span data-ttu-id="cb24d-169">**Instalace akce skriptu**: ponechte výchozí hodnotu pro procházení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="cb24d-169">**Install Script Action**: Keep the default value for going through this tutorial.</span></span>
     
     <span data-ttu-id="cb24d-170">Některé vlastnosti byly pevně kódovaný v šabloně: typ clusteru, počet uzlů pracovního procesu clusteru, velikost uzlu Edge a název uzlu Edge.</span><span class="sxs-lookup"><span data-stu-id="cb24d-170">Some properties have been hardcoded in the template: Cluster type, Cluster worker node count, Edge node size, and Edge node name.</span></span>
4. <span data-ttu-id="cb24d-171">Zkontrolujte **souhlasím s podmínkami a ujednáními výše uvedených**a potom klikněte na **nákupu** k vytvoření clusteru s hraničního uzlu.</span><span class="sxs-lookup"><span data-stu-id="cb24d-171">Check **I agree to the terms and conditions stated above**, and then click  **Purchase** to create the cluster with the edge node.</span></span>

## <a name="access-an-edge-node"></a><span data-ttu-id="cb24d-172">Přístup uzlu edge</span><span class="sxs-lookup"><span data-stu-id="cb24d-172">Access an edge node</span></span>
<span data-ttu-id="cb24d-173">Hraničního uzlu ssh koncový bod je &lt;EdgeNodeName >.&lt; Název clusteru >-ssh.azurehdinsight.net:22.</span><span class="sxs-lookup"><span data-stu-id="cb24d-173">The edge node ssh endpoint is &lt;EdgeNodeName>.&lt;ClusterName>-ssh.azurehdinsight.net:22.</span></span>  <span data-ttu-id="cb24d-174">Například nové edgenode.myedgenode0914-ssh.azurehdinsight.net:22.</span><span class="sxs-lookup"><span data-stu-id="cb24d-174">For example, new-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.</span></span>

<span data-ttu-id="cb24d-175">Hraničního uzlu se zobrazí jako aplikace na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cb24d-175">The edge node appears as an application on the Azure portal.</span></span>  <span data-ttu-id="cb24d-176">Portál obsahuje informace, přístup k uzlu edge pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="cb24d-176">The portal gives you the information to access the edge node using SSH.</span></span>

<span data-ttu-id="cb24d-177">**Chcete-li ověřit koncový bod SSH uzlu edge**</span><span class="sxs-lookup"><span data-stu-id="cb24d-177">**To verify the edge node SSH endpoint**</span></span>

1. <span data-ttu-id="cb24d-178">Přihlaste se k portálu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cb24d-178">Sign on to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="cb24d-179">Otevřete HDInsight cluster se hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="cb24d-179">Open the HDInsight cluster with an edge node.</span></span>
3. <span data-ttu-id="cb24d-180">Klikněte na tlačítko **aplikace** z okna clusteru.</span><span class="sxs-lookup"><span data-stu-id="cb24d-180">Click **Applications** from the cluster blade.</span></span> <span data-ttu-id="cb24d-181">Zobrazí se hraničního uzlu.</span><span class="sxs-lookup"><span data-stu-id="cb24d-181">You shall see the edge node.</span></span>  <span data-ttu-id="cb24d-182">Výchozí název je **nové edgenode**.</span><span class="sxs-lookup"><span data-stu-id="cb24d-182">The default name is **new-edgenode**.</span></span>
4. <span data-ttu-id="cb24d-183">Klikněte na tlačítko hraničního uzlu.</span><span class="sxs-lookup"><span data-stu-id="cb24d-183">Click the edge node.</span></span> <span data-ttu-id="cb24d-184">Zobrazí se koncový bod SSH.</span><span class="sxs-lookup"><span data-stu-id="cb24d-184">You shall see the SSH endpoint.</span></span>

<span data-ttu-id="cb24d-185">**Používání Hive v uzlu edge**</span><span class="sxs-lookup"><span data-stu-id="cb24d-185">**To use Hive on the edge node**</span></span>

1. <span data-ttu-id="cb24d-186">Použití SSH se připojit k uzlu edge.</span><span class="sxs-lookup"><span data-stu-id="cb24d-186">Use SSH to connect to the edge node.</span></span> <span data-ttu-id="cb24d-187">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="cb24d-187">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="cb24d-188">Po připojení k uzlu edge pomocí protokolu SSH, použijte následující příkaz pro otevření konzoly Hive:</span><span class="sxs-lookup"><span data-stu-id="cb24d-188">After you have connected to the edge node using SSH, use the following command to open the Hive console:</span></span>
   
        hive
3. <span data-ttu-id="cb24d-189">Spusťte následující příkaz k zobrazení tabulek Hive v clusteru:</span><span class="sxs-lookup"><span data-stu-id="cb24d-189">Run the following command to show Hive tables in the cluster:</span></span>
   
        show tables;

## <a name="delete-an-edge-node"></a><span data-ttu-id="cb24d-190">Odstranit hraniční uzel</span><span class="sxs-lookup"><span data-stu-id="cb24d-190">Delete an edge node</span></span>
<span data-ttu-id="cb24d-191">Hraniční uzel můžete odstranit z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cb24d-191">You can delete an edge node from the Azure portal.</span></span>

<span data-ttu-id="cb24d-192">**Pro přístup uzlu edge**</span><span class="sxs-lookup"><span data-stu-id="cb24d-192">**To access an edge node**</span></span>

1. <span data-ttu-id="cb24d-193">Přihlaste se k portálu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cb24d-193">Sign on to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="cb24d-194">Otevřete HDInsight cluster se hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="cb24d-194">Open the HDInsight cluster with an edge node.</span></span>
3. <span data-ttu-id="cb24d-195">Klikněte na tlačítko **aplikace** z okna clusteru.</span><span class="sxs-lookup"><span data-stu-id="cb24d-195">Click **Applications** from the cluster blade.</span></span> <span data-ttu-id="cb24d-196">Zobrazí se seznam uzlů okraj.</span><span class="sxs-lookup"><span data-stu-id="cb24d-196">You shall see a list of edge nodes.</span></span>  
4. <span data-ttu-id="cb24d-197">Klikněte pravým tlačítkem na hraniční uzel, který chcete odstranit a potom klikněte na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="cb24d-197">Right-click the edge node you want to delete, and then click **Delete**.</span></span>
5. <span data-ttu-id="cb24d-198">Pro potvrzení klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="cb24d-198">Click **Yes** to confirm.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cb24d-199">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cb24d-199">Next steps</span></span>
<span data-ttu-id="cb24d-200">V tomto článku jste se naučili postup přidání hraniční uzel a jak získat přístup k uzlu edge.</span><span class="sxs-lookup"><span data-stu-id="cb24d-200">In this article, you have learned how to add an edge node and how to access the edge node.</span></span> <span data-ttu-id="cb24d-201">Další informace naleznete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="cb24d-201">To learn more, see the following articles:</span></span>

* <span data-ttu-id="cb24d-202">[Instalace aplikací HDInsight](hdinsight-apps-install-applications.md): Naučte se instalovat aplikace HDInsight do svých clusterů.</span><span class="sxs-lookup"><span data-stu-id="cb24d-202">[Install HDInsight applications](hdinsight-apps-install-applications.md): Learn how to install an HDInsight application to your clusters.</span></span>
* <span data-ttu-id="cb24d-203">[Instalace vlastních aplikací HDInsight](hdinsight-apps-install-custom-applications.md): naučit se nasazovat nepublikované aplikace HDInsight do HDInsight.</span><span class="sxs-lookup"><span data-stu-id="cb24d-203">[Install custom HDInsight applications](hdinsight-apps-install-custom-applications.md): learn how to deploy an unpublished HDInsight application to HDInsight.</span></span>
* <span data-ttu-id="cb24d-204">[Publikování aplikací HDInsight](hdinsight-apps-publish-applications.md): Zjistěte, jak publikovat vlastní aplikace HDInsight do obchodu Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="cb24d-204">[Publish HDInsight applications](hdinsight-apps-publish-applications.md): Learn how to publish your custom HDInsight applications to Azure Marketplace.</span></span>
* <span data-ttu-id="cb24d-205">[MSDN: Instalace aplikace HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Další informace jak definovat aplikace HDInsight.</span><span class="sxs-lookup"><span data-stu-id="cb24d-205">[MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx): Learn how to define HDInsight applications.</span></span>
* <span data-ttu-id="cb24d-206">[Přizpůsobení clusterů HDInsight v systému Linux pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md): další informace o použití akce skriptu k instalaci dalších aplikací.</span><span class="sxs-lookup"><span data-stu-id="cb24d-206">[Customize Linux-based HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md): learn how to use Script Action to install additional applications.</span></span>
* <span data-ttu-id="cb24d-207">[Vytváření clusterů Hadoop na systému Linux v HDInsight pomocí šablon Resource Manageru](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Zjistěte, jak voláním šablon Resource Manageru vytvoříte clustery HDInsight.</span><span class="sxs-lookup"><span data-stu-id="cb24d-207">[Create Linux-based Hadoop clusters in HDInsight using Resource Manager templates](hdinsight-hadoop-create-linux-clusters-arm-templates.md): learn how to call Resource Manager templates to create HDInsight clusters.</span></span>

