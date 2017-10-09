---
title: "aaaIntegrating aplikací s Azure Active Directory | Microsoft Docs"
description: Podrobnosti o tom, jak tooadd, aktualizovat nebo odebrat aplikace v Azure Active Directory (Azure AD).
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
ms.openlocfilehash: c6bf1123bb3a4d78b55c1c55558e684fb844e687
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrating-applications-with-azure-active-directory"></a>Integrace aplikací s Azure Active Directory
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Podnikové vývojáře a poskytovatelé softwaru jako služba (SaaS) můžete vyvíjet komerční cloudové služby nebo obchodních aplikací, které můžou být integrované se službou Azure Active Directory (Azure AD) tooprovide zabezpečené přihlaste a autorizace pro jejich služby. toointegrate aplikace nebo služby s Azure AD, musí vývojář nejprve zaregistrovat s Azure AD pomocí portálu Azure classic hello hello podrobnosti o svých aplikacích.

Tento článek ukazuje, jak tooadd, aktualizovat nebo odebrat aplikace ve službě Azure AD. Získáte informace o různých typů aplikací, které lze integrovat s Azure AD, jak hello tooconfigure vaší aplikace tooaccess jiné prostředky, jako jsou webová rozhraní API a další.

toolearn Další informace o hello dva Azure AD objekty, které představují zaregistrovanou aplikaci a hello vztah mezi nimi, najdete v části [objekty aplikací a hlavní objekty služeb](active-directory-application-objects.md); toolearn více informací o hello branding pokyny vám by měl využít při vývoji aplikací s Azure Active Directory najdete v tématu [Branding pokyny pro integrované aplikace](active-directory-branding-guidelines.md).

## <a name="adding-an-application"></a>Přidání aplikace
Jakékoli aplikace, která chce toouse hello možnosti služby Azure AD musí být zaregistrován nejdřív v klientovi služby Azure AD. Zahrnuje tohoto procesu registrace poskytne Azure AD podrobnosti o aplikaci, jako je například adresa URL hello tam, kde je umístěn, hello adresa URL odpovědi toosend po ověření uživatele, hello identifikátor URI, který identifikuje hello aplikace a tak dále.

