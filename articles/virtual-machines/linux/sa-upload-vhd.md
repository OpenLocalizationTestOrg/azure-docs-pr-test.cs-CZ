---
title: "Nahrát vlastní disk Linux s 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Vytvoření a nahrání virtuálního pevného disku (VHD) do Azure pomocí modelu nasazení Resource Manager a Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a8c7818f-eb65-409e-aa91-ce5ae975c564
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 07/10/2017
ms.author: cynthn
ms.openlocfilehash: 9159960af396e89f373da711e0cc46fdd996ab83
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-with-the-azure-cli-20"></a><span data-ttu-id="ff878-103">Nahrání a vytvoření virtuálního počítače s Linuxem z vlastní disk s 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="ff878-103">Upload and create a Linux VM from custom disk with the Azure CLI 2.0</span></span>
<span data-ttu-id="ff878-104">Tento článek ukazuje, jak nahrát virtuální pevný disk (VHD) pro účet úložiště Azure s Azure CLI 2.0 a vytvořit virtuální počítače s Linuxem z tento vlastní disk.</span><span class="sxs-lookup"><span data-stu-id="ff878-104">This article shows you how to upload a virtual hard disk (VHD) to an Azure storage account with the Azure CLI 2.0 and create Linux VMs from this custom disk.</span></span> <span data-ttu-id="ff878-105">K provedení těchto kroků můžete také využít [Azure CLI 1.0](upload-vhd-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ff878-105">You can also perform these steps with the [Azure CLI 1.0](upload-vhd-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="ff878-106">Tato funkce umožňuje instalovat a konfigurovat Linux distro svých požadavků a použije tento virtuální pevný disk k rychlému vytvoření Azure virtuální počítače (VM).</span><span class="sxs-lookup"><span data-stu-id="ff878-106">This functionality allows you to install and configure a Linux distro to your requirements and then use that VHD to quickly create Azure virtual machines (VMs).</span></span>

<span data-ttu-id="ff878-107">Toto téma používá účty úložiště pro poslední virtuální pevné disky, ale můžete také provést tyto kroky, pomocí [discích spravovaných](upload-vhd.md).</span><span class="sxs-lookup"><span data-stu-id="ff878-107">This topic uses storage accounts for the final VHDs, but you can also do these steps using [managed disks](upload-vhd.md).</span></span> 

## <a name="quick-commands"></a><span data-ttu-id="ff878-108">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="ff878-108">Quick commands</span></span>
<span data-ttu-id="ff878-109">Pokud potřebujete rychle provedení úlohy, následující část podrobně popisuje základní příkazy a nahrát VHD do Azure.</span><span class="sxs-lookup"><span data-stu-id="ff878-109">If you need to quickly accomplish the task, the following section details the base commands to upload a VHD to Azure.</span></span> <span data-ttu-id="ff878-110">Podrobnější informace a kontext pro každý krok naleznete zbývající části dokumentu, [od zde](#requirements).</span><span class="sxs-lookup"><span data-stu-id="ff878-110">More detailed information and context for each step can be found the rest of the document, [starting here](#requirements).</span></span>

<span data-ttu-id="ff878-111">Ujistěte se, že máte nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="ff878-111">Make sure that you have the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="ff878-112">V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ff878-112">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="ff878-113">Názvy parametrů příklad zahrnuté `myResourceGroup`, `mystorageaccount`, a `mydisks`.</span><span class="sxs-lookup"><span data-stu-id="ff878-113">Example parameter names included `myResourceGroup`, `mystorageaccount`, and `mydisks`.</span></span>

<span data-ttu-id="ff878-114">Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="ff878-114">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="ff878-115">Následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v `WestUs` umístění:</span><span class="sxs-lookup"><span data-stu-id="ff878-115">The following example creates a resource group named `myResourceGroup` in the `WestUs` location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="ff878-116">Vytvořit účet úložiště pro virtuální disky s [vytvořit účet úložiště az](/cli/azure/storage/account#create).</span><span class="sxs-lookup"><span data-stu-id="ff878-116">Create a storage account to hold your virtual disks with [az storage account create](/cli/azure/storage/account#create).</span></span> <span data-ttu-id="ff878-117">Následující příklad vytvoří účet úložiště s názvem `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="ff878-117">The following example creates a storage account named `mystorageaccount`:</span></span>

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

<span data-ttu-id="ff878-118">Seznam přístupové klíče pro účet úložiště s [seznam klíčů účtu úložiště az](/cli/azure/storage/account/keys#list).</span><span class="sxs-lookup"><span data-stu-id="ff878-118">List the access keys for your storage account with [az storage account keys list](/cli/azure/storage/account/keys#list).</span></span> <span data-ttu-id="ff878-119">Poznamenejte si `key1`:</span><span class="sxs-lookup"><span data-stu-id="ff878-119">Make a note of `key1`:</span></span>

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
```

<span data-ttu-id="ff878-120">Vytvořit kontejner v rámci účtu úložiště pomocí klíče úložiště, můžete získat pomocí [vytvořit kontejner úložiště az](/cli/azure/storage/container#create).</span><span class="sxs-lookup"><span data-stu-id="ff878-120">Create a container within your storage account using the storage key you obtained with [az storage container create](/cli/azure/storage/container#create).</span></span> <span data-ttu-id="ff878-121">Následující příklad vytvoří kontejner s názvem `mydisks` z úložiště klíčů hodnoty `key1`:</span><span class="sxs-lookup"><span data-stu-id="ff878-121">The following example creates a container named `mydisks` using the storage key value from `key1`:</span></span>

```azurecli
az storage container create --account-name mystorageaccount \
    --account-key key1 --name mydisks
```

<span data-ttu-id="ff878-122">Nakonec nahrát svůj disk VHD do kontejneru, který jste vytvořili pomocí [az úložiště objektů blob nahrávání](/cli/azure/storage/blob#upload).</span><span class="sxs-lookup"><span data-stu-id="ff878-122">Finally, upload your VHD to the container you created with [az storage blob upload](/cli/azure/storage/blob#upload).</span></span> <span data-ttu-id="ff878-123">Zadejte místní cestu na vaše virtuální pevný disk v části `/path/to/disk/mydisk.vhd`:</span><span class="sxs-lookup"><span data-stu-id="ff878-123">Specify the local path to your VHD under `/path/to/disk/mydisk.vhd`:</span></span>

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

<span data-ttu-id="ff878-124">Zadejte identifikátor URI na disk (`--image`) s [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="ff878-124">Specify the URI to your disk (`--image`) with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="ff878-125">Následující příklad vytvoří virtuální počítač s názvem `myVM` pomocí virtuálního disku předtím nahrála:</span><span class="sxs-lookup"><span data-stu-id="ff878-125">The following example creates a VM named `myVM` using the virtual disk previously uploaded:</span></span>

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisk/myDisks.vhd \
    --use-unmanaged-disk
```

<span data-ttu-id="ff878-126">Cílový účet úložiště musí být stejný jako kde jste nahráli virtuální disk.</span><span class="sxs-lookup"><span data-stu-id="ff878-126">The destination storage account has to be the same as where you uploaded your virtual disk to.</span></span> <span data-ttu-id="ff878-127">Budete taky muset zadat nebo odpověď výzvy pro všechny další parametry, vyžaduje **vytvořit virtuální počítač az** příkaz například virtuální sítě, veřejnou IP adresu, uživatelské jméno a klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="ff878-127">You also need to specify, or answer prompts for, all the additional parameters required by the **az vm create** command such as virtual network, public IP address, username, and SSH keys.</span></span> <span data-ttu-id="ff878-128">Další informace o [dostupné parametry příkazového řádku Resource Manager](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="ff878-128">You can read more about the [available CLI Resource Manager parameters](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span></span>

## <a name="requirements"></a><span data-ttu-id="ff878-129">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ff878-129">Requirements</span></span>
<span data-ttu-id="ff878-130">Chcete-li provést následující kroky, je třeba:</span><span class="sxs-lookup"><span data-stu-id="ff878-130">To complete the following steps, you need:</span></span>

* <span data-ttu-id="ff878-131">**Operační systém Linux nainstalován v souboru VHD** -nainstalovat [distribuce schválené pro Azure Linux](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (nebo v tématu [informace pro neschválené distribuce](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) na virtuální disk ve formátu virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="ff878-131">**Linux operating system installed in a .vhd file** - Install an [Azure-endorsed Linux distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (or see [information for non-endorsed distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) to a virtual disk in the VHD format.</span></span> <span data-ttu-id="ff878-132">Existuje několik nástrojů k vytvoření virtuálního počítače a virtuálního pevného disku:</span><span class="sxs-lookup"><span data-stu-id="ff878-132">Multiple tools exist to create a VM and VHD:</span></span>
  * <span data-ttu-id="ff878-133">Instalace a konfigurace [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) nebo [KVM](http://www.linux-kvm.org/page/RunningKVM), aby byl používáte formát bitové kopie virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="ff878-133">Install and configure [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) or [KVM](http://www.linux-kvm.org/page/RunningKVM), taking care to use VHD as your image format.</span></span> <span data-ttu-id="ff878-134">V případě potřeby můžete [převést bitovou kopii](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) pomocí `qemu-img convert`.</span><span class="sxs-lookup"><span data-stu-id="ff878-134">If needed, you can [convert an image](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) using `qemu-img convert`.</span></span>
  * <span data-ttu-id="ff878-135">Můžete také použít technologie Hyper-V [ve Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) nebo [v systému Windows Server 2012 nebo 2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="ff878-135">You can also use Hyper-V [on Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) or [on Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="ff878-136">Novější formát VHDX není podporovaný v Azure.</span><span class="sxs-lookup"><span data-stu-id="ff878-136">The newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="ff878-137">Když vytvoříte virtuální počítač, zadejte jako formát virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="ff878-137">When you create a VM, specify VHD as the format.</span></span> <span data-ttu-id="ff878-138">V případě potřeby můžete převést disků VHDX virtuální pevný disk pomocí [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) nebo [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ff878-138">If needed, you can convert VHDX disks to VHD using [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) or the [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="ff878-139">Navíc Azure nepodporuje odesílání dynamické virtuální pevné disky, takže je nutné převést tyto disky na virtuální pevné disky statické před nahráním.</span><span class="sxs-lookup"><span data-stu-id="ff878-139">Further, Azure does not support uploading dynamic VHDs, so you need to convert such disks to static VHDs before uploading.</span></span> <span data-ttu-id="ff878-140">Můžete použít nástroje, jako [nástroje Azure virtuálního pevného disku pro přejděte](https://github.com/Microsoft/azure-vhd-utils-for-go) převést dynamické disky během procesu odesílání do Azure.</span><span class="sxs-lookup"><span data-stu-id="ff878-140">You can use tools such as [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) to convert dynamic disks during the process of uploading to Azure.</span></span>
> 
> 

* <span data-ttu-id="ff878-141">Virtuální počítače vytvořené z vlastní disk se musí nacházet ve stejném účtu úložiště jako samotném disku</span><span class="sxs-lookup"><span data-stu-id="ff878-141">VMs created from your custom disk must reside in the same storage account as the disk itself</span></span>
  * <span data-ttu-id="ff878-142">Vytvoření účtu úložiště a kontejner pro uložení vlastní disku a vytvořené virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="ff878-142">Create a storage account and container to hold both your custom disk and created VMs</span></span>
  * <span data-ttu-id="ff878-143">Po vytvoření všechny virtuální počítače, můžete bezpečně odstranit disk</span><span class="sxs-lookup"><span data-stu-id="ff878-143">After you have created all your VMs, you can safely delete your disk</span></span>

<span data-ttu-id="ff878-144">Ujistěte se, že máte nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="ff878-144">Make sure that you have the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="ff878-145">V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ff878-145">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="ff878-146">Názvy parametrů příklad zahrnuté `myResourceGroup`, `mystorageaccount`, a `mydisks`.</span><span class="sxs-lookup"><span data-stu-id="ff878-146">Example parameter names included `myResourceGroup`, `mystorageaccount`, and `mydisks`.</span></span>

<span data-ttu-id="ff878-147"><a id="prepimage"> </a></span><span class="sxs-lookup"><span data-stu-id="ff878-147"><a id="prepimage"> </a></span></span>

## <a name="prepare-the-disk-to-be-uploaded"></a><span data-ttu-id="ff878-148">Příprava disku k odeslání</span><span class="sxs-lookup"><span data-stu-id="ff878-148">Prepare the disk to be uploaded</span></span>
<span data-ttu-id="ff878-149">Azure podporuje různé Linuxových distribucích (viz [distribuce schválené](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span><span class="sxs-lookup"><span data-stu-id="ff878-149">Azure supports various Linux distributions (see [Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span></span> <span data-ttu-id="ff878-150">Postup přípravy různé distribuce systému Linux, které jsou podporovány v Azure vás provede v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="ff878-150">The following articles guide you through how to prepare the various Linux distributions that are supported on Azure:</span></span>

* <span data-ttu-id="ff878-151">**[Na základě centOS distribuce](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="ff878-151">**[CentOS-based Distributions](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="ff878-152">**[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="ff878-152">**[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="ff878-153">**[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="ff878-153">**[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="ff878-154">**[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="ff878-154">**[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="ff878-155">**[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="ff878-155">**[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="ff878-156">**[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="ff878-156">**[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="ff878-157">**[Další - neschválené distribuce](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="ff878-157">**[Other - Non-Endorsed Distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>

<span data-ttu-id="ff878-158">Viz také  **[poznámky k instalaci Linux](create-upload-generic.md#general-linux-installation-notes)**  Příprava bitové kopie systému Linux na Azure další Obecné tipy pro.</span><span class="sxs-lookup"><span data-stu-id="ff878-158">Also see the **[Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes)** for more general tips on preparing Linux images for Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="ff878-159">[Platformy Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) se vztahují na virtuální počítače se systémem Linux jenom v případě, že jeden z potvrzená distribuce se používá s podrobností konfigurace uvedeného v části "podporované verze [Linux na Azure-Endorsed distribuce](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ff878-159">The [Azure platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) applies to VMs running Linux only when one of the endorsed distributions is used with the configuration details as specified under 'Supported Versions' in [Linux on Azure-Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
> 

## <a name="create-a-resource-group"></a><span data-ttu-id="ff878-160">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="ff878-160">Create a resource group</span></span>
<span data-ttu-id="ff878-161">Skupiny prostředků logicky seskupit všechny prostředky Azure pro podporu virtuálních počítačů, jako je například virtuální sítě a úložiště.</span><span class="sxs-lookup"><span data-stu-id="ff878-161">Resource groups logically bring together all the Azure resources to support your virtual machines, such as the virtual networking and storage.</span></span> <span data-ttu-id="ff878-162">Další informace o skupin prostředků, najdete v části [skupiny zdrojů přehled](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ff878-162">For more information resource groups, see [resource groups overview](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="ff878-163">Před nahráním váš vlastní disk a vytváření virtuálních počítačů, musíte nejprve vytvořit skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="ff878-163">Before uploading your custom disk and creating VMs, you first need to create a resource group with [az group create](/cli/azure/group#create).</span></span>

<span data-ttu-id="ff878-164">Následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v `westus` umístění:</span><span class="sxs-lookup"><span data-stu-id="ff878-164">The following example creates a resource group named `myResourceGroup` in the `westus` location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-a-storage-account"></a><span data-ttu-id="ff878-165">vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="ff878-165">Create a storage account</span></span>

<span data-ttu-id="ff878-166">Vytvoření účtu úložiště vlastní disk a virtuální počítače s [vytvořit účet úložiště az](/cli/azure/storage/account#create).</span><span class="sxs-lookup"><span data-stu-id="ff878-166">Create a storage account for your custom disk and VMs with [az storage account create](/cli/azure/storage/account#create).</span></span> <span data-ttu-id="ff878-167">Všechny virtuální počítače s nespravované disky, které vytvoříte z vaší vlastní disku musí být ve stejném účtu úložiště jako disk.</span><span class="sxs-lookup"><span data-stu-id="ff878-167">Any VMs with unmanaged disks that you create from your custom disk need to be in the same storage account as that disk.</span></span> 

<span data-ttu-id="ff878-168">Následující příklad vytvoří účet úložiště s názvem `mystorageaccount` ve skupině prostředků vytvořili:</span><span class="sxs-lookup"><span data-stu-id="ff878-168">The following example creates a storage account named `mystorageaccount` in the resource group previously created:</span></span>

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

## <a name="list-storage-account-keys"></a><span data-ttu-id="ff878-169">Vypsat klíče účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="ff878-169">List storage account keys</span></span>
<span data-ttu-id="ff878-170">Vygeneruje Azure dva 512bitové přístupové klíče pro každý účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="ff878-170">Azure generates two 512-bit access keys for each storage account.</span></span> <span data-ttu-id="ff878-171">Tyto přístupové klíče se používají při ověřování k účtu úložiště, například k provedení operace zápisu.</span><span class="sxs-lookup"><span data-stu-id="ff878-171">These access keys are used when authenticating to the storage account, such as to carry out write operations.</span></span> <span data-ttu-id="ff878-172">Další informace o [Správa přístupu k úložišti zde](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="ff878-172">Read more about [managing access to storage here](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span> <span data-ttu-id="ff878-173">Zobrazit přístupové klíče s [seznam klíčů účtu úložiště az](/cli/azure/storage/account/keys#list).</span><span class="sxs-lookup"><span data-stu-id="ff878-173">You view the access keys with [az storage account keys list](/cli/azure/storage/account/keys#list).</span></span>

<span data-ttu-id="ff878-174">Zobrazte přístupové klíče pro účet úložiště, který jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="ff878-174">View the access keys for the storage account you created:</span></span>

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
```

<span data-ttu-id="ff878-175">Výstup je podobný tomuto:</span><span class="sxs-lookup"><span data-stu-id="ff878-175">The output is similar to:</span></span>

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK
```
<span data-ttu-id="ff878-176">Poznamenejte si `key1` jako použije k interakci se svým účtem úložiště v dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="ff878-176">Make a note of `key1` as you will use it to interact with your storage account in the next steps.</span></span>

## <a name="create-a-storage-container"></a><span data-ttu-id="ff878-177">Vytvoření kontejneru úložiště</span><span class="sxs-lookup"><span data-stu-id="ff878-177">Create a storage container</span></span>
<span data-ttu-id="ff878-178">V stejným způsobem, který vytvoříte různé adresáře, které logicky uspořádat do místního systému souborů můžete vytvořit kontejnery v účtu úložiště pro uspořádání vaše disky.</span><span class="sxs-lookup"><span data-stu-id="ff878-178">In the same way that you create different directories to logically organize your local file system, you create containers within a storage account to organize your disks.</span></span> <span data-ttu-id="ff878-179">Účet úložiště může obsahovat libovolný počet kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="ff878-179">A storage account can contain any number of containers.</span></span> <span data-ttu-id="ff878-180">Vytvořit kontejner s [vytvořit kontejner úložiště az](/cli/azure/storage/container#create).</span><span class="sxs-lookup"><span data-stu-id="ff878-180">Create a container with [az storage container create](/cli/azure/storage/container#create).</span></span>

<span data-ttu-id="ff878-181">Následující příklad vytvoří kontejner s názvem `mydisks`:</span><span class="sxs-lookup"><span data-stu-id="ff878-181">The following example creates a container named `mydisks`:</span></span>

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

## <a name="upload-vhd"></a><span data-ttu-id="ff878-182">Nahrání virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="ff878-182">Upload VHD</span></span>
<span data-ttu-id="ff878-183">Teď nahrát vlastní disk s [az úložiště objektů blob nahrávání](/cli/azure/storage/blob#upload).</span><span class="sxs-lookup"><span data-stu-id="ff878-183">Now upload your custom disk with [az storage blob upload](/cli/azure/storage/blob#upload).</span></span> <span data-ttu-id="ff878-184">Nahrání a ukládat vlastní disk jako objekt blob stránky.</span><span class="sxs-lookup"><span data-stu-id="ff878-184">You upload and store your custom disk as a page blob.</span></span>

<span data-ttu-id="ff878-185">Zadejte přístupový klíč, kontejner, který jste vytvořili v předchozím kroku a pak cestu k vlastní disku v místním počítači:</span><span class="sxs-lookup"><span data-stu-id="ff878-185">Specify your access key, the container you created in the previous step, and then the path to the custom disk on your local computer:</span></span>

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

## <a name="create-the-vm"></a><span data-ttu-id="ff878-186">Vytvořte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="ff878-186">Create the VM</span></span>
<span data-ttu-id="ff878-187">Pokud chcete vytvořit virtuální počítač s nespravované disky, zadejte identifikátor URI na disk (`--image`) s [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="ff878-187">To create a VM with unmanaged disks, specify the URI to your disk (`--image`) with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="ff878-188">Následující příklad vytvoří virtuální počítač s názvem `myVM` pomocí virtuálního disku předtím nahrála:</span><span class="sxs-lookup"><span data-stu-id="ff878-188">The following example creates a VM named `myVM` using the virtual disk previously uploaded:</span></span>

<span data-ttu-id="ff878-189">Zadáte `--image` parametr s [vytvořit virtuální počítač az](/cli/azure/vm#create) tak, aby odkazovalo na vlastní disk.</span><span class="sxs-lookup"><span data-stu-id="ff878-189">You specify the `--image` parameter with [az vm create](/cli/azure/vm#create) to point to your custom disk.</span></span> <span data-ttu-id="ff878-190">Ujistěte se, že `--storage-account` odpovídá účet úložiště, kde je uložený váš vlastní disk.</span><span class="sxs-lookup"><span data-stu-id="ff878-190">Ensure that `--storage-account` matches the storage account where your custom disk is stored.</span></span> <span data-ttu-id="ff878-191">Nemusíte používat stejný kontejner jako vlastní disk k uložení virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ff878-191">You do not have to use the same container as the custom disk to store your VMs.</span></span> <span data-ttu-id="ff878-192">Ujistěte se, že jste před nahráním váš vlastní disk vytvořit žádné další kontejnery stejným způsobem jako v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="ff878-192">Make sure to create any additional containers in the same way as the earlier steps before uploading your custom disk.</span></span>

<span data-ttu-id="ff878-193">Následující příklad vytvoří virtuální počítač s názvem `myVM` z vlastní disku:</span><span class="sxs-lookup"><span data-stu-id="ff878-193">The following example creates a VM named `myVM` from your custom disk:</span></span>

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd \
    --use-unmanaged-disk
```

<span data-ttu-id="ff878-194">Stále je třeba zadat nebo odpověď výzvy pro všechny další parametry, vyžaduje **vytvořit virtuální počítač az** příkaz například uživatelské jméno a klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="ff878-194">You still need to specify, or answer prompts for, all the additional parameters required by the **az vm create** command such as username and SSH keys.</span></span>


## <a name="resource-manager-template"></a><span data-ttu-id="ff878-195">Šablona Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="ff878-195">Resource Manager template</span></span>
<span data-ttu-id="ff878-196">Šablony Azure Resource Manageru jsou soubory JavaScript Object Notation (JSON), které definují prostředí, ve kterém chcete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="ff878-196">Azure Resource Manager templates are JavaScript Object Notation (JSON) files that define the environment you wish to build.</span></span> <span data-ttu-id="ff878-197">Šablony jsou rozděleny různých prostředků zprostředkovatelé například výpočetní nebo sítě.</span><span class="sxs-lookup"><span data-stu-id="ff878-197">The templates are broken down in to different resource providers such as compute or network.</span></span> <span data-ttu-id="ff878-198">Můžete použít existující šablony nebo napsat vlastní.</span><span class="sxs-lookup"><span data-stu-id="ff878-198">You can use existing templates or write your own.</span></span> <span data-ttu-id="ff878-199">Další informace o [pomocí Resource Manageru a šablony](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ff878-199">Read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="ff878-200">V rámci `Microsoft.Compute/virtualMachines` poskytovatele šablony, máte `storageProfile` uzlu, který obsahuje podrobnosti o konfiguraci pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="ff878-200">Within the `Microsoft.Compute/virtualMachines` provider of your template, you have a `storageProfile` node that contains the configuration details for your VM.</span></span> <span data-ttu-id="ff878-201">Jsou dvě hlavní parametry, které chcete upravit `image` a `vhd` identifikátory URI, které odkazují na vlastní disk a virtuální disk nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ff878-201">The two main parameters to edit are the `image` and `vhd` URIs that point to your custom disk and your new VM's virtual disk.</span></span> <span data-ttu-id="ff878-202">Na obrázku je příkladem JSON pro použití vlastního disku:</span><span class="sxs-lookup"><span data-stu-id="ff878-202">The following shows an example of the JSON for using a custom disk:</span></span>

```json
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

<span data-ttu-id="ff878-203">Můžete použít [tento existující šablonu k vytvoření virtuálního počítače z vlastní image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) nebo přečtěte si o [vlastních šablon Azure Resource Manager](../../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="ff878-203">You can use [this existing template to create a VM from a custom image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) or read about [creating your own Azure Resource Manager templates](../../azure-resource-manager/resource-group-authoring-templates.md).</span></span> 

<span data-ttu-id="ff878-204">Jakmile máte šablonu nakonfigurována, použijte [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create) k vytvoření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ff878-204">Once you have a template configured, use [az group deployment create](/cli/azure/group/deployment#create) to create your VMs.</span></span> <span data-ttu-id="ff878-205">Zadejte identifikátor URI šablony JSON s `--template-uri` parametr:</span><span class="sxs-lookup"><span data-stu-id="ff878-205">Specify the URI of your JSON template with the `--template-uri` parameter:</span></span>

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-uri https://uri.to.template/mytemplate.json
```

<span data-ttu-id="ff878-206">Pokud máte soubor JSON, který je uložený místně v počítači, můžete použít `--template-file` parametr místo:</span><span class="sxs-lookup"><span data-stu-id="ff878-206">If you have a JSON file stored locally on your computer, you can use the `--template-file` parameter instead:</span></span>

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a><span data-ttu-id="ff878-207">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ff878-207">Next steps</span></span>
<span data-ttu-id="ff878-208">Po připravené a nahrát vlastní virtuální disk, si můžete přečíst více o [pomocí Resource Manageru a šablony](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ff878-208">After you have prepared and uploaded your custom virtual disk, you can read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="ff878-209">Můžete také chtít [přidat datový disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) na nové virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="ff878-209">You may also want to [add a data disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) to your new VMs.</span></span> <span data-ttu-id="ff878-210">Pokud máte aplikace spuštěné na vaše virtuální počítače, které potřebujete získat přístup, je nutné [otevřete porty a koncové body](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ff878-210">If you have applications running on your VMs that you need to access, be sure to [open ports and endpoints](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

