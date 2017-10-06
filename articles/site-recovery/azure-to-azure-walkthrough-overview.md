---
title: "virtuální počítače Azure aaaReplicate mezi oblastmi Azure | Microsoft Docs"
description: "Shrnuje hello kroky požadované tooreplicate virtuálních počítačích Azure mezi oblastí Azure se službou Azure Site Recovery hello v hello portálu Azure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: 6dd36239-4363-4538-bf80-a18e71b8ec67
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: f4ac386f040945131f8a10f02143437f4441e298
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a>Replikace virtuálních počítačů Azure mezi oblastmi s Azure Site Recovery

>Tento článek obsahuje přehled hello kroky požadované tooreplicate virtuální počítače Azure (VM) v jedné oblasti Azure tooAzure virtuálních počítačů v jiné oblasti. 

>[!NOTE]
>
> Azure replikace virtuálního počítače je aktuálně ve verzi preview.

POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="step-1-review-architecture"></a>Krok 1: Kontrola architektura

Před zahájením nasazení, zkontrolujte architekturu scénáře hello a potřebujete toodeploy součásti hello.

Přejděte příliš[krok 1: Zkontrolujte architektura hello](azure-to-azure-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>Krok 2: Kontrola předpokladů

Zkontrolujte, že mají hello požadavky Azure v místech, včetně předplatného, virtuálních sítí, účty úložiště a požadavky virtuálního počítače.

Přejděte příliš[krok 2: ověření předpoklady a omezení](azure-to-azure-walkthrough-prerequisites.md)


## <a name="step-3-plan-networking"></a>Krok 3: Plánování sítě

Zkontrolujte, zda odchozí připojení nastavená na virtuálních počítačích Azure, které chcete tooreplicate a že jsou nastavení připojení z místní.

Přejděte příliš[krok 4: plánování sítě](azure-to-azure-walkthrough-network.md)



## <a name="step-4-create-a-vault"></a>Krok 4: Vytvoření trezoru 

Třeba tooset nahoru tooorchestrate trezoru služeb zotavení a správu replikace a zadejte hello zdroj oblast.

Přejděte příliš[krok 4: vytvoření trezoru](azure-to-azure-walkthrough-vault.md)


## <a name="step-5-enable-replication"></a>Krok 5: Povolení replikace


tooenable replikace, nakonfigurovat nastavení cílového umístění, nastavení zásad replikace a vyberte, které chcete tooreplicate hello virtuálních počítačích Azure. Když povolíte, dojde k počáteční replikaci hello virtuálních počítačů.

Přejděte příliš[krok 5: povolení replikace](azure-to-azure-walkthrough-enable-replication.md)


## <a name="step-6-run-a-test-failover"></a>Krok 6: Spuštění testu převzetí služeb

Po dokončení počáteční replikace a je spuštěna rozdílová replikace, můžete spustit toomake testovací převzetí služeb při selhání, se, že vše funguje podle očekávání.

Přejděte příliš[krok 6: spustit testovací převzetí služeb](azure-to-azure-walkthrough-test-failover.md)


