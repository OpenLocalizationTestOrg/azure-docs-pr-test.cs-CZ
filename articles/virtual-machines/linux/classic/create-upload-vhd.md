---
title: "aaaCreate a nahrání virtuálního pevného disku Linux tooAzure | Microsoft Docs"
description: "Vytvoření a odeslání Azure virtuálního pevného disku (VHD) obsahující operační systém Linux hello pomocí modelu nasazení Classic hello"
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
ms.openlocfilehash: 77b01316386c4a6eb68c129fa68d42f0a8996edc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-and-uploading-a-virtual-hard-disk-that-contains-hello-linux-operating-system"></a><span data-ttu-id="6ca19-103">Vytvoření a nahrání virtuálního pevného disku tohoto hello obsahuje operační systém Linux</span><span class="sxs-lookup"><span data-stu-id="6ca19-103">Creating and Uploading a Virtual Hard Disk that Contains hello Linux Operating System</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="6ca19-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="6ca19-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="6ca19-105">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="6ca19-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="6ca19-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="6ca19-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="6ca19-107">Můžete také [nahrajte image vlastní disku pomocí Azure Resource Manager](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6ca19-107">You can also [upload a custom disk image using Azure Resource Manager](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="6ca19-108">Tento článek ukazuje, jak toocreate a nahrání virtuálního pevného disku (VHD), abyste ho mohli používat jako vlastní image toocreate virtuálních počítačů v Azure.</span><span class="sxs-lookup"><span data-stu-id="6ca19-108">This article shows you how toocreate and upload a virtual hard disk (VHD) so you can use it as your own image toocreate virtual machines in Azure.</span></span> <span data-ttu-id="6ca19-109">Zjistěte, jak tooprepare hello operačního systému, abyste ji mohli používat toocreate více virtuálních počítačů na základě této bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="6ca19-109">Learn how tooprepare hello operating system so you can use it toocreate multiple virtual machines based on that image.</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="6ca19-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6ca19-110">Prerequisites</span></span>
<span data-ttu-id="6ca19-111">Tento článek předpokládá, že máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="6ca19-111">This article assumes that you have hello following items:</span></span>

* <span data-ttu-id="6ca19-112">**Operační systém Linux nainstalován v souboru VHD** -instalaci [distribuce schválené pro Azure Linux](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (nebo v tématu [informace pro neschválené distribuce](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtuálního disku ve formátu VHD hello.</span><span class="sxs-lookup"><span data-stu-id="6ca19-112">**Linux operating system installed in a .vhd file** - You have installed an [Azure-endorsed Linux distribution](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (or see [information for non-endorsed distributions](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtual disk in hello VHD format.</span></span> <span data-ttu-id="6ca19-113">Několik nástrojů existují toocreate virtuálního počítače a virtuálního pevného disku:</span><span class="sxs-lookup"><span data-stu-id="6ca19-113">Multiple tools exist toocreate a VM and VHD:</span></span>
  * <span data-ttu-id="6ca19-114">Instalace a konfigurace [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) nebo [KVM](http://www.linux-kvm.org/page/RunningKVM), dbejte na to toouse virtuálního pevného disku jako formát obrázku.</span><span class="sxs-lookup"><span data-stu-id="6ca19-114">Install and configure [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) or [KVM](http://www.linux-kvm.org/page/RunningKVM), taking care toouse VHD as your image format.</span></span> <span data-ttu-id="6ca19-115">V případě potřeby můžete [převést bitovou kopii](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) pomocí `qemu-img convert`.</span><span class="sxs-lookup"><span data-stu-id="6ca19-115">If needed, you can [convert an image](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) using `qemu-img convert`.</span></span>
  * <span data-ttu-id="6ca19-116">Můžete také použít technologie Hyper-V [ve Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) nebo [v systému Windows Server 2012 nebo 2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="6ca19-116">You can also use Hyper-V [on Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) or [on Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="6ca19-117">Hello novější formát VHDX není podporovaný v Azure.</span><span class="sxs-lookup"><span data-stu-id="6ca19-117">hello newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="6ca19-118">Když vytvoříte virtuální počítač, zadejte hello Formát virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="6ca19-118">When you create a VM, specify VHD as hello format.</span></span> <span data-ttu-id="6ca19-119">V případě potřeby můžete převést pomocí tooVHD disky VHDX [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) nebo hello [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6ca19-119">If needed, you can convert VHDX disks tooVHD using [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) or hello [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="6ca19-120">Navíc Azure nepodporuje odesílání dynamické virtuální pevné disky, takže je třeba tooconvert takové toostatic disky VHD před nahráním.</span><span class="sxs-lookup"><span data-stu-id="6ca19-120">Further, Azure does not support uploading dynamic VHDs, so you need tooconvert such disks toostatic VHDs before uploading.</span></span> <span data-ttu-id="6ca19-121">Můžete použít nástroje, jako [nástroje Azure virtuálního pevného disku pro přejděte](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert během procesu hello odesílání tooAzure dynamické disky.</span><span class="sxs-lookup"><span data-stu-id="6ca19-121">You can use tools such as [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert dynamic disks during hello process of uploading tooAzure.</span></span>

* <span data-ttu-id="6ca19-122">**Rozhraní příkazového řádku Azure** -instalace hello nejnovější [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) tooupload hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="6ca19-122">**Azure Command-line Interface** - Install hello latest [Azure Command-Line Interface](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) tooupload hello VHD.</span></span>

<span data-ttu-id="6ca19-123"><a id="prepimage"> </a></span><span class="sxs-lookup"><span data-stu-id="6ca19-123"><a id="prepimage"> </a></span></span>

## <a name="step-1-prepare-hello-image-toobe-uploaded"></a><span data-ttu-id="6ca19-124">Krok 1: Příprava toobe hello image nahrát</span><span class="sxs-lookup"><span data-stu-id="6ca19-124">Step 1: Prepare hello image toobe uploaded</span></span>
<span data-ttu-id="6ca19-125">Azure podporuje různé Linuxových distribucích (viz [distribuce schválené](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span><span class="sxs-lookup"><span data-stu-id="6ca19-125">Azure supports various Linux distributions (see [Endorsed Distributions](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span></span> <span data-ttu-id="6ca19-126">Následující články Hello provede jak tooprepare hello různé distribuce systému Linux, které jsou podporovány v Azure.</span><span class="sxs-lookup"><span data-stu-id="6ca19-126">hello following articles guide you through how tooprepare hello various Linux distributions that are supported on Azure.</span></span> <span data-ttu-id="6ca19-127">Po dokončení kroků hello v následujících příručkách hello pocházet vraťte se sem, až budete mít soubor virtuálního pevného disku, který je připraven tooupload tooAzure:</span><span class="sxs-lookup"><span data-stu-id="6ca19-127">After you complete hello steps in hello following guides, come back here once you have a VHD file that is ready tooupload tooAzure:</span></span>

* <span data-ttu-id="6ca19-128">**[Na základě centOS distribuce](../create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="6ca19-128">**[CentOS-based Distributions](../create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="6ca19-129">**[Debian Linux](../debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="6ca19-129">**[Debian Linux](../debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="6ca19-130">**[Oracle Linux](../oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="6ca19-130">**[Oracle Linux](../oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="6ca19-131">**[Red Hat Enterprise Linux](../redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="6ca19-131">**[Red Hat Enterprise Linux](../redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="6ca19-132">**[SLES & openSUSE](../suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="6ca19-132">**[SLES & openSUSE](../suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="6ca19-133">**[Ubuntu](../create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="6ca19-133">**[Ubuntu](../create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="6ca19-134">**[Další - neschválené distribuce](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="6ca19-134">**[Other - Non-Endorsed Distributions](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>

> [!NOTE]
> <span data-ttu-id="6ca19-135">Hello platformy Azure SLA se vztahuje toovirtual počítače běžící hello operační systém Linux jenom v případě, že distribuce schválené pro jednu z hello používá s podrobnosti konfigurace hello uvedeného v části "podporované verze na [Linux na Azure-Endorsed distribuce ](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6ca19-135">hello Azure platform SLA applies toovirtual machines running hello Linux OS only when one of hello endorsed distributions is used with hello configuration details as specified under 'Supported Versions' in [Linux on Azure-Endorsed Distributions](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="6ca19-136">Všechny Linuxových distribucích v galerii Azure image hello jsou potvrzená distribuce hello požadovanou konfigurací.</span><span class="sxs-lookup"><span data-stu-id="6ca19-136">All Linux distributions in hello Azure image gallery are endorsed distributions with hello required configuration.</span></span>
> 
> 

<span data-ttu-id="6ca19-137">Viz také hello  **[poznámky k instalaci Linux](../create-upload-generic.md#general-linux-installation-notes)**  Příprava bitové kopie systému Linux na Azure další Obecné tipy pro.</span><span class="sxs-lookup"><span data-stu-id="6ca19-137">Also see hello **[Linux Installation Notes](../create-upload-generic.md#general-linux-installation-notes)** for more general tips on preparing Linux images for Azure.</span></span>

<span data-ttu-id="6ca19-138"><a id="connect"> </a></span><span class="sxs-lookup"><span data-stu-id="6ca19-138"><a id="connect"> </a></span></span>

## <a name="step-2-prepare-hello-connection-tooazure"></a><span data-ttu-id="6ca19-139">Krok 2: Příprava tooAzure připojení hello</span><span class="sxs-lookup"><span data-stu-id="6ca19-139">Step 2: Prepare hello connection tooAzure</span></span>
<span data-ttu-id="6ca19-140">Zajistěte, aby používáte hello rozhraní příkazového řádku Azure v modelu nasazení classic hello (`azure config mode asm`), potom se přihlaste v tooyour účtu:</span><span class="sxs-lookup"><span data-stu-id="6ca19-140">Make sure you are using hello Azure CLI in hello classic deployment model (`azure config mode asm`), then log in tooyour account:</span></span>

```azurecli
azure login
```


<span data-ttu-id="6ca19-141"><a id="upload"> </a></span><span class="sxs-lookup"><span data-stu-id="6ca19-141"><a id="upload"> </a></span></span>

## <a name="step-3-upload-hello-image-tooazure"></a><span data-ttu-id="6ca19-142">Krok 3: Nahrát tooAzure hello bitové kopie</span><span class="sxs-lookup"><span data-stu-id="6ca19-142">Step 3: Upload hello image tooAzure</span></span>
<span data-ttu-id="6ca19-143">Je nutné souboru virtuálního pevného disku na tooupload účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="6ca19-143">You need a storage account tooupload your VHD file to.</span></span> <span data-ttu-id="6ca19-144">Můžete vybrat buď existující účet úložiště nebo [vytvořte novou](../../../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="6ca19-144">You can either pick an existing storage account or [create a new one](../../../storage/common/storage-create-storage-account.md).</span></span>

<span data-ttu-id="6ca19-145">Použijte bitovou kopii hello hello rozhraní příkazového řádku Azure tooupload pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6ca19-145">Use hello Azure CLI tooupload hello image by using hello following command:</span></span>

```azurecli
azure vm image create <ImageName> `
    --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> `
    --os Linux <PathToVHDFile>
```

<span data-ttu-id="6ca19-146">V předchozím příkladu hello:</span><span class="sxs-lookup"><span data-stu-id="6ca19-146">In hello previous example:</span></span>

* <span data-ttu-id="6ca19-147">**BlobStorageURL** hello adresa URL pro účet úložiště hello plánování toouse</span><span class="sxs-lookup"><span data-stu-id="6ca19-147">**BlobStorageURL** is hello URL for hello storage account you plan toouse</span></span>
* <span data-ttu-id="6ca19-148">**YourImagesFolder** je hello kontejneru v úložiště objektů blob místo toostore obrázků</span><span class="sxs-lookup"><span data-stu-id="6ca19-148">**YourImagesFolder** is hello container within blob storage where you want toostore your images</span></span>
* <span data-ttu-id="6ca19-149">**VHDName** je hello štítek, který se zobrazí v portálu tooidentify hello virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="6ca19-149">**VHDName** is hello label that appears in portal tooidentify hello virtual hard disk.</span></span>
* <span data-ttu-id="6ca19-150">**PathToVHDFile** je hello úplná cesta a název souboru VHD hello na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="6ca19-150">**PathToVHDFile** is hello full path and name of hello .vhd file on your machine.</span></span>

<span data-ttu-id="6ca19-151">Hello následující příkaz ukazuje kompletní příklad:</span><span class="sxs-lookup"><span data-stu-id="6ca19-151">hello following command shows a complete example:</span></span>

```azurecli
azure vm image create myImage `
    --blob-url https://mystorage.blob.core.windows.net/vhds/myimage.vhd `
    --os Linux /home/ahmet/myimage.vhd
```

## <a name="step-4-create-a-vm-from-hello-image"></a><span data-ttu-id="6ca19-152">Krok 4: Vytvoření virtuálního počítače z hello image</span><span class="sxs-lookup"><span data-stu-id="6ca19-152">Step 4: Create a VM from hello image</span></span>
<span data-ttu-id="6ca19-153">Vytvoříte virtuální počítač pomocí `azure vm create` v hello stejným způsobem jako regulární virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6ca19-153">You create a VM using `azure vm create` in hello same way as a regular VM.</span></span> <span data-ttu-id="6ca19-154">Zadejte název hello dáte bitové kopie v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="6ca19-154">Specify hello name you gave your image in hello previous step.</span></span> <span data-ttu-id="6ca19-155">V následujícím příkladu hello, použijeme hello **myImage** uveden v předchozím kroku hello název bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="6ca19-155">In hello following example, we use hello **myImage** image name given in hello previous step:</span></span>

```azurecli
azure vm create --userName ops --password P@ssw0rd! --vm-size Small --ssh `
    --location "West US" "myDeployedVM" myImage
```

<span data-ttu-id="6ca19-156">toocreate vlastní virtuální počítače, zadejte vlastní uživatelské jméno + heslo, umístění, název DNS a název bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="6ca19-156">toocreate your own VMs, provide your own username + password, location, DNS name, and image name.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6ca19-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6ca19-157">Next steps</span></span>
<span data-ttu-id="6ca19-158">Další informace najdete v tématu [referenční dokumentace rozhraní příkazového řádku Azure pro model nasazení Azure classic hello](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="6ca19-158">For more information, see [Azure CLI reference for hello Azure classic deployment model](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span>

[Step 1: Prepare hello image toobe uploaded]:#prepimage
[Step 2: Prepare hello connection tooAzure]:#connect
[Step 3: Upload hello image tooAzure]:#upload
