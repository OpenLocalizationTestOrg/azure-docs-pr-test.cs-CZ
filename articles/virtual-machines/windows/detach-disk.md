---
title: "Odpojit datový disk z virtuálního počítače Windows - Azure | Microsoft Docs"
description: "Naučte se odpojit datový disk z virtuálního počítače v Azure pomocí modelu nasazení Resource Manager."
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
ms.openlocfilehash: 97aa69745d200ee76f9f859eb3a8b0ad2f202bad
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a><span data-ttu-id="04acb-103">Jak se odpojit datový disk od virtuálního počítače s Windows</span><span class="sxs-lookup"><span data-stu-id="04acb-103">How to detach a data disk from a Windows virtual machine</span></span>
<span data-ttu-id="04acb-104">Když už nepotřebujete datový disk připojený k virtuálnímu počítači, můžete ho jednoduše odpojit.</span><span class="sxs-lookup"><span data-stu-id="04acb-104">When you no longer need a data disk that's attached to a virtual machine, you can easily detach it.</span></span> <span data-ttu-id="04acb-105">Odebere disk z virtuálního počítače, ale neodstraní z úložiště.</span><span class="sxs-lookup"><span data-stu-id="04acb-105">This removes the disk from the virtual machine, but doesn't remove it from storage.</span></span>

> [!WARNING]
> <span data-ttu-id="04acb-106">Pokud se odpojit disk není automaticky odstraněn.</span><span class="sxs-lookup"><span data-stu-id="04acb-106">If you detach a disk it is not automatically deleted.</span></span> <span data-ttu-id="04acb-107">Pokud jste přihlášení k odběru služby storage úrovně Premium, můžete nadále toho vám být účtovány poplatky za úložiště pro disk.</span><span class="sxs-lookup"><span data-stu-id="04acb-107">If you have subscribed to Premium storage, you will continue to incur storage charges for the disk.</span></span> <span data-ttu-id="04acb-108">Další informace najdete v části [ceny a fakturace při použití služby Premium Storage](../../storage/common/storage-premium-storage.md#pricing-and-billing).</span><span class="sxs-lookup"><span data-stu-id="04acb-108">For more information refer to [Pricing and Billing when using Premium Storage](../../storage/common/storage-premium-storage.md#pricing-and-billing).</span></span>
>
>

<span data-ttu-id="04acb-109">Pokud znovu chcete použít stávající data na disku, můžete ho znovu připojit ke stejnému nebo jinému virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="04acb-109">If you want to use the existing data on the disk again, you can reattach it to the same virtual machine, or another one.</span></span>

## <a name="detach-a-data-disk-using-the-portal"></a><span data-ttu-id="04acb-110">Odpojení datového disku pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="04acb-110">Detach a data disk using the portal</span></span>
1. <span data-ttu-id="04acb-111">V centru portálu vyberte **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="04acb-111">In the portal hub, select **Virtual Machines**.</span></span>
2. <span data-ttu-id="04acb-112">Vyberte virtuální počítač, který má datový disk, kterou chcete odpojit a klikněte na tlačítko **Zastavit** se zrušit přidělení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="04acb-112">Select the virtual machine that has the data disk you want to detach and click **Stop** to deallocate the VM.</span></span>
3. <span data-ttu-id="04acb-113">V okně virtuálního počítače vyberte **disky**.</span><span class="sxs-lookup"><span data-stu-id="04acb-113">In the virtual machine blade, select **Disks**.</span></span>
4. <span data-ttu-id="04acb-114">V horní části **disky** vyberte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="04acb-114">At the top of the **Disks** blade, select **Edit**.</span></span>
5. <span data-ttu-id="04acb-115">V **disky** okno na pravé straně datový disk, který chcete odpojit, klikněte na tlačítko ![obrázek tlačítka odpojení](./media/detach-disk/detach.png) odpojit tlačítko.</span><span class="sxs-lookup"><span data-stu-id="04acb-115">In the **Disks** blade, to the far right of the data disk that you would like to detach, click the ![Detach button image](./media/detach-disk/detach.png) detach button.</span></span>
5. <span data-ttu-id="04acb-116">Po odebrání disku nahoře v okně klikněte na tlačítko Uložit.</span><span class="sxs-lookup"><span data-stu-id="04acb-116">After the disk has been removed, click Save on the top of the blade.</span></span>
6. <span data-ttu-id="04acb-117">V okně virtuálního počítače klikněte na **přehled** a klikněte **spustit** tlačítka v horní části okna restartujte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="04acb-117">In the virtual machine blade, click **Overview** and then click the **Start** button at the top of the blade to restart the VM.</span></span>



<span data-ttu-id="04acb-118">Disk zůstává v úložišti, ale už není připojený k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="04acb-118">The disk remains in storage but is no longer attached to a virtual machine.</span></span>

## <a name="detach-a-data-disk-using-powershell"></a><span data-ttu-id="04acb-119">Odpojit datový disk pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="04acb-119">Detach a data disk using PowerShell</span></span>
<span data-ttu-id="04acb-120">V tomto příkladu v prvním příkazu je získán virtuální počítač s názvem **MyVM07** v **RG11** pomocí rutiny Get-AzureRmVM skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="04acb-120">In this example, the first command gets the virtual machine named **MyVM07** in the **RG11** resource group using the Get-AzureRmVM cmdlet.</span></span> <span data-ttu-id="04acb-121">Příkaz uloží virtuální počítač v **$VirtualMachine** proměnné.</span><span class="sxs-lookup"><span data-stu-id="04acb-121">The command stores the virtual machine in the **$VirtualMachine** variable.</span></span>

<span data-ttu-id="04acb-122">Druhý příkaz odebere datový disk s názvem DataDisk3 z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="04acb-122">The second command removes the data disk named DataDisk3 from the virtual machine.</span></span>

<span data-ttu-id="04acb-123">Poslední příkaz aktualizuje stav virtuálního počítače dokončete proces odebrání datový disk.</span><span class="sxs-lookup"><span data-stu-id="04acb-123">The final command updates the state of the virtual machine to complete the process of removing the data disk.</span></span>

```powershell
$VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07"
Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
Update-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" -VM $VirtualMachine
```

<span data-ttu-id="04acb-124">Další informace najdete v tématu [odebrat AzureRmVMDataDisk](/powershell/module/azurerm.compute/remove-azurermvmdatadisk).</span><span class="sxs-lookup"><span data-stu-id="04acb-124">For more information, see [Remove-AzureRmVMDataDisk](/powershell/module/azurerm.compute/remove-azurermvmdatadisk).</span></span>

## <a name="next-steps"></a><span data-ttu-id="04acb-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="04acb-125">Next steps</span></span>
<span data-ttu-id="04acb-126">Pokud chcete použít datový disk, jste právě [připojte ji k jiným virtuálním Počítačem](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="04acb-126">If you want to reuse the data disk, you can just [attach it to another VM](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>

