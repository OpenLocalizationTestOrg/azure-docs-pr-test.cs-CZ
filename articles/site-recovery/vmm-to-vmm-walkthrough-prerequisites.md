---
title: "aaaReview hello požadavky pro Hyper-V replikace tooa sekundární lokalita VMM s Azure Site Recovery | Microsoft Docs"
description: "Popisuje hello požadavky na replikaci virtuálních počítačů Hyper-V tooa sekundární lokalita VMM s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 21ff0545-8be5-4495-9804-78ab6e24add6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 1bd945fdda36c3cce5d159209abbd3c98a7e3682
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-2-review-hello-prerequisites-and-limitations-for-hyper-v-vm-replication-tooa-secondary-vmm-site"></a>Krok 2: Kontrola hello předpoklady a omezení pro virtuální počítač Hyper-V replikace tooa sekundární lokalita VMM


Po zkontrolování hello [architekturu scénáře](vmm-to-vmm-walkthrough-architecture.md), přečtěte si tento článek toomake musíte rozumět hello požadavcích pro nasazení, při replikaci místní technologie Hyper-V virtuálních počítačů (VM) spravovaných v System Center virtuální Machine Manager (VMM) cloudů, tooa sekundární lokality pomocí [Azure Site Recovery](site-recovery-overview.md) v hello portálu Azure.

Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prerequisites-and-limitations"></a>Požadavky a omezení

**Požadavek** | **Podrobnosti**
--- | ---
**Azure** | A [Microsoft Azure](http://azure.microsoft.com/) předplatné.<br/><br/> Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> [Další informace](https://azure.microsoft.com/pricing/details/site-recovery/) o cenách za Site Recovery<br/><br/> Zkontrolujte hello podporované oblasti pro obnovení lokality v rámci geografickou dostupností v [Azure Site Recovery podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).
**Servery VMM** | Doporučujeme, že abyste měli dva servery VMM, jeden v primární lokalitě hello a jeden v hello sekundární.<br/><br/> Replikace mezi cloudy na jednom serveru VMM je podporována.<br/><br/> Servery VMM by měl používat minimálně System Center 2012 SP1 s nejnovějšími aktualizacemi hello.<br/><br/> Servery VMM potřebovat přístup k Internetu.
**Cloudy VMM** | Každý server VMM musí mít na jeden nebo více cloudů a všechny cloudy musí mít nastaven profil hello kapacity technologie Hyper-V. <br/><br/>Cloudy musí obsahovat jeden nebo více skupin hostitelů VMM.<br/><br/> Pokud máte pouze jeden server VMM, musí aspoň dva cloudy, tooact jako primární a sekundární.
**Hyper-V** | Servery Hyper-V musí běžet minimálně Windows Server 2012 s rolí hello technologie Hyper-V, a mít hello nainstalované nejnovější aktualizace.<br/><br/> Server Hyper-V by měl obsahovat jeden nebo více virtuálních počítačů.<br/><br/>  Hostitelské servery Hyper-V by měl být umístěn v skupiny hostitelů v hello primárních a sekundárních cloudech VMM.<br/><br/> Pokud spustíte technologie Hyper-V v clusteru na Windows Server 2012 R2, nainstalujte [aktualizovat 2961977](https://support.microsoft.com/kb/2961977)<br/><br/> Pokud spustíte technologie Hyper-V v clusteru na Windows Server 2012, zprostředkovatele clusteru se nevytvoří automaticky, pokud máte statické IP adresy na základě clusteru. Ruční konfigurace zprostředkovatele clusteru hello. [Přečtěte si další informace](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).<br/><br/> Servery Hyper-V potřebují přístup k Internetu.




## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 3: plánování sítě](vmm-to-vmm-walkthrough-network.md).
