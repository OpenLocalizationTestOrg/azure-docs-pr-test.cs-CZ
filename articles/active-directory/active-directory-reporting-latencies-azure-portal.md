---
title: "latence sestav služby Active Directory aaaAzure | Microsoft Docs"
description: "Další informace o hello dobu potřebnou pro vytváření sestav tooshow událostí až na portálu Azure"
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
ms.openlocfilehash: eee959331262ba59b313dd038cb54699dbef48a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting-latencies"></a><span data-ttu-id="d9600-103">Latence sestav Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d9600-103">Azure Active Directory reporting latencies</span></span>

<span data-ttu-id="d9600-104">S [reporting](active-directory-preview-explainer.md) v hello Azure Active Directory, získáte všechny hello informace, které potřebujete toodetermine úspěšnost prostředí.</span><span class="sxs-lookup"><span data-stu-id="d9600-104">With [reporting](active-directory-preview-explainer.md) in hello Azure Active Directory, you get all hello information you need toodetermine how your environment is doing.</span></span> <span data-ttu-id="d9600-105">Hello dobu potřebnou pro vytváření sestav, data tooshow nahoru v hello portál Azure je také označován jako latence.</span><span class="sxs-lookup"><span data-stu-id="d9600-105">hello amount of time it takes for reporting data tooshow up in hello Azure portal is also known as latency.</span></span> 

<span data-ttu-id="d9600-106">Toto téma obsahuje informace o hello latenci hello všech sestav kategorií v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d9600-106">This topic lists hello latency information for hello all reporting categories in hello Azure portal.</span></span> 


## <a name="activity-reports"></a><span data-ttu-id="d9600-107">Sestavy aktivit</span><span class="sxs-lookup"><span data-stu-id="d9600-107">Activity reports</span></span>

<span data-ttu-id="d9600-108">Existují dvě oblasti aktivity generování sestav:</span><span class="sxs-lookup"><span data-stu-id="d9600-108">There are two areas of activity reporting:</span></span>

- <span data-ttu-id="d9600-109">**Přihlašovací aktivity** – informace o využití hello spravovaných aplikací a aktivit přihlášení uživatelů</span><span class="sxs-lookup"><span data-stu-id="d9600-109">**Sign-in activities** – Information about hello usage of managed applications and user sign-in activities</span></span>
- <span data-ttu-id="d9600-110">**Protokoly auditu** – informace aktivit systému o správě uživatelů a skupin, spravovaných aplikacích a aktivitách adresářů</span><span class="sxs-lookup"><span data-stu-id="d9600-110">**Audit logs** - System activity information about users and group management, your managed applications and directory activities</span></span>

<span data-ttu-id="d9600-111">Hello následující tabulka uvádí informace o protokolování aktivit hello latenci.</span><span class="sxs-lookup"><span data-stu-id="d9600-111">hello following table lists hello latency information for activity reports.</span></span>

| <span data-ttu-id="d9600-112">Sestava</span><span class="sxs-lookup"><span data-stu-id="d9600-112">Report</span></span> | <span data-ttu-id="d9600-113">Minimální</span><span class="sxs-lookup"><span data-stu-id="d9600-113">Minimum</span></span> | <span data-ttu-id="d9600-114">Průměr</span><span class="sxs-lookup"><span data-stu-id="d9600-114">Average</span></span> | <span data-ttu-id="d9600-115">Maximální počet</span><span class="sxs-lookup"><span data-stu-id="d9600-115">Maximum</span></span> |
| :-- | --- | --- | --- |
| <span data-ttu-id="d9600-116">Protokoly auditu</span><span class="sxs-lookup"><span data-stu-id="d9600-116">Audit logs</span></span>             | <span data-ttu-id="d9600-117">30 minut</span><span class="sxs-lookup"><span data-stu-id="d9600-117">30 minutes</span></span>  | <span data-ttu-id="d9600-118">45 minut</span><span class="sxs-lookup"><span data-stu-id="d9600-118">45 Minutes</span></span> | <span data-ttu-id="d9600-119">1 hodina</span><span class="sxs-lookup"><span data-stu-id="d9600-119">1 hour</span></span>     |
| <span data-ttu-id="d9600-120">Přihlášení</span><span class="sxs-lookup"><span data-stu-id="d9600-120">Sign-ins</span></span>               | <span data-ttu-id="d9600-121">15 minut</span><span class="sxs-lookup"><span data-stu-id="d9600-121">15 minutes</span></span>  | <span data-ttu-id="d9600-122">15 minut</span><span class="sxs-lookup"><span data-stu-id="d9600-122">15 minutes</span></span> | <span data-ttu-id="d9600-123">2 hodiny *</span><span class="sxs-lookup"><span data-stu-id="d9600-123">2 hours*</span></span>   |

