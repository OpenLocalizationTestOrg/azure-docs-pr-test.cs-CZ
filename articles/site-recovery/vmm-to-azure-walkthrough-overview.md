---
title: "aaaReplicate virtuálních počítačů Hyper-V v nástroji VMM cloudů tooAzure s Azure Site Recovery | Microsoft Docs"
description: "Poskytuje přehled pro replikaci virtuálních počítačů Hyper-V v tooAzure cloudů VMM pomocí služby Azure Site Recovery hello"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: d6f729a49cc86ea07bebc4d7266fd7b58b3998f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooazure-using-site-recovery-in-hello-azure-portal"></a>Replikace virtuálních počítačů technologie Hyper-V v tooAzure cloudů VMM pomocí Site Recovery v hello portálu Azure
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-azure.md)
> * [Azure Classic](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [PowerShell – Classic](site-recovery-deploy-with-powershell.md)


Tento článek obsahuje přehled hello kroky požadované tooreplicate místní virtuální počítače Hyper-V (VM) spravované v cloudech tooAzure System Center Virtual Machine Manager (VMM), pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v Hello portálu Azure.

Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="step-1-review-hello-scenario-architecture"></a>Krok 1: Posouzení architekturu scénáře hello

Před spuštěním nasazení, zkontrolujte architekturu scénáře hello, ujistěte se, že rozumíte všechny součásti hello musíte toodeploy.

Přejděte příliš[krok 1: Zkontrolujte architektura hello](vmm-to-azure-walkthrough-architecture.md)

## <a name="step-2-review-prerequisites-and-limitations"></a>Krok 2: Kontrola předpoklady a omezení

Ujistěte se, že rozumíte hello nasazení předpoklady a omezení.

**Požadavky Azure**: budete potřebovat účet Microsoft Azure, sítě Azure a účty úložiště.
**Na místní servery VMM a hostitelé Hyper-V**: Ujistěte se, že jsou servery VMM a hostitelé Hyper-V kompatibilní a připravený pro nasazení Site Recovery.
**Replikovat virtuální počítače**: Zkontrolujte, zda chcete tooreplicate virtuálních počítačů v souladu s požadavky na Azure.

Přejděte příliš[krok 2: ověření předpoklady a omezení](vmm-to-azure-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>Krok 3: Plánování kapacity

Pokud provádíte úplné nasazení, je třeba toofigure na jaké replikace prostředky, které potřebujete. Existuje několik z nástrojů k dispozici toohelp to uděláte. Pokud jste to nějakou rychlou nastavení tootest hello prostředí, můžete tento krok přeskočit.

Přejděte příliš[krok 3: plánování kapacity](vmm-to-azure-walkthrough-capacity.md)

## <a name="step-4-plan-networking"></a>Krok 4: Plánování sítě

Je nutné toodo některé síťové plánování tooensure, které lze konfigurovat mapování sítě, když nasazujete hello scénář, virtuální počítače Azure budou virtuální sítě připojený tooAzure po převzetí služeb při selhání, a který je přiřazený příslušné IP adresy.

Přejděte příliš[krok 4: plánování sítě](vmm-to-azure-walkthrough-network.md)


## <a name="step-5-prepare-azure-resources"></a>Krok 5: Příprava prostředků Azure

Nastavte účet Azure, sítě a úložiště. Můžete to provést během nasazení, ale doporučujeme, abyste že to uděláte před zahájením.

Přejděte příliš[krok 5: Příprava Azure](vmm-to-azure-walkthrough-prepare-azure.md)

## <a name="step-6-prepare-vmm-and-hyper-v"></a>Krok 6: Příprava VMM a technologie Hyper-V

Příprava serverů VMM místní hello a hostitelů Hyper-V pro nasazení Site Recovery.

Přejděte příliš[krok 6: Příprava na místní servery](vmm-to-azure-walkthrough-vmm-hyper-v.md)

## <a name="step-7-set-up-a-vault"></a>Krok 7: Nastavení trezoru

Nastavení trezoru služeb zotavení. Trezor Hello obsahuje nastavení konfigurace a orchestruje replikaci.

[Krok 7: Nastavení trezoru](vmm-to-azure-walkthrough-create-vault.md)

## <a name="step-8-configure-source-and-target-settings"></a>Krok 8: Konfigurace nastavení zdroje a cíle

Nastavte hello zdrojové a cílové umístění replikace. Přidejte hello VMM server toohello trezoru a stáhnout hello instalační soubory pro součásti Site Recovery. Spusťte instalační program zprostředkovatele Azure Site Recovery na serveru VMM hello. Instalační program nainstaluje hello zprostředkovatele na serveru VMM hello a zaregistruje hello server v trezoru hello. Nainstalujte agenta služeb zotavení Microsoft hello na každém hostiteli technologie Hyper-V.

Přejděte příliš[krok 8: Konfigurace nastavení zdroje a cíle](vmm-to-azure-walkthrough-source-target.md)

## <a name="step-9-configure-network-mapping"></a>Krok 9: Konfigurace mapování sítě

Mapa místní virtuální sítě pro tooAzure sítě virtuálních počítačů nástroje VMM. Po převzetí služeb při selhání jsou virtuální počítače Azure vytvořené v hello síť Azure, která mapuje sítě virtuálních počítačů místní toohello, ve které hello se nachází zdroj technologie Hyper-V.

Přejděte příliš[krok 9: nakonfigurování mapování sítě](vmm-to-azure-walkthrough-network-mapping.md)


## <a name="step-10-set-up-a-replication-policy"></a>Krok 10: Nastavení zásad replikace

Zadejte, jak místní virtuální počítače budou replikované tooAzure.

Přejděte příliš[krok 10: nastavení zásad replikace](vmm-to-azure-walkthrough-replication.md)


## <a name="step-11-enable-replication-for-vms"></a>Krok 11: Povolení replikace pro virtuální počítače

Vyberte virtuální počítače hello chcete tooreplicate. Povolení virtuálního počítače pro replikaci aktivační události hello počáteční replikace tooAzure, následovaný probíhající rozdílová replikace.

Přejděte příliš[krok 11: povolení replikace](vmm-to-azure-walkthrough-enable-replication.md)


## <a name="step-12-run-a-test-failover"></a>Krok 12: Spuštění testu převzetí služeb

Spusťte toomake testovací převzetí služeb při selhání, se, že vše funguje podle očekávání.

Přejděte příliš[krok 12: spustit testovací převzetí služeb](vmm-to-azure-walkthrough-test-failover.md)


