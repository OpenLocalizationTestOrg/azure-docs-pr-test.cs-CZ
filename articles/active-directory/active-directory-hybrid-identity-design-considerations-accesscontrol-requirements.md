---
title: "Azure Active Directory hybridní identity aspekty návrhu - určit požadavky řízení přístupu | Microsoft Docs"
description: "Obsahuje identitu a identifikovat požadavky přístup k prostředkům pro uživatele v hybridním prostředí."
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
ms.openlocfilehash: 6404940da460461632616fe49f055d50c2a7aba3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a><span data-ttu-id="994c0-103">Určete požadavky řízení přístupu pro vaše řešení hybridní identity</span><span class="sxs-lookup"><span data-stu-id="994c0-103">Determine access control requirements for your hybrid identity solution</span></span>
<span data-ttu-id="994c0-104">Při organizace je návrhu jejich hybridní řešení identit může také používat tuto příležitost ke kontrole požadavků na prostředky, které jsou plánování a zpřístupněte ji pro uživatele přístup.</span><span class="sxs-lookup"><span data-stu-id="994c0-104">When an organization is designing their hybrid identity solution they can also use this opportunity to review access requirements for the resources that they are planning to make it available for users.</span></span> <span data-ttu-id="994c0-105">Přístup k datům mezi všechny čtyři pilíře identity, které jsou:</span><span class="sxs-lookup"><span data-stu-id="994c0-105">The data access cross all four pillars of identity, which are:</span></span>

* <span data-ttu-id="994c0-106">Správa</span><span class="sxs-lookup"><span data-stu-id="994c0-106">Administration</span></span>
* <span data-ttu-id="994c0-107">Authentication</span><span class="sxs-lookup"><span data-stu-id="994c0-107">Authentication</span></span>
* <span data-ttu-id="994c0-108">Autorizace</span><span class="sxs-lookup"><span data-stu-id="994c0-108">Authorization</span></span>
* <span data-ttu-id="994c0-109">Auditování</span><span class="sxs-lookup"><span data-stu-id="994c0-109">Auditing</span></span>

<span data-ttu-id="994c0-110">Oddíly, které řídí se vztahují na ověřování a autorizace ve další podrobnosti, auditování a správu jsou součástí životního cyklu hybridní identity.</span><span class="sxs-lookup"><span data-stu-id="994c0-110">The sections that follows will cover authentication and authorization in more details, administration and auditing are part of the hybrid identity lifecycle.</span></span> <span data-ttu-id="994c0-111">Čtení [určit úlohy správy hybridní identity](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) pro další informace o těchto funkcích.</span><span class="sxs-lookup"><span data-stu-id="994c0-111">Read [Determine hybrid identity management tasks](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) for more information about these capabilities.</span></span>