>[!NOTE]
> <span data-ttu-id="d9600-124">Pro některá data aktivity přihlášení pocházejících z aplikace office starší verze může trvat too8 hodin hello generování sestav dat tooshow nahoru.</span><span class="sxs-lookup"><span data-stu-id="d9600-124">For some sign-ins activity data coming from legacy office applications, it can take too8 hours for hello reporting data tooshow up.</span></span> 


## <a name="security-reports"></a><span data-ttu-id="d9600-125">Sestavy zabezpečení</span><span class="sxs-lookup"><span data-stu-id="d9600-125">Security reports</span></span>

<span data-ttu-id="d9600-126">Existují dvě oblasti generování sestav zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="d9600-126">There are two areas of security reporting:</span></span>

- <span data-ttu-id="d9600-127">**Rizikové přihlášení** -rizikové přihlášení je indikátorem pro pokusu přihlášení, který se možná prováděly uživatelem, který není vlastníkem legitimní hello uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="d9600-127">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not hello legitimate owner of a user account.</span></span> 
- <span data-ttu-id="d9600-128">**Uživatelé označení příznakem rizika** – Rizikový uživatel je indikátorem uživatelského účtu, který mohl být ohrožený.</span><span class="sxs-lookup"><span data-stu-id="d9600-128">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> 

<span data-ttu-id="d9600-129">Hello následující tabulka uvádí informace o zabezpečení sestavy hello latenci.</span><span class="sxs-lookup"><span data-stu-id="d9600-129">hello following table lists hello latency information for security reports.</span></span>

| <span data-ttu-id="d9600-130">Sestava</span><span class="sxs-lookup"><span data-stu-id="d9600-130">Report</span></span> | <span data-ttu-id="d9600-131">Minimální</span><span class="sxs-lookup"><span data-stu-id="d9600-131">Minimum</span></span> | <span data-ttu-id="d9600-132">Průměr</span><span class="sxs-lookup"><span data-stu-id="d9600-132">Average</span></span> | <span data-ttu-id="d9600-133">Maximální počet</span><span class="sxs-lookup"><span data-stu-id="d9600-133">Maximum</span></span> |
| :-- | --- | --- | --- |
| <span data-ttu-id="d9600-134">Ohrožení uživatelé</span><span class="sxs-lookup"><span data-stu-id="d9600-134">Users at risk</span></span>          | <span data-ttu-id="d9600-135">5 minut</span><span class="sxs-lookup"><span data-stu-id="d9600-135">5 minutes</span></span>   | <span data-ttu-id="d9600-136">15 minut</span><span class="sxs-lookup"><span data-stu-id="d9600-136">15 minutes</span></span>  | <span data-ttu-id="d9600-137">2 hodiny</span><span class="sxs-lookup"><span data-stu-id="d9600-137">2 hours</span></span>  |
| <span data-ttu-id="d9600-138">Rizikové přihlášení</span><span class="sxs-lookup"><span data-stu-id="d9600-138">Risky sign-ins</span></span>         | <span data-ttu-id="d9600-139">5 minut</span><span class="sxs-lookup"><span data-stu-id="d9600-139">5 minutes</span></span>   | <span data-ttu-id="d9600-140">15 minut</span><span class="sxs-lookup"><span data-stu-id="d9600-140">15 minutes</span></span>  | <span data-ttu-id="d9600-141">2 hodiny</span><span class="sxs-lookup"><span data-stu-id="d9600-141">2 hours</span></span>  |

