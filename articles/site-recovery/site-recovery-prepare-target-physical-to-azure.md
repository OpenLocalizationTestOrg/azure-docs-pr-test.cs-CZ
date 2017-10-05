---
title: "Příprava cílového (fyzické do Azure) | Microsoft Docs"
description: "Tento článek popisuje postup přípravy prostředí Azure pro zahájení replikace fyzických serverů s Windows nebo Linuxem do Azure."
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
ms.openlocfilehash: aa7a32ace8354f615a8b8cc137f6bdf48fbadf48
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="prepare-target-vmware-to-azure"></a><span data-ttu-id="4d94a-103">Příprava cílového (VMware do Azure)</span><span class="sxs-lookup"><span data-stu-id="4d94a-103">Prepare target (VMware to Azure)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4d94a-104">Z VMware do Azure</span><span class="sxs-lookup"><span data-stu-id="4d94a-104">VMware to Azure</span></span>](./site-recovery-prepare-target-vmware-to-azure.md)
> * [<span data-ttu-id="4d94a-105">Fyzické do Azure</span><span class="sxs-lookup"><span data-stu-id="4d94a-105">Physical to Azure</span></span>](./site-recovery-prepare-target-physical-to-azure.md)

<span data-ttu-id="4d94a-106">Tento článek popisuje postup přípravy prostředí Azure pro zahájení replikace fyzických serverů (x 64) se systémem Windows nebo Linux do Azure.</span><span class="sxs-lookup"><span data-stu-id="4d94a-106">This article describes how to prepare your Azure environment to start replicating physical servers (x64) running Windows or Linux into Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4d94a-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4d94a-107">Prerequisites</span></span>

<span data-ttu-id="4d94a-108">Článek předpokládá následující:</span><span class="sxs-lookup"><span data-stu-id="4d94a-108">The article assumes the following:</span></span>
- <span data-ttu-id="4d94a-109">Jste vytvořili trezor služeb zotavení k ochraně fyzických serverů.</span><span class="sxs-lookup"><span data-stu-id="4d94a-109">You have created a Recovery Services Vault to protect your physical servers.</span></span> <span data-ttu-id="4d94a-110">Můžete vytvořit trezor služeb zotavení z [portál Azure](http://portal.azure.com "portál Azure").</span><span class="sxs-lookup"><span data-stu-id="4d94a-110">You can create a Recovery Services Vault from the [Azure portal](http://portal.azure.com "Azure portal").</span></span>
- <span data-ttu-id="4d94a-111">Máte [instalaci prostředí místní](./site-recovery-set-up-physical-to-azure.md) replikace fyzických serverů do Azure.</span><span class="sxs-lookup"><span data-stu-id="4d94a-111">You have [setup your on-premises environment](./site-recovery-set-up-physical-to-azure.md) to replicate physical servers to Azure.</span></span>

## <a name="prepare-target"></a><span data-ttu-id="4d94a-112">Příprava cílového</span><span class="sxs-lookup"><span data-stu-id="4d94a-112">Prepare target</span></span>

<span data-ttu-id="4d94a-113">Po dokončení **cíl ochrany krok 1: vyberte** a **krok 2: Příprava zdroj**, přejdete na **krok 3: cíl**</span><span class="sxs-lookup"><span data-stu-id="4d94a-113">After completing the **Step 1:Select Protection goal** and **Step 2:Prepare Source**, you are taken to **Step 3: Target**</span></span>

![Příprava cílového](./media/site-recovery-prepare-target-physical-to-azure/prepare-target-physical-to-azure.png)

1. <span data-ttu-id="4d94a-115">**Předplatné:** z rozevírací nabídky vyberte odběr, který chcete replikovat fyzických serverů.</span><span class="sxs-lookup"><span data-stu-id="4d94a-115">**Subscription:** From the drop down menu, select the Subscription that you want to replicate your physical servers to.</span></span>
2. <span data-ttu-id="4d94a-116">**Model nasazení:** vyberte model nasazení (Classic nebo Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="4d94a-116">**Deployment Model:** Select the deployment model (Classic or Resource Manager)</span></span>

<span data-ttu-id="4d94a-117">Podle modelu vybrané nasazení, je zajistit, že máte alespoň jeden účet kompatibilní úložiště a virtuální sítě v cílové předplatné pro replikaci a převzetí služeb při selhání vaší fyzických serverů na spusťte ověřování.</span><span class="sxs-lookup"><span data-stu-id="4d94a-117">Based on the chosen deployment model, a validation is run to ensure that you have at least one compatible storage account and virtual network in the target subscription to replicate and failover your physical servers to.</span></span>

<span data-ttu-id="4d94a-118">Jakmile ověřovací úspěšně dokončit, klikněte na tlačítko OK přejdete k dalšímu kroku.</span><span class="sxs-lookup"><span data-stu-id="4d94a-118">Once the validations complete successfully, click OK to go to the next step.</span></span>

<span data-ttu-id="4d94a-119">Pokud nemáte účet úložiště kompatibilní Resource Manager nebo virtuální sítě, nebo chcete přidat více, můžete provést kliknutím **+ účet úložiště** nebo **+ síť** tlačítka nahoře v okně.</span><span class="sxs-lookup"><span data-stu-id="4d94a-119">If you don't have a compatible Resource Manager storage account or virtual network, or would like to add more, you can do so by clicking the **+ Storage Account** or **+ Network** buttons on the top of the blade.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4d94a-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4d94a-120">Next steps</span></span>
<span data-ttu-id="4d94a-121">[Konfigurace nastavení replikace](./site-recovery-setup-replication-settings-vmware.md).</span><span class="sxs-lookup"><span data-stu-id="4d94a-121">[Configure replication settings](./site-recovery-setup-replication-settings-vmware.md).</span></span>
