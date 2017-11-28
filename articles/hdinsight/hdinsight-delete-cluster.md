---
title: "aaaHow toodelete clusteru služby HDInsight - Azure | Microsoft Docs"
description: "Informace o hello různé způsoby, kterými můžete odstranit cluster služby HDInsight."
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
ms.openlocfilehash: 5b9d9a09eecfdcfaed7a1f5ebab440e13bd358b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-hello-azure-cli"></a><span data-ttu-id="8b4e1-103">Odstranění clusteru služby HDInsight pomocí vašeho prohlížeče, prostředí PowerShell nebo hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="8b4e1-103">Delete an HDInsight cluster using your browser, PowerShell, or hello Azure CLI</span></span>

<span data-ttu-id="8b4e1-104">Jakmile clusteru je vytvořen a zastaví, když odstranění clusteru hello začne běžet fakturace clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8b4e1-104">HDInsight cluster billing starts once a cluster is created and stops when hello cluster is deleted.</span></span> <span data-ttu-id="8b4e1-105">Fakturuje se fakturují za minutu, proto byste měli vždy odstranit cluster, když je již používán.</span><span class="sxs-lookup"><span data-stu-id="8b4e1-105">Billing is pro-rated per minute, so you should always delete your cluster when it is no longer in use.</span></span> <span data-ttu-id="8b4e1-106">V tomto dokumentu zjistíte, jak hello toodelete clusteru pomocí portálu Azure, Azure PowerShell a hello Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="8b4e1-106">In this document, you learn how toodelete a cluster using hello Azure portal, Azure PowerShell, and hello Azure CLI 1.0.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8b4e1-107">Odstranění clusteru služby HDInsight neodstraní hello účty Azure Storage nebo Data Lake Store přidruženého k hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="8b4e1-107">Deleting an HDInsight cluster does not delete hello Azure Storage accounts or Data Lake Store associated with hello cluster.</span></span> <span data-ttu-id="8b4e1-108">Data uložená v těchto služeb v hello budoucí můžete znovu použít.</span><span class="sxs-lookup"><span data-stu-id="8b4e1-108">You can reuse data stored in those services in hello future.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="8b4e1-109">portál Azure</span><span class="sxs-lookup"><span data-stu-id="8b4e1-109">Azure portal</span></span>

1. <span data-ttu-id="8b4e1-110">Přihlaste se toohello [portál Azure](https://portal.azure.com) a vyberte clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8b4e1-110">Log in toohello [Azure portal](https://portal.azure.com) and select your HDInsight cluster.</span></span> <span data-ttu-id="8b4e1-111">Pokud váš cluster HDInsight není vázaný toohello řídicí panel, můžete je vyhledat podle názvu pomocí pole hledání hello.</span><span class="sxs-lookup"><span data-stu-id="8b4e1-111">If your HDInsight cluster is not pinned toohello dashboard, you can search for it by name using hello search field.</span></span>
   
    ![hledání portálu](./media/hdinsight-delete-cluster/navbar.png)

2. <span data-ttu-id="8b4e1-113">Po otevření okna hello hello clusteru vybrat hello **odstranit** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8b4e1-113">Once hello blade opens for hello cluster, select hello **Delete** icon.</span></span> <span data-ttu-id="8b4e1-114">Po zobrazení výzvy vyberte **Ano** toodelete hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="8b4e1-114">When prompted, select **Yes** toodelete hello cluster.</span></span>
   
    ![Odstranit ikonu](./media/hdinsight-delete-cluster/deletecluster.png)

## <a name="azure-powershell"></a><span data-ttu-id="8b4e1-116">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="8b4e1-116">Azure PowerShell</span></span>

<span data-ttu-id="8b4e1-117">Z řádku prostředí PowerShell použijte následující příkaz toodelete hello clusteru hello:</span><span class="sxs-lookup"><span data-stu-id="8b4e1-117">From a PowerShell prompt, use hello following command toodelete hello cluster:</span></span>

    Remove-AzureRmHDInsightCluster -ClusterName CLUSTERNAME

<span data-ttu-id="8b4e1-118">Nahraďte **CLUSTERNAME** s názvem hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8b4e1-118">Replace **CLUSTERNAME** with hello name of your HDInsight cluster.</span></span>

## <a name="azure-cli-10"></a><span data-ttu-id="8b4e1-119">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8b4e1-119">Azure CLI 1.0</span></span>

<span data-ttu-id="8b4e1-120">Z řádku použijte následující toodelete hello clusteru hello:</span><span class="sxs-lookup"><span data-stu-id="8b4e1-120">From a prompt, use hello following toodelete hello cluster:</span></span>

    azure hdinsight cluster delete CLUSTERNAME

<span data-ttu-id="8b4e1-121">Nahraďte **CLUSTERNAME** s názvem hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8b4e1-121">Replace **CLUSTERNAME** with hello name of your HDInsight cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="8b4e1-122">Azure CLI 2.0 nepodporuje odstraňování clusterů HDInsight v tuto chvíli (31 července 2017).</span><span class="sxs-lookup"><span data-stu-id="8b4e1-122">Azure CLI 2.0 does not support deleting HDInsight clusters at this time (July 31, 2017).</span></span>