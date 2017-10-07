---
title: "Azure AD Connect: Bezproblémové Single Sign-On - jak to funguje | Microsoft Docs"
description: "Tento článek popisuje, jak funguje hello Azure Active Directory bezproblémové jednotné přihlašování funkce."
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
ms.openlocfilehash: 17ce35b32832d241068ab878cf7aac42deab74ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-technical-deep-dive"></a><span data-ttu-id="1317e-104">Azure Active Directory bezproblémové jednotné přihlašování: podrobné technické informace</span><span class="sxs-lookup"><span data-stu-id="1317e-104">Azure Active Directory Seamless Single Sign-On: Technical deep dive</span></span>

<span data-ttu-id="1317e-105">Tento článek poskytuje podrobné technické informace do Princip hello Azure Active Directory bezproblémové jednotné přihlašování (SSO bezproblémové) funkce.</span><span class="sxs-lookup"><span data-stu-id="1317e-105">This article gives you technical details into how hello Azure Active Directory Seamless Single Sign-On (Seamless SSO) feature works.</span></span>

>[!IMPORTANT]
><span data-ttu-id="1317e-106">funkce jednotného přihlašování bezproblémové Hello je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="1317e-106">hello Seamless SSO feature is currently in preview.</span></span>

## <a name="how-does-seamless-sso-work"></a><span data-ttu-id="1317e-107">Jak funguje bezproblémové jednotné přihlašování?</span><span class="sxs-lookup"><span data-stu-id="1317e-107">How does Seamless SSO work?</span></span>

<span data-ttu-id="1317e-108">Tato část obsahuje dva tooit částí:</span><span class="sxs-lookup"><span data-stu-id="1317e-108">This section has two parts tooit:</span></span>
1. <span data-ttu-id="1317e-109">Instalační program Hello funkce hello bezproblémové jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1317e-109">hello setup of hello Seamless SSO feature.</span></span>
2. <span data-ttu-id="1317e-110">Jak funguje jednoho uživatele přihlásit transakce pomocí bezproblémové jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1317e-110">How a single user sign-in transaction works with Seamless SSO.</span></span>

### <a name="how-does-set-up-work"></a><span data-ttu-id="1317e-111">Jak nastavit pracovní?</span><span class="sxs-lookup"><span data-stu-id="1317e-111">How does set up work?</span></span>

<span data-ttu-id="1317e-112">Bezproblémové jednotného přihlašování je povoleno pomocí Azure AD Connect, jak je znázorněno [zde](active-directory-aadconnect-sso-quick-start.md).</span><span class="sxs-lookup"><span data-stu-id="1317e-112">Seamless SSO is enabled using Azure AD Connect as shown [here](active-directory-aadconnect-sso-quick-start.md).</span></span> <span data-ttu-id="1317e-113">Při povolení funkce hello, dojde k hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1317e-113">While enabling hello feature, hello following steps occur:</span></span>
- <span data-ttu-id="1317e-114">Účet počítače s názvem `AZUREADSSOACCT` (která představuje Azure AD) je vytvořen v místní službě Active Directory (AD).</span><span class="sxs-lookup"><span data-stu-id="1317e-114">A computer account named `AZUREADSSOACCT` (which represents Azure AD) is created in your on-premises Active Directory (AD).</span></span>
- <span data-ttu-id="1317e-115">účet počítače Hello protokolu Kerberos dešifrovací klíč je bezpečně sdílet s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1317e-115">hello computer account's Kerberos decryption key is shared securely with Azure AD.</span></span>
- <span data-ttu-id="1317e-116">Kromě dvou protokolu Kerberos hlavní názvy služby (SPN) vytvoří dvě adresy URL toorepresent, které se používají při přihlášení k Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1317e-116">In addition, two Kerberos service principal names (SPNs) are created toorepresent two URLs that are used during Azure AD sign-in.</span></span>

>[!NOTE]
> <span data-ttu-id="1317e-117">účet počítače Hello a SPN hello protokolu Kerberos se vytvoří v každé doménové struktuře AD synchronizovat tooAzure AD (přes Azure AD Connect) a pro uživatele, jehož chcete bezproblémové jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1317e-117">hello computer account and hello Kerberos SPNs are created in each AD forest you synchronize tooAzure AD (using Azure AD Connect) and for whose users you want Seamless SSO.</span></span> <span data-ttu-id="1317e-118">Přesunout hello `AZUREADSSOACCT` tooan účet počítače organizační jednotce (OU) kde jsou uložené tooensure, který je spravován v jiné účty počítače hello stejný způsobem a není odstraněn.</span><span class="sxs-lookup"><span data-stu-id="1317e-118">Move hello `AZUREADSSOACCT` computer account tooan Organization Unit (OU) where other computer accounts are stored tooensure that it is managed in hello same way and is not deleted.</span></span>

