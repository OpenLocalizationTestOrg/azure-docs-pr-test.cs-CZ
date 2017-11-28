---
title: "Z virtuálního počítače vytvořit vlastní image Azure DevTest Labs | Microsoft Docs"
description: "Naučte se vytvořit vlastní image v Azure DevTest Labs ze zřízeného virtuálního počítače pomocí portálu Azure"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 9d2dcf7164985508d691e8a0c123efaf3b8aa19a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-custom-image-from-a-vm"></a><span data-ttu-id="d1bee-103">Vytvořit vlastní image z virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="d1bee-103">Create a custom image from a VM</span></span>

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a><span data-ttu-id="d1bee-104">Podrobné pokyny</span><span class="sxs-lookup"><span data-stu-id="d1bee-104">Step-by-step instructions</span></span>

<span data-ttu-id="d1bee-105">Můžete vytvořit vlastní image ze zřízeného virtuálního počítače a pak použít tento vlastní bitovou kopii k Vytvořte identické virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="d1bee-105">You can create a custom image from a provisioned VM, and afterwards use that custom image to create identical VMs.</span></span> <span data-ttu-id="d1bee-106">Následující kroky ukazují, jak vytvořit vlastní image z virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="d1bee-106">The following steps illustrate how to create a custom image from a VM:</span></span>

1. <span data-ttu-id="d1bee-107">Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="d1bee-107">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="d1bee-108">Vyberte **Další služby** a poté ze seznamu vyberte **DevTest Labs**.</span><span class="sxs-lookup"><span data-stu-id="d1bee-108">Select **More services**, and then select **DevTest Labs** from the list.</span></span>

1. <span data-ttu-id="d1bee-109">Ze seznamu labs vyberte požadované testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="d1bee-109">From the list of labs, select the desired lab.</span></span>  

1. <span data-ttu-id="d1bee-110">V okně v prostředí, vyberte **Můj virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="d1bee-110">On the lab's blade, select **My virtual machines**.</span></span>
 
1. <span data-ttu-id="d1bee-111">Na **Můj virtuální počítače** okně vyberte virtuální počítač, ze kterého chcete vytvořit vlastní image.</span><span class="sxs-lookup"><span data-stu-id="d1bee-111">On the **My virtual machines** blade, select the VM from which you want to create the custom image.</span></span>

1. <span data-ttu-id="d1bee-112">V okně Virtuálního počítače, vyberte **vytvořit vlastní image (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="d1bee-112">On the VM's blade, select **Create custom image (VHD)**.</span></span>

    ![Vytvoření vlastní image položky nabídky](./media/devtest-lab-create-template/create-custom-image.png)

1. <span data-ttu-id="d1bee-114">Na **vytvořit image** okno, zadejte název a popis pro vlastní bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="d1bee-114">On the **Create image** blade, enter a name and description for your custom image.</span></span> <span data-ttu-id="d1bee-115">Tyto informace se zobrazí v seznamu základů při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d1bee-115">This information is displayed in the list of bases when you create a VM.</span></span>

    ![Vytvořit vlastní image okno](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. <span data-ttu-id="d1bee-117">Vyberte, zda byl spuštěn nástroj sysprep na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="d1bee-117">Select whether sysprep was run on the VM.</span></span> <span data-ttu-id="d1bee-118">Pokud na virtuální počítač nebyl spuštěn nástroje sysprep, určete, jestli má nástroj sysprep, které jsou spuštěny při vytvoření virtuálního počítače z této vlastní image.</span><span class="sxs-lookup"><span data-stu-id="d1bee-118">If the sysprep was not run on the VM, specify whether you want sysprep run when a VM is created from this custom image.</span></span>

1. <span data-ttu-id="d1bee-119">Vyberte **OK** po dokončení vytvoření vlastní image.</span><span class="sxs-lookup"><span data-stu-id="d1bee-119">Select **OK** when finished to create the custom image.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="d1bee-120">Příspěvky blogu související</span><span class="sxs-lookup"><span data-stu-id="d1bee-120">Related blog posts</span></span>

- [<span data-ttu-id="d1bee-121">Vlastní Image nebo vzorce?</span><span class="sxs-lookup"><span data-stu-id="d1bee-121">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [<span data-ttu-id="d1bee-122">Kopírování vlastních bitových kopií mezi Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="d1bee-122">Copying Custom Images between Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a><span data-ttu-id="d1bee-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d1bee-123">Next steps</span></span>

- [<span data-ttu-id="d1bee-124">Přidejte virtuální počítač do testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="d1bee-124">Add a VM to your lab</span></span>](./devtest-lab-add-vm-with-artifacts.md)
