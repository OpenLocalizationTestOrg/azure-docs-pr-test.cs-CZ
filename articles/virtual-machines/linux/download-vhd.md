---
title: "aaaDownload Linux virtuálního pevného disku z Azure | Microsoft Docs"
description: "Stáhněte si Linux virtuální pevný disk pomocí hello rozhraní příkazového řádku Azure a hello portálu Azure."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: davidmu
ms.openlocfilehash: 7e08e985a64a6be581b8f5eedcce60fbd314eaf1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="download-a-linux-vhd-from-azure"></a><span data-ttu-id="ad366-103">Stáhnout Linux virtuální pevný disk z Azure</span><span class="sxs-lookup"><span data-stu-id="ad366-103">Download a Linux VHD from Azure</span></span>

<span data-ttu-id="ad366-104">V tomto článku se dozvíte, jak toodownload [Linux virtuální pevný disk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) hello soubor z Azure pomocí rozhraní příkazového řádku Azure a portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ad366-104">In this article, you learn how toodownload a [Linux virtual hard disk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) file from Azure using hello Azure CLI and Azure portal.</span></span> 

<span data-ttu-id="ad366-105">Virtuální počítače (VM) Azure používá [disky](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) jako místní toostore operačního systému, aplikace a data.</span><span class="sxs-lookup"><span data-stu-id="ad366-105">Virtual machines (VMs) in Azure use [disks](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) as a place toostore an operating system, applications, and data.</span></span> <span data-ttu-id="ad366-106">Všechny virtuální počítače Azure mít aspoň dva disky – disk operačního systému Windows a dočasný disk.</span><span class="sxs-lookup"><span data-stu-id="ad366-106">All Azure VMs have at least two disks – a Windows operating system disk and a temporary disk.</span></span> <span data-ttu-id="ad366-107">disk s operačním systémem Hello je původně vytvořili z bitové kopie a disku operačního systému hello i hello image jsou virtuální pevné disky uložené v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="ad366-107">hello operating system disk is initially created from an image, and both hello operating system disk and hello image are VHDs stored in an Azure storage account.</span></span> <span data-ttu-id="ad366-108">Virtuální počítače také může mít jeden nebo více datových disků, které jsou také uloženy jako virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="ad366-108">Virtual machines also can have one or more data disks, that are also stored as VHDs.</span></span>

