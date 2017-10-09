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
# <a name="step-6-prepare-hyper-v-hosts-for-replication-tooazure"></a>Krok 6: Příprava hostitele Hyper-V na tooAzure replikace

Použití hello pokynů tohoto článku tooprepare místní toointeract hostitele technologie Hyper-V s Azure Site Recovery.

Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello, nebo požádat technické dotazy na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prepare-hosts"></a>Příprava hostitele

- Ujistěte se, že hello technologie Hyper-V hostitelé splňují hello [požadavky](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm).
- Ujistěte se, že hello hostitelé mají přístup k hello požadované adresy URL:

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- Pokud máte pravidla brány firewall založená na adresu IP, ověřte, že umožňují tooAzure komunikace.
- Povolit hello [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653)a hello port HTTPS (443).
- Povolte rozsahy IP adres pro hello oblast Azure svého předplatného a západní USA (používá se pro správu Identity a řízení přístupu).

Během nasazování Site Recovery přidáte hostitele Hyper-V, které obsahují virtuální počítače chcete tooreplicate tooa technologie Hyper-V lokalitě. Hello zprostředkovatele služby Site Recovery a agenta služeb zotavení jsou nainstalovány na každém hostiteli. Hello technologie Hyper-V lokality je zaregistrován v trezoru služeb zotavení hello.

## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 7: vytvoření trezoru](hyper-v-site-walkthrough-create-vault.md)

