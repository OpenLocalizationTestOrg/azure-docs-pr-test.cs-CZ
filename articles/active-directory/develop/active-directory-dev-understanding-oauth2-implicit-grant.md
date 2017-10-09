---
title: "hello aaaUnderstanding OAuth2 implicitní tok poskytování ve službě Azure AD | Microsoft Docs"
description: "Další informace o službě Azure Active Directory implementace hello OAuth2 grant implicitní tok, a jestli je pro vaši aplikaci nejvhodnější."
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 90e42ff9-43b0-4b4f-a222-51df847b2a8d
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/15/2016
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 3402e6619709e1a5b1e790ffd79dc62139552d9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-hello-oauth2-implicit-grant-flow-in-azure-active-directory-ad"></a>Principy hello OAuth2 implicitní udělit toku v Azure Active Directory (AD)
implicitní grant Hello OAuth2 je notorious pro probíhá hello grant s hello nejdelší seznam zabezpečení se specifikací OAuth2 hello. A ještě, který je implementované ADAL JS a hello jeden, doporučujeme při psaní aplikací SPA přístup hello. Co nabízí? Je všechny řádu kompromisy: jak se jím implicitní grant hello se hello nejlepším postupem můžete pokračovat pro aplikace, které využívají webové rozhraní API prostřednictvím jazyka JavaScript v prohlížeči.

