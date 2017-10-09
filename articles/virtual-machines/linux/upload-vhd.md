---
title: "aaaUpload nebo kopírování vlastní virtuální počítač s Linuxem pomocí Azure CLI 2.0 | Microsoft Docs"
description: "Nahrát, nebo můžete zkopírovat vlastní virtuální počítač pomocí modelu nasazení Resource Manager hello a hello 2.0 rozhraní příkazového řádku Azure"
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
ms.openlocfilehash: 79af897120a6ba7f4a427ba6c7d0c31b7b870bd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-vm-from-custom-disk-with-hello-azure-cli-20"></a><span data-ttu-id="5728f-103">Vytvoření virtuálního počítače s Linuxem z vlastní disk s hello 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="5728f-103">Create a Linux VM from custom disk with hello Azure CLI 2.0</span></span>

<!-- rename toocreate-vm-specialized -->

<span data-ttu-id="5728f-104">Tento článek ukazuje, jak tooupload přizpůsobené virtuální pevný disk (VHD) nebo zkopírovat existující virtuální pevný disk v Azure a vytvoření nových Linux virtuálních počítačů (VM) z vlastní disku hello.</span><span class="sxs-lookup"><span data-stu-id="5728f-104">This article shows you how tooupload a customized virtual hard disk (VHD) or copy a an existing VHD in Azure and create new Linux virtual machines (VMs) from hello custom disk.</span></span> <span data-ttu-id="5728f-105">Můžete instalovat a konfigurovat požadavky na tooyour distro Linux a pak použít tento virtuální pevný disk tooquickly vytvořit nový virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="5728f-105">You can install and configure a Linux distro tooyour requirements and then use that VHD tooquickly create a new Azure virtual machine.</span></span>

