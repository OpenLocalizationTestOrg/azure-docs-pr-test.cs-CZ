---
title: "Nahrát, nebo můžete zkopírovat vlastní virtuální počítač s Linuxem pomocí Azure CLI 2.0 | Microsoft Docs"
description: "Nahrát, nebo můžete zkopírovat vlastní virtuální počítač pomocí modelu nasazení Resource Manager a Azure CLI 2.0"
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
ms.date: 07/06/2017
ms.author: cynthn
ms.openlocfilehash: 7c297725c26ea6c44403a10ecdcc3542f89f10b4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-linux-vm-from-custom-disk-with-the-azure-cli-20"></a><span data-ttu-id="e3ea0-103">Vytvoření virtuálního počítače s Linuxem z vlastní disk s 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="e3ea0-103">Create a Linux VM from custom disk with the Azure CLI 2.0</span></span>

<!-- rename to create-vm-specialized -->

<span data-ttu-id="e3ea0-104">Tento článek ukazuje, jak nahrát vlastní virtuální pevný disk (VHD) nebo kopírování existující virtuální pevný disk v Azure a vytvoření nových Linux virtuálních počítačů (VM) z vlastní disku.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-104">This article shows you how to upload a customized virtual hard disk (VHD) or copy a an existing VHD in Azure and create new Linux virtual machines (VMs) from the custom disk.</span></span> <span data-ttu-id="e3ea0-105">Můžete nainstalovat a nakonfigurovat Linux distro svých požadavků a použije tento virtuální pevný disk rychle vytvořit nový virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-105">You can install and configure a Linux distro to your requirements and then use that VHD to quickly create a new Azure virtual machine.</span></span>

<span data-ttu-id="e3ea0-106">Pokud chcete vytvořit víc virtuálních počítačů z přizpůsobené disku, měli byste vytvořit bitovou kopii z virtuálního počítače nebo virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-106">If you want to create multiple VMs from your customized disk, you should create an image from your VM or VHD.</span></span> <span data-ttu-id="e3ea0-107">Další informace najdete v tématu [vytvořit vlastní image virtuálního počítače Azure pomocí rozhraní příkazového řádku](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="e3ea0-107">For more information, see [Create a custom image of an Azure VM using the CLI](tutorial-custom-images.md).</span></span>

