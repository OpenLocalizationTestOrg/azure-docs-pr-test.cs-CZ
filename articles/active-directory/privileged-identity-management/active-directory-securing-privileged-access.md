---
title: "Zabezpečení privilegovaného přístupu ve službě Azure AD | Microsoft Docs"
description: "Téma, které popisuje přístupy k zabezpečení privilegovaného přístupu v Azure, Azure Active Directory a služeb Microsoft Online Services."
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
ms.openlocfilehash: acd1c11cecfa37f607fe5d82a76a40647093f43f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="securing-privileged-access-in-azure-ad"></a><span data-ttu-id="cbb3e-103">Zabezpečení privilegovaného přístupu ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="cbb3e-103">Securing privileged access in Azure AD</span></span>
<span data-ttu-id="cbb3e-104">Zabezpečení privilegovaného přístupu je důležitým prvním krokem k ochraně obchodní prostředky v moderní organizaci.</span><span class="sxs-lookup"><span data-stu-id="cbb3e-104">Securing privileged access is a critical first step to help protect business assets in a modern organization.</span></span> <span data-ttu-id="cbb3e-105">Privilegované účty jsou účty, které správu a spravovat systémy IT.</span><span class="sxs-lookup"><span data-stu-id="cbb3e-105">Privileged accounts are accounts that administer and manage IT systems.</span></span> <span data-ttu-id="cbb3e-106">Internetový útočník cíle tyto účty pro přístup k datům a systémy organizace.</span><span class="sxs-lookup"><span data-stu-id="cbb3e-106">Cyber-attackers target these accounts to gain access to an organization’s data and systems.</span></span> <span data-ttu-id="cbb3e-107">K zabezpečení privilegovaného přístupu, měli izolovat účty a systémy ze riziko napadení se zlými úmysly.</span><span class="sxs-lookup"><span data-stu-id="cbb3e-107">To secure privileged access, you should isolate the accounts and systems from the risk of being exposed to a malicious user.</span></span>

<span data-ttu-id="cbb3e-108">Další uživatelé spouštějí získat privilegovaný přístup prostřednictvím cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="cbb3e-108">More users are starting to get privileged access through cloud services.</span></span> <span data-ttu-id="cbb3e-109">To může zahrnovat globální správce Office 365, správci předplatného Azure a uživatelé, kteří mají přístup pro správu ve virtuálních počítačích nebo na aplikace SaaS.</span><span class="sxs-lookup"><span data-stu-id="cbb3e-109">This can include global administrators of Office365, Azure subscription administrators, and users who have administrative access in VMs or on SaaS apps.</span></span>

