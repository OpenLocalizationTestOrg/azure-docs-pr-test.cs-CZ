---
title: "aaaPrepare prostředky Azure tooreplicate virtuálních počítačů technologie Hyper-V (bez System Center VMM) tooAzure pomocí Azure Site Recovery | Microsoft Docs"
description: "Popisuje, co potřebujete zavedené v Azure před zahájením replikace virtuálních počítačů Hyper-V (bez VMM) tooAzure pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 28fa722c-675e-4637-98eb-7ccbf3806d69
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: f659e300c39253b0eaf7218bee9d39b11682edb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-prepare-azure-resources-for-hyper-v-replication-tooazure"></a>Krok 5: Příprava prostředků Azure pro tooAzure replikace technologie Hyper-V

Použijte hello pokyny v této článku tooprepare Azure tak, aby můžete replikovat místní prostředky virtuálních počítačů technologie Hyper-V (bez System Center VMM) pomocí hello tooAzure [Azure Site Recovery](site-recovery-overview.md) služby.

Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello, nebo požádat technické dotazy na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Než začnete

Ujistěte se, že jste si přečetli hello [požadavky](hyper-v-site-walkthrough-prerequisites.md)

## <a name="set-up-an-azure-account"></a>Nastavit účet Azure

- Získání [účet Microsoft Azure](http://azure.microsoft.com/).
- Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).
- Zkontrolujte hello podporované oblasti pro obnovení lokality v rámci geografickou dostupností v [Azure Site Recovery podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).
- Další informace o [cenách za Site Recovery](site-recovery-faq.md#pricing)a získat hello [podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).


## <a name="set-up-an-azure-network"></a>Nastavení sítě Azure

- Nastavte síť Azure. Virtuální počítače Azure jsou umístěny v této síti, když jste vytvořili po převzetí služeb při selhání.
- Hello síť musí být ve hello stejné oblasti jako hello trezoru služeb zotavení
- Site Recovery na hello portálu Azure můžete použít nastavení sítích [Resource Manager](../resource-manager-deployment-model.md), nebo v klasickém režimu.
- Doporučujeme nastavit síť ještě před tím, než začnete. Pokud to neuděláte, musíte toodo ji během nasazování Site Recovery.
- Další informace o [ceny virtuální sítě](https://azure.microsoft.com/pricing/details/virtual-network/).


## <a name="set-up-an-azure-storage-account"></a>Nastavení účtu úložiště Azure

- Site Recovery replikuje úložiště tooAzure místního počítače. Virtuální počítače Azure jsou vytvořené z úložiště hello po převzetí služeb při selhání.
- Nastavit standard nebo premium [účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account) toohold data replikují tooAzure.
- [Storage úrovně Premium](../storage/common/storage-premium-storage.md) se obvykle používá u virtuálních počítačů, které je třeba konzistentně vysoký výkon vstupně-výstupní operace a s nízkou latencí toohost vstupně-výstupní operace náročné úlohy.
- Pokud chcete toouse toostore účtu premium replikovaná data, musíte taky standardní úložiště účet toostore protokoly replikace, zachycení probíhající změny tooon místní data.
- V závislosti na modelu prostředků hello chcete toouse pro převzetí služeb virtuálních počítačích Azure při selhání, nastavíte účet v [režimu Resource Manager](../storage/common/storage-create-storage-account.md), nebo [klasickém režimu](../storage/common/storage-create-storage-account.md).
- Doporučujeme, abyste před zahájením nastavení účtu úložiště. Pokud to neuděláte, budete potřebovat toodo ji během nasazování Site Recovery. Hello účty musí být v hello stejné oblasti jako hello trezoru služeb zotavení.
- Nelze přesunout, účty úložiště používané při Site Recovery přes skupiny prostředků v rámci hello stejné předplatné, nebo v různých předplatných.


## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 6: Příprava technologie Hyper-V prostředky](hyper-v-site-walkthrough-prepare-hyper-v.md)
