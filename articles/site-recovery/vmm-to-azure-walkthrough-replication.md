---
title: "aaaSet se zásada replikace pro virtuální počítač Hyper-V (s nástrojem VMM) tooAzure replikace s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak tooset se zásada replikace pro virtuální počítač Hyper-V (s nástrojem VMM) tooAzure replikace s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ee808b20-324b-4198-b831-edb65b95e8b7
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: e1579fde559ca34eca19a01e740fec28a0df2f9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-set-up-a-replication-policy-for-hyper-v-vm-replication-with-vmm-tooazure"></a>Krok 10: Nastavení zásad replikace pro virtuální počítač Hyper-V tooAzure replikace (s VMM)


Po nastavení [mapování sítě](vmm-to-azure-walkthrough-network-mapping.md), použijte tento článek tooconfigure zásadu replikace T\tooreplicate místní virtuální počítače Hyper-V spravované v cloudech tooAzure System Center Virtual Machine Manager (VMM), pomocí hello [ Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.

Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## <a name="create-a-policy"></a>Vytvořit zásadu

1. Klikněte na tlačítko toocreate novou zásadu replikace, **připravit infrastrukturu** > **nastavení replikace** > **+ vytvořit a přidružit**.

    ![Síť](./media/vmm-to-azure-walkthrough-replication/gs-replication.png)
2. V části **Vytvořit a přidružit zásady** zadejte název zásady.
3. V **frekvence kopírování**, určete, jak často má tooreplicate rozdílová data po hello počáteční replikaci (každých 30 sekund, 5 nebo 15 minut).

    > [!NOTE]
    >  30 druhý frekvence nepodporuje při replikaci toopremium úložiště. omezení Hello je určen podle hello počet snímků za blob (100) nepodporuje storage úrovně premium. [Další informace](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob)

4. V **uchování bodu obnovení**, zadejte v hodinách, jak dlouho bude hello intervalem pro uchovávání dat se pro každý bod obnovení. Chráněné počítače může být obnovena tooany bodu v rámci časového období.
5. V nastavení **Frekvence snímků konzistentní vzhledem k aplikacím** určete, jak často (1–12 hodin) se mají vytvářet body obnovení obsahující snímky konzistentní vzhledem k aplikacím. Technologie Hyper-V používá dva typy snímků – standardní snímek, což je přírůstkový snímek celého virtuálního počítače hello a snímky konzistentní s aplikací, která přebírá v okamžiku snímek dat aplikací hello uvnitř hello virtuálního počítače. Snímky konzistentní s aplikacemi pomocí tooensure služby Stínová kopie svazku (VSS), které aplikace budou při pořízení snímku hello v konzistentním stavu. Všimněte si, že pokud povolíte snímky konzistentní s aplikacemi, bude mít vliv hello výkon aplikací běžících na zdrojových virtuálních počítačích. Zajistěte, aby byl hello hodnoty, které nastavíte, menší než počet hello další body obnovení, které nakonfigurujete.
6. V **čas spuštění počáteční replikace**, indikovat, kdy toostart hello počáteční replikace. Hello replikace se provede přes pásma vašeho internetového připojení, můžete chtít tooschedule ho dobu mimo špičky.
7. V **šifrovat data uložená v Azure**, určit, zda tooencrypt neaktivní data v úložišti Azure. Pak klikněte na **OK**.

    ![Zásady replikace](./media/vmm-to-azure-walkthrough-replication/gs-replication2.png)
8. Když vytvoříte novou zásadu je přidružená automaticky hello cloudu VMM. Klikněte na **OK**. K této zásadě replikace můžete přidružit další cloudy VMM (a hello virtuální počítače v nich) **replikace** > název zásady > **přidružit Cloud VMM**.

    ![Zásady replikace](./media/vmm-to-azure-walkthrough-replication/policy-associate.png)



## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 11: povolení replikace](vmm-to-azure-walkthrough-enable-replication.md)
