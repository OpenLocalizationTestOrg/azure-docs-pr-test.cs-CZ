---
title: aaaConfigure Azure AD Privileged Identity managementu | Microsoft Docs
description: "Téma, které vysvětluje, co je Azure AD Privileged Identity managementu a jak toouse PIM tooimprove zabezpečení vašeho cloudu."
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
ms.openlocfilehash: dbe49fe4a0f6e5b46ed5a17fc7e8dcdacafe3846
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-ad-privileged-identity-management"></a><span data-ttu-id="deddc-103">Co je Azure AD Privileged Identity Management?</span><span class="sxs-lookup"><span data-stu-id="deddc-103">What is Azure AD Privileged Identity Management?</span></span>
<span data-ttu-id="deddc-104">Pomocí aplikace Azure Active Directory (AD) Privileged Identity Management můžete spravovat, řídit a sledovat přístup v rámci organizace.</span><span class="sxs-lookup"><span data-stu-id="deddc-104">With Azure Active Directory (AD) Privileged Identity Management, you can manage, control, and monitor access within your organization.</span></span> <span data-ttu-id="deddc-105">To zahrnuje přístup tooresources ve službě Azure AD a dalších služeb Microsoft online services jako je Office 365 nebo Microsoft Intune.</span><span class="sxs-lookup"><span data-stu-id="deddc-105">This includes access tooresources in Azure AD and other Microsoft online services like Office 365 or Microsoft Intune.</span></span>  

> [!NOTE]
> <span data-ttu-id="deddc-106">Privileged Identity Management je k dispozici tooyour celé organizace, když licence správce s hello Premium P2 edice služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="deddc-106">Privileged Identity Management is available tooyour entire organization when you license your Administrators with hello Premium P2 edition of Azure Active Directory.</span></span> <span data-ttu-id="deddc-107">Další informace najdete v článku [Edice služby Azure Active Directory](active-directory-editions.md).</span><span class="sxs-lookup"><span data-stu-id="deddc-107">For more information, see [Azure Active Directory editions](active-directory-editions.md).</span></span>

<span data-ttu-id="deddc-108">Organizace má toominimize hello počet lidem, kteří mají přístup k informacím toosecure nebo prostředky, protože který snižuje pravděpodobnost, že uživatel se zlými úmysly získávání tento přístup hello.</span><span class="sxs-lookup"><span data-stu-id="deddc-108">Organizations want toominimize hello number of people who have access toosecure information or resources, because that reduces hello chance of a malicious user getting that access.</span></span> <span data-ttu-id="deddc-109">Uživatelé musí však stále toocarry out privilegované operace v Azure, Office 365 nebo SaaS aplikace.</span><span class="sxs-lookup"><span data-stu-id="deddc-109">However, users still need toocarry out privileged operations in Azure, Office 365, or SaaS apps.</span></span> <span data-ttu-id="deddc-110">Organizace poskytnout privilegovaného přístupu uživatelů ve službě Azure AD bez monitorování tyto činnosti uživatelů s jejich oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="deddc-110">Organizations give users privileged access in Azure AD without monitoring what those users are doing with their admin privileges.</span></span> <span data-ttu-id="deddc-111">Azure AD Privileged Identity Management pomáhá tooresolve toto riziko.</span><span class="sxs-lookup"><span data-stu-id="deddc-111">Azure AD Privileged Identity Management helps tooresolve this risk.</span></span>  

<span data-ttu-id="deddc-112">Azure AD Privileged Identity Management vám pomůže:</span><span class="sxs-lookup"><span data-stu-id="deddc-112">Azure AD Privileged Identity Management helps you:</span></span>  

* <span data-ttu-id="deddc-113">Uvidíte, kteří uživatelé jsou správci služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="deddc-113">See which users are Azure AD administrators</span></span>
* <span data-ttu-id="deddc-114">Povolit na vyžádání "právě v čase" tooMicrosoft přístup pro správu Online služeb, jako třeba Office 365 a Intune</span><span class="sxs-lookup"><span data-stu-id="deddc-114">Enable on-demand, "just in time" administrative access tooMicrosoft Online Services like Office 365 and Intune</span></span>
* <span data-ttu-id="deddc-115">Získání sestavy o historii přístup správce a změny v přiřazení správců</span><span class="sxs-lookup"><span data-stu-id="deddc-115">Get reports about administrator access history and changes in administrator assignments</span></span>
* <span data-ttu-id="deddc-116">Dostávat upozornění na přístup tooa privilegované role</span><span class="sxs-lookup"><span data-stu-id="deddc-116">Get alerts about access tooa privileged role</span></span>
* <span data-ttu-id="deddc-117">Vyžadovat schválení tooactivate (Preview)</span><span class="sxs-lookup"><span data-stu-id="deddc-117">Require approval tooactivate (Preview)</span></span>

