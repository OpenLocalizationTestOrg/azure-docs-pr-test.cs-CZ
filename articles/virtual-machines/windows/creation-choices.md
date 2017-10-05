---
title: "Různé způsoby vytvoření virtuálního počítače s Windows v Azure | Microsoft Docs"
description: "Uvádí různé způsoby vytvoření virtuálního počítače s Windows pomocí Resource Manageru."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 809ba8f4-b54e-43c5-bbe3-8e710c49971f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/02/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5e51c49aac01a22d86c7c1a12b2f2ca7ddc056bc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="different-ways-to-create-a-windows-virtual-machine"></a>Různé způsoby vytvoření virtuálního počítače s Windows

Azure nabízí různé způsoby vytvoření virtuálního počítače, protože virtuální počítače jsou vhodné pro různé uživatele a účely. To znamená, že budete muset některé rozhodování o virtuální počítač a jak ji vytvořit. Tento článek poskytuje souhrn volby a odkazy na pokyny.

## <a name="azure-portal"></a>portál Azure
Pomocí portálu Azure je jednoduchý způsob, jak vyzkoušet na virtuálním počítači, zejména v případě, že jste právě začínáte s Azure. 

[Vytvoření virtuálního počítače s Windows pomocí portálu](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="template"></a>Šablona
Virtuální počítače vyžaduje kombinaci prostředků (například s dostupností sad a účty úložiště). Namísto nasazení a správa každého prostředku samostatně, můžete vytvořit šablonu Azure Resource Manager, která nasazuje a zřizuje všechny prostředky v rámci jediné koordinované operace.

* [Vytvoření virtuálního počítače s Windows pomocí šablony Resource Manageru](ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="azure-powershell"></a>Azure PowerShell
Pokud dáváte přednost práci v příkazovém řádku, můžete použít Azure PowerShell.

* [Vytvoření virtuálního počítače s Windows pomocí prostředí PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="visual-studio"></a>Visual Studio
Pomocí sady Visual Studio k vytvoření, správě a nasazení virtuálních počítačů pomocí nástroje Azure pro Visual Studio a sady Azure SDK.

[Nástroje Azure pro sadu Visual Studio](https://www.visualstudio.com/features/azure-tools-vs)

