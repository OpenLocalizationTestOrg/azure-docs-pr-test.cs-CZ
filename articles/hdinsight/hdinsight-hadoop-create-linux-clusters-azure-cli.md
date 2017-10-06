---
title: "aaaCreate clusterů systému Hadoop pomocí hello příkazového řádku - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toocreate clusterů HDInsight pomocí hello a platformy Azure CLI 1.0."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 50b01483-455c-4d87-b754-2229005a8ab9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 5295b01054b8c23df0e3b75a3e0e8c933ac48b3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-hdinsight-clusters-using-hello-azure-cli"></a><span data-ttu-id="825d5-103">Vytvoření clusterů HDInsight pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="825d5-103">Create HDInsight clusters using hello Azure CLI</span></span>

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="825d5-104">Hello kroky tohoto návodu dokumentu vytváření clusteru HDInsight 3.5 pomocí hello Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="825d5-104">hello steps in this document walk-through creating a HDInsight 3.5 cluster using hello Azure CLI 1.0.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="825d5-105">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="825d5-105">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="825d5-106">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="825d5-106">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="825d5-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="825d5-107">Prerequisites</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* <span data-ttu-id="825d5-108">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="825d5-108">**An Azure subscription**.</span></span> <span data-ttu-id="825d5-109">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="825d5-109">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

* <span data-ttu-id="825d5-110">**Azure CLI**.</span><span class="sxs-lookup"><span data-stu-id="825d5-110">**Azure CLI**.</span></span> <span data-ttu-id="825d5-111">pomocí Azure CLI verze 0.10.14 poslední testovali Hello kroky v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="825d5-111">hello steps in this document were last tested with Azure CLI version 0.10.14.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="825d5-112">pomocí Azure CLI 2.0 nefungují Hello kroky v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="825d5-112">hello steps in this document do not work with Azure CLI 2.0.</span></span> <span data-ttu-id="825d5-113">Azure CLI 2.0 nepodporuje vytváření clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="825d5-113">Azure CLI 2.0 does not support creating an HDInsight cluster.</span></span>

## <a name="log-in-tooyour-azure-subscription"></a><span data-ttu-id="825d5-114">Přihlaste se tooyour předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="825d5-114">Log in tooyour Azure subscription</span></span>

<span data-ttu-id="825d5-115">Postupujte podle kroků hello popsaných v [připojit tooan předplatného Azure z rozhraní příkazového řádku Azure (Azure CLI) hello](../xplat-cli-connect.md) a připojte se pomocí hello předplatné tooyour **přihlášení** metoda.</span><span class="sxs-lookup"><span data-stu-id="825d5-115">Follow hello steps documented in [Connect tooan Azure subscription from hello Azure Command-Line Interface (Azure CLI)](../xplat-cli-connect.md) and connect tooyour subscription using hello **login** method.</span></span>

## <a name="create-a-cluster"></a><span data-ttu-id="825d5-116">Vytvoření clusteru</span><span class="sxs-lookup"><span data-stu-id="825d5-116">Create a cluster</span></span>

<span data-ttu-id="825d5-117">z příkazového řádku, jako je například PowerShell nebo Bash je třeba provést následující kroky Hello.</span><span class="sxs-lookup"><span data-stu-id="825d5-117">hello following steps should be performed from a command line, such as PowerShell or Bash.</span></span>

1. <span data-ttu-id="825d5-118">Použijte následující příkaz tooauthenticate tooyour předplatného Azure hello:</span><span class="sxs-lookup"><span data-stu-id="825d5-118">Use hello following command tooauthenticate tooyour Azure subscription:</span></span>

        azure login

    <span data-ttu-id="825d5-119">Můžete se výzvami tooprovide své jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="825d5-119">You are prompted tooprovide your name and password.</span></span> <span data-ttu-id="825d5-120">Pokud máte víc předplatných Azure, použijte `azure account set <subscriptionname>` použijte příkazy tooset hello předplatné, které hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="825d5-120">If you have multiple Azure subscriptions, use `azure account set <subscriptionname>` tooset hello subscription that hello Azure CLI commands use.</span></span>

2. <span data-ttu-id="825d5-121">Přepnutí režimu Resource Manager tooAzure pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="825d5-121">Switch tooAzure Resource Manager mode using hello following command:</span></span>

        azure config mode arm

