---
title: "Stáhnout Linux virtuální pevný disk z Azure | Microsoft Docs"
description: "Stáhněte si Linux virtuální pevný disk pomocí rozhraní příkazového řádku Azure a webu Azure portal."
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
ms.openlocfilehash: 3eb88478b43f8e3a36ae04bf3703f238e8cb1f3e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="download-a-linux-vhd-from-azure"></a><span data-ttu-id="beb13-103">Stáhnout Linux virtuální pevný disk z Azure</span><span class="sxs-lookup"><span data-stu-id="beb13-103">Download a Linux VHD from Azure</span></span>

<span data-ttu-id="beb13-104">V tomto článku se dozvíte, jak stáhnout [Linux virtuální pevný disk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) soubor z Azure pomocí rozhraní příkazového řádku Azure a portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="beb13-104">In this article, you learn how to download a [Linux virtual hard disk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) file from Azure using the Azure CLI and Azure portal.</span></span> 

<span data-ttu-id="beb13-105">Virtuální počítače (VM) Azure používá [disky](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) jako místo pro uložení operačního systému, aplikace a data.</span><span class="sxs-lookup"><span data-stu-id="beb13-105">Virtual machines (VMs) in Azure use [disks](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) as a place to store an operating system, applications, and data.</span></span> <span data-ttu-id="beb13-106">Všechny virtuální počítače Azure mít aspoň dva disky – disk operačního systému Windows a dočasný disk.</span><span class="sxs-lookup"><span data-stu-id="beb13-106">All Azure VMs have at least two disks – a Windows operating system disk and a temporary disk.</span></span> <span data-ttu-id="beb13-107">Disk s operačním systémem je původně vytvořili z bitové kopie a disku operačního systému a image jsou virtuální pevné disky uložené v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="beb13-107">The operating system disk is initially created from an image, and both the operating system disk and the image are VHDs stored in an Azure storage account.</span></span> <span data-ttu-id="beb13-108">Virtuální počítače také může mít jeden nebo více datových disků, které jsou také uloženy jako virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="beb13-108">Virtual machines also can have one or more data disks, that are also stored as VHDs.</span></span>

