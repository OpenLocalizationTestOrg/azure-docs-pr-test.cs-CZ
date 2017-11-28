---
title: Konfigurovat Azure AD Privileged Identity Management | Microsoft Docs
description: "Téma, které vysvětluje, co je Azure AD Privileged Identity managementu a jak používat PIM k zlepšují zabezpečení vašeho cloudu."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: c548ed2e-06e3-4eaf-a63d-0f02ee72da25
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: eb7059368cb80be7dd625f9dc6ad2aab1bad709a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-azure-ad-privileged-identity-management"></a><span data-ttu-id="8a8a0-103">Co je Azure AD Privileged Identity Management?</span><span class="sxs-lookup"><span data-stu-id="8a8a0-103">What is Azure AD Privileged Identity Management?</span></span>
<span data-ttu-id="8a8a0-104">Pomocí aplikace Azure Active Directory (AD) Privileged Identity Management můžete spravovat, řídit a sledovat přístup v rámci organizace.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-104">With Azure Active Directory (AD) Privileged Identity Management, you can manage, control, and monitor access within your organization.</span></span> <span data-ttu-id="8a8a0-105">To zahrnuje přístup k prostředkům v Azure AD a dalších online službách Microsoftu jako Office 365 nebo Microsoft Intune.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-105">This includes access to resources in Azure AD and other Microsoft online services like Office 365 or Microsoft Intune.</span></span>  

> [!NOTE]
> <span data-ttu-id="8a8a0-106">Správa privilegovaných identit je dostupná pro celou organizaci, když licence správce s na edici Premium P2 služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-106">Privileged Identity Management is available to your entire organization when you license your Administrators with the Premium P2 edition of Azure Active Directory.</span></span> <span data-ttu-id="8a8a0-107">Další informace najdete v článku [Edice služby Azure Active Directory](active-directory-editions.md).</span><span class="sxs-lookup"><span data-stu-id="8a8a0-107">For more information, see [Azure Active Directory editions](active-directory-editions.md).</span></span>

<span data-ttu-id="8a8a0-108">Organizace chcete minimalizovat počet lidí, kteří mají přístup k zabezpečeným informacím nebo prostředkům, protože který snižuje riziko uživatel se zlými úmysly získávání tento přístup.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-108">Organizations want to minimize the number of people who have access to secure information or resources, because that reduces the chance of a malicious user getting that access.</span></span> <span data-ttu-id="8a8a0-109">Ale uživatelé stále muset provádět privilegované operace v Azure, Office 365 nebo SaaS aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-109">However, users still need to carry out privileged operations in Azure, Office 365, or SaaS apps.</span></span> <span data-ttu-id="8a8a0-110">Organizace poskytnout privilegovaného přístupu uživatelů ve službě Azure AD bez monitorování tyto činnosti uživatelů s jejich oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-110">Organizations give users privileged access in Azure AD without monitoring what those users are doing with their admin privileges.</span></span> <span data-ttu-id="8a8a0-111">Azure AD Privileged Identity Management pomáhá vyřešit toto riziko.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-111">Azure AD Privileged Identity Management helps to resolve this risk.</span></span>  

<span data-ttu-id="8a8a0-112">Azure AD Privileged Identity Management vám pomůže:</span><span class="sxs-lookup"><span data-stu-id="8a8a0-112">Azure AD Privileged Identity Management helps you:</span></span>  

* <span data-ttu-id="8a8a0-113">Uvidíte, kteří uživatelé jsou správci služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8a8a0-113">See which users are Azure AD administrators</span></span>
* <span data-ttu-id="8a8a0-114">Povolit na vyžádání "právě v čase" pro správu přístup k Online službám společnosti Microsoft, jako třeba Office 365 a Intune</span><span class="sxs-lookup"><span data-stu-id="8a8a0-114">Enable on-demand, "just in time" administrative access to Microsoft Online Services like Office 365 and Intune</span></span>
* <span data-ttu-id="8a8a0-115">Získání sestavy o historii přístup správce a změny v přiřazení správců</span><span class="sxs-lookup"><span data-stu-id="8a8a0-115">Get reports about administrator access history and changes in administrator assignments</span></span>
* <span data-ttu-id="8a8a0-116">Získání výstrahy týkající se přístupu k privilegované role.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-116">Get alerts about access to a privileged role</span></span>
* <span data-ttu-id="8a8a0-117">Vyžadovat schválení aktivovat (Preview)</span><span class="sxs-lookup"><span data-stu-id="8a8a0-117">Require approval to activate (Preview)</span></span>

