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
# <a name="azure-active-directory-seamless-single-sign-on-technical-deep-dive"></a>Azure Active Directory bezproblémové jednotné přihlašování: podrobné technické informace

Tento článek poskytuje podrobné technické informace do Princip hello Azure Active Directory bezproblémové jednotné přihlašování (SSO bezproblémové) funkce.

>[!IMPORTANT]
>funkce jednotného přihlašování bezproblémové Hello je aktuálně ve verzi preview.

## <a name="how-does-seamless-sso-work"></a>Jak funguje bezproblémové jednotné přihlašování?

Tato část obsahuje dva tooit částí:
1. Instalační program Hello funkce hello bezproblémové jednotné přihlašování.
2. Jak funguje jednoho uživatele přihlásit transakce pomocí bezproblémové jednotného přihlašování.

### <a name="how-does-set-up-work"></a>Jak nastavit pracovní?

Bezproblémové jednotného přihlašování je povoleno pomocí Azure AD Connect, jak je znázorněno [zde](active-directory-aadconnect-sso-quick-start.md). Při povolení funkce hello, dojde k hello následující kroky:
- Účet počítače s názvem `AZUREADSSOACCT` (která představuje Azure AD) je vytvořen v místní službě Active Directory (AD).
- účet počítače Hello protokolu Kerberos dešifrovací klíč je bezpečně sdílet s Azure AD.
- Kromě dvou protokolu Kerberos hlavní názvy služby (SPN) vytvoří dvě adresy URL toorepresent, které se používají při přihlášení k Azure AD.

>[!NOTE]
> účet počítače Hello a SPN hello protokolu Kerberos se vytvoří v každé doménové struktuře AD synchronizovat tooAzure AD (přes Azure AD Connect) a pro uživatele, jehož chcete bezproblémové jednotné přihlašování. Přesunout hello `AZUREADSSOACCT` tooan účet počítače organizační jednotce (OU) kde jsou uložené tooensure, který je spravován v jiné účty počítače hello stejný způsobem a není odstraněn.

>[!IMPORTANT]
>Důrazně doporučujeme, aby vám [mění hello protokolu Kerberos dešifrovací klíč](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacct-computer-account) z hello `AZUREADSSOACCT` účet počítače minimálně každých 30 dnů.

### <a name="how-does-sign-in-with-seamless-sso-work"></a>Jak Přihlaste se pomocí pracovních bezproblémové jednotné přihlašování?

Po dokončení nastavení hello bezproblémové jednotné přihlašování funguje hello stejně jako libovolný jiný způsob přihlášení, která používá integrované ověřování systému Windows (IWA). tok Hello vypadá takto:

1. uživatel Hello pokusí tooaccess aplikace (například hello aplikaci Outlook Web App - https://outlook.office365.com/owa/) z připojených k doméně podnikové zařízení uvnitř firemní sítě.
2. Pokud již není přihlášený hello uživatele, uživatel hello je přesměrovaného toohello Azure AD přihlašovací stránky.

  >[!NOTE]
  >Pokud hello Azure AD přihlášení žádost obsahuje `domain_hint` (například identifikaci klienta -, contoso.onmicrosoft.com) nebo `login_hint` (Identifikace hello uživatel – třeba user@contoso.onmicrosoft.com nebo user@contoso.com) parametr a potom krok 2 bude přeskočena.

3. typy uživatelských Hello ve své uživatelské jméno do přihlašovací stránku hello Azure AD.
4. Pomocí jazyka JavaScript v pozadí hello, Azure AD vyzve hello prohlížeče prostřednictvím 401 neoprávněný odpovědi tooprovide při lístek protokolu Kerberos.
5. Hello prohlížeče, pak vyžádá lístek ze služby Active Directory pro hello `AZUREADSSOACCT` účet počítače (což představuje Azure AD).
6. Služby Active Directory vyhledá účet počítače hello a vrátí prohlížeči toohello lístek protokolu Kerberos šifrován tajný klíč pro účet počítače hello.
7. Prohlížeč Hello předává lístek protokolu Kerberos hello ho získat ze služby Active Directory tooAzure AD (v jednom z hello [adresy URL Azure AD dříve přidané nastavení zóny intranetu prohlížeče toohello](active-directory-aadconnect-sso-quick-start.md#step-3-roll-out-the-feature)).
8. Azure AD dešifruje hello lístek protokolu Kerberos, která obsahuje hello identitu uživatele hello přihlásili hello podnikových zařízení, pomocí hello dříve sdílený klíč.
9. Po vyhodnocení Azure AD buď vrátí zpět toohello tokenu aplikace, nebo požádá uživatele tooperform hello dodatečné důkazy, jako je Vícefaktorové ověřování.
10. Pokud přihlášení uživatele hello je úspěšné, uživatel hello je možné tooaccess hello aplikace.

Hello následující diagram znázorňuje všechny komponenty hello a hello kroky.

![Bezproblémové jednotného přihlašování](./media/active-directory-aadconnect-sso/sso2.png)

Bezproblémové jednotného přihlašování je oportunistické, což znamená, že pokud se nezdaří, hello přihlašování spadne zpět regulární chování tooits – tj, hello uživatel potřebuje tooenter jejich toosign heslo v.

## <a name="next-steps"></a>Další kroky

- [**Rychlý Start** ](active-directory-aadconnect-sso-quick-start.md) – zprovoznění a systémem Azure bezproblémové jednotného přihlašování k AD.
- [**Nejčastější dotazy** ](active-directory-aadconnect-sso-faq.md) -odpovědi toofrequently dotazy.
- [**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-sso.md) – zjistěte, jak tooresolve běžné problémy s funkcí hello.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.
