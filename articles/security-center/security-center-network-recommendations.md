---
title: "aaaProtecting síti v Azure Security Center | Microsoft Docs"
description: "Tento dokument adresy doporučení v Azure Security Center, které vám pomůžou chránit vaši síť Azure a zůstat souladu se zásadami zabezpečení."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 96c55a02-afd6-478b-9c1f-039528f3dea0
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: terrylan
ms.openlocfilehash: 053738da432edf13b40172fb44d2044702dd8211
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="protecting-your-network-in-azure-security-center"></a>Ochrana sítě v Azure Security Center
Azure Security Center analyzuje stav zabezpečení hello vašich prostředků Azure. Když Security Center identifikuje potenciální ohrožení zabezpečení, vytvoří se doporučení, která vás provede procesem hello konfigurace hello potřebné ovládací prvky.  Doporučení se týkají tooAzure typy prostředků: virtuální počítače (VM), sítě, SQL a aplikace.

Tento článek se zaměřuje na doporučení, které se vztahují tooyour sítě.  Síťového střediska doporučení ohledně další brány firewall generace, skupiny zabezpečení sítě, konfiguraci pravidla pro příchozí provoz a další.  Použití hello tabulce jako referenční toohelp porozumíte hello dostupná síťová doporučení a co každé z nich dělá, pokud ji použijete.

## <a name="available-network-recommendations"></a>Doporučení k dispozici sítě
| Doporučení | Popis |
| --- | --- |
| [Přidání brány firewall příští generace](security-center-add-next-generation-firewall.md) |Doporučuje se, že přidáte brány Firewall pro další generace (NGFW) z tooincrease partnera Microsoft vaše ochranu zabezpečení. |
| [Směrování provozu jenom přes NGFW](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |Doporučuje konfiguraci pravidla zabezpečení skupiny (NSG) sítě, které vynutí tooyour příchozí přenosy virtuálních počítačů prostřednictvím vaší NGFW. |
| [Povolení skupin zabezpečení sítě pro podsítě nebo virtuální počítače](security-center-enable-network-security-groups.md) |Doporučuje, abyste povolili skupiny Nsg na podsítě nebo virtuálních počítačů. |
| [Omezení přístupu prostřednictvím internetové koncový bod](security-center-restrict-access-through-internet-facing-endpoints.md) |Doporučuje konfigurace pravidla pro příchozí provoz pro skupiny Nsg. |

## <a name="see-also"></a>Viz také
toolearn Další informace o doporučení, které se vztahují tooother typy prostředků Azure, najdete v části hello následující:

* [Ochrana virtuálních počítačů v Azure Security Center](security-center-virtual-machine-recommendations.md)
* [Ochrana aplikací v Azure Security Center](security-center-application-recommendations.md)
* [Ochrana služby Azure SQL v Azure Security Center](security-center-sql-service-recommendations.md)

toolearn Další informace o Security Center, najdete v části hello následující:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
