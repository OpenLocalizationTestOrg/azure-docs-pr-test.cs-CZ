---
title: "Příprava cílového (fyzické tooAzure) | Microsoft Docs"
description: "Tento článek popisuje, jak tooprepare toostart vašeho prostředí Azure replikovat fyzické servery se systémem Windows nebo Linux tooAzure."
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
ms.openlocfilehash: 126fb86133e1a00f5669410943565c4cd78e4369
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-target-vmware-tooazure"></a><span data-ttu-id="6a5fb-103">Příprava cílového (VMware tooAzure)</span><span class="sxs-lookup"><span data-stu-id="6a5fb-103">Prepare target (VMware tooAzure)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6a5fb-104">VMware tooAzure</span><span class="sxs-lookup"><span data-stu-id="6a5fb-104">VMware tooAzure</span></span>](./site-recovery-prepare-target-vmware-to-azure.md)
> * [<span data-ttu-id="6a5fb-105">Fyzické tooAzure</span><span class="sxs-lookup"><span data-stu-id="6a5fb-105">Physical tooAzure</span></span>](./site-recovery-prepare-target-physical-to-azure.md)

<span data-ttu-id="6a5fb-106">Tento článek popisuje, jak tooprepare vašeho prostředí Azure toostart replikace fyzických serverů (x 64) se systémem Windows nebo Linux do Azure.</span><span class="sxs-lookup"><span data-stu-id="6a5fb-106">This article describes how tooprepare your Azure environment toostart replicating physical servers (x64) running Windows or Linux into Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6a5fb-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6a5fb-107">Prerequisites</span></span>

<span data-ttu-id="6a5fb-108">Hello článek předpokládá hello následující:</span><span class="sxs-lookup"><span data-stu-id="6a5fb-108">hello article assumes hello following:</span></span>
- <span data-ttu-id="6a5fb-109">Vytvoření trezoru služeb zotavení tooprotect fyzických serverů.</span><span class="sxs-lookup"><span data-stu-id="6a5fb-109">You have created a Recovery Services Vault tooprotect your physical servers.</span></span> <span data-ttu-id="6a5fb-110">Můžete vytvořit trezor služeb zotavení z hello [portál Azure](http://portal.azure.com "portál Azure").</span><span class="sxs-lookup"><span data-stu-id="6a5fb-110">You can create a Recovery Services Vault from hello [Azure portal](http://portal.azure.com "Azure portal").</span></span>
- <span data-ttu-id="6a5fb-111">Máte [instalaci prostředí místní](./site-recovery-set-up-physical-to-azure.md) tooAzure tooreplicate fyzických serverů.</span><span class="sxs-lookup"><span data-stu-id="6a5fb-111">You have [setup your on-premises environment](./site-recovery-set-up-physical-to-azure.md) tooreplicate physical servers tooAzure.</span></span>

## <a name="prepare-target"></a><span data-ttu-id="6a5fb-112">Příprava cílového</span><span class="sxs-lookup"><span data-stu-id="6a5fb-112">Prepare target</span></span>

<span data-ttu-id="6a5fb-113">Po dokončení hello **cíl ochrany krok 1: vyberte** a **krok 2: Příprava zdroj**, budete přesměrováni příliš**krok 3: cíl**</span><span class="sxs-lookup"><span data-stu-id="6a5fb-113">After completing hello **Step 1:Select Protection goal** and **Step 2:Prepare Source**, you are taken too**Step 3: Target**</span></span>

![Příprava cílového](./media/site-recovery-prepare-target-physical-to-azure/prepare-target-physical-to-azure.png)

1. <span data-ttu-id="6a5fb-115">**Předplatné:** z hello rozevírací nabídce vyberte hello předplatné, které chcete tooreplicate fyzických serverů.</span><span class="sxs-lookup"><span data-stu-id="6a5fb-115">**Subscription:** From hello drop down menu, select hello Subscription that you want tooreplicate your physical servers to.</span></span>
2. <span data-ttu-id="6a5fb-116">**Model nasazení:** model nasazení vyberte hello (Classic nebo Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="6a5fb-116">**Deployment Model:** Select hello deployment model (Classic or Resource Manager)</span></span>

<span data-ttu-id="6a5fb-117">Podle hello zvolený model nasazení, ověřování tooensure, jestli máte aspoň jeden účet kompatibilní úložiště a virtuální sítě v hello cílové předplatné tooreplicate a převzetí služeb při selhání vaší fyzických serverů na spuštění.</span><span class="sxs-lookup"><span data-stu-id="6a5fb-117">Based on hello chosen deployment model, a validation is run tooensure that you have at least one compatible storage account and virtual network in hello target subscription tooreplicate and failover your physical servers to.</span></span>

<span data-ttu-id="6a5fb-118">Po ověření hello úspěšně dokončit, klikněte na tlačítko OK toogo toohello další krok.</span><span class="sxs-lookup"><span data-stu-id="6a5fb-118">Once hello validations complete successfully, click OK toogo toohello next step.</span></span>

<span data-ttu-id="6a5fb-119">Pokud nemáte účet úložiště kompatibilní Resource Manager nebo virtuální sítě, nebo chcete tooadd další, můžete provést kliknutím hello **+ účet úložiště** nebo **+ síť** tlačítka na začátku hello hello okno.</span><span class="sxs-lookup"><span data-stu-id="6a5fb-119">If you don't have a compatible Resource Manager storage account or virtual network, or would like tooadd more, you can do so by clicking hello **+ Storage Account** or **+ Network** buttons on hello top of hello blade.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6a5fb-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6a5fb-120">Next steps</span></span>
<span data-ttu-id="6a5fb-121">[Konfigurace nastavení replikace](./site-recovery-setup-replication-settings-vmware.md).</span><span class="sxs-lookup"><span data-stu-id="6a5fb-121">[Configure replication settings](./site-recovery-setup-replication-settings-vmware.md).</span></span>
