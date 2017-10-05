---
title: "Sestava zabezpečení Uživatelé označení příznakem rizika na portálu Azure Active Directory | Dokumentace Microsoftu"
description: "Přečtěte si o sestavě zabezpečení Uživatelé označení příznakem rizika na portálu Azure Active Directory"
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
ms.openlocfilehash: 04f15384a7cd0fa03300acdf159d371569ecf9fc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="users-flagged-for-risk-security-report-in-the-azure-active-directory-portal"></a><span data-ttu-id="8cb0e-103">Sestava zabezpečení Uživatelé označení příznakem rizika na portálu Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8cb0e-103">Users flagged for risk security report in the Azure Active Directory portal</span></span>

<span data-ttu-id="8cb0e-104">Sestavy zabezpečení v Azure Active Directory (Azure AD) umožňují získat přehled o pravděpodobnosti ohrožení uživatelských účtů ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="8cb0e-104">With the security reports in the Azure Active Directory (Azure AD), you can gain insights into the probability of compromised user accounts in your environment.</span></span> 

<span data-ttu-id="8cb0e-105">Azure Active Directory detekuje podezřelé akce, které souvisejí s vašimi uživatelskými účty.</span><span class="sxs-lookup"><span data-stu-id="8cb0e-105">Azure Active Directory detects suspicious actions that are related to your user accounts.</span></span> <span data-ttu-id="8cb0e-106">Pro každou zjištěnou akci se vytvoří záznam nazvaný *riziková událost*.</span><span class="sxs-lookup"><span data-stu-id="8cb0e-106">For each detected action, a record called *risk event* is created.</span></span> <span data-ttu-id="8cb0e-107">Další informace najdete v tématu věnovaném [rizikovým událostem služby Azure Active Directory](active-directory-identity-protection-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="8cb0e-107">For more information, see [Azure Active Directory risk events](active-directory-identity-protection-risk-events.md).</span></span> 

<span data-ttu-id="8cb0e-108">Zjištěné rizikové události se použijí k výpočtu těchto údajů:</span><span class="sxs-lookup"><span data-stu-id="8cb0e-108">The detected risk events are used to calculate:</span></span>

- <span data-ttu-id="8cb0e-109">**Riziková přihlášení** – Rizikové přihlášení je indikátorem pokusu o přihlášení, který mohl provést někdo, kdo není legitimním vlastníkem uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="8cb0e-109">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not the legitimate owner of a user account.</span></span> <span data-ttu-id="8cb0e-110">Další informace najdete v tématu popisujícím [riziková přihlášení](active-directory-identityprotection.md#risky-sign-ins).</span><span class="sxs-lookup"><span data-stu-id="8cb0e-110">For more information, see [Risky sign-ins](active-directory-identityprotection.md#risky-sign-ins).</span></span> 

- <span data-ttu-id="8cb0e-111">**Uživatelé označení příznakem rizika** – Rizikový uživatel je indikátorem uživatelského účtu, který mohl být ohrožený.</span><span class="sxs-lookup"><span data-stu-id="8cb0e-111">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> <span data-ttu-id="8cb0e-112">Další informace najdete v tématu popisujícím [uživatele označené příznakem rizika](active-directory-identityprotection.md#users-flagged-for-risk).</span><span class="sxs-lookup"><span data-stu-id="8cb0e-112">For more information, see [Users flagged for risk](active-directory-identityprotection.md#users-flagged-for-risk).</span></span>  

<span data-ttu-id="8cb0e-113">Na webu Azure Portal najdete sestavy zabezpečení v okně **Azure Active Directory** v části **Zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="8cb0e-113">In the Azure portal, you can find the security reports on the **Azure Active Directory** blade in the **Security** section.</span></span>  

![Riziková přihlášení](./media/active-directory-reporting-security-user-at-risk/10.png)



## <a name="what-azure-ad-license-do-you-need-to-access-a-security-report"></a><span data-ttu-id="8cb0e-115">Jaká licence Azure AD je potřeba pro přístup k sestavě zabezpečení?</span><span class="sxs-lookup"><span data-stu-id="8cb0e-115">What Azure AD license do you need to access a security report?</span></span>  

<span data-ttu-id="8cb0e-116">Sestavy uživatelů označených příznakem rizika nabízí všechny edice Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="8cb0e-116">All editions of Azure Active Directory provide you with users flagged for risk reports.</span></span>  
<span data-ttu-id="8cb0e-117">Úroveň podrobností sestav se však mezi jednotlivými edicemi liší:</span><span class="sxs-lookup"><span data-stu-id="8cb0e-117">However, the level of report granularity varies between the editions:</span></span> 

- <span data-ttu-id="8cb0e-118">**Edice Azure Active Directory Free a Basic** již nabízí seznam uživatelů označených příznakem rizika.</span><span class="sxs-lookup"><span data-stu-id="8cb0e-118">In the **Azure Active Directory Free and Basic editions**, you already get a list of users flagged for risk.</span></span> 

- <span data-ttu-id="8cb0e-119">Edice **Azure Active Directory Premium 1** tento model rozšiřuje tím, že umožňuje také prozkoumávat některé ze základních rizikových událostí, které byly v každé sestavě rozpoznány.</span><span class="sxs-lookup"><span data-stu-id="8cb0e-119">The **Azure Active Directory Premium 1** edition extends this model by also enabling you to examine some of the underlying risk events that have been detected for each report.</span></span> 

- <span data-ttu-id="8cb0e-120">Edice **Azure Active Directory Premium 2** poskytuje nejpodrobnější informace o všech základních rizikových událostech a umožňuje konfigurovat zásady zabezpečení, které automaticky reagují na nakonfigurované úrovně rizika.</span><span class="sxs-lookup"><span data-stu-id="8cb0e-120">The **Azure Active Directory Premium 2** edition provides you with the most detailed information about all underlying risk events and enables you to configure security policies that automatically respond to configured risk levels.</span></span>



## <a name="azure-active-directory-free-and-basic-edition"></a><span data-ttu-id="8cb0e-121">Edice Free a Basic služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8cb0e-121">Azure Active Directory free and basic edition</span></span>

<span data-ttu-id="8cb0e-122">Edice Free a Basic služby Azure Active Directory poskytuje sestavu uživatelů označených příznakem rizika, která obsahuje seznam uživatelských účtů, které můžou být ohrožené.</span><span class="sxs-lookup"><span data-stu-id="8cb0e-122">The users flagged for risk report in the Azure Active Directory free and basic editions provides you with a list of user accounts that may have been compromised.</span></span> 


![Riziková přihlášení](./media/active-directory-reporting-security-user-at-risk/03.png)

<span data-ttu-id="8cb0e-124">Kliknutím na uživatele otevřete okno se souvisejícími uživatelskými daty.</span><span class="sxs-lookup"><span data-stu-id="8cb0e-124">Selecting a user opens the related user data blade.</span></span>
<span data-ttu-id="8cb0e-125">U ohrožených uživatelů můžete zkontrolovat historii jejich přihlášení a v případě potřeby resetovat heslo.</span><span class="sxs-lookup"><span data-stu-id="8cb0e-125">For users that are at risk, you can review the user’s sign-in history and reset the password if necessary.</span></span>

![Riziková přihlášení](./media/active-directory-reporting-security-user-at-risk/46.png)


<span data-ttu-id="8cb0e-127">V tomto dialogovém okně máte možnost:</span><span class="sxs-lookup"><span data-stu-id="8cb0e-127">This dialog provides you with an option to:</span></span>

- <span data-ttu-id="8cb0e-128">Stáhnout sestavu</span><span class="sxs-lookup"><span data-stu-id="8cb0e-128">Download the report</span></span>

- <span data-ttu-id="8cb0e-129">Vyhledávat uživatele</span><span class="sxs-lookup"><span data-stu-id="8cb0e-129">Search users</span></span>

![Riziková přihlášení](./media/active-directory-reporting-security-user-at-risk/16.png)


## <a name="azure-active-directory-premium-editions"></a><span data-ttu-id="8cb0e-131">Edice Premium služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8cb0e-131">Azure Active Directory premium editions</span></span>

<span data-ttu-id="8cb0e-132">Sestava uživatelů označených příznakem rizika v edicích Premium služby Azure Active Directory vám nabízí:</span><span class="sxs-lookup"><span data-stu-id="8cb0e-132">The users flagged for risk report in the Azure Active Directory premium editions provides you with:</span></span>

- <span data-ttu-id="8cb0e-133">[Seznam uživatelských účtů](active-directory-identityprotection.md#users-flagged-for-risk), které mohly být napadeny nebo vyzrazeny</span><span class="sxs-lookup"><span data-stu-id="8cb0e-133">A [list of user accounts](active-directory-identityprotection.md#users-flagged-for-risk) that may have been compromised</span></span> 

- <span data-ttu-id="8cb0e-134">Agregované informace o [typech rizikových událostí](active-directory-identity-protection-risk-events.md), které byly zjištěné</span><span class="sxs-lookup"><span data-stu-id="8cb0e-134">Aggregated information about the [risk event types](active-directory-identity-protection-risk-events.md) that have been detected</span></span>

- <span data-ttu-id="8cb0e-135">Možnost stažení sestavy</span><span class="sxs-lookup"><span data-stu-id="8cb0e-135">An option to download the report</span></span>

- <span data-ttu-id="8cb0e-136">Možnost konfigurace [zásad odstraňování rizik uživatelů](active-directory-identityprotection.md#user-risk-security-policy)</span><span class="sxs-lookup"><span data-stu-id="8cb0e-136">An option to configure a [user risk remediation policy](active-directory-identityprotection.md#user-risk-security-policy)</span></span>  


![Riziková přihlášení](./media/active-directory-reporting-security-user-at-risk/71.png)

<span data-ttu-id="8cb0e-138">Po výběru uživatele získáte podrobné zobrazení sestavy pro tohoto uživatele, které vám umožňuje:</span><span class="sxs-lookup"><span data-stu-id="8cb0e-138">When you select a user, you get a detailed report view for this user that enables you to:</span></span>

- <span data-ttu-id="8cb0e-139">Otevřít zobrazení Všechna přihlášení</span><span class="sxs-lookup"><span data-stu-id="8cb0e-139">Open the All sign-ins view</span></span>

- <span data-ttu-id="8cb0e-140">Resetovat heslo uživatele</span><span class="sxs-lookup"><span data-stu-id="8cb0e-140">Reset the user's password</span></span>

- <span data-ttu-id="8cb0e-141">Zavřít všechny události</span><span class="sxs-lookup"><span data-stu-id="8cb0e-141">Dismiss all events</span></span>

- <span data-ttu-id="8cb0e-142">Vyšetřit rizikové události oznámené pro uživatele</span><span class="sxs-lookup"><span data-stu-id="8cb0e-142">Investigate reported risk events for the user.</span></span> 


![Riziková přihlášení](./media/active-directory-reporting-security-user-at-risk/324.png)


<span data-ttu-id="8cb0e-144">Pokud chcete vyšetřit rizikovou událost, vyberte některou ze seznamu a otevře se okno **Podrobnosti** pro danou rizikovou událost.</span><span class="sxs-lookup"><span data-stu-id="8cb0e-144">To investigate a risk event, select one from the list to open the **Details** blade for this risk event.</span></span> <span data-ttu-id="8cb0e-145">V okně **Podrobnosti** můžete zvolit [ruční zavření rizikové události](active-directory-identityprotection.md#closing-risk-events-manually) nebo reaktivaci ručně zavřené rizikové události.</span><span class="sxs-lookup"><span data-stu-id="8cb0e-145">On the **Details** blade, you have the option to either [manually close a risk event](active-directory-identityprotection.md#closing-risk-events-manually) or reactivate a manually closed risk event.</span></span> 


![Riziková přihlášení](./media/active-directory-reporting-security-user-at-risk/325.png)



## <a name="next-steps"></a><span data-ttu-id="8cb0e-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8cb0e-147">Next steps</span></span>

- <span data-ttu-id="8cb0e-148">Další informace o Azure Active Directory Identity Protection najdete v tématu [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span><span class="sxs-lookup"><span data-stu-id="8cb0e-148">For more information about Azure Active Directory Identity Protection, see [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span></span>

