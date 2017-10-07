---
title: "Zásady: Azure AD SSPR | Microsoft Docs"
description: "Možnosti zásad resetování hesel samoobslužné služby Azure AD"
services: active-directory
keywords: "Správa hesel služby Active directory, správou hesel Azure AD samoobslužném resetování hesla služby"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: af7cb13794bf3a9fee91d355f788aa5c2246e57c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Zásady hesel a omezení v Azure Active Directory

Tento článek popisuje hello zásady pro hesla a požadavky na složitost spojené s uživatelskými účty, které jsou uložené v klientovi služby Azure AD.

## <a name="administrator-password-policy-differences"></a>Rozdíly zásady hesla správce

Microsoft vynucuje silné výchozí zásady resetování hesel se **dvěma branami** pro libovolnou roli správce Azure (příklad: globální správce, správce technické podpory, správce hesel atd.).

To zakáže správci pomocí bezpečnostních otázek a vynucuje následující hello.

Dvě zásady, které vyžadují dva kusy data ověřování brány (e-mailová adresa **a** telefonní číslo), platí v následujících případech hello

* Všechny role Správce služby Azure
  * Správce technické podpory
  * Správce služeb
  * Správce fakturace
  * Podpora Tier1 partnera
  * Podpora Tier2 partnera
  * Správce služby Exchange
  * Správce služby Lync
  * Správce účtu uživatele
  * Directory zapisovače
  * Globální správce správce nebo společnosti
  * Správce služby SharePoint
  * Správce dodržování předpisů
  * Správce aplikací
  * Správce zabezpečení.
  * Privilegované Role Správce
  * Správce služby Intune
  * Správce služby Proxy aplikace
  * Správce služeb CRM
  * Správce služby Power BI
  
* Po uplynutí 30 dnů ve zkušební verzi **nebo**
* Je k dispozici jednoduché domény (contoso.com) **nebo**
* Azure AD Connect je synchronizaci identit z vašeho místního adresáře

### <a name="exceptions"></a>Výjimky
Jedna brána zásadu, vyžadující jeden prvek ověření dat (e-mailová adresa **nebo** telefonní číslo), platí v následujících případech hello

* Prvních 30 dnů a zkušební **nebo**
* Jednoduché domény není k dispozici (*. onmicrosoft.com) **a** Azure AD Connect není synchronizaci identit


## <a name="userprincipalname-policies-that-apply-tooall-user-accounts"></a>UserPrincipalName zásady, které se vztahují tooall uživatelské účty

Každý uživatelský účet, který potřebuje toosign v tooAzure AD musí mít hodnotu atributu jedinečného uživatelského hlavní název (UPN) přidružené k účtu. Hello tabulce obrysy, které hello zásady, které se vztahují tooboth místní služby Active Directory uživatelské účty synchronizovány toohello cloudu a jen toocloud uživatelské účty.

| Vlastnost | UserPrincipalName požadavky |
| --- | --- |
| Povolené znaky |<ul> <li>A – Z</li> <li>a – z</li><li>0 – 9</li> <li> . - \_ ! \# ^ \~</li></ul> |
| Znaky nejsou povoleny |<ul> <li>Všechny ' @' znak, který není oddělení hello uživatelské jméno z domény hello.</li> <li>Nesmí obsahovat znak tečky '.' okamžitě předchozí hello ' @' – symbol</li></ul> |
| Omezení délky |<ul> <li>Celková délka nesmí přesáhnout délku 113 znaků</li><li>64 znaků před hello ' @' – symbol</li><li>48 znaků po hello ' @' – symbol</li></ul> |

## <a name="password-policies-that-apply-only-toocloud-user-accounts"></a>Zásady pro hesla, které se vztahují pouze toocloud uživatelské účty

Hello následující tabulka popisuje nastavení zásad hello k dispozici heslo, které můžou být použité toouser účty, které jsou vytvořeny a spravované ve službě Azure AD.

| Vlastnost | Požadavky |
| --- | --- |
| Povolené znaky |<ul><li>A – Z</li><li>a – z</li><li>0 – 9</li> <li>@ # $ % ^ & * - _ ! + = [ ] { } &#124; \ : ‘ , . ? / ` ~ “ ( ) ;</li></ul> |
| Znaky nejsou povoleny |<ul><li>Znaky kódování Unicode</li><li>Mezery</li><li> **Silná hesla jenom**: nesmí obsahovat tečku znak '.' okamžitě předchozí hello ' @' symbol</li></ul> |
| Omezení hesla |<ul><li>minimální 8 znaků a maximálně 16 znaků.</li><li>**Silná hesla jenom**: vyžaduje 3 mimo 4 hello následující:<ul><li>Malá písmena</li><li>Velká písmena</li><li>Čísla (0-9)</li><li>Symboly (viz výše uvedené omezení hesla)</li></ul></li></ul> |
| Doba vypršení platnosti hesla |<ul><li>Výchozí hodnota: **90** dnů </li><li>Hodnota je možné konfigurovat pomocí rutiny Set-MsolPasswordPolicy hello z hello Azure Active Directory modul pro prostředí Windows PowerShell.</li></ul> |
| Oznámení o vypršení hesla |<ul><li>Výchozí hodnota: **14** (před vypršením platnosti hesla)</li><li>Hodnota je možné konfigurovat pomocí rutiny Set-MsolPasswordPolicy hello.</li></ul> |
| Vypršení platnosti hesla |<ul><li>Výchozí hodnota: **false** dní (to znamená, že vypršení platnosti hesla je povolené) </li><li>Hodnota jde konfigurovat pro jednotlivé uživatelské účty pomocí rutiny Set-MsolUser hello. </li></ul> |
| Heslo **změnit** historie |Poslední heslo **nelze** znovu použít při **změna** heslo. |
| Heslo **resetovat** historie | Poslední heslo **může** znovu použít při **resetování** zapomenuté heslo. |
| Uzamčení účtu |Po 10 neúspěšných pokusech o přihlášení (chybné heslo) bude uživatel hello uzamčena na jednu minutu. Další nesprávný pokusy o přihlášení uzamčení hello uživatele pro zvýšení doby trvání. |