<span data-ttu-id="8a8a0-118">Azure AD Privileged Identity Management můžete spravovat integrované organizační role Azure AD, včetně (ale bez omezení):</span><span class="sxs-lookup"><span data-stu-id="8a8a0-118">Azure AD Privileged Identity Management can manage the built-in Azure AD organizational roles, including (but not limited to):</span></span>  

* <span data-ttu-id="8a8a0-119">Globální správce</span><span class="sxs-lookup"><span data-stu-id="8a8a0-119">Global Administrator</span></span>
* <span data-ttu-id="8a8a0-120">Správce fakturace</span><span class="sxs-lookup"><span data-stu-id="8a8a0-120">Billing Administrator</span></span>
* <span data-ttu-id="8a8a0-121">Správce služeb</span><span class="sxs-lookup"><span data-stu-id="8a8a0-121">Service Administrator</span></span>  
* <span data-ttu-id="8a8a0-122">Správce uživatele</span><span class="sxs-lookup"><span data-stu-id="8a8a0-122">User Administrator</span></span>
* <span data-ttu-id="8a8a0-123">Správce hesel</span><span class="sxs-lookup"><span data-stu-id="8a8a0-123">Password Administrator</span></span>

## <a name="just-in-time-administrator-access"></a><span data-ttu-id="8a8a0-124">Právě v přístup správce čas</span><span class="sxs-lookup"><span data-stu-id="8a8a0-124">Just in time administrator access</span></span>
<span data-ttu-id="8a8a0-125">V minulosti může přiřadit uživatele k roli správce prostřednictvím portálu Azure classic nebo prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-125">Historically, you could assign a user to an admin role through the Azure classic portal or Windows PowerShell.</span></span> <span data-ttu-id="8a8a0-126">V důsledku toho stane **trvalé správce**, vždy aktivní v přiřazenou roli.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-126">As a result, that user becomes a **permanent admin**, always active in the assigned role.</span></span> <span data-ttu-id="8a8a0-127">Azure AD Privileged Identity Management zavádí koncepci **oprávněné správce**.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-127">Azure AD Privileged Identity Management introduces the concept of an **eligible admin**.</span></span> <span data-ttu-id="8a8a0-128">Oprávněné admins by měl být uživatelům, kteří potřebují privilegovaného přístupu a poté, ale ne každý den.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-128">Eligible admins should be users that need privileged access now and then, but not every day.</span></span> <span data-ttu-id="8a8a0-129">Role je neaktivní, dokud uživatel potřebuje přístup, pak se dokončit proces aktivace a stane aktivní správce po předem určenou dobu.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-129">The role is inactive until the user needs access, then they complete an activation process and become an active admin for a predetermined amount of time.</span></span>