<span data-ttu-id="cbb3e-110">Společnost Microsoft doporučuje podle tento plán [zabezpečení privilegovaného přístupu](https://technet.microsoft.com/library/mt631194.aspx).</span><span class="sxs-lookup"><span data-stu-id="cbb3e-110">Microsoft recommends you follow this roadmap on [Securing Privileged Access](https://technet.microsoft.com/library/mt631194.aspx).</span></span>

<span data-ttu-id="cbb3e-111">Tyto zásady se použijí pro zákazníky pomocí Azure Active Directory, Office 365 nebo jiné služby společnosti Microsoft a aplikace, zda jsou uživatelské účty spravovat a ověřovat pomocí služby Active Directory nebo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="cbb3e-111">For customers using Azure Active Directory, Office 365, or other Microsoft services and applications, these principles apply whether user accounts are managed and authenticated by Active Directory or in Azure Active Directory.</span></span> <span data-ttu-id="cbb3e-112">Následující části obsahují další informace o funkcích služby Azure AD pro podporu zabezpečení privilegovaného přístupu.</span><span class="sxs-lookup"><span data-stu-id="cbb3e-112">The following sections provide more information on Azure AD features to support securing privileged access.</span></span>

## <a name="azure-multi-factor-authentication"></a><span data-ttu-id="cbb3e-113">Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="cbb3e-113">Azure Multi-Factor Authentication</span></span>
<span data-ttu-id="cbb3e-114">Pokud chcete zvýšit zabezpečení ověření správce, byste měli vyžadovat dvoustupňové ověření před udělením oprávnění.</span><span class="sxs-lookup"><span data-stu-id="cbb3e-114">To increase the security of administrator authentication, you should require two-step verification before granting privileges.</span></span> <span data-ttu-id="cbb3e-115">Dvoustupňové ověření je metoda ověřování, který jste si, že vyžaduje použití víc věcí než jenom uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="cbb3e-115">Two-step verification is a method of verifying who you are that requires the use of more than just a username and password.</span></span> <span data-ttu-id="cbb3e-116">Poskytuje druhou vrstvu zabezpečení uživatelská přihlášení a transakce.</span><span class="sxs-lookup"><span data-stu-id="cbb3e-116">It provides a second layer of security to user sign-ins and transactions.</span></span>

<span data-ttu-id="cbb3e-117">Azure Multi-Factor Authentication (MFA) se společnosti Microsoft dvoustupňové ověření řešení, které pomáhá chránit přístup k datům a aplikacím zároveň pokrývat uživatele potřebují pro jednoduchý proces přihlášení.</span><span class="sxs-lookup"><span data-stu-id="cbb3e-117">Azure Multi-Factor Authentication (MFA) is Microsoft's two-step verification solution, which helps safeguard access to data and applications while meeting user demand for a simple sign-in process.</span></span> <span data-ttu-id="cbb3e-118">Zajišťuje silné ověřování přes celou řadu možností snadno ověření, včetně:</span><span class="sxs-lookup"><span data-stu-id="cbb3e-118">It delivers strong authentication via a range of easy verification options including:</span></span>

- <span data-ttu-id="cbb3e-119">telefonní hovory</span><span class="sxs-lookup"><span data-stu-id="cbb3e-119">phone calls</span></span>
- <span data-ttu-id="cbb3e-120">Textové zprávy</span><span class="sxs-lookup"><span data-stu-id="cbb3e-120">text messages</span></span>
- <span data-ttu-id="cbb3e-121">oznámení mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="cbb3e-121">mobile app notifications</span></span>
- <span data-ttu-id="cbb3e-122">Kódy ověřování mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="cbb3e-122">mobile app verification codes</span></span>
- <span data-ttu-id="cbb3e-123">Tokeny OATH třetích stran</span><span class="sxs-lookup"><span data-stu-id="cbb3e-123">third-party OATH tokens</span></span>

<span data-ttu-id="cbb3e-124">Přehled o tom, jak funguje Azure Multi-Factor Authentication najdete v následujícím videu:</span><span class="sxs-lookup"><span data-stu-id="cbb3e-124">For an overview of how Azure Multi-Factor Authentication works see the following video:</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]

<span data-ttu-id="cbb3e-125">Další informace najdete v tématu [MFA pro Office 365 a Azure MFA](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).</span><span class="sxs-lookup"><span data-stu-id="cbb3e-125">For more information, see [MFA for Office 365 and MFA for Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).</span></span>

## <a name="time-bound-privileges"></a><span data-ttu-id="cbb3e-126">Časově vázaných oprávnění</span><span class="sxs-lookup"><span data-stu-id="cbb3e-126">Time-bound privileges</span></span>
<span data-ttu-id="cbb3e-127">Některé organizace zjistit, zda mají příliš mnoho uživatelů ve vysoce privilegované role.</span><span class="sxs-lookup"><span data-stu-id="cbb3e-127">Some organizations may find that they have too many users in highly privileged roles.</span></span> <span data-ttu-id="cbb3e-128">Uživatel byla pravděpodobně přidána do rolí pro konkrétní aktivitu, jako je zápis pro služby, ale často následně nepoužili těchto oprávnění.</span><span class="sxs-lookup"><span data-stu-id="cbb3e-128">A user might have been added to the role for a particular activity, like to sign up for a service, but didn't use those privileges frequently afterward.</span></span>

