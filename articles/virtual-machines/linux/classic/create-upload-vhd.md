---
title: "Vytvoření a nahrání virtuálního pevného disku Linux do Azure | Microsoft Docs"
description: "Vytvoření a odeslání Azure virtuálního pevného disku (VHD) obsahující operační systém Linux pomocí modelu nasazení Classic"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 8058ff98-db03-4309-9bf4-69842bd64dd4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: iainfou
ms.openlocfilehash: 23c30c954875598ce3e01db137b0ef8cda9779f4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="creating-and-uploading-a-virtual-hard-disk-that-contains-the-linux-operating-system"></a><span data-ttu-id="d8b10-103">Vytvoření a nahrání virtuálního pevného disku obsahujícího operační systém Linux</span><span class="sxs-lookup"><span data-stu-id="d8b10-103">Creating and Uploading a Virtual Hard Disk that Contains the Linux Operating System</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="d8b10-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="d8b10-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="d8b10-105">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="d8b10-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="d8b10-106">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d8b10-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="d8b10-107">Můžete také [nahrajte image vlastní disku pomocí Azure Resource Manager](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d8b10-107">You can also [upload a custom disk image using Azure Resource Manager](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="d8b10-108">Tento článek ukazuje, jak vytvořit a odeslat virtuální pevný disk (VHD), můžete ho použít jako vlastní image pro vytváření virtuálních počítačů v Azure.</span><span class="sxs-lookup"><span data-stu-id="d8b10-108">This article shows you how to create and upload a virtual hard disk (VHD) so you can use it as your own image to create virtual machines in Azure.</span></span> <span data-ttu-id="d8b10-109">Zjistěte, jak připravit operační systém, ve kterém můžete vytvořit více virtuálních počítačů na základě této bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="d8b10-109">Learn how to prepare the operating system so you can use it to create multiple virtual machines based on that image.</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="d8b10-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d8b10-110">Prerequisites</span></span>
<span data-ttu-id="d8b10-111">Tento článek předpokládá, že máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="d8b10-111">This article assumes that you have the following items:</span></span>

* <span data-ttu-id="d8b10-112">**Operační systém Linux nainstalován v souboru VHD** -instalaci [distribuce schválené pro Azure Linux](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (nebo v tématu [informace pro neschválené distribuce](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) na virtuální disk v Formát virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="d8b10-112">**Linux operating system installed in a .vhd file** - You have installed an [Azure-endorsed Linux distribution](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (or see [information for non-endorsed distributions](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) to a virtual disk in the VHD format.</span></span> <span data-ttu-id="d8b10-113">Existuje několik nástrojů k vytvoření virtuálního počítače a virtuálního pevného disku:</span><span class="sxs-lookup"><span data-stu-id="d8b10-113">Multiple tools exist to create a VM and VHD:</span></span>
  * <span data-ttu-id="d8b10-114">Instalace a konfigurace [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) nebo [KVM](http://www.linux-kvm.org/page/RunningKVM), aby byl používáte formát bitové kopie virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="d8b10-114">Install and configure [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) or [KVM](http://www.linux-kvm.org/page/RunningKVM), taking care to use VHD as your image format.</span></span> <span data-ttu-id="d8b10-115">V případě potřeby můžete [převést bitovou kopii](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) pomocí `qemu-img convert`.</span><span class="sxs-lookup"><span data-stu-id="d8b10-115">If needed, you can [convert an image](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) using `qemu-img convert`.</span></span>
  * <span data-ttu-id="d8b10-116">Můžete také použít technologie Hyper-V [ve Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) nebo [v systému Windows Server 2012 nebo 2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="d8b10-116">You can also use Hyper-V [on Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) or [on Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="d8b10-117">Novější formát VHDX není podporovaný v Azure.</span><span class="sxs-lookup"><span data-stu-id="d8b10-117">The newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="d8b10-118">Když vytvoříte virtuální počítač, zadejte jako formát virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="d8b10-118">When you create a VM, specify VHD as the format.</span></span> <span data-ttu-id="d8b10-119">V případě potřeby můžete převést disků VHDX virtuální pevný disk pomocí [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) nebo [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d8b10-119">If needed, you can convert VHDX disks to VHD using [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) or the [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="d8b10-120">Navíc Azure nepodporuje odesílání dynamické virtuální pevné disky, takže je nutné převést tyto disky na virtuální pevné disky statické před nahráním.</span><span class="sxs-lookup"><span data-stu-id="d8b10-120">Further, Azure does not support uploading dynamic VHDs, so you need to convert such disks to static VHDs before uploading.</span></span> <span data-ttu-id="d8b10-121">Můžete použít nástroje, jako [nástroje Azure virtuálního pevného disku pro přejděte](https://github.com/Microsoft/azure-vhd-utils-for-go) převést dynamické disky během procesu odesílání do Azure.</span><span class="sxs-lookup"><span data-stu-id="d8b10-121">You can use tools such as [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) to convert dynamic disks during the process of uploading to Azure.</span></span>

* <span data-ttu-id="d8b10-122">**Rozhraní příkazového řádku Azure** – nainstalujte si nejnovější verzi [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) odeslání disku VHD.</span><span class="sxs-lookup"><span data-stu-id="d8b10-122">**Azure Command-line Interface** - Install the latest [Azure Command-Line Interface](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) to upload the VHD.</span></span>

<span data-ttu-id="d8b10-123"><a id="prepimage"> </a></span><span class="sxs-lookup"><span data-stu-id="d8b10-123"><a id="prepimage"> </a></span></span>

## <a name="step-1-prepare-the-image-to-be-uploaded"></a><span data-ttu-id="d8b10-124">Krok 1: Příprava bitové kopie k odeslání</span><span class="sxs-lookup"><span data-stu-id="d8b10-124">Step 1: Prepare the image to be uploaded</span></span>
<span data-ttu-id="d8b10-125">Azure podporuje různé Linuxových distribucích (viz [distribuce schválené](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span><span class="sxs-lookup"><span data-stu-id="d8b10-125">Azure supports various Linux distributions (see [Endorsed Distributions](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span></span> <span data-ttu-id="d8b10-126">V následujících článcích vás provede postup přípravy různé distribuce systému Linux, které jsou podporovány v Azure.</span><span class="sxs-lookup"><span data-stu-id="d8b10-126">The following articles guide you through how to prepare the various Linux distributions that are supported on Azure.</span></span> <span data-ttu-id="d8b10-127">Po dokončení kroků v následujících pokynech se vraťte se sem, až budete mít soubor virtuálního pevného disku, který je připraven k nahrání do Azure:</span><span class="sxs-lookup"><span data-stu-id="d8b10-127">After you complete the steps in the following guides, come back here once you have a VHD file that is ready to upload to Azure:</span></span>

* <span data-ttu-id="d8b10-128">**[Na základě centOS distribuce](../create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="d8b10-128">**[CentOS-based Distributions](../create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="d8b10-129">**[Debian Linux](../debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="d8b10-129">**[Debian Linux](../debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="d8b10-130">**[Oracle Linux](../oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="d8b10-130">**[Oracle Linux](../oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="d8b10-131">**[Red Hat Enterprise Linux](../redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="d8b10-131">**[Red Hat Enterprise Linux](../redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="d8b10-132">**[SLES & openSUSE](../suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="d8b10-132">**[SLES & openSUSE](../suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="d8b10-133">**[Ubuntu](../create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="d8b10-133">**[Ubuntu](../create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="d8b10-134">**[Další - neschválené distribuce](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="d8b10-134">**[Other - Non-Endorsed Distributions](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>

> [!NOTE]
> <span data-ttu-id="d8b10-135">Platformy Azure SLA se vztahuje na virtuální počítače běžící operační systém Linux jenom v případě, že jeden z potvrzená distribuce se používá s konfigurací podrobnosti uvedeného v části "podporované verze v [Linux na Azure-Endorsed distribuce](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d8b10-135">The Azure platform SLA applies to virtual machines running the Linux OS only when one of the endorsed distributions is used with the configuration details as specified under 'Supported Versions' in [Linux on Azure-Endorsed Distributions](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="d8b10-136">Všechny Linuxových distribucích v galerii Azure bitové kopie jsou potvrzená distribuce s požadovanou konfigurací.</span><span class="sxs-lookup"><span data-stu-id="d8b10-136">All Linux distributions in the Azure image gallery are endorsed distributions with the required configuration.</span></span>
> 
> 

<span data-ttu-id="d8b10-137">Viz také  **[poznámky k instalaci Linux](../create-upload-generic.md#general-linux-installation-notes)**  Příprava bitové kopie systému Linux na Azure další Obecné tipy pro.</span><span class="sxs-lookup"><span data-stu-id="d8b10-137">Also see the **[Linux Installation Notes](../create-upload-generic.md#general-linux-installation-notes)** for more general tips on preparing Linux images for Azure.</span></span>

<span data-ttu-id="d8b10-138"><a id="connect"> </a></span><span class="sxs-lookup"><span data-stu-id="d8b10-138"><a id="connect"> </a></span></span>

## <a name="step-2-prepare-the-connection-to-azure"></a><span data-ttu-id="d8b10-139">Krok 2: Příprava připojení k Azure</span><span class="sxs-lookup"><span data-stu-id="d8b10-139">Step 2: Prepare the connection to Azure</span></span>
<span data-ttu-id="d8b10-140">Zajistěte, aby se pomocí rozhraní příkazového řádku Azure v modelu nasazení classic (`azure config mode asm`), pak se přihlaste k účtu:</span><span class="sxs-lookup"><span data-stu-id="d8b10-140">Make sure you are using the Azure CLI in the classic deployment model (`azure config mode asm`), then log in to your account:</span></span>

```azurecli
azure login
```


<span data-ttu-id="d8b10-141"><a id="upload"> </a></span><span class="sxs-lookup"><span data-stu-id="d8b10-141"><a id="upload"> </a></span></span>

## <a name="step-3-upload-the-image-to-azure"></a><span data-ttu-id="d8b10-142">Krok 3: Nahrát na server bitovou kopii do Azure</span><span class="sxs-lookup"><span data-stu-id="d8b10-142">Step 3: Upload the image to Azure</span></span>
<span data-ttu-id="d8b10-143">Musíte nahrát váš soubor virtuálního pevného disku do účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d8b10-143">You need a storage account to upload your VHD file to.</span></span> <span data-ttu-id="d8b10-144">Můžete vybrat buď existující účet úložiště nebo [vytvořte novou](../../../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="d8b10-144">You can either pick an existing storage account or [create a new one](../../../storage/common/storage-create-storage-account.md).</span></span>

<span data-ttu-id="d8b10-145">Pomocí rozhraní příkazového řádku Azure CLI pro nahrání bitovou kopii pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="d8b10-145">Use the Azure CLI to upload the image by using the following command:</span></span>

```azurecli
azure vm image create <ImageName> `
    --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> `
    --os Linux <PathToVHDFile>
```

<span data-ttu-id="d8b10-146">V předchozím příkladu:</span><span class="sxs-lookup"><span data-stu-id="d8b10-146">In the previous example:</span></span>

* <span data-ttu-id="d8b10-147">**BlobStorageURL** je adresa URL pro účet úložiště, kterou plánujete použít</span><span class="sxs-lookup"><span data-stu-id="d8b10-147">**BlobStorageURL** is the URL for the storage account you plan to use</span></span>
* <span data-ttu-id="d8b10-148">**YourImagesFolder** je kontejneru v úložiště objektů blob, kam chcete uložit obrázků</span><span class="sxs-lookup"><span data-stu-id="d8b10-148">**YourImagesFolder** is the container within blob storage where you want to store your images</span></span>
* <span data-ttu-id="d8b10-149">**VHDName** je štítek, který se zobrazí na portálu pro identifikaci virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="d8b10-149">**VHDName** is the label that appears in portal to identify the virtual hard disk.</span></span>
* <span data-ttu-id="d8b10-150">**PathToVHDFile** je úplná cesta a název souboru VHD na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="d8b10-150">**PathToVHDFile** is the full path and name of the .vhd file on your machine.</span></span>

<span data-ttu-id="d8b10-151">Následující příkaz zobrazí úplný příklad:</span><span class="sxs-lookup"><span data-stu-id="d8b10-151">The following command shows a complete example:</span></span>

```azurecli
azure vm image create myImage `
    --blob-url https://mystorage.blob.core.windows.net/vhds/myimage.vhd `
    --os Linux /home/ahmet/myimage.vhd
```

## <a name="step-4-create-a-vm-from-the-image"></a><span data-ttu-id="d8b10-152">Krok 4: Vytvořte virtuální počítač z bitové kopie</span><span class="sxs-lookup"><span data-stu-id="d8b10-152">Step 4: Create a VM from the image</span></span>
<span data-ttu-id="d8b10-153">Vytvoříte virtuální počítač pomocí `azure vm create` stejným způsobem jako regulární virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d8b10-153">You create a VM using `azure vm create` in the same way as a regular VM.</span></span> <span data-ttu-id="d8b10-154">Zadejte název bitové kopie jste zadali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="d8b10-154">Specify the name you gave your image in the previous step.</span></span> <span data-ttu-id="d8b10-155">V následujícím příkladu použijeme **myImage** název bitové kopie v předchozím kroku:</span><span class="sxs-lookup"><span data-stu-id="d8b10-155">In the following example, we use the **myImage** image name given in the previous step:</span></span>

```azurecli
azure vm create --userName ops --password P@ssw0rd! --vm-size Small --ssh `
    --location "West US" "myDeployedVM" myImage
```

<span data-ttu-id="d8b10-156">Pokud chcete vytvořit vlastní virtuální počítače, zadejte vlastní uživatelské jméno + heslo, umístění, název DNS a název bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="d8b10-156">To create your own VMs, provide your own username + password, location, DNS name, and image name.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d8b10-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d8b10-157">Next steps</span></span>
<span data-ttu-id="d8b10-158">Další informace najdete v tématu [referenční dokumentace rozhraní příkazového řádku Azure pro model nasazení Azure classic](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="d8b10-158">For more information, see [Azure CLI reference for the Azure classic deployment model](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span>

[Step 1: Prepare the image to be uploaded]:#prepimage
[Step 2: Prepare the connection to Azure]:#connect
[Step 3: Upload the image to Azure]:#upload
