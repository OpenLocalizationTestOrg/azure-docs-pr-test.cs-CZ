---
title: "aaaCreate na virtuální počítač Azure na základě image Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak toocreate obrázek pro Azure RemoteApp od virtuálního počítače Azure."
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
ms.openlocfilehash: 2d432bcb15be68a2946d91b5f36f41d980726338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-azure-remoteapp-image-based-on-an-azure-virtual-machine"></a><span data-ttu-id="b8847-103">Vytvoření image Azure Remoteappu založené na virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="b8847-103">Create a Azure RemoteApp image based on an Azure virtual machine</span></span>
> [!IMPORTANT]
> <span data-ttu-id="b8847-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="b8847-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="b8847-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="b8847-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="b8847-106">Z virtuálního počítače Azure můžete vytvořit Image Azure Remoteappu, (které uložení hello aplikace, které sdílíte v kolekci).</span><span class="sxs-lookup"><span data-stu-id="b8847-106">You can create Azure RemoteApp images (which hold hello apps you share in your collection) from an Azure virtual machine.</span></span> <span data-ttu-id="b8847-107">Může také vybrat toouse jsme přidali toohello Galerie obrázků virtuálního počítače Azure, která splňuje všechny požadavky na image Azure Remoteappu hello image virtuálního počítače – můžete použít této bitové kopie virtuálních počítačů jako výchozí bod pro vlastní virtuální počítač, pokud chcete.</span><span class="sxs-lookup"><span data-stu-id="b8847-107">You could also choose toouse a virtual machine image we added toohello Azure VM image gallery that meets all hello Azure RemoteApp image requirements - you can use that VM image as a starting point for your own VM, if you want.</span></span> <span data-ttu-id="b8847-108">Vyhledejte právě hello "Windows Server hostitele relace vzdálené plochy" image v hello knihovně.</span><span class="sxs-lookup"><span data-stu-id="b8847-108">Just look for hello "Windows Server Remote Desktop Session Host" image in hello library.</span></span>

<span data-ttu-id="b8847-109">Existují dva kroky toocreate vlastní image založené na virtuální počítač Azure – vytvoření hello bitové kopie a nahrajte ho z hello virtuálního počítače Azure knihovna tooAzure vzdálené aplikace RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="b8847-109">There are two steps toocreate your own image based on an Azure VM - create hello image and then upload it from hello Azure VM library tooAzure RemoteApp.</span></span>

## <a name="create-a-custom-image-based-on-an-azure-vm"></a><span data-ttu-id="b8847-110">Vytvořit vlastní image založené na virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="b8847-110">Create a custom image based on an Azure VM</span></span>
<span data-ttu-id="b8847-111">Pomocí těchto kroků toocreate image založené na virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="b8847-111">Use these steps toocreate an image based on an Azure VM.</span></span>

