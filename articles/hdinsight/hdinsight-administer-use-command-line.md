---
title: "Správa clusterů systému Hadoop pomocí rozhraní příkazového řádku Azure - Azure HDInsight | Microsoft Docs"
description: "Naučte se používat rozhraní příkazového řádku pro správu clusterů systému Hadoop v prostředí Azure HDInsight. Rozhraní příkazového řádku Azure funguje v systému Windows, Mac a Linux."
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
ms.openlocfilehash: 0ee9f2f28978b207dcaf8f77950bd82a897d3fd1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-using-the-azure-cli"></a><span data-ttu-id="287c4-104">Správa clusterů systému Hadoop v HDInsight pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="287c4-104">Manage Hadoop clusters in HDInsight using the Azure CLI</span></span>
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

<span data-ttu-id="287c4-105">Další informace o použití [rozhraní příkazového řádku Azure](../cli-install-nodejs.md) ke správě clusterů systému Hadoop v prostředí Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="287c4-105">Learn how to use the [Azure Command-line Interface](../cli-install-nodejs.md) to manage Hadoop clusters in Azure HDInsight.</span></span> <span data-ttu-id="287c4-106">Rozhraní příkazového řádku Azure je implementované v Node.js.</span><span class="sxs-lookup"><span data-stu-id="287c4-106">The Azure CLI is implemented in Node.js.</span></span> <span data-ttu-id="287c4-107">Dá se použít na jakékoli platformě, která podporuje Node.js, včetně systému Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="287c4-107">It can be used on any platform that supports Node.js, including Windows, Mac, and Linux.</span></span>

<span data-ttu-id="287c4-108">Tento článek se týká jenom používání rozhraní příkazového řádku Azure s HDInsight.</span><span class="sxs-lookup"><span data-stu-id="287c4-108">This article covers only using the Azure CLI with HDInsight.</span></span> <span data-ttu-id="287c4-109">Obecné informace o tom, jak používat rozhraní příkazového řádku Azure najdete v tématu [instalace a konfigurace rozhraní příkazového řádku Azure][azure-command-line-tools].</span><span class="sxs-lookup"><span data-stu-id="287c4-109">For a general guide on how to use Azure CLI, see [Install and configure Azure CLI][azure-command-line-tools].</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

## <a name="prerequisites"></a><span data-ttu-id="287c4-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="287c4-110">Prerequisites</span></span>
<span data-ttu-id="287c4-111">Je nutné, abyste před zahájením tohoto článku měli tyto položky:</span><span class="sxs-lookup"><span data-stu-id="287c4-111">Before you begin this article, you must have the following:</span></span>

