---
title: "Vytvoření více virtuálních počítačů | Microsoft Docs"
description: "Možnosti pro vytvoření více virtuálních počítačů v systému Windows"
services: virtual-machines-windows
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: dfc1d1bb-a47d-4d7c-9fd2-f12050baacab
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2016
ms.author: guybo
ms.openlocfilehash: 5e96805f8880a30a5fc8779d8f07addb6d068c09
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-multiple-azure-virtual-machines"></a>Vytvoření několika virtuálních počítačů Azure
Existuje mnoho scénářů, kde je potřeba vytvořit velký počet podobných virtuálních počítačů (VM). Mezi příklady patří vysoce výkonné výpočetní (HPC), rozsáhlé datové analýzy, škálovatelná a často bezstavové střední vrstvy nebo back-end serverů (například webové servery) a distribuované databáze.

Tento článek popisuje možnosti dostupné v Azure vytvořit víc virtuálních počítačů. Tyto možnosti překročila jednoduchých případech, kdy ručně vytvoříte řadu virtuálních počítačů. Pokud chcete vytvořit mnoho virtuálních počítačů, procesů, které se obvykle používají nemáte škálovat i v případě, že je potřeba vytvořit víc než několik virtuálních počítačů.

Jeden způsob, jak vytvořit hodně podobné virtuálních počítačů je použít konstrukce Azure Resource Manageru z *prostředků smyčky*.

## <a name="resource-loops"></a>Smyčky prostředků
Smyčky prostředků jsou syntaktické sdružená v šablonách Azure Resource Manager. Smyčky prostředků můžete vytvořit sadu podobně nakonfigurované prostředky ve smyčce. Smyčky prostředků můžete vytvořit více účtů úložiště, síťová rozhraní nebo virtuálních počítačů. Další informace o prostředku smyčky, najdete v části [vytvořit virtuální počítače ve skupinách dostupnosti pomocí smyčky prostředků](https://azure.microsoft.com/documentation/templates/201-vm-copy-index-loops/).

## <a name="challenges-of-scale"></a>Výzvy měřítka
I když smyčky prostředků usnadňují vytváří infrastruktura cloudu ve velkém měřítku a vytvořit přesnější šablony, zůstanou některé problémy. Například pokud použijete smyčku prostředků k vytvoření 100 virtuálních počítačů, budete muset korelovat řadiče síťového rozhraní (NIC) s odpovídající virtuální počítače a účty úložiště. Vzhledem k tomu, že počet virtuálních počítačů je pravděpodobně lišit od počet účtů úložiště, budete mít jak nakládat s velikostí prostředků jiného smyčky, příliš. Jedná se o solvable problémy, ale složitost výrazně zvyšuje s měřítkem.

Dalším problémem nastane, když potřebovat infrastrukturu, která je Elasticky škálovatelná. Například můžete škálování infrastruktura, která automaticky zvyšuje nebo snižuje počet virtuálních počítačů v reakci na zatížení. Virtuální počítače neposkytují mechanismus integrované ke změně počtu (škálování a škálování v). Pokud je škálovat v odebráním virtuálních počítačů, je obtížné zajistit vysokou dostupnost tím, že zajistí, že virtuální počítače jsou vyváženy napříč doménami aktualizace a odolnost.

Při použití smyčku prostředků více volání vytvořit prostředky přejděte ke podkladových prostředků infrastruktury. Při volání více vytvořit podobné prostředky, Azure má příležitost implicitní zdokonalit tento návrh a optimalizovat nasazení spolehlivost a výkon. To je, kdy *sady škálování virtuálního počítače* mají.

## <a name="virtual-machine-scale-sets"></a>Škálovací sady virtuálních počítačů
Sady škálování virtuálního počítače jsou prostředek Azure Compute k nasazení a správě sadu identické virtuální počítače. U všech virtuálních počítačů nakonfigurovaný stejné, měřítko virtuálních počítačů, které nastavuje jsou snadno škálovat v a horizontální rozšíření kapacity. Jednoduše změnit počet virtuálních počítačů v sadě. Můžete také nakonfigurovat škálovatelné sady virtuálních počítačů, které chcete používat automatické škálování na základě požadavků pracovního vytížení.

Pro aplikace, které je potřeba škálovat výpočetní prostředky v a jsou operace škálování implicitně vyvážená doménách selhání a aktualizace.

Místo korelace více prostředků, jako jsou síťové karty a virtuální počítače, sadu škálování virtuálního počítače má sítě, úložiště, virtuální počítač a vlastnosti rozšíření, která můžete konfigurovat centrálně.

Úvod do sady škálování virtuálních počítačů, najdete v části [sadách škálování virtuálních počítačů stránky produktu](https://azure.microsoft.com/services/virtual-machine-scale-sets/). Podrobné informace, přejděte na [sadách škálování virtuálních počítačů dokumentaci](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/).

