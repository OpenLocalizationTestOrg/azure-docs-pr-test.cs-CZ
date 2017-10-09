---
title: "aaaCreate vlastní obrázek z virtuálního počítače Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak hello toocreate vlastní image v Azure DevTest Labs z zřízeného virtuálního počítače pomocí portálu Azure"
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
ms.openlocfilehash: 7dccb79d3db4aae676c7bd2f6b800301210491e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-from-a-vm"></a><span data-ttu-id="ac5b3-103">Vytvořit vlastní image z virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="ac5b3-103">Create a custom image from a VM</span></span>

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a><span data-ttu-id="ac5b3-104">Podrobné pokyny</span><span class="sxs-lookup"><span data-stu-id="ac5b3-104">Step-by-step instructions</span></span>

<span data-ttu-id="ac5b3-105">Můžete vytvořit vlastní image ze zřízeného virtuálního počítače a pak použít tento vlastní image toocreate identické virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="ac5b3-105">You can create a custom image from a provisioned VM, and afterwards use that custom image toocreate identical VMs.</span></span> <span data-ttu-id="ac5b3-106">Hello následující kroky popisují, jak toocreate vlastní obrázek z virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="ac5b3-106">hello following steps illustrate how toocreate a custom image from a VM:</span></span>

1. <span data-ttu-id="ac5b3-107">Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="ac5b3-107">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="ac5b3-108">Vyberte **další služby**a potom vyberte **DevTest Labs** hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="ac5b3-108">Select **More services**, and then select **DevTest Labs** from hello list.</span></span>

1. <span data-ttu-id="ac5b3-109">Ze seznamu hello labs vyberte požadované prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="ac5b3-109">From hello list of labs, select hello desired lab.</span></span>  

1. <span data-ttu-id="ac5b3-110">V okně prostředí hello vyberte **Můj virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="ac5b3-110">On hello lab's blade, select **My virtual machines**.</span></span>
 
1. <span data-ttu-id="ac5b3-111">Na hello **Můj virtuální počítače** okně vyberte hello virtuálních počítačů, ze kterého mají být toocreate hello vlastní image.</span><span class="sxs-lookup"><span data-stu-id="ac5b3-111">On hello **My virtual machines** blade, select hello VM from which you want toocreate hello custom image.</span></span>

1. <span data-ttu-id="ac5b3-112">V okně hello Virtuálního počítače, vyberte **vytvořit vlastní image (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="ac5b3-112">On hello VM's blade, select **Create custom image (VHD)**.</span></span>

    ![Vytvoření vlastní image položky nabídky](./media/devtest-lab-create-template/create-custom-image.png)

1. <span data-ttu-id="ac5b3-114">Na hello **vytvořit image** okno, zadejte název a popis pro vlastní bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="ac5b3-114">On hello **Create image** blade, enter a name and description for your custom image.</span></span> <span data-ttu-id="ac5b3-115">Tyto informace se zobrazí v seznamu hello základů při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ac5b3-115">This information is displayed in hello list of bases when you create a VM.</span></span>

    ![Vytvořit vlastní image okno](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. <span data-ttu-id="ac5b3-117">Vyberte, zda byl spuštěn nástroj sysprep hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ac5b3-117">Select whether sysprep was run on hello VM.</span></span> <span data-ttu-id="ac5b3-118">Pokud na hello virtuální počítač nebyl spuštěn nástroj sysprep hello, určete, jestli má nástroj sysprep, které jsou spuštěny při vytvoření virtuálního počítače z této vlastní image.</span><span class="sxs-lookup"><span data-stu-id="ac5b3-118">If hello sysprep was not run on hello VM, specify whether you want sysprep run when a VM is created from this custom image.</span></span>

1. <span data-ttu-id="ac5b3-119">Vyberte **OK** při dokončení toocreate hello vlastní image.</span><span class="sxs-lookup"><span data-stu-id="ac5b3-119">Select **OK** when finished toocreate hello custom image.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="ac5b3-120">Příspěvky blogu související</span><span class="sxs-lookup"><span data-stu-id="ac5b3-120">Related blog posts</span></span>

- [<span data-ttu-id="ac5b3-121">Vlastní Image nebo vzorce?</span><span class="sxs-lookup"><span data-stu-id="ac5b3-121">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [<span data-ttu-id="ac5b3-122">Kopírování vlastních bitových kopií mezi Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="ac5b3-122">Copying Custom Images between Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a><span data-ttu-id="ac5b3-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ac5b3-123">Next steps</span></span>

- [<span data-ttu-id="ac5b3-124">Přidání testovacího prostředí tooyour virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="ac5b3-124">Add a VM tooyour lab</span></span>](./devtest-lab-add-vm-with-artifacts.md)
