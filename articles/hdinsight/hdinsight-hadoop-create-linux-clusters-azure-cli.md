---
title: "Vytváření clusterů systému Hadoop pomocí příkazového řádku-Azure HDInsight | Microsoft Docs"
description: "Informace o vytváření clusterů HDInsight pomocí rozhraní příkazového řádku Azure napříč platformami 1.0."
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
ms.openlocfilehash: 8f2fcb46789d000cd66164508f1159338dcae5f9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-hdinsight-clusters-using-the-azure-cli"></a><span data-ttu-id="b4e57-103">Vytvoření clusterů HDInsight pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="b4e57-103">Create HDInsight clusters using the Azure CLI</span></span>

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="b4e57-104">Kroky v tohoto návodu dokumentu vytváření clusteru HDInsight 3.5 pomocí Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="b4e57-104">The steps in this document walk-through creating a HDInsight 3.5 cluster using the Azure CLI 1.0.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b4e57-105">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="b4e57-105">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="b4e57-106">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="b4e57-106">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="b4e57-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b4e57-107">Prerequisites</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* <span data-ttu-id="b4e57-108">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="b4e57-108">**An Azure subscription**.</span></span> <span data-ttu-id="b4e57-109">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="b4e57-109">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

* <span data-ttu-id="b4e57-110">**Rozhraní příkazového řádku Azure**.</span><span class="sxs-lookup"><span data-stu-id="b4e57-110">**Azure CLI**.</span></span> <span data-ttu-id="b4e57-111">Kroky v tomto dokumentu byly naposledy testovány s Azure CLI verze 0.10.14.</span><span class="sxs-lookup"><span data-stu-id="b4e57-111">The steps in this document were last tested with Azure CLI version 0.10.14.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="b4e57-112">Kroky v tomto dokumentu není kompatibilní s Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="b4e57-112">The steps in this document do not work with Azure CLI 2.0.</span></span> <span data-ttu-id="b4e57-113">Azure CLI 2.0 nepodporuje vytváření clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b4e57-113">Azure CLI 2.0 does not support creating an HDInsight cluster.</span></span>

## <a name="log-in-to-your-azure-subscription"></a><span data-ttu-id="b4e57-114">Přihlášení k předplatnému Azure</span><span class="sxs-lookup"><span data-stu-id="b4e57-114">Log in to your Azure subscription</span></span>

<span data-ttu-id="b4e57-115">Postupujte podle kroků popsaných v tématu [Připojení k předplatnému Azure z rozhraní příkazového řádku Azure](../xplat-cli-connect.md) a připojte se k předplatnému pomocí metody **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="b4e57-115">Follow the steps documented in [Connect to an Azure subscription from the Azure Command-Line Interface (Azure CLI)](../xplat-cli-connect.md) and connect to your subscription using the **login** method.</span></span>

## <a name="create-a-cluster"></a><span data-ttu-id="b4e57-116">Vytvoření clusteru</span><span class="sxs-lookup"><span data-stu-id="b4e57-116">Create a cluster</span></span>

<span data-ttu-id="b4e57-117">Z příkazového řádku, jako je například PowerShell nebo Bash je třeba provést následující kroky.</span><span class="sxs-lookup"><span data-stu-id="b4e57-117">The following steps should be performed from a command line, such as PowerShell or Bash.</span></span>

1. <span data-ttu-id="b4e57-118">K ověření vašeho předplatného Azure, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b4e57-118">Use the following command to authenticate to your Azure subscription:</span></span>

        azure login

    <span data-ttu-id="b4e57-119">Zobrazí se výzva k zadání jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="b4e57-119">You are prompted to provide your name and password.</span></span> <span data-ttu-id="b4e57-120">Pokud máte víc předplatných Azure, použijte `azure account set <subscriptionname>` nastavte předplatné, které příkazy rozhraní příkazového řádku Azure používají.</span><span class="sxs-lookup"><span data-stu-id="b4e57-120">If you have multiple Azure subscriptions, use `azure account set <subscriptionname>` to set the subscription that the Azure CLI commands use.</span></span>

2. <span data-ttu-id="b4e57-121">Přepněte do režimu Azure Resource Manager pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="b4e57-121">Switch to Azure Resource Manager mode using the following command:</span></span>

        azure config mode arm

