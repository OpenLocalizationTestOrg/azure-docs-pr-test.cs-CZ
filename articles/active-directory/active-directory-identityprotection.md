---
title: Ochrany identit Azure Active Directory | Microsoft Docs
description: "Zjistěte, jak Azure AD Identity Protection umožňuje omezit možnost útočník zneužít ohroženými identity nebo zařízení a zabezpečit identity nebo zařízení, která byla dříve by mohly vzbuzovat podezření nebo známé došlo k narušení."
services: active-directory
keywords: "ochrany identit Azure active directory, cloud app discovery,. Správa aplikací, zabezpečení, rizik, úroveň rizika, ohrožení zabezpečení, zásady zabezpečení"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 0c7a8d68c0df729441e3f7faa5cd06066db1261d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-identity-protection"></a><span data-ttu-id="7ae87-104">Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="7ae87-104">Azure Active Directory Identity Protection</span></span>

<span data-ttu-id="7ae87-105">Azure Active Directory ochrany identit je funkce služby Azure AD Premium P2 edici, která vám umožní:</span><span class="sxs-lookup"><span data-stu-id="7ae87-105">Azure Active Directory Identity Protection is a feature of the Azure AD Premium P2 edition that enables you to:</span></span>

- <span data-ttu-id="7ae87-106">Zjistit potenciální ohrožení zabezpečení, které ovlivňují identity ve vaší organizaci</span><span class="sxs-lookup"><span data-stu-id="7ae87-106">Detect potential vulnerabilities affecting your organization’s identities</span></span>

- <span data-ttu-id="7ae87-107">Nakonfigurovat automatické odpovědi na zjištěné podezřelé akce, které se vztahují k identity ve vaší organizaci</span><span class="sxs-lookup"><span data-stu-id="7ae87-107">Configure automated responses to detected suspicious actions that are related to your organization’s identities</span></span>  

- <span data-ttu-id="7ae87-108">Prozkoumat podezřelé incidenty a proveďte příslušné akce jejich řešení</span><span class="sxs-lookup"><span data-stu-id="7ae87-108">Investigate suspicious incidents and take appropriate action to resolve them</span></span>   


## <a name="getting-started"></a><span data-ttu-id="7ae87-109">Začínáme</span><span class="sxs-lookup"><span data-stu-id="7ae87-109">Getting started</span></span>

<span data-ttu-id="7ae87-110">Microsoft zabezpečuje cloudové identity pro více než deset.</span><span class="sxs-lookup"><span data-stu-id="7ae87-110">Microsoft secures cloud-based identities for more than a decade.</span></span> <span data-ttu-id="7ae87-111">S Azure Active Directory Identity Protection ve vašem prostředí, můžete použít stejné ochrany systémů, které společnost Microsoft používá k zabezpečení identity.</span><span class="sxs-lookup"><span data-stu-id="7ae87-111">With Azure Active Directory Identity Protection, in your environment, you can use the same protection systems Microsoft uses to secure identities.</span></span>

<span data-ttu-id="7ae87-112">Velká většina narušení zabezpečení se provede při útočník přístupu do prostředí tak, že krádež identity uživatele.</span><span class="sxs-lookup"><span data-stu-id="7ae87-112">The vast majority of security breaches take place when attackers gain access to an environment by stealing a user’s identity.</span></span> <span data-ttu-id="7ae87-113">V průběhu let se staly útočníci stále efektivní využívání narušení třetích stran a za použití útoky phishing sofistikované.</span><span class="sxs-lookup"><span data-stu-id="7ae87-113">Over the years, attackers have become increasingly effective in leveraging third party breaches and using sophisticated phishing attacks.</span></span> <span data-ttu-id="7ae87-114">Jakmile útočník získá přístup k i nízkou privilegované uživatelských účtů, je pro ně k získání přístupu k prostředkům společnosti důležité prostřednictvím laterální pohyb je poměrně snadné.</span><span class="sxs-lookup"><span data-stu-id="7ae87-114">As soon as an attacker gains access to even low privileged user accounts, it is relatively easy for them to gain access to important company resources through lateral movement.</span></span>

<span data-ttu-id="7ae87-115">V důsledku toho je potřeba:</span><span class="sxs-lookup"><span data-stu-id="7ae87-115">As a consequence of this, you need to:</span></span>

- <span data-ttu-id="7ae87-116">Chránit všechny identity, bez ohledu na jejich úroveň oprávnění</span><span class="sxs-lookup"><span data-stu-id="7ae87-116">Protect all identities regardless of their privilege level</span></span>

- <span data-ttu-id="7ae87-117">Proaktivní ohroženými identity zabránit se překročen</span><span class="sxs-lookup"><span data-stu-id="7ae87-117">Proactively prevent compromised identities from being abused</span></span>

<span data-ttu-id="7ae87-118">Zjišťování ohrožení zabezpečení identity je bez snadno úlohy.</span><span class="sxs-lookup"><span data-stu-id="7ae87-118">Discovering compromised identities is no easy task.</span></span> <span data-ttu-id="7ae87-119">Azure Active Directory používá algoritmy adaptivní strojového učení a heuristiky zjištění anomálií a podezřelé incidenty, které indikují potenciálně ohrožených identity.</span><span class="sxs-lookup"><span data-stu-id="7ae87-119">Azure Active Directory uses adaptive machine learning algorithms and heuristics to detect anomalies and suspicious incidents that indicate potentially compromised identities.</span></span> <span data-ttu-id="7ae87-120">Na základě těchto dat Identity Protection generuje sestavy a výstrahy, které umožňují vyhodnotit zjištěné problémy a proveďte příslušné zmírnění nebo nápravné akce.</span><span class="sxs-lookup"><span data-stu-id="7ae87-120">Using this data, Identity Protection generates reports and alerts that enable you to evaluate the detected issues and take appropriate mitigation or remediation actions.</span></span>

<span data-ttu-id="7ae87-121">Azure Active Directory Identity Protection je větší než monitorování a vytváření sestav nástroje.</span><span class="sxs-lookup"><span data-stu-id="7ae87-121">Azure Active Directory Identity Protection is more than a monitoring and reporting tool.</span></span> <span data-ttu-id="7ae87-122">Pokud chcete ochránit identity ve vaší organizaci, můžete nakonfigurovat zásady na základě rizik, které automaticky reagovat na zjištěné problémy, pokud bylo dosaženo úroveň zadaný rizika.</span><span class="sxs-lookup"><span data-stu-id="7ae87-122">To protect your organization's identities, you can configure risk-based policies that automatically respond to detected issues when a specified risk level has been reached.</span></span> <span data-ttu-id="7ae87-123">Tyto zásady, kromě jiných ovládacích prvků podmíněného přístupu poskytuje Azure Active Directory a EMS, můžete buď automaticky blokovat nebo zahájit adaptivní nápravných akcí, které resetuje včetně heslo a vynucování služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="7ae87-123">These policies, in addition to other conditional access controls provided by Azure Active Directory and EMS, can either automatically block or initiate adaptive remediation actions including password resets and multi-factor authentication enforcement.</span></span>


#### <a name="identity-protection-capabilities"></a><span data-ttu-id="7ae87-124">Funkce ochrany identit</span><span class="sxs-lookup"><span data-stu-id="7ae87-124">Identity Protection capabilities</span></span>

<span data-ttu-id="7ae87-125">**Zjišťování ohrožení zabezpečení a rizikové účty:**</span><span class="sxs-lookup"><span data-stu-id="7ae87-125">**Detecting vulnerabilities and risky accounts:**</span></span>  

* <span data-ttu-id="7ae87-126">Poskytování vlastních doporučení pro zlepšení celkové postavení zabezpečení podle zvýraznění ohrožení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="7ae87-126">Providing custom recommendations to improve overall security posture by highlighting vulnerabilities</span></span>
* <span data-ttu-id="7ae87-127">Výpočet úrovní rizika přihlášení</span><span class="sxs-lookup"><span data-stu-id="7ae87-127">Calculating sign-in risk levels</span></span>
* <span data-ttu-id="7ae87-128">Výpočet úrovní rizika uživatele</span><span class="sxs-lookup"><span data-stu-id="7ae87-128">Calculating user risk levels</span></span>


<span data-ttu-id="7ae87-129">**Zkoumání rizikových událostí:**</span><span class="sxs-lookup"><span data-stu-id="7ae87-129">**Investigating risk events:**</span></span>

* <span data-ttu-id="7ae87-130">Odesílání oznámení o rizikových událostech</span><span class="sxs-lookup"><span data-stu-id="7ae87-130">Sending notifications for risk events</span></span>
* <span data-ttu-id="7ae87-131">Zkoumání rizikových událostí pomocí relevantní a kontextové informace</span><span class="sxs-lookup"><span data-stu-id="7ae87-131">Investigating risk events using relevant and contextual information</span></span>
* <span data-ttu-id="7ae87-132">Poskytuje základní pracovní postupy pro sledování šetření</span><span class="sxs-lookup"><span data-stu-id="7ae87-132">Providing basic workflows to track investigations</span></span>
* <span data-ttu-id="7ae87-133">Poskytuje snadný přístup k nápravné akce, jako je například resetování hesla</span><span class="sxs-lookup"><span data-stu-id="7ae87-133">Providing easy access to remediation actions such as password reset</span></span>

<span data-ttu-id="7ae87-134">**Zásady podmíněného přístupu na základě rizika:**</span><span class="sxs-lookup"><span data-stu-id="7ae87-134">**Risk-based conditional access policies:**</span></span>

