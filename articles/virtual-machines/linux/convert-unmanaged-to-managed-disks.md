---
title: "Převeďte virtuální počítač s Linuxem v Azure z nespravovaných disků na spravované disky - disky spravované Azure | Microsoft Docs"
description: "Jak převést virtuální počítač s Linuxem z disků nespravované na spravované disky pomocí Azure CLI 2.0 v modelu nasazení Resource Manager"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 06/23/2017
ms.author: iainfou
ms.openlocfilehash: 94f8e3330fb2d6547811315fcfdb8ced338e0247
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="convert-a-linux-virtual-machine-from-unmanaged-disks-to-managed-disks"></a><span data-ttu-id="eae27-103">Převeďte virtuální počítač s Linuxem z disků nespravované na spravované disky</span><span class="sxs-lookup"><span data-stu-id="eae27-103">Convert a Linux virtual machine from unmanaged disks to managed disks</span></span>

<span data-ttu-id="eae27-104">Pokud máte existující virtuální počítače s Linuxem (VM) používající nespravované disky, můžete převést virtuální počítače použít spravované disky prostřednictvím [Azure spravované disky](../windows/managed-disks-overview.md) služby.</span><span class="sxs-lookup"><span data-stu-id="eae27-104">If you have existing Linux virtual machines (VMs) that use unmanaged disks, you can convert the VMs to use managed disks through the [Azure Managed Disks](../windows/managed-disks-overview.md) service.</span></span> <span data-ttu-id="eae27-105">Tento proces převede disk operačního systému a všechny připojené datových disků.</span><span class="sxs-lookup"><span data-stu-id="eae27-105">This process converts both the OS disk and any attached data disks.</span></span>

<span data-ttu-id="eae27-106">Tento článek ukazuje, jak převést virtuální počítače pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="eae27-106">This article shows you how to convert VMs by using the Azure CLI.</span></span> <span data-ttu-id="eae27-107">Pokud je potřeba nainstalovat nebo upgradovat najdete v tématu [nainstalovat Azure CLI 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="eae27-107">If you need to install or upgrade it, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="eae27-108">Než začnete</span><span class="sxs-lookup"><span data-stu-id="eae27-108">Before you begin</span></span>

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]


## <a name="convert-single-instance-vms"></a><span data-ttu-id="eae27-109">Převést virtuální počítače jednou instancí</span><span class="sxs-lookup"><span data-stu-id="eae27-109">Convert single-instance VMs</span></span>
<span data-ttu-id="eae27-110">Tato část popisuje jak převést virtuální počítače Azure jednou instancí z nespravovaných disků na spravované disky.</span><span class="sxs-lookup"><span data-stu-id="eae27-110">This section covers how to convert single-instance Azure VMs from unmanaged disks to managed disks.</span></span> <span data-ttu-id="eae27-111">(Pokud jsou vaše virtuální počítače v nastavení dostupnosti, najdete v další části.) Tento proces můžete převést virtuální počítače z disků disky na disky premium spravované nebo z standard (HDD) nespravované premium (SSD) nespravované na spravované standardní disky.</span><span class="sxs-lookup"><span data-stu-id="eae27-111">(If your VMs are in an availability set, see the next section.) You can use this process to convert the VMs from premium (SSD) unmanaged disks to premium managed disks, or from standard (HDD) unmanaged disks to standard managed disks.</span></span>

