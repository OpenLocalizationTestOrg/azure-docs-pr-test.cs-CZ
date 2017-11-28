---
title: "aaaSecuring privilegovaného přístupu ve službě Azure AD | Microsoft Docs"
description: "Téma, které vysvětluje hello blíží k zabezpečení privilegovaného přístupu v Azure, Azure Active Directory a služeb Microsoft Online Services."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: mwahl
ms.assetid: 235a0ce9-1daf-4433-8f65-9c6afcd64d08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: kgremban
ms.custom: pim
ms.openlocfilehash: 694835dc5c41640673dbd996d44b0d1f217220de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="securing-privileged-access-in-azure-ad"></a><span data-ttu-id="57d9a-103">Zabezpečení privilegovaného přístupu ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="57d9a-103">Securing privileged access in Azure AD</span></span>
<span data-ttu-id="57d9a-104">Zabezpečení privilegovaného přístupu je důležitým prvním krokem toohelp chránit prostředky obchodní moderní organizace.</span><span class="sxs-lookup"><span data-stu-id="57d9a-104">Securing privileged access is a critical first step toohelp protect business assets in a modern organization.</span></span> <span data-ttu-id="57d9a-105">Privilegované účty jsou účty, které správu a spravovat systémy IT.</span><span class="sxs-lookup"><span data-stu-id="57d9a-105">Privileged accounts are accounts that administer and manage IT systems.</span></span> <span data-ttu-id="57d9a-106">Internetový útočník cílové tyto účty toogain přístup tooan organizace dat a systémy.</span><span class="sxs-lookup"><span data-stu-id="57d9a-106">Cyber-attackers target these accounts toogain access tooan organization’s data and systems.</span></span> <span data-ttu-id="57d9a-107">toosecure privilegovaný přístup, musí izolovat hello účty a systémy ze hello riziko přičemž viditelné tooa uživatel se zlými úmysly.</span><span class="sxs-lookup"><span data-stu-id="57d9a-107">toosecure privileged access, you should isolate hello accounts and systems from hello risk of being exposed tooa malicious user.</span></span>

<span data-ttu-id="57d9a-108">Další uživatelé spouštějí tooget privilegovaný přístup prostřednictvím cloudových služeb.</span><span class="sxs-lookup"><span data-stu-id="57d9a-108">More users are starting tooget privileged access through cloud services.</span></span> <span data-ttu-id="57d9a-109">To může zahrnovat globální správce Office 365, správci předplatného Azure a uživatelé, kteří mají přístup pro správu ve virtuálních počítačích nebo na aplikace SaaS.</span><span class="sxs-lookup"><span data-stu-id="57d9a-109">This can include global administrators of Office365, Azure subscription administrators, and users who have administrative access in VMs or on SaaS apps.</span></span>

