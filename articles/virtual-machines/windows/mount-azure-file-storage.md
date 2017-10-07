---
title: "aaaMount úložiště Azure file z virtuálního počítače Windows Azure | Microsoft Docs"
description: "Uložte soubor v hello cloudu s Azure file storage a připojte svou cloudovou sdílenou z Azure virtuálního počítače (VM)."
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: 
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: 965f1c1b3f0d07fec6d86f9312a05e02e8ce7fe0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-file-shares-with-windows-vms"></a><span data-ttu-id="17a38-103">Použít sdílené složky Azure s virtuálními počítači Windows</span><span class="sxs-lookup"><span data-stu-id="17a38-103">Use Azure file shares with Windows VMs</span></span> 

<span data-ttu-id="17a38-104">Sdílené složky Azure můžete použít jako způsob toostore a přístup k souborům z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="17a38-104">You can use Azure file shares as a way toostore and access files from your VM.</span></span> <span data-ttu-id="17a38-105">Můžete například uložit skript nebo konfiguračního souboru aplikace, které chcete všechny virtuální počítače tooshare.</span><span class="sxs-lookup"><span data-stu-id="17a38-105">For example, you can store a script or an application configuration file that you want all your VMs tooshare.</span></span> <span data-ttu-id="17a38-106">V tomto tématu Ukážeme vám, jak toocreate a připojení Azure pro sdílení souborů a tooupload a stahování souborů.</span><span class="sxs-lookup"><span data-stu-id="17a38-106">In this topic, we show you how toocreate and mount an Azure file share, and how tooupload and download files.</span></span>

## <a name="connect-tooa-file-share-from-a-vm"></a><span data-ttu-id="17a38-107">Připojit tooa sdílené složky z virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="17a38-107">Connect tooa file share from a VM</span></span>

