---
title: "Úvod do topologii sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje přehled možností topologie sledovací proces sítě"
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
ms.openlocfilehash: 42443f614b76b8180ac163b9889163021adbf048
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-topology-in-azure-network-watcher"></a>Úvod do topologii sledovací proces sítě Azure

Topologie vrátí graf síťovým prostředkům ve virtuální síti. Graf znázorňuje propojení mezi prostředky představující koncové síťové připojení.

![Přehled topologie][1]

Na portálu, vrátí topologie objektů prostředků v na základě virtuální sítě. Vztahy, jsou použité v ukázkách linkami mezi prostředky prostředky mimo oblast sledovací proces sítě, i když v prostředku skupiny se nezobrazí. Prostředky, vrátí se v zobrazení portálu, jsou podmnožinou síťové součásti, které vykreslovacích. Pokud chcete zobrazit úplný seznam síťových prostředků, můžete použít [prostředí PowerShell](network-watcher-topology-powershell.md) nebo [REST](network-watcher-topology-rest.md)

> [!NOTE]
> V každé oblasti, kterou chcete spustit topologie na je vyžadován instanci sledovací proces sítě.

Prostředky jsou vrácený připojení mezi nimi jsou modelovat v dva vztahy.

- **Členství ve skupině** – příklad: virtuální síť obsahuje podsítě, který obsahuje síťový adaptér
- **Související** – příklad: síťový adaptér A je přidružený virtuální počítač

### <a name="next-steps"></a>Další kroky

Další informace o použití prostředí PowerShell k načtení zobrazení topologie navštivte stránky [sledovací proces sítě topologie pomocí prostředí PowerShell](network-watcher-topology-powershell.md)

<!--Image references-->

[1]: ./media/network-watcher-topology-overview/topology.png