>[!IMPORTANT]
><span data-ttu-id="1317e-119">Důrazně doporučujeme, aby vám [mění hello protokolu Kerberos dešifrovací klíč](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacct-computer-account) z hello `AZUREADSSOACCT` účet počítače minimálně každých 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="1317e-119">We highly recommend that you [roll over hello Kerberos decryption key](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacct-computer-account) of hello `AZUREADSSOACCT` computer account at least every 30 days.</span></span>

### <a name="how-does-sign-in-with-seamless-sso-work"></a><span data-ttu-id="1317e-120">Jak Přihlaste se pomocí pracovních bezproblémové jednotné přihlašování?</span><span class="sxs-lookup"><span data-stu-id="1317e-120">How does sign-in with Seamless SSO work?</span></span>

<span data-ttu-id="1317e-121">Po dokončení nastavení hello bezproblémové jednotné přihlašování funguje hello stejně jako libovolný jiný způsob přihlášení, která používá integrované ověřování systému Windows (IWA).</span><span class="sxs-lookup"><span data-stu-id="1317e-121">Once hello set-up is complete, Seamless SSO works hello same way as any other sign-in that uses Integrated Windows Authentication (IWA).</span></span> <span data-ttu-id="1317e-122">tok Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="1317e-122">hello flow is as follows:</span></span>

