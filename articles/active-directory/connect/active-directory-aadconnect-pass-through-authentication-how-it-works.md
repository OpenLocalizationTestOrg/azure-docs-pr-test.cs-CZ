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
ms.openlocfilehash: ffcebee572a9ba2840e81250651dea45599d65d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a><span data-ttu-id="7b557-105">Azure předávací ověřování služby Active Directory: Technické podrobné informace</span><span class="sxs-lookup"><span data-stu-id="7b557-105">Azure Active Directory Pass-through Authentication: Technical deep dive</span></span>

>[!IMPORTANT]
><span data-ttu-id="7b557-106">Předávací ověřování Azure AD je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="7b557-106">Azure AD Pass-through Authentication is currently in preview.</span></span> 

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a><span data-ttu-id="7b557-107">Jak funguje Azure předávací ověřování služby Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7b557-107">How does Azure Active Directory Pass-through Authentication work?</span></span>

<span data-ttu-id="7b557-108">Když se uživatel pokusí toosign do k aplikaci zabezpečené službou Azure Active Directory (Azure AD) a pokud je povolené ověřování průchozí na klienta hello hello provedeny následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7b557-108">When a user attempts toosign into an application secured by Azure Active Directory (Azure AD), and if Pass-through Authentication is enabled on hello tenant, hello following steps occur:</span></span>

