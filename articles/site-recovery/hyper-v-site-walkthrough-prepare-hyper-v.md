---
title: "Příprava hostitele Hyper-V (bez System Center VMM) pro replikaci do Azure | Microsoft Docs"
description: "Popisuje postup přípravy hostitelů Hyper-V pro replikaci do Azure pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 0f204e24-8d78-4076-95c5-8137d1be9c01
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: f9bcaa8e55be6e8fddaf88ebc3f18f5dbb2811e4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-6-prepare-hyper-v-hosts-for-replication-to-azure"></a><span data-ttu-id="3d20e-103">Krok 6: Příprava hostitele Hyper-V pro replikaci do Azure.</span><span class="sxs-lookup"><span data-stu-id="3d20e-103">Step 6: Prepare Hyper-V hosts for replication to Azure</span></span>

<span data-ttu-id="3d20e-104">Příprava místního hostitele Hyper-V pro interakci s Azure Site Recovery, postupujte podle pokynů v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="3d20e-104">Use the instructions in this article to prepare on-premises Hyper-V hosts to interact with Azure Site Recovery.</span></span>

<span data-ttu-id="3d20e-105">Po přečtení tohoto článku, post jakékoli komentáře v dolní části, nebo požádat technické dotazy na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="3d20e-105">After reading this article, post any comments at the bottom, or ask technical questions on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="prepare-hosts"></a><span data-ttu-id="3d20e-106">Příprava hostitele</span><span class="sxs-lookup"><span data-stu-id="3d20e-106">Prepare hosts</span></span>

- <span data-ttu-id="3d20e-107">Ujistěte se, že splňují hostitelů Hyper-V [požadavky](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm).</span><span class="sxs-lookup"><span data-stu-id="3d20e-107">Make sure that the Hyper-V hosts meet the [prerequisites](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm).</span></span>
- <span data-ttu-id="3d20e-108">Ujistěte se, že hostitelé mají přístup k požadované adresy URL:</span><span class="sxs-lookup"><span data-stu-id="3d20e-108">Make sure that the hosts can access the required URLs:</span></span>

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- <span data-ttu-id="3d20e-109">Pokud máte zavedená pravidla brány firewall založená na IP adrese, zkontrolujte, že tato pravidla umožňují komunikaci s Azure.</span><span class="sxs-lookup"><span data-stu-id="3d20e-109">If you have IP address-based firewall rules, ensure they allow communication to Azure.</span></span>
- <span data-ttu-id="3d20e-110">Povolte [Rozsahy IP adres datového centra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) a port HTTPS (443).</span><span class="sxs-lookup"><span data-stu-id="3d20e-110">Allow the [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653), and the HTTPS (443) port.</span></span>
- <span data-ttu-id="3d20e-111">Povolte rozsahy IP adres pro oblast Azure svého předplatného a pro oblast Západní USA (používá se pro řízení přístupu a správu identit).</span><span class="sxs-lookup"><span data-stu-id="3d20e-111">Allow IP address ranges for the Azure region of your subscription, and for West US (used for Access Control and Identity Management).</span></span>

<span data-ttu-id="3d20e-112">Během nasazování Site Recovery přidáte hostitele Hyper-V, které obsahují virtuální počítače, které chcete replikovat do lokality Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="3d20e-112">During Site Recovery deployment, you add Hyper-V hosts that contain VMs you want to replicate to a Hyper-V site.</span></span> <span data-ttu-id="3d20e-113">Zprostředkovatele služby Site Recovery a agenta služeb zotavení jsou nainstalované na každém hostiteli.</span><span class="sxs-lookup"><span data-stu-id="3d20e-113">The Site Recovery Provider, and Recovery Services agent are installed on each host.</span></span> <span data-ttu-id="3d20e-114">Lokality Hyper-V je zaregistrován v trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="3d20e-114">The Hyper-V site is registered in the Recovery Services vault.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3d20e-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3d20e-115">Next steps</span></span>

<span data-ttu-id="3d20e-116">Přejděte na [krok 7: vytvoření trezoru](hyper-v-site-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="3d20e-116">Go to [Step 7: Create a vault](hyper-v-site-walkthrough-create-vault.md)</span></span>