<span data-ttu-id="17a38-108">V této části se předpokládá, že už máte sdílenou složku, které chcete tooconnect k.</span><span class="sxs-lookup"><span data-stu-id="17a38-108">This section assumes you already have a file share that you want tooconnect to.</span></span> <span data-ttu-id="17a38-109">Pokud potřebujete toocreate jeden, přečtěte si [vytvoření sdílené složky](#create-a-file-share) dál v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="17a38-109">If you need toocreate one, see [Create a file share](#create-a-file-share) later in this topic.</span></span>

1. <span data-ttu-id="17a38-110">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="17a38-110">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="17a38-111">V levé nabídce hello, klikněte na tlačítko **účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="17a38-111">On hello left menu, click **Storage accounts**.</span></span>
3. <span data-ttu-id="17a38-112">Vyberte svůj účet úložiště</span><span class="sxs-lookup"><span data-stu-id="17a38-112">Choose your storage account.</span></span>
4. <span data-ttu-id="17a38-113">V hello **přehled** v části **služby**, vyberte **soubory**.</span><span class="sxs-lookup"><span data-stu-id="17a38-113">In hello **Overview** page, under **Services**, select **Files**.</span></span>
5. <span data-ttu-id="17a38-114">Vyberte sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="17a38-114">Select a file share.</span></span>
6. <span data-ttu-id="17a38-115">Klikněte na tlačítko **připojit** tooopen stránku, která ukazuje hello syntaxe příkazového řádku pro sdílenou složku hello připojení ze systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="17a38-115">Click **Connect** tooopen a page that shows hello command-line syntax for mounting hello file share from Windows or Linux.</span></span>
7. <span data-ttu-id="17a38-116">Zvýrazněte hello syntaxe příkazu hello a vložte jej do programu Poznámkový blok nebo jiném místě kde můžete snadno k němu přístup.</span><span class="sxs-lookup"><span data-stu-id="17a38-116">Highlight hello syntax of hello command and paste it into Notepad or someplace else where you can easily access it.</span></span> 
8. <span data-ttu-id="17a38-117">Upravit hello syntaxe tooremove hello úvodní ** > ** a nahraďte *[písmeno jednotky]* hello písmeno jednotky (například **/Y:**) kam chcete toomount hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="17a38-117">Edit hello syntax tooremove hello leading **> ** and replace *[drive letter]* with hello drive letter (for example, **Y:**) where you would like toomount hello file share.</span></span>
8. <span data-ttu-id="17a38-118">Připojte tooyour virtuálního počítače a otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="17a38-118">Connect tooyour VM and open a command prompt.</span></span>
9. <span data-ttu-id="17a38-119">Vložením hello upravovat připojení syntaxe a stiskněte tlačítko **Enter**.</span><span class="sxs-lookup"><span data-stu-id="17a38-119">Paste in hello edited connection syntax and hit **Enter**.</span></span>
10. <span data-ttu-id="17a38-120">Po vytvoření připojení hello se zobrazí zpráva hello **hello příkaz úspěšně dokončil.**</span><span class="sxs-lookup"><span data-stu-id="17a38-120">When hello connection has been created, you get hello message **hello command completed successfully.**</span></span>
11. <span data-ttu-id="17a38-121">Zkontrolujte připojení hello zadáním hello jednotky písmeno tooswitch toothat jednotky a pak zadejte **dir** toosee hello obsah hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="17a38-121">Check hello connection by typing in hello drive letter tooswitch toothat drive and then type **dir** toosee hello contents of hello file share.</span></span>



## <a name="create-a-file-share"></a><span data-ttu-id="17a38-122">Vytvoření sdílené složky</span><span class="sxs-lookup"><span data-stu-id="17a38-122">Create a file share</span></span> 
1. <span data-ttu-id="17a38-123">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="17a38-123">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="17a38-124">V levé nabídce hello, klikněte na tlačítko **účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="17a38-124">On hello left menu, click **Storage accounts**.</span></span>
3. <span data-ttu-id="17a38-125">Vyberte svůj účet úložiště</span><span class="sxs-lookup"><span data-stu-id="17a38-125">Choose your storage account.</span></span>
4. <span data-ttu-id="17a38-126">V hello **přehled** v části **služby**, vyberte **soubory**.</span><span class="sxs-lookup"><span data-stu-id="17a38-126">In hello **Overview** page, under **Services**, select **Files**.</span></span>
5. <span data-ttu-id="17a38-127">Na stránku hello souboru služby, klikněte na **+ sdílená** toocreate sdílet první soubor. \\</span><span class="sxs-lookup"><span data-stu-id="17a38-127">In hello File Service page, click **+ File share** toocreate your first file share.\\</span></span>
6. <span data-ttu-id="17a38-128">Zadejte název sdílené složky souborů hello.</span><span class="sxs-lookup"><span data-stu-id="17a38-128">Fill in hello file share name.</span></span> <span data-ttu-id="17a38-129">Názvy sdílených složek můžete použít, malá písmena, číslice a jeden pomlčky.</span><span class="sxs-lookup"><span data-stu-id="17a38-129">File share names can use lowercase letters, numbers and single hyphens.</span></span> <span data-ttu-id="17a38-130">Hello název nemůže začínat pomlčkou a nemůže používat více po sobě jdoucí pomlčky.</span><span class="sxs-lookup"><span data-stu-id="17a38-130">hello name cannot start with a hyphen and you can't use multiple, consecutive hyphens.</span></span> 
7. <span data-ttu-id="17a38-131">Vyplňte omezení v tom, jak velký hello souboru může být až too5120 GB.</span><span class="sxs-lookup"><span data-stu-id="17a38-131">Fill in a limit on how large hello file can be, up too5120 GB.</span></span>
8. <span data-ttu-id="17a38-132">Klikněte na tlačítko **OK** toodeploy hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="17a38-132">Click **OK** toodeploy hello file share.</span></span>
   
## <a name="upload-files"></a><span data-ttu-id="17a38-133">Nahrání souborů</span><span class="sxs-lookup"><span data-stu-id="17a38-133">Upload files</span></span>
1. <span data-ttu-id="17a38-134">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="17a38-134">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="17a38-135">V levé nabídce hello, klikněte na tlačítko **účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="17a38-135">On hello left menu, click **Storage accounts**.</span></span>
3. <span data-ttu-id="17a38-136">Vyberte svůj účet úložiště</span><span class="sxs-lookup"><span data-stu-id="17a38-136">Choose your storage account.</span></span>
4. <span data-ttu-id="17a38-137">V hello **přehled** v části **služby**, vyberte **soubory**.</span><span class="sxs-lookup"><span data-stu-id="17a38-137">In hello **Overview** page, under **Services**, select **Files**.</span></span>
5. <span data-ttu-id="17a38-138">Vyberte sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="17a38-138">Select a file share.</span></span>
6. <span data-ttu-id="17a38-139">Klikněte na tlačítko **nahrát** tooopen hello **nahrání souborů** stránky.</span><span class="sxs-lookup"><span data-stu-id="17a38-139">Click **Upload** tooopen hello **Upload files** page.</span></span>
7. <span data-ttu-id="17a38-140">Klikněte na hello složky ikonu toobrowse systému místního souboru pro soubor tooupload.</span><span class="sxs-lookup"><span data-stu-id="17a38-140">Click on hello folder icon toobrowse your local file system for a file tooupload.</span></span>   
8. <span data-ttu-id="17a38-141">Klikněte na tlačítko **nahrát** tooupload hello souboru toohello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="17a38-141">Click **Upload** tooupload hello file toohello file share.</span></span>

## <a name="download-files"></a><span data-ttu-id="17a38-142">Stažení souborů</span><span class="sxs-lookup"><span data-stu-id="17a38-142">Download files</span></span>
1. <span data-ttu-id="17a38-143">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="17a38-143">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="17a38-144">V levé nabídce hello, klikněte na tlačítko **účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="17a38-144">On hello left menu, click **Storage accounts**.</span></span>
3. <span data-ttu-id="17a38-145">Vyberte svůj účet úložiště</span><span class="sxs-lookup"><span data-stu-id="17a38-145">Choose your storage account.</span></span>
4. <span data-ttu-id="17a38-146">V hello **přehled** v části **služby**, vyberte **soubory**.</span><span class="sxs-lookup"><span data-stu-id="17a38-146">In hello **Overview** page, under **Services**, select **Files**.</span></span>
5. <span data-ttu-id="17a38-147">Vyberte sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="17a38-147">Select a file share.</span></span>
6. <span data-ttu-id="17a38-148">Klikněte pravým tlačítkem na soubor hello a zvolte **Stáhnout** toodownload ho tooyour místního počítače.</span><span class="sxs-lookup"><span data-stu-id="17a38-148">Right-click on hello file and choose **Download** toodownload it tooyour local machine.</span></span>
   

## <a name="next-steps"></a><span data-ttu-id="17a38-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="17a38-149">Next steps</span></span>

<span data-ttu-id="17a38-150">Můžete také vytvořit a spravovat sdílené složky pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="17a38-150">You can also create and manage file shares using PowerShell.</span></span> <span data-ttu-id="17a38-151">Další informace najdete v tématu [Začínáme s Azure File storage ve Windows](../../storage/files/storage-dotnet-how-to-use-files.md).</span><span class="sxs-lookup"><span data-stu-id="17a38-151">For more information, see [Get started with Azure File storage on Windows](../../storage/files/storage-dotnet-how-to-use-files.md).</span></span>
