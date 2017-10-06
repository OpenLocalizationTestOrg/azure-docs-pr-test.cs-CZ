---
title: "aaaRun testovací převzetí služeb při selhání pro replikaci virtuálního počítače Azure s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje hello kroky, které můžete potřebovat pro spuštění převzetí služeb při selhání pro virtuální počítače Azure replikaci tooanother oblast Azure pomocí Azure Site Recovery hello služby."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: e15c1b0c-5d75-4fdf-acb0-e61def9e9339
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: c1f765aa94c59dd70b33317ebbcd04beb7977969
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-run-a-test-failover-for-azure-vm-replication"></a>Krok 6: Spuštění testovací převzetí služeb při selhání pro replikaci virtuálního počítače Azure

Po povolení replikace pro virtuální počítač Azure (VM), postupujte podle kroků hello v tomto článku, toorun testovací převzetí služeb při selhání tooanother jedné oblasti Azure, pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.

- Po dokončení hello článku měli jste ověřili s testovací převzetí služeb při selhání, že alespoň jeden virtuální počítač Azure můžete převzít služby sekundární oblast Azure tooyour. 
- Odeslat všechny komentáře v dolní části hello tohoto článku nebo pokládání dotazů v hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

>[!NOTE]
>
> Azure replikace virtuálního počítače je aktuálně ve verzi preview.


## <a name="before-you-start"></a>Než začnete

- Doporučujeme, abyste před spuštěním testu převzetí služeb, ověřte vlastnosti virtuálního počítače hello a proveďte změny, které potřebujete. můžete přistupovat hello vlastností virtuálního počítače v **replikované položky**. Hello **Essentials** okno zobrazuje informace o nastavení počítače a stav.
- Doporučujeme použít samostatnou síť virtuálních počítačů Azure pro hello testovací převzetí služeb při selhání a ne hello síť (výchozí nebo přizpůsobit), byla nastavena na výrobní převzetí služeb při selhání.
- Hello testovací převzetí služeb při selhání selže přes virtuální počítače Azure (a jejich úložiště) toohello sekundární oblast Azure. Není replikovat, všechny závislé aplikace nebo prostředky. Pokud aplikace běžící na selhání přes virtuální počítače jsou závislé na jiné prostředky, třeba služby Active Directory nebo DNS, je třeba tooreplicate tyto příliš, pokud nejsou již k dispozici v sekundární oblasti. [Další informace](site-recovery-test-failover-to-azure.md#prepare-active-directory-and-dns)
- Pokud chcete tooaccess replikovat virtuální počítače po převzetí služeb při selhání z místního serveru, je třeba příliš[Příprava tooconnect](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) toothese virtuálních počítačů.

## <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání

1. V **nastavení** > **replikované položky**, klikněte na tlačítko hello virtuálních počítačů **+ testovací převzetí služeb při selhání** ikonu. 

2. V **testovací převzetí služeb při selhání**, vyberte toouse bod obnovení pro převzetí služeb při selhání hello:

    - **Nejnovější zpracované**: selže hello virtuálních počítačů přes toohello nejnovější bod obnovení, který byl zpracován službou Site Recovery hello. Zobrazí se Hello časové razítko. Pomocí této možnosti je žádné čas strávený zpracováváním dat, takže nabízí nízkou RTO (plánovanou dobu obnovení).
    - **Nejnovější aplikace konzistentní**: tuto možnost převezme všechny virtuální počítače toohello nejnovější konzistentní bod obnovení. Zobrazí se Hello časové razítko. 
    - **Vlastní**: Vyberte libovolného bodu obnovení.
 
3. Vyberte hello cílový virtuální síť Azure toowhich virtuální počítače Azure v sekundární oblasti hello se připojí po převzetí služeb při selhání hello.
4. toostart hello převzetí služeb při selhání, klikněte na tlačítko **OK**. tootrack průběhu, klikněte na virtuální počítač tooopen hello jeho vlastnosti. Nebo můžete kliknout na hello **testovací převzetí služeb při selhání** úlohy v název trezoru hello > **nastavení** > **úlohy** > **úlohy Site Recovery**.
5. Po hello dokončení převzetí služeb při selhání, hello repliky virtuálního počítače Azure se zobrazí v hello portálu Azure > **virtuální počítače**. Zajistěte, aby byl tento hello virtuálních počítačů hello odpovídající velikost, byl připojený toohello příslušnou síť, a zda je spuštěna.
6. Klikněte na tlačítko toodelete hello virtuálních počítačů, které byly vytvořeny během hello testovací převzetí služeb při selhání, **vyčistit testovací převzetí služeb při selhání** na hello replikované položky nebo hello plánu obnovení. V **poznámky**, zaznamenejte a uložte jakékoli připomínky související s hello testovací převzetí služeb při selhání. 

[Další informace](site-recovery-test-failover-to-azure.md) o testovací převzetí služeb při selhání.

## <a name="next-steps"></a>Další kroky

Po otestujete převzetí služeb při selhání, tento postup je dokončena. Nyní Další informace o spuštění převzetí služeb při selhání v produkčním prostředí:

- [Další informace](site-recovery-failover.md) o různých typech převzetí služeb při selhání a jak toorun je.
- Další informace o selhání přes víc virtuálních počítačů [pomocí plán obnovení](site-recovery-create-recovery-plans.md).
- Další informace o [pomocí plánů obnovení](site-recovery-create-recovery-plans.md).
- Další informace o [opětovnou ochranu virtuálních počítačů Azure](site-recovery-how-to-reprotect.md) po převzetí služeb při selhání.