3. <span data-ttu-id="825d5-122">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="825d5-122">Create a resource group.</span></span> <span data-ttu-id="825d5-123">Tato skupina prostředků obsahuje hello HDInsight cluster a související účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="825d5-123">This resource group contains hello HDInsight cluster and associated storage account.</span></span>

        azure group create groupname location

    * <span data-ttu-id="825d5-124">Nahraďte `groupname` s jedinečným názvem skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="825d5-124">Replace `groupname` with a unique name for hello group.</span></span>

    * <span data-ttu-id="825d5-125">Nahraďte `location` s hello zeměpisnou oblast, která chcete v toocreate hello skupiny.</span><span class="sxs-lookup"><span data-stu-id="825d5-125">Replace `location` with hello geographic region that you want toocreate hello group in.</span></span>

       <span data-ttu-id="825d5-126">Seznam platných umístění, použijte hello `azure location list` příkaz a potom použijte jednu z umístění hello z hello `Name` sloupce.</span><span class="sxs-lookup"><span data-stu-id="825d5-126">For a list of valid locations, use hello `azure location list` command, and then use one of hello locations from hello `Name` column.</span></span>

4. <span data-ttu-id="825d5-127">Vytvoření účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="825d5-127">Create a storage account.</span></span> <span data-ttu-id="825d5-128">Tento účet úložiště se používá jako hello výchozí úložiště pro hello HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="825d5-128">This storage account is used as hello default storage for hello HDInsight cluster.</span></span>

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename

    * <span data-ttu-id="825d5-129">Nahraďte `groupname` hello názvem skupiny hello vytvořili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="825d5-129">Replace `groupname` with hello name of hello group created in hello previous step.</span></span>

    * <span data-ttu-id="825d5-130">Nahraďte `location` s hello používá stejné umístění v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="825d5-130">Replace `location` with hello same location used in hello previous step.</span></span>

    * <span data-ttu-id="825d5-131">Nahraďte `storagename` s jedinečným názvem pro účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="825d5-131">Replace `storagename` with a unique name for hello storage account.</span></span>

        > [!NOTE]
        > <span data-ttu-id="825d5-132">Další informace o hello parametry použité v tomto příkazu použít `azure storage account create -h` tooview nápovědu pro tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="825d5-132">For more information on hello parameters used in this command, use `azure storage account create -h` tooview help for this command.</span></span>

5. <span data-ttu-id="825d5-133">Načtení hello klíč používá účet úložiště tooaccess hello.</span><span class="sxs-lookup"><span data-stu-id="825d5-133">Retrieve hello key used tooaccess hello storage account.</span></span>

        azure storage account keys list -g groupname storagename

    * <span data-ttu-id="825d5-134">Nahraďte `groupname` s názvem skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="825d5-134">Replace `groupname` with hello resource group name.</span></span>
    * <span data-ttu-id="825d5-135">Nahraďte `storagename` hello název účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="825d5-135">Replace `storagename` with hello name of hello storage account.</span></span>

     <span data-ttu-id="825d5-136">V hello data, která je vrácena, uložte hello `key` hodnota `key1`.</span><span class="sxs-lookup"><span data-stu-id="825d5-136">In hello data that is returned, save hello `key` value for `key1`.</span></span>

