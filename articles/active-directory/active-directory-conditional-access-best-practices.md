---
title: "aaaBest postupy pro podmíněný přístup v Azure Active Directory | Microsoft Docs"
description: "Další informace o věcí, které byste měli vědět, a co je, že byste neměli dělat při konfiguraci zásad podmíněného přístupu."
services: active-directory
keywords: "podmíněný přístup tooapps, podmíněného přístupu s Azure AD, zabezpečení přístupu k prostředkům toocompany, zásady podmíněného přístupu"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 4952f8746a2e583380b3bb99cfe2fbdae1c07b08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a><span data-ttu-id="ad1f8-104">Osvědčené postupy pro podmíněný přístup v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ad1f8-104">Best practices for conditional access in Azure Active Directory</span></span>

<span data-ttu-id="ad1f8-105">Toto téma poskytuje informace o věcí, které byste měli vědět, a co je, že byste neměli dělat při konfiguraci zásad podmíněného přístupu.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-105">This topic provides you with information about things you should know and what it is you should avoid doing when configuring conditional access policies.</span></span> <span data-ttu-id="ad1f8-106">Než si přečtete v tomto tématu, měli seznámit se s koncepty hello a terminologie hello uvedených v [podmíněný přístup v Azure Active Directory](active-directory-conditional-access-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="ad1f8-106">Before reading this topic, you should familiarize yourself with hello concepts and hello terminology outlined in [Conditional access in Azure Active Directory](active-directory-conditional-access-azure-portal.md)</span></span>

## <a name="what-you-should-know"></a><span data-ttu-id="ad1f8-107">Důležité informace</span><span class="sxs-lookup"><span data-stu-id="ad1f8-107">What you should know</span></span>

### <a name="whats-required-toomake-a-policy-work"></a><span data-ttu-id="ad1f8-108">Jaké jsou požadavky toomake pracovní zásady?</span><span class="sxs-lookup"><span data-stu-id="ad1f8-108">What’s required toomake a policy work?</span></span>

<span data-ttu-id="ad1f8-109">Když vytvoříte novou zásadu, neexistují žádné uživatele, skupiny, aplikace nebo vybrané řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-109">When you create a new policy, there are no users, groups, apps or access controls selected.</span></span>

![Cloudové aplikace](./media/active-directory-conditional-access-best-practices/02.png)


<span data-ttu-id="ad1f8-111">toomake zásady vaší práci, je nutné nakonfigurovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="ad1f8-111">toomake your policy work, you must configure hello following:</span></span>


|<span data-ttu-id="ad1f8-112">Co</span><span class="sxs-lookup"><span data-stu-id="ad1f8-112">What</span></span>           | <span data-ttu-id="ad1f8-113">Postupy</span><span class="sxs-lookup"><span data-stu-id="ad1f8-113">How</span></span>                                  | <span data-ttu-id="ad1f8-114">Proč</span><span class="sxs-lookup"><span data-stu-id="ad1f8-114">Why</span></span>|
|:--            | :--                                  | :-- |
|<span data-ttu-id="ad1f8-115">**Cloudové aplikace**</span><span class="sxs-lookup"><span data-stu-id="ad1f8-115">**Cloud apps**</span></span> |<span data-ttu-id="ad1f8-116">Je nutné tooselect jednu nebo více aplikací.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-116">You need tooselect one or more apps.</span></span>  | <span data-ttu-id="ad1f8-117">Hello cílem zásad podmíněného přístupu je tooenable toofine tune jak Autorizovaní uživatelé můžou používat vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-117">hello goal of a conditional access policy is tooenable you toofine-tune how authorized users can access your apps.</span></span>|
| <span data-ttu-id="ad1f8-118">**Uživatelé a skupiny**</span><span class="sxs-lookup"><span data-stu-id="ad1f8-118">**Users and groups**</span></span> | <span data-ttu-id="ad1f8-119">Je třeba tooselect alespoň jeden uživatel nebo skupina, která je autorizovaný tooaccess hello cloudových aplikací jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-119">You need tooselect at least one user or group that is authorized tooaccess hello cloud apps you have selected.</span></span> | <span data-ttu-id="ad1f8-120">Zásady podmíněného přístupu, který nemá žádné uživatele a skupiny přiřazené, je neaktivní.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-120">A conditional access policy that has no users and groups assigned, is never triggered.</span></span> |
| <span data-ttu-id="ad1f8-121">**Řízení přístupu**</span><span class="sxs-lookup"><span data-stu-id="ad1f8-121">**Access controls**</span></span> | <span data-ttu-id="ad1f8-122">Je třeba tooselect alespoň jeden řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-122">You need tooselect at least one access control.</span></span> | <span data-ttu-id="ad1f8-123">Procesor zásady musí tooknow, jaké toodo Pokud splnění podmínek.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-123">Your policy processor needs tooknow what toodo if your conditions are satisfied.</span></span>|


