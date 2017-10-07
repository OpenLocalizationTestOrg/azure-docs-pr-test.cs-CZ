---
title: "aaaEnable replikace pro fyzické servery replikace tooAzure s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky hello potřebujete tooAzure tooenable replikace pro fyzické servery pomocí služby Azure Site Recovery hello"
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
ms.openlocfilehash: dde4b1463023d2ccefa498f72bb51e57d60ac0d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-enable-replication-for-physical-servers-tooazure"></a>Krok 10: Povolení replikace pro tooAzure fyzických serverů


Tento článek popisuje, jak tooenable replikace pro místní Windows nebo Linuxem tooAzure fyzických serverů, pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.

POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Než začnete

- Servery musí mít hello [nainstalovat službu mobility](physical-walkthrough-install-mobility.md).
- Virtuální počítač připravený na nabízenou instalaci, hello procesový server automaticky nainstaluje služba Mobility hello při povolení replikace.
- Když povolíte stroj pro replikaci, Azure uživatelský účet potřebuje konkrétní [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooensure jste možnost toouse úložiště Azure a vytvořit virtuální počítače Azure.
- Když přidáváte nebo odebíráte IP adres pro servery, může trvat až too15 minut nebo déle změny tootake účinek a pro jejich tooappear hello portálu.


## <a name="exclude-disks-from-replication"></a>Vyloučení disků z replikace

Ve výchozím nastavení jsou všechny disky na počítači replikují. Disky můžete z replikace vyloučit. Například chcete nemusí tooreplicate disků se dočasná data nebo data, která se má aktualizovat pokaždé, když je počítač nebo restartování aplikace (například pagefile.sys nebo databázi tempdb systému SQL Server). [Další informace](site-recovery-exclude-disk.md)

## <a name="replicate-servers"></a>Replikace serverů

1. Klikněte na **Krok 2: Replikujte aplikaci** > **Zdroj**.
2. V **zdroj**vyberte hello konfigurační server.
3. V **počítač typ**, vyberte **fyzických počítačů**.
4. Vyberte procesový server hello. Pokud jste nevytvořili žádné servery další proces bude hello konfigurační server. Pak klikněte na **OK**.
5. V **cíl**, vyberte předplatné hello a hello skupinu prostředků, ve kterém chcete toocreate hello převzal virtuálních počítačů. Vyberte model nasazení hello, který chcete toouse v Azure (klasický nebo prostředek management), pro hello převzal virtuální počítače.
6. Vyberte účet úložiště Azure hello, že který má toouse pro replikaci dat. Pokud nechcete, aby toouse účet, který jste již nastavili, můžete vytvořit nový.
7. Vyberte hello **síť Azure** a **podsíť** toowhich virtuální počítače Azure po převzetí služeb při selhání připojit. Vyberte **nakonfigurovat pro vybrané počítače** tooapply hello nastavení tooall počítače sítě je vybrat pro ochranu. Vyberte **nakonfigurovat později** tooselect hello síť Azure pro konkrétní počítač. Pokud nechcete, aby toouse existující síť, můžete vytvořit jeden.

    ![Povolení replikace](./media/physical-walkthrough-enable-replication/targetsettings.png)

8. V **fyzické počítače**, klikněte na tlačítko **+ fyzický počítač** a zadejte hello **název** a **IP adresu**. Vyberte hello operační systém počítače hello chcete tooreplicate. Jak dlouho trvá několik minut, dokud nebudou zjištěny a zobrazí v seznamu hello počítače.
9. V **vlastnosti** > **konfigurovat vlastnosti**vyberte hello účet, který se použije hello proces serveru tooautomatically instalaci služby Mobility hello na počítači hello.
10. Ve výchozím nastavení jsou všechny disky replikovat. Klikněte na tlačítko **všechny disky** a zrušte zaškrtnutí všech disků, které nechcete tooreplicate. Pak klikněte na **OK**. Později můžete nastavit další vlastnosti virtuálního počítače.

    ![Povolení replikace](./media/physical-walkthrough-enable-replication/enable-replication6.png)
11. V **nastavení replikace** > **nakonfigurujete nastavení replikace**, ověřte, že hello správné zásady replikace je vybrána. Pokud změníte zásady, změny budou použité tooreplicating počítač a toonew počítače.
12. Povolit **konzistence pro víc Virtuálních** Pokud mají toogather počítače do replikační skupiny a zadejte název pro skupinu hello. Pak klikněte na **OK**. Poznámky:

    * Počítače v replikační skupiny replikovat společně a mít sdílené body obnovení konzistentní při selhání a konzistentní s aplikací, když se převzetí služeb při selhání.
    * Doporučujeme vám, že můžete shromažďování fyzických serverů, aby odpovídaly úlohy. Povolení konzistence více virtuálních počítačů může ovlivnit výkon pracovního vytížení a musí být použit pouze pokud počítače běží hello stejné úlohy a potřebujete konzistence.

13. Klikněte na tlačítko **povolit replikaci**. Můžete sledovat průběh hello **povolení ochrany** úlohy v **nastavení** > **úlohy** > **úlohy Site Recovery**. Po hello **dokončení ochrany** úloha spustí počítač hello je připraven na převzetí služeb při selhání.

## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 11: spustit testovací převzetí služeb](physical-walkthrough-test-failover.md)