* <span data-ttu-id="287c4-112">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="287c4-112">**An Azure subscription**.</span></span> <span data-ttu-id="287c4-113">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="287c4-113">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="287c4-114">**Rozhraní příkazového řádku Azure** – Informace týkající se instalace a konfigurace najdete v tématu [Instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="287c4-114">**Azure CLI** - See [Install and configure the Azure CLI](../cli-install-nodejs.md) for installation and configuration information.</span></span>
* <span data-ttu-id="287c4-115">**Připojit k Azure**, pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="287c4-115">**Connect to Azure**, using the following command:</span></span>
  
        azure login
  
    <span data-ttu-id="287c4-116">Další informace týkající se ověřování pomocí pracovního nebo školního účtu najdete v tématu [Připojení k předplatnému Azure z rozhraní příkazového řádku Azure](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="287c4-116">For more information on authenticating using a work or school account, see [Connect to an Azure subscription from the Azure CLI](../xplat-cli-connect.md).</span></span>
* <span data-ttu-id="287c4-117">**Přepněte do režimu Azure Resource Manager**, a to pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="287c4-117">**Switch to the Azure Resource Manager mode**, using the following command:</span></span>
  
        azure config mode arm

<span data-ttu-id="287c4-118">Chcete-li získat nápovědu, použijte **-h** přepínače.</span><span class="sxs-lookup"><span data-stu-id="287c4-118">To get help, use the **-h** switch.</span></span>  <span data-ttu-id="287c4-119">Například:</span><span class="sxs-lookup"><span data-stu-id="287c4-119">For example:</span></span>

    azure hdinsight cluster create -h

## <a name="create-clusters-with-the-cli"></a><span data-ttu-id="287c4-120">Vytvoření clusterů pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="287c4-120">Create clusters with the CLI</span></span>
<span data-ttu-id="287c4-121">V tématu [Vytvoření clusterů v HDInsight pomocí rozhraní příkazového řádku Azure](hdinsight-hadoop-create-linux-clusters-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="287c4-121">See [Create clusters in HDInsight using the Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md).</span></span>

## <a name="list-and-show-cluster-details"></a><span data-ttu-id="287c4-122">Seznam a zobrazit podrobnosti o clusteru</span><span class="sxs-lookup"><span data-stu-id="287c4-122">List and show cluster details</span></span>
<span data-ttu-id="287c4-123">Použijte následující příkazy na seznamu a zobrazit podrobnosti o clusteru:</span><span class="sxs-lookup"><span data-stu-id="287c4-123">Use the following commands to list and show cluster details:</span></span>

    azure hdinsight cluster list
    azure hdinsight cluster show <Cluster Name>

<span data-ttu-id="287c4-124">![Příkazového řádku zobrazení seznamu clusteru][image-cli-clusterlisting]</span><span class="sxs-lookup"><span data-stu-id="287c4-124">![Command-line view of cluster list][image-cli-clusterlisting]</span></span>

## <a name="delete-clusters"></a><span data-ttu-id="287c4-125">Odstranění clusterů</span><span class="sxs-lookup"><span data-stu-id="287c4-125">Delete clusters</span></span>
<span data-ttu-id="287c4-126">Pokud chcete odstranit cluster použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="287c4-126">Use the following command to delete a cluster:</span></span>

    azure hdinsight cluster delete <Cluster Name>

<span data-ttu-id="287c4-127">Odstraněním skupiny prostředků, která obsahuje clusteru můžete také odstranit cluster.</span><span class="sxs-lookup"><span data-stu-id="287c4-127">You can also delete a cluster by deleting the resource group that contains the cluster.</span></span> <span data-ttu-id="287c4-128">Poznámka: Tato akce odstraní všechny prostředky ve skupině, včetně výchozí účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="287c4-128">Please note, this will delete all the resources in the group including the default storage account.</span></span>

    azure group delete <Resource Group Name>

## <a name="scale-clusters"></a><span data-ttu-id="287c4-129">Škálování clusterů</span><span class="sxs-lookup"><span data-stu-id="287c4-129">Scale clusters</span></span>
<span data-ttu-id="287c4-130">Chcete-li změnit velikost clusteru Hadoop:</span><span class="sxs-lookup"><span data-stu-id="287c4-130">To change the Hadoop cluster size:</span></span>

    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>


## <a name="enabledisable-http-access-for-a-cluster"></a><span data-ttu-id="287c4-131">Povolí nebo zakáže přístup protokolu HTTP pro cluster</span><span class="sxs-lookup"><span data-stu-id="287c4-131">Enable/disable HTTP access for a cluster</span></span>
    azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
    azure hdinsight cluster disable-http-access [options] <Cluster Name>

## <a name="enabledisable-rdp-access-for-a-cluster"></a><span data-ttu-id="287c4-132">Povolí nebo zakáže přístup RDP pro cluster</span><span class="sxs-lookup"><span data-stu-id="287c4-132">Enable/disable RDP access for a cluster</span></span>
      azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
      azure hdinsight cluster disable-rdp-access [options] <Cluster Name>


## <a name="next-steps"></a><span data-ttu-id="287c4-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="287c4-133">Next steps</span></span>
<span data-ttu-id="287c4-134">V tomto článku jste se naučili, jak provádět různé úlohy správy clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="287c4-134">In this article, you have learned how to perform different HDInsight cluster administrative tasks.</span></span> <span data-ttu-id="287c4-135">Další informace naleznete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="287c4-135">To learn more, see the following articles:</span></span>

* <span data-ttu-id="287c4-136">[Spravovat HDInsight pomocí portálu Azure][hdinsight-admin-portal]</span><span class="sxs-lookup"><span data-stu-id="287c4-136">[Administer HDInsight by using the Azure Portal][hdinsight-admin-portal]</span></span>
* <span data-ttu-id="287c4-137">[Spravovat HDInsight pomocí prostředí Azure PowerShell][hdinsight-admin-powershell]</span><span class="sxs-lookup"><span data-stu-id="287c4-137">[Administer HDInsight by using Azure PowerShell][hdinsight-admin-powershell]</span></span>
* <span data-ttu-id="287c4-138">[Začínáme se službou Azure HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="287c4-138">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="287c4-139">[Jak používat rozhraní příkazového řádku Azure][azure-command-line-tools]</span><span class="sxs-lookup"><span data-stu-id="287c4-139">[How to use the Azure CLI][azure-command-line-tools]</span></span>

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
