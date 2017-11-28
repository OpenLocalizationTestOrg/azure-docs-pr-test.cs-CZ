---
title: "aaaDetach datový disk od virtuálního počítače s Linuxem – Azure | Microsoft Docs"
description: "Přečtěte si toodetach datový disk z virtuálního počítače v Azure pomocí rozhraní příkazového řádku 2.0 nebo hello portálu Azure."
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
ms.openlocfilehash: 1c6145fc97f13179457225e93e0fb7adc261a65b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodetach-a-data-disk-from-a-linux-virtual-machine"></a><span data-ttu-id="7a3df-103">Jak toodetach datový disk z virtuálního počítače systému Linux</span><span class="sxs-lookup"><span data-stu-id="7a3df-103">How toodetach a data disk from a Linux virtual machine</span></span>

<span data-ttu-id="7a3df-104">Pokud již nepotřebujete datový disk, který je připojený tooa virtuální počítač, můžete ho snadno odpojit.</span><span class="sxs-lookup"><span data-stu-id="7a3df-104">When you no longer need a data disk that's attached tooa virtual machine, you can easily detach it.</span></span> <span data-ttu-id="7a3df-105">Odebere hello disk z hello virtuálního počítače, ale neodstraní z úložiště.</span><span class="sxs-lookup"><span data-stu-id="7a3df-105">This removes hello disk from hello virtual machine, but doesn't remove it from storage.</span></span> 

> [!WARNING]
> <span data-ttu-id="7a3df-106">Pokud se odpojit disk není automaticky odstraněn.</span><span class="sxs-lookup"><span data-stu-id="7a3df-106">If you detach a disk it is not automatically deleted.</span></span> <span data-ttu-id="7a3df-107">Pokud odebíráte tooPremium úložiště, budete moct dále tooincur poplatky za úložiště pro hello disk.</span><span class="sxs-lookup"><span data-stu-id="7a3df-107">If you have subscribed tooPremium storage, you will continue tooincur storage charges for hello disk.</span></span> <span data-ttu-id="7a3df-108">Další informace najdete v části příliš[ceny a fakturace při použití služby Premium Storage](../../storage/common/storage-premium-storage.md#pricing-and-billing).</span><span class="sxs-lookup"><span data-stu-id="7a3df-108">For more information refer too[Pricing and Billing when using Premium Storage](../../storage/common/storage-premium-storage.md#pricing-and-billing).</span></span> 
> 
> 

<span data-ttu-id="7a3df-109">Pokud chcete toouse hello existující data na disku hello znovu, můžete ji můžete opět připojit toohello stejného virtuálního počítače nebo jiný.</span><span class="sxs-lookup"><span data-stu-id="7a3df-109">If you want toouse hello existing data on hello disk again, you can reattach it toohello same virtual machine, or another one.</span></span>  

## <a name="detach-a-data-disk-using-cli-20"></a><span data-ttu-id="7a3df-110">Odpojit datový disk pomocí rozhraní příkazového řádku 2.0</span><span class="sxs-lookup"><span data-stu-id="7a3df-110">Detach a data disk using CLI 2.0</span></span>

```azurecli
az vm disk detach -g myResourceGroup --vm-name myVm -n myDataDisk
```

<span data-ttu-id="7a3df-111">Hello disk zůstává v úložišti, ale je už připojené tooa virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="7a3df-111">hello disk remains in storage but is no longer attached tooa virtual machine.</span></span>


## <a name="detach-a-data-disk-using-hello-portal"></a><span data-ttu-id="7a3df-112">Odpojit datový disk pomocí portálu hello</span><span class="sxs-lookup"><span data-stu-id="7a3df-112">Detach a data disk using hello portal</span></span>
1. <span data-ttu-id="7a3df-113">V portálu centra hello, vyberte **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="7a3df-113">In hello portal hub, select **Virtual Machines**.</span></span>
2. <span data-ttu-id="7a3df-114">Vyberte hello virtuální počítač, který má datový disk hello toodetach a klikněte na **Zastavit** toodeallocate hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="7a3df-114">Select hello virtual machine that has hello data disk you want toodetach and click **Stop** toodeallocate hello VM.</span></span>
3. <span data-ttu-id="7a3df-115">V okně hello virtuálního počítače, vyberte **disky**.</span><span class="sxs-lookup"><span data-stu-id="7a3df-115">In hello virtual machine blade, select **Disks**.</span></span>
4. <span data-ttu-id="7a3df-116">Hello horní části hello **disky** vyberte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="7a3df-116">At hello top of hello **Disks** blade, select **Edit**.</span></span>
5. <span data-ttu-id="7a3df-117">V hello **disky** okně toohello daleko vpravo od hello datový disk, které chcete toodetach, klikněte na tlačítko hello ![obrázek tlačítka odpojení](./media/detach-disk/detach.png) odpojit tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7a3df-117">In hello **Disks** blade, toohello far right of hello data disk that you would like toodetach, click hello ![Detach button image](./media/detach-disk/detach.png) detach button.</span></span>
5. <span data-ttu-id="7a3df-118">Po hello disk odebrat, klikněte na Uložit na hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="7a3df-118">After hello disk has been removed, click Save on hello top of hello blade.</span></span>
6. <span data-ttu-id="7a3df-119">V okně hello virtuální počítač, klikněte na tlačítko **přehled** a pak klikněte na tlačítko hello **spustit** tlačítko hello horní části hello okno toorestart hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="7a3df-119">In hello virtual machine blade, click **Overview** and then click hello **Start** button at hello top of hello blade toorestart hello VM.</span></span>

<span data-ttu-id="7a3df-120">Hello disk zůstává v úložišti, ale je už připojené tooa virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="7a3df-120">hello disk remains in storage but is no longer attached tooa virtual machine.</span></span>








## <a name="next-steps"></a><span data-ttu-id="7a3df-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7a3df-121">Next steps</span></span>
<span data-ttu-id="7a3df-122">Pokud chcete tooreuse hello datový disk, jste právě [připojte ji tooanother virtuálních počítačů](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7a3df-122">If you want tooreuse hello data disk, you can just [attach it tooanother VM](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

