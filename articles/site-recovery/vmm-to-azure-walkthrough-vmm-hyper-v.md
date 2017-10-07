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
# <a name="step-6-prepare-vmm-servers-and-hyper-v-hosts-for-hyper-v-replication-tooazure"></a>Krok 6: Připravte servery VMM a hostitelé Hyper-V pro tooAzure replikace technologie Hyper-V

Po nastavení [Azure součásti](vmm-to-azure-walkthrough-prepare-azure.md) hello nasazení, použijte hello pokyny v tomto článku tooprepare místní VMM servery a toointeract hostitele technologie Hyper-V s Azure Site Recovery.

Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello, nebo požádat technické dotazy na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prepare-vmm-servers"></a>Připravit servery VMM

- Musíte mít alespoň jeden server VMM, který splňuje požadavky na podporu hello pro Site Recovery replikaci (site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).
- Ujistěte se, že jste připravili hello server VMM pro [mapování sítě](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).
- Ujistěte se, že tento server VMM hello přístup tyto adresy URL

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- Pokud máte pravidla brány firewall založená na adresu IP, ověřte, že umožňují tooAzure komunikace.
- Povolit hello [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653)a hello port HTTPS (443).
- Povolte rozsahy IP adres pro hello oblast Azure svého předplatného a západní USA (používá se pro správu Identity a řízení přístupu).

Během nasazování Site Recovery stáhněte hello zprostředkovatele služby Site Recovery a nainstalovat na každý server VMM. Hello VMM server je zaregistrován v trezoru služeb zotavení hello.




## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 7: vytvoření trezoru](vmm-to-azure-walkthrough-create-vault.md)

