---
title: "na základě aaaAttribute dynamické členství ve skupině v Azure Active Directory | Microsoft Docs"
description: "Jak toocreate rozšířené pravidla pro dynamické členství ve skupině včetně podporovaných výraz pravidlo operátory a parametrů."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: fb434cc2-9a91-4ebf-9753-dd81e289787e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8cd06ed70433eff65401c67d7351d5dcc12a9dd5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-attribute-based-rules-for-dynamic-group-membership-in-azure-active-directory"></a>Vytvořit pravidla založená na atributu pro dynamické členství ve skupině v Azure Active Directory
V Azure Active Directory (Azure AD) můžete vytvořit rozšířených pravidel tooenable komplexní založená na atributu dynamické členství ve skupinách. Tento článek podrobně hello atributy a syntaxe toocreate pravidla dynamické členství pro uživatele nebo zařízení.

Pokud žádné atributy uživatele nebo zařízení změnu hello systému hodnotí všechna pravidla dynamické skupiny v adresáři toosee Pokud by aktivovat hello změnu všechny skupiny přidá nebo odebere. Pokud na uživatele nebo zařízení splňuje pravidlo ve skupině, přidají se jako člena této skupiny. Pokud už vyhovují hello pravidlo, budou odstraněny.

> [!NOTE]
> - Pravidlo pro dynamické členství můžete nastavit pro skupiny zabezpečení nebo pro skupiny Office 365.
>
> - Tato funkce vyžaduje licenci Azure AD Premium P1 pro každou dynamickou skupinu uživatele člen přidané tooat minimálně jeden.
>
> - Můžete vytvořit skupinu dynamické pro zařízení nebo uživatelů, ale nelze vytvořit pravidlo, které obsahuje uživatele a zařízení.

> - Momentálně hello není možné toocreate skupiny zařízení podle vlastnící atributy uživatele. Pravidla členství zařízení můžete pouze odkaz na okamžitý atributy zařízení objektů v adresáři hello.

