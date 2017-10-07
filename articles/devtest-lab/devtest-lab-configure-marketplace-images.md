---
title: "nastavení bitové kopie aaaConfigure Azure Marketplace v Azure DevTest Labs | Microsoft Docs"
description: "Konfigurace, které Azure Marketplace obrázky lze použít při vytváření virtuálního počítače v Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 804c6af2-17e9-4320-af3a-f454bd398379
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: bb4b7f1c0cbe967bee724f7ee20f64f8c4ea58ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-marketplace-image-settings-in-azure-devtest-labs"></a><span data-ttu-id="0c1a7-103">Konfigurace nastavení Azure Marketplace bitové kopie v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="0c1a7-103">Configure Azure Marketplace image settings in Azure DevTest Labs</span></span>
<span data-ttu-id="0c1a7-104">DevTest Labs podporuje vytváření virtuálních počítačů založené na imagích Azure Marketplace v závislosti na tom, jak jste nakonfigurovali toobe Image Azure Marketplace použít ve svém testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="0c1a7-104">DevTest Labs supports creating VMs based on Azure Marketplace images depending on how you have configured Azure Marketplace images toobe used in your lab.</span></span> <span data-ttu-id="0c1a7-105">Tento článek ukazuje, jak toospecify, který, pokud existuje, Azure Marketplace bitové kopie lze použít při vytváření virtuálních počítačů v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="0c1a7-105">This article shows you how toospecify which, if any, Azure Marketplace images can be used when creating VMs in a lab.</span></span> <span data-ttu-id="0c1a7-106">Tím se zajistí, že má váš tým pouze přístup toohello Marketplace obrázky, které potřebují.</span><span class="sxs-lookup"><span data-stu-id="0c1a7-106">This ensures that your team only has access toohello Marketplace images they need.</span></span> 

## <a name="select-which-azure-marketplace-images-are-allowed-when-creating-a-vm"></a><span data-ttu-id="0c1a7-107">Vyberte, které Azure Marketplace obrázků jsou povoleny při vytváření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="0c1a7-107">Select which Azure Marketplace images are allowed when creating a VM</span></span>
1. <span data-ttu-id="0c1a7-108">Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="0c1a7-108">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="0c1a7-109">Vyberte **více služeb**a potom vyberte **DevTest Labs** hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="0c1a7-109">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
3. <span data-ttu-id="0c1a7-110">Ze seznamu hello labs vyberte požadované prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="0c1a7-110">From hello list of labs, select hello desired lab.</span></span> 
4. <span data-ttu-id="0c1a7-111">V okně prostředí hello vyberte **konfiguraci a zásady**.</span><span class="sxs-lookup"><span data-stu-id="0c1a7-111">On hello lab's blade, select **Configuration and policies**.</span></span>
5. <span data-ttu-id="0c1a7-112">Na testovacího prostředí na **konfiguraci a zásady** okno pod **základů virtuálního počítače**, vyberte **Marketplace image**.</span><span class="sxs-lookup"><span data-stu-id="0c1a7-112">On lab's **Configuration and policies** blade under **Virtual Machine Bases**, select **Marketplace images**.</span></span>
6. <span data-ttu-id="0c1a7-113">Zadejte, zda chcete, že všechny hello kvalifikovaný Image Azure Marketplace toobe k dispozici pro použití jako základ nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0c1a7-113">Specify whether you want all hello qualified Azure Marketplace images toobe available for use as a base of a new VM.</span></span> <span data-ttu-id="0c1a7-114">Pokud vyberete **Ano**, pak všechny Image Azure Marketplace hello které splňují všechny hello následující kritéria jsou povoleny v testovacím prostředí hello:</span><span class="sxs-lookup"><span data-stu-id="0c1a7-114">If you select **Yes**, then all hello Azure Marketplace images that meet all hello following criteria are allowed in hello lab:</span></span>
   
   * <span data-ttu-id="0c1a7-115">Obrázek Hello vytvoří jeden virtuální počítač, **a**</span><span class="sxs-lookup"><span data-stu-id="0c1a7-115">hello image creates a single VM, **and**</span></span>
   * <span data-ttu-id="0c1a7-116">Obrázek Hello používá tooprovision virtuálních počítačích Azure Resource Manager, **a**</span><span class="sxs-lookup"><span data-stu-id="0c1a7-116">hello image uses Azure Resource Manager tooprovision VMs, **and**</span></span>
   * <span data-ttu-id="0c1a7-117">Obrázek Hello nevyžaduje zakoupení velmi licenčního plánu</span><span class="sxs-lookup"><span data-stu-id="0c1a7-117">hello image doesn't require purchasing an extra licensing plan</span></span>
     
    <span data-ttu-id="0c1a7-118">Pokud chcete, aby žádné toobe bitové kopie povolené, nebo chcete toospecify obrázky, které lze použít, vyberte **ne**.</span><span class="sxs-lookup"><span data-stu-id="0c1a7-118">If you want no images toobe allowed, or you want toospecify which images can be used, select **No**.</span></span>
     
     ![Možnost tooallow všechny bitové kopie toobe Marketplace použít jako základní bitové kopie u virtuálních počítačů](./media/devtest-lab-configure-marketplace-images/allow-all-marketplace-images.png)
7. <span data-ttu-id="0c1a7-120">Pokud vyberete **ne** toohello předchozí krok, hello **bitové kopie nebo vyberte povoleno, všechny** zaškrtávací políčko je aktivní.</span><span class="sxs-lookup"><span data-stu-id="0c1a7-120">If you select **No** toohello previous step, hello **Allowed images/Select all** checkbox is enabled.</span></span> 
   <span data-ttu-id="0c1a7-121">Můžete použít tuto možnost společně s hello vyhledávací pole tooquickly vyberte nebo zrušte všechny položky hello zobrazí v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="0c1a7-121">You can use this option together with hello search box tooquickly select or deselect all hello items displayed in hello list.</span></span>
   * <span data-ttu-id="0c1a7-122">Vyberte hello Image Azure Marketplace chcete tooallow pro vytvoření virtuálních počítačů jednotlivě zaškrtnutím políčka odpovídající každé bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="0c1a7-122">Select hello Azure Marketplace images you want tooallow for VM creation individually by checking each image's corresponding checkbox.</span></span>
   * <span data-ttu-id="0c1a7-123">Pokud nechcete, aby tooallow žádné toobe Image Azure Marketplace v testovacím prostředí hello používá, vyberte ze seznamu hello nic.</span><span class="sxs-lookup"><span data-stu-id="0c1a7-123">Select nothing from hello list if you don't want tooallow any Azure Marketplace images toobe used in hello lab.</span></span>
   
    ![Můžete zadat, které Azure Marketplace obrázky lze použít jako základní bitové kopie u virtuálních počítačů](./media/devtest-lab-configure-marketplace-images/select-marketplace-images.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="0c1a7-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0c1a7-125">Next steps</span></span>
<span data-ttu-id="0c1a7-126">Nakonfigurované tak, jak jsou povoleny Azure Marketplace obrázky, při vytváření virtuálního počítače, je dalším krokem hello příliš[přidání testovacího prostředí virtuálních počítačů tooyour](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="0c1a7-126">Once you have configured how Azure Marketplace images are allowed when creating a VM, hello next step is too[add a VM tooyour lab](devtest-lab-add-vm-with-artifacts.md).</span></span>

