---
title: "Spravovat vzorce v Azure DevTest Labs pro vytvoření virtuálních počítačů | Microsoft Docs"
description: "Zjistěte, jak aktualizovat a odebírat vzorce Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 841dd95a-657f-4d80-ba26-59a9b5104fe4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: tarcher
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bfdab5def50158f9b764bbb1e50c2624cc6d5fb3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-azure-devtest-labs-formulas"></a><span data-ttu-id="064da-103">Správa Azure DevTest Labs vzorce</span><span class="sxs-lookup"><span data-stu-id="064da-103">Manage Azure DevTest Labs formulas</span></span>

[!INCLUDE [devtest-lab-formula-definition](../../includes/devtest-lab-formula-definition.md)]

<span data-ttu-id="064da-104">Tento článek ukazuje, jak vytvořit vzorec ze základní (vlastní image, Marketplace image nebo jiného vzorce) nebo stávajícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="064da-104">This article illustrates how to create a formula from either a base (custom image, Marketplace image, or another formula) or an existing VM.</span></span> <span data-ttu-id="064da-105">Tento článek také vás provede Správa existující vzorce.</span><span class="sxs-lookup"><span data-stu-id="064da-105">This article also guides you through managing existing formulas.</span></span>

## <a name="create-a-formula"></a><span data-ttu-id="064da-106">Vytvoření vzorce</span><span class="sxs-lookup"><span data-stu-id="064da-106">Create a formula</span></span>
<span data-ttu-id="064da-107">Každý, kdo má DevTest Labs *uživatelé* oprávnění je možné vytvořit pomocí vzorce jako na základní virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="064da-107">Anyone with DevTest Labs *Users* permissions is able to create VMs using a formula as a base.</span></span> <span data-ttu-id="064da-108">Existují dva způsoby vytvoření vzorce:</span><span class="sxs-lookup"><span data-stu-id="064da-108">There are two ways to create formulas:</span></span> 

* <span data-ttu-id="064da-109">Z základní - použijte, pokud chcete definovat všechny vlastnosti vzorec.</span><span class="sxs-lookup"><span data-stu-id="064da-109">From a base - Use when you want to define all the characteristics of the formula.</span></span>
* <span data-ttu-id="064da-110">Z existujícího testovacího prostředí virtuálního počítače – použijte v případě, že chcete vytvořit vzorec na základě nastavení existujícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="064da-110">From an existing lab VM - Use when you want to create a formula based on the settings of an existing VM.</span></span>

<span data-ttu-id="064da-111">Další informace o přidávání uživatelů a oprávnění najdete v tématu [přidat vlastníků a uživatelé v Azure DevTest Labs](./devtest-lab-add-devtest-user.md).</span><span class="sxs-lookup"><span data-stu-id="064da-111">For more information about adding users and permissions, see [Add owners and users in Azure DevTest Labs](./devtest-lab-add-devtest-user.md).</span></span>

### <a name="create-a-formula-from-a-base"></a><span data-ttu-id="064da-112">Vytvoření vzorce od základní</span><span class="sxs-lookup"><span data-stu-id="064da-112">Create a formula from a base</span></span>
<span data-ttu-id="064da-113">Následující postup vás provede procesem vytvoření vzorec z vlastní image, Marketplace image nebo jiného vzorce.</span><span class="sxs-lookup"><span data-stu-id="064da-113">The following steps guide you through the process of creating a formula from a custom image, Marketplace image, or another formula.</span></span>

1. <span data-ttu-id="064da-114">Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="064da-114">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

2. <span data-ttu-id="064da-115">Vyberte **více služeb**a potom vyberte **DevTest Labs** ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="064da-115">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>

3. <span data-ttu-id="064da-116">Ze seznamu labs vyberte požadované testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="064da-116">From the list of labs, select the desired lab.</span></span>  

4. <span data-ttu-id="064da-117">V okně v prostředí, vyberte **vzorce (opakovaně použitelné základů)**.</span><span class="sxs-lookup"><span data-stu-id="064da-117">On the lab's blade, select **Formulas (reusable bases)**.</span></span>
   
    ![Vzorce nabídky](./media/devtest-lab-create-formulas/lab-settings-formulas.png)

5. <span data-ttu-id="064da-119">Na **vzorce** vyberte **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="064da-119">On the **Formulas** blade, select **+ Add**.</span></span>
   
    ![Přidat vzorec.](./media/devtest-lab-create-formulas/add-formula.png)

