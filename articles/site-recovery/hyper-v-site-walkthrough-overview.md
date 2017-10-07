---
title: "aaaReplicate tooAzure virtuálních počítačů Hyper-V s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak tooorchestrate replikace, převzetí služeb při selhání a obnovení místní technologie Hyper-V virtuální počítače tooAzure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: efddd986-bc13-4a1d-932d-5484cdc7ad8d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: ab9cd14149ef32a416428d0f4327aa18b042e9c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-without-vmm-tooazure"></a>Replikovat virtuální počítače (bez VMM) tooAzure technologie Hyper-V 

> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
> * [Azure Classic](site-recovery-hyper-v-site-to-azure-classic.md)
> * [PowerShell – Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
>
>

Tento článek obsahuje přehled hello kroky tooreplicate místní technologie Hyper-V virtuální počítače tooAzure, pomocí hello [Azure Site Recovery](site-recovery-overview.md) v hello portálu Azure. Ve virtuálních počítačích této nasazení technologie Hyper-V nejsou spravovány nástrojem System Center Virtual Machine Manager (VMM).


Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello, nebo požádat technické dotazy na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="step-1-review-architecture-and-prerequisites"></a>Krok 1: Posouzení architektura a požadavky

Před zahájením nasazení, zkontrolujte architekturu scénáře hello a ujistěte se, že rozumíte všechny součásti hello potřebujete toodeploy

Přejděte příliš[krok 1: Zkontrolujte architektura hello](hyper-v-site-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>Krok 2: Kontrola předpokladů

Ujistěte se, že máte nastavené pro jednotlivé součásti nasazení hello požadavky:

- **Požadavky Azure**: budete potřebovat účet Microsoft Azure, sítě Azure a účty úložiště.
- **Požadavky technologie Hyper-V na místě**: Zajistěte, aby hostitelů Hyper-V jsou připraveny pro nasazení Site Recovery.
- **Replikovat virtuální počítače**: virtuální počítače chcete tooreplicate potřebovat toocomply s požadavky na Azure.

Přejděte příliš[krok 2: ověření předpoklady a omezení](hyper-v-site-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>Krok 3: Plánování kapacity

Pokud provádíte úplné nasazení musíte toofigure na jaké replikace prostředky, které potřebujete. Existuje několik z nástrojů k dispozici toohelp to uděláte. Přejděte tooStep 2. Pokud provádíte nějakou rychlou nastavení tootest hello prostředí můžete tento krok přeskočit.

Přejděte příliš[krok 3: plánování kapacity](hyper-v-site-walkthrough-capacity.md)

## <a name="step-4-plan-networking"></a>Krok 4: Plánování sítě

Je nutné toodo některé síťové plánování tooensure, že virtuální počítače Azure jsou připojené toonetworks po převzetí služeb při selhání, a že, ke kterým mají hello správné IP adres.

Přejděte příliš[krok 4: plánování sítě](hyper-v-site-walkthrough-network.md)

##  <a name="step-5-prepare-azure-resources"></a>Krok 5: Příprava prostředků Azure

Nastavte před zahájením Azure sítím a úložišti. Můžete to provést během nasazení, ale doporučujeme, abyste že to uděláte před zahájením.

Přejděte příliš[krok 5: Příprava Azure](hyper-v-site-walkthrough-prepare-azure.md)


## <a name="step-6-prepare-hyper-v"></a>Krok 6: Příprava technologie Hyper-V

Ujistěte se, že technologie Hyper-V servery splňují požadavky na nasazení Site Recovery.

Přejděte příliš[krok 6: Příprava technologie Hyper-V](hyper-v-site-walkthrough-prepare-hyper-v.md)

## <a name="step-7-set-up-a-vault"></a>Krok 7: Nastavení trezoru

Potřebujete tooset nahoru tooorchestrate trezoru služeb zotavení a správě replikace. Při nastavování hello trezoru, určíte, co chcete tooreplicate, a místo, kam chcete tooreplicate jeho.

Přejděte příliš[krok 7: vytvoření trezoru](hyper-v-site-walkthrough-create-vault.md)

## <a name="step-8-configure-source-and-target-settings"></a>Krok 8: Konfigurace nastavení zdroje a cíle

Nastavte hello zdroj a cíl, který se používá pro replikaci. Nastavení nastavení zdroje, zahrnuje přidání hostitelů technologie Hyper-V, tooa web Hyper-V, instalaci hello zprostředkovatele služby Site Recovery a agenta služeb zotavení na každém hostiteli technologie Hyper-V a registraci hello lokality v trezoru služeb zotavení hello.

Přejděte příliš[krok 8: nastavení hello zdroje a cíle](hyper-v-site-walkthrough-source-target.md)

## <a name="step-9-set-up-a-replication-policy"></a>Krok 9: Nastavení zásad replikace

Můžete nastavit nastavení replikace toospecify zásady pro virtuální počítače Hyper-v trezoru hello.

Přejděte příliš[krok 9: nastavení zásad replikace](hyper-v-site-walkthrough-replication.md)


## <a name="step-10-enable-replication"></a>Krok 10: Povolení replikace

Až budete mít zásadu replikace na místě, po povolení, dojde k počáteční replikaci hello virtuálních počítačů.

Přejděte příliš[krok 10: povolení replikace](hyper-v-site-walkthrough-enable-replication.md)

## <a name="step-11-run-a-test-failover"></a>Krok 11: Spuštění testu převzetí služeb

Po dokončení počáteční replikace a je spuštěna rozdílová replikace, můžete spustit toomake testovací převzetí služeb při selhání, se, že vše funguje podle očekávání.

Přejděte příliš[krok 11: spustit testovací převzetí služeb](hyper-v-site-walkthrough-test-failover.md)