1. <span data-ttu-id="7b557-109">uživatel Hello pokusí tooaccess aplikace (například hello aplikaci Outlook Web App - https://outlook.office365.com/owa/).</span><span class="sxs-lookup"><span data-stu-id="7b557-109">hello user tries tooaccess an application (for example, hello Outlook Web App - https://outlook.office365.com/owa/).</span></span>
2. <span data-ttu-id="7b557-110">Pokud již není přihlášený hello uživatele, uživatel hello je přesměrovaného toohello Azure AD přihlašovací stránky.</span><span class="sxs-lookup"><span data-stu-id="7b557-110">If hello user is not already signed in, hello user is redirected toohello Azure AD sign-in page.</span></span>
3. <span data-ttu-id="7b557-111">Hello uživatel zadá své uživatelské jméno a heslo do Azure AD hello přihlašovací stránku a klikne na tlačítko "Přihlásit" hello.</span><span class="sxs-lookup"><span data-stu-id="7b557-111">hello user enters their username and password into hello Azure AD sign-in page and clicks hello "Sign in" button.</span></span>
4. <span data-ttu-id="7b557-112">Azure AD, na příjem hello požádat o přihlášení, umístí hello uživatelského jména a hesla (šifrované pomocí veřejného klíče) ve frontě.</span><span class="sxs-lookup"><span data-stu-id="7b557-112">Azure AD, on receiving hello sign-in request, places hello username and password (encrypted  using a public key) on a queue.</span></span>
5. <span data-ttu-id="7b557-113">Předávací ověřování agenta místní vytvoří frontu toohello odchozí volání a načte hello uživatelské jméno a heslo šifrované.</span><span class="sxs-lookup"><span data-stu-id="7b557-113">An on-premises Pass-through Authentication agent makes an outbound call toohello queue and retrieves hello username and encrypted password.</span></span>
6. <span data-ttu-id="7b557-114">Hello agent dešifruje hello heslo pomocí jeho privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="7b557-114">hello agent decrypts hello password using its private key.</span></span>
7. <span data-ttu-id="7b557-115">Hello agent pak ověří hello uživatelské jméno a heslo pro službu Active Directory pomocí standardních API systému Windows (podobně jako mechanismus toowhat používá Active Directory Federation Services).</span><span class="sxs-lookup"><span data-stu-id="7b557-115">hello agent then validates hello username and password against Active Directory using standard Windows APIs (a similar mechanism toowhat is used by Active Directory Federation Services).</span></span> <span data-ttu-id="7b557-116">Hello uživatelské jméno může být buď hello místní výchozí uživatelské jméno (obvykle `userPrincipalName`) nebo jiný atribut, které jsou nakonfigurované v Azure AD Connect (označované jako `Alternate ID`).</span><span class="sxs-lookup"><span data-stu-id="7b557-116">hello username can be either hello on-premises default username (usually `userPrincipalName`) or another attribute configured in Azure AD Connect (known as `Alternate ID`).</span></span>
8. <span data-ttu-id="7b557-117">Hello místní řadiči domény Active Directory (DC) pak vyhodnotí hello požadavku, a vrátí hello odpovídající odpověď (úspěch, chyba, platnost hesla nebo uzamčení uživatele) toohello agenta.</span><span class="sxs-lookup"><span data-stu-id="7b557-117">hello on-premises Active Directory Domain Controller (DC) then evaluates hello request and returns hello appropriate response (success, failure, password expired or user locked out) toohello agent.</span></span>
9. <span data-ttu-id="7b557-118">Hello agenta, pak vrátí zpět tooAzure AD této odpovědi.</span><span class="sxs-lookup"><span data-stu-id="7b557-118">hello agent, in turn, returns this response back tooAzure AD.</span></span>
10. <span data-ttu-id="7b557-119">Azure AD vyhodnotí hello odpovědi a odpoví toohello uživatele podle potřeby – například nebude moci okamžitě přihlásí uživatel hello nebo požadavky pro multi-Factor Authentication (MFA).</span><span class="sxs-lookup"><span data-stu-id="7b557-119">Azure AD evaluates hello response and responds toohello user as appropriate - for example, it either signs hello user in immediately or requests for Multi-Factor Authentication (MFA).</span></span>
11. <span data-ttu-id="7b557-120">Pokud přihlášení uživatele hello je úspěšné, uživatel hello je možné tooaccess hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="7b557-120">If hello user sign-in is successful, hello user is able tooaccess hello application.</span></span>

<span data-ttu-id="7b557-121">Hello následující diagram znázorňuje všechny komponenty hello a hello kroky.</span><span class="sxs-lookup"><span data-stu-id="7b557-121">hello following diagram illustrates all hello components and hello steps involved.</span></span>

![Předávací ověřování](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="next-steps"></a><span data-ttu-id="7b557-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7b557-123">Next steps</span></span>
- <span data-ttu-id="7b557-124">[**Aktuální omezení** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) – tato funkce je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="7b557-124">[**Current limitations**](active-directory-aadconnect-pass-through-authentication-current-limitations.md) - This feature is currently in preview.</span></span> <span data-ttu-id="7b557-125">Zjistěte, jaké postupy se podporují, a ty, které nejsou.</span><span class="sxs-lookup"><span data-stu-id="7b557-125">Learn which scenarios are supported and which ones are not.</span></span>
- <span data-ttu-id="7b557-126">[**Rychlý Start** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) – zprovoznění a systémem Azure AD předávací ověřování.</span><span class="sxs-lookup"><span data-stu-id="7b557-126">[**Quick Start**](active-directory-aadconnect-pass-through-authentication-quick-start.md) - Get up and running Azure AD Pass-through Authentication.</span></span>
- <span data-ttu-id="7b557-127">[**Nejčastější dotazy** ](active-directory-aadconnect-pass-through-authentication-faq.md) -odpovědi toofrequently dotazy.</span><span class="sxs-lookup"><span data-stu-id="7b557-127">[**Frequently Asked Questions**](active-directory-aadconnect-pass-through-authentication-faq.md) - Answers toofrequently asked questions.</span></span>
- <span data-ttu-id="7b557-128">[**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – zjistěte, jak tooresolve běžné problémy s funkcí hello.</span><span class="sxs-lookup"><span data-stu-id="7b557-128">[**Troubleshoot**](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) - Learn how tooresolve common issues with hello feature.</span></span>
- <span data-ttu-id="7b557-129">[**Azure AD bezproblémové SSO** ](active-directory-aadconnect-sso.md) -Další informace o této funkci vzájemně doplňují.</span><span class="sxs-lookup"><span data-stu-id="7b557-129">[**Azure AD Seamless SSO**](active-directory-aadconnect-sso.md) - Learn more about this complementary feature.</span></span>
- <span data-ttu-id="7b557-130">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.</span><span class="sxs-lookup"><span data-stu-id="7b557-130">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