6. <span data-ttu-id="064da-121">Na **zvolte na základní** okně vyberte základní (vlastní image, Marketplace image nebo vzorec), ze kterého chcete vytvořit vzorec.</span><span class="sxs-lookup"><span data-stu-id="064da-121">On the **Choose a base** blade, select the base (custom image, Marketplace image, or formula) from which you want to create the formula.</span></span>
   
    ![Základní seznam](./media/devtest-lab-create-formulas/base-list.png)

7. <span data-ttu-id="064da-123">Na **vytvořit vzorec** okno, zadejte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="064da-123">On the **Create formula** blade, specify the following values:</span></span>
   
    * <span data-ttu-id="064da-124">**Název vzorce** – zadejte název vzorce.</span><span class="sxs-lookup"><span data-stu-id="064da-124">**Formula name** - Enter a name for your formula.</span></span> <span data-ttu-id="064da-125">Tato hodnota se zobrazí v seznamu základní Image při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="064da-125">This value is displayed in the list of base images when you create a VM.</span></span> <span data-ttu-id="064da-126">Název je ověřit, protože jej zadáte, a pokud není platný, zprávu označuje požadavky na platný název.</span><span class="sxs-lookup"><span data-stu-id="064da-126">The name is validated as you type it, and if not valid, a message indicates the requirements for a valid name.</span></span>
    * <span data-ttu-id="064da-127">**Popis** -Zadejte výstižný popis vzorce.</span><span class="sxs-lookup"><span data-stu-id="064da-127">**Description** - Enter a meaningful description for your formula.</span></span> <span data-ttu-id="064da-128">Tato hodnota je k dispozici vzorec místní nabídce, když vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="064da-128">This value is available from the formula's context menu when you create a VM.</span></span>
    * <span data-ttu-id="064da-129">**Uživatelské jméno** -zadejte uživatelské jméno, které jsou udělena oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="064da-129">**User name** - Enter a user name that is granted administrator privileges.</span></span>
    * <span data-ttu-id="064da-130">**Heslo** – zadejte - nebo vyberte z rozevíracího seznamu - hodnotu, která souvisí s tajný klíč (heslo), který chcete použít pro zadaného uživatele.</span><span class="sxs-lookup"><span data-stu-id="064da-130">**Password** - Enter - or select from the dropdown - a value that is associated with the secret (password) that you want to use for the specified user.</span></span> <span data-ttu-id="064da-131">Další informace o těchto tajných klíčů najdete v tématu [Azure DevTest Labs: osobním úložišti tajný](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store/).</span><span class="sxs-lookup"><span data-stu-id="064da-131">For more information about the secrets, see [Azure DevTest Labs: Personal secret store](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store/).</span></span>
    * <span data-ttu-id="064da-132">**Typ disku virtuálního počítače** – zadejte buď HDD (jednotku pevného disku) nebo SSD (jednotka SSD) k označení disku typů úložiště, které je povoleno pro virtuální počítače zřízené tuto základní image používá.</span><span class="sxs-lookup"><span data-stu-id="064da-132">**Virtual machine disk type** - Specify either HDD (hard-disk drive) or SSD (solid-state drive) to indicate which storage disk type is allowed for the virtual machines provisioned using this base image.</span></span>
    * <span data-ttu-id="064da-133">** Virtuální počítač velikost ** – vyberte jednu z předdefinovaných položky, které zadejte jader procesoru, velikosti paměti RAM a velikost pevného disku pro vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="064da-133">** Virtual machine size** - Select one of the predefined items that specify the processor cores, RAM size, and the hard drive size of the VM to create.</span></span> 
    * <span data-ttu-id="064da-134">**Artefakty** – vyberte, chcete-li otevřít **přidat artefakty** okno, ve kterém můžete vybrat a nakonfigurovat artefaktů, které chcete přidat do základní bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="064da-134">**Artifacts** - Select to open the **Add artifacts** blade, in which you select and configure the artifacts that you want to add to the base image.</span></span> <span data-ttu-id="064da-135">Další informace o artefakty najdete v tématu [artefakty Správa virtuálních počítačů v Azure DevTest Labs](./devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="064da-135">For more information about artifacts, see [Manage VM artifacts in Azure DevTest Labs](./devtest-lab-add-vm-with-artifacts.md).</span></span>
    * <span data-ttu-id="064da-136">**Upřesňující nastavení** – vyberte, chcete-li otevřít **Upřesnit** okno, kde můžete nakonfigurovat následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="064da-136">**Advanced settings** - Select to open the **Advanced** blade where you configure the following settings:</span></span>
        * <span data-ttu-id="064da-137">**Virtuální síť** -zadejte požadované virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="064da-137">**Virtual network** - Specify the desired virtual network.</span></span>
        * <span data-ttu-id="064da-138">**Podsíť** -zadejte požadovanou podsíť.</span><span class="sxs-lookup"><span data-stu-id="064da-138">**Subnet** - Specify the desired subnet.</span></span>    
        * <span data-ttu-id="064da-139">**Konfiguraci IP adresy** – určete, zda mají veřejné, privátní nebo sdílet IP adres.</span><span class="sxs-lookup"><span data-stu-id="064da-139">**IP address configuration** - Specify if you want the Public, Private, or Shared IP addresses.</span></span> <span data-ttu-id="064da-140">Další informace o sdílené IP adresy najdete v tématu [Rady pro pochopení sdílet IP adresy v Azure DevTest Labs](./devtest-lab-shared-ip.md).</span><span class="sxs-lookup"><span data-stu-id="064da-140">For more information about shared IP addresses, see [Understand shared IP addresses in Azure DevTest Labs](./devtest-lab-shared-ip.md).</span></span>
        * <span data-ttu-id="064da-141">**Nastavit tento počítač vymahatelných** -provádění na počítač "vymahatelných" znamená, že jej nebude přiřadit vlastnictví v době vytvoření.</span><span class="sxs-lookup"><span data-stu-id="064da-141">**Make this machine claimable** - Making a machine "claimable" means that it will not be assigned ownership at the time of creation.</span></span> <span data-ttu-id="064da-142">Místo toho lab uživatelů bude moci převzít vlastnictví ("deklarace identity") počítače v okně v prostředí.</span><span class="sxs-lookup"><span data-stu-id="064da-142">Instead lab users will be able to take ownership ("claim") the machine in the lab's blade.</span></span>     
    * <span data-ttu-id="064da-143">**Obrázek** – toto pole zobrazuje název základní image jste vybrali na předchozí okna.</span><span class="sxs-lookup"><span data-stu-id="064da-143">**Image** - This field displays name of the base image you selected on the previous blade.</span></span> 
     
       ![Vytvoření vzorce](./media/devtest-lab-create-formulas/create-formula.png)

8. <span data-ttu-id="064da-145">Vyberte **vytvořit** vytvořit vzorec.</span><span class="sxs-lookup"><span data-stu-id="064da-145">Select **Create** to create the formula.</span></span>

9. <span data-ttu-id="064da-146">Po vytvoření vzorec, se zobrazí v seznamu na **vzorce** okno.</span><span class="sxs-lookup"><span data-stu-id="064da-146">When the formula has been created, it displays in the list on the **Formulas** blade.</span></span>

### <a name="create-a-formula-from-a-vm"></a><span data-ttu-id="064da-147">Vytvoření vzorce z virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="064da-147">Create a formula from a VM</span></span>
<span data-ttu-id="064da-148">Následující postup vás provede procesem vytvoření vzorec podle stávajícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="064da-148">The following steps guide you through the process of creating a formula based on an existing VM.</span></span> 

> [!NOTE]
> <span data-ttu-id="064da-149">Pokud chcete vytvořit vzorec z virtuálního počítače, musí být nejprve vytvořen virtuální počítač po 30. března 2016.</span><span class="sxs-lookup"><span data-stu-id="064da-149">To create a formula from a VM, the VM must have been created after March 30, 2016.</span></span> 
> 
> 

1. <span data-ttu-id="064da-150">Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="064da-150">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="064da-151">Vyberte **více služeb**a potom vyberte **DevTest Labs** ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="064da-151">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>
3. <span data-ttu-id="064da-152">Ze seznamu labs vyberte požadované testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="064da-152">From the list of labs, select the desired lab.</span></span>  
4. <span data-ttu-id="064da-153">V tomto prostředí **přehled** okně vyberte virtuální počítač, ze kterého chcete vytvořit vzorec.</span><span class="sxs-lookup"><span data-stu-id="064da-153">On the lab's **Overview** blade, select the VM from which you wish to create the formula.</span></span>
   
    ![Virtuální laboratoře počítače](./media/devtest-lab-create-formulas/my-vms.png)
5. <span data-ttu-id="064da-155">V okně Virtuálního počítače, vyberte **vytvořit vzorec (opakovaně použitelné base)**.</span><span class="sxs-lookup"><span data-stu-id="064da-155">On the VM's blade, select **Create formula (reusable base)**.</span></span>
   
    ![Vytvoření vzorce](./media/devtest-lab-create-formulas/create-formula-menu.png)
6. <span data-ttu-id="064da-157">Na **vytvořit vzorec** okno, zadejte **název** a **popis** pro nový vzorec.</span><span class="sxs-lookup"><span data-stu-id="064da-157">On the **Create formula** blade, enter a **Name** and **Description** for your new formula.</span></span>
   
    ![Vzorce okno vytvořit](./media/devtest-lab-create-formulas/create-formula-blade.png)
7. <span data-ttu-id="064da-159">Vyberte **OK** vytvořit vzorec.</span><span class="sxs-lookup"><span data-stu-id="064da-159">Select **OK** to create the formula.</span></span>

## <a name="modify-a-formula"></a><span data-ttu-id="064da-160">Upravit vzorec.</span><span class="sxs-lookup"><span data-stu-id="064da-160">Modify a formula</span></span>
<span data-ttu-id="064da-161">Pokud chcete upravit vzorec, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="064da-161">To modify a formula, follow these steps:</span></span>

1. <span data-ttu-id="064da-162">Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="064da-162">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="064da-163">Vyberte **více služeb**a potom vyberte **DevTest Labs** ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="064da-163">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>
3. <span data-ttu-id="064da-164">Ze seznamu labs vyberte požadované testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="064da-164">From the list of labs, select the desired lab.</span></span>  
4. <span data-ttu-id="064da-165">V okně v prostředí, vyberte **vzorce (opakovaně použitelné základů)**.</span><span class="sxs-lookup"><span data-stu-id="064da-165">On the lab's blade, select **Formulas (reusable bases)**.</span></span>
   
    ![Vzorce nabídky](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. <span data-ttu-id="064da-167">Na **testovacím vzorce** okně vyberte vzorec, který chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="064da-167">On the **Lab formulas** blade, select the formula you wish to modify.</span></span>
6. <span data-ttu-id="064da-168">Na **aktualizovat vzorec** okně udělejte požadované úpravy a vyberte **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="064da-168">On the **Update formula** blade, make the desired edits, and select **Update**.</span></span>

## <a name="delete-a-formula"></a><span data-ttu-id="064da-169">Odstranit vzorec.</span><span class="sxs-lookup"><span data-stu-id="064da-169">Delete a formula</span></span>
<span data-ttu-id="064da-170">Pokud chcete odstranit vzorec, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="064da-170">To delete a formula, follow these steps:</span></span>

1. <span data-ttu-id="064da-171">Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="064da-171">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="064da-172">Vyberte **více služeb**a potom vyberte **DevTest Labs** ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="064da-172">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>
3. <span data-ttu-id="064da-173">Ze seznamu labs vyberte požadované testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="064da-173">From the list of labs, select the desired lab.</span></span>  
4. <span data-ttu-id="064da-174">V testovacím prostředí **nastavení** vyberte **vzorce**.</span><span class="sxs-lookup"><span data-stu-id="064da-174">On the lab **Settings** blade, select **Formulas**.</span></span>
   
    ![Vzorce nabídky](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. <span data-ttu-id="064da-176">Na **testovacím vzorce** okně vyberte se třemi tečkami napravo od vzorec chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="064da-176">On the **Lab formulas** blade, select the ellipsis to the right of the formula you wish to delete.</span></span>
   
    ![Vzorce nabídky](./media/devtest-lab-manage-formulas/lab-formulas-blade.png)
6. <span data-ttu-id="064da-178">V místní nabídce vzorec, vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="064da-178">On the formula's context menu, select **Delete**.</span></span>
   
    ![Vzorce kontextové nabídky](./media/devtest-lab-manage-formulas/formula-delete-context-menu.png)
7. <span data-ttu-id="064da-180">Vyberte **Ano** do dialogového okna potvrzení odstranění.</span><span class="sxs-lookup"><span data-stu-id="064da-180">Select **Yes** to the deletion confirmation dialog.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="064da-181">Příspěvky blogu související</span><span class="sxs-lookup"><span data-stu-id="064da-181">Related blog posts</span></span>
* [<span data-ttu-id="064da-182">Vlastní Image nebo vzorce?</span><span class="sxs-lookup"><span data-stu-id="064da-182">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a><span data-ttu-id="064da-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="064da-183">Next steps</span></span>
<span data-ttu-id="064da-184">Po vytvoření vzorec pro použití při vytváření virtuálního počítače, dalším krokem je [přidat virtuální počítač do testovacího prostředí](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="064da-184">Once you have created a formula for use when creating a VM, the next step is to [add a VM to your lab](devtest-lab-add-vm-with-artifacts.md).</span></span>

