---
title: "aaaReview hello předpoklady pro replikaci technologie Hyper-V tooAzure (bez System Center VMM) pomocí Azure Site Recovery | Microsoft Docs"
description: "Popisuje hello požadavky pro nastavení replikace, převzetí služeb při selhání a obnovení tooAzure místní virtuální počítače Hyper-V s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 7ef3fb46-52f5-4c8a-b1a1-658c2305762a
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: 3eefc3b7e3982ec6c413c1db7f7784863f9c701d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-2-review-hello-prerequisites-for-hyper-v-without-vmm-tooazure-replication"></a><span data-ttu-id="b7f16-103">Krok 2: Kontrola hello předpoklady pro replikaci tooAzure technologie Hyper-V (bez VMM)</span><span class="sxs-lookup"><span data-stu-id="b7f16-103">Step 2: Review hello prerequisites for Hyper-V (without VMM) tooAzure replication</span></span>

<span data-ttu-id="b7f16-104">Hello požadavky jsou shrnuty v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="b7f16-104">hello prerequisites are summarized in hello table.</span></span>


<span data-ttu-id="b7f16-105">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="b7f16-105">**Prerequisite**</span></span> | <span data-ttu-id="b7f16-106">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="b7f16-106">**Details**</span></span> 
--- | --- 
<span data-ttu-id="b7f16-107">**Azure**</span><span class="sxs-lookup"><span data-stu-id="b7f16-107">**Azure**</span></span> | <span data-ttu-id="b7f16-108">Přečtěte si o [požadavcích na Azure](site-recovery-prereq.md#azure-requirements).</span><span class="sxs-lookup"><span data-stu-id="b7f16-108">Learn about [Azure requirements](site-recovery-prereq.md#azure-requirements).</span></span>
<span data-ttu-id="b7f16-109">**Místní servery**</span><span class="sxs-lookup"><span data-stu-id="b7f16-109">**On-premises servers**</span></span> | <span data-ttu-id="b7f16-110">[Další informace](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm) o požadavcích pro hostitele Hyper-V místní hello.</span><span class="sxs-lookup"><span data-stu-id="b7f16-110">[Learn more](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm) about requirements for hello on-premises Hyper-V hosts.</span></span>
<span data-ttu-id="b7f16-111">**Místní virtuální počítače Hyper-V**</span><span class="sxs-lookup"><span data-stu-id="b7f16-111">**On-premises Hyper-V VMs**</span></span> | <span data-ttu-id="b7f16-112">Virtuální počítače chcete tooreplicate by měla být spuštěná [podporovaný operační systém](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)a v souladu s [požadavky Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="b7f16-112">VMs you want tooreplicate should be running a [supported operating system](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions), and conform with [Azure prerequisites](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span>
<span data-ttu-id="b7f16-113">**Adresy URL Azure**</span><span class="sxs-lookup"><span data-stu-id="b7f16-113">**Azure URLs**</span></span> | <span data-ttu-id="b7f16-114">Hostitelé Hyper-V, potřebujete přístup toothese adresy URL:</span><span class="sxs-lookup"><span data-stu-id="b7f16-114">Hyper-V hosts need access toothese URLs:</span></span><br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> <span data-ttu-id="b7f16-115">Pokud máte pravidla brány firewall založená na adresu IP, ověřte, že umožňují tooAzure komunikace.</span><span class="sxs-lookup"><span data-stu-id="b7f16-115">If you have IP address-based firewall rules, ensure they allow communication tooAzure.</span></span><br/></br> <span data-ttu-id="b7f16-116">Povolit hello [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653)a hello port HTTPS (443).</span><span class="sxs-lookup"><span data-stu-id="b7f16-116">Allow hello [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653), and hello HTTPS (443) port.</span></span><br/></br> <span data-ttu-id="b7f16-117">Povolte rozsahy IP adres pro hello oblast Azure svého předplatného a západní USA (používá se pro správu Identity a řízení přístupu).</span><span class="sxs-lookup"><span data-stu-id="b7f16-117">Allow IP address ranges for hello Azure region of your subscription, and for West US (used for Access Control and Identity Management).</span></span>



## <a name="next-steps"></a><span data-ttu-id="b7f16-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b7f16-118">Next steps</span></span>

- <span data-ttu-id="b7f16-119">Pokud jste to úplné nasazení, přejděte příliš[krok 3: plánování kapacity](hyper-v-site-walkthrough-capacity.md)</span><span class="sxs-lookup"><span data-stu-id="b7f16-119">If you're doing a full deployment, go too[Step 3: Plan capacity](hyper-v-site-walkthrough-capacity.md)</span></span>
- <span data-ttu-id="b7f16-120">Pokud jste to jednoduchá testovací nasazení, přejděte příliš[krok 4: plánování sítě](hyper-v-site-walkthrough-network.md).</span><span class="sxs-lookup"><span data-stu-id="b7f16-120">If you're doing a simple test deployment, go too[Step 4: Plan networking](hyper-v-site-walkthrough-network.md).</span></span>
