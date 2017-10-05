---
title: Latence sestav Azure Active Directory | Microsoft Docs
description: "Další informace o dobu potřebnou pro události vytváření sestav objeví na portálu Azure"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 9b88958d-94a2-4f4b-a18c-616f0617a24e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi;dhanyahk
ms.reviewer: dhanyahk
ms.openlocfilehash: 93cb0baeab8f13f81257ed1bd32ed08561c54b72
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-reporting-latencies"></a><span data-ttu-id="44b6f-103">Latence sestav Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="44b6f-103">Azure Active Directory reporting latencies</span></span>

<span data-ttu-id="44b6f-104">S [reporting](active-directory-preview-explainer.md) ve službě Azure Active Directory, získáte všechny informace, které potřebujete k určení, jak je to vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="44b6f-104">With [reporting](active-directory-preview-explainer.md) in the Azure Active Directory, you get all the information you need to determine how your environment is doing.</span></span> <span data-ttu-id="44b6f-105">Dobu potřebnou pro vytváření sestav dat objeví na portálu Azure se taky říká latence.</span><span class="sxs-lookup"><span data-stu-id="44b6f-105">The amount of time it takes for reporting data to show up in the Azure portal is also known as latency.</span></span> 

<span data-ttu-id="44b6f-106">Toto téma obsahuje informace o latenci pro všechny sestavy kategorií v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="44b6f-106">This topic lists the latency information for the all reporting categories in the Azure portal.</span></span> 


## <a name="activity-reports"></a><span data-ttu-id="44b6f-107">Sestavy aktivit</span><span class="sxs-lookup"><span data-stu-id="44b6f-107">Activity reports</span></span>

<span data-ttu-id="44b6f-108">Existují dvě oblasti aktivity generování sestav:</span><span class="sxs-lookup"><span data-stu-id="44b6f-108">There are two areas of activity reporting:</span></span>

- <span data-ttu-id="44b6f-109">**Aktivity přihlašování** – informace o použití spravovaných aplikací a aktivitách přihlašování uživatelů</span><span class="sxs-lookup"><span data-stu-id="44b6f-109">**Sign-in activities** – Information about the usage of managed applications and user sign-in activities</span></span>
- <span data-ttu-id="44b6f-110">**Protokoly auditu** – informace aktivit systému o správě uživatelů a skupin, spravovaných aplikacích a aktivitách adresářů</span><span class="sxs-lookup"><span data-stu-id="44b6f-110">**Audit logs** - System activity information about users and group management, your managed applications and directory activities</span></span>

<span data-ttu-id="44b6f-111">Následující tabulka uvádí informace o protokolování aktivit latenci.</span><span class="sxs-lookup"><span data-stu-id="44b6f-111">The following table lists the latency information for activity reports.</span></span>

| <span data-ttu-id="44b6f-112">Sestava</span><span class="sxs-lookup"><span data-stu-id="44b6f-112">Report</span></span> | <span data-ttu-id="44b6f-113">Minimální</span><span class="sxs-lookup"><span data-stu-id="44b6f-113">Minimum</span></span> | <span data-ttu-id="44b6f-114">Průměr</span><span class="sxs-lookup"><span data-stu-id="44b6f-114">Average</span></span> | <span data-ttu-id="44b6f-115">Maximální počet</span><span class="sxs-lookup"><span data-stu-id="44b6f-115">Maximum</span></span> |
| :-- | --- | --- | --- |
| <span data-ttu-id="44b6f-116">Protokoly auditu</span><span class="sxs-lookup"><span data-stu-id="44b6f-116">Audit logs</span></span>             | <span data-ttu-id="44b6f-117">30 minut</span><span class="sxs-lookup"><span data-stu-id="44b6f-117">30 minutes</span></span>  | <span data-ttu-id="44b6f-118">45 minut</span><span class="sxs-lookup"><span data-stu-id="44b6f-118">45 Minutes</span></span> | <span data-ttu-id="44b6f-119">1 hodina</span><span class="sxs-lookup"><span data-stu-id="44b6f-119">1 hour</span></span>     |
| <span data-ttu-id="44b6f-120">Přihlášení</span><span class="sxs-lookup"><span data-stu-id="44b6f-120">Sign-ins</span></span>               | <span data-ttu-id="44b6f-121">15 minut</span><span class="sxs-lookup"><span data-stu-id="44b6f-121">15 minutes</span></span>  | <span data-ttu-id="44b6f-122">15 minut</span><span class="sxs-lookup"><span data-stu-id="44b6f-122">15 minutes</span></span> | <span data-ttu-id="44b6f-123">2 hodiny *</span><span class="sxs-lookup"><span data-stu-id="44b6f-123">2 hours*</span></span>   |

