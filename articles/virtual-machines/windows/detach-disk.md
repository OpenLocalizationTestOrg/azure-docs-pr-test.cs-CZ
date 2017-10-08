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
# <a name="how-toodetach-a-data-disk-from-a-windows-virtual-machine"></a><span data-ttu-id="2bd53-103">Jak toodetach datový disk z virtuálního počítače s Windows</span><span class="sxs-lookup"><span data-stu-id="2bd53-103">How toodetach a data disk from a Windows virtual machine</span></span>
<span data-ttu-id="2bd53-104">Pokud již nepotřebujete datový disk, který je připojený tooa virtuální počítač, můžete ho snadno odpojit.</span><span class="sxs-lookup"><span data-stu-id="2bd53-104">When you no longer need a data disk that's attached tooa virtual machine, you can easily detach it.</span></span> <span data-ttu-id="2bd53-105">Odebere hello disk z hello virtuálního počítače, ale neodstraní z úložiště.</span><span class="sxs-lookup"><span data-stu-id="2bd53-105">This removes hello disk from hello virtual machine, but doesn't remove it from storage.</span></span>

> [!WARNING]
> <span data-ttu-id="2bd53-106">Pokud se odpojit disk není automaticky odstraněn.</span><span class="sxs-lookup"><span data-stu-id="2bd53-106">If you detach a disk it is not automatically deleted.</span></span> <span data-ttu-id="2bd53-107">Pokud odebíráte tooPremium úložiště, budete moct dále tooincur poplatky za úložiště pro hello disk.</span><span class="sxs-lookup"><span data-stu-id="2bd53-107">If you have subscribed tooPremium storage, you will continue tooincur storage charges for hello disk.</span></span> <span data-ttu-id="2bd53-108">Další informace najdete v části příliš[ceny a fakturace při použití služby Premium Storage](../../storage/common/storage-premium-storage.md#pricing-and-billing).</span><span class="sxs-lookup"><span data-stu-id="2bd53-108">For more information refer too[Pricing and Billing when using Premium Storage](../../storage/common/storage-premium-storage.md#pricing-and-billing).</span></span>
>
>

<span data-ttu-id="2bd53-109">Pokud chcete toouse hello existující data na disku hello znovu, můžete ji můžete opět připojit toohello stejného virtuálního počítače nebo jiný.</span><span class="sxs-lookup"><span data-stu-id="2bd53-109">If you want toouse hello existing data on hello disk again, you can reattach it toohello same virtual machine, or another one.</span></span>

## <a name="detach-a-data-disk-using-hello-portal"></a><span data-ttu-id="2bd53-110">Odpojit datový disk pomocí portálu hello</span><span class="sxs-lookup"><span data-stu-id="2bd53-110">Detach a data disk using hello portal</span></span>
1. <span data-ttu-id="2bd53-111">V portálu centra hello, vyberte **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="2bd53-111">In hello portal hub, select **Virtual Machines**.</span></span>
2. <span data-ttu-id="2bd53-112">Vyberte hello virtuální počítač, který má datový disk hello toodetach a klikněte na **Zastavit** toodeallocate hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2bd53-112">Select hello virtual machine that has hello data disk you want toodetach and click **Stop** toodeallocate hello VM.</span></span>
3. <span data-ttu-id="2bd53-113">V okně hello virtuálního počítače, vyberte **disky**.</span><span class="sxs-lookup"><span data-stu-id="2bd53-113">In hello virtual machine blade, select **Disks**.</span></span>
4. <span data-ttu-id="2bd53-114">Hello horní části hello **disky** vyberte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="2bd53-114">At hello top of hello **Disks** blade, select **Edit**.</span></span>
5. <span data-ttu-id="2bd53-115">V hello **disky** okně toohello daleko vpravo od hello datový disk, které chcete toodetach, klikněte na tlačítko hello ![obrázek tlačítka odpojení](./media/detach-disk/detach.png) odpojit tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2bd53-115">In hello **Disks** blade, toohello far right of hello data disk that you would like toodetach, click hello ![Detach button image](./media/detach-disk/detach.png) detach button.</span></span>
5. <span data-ttu-id="2bd53-116">Po hello disk odebrat, klikněte na Uložit na hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="2bd53-116">After hello disk has been removed, click Save on hello top of hello blade.</span></span>
6. <span data-ttu-id="2bd53-117">V okně hello virtuální počítač, klikněte na tlačítko **přehled** a pak klikněte na tlačítko hello **spustit** tlačítko hello horní části hello okno toorestart hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2bd53-117">In hello virtual machine blade, click **Overview** and then click hello **Start** button at hello top of hello blade toorestart hello VM.</span></span>



<span data-ttu-id="2bd53-118">Hello disk zůstává v úložišti, ale je už připojené tooa virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="2bd53-118">hello disk remains in storage but is no longer attached tooa virtual machine.</span></span>

## <a name="detach-a-data-disk-using-powershell"></a><span data-ttu-id="2bd53-119">Odpojit datový disk pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2bd53-119">Detach a data disk using PowerShell</span></span>
<span data-ttu-id="2bd53-120">V tomto příkladu hello první příkaz získá hello virtuální počítač s názvem **MyVM07** v hello **RG11** skupiny prostředků pomocí rutiny Get-AzureRmVM hello.</span><span class="sxs-lookup"><span data-stu-id="2bd53-120">In this example, hello first command gets hello virtual machine named **MyVM07** in hello **RG11** resource group using hello Get-AzureRmVM cmdlet.</span></span> <span data-ttu-id="2bd53-121">příkaz hello úložiště virtuálního počítače v hello Hello **$VirtualMachine** proměnné.</span><span class="sxs-lookup"><span data-stu-id="2bd53-121">hello command stores hello virtual machine in hello **$VirtualMachine** variable.</span></span>

<span data-ttu-id="2bd53-122">druhý příkaz Hello odebere hello datový disk s názvem DataDisk3 z hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2bd53-122">hello second command removes hello data disk named DataDisk3 from hello virtual machine.</span></span>

<span data-ttu-id="2bd53-123">poslední příkaz Hello aktualizuje stav hello hello virtuálního počítače toocomplete hello proces odebrání hello datový disk.</span><span class="sxs-lookup"><span data-stu-id="2bd53-123">hello final command updates hello state of hello virtual machine toocomplete hello process of removing hello data disk.</span></span>

```powershell
$VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07"
Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
Update-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" -VM $VirtualMachine
```

<span data-ttu-id="2bd53-124">Další informace najdete v tématu [odebrat AzureRmVMDataDisk](/powershell/module/azurerm.compute/remove-azurermvmdatadisk).</span><span class="sxs-lookup"><span data-stu-id="2bd53-124">For more information, see [Remove-AzureRmVMDataDisk](/powershell/module/azurerm.compute/remove-azurermvmdatadisk).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2bd53-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2bd53-125">Next steps</span></span>
<span data-ttu-id="2bd53-126">Pokud chcete tooreuse hello datový disk, jste právě [připojte ji tooanother virtuálních počítačů](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="2bd53-126">If you want tooreuse hello data disk, you can just [attach it tooanother VM](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>

