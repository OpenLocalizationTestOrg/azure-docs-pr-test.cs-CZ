---
title: "Integrace aplikací s Azure Active Directory | Microsoft Docs"
description: "Podrobnosti o tom, jak přidat, aktualizovat nebo odebrat aplikace v Azure Active Directory (Azure AD)."
services: active-directory
documentationcenter: 
author: lnalepa
manager: mbaldwin
editor: mbaldwin
ms.assetid: ae637be5-0b71-4b1e-b1fe-b83df3eb4845
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: lenalepa
ms.custom: aaddev
ms.reviewer: luleon
ms.openlocfilehash: 3be341bcb897a1481f145825429a1a94dfaae3b0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="integrating-applications-with-azure-active-directory"></a>Integrace aplikací s Azure Active Directory
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Podnikoví vývojáři a poskytovatelé SaaS (software jako služba) můžou vyvíjet komerční cloudové služby nebo podnikové aplikace s možností integrace s Azure Active Directory (AD) za účelem zajištění zabezpečeného přihlašování a autorizace pro služby. Při integraci s Azure AD k aplikaci nebo službě, musí vývojář nejprve zaregistrovat podrobnosti o jejich používání s Azure AD pomocí portálu Azure classic.

Tento článek ukazuje, jak přidat, aktualizovat nebo odebrat aplikace ve službě Azure AD. Se dozvíte o různých typech aplikací, které můžou být integrované se službou Azure AD, jak nakonfigurovat vaší aplikace pro přístup k jiným prostředkům, například webových rozhraní API a další.

Další informace o dva objekty Azure AD, které představují zaregistrovanou aplikaci a vztahů mezi nimi, najdete v části [objekty aplikací a hlavní objekty služeb](active-directory-application-objects.md); Další informace o značce pokynů byste měli používat při vývoji aplikací s Azure Active Directory, najdete v části [Branding pokyny pro integrované aplikace](active-directory-branding-guidelines.md).

## <a name="adding-an-application"></a>Přidání aplikace
Všechny aplikace, který chce využívat možnosti Azure AD musí být zaregistrován nejdřív v klientovi služby Azure AD. Zahrnuje tohoto procesu registrace poskytne Azure AD podrobnosti o aplikaci, jako je například adresa URL, kde je umístěn, adresu URL pro odeslání odpovědi po ověření uživatele je identifikátor URI, který identifikuje aplikaci a tak dále.