>[!NOTE]
> <span data-ttu-id="44b6f-124">Pro některá data aktivit přihlášení pocházející z aplikací Office starší verze může zobrazení dat pro generování sestav trvat až 8 hodin.</span><span class="sxs-lookup"><span data-stu-id="44b6f-124">For some sign-ins activity data coming from legacy office applications, it can take to 8 hours for the reporting data to show up.</span></span> 


## <a name="security-reports"></a><span data-ttu-id="44b6f-125">Sestavy zabezpečení</span><span class="sxs-lookup"><span data-stu-id="44b6f-125">Security reports</span></span>

<span data-ttu-id="44b6f-126">Existují dvě oblasti generování sestav zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="44b6f-126">There are two areas of security reporting:</span></span>

- <span data-ttu-id="44b6f-127">**Riziková přihlášení** – Rizikové přihlášení je indikátorem pokusu o přihlášení, který mohl provést někdo, kdo není legitimním vlastníkem uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="44b6f-127">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not the legitimate owner of a user account.</span></span> 
- <span data-ttu-id="44b6f-128">**Uživatelé označení příznakem rizika** – Rizikový uživatel je indikátorem uživatelského účtu, který mohl být ohrožený.</span><span class="sxs-lookup"><span data-stu-id="44b6f-128">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> 

<span data-ttu-id="44b6f-129">Následující tabulka uvádí informace o zabezpečení sestavy latenci.</span><span class="sxs-lookup"><span data-stu-id="44b6f-129">The following table lists the latency information for security reports.</span></span>

| <span data-ttu-id="44b6f-130">Sestava</span><span class="sxs-lookup"><span data-stu-id="44b6f-130">Report</span></span> | <span data-ttu-id="44b6f-131">Minimální</span><span class="sxs-lookup"><span data-stu-id="44b6f-131">Minimum</span></span> | <span data-ttu-id="44b6f-132">Průměr</span><span class="sxs-lookup"><span data-stu-id="44b6f-132">Average</span></span> | <span data-ttu-id="44b6f-133">Maximální počet</span><span class="sxs-lookup"><span data-stu-id="44b6f-133">Maximum</span></span> |
| :-- | --- | --- | --- |
| <span data-ttu-id="44b6f-134">Ohrožení uživatelé</span><span class="sxs-lookup"><span data-stu-id="44b6f-134">Users at risk</span></span>          | <span data-ttu-id="44b6f-135">5 minut</span><span class="sxs-lookup"><span data-stu-id="44b6f-135">5 minutes</span></span>   | <span data-ttu-id="44b6f-136">15 minut</span><span class="sxs-lookup"><span data-stu-id="44b6f-136">15 minutes</span></span>  | <span data-ttu-id="44b6f-137">2 hodiny</span><span class="sxs-lookup"><span data-stu-id="44b6f-137">2 hours</span></span>  |
| <span data-ttu-id="44b6f-138">Rizikové přihlášení</span><span class="sxs-lookup"><span data-stu-id="44b6f-138">Risky sign-ins</span></span>         | <span data-ttu-id="44b6f-139">5 minut</span><span class="sxs-lookup"><span data-stu-id="44b6f-139">5 minutes</span></span>   | <span data-ttu-id="44b6f-140">15 minut</span><span class="sxs-lookup"><span data-stu-id="44b6f-140">15 minutes</span></span>  | <span data-ttu-id="44b6f-141">2 hodiny</span><span class="sxs-lookup"><span data-stu-id="44b6f-141">2 hours</span></span>  |

