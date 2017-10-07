---
title: "tooa replikace aaaEnable technologie Hyper-V s Azure Site Recovery sekundární lokality. | Microsoft Docs"
description: "Popisuje, jak tooenable replikace pro virtuální počítače Hyper-V replikuje tooa sekundární System Center VMM lokalitu s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d8782d14-9fef-4396-8912-ff945efc851b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: b4484e0118cb23f343187fe867d9795d30926baf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-enable-replication-tooa-secondary-site-for-hyper-v-vms"></a>Krok 9: Povolení sekundární lokality tooa replikace pro virtuální počítače Hyper-V


Po nastavení zásad replikace, tento článek tooenable replikace tooa sekundární System Center Virtual Machine Manager (VMM) web použít pro místní technologie Hyper-V virtuální počítače (VM), pomocí [Azure Site Recovery](site-recovery-overview.md).

Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## <a name="select-vms-tooreplicate"></a>Vyberte tooreplicate virtuální počítače

1. Klikněte na **Krok 2: Replikujte aplikaci** > **Zdroj**. 

    ![Povolení replikace](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication1.png)

2. V **zdroj**, vyberte hello VMM server a hello cloudu, ve které hello hostitelů Hyper-V, které chcete tooreplicate nacházejí. Pak klikněte na **OK**.

    ![Povolení replikace](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication2.png)
3. V **cíl**, ověřte hello sekundární server VMM a cloud.
4. V **virtuální počítače**, vyberte virtuální počítače hello chcete tooprotect hello seznamu.

    ![Povolení ochrany virtuálního počítače](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication5.png)

Můžete sledovat průběh hello **povolení ochrany** akce v **úlohy** > **úlohy Site Recovery**. Po hello **dokončení ochrany** dokončení úlohy, hello počáteční replikace je dokončena a hello virtuálního počítače je připravený k převzetí služeb při selhání.

Poznámky:

- Můžete také povolit ochranu pro virtuální počítače v konzole VMM hello. Klikněte na tlačítko **povolení ochrany** na panelu nástrojů hello ve vlastnostech virtuálního počítače hello > **Azure Site Recovery** kartě.
- Po povolení replikace, můžete zobrazit vlastnosti pro hello virtuálních počítačů v **replikované položky**. Na hello **Essentials** řídicího panelu, zobrazí se informace o zásadách replikace hello hello virtuálního počítače a jeho stav. Klikněte na tlačítko **vlastnosti** další podrobnosti.

## <a name="onboard-existing-vms"></a>Zařadit stávající virtuální počítače

Pokud máte existující virtuální počítače v nástroji VMM, které jsou replikace s replikou technologie Hyper-V, můžete připojit je pro Azure Site Recovery replikaci následujícím způsobem:

1. Zkontrolujte, zda server hello technologie Hyper-V, který hostuje hello existující virtuální počítač se nachází v primárním cloudu hello a tento server technologie Hyper-V hello hostování virtuální počítač repliky hello je umístěný v cloudu sekundární hello.
2. Zajistěte, aby že zásady replikace je nakonfigurován pro hello primárního cloudu VMM.
3. Povolení replikace pro primární virtuální počítač hello. Azure Site Recovery a VMM zkontrolujte, zda hello stejného hostitele repliky a virtuální počítač je zjištěna a bude používat Azure Site Recovery a obnovit replikaci s využitím hello zadané nastavení.


## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 10: spustit testovací převzetí služeb](vmm-to-vmm-walkthrough-test-failover.md).
