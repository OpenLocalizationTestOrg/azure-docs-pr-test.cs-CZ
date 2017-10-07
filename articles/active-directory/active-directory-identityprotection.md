---
title: "aaaAzure ochrany identit služby Active Directory | Microsoft Docs"
description: "Zjistěte, jak Azure AD Identity Protection vám umožní toolimit hello schopnost útočník tooexploit ohroženými identity nebo zařízení a toosecure identity nebo zařízení, která byla dříve toobe podezření nebo známých ohrožení zabezpečení."
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
ms.openlocfilehash: ecca4f3cdb65585687cf44a80024f26c7cab22ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection"></a><span data-ttu-id="0d397-104">Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="0d397-104">Azure Active Directory Identity Protection</span></span>

<span data-ttu-id="0d397-105">Azure Active Directory ochrany identit je funkce hello Azure AD Premium P2 edici, která vám umožní:</span><span class="sxs-lookup"><span data-stu-id="0d397-105">Azure Active Directory Identity Protection is a feature of hello Azure AD Premium P2 edition that enables you to:</span></span>

- <span data-ttu-id="0d397-106">Zjistit potenciální ohrožení zabezpečení, které ovlivňují identity ve vaší organizaci</span><span class="sxs-lookup"><span data-stu-id="0d397-106">Detect potential vulnerabilities affecting your organization’s identities</span></span>

- <span data-ttu-id="0d397-107">Konfigurace toodetected automatické odpovědi podezřelé se akce, které jsou identity související tooyour organizace</span><span class="sxs-lookup"><span data-stu-id="0d397-107">Configure automated responses toodetected suspicious actions that are related tooyour organization’s identities</span></span>  

- <span data-ttu-id="0d397-108">Prozkoumat podezřelé incidenty a proveďte příslušnou akci tooresolve je</span><span class="sxs-lookup"><span data-stu-id="0d397-108">Investigate suspicious incidents and take appropriate action tooresolve them</span></span>   


## <a name="getting-started"></a><span data-ttu-id="0d397-109">Začínáme</span><span class="sxs-lookup"><span data-stu-id="0d397-109">Getting started</span></span>

<span data-ttu-id="0d397-110">Microsoft zabezpečuje cloudové identity pro více než deset.</span><span class="sxs-lookup"><span data-stu-id="0d397-110">Microsoft secures cloud-based identities for more than a decade.</span></span> <span data-ttu-id="0d397-111">V Azure Active Directory Identity Protection, ve vašem prostředí, můžete použít hello stejné ochrany systémy Microsoft používá toosecure identity.</span><span class="sxs-lookup"><span data-stu-id="0d397-111">With Azure Active Directory Identity Protection, in your environment, you can use hello same protection systems Microsoft uses toosecure identities.</span></span>

<span data-ttu-id="0d397-112">velká většina Hello zabezpečení narušení trvat umístit pokud útočník přístup tooan prostředí tak, že krádež identity uživatele.</span><span class="sxs-lookup"><span data-stu-id="0d397-112">hello vast majority of security breaches take place when attackers gain access tooan environment by stealing a user’s identity.</span></span> <span data-ttu-id="0d397-113">V průběhu let hello se staly útočníci stále efektivní využívání narušení třetích stran a za použití útoky phishing sofistikované.</span><span class="sxs-lookup"><span data-stu-id="0d397-113">Over hello years, attackers have become increasingly effective in leveraging third party breaches and using sophisticated phishing attacks.</span></span> <span data-ttu-id="0d397-114">Jakmile útočník získá přístup tooeven nízkou privilegované uživatelských účtů, je pro ně toogain přístup tooimportant firemním prostředkům prostřednictvím laterální pohyb je poměrně snadné.</span><span class="sxs-lookup"><span data-stu-id="0d397-114">As soon as an attacker gains access tooeven low privileged user accounts, it is relatively easy for them toogain access tooimportant company resources through lateral movement.</span></span>

<span data-ttu-id="0d397-115">V důsledku toho je potřeba:</span><span class="sxs-lookup"><span data-stu-id="0d397-115">As a consequence of this, you need to:</span></span>

- <span data-ttu-id="0d397-116">Chránit všechny identity, bez ohledu na jejich úroveň oprávnění</span><span class="sxs-lookup"><span data-stu-id="0d397-116">Protect all identities regardless of their privilege level</span></span>

- <span data-ttu-id="0d397-117">Proaktivní ohroženými identity zabránit se překročen</span><span class="sxs-lookup"><span data-stu-id="0d397-117">Proactively prevent compromised identities from being abused</span></span>

<span data-ttu-id="0d397-118">Zjišťování ohrožení zabezpečení identity je bez snadno úlohy.</span><span class="sxs-lookup"><span data-stu-id="0d397-118">Discovering compromised identities is no easy task.</span></span> <span data-ttu-id="0d397-119">Azure Active Directory používá algoritmy adaptivní strojového učení a heuristiky toodetect anomálií a podezřelé incidenty, které indikují potenciálně ohrožených identity.</span><span class="sxs-lookup"><span data-stu-id="0d397-119">Azure Active Directory uses adaptive machine learning algorithms and heuristics toodetect anomalies and suspicious incidents that indicate potentially compromised identities.</span></span> <span data-ttu-id="0d397-120">Na základě těchto dat Identity Protection generuje sestavy a výstrahy, které umožňují tooevaluate hello zjištěné problémy a proveďte příslušné zmírnění nebo nápravné akce.</span><span class="sxs-lookup"><span data-stu-id="0d397-120">Using this data, Identity Protection generates reports and alerts that enable you tooevaluate hello detected issues and take appropriate mitigation or remediation actions.</span></span>

<span data-ttu-id="0d397-121">Azure Active Directory Identity Protection je větší než monitorování a vytváření sestav nástroje.</span><span class="sxs-lookup"><span data-stu-id="0d397-121">Azure Active Directory Identity Protection is more than a monitoring and reporting tool.</span></span> <span data-ttu-id="0d397-122">tooprotect identity ve vaší organizaci, můžete nakonfigurovat zásady na základě rizik, které automaticky odpovídat toodetected problémy po dosažení zadané riziko úroveň.</span><span class="sxs-lookup"><span data-stu-id="0d397-122">tooprotect your organization's identities, you can configure risk-based policies that automatically respond toodetected issues when a specified risk level has been reached.</span></span> <span data-ttu-id="0d397-123">Tyto zásady, kromě tooother podmíněný přístup k ovládací prvky, které poskytuje Azure Active Directory a EMS, můžete buď automaticky blokovat nebo zahájit adaptivní nápravné akce, včetně resetování hesel a vynucení služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="0d397-123">These policies, in addition tooother conditional access controls provided by Azure Active Directory and EMS, can either automatically block or initiate adaptive remediation actions including password resets and multi-factor authentication enforcement.</span></span>


#### <a name="identity-protection-capabilities"></a><span data-ttu-id="0d397-124">Funkce ochrany identit</span><span class="sxs-lookup"><span data-stu-id="0d397-124">Identity Protection capabilities</span></span>

<span data-ttu-id="0d397-125">**Zjišťování ohrožení zabezpečení a rizikové účty:**</span><span class="sxs-lookup"><span data-stu-id="0d397-125">**Detecting vulnerabilities and risky accounts:**</span></span>  

* <span data-ttu-id="0d397-126">Poskytování vlastních doporučení tooimprove celkové postavení zabezpečení podle zvýraznění ohrožení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="0d397-126">Providing custom recommendations tooimprove overall security posture by highlighting vulnerabilities</span></span>
* <span data-ttu-id="0d397-127">Výpočet úrovní rizika přihlášení</span><span class="sxs-lookup"><span data-stu-id="0d397-127">Calculating sign-in risk levels</span></span>
* <span data-ttu-id="0d397-128">Výpočet úrovní rizika uživatele</span><span class="sxs-lookup"><span data-stu-id="0d397-128">Calculating user risk levels</span></span>


<span data-ttu-id="0d397-129">**Zkoumání rizikových událostí:**</span><span class="sxs-lookup"><span data-stu-id="0d397-129">**Investigating risk events:**</span></span>

* <span data-ttu-id="0d397-130">Odesílání oznámení o rizikových událostech</span><span class="sxs-lookup"><span data-stu-id="0d397-130">Sending notifications for risk events</span></span>
* <span data-ttu-id="0d397-131">Zkoumání rizikových událostí pomocí relevantní a kontextové informace</span><span class="sxs-lookup"><span data-stu-id="0d397-131">Investigating risk events using relevant and contextual information</span></span>
* <span data-ttu-id="0d397-132">Poskytuje základní pracovních tootrack šetření</span><span class="sxs-lookup"><span data-stu-id="0d397-132">Providing basic workflows tootrack investigations</span></span>
* <span data-ttu-id="0d397-133">Poskytuje snadný přístup tooremediation akcí, jako je resetování hesla</span><span class="sxs-lookup"><span data-stu-id="0d397-133">Providing easy access tooremediation actions such as password reset</span></span>

<span data-ttu-id="0d397-134">**Zásady podmíněného přístupu na základě rizika:**</span><span class="sxs-lookup"><span data-stu-id="0d397-134">**Risk-based conditional access policies:**</span></span>

