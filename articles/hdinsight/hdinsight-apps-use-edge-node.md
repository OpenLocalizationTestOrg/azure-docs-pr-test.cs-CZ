---
title: "aaaUse prázdný edge uzly clusterů systému Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Jak tooadd prázdný hraniční uzel tooan HDInsight clusteru, který může být použit jako klient a potom testovacího nebo hostitele aplikace HDInsight."
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
ms.openlocfilehash: 9c910905b51f2fe92e6e5d47d86a32bd5247c2cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-empty-edge-nodes-on-hadoop-clusters-in-hdinsight"></a><span data-ttu-id="a5ae9-103">Použít prázdný edge uzly na clustery systému Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a5ae9-103">Use empty edge nodes on Hadoop clusters in HDInsight</span></span>

<span data-ttu-id="a5ae9-104">Zjistěte, jak tooadd prázdnou okraj uzlu clusteru HDInsight tooan.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-104">Learn how tooadd an empty edge node tooan HDInsight cluster.</span></span> <span data-ttu-id="a5ae9-105">Prázdný hraniční uzel je virtuální počítač s Linuxem s hello stejné nástrojích klienta nainstalován a nakonfigurován jako hello headnodes, ale žádné služby Hadoop systémem.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-105">An empty edge node is a Linux virtual machine with hello same client tools installed and configured as in hello headnodes, but with no Hadoop services running.</span></span> <span data-ttu-id="a5ae9-106">Hello hraniční uzel můžete použít pro přístup k hello clusteru, testování vaší klientské aplikace a hostování vaší klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-106">You can use hello edge node for accessing hello cluster, testing your client applications, and hosting your client applications.</span></span> 

<span data-ttu-id="a5ae9-107">Při vytváření clusteru hello můžete přidat prázdný hraniční uzel tooan stávajícího clusteru HDInsight, tooa nového clusteru.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-107">You can add an empty edge node tooan existing HDInsight cluster, tooa new cluster when you create hello cluster.</span></span> <span data-ttu-id="a5ae9-108">Přidání prázdný hraniční uzel se provádí pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-108">Adding an empty edge node is done using Azure Resource Manager template.</span></span>  <span data-ttu-id="a5ae9-109">Hello následující příklad ukazuje, jak se provádí pomocí šablony:</span><span class="sxs-lookup"><span data-stu-id="a5ae9-109">hello following sample demonstrates how it is done using a template:</span></span>

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

<span data-ttu-id="a5ae9-110">Jak je znázorněno v ukázce hello, můžete volitelně volat [skript akce](hdinsight-hadoop-customize-cluster-linux.md) tooperform další konfiguraci, například při instalaci [Apache Hue](hdinsight-hadoop-hue-linux.md) v uzlu edge hello.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-110">As shown in hello sample, you can optionally call a [script action](hdinsight-hadoop-customize-cluster-linux.md) tooperform additional configuration, such as installing [Apache Hue](hdinsight-hadoop-hue-linux.md) in hello edge node.</span></span> <span data-ttu-id="a5ae9-111">Hello skript akce skriptu musí být veřejně přístupné na webu hello.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-111">hello script action script must be publicly accessible on hello web.</span></span>  <span data-ttu-id="a5ae9-112">Například pokud hello skript je uložený v úložišti Azure, použijte veřejné kontejnery nebo veřejné objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-112">For example, if hello script is stored in Azure storage, use either public containers or public blobs.</span></span>

<span data-ttu-id="a5ae9-113">velikost virtuálního počítače uzlu edge Hello musí splňovat požadavky velikost virtuálního počítače pracovní hello HDInsight clusteru uzlu.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-113">hello edge node virtual machine size must meet hello HDInsight cluster worker node vm size requirements.</span></span> <span data-ttu-id="a5ae9-114">Hello nedoporučuje pracovní uzel velikosti virtuálních počítačů, najdete v části [vytvoření Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span><span class="sxs-lookup"><span data-stu-id="a5ae9-114">For hello recommended worker node vm sizes, see [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span></span>

<span data-ttu-id="a5ae9-115">Po vytvoření hraniční uzel, můžete připojit pomocí protokolu SSH toohello hraniční uzel a spusťte klienta nástroje tooaccess hello Hadoop clusteru v prostředí HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-115">After you have created an edge node, you can connect toohello edge node using SSH, and run client tools tooaccess hello Hadoop cluster in HDInsight.</span></span>

