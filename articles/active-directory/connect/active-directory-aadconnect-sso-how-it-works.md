---
title: "Azure AD Connect: Bezproblémové Single Sign-On - jak to funguje | Microsoft Docs"
description: "Tento článek popisuje, jak funkce Azure Active Directory bezproblémové jednotné přihlašování funguje."
services: active-directory
keywords: "Co je Azure AD Connect, instalace služby Active Directory, požadované součásti pro Azure AD, jednotné přihlašování, jednotné přihlašování"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: billmath
ms.openlocfilehash: f0bcbdb03fbb70ff91ac3a56974a88eb1b26c245
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-technical-deep-dive"></a><span data-ttu-id="2f299-104">Azure Active Directory bezproblémové jednotné přihlašování: podrobné technické informace</span><span class="sxs-lookup"><span data-stu-id="2f299-104">Azure Active Directory Seamless Single Sign-On: Technical deep dive</span></span>

<span data-ttu-id="2f299-105">Tento článek poskytuje podrobné technické informace do Princip funkce Azure Active Directory bezproblémové jednotné přihlašování (SSO bezproblémové).</span><span class="sxs-lookup"><span data-stu-id="2f299-105">This article gives you technical details into how the Azure Active Directory Seamless Single Sign-On (Seamless SSO) feature works.</span></span>

>[!IMPORTANT]
><span data-ttu-id="2f299-106">Funkci bezproblémové jednotného přihlašování je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="2f299-106">The Seamless SSO feature is currently in preview.</span></span>

## <a name="how-does-seamless-sso-work"></a><span data-ttu-id="2f299-107">Jak funguje bezproblémové jednotné přihlašování?</span><span class="sxs-lookup"><span data-stu-id="2f299-107">How does Seamless SSO work?</span></span>

<span data-ttu-id="2f299-108">Tato část obsahuje dvě části:</span><span class="sxs-lookup"><span data-stu-id="2f299-108">This section has two parts to it:</span></span>
1. <span data-ttu-id="2f299-109">Instalace funkce bezproblémové jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2f299-109">The setup of the Seamless SSO feature.</span></span>
2. <span data-ttu-id="2f299-110">Jak funguje jednoho uživatele přihlásit transakce pomocí bezproblémové jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2f299-110">How a single user sign-in transaction works with Seamless SSO.</span></span>

### <a name="how-does-set-up-work"></a><span data-ttu-id="2f299-111">Jak nastavit pracovní?</span><span class="sxs-lookup"><span data-stu-id="2f299-111">How does set up work?</span></span>

<span data-ttu-id="2f299-112">Bezproblémové jednotného přihlašování je povoleno pomocí Azure AD Connect, jak je znázorněno [zde](active-directory-aadconnect-sso-quick-start.md).</span><span class="sxs-lookup"><span data-stu-id="2f299-112">Seamless SSO is enabled using Azure AD Connect as shown [here](active-directory-aadconnect-sso-quick-start.md).</span></span> <span data-ttu-id="2f299-113">Při povolení funkce, jsou provedeny následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2f299-113">While enabling the feature, the following steps occur:</span></span>
- <span data-ttu-id="2f299-114">Účet počítače s názvem `AZUREADSSOACCT` (která představuje Azure AD) je vytvořen v místní službě Active Directory (AD).</span><span class="sxs-lookup"><span data-stu-id="2f299-114">A computer account named `AZUREADSSOACCT` (which represents Azure AD) is created in your on-premises Active Directory (AD).</span></span>
- <span data-ttu-id="2f299-115">Účet počítače pomocí protokolu Kerberos dešifrovací klíč je bezpečně sdílet s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2f299-115">The computer account's Kerberos decryption key is shared securely with Azure AD.</span></span>
- <span data-ttu-id="2f299-116">Kromě toho jsou dvě Kerberos hlavní názvy služby (SPN) vytvoří představují dvou adres URL, které se používají při přihlášení k Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2f299-116">In addition, two Kerberos service principal names (SPNs) are created to represent two URLs that are used during Azure AD sign-in.</span></span>

