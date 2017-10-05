---
title: "Odpojit datový disk z virtuálního počítače s Linuxem – Azure | Microsoft Docs"
description: "Naučte se odpojit datový disk z virtuálního počítače v Azure pomocí rozhraní příkazového řádku 2.0 nebo portálu Azure."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.topic: article
ms.date: 03/21/2017
ms.author: cynthn
ms.openlocfilehash: 3f29547e1da6028b1e4b91d9e29fd3bcdfe08d50
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-detach-a-data-disk-from-a-linux-virtual-machine"></a><span data-ttu-id="a8bba-103">Postup odpojit datový disk z virtuálního počítače systému Linux</span><span class="sxs-lookup"><span data-stu-id="a8bba-103">How to detach a data disk from a Linux virtual machine</span></span>

<span data-ttu-id="a8bba-104">Když už nepotřebujete datový disk připojený k virtuálnímu počítači, můžete ho jednoduše odpojit.</span><span class="sxs-lookup"><span data-stu-id="a8bba-104">When you no longer need a data disk that's attached to a virtual machine, you can easily detach it.</span></span> <span data-ttu-id="a8bba-105">Odebere disk z virtuálního počítače, ale neodstraní z úložiště.</span><span class="sxs-lookup"><span data-stu-id="a8bba-105">This removes the disk from the virtual machine, but doesn't remove it from storage.</span></span> 

> [!WARNING]
> <span data-ttu-id="a8bba-106">Pokud se odpojit disk není automaticky odstraněn.</span><span class="sxs-lookup"><span data-stu-id="a8bba-106">If you detach a disk it is not automatically deleted.</span></span> <span data-ttu-id="a8bba-107">Pokud jste přihlášení k odběru služby storage úrovně Premium, můžete nadále toho vám být účtovány poplatky za úložiště pro disk.</span><span class="sxs-lookup"><span data-stu-id="a8bba-107">If you have subscribed to Premium storage, you will continue to incur storage charges for the disk.</span></span> <span data-ttu-id="a8bba-108">Další informace najdete v části [ceny a fakturace při použití služby Premium Storage](../../storage/common/storage-premium-storage.md#pricing-and-billing).</span><span class="sxs-lookup"><span data-stu-id="a8bba-108">For more information refer to [Pricing and Billing when using Premium Storage](../../storage/common/storage-premium-storage.md#pricing-and-billing).</span></span> 
> 
> 

<span data-ttu-id="a8bba-109">Pokud znovu chcete použít stávající data na disku, můžete ho znovu připojit ke stejnému nebo jinému virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="a8bba-109">If you want to use the existing data on the disk again, you can reattach it to the same virtual machine, or another one.</span></span>  

## <a name="detach-a-data-disk-using-cli-20"></a><span data-ttu-id="a8bba-110">Odpojit datový disk pomocí rozhraní příkazového řádku 2.0</span><span class="sxs-lookup"><span data-stu-id="a8bba-110">Detach a data disk using CLI 2.0</span></span>

```azurecli
az vm disk detach -g myResourceGroup --vm-name myVm -n myDataDisk
```

<span data-ttu-id="a8bba-111">Disk zůstává v úložišti, ale už není připojený k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="a8bba-111">The disk remains in storage but is no longer attached to a virtual machine.</span></span>


## <a name="detach-a-data-disk-using-the-portal"></a><span data-ttu-id="a8bba-112">Odpojení datového disku pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="a8bba-112">Detach a data disk using the portal</span></span>
1. <span data-ttu-id="a8bba-113">V centru portálu vyberte **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="a8bba-113">In the portal hub, select **Virtual Machines**.</span></span>
2. <span data-ttu-id="a8bba-114">Vyberte virtuální počítač, který má datový disk, kterou chcete odpojit a klikněte na tlačítko **Zastavit** se zrušit přidělení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a8bba-114">Select the virtual machine that has the data disk you want to detach and click **Stop** to deallocate the VM.</span></span>
3. <span data-ttu-id="a8bba-115">V okně virtuálního počítače vyberte **disky**.</span><span class="sxs-lookup"><span data-stu-id="a8bba-115">In the virtual machine blade, select **Disks**.</span></span>
4. <span data-ttu-id="a8bba-116">V horní části **disky** vyberte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="a8bba-116">At the top of the **Disks** blade, select **Edit**.</span></span>
5. <span data-ttu-id="a8bba-117">V **disky** okno na pravé straně datový disk, který chcete odpojit, klikněte na tlačítko ![obrázek tlačítka odpojení](./media/detach-disk/detach.png) odpojit tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a8bba-117">In the **Disks** blade, to the far right of the data disk that you would like to detach, click the ![Detach button image](./media/detach-disk/detach.png) detach button.</span></span>
5. <span data-ttu-id="a8bba-118">Po odebrání disku nahoře v okně klikněte na tlačítko Uložit.</span><span class="sxs-lookup"><span data-stu-id="a8bba-118">After the disk has been removed, click Save on the top of the blade.</span></span>
6. <span data-ttu-id="a8bba-119">V okně virtuálního počítače klikněte na **přehled** a klikněte **spustit** tlačítka v horní části okna restartujte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a8bba-119">In the virtual machine blade, click **Overview** and then click the **Start** button at the top of the blade to restart the VM.</span></span>

<span data-ttu-id="a8bba-120">Disk zůstává v úložišti, ale už není připojený k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="a8bba-120">The disk remains in storage but is no longer attached to a virtual machine.</span></span>








## <a name="next-steps"></a><span data-ttu-id="a8bba-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a8bba-121">Next steps</span></span>
<span data-ttu-id="a8bba-122">Pokud chcete použít datový disk, jste právě [připojte ji k jiným virtuálním Počítačem](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a8bba-122">If you want to reuse the data disk, you can just [attach it to another VM](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