## <a name="risk-events"></a><span data-ttu-id="d9600-142">Riziko události</span><span class="sxs-lookup"><span data-stu-id="d9600-142">Risk events</span></span>

<span data-ttu-id="d9600-143">Azure Active Directory používá adaptivní strojového učení algoritmů a heuristiky podezřelé akce toodetect, které jsou související tooyour uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="d9600-143">Azure Active Directory uses adaptive machine learning algorithms and heuristics toodetect suspicious actions that are related tooyour user accounts.</span></span> <span data-ttu-id="d9600-144">Všechny zjištěné podezřelé akce je uložený v události zavolat riziko záznamu.</span><span class="sxs-lookup"><span data-stu-id="d9600-144">Each detected suspicious action is stored in a record called risk event.</span></span>

<span data-ttu-id="d9600-145">Hello následující tabulka uvádí informace o rizikových událostech hello latenci.</span><span class="sxs-lookup"><span data-stu-id="d9600-145">hello following table lists hello latency information for risk events.</span></span>

| <span data-ttu-id="d9600-146">Sestava</span><span class="sxs-lookup"><span data-stu-id="d9600-146">Report</span></span> | <span data-ttu-id="d9600-147">Minimální</span><span class="sxs-lookup"><span data-stu-id="d9600-147">Minimum</span></span> | <span data-ttu-id="d9600-148">Průměr</span><span class="sxs-lookup"><span data-stu-id="d9600-148">Average</span></span> | <span data-ttu-id="d9600-149">Maximální počet</span><span class="sxs-lookup"><span data-stu-id="d9600-149">Maximum</span></span> |
| :-- | --- | --- | --- |
| <span data-ttu-id="d9600-150">Přihlášení z anonymních IP adres</span><span class="sxs-lookup"><span data-stu-id="d9600-150">Sign-ins from anonymous IP addresses</span></span> |<span data-ttu-id="d9600-151">5 minut</span><span class="sxs-lookup"><span data-stu-id="d9600-151">5 minutes</span></span> |<span data-ttu-id="d9600-152">15 minut</span><span class="sxs-lookup"><span data-stu-id="d9600-152">15 Minutes</span></span> |<span data-ttu-id="d9600-153">2 hodiny</span><span class="sxs-lookup"><span data-stu-id="d9600-153">2 hours</span></span> |
| <span data-ttu-id="d9600-154">Přihlášení z neznámých míst</span><span class="sxs-lookup"><span data-stu-id="d9600-154">Sign-ins from unfamiliar locations</span></span> |<span data-ttu-id="d9600-155">5 minut</span><span class="sxs-lookup"><span data-stu-id="d9600-155">5 minutes</span></span> |<span data-ttu-id="d9600-156">15 minut</span><span class="sxs-lookup"><span data-stu-id="d9600-156">15 Minutes</span></span> |<span data-ttu-id="d9600-157">2 hodiny</span><span class="sxs-lookup"><span data-stu-id="d9600-157">2 hours</span></span> |
| <span data-ttu-id="d9600-158">Uživatelé s uniklými přihlašovacími údaji</span><span class="sxs-lookup"><span data-stu-id="d9600-158">Users with leaked credentials</span></span> |<span data-ttu-id="d9600-159">2 hodiny</span><span class="sxs-lookup"><span data-stu-id="d9600-159">2 hours</span></span> |<span data-ttu-id="d9600-160">4 hodiny</span><span class="sxs-lookup"><span data-stu-id="d9600-160">4 hours</span></span> |<span data-ttu-id="d9600-161">8 hodin</span><span class="sxs-lookup"><span data-stu-id="d9600-161">8 hours</span></span> |
| <span data-ttu-id="d9600-162">Neuskutečnitelná cesta tooatypical umístění</span><span class="sxs-lookup"><span data-stu-id="d9600-162">Impossible travel tooatypical locations</span></span> |<span data-ttu-id="d9600-163">5 minut</span><span class="sxs-lookup"><span data-stu-id="d9600-163">5 minutes</span></span> |<span data-ttu-id="d9600-164">1 hodina</span><span class="sxs-lookup"><span data-stu-id="d9600-164">1 hour</span></span> |<span data-ttu-id="d9600-165">8 hodin</span><span class="sxs-lookup"><span data-stu-id="d9600-165">8 hours</span></span>  |
| <span data-ttu-id="d9600-166">Přihlášení z nakažených zařízení</span><span class="sxs-lookup"><span data-stu-id="d9600-166">Sign-ins from infected devices</span></span> |<span data-ttu-id="d9600-167">2 hodiny</span><span class="sxs-lookup"><span data-stu-id="d9600-167">2 hours</span></span> |<span data-ttu-id="d9600-168">4 hodiny</span><span class="sxs-lookup"><span data-stu-id="d9600-168">4 hours</span></span> |<span data-ttu-id="d9600-169">8 hodin</span><span class="sxs-lookup"><span data-stu-id="d9600-169">8 hours</span></span>  |
| <span data-ttu-id="d9600-170">Přihlášení z IP adres s podezřelou aktivitou</span><span class="sxs-lookup"><span data-stu-id="d9600-170">Sign-ins from IP addresses with suspicious activity</span></span> |<span data-ttu-id="d9600-171">2 hodiny</span><span class="sxs-lookup"><span data-stu-id="d9600-171">2 hours</span></span> |<span data-ttu-id="d9600-172">4 hodiny</span><span class="sxs-lookup"><span data-stu-id="d9600-172">4 hours</span></span> |<span data-ttu-id="d9600-173">8 hodin</span><span class="sxs-lookup"><span data-stu-id="d9600-173">8 hours</span></span>  |



