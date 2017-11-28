---
title: "aaaCreate vlastní obrázek ze souboru virtuálního pevného disku Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak hello toocreate vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí portálu Azure"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: b795bc61-7c28-40e6-82fc-96d629ee0568
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 80af8ea1cb72380f868df0a76c4a0dcd92e63cf5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-from-a-vhd-file"></a><span data-ttu-id="a2f3f-103">Vytvořit vlastní image ze souboru virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="a2f3f-103">Create a custom image from a VHD file</span></span>

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a><span data-ttu-id="a2f3f-104">Podrobné pokyny</span><span class="sxs-lookup"><span data-stu-id="a2f3f-104">Step-by-step instructions</span></span>

<span data-ttu-id="a2f3f-105">Hello následující postup vás provede procesem vytvoření vlastní image ze souboru virtuálního pevného disku pomocí hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="a2f3f-105">hello following steps walk you through creating a custom image from a VHD file using hello Azure portal:</span></span>

1. <span data-ttu-id="a2f3f-106">Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="a2f3f-106">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="a2f3f-107">Vyberte **další služby**a potom vyberte **DevTest Labs** hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="a2f3f-107">Select **More services**, and then select **DevTest Labs** from hello list.</span></span>

1. <span data-ttu-id="a2f3f-108">Ze seznamu hello labs vyberte požadované prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="a2f3f-108">From hello list of labs, select hello desired lab.</span></span>  

1. <span data-ttu-id="a2f3f-109">V okně prostředí hello vyberte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="a2f3f-109">On hello lab's blade, select **Configuration**.</span></span> 

1. <span data-ttu-id="a2f3f-110">V testovacím hello **konfigurace** vyberte **vlastní Image (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="a2f3f-110">On hello lab **Configuration** blade, select **Custom images (VHDs)**.</span></span>

1. <span data-ttu-id="a2f3f-111">Na hello **vlastní image** vyberte **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="a2f3f-111">On hello **Custom images** blade, select **+Add**.</span></span>

    ![Přidat vlastní obrázek](./media/devtest-lab-create-template/add-custom-image.png)

1. <span data-ttu-id="a2f3f-113">Zadejte název hello hello vlastní Image.</span><span class="sxs-lookup"><span data-stu-id="a2f3f-113">Enter hello name of hello custom image.</span></span> <span data-ttu-id="a2f3f-114">Tento název se zobrazí v seznamu hello základní Image při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a2f3f-114">This name is displayed in hello list of base images when creating a VM.</span></span>

1. <span data-ttu-id="a2f3f-115">Zadejte popis hello hello vlastní image.</span><span class="sxs-lookup"><span data-stu-id="a2f3f-115">Enter hello description of hello custom image.</span></span> <span data-ttu-id="a2f3f-116">Tento popis se zobrazí v seznamu hello základní Image při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a2f3f-116">This description is displayed in hello list of base images when creating a VM.</span></span>

1. <span data-ttu-id="a2f3f-117">Vyberte **virtuálního pevného disku**.</span><span class="sxs-lookup"><span data-stu-id="a2f3f-117">Select **VHD**.</span></span>

1. <span data-ttu-id="a2f3f-118">Z hello **virtuálního pevného disku** okně, vyberte hello požadovaného souboru virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="a2f3f-118">From hello **VHD** blade, select hello desired VHD file.</span></span>

1. <span data-ttu-id="a2f3f-119">Vyberte **OK** tooclose hello **virtuálního pevného disku** okno.</span><span class="sxs-lookup"><span data-stu-id="a2f3f-119">Select **OK** tooclose hello **VHD** blade.</span></span>

1. <span data-ttu-id="a2f3f-120">Vyberte **konfigurace operačního systému**.</span><span class="sxs-lookup"><span data-stu-id="a2f3f-120">Select **OS configuration**.</span></span>

1. <span data-ttu-id="a2f3f-121">Na hello **konfigurace operačního systému** , vyberte buď **Windows** nebo **Linux**.</span><span class="sxs-lookup"><span data-stu-id="a2f3f-121">On hello **OS configuration** tab, select either **Windows** or **Linux**.</span></span>

1. <span data-ttu-id="a2f3f-122">Pokud **Windows** je vybrána, zadejte prostřednictvím hello políčko zda *Sysprep* byl spuštěn na počítači hello.</span><span class="sxs-lookup"><span data-stu-id="a2f3f-122">If **Windows** is selected, specify via hello checkbox whether *Sysprep* has been run on hello machine.</span></span> 

1. <span data-ttu-id="a2f3f-123">Vyberte **OK** tooclose hello **konfigurace operačního systému** okno.</span><span class="sxs-lookup"><span data-stu-id="a2f3f-123">Select **OK** tooclose hello **OS configuration** blade.</span></span>

1. <span data-ttu-id="a2f3f-124">Vyberte **OK** toocreate hello vlastní image.</span><span class="sxs-lookup"><span data-stu-id="a2f3f-124">Select **OK** toocreate hello custom image.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="a2f3f-125">Příspěvky blogu související</span><span class="sxs-lookup"><span data-stu-id="a2f3f-125">Related blog posts</span></span>

- [<span data-ttu-id="a2f3f-126">Vlastní Image nebo vzorce?</span><span class="sxs-lookup"><span data-stu-id="a2f3f-126">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [<span data-ttu-id="a2f3f-127">Kopírování vlastních bitových kopií mezi Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="a2f3f-127">Copying Custom Images between Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a><span data-ttu-id="a2f3f-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a2f3f-128">Next steps</span></span>

- [<span data-ttu-id="a2f3f-129">Přidání testovacího prostředí tooyour virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="a2f3f-129">Add a VM tooyour lab</span></span>](./devtest-lab-add-vm-with-artifacts.md)
