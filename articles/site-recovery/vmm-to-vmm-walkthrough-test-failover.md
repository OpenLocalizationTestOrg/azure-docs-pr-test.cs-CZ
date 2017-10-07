---
title: "aaaRun testovací převzetí služeb při selhání pro virtuální počítač Hyper-V replikace tooa sekundární lokalitu s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak toorun testovací převzetí služeb při selhání pro virtuální počítač Hyper-V replikace tooa sekundární System Center VMM lokalitu s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a3fd11ce-65a1-4308-8435-45cf954353ef
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: a60411541c2db001c573735bcb0d651f6618f295
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-run-a-test-failover-for-hyper-v-replication-tooa-secondary-site"></a>Krok 10: Spuštění testovací převzetí služeb při selhání pro sekundární lokalitě tooa replikace Hyper-V


Po povolení replikace pro virtuální počítače Hyper-V (VM) s [Azure Site Recovery](site-recovery-overview.md), použijte tento článek toorun testovací převzetí služeb. Testovací převzetí služeb ověřuje, že replikace pracuje bez dopadu na produkční prostředí. 


Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Než začnete

- Když se aktivuje testovací převzetí služeb, můžete zadat hello sítě toowhich hello testovací repliku, které se připojí virtuální počítače. [Další informace](site-recovery-test-failover-vmm-to-vmm.md#network-options-in-site-recovery) o možnostech sítě hello.
- Doporučujeme, abyste si zvolili síť, kterou jste vybrali při mapování sítě.
- Hello pokyny v tomto článku popisují, jak toofail přes jeden virtuální počítač. Přečtěte si informace o [vytváření plánů obnovení](site-recovery-create-recovery-plans.md) Chcete-li toofail přes víc virtuálních počítačů současně.
- Přečtěte si informace o [omezení pro testovací převzetí služeb při selhání](site-recovery-test-failover-vmm-to-vmm.md#things-to-note).
- Hello IP adresa poskytnutý tooa virtuálních počítačů během testovacího převzetí služeb při selhání je hello stejnou IP adresu, která hello virtuálních počítačů by přijímat plánovaném nebo neplánovaném převzetí služeb při selhání (za předpokladu, že hello IP adresa je k dispozici v hello testovací převzetí služeb při selhání sítě). Pokud hello IP adresa není k dispozici v hello testovací převzetí služeb při selhání sítě, obdrží hello virtuálních počítačů jinou IP adresu, která je k dispozici v hello testovací převzetí služeb při selhání sítě.
- Pokud chcete, toodo testovací převzetí služeb v produkční sítě, pro úplné ověření počítače připojení k síti začátku do konce, Všimněte si, že:
    - Dobrý den, by měl být primární virtuální počítač vypnut, když provádíte hello testovací převzetí služeb při selhání. Jinak, dva virtuální počítače s hello stejnou identitu bude spuštěna v hello stejné sítě v hello stejnou dobu. 
    - Pokud provedete změny tootest virtuálních počítačů, tyto změny budou ztraceny při čištění hello testování převzetí služeb při selhání. Tyto změny nejsou replikované back toohello primárního virtuálního počítače.
    - Testování produkční síť vede toodowntime pro produkční zatížení. Požádejte uživatele není toouse hello aplikace, když probíhá hello postupu zotavení po havárii.  


## <a name="run-a-test-failover-for-a-vm"></a>Spustit testovací převzetí služeb pro virtuální počítač

1. toofail přes jeden virtuální počítač, v **replikované položky**, klikněte na tlačítko hello virtuálního počítače > **testovací převzetí služeb při selhání**.
2. V **testovací převzetí služeb při selhání**, zadejte, jak testovací virtuální počítače budou připojené toonetworks po hello testovací převzetí služeb při selhání. 
3. Klikněte na tlačítko **OK** toobegin hello převzetí služeb při selhání. Průběh sledovat na hello **úlohy** kartě.
5. Po dokončení převzetí služeb při selhání ověřte úspěšně testu hello spuštění virtuálních počítačů.
6. Když jste hotovi, klikněte na tlačítko **vyčistit testovací převzetí služeb při selhání** v plánu obnovení hello.
7. V **poznámky**, zaznamenejte a uložte jakékoli připomínky související s hello testovací převzetí služeb při selhání. Tento krok odstraní hello virtuální počítače a sítě, které byly vytvořeny během testovacího převzetí služeb při selhání.


## <a name="next-steps"></a>Další kroky

Po nasazení hello otestujete, další informace o dalších typů [převzetí služeb při selhání](site-recovery-failover.md).
