---
title: "aaaHow toobuild aplikaci, která se můžete přihlásit žádné uživatele Azure AD | Microsoft Docs"
description: "Podrobné pokyny pro vytváření aplikace, které můžou přihlásit uživatel jakéhokoli klienta Azure Active Directory, také známé jako aplikaci na více klientů."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 35af95cb-ced3-46ad-b01d-5d2f6fd064a3
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/26/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 123ea8125fa3c308ce0f124cc58e85ec28d476d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosign-in-any-azure-active-directory-ad-user-using-hello-multi-tenant-application-pattern"></a>Jak toosign v jakékoli pomocí uživatele Azure Active Directory (AD) hello vzor víceklientské aplikací
Pokud nabízíte softwaru jako toomany aplikace služby organizace, můžete nakonfigurovat vaše aplikace tooaccept přihlášení jakéhokoli klienta Azure AD.  Ve službě Azure AD se nazývá provedení víceklientské vaší aplikace.  Budou uživatelé v jakékoli klientovi Azure AD může toosign v aplikaci tooyour po souhlas toouse svůj účet s vaší aplikací.  

Pokud máte existující aplikaci, která má svůj vlastní účet systému nebo jiných druhů přihlášení od jiných poskytovatelů cloudu podporuje, klepněte přidání Azure AD přihlášení jakéhokoli klienta je jednoduché. Právě svou aplikaci zaregistrovat, přidejte kód přihlášení přes OAuth2, OpenID Connect nebo SAML a umístí tlačítko "Přihlášení v with Microsoft" vaší aplikace. Klikněte na následující tlačítko toolearn více informací o branding vaší aplikace hello.

[! [Přihlašovací tlačítko] [AAD-Sign-In]][AAD-App-Branding]

Tento článek předpokládá, že jste již obeznámeni s vytvoření jednoho klienta aplikace pro Azure AD.  Pokud si nejste, head, zálohujte toohello [domovské stránce průvodce vývojáře] [ AAD-Dev-Guide] a vyzkoušet některý z našich rychlé zahájení práce!

Existují čtyři jednoduchých kroků tooconvert vaší aplikace do více klientů aplikace Azure AD:

1. Aktualizovat vaše aplikace registrace toobe víceklientské
2. Aktualizovat/Common váš kód toosend požadavky toohello koncový bod 
3. Aktualizovat toohandle váš kód více hodnot vystavitele
4. Rady pro pochopení souhlasu uživatele a správce a proveďte změny správný kód

Podívejme se na každý krok podrobně. Můžete také přejít rovnou příliš[tento seznam víceklientské ukázky][AAD-Samples-MT].

## <a name="update-registration-toobe-multi-tenant"></a>Aktualizace registrace toobe více klientů
Ve výchozím nastavení jsou webové aplikaci nebo API registrace ve službě Azure AD jednoho klienta.  Můžete provést registrace víceklientské tak, že najdete na stránce vlastností hello registrace vaší aplikace v hello hello "více nevyužívá dělené tabulky" přepínač [portál Azure] [ AZURE-portal] a jeho nastavení příliš "Ano".

Všimněte si také, než aplikace můžete provedeny více klientů Azure AD vyžaduje hello identifikátor ID URI aplikace z toobe aplikace hello globálně jedinečný. Hello identifikátor ID URI aplikace je jedním ze způsobů hello, které aplikace je definována ve zprávách protokolu.  Aplikace pomocí jednoho klienta je dostačující pro toobe identifikátor ID URI aplikace hello jedinečné v rámci tohoto klienta.  Pro více klientů aplikace musí být globálně jedinečné, Azure AD můžete najít aplikaci hello napříč všichni klienti.  Globální jedinečnosti se vynucuje vyžadováním toohave identifikátor ID URI aplikace hello název hostitele, který odpovídá ověřené domény klienta Azure AD hello.  Například pokud hello název vašeho klienta se contoso.onmicrosoft.com pak platný identifikátor ID URI aplikace by být `https://contoso.onmicrosoft.com/myapp`.  Pokud váš klient měli ověřené domény `contoso.com`, pak by také být platný identifikátor ID URI aplikace `https://contoso.com/myapp`.  Nastavení aplikace jako víceklientské se nezdaří, pokud hello identifikátor ID URI aplikace není postupujte podle tohoto vzoru.

