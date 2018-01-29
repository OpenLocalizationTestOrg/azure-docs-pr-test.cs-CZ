---
title: "Požadavky na Azure AD SSPR dat | Microsoft Docs"
description: "Požadavky na data pro hesla pomocí samoobslužné služby Azure AD pro resetování a postup jejich splnění"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 504316a7d09595d24318d1f574b0f8927914d424
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/25/2018
---
# <a name="deploy-password-reset-without-requiring-end-user-registration"></a>Nasazení bez nutnosti registrace koncového uživatele pro vytvoření nového hesla

K nasazení služby Azure Active Directory (Azure AD) samoobslužné resetování hesla (SSPR), data ověřování musí být k dispozici. Některé organizace mají své uživatele, zadejte svoje data ověřování sami. Ale mnoha organizacích je synchronizovat s daty, která již existuje ve službě Active Directory. Synchronizovaná data, která je k dispozici pro Azure AD a SSPR bez nutnosti zásahu uživatele, pokud je:
   * Správně formátu data v místním adresáři.
   * Konfigurace [Azure AD Connect pomocí expresního nastavení](./connect/active-directory-aadconnect-get-started-express.md).

Fungovalo správně, musí být telefonních čísel ve formátu *+ CountryCode PhoneNumber*, například 4255551234 + 1.

> [!NOTE]
> Je potřeba mezeru mezi kód země a telefonní číslo.
>
> Resetování hesla nepodporuje telefonní klapky. I ve formátu 12345 4255551234 + 1 X jsou rozšíření odebrat před uvedením volání.

## <a name="fields-populated"></a>Pole se vyplní

Pokud použijete výchozí nastavení v Azure AD Connect, budou vytvořeny následující mapování:

| Místní služby Active Directory | Azure AD | Azure AD authentication kontaktní údaje |
| --- | --- | --- |
| telephoneNumber | Telefon do kanceláře | Jiný telefon |
| mobilní | Mobilní telefon | Telefon |


## <a name="security-questions-and-answers"></a>Bezpečnostní otázky a odpovědi

Bezpečnostní otázky a odpovědi jsou bezpečně uloženy v klientovi služby Azure AD a jsou přístupné prostřednictvím jenom [portálu pro registraci SSPR](https://aka.ms/ssprsetup). Správci nelze zobrazit ani upravovat obsah otázky a odpovědi jiného uživatele.

### <a name="what-happens-when-a-user-registers"></a>Co se stane, když se uživatel zaregistruje

Když se uživatel zaregistruje, nastavuje registrační stránce následující pole:

* **Telefon pro ověření**
* **Ověření e-mailu**
* **Bezpečnostní otázky a odpovědi**

Pokud jste zadali hodnotu **mobilní telefon** nebo **alternativní e-mailu**, uživatelé mohou okamžitě pomocí tyto hodnoty resetovat vlastní hesla i v případě, že nebyly registrovány pro službu. Kromě toho uvidí tyto hodnoty se zaregistrujte poprvé, a můžete se je upravit, pokud chtějí. Po jejich registraci úspěšně, budou tyto hodnoty v jako trvalý **telefon pro ověření** a **ověřování e-mailu** pole, v uvedeném pořadí.

## <a name="set-and-read-the-authentication-data-through-powershell"></a>Nastavit a číst data ověřování pomocí prostředí PowerShell

Následující pole můžete nastavit pomocí prostředí PowerShell:

* **Alternativní e-mailu**
* **Mobilní telefon**
* **Telefon do kanceláře**: lze nastavit pouze v případě, že se nesynchronizují s místním adresářem

### <a name="use-powershell-version-1"></a>Pomocí prostředí PowerShell verze 1

Chcete-li začít pracovat, je potřeba [stáhněte a nainstalujte modul Azure AD PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule). Až budete mít nainstalováno, můžete použít následující postup ke konfiguraci každé pole.

#### <a name="set-the-authentication-data-with-powershell-version-1"></a>Nastavení ověřování dat v prostředí PowerShell verze 1

```
Connect-MsolService

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com") -MobilePhone "+1 1234567890" -PhoneNumber "+1 1234567890"
```

#### <a name="read-the-authentication-data-with-powershell-version-1"></a>Čtení dat ověřování pomocí prostředí PowerShell verze 1

```
Connect-MsolService

Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber

Get-MsolUser | select DisplayName,UserPrincipalName,AlternateEmailAddresses,MobilePhone,PhoneNumber | Format-Table
```

#### <a name="read-the-authentication-phone-and-authentication-email-options"></a>Čtení možnosti Telefon pro ověření a ověřování e-mailu

Ke čtení **telefon pro ověření** a **ověřování e-mailu** při použití prostředí PowerShell verze 1, použijte následující příkazy:

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

### <a name="use-powershell-version-2"></a>Pomocí prostředí PowerShell verze 2

Chcete-li začít pracovat, je potřeba [stáhněte a nainstalujte modul prostředí PowerShell verze 2 Azure AD](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure%20AD%20Cmdlets/AzureAD/index.md). Až budete mít nainstalováno, můžete použít následující postup ke konfiguraci každé pole.

Pokud chcete rychle nainstalovat z nejnovější verze prostředí PowerShell, které podporují instalaci modulu, spusťte následující příkazy. (První řádek zkontroluje, jestli je už nainstalovaný modul.)

```
Get-Module AzureADPreview
Install-Module AzureADPreview
Connect-AzureAD
```

#### <a name="set-the-authentication-data-with-powershell-version-2"></a>Nastavení ověřování dat v prostředí PowerShell verze 2

```
Connect-AzureAD

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("email@domain.com")
Set-AzureADUser -ObjectId user@domain.com -Mobile "+1 2345678901"
Set-AzureADUser -ObjectId user@domain.com -TelephoneNumber "+1 1234567890"

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("emails@domain.com") -Mobile "+1 1234567890" -TelephoneNumber "+1 1234567890"
```

#### <a name="read-the-authentication-data-with-powershell-version-2"></a>Čtení dat ověřování pomocí prostředí PowerShell, verze 2

```
Connect-AzureAD

Get-AzureADUser -ObjectID user@domain.com | select otherMails
Get-AzureADUser -ObjectID user@domain.com | select Mobile
Get-AzureADUser -ObjectID user@domain.com | select TelephoneNumber

Get-AzureADUser | select DisplayName,UserPrincipalName,otherMails,Mobile,TelephoneNumber | Format-Table
```

## <a name="next-steps"></a>Další postup

* [Jak dokončit úspěšné zavedení SSPR?](active-directory-passwords-best-practices.md)
* [Resetování nebo změna hesla](active-directory-passwords-update-your-own-password.md)
* [Registrace samoobslužného resetování hesla](active-directory-passwords-reset-register.md)
* [Máte dotaz k licencování?](active-directory-passwords-licensing.md)
* [Které metody ověřování jsou dostupné pro uživatele?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Jaké jsou možnosti zásad se SSPR?](active-directory-passwords-policy.md)
* [Co je zpětný zápis hesla a proč byste se o něj měli starat?](active-directory-passwords-writeback.md)
* [Jak hlásit aktivitu v SSPR?](active-directory-passwords-reporting.md)
* [Jaké jsou všechny možnosti v SSPR a co znamenají?](active-directory-passwords-how-it-works.md)
* [Myslím, že je něco poškozené. Jak řešit problémy SSPR?](active-directory-passwords-troubleshoot.md)
* [Mám otázku, která není zodpovězená jinde](active-directory-passwords-faq.md)
