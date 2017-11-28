---
title: "Azure AD Connect: Předávací ověřování – aktuální omezení | Microsoft Docs"
description: "Tento článek popisuje aktuální omezení hello předávací ověřování Azure Active Directory (Azure AD)."
services: active-directory
keywords: "Azure AD Connect předávací ověřování, instalace služby Active Directory, požadované součásti pro Azure AD, jednotné přihlašování, jednotné přihlašování"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: billmath
ms.openlocfilehash: 2933745d071aae205c44659e6ea92697f390effb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a><span data-ttu-id="77eec-104">Azure předávací ověřování služby Active Directory: Aktuální omezení</span><span class="sxs-lookup"><span data-stu-id="77eec-104">Azure Active Directory Pass-through Authentication: Current limitations</span></span>

>[!IMPORTANT]
><span data-ttu-id="77eec-105">Předávací ověřování Azure AD je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="77eec-105">Azure AD Pass-through Authentication is currently in preview.</span></span> <span data-ttu-id="77eec-106">Je bezplatné funkce a nepotřebujete žádné placené edice Azure AD toouse ho.</span><span class="sxs-lookup"><span data-stu-id="77eec-106">It is a free feature, and you don't need any paid editions of Azure AD toouse it.</span></span> <span data-ttu-id="77eec-107">Předávací ověřování je dostupný jenom v hello celosvětové instance služby Azure AD a ne v [Microsoft Cloud Německo](http://www.microsoft.de/cloud-deutschland) a [cloudu Microsoft Azure Government](https://azure.microsoft.com/features/gov/).</span><span class="sxs-lookup"><span data-stu-id="77eec-107">Pass-through Authentication is only available in hello world-wide instance of Azure AD, and not on [Microsoft Cloud Germany](http://www.microsoft.de/cloud-deutschland) and [Microsoft Azure Government Cloud](https://azure.microsoft.com/features/gov/).</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="77eec-108">Podporované scénáře</span><span class="sxs-lookup"><span data-stu-id="77eec-108">Supported scenarios</span></span>

<span data-ttu-id="77eec-109">Hello následující scénáře jsou plně podporované verzi Preview:</span><span class="sxs-lookup"><span data-stu-id="77eec-109">hello following scenarios are fully supported during preview:</span></span>

