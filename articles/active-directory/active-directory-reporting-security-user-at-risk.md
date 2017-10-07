---
title: "aaaUsers příznakem pro sestavu riziko zabezpečení Azure Active Directory portálu hello | Microsoft Docs"
description: "Další informace o hello uživatelé označení příznakem pro sestavu riziko zabezpečení Azure Active Directory portálu hello"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 5077cd61d6119745a85ed712623904633a151331
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="users-flagged-for-risk-security-report-in-hello-azure-active-directory-portal"></a><span data-ttu-id="ae103-103">Uživatelé označení příznakem pro sestavu riziko zabezpečení Azure Active Directory portálu hello</span><span class="sxs-lookup"><span data-stu-id="ae103-103">Users flagged for risk security report in hello Azure Active Directory portal</span></span>

<span data-ttu-id="ae103-104">Pomocí sestavy zabezpečení hello v hello Azure Active Directory (Azure AD) je možné získat přehled o hello pravděpodobnost ohroženými uživatelskými účty ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="ae103-104">With hello security reports in hello Azure Active Directory (Azure AD), you can gain insights into hello probability of compromised user accounts in your environment.</span></span> 

<span data-ttu-id="ae103-105">Azure Active Directory zjistí podezřelou akce, které jsou související tooyour uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="ae103-105">Azure Active Directory detects suspicious actions that are related tooyour user accounts.</span></span> <span data-ttu-id="ae103-106">Pro každou zjištěnou akci se vytvoří záznam nazvaný *riziková událost*.</span><span class="sxs-lookup"><span data-stu-id="ae103-106">For each detected action, a record called *risk event* is created.</span></span> <span data-ttu-id="ae103-107">Další informace najdete v tématu věnovaném [rizikovým událostem služby Azure Active Directory](active-directory-identity-protection-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="ae103-107">For more information, see [Azure Active Directory risk events](active-directory-identity-protection-risk-events.md).</span></span> 

<span data-ttu-id="ae103-108">Hello zjistila rizikových událostí jsou použité toocalculate:</span><span class="sxs-lookup"><span data-stu-id="ae103-108">hello detected risk events are used toocalculate:</span></span>

- <span data-ttu-id="ae103-109">**Rizikové přihlášení** -rizikové přihlášení je indikátorem pro pokusu přihlášení, který se možná prováděly uživatelem, který není vlastníkem legitimní hello uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="ae103-109">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not hello legitimate owner of a user account.</span></span> <span data-ttu-id="ae103-110">Další informace najdete v tématu popisujícím [riziková přihlášení](active-directory-identityprotection.md#risky-sign-ins).</span><span class="sxs-lookup"><span data-stu-id="ae103-110">For more information, see [Risky sign-ins](active-directory-identityprotection.md#risky-sign-ins).</span></span> 

- <span data-ttu-id="ae103-111">**Uživatelé označení příznakem rizika** – Rizikový uživatel je indikátorem uživatelského účtu, který mohl být ohrožený.</span><span class="sxs-lookup"><span data-stu-id="ae103-111">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> <span data-ttu-id="ae103-112">Další informace najdete v tématu popisujícím [uživatele označené příznakem rizika](active-directory-identityprotection.md#users-flagged-for-risk).</span><span class="sxs-lookup"><span data-stu-id="ae103-112">For more information, see [Users flagged for risk](active-directory-identityprotection.md#users-flagged-for-risk).</span></span>  

<span data-ttu-id="ae103-113">V hello portálu Azure, můžete najít hello zabezpečení hlásí hello **Azure Active Directory** okno v hello **zabezpečení** části.</span><span class="sxs-lookup"><span data-stu-id="ae103-113">In hello Azure portal, you can find hello security reports on hello **Azure Active Directory** blade in hello **Security** section.</span></span>  

![Riziková přihlášení](./media/active-directory-reporting-security-user-at-risk/10.png)



## <a name="what-azure-ad-license-do-you-need-tooaccess-a-security-report"></a><span data-ttu-id="ae103-115">Jaké licence Azure AD potřebujete tooaccess sestavy zabezpečení?</span><span class="sxs-lookup"><span data-stu-id="ae103-115">What Azure AD license do you need tooaccess a security report?</span></span>  

<span data-ttu-id="ae103-116">Sestavy uživatelů označených příznakem rizika nabízí všechny edice Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ae103-116">All editions of Azure Active Directory provide you with users flagged for risk reports.</span></span>  
<span data-ttu-id="ae103-117">Ale hello úroveň členitost liší hello edice:</span><span class="sxs-lookup"><span data-stu-id="ae103-117">However, hello level of report granularity varies between hello editions:</span></span> 

- <span data-ttu-id="ae103-118">V hello **edice Basic a Azure Active Directory volné**, již získat seznam uživatelů, které jsou označeny k riziko.</span><span class="sxs-lookup"><span data-stu-id="ae103-118">In hello **Azure Active Directory Free and Basic editions**, you already get a list of users flagged for risk.</span></span> 

- <span data-ttu-id="ae103-119">Hello **Azure Active Directory Premium 1** edice rozšiřuje tohoto modelu tím, že také umožňuje tooexamine, některé základní riziko události, které pro každou sestavu nebyla zjištěna hello.</span><span class="sxs-lookup"><span data-stu-id="ae103-119">hello **Azure Active Directory Premium 1** edition extends this model by also enabling you tooexamine some of hello underlying risk events that have been detected for each report.</span></span> 

- <span data-ttu-id="ae103-120">Hello **Azure Active Directory Premium 2** edice nabízí hello nejvíce podrobné informace o všech základní rizikových událostech a umožní vám tooconfigure zásady zabezpečení, které automaticky odpovídat tooconfigured rizik úrovně.</span><span class="sxs-lookup"><span data-stu-id="ae103-120">hello **Azure Active Directory Premium 2** edition provides you with hello most detailed information about all underlying risk events and enables you tooconfigure security policies that automatically respond tooconfigured risk levels.</span></span>



## <a name="azure-active-directory-free-and-basic-edition"></a><span data-ttu-id="ae103-121">Edice Free a Basic služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ae103-121">Azure Active Directory free and basic edition</span></span>

<span data-ttu-id="ae103-122">Hello uživatelé označení příznakem pro sestavu riziko v edicích volné a basic služby Azure Active Directory hello vám poskytne seznam uživatelské účty, které může být ohrožena.</span><span class="sxs-lookup"><span data-stu-id="ae103-122">hello users flagged for risk report in hello Azure Active Directory free and basic editions provides you with a list of user accounts that may have been compromised.</span></span> 


![Riziková přihlášení](./media/active-directory-reporting-security-user-at-risk/03.png)

<span data-ttu-id="ae103-124">Výběr uživatele, otevře se okno data související uživatelské hello.</span><span class="sxs-lookup"><span data-stu-id="ae103-124">Selecting a user opens hello related user data blade.</span></span>
<span data-ttu-id="ae103-125">Pro uživatele, kteří jsou v ohrožení můžete zkontrolovat historie přihlašování hello uživatelů a resetování hesla hello v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="ae103-125">For users that are at risk, you can review hello user’s sign-in history and reset hello password if necessary.</span></span>

![Riziková přihlášení](./media/active-directory-reporting-security-user-at-risk/46.png)


<span data-ttu-id="ae103-127">V tomto dialogovém okně máte možnost:</span><span class="sxs-lookup"><span data-stu-id="ae103-127">This dialog provides you with an option to:</span></span>

- <span data-ttu-id="ae103-128">Stažení sestavy hello</span><span class="sxs-lookup"><span data-stu-id="ae103-128">Download hello report</span></span>

- <span data-ttu-id="ae103-129">Vyhledávat uživatele</span><span class="sxs-lookup"><span data-stu-id="ae103-129">Search users</span></span>

![Riziková přihlášení](./media/active-directory-reporting-security-user-at-risk/16.png)


## <a name="azure-active-directory-premium-editions"></a><span data-ttu-id="ae103-131">Edice Premium služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ae103-131">Azure Active Directory premium editions</span></span>

<span data-ttu-id="ae103-132">Hello uživatelé označení příznakem pro sestavu riziko v edicích hello Azure Active Directory premium nabízí:</span><span class="sxs-lookup"><span data-stu-id="ae103-132">hello users flagged for risk report in hello Azure Active Directory premium editions provides you with:</span></span>

- <span data-ttu-id="ae103-133">[Seznam uživatelských účtů](active-directory-identityprotection.md#users-flagged-for-risk), které mohly být napadeny nebo vyzrazeny</span><span class="sxs-lookup"><span data-stu-id="ae103-133">A [list of user accounts](active-directory-identityprotection.md#users-flagged-for-risk) that may have been compromised</span></span> 

- <span data-ttu-id="ae103-134">Agregovat informace o hello [rizik typů událostí](active-directory-identity-protection-risk-events.md) , o kterých bylo zjištěno</span><span class="sxs-lookup"><span data-stu-id="ae103-134">Aggregated information about hello [risk event types](active-directory-identity-protection-risk-events.md) that have been detected</span></span>

- <span data-ttu-id="ae103-135">Zprávu o možnost toodownload hello</span><span class="sxs-lookup"><span data-stu-id="ae103-135">An option toodownload hello report</span></span>

- <span data-ttu-id="ae103-136">Možnost tooconfigure [uživatele riziko nápravy zásad](active-directory-identityprotection.md#user-risk-security-policy)</span><span class="sxs-lookup"><span data-stu-id="ae103-136">An option tooconfigure a [user risk remediation policy](active-directory-identityprotection.md#user-risk-security-policy)</span></span>  


![Riziková přihlášení](./media/active-directory-reporting-security-user-at-risk/71.png)

<span data-ttu-id="ae103-138">Po výběru uživatele získáte podrobné zobrazení sestavy pro tohoto uživatele, které vám umožňuje:</span><span class="sxs-lookup"><span data-stu-id="ae103-138">When you select a user, you get a detailed report view for this user that enables you to:</span></span>

- <span data-ttu-id="ae103-139">Zobrazit všechny přihlášení otevřete hello</span><span class="sxs-lookup"><span data-stu-id="ae103-139">Open hello All sign-ins view</span></span>

- <span data-ttu-id="ae103-140">Resetování hesla hello uživatele</span><span class="sxs-lookup"><span data-stu-id="ae103-140">Reset hello user's password</span></span>

- <span data-ttu-id="ae103-141">Zavřít všechny události</span><span class="sxs-lookup"><span data-stu-id="ae103-141">Dismiss all events</span></span>

- <span data-ttu-id="ae103-142">Prozkoumejte hlášené riziko události pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="ae103-142">Investigate reported risk events for hello user.</span></span> 


![Riziková přihlášení](./media/active-directory-reporting-security-user-at-risk/324.png)


<span data-ttu-id="ae103-144">tooinvestigate riziko události, vyberte jednu z hello seznamu tooopen hello **podrobnosti** okno pro tuto událost riziko.</span><span class="sxs-lookup"><span data-stu-id="ae103-144">tooinvestigate a risk event, select one from hello list tooopen hello **Details** blade for this risk event.</span></span> <span data-ttu-id="ae103-145">Na hello **podrobnosti** okno, máte možnost tooeither hello [ruční zavření riziko událostí](active-directory-identityprotection.md#closing-risk-events-manually) nebo znovu aktivovat ručně uzavřené riziko událostí.</span><span class="sxs-lookup"><span data-stu-id="ae103-145">On hello **Details** blade, you have hello option tooeither [manually close a risk event](active-directory-identityprotection.md#closing-risk-events-manually) or reactivate a manually closed risk event.</span></span> 


![Riziková přihlášení](./media/active-directory-reporting-security-user-at-risk/325.png)



## <a name="next-steps"></a><span data-ttu-id="ae103-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ae103-147">Next steps</span></span>

- <span data-ttu-id="ae103-148">Další informace o Azure Active Directory Identity Protection najdete v tématu [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span><span class="sxs-lookup"><span data-stu-id="ae103-148">For more information about Azure Active Directory Identity Protection, see [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span></span>

