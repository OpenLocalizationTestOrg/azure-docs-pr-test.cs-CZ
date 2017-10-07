---
title: "aaaReplicate fyzický místní servery tooAzure s Azure Site Recovery | Microsoft Docs"
description: "Poskytuje přehled hello kroky pro replikovat úlohy běžící na místní Windows nebo Linuxem fyzických serverů tooAzure s hello služba Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 20122f01-929a-4675-b85b-a9b99d2618bc
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: f801b9544072d4029ec06cc1abfd4ff370e852e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-physical-servers-tooazure-with-site-recovery"></a>Replikace fyzických serverů tooAzure službou Site Recovery

Tento článek obsahuje přehled hello kroky požadované tooreplicate místní Windows nebo Linuxem fyzických serverů tooAzure, pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.


## <a name="step-1-review-architecture-and-prerequisites"></a>Krok 1: Posouzení architektura a požadavky

Před zahájením nasazení, zkontrolujte architekturu scénáře hello a ujistěte se, že rozumíte všechny součásti hello potřebujete tooset až hello nasazení.

Přejděte příliš[krok 1: Zkontrolujte architektura hello](physical-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>Krok 2: Kontrola předpokladů

Ujistěte se, že máte nastavené pro jednotlivé součásti nasazení hello požadavky:

- **Požadavky Azure**: budete potřebovat účet Microsoft Azure, sítě Azure a účty úložiště.
- **Místní součásti Site Recovery**: budete potřebovat počítač se systémem součásti Site Recovery na místě.
- **Replikovat počítače**: servery, které chcete tooreplicate potřebovat toocomply s místními a požadavky pro Azure.

Přejděte příliš[krok 2: Přečtěte si požadavky a omezení](physical-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>Krok 3: Plánování kapacity

Pokud provádíte úplné nasazení musíte toofigure na jaké replikace prostředky, které potřebujete. Pokud jste to nějakou rychlou nastavení tootest hello prostředí, můžete tento krok přeskočit.

Přejděte příliš[krok 3: plánování kapacity](physical-walkthrough-capacity.md)

## <a name="step-4-plan-networking"></a>Krok 4: Plánování sítě

Je nutné toodo některé síťové plánování tooensure, že virtuální počítače Azure jsou připojené toonetworks po převzetí služeb při selhání, a že, ke kterým mají hello správné IP adres.

Přejděte příliš[krok 4: plánování sítě](physical-walkthrough-network.md)

##  <a name="step-5-prepare-azure-resources"></a>Krok 5: Příprava prostředků Azure

Nastavte před zahájením Azure sítím a úložišti. 

Přejděte příliš[krok 5: Příprava Azure](physical-walkthrough-prepare-azure.md)


## <a name="step-6-set-up-a-vault"></a>Krok 6: Nastavení trezoru

Můžete nastavit tooorchestrate trezoru služeb zotavení a spravovat replikace. Při nastavování hello trezoru, určíte, co chcete tooreplicate, a místo, kam chcete tooreplicate jeho.

Přejděte příliš[krok 6: nastavení trezoru](physical-walkthrough-create-vault.md)

## <a name="step-7-configure-source-and-target-settings"></a>Krok 7: Konfigurace nastavení zdroje a cíle

Nakonfigurujte nastavení pro hello zdrojové a cílové lokality (Azure). Nastavení zdroje zahrnuje spuštění tooinstall Unified instalace součásti Site Recovery místní hello.

Přejděte příliš[krok 7: nastavení hello zdroje a cíle](physical-walkthrough-source-target.md)

## <a name="step-8-set-up-a-replication-policy"></a>Krok 8: Nastavení zásad replikace

Nastavení zásad toospecify jak fyzické servery musí replikovat.

Přejděte příliš[krok 8: nastavení zásad replikace](physical-walkthrough-replication.md)

## <a name="step-9-install-hello-mobility-service"></a>Krok 9: Instalace služby Mobility hello

Služba Mobility Hello musí být nainstalován na každém serveru chcete tooreplicate. Existuje několik způsobů tooset až služba hello se instalace push nebo pull.

Přejděte příliš[krok 9: instalace služby Mobility hello](physical-walkthrough-install-mobility.md)

## <a name="step-10-enable-replication"></a>Krok 10: Povolení replikace

Po hello služba Mobility běží na serveru, můžete povolit pro něho replikaci. Když povolíte, dojde k počáteční replikaci hello virtuálních počítačů.

Přejděte příliš[krok 10: povolení replikace](physical-walkthrough-enable-replication.md)

## <a name="step-11-run-a-test-failover"></a>Krok 11: Spuštění testu převzetí služeb

Po dokončení počáteční replikace a je spuštěna rozdílová replikace, můžete spustit toomake testovací převzetí služeb při selhání, se, že vše funguje podle očekávání.

Přejděte příliš[krok 11: spustit testovací převzetí služeb](physical-walkthrough-test-failover.md)