<span data-ttu-id="ad1f8-124">V přidání toothese základní požadavky, v řadě případů byste měli nakonfigurovat také podmínku.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-124">In addition toothese basic requirements, in many cases, you should also configure a condition.</span></span> <span data-ttu-id="ad1f8-125">Zatímco zásady by taky měly fungovat bez nakonfigurovaného podmínky, jsou podmínky hello řízení faktor pro optimalizaci přístupu tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-125">While a policy would also work without a configured condition, conditions are hello driving factor for fine-tuning access tooyour apps.</span></span>


![Cloudové aplikace](./media/active-directory-conditional-access-best-practices/04.png)



### <a name="how-are-assignments-evaluated"></a><span data-ttu-id="ad1f8-127">Jak se vyhodnocují přiřazení?</span><span class="sxs-lookup"><span data-stu-id="ad1f8-127">How are assignments evaluated?</span></span>

<span data-ttu-id="ad1f8-128">Všechna přiřazení jsou logicky **spojeny**.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-128">All assignments are logically **ANDed**.</span></span> <span data-ttu-id="ad1f8-129">Pokud máte více než jeden přiřazení nakonfigurované, tootrigger zásady, musí být splněny všechna přiřazení.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-129">If you have more than one assignment configured, tootrigger a policy, all assignments must be satisfied.</span></span>  

<span data-ttu-id="ad1f8-130">Pokud potřebujete tooconfigure umístění podmínku, která používá tooall připojení z mimo síti vaší organizace, můžete to provést pomocí:</span><span class="sxs-lookup"><span data-stu-id="ad1f8-130">If you need tooconfigure a location condition that applies tooall connections made from outside your organization's network, you can accomplish this by:</span></span>

- <span data-ttu-id="ad1f8-131">Včetně **všech umístění**</span><span class="sxs-lookup"><span data-stu-id="ad1f8-131">Including **All locations**</span></span>
- <span data-ttu-id="ad1f8-132">S výjimkou **všechny důvěryhodné IP adresy**</span><span class="sxs-lookup"><span data-stu-id="ad1f8-132">Excluding **All trusted IPs**</span></span>

### <a name="what-happens-if-you-have-policies-in-hello-azure-classic-portal-and-azure-portal-configured"></a><span data-ttu-id="ad1f8-133">Co se stane, pokud máte zásady v hello portál Azure classic a nakonfigurovat portál Azure?</span><span class="sxs-lookup"><span data-stu-id="ad1f8-133">What happens if you have policies in hello Azure classic portal and Azure portal configured?</span></span>  
<span data-ttu-id="ad1f8-134">Obě zásady se vynucují službou Azure Active Directory a hello uživatel získá přístup jenom v případě, že jsou splněny všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-134">Both policies are enforced by Azure Active Directory and hello user gets access only when all requirements are met.</span></span>

### <a name="what-happens-if-you-have-policies-in-hello-intune-silverlight-portal-and-hello-azure-portal"></a><span data-ttu-id="ad1f8-135">Co se stane, pokud máte zásady v hello portál Intune Silverlight a hello portálu Azure?</span><span class="sxs-lookup"><span data-stu-id="ad1f8-135">What happens if you have policies in hello Intune Silverlight portal and hello Azure Portal?</span></span>
<span data-ttu-id="ad1f8-136">Obě zásady se vynucují službou Azure Active Directory a hello uživatel získá přístup jenom v případě, že jsou splněny všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-136">Both policies are enforced by Azure Active Directory and hello user gets access only when all requirements are met.</span></span>

### <a name="what-happens-if-i-have-multiple-policies-for-hello-same-user-configured"></a><span data-ttu-id="ad1f8-137">Co se stane, když mám několik zásad pro hello nakonfigurované stejného uživatele?</span><span class="sxs-lookup"><span data-stu-id="ad1f8-137">What happens if I have multiple policies for hello same user configured?</span></span>  
<span data-ttu-id="ad1f8-138">Pro každé přihlášení Azure Active Directory vyhodnotí všechny zásady a zajistí, že jsou splněny všechny požadavky předtím, než uživatel toohello udělí přístup.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-138">For every sign-in, Azure Active Directory evaluates all policies and ensures that all requirements are met before granted access toohello user.</span></span>