## <a name="risk-events"></a><span data-ttu-id="44b6f-142">Riziko události</span><span class="sxs-lookup"><span data-stu-id="44b6f-142">Risk events</span></span>

<span data-ttu-id="44b6f-143">Azure Active Directory používá algoritmy adaptivní strojového učení a heuristiky ke zjištění podezřelé akce, které se vztahují k vaší uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="44b6f-143">Azure Active Directory uses adaptive machine learning algorithms and heuristics to detect suspicious actions that are related to your user accounts.</span></span> <span data-ttu-id="44b6f-144">Všechny zjištěné podezřelé akce je uložený v události zavolat riziko záznamu.</span><span class="sxs-lookup"><span data-stu-id="44b6f-144">Each detected suspicious action is stored in a record called risk event.</span></span>

<span data-ttu-id="44b6f-145">Následující tabulka uvádí informace o rizikových událostech latenci.</span><span class="sxs-lookup"><span data-stu-id="44b6f-145">The following table lists the latency information for risk events.</span></span>

| <span data-ttu-id="44b6f-146">Sestava</span><span class="sxs-lookup"><span data-stu-id="44b6f-146">Report</span></span> | <span data-ttu-id="44b6f-147">Minimální</span><span class="sxs-lookup"><span data-stu-id="44b6f-147">Minimum</span></span> | <span data-ttu-id="44b6f-148">Průměr</span><span class="sxs-lookup"><span data-stu-id="44b6f-148">Average</span></span> | <span data-ttu-id="44b6f-149">Maximální počet</span><span class="sxs-lookup"><span data-stu-id="44b6f-149">Maximum</span></span> |
| :-- | --- | --- | --- |
| <span data-ttu-id="44b6f-150">Přihlášení z anonymních IP adres</span><span class="sxs-lookup"><span data-stu-id="44b6f-150">Sign-ins from anonymous IP addresses</span></span> |<span data-ttu-id="44b6f-151">5 minut</span><span class="sxs-lookup"><span data-stu-id="44b6f-151">5 minutes</span></span> |<span data-ttu-id="44b6f-152">15 minut</span><span class="sxs-lookup"><span data-stu-id="44b6f-152">15 Minutes</span></span> |<span data-ttu-id="44b6f-153">2 hodiny</span><span class="sxs-lookup"><span data-stu-id="44b6f-153">2 hours</span></span> |
| <span data-ttu-id="44b6f-154">Přihlášení z neznámých míst</span><span class="sxs-lookup"><span data-stu-id="44b6f-154">Sign-ins from unfamiliar locations</span></span> |<span data-ttu-id="44b6f-155">5 minut</span><span class="sxs-lookup"><span data-stu-id="44b6f-155">5 minutes</span></span> |<span data-ttu-id="44b6f-156">15 minut</span><span class="sxs-lookup"><span data-stu-id="44b6f-156">15 Minutes</span></span> |<span data-ttu-id="44b6f-157">2 hodiny</span><span class="sxs-lookup"><span data-stu-id="44b6f-157">2 hours</span></span> |
| <span data-ttu-id="44b6f-158">Uživatelé s uniklými přihlašovacími údaji</span><span class="sxs-lookup"><span data-stu-id="44b6f-158">Users with leaked credentials</span></span> |<span data-ttu-id="44b6f-159">2 hodiny</span><span class="sxs-lookup"><span data-stu-id="44b6f-159">2 hours</span></span> |<span data-ttu-id="44b6f-160">4 hodiny</span><span class="sxs-lookup"><span data-stu-id="44b6f-160">4 hours</span></span> |<span data-ttu-id="44b6f-161">8 hodin</span><span class="sxs-lookup"><span data-stu-id="44b6f-161">8 hours</span></span> |
| <span data-ttu-id="44b6f-162">Nemožná cesta do netypických míst</span><span class="sxs-lookup"><span data-stu-id="44b6f-162">Impossible travel to atypical locations</span></span> |<span data-ttu-id="44b6f-163">5 minut</span><span class="sxs-lookup"><span data-stu-id="44b6f-163">5 minutes</span></span> |<span data-ttu-id="44b6f-164">1 hodina</span><span class="sxs-lookup"><span data-stu-id="44b6f-164">1 hour</span></span> |<span data-ttu-id="44b6f-165">8 hodin</span><span class="sxs-lookup"><span data-stu-id="44b6f-165">8 hours</span></span>  |
| <span data-ttu-id="44b6f-166">Přihlášení z nakažených zařízení</span><span class="sxs-lookup"><span data-stu-id="44b6f-166">Sign-ins from infected devices</span></span> |<span data-ttu-id="44b6f-167">2 hodiny</span><span class="sxs-lookup"><span data-stu-id="44b6f-167">2 hours</span></span> |<span data-ttu-id="44b6f-168">4 hodiny</span><span class="sxs-lookup"><span data-stu-id="44b6f-168">4 hours</span></span> |<span data-ttu-id="44b6f-169">8 hodin</span><span class="sxs-lookup"><span data-stu-id="44b6f-169">8 hours</span></span>  |
| <span data-ttu-id="44b6f-170">Přihlášení z IP adres s podezřelou aktivitou</span><span class="sxs-lookup"><span data-stu-id="44b6f-170">Sign-ins from IP addresses with suspicious activity</span></span> |<span data-ttu-id="44b6f-171">2 hodiny</span><span class="sxs-lookup"><span data-stu-id="44b6f-171">2 hours</span></span> |<span data-ttu-id="44b6f-172">4 hodiny</span><span class="sxs-lookup"><span data-stu-id="44b6f-172">4 hours</span></span> |<span data-ttu-id="44b6f-173">8 hodin</span><span class="sxs-lookup"><span data-stu-id="44b6f-173">8 hours</span></span>  |



