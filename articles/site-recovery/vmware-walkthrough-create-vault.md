---
title: "aaaSet až do trezoru pro tooAzure replikace VMware pomocí Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky hello potřebujete tooset až do trezoru pro tooAzure replikace VMware pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8bce940e-f19f-4418-8360-aee7b073519a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 8a7755a6c9a3f55f241c615e425285bc4b782493
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-set-up-a-vault-for-vmware-replication-tooazure"></a>Krok 7: Nastavení trezoru pro tooAzure replikace VMware


Tento článek popisuje, jak tooset až do trezoru a určit, co chcete tooreplicate z místního umístění, tooAzure pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.


POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).




## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Vyberte cíl ochrany

Vyberte, co chcete tooreplicate, a místo, kam chcete tooreplicate k.

1. Klikněte na tlačítko **trezory služeb zotavení** > trezoru.
2. V hello prostředků nabídky, klikněte na **Site Recovery** > **Příprava infrastruktury** > **cíl ochrany**.
3. V **cíl ochrany**, vyberte **tooAzure** > **Ano, s hypervisoru VMware vSphere**.



## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 8: nastavit zdroje a cíle](vmware-walkthrough-source-target.md)
