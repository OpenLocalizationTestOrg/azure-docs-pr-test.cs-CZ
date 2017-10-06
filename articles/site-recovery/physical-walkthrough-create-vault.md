---
title: "aaaSet až do trezoru pro fyzický server replikace tooAzure pomocí Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky hello potřebujete tooset až tooAzure trezoru tooreplicate fyzických serverů pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d99f2605-f417-4995-be77-5323136b814f
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 988928e3ece31116823f132cc39223fe44443468
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-set-up-a-vault-for-physical-server-replication-tooazure"></a>Krok 6: Nastavení trezoru pro tooAzure replikace fyzického serveru


Tento článek popisuje, jak tooset až do trezoru. Vytvoření trezoru hello a určit, co chcete tooreplicate z vaší místní tooAzure umístění pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.


POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).




## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Vyberte cíl ochrany

Vyberte, co chcete tooreplicate, a místo, kam chcete tooreplicate k.

1. Klikněte na tlačítko **trezory služeb zotavení** > trezoru.
2. V hello prostředků nabídky, klikněte na **Site Recovery** > **Příprava infrastruktury** > **cíl ochrany**.
3. V **cíl ochrany**, vyberte **tooAzure** > **není virtualizované/jiné**.


## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 7: nastavit zdroje a cíle](physical-walkthrough-source-target.md)
