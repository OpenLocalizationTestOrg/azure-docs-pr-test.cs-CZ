---
title: "aaaExpand virtuální pevné disky na virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Zjistěte, jak hello tooexpand virtuální pevné disky na virtuální počítač s Linuxem pomocí Azure CLI 2.0"
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
ms.openlocfilehash: 7c09a682cb4322c027e57667640e8f1f8e6612f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooexpand-virtual-hard-disks-on-a-linux-vm-with-hello-azure-cli"></a><span data-ttu-id="c5caa-103">Jak hello tooexpand virtuální pevné disky na virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="c5caa-103">How tooexpand virtual hard disks on a Linux VM with hello Azure CLI</span></span>
<span data-ttu-id="c5caa-104">Hello výchozí velikost virtuálního pevného disku pro hello operační systém (OS) je obvykle 30 GB na virtuální počítač s Linuxem (VM) v Azure.</span><span class="sxs-lookup"><span data-stu-id="c5caa-104">hello default virtual hard disk size for hello operating system (OS) is typically 30 GB on a Linux virtual machine (VM) in Azure.</span></span> <span data-ttu-id="c5caa-105">Můžete [přidat datových disků](add-disk.md) tooprovide dalšího volného místa, ale může také chcete tooexpand stávající datový disk.</span><span class="sxs-lookup"><span data-stu-id="c5caa-105">You can [add data disks](add-disk.md) tooprovide for additional storage space, but you may also wish tooexpand an existing data disk.</span></span> <span data-ttu-id="c5caa-106">Tento článek popisuje, jak tooexpand spravovaná disky pro virtuální počítač s Linuxem pomocí hello 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="c5caa-106">This article details how tooexpand managed disks for a Linux VM with hello Azure CLI 2.0.</span></span> <span data-ttu-id="c5caa-107">Můžete také rozšířit hello nespravované disk operačního systému s hello [Azure CLI 1.0](expand-disks-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="c5caa-107">You can also expand hello unmanaged OS disk with hello [Azure CLI 1.0](expand-disks-nodejs.md).</span></span>

> [!WARNING]
> <span data-ttu-id="c5caa-108">Ujistěte se, můžete zálohovat data před provedením disku změnit velikost operace vždy.</span><span class="sxs-lookup"><span data-stu-id="c5caa-108">Always make sure that you back up your data before you perform disk resize operations.</span></span> <span data-ttu-id="c5caa-109">Další informace najdete v tématu [zálohovat virtuální počítače s Linuxem v Azure](tutorial-backup-vms.md).</span><span class="sxs-lookup"><span data-stu-id="c5caa-109">For more information, see [Back up Linux VMs in Azure](tutorial-backup-vms.md).</span></span>

## <a name="expand-disk"></a><span data-ttu-id="c5caa-110">Rozbalte disk</span><span class="sxs-lookup"><span data-stu-id="c5caa-110">Expand disk</span></span>
<span data-ttu-id="c5caa-111">Ujistěte se, že máte hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="c5caa-111">Make sure that you have hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="c5caa-112">Tento článek vyžaduje existující virtuální počítač v Azure s alespoň jeden datový disk připojený a připravený.</span><span class="sxs-lookup"><span data-stu-id="c5caa-112">This article requires an existing VM in Azure with at least one data disk attached and prepared.</span></span> <span data-ttu-id="c5caa-113">Pokud jste ještě není virtuální počítač, který můžete použít, najdete v části [vytvořit a připravit virtuální počítač s datovými disky](tutorial-manage-disks.md#create-and-attach-disks).</span><span class="sxs-lookup"><span data-stu-id="c5caa-113">If you do not already have a VM that you can use, see [Create and prepare a VM with data disks](tutorial-manage-disks.md#create-and-attach-disks).</span></span>

<span data-ttu-id="c5caa-114">V hello následující ukázky, nahraďte vlastními hodnotami příklad názvy parametrů.</span><span class="sxs-lookup"><span data-stu-id="c5caa-114">In hello following samples, replace example parameter names with your own values.</span></span> <span data-ttu-id="c5caa-115">Zahrnout názvy parametrů příklad *myResourceGroup* a *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="c5caa-115">Example parameter names include *myResourceGroup* and *myVM*.</span></span>

1. <span data-ttu-id="c5caa-116">Operace na virtuálních pevných discích nelze provést s hello virtuální počítač spuštěn.</span><span class="sxs-lookup"><span data-stu-id="c5caa-116">Operations on virtual hard disks cannot be performed with hello VM running.</span></span> <span data-ttu-id="c5caa-117">Zrušit přidělení virtuálního počítače s [az OM deallocate](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="c5caa-117">Deallocate your VM with [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="c5caa-118">Hello následující příklad zruší přidělení hello virtuálního počítače s názvem *Můjvp* v hello skupinu prostředků s názvem *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="c5caa-118">hello following example deallocates hello VM named *myVM* in hello resource group named *myResourceGroup*:</span></span>

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > <span data-ttu-id="c5caa-119">`az vm stop`neuvolní hello výpočetní prostředky.</span><span class="sxs-lookup"><span data-stu-id="c5caa-119">`az vm stop` does not release hello compute resources.</span></span> <span data-ttu-id="c5caa-120">toorelease výpočetní prostředky, použijte `az vm deallocate`.</span><span class="sxs-lookup"><span data-stu-id="c5caa-120">toorelease compute resources, use `az vm deallocate`.</span></span> <span data-ttu-id="c5caa-121">Hello virtuálního počítače musí být navrácena tooexpand hello virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="c5caa-121">hello VM must be deallocated tooexpand hello virtual hard disk.</span></span>

2. <span data-ttu-id="c5caa-122">Zobrazit seznam spravovaných disků ve skupině prostředků s [seznam disků az](/cli/azure/disk#list).</span><span class="sxs-lookup"><span data-stu-id="c5caa-122">View a list of managed disks in a resource group with [az disk list](/cli/azure/disk#list).</span></span> <span data-ttu-id="c5caa-123">Hello tento příklad zobrazuje seznam spravovaných disků ve skupině prostředků hello s názvem *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="c5caa-123">hello following example displays a list of managed disks in hello resource group named *myResourceGroup*:</span></span>

    ```azurecli
    az disk list \
        --resource-group myResourceGroup \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

    <span data-ttu-id="c5caa-124">Rozbalte hello požadované disku s [az disku aktualizace](/cli/azure/disk#update).</span><span class="sxs-lookup"><span data-stu-id="c5caa-124">Expand hello required disk with [az disk update](/cli/azure/disk#update).</span></span> <span data-ttu-id="c5caa-125">Hello následující příklad rozšíří hello spravované disk s názvem *myDataDisk* toobe *200*Gb velikost:</span><span class="sxs-lookup"><span data-stu-id="c5caa-125">hello following example expands hello managed disk named *myDataDisk* toobe *200*Gb in size:</span></span>

    ```azurecli
    az disk update \
        --resource-group myResourceGroup \
        --name myDataDisk \
        --size-gb 200
    ```

    > [!NOTE]
    > <span data-ttu-id="c5caa-126">Když rozšiřujete se spravovaným diskem, hello aktualizovat velikost je namapované toohello nejbližší velikost spravovaného disku.</span><span class="sxs-lookup"><span data-stu-id="c5caa-126">When you expand a managed disk, hello updated size is mapped toohello nearest managed disk size.</span></span> <span data-ttu-id="c5caa-127">Tabulku hello spravovaných disků na dostupné velikosti a vrstvy, najdete v části [spravované disky přehled Azure – ceny a fakturace](../windows/managed-disks-overview.md#pricing-and-billing).</span><span class="sxs-lookup"><span data-stu-id="c5caa-127">For a table of hello available managed disk sizes and tiers, see [Azure Managed Disks Overview - Pricing and Billing](../windows/managed-disks-overview.md#pricing-and-billing).</span></span>

3. <span data-ttu-id="c5caa-128">Spuštění virtuálního počítače s [spuštění virtuálního počítače az](/cli/azure/vm#start).</span><span class="sxs-lookup"><span data-stu-id="c5caa-128">Start your VM with [az vm start](/cli/azure/vm#start).</span></span> <span data-ttu-id="c5caa-129">Následující příklad spustí Hello hello virtuálního počítače s názvem *Můjvp* v hello skupinu prostředků s názvem *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="c5caa-129">hello following example starts hello VM named *myVM* in hello resource group named *myResourceGroup*:</span></span>

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

4. <span data-ttu-id="c5caa-130">SSH tooyour virtuálních počítačů s příslušnými přihlašovacími údaji hello.</span><span class="sxs-lookup"><span data-stu-id="c5caa-130">SSH tooyour VM with hello appropriate credentials.</span></span> <span data-ttu-id="c5caa-131">Můžete získat hello veřejnou IP adresu vašeho virtuálního počítače s [az virtuálních počítačů zobrazit](/cli/azure/vm#show):</span><span class="sxs-lookup"><span data-stu-id="c5caa-131">You can obtain hello public IP address of your VM with [az vm show](/cli/azure/vm#show):</span></span>

    ```azurecli
    az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
    ```

5. <span data-ttu-id="c5caa-132">toouse hello rozšířit disku, musíte tooexpand hello základní oddíl a systému souborů.</span><span class="sxs-lookup"><span data-stu-id="c5caa-132">toouse hello expanded disk, you need tooexpand hello underlying partition and filesystem.</span></span>

    <span data-ttu-id="c5caa-133">a.</span><span class="sxs-lookup"><span data-stu-id="c5caa-133">a.</span></span> <span data-ttu-id="c5caa-134">Pokud je již připojen, odpojte hello disk:</span><span class="sxs-lookup"><span data-stu-id="c5caa-134">If already mounted, unmount hello disk:</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

    <span data-ttu-id="c5caa-135">b.</span><span class="sxs-lookup"><span data-stu-id="c5caa-135">b.</span></span> <span data-ttu-id="c5caa-136">Použití `parted` tooview informace na disku a změňte velikost oddílu hello:</span><span class="sxs-lookup"><span data-stu-id="c5caa-136">Use `parted` tooview disk information and resize hello partition:</span></span>

    ```bash
    sudo parted /dev/sdc
    ```

    <span data-ttu-id="c5caa-137">Zobrazení informací o hello existující rozložení oddílů s `print`.</span><span class="sxs-lookup"><span data-stu-id="c5caa-137">View information about hello existing partition layout with `print`.</span></span> <span data-ttu-id="c5caa-138">Hello výstup je podobné toohello následující příklad, který ukazuje, že se základní disk hello 215 Gb velikost:</span><span class="sxs-lookup"><span data-stu-id="c5caa-138">hello output is similar toohello following example, which shows hello underlying disk is 215Gb in size:</span></span>

    ```bash
    GNU Parted 3.2
    Using /dev/sdc1
    Welcome tooGNU Parted! Type 'help' tooview a list of commands.
    (parted) print
    Model: Unknown Msft Virtual Disk (scsi)
    Disk /dev/sdc1: 215GB
    Sector size (logical/physical): 512B/4096B
    Partition Table: loop
    Disk Flags:
    
    Number  Start  End    Size   File system  Flags
        1      0.00B  107GB  107GB  ext4
    ```

    <span data-ttu-id="c5caa-139">c.</span><span class="sxs-lookup"><span data-stu-id="c5caa-139">c.</span></span> <span data-ttu-id="c5caa-140">Rozbalte oddíl hello s `resizepart`.</span><span class="sxs-lookup"><span data-stu-id="c5caa-140">Expand hello partition with `resizepart`.</span></span> <span data-ttu-id="c5caa-141">Zadejte číslo oddílu hello, *1*a velikost pro nový oddíl hello:</span><span class="sxs-lookup"><span data-stu-id="c5caa-141">Enter hello partition number, *1*, and a size for hello new partition:</span></span>

    ```bash
    (parted) resizepart
    Partition number? 1
    End?  [107GB]? 215GB
    ```

    <span data-ttu-id="c5caa-142">d.</span><span class="sxs-lookup"><span data-stu-id="c5caa-142">d.</span></span> <span data-ttu-id="c5caa-143">tooexit, zadejte`quit`</span><span class="sxs-lookup"><span data-stu-id="c5caa-143">tooexit, enter `quit`</span></span>

5. <span data-ttu-id="c5caa-144">S oddílem hello po změně velikosti, ověření konzistence oddílu hello s `e2fsck`:</span><span class="sxs-lookup"><span data-stu-id="c5caa-144">With hello partition resized, verify hello partition consistency with `e2fsck`:</span></span>

    ```bash
    sudo e2fsck -f /dev/sdc1
    ```

6. <span data-ttu-id="c5caa-145">Nyní změnit velikost hello filesystem s `resize2fs`:</span><span class="sxs-lookup"><span data-stu-id="c5caa-145">Now resize hello filesystem with `resize2fs`:</span></span>

    ```bash
    sudo resize2fs /dev/sdc1
    ```

7. <span data-ttu-id="c5caa-146">Připojení hello oddílu toohello požadovaného umístění, jako například `/datadrive`:</span><span class="sxs-lookup"><span data-stu-id="c5caa-146">Mount hello partition toohello desired location, such as `/datadrive`:</span></span>

    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```

8. <span data-ttu-id="c5caa-147">změně velikosti disku tooverify hello operačního systému, použijte `df -h`.</span><span class="sxs-lookup"><span data-stu-id="c5caa-147">tooverify hello OS disk has been resized, use `df -h`.</span></span> <span data-ttu-id="c5caa-148">Hello následující příklad výstupu zobrazuje hello datová jednotka, */dev/sdc1*, je nyní 200 GB:</span><span class="sxs-lookup"><span data-stu-id="c5caa-148">hello following example output shows hello data drive, */dev/sdc1*, is now 200 GB:</span></span>

    ```bash
    Filesystem      Size   Used  Avail Use% Mounted on
    /dev/sdc1        197G   60M   187G   1% /datadrive
    ```

## <a name="next-steps"></a><span data-ttu-id="c5caa-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c5caa-149">Next steps</span></span>
<span data-ttu-id="c5caa-150">Pokud potřebujete další úložiště, můžete také [přidat tooa datové disky virtuálního počítače s Linuxem](add-disk.md).</span><span class="sxs-lookup"><span data-stu-id="c5caa-150">If you need additional storage, you also [add data disks tooa Linux VM](add-disk.md).</span></span> <span data-ttu-id="c5caa-151">Další informace o šifrování disku najdete v tématu [hello šifrovat disky na virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure](encrypt-disks.md).</span><span class="sxs-lookup"><span data-stu-id="c5caa-151">For more information about disk encryption, see [Encrypt disks on a Linux VM using hello Azure CLI](encrypt-disks.md).</span></span>