<span data-ttu-id="57d9a-110">Společnost Microsoft doporučuje podle tento plán [zabezpečení privilegovaného přístupu](https://technet.microsoft.com/library/mt631194.aspx).</span><span class="sxs-lookup"><span data-stu-id="57d9a-110">Microsoft recommends you follow this roadmap on [Securing Privileged Access](https://technet.microsoft.com/library/mt631194.aspx).</span></span>

<span data-ttu-id="57d9a-111">Tyto zásady se použijí pro zákazníky pomocí Azure Active Directory, Office 365 nebo jiné služby společnosti Microsoft a aplikace, zda jsou uživatelské účty spravovat a ověřovat pomocí služby Active Directory nebo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="57d9a-111">For customers using Azure Active Directory, Office 365, or other Microsoft services and applications, these principles apply whether user accounts are managed and authenticated by Active Directory or in Azure Active Directory.</span></span> <span data-ttu-id="57d9a-112">Hello následující části poskytují další informace o Azure AD toosupport funkce zabezpečení privilegovaného přístupu.</span><span class="sxs-lookup"><span data-stu-id="57d9a-112">hello following sections provide more information on Azure AD features toosupport securing privileged access.</span></span>

## <a name="azure-multi-factor-authentication"></a><span data-ttu-id="57d9a-113">Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="57d9a-113">Azure Multi-Factor Authentication</span></span>
<span data-ttu-id="57d9a-114">zabezpečení hello tooincrease Správce ověřování, byste měli vyžadovat dvoustupňové ověření před udělením oprávnění.</span><span class="sxs-lookup"><span data-stu-id="57d9a-114">tooincrease hello security of administrator authentication, you should require two-step verification before granting privileges.</span></span> <span data-ttu-id="57d9a-115">Dvoustupňové ověření je metoda ověřování, který jste si, že vyžaduje použití hello víc věcí než jenom uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="57d9a-115">Two-step verification is a method of verifying who you are that requires hello use of more than just a username and password.</span></span> <span data-ttu-id="57d9a-116">Poskytuje druhou vrstvu zabezpečení toouser přihlášení a transakce.</span><span class="sxs-lookup"><span data-stu-id="57d9a-116">It provides a second layer of security toouser sign-ins and transactions.</span></span>

<span data-ttu-id="57d9a-117">Azure Multi-Factor Authentication (MFA) se společnosti Microsoft dvoustupňové ověření řešení, která pomáhá zabezpečit přístup toodata a aplikace při splnění požadavků uživatelů pro jednoduchý proces přihlášení.</span><span class="sxs-lookup"><span data-stu-id="57d9a-117">Azure Multi-Factor Authentication (MFA) is Microsoft's two-step verification solution, which helps safeguard access toodata and applications while meeting user demand for a simple sign-in process.</span></span> <span data-ttu-id="57d9a-118">Zajišťuje silné ověřování přes celou řadu možností snadno ověření, včetně:</span><span class="sxs-lookup"><span data-stu-id="57d9a-118">It delivers strong authentication via a range of easy verification options including:</span></span>

- <span data-ttu-id="57d9a-119">telefonní hovory</span><span class="sxs-lookup"><span data-stu-id="57d9a-119">phone calls</span></span>
- <span data-ttu-id="57d9a-120">Textové zprávy</span><span class="sxs-lookup"><span data-stu-id="57d9a-120">text messages</span></span>
- <span data-ttu-id="57d9a-121">oznámení mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="57d9a-121">mobile app notifications</span></span>
- <span data-ttu-id="57d9a-122">Kódy ověřování mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="57d9a-122">mobile app verification codes</span></span>
- <span data-ttu-id="57d9a-123">Tokeny OATH třetích stran</span><span class="sxs-lookup"><span data-stu-id="57d9a-123">third-party OATH tokens</span></span>

<span data-ttu-id="57d9a-124">Přehled o tom, jak funguje Azure Multi-Factor Authentication najdete hello následující video:</span><span class="sxs-lookup"><span data-stu-id="57d9a-124">For an overview of how Azure Multi-Factor Authentication works see hello following video:</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]

<span data-ttu-id="57d9a-125">Další informace najdete v tématu [MFA pro Office 365 a Azure MFA](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).</span><span class="sxs-lookup"><span data-stu-id="57d9a-125">For more information, see [MFA for Office 365 and MFA for Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).</span></span>

## <a name="time-bound-privileges"></a><span data-ttu-id="57d9a-126">Časově vázaných oprávnění</span><span class="sxs-lookup"><span data-stu-id="57d9a-126">Time-bound privileges</span></span>
<span data-ttu-id="57d9a-127">Některé organizace zjistit, zda mají příliš mnoho uživatelů ve vysoce privilegované role.</span><span class="sxs-lookup"><span data-stu-id="57d9a-127">Some organizations may find that they have too many users in highly privileged roles.</span></span> <span data-ttu-id="57d9a-128">Uživatel byla pravděpodobně přidána toohello role pro konkrétní aktivity, jako je toosign pro služby, ale často následně nepoužili těchto oprávnění.</span><span class="sxs-lookup"><span data-stu-id="57d9a-128">A user might have been added toohello role for a particular activity, like toosign up for a service, but didn't use those privileges frequently afterward.</span></span>

