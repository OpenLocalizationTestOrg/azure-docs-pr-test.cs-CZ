---
title: "Azure AD Connect: Bezproblémové jednotné přihlašování – nejčastější dotazy | Microsoft Docs"
description: "Odpovědi toofrequently kladené otázky o Azure Active Directory bezproblémové jednotné přihlašování."
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
ms.openlocfilehash: e91e7950670633c08c180ece873f7451fa19eef8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-frequently-asked-questions"></a>Azure Active Directory bezproblémové jednotné přihlašování: Nejčastější dotazy

V tomto článku jsme adres často kladené otázky o Azure Active Directory bezproblémové jednotné přihlašování (SSO bezproblémové). Kontrolovat zpět pro nový obsah.

>[!IMPORTANT]
>funkce jednotného přihlašování bezproblémové Hello je aktuálně ve verzi preview.

## <a name="what-sign-in-methods-do-seamless-sso-work-with"></a>S jaké metody přihlašování fungují bezproblémové jednotné přihlašování?

Bezproblémové jednotného přihlašování je možné kombinovat s buď hello [synchronizaci hodnoty Hash hesla](active-directory-aadconnectsync-implement-password-synchronization.md) nebo [předávací ověřování](active-directory-aadconnect-pass-through-authentication.md) metody přihlašování. Tato funkce ale nelze použít s Active Directory Federation Services (ADFS).

## <a name="is-seamless-sso-a-free-feature"></a>Je bezproblémové jednotného přihlašování k bezplatné funkce?

Bezproblémové jednotného přihlašování je bezplatné funkce a nepotřebujete žádné placené edice Azure AD toouse ho. Zůstane volné, když funkce hello dosáhne obecné dostupnosti.

## <a name="what-applications-take-advantage-of-domainhint-or-loginhint-parameter-capability-of-seamless-sso"></a>Jaké aplikace využívat `domain_hint` nebo `login_hint` parametr funkce bezproblémové přihlašování?

Snažíme se v procesu hello kompilování hello seznam aplikací, které odesílají tyto parametry a hello ta, která není. Pokud máte aplikace, které zajímá, dejte nám vědět v sekci komentáře hello.

## <a name="does-seamless-sso-support-alternate-id-as-hello-username-instead-of-userprincipalname"></a>Podporuje bezproblémové SSO `Alternate ID` jako hello username, ne `userPrincipalName`?

Ano. Bezproblémové jednotného přihlašování podporuje `Alternate ID` jako hello uživatelské jméno při konfiguraci ve službě Azure AD Connect, jak je znázorněno [zde](active-directory-aadconnect-get-started-custom.md). Ne všechny aplikace Office 365 podporují `Alternate ID`. Pro příkaz hello podpory naleznete v dokumentaci toohello konkrétní aplikaci.

## <a name="i-want-tooregister-non-windows-10-devices-with-azure-ad-without-using-ad-fs-can-i-use-seamless-sso-instead"></a>Chci tooregister Windows 10 zařízení s Azure AD, bez použití služby AD FS. Můžete použít bezproblémové SSO místo?

