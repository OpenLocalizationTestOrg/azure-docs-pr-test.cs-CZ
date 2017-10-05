---
title: "Vytvoření virtuálního počítače na portálu Azure | Microsoft Docs"
description: "Vytvoření virtuálního počítače s Windows v portálu Azure."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1871f823-ebd7-4eff-9a22-8e2411555595
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 0981872ff819fdf49a9cc97afce3c212013ce76b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-machine-running-windows-in-the-azure-portal"></a>Vytvoření virtuálního počítače se systémem Windows na portálu Azure
> [!div class="op_single_selector"]
> * [Azure Portal](tutorial.md)
> * [PowerShell: Nasazení Classic](create-powershell.md)
>
>

<br>

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic. Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager. Zjistěte, jak [proveďte tyto kroky, pomocí modelu nasazení Resource Manager](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) pomocí **portál Azure**.

V tomto kurzu se dozvíte, jak vytvořit virtuální počítač Azure (VM) s Windows v portálu Azure. Jako příklad použijeme bitovou kopii systému Windows Server, ale který je jen jednou z mnoha imagí, které Azure nabízí. Všimněte si, že tato volba image závisí na vaše předplatné. Například může být plochy bitových kopií systému Windows k dispozici předplatitelům služby MSDN.

V této části se dozvíte, jak používat **řídicí panel** na portálu Azure vyberte a poté vytvořit virtuální počítač.

Můžete také vytvořit virtuální počítače pomocí [vlastní image](createupload-vhd.md). Informace o tomto a dalších metod najdete v tématu [různé způsoby vytvoření virtuálního počítače s Windows](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

<!-- 02/27/2017 Video removed as it was based on the classic portal. -->

## <a id="createvirtualmachine"></a>Vytvoření virtuálního počítače
[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak [vytvoření virtuálního počítače pomocí modelu nasazení Resource Manager](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) na portálu Azure.
* Přihlaste se k virtuálnímu počítači. Pokyny najdete v tématu [Přihlaste se k virtuálního počítače se systémem Windows Server](connect-logon.md).
* Připojte disk k uložení dat. Můžete připojit prázdné disky a disky, které obsahují data. Pokyny najdete v tématu [připojit datový disk k virtuálnímu počítači Windows vytvořené pomocí modelu nasazení classic](attach-disk.md).
