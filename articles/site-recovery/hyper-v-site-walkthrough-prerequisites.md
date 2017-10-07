---
title: "aaaReview hello předpoklady pro replikaci technologie Hyper-V tooAzure (bez System Center VMM) pomocí Azure Site Recovery | Microsoft Docs"
description: "Popisuje hello požadavky pro nastavení replikace, převzetí služeb při selhání a obnovení tooAzure místní virtuální počítače Hyper-V s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 7ef3fb46-52f5-4c8a-b1a1-658c2305762a
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: 3eefc3b7e3982ec6c413c1db7f7784863f9c701d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-2-review-hello-prerequisites-for-hyper-v-without-vmm-tooazure-replication"></a>Krok 2: Kontrola hello předpoklady pro replikaci tooAzure technologie Hyper-V (bez VMM)

Hello požadavky jsou shrnuty v tabulce hello.


**Požadavek** | **Podrobnosti** 
--- | --- 
**Azure** | Přečtěte si o [požadavcích na Azure](site-recovery-prereq.md#azure-requirements).
**Místní servery** | [Další informace](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm) o požadavcích pro hostitele Hyper-V místní hello.
**Místní virtuální počítače Hyper-V** | Virtuální počítače chcete tooreplicate by měla být spuštěná [podporovaný operační systém](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)a v souladu s [požadavky Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
**Adresy URL Azure** | Hostitelé Hyper-V, potřebujete přístup toothese adresy URL:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Pokud máte pravidla brány firewall založená na adresu IP, ověřte, že umožňují tooAzure komunikace.<br/></br> Povolit hello [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653)a hello port HTTPS (443).<br/></br> Povolte rozsahy IP adres pro hello oblast Azure svého předplatného a západní USA (používá se pro správu Identity a řízení přístupu).



## <a name="next-steps"></a>Další kroky

- Pokud jste to úplné nasazení, přejděte příliš[krok 3: plánování kapacity](hyper-v-site-walkthrough-capacity.md)
- Pokud jste to jednoduchá testovací nasazení, přejděte příliš[krok 4: plánování sítě](hyper-v-site-walkthrough-network.md).
