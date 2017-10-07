---
title: Azure Active Directory Application Manifest hello aaaUnderstanding | Microsoft Docs
description: "Podrobné pokrytí manifest aplikace Azure Active Directory hello, která představuje konfiguraci identity aplikace v klientovi služby Azure AD a je použité toofacilitate autorizace OAuth, souhlasu prostředí a další."
services: active-directory
documentationcenter: 
author: sureshja
manager: mbaldwin
editor: 
ms.assetid: 4804f3d4-0ff1-4280-b663-f8f10d54d184
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: sureshja
ms.custom: aaddev
ms.reviewer: elisol
ms.openlocfilehash: 096c9d5501edbfc08731fea670cee559d4442ad1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-hello-azure-active-directory-application-manifest"></a>Principy hello manifest aplikace Azure Active Directory
Aplikace, které se integrují s Azure Active Directory (AD) musí být zaregistrován u klienta služby Azure AD, poskytuje konfiguraci trvalou identitu pro aplikace hello. Tato konfigurace je konzultaci za běhu, povolení scénáře, které umožňují toooutsource a zprostředkovatele ověřování/autorizace aplikací prostřednictvím služby Azure AD. Další informace o modelu aplikace hello Azure AD najdete v tématu hello [přidání, aktualizace a odebrání aplikace] [ ADD-UPD-RMV-APP] článku.

## <a name="updating-an-applications-identity-configuration"></a>Aktualizuje se konfigurace identity aplikace
Nejsou k dispozici pro aktualizaci hello vlastnosti konfigurace identity aplikace, které se liší v možnosti a stupňů potíže, včetně následujících hello ve skutečnosti více možnosti:

* Hello  **[portál Azure] [ AZURE-PORTAL] webové uživatelské rozhraní** vám umožní tooupdate hello nejběžnější vlastností aplikace. Toto je hello nejrychlejší a minimálně chyba náchylné k chybám způsob aktualizace vlastností vaší aplikace, ale nemá získáte tak úplný přístup tooall vlastnosti, jako jsou hello následující dvě metody.
* Pro pokročilejší scénáře, kde je nutné tooupdate vlastnosti, které nejsou vystaveny v hello portál Azure classic, můžete upravit hello **manifest aplikace**. To je hello zaměřením tohoto článku a je podrobněji popsána spouštění v další části hello.
* Je také možné příliš**zápisu aplikace, která používá hello [rozhraní Graph API] [ GRAPH-API]**  tooupdate vaší aplikace, které vyžaduje hello většina úsilí. Může se jednat atraktivní možnosti, když, pokud jsou zápis software pro správu, nebo potřebujete tooupdate vlastnosti aplikace v pravidelných intervalech automatizovaně.

## <a name="using-hello-application-manifest-tooupdate-an-applications-identity-configuration"></a>Pomocí tooupdate manifestu aplikace hello konfigurace identity aplikace
Prostřednictvím hello [portál Azure][AZURE-PORTAL], konfigurace identity aplikace můžete spravovat aktualizace pomocí editoru manifestu vložené hello manifest aplikace hello. Můžete také stáhnout a nahrát hello manifest aplikace jako soubor ve formátu JSON. Žádný skutečný soubor je uložen v adresáři hello. Hello manifest aplikace je jenom metody GET protokolu HTTP operace u entity aplikace hello Azure AD Graph API a hello se nahrávají na entity aplikace hello operace HTTP PATCH.

V důsledku toho ve formátu hello toounderstand pořadí a vlastnosti hello manifest aplikace, budete potřebovat tooreference hello rozhraní Graph API [aplikace entity] [ APPLICATION-ENTITY] dokumentaci. Příklady aktualizace, které lze provést, i když nahrávání manifest aplikace:

