---
title: "aaaDiagnose artefaktů selhání ve virtuálním počítači Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak tootroubleshoot artefaktů selhání v DevTest Labs"
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
ms.openlocfilehash: 40b3cea72cf071cc5d9a6d002d309d923c3d3084
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-artifact-failures-in-hello-lab"></a><span data-ttu-id="ccde0-103">Diagnostikování selhání artefaktů v testovacím prostředí hello</span><span class="sxs-lookup"><span data-stu-id="ccde0-103">Diagnose artifact failures in hello lab</span></span> 
<span data-ttu-id="ccde0-104">Po vytvoření artefakt, můžete zkontrolovat toosee, pokud ji byla úspěšná nebo neúspěšná.</span><span class="sxs-lookup"><span data-stu-id="ccde0-104">After you have created an artifact, you can check toosee if it succeeded or failed.</span></span> <span data-ttu-id="ccde0-105">Protokoly artefaktů v DevTest Labs poskytují informace, které můžete použít toodiagnose chybu artefaktů.</span><span class="sxs-lookup"><span data-stu-id="ccde0-105">Artifact logs in DevTest Labs provide information you can use toodiagnose an artifact failure.</span></span> <span data-ttu-id="ccde0-106">Zobrazí se informace protokolu hello artefaktů pro virtuální počítač s Windows několik různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="ccde0-106">There are a couple different ways you can view hello artifact log information for a Windows VM.</span></span>

> [!NOTE]
> <span data-ttu-id="ccde0-107">tooensure, že chyby jsou správně identifikovat a vysvětlení, je důležité, zda je správně strukturu této hello artefaktů.</span><span class="sxs-lookup"><span data-stu-id="ccde0-107">tooensure that failures are correctly identified and explained, it is important that hello artifact is properly structured.</span></span> <span data-ttu-id="ccde0-108">Informace o tom, jak toocorrectly Vytvořit artefakt najdete v tématu [vytvoření vlastních artefaktů](devtest-lab-artifact-author.md).</span><span class="sxs-lookup"><span data-stu-id="ccde0-108">For information about how toocorrectly construct an artifact, see [Create custom artifacts](devtest-lab-artifact-author.md).</span></span> <span data-ttu-id="ccde0-109">A toosee příkladem správně strukturovaný artefaktů, podívejte se na to [typy parametrů testovací](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-test-paramtypes) artefaktů.</span><span class="sxs-lookup"><span data-stu-id="ccde0-109">And toosee an example of a properly structured artifact, check out this [Test Parameter Types](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-test-paramtypes) artifact.</span></span>

## <a name="troubleshoot-artifact-failures-using-hello-azure-portal"></a><span data-ttu-id="ccde0-110">Řešení potíží s artefaktů selhání pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ccde0-110">Troubleshoot artifact failures using hello Azure portal</span></span>
<span data-ttu-id="ccde0-111">toouse hello Azure portálu toodiagnose selhání během vytváření artefaktů, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ccde0-111">toouse hello Azure portal toodiagnose failures during artifact creation, follow these steps:</span></span>

1. <span data-ttu-id="ccde0-112">Hello seznamu prostředků vyberte testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="ccde0-112">From hello list of resources, select your lab.</span></span>

2. <span data-ttu-id="ccde0-113">Vyberte virtuální počítač Windows, který zahrnuje hello artefaktů chcete tooinvestigate hello.</span><span class="sxs-lookup"><span data-stu-id="ccde0-113">Choose hello Windows VM that includes hello artifact you want tooinvestigate.</span></span>

3. <span data-ttu-id="ccde0-114">V levém panelu hello pod **Obecné**, zvolte **artefakty**.</span><span class="sxs-lookup"><span data-stu-id="ccde0-114">In hello left panel under **GENERAL**, choose **Artifacts**.</span></span> <span data-ttu-id="ccde0-115">Artefakty související s tohoto virtuálního počítače se zobrazí seznam, která udává název hello hello artefaktů a jeho stav.</span><span class="sxs-lookup"><span data-stu-id="ccde0-115">A list of artifacts associated with that VM appears, indicating hello name of hello artifact and its status.</span></span>

   ![Příklad úložišti git artefaktů](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifacts-failure.png)

4. <span data-ttu-id="ccde0-117">Zvolte artefakt zobrazující stav **se nezdařilo**.</span><span class="sxs-lookup"><span data-stu-id="ccde0-117">Choose an artifact that shows a status of **Failed**.</span></span> <span data-ttu-id="ccde0-118">Hello artefaktů otevře a zobrazí zprávu rozšíření, který obsahuje podrobnosti o selhání hello artefakt hello.</span><span class="sxs-lookup"><span data-stu-id="ccde0-118">hello artifact opens and shows an extension message that includes details about hello failure of hello artifact.</span></span>

   ![Příklad úložišti git artefaktů](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error.png)


## <a name="troubleshoot-artifact-failures-from-within-hello-vm"></a><span data-ttu-id="ccde0-120">Řešení potíží s selhání artefaktů z v rámci hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="ccde0-120">Troubleshoot artifact failures from within hello VM</span></span>
<span data-ttu-id="ccde0-121">tooview protokoly hello artefaktů z hello virtuálním počítači, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ccde0-121">tooview hello artifact logs from within hello virtual machine, follow these steps:</span></span>

1. <span data-ttu-id="ccde0-122">Přihlaste se toohello virtuální počítač, který obsahuje hello artefaktů chcete toodiagnose.</span><span class="sxs-lookup"><span data-stu-id="ccde0-122">Log in toohello VM that contains hello artifact you want toodiagnose.</span></span>

2. <span data-ttu-id="ccde0-123">Přejděte tooC:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.9\Status kde "1.9 je hello číslo verze rozšíření na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="ccde0-123">Navigate tooC:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.9\Status where "1.9 is hello CSE version number.</span></span>

   ![Příklad úložišti git artefaktů](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error-vm-status.png)

3. <span data-ttu-id="ccde0-125">Otevřete hello **stav** souboru tooview informace, že pomáhá diagnostikování selhání artefaktů tohoto virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ccde0-125">Open hello **status** file tooview information that helps diagnose artifact failures for that VM.</span></span>




[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="ccde0-126">Příspěvky blogu související</span><span class="sxs-lookup"><span data-stu-id="ccde0-126">Related blog posts</span></span>
* [<span data-ttu-id="ccde0-127">Připojení k tooexisting virtuálního počítače domény AD pomocí šablony správce prostředků v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="ccde0-127">Join a VM tooexisting AD Domain using a resource manager template in Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a><span data-ttu-id="ccde0-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ccde0-128">Next steps</span></span>
* <span data-ttu-id="ccde0-129">Zjistěte, jak příliš[přidání testovacího prostředí tooa úložiště Git](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="ccde0-129">Learn how too[add a Git repository tooa lab](devtest-lab-add-artifact-repo.md).</span></span>

