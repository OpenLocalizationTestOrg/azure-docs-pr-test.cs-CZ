---
title: "Azure AD Connect: Předávací ověřování – jak to funguje? | Dokumentace Microsoftu"
description: "Tento článek popisuje, jak funguje Azure Active Directory předávací ověřování."
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
ms.date: 07/27/2017
ms.author: billmath
ms.openlocfilehash: d34ccd40082edbe036d963ad548bff648119bdd4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a><span data-ttu-id="e727c-105">Azure předávací ověřování služby Active Directory: Technické podrobné informace</span><span class="sxs-lookup"><span data-stu-id="e727c-105">Azure Active Directory Pass-through Authentication: Technical deep dive</span></span>

>[!IMPORTANT]
><span data-ttu-id="e727c-106">Předávací ověřování Azure AD je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="e727c-106">Azure AD Pass-through Authentication is currently in preview.</span></span> 

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a><span data-ttu-id="e727c-107">Jak funguje Azure předávací ověřování služby Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e727c-107">How does Azure Active Directory Pass-through Authentication work?</span></span>

<span data-ttu-id="e727c-108">Když se uživatel pokusí přihlásit k aplikaci zabezpečené službou Azure Active Directory (Azure AD), a pokud je povolené předávací ověřování u klienta, jsou provedeny následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e727c-108">When a user attempts to sign into an application secured by Azure Active Directory (Azure AD), and if Pass-through Authentication is enabled on the tenant, the following steps occur:</span></span>

