---
title: "Příprava cílového (fyzické tooAzure) | Microsoft Docs"
description: "Tento článek popisuje, jak tooprepare toostart vašeho prostředí Azure replikovat fyzické servery se systémem Windows nebo Linux tooAzure."
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhemraj
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 5/31/2017
ms.author: bsiva
ms.openlocfilehash: 126fb86133e1a00f5669410943565c4cd78e4369
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-target-vmware-tooazure"></a>Příprava cílového (VMware tooAzure)
> [!div class="op_single_selector"]
> * [VMware tooAzure](./site-recovery-prepare-target-vmware-to-azure.md)
> * [Fyzické tooAzure](./site-recovery-prepare-target-physical-to-azure.md)

Tento článek popisuje, jak tooprepare vašeho prostředí Azure toostart replikace fyzických serverů (x 64) se systémem Windows nebo Linux do Azure.

## <a name="prerequisites"></a>Požadavky

Hello článek předpokládá hello následující:
- Vytvoření trezoru služeb zotavení tooprotect fyzických serverů. Můžete vytvořit trezor služeb zotavení z hello [portál Azure](http://portal.azure.com "portál Azure").
- Máte [instalaci prostředí místní](./site-recovery-set-up-physical-to-azure.md) tooAzure tooreplicate fyzických serverů.

## <a name="prepare-target"></a>Příprava cílového

Po dokončení hello **cíl ochrany krok 1: vyberte** a **krok 2: Příprava zdroj**, budete přesměrováni příliš**krok 3: cíl**

![Příprava cílového](./media/site-recovery-prepare-target-physical-to-azure/prepare-target-physical-to-azure.png)

1. **Předplatné:** z hello rozevírací nabídce vyberte hello předplatné, které chcete tooreplicate fyzických serverů.
2. **Model nasazení:** model nasazení vyberte hello (Classic nebo Resource Manager)

Podle hello zvolený model nasazení, ověřování tooensure, jestli máte aspoň jeden účet kompatibilní úložiště a virtuální sítě v hello cílové předplatné tooreplicate a převzetí služeb při selhání vaší fyzických serverů na spuštění.

Po ověření hello úspěšně dokončit, klikněte na tlačítko OK toogo toohello další krok.

Pokud nemáte účet úložiště kompatibilní Resource Manager nebo virtuální sítě, nebo chcete tooadd další, můžete provést kliknutím hello **+ účet úložiště** nebo **+ síť** tlačítka na začátku hello hello okno.

## <a name="next-steps"></a>Další kroky
[Konfigurace nastavení replikace](./site-recovery-setup-replication-settings-vmware.md).
