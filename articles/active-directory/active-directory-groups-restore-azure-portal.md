---
title: "skupiny aaaRestore odstraněné Office 365 ve službě Azure Active Directory | Microsoft Docs"
description: "Jak toorestore odstraněné skupině, zobrazit obnovitelné skupiny a permamnently odstranit skupinu v Azure Active Directory"
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
ms.openlocfilehash: 4e6650d64aa19f0c909f1c511f48681de8032f63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restore-a-deleted-office-365-group-in-azure-active-directory"></a><span data-ttu-id="071d4-103">Obnovit odstraněné skupiny Office 365 ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="071d4-103">Restore a deleted Office 365 group in Azure Active Directory</span></span>

<span data-ttu-id="071d4-104">Při odstranění skupiny služby Office 365 v hello Azure Active Directory (Azure AD), hello odstranit skupina je zachované, ale nejsou viditelné po dobu 30 dnů od data odstranění hello.</span><span class="sxs-lookup"><span data-stu-id="071d4-104">When you delete an Office 365 group in hello Azure Active Directory (Azure AD), hello deleted group is retained but not visible for 30 days from hello deletion date.</span></span> <span data-ttu-id="071d4-105">Toto je tak, aby hello skupiny a její obsah se dají obnovovat v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="071d4-105">This is so that hello group and its contents can be restored if needed.</span></span> <span data-ttu-id="071d4-106">Tato funkce je omezen výhradně tooOffice 365 skupiny ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="071d4-106">This functionality is restricted exclusively tooOffice 365 groups in Azure AD.</span></span> <span data-ttu-id="071d4-107">Není k dispozici pro skupiny zabezpečení a distribučních skupin.</span><span class="sxs-lookup"><span data-stu-id="071d4-107">It is not available for security groups and distribution groups.</span></span>

> [!NOTE] 
> <span data-ttu-id="071d4-108">Nepoužívejte `Remove-MsolGroup` protože skupiny hello trvale odstraní.</span><span class="sxs-lookup"><span data-stu-id="071d4-108">Don't use `Remove-MsolGroup` because it purges hello group permanently.</span></span> <span data-ttu-id="071d4-109">Vždy používat `Remove-AzureADMSGroup` skupiny toodelete O365.</span><span class="sxs-lookup"><span data-stu-id="071d4-109">Always use `Remove-AzureADMSGroup` toodelete an O365 group.</span></span> 

<span data-ttu-id="071d4-110">Hello oprávnění požadované toorestore skupiny může být libovolná z hello následující:</span><span class="sxs-lookup"><span data-stu-id="071d4-110">hello permissions required toorestore a group can be any of hello following:</span></span>

<span data-ttu-id="071d4-111">Role</span><span class="sxs-lookup"><span data-stu-id="071d4-111">Role</span></span>  | <span data-ttu-id="071d4-112">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="071d4-112">Permissions</span></span> 
--------- | ---------
<span data-ttu-id="071d4-113">Správce společnosti, partnera Tier2 podpory a správci služby InTune</span><span class="sxs-lookup"><span data-stu-id="071d4-113">Company Administrator, Partner Tier2 support, and InTune Service Admins</span></span> | <span data-ttu-id="071d4-114">Můžete obnovit všechny odstraněné skupiny Office 365</span><span class="sxs-lookup"><span data-stu-id="071d4-114">Can restore any deleted Office 365 group</span></span> 
<span data-ttu-id="071d4-115">Podpora uživatelského účtu správce a Tier1 partnera</span><span class="sxs-lookup"><span data-stu-id="071d4-115">User Account Administrator and Partner Tier1 support</span></span> | <span data-ttu-id="071d4-116">Můžete obnovit všechny odstraněné skupiny Office 365 s výjimkou těchto toohello přiřazenou roli správce společnosti</span><span class="sxs-lookup"><span data-stu-id="071d4-116">Can restore any deleted Office 365 group except those assigned toohello Company Administrator role</span></span> 
<span data-ttu-id="071d4-117">Uživatel</span><span class="sxs-lookup"><span data-stu-id="071d4-117">User</span></span> | <span data-ttu-id="071d4-118">Můžete obnovit všechny odstraněné skupiny Office 365, který se ve vlastnictví</span><span class="sxs-lookup"><span data-stu-id="071d4-118">Can restore any deleted Office 365 group that they owned</span></span> 


## <a name="view-hello-deleted-office-365-groups-that-are-available-toorestore"></a><span data-ttu-id="071d4-119">Zobrazení hello odstranit skupiny Office 365, které jsou k dispozici toorestore</span><span class="sxs-lookup"><span data-stu-id="071d4-119">View hello deleted Office 365 groups that are available toorestore</span></span>
<span data-ttu-id="071d4-120">Hello následující rutiny lze použít tooview hello odstranit skupiny tooverify, který hello jednu nebo ty, které, které vás zajímají nebyly dosud vyprázdnila trvale.</span><span class="sxs-lookup"><span data-stu-id="071d4-120">hello following cmdlets can be used tooview hello deleted groups tooverify that hello one or ones you're interested in have not yet been permanently purged.</span></span> <span data-ttu-id="071d4-121">Tyto rutiny jsou součástí hello [modulu Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureAD/).</span><span class="sxs-lookup"><span data-stu-id="071d4-121">These cmdlets are part of hello [Azure AD PowerShell module](https://www.powershellgallery.com/packages/AzureAD/).</span></span> <span data-ttu-id="071d4-122">Další informace o tomto modulu najdete v hello [Azure Active Directory PowerShell verze 2](/powershell/azure/install-adv2?view=azureadps-2.0) článku.</span><span class="sxs-lookup"><span data-stu-id="071d4-122">More information about this module can be found in hello [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0) article.</span></span>

