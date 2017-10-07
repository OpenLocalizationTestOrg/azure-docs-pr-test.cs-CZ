---
title: "aaaPrerequisites pro replikaci tooAzure pomocí Azure Site Recovery | Microsoft Docs"
description: "Tento článek shrnuje požadavky na replikaci virtuálních počítačů a fyzických počítačů tooAzure pomocí služby Azure Site Recovery hello."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: jwhit
editor: tysonn
ms.assetid: e24eea6c-50a7-4cd5-aab4-2c5c4d72ee2d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/01/2017
ms.author: rajanaki
ms.openlocfilehash: c66cea8b097a872bae57e7b42e659e58c4b0b1f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
#  <a name="prerequisites-for-replicating-azure-virtual-machines-tooanother-region-by-using-azure-site-recovery"></a>Požadavky na replikaci oblast tooanother virtuální počítače Azure pomocí Azure Site Recovery

> [!div class="op_single_selector"]
> * [Replikace z Azure tooAzure](site-recovery-azure-to-azure-prereq.md)
> * [Replikace z místní tooAzure](site-recovery-prereq.md)

Hello služba Azure Site Recovery přispívá tooyour provozní kontinuitu a strategie po havárii (BCDR) tím, že orchestruje:
* Replikace virtuálních počítačů Azure tooanother oblast Azure.
* Replikace místní fyzických serverů a virtuálních počítačů tooAzure nebo tooa sekundárního datacentra. 

Pokud dojde k výpadkům ve vašem primárním umístění, můžete převzít tooa sekundárního umístění tookeep aplikace a úlohy, které jsou k dispozici. Primární umístění back tooyour může selhat, až se obnoví toonormal operace. Další informace o Site Recovery najdete v tématu [co je Site Recovery?](site-recovery-overview.md).

Tento článek obsahuje souhrn hello požadavky požadované toobegin Site Recovery replikaci z místní tooAzure.

Odeslat všechny komentáře v dolní části hello hello článku, nebo požádat technické dotazy na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="azure-requirements"></a>Požadavky na Azure

**Požadavek** | **Podrobnosti**
--- | ---
**Účet Azure** | A [Microsoft Azure](http://azure.microsoft.com/) účtu.<br/><br/> Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).
**Služba Site Recovery** | Další informace o cenách za Site Recovery najdete v tématu [cenách za Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/). Doporučujeme vytvořit trezor služeb zotavení v cílové hello oblast Azure, které chcete toouse jako umístění pro obnovení po havárii. Například pokud zdrojové virtuální počítače běží v oblasti Východ USA a chcete, aby tooreplicate tooCentral nám, doporučujeme vytvořit trezor hello v střed USA.|
**Azure kapacity** | Pro cíl hello oblast Azure, který má toouse jako umístění pro obnovení po havárii budete potřebovat toohave předplatné s dostatečnou kapacitu pro virtuální počítače, účty úložiště a síťové součásti. Obraťte se na podporu tooadd víc kapacity.
**Pokynů pro virtualizované úložiště** | Ujistěte se, postupujte podle hello [pokynů pro virtualizované úložiště](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) pro váš zdroj Azure virtuální počítače tooavoid žádné problémy s výkonem. Pokud budete postupovat podle hello výchozí nastavení, Site Recovery vytvoří hello požadované účty úložiště na základě hello zdroj konfigurace. Pokud vlastní nastavení a vyberte vlastní nastavení, ujistěte se, postupujte podle hello [cíle škálovatelnosti pro disky virtuálního počítače](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks).
**Pokyny sítě** | Potřebujete toowhitelist hello odchozí připojení ze svého virtuálního počítače Azure pro konkrétní adresy URL nebo IP adresu rozsahy. Další podrobnosti najdete v části toohello [sítě pokyny pro replikaci virtuálních počítačů Azure](site-recovery-azure-to-azure-networking-guidance.md) článku.
**Virtuální počítač Azure** | Zajistěte, aby byly všechny kořenové certifikáty nejnovější hello přítomna na hello Windows nebo virtuálního počítače s Linuxem. Pokud nejsou zadány hello nejnovější kořenové certifikáty, hello virtuálního počítače nemůže být registrovaný tooSite obnovení z důvodu omezení toosecurity.

>[!NOTE]
>Další informace o podpoře pro konkrétní konfigurace, najdete v tématu hello [matici podpory](site-recovery-support-matrix-azure-to-azure.md).

## <a name="next-steps"></a>Další kroky
- Další informace o [sítě pokyny pro replikaci virtuálních počítačů Azure](site-recovery-azure-to-azure-networking-guidance.md).
- Začněte chránit vaše úlohy [replikovat virtuální počítače Azure](site-recovery-azure-to-azure.md).
