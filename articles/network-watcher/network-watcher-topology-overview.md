---
title: "aaaIntroduction tootopology v sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje přehled možností topologie hello sledovací proces sítě"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e753a435-38e0-482b-846b-121cb547555c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 7fa1c5518e4a25a5db999d898a9ee19fd0121db7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tootopology-in-azure-network-watcher"></a>Úvod tootopology v sledovací proces sítě Azure

Topologie vrátí graf síťovým prostředkům ve virtuální síti. Hello graf znázorňuje hello propojení mezi hello prostředky toorepresent hello end tooend připojení k síti.

![Přehled topologie][1]

Topologie hello portálu, vrátí hello objektů prostředků v na základě virtuální sítě. vztahy Hello, jsou použité v ukázkách linkami mezi prostředky hello prostředky mimo oblast hello sledovací proces sítě, i v případě, že v hello prostředku skupiny se nezobrazí. Hello prostředky, vrátí se v zobrazení portálu hello jsou podmnožinou hello síťové součásti, které vykreslovacích. Úplný seznam toosee hello síťové prostředky můžete použít [prostředí PowerShell](network-watcher-topology-powershell.md) nebo [REST](network-watcher-topology-rest.md)

> [!NOTE]
> V každé oblasti, které chcete toorun topologie na je vyžadován instanci sledovací proces sítě.

Prostředky jsou vrácený hello připojení mezi nimi jsou modelovat v dva vztahy.

- **Členství ve skupině** – příklad: virtuální síť obsahuje podsítě, který obsahuje síťový adaptér
- **Související** – příklad: síťový adaptér A je přidružený virtuální počítač

### <a name="next-steps"></a>Další kroky

Zjistěte, jak toouse prostředí PowerShell tooretrieve hello topologie zobrazit tak, že navštívíte [sledovací proces sítě topologie pomocí prostředí PowerShell](network-watcher-topology-powershell.md)

<!--Image references-->

[1]: ./media/network-watcher-topology-overview/topology.png