* <span data-ttu-id="0d397-135">Zásady toomitigate rizikové přihlášení blokování přihlášení nebo že vyřeší problémy spojené služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="0d397-135">Policy toomitigate risky sign-ins by blocking sign-ins or requiring multi-factor authentication challenges.</span></span>
* <span data-ttu-id="0d397-136">Zásady tooblock nebo zabezpečený rizikové uživatelské účty</span><span class="sxs-lookup"><span data-stu-id="0d397-136">Policy tooblock or secure risky user accounts</span></span>
* <span data-ttu-id="0d397-137">Tooregister uživatelé toorequire zásad pro službu Multi-Factor authentication</span><span class="sxs-lookup"><span data-stu-id="0d397-137">Policy toorequire users tooregister for multi-factor authentication</span></span>



## <a name="identity-protection-roles"></a><span data-ttu-id="0d397-138">Role ochranu identity</span><span class="sxs-lookup"><span data-stu-id="0d397-138">Identity Protection roles</span></span>

<span data-ttu-id="0d397-139">tooload vyrovnávání hello správu činnosti týkající se vaší implementace ochrany identit, můžete přiřadit několik rolí.</span><span class="sxs-lookup"><span data-stu-id="0d397-139">tooload balance hello management activities around your Identity Protection implementation, you can assign several roles.</span></span> <span data-ttu-id="0d397-140">Azure AD Identity Protection podporuje 3 directory role:</span><span class="sxs-lookup"><span data-stu-id="0d397-140">Azure AD Identity Protection supports 3 directory roles:</span></span>

| <span data-ttu-id="0d397-141">Role</span><span class="sxs-lookup"><span data-stu-id="0d397-141">Role</span></span>                         | <span data-ttu-id="0d397-142">Můžete provést</span><span class="sxs-lookup"><span data-stu-id="0d397-142">Can do</span></span>                          | <span data-ttu-id="0d397-143">Nelze provést</span><span class="sxs-lookup"><span data-stu-id="0d397-143">Cannot do</span></span>
| :--                          | ---                                |  ---   |
| <span data-ttu-id="0d397-144">Globální správce</span><span class="sxs-lookup"><span data-stu-id="0d397-144">Global administrator</span></span>         | <span data-ttu-id="0d397-145">Úplný přístup tooIdentity ochranu zařadit Identity Protection</span><span class="sxs-lookup"><span data-stu-id="0d397-145">Full access tooIdentity Protection, Onboard Identity Protection</span></span>| |
| <span data-ttu-id="0d397-146">Správce zabezpečení</span><span class="sxs-lookup"><span data-stu-id="0d397-146">Security administrator</span></span>       | <span data-ttu-id="0d397-147">Úplný přístup tooIdentity ochrany</span><span class="sxs-lookup"><span data-stu-id="0d397-147">Full access tooIdentity Protection</span></span> | <span data-ttu-id="0d397-148">Zařadit Identity Protection resetovat hesla pro uživatele</span><span class="sxs-lookup"><span data-stu-id="0d397-148">Onboard Identity Protection,  reset passwords for a user</span></span> |
| <span data-ttu-id="0d397-149">Čtenář zabezpečení</span><span class="sxs-lookup"><span data-stu-id="0d397-149">Security reader</span></span>              | <span data-ttu-id="0d397-150">Přístup jen připravené tooIdentity ochrany</span><span class="sxs-lookup"><span data-stu-id="0d397-150">Ready-only access tooIdentity Protection</span></span> | <span data-ttu-id="0d397-151">Zařadit Identity Protection, uživatelé remidiate, nakonfigurovat zásady, resetování hesla</span><span class="sxs-lookup"><span data-stu-id="0d397-151">Onboard Identity Protection, remidiate users, configure policies,  reset passwords</span></span> |




