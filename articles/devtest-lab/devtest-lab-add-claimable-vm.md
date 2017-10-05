---
title: "Přidat vymahatelných virtuální počítač do testovacího prostředí v Azure DevTest Labs | Microsoft Docs"
description: "Informace o postupu přidání vymahatelných virtuálního počítače do testovacího prostředí v Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: f671e66e-9630-4e30-a131-a6bad9ed9c11
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: tarcher
ms.openlocfilehash: 98950d72e90b0e178bae2fffa7644fd824a25eea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-claimable-vm-to-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="76243-103">Přidat vymahatelných virtuální počítač do testovacího prostředí v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="76243-103">Add a claimable VM to a lab in Azure DevTest Labs</span></span>
<span data-ttu-id="76243-104">Přidat vymahatelných virtuálního počítače do testovacího prostředí podobným způsobem, jak můžete [přidat standardní virtuální počítač](devtest-lab-add-vm.md) – z *základní* to znamená buď [vlastní image](devtest-lab-create-template.md), [vzorec](devtest-lab-manage-formulas.md), nebo [Marketplace image](devtest-lab-configure-marketplace-images.md).</span><span class="sxs-lookup"><span data-stu-id="76243-104">You add a claimable VM to a lab in a similar manner to how you [add a standard VM](devtest-lab-add-vm.md) – from a *base* that is either a [custom image](devtest-lab-create-template.md), [formula](devtest-lab-manage-formulas.md), or [Marketplace image](devtest-lab-configure-marketplace-images.md).</span></span> <span data-ttu-id="76243-105">Tento kurz vás provede procesem přidání vymahatelných virtuálního počítače do testovacího prostředí v DevTest Labs pomocí portálu Azure a zobrazuje proces, že uživatel přejde do deklarací identity virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="76243-105">This tutorial walks you through using the Azure portal to add a claimable VM to a lab in DevTest Labs, and shows the process a user follows to claim the VM.</span></span>

> [!NOTE]
> <span data-ttu-id="76243-106">Pokud nasadíte virtuální počítače testovacího prostředí prostřednictvím [šablon Azure Resource Manageru](devtest-lab-create-environment-from-arm.md), můžete vytvořit virtuální počítače vymahatelných nastavením **allowClaim** vlastnost na hodnotu true v části Vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="76243-106">If you deploy lab VMs through [Azure Resource Manager templates](devtest-lab-create-environment-from-arm.md), you can create claimable VMs by setting the **allowClaim** property to true in the properties section.</span></span>
>
>

