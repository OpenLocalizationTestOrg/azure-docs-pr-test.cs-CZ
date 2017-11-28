---
title: "vzorce aaaManage v Azure DevTest Labs toocreate virtuální počítače | Microsoft Docs"
description: "Zjistěte, jak tooupdate a odebrat vzorce Azure DevTest Labs"
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
ms.openlocfilehash: 855debe46f3b70cc45ea5d55869663b64e225124
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-devtest-labs-formulas"></a><span data-ttu-id="e624c-103">Správa Azure DevTest Labs vzorce</span><span class="sxs-lookup"><span data-stu-id="e624c-103">Manage Azure DevTest Labs formulas</span></span>

[!INCLUDE [devtest-lab-formula-definition](../../includes/devtest-lab-formula-definition.md)]

<span data-ttu-id="e624c-104">Tento článek ukazuje, jak toocreate vzorec ze základní (vlastní image, Marketplace image nebo jiného vzorce) nebo stávajícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e624c-104">This article illustrates how toocreate a formula from either a base (custom image, Marketplace image, or another formula) or an existing VM.</span></span> <span data-ttu-id="e624c-105">Tento článek také vás provede Správa existující vzorce.</span><span class="sxs-lookup"><span data-stu-id="e624c-105">This article also guides you through managing existing formulas.</span></span>

## <a name="create-a-formula"></a><span data-ttu-id="e624c-106">Vytvoření vzorce</span><span class="sxs-lookup"><span data-stu-id="e624c-106">Create a formula</span></span>
<span data-ttu-id="e624c-107">Každý, kdo má DevTest Labs *uživatelé* oprávnění je možné toocreate virtuálních počítačů pomocí vzorce jako základ.</span><span class="sxs-lookup"><span data-stu-id="e624c-107">Anyone with DevTest Labs *Users* permissions is able toocreate VMs using a formula as a base.</span></span> <span data-ttu-id="e624c-108">Existují dva způsoby toocreate vzorce:</span><span class="sxs-lookup"><span data-stu-id="e624c-108">There are two ways toocreate formulas:</span></span> 

* <span data-ttu-id="e624c-109">Od základní - použijte, pokud chcete toodefine všechny hello charakteristiky hello vzorec.</span><span class="sxs-lookup"><span data-stu-id="e624c-109">From a base - Use when you want toodefine all hello characteristics of hello formula.</span></span>
* <span data-ttu-id="e624c-110">Z existujícího testovacího prostředí virtuálního počítače – použijte v případě, že chcete toocreate vzorec na základě hello nastavení existujícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e624c-110">From an existing lab VM - Use when you want toocreate a formula based on hello settings of an existing VM.</span></span>

<span data-ttu-id="e624c-111">Další informace o přidávání uživatelů a oprávnění najdete v tématu [přidat vlastníků a uživatelé v Azure DevTest Labs](./devtest-lab-add-devtest-user.md).</span><span class="sxs-lookup"><span data-stu-id="e624c-111">For more information about adding users and permissions, see [Add owners and users in Azure DevTest Labs](./devtest-lab-add-devtest-user.md).</span></span>

### <a name="create-a-formula-from-a-base"></a><span data-ttu-id="e624c-112">Vytvoření vzorce od základní</span><span class="sxs-lookup"><span data-stu-id="e624c-112">Create a formula from a base</span></span>
<span data-ttu-id="e624c-113">Hello následující kroky vás provede procesem hello vytvoření vzorec z vlastní image, Marketplace image nebo jiného vzorce.</span><span class="sxs-lookup"><span data-stu-id="e624c-113">hello following steps guide you through hello process of creating a formula from a custom image, Marketplace image, or another formula.</span></span>

1. <span data-ttu-id="e624c-114">Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="e624c-114">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

2. <span data-ttu-id="e624c-115">Vyberte **více služeb**a potom vyberte **DevTest Labs** hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="e624c-115">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>

3. <span data-ttu-id="e624c-116">Ze seznamu hello labs vyberte požadované prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="e624c-116">From hello list of labs, select hello desired lab.</span></span>  

4. <span data-ttu-id="e624c-117">V okně prostředí hello vyberte **vzorce (opakovaně použitelné základů)**.</span><span class="sxs-lookup"><span data-stu-id="e624c-117">On hello lab's blade, select **Formulas (reusable bases)**.</span></span>
   
    ![Vzorce nabídky](./media/devtest-lab-create-formulas/lab-settings-formulas.png)

5. <span data-ttu-id="e624c-119">Na hello **vzorce** vyberte **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="e624c-119">On hello **Formulas** blade, select **+ Add**.</span></span>
   
    ![Přidat vzorec.](./media/devtest-lab-create-formulas/add-formula.png)

