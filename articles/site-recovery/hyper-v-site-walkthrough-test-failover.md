---
title: "aaaRun testovací převzetí služeb při selhání pro Hyper-V tooAzure replikace (bez System Center VMM) | Microsoft Docs"
description: "Shrnuje hello kroky, které je nutné ke spuštění převzetí služeb při selhání pro virtuální počítače Hyper-V replikuje tooAzure pomocí služby Azure Site Recovery hello."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c9a4c3ca-84a0-4668-b700-9b0046202299
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: dbcca080a8fa139e2ea0d132323101dd0f7cd265
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-11-run-a-test-failover-for-hyper-v-replication-tooazure"></a>Krok 11: Spuštění testovací převzetí služeb při selhání pro tooAzure replikace technologie Hyper-V

Tento článek popisuje, jak toorun převzetí služeb při selhání z místní virtuální počítače Hyper-V (nejsou spravována službou System Center VMM) tooAzure pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.

POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Než začnete

Před spuštěním testu převzetí služeb, které doporučujeme ověřit vlastnosti virtuálního počítače hello a proveďte požadované změny potřebujete. můžete přistupovat hello vlastností virtuálního počítače v **replikované položky**. Hello **Essentials** okno zobrazuje informace o nastavení počítače a stav.

## <a name="managed-disk-considerations"></a>Důležité informace o spravovaných disků

[Spravované disky](../virtual-machines/windows/managed-disks-overview.md) zjednodušit správu disku pro virtuální počítače Azure, pomocí správy hello účty úložiště přidružené ke hello disky virtuálních počítačů. 

- Spravované disky jsou vytvořeny a jenom v případě, že dojde k převzetí služeb při selhání tooAzure připojené toohello virtuálních počítačů. Pokud povolíte ochranu, replikuje data z virtuálních počítačů místní toostorage účty.
- Spravované disky se dají vytvořit jenom pro virtuální počítače, které jsou nasazeny pomocí modelu nasazení Resource manager hello.
- Navrácení služeb po obnovení z Azure tooan místního prostředí Hyper-V není aktuálně podporována u počítačů s spravované disky. Měli byste vždy nastavit **použijte spravované disky** příliš**Ano** Pokud provádíte migrace pouze (převzetí služeb při selhání tooAzure bez navrácení služeb po obnovení)
- Toto nastavení povoleno, nastaví pouze dostupnosti ve skupinách prostředků, které mají **použijte spravované disky** povoleno lze vybrat. Virtuální počítače s spravované disky musí být ve skupinách dostupnosti s **použijte spravované disky** nastavit příliš**Ano**. Pokud je nastavení hello není povoleno pro virtuální počítače, lze vybrat pouze skupiny dostupnosti v skupinám prostředků bez spravovaných disků povolená. [Další informace](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).
- - Pokud hello účet úložiště, které používáte pro replikaci byla zašifrována pomocí šifrování služby úložiště, spravovaných disků nelze vytvořit během převzetí služeb při selhání. V takovém případě buď není povolit použití spravovaných disků, nebo zakažte ochranu pro hello virtuálních počítačů a znovu ji povolit toouse účet úložiště, který nemá povolené šifrování. [Další informace](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption).

 
## <a name="network-considerations"></a>Důležité informace o síti
    
- Můžete nastavit hello cílové IP adresy toobe použít pro hello virtuálního počítače Azure po převzetí služeb při selhání. Pokud adresu nezadáte, použije hello převzal počítač DHCP. Pokud nastavíte adresu, která není k dispozici na převzetí služeb při selhání, hello převzetí služeb při selhání se nezdaří. Dobrý den, které lze použít stejnou cílovou IP adresu pro testovací převzetí služeb při selhání, pokud není k dispozici v hello testovací převzetí služeb při selhání sítě hello adresa.
- Hello počet síťových adaptérů závisí hello velikost, který jste zadali pro hello cílový virtuální počítač následujícím způsobem:
    - Pokud hello počet síťových adaptérů na zdrojovém počítači hello je menší než nebo rovna toohello počet adaptérů povoleno pro velikost cílového počítače hello pak bude mít cíl hello hello stejný počet adaptérů jako zdroj hello.
    - Pokud hello počet adaptérů pro hello zdrojový virtuální počítač překračuje počet hello povolený pro cílovou velikost hello pak maximální velikost cíle hello se použije.
    - Například pokud má zdrojový počítač dva síťové adaptéry a velikost hello cílového počítače podporuje čtyři, bude mít hello cílový počítač dva adaptéry. Pokud má hello zdrojový počítač dva adaptéry, ale hello podporovaná velikost cíle podporuje pouze jeden, bude mít cílový počítač hello jenom jeden adaptér.     
