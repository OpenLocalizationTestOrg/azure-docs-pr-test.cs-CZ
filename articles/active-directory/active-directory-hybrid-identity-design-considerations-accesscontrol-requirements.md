---
title: "aspekty návrhu rozšíření aaaAzure služby Active Directory hybridní identity - určit požadavky řízení přístupu | Microsoft Docs"
description: "Zahrnuje hello identity a identifikace požadavků na prostředky pro uživatele v hybridním prostředí přístup."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: e3b3b984-0d15-4654-93be-a396324b9f5e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: f0c22629f732a4c13ee7a24456651bec7637c387
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a><span data-ttu-id="6f3a8-103">Určete požadavky řízení přístupu pro vaše řešení hybridní identity</span><span class="sxs-lookup"><span data-stu-id="6f3a8-103">Determine access control requirements for your hybrid identity solution</span></span>
<span data-ttu-id="6f3a8-104">Při organizace je návrhu jejich hybridní řešení identit může také používat tuto možnost tooreview přístup k požadavkům hello prostředky, se plánováním toomake je k dispozici pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="6f3a8-104">When an organization is designing their hybrid identity solution they can also use this opportunity tooreview access requirements for hello resources that they are planning toomake it available for users.</span></span> <span data-ttu-id="6f3a8-105">přístup k datům Hello mezi všechny čtyři pilíře identity, které jsou:</span><span class="sxs-lookup"><span data-stu-id="6f3a8-105">hello data access cross all four pillars of identity, which are:</span></span>

* <span data-ttu-id="6f3a8-106">Správa</span><span class="sxs-lookup"><span data-stu-id="6f3a8-106">Administration</span></span>
* <span data-ttu-id="6f3a8-107">Authentication</span><span class="sxs-lookup"><span data-stu-id="6f3a8-107">Authentication</span></span>
* <span data-ttu-id="6f3a8-108">Autorizace</span><span class="sxs-lookup"><span data-stu-id="6f3a8-108">Authorization</span></span>
* <span data-ttu-id="6f3a8-109">Auditování</span><span class="sxs-lookup"><span data-stu-id="6f3a8-109">Auditing</span></span>

<span data-ttu-id="6f3a8-110">Hello oddíly, které řídí se vztahují na ověřování a autorizace ve další podrobnosti, auditování a správu jsou součástí hello hybridní identity životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="6f3a8-110">hello sections that follows will cover authentication and authorization in more details, administration and auditing are part of hello hybrid identity lifecycle.</span></span> <span data-ttu-id="6f3a8-111">Čtení [určit úlohy správy hybridní identity](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) pro další informace o těchto funkcích.</span><span class="sxs-lookup"><span data-stu-id="6f3a8-111">Read [Determine hybrid identity management tasks](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) for more information about these capabilities.</span></span>

