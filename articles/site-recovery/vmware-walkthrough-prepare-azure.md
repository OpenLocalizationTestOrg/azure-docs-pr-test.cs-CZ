---
title: "prostředky Azure tooreplicate aaaPrepare místní virtuální počítače VMware tooAzure pomocí Azure Site Recovery | Microsoft Docs"
description: "Popisuje, co potřebujete zavedené v Azure než začnete replikovat místní virtuální počítače VMware tooAzure pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 4e320d9b-8bb8-46bb-ba21-77c5d16748ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: ac72fff0593783add789408ecfeb1812d70108b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-prepare-azure-resources-for-vmware-replication-tooazure"></a>Krok 5: Příprava prostředků Azure pro tooAzure replikace VMWare


Použijte hello pokyny v této článku tooprepare Azure prostředky tak, aby můžete replikovat místní počítače tooAzure pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby.

POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Než začnete

Ujistěte se, že jste si přečetli hello [požadavky](vmware-walkthrough-prerequisites.md)

## <a name="set-up-an-azure-account"></a>Nastavit účet Azure

- Získání [účet Microsoft Azure](http://azure.microsoft.com/).
- Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).
- Zkontrolujte hello podporované oblasti pro obnovení lokality v rámci geografickou dostupností v [Azure Site Recovery podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).
- Další informace o [cenách za Site Recovery](site-recovery-faq.md#pricing)a získat hello [podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).



## <a name="set-up-an-azure-network"></a>Nastavení sítě Azure

- Nastavte síť Azure. Virtuální počítače Azure jsou umístěny v této síti, když jste vytvořili po převzetí služeb při selhání.
- Site Recovery na hello portálu Azure můžete použít nastavení sítích [Resource Manager](../resource-manager-deployment-model.md), nebo v klasickém režimu.
- Hello síť musí být ve hello stejné oblasti jako hello trezoru služeb zotavení
- Další informace o [ceny virtuální sítě](https://azure.microsoft.com/pricing/details/virtual-network/).
- Další informace o [připojení virtuálního počítače Azure](site-recovery-network-design.md) po převzetí služeb při selhání.


## <a name="set-up-an-azure-storage-account"></a>Nastavení účtu úložiště Azure

- Site Recovery replikuje úložiště tooAzure místního počítače. Virtuální počítače Azure jsou vytvořené z úložiště hello po převzetí služeb při selhání.
- Nastavení [účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account) pro replikovaná data.
- Site Recovery na hello portálu Azure můžete použít účty úložiště nastavit ve službě Správce prostředků, nebo v klasickém režimu.
- účet úložiště Hello může být standardní nebo [premium](../storage/common/storage-premium-storage.md).
- Pokud jste nastavili prémiového účtu taky musíte další standardní účet pro data protokolu.


## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 6: Příprava VMware prostředky](vmware-walkthrough-prepare-vmware.md)
