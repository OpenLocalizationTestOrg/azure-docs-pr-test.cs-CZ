---
title: "Vytváření clusterů systému Hadoop pomocí prostředí PowerShell – Azure HDInsight | Microsoft Docs"
description: "Naučte se vytvořit clustery Hadoop, HBase, Storm a Spark pro HDInsight pomocí prostředí Azure PowerShell v systému Linux."
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
ms.openlocfilehash: ca75974e6ec4f60739137d4cb5458bbfd735de3e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-linux-based-clusters-in-hdinsight-using-azure-powershell"></a><span data-ttu-id="056fb-103">Vytvořit clustery se systémem Linux v HDInsight pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="056fb-103">Create Linux-based clusters in HDInsight using Azure PowerShell</span></span>

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="056fb-104">Prostředí Azure PowerShell je výkonná skriptovací prostředí, které můžete řídit a automatizovat nasazení a správu vašich úloh ve službě Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="056fb-104">Azure PowerShell is a powerful scripting environment that you can use to control and automate the deployment and management of your workloads in Microsoft Azure.</span></span> <span data-ttu-id="056fb-105">Tento dokument obsahuje informace o tom, jak vytvořit cluster HDInsight se systémem Linux pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="056fb-105">This document provides information about how to create a Linux-based HDInsight cluster by using Azure PowerShell.</span></span> <span data-ttu-id="056fb-106">Zahrnuje také ukázkový skript.</span><span class="sxs-lookup"><span data-stu-id="056fb-106">It also includes an example script.</span></span>

> [!NOTE]
> <span data-ttu-id="056fb-107">Prostředí Azure PowerShell je dostupná pouze na klienty se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="056fb-107">Azure PowerShell is only available on Windows clients.</span></span> <span data-ttu-id="056fb-108">Pokud používáte klienta Linux, Unix nebo Mac OS X, přečtěte si téma [vytvořit cluster HDInsight se systémem Linux pomocí příkazového řádku Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) informace o používání rozhraní příkazového řádku Azure k vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="056fb-108">If you are using a Linux, Unix, or Mac OS X client, see [Create a Linux-based HDInsight cluster using Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) for information about using the Azure CLI to create a cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="056fb-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="056fb-109">Prerequisites</span></span>
<span data-ttu-id="056fb-110">Musí mít před zahájením tohoto postupu následující:</span><span class="sxs-lookup"><span data-stu-id="056fb-110">You must have the following before starting this procedure:</span></span>