- Pokud hello virtuální počítač má několik síťových adaptérů připojí se všechny toohello stejné síti.
- Pokud hello virtuální počítač více síťových adaptérů pak hello jeden uvedené v seznamu hello se stal hello *výchozí* síťový adaptér v hello virtuální počítač Azure.


## <a name="view-and-manage-vm-settings"></a>Zobrazit a spravovat nastavení virtuálního počítače

Doporučujeme ověřit vlastnosti hello hello zdrojového počítače před spuštěním převzetí služeb při selhání.

1. V **chráněné položky**, klikněte na tlačítko **replikované položky**a klikněte na tlačítko hello virtuálních počítačů.

    ![Povolení replikace](./media/hyper-v-site-walkthrough-test-failover/test-failover1.png)
2. V hello **replikované položky** podokně, zobrazí se souhrn informací, stav a hello nejnovější dostupné body obnovení virtuálního počítače. Klikněte na tlačítko **vlastnosti** tooview více podrobností.

    ![Povolení replikace](./media/hyper-v-site-walkthrough-test-failover/test-failover2.png)
3. V **výpočty a síť**, můžete:
    - Změnit název virtuálního počítače Azure hello. musí splňovat Hello název [požadavky pro Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
    - Zadejte post-převzetí služeb při selhání [skupiny prostředků].
    - Zadejte cílovou velikost pro hello virtuálního počítače Azure
    - Vyberte [skupinu dostupnosti](../virtual-machines/windows/tutorial-availability-sets.md).
    - Určit, zda toouse [discích spravovaných](#managed-disk-considerations). Vyberte **Ano**, pokud chcete počítač tooyour tooattach spravovaných disků na tooAzure migrace.
    - Zobrazit nebo upravit nastavení sítě, včetně hello sítě a podsítě, ve které hello virtuálního počítače Azure budou umístěné po převzetí služeb při selhání a hello IP adresu, která bude přiřazena tooit.

    ![Povolení replikace](./media/hyper-v-site-walkthrough-test-failover/test-failover4.png)
4. V **disky**, se zobrazí informace o hello operačního systému a datové disky na hello virtuálních počítačů.


## <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání

Teď spusťte toomake testovací převzetí služeb při selhání, se, že vše funguje podle očekávání.

- Pokud chcete, aby tooconnect tooAzure virtuálních počítačů pomocí protokolu RDP po převzetí služeb při selhání, [Příprava tooconnect](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
 - test toofully potřebujete toocopy služby Active Directory a DNS v testovacím prostředí. [Další informace](site-recovery-active-directory.md#test-failover-considerations).
 - Úplné informace o převzetí služeb při selhání, přečtěte si [v tomto článku](site-recovery-test-failover-to-azure.md) článku.
 
 Nyní spusťte převzetí služeb při selhání:

1. toofail přes jeden počítač, v **replikované položky**, klikněte na tlačítko hello virtuálního počítače > **+ testovací převzetí služeb při selhání** ikonu.
2. plánování toofail přes obnovení, **plány obnovení**, klikněte pravým tlačítkem na hello plán > **testovací převzetí služeb při selhání**. plán obnovení toocreate [postupujte podle těchto pokynů](site-recovery-create-recovery-plans.md).
3. V **testovací převzetí služeb při selhání**, vyberte síť Azure toowhich hello virtuální počítače Azure připojí po převzetí služeb při selhání.
4. Klikněte na tlačítko **OK** toobegin hello převzetí služeb při selhání. Průběh můžete sledovat, když kliknete na virtuální počítač tooopen hello jeho vlastnosti, nebo na hello **testovací převzetí služeb při selhání** úlohy v název trezoru > **úlohy** > **úlohy Site Recovery**.
5. Po dokončení převzetí služeb při selhání hello, měli byste také mít možnost toosee hello repliky počítač Azure se zobrazí v hello portálu Azure > **virtuální počítače**. Měli byste si ověřit, že hello virtuálního počítače je hello odpovídající velikost, byl připojený toohello příslušnou síť, a zda je spuštěna.
6. Pokud jste připravili připojení po převzetí služeb při selhání, musí být schopný tooconnect toohello virtuálního počítače Azure.
7. Až dokončíte, klikněte na **vyčistit testovací převzetí služeb při selhání** v plánu obnovení hello. V **poznámky** zaznamenejte a uložte jakékoli připomínky související s hello testovací převzetí služeb při selhání. Tato akce odstraní hello virtuálních počítačů, které byly vytvořeny během testovacího převzetí služeb při selhání.



## <a name="next-steps"></a>Další kroky

- [Další informace](site-recovery-failover.md) o různých typech převzetí služeb při selhání a jak toorun je.
- [Přečtěte si informace o navrácení služeb po obnovení](site-recovery-failback-from-azure-to-hyper-v.md), toofail zpět a replikaci virtuálních počítačů Azure zpět toohello primární místní technologie Hyper-V lokalitě.

