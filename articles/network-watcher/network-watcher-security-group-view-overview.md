---
title: "zobrazení skupiny toosecurity aaaIntroduction v sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje přehled hello schopností zobrazení zabezpečení sledovací proces sítě"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: ad27ab85-9d84-4759-b2b9-e861ef8ea8d8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: gwallace
ms.openlocfilehash: c2f6dbbffd0aedbb9db4b69d1758f2e66dd7abb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toonetwork-security-group-view-in-azure-network-watcher"></a>Zobrazení skupiny zabezpečení Úvod toonetwork v sledovací proces sítě Azure

Skupiny zabezpečení sítě na úrovni podsítě nebo na úrovni seskupování souvisejí. Když přidružené na úrovni podsítě, bude se vztahovat tooall hello instance virtuálních počítačů v podsíti hello. Skupina zabezpečení sítě zobrazení vrátí všechny hello nakonfigurované skupiny Nsg a pravidel, které jsou přidružené úrovni síťových Adaptérů a podsítě pro virtuální počítač poskytuje přehled o konfiguraci hello. Kromě toho pravidla efektivní zabezpečení hello se vrátí pro každou hello síťových adaptérů ve virtuálním počítači. Zobrazení pomocí skupiny zabezpečení sítě, můžete vyhodnotit virtuální počítač chyb zabezpečení sítě, jako je například otevřené porty. Můžete také ověřit, pokud vaše skupina zabezpečení sítě funguje podle očekávání, na základě [porovnání mezi hello nakonfigurované a hello pravidla efektivní zabezpečení](network-watcher-nsg-auditing-powershell.md).

Více rozšířených případ použití se dodržování předpisů pro zabezpečení a auditování. Můžete definovat doporučený sadu pravidel zabezpečení jako model pro řízení zabezpečení ve vaší organizaci. Audit pravidelné dodržování předpisů, můžou se implementovat v programový způsob tak, že porovnáte hello doporučený pravidla s hello efektivní pravidla pro každou hello virtuálních počítačů ve vaší síti.

V hello portálu pravidla jsou dělený platné, podsítě, síťové a výchozí. To poskytuje jednoduché zobrazení do hello pravidla použít tooa virtuálního počítače. Tlačítko Stáhnout se tooeasily stáhnout všechna pravidla zabezpečení hello bez ohledu na to, karta hello do souboru CSV.

![zobrazení skupiny zabezpečení][1]

Pravidla lze vybrat a otevře se nové okno tooshow hello skupinu zabezpečení sítě a zdrojové a cílové předpony. V tomto okně můžete přejít přímo prostředků toohello skupinu zabezpečení sítě.

![Přejít k podrobnostem][2]

### <a name="next-steps"></a>Další kroky

Zjistěte, jak tooaudit zabezpečení sítě skupiny nastavení, navštivte stránky [nastavení auditu skupinu zabezpečení sítě pomocí prostředí PowerShell](network-watcher-nsg-auditing-powershell.md)

[1]: ./media/network-watcher-security-group-view-overview/securitygroupview.png
[2]: ./media/network-watcher-security-group-view-overview/figure1.png









