---
title: "Diagnostikování selhání artefaktů ve virtuálním počítači Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak vyřešit selhání artefaktů v DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 115e0086-3293-4adf-8738-9f639f31f918
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: tarcher
ms.openlocfilehash: e4f2946d0ba0756f36622aded0e8594acabb9527
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="diagnose-artifact-failures-in-the-lab"></a><span data-ttu-id="20398-103">Diagnostikování selhání artefaktů v testovacím prostředí</span><span class="sxs-lookup"><span data-stu-id="20398-103">Diagnose artifact failures in the lab</span></span> 
<span data-ttu-id="20398-104">Po vytvoření artefakt, můžete zkontrolovat v tématu, pokud byla úspěšná nebo neúspěšná.</span><span class="sxs-lookup"><span data-stu-id="20398-104">After you have created an artifact, you can check to see if it succeeded or failed.</span></span> <span data-ttu-id="20398-105">Protokoly artefaktů v DevTest Labs poskytují informace, které můžete použít k diagnostikování selhání artefaktů.</span><span class="sxs-lookup"><span data-stu-id="20398-105">Artifact logs in DevTest Labs provide information you can use to diagnose an artifact failure.</span></span> <span data-ttu-id="20398-106">Existuje několik způsobů, můžete zobrazit informace o protokolu artefaktů pro virtuální počítač s Windows.</span><span class="sxs-lookup"><span data-stu-id="20398-106">There are a couple different ways you can view the artifact log information for a Windows VM.</span></span>

> [!NOTE]
> <span data-ttu-id="20398-107">Zajistit, že chyby jsou správně identifikovat a vysvětlení, je důležité, že je správně strukturován artefaktu.</span><span class="sxs-lookup"><span data-stu-id="20398-107">To ensure that failures are correctly identified and explained, it is important that the artifact is properly structured.</span></span> <span data-ttu-id="20398-108">Informace o tom, jak správně vytvořit artefakt najdete v tématu [vytvoření vlastních artefaktů](devtest-lab-artifact-author.md).</span><span class="sxs-lookup"><span data-stu-id="20398-108">For information about how to correctly construct an artifact, see [Create custom artifacts](devtest-lab-artifact-author.md).</span></span> <span data-ttu-id="20398-109">A pokud chcete zobrazit příklad správně strukturovaný artefaktů, podívejte se na to [typy parametrů testovací](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-test-paramtypes) artefaktů.</span><span class="sxs-lookup"><span data-stu-id="20398-109">And to see an example of a properly structured artifact, check out this [Test Parameter Types](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-test-paramtypes) artifact.</span></span>

## <a name="troubleshoot-artifact-failures-using-the-azure-portal"></a><span data-ttu-id="20398-110">Řešení potíží s artefaktů selhání pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="20398-110">Troubleshoot artifact failures using the Azure portal</span></span>
<span data-ttu-id="20398-111">Portál Azure k diagnostikování selhání během vytváření artefaktů, postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="20398-111">To use the Azure portal to diagnose failures during artifact creation, follow these steps:</span></span>

1. <span data-ttu-id="20398-112">V seznamu zdrojů vyberte testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="20398-112">From the list of resources, select your lab.</span></span>

2. <span data-ttu-id="20398-113">Vyberte virtuální počítač Windows, který zahrnuje artefaktu, který chcete prozkoumat.</span><span class="sxs-lookup"><span data-stu-id="20398-113">Choose the Windows VM that includes the artifact you want to investigate.</span></span>

3. <span data-ttu-id="20398-114">V levém podokně v části **Obecné**, zvolte **artefakty**.</span><span class="sxs-lookup"><span data-stu-id="20398-114">In the left panel under **GENERAL**, choose **Artifacts**.</span></span> <span data-ttu-id="20398-115">Zobrazí se seznam artefakty související s tohoto virtuálního počítače, oznamující název artefaktu a jeho stav.</span><span class="sxs-lookup"><span data-stu-id="20398-115">A list of artifacts associated with that VM appears, indicating the name of the artifact and its status.</span></span>

   ![Příklad úložišti git artefaktů](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifacts-failure.png)

4. <span data-ttu-id="20398-117">Zvolte artefakt zobrazující stav **se nezdařilo**.</span><span class="sxs-lookup"><span data-stu-id="20398-117">Choose an artifact that shows a status of **Failed**.</span></span> <span data-ttu-id="20398-118">Artefaktu otevře a zobrazí zprávu rozšíření, která obsahuje podrobnosti o selhání artefaktu.</span><span class="sxs-lookup"><span data-stu-id="20398-118">The artifact opens and shows an extension message that includes details about the failure of the artifact.</span></span>

   ![Příklad úložišti git artefaktů](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error.png)


## <a name="troubleshoot-artifact-failures-from-within-the-vm"></a><span data-ttu-id="20398-120">Řešení potíží s selhání artefaktů z virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="20398-120">Troubleshoot artifact failures from within the VM</span></span>
<span data-ttu-id="20398-121">Pokud chcete zobrazit protokoly artefaktů z virtuálního počítače, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="20398-121">To view the artifact logs from within the virtual machine, follow these steps:</span></span>

1. <span data-ttu-id="20398-122">Přihlaste se k virtuálním počítači, který obsahuje artefaktů, které chcete diagnostikovat.</span><span class="sxs-lookup"><span data-stu-id="20398-122">Log in to the VM that contains the artifact you want to diagnose.</span></span>

2. <span data-ttu-id="20398-123">Přejděte na C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.9\Status kde "1.9 je číslo verze rozšíření na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="20398-123">Navigate to C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.9\Status where "1.9 is the CSE version number.</span></span>

   ![Příklad úložišti git artefaktů](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error-vm-status.png)

3. <span data-ttu-id="20398-125">Otevřete **stav** soubor k zobrazení informací, že pomáhá diagnostikování selhání artefaktů tohoto virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="20398-125">Open the **status** file to view information that helps diagnose artifact failures for that VM.</span></span>




[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="20398-126">Příspěvky blogu související</span><span class="sxs-lookup"><span data-stu-id="20398-126">Related blog posts</span></span>
* [<span data-ttu-id="20398-127">Připojení virtuálního počítače do existující domény AD pomocí šablony správce prostředků v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="20398-127">Join a VM to existing AD Domain using a resource manager template in Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a><span data-ttu-id="20398-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="20398-128">Next steps</span></span>
* <span data-ttu-id="20398-129">Zjistěte, jak [přidejte úložiště Git do testovacího prostředí](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="20398-129">Learn how to [add a Git repository to a lab](devtest-lab-add-artifact-repo.md).</span></span>

