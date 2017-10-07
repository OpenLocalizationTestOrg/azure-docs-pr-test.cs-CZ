---
title: "aaaManage clusterů systému Hadoop pomocí rozhraní příkazového řádku Azure - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toouse hello rozhraní příkazového řádku Azure toomanage Hadoop clusterů v Azure HDInsight. Hello rozhraní příkazového řádku Azure funguje v systému Windows, Mac a Linux."
services: hdinsight
editor: cgronlun
manager: jhubbard
author: mumian
tags: azure-portal
documentationcenter: 
ms.assetid: 4f26c79f-8540-44bd-a470-84722a9e4eca
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 03b0cff9331c1c581095b80cc6d1177d843ffa83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-using-hello-azure-cli"></a><span data-ttu-id="d7491-104">Správa clusterů systému Hadoop v HDInsight pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="d7491-104">Manage Hadoop clusters in HDInsight using hello Azure CLI</span></span>
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

<span data-ttu-id="d7491-105">Zjistěte, jak toouse hello [rozhraní příkazového řádku Azure](../cli-install-nodejs.md) toomanage Hadoop clusterů v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d7491-105">Learn how toouse hello [Azure Command-line Interface](../cli-install-nodejs.md) toomanage Hadoop clusters in Azure HDInsight.</span></span> <span data-ttu-id="d7491-106">Hello rozhraní příkazového řádku Azure je implementované v Node.js.</span><span class="sxs-lookup"><span data-stu-id="d7491-106">hello Azure CLI is implemented in Node.js.</span></span> <span data-ttu-id="d7491-107">Dá se použít na jakékoli platformě, která podporuje Node.js, včetně systému Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="d7491-107">It can be used on any platform that supports Node.js, including Windows, Mac, and Linux.</span></span>

<span data-ttu-id="d7491-108">Tento článek se týká jenom používání hello rozhraní příkazového řádku Azure s HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d7491-108">This article covers only using hello Azure CLI with HDInsight.</span></span> <span data-ttu-id="d7491-109">Obecné informace o tom, toouse používáte Azure CLI, najdete v části [instalace a konfigurace rozhraní příkazového řádku Azure][azure-command-line-tools].</span><span class="sxs-lookup"><span data-stu-id="d7491-109">For a general guide on how toouse Azure CLI, see [Install and configure Azure CLI][azure-command-line-tools].</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

