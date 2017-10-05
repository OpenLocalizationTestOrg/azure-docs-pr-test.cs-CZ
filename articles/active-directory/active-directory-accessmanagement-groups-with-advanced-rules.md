---
title: "Naplnění skupiny dynamicky podle atributů objektu ve službě Azure Active Directory | Microsoft Docs"
description: "Jak – do této vytváření rozšířených pravidel, včetně členství ve skupině podporované výraz pravidlo operátory a parametry."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 04813a42-d40a-48d6-ae96-15b7e5025884
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: curtand
ms.reviewer: rodejo
ms.openlocfilehash: b9b5ddf42958a2b4e241d0252101d979009e7dc0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="populate-groups-dynamically-based-on-object-attributes"></a>Naplnění skupiny dynamicky podle atributů objektu
Klasický portál Azure poskytuje možnost povolit složitější založená na atributu dynamické členství ve skupinách pro skupiny Azure Active Directory (Azure AD).  

Pokud žádné atributy uživatele nebo zařízení změnit, systém vyhodnotí všechna pravidla dynamické skupiny v adresáři, abyste zjistili, zda by změna aktivovat libovolnou skupinu přidá nebo odebere. Pokud na uživatele nebo zařízení splňuje pravidlo ve skupině, přidají se jako člena této skupiny. Pokud se už splňovat pravidla, se odeberou.

> [!NOTE]
> - Pravidlo pro dynamické členství můžete nastavit pro skupiny zabezpečení nebo pro skupiny Office 365.
>
> - Tato funkce vyžaduje pro každého člena uživatel přidán do alespoň jednu skupinu dynamické licenci Azure AD Premium P1.
>
> - Můžete vytvořit skupinu dynamické pro zařízení nebo uživatelů, ale nelze vytvořit pravidlo, které obsahuje uživatele a zařízení.

> - V tuto chvíli není možné vytvořit skupinu zařízení podle vlastnící atributy uživatele. Pravidla členství zařízení může odkazovat pouze na okamžitou atributy zařízení objektů v adresáři.

