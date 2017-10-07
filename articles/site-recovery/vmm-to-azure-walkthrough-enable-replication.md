---
title: "aaaEnable tooAzure replikace pro virtuální počítače Hyper-V v nástroji VMM cloudů s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak tooenable tooAzure replikace pro virtuální počítače Hyper-V v nástroji VMM cloudů s hello služba Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 89a2c4fc-7e03-4a86-a2c0-52831ccebc1a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 1a39bf65490c59a89a5e891184cadfacc2adee6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-11-enable-replication-tooazure-for-hyper-v-vms-in-vmm-clouds"></a>Krok 11: Povolení tooAzure replikace pro virtuální počítače Hyper-V v cloudech VMM

Poté, co jste nastavili zásadu replikace, použijte tento článek tooenable replikace pro virtuální počítače (VM), místní technologie Hyper-V spravované v cloudech System Center Virtual Machine Manager (VMM)), tooAzure pomocí hello [Azure Site Recovery](site-recovery-overview.md)služby v hello portálu Azure.

POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Než začnete

Ověřte, zda má váš účet Azure hello správné [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)toocreate virtuálních počítačích Azure. [Další informace](../active-directory/role-based-access-built-in-roles.md) o řízení přístupu Azure na základě rolí.

## <a name="exclude-disks-from-replication"></a>Vyloučení disků z replikace

Ve výchozím nastavení jsou všechny disky na počítači replikují. Disky můžete z replikace vyloučit. Například chcete nemusí tooreplicate disků se dočasná data nebo data, která se má aktualizovat pokaždé, když je počítač nebo restartování aplikace (například pagefile.sys nebo databázi tempdb systému SQL Server). [Další informace](site-recovery-exclude-disk.md)

## <a name="replicate-vms"></a>Replikace virtuálních počítačů

Povolení replikace pro virtuální počítače takto:  

1. Klikněte na **Krok 2: Replikujte aplikaci** > **Zdroj**. Po povolení replikace pro hello poprvé, klikněte na tlačítko **+ replikovat** v hello trezoru tooenable replikaci pro další počítače.

    ![Povolení replikace](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication1.png)
2. V hello **zdroj** okně, vyberte hello serveru VMM a hello cloud v které hello technologie Hyper-V jsou umístěni hostitelé. Pak klikněte na **OK**.

    ![Povolení replikace](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication-source.png)
3. V **cíl**, vyberte hello předplatné, model nasazení post-převzetí služeb při selhání, a hello účet úložiště, který používáte pro replikovaná data.

    ![Povolení replikace](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication-target.png)
4. Vyberte účet úložiště hello, že chcete toouse. Pokud chcete, aby toouse jiný účet úložiště než ty, které máte, můžete [vytvořit](#set-up-an-azure-storage-account). Pokud pro replikovaná data používáte účet úložiště premium, je třeba tooselect protokoly další standardní úložiště účet toostore replikace, které zaznamenat data.toocreate probíhající změny tooon místní účet úložiště pomocí modelu Resource Manager hello Klikněte na tlačítko **vytvořit nový**. Pokud chcete účet úložiště pomocí klasického modelu hello toocreate, udělat [v hello portál Azure](../storage/common/storage-create-storage-account.md). Pak klikněte na **OK**.
5. Vyberte hello Azure toowhich síť a podsíť virtuálních počítačů Azure se připojí, když jste vytvořili po převzetí služeb při selhání. Vyberte **nakonfigurovat pro vybrané počítače**, tooapply hello nastavení tooall počítače sítě je vybrat pro ochranu. Vyberte **nakonfigurovat později**, tooselect hello síť Azure pro konkrétní počítač. Pokud chcete, aby toouse jinou síť než ten, který máte, můžete [vytvořit](#set-up-an-azure-network). hello toocreate sítě pomocí modelu Resource Manager, klikněte na **vytvořit nový**. Pokud chcete toocreate síť pomocí klasického modelu hello, udělat [v hello portál Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). V odpovídajícím případě vyberte podsíť. Pak klikněte na **OK**.
6. V **virtuální počítače** > **vybrat virtuální počítače**, klikněte na tlačítko a vyberte každý počítač má tooreplicate. Můžete vybrat pouze počítače, pro které je možné povolit replikaci. Pak klikněte na **OK**.

    ![Povolení replikace](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication5.png)

7. V **vlastnosti** > **konfigurovat vlastnosti**, vyberte hello operační systém pro virtuální počítače hello vybrané a hello disk operačního systému.

    - Ověřte, že hello název virtuálního počítače Azure (název cílové) v souladu s [požadavky na virtuální počítač Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).   
    - Ve výchozím nastavení jsou vybrány všechny disky hello hello virtuálních počítačů pro replikaci, je však možné vyjmout tooexclude disků je.

        - Můžete chtít tooexclude disky tooreduce replikace šířky pásma. Například chcete nemusí tooreplicate disků se dočasná data nebo data, která se má aktualizovat pokaždé, když počítače nebo aplikace se restartuje (například pagefile.sys nebo databázi tempdb systému Microsoft SQL Server). Hello disku můžete z replikace vyloučit unselecting hello disk.
        - Vyloučit můžete jenom základní disky. Disky operačního systému vyloučit nelze.
        - Doporučujeme, abyste nevylučovali dynamické disky. Site Recovery nemůže zjistit, jestli je virtuální pevný disk ve virtuálním počítači hosta základní nebo dynamický. Pokud nejsou všechny disky, závislé dynamický svazek vyloučené, hello chráněné dynamický disk se zobrazí jako poškozený disk při hello virtuálních počítačů při selhání a hello data na tomto disku nebudou přístupné.
        - Po povolení replikace už není možné přidávat nebo odebírat disky pro replikaci. Chcete-li tooadd nebo vyloučit disk, potřebujete toodisable ochranu pro hello virtuálního počítače a pak ji znovu povolte.
        - Disky, které ručně vytvoříte v Azure, nebude možné po navrácení služeb obnovit. Například pokud selhání tři disky a vytvořte dvě přímo ve virtuálním počítači Azure, pouze disky tři hello, které byly převzetí služeb při selhání se nepodařilo zpět z Azure tooHyper-V. Nesmí obsahovat disky ručně vytvořené v navrácení služeb po obnovení, nebo v zpětná replikace z tooAzure technologie Hyper-V.
        - Pokud vyloučíte disk, který je potřeba pro toooperate aplikaci, po převzetí služeb při selhání tooAzure musíte toocreate ručně v Azure, takže tento hello replikované aplikace, mohou spouštět. Alternativně může integrovat Azure automation do plánu obnovení disku hello toocreate během převzetí služeb při selhání hello počítače.

    Klikněte na tlačítko **OK** toosave změny. Později můžete nastavit další vlastnosti.

    ![Povolení replikace](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication6-with-exclude-disk.png)

8. V **nastavení replikace** > **nakonfigurujete nastavení replikace**, vyberte zásadu replikace hello chcete tooapply hello chráněné virtuální počítače. Pak klikněte na **OK**. Můžete změnit zásady replikace hello v **zásady replikace** > název zásady > **upravit nastavení**. Změny, které použijete, se použijí pro počítače, které už replikujete, a nové počítače.

   ![Povolení replikace](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication7.png)

Můžete sledovat průběh hello **povolení ochrany** úlohy v **úlohy** > **úlohy Site Recovery**. Po hello **dokončení ochrany** úloha spustí, počítač hello je připraven na převzetí služeb při selhání.



## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 12: spustit testovací převzetí služeb](vmm-to-azure-walkthrough-test-failover.md)