## <a name="set-password-expiration-policies-in-azure-active-directory"></a>Nastavení zásad vypršení platnosti hesla ve službě Azure Active Directory

Globální správce pro cloudové služby Microsoftu můžete použít Microsoft Azure Active Directory modul pro prostředí Windows PowerShell tooset hello až uživatelská hesla není tooexpire. Můžete také použít rutiny tooremove hello nikdy-vyprší platnost konfigurace nebo toosee které uživatelská hesla jsou nastavit není tooexpire prostředí Windows PowerShell. Tyto pokyny platí tooother poskytovatelů, jako například Microsoft Intune a Office 365, které také závisí na Microsoft Azure Active Directory pro identitu a adresářové služby.

> [!NOTE]
> Jenom hesla pro uživatelské účty, které nejsou synchronizovány pomocí synchronizace adresářů lze nakonfigurovat toonot vyprší. Další informace o synchronizaci adresářů v tématu[AD s Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).
>
>

## <a name="set-or-check-password-policies-using-powershell"></a>Nastavte nebo zkontrolujte zásady pro hesla pomocí prostředí PowerShell

tooget spuštění, budete potřebovat příliš[stáhněte a nainstalujte modul Azure AD PowerShell hello](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0). Až budete mít nainstalováno, můžete provést následující tooconfigure postup hello každé pole.

### <a name="how-toocheck-expiration-policy-for-a-password"></a>Jak toocheck zásad vypršení platnosti hesla
1. Připojte tooWindows prostředí PowerShell pomocí svých přihlašovacích údajů správce společnosti.
2. Spusťte jeden z hello následující příkazy:

   * toosee jenom jednoho konkrétního uživatele heslo se nastavuje, zda toonever vyprší, spusťte následující rutinu pomocí hello hlavní název uživatele (UPN) hello (například aprilr@contoso.onmicrosoft.com) nebo hello ID uživatele hello uživatele chcete toocheck:`Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`
   * toosee hello "Heslo je platné stále" nastavení pro všechny uživatele, spusťte následující rutinu hello:`Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

### <a name="set-a-password-tooexpire"></a>Nastavit heslo tooexpire

1. Připojte tooWindows prostředí PowerShell pomocí svých přihlašovacích údajů správce společnosti.
2. Spusťte jeden z hello následující příkazy:

   * heslo hello tooset jednoho uživatele tak, aby hello hesla vyprší, spusťte následující rutinu pomocí hello hlavní název uživatele (UPN) hello nebo hello ID uživatele hello:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`
   * tooset hello hesel všech uživatelů v organizaci hello tak, aby vypršení jejich platnosti, použijte následující rutinu hello:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

### <a name="set-a-password-toonever-expire"></a>Sada toonever heslo vyprší

1. Připojte tooWindows prostředí PowerShell pomocí svých přihlašovacích údajů správce společnosti.
2. Spusťte jeden z hello následující příkazy:

   * heslo hello tooset jeden uživatel toonever vypršení platnosti, spusťte následující rutinu pomocí hello hlavní název uživatele (UPN) hello nebo hello ID uživatele hello:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`
   * tooset hello hesel všech uživatelů hello v toonever organizaci vypršení platnosti, spusťte následující rutinu hello:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

## <a name="next-steps"></a>Další kroky

Hello následující odkazy obsahují další informace o resetování hesla pomocí služby Azure AD

* [**Rychlý Start**](active-directory-passwords-getting-started.md) – Zprovozněte samoobslužné resetování hesla Azure AD. 
* [**Správa licencí**](active-directory-passwords-licensing.md) – Konfigurujte licencování Azure AD.
* [**Data** ](active-directory-passwords-data.md) – pochopit hello data, která je požadována a jak se používají pro správu hesel
* [**Zavedení** ](active-directory-passwords-best-practices.md) -plánování a nasazení SSPR tooyour uživatelů podle pokynů hello je zde uveden
* [**Přizpůsobení** ](active-directory-passwords-customize.md) -přizpůsobit hello vzhledu a chování hello SSPR prostředí pro vaši společnost.
* [**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.
* [**Podrobné technické informace** ](active-directory-passwords-how-it-works.md) -přejděte za hello závěsem toounderstand, jak to funguje
* [**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak? Proč? Co? Kde? Kdo? Kdy? -Odpovědi tooquestions vždy chtěli tooask
* [**Řešení potíží s** ](active-directory-passwords-troubleshoot.md) – zjistěte, jak tooresolve běžné problémy, že vidíte s SSPR
