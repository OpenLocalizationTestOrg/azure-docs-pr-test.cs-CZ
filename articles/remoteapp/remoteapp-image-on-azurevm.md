---
title: "Vytvoření image Azure Remoteappu založené na virtuálním počítači Azure | Microsoft Docs"
description: "Naučte se vytvořit bitovou kopii pro Azure RemoteApp od virtuálního počítače Azure."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: d41583ef-6cd8-4115-8dcb-b2cd5b3d301a
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: ee64b86835af8e6237cddcd8acc779fc6dac8214
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-azure-remoteapp-image-based-on-an-azure-virtual-machine"></a><span data-ttu-id="b2305-103">Vytvoření image Azure Remoteappu založené na virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="b2305-103">Create a Azure RemoteApp image based on an Azure virtual machine</span></span>
> [!IMPORTANT]
> <span data-ttu-id="b2305-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="b2305-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="b2305-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="b2305-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="b2305-106">Z virtuálního počítače Azure můžete vytvořit Image Azure Remoteappu, (které s aplikacemi, které sdílíte v kolekci).</span><span class="sxs-lookup"><span data-stu-id="b2305-106">You can create Azure RemoteApp images (which hold the apps you share in your collection) from an Azure virtual machine.</span></span> <span data-ttu-id="b2305-107">Může se také rozhodnout použít bitovou kopii virtuálního počítače, kterou jsme přidali do Galerie bitové kopie virtuálního počítače Azure, který splňuje všechny požadavky image Azure Remoteappu – můžete použít této bitové kopie virtuálních počítačů jako výchozí bod pro vlastní virtuální počítač, pokud chcete.</span><span class="sxs-lookup"><span data-stu-id="b2305-107">You could also choose to use a virtual machine image we added to the Azure VM image gallery that meets all the Azure RemoteApp image requirements - you can use that VM image as a starting point for your own VM, if you want.</span></span> <span data-ttu-id="b2305-108">Právě hledejte bitovou kopii "Windows Server hostitele relace vzdálené plochy" v knihovně.</span><span class="sxs-lookup"><span data-stu-id="b2305-108">Just look for the "Windows Server Remote Desktop Session Host" image in the library.</span></span>

<span data-ttu-id="b2305-109">Existují dva kroky pro vytvoření vlastní image založené na virtuální počítač Azure – vytvoření bitové kopie a nahrajte ho z knihovny virtuálního počítače Azure do Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="b2305-109">There are two steps to create your own image based on an Azure VM - create the image and then upload it from the Azure VM library to Azure RemoteApp.</span></span>

## <a name="create-a-custom-image-based-on-an-azure-vm"></a><span data-ttu-id="b2305-110">Vytvořit vlastní image založené na virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="b2305-110">Create a custom image based on an Azure VM</span></span>
<span data-ttu-id="b2305-111">Tyto kroky použijte pro vytvoření bitové kopie založené na virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="b2305-111">Use these steps to create an image based on an Azure VM.</span></span>

