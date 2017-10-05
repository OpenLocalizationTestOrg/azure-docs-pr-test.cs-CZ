---
title: "Připojení Azure úložiště file z virtuálního počítače Windows Azure | Microsoft Docs"
description: "Uložte soubor v cloudu s Azure file storage a připojte svou cloudovou sdílenou z Azure virtuálního počítače (VM)."
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
ms.openlocfilehash: 6ffb2d2da1e2439df6f5da543411e3c2c68d3435
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-file-shares-with-windows-vms"></a><span data-ttu-id="4325b-103">Použít sdílené složky Azure s virtuálními počítači Windows</span><span class="sxs-lookup"><span data-stu-id="4325b-103">Use Azure file shares with Windows VMs</span></span> 

<span data-ttu-id="4325b-104">Sdílené složky Azure můžete použít jako způsob, jak ukládat a přistupovat k souborům z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4325b-104">You can use Azure file shares as a way to store and access files from your VM.</span></span> <span data-ttu-id="4325b-105">Můžete například uložit skript nebo konfiguračního souboru aplikace, které chcete všechny virtuální počítače do sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="4325b-105">For example, you can store a script or an application configuration file that you want all your VMs to share.</span></span> <span data-ttu-id="4325b-106">V tomto tématu jsme ukážeme, jak vytvořit a připojit sdílenou složku Azure a jak pro nahrávání a stahování souborů.</span><span class="sxs-lookup"><span data-stu-id="4325b-106">In this topic, we show you how to create and mount an Azure file share, and how to upload and download files.</span></span>

## <a name="connect-to-a-file-share-from-a-vm"></a><span data-ttu-id="4325b-107">Připojení ke sdílené složce z virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="4325b-107">Connect to a file share from a VM</span></span>

