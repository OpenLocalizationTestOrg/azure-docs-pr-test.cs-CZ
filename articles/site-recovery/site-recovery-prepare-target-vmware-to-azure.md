---
title: "Příprava cílového (VMware do Azure) | Microsoft Docs"
description: "Tento článek popisuje postup přípravy prostředí Azure pro zahájení replikace virtuálních počítačů VMware do Azure."
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhemraj
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 5/31/2017
ms.author: bsiva
ms.openlocfilehash: c84a775564769ddc796aa9d75add019ef1003175
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="prepare-target-vmware-to-azure"></a><span data-ttu-id="e34ee-103">Příprava cílového (VMware do Azure)</span><span class="sxs-lookup"><span data-stu-id="e34ee-103">Prepare target (VMware to Azure)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e34ee-104">Z VMware do Azure</span><span class="sxs-lookup"><span data-stu-id="e34ee-104">VMware to Azure</span></span>](./site-recovery-prepare-target-vmware-to-azure.md)
> * [<span data-ttu-id="e34ee-105">Fyzické do Azure</span><span class="sxs-lookup"><span data-stu-id="e34ee-105">Physical to Azure</span></span>](./site-recovery-prepare-target-physical-to-azure.md)

<span data-ttu-id="e34ee-106">Tento článek popisuje postup přípravy prostředí Azure pro zahájení replikace virtuálních počítačů VMware do Azure.</span><span class="sxs-lookup"><span data-stu-id="e34ee-106">This article describes how to prepare your Azure environment to start replicating VMware virtual machines to Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e34ee-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e34ee-107">Prerequisites</span></span>

<span data-ttu-id="e34ee-108">Článek předpokládá následující:</span><span class="sxs-lookup"><span data-stu-id="e34ee-108">The article assumes the following:</span></span>
- <span data-ttu-id="e34ee-109">Jste vytvořili trezor služeb zotavení k ochraně virtuálních počítačů VMware.</span><span class="sxs-lookup"><span data-stu-id="e34ee-109">You have created a Recovery Services Vault to protect your VMware virtual machines.</span></span> <span data-ttu-id="e34ee-110">Můžete vytvořit trezor služeb zotavení z [portál Azure](http://portal.azure.com "portál Azure").</span><span class="sxs-lookup"><span data-stu-id="e34ee-110">You can create a Recovery Services Vault from the [Azure portal](http://portal.azure.com "Azure portal").</span></span>
- <span data-ttu-id="e34ee-111">Máte [instalaci prostředí místní](./site-recovery-set-up-vmware-to-azure.md) k replikaci virtuálních počítačů VMware do Azure.</span><span class="sxs-lookup"><span data-stu-id="e34ee-111">You have [setup your on-premises environment](./site-recovery-set-up-vmware-to-azure.md) to replicate VMware virtual machines to Azure.</span></span>

## <a name="prepare-target"></a><span data-ttu-id="e34ee-112">Příprava cílového</span><span class="sxs-lookup"><span data-stu-id="e34ee-112">Prepare target</span></span>

<span data-ttu-id="e34ee-113">Po dokončení **cíl ochrany krok 1: vyberte** a **krok 2: Příprava zdroj**, přejdete na **krok 3: cíl**</span><span class="sxs-lookup"><span data-stu-id="e34ee-113">After completing the **Step 1:Select Protection goal** and **Step 2:Prepare Source**, you are taken to **Step 3: Target**</span></span>

![Příprava cílového](./media/site-recovery-prepare-target-vmware-to-azure/prepare-target-vmware-to-azure.png)

1. <span data-ttu-id="e34ee-115">**Předplatné:** z rozevírací nabídky vyberte odběr, který chcete replikovat virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="e34ee-115">**Subscription:** From the drop down menu, select the Subscription that you want to replicate your virtual machines to.</span></span>
2. <span data-ttu-id="e34ee-116">**Model nasazení:** vyberte model nasazení (Classic nebo Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="e34ee-116">**Deployment Model:** Select the deployment model (Classic or Resource Manager)</span></span>

<span data-ttu-id="e34ee-117">Založená na modelu vybrané nasazení, je zajistit, že máte alespoň jeden účet kompatibilní úložiště a virtuální sítě v cílové předplatné pro replikaci a převzetí služeb při selhání virtuálního počítače do spusťte ověřování.</span><span class="sxs-lookup"><span data-stu-id="e34ee-117">Based on the chosen deployment model, a validation is run to ensure that you have at least one compatible storage account and virtual network in the target subscription to replicate and failover your virtual machine to.</span></span>

<span data-ttu-id="e34ee-118">Jakmile ověřovací úspěšně dokončit, klikněte na tlačítko OK přejdete k dalšímu kroku.</span><span class="sxs-lookup"><span data-stu-id="e34ee-118">Once the validations complete successfully, click OK to go to the next step.</span></span>

<span data-ttu-id="e34ee-119">Pokud nemáte účet úložiště kompatibilní Resource Manager nebo virtuální sítě, nebo chcete přidat více, můžete provést kliknutím **+ účet úložiště** nebo **+ síť** tlačítka nahoře v okně.</span><span class="sxs-lookup"><span data-stu-id="e34ee-119">If you don't have a compatible Resource Manager storage account or virtual network, or would like to add more, you can do so by clicking the **+ Storage Account** or **+ Network** buttons on the top of the blade.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e34ee-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e34ee-120">Next steps</span></span>
<span data-ttu-id="e34ee-121">[Konfigurace nastavení replikace](./site-recovery-setup-replication-settings-vmware.md).</span><span class="sxs-lookup"><span data-stu-id="e34ee-121">[Configure replication settings](./site-recovery-setup-replication-settings-vmware.md).</span></span>
