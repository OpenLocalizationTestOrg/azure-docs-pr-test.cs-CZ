---
title: "aaaMap sítě pro virtuální počítač Hyper-V replikace tooa sekundární lokalitu s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak toomap sítě při replikaci virtuálních počítačů Hyper-V tooa sekundární lokalita VMM s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 461b7c1c-ef61-4005-aeec-2324e727a3d0
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: d4f621df4ce08ae055bc6809daea0b71b76754ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-map-networks-for-hyper-v-vm-replication-tooa-secondary-site"></a>Krok 7: Mapování sítě pro virtuální počítač Hyper-V replikace tooa sekundární lokality


Po nastavení [zdrojové a cílové nastavení](vmm-to-vmm-walkthrough-source-target.md) pro replikaci technologie Hyper-V virtuální počítače (VM) tooa sekundární System Center Virtual Machine Manager (VMM) lokality, použijte toto mapování článku tooconfigure sítě pro virtuální technologii Hyper-V počítač (VM) replikace tooa sekundární lokality, pomocí [Azure Site Recovery](site-recovery-overview.md).

Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Než začnete

- Další informace o [mapování sítě](vmm-to-vmm-walkthrough-network.md#network-mapping-overview) před zahájením.
- Ověřte, zda jsou virtuální počítače na serverech VMM připojené tooa síť virtuálních počítačů.

## <a name="configure-network-mapping"></a>Konfigurace mapování sítě

1. V **mapování sítě** > **mapování sítí**, klikněte na tlačítko **+ mapování sítě**.
2. V **přidání mapování sítě** vyberte hello zdrojové a cílové servery VMM. Hello sítě virtuálních počítačů spojené s servery VMM hello jsou načteny.
3. V **zdrojové síti**vyberte hello sítě chcete toouse ze seznamu hello sítě virtuálních počítačů spojené s primárním serverem VMM hello.
4. V **Cílová síť**vyberte hello sítě chcete toouse na sekundárním serveru VMM hello. Pak klikněte na **OK**.

    ![Mapování sítě](./media/vmm-to-vmm-walkthrough-network-mapping/network-mapping2.png)

Když se začne mapovat síť, dojde k tomuto:

* Všechny existující repliky virtuálních počítačů, které odpovídají zdrojové síti virtuálních počítačů toohello bude síť virtuálních počítačů připojených toohello cíl.
* Nové virtuální počítače, které jsou připojené toohello zdrojové síti virtuálních počítačů bude po replikaci připojené toohello cíl namapované síťové.
* Pokud upravíte existující mapování u nové sítě, virtuální počítače repliky připojí pomocí nového nastavení hello.
* Pokud hello Cílová síť více podsítí a jedna z těchto podsítí má hello nachází stejný název jako podsíť, na které hello zdrojového virtuálního počítače a potom virtuální počítač repliky hello budou po převzetí služeb při selhání připojené toothat cílové podsíti. Pokud není žádná cílová podsíť s odpovídajícím názvem, hello virtuální počítač bude připojený toohello první podsíť v síti hello.



## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 8: Nakonfigurujte zásady replikace](vmm-to-vmm-walkthrough-replication.md).