## <a name="next-steps"></a><span data-ttu-id="d9600-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d9600-174">Next steps</span></span>

<span data-ttu-id="d9600-175">Pokud chcete tooknow Další informace o sestavách hello aktivity v hello portálu Azure, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="d9600-175">If you want tooknow more about hello activity reports in hello Azure portal, see:</span></span>

- [<span data-ttu-id="d9600-176">Přihlašovací aktivity sestav na portálu Azure Active Directory hello</span><span class="sxs-lookup"><span data-stu-id="d9600-176">Sign-in activity reports in hello Azure Active Directory portal</span></span>](active-directory-reporting-activity-sign-ins.md)
- [<span data-ttu-id="d9600-177">Audit aktivity sestav na portálu Azure Active Directory hello</span><span class="sxs-lookup"><span data-stu-id="d9600-177">Audit activity reports in hello Azure Active Directory portal</span></span>](active-directory-reporting-activity-audit-logs.md)

<span data-ttu-id="d9600-178">Pokud chcete tooknow Další informace o sestavách hello zabezpečení v hello portálu Azure, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="d9600-178">If you want tooknow more about hello security reports in hello Azure portal, see:</span></span>

- [<span data-ttu-id="d9600-179">Uživatelé na riziko zabezpečení sestav na portálu Azure Active Directory hello</span><span class="sxs-lookup"><span data-stu-id="d9600-179">Users at risk security report in hello Azure Active Directory portal</span></span>](active-directory-reporting-security-user-at-risk.md)
- [<span data-ttu-id="d9600-180">Sestava rizikové přihlášení na portálu Azure Active Directory hello</span><span class="sxs-lookup"><span data-stu-id="d9600-180">Risky sign-ins report in hello Azure Active Directory portal</span></span>](active-directory-reporting-security-risky-sign-ins.md)

<span data-ttu-id="d9600-181">Pokud chcete více informací o rizikových událostech tooknow, najdete v části [Azure Active Directory rizikových událostech](active-directory-reporting-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="d9600-181">If you want tooknow more about risk events, see [Azure Active Directory risk events](active-directory-reporting-risk-events.md).</span></span>
