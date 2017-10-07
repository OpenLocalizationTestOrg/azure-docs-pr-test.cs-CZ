---
title: "aaaDifferent způsoby toocreate virtuální počítač s Windows v Azure | Microsoft Docs"
description: "Uvádí různé způsoby toocreate hello virtuálního počítače s Windows pomocí Resource Manageru."
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
ms.openlocfilehash: 2928d4daa9b44c4d3a5083092a82c9a7f7c69fae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="different-ways-toocreate-a-windows-virtual-machine"></a>Různé způsoby toocreate virtuálního počítače s Windows

Azure nabízí různé způsoby toocreate vzhledem k tomu, že virtuální počítače jsou vhodné pro různé uživatele a účely virtuálního počítače. To znamená, je nutné toomake některé možnosti o hello virtuální počítač a jak toocreate ho. Tento článek poskytuje souhrn tyto volby a propojuje tooinstructions.

## <a name="azure-portal"></a>portál Azure
Použití hello portál Azure je jednoduchý způsob tootry na virtuálním počítači, zejména v případě, že jste právě začínáte s Azure. 

[Vytvoření virtuálního počítače se systémem Windows pomocí portálu hello](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="template"></a>Šablona
Virtuální počítače vyžaduje kombinaci prostředků (například s dostupností sad a účty úložiště). Namísto nasazení a správa každého prostředku samostatně, můžete vytvořit šablonu Azure Resource Manager, která nasazuje a zřizuje všechny hello prostředky v rámci jediné koordinované operace.

* [Vytvoření virtuálního počítače s Windows pomocí šablony Resource Manageru](ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="azure-powershell"></a>Azure PowerShell
Pokud dáváte přednost práci v příkazovém řádku, můžete použít Azure PowerShell.

* [Vytvoření virtuálního počítače s Windows pomocí prostředí PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="visual-studio"></a>Visual Studio
Pomocí sady Visual Studio toobuild, spravovat a nasazení virtuálních počítačů pomocí nástroje hello Azure pro sadu Visual Studio a hello Azure SDK.

[Nástroje Azure pro sadu Visual Studio](https://www.visualstudio.com/features/azure-tools-vs)