## <a name="prerequisites"></a><span data-ttu-id="d7491-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d7491-110">Prerequisites</span></span>
<span data-ttu-id="d7491-111">Před zahájením tohoto článku, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="d7491-111">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="d7491-112">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="d7491-112">**An Azure subscription**.</span></span> <span data-ttu-id="d7491-113">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="d7491-113">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="d7491-114">**Rozhraní příkazového řádku Azure** -najdete v části [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) informace týkající se instalace a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d7491-114">**Azure CLI** - See [Install and configure hello Azure CLI](../cli-install-nodejs.md) for installation and configuration information.</span></span>
* <span data-ttu-id="d7491-115">**Připojit tooAzure**pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d7491-115">**Connect tooAzure**, using hello following command:</span></span>
  
        azure login
  
    <span data-ttu-id="d7491-116">Další informace týkající se ověřování pomocí pracovního nebo školního účtu najdete v tématu [připojit tooan předplatného Azure z rozhraní příkazového řádku Azure hello](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="d7491-116">For more information on authenticating using a work or school account, see [Connect tooan Azure subscription from hello Azure CLI](../xplat-cli-connect.md).</span></span>
* <span data-ttu-id="d7491-117">**Přepínač režimu Azure Resource Manager toohello**pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d7491-117">**Switch toohello Azure Resource Manager mode**, using hello following command:</span></span>
  
        azure config mode arm

<span data-ttu-id="d7491-118">tooget nápovědy použijte hello **-h** přepínače.</span><span class="sxs-lookup"><span data-stu-id="d7491-118">tooget help, use hello **-h** switch.</span></span>  <span data-ttu-id="d7491-119">Například:</span><span class="sxs-lookup"><span data-stu-id="d7491-119">For example:</span></span>

    azure hdinsight cluster create -h

## <a name="create-clusters-with-hello-cli"></a><span data-ttu-id="d7491-120">Vytvoření clusterů s hello rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="d7491-120">Create clusters with hello CLI</span></span>
<span data-ttu-id="d7491-121">V tématu [hello vytvořit clusterů v HDInsight pomocí rozhraní příkazového řádku Azure](hdinsight-hadoop-create-linux-clusters-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="d7491-121">See [Create clusters in HDInsight using hello Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md).</span></span>

## <a name="list-and-show-cluster-details"></a><span data-ttu-id="d7491-122">Seznam a zobrazit podrobnosti o clusteru</span><span class="sxs-lookup"><span data-stu-id="d7491-122">List and show cluster details</span></span>
<span data-ttu-id="d7491-123">Použijte následující příkazy toolist hello a zobrazit podrobnosti o clusteru:</span><span class="sxs-lookup"><span data-stu-id="d7491-123">Use hello following commands toolist and show cluster details:</span></span>

    azure hdinsight cluster list
    azure hdinsight cluster show <Cluster Name>

<span data-ttu-id="d7491-124">![Příkazového řádku zobrazení seznamu clusteru][image-cli-clusterlisting]</span><span class="sxs-lookup"><span data-stu-id="d7491-124">![Command-line view of cluster list][image-cli-clusterlisting]</span></span>

## <a name="delete-clusters"></a><span data-ttu-id="d7491-125">Odstranění clusterů</span><span class="sxs-lookup"><span data-stu-id="d7491-125">Delete clusters</span></span>
<span data-ttu-id="d7491-126">Použijte následující příkaz toodelete cluster hello:</span><span class="sxs-lookup"><span data-stu-id="d7491-126">Use hello following command toodelete a cluster:</span></span>

    azure hdinsight cluster delete <Cluster Name>

<span data-ttu-id="d7491-127">Můžete také odstranit cluster odstraněním hello skupinu prostředků, který obsahuje hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="d7491-127">You can also delete a cluster by deleting hello resource group that contains hello cluster.</span></span> <span data-ttu-id="d7491-128">Poznámka: Tato akce odstraní všechny hello prostředky ve skupině hello včetně hello výchozí účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="d7491-128">Please note, this will delete all hello resources in hello group including hello default storage account.</span></span>

    azure group delete <Resource Group Name>

## <a name="scale-clusters"></a><span data-ttu-id="d7491-129">Škálování clusterů</span><span class="sxs-lookup"><span data-stu-id="d7491-129">Scale clusters</span></span>
<span data-ttu-id="d7491-130">toochange hello velikost clusteru Hadoop:</span><span class="sxs-lookup"><span data-stu-id="d7491-130">toochange hello Hadoop cluster size:</span></span>

    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>


## <a name="enabledisable-http-access-for-a-cluster"></a><span data-ttu-id="d7491-131">Povolí nebo zakáže přístup protokolu HTTP pro cluster</span><span class="sxs-lookup"><span data-stu-id="d7491-131">Enable/disable HTTP access for a cluster</span></span>
    azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
    azure hdinsight cluster disable-http-access [options] <Cluster Name>

## <a name="enabledisable-rdp-access-for-a-cluster"></a><span data-ttu-id="d7491-132">Povolí nebo zakáže přístup RDP pro cluster</span><span class="sxs-lookup"><span data-stu-id="d7491-132">Enable/disable RDP access for a cluster</span></span>
      azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
      azure hdinsight cluster disable-rdp-access [options] <Cluster Name>


## <a name="next-steps"></a><span data-ttu-id="d7491-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d7491-133">Next steps</span></span>
<span data-ttu-id="d7491-134">V tomto článku jste se naučili jak tooperform různé HDInsight clusteru úlohy správy.</span><span class="sxs-lookup"><span data-stu-id="d7491-134">In this article, you have learned how tooperform different HDInsight cluster administrative tasks.</span></span> <span data-ttu-id="d7491-135">toolearn více, najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="d7491-135">toolearn more, see hello following articles:</span></span>

* <span data-ttu-id="d7491-136">[Spravovat HDInsight pomocí hello portálu Azure][hdinsight-admin-portal]</span><span class="sxs-lookup"><span data-stu-id="d7491-136">[Administer HDInsight by using hello Azure Portal][hdinsight-admin-portal]</span></span>
* <span data-ttu-id="d7491-137">[Spravovat HDInsight pomocí prostředí Azure PowerShell][hdinsight-admin-powershell]</span><span class="sxs-lookup"><span data-stu-id="d7491-137">[Administer HDInsight by using Azure PowerShell][hdinsight-admin-powershell]</span></span>
* <span data-ttu-id="d7491-138">[Začínáme se službou Azure HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="d7491-138">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="d7491-139">[Jak toouse hello rozhraní příkazového řádku Azure][azure-command-line-tools]</span><span class="sxs-lookup"><span data-stu-id="d7491-139">[How toouse hello Azure CLI][azure-command-line-tools]</span></span>

[azure-command-line-tools]: ../cli-install-nodejs.md
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/command-line-list-of-clusters.png "Seznam a zobrazit clustery"
