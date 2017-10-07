---
title: "disk aaaExpand operačního systému na virtuální počítač s Linuxem s hello 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak tooexpand hello virtuální disk operačního systému (OS) na virtuální počítač s Linuxem pomocí hello 1.0 rozhraní příkazového řádku Azure a modelu nasazení Resource Manager hello"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 0db78c0b86b48b2c5358611e11bb0b7ad781a559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="expand-os-disk-on-a-linux-vm-using-hello-azure-cli-with-hello-azure-cli-10"></a><span data-ttu-id="cfa89-103">Rozbalte disk operačního systému na virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure hello hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="cfa89-103">Expand OS disk on a Linux VM using hello Azure CLI with hello Azure CLI 1.0</span></span>
<span data-ttu-id="cfa89-104">Hello výchozí velikost virtuálního pevného disku pro hello operační systém (OS) je obvykle 30 GB na virtuální počítač s Linuxem (VM) v Azure.</span><span class="sxs-lookup"><span data-stu-id="cfa89-104">hello default virtual hard disk size for hello operating system (OS) is typically 30 GB on a Linux virtual machine (VM) in Azure.</span></span> <span data-ttu-id="cfa89-105">Můžete [přidat datových disků](add-disk.md) tooprovide dalšího volného místa, ale také může přát tooexpand hello operačního systému disku.</span><span class="sxs-lookup"><span data-stu-id="cfa89-105">You can [add data disks](add-disk.md) tooprovide for additional storage space, but you may also wish tooexpand hello OS disk.</span></span> <span data-ttu-id="cfa89-106">Tento článek popisuje, jak tooexpand hello operačního systému na disku pro virtuální počítač s Linuxem pomocí nespravované disky hello Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="cfa89-106">This article details how tooexpand hello OS disk for a Linux VM using unmanaged disks with hello Azure CLI 1.0.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="cfa89-107">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="cfa89-107">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="cfa89-108">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="cfa89-108">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="cfa89-109">[Azure CLI 1.0](#prerequisites) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="cfa89-109">[Azure CLI 1.0](#prerequisites) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="cfa89-110">[Azure CLI 2.0](expand-disks.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="cfa89-110">[Azure CLI 2.0](expand-disks.md) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cfa89-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cfa89-111">Prerequisites</span></span>
<span data-ttu-id="cfa89-112">Je třeba hello [nejnovější Azure CLI 1.0](../../cli-install-nodejs.md) nainstalován a přihlášení tooan [účet Azure](https://azure.microsoft.com/pricing/free-trial/) pomocí režimu Resource Manager hello takto:</span><span class="sxs-lookup"><span data-stu-id="cfa89-112">You need hello [latest Azure CLI 1.0](../../cli-install-nodejs.md) installed and logged in tooan [Azure account](https://azure.microsoft.com/pricing/free-trial/) using hello Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="cfa89-113">V hello následující ukázky, nahraďte vlastními hodnotami příklad názvy parametrů.</span><span class="sxs-lookup"><span data-stu-id="cfa89-113">In hello following samples, replace example parameter names with your own values.</span></span> <span data-ttu-id="cfa89-114">Zahrnout názvy parametrů příklad *myResourceGroup* a *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="cfa89-114">Example parameter names include *myResourceGroup* and *myVM*.</span></span>

## <a name="expand-os-disk"></a><span data-ttu-id="cfa89-115">Rozbalení disku operačního systému</span><span class="sxs-lookup"><span data-stu-id="cfa89-115">Expand OS disk</span></span>

1. <span data-ttu-id="cfa89-116">Operace na virtuálních pevných discích nelze provést s hello virtuální počítač spuštěn.</span><span class="sxs-lookup"><span data-stu-id="cfa89-116">Operations on virtual hard disks cannot be performed with hello VM running.</span></span> <span data-ttu-id="cfa89-117">Hello následující příklad zastaví a zruší přidělení hello virtuálního počítače s názvem *Můjvp* v hello skupinu prostředků s názvem *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="cfa89-117">hello following example stops and deallocates hello VM named *myVM* in hello resource group named *myResourceGroup*:</span></span>

    ```azurecli
    azure vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > <span data-ttu-id="cfa89-118">`azure vm stop`neuvolní hello výpočetní prostředky.</span><span class="sxs-lookup"><span data-stu-id="cfa89-118">`azure vm stop` does not release hello compute resources.</span></span> <span data-ttu-id="cfa89-119">toorelease výpočetní prostředky, použijte `azure vm deallocate`.</span><span class="sxs-lookup"><span data-stu-id="cfa89-119">toorelease compute resources, use `azure vm deallocate`.</span></span> <span data-ttu-id="cfa89-120">Hello virtuálního počítače musí být navrácena tooexpand hello virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="cfa89-120">hello VM must be deallocated tooexpand hello virtual hard disk.</span></span>

2. <span data-ttu-id="cfa89-121">Aktualizovat hello velikost hello nespravované disk operačního systému pomocí hello `azure vm set` příkaz.</span><span class="sxs-lookup"><span data-stu-id="cfa89-121">Update hello size of hello unmanaged OS disk using hello `azure vm set` command.</span></span> <span data-ttu-id="cfa89-122">Následující příklad aktualizace Hello hello virtuálního počítače s názvem *Můjvp* v hello skupinu prostředků s názvem *myResourceGroup* toobe *50* GB:</span><span class="sxs-lookup"><span data-stu-id="cfa89-122">hello following example updates hello VM named *myVM* in hello resource group named *myResourceGroup* toobe *50* GB:</span></span>

    ```azurecli
    azure vm set \
        --resource-group myResourceGroup \
        --name myVM \
        --new-os-disk-size 50
    ```

3. <span data-ttu-id="cfa89-123">Spusťte virtuální počítač následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cfa89-123">Start your VM as follows:</span></span>

    ```azurecli
    azure vm start --resource-group myResourceGroup --name myVM
    ```

4. <span data-ttu-id="cfa89-124">SSH tooyour virtuálních počítačů s příslušnými přihlašovacími údaji hello.</span><span class="sxs-lookup"><span data-stu-id="cfa89-124">SSH tooyour VM with hello appropriate credentials.</span></span> <span data-ttu-id="cfa89-125">změně velikosti disku tooverify hello operačního systému, použijte `df -h`.</span><span class="sxs-lookup"><span data-stu-id="cfa89-125">tooverify hello OS disk has been resized, use `df -h`.</span></span> <span data-ttu-id="cfa89-126">primární oddíl hello ukazuje následující příklad výstupu Hello (*/dev/sda1*) je nyní 50 GB:</span><span class="sxs-lookup"><span data-stu-id="cfa89-126">hello following example output shows hello primary partition (*/dev/sda1*) is now 50 GB:</span></span>

    ```bash
    Filesystem      Size  Used Avail Use% Mounted on
    udev            1.7G     0  1.7G   0% /dev
    tmpfs           344M  5.0M  340M   2% /run
    /dev/sda1        49G  1.3G   48G   3% /
    ```

## <a name="next-steps"></a><span data-ttu-id="cfa89-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cfa89-127">Next steps</span></span>
<span data-ttu-id="cfa89-128">Pokud potřebujete další úložiště, můžete také [přidat tooa datové disky virtuálního počítače s Linuxem](add-disk.md).</span><span class="sxs-lookup"><span data-stu-id="cfa89-128">If you need additional storage, you also [add data disks tooa Linux VM](add-disk.md).</span></span> <span data-ttu-id="cfa89-129">Další informace o šifrování disku najdete v tématu [hello šifrovat disky na virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure](encrypt-disks.md).</span><span class="sxs-lookup"><span data-stu-id="cfa89-129">For more information about disk encryption, see [Encrypt disks on a Linux VM using hello Azure CLI](encrypt-disks.md).</span></span>
