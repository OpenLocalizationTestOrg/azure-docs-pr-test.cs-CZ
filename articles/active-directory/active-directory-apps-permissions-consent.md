---
title: "Aplikace, oprávnění a vyjádření souhlasu v Azure Active Directory | Dokumentace Microsoftu"
description: "Azure AD Connect integruje vaše místní adresáře do služby Azure Active Directory. To vám umožní tooprovide společnou identitu pro aplikace Office 365, Azure a SaaS integrované s Azure AD."
keywords: "tooAzure Úvod AD, aplikace, co je Azure AD Connect, nainstalovat službu active directory"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 25897cc4-7687-49b6-b0d5-71f51302b6b1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/31/2017
ms.author: billmath
ms.reviewer: jesakowi
ms.custom: oldportal;it-pro;
ms.openlocfilehash: af0c2669199736fdb41e85876a7e3a7064e80770
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="apps-permissions-and-consent-in-azure-active-directory"></a>Aplikace, oprávnění a vyjádření souhlasu v Azure Active Directory
V rámci Azure Active Directory můžete přidat adresář tooyour aplikace.  aplikace Hello se může lišit v závislosti na typu hello aplikace.  tooview aplikace hello portálu classic, vyberte adresář a vyberte aplikace.

![](media/active-directory-apps-permissions-consent/apps1.png)

> [!IMPORTANT]
> Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku.

## <a name="types-of-apps"></a>Typy aplikací

1. **Aplikace s jedním tenantem** </br>
    - **Jednoho klienta aplikace** -často označuje tooas-obchodní (LOB) aplikace. Toto je hello případ, kdy někdo v rámci vaší organizace sama vyvinula vlastní aplikaci a mají uživatelé v hello organizace toobe možné toosign v toohello aplikaci.
    ![](media/active-directory-apps-permissions-consent/apps2.png)
    - **Proxy aplikace aplikace** – Pokud vystavit místní aplikace s Proxy aplikace Azure AD, aplikace na jednoho klienta je zaregistrován ve vašem klientovi (v toohello přidání Proxy aplikace služby). Taková aplikace reprezentuje vaši místní aplikaci pro veškeré cloudové interakce (například ověřování). (App Proxy vyžaduje Azure AD Basic nebo vyšší.)


2. **Aplikace s více tenanty**
    - **Víceklientské aplikace, které můžete s ostatními souhlasit** – podobné příliš "jednoho klienta aplikace, které vaše organizace sama vyvinula". Hlavní rozdíl Hello (kromě hello logiku hello aplikace) je uživatelé z jiných klientů můžete také souhlas tooand přihlášení toohello aplikace.</br>
    ![](media/active-directory-apps-permissions-consent/apps4.png)
    - **Aplikace s více tenanty vyvíjené ostatními, se kterými může Contoso vyjádřit souhlas**. (zkráceně odsouhlasené aplikace) Toto je straně překlopit hello "víceklientské aplikací, které vaše organizace sama vyvinula". Když jiné organizaci sama vyvinula víceklientské aplikace, můžete uživatele organizaci souhlas toohello aplikace a přihlaste tooit.
    - **Vlastní aplikace Microsoftu** – Aplikace, které představují služby Microsoftu. Souhlas doprovází hello skutečnost, že registrace pro službu hello. Je někdy speciální UX a logiku pro určité aplikace první strany, která se často používá při vytváření zásady kolem toohello aplikace access.</br>
    ![](media/active-directory-apps-permissions-consent/apps3.png)
    - **Předběžně integrované aplikace** – aplikace, které jsou k dispozici v hello Azure AD aplikace Galerie, které můžete přidat tooyour directory tooprovide jedním přihlašování (a v některých případech zřizování) toopopular SaaS aplikace.
    - **Jednotné přihlašování Azure AD**: Opravdové jednotné přihlašování pro aplikace, které lze integrovat s Azure AD, prostřednictvím podporovaného přihlašovacího protokolu, jako je SAML 2.0 nebo OpenID Connect. Hello Průvodce vás provede procesem jeho nastavení.
    - **Heslo jednotné přihlašování**: Azure AD bezpečně uloží hello uživatelská pověření pro aplikace hello a hello přihlašovací údaje jsou "vloženy" do hello přihlašovací formulář hello rozšíření prohlížeče přístup k aplikaci Azure AD. Označuje se také jako ukládání hesel do trezoru.