<span data-ttu-id="ad366-109">Pokud jste tak již neučinili, nainstalujte [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="ad366-109">If you haven't already done so, install [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span></span>

## <a name="stop-hello-vm"></a><span data-ttu-id="ad366-110">Zastavit hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="ad366-110">Stop hello VM</span></span>

<span data-ttu-id="ad366-111">Virtuální pevný disk nelze stáhnout ze služby Azure, pokud je připojen tooa spuštění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ad366-111">A VHD can’t be downloaded from Azure if it's attached tooa running VM.</span></span> <span data-ttu-id="ad366-112">Je nutné toostop hello virtuálních počítačů toodownload virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="ad366-112">You need toostop hello VM toodownload a VHD.</span></span> <span data-ttu-id="ad366-113">Pokud chcete toouse virtuální pevný disk jako [image](tutorial-custom-images.md) toocreate ostatních virtuálních počítačů s nové disky, potřebujete toodeprovision a generalize hello operačního systému, která je součástí hello souboru a zastavit hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ad366-113">If you want toouse a VHD as an [image](tutorial-custom-images.md) toocreate other VMs with new disks, you need toodeprovision and generalize hello operating system contained in hello file and stop hello VM.</span></span> <span data-ttu-id="ad366-114">toouse hello virtuální pevný disk jako disk pro novou instanci třídy na existující virtuální počítač nebo datový disk, pouze potřebujete toostop a navrátit hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ad366-114">toouse hello VHD as a disk for a new instance of an existing VM or data disk, you only need toostop and deallocate hello VM.</span></span>

<span data-ttu-id="ad366-115">toouse hello virtuálního pevného disku jako image toocreate ostatní virtuální počítače, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ad366-115">toouse hello VHD as an image toocreate other VMs, complete these steps:</span></span>

1. <span data-ttu-id="ad366-116">Použití SSH, název účtu hello a hello veřejné IP adresy tooit tooconnect hello virtuálních počítačů a zrušit jejich zřízení ho.</span><span class="sxs-lookup"><span data-stu-id="ad366-116">Use SSH, hello account name, and hello public IP address of hello VM tooconnect tooit and deprovision it.</span></span> <span data-ttu-id="ad366-117">Vítejte + uživatele parametr také odebere poslední účet zřízení uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="ad366-117">hello +user parameter also removes hello last provisioned user account.</span></span> <span data-ttu-id="ad366-118">Pokud jsou pečení přihlašovací údaje toohello virtuálních počítačů, nechte si to + parametr uživatele.</span><span class="sxs-lookup"><span data-stu-id="ad366-118">If you are baking account credentials in toohello VM, leave out this +user parameter.</span></span> <span data-ttu-id="ad366-119">Hello následující příklad odebere poslední účet zřízení uživatele hello:</span><span class="sxs-lookup"><span data-stu-id="ad366-119">hello following example removes hello last provisioned user account:</span></span>

    ```bash
    ssh azureuser@40.118.249.235
    sudo waagent -deprovision+user -force
    exit 
    ```

2. <span data-ttu-id="ad366-120">Přihlaste se tooyour účet Azure s [az přihlášení](https://docs.microsoft.com/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="ad366-120">Sign in tooyour Azure account with [az login](https://docs.microsoft.com/cli/azure/#login).</span></span>
3. <span data-ttu-id="ad366-121">Zastavte a navrátit hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ad366-121">Stop and deallocate hello VM.</span></span>

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

4. <span data-ttu-id="ad366-122">Generalize hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ad366-122">Generalize hello VM.</span></span> 

    ```azurecli
    az vm generalize --resource-group myResourceGroup --name myVM
    ``` 

<span data-ttu-id="ad366-123">toouse hello virtuální pevný disk jako disk pro novou instanci třídy na existující virtuální počítač nebo datový disk, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="ad366-123">toouse hello VHD as a disk for a new instance of an existing VM or data disk, complete these steps:</span></span>

1.  <span data-ttu-id="ad366-124">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="ad366-124">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2.  <span data-ttu-id="ad366-125">V nabídce centra hello, klikněte na tlačítko **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="ad366-125">On hello Hub menu, click **Virtual Machines**.</span></span>
3.  <span data-ttu-id="ad366-126">Vyberte ze seznamu hello hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ad366-126">Select hello VM from hello list.</span></span>
4.  <span data-ttu-id="ad366-127">V okně hello hello virtuálních počítačů, klikněte na tlačítko **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="ad366-127">On hello blade for hello VM, click **Stop**.</span></span>

    ![Zastavit virtuální počítač](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a><span data-ttu-id="ad366-129">Generování adresy URL SAS</span><span class="sxs-lookup"><span data-stu-id="ad366-129">Generate SAS URL</span></span>

<span data-ttu-id="ad366-130">soubor VHD hello toodownload, je nutné toogenerate [sdílený přístupový podpis (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) adresy URL.</span><span class="sxs-lookup"><span data-stu-id="ad366-130">toodownload hello VHD file, you need toogenerate a [shared access signature (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL.</span></span> <span data-ttu-id="ad366-131">Generování adresy URL hello čas vypršení platnosti je přiřazena toohello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="ad366-131">When hello URL is generated, an expiration time is assigned toohello URL.</span></span>

1.  <span data-ttu-id="ad366-132">V nabídce hello hello okna pro hello virtuálních počítačů, klikněte na tlačítko **disky**.</span><span class="sxs-lookup"><span data-stu-id="ad366-132">On hello menu of hello blade for hello VM, click **Disks**.</span></span>
2.  <span data-ttu-id="ad366-133">Vyberte hello disk operačního systému pro hello virtuálních počítačů a pak klikněte na tlačítko **exportovat**.</span><span class="sxs-lookup"><span data-stu-id="ad366-133">Select hello operating system disk for hello VM, and then click **Export**.</span></span>
3.  <span data-ttu-id="ad366-134">Klikněte na tlačítko **generování adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="ad366-134">Click **Generate URL**.</span></span>

    ![Generování adresy URL](./media/download-vhd/export-generate.png)

## <a name="download-vhd"></a><span data-ttu-id="ad366-136">Stáhnout virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="ad366-136">Download VHD</span></span>

1.  <span data-ttu-id="ad366-137">V části hello adresu URL, která byla vygenerována klikněte na soubor VHD hello stahování.</span><span class="sxs-lookup"><span data-stu-id="ad366-137">Under hello URL that was generated, click Download hello VHD file.</span></span>

    ![Stáhnout virtuálního pevného disku](./media/download-vhd/export-download.png)

2.  <span data-ttu-id="ad366-139">Může být nutné tooclick **Uložit** v hello prohlížeče toostart hello stahování.</span><span class="sxs-lookup"><span data-stu-id="ad366-139">You may need tooclick **Save** in hello browser toostart hello download.</span></span> <span data-ttu-id="ad366-140">Hello výchozí název souboru virtuálního pevného disku hello je *abcd*.</span><span class="sxs-lookup"><span data-stu-id="ad366-140">hello default name for hello VHD file is *abcd*.</span></span>

    ![Kliknutím na Uložit v prohlížeči hello](./media/download-vhd/export-save.png)

## <a name="next-steps"></a><span data-ttu-id="ad366-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ad366-142">Next steps</span></span>

- <span data-ttu-id="ad366-143">Zjistěte, jak příliš[odesílání a vytvoření virtuálního počítače s Linuxem z vlastní disk s hello Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ad366-143">Learn how too[upload and create a Linux VM from custom disk with hello Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 
- <span data-ttu-id="ad366-144">[Správa Azure disky hello rozhraní příkazového řádku Azure](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ad366-144">[Manage Azure disks hello Azure CLI](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