## <a name="enable-privileged-identity-management-for-your-directory"></a><span data-ttu-id="8a8a0-130">Povolit Privileged Identity Management pro svůj adresář</span><span class="sxs-lookup"><span data-stu-id="8a8a0-130">Enable Privileged Identity Management for your directory</span></span>
<span data-ttu-id="8a8a0-131">Můžete začít používat Azure AD Privileged Identity Management [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="8a8a0-131">You can start using Azure AD Privileged Identity Management in the [Azure portal](https://portal.azure.com/).</span></span>

> [!NOTE]
> <span data-ttu-id="8a8a0-132">Musí být globální správce s účtem organizace (například @yourdomain.com), nikoli účet Microsoft (například @outlook.com), aby pro adresář Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-132">You must be a global administrator with an organizational account (for example, @yourdomain.com), not a Microsoft account (for example, @outlook.com), to enable Azure AD Privileged Identity Management for a directory.</span></span>

1. <span data-ttu-id="8a8a0-133">Přihlaste se k webu [Azure Portal](https://portal.azure.com/) jako globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-133">Sign in to the [Azure portal](https://portal.azure.com/) as a global administrator of your directory.</span></span>
2. <span data-ttu-id="8a8a0-134">Pokud má vaše organizace víc než jeden adresář, vyberte své uživatelské jméno v pravém horním rohu webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-134">If your organization has more than one directory, select your username in the upper right-hand corner of the Azure portal.</span></span> <span data-ttu-id="8a8a0-135">Vyberte adresář, kde budete používat Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-135">Select the directory where you will use Azure AD Privileged Identity Management.</span></span>
3. <span data-ttu-id="8a8a0-136">Vyberte **Další služby** a pomocí pole Filtr najděte **Azure AD Privileged Identity Management**.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-136">Select **More services** and use the Filter textbox to search for **Azure AD Privileged Identity Management**.</span></span>
4. <span data-ttu-id="8a8a0-137">Zaškrtněte **Připnout na řídicí panel** a potom klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-137">Check **Pin to dashboard** and then click **Create**.</span></span> <span data-ttu-id="8a8a0-138">Aplikace Privileged Identity Management se otevře.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-138">The Privileged Identity Management application opens.</span></span>

<span data-ttu-id="8a8a0-139">Pokud jste první, kdo bude ve vašem adresáři používat aplikaci Azure AD Privileged Identity Management, [průvodce zabezpečením](active-directory-privileged-identity-management-security-wizard.md) vás provede počátečním přiřazením.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-139">If you're the first person to use Azure AD Privileged Identity Management in your directory, then the [security wizard](active-directory-privileged-identity-management-security-wizard.md) walks you through the initial assignment experience.</span></span> <span data-ttu-id="8a8a0-140">Následně se automaticky stane první **správce zabezpečení** a **správce privilegovaných rolí** adresáře.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-140">After that you automatically become the first **Security administrator** and **Privileged role administrator** of the directory.</span></span>

<span data-ttu-id="8a8a0-141">Pouze správce privilegovaných rolí můžete spravovat přístup pro jiné správce.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-141">Only a privileged role administrator can manage access for other administrators.</span></span> <span data-ttu-id="8a8a0-142">Můžete [ostatním uživatelům předat, schopnost spravovat v PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span><span class="sxs-lookup"><span data-stu-id="8a8a0-142">You can [give other users the ability to manage in PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span></span>

## <a name="privileged-identity-management-admin-dashboard"></a><span data-ttu-id="8a8a0-143">Privilegované řídicí panel Správce Identity Management</span><span class="sxs-lookup"><span data-stu-id="8a8a0-143">Privileged Identity Management admin dashboard</span></span>
<span data-ttu-id="8a8a0-144">Azure AD Privileged Identity Manager poskytuje správce řídicího panelu, který poskytuje důležité informace, jako:</span><span class="sxs-lookup"><span data-stu-id="8a8a0-144">Azure AD Privileged Identity Manager provides an admin dashboard that gives you important information such as:</span></span>

* <span data-ttu-id="8a8a0-145">Výstrahy, které odkazují na příležitosti pro zlepšení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="8a8a0-145">Alerts that point out opportunities to improve security</span></span>
* <span data-ttu-id="8a8a0-146">Počet uživatelů, kteří jsou přiřazeni k každé privilegované role</span><span class="sxs-lookup"><span data-stu-id="8a8a0-146">The number of users who are assigned to each privileged role</span></span>  
* <span data-ttu-id="8a8a0-147">Počet oprávněných a trvalých správců</span><span class="sxs-lookup"><span data-stu-id="8a8a0-147">The number of eligible and permanent admins</span></span>
* <span data-ttu-id="8a8a0-148">Graf aktivací privilegované role ve vašem adresáři</span><span class="sxs-lookup"><span data-stu-id="8a8a0-148">A graph of privileged role activations in your directory</span></span>

![Řídicí panel PIM – snímek obrazovky][2]

## <a name="privileged-role-management"></a><span data-ttu-id="8a8a0-150">Správa privilegovaných rolí</span><span class="sxs-lookup"><span data-stu-id="8a8a0-150">Privileged role management</span></span>
<span data-ttu-id="8a8a0-151">S Azure AD Privileged Identity Management můžete spravovat správce přidáním nebo odebráním trvalé nebo oprávněné správce ke každé roli.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-151">With Azure AD Privileged Identity Management, you can manage the administrators by adding or removing permanent or eligible administrators to each role.</span></span>

![Správci přidat nebo odebrat PIM – snímek obrazovky][3]

## <a name="configure-the-role-activation-settings"></a><span data-ttu-id="8a8a0-153">Konfigurace nastavení aktivace role</span><span class="sxs-lookup"><span data-stu-id="8a8a0-153">Configure the role activation settings</span></span>
<span data-ttu-id="8a8a0-154">Pomocí [nastavení role](active-directory-privileged-identity-management-how-to-change-default-settings.md) můžete konfigurovat vlastnosti aktivace vhodné role, včetně:</span><span class="sxs-lookup"><span data-stu-id="8a8a0-154">Using the [role settings](active-directory-privileged-identity-management-how-to-change-default-settings.md) you can configure the eligible role activation properties including:</span></span>

* <span data-ttu-id="8a8a0-155">Doba trvání v období aktivace role</span><span class="sxs-lookup"><span data-stu-id="8a8a0-155">The duration of the role activation period</span></span>
* <span data-ttu-id="8a8a0-156">Oznámení o aktivaci role</span><span class="sxs-lookup"><span data-stu-id="8a8a0-156">The role activation notification</span></span>
* <span data-ttu-id="8a8a0-157">Informace, které uživatel musí poskytnout během procesu aktivace role</span><span class="sxs-lookup"><span data-stu-id="8a8a0-157">The information a user needs to provide during the role activation process</span></span>
* <span data-ttu-id="8a8a0-158">Číslo služby lístků nebo incidenty.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-158">Service ticket or incident number</span></span>
* [<span data-ttu-id="8a8a0-159">Požadavky na pracovní postup schválení – náhled</span><span class="sxs-lookup"><span data-stu-id="8a8a0-159">Approval workflow requirements - Preview</span></span>](./privileged-identity-management/azure-ad-pim-approval-workflow.md)

![Snímek obrazovky nastavení – správce aktivace – PIM][4]

<span data-ttu-id="8a8a0-161">Všimněte si, že se na obrázku tlačítka pro **Multi-Factor Authentication** jsou zakázány.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-161">Note that in the image, the buttons for **Multi-Factor Authentication** are disabled.</span></span> <span data-ttu-id="8a8a0-162">Pro některé, vysoce privilegované role, jsme vícefaktorové ověřování vyžadovat pro zvýšenou ochranu.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-162">For certain, highly privileged roles, we require MFA for heightened protection.</span></span>

## <a name="role-activation"></a><span data-ttu-id="8a8a0-163">Aktivace role</span><span class="sxs-lookup"><span data-stu-id="8a8a0-163">Role activation</span></span>
<span data-ttu-id="8a8a0-164">K [aktivovat roli](active-directory-privileged-identity-management-how-to-activate-role.md), oprávněné správce požadavky časově vázaných "aktivace" pro roli.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-164">To [activate a role](active-directory-privileged-identity-management-how-to-activate-role.md), an eligible admin requests a time-bound "activation" for the role.</span></span> <span data-ttu-id="8a8a0-165">Aktivace může být požadováno pomocí **aktivovat Moje role** možnost v Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-165">The activation can be requested using the **Activate my role** option in Azure AD Privileged Identity Management.</span></span>

<span data-ttu-id="8a8a0-166">Kdo chce aktivovat roli správce musí inicializovat Azure AD Privileged Identity Management na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-166">An admin who wants to activate a role needs to initialize Azure AD Privileged Identity Management in the Azure portal.</span></span>

<span data-ttu-id="8a8a0-167">Aktivaci role je přizpůsobitelná.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-167">Role activation is customizable.</span></span> <span data-ttu-id="8a8a0-168">V nastavení PIM, můžete zjistit délku aktivace a co informace nutné poskytnout při aktivaci role správce.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-168">In the PIM settings, you can determine the length of the activation and what information the admin needs to provide to activate the role.</span></span>

![Aktivace role PIM správce žádost – snímek obrazovky][5]

## <a name="review-role-activity"></a><span data-ttu-id="8a8a0-170">Kontrolní aktivita role</span><span class="sxs-lookup"><span data-stu-id="8a8a0-170">Review role activity</span></span>
<span data-ttu-id="8a8a0-171">Existují dva způsoby, jak sledovat, jak zaměstnancům a správci používají privilegované role.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-171">There are two ways to track how your employees and admins are using privileged roles.</span></span> <span data-ttu-id="8a8a0-172">První možností je použití [historie auditu role Directory](active-directory-privileged-identity-management-how-to-use-audit-log.md).</span><span class="sxs-lookup"><span data-stu-id="8a8a0-172">The first option is using [Directory Roles audit history](active-directory-privileged-identity-management-how-to-use-audit-log.md).</span></span> <span data-ttu-id="8a8a0-173">Historie auditu zaznamená sledovat změny v přiřazení privilegované role a role aktivace historie.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-173">The audit history logs track changes in privileged role assignments and role activation history.</span></span>

![Historie aktivace PIM – snímek obrazovky][6]

<span data-ttu-id="8a8a0-175">Druhou možností je nastavit regular [přístup recenze](active-directory-privileged-identity-management-how-to-start-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="8a8a0-175">The second option is to set up regular [access reviews](active-directory-privileged-identity-management-how-to-start-security-review.md).</span></span> <span data-ttu-id="8a8a0-176">Tyto recenze přístup může být provádí a přiřazení kontrolor (jako je team manager) nebo zaměstnanci můžete zkontrolovat sami.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-176">These access reviews can be performed by and assigned reviewer (like a team manager) or the employees can review themselves.</span></span> <span data-ttu-id="8a8a0-177">Toto je nejlepší způsob, jak monitorovat kdo stále vyžaduje přístup a kteří už nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-177">This is the best way to monitor who still requires access, and who no longer does.</span></span>

## <a name="azure-ad-pim-at-subscription-expiration"></a><span data-ttu-id="8a8a0-178">Azure AD PIM na vypršení platnosti odběru</span><span class="sxs-lookup"><span data-stu-id="8a8a0-178">Azure AD PIM at subscription expiration</span></span>
<span data-ttu-id="8a8a0-179">Před dosažení obecné dostupnosti Azure AD PIM byl ve verzi preview a k dispozici nebyly žádné kontroly licence pro klienta pro náhled Azure AD PIM.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-179">Prior to reaching general availability Azure AD PIM was in preview and there were no license checks for a tenant to preview Azure AD PIM.</span></span>  <span data-ttu-id="8a8a0-180">Teď, když Azure AD PIM byl dosažen obecné dostupnosti, musí přiřadit licence zkušební nebo placený správcům klienta chcete pokračovat v používání PIM.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-180">Now that Azure AD PIM has reached general availability, trial or paid licenses must be assigned to the administrators of the tenant to continue using PIM.</span></span>  <span data-ttu-id="8a8a0-181">Pokud si vaše organizace není koupit Azure AD Premium P2 nebo vypršení zkušební doby, většinou všechny funkce Azure AD PIM bude k dispozici ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="8a8a0-181">If your organization does not purchase Azure AD Premium P2 or your trial expires, mostly all of the Azure AD PIM features will no longer be available in your tenant.</span></span>  <span data-ttu-id="8a8a0-182">Další informace v [požadavků předplatné Azure AD PIM](./privileged-identity-management/subscription-requirements.md)</span><span class="sxs-lookup"><span data-stu-id="8a8a0-182">You can read more in the [Azure AD PIM subscription requirements](./privileged-identity-management/subscription-requirements.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="8a8a0-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8a8a0-183">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Admin_Overview.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_Settings_w_Approval_Disabled.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png
