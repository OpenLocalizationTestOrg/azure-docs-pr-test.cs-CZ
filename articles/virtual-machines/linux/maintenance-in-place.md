---
title: "aaaVM zachování údržby pro virtuální počítače Windows v Azure | Microsoft Docs"
description: "Místní migrace virtuálních počítačů pro zachování aktualizace paměti."
services: virtual-machines-windows
documentationcenter: 
author: 
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
ms.author: 
ms.openlocfilehash: 544a2dcca52bb3ac51d341bceaf4ba3e7c71fd82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="vm-preserving-maintenance-in-place-vm-migration"></a>Virtuální počítač zachování údržby (místní počítač migrace)

Většina hello aktualizací používat žádné dopad toohosted virtuálních počítačů, jsou případy, kdy toocomponents aktualizace nebo služby výsledkem minimální narušení toorunning virtuálních počítačů (bez úplné restartování hello virtuálního počítače).

Tyto aktualizace jsou provést pomocí technologie, která umožňuje migrace za provozu na místě, označované taky jako "zachování paměti update". Při aktualizaci hello hostitele, hello je umístěn virtuální počítač do stavu "pozastavení", zachování hello paměti RAM, během hello hostitelské prostředí (například příslušný operační systém) hello potřebné aktualizace a opravy.
Hello virtuálního počítače je pak pokračuje v rámci 30 sekund byla pozastavena.
Po obnovení, je automaticky synchronizovat hodiny hello hello virtuálního počítače.

Ne všechny aktualizace můžete nasadit pomocí tento mechanismus, ale dané období krátké pozastavení nasazení aktualizací v tomto způsobem výrazně snižuje dopad toovirtual počítače.

Aktualizace s více instancemi (virtuálních počítačů v nastavení dostupnosti) jsou použité jednu aktualizační doménu najednou.

Některé aplikace může být ovlivněno tyto aktualizace více než jiné. Aplikace, které provádějí zpracování událostí v reálném čase, streamování médií nebo překódování nebo vysokou propustnost sítě scénáře, nemusí být například navrženou tootolerate 30 sekundové pauzy. Aplikace běžící na virtuálním počítači můžete další informace o nadcházejících aktualizace pomocí volání hello [naplánované události](../virtual-machines-scheduled-events.md) API hello [Azure Metadata služby](../virtual-machines-instancemetadataservice-overview.md).