<span data-ttu-id="cbb3e-129">Pokud chcete snížit čas ohrožení oprávnění a zvýšit vaši přehled o jejich používání, omezení uživatelů na pouze s ohledem na jejich oprávnění "právě v čase" (JIT), když potřebují k provedení úkolu.</span><span class="sxs-lookup"><span data-stu-id="cbb3e-129">To lower the exposure time of privileges and increase your visibility into their use, limit users to only taking on their privileges "just in time" (JIT), when they need to perform a task.</span></span> <span data-ttu-id="cbb3e-130">Pro Azure Active Directory a služeb Microsoft Online Services, můžete použít [Azure AD Privileged Identity Management (PIM)](http://aka.ms/AzurePIM).</span><span class="sxs-lookup"><span data-stu-id="cbb3e-130">For Azure Active Directory and Microsoft Online Services, you can use [Azure AD Privileged Identity Management (PIM)](http://aka.ms/AzurePIM).</span></span>

![Řídicí panel PIM][2]

## <a name="attack-detection"></a><span data-ttu-id="cbb3e-132">Detekce útoku</span><span class="sxs-lookup"><span data-stu-id="cbb3e-132">Attack detection</span></span>
<span data-ttu-id="cbb3e-133">[Azure Active Directory Identity Protection](../active-directory-identityprotection.md) poskytuje ucelený přehled o rizikových událostech a potenciální ohrožení zabezpečení, které ovlivňují identity ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="cbb3e-133">[Azure Active Directory Identity Protection](../active-directory-identityprotection.md) provides a consolidated view into risk events and potential vulnerabilities affecting your organization’s identities.</span></span> <span data-ttu-id="cbb3e-134">Na základě riziko událostí, vypočítá ochrany identit úroveň rizika uživatele pro každého uživatele, což umožňuje konfigurovat zásady na základě riziko k automatické ochraně identity vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="cbb3e-134">Based on risk events, Identity Protection calculates a user risk level for each user, enabling you to configure risk-based policies to automatically protect the identities of your organization.</span></span> <span data-ttu-id="cbb3e-135">Tyto zásady, spolu s další řízení podmíněného přístupu poskytuje Azure Active Directory a EMS, můžete automaticky blokovat uživateli nebo rámci návrhů, které zahrnují resetování hesel a vynucení služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="cbb3e-135">These policies, along with other conditional access controls provided by Azure Active Directory and EMS, can automatically block the user or offer suggestions that include password resets and multi-factor authentication enforcement.</span></span>

![Azure AD Identity Protection][3]

## <a name="conditional-access"></a><span data-ttu-id="cbb3e-137">Podmíněný přístup</span><span class="sxs-lookup"><span data-stu-id="cbb3e-137">Conditional access</span></span>
<span data-ttu-id="cbb3e-138">Pomocí podmíněného řízení přístupu Azure Active Directory kontroluje konkrétních podmínek, které zvolíte při ověřování uživatele, před povolením přístupu k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cbb3e-138">With conditional access control, Azure Active Directory checks the specific conditions you choose when authenticating a user, before allowing access to an application.</span></span> <span data-ttu-id="cbb3e-139">Po splnění těchto podmínek je uživatel ověřený a přistupovat k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cbb3e-139">Once those conditions are met, the user is authenticated and allowed access to the application.</span></span>

![Nastavení pravidla podmíněného přístupu pomocí vícefaktorového ověřování][4]

## <a name="related-articles"></a><span data-ttu-id="cbb3e-141">Související články</span><span class="sxs-lookup"><span data-stu-id="cbb3e-141">Related articles</span></span>
* <span data-ttu-id="cbb3e-142">Povolit [Azure Multi-Factor Authentication](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)</span><span class="sxs-lookup"><span data-stu-id="cbb3e-142">Enable [Azure Multi-Factor Authentication](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)</span></span>
* <span data-ttu-id="cbb3e-143">Povolit [Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-configure.md)</span><span class="sxs-lookup"><span data-stu-id="cbb3e-143">Enable [Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-configure.md)</span></span>
* <span data-ttu-id="cbb3e-144">Povolit [ochrany identit Azure AD](../active-directory-identityprotection.md)</span><span class="sxs-lookup"><span data-stu-id="cbb3e-144">Enable [Azure AD Identity Protection](../active-directory-identityprotection.md)</span></span>
* <span data-ttu-id="cbb3e-145">Povolit [řízení podmíněného přístupu](../active-directory-conditional-access.md)</span><span class="sxs-lookup"><span data-stu-id="cbb3e-145">Enable [conditional access controls](../active-directory-conditional-access.md)</span></span>

<span data-ttu-id="cbb3e-146">Další informace o vytváření plán dokončení zabezpečení, najdete v části "odpovědnosti zákazníka a plán" z [Microsoft Cloud Security Enterprise architekty](http://aka.ms/securecustomer) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="cbb3e-146">For more information on building a complete security roadmap, see the “Customer responsibilities and roadmap” section of the [Microsoft Cloud Security for Enterprise Architects](http://aka.ms/securecustomer) document.</span></span> <span data-ttu-id="cbb3e-147">Další informace o zapojení služby společnosti Microsoft, které pomáhají s žádným z těchto témat, obraťte se na zástupce společnosti Microsoft nebo navštivte naše [počítačové bezpečnosti řešení stránky](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).</span><span class="sxs-lookup"><span data-stu-id="cbb3e-147">For more information on engaging Microsoft services to assist with any of these topics, contact your Microsoft representative or visit our [Cybersecurity solutions page](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).</span></span>

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
[4]: ../media/active-directory-conditional-access/conditionalaccess-saas-apps.png