## <a name="what-is-hello-oauth2-implicit-grant"></a>Co je implicitní grant hello OAuth2?
Hello quintessential [udělení autorizačního kódu OAuth2](https://tools.ietf.org/html/rfc6749#section-1.3.1) je udělení hello autorizace, který používá dva samostatné koncové body. koncový bod autorizace Hello se používá pro hello uživatelské interakce fáze, což vede k autorizační kód. koncový bod tokenu Hello se pak použije hello klienta pro výměnu hello kód pro přístupový token a často token obnovení. Webové aplikace jsou požadované toopresent svoje vlastní přihlašovací údaje toohello tokenu koncový bod aplikace, tak, aby hello serveru ověřování můžete ověřit hello klienta.

Hello [OAuth2 implicitní grant](https://tools.ietf.org/html/rfc6749#section-1.3.2) je varianta jiných uděluje autorizace. Umožňuje klienta tooobtain přístupový token (a požadavku id_token, při použití [OpenId Connect](http://openid.net/specs/openid-connect-core-1_0.html)) přímo z koncového bodu autorizace hello, bez kontaktování koncový bod tokenu hello ani ověřování klienta hello. Tento typ variant byl speciálně pro JavaScript na základě aplikací, které běží ve webovém prohlížeči: OAuth2 specifikací původní hello tokeny jsou vráceny ve fragmentu identifikátor URI. Který tokenu bits hello k dispozici toohello kódu jazyka JavaScript v klientovi hello se však zaručuje, že se nebudou zahrnuty do přesměruje na hello server. Vrácení tokeny přes prohlížeč přesměruje přímo z koncového bodu autorizace hello. Je také výhod hello odstraňuje všechny požadavky pro křížové počátek volání, které jsou nutné v případě hello JavaScript aplikace je požadovaná toocontact koncový bod tokenu hello.

Důležitou vlastností objektu implicitní grant hello OAuth2 je hello skutečnost, že tyto toky nikdy se nevrací aktualizace tokeny toohello klienta. Jak vidíme v další části hello, který není nezbytně nutné a bude ve skutečnosti porušení zabezpečení.

## <a name="suitable-scenarios-for-hello-oauth2-implicit-grant"></a>Poskytnout vhodný scénáře pro hello implicitní OAuth2
Podle specifikace hello OAuth2, samotné deklaruje, hello implicitní grant byl připravit tooenable uživatelského agenta aplikace – které je toosay, JavaScript aplikace spouštění v prohlížeči. Hello definování typické pro takové aplikace je, že kód jazyka JavaScript se používá pro přístup k prostředkům serveru (obvykle Web API) a pro aktualizaci aplikace hello UX odpovídajícím způsobem. Představte si, že aplikace, jako je z Gmailu nebo Outlook Web Access: Když vyberete zprávy z vaší doručené poště, pouze uvítací zprávu vizualizace panel změní výběr, nové toodisplay hello při hello zbytek stránku hello zůstává beze změny. To je rozdíl od tradičních na základě přesměrování webové aplikace, kde každý interakci s uživatelem výsledkem zpětné volání celou stránku a vykreslování celou stránku hello nové odpovědi serveru.

Aplikace, které potřebují hello založené na jazyce JavaScript přístup tooits extrémně se nazývají jednostránkové aplikace nebo SPA: hello Rada je, že tyto aplikace jenom sloužit úvodní stránce HTML a přidružené JavaScript, s všechny následné interakce se řídí Volání webového rozhraní API se provádí prostřednictvím jazyka JavaScript. Ale hybridní přístupy, kde aplikace hello je ve většině případů řízené zpětné volání, ale provádí volání příležitostně JS, nejsou – hello diskuzi o využití implicitní tok je relevantní pro ty také.

Aplikace založené na přesměrování obvykle zabezpečit jejich požadavky pomocí souborů cookie, ale které přístup nefunguje také pro aplikace JavaScript. Soubory cookie fungovat pouze proti hello domény, které budou byly vygenerovány pro, při volání JavaScriptu může být zaměřený na jiných domén. Ve skutečnosti, které často bude případ hello: Představte si, že aplikace volání rozhraní API Microsoft Graph API, rozhraní API Office, Azure – všechny umístěný mimo doménu hello z kde obsluhovaného aplikace hello. Rostoucí trend pro aplikace JavaScript je toohave žádné back-end, předávající 100 % na 3. stran webovým rozhraním API tooimplement funkce své firmy.

V současné době hello upřednostňovanou metodu ochrany tooa volání webového rozhraní API je toouse hello OAuth2 nosiče tokenu přístup, kde každé volání je přiložena přístupový token OAuth2. Hello webového rozhraní API prozkoumá hello příchozí přístupový token, a pokud najde v jeho hello nezbytné obory, udělí přístup toohello požadovanou operaci. implicitní tok Hello poskytuje vhodný mechanismus pro aplikace JavaScript tooobtain přístupových tokenů pro webového rozhraní API nabízející že řadu výhod v respektují toocookies:

* Tokeny spolehlivě získáte bez nutnosti křížové volání počátek – povinné registrace toowhich identifikátor URI přesměrování hello tokeny jsou vrátit zaručuje, že nejsou posunout tokeny
* JavaScript aplikací můžete získat podle potřeby, pro tolik webové rozhraní API se cíle – bez omezení v doménách přístupové tokeny
* Funkce jazyka HTML5, jako jsou relace nebo místní úložiště poskytují plnou kontrolu nad ukládání tokenu do mezipaměti a správu životního cyklu, zatímco Správa souborů cookie je neprůhledné toohello aplikace
* Přístupové tokeny nejsou náchylné tooCross požadavek padělání (proti útokům CSRF) útoky

tok implicitní grant Hello nevydává tokeny obnovení, většinou z bezpečnostních důvodů. Token obnovení není v jako přístupových tokenů, udělení podstatně více power proto inflicting podstatně více poškození v případě, že je neuniknou jako úzce oboru. V hello implicitní tok tokeny se dodávají v adrese URL hello, proto je vyšší než v udělení autorizačního kódu hello hello riziko zachycení.

Uvědomte si však, že aplikace JavaScript má jiný mechanismus k dispozici pro obnovení přístupových tokenů bez opakovaného výzvy hello uživatele pro přihlašovací údaje. Hello aplikace můžete použít skrytá iframe tooperform nové žádosti o tokeny hello koncový bod autorizace služby Azure AD: tak dlouho, dokud hello prohlížeče má stále aktivní relace (čtení: obsahuje soubor cookie relace) vůči doméně hello Azure AD, hello žádosti o ověření může dojít úspěšně bez nutnosti zásahu uživatele.

Tento model umožňuje hello JavaScript aplikace hello tooindependently obnovte přístupové tokeny a i získat nové pro nové rozhraní API (za předpokladu, že hello uživatele dříve dá souhlas pro ně. Tím je zabráněno hello přidané zatížení získávání, Správa a ochrana artefaktů vysoké hodnoty například token obnovení. Hello artefaktů, která umožňuje tichou obnovení hello souboru cookie relace hello Azure AD, se spravuje mimo aplikace hello. Další výhodou tohoto přístupu je, že uživatel může odhlásit se ze služby Azure AD pomocí některé z aplikací hello přihlášení k Azure AD, spuštěná v jakémkoli hello záložkách prohlížeče. Výsledkem je odstranění hello souboru cookie relace hello Azure AD, a hello aplikace JavaScript automaticky ztratíte možnost hello toorenew tokeny pro hello odhlášení uživatele.

## <a name="is-hello-implicit-grant-suitable-for-my-app"></a>Implicitní grant hello je vhodný pro mé aplikace?
implicitní grant Hello představuje další rizik než ostatní uděluje a oblasti hello potřebujete tooare pozornost toopay dobře zdokumentovat. Například [zneužití z aplikaci přístup k tokenu tooImpersonate vlastníka prostředku v implicitní tok] [ OAuth2-Spec-Implicit-Misuse] a [Model hrozeb OAuth 2.0 a aspekty zabezpečení] [ OAuth2-Threat-Model-And-Security-Implications]). Ale hello vyšší riziko profil je z velké části z důvodu toohello skutečnost, že je určená tooenable aplikace, které spustit kód s aktivní, obsloužených prohlížeče tooa vzdálený prostředek. Pokud chcete mít žádné součásti back-end SPA architekturu, nebo chcete tooinvoke webového rozhraní API prostřednictvím jazyka JavaScript, se doporučuje použít hello implicitní tok pro získání tokenu.

Pokud vaše aplikace je nativní klient, není implicitního toku hello skvělé přizpůsobit. Hello absenci souboru cookie relace hello Azure AD v kontextu hello nativního klienta zbaví aplikace hello prostředkem k zachování dlouho povahy relace. To znamená, vaše aplikace opakovaně hello uživatel vyzván při získání přístupových tokenů pro nové prostředky.

Pokud vyvíjíte webovou aplikaci, která zahrnuje back-end a využívání rozhraní API z jeho back-end kód, hello implicitní tok je také vhodné. Další uděluje získáte mnohem více energie. Například udělení pověření klienta hello OAuth2 poskytuje hello možnost tooobtain tokeny, které odráží hello oprávnění přiřazená toohello vlastní aplikace, jako názvem na rozdíl od toouser delegování. To znamená, že má klient hello hello možnost toomaintain programový přístup tooresources i v případě, že uživatel není aktivně zapojený do relace a tak dále. Nejenom, ale tyto uděluje poskytují vyšší záruky zabezpečení. Pro instanci přístupové tokeny nikdy přenosu přes prohlížeč hello uživatel, nemáte rizik se uloží v historii prohlížeče hello a tak dále. klientská aplikace Hello můžete také provést silné ověřování, žádosti o token.

## <a name="next-steps"></a>Další kroky
* Úplný seznam materiály pro vývojáře, včetně referenční informace pro uděluje hello protokoly a OAuth2 toky podpory Azure AD, najdete v části toohello [Příručka vývojáře pro Azure AD][AAD-Developers-Guide]
* V tématu [jak toointegrate aplikaci s Azure AD] [ ACOM-How-To-Integrate] pro další hloubku na proces integrace aplikace hello.

<!--Image references-->

<!--Reference style links in use-->
[AAD-Developers-Guide]: active-directory-developers-guide.md
[ACOM-How-And-Why-Apps-Added-To-AAD]: active-directory-how-applications-are-added.md
[ACOM-How-To-Integrate]: active-directory-how-to-integrate.md
[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819