* <span data-ttu-id="7ae87-135">Zásady pro zmírnění rizikové přihlášení blokování přihlášení nebo že vyřeší problémy spojené služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="7ae87-135">Policy to mitigate risky sign-ins by blocking sign-ins or requiring multi-factor authentication challenges.</span></span>
* <span data-ttu-id="7ae87-136">Zásady na blokování nebo zabezpečený rizikové uživatelské účty</span><span class="sxs-lookup"><span data-stu-id="7ae87-136">Policy to block or secure risky user accounts</span></span>
* <span data-ttu-id="7ae87-137">Zásady budou muset uživatelé zaregistrovat u služby Multi-Factor authentication</span><span class="sxs-lookup"><span data-stu-id="7ae87-137">Policy to require users to register for multi-factor authentication</span></span>



## <a name="identity-protection-roles"></a><span data-ttu-id="7ae87-138">Role ochranu identity</span><span class="sxs-lookup"><span data-stu-id="7ae87-138">Identity Protection roles</span></span>

<span data-ttu-id="7ae87-139">Pro vyrovnávání zatížení činnosti správy kolem implementaci Identity Protection můžete přiřadit několik rolí.</span><span class="sxs-lookup"><span data-stu-id="7ae87-139">To load balance the management activities around your Identity Protection implementation, you can assign several roles.</span></span> <span data-ttu-id="7ae87-140">Azure AD Identity Protection podporuje 3 directory role:</span><span class="sxs-lookup"><span data-stu-id="7ae87-140">Azure AD Identity Protection supports 3 directory roles:</span></span>

| <span data-ttu-id="7ae87-141">Role</span><span class="sxs-lookup"><span data-stu-id="7ae87-141">Role</span></span>                         | <span data-ttu-id="7ae87-142">Můžete provést</span><span class="sxs-lookup"><span data-stu-id="7ae87-142">Can do</span></span>                          | <span data-ttu-id="7ae87-143">Nelze provést</span><span class="sxs-lookup"><span data-stu-id="7ae87-143">Cannot do</span></span>
| :--                          | ---                                |  ---   |
| <span data-ttu-id="7ae87-144">Globální správce</span><span class="sxs-lookup"><span data-stu-id="7ae87-144">Global administrator</span></span>         | <span data-ttu-id="7ae87-145">Úplný přístup k ochraně Identity, zařadit ochranu Identity</span><span class="sxs-lookup"><span data-stu-id="7ae87-145">Full access to Identity Protection, Onboard Identity Protection</span></span>| |
| <span data-ttu-id="7ae87-146">Správce zabezpečení</span><span class="sxs-lookup"><span data-stu-id="7ae87-146">Security administrator</span></span>       | <span data-ttu-id="7ae87-147">Úplný přístup k Identity Protection</span><span class="sxs-lookup"><span data-stu-id="7ae87-147">Full access to Identity Protection</span></span> | <span data-ttu-id="7ae87-148">Zařadit Identity Protection resetovat hesla pro uživatele</span><span class="sxs-lookup"><span data-stu-id="7ae87-148">Onboard Identity Protection,  reset passwords for a user</span></span> |
| <span data-ttu-id="7ae87-149">Čtenář zabezpečení</span><span class="sxs-lookup"><span data-stu-id="7ae87-149">Security reader</span></span>              | <span data-ttu-id="7ae87-150">Přístup jen připravené k Identity Protection</span><span class="sxs-lookup"><span data-stu-id="7ae87-150">Ready-only access to Identity Protection</span></span> | <span data-ttu-id="7ae87-151">Zařadit Identity Protection, uživatelé remidiate, nakonfigurovat zásady, resetování hesla</span><span class="sxs-lookup"><span data-stu-id="7ae87-151">Onboard Identity Protection, remidiate users, configure policies,  reset passwords</span></span> |