- <span data-ttu-id="77eec-110">Přihlášení uživatele do všech webových aplikací založené na prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="77eec-110">User sign-ins into all web browser-based applications.</span></span>
- <span data-ttu-id="77eec-111">Uživatelská přihlášení do služeb Office 365 klientské aplikace, které podporují [moderní ověřování](https://aka.ms/modernauthga).</span><span class="sxs-lookup"><span data-stu-id="77eec-111">User sign-ins into Office 365 client applications that support [modern authentication](https://aka.ms/modernauthga).</span></span>
- <span data-ttu-id="77eec-112">Azure AD Join pro zařízení s Windows 10.</span><span class="sxs-lookup"><span data-stu-id="77eec-112">Azure AD Join for Windows 10 devices.</span></span>
- <span data-ttu-id="77eec-113">Podpora protokolu Exchange ActiveSync.</span><span class="sxs-lookup"><span data-stu-id="77eec-113">Exchange ActiveSync support.</span></span>

## <a name="unsupported-scenarios"></a><span data-ttu-id="77eec-114">Nepodporované scénáře</span><span class="sxs-lookup"><span data-stu-id="77eec-114">Unsupported scenarios</span></span>

<span data-ttu-id="77eec-115">Hello následující scénáře jsou _není_ během preview podporovány:</span><span class="sxs-lookup"><span data-stu-id="77eec-115">hello following scenarios are _not_ supported during preview:</span></span>

- <span data-ttu-id="77eec-116">Přihlášení uživatele do klientské aplikace starší verze systému Office (Office 2013 nebo starší).</span><span class="sxs-lookup"><span data-stu-id="77eec-116">User sign-ins into legacy Office client applications (Office 2013 or earlier).</span></span> <span data-ttu-id="77eec-117">Organizace jsou podporovali tooswitch toomodern ověřování, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="77eec-117">Organizations are encouraged tooswitch toomodern authentication, if possible.</span></span> <span data-ttu-id="77eec-118">Moderní ověřování umožňuje podporu předávací ověřování, ale také pomáhá se zabezpečením vašich uživatelských účtů pomocí [podmíněného přístupu](../active-directory-conditional-access.md) funkce jako je například služba Multi-Factor Authentication (MFA).</span><span class="sxs-lookup"><span data-stu-id="77eec-118">Modern authentication allows for Pass-through Authentication support but also helps you secure your user accounts using [conditional access](../active-directory-conditional-access.md) features such as Multi-Factor Authentication (MFA).</span></span>
- <span data-ttu-id="77eec-119">Uživatelská přihlášení do Skype pro firmy klientské aplikace, včetně Skype pro firmy 2016.</span><span class="sxs-lookup"><span data-stu-id="77eec-119">User sign-ins into Skype for Business client applications, including Skype for Business 2016.</span></span>
- <span data-ttu-id="77eec-120">Přihlášení uživatele do prostředí PowerShell 1.0.</span><span class="sxs-lookup"><span data-stu-id="77eec-120">User sign-ins into PowerShell v1.0.</span></span> <span data-ttu-id="77eec-121">Doporučuje se místo toho použít PowerShell v2.0.</span><span class="sxs-lookup"><span data-stu-id="77eec-121">It is recommended that you use PowerShell v2.0 instead.</span></span>

>[!IMPORTANT]
><span data-ttu-id="77eec-122">Jako řešení pro nepodporované scénáře, povolte synchronizaci hodnoty Hash hesla na hello [volitelné funkce](active-directory-aadconnect-get-started-custom.md#optional-features) stránky v Průvodci hello Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="77eec-122">As a workaround for unsupported scenarios, enable Password Hash Synchronization on hello [Optional features](active-directory-aadconnect-get-started-custom.md#optional-features) page in hello Azure AD Connect wizard.</span></span> <span data-ttu-id="77eec-123">Synchronizaci hodnoty Hash hesla funguje jako nouzové řešení pro hello předcházející scénáře _pouze_ (a _není_ jako obecný záložní tooPass prostřednictvím ověřování).</span><span class="sxs-lookup"><span data-stu-id="77eec-123">Password Hash Synchronization acts as a fallback for hello preceding scenarios _only_ (and _not_ as a generic fallback tooPass-through Authentication).</span></span> <span data-ttu-id="77eec-124">Pokud nepotřebujete tyto scénáře, vypněte synchronizaci hodnoty Hash hesla.</span><span class="sxs-lookup"><span data-stu-id="77eec-124">If you don't need these scenarios, turn off Password Hash Synchronization.</span></span>

## <a name="next-steps"></a><span data-ttu-id="77eec-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="77eec-125">Next steps</span></span>
- <span data-ttu-id="77eec-126">[**Rychlý Start** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) – zprovoznění a systémem Azure AD předávací ověřování.</span><span class="sxs-lookup"><span data-stu-id="77eec-126">[**Quick Start**](active-directory-aadconnect-pass-through-authentication-quick-start.md) - Get up and running Azure AD Pass-through Authentication.</span></span>
- <span data-ttu-id="77eec-127">[**Podrobné technické informace** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -pochopit, jak tato funkce funguje.</span><span class="sxs-lookup"><span data-stu-id="77eec-127">[**Technical Deep Dive**](active-directory-aadconnect-pass-through-authentication-how-it-works.md) - Understand how this feature works.</span></span>
- <span data-ttu-id="77eec-128">[**Nejčastější dotazy** ](active-directory-aadconnect-pass-through-authentication-faq.md) -odpovědi toofrequently dotazy.</span><span class="sxs-lookup"><span data-stu-id="77eec-128">[**Frequently Asked Questions**](active-directory-aadconnect-pass-through-authentication-faq.md) - Answers toofrequently asked questions.</span></span>
- <span data-ttu-id="77eec-129">[**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – zjistěte, jak tooresolve běžné problémy s funkcí hello.</span><span class="sxs-lookup"><span data-stu-id="77eec-129">[**Troubleshoot**](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) - Learn how tooresolve common issues with hello feature.</span></span>
- <span data-ttu-id="77eec-130">[**Azure AD bezproblémové SSO** ](active-directory-aadconnect-sso.md) -Další informace o této funkci vzájemně doplňují.</span><span class="sxs-lookup"><span data-stu-id="77eec-130">[**Azure AD Seamless SSO**](active-directory-aadconnect-sso.md) - Learn more about this complementary feature.</span></span>
- <span data-ttu-id="77eec-131">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.</span><span class="sxs-lookup"><span data-stu-id="77eec-131">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
