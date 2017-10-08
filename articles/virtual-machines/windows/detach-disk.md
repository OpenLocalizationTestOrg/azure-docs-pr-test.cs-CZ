---
title: "aaaDetach datový disk od virtuálního počítače Windows – Azure | Microsoft Docs"
description: "Přečtěte si toodetach datový disk z virtuálního počítače v Azure pomocí modelu nasazení Resource Manager hello."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 13180343-ac49-4a3a-85d8-0ead95e2028c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: cynthn
ms.openlocfilehash: f3f581d3f33329db2ecb7d25a68bc59af7361aad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodetach-a-data-disk-from-a-windows-virtual-machine"></a>Jak toodetach datový disk z virtuálního počítače s Windows
Pokud již nepotřebujete datový disk, který je připojený tooa virtuální počítač, můžete ho snadno odpojit. Odebere hello disk z hello virtuálního počítače, ale neodstraní z úložiště.

> [!WARNING]
> Pokud se odpojit disk není automaticky odstraněn. Pokud odebíráte tooPremium úložiště, budete moct dále tooincur poplatky za úložiště pro hello disk. Další informace najdete v části příliš[ceny a fakturace při použití služby Premium Storage](../../storage/common/storage-premium-storage.md#pricing-and-billing).
>
>

Pokud chcete toouse hello existující data na disku hello znovu, můžete ji můžete opět připojit toohello stejného virtuálního počítače nebo jiný.

## <a name="detach-a-data-disk-using-hello-portal"></a>Odpojit datový disk pomocí portálu hello
1. V portálu centra hello, vyberte **virtuální počítače**.
2. Vyberte hello virtuální počítač, který má datový disk hello toodetach a klikněte na **Zastavit** toodeallocate hello virtuálních počítačů.
3. V okně hello virtuálního počítače, vyberte **disky**.
4. Hello horní části hello **disky** vyberte **upravit**.
5. V hello **disky** okně toohello daleko vpravo od hello datový disk, které chcete toodetach, klikněte na tlačítko hello ![obrázek tlačítka odpojení](./media/detach-disk/detach.png) odpojit tlačítko.
5. Po hello disk odebrat, klikněte na Uložit na hello horní části okna hello.
6. V okně hello virtuální počítač, klikněte na tlačítko **přehled** a pak klikněte na tlačítko hello **spustit** tlačítko hello horní části hello okno toorestart hello virtuálních počítačů.



Hello disk zůstává v úložišti, ale je už připojené tooa virtuální počítač.

## <a name="detach-a-data-disk-using-powershell"></a>Odpojit datový disk pomocí prostředí PowerShell
V tomto příkladu hello první příkaz získá hello virtuální počítač s názvem **MyVM07** v hello **RG11** skupiny prostředků pomocí rutiny Get-AzureRmVM hello. příkaz hello úložiště virtuálního počítače v hello Hello **$VirtualMachine** proměnné.

druhý příkaz Hello odebere hello datový disk s názvem DataDisk3 z hello virtuálního počítače.

poslední příkaz Hello aktualizuje stav hello hello virtuálního počítače toocomplete hello proces odebrání hello datový disk.

```powershell
$VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07"
Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
Update-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" -VM $VirtualMachine
```

Další informace najdete v tématu [odebrat AzureRmVMDataDisk](/powershell/module/azurerm.compute/remove-azurermvmdatadisk).

## <a name="next-steps"></a>Další kroky
Pokud chcete tooreuse hello datový disk, jste právě [připojte ji tooanother virtuálních počítačů](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

