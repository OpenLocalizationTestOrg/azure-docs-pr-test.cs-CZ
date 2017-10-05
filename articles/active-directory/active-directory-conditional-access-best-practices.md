---
title: "Osvědčené postupy pro podmíněný přístup v Azure Active Directory | Microsoft Docs"
description: "Další informace o věcí, které byste měli vědět, a co je, že byste neměli dělat při konfiguraci zásad podmíněného přístupu."
services: active-directory
keywords: "podmíněný přístup k aplikacím, podmíněného přístupu s Azure AD, zabezpečený přístup k prostředkům společnosti, zásady podmíněného přístupu"
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
ms.openlocfilehash: 3e524c116479c1af6eb6a601c9b57d27a697c5a2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a><span data-ttu-id="ffcf6-104">Osvědčené postupy pro podmíněný přístup v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ffcf6-104">Best practices for conditional access in Azure Active Directory</span></span>

<span data-ttu-id="ffcf6-105">Toto téma poskytuje informace o věcí, které byste měli vědět, a co je, že byste neměli dělat při konfiguraci zásad podmíněného přístupu.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-105">This topic provides you with information about things you should know and what it is you should avoid doing when configuring conditional access policies.</span></span> <span data-ttu-id="ffcf6-106">Než si přečtete v tomto tématu, měli seznámit se s koncepty a technologiím uvedených v [podmíněný přístup v Azure Active Directory](active-directory-conditional-access-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="ffcf6-106">Before reading this topic, you should familiarize yourself with the concepts and the terminology outlined in [Conditional access in Azure Active Directory](active-directory-conditional-access-azure-portal.md)</span></span>

## <a name="what-you-should-know"></a><span data-ttu-id="ffcf6-107">Důležité informace</span><span class="sxs-lookup"><span data-stu-id="ffcf6-107">What you should know</span></span>

### <a name="whats-required-to-make-a-policy-work"></a><span data-ttu-id="ffcf6-108">Co potřebné k tomu, aby zásada fungovat?</span><span class="sxs-lookup"><span data-stu-id="ffcf6-108">What’s required to make a policy work?</span></span>

<span data-ttu-id="ffcf6-109">Když vytvoříte novou zásadu, neexistují žádné uživatele, skupiny, aplikace nebo vybrané řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-109">When you create a new policy, there are no users, groups, apps or access controls selected.</span></span>

![Cloudové aplikace](./media/active-directory-conditional-access-best-practices/02.png)


<span data-ttu-id="ffcf6-111">Chcete-li vaše zásady fungovat, je nutné nakonfigurovat následující:</span><span class="sxs-lookup"><span data-stu-id="ffcf6-111">To make your policy work, you must configure the following:</span></span>


|<span data-ttu-id="ffcf6-112">Co</span><span class="sxs-lookup"><span data-stu-id="ffcf6-112">What</span></span>           | <span data-ttu-id="ffcf6-113">Postupy</span><span class="sxs-lookup"><span data-stu-id="ffcf6-113">How</span></span>                                  | <span data-ttu-id="ffcf6-114">Proč</span><span class="sxs-lookup"><span data-stu-id="ffcf6-114">Why</span></span>|
|:--            | :--                                  | :-- |
|<span data-ttu-id="ffcf6-115">**Cloudové aplikace**</span><span class="sxs-lookup"><span data-stu-id="ffcf6-115">**Cloud apps**</span></span> |<span data-ttu-id="ffcf6-116">Musíte vybrat jednu nebo více aplikací.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-116">You need to select one or more apps.</span></span>  | <span data-ttu-id="ffcf6-117">Cílem zásad podmíněného přístupu je umožnit a systém doladit jak Autorizovaní uživatelé můžou používat vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-117">The goal of a conditional access policy is to enable you to fine-tune how authorized users can access your apps.</span></span>|
| <span data-ttu-id="ffcf6-118">**Uživatelé a skupiny**</span><span class="sxs-lookup"><span data-stu-id="ffcf6-118">**Users and groups**</span></span> | <span data-ttu-id="ffcf6-119">Je nutné vybrat alespoň jeden uživatel nebo skupina, který má oprávnění pro přístup k cloudové aplikace, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-119">You need to select at least one user or group that is authorized to access the cloud apps you have selected.</span></span> | <span data-ttu-id="ffcf6-120">Zásady podmíněného přístupu, který nemá žádné uživatele a skupiny přiřazené, je neaktivní.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-120">A conditional access policy that has no users and groups assigned, is never triggered.</span></span> |
| <span data-ttu-id="ffcf6-121">**Řízení přístupu**</span><span class="sxs-lookup"><span data-stu-id="ffcf6-121">**Access controls**</span></span> | <span data-ttu-id="ffcf6-122">Musíte vybrat aspoň jedno přístupu řízení.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-122">You need to select at least one access control.</span></span> | <span data-ttu-id="ffcf6-123">Procesor zásad je potřeba vědět, co dělat v případě splnění podmínek.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-123">Your policy processor needs to know what to do if your conditions are satisfied.</span></span>|