## <a name="steps-to-add-a-claimable-vm-to-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="76243-107">Postup pro přidání do testovacího prostředí v Azure DevTest Labs vymahatelných virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="76243-107">Steps to add a claimable VM to a lab in Azure DevTest Labs</span></span>
1. <span data-ttu-id="76243-108">Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="76243-108">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="76243-109">Vyberte **více služeb**a potom vyberte **DevTest Labs** ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="76243-109">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>
1. <span data-ttu-id="76243-110">Ze seznamu labs vyberte testovací prostředí, ve kterém chcete vytvořit vymahatelných virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="76243-110">From the list of labs, select the lab in which you want to create the claimable VM.</span></span>  
1. <span data-ttu-id="76243-111">V tomto prostředí **přehled** vyberte **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="76243-111">On the lab's **Overview** blade, select **+ Add**.</span></span>  

    ![Virtuální počítač tlačítko Přidat](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. <span data-ttu-id="76243-113">Na **zvolte na základní** okně vyberte základ pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="76243-113">On the **Choose a base** blade, select a base for the VM.</span></span>
1. <span data-ttu-id="76243-114">Na **virtuálního počítače** okno, zadejte název pro nový virtuální počítač **název virtuálního počítače** textového pole.</span><span class="sxs-lookup"><span data-stu-id="76243-114">On the **Virtual machine** blade, enter a name for the new virtual machine in the **Virtual machine name** text box.</span></span>

    ![Okno prostředí virtuálních počítačů](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. <span data-ttu-id="76243-116">Zadejte **uživatelské jméno** , jsou udělena oprávnění správce na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="76243-116">Enter a **User Name** that is granted administrator privileges on the virtual machine.</span></span>  
1. <span data-ttu-id="76243-117">Pokud chcete použít heslo uložené v vaše [tajný úložiště](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store), vyberte **použít uložené tajný klíč**a zadejte hodnotu klíče, která odpovídá váš tajný klíč (heslo).</span><span class="sxs-lookup"><span data-stu-id="76243-117">If you want to use a password stored in your [secret store](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store), select **Use a saved secret**, and specify a key value that corresponds to your secret (password).</span></span> <span data-ttu-id="76243-118">Jinak, zadejte heslo do textového pole s názvem bez přípony **zadejte hodnotu**.</span><span class="sxs-lookup"><span data-stu-id="76243-118">Otherwise, enter a password in the text field labeled **Type a value**.</span></span>
1. <span data-ttu-id="76243-119">**Typ disku virtuálního počítače** určuje disku typů úložiště, které u virtuálních počítačů v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="76243-119">The **Virtual machine disk type** determines which storage disk type is allowed for the virtual machines in the lab.</span></span>
1. <span data-ttu-id="76243-120">Vyberte **velikost virtuálního počítače** a vyberte jednu z předdefinovaných položky, které zadejte jader procesoru, velikosti paměti RAM a velikost pevného disku pro vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="76243-120">Select **Virtual machine size** and select one of the predefined items that specify the processor cores, RAM size, and the hard drive size of the VM to create.</span></span>
1. <span data-ttu-id="76243-121">Vyberte **artefakty** a ze seznamu artefaktů, vyberte a nakonfigurujte artefaktů, které chcete přidat do základní bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="76243-121">Select **Artifacts** and from the list of artifacts, select and configure the artifacts that you want to add to the base image.</span></span> <span data-ttu-id="76243-122">Pokud nepracovali DevTest Labs nebo konfigurace artefaktů, vyhledejte [přidat existující artefaktů pro virtuální počítač](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) části a vraťte se sem po dokončení.</span><span class="sxs-lookup"><span data-stu-id="76243-122">If you're new to DevTest Labs or configuring artifacts, refer to the [Add an existing artifact to a VM](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) section, and then return here when finished.</span></span>
1. <span data-ttu-id="76243-123">Vyberte **upřesňující nastavení** nakonfigurovat možnosti sítě a vypršení platnosti možnosti Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="76243-123">Select **Advanced settings** to configure the VM's network options and expiration options.</span></span> <span data-ttu-id="76243-124">V části **deklarace identity možnosti**, zvolte **Ano** k zajištění vymahatelných počítače.</span><span class="sxs-lookup"><span data-stu-id="76243-124">Under **Claim options**, choose **Yes** to make the machine claimable.</span></span>

  ![Zvolte, aby vymahatelných virtuálního počítače.](./media/devtest-lab-add-vm/devtestlab-claim-VM-option.png)

1. <span data-ttu-id="76243-126">Pokud chcete zobrazit nebo zkopírování šablony Azure Resource Manager, přečtěte si [šablony uložit Azure Resource Manageru](devtest-lab-add-vm.md#save-azure-resource-manager-template) části a sem vraťte po dokončení.</span><span class="sxs-lookup"><span data-stu-id="76243-126">If you want to view or copy the Azure Resource Manager template, refer to the [Save Azure Resource Manager template](devtest-lab-add-vm.md#save-azure-resource-manager-template) section, and return here when finished.</span></span>
1. <span data-ttu-id="76243-127">Vyberte **vytvořit** přidat zadaný virtuální počítač do testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="76243-127">Select **Create** to add the specified VM to the lab.</span></span>
1. <span data-ttu-id="76243-128">V okně prostředí zobrazí stav vytváření Virtuálního počítače – nejprve jako **vytváření**, pak jako **systémem** po spuštění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="76243-128">The lab blade displays the status of the VM's creation - first as **Creating**, then as **Running** after the VM has been started.</span></span>


## <a name="using-a-claimable-vm"></a><span data-ttu-id="76243-129">Pomocí vymahatelných virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="76243-129">Using a claimable VM</span></span>

<span data-ttu-id="76243-130">Uživatel může deklarace identity žádné virtuální počítače ze seznamu "Vymahatelných virtuální počítače" jedním z těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="76243-130">A user can claim any VM from the list of "Claimable virtual machines" by doing one of these steps:</span></span>

* <span data-ttu-id="76243-131">Ze seznamu "Vymahatelných virtuální počítače" v dolní části okna Přehled v prostředí, klikněte pravým tlačítkem na jednu z virtuálních počítačů v seznamu a vyberte **deklarace identity počítače**.</span><span class="sxs-lookup"><span data-stu-id="76243-131">From the list of "Claimable virtual machines" at the bottom of the lab's Overview blade, right-click on one of the VMs in the list and choose **Claim machine**.</span></span>

 ![Požádat o konkrétní vymahatelných virtuální počítač.](./media/devtest-lab-add-vm/devtestlab-claim-VM.png)


* <span data-ttu-id="76243-133">V horní části **přehled** okně zvolte **všechny deklarace**.</span><span class="sxs-lookup"><span data-stu-id="76243-133">At the top of the **Overview** blade, choose **Claim any**.</span></span> <span data-ttu-id="76243-134">Náhodné virtuální počítač přidělenou ze seznamu vymahatelných virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="76243-134">A random virtual machine is assigned from the list of claimable VMs.</span></span>

 ![Požádat o žádné vymahatelných virtuální počítače.](./media/devtest-lab-add-vm/devtestlab-claim-any.png)


<span data-ttu-id="76243-136">Poté, co uživatel deklarací virtuálního počítače, je do jejich seznam "Moje virtuální počítače" Přesunout nahoru a již není vymahatelných jiným uživatelem.</span><span class="sxs-lookup"><span data-stu-id="76243-136">After a user claims a VM, it is moved up into their list of "My virtual machines" and is no longer claimable by any other user.</span></span>

## <a name="next-steps"></a><span data-ttu-id="76243-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="76243-137">Next steps</span></span>
* <span data-ttu-id="76243-138">Po vytvoření virtuálního počítače, můžete připojit k virtuálnímu počítači tak, že vyberete **Connect** v okně Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="76243-138">Once the VM has been created, you can connect to the VM by selecting **Connect** on the VM's blade.</span></span>
* <span data-ttu-id="76243-139">Prozkoumejte [Galerie šablon DevTest Labs Azure Resource Manager rychlý start](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates)</span><span class="sxs-lookup"><span data-stu-id="76243-139">Explore the [DevTest Labs Azure Resource Manager QuickStart template gallery](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates)</span></span>
