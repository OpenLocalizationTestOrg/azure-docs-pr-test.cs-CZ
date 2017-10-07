---
title: "aaaMigrate tooAzure službou Site Recovery | Microsoft Docs"
description: "Tento článek obsahuje přehled migrace virtuálních počítačů a fyzických serverů tooAzure s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/05/2017
ms.author: raynew
ms.openlocfilehash: 6e65deee337c5371d441812ddb820dc8bc233684
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-tooazure-with-site-recovery"></a>Migrace tooAzure službou Site Recovery

Přečtěte si tento článek Přehled použití služby Azure Site Recovery hello pro migraci virtuálních počítačů a fyzických serverů.

Site Recovery je služba Azure, která přispívá strategie BCDR tooyour, tím, že orchestruje replikaci místní fyzických serverů a virtuálních počítačů toohello cloudu (Azure) nebo tooa sekundárního datacentra. Pokud dojde k výpadkům ve vašem primárním umístění, můžete převzít toohello sekundárního umístění tookeep aplikace a úlohy, které jsou k dispozici. Primární umístění back tooyour nezdaří, až se obnoví toonormal operace. Potřebujete další informace [O službě Site Recovery?](site-recovery-overview.md) Můžete také použít Site Recovery toomigrate stávající místní tooexpedite tooAzure zatížení cloudu cesty a avail hello řadu funkcí, které Azure nabízí.

Rychlý přehled o tooperform migrace naleznete toothis video.
>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/ASRHowTo-Video2-Migrate-Virtual-Machines-to-Azure/player]