<span data-ttu-id="ffcf6-124">Kromě těchto základních požadavků v řadě případů byste měli také nakonfigurovat podmínku.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-124">In addition to these basic requirements, in many cases, you should also configure a condition.</span></span> <span data-ttu-id="ffcf6-125">Zatímco zásady by taky měly fungovat bez nakonfigurovaného podmínky, jsou podmínky řízení faktor pro optimalizaci přístupu k aplikacím.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-125">While a policy would also work without a configured condition, conditions are the driving factor for fine-tuning access to your apps.</span></span>


![Cloudové aplikace](./media/active-directory-conditional-access-best-practices/04.png)



### <a name="how-are-assignments-evaluated"></a><span data-ttu-id="ffcf6-127">Jak se vyhodnocují přiřazení?</span><span class="sxs-lookup"><span data-stu-id="ffcf6-127">How are assignments evaluated?</span></span>

<span data-ttu-id="ffcf6-128">Všechna přiřazení jsou logicky **spojeny**.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-128">All assignments are logically **ANDed**.</span></span> <span data-ttu-id="ffcf6-129">Pokud máte více než jeden přiřazení nakonfigurovaný pro aktivaci a zásady musí být splněny všechna přiřazení.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-129">If you have more than one assignment configured, to trigger a policy, all assignments must be satisfied.</span></span>  

<span data-ttu-id="ffcf6-130">Pokud potřebujete nakonfigurovat umístění podmínku, která platí pro všechna připojení z mimo síti vaší organizace, můžete to provést pomocí:</span><span class="sxs-lookup"><span data-stu-id="ffcf6-130">If you need to configure a location condition that applies to all connections made from outside your organization's network, you can accomplish this by:</span></span>

- <span data-ttu-id="ffcf6-131">Včetně **všech umístění**</span><span class="sxs-lookup"><span data-stu-id="ffcf6-131">Including **All locations**</span></span>
- <span data-ttu-id="ffcf6-132">S výjimkou **všechny důvěryhodné IP adresy**</span><span class="sxs-lookup"><span data-stu-id="ffcf6-132">Excluding **All trusted IPs**</span></span>

### <a name="what-happens-if-you-have-policies-in-the-azure-classic-portal-and-azure-portal-configured"></a><span data-ttu-id="ffcf6-133">Co se stane, pokud máte v portálu Azure classic a portálu Azure, které jsou nakonfigurované zásady?</span><span class="sxs-lookup"><span data-stu-id="ffcf6-133">What happens if you have policies in the Azure classic portal and Azure portal configured?</span></span>  
<span data-ttu-id="ffcf6-134">Obě zásady se vynucují službou Azure Active Directory a uživatel získá přístup jenom v případě, že jsou splněny všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-134">Both policies are enforced by Azure Active Directory and the user gets access only when all requirements are met.</span></span>

### <a name="what-happens-if-you-have-policies-in-the-intune-silverlight-portal-and-the-azure-portal"></a><span data-ttu-id="ffcf6-135">Co se stane, pokud máte zásady v portálu Intune Silverlight a portálu Azure?</span><span class="sxs-lookup"><span data-stu-id="ffcf6-135">What happens if you have policies in the Intune Silverlight portal and the Azure Portal?</span></span>
<span data-ttu-id="ffcf6-136">Obě zásady se vynucují službou Azure Active Directory a uživatel získá přístup jenom v případě, že jsou splněny všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-136">Both policies are enforced by Azure Active Directory and the user gets access only when all requirements are met.</span></span>

### <a name="what-happens-if-i-have-multiple-policies-for-the-same-user-configured"></a><span data-ttu-id="ffcf6-137">Co se stane, když mám několik zásad pro stejného uživatele nakonfigurované?</span><span class="sxs-lookup"><span data-stu-id="ffcf6-137">What happens if I have multiple policies for the same user configured?</span></span>  
<span data-ttu-id="ffcf6-138">Pro každé přihlášení Azure Active Directory vyhodnotí všechny zásady a zajistí, že jsou splněny všechny požadavky před udělil přístup pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-138">For every sign-in, Azure Active Directory evaluates all policies and ensures that all requirements are met before granted access to the user.</span></span>