<span data-ttu-id="e3ea0-108">Máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="e3ea0-108">You have two options:</span></span>
* [<span data-ttu-id="e3ea0-109">Nahrání virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="e3ea0-109">Upload a VHD</span></span>](#option-1-upload-a-specialized-vhd)
* [<span data-ttu-id="e3ea0-110">Zkopírujte existující virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="e3ea0-110">Copy an existing Azure VM</span></span>](#option-2-copy-an-existing-azure-vm)

## <a name="quick-commands"></a><span data-ttu-id="e3ea0-111">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="e3ea0-111">Quick commands</span></span>

<span data-ttu-id="e3ea0-112">Při vytváření nového virtuálního počítače pomocí [az virtuální počítač vytvořit](/cli/azure/vm#create) z přizpůsobené nebo specializované disku můžete **připojit** disku (– připojit disk operačního systému) místo zadávání vlastního nebo marketplace image (--bitové kopie).</span><span class="sxs-lookup"><span data-stu-id="e3ea0-112">When creating a new VM using [az vm create](/cli/azure/vm#create) from a customized or specialized disk you **attach** the disk (--attach-os-disk) instead of specifying a custom or marketplace image (--image).</span></span> <span data-ttu-id="e3ea0-113">Následující příklad vytvoří virtuální počítač s názvem *Můjvp* pomocí spravovaného disku s názvem *myManagedDisk* vytvořené z vaší vlastní virtuální pevný disk:</span><span class="sxs-lookup"><span data-stu-id="e3ea0-113">The following example creates a VM named *myVM* using the managed disk named *myManagedDisk* created from your customized VHD:</span></span>

```azurecli
az vm create --resource-group myResourceGroup --location eastus --name myVM \
   --os-type linux --attach-os-disk myManagedDisk
```

## <a name="requirements"></a><span data-ttu-id="e3ea0-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e3ea0-114">Requirements</span></span>
<span data-ttu-id="e3ea0-115">Chcete-li provést následující kroky, je třeba:</span><span class="sxs-lookup"><span data-stu-id="e3ea0-115">To complete the following steps, you need:</span></span>

* <span data-ttu-id="e3ea0-116">Virtuální počítač Linux, který je připravena pro použití v Azure.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-116">A Linux virtual machine that has been prepared for use in Azure.</span></span> <span data-ttu-id="e3ea0-117">[Příprava virtuálního počítače](#prepare-the-vm) tohoto článku popisuje jak najít distro konkrétní informace o instalaci Azure Linux Agent (příkaz waagent), který je nutný pro virtuální počítač fungovat správně v Azure a abyste mohli připojit se pomocí SSH.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-117">The [Prepare the VM](#prepare-the-vm) section of this article covers how to find distro specific information on installing the Azure Linux Agent (waagent) which is required for the VM to work properly in Azure and for you to be able to connect to it using SSH.</span></span>
* <span data-ttu-id="e3ea0-118">Soubor virtuálního pevného disku z existující [distribuce schválené pro Azure Linux](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (nebo v tématu [informace pro neschválené distribuce](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) na virtuální disk ve formátu virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-118">The VHD file from an existing [Azure-endorsed Linux distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (or see [information for non-endorsed distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) to a virtual disk in the VHD format.</span></span> <span data-ttu-id="e3ea0-119">Existuje několik nástrojů k vytvoření virtuálního počítače a virtuálního pevného disku:</span><span class="sxs-lookup"><span data-stu-id="e3ea0-119">Multiple tools exist to create a VM and VHD:</span></span>
  * <span data-ttu-id="e3ea0-120">Instalace a konfigurace [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) nebo [KVM](http://www.linux-kvm.org/page/RunningKVM), aby byl používáte formát bitové kopie virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-120">Install and configure [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) or [KVM](http://www.linux-kvm.org/page/RunningKVM), taking care to use VHD as your image format.</span></span> <span data-ttu-id="e3ea0-121">V případě potřeby můžete [převést bitovou kopii](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) pomocí **qemu img převést**.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-121">If needed, you can [convert an image](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) using **qemu-img convert**.</span></span>
  * <span data-ttu-id="e3ea0-122">Můžete také použít technologie Hyper-V [ve Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) nebo [v systému Windows Server 2012 nebo 2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="e3ea0-122">You can also use Hyper-V [on Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) or [on Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="e3ea0-123">Novější formát VHDX není podporovaný v Azure.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-123">The newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="e3ea0-124">Když vytvoříte virtuální počítač, zadejte jako formát virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-124">When you create a VM, specify VHD as the format.</span></span> <span data-ttu-id="e3ea0-125">V případě potřeby můžete převést disků VHDX virtuální pevný disk pomocí [qemu img převést](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) nebo [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-125">If needed, you can convert VHDX disks to VHD using [qemu-img convert](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) or the [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="e3ea0-126">Navíc Azure nepodporuje odesílání dynamické virtuální pevné disky, takže je nutné převést tyto disky na virtuální pevné disky statické před nahráním.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-126">Further, Azure does not support uploading dynamic VHDs, so you need to convert such disks to static VHDs before uploading.</span></span> <span data-ttu-id="e3ea0-127">Můžete použít nástroje, jako [nástroje Azure virtuálního pevného disku pro přejděte](https://github.com/Microsoft/azure-vhd-utils-for-go) převést dynamické disky během procesu odesílání do Azure.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-127">You can use tools such as [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) to convert dynamic disks during the process of uploading to Azure.</span></span>
> 
> 


* <span data-ttu-id="e3ea0-128">Ujistěte se, že máte nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="e3ea0-128">Make sure that you have the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="e3ea0-129">V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-129">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="e3ea0-130">Názvy parametrů příklad zahrnuté *myResourceGroup*, *můj_účet_úložiště*, a *mydisks*.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-130">Example parameter names included *myResourceGroup*, *mystorageaccount*, and *mydisks*.</span></span>

<span data-ttu-id="e3ea0-131"><a id="prepimage"> </a></span><span class="sxs-lookup"><span data-stu-id="e3ea0-131"><a id="prepimage"> </a></span></span>

## <a name="prepare-the-vm"></a><span data-ttu-id="e3ea0-132">Příprava virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e3ea0-132">Prepare the VM</span></span>

<span data-ttu-id="e3ea0-133">Azure podporuje různé Linuxových distribucích (viz [distribuce schválené](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span><span class="sxs-lookup"><span data-stu-id="e3ea0-133">Azure supports various Linux distributions (see [Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span></span> <span data-ttu-id="e3ea0-134">Postup přípravy různé distribuce systému Linux, které jsou podporovány v Azure vás provede v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="e3ea0-134">The following articles guide you through how to prepare the various Linux distributions that are supported on Azure:</span></span>

* [<span data-ttu-id="e3ea0-135">Na základě centOS distribuce</span><span class="sxs-lookup"><span data-stu-id="e3ea0-135">CentOS-based Distributions</span></span>](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="e3ea0-136">Debian Linux</span><span class="sxs-lookup"><span data-stu-id="e3ea0-136">Debian Linux</span></span>](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="e3ea0-137">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="e3ea0-137">Oracle Linux</span></span>](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="e3ea0-138">Red Hat Enterprise Linux</span><span class="sxs-lookup"><span data-stu-id="e3ea0-138">Red Hat Enterprise Linux</span></span>](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="e3ea0-139">SLES & openSUSE</span><span class="sxs-lookup"><span data-stu-id="e3ea0-139">SLES & openSUSE</span></span>](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="e3ea0-140">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="e3ea0-140">Ubuntu</span></span>](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="e3ea0-141">Další - neschválené distribuce</span><span class="sxs-lookup"><span data-stu-id="e3ea0-141">Other - Non-Endorsed Distributions</span></span>](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

<span data-ttu-id="e3ea0-142">Viz také [poznámky k instalaci Linux](create-upload-generic.md#general-linux-installation-notes) Příprava bitové kopie systému Linux na Azure další Obecné tipy pro.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-142">Also see the [Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more general tips on preparing Linux images for Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="e3ea0-143">[Platformy Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) se vztahují na virtuální počítače se systémem Linux jenom v případě, že jeden z potvrzená distribuce se používá s podrobností konfigurace uvedeného v části "podporované verze [Linux na Azure-Endorsed distribuce](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e3ea0-143">The [Azure platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) applies to VMs running Linux only when one of the endorsed distributions is used with the configuration details as specified under 'Supported Versions' in [Linux on Azure-Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
> 

## <a name="option-1-upload-a-vhd"></a><span data-ttu-id="e3ea0-144">Možnost 1: Nahrání virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="e3ea0-144">Option 1: Upload a VHD</span></span>

<span data-ttu-id="e3ea0-145">Můžete nahrát vlastní virtuální pevný disk, ke které máte spuštěné v místním počítači nebo který jste exportovali z jiného cloudu.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-145">You can upload a customized VHD that you have running on a local machine or that you exported from another cloud.</span></span> <span data-ttu-id="e3ea0-146">Chcete-li použít virtuální pevný disk k vytvoření nového virtuálního počítače Azure, potřebujete nahrání virtuálního pevného disku do účtu úložiště a vytvoření spravovaného disku z virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-146">To use the VHD to create a new Azure VM, you need to upload the VHD to a storage account and create a managed disk from the VHD.</span></span> 

### <a name="create-a-resource-group"></a><span data-ttu-id="e3ea0-147">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="e3ea0-147">Create a resource group</span></span>

<span data-ttu-id="e3ea0-148">Před nahráním váš vlastní disk a vytváření virtuálních počítačů, musíte nejprve vytvořit skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="e3ea0-148">Before uploading your custom disk and creating VMs, you first need to create a resource group with [az group create](/cli/azure/group#create).</span></span>

<span data-ttu-id="e3ea0-149">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v *eastus* umístění: [disky spravované Azure – přehled](../windows/managed-disks-overview.md)</span><span class="sxs-lookup"><span data-stu-id="e3ea0-149">The following example creates a resource group named *myResourceGroup* in the *eastus* location: [Azure Managed Disks overview](../windows/managed-disks-overview.md)</span></span>
```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

### <a name="create-a-storage-account"></a><span data-ttu-id="e3ea0-150">vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="e3ea0-150">Create a storage account</span></span>

<span data-ttu-id="e3ea0-151">Vytvoření účtu úložiště vlastní disk a virtuální počítače s [vytvořit účet úložiště az](/cli/azure/storage/account#create).</span><span class="sxs-lookup"><span data-stu-id="e3ea0-151">Create a storage account for your custom disk and VMs with [az storage account create](/cli/azure/storage/account#create).</span></span> 

<span data-ttu-id="e3ea0-152">Následující příklad vytvoří účet úložiště s názvem *můj_účet_úložiště* ve skupině prostředků vytvořili:</span><span class="sxs-lookup"><span data-stu-id="e3ea0-152">The following example creates a storage account named *mystorageaccount* in the resource group previously created:</span></span>

```azurecli
az storage account create \
    --resource-group myResourceGroup \
    --location eastus \
    --name mystorageaccount \
    --kind Storage \
    --sku Standard_LRS
```

### <a name="list-storage-account-keys"></a><span data-ttu-id="e3ea0-153">Vypsat klíče účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="e3ea0-153">List storage account keys</span></span>
<span data-ttu-id="e3ea0-154">Vygeneruje Azure dva 512bitové přístupové klíče pro každý účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-154">Azure generates two 512-bit access keys for each storage account.</span></span> <span data-ttu-id="e3ea0-155">Tyto přístupové klíče se používají při ověřování k účtu úložiště, jako je provádění operace zápisu.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-155">These access keys are used when authenticating to the storage account, like carrying out write operations.</span></span> <span data-ttu-id="e3ea0-156">Další informace o [Správa přístupu k úložišti zde](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="e3ea0-156">Read more about [managing access to storage here](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span> <span data-ttu-id="e3ea0-157">Zobrazit přístupové klíče s [seznam klíčů účtu úložiště az](/cli/azure/storage/account/keys#list).</span><span class="sxs-lookup"><span data-stu-id="e3ea0-157">You view the access keys with [az storage account keys list](/cli/azure/storage/account/keys#list).</span></span>

<span data-ttu-id="e3ea0-158">Zobrazte přístupové klíče pro účet úložiště, který jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="e3ea0-158">View the access keys for the storage account you created:</span></span>

```azurecli
az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount
```

<span data-ttu-id="e3ea0-159">Výstup je podobný tomuto:</span><span class="sxs-lookup"><span data-stu-id="e3ea0-159">The output is similar to:</span></span>

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK
```
<span data-ttu-id="e3ea0-160">Poznamenejte si **key1** jako použije k interakci se svým účtem úložiště v dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-160">Make a note of **key1** as you will use it to interact with your storage account in the next steps.</span></span>

### <a name="create-a-storage-container"></a><span data-ttu-id="e3ea0-161">Vytvoření kontejneru úložiště</span><span class="sxs-lookup"><span data-stu-id="e3ea0-161">Create a storage container</span></span>
<span data-ttu-id="e3ea0-162">V stejným způsobem, který vytvoříte různé adresáře, které logicky uspořádat do místního systému souborů můžete vytvořit kontejnery v účtu úložiště pro uspořádání vaše disky.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-162">In the same way that you create different directories to logically organize your local file system, you create containers within a storage account to organize your disks.</span></span> <span data-ttu-id="e3ea0-163">Účet úložiště může obsahovat libovolný počet kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-163">A storage account can contain any number of containers.</span></span> <span data-ttu-id="e3ea0-164">Vytvořit kontejner s [vytvořit kontejner úložiště az](/cli/azure/storage/container#create).</span><span class="sxs-lookup"><span data-stu-id="e3ea0-164">Create a container with [az storage container create](/cli/azure/storage/container#create).</span></span>

<span data-ttu-id="e3ea0-165">Následující příklad vytvoří kontejner s názvem *mydisks*:</span><span class="sxs-lookup"><span data-stu-id="e3ea0-165">The following example creates a container named *mydisks*:</span></span>

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

### <a name="upload-the-vhd"></a><span data-ttu-id="e3ea0-166">Nahrání virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="e3ea0-166">Upload the VHD</span></span>
<span data-ttu-id="e3ea0-167">Teď nahrát vlastní disk s [az úložiště objektů blob nahrávání](/cli/azure/storage/blob#upload).</span><span class="sxs-lookup"><span data-stu-id="e3ea0-167">Now upload your custom disk with [az storage blob upload](/cli/azure/storage/blob#upload).</span></span> <span data-ttu-id="e3ea0-168">Nahrání a ukládat vlastní disk jako objekt blob stránky.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-168">You upload and store your custom disk as a page blob.</span></span>

<span data-ttu-id="e3ea0-169">Zadejte přístupový klíč, kontejner, který jste vytvořili v předchozím kroku a pak cestu k vlastní disku v místním počítači:</span><span class="sxs-lookup"><span data-stu-id="e3ea0-169">Specify your access key, the container you created in the previous step, and then the path to the custom disk on your local computer:</span></span>

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 \
    --container-name mydisks \
    --type page \
    --file /path/to/disk/mydisk.vhd \
    --name myDisk.vhd
```
<span data-ttu-id="e3ea0-170">Nahrání virtuálního pevného disku, může chvíli trvat.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-170">Uploading the VHD may take a while.</span></span>

### <a name="create-a-managed-disk"></a><span data-ttu-id="e3ea0-171">Vytvoření spravovaného disku</span><span class="sxs-lookup"><span data-stu-id="e3ea0-171">Create a managed disk</span></span>


<span data-ttu-id="e3ea0-172">Vytvoření spravovaného disku z virtuálního pevného disku pomocí [vytvoření disku az](/cli/azure/disk#create).</span><span class="sxs-lookup"><span data-stu-id="e3ea0-172">Create a managed disk from the VHD using [az disk create](/cli/azure/disk#create).</span></span> <span data-ttu-id="e3ea0-173">Následující příklad vytvoří spravované disk s názvem *myManagedDisk* z virtuálního pevného disku, který jste nahráli k účtu s názvem úložiště a kontejneru:</span><span class="sxs-lookup"><span data-stu-id="e3ea0-173">The following example creates a managed disk named *myManagedDisk* from the VHD you uploaded to your named storage account and container:</span></span>

```azurecli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
  --source https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd
```
## <a name="option-2-copy-an-existing-vm"></a><span data-ttu-id="e3ea0-174">Možnost 2: Zkopírujte existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="e3ea0-174">Option 2: Copy an existing VM</span></span>

<span data-ttu-id="e3ea0-175">Můžete také vytvořit vlastní virtuální počítač v Azure a pak zkopírujte disk operačního systému a připojte ji k vytvoření nového virtuálního počítače vytvořit další kopii.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-175">You can also create the customized VM in Azure and then copy the OS disk and attach it to a new VM to create another copy.</span></span> <span data-ttu-id="e3ea0-176">To je v pořádku pro testování, ale pokud chcete použít existující virtuální počítač Azure jako model pro více nové virtuální počítače, ve skutečnosti měli byste vytvořit **image** místo.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-176">This is fine for testing, but if you want to use an existing Azure VM as the model for multiple new VMs, you really should create an **image** instead.</span></span> <span data-ttu-id="e3ea0-177">Další informace o vytvoření bitové kopie ze stávajícího virtuálního počítače Azure najdete v tématu [vytvořit vlastní image virtuálního počítače Azure pomocí rozhraní příkazového řádku](tutorial-custom-images.md)</span><span class="sxs-lookup"><span data-stu-id="e3ea0-177">For more information about creating an image from an existing Azure VM, see [Create a custom image of an Azure VM using the CLI](tutorial-custom-images.md)</span></span>

### <a name="create-a-snapshot"></a><span data-ttu-id="e3ea0-178">Vytvoření snímku</span><span class="sxs-lookup"><span data-stu-id="e3ea0-178">Create a snapshot</span></span>

<span data-ttu-id="e3ea0-179">Tento příklad vytvoří snímek virtuálního počítače s názvem *Můjvp* ve skupině prostředků *myResourceGroup* a vytvoří snímek s názvem *osDiskSnapshot*.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-179">This example creates a snapshot of a VM named *myVM* in resource group *myResourceGroup* and creates a snapshot named *osDiskSnapshot*.</span></span>

```azure-cli
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDiskSnapshot
```
###  <a name="create-the-managed-disk"></a><span data-ttu-id="e3ea0-180">Vytvoření spravovaného disku</span><span class="sxs-lookup"><span data-stu-id="e3ea0-180">Create the managed disk</span></span>

<span data-ttu-id="e3ea0-181">Vytvoření nového spravovaného disku ze snímku.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-181">Create a new managed disk from the snapshot.</span></span>

<span data-ttu-id="e3ea0-182">Získáte Identifikátor snímku.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-182">Get the ID of the snapshot.</span></span> <span data-ttu-id="e3ea0-183">V tomto příkladu je název snímku *osDiskSnapshot* a je v *myResourceGroup* skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-183">In this example, the snapshot is named *osDiskSnapshot* and it is in the *myResourceGroup* resource group.</span></span>

```azure-cli
snapshotId=$(az snapshot show --name osDiskSnapshot --resource-group myResourceGroup --query [id] -o tsv)
```

<span data-ttu-id="e3ea0-184">Vytvoření spravovaného disku.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-184">Create the managed disk.</span></span> <span data-ttu-id="e3ea0-185">V tomto příkladu vytvoříme spravované disk s názvem *myManagedDisk* ze naše snímek, který je 128 GB velikost ve standardní úložiště.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-185">In this example, we will create a managed disk named *myManagedDisk* from our snapshot, that is 128GB in size in standard storage.</span></span>

```azure-cli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
    --sku Standard_LRS \
    --size-gb 128 \
    --source $snapshotId
```

## <a name="create-the-vm"></a><span data-ttu-id="e3ea0-186">Vytvořte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-186">Create the VM</span></span>

<span data-ttu-id="e3ea0-187">Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#create) a připojte (--připojit disk operačního systému) spravovaných disk jako disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-187">Now, create your VM with [az vm create](/cli/azure/vm#create) and attach (--attach-os-disk) the managed disk as the OS disk.</span></span> <span data-ttu-id="e3ea0-188">Následující příklad vytvoří virtuální počítač s názvem *myNewVM* pomocí spravovaného disku vytvořené z vaší nahraný virtuální pevný disk:</span><span class="sxs-lookup"><span data-stu-id="e3ea0-188">The following example creates a VM named *myNewVM* using the managed disk created from your uploaded VHD:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNewVM \
    --os-type linux \
    --attach-os-disk myManagedDisk
```

<span data-ttu-id="e3ea0-189">Nyní byste měli mít k SSH do virtuálního počítače pomocí přihlašovacích údajů, ze zdrojového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-189">You should be able to SSH into the VM using the credentials from the source VM.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="e3ea0-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e3ea0-190">Next steps</span></span>
<span data-ttu-id="e3ea0-191">Po připravené a nahrát vlastní virtuální disk, si můžete přečíst více o [pomocí Resource Manageru a šablony](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e3ea0-191">After you have prepared and uploaded your custom virtual disk, you can read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="e3ea0-192">Můžete také chtít [přidat datový disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) na nové virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="e3ea0-192">You may also want to [add a data disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) to your new VMs.</span></span> <span data-ttu-id="e3ea0-193">Pokud máte aplikace spuštěné na vaše virtuální počítače, které potřebujete získat přístup, je nutné [otevřete porty a koncové body](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e3ea0-193">If you have applications running on your VMs that you need to access, be sure to [open ports and endpoints](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

