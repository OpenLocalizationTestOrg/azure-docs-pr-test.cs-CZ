---
title: "Migrace virtuálních počítačů Azure IaaS mezi oblastmi Azure | Microsoft Docs"
description: "Pomocí Azure Site Recovery k migraci virtuálních počítačů Azure IaaS z jedné oblasti Azure do jiné."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8a29e0d9-0010-4739-972f-02b8bdf360f6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: ef2972c077a2b1dd2b2fd6ce53cc6560520ea870
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>Migrovat virtuální počítače Azure IaaS mezi oblastmi Azure s Azure Site Recovery
## <a name="overview"></a>Přehled
Vítejte v Azure Site Recovery! Tento článek použijte, pokud chcete migrovat virtuální počítače Azure mezi oblastmi Azure. Než začnete, Všimněte si, že:

* Azure má dva různé modely nasazení pro vytváření a práci s prostředky: Azure Resource Manager a Klasický model. Azure má také dva portály – portál Azure Classic, který podporuje klasický model nasazení, a portál Azure s podporou pro oba modely nasazení. Základní kroky pro migraci jsou stejné, zda konfigurujete Site Recovery ve Správci prostředků nebo classic. Ale pokyny uživatelského rozhraní a snímky obrazovky v tomto článku se vztahují k portálu Azure.
* **Aktuálně jste můžete migrovat pouze z jedné oblasti do jiné. Můžete převzít virtuálních počítačů z jedné oblasti Azure do jiné, ale nemůžete je po obnovení zpět znovu.**

Jakékoli dotazy nebo připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Požadavky
Zde je nutné pro toto nasazení:

* **Virtuální počítače IaaS**: virtuálních počítačů, které chcete migrovat. Tyto virtuální počítače migrujete tak, že je považuje jako fyzické počítače.

## <a name="deployment-steps"></a>Kroky nasazení
Tato část popisuje kroky nasazení nového portálu Azure.

1. [Vytvoření trezoru](site-recovery-vmware-to-azure.md).
2. [Povolit replikaci](site-recovery-vmware-to-azure.md). Povolení replikace pro virtuální počítače, které chcete migrovat a zvolte Azure jako zdroj. 
3. [Spustit neplánované převzetí služeb při selhání](site-recovery-failover.md). Po dokončení počáteční replikace můžete spustit neplánované převzetí služeb při selhání z jedné oblasti Azure do jiné. Volitelně můžete vytvořit plán obnovení a spusťte neplánované převzetí služeb při selhání, migrovat několik virtuálních počítačů mezi oblastmi. [Další informace](site-recovery-create-recovery-plans.md) o plány obnovení.

## <a name="next-steps"></a>Další kroky
Další informace o jiných scénářích replikace v [co je Azure Site Recovery?](site-recovery-overview.md)