### <a name="does-conditional-access-work-with-exchange-activesync"></a><span data-ttu-id="ffcf6-139">Podmíněný přístup funguje s Exchange ActiveSync?</span><span class="sxs-lookup"><span data-stu-id="ffcf6-139">Does conditional access work with Exchange ActiveSync?</span></span>

<span data-ttu-id="ffcf6-140">Ano, pomocí protokolu Exchange ActiveSync v zásadách podmíněného přístupu.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-140">Yes, you can use Exchange ActiveSync in a conditional access policy.</span></span>


## <a name="what-you-should-avoid-doing"></a><span data-ttu-id="ffcf6-141">Co byste neměli dělat</span><span class="sxs-lookup"><span data-stu-id="ffcf6-141">What you should avoid doing</span></span>

<span data-ttu-id="ffcf6-142">Rozhraní framework podmíněného přístupu vám poskytne skvělé konfigurace flexibilitu.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-142">The conditional access framework provides you with a great configuration flexibility.</span></span> <span data-ttu-id="ffcf6-143">Ale flexibilitu také znamená, že byste měli pečlivě zkontrolovat každou zásadu konfigurace před uvolněním, aby se zabránilo nežádoucí výsledky.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-143">However, great flexibility  also means that you should carefully review each configuration policy prior to releasing it to avoid undesirable results.</span></span> <span data-ttu-id="ffcf6-144">V tomto kontextu, měli byste věnovat zvláštní pozornost, jako by to ovlivnilo kompletní sady přiřazení **všichni uživatelé / skupiny / cloudových aplikací**.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-144">In this context, you should pay special attention to assignments affecting complete sets such as **all users / groups / cloud apps**.</span></span>

<span data-ttu-id="ffcf6-145">Ve vašem prostředí neměli byste následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="ffcf6-145">In your environment, you should avoid the following configurations:</span></span>


<span data-ttu-id="ffcf6-146">**Pro všechny uživatele všech cloudových aplikací:**</span><span class="sxs-lookup"><span data-stu-id="ffcf6-146">**For all users, all cloud apps:**</span></span>

- <span data-ttu-id="ffcf6-147">**Blokovat přístup** – tato konfigurace zablokuje celé organizace, která není výborný vhodné.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-147">**Block access** - This configuration blocks your entire organization, which is definitely not a good idea.</span></span>

- <span data-ttu-id="ffcf6-148">**Vyžaduje kompatibilní zařízení** – pro uživatele, který nebudete mít zaregistrovali svá zařízení ještě tato zásada blokuje veškerý přístup, včetně přístupu k portálu služby Intune.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-148">**Require compliant device** - For users that don't have enrolled their devices yet, this policy blocks all access including access to the Intune portal.</span></span> <span data-ttu-id="ffcf6-149">Pokud jste správce bez registrovaných zařízení, zablokuje vás tyto zásady z získali zpět do portálu Azure, změna zásad.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-149">If you are an administrator without an enrolled device, this policy blocks you from getting back into the Azure portal to change the policy.</span></span>

- <span data-ttu-id="ffcf6-150">**Vyžadovat připojení k doméně** – tento blok zásad přístupu se taky může blokovat přístup pro všechny uživatele ve vaší organizaci, pokud ještě nemáte zařízení připojených k doméně.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-150">**Require domain join** - This policy block access has also the potential to block access for all users in your organization if you don't have a domain-joined device yet.</span></span>


<span data-ttu-id="ffcf6-151">**Pro všechny uživatele, všechny cloudové aplikace, všechny platformy zařízení:**</span><span class="sxs-lookup"><span data-stu-id="ffcf6-151">**For all users, all cloud apps, all device platforms:**</span></span>

- <span data-ttu-id="ffcf6-152">**Blokovat přístup** – tato konfigurace zablokuje celé organizace, která není výborný vhodné.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-152">**Block access** - This configuration blocks your entire organization, which is definitely not a good idea.</span></span>


## <a name="common-scenarios"></a><span data-ttu-id="ffcf6-153">Obvyklé scénáře</span><span class="sxs-lookup"><span data-stu-id="ffcf6-153">Common scenarios</span></span>

### <a name="requiring-multi-factor-authentication-for-apps"></a><span data-ttu-id="ffcf6-154">Vyžadování vícefaktorového ověřování pro aplikace</span><span class="sxs-lookup"><span data-stu-id="ffcf6-154">Requiring multi-factor authentication for apps</span></span>