* **Deklarovat obory oprávnění (oauth2Permissions)** vystavené webového rozhraní API. Naleznete v tématu "Exposing webovým rozhraním API tooOther aplikace" hello v [integrace aplikací s Azure Active Directory] [ INTEGRATING-APPLICATIONS-AAD] informace o implementaci vystupovat jako jiný uživatel pomocí hello oauth2Permissions delegovaná oprávnění oboru. Jak je uvedeno nahoře, vlastností entity aplikací jsou popsané v hello rozhraní Graph API [Entity a komplexní typ] [ APPLICATION-ENTITY] článku, včetně hello oauth2Permissions vlastnost, která je kolekce typu [OAuth2Permission][APPLICATION-ENTITY-OAUTH2-PERMISSION].
* **Deklarovat aplikační role (appRoles) vystavené aplikace**. Hello vlastnost appRoles aplikace entity je kolekce typu [aplikační role][APPLICATION-ENTITY-APP-ROLE]. V tématu hello [řízení přístupu v cloudových aplikacích pomocí služby Azure AD na základě Role] [ RBAC-CLOUD-APPS-AZUREAD] článku příklad implementace.
* **Deklarovat známé klienta aplikace (knownClientApplications)**, které umožňují toologically vazbě hello souhlasu hello zadaný klienta aplikace toohello prostředků nebo webové rozhraní API.
* **Žádosti o členství ve skupinách Azure AD tooissue deklarace** pro hello přihlášení uživatele (groupMembershipClaims).  To může být také nakonfigurované tooissue deklarace identity o členství role hello uživatele v adresáři. V tématu hello [autorizace v cloudových aplikací pomocí skupiny AD] [ AAD-GROUPS-FOR-AUTHORIZATION] článku příklad implementace.
* **Vaše aplikace toosupport OAuth 2.0 implicitní udělení povolit** toků (oauth2AllowImplicitFlow). Tento typ udělení toku se používá s embedded webové stránky JavaScript nebo jedné stránky aplikace (SPA). Další informace o udělení hello implicitní autorizace, najdete v části [Principy hello OAuth2 implicitní tok v Azure Active Directory poskytování][IMPLICIT-GRANT].
* **Povolit použití X509 certifikáty jako tajný klíč hello** (keyCredentials). V tématu hello [vytvářet aplikace, služby a démon v Office 365] [ O365-SERVICE-DAEMON-APPS] a [tooauth Příručka pro vývojáře s rozhraním API pro Azure Resource Manager] [ DEV-GUIDE-TO-AUTH-WITH-ARM] články pro příklady implementace.
* **Přidat nový identifikátor ID URI aplikace** pro vaši aplikaci (identifierURIs[]). ID aplikace identifikátory URI používají toouniquely identifikaci aplikace v rámci jeho klienta Azure AD (nebo napříč více klientů Azure AD, pro víc klientů scénáře při kvalifikovaný prostřednictvím ověřené vlastní domény). Používají se při požadování oprávnění tooa prostředků aplikace, nebo získání tokenu přístupu pro aplikaci prostředků. Při aktualizaci tohoto elementu hello stejnou aktualizaci se provádí kolekce [] servicePrincipalNames toohello odpovídající instančního objektu, který je umístěn v domácí klienta aplikace hello.

Hello manifest aplikace také poskytuje vhodný způsob tootrack hello stav registrace vaší aplikace. Protože je k dispozici ve formátu JSON, lze do zdrojového kódu, společně s zdrojový kód aplikace zkontrolovat hello soubor, který představuje.

## <a name="step-by-step-example"></a>Krok příklad krok
Teď umožňuje provede požadované tooupdate hello kroky konfigurace identity aplikace prostřednictvím hello manifest aplikace. Zvýrazníme jeden z předchozích příkladech hello znázorňující, jak toodeclare nové oprávnění obor na prostředek aplikace:

