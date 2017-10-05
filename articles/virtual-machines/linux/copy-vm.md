---
title: "Kopírování virtuálního počítače s Linuxem pomocí Azure CLI 2.0 | Microsoft Docs"
description: "Naučte se vytvořit kopii virtuálním počítačům s Linuxem Azure pomocí Azure CLI 2.0 a spravované disky."
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
ms.openlocfilehash: 7983061a933370803669480296d7625106e1360c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-copy-of-a-linux-vm-by-using-azure-cli-20-and-managed-disks"></a><span data-ttu-id="71c25-103">Vytvoří kopii virtuálního počítače s Linuxem pomocí Azure CLI 2.0 a spravované disky</span><span class="sxs-lookup"><span data-stu-id="71c25-103">Create a copy of a Linux VM by using Azure CLI 2.0 and Managed Disks</span></span>


<span data-ttu-id="71c25-104">Tento článek ukazuje, jak vytvořit kopii Azure virtuálního počítače (VM) s Linuxem pomocí Azure CLI 2.0 a modelu nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="71c25-104">This article shows you how to create a copy of your Azure virtual machine (VM) running Linux using the Azure CLI 2.0 and the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="71c25-105">K provedení těchto kroků můžete také využít [Azure CLI 1.0](copy-vm-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="71c25-105">You can also perform these steps with the [Azure CLI 1.0](copy-vm-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="71c25-106">Můžete také [odesílání a vytvoření virtuálního počítače z disku VHD](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="71c25-106">You can also [upload and create a VM from a VHD](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="71c25-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="71c25-107">Prerequisites</span></span>


-   <span data-ttu-id="71c25-108">Nainstalujte [rozhraní příkazového řádku Azure 2.0](/cli/azure/install-az-cli2)</span><span class="sxs-lookup"><span data-stu-id="71c25-108">Install [Azure CLI 2.0](/cli/azure/install-az-cli2)</span></span>

-   <span data-ttu-id="71c25-109">Přihlaste se k účtu Azure s [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="71c25-109">Sign in to an Azure account with [az login](/cli/azure/#login).</span></span>

-   <span data-ttu-id="71c25-110">Máte virtuální počítač Azure má použít jako zdroj pro vaší kopie.</span><span class="sxs-lookup"><span data-stu-id="71c25-110">Have an Azure VM to use as the source for your copy.</span></span>

## <a name="step-1-stop-the-source-vm"></a><span data-ttu-id="71c25-111">Krok 1: Zastavit zdrojového virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="71c25-111">Step 1: Stop the source VM</span></span>


<span data-ttu-id="71c25-112">Deallocate zdrojového virtuálního počítače pomocí [az OM deallocate](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="71c25-112">Deallocate the source VM by using [az vm deallocate](/cli/azure/vm#deallocate).</span></span>
<span data-ttu-id="71c25-113">Následující příklad zruší přidělení virtuálního počítače s názvem **Můjvp** ve skupině prostředků **myResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="71c25-113">The following example deallocates the VM named **myVM** in the resource group **myResourceGroup**:</span></span>

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

## <a name="step-2-copy-the-source-vm"></a><span data-ttu-id="71c25-114">Krok 2: Kopírování zdrojového virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="71c25-114">Step 2: Copy the source VM</span></span>


<span data-ttu-id="71c25-115">Pokud chcete zkopírovat virtuální počítač, vytvořit kopii základní virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="71c25-115">To copy a VM, you create a copy of the underlying virtual hard disk.</span></span> <span data-ttu-id="71c25-116">Tento proces vytvoří specializované virtuálního pevného disku jako spravované Disk, který obsahuje stejnou konfiguraci a nastavení jako zdrojový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="71c25-116">This process creates a specialized VHD as a Managed Disk that contains the same configuration and settings as the source VM.</span></span>

<span data-ttu-id="71c25-117">Další informace o službě Azure Managed Disks najdete v tématu [Přehled služby Azure Managed Disks](../windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="71c25-117">For more information about Azure Managed Disks, see [Azure Managed Disks overview](../windows/managed-disks-overview.md).</span></span> 

1.  <span data-ttu-id="71c25-118">Seznam jednotlivé virtuální počítače a název jeho operačního systému na disku s [seznamu virtuálních počítačů az](/cli/azure/vm#list).</span><span class="sxs-lookup"><span data-stu-id="71c25-118">List each VM and the name of its OS disk with [az vm list](/cli/azure/vm#list).</span></span> <span data-ttu-id="71c25-119">Následující příklad vypíše všechny virtuální počítače ve skupině prostředků s názvem **myResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="71c25-119">The following example lists all VMs in the resource group named **myResourceGroup**:</span></span>
    
    ```azurecli
    az vm list -g myTestRG --query '[].{Name:name,DiskName:storageProfile.osDisk.name}' --output table
    ```

    <span data-ttu-id="71c25-120">Výstup se podobá následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="71c25-120">The output is similar to the following example:</span></span>

    ```azurecli
    Name    DiskName
    ------  --------
    myVM    myDisk
    ```

1.  <span data-ttu-id="71c25-121">Kopie disku tak, že vytvoříte novou spravovat pomocí disku [vytvoření disku az](/cli/azure/disk#create).</span><span class="sxs-lookup"><span data-stu-id="71c25-121">Copy the disk by creating a new managed disk using [az disk create](/cli/azure/disk#create).</span></span> <span data-ttu-id="71c25-122">Následující příklad vytvoří disk s názvem **myCopiedDisk** ze spravovaných disk s názvem **myDisk**:</span><span class="sxs-lookup"><span data-stu-id="71c25-122">The following example creates a disk named **myCopiedDisk** from the managed disk named **myDisk**:</span></span>

    ```azurecli
    az disk create --resource-group myResourceGroup --name myCopiedDisk --source myDisk
    ``` 

1.  <span data-ttu-id="71c25-123">Ověřit spravované disky nyní ve vaší skupině prostředků pomocí [seznam disků az](/cli/azure/disk#list).</span><span class="sxs-lookup"><span data-stu-id="71c25-123">Verify the managed disks now in your resource group by using [az disk list](/cli/azure/disk#list).</span></span> <span data-ttu-id="71c25-124">Následující příklad vypíše disky spravované ve skupině prostředků s názvem **myResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="71c25-124">The following example lists the managed disks in the resource group named **myResourceGroup**:</span></span>

    ```azurecli
    az disk list --resource-group myResourceGroup --output table
    ```

1.  <span data-ttu-id="71c25-125">Přejděte k ["krok 3: nastavení virtuální sítě"](#step-3-set-up-a-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="71c25-125">Skip to ["Step 3: Set up a virtual network"](#step-3-set-up-a-virtual-network).</span></span>


## <a name="step-3-set-up-a-virtual-network"></a><span data-ttu-id="71c25-126">Krok 3: Nastavení virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="71c25-126">Step 3: Set up a virtual network</span></span>


<span data-ttu-id="71c25-127">Následující volitelný postup vytvoření nové virtuální sítě, podsítě, veřejnou IP adresu a virtuální síťová karta (NIC).</span><span class="sxs-lookup"><span data-stu-id="71c25-127">The following optional steps create a new virtual network, subnet, public IP address, and virtual network interface card (NIC).</span></span>

<span data-ttu-id="71c25-128">Pokud kopírujete virtuální počítač pro řešení potíží s účely nebo další nasazení, nemusí chcete použít virtuální počítač v existující virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="71c25-128">If you are copying a VM for troubleshooting purposes or additional deployments, you might not want to use a VM in an existing virtual network.</span></span>

<span data-ttu-id="71c25-129">Pokud chcete vytvořit virtuální síťovou infrastrukturu pro zkopírovaný virtuální počítače, postupujte podle několika dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="71c25-129">If you want to create a virtual network infrastructure for your copied VMs, follow the next few steps.</span></span> <span data-ttu-id="71c25-130">Pokud nechcete vytvořit virtuální síť, přejděte k [krok 4: vytvoření virtuálního počítače](#step-4-create-a-vm).</span><span class="sxs-lookup"><span data-stu-id="71c25-130">If you don't want to create a virtual network, skip to [Step 4: Create a VM](#step-4-create-a-vm).</span></span>

1.  <span data-ttu-id="71c25-131">Vytvoření virtuální sítě pomocí [vytvoření sítě vnet az](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="71c25-131">Create the virtual network by using [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="71c25-132">Následující příklad vytvoří virtuální síť s názvem **myVnet** a podsíť s názvem **mySubnet**:</span><span class="sxs-lookup"><span data-stu-id="71c25-132">The following example creates a virtual network named **myVnet** and a subnet named **mySubnet**:</span></span>

    ```azurecli
    az network vnet create --resource-group myResourceGroup --location westus --name myVnet \
        --address-prefix 192.168.0.0/16 --subnet-name mySubnet --subnet-prefix 192.168.1.0/24
    ```

1.  <span data-ttu-id="71c25-133">Vytvoření veřejné IP adresy pomocí [vytvoření veřejné sítě az-ip](/cli/azure/network/public-ip#create).</span><span class="sxs-lookup"><span data-stu-id="71c25-133">Create a public IP by using [az network public-ip create](/cli/azure/network/public-ip#create).</span></span> <span data-ttu-id="71c25-134">Následující příklad vytvoří veřejnou IP adresu s názvem **myPublicIP** s názvem DNS **mypublicdns**.</span><span class="sxs-lookup"><span data-stu-id="71c25-134">The following example creates a public IP named **myPublicIP** with the DNS name of **mypublicdns**.</span></span> <span data-ttu-id="71c25-135">(Název DNS musí být jedinečný, takže zadejte jedinečný název.)</span><span class="sxs-lookup"><span data-stu-id="71c25-135">(The DNS name must be unique, so provide a unique name.)</span></span>

    ```azurecli
    az network public-ip create --resource-group myResourceGroup --location westus \
        --name myPublicIP --dns-name mypublicdns --allocation-method static --idle-timeout 4
    ```

1.  <span data-ttu-id="71c25-136">Vytvořte pomocí seskupování [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="71c25-136">Create the NIC using [az network nic create](/cli/azure/network/nic#create).</span></span>
    <span data-ttu-id="71c25-137">Následující příklad vytvoří síťový adaptér s názvem **myNic** připojená k **mySubnet** podsítě:</span><span class="sxs-lookup"><span data-stu-id="71c25-137">The following example creates a NIC named **myNic** that's attached to the **mySubnet** subnet:</span></span>

    ```azurecli
    az network nic create --resource-group myResourceGroup --location westus --name myNic \
        --vnet-name myVnet --subnet mySubnet --public-ip-address myPublicIP
    ```

## <a name="step-4-create-a-vm"></a><span data-ttu-id="71c25-138">Krok 4: Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="71c25-138">Step 4: Create a VM</span></span>

<span data-ttu-id="71c25-139">Nyní můžete vytvořit virtuální počítač pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="71c25-139">You can now create a VM by using [az vm create](/cli/azure/vm#create).</span></span>

<span data-ttu-id="71c25-140">Zadejte zkopírovaný spravované disk, který chcete použít jako disk operačního systému (--připojit disk operačního systému), a to takto:</span><span class="sxs-lookup"><span data-stu-id="71c25-140">Specify the copied managed disk to use as the OS disk (--attach-os-disk), as follows:</span></span>

```azurecli
az vm create --resource-group myResourceGroup --name myCopiedVM \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics myNic --size Standard_DS1_v2 --os-type Linux \
    --attach-os-disk myCopiedDisk
```

## <a name="next-steps"></a><span data-ttu-id="71c25-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="71c25-141">Next steps</span></span>

<span data-ttu-id="71c25-142">Naučte se používat rozhraní příkazového řádku Azure ke správě nový virtuální počítač, najdete v tématu [rozhraní příkazového řádku Azure pro Azure Resource Manager](../azure-cli-arm-commands.md).</span><span class="sxs-lookup"><span data-stu-id="71c25-142">To learn how to use Azure CLI to manage your new VM, see [Azure CLI commands for the Azure Resource Manager](../azure-cli-arm-commands.md).</span></span>
