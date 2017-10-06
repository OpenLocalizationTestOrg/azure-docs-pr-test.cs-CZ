---
title: "aaaSet se zásada replikace pro virtuální počítač Hyper-V tooAzure replikace (bez System Center VMM) s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky hello potřebujete tooset si zásadu replikace při replikaci virtuálních počítačů Hyper-V tooAzure úložiště"
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 1777e0eb-accb-42b5-a747-11272e131a52
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: 4bd7161f4a0f015da0ecf595fbc6861ede5d68b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-set-up-a-replication-policy-for-hyper-v-vm-replication-tooazure"></a>Krok 9: Nastavení zásad replikace pro tooAzure replikace virtuálního počítače technologie Hyper-V

Tento článek popisuje, jak tooset si zásadu replikace při replikaci tooAzure virtuálních počítačů Hyper-V (bez System Center VMM) pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.


POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="about-snapshots"></a>O snímky

Technologie Hyper-V používá dva typy snímků – standardní snímek, což je přírůstkový snímek celého virtuálního počítače hello a snímky konzistentní s aplikací, která přebírá v okamžiku snímek dat aplikací hello uvnitř hello virtuálního počítače.
    - Snímky konzistentní s aplikacemi pomocí tooensure služby Stínová kopie svazku (VSS), které aplikace budou při pořízení snímku hello v konzistentním stavu.
    - Pokud povolíte snímky konzistentní s aplikacemi, ovlivní hello výkon aplikací běžících na zdrojových virtuálních počítačích. Zajistěte, aby byl hello hodnoty, které nastavíte, menší než počet hello další body obnovení, které nakonfigurujete.

## <a name="set-up-a-replication-policy"></a>Nastavení zásad replikace

1. Klikněte na tlačítko toocreate novou zásadu replikace, **připravit infrastrukturu** > **nastavení replikace** > **+ vytvořit a přidružit**.

    ![Síť](./media/hyper-v-site-walkthrough-replication/gs-replication.png)
2. V části **Vytvořit a přidružit zásady** zadejte název zásady.
3. V **frekvence kopírování**, určete, jak často má tooreplicate rozdílová data po hello počáteční replikaci (každých 30 sekund, 5 nebo 15 minut).

    > [!NOTE]
    > 30 druhý frekvence nepodporuje při replikaci toopremium úložiště. omezení Hello je určen podle hello počet snímků za blob (100) nepodporuje storage úrovně premium. [Další informace](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob).

4. V **uchování bodu obnovení**, zadejte v hodinách, jak dlouho hello intervalem pro uchovávání dat je pro každý bod obnovení. Virtuální počítače mohou být obnovena tooany bodu v rámci časového období.
5. V **frekvence snímkování konzistentní aplikace vzhledem**, zadejte, jak často (1 – 12 hodin) body obnovení obsahující snímky konzistentní s aplikacemi jsou vytvořeny.
6. V **čas spuštění počáteční replikace**, zadejte, kdy toostart hello počáteční replikace. Hello replikace se provede přes pásma vašeho internetového připojení, můžete chtít tooschedule ho dobu mimo špičky. Pak klikněte na **OK**.

    ![Zásady replikace](./media/hyper-v-site-walkthrough-replication/gs-replication2.png)

Když vytvoříte novou zásadu, je automaticky přidružená hello web Hyper-V. Budete moct přidružit k víc zásad replikace v lokalitě Hyper-V (a hello virtuálních počítačů v ní) **replikace** > název zásady > **přidružit web Hyper-V**.



## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 10: povolení replikace](hyper-v-site-walkthrough-enable-replication.md)