## <a name="to-create-an-advanced-rule"></a>Vytvoření pokročilé pravidla
1. V [portál Azure classic](https://manage.windowsazure.com), vyberte **služby Active Directory**a pak otevřete adresáři vaší organizace.
2. Vyberte **skupiny** kartě a pak otevřete skupinu, kterou chcete upravit.
3. Vyberte **konfigurace** vyberte **rozšířené pravidlo** možnost a potom do textového pole zadejte rozšířeného pravidla.

## <a name="constructing-the-body-of-an-advanced-rule"></a>Vytváření textu rozšířené pravidlo
Pokročilé pravidlo, které můžete vytvořit pro dynamické členství ve skupinách je v podstatě binárního výrazu, který se skládá ze tří částí a výsledkem výsledek true nebo false. Jsou tři části:

* Levý parametr
* Binární operátor
* Pravé konstanta

Dokončení rozšířeného pravidla vypadá podobně jako tento: (leftParameter binaryOperator "RightConstant"), kde otevírání a kulaté závorky jsou vyžadovány pro celý výraz binární, dvojité uvozovky jsou požadovány pro správné konstanta a syntaxe Levý parametr je user.property. Rozšířené pravidlo se může skládat z více než jeden binární výrazy oddělených- a- nebo a - není logické operátory.
Následují příklady správně strukturovaný pokročilé pravidla:

* (user.department - eq "Prodej")- nebo (user.department - eq "Marketing")
* (user.department - eq "Prodej")- a - není (user.jobTitle – obsahuje "SDE")

Úplný seznam podporovaných parametrů a operátory výraz pravidlo najdete v níže uvedených částech.


Všimněte si, že vlastnost musí být předponu správný objekt typu: uživatel nebo zařízení.
Následující pravidlo se nezdaří ověření: poštovní – ne null

Správné pravidlo by byl:

User.mail – ne hodnotu null.

Celková délka textu rozšířeného pravidla nesmí překročit hodnotu 2048 znaků.

> [!NOTE]
> Řetězec a regex operace jsou malá a velká písmena.
> Řetězce obsahující uvozovky "by měly být ukončeny pomocí, například znak user.department - eq \`"Prodej".
> Uvozovky používejte pouze pro hodnoty typu řetězec a používat pouze angličtinu uvozovky.
>
>

## <a name="supported-expression-rule-operators"></a>Podporované výraz pravidlo operátory
V následující tabulce jsou uvedeny všechny operátory pravidlo podporované výrazu a jejich syntaxi, který se má použít v těle rozšířeného pravidla:

| Operátor | Syntaxe |
| --- | --- |
| Nerovná se |-ne |
| Rovná se |-eq |
| Není začíná |-notStartsWith |
| Začíná |-startsWith |
| Neobsahuje |-notContains |
| Contains |-obsahuje |
| Neodpovídá |-notMatch |
| Shoda |-odpovídat |
| V | -v |
| Není ve | -notIn |

## <a name="operator-precedence"></a>Priorita operátorů

Všechny operátory jsou uvedeny níže podle priority od nižší na vyšší, operátor ve stejném řádku jsou ve stejnou přednost – všechny – všechny - nebo - a - ne - eq - ne - startsWith - notStartsWith-obsahuje - notContains-odpovídat – notMatch-v - notIn

Všechny operátory lze použít s nebo bez předpony pomlčkou.

Všimněte si, že nejsou vždycky potřeboval závorky potřebujete přidat závorky, pokud přednost nesplňuje vaše požadavky například:

   User.Department – eq "Marketing" – a User.Country. – eq "US"

je ekvivalentní:

   (user.department – eq "Marketing") – a (User.Country. – eq "US")

## <a name="using-the--in-and--notin-operators"></a>Pomocí-v a notIn – operátory

Pokud chcete porovnat hodnotu atributu uživatele proti počet různých hodnot můžete použít-v nebo - notIn operátory. Tady je příklad použití-v operátoru:

    user.department -In [ "50001", "50002", "50003", “50005”, “50006”, “50007”, “50008”, “50016”, “50020”, “50024”, “50038”, “50039”, “51100” ]

Všimněte si použití "[" a "]" na začátku a konci seznamu hodnot. Tato podmínka vyhodnocena jako True hodnota rovná user.department jedna z hodnot v seznamu.

## <a name="query-error-remediation"></a>Náprava chyby dotazu
Následující tabulka uvádí možné chyby a jak je opravit, pokud k nim dojde.

| Chyba analýzy dotazu | Chyba použití | Opravené využití |
| --- | --- | --- |
| Chyba: Atribut není podporován. |(user.invalidProperty - eq "Value") |(user.department - eq "value")<br/>Vlastnost by měla shodovat s jedním z [podporované seznam vlastností](#supported-properties). |
| Chyba: Operátor není podporován pro atribut. |(user.accountEnabled – obsahuje hodnotu PRAVDA) |(user.accountEnabled - eq true)<br/>Je vlastnost typu boolean. Podporované operátory (-eq nebo - ne) pro logický typ ze seznamu výše. |
| Chyba: Chyba při kompilaci dotazu. |(user.department - eq "Prodej")- a (user.department - eq "Marketing") (user.userPrincipalName-match "*@domain.ext") |(user.department - eq "Prodej")- a (user.department - eq "Marketing")<br/>Logický operátor musí shodovat s jedním ze seznamu podporovaných vlastností výše. (user.userPrincipalName-shodovat s ". *@domain.ext") nebo (user.userPrincipalName-shodovat s "@domain.ext$") došlo k chybě v regulární výraz. |
| Chyba: Binárního výrazu není ve správném formátu. |(user.department – eq "Prodej") (user.department - eq "Prodej") (user.department-eq "Prodej") |(user.accountEnabled - eq true)- a (user.userPrincipalName – obsahuje "alias@domain")<br/>Dotaz obsahuje více chyb. Závorky není ve správném místě. |
| Chyba: Během nastavení dynamické členství došlo k neznámé chybě. |(user.accountEnabled - eq "True" a user.userPrincipalName – obsahuje "alias@domain") |(user.accountEnabled - eq true)- a (user.userPrincipalName – obsahuje "alias@domain")<br/>Dotaz obsahuje více chyb. Závorky není ve správném místě. |

## <a name="supported-properties"></a>Podporovaných vlastností
Tady jsou všechny vlastnosti uživatele, které můžete použít v pokročilé pravidla:

### <a name="properties-of-type-boolean"></a>Vlastnosti typu logická hodnota
Povolené operátory

* -eq
* -ne

| Vlastnosti | Povolené hodnoty | Využití |
| --- | --- | --- |
| accountEnabled |Hodnota TRUE, false |user.accountEnabled - eq true |
| dirSyncEnabled |Hodnota TRUE, false |user.dirSyncEnabled - eq true |

### <a name="properties-of-type-string"></a>Vlastnosti typu řetězec
Povolené operátory

* -eq
* -ne
* -notStartsWith
* -StartsWith
* -obsahuje
* -notContains
* -odpovídat
* -notMatch
* -v
* -notIn

| Vlastnosti | Povolené hodnoty | Využití |
| --- | --- | --- |
| city |Všechny hodnoty řetězce nebo $null |(user.city - eq "value") |
| Země |Všechny hodnoty řetězce nebo $null |(User.Country. - eq "value") |
| NázevSpolečnosti | Všechny hodnoty řetězce nebo $null | (user.companyName - eq "value") |
| Oddělení |Všechny hodnoty řetězce nebo $null |(user.department - eq "value") |
| displayName |Libovolnou hodnotu řetězce |(user.displayName - eq "value") |
| facsimileTelephoneNumber |Všechny hodnoty řetězce nebo $null |(user.facsimileTelephoneNumber - eq "value") |
| givenName |Všechny hodnoty řetězce nebo $null |(user.givenName - eq "value") |
| pracovní funkce |Všechny hodnoty řetězce nebo $null |(user.jobTitle - eq "value") |
| E-mailu |Všechny hodnoty řetězce nebo $null (adresa SMTP uživatele) |(user.mail - eq "value") |
| mailNickName |Libovolnou hodnotu řetězce (e-mailu alias uživatele) |(user.mailNickName - eq "value") |
| mobilní |Všechny hodnoty řetězce nebo $null |(user.mobile - eq "value") |
| objectId |GUID objektu uživatele |(user.objectId - eq "1111111-1111-1111-1111-111111111111") |
| onPremisesSecurityIdentifier | Místní identifikátor zabezpečení (SID) pro uživatele, kteří se synchronizovaly z místní do cloudu. |(user.onPremisesSecurityIdentifier - eq "S-1-1-11-1111111111-1111111111-1111111111-1111111") |
| passwordPolicies |Žádný DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration, DisableStrongPassword |(user.passwordPolicies - eq "DisableStrongPassword") |
| physicalDeliveryOfficeName |Všechny hodnoty řetězce nebo $null |(user.physicalDeliveryOfficeName - eq "value") |
| PSČ |Všechny hodnoty řetězce nebo $null |(user.postalCode - eq "value") |
| preferredLanguage |Kód ISO 639-1 |(user.preferredLanguage - eq "en US") |
| sipProxyAddress |Všechny hodnoty řetězce nebo $null |(user.sipProxyAddress - eq "value") |
| state |Všechny hodnoty řetězce nebo $null |(user.state - eq "value") |
| StreetAddress |Všechny hodnoty řetězce nebo $null |(user.streetAddress - eq "value") |
| Příjmení |Všechny hodnoty řetězce nebo $null |(user.surname - eq "value") |
| telephoneNumber |Všechny hodnoty řetězce nebo $null |(user.telephoneNumber - eq "value") |
| usageLocation |Dva písmeny směrové číslo země |(user.usageLocation - eq "US") |
| UserPrincipalName |Libovolnou hodnotu řetězce |(user.userPrincipalName - eq "alias@domain") |
| UserType |člen hosta $null |(user.userType - eq "Člen") |

### <a name="properties-of-type-string-collection"></a>Vlastnosti typu řetězec kolekce
Povolené operátory

* -obsahuje
* -notContains

| Vlastnosti | Povolené hodnoty | Využití |
| --- | --- | --- |
| otherMails |Libovolnou hodnotu řetězce |(user.otherMails – obsahuje "alias@domain") |
| proxyAddresses |SMTP: alias@domain smtp:alias@domain |(user.proxyAddresses – obsahuje "SMTP: alias@domain") |

## <a name="multi-value-properties"></a>Vícehodnotový vlastnosti
Povolené operátory

* -všechny (uspokojit, když minimálně jedna položka v kolekci odpovídá podmínku)
* -všechny (uspokojit, když všechny položky v kolekci splňují podmínku)

| Vlastnosti | Hodnoty | Využití |
| --- | --- | --- |
| assignedPlans |Každý objekt v kolekci zpřístupní následující vlastnosti řetězce: capabilityStatus, služby, servicePlanId |user.assignedPlans-všechny (assignedPlan.servicePlanId - eq "efb87545-963c-4e0d-99df-69c6916d9eb0"- a assignedPlan.capabilityStatus - eq "Povoleno") |

Vícehodnotový vlastnosti jsou kolekce objektů stejného typu. Můžete použít – všechny a - všechny operátory použít podmínku, na jeden nebo všechny položky v kolekci, v uvedeném pořadí. Například:

assignedPlans je vícehodnotový vlastnost, která uvádí všechny plány služby, které jsou přiřazeny uživateli. Níže výraz bude vyberte uživatele, kteří mají Exchange Online (plán 2) plán služeb, který je sám ve stavu povoleno:

```
user.assignedPlans -any (assignedPlan.servicePlanId -eq "efb87545-963c-4e0d-99df-69c6916d9eb0" -and assignedPlan.capabilityStatus -eq "Enabled")
```

(Identifikátor Guid identifikuje tarifu Exchange Online (plán 2).)

> [!NOTE]
> To je užitečné, pokud chcete identifikovat všechny uživatele, pro kterého Office 365 (nebo jinou službu Microsoft Online) povoleno schopnosti, třeba pro jejich sadu zásad.

Následující výraz vybere všechny uživatele, kteří mají plán žádné služby, která souvisí se službou Intune (identifikovaný název služby "SCO"):
```
user.assignedPlans -any (assignedPlan.service -eq "SCO" -and assignedPlan.capabilityStatus -eq "Enabled")
```

## <a name="use-of-null-values"></a>Použití hodnoty Null

Chcete-li určit hodnotu null v pravidle, můžete použít "null" nebo $null. Příklad:

   je ekvivalentní user.mail – ne $null User.mail – ne hodnotu null.

## <a name="extension-attributes-and-custom-attributes"></a>Atributy rozšíření a vlastní atributy
Atributy rozšíření a vlastní atributy jsou podporovány v pravidlech dynamické členství.

Rozšíření atributy jsou synchronizované z místní okno Server AD a proveďte formát "ExtensionAttributeX", kde X je rovno 1 až 15.
Jedná se například pravidla, které používá atribut rozšíření

(user.extensionAttribute15 - eq "Marketing")

Vlastní atributy se synchronizují z místního systému Windows Server AD nebo z připojených aplikací SaaS a formát "user.extension_[GUID]\__ [atribut]", kde [identifikátor GUID] je jedinečný identifikátor v AAD aplikace, která vytvoří atribut v AAD a [atribut] je název atributu, který byl vytvořen.
Je například pravidla, které používá vlastní atribut

User.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  

Název vlastního atributu naleznete v adresáři dotazováním uživatele je atribut pomocí Průzkumníka grafu a vyhledávání pro název atributu.

## <a name="direct-reports-rule"></a>Pravidlo "Přímé podřízené"
Můžete vytvořit skupina obsahující všechny přímé podřízené manažera. Pokud v budoucnu změnit manažera přímé podřízené skupiny členství se upraví, automaticky.

> [!NOTE]
> 1. Pravidla pro práci, zajistěte, aby **Manager ID** vlastnost je správně nastavena na uživatele ve vašem klientovi. Aktuální hodnota pro uživatele můžete zkontrolovat na jejich **kartu profil**.
> 2. Toto pravidlo podporuje pouze **přímé** sestavy. Aktuálně není možné vytvořit skupinu pro vnořené hierarchie, například ke skupině, která obsahuje přímé podřízené a jejich sestavy.

**Konfigurace skupiny**

1. Postupujte podle kroků 1 až 5 z oddílu [k vytvoření rozšířeného pravidla](#to-create-the-advanced-rule)a vyberte **typ členství** z **dynamické uživatele**.
2. Na **pravidla dynamické členství** okno, zadejte pravidlo s následující syntaxí:

    *Přímé podřízené pro "{obectID_of_manager}"*

    Příklad platné pravidlo:
```
                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863"
```
    where “62e19b97-8b3d-4d4a-a106-4ce66896a863” is the objectID of the manager. The object ID can be found on manager's **Profile tab**.
3. Po uložení pravidlo, všichni uživatelé se zadanou hodnotou Manager ID přidá do skupiny.

## <a name="using-attributes-to-create-rules-for-device-objects"></a>Vytváření pravidel pro objekty zařízení pomocí atributů
Můžete také vytvořit pravidlo, které vybere objekty zařízení pro členství ve skupině. Můžete použít následující atributy zařízení:

| Vlastnosti              | Povolené hodnoty                  | Využití                                                       |
|-------------------------|---------------------------------|-------------------------------------------------------------|
| accountEnabled          | Hodnota TRUE, false                      | (device.accountEnabled - eq true)                            |
| displayName             | Libovolnou hodnotu řetězce                | (device.displayName - eq "Rob Iphone")                       |
| deviceOSType            | Libovolnou hodnotu řetězce                | (device.deviceOSType - eq "IOS")                             |
| DeviceOSVersion         | Libovolnou hodnotu řetězce                | (zařízení. OSVersion - eq "9.1")                                |
| deviceCategory          | Libovolnou hodnotu řetězce                | (device.deviceCategory - eq "")                              |
| DeviceManufacturer      | Libovolnou hodnotu řetězce                | (device.deviceManufacturer - eq "Microsoft")                 |
| DeviceModel             | Libovolnou hodnotu řetězce                | (device.deviceModel - eq "IPhone 7 +")                        |
| deviceOwnership         | Libovolnou hodnotu řetězce                | (device.deviceOwnership - eq "")                             |
| domainName              | Libovolnou hodnotu řetězce                | (device.domainName - eq "contoso.com")                       |
| enrollmentProfileName   | Libovolnou hodnotu řetězce                | (device.enrollmentProfileName - eq "")                       |
| isRooted                | Hodnota TRUE, false                      | (device.deviceOSType - eq true)                              |
| managementType          | Libovolnou hodnotu řetězce                | (device.managementType - eq "")                              |
| OrganizationalUnit      | Libovolnou hodnotu řetězce                | (device.organizationalUnit - eq "")                          |
| deviceId                | platné ID zařízení                | (device.deviceId - eq "d4fe7726-5966-431c-b3b8-cddc8fdb717d") |
| objectId                | platný objectId AAD            | (device.objectId - eq "76ad43c9-32c5-45e8-a272-7b58b58f596d") |

> [!NOTE]
> Tato pravidla zařízení nelze vytvořit pomocí rozevíracího seznamu "jednoduché pravidlo" na portálu Azure classic.
>
>

## <a name="next-steps"></a>Další kroky
Následující články poskytují další informace o službě Azure Active Directory.

* [Řešení potíží s dynamické členství ve skupinách](active-directory-accessmanagement-troubleshooting.md)
* [Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory](active-directory-manage-groups.md)
* [Rutiny služby Azure Active Directory pro konfiguraci nastavení skupiny](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
