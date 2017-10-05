---
title: "Azure AD Connect: Předávací ověřování – aktuální omezení | Microsoft Docs"
description: "Tento článek popisuje aktuální omezení předávací ověřování Azure Active Directory (Azure AD)."
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
ms.openlocfilehash: 37c0ea094d02208f2516a4a040f75894e046c670
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a><span data-ttu-id="7ec02-104">Azure předávací ověřování služby Active Directory: Aktuální omezení</span><span class="sxs-lookup"><span data-stu-id="7ec02-104">Azure Active Directory Pass-through Authentication: Current limitations</span></span>

>[!IMPORTANT]
><span data-ttu-id="7ec02-105">Předávací ověřování Azure AD je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="7ec02-105">Azure AD Pass-through Authentication is currently in preview.</span></span> <span data-ttu-id="7ec02-106">Je bezplatné funkce a nepotřebujete žádné placené edice Azure AD pro použití.</span><span class="sxs-lookup"><span data-stu-id="7ec02-106">It is a free feature, and you don't need any paid editions of Azure AD to use it.</span></span> <span data-ttu-id="7ec02-107">Předávací ověřování je k dispozici v celém světě instanci Azure AD a ne v pouze [Microsoft Cloud Německo](http://www.microsoft.de/cloud-deutschland) a [cloudu Microsoft Azure Government](https://azure.microsoft.com/features/gov/).</span><span class="sxs-lookup"><span data-stu-id="7ec02-107">Pass-through Authentication is only available in the world-wide instance of Azure AD, and not on [Microsoft Cloud Germany](http://www.microsoft.de/cloud-deutschland) and [Microsoft Azure Government Cloud](https://azure.microsoft.com/features/gov/).</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="7ec02-108">Podporované scénáře</span><span class="sxs-lookup"><span data-stu-id="7ec02-108">Supported scenarios</span></span>

<span data-ttu-id="7ec02-109">Následující scénáře jsou plně podporované verzi Preview:</span><span class="sxs-lookup"><span data-stu-id="7ec02-109">The following scenarios are fully supported during preview:</span></span>

