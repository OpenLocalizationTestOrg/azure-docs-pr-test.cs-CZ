---
title: aaaPrepare System Center VMM pro tooAzure replikace technologie Hyper-V | Microsoft Docs
description: "Popisuje, jak server System Center VMM tooprepare pro tooAzure replikace technologie Hyper-V, pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: afcd81ae-d192-4013-a0af-3dac45b3c7e9
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 773b06afaf7d3eea1fe64f050bf3970943cf466a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-prepare-vmm-servers-and-hyper-v-hosts-for-hyper-v-replication-tooazure"></a><span data-ttu-id="0ccd4-103">Krok 6: Připravte servery VMM a hostitelé Hyper-V pro tooAzure replikace technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="0ccd4-103">Step 6: Prepare VMM servers and Hyper-V hosts for Hyper-V replication tooAzure</span></span>

<span data-ttu-id="0ccd4-104">Po nastavení [Azure součásti](vmm-to-azure-walkthrough-prepare-azure.md) hello nasazení, použijte hello pokyny v tomto článku tooprepare místní VMM servery a toointeract hostitele technologie Hyper-V s Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="0ccd4-104">After setting up [Azure components](vmm-to-azure-walkthrough-prepare-azure.md) for hello deployment, use hello instructions in this article tooprepare on-premises VMM servers and Hyper-V hosts toointeract with Azure Site Recovery.</span></span>

<span data-ttu-id="0ccd4-105">Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello, nebo požádat technické dotazy na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="0ccd4-105">After reading this article, post any comments at hello bottom, or ask technical questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="prepare-vmm-servers"></a><span data-ttu-id="0ccd4-106">Připravit servery VMM</span><span class="sxs-lookup"><span data-stu-id="0ccd4-106">Prepare VMM servers</span></span>

- <span data-ttu-id="0ccd4-107">Musíte mít alespoň jeden server VMM, který splňuje požadavky na podporu hello pro Site Recovery replikaci (site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).</span><span class="sxs-lookup"><span data-stu-id="0ccd4-107">You need at least one VMM server that meet hello support requirements for Site Recovery replication (site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).</span></span>
- <span data-ttu-id="0ccd4-108">Ujistěte se, že jste připravili hello server VMM pro [mapování sítě](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).</span><span class="sxs-lookup"><span data-stu-id="0ccd4-108">Make sure you've prepared hello VMM server for [network mapping](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).</span></span>
- <span data-ttu-id="0ccd4-109">Ujistěte se, že tento server VMM hello přístup tyto adresy URL</span><span class="sxs-lookup"><span data-stu-id="0ccd4-109">Make sure that hello VMM server can access these URLs</span></span>

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- <span data-ttu-id="0ccd4-110">Pokud máte pravidla brány firewall založená na adresu IP, ověřte, že umožňují tooAzure komunikace.</span><span class="sxs-lookup"><span data-stu-id="0ccd4-110">If you have IP address-based firewall rules, ensure they allow communication tooAzure.</span></span>
- <span data-ttu-id="0ccd4-111">Povolit hello [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653)a hello port HTTPS (443).</span><span class="sxs-lookup"><span data-stu-id="0ccd4-111">Allow hello [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653), and hello HTTPS (443) port.</span></span>
- <span data-ttu-id="0ccd4-112">Povolte rozsahy IP adres pro hello oblast Azure svého předplatného a západní USA (používá se pro správu Identity a řízení přístupu).</span><span class="sxs-lookup"><span data-stu-id="0ccd4-112">Allow IP address ranges for hello Azure region of your subscription, and for West US (used for Access Control and Identity Management).</span></span>

<span data-ttu-id="0ccd4-113">Během nasazování Site Recovery stáhněte hello zprostředkovatele služby Site Recovery a nainstalovat na každý server VMM.</span><span class="sxs-lookup"><span data-stu-id="0ccd4-113">During Site Recovery deployment, you download hello Site Recovery Provider and install it on each VMM server.</span></span> <span data-ttu-id="0ccd4-114">Hello VMM server je zaregistrován v trezoru služeb zotavení hello.</span><span class="sxs-lookup"><span data-stu-id="0ccd4-114">hello VMM server is registered in hello Recovery Services vault.</span></span>




## <a name="next-steps"></a><span data-ttu-id="0ccd4-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0ccd4-115">Next steps</span></span>

<span data-ttu-id="0ccd4-116">Přejděte příliš[krok 7: vytvoření trezoru](vmm-to-azure-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="0ccd4-116">Go too[Step 7: Create a vault](vmm-to-azure-walkthrough-create-vault.md)</span></span>