* <span data-ttu-id="056fb-111">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="056fb-111">An Azure subscription.</span></span> <span data-ttu-id="056fb-112">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="056fb-112">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* [<span data-ttu-id="056fb-113">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="056fb-113">Azure PowerShell</span></span>](/powershell/azure/install-azurerm-ps)

    > [!IMPORTANT]
    > <span data-ttu-id="056fb-114">Podpora prostředí Azure PowerShell pro správu prostředků služby HDInsight pomocí Azure Service Manageru je **zastaralá** a k 1. lednu 2017 jsme ji odebrali.</span><span class="sxs-lookup"><span data-stu-id="056fb-114">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and was removed on January 1, 2017.</span></span> <span data-ttu-id="056fb-115">Kroky v tomto dokumentu používají nové rutiny služby HDInsight, které pracují s Azure Resource Managerem.</span><span class="sxs-lookup"><span data-stu-id="056fb-115">The steps in this document use the new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="056fb-116">Postupujte podle kroků v [nainstalovat Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) nainstalovat nejnovější verzi prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="056fb-116">Please follow the steps in [Install Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) to install the latest version of Azure PowerShell.</span></span> <span data-ttu-id="056fb-117">Pokud máte skripty, které je potřeba upravit tak, aby používaly nové rutiny, které pracují s nástrojem Azure Resource Manager, najdete další informace v tématu [Migrace na vývojové nástroje založené na Azure Resource Manageru pro clustery služby HDInsight](hdinsight-hadoop-development-using-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="056fb-117">If you have scripts that need to be modified to use the new cmdlets that work with Azure Resource Manager, see [Migrating to Azure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

## <a name="create-cluster"></a><span data-ttu-id="056fb-118">Vytvoření clusteru</span><span class="sxs-lookup"><span data-stu-id="056fb-118">Create cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="056fb-119">K vytvoření clusteru HDInsight pomocí prostředí Azure PowerShell, musíte provést následující postupy:</span><span class="sxs-lookup"><span data-stu-id="056fb-119">To create an HDInsight cluster by using Azure PowerShell, you must complete the following procedures:</span></span>

* <span data-ttu-id="056fb-120">Vytvoření skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="056fb-120">Create an Azure resource group</span></span>
* <span data-ttu-id="056fb-121">Vytvoření účtu služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="056fb-121">Create an Azure Storage account</span></span>
* <span data-ttu-id="056fb-122">Vytvořit kontejner objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="056fb-122">Create an Azure Blob container</span></span>
* <span data-ttu-id="056fb-123">Vytvoření clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="056fb-123">Create an HDInsight cluster</span></span>

<span data-ttu-id="056fb-124">Následující skript ukazuje postup vytvoření nového clusteru:</span><span class="sxs-lookup"><span data-stu-id="056fb-124">The following script demonstrates how to create a new cluster:</span></span>

<span data-ttu-id="056fb-125">[!code-powershell[hlavní](../../powershell_scripts/hdinsight/create-cluster/create-cluster.ps1?range=5-71)]</span><span class="sxs-lookup"><span data-stu-id="056fb-125">[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster.ps1?range=5-71)]</span></span>

<span data-ttu-id="056fb-126">Hodnoty, které zadáte pro přihlášení clusteru se používají k vytvoření uživatelského účtu Hadoop pro cluster.</span><span class="sxs-lookup"><span data-stu-id="056fb-126">The values you specify for the cluster login are used to create the Hadoop user account for the cluster.</span></span> <span data-ttu-id="056fb-127">Tento účet slouží k připojení ke službám hostovaným v clusteru, jako jsou webová uživatelská rozhraní nebo rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="056fb-127">Use this account to connect to services hosted on the cluster such as web UIs or REST APIs.</span></span>

<span data-ttu-id="056fb-128">Hodnoty, které zadáte pro uživatele SSH se používají k vytvoření uživatele SSH pro cluster.</span><span class="sxs-lookup"><span data-stu-id="056fb-128">The values you specify for the SSH user are used to create the SSH user for the cluster.</span></span> <span data-ttu-id="056fb-129">Tento účet použijte ke spuštění vzdálené relace SSH v clusteru a spouštění úloh.</span><span class="sxs-lookup"><span data-stu-id="056fb-129">Use this account to start a remote SSH session on the cluster and run jobs.</span></span> <span data-ttu-id="056fb-130">Další informace najdete v dokumentu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="056fb-130">For more information, see the [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="056fb-131">Pokud máte v plánu používat více než 32 uzlů pracovního procesu (buď při vytváření clusteru, nebo po vytvoření škálování clusteru), musíte také zadat velikost hlavního uzlu s alespoň s 8 jádry a 14 GB paměti RAM.</span><span class="sxs-lookup"><span data-stu-id="056fb-131">If you plan to use more than 32 worker nodes (either at cluster creation or by scaling the cluster after creation), you must also specify a head node size with at least 8 cores and 14 GB of RAM.</span></span>
>
> <span data-ttu-id="056fb-132">Další informace o velikosti uzlu a souvisejících nákladů, najdete v části [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="056fb-132">For more information on node sizes and associated costs, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

<span data-ttu-id="056fb-133">To může trvat až 20 minut pro vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="056fb-133">It can take up to 20 minutes to create a cluster.</span></span>

## <a name="create-cluster-configuration-object"></a><span data-ttu-id="056fb-134">Vytvoření clusteru: objekt konfigurace</span><span class="sxs-lookup"><span data-stu-id="056fb-134">Create cluster: Configuration object</span></span>

<span data-ttu-id="056fb-135">Můžete taky vytvořit objekt konfigurace HDInsight pomocí `New-AzureRmHDInsightClusterConfig` rutiny.</span><span class="sxs-lookup"><span data-stu-id="056fb-135">You can also create an HDInsight configuration object using `New-AzureRmHDInsightClusterConfig` cmdlet.</span></span> <span data-ttu-id="056fb-136">Poté můžete upravit tento objekt konfigurace povolit další možnosti konfigurace pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="056fb-136">You can then modify this configuration object to enable additional configuration options for your cluster.</span></span> <span data-ttu-id="056fb-137">Nakonec použijte `-Config` parametr `New-AzureRmHDInsightCluster` rutiny používají konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="056fb-137">Finally, use the `-Config` parameter of the `New-AzureRmHDInsightCluster` cmdlet to use the configuration.</span></span>

<span data-ttu-id="056fb-138">Tento skript vytvoří objekt konfigurace ke konfiguraci serveru R na typ clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="056fb-138">The following script creates a configuration object to configure an R Server on HDInsight cluster type.</span></span> <span data-ttu-id="056fb-139">Konfigurace umožňuje hraniční uzel, Rstudia a účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="056fb-139">The configuration enables an edge node, RStudio, and an additional storage account.</span></span>

<span data-ttu-id="056fb-140">[!code-powershell[hlavní](../../powershell_scripts/hdinsight/create-cluster/create-cluster-with-config.ps1?range=59-98)]</span><span class="sxs-lookup"><span data-stu-id="056fb-140">[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster-with-config.ps1?range=59-98)]</span></span>

> [!WARNING]
> <span data-ttu-id="056fb-141">Použití účtu úložiště v jiném umístění než HDInsight cluster není podporováno.</span><span class="sxs-lookup"><span data-stu-id="056fb-141">Using a storage account in a different location than the HDInsight cluster is not supported.</span></span> <span data-ttu-id="056fb-142">Při použití v tomto příkladu, vytvořte účet úložiště ve stejném umístění jako server.</span><span class="sxs-lookup"><span data-stu-id="056fb-142">When using this example, create the additional storage account in the same location as the server.</span></span>

## <a name="customize-clusters"></a><span data-ttu-id="056fb-143">Přizpůsobení clusterů</span><span class="sxs-lookup"><span data-stu-id="056fb-143">Customize clusters</span></span>

* <span data-ttu-id="056fb-144">V tématu [clusterů HDInsight přizpůsobit pomocí Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="056fb-144">See [Customize HDInsight clusters using Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).</span></span>
* <span data-ttu-id="056fb-145">V tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="056fb-145">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

## <a name="delete-the-cluster"></a><span data-ttu-id="056fb-146">Odstranění clusteru</span><span class="sxs-lookup"><span data-stu-id="056fb-146">Delete the cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a><span data-ttu-id="056fb-147">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="056fb-147">Troubleshoot</span></span>

<span data-ttu-id="056fb-148">Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="056fb-148">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="056fb-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="056fb-149">Next steps</span></span>

<span data-ttu-id="056fb-150">Teď, když jste úspěšně vytvořili clusteru služby HDInsight, použijte v následujících zdrojích informací se dozvíte, jak pracovat s vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="056fb-150">Now that you have successfully created an HDInsight cluster, use the following resources to learn how to work with your cluster.</span></span>

### <a name="hadoop-clusters"></a><span data-ttu-id="056fb-151">Clustery Hadoop</span><span class="sxs-lookup"><span data-stu-id="056fb-151">Hadoop clusters</span></span>

* [<span data-ttu-id="056fb-152">Použití Hivu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="056fb-152">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="056fb-153">Použití Pigu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="056fb-153">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="056fb-154">Používání nástroje MapReduce s HDInsight</span><span class="sxs-lookup"><span data-stu-id="056fb-154">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a><span data-ttu-id="056fb-155">Clustery HBase</span><span class="sxs-lookup"><span data-stu-id="056fb-155">HBase clusters</span></span>

* [<span data-ttu-id="056fb-156">Začínáme s HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="056fb-156">Get started with HBase on HDInsight</span></span>](hdinsight-hbase-tutorial-get-started-linux.md)
* [<span data-ttu-id="056fb-157">Vývoj aplikací v jazyce Java pro HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="056fb-157">Develop Java applications for HBase on HDInsight</span></span>](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a><span data-ttu-id="056fb-158">Clustery Storm</span><span class="sxs-lookup"><span data-stu-id="056fb-158">Storm clusters</span></span>

* [<span data-ttu-id="056fb-159">Vývoj topologie Java pro Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="056fb-159">Develop Java topologies for Storm on HDInsight</span></span>](hdinsight-storm-develop-java-topology.md)
* [<span data-ttu-id="056fb-160">Použití komponent, Python v Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="056fb-160">Use Python components in Storm on HDInsight</span></span>](hdinsight-storm-develop-python-topology.md)
* [<span data-ttu-id="056fb-161">Nasazení a monitorování topologie se Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="056fb-161">Deploy and monitor topologies with Storm on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a><span data-ttu-id="056fb-162">Clustery Spark</span><span class="sxs-lookup"><span data-stu-id="056fb-162">Spark clusters</span></span>

* [<span data-ttu-id="056fb-163">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="056fb-163">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="056fb-164">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="056fb-164">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)
* [<span data-ttu-id="056fb-165">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="056fb-165">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="056fb-166">Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin</span><span class="sxs-lookup"><span data-stu-id="056fb-166">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="056fb-167">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="056fb-167">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)

