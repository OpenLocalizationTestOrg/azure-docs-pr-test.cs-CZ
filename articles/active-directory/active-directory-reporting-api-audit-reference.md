---
title: "auditování služby Active Directory aaaAzure referenční dokumentace rozhraní API | Microsoft Docs"
description: "Jak tooget pracovat s hello API auditování Azure Active Directory"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 44e46be8-09e5-4981-be2b-d474aaa92792
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/05/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 5f33b62ede9be445f35704739e328580dc454368
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-audit-api-reference"></a>Azure Active Directory auditu referenční dokumentace rozhraní API
Toto téma je součástí kolekce témat o hello Azure Active Directory reporting rozhraní API.  
Generování sestav služby Azure AD poskytuje rozhraní API, které vám umožní tooaccess auditu dat pomocí kódu nebo související nástroje.
Hello obor tohoto tématu je tooprovide vám referenční informace o hello **audit rozhraní API**.

Přejděte na téma:

* [Protokoly auditu](active-directory-reporting-azure-portal.md#activity-reports) další koncepční informace

* [Začínáme s Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md) Další informace o hello reporting rozhraní API.


Pro:

- Nejčastější dotazy, přečtěte si naše [– nejčastější dotazy](active-directory-reporting-faq.md) 

- Problémy prosím [souboru lístek podpory](active-directory-troubleshooting-support-howto.md) 


## <a name="who-can-access-hello-data"></a>Kdo může přistupovat k datům hello?
* Uživatelé v roli správce zabezpečení nebo zabezpečení čtečky hello
* Globální správci
* Jakékoli aplikaci, která má autorizaci tooaccess hello rozhraní API (autorizace služby app lze pouze na základě oprávnění globálního správce)

## <a name="prerequisites"></a>Požadavky
V pořadí tooaccess to sestavy prostřednictvím hello Reporting rozhraní API, musíte mít:

* [Lepší edici nebo Azure Active Directory volné](active-directory-editions.md)
* Dokončené hello [požadavky tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md). 

## <a name="accessing-hello-api"></a>Přístup k hello API
Toto rozhraní API můžete buď přistupovat prostřednictvím hello [grafu Explorer](https://graphexplorer2.cloudapp.net) nebo prostřednictvím kódu programu, například pomocí prostředí PowerShell. V pořadí pro prostředí PowerShell toocorrectly interpretovat se syntaxí filtru OData hello používá při voláních REST grafu AAD, je nutné použít hello backtick (neboli: čárka) znak příliš "znaku" hello $. Hello backtick znak slouží jako [Powershellu řídicí znak](https://technet.microsoft.com/library/hh847755.aspx), povolení prostředí PowerShell toodo literálu výklad hello znak $ a zabránit složitá jako název proměnné prostředí PowerShell (ie: $filter).

hello grafu Explorer je aktivní Hello tohoto tématu. V příkladu prostředí PowerShell najdete [skript prostředí PowerShell](active-directory-reporting-api-audit-samples.md#powershell-script).

## <a name="api-endpoint"></a>Koncový bod rozhraní API
Toto rozhraní API pomocí hello následující identifikátor URI se můžete dostat:  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

Neexistuje žádné omezení na hello počet záznamů vrácených API auditu hello Azure AD (pomocí stránkování OData).
Pro uchování omezení pro vytváření sestav dat, podívejte se na [Reporting zásady uchovávání informací](active-directory-reporting-retention.md).

Toto volání se vrátí hello data v dávkách. Má každé dávky nesmí být delší než 1 000 záznamů.  
hello další dávku tooget záznamů, použijte odkaz Další hello. Získáte informace o skiptoken hello z první sady hello vrácené záznamy. token přeskočit Hello bude na konci hello hello sadu výsledků dotazu.  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a>Podporované filtry
Můžete zúžit hello počet záznamů, které se vrátí pomocí rozhraní API volat v podobě filtru.  
Pro přihlášení rozhraní API související data, hello následující filtry jsou podporovány:

* **$top =\<počet toobe záznamů vrácených\>**  -toolimit hello počet vrácených záznamů. Toto je náročná operace. Pokud chcete, aby tooreturn tisíc objektů, které byste neměli používat tento filtr.     
* **$filter =\<údajů filtru\>**  -toospecify na základě hello podporovaný filtr polí, hello typ záznamy, na kterých vám nejvíc záleží

## <a name="supported-filter-fields-and-operators"></a>Pole podporovaný filtr a operátory
toospecify hello typu záznamů, které se zajímáte o, můžete vytvořit filtr příkaz, který může obsahovat jedno nebo kombinaci hello následující pole filtru:

* [Datum](#activitydate) -definuje datum nebo rozsah dat
* [kategorie](#category) – definuje kategorie hello chcete toofilter na.
* [activityStatus](#activitystatus) -definuje hello stav aktivity
* [activityType](#activitytype) -definuje hello typ aktivity
* [aktivita](#activity) -definuje hello aktivitu jako řetězec  
* [objektu actor nebo název](#actorname) -definuje hello objektu actor v podobě hello actor názvu
* [objektu actor/objectid](#actorobjectid) -definuje hello objektu actor v podobě hello actor ID   
* [objektu actor/upn](#actorupn) -definuje hello objektu actor v podobě hello actor název Princip uživatele (UPN) 
* [Cílová nebo](#targetname) -definuje hello cíl v podobě hello actor názvu
* [cíl/objectid](#targetobjectid) -definuje hello cíl v podobě ID cíle hello  
* [cíl/upn](#targetupn) -definuje hello objektu actor v podobě hello actor název Princip uživatele (UPN)   

- - -
### <a name="activitydate"></a>Datum
**Podporované operátory**: eq, ge, le, gt, lt

**Příklad**:

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=activityDate gt ' + $7daysago    

**Poznámky k**:

data a času musí být ve formátu UTC

- - -
### <a name="category"></a>category

**Podporované hodnoty**:

| Kategorie                         | Hodnota     |
| :--                              | ---       |
| Základní adresář                   | Adresář |
| Samoobslužná správa hesel | SSPR      |
| Samoobslužná správa skupin    | SSGM      |
| Zřizování účtů             | Sync      |
| Automatická změna hesel      | Automatická změna hesel |
| Identity Protection              | IdentityProtection |
| Pozvaní uživatelé                    | Pozvaní uživatelé |
| Služba MIM                      | Služba MIM |



**Podporované operátory**: eq

**Příklad**:

    $filter=category eq 'SSPR'
- - -
### <a name="activitystatus"></a>ActivityStatus

**Podporované hodnoty**:

| Stav aktivity | Hodnota |
| :--             | ---   |
| Úspěch         | 0     |
| Selhání         | - 1   |

**Podporované operátory**: eq

**Příklad**:

    $filter=activityStatus eq -1    

---
### <a name="activitytype"></a>activityType
**Podporované operátory**: eq

**Příklad**:

    $filter=activityType eq 'User'    

**Poznámky k**:

malá a velká písmena

- - -
### <a name="activity"></a>Aktivity
**Podporované operátory**: eq, obsahuje, startsWith

**Příklad**:

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')    

**Poznámky k**:

malá a velká písmena

- - -
### <a name="actorname"></a>objektu actor nebo název
**Podporované operátory**: eq, obsahuje, startsWith

**Příklad**:

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')    

**Poznámky k**:

Velká a malá písmena

- - -
### <a name="actorobjectid"></a>objektu actor/objectId
**Podporované operátory**: eq

**Příklad**:

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    


- - -
### <a name="targetname"></a>Cílová nebo
**Podporované operátory**: eq, obsahuje, startsWith

**Příklad**:

    $filter=targets/any(t: t/name eq 'some name')    

**Poznámky k**:

Velká a malá písmena

- - -
### <a name="targetupn"></a>cíl/upn
**Podporované operátory**: eq startsWith

**Příklad**:

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc'))    

**Poznámky k**:

* Velká a malá písmena
* Je třeba při dotazování Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity tooadd hello úplný obor názvů

- - -
### <a name="targetobjectid"></a>cíl/objectId
**Podporované operátory**: eq

**Příklad**:

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

- - -
### <a name="actorupn"></a>objektu actor/upn
**Podporované operátory**: eq startsWith

**Příklad**:

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')    

**Poznámky k**:

* Velká a malá písmena 
* Je třeba při dotazování Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity tooadd hello úplný obor názvů

- - -
## <a name="next-steps"></a>Další kroky
* Chcete pro filtrovaný systému aktivity toosee příklady? Podívejte se na hello [ukázky auditu rozhraní API služby Azure Active Directory](active-directory-reporting-api-audit-samples.md).
* Chcete, aby tooknow Další informace o vytváření sestav API hello Azure AD? V tématu [Začínáme s Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md).

