---
title: "Požadavky na Azure AD SSPR dat | Microsoft Docs"
description: "Obnovení dat požadavky pro hesla pomocí samoobslužné služby Azure AD a jak toosatisfy je"
services: active-directory
keywords: 
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
ms.openlocfilehash: b68a1d7914dcd0bb4509d0e94914dc4309f4463a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-password-reset-without-requiring-end-user-registration"></a>Nasazení bez nutnosti registrace koncového uživatele pro vytvoření nového hesla

Nasazení samoobslužného resetování hesla (SSPR) vyžaduje ověřování dat toobe přítomen. Některé organizace mají své uživatele, zadejte svoje data ověřování sami, ale celá řada organizací preferuje toosynchronize s existujícími daty ve službě Active Directory. Pokud jste správně formátovaných dat ve vašem místním adresáři a nakonfigurovat [Azure AD Connect pomocí expresního nastavení](./connect/active-directory-aadconnect-get-started-express.md), že data budou k dispozici tooAzure AD a SSPR se nevyžaduje zásah uživatele.

Žádné telefonní číslo musí být ve formátu hello + CountryCode PhoneNumber příklad: + 1 4255551234 toowork správně.

> [!NOTE]
> Resetování hesla nepodporuje telefonní klapky. I v hello 12345 formátu 4255551234 + 1 X předtím, než je uskutečněn hovor hello odebrané rozšíření.

## <a name="fields-populated"></a>Pole se vyplní

Pokud použijete výchozí nastavení hello v Azure AD Connect hello následující mapování jsou vytvářeny.

| Místní AD | Azure AD | Azure AD Authentication kontaktní údaje |
| --- | --- | --- |
| telephoneNumber | Telefon do kanceláře | Alternativní telefonní |
| mobilní | Mobilní telefon | Telefon |


## <a name="security-questions-and-answers"></a>Bezpečnostní otázky a odpovědi

Bezpečnostní otázky a odpovědi jsou bezpečně uloženy v klientovi služby Azure AD a jsou pouze přístupné toousers prostřednictvím hello [portálu pro registraci SSPR](https://aka.ms/ssprsetup). Správci nelze zobrazit ani upravit obsah hello jiného uživatele otázek a odpovědí.

### <a name="what-happens-when-a-user-registers"></a>Co se stane, když se uživatel zaregistruje

Když se uživatel zaregistruje, nastaví hello registrační stránku hello následující pole:

* Telefon pro ověření
* Ověření e-mailu
* Bezpečnostní otázky a odpovědi

Pokud jste zadali hodnotu **mobilní telefon** nebo **alternativní e-mailovou**, uživatelé mohou okamžitě používat tyto hodnoty tooreset jejich hesla, i v případě, že nebyly registrovány pro službu hello. Kromě toho uživatele při registraci pro hello poprvé, najdete v části tyto hodnoty a je upravit, pokud si přejí. Po jejich úspěšně zaregistrovat, budou tyto hodnoty zachovat v hello **telefon pro ověření** a **ověřování e-mailu** pole, v uvedeném pořadí.

## <a name="set-and-read-authentication-data-using-powershell"></a>Nastavit a číst data ověřování pomocí prostředí PowerShell

Následující pole Hello se dá nastavit pomocí prostředí PowerShell

* Alternativní e-mailu
* Mobilní telefon
* Telefon do kanceláře - lze nastavit pouze v případě není synchronizaci s místním adresářem

### <a name="using-powershell-v1"></a>Pomocí prostředí PowerShell V1

tooget spuštění, budete potřebovat příliš[stáhněte a nainstalujte modul Azure AD PowerShell hello](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule). Až budete mít nainstalováno, můžete provést hello kroky, které následují tooconfigure každé pole.

#### <a name="set-authentication-data-with-powershell-v1"></a>Nastavení ověření dat pomocí prostředí PowerShell V1

```
Connect-MsolService

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com") -MobilePhone "+1 1234567890" -PhoneNumber "+1 1234567890"
```

#### <a name="read-authentication-data-with-powershellpowershell-v1"></a>Čtení dat ověřování pomocí PowerShellPowerShell V1

```
Connect-MsolService

Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber

Get-MsolUser | select DisplayName,UserPrincipalName,AlternateEmailAddresses,MobilePhone,PhoneNumber | Format-Table
```

#### <a name="authentication-phone-and-authentication-email-can-only-be-read-using-powershell-v1-using-hello-commands-that-follow"></a>Telefon pro ověření a e-mailu ověřování lze číst pouze pomocí prostředí Powershell V1 hello pomocí následujících příkazů

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

### <a name="using-powershell-v2"></a>Pomocí prostředí PowerShell V2

tooget spuštění, budete potřebovat příliš[stáhněte a nainstalujte modul prostředí PowerShell hello Azure AD V2](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure%20AD%20Cmdlets/AzureAD/index.md). Až budete mít nainstalováno, můžete provést hello kroky, které následují tooconfigure každé pole.

tooinstall rychle z nejnovější verze prostředí PowerShell, které podporují instalaci modulu, spuštěním těchto příkazů (první řádek hello jednoduše kontroluje toosee Pokud je už nainstalovaná):

```
Get-Module AzureADPreview
Install-Module AzureADPreview
Connect-AzureAD
```

#### <a name="set-authentication-data-with-powershell-v2"></a>Nastavení ověření dat pomocí prostředí PowerShell V2

```
Connect-AzureAD

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("email@domain.com")
Set-AzureADUser -ObjectId user@domain.com -Mobile "+1 2345678901"
Set-AzureADUser -ObjectId user@domain.com -TelephoneNumber "+1 1234567890"

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("emails@domain.com") -Mobile "+1 1234567890" -TelephoneNumber "+1 1234567890"
```

### <a name="read-authentication-data-with-powershell-v2"></a>Čtení dat ověřování pomocí prostředí PowerShell V2

```
Connect-AzureAD

Get-AzureADUser -ObjectID user@domain.com | select otherMails
Get-AzureADUser -ObjectID user@domain.com | select Mobile
Get-AzureADUser -ObjectID user@domain.com | select TelephoneNumber

Get-AzureADUser | select DisplayName,UserPrincipalName,otherMails,Mobile,TelephoneNumber | Format-Table
```

## <a name="next-steps"></a>Další kroky

Hello následující odkazy obsahují další informace o resetování hesla pomocí služby Azure AD

* [**Rychlý Start**](active-directory-passwords-getting-started.md) – Zprovozněte samoobslužné resetování hesla Azure AD. 
* [**Správa licencí**](active-directory-passwords-licensing.md) – Konfigurujte licencování Azure AD.
* [**Zavedení** ](active-directory-passwords-best-practices.md) -plánování a nasazení SSPR tooyour uživatelů podle pokynů hello je zde uveden
* [**Přizpůsobení** ](active-directory-passwords-customize.md) -přizpůsobit hello vzhledu a chování hello SSPR prostředí pro vaši společnost.
* [**Zásady**](active-directory-passwords-policy.md) – Pochopte a nastavte zásady hesel Azure AD.
* [**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.
* [**Podrobné technické informace** ](active-directory-passwords-how-it-works.md) -přejděte za hello závěsem toounderstand, jak to funguje
* [**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak? Proč? Co? Kde? Kdo? Kdy? -Odpovědi tooquestions vždy chtěli tooask
* [**Řešení potíží s** ](active-directory-passwords-troubleshoot.md) – zjistěte, jak tooresolve běžné problémy, že vidíte s SSPR