## <a name="toocreate-an-advanced-rule"></a>toocreate rozšířené pravidlo
1. Přihlaste se toohello [centra pro správu Azure AD](https://aad.portal.azure.com) pomocí účtu, který je globální správce nebo správce účtu uživatele.
2. Vyberte **uživatelů a skupin**.
3. Vyberte **všechny skupiny**.

   ![Okno skupiny otevírání hello](./media/active-directory-groups-dynamic-membership-azure-portal/view-groups-blade.png)
4. V **všechny skupiny**, vyberte **nové skupiny**.

   ![Přidat nové skupiny](./media/active-directory-groups-dynamic-membership-azure-portal/add-group-type.png)
5. Na hello **skupiny** okno, zadejte název a popis nové skupiny hello. Vyberte **typ členství** buď **dynamické uživatele** nebo **dynamické zařízení**, v závislosti na tom, jestli chcete toocreate pravidlo pro uživatele nebo zařízení a potom vyberte **Přidat dynamické dotazu**. Atributy hello používané pro pravidla zařízení, najdete v části [pomocí pravidel toocreate atributy pro objekty zařízení](#using-attributes-to-create-rules-for-device-objects).

   ![Přidat pravidlo dynamické členství](./media/active-directory-groups-dynamic-membership-azure-portal/add-dynamic-group-rule.png)
6. Na hello **pravidla dynamické členství** okno, zadejte pravidla do hello **přidat dynamické členství rozšířeného pravidla** , stiskněte klávesu Enter a pak vyberte **vytvořit** dole hello okno Hello.
7. Vyberte **vytvořit** na hello **skupiny** okno toocreate hello skupiny.

## <a name="constructing-hello-body-of-an-advanced-rule"></a>Vytváření textu hello rozšířené pravidlo
Hello pokročilé pravidlo, které můžete vytvořit pro hello dynamické členství ve skupinách je v podstatě binárního výrazu, který se skládá ze tří částí a výsledkem výsledek true nebo false. jsou Hello tří částí:

* Levý parametr
* Binární operátor
* Pravé konstanta

Dokončení rozšířeného pravidla vypadá podobně jako toothis: (leftParameter binaryOperator "RightConstant"), kde jsou volitelné pro celý výraz binární hello hello otvírání a zavírání závorky, dvojité uvozovky jsou volitelné taky jen vyžaduje pro správné hello Konstanta, když je řetězec, který hello syntaxe pro levý parametr hello je user.property. Rozšířené pravidlo se může skládat z více než jeden binární výrazy oddělených hello- a- nebo a - není logické operátory.

Hello Následují příklady správně strukturovaný rozšířeného pravidla:
```
(user.department -eq "Sales") -or (user.department -eq "Marketing")
(user.department -eq "Sales") -and -not (user.jobTitle -contains "SDE")
```
Hello úplný seznam podporovaných parametrů a operátory výraz pravidlo najdete v níže uvedených částech. Atributy hello používané pro pravidla zařízení, najdete v části [pomocí pravidel toocreate atributy pro objekty zařízení](#using-attributes-to-create-rules-for-device-objects).

Hello celková délka textu hello pokročilé pravidla nesmí překročit hodnotu 2048 znaků.

> [!NOTE]
> Operace řetězec a regex se nerozlišují malá a velká písmena. Můžete také provést kontroly hodnotu Null, například pomocí $null jako konstanta, user.department - eq $null.
> Řetězce obsahující uvozovky "by měly být ukončeny pomocí, například znak user.department - eq \`"Prodej".

## <a name="supported-expression-rule-operators"></a>Podporované výraz pravidlo operátory
Hello následující tabulka uvádí všechny podporované hello výraz pravidlo operátory a jejich syntaxi toobe použit v textu hello hello rozšířeného pravidla:

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

Níže jsou uvedeny všechny operátory za z toohigher nižší prioritu. Operátory na stejném řádku jsou ve stejné prioritu:
````
-any -all
-or
-and
-not
-eq -ne -startsWith -notStartsWith -contains -notContains -match –notMatch -in -notIn
````
Všechny operátory lze použít s nebo bez předpony pomlčka hello. Závorky jsou potřeba jenom v případě, že přednost nesplňuje vaše požadavky.
Například:
```
   user.department –eq "Marketing" –and user.country –eq "US"
```
je ekvivalentní:
```
   (user.department –eq "Marketing") –and (user.country –eq "US")
```
## <a name="using-hello--in-and--notin-operators"></a>Pomocí hello - v a notIn – operátory

Pokud chcete toocompare hello hodnota atributu uživatele proti počet různých hodnot můžete použít hello - v nebo - notIn operátory. Tady je příklad použití hello – v operátoru:
```
    user.department -In [ "50001", "50002", "50003", “50005”, “50006”, “50007”, “50008”, “50016”, “50020”, “50024”, “50038”, “50039”, “51100” ]
```
Všimněte si použití hello hello "[" a "]" v hello začátek a konec hello seznamu hodnot. Tato podmínka vyhodnocena jako tooTrue hello hodnota rovná user.department jeden hello hodnot v seznamu hello.


## <a name="query-error-remediation"></a>Náprava chyby dotazu
Hello následující tabulka uvádí možné chyby a jak toocorrect vzbuzené. Pokud k nim dojde.

| Chyba analýzy dotazu | Chyba použití | Opravené využití |
| --- | --- | --- |
| Chyba: Atribut není podporován. |(user.invalidProperty - eq "Value") |(user.department - eq "value")<br/>Vlastnost by měla shodovat s jedním z hello [podporované seznam vlastností](#supported-properties). |
| Chyba: Operátor není podporován pro atribut. |(user.accountEnabled – obsahuje hodnotu PRAVDA) |(user.accountEnabled - eq true)<br/>Je vlastnost typu boolean. Operátory hello podporované (-eq nebo - ne) na typ boolean z hello výše seznamu. |
| Chyba: Chyba při kompilaci dotazu. |(user.department - eq "Prodej")- a (user.department - eq "Marketing") (user.userPrincipalName-match "*@domain.ext") |(user.department - eq "Prodej")- a (user.department - eq "Marketing")<br/>Logický operátor by měl odpovídat jedné ze seznamu vlastnosti hello podporované výše. (user.userPrincipalName-shodovat s ". *@domain.ext") nebo (user.userPrincipalName-shodovat s "@domain.ext$") došlo k chybě v regulární výraz. |
| Chyba: Binárního výrazu není ve správném formátu. |(user.department – eq "Prodej") (user.department - eq "Prodej") (user.department-eq "Prodej") |(user.accountEnabled - eq true)- a (user.userPrincipalName – obsahuje "alias@domain")<br/>Dotaz obsahuje více chyb. Závorky není ve správném místě. |
| Chyba: Během nastavení dynamické členství došlo k neznámé chybě. |(user.accountEnabled - eq "True" a user.userPrincipalName – obsahuje "alias@domain") |(user.accountEnabled - eq true)- a (user.userPrincipalName – obsahuje "alias@domain")<br/>Dotaz obsahuje více chyb. Závorky není ve správném místě. |

## <a name="supported-properties"></a>Podporovaných vlastností
všechny vlastnosti hello uživatele, které můžete použít v pokročilé pravidla se Hello následující:

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
| E-mailu |Všechny hodnoty řetězce nebo $null (adresa SMTP uživatele hello) |(user.mail - eq "value") |
| mailNickName |Libovolnou hodnotu řetězce (e-mailu alias uživatele hello) |(user.mailNickName - eq "value") |
| mobilní |Všechny hodnoty řetězce nebo $null |(user.mobile - eq "value") |
| objectId |GUID objektu uživatele hello |(user.objectId - eq "1111111-1111-1111-1111-111111111111") |
| onPremisesSecurityIdentifier | Místní identifikátor zabezpečení (SID) pro uživatele, kteří se synchronizovaly z místní toohello cloudu. |(user.onPremisesSecurityIdentifier - eq "S-1-1-11-1111111111-1111111111-1111111111-1111111") |
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

* -všechny (splněna, když minimálně jedna položka v kolekci hello odpovídá hello podmínku)
* -všechny (uspokojit, když všechny položky v kolekci hello vyhovují podmínce hello)

| Vlastnosti | Hodnoty | Využití |
| --- | --- | --- |
| assignedPlans |Každý objekt v kolekci hello zpřístupní hello následující vlastnosti řetězce: capabilityStatus, služby, servicePlanId |user.assignedPlans-všechny (assignedPlan.servicePlanId - eq "efb87545-963c-4e0d-99df-69c6916d9eb0"- a assignedPlan.capabilityStatus - eq "Povoleno") |

Vícehodnotový vlastnosti jsou kolekce objektů hello stejného typu. Můžete použít – všechny a - všechny operátory tooapply podmínku tooone nebo všech hello položky v kolekci hello v uvedeném pořadí. Například:

assignedPlans je vícehodnotový vlastnost, která uvádí všechny plány služby přiřazený uživatel toohello. Hello níže výraz bude vyberte uživatele, kteří mají hello Exchange Online (plán 2) plán služeb, který je sám ve stavu povoleno:

```
user.assignedPlans -any (assignedPlan.servicePlanId -eq "efb87545-963c-4e0d-99df-69c6916d9eb0" -and assignedPlan.capabilityStatus -eq "Enabled")
```

(identifikátor Guid hello identifikuje plán služby Exchange Online (plán 2) hello).

> [!NOTE]
> To je užitečné, pokud chcete všechny uživatele, tooidentify pro kterého Office 365 (nebo jinou službu Microsoft Online) povolena funkce pro příklad tootarget je pomocí sady zásad.

Hello následující výraz vybere všechny uživatele, kteří mají všechny plán služeb, který je přidružen hello služby Intune (identifikovaný název služby "SCO"):
```
user.assignedPlans -any (assignedPlan.service -eq "SCO" -and assignedPlan.capabilityStatus -eq "Enabled")
```

## <a name="use-of-null-values"></a>Použití hodnoty Null

Hodnota null se toospecify v pravidle, můžete použít "null" nebo $null. Příklad:
```
   user.mail –ne null
```
je ekvivalentní
```
   user.mail –ne $null
   ```

## <a name="extension-attributes-and-custom-attributes"></a>Atributy rozšíření a vlastní atributy
Atributy rozšíření a vlastní atributy jsou podporovány v pravidlech dynamické členství.

Rozšíření atributy jsou synchronizované z místní okno Server AD a proveďte hello formát "ExtensionAttributeX", kde X je rovno 1 až 15.
Jedná se například pravidla, které používá atribut rozšíření
```
(user.extensionAttribute15 -eq "Marketing")
```
Vlastní atributy se synchronizují z místního systému Windows Server AD nebo z připojeného SaaS aplikace hello hello formátu a z "user.extension_[GUID]\__ [atribut]", kde [identifikátor GUID] je jedinečný identifikátor hello v AAD aplikace hello, atribut vytvořený hello v AAD a [atribut] je hello název atributu hello, protože byla vytvořena.
Je například pravidla, které používá vlastní atribut
```
user.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  
```
Hello vlastním názvu atributu naleznete v adresáři hello dotazováním uživatele je atribut pomocí Průzkumníka grafu a vyhledávání pro název atributu hello.

## <a name="direct-reports-rule"></a>Pravidlo "Přímé podřízené"
Můžete vytvořit skupina obsahující všechny přímé podřízené manažera. Pokud správce hello přímé podřízené změnit v hello budoucí, členství ve skupině hello se upraví, automaticky.

> [!NOTE]
> 1. Pro pravidlo toowork hello, ujistěte se, zda text hello **Manager ID** vlastnost je správně nastavena na uživatele ve vašem klientovi. Aktuální hodnota hello pro uživatele můžete zkontrolovat na jejich **kartu profil**.
> 2. Toto pravidlo podporuje pouze **přímé** sestavy. Aktuálně není možné toocreate skupinu pro vnořené hierarchie, například ke skupině, která obsahuje přímé podřízené a jejich sestavy.

**tooconfigure hello skupiny**

1. Postupujte podle kroků 1 až 5 z oddílu [toocreate hello rozšířeného pravidla](#to-create-the-advanced-rule)a vyberte **typ členství** z **dynamické uživatele**.
2. Na hello **pravidla dynamické členství** okno, zadejte hello pravidlo s hello následující syntaxi:

    *Přímé podřízené pro "{obectID_of_manager}"*

    Příklad platné pravidlo:
```
                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863"
```
    where “62e19b97-8b3d-4d4a-a106-4ce66896a863” is hello objectID of hello manager. hello object ID can be found on manager's **Profile tab**.
3. Po uložení hello pravidlo, všichni uživatelé s hello zadaný toohello skupiny bude přidána hodnota ID správce.

## <a name="using-attributes-toocreate-rules-for-device-objects"></a>Použití pravidel toocreate atributy pro objekty zařízení
Můžete také vytvořit pravidlo, které vybere objekty zařízení pro členství ve skupině. Hello následující atributy zařízení lze použít.

 Atribut zařízení  | Hodnoty | Příklad
 ----- | ----- | ----------------
 accountEnabled | Hodnota TRUE, false | (device.accountEnabled - eq true)
 displayName | Libovolnou hodnotu řetězce |(device.displayName - eq "Rob Iphone")
 deviceOSType | Libovolnou hodnotu řetězce | (device.deviceOSType - eq "IOS")
 DeviceOSVersion | Libovolnou hodnotu řetězce | (zařízení. OSVersion - eq "9.1")
 deviceCategory | Název kategorie platný zařízení | (device.deviceCategory - eq "BYOD")
 DeviceManufacturer | Libovolnou hodnotu řetězce | (device.deviceManufacturer - eq "Samsung")
 DeviceModel | Libovolnou hodnotu řetězce | (device.deviceModel - eq "iPad vzduchem")
 deviceOwnership | Osobní společnosti | (device.deviceOwnership - eq "Společnost")
 domainName | Libovolnou hodnotu řetězce | (device.domainName - eq "contoso.com")
 enrollmentProfileName | Název profilu registrace zařízení Apple | (device.enrollmentProfileName - eq "DEP Iphony")
 isRooted | Hodnota TRUE, false | (device.isRooted - eq true)
 managementType | MDM (pro mobilní zařízení)<br>PC (pro počítače spravované agentem hello počítačů s Intune) | (device.managementType - eq "MDM")
 OrganizationalUnit | libovolnou hodnotu řetězce odpovídající názvu hello hello organizační jednotky ve služby Active Directory místní nastavení | (device.organizationalUnit - eq "Počítačů USA")
 deviceId | platné ID zařízení služby Azure AD | (device.deviceId - eq "d4fe7726-5966-431c-b3b8-cddc8fdb717d")
 objectId | ID objektu platný Azure AD |  (76ad43c9-32c5-45e8-a272-7b58b58f596d device.objectId - eq")




## <a name="next-steps"></a>Další kroky
Tyto články poskytují další informace o skupin v Azure Active Directory.

* [Zobrazení existujících skupin](active-directory-groups-view-azure-portal.md)
* [Vytvoření nové skupiny a přidávání členů](active-directory-groups-create-azure-portal.md)
* [Spravovat nastavení skupiny](active-directory-groups-settings-azure-portal.md)
* [Správa členství ve skupině](active-directory-groups-membership-azure-portal.md)
* [Dynamická pravidla pro uživatele ve skupině pro správu](active-directory-groups-dynamic-membership-azure-portal.md)