> [!WARNING] 
> <span data-ttu-id="a5ae9-116">Pomocí prázdné hraniční uzel s HDInsight je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-116">Using an empty edge node with HDInsight is currently in preview.</span></span> <span data-ttu-id="a5ae9-117">Vlastní komponenty, které jsou nainstalovány na uzlu edge hello získání vyvineme podpory společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-117">Custom components that are installed on hello edge node receive commercially reasonable support from Microsoft.</span></span> <span data-ttu-id="a5ae9-118">Může to způsobit řešení problémů, na které narazíte.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-118">This might result in resolving problems you encounter.</span></span> <span data-ttu-id="a5ae9-119">Nebo může být toocommunity odkazované prostředky pro další pomoc.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-119">Or you may be referred toocommunity resources for further assistance.</span></span> <span data-ttu-id="a5ae9-120">Hello Toto jsou některé hello většina aktivní lokality pro získání pomoci z komunity hello:</span><span class="sxs-lookup"><span data-stu-id="a5ae9-120">hello following are some of hello most active sites for getting help from hello community:</span></span>
>
> * [<span data-ttu-id="a5ae9-121">Fórum MSDN pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="a5ae9-121">MSDN forum for HDInsight</span></span>](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight)
> * <span data-ttu-id="a5ae9-122">[http://stackoverflow.com](http://stackoverflow.com).</span><span class="sxs-lookup"><span data-stu-id="a5ae9-122">[http://stackoverflow.com](http://stackoverflow.com).</span></span>
>
> <span data-ttu-id="a5ae9-123">Pokud používáte technologie Apache, možná budete moct toofind pomoc prostřednictvím hello Apache projektu weby na [http://apache.org](http://apache.org), jako je například hello [Hadoop](http://hadoop.apache.org/) lokality.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-123">If you are using an Apache technology, you may be able toofind assistance through hello Apache project sites on [http://apache.org](http://apache.org), such as hello [Hadoop](http://hadoop.apache.org/) site.</span></span>

## <a name="add-an-edge-node-tooan-existing-cluster"></a><span data-ttu-id="a5ae9-124">Přidání okraj uzlu tooan stávajícího clusteru</span><span class="sxs-lookup"><span data-stu-id="a5ae9-124">Add an edge node tooan existing cluster</span></span>
<span data-ttu-id="a5ae9-125">V této části použijte Správce prostředků šablony tooadd hraniční uzel tooan stávajícího clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-125">In this section, you use a Resource Manager template tooadd an edge node tooan existing HDInsight cluster.</span></span>  <span data-ttu-id="a5ae9-126">šablony Resource Manageru Hello lze nalézt v [Githubu](https://azure.microsoft.com/en-us/resources/templates/101-hdinsight-linux-add-edge-node/).</span><span class="sxs-lookup"><span data-stu-id="a5ae9-126">hello Resource Manager template can be found in [GitHub](https://azure.microsoft.com/en-us/resources/templates/101-hdinsight-linux-add-edge-node/).</span></span> <span data-ttu-id="a5ae9-127">šablony Resource Manageru Hello volá akci skriptu nacházející se v https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-add-edge-node/scripts/EmptyNodeSetup.sh. skript Hello nebude provádět žádné akce.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-127">hello Resource Manager template calls a script action located at https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-add-edge-node/scripts/EmptyNodeSetup.sh. hello script doesn't perform any actions.</span></span>  <span data-ttu-id="a5ae9-128">Je toodemonstrate volání akce skriptu z šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-128">It is toodemonstrate calling script action from a Resource Manager template.</span></span>

<span data-ttu-id="a5ae9-129">**tooadd prázdný hraniční uzel tooan stávajícího clusteru**</span><span class="sxs-lookup"><span data-stu-id="a5ae9-129">**tooadd an empty edge node tooan existing cluster**</span></span>

1. <span data-ttu-id="a5ae9-130">Pokud ještě nemáte, vytvořte cluster služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-130">Create an HDInsight cluster if you don't have one yet.</span></span>  <span data-ttu-id="a5ae9-131">V tématu [kurz Hadoopu: začněte s Hadoop v HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a5ae9-131">See [Hadoop tutorial: Get started using Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
2. <span data-ttu-id="a5ae9-132">Klikněte na tlačítko hello toosign bitové kopie v tooAzure a šablony Azure Resource Manageru otevřete hello v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-132">Click hello following image toosign in tooAzure and open hello Azure Resource Manager template in hello Azure portal.</span></span> 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-add-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy tooAzure"></a>
3. <span data-ttu-id="a5ae9-133">Nakonfigurujte hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="a5ae9-133">Configure hello following properties:</span></span>
   
   * <span data-ttu-id="a5ae9-134">**Předplatné**: Vyberte předplatné Azure použitý k vytvoření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-134">**Subscription**: Select an Azure subscription used for creating hello cluster.</span></span>
   * <span data-ttu-id="a5ae9-135">**Skupina prostředků**: Skupina prostředků vyberte hello používá pro existující cluster HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-135">**Resource group**: Select hello resource group used for hello existing HDInsight cluster.</span></span>
   * <span data-ttu-id="a5ae9-136">**Umístění**: Vybrat umístění hello hello stávajícího clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-136">**Location**: Select hello location of hello existing HDInsight cluster.</span></span>
   * <span data-ttu-id="a5ae9-137">**Název clusteru**: Zadejte název hello stávajícího clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-137">**Cluster Name**: Enter hello name of an existing HDInsight cluster.</span></span>
   * <span data-ttu-id="a5ae9-138">**Okraj velikost uzlu**: vyberte jednu z hello velikosti virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-138">**Edge Node Size**: Select one of hello VM sizes.</span></span> <span data-ttu-id="a5ae9-139">Hello velikost virtuálního počítače musí splňovat požadavky na velikost virtuálního počítače hello pracovní uzly.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-139">hello vm size must meet hello worker node vm size requirements.</span></span> <span data-ttu-id="a5ae9-140">Hello nedoporučuje pracovní uzel velikosti virtuálních počítačů, najdete v části [vytvoření Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span><span class="sxs-lookup"><span data-stu-id="a5ae9-140">For hello recommended worker node vm sizes, see [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span></span>
   * <span data-ttu-id="a5ae9-141">**Okraj uzlu předpony**: hello výchozí hodnota je **nové**.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-141">**Edge Node Prefix**: hello default value is **new**.</span></span>  <span data-ttu-id="a5ae9-142">Pomocí hello výchozí hodnoty, je název uzlu edge hello **nové edgenode**.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-142">Using hello default value, hello edge node name is **new-edgenode**.</span></span>  <span data-ttu-id="a5ae9-143">Můžete upravit předponu hello z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-143">You can customize hello prefix from hello portal.</span></span> <span data-ttu-id="a5ae9-144">Můžete také upravit hello úplný název ze šablony hello.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-144">You can also customize hello full name from hello template.</span></span>

4. <span data-ttu-id="a5ae9-145">Zkontrolujte **souhlasím toohello podmínky a ujednání, které jsou uvedené výše**a potom klikněte na **nákupu** toocreate hello hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-145">Check **I agree toohello terms and conditions stated above**, and then click  **Purchase** toocreate hello edge node.</span></span>

>[!IMPORTANT]
> <span data-ttu-id="a5ae9-146">Ujistěte se, že skupina prostředků Azure pro existující cluster HDInsight hello tooselect hello.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-146">Make sure tooselect hello Azure resource group for hello existing HDInsight cluster.</span></span>  <span data-ttu-id="a5ae9-147">Jinak hodnota zobrazí hello chybová zpráva "nelze provést požadovanou operaci na vnořeného prostředku.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-147">Otherwise, you get hello error message "Can not perform requested operation on nested resource.</span></span> <span data-ttu-id="a5ae9-148">Nadřazený prostředek '&lt;ClusterName >' nebyl nalezen. "</span><span class="sxs-lookup"><span data-stu-id="a5ae9-148">Parent resource '&lt;ClusterName>' not found."</span></span>

## <a name="add-an-edge-node-when-creating-a-cluster"></a><span data-ttu-id="a5ae9-149">Přidat hraniční uzel, při vytváření clusteru</span><span class="sxs-lookup"><span data-stu-id="a5ae9-149">Add an edge node when creating a cluster</span></span>
<span data-ttu-id="a5ae9-150">V této části použijete s hraniční uzel, na cluster HDInsight toocreate šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-150">In this section, you use a Resource Manager template toocreate HDInsight cluster with an edge node.</span></span>  <span data-ttu-id="a5ae9-151">šablony Resource Manageru Hello lze nalézt v hello [galerii šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates/101-hdinsight-linux-with-edge-node/).</span><span class="sxs-lookup"><span data-stu-id="a5ae9-151">hello Resource Manager template can be found in hello [Azure QuickStart Templates gallery](https://azure.microsoft.com/documentation/templates/101-hdinsight-linux-with-edge-node/).</span></span> <span data-ttu-id="a5ae9-152">šablony Resource Manageru Hello volá akci skriptu nacházející se v https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-with-edge-node/scripts/EmptyNodeSetup.sh. skript Hello nebude provádět žádné akce.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-152">hello Resource Manager template calls a script action located at https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-with-edge-node/scripts/EmptyNodeSetup.sh. hello script doesn't perform any actions.</span></span>  <span data-ttu-id="a5ae9-153">Je toodemonstrate volání akce skriptu z šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-153">It is toodemonstrate calling script action from a Resource Manager template.</span></span>

<span data-ttu-id="a5ae9-154">**tooadd prázdný hraniční uzel tooan stávajícího clusteru**</span><span class="sxs-lookup"><span data-stu-id="a5ae9-154">**tooadd an empty edge node tooan existing cluster**</span></span>

1. <span data-ttu-id="a5ae9-155">Pokud ještě nemáte, vytvořte cluster služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-155">Create an HDInsight cluster if you don't have one yet.</span></span>  <span data-ttu-id="a5ae9-156">V tématu [začněte s Hadoop v HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a5ae9-156">See [Get started using Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
2. <span data-ttu-id="a5ae9-157">Klikněte na tlačítko hello toosign bitové kopie v tooAzure a šablony Azure Resource Manageru otevřete hello v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-157">Click hello following image toosign in tooAzure and open hello Azure Resource Manager template in hello Azure portal.</span></span> 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy tooAzure"></a>
3. <span data-ttu-id="a5ae9-158">Nakonfigurujte hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="a5ae9-158">Configure hello following properties:</span></span>
   
   * <span data-ttu-id="a5ae9-159">**Předplatné**: Vyberte předplatné Azure použitý k vytvoření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-159">**Subscription**: Select an Azure subscription used for creating hello cluster.</span></span>
   * <span data-ttu-id="a5ae9-160">**Skupina prostředků**: Vytvořte novou skupinu prostředků určené pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-160">**Resource group**: Create a new resource group used for hello cluster.</span></span>
   * <span data-ttu-id="a5ae9-161">**Umístění**: Vyberte umístění pro skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-161">**Location**: Select a location for hello resource group.</span></span>
   * <span data-ttu-id="a5ae9-162">**Název clusteru**: Zadejte název pro nový cluster toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-162">**Cluster Name**: Enter a name for hello new cluster toocreate.</span></span>
   * <span data-ttu-id="a5ae9-163">**Uživatelské jméno přihlášení clusteru**: Zadejte uživatelské jméno hello Hadoop HTTP.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-163">**Cluster Login User Name**: Enter hello Hadoop HTTP user name.</span></span>  <span data-ttu-id="a5ae9-164">Hello výchozí název je **správce**.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-164">hello default name is **admin**.</span></span>
   * <span data-ttu-id="a5ae9-165">**Heslo pro přihlášení clusteru**: Zadejte heslo uživatele hello Hadoop HTTP.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-165">**Cluster Login Password**: Enter hello Hadoop HTTP user password.</span></span>
   * <span data-ttu-id="a5ae9-166">**SSH uživatelské jméno**: Zadejte uživatelské jméno SSH hello.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-166">**Ssh User Name**: Enter hello SSH user name.</span></span> <span data-ttu-id="a5ae9-167">Hello výchozí název je **sshuser**.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-167">hello default name is **sshuser**.</span></span>
   * <span data-ttu-id="a5ae9-168">**SSH heslo**: Zadejte heslo uživatele SSH hello.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-168">**Ssh Password**: Enter hello SSH user password.</span></span>
   * <span data-ttu-id="a5ae9-169">**Instalace akce skriptu**: ponechte výchozí hodnotu hello pro procházení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-169">**Install Script Action**: Keep hello default value for going through this tutorial.</span></span>
     
     <span data-ttu-id="a5ae9-170">Některé vlastnosti byly pevně kódovaný v šabloně hello: typ clusteru, počet uzlů pracovního procesu clusteru, velikost uzlu Edge a název uzlu Edge.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-170">Some properties have been hardcoded in hello template: Cluster type, Cluster worker node count, Edge node size, and Edge node name.</span></span>
4. <span data-ttu-id="a5ae9-171">Zkontrolujte **souhlasím toohello podmínky a ujednání, které jsou uvedené výše**a potom klikněte na **nákupu** toocreate hello clusteru k uzlu edge hello.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-171">Check **I agree toohello terms and conditions stated above**, and then click  **Purchase** toocreate hello cluster with hello edge node.</span></span>

## <a name="access-an-edge-node"></a><span data-ttu-id="a5ae9-172">Přístup uzlu edge</span><span class="sxs-lookup"><span data-stu-id="a5ae9-172">Access an edge node</span></span>
<span data-ttu-id="a5ae9-173">Hello hraniční uzel ssh koncový bod je &lt;EdgeNodeName >.&lt; Název clusteru >-ssh.azurehdinsight.net:22.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-173">hello edge node ssh endpoint is &lt;EdgeNodeName>.&lt;ClusterName>-ssh.azurehdinsight.net:22.</span></span>  <span data-ttu-id="a5ae9-174">Například nové edgenode.myedgenode0914-ssh.azurehdinsight.net:22.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-174">For example, new-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.</span></span>

<span data-ttu-id="a5ae9-175">Hello hraniční uzel se objeví jako aplikace na hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-175">hello edge node appears as an application on hello Azure portal.</span></span>  <span data-ttu-id="a5ae9-176">poskytuje portálu Hello hello informace tooaccess hello okraj uzlu pomocí SSH.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-176">hello portal gives you hello information tooaccess hello edge node using SSH.</span></span>

<span data-ttu-id="a5ae9-177">**tooverify hello hraniční uzel koncový bod SSH**</span><span class="sxs-lookup"><span data-stu-id="a5ae9-177">**tooverify hello edge node SSH endpoint**</span></span>

1. <span data-ttu-id="a5ae9-178">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a5ae9-178">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="a5ae9-179">Otevřete hello HDInsight cluster s hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-179">Open hello HDInsight cluster with an edge node.</span></span>
3. <span data-ttu-id="a5ae9-180">Klikněte na tlačítko **aplikace** v okně clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-180">Click **Applications** from hello cluster blade.</span></span> <span data-ttu-id="a5ae9-181">Zobrazí se hello hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-181">You shall see hello edge node.</span></span>  <span data-ttu-id="a5ae9-182">Hello výchozí název je **nové edgenode**.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-182">hello default name is **new-edgenode**.</span></span>
4. <span data-ttu-id="a5ae9-183">Klikněte na tlačítko hello hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-183">Click hello edge node.</span></span> <span data-ttu-id="a5ae9-184">Zobrazí se koncový bod SSH hello.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-184">You shall see hello SSH endpoint.</span></span>

<span data-ttu-id="a5ae9-185">**toouse Hive v uzlu edge hello**</span><span class="sxs-lookup"><span data-stu-id="a5ae9-185">**toouse Hive on hello edge node**</span></span>

1. <span data-ttu-id="a5ae9-186">Použití SSH tooconnect toohello hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-186">Use SSH tooconnect toohello edge node.</span></span> <span data-ttu-id="a5ae9-187">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="a5ae9-187">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="a5ae9-188">Po připojení pomocí protokolu SSH toohello hraniční uzel, použijte následující příkaz tooopen hello Hive konzoly hello:</span><span class="sxs-lookup"><span data-stu-id="a5ae9-188">After you have connected toohello edge node using SSH, use hello following command tooopen hello Hive console:</span></span>
   
        hive
3. <span data-ttu-id="a5ae9-189">Spusťte následující příkaz tooshow tabulek Hive v clusteru hello hello:</span><span class="sxs-lookup"><span data-stu-id="a5ae9-189">Run hello following command tooshow Hive tables in hello cluster:</span></span>
   
        show tables;

## <a name="delete-an-edge-node"></a><span data-ttu-id="a5ae9-190">Odstranit hraniční uzel</span><span class="sxs-lookup"><span data-stu-id="a5ae9-190">Delete an edge node</span></span>
<span data-ttu-id="a5ae9-191">Hraniční uzel můžete odstranit z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-191">You can delete an edge node from hello Azure portal.</span></span>

<span data-ttu-id="a5ae9-192">**tooaccess hraniční uzel**</span><span class="sxs-lookup"><span data-stu-id="a5ae9-192">**tooaccess an edge node**</span></span>

1. <span data-ttu-id="a5ae9-193">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a5ae9-193">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="a5ae9-194">Otevřete hello HDInsight cluster s hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-194">Open hello HDInsight cluster with an edge node.</span></span>
3. <span data-ttu-id="a5ae9-195">Klikněte na tlačítko **aplikace** v okně clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-195">Click **Applications** from hello cluster blade.</span></span> <span data-ttu-id="a5ae9-196">Zobrazí se seznam uzlů okraj.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-196">You shall see a list of edge nodes.</span></span>  
4. <span data-ttu-id="a5ae9-197">Klikněte pravým tlačítkem na hello hraniční uzel toodelete a pak klikněte na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-197">Right-click hello edge node you want toodelete, and then click **Delete**.</span></span>
5. <span data-ttu-id="a5ae9-198">Klikněte na tlačítko **Ano** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-198">Click **Yes** tooconfirm.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a5ae9-199">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a5ae9-199">Next steps</span></span>
<span data-ttu-id="a5ae9-200">V tomto článku jste se naučili jak tooadd hraniční uzel a jak tooaccess hello hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-200">In this article, you have learned how tooadd an edge node and how tooaccess hello edge node.</span></span> <span data-ttu-id="a5ae9-201">toolearn více, najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="a5ae9-201">toolearn more, see hello following articles:</span></span>

* <span data-ttu-id="a5ae9-202">[Instalace aplikací HDInsight](hdinsight-apps-install-applications.md): Zjistěte, jak tooinstall tooyour aplikace HDInsight clustery.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-202">[Install HDInsight applications](hdinsight-apps-install-applications.md): Learn how tooinstall an HDInsight application tooyour clusters.</span></span>
* <span data-ttu-id="a5ae9-203">[Instalace vlastních aplikací HDInsight](hdinsight-apps-install-custom-applications.md): Zjistěte, jak toodeploy publikování aplikace tooHDInsight HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-203">[Install custom HDInsight applications](hdinsight-apps-install-custom-applications.md): learn how toodeploy an unpublished HDInsight application tooHDInsight.</span></span>
* <span data-ttu-id="a5ae9-204">[Publikování aplikací HDInsight](hdinsight-apps-publish-applications.md): Zjistěte, jak toopublish vaše vlastní tooAzure aplikace HDInsight Marketplace.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-204">[Publish HDInsight applications](hdinsight-apps-publish-applications.md): Learn how toopublish your custom HDInsight applications tooAzure Marketplace.</span></span>
* <span data-ttu-id="a5ae9-205">[MSDN: Instalace aplikace HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Zjistěte, jak toodefine aplikace HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-205">[MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx): Learn how toodefine HDInsight applications.</span></span>
* <span data-ttu-id="a5ae9-206">[Přizpůsobení clusterů HDInsight se systémem Linux pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md): Zjistěte, jak toouse akce skriptu tooinstall další aplikace.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-206">[Customize Linux-based HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md): learn how toouse Script Action tooinstall additional applications.</span></span>
* <span data-ttu-id="a5ae9-207">[Vytvořit clustery se systémem Linux Hadoop v HDInsight pomocí šablony Resource Manageru](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Zjistěte, jak toocreate šablony Resource Manageru toocall HDInsight clustery.</span><span class="sxs-lookup"><span data-stu-id="a5ae9-207">[Create Linux-based Hadoop clusters in HDInsight using Resource Manager templates](hdinsight-hadoop-create-linux-clusters-arm-templates.md): learn how toocall Resource Manager templates toocreate HDInsight clusters.</span></span>

