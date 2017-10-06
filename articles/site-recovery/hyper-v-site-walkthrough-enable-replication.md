---
title: "aaaEnable replikace pro virtuální počítače Hyper-V replikuje tooAzure (bez System Center VMM) s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky hello potřebujete tooAzure tooenable replikace pro virtuální počítače Hyper-V pomocí služby Azure Site Recovery hello"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: bd31ef01-69f1-4591-a519-e42510a6e2f4
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: 1589cb7aa1fe954e075cb7bf1f4a4ec199ed3ec7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-enable-replication-for-hyper-v-vms-replicating-tooazure"></a>Krok 10: Povolení replikace pro virtuální počítače Hyper-V replikuje tooAzure


Tento článek popisuje, jak tooenable replikace pro místní virtuální počítače Hyper-V (nejsou spravována službou System Center VMM) tooAzure pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.

POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).




## <a name="before-you-start"></a>Než začnete

Než začnete, zajistěte, aby účtu uživatele Azure hello požadované [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikaci nové tooAzure virtuálního počítače.

## <a name="exclude-disks-from-replication"></a>Vyloučení disků z replikace

Ve výchozím nastavení jsou všechny disky na počítači replikují. Disky můžete z replikace vyloučit. Například chcete nemusí tooreplicate disků se dočasná data nebo data, která se má aktualizovat pokaždé, když je počítač nebo restartování aplikace (například pagefile.sys nebo databázi tempdb systému SQL Server). [Další informace](site-recovery-exclude-disk.md)


## <a name="replicate-vms"></a>Replikace virtuálních počítačů

Povolení replikace pro virtuální počítače takto:          

1. Klikněte na tlačítko **replikujte aplikaci** > **zdroj**. Poté, co jste nastavili replikaci hello poprvé, můžete kliknout na **+ replikovat** tooenable replikaci pro další počítače.

    ![Povolení replikace](./media/hyper-v-site-walkthrough-enable-replication/enable-replication.png)
2. V **zdroj**, vyberte lokality hello technologie Hyper-V. Pak klikněte na **OK**.
3. V **cíl**, vyberte předplatné trezoru hello a hello převzetí služeb při selhání modelu chcete toouse v Azure (klasický nebo prostředek management), po převzetí služeb při selhání.
4. Vyberte účet úložiště hello, že chcete toouse. Pokud nemáte účet, který chcete toouse, můžete [vytvořit](#set-up-an-azure-storage-account). Pak klikněte na **OK**.
5. Vyberte hello Azure toowhich síť a podsíť virtuálních počítačů Azure se připojí, když vytváří převzetí služeb při selhání.

    - Vyberte tooapply hello nastavení tooall počítače sítě můžete povolit pro replikaci, **nakonfigurovat pro vybrané počítače**.
    - Vyberte **nakonfigurovat později** tooselect hello síť Azure pro konkrétní počítač.
    - Pokud nemáte síť chcete toouse, můžete [vytvořit](#set-up-an-azure-network). V odpovídajícím případě vyberte podsíť. Pak klikněte na **OK**.

   ![Povolení replikace](./media/hyper-v-site-walkthrough-enable-replication/enable-replication11.png)

6. V **virtuální počítače** > **vybrat virtuální počítače**, klikněte na tlačítko a vyberte každý počítač má tooreplicate. Můžete vybrat pouze počítače, pro které je možné povolit replikaci. Pak klikněte na **OK**.

    ![Povolení replikace](./media/hyper-v-site-walkthrough-enable-replication/enable-replication5-for-exclude-disk.png)

7. V **vlastnosti** > **konfigurovat vlastnosti**, vyberte hello operační systém pro virtuální počítače hello vybrané a hello disk operačního systému.
8. Ověřte, že hello název virtuálního počítače Azure (název cílové) v souladu s [požadavky na virtuální počítač Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
9. Ve výchozím nastavení jsou vybrané všechny disky hello hello virtuálních počítačů pro replikaci. Vymazat disky tooexclude je.
10. Klikněte na tlačítko **OK** toosave změny. Později můžete nastavit další vlastnosti.

    ![Povolení replikace](./media/hyper-v-site-walkthrough-enable-replication/enable-replication6-with-exclude-disk.png)

11. V **nastavení replikace** > **nakonfigurujete nastavení replikace**, vyberte zásadu replikace hello chcete tooapply hello chráněné virtuální počítače. Pak klikněte na **OK**. Můžete změnit zásady replikace hello v **zásady replikace** > název zásady > **upravit nastavení**. Změny, které použijete, se použijí pro počítače, které už replikujete, a nové počítače.


   ![Povolení replikace](./media/hyper-v-site-walkthrough-enable-replication/enable-replication7.png)

Můžete sledovat průběh hello **povolení ochrany** úlohy v **úlohy** > **úlohy Site Recovery**. Po hello **dokončení ochrany** úloha spustí počítač hello je připraven na převzetí služeb při selhání.


## <a name="next-steps"></a>Další kroky


Přejděte příliš[krok 11: spustit testovací převzetí služeb](hyper-v-site-walkthrough-test-failover.md)
