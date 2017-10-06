---
title: "virtuální počítač s Linuxem pomocí Azure CLI 2.0 aaaCopy | Microsoft Docs"
description: "Zjistěte, jak toocreate kopii virtuálním počítačům s Linuxem Azure pomocí Azure CLI 2.0 a spravované disky."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
tags: azure-resource-manager
ms.assetid: 770569d2-23c1-4a5b-801e-cddcd1375164
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 03/10/2017
ms.author: cynthn
ms.openlocfilehash: ee34a4259dd0c1e7bf49312fe3fe3ba809bf69e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-copy-of-a-linux-vm-by-using-azure-cli-20-and-managed-disks"></a><span data-ttu-id="44b08-103">Vytvoří kopii virtuálního počítače s Linuxem pomocí Azure CLI 2.0 a spravované disky</span><span class="sxs-lookup"><span data-stu-id="44b08-103">Create a copy of a Linux VM by using Azure CLI 2.0 and Managed Disks</span></span>


<span data-ttu-id="44b08-104">Tento článek ukazuje, jak hello toocreate kopii Azure virtuálního počítače (VM) systémem Linux pomocí Azure CLI 2.0 a modelu nasazení Azure Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="44b08-104">This article shows you how toocreate a copy of your Azure virtual machine (VM) running Linux using hello Azure CLI 2.0 and hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="44b08-105">Můžete také provést tyto kroky hello [Azure CLI 1.0](copy-vm-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="44b08-105">You can also perform these steps with hello [Azure CLI 1.0](copy-vm-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="44b08-106">Můžete také [odesílání a vytvoření virtuálního počítače z disku VHD](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="44b08-106">You can also [upload and create a VM from a VHD](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="44b08-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="44b08-107">Prerequisites</span></span>


-   <span data-ttu-id="44b08-108">Nainstalujte [rozhraní příkazového řádku Azure 2.0](/cli/azure/install-az-cli2)</span><span class="sxs-lookup"><span data-stu-id="44b08-108">Install [Azure CLI 2.0](/cli/azure/install-az-cli2)</span></span>

-   <span data-ttu-id="44b08-109">Přihlaste se tooan účet Azure s [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="44b08-109">Sign in tooan Azure account with [az login](/cli/azure/#login).</span></span>

-   <span data-ttu-id="44b08-110">Máte virtuálním počítači Azure toouse jako zdroj hello vaší kopie.</span><span class="sxs-lookup"><span data-stu-id="44b08-110">Have an Azure VM toouse as hello source for your copy.</span></span>

## <a name="step-1-stop-hello-source-vm"></a><span data-ttu-id="44b08-111">Krok 1: Zastavení hello zdrojového virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="44b08-111">Step 1: Stop hello source VM</span></span>


<span data-ttu-id="44b08-112">Deallocate hello zdrojového virtuálního počítače pomocí [az OM deallocate](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="44b08-112">Deallocate hello source VM by using [az vm deallocate](/cli/azure/vm#deallocate).</span></span>
<span data-ttu-id="44b08-113">Hello následující příklad zruší přidělení hello virtuálního počítače s názvem **Můjvp** ve skupině prostředků hello **myResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="44b08-113">hello following example deallocates hello VM named **myVM** in hello resource group **myResourceGroup**:</span></span>

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

## <a name="step-2-copy-hello-source-vm"></a><span data-ttu-id="44b08-114">Krok 2: Kopírování hello zdrojového virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="44b08-114">Step 2: Copy hello source VM</span></span>


<span data-ttu-id="44b08-115">toocopy virtuálního počítače, vytvořte kopii hello základní virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="44b08-115">toocopy a VM, you create a copy of hello underlying virtual hard disk.</span></span> <span data-ttu-id="44b08-116">Tento proces vytvoří specializované virtuálního pevného disku jako spravované Disk, který obsahuje hello stejné konfigurace a nastavení, jako hello zdrojového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="44b08-116">This process creates a specialized VHD as a Managed Disk that contains hello same configuration and settings as hello source VM.</span></span>

<span data-ttu-id="44b08-117">Další informace o službě Azure Managed Disks najdete v tématu [Přehled služby Azure Managed Disks](../windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="44b08-117">For more information about Azure Managed Disks, see [Azure Managed Disks overview](../windows/managed-disks-overview.md).</span></span> 

1.  <span data-ttu-id="44b08-118">Seznam každý virtuální počítač a hello název jeho disk operačního systému s [seznamu virtuálních počítačů az](/cli/azure/vm#list).</span><span class="sxs-lookup"><span data-stu-id="44b08-118">List each VM and hello name of its OS disk with [az vm list](/cli/azure/vm#list).</span></span> <span data-ttu-id="44b08-119">Hello následující příklad vypíše všechny virtuální počítače ve skupině prostředků s názvem **myResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="44b08-119">hello following example lists all VMs in the resource group named **myResourceGroup**:</span></span>
    
    ```azurecli
    az vm list -g myTestRG --query '[].{Name:name,DiskName:storageProfile.osDisk.name}' --output table
    ```

    <span data-ttu-id="44b08-120">Hello výstup je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="44b08-120">hello output is similar toohello following example:</span></span>

    ```azurecli
    Name    DiskName
    ------  --------
    myVM    myDisk
    ```

1.  <span data-ttu-id="44b08-121">Kopírování hello disku tak, že vytvoříte novou spravovat pomocí disku [vytvoření disku az](/cli/azure/disk#create).</span><span class="sxs-lookup"><span data-stu-id="44b08-121">Copy hello disk by creating a new managed disk using [az disk create](/cli/azure/disk#create).</span></span> <span data-ttu-id="44b08-122">Hello následující příklad vytvoří disk s názvem **myCopiedDisk** z hello spravované disk s názvem **myDisk**:</span><span class="sxs-lookup"><span data-stu-id="44b08-122">hello following example creates a disk named **myCopiedDisk** from hello managed disk named **myDisk**:</span></span>

    ```azurecli
    az disk create --resource-group myResourceGroup --name myCopiedDisk --source myDisk
    ``` 

1.  <span data-ttu-id="44b08-123">Ověřit disky hello spravované nyní ve vaší skupině prostředků pomocí [seznam disků az](/cli/azure/disk#list).</span><span class="sxs-lookup"><span data-stu-id="44b08-123">Verify hello managed disks now in your resource group by using [az disk list](/cli/azure/disk#list).</span></span> <span data-ttu-id="44b08-124">Hello následující příklad vypíše disky hello spravované v hello skupinu prostředků s názvem **myResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="44b08-124">hello following example lists hello managed disks in hello resource group named **myResourceGroup**:</span></span>

    ```azurecli
    az disk list --resource-group myResourceGroup --output table
    ```

1.  <span data-ttu-id="44b08-125">Přeskočit příliš["krok 3: nastavení virtuální sítě"](#step-3-set-up-a-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="44b08-125">Skip too["Step 3: Set up a virtual network"](#step-3-set-up-a-virtual-network).</span></span>


## <a name="step-3-set-up-a-virtual-network"></a><span data-ttu-id="44b08-126">Krok 3: Nastavení virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="44b08-126">Step 3: Set up a virtual network</span></span>


<span data-ttu-id="44b08-127">Hello následující volitelné kroky vytvoření nové virtuální sítě, podsítě, veřejnou IP adresu a virtuální síťová karta (NIC).</span><span class="sxs-lookup"><span data-stu-id="44b08-127">hello following optional steps create a new virtual network, subnet, public IP address, and virtual network interface card (NIC).</span></span>

<span data-ttu-id="44b08-128">Pokud kopírujete virtuální počítač pro řešení potíží s účely nebo další nasazení, není vhodné toouse virtuálního počítače ve stávající virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="44b08-128">If you are copying a VM for troubleshooting purposes or additional deployments, you might not want toouse a VM in an existing virtual network.</span></span>

<span data-ttu-id="44b08-129">Pokud chcete toocreate infrastruktury virtuální sítě pro zkopírovaný virtuální počítače, postupujte podle hello vedle několika krocích.</span><span class="sxs-lookup"><span data-stu-id="44b08-129">If you want toocreate a virtual network infrastructure for your copied VMs, follow hello next few steps.</span></span> <span data-ttu-id="44b08-130">Pokud nechcete, aby toocreate virtuální síť, přeskočte příliš[krok 4: vytvoření virtuálního počítače](#step-4-create-a-vm).</span><span class="sxs-lookup"><span data-stu-id="44b08-130">If you don't want toocreate a virtual network, skip too[Step 4: Create a VM](#step-4-create-a-vm).</span></span>

1.  <span data-ttu-id="44b08-131">Vytvoření virtuální sítě hello pomocí [vytvoření sítě vnet az](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="44b08-131">Create hello virtual network by using [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="44b08-132">Hello následující příklad vytvoří virtuální síť s názvem **myVnet** a podsíť s názvem **mySubnet**:</span><span class="sxs-lookup"><span data-stu-id="44b08-132">hello following example creates a virtual network named **myVnet** and a subnet named **mySubnet**:</span></span>

    ```azurecli
    az network vnet create --resource-group myResourceGroup --location westus --name myVnet \
        --address-prefix 192.168.0.0/16 --subnet-name mySubnet --subnet-prefix 192.168.1.0/24
    ```

1.  <span data-ttu-id="44b08-133">Vytvoření veřejné IP adresy pomocí [vytvoření veřejné sítě az-ip](/cli/azure/network/public-ip#create).</span><span class="sxs-lookup"><span data-stu-id="44b08-133">Create a public IP by using [az network public-ip create](/cli/azure/network/public-ip#create).</span></span> <span data-ttu-id="44b08-134">Hello následující příklad vytvoří veřejnou IP adresu s názvem **myPublicIP** s názvem DNS hello **mypublicdns**.</span><span class="sxs-lookup"><span data-stu-id="44b08-134">hello following example creates a public IP named **myPublicIP** with hello DNS name of **mypublicdns**.</span></span> <span data-ttu-id="44b08-135">(název DNS hello musí být jedinečný, takže zadejte jedinečný název.)</span><span class="sxs-lookup"><span data-stu-id="44b08-135">(hello DNS name must be unique, so provide a unique name.)</span></span>

    ```azurecli
    az network public-ip create --resource-group myResourceGroup --location westus \
        --name myPublicIP --dns-name mypublicdns --allocation-method static --idle-timeout 4
    ```

1.  <span data-ttu-id="44b08-136">Vytvořte pomocí seskupování hello [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="44b08-136">Create hello NIC using [az network nic create](/cli/azure/network/nic#create).</span></span>
    <span data-ttu-id="44b08-137">Hello následující příklad vytvoří síťový adaptér s názvem **myNic** toho připojené toothe **mySubnet** podsítě:</span><span class="sxs-lookup"><span data-stu-id="44b08-137">hello following example creates a NIC named **myNic** that's attached toothe **mySubnet** subnet:</span></span>

    ```azurecli
    az network nic create --resource-group myResourceGroup --location westus --name myNic \
        --vnet-name myVnet --subnet mySubnet --public-ip-address myPublicIP
    ```

## <a name="step-4-create-a-vm"></a><span data-ttu-id="44b08-138">Krok 4: Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="44b08-138">Step 4: Create a VM</span></span>

<span data-ttu-id="44b08-139">Nyní můžete vytvořit virtuální počítač pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="44b08-139">You can now create a VM by using [az vm create](/cli/azure/vm#create).</span></span>

<span data-ttu-id="44b08-140">Zadejte hello zkopírovat toouse spravované disk jako disk hello operačního systému (--připojit disk operačního systému), a to takto:</span><span class="sxs-lookup"><span data-stu-id="44b08-140">Specify hello copied managed disk toouse as hello OS disk (--attach-os-disk), as follows:</span></span>

```azurecli
az vm create --resource-group myResourceGroup --name myCopiedVM \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics myNic --size Standard_DS1_v2 --os-type Linux \
    --attach-os-disk myCopiedDisk
```

## <a name="next-steps"></a><span data-ttu-id="44b08-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="44b08-141">Next steps</span></span>

<span data-ttu-id="44b08-142">toolearn jak toouse rozhraní příkazového řádku Azure toomanage nový virtuální počítač, najdete v části [příkazy příkazového řádku Azure CLI pro hello Azure Resource Manager](../azure-cli-arm-commands.md).</span><span class="sxs-lookup"><span data-stu-id="44b08-142">toolearn how toouse Azure CLI toomanage your new VM, see [Azure CLI commands for hello Azure Resource Manager](../azure-cli-arm-commands.md).</span></span>
