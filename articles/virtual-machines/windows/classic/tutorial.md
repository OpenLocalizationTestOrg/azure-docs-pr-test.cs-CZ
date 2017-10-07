---
title: "aaaCreate virtuálního počítače v hello portálu Azure | Microsoft Docs"
description: "Vytvoření virtuálního počítače s Windows v hello portálu Azure."
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
ms.openlocfilehash: 3848a3d06a7ce377633db78ea385826ed40f94fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-running-windows-in-hello-azure-portal"></a>Vytvoření virtuálního počítače se systémem Windows v hello portálu Azure
> [!div class="op_single_selector"]
> * [Azure Portal](tutorial.md)
> * [PowerShell: Nasazení Classic](create-powershell.md)
>
>

<br>

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Zjistěte, jak příliš[proveďte tyto kroky, pomocí modelu nasazení Resource Manager hello](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) pomocí hello **portál Azure**.

Tento kurz ukazuje, jak toocreate Azure virtuálního počítače (VM) s Windows v hello portálu Azure. Jako příklad použijeme bitovou kopii systému Windows Server, ale který je jen jednou z hello mnoho Image Azure nabízí. Všimněte si, že tato volba image závisí na vaše předplatné. Například stolní bitových kopií systému Windows může být k dispozici tooMSDN odběratele.

V této části se dozvíte, jak toouse hello **řídicí panel** v hello Azure tooselect portálu a poté vytvořit hello virtuálního počítače.

Můžete také vytvořit virtuální počítače pomocí [vlastní image](createupload-vhd.md). toolearn o tomto a dalších metod najdete v části [různé způsoby toocreate virtuálního počítače s Windows](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

<!-- 02/27/2017 Video removed as it was based on hello classic portal. -->

## <a id="createvirtualmachine"></a>Vytvoření hello virtuálního počítače
[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[vytvoření virtuálního počítače pomocí modelu nasazení Resource Manager hello](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) v hello portálu Azure.
* Přihlaste se toohello virtuálního počítače. Pokyny najdete v tématu [přihlásit tooa virtuální počítač se systémem Windows Server](connect-logon.md).
* Připojte datový disk toostore. Můžete připojit prázdné disky a disky, které obsahují data. Pokyny najdete v tématu hello [připojit datový disk tooa virtuálního počítače s Windows vytvořené pomocí modelu nasazení classic hello](attach-disk.md).
