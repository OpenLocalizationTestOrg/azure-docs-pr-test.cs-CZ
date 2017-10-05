---
title: "Rozbalte virtuální pevné disky na virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Zjistěte, jak rozšířit virtuální pevné disky na virtuální počítač s Linuxem pomocí Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: b82cc0473c003da767ee230ab485c69b233977d1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-expand-virtual-hard-disks-on-a-linux-vm-with-the-azure-cli"></a><span data-ttu-id="ebb02-103">Způsob, jak rozbalit virtuální pevné disky na virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="ebb02-103">How to expand virtual hard disks on a Linux VM with the Azure CLI</span></span>
<span data-ttu-id="ebb02-104">Výchozí velikost virtuálního pevného disku pro operační systém (OS) je obvykle 30 GB na virtuální počítač s Linuxem (VM) v Azure.</span><span class="sxs-lookup"><span data-stu-id="ebb02-104">The default virtual hard disk size for the operating system (OS) is typically 30 GB on a Linux virtual machine (VM) in Azure.</span></span> <span data-ttu-id="ebb02-105">Můžete [přidat datových disků](add-disk.md) zajistit pro dalšího volného místa, ale můžete také chtít rozšířit stávající datový disk.</span><span class="sxs-lookup"><span data-stu-id="ebb02-105">You can [add data disks](add-disk.md) to provide for additional storage space, but you may also wish to expand an existing data disk.</span></span> <span data-ttu-id="ebb02-106">Tento článek podrobně popisují rozbalte spravované disky pro virtuální počítač s Linuxem pomocí Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="ebb02-106">This article details how to expand managed disks for a Linux VM with the Azure CLI 2.0.</span></span> <span data-ttu-id="ebb02-107">Můžete také rozšířit nespravované disk operačního systému pomocí [Azure CLI 1.0](expand-disks-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="ebb02-107">You can also expand the unmanaged OS disk with the [Azure CLI 1.0](expand-disks-nodejs.md).</span></span>

> [!WARNING]
> <span data-ttu-id="ebb02-108">Ujistěte se, můžete zálohovat data před provedením disku změnit velikost operace vždy.</span><span class="sxs-lookup"><span data-stu-id="ebb02-108">Always make sure that you back up your data before you perform disk resize operations.</span></span> <span data-ttu-id="ebb02-109">Další informace najdete v tématu [zálohovat virtuální počítače s Linuxem v Azure](tutorial-backup-vms.md).</span><span class="sxs-lookup"><span data-stu-id="ebb02-109">For more information, see [Back up Linux VMs in Azure](tutorial-backup-vms.md).</span></span>

## <a name="expand-disk"></a><span data-ttu-id="ebb02-110">Rozbalte disk</span><span class="sxs-lookup"><span data-stu-id="ebb02-110">Expand disk</span></span>
<span data-ttu-id="ebb02-111">Ujistěte se, že máte nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="ebb02-111">Make sure that you have the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="ebb02-112">Tento článek vyžaduje existující virtuální počítač v Azure s alespoň jeden datový disk připojený a připravený.</span><span class="sxs-lookup"><span data-stu-id="ebb02-112">This article requires an existing VM in Azure with at least one data disk attached and prepared.</span></span> <span data-ttu-id="ebb02-113">Pokud jste ještě není virtuální počítač, který můžete použít, najdete v části [vytvořit a připravit virtuální počítač s datovými disky](tutorial-manage-disks.md#create-and-attach-disks).</span><span class="sxs-lookup"><span data-stu-id="ebb02-113">If you do not already have a VM that you can use, see [Create and prepare a VM with data disks](tutorial-manage-disks.md#create-and-attach-disks).</span></span>

<span data-ttu-id="ebb02-114">V následující ukázky nahraďte názvy parametrů příklad vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ebb02-114">In the following samples, replace example parameter names with your own values.</span></span> <span data-ttu-id="ebb02-115">Zahrnout názvy parametrů příklad *myResourceGroup* a *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="ebb02-115">Example parameter names include *myResourceGroup* and *myVM*.</span></span>

1. <span data-ttu-id="ebb02-116">Operace na virtuálních pevných discích nelze provést s virtuální počítač spuštěn.</span><span class="sxs-lookup"><span data-stu-id="ebb02-116">Operations on virtual hard disks cannot be performed with the VM running.</span></span> <span data-ttu-id="ebb02-117">Zrušit přidělení virtuálního počítače s [az OM deallocate](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="ebb02-117">Deallocate your VM with [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="ebb02-118">Následující příklad zruší přidělení virtuálního počítače s názvem *Můjvp* ve skupině prostředků s názvem *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="ebb02-118">The following example deallocates the VM named *myVM* in the resource group named *myResourceGroup*:</span></span>

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > <span data-ttu-id="ebb02-119">`az vm stop`neuvolní výpočetní prostředky.</span><span class="sxs-lookup"><span data-stu-id="ebb02-119">`az vm stop` does not release the compute resources.</span></span> <span data-ttu-id="ebb02-120">Chcete-li uvolnit výpočetní prostředky, pomocí `az vm deallocate`.</span><span class="sxs-lookup"><span data-stu-id="ebb02-120">To release compute resources, use `az vm deallocate`.</span></span> <span data-ttu-id="ebb02-121">Virtuální počítač musí být navrácena rozbalte virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="ebb02-121">The VM must be deallocated to expand the virtual hard disk.</span></span>

2. <span data-ttu-id="ebb02-122">Zobrazit seznam spravovaných disků ve skupině prostředků s [seznam disků az](/cli/azure/disk#list).</span><span class="sxs-lookup"><span data-stu-id="ebb02-122">View a list of managed disks in a resource group with [az disk list](/cli/azure/disk#list).</span></span> <span data-ttu-id="ebb02-123">Tento příklad zobrazuje seznam spravovaných disků ve skupině prostředků s názvem *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="ebb02-123">The following example displays a list of managed disks in the resource group named *myResourceGroup*:</span></span>

    ```azurecli
    az disk list \
        --resource-group myResourceGroup \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

    <span data-ttu-id="ebb02-124">Rozbalte požadované disk s [az disku aktualizace](/cli/azure/disk#update).</span><span class="sxs-lookup"><span data-stu-id="ebb02-124">Expand the required disk with [az disk update](/cli/azure/disk#update).</span></span> <span data-ttu-id="ebb02-125">Následující příklad rozšíří spravované disk s názvem *myDataDisk* být *200*Gb velikost:</span><span class="sxs-lookup"><span data-stu-id="ebb02-125">The following example expands the managed disk named *myDataDisk* to be *200*Gb in size:</span></span>

    ```azurecli
    az disk update \
        --resource-group myResourceGroup \
        --name myDataDisk \
        --size-gb 200
    ```

    > [!NOTE]
    > <span data-ttu-id="ebb02-126">Když rozšiřujete se spravovaným diskem, aktualizované velikost je namapována na nejbližší velikost spravovaného disku.</span><span class="sxs-lookup"><span data-stu-id="ebb02-126">When you expand a managed disk, the updated size is mapped to the nearest managed disk size.</span></span> <span data-ttu-id="ebb02-127">Tabulku spravovaných disků na dostupné velikosti a vrstvy, najdete v části [spravované disky přehled Azure – ceny a fakturace](../windows/managed-disks-overview.md#pricing-and-billing).</span><span class="sxs-lookup"><span data-stu-id="ebb02-127">For a table of the available managed disk sizes and tiers, see [Azure Managed Disks Overview - Pricing and Billing](../windows/managed-disks-overview.md#pricing-and-billing).</span></span>

3. <span data-ttu-id="ebb02-128">Spuštění virtuálního počítače s [spuštění virtuálního počítače az](/cli/azure/vm#start).</span><span class="sxs-lookup"><span data-stu-id="ebb02-128">Start your VM with [az vm start](/cli/azure/vm#start).</span></span> <span data-ttu-id="ebb02-129">Následující příklad spustí virtuální počítač s názvem *Můjvp* ve skupině prostředků s názvem *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="ebb02-129">The following example starts the VM named *myVM* in the resource group named *myResourceGroup*:</span></span>

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

4. <span data-ttu-id="ebb02-130">SSH pro virtuální počítač s příslušnými přihlašovacími údaji.</span><span class="sxs-lookup"><span data-stu-id="ebb02-130">SSH to your VM with the appropriate credentials.</span></span> <span data-ttu-id="ebb02-131">Můžete získat veřejnou IP adresu vašeho virtuálního počítače s [az virtuálních počítačů zobrazit](/cli/azure/vm#show):</span><span class="sxs-lookup"><span data-stu-id="ebb02-131">You can obtain the public IP address of your VM with [az vm show](/cli/azure/vm#show):</span></span>

    ```azurecli
    az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
    ```

5. <span data-ttu-id="ebb02-132">Chcete-li použít rozšířené disk, rozbalte položku Základní oddílu a systému souborů.</span><span class="sxs-lookup"><span data-stu-id="ebb02-132">To use the expanded disk, you need to expand the underlying partition and filesystem.</span></span>

    <span data-ttu-id="ebb02-133">a.</span><span class="sxs-lookup"><span data-stu-id="ebb02-133">a.</span></span> <span data-ttu-id="ebb02-134">Pokud je již připojen, odpojte disk:</span><span class="sxs-lookup"><span data-stu-id="ebb02-134">If already mounted, unmount the disk:</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

    <span data-ttu-id="ebb02-135">b.</span><span class="sxs-lookup"><span data-stu-id="ebb02-135">b.</span></span> <span data-ttu-id="ebb02-136">Použití `parted` k zobrazení informací o disku a změňte velikost oddílu:</span><span class="sxs-lookup"><span data-stu-id="ebb02-136">Use `parted` to view disk information and resize the partition:</span></span>

    ```bash
    sudo parted /dev/sdc
    ```

    <span data-ttu-id="ebb02-137">Zobrazení informací o existující rozložení oddílů s `print`.</span><span class="sxs-lookup"><span data-stu-id="ebb02-137">View information about the existing partition layout with `print`.</span></span> <span data-ttu-id="ebb02-138">Výstup bude vypadat v následujícím příkladu, který ukazuje, že disku, na kterém je 215 Gb velikost:</span><span class="sxs-lookup"><span data-stu-id="ebb02-138">The output is similar to the following example, which shows the underlying disk is 215Gb in size:</span></span>

    ```bash
    GNU Parted 3.2
    Using /dev/sdc1
    Welcome to GNU Parted! Type 'help' to view a list of commands.
    (parted) print
    Model: Unknown Msft Virtual Disk (scsi)
    Disk /dev/sdc1: 215GB
    Sector size (logical/physical): 512B/4096B
    Partition Table: loop
    Disk Flags:
    
    Number  Start  End    Size   File system  Flags
        1      0.00B  107GB  107GB  ext4
    ```

    <span data-ttu-id="ebb02-139">c.</span><span class="sxs-lookup"><span data-stu-id="ebb02-139">c.</span></span> <span data-ttu-id="ebb02-140">Rozbalte oddíl s `resizepart`.</span><span class="sxs-lookup"><span data-stu-id="ebb02-140">Expand the partition with `resizepart`.</span></span> <span data-ttu-id="ebb02-141">Zadejte číslo oddílu *1*a velikost nového oddílu:</span><span class="sxs-lookup"><span data-stu-id="ebb02-141">Enter the partition number, *1*, and a size for the new partition:</span></span>

    ```bash
    (parted) resizepart
    Partition number? 1
    End?  [107GB]? 215GB
    ```

    <span data-ttu-id="ebb02-142">d.</span><span class="sxs-lookup"><span data-stu-id="ebb02-142">d.</span></span> <span data-ttu-id="ebb02-143">Chcete-li ukončit, zadejte`quit`</span><span class="sxs-lookup"><span data-stu-id="ebb02-143">To exit, enter `quit`</span></span>

5. <span data-ttu-id="ebb02-144">S oddílem po změně velikosti, ověření konzistence oddílu s `e2fsck`:</span><span class="sxs-lookup"><span data-stu-id="ebb02-144">With the partition resized, verify the partition consistency with `e2fsck`:</span></span>

    ```bash
    sudo e2fsck -f /dev/sdc1
    ```

6. <span data-ttu-id="ebb02-145">Nyní změnit velikost souborů s `resize2fs`:</span><span class="sxs-lookup"><span data-stu-id="ebb02-145">Now resize the filesystem with `resize2fs`:</span></span>

    ```bash
    sudo resize2fs /dev/sdc1
    ```

7. <span data-ttu-id="ebb02-146">Připojit oddíl do požadovaného umístění, jako například `/datadrive`:</span><span class="sxs-lookup"><span data-stu-id="ebb02-146">Mount the partition to the desired location, such as `/datadrive`:</span></span>

    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```

8. <span data-ttu-id="ebb02-147">Pokud chcete ověřit, změně velikosti disku operačního systému, použijte `df -h`.</span><span class="sxs-lookup"><span data-stu-id="ebb02-147">To verify the OS disk has been resized, use `df -h`.</span></span> <span data-ttu-id="ebb02-148">Následující příklad výstupu zobrazuje datová jednotka */dev/sdc1*, je nyní 200 GB:</span><span class="sxs-lookup"><span data-stu-id="ebb02-148">The following example output shows the data drive, */dev/sdc1*, is now 200 GB:</span></span>

    ```bash
    Filesystem      Size   Used  Avail Use% Mounted on
    /dev/sdc1        197G   60M   187G   1% /datadrive
    ```

## <a name="next-steps"></a><span data-ttu-id="ebb02-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ebb02-149">Next steps</span></span>
<span data-ttu-id="ebb02-150">Pokud potřebujete další úložiště, můžete také [přidat datových disků pro virtuální počítač s Linuxem](add-disk.md).</span><span class="sxs-lookup"><span data-stu-id="ebb02-150">If you need additional storage, you also [add data disks to a Linux VM](add-disk.md).</span></span> <span data-ttu-id="ebb02-151">Další informace o šifrování disku najdete v tématu [šifrování disky na virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure](encrypt-disks.md).</span><span class="sxs-lookup"><span data-stu-id="ebb02-151">For more information about disk encryption, see [Encrypt disks on a Linux VM using the Azure CLI](encrypt-disks.md).</span></span>