<span data-ttu-id="beb13-109">Pokud jste tak již neučinili, nainstalujte [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="beb13-109">If you haven't already done so, install [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span></span>

## <a name="stop-the-vm"></a><span data-ttu-id="beb13-110">Zastavení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="beb13-110">Stop the VM</span></span>

<span data-ttu-id="beb13-111">Virtuální pevný disk nelze stáhnout ze služby Azure, pokud je připojen k spuštění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="beb13-111">A VHD can’t be downloaded from Azure if it's attached to a running VM.</span></span> <span data-ttu-id="beb13-112">Budete muset zastavit virtuální počítač ke stažení virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="beb13-112">You need to stop the VM to download a VHD.</span></span> <span data-ttu-id="beb13-113">Pokud chcete použít jako virtuální pevný disk [image](tutorial-custom-images.md) vytvoření dalších virtuálních počítačů pomocí nové disky, budete muset zrušit jejich zřízení a zobecní operační systém obsažený v souboru a zastavte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="beb13-113">If you want to use a VHD as an [image](tutorial-custom-images.md) to create other VMs with new disks, you need to deprovision and generalize the operating system contained in the file and stop the VM.</span></span> <span data-ttu-id="beb13-114">Pokud chcete použít virtuální pevný disk jako disk pro novou instanci třídy na existující virtuální počítač nebo datový disk, stačí k zastavení a zrušit přidělení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="beb13-114">To use the VHD as a disk for a new instance of an existing VM or data disk, you only need to stop and deallocate the VM.</span></span>

<span data-ttu-id="beb13-115">Chcete-li použít virtuální pevný disk jako bitovou kopii k vytvoření dalších virtuálních počítačů, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="beb13-115">To use the VHD as an image to create other VMs, complete these steps:</span></span>

1. <span data-ttu-id="beb13-116">Použijte k připojení k němu a zrušit jejich zřízení se SSH, název účtu a veřejnou IP adresu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="beb13-116">Use SSH, the account name, and the public IP address of the VM to connect to it and deprovision it.</span></span> <span data-ttu-id="beb13-117">+ Uživatele parametr také odebere poslední účet zřízení uživatele.</span><span class="sxs-lookup"><span data-stu-id="beb13-117">The +user parameter also removes the last provisioned user account.</span></span> <span data-ttu-id="beb13-118">Pokud jsou pečení přihlašovací údaje k virtuálnímu počítači, nechte si to + parametr uživatele.</span><span class="sxs-lookup"><span data-stu-id="beb13-118">If you are baking account credentials in to the VM, leave out this +user parameter.</span></span> <span data-ttu-id="beb13-119">Následující příklad odebere poslední účet zřízení uživatele:</span><span class="sxs-lookup"><span data-stu-id="beb13-119">The following example removes the last provisioned user account:</span></span>

    ```bash
    ssh azureuser@40.118.249.235
    sudo waagent -deprovision+user -force
    exit 
    ```

2. <span data-ttu-id="beb13-120">Přihlaste se k účtu Azure s [az přihlášení](https://docs.microsoft.com/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="beb13-120">Sign in to your Azure account with [az login](https://docs.microsoft.com/cli/azure/#login).</span></span>
3. <span data-ttu-id="beb13-121">Zastavte a zrušit přidělení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="beb13-121">Stop and deallocate the VM.</span></span>

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

4. <span data-ttu-id="beb13-122">Generalize virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="beb13-122">Generalize the VM.</span></span> 

    ```azurecli
    az vm generalize --resource-group myResourceGroup --name myVM
    ``` 

<span data-ttu-id="beb13-123">Pokud chcete použít virtuální pevný disk jako disk pro novou instanci třídy na existující virtuální počítač nebo datový disk, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="beb13-123">To use the VHD as a disk for a new instance of an existing VM or data disk, complete these steps:</span></span>

1.  <span data-ttu-id="beb13-124">Přihlaste se k webu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="beb13-124">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2.  <span data-ttu-id="beb13-125">V nabídce centra klikněte na **Virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="beb13-125">On the Hub menu, click **Virtual Machines**.</span></span>
3.  <span data-ttu-id="beb13-126">Vyberte virtuální počítač ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="beb13-126">Select the VM from the list.</span></span>
4.  <span data-ttu-id="beb13-127">V okně pro virtuální počítač, klikněte na **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="beb13-127">On the blade for the VM, click **Stop**.</span></span>

    ![Zastavit virtuální počítač](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a><span data-ttu-id="beb13-129">Generování adresy URL SAS</span><span class="sxs-lookup"><span data-stu-id="beb13-129">Generate SAS URL</span></span>

<span data-ttu-id="beb13-130">Ke stažení souboru virtuálního pevného disku, je nutné generovat [sdílený přístupový podpis (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) adresy URL.</span><span class="sxs-lookup"><span data-stu-id="beb13-130">To download the VHD file, you need to generate a [shared access signature (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL.</span></span> <span data-ttu-id="beb13-131">Generování adresy URL čas vypršení platnosti je přiřazena k adrese URL.</span><span class="sxs-lookup"><span data-stu-id="beb13-131">When the URL is generated, an expiration time is assigned to the URL.</span></span>

1.  <span data-ttu-id="beb13-132">V nabídce v okně pro virtuální počítač, klikněte na tlačítko **disky**.</span><span class="sxs-lookup"><span data-stu-id="beb13-132">On the menu of the blade for the VM, click **Disks**.</span></span>
2.  <span data-ttu-id="beb13-133">Vyberte disk operačního systému pro virtuální počítač a pak klikněte na tlačítko **exportovat**.</span><span class="sxs-lookup"><span data-stu-id="beb13-133">Select the operating system disk for the VM, and then click **Export**.</span></span>
3.  <span data-ttu-id="beb13-134">Klikněte na tlačítko **generování adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="beb13-134">Click **Generate URL**.</span></span>

    ![Generování adresy URL](./media/download-vhd/export-generate.png)

## <a name="download-vhd"></a><span data-ttu-id="beb13-136">Stáhnout virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="beb13-136">Download VHD</span></span>

1.  <span data-ttu-id="beb13-137">V části Adresa URL, která byla vygenerována klikněte na tlačítko Stáhnout soubor VHD.</span><span class="sxs-lookup"><span data-stu-id="beb13-137">Under the URL that was generated, click Download the VHD file.</span></span>

    ![Stáhnout virtuálního pevného disku](./media/download-vhd/export-download.png)

2.  <span data-ttu-id="beb13-139">Je třeba kliknout na **Uložit** v prohlížeči zahájíte stahování.</span><span class="sxs-lookup"><span data-stu-id="beb13-139">You may need to click **Save** in the browser to start the download.</span></span> <span data-ttu-id="beb13-140">Výchozí název souboru virtuálního pevného disku je *abcd*.</span><span class="sxs-lookup"><span data-stu-id="beb13-140">The default name for the VHD file is *abcd*.</span></span>

    ![Kliknutím na Uložit v prohlížeči](./media/download-vhd/export-save.png)

## <a name="next-steps"></a><span data-ttu-id="beb13-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="beb13-142">Next steps</span></span>

- <span data-ttu-id="beb13-143">Zjistěte, jak [odesílání a vytvoření virtuálního počítače s Linuxem z vlastní disk s Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="beb13-143">Learn how to [upload and create a Linux VM from custom disk with the Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 
- <span data-ttu-id="beb13-144">[Správa Azure disků Azure CLI](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="beb13-144">[Manage Azure disks the Azure CLI](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

