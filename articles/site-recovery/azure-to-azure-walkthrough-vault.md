---
title: "aaaSet až úložiště pro virtuální počítač Azure repliction mezi oblastmi s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky hello nutné tooset až do trezoru pro Azure replikaci mezi oblastmi Azure pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: 40472189-3d80-4963-b175-8bddcbc2f61f
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: 9959c59c7ea57114763f13bf060404ddd267ba80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-4-set-up-a-vault-for-azure-tooazure-replication"></a>Krok 4: Nastavení trezoru Azure tooAzure replikace

Po [plánování sítě](azure-to-azure-walkthrough-network.md), použijte tento článek tooset nahoru k trezoru pro virtuální počítače Azure (VM) replikace tooanother oblast Azure, pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.

- Po dokončení hello článku, měli byste mít nastavení trezoru služeb zotavení.
- Odeslat všechny komentáře v dolní části hello tohoto článku nebo pokládání dotazů v hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



>[!NOTE]
>
> Azure replikace virtuálního počítače je aktuálně ve verzi preview.




## <a name="create-a-vault"></a>Vytvoření trezoru

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> Doporučujeme vytvořit trezor služeb zotavení hello v hello umístění, kam má tooreplicate vaše virtuální počítače. Například pokud cílové umístění je hello střed USA, vytvořte trezor hello v **střed USA**.


## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 5: povolení replikace](azure-to-azure-walkthrough-enable-replication.md)