1. Přihlaste se toohello [portál Azure][AZURE-PORTAL].
2. Po došlo k ověření, zvolte výběrem z hello pravém horním rohu stránky hello klientovi Azure AD.
3. Vyberte **Azure Active Directory** rozšíření z hello na vlevo navigačního panelu a klikněte na tlačítko **registrace aplikace**.
4. Najít aplikace hello tooupdate hello seznamu a klikněte na na něm.
5. Na stránce aplikace hello, klikněte na tlačítko **Manifest** tooopen hello vložené manifestu editor. 
6. Lze přímo upravit hello manifest pomocí tohoto editoru. Všimněte si, že manifest hello dodržuje hello schéma pro hello [aplikace entity] [ APPLICATION-ENTITY] jak jsme už zmínili dřív: například za předpokladu, že chceme tooimplement/zveřejněte nové oprávnění s názvem "Employees.Read.All" v našem prostředků aplikace (API) by kolekce oauth2Permissions toohello element nové za sekundu, přidejte jednoduše ie:
   
        "oauth2Permissions": [
        {
        "adminConsentDescription": "Allow hello application tooaccess MyWebApplication on behalf of hello signed-in user.",
        "adminConsentDisplayName": "Access MyWebApplication",
        "id": "aade5b35-ea3e-481c-b38d-cba4c78682a0",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow hello application tooaccess MyWebApplication on your behalf.",
        "userConsentDisplayName": "Access MyWebApplication",
        "value": "user_impersonation"
        },
        {
        "adminConsentDescription": "Allow hello application toohave read-only access tooall Employee data.",
        "adminConsentDisplayName": "Read-only access tooEmployee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow hello application toohave read-only access tooyour Employee data.",
        "userConsentDisplayName": "Read-only access tooyour Employee records",
        "value": "Employees.Read.All"
        }
        ],
   
    Hello položka musí být jedinečný, a proto musíte vygenerovat nový globálně jedinečné ID (GUID) pro hello `"id"` vlastnost. V takovém případě vzhledem k tomu, že jsme zadali `"type": "User"`, toto oprávnění může být svolení tooby libovolný účet ověřit hello klienta Azure AD, ve které hello je zaregistrovaný aplikace prostředků nebo rozhraní API. Tato oprávnění tooaccess uděluje hello klienta aplikace se jménem hello účtu. Hello popis a zobrazovaný název řetězce se používají během souhlasu a pro zobrazení v hello portálu Azure.
6. Po dokončení aktualizace hello manifest, klikněte na tlačítko **Uložit** toosave hello manifestu.  
   
Teď, když hello manifest je uložen, můžete udělit registrovaného klienta aplikace přístup toohello nové oprávnění, které jsme přidali výše. Tento čas, které můžete použít hello portál Azure webového uživatelského rozhraní neupravujte manifest aplikace hello klienta:  

1. Nejprve přejděte toohello **nastavení** okno hello klienta aplikace toowhich chcete tooadd přístup toohello nové rozhraní API, klikněte na tlačítko **požadovaných oprávnění** a zvolte **vybrat rozhraní API** .
2. Potom zobrazí s hello seznam registrovaných prostředků aplikace (API) v klientovi hello. Klikněte na tlačítko tooselect aplikace hello prostředků, nebo název typu hello hello aplikace hello vyhledávacího pole. Když jste najde hello aplikace, klikněte na tlačítko **vyberte**.  
3. Tím přejdete toohello **vyberte oprávnění** stránky, které se zobrazí seznam hello oprávnění aplikací a delegovaná oprávnění k dispozici pro hello prostředků aplikace. Vyberte hello nová položka oprávnění v pořadí tooadd ho toohello klienta požadované seznam oprávnění. Toto nové oprávnění se uloží v konfiguraci identity hello klientskou aplikaci v vlastnost kolekce "requiredResourceAccess" hello.


A je to. Teď vaše aplikace bude spuštěna s použitím jejich novou konfiguraci identity.

## <a name="next-steps"></a>Další kroky
* Další informace o hello vztah mezi objekty aplikace a služby hlavní aplikace v [aplikace a služby hlavní objekty ve službě Azure AD][AAD-APP-OBJECTS].
* V tématu hello [Glosář vývojáře Azure AD] [ AAD-DEVELOPER-GLOSSARY] definice některých koncepty pro vývojáře Azure Active Directory (AD) hello jádra.

Použijte sekci komentáře hello níže tooprovide zpětnou vazbu a Pomozte nám vylepšit a utvářejí náš obsah.

<!--article references -->
[AAD-APP-OBJECTS]: active-directory-application-objects.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-PORTAL]: https://portal.azure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]: active-directory-dev-understanding-oauth2-implicit-grant.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

