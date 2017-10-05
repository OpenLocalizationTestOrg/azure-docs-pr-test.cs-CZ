---
title: "Obnovit odstraněné skupiny Office 365 ve službě Azure Active Directory | Microsoft Docs"
description: "Jak obnovit odstraněné skupině, zobrazit obnovitelné skupiny a permamnently odstranit skupinu v Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cc5f232a-1e77-45c2-b28b-1fcb4621725c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: curtand
ms.openlocfilehash: 32d36ae96797a59bb47bf782ab038f21f231f046
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="restore-a-deleted-office-365-group-in-azure-active-directory"></a><span data-ttu-id="d0977-103">Obnovit odstraněné skupiny Office 365 ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d0977-103">Restore a deleted Office 365 group in Azure Active Directory</span></span>

<span data-ttu-id="d0977-104">Pokud odstraníte skupinu služeb Office 365 ve službě Azure Active Directory (Azure AD), z odstraněné skupiny je zachované, ale nejsou viditelné po dobu 30 dnů od data odstranění.</span><span class="sxs-lookup"><span data-stu-id="d0977-104">When you delete an Office 365 group in the Azure Active Directory (Azure AD), the deleted group is retained but not visible for 30 days from the deletion date.</span></span> <span data-ttu-id="d0977-105">Toto je tak, aby skupiny a její obsah se dají obnovovat v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="d0977-105">This is so that the group and its contents can be restored if needed.</span></span> <span data-ttu-id="d0977-106">Tato funkce je omezen výhradně na skupiny Office 365 ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d0977-106">This functionality is restricted exclusively to Office 365 groups in Azure AD.</span></span> <span data-ttu-id="d0977-107">Není k dispozici pro skupiny zabezpečení a distribučních skupin.</span><span class="sxs-lookup"><span data-stu-id="d0977-107">It is not available for security groups and distribution groups.</span></span>

> [!NOTE] 
> <span data-ttu-id="d0977-108">Nepoužívejte `Remove-MsolGroup` protože skupině trvale odstraní.</span><span class="sxs-lookup"><span data-stu-id="d0977-108">Don't use `Remove-MsolGroup` because it purges the group permanently.</span></span> <span data-ttu-id="d0977-109">Vždy používat `Remove-AzureADMSGroup` odstranění skupiny O365.</span><span class="sxs-lookup"><span data-stu-id="d0977-109">Always use `Remove-AzureADMSGroup` to delete an O365 group.</span></span> 

<span data-ttu-id="d0977-110">Oprávnění potřebná k obnovení skupiny může být jedno z následujících:</span><span class="sxs-lookup"><span data-stu-id="d0977-110">The permissions required to restore a group can be any of the following:</span></span>

<span data-ttu-id="d0977-111">Role</span><span class="sxs-lookup"><span data-stu-id="d0977-111">Role</span></span>  | <span data-ttu-id="d0977-112">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="d0977-112">Permissions</span></span> 
--------- | ---------
<span data-ttu-id="d0977-113">Správce společnosti, partnera Tier2 podpory a správci služby InTune</span><span class="sxs-lookup"><span data-stu-id="d0977-113">Company Administrator, Partner Tier2 support, and InTune Service Admins</span></span> | <span data-ttu-id="d0977-114">Můžete obnovit všechny odstraněné skupiny Office 365</span><span class="sxs-lookup"><span data-stu-id="d0977-114">Can restore any deleted Office 365 group</span></span> 
<span data-ttu-id="d0977-115">Podpora uživatelského účtu správce a Tier1 partnera</span><span class="sxs-lookup"><span data-stu-id="d0977-115">User Account Administrator and Partner Tier1 support</span></span> | <span data-ttu-id="d0977-116">Můžete obnovit všechny odstraněné skupiny Office 365 s výjimkou jsou přiřazena k roli správce společnosti</span><span class="sxs-lookup"><span data-stu-id="d0977-116">Can restore any deleted Office 365 group except those assigned to the Company Administrator role</span></span> 
<span data-ttu-id="d0977-117">Uživatel</span><span class="sxs-lookup"><span data-stu-id="d0977-117">User</span></span> | <span data-ttu-id="d0977-118">Můžete obnovit všechny odstraněné skupiny Office 365, který se ve vlastnictví</span><span class="sxs-lookup"><span data-stu-id="d0977-118">Can restore any deleted Office 365 group that they owned</span></span> 


## <a name="view-the-deleted-office-365-groups-that-are-available-to-restore"></a><span data-ttu-id="d0977-119">Zobrazit odstraněné skupiny Office 365, které jsou k dispozici pro obnovení</span><span class="sxs-lookup"><span data-stu-id="d0977-119">View the deleted Office 365 groups that are available to restore</span></span>
<span data-ttu-id="d0977-120">Chcete-li zobrazit tyto odstraněné skupiny ověřte, že jeden nebo ty, které vás zajímají nebyly dosud vyprázdnila trvale slouží následující rutiny.</span><span class="sxs-lookup"><span data-stu-id="d0977-120">The following cmdlets can be used to view the deleted groups to verify that the one or ones you're interested in have not yet been permanently purged.</span></span> <span data-ttu-id="d0977-121">Tyto rutiny jsou součástí [modulu Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureAD/).</span><span class="sxs-lookup"><span data-stu-id="d0977-121">These cmdlets are part of the [Azure AD PowerShell module](https://www.powershellgallery.com/packages/AzureAD/).</span></span> <span data-ttu-id="d0977-122">Další informace o tomto modulu najdete v [Azure Active Directory PowerShell verze 2](/powershell/azure/install-adv2?view=azureadps-2.0) článku.</span><span class="sxs-lookup"><span data-stu-id="d0977-122">More information about this module can be found in the [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0) article.</span></span>

