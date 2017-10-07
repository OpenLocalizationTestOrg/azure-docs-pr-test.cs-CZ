---
title: "aaaAdd vlastníků a uživatele v Azure DevTest Labs | Microsoft Docs"
description: "Přidat vlastníků a uživatelé v Azure DevTest Labs pomocí hello portál Azure nebo prostředí PowerShell"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 4f51d9a5-2702-45f0-a2d5-a3635b58c416
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: tarcher
ms.openlocfilehash: 2a98f5fe1efbd7c23e0d97f58f47c37462aed3b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-owners-and-users-in-azure-devtest-labs"></a><span data-ttu-id="eb265-103">Přidat vlastníků a uživatelé v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="eb265-103">Add owners and users in Azure DevTest Labs</span></span>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/How-to-set-security-in-your-DevTest-Lab/player]
> 
> 

<span data-ttu-id="eb265-104">Přístup v Azure DevTest Labs řídí [řízení řízení přístupu (RBAC)](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="eb265-104">Access in Azure DevTest Labs is controlled by [Azure Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-what-is.md).</span></span> <span data-ttu-id="eb265-105">Pomocí RBAC, můžete oddělit povinností v rámci týmu do *role* kde byste udělit pouze hello množství přístup nezbytné toousers tooperform svou práci.</span><span class="sxs-lookup"><span data-stu-id="eb265-105">Using RBAC, you can segregate duties within your team into *roles* where you grant only hello amount of access necessary toousers tooperform their jobs.</span></span> <span data-ttu-id="eb265-106">Jsou tři tyto role RBAC *vlastníka*, *uživatel DevTest Labs*, a *Přispěvatel*.</span><span class="sxs-lookup"><span data-stu-id="eb265-106">Three of these RBAC roles are *Owner*, *DevTest Labs User*, and *Contributor*.</span></span> <span data-ttu-id="eb265-107">V tomto článku se dozvíte, jaké akce lze provést v každé role RBAC tři hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="eb265-107">In this article, you learn what actions can be performed in each of hello three main RBAC roles.</span></span> <span data-ttu-id="eb265-108">Odtud, zjistíte, jak se tooadd uživatelé tooa laboratoř - i prostřednictvím hello portálu a pomocí skriptu prostředí PowerShell a jak hello tooadd uživatele na úrovni předplatného.</span><span class="sxs-lookup"><span data-stu-id="eb265-108">From there, you learn how tooadd users tooa lab - both via hello portal and via a PowerShell script, and how tooadd users at hello subscription level.</span></span>

## <a name="actions-that-can-be-performed-in-each-role"></a><span data-ttu-id="eb265-109">Akce, které mohou být prováděny v každé role</span><span class="sxs-lookup"><span data-stu-id="eb265-109">Actions that can be performed in each role</span></span>
<span data-ttu-id="eb265-110">Existují tři hlavní role, můžete přiřadit uživatele:</span><span class="sxs-lookup"><span data-stu-id="eb265-110">There are three main roles that you can assign a user:</span></span>

* <span data-ttu-id="eb265-111">Vlastník</span><span class="sxs-lookup"><span data-stu-id="eb265-111">Owner</span></span>
* <span data-ttu-id="eb265-112">Uživatel DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="eb265-112">DevTest Labs User</span></span>
* <span data-ttu-id="eb265-113">Přispěvatel</span><span class="sxs-lookup"><span data-stu-id="eb265-113">Contributor</span></span>

<span data-ttu-id="eb265-114">Hello následující tabulka znázorňuje hello akce, které lze provést pomocí uživatelů v každé z těchto rolí:</span><span class="sxs-lookup"><span data-stu-id="eb265-114">hello following table illustrates hello actions that can be performed by users in each of these roles:</span></span>

| <span data-ttu-id="eb265-115">**Můžete provést akce, které uživatelé v této roli**</span><span class="sxs-lookup"><span data-stu-id="eb265-115">**Actions users in this role can perform**</span></span> | <span data-ttu-id="eb265-116">**Uživatel DevTest Labs**</span><span class="sxs-lookup"><span data-stu-id="eb265-116">**DevTest Labs User**</span></span> | <span data-ttu-id="eb265-117">**Vlastník**</span><span class="sxs-lookup"><span data-stu-id="eb265-117">**Owner**</span></span> | <span data-ttu-id="eb265-118">**Přispěvatel**</span><span class="sxs-lookup"><span data-stu-id="eb265-118">**Contributor**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="eb265-119">**Úlohy testovacího prostředí**</span><span class="sxs-lookup"><span data-stu-id="eb265-119">**Lab tasks**</span></span> | | | |
| <span data-ttu-id="eb265-120">Přidat tooa lab uživatelů</span><span class="sxs-lookup"><span data-stu-id="eb265-120">Add users tooa lab</span></span> |<span data-ttu-id="eb265-121">Ne</span><span class="sxs-lookup"><span data-stu-id="eb265-121">No</span></span> |<span data-ttu-id="eb265-122">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-122">Yes</span></span> |<span data-ttu-id="eb265-123">Ne</span><span class="sxs-lookup"><span data-stu-id="eb265-123">No</span></span> |
| <span data-ttu-id="eb265-124">Aktualizovat nastavení náklady</span><span class="sxs-lookup"><span data-stu-id="eb265-124">Update cost settings</span></span> |<span data-ttu-id="eb265-125">Ne</span><span class="sxs-lookup"><span data-stu-id="eb265-125">No</span></span> |<span data-ttu-id="eb265-126">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-126">Yes</span></span> |<span data-ttu-id="eb265-127">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-127">Yes</span></span> |
| <span data-ttu-id="eb265-128">**Základní úkoly virtuálních počítačů**</span><span class="sxs-lookup"><span data-stu-id="eb265-128">**VM base tasks**</span></span> | | | |
| <span data-ttu-id="eb265-129">Přidání a odebrání vlastních bitových kopií</span><span class="sxs-lookup"><span data-stu-id="eb265-129">Add and remove custom images</span></span> |<span data-ttu-id="eb265-130">Ne</span><span class="sxs-lookup"><span data-stu-id="eb265-130">No</span></span> |<span data-ttu-id="eb265-131">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-131">Yes</span></span> |<span data-ttu-id="eb265-132">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-132">Yes</span></span> |
| <span data-ttu-id="eb265-133">Přidat, aktualizovat a odstranit vzorce</span><span class="sxs-lookup"><span data-stu-id="eb265-133">Add, update, and delete formulas</span></span> |<span data-ttu-id="eb265-134">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-134">Yes</span></span> |<span data-ttu-id="eb265-135">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-135">Yes</span></span> |<span data-ttu-id="eb265-136">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-136">Yes</span></span> |
| <span data-ttu-id="eb265-137">Seznam povolených adres Azure Marketplace obrázků</span><span class="sxs-lookup"><span data-stu-id="eb265-137">Whitelist Azure Marketplace images</span></span> |<span data-ttu-id="eb265-138">Ne</span><span class="sxs-lookup"><span data-stu-id="eb265-138">No</span></span> |<span data-ttu-id="eb265-139">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-139">Yes</span></span> |<span data-ttu-id="eb265-140">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-140">Yes</span></span> |
| <span data-ttu-id="eb265-141">**Úkoly virtuálních počítačů**</span><span class="sxs-lookup"><span data-stu-id="eb265-141">**VM tasks**</span></span> | | | |
| <span data-ttu-id="eb265-142">Vytvoření virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="eb265-142">Create VMs</span></span> |<span data-ttu-id="eb265-143">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-143">Yes</span></span> |<span data-ttu-id="eb265-144">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-144">Yes</span></span> |<span data-ttu-id="eb265-145">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-145">Yes</span></span> |
| <span data-ttu-id="eb265-146">Spuštění, zastavení a odstranění virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="eb265-146">Start, stop, and delete VMs</span></span> |<span data-ttu-id="eb265-147">Pouze virtuální počítače vytvořené uživatelem hello</span><span class="sxs-lookup"><span data-stu-id="eb265-147">Only VMs created by hello user</span></span> |<span data-ttu-id="eb265-148">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-148">Yes</span></span> |<span data-ttu-id="eb265-149">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-149">Yes</span></span> |
| <span data-ttu-id="eb265-150">Aktualizovat zásady virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="eb265-150">Update VM policies</span></span> |<span data-ttu-id="eb265-151">Ne</span><span class="sxs-lookup"><span data-stu-id="eb265-151">No</span></span> |<span data-ttu-id="eb265-152">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-152">Yes</span></span> |<span data-ttu-id="eb265-153">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-153">Yes</span></span> |
| <span data-ttu-id="eb265-154">Přidání nebo odebrání datových disků z virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="eb265-154">Add/remove data disks to/from VMs</span></span> |<span data-ttu-id="eb265-155">Pouze virtuální počítače vytvořené uživatelem hello</span><span class="sxs-lookup"><span data-stu-id="eb265-155">Only VMs created by hello user</span></span> |<span data-ttu-id="eb265-156">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-156">Yes</span></span> |<span data-ttu-id="eb265-157">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-157">Yes</span></span> |
| <span data-ttu-id="eb265-158">**Artefaktů úlohy**</span><span class="sxs-lookup"><span data-stu-id="eb265-158">**Artifact tasks**</span></span> | | | |
| <span data-ttu-id="eb265-159">Přidání a odebrání úložiště artefaktů</span><span class="sxs-lookup"><span data-stu-id="eb265-159">Add and remove artifact repositories</span></span> |<span data-ttu-id="eb265-160">Ne</span><span class="sxs-lookup"><span data-stu-id="eb265-160">No</span></span> |<span data-ttu-id="eb265-161">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-161">Yes</span></span> |<span data-ttu-id="eb265-162">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-162">Yes</span></span> |
| <span data-ttu-id="eb265-163">Použít artefaktů</span><span class="sxs-lookup"><span data-stu-id="eb265-163">Apply artifacts</span></span> |<span data-ttu-id="eb265-164">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-164">Yes</span></span> |<span data-ttu-id="eb265-165">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-165">Yes</span></span> |<span data-ttu-id="eb265-166">Ano</span><span class="sxs-lookup"><span data-stu-id="eb265-166">Yes</span></span> |

> [!NOTE]
> <span data-ttu-id="eb265-167">Pokud uživatel vytvoří virtuální počítač, tento uživatel bude automaticky přiřazen toohello **vlastníka** role hello vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="eb265-167">When a user creates a VM, that user is automatically assigned toohello **Owner** role of hello created VM.</span></span>
> 
> 

## <a name="add-an-owner-or-user-at-hello-lab-level"></a><span data-ttu-id="eb265-168">Přidat vlastníkem nebo uživateli na úrovni hello testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="eb265-168">Add an owner or user at hello lab level</span></span>
<span data-ttu-id="eb265-169">Vlastníci a uživatelé mohou být přidány na úrovni testovacím hello prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="eb265-169">Owners and users can be added at hello lab level via hello Azure portal.</span></span> <span data-ttu-id="eb265-170">To zahrnuje externí uživatele s platnou [účet Microsoft (MSA)](devtest-lab-faq.md#what-is-a-microsoft-account).</span><span class="sxs-lookup"><span data-stu-id="eb265-170">This includes external users with a valid [Microsoft account (MSA)](devtest-lab-faq.md#what-is-a-microsoft-account).</span></span>
<span data-ttu-id="eb265-171">Hello následující kroky vás provedou hello proces přidávání tooa laboratoře protokolu vlastníka nebo uživatele v Azure DevTest Labs:</span><span class="sxs-lookup"><span data-stu-id="eb265-171">hello following steps guide you through hello process of adding an owner or user tooa lab in Azure DevTest Labs:</span></span>

1. <span data-ttu-id="eb265-172">Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="eb265-172">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="eb265-173">Vyberte **další služby**a potom vyberte **DevTest Labs** hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="eb265-173">Select **More services**, and then select **DevTest Labs** from hello list.</span></span>
3. <span data-ttu-id="eb265-174">Ze seznamu hello labs vyberte požadované prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="eb265-174">From hello list of labs, select hello desired lab.</span></span>
4. <span data-ttu-id="eb265-175">V okně prostředí hello vyberte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="eb265-175">On hello lab's blade, select **Configuration**.</span></span> 
5. <span data-ttu-id="eb265-176">Na hello **konfigurace** vyberte **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="eb265-176">On hello **Configuration** blade, select **Users**.</span></span>
6. <span data-ttu-id="eb265-177">Na hello **uživatelé** vyberte **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="eb265-177">On hello **Users** blade, select **+Add**.</span></span>
   
    ![Přidání uživatele](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
7. <span data-ttu-id="eb265-179">Na hello **vyberte roli** okně, vyberte hello požadované role.</span><span class="sxs-lookup"><span data-stu-id="eb265-179">On hello **Select a role** blade, select hello desired role.</span></span> <span data-ttu-id="eb265-180">Hello části [akce, které mohou být prováděny v každé role](#actions-that-can-be-performed-in-each-role) seznamy hello různé akce, které lze provést pomocí uživatelé v rolích hello vlastník, uživatel DevTest a Přispěvatel.</span><span class="sxs-lookup"><span data-stu-id="eb265-180">hello section [Actions that can be performed in each role](#actions-that-can-be-performed-in-each-role) lists hello various actions that can be performed by users in hello Owner, DevTest User, and Contributor roles.</span></span>
8. <span data-ttu-id="eb265-181">Na hello **přidat uživatele** okno, zadejte hello e-mailovou adresu nebo název hello uživatele chcete tooadd hello role, které jste zadali.</span><span class="sxs-lookup"><span data-stu-id="eb265-181">On hello **Add users** blade, enter hello email address or name of hello user you want tooadd in hello role you specified.</span></span> <span data-ttu-id="eb265-182">Pokud uživatel hello nelze nalézt, chybová zpráva vysvětluje hello problém.</span><span class="sxs-lookup"><span data-stu-id="eb265-182">If hello user can't be found, an error message explains hello issue.</span></span> <span data-ttu-id="eb265-183">Pokud je nalezen hello uživatele, tento uživatel je uvedena v seznamu a vybrané.</span><span class="sxs-lookup"><span data-stu-id="eb265-183">If hello user is found, that user is listed and selected.</span></span> 
9. <span data-ttu-id="eb265-184">Vyberte **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="eb265-184">Select **Select**.</span></span>
10. <span data-ttu-id="eb265-185">Vyberte **OK** tooclose hello **přidat přístup** okno.</span><span class="sxs-lookup"><span data-stu-id="eb265-185">Select **OK** tooclose hello **Add access** blade.</span></span>
11. <span data-ttu-id="eb265-186">Když se vrátíte toohello **uživatelé** okně hello uživatel přidaný.</span><span class="sxs-lookup"><span data-stu-id="eb265-186">When you return toohello **Users** blade, hello user has been added.</span></span>  

## <a name="add-an-external-user-tooa-lab-using-powershell"></a><span data-ttu-id="eb265-187">Přidat testovacím tooa externího uživatele pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="eb265-187">Add an external user tooa lab using PowerShell</span></span>
<span data-ttu-id="eb265-188">Kromě toho tooadding uživatele v hello portálu Azure, můžete přidat prostředí tooyour externího uživatele pomocí skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="eb265-188">In addition tooadding users in hello Azure portal, you can add an external user tooyour lab using a PowerShell script.</span></span> <span data-ttu-id="eb265-189">V hello následující ukázka, stačí upravit hodnoty parametrů hello pod hello **toochange hodnoty** komentář.</span><span class="sxs-lookup"><span data-stu-id="eb265-189">In hello following example, simply modify hello parameter values under hello **Values toochange** comment.</span></span>
<span data-ttu-id="eb265-190">Můžete načíst hello `subscriptionId`, `labResourceGroup`, a `labName` hodnoty v okně prostředí hello v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="eb265-190">You can retrieve hello `subscriptionId`, `labResourceGroup`, and `labName` values from hello lab blade in hello Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="eb265-191">Hello ukázkový skript předpokládá, že tento hello zadaný uživatel byl přidán jako hosta toohello služby Active Directory a se nezdaří, pokud to není hello případ.</span><span class="sxs-lookup"><span data-stu-id="eb265-191">hello sample script assumes that hello specified user has been added as a guest toohello Active Directory, and will fail if that is not hello case.</span></span> <span data-ttu-id="eb265-192">tooadd uživatel není v hello tooa testovací prostředí služby Active Directory, pomocí hello Azure portálu tooassign hello uživatele tooa role podle pokynů v části hello [přidat vlastníkem nebo uživateli na úrovni testovacím hello](#add-an-owner-or-user-at-the-lab-level).</span><span class="sxs-lookup"><span data-stu-id="eb265-192">tooadd a user not in hello Active Directory tooa lab, use hello Azure portal tooassign hello user tooa role as illustrated in hello section, [Add an owner or user at hello lab level](#add-an-owner-or-user-at-the-lab-level).</span></span>   
> 
> 

    # Add an external user in DevTest Labs user role tooa lab
    # Ensure that guest users can be added toohello Azure Active directory:
    # https://azure.microsoft.com/en-us/documentation/articles/active-directory-create-users/#set-guest-user-access-policies

    # Values toochange
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource name here>"
    $labName = "<Enter lab name here>"
    $userDisplayName = "<Enter user's display name here>"

    # Log into your Azure account
    Login-AzureRmAccount

    # Select hello Azure subscription that contains hello lab. 
    # This step is optional if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Retrieve hello user object
    $adObject = Get-AzureRmADUser -SearchString $userDisplayName

    # Create hello role assignment. 
    $labId = ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    New-AzureRmRoleAssignment -ObjectId $adObject.Id -RoleDefinitionName 'DevTest Labs User' -Scope $labId

## <a name="add-an-owner-or-user-at-hello-subscription-level"></a><span data-ttu-id="eb265-193">Přidat vlastníkem nebo uživateli na úrovni předplatného hello</span><span class="sxs-lookup"><span data-stu-id="eb265-193">Add an owner or user at hello subscription level</span></span>
<span data-ttu-id="eb265-194">Azure oprávnění rozšířeny z nadřazeného oboru toochild oboru v Azure.</span><span class="sxs-lookup"><span data-stu-id="eb265-194">Azure permissions are propagated from parent scope toochild scope in Azure.</span></span> <span data-ttu-id="eb265-195">Proto vlastníci předplatné Azure, který obsahuje labs jsou automaticky vlastníků těchto laboratoře.</span><span class="sxs-lookup"><span data-stu-id="eb265-195">Therefore, owners of an Azure subscription that contains labs are automatically owners of those labs.</span></span> <span data-ttu-id="eb265-196">Vlastní také hello virtuální počítače a další prostředky vytvořené hello lab uživatelů a hello Azure DevTest Labs služby.</span><span class="sxs-lookup"><span data-stu-id="eb265-196">They also own hello VMs and other resources created by hello lab's users, and hello Azure DevTest Labs service.</span></span> 

<span data-ttu-id="eb265-197">Můžete přidat další vlastníky tooa testovacím prostřednictvím hello testovacím okno v hello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="eb265-197">You can add additional owners tooa lab via hello lab's blade in hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span> <span data-ttu-id="eb265-198">Ale hello přidat obor vlastníka správy je užším než vlastník předplatného hello oboru.</span><span class="sxs-lookup"><span data-stu-id="eb265-198">However, hello added owner's scope of administration is more narrow than hello subscription owner's scope.</span></span> <span data-ttu-id="eb265-199">Například hello přidat vlastníky nemají úplný přístup toosome hello prostředků, které vytvářejí v rámci předplatného hello hello DevTest Labs služby.</span><span class="sxs-lookup"><span data-stu-id="eb265-199">For example, hello added owners do not have full access toosome of hello resources that are created in hello subscription by hello DevTest Labs service.</span></span> 

<span data-ttu-id="eb265-200">tooadd tooan vlastníka předplatného Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="eb265-200">tooadd an owner tooan Azure subscription, follow these steps:</span></span>

1. <span data-ttu-id="eb265-201">Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="eb265-201">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="eb265-202">Vyberte **více služeb**a potom vyberte **odběry** hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="eb265-202">Select **More Services**, and then select **Subscriptions** from hello list.</span></span>
3. <span data-ttu-id="eb265-203">Vyberte předplatné hello potřeby.</span><span class="sxs-lookup"><span data-stu-id="eb265-203">Select hello desired subscription.</span></span>
4. <span data-ttu-id="eb265-204">Vyberte **přístup** ikonu.</span><span class="sxs-lookup"><span data-stu-id="eb265-204">Select **Access** icon.</span></span> 
   
    ![Uživatelé přístup](./media/devtest-lab-add-devtest-user/access-users.png)
5. <span data-ttu-id="eb265-206">Na hello **uživatelé** vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="eb265-206">On hello **Users** blade, select **Add**.</span></span>
   
    ![Přidání uživatele](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
6. <span data-ttu-id="eb265-208">Na hello **vyberte roli** okně vyberte **vlastníka**.</span><span class="sxs-lookup"><span data-stu-id="eb265-208">On hello **Select a role** blade, select **Owner**.</span></span>
7. <span data-ttu-id="eb265-209">Na hello **přidat uživatele** okno, zadejte hello e-mailovou adresu nebo název hello uživatele chcete tooadd jako vlastníka.</span><span class="sxs-lookup"><span data-stu-id="eb265-209">On hello **Add users** blade, enter hello email address or name of hello user you want tooadd as an owner.</span></span> <span data-ttu-id="eb265-210">Pokud hello uživatele nelze nalézt, zobrazí chybovou zprávu vysvětlením hello problém.</span><span class="sxs-lookup"><span data-stu-id="eb265-210">If hello user can't be found, you get an error message explaining hello issue.</span></span> <span data-ttu-id="eb265-211">Pokud uživatel hello je nalezen, je tento uživatel uveden v části hello **uživatele** textové pole.</span><span class="sxs-lookup"><span data-stu-id="eb265-211">If hello user is found, that user is listed under hello **User** text box.</span></span>
8. <span data-ttu-id="eb265-212">Vyberte hello nachází uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="eb265-212">Select hello located user name.</span></span>
9. <span data-ttu-id="eb265-213">Vyberte **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="eb265-213">Select **Select**.</span></span>
10. <span data-ttu-id="eb265-214">Vyberte **OK** tooclose hello **přidat přístup** okno.</span><span class="sxs-lookup"><span data-stu-id="eb265-214">Select **OK** tooclose hello **Add access** blade.</span></span>
11. <span data-ttu-id="eb265-215">Když se vrátíte toohello **uživatelé** okně hello uživatel přidaný jako vlastníka.</span><span class="sxs-lookup"><span data-stu-id="eb265-215">When you return toohello **Users** blade, hello user has been added as an owner.</span></span> <span data-ttu-id="eb265-216">Tento uživatel je teď vlastníkem žádné labs vytvořil v rámci tohoto předplatného a proto být schopný tooperform vlastník úlohy.</span><span class="sxs-lookup"><span data-stu-id="eb265-216">This user is now an owner of any labs created under this subscription, and thus be able tooperform owner tasks.</span></span> 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

