---
title: "aaaReplicate virtuálních počítačů Hyper-V tooa sekundární lokalita VMM s Azure Site Recovery | Microsoft Docs"
description: "Poskytuje přehled pro replikaci virtuálních počítačů Hyper-V tooa sekundární lokalita VMM pomocí hello portálu Azure."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 476ca82a-8f5c-4498-9dcf-e1011d60ed59
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: raynew
ms.openlocfilehash: 90e44bfc2237dfa7646fb2b7b1e533a7f6d83c50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooa-secondary-vmm-site"></a>Replikace virtuálních počítačů technologie Hyper-V v nástroji VMM cloudy tooa sekundární lokalita VMM

> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-vmm.md)
> * [Portál Classic](site-recovery-vmm-to-vmm-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

Tento článek obsahuje přehled hello kroky požadované tooreplicate místně spravované v cloudech System Center Virtual Machine Manager (VMM), tooa sekundárního umístění VMM, Hyper-V virtuální počítače (VM) pomocí [Azure Site Recovery](site-recovery-overview.md)v hello portálu Azure.

Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="step-1-review-hello-scenario-architecture"></a>Krok 1: Posouzení architekturu scénáře hello

Před spuštěním nasazení, zkontrolujte architekturu scénáře hello, ujistěte se, že rozumíte všechny součásti hello musíte toodeploy.

Přejděte příliš[krok 1: Zkontrolujte hello architektura](vmm-to-vmm-walkthrough-architecture.md).

## <a name="step-2-review-prerequisites-and-limitations"></a>Krok 2: Kontrola předpoklady a omezení

Ujistěte se, že rozumíte hello nasazení předpoklady a omezení.

**Požadavky Azure**: musíte mít předplatné Microsoft Azure a služeb zotavení Azure trezoru, tooorchestrate a spravovat replikace.
**Na místní servery VMM a hostitelé Hyper-V**: Ujistěte se, že jsou servery VMM a hostitelé Hyper-V kompatibilní a připravený pro nasazení Site Recovery.

Přejděte příliš[krok 2: ověření předpoklady a omezení](vmm-to-vmm-walkthrough-prerequisites.md).

## <a name="step-3-plan-networking"></a>Krok 3: Plánování sítě

Je nutné toodo některé síťové plánování tooensure lze nastavit mapování sítě mezi sítěmi virtuálních počítačů nástroje VMM při nasazování hello scénář.

Přejděte příliš[krok 3: plánování sítě](vmm-to-vmm-walkthrough-network.md).


## <a name="step-4-prepare-vmm-and-hyper-v"></a>Krok 4: Příprava VMM a technologie Hyper-V

Příprava pro nasazení Site Recovery hello servery VMM a hostitelů Hyper-V.

Přejděte příliš[krok 4: Příprava na místní servery](vmm-to-vmm-walkthrough-vmm-hyper-v.md).

## <a name="step-5-set-up-a-vault"></a>Krok 5: Nastavení trezoru

Nastavení trezoru služeb zotavení. Trezor Hello obsahuje nastavení konfigurace a orchestruje replikaci.

[Krok 5: Nastavení trezoru](vmm-to-vmm-walkthrough-create-vault.md).

## <a name="step-6-set-up-source-and-target-settings"></a>Krok 6: Nastavení zdrojové a cílové nastavení

Nastavte hello zdrojové a cílové replikace VMM umístění. Přidejte hello VMM servery toohello trezoru a stáhnout hello instalační soubory pro součásti Site Recovery. Spusťte instalační program zprostředkovatele Azure Site Recovery na serveru VMM hello. Instalační program nainstaluje hello zprostředkovatele na serveru VMM hello a zaregistruje hello server v trezoru hello. Nainstalujte agenta služeb zotavení Microsoft hello na každém hostiteli technologie Hyper-V.

Přejděte příliš[krok 6: nastavení hello zdrojové a cílové nastavení](vmm-to-vmm-walkthrough-source-target.md).

## <a name="step-7-configure-network-mapping"></a>Krok 7: Nakonfigurování mapování sítě

Mapování sítě virtuálních počítačů nástroje VMM v hello zdrojové a cílové umístění. Po převzetí služeb při selhání jsou virtuální počítače vytvořené v cílové síti hello této mapy toohello zdrojové síti virtuálních počítačů ve které hello se nachází zdrojový virtuální počítač Hyper-V.

Přejděte příliš[krok 7: nakonfigurování mapování sítě](vmm-to-vmm-walkthrough-network-mapping.md).


## <a name="step-8-set-up-a-replication-policy"></a>Krok 8: Nastavení zásad replikace

Zadejte, jak bude replikován virtuální počítače mezi umístěními VMM.

Přejděte příliš[krok 8: nastavte zásady replikace](vmm-to-vmm-walkthrough-replication.md).


## <a name="step-9-enable-replication-for-vms"></a>Krok 9: Povolení replikace pro virtuální počítače

Vyberte virtuální počítače hello chcete tooreplicate. Povolení virtuálního počítače pro replikaci aktivační události hello počáteční replikace toohello sekundární lokality, pak probíhající rozdílová replikace.

Přejděte příliš[krok 9: povolení replikace](vmm-to-vmm-walkthrough-enable-replication.md).


## <a name="step-10-run-a-test-failover"></a>Krok 10: Spustit testovací převzetí služeb

Spusťte toomake testovací převzetí služeb při selhání, se, že vše funguje podle očekávání.

Přejděte příliš[krok 10: spustit testovací převzetí služeb](vmm-to-vmm-walkthrough-test-failover.md).
