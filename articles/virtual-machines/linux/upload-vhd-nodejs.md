---
title: "Nahrát vlastní image Linux s 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Vytvoření a nahrání virtuálního pevného disku (VHD) do Azure s vlastní image Linux pomocí modelu nasazení Resource Manager a Azure CLI 1.0."
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
ms.openlocfilehash: ca4c6cb9296028275b2b032af0c94baabeec1223
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-image-by-using-the-azure-cli-10"></a><span data-ttu-id="4570f-103">Nahrání a vytvoření virtuálního počítače s Linuxem z image vlastní disku pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="4570f-103">Upload and create a Linux VM from custom disk image by using the Azure CLI 1.0</span></span>
<span data-ttu-id="4570f-104">Tento článek ukazuje, jak nahrát virtuální pevný disk (VHD) do Azure pomocí modelu nasazení Resource Manager a vytvořit virtuální počítače s Linuxem z této vlastní image.</span><span class="sxs-lookup"><span data-stu-id="4570f-104">This article shows you how to upload a virtual hard disk (VHD) to Azure using the Resource Manager deployment model and create Linux VMs from this custom image.</span></span> <span data-ttu-id="4570f-105">Tato funkce umožňuje instalovat a konfigurovat Linux distro svých požadavků a použije tento virtuální pevný disk k rychlému vytvoření Azure virtuální počítače (VM).</span><span class="sxs-lookup"><span data-stu-id="4570f-105">This functionality allows you to install and configure a Linux distro to your requirements and then use that VHD to quickly create Azure virtual machines (VMs).</span></span>


## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="4570f-106">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="4570f-106">CLI versions to complete the task</span></span>
<span data-ttu-id="4570f-107">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="4570f-107">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="4570f-108">[Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="4570f-108">[Azure CLI 1.0](#quick-commands) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="4570f-109">[Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="4570f-109">[Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="4570f-110">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="4570f-110">Quick commands</span></span>
<span data-ttu-id="4570f-111">Pokud potřebujete rychle provedení úlohy, následující část podrobně popisuje základní příkazy k nahrání virtuálního počítače do Azure.</span><span class="sxs-lookup"><span data-stu-id="4570f-111">If you need to quickly accomplish the task, the following section details the base commands to upload a VM to Azure.</span></span> <span data-ttu-id="4570f-112">Podrobnější informace a kontext pro každý krok naleznete zbývající části dokumentu, [od zde](#requirements).</span><span class="sxs-lookup"><span data-stu-id="4570f-112">More detailed information and context for each step can be found the rest of the document, [starting here](#requirements).</span></span>

<span data-ttu-id="4570f-113">Ujistěte se, že máte [Azure CLI 1.0](../../cli-install-nodejs.md) přihlášení a použití režimu Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="4570f-113">Make sure that you have [the Azure CLI 1.0](../../cli-install-nodejs.md) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="4570f-114">V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4570f-114">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="4570f-115">Názvy parametrů příklad zahrnuté `myResourceGroup`, `mystorageaccount`, a `myimages`.</span><span class="sxs-lookup"><span data-stu-id="4570f-115">Example parameter names included `myResourceGroup`, `mystorageaccount`, and `myimages`.</span></span>

<span data-ttu-id="4570f-116">Nejprve vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="4570f-116">First, create a resource group.</span></span> <span data-ttu-id="4570f-117">Následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v `WestUs` umístění:</span><span class="sxs-lookup"><span data-stu-id="4570f-117">The following example creates a resource group named `myResourceGroup` in the `WestUs` location:</span></span>

```azurecli
azure group create myResourceGroup --location "WestUS"
```

<span data-ttu-id="4570f-118">Vytvořte účet úložiště pro virtuální disky.</span><span class="sxs-lookup"><span data-stu-id="4570f-118">Create a storage account to hold your virtual disks.</span></span> <span data-ttu-id="4570f-119">Následující příklad vytvoří účet úložiště s názvem `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="4570f-119">The following example creates a storage account named `mystorageaccount`:</span></span>

```azurecli
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

<span data-ttu-id="4570f-120">Zobrazí seznam přístupové klíče pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="4570f-120">List the access keys for your storage account.</span></span> <span data-ttu-id="4570f-121">Poznamenejte si `key1`:</span><span class="sxs-lookup"><span data-stu-id="4570f-121">Make a note of `key1`:</span></span>

```azurecli
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

<span data-ttu-id="4570f-122">Vytvořte kontejner v rámci účtu úložiště pomocí klíče úložiště, který jste obdrželi.</span><span class="sxs-lookup"><span data-stu-id="4570f-122">Create a container within your storage account using the storage key you obtained.</span></span> <span data-ttu-id="4570f-123">Následující příklad vytvoří kontejner s názvem `myimages` z úložiště klíčů hodnoty `key1`:</span><span class="sxs-lookup"><span data-stu-id="4570f-123">The following example creates a container named `myimages` using the storage key value from `key1`:</span></span>

```azurecli
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

<span data-ttu-id="4570f-124">Nakonec Nahrajte svůj disk VHD do kontejneru, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="4570f-124">Finally, upload your VHD to the container you created.</span></span> <span data-ttu-id="4570f-125">Zadejte místní cestu na vaše virtuální pevný disk v části `/path/to/disk/mydisk.vhd`:</span><span class="sxs-lookup"><span data-stu-id="4570f-125">Specify the local path to your VHD under `/path/to/disk/mydisk.vhd`:</span></span>

```azurecli
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

<span data-ttu-id="4570f-126">Nyní můžete vytvořit virtuální počítač z nahraný virtuální disk [pomocí šablony Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd).</span><span class="sxs-lookup"><span data-stu-id="4570f-126">You can now create a VM from your uploaded virtual disk [using a Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd).</span></span> <span data-ttu-id="4570f-127">Můžete také použít rozhraní příkazového řádku zadáním identifikátor URI na disk (`--image-urn`).</span><span class="sxs-lookup"><span data-stu-id="4570f-127">You can also use the CLI by specifying the URI to your disk (`--image-urn`).</span></span> <span data-ttu-id="4570f-128">Následující příklad vytvoří virtuální počítač s názvem `myVM` pomocí virtuálního disku předtím nahrála:</span><span class="sxs-lookup"><span data-stu-id="4570f-128">The following example creates a VM named `myVM` using the virtual disk previously uploaded:</span></span>

```azurecli
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
```

<span data-ttu-id="4570f-129">Cílový účet úložiště musí být stejný jako kde jste nahráli virtuální disk.</span><span class="sxs-lookup"><span data-stu-id="4570f-129">The destination storage account has to be the same as where you uploaded your virtual disk to.</span></span> <span data-ttu-id="4570f-130">Budete taky muset zadat nebo odpověď výzvy pro všechny další parametry, vyžaduje `azure vm create` příkaz například virtuální sítě, veřejnou IP adresu, uživatelské jméno a klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="4570f-130">You also need to specify, or answer prompts for, all the additional parameters required by the `azure vm create` command such as virtual network, public IP address, username, and SSH keys.</span></span> <span data-ttu-id="4570f-131">Další informace o [dostupné parametry příkazového řádku Resource Manager](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="4570f-131">You can read more about the [available CLI Resource Manager parameters](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span></span>

## <a name="requirements"></a><span data-ttu-id="4570f-132">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4570f-132">Requirements</span></span>
<span data-ttu-id="4570f-133">Chcete-li provést následující kroky, je třeba:</span><span class="sxs-lookup"><span data-stu-id="4570f-133">To complete the following steps, you need:</span></span>

* <span data-ttu-id="4570f-134">**Operační systém Linux nainstalován v souboru VHD** -nainstalovat [distribuce schválené pro Azure Linux](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (nebo v tématu [informace pro neschválené distribuce](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) na virtuální disk ve formátu virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="4570f-134">**Linux operating system installed in a .vhd file** - Install an [Azure-endorsed Linux distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (or see [information for non-endorsed distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) to a virtual disk in the VHD format.</span></span> <span data-ttu-id="4570f-135">Existuje několik nástrojů k vytvoření virtuálního počítače a virtuálního pevného disku:</span><span class="sxs-lookup"><span data-stu-id="4570f-135">Multiple tools exist to create a VM and VHD:</span></span>
  * <span data-ttu-id="4570f-136">Instalace a konfigurace [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) nebo [KVM](http://www.linux-kvm.org/page/RunningKVM), aby byl používáte formát bitové kopie virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="4570f-136">Install and configure [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) or [KVM](http://www.linux-kvm.org/page/RunningKVM), taking care to use VHD as your image format.</span></span> <span data-ttu-id="4570f-137">V případě potřeby můžete [převést bitovou kopii](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) pomocí `qemu-img convert`.</span><span class="sxs-lookup"><span data-stu-id="4570f-137">If needed, you can [convert an image](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) using `qemu-img convert`.</span></span>
  * <span data-ttu-id="4570f-138">Můžete také použít technologie Hyper-V [ve Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) nebo [v systému Windows Server 2012 nebo 2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="4570f-138">You can also use Hyper-V [on Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) or [on Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="4570f-139">Novější formát VHDX není podporovaný v Azure.</span><span class="sxs-lookup"><span data-stu-id="4570f-139">The newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="4570f-140">Když vytvoříte virtuální počítač, zadejte jako formát virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="4570f-140">When you create a VM, specify VHD as the format.</span></span> <span data-ttu-id="4570f-141">V případě potřeby můžete převést disků VHDX virtuální pevný disk pomocí [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) nebo [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4570f-141">If needed, you can convert VHDX disks to VHD using [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) or the [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="4570f-142">Navíc Azure nepodporuje odesílání dynamické virtuální pevné disky, takže je nutné převést tyto disky na virtuální pevné disky statické před nahráním.</span><span class="sxs-lookup"><span data-stu-id="4570f-142">Further, Azure does not support uploading dynamic VHDs, so you need to convert such disks to static VHDs before uploading.</span></span> <span data-ttu-id="4570f-143">Můžete použít nástroje, jako [nástroje Azure virtuálního pevného disku pro přejděte](https://github.com/Microsoft/azure-vhd-utils-for-go) převést dynamické disky během procesu odesílání do Azure.</span><span class="sxs-lookup"><span data-stu-id="4570f-143">You can use tools such as [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) to convert dynamic disks during the process of uploading to Azure.</span></span>
> 
> 

* <span data-ttu-id="4570f-144">Virtuální počítače vytvořené pomocí vlastní image se musí nacházet ve stejném účtu úložiště jako samotný obraz</span><span class="sxs-lookup"><span data-stu-id="4570f-144">VMs created from your custom image must reside in the same storage account as the image itself</span></span>
  * <span data-ttu-id="4570f-145">Vytvoření účtu úložiště a kontejner pro uložení vlastní image a vytvořené virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="4570f-145">Create a storage account and container to hold both your custom image and created VMs</span></span>
  * <span data-ttu-id="4570f-146">Po vytvoření všechny virtuální počítače, můžete bezpečně odstranit image</span><span class="sxs-lookup"><span data-stu-id="4570f-146">After you have created all your VMs, you can safely delete your image</span></span>

<span data-ttu-id="4570f-147">Ujistěte se, že máte [Azure CLI 1.0](../../cli-install-nodejs.md) přihlášení a použití režimu Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="4570f-147">Make sure that you have [the Azure CLI 1.0](../../cli-install-nodejs.md) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="4570f-148">V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4570f-148">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="4570f-149">Názvy parametrů příklad zahrnuté `myResourceGroup`, `mystorageaccount`, a `myimages`.</span><span class="sxs-lookup"><span data-stu-id="4570f-149">Example parameter names included `myResourceGroup`, `mystorageaccount`, and `myimages`.</span></span>

<span data-ttu-id="4570f-150"><a id="prepimage"> </a></span><span class="sxs-lookup"><span data-stu-id="4570f-150"><a id="prepimage"> </a></span></span>

## <a name="prepare-the-image-to-be-uploaded"></a><span data-ttu-id="4570f-151">Příprava bitové kopie k odeslání</span><span class="sxs-lookup"><span data-stu-id="4570f-151">Prepare the image to be uploaded</span></span>
<span data-ttu-id="4570f-152">Azure podporuje různé Linuxových distribucích (viz [distribuce schválené](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span><span class="sxs-lookup"><span data-stu-id="4570f-152">Azure supports various Linux distributions (see [Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span></span> <span data-ttu-id="4570f-153">Postup přípravy různé distribuce systému Linux, které jsou podporovány v Azure vás provede v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="4570f-153">The following articles guide you through how to prepare the various Linux distributions that are supported on Azure:</span></span>

* <span data-ttu-id="4570f-154">**[Na základě centOS distribuce](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="4570f-154">**[CentOS-based Distributions](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="4570f-155">**[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="4570f-155">**[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="4570f-156">**[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="4570f-156">**[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="4570f-157">**[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="4570f-157">**[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="4570f-158">**[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="4570f-158">**[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="4570f-159">**[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="4570f-159">**[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="4570f-160">**[Další - neschválené distribuce](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="4570f-160">**[Other - Non-Endorsed Distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>

<span data-ttu-id="4570f-161">Viz také  **[poznámky k instalaci Linux](create-upload-generic.md#general-linux-installation-notes)**  Příprava bitové kopie systému Linux na Azure další Obecné tipy pro.</span><span class="sxs-lookup"><span data-stu-id="4570f-161">Also see the **[Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes)** for more general tips on preparing Linux images for Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="4570f-162">[Platformy Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) se vztahují na virtuální počítače se systémem Linux jenom v případě, že jeden z potvrzená distribuce se používá s podrobností konfigurace uvedeného v části "podporované verze [Linux na Azure-Endorsed distribuce](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4570f-162">The [Azure platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) applies to VMs running Linux only when one of the endorsed distributions is used with the configuration details as specified under 'Supported Versions' in [Linux on Azure-Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


## <a name="create-a-resource-group"></a><span data-ttu-id="4570f-163">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="4570f-163">Create a resource group</span></span>
<span data-ttu-id="4570f-164">Skupiny prostředků logicky seskupit všechny prostředky Azure pro podporu virtuálních počítačů, jako je například virtuální sítě a úložiště.</span><span class="sxs-lookup"><span data-stu-id="4570f-164">Resource groups logically bring together all the Azure resources to support your virtual machines, such as the virtual networking and storage.</span></span> <span data-ttu-id="4570f-165">Další informace o [skupin prostředků Azure zde](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4570f-165">Read more about [Azure resource groups here](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="4570f-166">Před nahráním image vlastní disku a vytváření virtuálních počítačů, musíte nejprve vytvořit skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="4570f-166">Before uploading your custom disk image and creating VMs, you first need to create a resource group.</span></span> 

<span data-ttu-id="4570f-167">Následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v `WestUS` umístění:</span><span class="sxs-lookup"><span data-stu-id="4570f-167">The following example creates a resource group named `myResourceGroup` in the `WestUS` location:</span></span>

```azurecli
azure group create myResourceGroup --location "WestUS"
```

## <a name="create-a-storage-account"></a><span data-ttu-id="4570f-168">vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="4570f-168">Create a storage account</span></span>
<span data-ttu-id="4570f-169">Virtuální počítače jsou uloženy jako objekty BLOB stránky v rámci účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="4570f-169">VMs are stored as page blobs within a storage account.</span></span> <span data-ttu-id="4570f-170">Další informace o [zde úložiště objektů blob Azure](../../storage/common/storage-introduction.md#blob-storage).</span><span class="sxs-lookup"><span data-stu-id="4570f-170">Read more about [Azure blob storage here](../../storage/common/storage-introduction.md#blob-storage).</span></span> <span data-ttu-id="4570f-171">Můžete vytvořit účet úložiště pro image vlastní disku a virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="4570f-171">You create a storage account for your custom disk image and VMs.</span></span> <span data-ttu-id="4570f-172">Všechny virtuální počítače, které vytvoříte z bitové kopie vlastní disku musí být ve stejném účtu úložiště jako této bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="4570f-172">Any VMs that you create from your custom disk image need to be in the same storage account as that image.</span></span>

<span data-ttu-id="4570f-173">Následující příklad vytvoří účet úložiště s názvem `mystorageaccount` ve skupině prostředků vytvořili:</span><span class="sxs-lookup"><span data-stu-id="4570f-173">The following example creates a storage account named `mystorageaccount` in the resource group previously created:</span></span>

```azurecli
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

## <a name="list-storage-account-keys"></a><span data-ttu-id="4570f-174">Vypsat klíče účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="4570f-174">List storage account keys</span></span>
<span data-ttu-id="4570f-175">Vygeneruje Azure dva 512bitové přístupové klíče pro každý účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="4570f-175">Azure generates two 512-bit access keys for each storage account.</span></span> <span data-ttu-id="4570f-176">Tyto přístupové klíče se používají při ověřování k účtu úložiště, například k provedení operace zápisu.</span><span class="sxs-lookup"><span data-stu-id="4570f-176">These access keys are used when authenticating to the storage account, such as to carry out write operations.</span></span> <span data-ttu-id="4570f-177">Další informace o [Správa přístupu k úložišti zde](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="4570f-177">Read more about [managing access to storage here](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span> <span data-ttu-id="4570f-178">Můžete zobrazit přístupové klíče se `azure storage account keys list` příkaz.</span><span class="sxs-lookup"><span data-stu-id="4570f-178">You can view access keys with the `azure storage account keys list` command.</span></span>

<span data-ttu-id="4570f-179">Zobrazte přístupové klíče pro účet úložiště, který jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="4570f-179">View the access keys for the storage account you created:</span></span>

```azurecli
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

<span data-ttu-id="4570f-180">Výstup je podobný tomuto:</span><span class="sxs-lookup"><span data-stu-id="4570f-180">The output is similar to:</span></span>

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK

```
<span data-ttu-id="4570f-181">Poznamenejte si `key1` jako použije k interakci se svým účtem úložiště v dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="4570f-181">Make a note of `key1` as you will use it to interact with your storage account in the next steps.</span></span>

## <a name="create-a-storage-container"></a><span data-ttu-id="4570f-182">Vytvoření kontejneru úložiště</span><span class="sxs-lookup"><span data-stu-id="4570f-182">Create a storage container</span></span>
<span data-ttu-id="4570f-183">V stejným způsobem, který vytvoříte různé adresáře, které logicky uspořádat do místního systému souborů můžete vytvořit kontejnery v účtu úložiště k organizaci virtuální disky a bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="4570f-183">In the same way that you create different directories to logically organize your local file system, you create containers within a storage account to organize your virtual disks and images.</span></span> <span data-ttu-id="4570f-184">Účet úložiště může obsahovat libovolný počet kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="4570f-184">A storage account can contain any number of containers.</span></span> 

<span data-ttu-id="4570f-185">Následující příklad vytvoří kontejner s názvem `myimages`, zadání přístupového klíče získaných v předchozím kroku (`key1`):</span><span class="sxs-lookup"><span data-stu-id="4570f-185">The following example creates a container named `myimages`, specifying the access key obtained in the previous step (`key1`):</span></span>

```azurecli
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

## <a name="upload-vhd"></a><span data-ttu-id="4570f-186">Nahrání virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="4570f-186">Upload VHD</span></span>
<span data-ttu-id="4570f-187">Nyní můžete ve skutečnosti nahrát vlastní disku image.</span><span class="sxs-lookup"><span data-stu-id="4570f-187">Now you can actually upload your custom disk image.</span></span> <span data-ttu-id="4570f-188">S všechny virtuální disky, které jsou používány virtuálními počítači, můžete nahrát a uložení bitové kopie disku vlastní jako objekt blob stránky.</span><span class="sxs-lookup"><span data-stu-id="4570f-188">As with all virtual disks used by VMs, you upload and store your custom disk image as a page blob.</span></span>

<span data-ttu-id="4570f-189">Zadejte přístupový klíč, kontejner, který jste vytvořili v předchozím kroku a pak cestu k bitové kopie vlastní disku v místním počítači:</span><span class="sxs-lookup"><span data-stu-id="4570f-189">Specify your access key, the container you created in the previous step, and then the path to the custom disk image on your local computer:</span></span>

```azurecli
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

## <a name="create-vm-from-custom-image"></a><span data-ttu-id="4570f-190">Vytvoření virtuálního počítače z vlastní image</span><span class="sxs-lookup"><span data-stu-id="4570f-190">Create VM from custom image</span></span>
<span data-ttu-id="4570f-191">Když vytvoříte virtuální počítače z image vlastní disk, zadejte identifikátor URI pro bitové kopie disku.</span><span class="sxs-lookup"><span data-stu-id="4570f-191">When you create VMs from your custom disk image, specify the URI to the disk image.</span></span> <span data-ttu-id="4570f-192">Zajistěte, aby na cílové odpovídá účet úložiště se uloží vlastní disku image.</span><span class="sxs-lookup"><span data-stu-id="4570f-192">Ensure that the destination storage account matches where your custom disk image is stored.</span></span> <span data-ttu-id="4570f-193">Můžete vytvořit pomocí rozhraní příkazového řádku Azure nebo Resource Manager JSON šablony virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4570f-193">You can create your VM using the Azure CLI or Resource Manager JSON template.</span></span>

### <a name="create-a-vm-using-the-azure-cli"></a><span data-ttu-id="4570f-194">Vytvoření virtuálního počítače pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="4570f-194">Create a VM using the Azure CLI</span></span>
<span data-ttu-id="4570f-195">Zadáte `--image-urn` parametr s `azure vm create` příkaz tak, aby odkazovaly do bitové kopie disku vlastní.</span><span class="sxs-lookup"><span data-stu-id="4570f-195">You specify the `--image-urn` parameter with the `azure vm create` command to point to your custom disk image.</span></span> <span data-ttu-id="4570f-196">Ujistěte se, že `--storage-account-name` odpovídá účet úložiště, kde je uložena bitová kopie vaše vlastní disku.</span><span class="sxs-lookup"><span data-stu-id="4570f-196">Ensure that `--storage-account-name` matches the storage account where your custom disk image is stored.</span></span> <span data-ttu-id="4570f-197">Není nutné používat stejný kontejner jako bitové kopie vlastní disku k uložení virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4570f-197">You do not have to use the same container as the custom disk image to store your VMs.</span></span> <span data-ttu-id="4570f-198">Ujistěte se, že jste před nahráním vaše vlastní disku Image vytvořit žádné další kontejnery stejným způsobem jako v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="4570f-198">Make sure to create any additional containers in the same way as the earlier steps before uploading your custom disk images.</span></span>

<span data-ttu-id="4570f-199">Následující příklad vytvoří virtuální počítač s názvem `myVM` z bitové kopie disku vlastní:</span><span class="sxs-lookup"><span data-stu-id="4570f-199">The following example creates a VM named `myVM` from your custom disk image:</span></span>

```azurecli
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
    --storage-account-name mystorageaccount
```

<span data-ttu-id="4570f-200">Stále je třeba zadat nebo odpověď výzvy pro všechny další parametry, vyžaduje `azure vm create` příkaz například virtuální sítě, veřejnou IP adresu, uživatelské jméno a klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="4570f-200">You still need to specify, or answer prompts for, all the additional parameters required by the `azure vm create` command such as virtual network, public IP address, username, and SSH keys.</span></span> <span data-ttu-id="4570f-201">Další informace o [dostupné parametry příkazového řádku Resource Manager](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="4570f-201">Read more about the [available CLI Resource Manager parameters](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span></span>

### <a name="create-a-vm-using-a-json-template"></a><span data-ttu-id="4570f-202">Vytvoření virtuálního počítače pomocí šablony JSON</span><span class="sxs-lookup"><span data-stu-id="4570f-202">Create a VM using a JSON template</span></span>
<span data-ttu-id="4570f-203">Šablony Azure Resource Manageru jsou soubory JavaScript Object Notation (JSON), které definují prostředí, ve kterém chcete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="4570f-203">Azure Resource Manager templates are JavaScript Object Notation (JSON) files that define the environment you wish to build.</span></span> <span data-ttu-id="4570f-204">Šablony jsou rozděleny různých prostředků zprostředkovatelé například výpočetní nebo sítě.</span><span class="sxs-lookup"><span data-stu-id="4570f-204">The templates are broken down in to different resource providers such as compute or network.</span></span> <span data-ttu-id="4570f-205">Můžete použít existující šablony nebo napsat vlastní.</span><span class="sxs-lookup"><span data-stu-id="4570f-205">You can use existing templates or write your own.</span></span> <span data-ttu-id="4570f-206">Další informace o [pomocí Resource Manageru a šablony](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4570f-206">Read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="4570f-207">V rámci `Microsoft.Compute/virtualMachines` poskytovatele šablony, máte `storageProfile` uzlu, který obsahuje podrobnosti o konfiguraci pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="4570f-207">Within the `Microsoft.Compute/virtualMachines` provider of your template, you have a `storageProfile` node that contains the configuration details for your VM.</span></span> <span data-ttu-id="4570f-208">Jsou dvě hlavní parametry, které chcete upravit `image` a `vhd` identifikátory URI, které odkazují na vlastní disku image a virtuální disk nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4570f-208">The two main parameters to edit are the `image` and `vhd` URIs that point to your custom disk image and your new VM's virtual disk.</span></span> <span data-ttu-id="4570f-209">Na obrázku je příkladem JSON pro použití image vlastní disku:</span><span class="sxs-lookup"><span data-stu-id="4570f-209">The following shows an example of the JSON for using a custom disk image:</span></span>

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

<span data-ttu-id="4570f-210">Můžete použít [tento existující šablonu k vytvoření virtuálního počítače z vlastní image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) nebo přečtěte si o [vlastních šablon Azure Resource Manager](../../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="4570f-210">You can use [this existing template to create a VM from a custom image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) or read about [creating your own Azure Resource Manager templates](../../azure-resource-manager/resource-group-authoring-templates.md).</span></span> 

<span data-ttu-id="4570f-211">Jakmile máte nakonfigurované šablony, vytvoříte virtuální počítače pomocí `azure group deployment create` příkaz.</span><span class="sxs-lookup"><span data-stu-id="4570f-211">Once you have a template configured, you create your VMs using the `azure group deployment create` command.</span></span> <span data-ttu-id="4570f-212">Zadejte identifikátor URI šablony JSON s `--template-uri` parametr:</span><span class="sxs-lookup"><span data-stu-id="4570f-212">Specify the URI of your JSON template with the `--template-uri` parameter:</span></span>

```azurecli
azure group deployment create --resource-group myResourceGroup
    --template-uri https://uri.to.template/mytemplate.json
```

<span data-ttu-id="4570f-213">Pokud máte soubor JSON, který je uložený místně v počítači, můžete použít `--template-file` parametr místo:</span><span class="sxs-lookup"><span data-stu-id="4570f-213">If you have a JSON file stored locally on your computer, you can use the `--template-file` parameter instead:</span></span>

```azurecli
azure group deployment create --resource-group myResourceGroup
    --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a><span data-ttu-id="4570f-214">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4570f-214">Next steps</span></span>
<span data-ttu-id="4570f-215">Po připravené a nahrát vlastní virtuální disk, si můžete přečíst více o [pomocí Resource Manageru a šablony](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4570f-215">After you have prepared and uploaded your custom virtual disk, you can read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="4570f-216">Můžete také chtít [přidat datový disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) na nové virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="4570f-216">You may also want to [add a data disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) to your new VMs.</span></span> <span data-ttu-id="4570f-217">Pokud máte aplikace spuštěné na vaše virtuální počítače, které potřebujete získat přístup, je nutné [otevřete porty a koncové body](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4570f-217">If you have applications running on your VMs that you need to access, be sure to [open ports and endpoints](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

