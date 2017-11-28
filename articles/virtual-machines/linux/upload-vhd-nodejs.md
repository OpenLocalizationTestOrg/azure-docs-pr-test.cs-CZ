---
title: "aaaUpload vlastní image Linux s 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Vytvoření a nahrání virtuálního pevného disku (VHD) tooAzure s vlastní image Linux pomocí modelu nasazení Resource Manager hello a hello Azure CLI 1.0."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a8c7818f-eb65-409e-aa91-ce5ae975c564
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/10/2016
ms.author: iainfou
ms.openlocfilehash: 0bbd232debee1e632233d1c010e388dbc1548bf3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-image-by-using-hello-azure-cli-10"></a><span data-ttu-id="b40be-103">Nahrání a vytvoření virtuálního počítače s Linuxem z image vlastní disku pomocí hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="b40be-103">Upload and create a Linux VM from custom disk image by using hello Azure CLI 1.0</span></span>
<span data-ttu-id="b40be-104">Tento článek ukazuje, jak tooupload tooAzure virtuální pevný disk (VHD) pomocí hello modelu nasazení Resource Manager a vytvořit virtuální počítače s Linuxem z této vlastní image.</span><span class="sxs-lookup"><span data-stu-id="b40be-104">This article shows you how tooupload a virtual hard disk (VHD) tooAzure using hello Resource Manager deployment model and create Linux VMs from this custom image.</span></span> <span data-ttu-id="b40be-105">Tato funkce vám umožní tooinstall a konfigurovat požadavky na tooyour distro Linux a pak použít tento virtuální pevný disk tooquickly vytvoření Azure virtuálních počítačů (VM).</span><span class="sxs-lookup"><span data-stu-id="b40be-105">This functionality allows you tooinstall and configure a Linux distro tooyour requirements and then use that VHD tooquickly create Azure virtual machines (VMs).</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="b40be-106">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="b40be-106">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="b40be-107">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="b40be-107">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="b40be-108">[Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="b40be-108">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="b40be-109">[Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="b40be-109">[Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="b40be-110">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="b40be-110">Quick commands</span></span>
<span data-ttu-id="b40be-111">Pokud je třeba tooquickly dosáhnout hello, následující části Podrobnosti hello hello základní příkazy tooupload tooAzure virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b40be-111">If you need tooquickly accomplish hello task, hello following section details hello base commands tooupload a VM tooAzure.</span></span> <span data-ttu-id="b40be-112">Podrobnější informace a kontext pro každý krok naleznete hello zbytek dokumentu hello [od zde](#requirements).</span><span class="sxs-lookup"><span data-stu-id="b40be-112">More detailed information and context for each step can be found hello rest of hello document, [starting here](#requirements).</span></span>

<span data-ttu-id="b40be-113">Ujistěte se, že máte [hello Azure CLI 1.0](../../cli-install-nodejs.md) přihlášení a použití režimu Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="b40be-113">Make sure that you have [hello Azure CLI 1.0](../../cli-install-nodejs.md) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="b40be-114">Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="b40be-114">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="b40be-115">Názvy parametrů příklad zahrnuté `myResourceGroup`, `mystorageaccount`, a `myimages`.</span><span class="sxs-lookup"><span data-stu-id="b40be-115">Example parameter names included `myResourceGroup`, `mystorageaccount`, and `myimages`.</span></span>

<span data-ttu-id="b40be-116">Nejprve vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="b40be-116">First, create a resource group.</span></span> <span data-ttu-id="b40be-117">Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `WestUs` umístění:</span><span class="sxs-lookup"><span data-stu-id="b40be-117">hello following example creates a resource group named `myResourceGroup` in hello `WestUs` location:</span></span>

```azurecli
azure group create myResourceGroup --location "WestUS"
```

<span data-ttu-id="b40be-118">Vytvořte virtuální disky toohold účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="b40be-118">Create a storage account toohold your virtual disks.</span></span> <span data-ttu-id="b40be-119">Hello následující příklad vytvoří účet úložiště s názvem `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="b40be-119">hello following example creates a storage account named `mystorageaccount`:</span></span>

```azurecli
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

<span data-ttu-id="b40be-120">Zobrazí seznam hello přístupové klíče pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="b40be-120">List hello access keys for your storage account.</span></span> <span data-ttu-id="b40be-121">Poznamenejte si `key1`:</span><span class="sxs-lookup"><span data-stu-id="b40be-121">Make a note of `key1`:</span></span>

```azurecli
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

<span data-ttu-id="b40be-122">Vytvořte kontejner v rámci účtu úložiště pomocí klíče hello úložiště, který jste obdrželi.</span><span class="sxs-lookup"><span data-stu-id="b40be-122">Create a container within your storage account using hello storage key you obtained.</span></span> <span data-ttu-id="b40be-123">Hello následující příklad vytvoří kontejner s názvem `myimages` pomocí hello úložiště klíčů hodnoty z `key1`:</span><span class="sxs-lookup"><span data-stu-id="b40be-123">hello following example creates a container named `myimages` using hello storage key value from `key1`:</span></span>

```azurecli
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

<span data-ttu-id="b40be-124">Nakonec nahrajte váš kontejner toohello virtuálního pevného disku, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="b40be-124">Finally, upload your VHD toohello container you created.</span></span> <span data-ttu-id="b40be-125">Zadejte místní cestu tooyour hello virtuální pevný disk v části `/path/to/disk/mydisk.vhd`:</span><span class="sxs-lookup"><span data-stu-id="b40be-125">Specify hello local path tooyour VHD under `/path/to/disk/mydisk.vhd`:</span></span>

```azurecli
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

<span data-ttu-id="b40be-126">Nyní můžete vytvořit virtuální počítač z nahraný virtuální disk [pomocí šablony Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd).</span><span class="sxs-lookup"><span data-stu-id="b40be-126">You can now create a VM from your uploaded virtual disk [using a Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd).</span></span> <span data-ttu-id="b40be-127">Můžete také použít hello rozhraní příkazového řádku zadáním hello URI tooyour disku (`--image-urn`).</span><span class="sxs-lookup"><span data-stu-id="b40be-127">You can also use hello CLI by specifying hello URI tooyour disk (`--image-urn`).</span></span> <span data-ttu-id="b40be-128">Hello následující příklad vytvoří virtuální počítač s názvem `myVM` pomocí hello předtím nahrála virtuální disk:</span><span class="sxs-lookup"><span data-stu-id="b40be-128">hello following example creates a VM named `myVM` using hello virtual disk previously uploaded:</span></span>

```azurecli
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
```

<span data-ttu-id="b40be-129">Hello cílový účet úložiště obsahuje toobe hello stejné jako kde jste nahráli virtuální disk.</span><span class="sxs-lookup"><span data-stu-id="b40be-129">hello destination storage account has toobe hello same as where you uploaded your virtual disk to.</span></span> <span data-ttu-id="b40be-130">Můžete také potřebovat toospecify nebo zodpovědět vyzve k zadání, všechny hello další parametry, které vyžadují hello `azure vm create` příkaz například virtuální sítě, veřejnou IP adresu, uživatelské jméno a klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="b40be-130">You also need toospecify, or answer prompts for, all hello additional parameters required by hello `azure vm create` command such as virtual network, public IP address, username, and SSH keys.</span></span> <span data-ttu-id="b40be-131">Další informace o hello [dostupné parametry příkazového řádku Resource Manager](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="b40be-131">You can read more about hello [available CLI Resource Manager parameters](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span></span>

## <a name="requirements"></a><span data-ttu-id="b40be-132">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b40be-132">Requirements</span></span>
<span data-ttu-id="b40be-133">toocomplete hello následující kroky, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="b40be-133">toocomplete hello following steps, you need:</span></span>

* <span data-ttu-id="b40be-134">**Operační systém Linux nainstalován v souboru VHD** -nainstalovat [distribuce schválené pro Azure Linux](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (nebo v tématu [informace pro neschválené distribuce](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtuálního disku v hello virtuálního pevného disku formát.</span><span class="sxs-lookup"><span data-stu-id="b40be-134">**Linux operating system installed in a .vhd file** - Install an [Azure-endorsed Linux distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (or see [information for non-endorsed distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtual disk in hello VHD format.</span></span> <span data-ttu-id="b40be-135">Několik nástrojů existují toocreate virtuálního počítače a virtuálního pevného disku:</span><span class="sxs-lookup"><span data-stu-id="b40be-135">Multiple tools exist toocreate a VM and VHD:</span></span>
  * <span data-ttu-id="b40be-136">Instalace a konfigurace [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) nebo [KVM](http://www.linux-kvm.org/page/RunningKVM), dbejte na to toouse virtuálního pevného disku jako formát obrázku.</span><span class="sxs-lookup"><span data-stu-id="b40be-136">Install and configure [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) or [KVM](http://www.linux-kvm.org/page/RunningKVM), taking care toouse VHD as your image format.</span></span> <span data-ttu-id="b40be-137">V případě potřeby můžete [převést bitovou kopii](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) pomocí `qemu-img convert`.</span><span class="sxs-lookup"><span data-stu-id="b40be-137">If needed, you can [convert an image](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) using `qemu-img convert`.</span></span>
  * <span data-ttu-id="b40be-138">Můžete také použít technologie Hyper-V [ve Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) nebo [v systému Windows Server 2012 nebo 2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="b40be-138">You can also use Hyper-V [on Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) or [on Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="b40be-139">Hello novější formát VHDX není podporovaný v Azure.</span><span class="sxs-lookup"><span data-stu-id="b40be-139">hello newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="b40be-140">Když vytvoříte virtuální počítač, zadejte hello Formát virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="b40be-140">When you create a VM, specify VHD as hello format.</span></span> <span data-ttu-id="b40be-141">V případě potřeby můžete převést pomocí tooVHD disky VHDX [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) nebo hello [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b40be-141">If needed, you can convert VHDX disks tooVHD using [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) or hello [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="b40be-142">Navíc Azure nepodporuje odesílání dynamické virtuální pevné disky, takže je třeba tooconvert takové toostatic disky VHD před nahráním.</span><span class="sxs-lookup"><span data-stu-id="b40be-142">Further, Azure does not support uploading dynamic VHDs, so you need tooconvert such disks toostatic VHDs before uploading.</span></span> <span data-ttu-id="b40be-143">Můžete použít nástroje, jako [nástroje Azure virtuálního pevného disku pro přejděte](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert během procesu hello odesílání tooAzure dynamické disky.</span><span class="sxs-lookup"><span data-stu-id="b40be-143">You can use tools such as [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert dynamic disks during hello process of uploading tooAzure.</span></span>
> 
> 

* <span data-ttu-id="b40be-144">Virtuální počítače vytvořené pomocí vlastní image se musí nacházet v hello stejný účet úložiště jako samotný obraz hello</span><span class="sxs-lookup"><span data-stu-id="b40be-144">VMs created from your custom image must reside in hello same storage account as hello image itself</span></span>
  * <span data-ttu-id="b40be-145">Vytvoření úložiště účet a kontejner toohold vlastní image a vytvořené virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="b40be-145">Create a storage account and container toohold both your custom image and created VMs</span></span>
  * <span data-ttu-id="b40be-146">Po vytvoření všechny virtuální počítače, můžete bezpečně odstranit image</span><span class="sxs-lookup"><span data-stu-id="b40be-146">After you have created all your VMs, you can safely delete your image</span></span>

<span data-ttu-id="b40be-147">Ujistěte se, že máte [hello Azure CLI 1.0](../../cli-install-nodejs.md) přihlášení a použití režimu Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="b40be-147">Make sure that you have [hello Azure CLI 1.0](../../cli-install-nodejs.md) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="b40be-148">Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="b40be-148">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="b40be-149">Názvy parametrů příklad zahrnuté `myResourceGroup`, `mystorageaccount`, a `myimages`.</span><span class="sxs-lookup"><span data-stu-id="b40be-149">Example parameter names included `myResourceGroup`, `mystorageaccount`, and `myimages`.</span></span>

<span data-ttu-id="b40be-150"><a id="prepimage"> </a></span><span class="sxs-lookup"><span data-stu-id="b40be-150"><a id="prepimage"> </a></span></span>

## <a name="prepare-hello-image-toobe-uploaded"></a><span data-ttu-id="b40be-151">Příprava toobe hello image nahrát</span><span class="sxs-lookup"><span data-stu-id="b40be-151">Prepare hello image toobe uploaded</span></span>
<span data-ttu-id="b40be-152">Azure podporuje různé Linuxových distribucích (viz [distribuce schválené](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span><span class="sxs-lookup"><span data-stu-id="b40be-152">Azure supports various Linux distributions (see [Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span></span> <span data-ttu-id="b40be-153">Následující články Hello provede jak tooprepare hello různé distribuce systému Linux, které jsou podporovány v Azure:</span><span class="sxs-lookup"><span data-stu-id="b40be-153">hello following articles guide you through how tooprepare hello various Linux distributions that are supported on Azure:</span></span>

* <span data-ttu-id="b40be-154">**[Na základě centOS distribuce](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="b40be-154">**[CentOS-based Distributions](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="b40be-155">**[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="b40be-155">**[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="b40be-156">**[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="b40be-156">**[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="b40be-157">**[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="b40be-157">**[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="b40be-158">**[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="b40be-158">**[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="b40be-159">**[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="b40be-159">**[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="b40be-160">**[Další - neschválené distribuce](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="b40be-160">**[Other - Non-Endorsed Distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>

<span data-ttu-id="b40be-161">Viz také hello  **[poznámky k instalaci Linux](create-upload-generic.md#general-linux-installation-notes)**  Příprava bitové kopie systému Linux na Azure další Obecné tipy pro.</span><span class="sxs-lookup"><span data-stu-id="b40be-161">Also see hello **[Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes)** for more general tips on preparing Linux images for Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="b40be-162">Hello [platformy Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) platí tooVMs běží Linux, jen pokud jeden z hello distribuce schválené se používá s podrobnosti konfigurace hello uvedeného v části "podporované verze v [Linux na schválené pro Azure Distribuce](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b40be-162">hello [Azure platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) applies tooVMs running Linux only when one of hello endorsed distributions is used with hello configuration details as specified under 'Supported Versions' in [Linux on Azure-Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


## <a name="create-a-resource-group"></a><span data-ttu-id="b40be-163">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="b40be-163">Create a resource group</span></span>
<span data-ttu-id="b40be-164">Skupiny prostředků logicky seskupit všechny prostředky Azure toosupport hello virtuálních počítačů, jako je například hello virtuální sítě a úložiště.</span><span class="sxs-lookup"><span data-stu-id="b40be-164">Resource groups logically bring together all hello Azure resources toosupport your virtual machines, such as hello virtual networking and storage.</span></span> <span data-ttu-id="b40be-165">Další informace o [skupin prostředků Azure zde](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b40be-165">Read more about [Azure resource groups here](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="b40be-166">Před nahráním image vlastní disku a vytváření virtuálních počítačů, je nutné nejprve toocreate skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="b40be-166">Before uploading your custom disk image and creating VMs, you first need toocreate a resource group.</span></span> 

<span data-ttu-id="b40be-167">Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `WestUS` umístění:</span><span class="sxs-lookup"><span data-stu-id="b40be-167">hello following example creates a resource group named `myResourceGroup` in hello `WestUS` location:</span></span>

```azurecli
azure group create myResourceGroup --location "WestUS"
```

## <a name="create-a-storage-account"></a><span data-ttu-id="b40be-168">vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="b40be-168">Create a storage account</span></span>
<span data-ttu-id="b40be-169">Virtuální počítače jsou uloženy jako objekty BLOB stránky v rámci účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="b40be-169">VMs are stored as page blobs within a storage account.</span></span> <span data-ttu-id="b40be-170">Další informace o [zde úložiště objektů blob Azure](../../storage/common/storage-introduction.md#blob-storage).</span><span class="sxs-lookup"><span data-stu-id="b40be-170">Read more about [Azure blob storage here](../../storage/common/storage-introduction.md#blob-storage).</span></span> <span data-ttu-id="b40be-171">Můžete vytvořit účet úložiště pro image vlastní disku a virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="b40be-171">You create a storage account for your custom disk image and VMs.</span></span> <span data-ttu-id="b40be-172">Všechny virtuální počítače, které vytvoříte z vaší vlastní disku image nutné toobe v hello stejný účet úložiště jako této bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="b40be-172">Any VMs that you create from your custom disk image need toobe in hello same storage account as that image.</span></span>

<span data-ttu-id="b40be-173">Hello následující příklad vytvoří účet úložiště s názvem `mystorageaccount` ve skupině prostředků hello vytvořili:</span><span class="sxs-lookup"><span data-stu-id="b40be-173">hello following example creates a storage account named `mystorageaccount` in hello resource group previously created:</span></span>

```azurecli
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

## <a name="list-storage-account-keys"></a><span data-ttu-id="b40be-174">Vypsat klíče účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="b40be-174">List storage account keys</span></span>
<span data-ttu-id="b40be-175">Vygeneruje Azure dva 512bitové přístupové klíče pro každý účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="b40be-175">Azure generates two 512-bit access keys for each storage account.</span></span> <span data-ttu-id="b40be-176">Tyto přístupové klíče se používají při ověřování účtu úložiště toohello, jako je například toocarry se operace zápisu.</span><span class="sxs-lookup"><span data-stu-id="b40be-176">These access keys are used when authenticating toohello storage account, such as toocarry out write operations.</span></span> <span data-ttu-id="b40be-177">Další informace o [Správa přístup toostorage zde](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="b40be-177">Read more about [managing access toostorage here](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span> <span data-ttu-id="b40be-178">Můžete zobrazit přístupové klíče s hello `azure storage account keys list` příkaz.</span><span class="sxs-lookup"><span data-stu-id="b40be-178">You can view access keys with hello `azure storage account keys list` command.</span></span>

<span data-ttu-id="b40be-179">Zobrazit hello přístupové klíče pro účet úložiště hello, kterou jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="b40be-179">View hello access keys for hello storage account you created:</span></span>

```azurecli
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

<span data-ttu-id="b40be-180">výstup Hello je podobná:</span><span class="sxs-lookup"><span data-stu-id="b40be-180">hello output is similar to:</span></span>

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK

```
<span data-ttu-id="b40be-181">Poznamenejte si `key1` jak ji budete používat toointeract se svým účtem úložiště v dalších krocích hello.</span><span class="sxs-lookup"><span data-stu-id="b40be-181">Make a note of `key1` as you will use it toointeract with your storage account in hello next steps.</span></span>

## <a name="create-a-storage-container"></a><span data-ttu-id="b40be-182">Vytvoření kontejneru úložiště</span><span class="sxs-lookup"><span data-stu-id="b40be-182">Create a storage container</span></span>
<span data-ttu-id="b40be-183">V hello stejným způsobem, abyste vytvořili různé adresáře toologically uspořádat do místního systému souborů, vytvoření kontejnerů v rámci účtu tooorganize úložiště virtuálních disků a bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="b40be-183">In hello same way that you create different directories toologically organize your local file system, you create containers within a storage account tooorganize your virtual disks and images.</span></span> <span data-ttu-id="b40be-184">Účet úložiště může obsahovat libovolný počet kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="b40be-184">A storage account can contain any number of containers.</span></span> 

<span data-ttu-id="b40be-185">Hello následující příklad vytvoří kontejner s názvem `myimages`, zadání hello přístupový klíč získaných v předchozím kroku hello (`key1`):</span><span class="sxs-lookup"><span data-stu-id="b40be-185">hello following example creates a container named `myimages`, specifying hello access key obtained in hello previous step (`key1`):</span></span>

```azurecli
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

## <a name="upload-vhd"></a><span data-ttu-id="b40be-186">Nahrání virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="b40be-186">Upload VHD</span></span>
<span data-ttu-id="b40be-187">Nyní můžete ve skutečnosti nahrát vlastní disku image.</span><span class="sxs-lookup"><span data-stu-id="b40be-187">Now you can actually upload your custom disk image.</span></span> <span data-ttu-id="b40be-188">S všechny virtuální disky, které jsou používány virtuálními počítači, můžete nahrát a uložení bitové kopie disku vlastní jako objekt blob stránky.</span><span class="sxs-lookup"><span data-stu-id="b40be-188">As with all virtual disks used by VMs, you upload and store your custom disk image as a page blob.</span></span>

<span data-ttu-id="b40be-189">Zadejte přístupový klíč, hello kontejneru, kterou jste vytvořili v předchozím kroku hello a pak hello image vlastní disku toohello cestu v místním počítači:</span><span class="sxs-lookup"><span data-stu-id="b40be-189">Specify your access key, hello container you created in hello previous step, and then hello path toohello custom disk image on your local computer:</span></span>

```azurecli
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

## <a name="create-vm-from-custom-image"></a><span data-ttu-id="b40be-190">Vytvoření virtuálního počítače z vlastní image</span><span class="sxs-lookup"><span data-stu-id="b40be-190">Create VM from custom image</span></span>
<span data-ttu-id="b40be-191">Když vytvoříte virtuální počítače z image vlastní disk, zadejte hello bitové kopie disku toohello identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="b40be-191">When you create VMs from your custom disk image, specify hello URI toohello disk image.</span></span> <span data-ttu-id="b40be-192">Ujistěte se, že hello cílového úložiště účet shoduje se uloží vlastní disku image.</span><span class="sxs-lookup"><span data-stu-id="b40be-192">Ensure that hello destination storage account matches where your custom disk image is stored.</span></span> <span data-ttu-id="b40be-193">Můžete vytvořit virtuální počítač pomocí šablony Azure CLI nebo Resource Manager JSON hello.</span><span class="sxs-lookup"><span data-stu-id="b40be-193">You can create your VM using hello Azure CLI or Resource Manager JSON template.</span></span>

### <a name="create-a-vm-using-hello-azure-cli"></a><span data-ttu-id="b40be-194">Vytvoření virtuálního počítače pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="b40be-194">Create a VM using hello Azure CLI</span></span>
<span data-ttu-id="b40be-195">Zadejte hello `--image-urn` parametr s hello `azure vm create` příkaz toopoint tooyour vlastní disku image.</span><span class="sxs-lookup"><span data-stu-id="b40be-195">You specify hello `--image-urn` parameter with hello `azure vm create` command toopoint tooyour custom disk image.</span></span> <span data-ttu-id="b40be-196">Ujistěte se, že `--storage-account-name` odpovídá hello účet úložiště, kde je uložena bitová kopie vaše vlastní disku.</span><span class="sxs-lookup"><span data-stu-id="b40be-196">Ensure that `--storage-account-name` matches hello storage account where your custom disk image is stored.</span></span> <span data-ttu-id="b40be-197">Nemáte toouse hello stejném kontejneru jako hello toostore bitové kopie disku vlastní virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="b40be-197">You do not have toouse hello same container as hello custom disk image toostore your VMs.</span></span> <span data-ttu-id="b40be-198">Zkontrolujte, zda toocreate žádné další kontejnery v hello stejným způsobem, jak hello dřívějších krocích před nahráním vaše vlastní disku Image.</span><span class="sxs-lookup"><span data-stu-id="b40be-198">Make sure toocreate any additional containers in hello same way as hello earlier steps before uploading your custom disk images.</span></span>

<span data-ttu-id="b40be-199">Hello následující příklad vytvoří virtuální počítač s názvem `myVM` z bitové kopie disku vlastní:</span><span class="sxs-lookup"><span data-stu-id="b40be-199">hello following example creates a VM named `myVM` from your custom disk image:</span></span>

```azurecli
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
    --storage-account-name mystorageaccount
```

<span data-ttu-id="b40be-200">Stále nutné toospecify, nebo se zodpovědět vyzve k zadání, všechny hello další parametry, které vyžadují hello `azure vm create` příkaz například virtuální sítě, veřejnou IP adresu, uživatelské jméno a klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="b40be-200">You still need toospecify, or answer prompts for, all hello additional parameters required by hello `azure vm create` command such as virtual network, public IP address, username, and SSH keys.</span></span> <span data-ttu-id="b40be-201">Další informace o hello [dostupné parametry příkazového řádku Resource Manager](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="b40be-201">Read more about hello [available CLI Resource Manager parameters](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span></span>

### <a name="create-a-vm-using-a-json-template"></a><span data-ttu-id="b40be-202">Vytvoření virtuálního počítače pomocí šablony JSON</span><span class="sxs-lookup"><span data-stu-id="b40be-202">Create a VM using a JSON template</span></span>
<span data-ttu-id="b40be-203">Šablony Azure Resource Manageru jsou soubory JavaScript Object Notation (JSON), které definují hello prostředí, které chcete toobuild.</span><span class="sxs-lookup"><span data-stu-id="b40be-203">Azure Resource Manager templates are JavaScript Object Notation (JSON) files that define hello environment you wish toobuild.</span></span> <span data-ttu-id="b40be-204">Hello šablony jsou rozděleny u zprostředkovatelů prostředků toodifferent například výpočetní nebo sítích.</span><span class="sxs-lookup"><span data-stu-id="b40be-204">hello templates are broken down in toodifferent resource providers such as compute or network.</span></span> <span data-ttu-id="b40be-205">Můžete použít existující šablony nebo napsat vlastní.</span><span class="sxs-lookup"><span data-stu-id="b40be-205">You can use existing templates or write your own.</span></span> <span data-ttu-id="b40be-206">Další informace o [pomocí Resource Manageru a šablony](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b40be-206">Read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="b40be-207">V rámci hello `Microsoft.Compute/virtualMachines` poskytovatele šablony, máte `storageProfile` uzlu, který obsahuje podrobnosti o hello konfiguraci pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b40be-207">Within hello `Microsoft.Compute/virtualMachines` provider of your template, you have a `storageProfile` node that contains hello configuration details for your VM.</span></span> <span data-ttu-id="b40be-208">tooedit Hello dva hlavní parametry jsou hello `image` a `vhd` identifikátory URI, který bod image vlastní disku tooyour a virtuální disk nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b40be-208">hello two main parameters tooedit are hello `image` and `vhd` URIs that point tooyour custom disk image and your new VM's virtual disk.</span></span> <span data-ttu-id="b40be-209">Následující Hello ukazuje příklad hello JSON pro použití image vlastní disku:</span><span class="sxs-lookup"><span data-stu-id="b40be-209">hello following shows an example of hello JSON for using a custom disk image:</span></span>

```json
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

<span data-ttu-id="b40be-210">Můžete použít [toocreate tento existující šablony virtuálního počítače z vlastní image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) nebo přečtěte si o [vlastních šablon Azure Resource Manager](../../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b40be-210">You can use [this existing template toocreate a VM from a custom image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) or read about [creating your own Azure Resource Manager templates](../../azure-resource-manager/resource-group-authoring-templates.md).</span></span> 

<span data-ttu-id="b40be-211">Jakmile máte nakonfigurované šablony, vytvoříte virtuální počítače pomocí hello `azure group deployment create` příkaz.</span><span class="sxs-lookup"><span data-stu-id="b40be-211">Once you have a template configured, you create your VMs using hello `azure group deployment create` command.</span></span> <span data-ttu-id="b40be-212">Zadejte hello URI šablony JSON s hello `--template-uri` parametr:</span><span class="sxs-lookup"><span data-stu-id="b40be-212">Specify hello URI of your JSON template with hello `--template-uri` parameter:</span></span>

```azurecli
azure group deployment create --resource-group myResourceGroup
    --template-uri https://uri.to.template/mytemplate.json
```

<span data-ttu-id="b40be-213">Pokud máte soubor JSON, který je uložený místně v počítači, můžete použít hello `--template-file` parametr místo:</span><span class="sxs-lookup"><span data-stu-id="b40be-213">If you have a JSON file stored locally on your computer, you can use hello `--template-file` parameter instead:</span></span>

```azurecli
azure group deployment create --resource-group myResourceGroup
    --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a><span data-ttu-id="b40be-214">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b40be-214">Next steps</span></span>
<span data-ttu-id="b40be-215">Po připravené a nahrát vlastní virtuální disk, si můžete přečíst více o [pomocí Resource Manageru a šablony](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b40be-215">After you have prepared and uploaded your custom virtual disk, you can read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="b40be-216">Můžete také příliš[přidat datový disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour nové virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="b40be-216">You may also want too[add a data disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour new VMs.</span></span> <span data-ttu-id="b40be-217">Pokud máte aplikace spuštěné na virtuálních počítačů, je nutné, aby tooaccess, nezapomeňte příliš[otevřete porty a koncové body](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b40be-217">If you have applications running on your VMs that you need tooaccess, be sure too[open ports and endpoints](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

