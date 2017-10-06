---
title: "aaaAdd vymahatelných testovacího prostředí tooa virtuálních počítačů v Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak tooadd testovacího prostředí tooa vymahatelných virtuálního počítače v Azure DevTest Labs"
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
ms.openlocfilehash: fe6385ae2e59b9636b82aec250dc3a1f8a40ba5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-claimable-vm-tooa-lab-in-azure-devtest-labs"></a><span data-ttu-id="86254-103">Přidání testovacího prostředí vymahatelných tooa virtuálních počítačů v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="86254-103">Add a claimable VM tooa lab in Azure DevTest Labs</span></span>
<span data-ttu-id="86254-104">Přidání testovacího prostředí vymahatelných tooa virtuálních počítačů v podobným způsobem toohow jste [přidat standardní virtuální počítač](devtest-lab-add-vm.md) – z *základní* to znamená buď [vlastní image](devtest-lab-create-template.md), [vzorec](devtest-lab-manage-formulas.md), nebo [Marketplace image](devtest-lab-configure-marketplace-images.md).</span><span class="sxs-lookup"><span data-stu-id="86254-104">You add a claimable VM tooa lab in a similar manner toohow you [add a standard VM](devtest-lab-add-vm.md) – from a *base* that is either a [custom image](devtest-lab-create-template.md), [formula](devtest-lab-manage-formulas.md), or [Marketplace image](devtest-lab-configure-marketplace-images.md).</span></span> <span data-ttu-id="86254-105">Tento kurz vás provede procesem pomocí hello Azure portálu tooadd vymahatelných testovacího virtuálního počítače tooa prostředí v DevTest Labs a zobrazuje hello proces že uživatel bude postupovat tooclaim hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="86254-105">This tutorial walks you through using hello Azure portal tooadd a claimable VM tooa lab in DevTest Labs, and shows hello process a user follows tooclaim hello VM.</span></span>

> [!NOTE]
> <span data-ttu-id="86254-106">Pokud nasadíte virtuální počítače testovacího prostředí prostřednictvím [šablon Azure Resource Manageru](devtest-lab-create-environment-from-arm.md), můžete vytvořit virtuální počítače vymahatelných nastavení hello **allowClaim** vlastnost tootrue v části Vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="86254-106">If you deploy lab VMs through [Azure Resource Manager templates](devtest-lab-create-environment-from-arm.md), you can create claimable VMs by setting hello **allowClaim** property tootrue in hello properties section.</span></span>
>
>

