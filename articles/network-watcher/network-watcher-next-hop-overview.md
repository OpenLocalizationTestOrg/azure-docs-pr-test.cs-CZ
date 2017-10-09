---
title: "aaaIntroduction toonext směrování v sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje přehled hello sledovací proces sítě další funkce směrování"
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
ms.openlocfilehash: 916af736d0d52abadeafed746f0f8a0173b11033
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toonext-hop-in-azure-network-watcher"></a>Úvod toonext směrování v sledovací proces sítě Azure

Přenosy z virtuálního počítače se budou odesílat tooa cíl v závislosti na hello účinné postupy spojené s síťový adaptér. Další směrování získá hello typ dalšího směrování a IP adresa z paketu z konkrétní virtuální počítač a síťový adaptér. To pomáhá toodetermine, pokud hello paketů se směrovanou toohello cílové nebo je dírkového hello provoz probíhá černé. Nesprávná konfigurace trasy uživatelem hello, kde je přenosem směrovanou tooan místní umístění nebo virtuální zařízení, může způsobit problémy s tooconnectivity. Další směrování také vrátí hodnotu hello směrovací tabulce přidružené k dalším místě směrování hello. Při dotazování další směrování, pokud trasu hello je definován jako trasy definované uživatelem, bude vrácen danou trasu. V opačném případě vrátí další segment "Systémová trasa".

![dalšího směrování – přehled][1]

Hello následuje seznam hello další směrování typy, které mohou být vráceny při dotazování dalším místě směrování.

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* HyperNetGateway
* VnetPeering
* Žádný

### <a name="next-steps"></a>Další kroky

Zjistěte, jak toouse další směrování toofind problémy s připojením k síti navštivte stránky [kontrola hello dalšího přechodu na virtuálním počítači](network-watcher-check-next-hop-portal.md)

<!--Image references-->
[1]: ./media/network-watcher-next-hop-overview/figure1.png













