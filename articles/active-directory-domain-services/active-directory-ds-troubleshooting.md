---
title: "Azure Active Directory Domain Services: Průvodce odstraňováním potíží | Microsoft Docs"
description: "Průvodce řešením potíží pro Azure AD Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 4bc8c604-f57c-4f28-9dac-8b9164a0cf0b
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 86eb3513b7bc921c59287600b1b76eeda20c1356
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-domain-services---troubleshooting-guide"></a>Azure AD Domain Services – Průvodce odstraňováním potíží s
Tento článek obsahuje pokyny k odstranění potíží pro problémy, se kterými se můžete setkat při nastavení nebo jejich správě Azure Active Directory (AD) Domain Services.

## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>Nejde povolit Azure AD Domain Services pro svůj adresář Azure AD
Tato část pomůže řešení chyb při akci tooenable Azure AD Domain Services pro svůj adresář a selže nebo získá přepínat stav back too'Disabled'.

Vyberte hello řešení potíží s kroky, které odpovídají toohello chybová zpráva, na které narazíte.

| **Chybová zpráva** | **Řešení** |
| --- |:--- |
| *contoso100.com Hello název je již používán v této síti. Zadejte název, který se nepoužívá.* |[Konflikt názvů domény ve virtuální síti hello](active-directory-ds-troubleshooting.md#domain-name-conflict) |
| *Domain Services nelze povolit v klientovi Azure AD. Hello služba nemá dostatečná oprávnění toohello aplikaci s názvem 'Synchronizace Azure AD Domain Services'. Odstraňte hello aplikaci s názvem 'synchronizace Azure AD Domain Services a opakujte tooenable Domain Services pro vašeho tenanta Azure AD.* |[Domain Services nemá odpovídající oprávnění toohello synchronizace Azure AD Domain Services aplikace](active-directory-ds-troubleshooting.md#inadequate-permissions) |
| *Domain Services nelze povolit v klientovi Azure AD. Hello mít hello není aplikace v klientovi služby Azure AD Domain Services vyžaduje oprávnění tooenable Domain Services. Odstranit aplikaci hello s d87dcbc6-a371-462e-88e3-28ad15ec4e64 identifikátor aplikace hello a opakujte tooenable pro vašeho tenanta Azure AD Domain Services.* |[Hello aplikace Domain Services není správně nakonfigurována ve vašem klientovi](active-directory-ds-troubleshooting.md#invalid-configuration) |
| *Domain Services nelze povolit v klientovi Azure AD. Hello aplikace Microsoft Azure AD je zakázána v klientovi Azure AD. Povolit aplikaci hello s 00000002-0000-0000-c000-000000000000 identifikátor aplikace hello a opakujte tooenable pro vašeho tenanta Azure AD Domain Services.* |[v klientovi Azure AD je zakázáno Hello aplikace Microsoft Graph](active-directory-ds-troubleshooting.md#microsoft-graph-disabled) |

### <a name="domain-name-conflict"></a>Konflikt názvů domény
**Chybová zpráva:**

*contoso100.com Hello název je již používán v této síti. Zadejte název, který se nepoužívá.*

**Náprava:**

Ujistěte se, že nemáte existující domény s hello stejným názvem domény k dispozici ve virtuální síti. Například předpokládejme, že máte doméně s názvem "contoso.com" již k dispozici na vybranou virtuální síť hello. Později, zkuste tooenable spravované doméně služby Azure AD Domain Services s hello stejným názvem domény (tedy "contoso.com") ve virtuální síti. Dojde k chybě při pokusu o tooenable Azure AD Domain Services.

Toto selhání je z důvodu konfliktu tooname pro název domény hello ve virtuální síti. V takovém případě musíte použít jiný název tooset zálohu vaší spravované domény služby Azure AD Domain Services. Alternativně můžete zrušte zřizovat hello existující domény a poté pokračujte tooenable Azure AD Domain Services.

### <a name="inadequate-permissions"></a>Nedostatečná oprávnění
**Chybová zpráva:**

*Domain Services nelze povolit v klientovi Azure AD. Hello služba nemá dostatečná oprávnění toohello aplikaci s názvem 'Synchronizace Azure AD Domain Services'. Odstraňte hello aplikaci s názvem 'synchronizace Azure AD Domain Services a opakujte tooenable Domain Services pro vašeho tenanta Azure AD.*

**Náprava:**

Zkontrolujte toosee, pokud je aplikace s názvem hello 'synchronizace Azure AD Domain Services, v adresáři služby Azure AD. Pokud tuto aplikaci existuje, odstraňte ji a pak opětovného povolení služby Azure AD Domain Services.

Provedení hello následující kroky toocheck hello přítomnost aplikace hello a toodelete, pokud existuje hello aplikace:

1. Přejděte toohello **portál Azure classic** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
2. Vyberte hello **služby Active Directory** uzlu v levém podokně hello.
3. Klient vyberte hello Azure AD (adresář), pro kterou chcete tooenable Azure AD Domain Services.
4. Přejděte toohello **aplikace** kartě.
5. Vyberte hello **aplikace Moje společnost vlastní** možnost v rozevíracím seznamu hello.
6. Zkontrolujte aplikaci s názvem **synchronizace Azure AD Domain Services**. Pokud aplikace hello existuje, pokračujte toodelete ho.
7. Jakmile jste odstranili hello aplikace, zkuste znovu tooenable Azure AD Domain Services.

### <a name="invalid-configuration"></a>Neplatná konfigurace
**Chybová zpráva:**

*Domain Services nelze povolit v klientovi Azure AD. Hello mít hello není aplikace v klientovi služby Azure AD Domain Services vyžaduje oprávnění tooenable Domain Services. Odstranit aplikaci hello s d87dcbc6-a371-462e-88e3-28ad15ec4e64 identifikátor aplikace hello a opakujte tooenable pro vašeho tenanta Azure AD Domain Services.*

**Náprava:**

Zkontrolujte toosee, pokud máte aplikace s názvem hello 'AzureActiveDirectoryDomainControllerServices' (s identifikátorem aplikace d87dcbc6-a371-462e-88e3-28ad15ec4e64) v adresáři služby Azure AD. Pokud tuto aplikaci existuje, je třeba toodelete ho a pak znovu povolte Azure AD Domain Services.

Použijte hello následující aplikace hello toofind skriptu prostředí PowerShell a odstraňte ji.

> [!NOTE]
> Tento skript používá **Azure AD PowerShell verze 2** rutiny. Úplný seznam všech dostupných rutin a toodownload hello modulu, najdete v tématu hello [AzureAD PowerShell referenční dokumentaci k nástroji](https://msdn.microsoft.com/library/azure/mt757189.aspx).
>
>

```
$InformationPreference = "Continue"
$WarningPreference = "Continue"

$aadDsSp = Get-AzureADServicePrincipal -Filter "AppId eq 'd87dcbc6-a371-462e-88e3-28ad15ec4e64'" -ErrorAction Ignore
if ($aadDsSp -ne $null)
{
    Write-Information "Found Azure AD Domain Services application. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $aadDsSp.ObjectId
    Write-Information "Deleted hello Azure AD Domain Services application."
}

$identifierUri = "https://sync.aaddc.activedirectory.windowsazure.com"
$appFilter = "IdentifierUris eq '" + $identifierUri + "'"
$app = Get-AzureADApplication -Filter $appFilter
if ($app -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync application. Deleting it ..."
    Remove-AzureADApplication -ObjectId $app.ObjectId
    Write-Information "Deleted hello Azure AD Domain Services Sync application."
}

$spFilter = "ServicePrincipalNames eq '" + $identifierUri + "'"
$sp = Get-AzureADServicePrincipal -Filter $spFilter
if ($sp -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync service principal. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $sp.ObjectId
    Write-Information "Deleted hello Azure AD Domain Services Sync service principal."
}
```
<br>

### <a name="microsoft-graph-disabled"></a>Microsoft Graph zakázáno
**Chybová zpráva:**

Domain Services nelze povolit v klientovi Azure AD. Hello aplikace Microsoft Azure AD je zakázána v klientovi Azure AD. Povolit aplikaci hello s 00000002-0000-0000-c000-000000000000 identifikátor aplikace hello a opakujte tooenable pro vašeho tenanta Azure AD Domain Services.

**Náprava:**

Zkontrolujte toosee, pokud je zakázána aplikaci s identifikátorem hello 00000002-0000-0000-c000-000000000000. Tato aplikace je aplikace Microsoft Azure AD hello a zajišťuje tooyour Azure AD Graph API přístup. Azure AD Domain Services musí tento toosynchronize toobe povolené aplikace spravované vaší tooyour klienta Azure AD domény.

tooresolve tuto chybu, povolte tuto aplikaci a opakujte tooenable Domain Services pro vašeho tenanta Azure AD.

## <a name="users-are-unable-toosign-in-toohello-azure-ad-domain-services-managed-domain"></a>Uživatelé jsou nelze toosign v toohello spravované doméně služby Azure AD Domain Services
Pokud jeden nebo více uživatelů v klientovi služby Azure AD nejde toosign ve spravované doméně toohello nově vytvořený, proveďte následující kroky řešení potíží hello:

* **Přihlášení pomocí formátu UPN:** zkuste toosign ve formátu UPN hello (například "joeuser@contoso.com') namísto hello SAMAccountName formátu (CONTOSO\joeuser). Hello SAMAccountName lze automaticky generovat pro uživatele, jehož předpona názvu UPN je příliš dlouhý nebo hello stejné jako jiný uživatel na hello spravované domény. formát UPN Hello záruku, že se toobe jedinečné v rámci klienta Azure AD.

> [!NOTE]
> Doporučujeme používat hello UPN formátu toosign ve spravované doméně toohello Azure AD Domain Services.
>
>

* Ujistěte se, že máte [povolena synchronizace hesel](active-directory-ds-getting-started-password-sync.md) v souladu s hello kroků uvedených v příručce Začínáme hello.
* **Externí účty:** Ujistěte se, že hello vliv na uživatelský účet není externího účtu v klientovi hello Azure AD. Příklady externí účty jsou účty Microsoft (například "joe@live.com') nebo uživatelské účty z externího adresář Azure AD. Vzhledem k tomu, že Azure AD Domain Services nemá oprávnění pro tyto uživatelské účty, nemůže tyto uživatele přihlásit toohello spravované domény.
* **Synchronizovat účty:** Pokud hello vliv na uživatelské účty jsou synchronizované z místního adresáře, ověřte, že:

  * Nasazení nebo aktualizovat toohello [doporučujeme nejnovější verzi služby Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594).
  * Jste nakonfigurovali Azure AD Connect příliš[provést úplnou synchronizaci](active-directory-ds-getting-started-password-sync.md).
  * V závislosti na velikosti hello adresáře může chvíli trvat, než pro uživatelské účty a přihlašovací údaje toobe hodnoty hash je k dispozici v Azure AD Domain Services. Zajistěte, že abyste počkali dost dlouho, než se budete pokoušet ověřování (v závislosti na velikosti adresáře - den tooa několik hodin nebo dvě pro velké adresáře hello).
  * Pokud hello potíže potrvají po ověření hello předchozích kroků, zkuste restartovat hello služby Microsoft Azure AD Sync. Z počítače synchronizace spusťte příkazový řádek a spusťte následující příkazy hello:

    1. net stop "Microsoft Azure AD Sync.
    2. net start "Microsoft Azure AD Sync.
* **Jen cloudové účty**: Pokud hello vliv na uživatelský účet je účet uživatele jenom pro cloud, ujistěte se, tento uživatel hello změnil své heslo po povolení služby Azure AD Domain Services. Tento krok způsobí, že hello přihlašovací údaje potřebné pro Azure AD Domain Services toobe generované hodnoty hash.

## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>Uživatelé odebrána z vašeho klienta Azure AD nejsou odebrány z vaší spravované domény
Azure AD vás chrání před náhodným odstraněním objektů uživatelů. Pokud odstraníte účet uživatele z vašeho klienta Azure AD, odpovídající objekt uživatele hello je přesunutý toohello Koš. Spravované domény synchronizované tooyour po této operace odstranění způsobí hello odpovídající toobe účet uživatele označena zakázáno. Tato funkce vám pomůže obnovit nebo zrušení odstranění uživatelského účtu hello později.

Dobrý den uživatelský účet pořád hello zakázáno stavu ve vaší spravované domény, i když je znovu vytvořit uživatelský účet s hello stejný hlavní název uživatele v adresáři služby Azure AD. tooremove hello uživatelského účtu z vaší spravované domény, je nutné tooforce odstranit z vašeho klienta Azure AD.

uživatel hello tooremove hello uživatelského účtu plně z vaší spravované domény, trvale odstranit z vašeho klienta Azure AD. Použijte rutinu Remove-MsolUser Powershellu hello s hello '-RemoveFromRecycleBin' možnost, jak je popsáno v tomto [článku na webu MSDN](https://msdn.microsoft.com/library/azure/dn194132.aspx).

## <a name="contact-us"></a>Kontaktujte nás
Obraťte se na tým produktu Azure Active Directory Domain Services hello příliš[sdílet zpětnou vazbu nebo pro podporu](active-directory-ds-contact-us.md).
