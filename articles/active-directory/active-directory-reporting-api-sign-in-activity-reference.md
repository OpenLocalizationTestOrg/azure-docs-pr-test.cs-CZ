---
title: "Sestava aktivit aaaAzure přihlášení služby Active Directory referenční dokumentace rozhraní API | Microsoft Docs"
description: "Referenční dokumentace pro sestavu aktivit hello přihlášení k Azure Active Directory rozhraní API"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ddcd9ae0-f6b7-4f13-a5e1-6cbf51a25634
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 8123f308a291503f2c61ac5de26696806c6402ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a>Přihlašovací aktivity sestav Azure Active Directory referenční dokumentace rozhraní API
Toto téma je součástí kolekce témat o hello Azure Active Directory reporting rozhraní API.  
Generování sestav služby Azure AD poskytuje rozhraní API, které vám umožní tooaccess přihlašovací aktivita sestavu dat pomocí kódu nebo související nástroje.
Hello obor tohoto tématu je tooprovide vám referenční informace o hello **API sestavy aktivity přihlášení**.

Přejděte na téma:

* [Přihlašovací aktivity](active-directory-reporting-azure-portal.md#activity-reports) další koncepční informace
* [Začínáme s Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md) Další informace o hello reporting rozhraní API.


## <a name="who-can-access-hello-api-data"></a>Kdo může přistupovat k datům hello rozhraní API?
* Uživatelé a objekty služby v roli správce zabezpečení nebo zabezpečení čtečky hello
* Globální správci
* Jakékoli aplikaci, která má autorizaci tooaccess hello rozhraní API (autorizace služby app lze pouze na základě oprávnění globálního správce)

tooconfigure přístup aplikaci tooaccess zabezpečení rozhraní API jako jsou například události přihlášení, použijte hello následující aplikace hello tooadd prostředí PowerShell objektu služby do role zabezpečení čtečky hello

```PowerShell
Connect-MsolService
$servicePrincipal = Get-MsolServicePrincipal -AppPrincipalId "<app client id>"
$role = Get-MsolRole | ? Name -eq "Security Reader"
Add-MsolRoleMember -RoleObjectId $role.ObjectId -RoleMemberType ServicePrincipal -RoleMemberObjectId $servicePrincipal.ObjectId
```

## <a name="prerequisites"></a>Požadavky
tooaccess to sestavy prostřednictvím hello reporting rozhraní API, musíte mít:

* [Edici Azure Active Directory Premium P1 a P2](active-directory-editions.md)
* Dokončené hello [požadavky tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md). 

## <a name="accessing-hello-api"></a>Přístup k hello API
Toto rozhraní API můžete buď přistupovat prostřednictvím hello [grafu Explorer](https://graphexplorer2.cloudapp.net) nebo prostřednictvím kódu programu, například pomocí prostředí PowerShell. V pořadí pro prostředí PowerShell toocorrectly interpretovat se syntaxí filtru OData hello používá při voláních REST grafu AAD, je nutné použít hello backtick (neboli: čárka) znak příliš "znaku" hello $. Hello backtick znak slouží jako [Powershellu řídicí znak](https://technet.microsoft.com/library/hh847755.aspx), povolení prostředí PowerShell toodo literálu výklad hello znak $ a zabránit složitá jako název proměnné prostředí PowerShell (ie: $filter).

hello grafu Explorer je aktivní Hello tohoto tématu. V příkladu prostředí PowerShell najdete [skript prostředí PowerShell](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).

## <a name="api-endpoint"></a>Koncový bod rozhraní API
Toto rozhraní API pomocí hello následující základní identifikátor URI se můžete dostat:  

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



Z důvodu toohello objem dat toto rozhraní API může mít jeden milión vrácené záznamy. 

Toto volání se vrátí hello data v dávkách. Má každé dávky nesmí být delší než 1 000 záznamů.  
hello další dávku tooget záznamů, použijte odkaz Další hello. Získat hello [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) informace z první sady hello vrácené záznamy. token přeskočit Hello bude na konci hello hello sadu výsledků dotazu.  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a>Podporované filtry
Můžete zúžit hello počet záznamů, které se vrátí pomocí rozhraní API volat v podobě filtru.  
Pro přihlášení rozhraní API související data, hello následující filtry jsou podporovány:

* **$top =\<počet toobe záznamů vrácených\>**  -toolimit hello počet vrácených záznamů. Toto je náročná operace. Pokud chcete, aby tooreturn tisíc objektů, které byste neměli používat tento filtr.  
* **$filter =\<údajů filtru\>**  -toospecify na základě hello podporovaný filtr polí, hello typ záznamy, na kterých vám nejvíc záleží

## <a name="supported-filter-fields-and-operators"></a>Pole podporovaný filtr a operátory
toospecify hello typu záznamů, které se zajímáte o, můžete vytvořit filtr příkaz, který může obsahovat jedno nebo kombinaci hello následující pole filtru:

* [signinDateTime](#signindatetime) -definuje datum nebo rozsah dat
* [ID uživatele](#userid) -definuje ID uživatele konkrétního uživatele na základě hello.
* [userPrincipalName](#userprincipalname) -definuje uživatele hello konkrétního uživatele na základě hlavní název uživatele (UPN)
* [appId](#appid) -definuje ID aplikace konkrétní aplikaci na základě hello
* [appDisplayName](#appdisplayname) -definuje aplikace hello konkrétní aplikaci na základě zobrazovaný název
* [loginStatus](#loginStatus) -definuje hello stav přihlášení hello (úspěch nebo chyba)

> [!NOTE]
> Při používání nástroje Průzkumník grafu, třeba můžete hello toouse hello správný, že je vaše pole Filtr případu pro každý písmeno.
> 
> 

toonarrow dolů hello oboru hello vrátil data, můžete vytvořit kombinace hello podporované filtry a pole filtru. Například hello následující příkaz vrátí hello top 10 záznamy mezi 1. července 2016 a července 2016 6.:

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


- - -
### <a name="signindatetime"></a>signinDateTime
**Podporované operátory**: eq, ge, le, gt, lt

**Příklad**:

Pomocí konkrétní datum

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z    



Použití rozsahu dat.    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


**Poznámky k**:

Parametr Hello data a času musí být ve formátu UTC hello 

- - -
### <a name="userid"></a>ID uživatele
**Podporované operátory**: eq

**Příklad**:

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

**Poznámky k**:

Hello hodnota userId je řetězcová hodnota

- - -
### <a name="userprincipalname"></a>UserPrincipalName
**Podporované operátory**: eq

**Příklad**:

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


**Poznámky k**:

Hodnota Hello userPrincipalName je řetězcová hodnota

- - -
### <a name="appid"></a>appId
**Podporované operátory**: eq

**Příklad**:

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



**Poznámky k**:

Hodnota Hello appId je řetězcová hodnota

- - -
### <a name="appdisplayname"></a>appDisplayName
**Podporované operátory**: eq

**Příklad**:

    $filter=appDisplayName+eq+'Azure+Portal' 


**Poznámky k**:

Hodnota Hello appDisplayName je řetězcová hodnota

- - -
### <a name="loginstatus"></a>loginStatus
**Podporované operátory**: eq

**Příklad**:

    $filter=loginStatus+eq+'1'  


**Poznámky k**:

Existují dvě možnosti pro hello loginStatus: 0 - Úspěch, 1 – Chyba

- - -
## <a name="next-steps"></a>Další kroky
* Chcete pro filtrovaný přihlašovací aktivity toosee příklady? Podívejte se na hello [ukázky sestavy rozhraní API služby Azure Active Directory přihlašovací aktivita](active-directory-reporting-api-sign-in-activity-samples.md).
* Chcete, aby tooknow Další informace o vytváření sestav API hello Azure AD? V tématu [Začínáme s Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md).

