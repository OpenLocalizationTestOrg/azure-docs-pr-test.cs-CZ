---
title: "aaaDownload Windows virtuálního pevného disku z Azure | Microsoft Docs"
description: "Stáhněte si Windows virtuální pevný disk pomocí hello portálu Azure."
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
ms.openlocfilehash: d0ca8842db98f22751f01648c0ba4e5cde090043
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="download-a-windows-vhd-from-azure"></a><span data-ttu-id="3b7a7-103">Stáhněte si Windows virtuálního pevného disku z Azure</span><span class="sxs-lookup"><span data-stu-id="3b7a7-103">Download a Windows VHD from Azure</span></span>

<span data-ttu-id="3b7a7-104">V tomto článku se dozvíte, jak toodownload [Windows virtuální pevný disk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) hello soubor z Azure pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-104">In this article, you learn how toodownload a [Windows virtual hard disk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) file from Azure using hello Azure portal.</span></span> 

<span data-ttu-id="3b7a7-105">Virtuální počítače (VM) Azure používá [disky](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) jako místní toostore operačního systému, aplikace a data.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-105">Virtual machines (VMs) in Azure use [disks](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) as a place toostore an operating system, applications, and data.</span></span> <span data-ttu-id="3b7a7-106">Všechny virtuální počítače Azure mít aspoň dva disky – disk operačního systému Windows a dočasný disk.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-106">All Azure VMs have at least two disks – a Windows operating system disk and a temporary disk.</span></span> <span data-ttu-id="3b7a7-107">disk s operačním systémem Hello je původně vytvořili z bitové kopie a disku operačního systému hello i hello image jsou virtuální pevné disky uložené v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-107">hello operating system disk is initially created from an image, and both hello operating system disk and hello image are VHDs stored in an Azure storage account.</span></span> <span data-ttu-id="3b7a7-108">Virtuální počítače také může mít jeden nebo více datových disků, které jsou také uloženy jako virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-108">Virtual machines also can have one or more data disks, that are also stored as VHDs.</span></span>

## <a name="stop-hello-vm"></a><span data-ttu-id="3b7a7-109">Zastavit hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="3b7a7-109">Stop hello VM</span></span>

<span data-ttu-id="3b7a7-110">Virtuální pevný disk nelze stáhnout ze služby Azure, pokud je připojen tooa spuštění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-110">A VHD can’t be downloaded from Azure if it's attached tooa running VM.</span></span> <span data-ttu-id="3b7a7-111">Je nutné toostop hello virtuálních počítačů toodownload virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-111">You need toostop hello VM toodownload a VHD.</span></span> <span data-ttu-id="3b7a7-112">Pokud chcete toouse virtuální pevný disk jako [image](tutorial-custom-images.md) toocreate ostatních virtuálních počítačů s nové disky použijete [Sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) toogeneralize hello obsažené v souboru hello operačního systému a poté jej zastavit hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-112">If you want toouse a VHD as an [image](tutorial-custom-images.md) toocreate other VMs with new disks, you use [Sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) toogeneralize hello operating system contained in hello file and then stop hello VM.</span></span> <span data-ttu-id="3b7a7-113">toouse hello virtuální pevný disk jako disk pro novou instanci třídy na existující virtuální počítač nebo datový disk, pouze potřebujete toostop a navrátit hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-113">toouse hello VHD as a disk for a new instance of an existing VM or data disk, you only need toostop and deallocate hello VM.</span></span>

<span data-ttu-id="3b7a7-114">toouse hello virtuálního pevného disku jako image toocreate ostatní virtuální počítače, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3b7a7-114">toouse hello VHD as an image toocreate other VMs, complete these steps:</span></span>

1.  <span data-ttu-id="3b7a7-115">Pokud jste tak již neučinili, přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="3b7a7-115">If you haven't already done so, sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2.  <span data-ttu-id="3b7a7-116">[Připojit virtuální počítač toohello](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3b7a7-116">[Connect toohello VM](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 
3.  <span data-ttu-id="3b7a7-117">Na hello virtuálních počítačů otevřete okno příkazového řádku hello jako správce.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-117">On hello VM, open hello Command Prompt window as an administrator.</span></span>
4.  <span data-ttu-id="3b7a7-118">Změnit adresář, hello příliš*%windir%\system32\sysprep* a spusťte sysprep.exe.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-118">Change hello directory too*%windir%\system32\sysprep* and run sysprep.exe.</span></span>
5.  <span data-ttu-id="3b7a7-119">V dialogovém okně Nástroj pro přípravu systému hello, vyberte **prostředí Out-of-Box zadejte systému (při prvním zapnutí)**a ujistěte se, že **generalizace** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-119">In hello System Preparation Tool dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that **Generalize** is selected.</span></span>
6.  <span data-ttu-id="3b7a7-120">V možnosti vypnutí, vyberte **vypnutí**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-120">In Shutdown Options, select **Shutdown**, and then click **OK**.</span></span> 

