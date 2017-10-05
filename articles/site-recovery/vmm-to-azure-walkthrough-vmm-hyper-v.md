---
title: "Příprava System Center VMM pro replikaci technologie Hyper-V do Azure | Microsoft Docs"
description: "Popisuje, jak připravit server System Center VMM pro replikaci technologie Hyper-V do Azure pomocí Azure Site Recovery"
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
ms.openlocfilehash: ec118ed837dbf140083b3ae1e4ecd41c81562018
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="step-6-prepare-vmm-servers-and-hyper-v-hosts-for-hyper-v-replication-to-azure"></a><span data-ttu-id="b5a28-103">Krok 6: Připravte servery VMM a hostitelé Hyper-V pro replikaci technologie Hyper-V do Azure.</span><span class="sxs-lookup"><span data-stu-id="b5a28-103">Step 6: Prepare VMM servers and Hyper-V hosts for Hyper-V replication to Azure</span></span>

<span data-ttu-id="b5a28-104">Po nastavení [Azure součásti](vmm-to-azure-walkthrough-prepare-azure.md) pro nasazení, postupujte podle pokynů v tomto článku připravit místní servery VMM a hostitelé Hyper-V pro interakci s Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="b5a28-104">After setting up [Azure components](vmm-to-azure-walkthrough-prepare-azure.md) for the deployment, use the instructions in this article to prepare on-premises VMM servers and Hyper-V hosts to interact with Azure Site Recovery.</span></span>

<span data-ttu-id="b5a28-105">Po přečtení tohoto článku, post jakékoli komentáře v dolní části, nebo požádat technické dotazy na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="b5a28-105">After reading this article, post any comments at the bottom, or ask technical questions on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="prepare-vmm-servers"></a><span data-ttu-id="b5a28-106">Připravit servery VMM</span><span class="sxs-lookup"><span data-stu-id="b5a28-106">Prepare VMM servers</span></span>

- <span data-ttu-id="b5a28-107">Musíte mít alespoň jeden server VMM, který splňuje požadavky na podporu pro Site Recovery replikaci (site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).</span><span class="sxs-lookup"><span data-stu-id="b5a28-107">You need at least one VMM server that meet the support requirements for Site Recovery replication (site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).</span></span>
- <span data-ttu-id="b5a28-108">Zajistěte, aby Připravili jste server VMM pro [mapování sítě](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).</span><span class="sxs-lookup"><span data-stu-id="b5a28-108">Make sure you've prepared the VMM server for [network mapping](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).</span></span>
- <span data-ttu-id="b5a28-109">Ujistěte se, že můžete přístup k serveru VMM pro tyto adresy URL</span><span class="sxs-lookup"><span data-stu-id="b5a28-109">Make sure that the VMM server can access these URLs</span></span>

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- <span data-ttu-id="b5a28-110">Pokud máte zavedená pravidla brány firewall založená na IP adrese, zkontrolujte, že tato pravidla umožňují komunikaci s Azure.</span><span class="sxs-lookup"><span data-stu-id="b5a28-110">If you have IP address-based firewall rules, ensure they allow communication to Azure.</span></span>
- <span data-ttu-id="b5a28-111">Povolte [Rozsahy IP adres datového centra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) a port HTTPS (443).</span><span class="sxs-lookup"><span data-stu-id="b5a28-111">Allow the [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653), and the HTTPS (443) port.</span></span>
- <span data-ttu-id="b5a28-112">Povolte rozsahy IP adres pro oblast Azure svého předplatného a pro oblast Západní USA (používá se pro řízení přístupu a správu identit).</span><span class="sxs-lookup"><span data-stu-id="b5a28-112">Allow IP address ranges for the Azure region of your subscription, and for West US (used for Access Control and Identity Management).</span></span>

<span data-ttu-id="b5a28-113">Během nasazování Site Recovery stáhněte zprostředkovatele služby Site Recovery a nainstalovat na každý server VMM.</span><span class="sxs-lookup"><span data-stu-id="b5a28-113">During Site Recovery deployment, you download the Site Recovery Provider and install it on each VMM server.</span></span> <span data-ttu-id="b5a28-114">VMM server je zaregistrován v trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="b5a28-114">The VMM server is registered in the Recovery Services vault.</span></span>




## <a name="next-steps"></a><span data-ttu-id="b5a28-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b5a28-115">Next steps</span></span>

<span data-ttu-id="b5a28-116">Přejděte na [krok 7: vytvoření trezoru](vmm-to-azure-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="b5a28-116">Go to [Step 7: Create a vault](vmm-to-azure-walkthrough-create-vault.md)</span></span>