### <a name="does-conditional-access-work-with-exchange-activesync"></a><span data-ttu-id="ad1f8-139">Podmíněný přístup funguje s Exchange ActiveSync?</span><span class="sxs-lookup"><span data-stu-id="ad1f8-139">Does conditional access work with Exchange ActiveSync?</span></span>

<span data-ttu-id="ad1f8-140">Ano, pomocí protokolu Exchange ActiveSync v zásadách podmíněného přístupu.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-140">Yes, you can use Exchange ActiveSync in a conditional access policy.</span></span>


## <a name="what-you-should-avoid-doing"></a><span data-ttu-id="ad1f8-141">Co byste neměli dělat</span><span class="sxs-lookup"><span data-stu-id="ad1f8-141">What you should avoid doing</span></span>

<span data-ttu-id="ad1f8-142">framework Hello podmíněného přístupu vám poskytne skvělé konfigurace flexibilitu.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-142">hello conditional access framework provides you with a great configuration flexibility.</span></span> <span data-ttu-id="ad1f8-143">Flexibilitu však také znamená, pečlivě zkontrolujte to každé konfiguraci zásad před tooreleasing ho tooavoid nežádoucí výsledky.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-143">However, great flexibility  also means that you should carefully review each configuration policy prior tooreleasing it tooavoid undesirable results.</span></span> <span data-ttu-id="ad1f8-144">V tomto kontextu, měli byste věnovat zvláštní pozornost tooassignments, jako by to ovlivnilo kompletní sady **všichni uživatelé / skupiny / cloudových aplikací**.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-144">In this context, you should pay special attention tooassignments affecting complete sets such as **all users / groups / cloud apps**.</span></span>

<span data-ttu-id="ad1f8-145">Ve vašem prostředí neměli byste hello následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="ad1f8-145">In your environment, you should avoid hello following configurations:</span></span>


<span data-ttu-id="ad1f8-146">**Pro všechny uživatele všech cloudových aplikací:**</span><span class="sxs-lookup"><span data-stu-id="ad1f8-146">**For all users, all cloud apps:**</span></span>

- <span data-ttu-id="ad1f8-147">**Blokovat přístup** – tato konfigurace zablokuje celé organizace, která není výborný vhodné.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-147">**Block access** - This configuration blocks your entire organization, which is definitely not a good idea.</span></span>

- <span data-ttu-id="ad1f8-148">**Vyžaduje kompatibilní zařízení** – pro uživatele, který nebudete mít zaregistrovali svá zařízení ještě tato zásada blokuje veškerý přístup, včetně portálu Intune toohello přístup.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-148">**Require compliant device** - For users that don't have enrolled their devices yet, this policy blocks all access including access toohello Intune portal.</span></span> <span data-ttu-id="ad1f8-149">Pokud jste správce bez registrovaných zařízení, zablokuje vás tyto zásady z získali zpět do hello Azure portálu toochange hello zásady.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-149">If you are an administrator without an enrolled device, this policy blocks you from getting back into hello Azure portal toochange hello policy.</span></span>

- <span data-ttu-id="ad1f8-150">**Vyžadovat připojení k doméně** – tento blok zásad přístupu má taky hello potenciální tooblock přístup pro všechny uživatele ve vaší organizaci Pokud ještě nemáte zařízení připojených k doméně.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-150">**Require domain join** - This policy block access has also hello potential tooblock access for all users in your organization if you don't have a domain-joined device yet.</span></span>


<span data-ttu-id="ad1f8-151">**Pro všechny uživatele, všechny cloudové aplikace, všechny platformy zařízení:**</span><span class="sxs-lookup"><span data-stu-id="ad1f8-151">**For all users, all cloud apps, all device platforms:**</span></span>

- <span data-ttu-id="ad1f8-152">**Blokovat přístup** – tato konfigurace zablokuje celé organizace, která není výborný vhodné.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-152">**Block access** - This configuration blocks your entire organization, which is definitely not a good idea.</span></span>


## <a name="common-scenarios"></a><span data-ttu-id="ad1f8-153">Obvyklé scénáře</span><span class="sxs-lookup"><span data-stu-id="ad1f8-153">Common scenarios</span></span>

### <a name="requiring-multi-factor-authentication-for-apps"></a><span data-ttu-id="ad1f8-154">Vyžadování vícefaktorového ověřování pro aplikace</span><span class="sxs-lookup"><span data-stu-id="ad1f8-154">Requiring multi-factor authentication for apps</span></span>

