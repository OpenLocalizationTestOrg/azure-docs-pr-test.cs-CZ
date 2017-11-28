---
title: "aaaCreate kopii virtuálním počítačům s Linuxem pomocí hello 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate kopii virtuálního počítače Azure Linux s hello Azure CLI 1.0 v modelu nasazení Resource Manager hello"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
tags: azure-resource-manager
ms.assetid: 770569d2-23c1-4a5b-801e-cddcd1375164
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 997a2c8109e7083ececd76fd1013e9ed4d3e6afd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-copy-of-a-linux-virtual-machine-running-on-azure-with-hello-azure-cli-10"></a><span data-ttu-id="a079c-103">Vytvořit kopii virtuální počítač s Linuxem v Azure s hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="a079c-103">Create a copy of a Linux virtual machine running on Azure with hello Azure CLI 1.0</span></span>
<span data-ttu-id="a079c-104">Tento článek ukazuje, jak hello toocreate kopie vašeho virtuálních počítačů (VM) Azure spuštěné Linux pomocí modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a079c-104">This article shows you how toocreate a copy of your Azure virtual machine (VM) running Linux using hello Resource Manager deployment model.</span></span> <span data-ttu-id="a079c-105">Nejdřív zkopírujte přes hello operačního systému a datové disky tooa nový kontejner, pak nastavíte hello síťové prostředky a vytvořit nový virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="a079c-105">First you copy over hello operating system and data disks tooa new container, then set up hello network resources and create hello new virtual machine.</span></span>