>[!NOTE]
> <span data-ttu-id="2f299-117">V každé doménové struktuře AD synchronizovat do Azure AD (přes Azure AD Connect) a pro uživatele, jehož chcete bezproblémové jednotné přihlašování se vytvoří účet počítače a SPN protokolu Kerberos.</span><span class="sxs-lookup"><span data-stu-id="2f299-117">The computer account and the Kerberos SPNs are created in each AD forest you synchronize to Azure AD (using Azure AD Connect) and for whose users you want Seamless SSO.</span></span> <span data-ttu-id="2f299-118">Přesunout `AZUREADSSOACCT` účet počítače pro organizaci jednotce (OU) ukládat další účty počítačů zajistit, že je spravovat stejným způsobem a není odstraněn.</span><span class="sxs-lookup"><span data-stu-id="2f299-118">Move the `AZUREADSSOACCT` computer account to an Organization Unit (OU) where other computer accounts are stored to ensure that it is managed in the same way and is not deleted.</span></span>

>[!IMPORTANT]
><span data-ttu-id="2f299-119">Důrazně doporučujeme, aby vám [mění dešifrovací klíč protokolu Kerberos](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacct-computer-account) z `AZUREADSSOACCT` účet počítače minimálně každých 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="2f299-119">We highly recommend that you [roll over the Kerberos decryption key](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacct-computer-account) of the `AZUREADSSOACCT` computer account at least every 30 days.</span></span>

### <a name="how-does-sign-in-with-seamless-sso-work"></a><span data-ttu-id="2f299-120">Jak Přihlaste se pomocí pracovních bezproblémové jednotné přihlašování?</span><span class="sxs-lookup"><span data-stu-id="2f299-120">How does sign-in with Seamless SSO work?</span></span>

<span data-ttu-id="2f299-121">Po dokončení instalace bezproblémové jednotné přihlašování funguje stejně jako všechny ostatní přihlášení používající integrované ověřování systému Windows (IWA).</span><span class="sxs-lookup"><span data-stu-id="2f299-121">Once the set-up is complete, Seamless SSO works the same way as any other sign-in that uses Integrated Windows Authentication (IWA).</span></span> <span data-ttu-id="2f299-122">Postup je následující:</span><span class="sxs-lookup"><span data-stu-id="2f299-122">The flow is as follows:</span></span>