Tento článek popisuje nasazení v hello [portál Azure](https://portal.azure.com). Hello [portál Azure classic](https://manage.windowsazure.com/) můžou být použité toomaintain existující trezorů Site Recovery, ale nelze vytvořit nové trezory.

Odeslat všechny komentáře v dolní části hello tohoto článku. Technické dotazy posílejte na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="what-do-we-mean-by-migration"></a>Co myslíme pojmem migrace?

Site Recovery můžete nasadit pro replikaci místní virtuální počítače a fyzické servery, tooAzure nebo tooa sekundární lokality. Replikovat počítače, převzetí služeb při selhání je z primární lokality hello při výpadků dochází a nesplní je back toohello primární lokality, když se obnoví. V přidání toothis můžete použít Site Recovery toomigrate virtuální počítače a fyzické servery tooAzure tak, aby uživatelé mají přístup k jejich jako virtuální počítače Azure. Migrace zahrnuje replikaci a převzetí služeb při selhání z primární lokality tooAzure hello a gesto provedení migrace.

## <a name="what-can-site-recovery-migrate"></a>Co může Site Recovery migrovat?

Můžete:

- Migrujte úlohy běžící na místní virtuální počítače Hyper-V, virtuálních počítačů VMware a fyzické servery, toorun na virtuálních počítačích Azure. V tomto scénáři můžete také provádět úplnou replikaci a navrácení služeb po obnovení.
- Migrovat [virtuální počítače Azure IaaS](site-recovery-migrate-azure-to-azure.md) mezi oblastmi Azure. V tomto scénáři je aktuálně podporovaná pouze migrace, což znamená, že nemůžete navrátit služby po obnovení.
- Migrace [instance AWS Windows](site-recovery-migrate-aws-to-azure.md) tooAzure virtuální počítače IaaS. V tomto scénáři je aktuálně podporovaná pouze migrace, což znamená, že nemůžete navrátit služby po obnovení.

## <a name="migrate-on-premises-vms-and-physical-servers"></a>Migrace místních virtuálních počítačů a fyzických serverů

toomigrate místní virtuální počítače Hyper-V, virtuálních počítačů VMware a fyzické servery, postupujte téměř hello stejné kroky jako používaných pro běžné replikace.

1. Nastavte trezor služby Recovery Services.
2. Konfigurace serverů pro správu vyžaduje hello (VMware, VMM, Hyper-V – v závislosti na tom, co chcete toomigrate), přidejte je toohello trezoru a zadejte nastavení replikace.
3. Povolení replikace pro hello počítače, které chcete toomigrate
4. Po migraci počáteční hello spusťte tooensure Rychlý test převzetí služeb při selhání, který vše funguje podle by měl.
5. Po ověření, že prostředí replikace funguje, použijete plánované nebo neplánované převzetí služeb při selhání v závislosti na tom, [kterou možnost váš scénář podporuje](site-recovery-failover.md). Doporučujeme, že abyste použili plánované převzetí služeb při selhání, pokud je to možné.
6. Pro migraci není nutné toocommit převzetí služeb při selhání nebo ho odstranit. Místo toho vyberte hello **dokončení migrace** možnost pro každý počítač má toomigrate.
     - V **replikované položky**, klikněte pravým tlačítkem na hello virtuálních počítačů a klikněte na **dokončení migrace**. Klikněte na tlačítko **OK** toocomplete. Můžete sledovat průběh ve vlastnostech virtuálního počítače hello v monitorování hello úlohy dokončení migrace v **úlohy Site Recovery**.
     - Hello **dokončení migrace** akci dokončí se proces migrace hello, odebere replikaci pro počítač hello a zastaví Site Recovery fakturace pro počítač hello.

![completemigration](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

## <a name="migrate-between-azure-regions"></a>Migrace mezi oblastmi Azure

Pomocí Site Recovery můžete migrovat virtuální počítače Azure mezi oblastmi. V tomto scénáři je podporována pouze migrace. Jinými slovy můžete replikovat virtuální počítače Azure hello a převzetí služeb při selhání je tooanother oblasti, ale můžete nelze navrácení služeb po obnovení. V tomto scénáři, kterou vytvoříte trezor služeb zotavení nasazení místní konfigurace serveru toomanage replikace, přidejte ho toohello trezoru a zadejte nastavení replikace. Povolíte replikaci pro hello počítače chcete toomigrate a spustit rychlou testovací převzetí služeb při selhání. Potom spustíte neplánované převzetí služeb při selhání se hello **dokončení migrace** možnost.

## <a name="migrate-aws-tooazure"></a>Migrace AWS tooAzure

Můžete migrovat tooAzure instance AWS virtuálních počítačů. V tomto scénáři je podporována pouze migrace. Jinými slovy můžete replikovat hello instance AWS a převzetí služeb při selhání je tooAzure, ale můžete nelze navrácení služeb po obnovení. Instance AWS jsou zpracovávány v hello stejný způsob jako fyzické servery pro účely migrace. Nastavení trezoru služeb zotavení, nasazení místní konfigurace serveru toomanage replikace, přidejte ho toohello trezoru a zadejte nastavení replikace. Povolíte replikaci pro hello počítače chcete toomigrate a spustit rychlou testovací převzetí služeb při selhání. Potom spustíte neplánované převzetí služeb při selhání se hello **dokončení migrace** možnost.




## <a name="next-steps"></a>Další kroky

- [Migrovat virtuální počítače VMware tooAzure](site-recovery-vmware-to-azure.md)
- [Migrace virtuálních počítačů Hyper-V v tooAzure cloudy VMM](site-recovery-vmm-to-azure.md)
- [Migrovat virtuální počítače Hyper-V, aniž by VMM tooAzure](site-recovery-hyper-v-site-to-azure.md)
- [Migrace virtuálních počítačů Azure mezi oblastmi Azure](site-recovery-migrate-azure-to-azure.md)
- [Migrace tooAzure instance AWS](site-recovery-migrate-aws-to-azure.md)
- [Příprava migrované počítače tooenable replikace](site-recovery-azure-to-azure-after-migration.md) tooanother oblast potřebovat obnovení po havárii.
- Začněte chránit svoje úlohy pomocí [replikace virtuálních počítačů Azure](site-recovery-azure-to-azure.md).
