---
title: "aaaAzure AD v2 ASP.NET Web Server Začínáme - Test | Microsoft Docs"
description: "Implementace přihlašování společnosti Microsoft na řešení technologie ASP.NET s tradiční webovou aplikací využívajících prohlížeč pomocí OpenID Connect standard"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 99c7525b9146605142180962fc2a61b3c953c064
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a>Otestujte svůj kód

Stiskněte klávesu `F5` toorun projektu v sadě Visual Studio. Hello prohlížeč a směrovat je příliš*http://localhost: {port}* kde uvidíte hello *přihlásit pomocí Microsoft* tlačítko. Pokračujte a klikněte na něj toosign v.

Až budete připravené tootest, použijte pracovní nebo školní (Azure Active Directory) nebo osobní (live.com, outlook.com) toosign v účtu. 

![Přihlaste se pomocí okna prohlížeče Microsoft](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin.png)

![Přihlaste se pomocí okna prohlížeče Microsoft](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin2.png)

#### <a name="expected-results"></a>Očekávané výsledky
Po přihlášení uživatel hello je přesměrovaného toohello domovskou stránku vašeho webu, který je hello adresy URL HTTPS zadaná v informace o registraci aplikace v portálu pro registraci aplikace Microsoft hello. Tato stránka se teď zobrazuje *Hello {uživatele}* a na odkaz toosign na více systémů a odkaz toosee hello deklarace identity uživatele – které je řadič odkaz toohello autorizovat vytvořili dříve.

### <a name="see-users-claims"></a>V tématu deklarací identity uživatele
Vyberte hello hyperlink toosee hello deklarací identity uživatele. To vás toohello řadiče a zobrazení, která je k dispozici toousers, který se ověřují jenom.

#### <a name="expected-results"></a>Očekávané výsledky
 Měli byste vidět tabulku obsahující základní vlastnosti hello hello přihlášení uživatele:

| Vlastnost | Hodnota | Popis|
|---|---|---|
| Name (Název) | {Úplné uživatelské jméno} | Hello uživatelské jméno a příjmení
|Uživatelské jméno | <span>user@domain.com</span>| uživatelské jméno Hello používá tooidentify hello přihlášení uživatele
| Předmět| {Subjektu}|Řetězec toouniquely identifikovat hello přihlášení uživatele na hello web|
| ID tenanta| {Guid}| A *guid* toouniquely představují hello uživatele organizaci Azure Active Directory.|

Kromě toho se zobrazí tabulku včetně všech deklarací identity uživatele zahrnuta v žádosti o ověření. Seznam všech deklarací identity do Id tokenu a vysvětlení najdete v tématu to [článku](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "seznam deklarací identity v Id tokenu").


### <a name="test-accessing-a-method-that-has-an-authorize-attribute-optional"></a>Test přístupu k metodě, která má *[Authorize]* atribut (volitelné)
V tomto kroku budete testovat přístupem řadiče hello ověřený jako anonymní uživatel:<br/>
Vyberte hello propojit na více systémů toosign hello uživatele a dokončení hello proces přihlášení.<br/>
Teď v prohlížeči zadejte http://localhost: {port} / ověření tooaccess řadiči, která je chráněná pomocí hello `[Authorize]` atribut

#### <a name="expected-results"></a>Očekávané výsledky
Měli byste obdržet hello řádku, které vyžadují tooauthenticate toosee hello zobrazení.

## <a name="additional-information"></a>Další informace

<!--start-collapse-->
### <a name="protect-your-entire-web-site"></a>Chránit celý web
tooprotect celý web, přidejte hello `AuthorizeAttribute` příliš`GlobalFilters` v `Global.asax` `Application_Start` metoda:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

<div></div>
<br/>

> [!NOTE]
> **Jak uživatelé toorestrict z toosign pouze jedna organizace v aplikaci tooyour**

> Ve výchozím nastavení, osobní účty (včetně outlook.com, live.com a dalších), jakož i pracovním a školním účtům v jakémkoli společnosti nebo organizace, která má integrované s Azure Active Directory můžete přihlásit tooyour aplikace. 

> Pokud chcete, aby vaše aplikace tooaccept přihlášení z pouze jedna organizace Azure Active Directory, nahraďte hello `Tenant` parametr v *web.config* z `Common` název klienta toohello hello organizace – například *contoso.onmicrosoft.com*. Potom změnit hello `ValidateIssuer` argument ve vaší *třídy pro spuštění OWIN* příliš`true`.

> nastavit tooallow uživatele ze seznamu pouze konkrétní organizací `ValidateIssuer` tootrue a používání hello `ValidIssuers` parametr toospecify seznam organizace.

> Další možností je vlastní metoda tooimplement vystavitelů hello toovalidate pomocí parametru IssuerValidator. Další informace o `TokenValidationParameters`, najdete v tématu [to](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "článku na webu MSDN parametry tokenvalidationparameters") článku na webu MSDN.