<span data-ttu-id="a079c-106">Můžete také [odesílání a vytvořit virtuální počítač z bitové kopie disku vlastní](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a079c-106">You can also [upload and create a VM from custom disk image](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="a079c-107">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="a079c-107">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="a079c-108">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="a079c-108">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="a079c-109">Rozhraní příkazového řádku Azure 1.0 – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="a079c-109">Azure CLI 1.0 – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="a079c-110">[Azure CLI 2.0](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="a079c-110">[Azure CLI 2.0](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="a079c-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="a079c-111">Before you begin</span></span>
<span data-ttu-id="a079c-112">Ujistěte se, že splňujete následující požadavky, než začnete hello kroky hello:</span><span class="sxs-lookup"><span data-stu-id="a079c-112">Ensure that you meet hello following prerequisites before you start hello steps:</span></span>

* <span data-ttu-id="a079c-113">Máte hello [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) stažen a nainstalován na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="a079c-113">You have hello [Azure CLI](../../cli-install-nodejs.md) downloaded and installed on your machine.</span></span> 
* <span data-ttu-id="a079c-114">Musíte taky některé informace o existující virtuální počítač Azure Linux:</span><span class="sxs-lookup"><span data-stu-id="a079c-114">You also need some information about your existing Azure Linux VM:</span></span>

| <span data-ttu-id="a079c-115">Informace o zdroji virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="a079c-115">Source VM information</span></span> | <span data-ttu-id="a079c-116">Kde tooget ho</span><span class="sxs-lookup"><span data-stu-id="a079c-116">Where tooget it</span></span> |
| --- | --- |
| <span data-ttu-id="a079c-117">název virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="a079c-117">VM name</span></span> |`azure vm list` |
| <span data-ttu-id="a079c-118">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="a079c-118">Resource Group name</span></span> |`azure vm list` |
| <span data-ttu-id="a079c-119">Umístění</span><span class="sxs-lookup"><span data-stu-id="a079c-119">Location</span></span> |`azure vm list` |
| <span data-ttu-id="a079c-120">Název účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="a079c-120">Storage Account name</span></span> |`azure storage account list -g <resourceGroup>` |
| <span data-ttu-id="a079c-121">Název kontejneru</span><span class="sxs-lookup"><span data-stu-id="a079c-121">Container name</span></span> |`azure storage container list -a <sourcestorageaccountname>` |
| <span data-ttu-id="a079c-122">Název zdrojového souboru virtuálního pevného disku virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="a079c-122">Source VM VHD file name</span></span> |`azure storage blob list --container <containerName>` |

* <span data-ttu-id="a079c-123">Budete potřebovat toomake některé možnosti o nový virtuální počítač:   </span><span class="sxs-lookup"><span data-stu-id="a079c-123">You will need toomake some choices about your new VM:    </span></span><br> <span data-ttu-id="a079c-124">-Název kontejneru   </span><span class="sxs-lookup"><span data-stu-id="a079c-124">-Container name    </span></span><br> <span data-ttu-id="a079c-125">-Název virtuálního počítače.   </span><span class="sxs-lookup"><span data-stu-id="a079c-125">-VM name    </span></span><br> <span data-ttu-id="a079c-126">Velikost virtuálního počítače-   </span><span class="sxs-lookup"><span data-stu-id="a079c-126">-VM size    </span></span><br> <span data-ttu-id="a079c-127">Název - vNet   </span><span class="sxs-lookup"><span data-stu-id="a079c-127">-vNet name    </span></span><br> <span data-ttu-id="a079c-128">-Název podsítě.   </span><span class="sxs-lookup"><span data-stu-id="a079c-128">-SubNet name    </span></span><br> <span data-ttu-id="a079c-129">Název - IP   </span><span class="sxs-lookup"><span data-stu-id="a079c-129">-IP Name    </span></span><br> <span data-ttu-id="a079c-130">Název - NIC</span><span class="sxs-lookup"><span data-stu-id="a079c-130">-NIC name</span></span>

## <a name="login-and-set-your-subscription"></a><span data-ttu-id="a079c-131">Přihlášení a nastavte předplatné</span><span class="sxs-lookup"><span data-stu-id="a079c-131">Login and set your subscription</span></span>
1. <span data-ttu-id="a079c-132">Přihlášení toohello rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="a079c-132">Login toohello CLI.</span></span>

    ```azurecli
    azure login
    ```
2. <span data-ttu-id="a079c-133">Ujistěte se, že jste v režimu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a079c-133">Make sure you are in Resource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```
3. <span data-ttu-id="a079c-134">Nastavit hello správné předplatné.</span><span class="sxs-lookup"><span data-stu-id="a079c-134">Set hello correct subscription.</span></span> <span data-ttu-id="a079c-135">Můžete použít "účet azure seznam" toosee všech vašich předplatných.</span><span class="sxs-lookup"><span data-stu-id="a079c-135">You can use 'azure account list' toosee all of your subscriptions.</span></span>

    ```azurecli
    azure account set mySubscriptionID
    ```

## <a name="stop-hello-vm"></a><span data-ttu-id="a079c-136">Zastavit hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="a079c-136">Stop hello VM</span></span>
<span data-ttu-id="a079c-137">Zastavte a navrátit hello zdrojového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a079c-137">Stop and deallocate hello source VM.</span></span> <span data-ttu-id="a079c-138">Můžete použít "seznam virtuálních počítačů azure" tooget seznam všech hello virtuální počítače ve vašem předplatném a jejich názvy skupin prostředků.</span><span class="sxs-lookup"><span data-stu-id="a079c-138">You can use 'azure vm list' tooget a list of all of hello VMs in your subscription and their resource group names.</span></span>

```azurecli
azure vm stop myResourceGroup myVM
azure vm deallocate myResourceGroup MyVM
```


## <a name="copy-hello-vhd"></a><span data-ttu-id="a079c-139">Zkopírujte hello virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="a079c-139">Copy hello VHD</span></span>
<span data-ttu-id="a079c-140">Hello virtuální pevný disk můžete zkopírovat z hello zdroj úložiště toohello cíl pomocí hello `azure storage blob copy start`.</span><span class="sxs-lookup"><span data-stu-id="a079c-140">You can copy hello VHD from hello source storage toohello destination using hello `azure storage blob copy start`.</span></span> <span data-ttu-id="a079c-141">V tomto příkladu přidáme toocopy hello virtuálního pevného disku toohello stejný účet úložiště, ale jiný kontejner.</span><span class="sxs-lookup"><span data-stu-id="a079c-141">In this example, we are going toocopy hello VHD toohello same storage account, but a different container.</span></span>

<span data-ttu-id="a079c-142">toocopy hello virtuálního pevného disku tooanother kontejneru v hello stejný účet úložiště, zadejte:</span><span class="sxs-lookup"><span data-stu-id="a079c-142">toocopy hello VHD tooanother container in hello same storage account, type:</span></span>

```azurecli
azure storage blob copy start \
        https://mystorageaccountname.blob.core.windows.net:8080/mycontainername/myVHD.vhd \
        myNewContainerName
```

## <a name="set-up-hello-virtual-network-for-your-new-vm"></a><span data-ttu-id="a079c-143">Nastavení hello virtuální sítě pro nový virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="a079c-143">Set up hello virtual network for your new VM</span></span>
<span data-ttu-id="a079c-144">Nastavení virtuální sítě a síťovou kartu pro nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a079c-144">Set up a virtual network and NIC for your new VM.</span></span> 

```azurecli
azure network vnet create myResourceGroup myVnet -l myLocation

azure network vnet subnet create -a <address.prefix.in.CIDR/format> myResourceGroup myVnet mySubnet

azure network public-ip create myResourceGroup myPublicIP -l myLocation

azure network nic create myResourceGroup myNic -k mySubnet -m myVnet -p myPublicIP -l myLocation
```


## <a name="create-hello-new-vm"></a><span data-ttu-id="a079c-145">Vytvoření nového virtuálního počítače hello</span><span class="sxs-lookup"><span data-stu-id="a079c-145">Create hello new VM</span></span>
<span data-ttu-id="a079c-146">Nyní můžete vytvořit virtuální počítač z nahraný virtuální disk [pomocí šablony správce prostředků](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) nebo prostřednictvím hello rozhraní příkazového řádku zadáním hello URI tooyour zkopírovaných disku zadáním:</span><span class="sxs-lookup"><span data-stu-id="a079c-146">You can now create a VM from your uploaded virtual disk [using a resource manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) or through hello CLI by specifying hello URI tooyour copied disk by typing:</span></span>

```azurecli
azure vm create -n myVM -l myLocation -g myResourceGroup -f myNic \
        -z Standard_DS1_v2 -y Linux \
        https://mystorageaccountname.blob.core.windows.net:8080/mycontainername/myVHD.vhd 
```



## <a name="next-steps"></a><span data-ttu-id="a079c-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a079c-147">Next steps</span></span>
<span data-ttu-id="a079c-148">toolearn jak toouse rozhraní příkazového řádku Azure toomanage nového virtuálního počítače najdete v části [příkazy příkazového řádku Azure CLI pro hello Azure Resource Manager](../azure-cli-arm-commands.md).</span><span class="sxs-lookup"><span data-stu-id="a079c-148">toolearn how toouse Azure CLI toomanage your new virtual machine, see [Azure CLI commands for hello Azure Resource Manager](../azure-cli-arm-commands.md).</span></span>

