---
title: "aaaReview hello předpoklady pro replikaci technologie Hyper-V tooAzure (s nástrojem System Center VMM) pomocí Azure Site Recovery | Microsoft Docs"
description: "Popisuje hello požadavky pro nastavení replikace, převzetí služeb při selhání a obnovení virtuálních počítačů technologie Hyper-V na místě v tooAzure cloudy VMM, s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a1c30fd5-c979-473c-af44-4f725ad3e3ba
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/24/2017
ms.author: raynew
ms.openlocfilehash: de13a2d80b1a9a5d968a180d559f661ab11e70c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-2-review-hello-prerequisites-for-hyper-v-with-vmm-tooazure-replication"></a>Krok 2: Kontrola hello předpoklady pro replikaci tooAzure technologie Hyper-V (s nástrojem VMM)

Poté, co jste prohlédli hello [architekturu scénáře](vmm-to-azure-walkthrough-architecture.md), přečtěte si tento článek toomake rozumět požadavky nasazení hello. 

## <a name="prerequisites-and-limitations"></a>Požadavky a omezení

**Požadavek** | **Podrobnosti**
--- | ---
**Účet Azure** | Potřebujete [účet Microsoft Azure](http://azure.microsoft.com/).
**Úložiště Azure** | Je třeba data toostore replikovat účtu úložiště Azure.<br/><br/> účet úložiště Hello musí být v hello trezoru služeb zotavení Azure stejné oblasti jako hello.<br/><br/>Můžete použít [geograficky redundantní úložiště](../storage/common/storage-redundancy.md#geo-redundant-storage) nebo místně redundantní úložiště. Doporučujeme použít geograficky redundantní úložiště. S geograficky redundantní úložiště byla zajištěna odolnost dat, pokud oblastního výpadku nebo pokud není možné obnovit primární oblast hello.<br/><br/> Můžete použít účet úložiště Azure úrovně Standard nebo [Azure Storage úrovně Premium](../storage/common/storage-premium-storage.md). Služba Storage úrovně Premium může být hostitelem úloh náročných na vstupně-výstupní operace a obvykle se používá pro virtuální počítače, které potřebují trvale vysoký výkon vstupně-výstupních operací a nízkou latenci. Pokud použijete službu Storage úrovně Premium pro replikovaná data, potřebujete také účet úložiště úrovně Standard. Standardní účet úložiště ukládá protokoly replikace, které zaznamenání dat tooon místní probíhající změny.
**Síť Azure** | Budete potřebovat [síť Azure](../virtual-network/virtual-network-get-started-vnet-subnet.md), toowhich virtuální počítače Azure po převzetí služeb při selhání připojit. Hello síť Azure musí být v hello stejné oblasti jako hello trezoru služeb zotavení.
**Místní servery VMM** | Potřebujete jeden nebo více serverů VMM s nástrojem System Center 2012 R2 nebo novějším.<br/><br/> Každý server VMM musí mít jeden nebo více privátních cloudů. Každý cloud potřebuje jednu nebo více skupin hostitelů.<br/><br/> Hello VMM server potřebuje přístup k Internetu.
**Místní Hyper-V** | Hostitelské servery Hyper-V musí běžet minimálně Windows Server 2012 R2 s povolenou roli hello technologie Hyper-V nebo Microsoft Hyper-V Server 2012 R2. musí být nainstalované nejnovější aktualizace Hello.<br/><br/> Hello hostitele Hyper-V se musí nacházet ve skupině hostitelů VMM (umístěný v cloudu VMM).<br/><br/> Hostitel musí mít jeden nebo více virtuálních počítačů, které chcete tooreplicated.<br/><br/> Hostitelé technologie Hyper-V musí být připojené toohello Internetu pro replikaci tooAzure přímo nebo pomocí serveru proxy. Servery Hyper-V musí mít hello opravy popsanou v článku [2961977](https://support.microsoft.com/kb/2961977).
**Místní virtuální počítače Hyper-V** | Virtuální počítače chcete tooreplicate by měla být spuštěná [podporovaný operační systém](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)a v souladu s [požadavky Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements). název virtuálního počítače Hello můžete změnit, jakmile je zapnutá replikace. 




## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 3: plánování kapacity](vmm-to-azure-walkthrough-capacity.md)