<span data-ttu-id="ffcf6-155">Mnoho prostředí mít aplikace, které vyžadují vyšší úroveň ochrany než jiné.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-155">Many environments have apps requiring a higher level of protection than the others.</span></span>
<span data-ttu-id="ffcf6-156">To platí, například pro aplikace, které mají přístup k citlivým datům.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-156">This is, for example, the case for apps that have access to sensitive data.</span></span>
<span data-ttu-id="ffcf6-157">Pokud chcete přidat další vrstvu ochrany do těchto aplikací, můžete nakonfigurovat zásady podmíněného přístupu, který vyžaduje službu Multi-Factor authentication, když uživatelé přistupují těchto aplikací.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-157">If you want to add another layer of protection to these apps, you can configure a conditional access policy that requires multi-factor authentication when users are accessing these apps.</span></span>


### <a name="requiring-multi-factor-authentication-for-access-from-networks-that-are-not-trusted"></a><span data-ttu-id="ffcf6-158">Vyžadování vícefaktorového ověřování pro přístup ze sítě, které nejsou důvěryhodné</span><span class="sxs-lookup"><span data-stu-id="ffcf6-158">Requiring multi-factor authentication for access from networks that are not trusted</span></span>

<span data-ttu-id="ffcf6-159">Tento scénář je podobný předchozímu scénáři, protože ho přidá požadavek pro službu Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-159">This scenario is similar to the previous scenario because it adds a requirement for multi-factor authentication.</span></span>
<span data-ttu-id="ffcf6-160">Hlavní rozdíl je však podmínky pro tento požadavek.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-160">However, the main difference is the condition for this requirement.</span></span>  
<span data-ttu-id="ffcf6-161">Během fokus předchozím scénáři je pro aplikace s přístupem k datům sensitve, je aktivní tohoto scénáře důvěryhodného umístění.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-161">While the focus of the previous scenario was on apps with access to sensitve data, the focus of this scenario is on trusted locations.</span></span>  
<span data-ttu-id="ffcf6-162">Jinými slovy může mít požadavek pro službu Multi-Factor authentication, pokud uživatel ze sítě, kterým nedůvěřujete přístupu k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-162">In other words, you might have a requirement for multi-factor authentication if an app is accessed by a user from a network you don't trust.</span></span>


### <a name="only-trusted-devices-can-access-office-365-services"></a><span data-ttu-id="ffcf6-163">Jenom důvěryhodné zařízení mají přístup ke službám Office 365</span><span class="sxs-lookup"><span data-stu-id="ffcf6-163">Only trusted devices can access Office 365 services</span></span>

<span data-ttu-id="ffcf6-164">Pokud ve svém prostředí používáte Intune, můžete okamžitě začít používat rozhraní zásad podmíněného přístupu v konzole Azure.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-164">If you are using Intune in your environment, you can immediately start using the conditional access policy interface in the Azure console.</span></span>

<span data-ttu-id="ffcf6-165">Mnoho zákazníků Intune použití podmíněného přístupu k zajištění, že pouze důvěryhodné zařízení mají přístup ke službám Office 365.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-165">Many Intune customers are using conditional access to ensure that only trusted devices can access Office 365 services.</span></span> <span data-ttu-id="ffcf6-166">To znamená, že mobilní zařízení jsou zaregistrovaná v Intune a splňovat požadavky zásad dodržování předpisů, a že jsou počítače s Windows připojený k doméně místní.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-166">This means that mobile devices are enrolled with Intune and meet compliance policy requirements, and that Windows PCs are joined to an on-premises domain.</span></span> <span data-ttu-id="ffcf6-167">Klíče zlepšování je, že není nutné nastavit stejné zásady pro každou služeb Office 365.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-167">A key improvement is that you do not have to set the same policy for each of the Office 365 services.</span></span>  <span data-ttu-id="ffcf6-168">Když vytvoříte novou zásadu, nakonfigurujte cloudových aplikací na každý ze O365 aplikace, které chcete chránit pomocí s podmíněným přístupem.</span><span class="sxs-lookup"><span data-stu-id="ffcf6-168">When you create a new policy, configure the Cloud apps to include each of the O365 apps that you wish to protect with  with Conditional Access.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ffcf6-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ffcf6-169">Next steps</span></span>

<span data-ttu-id="ffcf6-170">Pokud chcete vědět, jak konfigurovat zásadu podmíněného přístupu, najdete v článku [Začínáme s podmíněným přístupem v Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ffcf6-170">If you want to know how to configure a conditional access policy, see [Get started with conditional access in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).</span></span>
