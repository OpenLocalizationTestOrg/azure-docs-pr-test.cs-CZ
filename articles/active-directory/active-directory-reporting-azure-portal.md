---
title: "Generování sestav v Azure Active Directory | Dokumentace Microsoftu"
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
ms.openlocfilehash: 738c8f4a56586b87f03973ec258b0a3023affa60
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-reporting"></a><span data-ttu-id="c3109-103">Generování sestav v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c3109-103">Azure Active Directory reporting</span></span>

<span data-ttu-id="c3109-104">Pomocí generování sestav v Azure Active Directory můžete získat přehled o stavu vašeho prostředí.</span><span class="sxs-lookup"><span data-stu-id="c3109-104">With Azure Active Directory reporting, you can gain insights into how your environment is doing.</span></span>  
<span data-ttu-id="c3109-105">Poskytnutá data vám umožní:</span><span class="sxs-lookup"><span data-stu-id="c3109-105">The provided data enables you to:</span></span>

- <span data-ttu-id="c3109-106">Určit, jak uživatelé využívají vaše aplikace a služby.</span><span class="sxs-lookup"><span data-stu-id="c3109-106">Determine how your apps and services are utilized by your users</span></span>
- <span data-ttu-id="c3109-107">Rozpoznat potenciální rizika ovlivňující stav vašeho prostředí.</span><span class="sxs-lookup"><span data-stu-id="c3109-107">Detect potential risks affecting the health of your environment</span></span>
- <span data-ttu-id="c3109-108">Řešit problémy, které brání uživatelům v práci.</span><span class="sxs-lookup"><span data-stu-id="c3109-108">Troubleshoot issues preventing your users from getting their work done</span></span>  

<span data-ttu-id="c3109-109">Architektura generování sestav se spoléhá na dva hlavní pilíře:</span><span class="sxs-lookup"><span data-stu-id="c3109-109">The reporting architecture relies on two main pillars:</span></span>

- <span data-ttu-id="c3109-110">Sestavy zabezpečení</span><span class="sxs-lookup"><span data-stu-id="c3109-110">Security reports</span></span>
- <span data-ttu-id="c3109-111">Sestavy aktivit</span><span class="sxs-lookup"><span data-stu-id="c3109-111">Activity reports</span></span>

![Vytváření sestav](./media/active-directory-reporting-azure-portal/01.png)



## <a name="security-reports"></a><span data-ttu-id="c3109-113">Sestavy zabezpečení</span><span class="sxs-lookup"><span data-stu-id="c3109-113">Security reports</span></span>

<span data-ttu-id="c3109-114">Sestavy zabezpečení v Azure Active Directory pomáhají chránit identity vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="c3109-114">The security reports in Azure Active Directory help you to protect your organization's identities.</span></span>  
<span data-ttu-id="c3109-115">V Azure Active Directory existují dva typy sestav zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="c3109-115">There are two types of security reports in Azure Active Directory:</span></span>

- <span data-ttu-id="c3109-116">**Uživatelé označení příznakem rizika** – Ze [sestavy zabezpečení uživatelů označených příznakem rizika](active-directory-reporting-security-user-at-risk.md) získáte přehled o uživatelských účtech, u kterých mohlo dojít k ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="c3109-116">**Users flagged for risk** - From the [users flagged for risk security report](active-directory-reporting-security-user-at-risk.md), you get an overview of user accounts that might have been compromised.</span></span>

- <span data-ttu-id="c3109-117">**Riziková přihlášení** – Se [sestavou zabezpečení rizikových přihlášení](active-directory-reporting-security-risky-sign-ins.md) získáte indikátor pokusů o přihlášení, které mohl provést někdo, kdo není legitimním vlastníkem uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="c3109-117">**Risky sign-ins** - With the [risky sign-in security report](active-directory-reporting-security-risky-sign-ins.md), you get an indicator for sign-in attempts that might have been performed by someone who is not the legitimate owner of a user account.</span></span> 