<span data-ttu-id="deddc-118">Azure AD Privileged Identity Management může spravovat organizační role hello předdefinované Azure AD, včetně (ale bez omezení):</span><span class="sxs-lookup"><span data-stu-id="deddc-118">Azure AD Privileged Identity Management can manage hello built-in Azure AD organizational roles, including (but not limited to):</span></span>  

* <span data-ttu-id="deddc-119">Globální správce</span><span class="sxs-lookup"><span data-stu-id="deddc-119">Global Administrator</span></span>
* <span data-ttu-id="deddc-120">Správce fakturace</span><span class="sxs-lookup"><span data-stu-id="deddc-120">Billing Administrator</span></span>
* <span data-ttu-id="deddc-121">Správce služeb</span><span class="sxs-lookup"><span data-stu-id="deddc-121">Service Administrator</span></span>  
* <span data-ttu-id="deddc-122">Správce uživatele</span><span class="sxs-lookup"><span data-stu-id="deddc-122">User Administrator</span></span>
* <span data-ttu-id="deddc-123">Správce hesel</span><span class="sxs-lookup"><span data-stu-id="deddc-123">Password Administrator</span></span>

## <a name="just-in-time-administrator-access"></a><span data-ttu-id="deddc-124">Právě v přístup správce čas</span><span class="sxs-lookup"><span data-stu-id="deddc-124">Just in time administrator access</span></span>
<span data-ttu-id="deddc-125">Je v minulosti přiřadit role správce uživatele tooan prostřednictvím hello portál Azure classic nebo prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="deddc-125">Historically, you could assign a user tooan admin role through hello Azure classic portal or Windows PowerShell.</span></span> <span data-ttu-id="deddc-126">V důsledku toho stane **trvalé správce**, vždy aktivní hello přiřadit role.</span><span class="sxs-lookup"><span data-stu-id="deddc-126">As a result, that user becomes a **permanent admin**, always active in hello assigned role.</span></span> <span data-ttu-id="deddc-127">Azure AD Privileged Identity Management zavádí koncepci hello **oprávněné správce**. Oprávněné admins by měl být uživatelům, kteří potřebují privilegovaného přístupu a poté, ale ne každý den.</span><span class="sxs-lookup"><span data-stu-id="deddc-127">Azure AD Privileged Identity Management introduces hello concept of an **eligible admin**. Eligible admins should be users that need privileged access now and then, but not every day.</span></span> <span data-ttu-id="deddc-128">Hello role je neaktivní, dokud hello uživatel potřebuje přístup, pak se dokončit proces aktivace a stane aktivní správce po předem určenou dobu.</span><span class="sxs-lookup"><span data-stu-id="deddc-128">hello role is inactive until hello user needs access, then they complete an activation process and become an active admin for a predetermined amount of time.</span></span>

