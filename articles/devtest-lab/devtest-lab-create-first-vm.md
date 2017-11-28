---
title: "aaaCreate vaše první virtuální počítač v testovacím prostředí v Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak toocreate prvním virtuálním počítači v testovacím prostředí v Azure DevTest Labs"
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
ms.openlocfilehash: 4c3257efca9be6fdd190eaac1db731464e07fcfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-vm-in-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="5561d-103">Vytvoření vaší první virtuální počítač v testovacím prostředí v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="5561d-103">Create your first VM in a lab in Azure DevTest Labs</span></span>

<span data-ttu-id="5561d-104">Pokud jste původně přístup DevTest Labs a chcete toocreate vaše první virtuální počítač, bude pravděpodobně uděláte pomocí předem načtený [image základní marketplace](devtest-lab-configure-marketplace-images.md).</span><span class="sxs-lookup"><span data-stu-id="5561d-104">When you initially access DevTest Labs and want toocreate your first VM, you will likely do so using a pre-loaded [base marketplace image](devtest-lab-configure-marketplace-images.md).</span></span> <span data-ttu-id="5561d-105">Později na taky budete moct toochoose z [vlastní image a vzorce](devtest-lab-add-vm.md) při vytváření více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5561d-105">Later on, you'll also be able toochoose from a [custom image and a formula](devtest-lab-add-vm.md) when creating more VMs.</span></span> 

<span data-ttu-id="5561d-106">Tento kurz vás provede pomocí hello Azure portálu tooadd první testovacího virtuálního počítače tooa prostředí v DevTest Labs.</span><span class="sxs-lookup"><span data-stu-id="5561d-106">This tutorial walks you through using hello Azure portal tooadd your first VM tooa lab in DevTest Labs.</span></span>

## <a name="steps-tooadd-your-first-vm-tooa-lab-in-azure-devtest-labs"></a><span data-ttu-id="5561d-107">Kroky tooadd první testovacího prostředí tooa virtuálních počítačů v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="5561d-107">Steps tooadd your first VM tooa lab in Azure DevTest Labs</span></span>
1. <span data-ttu-id="5561d-108">Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="5561d-108">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="5561d-109">Vyberte **více služeb**a potom vyberte **DevTest Labs** hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="5561d-109">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
1. <span data-ttu-id="5561d-110">Ze seznamu hello labs vyberte hello testovacího prostředí, ve kterém chcete toocreate hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5561d-110">From hello list of labs, select hello lab in which you want toocreate hello VM.</span></span>  
1. <span data-ttu-id="5561d-111">V testovacím hello **přehled** vyberte **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="5561d-111">On hello lab's **Overview** blade, select **+ Add**.</span></span>  

    ![Virtuální počítač tlačítko Přidat](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. <span data-ttu-id="5561d-113">Na hello **zvolte na základní** okně, vyberte bitovou kopii na trhu hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5561d-113">On hello **Choose a base** blade, select a marketplace image for hello VM.</span></span>
1. <span data-ttu-id="5561d-114">Na hello **virtuálního počítače** okno, zadejte název pro nový virtuální počítač hello v hello **název virtuálního počítače** textové pole.</span><span class="sxs-lookup"><span data-stu-id="5561d-114">On hello **Virtual machine** blade, enter a name for hello new virtual machine in hello **Virtual machine name** text box.</span></span>

    ![Okno prostředí virtuálních počítačů](./media/devtest-lab-add-vm/devtestlab-lab-add-first-vm.png)

1. <span data-ttu-id="5561d-116">Zadejte **uživatelské jméno** , jsou udělena oprávnění správce na virtuálním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="5561d-116">Enter a **User Name** that is granted administrator privileges on hello virtual machine.</span></span>  
1. <span data-ttu-id="5561d-117">Zadejte heslo do hello textové pole s názvem bez přípony **zadejte hodnotu**.</span><span class="sxs-lookup"><span data-stu-id="5561d-117">Enter a password in hello text field labeled **Type a value**.</span></span>
1. <span data-ttu-id="5561d-118">Hello **typ disku virtuálního počítače** Určuje, jaký typ úložiště disku je povoleno pro hello virtuálních počítačů v testovacím prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="5561d-118">hello **Virtual machine disk type** determines which storage disk type is allowed for hello virtual machines in hello lab.</span></span>
1. <span data-ttu-id="5561d-119">Vyberte **velikost virtuálního počítače** a vyberte jednu z hello předdefinované položky, které zadejte hello jader procesoru, velikost paměti RAM a velikost pevného disku hello toocreate hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5561d-119">Select **Virtual machine size** and select one of hello predefined items that specify hello processor cores, RAM size, and hello hard drive size of hello VM toocreate.</span></span>
1. <span data-ttu-id="5561d-120">Vyberte **artefakty** a - z hello seznam artefakty - vyberte a nakonfigurujte hello artefakty, že chcete tooadd toohello základní image.</span><span class="sxs-lookup"><span data-stu-id="5561d-120">Select **Artifacts** and - from hello list of artifacts - select and configure hello artifacts that you want tooadd toohello base image.</span></span>
    <span data-ttu-id="5561d-121">**Poznámka:** nové Labs tooDevTest nebo toohello konfigurace artefaktů, najdete v [přidat existující artefaktů tooa virtuálních počítačů](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) části a vraťte se sem po dokončení.</span><span class="sxs-lookup"><span data-stu-id="5561d-121">**Note:** If you're new tooDevTest Labs or configuring artifacts, refer toohello [Add an existing artifact tooa VM](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) section, and then return here when finished.</span></span>
1. <span data-ttu-id="5561d-122">Vyberte **vytvořit** tooadd hello zadaný testovacím toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5561d-122">Select **Create** tooadd hello specified VM toohello lab.</span></span>

   <span data-ttu-id="5561d-123">Hello testovacím zobrazuje hello stav vytvoření hello VM - nejprve jako **vytváření**, pak jako **systémem** po hello virtuálního počítače byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="5561d-123">hello lab blade displays hello status of hello VM's creation - first as **Creating**, then as **Running** after hello VM has been started.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5561d-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5561d-124">Next steps</span></span>
* <span data-ttu-id="5561d-125">Jednou hello vytvořil virtuální počítač, můžete připojit toohello virtuálních počítačů tak, že vyberete **Connect** v okně hello Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5561d-125">Once hello VM has been created, you can connect toohello VM by selecting **Connect** on hello VM's blade.</span></span>
* <span data-ttu-id="5561d-126">Podívejte se na [přidání testovacího prostředí virtuálních počítačů tooa](devtest-lab-add-vm.md) podrobnější informace o přidávání dalších virtuálních počítačů ve svém testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5561d-126">Check out [Add a VM tooa lab](devtest-lab-add-vm.md) for more complete info about adding subsequent VMs in your lab.</span></span>
* <span data-ttu-id="5561d-127">Prozkoumejte hello [Galerie šablon DevTest Labs Azure Resource Manager QuickStart](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).</span><span class="sxs-lookup"><span data-stu-id="5561d-127">Explore hello [DevTest Labs Azure Resource Manager QuickStart template gallery](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).</span></span>