> [!NOTE]
> <span data-ttu-id="6f3a8-112">Čtení [hello čtyři pilíře z Identity - Identity Management hello stáří hybridního IT](http://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) Další informace o každé z nich tyto pilíře chybí.</span><span class="sxs-lookup"><span data-stu-id="6f3a8-112">Read [hello Four Pillars of Identity - Identity Management in hello Age of Hybrid IT](http://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) for more information about each one of those pillars.</span></span>
> 
> 

## <a name="authentication-and-authorization"></a><span data-ttu-id="6f3a8-113">Ověřování a autorizace</span><span class="sxs-lookup"><span data-stu-id="6f3a8-113">Authentication and authorization</span></span>
<span data-ttu-id="6f3a8-114">Existují různé scénáře pro ověřování a autorizaci, tyto scénáře bude mít specifické požadavky, které musí splňovat hello hybridní řešení identit, je hello společnosti tooadopt probíhající.</span><span class="sxs-lookup"><span data-stu-id="6f3a8-114">There are different scenarios for authentication and authorization, these scenarios will have specific requirements that must be fulfilled by hello hybrid identity solution that hello company is going tooadopt.</span></span> <span data-ttu-id="6f3a8-115">Scénáře zahrnující obchodní tooBusiness (B2B) komunikace můžete přidat další výzvu pro správce IT vzhledem k tomu, že bude nutné tooensure, který hello ověřování a autorizace metodu používanou hello organizace může komunikovat s jejich obchodními partnery.</span><span class="sxs-lookup"><span data-stu-id="6f3a8-115">Scenarios involving Business tooBusiness (B2B) communication can add an extra challenge for IT Admins since they will need tooensure that hello authentication and authorization method used by hello organization can communicate with their business partners.</span></span> <span data-ttu-id="6f3a8-116">Během procesu ověřování a autorizaci požadavků na návrh hello Ujistěte se, že hello následující otázky jsou zodpovězeny:</span><span class="sxs-lookup"><span data-stu-id="6f3a8-116">During hello designing process for authentication and authorization requirements, ensure that hello following questions are answered:</span></span>

* <span data-ttu-id="6f3a8-117">Bude vaše organizace ověřování a autorizaci jenom uživatelé nacházející se v jejich systém správy identity?</span><span class="sxs-lookup"><span data-stu-id="6f3a8-117">Will your organization authenticate and authorize only users located at their identity management system?</span></span>
  * <span data-ttu-id="6f3a8-118">Jsou všechny plány pro scénáře B2B?</span><span class="sxs-lookup"><span data-stu-id="6f3a8-118">Are there any plans for B2B scenarios?</span></span>
  * <span data-ttu-id="6f3a8-119">Pokud ano, už víte, které protokoly (SAML, OAuth, protokol Kerberos, tokeny nebo certifikáty) se být použité tooconnect obou firmy?</span><span class="sxs-lookup"><span data-stu-id="6f3a8-119">If yes, do you already know which protocols (SAML, OAuth, Kerberos, Tokens or Certificates) will be used tooconnect both businesses?</span></span>
* <span data-ttu-id="6f3a8-120">Podporuje řešení hybridní identity hello budete tooadopt tyto protokoly?</span><span class="sxs-lookup"><span data-stu-id="6f3a8-120">Does hello hybrid identity solution that you are going tooadopt support those protocols?</span></span>

<span data-ttu-id="6f3a8-121">Jinou důležitá tooconsider bod je, kde budou umístěné hello ověřování úložiště, který se použije uživatelů a partnery a toobe model správy hello používá.</span><span class="sxs-lookup"><span data-stu-id="6f3a8-121">Another important point tooconsider is where hello authentication repository that will be used by users and partners will be located and hello administrative model toobe used.</span></span> <span data-ttu-id="6f3a8-122">Vezměte v úvahu hello následující dvě základní možnosti:</span><span class="sxs-lookup"><span data-stu-id="6f3a8-122">Consider hello following two core options:</span></span>

* <span data-ttu-id="6f3a8-123">Centralizované: v této modelu hello přihlašovací údaje uživatele, zásad a správu může být centralizovaný místně nebo v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="6f3a8-123">Centralized: in this model hello user’s credentials, policies and administration can be centralized on-premises or in hello cloud.</span></span>
* <span data-ttu-id="6f3a8-124">Hybridní: v této modelu hello přihlašovací údaje uživatele, zásad a správu bude centralizovaný místně a replikované v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="6f3a8-124">Hybrid: in this model hello user’s credentials, policies and administration will be centralized on-premises and a replicated in hello cloud.</span></span>

<span data-ttu-id="6f3a8-125">Vaše organizace zavede modelu se bude lišit podle tootheir obchodní požadavky, je vhodné tooanswer hello následující otázky tooidentify, kde bude systém správy identit hello nacházet a hello toouse správu režimu:</span><span class="sxs-lookup"><span data-stu-id="6f3a8-125">Which model your organization will adopt will vary according tootheir business requirements, you want tooanswer hello following questions tooidentify where hello identity management system will reside and hello administrative mode toouse:</span></span>

* <span data-ttu-id="6f3a8-126">Vaše organizace aktuálně má správy identit místně?</span><span class="sxs-lookup"><span data-stu-id="6f3a8-126">Does your organization currently have an identity management on-premises?</span></span>
  * <span data-ttu-id="6f3a8-127">Pokud ano, proveďte jejich plánování tookeep ho?</span><span class="sxs-lookup"><span data-stu-id="6f3a8-127">If yes, do they plan tookeep it?</span></span>
  * <span data-ttu-id="6f3a8-128">Existují nějaké požadavky nařízení nebo dodržování předpisů, vaše organizace postupujte této určují, kde by měl být umístěn systém správy identit hello?</span><span class="sxs-lookup"><span data-stu-id="6f3a8-128">Are there any regulation or compliance requirements that your organization must follow that dictates where hello identity management system should reside?</span></span>
* <span data-ttu-id="6f3a8-129">Vaše organizace používá jednotné přihlašování pro aplikace umístěné v místě nebo v cloudu hello?</span><span class="sxs-lookup"><span data-stu-id="6f3a8-129">Does your organization use single sign-on for apps located on-premises or in hello cloud?</span></span>
  * <span data-ttu-id="6f3a8-130">Pokud ano, hello přijetí modelu hybridní identity vliv tento proces?</span><span class="sxs-lookup"><span data-stu-id="6f3a8-130">If yes, does hello adoption of a hybrid identity model affect this process?</span></span>

## <a name="access-control"></a><span data-ttu-id="6f3a8-131">Access Control</span><span class="sxs-lookup"><span data-stu-id="6f3a8-131">Access Control</span></span>
<span data-ttu-id="6f3a8-132">Při ověřování a autorizace jsou základní prvky tooenable přístup toocorporate data prostřednictvím ověření uživatele, je také důležité toocontrol hello úroveň přístupu, který tito uživatelé budou mít a hello úroveň správci přístupu bude mít přes hello prostředky, které budou spravovat.</span><span class="sxs-lookup"><span data-stu-id="6f3a8-132">While authentication and authorization are core elements tooenable access toocorporate data through user’s validation, it is also important toocontrol hello level of access that these users will have and hello level of access administrators will have over hello resources that they are managing.</span></span> <span data-ttu-id="6f3a8-133">Hybridní řešení identit musí být schopný tooprovide tooresources granulární přístup a delegování řízení přístupu na základě rolí.</span><span class="sxs-lookup"><span data-stu-id="6f3a8-133">Your hybrid identity solution must be able tooprovide granular access tooresources, delegation and role base access control.</span></span> <span data-ttu-id="6f3a8-134">Ujistěte se, že hello následující otázky jsou zodpovězeny týkající se řízení přístupu:</span><span class="sxs-lookup"><span data-stu-id="6f3a8-134">Ensure that hello following question are answered regarding access control:</span></span>

* <span data-ttu-id="6f3a8-135">Vaše společnost má více než jeden uživatel s oprávněním vyšší úrovně oprávnění toomanage systému identity?</span><span class="sxs-lookup"><span data-stu-id="6f3a8-135">Does your company have more than one user with elevated privilege toomanage your identity system?</span></span>
  * <span data-ttu-id="6f3a8-136">Pokud ano, každý uživatel potřebovat hello stejnou úroveň přístupu?</span><span class="sxs-lookup"><span data-stu-id="6f3a8-136">If yes, does each user need hello same access level?</span></span>
* <span data-ttu-id="6f3a8-137">By vaše podnikové potřeby toodelegate přístup toousers toomanage konkrétní prostředky?</span><span class="sxs-lookup"><span data-stu-id="6f3a8-137">Would your company need toodelegate access toousers toomanage specific resources?</span></span>
  * <span data-ttu-id="6f3a8-138">Pokud ano, jak často k tomu dojde?</span><span class="sxs-lookup"><span data-stu-id="6f3a8-138">If yes, how frequently this happens?</span></span>
* <span data-ttu-id="6f3a8-139">Vaše společnost potřebovat možnosti řízení přístupu toointegrate mezi místními a cloudovými prostředky?</span><span class="sxs-lookup"><span data-stu-id="6f3a8-139">Would your company need toointegrate access control capabilities between on-premises and cloud resources?</span></span>
* <span data-ttu-id="6f3a8-140">Vaše společnost potřebovat tooresources toolimit přístup podle podmínek toosome?</span><span class="sxs-lookup"><span data-stu-id="6f3a8-140">Would your company need toolimit access tooresources according toosome conditions?</span></span>
* <span data-ttu-id="6f3a8-141">By vaše společnost nějaké jakékoli aplikace, která potřebuje vlastního ovládacího prvku, přístup k prostředkům toosome?</span><span class="sxs-lookup"><span data-stu-id="6f3a8-141">Would your company have any application that needs custom control access toosome resources?</span></span>
  * <span data-ttu-id="6f3a8-142">Pokud ano, kde jsou tyto aplikace umístěné (místně nebo v cloudu hello)?</span><span class="sxs-lookup"><span data-stu-id="6f3a8-142">If yes, where are those apps located (on-premises or in hello cloud)?</span></span>
  * <span data-ttu-id="6f3a8-143">Pokud ano, kde jsou ty, cílové prostředky uložené (místně nebo v cloudu hello)?</span><span class="sxs-lookup"><span data-stu-id="6f3a8-143">If yes, where are those target resources located (on-premises or in hello cloud)?</span></span>

> [!NOTE]
> <span data-ttu-id="6f3a8-144">Ujistěte se, že tootake poznámky u každé odpovědi a pochopit hello důvody, které hello odpovědí.</span><span class="sxs-lookup"><span data-stu-id="6f3a8-144">Make sure tootake notes of each answer and understand hello rationale behind hello answer.</span></span> <span data-ttu-id="6f3a8-145">[Definování strategie ochrany dat](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) budou přenášeny po hello dostupných možností a výhod i nevýhod jednotlivých možností.</span><span class="sxs-lookup"><span data-stu-id="6f3a8-145">[Define Data Protection Strategy](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) will go over hello options available and advantages/disadvantages of each option.</span></span>  <span data-ttu-id="6f3a8-146">Odpovědi na tyto otázky budou vybírat, která možnost nejlépe vyhovuje vašim obchodním potřebám.</span><span class="sxs-lookup"><span data-stu-id="6f3a8-146">By answering those questions you will select which option best suits your business needs.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="6f3a8-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6f3a8-147">Next steps</span></span>
[<span data-ttu-id="6f3a8-148">Stanovení požadavků na reakce na incidenty</span><span class="sxs-lookup"><span data-stu-id="6f3a8-148">Determine incident response requirements</span></span>](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a><span data-ttu-id="6f3a8-149">Viz také</span><span class="sxs-lookup"><span data-stu-id="6f3a8-149">See Also</span></span>
[<span data-ttu-id="6f3a8-150">Přehled aspektů návrhu</span><span class="sxs-lookup"><span data-stu-id="6f3a8-150">Design considerations overview</span></span>](active-directory-hybrid-identity-design-considerations-overview.md)