## <a name="steps-tooadd-a-claimable-vm-tooa-lab-in-azure-devtest-labs"></a><span data-ttu-id="86254-107">Kroky tooadd vymahatelných testovacího prostředí tooa virtuálních počítačů v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="86254-107">Steps tooadd a claimable VM tooa lab in Azure DevTest Labs</span></span>
1. <span data-ttu-id="86254-108">Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="86254-108">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="86254-109">Vyberte **více služeb**a potom vyberte **DevTest Labs** hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="86254-109">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
1. <span data-ttu-id="86254-110">Ze seznamu hello labs, vyberte hello testovacího prostředí do kterých chcete toocreate hello vymahatelných virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="86254-110">From hello list of labs, select hello lab in which you want toocreate hello claimable VM.</span></span>  
1. <span data-ttu-id="86254-111">V testovacím hello **přehled** vyberte **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="86254-111">On hello lab's **Overview** blade, select **+ Add**.</span></span>  

    ![Virtuální počítač tlačítko Přidat](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. <span data-ttu-id="86254-113">Na hello **zvolte na základní** okně vyberte základ pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="86254-113">On hello **Choose a base** blade, select a base for hello VM.</span></span>
1. <span data-ttu-id="86254-114">Na hello **virtuálního počítače** okno, zadejte název pro nový virtuální počítač hello v hello **název virtuálního počítače** textové pole.</span><span class="sxs-lookup"><span data-stu-id="86254-114">On hello **Virtual machine** blade, enter a name for hello new virtual machine in hello **Virtual machine name** text box.</span></span>

    ![Okno prostředí virtuálních počítačů](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. <span data-ttu-id="86254-116">Zadejte **uživatelské jméno** , jsou udělena oprávnění správce na virtuálním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="86254-116">Enter a **User Name** that is granted administrator privileges on hello virtual machine.</span></span>  
1. <span data-ttu-id="86254-117">Pokud chcete, aby toouse heslo uložené v vaše [tajný úložiště](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store), vyberte **použít uložené tajný klíč**a zadejte hodnotu klíče, která odpovídá tooyour tajný klíč (heslo).</span><span class="sxs-lookup"><span data-stu-id="86254-117">If you want toouse a password stored in your [secret store](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store), select **Use a saved secret**, and specify a key value that corresponds tooyour secret (password).</span></span> <span data-ttu-id="86254-118">Jinak, zadejte heslo do hello textové pole s názvem bez přípony **zadejte hodnotu**.</span><span class="sxs-lookup"><span data-stu-id="86254-118">Otherwise, enter a password in hello text field labeled **Type a value**.</span></span>
1. <span data-ttu-id="86254-119">Hello **typ disku virtuálního počítače** Určuje, jaký typ úložiště disku je povoleno pro hello virtuálních počítačů v testovacím prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="86254-119">hello **Virtual machine disk type** determines which storage disk type is allowed for hello virtual machines in hello lab.</span></span>
1. <span data-ttu-id="86254-120">Vyberte **velikost virtuálního počítače** a vyberte jednu z hello předdefinované položky, které zadejte hello jader procesoru, velikost paměti RAM a velikost pevného disku hello toocreate hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="86254-120">Select **Virtual machine size** and select one of hello predefined items that specify hello processor cores, RAM size, and hello hard drive size of hello VM toocreate.</span></span>
1. <span data-ttu-id="86254-121">Vyberte **artefakty** a ze seznamu hello artefaktů, vyberte a nakonfigurujte hello artefakty, že chcete tooadd toohello základní image.</span><span class="sxs-lookup"><span data-stu-id="86254-121">Select **Artifacts** and from hello list of artifacts, select and configure hello artifacts that you want tooadd toohello base image.</span></span> <span data-ttu-id="86254-122">Pokud je nový Labs tooDevTest nebo toohello konfigurace artefaktů, najdete v [přidat existující artefaktů tooa virtuálních počítačů](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) části a vraťte se sem po dokončení.</span><span class="sxs-lookup"><span data-stu-id="86254-122">If you're new tooDevTest Labs or configuring artifacts, refer toohello [Add an existing artifact tooa VM](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) section, and then return here when finished.</span></span>
1. <span data-ttu-id="86254-123">Vyberte **upřesňující nastavení** tooconfigure hello síť virtuálních počítačů na možnosti možnosti a vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="86254-123">Select **Advanced settings** tooconfigure hello VM's network options and expiration options.</span></span> <span data-ttu-id="86254-124">V části **deklarace identity možnosti**, zvolte **Ano** toomake hello počítač vymahatelných.</span><span class="sxs-lookup"><span data-stu-id="86254-124">Under **Claim options**, choose **Yes** toomake hello machine claimable.</span></span>

  ![Zvolte toomake hello vymahatelných virtuálních počítačů.](./media/devtest-lab-add-vm/devtestlab-claim-VM-option.png)

1. <span data-ttu-id="86254-126">Chcete-li tooview nebo zkopírujte hello šablony Azure Resource Manageru, najdete v toohello [šablony uložit Azure Resource Manageru](devtest-lab-add-vm.md#save-azure-resource-manager-template) části a sem vraťte po dokončení.</span><span class="sxs-lookup"><span data-stu-id="86254-126">If you want tooview or copy hello Azure Resource Manager template, refer toohello [Save Azure Resource Manager template](devtest-lab-add-vm.md#save-azure-resource-manager-template) section, and return here when finished.</span></span>
1. <span data-ttu-id="86254-127">Vyberte **vytvořit** tooadd hello zadaný testovacím toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="86254-127">Select **Create** tooadd hello specified VM toohello lab.</span></span>
1. <span data-ttu-id="86254-128">Hello testovacím zobrazuje hello stav vytvoření hello VM - nejprve jako **vytváření**, pak jako **systémem** po hello virtuálního počítače byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="86254-128">hello lab blade displays hello status of hello VM's creation - first as **Creating**, then as **Running** after hello VM has been started.</span></span>


## <a name="using-a-claimable-vm"></a><span data-ttu-id="86254-129">Pomocí vymahatelných virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="86254-129">Using a claimable VM</span></span>

<span data-ttu-id="86254-130">Uživatel může deklarace identity žádné virtuální počítače ze seznamu hello "Vymahatelných virtuální počítače" jedním z těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="86254-130">A user can claim any VM from hello list of "Claimable virtual machines" by doing one of these steps:</span></span>

* <span data-ttu-id="86254-131">Ze seznamu hello "Vymahatelných virtuální počítače" hello dolní části okna Přehled hello laboratoře, klikněte pravým tlačítkem na jednu z hello virtuální počítače v seznamu hello a zvolte **deklarace identity počítače**.</span><span class="sxs-lookup"><span data-stu-id="86254-131">From hello list of "Claimable virtual machines" at hello bottom of hello lab's Overview blade, right-click on one of hello VMs in hello list and choose **Claim machine**.</span></span>

 ![Požádat o konkrétní vymahatelných virtuální počítač.](./media/devtest-lab-add-vm/devtestlab-claim-VM.png)


* <span data-ttu-id="86254-133">Hello horní části hello **přehled** okně zvolte **všechny deklarace**.</span><span class="sxs-lookup"><span data-stu-id="86254-133">At hello top of hello **Overview** blade, choose **Claim any**.</span></span> <span data-ttu-id="86254-134">Náhodné virtuální počítač je přiřazen seznamu hello vymahatelných virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="86254-134">A random virtual machine is assigned from hello list of claimable VMs.</span></span>

 ![Požádat o žádné vymahatelných virtuální počítače.](./media/devtest-lab-add-vm/devtestlab-claim-any.png)


<span data-ttu-id="86254-136">Poté, co uživatel deklarací virtuálního počítače, je do jejich seznam "Moje virtuální počítače" Přesunout nahoru a již není vymahatelných jiným uživatelem.</span><span class="sxs-lookup"><span data-stu-id="86254-136">After a user claims a VM, it is moved up into their list of "My virtual machines" and is no longer claimable by any other user.</span></span>

## <a name="next-steps"></a><span data-ttu-id="86254-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="86254-137">Next steps</span></span>
* <span data-ttu-id="86254-138">Jednou hello vytvořil virtuální počítač, můžete připojit toohello virtuálních počítačů tak, že vyberete **Connect** v okně hello Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="86254-138">Once hello VM has been created, you can connect toohello VM by selecting **Connect** on hello VM's blade.</span></span>
* <span data-ttu-id="86254-139">Prozkoumejte hello [Galerie šablon DevTest Labs Azure Resource Manager rychlý start](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates)</span><span class="sxs-lookup"><span data-stu-id="86254-139">Explore hello [DevTest Labs Azure Resource Manager QuickStart template gallery](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates)</span></span>
