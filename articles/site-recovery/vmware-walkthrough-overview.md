---
title: "virtuální počítače VMware tooAzure aaaReplicate s Azure Site Recovery | Microsoft Docs"
description: "Poskytuje přehled hello kroky pro replikaci úloh běžících na virtuálních počítačích VMware tooAzure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 7104f67a3635b916048dcb61bca770c89f0c77fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-vmware-vms-tooazure-with-site-recovery"></a>Replikovat virtuální počítače VMware tooAzure službou Site Recovery

Tento článek obsahuje přehled hello kroky požadované tooreplicate místní VMware virtuální počítače tooAzure, pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.


![Proces nasazení](./media/vmware-walkthrough-overview/vmware-to-azure-process.png)

**Obrázek 1: Souhrn procesu nasazení**

## <a name="step-1-review-architecture-and-prerequisites"></a>Krok 1: Posouzení architektura a požadavky

Před zahájením nasazení, zkontrolujte architekturu scénáře hello a ujistěte se, že rozumíte všechny součásti hello potřebujete toodeploy

Přejděte příliš[krok 1: Zkontrolujte architektura hello](vmware-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>Krok 2: Kontrola předpokladů

Ujistěte se, že máte nastavené pro jednotlivé součásti nasazení hello požadavky:

- **Požadavky Azure**: budete potřebovat účet Microsoft Azure, sítě Azure a účty úložiště.
- **Místní součásti Site Recovery**: budete potřebovat počítač se systémem součásti Site Recovery na místě.
- **Místní požadavky VMware**: budete potřebovat tooset účtů, aby Site Recovery přístup serverů VMware a virtuálních počítačů.
- **Replikovat virtuální počítače**: virtuální počítače chcete tooreplicate potřeba toocomply s požadavky na Azure a mají hello nainstalovat službu mobility.

Přejděte příliš[krok 2: Přečtěte si požadavky a omezení](vmware-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>Krok 3: Plánování kapacity

Pokud provádíte úplné nasazení musíte toofigure na jaké replikace prostředky, které potřebujete. Existuje několik z nástrojů k dispozici toohelp to uděláte. Přejděte tooStep 2. Pokud provádíte nějakou rychlou nastavení tootest hello prostředí můžete tento krok přeskočit.

Přejděte příliš[krok 3: plánování kapacity](vmware-walkthrough-capacity.md)

## <a name="step-4-plan-networking"></a>Krok 4: Plánování sítě

Je nutné toodo některé síťové plánování tooensure, že virtuální počítače Azure jsou připojené toonetworks po převzetí služeb při selhání, a že, ke kterým mají hello správné IP adres.

Přejděte příliš[krok 4: plánování sítě](vmware-walkthrough-network.md)

##  <a name="step-5-prepare-azure-resources"></a>Krok 5: Příprava prostředků Azure

Nastavte před zahájením Azure sítím a úložišti. Můžete to provést během nasazení, ale doporučujeme, abyste že to uděláte před zahájením.

Přejděte příliš[krok 5: Příprava Azure](vmware-walkthrough-prepare-azure.md)


## <a name="step-6-prepare-vmware"></a>Krok 6: Příprava VMware

Je třeba tooset účtů, které budou používat Site Recovery na:

- Přístup k VMware virtualizace serverů tooautomatically zjistit virtuální počítače.
- Přístup k služba Mobility hello tooinstall virtuálních počítačů. Každý virtuální počítač, budete ho chtít tooreplicate musí mít agenta služby Mobility hello nainstalovat předtím, než můžete povolit pro něho replikaci.

Přejděte příliš[krok 6: Příprava VMware](vmware-walkthrough-prepare-vmware.md)

## <a name="step-7-set-up-a-vault"></a>Krok 7: Nastavení trezoru

Potřebujete tooset nahoru tooorchestrate trezoru služeb zotavení a správě replikace. Při nastavování hello trezoru, určíte, co chcete tooreplicate, a místo, kam chcete tooreplicate jeho.

Přejděte příliš[krok 7: nastavení trezoru](vmware-walkthrough-create-vault.md)

## <a name="step-8-configure-source-and-target-settings"></a>Krok 8: Konfigurace nastavení zdroje a cíle

Nastavte hello zdroj a cíl, který se používá pro replikaci. Nastavení nastavení zdroje zahrnuje spuštění tooinstall Unified instalace součásti Site Recovery místní hello.

Přejděte příliš[krok 8: nastavení hello zdroje a cíle](vmware-walkthrough-source-target.md)

## <a name="step-9-set-up-a-replication-policy"></a>Krok 9: Nastavení zásad replikace

Můžete nastavit nastavení replikace toospecify zásady pro virtuální počítače VMware v trezoru hello.

Přejděte příliš[krok 9: nastavení zásad replikace](vmware-walkthrough-replication.md)

## <a name="step-10-install-hello-mobility-service"></a>Krok 10: Instalace služby Mobility hello

musí být nainstalován Hello služba Mobility na jednotlivé virtuální počítače chcete tooreplicate. Existuje několik způsobů tooset až služba hello se instalace push nebo pull.

Přejděte příliš[krok 10: instalace služby Mobility hello](vmware-walkthrough-install-mobility.md)

## <a name="step-11-enable-replication"></a>Krok 11: Povolení replikace

Po hello služba Mobility je spuštěna na virtuálním počítači můžete povolit pro něho replikaci. Když povolíte, dojde k počáteční replikaci hello virtuálních počítačů.

Přejděte příliš[krok 11: povolení replikace](vmware-walkthrough-enable-replication.md)

## <a name="step-12-run-a-test-failover"></a>Krok 12: Spuštění testu převzetí služeb

Po dokončení počáteční replikace a je spuštěna rozdílová replikace, můžete spustit toomake testovací převzetí služeb při selhání, se, že vše funguje podle očekávání.

Přejděte příliš[krok 12: spustit testovací převzetí služeb](vmware-walkthrough-test-failover.md)