1. <span data-ttu-id="e727c-109">Uživatel se pokusí o přístup k aplikaci (například aplikaci Outlook Web App - https://outlook.office365.com/owa/).</span><span class="sxs-lookup"><span data-stu-id="e727c-109">The user tries to access an application (for example, the Outlook Web App - https://outlook.office365.com/owa/).</span></span>
2. <span data-ttu-id="e727c-110">Pokud již není přihlášený uživatel, se uživatel přesměruje na přihlašovací stránku služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e727c-110">If the user is not already signed in, the user is redirected to the Azure AD sign-in page.</span></span>
3. <span data-ttu-id="e727c-111">Uživatel zadá své uživatelské jméno a heslo na přihlašovací stránku služby Azure AD a klikne na tlačítko "Přihlásit".</span><span class="sxs-lookup"><span data-stu-id="e727c-111">The user enters their username and password into the Azure AD sign-in page and clicks the "Sign in" button.</span></span>
4. <span data-ttu-id="e727c-112">Azure AD, na přijme požadavek přihlášení umístí uživatelského jména a hesla (šifrované pomocí veřejného klíče) ve frontě.</span><span class="sxs-lookup"><span data-stu-id="e727c-112">Azure AD, on receiving the sign-in request, places the username and password (encrypted  using a public key) on a queue.</span></span>
5. <span data-ttu-id="e727c-113">Předávací ověřování agenta místní provede odchozí volání do fronty a načte uživatelské jméno a zašifrované heslo.</span><span class="sxs-lookup"><span data-stu-id="e727c-113">An on-premises Pass-through Authentication agent makes an outbound call to the queue and retrieves the username and encrypted password.</span></span>
6. <span data-ttu-id="e727c-114">Agent dešifruje heslo pomocí jeho privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="e727c-114">The agent decrypts the password using its private key.</span></span>
7. <span data-ttu-id="e727c-115">Agent pak ověří uživatelské jméno a heslo pro službu Active Directory pomocí standardních API systému Windows (podobně jako mechanismus pro co je používán Active Directory Federation Services).</span><span class="sxs-lookup"><span data-stu-id="e727c-115">The agent then validates the username and password against Active Directory using standard Windows APIs (a similar mechanism to what is used by Active Directory Federation Services).</span></span> <span data-ttu-id="e727c-116">Uživatelské jméno může být buď místní výchozí uživatelské jméno (obvykle `userPrincipalName`) nebo jiný atribut, které jsou nakonfigurované v Azure AD Connect (označované jako `Alternate ID`).</span><span class="sxs-lookup"><span data-stu-id="e727c-116">The username can be either the on-premises default username (usually `userPrincipalName`) or another attribute configured in Azure AD Connect (known as `Alternate ID`).</span></span>
8. <span data-ttu-id="e727c-117">V místní službě Active Directory řadiči domény (DC) pak vyhodnotí žádost a vrátí odpovídající odpověď (úspěch, chyba, platnost hesla nebo uzamčení uživatele) k agentovi.</span><span class="sxs-lookup"><span data-stu-id="e727c-117">The on-premises Active Directory Domain Controller (DC) then evaluates the request and returns the appropriate response (success, failure, password expired or user locked out) to the agent.</span></span>
9. <span data-ttu-id="e727c-118">Agent, pak vrátí tuto odpověď zpět do Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e727c-118">The agent, in turn, returns this response back to Azure AD.</span></span>
10. <span data-ttu-id="e727c-119">Azure AD vyhodnotí odpovědi a odpovídat na uživatele podle potřeby – například ji okamžitě přihlásí uživatel nebo požadavky pro multi-Factor Authentication (MFA).</span><span class="sxs-lookup"><span data-stu-id="e727c-119">Azure AD evaluates the response and responds to the user as appropriate - for example, it either signs the user in immediately or requests for Multi-Factor Authentication (MFA).</span></span>
11. <span data-ttu-id="e727c-120">Pokud přihlášení uživatele je úspěšné, uživatel je možné získat přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e727c-120">If the user sign-in is successful, the user is able to access the application.</span></span>

<span data-ttu-id="e727c-121">Následující diagram znázorňuje všechny součásti a kroky.</span><span class="sxs-lookup"><span data-stu-id="e727c-121">The following diagram illustrates all the components and the steps involved.</span></span>

![Předávací ověřování](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="next-steps"></a><span data-ttu-id="e727c-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e727c-123">Next steps</span></span>
- <span data-ttu-id="e727c-124">[**Aktuální omezení** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) – tato funkce je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="e727c-124">[**Current limitations**](active-directory-aadconnect-pass-through-authentication-current-limitations.md) - This feature is currently in preview.</span></span> <span data-ttu-id="e727c-125">Zjistěte, jaké postupy se podporují, a ty, které nejsou.</span><span class="sxs-lookup"><span data-stu-id="e727c-125">Learn which scenarios are supported and which ones are not.</span></span>
- <span data-ttu-id="e727c-126">[**Rychlý Start** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) – zprovoznění a systémem Azure AD předávací ověřování.</span><span class="sxs-lookup"><span data-stu-id="e727c-126">[**Quick Start**](active-directory-aadconnect-pass-through-authentication-quick-start.md) - Get up and running Azure AD Pass-through Authentication.</span></span>
- <span data-ttu-id="e727c-127">[**Nejčastější dotazy** ](active-directory-aadconnect-pass-through-authentication-faq.md) -odpovědi na nejčastější dotazy.</span><span class="sxs-lookup"><span data-stu-id="e727c-127">[**Frequently Asked Questions**](active-directory-aadconnect-pass-through-authentication-faq.md) - Answers to frequently asked questions.</span></span>
- <span data-ttu-id="e727c-128">[**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – zjistěte, jak řešit obvyklé problémy s funkcí.</span><span class="sxs-lookup"><span data-stu-id="e727c-128">[**Troubleshoot**](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) - Learn how to resolve common issues with the feature.</span></span>
- <span data-ttu-id="e727c-129">[**Azure AD bezproblémové SSO** ](active-directory-aadconnect-sso.md) -Další informace o této funkci vzájemně doplňují.</span><span class="sxs-lookup"><span data-stu-id="e727c-129">[**Azure AD Seamless SSO**](active-directory-aadconnect-sso.md) - Learn more about this complementary feature.</span></span>
- <span data-ttu-id="e727c-130">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.</span><span class="sxs-lookup"><span data-stu-id="e727c-130">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
