---
title: "aaaConfigure mapování sítě pro replikaci virtuálních počítačů Hyper-V v nástroji VMM cloudů tooAzure s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak mapování sítě tooconfigure při replikaci virtuálních počítačů Hyper-V v nástroji VMM cloudů tooAzure s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 081a9fdb0ffa4114099e9bcb9c1b1e43ad26ecbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-configure-network-mapping-for-hyper-v-replication-with-vmm-tooazure"></a>Krok 9: Konfigurace mapování sítě pro tooAzure replikace (s VMM) technologie Hyper-V

Po nastavení hello [zdrojové a cílové nastavení replikace](vmm-to-azure-walkthrough-source-target.md), použijte tento článek tooconfigure sítě mapování toomap mezi místními sítěmi virtuálních počítačů nástroje VMM a sítě Azure.

POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Než začnete

- Další informace o [mapování sítě](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).
- [Příprava mapování sítě VMM](vmm-to-azure-walkthrough-network.md#prepare-vmm-for-network-mapping). 
- Ověřte, že jsou virtuální počítače na serveru VMM hello připojené tooa síť virtuálních počítačů a že jste vytvořili aspoň jednu virtuální síť Azure. Více sítí virtuálních počítačů může být namapované tooa jednu síť Azure.

## <a name="configure-mapping"></a>Konfigurace mapování

Nakonfigurujte mapování následujícím způsobem:

1. V **infrastruktura Site Recovery** > **mapování sítí** > **mapování sítě**, klikněte na tlačítko hello **+ mapování sítě**  ikonu.

    ![Mapování sítě](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping1.png)
2. V **přidání mapování sítě**, vyberte hello zdrojový server VMM, a **Azure** jako cíl hello.
3. Ověřte předplatné hello a hello model nasazení po převzetí služeb při selhání.
4. V **zdrojové síti**vyberte hello zdrojovou místní síť virtuálních počítačů má toomap hello seznamu přidružené k serveru VMM hello.
5. V **Cílová síť**, vyberte hello síť Azure, ve které repliky virtuálních počítačů Azure budou umístěné při vytváří. Pak klikněte na **OK**.

    ![Mapování sítě](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping2.png)

Když se začne mapovat síť, dojde k tomuto:

* Existující virtuální počítače ve zdrojové síti virtuálních počítačů hello jsou připojené toohello cílové síti, když začne mapování. Nová síť virtuálních počítačů připojených toohello zdrojové virtuální počítače jsou připojené toohello namapované síti Azure, když dojde k replikaci.
* Pokud upravíte existující mapování sítě, virtuální počítače repliky se připojují pomocí hello nové nastavení.
* Pokud má hello Cílová síť více podsítí a jedna z těchto podsítí má hello stejný název jako podsíť, na které hello zdrojový virtuální počítač nachází, pak virtuální počítač repliky hello připojí po převzetí služeb při selhání toothat cílové podsíti.
* Pokud není žádná cílová podsíť s odpovídajícím názvem, hello virtuální počítač připojí toohello první podsíť v síti hello.



## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 10: vytvoření zásady replikace](vmm-to-azure-walkthrough-replication.md)
