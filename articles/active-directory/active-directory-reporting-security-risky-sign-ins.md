---
title: "Sestavy rizikových přihlášení na portálu Azure Active Directory | Dokumentace Microsoftu"
description: "Informace o sestavách rizikových přihlášení na portálu Azure Active Directory"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: 7728fcd7-3dd5-4b99-a0e4-949c69788c0f
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 45a6f63bd920c9a70c25b8dfae084ea030256cf4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="risky-sign-ins-report-in-the-azure-active-directory-portal"></a><span data-ttu-id="0b456-103">Sestavy rizikových přihlášení na portálu Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0b456-103">Risky sign-ins report in the Azure Active Directory portal</span></span>

<span data-ttu-id="0b456-104">Sestavy zabezpečení v Azure Active Directory (Azure AD) umožňují získat přehled o pravděpodobnosti ohrožení uživatelských účtů ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="0b456-104">With the security reports in Azure Active Directory (Azure AD) you can gain insights into the probability of compromised user accounts in your environment.</span></span> 

<span data-ttu-id="0b456-105">Azure AD detekuje podezřelé akce, které souvisejí s vašimi uživatelskými účty.</span><span class="sxs-lookup"><span data-stu-id="0b456-105">Azure AD detects suspicious actions that are related to your user accounts.</span></span> <span data-ttu-id="0b456-106">Pro každou zjištěnou akci se vytvoří záznam nazvaný *riziková událost*.</span><span class="sxs-lookup"><span data-stu-id="0b456-106">For each detected action, a record called *risk event* is created.</span></span> <span data-ttu-id="0b456-107">Další podrobnosti najdete v tématu věnovaném [rizikovým událostem služby Azure Active Directory](active-directory-identity-protection-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="0b456-107">For more details, see [Azure Active Directory risk events](active-directory-identity-protection-risk-events.md).</span></span> 

<span data-ttu-id="0b456-108">Zjištěné rizikové události se použijí k výpočtu těchto údajů:</span><span class="sxs-lookup"><span data-stu-id="0b456-108">The detected risk events are used to calculate:</span></span>

- <span data-ttu-id="0b456-109">**Riziková přihlášení** – Rizikové přihlášení je indikátorem pokusu o přihlášení, který mohl provést někdo, kdo není legitimním vlastníkem uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="0b456-109">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not the legitimate owner of a user account.</span></span> <span data-ttu-id="0b456-110">Další podrobnosti najdete v tématu [Riziková přihlášení](active-directory-identityprotection.md#risky-sign-ins).</span><span class="sxs-lookup"><span data-stu-id="0b456-110">For more details, see [Risky sign-ins](active-directory-identityprotection.md#risky-sign-ins).</span></span> 

- <span data-ttu-id="0b456-111">**Uživatelé označení příznakem rizika** – Rizikový uživatel je indikátorem uživatelského účtu, který mohl být ohrožený.</span><span class="sxs-lookup"><span data-stu-id="0b456-111">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> <span data-ttu-id="0b456-112">Další podrobnosti najdete v tématu [Uživatelé označení příznakem rizika](active-directory-identityprotection.md#users-flagged-for-risk).</span><span class="sxs-lookup"><span data-stu-id="0b456-112">For more details, see [Users flagged for risk](active-directory-identityprotection.md#users-flagged-for-risk).</span></span>  

<span data-ttu-id="0b456-113">Na webu [Azure Portal](https://portal.azure.com) najdete sestavy zabezpečení v okně **Azure Active Directory** v části **Zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="0b456-113">In [the Azure portal](https://portal.azure.com), you can find the security reports on the **Azure Active Directory** blade in the **Security** section.</span></span> 

![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/10.png)


## <a name="what-azure-ad-license-do-you-need-to-access-a-security-report"></a><span data-ttu-id="0b456-115">Jaká licence Azure AD je potřeba pro přístup k sestavě zabezpečení?</span><span class="sxs-lookup"><span data-stu-id="0b456-115">What Azure AD license do you need to access a security report?</span></span>  

<span data-ttu-id="0b456-116">Sestavy rizikových přihlášení nabízí všechny edice Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0b456-116">All editions of Azure Active Directory provide you with risky sign-ins reports.</span></span>  
<span data-ttu-id="0b456-117">Úroveň podrobností sestav se však mezi jednotlivými edicemi liší:</span><span class="sxs-lookup"><span data-stu-id="0b456-117">However, the level of report granularity varies between the editions:</span></span> 

- <span data-ttu-id="0b456-118">**Edice Azure Active Directory Free a Basic** již nabízí seznam rizikových přihlášení.</span><span class="sxs-lookup"><span data-stu-id="0b456-118">In the **Azure Active Directory Free and Basic editions**, you already get a list of risky sign-ins.</span></span> 

- <span data-ttu-id="0b456-119">Edice **Azure Active Directory Premium 1** tento model rozšiřuje tím, že umožňuje také prozkoumávat některé ze základních rizikových událostí, které byly v každé sestavě rozpoznány.</span><span class="sxs-lookup"><span data-stu-id="0b456-119">The **Azure Active Directory Premium 1** edition extends this model by also enabling you to examine some of the underlying risk events that have been detected for each report.</span></span> 

- <span data-ttu-id="0b456-120">Edice **Azure Active Directory Premium 2** poskytuje nejpodrobnější informace o všech základních rizikových událostech a umožňuje také konfigurovat zásady zabezpečení, které automaticky reagují na nakonfigurované úrovně rizika.</span><span class="sxs-lookup"><span data-stu-id="0b456-120">The **Azure Active Directory Premium 2** edition provides you with the most detailed information about all underlying risk events and it also enables you to configure security policies that automatically respond to configured risk levels.</span></span>



## <a name="azure-active-directory-free-and-basic-edition"></a><span data-ttu-id="0b456-121">Edice Free a Basic služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0b456-121">Azure Active Directory free and basic edition</span></span>

<span data-ttu-id="0b456-122">Edice Free a Basic služby Azure Active Directory poskytují seznam rizikových přihlášení, která byla zjištěná pro vaše uživatele.</span><span class="sxs-lookup"><span data-stu-id="0b456-122">The Azure Active Directory free and basic editions provide you with a list of risky sign-ins that have been detected for your users.</span></span> <span data-ttu-id="0b456-123">Tato sestava uvádí:</span><span class="sxs-lookup"><span data-stu-id="0b456-123">This report lists:</span></span>

- <span data-ttu-id="0b456-124">**Uživatel** – Jméno uživatele použité během přihlašovací operace</span><span class="sxs-lookup"><span data-stu-id="0b456-124">**User** - The name of the user that was used during the sign-in operation</span></span>
- <span data-ttu-id="0b456-125">**IP** – IP adresa zařízení použitá pro připojení k Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0b456-125">**IP** - The IP address of the device that was used to connect to Azure Active Directory</span></span>
- <span data-ttu-id="0b456-126">**Umístění** – Umístění použité pro připojení k Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0b456-126">**Location** - The location used to connect to Azure Active Directory</span></span>
- <span data-ttu-id="0b456-127">**Čas přihlášení** – Čas, kdy k přihlášení došlo</span><span class="sxs-lookup"><span data-stu-id="0b456-127">**Sign-in time** - The time when the sign-in was performed</span></span>
- <span data-ttu-id="0b456-128">**Stav** – Stav přihlášení</span><span class="sxs-lookup"><span data-stu-id="0b456-128">**Status** - The status of the sign-in</span></span>


![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/01.png)

<span data-ttu-id="0b456-130">Na základě prošetření rizikového přihlášení můžete službě Azure Active Directory poskytnout zpětnou vazbu ve formě následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="0b456-130">Based on your investigation of the risky sign-in, you can provide feedback to Azure Active Directory in form of the following actions:</span></span>

- <span data-ttu-id="0b456-131">Vyřešit</span><span class="sxs-lookup"><span data-stu-id="0b456-131">Resolve</span></span>
- <span data-ttu-id="0b456-132">Označit jako falešně pozitivní</span><span class="sxs-lookup"><span data-stu-id="0b456-132">Mark as false positive</span></span>
- <span data-ttu-id="0b456-133">Ignorovat</span><span class="sxs-lookup"><span data-stu-id="0b456-133">Ignore</span></span>
- <span data-ttu-id="0b456-134">Znovu aktivovat</span><span class="sxs-lookup"><span data-stu-id="0b456-134">Reactivate</span></span>

![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/21.png)

<span data-ttu-id="0b456-136">další informace najdete v tématu věnovaném [ručnímu zavření rizikových událostí](active-directory-identityprotection.md#closing-risk-events-manually).</span><span class="sxs-lookup"><span data-stu-id="0b456-136">For more details, see [Closing risk events manually](active-directory-identityprotection.md#closing-risk-events-manually).</span></span>

<span data-ttu-id="0b456-137">V tomto dialogovém okně máte možnost:</span><span class="sxs-lookup"><span data-stu-id="0b456-137">This report provides you with an option to:</span></span>

- <span data-ttu-id="0b456-138">Vyhledávat prostředky</span><span class="sxs-lookup"><span data-stu-id="0b456-138">Searh resources</span></span>
- <span data-ttu-id="0b456-139">Stáhnout data sestavy</span><span class="sxs-lookup"><span data-stu-id="0b456-139">Download the report data</span></span>


![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/93.png)


## <a name="azure-active-directory-premium-editions"></a><span data-ttu-id="0b456-141">Edice Premium služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0b456-141">Azure Active Directory premium editions</span></span>

<span data-ttu-id="0b456-142">Sestavy rizikových přihlášení v edicích Premium služby Azure Active Directory vám nabízí:</span><span class="sxs-lookup"><span data-stu-id="0b456-142">The risky sign-ins report in the Azure Active Directory premium editions provides you with:</span></span>

- <span data-ttu-id="0b456-143">Agregované informace o [typech rizikových událostí](active-directory-identity-protection-risk-events.md), které byly zjištěné</span><span class="sxs-lookup"><span data-stu-id="0b456-143">Aggregated information about the [risk event types](active-directory-identity-protection-risk-events.md) that have been detected</span></span>

- <span data-ttu-id="0b456-144">Možnost stažení sestavy</span><span class="sxs-lookup"><span data-stu-id="0b456-144">An option to download the report</span></span>


![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/456.png)


<span data-ttu-id="0b456-146">Při výběru rizikové události získáte zobrazení podrobné sestavy pro tuto rizikovou událost, které umožňuje následující akce:</span><span class="sxs-lookup"><span data-stu-id="0b456-146">When you select a risk event, you get a detailed report view for this risk event that enables you to:</span></span>

- <span data-ttu-id="0b456-147">Výběr možnosti konfigurace [zásad odstraňování rizik uživatelů](active-directory-identityprotection.md#user-risk-security-policy)</span><span class="sxs-lookup"><span data-stu-id="0b456-147">An option to configure a [user risk remediation policy](active-directory-identityprotection.md#user-risk-security-policy)</span></span>  

- <span data-ttu-id="0b456-148">Kontrola časové osy zjištění pro rizikové události</span><span class="sxs-lookup"><span data-stu-id="0b456-148">Review the detection timeline for the risk event</span></span>  

- <span data-ttu-id="0b456-149">Kontrola seznamu uživatelů, pro které byla riziková událost zjištěná</span><span class="sxs-lookup"><span data-stu-id="0b456-149">Review a list of users for which this risk event has been detected</span></span>

- <span data-ttu-id="0b456-150">[Ruční zavření rizikových událostí](active-directory-identityprotection.md#closing-risk-events-manually) nebo reaktivace ručně zavřené rizikové události</span><span class="sxs-lookup"><span data-stu-id="0b456-150">[Manually close risk events](active-directory-identityprotection.md#closing-risk-events-manually) or reactivate a manually closed risk event.</span></span> 


![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/457.png)

<span data-ttu-id="0b456-152">Po výběru uživatele získáte podrobné zobrazení sestavy pro tohoto uživatele, které vám umožňuje:</span><span class="sxs-lookup"><span data-stu-id="0b456-152">When you select a user, you get a detailed report view for this user that enables you to:</span></span>

- <span data-ttu-id="0b456-153">Otevřít zobrazení Všechna přihlášení</span><span class="sxs-lookup"><span data-stu-id="0b456-153">Open the All sign-ins view</span></span>

- <span data-ttu-id="0b456-154">Resetovat heslo uživatele</span><span class="sxs-lookup"><span data-stu-id="0b456-154">Reset the user's password</span></span>

- <span data-ttu-id="0b456-155">Zavřít všechny události</span><span class="sxs-lookup"><span data-stu-id="0b456-155">Dismiss all events</span></span>

- <span data-ttu-id="0b456-156">Vyšetřit rizikové události oznámené pro uživatele</span><span class="sxs-lookup"><span data-stu-id="0b456-156">Investigate reported risk events for the user.</span></span> 


![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/324.png)


<span data-ttu-id="0b456-158">Pokud chcete vyšetřovat rizikovou událost, vyberte ji ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="0b456-158">To investigate a risk event, select one from the list.</span></span>  
<span data-ttu-id="0b456-159">Tím otevřete okno **Podrobnosti** pro tuto rizikovou událost.</span><span class="sxs-lookup"><span data-stu-id="0b456-159">This opens the **Details** blade for this risk event.</span></span> <span data-ttu-id="0b456-160">V okně **Podrobnosti** můžete zvolit [ruční zavření rizikové události](active-directory-identityprotection.md#closing-risk-events-manually) nebo reaktivaci ručně zavřené rizikové události.</span><span class="sxs-lookup"><span data-stu-id="0b456-160">On the **Details** blade, you have the option to either [manually close a risk event](active-directory-identityprotection.md#closing-risk-events-manually) or reactivate a manually closed risk event.</span></span> 


![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/325.png)





## <a name="next-steps"></a><span data-ttu-id="0b456-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0b456-162">Next steps</span></span>

- <span data-ttu-id="0b456-163">Další informace o Azure Active Directory Identity Protection najdete v tématu [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span><span class="sxs-lookup"><span data-stu-id="0b456-163">For more information about Azure Active Directory Identity Protection, see [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span></span>

