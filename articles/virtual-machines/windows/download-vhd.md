---
title: "Stáhněte si Windows virtuálního pevného disku z Azure | Microsoft Docs"
description: "Stáhněte si Windows virtuální pevný disk pomocí portálu Azure."
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
ms.openlocfilehash: d8bf89a4b7c2a158302f9ba09a182a3d8d062adc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="download-a-windows-vhd-from-azure"></a><span data-ttu-id="d12c3-103">Stáhněte si Windows virtuálního pevného disku z Azure</span><span class="sxs-lookup"><span data-stu-id="d12c3-103">Download a Windows VHD from Azure</span></span>

<span data-ttu-id="d12c3-104">V tomto článku se dozvíte, jak stáhnout [Windows virtuální pevný disk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) soubor z Azure pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d12c3-104">In this article, you learn how to download a [Windows virtual hard disk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) file from Azure using the Azure portal.</span></span> 

<span data-ttu-id="d12c3-105">Virtuální počítače (VM) Azure používá [disky](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) jako místo pro uložení operačního systému, aplikace a data.</span><span class="sxs-lookup"><span data-stu-id="d12c3-105">Virtual machines (VMs) in Azure use [disks](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) as a place to store an operating system, applications, and data.</span></span> <span data-ttu-id="d12c3-106">Všechny virtuální počítače Azure mít aspoň dva disky – disk operačního systému Windows a dočasný disk.</span><span class="sxs-lookup"><span data-stu-id="d12c3-106">All Azure VMs have at least two disks – a Windows operating system disk and a temporary disk.</span></span> <span data-ttu-id="d12c3-107">Disk s operačním systémem je původně vytvořili z bitové kopie a disku operačního systému a image jsou virtuální pevné disky uložené v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="d12c3-107">The operating system disk is initially created from an image, and both the operating system disk and the image are VHDs stored in an Azure storage account.</span></span> <span data-ttu-id="d12c3-108">Virtuální počítače také může mít jeden nebo více datových disků, které jsou také uloženy jako virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="d12c3-108">Virtual machines also can have one or more data disks, that are also stored as VHDs.</span></span>

## <a name="stop-the-vm"></a><span data-ttu-id="d12c3-109">Zastavení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="d12c3-109">Stop the VM</span></span>

<span data-ttu-id="d12c3-110">Virtuální pevný disk nelze stáhnout ze služby Azure, pokud je připojen k spuštění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d12c3-110">A VHD can’t be downloaded from Azure if it's attached to a running VM.</span></span> <span data-ttu-id="d12c3-111">Budete muset zastavit virtuální počítač ke stažení virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="d12c3-111">You need to stop the VM to download a VHD.</span></span> <span data-ttu-id="d12c3-112">Pokud chcete použít jako virtuální pevný disk [image](tutorial-custom-images.md) k vytvoření dalších virtuálních počítačů s nové disky, použijete [Sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) zobecní operační systém obsažený v souboru a potom zastavte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d12c3-112">If you want to use a VHD as an [image](tutorial-custom-images.md) to create other VMs with new disks, you use [Sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) to generalize the operating system contained in the file and then stop the VM.</span></span> <span data-ttu-id="d12c3-113">Pokud chcete použít virtuální pevný disk jako disk pro novou instanci třídy na existující virtuální počítač nebo datový disk, stačí k zastavení a zrušit přidělení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d12c3-113">To use the VHD as a disk for a new instance of an existing VM or data disk, you only need to stop and deallocate the VM.</span></span>

<span data-ttu-id="d12c3-114">Chcete-li použít virtuální pevný disk jako bitovou kopii k vytvoření dalších virtuálních počítačů, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="d12c3-114">To use the VHD as an image to create other VMs, complete these steps:</span></span>

