---
title: "Postup správy nastavení aktivace role | Microsoft Docs"
description: "Zjistěte, jak změnit výchozí nastavení pro privilegované identity pomocí rozšíření Azure Active Directory Privileged Identity Management."
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
ms.openlocfilehash: 23605e89cd1846d2e06e48cb5d3e0191cb9e9b4a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-role-activation-settings-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="64629-103">Postup správy nastavení aktivace role v Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="64629-103">How to manage role activation settings in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="64629-104">Správce privilegovaných rolí můžete přizpůsobit Azure AD Privileged Identity Management (PIM) ve své organizaci, včetně změny prostředí pro uživatele, který je aktivace přiřazení role vhodné.</span><span class="sxs-lookup"><span data-stu-id="64629-104">A privileged role administrator can customize Azure AD Privileged Identity Management (PIM) in their organization, including changing the experience for a user who is activating an eligible role assignment.</span></span>

## <a name="manage-the-role-activation-settings"></a><span data-ttu-id="64629-105">Spravovat nastavení aktivace role</span><span class="sxs-lookup"><span data-stu-id="64629-105">Manage the role activation settings</span></span>
1. <span data-ttu-id="64629-106">Přejděte na [portál Azure](https://portal.azure.com) a vyberte **Azure AD Privileged Identity Management** aplikace z řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="64629-106">Go to the [Azure portal](https://portal.azure.com) and select the **Azure AD Privileged Identity Management** app from the dashboard.</span></span>
2. <span data-ttu-id="64629-107">Vyberte **spravovat privilegované role** > **nastavení** > **privilegované role**.</span><span class="sxs-lookup"><span data-stu-id="64629-107">Select **Manage privileged roles** > **Settings** > **Privileged Roles**.</span></span>
3. <span data-ttu-id="64629-108">Vyberte roli, jehož nastavení chcete spravovat.</span><span class="sxs-lookup"><span data-stu-id="64629-108">Choose the role whose settings you want to manage.</span></span>

<span data-ttu-id="64629-109">Na stránce nastavení pro jednotlivé role existuje několik nastavení, které můžete konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="64629-109">On the settings page for each role, there are a number of settings you can configure.</span></span> <span data-ttu-id="64629-110">Tato nastavení ovlivňují jenom uživatelé, kteří jsou správci oprávněné, není trvalých správců.</span><span class="sxs-lookup"><span data-stu-id="64629-110">These settings only affect users who are eligible admins, not permanent admins.</span></span>

<span data-ttu-id="64629-111">**Aktivace**: doba, v hodinách, které role zůstává aktivní, než vyprší její platnost.</span><span class="sxs-lookup"><span data-stu-id="64629-111">**Activations**: The time, in hours, that a role stays active before it expires.</span></span> <span data-ttu-id="64629-112">To může být v rozmezí 1 až 72 hodin.</span><span class="sxs-lookup"><span data-stu-id="64629-112">This can be between 1 and 72 hours.</span></span>

<span data-ttu-id="64629-113">**Oznámení**: můžete zvolit, zda systém admins potvrzení, že se mají aktivovat roli odešle e-mailů.</span><span class="sxs-lookup"><span data-stu-id="64629-113">**Notifications**: You can choose whether or not the system sends emails to admins confirming that they have activated a role.</span></span> <span data-ttu-id="64629-114">To může být užitečné pro zjišťování neoprávněným nebo nezákonných aktivací.</span><span class="sxs-lookup"><span data-stu-id="64629-114">This can be useful for detecting unauthorized or illegitimate activations.</span></span>

<span data-ttu-id="64629-115">**Incident nebo žádost o lístek**: můžete zvolit, zda se má vyžadovat oprávněné správcům zahrnout číslo lístku aktivují jejich role.</span><span class="sxs-lookup"><span data-stu-id="64629-115">**Incident/Request Ticket**: You can choose whether or not to require eligible admins to include a ticket number when they activate their role.</span></span> <span data-ttu-id="64629-116">To může být užitečné při provádění auditů role přístup.</span><span class="sxs-lookup"><span data-stu-id="64629-116">This can be useful when you perform role access audits.</span></span>

<span data-ttu-id="64629-117">**Služba Multi-Factor Authentication**: můžete zvolit, zda budou muset uživatelé před aktivací jejich rolí, ověřovali svoji identitu pomocí vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="64629-117">**Multi-Factor Authentication**: You can choose whether or not to require users to verify their identity with MFA before they can activate their roles.</span></span> <span data-ttu-id="64629-118">Mají pouze k ověření tohoto jednou na relaci, není pokaždé, když se aktivovat roli.</span><span class="sxs-lookup"><span data-stu-id="64629-118">They only have to verify this once per session, not every time they activate a role.</span></span> <span data-ttu-id="64629-119">Existují dva typy mít na paměti, když povolíte vícefaktorového ověřování:</span><span class="sxs-lookup"><span data-stu-id="64629-119">There are two tips to keep in mind when you enable MFA:</span></span>

* <span data-ttu-id="64629-120">Uživatelé, kteří mají účty Microsoft pro jejich e-mailové adresy (obvykle @outlook.com, ale ne vždy) nelze zaregistrovat pro Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="64629-120">Users who have Microsoft accounts for their email addresses (typically @outlook.com, but not always) cannot register for Azure MFA.</span></span> <span data-ttu-id="64629-121">Pokud chcete přiřadit role pro uživatele s účty Microsoft, by je provést trvalých správců nebo zakázat MFA pro tuto roli.</span><span class="sxs-lookup"><span data-stu-id="64629-121">If you want to assign roles to users with Microsoft accounts, you should either make them permanent admins or disable MFA for that role.</span></span>
* <span data-ttu-id="64629-122">Nelze zakázat MFA pro vysoce privilegované role pro Azure AD a Office 365.</span><span class="sxs-lookup"><span data-stu-id="64629-122">You cannot disable MFA for highly privileged roles for Azure AD and Office365.</span></span> <span data-ttu-id="64629-123">Toto je funkce zabezpečení, protože tyto role je třeba pečlivě chránit:</span><span class="sxs-lookup"><span data-stu-id="64629-123">This is a safety feature because these roles should be carefully protected:</span></span>  
  
  * <span data-ttu-id="64629-124">Správce aplikací</span><span class="sxs-lookup"><span data-stu-id="64629-124">Application administrator</span></span>
  * <span data-ttu-id="64629-125">Správce serveru Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="64629-125">Application Proxy server administrator</span></span>
  * <span data-ttu-id="64629-126">Správce fakturace</span><span class="sxs-lookup"><span data-stu-id="64629-126">Billing administrator</span></span>  
  * <span data-ttu-id="64629-127">Správce dodržování předpisů</span><span class="sxs-lookup"><span data-stu-id="64629-127">Compliance administrator</span></span>  
  * <span data-ttu-id="64629-128">Správce služeb CRM</span><span class="sxs-lookup"><span data-stu-id="64629-128">CRM service administrator</span></span>
  * <span data-ttu-id="64629-129">Schvalovatel přístup bezpečnostního modulu zákazníka</span><span class="sxs-lookup"><span data-stu-id="64629-129">Customer LockBox access approver</span></span>
  * <span data-ttu-id="64629-130">Zapisovač adresáře</span><span class="sxs-lookup"><span data-stu-id="64629-130">Directory writer</span></span>  
  * <span data-ttu-id="64629-131">Správce serveru Exchange</span><span class="sxs-lookup"><span data-stu-id="64629-131">Exchange administrator</span></span>  
  * <span data-ttu-id="64629-132">Globální správce</span><span class="sxs-lookup"><span data-stu-id="64629-132">Global administrator</span></span>
  * <span data-ttu-id="64629-133">Správce služby Intune</span><span class="sxs-lookup"><span data-stu-id="64629-133">Intune service administrator</span></span>
  * <span data-ttu-id="64629-134">Poštovní schránky správce</span><span class="sxs-lookup"><span data-stu-id="64629-134">Mailbox administrator</span></span>  
  * <span data-ttu-id="64629-135">Podpora tier1 partnera</span><span class="sxs-lookup"><span data-stu-id="64629-135">Partner tier1 support</span></span>  
  * <span data-ttu-id="64629-136">Podpora tier2 partnera</span><span class="sxs-lookup"><span data-stu-id="64629-136">Partner tier2 support</span></span>  
  * <span data-ttu-id="64629-137">Správce privilegovaných rolí</span><span class="sxs-lookup"><span data-stu-id="64629-137">Privileged role administrator</span></span>   
  * <span data-ttu-id="64629-138">Správce zabezpečení</span><span class="sxs-lookup"><span data-stu-id="64629-138">Security administrator</span></span>  
  * <span data-ttu-id="64629-139">Správce služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="64629-139">SharePoint administrator</span></span>  
  * <span data-ttu-id="64629-140">Správce Skypu pro firmy</span><span class="sxs-lookup"><span data-stu-id="64629-140">Skype for Business administrator</span></span>  
  * <span data-ttu-id="64629-141">Správce účtu uživatele</span><span class="sxs-lookup"><span data-stu-id="64629-141">User account administrator</span></span>  

<span data-ttu-id="64629-142">Další informace o použití vícefaktorového ověřování se PIM najdete v části [jak vyžadovat vícefaktorové ověřování](active-directory-privileged-identity-management-how-to-require-mfa.md).</span><span class="sxs-lookup"><span data-stu-id="64629-142">For more information about using MFA with PIM see [How to Require MFA](active-directory-privileged-identity-management-how-to-require-mfa.md).</span></span>

<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="64629-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="64629-143">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