1. <span data-ttu-id="b2305-112">Vytvořte virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="b2305-112">Create an Azure virtual machine.</span></span> <span data-ttu-id="b2305-113">Můžete použít "Windows Server hostitele relace vzdálené plochy" nebo "Windows Server vzdálené plochy relace hostitele s Microsoft Office 365 ProPlus" bitové kopie z Galerie obrázků virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="b2305-113">You can use the "Windows Server Remote Desktop Session Host" or the "Windows Server Remote Desktop Session Host with Microsoft Office 365 ProPlus" image from the Azure virtual machine image gallery.</span></span> <span data-ttu-id="b2305-114">Tento image splňuje všechny požadavky image Azure Remoteappu šablony.</span><span class="sxs-lookup"><span data-stu-id="b2305-114">This image meets all the Azure RemoteApp template image requirements.</span></span>
   
    <span data-ttu-id="b2305-115">Podrobnosti najdete v tématu [vytvoření virtuálního počítače s Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b2305-115">For details, see [Create a VM running Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
2. <span data-ttu-id="b2305-116">Připojení k virtuálnímu počítači a nainstalujte a nakonfigurujte aplikace, které chcete sdílet prostřednictvím vzdálené aplikace RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="b2305-116">Connect to the VM and install and configure the apps that you want to share through RemoteApp.</span></span> <span data-ttu-id="b2305-117">Ujistěte se, že provádět žádné další konfigurace Windows vyžaduje vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="b2305-117">Make sure to perform any additional Windows configurations required by your apps.</span></span>
   
    <span data-ttu-id="b2305-118">Podrobnosti najdete v tématu [postup Přihlaste se k Windows serveru spuštěný virtuální počítač](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b2305-118">For details, see [How to Log on to a Virtual Machine Running Windows Server](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>
3. <span data-ttu-id="b2305-119">Pokud použijete jednu z imagí hostitele pro relace vzdálené plochy systému Windows Server, je ověření zahrnuté skript, který zajistí, že virtuální počítač splňuje pre-reqs. vzdálené aplikace RemoteApp</span><span class="sxs-lookup"><span data-stu-id="b2305-119">If you are using one of the Windows Server Remote Desktop Session Host images, there is an included validation script that will ensure your VM meets the RemoteApp pre-reqs.</span></span> <span data-ttu-id="b2305-120">Chcete-li spustit skript, dvakrát klikněte na **ValidateRemoteAppImage** na ploše.</span><span class="sxs-lookup"><span data-stu-id="b2305-120">To run script, double-click **ValidateRemoteAppImage** on the desktop.</span></span> <span data-ttu-id="b2305-121">Ujistěte se, zda jsou před pokračováním k dalšímu kroku opravit všechny chyby, které skript nahlásí.</span><span class="sxs-lookup"><span data-stu-id="b2305-121">Ensure that all errors reported by the script are fixed before proceeding to the next step.</span></span>
4. <span data-ttu-id="b2305-122">Nástroj SYSPREP generalize a zachycení bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="b2305-122">SYSPREP generalize and capture the image.</span></span> <span data-ttu-id="b2305-123">V tématu [jak zachytit virtuální počítač Windows pro použití jako šablona](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) pokyny.</span><span class="sxs-lookup"><span data-stu-id="b2305-123">See [How to Capture a Windows Virtual Machine to Use as a Template](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) for instructions.</span></span>

## <a name="import-the-image-into-the-azure-remoteapp-image-library"></a><span data-ttu-id="b2305-124">Import bitovou kopii do bitové kopie knihovny Azure Remoteappu</span><span class="sxs-lookup"><span data-stu-id="b2305-124">Import the image into the Azure RemoteApp image library</span></span>
<span data-ttu-id="b2305-125">Importovat novou bitovou kopii do Azure RemoteApp pomocí těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="b2305-125">Use these steps to import the new image into Azure RemoteApp:</span></span>

1. <span data-ttu-id="b2305-126">V **Image šablony** karty:</span><span class="sxs-lookup"><span data-stu-id="b2305-126">In the **Template Images** tab:</span></span>
   
   * <span data-ttu-id="b2305-127">Pokud máte existující se žádné Image, klikněte na tlačítko **nahrát nebo importovat Image šablony**.</span><span class="sxs-lookup"><span data-stu-id="b2305-127">If you have no existing images, click **Upload or Import a Template Image**.</span></span>
   * <span data-ttu-id="b2305-128">Pokud již máte alespoň jednu bitovou kopii, klikněte na tlačítko  **+**  přidat novou bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="b2305-128">If you have at least one image already, click **+** to add a new image.</span></span>
2. <span data-ttu-id="b2305-129">Vyberte **importovat bitovou kopii z virtuálních počítačů** knihovny a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="b2305-129">Select **Import an image from your Virtual Machines** library, and then click **Next**.</span></span>
3. <span data-ttu-id="b2305-130">Na další stránce ze seznamu vyberte vlastní image a potvrďte, že jste postupovali podle kroků uvedených při vytváření bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="b2305-130">On the next page, select your custom image from the list and confirm that you followed the steps listed when you created your image.</span></span> <span data-ttu-id="b2305-131">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="b2305-131">Click **Next**.</span></span>
4. <span data-ttu-id="b2305-132">Zadejte název pro novou bitovou kopii vzdálené aplikace RemoteApp a vyberte umístění a pak kliknutím na značku zaškrtnutí zahájíte proces importu.</span><span class="sxs-lookup"><span data-stu-id="b2305-132">Enter a name for the new RemoteApp image and pick the location, then click the checkmark to start the import process.</span></span>

> [!NOTE]
> <span data-ttu-id="b2305-133">Bitové kopie můžete importovat z libovolného místa a Azure podporována ve virtuálních počítačích Azure do libovolného umístění Azure, Azure RemoteApp nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="b2305-133">You can import images from any Azure location supported by Azure Virtual Machines to any Azure location supported by Azure RemoteApp.</span></span> <span data-ttu-id="b2305-134">V závislosti na umístění importu může trvat až 25.</span><span class="sxs-lookup"><span data-stu-id="b2305-134">Depending on the locations the import can take up to 25 minutes.</span></span>
> 
> 

<span data-ttu-id="b2305-135">Nyní jste připraveni k vytvoření nové kolekce, buď [cloudu](remoteapp-create-cloud-deployment.md) kolekce nebo [hybridní](remoteapp-create-hybrid-deployment.md), v závislosti na vašich potřebách.</span><span class="sxs-lookup"><span data-stu-id="b2305-135">Now you are ready to create your new collection, either a [cloud](remoteapp-create-cloud-deployment.md) collection or [hybrid](remoteapp-create-hybrid-deployment.md), depending on your needs.</span></span>