1.  <span data-ttu-id="d12c3-115">Pokud jste to ještě neudělali, přihlaste se k [Portálu Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d12c3-115">If you haven't already done so, sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2.  <span data-ttu-id="d12c3-116">[Připojte se k Virtuálnímu](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d12c3-116">[Connect to the VM](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 
3.  <span data-ttu-id="d12c3-117">Ve virtuálním počítači otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="d12c3-117">On the VM, open the Command Prompt window as an administrator.</span></span>
4.  <span data-ttu-id="d12c3-118">Změňte adresář na *%windir%\system32\sysprep* a spusťte sysprep.exe.</span><span class="sxs-lookup"><span data-stu-id="d12c3-118">Change the directory to *%windir%\system32\sysprep* and run sysprep.exe.</span></span>
5.  <span data-ttu-id="d12c3-119">V dialogovém okně Nástroj pro přípravu systému vyberte **prostředí Out-of-Box zadejte systému (při prvním zapnutí)**a ujistěte se, že **generalizace** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="d12c3-119">In the System Preparation Tool dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that **Generalize** is selected.</span></span>
6.  <span data-ttu-id="d12c3-120">V možnosti vypnutí, vyberte **vypnutí**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="d12c3-120">In Shutdown Options, select **Shutdown**, and then click **OK**.</span></span> 

<span data-ttu-id="d12c3-121">Pokud chcete použít virtuální pevný disk jako disk pro novou instanci třídy na existující virtuální počítač nebo datový disk, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="d12c3-121">To use the VHD as a disk for a new instance of an existing VM or data disk, complete these steps:</span></span>

1.  <span data-ttu-id="d12c3-122">V nabídce centra v portálu Azure, klikněte na tlačítko **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="d12c3-122">On the Hub menu in the Azure portal, click **Virtual Machines**.</span></span>
2.  <span data-ttu-id="d12c3-123">Vyberte virtuální počítač ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="d12c3-123">Select the VM from the list.</span></span>
3.  <span data-ttu-id="d12c3-124">V okně pro virtuální počítač, klikněte na **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="d12c3-124">On the blade for the VM, click **Stop**.</span></span>

    ![Zastavit virtuální počítač](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a><span data-ttu-id="d12c3-126">Generování adresy URL SAS</span><span class="sxs-lookup"><span data-stu-id="d12c3-126">Generate SAS URL</span></span>

<span data-ttu-id="d12c3-127">Ke stažení souboru virtuálního pevného disku, je nutné generovat [sdílený přístupový podpis (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) adresy URL.</span><span class="sxs-lookup"><span data-stu-id="d12c3-127">To download the VHD file, you need to generate a [shared access signature (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL.</span></span> <span data-ttu-id="d12c3-128">Generování adresy URL čas vypršení platnosti je přiřazena k adrese URL.</span><span class="sxs-lookup"><span data-stu-id="d12c3-128">When the URL is generated, an expiration time is assigned to the URL.</span></span>

1.  <span data-ttu-id="d12c3-129">V nabídce v okně pro virtuální počítač, klikněte na tlačítko **disky**.</span><span class="sxs-lookup"><span data-stu-id="d12c3-129">On the menu of the blade for the VM, click **Disks**.</span></span>
2.  <span data-ttu-id="d12c3-130">Vyberte disk operačního systému pro virtuální počítač a pak klikněte na tlačítko **exportovat**.</span><span class="sxs-lookup"><span data-stu-id="d12c3-130">Select the operating system disk for the VM, and then click **Export**.</span></span>
3.  <span data-ttu-id="d12c3-131">Nastavte hodnotu doby vypršení platnosti adresy URL do *36000*.</span><span class="sxs-lookup"><span data-stu-id="d12c3-131">Set the expiration time of the URL to *36000*.</span></span>
4.  <span data-ttu-id="d12c3-132">Klikněte na tlačítko **generování adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="d12c3-132">Click **Generate URL**.</span></span>

    ![Generování adresy URL](./media/download-vhd/export-generate.png)

> [!NOTE]
> <span data-ttu-id="d12c3-134">Čas vypršení platnosti je vyšší než výchozí zajistit dostatek času na stažení velkého souboru virtuálního pevného disku pro operační systém Windows Server.</span><span class="sxs-lookup"><span data-stu-id="d12c3-134">The expiration time is increased from the default to provide enough time to download the large VHD file for a Windows Server operating system.</span></span> <span data-ttu-id="d12c3-135">Můžete očekávat, že soubor virtuálního pevného disku, který obsahuje operační systém Windows Server do trvat několik hodin v závislosti na připojení stáhnout.</span><span class="sxs-lookup"><span data-stu-id="d12c3-135">You can expect a VHD file that contains the Windows Server operating system to take several hours to download depending on your connection.</span></span> <span data-ttu-id="d12c3-136">Pokud stahujete virtuální pevný disk pro datový disk, je výchozí doba dostatečná.</span><span class="sxs-lookup"><span data-stu-id="d12c3-136">If you are downloading a VHD for a data disk, the default time is sufficient.</span></span> 
> 
> 

## <a name="download-vhd"></a><span data-ttu-id="d12c3-137">Stáhnout virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="d12c3-137">Download VHD</span></span>

1.  <span data-ttu-id="d12c3-138">V části Adresa URL, která byla vygenerována klikněte na tlačítko Stáhnout soubor VHD.</span><span class="sxs-lookup"><span data-stu-id="d12c3-138">Under the URL that was generated, click Download the VHD file.</span></span>

    ![Stáhnout virtuálního pevného disku](./media/download-vhd/export-download.png)

2.  <span data-ttu-id="d12c3-140">Je třeba kliknout na **Uložit** v prohlížeči zahájíte stahování.</span><span class="sxs-lookup"><span data-stu-id="d12c3-140">You may need to click **Save** in the browser to start the download.</span></span> <span data-ttu-id="d12c3-141">Výchozí název souboru virtuálního pevného disku je *abcd*.</span><span class="sxs-lookup"><span data-stu-id="d12c3-141">The default name for the VHD file is *abcd*.</span></span>

    ![Kliknutím na Uložit v prohlížeči](./media/download-vhd/export-save.png)

## <a name="next-steps"></a><span data-ttu-id="d12c3-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d12c3-143">Next steps</span></span>

- <span data-ttu-id="d12c3-144">Zjistěte, jak [nahrát soubor virtuálního pevného disku do Azure](upload-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d12c3-144">Learn how to [upload a VHD file to Azure](upload-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 
- <span data-ttu-id="d12c3-145">[Vytvoření spravované disky z nespravovaných disků v účtu úložiště](attach-disk-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d12c3-145">[Create managed disks from unmanaged disks in a storage account](attach-disk-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
- <span data-ttu-id="d12c3-146">[Správa Azure disky pomocí prostředí PowerShell](tutorial-manage-data-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d12c3-146">[Manage Azure disks with PowerShell](tutorial-manage-data-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