Registrace nativního klienta jsou víceklientské ve výchozím nastavení.  Tootake není třeba žádné akce toomake více klientů nativního klienta aplikace registrace.

## <a name="update-your-code-toosend-requests-toocommon"></a>Aktualizace vašeho kódu toosend požadavky příliš/společné
V aplikaci jednoho klienta žádostí o přihlášení se odesílají klienta toohello přihlášení koncový bod. Například pro contoso.onmicrosoft.com by hello koncový bod:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

Odeslání požadavků, že koncový bod tooa tenant můžete přihlásit uživatele (nebo Hosté) v tomto tooapplications klienta v něm.  S více klienty aplikací, aplikace hello neví předem hello uživatele klienta je, aby nemohli odesílat požadavky klienta tooa koncový bod.  Místo toho jsou odeslány požadavky tooan koncový bod, který multiplexes napříč všechny klienty Azure AD:

    https://login.microsoftonline.com/common

Pokud Azure AD přijme požadavek na hello/běžné koncový bod, se přihlásí uživatel hello a v důsledku zjišťuje, které uživatel hello klienta je z.  Hello/společný koncový bod funguje se všemi hello ověřovací protokoly podporovaný službou Azure AD: OpenID Connect, OAuth 2.0, SAML 2.0 a WS-Federation.

Hello přihlášení odpovědi toohello aplikace pak obsahuje token představující uživatele hello.  Hodnota vystavitele Hello v tokenu hello informuje aplikace hello uživatele klienta je z.  Pokud vrátí odpověď z hello nebo běžné koncový bod, hodnota vystavitele hello v tokenu hello bude odpovídat toohello uživatele klienta.  Je důležité toonote hello/společný koncový bod není klienta a není vystavitele, je právě multiplexor.  Při použití/Common, hello logiku v tokenech toovalidate vaše aplikace potřebuje toobe aktualizovat tootake to v úvahu. 

Jako dříve uvedených, víceklientské aplikace by měl poskytovat také konzistentního prostředí přihlášení pro uživatele, následující hello branding pokyny aplikaci Azure AD. Klikněte na následující tlačítko toolearn více informací o branding vaší aplikace hello.

[! [Přihlašovací tlačítko] [AAD-Sign-In]][AAD-App-Branding]

Podívejme se na použití hello/Common hello koncový bod a implementaci kódu podrobněji.

## <a name="update-your-code-toohandle-multiple-issuer-values"></a>Aktualizovat toohandle váš kód více hodnot vystavitele
Webové aplikace a webové rozhraní API přijímat a ověřovat tokeny z Azure AD.  

> [!NOTE]
> Nativní klientské aplikace požadovat a přijímat tokeny z Azure AD, dělají Ano toosend je tooAPIs, kde se ověří.  Nativní aplikace neověřují tokeny a musí je považovat za neprůhledné.
> 
> 

Podívejme se na tom, jak aplikaci ověří tokeny obdrží z Azure AD.  Jednoho klienta aplikace bude obvykle trvat hodnotu koncového bodu jako:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

a použít ho tooconstruct jako adresu URL metadat (v tomto případě OpenID Connect):

    https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration

toodownload dva zásadní informace, které jsou používané toovalidate tokeny: hello klienta podepisovací klíče a hodnoty vystavitele.  Každý klient Azure AD má hodnotu jedinečný vystavitele hello formuláře:

    https://sts.windows.net/31537af4-6d77-4bb9-a681-d2394888ea26/

kde je hodnota GUID hello hello přejmenování bezpečnou verzi ID klienta hello hello klienta.  Pokud kliknete na hello předcházející odkaz metadata pro `contoso.onmicrosoft.com`, zobrazí se tato hodnota vystavitele v dokumentu hello.

Když aplikace jednoho klienta ověří token, zkontroluje podpis hello hello tokenu proti hello podpisových klíčů z dokumentu metadat hello. To umožňuje ho toomake zda hello vystavitele hodnotu v hello tokenu odpovídá hello, jeden a který byl nalezen v dokumentu metadat hello.