<span data-ttu-id="ad1f8-155">Mnoho prostředí mít aplikace, které vyžadují vyšší úroveň ochrany než hello, ostatní.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-155">Many environments have apps requiring a higher level of protection than hello others.</span></span>
<span data-ttu-id="ad1f8-156">To je třeba hello případ aplikací, které mají přístup k datům toosensitive.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-156">This is, for example, hello case for apps that have access toosensitive data.</span></span>
<span data-ttu-id="ad1f8-157">Pokud chcete tooadd další vrstvu ochrany toothese aplikací, můžete nakonfigurovat zásady podmíněného přístupu, který vyžaduje službu Multi-Factor authentication, když uživatelé přistupují těchto aplikací.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-157">If you want tooadd another layer of protection toothese apps, you can configure a conditional access policy that requires multi-factor authentication when users are accessing these apps.</span></span>


### <a name="requiring-multi-factor-authentication-for-access-from-networks-that-are-not-trusted"></a><span data-ttu-id="ad1f8-158">Vyžadování vícefaktorového ověřování pro přístup ze sítě, které nejsou důvěryhodné</span><span class="sxs-lookup"><span data-stu-id="ad1f8-158">Requiring multi-factor authentication for access from networks that are not trusted</span></span>

<span data-ttu-id="ad1f8-159">Tento scénář je podobný předchozímu scénáři toohello, protože přidá požadavek pro službu Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-159">This scenario is similar toohello previous scenario because it adds a requirement for multi-factor authentication.</span></span>
<span data-ttu-id="ad1f8-160">Hlavní rozdíl hello je však hello podmínky pro tento požadavek.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-160">However, hello main difference is hello condition for this requirement.</span></span>  
<span data-ttu-id="ad1f8-161">Během hello fokus hello předchozí situaci je pro aplikace s daty toosensitve přístup, je aktivní hello tohoto scénáře důvěryhodného umístění.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-161">While hello focus of hello previous scenario was on apps with access toosensitve data, hello focus of this scenario is on trusted locations.</span></span>  
<span data-ttu-id="ad1f8-162">Jinými slovy může mít požadavek pro službu Multi-Factor authentication, pokud uživatel ze sítě, kterým nedůvěřujete přístupu k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-162">In other words, you might have a requirement for multi-factor authentication if an app is accessed by a user from a network you don't trust.</span></span>


### <a name="only-trusted-devices-can-access-office-365-services"></a><span data-ttu-id="ad1f8-163">Jenom důvěryhodné zařízení mají přístup ke službám Office 365</span><span class="sxs-lookup"><span data-stu-id="ad1f8-163">Only trusted devices can access Office 365 services</span></span>

<span data-ttu-id="ad1f8-164">Pokud ve svém prostředí používáte Intune, můžete okamžitě začít používat rozhraní zásad podmíněného přístupu hello v hello konzoly Azure.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-164">If you are using Intune in your environment, you can immediately start using hello conditional access policy interface in hello Azure console.</span></span>

<span data-ttu-id="ad1f8-165">Mnoho zákazníků Intune používáte tooensure podmíněného přístupu, jenom důvěryhodné zařízení mají přístup ke službám Office 365.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-165">Many Intune customers are using conditional access tooensure that only trusted devices can access Office 365 services.</span></span> <span data-ttu-id="ad1f8-166">To znamená, že mobilní zařízení jsou zaregistrovaná v Intune a splňovat požadavky zásad dodržování předpisů, a že jsou počítače s Windows tooan připojené k místní doméně.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-166">This means that mobile devices are enrolled with Intune and meet compliance policy requirements, and that Windows PCs are joined tooan on-premises domain.</span></span> <span data-ttu-id="ad1f8-167">Klíče zlepšování je, že nemáte tooset hello stejné zásady pro jednotlivé služby hello Office 365.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-167">A key improvement is that you do not have tooset hello same policy for each of hello Office 365 services.</span></span>  <span data-ttu-id="ad1f8-168">Když vytvoříte novou zásadu, nakonfigurujte hello cloudové aplikace tooinclude každý hello O365 aplikací, které chcete tooprotect s pomocí podmíněného přístupu.</span><span class="sxs-lookup"><span data-stu-id="ad1f8-168">When you create a new policy, configure hello Cloud apps tooinclude each of hello O365 apps that you wish tooprotect with  with Conditional Access.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ad1f8-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ad1f8-169">Next steps</span></span>

<span data-ttu-id="ad1f8-170">Pokud chcete, aby tooknow jak tooconfigure zásady podmíněného přístupu, najdete v části [Začínáme s podmíněným přístupem v Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ad1f8-170">If you want tooknow how tooconfigure a conditional access policy, see [Get started with conditional access in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).</span></span>