<span data-ttu-id="c3109-118">**Jaká licence Azure AD je potřeba pro přístup k sestavě zabezpečení?**</span><span class="sxs-lookup"><span data-stu-id="c3109-118">**What Azure AD license do you need to access a security report?**</span></span>  
<span data-ttu-id="c3109-119">Sestavy uživatelů označených příznakem rizika a rizikových přihlášení nabízí všechny edice Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c3109-119">All editions of Azure Active Directory provide you with users flagged for risk and risky sign-ins reports.</span></span>  
<span data-ttu-id="c3109-120">Úroveň podrobností sestav se však mezi jednotlivými edicemi liší:</span><span class="sxs-lookup"><span data-stu-id="c3109-120">However, the level of report granularity varies between the editions:</span></span> 

- <span data-ttu-id="c3109-121">**Edice Azure Active Directory Free a Basic** již nabízí seznam uživatelů označených příznakem rizika a rizikových přihlášení.</span><span class="sxs-lookup"><span data-stu-id="c3109-121">In the **Azure Active Directory Free and Basic editions**, you already get a list of users flagged for risk and risky sign-ins.</span></span> 

- <span data-ttu-id="c3109-122">Edice **Azure Active Directory Premium 1** tento model rozšiřuje tím, že umožňuje také prozkoumávat některé ze základních rizikových událostí, které byly v každé sestavě rozpoznány.</span><span class="sxs-lookup"><span data-stu-id="c3109-122">The **Azure Active Directory Premium 1** edition extends this model by also enabling you to examine some of the underlying risk events that have been detected for each report.</span></span> 

- <span data-ttu-id="c3109-123">Edice **Azure Active Directory Premium 2** poskytuje nejpodrobnější informace o základních rizikových událostech a umožňuje také konfigurovat zásady zabezpečení, které automaticky reagují na nakonfigurované úrovně rizika.</span><span class="sxs-lookup"><span data-stu-id="c3109-123">The **Azure Active Directory Premium 2** edition provides you with the most detailed information about the underlying risk events and it also enables you to configure security policies that automatically respond to configured risk levels.</span></span>


## <a name="activity-reports"></a><span data-ttu-id="c3109-124">Sestavy aktivit</span><span class="sxs-lookup"><span data-stu-id="c3109-124">Activity reports</span></span>

<span data-ttu-id="c3109-125">V Azure Active Directory existují dva typy sestav aktivit:</span><span class="sxs-lookup"><span data-stu-id="c3109-125">There are two types of activity reports in Azure Active Directory:</span></span>

- <span data-ttu-id="c3109-126">**Protokoly auditu** – [Sestava aktivit protokolů auditu](active-directory-reporting-activity-audit-logs.md) poskytuje přístup k historii každé úlohy provedené ve vašem tenantovi.</span><span class="sxs-lookup"><span data-stu-id="c3109-126">**Audit logs** - The [audit logs activity report](active-directory-reporting-activity-audit-logs.md) provides you with access to the history of every task performed in your tenant.</span></span>

- <span data-ttu-id="c3109-127">**Přihlášení** – Se [sestavou aktivit přihlašování](active-directory-reporting-activity-sign-ins.md) můžete určit, kdo provedl úlohy hlášené sestavou protokolů auditu.</span><span class="sxs-lookup"><span data-stu-id="c3109-127">**Sign-ins** -  With the [sign-ins activity report](active-directory-reporting-activity-sign-ins.md), you can determine, who has performed the tasks reported by the audit logs report.</span></span>



<span data-ttu-id="c3109-128">**Sestava protokolů auditu** poskytuje záznamy systémových aktivit pro zajištění dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="c3109-128">The **audit logs report** provides you with records of system activities for compliance.</span></span>
<span data-ttu-id="c3109-129">Poskytnutá data umožňují mimo jiné řešit běžné scénáře tohoto typu:</span><span class="sxs-lookup"><span data-stu-id="c3109-129">Amongst others, the provided data enables you to address common scenarios such as:</span></span>

