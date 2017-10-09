---
title: hostuje aaaPrepare technologie Hyper-V (bez System Center VMM) pro replikaci tooAzure | Microsoft Docs
description: "Popisuje, jak tooprepare technologie Hyper-V hostuje pro replikaci tooAzure pomocí Azure Site Recovery"
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
ms.openlocfilehash: 714b229d5efbd66a9844bd09e36ac3f69919a6bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-prepare-hyper-v-hosts-for-replication-tooazure"></a><span data-ttu-id="761dc-103">Krok 6: Příprava hostitele Hyper-V na tooAzure replikace</span><span class="sxs-lookup"><span data-stu-id="761dc-103">Step 6: Prepare Hyper-V hosts for replication tooAzure</span></span>

<span data-ttu-id="761dc-104">Použití hello pokynů tohoto článku tooprepare místní toointeract hostitele technologie Hyper-V s Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="761dc-104">Use hello instructions in this article tooprepare on-premises Hyper-V hosts toointeract with Azure Site Recovery.</span></span>

<span data-ttu-id="761dc-105">Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello, nebo požádat technické dotazy na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="761dc-105">After reading this article, post any comments at hello bottom, or ask technical questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="prepare-hosts"></a><span data-ttu-id="761dc-106">Příprava hostitele</span><span class="sxs-lookup"><span data-stu-id="761dc-106">Prepare hosts</span></span>

- <span data-ttu-id="761dc-107">Ujistěte se, že hello technologie Hyper-V hostitelé splňují hello [požadavky](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm).</span><span class="sxs-lookup"><span data-stu-id="761dc-107">Make sure that hello Hyper-V hosts meet hello [prerequisites](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm).</span></span>
- <span data-ttu-id="761dc-108">Ujistěte se, že hello hostitelé mají přístup k hello požadované adresy URL:</span><span class="sxs-lookup"><span data-stu-id="761dc-108">Make sure that hello hosts can access hello required URLs:</span></span>

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- <span data-ttu-id="761dc-109">Pokud máte pravidla brány firewall založená na adresu IP, ověřte, že umožňují tooAzure komunikace.</span><span class="sxs-lookup"><span data-stu-id="761dc-109">If you have IP address-based firewall rules, ensure they allow communication tooAzure.</span></span>
- <span data-ttu-id="761dc-110">Povolit hello [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653)a hello port HTTPS (443).</span><span class="sxs-lookup"><span data-stu-id="761dc-110">Allow hello [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653), and hello HTTPS (443) port.</span></span>
- <span data-ttu-id="761dc-111">Povolte rozsahy IP adres pro hello oblast Azure svého předplatného a západní USA (používá se pro správu Identity a řízení přístupu).</span><span class="sxs-lookup"><span data-stu-id="761dc-111">Allow IP address ranges for hello Azure region of your subscription, and for West US (used for Access Control and Identity Management).</span></span>

<span data-ttu-id="761dc-112">Během nasazování Site Recovery přidáte hostitele Hyper-V, které obsahují virtuální počítače chcete tooreplicate tooa technologie Hyper-V lokalitě.</span><span class="sxs-lookup"><span data-stu-id="761dc-112">During Site Recovery deployment, you add Hyper-V hosts that contain VMs you want tooreplicate tooa Hyper-V site.</span></span> <span data-ttu-id="761dc-113">Hello zprostředkovatele služby Site Recovery a agenta služeb zotavení jsou nainstalovány na každém hostiteli.</span><span class="sxs-lookup"><span data-stu-id="761dc-113">hello Site Recovery Provider, and Recovery Services agent are installed on each host.</span></span> <span data-ttu-id="761dc-114">Hello technologie Hyper-V lokality je zaregistrován v trezoru služeb zotavení hello.</span><span class="sxs-lookup"><span data-stu-id="761dc-114">hello Hyper-V site is registered in hello Recovery Services vault.</span></span>

## <a name="next-steps"></a><span data-ttu-id="761dc-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="761dc-115">Next steps</span></span>

<span data-ttu-id="761dc-116">Přejděte příliš[krok 7: vytvoření trezoru](hyper-v-site-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="761dc-116">Go too[Step 7: Create a vault](hyper-v-site-walkthrough-create-vault.md)</span></span>

