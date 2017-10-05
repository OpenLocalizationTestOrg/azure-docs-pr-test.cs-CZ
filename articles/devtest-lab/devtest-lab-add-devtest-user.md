---
title: "Přidat vlastníků a uživatelé v Azure DevTest Labs | Microsoft Docs"
description: "Přidat vlastníků a uživatelé v Azure DevTest Labs buď pomocí portálu Azure nebo pomocí prostředí PowerShell"
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
ms.openlocfilehash: d67fa257574d6cb4ad4b18521900374fb51da290
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-owners-and-users-in-azure-devtest-labs"></a><span data-ttu-id="b990f-103">Přidat vlastníků a uživatelé v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="b990f-103">Add owners and users in Azure DevTest Labs</span></span>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/How-to-set-security-in-your-DevTest-Lab/player]
> 
> 

<span data-ttu-id="b990f-104">Přístup v Azure DevTest Labs řídí [řízení řízení přístupu (RBAC)](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="b990f-104">Access in Azure DevTest Labs is controlled by [Azure Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-what-is.md).</span></span> <span data-ttu-id="b990f-105">Pomocí RBAC, můžete oddělit povinností v rámci týmu do *role* kde můžete poskytnout pouze takovou úroveň přístupu, které jsou nezbytné pro uživatele k provádění svých úloh.</span><span class="sxs-lookup"><span data-stu-id="b990f-105">Using RBAC, you can segregate duties within your team into *roles* where you grant only the amount of access necessary to users to perform their jobs.</span></span> <span data-ttu-id="b990f-106">Jsou tři tyto role RBAC *vlastníka*, *uživatel DevTest Labs*, a *Přispěvatel*.</span><span class="sxs-lookup"><span data-stu-id="b990f-106">Three of these RBAC roles are *Owner*, *DevTest Labs User*, and *Contributor*.</span></span> <span data-ttu-id="b990f-107">V tomto článku se dozvíte, jaké akce lze provést v každé tři hlavní role RBAC.</span><span class="sxs-lookup"><span data-stu-id="b990f-107">In this article, you learn what actions can be performed in each of the three main RBAC roles.</span></span> <span data-ttu-id="b990f-108">Odtud zjistíte, jak přidat uživatele do testovacího prostředí - prostřednictvím portálu i pomocí skriptu prostředí PowerShell a jak přidat uživatele na úrovni předplatného.</span><span class="sxs-lookup"><span data-stu-id="b990f-108">From there, you learn how to add users to a lab - both via the portal and via a PowerShell script, and how to add users at the subscription level.</span></span>

## <a name="actions-that-can-be-performed-in-each-role"></a><span data-ttu-id="b990f-109">Akce, které mohou být prováděny v každé role</span><span class="sxs-lookup"><span data-stu-id="b990f-109">Actions that can be performed in each role</span></span>
<span data-ttu-id="b990f-110">Existují tři hlavní role, můžete přiřadit uživatele:</span><span class="sxs-lookup"><span data-stu-id="b990f-110">There are three main roles that you can assign a user:</span></span>

* <span data-ttu-id="b990f-111">Vlastník</span><span class="sxs-lookup"><span data-stu-id="b990f-111">Owner</span></span>
* <span data-ttu-id="b990f-112">Uživatel DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="b990f-112">DevTest Labs User</span></span>
* <span data-ttu-id="b990f-113">Přispěvatel</span><span class="sxs-lookup"><span data-stu-id="b990f-113">Contributor</span></span>

<span data-ttu-id="b990f-114">Následující tabulka uvádí akce, které lze provést pomocí uživatelů v každé z těchto rolí:</span><span class="sxs-lookup"><span data-stu-id="b990f-114">The following table illustrates the actions that can be performed by users in each of these roles:</span></span>

| <span data-ttu-id="b990f-115">**Můžete provést akce, které uživatelé v této roli**</span><span class="sxs-lookup"><span data-stu-id="b990f-115">**Actions users in this role can perform**</span></span> | <span data-ttu-id="b990f-116">**Uživatel DevTest Labs**</span><span class="sxs-lookup"><span data-stu-id="b990f-116">**DevTest Labs User**</span></span> | <span data-ttu-id="b990f-117">**Vlastník**</span><span class="sxs-lookup"><span data-stu-id="b990f-117">**Owner**</span></span> | <span data-ttu-id="b990f-118">**Přispěvatel**</span><span class="sxs-lookup"><span data-stu-id="b990f-118">**Contributor**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b990f-119">**Úlohy testovacího prostředí**</span><span class="sxs-lookup"><span data-stu-id="b990f-119">**Lab tasks**</span></span> | | | |
| <span data-ttu-id="b990f-120">Přidání uživatelů do testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="b990f-120">Add users to a lab</span></span> |<span data-ttu-id="b990f-121">Ne</span><span class="sxs-lookup"><span data-stu-id="b990f-121">No</span></span> |<span data-ttu-id="b990f-122">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-122">Yes</span></span> |<span data-ttu-id="b990f-123">Ne</span><span class="sxs-lookup"><span data-stu-id="b990f-123">No</span></span> |
| <span data-ttu-id="b990f-124">Aktualizovat nastavení náklady</span><span class="sxs-lookup"><span data-stu-id="b990f-124">Update cost settings</span></span> |<span data-ttu-id="b990f-125">Ne</span><span class="sxs-lookup"><span data-stu-id="b990f-125">No</span></span> |<span data-ttu-id="b990f-126">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-126">Yes</span></span> |<span data-ttu-id="b990f-127">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-127">Yes</span></span> |
| <span data-ttu-id="b990f-128">**Základní úkoly virtuálních počítačů**</span><span class="sxs-lookup"><span data-stu-id="b990f-128">**VM base tasks**</span></span> | | | |
| <span data-ttu-id="b990f-129">Přidání a odebrání vlastních bitových kopií</span><span class="sxs-lookup"><span data-stu-id="b990f-129">Add and remove custom images</span></span> |<span data-ttu-id="b990f-130">Ne</span><span class="sxs-lookup"><span data-stu-id="b990f-130">No</span></span> |<span data-ttu-id="b990f-131">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-131">Yes</span></span> |<span data-ttu-id="b990f-132">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-132">Yes</span></span> |
| <span data-ttu-id="b990f-133">Přidat, aktualizovat a odstranit vzorce</span><span class="sxs-lookup"><span data-stu-id="b990f-133">Add, update, and delete formulas</span></span> |<span data-ttu-id="b990f-134">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-134">Yes</span></span> |<span data-ttu-id="b990f-135">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-135">Yes</span></span> |<span data-ttu-id="b990f-136">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-136">Yes</span></span> |
| <span data-ttu-id="b990f-137">Seznam povolených adres Azure Marketplace obrázků</span><span class="sxs-lookup"><span data-stu-id="b990f-137">Whitelist Azure Marketplace images</span></span> |<span data-ttu-id="b990f-138">Ne</span><span class="sxs-lookup"><span data-stu-id="b990f-138">No</span></span> |<span data-ttu-id="b990f-139">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-139">Yes</span></span> |<span data-ttu-id="b990f-140">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-140">Yes</span></span> |
| <span data-ttu-id="b990f-141">**Úkoly virtuálních počítačů**</span><span class="sxs-lookup"><span data-stu-id="b990f-141">**VM tasks**</span></span> | | | |
| <span data-ttu-id="b990f-142">Vytvoření virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="b990f-142">Create VMs</span></span> |<span data-ttu-id="b990f-143">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-143">Yes</span></span> |<span data-ttu-id="b990f-144">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-144">Yes</span></span> |<span data-ttu-id="b990f-145">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-145">Yes</span></span> |
| <span data-ttu-id="b990f-146">Spuštění, zastavení a odstranění virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="b990f-146">Start, stop, and delete VMs</span></span> |<span data-ttu-id="b990f-147">Vytvořený uživatelem jenom virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="b990f-147">Only VMs created by the user</span></span> |<span data-ttu-id="b990f-148">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-148">Yes</span></span> |<span data-ttu-id="b990f-149">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-149">Yes</span></span> |
| <span data-ttu-id="b990f-150">Aktualizovat zásady virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="b990f-150">Update VM policies</span></span> |<span data-ttu-id="b990f-151">Ne</span><span class="sxs-lookup"><span data-stu-id="b990f-151">No</span></span> |<span data-ttu-id="b990f-152">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-152">Yes</span></span> |<span data-ttu-id="b990f-153">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-153">Yes</span></span> |
| <span data-ttu-id="b990f-154">Přidání nebo odebrání datových disků z virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="b990f-154">Add/remove data disks to/from VMs</span></span> |<span data-ttu-id="b990f-155">Vytvořený uživatelem jenom virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="b990f-155">Only VMs created by the user</span></span> |<span data-ttu-id="b990f-156">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-156">Yes</span></span> |<span data-ttu-id="b990f-157">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-157">Yes</span></span> |
| <span data-ttu-id="b990f-158">**Artefaktů úlohy**</span><span class="sxs-lookup"><span data-stu-id="b990f-158">**Artifact tasks**</span></span> | | | |
| <span data-ttu-id="b990f-159">Přidání a odebrání úložiště artefaktů</span><span class="sxs-lookup"><span data-stu-id="b990f-159">Add and remove artifact repositories</span></span> |<span data-ttu-id="b990f-160">Ne</span><span class="sxs-lookup"><span data-stu-id="b990f-160">No</span></span> |<span data-ttu-id="b990f-161">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-161">Yes</span></span> |<span data-ttu-id="b990f-162">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-162">Yes</span></span> |
| <span data-ttu-id="b990f-163">Použít artefaktů</span><span class="sxs-lookup"><span data-stu-id="b990f-163">Apply artifacts</span></span> |<span data-ttu-id="b990f-164">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-164">Yes</span></span> |<span data-ttu-id="b990f-165">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-165">Yes</span></span> |<span data-ttu-id="b990f-166">Ano</span><span class="sxs-lookup"><span data-stu-id="b990f-166">Yes</span></span> |

> [!NOTE]
> <span data-ttu-id="b990f-167">Když uživatel vytvoří virtuální počítač, se automaticky přiřadí tohoto uživatele k **vlastníka** role vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b990f-167">When a user creates a VM, that user is automatically assigned to the **Owner** role of the created VM.</span></span>
> 
> 

## <a name="add-an-owner-or-user-at-the-lab-level"></a><span data-ttu-id="b990f-168">Přidat vlastníkem nebo uživateli na úrovni testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="b990f-168">Add an owner or user at the lab level</span></span>
<span data-ttu-id="b990f-169">Vlastníci a uživatelé mohou být přidány na úrovni testovacího prostředí prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b990f-169">Owners and users can be added at the lab level via the Azure portal.</span></span> <span data-ttu-id="b990f-170">To zahrnuje externí uživatele s platnou [účet Microsoft (MSA)](devtest-lab-faq.md#what-is-a-microsoft-account).</span><span class="sxs-lookup"><span data-stu-id="b990f-170">This includes external users with a valid [Microsoft account (MSA)](devtest-lab-faq.md#what-is-a-microsoft-account).</span></span>
<span data-ttu-id="b990f-171">Proces přidávání roli vlastníka nebo uživatele do testovacího prostředí v Azure DevTest Labs vás provede následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b990f-171">The following steps guide you through the process of adding an owner or user to a lab in Azure DevTest Labs:</span></span>

1. <span data-ttu-id="b990f-172">Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="b990f-172">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="b990f-173">Vyberte **Další služby** a poté ze seznamu vyberte **DevTest Labs**.</span><span class="sxs-lookup"><span data-stu-id="b990f-173">Select **More services**, and then select **DevTest Labs** from the list.</span></span>
3. <span data-ttu-id="b990f-174">Ze seznamu labs vyberte požadované testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="b990f-174">From the list of labs, select the desired lab.</span></span>
4. <span data-ttu-id="b990f-175">V okně v prostředí, vyberte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="b990f-175">On the lab's blade, select **Configuration**.</span></span> 
5. <span data-ttu-id="b990f-176">Na **konfigurace** vyberte **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b990f-176">On the **Configuration** blade, select **Users**.</span></span>
6. <span data-ttu-id="b990f-177">Na **uživatelé** vyberte **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="b990f-177">On the **Users** blade, select **+Add**.</span></span>
   
    ![Přidání uživatele](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
7. <span data-ttu-id="b990f-179">Na **vyberte roli** okno, vyberte požadovanou roli.</span><span class="sxs-lookup"><span data-stu-id="b990f-179">On the **Select a role** blade, select the desired role.</span></span> <span data-ttu-id="b990f-180">V části [akce, které mohou být prováděny v každé role](#actions-that-can-be-performed-in-each-role) uvádí různé akce, které lze provést pomocí uživatelé v rolích vlastník, uživatel DevTest a Přispěvatel.</span><span class="sxs-lookup"><span data-stu-id="b990f-180">The section [Actions that can be performed in each role](#actions-that-can-be-performed-in-each-role) lists the various actions that can be performed by users in the Owner, DevTest User, and Contributor roles.</span></span>
8. <span data-ttu-id="b990f-181">Na **přidat uživatele** okno, zadejte e-mailovou adresu nebo jméno uživatele, který chcete přidat v roli, která jste zadali.</span><span class="sxs-lookup"><span data-stu-id="b990f-181">On the **Add users** blade, enter the email address or name of the user you want to add in the role you specified.</span></span> <span data-ttu-id="b990f-182">Pokud uživatel nebyl nalezen, chybová zpráva popisuje problém.</span><span class="sxs-lookup"><span data-stu-id="b990f-182">If the user can't be found, an error message explains the issue.</span></span> <span data-ttu-id="b990f-183">Pokud je uživatel nalezen, je uvedena v seznamu a vybrané tohoto uživatele.</span><span class="sxs-lookup"><span data-stu-id="b990f-183">If the user is found, that user is listed and selected.</span></span> 
9. <span data-ttu-id="b990f-184">Vyberte **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="b990f-184">Select **Select**.</span></span>
10. <span data-ttu-id="b990f-185">Vyberte **OK** zavřete **přidat přístup** okno.</span><span class="sxs-lookup"><span data-stu-id="b990f-185">Select **OK** to close the **Add access** blade.</span></span>
11. <span data-ttu-id="b990f-186">Když se vrátíte **uživatelé** okně přidal uživatele.</span><span class="sxs-lookup"><span data-stu-id="b990f-186">When you return to the **Users** blade, the user has been added.</span></span>  

## <a name="add-an-external-user-to-a-lab-using-powershell"></a><span data-ttu-id="b990f-187">Přidat externího uživatele k testovacím prostředí pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="b990f-187">Add an external user to a lab using PowerShell</span></span>
<span data-ttu-id="b990f-188">Kromě přidání uživatelů na portálu Azure, můžete přidat externího uživatele na vašem testovacím prostředí pomocí skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b990f-188">In addition to adding users in the Azure portal, you can add an external user to your lab using a PowerShell script.</span></span> <span data-ttu-id="b990f-189">V následujícím příkladu, stačí upravit hodnoty parametru v části **hodnoty změnit** komentář.</span><span class="sxs-lookup"><span data-stu-id="b990f-189">In the following example, simply modify the parameter values under the **Values to change** comment.</span></span>
<span data-ttu-id="b990f-190">Můžete získat `subscriptionId`, `labResourceGroup`, a `labName` hodnoty v okně prostředí na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b990f-190">You can retrieve the `subscriptionId`, `labResourceGroup`, and `labName` values from the lab blade in the Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="b990f-191">Ukázkový skript předpokládá, že zadaný uživatel byl přidán jako Host ke službě Active Directory a se nezdaří, pokud se nejedná o tento případ.</span><span class="sxs-lookup"><span data-stu-id="b990f-191">The sample script assumes that the specified user has been added as a guest to the Active Directory, and will fail if that is not the case.</span></span> <span data-ttu-id="b990f-192">K přidání uživatele není ve službě Active Directory do testovacího prostředí, použijte portál Azure přiřadit uživatele k roli podle pokynů v části [přidat vlastníkem nebo uživateli na úrovni testovacího prostředí](#add-an-owner-or-user-at-the-lab-level).</span><span class="sxs-lookup"><span data-stu-id="b990f-192">To add a user not in the Active Directory to a lab, use the Azure portal to assign the user to a role as illustrated in the section, [Add an owner or user at the lab level](#add-an-owner-or-user-at-the-lab-level).</span></span>   
> 
> 

    # Add an external user in DevTest Labs user role to a lab
    # Ensure that guest users can be added to the Azure Active directory:
    # https://azure.microsoft.com/en-us/documentation/articles/active-directory-create-users/#set-guest-user-access-policies

    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource name here>"
    $labName = "<Enter lab name here>"
    $userDisplayName = "<Enter user's display name here>"

    # Log into your Azure account
    Login-AzureRmAccount

    # Select the Azure subscription that contains the lab. 
    # This step is optional if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Retrieve the user object
    $adObject = Get-AzureRmADUser -SearchString $userDisplayName

    # Create the role assignment. 
    $labId = ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    New-AzureRmRoleAssignment -ObjectId $adObject.Id -RoleDefinitionName 'DevTest Labs User' -Scope $labId

## <a name="add-an-owner-or-user-at-the-subscription-level"></a><span data-ttu-id="b990f-193">Přidání vlastníkem nebo uživateli na úrovni předplatného</span><span class="sxs-lookup"><span data-stu-id="b990f-193">Add an owner or user at the subscription level</span></span>
<span data-ttu-id="b990f-194">Azure oprávnění rozšířeny z nadřazeného oboru podřízeném oboru v Azure.</span><span class="sxs-lookup"><span data-stu-id="b990f-194">Azure permissions are propagated from parent scope to child scope in Azure.</span></span> <span data-ttu-id="b990f-195">Proto vlastníci předplatné Azure, který obsahuje labs jsou automaticky vlastníků těchto laboratoře.</span><span class="sxs-lookup"><span data-stu-id="b990f-195">Therefore, owners of an Azure subscription that contains labs are automatically owners of those labs.</span></span> <span data-ttu-id="b990f-196">Také vlastní virtuální počítače a další prostředky vytvořené v prostředí uživatelů a službu Azure DevTest Labs.</span><span class="sxs-lookup"><span data-stu-id="b990f-196">They also own the VMs and other resources created by the lab's users, and the Azure DevTest Labs service.</span></span> 

<span data-ttu-id="b990f-197">Můžete přidat další vlastníky prostřednictvím okna v prostředí v testovacím prostředí [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="b990f-197">You can add additional owners to a lab via the lab's blade in the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span> <span data-ttu-id="b990f-198">Vlastník přidaný oboru správy je však užším než oboru vlastník předplatného.</span><span class="sxs-lookup"><span data-stu-id="b990f-198">However, the added owner's scope of administration is more narrow than the subscription owner's scope.</span></span> <span data-ttu-id="b990f-199">Například přidané vlastníky nemají úplný přístup k některým z prostředků, které jsou vytvořené v rámci předplatného službou DevTest Labs.</span><span class="sxs-lookup"><span data-stu-id="b990f-199">For example, the added owners do not have full access to some of the resources that are created in the subscription by the DevTest Labs service.</span></span> 

<span data-ttu-id="b990f-200">Pokud chcete přidat vlastníka k předplatnému Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="b990f-200">To add an owner to an Azure subscription, follow these steps:</span></span>

1. <span data-ttu-id="b990f-201">Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="b990f-201">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="b990f-202">Vyberte **více služeb**a potom vyberte **odběry** ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="b990f-202">Select **More Services**, and then select **Subscriptions** from the list.</span></span>
3. <span data-ttu-id="b990f-203">Vyberte požadované předplatné.</span><span class="sxs-lookup"><span data-stu-id="b990f-203">Select the desired subscription.</span></span>
4. <span data-ttu-id="b990f-204">Vyberte **přístup** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b990f-204">Select **Access** icon.</span></span> 
   
    ![Uživatelé přístup](./media/devtest-lab-add-devtest-user/access-users.png)
5. <span data-ttu-id="b990f-206">Na **uživatelé** vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="b990f-206">On the **Users** blade, select **Add**.</span></span>
   
    ![Přidání uživatele](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
6. <span data-ttu-id="b990f-208">Na **vyberte roli** okně vyberte **vlastníka**.</span><span class="sxs-lookup"><span data-stu-id="b990f-208">On the **Select a role** blade, select **Owner**.</span></span>
7. <span data-ttu-id="b990f-209">Na **přidat uživatele** okno, zadejte e-mailovou adresu nebo jméno uživatele, který chcete přidat jako vlastníka.</span><span class="sxs-lookup"><span data-stu-id="b990f-209">On the **Add users** blade, enter the email address or name of the user you want to add as an owner.</span></span> <span data-ttu-id="b990f-210">Pokud uživatel nebyl nalezen, zobrazí chybovou zprávu vysvětlením problém.</span><span class="sxs-lookup"><span data-stu-id="b990f-210">If the user can't be found, you get an error message explaining the issue.</span></span> <span data-ttu-id="b990f-211">Pokud je uživatel nalezen, je tento uživatel uveden v části **uživatele** textové pole.</span><span class="sxs-lookup"><span data-stu-id="b990f-211">If the user is found, that user is listed under the **User** text box.</span></span>
8. <span data-ttu-id="b990f-212">Vyberte umístění uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="b990f-212">Select the located user name.</span></span>
9. <span data-ttu-id="b990f-213">Vyberte **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="b990f-213">Select **Select**.</span></span>
10. <span data-ttu-id="b990f-214">Vyberte **OK** zavřete **přidat přístup** okno.</span><span class="sxs-lookup"><span data-stu-id="b990f-214">Select **OK** to close the **Add access** blade.</span></span>
11. <span data-ttu-id="b990f-215">Když se vrátíte **uživatelé** okně Uživatel byl přidán jako vlastníka.</span><span class="sxs-lookup"><span data-stu-id="b990f-215">When you return to the **Users** blade, the user has been added as an owner.</span></span> <span data-ttu-id="b990f-216">Tento uživatel je teď vlastníkem žádné labs vytvořil v rámci tohoto předplatného a tak bude moci provádět úlohy vlastníka.</span><span class="sxs-lookup"><span data-stu-id="b990f-216">This user is now an owner of any labs created under this subscription, and thus be able to perform owner tasks.</span></span> 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