- <span data-ttu-id="7ec02-110">Přihlášení uživatele do všech webových aplikací založené na prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="7ec02-110">User sign-ins into all web browser-based applications.</span></span>
- <span data-ttu-id="7ec02-111">Uživatelská přihlášení do služeb Office 365 klientské aplikace, které podporují [moderní ověřování](https://aka.ms/modernauthga).</span><span class="sxs-lookup"><span data-stu-id="7ec02-111">User sign-ins into Office 365 client applications that support [modern authentication](https://aka.ms/modernauthga).</span></span>
- <span data-ttu-id="7ec02-112">Azure AD Join pro zařízení s Windows 10.</span><span class="sxs-lookup"><span data-stu-id="7ec02-112">Azure AD Join for Windows 10 devices.</span></span>
- <span data-ttu-id="7ec02-113">Podpora protokolu Exchange ActiveSync.</span><span class="sxs-lookup"><span data-stu-id="7ec02-113">Exchange ActiveSync support.</span></span>

## <a name="unsupported-scenarios"></a><span data-ttu-id="7ec02-114">Nepodporované scénáře</span><span class="sxs-lookup"><span data-stu-id="7ec02-114">Unsupported scenarios</span></span>

<span data-ttu-id="7ec02-115">Následující scénáře jsou _není_ během preview podporovány:</span><span class="sxs-lookup"><span data-stu-id="7ec02-115">The following scenarios are _not_ supported during preview:</span></span>

- <span data-ttu-id="7ec02-116">Přihlášení uživatele do klientské aplikace starší verze systému Office (Office 2013 nebo starší).</span><span class="sxs-lookup"><span data-stu-id="7ec02-116">User sign-ins into legacy Office client applications (Office 2013 or earlier).</span></span> <span data-ttu-id="7ec02-117">Organizace doporučujeme přepnout na moderní ověřování, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="7ec02-117">Organizations are encouraged to switch to modern authentication, if possible.</span></span> <span data-ttu-id="7ec02-118">Moderní ověřování umožňuje podporu předávací ověřování, ale také pomáhá se zabezpečením vašich uživatelských účtů pomocí [podmíněného přístupu](../active-directory-conditional-access.md) funkce jako je například služba Multi-Factor Authentication (MFA).</span><span class="sxs-lookup"><span data-stu-id="7ec02-118">Modern authentication allows for Pass-through Authentication support but also helps you secure your user accounts using [conditional access](../active-directory-conditional-access.md) features such as Multi-Factor Authentication (MFA).</span></span>
- <span data-ttu-id="7ec02-119">Uživatelská přihlášení do Skype pro firmy klientské aplikace, včetně Skype pro firmy 2016.</span><span class="sxs-lookup"><span data-stu-id="7ec02-119">User sign-ins into Skype for Business client applications, including Skype for Business 2016.</span></span>
- <span data-ttu-id="7ec02-120">Přihlášení uživatele do prostředí PowerShell 1.0.</span><span class="sxs-lookup"><span data-stu-id="7ec02-120">User sign-ins into PowerShell v1.0.</span></span> <span data-ttu-id="7ec02-121">Doporučuje se místo toho použít PowerShell v2.0.</span><span class="sxs-lookup"><span data-stu-id="7ec02-121">It is recommended that you use PowerShell v2.0 instead.</span></span>

>[!IMPORTANT]
><span data-ttu-id="7ec02-122">Jako řešení pro nepodporované scénáře, povolte synchronizaci hodnoty Hash hesla na [volitelné funkce](active-directory-aadconnect-get-started-custom.md#optional-features) stránku průvodce Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="7ec02-122">As a workaround for unsupported scenarios, enable Password Hash Synchronization on the [Optional features](active-directory-aadconnect-get-started-custom.md#optional-features) page in the Azure AD Connect wizard.</span></span> <span data-ttu-id="7ec02-123">Synchronizaci hodnoty Hash hesla funguje jako nouzové řešení pro scénáře předchozí _pouze_ (a _není_ jako obecný zálohu předávací ověřování).</span><span class="sxs-lookup"><span data-stu-id="7ec02-123">Password Hash Synchronization acts as a fallback for the preceding scenarios _only_ (and _not_ as a generic fallback to Pass-through Authentication).</span></span> <span data-ttu-id="7ec02-124">Pokud nepotřebujete tyto scénáře, vypněte synchronizaci hodnoty Hash hesla.</span><span class="sxs-lookup"><span data-stu-id="7ec02-124">If you don't need these scenarios, turn off Password Hash Synchronization.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7ec02-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7ec02-125">Next steps</span></span>
- <span data-ttu-id="7ec02-126">[**Rychlý Start** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) – zprovoznění a systémem Azure AD předávací ověřování.</span><span class="sxs-lookup"><span data-stu-id="7ec02-126">[**Quick Start**](active-directory-aadconnect-pass-through-authentication-quick-start.md) - Get up and running Azure AD Pass-through Authentication.</span></span>
- <span data-ttu-id="7ec02-127">[**Podrobné technické informace** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -pochopit, jak tato funkce funguje.</span><span class="sxs-lookup"><span data-stu-id="7ec02-127">[**Technical Deep Dive**](active-directory-aadconnect-pass-through-authentication-how-it-works.md) - Understand how this feature works.</span></span>
- <span data-ttu-id="7ec02-128">[**Nejčastější dotazy** ](active-directory-aadconnect-pass-through-authentication-faq.md) -odpovědi na nejčastější dotazy.</span><span class="sxs-lookup"><span data-stu-id="7ec02-128">[**Frequently Asked Questions**](active-directory-aadconnect-pass-through-authentication-faq.md) - Answers to frequently asked questions.</span></span>
- <span data-ttu-id="7ec02-129">[**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – zjistěte, jak řešit obvyklé problémy s funkcí.</span><span class="sxs-lookup"><span data-stu-id="7ec02-129">[**Troubleshoot**](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) - Learn how to resolve common issues with the feature.</span></span>
- <span data-ttu-id="7ec02-130">[**Azure AD bezproblémové SSO** ](active-directory-aadconnect-sso.md) -Další informace o této funkci vzájemně doplňují.</span><span class="sxs-lookup"><span data-stu-id="7ec02-130">[**Azure AD Seamless SSO**](active-directory-aadconnect-sso.md) - Learn more about this complementary feature.</span></span>
- <span data-ttu-id="7ec02-131">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.</span><span class="sxs-lookup"><span data-stu-id="7ec02-131">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