## <a name="permissions"></a>Oprávnění

Při registraci aplikace hello uživatele provedení registrace aplikace hello (tedy hello vývojáře) definuje, které aplikace hello oprávnění potřebuje přístup k a prostředky, ke kterým. (hello prostředky jsou, sami definován jako ostatní aplikace). Například někdo vytváření aplikaci čtečky pomocí e-mailu, by stavu, že jejich aplikace vyžaduje oprávnění hello "Přístup k poštovním schránkám jako hello přihlášeného uživatele" hello "Office 365 Exchange Online" prostředků:
    
![](media/active-directory-apps-permissions-consent/apps6.png)

V pořadí pro jednu aplikaci (klient hello) toorequest oprávnění z jiné aplikace (hello prostředků) definuje hello vývojáře hello prostředků aplikace hello oprávnění, která neexistuje. V našem příkladu společnosti Microsoft, vlastníka hello prostředků aplikace "Office 365 Exchange Online" hello definovali oprávnění s názvem "Přístup k poštovním schránkám jako hello přihlášeného uživatele".

Při definování oprávnění, vývojáři aplikace hello musí definovat, pokud může být souhlas hello oprávnění, nebo jestli vyžaduje souhlas správce. To umožňuje vývojářům tooconsent tooallow uživatelé na svých vlastních tooapps vyžaduje pouze oprávnění nízká citlivosti, ale vyžadují admins tooconsent toomore citlivé oprávnění. Například hello "Azure Active Directory" prostředků aplikace, byla definována, takže uživatelé mohou souhlas tooapps, požaduje omezenými oprávněními jen pro čtení.  Pro úplná oprávnění ke čtení a veškerá oprávnění k zápisu však vyžaduje souhlas správce.

Vzhledem k tomu, že se nativní klienti neověřují, aplikace definovaná jako nativní klientská aplikace může požadovat pouze delegovaná oprávnění. To znamená, že získání tokenu se musí vždy účastnit i skutečný uživatel. Webové aplikace a webová rozhraní API (důvěrní klienti) se musí při každém získání přístupového tokenu ověřovat pomocí Azure AD. Znamená, mají možnost hello požadovat oprávnění jen pro aplikace. Například jedna služba back-end potřebuje tooauthenticate tooanother back-end službu. Aplikace vyžadující oprávnění jen pro aplikace vždy vyžadují souhlas správce.

Shrňme si to:



- Aplikace (klient) stavy hello oprávnění, která potřebuje pro jiné aplikace (prostředky).
- Aplikace (prostředků) určují, jaká oprávnění jsou zveřejněné tooother aplikace (klientů).
- Oprávnění může být jen pro aplikace nebo delegované.
- Delegované oprávnění může být označeno jako „umožňuje souhlas uživatele“ nebo „vyžaduje souhlas správce“.
- Aplikace se může chovat jako klient (deklarováním potřebuje oprávnění tooa prostředku), jako prostředek (deklarováním oprávnění, která zpřístupňuje) nebo obě.

## <a name="controls"></a>Ovládací prvky

Hello následuje seznam hello jiný správce kontrolních mechanismů pro toto chování. Dobrý den, správce, které jsou přístupné ovládací prvky portálu classic hello z konfigurace v adresáři hello.

![](media/active-directory-apps-permissions-consent/apps7.png)

V hello Azure portálu pod **spravovat**, **uživatelská nastavení**.

![](media/active-directory-apps-permissions-consent/apps11.png)



- Můžete řídit, jestli uživatelé mohou souhlas tooapps:

Na portálu classic hello vyberte **uživatelé mohou poskytnout tooaccess oprávnění aplikací svá data.**
![](media/active-directory-apps-permissions-consent/apps8.png)

V hello portálu Azure, vyberte **uživatelé můžou aplikace tooaccess svá data**.
![](media/active-directory-apps-permissions-consent/apps12.png)



- Můžete řídit, jestli uživatelé mohou registrovat své vlastní obchodní aplikace jednoho klienta: V hello klasického portálu vyberte **uživatelé mohou přidat integrovaných aplikací.**
![](media/active-directory-apps-permissions-consent/apps9.png)

