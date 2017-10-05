---
title: "Úvod do dalšího směrování v sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje přehled sledovací proces sítě další funkce směrování"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: febf7bca-e0b7-41d5-838f-a5a40ebc5aac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 5dd65d2418cae206965a13013dd990b916ad0733
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-next-hop-in-azure-network-watcher"></a>Úvod do dalšího směrování v sledovací proces sítě Azure

Přenosy z virtuálního počítače se odesílají do cílového umístění v závislosti na účinné postupy spojené s síťový adaptér. Další směrování získá typ dalšího směrování a IP adresa z paketu z konkrétní virtuální počítač a síťový adaptér. To pomůže určete, jestli je paket směrovat do cílového umístění nebo je provoz probíhá černým dírkového. Nesprávná konfigurace trasy uživatelem, kde je přesměrován na místní umístění nebo virtuální zařízení provoz, může způsobit problémy s připojením. Další směrování také vrátí hodnotu směrovací tabulka přidružené k dalším místě směrování. Při dotazování další směrování, pokud trasy, která je definována jako trasy definované uživatelem, bude vrácen danou trasu. V opačném případě vrátí další segment "Systémová trasa".

![dalšího směrování – přehled][1]

Následuje seznam další typy směrování, které mohou být vráceny při dotazování dalším místě směrování.

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* HyperNetGateway
* VnetPeering
* Žádný

### <a name="next-steps"></a>Další kroky

Další informace o použití další segment najít problémy s připojením k síti, navštivte stránky [zkontrolujte další směrování na virtuálním počítači](network-watcher-check-next-hop-portal.md)

<!--Image references-->
[1]: ./media/network-watcher-next-hop-overview/figure1.png