<span data-ttu-id="5728f-106">Pokud toocreate chcete z přizpůsobené disku víc virtuálních počítačů, měli byste vytvořit bitovou kopii z virtuálního počítače nebo virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="5728f-106">If you want toocreate multiple VMs from your customized disk, you should create an image from your VM or VHD.</span></span> <span data-ttu-id="5728f-107">Další informace najdete v tématu [vytvořit vlastní image virtuálního počítače Azure pomocí rozhraní příkazového řádku hello](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="5728f-107">For more information, see [Create a custom image of an Azure VM using hello CLI](tutorial-custom-images.md).</span></span>

<span data-ttu-id="5728f-108">Máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="5728f-108">You have two options:</span></span>
* [<span data-ttu-id="5728f-109">Nahrání virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="5728f-109">Upload a VHD</span></span>](#option-1-upload-a-specialized-vhd)
* [<span data-ttu-id="5728f-110">Zkopírujte existující virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="5728f-110">Copy an existing Azure VM</span></span>](#option-2-copy-an-existing-azure-vm)

## <a name="quick-commands"></a><span data-ttu-id="5728f-111">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="5728f-111">Quick commands</span></span>

<span data-ttu-id="5728f-112">Při vytváření nového virtuálního počítače pomocí [az virtuální počítač vytvořit](/cli/azure/vm#create) z přizpůsobené nebo specializované disku můžete **připojit** hello disku (– připojit disk operačního systému) místo zadávání vlastního nebo marketplace image (--bitové kopie).</span><span class="sxs-lookup"><span data-stu-id="5728f-112">When creating a new VM using [az vm create](/cli/azure/vm#create) from a customized or specialized disk you **attach** hello disk (--attach-os-disk) instead of specifying a custom or marketplace image (--image).</span></span> <span data-ttu-id="5728f-113">Hello následující příklad vytvoří virtuální počítač s názvem *Můjvp* pomocí hello spravovaného disku s názvem *myManagedDisk* vytvořené z vaší vlastní virtuální pevný disk:</span><span class="sxs-lookup"><span data-stu-id="5728f-113">hello following example creates a VM named *myVM* using hello managed disk named *myManagedDisk* created from your customized VHD:</span></span>

```azurecli
az vm create --resource-group myResourceGroup --location eastus --name myVM \
   --os-type linux --attach-os-disk myManagedDisk
```

## <a name="requirements"></a><span data-ttu-id="5728f-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5728f-114">Requirements</span></span>
<span data-ttu-id="5728f-115">toocomplete hello následující kroky, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="5728f-115">toocomplete hello following steps, you need:</span></span>

* <span data-ttu-id="5728f-116">Virtuální počítač Linux, který je připravena pro použití v Azure.</span><span class="sxs-lookup"><span data-stu-id="5728f-116">A Linux virtual machine that has been prepared for use in Azure.</span></span> <span data-ttu-id="5728f-117">Hello [hello Příprava virtuálního počítače](#prepare-the-vm) tohoto článku popisuje, jak toofind distro konkrétní informace o instalaci hello Azure Linux Agent (příkaz waagent), který je nutné pro virtuální počítač toowork hello správně v Azure a jste toobe možné tooconnect tooit pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="5728f-117">hello [Prepare hello VM](#prepare-the-vm) section of this article covers how toofind distro specific information on installing hello Azure Linux Agent (waagent) which is required for hello VM toowork properly in Azure and for you toobe able tooconnect tooit using SSH.</span></span>
* <span data-ttu-id="5728f-118">soubor VHD Hello z existující [distribuce schválené pro Azure Linux](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (nebo v tématu [informace pro neschválené distribuce](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtuální disk ve formátu virtuálního pevného disku hello.</span><span class="sxs-lookup"><span data-stu-id="5728f-118">hello VHD file from an existing [Azure-endorsed Linux distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (or see [information for non-endorsed distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtual disk in hello VHD format.</span></span> <span data-ttu-id="5728f-119">Několik nástrojů existují toocreate virtuálního počítače a virtuálního pevného disku:</span><span class="sxs-lookup"><span data-stu-id="5728f-119">Multiple tools exist toocreate a VM and VHD:</span></span>
  * <span data-ttu-id="5728f-120">Instalace a konfigurace [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) nebo [KVM](http://www.linux-kvm.org/page/RunningKVM), dbejte na to toouse virtuálního pevného disku jako formát obrázku.</span><span class="sxs-lookup"><span data-stu-id="5728f-120">Install and configure [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) or [KVM](http://www.linux-kvm.org/page/RunningKVM), taking care toouse VHD as your image format.</span></span> <span data-ttu-id="5728f-121">V případě potřeby můžete [převést bitovou kopii](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) pomocí **qemu img převést**.</span><span class="sxs-lookup"><span data-stu-id="5728f-121">If needed, you can [convert an image](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) using **qemu-img convert**.</span></span>
  * <span data-ttu-id="5728f-122">Můžete také použít technologie Hyper-V [ve Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) nebo [v systému Windows Server 2012 nebo 2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="5728f-122">You can also use Hyper-V [on Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) or [on Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="5728f-123">Hello novější formát VHDX není podporovaný v Azure.</span><span class="sxs-lookup"><span data-stu-id="5728f-123">hello newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="5728f-124">Když vytvoříte virtuální počítač, zadejte hello Formát virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="5728f-124">When you create a VM, specify VHD as hello format.</span></span> <span data-ttu-id="5728f-125">V případě potřeby můžete převést pomocí tooVHD disky VHDX [qemu img převést](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) nebo hello [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5728f-125">If needed, you can convert VHDX disks tooVHD using [qemu-img convert](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) or hello [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="5728f-126">Navíc Azure nepodporuje odesílání dynamické virtuální pevné disky, takže je třeba tooconvert takové toostatic disky VHD před nahráním.</span><span class="sxs-lookup"><span data-stu-id="5728f-126">Further, Azure does not support uploading dynamic VHDs, so you need tooconvert such disks toostatic VHDs before uploading.</span></span> <span data-ttu-id="5728f-127">Můžete použít nástroje, jako [nástroje Azure virtuálního pevného disku pro přejděte](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert během procesu hello odesílání tooAzure dynamické disky.</span><span class="sxs-lookup"><span data-stu-id="5728f-127">You can use tools such as [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert dynamic disks during hello process of uploading tooAzure.</span></span>
> 
> 


* <span data-ttu-id="5728f-128">Ujistěte se, že máte hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="5728f-128">Make sure that you have hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="5728f-129">Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="5728f-129">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="5728f-130">Názvy parametrů příklad zahrnuté *myResourceGroup*, *můj_účet_úložiště*, a *mydisks*.</span><span class="sxs-lookup"><span data-stu-id="5728f-130">Example parameter names included *myResourceGroup*, *mystorageaccount*, and *mydisks*.</span></span>

<span data-ttu-id="5728f-131"><a id="prepimage"> </a></span><span class="sxs-lookup"><span data-stu-id="5728f-131"><a id="prepimage"> </a></span></span>

## <a name="prepare-hello-vm"></a><span data-ttu-id="5728f-132">Příprava hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="5728f-132">Prepare hello VM</span></span>

<span data-ttu-id="5728f-133">Azure podporuje různé Linuxových distribucích (viz [distribuce schválené](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span><span class="sxs-lookup"><span data-stu-id="5728f-133">Azure supports various Linux distributions (see [Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span></span> <span data-ttu-id="5728f-134">Následující články Hello provede jak tooprepare hello různé distribuce systému Linux, které jsou podporovány v Azure:</span><span class="sxs-lookup"><span data-stu-id="5728f-134">hello following articles guide you through how tooprepare hello various Linux distributions that are supported on Azure:</span></span>

* [<span data-ttu-id="5728f-135">Na základě centOS distribuce</span><span class="sxs-lookup"><span data-stu-id="5728f-135">CentOS-based Distributions</span></span>](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="5728f-136">Debian Linux</span><span class="sxs-lookup"><span data-stu-id="5728f-136">Debian Linux</span></span>](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="5728f-137">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="5728f-137">Oracle Linux</span></span>](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="5728f-138">Red Hat Enterprise Linux</span><span class="sxs-lookup"><span data-stu-id="5728f-138">Red Hat Enterprise Linux</span></span>](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="5728f-139">SLES & openSUSE</span><span class="sxs-lookup"><span data-stu-id="5728f-139">SLES & openSUSE</span></span>](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="5728f-140">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="5728f-140">Ubuntu</span></span>](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="5728f-141">Další - neschválené distribuce</span><span class="sxs-lookup"><span data-stu-id="5728f-141">Other - Non-Endorsed Distributions</span></span>](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

<span data-ttu-id="5728f-142">Viz také hello [poznámky k instalaci Linux](create-upload-generic.md#general-linux-installation-notes) Příprava bitové kopie systému Linux na Azure další Obecné tipy pro.</span><span class="sxs-lookup"><span data-stu-id="5728f-142">Also see hello [Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more general tips on preparing Linux images for Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="5728f-143">Hello [platformy Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) platí tooVMs běží Linux, jen pokud jeden z hello distribuce schválené se používá s podrobnosti konfigurace hello uvedeného v části "podporované verze v [Linux na schválené pro Azure Distribuce](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5728f-143">hello [Azure platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) applies tooVMs running Linux only when one of hello endorsed distributions is used with hello configuration details as specified under 'Supported Versions' in [Linux on Azure-Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
> 

## <a name="option-1-upload-a-vhd"></a><span data-ttu-id="5728f-144">Možnost 1: Nahrání virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="5728f-144">Option 1: Upload a VHD</span></span>

<span data-ttu-id="5728f-145">Můžete nahrát vlastní virtuální pevný disk, ke které máte spuštěné v místním počítači nebo který jste exportovali z jiného cloudu.</span><span class="sxs-lookup"><span data-stu-id="5728f-145">You can upload a customized VHD that you have running on a local machine or that you exported from another cloud.</span></span> <span data-ttu-id="5728f-146">toouse hello virtuálního pevného disku toocreate nový virtuální počítač Azure, budete potřebovat tooupload hello virtuálního pevného disku tooa úložiště účtu a vytvoření spravovaného disku ze hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="5728f-146">toouse hello VHD toocreate a new Azure VM, you need tooupload hello VHD tooa storage account and create a managed disk from hello VHD.</span></span> 

### <a name="create-a-resource-group"></a><span data-ttu-id="5728f-147">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="5728f-147">Create a resource group</span></span>

<span data-ttu-id="5728f-148">Před nahráním váš vlastní disk a vytváření virtuálních počítačů, musíte nejprve toocreate skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="5728f-148">Before uploading your custom disk and creating VMs, you first need toocreate a resource group with [az group create](/cli/azure/group#create).</span></span>

<span data-ttu-id="5728f-149">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění: [disky spravované Azure – přehled](../windows/managed-disks-overview.md)</span><span class="sxs-lookup"><span data-stu-id="5728f-149">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location: [Azure Managed Disks overview](../windows/managed-disks-overview.md)</span></span>
```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

### <a name="create-a-storage-account"></a><span data-ttu-id="5728f-150">vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="5728f-150">Create a storage account</span></span>

<span data-ttu-id="5728f-151">Vytvoření účtu úložiště vlastní disk a virtuální počítače s [vytvořit účet úložiště az](/cli/azure/storage/account#create).</span><span class="sxs-lookup"><span data-stu-id="5728f-151">Create a storage account for your custom disk and VMs with [az storage account create](/cli/azure/storage/account#create).</span></span> 

<span data-ttu-id="5728f-152">Hello následující příklad vytvoří účet úložiště s názvem *můj_účet_úložiště* ve skupině prostředků hello vytvořili:</span><span class="sxs-lookup"><span data-stu-id="5728f-152">hello following example creates a storage account named *mystorageaccount* in hello resource group previously created:</span></span>

```azurecli
az storage account create \
    --resource-group myResourceGroup \
    --location eastus \
    --name mystorageaccount \
    --kind Storage \
    --sku Standard_LRS
```

### <a name="list-storage-account-keys"></a><span data-ttu-id="5728f-153">Vypsat klíče účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="5728f-153">List storage account keys</span></span>
<span data-ttu-id="5728f-154">Vygeneruje Azure dva 512bitové přístupové klíče pro každý účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="5728f-154">Azure generates two 512-bit access keys for each storage account.</span></span> <span data-ttu-id="5728f-155">Tyto přístupové klíče se používají při ověřování účtu úložiště toohello, jako je provádění operace zápisu.</span><span class="sxs-lookup"><span data-stu-id="5728f-155">These access keys are used when authenticating toohello storage account, like carrying out write operations.</span></span> <span data-ttu-id="5728f-156">Další informace o [Správa přístup toostorage zde](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="5728f-156">Read more about [managing access toostorage here](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span> <span data-ttu-id="5728f-157">Zobrazit přístupové klíče hello s [seznam klíčů účtu úložiště az](/cli/azure/storage/account/keys#list).</span><span class="sxs-lookup"><span data-stu-id="5728f-157">You view hello access keys with [az storage account keys list](/cli/azure/storage/account/keys#list).</span></span>

<span data-ttu-id="5728f-158">Zobrazit hello přístupové klíče pro účet úložiště hello, kterou jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="5728f-158">View hello access keys for hello storage account you created:</span></span>

```azurecli
az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount
```

<span data-ttu-id="5728f-159">výstup Hello je podobná:</span><span class="sxs-lookup"><span data-stu-id="5728f-159">hello output is similar to:</span></span>

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK
```
<span data-ttu-id="5728f-160">Poznamenejte si **key1** jak ji budete používat toointeract se svým účtem úložiště v dalších krocích hello.</span><span class="sxs-lookup"><span data-stu-id="5728f-160">Make a note of **key1** as you will use it toointeract with your storage account in hello next steps.</span></span>

### <a name="create-a-storage-container"></a><span data-ttu-id="5728f-161">Vytvoření kontejneru úložiště</span><span class="sxs-lookup"><span data-stu-id="5728f-161">Create a storage container</span></span>
<span data-ttu-id="5728f-162">V hello stejným způsobem, abyste vytvořili různé adresáře toologically uspořádat do místního systému souborů, vytvoření kontejnerů v rámci tooorganize účet úložiště vaše disky.</span><span class="sxs-lookup"><span data-stu-id="5728f-162">In hello same way that you create different directories toologically organize your local file system, you create containers within a storage account tooorganize your disks.</span></span> <span data-ttu-id="5728f-163">Účet úložiště může obsahovat libovolný počet kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="5728f-163">A storage account can contain any number of containers.</span></span> <span data-ttu-id="5728f-164">Vytvořit kontejner s [vytvořit kontejner úložiště az](/cli/azure/storage/container#create).</span><span class="sxs-lookup"><span data-stu-id="5728f-164">Create a container with [az storage container create](/cli/azure/storage/container#create).</span></span>

<span data-ttu-id="5728f-165">Hello následující příklad vytvoří kontejner s názvem *mydisks*:</span><span class="sxs-lookup"><span data-stu-id="5728f-165">hello following example creates a container named *mydisks*:</span></span>

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

### <a name="upload-hello-vhd"></a><span data-ttu-id="5728f-166">Nahrát hello virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="5728f-166">Upload hello VHD</span></span>
<span data-ttu-id="5728f-167">Teď nahrát vlastní disk s [az úložiště objektů blob nahrávání](/cli/azure/storage/blob#upload).</span><span class="sxs-lookup"><span data-stu-id="5728f-167">Now upload your custom disk with [az storage blob upload](/cli/azure/storage/blob#upload).</span></span> <span data-ttu-id="5728f-168">Nahrání a ukládat vlastní disk jako objekt blob stránky.</span><span class="sxs-lookup"><span data-stu-id="5728f-168">You upload and store your custom disk as a page blob.</span></span>

<span data-ttu-id="5728f-169">Zadejte přístupový klíč, hello kontejneru, kterou jste vytvořili v předchozím kroku hello a pak hello cesta toohello vlastní disku v místním počítači:</span><span class="sxs-lookup"><span data-stu-id="5728f-169">Specify your access key, hello container you created in hello previous step, and then hello path toohello custom disk on your local computer:</span></span>

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 \
    --container-name mydisks \
    --type page \
    --file /path/to/disk/mydisk.vhd \
    --name myDisk.vhd
```
<span data-ttu-id="5728f-170">Odesílání hello virtuálního pevného disku může chvíli trvat.</span><span class="sxs-lookup"><span data-stu-id="5728f-170">Uploading hello VHD may take a while.</span></span>

### <a name="create-a-managed-disk"></a><span data-ttu-id="5728f-171">Vytvoření spravovaného disku</span><span class="sxs-lookup"><span data-stu-id="5728f-171">Create a managed disk</span></span>


<span data-ttu-id="5728f-172">Vytvoření spravovaného disku pomocí virtuálního pevného disku hello [vytvoření disku az](/cli/azure/disk#create).</span><span class="sxs-lookup"><span data-stu-id="5728f-172">Create a managed disk from hello VHD using [az disk create](/cli/azure/disk#create).</span></span> <span data-ttu-id="5728f-173">Hello následující příklad vytvoří spravované disk s názvem *myManagedDisk* z hello virtuálního pevného disku, který jste nahráli tooyour s názvem účtu úložiště a kontejneru:</span><span class="sxs-lookup"><span data-stu-id="5728f-173">hello following example creates a managed disk named *myManagedDisk* from hello VHD you uploaded tooyour named storage account and container:</span></span>

```azurecli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
  --source https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd
```
## <a name="option-2-copy-an-existing-vm"></a><span data-ttu-id="5728f-174">Možnost 2: Zkopírujte existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="5728f-174">Option 2: Copy an existing VM</span></span>

<span data-ttu-id="5728f-175">Můžete také vytvořit hello vlastní virtuální počítač v Azure a pak kopie disku hello operačního systému a jeho připojení tooa nový virtuální počítač toocreate jiná kopie.</span><span class="sxs-lookup"><span data-stu-id="5728f-175">You can also create hello customized VM in Azure and then copy hello OS disk and attach it tooa new VM toocreate another copy.</span></span> <span data-ttu-id="5728f-176">To je v pořádku pro testování, ale pokud chcete toouse existující virtuální počítač Azure jako hello model pro více nové virtuální počítače, ve skutečnosti měli byste vytvořit **image** místo.</span><span class="sxs-lookup"><span data-stu-id="5728f-176">This is fine for testing, but if you want toouse an existing Azure VM as hello model for multiple new VMs, you really should create an **image** instead.</span></span> <span data-ttu-id="5728f-177">Další informace o vytvoření bitové kopie ze stávajícího virtuálního počítače Azure najdete v tématu [vytvořit vlastní image virtuálního počítače Azure pomocí hello rozhraní příkazového řádku](tutorial-custom-images.md)</span><span class="sxs-lookup"><span data-stu-id="5728f-177">For more information about creating an image from an existing Azure VM, see [Create a custom image of an Azure VM using hello CLI](tutorial-custom-images.md)</span></span>

### <a name="create-a-snapshot"></a><span data-ttu-id="5728f-178">Vytvoření snímku</span><span class="sxs-lookup"><span data-stu-id="5728f-178">Create a snapshot</span></span>

<span data-ttu-id="5728f-179">Tento příklad vytvoří snímek virtuálního počítače s názvem *Můjvp* ve skupině prostředků *myResourceGroup* a vytvoří snímek s názvem *osDiskSnapshot*.</span><span class="sxs-lookup"><span data-stu-id="5728f-179">This example creates a snapshot of a VM named *myVM* in resource group *myResourceGroup* and creates a snapshot named *osDiskSnapshot*.</span></span>

```azure-cli
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDiskSnapshot
```
###  <a name="create-hello-managed-disk"></a><span data-ttu-id="5728f-180">Vytvoření spravovaného disku hello</span><span class="sxs-lookup"><span data-stu-id="5728f-180">Create hello managed disk</span></span>

<span data-ttu-id="5728f-181">Vytvoření nového spravovaného disku ze snímku hello.</span><span class="sxs-lookup"><span data-stu-id="5728f-181">Create a new managed disk from hello snapshot.</span></span>

<span data-ttu-id="5728f-182">Získání ID hello hello snímku.</span><span class="sxs-lookup"><span data-stu-id="5728f-182">Get hello ID of hello snapshot.</span></span> <span data-ttu-id="5728f-183">V tomto příkladu je název snímku hello *osDiskSnapshot* a je v hello *myResourceGroup* skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="5728f-183">In this example, hello snapshot is named *osDiskSnapshot* and it is in hello *myResourceGroup* resource group.</span></span>

```azure-cli
snapshotId=$(az snapshot show --name osDiskSnapshot --resource-group myResourceGroup --query [id] -o tsv)
```

<span data-ttu-id="5728f-184">Vytvoření hello spravovaného disku.</span><span class="sxs-lookup"><span data-stu-id="5728f-184">Create hello managed disk.</span></span> <span data-ttu-id="5728f-185">V tomto příkladu vytvoříme spravované disk s názvem *myManagedDisk* ze naše snímek, který je 128 GB velikost ve standardní úložiště.</span><span class="sxs-lookup"><span data-stu-id="5728f-185">In this example, we will create a managed disk named *myManagedDisk* from our snapshot, that is 128GB in size in standard storage.</span></span>

```azure-cli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
    --sku Standard_LRS \
    --size-gb 128 \
    --source $snapshotId
```

## <a name="create-hello-vm"></a><span data-ttu-id="5728f-186">Vytvoření hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="5728f-186">Create hello VM</span></span>

<span data-ttu-id="5728f-187">Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#create) a připojte (--připojit disk operačního systému) hello spravované disk jako disk hello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="5728f-187">Now, create your VM with [az vm create](/cli/azure/vm#create) and attach (--attach-os-disk) hello managed disk as hello OS disk.</span></span> <span data-ttu-id="5728f-188">Hello následující příklad vytvoří virtuální počítač s názvem *myNewVM* pomocí hello spravovaného disku vytvořené z vaší nahraný virtuální pevný disk:</span><span class="sxs-lookup"><span data-stu-id="5728f-188">hello following example creates a VM named *myNewVM* using hello managed disk created from your uploaded VHD:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNewVM \
    --os-type linux \
    --attach-os-disk myManagedDisk
```

<span data-ttu-id="5728f-189">Musí být schopný tooSSH do hello virtuálních počítačů pomocí přihlašovacích údajů hello z hello zdrojového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5728f-189">You should be able tooSSH into hello VM using hello credentials from hello source VM.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="5728f-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5728f-190">Next steps</span></span>
<span data-ttu-id="5728f-191">Po připravené a nahrát vlastní virtuální disk, si můžete přečíst více o [pomocí Resource Manageru a šablony](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5728f-191">After you have prepared and uploaded your custom virtual disk, you can read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="5728f-192">Můžete také příliš[přidat datový disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour nové virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="5728f-192">You may also want too[add a data disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour new VMs.</span></span> <span data-ttu-id="5728f-193">Pokud máte aplikace spuštěné na virtuálních počítačů, je nutné, aby tooaccess, nezapomeňte příliš[otevřete porty a koncové body](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5728f-193">If you have applications running on your VMs that you need tooaccess, be sure too[open ports and endpoints](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

