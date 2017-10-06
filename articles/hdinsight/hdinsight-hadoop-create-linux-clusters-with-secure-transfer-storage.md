---
title: "aaaCreate Hadoop cluster s účty úložiště bezpečnému přenosu v Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak clustery HDInsight toocreate s bezpečnému přenosu povolené účty úložiště Azure."
keywords: hadoop getting started, hadoop linux, hadoop quickstart, secure transfer, azure storage account
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/21/2017
ms.author: jgao
ms.openlocfilehash: 0acb8814ad0d5d5b5652d930b2e3da90f9d7978d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-hadoop-cluster-with-secure-transfer-storage-accounts-in-azure-hdinsight"></a><span data-ttu-id="ade88-104">Vytvoření clusteru Hadoop s účty úložiště s bezpečným přenosem ve službě Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="ade88-104">Create Hadoop cluster with secure transfer storage accounts in Azure HDInsight</span></span>

<span data-ttu-id="ade88-105">Hello [zabezpečení přenosu požadované](../storage/common/storage-require-secure-transfer.md) funkce zlepšuje hello zabezpečení vašeho účtu úložiště Azure vynucením všechny požadavky tooyour účet prostřednictvím zabezpečeného připojení.</span><span class="sxs-lookup"><span data-stu-id="ade88-105">hello [Secure transfer required](../storage/common/storage-require-secure-transfer.md) feature enhances hello security of your Azure Storage account by enforcing all requests tooyour account through a secure connection.</span></span> <span data-ttu-id="ade88-106">Tato funkce a hello wasbs schématu jsou podporovány pouze verze clusteru HDInsight, 3,6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ade88-106">This feature and hello wasbs scheme are only supported by HDInsight cluster version 3.6 or newer.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="ade88-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ade88-107">Prerequisites</span></span>
<span data-ttu-id="ade88-108">Než začnete tento kurz, musíte mít:</span><span class="sxs-lookup"><span data-stu-id="ade88-108">Before you begin this tutorial, you must have:</span></span>