1. <span data-ttu-id="eae27-112">Zrušit přidělení virtuálního počítače pomocí [az OM deallocate](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="eae27-112">Deallocate the VM by using [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="eae27-113">Následující příklad zruší přidělení virtuálního počítače s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="eae27-113">The following example deallocates the VM named `myVM` in the resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

2. <span data-ttu-id="eae27-114">Převeďte virtuální počítač na spravované disky pomocí [převést virtuální počítač az](/cli/azure/vm#convert).</span><span class="sxs-lookup"><span data-stu-id="eae27-114">Convert the VM to managed disks by using [az vm convert](/cli/azure/vm#convert).</span></span> <span data-ttu-id="eae27-115">Následující proces převede virtuální počítač s názvem `myVM`, včetně disku operačního systému a všechny datové disky:</span><span class="sxs-lookup"><span data-stu-id="eae27-115">The following process converts the VM named `myVM`, including the OS disk and any data disks:</span></span>

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

3. <span data-ttu-id="eae27-116">Spusťte virtuální počítač po převodu na spravované disky pomocí [spuštění virtuálního počítače az](/cli/azure/vm#start).</span><span class="sxs-lookup"><span data-stu-id="eae27-116">Start the VM after the conversion to managed disks by using [az vm start](/cli/azure/vm#start).</span></span> <span data-ttu-id="eae27-117">Následující příklad spustí virtuální počítač s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="eae27-117">The following example starts the VM named `myVM` in the resource group named `myResourceGroup`.</span></span>

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="convert-vms-in-an-availability-set"></a><span data-ttu-id="eae27-118">Převést virtuální počítače v nastavení dostupnosti</span><span class="sxs-lookup"><span data-stu-id="eae27-118">Convert VMs in an availability set</span></span>

<span data-ttu-id="eae27-119">Pokud virtuální počítače, které chcete převést na spravované disky jsou v nastavení dostupnosti, je nutné nejprve převést skupinu dostupnosti do skupiny spravované dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="eae27-119">If the VMs that you want to convert to managed disks are in an availability set, you first need to convert the availability set to a managed availability set.</span></span>

<span data-ttu-id="eae27-120">Všechny virtuální počítače v sadě dostupnosti musí být navrácena předtím, než převedete sadu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="eae27-120">All VMs in the availability set must be deallocated before you convert the availability set.</span></span> <span data-ttu-id="eae27-121">Naplánujte převeďte všechny virtuální počítače na spravovaného disky po samotné sadu dostupnosti byl převeden na sadu spravovaných dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="eae27-121">Plan to convert all VMs to managed disks after the availability set itself has been converted to a managed availability set.</span></span> <span data-ttu-id="eae27-122">Potom spusťte všechny virtuální počítače a pokračovat normální.</span><span class="sxs-lookup"><span data-stu-id="eae27-122">Then, start all the VMs and continue operating as normal.</span></span>

1. <span data-ttu-id="eae27-123">Zobrazí seznam všech virtuálních počítačů ve skupině dostupnosti, nastavit pomocí [seznamu skupinu dostupnosti virtuálních počítačů az](/cli/azure/vm/availability-set#list).</span><span class="sxs-lookup"><span data-stu-id="eae27-123">List all VMs in an availability set by using [az vm availability-set list](/cli/azure/vm/availability-set#list).</span></span> <span data-ttu-id="eae27-124">Následující příklad vypíše všechny virtuální počítače ve skupině s názvem dostupnosti `myAvailabilitySet` ve skupině prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="eae27-124">The following example lists all VMs in the availability set named `myAvailabilitySet` in the resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm availability-set show \
        --resource-group myResourceGroup \
        --name myAvailabilitySet \
        --query [virtualMachines[*].id] \
        --output table
    ```

2. <span data-ttu-id="eae27-125">Deallocate všechny virtuální počítače pomocí [az OM deallocate](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="eae27-125">Deallocate all the VMs by using [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="eae27-126">Následující příklad zruší přidělení virtuálního počítače s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="eae27-126">The following example deallocates the VM named `myVM` in the resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

3. <span data-ttu-id="eae27-127">Převést skupinu dostupnosti pomocí [převést skupinu dostupnosti virtuálních počítačů az](/cli/azure/vm/availability-set#convert).</span><span class="sxs-lookup"><span data-stu-id="eae27-127">Convert the availability set by using [az vm availability-set convert](/cli/azure/vm/availability-set#convert).</span></span> <span data-ttu-id="eae27-128">Následující příklad převede skupinu dostupnosti s názvem `myAvailabilitySet` ve skupině prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="eae27-128">The following example converts the availability set named `myAvailabilitySet` in the resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm availability-set convert \
        --resource-group myResourceGroup \
        --name myAvailabilitySet
    ```

4. <span data-ttu-id="eae27-129">Převeďte všechny virtuální počítače na spravovaného disky pomocí [převést virtuální počítač az](/cli/azure/vm#convert).</span><span class="sxs-lookup"><span data-stu-id="eae27-129">Convert all the VMs to managed disks by using [az vm convert](/cli/azure/vm#convert).</span></span> <span data-ttu-id="eae27-130">Následující proces převede virtuální počítač s názvem `myVM`, včetně disku operačního systému a všechny datové disky:</span><span class="sxs-lookup"><span data-stu-id="eae27-130">The following process converts the VM named `myVM`, including the OS disk and any data disks:</span></span>

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

5. <span data-ttu-id="eae27-131">Spusťte všechny virtuální počítače po převodu na spravované disky pomocí [spuštění virtuálního počítače az](/cli/azure/vm#start).</span><span class="sxs-lookup"><span data-stu-id="eae27-131">Start all the VMs after the conversion to managed disks by using [az vm start](/cli/azure/vm#start).</span></span> <span data-ttu-id="eae27-132">Následující příklad spustí virtuální počítač s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="eae27-132">The following example starts the VM named `myVM` in the resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="next-steps"></a><span data-ttu-id="eae27-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eae27-133">Next steps</span></span>
<span data-ttu-id="eae27-134">Další informace o možnostech úložiště najdete v tématu [přehled Azure spravované disky](../windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="eae27-134">For more information about storage options, see [Azure Managed Disks overview](../windows/managed-disks-overview.md).</span></span>
