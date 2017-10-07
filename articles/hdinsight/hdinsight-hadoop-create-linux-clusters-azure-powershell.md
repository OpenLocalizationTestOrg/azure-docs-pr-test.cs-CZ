---
title: "aaaCreate clusterů systému Hadoop pomocí prostředí PowerShell – Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak se toocreate Hadoop, HBase, Storm a Spark clustery v Linuxu pro HDInsight pomocí prostředí Azure PowerShell."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 4208deca-d64a-45e1-8948-2673d5d7678c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 53afe4702f6b61a0720ceda48a4a34d7fa8797d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-linux-based-clusters-in-hdinsight-using-azure-powershell"></a><span data-ttu-id="c14ea-103">Vytvořit clustery se systémem Linux v HDInsight pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c14ea-103">Create Linux-based clusters in HDInsight using Azure PowerShell</span></span>

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="c14ea-104">Prostředí Azure PowerShell je výkonná skriptovací prostředí, můžete použít toocontrol a automatizovat hello nasazení a správy vašich úloh ve službě Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c14ea-104">Azure PowerShell is a powerful scripting environment that you can use toocontrol and automate hello deployment and management of your workloads in Microsoft Azure.</span></span> <span data-ttu-id="c14ea-105">Tento dokument obsahuje informace o tom, jak toocreate HDInsight se systémem Linux clusteru pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c14ea-105">This document provides information about how toocreate a Linux-based HDInsight cluster by using Azure PowerShell.</span></span> <span data-ttu-id="c14ea-106">Zahrnuje také ukázkový skript.</span><span class="sxs-lookup"><span data-stu-id="c14ea-106">It also includes an example script.</span></span>

> [!NOTE]
> <span data-ttu-id="c14ea-107">Prostředí Azure PowerShell je dostupná pouze na klienty se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="c14ea-107">Azure PowerShell is only available on Windows clients.</span></span> <span data-ttu-id="c14ea-108">Pokud používáte klienta Linux, Unix nebo Mac OS X, přečtěte si téma [vytvořit cluster HDInsight se systémem Linux pomocí příkazového řádku Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) informace o používání rozhraní příkazového řádku Azure toocreate hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="c14ea-108">If you are using a Linux, Unix, or Mac OS X client, see [Create a Linux-based HDInsight cluster using Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) for information about using hello Azure CLI toocreate a cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c14ea-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c14ea-109">Prerequisites</span></span>
<span data-ttu-id="c14ea-110">Musí mít před zahájením tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="c14ea-110">You must have hello following before starting this procedure:</span></span>