<span data-ttu-id="7ae87-152">Další podrobnosti najdete v tématu [přiřazení rolí správce v Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="7ae87-152">For more details, see [Assigning administrator roles in Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md)</span></span>



## <a name="detection"></a><span data-ttu-id="7ae87-153">Detection (Detekce)</span><span class="sxs-lookup"><span data-stu-id="7ae87-153">Detection</span></span>

### <a name="vulnerabilities"></a><span data-ttu-id="7ae87-154">Ohrožení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="7ae87-154">Vulnerabilities</span></span>

<span data-ttu-id="7ae87-155">Azure Active Directory Identity Protection analýz konfiguraci a zjistí chyby zabezpečení, které můžou mít vliv na identit uživatelů.</span><span class="sxs-lookup"><span data-stu-id="7ae87-155">Azure Active Directory Identity Protection analyses your configuration and detects vulnerabilities that can have an impact on your user's identities.</span></span> <span data-ttu-id="7ae87-156">Další podrobnosti najdete v tématu [chyb zabezpečení detekovaných službou Azure Active Directory Identity Protection](active-directory-identityprotection-vulnerabilities.md).</span><span class="sxs-lookup"><span data-stu-id="7ae87-156">For more details, see [Vulnerabilities detected by Azure Active Directory Identity Protection](active-directory-identityprotection-vulnerabilities.md).</span></span>

### <a name="risk-events"></a><span data-ttu-id="7ae87-157">Riziko události</span><span class="sxs-lookup"><span data-stu-id="7ae87-157">Risk events</span></span>

<span data-ttu-id="7ae87-158">Azure Active Directory používá algoritmy adaptivní strojového učení a heuristiky ke zjištění podezřelé akce, které se vztahují k identit uživatelů.</span><span class="sxs-lookup"><span data-stu-id="7ae87-158">Azure Active Directory uses adaptive machine learning algorithms and heuristics to detect suspicious actions that are related to your user's identities.</span></span> <span data-ttu-id="7ae87-159">Systém vytvoří záznam pro každé zjištěné podezřelé akce.</span><span class="sxs-lookup"><span data-stu-id="7ae87-159">The system creates a record for each detected suspicious action.</span></span> <span data-ttu-id="7ae87-160">Tyto záznamy se také označují jako rizikových událostech.</span><span class="sxs-lookup"><span data-stu-id="7ae87-160">These records are also known as risk events.</span></span>  
<span data-ttu-id="7ae87-161">Další podrobnosti najdete v tématu věnovaném [rizikovým událostem služby Azure Active Directory](active-directory-identity-protection-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="7ae87-161">For more details, see [Azure Active Directory risk events](active-directory-identity-protection-risk-events.md).</span></span>


## <a name="investigation"></a><span data-ttu-id="7ae87-162">Šetření</span><span class="sxs-lookup"><span data-stu-id="7ae87-162">Investigation</span></span>
<span data-ttu-id="7ae87-163">Vaše cesty přes Identity Protection obvykle začíná řídicího panelu ochrany identit.</span><span class="sxs-lookup"><span data-stu-id="7ae87-163">Your journey through Identity Protection typically starts with the Identity Protection dashboard.</span></span>

<span data-ttu-id="7ae87-164">![Náprava](./media/active-directory-identityprotection/1000.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="7ae87-164">![Remediation](./media/active-directory-identityprotection/1000.png "Remediation")</span></span>

<span data-ttu-id="7ae87-165">Řídicí panel poskytuje přístup k:</span><span class="sxs-lookup"><span data-stu-id="7ae87-165">The dashboard gives you access to:</span></span>

* <span data-ttu-id="7ae87-166">Sestavy, jako **uživatelé označení příznakem rizik**, **rizik události** a **ohrožení zabezpečení**</span><span class="sxs-lookup"><span data-stu-id="7ae87-166">Reports such as **Users flagged for risk**, **Risk events** and **Vulnerabilities**</span></span>
* <span data-ttu-id="7ae87-167">Nastavení, jako například konfigurace vaší **zásady zabezpečení**, **oznámení** a **registrace služby Multi-Factor authentication**</span><span class="sxs-lookup"><span data-stu-id="7ae87-167">Settings such as the configuration of your **Security Policies**, **Notifications** and **multi-factor authentication registration**</span></span>

<span data-ttu-id="7ae87-168">Je obvykle počáteční bod pro šetření, což je proces kontroly aktivity, protokoly a další důležité informace vztahující se k události riziko rozhodnout, zda jsou potřebné kroky ke zmírnění nebo nápravy, a jak se identita dojde k ohrožení a pochopit použití ohroženými identity.</span><span class="sxs-lookup"><span data-stu-id="7ae87-168">It is typically your starting point for investigation, which is the process of reviewing the activities, logs, and other relevant information related to a risk event to decide whether remediation or mitigation steps are necessary,  and how the identity was compromised, and understand how the compromised identity was used.</span></span>

<span data-ttu-id="7ae87-169">Dokáže spojit vaše aktivity šetření a [oznámení](active-directory-identityprotection-notifications.md) Azure Active Directory Protection odešle na e-mailu.</span><span class="sxs-lookup"><span data-stu-id="7ae87-169">You can tie your investigation activities to the [notifications](active-directory-identityprotection-notifications.md) Azure Active Directory Protection sends per email.</span></span>

<span data-ttu-id="7ae87-170">Následující části poskytují další podrobnosti a kroky, které se vztahují k vyšetřování.</span><span class="sxs-lookup"><span data-stu-id="7ae87-170">The following sections provide you with more details and the steps that are related to an investigation.</span></span>  


## <a name="risky-sign-ins"></a><span data-ttu-id="7ae87-171">Rizikové přihlášení</span><span class="sxs-lookup"><span data-stu-id="7ae87-171">Risky sign-ins</span></span>

<span data-ttu-id="7ae87-172">Azure Active Directory zjistí [rizik typů událostí](active-directory-reporting-risk-events.md#risk-event-types) v reálném čase a offline.</span><span class="sxs-lookup"><span data-stu-id="7ae87-172">Azure Active Directory detects [risk event types](active-directory-reporting-risk-events.md#risk-event-types) in real-time and offline.</span></span> <span data-ttu-id="7ae87-173">Každý riziko událost, která byla zjištěna u přihlášení uživatele přispívá k logický pojem, který volá rizikové přihlášení.</span><span class="sxs-lookup"><span data-stu-id="7ae87-173">Each risk event that has been detected for a sign-in of a user contributes to a logical concept called risky sign-in.</span></span> <span data-ttu-id="7ae87-174">Rizikové přihlášení je indikátorem pro pokusu přihlášení, který nemusí provedly legitimní vlastníkem uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="7ae87-174">A risky sign-in is an indicator for a sign-in attempt that might not have been performed by the legitimate owner of a user account.</span></span>


### <a name="sign-in-risk-level"></a><span data-ttu-id="7ae87-175">Úroveň rizika přihlášení</span><span class="sxs-lookup"><span data-stu-id="7ae87-175">Sign-in risk level</span></span>

<span data-ttu-id="7ae87-176">Úroveň rizika přihlášení je označením (vysoká, střední nebo nízká) pravděpodobnost, že pokus o přihlášení nebyla provedena legitimní vlastníkem uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="7ae87-176">A sign-in risk level is an indication (High, Medium, or Low) of the likelihood that a sign-in attempt was not performed by the legitimate owner of a user account.</span></span>

### <a name="mitigating-sign-in-risk-events"></a><span data-ttu-id="7ae87-177">Zmírnění rizika přihlašovací události</span><span class="sxs-lookup"><span data-stu-id="7ae87-177">Mitigating sign-in risk events</span></span>

<span data-ttu-id="7ae87-178">Zmírnění je akci, kterou chcete omezit schopnost útočník zneužít ohroženými identity nebo zařízení bez obnovení identity nebo zařízení do bezpečného stavu.</span><span class="sxs-lookup"><span data-stu-id="7ae87-178">A mitigation is an action to limit the ability of an attacker to exploit a compromised identity or device without restoring the identity or device to a safe state.</span></span> <span data-ttu-id="7ae87-179">Zmírnění nevyřeší předchozí přihlášení rizikových událostech spojených s identity nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="7ae87-179">A mitigation does not resolve previous sign-in risk events associated with the identity or device.</span></span>

<span data-ttu-id="7ae87-180">Pro zmírnění rizikové přihlášení automaticky, můžete nakonfigurovat policicies přihlášení riziko zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="7ae87-180">To mitigate risky sign-ins automatically, you can configure sign-in risk security policicies.</span></span> <span data-ttu-id="7ae87-181">Pomocí těchto zásad, zvažte úroveň rizika uživatele nebo přihlášení k blokování rizikové přihlášení nebo vyžadovat, aby uživatel k provedení ověřování Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="7ae87-181">Using these policies, you consider the risk level of the user or the sign-in to block risky sign-ins or require the user to perform multi-factor authentication.</span></span> <span data-ttu-id="7ae87-182">Tato akce může zabránit útočníkům ve využívání odcizené identity způsobit poškození a může získáte chvíli k zabezpečení identity.</span><span class="sxs-lookup"><span data-stu-id="7ae87-182">These actions may prevent an attacker from exploiting a stolen identity to cause damage, and may give you some time to secure the identity.</span></span>

### <a name="sign-in-risk-security-policy"></a><span data-ttu-id="7ae87-183">Zásady zabezpečení riziko přihlášení</span><span class="sxs-lookup"><span data-stu-id="7ae87-183">Sign-in risk security policy</span></span>
<span data-ttu-id="7ae87-184">Zásady přihlášení riziko je zásadu podmíněného přístupu, která vyhodnotí riziko pro konkrétní přihlášení a použije způsoby zmírnění rizik na základě předem definované podmínky a pravidla.</span><span class="sxs-lookup"><span data-stu-id="7ae87-184">A sign-in risk policy is a conditional access policy that evaluates the risk to a specific sign-in and applies mitigations based on predefined conditions and rules.</span></span>

<span data-ttu-id="7ae87-185">![Zásady přihlášení riziko](./media/active-directory-identityprotection/1014.png "zásad riziko přihlašování")</span><span class="sxs-lookup"><span data-stu-id="7ae87-185">![Sign-in risk policy](./media/active-directory-identityprotection/1014.png "Sign-in risk policy")</span></span>

<span data-ttu-id="7ae87-186">Azure AD Identity Protection pomáhá spravovat zmírnění rizikové přihlášení tím, že vám umožňuje:</span><span class="sxs-lookup"><span data-stu-id="7ae87-186">Azure AD Identity Protection helps you manage the mitigation of risky sign-ins by enabling you to:</span></span>

* <span data-ttu-id="7ae87-187">Nastavte uživatele a skupiny, které zásady platí pro:</span><span class="sxs-lookup"><span data-stu-id="7ae87-187">Set the users and groups the policy applies to:</span></span>

    <span data-ttu-id="7ae87-188">![Zásady přihlášení riziko](./media/active-directory-identityprotection/1015.png "zásad riziko přihlašování")</span><span class="sxs-lookup"><span data-stu-id="7ae87-188">![Sign-in risk policy](./media/active-directory-identityprotection/1015.png "Sign-in risk policy")</span></span>
* <span data-ttu-id="7ae87-189">Nastavte přihlašovací riziko úrovně prahovou hodnotu (nízká, střední nebo vysokou), která spustí zásady:</span><span class="sxs-lookup"><span data-stu-id="7ae87-189">Set the sign-in risk level threshold (low, medium, or high) that triggers the policy:</span></span>

    <span data-ttu-id="7ae87-190">![Zásady přihlášení riziko](./media/active-directory-identityprotection/1016.png "zásad riziko přihlašování")</span><span class="sxs-lookup"><span data-stu-id="7ae87-190">![Sign-in risk policy](./media/active-directory-identityprotection/1016.png "Sign-in risk policy")</span></span>
* <span data-ttu-id="7ae87-191">Nastavte ovládací prvky vynutit, pokud aktivuje zásady:</span><span class="sxs-lookup"><span data-stu-id="7ae87-191">Set the controls to be enforced when the policy triggers:</span></span>  

    <span data-ttu-id="7ae87-192">![Zásady přihlášení riziko](./media/active-directory-identityprotection/1017.png "zásad riziko přihlašování")</span><span class="sxs-lookup"><span data-stu-id="7ae87-192">![Sign-in risk policy](./media/active-directory-identityprotection/1017.png "Sign-in risk policy")</span></span>
* <span data-ttu-id="7ae87-193">Přepnutí stavu zásad:</span><span class="sxs-lookup"><span data-stu-id="7ae87-193">Switch the state of your policy:</span></span>

    <span data-ttu-id="7ae87-194">![Registrace MFA](./media/active-directory-identityprotection/403.png "registrace MFA")</span><span class="sxs-lookup"><span data-stu-id="7ae87-194">![MFA Registration](./media/active-directory-identityprotection/403.png "MFA Registration")</span></span>
* <span data-ttu-id="7ae87-195">Kontrola a vyhodnocení dopad změny před aktivací ho:</span><span class="sxs-lookup"><span data-stu-id="7ae87-195">Review and evaluate the impact of a change before activating it:</span></span>

    <span data-ttu-id="7ae87-196">![Zásady přihlášení riziko](./media/active-directory-identityprotection/1018.png "zásad riziko přihlašování")</span><span class="sxs-lookup"><span data-stu-id="7ae87-196">![Sign-in risk policy](./media/active-directory-identityprotection/1018.png "Sign-in risk policy")</span></span>

#### <a name="what-you-need-to-know"></a><span data-ttu-id="7ae87-197">Co potřebujete vědět</span><span class="sxs-lookup"><span data-stu-id="7ae87-197">What you need to know</span></span>
<span data-ttu-id="7ae87-198">Můžete nakonfigurovat zásady zabezpečení riziko přihlášení pro požadovat použití vícefaktorového ověřování:</span><span class="sxs-lookup"><span data-stu-id="7ae87-198">You can configure a sign-in risk security policy to require multi-factor authentication:</span></span>

<span data-ttu-id="7ae87-199">![Zásady přihlášení riziko](./media/active-directory-identityprotection/1017.png "zásad riziko přihlašování")</span><span class="sxs-lookup"><span data-stu-id="7ae87-199">![Sign-in risk policy](./media/active-directory-identityprotection/1017.png "Sign-in risk policy")</span></span>

<span data-ttu-id="7ae87-200">Ale z bezpečnostních důvodů se toto nastavení funguje pouze pro uživatele, kteří již byl registrován pro službu Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="7ae87-200">However, for security reasons, this setting only works for users that have already been registered for multi-factor authentication.</span></span> <span data-ttu-id="7ae87-201">Pokud je splněna podmínka vyžadovat vícefaktorové ověřování pro uživatele, který dosud není registrován u služby Multi-Factor authentication, uživatel je blokován.</span><span class="sxs-lookup"><span data-stu-id="7ae87-201">If the condition to require multi-factor authentication is satisfied for a user who is not yet registered for multi-factor authentication, the user is blocked.</span></span>

<span data-ttu-id="7ae87-202">Jako osvědčený postup Pokud chcete požadovat použití vícefaktorového ověřování pro rizikové přihlášení, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7ae87-202">As a best practice, if you want to require multi-factor authentication for risky sign-ins, you should:</span></span>

1. <span data-ttu-id="7ae87-203">Povolit [zásady registrace služby Multi-Factor authentication](#multi-factor-authentication-registration-policy) pro příslušné uživatele.</span><span class="sxs-lookup"><span data-stu-id="7ae87-203">Enable the [multi-factor authentication registration policy](#multi-factor-authentication-registration-policy) for the affected users.</span></span>
2. <span data-ttu-id="7ae87-204">Vyžadovat ovlivněných uživatelů na přihlášení v relaci rizikové k provedení registrace MFA</span><span class="sxs-lookup"><span data-stu-id="7ae87-204">Require the affected users to login in a non-risky session to perform a MFA registration</span></span>

<span data-ttu-id="7ae87-205">Dokončení těchto kroků zajistí, že je vyžadované pro rizikové přihlášení vícefaktorové ověřování.</span><span class="sxs-lookup"><span data-stu-id="7ae87-205">Completing these steps ensures that multi-factor authentication is required for a risky sign-in.</span></span>

#### <a name="best-practices"></a><span data-ttu-id="7ae87-206">Osvědčené postupy</span><span class="sxs-lookup"><span data-stu-id="7ae87-206">Best practices</span></span>
<span data-ttu-id="7ae87-207">Výběr **vysokou** prahová hodnota snižuje počet zásady se aktivuje a minimalizuje dopad na uživatele.</span><span class="sxs-lookup"><span data-stu-id="7ae87-207">Choosing a **High** threshold reduces the number of times a policy is triggered and minimizes the impact to users.</span></span>  

<span data-ttu-id="7ae87-208">Ale vyloučí **nízká** a **střední** přihlášení příznakem rizik ze zásad, které nemusí blokovat útočník z zneužitím ohrožení zabezpečení identity.</span><span class="sxs-lookup"><span data-stu-id="7ae87-208">However, it excludes **Low** and **Medium** sign-ins flagged for risk from the policy, which may not block an attacker from exploiting a compromised identity.</span></span>

<span data-ttu-id="7ae87-209">Při nastavení zásad,</span><span class="sxs-lookup"><span data-stu-id="7ae87-209">When setting the policy,</span></span>

* <span data-ttu-id="7ae87-210">Vyloučit uživatele, kteří nepodporují / nemůže mít vícefaktorového ověřování</span><span class="sxs-lookup"><span data-stu-id="7ae87-210">Exclude users who do not/cannot have multi-factor authentication</span></span>
* <span data-ttu-id="7ae87-211">Vyloučit uživatele v národní prostředí, kde není praktické povolení zásad (například žádný přístup na technickou podporu)</span><span class="sxs-lookup"><span data-stu-id="7ae87-211">Exclude users in locales where enabling the policy is not practical (for example no access to helpdesk)</span></span>
* <span data-ttu-id="7ae87-212">Vyloučení uživatelé, kteří jsou pravděpodobně vygeneroval velké množství false pozitivních (vývojáři, analytikům zabezpečení)</span><span class="sxs-lookup"><span data-stu-id="7ae87-212">Exclude users who are likely to generate a lot of false-positives (developers, security analysts)</span></span>
* <span data-ttu-id="7ae87-213">Použití **vysokou** prahová hodnota během počáteční zásadách, nebo pokud minimalizujete musí výzvy pohledu koncové uživatele.</span><span class="sxs-lookup"><span data-stu-id="7ae87-213">Use a **High** threshold during initial policy roll out, or if you must minimize challenges seen by end users.</span></span>
* <span data-ttu-id="7ae87-214">Použití **nízká** prahovou hodnotu, pokud vaše organizace vyžaduje vyšší úroveň zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="7ae87-214">Use a **Low**  threshold if your organization requires greater security.</span></span> <span data-ttu-id="7ae87-215">Výběr **nízká** prahová hodnota zavádí další uživatelské přihlašovací výzvy, ale zvýšení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="7ae87-215">Selecting a **Low** threshold introduces additional user sign-in challenges, but increased security.</span></span>

<span data-ttu-id="7ae87-216">Doporučené výchozí hodnota pro většinu organizací je nakonfigurovat pravidlo pro **střední** prahovou hodnotu na vytvořit rovnováhu mezi využitelností a zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="7ae87-216">The recommended default for most organizations is to configure a rule for a **Medium** threshold to strike a balance between usability and security.</span></span>

<span data-ttu-id="7ae87-217">Zásady přihlášení riziko je:</span><span class="sxs-lookup"><span data-stu-id="7ae87-217">The sign-in risk policy is:</span></span>

* <span data-ttu-id="7ae87-218">Použít pro všechny přenosy prohlížeče a přihlášení pomocí moderní ověřování.</span><span class="sxs-lookup"><span data-stu-id="7ae87-218">Applied to all browser traffic and sign-ins using modern authentication.</span></span>
* <span data-ttu-id="7ae87-219">Nevztahuje se na aplikace, které používají starší protokoly zabezpečení zakázáním WS-Trust koncový bod na federované rozšíření IDP, jako je například služba AD FS.</span><span class="sxs-lookup"><span data-stu-id="7ae87-219">Not applied to applications using older security protocols by disabling the WS-Trust endpoint at the federated IDP, such as ADFS.</span></span>

<span data-ttu-id="7ae87-220">**Rizikových událostech** stránky v konzole Identity Protection zobrazuje všechny události:</span><span class="sxs-lookup"><span data-stu-id="7ae87-220">The **Risk Events** page in the Identity Protection console lists all events:</span></span>

* <span data-ttu-id="7ae87-221">Použití této zásady</span><span class="sxs-lookup"><span data-stu-id="7ae87-221">This policy was applied to</span></span>
* <span data-ttu-id="7ae87-222">Můžete zkontrolovat aktivity a zjistit, zda byla akce odpovídající</span><span class="sxs-lookup"><span data-stu-id="7ae87-222">You can review the activity and determine whether the action was appropriate or not</span></span>

<span data-ttu-id="7ae87-223">Přehled související uživatelské prostředí najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="7ae87-223">For an overview of the related user experience, see:</span></span>

* [<span data-ttu-id="7ae87-224">Obnovení rizikové přihlášení</span><span class="sxs-lookup"><span data-stu-id="7ae87-224">Risky sign-in recovery</span></span>](active-directory-identityprotection-flows.md#risky-sign-in-recovery)
* [<span data-ttu-id="7ae87-225">Rizikové přihlášení blokován</span><span class="sxs-lookup"><span data-stu-id="7ae87-225">Risky sign-in blocked</span></span>](active-directory-identityprotection-flows.md#risky-sign-in-blocked)  
* [<span data-ttu-id="7ae87-226">Možnosti přihlášení s Azure AD Identity Protection</span><span class="sxs-lookup"><span data-stu-id="7ae87-226">Sign-in experiences with Azure AD Identity Protection</span></span>](active-directory-identityprotection-flows.md)  

<span data-ttu-id="7ae87-227">**Tím otevřete dialogové okno Konfigurace související**:</span><span class="sxs-lookup"><span data-stu-id="7ae87-227">**To open the related configuration dialog**:</span></span>

- <span data-ttu-id="7ae87-228">Na **Azure AD Identity Protection** okno v **konfigurace** klikněte na tlačítko **zásad přihlašování riziko**.</span><span class="sxs-lookup"><span data-stu-id="7ae87-228">On the **Azure AD Identity Protection** blade, in the **Configure** section, click **Sign-in risk policy**.</span></span>

    <span data-ttu-id="7ae87-229">![Zásady uživatele ridk](./media/active-directory-identityprotection/1014.png "ridk zásady uživatele")</span><span class="sxs-lookup"><span data-stu-id="7ae87-229">![User ridk policy](./media/active-directory-identityprotection/1014.png "User ridk policy")</span></span>



## <a name="users-flagged-for-risk"></a><span data-ttu-id="7ae87-230">Uživatelé označení příznakem rizika</span><span class="sxs-lookup"><span data-stu-id="7ae87-230">Users flagged for risk</span></span>

<span data-ttu-id="7ae87-231">Všechny aktivní [rizik události](active-directory-identity-protection-risk-events.md) , byly zjištěny službou Azure Active Directory pro uživatele můžete přispět k logický pojem, který volá riziko pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="7ae87-231">All active [risk events](active-directory-identity-protection-risk-events.md) that were detected by Azure Active Directory for a user contribute to a logical concept called user risk.</span></span> <span data-ttu-id="7ae87-232">Uživatele s příznakem pro riziko je indikátorem pro uživatelský účet, který byl napaden.</span><span class="sxs-lookup"><span data-stu-id="7ae87-232">A user flagged for risk is an indicator for a user account that might have been compromised.</span></span>

![Uživatelé označení příznakem rizika](./media/active-directory-identityprotection/1200.png)


### <a name="user-risk-level"></a><span data-ttu-id="7ae87-234">Úroveň rizika uživatele</span><span class="sxs-lookup"><span data-stu-id="7ae87-234">User risk level</span></span>

<span data-ttu-id="7ae87-235">Úroveň rizika uživatel je označením (vysoká, střední nebo nízká) pravděpodobnost, že byl napaden identitu uživatele.</span><span class="sxs-lookup"><span data-stu-id="7ae87-235">A user risk level is an indication (High, Medium, or Low) of the likelihood that the user’s identity has been compromised.</span></span> <span data-ttu-id="7ae87-236">Se počítá na základě událostí riziko uživatele, které jsou přidružené k identitě uživatele.</span><span class="sxs-lookup"><span data-stu-id="7ae87-236">It is calculated based on the user risk events that are associated with a user's identity.</span></span>

<span data-ttu-id="7ae87-237">Stav události riziko je buď **Active** nebo **uzavřeno**.</span><span class="sxs-lookup"><span data-stu-id="7ae87-237">The status of a risk event is either **Active** or **Closed**.</span></span> <span data-ttu-id="7ae87-238">Pouze riziko události, které jsou **Active** podílet se na uživatelské úrovni výpočet riziko.</span><span class="sxs-lookup"><span data-stu-id="7ae87-238">Only risk events that are **Active** contribute to the user risk level calculation.</span></span>

<span data-ttu-id="7ae87-239">Úroveň rizika uživatele se počítá pomocí následující zadání:</span><span class="sxs-lookup"><span data-stu-id="7ae87-239">The user risk level is calculated using the following inputs:</span></span>

* <span data-ttu-id="7ae87-240">Aktivní rizikových událostech, které mají vliv uživatele</span><span class="sxs-lookup"><span data-stu-id="7ae87-240">Active risk events impacting the user</span></span>
* <span data-ttu-id="7ae87-241">Úroveň rizika tyto události</span><span class="sxs-lookup"><span data-stu-id="7ae87-241">Risk level of these events</span></span>
* <span data-ttu-id="7ae87-242">Jestli nebyly provedeny žádné nápravné akce</span><span class="sxs-lookup"><span data-stu-id="7ae87-242">Whether any remediation actions have been taken</span></span>

<span data-ttu-id="7ae87-243">![Uživatel rizika](./media/active-directory-identityprotection/1031.png "rizika uživatele")</span><span class="sxs-lookup"><span data-stu-id="7ae87-243">![User risks](./media/active-directory-identityprotection/1031.png "User risks")</span></span>

<span data-ttu-id="7ae87-244">Můžete použít k vytvoření zásady podmíněného přístupu, které zablokovat rizikové uživatelům přihlášení úrovní rizika uživatele nebo vynutit, aby bezpečně změnit své heslo.</span><span class="sxs-lookup"><span data-stu-id="7ae87-244">You can use the user risk levels to create conditional access policies that block risky users from signing in, or force them to securely change their password.</span></span>

### <a name="closing-risk-events-manually"></a><span data-ttu-id="7ae87-245">Zavřením rizikových událostí ručně</span><span class="sxs-lookup"><span data-stu-id="7ae87-245">Closing risk events manually</span></span>

<span data-ttu-id="7ae87-246">Ve většině případů bude trvat nápravné akce, jako například zabezpečené heslo resetovat automaticky uzavřít rizikových událostech.</span><span class="sxs-lookup"><span data-stu-id="7ae87-246">In most cases, you will take remediation actions such as a secure password reset to automatically close risk events.</span></span> <span data-ttu-id="7ae87-247">Ale to nemusí být vždy možné.</span><span class="sxs-lookup"><span data-stu-id="7ae87-247">However, this might not always be possible.</span></span>  
<span data-ttu-id="7ae87-248">Toto je, například případě, když:</span><span class="sxs-lookup"><span data-stu-id="7ae87-248">This is, for example, the case, when:</span></span>

* <span data-ttu-id="7ae87-249">Uživatel s Active rizikových událostí byl odstraněn.</span><span class="sxs-lookup"><span data-stu-id="7ae87-249">A user with Active risk events has been deleted</span></span>
* <span data-ttu-id="7ae87-250">Šetření zjistí, že byla událostí vykazuje riziko provést legitimní uživatel</span><span class="sxs-lookup"><span data-stu-id="7ae87-250">An investigation reveals that a reported risk event has been perform by the legitimate user</span></span>

<span data-ttu-id="7ae87-251">Protože rizikových událostech, které jsou **Active** přispívat k výpočtu riziko uživatele, možná budete muset ručně nižší úroveň rizika ukončením rizikových událostí ručně.</span><span class="sxs-lookup"><span data-stu-id="7ae87-251">Because risk events that are **Active** contribute to the user risk calculation, you may have to manually lower a risk level by closing risk events manually.</span></span>  
<span data-ttu-id="7ae87-252">Během šetření můžete provést některou z těchto akcí pro změnu stavu riziko události:</span><span class="sxs-lookup"><span data-stu-id="7ae87-252">During the course of investigation, you can choose to take any of these actions to change the status of a risk event:</span></span>

<span data-ttu-id="7ae87-253">![Akce](./media/active-directory-identityprotection/34.png "akce")</span><span class="sxs-lookup"><span data-stu-id="7ae87-253">![Actions](./media/active-directory-identityprotection/34.png "Actions")</span></span>

* <span data-ttu-id="7ae87-254">**Vyřešte** – Pokud po prozkoumání riziko událostí, trvalo akce odpovídající nápravu mimo ochrany identit a budete mít dojem, že riziko události by se měly zvažovat zavřená, označte událostí jako Vyřešeno.</span><span class="sxs-lookup"><span data-stu-id="7ae87-254">**Resolve** - If after investigating a risk event, you took an appropriate remediation action outside Identity Protection, and you believe that the risk event should be considered closed, mark the event as Resolved.</span></span> <span data-ttu-id="7ae87-255">Přeložit události na uzavřený nastaví stav riziko události a události riziko už přispějí k riziko pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="7ae87-255">Resolved events will set the risk event’s status to Closed and the risk event will no longer contribute to user risk.</span></span>
* <span data-ttu-id="7ae87-256">**Označit jako falešně pozitivní** – v některých případech můžete prozkoumat události riziko a zjistit, že byla nesprávně označena jako rizikové.</span><span class="sxs-lookup"><span data-stu-id="7ae87-256">**Mark as false-positive** - In some cases, you may investigate a risk event and discover that it was incorrectly flagged as a risky.</span></span> <span data-ttu-id="7ae87-257">Můžete snížit počet takových výskytů označením riziko událostí jako falešně pozitivní.</span><span class="sxs-lookup"><span data-stu-id="7ae87-257">You can help reduce the number of such occurrences by marking the risk event as False-positive.</span></span> <span data-ttu-id="7ae87-258">To vám pomůže zlepšit klasifikaci podobné události v budoucnu algoritmů strojového učení.</span><span class="sxs-lookup"><span data-stu-id="7ae87-258">This will help the machine learning algorithms to improve the classification of similar events in the future.</span></span> <span data-ttu-id="7ae87-259">Stav falešně pozitivní událostí je **uzavřeno** a už přispívají k riziko pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="7ae87-259">The status of false-positive events is to **Closed** and they will no longer contribute to user risk.</span></span>
* <span data-ttu-id="7ae87-260">**Ignorovat** – Pokud jste ještě nevstoupilo žádnou akci nápravy, ale má být odebrán ze seznamu active událost riziko můžete označit riziko událostí ignorovat a bude uzavřen stav události.</span><span class="sxs-lookup"><span data-stu-id="7ae87-260">**Ignore** - If you have not taken any remediation action, but want the risk event to be removed from the active list, you can mark a risk event Ignore and the event status will be Closed.</span></span> <span data-ttu-id="7ae87-261">Ignoruje události nepřispívají k riziko pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="7ae87-261">Ignored events do not contribute to user risk.</span></span> <span data-ttu-id="7ae87-262">Tato možnost by měla být použita pouze za neobvyklé okolnosti.</span><span class="sxs-lookup"><span data-stu-id="7ae87-262">This option should only be used under unusual circumstances.</span></span>
* <span data-ttu-id="7ae87-263">**Znovu aktivovat** -riziko události, které byly ručně (výběrem **vyřešit**, **falešně pozitivní**, nebo **Ignorovat**) lze znovu aktivovat, nastavení události stavu zpět do **Active**.</span><span class="sxs-lookup"><span data-stu-id="7ae87-263">**Reactivate** - Risk events that were manually closed (by choosing **Resolve**, **False positive**, or **Ignore**) can be reactivated, setting the event status back to **Active**.</span></span> <span data-ttu-id="7ae87-264">Opětovně aktivovaných rizikových událostech podílet se na uživatelské úrovni výpočet riziko.</span><span class="sxs-lookup"><span data-stu-id="7ae87-264">Reactivated risk events contribute to the user risk level calculation.</span></span> <span data-ttu-id="7ae87-265">Nelze znovu aktivovat, rizikových událostech (například zabezpečené heslo resetovat) uzavřeny prostřednictvím nápravy.</span><span class="sxs-lookup"><span data-stu-id="7ae87-265">Risk events closed through remediation (such as a secure password reset) cannot be reactivated.</span></span>

<span data-ttu-id="7ae87-266">**Tím otevřete dialogové okno Konfigurace související**:</span><span class="sxs-lookup"><span data-stu-id="7ae87-266">**To open the related configuration dialog**:</span></span>

1. <span data-ttu-id="7ae87-267">Na **Azure AD Identity Protection** okno, v části **prošetření**, klikněte na tlačítko **rizik události**.</span><span class="sxs-lookup"><span data-stu-id="7ae87-267">On the **Azure AD Identity Protection** blade, under **Investigate**, click **Risk events**.</span></span>

    <span data-ttu-id="7ae87-268">![Resetování hesla ruční](./media/active-directory-identityprotection/1002.png "resetování hesla ruční")</span><span class="sxs-lookup"><span data-stu-id="7ae87-268">![Manual password reset](./media/active-directory-identityprotection/1002.png "Manual password reset")</span></span>
2. <span data-ttu-id="7ae87-269">V **rizik události** klikněte na riziko.</span><span class="sxs-lookup"><span data-stu-id="7ae87-269">In the **Risk events** list, click a risk.</span></span>

    <span data-ttu-id="7ae87-270">![Resetování hesla ruční](./media/active-directory-identityprotection/1003.png "resetování hesla ruční")</span><span class="sxs-lookup"><span data-stu-id="7ae87-270">![Manual password reset](./media/active-directory-identityprotection/1003.png "Manual password reset")</span></span>
3. <span data-ttu-id="7ae87-271">V okně riziko klikněte pravým tlačítkem na uživatele.</span><span class="sxs-lookup"><span data-stu-id="7ae87-271">On the risk blade, right-click a user.</span></span>

    <span data-ttu-id="7ae87-272">![Resetování hesla ruční](./media/active-directory-identityprotection/1004.png "resetování hesla ruční")</span><span class="sxs-lookup"><span data-stu-id="7ae87-272">![Manual password reset](./media/active-directory-identityprotection/1004.png "Manual password reset")</span></span>

### <a name="closing-all-risk-events-for-a-user-manually"></a><span data-ttu-id="7ae87-273">Zavřít všechny události riziko pro uživatele ručně</span><span class="sxs-lookup"><span data-stu-id="7ae87-273">Closing all risk events for a user manually</span></span>
<span data-ttu-id="7ae87-274">Místo ručně zavřete riziko události pro uživatele jednotlivě, Azure Active Directory Identity Protection také poskytuje metodu zavřete všechny události pro uživatele s jedním kliknutím.</span><span class="sxs-lookup"><span data-stu-id="7ae87-274">Instead of manually closing risk events for a user individually, Azure Active Directory Identity Protection also provides you with a method to close all events for a user with one click.</span></span>

<span data-ttu-id="7ae87-275">![Akce](./media/active-directory-identityprotection/2222.png "akce")</span><span class="sxs-lookup"><span data-stu-id="7ae87-275">![Actions](./media/active-directory-identityprotection/2222.png "Actions")</span></span>

<span data-ttu-id="7ae87-276">Když kliknete na tlačítko **zavřít všechny události**, jsou zavřeny všechny události a ovlivněného uživatele jsou již v ohrožení.</span><span class="sxs-lookup"><span data-stu-id="7ae87-276">When you click **Dismiss all events**, all events are closed and the affected user is no longer at risk.</span></span>

### <a name="remediating-user-risk-events"></a><span data-ttu-id="7ae87-277">Nekompatibilních rizikových událostí uživatele</span><span class="sxs-lookup"><span data-stu-id="7ae87-277">Remediating user risk events</span></span>

<span data-ttu-id="7ae87-278">Nápravy je akci, kterou chcete zabezpečit identity nebo zařízení, která byla dříve by mohly vzbuzovat podezření nebo známé došlo k narušení.</span><span class="sxs-lookup"><span data-stu-id="7ae87-278">A remediation is an action to secure an identity or a device that was previously suspected or known to be compromised.</span></span> <span data-ttu-id="7ae87-279">Akce nápravy obnoví identity nebo zařízení do bezpečného stavu a odstraňuje předchozí rizikových událostech spojených s identity nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="7ae87-279">A remediation action restores the identity or device to a safe state, and resolves previous risk events associated with the identity or device.</span></span>

<span data-ttu-id="7ae87-280">Chcete-li opravit událostí riziko uživatele, můžete:</span><span class="sxs-lookup"><span data-stu-id="7ae87-280">To remediate user risk events, you can:</span></span>

* <span data-ttu-id="7ae87-281">Zabezpečené heslo resetovat k nápravě uživatele rizikových událostí ručně</span><span class="sxs-lookup"><span data-stu-id="7ae87-281">Perform a secure password reset to remediate user risk events manually</span></span>
* <span data-ttu-id="7ae87-282">Konfigurace uživatelských zásad zabezpečení riziko zmírnit nebo automaticky napravovat riziko událostí uživatele</span><span class="sxs-lookup"><span data-stu-id="7ae87-282">Configure a user risk security policy to mitigate or remediate user risk events automatically</span></span>
* <span data-ttu-id="7ae87-283">Znovu přeinstalovat image nakažených zařízení</span><span class="sxs-lookup"><span data-stu-id="7ae87-283">Re-image the infected device</span></span>  

#### <a name="manual-secure-password-reset"></a><span data-ttu-id="7ae87-284">Resetování ruční zabezpečeného hesla</span><span class="sxs-lookup"><span data-stu-id="7ae87-284">Manual secure password reset</span></span>
<span data-ttu-id="7ae87-285">Resetování zabezpečeného hesla je efektivní nápravy pro mnoho událostí riziko a při provádění automaticky zavře tyto události riziko přepočítá úroveň rizika uživatele.</span><span class="sxs-lookup"><span data-stu-id="7ae87-285">A secure password reset is an effective remediation for many risk events, and when performed, automatically closes these risk events and recalculates the user risk level.</span></span> <span data-ttu-id="7ae87-286">Řídicí panel ochrany identit můžete zahájit obnovení hesla pro rizikové uživatele.</span><span class="sxs-lookup"><span data-stu-id="7ae87-286">You can use the Identity Protection dashboard to initiate a password reset for a risky user.</span></span>

<span data-ttu-id="7ae87-287">Související dialogové okno obsahuje dvě různé metody pro obnovení hesla:</span><span class="sxs-lookup"><span data-stu-id="7ae87-287">The related dialog provides two different methods to reset a password:</span></span>

<span data-ttu-id="7ae87-288">**Resetovat heslo** – vyberte **vyžadovat, aby uživatel změnit heslo** chcete umožnit uživatelům samoobslužné obnovení, pokud má uživatel zaregistrován u služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="7ae87-288">**Reset password** - Select **Require the user to reset their password** to allow the user to self-recover if the user has registered for multi-factor authentication.</span></span> <span data-ttu-id="7ae87-289">Při jeho příštím přihlášení bude potřeba řešit výzvy ověřování Multi-Factor authentication úspěšně a pak přinucení změnit heslo uživatele.</span><span class="sxs-lookup"><span data-stu-id="7ae87-289">During the user's next sign-in, the user will be required to solve a multi-factor authentication challenge successfully and then, forced to change the password.</span></span> <span data-ttu-id="7ae87-290">Tato možnost není dostupná, pokud uživatelský účet ještě není registrované služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="7ae87-290">This option isn't available if the user account is not already registered multi-factor authentication.</span></span>

<span data-ttu-id="7ae87-291">**Dočasné heslo** – vyberte **generovat dočasné heslo** okamžitě zneplatní stávající heslo a vytvořit nové dočasné heslo pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="7ae87-291">**Temporary password** - Select **Generate a temporary password** to immediately invalidate the existing password, and create a new temporary password for the user.</span></span> <span data-ttu-id="7ae87-292">Odeslání nové dočasné heslo na alternativní e-mailovou adresu pro uživatele nebo pro uživatele správce.</span><span class="sxs-lookup"><span data-stu-id="7ae87-292">Send the new temporary password to an alternate email address for the user or to the user's manager.</span></span> <span data-ttu-id="7ae87-293">Protože je dočasné heslo, uživatel se vyzve k změnit heslo při přihlášení.</span><span class="sxs-lookup"><span data-stu-id="7ae87-293">Because the password is temporary, the user will be prompted to change the password upon sign-in.</span></span>

<span data-ttu-id="7ae87-294">![Zásady](./media/active-directory-identityprotection/1005.png "zásad")</span><span class="sxs-lookup"><span data-stu-id="7ae87-294">![Policy](./media/active-directory-identityprotection/1005.png "Policy")</span></span>

<span data-ttu-id="7ae87-295">**Tím otevřete dialogové okno Konfigurace související**:</span><span class="sxs-lookup"><span data-stu-id="7ae87-295">**To open the related configuration dialog**:</span></span>

1. <span data-ttu-id="7ae87-296">Na **Azure AD Identity Protection** okně klikněte na tlačítko **uživatelé označení příznakem rizik**.</span><span class="sxs-lookup"><span data-stu-id="7ae87-296">On the **Azure AD Identity Protection** blade, click **Users flagged for risk**.</span></span>

    <span data-ttu-id="7ae87-297">![Resetování hesla ruční](./media/active-directory-identityprotection/1006.png "resetování hesla ruční")</span><span class="sxs-lookup"><span data-stu-id="7ae87-297">![Manual password reset](./media/active-directory-identityprotection/1006.png "Manual password reset")</span></span>
2. <span data-ttu-id="7ae87-298">V seznamu uživatelů vyberte uživatele, který má alespoň jeden rizikových událostí.</span><span class="sxs-lookup"><span data-stu-id="7ae87-298">From the list of users, select a user with at least one risk events.</span></span>

    <span data-ttu-id="7ae87-299">![Resetování hesla ruční](./media/active-directory-identityprotection/1007.png "resetování hesla ruční")</span><span class="sxs-lookup"><span data-stu-id="7ae87-299">![Manual password reset](./media/active-directory-identityprotection/1007.png "Manual password reset")</span></span>
3. <span data-ttu-id="7ae87-300">V okně uživatele klikněte na **resetovat heslo**.</span><span class="sxs-lookup"><span data-stu-id="7ae87-300">On the user blade, click **Reset password**.</span></span>

    <span data-ttu-id="7ae87-301">![Resetování hesla ruční](./media/active-directory-identityprotection/1008.png "resetování hesla ruční")</span><span class="sxs-lookup"><span data-stu-id="7ae87-301">![Manual password reset](./media/active-directory-identityprotection/1008.png "Manual password reset")</span></span>

### <a name="user-risk-security-policy"></a><span data-ttu-id="7ae87-302">Zásada zabezpečení riziko uživatelů</span><span class="sxs-lookup"><span data-stu-id="7ae87-302">User risk security policy</span></span>
<span data-ttu-id="7ae87-303">Zásady uživatele rizik zabezpečení je zásadu podmíněného přístupu, která vyhodnotí úroveň rizika pro konkrétního uživatele a použije nápravy a zmírnění akce na základě předem definované podmínky a pravidla.</span><span class="sxs-lookup"><span data-stu-id="7ae87-303">A user risk security policy is a conditional access policy that evaluates the risk level to a specific user and applies remediation and mitigation actions based on predefined conditions and rules.</span></span>

<span data-ttu-id="7ae87-304">![Zásady uživatele ridk](./media/active-directory-identityprotection/1009.png "ridk zásady uživatele")</span><span class="sxs-lookup"><span data-stu-id="7ae87-304">![User ridk policy](./media/active-directory-identityprotection/1009.png "User ridk policy")</span></span>

<span data-ttu-id="7ae87-305">Azure AD Identity Protection pomáhá spravovat omezení rizik a náprava uživatelé označení příznakem rizik a to:</span><span class="sxs-lookup"><span data-stu-id="7ae87-305">Azure AD Identity Protection helps you manage the mitigation and remediation of users flagged for risk by enabling you to:</span></span>

* <span data-ttu-id="7ae87-306">Nastavte uživatele a skupiny, které zásady platí pro:</span><span class="sxs-lookup"><span data-stu-id="7ae87-306">Set the users and groups the policy applies to:</span></span>

    <span data-ttu-id="7ae87-307">![Zásady uživatele ridk](./media/active-directory-identityprotection/1010.png "ridk zásady uživatele")</span><span class="sxs-lookup"><span data-stu-id="7ae87-307">![User ridk policy](./media/active-directory-identityprotection/1010.png "User ridk policy")</span></span>
* <span data-ttu-id="7ae87-308">Nastavte uživatele riziko úrovně prahovou hodnotu (nízká, střední nebo vysokou), která spustí zásady:</span><span class="sxs-lookup"><span data-stu-id="7ae87-308">Set the user risk level threshold (low, medium, or high) that triggers the policy:</span></span>

    <span data-ttu-id="7ae87-309">![Zásady uživatele ridk](./media/active-directory-identityprotection/1011.png "ridk zásady uživatele")</span><span class="sxs-lookup"><span data-stu-id="7ae87-309">![User ridk policy](./media/active-directory-identityprotection/1011.png "User ridk policy")</span></span>
* <span data-ttu-id="7ae87-310">Nastavte ovládací prvky vynutit, pokud aktivuje zásady:</span><span class="sxs-lookup"><span data-stu-id="7ae87-310">Set the controls to be enforced when the policy triggers:</span></span>

    <span data-ttu-id="7ae87-311">![Zásady uživatele ridk](./media/active-directory-identityprotection/1012.png "ridk zásady uživatele")</span><span class="sxs-lookup"><span data-stu-id="7ae87-311">![User ridk policy](./media/active-directory-identityprotection/1012.png "User ridk policy")</span></span>
* <span data-ttu-id="7ae87-312">Přepnutí stavu zásad:</span><span class="sxs-lookup"><span data-stu-id="7ae87-312">Switch the state of your policy:</span></span>

    <span data-ttu-id="7ae87-313">![Zásady uživatele ridk](./media/active-directory-identityprotection/403.png "registrace MFA")</span><span class="sxs-lookup"><span data-stu-id="7ae87-313">![User ridk policy](./media/active-directory-identityprotection/403.png "MFA Registration")</span></span>
* <span data-ttu-id="7ae87-314">Kontrola a vyhodnocení dopad změny před aktivací ho:</span><span class="sxs-lookup"><span data-stu-id="7ae87-314">Review and evaluate the impact of a change before activating it:</span></span>

    <span data-ttu-id="7ae87-315">![Zásady uživatele ridk](./media/active-directory-identityprotection/1013.png "ridk zásady uživatele")</span><span class="sxs-lookup"><span data-stu-id="7ae87-315">![User ridk policy](./media/active-directory-identityprotection/1013.png "User ridk policy")</span></span>

<span data-ttu-id="7ae87-316">Výběr **vysokou** prahová hodnota snižuje počet zásady se aktivuje a minimalizuje dopad na uživatele.</span><span class="sxs-lookup"><span data-stu-id="7ae87-316">Choosing a **High** threshold reduces the number of times a policy is triggered and minimizes the impact to users.</span></span>
<span data-ttu-id="7ae87-317">Ale vyloučí **nízká** a **střední** uživatelé označení příznakem rizik ze zásad, které nemusí zabezpečit identity nebo zařízení, měla by mohly vzbuzovat podezření nebo známé došlo k narušení.</span><span class="sxs-lookup"><span data-stu-id="7ae87-317">However, it excludes **Low** and **Medium** users flagged for risk from the policy, which may not secure identities or devices that were previously suspected or known to be compromised.</span></span>

<span data-ttu-id="7ae87-318">Při nastavení zásad,</span><span class="sxs-lookup"><span data-stu-id="7ae87-318">When setting the policy,</span></span>

* <span data-ttu-id="7ae87-319">Vyloučení uživatelé, kteří jsou pravděpodobně vygeneroval velké množství false pozitivních (vývojáři, analytikům zabezpečení)</span><span class="sxs-lookup"><span data-stu-id="7ae87-319">Exclude users who are likely to generate a lot of false-positives (developers, security analysts)</span></span>
* <span data-ttu-id="7ae87-320">Vyloučit uživatele v národní prostředí, kde není praktické povolení zásad (například žádný přístup na technickou podporu)</span><span class="sxs-lookup"><span data-stu-id="7ae87-320">Exclude users in locales where enabling the policy is not practical (for example no access to helpdesk)</span></span>
* <span data-ttu-id="7ae87-321">Použití **vysokou** prahová hodnota během počáteční zásadách, nebo pokud minimalizujete musí výzvy pohledu koncové uživatele.</span><span class="sxs-lookup"><span data-stu-id="7ae87-321">Use a **High** threshold during initial policy roll out, or if you must minimize challenges seen by end users.</span></span>
* <span data-ttu-id="7ae87-322">Použití **nízká** prahovou hodnotu, pokud vaše organizace vyžaduje vyšší úroveň zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="7ae87-322">Use a **Low** threshold if your organization requires greater security.</span></span> <span data-ttu-id="7ae87-323">Výběr **nízká** prahová hodnota zavádí další uživatelské přihlašovací výzvy, ale zvýšení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="7ae87-323">Selecting a **Low** threshold introduces additional user sign-in challenges, but increased security.</span></span>

<span data-ttu-id="7ae87-324">Doporučené výchozí hodnota pro většinu organizací je nakonfigurovat pravidlo pro **střední** prahovou hodnotu na vytvořit rovnováhu mezi využitelností a zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="7ae87-324">The recommended default for most organizations is to configure a rule for a **Medium** threshold to strike a balance between usability and security.</span></span>

<span data-ttu-id="7ae87-325">Přehled související uživatelské prostředí najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="7ae87-325">For an overview of the related user experience, see:</span></span>

* <span data-ttu-id="7ae87-326">[Dojde k ohrožení tok obnovení účtu](active-directory-identityprotection-flows.md#compromised-account-recovery).</span><span class="sxs-lookup"><span data-stu-id="7ae87-326">[Compromised account recovery flow](active-directory-identityprotection-flows.md#compromised-account-recovery).</span></span>  
* <span data-ttu-id="7ae87-327">[Dojde k ohrožení účet byl uzamčen toku](active-directory-identityprotection-flows.md#compromised-account-blocked).</span><span class="sxs-lookup"><span data-stu-id="7ae87-327">[Compromised account blocked flow](active-directory-identityprotection-flows.md#compromised-account-blocked).</span></span>  

<span data-ttu-id="7ae87-328">**Tím otevřete dialogové okno Konfigurace související**:</span><span class="sxs-lookup"><span data-stu-id="7ae87-328">**To open the related configuration dialog**:</span></span>

- <span data-ttu-id="7ae87-329">Na **Azure AD Identity Protection** okno v **konfigurace** klikněte na tlačítko **zásady uživatele riziko**.</span><span class="sxs-lookup"><span data-stu-id="7ae87-329">On the **Azure AD Identity Protection** blade, in the **Configure** section, click **User risk policy**.</span></span>

    <span data-ttu-id="7ae87-330">![Zásady uživatele ridk](./media/active-directory-identityprotection/1009.png "ridk zásady uživatele")</span><span class="sxs-lookup"><span data-stu-id="7ae87-330">![User ridk policy](./media/active-directory-identityprotection/1009.png "User ridk policy")</span></span>

### <a name="mitigating-user-risk-events"></a><span data-ttu-id="7ae87-331">Zmírnění rizik událostí uživatele</span><span class="sxs-lookup"><span data-stu-id="7ae87-331">Mitigating user risk events</span></span>
<span data-ttu-id="7ae87-332">Správci mohou nastavit zásady uživatele riziko zabezpečení pro blokování uživatele při přihlášení v závislosti na úroveň rizika.</span><span class="sxs-lookup"><span data-stu-id="7ae87-332">Administrators can set a user risk security policy to block users upon sign-in depending on the risk level.</span></span>

<span data-ttu-id="7ae87-333">Blokování přihlášení:</span><span class="sxs-lookup"><span data-stu-id="7ae87-333">Blocking a sign-in:</span></span>

* <span data-ttu-id="7ae87-334">Zabrání generování nových událostí riziko uživatele pro ovlivněného uživatele</span><span class="sxs-lookup"><span data-stu-id="7ae87-334">Prevents the generation of new user risk events for the affected user</span></span>
* <span data-ttu-id="7ae87-335">Umožňuje správcům ručně opravit rizikových událostech, které mají vliv na identitu uživatele a obnovte ji do zabezpečené stavu</span><span class="sxs-lookup"><span data-stu-id="7ae87-335">Enables administrators to manually remediate the risk events affecting the user's identity and restore it to a secure state</span></span>



## <a name="multi-factor-authentication-registration-policy"></a><span data-ttu-id="7ae87-336">Zásady registrace služby Multi-Factor authentication</span><span class="sxs-lookup"><span data-stu-id="7ae87-336">Multi-factor authentication registration policy</span></span>
<span data-ttu-id="7ae87-337">Ověřování Azure Multi-Factor authentication je metoda ověřování, který jste, která vyžaduje použití víc věcí než jenom uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="7ae87-337">Azure multi-factor authentication is a method of verifying who you are that requires the use of more than just a username and password.</span></span> <span data-ttu-id="7ae87-338">Poskytuje druhou vrstvu zabezpečení uživatelská přihlášení a transakce.</span><span class="sxs-lookup"><span data-stu-id="7ae87-338">It provides a second layer of security to user sign-ins and transactions.</span></span>  
<span data-ttu-id="7ae87-339">Doporučujeme vyžadovat ověřování Azure Multi-Factor authentication pro přihlášení uživatele, protože ho:</span><span class="sxs-lookup"><span data-stu-id="7ae87-339">We recommend that you require Azure multi-factor authentication for user sign-ins because it:</span></span>

* <span data-ttu-id="7ae87-340">Poskytuje silné ověřování s celou řadu možností snadno ověření</span><span class="sxs-lookup"><span data-stu-id="7ae87-340">Delivers strong authentication with a range of easy verification options</span></span>
* <span data-ttu-id="7ae87-341">Hraje důležitou roli při přípravě vaší organizace k ochraně a obnovování z účtu ohrožení</span><span class="sxs-lookup"><span data-stu-id="7ae87-341">Plays a key role in preparing your organization to protect and recover from account compromises</span></span>

<span data-ttu-id="7ae87-342">![Zásady uživatele ridk](./media/active-directory-identityprotection/1019.png "ridk zásady uživatele")</span><span class="sxs-lookup"><span data-stu-id="7ae87-342">![User ridk policy](./media/active-directory-identityprotection/1019.png "User ridk policy")</span></span>

<span data-ttu-id="7ae87-343">Další podrobnosti najdete v tématu [co je Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span><span class="sxs-lookup"><span data-stu-id="7ae87-343">For more details, see [What is Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span></span>

<span data-ttu-id="7ae87-344">Azure AD Identity Protection pomáhá spravovat zavádění registrace služby Multi-Factor authentication tím, že nakonfigurujete zásadu, která umožňuje:</span><span class="sxs-lookup"><span data-stu-id="7ae87-344">Azure AD Identity Protection helps you manage the roll-out of multi-factor authentication registration by configuring a policy that enables you to:</span></span>

* <span data-ttu-id="7ae87-345">Nastavte uživatele a skupiny, které zásady platí pro:</span><span class="sxs-lookup"><span data-stu-id="7ae87-345">Set the users and groups the policy applies to:</span></span>

    <span data-ttu-id="7ae87-346">![Zásady vícefaktorového ověřování](./media/active-directory-identityprotection/1020.png "zásad vícefaktorového ověřování")</span><span class="sxs-lookup"><span data-stu-id="7ae87-346">![MFA policy](./media/active-directory-identityprotection/1020.png "MFA policy")</span></span>
* <span data-ttu-id="7ae87-347">Nastavit ovládací prvky vynutit, pokud zásady aktivuje::</span><span class="sxs-lookup"><span data-stu-id="7ae87-347">Set the controls to be enforced when the policy triggers::</span></span>  

    <span data-ttu-id="7ae87-348">![Zásady vícefaktorového ověřování](./media/active-directory-identityprotection/1021.png "zásad vícefaktorového ověřování")</span><span class="sxs-lookup"><span data-stu-id="7ae87-348">![MFA policy](./media/active-directory-identityprotection/1021.png "MFA policy")</span></span>
* <span data-ttu-id="7ae87-349">Přepnutí stavu zásad:</span><span class="sxs-lookup"><span data-stu-id="7ae87-349">Switch the state of your policy:</span></span>

    <span data-ttu-id="7ae87-350">![Zásady vícefaktorového ověřování](./media/active-directory-identityprotection/403.png "zásad vícefaktorového ověřování")</span><span class="sxs-lookup"><span data-stu-id="7ae87-350">![MFA policy](./media/active-directory-identityprotection/403.png "MFA policy")</span></span>
* <span data-ttu-id="7ae87-351">Zobrazte aktuální stav registrace:</span><span class="sxs-lookup"><span data-stu-id="7ae87-351">View the current registration status:</span></span>

    <span data-ttu-id="7ae87-352">![Zásady vícefaktorového ověřování](./media/active-directory-identityprotection/1022.png "zásad vícefaktorového ověřování")</span><span class="sxs-lookup"><span data-stu-id="7ae87-352">![MFA policy](./media/active-directory-identityprotection/1022.png "MFA policy")</span></span>

<span data-ttu-id="7ae87-353">Přehled související uživatelské prostředí najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="7ae87-353">For an overview of the related user experience, see:</span></span>

* <span data-ttu-id="7ae87-354">[Postup registrace služby Multi-Factor authentication](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).</span><span class="sxs-lookup"><span data-stu-id="7ae87-354">[Multi-factor authentication registration flow](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).</span></span>  
* <span data-ttu-id="7ae87-355">[Přihlášení vyskytne s Azure AD Identity Protection](active-directory-identityprotection-flows.md).</span><span class="sxs-lookup"><span data-stu-id="7ae87-355">[Sign-in experiences with Azure AD Identity Protection](active-directory-identityprotection-flows.md).</span></span>  

<span data-ttu-id="7ae87-356">**Tím otevřete dialogové okno Konfigurace související**:</span><span class="sxs-lookup"><span data-stu-id="7ae87-356">**To open the related configuration dialog**:</span></span>

- <span data-ttu-id="7ae87-357">Na **Azure AD Identity Protection** okno v **konfigurace** klikněte na tlačítko **registrace služby Multi-Factor authentication**.</span><span class="sxs-lookup"><span data-stu-id="7ae87-357">On the **Azure AD Identity Protection** blade, in the **Configure** section, click **Multi-factor authentication registration**.</span></span>

    <span data-ttu-id="7ae87-358">![Zásady vícefaktorového ověřování](./media/active-directory-identityprotection/1019.png "zásad vícefaktorového ověřování")</span><span class="sxs-lookup"><span data-stu-id="7ae87-358">![MFA policy](./media/active-directory-identityprotection/1019.png "MFA policy")</span></span>

## <a name="next-steps"></a><span data-ttu-id="7ae87-359">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7ae87-359">Next steps</span></span>
* [<span data-ttu-id="7ae87-360">Kanál 9: Azure AD a Identity zobrazení: Identity Protection verze Preview</span><span class="sxs-lookup"><span data-stu-id="7ae87-360">Channel 9: Azure AD and Identity Show: Identity Protection Preview</span></span>](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

* [<span data-ttu-id="7ae87-361">Povolení ochrany identit Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7ae87-361">Enabling Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection-enable.md)

* [<span data-ttu-id="7ae87-362">Chyb zabezpečení detekovaných službou Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="7ae87-362">Vulnerabilities detected by Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection-vulnerabilities.md)

* [<span data-ttu-id="7ae87-363">Azure Active Directory rizikových událostí</span><span class="sxs-lookup"><span data-stu-id="7ae87-363">Azure Active Directory risk events</span></span>](active-directory-identity-protection-risk-events.md)

* [<span data-ttu-id="7ae87-364">Azure oznámení ochrany identit služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="7ae87-364">Azure Active Directory Identity Protection notifications</span></span>](active-directory-identityprotection-notifications.md)

* [<span data-ttu-id="7ae87-365">Azure seznam strategií ochrany identit Active Directory</span><span class="sxs-lookup"><span data-stu-id="7ae87-365">Azure Active Directory Identity Protection playbook</span></span>](active-directory-identityprotection-playbook.md)

* [<span data-ttu-id="7ae87-366">Azure slovník ochrany identit služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="7ae87-366">Azure Active Directory Identity Protection glossary</span></span>](active-directory-identityprotection-glossary.md)

* [<span data-ttu-id="7ae87-367">Možnosti přihlášení s Azure AD Identity Protection</span><span class="sxs-lookup"><span data-stu-id="7ae87-367">Sign-in experiences with Azure AD Identity Protection</span></span>](active-directory-identityprotection-flows.md)

* [<span data-ttu-id="7ae87-368">Azure Active Directory identitu ochrana – jak odblokovat uživatele</span><span class="sxs-lookup"><span data-stu-id="7ae87-368">Azure Active Directory Identity Protection - How to unblock users</span></span>](active-directory-identityprotection-unblock-howto.md)

* [<span data-ttu-id="7ae87-369">Začínáme s Azure Active Directory Identity Protection a Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="7ae87-369">Get started with Azure Active Directory Identity Protection and Microsoft Graph</span></span>](active-directory-identityprotection-graph-getting-started.md)
