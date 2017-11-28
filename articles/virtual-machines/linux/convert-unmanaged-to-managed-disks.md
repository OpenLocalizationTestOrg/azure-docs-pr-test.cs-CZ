---
title: "virtuální počítač s Linuxem v Azure z nespravovaných aaaConvert disky toomanaged disků - disků spravované Azure | Microsoft Docs"
description: "Jak tooconvert virtuálního počítače s Linuxem z nespravovaných disky toomanaged disky pomocí Azure CLI 2.0 v modelu nasazení Resource Manager hello"
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
ms.openlocfilehash: 1b94da11deab46f344e28ab4491cf220506b6347
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="convert-a-linux-virtual-machine-from-unmanaged-disks-toomanaged-disks"></a><span data-ttu-id="83104-103">Převést virtuální počítač s Linuxem z nespravovaných disky toomanaged disků</span><span class="sxs-lookup"><span data-stu-id="83104-103">Convert a Linux virtual machine from unmanaged disks toomanaged disks</span></span>

<span data-ttu-id="83104-104">Pokud máte existující virtuální počítače s Linuxem (VM) používající nespravované disky, můžete převést disky toouse spravované hello virtuálních počítačů prostřednictvím hello [Azure spravované disky](../windows/managed-disks-overview.md) služby.</span><span class="sxs-lookup"><span data-stu-id="83104-104">If you have existing Linux virtual machines (VMs) that use unmanaged disks, you can convert hello VMs toouse managed disks through hello [Azure Managed Disks](../windows/managed-disks-overview.md) service.</span></span> <span data-ttu-id="83104-105">Tento proces převede disk hello operačního systému a všechny připojené datových disků.</span><span class="sxs-lookup"><span data-stu-id="83104-105">This process converts both hello OS disk and any attached data disks.</span></span>

<span data-ttu-id="83104-106">Tento článek ukazuje, jak hello tooconvert virtuálních počítačů pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="83104-106">This article shows you how tooconvert VMs by using hello Azure CLI.</span></span> <span data-ttu-id="83104-107">Pokud potřebujete tooinstall nebo ho upgradovat, přečtěte si téma [nainstalovat Azure CLI 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="83104-107">If you need tooinstall or upgrade it, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="83104-108">Než začnete</span><span class="sxs-lookup"><span data-stu-id="83104-108">Before you begin</span></span>

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]


## <a name="convert-single-instance-vms"></a><span data-ttu-id="83104-109">Převést virtuální počítače jednou instancí</span><span class="sxs-lookup"><span data-stu-id="83104-109">Convert single-instance VMs</span></span>
<span data-ttu-id="83104-110">Tato část popisuje, jak tooconvert jedné instance virtuální počítače Azure z nespravovaných disky toomanaged disky.</span><span class="sxs-lookup"><span data-stu-id="83104-110">This section covers how tooconvert single-instance Azure VMs from unmanaged disks toomanaged disks.</span></span> <span data-ttu-id="83104-111">(Pokud jsou vaše virtuální počítače v nastavení dostupnosti, viz další část hello.) Můžete použít tento proces tooconvert hello virtuální počítače z nespravované premium (SSD) disky toopremium spravované disky nebo standard (HDD) nespravované disky toostandard spravované disků.</span><span class="sxs-lookup"><span data-stu-id="83104-111">(If your VMs are in an availability set, see hello next section.) You can use this process tooconvert hello VMs from premium (SSD) unmanaged disks toopremium managed disks, or from standard (HDD) unmanaged disks toostandard managed disks.</span></span>