## <a name="enable-privileged-identity-management-for-your-directory"></a><span data-ttu-id="deddc-129">Povolit Privileged Identity Management pro svůj adresář</span><span class="sxs-lookup"><span data-stu-id="deddc-129">Enable Privileged Identity Management for your directory</span></span>
<span data-ttu-id="deddc-130">Můžete začít používat Azure AD Privileged Identity Management v hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="deddc-130">You can start using Azure AD Privileged Identity Management in hello [Azure portal](https://portal.azure.com/).</span></span>

> [!NOTE]
> <span data-ttu-id="deddc-131">Musí být globální správce s účtem organizace (například @yourdomain.com), nikoli účet Microsoft (například @outlook.com), tooenable pro adresář Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="deddc-131">You must be a global administrator with an organizational account (for example, @yourdomain.com), not a Microsoft account (for example, @outlook.com), tooenable Azure AD Privileged Identity Management for a directory.</span></span>

1. <span data-ttu-id="deddc-132">Přihlaste se toohello [portál Azure](https://portal.azure.com/) jako globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="deddc-132">Sign in toohello [Azure portal](https://portal.azure.com/) as a global administrator of your directory.</span></span>
2. <span data-ttu-id="deddc-133">Pokud má vaše organizace více než jeden adresář, vyberte své uživatelské jméno v hello pravém horním rohu hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="deddc-133">If your organization has more than one directory, select your username in hello upper right-hand corner of hello Azure portal.</span></span> <span data-ttu-id="deddc-134">Vyberte adresář hello, kde budete používat Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="deddc-134">Select hello directory where you will use Azure AD Privileged Identity Management.</span></span>
3. <span data-ttu-id="deddc-135">Vyberte **další služby** a používat hello filtru textbox toosearch pro **Azure AD Privileged Identity Management**.</span><span class="sxs-lookup"><span data-stu-id="deddc-135">Select **More services** and use hello Filter textbox toosearch for **Azure AD Privileged Identity Management**.</span></span>
4. <span data-ttu-id="deddc-136">Zkontrolujte **Pin toodashboard** a pak klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="deddc-136">Check **Pin toodashboard** and then click **Create**.</span></span> <span data-ttu-id="deddc-137">Otevře se Hello aplikace Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="deddc-137">hello Privileged Identity Management application opens.</span></span>

<span data-ttu-id="deddc-138">Pokud jste hello první, kdo toouse Azure AD Privileged Identity Management ve vašem adresáři, pak hello [Průvodce zabezpečení](active-directory-privileged-identity-management-security-wizard.md) vás provede počátečním přiřazením hello.</span><span class="sxs-lookup"><span data-stu-id="deddc-138">If you're hello first person toouse Azure AD Privileged Identity Management in your directory, then hello [security wizard](active-directory-privileged-identity-management-security-wizard.md) walks you through hello initial assignment experience.</span></span> <span data-ttu-id="deddc-139">Poté můžete automaticky stane hello nejprve **správce zabezpečení** a **správce privilegovaných rolí** hello adresáře.</span><span class="sxs-lookup"><span data-stu-id="deddc-139">After that you automatically become hello first **Security administrator** and **Privileged role administrator** of hello directory.</span></span>

<span data-ttu-id="deddc-140">Pouze správce privilegovaných rolí můžete spravovat přístup pro jiné správce.</span><span class="sxs-lookup"><span data-stu-id="deddc-140">Only a privileged role administrator can manage access for other administrators.</span></span> <span data-ttu-id="deddc-141">Můžete [poskytnout další uživatelé hello možnost toomanage v PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span><span class="sxs-lookup"><span data-stu-id="deddc-141">You can [give other users hello ability toomanage in PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span></span>

## <a name="privileged-identity-management-admin-dashboard"></a><span data-ttu-id="deddc-142">Privilegované řídicí panel Správce Identity Management</span><span class="sxs-lookup"><span data-stu-id="deddc-142">Privileged Identity Management admin dashboard</span></span>
<span data-ttu-id="deddc-143">Azure AD Privileged Identity Manager poskytuje správce řídicího panelu, který poskytuje důležité informace, jako:</span><span class="sxs-lookup"><span data-stu-id="deddc-143">Azure AD Privileged Identity Manager provides an admin dashboard that gives you important information such as:</span></span>

* <span data-ttu-id="deddc-144">Výstrahy, které odkazují na příležitosti tooimprove zabezpečení</span><span class="sxs-lookup"><span data-stu-id="deddc-144">Alerts that point out opportunities tooimprove security</span></span>
* <span data-ttu-id="deddc-145">Hello počet uživatelů, kteří jsou přiřazeni tooeach privilegované role</span><span class="sxs-lookup"><span data-stu-id="deddc-145">hello number of users who are assigned tooeach privileged role</span></span>  
* <span data-ttu-id="deddc-146">Hello počet oprávněných a trvalých správců</span><span class="sxs-lookup"><span data-stu-id="deddc-146">hello number of eligible and permanent admins</span></span>
* <span data-ttu-id="deddc-147">Graf aktivací privilegované role ve vašem adresáři</span><span class="sxs-lookup"><span data-stu-id="deddc-147">A graph of privileged role activations in your directory</span></span>

![Řídicí panel PIM – snímek obrazovky][2]

## <a name="privileged-role-management"></a><span data-ttu-id="deddc-149">Správa privilegovaných rolí</span><span class="sxs-lookup"><span data-stu-id="deddc-149">Privileged role management</span></span>
<span data-ttu-id="deddc-150">S Azure AD Privileged Identity Management můžete spravovat správce hello přidáním nebo odebráním rolí tooeach trvalé nebo oprávněné správce.</span><span class="sxs-lookup"><span data-stu-id="deddc-150">With Azure AD Privileged Identity Management, you can manage hello administrators by adding or removing permanent or eligible administrators tooeach role.</span></span>

![Správci přidat nebo odebrat PIM – snímek obrazovky][3]

## <a name="configure-hello-role-activation-settings"></a><span data-ttu-id="deddc-152">Konfigurace nastavení aktivace role hello</span><span class="sxs-lookup"><span data-stu-id="deddc-152">Configure hello role activation settings</span></span>
<span data-ttu-id="deddc-153">Pomocí hello [nastavení role](active-directory-privileged-identity-management-how-to-change-default-settings.md) můžete nakonfigurovat vlastnosti aktivace vhodné role hello včetně:</span><span class="sxs-lookup"><span data-stu-id="deddc-153">Using hello [role settings](active-directory-privileged-identity-management-how-to-change-default-settings.md) you can configure hello eligible role activation properties including:</span></span>

* <span data-ttu-id="deddc-154">Hello trvání hello období aktivace role</span><span class="sxs-lookup"><span data-stu-id="deddc-154">hello duration of hello role activation period</span></span>
* <span data-ttu-id="deddc-155">oznámení o aktivaci role Hello</span><span class="sxs-lookup"><span data-stu-id="deddc-155">hello role activation notification</span></span>
* <span data-ttu-id="deddc-156">Hello informace, které uživatel potřebuje tooprovide během procesu aktivace role hello</span><span class="sxs-lookup"><span data-stu-id="deddc-156">hello information a user needs tooprovide during hello role activation process</span></span>
* <span data-ttu-id="deddc-157">Číslo služby lístků nebo incidenty.</span><span class="sxs-lookup"><span data-stu-id="deddc-157">Service ticket or incident number</span></span>
* [<span data-ttu-id="deddc-158">Požadavky na pracovní postup schválení – náhled</span><span class="sxs-lookup"><span data-stu-id="deddc-158">Approval workflow requirements - Preview</span></span>](./privileged-identity-management/azure-ad-pim-approval-workflow.md)

![Snímek obrazovky nastavení – správce aktivace – PIM][4]

<span data-ttu-id="deddc-160">Všimněte si, že obrázek hello hello tlačítka pro **Multi-Factor Authentication** jsou zakázány.</span><span class="sxs-lookup"><span data-stu-id="deddc-160">Note that in hello image, hello buttons for **Multi-Factor Authentication** are disabled.</span></span> <span data-ttu-id="deddc-161">Pro některé, vysoce privilegované role, jsme vícefaktorové ověřování vyžadovat pro zvýšenou ochranu.</span><span class="sxs-lookup"><span data-stu-id="deddc-161">For certain, highly privileged roles, we require MFA for heightened protection.</span></span>

## <a name="role-activation"></a><span data-ttu-id="deddc-162">Aktivace role</span><span class="sxs-lookup"><span data-stu-id="deddc-162">Role activation</span></span>
<span data-ttu-id="deddc-163">příliš[aktivovat roli](active-directory-privileged-identity-management-how-to-activate-role.md), požadavky časově vázaných "aktivace" pro roli hello oprávněné správce.</span><span class="sxs-lookup"><span data-stu-id="deddc-163">too[activate a role](active-directory-privileged-identity-management-how-to-activate-role.md), an eligible admin requests a time-bound "activation" for hello role.</span></span> <span data-ttu-id="deddc-164">Hello aktivace může být požadováno pomocí hello **aktivovat Moje role** možnost v Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="deddc-164">hello activation can be requested using hello **Activate my role** option in Azure AD Privileged Identity Management.</span></span>

<span data-ttu-id="deddc-165">Kdo chce tooactivate roli správce musí tooinitialize Azure AD Privileged Identity Management hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="deddc-165">An admin who wants tooactivate a role needs tooinitialize Azure AD Privileged Identity Management in hello Azure portal.</span></span>

<span data-ttu-id="deddc-166">Aktivaci role je přizpůsobitelná.</span><span class="sxs-lookup"><span data-stu-id="deddc-166">Role activation is customizable.</span></span> <span data-ttu-id="deddc-167">V nastavení PIM hello můžete určit hello délka hello aktivace a jaké informace Dobrý den, Správce musí tooprovide tooactivate hello role.</span><span class="sxs-lookup"><span data-stu-id="deddc-167">In hello PIM settings, you can determine hello length of hello activation and what information hello admin needs tooprovide tooactivate hello role.</span></span>

![Aktivace role PIM správce žádost – snímek obrazovky][5]

## <a name="review-role-activity"></a><span data-ttu-id="deddc-169">Kontrolní aktivita role</span><span class="sxs-lookup"><span data-stu-id="deddc-169">Review role activity</span></span>
<span data-ttu-id="deddc-170">Existují dva způsoby tootrack jak zaměstnanci a správci používají privilegované role.</span><span class="sxs-lookup"><span data-stu-id="deddc-170">There are two ways tootrack how your employees and admins are using privileged roles.</span></span> <span data-ttu-id="deddc-171">první možnost Hello používá [historie auditu role Directory](active-directory-privileged-identity-management-how-to-use-audit-log.md).</span><span class="sxs-lookup"><span data-stu-id="deddc-171">hello first option is using [Directory Roles audit history](active-directory-privileged-identity-management-how-to-use-audit-log.md).</span></span> <span data-ttu-id="deddc-172">historie auditu Hello zaznamená sledovat změny v přiřazení privilegované role a role aktivace historie.</span><span class="sxs-lookup"><span data-stu-id="deddc-172">hello audit history logs track changes in privileged role assignments and role activation history.</span></span>

![Historie aktivace PIM – snímek obrazovky][6]

<span data-ttu-id="deddc-174">Hello druhou možností je tooset až regular [přístup recenze](active-directory-privileged-identity-management-how-to-start-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="deddc-174">hello second option is tooset up regular [access reviews](active-directory-privileged-identity-management-how-to-start-security-review.md).</span></span> <span data-ttu-id="deddc-175">Tyto recenze přístup může být provádí a přiřazení kontrolor (jako je team manager) nebo hello zaměstnanci můžete zkontrolovat sami.</span><span class="sxs-lookup"><span data-stu-id="deddc-175">These access reviews can be performed by and assigned reviewer (like a team manager) or hello employees can review themselves.</span></span> <span data-ttu-id="deddc-176">Toto je hello nejlepší způsob, jak toomonitor kdo stále vyžaduje přístup a kteří už nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="deddc-176">This is hello best way toomonitor who still requires access, and who no longer does.</span></span>

## <a name="azure-ad-pim-at-subscription-expiration"></a><span data-ttu-id="deddc-177">Azure AD PIM na vypršení platnosti odběru</span><span class="sxs-lookup"><span data-stu-id="deddc-177">Azure AD PIM at subscription expiration</span></span>
<span data-ttu-id="deddc-178">Předchozí tooreaching obecné dostupnosti Azure AD PIM byl ve verzi preview a nebyly k dispozici žádná licence zkontroluje toopreview klienta Azure AD PIM.</span><span class="sxs-lookup"><span data-stu-id="deddc-178">Prior tooreaching general availability Azure AD PIM was in preview and there were no license checks for a tenant toopreview Azure AD PIM.</span></span>  <span data-ttu-id="deddc-179">Teď, když Azure AD PIM byl dosažen obecné dostupnosti, musí být přiřazeni správci toohello toocontinue hello klienta pomocí PIM zkušební nebo placené licence.</span><span class="sxs-lookup"><span data-stu-id="deddc-179">Now that Azure AD PIM has reached general availability, trial or paid licenses must be assigned toohello administrators of hello tenant toocontinue using PIM.</span></span>  <span data-ttu-id="deddc-180">Pokud si vaše organizace není koupit Azure AD Premium P2 nebo vypršení zkušební doby, většinou všechny funkce Azure AD PIM hello bude k dispozici ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="deddc-180">If your organization does not purchase Azure AD Premium P2 or your trial expires, mostly all of hello Azure AD PIM features will no longer be available in your tenant.</span></span>  <span data-ttu-id="deddc-181">Další informace v hello [požadavků předplatné Azure AD PIM](./privileged-identity-management/subscription-requirements.md)</span><span class="sxs-lookup"><span data-stu-id="deddc-181">You can read more in hello [Azure AD PIM subscription requirements](./privileged-identity-management/subscription-requirements.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="deddc-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="deddc-182">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Admin_Overview.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_Settings_w_Approval_Disabled.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png