1. <span data-ttu-id="2f299-123">Uživatel se pokusí o přístup k aplikaci (například aplikaci Outlook Web App - https://outlook.office365.com/owa/) z připojených k doméně podnikové zařízení uvnitř firemní sítě.</span><span class="sxs-lookup"><span data-stu-id="2f299-123">The user tries to access an application (for example, the Outlook Web App - https://outlook.office365.com/owa/) from a domain-joined corporate device inside your corporate network.</span></span>
2. <span data-ttu-id="2f299-124">Pokud již není přihlášený uživatel, se uživatel přesměruje na přihlašovací stránku služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2f299-124">If the user is not already signed in, the user is redirected to the Azure AD sign-in page.</span></span>

  >[!NOTE]
  ><span data-ttu-id="2f299-125">Pokud Azure AD přihlášení požadavek obsahuje `domain_hint` (například identifikaci klienta -, contoso.onmicrosoft.com) nebo `login_hint` (Identifikace uživatel – třeba user@contoso.onmicrosoft.com nebo user@contoso.com) parametr a potom krok 2 bude přeskočena.</span><span class="sxs-lookup"><span data-stu-id="2f299-125">If the Azure AD sign-in request includes a `domain_hint` (identifying your tenant- for example, contoso.onmicrosoft.com) or `login_hint` (identifying the user - for example, user@contoso.onmicrosoft.com or user@contoso.com) parameter, then step 2 is skipped.</span></span>

3. <span data-ttu-id="2f299-126">Typy uživatelů ve své uživatelské jméno na přihlašovací stránku služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2f299-126">The user types in their user name into the Azure AD sign-in page.</span></span>
4. <span data-ttu-id="2f299-127">Azure AD pomocí jazyka JavaScript na pozadí, vyzve prohlížeče, prostřednictvím 401 neoprávněný odpověď, k poskytování lístek protokolu Kerberos.</span><span class="sxs-lookup"><span data-stu-id="2f299-127">Using JavaScript in the background, Azure AD challenges the browser, via a 401 Unauthorized response, to provide a Kerberos ticket.</span></span>
5. <span data-ttu-id="2f299-128">Prohlížeč, pak vyžádá lístek ze služby Active Directory pro `AZUREADSSOACCT` účet počítače (což představuje Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2f299-128">The browser, in turn, requests a ticket from Active Directory for the `AZUREADSSOACCT` computer account (which represents Azure AD).</span></span>
6. <span data-ttu-id="2f299-129">Služby Active Directory vyhledá účet počítače a vrátí lístek protokolu Kerberos v prohlížeči šifrován tajný klíč pro účet počítače.</span><span class="sxs-lookup"><span data-stu-id="2f299-129">Active Directory locates the computer account and returns a Kerberos ticket to the browser encrypted with the computer account's secret.</span></span>
7. <span data-ttu-id="2f299-130">V prohlížeči předává lístek protokolu Kerberos je získat ze služby Active Directory do Azure AD (v jednom z [Azure AD adresy URL, dříve přidány do nastavení zóny intranetu prohlížeče](active-directory-aadconnect-sso-quick-start.md#step-3-roll-out-the-feature)).</span><span class="sxs-lookup"><span data-stu-id="2f299-130">The browser forwards the Kerberos ticket it acquired from Active Directory to Azure AD (on one of the [Azure AD URLs previously added to the browser's Intranet zone settings](active-directory-aadconnect-sso-quick-start.md#step-3-roll-out-the-feature)).</span></span>
8. <span data-ttu-id="2f299-131">Azure AD dešifruje lístek protokolu Kerberos, která zahrnuje identitu uživatele přihlášeni firemních zařízení pomocí dříve sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="2f299-131">Azure AD decrypts the Kerberos ticket, which includes the identity of the user signed into the corporate device, using the previously shared key.</span></span>
9. <span data-ttu-id="2f299-132">Po vyhodnocení, Azure AD buď vrátí token zpět do aplikace nebo vyzývá uživatel k provedení dodatečné důkazy, jako je Vícefaktorové ověřování.</span><span class="sxs-lookup"><span data-stu-id="2f299-132">After evaluation, Azure AD either returns a token back to the application or asks the user to perform additional proofs, such as Multi-Factor Authentication.</span></span>
10. <span data-ttu-id="2f299-133">Pokud přihlášení uživatele je úspěšné, uživatel je možné získat přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2f299-133">If the user sign-in is successful, the user is able to access the application.</span></span>

<span data-ttu-id="2f299-134">Následující diagram znázorňuje všechny součásti a kroky.</span><span class="sxs-lookup"><span data-stu-id="2f299-134">The following diagram illustrates all the components and the steps involved.</span></span>

![Bezproblémové jednotného přihlašování](./media/active-directory-aadconnect-sso/sso2.png)

<span data-ttu-id="2f299-136">Bezproblémové jednotného přihlašování je oportunistické, což znamená, že pokud se nezdaří, přihlašování uživatelů spadne zpět na regulární chování – tj, uživatel musí zadat svoje heslo k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="2f299-136">Seamless SSO is opportunistic, which means if it fails, the sign-in experience falls back to its regular behavior - i.e, the user needs to enter their password to sign in.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2f299-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2f299-137">Next steps</span></span>

- <span data-ttu-id="2f299-138">[**Rychlý Start** ](active-directory-aadconnect-sso-quick-start.md) – zprovoznění a systémem Azure bezproblémové jednotného přihlašování k AD.</span><span class="sxs-lookup"><span data-stu-id="2f299-138">[**Quick Start**](active-directory-aadconnect-sso-quick-start.md) - Get up and running Azure AD Seamless SSO.</span></span>
- <span data-ttu-id="2f299-139">[**Nejčastější dotazy** ](active-directory-aadconnect-sso-faq.md) -odpovědi na nejčastější dotazy.</span><span class="sxs-lookup"><span data-stu-id="2f299-139">[**Frequently Asked Questions**](active-directory-aadconnect-sso-faq.md) - Answers to frequently asked questions.</span></span>
- <span data-ttu-id="2f299-140">[**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-sso.md) – zjistěte, jak řešit obvyklé problémy s funkcí.</span><span class="sxs-lookup"><span data-stu-id="2f299-140">[**Troubleshoot**](active-directory-aadconnect-troubleshoot-sso.md) - Learn how to resolve common issues with the feature.</span></span>
- <span data-ttu-id="2f299-141">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.</span><span class="sxs-lookup"><span data-stu-id="2f299-141">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