> [!NOTE]
> <span data-ttu-id="994c0-112">Čtení [The čtyři pilíře z Identity - Identity Management stáří hybridního IT](http://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) Další informace o každé z nich tyto pilíře chybí.</span><span class="sxs-lookup"><span data-stu-id="994c0-112">Read [The Four Pillars of Identity - Identity Management in the Age of Hybrid IT](http://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) for more information about each one of those pillars.</span></span>
> 
> 

## <a name="authentication-and-authorization"></a><span data-ttu-id="994c0-113">Ověřování a autorizace</span><span class="sxs-lookup"><span data-stu-id="994c0-113">Authentication and authorization</span></span>
<span data-ttu-id="994c0-114">Existují různé scénáře pro ověřování a autorizaci, tyto scénáře bude mít specifické požadavky, které musí splňovat hybridní řešení identit, které společnost přechází přijmout.</span><span class="sxs-lookup"><span data-stu-id="994c0-114">There are different scenarios for authentication and authorization, these scenarios will have specific requirements that must be fulfilled by the hybrid identity solution that the company is going to adopt.</span></span> <span data-ttu-id="994c0-115">Scénáře zahrnující komunikace Business to Business (B2B) můžete přidat další výzvy pro správce IT, vzhledem k tomu, že bude nutné zajistit, že organizace používá metodu ověřování a autorizace může komunikovat s jejich obchodními partnery.</span><span class="sxs-lookup"><span data-stu-id="994c0-115">Scenarios involving Business to Business (B2B) communication can add an extra challenge for IT Admins since they will need to ensure that the authentication and authorization method used by the organization can communicate with their business partners.</span></span> <span data-ttu-id="994c0-116">Během procesu návrhu pro požadavky na ověřování a autorizaci zkontrolujte odpovědi na tyto otázky:</span><span class="sxs-lookup"><span data-stu-id="994c0-116">During the designing process for authentication and authorization requirements, ensure that the following questions are answered:</span></span>

* <span data-ttu-id="994c0-117">Bude vaše organizace ověřování a autorizaci jenom uživatelé nacházející se v jejich systém správy identity?</span><span class="sxs-lookup"><span data-stu-id="994c0-117">Will your organization authenticate and authorize only users located at their identity management system?</span></span>
  * <span data-ttu-id="994c0-118">Jsou všechny plány pro scénáře B2B?</span><span class="sxs-lookup"><span data-stu-id="994c0-118">Are there any plans for B2B scenarios?</span></span>
  * <span data-ttu-id="994c0-119">Pokud ano, už znáte protokoly, které (SAML, OAuth, protokol Kerberos, tokeny nebo certifikáty) se použije k připojení obou firmy?</span><span class="sxs-lookup"><span data-stu-id="994c0-119">If yes, do you already know which protocols (SAML, OAuth, Kerberos, Tokens or Certificates) will be used to connect both businesses?</span></span>
* <span data-ttu-id="994c0-120">Podporuje řešení hybridní identity, která se má přijmout podporu těchto protokolů?</span><span class="sxs-lookup"><span data-stu-id="994c0-120">Does the hybrid identity solution that you are going to adopt support those protocols?</span></span>

<span data-ttu-id="994c0-121">Dalším důležitým bodem ke zvážení je, kde budou umístěné úložiště ověřování, který se použije uživatelů a partnery a modelu správy, který se má použít.</span><span class="sxs-lookup"><span data-stu-id="994c0-121">Another important point to consider is where the authentication repository that will be used by users and partners will be located and the administrative model to be used.</span></span> <span data-ttu-id="994c0-122">Vezměte v úvahu následující dvě jádra:</span><span class="sxs-lookup"><span data-stu-id="994c0-122">Consider the following two core options:</span></span>

* <span data-ttu-id="994c0-123">Centralizované: v tomto modelu přihlašovací údaje uživatele, zásad a správu může být centralizovaný místně nebo v cloudu.</span><span class="sxs-lookup"><span data-stu-id="994c0-123">Centralized: in this model the user’s credentials, policies and administration can be centralized on-premises or in the cloud.</span></span>
* <span data-ttu-id="994c0-124">Hybridní: v tomto modelu přihlašovací údaje uživatele, zásad a správu bude centralizovaný místně a replikované v cloudu.</span><span class="sxs-lookup"><span data-stu-id="994c0-124">Hybrid: in this model the user’s credentials, policies and administration will be centralized on-premises and a replicated in the cloud.</span></span>

<span data-ttu-id="994c0-125">Modelu zavede vaší organizace se bude lišit podle jejich obchodní požadavky, je vhodné zodpovědět následující otázky k identifikaci, kde se bude nacházet systém správy identit a správu režimu použít:</span><span class="sxs-lookup"><span data-stu-id="994c0-125">Which model your organization will adopt will vary according to their business requirements, you want to answer the following questions to identify where the identity management system will reside and the administrative mode to use:</span></span>

* <span data-ttu-id="994c0-126">Vaše organizace aktuálně má správy identit místně?</span><span class="sxs-lookup"><span data-stu-id="994c0-126">Does your organization currently have an identity management on-premises?</span></span>
  * <span data-ttu-id="994c0-127">Pokud ano, jejich plánování zajistit jeho?</span><span class="sxs-lookup"><span data-stu-id="994c0-127">If yes, do they plan to keep it?</span></span>
  * <span data-ttu-id="994c0-128">Existují nějaké požadavky nařízení nebo dodržování předpisů, vaše organizace postupujte této určují, kde by měl být umístěn do systému správy identity?</span><span class="sxs-lookup"><span data-stu-id="994c0-128">Are there any regulation or compliance requirements that your organization must follow that dictates where the identity management system should reside?</span></span>
* <span data-ttu-id="994c0-129">Vaše organizace používá jednotné přihlašování pro aplikace umístěné v místě nebo v cloudu?</span><span class="sxs-lookup"><span data-stu-id="994c0-129">Does your organization use single sign-on for apps located on-premises or in the cloud?</span></span>
  * <span data-ttu-id="994c0-130">Pokud ano, přijetí modelu hybridní identity vliv tento proces?</span><span class="sxs-lookup"><span data-stu-id="994c0-130">If yes, does the adoption of a hybrid identity model affect this process?</span></span>

## <a name="access-control"></a><span data-ttu-id="994c0-131">Access Control</span><span class="sxs-lookup"><span data-stu-id="994c0-131">Access Control</span></span>
<span data-ttu-id="994c0-132">Při ověřování a autorizace jsou základní prvky pro povolení přístupu k firemním datům prostřednictvím ověření uživatele, je také důležité řídit úroveň přístupu, který tito uživatelé budou mít a úroveň správci přístupu bude mít přes prostředky jejich správu.</span><span class="sxs-lookup"><span data-stu-id="994c0-132">While authentication and authorization are core elements to enable access to corporate data through user’s validation, it is also important to control the level of access that these users will have and the level of access administrators will have over the resources that they are managing.</span></span> <span data-ttu-id="994c0-133">Hybridní řešení identit musí být schopen poskytnout podrobné přístup k prostředkům, delegování a řízení přístupu na základě rolí.</span><span class="sxs-lookup"><span data-stu-id="994c0-133">Your hybrid identity solution must be able to provide granular access to resources, delegation and role base access control.</span></span> <span data-ttu-id="994c0-134">Ujistěte se, že odpovědi následující otázky týkající se řízení přístupu:</span><span class="sxs-lookup"><span data-stu-id="994c0-134">Ensure that the following question are answered regarding access control:</span></span>

* <span data-ttu-id="994c0-135">Má vaše společnost více než jeden uživatel s oprávněním vyšší úrovně oprávnění ke správě systému identity?</span><span class="sxs-lookup"><span data-stu-id="994c0-135">Does your company have more than one user with elevated privilege to manage your identity system?</span></span>
  * <span data-ttu-id="994c0-136">Pokud ano, každý uživatel potřebuje stejnou úroveň přístupu?</span><span class="sxs-lookup"><span data-stu-id="994c0-136">If yes, does each user need the same access level?</span></span>
* <span data-ttu-id="994c0-137">Vaše společnost potřebovat delegovat přístup k uživatelům spravovat konkrétní prostředky?</span><span class="sxs-lookup"><span data-stu-id="994c0-137">Would your company need to delegate access to users to manage specific resources?</span></span>
  * <span data-ttu-id="994c0-138">Pokud ano, jak často k tomu dojde?</span><span class="sxs-lookup"><span data-stu-id="994c0-138">If yes, how frequently this happens?</span></span>
* <span data-ttu-id="994c0-139">Vaše společnost potřebovat k integraci funkcí řízení přístupu mezi místními a cloudovými prostředky?</span><span class="sxs-lookup"><span data-stu-id="994c0-139">Would your company need to integrate access control capabilities between on-premises and cloud resources?</span></span>
* <span data-ttu-id="994c0-140">Omezit přístup k prostředkům na základě některé podmínky potřebovat vaše společnost?</span><span class="sxs-lookup"><span data-stu-id="994c0-140">Would your company need to limit access to resources according to some conditions?</span></span>
* <span data-ttu-id="994c0-141">By vaše společnost nějaké jakékoli aplikace, která potřebuje vlastního ovládacího prvku přístup k některým prostředkům?</span><span class="sxs-lookup"><span data-stu-id="994c0-141">Would your company have any application that needs custom control access to some resources?</span></span>
  * <span data-ttu-id="994c0-142">Pokud ano, kde jsou tyto aplikace umístěné (místně nebo v cloudu)?</span><span class="sxs-lookup"><span data-stu-id="994c0-142">If yes, where are those apps located (on-premises or in the cloud)?</span></span>
  * <span data-ttu-id="994c0-143">Pokud ano, kde jsou ty, cílové prostředky uložené (místně nebo v cloudu)?</span><span class="sxs-lookup"><span data-stu-id="994c0-143">If yes, where are those target resources located (on-premises or in the cloud)?</span></span>

> [!NOTE]
> <span data-ttu-id="994c0-144">Zajistěte, aby poznamenejte každou odpověď a pochopit důvody odpověď.</span><span class="sxs-lookup"><span data-stu-id="994c0-144">Make sure to take notes of each answer and understand the rationale behind the answer.</span></span> <span data-ttu-id="994c0-145">[Definování strategie ochrany dat](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) budou přenášeny po dostupných možností a výhod i nevýhod jednotlivých možností.</span><span class="sxs-lookup"><span data-stu-id="994c0-145">[Define Data Protection Strategy](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) will go over the options available and advantages/disadvantages of each option.</span></span>  <span data-ttu-id="994c0-146">Odpovědi na tyto otázky budou vybírat, která možnost nejlépe vyhovuje vašim obchodním potřebám.</span><span class="sxs-lookup"><span data-stu-id="994c0-146">By answering those questions you will select which option best suits your business needs.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="994c0-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="994c0-147">Next steps</span></span>
[<span data-ttu-id="994c0-148">Stanovení požadavků na reakce na incidenty</span><span class="sxs-lookup"><span data-stu-id="994c0-148">Determine incident response requirements</span></span>](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a><span data-ttu-id="994c0-149">Viz také</span><span class="sxs-lookup"><span data-stu-id="994c0-149">See Also</span></span>
[<span data-ttu-id="994c0-150">Přehled aspektů návrhu</span><span class="sxs-lookup"><span data-stu-id="994c0-150">Design considerations overview</span></span>](active-directory-hybrid-identity-design-considerations-overview.md)

