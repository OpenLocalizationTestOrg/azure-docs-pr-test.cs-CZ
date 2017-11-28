---
title: "Sestava přihlašování aaaRisky hello Azure Active Directory portálu | Microsoft Docs"
description: "Další informace o hello rizikové přihlášení sestavy v portálu Azure Active Directory hello"
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
ms.openlocfilehash: d8df5cafea6b38f3e364c24a6aff599abe088e88
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="risky-sign-ins-report-in-hello-azure-active-directory-portal"></a><span data-ttu-id="7cb9c-103">Sestava rizikové přihlášení na portálu Azure Active Directory hello</span><span class="sxs-lookup"><span data-stu-id="7cb9c-103">Risky sign-ins report in hello Azure Active Directory portal</span></span>

<span data-ttu-id="7cb9c-104">Pomocí sestavy hello zabezpečení v Azure Active Directory (Azure AD) je možné získat přehled o hello pravděpodobnost ohroženými uživatelskými účty ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="7cb9c-104">With hello security reports in Azure Active Directory (Azure AD) you can gain insights into hello probability of compromised user accounts in your environment.</span></span> 

<span data-ttu-id="7cb9c-105">Azure AD detekuje podezřelé akce, které jsou související tooyour uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="7cb9c-105">Azure AD detects suspicious actions that are related tooyour user accounts.</span></span> <span data-ttu-id="7cb9c-106">Pro každou zjištěnou akci se vytvoří záznam nazvaný *riziková událost*.</span><span class="sxs-lookup"><span data-stu-id="7cb9c-106">For each detected action, a record called *risk event* is created.</span></span> <span data-ttu-id="7cb9c-107">Další podrobnosti najdete v tématu věnovaném [rizikovým událostem služby Azure Active Directory](active-directory-identity-protection-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="7cb9c-107">For more details, see [Azure Active Directory risk events](active-directory-identity-protection-risk-events.md).</span></span> 

<span data-ttu-id="7cb9c-108">Hello zjistila rizikových událostí jsou použité toocalculate:</span><span class="sxs-lookup"><span data-stu-id="7cb9c-108">hello detected risk events are used toocalculate:</span></span>

- <span data-ttu-id="7cb9c-109">**Rizikové přihlášení** -rizikové přihlášení je indikátorem pro pokusu přihlášení, který se možná prováděly uživatelem, který není vlastníkem legitimní hello uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="7cb9c-109">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not hello legitimate owner of a user account.</span></span> <span data-ttu-id="7cb9c-110">Další podrobnosti najdete v tématu [Riziková přihlášení](active-directory-identityprotection.md#risky-sign-ins).</span><span class="sxs-lookup"><span data-stu-id="7cb9c-110">For more details, see [Risky sign-ins](active-directory-identityprotection.md#risky-sign-ins).</span></span> 

- <span data-ttu-id="7cb9c-111">**Uživatelé označení příznakem rizika** – Rizikový uživatel je indikátorem uživatelského účtu, který mohl být ohrožený.</span><span class="sxs-lookup"><span data-stu-id="7cb9c-111">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> <span data-ttu-id="7cb9c-112">Další podrobnosti najdete v tématu [Uživatelé označení příznakem rizika](active-directory-identityprotection.md#users-flagged-for-risk).</span><span class="sxs-lookup"><span data-stu-id="7cb9c-112">For more details, see [Users flagged for risk](active-directory-identityprotection.md#users-flagged-for-risk).</span></span>  

<span data-ttu-id="7cb9c-113">V [hello portál Azure](https://portal.azure.com), můžete najít hello zabezpečení hlásí hello **Azure Active Directory** okno v hello **zabezpečení** části.</span><span class="sxs-lookup"><span data-stu-id="7cb9c-113">In [hello Azure portal](https://portal.azure.com), you can find hello security reports on hello **Azure Active Directory** blade in hello **Security** section.</span></span> 

![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/10.png)


## <a name="what-azure-ad-license-do-you-need-tooaccess-a-security-report"></a><span data-ttu-id="7cb9c-115">Jaké licence Azure AD potřebujete tooaccess sestavy zabezpečení?</span><span class="sxs-lookup"><span data-stu-id="7cb9c-115">What Azure AD license do you need tooaccess a security report?</span></span>  

<span data-ttu-id="7cb9c-116">Sestavy rizikových přihlášení nabízí všechny edice Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7cb9c-116">All editions of Azure Active Directory provide you with risky sign-ins reports.</span></span>  
<span data-ttu-id="7cb9c-117">Ale hello úroveň členitost liší hello edice:</span><span class="sxs-lookup"><span data-stu-id="7cb9c-117">However, hello level of report granularity varies between hello editions:</span></span> 

- <span data-ttu-id="7cb9c-118">V hello **edice Basic a Azure Active Directory volné**, již získat seznam rizikových přihlášení.</span><span class="sxs-lookup"><span data-stu-id="7cb9c-118">In hello **Azure Active Directory Free and Basic editions**, you already get a list of risky sign-ins.</span></span> 

- <span data-ttu-id="7cb9c-119">Hello **Azure Active Directory Premium 1** edice rozšiřuje tohoto modelu tím, že také umožňuje tooexamine, některé základní riziko události, které pro každou sestavu nebyla zjištěna hello.</span><span class="sxs-lookup"><span data-stu-id="7cb9c-119">hello **Azure Active Directory Premium 1** edition extends this model by also enabling you tooexamine some of hello underlying risk events that have been detected for each report.</span></span> 

- <span data-ttu-id="7cb9c-120">Hello **Azure Active Directory Premium 2** edice nabízí vám hello nejvíce podrobné informace o všech základní rizikových událostech a také vám umožní tooconfigure zásady zabezpečení, které automaticky odpovídat tooconfigured úrovní rizika.</span><span class="sxs-lookup"><span data-stu-id="7cb9c-120">hello **Azure Active Directory Premium 2** edition provides you with hello most detailed information about all underlying risk events and it also enables you tooconfigure security policies that automatically respond tooconfigured risk levels.</span></span>



## <a name="azure-active-directory-free-and-basic-edition"></a><span data-ttu-id="7cb9c-121">Edice Free a Basic služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7cb9c-121">Azure Active Directory free and basic edition</span></span>

<span data-ttu-id="7cb9c-122">Hello Azure Active Directory volné a Základní edice poskytují seznam rizikových přihlášení, které byly nalezeny pro vaše uživatele.</span><span class="sxs-lookup"><span data-stu-id="7cb9c-122">hello Azure Active Directory free and basic editions provide you with a list of risky sign-ins that have been detected for your users.</span></span> <span data-ttu-id="7cb9c-123">Tato sestava uvádí:</span><span class="sxs-lookup"><span data-stu-id="7cb9c-123">This report lists:</span></span>

- <span data-ttu-id="7cb9c-124">**Uživatel** – hello název hello uživatele, který byl použit během hello přihlášení operace</span><span class="sxs-lookup"><span data-stu-id="7cb9c-124">**User** - hello name of hello user that was used during hello sign-in operation</span></span>
- <span data-ttu-id="7cb9c-125">**IP** -hello IP adresa hello zařízení, která byla použité tooconnect tooAzure služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="7cb9c-125">**IP** - hello IP address of hello device that was used tooconnect tooAzure Active Directory</span></span>
- <span data-ttu-id="7cb9c-126">**Umístění** -hello umístění použít tooconnect tooAzure služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="7cb9c-126">**Location** - hello location used tooconnect tooAzure Active Directory</span></span>
- <span data-ttu-id="7cb9c-127">**Čas přihlášení** -hello čas, kdy provést přihlášení hello</span><span class="sxs-lookup"><span data-stu-id="7cb9c-127">**Sign-in time** - hello time when hello sign-in was performed</span></span>
- <span data-ttu-id="7cb9c-128">**Stav** -hello stav přihlášení hello</span><span class="sxs-lookup"><span data-stu-id="7cb9c-128">**Status** - hello status of hello sign-in</span></span>


![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/01.png)

<span data-ttu-id="7cb9c-130">Založené na šetření hello rizikové sign-in, můžete poskytnout zpětnou vazbu tooAzure služby Active Directory v podobě hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="7cb9c-130">Based on your investigation of hello risky sign-in, you can provide feedback tooAzure Active Directory in form of hello following actions:</span></span>

- <span data-ttu-id="7cb9c-131">Vyřešit</span><span class="sxs-lookup"><span data-stu-id="7cb9c-131">Resolve</span></span>
- <span data-ttu-id="7cb9c-132">Označit jako falešně pozitivní</span><span class="sxs-lookup"><span data-stu-id="7cb9c-132">Mark as false positive</span></span>
- <span data-ttu-id="7cb9c-133">Ignorovat</span><span class="sxs-lookup"><span data-stu-id="7cb9c-133">Ignore</span></span>
- <span data-ttu-id="7cb9c-134">Znovu aktivovat</span><span class="sxs-lookup"><span data-stu-id="7cb9c-134">Reactivate</span></span>

![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/21.png)

<span data-ttu-id="7cb9c-136">další informace najdete v tématu věnovaném [ručnímu zavření rizikových událostí](active-directory-identityprotection.md#closing-risk-events-manually).</span><span class="sxs-lookup"><span data-stu-id="7cb9c-136">For more details, see [Closing risk events manually](active-directory-identityprotection.md#closing-risk-events-manually).</span></span>

<span data-ttu-id="7cb9c-137">V tomto dialogovém okně máte možnost:</span><span class="sxs-lookup"><span data-stu-id="7cb9c-137">This report provides you with an option to:</span></span>

- <span data-ttu-id="7cb9c-138">Vyhledávat prostředky</span><span class="sxs-lookup"><span data-stu-id="7cb9c-138">Searh resources</span></span>
- <span data-ttu-id="7cb9c-139">Stáhněte si data sestavy hello</span><span class="sxs-lookup"><span data-stu-id="7cb9c-139">Download hello report data</span></span>


![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/93.png)


## <a name="azure-active-directory-premium-editions"></a><span data-ttu-id="7cb9c-141">Edice Premium služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7cb9c-141">Azure Active Directory premium editions</span></span>

<span data-ttu-id="7cb9c-142">Sestava Hello rizikové přihlášení v hello Azure Active Directory premium edicích nabízí:</span><span class="sxs-lookup"><span data-stu-id="7cb9c-142">hello risky sign-ins report in hello Azure Active Directory premium editions provides you with:</span></span>

- <span data-ttu-id="7cb9c-143">Agregovat informace o hello [rizik typů událostí](active-directory-identity-protection-risk-events.md) , o kterých bylo zjištěno</span><span class="sxs-lookup"><span data-stu-id="7cb9c-143">Aggregated information about hello [risk event types](active-directory-identity-protection-risk-events.md) that have been detected</span></span>

- <span data-ttu-id="7cb9c-144">Zprávu o možnost toodownload hello</span><span class="sxs-lookup"><span data-stu-id="7cb9c-144">An option toodownload hello report</span></span>


![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/456.png)


<span data-ttu-id="7cb9c-146">Při výběru rizikové události získáte zobrazení podrobné sestavy pro tuto rizikovou událost, které umožňuje následující akce:</span><span class="sxs-lookup"><span data-stu-id="7cb9c-146">When you select a risk event, you get a detailed report view for this risk event that enables you to:</span></span>

- <span data-ttu-id="7cb9c-147">Možnost tooconfigure [uživatele riziko nápravy zásad](active-directory-identityprotection.md#user-risk-security-policy)</span><span class="sxs-lookup"><span data-stu-id="7cb9c-147">An option tooconfigure a [user risk remediation policy](active-directory-identityprotection.md#user-risk-security-policy)</span></span>  

- <span data-ttu-id="7cb9c-148">Zkontrolujte hello detekce časovou osu pro hello riziko událostí</span><span class="sxs-lookup"><span data-stu-id="7cb9c-148">Review hello detection timeline for hello risk event</span></span>  

- <span data-ttu-id="7cb9c-149">Kontrola seznamu uživatelů, pro které byla riziková událost zjištěná</span><span class="sxs-lookup"><span data-stu-id="7cb9c-149">Review a list of users for which this risk event has been detected</span></span>

- <span data-ttu-id="7cb9c-150">[Ruční zavření rizikových událostí](active-directory-identityprotection.md#closing-risk-events-manually) nebo reaktivace ručně zavřené rizikové události</span><span class="sxs-lookup"><span data-stu-id="7cb9c-150">[Manually close risk events](active-directory-identityprotection.md#closing-risk-events-manually) or reactivate a manually closed risk event.</span></span> 


![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/457.png)

<span data-ttu-id="7cb9c-152">Po výběru uživatele získáte podrobné zobrazení sestavy pro tohoto uživatele, které vám umožňuje:</span><span class="sxs-lookup"><span data-stu-id="7cb9c-152">When you select a user, you get a detailed report view for this user that enables you to:</span></span>

- <span data-ttu-id="7cb9c-153">Zobrazit všechny přihlášení otevřete hello</span><span class="sxs-lookup"><span data-stu-id="7cb9c-153">Open hello All sign-ins view</span></span>

- <span data-ttu-id="7cb9c-154">Resetování hesla hello uživatele</span><span class="sxs-lookup"><span data-stu-id="7cb9c-154">Reset hello user's password</span></span>

- <span data-ttu-id="7cb9c-155">Zavřít všechny události</span><span class="sxs-lookup"><span data-stu-id="7cb9c-155">Dismiss all events</span></span>

- <span data-ttu-id="7cb9c-156">Prozkoumejte hlášené riziko události pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="7cb9c-156">Investigate reported risk events for hello user.</span></span> 


![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/324.png)


<span data-ttu-id="7cb9c-158">tooinvestigate riziko události, vyberte ze seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="7cb9c-158">tooinvestigate a risk event, select one from hello list.</span></span>  
<span data-ttu-id="7cb9c-159">Tím se otevře hello **podrobnosti** okno pro tuto událost riziko.</span><span class="sxs-lookup"><span data-stu-id="7cb9c-159">This opens hello **Details** blade for this risk event.</span></span> <span data-ttu-id="7cb9c-160">Na hello **podrobnosti** okno, máte možnost tooeither hello [ruční zavření riziko událostí](active-directory-identityprotection.md#closing-risk-events-manually) nebo znovu aktivovat ručně uzavřené riziko událostí.</span><span class="sxs-lookup"><span data-stu-id="7cb9c-160">On hello **Details** blade, you have hello option tooeither [manually close a risk event](active-directory-identityprotection.md#closing-risk-events-manually) or reactivate a manually closed risk event.</span></span> 


![Riziková přihlášení](./media/active-directory-reporting-security-risky-sign-ins/325.png)





## <a name="next-steps"></a><span data-ttu-id="7cb9c-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7cb9c-162">Next steps</span></span>

- <span data-ttu-id="7cb9c-163">Další informace o Azure Active Directory Identity Protection najdete v tématu [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span><span class="sxs-lookup"><span data-stu-id="7cb9c-163">For more information about Azure Active Directory Identity Protection, see [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span></span>