<span data-ttu-id="3b7a7-121">toouse hello virtuální pevný disk jako disk pro novou instanci třídy na existující virtuální počítač nebo datový disk, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="3b7a7-121">toouse hello VHD as a disk for a new instance of an existing VM or data disk, complete these steps:</span></span>

1.  <span data-ttu-id="3b7a7-122">V nabídce centra hello v hello portálu Azure, klikněte na tlačítko **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-122">On hello Hub menu in hello Azure portal, click **Virtual Machines**.</span></span>
2.  <span data-ttu-id="3b7a7-123">Vyberte ze seznamu hello hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-123">Select hello VM from hello list.</span></span>
3.  <span data-ttu-id="3b7a7-124">V okně hello hello virtuálních počítačů, klikněte na tlačítko **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-124">On hello blade for hello VM, click **Stop**.</span></span>

    ![Zastavit virtuální počítač](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a><span data-ttu-id="3b7a7-126">Generování adresy URL SAS</span><span class="sxs-lookup"><span data-stu-id="3b7a7-126">Generate SAS URL</span></span>

<span data-ttu-id="3b7a7-127">soubor VHD hello toodownload, je nutné toogenerate [sdílený přístupový podpis (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) adresy URL.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-127">toodownload hello VHD file, you need toogenerate a [shared access signature (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL.</span></span> <span data-ttu-id="3b7a7-128">Generování adresy URL hello čas vypršení platnosti je přiřazena toohello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-128">When hello URL is generated, an expiration time is assigned toohello URL.</span></span>

1.  <span data-ttu-id="3b7a7-129">V nabídce hello hello okna pro hello virtuálních počítačů, klikněte na tlačítko **disky**.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-129">On hello menu of hello blade for hello VM, click **Disks**.</span></span>
2.  <span data-ttu-id="3b7a7-130">Vyberte hello disk operačního systému pro hello virtuálních počítačů a pak klikněte na tlačítko **exportovat**.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-130">Select hello operating system disk for hello VM, and then click **Export**.</span></span>
3.  <span data-ttu-id="3b7a7-131">Nastavit čas vypršení platnosti hello adresy URL hello příliš*36000*.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-131">Set hello expiration time of hello URL too*36000*.</span></span>
4.  <span data-ttu-id="3b7a7-132">Klikněte na tlačítko **generování adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-132">Click **Generate URL**.</span></span>

    ![Generování adresy URL](./media/download-vhd/export-generate.png)

> [!NOTE]
> <span data-ttu-id="3b7a7-134">čas vypršení platnosti Hello vzroste z hello výchozí tooprovide dostatek času toodownload hello velkých souborů virtuálního pevného disku pro operační systém Windows Server.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-134">hello expiration time is increased from hello default tooprovide enough time toodownload hello large VHD file for a Windows Server operating system.</span></span> <span data-ttu-id="3b7a7-135">Můžete očekávat, že soubor virtuálního pevného disku, který obsahuje tootake operačního systému Windows Server hello toodownload několik hodin v závislosti na připojení.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-135">You can expect a VHD file that contains hello Windows Server operating system tootake several hours toodownload depending on your connection.</span></span> <span data-ttu-id="3b7a7-136">Pokud stahujete virtuální pevný disk pro datový disk, hello výchozí doba je dostačující.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-136">If you are downloading a VHD for a data disk, hello default time is sufficient.</span></span> 
> 
> 

## <a name="download-vhd"></a><span data-ttu-id="3b7a7-137">Stáhnout virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="3b7a7-137">Download VHD</span></span>

1.  <span data-ttu-id="3b7a7-138">V části hello adresu URL, která byla vygenerována klikněte na soubor VHD hello stahování.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-138">Under hello URL that was generated, click Download hello VHD file.</span></span>

    ![Stáhnout virtuálního pevného disku](./media/download-vhd/export-download.png)

2.  <span data-ttu-id="3b7a7-140">Může být nutné tooclick **Uložit** v hello prohlížeče toostart hello stahování.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-140">You may need tooclick **Save** in hello browser toostart hello download.</span></span> <span data-ttu-id="3b7a7-141">Hello výchozí název souboru virtuálního pevného disku hello je *abcd*.</span><span class="sxs-lookup"><span data-stu-id="3b7a7-141">hello default name for hello VHD file is *abcd*.</span></span>

    ![Kliknutím na Uložit v prohlížeči hello](./media/download-vhd/export-save.png)

## <a name="next-steps"></a><span data-ttu-id="3b7a7-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3b7a7-143">Next steps</span></span>

- <span data-ttu-id="3b7a7-144">Zjistěte, jak příliš[nahrát tooAzure souboru virtuálního pevného disku](upload-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3b7a7-144">Learn how too[upload a VHD file tooAzure](upload-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 
- <span data-ttu-id="3b7a7-145">[Vytvoření spravované disky z nespravovaných disků v účtu úložiště](attach-disk-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3b7a7-145">[Create managed disks from unmanaged disks in a storage account](attach-disk-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
- <span data-ttu-id="3b7a7-146">[Správa Azure disky pomocí prostředí PowerShell](tutorial-manage-data-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3b7a7-146">[Manage Azure disks with PowerShell](tutorial-manage-data-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