1. <span data-ttu-id="b8847-112">Vytvořte virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="b8847-112">Create an Azure virtual machine.</span></span> <span data-ttu-id="b8847-113">Můžete použít hello "Windows Server hostitele relace vzdálené plochy" nebo "Windows Server vzdálené plochy relace hostitele s Microsoft Office 365 ProPlus" image hello z Galerie obrázků hello virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="b8847-113">You can use hello "Windows Server Remote Desktop Session Host" or hello "Windows Server Remote Desktop Session Host with Microsoft Office 365 ProPlus" image from hello Azure virtual machine image gallery.</span></span> <span data-ttu-id="b8847-114">Tuto bitovou kopii splňuje požadavky image šablony vzdálené aplikace Azure RemoteApp na všechny hello.</span><span class="sxs-lookup"><span data-stu-id="b8847-114">This image meets all hello Azure RemoteApp template image requirements.</span></span>
   
    <span data-ttu-id="b8847-115">Podrobnosti najdete v tématu [vytvoření virtuálního počítače s Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b8847-115">For details, see [Create a VM running Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
2. <span data-ttu-id="b8847-116">Připojit toohello virtuálních počítačů a nainstalujte a nakonfigurujte hello aplikace, které chcete tooshare prostřednictvím vzdálené aplikace RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="b8847-116">Connect toohello VM and install and configure hello apps that you want tooshare through RemoteApp.</span></span> <span data-ttu-id="b8847-117">Ujistěte se, že tooperform žádné další konfigurace Windows vyžaduje vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="b8847-117">Make sure tooperform any additional Windows configurations required by your apps.</span></span>
   
    <span data-ttu-id="b8847-118">Podrobnosti najdete v tématu [jak tooLog na tooa virtuální počítač spuštěný Windows Server](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b8847-118">For details, see [How tooLog on tooa Virtual Machine Running Windows Server](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>
3. <span data-ttu-id="b8847-119">Pokud použijete jednu z imagí hello hostitele pro relace vzdálené plochy systému Windows Server, je ověření zahrnuté skript, který zajistí, že virtuální počítač splňuje hello vzdálené aplikace RemoteApp pre-reqs.</span><span class="sxs-lookup"><span data-stu-id="b8847-119">If you are using one of hello Windows Server Remote Desktop Session Host images, there is an included validation script that will ensure your VM meets hello RemoteApp pre-reqs.</span></span> <span data-ttu-id="b8847-120">toorun skriptu, klikněte dvakrát na **ValidateRemoteAppImage** na ploše hello.</span><span class="sxs-lookup"><span data-stu-id="b8847-120">toorun script, double-click **ValidateRemoteAppImage** on hello desktop.</span></span> <span data-ttu-id="b8847-121">Ujistěte se, zda jsou před pokračováním toohello další krok opravit všechny chyby hlášené hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="b8847-121">Ensure that all errors reported by hello script are fixed before proceeding toohello next step.</span></span>
4. <span data-ttu-id="b8847-122">Nástroj SYSPREP generalize a zachycení bitové kopie hello.</span><span class="sxs-lookup"><span data-stu-id="b8847-122">SYSPREP generalize and capture hello image.</span></span> <span data-ttu-id="b8847-123">V tématu [jak tooCapture tooUse virtuálního počítače Windows jako šablona](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) pokyny.</span><span class="sxs-lookup"><span data-stu-id="b8847-123">See [How tooCapture a Windows Virtual Machine tooUse as a Template](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) for instructions.</span></span>

## <a name="import-hello-image-into-hello-azure-remoteapp-image-library"></a><span data-ttu-id="b8847-124">Importovat hello bitovou kopii do knihovny image Azure Remoteappu hello</span><span class="sxs-lookup"><span data-stu-id="b8847-124">Import hello image into hello Azure RemoteApp image library</span></span>
<span data-ttu-id="b8847-125">Použijte tyto kroky tooimport hello novou bitovou kopii do Azure RemoteApp:</span><span class="sxs-lookup"><span data-stu-id="b8847-125">Use these steps tooimport hello new image into Azure RemoteApp:</span></span>

1. <span data-ttu-id="b8847-126">V hello **Image šablony** karty:</span><span class="sxs-lookup"><span data-stu-id="b8847-126">In hello **Template Images** tab:</span></span>
   
   * <span data-ttu-id="b8847-127">Pokud máte existující se žádné Image, klikněte na tlačítko **nahrát nebo importovat Image šablony**.</span><span class="sxs-lookup"><span data-stu-id="b8847-127">If you have no existing images, click **Upload or Import a Template Image**.</span></span>
   * <span data-ttu-id="b8847-128">Pokud již máte alespoň jednu bitovou kopii, klikněte na tlačítko  **+**  tooadd novou bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="b8847-128">If you have at least one image already, click **+** tooadd a new image.</span></span>
2. <span data-ttu-id="b8847-129">Vyberte **importovat bitovou kopii z virtuálních počítačů** knihovny a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="b8847-129">Select **Import an image from your Virtual Machines** library, and then click **Next**.</span></span>
3. <span data-ttu-id="b8847-130">Na další stránku hello vyberte ze seznamu hello vlastní image a potvrďte, že jste udělali kroky hello uvedené při vytváření bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="b8847-130">On hello next page, select your custom image from hello list and confirm that you followed hello steps listed when you created your image.</span></span> <span data-ttu-id="b8847-131">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="b8847-131">Click **Next**.</span></span>
4. <span data-ttu-id="b8847-132">Zadejte název nové image vzdálené aplikace RemoteApp hello a vyberte hello umístění a pak klikněte na hello zaškrtnutí toostart hello importu.</span><span class="sxs-lookup"><span data-stu-id="b8847-132">Enter a name for hello new RemoteApp image and pick hello location, then click hello checkmark toostart hello import process.</span></span>

> [!NOTE]
> <span data-ttu-id="b8847-133">Bitové kopie můžete importovat z libovolného místa a Azure nepodporuje virtuální počítače Azure tooany umístění Azure, které podporuje Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="b8847-133">You can import images from any Azure location supported by Azure Virtual Machines tooany Azure location supported by Azure RemoteApp.</span></span> <span data-ttu-id="b8847-134">V závislosti na umístění hello hello importu může trvat too25 minut.</span><span class="sxs-lookup"><span data-stu-id="b8847-134">Depending on hello locations hello import can take up too25 minutes.</span></span>
> 
> 

<span data-ttu-id="b8847-135">Nyní jste připraveni toocreate nové kolekce, buď [cloudu](remoteapp-create-cloud-deployment.md) kolekce nebo [hybridní](remoteapp-create-hybrid-deployment.md), v závislosti na vašich potřebách.</span><span class="sxs-lookup"><span data-stu-id="b8847-135">Now you are ready toocreate your new collection, either a [cloud](remoteapp-create-cloud-deployment.md) collection or [hybrid](remoteapp-create-hybrid-deployment.md), depending on your needs.</span></span>

