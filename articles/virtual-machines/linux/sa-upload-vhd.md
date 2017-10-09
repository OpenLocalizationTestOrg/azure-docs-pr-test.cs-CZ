---
title: "aaaUpload vlastní disk Linux s 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Vytvoření a nahrání virtuálního pevného disku (VHD) tooAzure, pomocí modelu nasazení Resource Manager hello a hello 2.0 rozhraní příkazového řádku Azure"
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
ms.openlocfilehash: cf8556ab4dfe6c4e5eff4e99fe1ddc44393f774c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-with-hello-azure-cli-20"></a><span data-ttu-id="066e2-103">Nahrání a vytvoření virtuálního počítače s Linuxem z vlastní disk s hello 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="066e2-103">Upload and create a Linux VM from custom disk with hello Azure CLI 2.0</span></span>
<span data-ttu-id="066e2-104">Tento článek ukazuje, jak tooupload virtuální pevný disk (VHD) tooan úložiště Azure účet s hello Azure CLI 2.0 a vytvořit virtuální počítače s Linuxem z tento vlastní disk.</span><span class="sxs-lookup"><span data-stu-id="066e2-104">This article shows you how tooupload a virtual hard disk (VHD) tooan Azure storage account with hello Azure CLI 2.0 and create Linux VMs from this custom disk.</span></span> <span data-ttu-id="066e2-105">Můžete také provést tyto kroky hello [Azure CLI 1.0](upload-vhd-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="066e2-105">You can also perform these steps with hello [Azure CLI 1.0](upload-vhd-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="066e2-106">Tato funkce vám umožní tooinstall a konfigurovat požadavky na tooyour distro Linux a pak použít tento virtuální pevný disk tooquickly vytvoření Azure virtuálních počítačů (VM).</span><span class="sxs-lookup"><span data-stu-id="066e2-106">This functionality allows you tooinstall and configure a Linux distro tooyour requirements and then use that VHD tooquickly create Azure virtual machines (VMs).</span></span>

<span data-ttu-id="066e2-107">Toto téma používá účty úložiště pro hello konečné virtuální pevné disky, ale můžete také provést tyto kroky, pomocí [discích spravovaných](upload-vhd.md).</span><span class="sxs-lookup"><span data-stu-id="066e2-107">This topic uses storage accounts for hello final VHDs, but you can also do these steps using [managed disks](upload-vhd.md).</span></span> 

## <a name="quick-commands"></a><span data-ttu-id="066e2-108">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="066e2-108">Quick commands</span></span>
<span data-ttu-id="066e2-109">Pokud je třeba tooquickly dosáhnout hello, následující části Podrobnosti hello hello základní příkazy tooupload tooAzure virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="066e2-109">If you need tooquickly accomplish hello task, hello following section details hello base commands tooupload a VHD tooAzure.</span></span> <span data-ttu-id="066e2-110">Podrobnější informace a kontext pro každý krok naleznete hello zbytek dokumentu hello [od zde](#requirements).</span><span class="sxs-lookup"><span data-stu-id="066e2-110">More detailed information and context for each step can be found hello rest of hello document, [starting here](#requirements).</span></span>

<span data-ttu-id="066e2-111">Ujistěte se, že máte hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="066e2-111">Make sure that you have hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="066e2-112">Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="066e2-112">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="066e2-113">Názvy parametrů příklad zahrnuté `myResourceGroup`, `mystorageaccount`, a `mydisks`.</span><span class="sxs-lookup"><span data-stu-id="066e2-113">Example parameter names included `myResourceGroup`, `mystorageaccount`, and `mydisks`.</span></span>

<span data-ttu-id="066e2-114">Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="066e2-114">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="066e2-115">Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `WestUs` umístění:</span><span class="sxs-lookup"><span data-stu-id="066e2-115">hello following example creates a resource group named `myResourceGroup` in hello `WestUs` location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="066e2-116">Vytvoření toohold účet úložiště virtuální disky s [vytvořit účet úložiště az](/cli/azure/storage/account#create).</span><span class="sxs-lookup"><span data-stu-id="066e2-116">Create a storage account toohold your virtual disks with [az storage account create](/cli/azure/storage/account#create).</span></span> <span data-ttu-id="066e2-117">Hello následující příklad vytvoří účet úložiště s názvem `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="066e2-117">hello following example creates a storage account named `mystorageaccount`:</span></span>

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

<span data-ttu-id="066e2-118">Seznam hello přístupové klíče pro účet úložiště s [seznam klíčů účtu úložiště az](/cli/azure/storage/account/keys#list).</span><span class="sxs-lookup"><span data-stu-id="066e2-118">List hello access keys for your storage account with [az storage account keys list](/cli/azure/storage/account/keys#list).</span></span> <span data-ttu-id="066e2-119">Poznamenejte si `key1`:</span><span class="sxs-lookup"><span data-stu-id="066e2-119">Make a note of `key1`:</span></span>

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
```

<span data-ttu-id="066e2-120">Vytvořit kontejner v rámci účtu úložiště pomocí klíč hello úložiště získat s [vytvořit kontejner úložiště az](/cli/azure/storage/container#create).</span><span class="sxs-lookup"><span data-stu-id="066e2-120">Create a container within your storage account using hello storage key you obtained with [az storage container create](/cli/azure/storage/container#create).</span></span> <span data-ttu-id="066e2-121">Hello následující příklad vytvoří kontejner s názvem `mydisks` pomocí hello úložiště klíčů hodnoty z `key1`:</span><span class="sxs-lookup"><span data-stu-id="066e2-121">hello following example creates a container named `mydisks` using hello storage key value from `key1`:</span></span>

```azurecli
az storage container create --account-name mystorageaccount \
    --account-key key1 --name mydisks
```

<span data-ttu-id="066e2-122">Nakonec nahrát váš kontejner toohello virtuálního pevného disku jste vytvořili pomocí [az úložiště objektů blob nahrávání](/cli/azure/storage/blob#upload).</span><span class="sxs-lookup"><span data-stu-id="066e2-122">Finally, upload your VHD toohello container you created with [az storage blob upload](/cli/azure/storage/blob#upload).</span></span> <span data-ttu-id="066e2-123">Zadejte místní cestu tooyour hello virtuální pevný disk v části `/path/to/disk/mydisk.vhd`:</span><span class="sxs-lookup"><span data-stu-id="066e2-123">Specify hello local path tooyour VHD under `/path/to/disk/mydisk.vhd`:</span></span>

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

<span data-ttu-id="066e2-124">Zadejte hello URI tooyour disk (`--image`) s [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="066e2-124">Specify hello URI tooyour disk (`--image`) with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="066e2-125">Hello následující příklad vytvoří virtuální počítač s názvem `myVM` pomocí hello předtím nahrála virtuální disk:</span><span class="sxs-lookup"><span data-stu-id="066e2-125">hello following example creates a VM named `myVM` using hello virtual disk previously uploaded:</span></span>

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisk/myDisks.vhd \
    --use-unmanaged-disk
```

<span data-ttu-id="066e2-126">Hello cílový účet úložiště obsahuje toobe hello stejné jako kde jste nahráli virtuální disk.</span><span class="sxs-lookup"><span data-stu-id="066e2-126">hello destination storage account has toobe hello same as where you uploaded your virtual disk to.</span></span> <span data-ttu-id="066e2-127">Můžete také potřebovat toospecify nebo zodpovědět vyzve k zadání, všechny hello další parametry, které vyžadují hello **vytvořit virtuální počítač az** příkaz například virtuální sítě, veřejnou IP adresu, uživatelské jméno a klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="066e2-127">You also need toospecify, or answer prompts for, all hello additional parameters required by hello **az vm create** command such as virtual network, public IP address, username, and SSH keys.</span></span> <span data-ttu-id="066e2-128">Další informace o hello [dostupné parametry příkazového řádku Resource Manager](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="066e2-128">You can read more about hello [available CLI Resource Manager parameters](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span></span>

## <a name="requirements"></a><span data-ttu-id="066e2-129">Požadavky</span><span class="sxs-lookup"><span data-stu-id="066e2-129">Requirements</span></span>
<span data-ttu-id="066e2-130">toocomplete hello následující kroky, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="066e2-130">toocomplete hello following steps, you need:</span></span>

* <span data-ttu-id="066e2-131">**Operační systém Linux nainstalován v souboru VHD** -nainstalovat [distribuce schválené pro Azure Linux](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (nebo v tématu [informace pro neschválené distribuce](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtuálního disku v hello virtuálního pevného disku formát.</span><span class="sxs-lookup"><span data-stu-id="066e2-131">**Linux operating system installed in a .vhd file** - Install an [Azure-endorsed Linux distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (or see [information for non-endorsed distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtual disk in hello VHD format.</span></span> <span data-ttu-id="066e2-132">Několik nástrojů existují toocreate virtuálního počítače a virtuálního pevného disku:</span><span class="sxs-lookup"><span data-stu-id="066e2-132">Multiple tools exist toocreate a VM and VHD:</span></span>
  * <span data-ttu-id="066e2-133">Instalace a konfigurace [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) nebo [KVM](http://www.linux-kvm.org/page/RunningKVM), dbejte na to toouse virtuálního pevného disku jako formát obrázku.</span><span class="sxs-lookup"><span data-stu-id="066e2-133">Install and configure [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) or [KVM](http://www.linux-kvm.org/page/RunningKVM), taking care toouse VHD as your image format.</span></span> <span data-ttu-id="066e2-134">V případě potřeby můžete [převést bitovou kopii](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) pomocí `qemu-img convert`.</span><span class="sxs-lookup"><span data-stu-id="066e2-134">If needed, you can [convert an image](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) using `qemu-img convert`.</span></span>
  * <span data-ttu-id="066e2-135">Můžete také použít technologie Hyper-V [ve Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) nebo [v systému Windows Server 2012 nebo 2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="066e2-135">You can also use Hyper-V [on Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) or [on Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="066e2-136">Hello novější formát VHDX není podporovaný v Azure.</span><span class="sxs-lookup"><span data-stu-id="066e2-136">hello newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="066e2-137">Když vytvoříte virtuální počítač, zadejte hello Formát virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="066e2-137">When you create a VM, specify VHD as hello format.</span></span> <span data-ttu-id="066e2-138">V případě potřeby můžete převést pomocí tooVHD disky VHDX [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) nebo hello [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="066e2-138">If needed, you can convert VHDX disks tooVHD using [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) or hello [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="066e2-139">Navíc Azure nepodporuje odesílání dynamické virtuální pevné disky, takže je třeba tooconvert takové toostatic disky VHD před nahráním.</span><span class="sxs-lookup"><span data-stu-id="066e2-139">Further, Azure does not support uploading dynamic VHDs, so you need tooconvert such disks toostatic VHDs before uploading.</span></span> <span data-ttu-id="066e2-140">Můžete použít nástroje, jako [nástroje Azure virtuálního pevného disku pro přejděte](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert během procesu hello odesílání tooAzure dynamické disky.</span><span class="sxs-lookup"><span data-stu-id="066e2-140">You can use tools such as [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert dynamic disks during hello process of uploading tooAzure.</span></span>
> 
> 

* <span data-ttu-id="066e2-141">Virtuální počítače vytvořené z vlastní disk se musí nacházet v hello stejný účet úložiště jako samotném disku hello</span><span class="sxs-lookup"><span data-stu-id="066e2-141">VMs created from your custom disk must reside in hello same storage account as hello disk itself</span></span>
  * <span data-ttu-id="066e2-142">Vytvoření úložiště účet a kontejner toohold vlastní disku a vytvořené virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="066e2-142">Create a storage account and container toohold both your custom disk and created VMs</span></span>
  * <span data-ttu-id="066e2-143">Po vytvoření všechny virtuální počítače, můžete bezpečně odstranit disk</span><span class="sxs-lookup"><span data-stu-id="066e2-143">After you have created all your VMs, you can safely delete your disk</span></span>

<span data-ttu-id="066e2-144">Ujistěte se, že máte hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="066e2-144">Make sure that you have hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="066e2-145">Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="066e2-145">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="066e2-146">Názvy parametrů příklad zahrnuté `myResourceGroup`, `mystorageaccount`, a `mydisks`.</span><span class="sxs-lookup"><span data-stu-id="066e2-146">Example parameter names included `myResourceGroup`, `mystorageaccount`, and `mydisks`.</span></span>

<span data-ttu-id="066e2-147"><a id="prepimage"> </a></span><span class="sxs-lookup"><span data-stu-id="066e2-147"><a id="prepimage"> </a></span></span>

## <a name="prepare-hello-disk-toobe-uploaded"></a><span data-ttu-id="066e2-148">Příprava toobe disku hello nahrát</span><span class="sxs-lookup"><span data-stu-id="066e2-148">Prepare hello disk toobe uploaded</span></span>
<span data-ttu-id="066e2-149">Azure podporuje různé Linuxových distribucích (viz [distribuce schválené](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span><span class="sxs-lookup"><span data-stu-id="066e2-149">Azure supports various Linux distributions (see [Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span></span> <span data-ttu-id="066e2-150">Následující články Hello provede jak tooprepare hello různé distribuce systému Linux, které jsou podporovány v Azure:</span><span class="sxs-lookup"><span data-stu-id="066e2-150">hello following articles guide you through how tooprepare hello various Linux distributions that are supported on Azure:</span></span>

* <span data-ttu-id="066e2-151">**[Na základě centOS distribuce](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="066e2-151">**[CentOS-based Distributions](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="066e2-152">**[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="066e2-152">**[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="066e2-153">**[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="066e2-153">**[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="066e2-154">**[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="066e2-154">**[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="066e2-155">**[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="066e2-155">**[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="066e2-156">**[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="066e2-156">**[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="066e2-157">**[Další - neschválené distribuce](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="066e2-157">**[Other - Non-Endorsed Distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>

<span data-ttu-id="066e2-158">Viz také hello  **[poznámky k instalaci Linux](create-upload-generic.md#general-linux-installation-notes)**  Příprava bitové kopie systému Linux na Azure další Obecné tipy pro.</span><span class="sxs-lookup"><span data-stu-id="066e2-158">Also see hello **[Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes)** for more general tips on preparing Linux images for Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="066e2-159">Hello [platformy Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) platí tooVMs běží Linux, jen pokud jeden z hello distribuce schválené se používá s podrobnosti konfigurace hello uvedeného v části "podporované verze v [Linux na schválené pro Azure Distribuce](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="066e2-159">hello [Azure platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) applies tooVMs running Linux only when one of hello endorsed distributions is used with hello configuration details as specified under 'Supported Versions' in [Linux on Azure-Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
> 

## <a name="create-a-resource-group"></a><span data-ttu-id="066e2-160">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="066e2-160">Create a resource group</span></span>
<span data-ttu-id="066e2-161">Skupiny prostředků logicky seskupit všechny prostředky Azure toosupport hello virtuálních počítačů, jako je například hello virtuální sítě a úložiště.</span><span class="sxs-lookup"><span data-stu-id="066e2-161">Resource groups logically bring together all hello Azure resources toosupport your virtual machines, such as hello virtual networking and storage.</span></span> <span data-ttu-id="066e2-162">Další informace o skupin prostředků, najdete v části [skupiny zdrojů přehled](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="066e2-162">For more information resource groups, see [resource groups overview](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="066e2-163">Před nahráním váš vlastní disk a vytváření virtuálních počítačů, musíte nejprve toocreate skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="066e2-163">Before uploading your custom disk and creating VMs, you first need toocreate a resource group with [az group create](/cli/azure/group#create).</span></span>

<span data-ttu-id="066e2-164">Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `westus` umístění:</span><span class="sxs-lookup"><span data-stu-id="066e2-164">hello following example creates a resource group named `myResourceGroup` in hello `westus` location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-a-storage-account"></a><span data-ttu-id="066e2-165">vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="066e2-165">Create a storage account</span></span>

<span data-ttu-id="066e2-166">Vytvoření účtu úložiště vlastní disk a virtuální počítače s [vytvořit účet úložiště az](/cli/azure/storage/account#create).</span><span class="sxs-lookup"><span data-stu-id="066e2-166">Create a storage account for your custom disk and VMs with [az storage account create](/cli/azure/storage/account#create).</span></span> <span data-ttu-id="066e2-167">Všechny virtuální počítače s nespravované disky, které vytvoříte z toobe třeba vaše vlastní disku v hello stejný účet úložiště jako disk.</span><span class="sxs-lookup"><span data-stu-id="066e2-167">Any VMs with unmanaged disks that you create from your custom disk need toobe in hello same storage account as that disk.</span></span> 

<span data-ttu-id="066e2-168">Hello následující příklad vytvoří účet úložiště s názvem `mystorageaccount` ve skupině prostředků hello vytvořili:</span><span class="sxs-lookup"><span data-stu-id="066e2-168">hello following example creates a storage account named `mystorageaccount` in hello resource group previously created:</span></span>

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

## <a name="list-storage-account-keys"></a><span data-ttu-id="066e2-169">Vypsat klíče účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="066e2-169">List storage account keys</span></span>
<span data-ttu-id="066e2-170">Vygeneruje Azure dva 512bitové přístupové klíče pro každý účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="066e2-170">Azure generates two 512-bit access keys for each storage account.</span></span> <span data-ttu-id="066e2-171">Tyto přístupové klíče se používají při ověřování účtu úložiště toohello, jako je například toocarry se operace zápisu.</span><span class="sxs-lookup"><span data-stu-id="066e2-171">These access keys are used when authenticating toohello storage account, such as toocarry out write operations.</span></span> <span data-ttu-id="066e2-172">Další informace o [Správa přístup toostorage zde](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="066e2-172">Read more about [managing access toostorage here](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span> <span data-ttu-id="066e2-173">Zobrazit přístupové klíče hello s [seznam klíčů účtu úložiště az](/cli/azure/storage/account/keys#list).</span><span class="sxs-lookup"><span data-stu-id="066e2-173">You view hello access keys with [az storage account keys list](/cli/azure/storage/account/keys#list).</span></span>

<span data-ttu-id="066e2-174">Zobrazit hello přístupové klíče pro účet úložiště hello, kterou jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="066e2-174">View hello access keys for hello storage account you created:</span></span>

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
```

<span data-ttu-id="066e2-175">výstup Hello je podobná:</span><span class="sxs-lookup"><span data-stu-id="066e2-175">hello output is similar to:</span></span>

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK
```
<span data-ttu-id="066e2-176">Poznamenejte si `key1` jak ji budete používat toointeract se svým účtem úložiště v dalších krocích hello.</span><span class="sxs-lookup"><span data-stu-id="066e2-176">Make a note of `key1` as you will use it toointeract with your storage account in hello next steps.</span></span>

## <a name="create-a-storage-container"></a><span data-ttu-id="066e2-177">Vytvoření kontejneru úložiště</span><span class="sxs-lookup"><span data-stu-id="066e2-177">Create a storage container</span></span>
<span data-ttu-id="066e2-178">V hello stejným způsobem, abyste vytvořili různé adresáře toologically uspořádat do místního systému souborů, vytvoření kontejnerů v rámci tooorganize účet úložiště vaše disky.</span><span class="sxs-lookup"><span data-stu-id="066e2-178">In hello same way that you create different directories toologically organize your local file system, you create containers within a storage account tooorganize your disks.</span></span> <span data-ttu-id="066e2-179">Účet úložiště může obsahovat libovolný počet kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="066e2-179">A storage account can contain any number of containers.</span></span> <span data-ttu-id="066e2-180">Vytvořit kontejner s [vytvořit kontejner úložiště az](/cli/azure/storage/container#create).</span><span class="sxs-lookup"><span data-stu-id="066e2-180">Create a container with [az storage container create](/cli/azure/storage/container#create).</span></span>

<span data-ttu-id="066e2-181">Hello následující příklad vytvoří kontejner s názvem `mydisks`:</span><span class="sxs-lookup"><span data-stu-id="066e2-181">hello following example creates a container named `mydisks`:</span></span>

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

## <a name="upload-vhd"></a><span data-ttu-id="066e2-182">Nahrání virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="066e2-182">Upload VHD</span></span>
<span data-ttu-id="066e2-183">Teď nahrát vlastní disk s [az úložiště objektů blob nahrávání](/cli/azure/storage/blob#upload).</span><span class="sxs-lookup"><span data-stu-id="066e2-183">Now upload your custom disk with [az storage blob upload](/cli/azure/storage/blob#upload).</span></span> <span data-ttu-id="066e2-184">Nahrání a ukládat vlastní disk jako objekt blob stránky.</span><span class="sxs-lookup"><span data-stu-id="066e2-184">You upload and store your custom disk as a page blob.</span></span>

<span data-ttu-id="066e2-185">Zadejte přístupový klíč, hello kontejneru, kterou jste vytvořili v předchozím kroku hello a pak hello cesta toohello vlastní disku v místním počítači:</span><span class="sxs-lookup"><span data-stu-id="066e2-185">Specify your access key, hello container you created in hello previous step, and then hello path toohello custom disk on your local computer:</span></span>

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

## <a name="create-hello-vm"></a><span data-ttu-id="066e2-186">Vytvoření hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="066e2-186">Create hello VM</span></span>
<span data-ttu-id="066e2-187">toocreate virtuální počítač s nespravované disky, zadejte hello URI tooyour disk (`--image`) s [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="066e2-187">toocreate a VM with unmanaged disks, specify hello URI tooyour disk (`--image`) with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="066e2-188">Hello následující příklad vytvoří virtuální počítač s názvem `myVM` pomocí hello předtím nahrála virtuální disk:</span><span class="sxs-lookup"><span data-stu-id="066e2-188">hello following example creates a VM named `myVM` using hello virtual disk previously uploaded:</span></span>

<span data-ttu-id="066e2-189">Zadejte hello `--image` parametr s [vytvořit virtuální počítač az](/cli/azure/vm#create) toopoint tooyour vlastní disku.</span><span class="sxs-lookup"><span data-stu-id="066e2-189">You specify hello `--image` parameter with [az vm create](/cli/azure/vm#create) toopoint tooyour custom disk.</span></span> <span data-ttu-id="066e2-190">Ujistěte se, že `--storage-account` odpovídá hello účet úložiště, kde je uložený váš vlastní disk.</span><span class="sxs-lookup"><span data-stu-id="066e2-190">Ensure that `--storage-account` matches hello storage account where your custom disk is stored.</span></span> <span data-ttu-id="066e2-191">Nemáte toouse hello stejném kontejneru jako hello vlastní disku toostore virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="066e2-191">You do not have toouse hello same container as hello custom disk toostore your VMs.</span></span> <span data-ttu-id="066e2-192">Zkontrolujte, zda toocreate žádné další kontejnery v hello stejným způsobem, jak hello dřívějších krocích před nahráním váš vlastní disk.</span><span class="sxs-lookup"><span data-stu-id="066e2-192">Make sure toocreate any additional containers in hello same way as hello earlier steps before uploading your custom disk.</span></span>

<span data-ttu-id="066e2-193">Hello následující příklad vytvoří virtuální počítač s názvem `myVM` z vlastní disku:</span><span class="sxs-lookup"><span data-stu-id="066e2-193">hello following example creates a VM named `myVM` from your custom disk:</span></span>

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd \
    --use-unmanaged-disk
```

<span data-ttu-id="066e2-194">Stále nutné toospecify, nebo se zodpovědět vyzve k zadání, všechny hello další parametry, které vyžadují hello **vytvořit virtuální počítač az** příkaz například uživatelské jméno a klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="066e2-194">You still need toospecify, or answer prompts for, all hello additional parameters required by hello **az vm create** command such as username and SSH keys.</span></span>


## <a name="resource-manager-template"></a><span data-ttu-id="066e2-195">Šablona Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="066e2-195">Resource Manager template</span></span>
<span data-ttu-id="066e2-196">Šablony Azure Resource Manageru jsou soubory JavaScript Object Notation (JSON), které definují hello prostředí, které chcete toobuild.</span><span class="sxs-lookup"><span data-stu-id="066e2-196">Azure Resource Manager templates are JavaScript Object Notation (JSON) files that define hello environment you wish toobuild.</span></span> <span data-ttu-id="066e2-197">Hello šablony jsou rozděleny u zprostředkovatelů prostředků toodifferent například výpočetní nebo sítích.</span><span class="sxs-lookup"><span data-stu-id="066e2-197">hello templates are broken down in toodifferent resource providers such as compute or network.</span></span> <span data-ttu-id="066e2-198">Můžete použít existující šablony nebo napsat vlastní.</span><span class="sxs-lookup"><span data-stu-id="066e2-198">You can use existing templates or write your own.</span></span> <span data-ttu-id="066e2-199">Další informace o [pomocí Resource Manageru a šablony](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="066e2-199">Read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="066e2-200">V rámci hello `Microsoft.Compute/virtualMachines` poskytovatele šablony, máte `storageProfile` uzlu, který obsahuje podrobnosti o hello konfiguraci pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="066e2-200">Within hello `Microsoft.Compute/virtualMachines` provider of your template, you have a `storageProfile` node that contains hello configuration details for your VM.</span></span> <span data-ttu-id="066e2-201">tooedit Hello dva hlavní parametry jsou hello `image` a `vhd` identifikátory URI, který bod vlastní tooyour a virtuální disk nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="066e2-201">hello two main parameters tooedit are hello `image` and `vhd` URIs that point tooyour custom disk and your new VM's virtual disk.</span></span> <span data-ttu-id="066e2-202">Následující Hello ukazuje příklad hello JSON pro použití vlastního disku:</span><span class="sxs-lookup"><span data-stu-id="066e2-202">hello following shows an example of hello JSON for using a custom disk:</span></span>

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

<span data-ttu-id="066e2-203">Můžete použít [toocreate tento existující šablony virtuálního počítače z vlastní image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) nebo přečtěte si o [vlastních šablon Azure Resource Manager](../../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="066e2-203">You can use [this existing template toocreate a VM from a custom image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) or read about [creating your own Azure Resource Manager templates](../../azure-resource-manager/resource-group-authoring-templates.md).</span></span> 

<span data-ttu-id="066e2-204">Jakmile máte šablonu nakonfigurována, použijte [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create) toocreate virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="066e2-204">Once you have a template configured, use [az group deployment create](/cli/azure/group/deployment#create) toocreate your VMs.</span></span> <span data-ttu-id="066e2-205">Zadejte hello URI šablony JSON s hello `--template-uri` parametr:</span><span class="sxs-lookup"><span data-stu-id="066e2-205">Specify hello URI of your JSON template with hello `--template-uri` parameter:</span></span>

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-uri https://uri.to.template/mytemplate.json
```

<span data-ttu-id="066e2-206">Pokud máte soubor JSON, který je uložený místně v počítači, můžete použít hello `--template-file` parametr místo:</span><span class="sxs-lookup"><span data-stu-id="066e2-206">If you have a JSON file stored locally on your computer, you can use hello `--template-file` parameter instead:</span></span>

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a><span data-ttu-id="066e2-207">Další kroky</span><span class="sxs-lookup"><span data-stu-id="066e2-207">Next steps</span></span>
<span data-ttu-id="066e2-208">Po připravené a nahrát vlastní virtuální disk, si můžete přečíst více o [pomocí Resource Manageru a šablony](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="066e2-208">After you have prepared and uploaded your custom virtual disk, you can read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="066e2-209">Můžete také příliš[přidat datový disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour nové virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="066e2-209">You may also want too[add a data disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour new VMs.</span></span> <span data-ttu-id="066e2-210">Pokud máte aplikace spuštěné na virtuálních počítačů, je nutné, aby tooaccess, nezapomeňte příliš[otevřete porty a koncové body](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="066e2-210">If you have applications running on your VMs that you need tooaccess, be sure too[open ports and endpoints](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