V hello portálu Azure, vyberte **uživatelé můžou aplikace tooaccess svá data**.
![](media/active-directory-apps-permissions-consent/apps13.png)

>[!NOTE]
>I v případě, že uživatelům umožnit tooregister jednoho klienta obchodní aplikace, existují omezení, které lze registrovat toowhat.  
>Například vývojáři, kteří nejsou správci adresáře.
>
>- Uživatelé nemohou udělat z aplikace pro jednoho tenanta aplikaci pro více tenantů.
>- Při registraci aplikace LOB jednoho klienta, uživatelé požádat o oprávnění jen aplikace tooother aplikace.
>- Při registraci aplikace LOB jednoho klienta, uživatelé nemůže požádat o aplikace tooother delegovaných oprávnění, pokud tato oprávnění vyžadovat souhlas správce.
>- Mohou uživatelé provádět tooapps změny, které nejsou vlastníky.



- Můžete řídit, jestli sami uživatelé mohou přidávat předem integrované aplikace, které používají jednotné přihlašování pomocí hesla (neboli ukládání hesel do trezoru).![](media/active-directory-apps-permissions-consent/apps10.png)



- Můžete řídit, za jakých podmínek je aplikace přístupná (tj. podmíněný přístup). Mějte na paměti, že platí toohello klientská aplikace i toohello prostředků aplikace. Ano stát, že nastavit zásady podmíněného přístupu s upozorněním, že tuto aplikaci "Office 365 Exchange Online" hello lze přistupovat pouze z počítačů, které splňují předpisy.  Tato zásada se nové i když se uživatel pokusí toouse klientské aplikace, který vyžaduje oprávnění tooExchange Online.



- Máte přehled, do kterého byla aplikace svolení tooand ty, které jsou používány.

1.  Když uživatel souhlasí tooan aplikace, vytvoří se objekt ServicePrincipal v klientovi hello. Vytvoření ServicePrincipal je součástí sestava auditu hello.
2.  Sestavy aktivit přihlášení uživatele zjistíte, které aplikace hello uživatele je přihlášení k. 

## <a name="example"></a>Příklad

Jako příklad Podívejme aplikaci "FabrikamMail pro Office 365" hello, která jste si všimli, že uživatelé ve vašem klientovi se přihlašujete k. FabrikamMail je aplikace pro čtení pošty pro Android vytvořená společností Fabrikam, Inc. To, které patří do hello "víceklientské aplikace jiné vývoj, které můžete souhlas Contoso".

Pokud mají uživatelé povoleno tooconsent, získají vyzvat hello souhlasu při prvním přihlášení:![](media/active-directory-apps-permissions-consent/apps14.png)

"Přístup k vaší poštovní schránky" je hello uživatelsky orientovaný souhlasu řetězec oprávnění "Přístup k poštovním schránkám jako hello přihlášeného uživatele" hello "Office 365 Exchange Online" (Exchange) vystavené.

