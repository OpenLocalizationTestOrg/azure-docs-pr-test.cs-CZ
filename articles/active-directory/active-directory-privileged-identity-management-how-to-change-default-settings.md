---
title: "nastavení aktivace role toomanage aaaHow | Microsoft Docs"
description: "Zjistěte, jak toochange hello výchozí nastavení pro privilegované identity s hello rozšíření Azure Active Directory Privileged Identity Management."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: f6cbcb6a-8a89-4077-afd8-06c94a64f4aa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 453bb6f8f8e0fd2598cb073ef86c1dd55458dc72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-role-activation-settings-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="70bcd-103">Jak toomanage nastavení aktivace role v Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="70bcd-103">How toomanage role activation settings in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="70bcd-104">Správce privilegovaných rolí můžete přizpůsobit Azure AD Privileged Identity Management (PIM) ve své organizaci, včetně změny hello prostředí pro uživatele, který je aktivace přiřazení role vhodné.</span><span class="sxs-lookup"><span data-stu-id="70bcd-104">A privileged role administrator can customize Azure AD Privileged Identity Management (PIM) in their organization, including changing hello experience for a user who is activating an eligible role assignment.</span></span>

## <a name="manage-hello-role-activation-settings"></a><span data-ttu-id="70bcd-105">Spravovat nastavení aktivace role hello</span><span class="sxs-lookup"><span data-stu-id="70bcd-105">Manage hello role activation settings</span></span>
1. <span data-ttu-id="70bcd-106">Přejděte toohello [portál Azure](https://portal.azure.com) a vyberte hello **Azure AD Privileged Identity Management** aplikace z řídicího panelu hello.</span><span class="sxs-lookup"><span data-stu-id="70bcd-106">Go toohello [Azure portal](https://portal.azure.com) and select hello **Azure AD Privileged Identity Management** app from hello dashboard.</span></span>
2. <span data-ttu-id="70bcd-107">Vyberte **spravovat privilegované role** > **nastavení** > **privilegované role**.</span><span class="sxs-lookup"><span data-stu-id="70bcd-107">Select **Manage privileged roles** > **Settings** > **Privileged Roles**.</span></span>
3. <span data-ttu-id="70bcd-108">Zvolte roli hello, jehož nastavení chcete toomanage.</span><span class="sxs-lookup"><span data-stu-id="70bcd-108">Choose hello role whose settings you want toomanage.</span></span>

<span data-ttu-id="70bcd-109">Na stránce nastavení hello u každé role existuje několik nastavení, které můžete konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="70bcd-109">On hello settings page for each role, there are a number of settings you can configure.</span></span> <span data-ttu-id="70bcd-110">Tato nastavení ovlivňují jenom uživatelé, kteří jsou správci oprávněné, není trvalých správců.</span><span class="sxs-lookup"><span data-stu-id="70bcd-110">These settings only affect users who are eligible admins, not permanent admins.</span></span>

<span data-ttu-id="70bcd-111">**Aktivace**: hello doba v hodinách, které role zůstává aktivní, než vyprší její platnost.</span><span class="sxs-lookup"><span data-stu-id="70bcd-111">**Activations**: hello time, in hours, that a role stays active before it expires.</span></span> <span data-ttu-id="70bcd-112">To může být v rozmezí 1 až 72 hodin.</span><span class="sxs-lookup"><span data-stu-id="70bcd-112">This can be between 1 and 72 hours.</span></span>

<span data-ttu-id="70bcd-113">**Oznámení**: můžete zvolit, zda systém hello odešle e-mailů tooadmins potvrzení, že se mají aktivovat roli.</span><span class="sxs-lookup"><span data-stu-id="70bcd-113">**Notifications**: You can choose whether or not hello system sends emails tooadmins confirming that they have activated a role.</span></span> <span data-ttu-id="70bcd-114">To může být užitečné pro zjišťování neoprávněným nebo nezákonných aktivací.</span><span class="sxs-lookup"><span data-stu-id="70bcd-114">This can be useful for detecting unauthorized or illegitimate activations.</span></span>

<span data-ttu-id="70bcd-115">**Incident nebo žádost o lístek**: můžete zvolit, zda toorequire oprávněné admins tooinclude lístek číselné při aktivují jejich role.</span><span class="sxs-lookup"><span data-stu-id="70bcd-115">**Incident/Request Ticket**: You can choose whether or not toorequire eligible admins tooinclude a ticket number when they activate their role.</span></span> <span data-ttu-id="70bcd-116">To může být užitečné při provádění auditů role přístup.</span><span class="sxs-lookup"><span data-stu-id="70bcd-116">This can be useful when you perform role access audits.</span></span>

<span data-ttu-id="70bcd-117">**Služby Multi-Factor Authentication**: můžete zvolit, jestli uživatelé tooverify toorequire svou identitu pomocí vícefaktorového ověřování před aktivací jejich rolí.</span><span class="sxs-lookup"><span data-stu-id="70bcd-117">**Multi-Factor Authentication**: You can choose whether or not toorequire users tooverify their identity with MFA before they can activate their roles.</span></span> <span data-ttu-id="70bcd-118">Mají pouze tooverify tento jednou na relaci, není pokaždé, když se aktivovat roli.</span><span class="sxs-lookup"><span data-stu-id="70bcd-118">They only have tooverify this once per session, not every time they activate a role.</span></span> <span data-ttu-id="70bcd-119">Existují dva typy tookeep na paměti, když povolíte vícefaktorového ověřování:</span><span class="sxs-lookup"><span data-stu-id="70bcd-119">There are two tips tookeep in mind when you enable MFA:</span></span>

* <span data-ttu-id="70bcd-120">Uživatelé, kteří mají účty Microsoft pro jejich e-mailové adresy (obvykle @outlook.com, ale ne vždy) nelze zaregistrovat pro Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="70bcd-120">Users who have Microsoft accounts for their email addresses (typically @outlook.com, but not always) cannot register for Azure MFA.</span></span> <span data-ttu-id="70bcd-121">Pokud chcete tooassign role toousers s účty Microsoft, by je provést trvalých správců nebo zakázat MFA pro tuto roli.</span><span class="sxs-lookup"><span data-stu-id="70bcd-121">If you want tooassign roles toousers with Microsoft accounts, you should either make them permanent admins or disable MFA for that role.</span></span>
* <span data-ttu-id="70bcd-122">Nelze zakázat MFA pro vysoce privilegované role pro Azure AD a Office 365.</span><span class="sxs-lookup"><span data-stu-id="70bcd-122">You cannot disable MFA for highly privileged roles for Azure AD and Office365.</span></span> <span data-ttu-id="70bcd-123">Toto je funkce zabezpečení, protože tyto role je třeba pečlivě chránit:</span><span class="sxs-lookup"><span data-stu-id="70bcd-123">This is a safety feature because these roles should be carefully protected:</span></span>  
  
  * <span data-ttu-id="70bcd-124">Správce aplikací</span><span class="sxs-lookup"><span data-stu-id="70bcd-124">Application administrator</span></span>
  * <span data-ttu-id="70bcd-125">Správce serveru Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="70bcd-125">Application Proxy server administrator</span></span>
  * <span data-ttu-id="70bcd-126">Správce fakturace</span><span class="sxs-lookup"><span data-stu-id="70bcd-126">Billing administrator</span></span>  
  * <span data-ttu-id="70bcd-127">Správce dodržování předpisů</span><span class="sxs-lookup"><span data-stu-id="70bcd-127">Compliance administrator</span></span>  
  * <span data-ttu-id="70bcd-128">Správce služeb CRM</span><span class="sxs-lookup"><span data-stu-id="70bcd-128">CRM service administrator</span></span>
  * <span data-ttu-id="70bcd-129">Schvalovatel přístup bezpečnostního modulu zákazníka</span><span class="sxs-lookup"><span data-stu-id="70bcd-129">Customer LockBox access approver</span></span>
  * <span data-ttu-id="70bcd-130">Zapisovač adresáře</span><span class="sxs-lookup"><span data-stu-id="70bcd-130">Directory writer</span></span>  
  * <span data-ttu-id="70bcd-131">Správce serveru Exchange</span><span class="sxs-lookup"><span data-stu-id="70bcd-131">Exchange administrator</span></span>  
  * <span data-ttu-id="70bcd-132">Globální správce</span><span class="sxs-lookup"><span data-stu-id="70bcd-132">Global administrator</span></span>
  * <span data-ttu-id="70bcd-133">Správce služby Intune</span><span class="sxs-lookup"><span data-stu-id="70bcd-133">Intune service administrator</span></span>
  * <span data-ttu-id="70bcd-134">Poštovní schránky správce</span><span class="sxs-lookup"><span data-stu-id="70bcd-134">Mailbox administrator</span></span>  
  * <span data-ttu-id="70bcd-135">Podpora tier1 partnera</span><span class="sxs-lookup"><span data-stu-id="70bcd-135">Partner tier1 support</span></span>  
  * <span data-ttu-id="70bcd-136">Podpora tier2 partnera</span><span class="sxs-lookup"><span data-stu-id="70bcd-136">Partner tier2 support</span></span>  
  * <span data-ttu-id="70bcd-137">Správce privilegovaných rolí</span><span class="sxs-lookup"><span data-stu-id="70bcd-137">Privileged role administrator</span></span>   
  * <span data-ttu-id="70bcd-138">Správce zabezpečení</span><span class="sxs-lookup"><span data-stu-id="70bcd-138">Security administrator</span></span>  
  * <span data-ttu-id="70bcd-139">Správce služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="70bcd-139">SharePoint administrator</span></span>  
  * <span data-ttu-id="70bcd-140">Správce Skypu pro firmy</span><span class="sxs-lookup"><span data-stu-id="70bcd-140">Skype for Business administrator</span></span>  
  * <span data-ttu-id="70bcd-141">Správce účtu uživatele</span><span class="sxs-lookup"><span data-stu-id="70bcd-141">User account administrator</span></span>  

<span data-ttu-id="70bcd-142">Další informace o použití vícefaktorového ověřování se PIM najdete v části [jak tooRequire MFA](active-directory-privileged-identity-management-how-to-require-mfa.md).</span><span class="sxs-lookup"><span data-stu-id="70bcd-142">For more information about using MFA with PIM see [How tooRequire MFA](active-directory-privileged-identity-management-how-to-require-mfa.md).</span></span>

<!--PLACEHOLDER: Need an explanation of what hello temporary Global Administrator setting is for.-->

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="70bcd-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="70bcd-143">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