<span data-ttu-id="4325b-108">V této části se předpokládá, že už máte sdílenou složku, kterou chcete připojit k.</span><span class="sxs-lookup"><span data-stu-id="4325b-108">This section assumes you already have a file share that you want to connect to.</span></span> <span data-ttu-id="4325b-109">Pokud potřebujete vytvořit, přečtěte si téma [vytvoření sdílené složky](#create-a-file-share) dál v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="4325b-109">If you need to create one, see [Create a file share](#create-a-file-share) later in this topic.</span></span>

1. <span data-ttu-id="4325b-110">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4325b-110">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="4325b-111">V nabídce vlevo klikněte na tlačítko **účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="4325b-111">On the left menu, click **Storage accounts**.</span></span>
3. <span data-ttu-id="4325b-112">Vyberte svůj účet úložiště</span><span class="sxs-lookup"><span data-stu-id="4325b-112">Choose your storage account.</span></span>
4. <span data-ttu-id="4325b-113">V **přehled** v části **služby**, vyberte **soubory**.</span><span class="sxs-lookup"><span data-stu-id="4325b-113">In the **Overview** page, under **Services**, select **Files**.</span></span>
5. <span data-ttu-id="4325b-114">Vyberte sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="4325b-114">Select a file share.</span></span>
6. <span data-ttu-id="4325b-115">Klikněte na tlačítko **Connect** otevřete stránku, která ukazuje syntaxi příkazového řádku pro připojení sdílené složky z Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="4325b-115">Click **Connect** to open a page that shows the command-line syntax for mounting the file share from Windows or Linux.</span></span>
7. <span data-ttu-id="4325b-116">Zvýraznění syntaxe příkazu a vložte jej do programu Poznámkový blok nebo jiném místě kde můžete snadno k němu přístup.</span><span class="sxs-lookup"><span data-stu-id="4325b-116">Highlight the syntax of the command and paste it into Notepad or someplace else where you can easily access it.</span></span> 
8. <span data-ttu-id="4325b-117">Upravit syntaxe odebrat úvodního ** > ** a nahraďte *[písmeno jednotky]* písmeno jednotky (například **/Y:**) kde byste chtěli připojit sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="4325b-117">Edit the syntax to remove the leading **> ** and replace *[drive letter]* with the drive letter (for example, **Y:**) where you would like to mount the file share.</span></span>
8. <span data-ttu-id="4325b-118">Připojte k virtuálnímu počítači a otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="4325b-118">Connect to your VM and open a command prompt.</span></span>
9. <span data-ttu-id="4325b-119">Vložte v syntaxi upravená připojení a stiskněte tlačítko **Enter**.</span><span class="sxs-lookup"><span data-stu-id="4325b-119">Paste in the edited connection syntax and hit **Enter**.</span></span>
10. <span data-ttu-id="4325b-120">Po vytvoření připojení, se zobrazí zpráva **příkaz úspěšně dokončil.**</span><span class="sxs-lookup"><span data-stu-id="4325b-120">When the connection has been created, you get the message **The command completed successfully.**</span></span>
11. <span data-ttu-id="4325b-121">Zkontrolujte připojení a to zadáním písmeno jednotky přepnout na tuto jednotku a pak zadejte **dir** zobrazí se obsah sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="4325b-121">Check the connection by typing in the drive letter to switch to that drive and then type **dir** to see the contents of the file share.</span></span>



## <a name="create-a-file-share"></a><span data-ttu-id="4325b-122">Vytvoření sdílené složky</span><span class="sxs-lookup"><span data-stu-id="4325b-122">Create a file share</span></span> 
1. <span data-ttu-id="4325b-123">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4325b-123">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="4325b-124">V nabídce vlevo klikněte na tlačítko **účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="4325b-124">On the left menu, click **Storage accounts**.</span></span>
3. <span data-ttu-id="4325b-125">Vyberte svůj účet úložiště</span><span class="sxs-lookup"><span data-stu-id="4325b-125">Choose your storage account.</span></span>
4. <span data-ttu-id="4325b-126">V **přehled** v části **služby**, vyberte **soubory**.</span><span class="sxs-lookup"><span data-stu-id="4325b-126">In the **Overview** page, under **Services**, select **Files**.</span></span>
5. <span data-ttu-id="4325b-127">Na stránce služby souborů, klikněte na tlačítko **+ sdílená** vytvořit svou první sdílenou. \\</span><span class="sxs-lookup"><span data-stu-id="4325b-127">In the File Service page, click **+ File share** to create your first file share.\\</span></span>
6. <span data-ttu-id="4325b-128">Zadejte název sdílené složky souborů.</span><span class="sxs-lookup"><span data-stu-id="4325b-128">Fill in the file share name.</span></span> <span data-ttu-id="4325b-129">Názvy sdílených složek můžete použít, malá písmena, číslice a jeden pomlčky.</span><span class="sxs-lookup"><span data-stu-id="4325b-129">File share names can use lowercase letters, numbers and single hyphens.</span></span> <span data-ttu-id="4325b-130">Název nemůže začínat pomlčkou a nemůže používat více po sobě jdoucí pomlčky.</span><span class="sxs-lookup"><span data-stu-id="4325b-130">The name cannot start with a hyphen and you can't use multiple, consecutive hyphens.</span></span> 
7. <span data-ttu-id="4325b-131">Vyplňte omezení na tom, jak velký soubor může být, až 5120 GB.</span><span class="sxs-lookup"><span data-stu-id="4325b-131">Fill in a limit on how large the file can be, up to 5120 GB.</span></span>
8. <span data-ttu-id="4325b-132">Klikněte na tlačítko **OK** nasazení sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="4325b-132">Click **OK** to deploy the file share.</span></span>
   
## <a name="upload-files"></a><span data-ttu-id="4325b-133">Nahrání souborů</span><span class="sxs-lookup"><span data-stu-id="4325b-133">Upload files</span></span>
1. <span data-ttu-id="4325b-134">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4325b-134">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="4325b-135">V nabídce vlevo klikněte na tlačítko **účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="4325b-135">On the left menu, click **Storage accounts**.</span></span>
3. <span data-ttu-id="4325b-136">Vyberte svůj účet úložiště</span><span class="sxs-lookup"><span data-stu-id="4325b-136">Choose your storage account.</span></span>
4. <span data-ttu-id="4325b-137">V **přehled** v části **služby**, vyberte **soubory**.</span><span class="sxs-lookup"><span data-stu-id="4325b-137">In the **Overview** page, under **Services**, select **Files**.</span></span>
5. <span data-ttu-id="4325b-138">Vyberte sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="4325b-138">Select a file share.</span></span>
6. <span data-ttu-id="4325b-139">Klikněte na tlačítko **nahrát** otevřete **nahrání souborů** stránky.</span><span class="sxs-lookup"><span data-stu-id="4325b-139">Click **Upload** to open the **Upload files** page.</span></span>
7. <span data-ttu-id="4325b-140">Klikněte na ikonu složky a přejděte do místního systému souborů pro soubor k odeslání.</span><span class="sxs-lookup"><span data-stu-id="4325b-140">Click on the folder icon to browse your local file system for a file to upload.</span></span>   
8. <span data-ttu-id="4325b-141">Klikněte na tlačítko **nahrát** nahrát soubor do sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="4325b-141">Click **Upload** to upload the file to the file share.</span></span>

## <a name="download-files"></a><span data-ttu-id="4325b-142">Stažení souborů</span><span class="sxs-lookup"><span data-stu-id="4325b-142">Download files</span></span>
1. <span data-ttu-id="4325b-143">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4325b-143">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="4325b-144">V nabídce vlevo klikněte na tlačítko **účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="4325b-144">On the left menu, click **Storage accounts**.</span></span>
3. <span data-ttu-id="4325b-145">Vyberte svůj účet úložiště</span><span class="sxs-lookup"><span data-stu-id="4325b-145">Choose your storage account.</span></span>
4. <span data-ttu-id="4325b-146">V **přehled** v části **služby**, vyberte **soubory**.</span><span class="sxs-lookup"><span data-stu-id="4325b-146">In the **Overview** page, under **Services**, select **Files**.</span></span>
5. <span data-ttu-id="4325b-147">Vyberte sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="4325b-147">Select a file share.</span></span>
6. <span data-ttu-id="4325b-148">Klikněte pravým tlačítkem na soubor a vyberte **Stáhnout** se stáhne do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="4325b-148">Right-click on the file and choose **Download** to download it to your local machine.</span></span>
   

## <a name="next-steps"></a><span data-ttu-id="4325b-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4325b-149">Next steps</span></span>

<span data-ttu-id="4325b-150">Můžete také vytvořit a spravovat sdílené složky pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4325b-150">You can also create and manage file shares using PowerShell.</span></span> <span data-ttu-id="4325b-151">Další informace najdete v tématu [Začínáme s Azure File storage ve Windows](../../storage/files/storage-dotnet-how-to-use-files.md).</span><span class="sxs-lookup"><span data-stu-id="4325b-151">For more information, see [Get started with Azure File storage on Windows](../../storage/files/storage-dotnet-how-to-use-files.md).</span></span>