* <span data-ttu-id="c14ea-111">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="c14ea-111">An Azure subscription.</span></span> <span data-ttu-id="c14ea-112">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="c14ea-112">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* [<span data-ttu-id="c14ea-113">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c14ea-113">Azure PowerShell</span></span>](/powershell/azure/install-azurerm-ps)

    > [!IMPORTANT]
    > <span data-ttu-id="c14ea-114">Podpora prostředí Azure PowerShell pro správu prostředků služby HDInsight pomocí Azure Service Manageru je **zastaralá** a k 1. lednu 2017 jsme ji odebrali.</span><span class="sxs-lookup"><span data-stu-id="c14ea-114">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and was removed on January 1, 2017.</span></span> <span data-ttu-id="c14ea-115">kroky Hello, v tento dokument použít hello nové rutiny služby HDInsight, které fungují s Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c14ea-115">hello steps in this document use hello new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="c14ea-116">Postupujte podle kroků hello v [nainstalovat Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) tooinstall hello nejnovější verzi prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c14ea-116">Please follow hello steps in [Install Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) tooinstall hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="c14ea-117">Pokud máte skripty, že toobe potřeba upravit hello toouse nové se rutiny, které pracují s Azure Resource Managerem najdete v tématu [tooAzure migrace založené na správci prostředků vývoj nástroje pro clustery služby HDInsight](hdinsight-hadoop-development-using-azure-resource-manager.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="c14ea-117">If you have scripts that need toobe modified toouse hello new cmdlets that work with Azure Resource Manager, see [Migrating tooAzure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

## <a name="create-cluster"></a><span data-ttu-id="c14ea-118">Vytvoření clusteru</span><span class="sxs-lookup"><span data-stu-id="c14ea-118">Create cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="c14ea-119">toocreate clusteru HDInsight pomocí prostředí Azure PowerShell, je třeba provést následující postupy hello:</span><span class="sxs-lookup"><span data-stu-id="c14ea-119">toocreate an HDInsight cluster by using Azure PowerShell, you must complete hello following procedures:</span></span>

* <span data-ttu-id="c14ea-120">Vytvoření skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="c14ea-120">Create an Azure resource group</span></span>
* <span data-ttu-id="c14ea-121">Vytvoření účtu služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="c14ea-121">Create an Azure Storage account</span></span>
* <span data-ttu-id="c14ea-122">Vytvořit kontejner objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="c14ea-122">Create an Azure Blob container</span></span>
* <span data-ttu-id="c14ea-123">Vytvoření clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="c14ea-123">Create an HDInsight cluster</span></span>

<span data-ttu-id="c14ea-124">ukazuje, Hello následující skript jak toocreate do nového clusteru:</span><span class="sxs-lookup"><span data-stu-id="c14ea-124">hello following script demonstrates how toocreate a new cluster:</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster.ps1?range=5-71)]

<span data-ttu-id="c14ea-125">Hello hodnoty, které zadáte pro přihlášení hello clusteru jsou použité toocreate hello uživatelský účet Hadoop pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="c14ea-125">hello values you specify for hello cluster login are used toocreate hello Hadoop user account for hello cluster.</span></span> <span data-ttu-id="c14ea-126">Použijte tento účet tooconnect tooservices hostovaná v clusteru hello jako jsou webová uživatelská rozhraní nebo rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="c14ea-126">Use this account tooconnect tooservices hosted on hello cluster such as web UIs or REST APIs.</span></span>

<span data-ttu-id="c14ea-127">Hello hodnoty, které zadáte pro uživatele SSH hello jsou použité toocreate hello SSH uživatele pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="c14ea-127">hello values you specify for hello SSH user are used toocreate hello SSH user for hello cluster.</span></span> <span data-ttu-id="c14ea-128">Použít účet toostart a vzdálené relace SSH na hello clusteru a spouštění úloh.</span><span class="sxs-lookup"><span data-stu-id="c14ea-128">Use this account toostart a remote SSH session on hello cluster and run jobs.</span></span> <span data-ttu-id="c14ea-129">Další informace najdete v tématu hello [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="c14ea-129">For more information, see hello [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c14ea-130">Pokud máte v plánu toouse víc než 32 uzlů pracovního procesu (buď při vytváření clusteru, nebo škálování hello clusteru po vytvoření), musíte také zadat velikost hlavního uzlu s alespoň s 8 jádry a 14 GB paměti RAM.</span><span class="sxs-lookup"><span data-stu-id="c14ea-130">If you plan toouse more than 32 worker nodes (either at cluster creation or by scaling hello cluster after creation), you must also specify a head node size with at least 8 cores and 14 GB of RAM.</span></span>
>
> <span data-ttu-id="c14ea-131">Další informace o velikosti uzlu a souvisejících nákladů, najdete v části [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="c14ea-131">For more information on node sizes and associated costs, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

<span data-ttu-id="c14ea-132">Může trvat až minut toocreate too20 clusteru.</span><span class="sxs-lookup"><span data-stu-id="c14ea-132">It can take up too20 minutes toocreate a cluster.</span></span>

## <a name="create-cluster-configuration-object"></a><span data-ttu-id="c14ea-133">Vytvoření clusteru: objekt konfigurace</span><span class="sxs-lookup"><span data-stu-id="c14ea-133">Create cluster: Configuration object</span></span>

<span data-ttu-id="c14ea-134">Můžete taky vytvořit objekt konfigurace HDInsight pomocí `New-AzureRmHDInsightClusterConfig` rutiny.</span><span class="sxs-lookup"><span data-stu-id="c14ea-134">You can also create an HDInsight configuration object using `New-AzureRmHDInsightClusterConfig` cmdlet.</span></span> <span data-ttu-id="c14ea-135">Poté můžete upravit tato konfigurace objektu tooenable další možnosti konfigurace pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="c14ea-135">You can then modify this configuration object tooenable additional configuration options for your cluster.</span></span> <span data-ttu-id="c14ea-136">Nakonec použijte hello `-Config` parametr hello `New-AzureRmHDInsightCluster` rutiny toouse hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c14ea-136">Finally, use hello `-Config` parameter of hello `New-AzureRmHDInsightCluster` cmdlet toouse hello configuration.</span></span>

<span data-ttu-id="c14ea-137">Hello následující skript vytvoří objekt tooconfigure konfigurace serveru R na typ clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c14ea-137">hello following script creates a configuration object tooconfigure an R Server on HDInsight cluster type.</span></span> <span data-ttu-id="c14ea-138">Konfigurace Hello umožňuje hraniční uzel, Rstudia a účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="c14ea-138">hello configuration enables an edge node, RStudio, and an additional storage account.</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster-with-config.ps1?range=59-98)]

> [!WARNING]
> <span data-ttu-id="c14ea-139">Použití účtu úložiště v jiném umístění než hello HDInsight cluster není podporováno.</span><span class="sxs-lookup"><span data-stu-id="c14ea-139">Using a storage account in a different location than hello HDInsight cluster is not supported.</span></span> <span data-ttu-id="c14ea-140">Při použití v tomto příkladu, vytvořit účet úložiště hello hello stejné umístění jako hello server.</span><span class="sxs-lookup"><span data-stu-id="c14ea-140">When using this example, create hello additional storage account in hello same location as hello server.</span></span>

## <a name="customize-clusters"></a><span data-ttu-id="c14ea-141">Přizpůsobení clusterů</span><span class="sxs-lookup"><span data-stu-id="c14ea-141">Customize clusters</span></span>

* <span data-ttu-id="c14ea-142">V tématu [clusterů HDInsight přizpůsobit pomocí Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="c14ea-142">See [Customize HDInsight clusters using Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).</span></span>
* <span data-ttu-id="c14ea-143">V tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="c14ea-143">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

## <a name="delete-hello-cluster"></a><span data-ttu-id="c14ea-144">Odstranění clusteru hello</span><span class="sxs-lookup"><span data-stu-id="c14ea-144">Delete hello cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a><span data-ttu-id="c14ea-145">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="c14ea-145">Troubleshoot</span></span>

<span data-ttu-id="c14ea-146">Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="c14ea-146">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c14ea-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c14ea-147">Next steps</span></span>

<span data-ttu-id="c14ea-148">Teď, když jste úspěšně vytvořili clusteru služby HDInsight, použijte následující prostředky toolearn jak hello toowork k vašemu clusteru.</span><span class="sxs-lookup"><span data-stu-id="c14ea-148">Now that you have successfully created an HDInsight cluster, use hello following resources toolearn how toowork with your cluster.</span></span>

### <a name="hadoop-clusters"></a><span data-ttu-id="c14ea-149">Clustery Hadoop</span><span class="sxs-lookup"><span data-stu-id="c14ea-149">Hadoop clusters</span></span>

* [<span data-ttu-id="c14ea-150">Použití Hivu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="c14ea-150">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="c14ea-151">Použití Pigu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="c14ea-151">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="c14ea-152">Používání nástroje MapReduce s HDInsight</span><span class="sxs-lookup"><span data-stu-id="c14ea-152">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a><span data-ttu-id="c14ea-153">Clustery HBase</span><span class="sxs-lookup"><span data-stu-id="c14ea-153">HBase clusters</span></span>

* [<span data-ttu-id="c14ea-154">Začínáme s HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="c14ea-154">Get started with HBase on HDInsight</span></span>](hdinsight-hbase-tutorial-get-started-linux.md)
* [<span data-ttu-id="c14ea-155">Vývoj aplikací v jazyce Java pro HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="c14ea-155">Develop Java applications for HBase on HDInsight</span></span>](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a><span data-ttu-id="c14ea-156">Clustery Storm</span><span class="sxs-lookup"><span data-stu-id="c14ea-156">Storm clusters</span></span>

* [<span data-ttu-id="c14ea-157">Vývoj topologie Java pro Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="c14ea-157">Develop Java topologies for Storm on HDInsight</span></span>](hdinsight-storm-develop-java-topology.md)
* [<span data-ttu-id="c14ea-158">Použití komponent, Python v Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="c14ea-158">Use Python components in Storm on HDInsight</span></span>](hdinsight-storm-develop-python-topology.md)
* [<span data-ttu-id="c14ea-159">Nasazení a monitorování topologie se Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="c14ea-159">Deploy and monitor topologies with Storm on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a><span data-ttu-id="c14ea-160">Clustery Spark</span><span class="sxs-lookup"><span data-stu-id="c14ea-160">Spark clusters</span></span>

* [<span data-ttu-id="c14ea-161">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="c14ea-161">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="c14ea-162">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="c14ea-162">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)
* [<span data-ttu-id="c14ea-163">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="c14ea-163">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="c14ea-164">Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight</span><span class="sxs-lookup"><span data-stu-id="c14ea-164">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="c14ea-165">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="c14ea-165">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)