Ano, tento scénář vyžaduje verze 2.1 nebo novější z hello [připojení k pracovišti klienta](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="how-can-i-roll-over-hello-kerberos-decryption-key-of-hello-azureadssoacct-computer-account"></a>Jak lze zahrnout přes hello protokolu Kerberos dešifrovací klíč hello `AZUREADSSOACCT` účet počítače?

Je důležité toofrequently mění hello protokolu Kerberos dešifrovací klíč hello `AZUREADSSOACCT` účet počítače, (což představuje Azure AD) vytvořené v místní doménové struktuře Active Directory.

>[!IMPORTANT]
>Důrazně doporučujeme, bude možné zahrnout přes hello protokolu Kerberos dešifrovací klíč minimálně každých 30 dnů.

Pomocí těchto kroků hello na místní server, kde je spuštěn nástroj Azure AD Connect:

### <a name="step-1-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>Krok 1. Získání seznamu doménových struktur služby AD, kde bylo povoleno bezproblémové jednotného přihlašování

1. Nejprve stáhnout a nainstalovat hello [Microsoft Online Services Sign-In Assistant](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Potom stáhněte a nainstalujte hello [64-bit modulu Azure Active Directory pro prostředí Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. Přejděte toohello `%programfiles%\Microsoft Azure Active Directory Connect` složky.
4. Import hello bezproblémové prostředí PowerShell jednotného přihlašování k modulu pomocí tohoto příkazu: `Import-Module .\AzureADSSO.psd1`.
5. Spusťte prostředí PowerShell jako správce. V prostředí PowerShell, zavolejte `New-AzureADSSOAuthenticationContext`. Tento příkaz měl dát tooenter místní přihlašovací údaje globálního správce vašeho klienta.
6. Volání `Get-AzureADSSOStatus`. Tento příkaz zjistí, zda že text hello seznam doménových struktur AD (podívejte se na seznam domén"hello") na které tato funkce povolená.

### <a name="step-2-update-hello-kerberos-decryption-key-on-each-ad-forest-that-it-was-set-it-up-on"></a>Krok 2. Aktualizovat hello protokolu Kerberos dešifrovací klíč v každé doménové struktuře AD, který ho se ho nastavit na

1. Volání `$creds = Get-Credential`. Po zobrazení výzvy zadejte přihlašovací údaje správce domény hello pro hello určené doménové struktuře Active Directory.
2. Volání `Update-AzureADSSOForest -OnPremCredentials $creds`. Tento příkaz aktualizace hello dešifrovací klíč protokolu Kerberos pro hello `AZUREADSSOACCT` účet počítače v této konkrétní doménové struktuře AD a aktualizace ve službě Azure AD.
3. Opakováním předchozích kroků pro každou doménovou strukturu AD, který jste nastavili hello funkce na hello.

>[!IMPORTANT]
>Ujistěte se, které jste _nemáte_ spustit hello `Update-AzureADSSOForest` příkaz více než jednou. V opačném hello funkce přestane fungovat až při hello lístky protokolu Kerberos vaši uživatelé vyprší a opakováno pomocí služby Active Directory v místě.

## <a name="how-can-i-disable-seamless-sso"></a>Jak můžete zakázat bezproblémové jednotné přihlašování?

Je možné zakázat bezproblémové jednotného přihlašování pomocí služby Azure AD Connect.

Spusťte Azure AD Connect, zvolte "Změna uživatele přihlašovací stránka" a klikněte na tlačítko "Další". Poté zrušte zaškrtnutí políčka možnost "Povolit jednotné přihlašování" hello. Pokračujte v Průvodci hello. Po dokončení Průvodce hello je bezproblémové jednotného přihlašování na váš klient zakázána.

Ale zobrazí se zpráva na obrazovce, který čte následujícím způsobem:

"Jednotné přihlašování je nyní zakázán, ale existují tooperform ručně provést další kroky v pořadí toocomplete čištění. Další informace"

toocomplete hello procesu, následujícím postupem ruční hello na místní server, kde je spuštěn nástroj Azure AD Connect:

### <a name="step-1-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>Krok 1. Získání seznamu doménových struktur služby AD, kde bylo povoleno bezproblémové jednotného přihlašování

1. Nejprve stáhnout a nainstalovat hello [Microsoft Online Services Sign-In Assistant](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Potom stáhněte a nainstalujte hello [64-bit modulu Azure Active Directory pro prostředí Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. Přejděte toohello `%programfiles%\Microsoft Azure Active Directory Connect` složky.
4. Import hello bezproblémové prostředí PowerShell jednotného přihlašování k modulu pomocí tohoto příkazu: `Import-Module .\AzureADSSO.psd1`.
5. Spusťte prostředí PowerShell jako správce. V prostředí PowerShell, zavolejte `New-AzureADSSOAuthenticationContext`. Tento příkaz měl dát tooenter místní přihlašovací údaje globálního správce vašeho klienta.
6. Volání `Get-AzureADSSOStatus`. Tento příkaz zjistí, zda že text hello seznam doménových struktur AD (podívejte se na seznam domén"hello") na které tato funkce povolená.

### <a name="step-2-manually-delete-hello-azureadssoacct-computer-account-from-each-ad-forest-that-you-see-listed"></a>Krok 2. Ručně odstraňte hello `AZUREADSSOACCT` účet počítače z každé doménové struktuře AD, které vidíte uvedené.

## <a name="next-steps"></a>Další kroky

- [**Rychlý Start** ](active-directory-aadconnect-sso-quick-start.md) – zprovoznění a systémem Azure bezproblémové jednotného přihlašování k AD.
- [**Podrobné technické informace** ](active-directory-aadconnect-sso-how-it-works.md) -pochopit, jak tato funkce funguje.
- [**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-sso.md) – zjistěte, jak tooresolve běžné problémy s funkcí hello.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.