Od hello/běžný koncový bod neodpovídá tooa klienta a není vystavitele, při zkontrolujte hodnotu vystavitele hello hello metadat pro/běžné má podle šablony URL namísto skutečné hodnoty:

    https://sts.windows.net/{tenantid}/

Proto víceklientské aplikace nemůže ověřit tokeny pouhým odpovídající hodnotě vystavitele hello v metadatech hello s hello `issuer` hodnotu v tokenu hello.  Víceklientské aplikace potřebuje logiku toodecide vystavitele hodnot, které jsou platné a které jsou není, podle klienta hello ID část hodnoty vystavitele hello.  

Například víceklientské aplikace umožňuje pouze přihlášení z konkrétní klientů, kteří zaregistrovali pro své služby, potom je nutné zkontrolovat hodnotě vystavitele hello nebo hello `tid` hodnoty v tokenu toomake hello se, že klienta je v jejich seznamu deklarace identity Odběratelé, kteří.  Pokud aplikace víceklientské pouze se zabývá jednotlivce a není rozhodnutí žádné přístupu na základě na klienty, potom ji můžete ignorovat hodnotě vystavitele hello zcela.

V hello víceklientské ukázky v hello [související obsahu](#related-content) oddíl hello konci tohoto článku, vystavitele ověřování je zakázáno tooenable žádné toosign klienta Azure AD v.

Nyní Podíváme se na hello uživatelské prostředí pro uživatele, kteří se přihlašují toomulti klientské aplikace.

## <a name="understanding-user-and-admin-consent"></a>Principy uživatelů a správce souhlasu
Pro uživatele toosign v tooan aplikaci ve službě Azure AD musí být aplikace hello reprezentován v klientovi hello uživatele.  To umožňuje hello organizace toodo akce, jako je po přihlášení uživatelé z jejich klienta toohello aplikaci použít jedinečný zásady.  Pro jednoho klienta aplikace je tato registrace jednoduché. má hello, ten, který se stane při registraci aplikace hello v hello [portál Azure][AZURE-portal].

Aplikace pomocí víceklientské hello počáteční registrace pro aplikace hello žije v klientovi Azure AD hello používá hello developer.  Pokud se uživatel z jiné klienta přihlásí v aplikaci toohello hello poprvé, Azure AD je požádá tooconsent toohello oprávnění požadovaná aplikace hello.  Pokud budou souhlasit, pak volat znázornění hello aplikace *instanční objekt* je vytvořen v hello uživatele klienta a přihlášení může pokračovat. Delegování je vytvořen také v hello adresář, který zaznamenává aplikaci toohello souhlasu hello uživatele. V tématu [objekty aplikací a hlavní objekty služeb] [ AAD-App-SP-Objects] podrobnosti o aplikace hello aplikace a ServicePrincipal objekty a jejich vzájemných tooeach jiné.

![Aplikace toosingle úroveň souhlasu][Consent-Single-Tier] 

Toto prostředí souhlasu je ovlivňován hello oprávnění požadovaná aplikace hello.  Azure AD podporuje dva druhy oprávnění jenom aplikace a delegovaným:

* Delegovaná oprávnění uděluje tooact možnost hello aplikaci, můžete stejně jako přihlášeného uživatele pro podmnožinu hello věcí hello uživatele.  Například můžete udělit aplikace hello delegovaná oprávnění tooread hello přihlášení uživatele kalendáře.
* Jen aplikace je povoleno přímo toohello identitu aplikace hello.  Například můžete udělit jenom aplikace hello oprávnění jen aplikace tooread hello seznam uživatelů v klientovi, bez ohledu na to, kdo je přihlášený toohello aplikace.

Některá oprávnění může být svolení tooby běžný uživatel, zatímco jiné vyžadují souhlas správce klienta. 

### <a name="admin-consent"></a>Správce souhlasu
Oprávnění jen aplikace vždy vyžadovat souhlas správce klienta.  Pokud vaše aplikace požaduje oprávnění jen aplikace a uživatel se pokusí toosign v aplikaci toohello, chybová zpráva zobrazí se, že uživatel hello není možné tooconsent.

Některé přidělená oprávnění také vyžadovat souhlas správce klienta.  Například hello možnost toowrite back tooAzure AD jako hello přihlášený uživatel vyžaduje souhlas správce klienta.  Například oprávnění jen pro aplikace Pokud běžného uživatele pokusí toosign v tooan aplikace, která požaduje delegované oprávnění, která vyžaduje souhlas správce vaší aplikace dojde k chybě.  Zda vyžaduje oprávnění správce souhlas je dáno hello vývojáře, která publikuje hello prostředků, může být v dokumentaci hello k hello prostředků.  Odkazy tootopics popisující hello dostupná oprávnění pro hello Azure AD Graph API a rozhraní Microsoft Graph API jsou v hello [související obsahu](#related-content) tohoto článku.

Pokud vaše aplikace používá oprávnění, která vyžadovat souhlas správce, musíte toohave gesto například tlačítko nebo odkaz, kde Dobrý den, správce můžete zahájit hello akce.  žádost o Hello vaše aplikace odešle pro tato akce je obvykle požadavek ověřování OAuth2/OpenID Connect, ale který také obsahuje hello `prompt=admin_consent` parametr řetězce dotazu.  Jakmile se dá souhlas Dobrý den, správce a hello instanční objekt se vytvoří v klientovi hello zákazníka, následných žádostí o přihlášení není nutné hello `prompt=admin_consent` parametr. Vzhledem k tomu, že správce hello se rozhodla, že hello požadovaná oprávnění jsou přijatelné, žádné jiné uživatelům v klientovi hello se zobrazí výzva k vyjádření souhlasu od tohoto okamžiku.

Hello `prompt=admin_consent` parametru lze také aplikace, které žádostí o oprávnění, které nevyžadují souhlas správce. To se provádí při hello aplikace vyžaduje prostředí kde Dobrý den, správce klienta "zaregistruje" jednu čas a žádné jiné uživateli zobrazí výzva k vyjádření souhlasu od tohoto okamžiku na.

Pokud aplikace vyžaduje souhlas správce a správce přihlášení v ale hello `prompt=admin_consent` parametr neposílají, dobrý den, správce se úspěšně souhlas toohello aplikace **pouze ke svému uživatelskému účtu**.  Běžní uživatelé stále nebude možné toosign v a aplikace toohello souhlasu.  To je užitečné, pokud chcete toogive hello klienta správce hello možnost tooexplore aplikace před povolením přístupu jiných uživatelů.

Správce klienta můžete zakázat možnost hello tooapplications tooconsent běžní uživatelé.  Pokud tato možnost je vypnuta, je vyžadováno pro toobe aplikace hello nastavit v klientovi hello vždy souhlas správce.  Pokud chcete tootest vaší aplikace pomocí běžných uživatelských souhlasu zakázaná, můžete najít hello konfigurace přepínače v klientovi Azure AD hello konfigurační oddíl hello [portál Azure][AZURE-portal].

> [!NOTE]
> Některé aplikace mají prostředí běžní uživatelé jsou možné tooconsent původně, kde novější hello aplikace může zahrnovat hello správce a oprávnění k žádostem vyžadující souhlas správce.  Neexistuje žádný způsob, jak toodo to s registrací jednu aplikaci ve službě Azure AD ještě dnes.  koncový bod v2 Hello nadcházející Azure AD vám umožní aplikace toorequest oprávnění za běhu, místo v době registrace, která vám umožní tento scénář.  Další informace najdete v tématu hello [Příručka vývojáře v2 modelu aplikace Azure AD][AAD-V2-Dev-Guide].
> 
> 

### <a name="consent-and-multi-tier-applications"></a>Souhlasu a vícevrstvé aplikace
Aplikace může mít několik vrstev, každý reprezentován vlastní registraci ve službě Azure AD.  Nativní aplikace, která volá webové rozhraní API nebo webovou aplikaci, například volání webového rozhraní API.  V obou případech hello klient (nativní aplikaci nebo webové aplikace) požaduje oprávnění toocall hello prostředků (webového rozhraní API).  Pro klienta toobe hello dá souhlas úspěšně do klienta zákazníka, musí všechny prostředky toowhich požaduje oprávnění již existují v klientovi hello zákazníka.  Pokud není tato podmínka splněná, Azure AD vrátí chybu, která hello prostředků musí být přidán jako první.

**Několik vrstev v jednoho klienta**

Může to být problém, pokud vaše logické aplikace se skládá ze dvou nebo více aplikací registrace, například klienta a prostředků.  Jak můžete získat hello prostředků do klienta zákazníka hello první?  Azure AD vysvětluje tento případ povolením klienta a toobe prostředků dá souhlas v jediném kroku. Hello uživateli se zobrazí celkový součet hello hello oprávnění požadoval hello klienta a prostředků na stránku souhlasu hello.  tooenable toto chování registrace aplikace hello prostředků musí obsahovat ID aplikace hello klienta jako `knownClientApplications` v manifestu aplikace.  Například:

    knownClientApplications": ["94da0930-763f-45c7-8d26-04d5938baab2"]

Tuto vlastnost lze aktualizovat prostřednictvím hello prostředků [manifestu aplikace][AAD-App-Manifest]. Tento postup je znázorněn v vícevrstvé nativního klienta volání ukázkové webové rozhraní API v hello [související obsahu](#related-content) oddíl hello konci tohoto článku. Hello následující diagram přehledně souhlasu pro vícevrstvé aplikace s zaregistrovaný v jednoho klienta:

![Souhlas toomulti vrstvě známé klientské aplikace][Consent-Multi-Tier-Known-Client] 

**Několik vrstev v několika klientech**

Podobný případ se stane, když jsou v jiných klientů zaregistrované hello různých vrstev aplikace.  Představte si třeba hello případ vytváření nativní klientskou aplikaci, která volá hello rozhraní API Office 365 Exchange Online.  toodevelop hello nativní aplikaci a novějším pro toorun nativní aplikace hello v klientovi zákazníka, hello Exchange Online instanční objekt musí být k dispozici.  V takovém případě hello developer a k zákaznickým, musí si koupit Exchange Online pro hello služby hlavní toobe vytvořené v svým klientům.  

V případě hello rozhraní API vytvořené v organizaci než Microsoft musí vývojář hello hello rozhraní API tooprovide způsob, jak pro své zákazníky tooconsent hello aplikaci do klientů svým zákazníkům. Hello doporučujeme návrhu je pro hello 3. stran vývojáře toobuild hello rozhraní API, tak, aby také může fungovat jako tooimplement webového klienta, registrace:

1. Postupujte podle hello dřívější části tooensure hello API implementuje požadavky na registraci nebo kód víceklientské aplikace hello
2. Kromě toho tooexposing hello rozhraní API obory nebo role, ujistěte se registrace hello zahrnuje hello "přihlášení a čtení profilu uživatele" oprávnění Azure AD (poskytuje se ve výchozím nastavení)
3. Implementovat sign v nebo registrace-množství stránku hello webovému klientovi, následující hello [souhlas správce](#admin-consent) pokyny uvedenými výše 
4. Jakmile hello uživatel souhlasí toohello aplikace, hello service principal a souhlasu delegování odkazy jsou vytvořené v jejich klienta a nativní aplikace hello mohou získat tokeny pro hello rozhraní API

Následující diagram Hello poskytuje přehled o souhlas pro vícevrstvé aplikace s zaregistrovaný v jiných klientů:

![Více stran aplikace toomulti úroveň souhlasu][Consent-Multi-Tier-Multi-Party] 

### <a name="revoking-consent"></a>Odvolání souhlasu
Uživatelé a správci můžete odvolat souhlas tooyour aplikace kdykoli:

* Uživatelé odvolat přístup k tooindividual aplikacím tím, že odebere je ze své [přístup k aplikacím panely] [ AAD-Access-Panel] seznamu.
* Správci odvolat přístup tooapplications je odstranit z Azure AD pomocí hello Azure AD management části hello [portál Azure][AZURE-portal].

Pokud správce souhlasí tooan aplikací pro všechny uživatele v klientovi, uživatelé nelze odvolat přístup jednotlivě.  Pouze správce hello můžete odvolat přístup a jenom pro celý aplikace hello.

### <a name="consent-and-protocol-support"></a>Souhlasu a podpora protokolu
Souhlas se podporuje ve službě Azure AD prostřednictvím hello OAuth, OpenID Connect, WS-Federation a protokoly SAML.  Hello protokoly SAML a WS-Federation nepodporují hello `prompt=admin_consent` parametr, tak je možné pomocí OAuth a OpenID Connect jenom souhlas správce.

## <a name="multi-tenant-applications-and-caching-access-tokens"></a>Víceklientské aplikace a ukládání do mezipaměti přístupové tokeny
Víceklientským aplikacím můžete také získat tokeny toocall přístup k rozhraní API, které jsou chráněné službou Azure AD.  Běžnou chybou při hello Active Directory Authentication Library (ADAL) pomocí víceklientské aplikace, je požadavek tooinitially token pro uživatele s využitím/Common, obdrží odpověď a pak požádat o další token pro tomuto uživateli také pomocí/Common.  Vzhledem k tomu, že hello odpověď z Azure AD pochází z klienta, není nebo běžné ADAL token hello jako pocházející z klienta hello ukládá do mezipaměti. Hello následných volání tooget příliš/společné, přístupový token pro položku mezipaměti hello uživatele neúspěchy hello a hello uživatele je výzvami toosign v akci.  tooavoid chybějící hello mezipaměti, ověřte zda následující volání pro již přihlášeného uživatele jsou vytvářeny koncový bod toohello tenant.

## <a name="next-steps"></a>Další kroky
V tomto článku jste se naučili jak toobuild aplikace, která se může přihlásit uživatel jakéhokoli klienta Azure Active Directory. Po povolení jednotného přihlašování mezi aplikací a Azure Active Directory, můžete také aktualizovat vaše aplikace tooaccess rozhraní API vystavené zdroje společnosti Microsoft, jako je Office 365. Přizpůsobené rozhraní můžete tak nabízet ve vaší aplikaci, například zobrazení kontextové informace toohello uživatelů, jako je jejich profilový obrázek nebo jejich další událost kalendáře. toolearn Další informace o vytváření rozhraní API, zavolá tooAzure služby Active Directory a služby Office 365, jako je Exchange, SharePoint, OneDrive, OneNote, Planner, Excel a další, navštivte: [Microsoft Graph API][MSFT-Graph-overview].


## <a name="related-content"></a>Související obsah
* [Ukázky víceklientské aplikace][AAD-Samples-MT]
* [Branding pokyny pro aplikace][AAD-App-Branding]
* [Průvodce Azure AD vývojáře][AAD-Dev-Guide]
* [Objekty aplikací a hlavní objekty služeb][AAD-App-SP-Objects]
* [Integrace aplikací s Azure Active Directory][AAD-Integrating-Apps]
* [Přehled hello souhlas Framework][AAD-Consent-Overview]
* [Obory oprávnění Microsoft Graph API][MSFT-Graph-permision-scopes]
* [Obory oprávnění rozhraní Azure AD Graph API][AAD-Graph-Perm-Scopes]

Použijte hello následující komentáře části tooprovide zpětnou vazbu a Pomozte nám vylepšit a utvářejí náš obsah.

<!--Reference style links IN USE -->
[AAD-Access-Panel]:  https://myapps.microsoft.com
[AAD-App-Branding]: ./active-directory-branding-guidelines.md
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Consent-Overview]: ./active-directory-integrating-applications.md#overview-of-the-consent-framework
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Overview]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-graph-api/
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Samples-MT]: https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multitenant
[AAD-Why-To-Integrate]: ./active-directory-how-to-integrate.md
[AZURE-portal]: https://portal.azure.com
[MSFT-Graph-overview]: https://graph.microsoft.io/en-us/docs/overview/overview
[MSFT-Graph-permision-scopes]: https://graph.microsoft.io/en-us/docs/authorization/permission_scopes

<!--Image references-->
[AAD-Sign-In]: ./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png
[Consent-Single-Tier]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-single-tier.png
[Consent-Multi-Tier-Known-Client]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-known-clients.png
[Consent-Multi-Tier-Multi-Party]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-multi-party.png

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AAD-V2-Dev-Guide]: ../active-directory-appmodel-v2-overview.md
[AZURE-portal]: https://portal.azure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Code-Grant-Flow]: https://msdn.microsoft.com/library/azure/dn645542.aspx
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3 
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken














