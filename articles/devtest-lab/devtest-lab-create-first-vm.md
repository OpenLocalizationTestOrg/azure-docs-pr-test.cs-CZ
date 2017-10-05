---
title: "Vytvoření vaší první virtuální počítač v testovacím prostředí v Azure DevTest Labs | Microsoft Docs"
description: "Naučte se vytvořit svůj první virtuální počítač v testovacím prostředí v Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: fbc5a438-6e02-4952-b654-b8fa7322ae5f
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/24/2017
ms.author: tarcher
ms.openlocfilehash: aa6b60b799e1e98815cf288d5612f98cd77cc00e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-first-vm-in-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="9e73b-103">Vytvoření vaší první virtuální počítač v testovacím prostředí v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="9e73b-103">Create your first VM in a lab in Azure DevTest Labs</span></span>

<span data-ttu-id="9e73b-104">Pokud jste původně přístup DevTest Labs a chcete k vytvoření vašeho prvního virtuálního počítače, bude pravděpodobně uděláte pomocí předem načtený [image základní marketplace](devtest-lab-configure-marketplace-images.md).</span><span class="sxs-lookup"><span data-stu-id="9e73b-104">When you initially access DevTest Labs and want to create your first VM, you will likely do so using a pre-loaded [base marketplace image](devtest-lab-configure-marketplace-images.md).</span></span> <span data-ttu-id="9e73b-105">Později na taky budete moct vybrat z [vlastní image a vzorce](devtest-lab-add-vm.md) při vytváření více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9e73b-105">Later on, you'll also be able to choose from a [custom image and a formula](devtest-lab-add-vm.md) when creating more VMs.</span></span> 

<span data-ttu-id="9e73b-106">Tento kurz vás provede procesem přidání vaší první virtuální počítač do testovacího prostředí v DevTest Labs pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9e73b-106">This tutorial walks you through using the Azure portal to add your first VM to a lab in DevTest Labs.</span></span>

## <a name="steps-to-add-your-first-vm-to-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="9e73b-107">Postup pro přidání do testovacího prostředí v Azure DevTest Labs vašeho prvního virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="9e73b-107">Steps to add your first VM to a lab in Azure DevTest Labs</span></span>
1. <span data-ttu-id="9e73b-108">Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="9e73b-108">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="9e73b-109">Vyberte **více služeb**a potom vyberte **DevTest Labs** ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="9e73b-109">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>
1. <span data-ttu-id="9e73b-110">Ze seznamu labs vyberte testovací prostředí, ve kterém chcete vytvořit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="9e73b-110">From the list of labs, select the lab in which you want to create the VM.</span></span>  
1. <span data-ttu-id="9e73b-111">V tomto prostředí **přehled** vyberte **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="9e73b-111">On the lab's **Overview** blade, select **+ Add**.</span></span>  

    ![Virtuální počítač tlačítko Přidat](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. <span data-ttu-id="9e73b-113">Na **zvolte na základní** okně, vyberte bitovou kopii na trhu pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="9e73b-113">On the **Choose a base** blade, select a marketplace image for the VM.</span></span>
1. <span data-ttu-id="9e73b-114">Na **virtuálního počítače** okno, zadejte název pro nový virtuální počítač **název virtuálního počítače** textového pole.</span><span class="sxs-lookup"><span data-stu-id="9e73b-114">On the **Virtual machine** blade, enter a name for the new virtual machine in the **Virtual machine name** text box.</span></span>

    ![Okno prostředí virtuálních počítačů](./media/devtest-lab-add-vm/devtestlab-lab-add-first-vm.png)

1. <span data-ttu-id="9e73b-116">Zadejte **uživatelské jméno** , jsou udělena oprávnění správce na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="9e73b-116">Enter a **User Name** that is granted administrator privileges on the virtual machine.</span></span>  
1. <span data-ttu-id="9e73b-117">Zadejte heslo do textového pole s názvem bez přípony **zadejte hodnotu**.</span><span class="sxs-lookup"><span data-stu-id="9e73b-117">Enter a password in the text field labeled **Type a value**.</span></span>
1. <span data-ttu-id="9e73b-118">**Typ disku virtuálního počítače** určuje disku typů úložiště, které u virtuálních počítačů v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9e73b-118">The **Virtual machine disk type** determines which storage disk type is allowed for the virtual machines in the lab.</span></span>
1. <span data-ttu-id="9e73b-119">Vyberte **velikost virtuálního počítače** a vyberte jednu z předdefinovaných položky, které zadejte jader procesoru, velikosti paměti RAM a velikost pevného disku pro vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9e73b-119">Select **Virtual machine size** and select one of the predefined items that specify the processor cores, RAM size, and the hard drive size of the VM to create.</span></span>
1. <span data-ttu-id="9e73b-120">Vyberte **artefakty** a - ze seznamu artefaktů - vyberte a nakonfigurujte artefaktů, které chcete přidat do základní bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="9e73b-120">Select **Artifacts** and - from the list of artifacts - select and configure the artifacts that you want to add to the base image.</span></span>
    <span data-ttu-id="9e73b-121">**Poznámka:** Pokud nepracovali DevTest Labs nebo konfigurace artefaktů, vyhledejte [přidat existující artefaktů pro virtuální počítač](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) části a vraťte se sem po dokončení.</span><span class="sxs-lookup"><span data-stu-id="9e73b-121">**Note:** If you're new to DevTest Labs or configuring artifacts, refer to the [Add an existing artifact to a VM](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) section, and then return here when finished.</span></span>
1. <span data-ttu-id="9e73b-122">Vyberte **vytvořit** přidat zadaný virtuální počítač do testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="9e73b-122">Select **Create** to add the specified VM to the lab.</span></span>

   <span data-ttu-id="9e73b-123">V okně prostředí zobrazí stav vytváření Virtuálního počítače – nejprve jako **vytváření**, pak jako **systémem** po spuštění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9e73b-123">The lab blade displays the status of the VM's creation - first as **Creating**, then as **Running** after the VM has been started.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9e73b-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9e73b-124">Next steps</span></span>
* <span data-ttu-id="9e73b-125">Po vytvoření virtuálního počítače, můžete připojit k virtuálnímu počítači tak, že vyberete **Connect** v okně Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9e73b-125">Once the VM has been created, you can connect to the VM by selecting **Connect** on the VM's blade.</span></span>
* <span data-ttu-id="9e73b-126">Podívejte se na [přidat virtuální počítač do testovacího prostředí](devtest-lab-add-vm.md) podrobnější informace o přidávání dalších virtuálních počítačů ve svém testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9e73b-126">Check out [Add a VM to a lab](devtest-lab-add-vm.md) for more complete info about adding subsequent VMs in your lab.</span></span>
* <span data-ttu-id="9e73b-127">Prozkoumejte [Galerie šablon DevTest Labs Azure Resource Manager QuickStart](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).</span><span class="sxs-lookup"><span data-stu-id="9e73b-127">Explore the [DevTest Labs Azure Resource Manager QuickStart template gallery](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).</span></span>