6. <span data-ttu-id="825d5-137">Vytvoření clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="825d5-137">Create an HDInsight cluster.</span></span>

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 3 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * <span data-ttu-id="825d5-138">Nahraďte `groupname` s názvem skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="825d5-138">Replace `groupname` with hello resource group name.</span></span>

    * <span data-ttu-id="825d5-139">Nahraďte `Hadoop` s typem clusteru hello chcete toocreate.</span><span class="sxs-lookup"><span data-stu-id="825d5-139">Replace `Hadoop` with hello cluster type that you wish toocreate.</span></span> <span data-ttu-id="825d5-140">Například `Hadoop`, `HBase`, `Kafka`, `Spark`, nebo `Storm`.</span><span class="sxs-lookup"><span data-stu-id="825d5-140">For example, `Hadoop`, `HBase`, `Kafka`, `Spark`, or `Storm`.</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="825d5-141">HDInsight clustery jsou v různých typů, které odpovídají toohello zatížení a technologie, která hello clusteru je přizpůsobená pro.</span><span class="sxs-lookup"><span data-stu-id="825d5-141">HDInsight clusters come in various types, which correspond toohello workload or technology that hello cluster is tuned for.</span></span> <span data-ttu-id="825d5-142">Neexistuje žádná podporovaná metoda toocreate clusteru, který kombinuje více typů, jako je například Storm a HBase v jednom clusteru.</span><span class="sxs-lookup"><span data-stu-id="825d5-142">There is no supported method toocreate a cluster that combines multiple types, such as Storm and HBase on one cluster.</span></span>

    * <span data-ttu-id="825d5-143">Nahraďte `location` s hello používá stejné umístění v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="825d5-143">Replace `location` with hello same location used in previous steps.</span></span>

    * <span data-ttu-id="825d5-144">Nahraďte `storagename` hello názvem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="825d5-144">Replace `storagename` with hello storage account name.</span></span>

    * <span data-ttu-id="825d5-145">Nahraďte `storagekey` klíčem hello získaných v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="825d5-145">Replace `storagekey` with hello key obtained in hello previous step.</span></span>

    * <span data-ttu-id="825d5-146">Pro hello `--defaultStorageContainer` parametr, hello použijte stejný název jako používáte pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="825d5-146">For hello `--defaultStorageContainer` parameter, use hello same name as you are using for hello cluster.</span></span>

    * <span data-ttu-id="825d5-147">Nahraďte `admin` a `httppassword` hello jméno a heslo chcete toouse při přístupu k hello clusteru prostřednictvím protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="825d5-147">Replace `admin` and `httppassword` with hello name and password you wish toouse when accessing hello cluster through HTTPS.</span></span>

    * <span data-ttu-id="825d5-148">Nahraďte `sshuser` a `sshuserpassword` s hello uživatelského jména a hesla nechcete toouse při přístupu k hello clusteru pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="825d5-148">Replace `sshuser` and `sshuserpassword` with hello username and password you wish toouse when accessing hello cluster using SSH</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="825d5-149">Tento příklad vytvoří cluster s dvě poznámky pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="825d5-149">This example creates a cluster with two worker notes.</span></span> <span data-ttu-id="825d5-150">Hello počet uzlů pracovního procesu můžete také změnit po vytvoření clusteru provedením operace škálování.</span><span class="sxs-lookup"><span data-stu-id="825d5-150">You can also change hello number of worker nodes after cluster creation by performing scaling operations.</span></span> <span data-ttu-id="825d5-151">Pokud máte v úmyslu používat více než 32 uzlů pracovního procesu, je nutné vybrat velikost hlavního uzlu s alespoň s 8 jádry a 14 GB paměti RAM.</span><span class="sxs-lookup"><span data-stu-id="825d5-151">If you plan on using more than 32 worker nodes, then you must select a head node size with at least 8 cores and 14-GB RAM.</span></span> <span data-ttu-id="825d5-152">Velikost hlavního uzlu hello lze nastavit pomocí hello `--headNodeSize` parametru během vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="825d5-152">You can set hello head node size by using hello `--headNodeSize` parameter during cluster creation.</span></span>
    >
    > <span data-ttu-id="825d5-153">Další informace o velikosti uzlu a souvisejících nákladů, najdete v části [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="825d5-153">For more information on node sizes and associated costs, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

    <span data-ttu-id="825d5-154">To může trvat několik minut, než toofinish procesu vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="825d5-154">It may take several minutes for hello cluster creation process toofinish.</span></span> <span data-ttu-id="825d5-155">Obvykle přibližně 15.</span><span class="sxs-lookup"><span data-stu-id="825d5-155">Usually around 15.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="825d5-156">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="825d5-156">Troubleshoot</span></span>

<span data-ttu-id="825d5-157">Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="825d5-157">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="825d5-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="825d5-158">Next steps</span></span>

<span data-ttu-id="825d5-159">Teď, když jste úspěšně vytvořili clusteru HDInsight pomocí hello rozhraní příkazového řádku Azure, použijte následující toolearn jak hello toowork k vašemu clusteru:</span><span class="sxs-lookup"><span data-stu-id="825d5-159">Now that you have successfully created an HDInsight cluster using hello Azure CLI, use hello following toolearn how toowork with your cluster:</span></span>

### <a name="hadoop-clusters"></a><span data-ttu-id="825d5-160">Clustery Hadoop</span><span class="sxs-lookup"><span data-stu-id="825d5-160">Hadoop clusters</span></span>

* [<span data-ttu-id="825d5-161">Použití Hivu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="825d5-161">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="825d5-162">Použití Pigu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="825d5-162">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="825d5-163">Používání nástroje MapReduce s HDInsight</span><span class="sxs-lookup"><span data-stu-id="825d5-163">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a><span data-ttu-id="825d5-164">Clustery HBase</span><span class="sxs-lookup"><span data-stu-id="825d5-164">HBase clusters</span></span>

* [<span data-ttu-id="825d5-165">Začínáme s HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="825d5-165">Get started with HBase on HDInsight</span></span>](hdinsight-hbase-tutorial-get-started-linux.md)
* [<span data-ttu-id="825d5-166">Vývoj aplikací v jazyce Java pro HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="825d5-166">Develop Java applications for HBase on HDInsight</span></span>](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a><span data-ttu-id="825d5-167">Clustery Storm</span><span class="sxs-lookup"><span data-stu-id="825d5-167">Storm clusters</span></span>

* [<span data-ttu-id="825d5-168">Vývoj topologie Java pro Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="825d5-168">Develop Java topologies for Storm on HDInsight</span></span>](hdinsight-storm-develop-java-topology.md)
* [<span data-ttu-id="825d5-169">Použití komponent, Python v Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="825d5-169">Use Python components in Storm on HDInsight</span></span>](hdinsight-storm-develop-python-topology.md)
* [<span data-ttu-id="825d5-170">Nasazení a monitorování topologie se Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="825d5-170">Deploy and monitor topologies with Storm on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology-linux.md)