Pokud vytváříte webovou aplikaci, která právě musí toosupport přihlášení pro uživatele ve službě Azure AD, můžete jednoduše provést níže uvedené pokyny hello. Pokud aplikace potřebuje přihlašovací údaje nebo oprávnění tooaccess tooa webového rozhraní API nebo potřebuje tooallow uživatelé z jiných klienty Azure AD tooaccess, najdete v části [aktualizaci aplikace](#updating-an-application) toocontinue část konfigurace vaší aplikace.

### <a name="tooregister-a-new-application-in-hello-azure-portal"></a>tooregister novou aplikaci v hello portálu Azure
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Zvolte výběrem účtu v hello pravém horním rohu stránky hello klientovi Azure AD.
3. V levém navigačním podokně hello zvolte **více služeb**, klikněte na tlačítko **registrace aplikace**a klikněte na tlačítko **přidat**.
4. Postupujte podle pokynů hello a vytvořte novou aplikaci. Pokud chcete konkrétní příklady pro webové aplikace nebo nativních aplikací, podívejte se na naše [– elementy QuickStart](active-directory-developers-guide.md).
  * Pro webové aplikace, zadejte hello **přihlašovací adresa URL**, což je hello základní adresu URL aplikace, kde uživatelé se mohou přihlásit v např `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: hello **App ID URI** is a unique identifier for your application. hello convention is toouse `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * U nativních aplikací, zadejte **identifikátor URI pro přesměrování**, které Azure AD používá tooreturn odpovědi tokenu. Zadejte hodnotu konkrétní tooyour aplikace pomocí. např`http://MyFirstAADApp`
5. Po dokončení registrace Azure AD přiřadí aplikace identifikátor jedinečných klientských hello ID aplikace. Byla přidána aplikace a stránky rychlý Start toohello se mají provést pro vaši aplikaci. V závislosti na tom, jestli je aplikace na web nebo nativní aplikace, zobrazí se různých možností, jak aplikace tooyour tooadd další možnosti. Po přidání aplikace, můžete zahájit aktualizaci vašeho toosign aplikace tooenable uživatelům v přístupu k webové rozhraní API v ostatních aplikacích nebo konfiguraci víceklientské aplikace (které umožní aplikaci tooaccess jiné organizace).

> [!NOTE]
> Ve výchozím nastavení je registrace aplikace hello nově vytvořený uživatelů nakonfigurovanou tooallow z vašeho adresáře toosign tooyour aplikace.
> 
> 

## <a name="updating-an-application"></a>Aktualizace aplikace
Jakmile aplikaci byl registrován u služby Azure AD, pravděpodobně bude nutné aktualizovat toobe tooprovide přístup tooweb rozhraní API, budou dostupné v jiných organizací a další. Tato část popisuje různé způsoby, ve kterých budete pravděpodobně tooconfigure další aplikace. Nejprve spustíme přehled hello souhlas Framework, která je důležité toounderstand, pokud vytváříte aplikace prostředků nebo rozhraní API, které budou využívat klientské aplikace vytvořené vývojáři ve vaší organizaci nebo jiná organizace.

Další informace o hello ověřování způsob, jak funguje ve službě Azure AD najdete v tématu [scénáře ověřování pro Azure AD](active-directory-authentication-scenarios.md).

### <a name="overview-of-hello-consent-framework"></a>Přehled hello souhlasu framework
Azure AD souhlasu framework umožňuje snadno toodevelop víceklientské webové a nativní klientské aplikace, které je třeba tooaccess webové rozhraní API pro zabezpečené klient služby Azure AD, liší od hello jeden kde registrovaná aplikace klienta hello. Tyto webové rozhraní API obsahovat hello Microsoft Graph API (tooaccess Azure Active Directory, Intune a službám Office 365) a další služby Microsoftu rozhraní API, kromě tooyour vlastní webové rozhraní API. Hello framework je založena na uživatele nebo správce udělení souhlasu tooan aplikace, která požaduje toobe zaregistrovaný v jejich adresáři, který může zahrnovat přístup k datům adresáře.

Například pokud webovou aplikaci klienta potřebuje informací z kalendáře tooread o hello uživatele ze služeb Office 365, bude tento uživatel vyžaduje tooconsent toohello klientské aplikace. Po lze souhlasu, hello klientská aplikace bude mít toocall hello Microsoft Graph API jménem uživatele hello a podle potřeby použijte informací z kalendáře hello. Hello [Microsoft Graph API](https://graph.microsoft.io) poskytuje přístup toodata v Office 365 (např. zprávy ze systému Exchange, webů a seznamy ze služby SharePoint, dokumenty z Onedrivu, poznámkových bloků z aplikace OneNote, úlohy z Planner sešity z kalendáře a V aplikaci Excel, apod), a také uživatelé a skupiny z Azure AD a jiných datových objektů z více cloudových služeb Microsoftu. 

framework souhlasu Hello je založený na protokolu OAuth 2.0 a jeho různé toky, jako je například autorizační kód poskytnout přihlašovací údaje k udělení a klienta, pomocí veřejného nebo důvěrné klientů. Pomocí standardu OAuth 2.0, Azure AD je možné toobuild mnoho různých typů klientské aplikace, například jako na telefon, tablet, server nebo webovou aplikaci a získat přístup toohello požadované prostředky.

Podrobnější informace o hello souhlasu framework, najdete v části [OAuth 2.0 ve službě Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx), [scénáře ověřování pro Azure AD](active-directory-authentication-scenarios.md)a informace o získání autorizovaný přístup tooOffice 365 prostřednictvím Microsoft Graph, najdete v části [ověřování aplikací s Microsoft Graph](https://graph.microsoft.io/docs/authorization/auth_overview).

#### <a name="example-of-hello-consent-experience"></a>Příklad hello souhlasu prostředí
Hello následující kroky se ukazují, jak funguje hello souhlasu prostředí pro vývojáře aplikace hello i uživateli.

1. Na stránce konfigurace aplikace webového klienta v hello portálu Azure nastavte hello oprávnění, která vaše aplikace vyžaduje pomocí nabídky hello v hello části požadovaných oprávnění.
   
    ![Oprávnění tooother aplikace](./media/active-directory-integrating-applications/requiredpermissions.png)
2. Zvažte oprávnění aplikace byly aktualizovány, hello aplikace běží a uživatel je o toouse pro hello poprvé. Pokud aplikace hello nebyla již získal přístup nebo aktualizace tokenu, hello aplikace je potřeba toogo tooAzure AD na autorizaci koncového bodu tooobtain autorizační kód, který lze použít tooacquire nové přístup a obnovovací token.
3. Pokud již není hello uživatel ověřen, vyzve toosign v tooAzure AD.
   
    ![Uživatel nebo správce přihlášení tooAzure AD](./media/active-directory-integrating-applications/usersignin.png)
4. Po hello uživatel přihlásí, bude Azure AD určují, jestli uživatel hello musí toobe ukazuje na stránku souhlasu. Toto rozhodnutí je založená na tom, jestli uživatel hello (nebo správce jeho organizace) již udělil souhlas aplikace hello. Pokud nebyl již udělen souhlas, Azure AD zobrazí výzvu hello uživatele o souhlas a zobrazí hello požadované oprávnění potřebná toofunction. Hello sadu oprávnění, která se zobrazí v dialogovém okně souhlasu hello jsou hello stejné jako co byla vybrána v hello delegovaná oprávnění v hello portálu Azure.
   
    ![Činnost koncového uživatele souhlasu](./media/active-directory-integrating-applications/consent.png)
5. Jakmile uživatel hello uděluje souhlas, autorizační kód se vrátí tooyour aplikaci, které může být uplatněn tooacquire přístupový token a aktualizovat token. Další informace o tomto toku najdete v tématu hello [webové části tooweb rozhraní API aplikace](active-directory-authentication-scenarios.md#web-application-to-web-api) kapitoly [scénáře ověřování pro Azure AD](active-directory-authentication-scenarios.md).

6. Jako správce můžete také souhlas přidělená oprávnění aplikace tooan jménem všichni uživatelé hello ve vašem klientovi. Dialogové okno hello to bude zabránit zobrazování pro každého uživatele v klientovi hello. Můžete to provést z hello [portál Azure](https://portal.azure.com) ze stránky vaší aplikace. Z hello **nastavení** pro vaši aplikaci, klikněte na **požadovaných oprávnění** a klikněte na hello **udělit oprávnění** tlačítko. 

    ![Udělení oprávnění pro explicitní správce souhlasu](./media/active-directory-integrating-applications/grantpermissions.png)
    
> [!NOTE]
> Udělení výslovným souhlasem pomocí hello **udělit oprávnění** tlačítko je momentálně nevyžaduje pro jednostránkové aplikace (SPA), které používají ADAL.js, hello přístupový token je požadované bez souhlasu výzvy, které se nezdaří, pokud není souhlasu udělili oprávnění.   

### <a name="configuring-a-client-application-tooaccess-web-apis"></a>Konfigurace klienta aplikace tooaccess webového rozhraní API
Aby důvěrné nebo webového klienta aplikace toobe možné tooparticipate v toku udělení autorizace, který vyžaduje ověření (a získat přístupový token) je potřeba vytvořit zabezpečené přihlašovací údaje. Metoda ověřování výchozí Hello nepodporuje hello portál Azure je ID klienta + symetrický klíč. Tato část se bude zabývat hello konfigurační kroky požadované tooprovide hello tajný klíč přihlašovací údaje vašeho klienta.

Kromě toho, než klient může přístup webového rozhraní API vystavené prostředků aplikace (ie: Microsoft Graph API), framework souhlasu hello zajistí hello klient obdrží hello udělení oprávnění požadovaná oprávnění požadované, na základě hello. Ve výchozím nastavení všechny aplikace můžete vybrat oprávnění z Azure Active Directory (rozhraní Graph API) a Azure Service Management API s oprávnění "Povolit přihlášení a čtení uživatelský profil" hello Azure AD vybrána ve výchozím nastavení. Pokud klientské aplikace registrované v klientovi Azure AD Office 365, webová rozhraní API a oprávnění pro služby SharePoint a Exchange Online bude také k dispozici pro výběr. Můžete vybrat z [dva typy oprávnění](active-directory-dev-glossary.md#permissions) v hello rozevíracích nabídek potřeby další toohello webové rozhraní API:

* Oprávnění aplikací: Aplikace klienta musí tooaccess hello webového rozhraní API přímo jako samotný (žádný kontext uživatele). Tento typ oprávnění vyžaduje souhlas správce a není k dispozici pro nativní klientské aplikace.
* Delegovaná oprávnění: Klientské aplikace potřebuje tooaccess hello webového rozhraní API jako hello přihlášeného uživatele, ale s přístupem k omezeno hello vybrali oprávnění. Tento typ oprávnění můžete udělit uživatelem, pokud hello oprávnění je nakonfigurovaný jako vyžadování souhlas správce. 

> [!NOTE]
> Přidání aplikace tooan delegovaná oprávnění neuděluje automaticky souhlas toohello uživatelů v rámci klienta hello, stejně jako ve hello portálu Azure Classic. Hello uživatelé musí ručně souhlas pro hello přidat delegovaná oprávnění za běhu, pokud správce hello klikne hello **udělit oprávnění** tlačítko z hello **požadovaných oprávnění** části stránky aplikací hello v hello portálu Azure. 

#### <a name="tooadd-credentials-or-permissions-tooaccess-web-apis"></a>přihlašovací údaje tooadd nebo oprávnění tooaccess webové rozhraní API
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Zvolte výběrem účtu v hello pravém horním rohu stránky hello klientovi Azure AD.
3. V horní nabídce hello zvolte **Azure Active Directory**, klikněte na tlačítko **registrace aplikace**a pak klikněte na tlačítko aplikace hello chcete tooconfigure. To se dostanete stránku rychlý start toohello aplikace, a také otevře okno hello nastavení pro aplikace hello.
4. tooadd tajný klíč pro přihlašovací údaje vaší webové aplikace, klikněte na část "Klíče" hello v okně Nastavení hello.  
   
   * Přidejte popis klíče a vyberte 1 nebo 2 rok trvání. 
   * sloupec nejvíce vpravo Hello bude obsahovat hodnotu klíče hello po uložení změn konfigurace hello. Být jisti toocome zpět toothis části a zkopírujte ji po kliknutí na Uložit, abyste je pro použití v aplikaci klienta během ověřování při spuštění.
5. tooadd oprávnění tooaccess prostředků rozhraní API z klienta, klikněte na část "Požadovaných oprávnění" hello v okně Nastavení hello. 
   
   * První klikněte na tlačítko "Přidat" hello.
   * Klikněte na tlačítko "Vyberte k rozhraní API" tooselect hello typ prostředků, které chcete toopick z.
   * Procházet hello seznam dostupných rozhraní API nebo použijte hello vyhledávací pole tooselect z hello aplikací dostupných prostředků ve vašem adresáři, které zveřejňují webového rozhraní API. Klikněte na prostředek hello zájem a pak klikněte na **vyberte**.
   * Po výběru, můžete přesunout toohello **vyberte oprávnění** nabídky, kde můžete vybrat oprávnění hello"aplikace" a "Delegovaná oprávnění" pro vaši aplikaci.
   
6. Po dokončení klikněte na tlačítko hello **provádí** tlačítko.

> [!NOTE]
> Kliknutím na hello **provádí** tlačítko taky automaticky nastaví hello oprávnění pro vaše aplikace ve vašem adresáři podle oprávnění hello tooother aplikace, které jste nakonfigurovali.  Tato oprávnění aplikací můžete zobrazit prohlížením aplikace hello **nastavení** okno.
> 
> 

### <a name="configuring-a-resource-application-tooexpose-web-apis"></a>Konfigurace prostředků aplikace tooexpose webového rozhraní API
Můžete vyvíjet webové rozhraní API a zkontrolujte díky zpřístupnění přístup aplikace k dispozici tooclient [obory](active-directory-dev-glossary.md#scopes) a [role](active-directory-dev-glossary.md#roles). Správně nakonfigurována webového rozhraní API je k dispozici, stejně jako hello dalších Microsoft webových rozhraní API, včetně hello rozhraní Graph API a hello rozhraní API Office 365. Obory přístupu a rolí se zveřejňují přes vaše [manifestu aplikace](active-directory-dev-glossary.md#application-manifest), což je soubor JSON, který představuje konfiguraci identity vaší aplikace.  

Hello následující části si ukážeme, jak tooexpose přístup rozsahy změnou manifest aplikace hello prostředků.

#### <a name="adding-access-scopes-tooyour-resource-application"></a>Přidání přístupu obory tooyour prostředků aplikace
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Zvolte výběrem účtu v hello pravém horním rohu stránky hello klientovi Azure AD.
3. V horní nabídce hello zvolte **Azure Active Directory**, klikněte na tlačítko **registrace aplikace**a pak klikněte na tlačítko aplikace hello chcete tooconfigure. To se dostanete stránku rychlý start toohello aplikace, a také otevře okno hello nastavení pro aplikace hello.
4. Klikněte na tlačítko **Manifest** z hello stránky tooopen hello vložené manifestu editoru aplikace. 
5. Uzel "oauth2Permissions" nahraďte hello následujícím fragmentu kódu JSON. Tento fragment kódu je příkladem jak tooexpose obor označuje jako "vystupovat jako jiný uživatel", což umožňuje toogive vlastníka prostředku klientskou aplikaci typ Delegovaný přístup tooa prostředků. Ujistěte se, změnit hello text a hodnoty pro své aplikaci:
   
        "oauth2Permissions": [
        {
            "adminConsentDescription": "Allow hello application full access toohello Todo List service on behalf of hello signed-in     user",
            "adminConsentDisplayName": "Have full access toohello Todo List service",
            "id": "b69ee3c9-c40d-4f2a-ac80-961cd1534e40",
            "isEnabled": true,
            "type": "User",
            "userConsentDescription": "Allow hello application full access toohello todo service on your behalf",
            "userConsentDisplayName": "Have full access toohello todo service",
            "value": "user_impersonation"
            }
        ],
   
    Hodnota id Hello musí být nové vytvářenému identifikátoru GUID, který vytvoříte pomocí [nástroj pro vytváření GUID](https://msdn.microsoft.com/library/ms241442%28v=vs.80%29.aspx) nebo prostřednictvím kódu programu. Představuje jedinečný identifikátor hello oprávnění, který je zveřejněný prostřednictvím hello webového rozhraní API. Jakmile se váš klient je správně nakonfigurovaný toorequest přístup tooyour webové rozhraní API a volání hello webového rozhraní API, nabídne tokenu OAuth 2.0 JWT, který má hello oboru (scp) deklarace identity nastavte toohello hodnotu vyšší, který v tomto případě je user_impersonation.
   
   > [!NOTE]
   > Další obory později podle potřeby můžete vystavit. Zvažte, zda webového rozhraní API mohou být vystaveny víc oborů, které jsou spojené s celou řadu různých funkcí. Nyní můžete řídit přístup toohello webové rozhraní API pomocí hello oboru (scp) deklarace identity v tokenu OAuth 2.0 JWT hello přijata.
   > 
   > 
6. Klikněte na tlačítko **Uložit** toosave hello manifestu. Vaše webová rozhraní API je nyní nakonfigurován toobe používané jinými aplikacemi ve vašem adresáři.

#### <a name="tooverify-hello-web-api-is-exposed-tooother-applications-in-your-directory"></a>tooverify hello webového rozhraní API je zveřejněné tooother aplikací ve vašem adresáři
1. V horní nabídce hello, klikněte na tlačítko **registrace aplikace**vyberte hello potřeby klientská aplikace má tooconfigure přístup toohello webového rozhraní API a přejděte toohello okno nastavení.
2. Z hello **požadovaných oprávnění** vyberte webového rozhraní API hello, který jste právě zveřejněný oprávnění pro. Z rozevírací nabídky hello delegovaná oprávnění vyberte novou oprávnění hello.

![Seznam úkolů oprávnění jsou zobrazeny.](./media/active-directory-integrating-applications/todolistpermissions.png)

#### <a name="more-on-hello-application-manifest"></a>Další informace o manifest aplikace hello
manifest aplikace Hello ve skutečnosti slouží jako mechanismus pro aktualizaci entity hello aplikace, který definuje všechny atributy aplikaci Azure AD identity konfigurace, včetně hello API přístup obory, které jsme se bavili. Další informace o hello entity aplikací, najdete v tématu hello [aplikace Graph API entity dokumentaci](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity). V něm najdete úplný referenční informace o hello toospecify oprávnění členové používá entity aplikací pro vaše rozhraní API:  

* Hello appRoles člena, který je kolekce z [aplikační role](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type) entity, které se dají použít toodefine hello **oprávnění aplikací** pro webové rozhraní API  
* Hello oauth2Permissions člena, který je kolekce z [OAuth2Permission](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type) entity, které se dají použít toodefine hello **delegovaná oprávnění** pro webové rozhraní API

Další informace o aplikaci manifestu koncepty obecně naleznete příliš[manifest aplikace Azure Active Directory hello Principy](active-directory-application-manifest.md).

### <a name="accessing-hello-azure-ad-graph-and-office-365-via-microsoft-graph-apis"></a>Přístup k Azure AD Graph hello a Office 365 přes rozhraní API pro Microsoft Graph  
Jako uvedených starší, kromě tooexposing nebo přístup k rozhraní API v aplikacích prostředků můžete také aktualizovat váš klient aplikace tooaccess rozhraní API vystavené zdrojů společnosti Microsoft.  Hello Microsoft Graph API, která se označuje jako "Microsoft Graph" v seznamu hello oprávnění tooother aplikací, je k dispozici nebo všechny aplikace, které jsou registrovány s Azure AD. Pokud registrujete klientskou aplikaci v rámci služeb Office 365 zřízenou klient Azure AD, můžete také přístup k veškerým hello oprávnění vystavené hello Microsoft Graph API toovarious prostředky Office 365.

Úplné informace o oborů přístupu vystavené Microsoft Graph API, najdete v tématu hello [obory oprávnění | Koncepty Microsoft Graph API](https://graph.microsoft.io/docs/authorization/permission_scopes) článku.

> [!NOTE]
> Kvůli aktuálním omezením tooa nativní klientské aplikace může volat pouze do hello Azure AD Graph API pokud používají oprávnění "Přístup k adresáři vaší organizace" hello.  Toto omezení neplatí pro webové aplikace.
> 
> 

### <a name="configuring-multi-tenant-applications"></a>Konfigurace aplikací pro více klientů
Při přidávání tooAzure aplikaci AD, můžete chtít toobe vaší aplikace přístup jenom uživatelé ve vaší organizaci. Alternativně můžete toobe vaší aplikace přístup uživatelé v externími organizacemi. Tyto typy aplikací se nazývají jednoho klienta a víceklientským aplikacím. Můžete změnit konfiguraci hello toomake aplikace jednoho klienta je víceklientské aplikaci, které tato část popisuje níže.

Je důležité toonote hello rozdíly mezi jednoho klienta a víceklientské aplikace:  

* Aplikace jednoho klienta je určena pro použití v jedné organizaci. Jsou obvykle aplikace obchodní (LoB), vytvořené pomocí vývojář enterprise. Jednoho klienta aplikace potřebuje pouze toobe přístup uživatelé v jednom adresáři, a v důsledku toho je nutné pouze toobe zřídit v jednom adresáři.
* Víceklientské aplikace, který určený pro použití v mnoha organizacích. Jsou webové aplikace software jako služba (SaaS) obvykle zapsaných správcem nezávislý dodavatel softwaru (ISV). Víceklientské aplikace potřebují toobe zřízené v každý adresář, kde se bude používat, který vyžaduje souhlas tooregister uživatel nebo správce je podporována prostřednictvím framework souhlasu hello Azure AD. Upozorňujeme, že všechny nativní klient aplikace jsou víceklientské ve výchozím nastavení, jako jsou nainstalované na zařízení vlastník prostředku hello. Hello přehled hello souhlas Framework části výše další podrobnosti najdete na hello souhlasu framework.

#### <a name="enabling-external-users-toogrant-your-application-access-tootheir-resources"></a>Povolení toogrant externích uživatelů vaší aplikace přístup k tootheir prostředkům
Pokud píšete aplikace mimo organizaci chcete toomake k dispozici tooyour zákazníky nebo partnery, budete potřebovat definice aplikace hello tooupdate v hello portálu Azure.

> [!NOTE]
> Při povolování více klientů, je nutné zajistit, aby identifikátor ID URI aplikace aplikace patří ověřené domény. Kromě toho hello vrátit adresa URL musí začínat https://. Další informace najdete v tématu [objekty aplikací a hlavní objekty služeb](active-directory-application-objects.md).
> 
> 

aplikace tooyour tooenable přístup pro externí uživatele: 

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Zvolte výběrem účtu v hello pravém horním rohu stránky hello klientovi Azure AD.
3. V horní nabídce hello zvolte **Azure Active Directory**, klikněte na tlačítko **registrace aplikace**a pak klikněte na tlačítko aplikace hello chcete tooconfigure. To se dostanete stránku rychlý start toohello aplikace, a také otevře okno hello nastavení pro aplikace hello.
4. V okně Nastavení hello, klikněte na tlačítko **vlastnosti** a flip hello **nevyužívá dělené tabulky více** přepínač příliš**Ano**.

Po provedení hello změnit výše, uživatelů a správců v jiných organizacích, bude možné toogrant adresáři tootheir přístup k vaší aplikace a další data.

#### <a name="triggering-hello-azure-ad-consent-framework-at-runtime"></a>Aktivuje hello Azure AD souhlasu framework za běhu
toouse hello souhlasu framework, víceklientské klientské aplikace musíte požádat o autorizaci s použitím protokolu OAuth 2.0. [Ukázky kódu jsou](https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multi-tenant) jsou k dispozici tooshow jak webová aplikace, nativní aplikace nebo autorizace požadavky aplikací serveru nebo démon kódy a přístup k tokeny toocall webovým rozhraním API.

Webové aplikace může také nabízí registrace prostředí pro uživatele. Pokud nabízíte registrace zkušeností, očekává se, že bude tento uživatel hello klikněte na na přihlašovací tlačítko, bude přesměrování hello prohlížeče toohello Azure AD OAuth2.0 autorizaci koncového bodu nebo koncový bod OpenID Connect informací o uživateli. Tyto koncové body umožňují zkontrolováním požadavku id_token hello hello aplikace tooget informace o hello nového uživatele. Tyto fáze registrace hello hello se uživateli s souhlasu výzva podobné toohello jeden výše uvedeném v hello přehled hello souhlas Framework části.

Alternativně webové aplikace může také nabízí prostředí, které umožňuje správci příliš "zaregistrujte Moje společnost". Toto prostředí by také přesměrovat toohello hello uživatele Azure AD OAuth 2.0 zajistí autorizaci koncového bodu. V takovém případě ale předat výzvu = admin_consent parametr toohello autorizaci koncového bodu tooforce hello správce souhlasu prostředí, kde bude správce hello udělte souhlas jménem své organizace. Jenom uživatel, který ověřuje pomocí účtu, který patří toohello role Globální správce může poskytnout souhlasu; ostatní dojde k chybě. Na úspěšné souhlasu hello odpověď bude obsahovat admin_consent = true. Když uplatňuje přístupový token, budete také zobrazí požadavku id_token, který poskytuje informace o organizaci hello a hello správce, který se zaregistrovali do vaší aplikace.

### <a name="enabling-oauth-20-implicit-grant-for-single-page-applications"></a>Povolení OAuth 2.0 implicitní udělit pro jednostránkové aplikace
Jednu stránku aplikace (SPA) je obvykle strukturovaná s JavaScript náročné front-end, který běží v hello prohlížeč, který volá hello aplikace webového rozhraní API back-end tooperform jeho obchodní logiku. Pro SPA hostované ve službě Azure AD, můžete použít implicitní udělení OAuth 2.0 tooauthenticate hello uživatele s Azure AD a získat token, který můžete použít toosecure volání z aplikace hello JavaScript klienta tooits zpět end webového rozhraní API. Po udělení souhlasu uživatele hello Tento stejný protokol ověřování lze použít tooobtain tokeny toosecure volání mezi klientem hello a dalším prostředkům webové rozhraní API nakonfigurovaný pro aplikaci hello. toolearn Další informace o udělení hello implicitní autorizace a pomáhá můžete rozhodnout, zda je pro váš scénář aplikace správný, najdete v části [Principy hello OAuth2 implicitní tok v Azure Active Directory poskytování ](active-directory-dev-understanding-oauth2-implicit-grant.md).

Ve výchozím nastavení je pro aplikace zakázaná implicitní Grant OAuth 2.0. Můžete povolit implicitní udělení OAuth 2.0 pro aplikaci nastavení hello `oauth2AllowImplicitFlow` hodnotu v jeho [manifest aplikace](active-directory-application-manifest.md), což je soubor JSON, který představuje konfiguraci identity vaší aplikace.

#### <a name="tooenable-oauth-20-implicit-grant"></a>implicitní grant tooenable OAuth 2.0
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Zvolte výběrem účtu v hello pravém horním rohu stránky hello klientovi Azure AD.
3. V horní nabídce hello zvolte **Azure Active Directory**, klikněte na tlačítko **registrace aplikace**a pak klikněte na tlačítko aplikace hello chcete tooconfigure. To se dostanete stránku rychlý start toohello aplikace, a také otevře okno hello nastavení pro aplikace hello.
4. Na stránce aplikace hello, klikněte na tlačítko **Manifest** tooopen hello vložené manifestu editor.
   Vyhledejte a nastavte hodnotu "oauth2AllowImplicitFlow" hello příliš "true". Ve výchozím nastavení je "false".
   
    `"oauth2AllowImplicitFlow": true,`
5. Uložte manifest hello aktualizovat. Po uložení webového rozhraní API je nyní nakonfigurován toouse OAuth 2.0 implicitní Grant tooauthenticate uživatele.


## <a name="removing-an-application"></a>Odebrání aplikace
Tato část popisuje, jak tooremove aplikace ze služby Azure AD klienta.

### <a name="removing-an-application-authored-by-your-organization"></a>Odebrání aplikace vytvořené ve vaší organizaci
Jedná se o hello aplikace, které se zobrazí v části "Vlastní aplikace Moje společnost" hello filtru na hlavní stránce "Aplikace" hello pro vašeho tenanta Azure AD. V technické podmínky ty jsou aplikace, které jste zaregistrovali buď ručně pomocí hello portál Azure classic nebo programově pomocí prostředí PowerShell nebo hello rozhraní Graph API. Přesněji řečeno jsou reprezentované pomocí k aplikaci a instanční objekt objektu v klientovi. V tématu [objekty aplikací a hlavní objekty služeb](active-directory-application-objects.md) Další informace.

#### <a name="tooremove-a-single-tenant-application-from-your-directory"></a>tooremove jednoho klienta aplikace z adresáře
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Zvolte výběrem účtu v hello pravém horním rohu stránky hello klientovi Azure AD.
3. V horní nabídce hello zvolte **Azure Active Directory**, klikněte na tlačítko **registrace aplikace**a pak klikněte na tlačítko aplikace hello chcete tooconfigure. To se dostanete stránku rychlý start toohello aplikace, a také otevře okno hello nastavení pro aplikace hello.
4. Na stránce aplikace hello, klikněte na tlačítko **odstranit**.
5. Klikněte na tlačítko **Ano** v hello potvrzovací zpráva.

#### <a name="tooremove-a-multi-tenant-application-from-your-directory"></a>tooremove víceklientské aplikace z adresáře
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Zvolte výběrem účtu v hello pravém horním rohu stránky hello klientovi Azure AD.
3. V horní nabídce hello zvolte **Azure Active Directory**, klikněte na tlačítko **registrace aplikace**a pak klikněte na tlačítko aplikace hello chcete tooconfigure. To se dostanete stránku rychlý start toohello aplikace, a také otevře okno hello nastavení pro aplikace hello.
4. V okně Nastavení hello, zvolte **vlastnosti** a flip hello **nevyužívá dělené tabulky více** přepínač příliš**ne**. To převede vaší aplikace toobe jednoho klienta, ale aplikace hello stále zůstane v organizaci, který již dá souhlas tooit.
5. Klikněte na hello **odstranit** tlačítko ze stránky aplikace hello.
6. Klikněte na tlačítko **Ano** v hello potvrzovací zpráva.

### <a name="removing-a-multi-tenant-application-authorized-by-another-organization"></a>Odebráním víceklientské aplikace, autorizovat jiné organizaci
Jedná se o podmnožinu hello aplikace, které se zobrazí v části "Aplikace společnost používá" hello filtr na stránce hlavní "aplikace" hello pro vašeho tenanta Azure AD, konkrétně hello ty, které nejsou uvedené v seznamu "Vlastní aplikace Moje společnost" hello. V technické podmínky jedná se o víceklientské aplikace během procesu souhlasu hello zaregistrovány. Přesněji řečeno jsou reprezentované pomocí pouze objekt zabezpečení služby ve vašem klientovi. V tématu [objekty aplikací a hlavní objekty služeb](active-directory-application-objects.md) Další informace.

V pořadí tooremove directory tooyour přístup k aplikaci víceklientské (po s udělen souhlas), správce společnosti hello musí mít předplatnému Azure tooremove přístup přes hello portálu Azure. Správce společnosti hello můžete alternativně použít hello [rutin prostředí Azure AD PowerShell](http://go.microsoft.com/fwlink/?LinkId=294151) tooremove přístup.

## <a name="next-steps"></a>Další kroky
* V tématu hello [Branding pokyny pro integrované aplikace](active-directory-branding-guidelines.md) tipy na visual pokyny pro vaši aplikaci.
* Další informace o hello vztah mezi objekty aplikace a služby hlavní aplikace v [objekty aplikací a hlavní objekty služeb](active-directory-application-objects.md).
* toolearn Další informace o hello role hello aplikace plní manifestu, najdete v části [manifest aplikace Azure Active Directory Principy hello](active-directory-application-manifest.md)
* V tématu hello [Glosář vývojáře Azure AD](active-directory-dev-glossary.md) definice některých koncepty pro vývojáře Azure Active Directory (AD) hello jádra.
* Navštivte hello [Příručka pro vývojáře Active Directory](active-directory-developers-guide.md) přehled všechny vývojáře související obsah.