3. <span data-ttu-id="b4e57-122">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="b4e57-122">Create a resource group.</span></span> <span data-ttu-id="b4e57-123">Tato skupina prostředků obsahuje HDInsight cluster a související účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="b4e57-123">This resource group contains the HDInsight cluster and associated storage account.</span></span>

        azure group create groupname location

    * <span data-ttu-id="b4e57-124">Nahraďte `groupname` s jedinečným názvem skupiny.</span><span class="sxs-lookup"><span data-stu-id="b4e57-124">Replace `groupname` with a unique name for the group.</span></span>

    * <span data-ttu-id="b4e57-125">Nahraďte `location` s zeměpisnou oblast, kterou chcete vytvořit skupinu v.</span><span class="sxs-lookup"><span data-stu-id="b4e57-125">Replace `location` with the geographic region that you want to create the group in.</span></span>

       <span data-ttu-id="b4e57-126">Seznam platných umístění, použijte `azure location list` příkaz a pak použijte jednu z umístění z `Name` sloupce.</span><span class="sxs-lookup"><span data-stu-id="b4e57-126">For a list of valid locations, use the `azure location list` command, and then use one of the locations from the `Name` column.</span></span>

4. <span data-ttu-id="b4e57-127">Vytvoření účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="b4e57-127">Create a storage account.</span></span> <span data-ttu-id="b4e57-128">Tento účet úložiště se používá jako výchozí úložiště pro HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="b4e57-128">This storage account is used as the default storage for the HDInsight cluster.</span></span>

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename

    * <span data-ttu-id="b4e57-129">Nahraďte `groupname` s názvem skupiny vytvořené v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="b4e57-129">Replace `groupname` with the name of the group created in the previous step.</span></span>

    * <span data-ttu-id="b4e57-130">Nahraďte `location` s stejné umístění, které jsou používané v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="b4e57-130">Replace `location` with the same location used in the previous step.</span></span>

    * <span data-ttu-id="b4e57-131">Nahraďte `storagename` s jedinečným názvem pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="b4e57-131">Replace `storagename` with a unique name for the storage account.</span></span>

        > [!NOTE]
        > <span data-ttu-id="b4e57-132">Další informace o parametrech použité v tomto příkazu použít `azure storage account create -h` chcete zobrazit nápovědu pro tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="b4e57-132">For more information on the parameters used in this command, use `azure storage account create -h` to view help for this command.</span></span>

5. <span data-ttu-id="b4e57-133">Načtěte klíč používaný k přístupu k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="b4e57-133">Retrieve the key used to access the storage account.</span></span>

        azure storage account keys list -g groupname storagename

    * <span data-ttu-id="b4e57-134">Nahraďte `groupname` s názvem skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="b4e57-134">Replace `groupname` with the resource group name.</span></span>
    * <span data-ttu-id="b4e57-135">Nahraďte `storagename` s názvem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="b4e57-135">Replace `storagename` with the name of the storage account.</span></span>

     <span data-ttu-id="b4e57-136">V datech, která je vrácena, uložte `key` hodnota `key1`.</span><span class="sxs-lookup"><span data-stu-id="b4e57-136">In the data that is returned, save the `key` value for `key1`.</span></span>

