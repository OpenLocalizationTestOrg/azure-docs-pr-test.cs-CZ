---
title: "aaaCreate trezoru pro tooa replikace technologie Hyper-V s Azure Site Recovery sekundární lokality. | Microsoft Docs"
description: "Popisuje, jak toocreate trezoru při replikaci virtuálních počítačů Hyper-V tooa sekundární System Center VMM lokalitu s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ff65dbfb-cb26-410e-ab48-76971625db08
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 96ee09cbf2376a5089b9efa09dc7ab3fb7d472cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-create-a-vault-for-hyper-v-replication-tooa-secondary-site"></a>Krok 5: Vytvoření trezoru pro sekundární lokalitě tooa replikace Hyper-V

Jakmile připravíte místní [servery System Center Virtual Machine Manager (VMM) a hostitele nebo Clustery Hyper-V](vmm-to-vmm-walkthrough-vmm-hyper-v.md) pro sekundární lokalitě tooa replikace technologie Hyper-V pomocí [Azure Site Recovery](site-recovery-overview.md), můžete vytvořit Trezor služeb zotavení a vyberte hello replikace scénář.

Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="choose-a-protection-goal"></a>Vyberte cíl ochrany

Vyberte, co chcete tooreplicate a místo, kam chcete tooreplicate k.

1. Klikněte na tlačítko **Site Recovery** > **krok 1: připravte infrastrukturu** > **cíl ochrany**.
2. Vyberte **toorecovery lokality**a vyberte **Ano, s technologií Hyper-V**.
3. Vyberte **Ano** tooindicate používáte hostitelů nástroje VMM toomanage hello technologie Hyper-V.
4. Vyberte **Ano** Pokud máte sekundární server VMM. Pokud nasazujete replikace mezi cloudy na jednom serveru VMM, klikněte na tlačítko **ne**. Pak klikněte na **OK**.

    ![Zvolte cíle.](./media/vmm-to-vmm-walkthrough-create-vault/choose-goals.png)



## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 6: nastavení hello replikace zdrojové a cílové](vmm-to-vmm-walkthrough-source-target.md).
