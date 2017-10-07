---
title: "aaaEnable replikace pro virtuální počítače VMware replikace tooAzure s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky hello nutné tooAzure tooenable replikace pro virtuální počítače VMware pomocí služby Azure Site Recovery hello"
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 519c5559-7032-4954-b8b5-f24f5242a954
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 490782bbbfa3dd92c626d3985c75d771df53d566
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-11-enable-replication-for-vmware-virtual-machines-tooazure"></a>Krok 11: Povolení replikace pro virtuální počítače tooAzure VMware


Tento článek popisuje, jak tooenable replikace pro VMware místní virtuální počítače tooAzure pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.

POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Než začnete

- Virtuální počítače VMware musí mít hello [nainstalovat službu mobility](vmware-walkthrough-install-mobility.md). – Pokud je virtuální počítač připravený na nabízenou instalaci, hello procesový server automaticky nainstaluje služba Mobility hello při povolení replikace.
- Azure uživatelský účet potřebuje konkrétní [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikaci tooAzure virtuálních počítačů
- Když přidáváte nebo odebíráte virtuálních počítačů, může trvat až too15 minut nebo déle pro změny tootake účinek a jejich tooappear hello portálu.
- Čas poslední zjištěné hello můžete zkontrolovat pro virtuální počítače v **konfigurační servery** > **poslední obraťte se na**.
- tooadd virtuálních počítačů bez čekání na hello naplánovaného zjišťování, zvýraznění hello konfigurační server (nemáte klikněte na něj) a klikněte na tlačítko **aktualizovat**.



## <a name="exclude-disks-from-replication"></a>Vyloučení disků z replikace

Ve výchozím nastavení jsou všechny disky na počítači replikují. Disky můžete z replikace vyloučit. Například chcete nemusí tooreplicate disků se dočasná data nebo data, která se má aktualizovat pokaždé, když je počítač nebo restartování aplikace (například pagefile.sys nebo databázi tempdb systému SQL Server). [Další informace](site-recovery-exclude-disk.md)

## <a name="replicate-vms"></a>Replikace virtuálních počítačů

Než začnete, podívejte se na rychlé video s přehledem

>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video3-Protect-VMware-Virtual-Machines/player]

1. Klikněte na **Krok 2: Replikujte aplikaci** > **Zdroj**.
2. V **zdroj**vyberte hello konfigurační server.
3. V **počítač typ**, vyberte **virtuální počítače**.
4. V **vCenter vSphere Hypervisor**, vyberte hello vCenter server, který spravuje hostitelů vSphere hello nebo vyberte hello hostitele.
5. Vyberte procesový server hello. Pokud jste nevytvořili žádné servery další proces bude hello konfigurační server. Pak klikněte na **OK**.

    ![Povolení replikace](./media/vmware-walkthrough-enable-replication/enable-replication2.png)

6. V **cíl**, vyberte předplatné hello a hello skupinu prostředků, ve kterém chcete toocreate hello převzal virtuálních počítačů. Vyberte model nasazení hello, který chcete toouse v Azure (klasický nebo prostředek management), pro hello převzal virtuální počítače.


7. Vyberte účet úložiště Azure hello, že který má toouse pro replikaci dat. Pokud nechcete, aby toouse účet, který jste již nastavili, můžete vytvořit nový.

8. Vyberte hello Azure toowhich síť a podsíť virtuálních počítačů Azure se připojí, když jste vytvořili po převzetí služeb při selhání. Vyberte **nakonfigurovat pro vybrané počítače**, tooapply hello nastavení tooall počítače sítě je vybrat pro ochranu. Vyberte **nakonfigurovat později** tooselect hello síť Azure pro konkrétní počítač. Pokud nechcete, aby toouse existující síť, můžete vytvořit jeden.

    ![Povolení replikace](./media/vmware-walkthrough-enable-replication/enable-rep3.png)
9. V **virtuální počítače** > **vybrat virtuální počítače**, klikněte na tlačítko a vyberte každý počítač má tooreplicate. Můžete vybrat pouze počítače, pro které je možné povolit replikaci. Pak klikněte na **OK**.

    ![Povolení replikace](./media/vmware-walkthrough-enable-replication/enable-replication5.png)
10. V **vlastnosti** > **konfigurovat vlastnosti**vyberte hello účet, který se použije hello proces serveru tooautomatically instalaci služby Mobility hello na počítači hello.
11. Ve výchozím nastavení jsou všechny disky replikovat. Klikněte na tlačítko **všechny disky** a zrušte zaškrtnutí všech disků, které nechcete tooreplicate. Pak klikněte na **OK**. Později můžete nastavit další vlastnosti virtuálního počítače.

    ![Povolení replikace](./media/vmware-walkthrough-enable-replication/enable-replication6.png)
11. V **nastavení replikace** > **nakonfigurujete nastavení replikace**, ověřte, že hello správné zásady replikace je vybrána. Pokud změníte zásady, změny budou použité tooreplicating počítač a toonew počítače.
12. Povolit **konzistence pro víc Virtuálních** Pokud mají toogather počítače do replikační skupiny a zadejte název pro skupinu hello. Pak klikněte na **OK**. Poznámky:

    * Počítače v replikační skupiny replikovat společně a mít sdílené body obnovení konzistentní při selhání a konzistentní s aplikací, když se převzetí služeb při selhání.
    * Doporučujeme vám, že shromáždíte virtuálních počítačů a fyzických serverů společně, aby odpovídaly vašich úloh. Povolení konzistence více virtuálních počítačů může ovlivnit výkon pracovního vytížení a musí být použit pouze pokud počítače běží hello stejné úlohy a potřebujete konzistence.

    ![Povolení replikace](./media/vmware-walkthrough-enable-replication/enable-replication7.png)
13. Klikněte na tlačítko **povolit replikaci**. Můžete sledovat průběh hello **povolení ochrany** úlohy v **nastavení** > **úlohy** > **úlohy Site Recovery**. Po hello **dokončení ochrany** úloha spustí počítač hello je připraven na převzetí služeb při selhání.

## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 12: spustit testovací převzetí služeb](vmware-walkthrough-test-failover.md)