6. <span data-ttu-id="e624c-121">Na hello **zvolte na základní** okně vyberte hello základní (vlastní image, Marketplace image nebo vzorec) ze kterého mají být toocreate hello vzorec.</span><span class="sxs-lookup"><span data-stu-id="e624c-121">On hello **Choose a base** blade, select hello base (custom image, Marketplace image, or formula) from which you want toocreate hello formula.</span></span>
   
    ![Základní seznam](./media/devtest-lab-create-formulas/base-list.png)

7. <span data-ttu-id="e624c-123">Na hello **vytvořit vzorec** okno, zadejte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="e624c-123">On hello **Create formula** blade, specify hello following values:</span></span>
   
    * <span data-ttu-id="e624c-124">**Název vzorce** – zadejte název vzorce.</span><span class="sxs-lookup"><span data-stu-id="e624c-124">**Formula name** - Enter a name for your formula.</span></span> <span data-ttu-id="e624c-125">Tato hodnota se zobrazí v seznamu hello základní Image při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e624c-125">This value is displayed in hello list of base images when you create a VM.</span></span> <span data-ttu-id="e624c-126">Název Hello je ověřit, protože jej zadáte, a pokud není platný, zprávu označuje hello požadavky platný název.</span><span class="sxs-lookup"><span data-stu-id="e624c-126">hello name is validated as you type it, and if not valid, a message indicates hello requirements for a valid name.</span></span>
    * <span data-ttu-id="e624c-127">**Popis** -Zadejte výstižný popis vzorce.</span><span class="sxs-lookup"><span data-stu-id="e624c-127">**Description** - Enter a meaningful description for your formula.</span></span> <span data-ttu-id="e624c-128">Tato hodnota je k dispozici hello vzorec kontextové nabídky, při vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e624c-128">This value is available from hello formula's context menu when you create a VM.</span></span>
    * <span data-ttu-id="e624c-129">**Uživatelské jméno** -zadejte uživatelské jméno, které jsou udělena oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="e624c-129">**User name** - Enter a user name that is granted administrator privileges.</span></span>
    * <span data-ttu-id="e624c-130">**Heslo** – zadejte - nebo vyberte z rozevíracího seznamu hello - hodnotu, která souvisí s hello heslo, že chcete toouse pro zadaného uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="e624c-130">**Password** - Enter - or select from hello dropdown - a value that is associated with hello secret (password) that you want toouse for hello specified user.</span></span> <span data-ttu-id="e624c-131">Další informace o hello tajných klíčů najdete v tématu [Azure DevTest Labs: osobním úložišti tajný](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store/).</span><span class="sxs-lookup"><span data-stu-id="e624c-131">For more information about hello secrets, see [Azure DevTest Labs: Personal secret store](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store/).</span></span>
    * <span data-ttu-id="e624c-132">**Typ disku virtuálního počítače** – zadejte buď HDD (jednotku pevného disku) nebo tooindicate SSD (solid-state jednotka), které úložiště typ disku je povoleno pro hello virtuální počítače zřízené tuto základní image používá.</span><span class="sxs-lookup"><span data-stu-id="e624c-132">**Virtual machine disk type** - Specify either HDD (hard-disk drive) or SSD (solid-state drive) tooindicate which storage disk type is allowed for hello virtual machines provisioned using this base image.</span></span>
    * <span data-ttu-id="e624c-133">** Virtuální počítač velikost ** – vyberte jednu z předdefinovaných hello položky, které zadejte hello jader procesoru, velikost paměti RAM a velikost pevného disku hello toocreate hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e624c-133">** Virtual machine size** - Select one of hello predefined items that specify hello processor cores, RAM size, and hello hard drive size of hello VM toocreate.</span></span> 
    * <span data-ttu-id="e624c-134">**Artefakty** -vyberte tooopen hello **přidat artefakty** okno, ve kterém můžete vybrat a nakonfigurovat hello artefakty, že chcete tooadd toohello základní image.</span><span class="sxs-lookup"><span data-stu-id="e624c-134">**Artifacts** - Select tooopen hello **Add artifacts** blade, in which you select and configure hello artifacts that you want tooadd toohello base image.</span></span> <span data-ttu-id="e624c-135">Další informace o artefakty najdete v tématu [artefakty Správa virtuálních počítačů v Azure DevTest Labs](./devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="e624c-135">For more information about artifacts, see [Manage VM artifacts in Azure DevTest Labs](./devtest-lab-add-vm-with-artifacts.md).</span></span>
    * <span data-ttu-id="e624c-136">**Upřesňující nastavení** -vyberte tooopen hello **Upřesnit** okno, kde můžete konfigurovat hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="e624c-136">**Advanced settings** - Select tooopen hello **Advanced** blade where you configure hello following settings:</span></span>
        * <span data-ttu-id="e624c-137">**Virtuální síť** -určit hello požadován virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="e624c-137">**Virtual network** - Specify hello desired virtual network.</span></span>
        * <span data-ttu-id="e624c-138">**Podsíť** -zadejte podsíť hello potřeby.</span><span class="sxs-lookup"><span data-stu-id="e624c-138">**Subnet** - Specify hello desired subnet.</span></span>    
        * <span data-ttu-id="e624c-139">**Konfiguraci IP adresy** – určete, zda mají hello veřejné, privátní nebo sdílet IP adresy.</span><span class="sxs-lookup"><span data-stu-id="e624c-139">**IP address configuration** - Specify if you want hello Public, Private, or Shared IP addresses.</span></span> <span data-ttu-id="e624c-140">Další informace o sdílené IP adresy najdete v tématu [Rady pro pochopení sdílet IP adresy v Azure DevTest Labs](./devtest-lab-shared-ip.md).</span><span class="sxs-lookup"><span data-stu-id="e624c-140">For more information about shared IP addresses, see [Understand shared IP addresses in Azure DevTest Labs](./devtest-lab-shared-ip.md).</span></span>
        * <span data-ttu-id="e624c-141">**Nastavit tento počítač vymahatelných** -provádění na počítač "vymahatelných" znamená, že jej nebude přiřadit vlastnictví v době vytvoření hello.</span><span class="sxs-lookup"><span data-stu-id="e624c-141">**Make this machine claimable** - Making a machine "claimable" means that it will not be assigned ownership at hello time of creation.</span></span> <span data-ttu-id="e624c-142">Místo toho budou uživatelé testovacího prostředí počítače hello možné tootake vlastnictví (dále jen "deklarace identity") v okně prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="e624c-142">Instead lab users will be able tootake ownership ("claim") hello machine in hello lab's blade.</span></span>     
    * <span data-ttu-id="e624c-143">**Obrázek** – toto pole zobrazuje název hello základní bitovou kopii, jste vybrali na předchozí okno hello.</span><span class="sxs-lookup"><span data-stu-id="e624c-143">**Image** - This field displays name of hello base image you selected on hello previous blade.</span></span> 
     
       ![Vytvoření vzorce](./media/devtest-lab-create-formulas/create-formula.png)

8. <span data-ttu-id="e624c-145">Vyberte **vytvořit** toocreate hello vzorec.</span><span class="sxs-lookup"><span data-stu-id="e624c-145">Select **Create** toocreate hello formula.</span></span>

9. <span data-ttu-id="e624c-146">Po vytvoření hello vzorec, se zobrazí v seznamu hello na hello **vzorce** okno.</span><span class="sxs-lookup"><span data-stu-id="e624c-146">When hello formula has been created, it displays in hello list on hello **Formulas** blade.</span></span>

### <a name="create-a-formula-from-a-vm"></a><span data-ttu-id="e624c-147">Vytvoření vzorce z virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e624c-147">Create a formula from a VM</span></span>
<span data-ttu-id="e624c-148">Hello následující kroky vás provedou procesem hello vytvoření vzorec podle stávajícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e624c-148">hello following steps guide you through hello process of creating a formula based on an existing VM.</span></span> 

> [!NOTE]
> <span data-ttu-id="e624c-149">toocreate vzorec z virtuálního počítače, hello virtuálního počítače musí být vytvořen po 30. března 2016.</span><span class="sxs-lookup"><span data-stu-id="e624c-149">toocreate a formula from a VM, hello VM must have been created after March 30, 2016.</span></span> 
> 
> 

1. <span data-ttu-id="e624c-150">Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="e624c-150">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="e624c-151">Vyberte **více služeb**a potom vyberte **DevTest Labs** hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="e624c-151">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
3. <span data-ttu-id="e624c-152">Ze seznamu hello labs vyberte požadované prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="e624c-152">From hello list of labs, select hello desired lab.</span></span>  
4. <span data-ttu-id="e624c-153">V testovacím hello **přehled** okně vyberte hello virtuálních počítačů, ze kterého chcete toocreate hello vzorec.</span><span class="sxs-lookup"><span data-stu-id="e624c-153">On hello lab's **Overview** blade, select hello VM from which you wish toocreate hello formula.</span></span>
   
    ![Virtuální laboratoře počítače](./media/devtest-lab-create-formulas/my-vms.png)
5. <span data-ttu-id="e624c-155">V okně hello Virtuálního počítače, vyberte **vytvořit vzorec (opakovaně použitelné base)**.</span><span class="sxs-lookup"><span data-stu-id="e624c-155">On hello VM's blade, select **Create formula (reusable base)**.</span></span>
   
    ![Vytvoření vzorce](./media/devtest-lab-create-formulas/create-formula-menu.png)
6. <span data-ttu-id="e624c-157">Na hello **vytvořit vzorec** okno, zadejte **název** a **popis** pro nový vzorec.</span><span class="sxs-lookup"><span data-stu-id="e624c-157">On hello **Create formula** blade, enter a **Name** and **Description** for your new formula.</span></span>
   
    ![Vzorce okno vytvořit](./media/devtest-lab-create-formulas/create-formula-blade.png)
7. <span data-ttu-id="e624c-159">Vyberte **OK** toocreate hello vzorec.</span><span class="sxs-lookup"><span data-stu-id="e624c-159">Select **OK** toocreate hello formula.</span></span>

## <a name="modify-a-formula"></a><span data-ttu-id="e624c-160">Upravit vzorec.</span><span class="sxs-lookup"><span data-stu-id="e624c-160">Modify a formula</span></span>
<span data-ttu-id="e624c-161">toomodify vzorec, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e624c-161">toomodify a formula, follow these steps:</span></span>

1. <span data-ttu-id="e624c-162">Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="e624c-162">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="e624c-163">Vyberte **více služeb**a potom vyberte **DevTest Labs** hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="e624c-163">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
3. <span data-ttu-id="e624c-164">Ze seznamu hello labs vyberte požadované prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="e624c-164">From hello list of labs, select hello desired lab.</span></span>  
4. <span data-ttu-id="e624c-165">V okně prostředí hello vyberte **vzorce (opakovaně použitelné základů)**.</span><span class="sxs-lookup"><span data-stu-id="e624c-165">On hello lab's blade, select **Formulas (reusable bases)**.</span></span>
   
    ![Vzorce nabídky](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. <span data-ttu-id="e624c-167">Na hello **testovacím vzorce** okně, vyberte hello vzorec chcete toomodify.</span><span class="sxs-lookup"><span data-stu-id="e624c-167">On hello **Lab formulas** blade, select hello formula you wish toomodify.</span></span>
6. <span data-ttu-id="e624c-168">Na hello **aktualizovat vzorec** okně hello potřeby úpravy a vyberte **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="e624c-168">On hello **Update formula** blade, make hello desired edits, and select **Update**.</span></span>

## <a name="delete-a-formula"></a><span data-ttu-id="e624c-169">Odstranit vzorec.</span><span class="sxs-lookup"><span data-stu-id="e624c-169">Delete a formula</span></span>
<span data-ttu-id="e624c-170">toodelete vzorec, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e624c-170">toodelete a formula, follow these steps:</span></span>

1. <span data-ttu-id="e624c-171">Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="e624c-171">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="e624c-172">Vyberte **více služeb**a potom vyberte **DevTest Labs** hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="e624c-172">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
3. <span data-ttu-id="e624c-173">Ze seznamu hello labs vyberte požadované prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="e624c-173">From hello list of labs, select hello desired lab.</span></span>  
4. <span data-ttu-id="e624c-174">V testovacím hello **nastavení** vyberte **vzorce**.</span><span class="sxs-lookup"><span data-stu-id="e624c-174">On hello lab **Settings** blade, select **Formulas**.</span></span>
   
    ![Vzorce nabídky](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. <span data-ttu-id="e624c-176">Na hello **testovacím vzorce** okně, vyberte hello toohello třemi tečkami napravo od hello vzorec chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="e624c-176">On hello **Lab formulas** blade, select hello ellipsis toohello right of hello formula you wish toodelete.</span></span>
   
    ![Vzorce nabídky](./media/devtest-lab-manage-formulas/lab-formulas-blade.png)
6. <span data-ttu-id="e624c-178">V místní nabídce hello vzorec, vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="e624c-178">On hello formula's context menu, select **Delete**.</span></span>
   
    ![Vzorce kontextové nabídky](./media/devtest-lab-manage-formulas/formula-delete-context-menu.png)
7. <span data-ttu-id="e624c-180">Vyberte **Ano** toohello dialogové okno potvrzení odstranění.</span><span class="sxs-lookup"><span data-stu-id="e624c-180">Select **Yes** toohello deletion confirmation dialog.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="e624c-181">Příspěvky blogu související</span><span class="sxs-lookup"><span data-stu-id="e624c-181">Related blog posts</span></span>
* [<span data-ttu-id="e624c-182">Vlastní Image nebo vzorce?</span><span class="sxs-lookup"><span data-stu-id="e624c-182">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a><span data-ttu-id="e624c-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e624c-183">Next steps</span></span>
<span data-ttu-id="e624c-184">Po vytvoření vzorec pro použití při vytváření virtuálního počítače, je dalším krokem hello příliš[přidání testovacího prostředí virtuálních počítačů tooyour](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="e624c-184">Once you have created a formula for use when creating a VM, hello next step is too[add a VM tooyour lab](devtest-lab-add-vm-with-artifacts.md).</span></span>