- <span data-ttu-id="c3109-130">Někdo v tenantovi získal přístup ke správcovské skupině.</span><span class="sxs-lookup"><span data-stu-id="c3109-130">Someone in my tenant got access to an admin group.</span></span> <span data-ttu-id="c3109-131">Kdo jim dal přístup?</span><span class="sxs-lookup"><span data-stu-id="c3109-131">Who gave them access?</span></span> 

- <span data-ttu-id="c3109-132">Chcete znát seznam uživatelů, kteří se přihlašují ke konkrétní aplikaci, protože jste aplikaci nedávno zprovoznili a chcete vědět, jestli funguje v pořádku.</span><span class="sxs-lookup"><span data-stu-id="c3109-132">I want to know the list of users signing into a specific app since I recently onboarded the app and want to know if it’s doing well</span></span>

- <span data-ttu-id="c3109-133">Chcete vědět, ke kolika resetováním hesla ve vašem tenantovi dochází.</span><span class="sxs-lookup"><span data-stu-id="c3109-133">I want to know how many password resets are happening in my tenant</span></span>


<span data-ttu-id="c3109-134">**Jaká licence Azure AD je potřeba pro přístup k sestavě protokolů auditu?**</span><span class="sxs-lookup"><span data-stu-id="c3109-134">**What Azure AD license do you need to access the audit logs report?**</span></span>  
<span data-ttu-id="c3109-135">Sestava protokolů auditu je dostupná pro funkce, ke kterým máte licence.</span><span class="sxs-lookup"><span data-stu-id="c3109-135">The audit logs report is available for features for which you have licenses.</span></span> <span data-ttu-id="c3109-136">Pokud máte licenci ke konkrétní funkci, máte u ní také přístup k informacím protokolu auditu.</span><span class="sxs-lookup"><span data-stu-id="c3109-136">If you have a license for a specific feature, you also have access to the audit log information for it.</span></span>

