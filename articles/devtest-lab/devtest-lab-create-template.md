---
title: "Vytvoření vlastní image Azure DevTest Labs ze souboru virtuálního pevného disku | Microsoft Docs"
description: "Naučte se vytvořit vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí portálu Azure"
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
ms.openlocfilehash: 9983ea9b847f44ed18a6169a4bdb224b63626a64
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-custom-image-from-a-vhd-file"></a><span data-ttu-id="b6ac3-103">Vytvořit vlastní image ze souboru virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="b6ac3-103">Create a custom image from a VHD file</span></span>

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a><span data-ttu-id="b6ac3-104">Podrobné pokyny</span><span class="sxs-lookup"><span data-stu-id="b6ac3-104">Step-by-step instructions</span></span>

<span data-ttu-id="b6ac3-105">Následující postup vás provede procesem vytvoření vlastní image ze souboru virtuálního pevného disku pomocí portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="b6ac3-105">The following steps walk you through creating a custom image from a VHD file using the Azure portal:</span></span>

1. <span data-ttu-id="b6ac3-106">Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="b6ac3-106">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="b6ac3-107">Vyberte **Další služby** a poté ze seznamu vyberte **DevTest Labs**.</span><span class="sxs-lookup"><span data-stu-id="b6ac3-107">Select **More services**, and then select **DevTest Labs** from the list.</span></span>

1. <span data-ttu-id="b6ac3-108">Ze seznamu labs vyberte požadované testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="b6ac3-108">From the list of labs, select the desired lab.</span></span>  

1. <span data-ttu-id="b6ac3-109">V okně v prostředí, vyberte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="b6ac3-109">On the lab's blade, select **Configuration**.</span></span> 

1. <span data-ttu-id="b6ac3-110">V testovacím prostředí **konfigurace** vyberte **vlastní Image (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="b6ac3-110">On the lab **Configuration** blade, select **Custom images (VHDs)**.</span></span>

1. <span data-ttu-id="b6ac3-111">Na **vlastní image** vyberte **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="b6ac3-111">On the **Custom images** blade, select **+Add**.</span></span>

    ![Přidat vlastní obrázek](./media/devtest-lab-create-template/add-custom-image.png)

1. <span data-ttu-id="b6ac3-113">Zadejte název pro vlastní image.</span><span class="sxs-lookup"><span data-stu-id="b6ac3-113">Enter the name of the custom image.</span></span> <span data-ttu-id="b6ac3-114">Tento název se zobrazí v seznamu základní Image při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b6ac3-114">This name is displayed in the list of base images when creating a VM.</span></span>

1. <span data-ttu-id="b6ac3-115">Zadejte popis vlastní image.</span><span class="sxs-lookup"><span data-stu-id="b6ac3-115">Enter the description of the custom image.</span></span> <span data-ttu-id="b6ac3-116">Tento popis se zobrazí v seznamu základní Image při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b6ac3-116">This description is displayed in the list of base images when creating a VM.</span></span>

1. <span data-ttu-id="b6ac3-117">Vyberte **virtuálního pevného disku**.</span><span class="sxs-lookup"><span data-stu-id="b6ac3-117">Select **VHD**.</span></span>

1. <span data-ttu-id="b6ac3-118">Z **virtuálního pevného disku** okně vyberte požadovaný soubor VHD.</span><span class="sxs-lookup"><span data-stu-id="b6ac3-118">From the **VHD** blade, select the desired VHD file.</span></span>

1. <span data-ttu-id="b6ac3-119">Vyberte **OK** zavřete **virtuálního pevného disku** okno.</span><span class="sxs-lookup"><span data-stu-id="b6ac3-119">Select **OK** to close the **VHD** blade.</span></span>

1. <span data-ttu-id="b6ac3-120">Vyberte **konfigurace operačního systému**.</span><span class="sxs-lookup"><span data-stu-id="b6ac3-120">Select **OS configuration**.</span></span>

1. <span data-ttu-id="b6ac3-121">Na **konfigurace operačního systému** , vyberte buď **Windows** nebo **Linux**.</span><span class="sxs-lookup"><span data-stu-id="b6ac3-121">On the **OS configuration** tab, select either **Windows** or **Linux**.</span></span>

1. <span data-ttu-id="b6ac3-122">Pokud **Windows** je vybrána, zadejte prostřednictvím políčka zda *Sysprep* byl spuštěn v počítači.</span><span class="sxs-lookup"><span data-stu-id="b6ac3-122">If **Windows** is selected, specify via the checkbox whether *Sysprep* has been run on the machine.</span></span> 

1. <span data-ttu-id="b6ac3-123">Vyberte **OK** zavřete **konfigurace operačního systému** okno.</span><span class="sxs-lookup"><span data-stu-id="b6ac3-123">Select **OK** to close the **OS configuration** blade.</span></span>

1. <span data-ttu-id="b6ac3-124">Vyberte **OK** vytvořit vlastní image.</span><span class="sxs-lookup"><span data-stu-id="b6ac3-124">Select **OK** to create the custom image.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="b6ac3-125">Příspěvky blogu související</span><span class="sxs-lookup"><span data-stu-id="b6ac3-125">Related blog posts</span></span>

- [<span data-ttu-id="b6ac3-126">Vlastní Image nebo vzorce?</span><span class="sxs-lookup"><span data-stu-id="b6ac3-126">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [<span data-ttu-id="b6ac3-127">Kopírování vlastních bitových kopií mezi Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="b6ac3-127">Copying Custom Images between Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a><span data-ttu-id="b6ac3-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b6ac3-128">Next steps</span></span>

- [<span data-ttu-id="b6ac3-129">Přidejte virtuální počítač do testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="b6ac3-129">Add a VM to your lab</span></span>](./devtest-lab-add-vm-with-artifacts.md)
