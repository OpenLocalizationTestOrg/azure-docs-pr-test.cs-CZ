---
title: "Postup odstranění clusteru služby HDInsight - Azure | Microsoft Docs"
description: "Informace o různých způsobů, jak můžete odstranit cluster služby HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 55f7838b-9786-47ff-96db-1b64437bd0bb
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 65dac529df15d2dd43eec17673d82a2832f7692e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-cli"></a><span data-ttu-id="a2877-103">Odstranění clusteru služby HDInsight pomocí prohlížeče, prostředí PowerShell nebo rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="a2877-103">Delete an HDInsight cluster using your browser, PowerShell, or the Azure CLI</span></span>

<span data-ttu-id="a2877-104">Jakmile clusteru je vytvořen a zastaví, když odstranění clusteru, začne běžet fakturace clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a2877-104">HDInsight cluster billing starts once a cluster is created and stops when the cluster is deleted.</span></span> <span data-ttu-id="a2877-105">Fakturuje se fakturují za minutu, proto byste měli vždy odstranit cluster, když je již používán.</span><span class="sxs-lookup"><span data-stu-id="a2877-105">Billing is pro-rated per minute, so you should always delete your cluster when it is no longer in use.</span></span> <span data-ttu-id="a2877-106">V tomto dokumentu zjistěte, jak odstranit cluster pomocí portálu Azure, Azure PowerShell a Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="a2877-106">In this document, you learn how to delete a cluster using the Azure portal, Azure PowerShell, and the Azure CLI 1.0.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a2877-107">Odstranění clusteru služby HDInsight neodstraní účty Azure Storage nebo Data Lake Store, které jsou přidruženy ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="a2877-107">Deleting an HDInsight cluster does not delete the Azure Storage accounts or Data Lake Store associated with the cluster.</span></span> <span data-ttu-id="a2877-108">Data uložená v těchto služeb v budoucnu můžete znovu použít.</span><span class="sxs-lookup"><span data-stu-id="a2877-108">You can reuse data stored in those services in the future.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="a2877-109">portál Azure</span><span class="sxs-lookup"><span data-stu-id="a2877-109">Azure portal</span></span>

1. <span data-ttu-id="a2877-110">Přihlaste se k [portál Azure](https://portal.azure.com) a vyberte clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a2877-110">Log in to the [Azure portal](https://portal.azure.com) and select your HDInsight cluster.</span></span> <span data-ttu-id="a2877-111">Pokud není clusteru HDInsight připnuli k řídicímu panelu, můžete je vyhledat podle názvu pomocí pole hledání.</span><span class="sxs-lookup"><span data-stu-id="a2877-111">If your HDInsight cluster is not pinned to the dashboard, you can search for it by name using the search field.</span></span>
   
    ![hledání portálu](./media/hdinsight-delete-cluster/navbar.png)

2. <span data-ttu-id="a2877-113">Po otevření okna pro cluster, vyberte **odstranit** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a2877-113">Once the blade opens for the cluster, select the **Delete** icon.</span></span> <span data-ttu-id="a2877-114">Po zobrazení výzvy vyberte **Ano** cluster odstranit.</span><span class="sxs-lookup"><span data-stu-id="a2877-114">When prompted, select **Yes** to delete the cluster.</span></span>
   
    ![Odstranit ikonu](./media/hdinsight-delete-cluster/deletecluster.png)

## <a name="azure-powershell"></a><span data-ttu-id="a2877-116">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="a2877-116">Azure PowerShell</span></span>

<span data-ttu-id="a2877-117">Příkazovém řádku prostředí PowerShell můžete cluster odstranit následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a2877-117">From a PowerShell prompt, use the following command to delete the cluster:</span></span>

    Remove-AzureRmHDInsightCluster -ClusterName CLUSTERNAME

<span data-ttu-id="a2877-118">Nahraďte **CLUSTERNAME** názvem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a2877-118">Replace **CLUSTERNAME** with the name of your HDInsight cluster.</span></span>

## <a name="azure-cli-10"></a><span data-ttu-id="a2877-119">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="a2877-119">Azure CLI 1.0</span></span>

<span data-ttu-id="a2877-120">K odstranění clusteru z řádku, použijte následující:</span><span class="sxs-lookup"><span data-stu-id="a2877-120">From a prompt, use the following to delete the cluster:</span></span>

    azure hdinsight cluster delete CLUSTERNAME

<span data-ttu-id="a2877-121">Nahraďte **CLUSTERNAME** názvem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a2877-121">Replace **CLUSTERNAME** with the name of your HDInsight cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="a2877-122">Azure CLI 2.0 nepodporuje odstraňování clusterů HDInsight v tuto chvíli (31 července 2017).</span><span class="sxs-lookup"><span data-stu-id="a2877-122">Azure CLI 2.0 does not support deleting HDInsight clusters at this time (July 31, 2017).</span></span>