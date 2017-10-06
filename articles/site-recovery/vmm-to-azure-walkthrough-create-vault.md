---
title: "aaaSet až úložiště pro Hyper-V tooAzure replikace (s nástrojem System Center VMM) pomocí Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky hello potřebujete tooset až do trezoru pro tooAzure replikace (s VMM) technologie Hyper-V pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: b3cd6f03-c33c-406d-91d4-5cba69f79abd
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: f2c90f3c8b0a48db1e57fefd9829d29cffff8d43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-set-up-a-vault-for-hyper-v-replication"></a>Krok 7: Nastavení trezoru pro replikaci technologie Hyper-V

Tento článek popisuje, jak tooset až do trezoru a určit, co chcete tooreplicate z místního umístění, tooAzure pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.


POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]



## <a name="select-a-protection-goal"></a>Vyberte cíl ochrany

Vyberte, co chcete tooreplicate, a místo, kam chcete tooreplicate k.

1. Klikněte na tlačítko **trezory služeb zotavení** > trezoru.
2. V hello prostředků nabídky, klikněte na **Site Recovery** > **Příprava infrastruktury** > **cíl ochrany**.
3. V **cíl ochrany**, vyberte **tooAzure** > **Ano, s technologií Hyper-V**. Vyberte **Ano** tooconfirm jste používání aplikace VMM. 

     ![Zvolte cíle.](./media/vmm-to-azure-walkthrough-create-vault/choose-goals.png)



## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 8: nastavit zdroje a cíle](vmm-to-azure-walkthrough-source-target.md)