Uvidíte hello oprávnění vyhledáním hello ServicePrincipal objekt pro Exchange (hello prostředků), která byla přidána při registraci pro Office 365. Objekt ServicePrincipal hello "instance" hello aplikace si můžete představit ve vašem klientovi, který se používá k zaznamenání různé možnosti a konfigurace.  Můžete to vidět pomocí hello `Get-AzureADServicePrincipal` v prostředí PowerShell.

    PS C:\> Get-AzureADServicePrincipal -ObjectId 383f7b97-6754-4d3d-9474-3908ebcba1c6 | fl *
    
    DeletionTimeStamp         : 
    ObjectId                  : 383f7b97-6754-4d3d-9474-3908ebcba1c6
    ObjectType                : ServicePrincipal
    AccountEnabled            : True
    AppDisplayName            : Office 365 Exchange Online
    AppId                     : 00000002-0000-0ff1-ce00-000000000000
    AppOwnerTenantId          : 
    AppRoleAssignmentRequired : False
    AppRoles                  : {...}
    DisplayName               : Microsoft.Exchange
    ErrorUrl                  : 
    Homepage                  : 
    KeyCredentials            : {}
    LogoutUrl                 : 
    Oauth2Permissions         : {...
                                , class OAuth2Permission {
                                  AdminConsentDescription : Allows hello app toohave hello same access toomailboxes as hello signed-in user via Exchange Web Services.
                                  AdminConsentDisplayName : Access mailboxes as hello signed-in user via Exchange Web Services
                                  Id                      : 3b5f3d61-589b-4a3c-a359-5dd4b5ee5bd5
                                  IsEnabled               : True
                                  Type                    : User
                                  UserConsentDescription  : Allows hello app full access tooyour mailboxes on your behalf.
                                  UserConsentDisplayName  : Access your mailboxes
                                  Value                   : full_access_as_user
                                },
                                ...}
    PasswordCredentials       : {}
    PublisherName             : 
    ReplyUrl                  : 
    SamlMetadataUrl           : 
    ServicePrincipalNames     : {00000002-0000-0ff1-ce00-000000000000/outlook.office365.com, 00000002-0000-0ff1-ce00-000000000000/mail.office365.com, 00000002-0000-0ff1-ce00-000000000000/outlook.com, 
                                00000002-0000-0ff1-ce00-000000000000/*.outlook.com...}
    Tags                      : {}

Souhlas je vyvoláno hello kliknutí na tlačítko "Přijmout". Nejprve je vytvořen objekt ServicePrincipal pro "FabrikamMail pro Office 365" v klientovi hello. Hello ServicePrincipal vypadá přibližně takto:

    PS C:\> Get-AzureADServicePrincipal -SearchString "FabrikamMail for Office 365" | fl *
    
    DeletionTimeStamp         : 
    ObjectId                  : a8b16333-851d-42e8-acd2-eac155849b37
    ObjectType                : ServicePrincipal
    AccountEnabled            : True
    AppDisplayName            : FabrikamMail for Office 365
    AppId                     : aba7c072-2267-4031-8960-e7a2db6e0590
    AppOwnerTenantId          : 4a4076e0-a70f-41c6-b819-6f9c4a86df89
    AppRoleAssignmentRequired : False
    AppRoles                  : {}
    DisplayName               : FabrikamMail for Office 365
    ErrorUrl                  : 
    Homepage                  : 
    KeyCredentials            : {}
    LogoutUrl                 : 
    Oauth2Permissions         : {}
    PasswordCredentials       : {}
    PublisherName             : Fabrikam, Inc.
    ReplyUrl                  : 
    SamlMetadataUrl           : 
    ServicePrincipalNames     : {aba7c072-2267-4031-8960-e7a2db6e0590}
    Tags                      : {WindowsAzureActiveDirectoryIntegratedApp}

Souhlas tooan aplikace vytvoří odkaz Oauth2PermissionGrant mezi hello následující:
  
- objekt uživatele Hello
- klientské aplikace Hello ServicePrincipalName (SPN)
- Hello prostředků aplikace ServicePrincipalName (SPN)
- oprávnění v hello prostředků aplikace.  

V případě hello FabrikamMail vypadá přibližně takto:

    PS C:\> Get-AzureADUserOAuth2PermissionGrant -ObjectId ddiggle@aadpremiumlab.onmicrosoft.com | fl *
    
    ClientId    : a8b16333-851d-42e8-acd2-eac155849b37
    ConsentType : Principal
    ExpiryTime  : 05/15/2017 07:02:39 AM
    ObjectId    : M2OxqB2F6EKs0urBVYSbN5d7PzhUZz1NlH25COvLocbJjoxkUFfRQauryBKwBWet
    PrincipalId : 648c8ec9-5750-41d1-abab-c812b00567ad
    ResourceId  : 383f7b97-6754-4d3d-9474-3908ebcba1c6
    Scope       : full_access_as_user
    StartTime   : 01/01/0001 12:00:00 AM

(**ClientId** je ID objektu zabezpečení služby je FabrikamMail (hello, ten, který právě nebyl vytvořen), **PrincipalId** je ID objektu uživatelské hello (of hello uživatel, který dá souhlas), **ResourceId**je služba Exchange pro ID objektu zabezpečení, rozsahu je hello oprávnění v systému Exchange, které se souhlas).

Pokud uživatelé nejsou povoleny tooconsent, zobrazí se obrazovky s upozorněním, že oprávnění je vyžadováno.

