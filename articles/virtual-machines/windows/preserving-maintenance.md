---
title: "Virtuální počítač zachování údržby pro virtuální počítače Windows v Azure | Microsoft Docs"
description: "Místní migrace virtuálních počítačů pro zachování aktualizace paměti."
services: virtual-machines-windows
documentationcenter: 
author: zivr
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: zivr
ms.openlocfilehash: 8888bafbc3aba24168312b611a9b4fbde25f376d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="vm-preserving-maintenance-with-in-place-vm-migration"></a>Virtuální počítač zachování údržby pomocí nástroje Migrace virtuálních počítačů na místě

Zatímco většina aktualizace nemají žádný vliv na hostované virtuální počítače, jsou případy, kdy aktualizace služeb nebo komponent způsobit minimální narušení k spuštěných virtuálních počítačů (bez úplné restartování virtuálního počítače).

Tyto aktualizace jsou provést pomocí technologie, která umožňuje migrace za provozu na místě, označované taky jako "zachování paměti update". Při aktualizaci hostitele, virtuální počítač je umístěn do stavu "pozastavení", zachování paměti v paměti RAM, zatímco hostitelské prostředí (například příslušný operační systém) se vztahuje potřebné aktualizace a opravy.
Virtuální počítač je potom pokračuje v rámci 30 sekund byla pozastavena.
Po obnovení se hodiny virtuálního počítače automaticky synchronizují.

Ne všechny aktualizace je možné nasadit pomocí tohoto mechanismu, ale díky krátkému období pozastavení má tento způsob nasazení aktualizací výrazně nižší vliv na virtuální počítače.

Aktualizace s více instancemi (virtuálních počítačů v nastavení dostupnosti) jsou použité jednu aktualizační doménu najednou.

Některé aplikace může být ovlivněno tyto aktualizace více než jiné. Aplikace, které provádějí zpracování událostí v reálném čase, streamování médií nebo překódování nebo vysokou propustnost sítě scénářů, například nemusí být navržena k tolerovat 30 sekundové pauzy. Aplikace běžící na virtuálním počítači můžete další informace o nadcházejících aktualizace voláním [naplánované události](../virtual-machines-scheduled-events.md) API [Metadata služby Azure](../virtual-machines-instancemetadataservice-overview.md).
