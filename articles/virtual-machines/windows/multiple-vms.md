---
title: "aaaCreate více virtuálních počítačů | Microsoft Docs"
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
ms.openlocfilehash: 37729fabd91049744f6bd94c55221f5c1c65d527
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-multiple-azure-virtual-machines"></a>Vytvoření několika virtuálních počítačů Azure
Existuje mnoho scénářů, kde je nutné toocreate velký počet podobných virtuálních počítačů (VM). Mezi příklady patří vysoce výkonné výpočetní (HPC), rozsáhlé datové analýzy, škálovatelná a často bezstavové střední vrstvy nebo back-end serverů (například webové servery) a distribuované databáze.

Tento článek popisuje dostupné možnosti toocreate hello více virtuálních počítačů v Azure. Tyto možnosti překročila hello jednoduchých případech, kdy ručně vytvoříte řadu virtuálních počítačů. toocreate hodně virtuálních počítačů, hello procesy, které se obvykle používají nemáte škálovat i v případě, že potřebujete toocreate víc než několik virtuálních počítačů.

Jedním ze způsobů toocreate hodně podobné virtuálních počítačů je toouse hello Azure Resource Manager konstrukce z *prostředků smyčky*.

## <a name="resource-loops"></a>Smyčky prostředků
Smyčky prostředků jsou syntaktické sdružená v šablonách Azure Resource Manager. Smyčky prostředků můžete vytvořit sadu podobně nakonfigurované prostředky ve smyčce. Toocreate smyčky prostředků můžete použít více účtů úložiště, síťová rozhraní nebo virtuální počítače. Další informace o prostředku smyčky, najdete v části příliš[vytvořit virtuální počítače ve skupinách dostupnosti pomocí smyčky prostředků](https://azure.microsoft.com/documentation/templates/201-vm-copy-index-loops/).

## <a name="challenges-of-scale"></a>Výzvy měřítka
I když prostředků smyčky bylo snazší toobuild out infrastruktury cloudu ve velkém měřítku a vytvořit přesnější šablony, zůstanou některé problémy. Například pokud chcete použít prostředek smyčky toocreate 100 virtuálních počítačů, musíte toocorrelate řadiče síťového rozhraní (NIC) s odpovídající virtuální počítače a účty úložiště. Protože hello počet virtuálních počítačů je pravděpodobně toobe liší od hello počet účtů úložiště, budete mít toodeal s velikostí prostředků jiného smyčky, příliš. Jedná se o solvable problémy, ale hello složitost výrazně zvyšuje s měřítkem.

Dalším problémem nastane, když potřebovat infrastrukturu, která je Elasticky škálovatelná. Například můžete škálování infrastruktura, která automaticky zvyšuje nebo snižuje hello počet virtuálních počítačů v tooworkload odpovědi. Virtuální počítače neposkytují toovary integrované mechanismus v číslo (škálování a škálování v). Pokud škálování v odebráním virtuálních počítačů je obtížné tooguarantee vysokou dostupnost tím, že zajistí, že virtuální počítače jsou vyváženy napříč doménami aktualizace a odolnost.

Pokud používáte smyčku prostředků, několik prostředků toocreate volání přejděte toohello základních prostředků infrastruktury. Když více volání vytvořit podobné prostředky, Azure má tooimprove implicitní možnost při tomto návrhu a optimalizovat nasazení spolehlivost a výkon. To je, kdy *sady škálování virtuálního počítače* mají.

## <a name="virtual-machine-scale-sets"></a>Škálovací sady virtuálních počítačů
Sady škálování virtuálního počítače se Azure Compute toodeploy prostředků a spravovat sadu identické virtuální počítače. S všechny virtuální počítače nakonfigurované hello stejné, škálovatelné sady virtuálních počítačů jsou snadno tooscale v a škálování. Jednoduše změňte hello počet virtuálních počítačů v sadě hello. Můžete také nakonfigurovat tooautoscale sady škálování virtuálního počítače na základě požadavků hello hello úlohy.

Pro aplikace, které potřebují tooscale výpočetní prostředky v škálování a operace jsou implicitně rovnoměrně rozdělen mezi doménách selhání a aktualizace.

Místo korelace více prostředků, jako jsou síťové karty a virtuální počítače, sadu škálování virtuálního počítače má sítě, úložiště, virtuální počítač a vlastnosti rozšíření, která můžete konfigurovat centrálně.

Nastaví měřítku tooVM úvod, naleznete toohello [sadách škálování virtuálních počítačů stránky produktu](https://azure.microsoft.com/services/virtual-machine-scale-sets/). Podrobné informace, přejděte toohello [sadách škálování virtuálních počítačů dokumentaci](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/).