<span data-ttu-id="0d397-152">Další podrobnosti najdete v tématu [přiřazení rolí správce v Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="0d397-152">For more details, see [Assigning administrator roles in Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md)</span></span>



## <a name="detection"></a><span data-ttu-id="0d397-153">Detection (Detekce)</span><span class="sxs-lookup"><span data-stu-id="0d397-153">Detection</span></span>

### <a name="vulnerabilities"></a><span data-ttu-id="0d397-154">Ohrožení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="0d397-154">Vulnerabilities</span></span>

<span data-ttu-id="0d397-155">Azure Active Directory Identity Protection analýz konfiguraci a zjistí chyby zabezpečení, které můžou mít vliv na identit uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0d397-155">Azure Active Directory Identity Protection analyses your configuration and detects vulnerabilities that can have an impact on your user's identities.</span></span> <span data-ttu-id="0d397-156">Další podrobnosti najdete v tématu [chyb zabezpečení detekovaných službou Azure Active Directory Identity Protection](active-directory-identityprotection-vulnerabilities.md).</span><span class="sxs-lookup"><span data-stu-id="0d397-156">For more details, see [Vulnerabilities detected by Azure Active Directory Identity Protection](active-directory-identityprotection-vulnerabilities.md).</span></span>

### <a name="risk-events"></a><span data-ttu-id="0d397-157">Riziko události</span><span class="sxs-lookup"><span data-stu-id="0d397-157">Risk events</span></span>

<span data-ttu-id="0d397-158">Azure Active Directory používá adaptivní strojového učení algoritmů a heuristiky podezřelé akce toodetect, které jsou související tooyour uživatelských identit.</span><span class="sxs-lookup"><span data-stu-id="0d397-158">Azure Active Directory uses adaptive machine learning algorithms and heuristics toodetect suspicious actions that are related tooyour user's identities.</span></span> <span data-ttu-id="0d397-159">Hello systém vytvoří záznam pro každé zjištěné podezřelé akce.</span><span class="sxs-lookup"><span data-stu-id="0d397-159">hello system creates a record for each detected suspicious action.</span></span> <span data-ttu-id="0d397-160">Tyto záznamy se také označují jako rizikových událostech.</span><span class="sxs-lookup"><span data-stu-id="0d397-160">These records are also known as risk events.</span></span>  
<span data-ttu-id="0d397-161">Další podrobnosti najdete v tématu věnovaném [rizikovým událostem služby Azure Active Directory](active-directory-identity-protection-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="0d397-161">For more details, see [Azure Active Directory risk events](active-directory-identity-protection-risk-events.md).</span></span>


## <a name="investigation"></a><span data-ttu-id="0d397-162">Šetření</span><span class="sxs-lookup"><span data-stu-id="0d397-162">Investigation</span></span>
<span data-ttu-id="0d397-163">Vaše cesty přes Identity Protection obvykle začíná řídicího panelu ochrany identit hello.</span><span class="sxs-lookup"><span data-stu-id="0d397-163">Your journey through Identity Protection typically starts with hello Identity Protection dashboard.</span></span>

<span data-ttu-id="0d397-164">![Náprava](./media/active-directory-identityprotection/1000.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="0d397-164">![Remediation](./media/active-directory-identityprotection/1000.png "Remediation")</span></span>

<span data-ttu-id="0d397-165">řídicí panel Hello poskytuje přístup k:</span><span class="sxs-lookup"><span data-stu-id="0d397-165">hello dashboard gives you access to:</span></span>

* <span data-ttu-id="0d397-166">Sestavy, jako **uživatelé označení příznakem rizik**, **rizik události** a **ohrožení zabezpečení**</span><span class="sxs-lookup"><span data-stu-id="0d397-166">Reports such as **Users flagged for risk**, **Risk events** and **Vulnerabilities**</span></span>
* <span data-ttu-id="0d397-167">Nastavení, jako například hello konfiguraci vašeho **zásady zabezpečení**, **oznámení** a **registrace služby Multi-Factor authentication**</span><span class="sxs-lookup"><span data-stu-id="0d397-167">Settings such as hello configuration of your **Security Policies**, **Notifications** and **multi-factor authentication registration**</span></span>

<span data-ttu-id="0d397-168">Je obvykle počáteční bod pro šetření, což je proces hello kontrole hello aktivity, protokoly a další důležité informace související tooa rizik toodecide událostí, zda jsou potřebné kroky ke zmírnění nebo nápravy, a jak byl hello identity dojde k ohrožení a pochopit, jak hello ohrožené identity byl použit.</span><span class="sxs-lookup"><span data-stu-id="0d397-168">It is typically your starting point for investigation, which is hello process of reviewing hello activities, logs, and other relevant information related tooa risk event toodecide whether remediation or mitigation steps are necessary,  and how hello identity was compromised, and understand how hello compromised identity was used.</span></span>

<span data-ttu-id="0d397-169">Dokáže spojit vaše aktivity toohello šetření [oznámení](active-directory-identityprotection-notifications.md) Azure Active Directory Protection odešle na e-mailu.</span><span class="sxs-lookup"><span data-stu-id="0d397-169">You can tie your investigation activities toohello [notifications](active-directory-identityprotection-notifications.md) Azure Active Directory Protection sends per email.</span></span>

<span data-ttu-id="0d397-170">Hello následující části poskytují další podrobnosti a hello kroky, které jsou související tooan šetření.</span><span class="sxs-lookup"><span data-stu-id="0d397-170">hello following sections provide you with more details and hello steps that are related tooan investigation.</span></span>  


## <a name="risky-sign-ins"></a><span data-ttu-id="0d397-171">Rizikové přihlášení</span><span class="sxs-lookup"><span data-stu-id="0d397-171">Risky sign-ins</span></span>

<span data-ttu-id="0d397-172">Azure Active Directory zjistí [rizik typů událostí](active-directory-reporting-risk-events.md#risk-event-types) v reálném čase a offline.</span><span class="sxs-lookup"><span data-stu-id="0d397-172">Azure Active Directory detects [risk event types](active-directory-reporting-risk-events.md#risk-event-types) in real-time and offline.</span></span> <span data-ttu-id="0d397-173">Každý riziko událost, která byla zjištěna u přihlášení uživatele přispívá tooa logický pojem názvem rizikové přihlášení.</span><span class="sxs-lookup"><span data-stu-id="0d397-173">Each risk event that has been detected for a sign-in of a user contributes tooa logical concept called risky sign-in.</span></span> <span data-ttu-id="0d397-174">Rizikové přihlášení je indikátorem pro pokusu přihlášení, který nemusí provedly legitimní vlastníkem hello uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="0d397-174">A risky sign-in is an indicator for a sign-in attempt that might not have been performed by hello legitimate owner of a user account.</span></span>


### <a name="sign-in-risk-level"></a><span data-ttu-id="0d397-175">Úroveň rizika přihlášení</span><span class="sxs-lookup"><span data-stu-id="0d397-175">Sign-in risk level</span></span>

<span data-ttu-id="0d397-176">Úroveň rizika přihlášení je označením (vysoká, střední nebo nízká) hello pravděpodobnost, že pokus o přihlášení nebyla provedena legitimní vlastníkem hello uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="0d397-176">A sign-in risk level is an indication (High, Medium, or Low) of hello likelihood that a sign-in attempt was not performed by hello legitimate owner of a user account.</span></span>

### <a name="mitigating-sign-in-risk-events"></a><span data-ttu-id="0d397-177">Zmírnění rizika přihlašovací události</span><span class="sxs-lookup"><span data-stu-id="0d397-177">Mitigating sign-in risk events</span></span>

<span data-ttu-id="0d397-178">Omezení rizik je funkce hello toolimit akce útočník tooexploit ohroženými identity nebo zařízení, aniž byste obnovili hello identity nebo zařízení tooa bezpečné stavu.</span><span class="sxs-lookup"><span data-stu-id="0d397-178">A mitigation is an action toolimit hello ability of an attacker tooexploit a compromised identity or device without restoring hello identity or device tooa safe state.</span></span> <span data-ttu-id="0d397-179">Zmírnění nevyřeší předchozí události přihlášení riziko spojené s hello identity nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="0d397-179">A mitigation does not resolve previous sign-in risk events associated with hello identity or device.</span></span>

<span data-ttu-id="0d397-180">toomitigate rizikové přihlášení automaticky, můžete nakonfigurovat policicies přihlášení riziko zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="0d397-180">toomitigate risky sign-ins automatically, you can configure sign-in risk security policicies.</span></span> <span data-ttu-id="0d397-181">Pomocí těchto zásad, zvažte úroveň rizika hello hello uživatele nebo hello přihlášení tooblock rizikové přihlášení nebo vyžadovat hello uživatele tooperform vícefaktorové ověřování.</span><span class="sxs-lookup"><span data-stu-id="0d397-181">Using these policies, you consider hello risk level of hello user or hello sign-in tooblock risky sign-ins or require hello user tooperform multi-factor authentication.</span></span> <span data-ttu-id="0d397-182">Tyto akce může zabránit útočníkům ve využívání odcizené identity toocause poškození a může poskytnout některé čas toosecure hello identity.</span><span class="sxs-lookup"><span data-stu-id="0d397-182">These actions may prevent an attacker from exploiting a stolen identity toocause damage, and may give you some time toosecure hello identity.</span></span>

### <a name="sign-in-risk-security-policy"></a><span data-ttu-id="0d397-183">Zásady zabezpečení riziko přihlášení</span><span class="sxs-lookup"><span data-stu-id="0d397-183">Sign-in risk security policy</span></span>
<span data-ttu-id="0d397-184">Zásady přihlášení riziko je zásadu podmíněného přístupu, která vyhodnotí hello riziko tooa konkrétní přihlášení a použije způsoby zmírnění rizik na základě předem definované podmínky a pravidla.</span><span class="sxs-lookup"><span data-stu-id="0d397-184">A sign-in risk policy is a conditional access policy that evaluates hello risk tooa specific sign-in and applies mitigations based on predefined conditions and rules.</span></span>

<span data-ttu-id="0d397-185">![Zásady přihlášení riziko](./media/active-directory-identityprotection/1014.png "zásad riziko přihlašování")</span><span class="sxs-lookup"><span data-stu-id="0d397-185">![Sign-in risk policy](./media/active-directory-identityprotection/1014.png "Sign-in risk policy")</span></span>

<span data-ttu-id="0d397-186">Azure AD Identity Protection pomáhá spravovat hello zmírnění rizikové přihlášení tím, že vám umožňuje:</span><span class="sxs-lookup"><span data-stu-id="0d397-186">Azure AD Identity Protection helps you manage hello mitigation of risky sign-ins by enabling you to:</span></span>

* <span data-ttu-id="0d397-187">Sada hello uživatelů a skupin hello zásad platí pro:</span><span class="sxs-lookup"><span data-stu-id="0d397-187">Set hello users and groups hello policy applies to:</span></span>

    <span data-ttu-id="0d397-188">![Zásady přihlášení riziko](./media/active-directory-identityprotection/1015.png "zásad riziko přihlašování")</span><span class="sxs-lookup"><span data-stu-id="0d397-188">![Sign-in risk policy](./media/active-directory-identityprotection/1015.png "Sign-in risk policy")</span></span>
* <span data-ttu-id="0d397-189">Nastavení hello přihlášení riziko úrovně prahové hodnoty (nízká, střední nebo vysokou), která spustí hello zásad:</span><span class="sxs-lookup"><span data-stu-id="0d397-189">Set hello sign-in risk level threshold (low, medium, or high) that triggers hello policy:</span></span>

    <span data-ttu-id="0d397-190">![Zásady přihlášení riziko](./media/active-directory-identityprotection/1016.png "zásad riziko přihlašování")</span><span class="sxs-lookup"><span data-stu-id="0d397-190">![Sign-in risk policy](./media/active-directory-identityprotection/1016.png "Sign-in risk policy")</span></span>
* <span data-ttu-id="0d397-191">Sada hello ovládací prvky toobe vynutí při aktivuje hello zásad:</span><span class="sxs-lookup"><span data-stu-id="0d397-191">Set hello controls toobe enforced when hello policy triggers:</span></span>  

    <span data-ttu-id="0d397-192">![Zásady přihlášení riziko](./media/active-directory-identityprotection/1017.png "zásad riziko přihlašování")</span><span class="sxs-lookup"><span data-stu-id="0d397-192">![Sign-in risk policy](./media/active-directory-identityprotection/1017.png "Sign-in risk policy")</span></span>
* <span data-ttu-id="0d397-193">Stav přepínače hello zásad:</span><span class="sxs-lookup"><span data-stu-id="0d397-193">Switch hello state of your policy:</span></span>

    <span data-ttu-id="0d397-194">![Registrace MFA](./media/active-directory-identityprotection/403.png "registrace MFA")</span><span class="sxs-lookup"><span data-stu-id="0d397-194">![MFA Registration](./media/active-directory-identityprotection/403.png "MFA Registration")</span></span>
* <span data-ttu-id="0d397-195">Kontrola a vyhodnocení hello dopad změny před aktivací ho:</span><span class="sxs-lookup"><span data-stu-id="0d397-195">Review and evaluate hello impact of a change before activating it:</span></span>

    <span data-ttu-id="0d397-196">![Zásady přihlášení riziko](./media/active-directory-identityprotection/1018.png "zásad riziko přihlašování")</span><span class="sxs-lookup"><span data-stu-id="0d397-196">![Sign-in risk policy](./media/active-directory-identityprotection/1018.png "Sign-in risk policy")</span></span>

#### <a name="what-you-need-tooknow"></a><span data-ttu-id="0d397-197">Co je třeba tooknow</span><span class="sxs-lookup"><span data-stu-id="0d397-197">What you need tooknow</span></span>
<span data-ttu-id="0d397-198">Můžete nakonfigurovat přihlášení riziko zabezpečení zásady toorequire službou Multi-Factor authentication:</span><span class="sxs-lookup"><span data-stu-id="0d397-198">You can configure a sign-in risk security policy toorequire multi-factor authentication:</span></span>

<span data-ttu-id="0d397-199">![Zásady přihlášení riziko](./media/active-directory-identityprotection/1017.png "zásad riziko přihlašování")</span><span class="sxs-lookup"><span data-stu-id="0d397-199">![Sign-in risk policy](./media/active-directory-identityprotection/1017.png "Sign-in risk policy")</span></span>

<span data-ttu-id="0d397-200">Ale z bezpečnostních důvodů se toto nastavení funguje pouze pro uživatele, kteří již byl registrován pro službu Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="0d397-200">However, for security reasons, this setting only works for users that have already been registered for multi-factor authentication.</span></span> <span data-ttu-id="0d397-201">Pokud se pro uživatele, který dosud není registrován u služby Multi-Factor authentication je splnit hello podmínku toorequire služby Multi-Factor authentication, uživatel hello je blokován.</span><span class="sxs-lookup"><span data-stu-id="0d397-201">If hello condition toorequire multi-factor authentication is satisfied for a user who is not yet registered for multi-factor authentication, hello user is blocked.</span></span>

<span data-ttu-id="0d397-202">Jako osvědčený postup Pokud chcete, aby toorequire vícefaktorového ověřování pro rizikové přihlášení, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0d397-202">As a best practice, if you want toorequire multi-factor authentication for risky sign-ins, you should:</span></span>

1. <span data-ttu-id="0d397-203">Povolit hello [zásady registrace služby Multi-Factor authentication](#multi-factor-authentication-registration-policy) hello vliv na uživatele.</span><span class="sxs-lookup"><span data-stu-id="0d397-203">Enable hello [multi-factor authentication registration policy](#multi-factor-authentication-registration-policy) for hello affected users.</span></span>
2. <span data-ttu-id="0d397-204">Vyžadovat hello ovlivněn toologin uživatelé v tooperform-rizikové relace registrace MFA</span><span class="sxs-lookup"><span data-stu-id="0d397-204">Require hello affected users toologin in a non-risky session tooperform a MFA registration</span></span>

<span data-ttu-id="0d397-205">Dokončení těchto kroků zajistí, že je vyžadované pro rizikové přihlášení vícefaktorové ověřování.</span><span class="sxs-lookup"><span data-stu-id="0d397-205">Completing these steps ensures that multi-factor authentication is required for a risky sign-in.</span></span>

#### <a name="best-practices"></a><span data-ttu-id="0d397-206">Osvědčené postupy</span><span class="sxs-lookup"><span data-stu-id="0d397-206">Best practices</span></span>
<span data-ttu-id="0d397-207">Výběr **vysokou** prahová hodnota snižuje hello stanovený počet zásady se aktivuje a minimalizuje dopad toousers hello.</span><span class="sxs-lookup"><span data-stu-id="0d397-207">Choosing a **High** threshold reduces hello number of times a policy is triggered and minimizes hello impact toousers.</span></span>  

<span data-ttu-id="0d397-208">Ale vyloučí **nízká** a **střední** přihlášení příznakem rizik z hello zásady, které nemusí blokovat útočník z zneužitím ohrožení zabezpečení identity.</span><span class="sxs-lookup"><span data-stu-id="0d397-208">However, it excludes **Low** and **Medium** sign-ins flagged for risk from hello policy, which may not block an attacker from exploiting a compromised identity.</span></span>

<span data-ttu-id="0d397-209">Při nastavení hello zásad</span><span class="sxs-lookup"><span data-stu-id="0d397-209">When setting hello policy,</span></span>

* <span data-ttu-id="0d397-210">Vyloučit uživatele, kteří nepodporují / nemůže mít vícefaktorového ověřování</span><span class="sxs-lookup"><span data-stu-id="0d397-210">Exclude users who do not/cannot have multi-factor authentication</span></span>
* <span data-ttu-id="0d397-211">Vyloučit uživatele v národní prostředí, kde není praktické povolit zásady hello (například žádný přístup toohelpdesk)</span><span class="sxs-lookup"><span data-stu-id="0d397-211">Exclude users in locales where enabling hello policy is not practical (for example no access toohelpdesk)</span></span>
* <span data-ttu-id="0d397-212">Vyloučení uživatelé, kteří jsou pravděpodobně toogenerate spoustu false pozitivních (vývojáři, analytikům zabezpečení)</span><span class="sxs-lookup"><span data-stu-id="0d397-212">Exclude users who are likely toogenerate a lot of false-positives (developers, security analysts)</span></span>
* <span data-ttu-id="0d397-213">Použití **vysokou** prahová hodnota během počáteční zásadách, nebo pokud minimalizujete musí výzvy pohledu koncové uživatele.</span><span class="sxs-lookup"><span data-stu-id="0d397-213">Use a **High** threshold during initial policy roll out, or if you must minimize challenges seen by end users.</span></span>
* <span data-ttu-id="0d397-214">Použití **nízká** prahovou hodnotu, pokud vaše organizace vyžaduje vyšší úroveň zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="0d397-214">Use a **Low**  threshold if your organization requires greater security.</span></span> <span data-ttu-id="0d397-215">Výběr **nízká** prahová hodnota zavádí další uživatelské přihlašovací výzvy, ale zvýšení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="0d397-215">Selecting a **Low** threshold introduces additional user sign-in challenges, but increased security.</span></span>

<span data-ttu-id="0d397-216">doporučené výchozí pro většinu organizací je tooconfigure pravidlo pro Hello **střední** prahová hodnota toostrike rovnováhu mezi využitelností a zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="0d397-216">hello recommended default for most organizations is tooconfigure a rule for a **Medium** threshold toostrike a balance between usability and security.</span></span>

<span data-ttu-id="0d397-217">zásady přihlášení riziko Hello je:</span><span class="sxs-lookup"><span data-stu-id="0d397-217">hello sign-in risk policy is:</span></span>

* <span data-ttu-id="0d397-218">Použité tooall prohlížeče provoz a přihlášení pomocí moderní ověřování.</span><span class="sxs-lookup"><span data-stu-id="0d397-218">Applied tooall browser traffic and sign-ins using modern authentication.</span></span>
* <span data-ttu-id="0d397-219">Není použité tooapplications pomocí starší protokolů zabezpečení zakázáním hello WS-Trust koncovému bodu IDP hello federovaný, jako je například služba AD FS.</span><span class="sxs-lookup"><span data-stu-id="0d397-219">Not applied tooapplications using older security protocols by disabling hello WS-Trust endpoint at hello federated IDP, such as ADFS.</span></span>

<span data-ttu-id="0d397-220">Hello **rizikových událostech** stránky v konzole Identity Protection hello zobrazuje všechny události:</span><span class="sxs-lookup"><span data-stu-id="0d397-220">hello **Risk Events** page in hello Identity Protection console lists all events:</span></span>

* <span data-ttu-id="0d397-221">Použití této zásady</span><span class="sxs-lookup"><span data-stu-id="0d397-221">This policy was applied to</span></span>
* <span data-ttu-id="0d397-222">Můžete zkontrolovat hello aktivity a zjistit, zda byla akce hello odpovídající nebo ne</span><span class="sxs-lookup"><span data-stu-id="0d397-222">You can review hello activity and determine whether hello action was appropriate or not</span></span>

<span data-ttu-id="0d397-223">Přehled hello související uživatelské prostředí, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="0d397-223">For an overview of hello related user experience, see:</span></span>

* [<span data-ttu-id="0d397-224">Obnovení rizikové přihlášení</span><span class="sxs-lookup"><span data-stu-id="0d397-224">Risky sign-in recovery</span></span>](active-directory-identityprotection-flows.md#risky-sign-in-recovery)
* [<span data-ttu-id="0d397-225">Rizikové přihlášení blokován</span><span class="sxs-lookup"><span data-stu-id="0d397-225">Risky sign-in blocked</span></span>](active-directory-identityprotection-flows.md#risky-sign-in-blocked)  
* [<span data-ttu-id="0d397-226">Možnosti přihlášení s Azure AD Identity Protection</span><span class="sxs-lookup"><span data-stu-id="0d397-226">Sign-in experiences with Azure AD Identity Protection</span></span>](active-directory-identityprotection-flows.md)  

<span data-ttu-id="0d397-227">**Dialogové okno související konfigurace hello tooopen**:</span><span class="sxs-lookup"><span data-stu-id="0d397-227">**tooopen hello related configuration dialog**:</span></span>

- <span data-ttu-id="0d397-228">Na hello **Azure AD Identity Protection** okno, v hello **konfigurace** klikněte na tlačítko **zásad přihlašování riziko**.</span><span class="sxs-lookup"><span data-stu-id="0d397-228">On hello **Azure AD Identity Protection** blade, in hello **Configure** section, click **Sign-in risk policy**.</span></span>

    <span data-ttu-id="0d397-229">![Zásady uživatele ridk](./media/active-directory-identityprotection/1014.png "ridk zásady uživatele")</span><span class="sxs-lookup"><span data-stu-id="0d397-229">![User ridk policy](./media/active-directory-identityprotection/1014.png "User ridk policy")</span></span>



## <a name="users-flagged-for-risk"></a><span data-ttu-id="0d397-230">Uživatelé označení příznakem rizika</span><span class="sxs-lookup"><span data-stu-id="0d397-230">Users flagged for risk</span></span>

<span data-ttu-id="0d397-231">Všechny aktivní [rizik události](active-directory-identity-protection-risk-events.md) , byly zjištěny službou Azure Active Directory pro uživatele přispívat tooa logický pojem názvem riziko pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="0d397-231">All active [risk events](active-directory-identity-protection-risk-events.md) that were detected by Azure Active Directory for a user contribute tooa logical concept called user risk.</span></span> <span data-ttu-id="0d397-232">Uživatele s příznakem pro riziko je indikátorem pro uživatelský účet, který byl napaden.</span><span class="sxs-lookup"><span data-stu-id="0d397-232">A user flagged for risk is an indicator for a user account that might have been compromised.</span></span>

![Uživatelé označení příznakem rizika](./media/active-directory-identityprotection/1200.png)


### <a name="user-risk-level"></a><span data-ttu-id="0d397-234">Úroveň rizika uživatele</span><span class="sxs-lookup"><span data-stu-id="0d397-234">User risk level</span></span>

<span data-ttu-id="0d397-235">Úroveň rizika uživatel je to znamenat (vysoká, střední nebo nízká) hello pravděpodobnost, že identita hello uživatele došlo k ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="0d397-235">A user risk level is an indication (High, Medium, or Low) of hello likelihood that hello user’s identity has been compromised.</span></span> <span data-ttu-id="0d397-236">Se počítá na základě na hello uživatelem riziko události, které jsou přidružené k identitě uživatele.</span><span class="sxs-lookup"><span data-stu-id="0d397-236">It is calculated based on hello user risk events that are associated with a user's identity.</span></span>

<span data-ttu-id="0d397-237">Hello stav události riziko je buď **Active** nebo **uzavřeno**.</span><span class="sxs-lookup"><span data-stu-id="0d397-237">hello status of a risk event is either **Active** or **Closed**.</span></span> <span data-ttu-id="0d397-238">Pouze riziko události, které jsou **Active** přispívat toohello uživatele riziko úrovně výpočtu.</span><span class="sxs-lookup"><span data-stu-id="0d397-238">Only risk events that are **Active** contribute toohello user risk level calculation.</span></span>

<span data-ttu-id="0d397-239">úroveň rizika uživatele Hello se počítá pomocí hello následující zadání:</span><span class="sxs-lookup"><span data-stu-id="0d397-239">hello user risk level is calculated using hello following inputs:</span></span>

* <span data-ttu-id="0d397-240">Aktivní rizikových událostí, které mají vliv hello uživatele</span><span class="sxs-lookup"><span data-stu-id="0d397-240">Active risk events impacting hello user</span></span>
* <span data-ttu-id="0d397-241">Úroveň rizika tyto události</span><span class="sxs-lookup"><span data-stu-id="0d397-241">Risk level of these events</span></span>
* <span data-ttu-id="0d397-242">Jestli nebyly provedeny žádné nápravné akce</span><span class="sxs-lookup"><span data-stu-id="0d397-242">Whether any remediation actions have been taken</span></span>

<span data-ttu-id="0d397-243">![Uživatel rizika](./media/active-directory-identityprotection/1031.png "rizika uživatele")</span><span class="sxs-lookup"><span data-stu-id="0d397-243">![User risks](./media/active-directory-identityprotection/1031.png "User risks")</span></span>

<span data-ttu-id="0d397-244">Můžete použít hello uživatele riziko úrovně toocreate zásady podmíněného přístupu které zablokovat rizikové uživatelům přihlášení nebo vynutit je toosecurely změnit své heslo.</span><span class="sxs-lookup"><span data-stu-id="0d397-244">You can use hello user risk levels toocreate conditional access policies that block risky users from signing in, or force them toosecurely change their password.</span></span>

### <a name="closing-risk-events-manually"></a><span data-ttu-id="0d397-245">Zavřením rizikových událostí ručně</span><span class="sxs-lookup"><span data-stu-id="0d397-245">Closing risk events manually</span></span>

<span data-ttu-id="0d397-246">Ve většině případů bude trvat nápravné akce, jako je zabezpečené heslo resetovat tooautomatically zavřít rizikových událostech.</span><span class="sxs-lookup"><span data-stu-id="0d397-246">In most cases, you will take remediation actions such as a secure password reset tooautomatically close risk events.</span></span> <span data-ttu-id="0d397-247">Ale to nemusí být vždy možné.</span><span class="sxs-lookup"><span data-stu-id="0d397-247">However, this might not always be possible.</span></span>  
<span data-ttu-id="0d397-248">Toto je, například hello případu, kdy:</span><span class="sxs-lookup"><span data-stu-id="0d397-248">This is, for example, hello case, when:</span></span>

* <span data-ttu-id="0d397-249">Uživatel s Active rizikových událostí byl odstraněn.</span><span class="sxs-lookup"><span data-stu-id="0d397-249">A user with Active risk events has been deleted</span></span>
* <span data-ttu-id="0d397-250">Šetření zjistí, že hlášené riziko událostí byl provést legitimní uživatel hello</span><span class="sxs-lookup"><span data-stu-id="0d397-250">An investigation reveals that a reported risk event has been perform by hello legitimate user</span></span>

<span data-ttu-id="0d397-251">Protože rizikových událostech, které jsou **Active** přispívat výpočtu riziko toohello uživatele, může mít nižší úroveň rizika ukončením rizikových událostí ručně toomanually.</span><span class="sxs-lookup"><span data-stu-id="0d397-251">Because risk events that are **Active** contribute toohello user risk calculation, you may have toomanually lower a risk level by closing risk events manually.</span></span>  
<span data-ttu-id="0d397-252">Během hello během šetření můžete zvolit tootake některé z těchto akcí toochange hello stav události rizika:</span><span class="sxs-lookup"><span data-stu-id="0d397-252">During hello course of investigation, you can choose tootake any of these actions toochange hello status of a risk event:</span></span>

<span data-ttu-id="0d397-253">![Akce](./media/active-directory-identityprotection/34.png "akce")</span><span class="sxs-lookup"><span data-stu-id="0d397-253">![Actions](./media/active-directory-identityprotection/34.png "Actions")</span></span>

* <span data-ttu-id="0d397-254">**Vyřešte** – Pokud po prozkoumání riziko událostí, trvalo akce odpovídající nápravu mimo ochrany identit a budete mít dojem, že událost riziko hello by se měly zvažovat zavřená, události hello označit jako Vyřešeno.</span><span class="sxs-lookup"><span data-stu-id="0d397-254">**Resolve** - If after investigating a risk event, you took an appropriate remediation action outside Identity Protection, and you believe that hello risk event should be considered closed, mark hello event as Resolved.</span></span> <span data-ttu-id="0d397-255">Vyřešit události nastaví tooClosed stav hello riziko události a události hello riziko už přispějí toouser riziko.</span><span class="sxs-lookup"><span data-stu-id="0d397-255">Resolved events will set hello risk event’s status tooClosed and hello risk event will no longer contribute toouser risk.</span></span>
* <span data-ttu-id="0d397-256">**Označit jako falešně pozitivní** – v některých případech můžete prozkoumat události riziko a zjistit, že byla nesprávně označena jako rizikové.</span><span class="sxs-lookup"><span data-stu-id="0d397-256">**Mark as false-positive** - In some cases, you may investigate a risk event and discover that it was incorrectly flagged as a risky.</span></span> <span data-ttu-id="0d397-257">Můžete snížit počet hello takové výskytů označením hello riziko událostí jako falešně pozitivní.</span><span class="sxs-lookup"><span data-stu-id="0d397-257">You can help reduce hello number of such occurrences by marking hello risk event as False-positive.</span></span> <span data-ttu-id="0d397-258">To vám pomůže hello strojového učení algoritmy tooimprove hello klasifikace podobné události v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="0d397-258">This will help hello machine learning algorithms tooimprove hello classification of similar events in hello future.</span></span> <span data-ttu-id="0d397-259">Stav Hello falešně pozitivní událostí je příliš**uzavřeno** a už přispějí toouser riziko.</span><span class="sxs-lookup"><span data-stu-id="0d397-259">hello status of false-positive events is too**Closed** and they will no longer contribute toouser risk.</span></span>
* <span data-ttu-id="0d397-260">**Ignorovat** – Pokud nebyly provedeny žádné akci automatické nápravy, ale aby hello riziko událostí toobe odebral ze seznamu active hello, můžete označit riziko událostí ignorovat a bude uzavřen hello stav události.</span><span class="sxs-lookup"><span data-stu-id="0d397-260">**Ignore** - If you have not taken any remediation action, but want hello risk event toobe removed from hello active list, you can mark a risk event Ignore and hello event status will be Closed.</span></span> <span data-ttu-id="0d397-261">Ignoruje události nepřispívají toouser riziko.</span><span class="sxs-lookup"><span data-stu-id="0d397-261">Ignored events do not contribute toouser risk.</span></span> <span data-ttu-id="0d397-262">Tato možnost by měla být použita pouze za neobvyklé okolnosti.</span><span class="sxs-lookup"><span data-stu-id="0d397-262">This option should only be used under unusual circumstances.</span></span>
* <span data-ttu-id="0d397-263">**Znovu aktivovat** -riziko události, které byly ručně (výběrem **vyřešit**, **falešně pozitivní**, nebo **Ignorovat**) lze znovu aktivovat, nastavení hello Stav události zpět příliš**Active**.</span><span class="sxs-lookup"><span data-stu-id="0d397-263">**Reactivate** - Risk events that were manually closed (by choosing **Resolve**, **False positive**, or **Ignore**) can be reactivated, setting hello event status back too**Active**.</span></span> <span data-ttu-id="0d397-264">Opětovně aktivovaných rizikových událostech přispívat toohello uživatele riziko úrovně výpočtu.</span><span class="sxs-lookup"><span data-stu-id="0d397-264">Reactivated risk events contribute toohello user risk level calculation.</span></span> <span data-ttu-id="0d397-265">Nelze znovu aktivovat, rizikových událostech (například zabezpečené heslo resetovat) uzavřeny prostřednictvím nápravy.</span><span class="sxs-lookup"><span data-stu-id="0d397-265">Risk events closed through remediation (such as a secure password reset) cannot be reactivated.</span></span>

<span data-ttu-id="0d397-266">**Dialogové okno související konfigurace hello tooopen**:</span><span class="sxs-lookup"><span data-stu-id="0d397-266">**tooopen hello related configuration dialog**:</span></span>

1. <span data-ttu-id="0d397-267">Na hello **Azure AD Identity Protection** okno, v části **prošetření**, klikněte na tlačítko **rizik události**.</span><span class="sxs-lookup"><span data-stu-id="0d397-267">On hello **Azure AD Identity Protection** blade, under **Investigate**, click **Risk events**.</span></span>

    <span data-ttu-id="0d397-268">![Resetování hesla ruční](./media/active-directory-identityprotection/1002.png "resetování hesla ruční")</span><span class="sxs-lookup"><span data-stu-id="0d397-268">![Manual password reset](./media/active-directory-identityprotection/1002.png "Manual password reset")</span></span>
2. <span data-ttu-id="0d397-269">V hello **rizik události** klikněte na riziko.</span><span class="sxs-lookup"><span data-stu-id="0d397-269">In hello **Risk events** list, click a risk.</span></span>

    <span data-ttu-id="0d397-270">![Resetování hesla ruční](./media/active-directory-identityprotection/1003.png "resetování hesla ruční")</span><span class="sxs-lookup"><span data-stu-id="0d397-270">![Manual password reset](./media/active-directory-identityprotection/1003.png "Manual password reset")</span></span>
3. <span data-ttu-id="0d397-271">V okně hello rizika klikněte pravým tlačítkem na uživatele.</span><span class="sxs-lookup"><span data-stu-id="0d397-271">On hello risk blade, right-click a user.</span></span>

    <span data-ttu-id="0d397-272">![Resetování hesla ruční](./media/active-directory-identityprotection/1004.png "resetování hesla ruční")</span><span class="sxs-lookup"><span data-stu-id="0d397-272">![Manual password reset](./media/active-directory-identityprotection/1004.png "Manual password reset")</span></span>

### <a name="closing-all-risk-events-for-a-user-manually"></a><span data-ttu-id="0d397-273">Zavřít všechny události riziko pro uživatele ručně</span><span class="sxs-lookup"><span data-stu-id="0d397-273">Closing all risk events for a user manually</span></span>
<span data-ttu-id="0d397-274">Místo ručně zavřete riziko události pro uživatele jednotlivě, Azure Active Directory Identity Protection také poskytuje metoda tooclose všechny události pro uživatele s jedním kliknutím.</span><span class="sxs-lookup"><span data-stu-id="0d397-274">Instead of manually closing risk events for a user individually, Azure Active Directory Identity Protection also provides you with a method tooclose all events for a user with one click.</span></span>

<span data-ttu-id="0d397-275">![Akce](./media/active-directory-identityprotection/2222.png "akce")</span><span class="sxs-lookup"><span data-stu-id="0d397-275">![Actions](./media/active-directory-identityprotection/2222.png "Actions")</span></span>

<span data-ttu-id="0d397-276">Když kliknete na tlačítko **zavřít všechny události**, jsou zavřeny všechny události a hello vliv na uživatele již není v ohrožení.</span><span class="sxs-lookup"><span data-stu-id="0d397-276">When you click **Dismiss all events**, all events are closed and hello affected user is no longer at risk.</span></span>

### <a name="remediating-user-risk-events"></a><span data-ttu-id="0d397-277">Nekompatibilních rizikových událostí uživatele</span><span class="sxs-lookup"><span data-stu-id="0d397-277">Remediating user risk events</span></span>

<span data-ttu-id="0d397-278">Nápravy je akci toosecure identity nebo zařízení, která byla dříve by mohly vzbuzovat podezření nebo známé toobe ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="0d397-278">A remediation is an action toosecure an identity or a device that was previously suspected or known toobe compromised.</span></span> <span data-ttu-id="0d397-279">Akce nápravy obnoví hello identity nebo zařízení tooa bezpečné stav a odstraňuje předchozí rizikových událostech spojených s hello identity nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="0d397-279">A remediation action restores hello identity or device tooa safe state, and resolves previous risk events associated with hello identity or device.</span></span>

<span data-ttu-id="0d397-280">tooremediate uživatele rizikových událostí, můžete postupovat následovně:</span><span class="sxs-lookup"><span data-stu-id="0d397-280">tooremediate user risk events, you can:</span></span>

* <span data-ttu-id="0d397-281">Proveďte ručně riziko událostí zabezpečené heslo resetovat tooremediate uživatele</span><span class="sxs-lookup"><span data-stu-id="0d397-281">Perform a secure password reset tooremediate user risk events manually</span></span>
* <span data-ttu-id="0d397-282">Konfigurace uživatel riziko zabezpečení zásady toomitigate nebo automaticky napravit uživatele rizikových událostí</span><span class="sxs-lookup"><span data-stu-id="0d397-282">Configure a user risk security policy toomitigate or remediate user risk events automatically</span></span>
* <span data-ttu-id="0d397-283">Znovu přeinstalovat image hello nakažených zařízení</span><span class="sxs-lookup"><span data-stu-id="0d397-283">Re-image hello infected device</span></span>  

#### <a name="manual-secure-password-reset"></a><span data-ttu-id="0d397-284">Resetování ruční zabezpečeného hesla</span><span class="sxs-lookup"><span data-stu-id="0d397-284">Manual secure password reset</span></span>
<span data-ttu-id="0d397-285">Resetování zabezpečeného hesla je efektivní nápravy pro mnoho událostí riziko a při provádění automaticky zavře tyto události riziko přepočítá úroveň rizika hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="0d397-285">A secure password reset is an effective remediation for many risk events, and when performed, automatically closes these risk events and recalculates hello user risk level.</span></span> <span data-ttu-id="0d397-286">Můžete použít hello Identity Protection řídicí panel tooinitiate obnovení hesla pro rizikové uživatele.</span><span class="sxs-lookup"><span data-stu-id="0d397-286">You can use hello Identity Protection dashboard tooinitiate a password reset for a risky user.</span></span>

<span data-ttu-id="0d397-287">Hello příslušné dialogové okno obsahuje dvě různé metody tooreset heslo:</span><span class="sxs-lookup"><span data-stu-id="0d397-287">hello related dialog provides two different methods tooreset a password:</span></span>

<span data-ttu-id="0d397-288">**Resetovat heslo** – vyberte **vyžadují hello uživatele tooreset své heslo** tooallow hello uživatele tooself obnovit, pokud má uživatel hello zaregistrován u služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="0d397-288">**Reset password** - Select **Require hello user tooreset their password** tooallow hello user tooself-recover if hello user has registered for multi-factor authentication.</span></span> <span data-ttu-id="0d397-289">Během hello uživatele příštím přihlášení bude uživatel hello požadované toosolve vícefaktorového ověřování challenge úspěšně a pak, vynucené toochange hello heslo.</span><span class="sxs-lookup"><span data-stu-id="0d397-289">During hello user's next sign-in, hello user will be required toosolve a multi-factor authentication challenge successfully and then, forced toochange hello password.</span></span> <span data-ttu-id="0d397-290">Tato možnost není dostupná, pokud hello uživatelský účet ještě není registrované služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="0d397-290">This option isn't available if hello user account is not already registered multi-factor authentication.</span></span>

<span data-ttu-id="0d397-291">**Dočasné heslo** – vyberte **generovat dočasné heslo** tooimmediately zneplatnit hello stávající heslo a vytvořit nové dočasné heslo pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="0d397-291">**Temporary password** - Select **Generate a temporary password** tooimmediately invalidate hello existing password, and create a new temporary password for hello user.</span></span> <span data-ttu-id="0d397-292">Odešlete hello nové dočasné heslo tooan alternativní e-mailovou adresu pro hello uživatel nebo správce toohello uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0d397-292">Send hello new temporary password tooan alternate email address for hello user or toohello user's manager.</span></span> <span data-ttu-id="0d397-293">Protože hello heslo je dočasný, bude uživatel hello výzvami toochange hello hesla při přihlášení.</span><span class="sxs-lookup"><span data-stu-id="0d397-293">Because hello password is temporary, hello user will be prompted toochange hello password upon sign-in.</span></span>

<span data-ttu-id="0d397-294">![Zásady](./media/active-directory-identityprotection/1005.png "zásad")</span><span class="sxs-lookup"><span data-stu-id="0d397-294">![Policy](./media/active-directory-identityprotection/1005.png "Policy")</span></span>

<span data-ttu-id="0d397-295">**Dialogové okno související konfigurace hello tooopen**:</span><span class="sxs-lookup"><span data-stu-id="0d397-295">**tooopen hello related configuration dialog**:</span></span>

1. <span data-ttu-id="0d397-296">Na hello **Azure AD Identity Protection** okně klikněte na tlačítko **uživatelé označení příznakem rizik**.</span><span class="sxs-lookup"><span data-stu-id="0d397-296">On hello **Azure AD Identity Protection** blade, click **Users flagged for risk**.</span></span>

    <span data-ttu-id="0d397-297">![Resetování hesla ruční](./media/active-directory-identityprotection/1006.png "resetování hesla ruční")</span><span class="sxs-lookup"><span data-stu-id="0d397-297">![Manual password reset](./media/active-directory-identityprotection/1006.png "Manual password reset")</span></span>
2. <span data-ttu-id="0d397-298">Hello seznam uživatelů vyberte uživatele, který má alespoň jeden rizikových událostech.</span><span class="sxs-lookup"><span data-stu-id="0d397-298">From hello list of users, select a user with at least one risk events.</span></span>

    <span data-ttu-id="0d397-299">![Resetování hesla ruční](./media/active-directory-identityprotection/1007.png "resetování hesla ruční")</span><span class="sxs-lookup"><span data-stu-id="0d397-299">![Manual password reset](./media/active-directory-identityprotection/1007.png "Manual password reset")</span></span>
3. <span data-ttu-id="0d397-300">V okně hello uživatele, klikněte na tlačítko **resetovat heslo**.</span><span class="sxs-lookup"><span data-stu-id="0d397-300">On hello user blade, click **Reset password**.</span></span>

    <span data-ttu-id="0d397-301">![Resetování hesla ruční](./media/active-directory-identityprotection/1008.png "resetování hesla ruční")</span><span class="sxs-lookup"><span data-stu-id="0d397-301">![Manual password reset](./media/active-directory-identityprotection/1008.png "Manual password reset")</span></span>

### <a name="user-risk-security-policy"></a><span data-ttu-id="0d397-302">Zásada zabezpečení riziko uživatelů</span><span class="sxs-lookup"><span data-stu-id="0d397-302">User risk security policy</span></span>
<span data-ttu-id="0d397-303">Zásady uživatele rizik zabezpečení je zásadu podmíněného přístupu, která vyhodnotí hello riziko úrovně tooa konkrétního uživatele a použije nápravy a zmírnění akce na základě předem definované podmínky a pravidla.</span><span class="sxs-lookup"><span data-stu-id="0d397-303">A user risk security policy is a conditional access policy that evaluates hello risk level tooa specific user and applies remediation and mitigation actions based on predefined conditions and rules.</span></span>

<span data-ttu-id="0d397-304">![Zásady uživatele ridk](./media/active-directory-identityprotection/1009.png "ridk zásady uživatele")</span><span class="sxs-lookup"><span data-stu-id="0d397-304">![User ridk policy](./media/active-directory-identityprotection/1009.png "User ridk policy")</span></span>

<span data-ttu-id="0d397-305">Azure AD Identity Protection pomáhá spravovat hello zmírnění a náprava uživatelé označení příznakem rizik a to:</span><span class="sxs-lookup"><span data-stu-id="0d397-305">Azure AD Identity Protection helps you manage hello mitigation and remediation of users flagged for risk by enabling you to:</span></span>

* <span data-ttu-id="0d397-306">Sada hello uživatelů a skupin hello zásad platí pro:</span><span class="sxs-lookup"><span data-stu-id="0d397-306">Set hello users and groups hello policy applies to:</span></span>

    <span data-ttu-id="0d397-307">![Zásady uživatele ridk](./media/active-directory-identityprotection/1010.png "ridk zásady uživatele")</span><span class="sxs-lookup"><span data-stu-id="0d397-307">![User ridk policy](./media/active-directory-identityprotection/1010.png "User ridk policy")</span></span>
* <span data-ttu-id="0d397-308">Nastavení hello uživatele riziko úrovně prahové hodnoty (nízká, střední nebo vysokou), která spustí hello zásad:</span><span class="sxs-lookup"><span data-stu-id="0d397-308">Set hello user risk level threshold (low, medium, or high) that triggers hello policy:</span></span>

    <span data-ttu-id="0d397-309">![Zásady uživatele ridk](./media/active-directory-identityprotection/1011.png "ridk zásady uživatele")</span><span class="sxs-lookup"><span data-stu-id="0d397-309">![User ridk policy](./media/active-directory-identityprotection/1011.png "User ridk policy")</span></span>
* <span data-ttu-id="0d397-310">Sada hello ovládací prvky toobe vynutí při aktivuje hello zásad:</span><span class="sxs-lookup"><span data-stu-id="0d397-310">Set hello controls toobe enforced when hello policy triggers:</span></span>

    <span data-ttu-id="0d397-311">![Zásady uživatele ridk](./media/active-directory-identityprotection/1012.png "ridk zásady uživatele")</span><span class="sxs-lookup"><span data-stu-id="0d397-311">![User ridk policy](./media/active-directory-identityprotection/1012.png "User ridk policy")</span></span>
* <span data-ttu-id="0d397-312">Stav přepínače hello zásad:</span><span class="sxs-lookup"><span data-stu-id="0d397-312">Switch hello state of your policy:</span></span>

    <span data-ttu-id="0d397-313">![Zásady uživatele ridk](./media/active-directory-identityprotection/403.png "registrace MFA")</span><span class="sxs-lookup"><span data-stu-id="0d397-313">![User ridk policy](./media/active-directory-identityprotection/403.png "MFA Registration")</span></span>
* <span data-ttu-id="0d397-314">Kontrola a vyhodnocení hello dopad změny před aktivací ho:</span><span class="sxs-lookup"><span data-stu-id="0d397-314">Review and evaluate hello impact of a change before activating it:</span></span>

    <span data-ttu-id="0d397-315">![Zásady uživatele ridk](./media/active-directory-identityprotection/1013.png "ridk zásady uživatele")</span><span class="sxs-lookup"><span data-stu-id="0d397-315">![User ridk policy](./media/active-directory-identityprotection/1013.png "User ridk policy")</span></span>

<span data-ttu-id="0d397-316">Výběr **vysokou** prahová hodnota snižuje hello stanovený počet zásady se aktivuje a minimalizuje dopad toousers hello.</span><span class="sxs-lookup"><span data-stu-id="0d397-316">Choosing a **High** threshold reduces hello number of times a policy is triggered and minimizes hello impact toousers.</span></span>
<span data-ttu-id="0d397-317">Ale vyloučí **nízká** a **střední** uživatelé označení příznakem rizik z hello zásady, které nemusí zabezpečit identity nebo zařízení, měla by mohly vzbuzovat podezření nebo známé toobe ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="0d397-317">However, it excludes **Low** and **Medium** users flagged for risk from hello policy, which may not secure identities or devices that were previously suspected or known toobe compromised.</span></span>

<span data-ttu-id="0d397-318">Při nastavení hello zásad</span><span class="sxs-lookup"><span data-stu-id="0d397-318">When setting hello policy,</span></span>

* <span data-ttu-id="0d397-319">Vyloučení uživatelé, kteří jsou pravděpodobně toogenerate spoustu false pozitivních (vývojáři, analytikům zabezpečení)</span><span class="sxs-lookup"><span data-stu-id="0d397-319">Exclude users who are likely toogenerate a lot of false-positives (developers, security analysts)</span></span>
* <span data-ttu-id="0d397-320">Vyloučit uživatele v národní prostředí, kde není praktické povolit zásady hello (například žádný přístup toohelpdesk)</span><span class="sxs-lookup"><span data-stu-id="0d397-320">Exclude users in locales where enabling hello policy is not practical (for example no access toohelpdesk)</span></span>
* <span data-ttu-id="0d397-321">Použití **vysokou** prahová hodnota během počáteční zásadách, nebo pokud minimalizujete musí výzvy pohledu koncové uživatele.</span><span class="sxs-lookup"><span data-stu-id="0d397-321">Use a **High** threshold during initial policy roll out, or if you must minimize challenges seen by end users.</span></span>
* <span data-ttu-id="0d397-322">Použití **nízká** prahovou hodnotu, pokud vaše organizace vyžaduje vyšší úroveň zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="0d397-322">Use a **Low** threshold if your organization requires greater security.</span></span> <span data-ttu-id="0d397-323">Výběr **nízká** prahová hodnota zavádí další uživatelské přihlašovací výzvy, ale zvýšení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="0d397-323">Selecting a **Low** threshold introduces additional user sign-in challenges, but increased security.</span></span>

<span data-ttu-id="0d397-324">doporučené výchozí pro většinu organizací je tooconfigure pravidlo pro Hello **střední** prahová hodnota toostrike rovnováhu mezi využitelností a zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="0d397-324">hello recommended default for most organizations is tooconfigure a rule for a **Medium** threshold toostrike a balance between usability and security.</span></span>

<span data-ttu-id="0d397-325">Přehled hello související uživatelské prostředí, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="0d397-325">For an overview of hello related user experience, see:</span></span>

* <span data-ttu-id="0d397-326">[Dojde k ohrožení tok obnovení účtu](active-directory-identityprotection-flows.md#compromised-account-recovery).</span><span class="sxs-lookup"><span data-stu-id="0d397-326">[Compromised account recovery flow](active-directory-identityprotection-flows.md#compromised-account-recovery).</span></span>  
* <span data-ttu-id="0d397-327">[Dojde k ohrožení účet byl uzamčen toku](active-directory-identityprotection-flows.md#compromised-account-blocked).</span><span class="sxs-lookup"><span data-stu-id="0d397-327">[Compromised account blocked flow](active-directory-identityprotection-flows.md#compromised-account-blocked).</span></span>  

<span data-ttu-id="0d397-328">**Dialogové okno související konfigurace hello tooopen**:</span><span class="sxs-lookup"><span data-stu-id="0d397-328">**tooopen hello related configuration dialog**:</span></span>

- <span data-ttu-id="0d397-329">Na hello **Azure AD Identity Protection** okno, v hello **konfigurace** klikněte na tlačítko **zásady uživatele riziko**.</span><span class="sxs-lookup"><span data-stu-id="0d397-329">On hello **Azure AD Identity Protection** blade, in hello **Configure** section, click **User risk policy**.</span></span>

    <span data-ttu-id="0d397-330">![Zásady uživatele ridk](./media/active-directory-identityprotection/1009.png "ridk zásady uživatele")</span><span class="sxs-lookup"><span data-stu-id="0d397-330">![User ridk policy](./media/active-directory-identityprotection/1009.png "User ridk policy")</span></span>

### <a name="mitigating-user-risk-events"></a><span data-ttu-id="0d397-331">Zmírnění rizik událostí uživatele</span><span class="sxs-lookup"><span data-stu-id="0d397-331">Mitigating user risk events</span></span>
<span data-ttu-id="0d397-332">Správci mohou nastavit uživatele riziko zabezpečení zásady tooblock uživatelů při přihlašování v závislosti na úroveň rizika hello.</span><span class="sxs-lookup"><span data-stu-id="0d397-332">Administrators can set a user risk security policy tooblock users upon sign-in depending on hello risk level.</span></span>

<span data-ttu-id="0d397-333">Blokování přihlášení:</span><span class="sxs-lookup"><span data-stu-id="0d397-333">Blocking a sign-in:</span></span>

* <span data-ttu-id="0d397-334">Zabraňuje hello generování nových událostí riziko uživatele pro hello vliv na uživatele</span><span class="sxs-lookup"><span data-stu-id="0d397-334">Prevents hello generation of new user risk events for hello affected user</span></span>
* <span data-ttu-id="0d397-335">Umožňuje správci toomanually napravit hello riziko událostech, které mají vliv identita uživatele hello a obnovte ji tooa zabezpečen.</span><span class="sxs-lookup"><span data-stu-id="0d397-335">Enables administrators toomanually remediate hello risk events affecting hello user's identity and restore it tooa secure state</span></span>



## <a name="multi-factor-authentication-registration-policy"></a><span data-ttu-id="0d397-336">Zásady registrace služby Multi-Factor authentication</span><span class="sxs-lookup"><span data-stu-id="0d397-336">Multi-factor authentication registration policy</span></span>
<span data-ttu-id="0d397-337">Ověřování Azure Multi-Factor authentication je metoda ověřování, který jste vyžadující hello použití více než jen uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="0d397-337">Azure multi-factor authentication is a method of verifying who you are that requires hello use of more than just a username and password.</span></span> <span data-ttu-id="0d397-338">Poskytuje druhou vrstvu zabezpečení toouser přihlášení a transakce.</span><span class="sxs-lookup"><span data-stu-id="0d397-338">It provides a second layer of security toouser sign-ins and transactions.</span></span>  
<span data-ttu-id="0d397-339">Doporučujeme vyžadovat ověřování Azure Multi-Factor authentication pro přihlášení uživatele, protože ho:</span><span class="sxs-lookup"><span data-stu-id="0d397-339">We recommend that you require Azure multi-factor authentication for user sign-ins because it:</span></span>

* <span data-ttu-id="0d397-340">Poskytuje silné ověřování s celou řadu možností snadno ověření</span><span class="sxs-lookup"><span data-stu-id="0d397-340">Delivers strong authentication with a range of easy verification options</span></span>
* <span data-ttu-id="0d397-341">Hraje důležitou roli při přípravě tooprotect vaší organizace a obnovit z účtu ohrožení</span><span class="sxs-lookup"><span data-stu-id="0d397-341">Plays a key role in preparing your organization tooprotect and recover from account compromises</span></span>

<span data-ttu-id="0d397-342">![Zásady uživatele ridk](./media/active-directory-identityprotection/1019.png "ridk zásady uživatele")</span><span class="sxs-lookup"><span data-stu-id="0d397-342">![User ridk policy](./media/active-directory-identityprotection/1019.png "User ridk policy")</span></span>

<span data-ttu-id="0d397-343">Další podrobnosti najdete v tématu [co je Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span><span class="sxs-lookup"><span data-stu-id="0d397-343">For more details, see [What is Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span></span>

<span data-ttu-id="0d397-344">Azure AD Identity Protection pomáhá spravovat hello zavádění registrace služby Multi-Factor authentication tím, že nakonfigurujete zásadu, která umožňuje:</span><span class="sxs-lookup"><span data-stu-id="0d397-344">Azure AD Identity Protection helps you manage hello roll-out of multi-factor authentication registration by configuring a policy that enables you to:</span></span>

* <span data-ttu-id="0d397-345">Sada hello uživatelů a skupin hello zásad platí pro:</span><span class="sxs-lookup"><span data-stu-id="0d397-345">Set hello users and groups hello policy applies to:</span></span>

    <span data-ttu-id="0d397-346">![Zásady vícefaktorového ověřování](./media/active-directory-identityprotection/1020.png "zásad vícefaktorového ověřování")</span><span class="sxs-lookup"><span data-stu-id="0d397-346">![MFA policy](./media/active-directory-identityprotection/1020.png "MFA policy")</span></span>
* <span data-ttu-id="0d397-347">Sada hello ovládací prvky toobe vynutí při hello zásad aktivuje::</span><span class="sxs-lookup"><span data-stu-id="0d397-347">Set hello controls toobe enforced when hello policy triggers::</span></span>  

    <span data-ttu-id="0d397-348">![Zásady vícefaktorového ověřování](./media/active-directory-identityprotection/1021.png "zásad vícefaktorového ověřování")</span><span class="sxs-lookup"><span data-stu-id="0d397-348">![MFA policy](./media/active-directory-identityprotection/1021.png "MFA policy")</span></span>
* <span data-ttu-id="0d397-349">Stav přepínače hello zásad:</span><span class="sxs-lookup"><span data-stu-id="0d397-349">Switch hello state of your policy:</span></span>

    <span data-ttu-id="0d397-350">![Zásady vícefaktorového ověřování](./media/active-directory-identityprotection/403.png "zásad vícefaktorového ověřování")</span><span class="sxs-lookup"><span data-stu-id="0d397-350">![MFA policy](./media/active-directory-identityprotection/403.png "MFA policy")</span></span>
* <span data-ttu-id="0d397-351">Zobrazit aktuální stav registrace hello:</span><span class="sxs-lookup"><span data-stu-id="0d397-351">View hello current registration status:</span></span>

    <span data-ttu-id="0d397-352">![Zásady vícefaktorového ověřování](./media/active-directory-identityprotection/1022.png "zásad vícefaktorového ověřování")</span><span class="sxs-lookup"><span data-stu-id="0d397-352">![MFA policy](./media/active-directory-identityprotection/1022.png "MFA policy")</span></span>

<span data-ttu-id="0d397-353">Přehled hello související uživatelské prostředí, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="0d397-353">For an overview of hello related user experience, see:</span></span>

* <span data-ttu-id="0d397-354">[Postup registrace služby Multi-Factor authentication](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).</span><span class="sxs-lookup"><span data-stu-id="0d397-354">[Multi-factor authentication registration flow](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).</span></span>  
* <span data-ttu-id="0d397-355">[Přihlášení vyskytne s Azure AD Identity Protection](active-directory-identityprotection-flows.md).</span><span class="sxs-lookup"><span data-stu-id="0d397-355">[Sign-in experiences with Azure AD Identity Protection](active-directory-identityprotection-flows.md).</span></span>  

<span data-ttu-id="0d397-356">**Dialogové okno související konfigurace hello tooopen**:</span><span class="sxs-lookup"><span data-stu-id="0d397-356">**tooopen hello related configuration dialog**:</span></span>

- <span data-ttu-id="0d397-357">Na hello **Azure AD Identity Protection** okno, v hello **konfigurace** klikněte na tlačítko **registrace služby Multi-Factor authentication**.</span><span class="sxs-lookup"><span data-stu-id="0d397-357">On hello **Azure AD Identity Protection** blade, in hello **Configure** section, click **Multi-factor authentication registration**.</span></span>

    <span data-ttu-id="0d397-358">![Zásady vícefaktorového ověřování](./media/active-directory-identityprotection/1019.png "zásad vícefaktorového ověřování")</span><span class="sxs-lookup"><span data-stu-id="0d397-358">![MFA policy](./media/active-directory-identityprotection/1019.png "MFA policy")</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d397-359">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0d397-359">Next steps</span></span>
* [<span data-ttu-id="0d397-360">Kanál 9: Azure AD a Identity zobrazení: Identity Protection verze Preview</span><span class="sxs-lookup"><span data-stu-id="0d397-360">Channel 9: Azure AD and Identity Show: Identity Protection Preview</span></span>](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

* [<span data-ttu-id="0d397-361">Povolení ochrany identit Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0d397-361">Enabling Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection-enable.md)

* [<span data-ttu-id="0d397-362">Chyb zabezpečení detekovaných službou Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="0d397-362">Vulnerabilities detected by Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection-vulnerabilities.md)

* [<span data-ttu-id="0d397-363">Azure Active Directory rizikových událostí</span><span class="sxs-lookup"><span data-stu-id="0d397-363">Azure Active Directory risk events</span></span>](active-directory-identity-protection-risk-events.md)

* [<span data-ttu-id="0d397-364">Azure oznámení ochrany identit služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="0d397-364">Azure Active Directory Identity Protection notifications</span></span>](active-directory-identityprotection-notifications.md)

* [<span data-ttu-id="0d397-365">Azure seznam strategií ochrany identit Active Directory</span><span class="sxs-lookup"><span data-stu-id="0d397-365">Azure Active Directory Identity Protection playbook</span></span>](active-directory-identityprotection-playbook.md)

* [<span data-ttu-id="0d397-366">Azure slovník ochrany identit služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="0d397-366">Azure Active Directory Identity Protection glossary</span></span>](active-directory-identityprotection-glossary.md)

* [<span data-ttu-id="0d397-367">Možnosti přihlášení s Azure AD Identity Protection</span><span class="sxs-lookup"><span data-stu-id="0d397-367">Sign-in experiences with Azure AD Identity Protection</span></span>](active-directory-identityprotection-flows.md)

* [<span data-ttu-id="0d397-368">Azure Active Directory identitu ochrana – jak toounblock uživatelů</span><span class="sxs-lookup"><span data-stu-id="0d397-368">Azure Active Directory Identity Protection - How toounblock users</span></span>](active-directory-identityprotection-unblock-howto.md)

* [<span data-ttu-id="0d397-369">Začínáme s Azure Active Directory Identity Protection a Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="0d397-369">Get started with Azure Active Directory Identity Protection and Microsoft Graph</span></span>](active-directory-identityprotection-graph-getting-started.md)