1. <span data-ttu-id="1317e-123">uživatel Hello pokusí tooaccess aplikace (například hello aplikaci Outlook Web App - https://outlook.office365.com/owa/) z připojených k doméně podnikové zařízení uvnitř firemní sítě.</span><span class="sxs-lookup"><span data-stu-id="1317e-123">hello user tries tooaccess an application (for example, hello Outlook Web App - https://outlook.office365.com/owa/) from a domain-joined corporate device inside your corporate network.</span></span>
2. <span data-ttu-id="1317e-124">Pokud již není přihlášený hello uživatele, uživatel hello je přesměrovaného toohello Azure AD přihlašovací stránky.</span><span class="sxs-lookup"><span data-stu-id="1317e-124">If hello user is not already signed in, hello user is redirected toohello Azure AD sign-in page.</span></span>

  >[!NOTE]
  ><span data-ttu-id="1317e-125">Pokud hello Azure AD přihlášení žádost obsahuje `domain_hint` (například identifikaci klienta -, contoso.onmicrosoft.com) nebo `login_hint` (Identifikace hello uživatel – třeba user@contoso.onmicrosoft.com nebo user@contoso.com) parametr a potom krok 2 bude přeskočena.</span><span class="sxs-lookup"><span data-stu-id="1317e-125">If hello Azure AD sign-in request includes a `domain_hint` (identifying your tenant- for example, contoso.onmicrosoft.com) or `login_hint` (identifying hello user - for example, user@contoso.onmicrosoft.com or user@contoso.com) parameter, then step 2 is skipped.</span></span>

3. <span data-ttu-id="1317e-126">typy uživatelských Hello ve své uživatelské jméno do přihlašovací stránku hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1317e-126">hello user types in their user name into hello Azure AD sign-in page.</span></span>
4. <span data-ttu-id="1317e-127">Pomocí jazyka JavaScript v pozadí hello, Azure AD vyzve hello prohlížeče prostřednictvím 401 neoprávněný odpovědi tooprovide při lístek protokolu Kerberos.</span><span class="sxs-lookup"><span data-stu-id="1317e-127">Using JavaScript in hello background, Azure AD challenges hello browser, via a 401 Unauthorized response, tooprovide a Kerberos ticket.</span></span>
5. <span data-ttu-id="1317e-128">Hello prohlížeče, pak vyžádá lístek ze služby Active Directory pro hello `AZUREADSSOACCT` účet počítače (což představuje Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1317e-128">hello browser, in turn, requests a ticket from Active Directory for hello `AZUREADSSOACCT` computer account (which represents Azure AD).</span></span>
6. <span data-ttu-id="1317e-129">Služby Active Directory vyhledá účet počítače hello a vrátí prohlížeči toohello lístek protokolu Kerberos šifrován tajný klíč pro účet počítače hello.</span><span class="sxs-lookup"><span data-stu-id="1317e-129">Active Directory locates hello computer account and returns a Kerberos ticket toohello browser encrypted with hello computer account's secret.</span></span>
7. <span data-ttu-id="1317e-130">Prohlížeč Hello předává lístek protokolu Kerberos hello ho získat ze služby Active Directory tooAzure AD (v jednom z hello [adresy URL Azure AD dříve přidané nastavení zóny intranetu prohlížeče toohello](active-directory-aadconnect-sso-quick-start.md#step-3-roll-out-the-feature)).</span><span class="sxs-lookup"><span data-stu-id="1317e-130">hello browser forwards hello Kerberos ticket it acquired from Active Directory tooAzure AD (on one of hello [Azure AD URLs previously added toohello browser's Intranet zone settings](active-directory-aadconnect-sso-quick-start.md#step-3-roll-out-the-feature)).</span></span>
8. <span data-ttu-id="1317e-131">Azure AD dešifruje hello lístek protokolu Kerberos, která obsahuje hello identitu uživatele hello přihlásili hello podnikových zařízení, pomocí hello dříve sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="1317e-131">Azure AD decrypts hello Kerberos ticket, which includes hello identity of hello user signed into hello corporate device, using hello previously shared key.</span></span>
9. <span data-ttu-id="1317e-132">Po vyhodnocení Azure AD buď vrátí zpět toohello tokenu aplikace, nebo požádá uživatele tooperform hello dodatečné důkazy, jako je Vícefaktorové ověřování.</span><span class="sxs-lookup"><span data-stu-id="1317e-132">After evaluation, Azure AD either returns a token back toohello application or asks hello user tooperform additional proofs, such as Multi-Factor Authentication.</span></span>
10. <span data-ttu-id="1317e-133">Pokud přihlášení uživatele hello je úspěšné, uživatel hello je možné tooaccess hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="1317e-133">If hello user sign-in is successful, hello user is able tooaccess hello application.</span></span>

<span data-ttu-id="1317e-134">Hello následující diagram znázorňuje všechny komponenty hello a hello kroky.</span><span class="sxs-lookup"><span data-stu-id="1317e-134">hello following diagram illustrates all hello components and hello steps involved.</span></span>

![Bezproblémové jednotného přihlašování](./media/active-directory-aadconnect-sso/sso2.png)

<span data-ttu-id="1317e-136">Bezproblémové jednotného přihlašování je oportunistické, což znamená, že pokud se nezdaří, hello přihlašování spadne zpět regulární chování tooits – tj, hello uživatel potřebuje tooenter jejich toosign heslo v.</span><span class="sxs-lookup"><span data-stu-id="1317e-136">Seamless SSO is opportunistic, which means if it fails, hello sign-in experience falls back tooits regular behavior - i.e, hello user needs tooenter their password toosign in.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1317e-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1317e-137">Next steps</span></span>

- <span data-ttu-id="1317e-138">[**Rychlý Start** ](active-directory-aadconnect-sso-quick-start.md) – zprovoznění a systémem Azure bezproblémové jednotného přihlašování k AD.</span><span class="sxs-lookup"><span data-stu-id="1317e-138">[**Quick Start**](active-directory-aadconnect-sso-quick-start.md) - Get up and running Azure AD Seamless SSO.</span></span>
- <span data-ttu-id="1317e-139">[**Nejčastější dotazy** ](active-directory-aadconnect-sso-faq.md) -odpovědi toofrequently dotazy.</span><span class="sxs-lookup"><span data-stu-id="1317e-139">[**Frequently Asked Questions**](active-directory-aadconnect-sso-faq.md) - Answers toofrequently asked questions.</span></span>
- <span data-ttu-id="1317e-140">[**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-sso.md) – zjistěte, jak tooresolve běžné problémy s funkcí hello.</span><span class="sxs-lookup"><span data-stu-id="1317e-140">[**Troubleshoot**](active-directory-aadconnect-troubleshoot-sso.md) - Learn how tooresolve common issues with hello feature.</span></span>
- <span data-ttu-id="1317e-141">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.</span><span class="sxs-lookup"><span data-stu-id="1317e-141">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