Pokud vytváříte webovou aplikaci, která právě musí podporovat přihlášení pro uživatele ve službě Azure AD, můžete jednoduše podle pokynů níže. Pokud vaše aplikace vyžaduje přihlašovací údaje nebo oprávnění pro přístup k webovému rozhraní API, nebo je potřeba povolit uživatelům z jiných klientů Azure AD k němu přístup, najdete v článku [aktualizaci aplikace](#updating-an-application) části a pokračujte v konfiguraci vaší aplikace.

### <a name="to-register-a-new-application-in-the-azure-portal"></a>Zaregistrujte novou aplikaci na portálu Azure
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Výběrem účtu v pravém horním rohu stránky zvolte klientovi Azure AD.
3. V levém navigačním podokně zvolte **více služeb**, klikněte na tlačítko **registrace aplikace**a klikněte na tlačítko **přidat**.
4. Postupujte podle zobrazených výzev a vytvořte novou aplikaci. Pokud chcete konkrétní příklady pro webové aplikace nebo nativních aplikací, podívejte se na naše [– elementy QuickStart](active-directory-developers-guide.md).
  * Pro webové aplikace, zadejte **přihlašovací adresa URL**, což je základní adresu URL aplikace, kde uživatelé se mohou přihlásit v např `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * U nativních aplikací, zadejte **identifikátor URI pro přesměrování**, které Azure AD se používá k vrácení odpovědi tokenu. Zadejte konkrétní hodnotu pro vaši aplikace, např. `http://MyFirstAADApp`.
5. Po dokončení registrace Azure AD přiřadí aplikace identifikátor jedinečných klientských ID aplikace. Aplikace byla přidána, a budete přesměrováni na stránku rychlý Start pro vaši aplikaci. V závislosti na tom, jestli je aplikace na web nebo nativní aplikace zobrazí se různé možnosti o tom, jak do aplikace přidat další možnosti. Po přidání aplikace, můžete začít aktualizace vaše aplikace umožňuje uživatelům přihlásit se, přístup k webové rozhraní API v ostatních aplikacích, nebo nakonfigurovat víceklientské aplikace (tím jiných organizací k přístup k aplikaci).

> [!NOTE]
> Ve výchozím nastavení je registrace nově vytvořené aplikace nakonfigurovat pro povolení uživatele z adresáře se přihlásit k aplikaci.
> 
> 

## <a name="updating-an-application"></a>Aktualizace aplikace
Jakmile aplikaci byl registrován u služby Azure AD, bude pravděpodobně nutné aktualizovat tak, aby poskytnout přístup k webovému rozhraní API, budou dostupné v jiných organizací a další. Tato část popisuje různé způsoby, ve kterých budete muset nakonfigurovat další aplikace. Nejprve spustíme přehled Framework souhlas, což je důležité si uvědomit, pokud vytváříte aplikace prostředků nebo rozhraní API, které budou využívat klientské aplikace vytvořené vývojáři ve vaší organizaci nebo jiná organizace.

Další informace o způsobu, jakým funguje ověřování ve službě Azure AD najdete v tématu [scénáře ověřování pro Azure AD](active-directory-authentication-scenarios.md).

### <a name="overview-of-the-consent-framework"></a>Přehled rozhraní souhlasu
Azure AD souhlasu framework usnadňuje vývoj víceklientské web a nativní klientských aplikací, které potřebují přístup k webové rozhraní API pro zabezpečené klient služby Azure AD, liší od verze, kde je registrovaná aplikace klienta. Tyto webové rozhraní API obsahovat Microsoft Graph API (pro přístup k Azure Active Directory, Intune a službám Office 365) a další služby Microsoftu rozhraní API, kromě vlastní webové rozhraní API. Rozhraní je založena na uživatele nebo správce udělení souhlasu k aplikaci, která požaduje být registrováno v jejich adresáři, který může zahrnovat přístup k datům adresáře.

Například pokud webovou aplikaci klienta potřebuje ke čtení informací z kalendáře o uživateli z Office 365, tento uživatel bude nutné nutné vyjádřit souhlas do klientské aplikace. Po lze souhlasu, nebudou klientská aplikace volání rozhraní Microsoft Graph API jménem uživatele, a podle potřeby použijte informace v kalendáři. [Microsoft Graph API](https://graph.microsoft.io) poskytuje přístup k datům v Office 365 (jako je například kalendáře a zprávy z Exchange, weby a seznamy ze služby SharePoint, dokumenty z Onedrivu, poznámkových bloků z aplikace OneNote, úlohy z Planner, sešity z aplikace Excel, atd.), a také uživatelé a skupiny z Azure AD a jiných datových objektů z více cloudových služeb Microsoftu. 

Rozhraní framework souhlasu je založen na OAuth 2.0 a jeho různé toky, například kód grant a klient přihlašovací údaje pro autorizaci udělit, pomocí veřejného nebo důvěrné klientů. Pomocí standardu OAuth 2.0, Azure AD umožňuje vytvářet mnoho různých typů klientské aplikace, například na telefon, tablet, server nebo webovou aplikaci, a získat přístup k požadované prostředky.

Podrobnější informace o rozhraní souhlasu, najdete v části [OAuth 2.0 ve službě Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx), [scénáře ověřování pro Azure AD](active-directory-authentication-scenarios.md)a informace o získání autorizovaný přístup k Office 365 přes Microsoft Graph najdete v tématu [ověřování aplikací s Microsoft Graph](https://graph.microsoft.io/docs/authorization/auth_overview).

#### <a name="example-of-the-consent-experience"></a>Příklad souhlasu prostředí
Následující postup se zobrazí, jak souhlasu prostředí funguje pro vývojáře aplikací a uživatele.

1. Na stránce konfigurace klientských aplikací na portálu Azure nastavte oprávnění, které vaše aplikace vyžaduje pomocí nabídek v části požadované oprávnění.
   
    ![Oprávnění k ostatním aplikacím](./media/active-directory-integrating-applications/requiredpermissions.png)
2. Zvažte oprávnění aplikace byly aktualizovány, je aplikace spuštěna a uživatel je použijte první. Pokud aplikace není již získal k přístupu nebo aktualizace tokenu, aplikace musí přejít na koncový bod autorizace Azure AD k získání autorizační kód, který slouží k získávání nových přístupu a aktualizace tokenu.
3. Pokud není uživatel ověřen, budete se zobrazí výzva k přihlášení do služby Azure AD.
   
    ![Uživatel nebo správce přihlášení do služby Azure AD](./media/active-directory-integrating-applications/usersignin.png)
4. Jakmile se uživatel přihlásil, Azure AD se určují, jestli uživatel musí zobrazený na stránce souhlasu. Toto rozhodnutí je založená na tom, jestli uživatel (nebo správce jeho organizace) již udělil souhlas aplikace. Pokud již byla nebyl udělen souhlas, Azure AD se zobrazí výzva k vyjádření souhlasu a zobrazí požadovaná oprávnění, musí se funkce. Sada oprávnění, která se zobrazí v dialogovém okně souhlasu jsou stejné jako co byla vybrána v delegovaná oprávnění na webu Azure portal.
   
    ![Činnost koncového uživatele souhlasu](./media/active-directory-integrating-applications/consent.png)
5. Jakmile uživatel uděluje souhlas, autorizační kód se vrátí do vaší aplikace, které lze uplatnit získat přístupový token a aktualizujte token. Další informace o tomto toku najdete v tématu [webové aplikace do webové části rozhraní API](active-directory-authentication-scenarios.md#web-application-to-web-api) kapitoly [scénáře ověřování pro Azure AD](active-directory-authentication-scenarios.md).

6. Jako správce můžete také souhlas přidělená oprávnění aplikace jménem všechny uživatele ve vašem klientovi. To bude zabránit dialogu souhlasu pro každého uživatele v klientovi. Můžete to provést z [portál Azure](https://portal.azure.com) ze stránky vaší aplikace. Z **nastavení** pro vaši aplikaci, klikněte na **požadovaných oprávnění** a klikněte na **udělit oprávnění** tlačítko. 

    ![Udělení oprávnění pro explicitní správce souhlasu](./media/active-directory-integrating-applications/grantpermissions.png)
    
> [!NOTE]
> Udělení explicitní souhlas pomocí **udělit oprávnění** tlačítko je momentálně nevyžaduje pro jednostránkové aplikace (SPA), které používají ADAL.js, jak přístupový token se požaduje bez souhlasu výzvy, které se nezdaří, pokud již není udělen souhlas.   

### <a name="configuring-a-client-application-to-access-web-apis"></a>Konfigurace klientskou aplikaci pro přístup k webové rozhraní API
Aby webové nebo důvěrné klientskou aplikaci, aby mohli účastnit tok udělení autorizace, který vyžaduje ověření (a získat přístupový token) je potřeba vytvořit zabezpečené přihlašovací údaje. Výchozí metoda ověřování nepodporuje portál Azure je ID klienta + symetrický klíč. Tato část popisuje kroky konfigurace, který je třeba zadat pověření vašeho klienta tajný klíč.

Kromě, než klient může přístup webového rozhraní API vystavené prostředků aplikace (ie: Microsoft Graph API), rozhraní souhlasu zajistí klient obdrží udělení oprávnění, která je potřeba, na základě požadovaná oprávnění. Ve výchozím nastavení všechny aplikace můžete vybrat oprávnění z Azure Active Directory (rozhraní Graph API) a Azure Service Management API s Azure AD "Povolit přihlášení a čtení uživatelský profil" oprávnění, která již vybrána ve výchozím nastavení. Pokud klientské aplikace registrované v klientovi Azure AD Office 365, webová rozhraní API a oprávnění pro služby SharePoint a Exchange Online bude také k dispozici pro výběr. Můžete vybrat z [dva typy oprávnění](active-directory-dev-glossary.md#permissions) v rozevíracích nabídek vedle požadované webové rozhraní API:

* Oprávnění aplikací: Klientské aplikace potřebuje přístup k webové rozhraní API přímo jako samotný (žádný kontext uživatele). Tento typ oprávnění vyžaduje souhlas správce a není k dispozici pro nativní klientské aplikace.
* Přidělená oprávnění: Klientské aplikace potřebuje přístup k webové rozhraní API jako přihlášeného uživatele, ale přístup omezen vybrané oprávnění. Tento typ oprávnění můžete udělit uživatelem, není-li toto oprávnění je nakonfigurovaný jako vyžadování souhlas správce. 

> [!NOTE]
> Přidání delegovaného oprávnění k aplikaci neuděluje automaticky souhlas pro uživatele v rámci klienta, stejně jako v portálu Azure Classic. Uživatelé musí ručně souhlas pro přidané přidělená oprávnění za běhu, pokud správce klikne **udělit oprávnění** tlačítko z **požadovaných oprávnění** oddílu aplikace na stránce na portálu Azure. 

#### <a name="to-add-credentials-or-permissions-to-access-web-apis"></a>Chcete-li přidat přihlašovací údaje nebo oprávnění pro přístup k webovým rozhraním API
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Výběrem účtu v pravém horním rohu stránky zvolte klientovi Azure AD.
3. V horní nabídce zvolte **Azure Active Directory**, klikněte na tlačítko **registrace aplikace**a pak klikněte na aplikaci, kterou chcete konfigurovat. To se dostanete na stránku aplikace rychlý start, a také otevře okno nastavení pro aplikaci.
4. Chcete-li přidat tajný klíč pro přihlašovací údaje pro webovou aplikaci, klikněte na tlačítko části "Klíče" v okně nastavení.  
   
   * Přidejte popis klíče a vyberte 1 nebo 2 rok trvání. 
   * Sloupec nejvíce vpravo bude obsahovat hodnotou klíče, po uložení změn konfigurace. Ujistěte se vraťte do této části a zkopírujte ho po kliknutí na Uložit, abyste je ho pro použití v aplikaci klienta během ověřování při spuštění.
5. Přidání oprávnění pro přístup k prostředku rozhraní API z vašeho klienta, klikněte v části "Požadovaných oprávnění" v okně nastavení. 
   
   * První klikněte na tlačítko "Přidat".
   * Klikněte na tlačítko "Vyberte k rozhraní API" a vyberte typ prostředků, kterou chcete vybrat z.
   * Procházet seznam dostupných rozhraní API nebo použijte pole hledání a vyberte z dostupných prostředků aplikací ve vašem adresáři, které zveřejňují webového rozhraní API. Klikněte na prostředek zájem a pak klikněte na **vyberte**.
   * Po výběru, můžete přesunout **vyberte oprávnění** nabídky, kde můžete vybrat "Oprávnění aplikací" a "Delegovaná oprávnění" pro vaši aplikaci.
   
6. Po dokončení klikněte **provádí** tlačítko.

> [!NOTE]
> Kliknutím **provádí** tlačítko taky automaticky nastaví oprávnění pro vaši aplikaci ve vašem adresáři na základě oprávnění k ostatním aplikacím, které jste nakonfigurovali.  Tato oprávnění aplikací můžete zobrazit prohlížením aplikace **nastavení** okno.
> 
> 

### <a name="configuring-a-resource-application-to-expose-web-apis"></a>Konfigurace prostředků aplikace ke zveřejnění webových rozhraní API
Můžete vyvíjet webové rozhraní API a nastavit jej jako dostupný pro klientské aplikace díky zpřístupnění přístup [obory](active-directory-dev-glossary.md#scopes) a [role](active-directory-dev-glossary.md#roles). Správně nakonfigurována webového rozhraní API je k dispozici stejně jako ostatní Microsoft webové rozhraní API, včetně rozhraní Graph API a rozhraní API Office 365. Obory přístupu a rolí se zveřejňují přes vaše [manifestu aplikace](active-directory-dev-glossary.md#application-manifest), což je soubor JSON, který představuje konfiguraci identity vaší aplikace.  

V následující části vám ukáže, jak vystavit oborů přístupu změnou manifest prostředků aplikace.

#### <a name="adding-access-scopes-to-your-resource-application"></a>Přidání oborů přístupu k vaší aplikaci prostředků
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Výběrem účtu v pravém horním rohu stránky zvolte klientovi Azure AD.
3. V horní nabídce zvolte **Azure Active Directory**, klikněte na tlačítko **registrace aplikace**a pak klikněte na aplikaci, kterou chcete konfigurovat. To se dostanete na stránku aplikace rychlý start, a také otevře okno nastavení pro aplikaci.
4. Klikněte na tlačítko **Manifest** ze stránky aplikace k otevření editoru vloženého manifestu. 
5. Následující fragment kódu JSON nahraďte "oauth2Permissions" uzlu. Tento fragment kódu je příklad toho, jak vystavit obor označuje jako "vystupovat jako jiný uživatel", což umožňuje vlastník prostředku umožňuje klientskou aplikaci typ Delegovaný přístup k prostředku. Ujistěte se, změnit text a hodnoty pro své aplikaci:
   
        "oauth2Permissions": [
        {
            "adminConsentDescription": "Allow the application full access to the Todo List service on behalf of the signed-in     user",
            "adminConsentDisplayName": "Have full access to the Todo List service",
            "id": "b69ee3c9-c40d-4f2a-ac80-961cd1534e40",
            "isEnabled": true,
            "type": "User",
            "userConsentDescription": "Allow the application full access to the todo service on your behalf",
            "userConsentDisplayName": "Have full access to the todo service",
            "value": "user_impersonation"
            }
        ],
   
    Hodnota id musí být nové vytvářenému identifikátoru GUID, který vytvoříte pomocí [nástroj pro vytváření GUID](https://msdn.microsoft.com/library/ms241442%28v=vs.80%29.aspx) nebo prostřednictvím kódu programu. Představuje jedinečný identifikátor pro oprávnění, který je zveřejněný prostřednictvím webového rozhraní API. Jakmile se váš klient je správně nakonfigurovaný s požadavkem o přístup k webovému rozhraní API a volání webového rozhraní API, nabídne tokenu OAuth 2.0 JWT, který má nastaven na hodnotu vyšší, který v tomto případě je user_impersonation deklarace oboru (scp).
   
   > [!NOTE]
   > Další obory později podle potřeby můžete vystavit. Zvažte, zda webového rozhraní API mohou být vystaveny víc oborů, které jsou spojené s celou řadu různých funkcí. Nyní můžete řídit přístup k webové rozhraní API pomocí deklarace oboru (scp) v přijatého tokenu OAuth 2.0 JWT.
   > 
   > 
6. Klikněte na tlačítko **Uložit** uložit manifest. Webové rozhraní API je nyní nakonfigurován pro použití jinými aplikacemi ve vašem adresáři.

#### <a name="to-verify-the-web-api-is-exposed-to-other-applications-in-your-directory"></a>Ověření webové rozhraní API je vystaven k ostatním aplikacím ve vašem adresáři
1. V horní nabídce klikněte na tlačítko **registrace aplikace**, vyberte požadované klientskou aplikaci, kterou chcete konfigurovat přístup k webovému rozhraní API a přejděte do okna nastavení.
2. Z **požadovaných oprávnění** vyberte webové rozhraní API, který jste právě zveřejněný oprávnění pro. Z rozevírací nabídky delegovaná oprávnění vyberte novou oprávnění.

![Seznam úkolů oprávnění jsou zobrazeny.](./media/active-directory-integrating-applications/todolistpermissions.png)

#### <a name="more-on-the-application-manifest"></a>Další informace o manifest aplikace
Manifest aplikace se ve skutečnosti slouží jako mechanismus pro aktualizaci entity aplikace, která definuje konfiguraci identity aplikaci Azure AD, včetně oborů přístupu rozhraní API, které jsme probrali všechny atributy. Další informace o aplikaci entity, najdete v tématu [aplikace Graph API entity dokumentaci](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity). V něm najdete úplný referenční informace o členech entity aplikací slouží k zadání oprávnění pro vaše rozhraní API:  

* appRoles člena, který je kolekce z [aplikační role](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type) entity, které lze použít k definování **oprávnění aplikací** pro webové rozhraní API  
* oauth2Permissions člena, který je kolekce z [OAuth2Permission](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type) entity, které lze použít k definování **delegovaná oprávnění** pro webové rozhraní API

Další informace o aplikaci manifestu koncepty obecně platí, naleznete v [pochopení manifest aplikace Azure Active Directory](active-directory-application-manifest.md).

### <a name="accessing-the-azure-ad-graph-and-office-365-via-microsoft-graph-apis"></a>Přístup k Azure AD Graph a Office 365 přes rozhraní API pro Microsoft Graph  
Jak už bylo zmíněno dříve, kromě vystavení nebo přístup k rozhraní API na vaše vlastní aplikace prostředků, můžete také aktualizovat klientskou aplikaci pro přístup k rozhraní API vystavené zdrojů společnosti Microsoft.  Rozhraní Microsoft Graph API, která se nazývá "Microsoft Graph" v seznamu oprávnění k ostatním aplikacím, je k dispozici nebo všechny aplikace, které jsou registrovány s Azure AD. Pokud registrujete klientskou aplikaci v rámci služeb Office 365 zřízenou klient Azure AD, můžete také přístup k veškerým oprávnění vystavené Microsoft Graph API k různým prostředkům Office 365.

Úplné informace o oborů přístupu vystavené Microsoft Graph API, najdete v tématu [obory oprávnění | Koncepty Microsoft Graph API](https://graph.microsoft.io/docs/authorization/permission_scopes) článku.

> [!NOTE]
> Kvůli aktuálním omezením nativní klientské aplikace může volat pouze do Azure AD Graph API pokud používají oprávnění "Přístup k adresáři vaší organizace".  Toto omezení neplatí pro webové aplikace.
> 
> 

### <a name="configuring-multi-tenant-applications"></a>Konfigurace aplikací pro více klientů
Když přidáváte aplikaci do služby Azure AD, můžete aplikace měly přístup jenom uživatelé ve vaší organizaci. Alternativně můžete aplikace mají přístup uživatelé v externími organizacemi. Tyto typy aplikací se nazývají jednoho klienta a víceklientským aplikacím. Můžete upravit konfiguraci jednoho klienta aplikace, aby aplikaci více klientů, které tato část popisuje níže.

Je důležité si uvědomit, rozdíly mezi jednoho klienta a víceklientské aplikace:  

* Aplikace jednoho klienta je určena pro použití v jedné organizaci. Jsou obvykle aplikace obchodní (LoB), vytvořené pomocí vývojář enterprise. Jednoho klienta aplikace potřebuje pouze mít přístup uživatelé v jednom adresáři, a v důsledku toho se pouze musí být zřízená v jednom adresáři.
* Víceklientské aplikace, který určený pro použití v mnoha organizacích. Jsou webové aplikace software jako služba (SaaS) obvykle zapsaných správcem nezávislý dodavatel softwaru (ISV). Víceklientské aplikace je nutné zřídit v každý adresář, kde se bude používat, který vyžaduje souhlas uživatele nebo správce registrace je podporované prostřednictvím rozhraní Azure AD souhlasu. Upozorňujeme, že všechny aplikace nativního klienta jsou víceklientské ve výchozím nastavení, jako se instalují na zařízení vlastníka prostředku. V rámci souhlasu naleznete v přehledu oddílu souhlas Framework výše pro další podrobnosti.

#### <a name="enabling-external-users-to-grant-your-application-access-to-their-resources"></a>Povolení externí uživatelům udělit přístup aplikace k jejich prostředkům
Pokud píšete aplikaci, kterou chcete zpřístupnit pro vaše zákazníky nebo partnery mimo organizaci, budete muset aktualizovat definice aplikace na portálu Azure.

> [!NOTE]
> Při povolování více klientů, je nutné zajistit, aby identifikátor ID URI aplikace aplikace patří ověřené domény. Kromě toho vrátí adresa URL musí začínat https://. Další informace najdete v tématu [objekty aplikací a hlavní objekty služeb](active-directory-application-objects.md).
> 
> 

Chcete-li povolit přístup k vaší aplikaci pro externí uživatele: 

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Výběrem účtu v pravém horním rohu stránky zvolte klientovi Azure AD.
3. V horní nabídce zvolte **Azure Active Directory**, klikněte na tlačítko **registrace aplikace**a pak klikněte na aplikaci, kterou chcete konfigurovat. To se dostanete na stránku aplikace rychlý start, a také otevře okno nastavení pro aplikaci.
4. V okně nastavení klikněte na tlačítko **vlastnosti** a překlopit **nevyužívá dělené tabulky více** přepnout **Ano**.

Po provedení změn výše, bude moci udělit přístup vaší aplikace do svého adresáře a dalších dat uživatelům a správcům v jiných organizacích.

#### <a name="triggering-the-azure-ad-consent-framework-at-runtime"></a>Spuštění rozhraní Azure AD souhlasu za běhu
Pokud chcete používat rozhraní souhlasu, víceklientské klientské aplikace musíte požádat o autorizace pomocí OAuth 2.0. [Ukázky kódu](https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multi-tenant) je možné zobrazit, jak webová aplikace, nativní aplikaci nebo aplikaci typu server/démon požadavky autorizačními kódy nebo přístupovými tokeny k volání webového rozhraní API.

Webové aplikace může také nabízí registrace prostředí pro uživatele. Pokud nabízíte registrace zkušeností, očekává se, že uživatel bude klikněte na na přihlašovací tlačítko, bude přesměrování prohlížeč Azure AD OAuth2.0 autorizaci koncového bodu nebo koncový bod OpenID Connect informací o uživateli. Tyto koncové body umožňuje aplikaci získat informace o novém uživateli zkontrolováním požadavku id_token. Následující fázi registrace uživatele zobrazí se výzva souhlasu podobné výše uvedeném v přehledu oddílu souhlas Framework.

Alternativně webové aplikace může také nabízí prostředí, které umožňuje správcům "zaregistrujte Moje společnost". Toto prostředí by také přesměruje uživatele na Azure AD OAuth 2.0 zajistí autorizaci koncového bodu. V takovém případě ale předat výzvu = admin_consent parametr koncového bodu authorize vynutit prostředí souhlas správce, kde správce udělí souhlas jménem své organizace. Jenom uživatel, který ověřuje pomocí účtu, který patří k roli globálního správce může poskytnout souhlasu; ostatní dojde k chybě. Odpověď na úspěšné souhlasu, bude obsahovat admin_consent = true. Když uplatňuje přístupový token, budou se zobrazovat také požadavku id_token, který poskytuje informace o organizaci a správce, který se zaregistrovali do vaší aplikace.

### <a name="enabling-oauth-20-implicit-grant-for-single-page-applications"></a>Povolení OAuth 2.0 implicitní udělit pro jednostránkové aplikace
Jednu stránku aplikace (SPA) je obvykle strukturovaná s JavaScript náročné front-end, který běží v prohlížeči, která volá webové aplikace API zpět mohli provést jeho obchodní logiku. Pro SPA hostované ve službě Azure AD použijete k ověření uživatele se službou Azure AD a získat token, který vám pomůže zabezpečená volání z aplikace JavaScript klienta, aby jeho back-endu webové rozhraní API implicitní udělení OAuth 2.0. Po udělení souhlasu uživatele Tento stejný protokol ověřování lze získat tokeny zabezpečení volání mezi klientem a dalších webových rozhraní API prostředků nakonfigurovaný pro aplikaci. Další informace o udělení implicitní autorizace a vám pomohou rozhodnout, zda je správný pro váš scénář aplikací najdete v tématu [pochopení OAuth2 implicitní tok v Azure Active Directory poskytování ](active-directory-dev-understanding-oauth2-implicit-grant.md).

Ve výchozím nastavení je pro aplikace zakázaná implicitní Grant OAuth 2.0. Implicitní udělení OAuth 2.0 pro vaši aplikaci můžete povolit tak `oauth2AllowImplicitFlow` hodnotu v jeho [manifest aplikace](active-directory-application-manifest.md), což je soubor JSON, který představuje konfiguraci identity vaší aplikace.

#### <a name="to-enable-oauth-20-implicit-grant"></a>Chcete-li povolit implicitní grant OAuth 2.0
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Výběrem účtu v pravém horním rohu stránky zvolte klientovi Azure AD.
3. V horní nabídce zvolte **Azure Active Directory**, klikněte na tlačítko **registrace aplikace**a pak klikněte na aplikaci, kterou chcete konfigurovat. To se dostanete na stránku aplikace rychlý start, a také otevře okno nastavení pro aplikaci.
4. Na stránce aplikace klikněte na tlačítko **Manifest** otevřete editor pro vložené manifestu.
   Vyhledejte a nastavte hodnotu "oauth2AllowImplicitFlow" na "true". Ve výchozím nastavení je "false".
   
    `"oauth2AllowImplicitFlow": true,`
5. Uložte aktualizovaný manifestu. Po uložení webového rozhraní API je nyní nakonfigurováno pro udělení implicitní OAuth 2.0 používat k ověřování uživatelů.


## <a name="removing-an-application"></a>Odebrání aplikace
Tato část popisuje postup odebrání aplikace z vašeho klienta Azure AD.

### <a name="removing-an-application-authored-by-your-organization"></a>Odebrání aplikace vytvořené ve vaší organizaci
Jedná se o aplikace, které se zobrazí v části "Vlastní aplikace Moje společnost" filtru na hlavní stránce "Aplikace" pro vašeho tenanta Azure AD. V technické podmínky jedná se o aplikace, které jste zaregistrovali buď ručně prostřednictvím portálu Azure classic nebo programově pomocí prostředí PowerShell nebo rozhraní Graph API. Přesněji řečeno jsou reprezentované pomocí k aplikaci a instanční objekt objektu v klientovi. V tématu [objekty aplikací a hlavní objekty služeb](active-directory-application-objects.md) Další informace.

#### <a name="to-remove-a-single-tenant-application-from-your-directory"></a>Pro jednoho klienta aplikaci odebrat z adresáře
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Výběrem účtu v pravém horním rohu stránky zvolte klientovi Azure AD.
3. V horní nabídce zvolte **Azure Active Directory**, klikněte na tlačítko **registrace aplikace**a pak klikněte na aplikaci, kterou chcete konfigurovat. To se dostanete na stránku aplikace rychlý start, a také otevře okno nastavení pro aplikaci.
4. Na stránce aplikace klikněte na tlačítko **odstranit**.
5. Klikněte na tlačítko **Ano** v potvrzovací zprávě.

#### <a name="to-remove-a-multi-tenant-application-from-your-directory"></a>Odebrání víceklientské aplikace z adresáře
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Výběrem účtu v pravém horním rohu stránky zvolte klientovi Azure AD.
3. V horní nabídce zvolte **Azure Active Directory**, klikněte na tlačítko **registrace aplikace**a pak klikněte na aplikaci, kterou chcete konfigurovat. To se dostanete na stránku aplikace rychlý start, a také otevře okno nastavení pro aplikaci.
4. V okně Nastavení zvolte **vlastnosti** a překlopit **nevyužívá dělené tabulky více** přepnout **ne**. To převede vaší aplikaci, která bude jednoho klienta, ale aplikace zůstanou i nadále v organizaci, kteří k němu již souhlasí.
5. Klikněte na **odstranit** tlačítko ze stránky aplikace.
6. Klikněte na tlačítko **Ano** v potvrzovací zprávě.

### <a name="removing-a-multi-tenant-application-authorized-by-another-organization"></a>Odebráním víceklientské aplikace, autorizovat jiné organizaci
Jedná se o podmnožinu aplikace, které se zobrazí v části "Aplikace společnost používá" filtr na stránce hlavní "aplikace" pro vašeho tenanta Azure AD, konkrétně ty, které nejsou uvedené v seznamu "Vlastní Moje společnost aplikace". V technické podmínky jedná se o víceklientské aplikace během procesu souhlasu zaregistrovány. Přesněji řečeno jsou reprezentované pomocí pouze objekt zabezpečení služby ve vašem klientovi. V tématu [objekty aplikací a hlavní objekty služeb](active-directory-application-objects.md) Další informace.

Chcete-li odebrat přístup k aplikaci víceklientské do vašeho adresáře (po s udělen souhlas), musí mít správce společnosti předplatného Azure k odebrání přístupu prostřednictvím portálu Azure. Alternativně můžete použít Správce společnosti [rutin prostředí Azure AD PowerShell](http://go.microsoft.com/fwlink/?LinkId=294151) k odebrání přístupu.

## <a name="next-steps"></a>Další kroky
* Najdete v článku [Branding pokyny pro integrované aplikace](active-directory-branding-guidelines.md) tipy na visual pokyny pro vaši aplikaci.
* Další informace o vztah mezi objekty aplikace a instanční objekt aplikace v [objekty aplikací a hlavní objekty služeb](active-directory-application-objects.md).
* Další informace o roli plní manifestu aplikace, najdete v části [pochopení manifest aplikace Azure Active Directory](active-directory-application-manifest.md)
* Najdete v článku [Glosář vývojáře Azure AD](active-directory-dev-glossary.md) definice některých koncepty pro vývojáře Azure Active Directory (AD) jádra.
* Přejděte [Příručka pro vývojáře Active Directory](active-directory-developers-guide.md) přehled všechny vývojáře související obsah.