6. <span data-ttu-id="b4e57-137">Vytvoření clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b4e57-137">Create an HDInsight cluster.</span></span>

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 3 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * <span data-ttu-id="b4e57-138">Nahraďte `groupname` s názvem skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="b4e57-138">Replace `groupname` with the resource group name.</span></span>

    * <span data-ttu-id="b4e57-139">Nahraďte `Hadoop` typu clusteru, který chcete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="b4e57-139">Replace `Hadoop` with the cluster type that you wish to create.</span></span> <span data-ttu-id="b4e57-140">Například `Hadoop`, `HBase`, `Kafka`, `Spark`, nebo `Storm`.</span><span class="sxs-lookup"><span data-stu-id="b4e57-140">For example, `Hadoop`, `HBase`, `Kafka`, `Spark`, or `Storm`.</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="b4e57-141">HDInsight clustery mají různé typy, které odpovídají zatížení nebo technologie, která clusteru je přizpůsobená pro.</span><span class="sxs-lookup"><span data-stu-id="b4e57-141">HDInsight clusters come in various types, which correspond to the workload or technology that the cluster is tuned for.</span></span> <span data-ttu-id="b4e57-142">Neexistuje žádná podporovaná metoda pro vytvoření clusteru, který kombinuje více typů, jako je například Storm a HBase v jednom clusteru.</span><span class="sxs-lookup"><span data-stu-id="b4e57-142">There is no supported method to create a cluster that combines multiple types, such as Storm and HBase on one cluster.</span></span>

    * <span data-ttu-id="b4e57-143">Nahraďte `location` s stejné umístění, které jsou používané v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="b4e57-143">Replace `location` with the same location used in previous steps.</span></span>

    * <span data-ttu-id="b4e57-144">Nahraďte `storagename` s názvem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="b4e57-144">Replace `storagename` with the storage account name.</span></span>

    * <span data-ttu-id="b4e57-145">Nahraďte `storagekey` klíčem získaných v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="b4e57-145">Replace `storagekey` with the key obtained in the previous step.</span></span>

    * <span data-ttu-id="b4e57-146">Pro `--defaultStorageContainer` parametr, použijte stejný název jako jste používali pro cluster.</span><span class="sxs-lookup"><span data-stu-id="b4e57-146">For the `--defaultStorageContainer` parameter, use the same name as you are using for the cluster.</span></span>

    * <span data-ttu-id="b4e57-147">Nahraďte `admin` a `httppassword` s jméno a heslo, které chcete použít při přístupu ke clusteru pomocí protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b4e57-147">Replace `admin` and `httppassword` with the name and password you wish to use when accessing the cluster through HTTPS.</span></span>

    * <span data-ttu-id="b4e57-148">Nahraďte `sshuser` a `sshuserpassword` pomocí uživatelského jména a hesla, které chcete použít při přístupu ke clusteru pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="b4e57-148">Replace `sshuser` and `sshuserpassword` with the username and password you wish to use when accessing the cluster using SSH</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="b4e57-149">Tento příklad vytvoří cluster s dvě poznámky pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="b4e57-149">This example creates a cluster with two worker notes.</span></span> <span data-ttu-id="b4e57-150">Můžete také změnit počet uzlů pracovního procesu až po vytvoření clusteru provedením operace škálování.</span><span class="sxs-lookup"><span data-stu-id="b4e57-150">You can also change the number of worker nodes after cluster creation by performing scaling operations.</span></span> <span data-ttu-id="b4e57-151">Pokud máte v úmyslu používat více než 32 uzlů pracovního procesu, je nutné vybrat velikost hlavního uzlu s alespoň s 8 jádry a 14 GB paměti RAM.</span><span class="sxs-lookup"><span data-stu-id="b4e57-151">If you plan on using more than 32 worker nodes, then you must select a head node size with at least 8 cores and 14-GB RAM.</span></span> <span data-ttu-id="b4e57-152">Velikost hlavního uzlu lze nastavit pomocí `--headNodeSize` parametru během vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="b4e57-152">You can set the head node size by using the `--headNodeSize` parameter during cluster creation.</span></span>
    >
    > <span data-ttu-id="b4e57-153">Další informace o velikosti uzlu a souvisejících nákladů, najdete v části [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="b4e57-153">For more information on node sizes and associated costs, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

    <span data-ttu-id="b4e57-154">To může trvat několik minut na dokončení procesu vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="b4e57-154">It may take several minutes for the cluster creation process to finish.</span></span> <span data-ttu-id="b4e57-155">Obvykle přibližně 15.</span><span class="sxs-lookup"><span data-stu-id="b4e57-155">Usually around 15.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="b4e57-156">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="b4e57-156">Troubleshoot</span></span>

<span data-ttu-id="b4e57-157">Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="b4e57-157">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b4e57-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b4e57-158">Next steps</span></span>

<span data-ttu-id="b4e57-159">Teď, když jste úspěšně vytvořili clusteru HDInsight pomocí rozhraní příkazového řádku Azure, použijte následující postupy se dozvíte, jak pracovat s clusteru:</span><span class="sxs-lookup"><span data-stu-id="b4e57-159">Now that you have successfully created an HDInsight cluster using the Azure CLI, use the following to learn how to work with your cluster:</span></span>

### <a name="hadoop-clusters"></a><span data-ttu-id="b4e57-160">Clustery Hadoop</span><span class="sxs-lookup"><span data-stu-id="b4e57-160">Hadoop clusters</span></span>

* [<span data-ttu-id="b4e57-161">Použití Hivu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="b4e57-161">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="b4e57-162">Použití Pigu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="b4e57-162">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="b4e57-163">Používání nástroje MapReduce s HDInsight</span><span class="sxs-lookup"><span data-stu-id="b4e57-163">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a><span data-ttu-id="b4e57-164">Clustery HBase</span><span class="sxs-lookup"><span data-stu-id="b4e57-164">HBase clusters</span></span>

* [<span data-ttu-id="b4e57-165">Začínáme s HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="b4e57-165">Get started with HBase on HDInsight</span></span>](hdinsight-hbase-tutorial-get-started-linux.md)
* [<span data-ttu-id="b4e57-166">Vývoj aplikací v jazyce Java pro HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="b4e57-166">Develop Java applications for HBase on HDInsight</span></span>](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a><span data-ttu-id="b4e57-167">Clustery Storm</span><span class="sxs-lookup"><span data-stu-id="b4e57-167">Storm clusters</span></span>

* [<span data-ttu-id="b4e57-168">Vývoj topologie Java pro Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="b4e57-168">Develop Java topologies for Storm on HDInsight</span></span>](hdinsight-storm-develop-java-topology.md)
* [<span data-ttu-id="b4e57-169">Použití komponent, Python v Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="b4e57-169">Use Python components in Storm on HDInsight</span></span>](hdinsight-storm-develop-python-topology.md)
* [<span data-ttu-id="b4e57-170">Nasazení a monitorování topologie se Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="b4e57-170">Deploy and monitor topologies with Storm on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology-linux.md)