1. <span data-ttu-id="83104-112">Deallocate hello virtuálních počítačů pomocí [az OM deallocate](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="83104-112">Deallocate hello VM by using [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="83104-113">Hello následující příklad zruší přidělení hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="83104-113">hello following example deallocates hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

2. <span data-ttu-id="83104-114">Převést disky toomanaged hello virtuálních počítačů pomocí [převést virtuální počítač az](/cli/azure/vm#convert).</span><span class="sxs-lookup"><span data-stu-id="83104-114">Convert hello VM toomanaged disks by using [az vm convert](/cli/azure/vm#convert).</span></span> <span data-ttu-id="83104-115">Hello následující proces převede hello virtuálního počítače s názvem `myVM`, včetně disku hello operačního systému a všechny datové disky:</span><span class="sxs-lookup"><span data-stu-id="83104-115">hello following process converts hello VM named `myVM`, including hello OS disk and any data disks:</span></span>

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

3. <span data-ttu-id="83104-116">Spustit hello virtuálních počítačů po hello převod toomanaged disky pomocí [spuštění virtuálního počítače az](/cli/azure/vm#start).</span><span class="sxs-lookup"><span data-stu-id="83104-116">Start hello VM after hello conversion toomanaged disks by using [az vm start](/cli/azure/vm#start).</span></span> <span data-ttu-id="83104-117">Následující příklad spustí Hello hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="83104-117">hello following example starts hello VM named `myVM` in hello resource group named `myResourceGroup`.</span></span>

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="convert-vms-in-an-availability-set"></a><span data-ttu-id="83104-118">Převést virtuální počítače v nastavení dostupnosti</span><span class="sxs-lookup"><span data-stu-id="83104-118">Convert VMs in an availability set</span></span>

<span data-ttu-id="83104-119">Pokud hello virtuálních počítačů, které chcete tooconvert toomanaged disky jsou v nastavení dostupnosti, je nutné nejprve tooconvert hello dostupnost sady tooa spravovat sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="83104-119">If hello VMs that you want tooconvert toomanaged disks are in an availability set, you first need tooconvert hello availability set tooa managed availability set.</span></span>

<span data-ttu-id="83104-120">Všechny virtuální počítače ve skupině dostupnosti hello musí být navrácena před převodem hello sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="83104-120">All VMs in hello availability set must be deallocated before you convert hello availability set.</span></span> <span data-ttu-id="83104-121">Plán tooconvert všechny disky toomanaged virtuálních počítačů po hello dostupnosti nastavit sám sebe byl převeden tooa spravovat sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="83104-121">Plan tooconvert all VMs toomanaged disks after hello availability set itself has been converted tooa managed availability set.</span></span> <span data-ttu-id="83104-122">Pak spusťte všechny virtuální počítače hello a pokračovat v činnosti jako normální.</span><span class="sxs-lookup"><span data-stu-id="83104-122">Then, start all hello VMs and continue operating as normal.</span></span>

1. <span data-ttu-id="83104-123">Zobrazí seznam všech virtuálních počítačů ve skupině dostupnosti, nastavit pomocí [seznamu skupinu dostupnosti virtuálních počítačů az](/cli/azure/vm/availability-set#list).</span><span class="sxs-lookup"><span data-stu-id="83104-123">List all VMs in an availability set by using [az vm availability-set list](/cli/azure/vm/availability-set#list).</span></span> <span data-ttu-id="83104-124">Hello následující příklad vypíše všechny virtuální počítače ve skupině s názvem dostupnosti hello `myAvailabilitySet` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="83104-124">hello following example lists all VMs in hello availability set named `myAvailabilitySet` in hello resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm availability-set show \
        --resource-group myResourceGroup \
        --name myAvailabilitySet \
        --query [virtualMachines[*].id] \
        --output table
    ```

2. <span data-ttu-id="83104-125">Deallocate všechny virtuální počítače hello pomocí [az OM deallocate](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="83104-125">Deallocate all hello VMs by using [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="83104-126">Hello následující příklad zruší přidělení hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="83104-126">hello following example deallocates hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

3. <span data-ttu-id="83104-127">Převést hello dostupnosti pomocí [převést skupinu dostupnosti virtuálních počítačů az](/cli/azure/vm/availability-set#convert).</span><span class="sxs-lookup"><span data-stu-id="83104-127">Convert hello availability set by using [az vm availability-set convert](/cli/azure/vm/availability-set#convert).</span></span> <span data-ttu-id="83104-128">Hello následující příklad převede sadu s názvem dostupnosti hello `myAvailabilitySet` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="83104-128">hello following example converts hello availability set named `myAvailabilitySet` in hello resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm availability-set convert \
        --resource-group myResourceGroup \
        --name myAvailabilitySet
    ```

4. <span data-ttu-id="83104-129">Převeďte všechny disky toomanaged hello virtuálních počítačů pomocí [převést virtuální počítač az](/cli/azure/vm#convert).</span><span class="sxs-lookup"><span data-stu-id="83104-129">Convert all hello VMs toomanaged disks by using [az vm convert](/cli/azure/vm#convert).</span></span> <span data-ttu-id="83104-130">Hello následující proces převede hello virtuálního počítače s názvem `myVM`, včetně disku hello operačního systému a všechny datové disky:</span><span class="sxs-lookup"><span data-stu-id="83104-130">hello following process converts hello VM named `myVM`, including hello OS disk and any data disks:</span></span>

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

5. <span data-ttu-id="83104-131">Spuštění všech virtuálních počítačů hello po hello převod toomanaged disky pomocí [spuštění virtuálního počítače az](/cli/azure/vm#start).</span><span class="sxs-lookup"><span data-stu-id="83104-131">Start all hello VMs after hello conversion toomanaged disks by using [az vm start](/cli/azure/vm#start).</span></span> <span data-ttu-id="83104-132">Následující příklad spustí Hello hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="83104-132">hello following example starts hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="next-steps"></a><span data-ttu-id="83104-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="83104-133">Next steps</span></span>
<span data-ttu-id="83104-134">Další informace o možnostech úložiště najdete v tématu [přehled Azure spravované disky](../windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="83104-134">For more information about storage options, see [Azure Managed Disks overview](../windows/managed-disks-overview.md).</span></span>