1.  <span data-ttu-id="d0977-123">Spusťte následující rutinu pro zobrazení všech odstranit skupiny Office 365 ve vašem klientovi, které jsou stále k dispozici pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="d0977-123">Run the following cmdlet to display all deleted Office 365 groups in your tenant that are still available to restore.</span></span>
  ```
  Get-AzureADMSDeletedGroup
  ```

2.  <span data-ttu-id="d0977-124">Případně pokud víte objectID určité skupiny (a můžete ho získat prostřednictvím rutinu v kroku 1), spusťte následující rutinu k ověření, že konkrétní odstraněné skupiny nebyl ještě vyprázdnila trvale.</span><span class="sxs-lookup"><span data-stu-id="d0977-124">Alternately, if you know the objectID of a specific group (and you can get it from the cmdlet in step 1), run the following cmdlet to verify that the specific deleted group has not yet been permanently purged.</span></span>
  ```
  Get-AzureADMSDeletedGroup –Id <objectId>
  ```



## <a name="how-to-restore-your-deleted-office-365-group"></a><span data-ttu-id="d0977-125">Jak obnovit odstraněné skupině Office 365</span><span class="sxs-lookup"><span data-stu-id="d0977-125">How to restore your deleted Office 365 group</span></span>
<span data-ttu-id="d0977-126">Jakmile si ověříte, zda je skupina stále k dispozici pro obnovení, obnovení odstraněné skupiny s jedním z následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="d0977-126">Once you have verified that the group is still available to restore, restore the deleted group with one of the following steps.</span></span> <span data-ttu-id="d0977-127">Pokud tato skupina obsahuje dokumenty, SP lokalit nebo jiné trvalé objekty, může trvat až 24 hodin plně obnovit skupinu a její obsah.</span><span class="sxs-lookup"><span data-stu-id="d0977-127">If the group contains documents, SP sites, or other persistent objects, it might take up to 24 hours to fully restore a group and its contents.</span></span>

1.  <span data-ttu-id="d0977-128">Spusťte následující rutinu k obnovení skupiny a její obsah.</span><span class="sxs-lookup"><span data-stu-id="d0977-128">Run the following cmdlet to restore the group and its contents.</span></span>
  
  ```
  Restore-AzureADMSDeletedDirectoryObject –Id <objectId>
  ``` 

<span data-ttu-id="d0977-129">Následující rutiny můžete spustit taky, trvale odeberete z odstraněné skupiny.</span><span class="sxs-lookup"><span data-stu-id="d0977-129">Alternatively, the following cmdlet can be run to permanently remove the deleted group.</span></span>
  ```
  Remove-AzureADMSDeletedDirectoryObject –Id <objectId>
  ```

## <a name="how-do-you-know-this-worked"></a><span data-ttu-id="d0977-130">Jak to šlo znáte?</span><span class="sxs-lookup"><span data-stu-id="d0977-130">How do you know this worked?</span></span>
<span data-ttu-id="d0977-131">Chcete-li ověřit, že jste úspěšně obnovit skupinu služeb Office 365, spusťte `Get-AzureADGroup –ObjectId <objectId>` rutiny zobrazíte informace o skupině.</span><span class="sxs-lookup"><span data-stu-id="d0977-131">To verify that you’ve successfully restored an Office 365 group, run the `Get-AzureADGroup –ObjectId <objectId>` cmdlet to display information about the group.</span></span> <span data-ttu-id="d0977-132">Po obnovení žádosti o dokončení:</span><span class="sxs-lookup"><span data-stu-id="d0977-132">After the restore request is completed:</span></span>
- <span data-ttu-id="d0977-133">Skupiny se zobrazí v levém navigačního panelu na Exchange</span><span class="sxs-lookup"><span data-stu-id="d0977-133">The group will appear in the Left nav bar on Exchange</span></span>
- <span data-ttu-id="d0977-134">Plán pro skupinu se objeví v plánovače</span><span class="sxs-lookup"><span data-stu-id="d0977-134">The plan for the group will appear in Planner</span></span>
- <span data-ttu-id="d0977-135">Všechny weby služby Sharepoint a veškerý jeho obsah bude k dispozici</span><span class="sxs-lookup"><span data-stu-id="d0977-135">Any Sharepoint sites and all of their contents will be available</span></span>
- <span data-ttu-id="d0977-136">Skupiny je přístupná ze všech koncových bodů Exchange a další úlohy Office 365, které podporují skupiny Office 365</span><span class="sxs-lookup"><span data-stu-id="d0977-136">The group can be accessed from any of the Exchange endpoints and other Office 365 workloads that support Office 365 groups</span></span>


## <a name="next-steps"></a><span data-ttu-id="d0977-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d0977-137">Next steps</span></span>
<span data-ttu-id="d0977-138">Tyto články poskytují další informace o skupinách Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d0977-138">These articles provide additional information on Azure Active Directory groups.</span></span>

* [<span data-ttu-id="d0977-139">Zobrazení existujících skupin</span><span class="sxs-lookup"><span data-stu-id="d0977-139">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="d0977-140">Spravovat nastavení skupiny</span><span class="sxs-lookup"><span data-stu-id="d0977-140">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="d0977-141">Správa členů skupiny</span><span class="sxs-lookup"><span data-stu-id="d0977-141">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="d0977-142">Správa členství ve skupině</span><span class="sxs-lookup"><span data-stu-id="d0977-142">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="d0977-143">Dynamická pravidla pro uživatele ve skupině pro správu</span><span class="sxs-lookup"><span data-stu-id="d0977-143">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