<span data-ttu-id="57d9a-129">Délka expozice hello toolower oprávnění a zvýšit vaši přehled o jejich používání tooonly limit uživatele s ohledem na jejich oprávnění "právě v čase" (JIT), když potřebují tooperform úlohu.</span><span class="sxs-lookup"><span data-stu-id="57d9a-129">toolower hello exposure time of privileges and increase your visibility into their use, limit users tooonly taking on their privileges "just in time" (JIT), when they need tooperform a task.</span></span> <span data-ttu-id="57d9a-130">Pro Azure Active Directory a služeb Microsoft Online Services, můžete použít [Azure AD Privileged Identity Management (PIM)](http://aka.ms/AzurePIM).</span><span class="sxs-lookup"><span data-stu-id="57d9a-130">For Azure Active Directory and Microsoft Online Services, you can use [Azure AD Privileged Identity Management (PIM)](http://aka.ms/AzurePIM).</span></span>

![Řídicí panel PIM][2]

## <a name="attack-detection"></a><span data-ttu-id="57d9a-132">Detekce útoku</span><span class="sxs-lookup"><span data-stu-id="57d9a-132">Attack detection</span></span>
<span data-ttu-id="57d9a-133">[Azure Active Directory Identity Protection](../active-directory-identityprotection.md) poskytuje ucelený přehled o rizikových událostech a potenciální ohrožení zabezpečení, které ovlivňují identity ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="57d9a-133">[Azure Active Directory Identity Protection](../active-directory-identityprotection.md) provides a consolidated view into risk events and potential vulnerabilities affecting your organization’s identities.</span></span> <span data-ttu-id="57d9a-134">Na základě riziko událostí, Identity Protection vypočítá úroveň rizika uživatele pro každého uživatele, umožňuje na základě riziko zásady tooconfigure tooautomatically chránit hello identity vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="57d9a-134">Based on risk events, Identity Protection calculates a user risk level for each user, enabling you tooconfigure risk-based policies tooautomatically protect hello identities of your organization.</span></span> <span data-ttu-id="57d9a-135">Tyto zásady, spolu s další řízení podmíněného přístupu poskytuje Azure Active Directory a EMS, můžete automaticky blokovat hello uživatele nebo rámci návrhů, které zahrnují resetování hesel a vynucení služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="57d9a-135">These policies, along with other conditional access controls provided by Azure Active Directory and EMS, can automatically block hello user or offer suggestions that include password resets and multi-factor authentication enforcement.</span></span>

![Azure AD Identity Protection][3]

## <a name="conditional-access"></a><span data-ttu-id="57d9a-137">Podmíněný přístup</span><span class="sxs-lookup"><span data-stu-id="57d9a-137">Conditional access</span></span>
<span data-ttu-id="57d9a-138">Pomocí podmíněného řízení přístupu Azure Active Directory kontroluje hello konkrétních podmínek, který si zvolíte při ověřování uživatele, před povolením přístupu tooan aplikace.</span><span class="sxs-lookup"><span data-stu-id="57d9a-138">With conditional access control, Azure Active Directory checks hello specific conditions you choose when authenticating a user, before allowing access tooan application.</span></span> <span data-ttu-id="57d9a-139">Po splnění těchto podmínek hello uživatel ověří a povolená přístup toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="57d9a-139">Once those conditions are met, hello user is authenticated and allowed access toohello application.</span></span>

![Nastavení pravidla podmíněného přístupu pomocí vícefaktorového ověřování][4]

## <a name="related-articles"></a><span data-ttu-id="57d9a-141">Související články</span><span class="sxs-lookup"><span data-stu-id="57d9a-141">Related articles</span></span>
* <span data-ttu-id="57d9a-142">Povolit [Azure Multi-Factor Authentication](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)</span><span class="sxs-lookup"><span data-stu-id="57d9a-142">Enable [Azure Multi-Factor Authentication](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)</span></span>
* <span data-ttu-id="57d9a-143">Povolit [Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-configure.md)</span><span class="sxs-lookup"><span data-stu-id="57d9a-143">Enable [Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-configure.md)</span></span>
* <span data-ttu-id="57d9a-144">Povolit [ochrany identit Azure AD](../active-directory-identityprotection.md)</span><span class="sxs-lookup"><span data-stu-id="57d9a-144">Enable [Azure AD Identity Protection](../active-directory-identityprotection.md)</span></span>
* <span data-ttu-id="57d9a-145">Povolit [řízení podmíněného přístupu](../active-directory-conditional-access.md)</span><span class="sxs-lookup"><span data-stu-id="57d9a-145">Enable [conditional access controls](../active-directory-conditional-access.md)</span></span>

<span data-ttu-id="57d9a-146">Další informace o vytváření plán dokončení zabezpečení, najdete v části hello "odpovědnosti zákazníka a plán" části hello [Microsoft Cloud Security Enterprise architekty](http://aka.ms/securecustomer) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="57d9a-146">For more information on building a complete security roadmap, see hello “Customer responsibilities and roadmap” section of hello [Microsoft Cloud Security for Enterprise Architects](http://aka.ms/securecustomer) document.</span></span> <span data-ttu-id="57d9a-147">Další informace o zapojení tooassist služby společnosti Microsoft s žádným z těchto témat, obraťte se na zástupce společnosti Microsoft nebo navštivte naše [počítačové bezpečnosti řešení stránky](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).</span><span class="sxs-lookup"><span data-stu-id="57d9a-147">For more information on engaging Microsoft services tooassist with any of these topics, contact your Microsoft representative or visit our [Cybersecurity solutions page](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).</span></span>

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
[4]: ../media/active-directory-conditional-access/conditionalaccess-saas-apps.png
