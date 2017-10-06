---
title: "virtuální počítače Azure IaaS aaaMigrate mezi oblastmi Azure | Microsoft Docs"
description: "Použijte virtuální počítače Azure IaaS Azure Site Recovery toomigrate z jedné oblasti Azure tooanother."
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
ms.openlocfilehash: c84dc77716b8d19969eab60707ed1332ca39b893
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>Migrovat virtuální počítače Azure IaaS mezi oblastmi Azure s Azure Site Recovery
## <a name="overview"></a>Přehled
Vítejte tooAzure Site Recovery! Tento článek použijte, pokud chcete, aby virtuální počítače Azure toomigrate mezi oblastmi Azure. Než začnete, Všimněte si, že:

* Azure má dva různé modely nasazení pro vytváření a práci s prostředky: Azure Resource Manager a Klasický model. Azure má také dva portály – portál Azure classic, která podporuje model nasazení classic hello hello a hello portál Azure s podporou pro oba modely nasazení. Hello základní kroky pro migraci jsou hello stejné zda konfigurujete Site Recovery ve Správci prostředků nebo classic. Ale hello pokyny uživatelského rozhraní a snímky obrazovky v tomto článku jsou relevantní pro hello portálu Azure.
* **Aktuálně je můžete migrovat pouze z jedné oblasti tooanother. Můžete převzít virtuální počítače z jedné oblasti Azure tooanother, ale nemůžete je po obnovení zpět znovu.**

POST jakékoli dotazy nebo připomínky v hello dolní části tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Požadavky
Zde je nutné pro toto nasazení:

* **Virtuální počítače IaaS**: hello chcete toomigrate virtuálních počítačů. Tyto virtuální počítače migrujete tak, že je považuje jako fyzické počítače.

## <a name="deployment-steps"></a>Kroky nasazení
Tato část popisuje postup nasazení hello hello novou verzi portálu Azure.

1. [Vytvoření trezoru](site-recovery-vmware-to-azure.md).
2. [Povolit replikaci](site-recovery-vmware-to-azure.md). Povolení replikace pro virtuální počítače chcete toomigrate a zvolte Azure jako zdroj hello. 
3. [Spustit neplánované převzetí služeb při selhání](site-recovery-failover.md). Po dokončení počáteční replikace můžete spustit neplánované převzetí služeb při selhání z jedné oblasti Azure tooanother. Volitelně můžete vytvořit plán obnovení a spustit více virtuálních počítačů neplánované převzetí služeb při selhání, toomigrate mezi oblastmi. [Další informace](site-recovery-create-recovery-plans.md) o plány obnovení.

## <a name="next-steps"></a>Další kroky
Další informace o jiných scénářích replikace v [co je Azure Site Recovery?](site-recovery-overview.md)
