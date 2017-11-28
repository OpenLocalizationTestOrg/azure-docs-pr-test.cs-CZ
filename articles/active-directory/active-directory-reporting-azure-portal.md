---
title: "vytváření sestav služby Active Directory aaaAzure | Microsoft Docs"
description: "Poskytuje obecný přehled generování sestav v Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 6141a333-38db-478a-927e-526f1e7614f4
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: c91813acbdc4b0bfcd164169b0b575accac227d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting"></a><span data-ttu-id="084de-103">Generování sestav v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="084de-103">Azure Active Directory reporting</span></span>

<span data-ttu-id="084de-104">Pomocí generování sestav v Azure Active Directory můžete získat přehled o stavu vašeho prostředí.</span><span class="sxs-lookup"><span data-stu-id="084de-104">With Azure Active Directory reporting, you can gain insights into how your environment is doing.</span></span>  
<span data-ttu-id="084de-105">Hello zadaná data umožňuje:</span><span class="sxs-lookup"><span data-stu-id="084de-105">hello provided data enables you to:</span></span>

- <span data-ttu-id="084de-106">Určit, jak uživatelé využívají vaše aplikace a služby.</span><span class="sxs-lookup"><span data-stu-id="084de-106">Determine how your apps and services are utilized by your users</span></span>
- <span data-ttu-id="084de-107">Zjistit potenciální rizika, které mají vliv na stav hello prostředí</span><span class="sxs-lookup"><span data-stu-id="084de-107">Detect potential risks affecting hello health of your environment</span></span>
- <span data-ttu-id="084de-108">Řešit problémy, které brání uživatelům v práci.</span><span class="sxs-lookup"><span data-stu-id="084de-108">Troubleshoot issues preventing your users from getting their work done</span></span>  

<span data-ttu-id="084de-109">Architektura pro vytváření sestav Hello spoléhá na dvě hlavní pilíře:</span><span class="sxs-lookup"><span data-stu-id="084de-109">hello reporting architecture relies on two main pillars:</span></span>

- <span data-ttu-id="084de-110">Sestavy zabezpečení</span><span class="sxs-lookup"><span data-stu-id="084de-110">Security reports</span></span>
- <span data-ttu-id="084de-111">Sestavy aktivit</span><span class="sxs-lookup"><span data-stu-id="084de-111">Activity reports</span></span>

![Vytváření sestav](./media/active-directory-reporting-azure-portal/01.png)



## <a name="security-reports"></a><span data-ttu-id="084de-113">Sestavy zabezpečení</span><span class="sxs-lookup"><span data-stu-id="084de-113">Security reports</span></span>

<span data-ttu-id="084de-114">Hello sestavy zabezpečení v Azure Active Directory pomoct vám tooprotect identity ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="084de-114">hello security reports in Azure Active Directory help you tooprotect your organization's identities.</span></span>  
<span data-ttu-id="084de-115">V Azure Active Directory existují dva typy sestav zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="084de-115">There are two types of security reports in Azure Active Directory:</span></span>

- <span data-ttu-id="084de-116">**Uživatelé označení příznakem rizik** – hello [uživatelé označení příznakem riziko zabezpečení sestavy](active-directory-reporting-security-user-at-risk.md), získat přehled o uživatelské účty, které může být ohrožena.</span><span class="sxs-lookup"><span data-stu-id="084de-116">**Users flagged for risk** - From hello [users flagged for risk security report](active-directory-reporting-security-user-at-risk.md), you get an overview of user accounts that might have been compromised.</span></span>

- <span data-ttu-id="084de-117">**Rizikové přihlášení** – s hello [sestavy zabezpečení rizikové přihlášení](active-directory-reporting-security-risky-sign-ins.md), můžete získat slouží jako ukazatel přihlašovací pokusy, které se možná prováděly někdo, kdo není hello legitimní vlastníka uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="084de-117">**Risky sign-ins** - With hello [risky sign-in security report](active-directory-reporting-security-risky-sign-ins.md), you get an indicator for sign-in attempts that might have been performed by someone who is not hello legitimate owner of a user account.</span></span> 

<span data-ttu-id="084de-118">**Jaké licence Azure AD potřebujete tooaccess sestavy zabezpečení?**</span><span class="sxs-lookup"><span data-stu-id="084de-118">**What Azure AD license do you need tooaccess a security report?**</span></span>  
<span data-ttu-id="084de-119">Sestavy uživatelů označených příznakem rizika a rizikových přihlášení nabízí všechny edice Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="084de-119">All editions of Azure Active Directory provide you with users flagged for risk and risky sign-ins reports.</span></span>  
<span data-ttu-id="084de-120">Ale hello úroveň členitost liší hello edice:</span><span class="sxs-lookup"><span data-stu-id="084de-120">However, hello level of report granularity varies between hello editions:</span></span> 

- <span data-ttu-id="084de-121">V hello **edice Basic a Azure Active Directory volné**, již získat seznam uživatelů, které jsou označeny k rizika a rizikové přihlášení.</span><span class="sxs-lookup"><span data-stu-id="084de-121">In hello **Azure Active Directory Free and Basic editions**, you already get a list of users flagged for risk and risky sign-ins.</span></span> 

