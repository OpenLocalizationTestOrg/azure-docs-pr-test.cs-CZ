---
title: "Architektura hello aaaReview tooa replikace technologie Hyper-V s Azure Site Recovery sekundární lokality. | Microsoft Docs"
description: "Tento článek obsahuje přehled o architektuře hello pro replikaci místní virtuální počítače Hyper-V tooa sekundární System Center VMM lokalitu s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 07099161-4cc7-4f32-8eb9-2a71bbf0750b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 0de4b4e8601116c73e6fd710597ce4e561884368
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-1-review-hello-architecture-for-hyper-v-replication-tooa-secondary-site"></a>Krok 1: Posouzení hello Architektura technologie Hyper-V replikace tooa sekundární lokality

Tento článek popisuje hello součásti a procesů při replikaci místní virtuální počítače Hyper-V (VM) v cloudech System Center Virtual Machine Manager (VMM), sekundární lokalita VMM tooa pomocí hello [Azure Site Recovery](site-recovery-overview.md)služby v hello portálu Azure.

Odeslat všechny komentáře dole hello v tomto článku, nebo v hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## <a name="architectural-components"></a>Komponenty architektury

Zde je, co potřebujete pro replikaci virtuálních počítačů Hyper-V tooa sekundární lokalita VMM.

**Komponenta** | **Umístění** | **Podrobnosti**
--- | --- | ---
**Azure** | Předplatné Azure. | Vytvoření trezoru služeb zotavení v hello předplatné Azure, tooorchestrate a spravovat replikace mezi umístěními VMM.
**Server VMM** | Potřebujete primární a sekundární umístění VMM. | Doporučujeme, abyste server VMM v primární lokalitě hello a po jednom v sekundární lokalitě hello 
**Server Hyper-V** |  Jeden nebo více servery Hyper-V host v hello primárních a sekundárních cloudech VMM. | Data se replikují mezi hello primární a sekundární servery Hyper-V hostiteli prostřednictvím hello LAN nebo VPN pomocí protokolu Kerberos nebo ověření certifikátu.  
**Virtuální počítače Hyper-V** | Na hostitelském serveru Hyper-V. | Hello zdrojový hostitelský server by měl mít aspoň jeden virtuální počítač, který má tooreplicate.

## <a name="replication-process"></a>Proces replikace

1. Nastavit hello účet Azure, vytvořte trezor služeb zotavení a určit, co chcete tooreplicate.
2. Nakonfigurujete hello zdrojové a cílové nastavení replikace, které zahrnuje instalaci hello zprostředkovatele Azure Site Recovery na servery VMM a agenta služeb zotavení Microsoft Azure hello na každém hostiteli technologie Hyper-V.
3. Můžete vytvořit zásadu replikace pro zdroj hello cloudu VMM. zásady Hello je použité tooall virtuální počítače umístěné na hostitelích v cloudu hello.
4. Povolení replikace pro každé VMM, a proběhne počáteční replikace virtuálního počítače, v souladu s nastavením text hello, které zvolíte.
5. Po počáteční replikaci se zahájí replikace rozdílových změn. Sledované změny se pro jednotlivé položky ukládají do souboru .hrl.


![Místní tooon místní](./media/vmm-to-vmm-walkthrough-architecture/arch-onprem-onprem.png)

## <a name="failover-and-failback-process"></a>Proces převzetí služeb při selhání a navrácení služeb po obnovení

1. Můžete spustit plánované nebo neplánované [převzetí služeb při selhání](site-recovery-failover.md) mezi místními lokalitami. Pokud spustíte plánované převzetí služeb při selhání, pak zdrojové virtuální počítače jsou vypnout tooensure nedošlo ke ztrátě dat.
2. Můžete převzít jeden počítač nebo vytvořit [plány obnovení](site-recovery-create-recovery-plans.md) tooorchestrate převzetí služeb při selhání více počítačů.
4. Pokud provádíte tooa sekundární lokality neplánované převzetí služeb při selhání, po převzetí služeb při selhání počítače hello v hello sekundárního umístění nejsou povolené pro ochranu nebo replikace. Pokud jste spustili plánované převzetí služeb při selhání, po převzetí služeb při selhání hello, jsou chráněné počítače v hello sekundárního umístění.
5. Poté potvrďte hello převzetí služeb při selhání toostart přístupem hello zatížení z hello repliky virtuálních počítačů.
6. Při primární lokality je opět k dispozici, můžete zahájit tooreplicate zpětná replikace z primární toohello sekundární lokality hello. Zpětná replikace přináší hello virtuální počítače v chráněném stavu, ale hello sekundárního datového centra je stále aktivní umístění hello.
7. toomake hello primární lokalitu do umístění služby active hello znovu spustíte plánované převzetí služeb při selhání z sekundární tooprimary, za nímž následuje jiný zpětné replikace.



## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 2: Přečtěte si hello předpoklady a omezení](vmm-to-vmm-walkthrough-prerequisites.md).
