---
title: "Architektura hello aaaReview pro replikaci virtuálních počítačů Azure mezi oblastmi Azure | Microsoft Docs"
description: "Tento článek obsahuje přehled součásti a architektura použít při replikaci virtuálních počítačů Azure mezi oblastmi Azure pomocí služby Azure Site Recovery hello."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/12/2017
ms.author: raynew
ms.openlocfilehash: 4caab4e7a764040f317201d1345c40c73f836d81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-1-review-hello-architecture-for-azure-vm-replication-between-azure-regions"></a>Krok 1: Posouzení hello architektura pro virtuální počítač Azure replikaci mezi oblastmi Azure


Po zkontrolování hello [přehled kroků](azure-to-azure-walkthrough-overview.md) pro toto nasazení, přečtěte si tento článek toounderstand hello součástech a procesech, které používají při replikaci a obnovení virtuálních počítačů Azure (VM) z jedné oblasti Azure tooanother pomocí [Azure Site Recovery](site-recovery-overview.md).

- Po dokončení hello článku měli vědět, jak funguje oblast tooanother replikace virtuálního počítače Azure.
- Odeslat všechny komentáře v dolní části hello tohoto článku nebo pokládání dotazů v hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

>[!NOTE]
>Azure replikace virtuálního počítače s hello služba Site Recovery je aktuálně ve verzi preview.



## <a name="architectural-components"></a>Komponenty architektury

Následující diagram Hello poskytuje podrobný pohled prostředí virtuálního počítače Azure v určité oblasti (v tomto příkladu hello umístění východní USA). V prostředí virtuálního počítače Azure:
- Aplikace může mít spuštěný na virtuálních počítačích s disky, které šíří mezi různými účty úložiště.
- Hello virtuální počítače můžou být součástí jedné nebo více podsítí v rámci virtuální sítě.

![prostředí zákazníka](./media/azure-to-azure-walkthrough-architecture/source-environment.png)

## <a name="replication-process"></a>Proces replikace

### <a name="step-1"></a>Krok 1

Když povolíte replikaci virtuálního počítače Azure v hello portálu Azure, hello prostředků ukazuje hello následujícím diagramu a v tabulce jsou automaticky vytvářeny v hello cílová oblast. Ve výchozím nastavení prostředky jsou vytvořeny na základě nastavení oblasti zdroje. Nastavení cílového hello podle potřeby můžete přizpůsobit. [Další informace](site-recovery-replicate-azure-to-azure.md).

![Povolit replikaci proces, krok 1](./media/azure-to-azure-walkthrough-architecture/enable-replication-step-1.png)

**Prostředek** | **Podrobnosti**
--- | ---
**Cílová skupina prostředků** | Hello toowhich skupiny prostředků patří replikované virtuální počítače po převzetí služeb při selhání.
**Cílová virtuální síť** | Hello virtuální sítě ve kterém jsou replikované virtuální počítače umístěné po převzetí služeb při selhání. Mapování sítě se vytvoří mezi zdrojovými a cílovými virtuální sítě a naopak.
**Účty úložiště mezipaměti** | Předtím, než změny na zdroje, které virtuální počítače jsou replikovány toohello cílový účet úložiště, jsou sledovány a odeslat účet úložiště mezipaměti toohello v cílovém umístění hello. Tím se zajistí minimální dopad na produkční aplikace běžící na hello virtuálních počítačů.
**Cílové úložiště účty**  | Účty úložiště ve hello cílová umístění toowhich hello data se replikují.
**Cílové skupiny dostupnosti**  | V které hello replikovat virtuální počítače se po převzetí služeb při selhání nacházejí sady dostupnosti.

### <a name="step-2"></a>Krok 2

Jak je zapnutá replikace, hello rozšíření Site Recovery Mobility service se automaticky nainstaluje na hello virtuálních počítačů. dojde k následujícímu Hello:

1. Hello virtuálního počítače je registrovaný pomocí Site Recovery.

2. Průběžná replikace je nakonfigurován pro hello virtuálních počítačů. Data zapíše na hello virtuálních počítačů jsou disky nepřetržitě přenést toohello účet úložiště mezipaměti v umístění zdroje hello.

   ![Povolit replikaci proces, krok 2](./media/azure-to-azure-walkthrough-architecture/enable-replication-step-2.png)

  
  Všimněte si, že Site Recovery nikdy potřebuje, příchozí připojení toohello virtuálních počítačů. Pouze odchozí připojení služby zotavení tooSite adresy URL nebo IP adresy, adresy URL nebo IP adresy ověřování Office 365 a mezipaměti úložiště účet IP adresy je potřeba. 

## <a name="continuous-replication-process"></a>Proces průběžná replikace

Po průběžné replikace funguje, disk, který provede zápis jsou okamžitě přenést toohello účet úložiště mezipaměti. Site Recovery zpracovává hello data a odešle ji toohello cílový účet úložiště. Po zpracování dat hello body obnovení jsou generovány v hello cílový účet úložiště každých několik minut.

## <a name="failover-process"></a>Proces převzetí služeb při selhání

Když iniciujete převzetí služeb při selhání, cílové hello, které jsou virtuální počítače vytvořené v hello cílová skupina prostředků, cílová virtuální síť, cílové podsíti a hello skupinu dostupnosti. Při selhání můžete použít jakýkoli bod obnovení.

![Proces převzetí služeb při selhání](./media/azure-to-azure-walkthrough-architecture/failover.png)

## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 2: ověření předpoklady a omezení](azure-to-azure-walkthrough-prerequisites.md)