1.  <span data-ttu-id="071d4-123">Spusťte následující rutinu toodisplay odstranit skupiny Office 365 ve vašem klientovi, které jsou stále k dispozici toorestore hello.</span><span class="sxs-lookup"><span data-stu-id="071d4-123">Run hello following cmdlet toodisplay all deleted Office 365 groups in your tenant that are still available toorestore.</span></span>
  ```
  Get-AzureADMSDeletedGroup
  ```

2.  <span data-ttu-id="071d4-124">Případně pokud víte hello objectID určité skupiny (a můžete ho získat prostřednictvím hello rutiny v kroku 1), spusťte hello následující rutiny tooverify, který hello konkrétní odstraněné skupiny nebyl ještě vyprázdnila trvale.</span><span class="sxs-lookup"><span data-stu-id="071d4-124">Alternately, if you know hello objectID of a specific group (and you can get it from hello cmdlet in step 1), run hello following cmdlet tooverify that hello specific deleted group has not yet been permanently purged.</span></span>
  ```
  Get-AzureADMSDeletedGroup –Id <objectId>
  ```



## <a name="how-toorestore-your-deleted-office-365-group"></a><span data-ttu-id="071d4-125">Jak toorestore vaše odstraněné skupiny Office 365</span><span class="sxs-lookup"><span data-stu-id="071d4-125">How toorestore your deleted Office 365 group</span></span>
<span data-ttu-id="071d4-126">Jakmile si ověříte, že dané skupiny hello je stále k dispozici toorestore, obnovení hello odstranit skupiny s jedním z následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="071d4-126">Once you have verified that hello group is still available toorestore, restore hello deleted group with one of hello following steps.</span></span> <span data-ttu-id="071d4-127">Pokud skupina hello obsahuje dokumenty, SP lokalit nebo jiné trvalé objekty, může trvat až too24 hodin toofully obnovení skupiny a její obsah.</span><span class="sxs-lookup"><span data-stu-id="071d4-127">If hello group contains documents, SP sites, or other persistent objects, it might take up too24 hours toofully restore a group and its contents.</span></span>

1.  <span data-ttu-id="071d4-128">Spuštění hello následující rutiny toorestore hello skupiny a její obsah.</span><span class="sxs-lookup"><span data-stu-id="071d4-128">Run hello following cmdlet toorestore hello group and its contents.</span></span>
  
  ```
  Restore-AzureADMSDeletedDirectoryObject –Id <objectId>
  ``` 

<span data-ttu-id="071d4-129">Alternativně hello následující rutiny můžete spustit toopermanently odstranit hello odstranit skupinu.</span><span class="sxs-lookup"><span data-stu-id="071d4-129">Alternatively, hello following cmdlet can be run toopermanently remove hello deleted group.</span></span>
  ```
  Remove-AzureADMSDeletedDirectoryObject –Id <objectId>
  ```

## <a name="how-do-you-know-this-worked"></a><span data-ttu-id="071d4-130">Jak to šlo znáte?</span><span class="sxs-lookup"><span data-stu-id="071d4-130">How do you know this worked?</span></span>
<span data-ttu-id="071d4-131">tooverify jste úspěšně obnovil skupiny služby Office 365, spusťte hello `Get-AzureADGroup –ObjectId <objectId>` rutiny toodisplay informace o skupině hello.</span><span class="sxs-lookup"><span data-stu-id="071d4-131">tooverify that you’ve successfully restored an Office 365 group, run hello `Get-AzureADGroup –ObjectId <objectId>` cmdlet toodisplay information about hello group.</span></span> <span data-ttu-id="071d4-132">Po obnovení hello dokončení žádosti:</span><span class="sxs-lookup"><span data-stu-id="071d4-132">After hello restore request is completed:</span></span>
- <span data-ttu-id="071d4-133">Skupina Hello se zobrazí v levém navigačního panelu hello na Exchange</span><span class="sxs-lookup"><span data-stu-id="071d4-133">hello group will appear in hello Left nav bar on Exchange</span></span>
- <span data-ttu-id="071d4-134">Zobrazí se v Planner Hello plán pro skupinu hello</span><span class="sxs-lookup"><span data-stu-id="071d4-134">hello plan for hello group will appear in Planner</span></span>
- <span data-ttu-id="071d4-135">Všechny weby služby Sharepoint a veškerý jeho obsah bude k dispozici</span><span class="sxs-lookup"><span data-stu-id="071d4-135">Any Sharepoint sites and all of their contents will be available</span></span>
- <span data-ttu-id="071d4-136">skupiny Hello je přístupná ze všech koncových bodů hello Exchange a další úlohy Office 365, které podporují skupiny Office 365</span><span class="sxs-lookup"><span data-stu-id="071d4-136">hello group can be accessed from any of hello Exchange endpoints and other Office 365 workloads that support Office 365 groups</span></span>


## <a name="next-steps"></a><span data-ttu-id="071d4-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="071d4-137">Next steps</span></span>
<span data-ttu-id="071d4-138">Tyto články poskytují další informace o skupinách Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="071d4-138">These articles provide additional information on Azure Active Directory groups.</span></span>

* [<span data-ttu-id="071d4-139">Zobrazení existujících skupin</span><span class="sxs-lookup"><span data-stu-id="071d4-139">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="071d4-140">Spravovat nastavení skupiny</span><span class="sxs-lookup"><span data-stu-id="071d4-140">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="071d4-141">Správa členů skupiny</span><span class="sxs-lookup"><span data-stu-id="071d4-141">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="071d4-142">Správa členství ve skupině</span><span class="sxs-lookup"><span data-stu-id="071d4-142">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="071d4-143">Dynamická pravidla pro uživatele ve skupině pro správu</span><span class="sxs-lookup"><span data-stu-id="071d4-143">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