* <span data-ttu-id="ade88-109">**Předplatné Azure**: toocreate Bezplatný zkušební účet jeden měsíc, procházet příliš[azure.microsoft.com/free](https://azure.microsoft.com/free).</span><span class="sxs-lookup"><span data-stu-id="ade88-109">**Azure subscription**: toocreate a free one-month trial account, browse too[azure.microsoft.com/free](https://azure.microsoft.com/free).</span></span>
* <span data-ttu-id="ade88-110">**Účet služby Azure Storage s povoleným zabezpečeným přenosem**.</span><span class="sxs-lookup"><span data-stu-id="ade88-110">**An Azure Storage account with secure transfer enabled**.</span></span> <span data-ttu-id="ade88-111">Hello pokyny najdete v tématu [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) a [vyžadují zabezpečený přenos](../storage/common/storage-require-secure-transfer.md).</span><span class="sxs-lookup"><span data-stu-id="ade88-111">For hello instructions, see [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) and [Require secure transfer](../storage/common/storage-require-secure-transfer.md).</span></span>
* <span data-ttu-id="ade88-112">**Kontejner objektů Blob v účtu úložiště hello**.</span><span class="sxs-lookup"><span data-stu-id="ade88-112">**A Blob container on hello storage account**.</span></span> 
## <a name="create-cluster"></a><span data-ttu-id="ade88-113">Vytvoření clusteru</span><span class="sxs-lookup"><span data-stu-id="ade88-113">Create cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


<span data-ttu-id="ade88-114">V této části vytvoříte cluster Hadoop ve službě HDInsight pomocí [šablony Azure Resource Manageru](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="ade88-114">In this section, you create a Hadoop cluster in HDInsight using an [Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span> <span data-ttu-id="ade88-115">Šablona Hello se nachází v [Gibhubu](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-existing-default-storage-account/).</span><span class="sxs-lookup"><span data-stu-id="ade88-115">hello template is located in [Gibhub](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-existing-default-storage-account/).</span></span> <span data-ttu-id="ade88-116">Zkušenosti s šablonou Resource Manageru nejsou pro postup dle tohoto kurzu vyžadovány.</span><span class="sxs-lookup"><span data-stu-id="ade88-116">Resource Manager template experience is not required for following this tutorial.</span></span> <span data-ttu-id="ade88-117">Další metody vytváření clusterů a principy vlastnosti hello používané v tomto kurzu, najdete v tématu [Tvorba clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="ade88-117">For other cluster creation methods and understanding hello properties used in this tutorial, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

1. <span data-ttu-id="ade88-118">Klikněte na tlačítko hello toosign bitové kopie v tooAzure a otevřete hello šablony Resource Manageru v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ade88-118">Click hello following image toosign in tooAzure and open hello Resource Manager template in hello Azure portal.</span></span> 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-existing-default-storage-account%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hadoop-linux-tutorial-get-started/deploy-to-azure.png" alt="Deploy tooAzure"></a>

2. <span data-ttu-id="ade88-119">Postupujte podle hello pokyny toocreate hello clusteru s hello následující specifikace:</span><span class="sxs-lookup"><span data-stu-id="ade88-119">Follow hello instructions toocreate hello cluster with hello following specifications:</span></span> 

    - <span data-ttu-id="ade88-120">Zadejte verzi služby HDInsight 3.6.</span><span class="sxs-lookup"><span data-stu-id="ade88-120">Specify HDInsight version 3.6.</span></span>  <span data-ttu-id="ade88-121">je Hello výchozí verze 3.5.</span><span class="sxs-lookup"><span data-stu-id="ade88-121">hello default version is 3.5.</span></span> <span data-ttu-id="ade88-122">Vyžaduje se verze 3.6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ade88-122">Version 3.6 or newer is required.</span></span>
    - <span data-ttu-id="ade88-123">Zadejte účet úložiště s povoleným zabezpečeným přenosem.</span><span class="sxs-lookup"><span data-stu-id="ade88-123">Specify a secure transfer enabled storage account.</span></span>
    - <span data-ttu-id="ade88-124">Použijte krátký název pro účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="ade88-124">Use short name for hello storage account.</span></span>
    - <span data-ttu-id="ade88-125">Hello účtu úložiště a kontejneru objektů blob hello musí být vytvořen předem.</span><span class="sxs-lookup"><span data-stu-id="ade88-125">Both hello storage account and hello blob container must be created beforehand.</span></span> 

    <span data-ttu-id="ade88-126">Hello pokyny najdete v tématu [vytvořit cluster](./hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).</span><span class="sxs-lookup"><span data-stu-id="ade88-126">For hello instructions, see [Create cluster](./hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).</span></span> 

<span data-ttu-id="ade88-127">Pokud používáte skript akce tooprovide vlastní konfigurační soubory, musíte použít wasbs v hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="ade88-127">If you use script action tooprovide your own configuration files, you must use wasbs in hello following settings:</span></span>

- <span data-ttu-id="ade88-128">fs.defaultFS (základní web)</span><span class="sxs-lookup"><span data-stu-id="ade88-128">fs.defaultFS (core-site)</span></span>
- <span data-ttu-id="ade88-129">spark.eventLog.dir</span><span class="sxs-lookup"><span data-stu-id="ade88-129">spark.eventLog.dir</span></span> 
- <span data-ttu-id="ade88-130">spark.history.fs.logDirectory</span><span class="sxs-lookup"><span data-stu-id="ade88-130">spark.history.fs.logDirectory</span></span>

## <a name="add-additional-storage-accounts"></a><span data-ttu-id="ade88-131">Přidání dalších účtů úložiště</span><span class="sxs-lookup"><span data-stu-id="ade88-131">Add additional storage accounts</span></span>

<span data-ttu-id="ade88-132">Existuje několik možností tooadd další bezpečnému přenosu povoleno úložiště účtů:</span><span class="sxs-lookup"><span data-stu-id="ade88-132">There are several options tooadd additional secure transfer enabled storage accounts:</span></span>

- <span data-ttu-id="ade88-133">Úprava šablony Azure Resource Manager hello v poslední části hello.</span><span class="sxs-lookup"><span data-stu-id="ade88-133">Modify hello Azure Resource Manager template in hello last section.</span></span>
- <span data-ttu-id="ade88-134">Vytvoření clusteru s podporou pomocí hello [portál Azure](https://portal.azure.com) a zadejte propojený účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="ade88-134">Create a cluster using hello [Azure portal](https://portal.azure.com) and specify linked storage account.</span></span>
- <span data-ttu-id="ade88-135">Použití skriptu akce tooadd další zabezpečené povoleno úložiště účtů tooan stávajícího clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ade88-135">Use script action tooadd additional secure transfer enabled storage accounts tooan existing HDInsight cluster.</span></span>  <span data-ttu-id="ade88-136">Další informace najdete v tématu [přidejte další úložiště účtů tooHDInsight](hdinsight-hadoop-add-storage.md).</span><span class="sxs-lookup"><span data-stu-id="ade88-136">For more information, see [Add additional storage accounts tooHDInsight](hdinsight-hadoop-add-storage.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ade88-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ade88-137">Next steps</span></span>
<span data-ttu-id="ade88-138">V tomto kurzu jste se naučili jak toocreate clusteru služby HDInsight a povolit zabezpečený přenos toohello účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="ade88-138">In this tutorial, you have learned how toocreate an HDInsight cluster, and enable secure transfer toohello storage accounts.</span></span>

<span data-ttu-id="ade88-139">toolearn Další informace o analýze dat pomocí HDInsight, najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="ade88-139">toolearn more about analyzing data with HDInsight, see hello following articles:</span></span>

* <span data-ttu-id="ade88-140">toolearn Další informace o používání Hive s HDInsight, včetně jak dotazuje tooperform Hive ze sady Visual Studio, najdete v části [používání Hive s HDInsight][hdinsight-use-hive].</span><span class="sxs-lookup"><span data-stu-id="ade88-140">toolearn more about using Hive with HDInsight, including how tooperform Hive queries from Visual Studio, see [Use Hive with HDInsight][hdinsight-use-hive].</span></span>
* <span data-ttu-id="ade88-141">toolearn o Pig jazyk používaný datový tootransform najdete v tématu [použijte Pig s HDInsight][hdinsight-use-pig].</span><span class="sxs-lookup"><span data-stu-id="ade88-141">toolearn about Pig, a language used tootransform data, see [Use Pig with HDInsight][hdinsight-use-pig].</span></span>
* <span data-ttu-id="ade88-142">toolearn o MapReduce, způsob, jakým toowrite programy, které zpracovávají data v Hadoop, najdete v části [používání MapReduce s HDInsight][hdinsight-use-mapreduce].</span><span class="sxs-lookup"><span data-stu-id="ade88-142">toolearn about MapReduce, a way toowrite programs that process data on Hadoop, see [Use MapReduce with HDInsight][hdinsight-use-mapreduce].</span></span>
* <span data-ttu-id="ade88-143">toolearn o používání hello nástroje HDInsight pro Visual Studio tooanalyze data v HDInsight, najdete v části [začněte používat nástroje Visual Studio Hadoop pro HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ade88-143">toolearn about using hello HDInsight Tools for Visual Studio tooanalyze data on HDInsight, see [Get started using Visual Studio Hadoop tools for HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

<span data-ttu-id="ade88-144">Další informace o tom, jak HDInsight ukládá data toolearn nebo jak tooget data do HDInsight, najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="ade88-144">toolearn more about how HDInsight stores data or how tooget data into HDInsight, see hello following articles:</span></span>

* <span data-ttu-id="ade88-145">Informace o tom, jak HDInsight používá Azure Storage, najdete v tématu [Používání Azure Storage s HDInsight](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="ade88-145">For information on how HDInsight uses Azure Storage, see [Use Azure Storage with HDInsight](hdinsight-hadoop-use-blob-storage.md).</span></span>
* <span data-ttu-id="ade88-146">Informace o tom tooupload data tooHDInsight, najdete v části [nahrát data tooHDInsight][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="ade88-146">For information on how tooupload data tooHDInsight, see [Upload data tooHDInsight][hdinsight-upload-data].</span></span>

<span data-ttu-id="ade88-147">toolearn Další informace o vytváření a správě clusteru služby HDInsight najdete hello následující články:</span><span class="sxs-lookup"><span data-stu-id="ade88-147">toolearn more about creating or managing an HDInsight cluster, see hello following articles:</span></span>

* <span data-ttu-id="ade88-148">toolearn o správě clusteru HDInsight se systémem Linux, najdete v části [Správa clusterů HDInsight pomocí Ambari](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="ade88-148">toolearn about managing your Linux-based HDInsight cluster, see [Manage HDInsight clusters using Ambari](hdinsight-hadoop-manage-ambari.md).</span></span>
* <span data-ttu-id="ade88-149">toolearn Další informace o možnosti hello můžete vybrat při vytváření clusteru HDInsight, najdete v části [vytváření HDInsight v Linuxu pomocí vlastních možností](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="ade88-149">toolearn more about hello options you can select when creating an HDInsight cluster, see [Creating HDInsight on Linux using custom options](hdinsight-hadoop-provision-linux-clusters.md).</span></span>
* <span data-ttu-id="ade88-150">Pokud jste obeznámeni s Linux a Hadoop, ale chcete tooknow podrobnosti o Hadoop na hello HDInsight, přečtěte si téma [práce s HDInsight v Linuxu](hdinsight-hadoop-linux-information.md).</span><span class="sxs-lookup"><span data-stu-id="ade88-150">If you are familiar with Linux, and Hadoop, but want tooknow specifics about Hadoop on hello HDInsight, see [Working with HDInsight on Linux](hdinsight-hadoop-linux-information.md).</span></span> <span data-ttu-id="ade88-151">Tento článek obsahuje informace o:</span><span class="sxs-lookup"><span data-stu-id="ade88-151">This article provides information such as:</span></span>
  
  * <span data-ttu-id="ade88-152">Adresách URL služeb hostovaných na hello clusteru, například Ambari a WebHCat</span><span class="sxs-lookup"><span data-stu-id="ade88-152">URLs for services hosted on hello cluster, such as Ambari and WebHCat</span></span>
  * <span data-ttu-id="ade88-153">Hello umístění souborů Hadoop a příkladech v hello místního systému souborů</span><span class="sxs-lookup"><span data-stu-id="ade88-153">hello location of Hadoop files and examples on hello local file system</span></span>
  * <span data-ttu-id="ade88-154">Hello použití služby Azure Storage (WASB) namísto HDFS jako úložiště dat výchozí hello</span><span class="sxs-lookup"><span data-stu-id="ade88-154">hello use of Azure Storage (WASB) instead of HDFS as hello default data store</span></span>

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md