## <a name="next-steps"></a><span data-ttu-id="44b6f-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="44b6f-174">Next steps</span></span>

<span data-ttu-id="44b6f-175">Pokud chcete získat další informace o protokolování aktivit na portálu Azure, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="44b6f-175">If you want to know more about the activity reports in the Azure portal, see:</span></span>

- [<span data-ttu-id="44b6f-176">Přihlašovací aktivity sestav na portálu Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="44b6f-176">Sign-in activity reports in the Azure Active Directory portal</span></span>](active-directory-reporting-activity-sign-ins.md)
- [<span data-ttu-id="44b6f-177">Sestavy auditu aktivity na portálu Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="44b6f-177">Audit activity reports in the Azure Active Directory portal</span></span>](active-directory-reporting-activity-audit-logs.md)

<span data-ttu-id="44b6f-178">Pokud chcete získat další informace o zabezpečení sestav na portálu Azure, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="44b6f-178">If you want to know more about the security reports in the Azure portal, see:</span></span>

- [<span data-ttu-id="44b6f-179">Uživatelé na riziko zabezpečení sestav na portálu Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="44b6f-179">Users at risk security report in the Azure Active Directory portal</span></span>](active-directory-reporting-security-user-at-risk.md)
- [<span data-ttu-id="44b6f-180">Sestava rizikové přihlášení na portálu Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="44b6f-180">Risky sign-ins report in the Azure Active Directory portal</span></span>](active-directory-reporting-security-risky-sign-ins.md)

<span data-ttu-id="44b6f-181">Pokud chcete získat další informace o rizikových událostech, přečtěte si téma [Azure Active Directory rizikových událostech](active-directory-reporting-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="44b6f-181">If you want to know more about risk events, see [Azure Active Directory risk events](active-directory-reporting-risk-events.md).</span></span>