<span data-ttu-id="c3109-137">Další podrobnosti najdete v části **Porovnání všeobecně dostupných funkcí v edicích Free, Basic a Premium** v tématu [Funkce a možnosti v Azure Active Directory](https://www.microsoft.com/cloud-platform/azure-active-directory-features).</span><span class="sxs-lookup"><span data-stu-id="c3109-137">For more details, see **Comparing generally available features of the Free, Basic, and Premium editions** in [Azure Active Directory features and capabilities](https://www.microsoft.com/cloud-platform/azure-active-directory-features).</span></span>   



<span data-ttu-id="c3109-138">**Sestava aktivit přihlašování** umožňuje najít odpovědi na otázky tohoto typu:</span><span class="sxs-lookup"><span data-stu-id="c3109-138">The **sign-ins activity report** enables to to find answers to questions such as:</span></span>

- <span data-ttu-id="c3109-139">Jaký je vzorec přihlašování uživatele?</span><span class="sxs-lookup"><span data-stu-id="c3109-139">What is the sign-in pattern of a user?</span></span>
- <span data-ttu-id="c3109-140">Kolik uživatelů se přihlásilo za týden?</span><span class="sxs-lookup"><span data-stu-id="c3109-140">How many users have users signed in over a week?</span></span>
- <span data-ttu-id="c3109-141">Jaký je stav těchto přihlášení?</span><span class="sxs-lookup"><span data-stu-id="c3109-141">What’s the status of these sign-ins?</span></span>


<span data-ttu-id="c3109-142">**Jaká licence Azure AD je potřeba pro přístup k sestavě aktivit přihlašování?**</span><span class="sxs-lookup"><span data-stu-id="c3109-142">**What Azure AD license do you need to access the sign-ins activity report?**</span></span>  
<span data-ttu-id="c3109-143">Pro přístup k sestavě aktivit přihlašování musí mít váš tenant přiřazenou licenci Azure AD Premium.</span><span class="sxs-lookup"><span data-stu-id="c3109-143">To access the sign-ins activity report, your tenant must have an Azure AD Premium license associated with it.</span></span>


## <a name="programmatic-access"></a><span data-ttu-id="c3109-144">Programový přístup</span><span class="sxs-lookup"><span data-stu-id="c3109-144">Programmatic access</span></span>

<span data-ttu-id="c3109-145">Kromě uživatelského rozhraní nabízí generování sestav v Azure Active Directory také [programový přístup](active-directory-reporting-api-getting-started-azure-portal.md) k datům sestav.</span><span class="sxs-lookup"><span data-stu-id="c3109-145">In addition to the user interface, Azure Active Directory reporting also provides you with [programmatic access](active-directory-reporting-api-getting-started-azure-portal.md) to the reporting data.</span></span> <span data-ttu-id="c3109-146">Data z těchto sestav můžou být velmi užitečná pro vaše aplikace, jako jsou systémy SIEM nebo nástroje pro auditování a business intelligence.</span><span class="sxs-lookup"><span data-stu-id="c3109-146">The data of these reports can be very useful to your applications, such as SIEM systems, audit, and business intelligence tools.</span></span> <span data-ttu-id="c3109-147">Rozhraní API pro generování sestav v Azure AD poskytují programový přístup k těmto datům prostřednictvím sady rozhraní API založených na REST.</span><span class="sxs-lookup"><span data-stu-id="c3109-147">The Azure AD reporting APIs provide programmatic access to the data through a set of REST-based APIs.</span></span> <span data-ttu-id="c3109-148">Tato rozhraní API můžete volat z nejrůznějších programovacích jazyků a nástrojů.</span><span class="sxs-lookup"><span data-stu-id="c3109-148">You can call these APIs from a variety of programming languages and tools.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="c3109-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c3109-149">Next steps</span></span>

<span data-ttu-id="c3109-150">Pokud se chcete dozvědět více o různých typech sestav v Azure Active Directory, přečtěte si:</span><span class="sxs-lookup"><span data-stu-id="c3109-150">If you want to know more about the various report types in Azure Active Directory, see:</span></span>

- [<span data-ttu-id="c3109-151">Sestava uživatelů s příznakem rizika</span><span class="sxs-lookup"><span data-stu-id="c3109-151">Users flagged for risk report</span></span>](active-directory-reporting-security-user-at-risk.md)
- [<span data-ttu-id="c3109-152">Sestava rizikových přihlášení</span><span class="sxs-lookup"><span data-stu-id="c3109-152">Risky sign-ins report</span></span>](active-directory-reporting-security-risky-sign-ins.md)
- [<span data-ttu-id="c3109-153">Sestava protokolů auditu</span><span class="sxs-lookup"><span data-stu-id="c3109-153">Audit logs report</span></span>](active-directory-reporting-activity-audit-logs.md)
- [<span data-ttu-id="c3109-154">Sestava protokolů přihlášení</span><span class="sxs-lookup"><span data-stu-id="c3109-154">Sign-ins logs report</span></span>](active-directory-reporting-activity-sign-ins.md)

<span data-ttu-id="c3109-155">Pokud se chcete dozvědět více o přístupu k datům sestav pomocí rozhraní API pro generování sestav, přečtěte si:</span><span class="sxs-lookup"><span data-stu-id="c3109-155">If you want to know more about accessing the the reporting data using the reporting API, see:</span></span> 

- [<span data-ttu-id="c3109-156">Začínáme s rozhraním API pro generování sestav v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c3109-156">Getting started with the Azure Active Directory reporting API</span></span>](active-directory-reporting-api-getting-started-azure-portal.md)


<!--Image references-->
[1]: ./media/active-directory-reporting-azure-portal/ic195031.png