- <span data-ttu-id="084de-122">Hello **Azure Active Directory Premium 1** edice rozšiřuje tohoto modelu tím, že také umožňuje tooexamine, některé základní riziko události, které pro každou sestavu nebyla zjištěna hello.</span><span class="sxs-lookup"><span data-stu-id="084de-122">hello **Azure Active Directory Premium 1** edition extends this model by also enabling you tooexamine some of hello underlying risk events that have been detected for each report.</span></span> 

- <span data-ttu-id="084de-123">Hello **Azure Active Directory Premium 2** edice nabízí vám hello nejvíce podrobné informace o hello základní rizikových událostech a také vám umožní tooconfigure zásady zabezpečení, které automaticky odpovídat tooconfigured úrovní rizika.</span><span class="sxs-lookup"><span data-stu-id="084de-123">hello **Azure Active Directory Premium 2** edition provides you with hello most detailed information about hello underlying risk events and it also enables you tooconfigure security policies that automatically respond tooconfigured risk levels.</span></span>


## <a name="activity-reports"></a><span data-ttu-id="084de-124">Sestavy aktivit</span><span class="sxs-lookup"><span data-stu-id="084de-124">Activity reports</span></span>

<span data-ttu-id="084de-125">V Azure Active Directory existují dva typy sestav aktivit:</span><span class="sxs-lookup"><span data-stu-id="084de-125">There are two types of activity reports in Azure Active Directory:</span></span>

- <span data-ttu-id="084de-126">**Protokoly auditu** – hello [protokoly auditu sestava aktivit](active-directory-reporting-activity-audit-logs.md) vám poskytne přístup k historii toohello každý úkol provést ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="084de-126">**Audit logs** - hello [audit logs activity report](active-directory-reporting-activity-audit-logs.md) provides you with access toohello history of every task performed in your tenant.</span></span>

- <span data-ttu-id="084de-127">**Přihlášení** – s hello [sestava aktivit přihlášení](active-directory-reporting-activity-sign-ins.md), můžete určit, kdo má provést úlohy hello hlášené sestavy protokolů auditu hello.</span><span class="sxs-lookup"><span data-stu-id="084de-127">**Sign-ins** -  With hello [sign-ins activity report](active-directory-reporting-activity-sign-ins.md), you can determine, who has performed hello tasks reported by hello audit logs report.</span></span>



<span data-ttu-id="084de-128">Hello **protokoly auditu sestavy** vám poskytne záznamů aktivit systému dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="084de-128">hello **audit logs report** provides you with records of system activities for compliance.</span></span>
<span data-ttu-id="084de-129">Mimo jiné hello za předpokladu, že data vám umožní tooaddress běžné scénáře, jako:</span><span class="sxs-lookup"><span data-stu-id="084de-129">Amongst others, hello provided data enables you tooaddress common scenarios such as:</span></span>

- <span data-ttu-id="084de-130">Někdo v klienta získali přístupové skupiny pro správu tooan.</span><span class="sxs-lookup"><span data-stu-id="084de-130">Someone in my tenant got access tooan admin group.</span></span> <span data-ttu-id="084de-131">Kdo jim dal přístup?</span><span class="sxs-lookup"><span data-stu-id="084de-131">Who gave them access?</span></span> 

- <span data-ttu-id="084de-132">Chci, aby tooknow hello seznam uživatelů, přihlášení k konkrétní aplikaci od I nedávno zařazený nemá hello aplikace a chcete tooknow Pokud dobře funguje</span><span class="sxs-lookup"><span data-stu-id="084de-132">I want tooknow hello list of users signing into a specific app since I recently onboarded hello app and want tooknow if it’s doing well</span></span>

- <span data-ttu-id="084de-133">Chci, aby tooknow kolik heslo resetovat se děje v klienta</span><span class="sxs-lookup"><span data-stu-id="084de-133">I want tooknow how many password resets are happening in my tenant</span></span>


<span data-ttu-id="084de-134">**Jaké licence Azure AD potřebujete sestavy protokolů auditu hello tooaccess?**</span><span class="sxs-lookup"><span data-stu-id="084de-134">**What Azure AD license do you need tooaccess hello audit logs report?**</span></span>  
<span data-ttu-id="084de-135">Sestava protokoly auditu Hello je k dispozici pro funkce, pro které máte licence.</span><span class="sxs-lookup"><span data-stu-id="084de-135">hello audit logs report is available for features for which you have licenses.</span></span> <span data-ttu-id="084de-136">Pokud máte licenci pro konkrétní funkci, máte také přístup toohello auditní informace protokolu pro ni.</span><span class="sxs-lookup"><span data-stu-id="084de-136">If you have a license for a specific feature, you also have access toohello audit log information for it.</span></span>

<span data-ttu-id="084de-137">Další podrobnosti najdete v tématu **porovnávání všeobecně dostupná funkce edice Free, Basic a Premium hello** v [Azure Active Directory funkce a možnosti](https://www.microsoft.com/cloud-platform/azure-active-directory-features).</span><span class="sxs-lookup"><span data-stu-id="084de-137">For more details, see **Comparing generally available features of hello Free, Basic, and Premium editions** in [Azure Active Directory features and capabilities](https://www.microsoft.com/cloud-platform/azure-active-directory-features).</span></span>   



<span data-ttu-id="084de-138">Hello **sestava aktivit přihlášení** umožňuje tootoofind odpovědi tooquestions jako například:</span><span class="sxs-lookup"><span data-stu-id="084de-138">hello **sign-ins activity report** enables tootoofind answers tooquestions such as:</span></span>

- <span data-ttu-id="084de-139">Co je hello přihlášení vzor uživatele?</span><span class="sxs-lookup"><span data-stu-id="084de-139">What is hello sign-in pattern of a user?</span></span>
- <span data-ttu-id="084de-140">Kolik uživatelů se přihlásilo za týden?</span><span class="sxs-lookup"><span data-stu-id="084de-140">How many users have users signed in over a week?</span></span>
- <span data-ttu-id="084de-141">Co je hello stav těchto přihlášení?</span><span class="sxs-lookup"><span data-stu-id="084de-141">What’s hello status of these sign-ins?</span></span>


<span data-ttu-id="084de-142">**Jaké licence Azure AD se budete potřebovat tooaccess hello sestava aktivit přihlášení?**</span><span class="sxs-lookup"><span data-stu-id="084de-142">**What Azure AD license do you need tooaccess hello sign-ins activity report?**</span></span>  
<span data-ttu-id="084de-143">Sestava aktivit přihlášení hello tooaccess, vašeho klienta musí mít licenci na Azure AD Premium s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="084de-143">tooaccess hello sign-ins activity report, your tenant must have an Azure AD Premium license associated with it.</span></span>


## <a name="programmatic-access"></a><span data-ttu-id="084de-144">Programový přístup</span><span class="sxs-lookup"><span data-stu-id="084de-144">Programmatic access</span></span>

<span data-ttu-id="084de-145">Přidání toohello uživatelské rozhraní, generování sestav Azure Active Directory také nabízí [programový přístup](active-directory-reporting-api-getting-started-azure-portal.md) toohello data pro vytváření sestav.</span><span class="sxs-lookup"><span data-stu-id="084de-145">In addition toohello user interface, Azure Active Directory reporting also provides you with [programmatic access](active-directory-reporting-api-getting-started-azure-portal.md) toohello reporting data.</span></span> <span data-ttu-id="084de-146">Hello data tyto sestavy může být velmi užitečná tooyour aplikace, jako je například systémů SIEM, auditování a nástroje business intelligence.</span><span class="sxs-lookup"><span data-stu-id="084de-146">hello data of these reports can be very useful tooyour applications, such as SIEM systems, audit, and business intelligence tools.</span></span> <span data-ttu-id="084de-147">rozhraní API poskytují programový přístup k datům toohello pomocí sady založené na REST API, vytváření sestav Hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="084de-147">hello Azure AD reporting APIs provide programmatic access toohello data through a set of REST-based APIs.</span></span> <span data-ttu-id="084de-148">Tato rozhraní API můžete volat z nejrůznějších programovacích jazyků a nástrojů.</span><span class="sxs-lookup"><span data-stu-id="084de-148">You can call these APIs from a variety of programming languages and tools.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="084de-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="084de-149">Next steps</span></span>

<span data-ttu-id="084de-150">Pokud chcete více informací o hello tooknow různé typy sestav v Azure Active Directory, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="084de-150">If you want tooknow more about hello various report types in Azure Active Directory, see:</span></span>

- [<span data-ttu-id="084de-151">Sestava uživatelů s příznakem rizika</span><span class="sxs-lookup"><span data-stu-id="084de-151">Users flagged for risk report</span></span>](active-directory-reporting-security-user-at-risk.md)
- [<span data-ttu-id="084de-152">Sestava rizikových přihlášení</span><span class="sxs-lookup"><span data-stu-id="084de-152">Risky sign-ins report</span></span>](active-directory-reporting-security-risky-sign-ins.md)
- [<span data-ttu-id="084de-153">Sestava protokolů auditu</span><span class="sxs-lookup"><span data-stu-id="084de-153">Audit logs report</span></span>](active-directory-reporting-activity-audit-logs.md)
- [<span data-ttu-id="084de-154">Sestava protokolů přihlášení</span><span class="sxs-lookup"><span data-stu-id="084de-154">Sign-ins logs report</span></span>](active-directory-reporting-activity-sign-ins.md)

<span data-ttu-id="084de-155">Pokud chcete více informací o přístupu k hello hello data pomocí hello reporting rozhraní API pro vytváření sestav tooknow, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="084de-155">If you want tooknow more about accessing hello hello reporting data using hello reporting API, see:</span></span> 

- [<span data-ttu-id="084de-156">Začínáme s Azure Active Directory, vytváření sestav API hello</span><span class="sxs-lookup"><span data-stu-id="084de-156">Getting started with hello Azure Active Directory reporting API</span></span>](active-directory-reporting-api-getting-started-azure-portal.md)


<!--Image references-->
[1]: ./media/active-directory-reporting-azure-portal/ic195031.png