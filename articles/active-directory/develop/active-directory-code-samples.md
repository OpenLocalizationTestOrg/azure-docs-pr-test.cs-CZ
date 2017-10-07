---
title: "aaaAzure ukázky kódu služby Active Directory | Microsoft Docs"
description: "Index ukázky kódu Azure Active Directory, uspořádané podle scénáře."
services: active-directory
documentationcenter: dev-center-name
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: mbaldwin
ms.custom: aaddev
ms.openlocfilehash: 921ce08766cc6e29a6db2c4f38d216e6de11730f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-code-samples"></a>Ukázky kódu Azure Active Directory
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Můžete použít Microsoft Azure Active Directory (Azure AD) tooadd ověřování a autorizace tooyour webových aplikací a webových rozhraní API. Tato část odkazy můžete toosamples která ukazují, jak se provádí a fragmenty kódu, které můžete použít ve svých aplikacích. Na stránce ukázkový kód hello zjistíte podrobné čtení-mi témata, které pomáhají s požadavky, instalace a nastavení. A kód hello je komentáři toohelp pochopit hello kritické oddíly.

toounderstand hello základní scénáře pro každý typ ukázka zjistit scénáře ověřování pro Azure AD.

Přispívat tooour ukázky z webu GitHub: [Microsoft Azure Active Directory ukázky a dokumentace](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="web-browser-tooweb-application"></a>Webové prohlížeče tooWeb aplikace
Tyto ukázky ukazují, jak toowrite webové aplikace, která přesměruje hello toosign prohlížeče uživatele je v tooAzure AD.

| Jazyk nebo platformu | Ukázka | Popis |
| --- | --- | --- |
| C# NEBO ROZHRANÍ .NET |[WebApp-OpenIDConnect-DotNet.](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) |Použijte OpenID Connect (ASP.Net OpenID Connect OWIN middleware) tooauthenticate uživatelé z tenanta služby Azure AD. |
| C# NEBO ROZHRANÍ .NET |[WebApp víceklientské OpenIdConnect DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) |Víceklientské .NET MVC webovou aplikaci, která používá OpenID Connect (ASP.Net OpenID Connect OWIN middleware) tooauthenticate uživatele z více klientů Azure AD. |
| C# NEBO ROZHRANÍ .NET |[WebApp-WSFederation-DotNet.](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) |Pomocí protokolu WS-Federation (WS-Federation OWIN ASP.Net middleware) tooauthenticate uživatelé z tenanta služby Azure AD. |

## <a name="single-page-application-spa"></a>Jednostránkové aplikace (SPA)
Tento příklad ukazuje, jak toowrite jednostránkové aplikace zabezpečené s Azure AD.  

| Jazyk nebo platformu | Ukázka | Popis |
| --- | --- | --- |
| JavaScript, C# nebo rozhraní .NET |[SinglePageApp DotNet.](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) |Pomocí ADAL pro JavaScript a Azure AD toosecure AngularJS na základě jednostránkové aplikace implementuje pomocí technologie ASP.NET web API back-end. |

## <a name="native-application-tooweb-api"></a>Nativní aplikace tooWeb rozhraní API
Tyto ukázky kódu ukazují, jak toobuild nativní klientských aplikací, které volání webových rozhraní API, které jsou zabezpečené službou Azure AD. Používají [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) a [OAuth 2.0 ve službě Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Jazyk nebo platformu | Ukázka | Popis |
| --- | --- | --- |
| JavaScript |[NativeClient. MultiTarget Cordova](https://github.com/Azure-Samples/active-directory-cordova-multitarget) |Použijte hello ADAL modulu plug-in pro Apache Cordova toobuild aplikaci Apache Cordova, která volá webové rozhraní API a používá Azure AD pro ověřování. |
| C# NEBO ROZHRANÍ .NET |[NativeClient DotNet.](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) |Aplikace WPF v rozhraní .NET, která volá webové rozhraní API, která je zabezpečena pomocí Azure AD. |
| C# NEBO ROZHRANÍ .NET |[NativeClient WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) |Aplikace Windows Store, která volá webové rozhraní API, která je zabezpečená službou Azure AD. |
| C# NEBO ROZHRANÍ .NET |[NativeClient WebAPI víceklientské WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) |Aplikaci pro Windows Store volání víceklientské webového rozhraní API, která je zabezpečená službou Azure AD. |
| C# NEBO ROZHRANÍ .NET |[WebAPI-OnBehalfOf-DotNet.](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof) |Nativní klientská aplikace, která volá webové rozhraní API, který získá token tooact jménem hello původního uživatele a pak používá hello tokenu toocall jiné webové rozhraní API. |
| C# NEBO ROZHRANÍ .NET |[NativeClient WindowsPhone8.1](https://github.com/Azure-Samples/active-directory-dotnet-windowsphone-8.1) |Aplikace Windows Store pro Windows Phone 8.1, která volá webové rozhraní API, která je zabezpečená službou Azure AD. |
| ObjC |[NativeClient iOS](https://github.com/Azure-Samples/active-directory-ios) |Aplikace pro iOS, která volá webové rozhraní API, která vyžaduje Azure AD pro ověřování. |
| C# NEBO ROZHRANÍ .NET |[WebAPI-ManuallyValidateJwt-DotNet.](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation) |Nativní klientská aplikace, která obsahuje logiku tooprocess JWT token webového rozhraní API, místo použití OWIN middleware. |
| C# nebo Xamarin |[NativeClient Xamarin.Android](https://github.com/Azure-Samples/active-directory-xamarin-android) |Xamarin, vazba toohello nativní Azure AD Authentication Library (ADAL) pro hello knihovna pro Android. |
| C# nebo Xamarin |[NativeClient. Xamarin iOS](https://github.com/Azure-Samples/active-directory-xamarin-ios) |Vazba toohello Xamarin nativní Azure AD Authentication Library (ADAL) pro iOS. |
| C# nebo Xamarin |[NativeClient-MultiTarget-DotNet.](https://github.com/Azure-Samples/active-directory-dotnet-native-multitarget) |Projekt Xamarin, který cílí pět platformy a volá webové rozhraní API, která je zabezpečená službou Azure AD. |
| C# NEBO ROZHRANÍ .NET |[NativeClient bezobslužných DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-headless) |Nativní aplikace, který provede neinteraktivního ověřování a volá webové rozhraní API, která je zabezpečená službou Azure AD. |

## <a name="web-application-tooweb-api"></a>Webové aplikace tooWeb rozhraní API
Tyto ukázky kódu, jak používat zobrazit [OAuth 2.0 ve službě Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) toobuild webových aplikací, které volají webové rozhraní API, které jsou zabezpečené službou Azure AD.

| Jazyk nebo platformu | Ukázka | Popis |
| --- | --- | --- |
| C# NEBO ROZHRANÍ .NET |[WebApp WebAPI OpenIDConnect DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect) |Volání webového rozhraní API s oprávněními hello přihlášeného uživatele. |
| C# NEBO ROZHRANÍ .NET |[WebApp-WebAPI-OAuth2-AppIdentity-DotNet.](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity) |Volání webového rozhraní API se oprávnění aplikace hello. |
| C# NEBO ROZHRANÍ .NET |[WebApp-WebAPI-OAuth2-UserIdentity-Dotnet.](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) |Přidat autorizační s [OAuth 2.0 ve službě Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) tooan stávající webovou aplikaci, můžete ho volání webového rozhraní API. |
| JavaScript |[WebAPI Nodejs](https://github.com/Azure-Samples/active-directory-node-webapi) |Nastavte službu REST API, která je integrovaná s Azure AD pro rozhraní API ochrany. Obsahuje server Node.js pomocí webového rozhraní API. |
| C# NEBO ROZHRANÍ .NET |[WebApp-WebAPI víceklientské OpenIdConnect-DotNet.](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-multitenant-openidconnect) |Víceklientské MVC webové aplikace, která používá OpenID Connect (ASP.Net OpenID Connect OWIN middleware) tooauthenticate uživatelé z tenanta služby Azure AD. Používá tooinvoke hello kódu autorizace rozhraní Graph API. |

## <a name="server-or-daemon-application-tooweb-api"></a>Server nebo aplikaci démon tooWeb rozhraní API
Tyto ukázky kódu ukazují, jak toobuild démon nebo serverové aplikace, který získá prostředky z webového rozhraní API pomocí [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) a [OAuth 2.0 ve službě Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Jazyk nebo platformu | Ukázka | Popis |
| --- | --- | --- |
| C# NEBO ROZHRANÍ .NET |[Démon DotNet.](https://github.com/Azure-Samples/active-directory-dotnet-daemon) |Konzolové aplikace volá webové rozhraní API. pověření klienta Hello je heslo. |
| C# NEBO ROZHRANÍ .NET |[Démon CertificateCredential-DotNet.](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential) |Konzolovou aplikaci, která volá webové rozhraní API. pověření klienta Hello je certifikát. |

## <a name="calling-azure-ad-graph-api"></a>Volání rozhraní API Azure AD Graph
Tato ukázka kódu ukazují, jak toobuild aplikace, které volají hello Azure AD Graph API tooread a zápis dat adresáře.

| Jazyk nebo platformu | Ukázka | Popis |
| --- | --- | --- |
| Java |[WebApp. GraphAPI Java](https://github.com/Azure-Samples/active-directory-java-graphapi-web) |Webové aplikace, která používá rozhraní Graph API tooaccess hello dat adresáře Azure AD. |
| PHP |[WebApp. GraphAPI PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-web) |Webové aplikace, která používá rozhraní Graph API tooaccess hello dat adresáře Azure AD. |
| C# NEBO ROZHRANÍ .NET |[WebApp-GraphAPI-DotNet.](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) |Webové aplikace, která používá rozhraní Graph API tooaccess hello dat adresáře Azure AD. |
| C# NEBO ROZHRANÍ .NET |[ConsoleApp-GraphAPI-DotNet.](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-console) |Tato Konzolová aplikace předvádí běžné ke čtení a zápisu volání toohello rozhraní Graph API a popisuje, jak tooexecute uživatele přiřazení licence a aktualizovat miniaturu fotografie a odkazy uživatele. |
| C# NEBO ROZHRANÍ .NET |[ConsoleApp GraphAPI DiffQuery DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-diffquery) |Konzolovou aplikaci, která používá hello rozdílové dotaz v tooget rozhraní Graph API hello pravidelné změny toouser objekty v klient služby Azure AD. |
| C# NEBO ROZHRANÍ .NET |[WebApp GraphAPI DirectoryExtensions DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-directoryextensions-web) |Aplikace MVC používá rozhraní Graph API dotazy toogenerate organizační diagram jednoduché společnosti. |
| PHP |[WebApp GraphAPI DirectoryExtensions PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-directoryextensions-web) |Aplikace PHP, která volá hello rozhraní Graph API tooregister rozšíření a přečtěte si, aktualizovat a odstranit hodnoty v atributu rozšíření hello. |

## <a name="authorization"></a>Autorizace
Tyto ukázky zobrazit code jak toouse Azure AD pro ověřování.

| Jazyk nebo platformu | Ukázka | Popis |
| --- | --- | --- |
| C# NEBO ROZHRANÍ .NET |[WebApp-GroupClaims-DotNet.](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) |Proveďte řízení přístupu na základě role (RBAC) pomocí deklarace skupiny Azure Active Directory v aplikaci, která je integrovaná se službou Azure AD. |
| C# NEBO ROZHRANÍ .NET |[WebApp-RoleClaims-DotNet.](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) |Proveďte řízení přístupu na základě role (RBAC) pomocí služby Azure Active Directory aplikační role v aplikaci, která je integrovaná se službou Azure AD. |

## <a name="legacy-walkthroughs"></a>Návody pro starší verze
Tyto postupy používají mírně starší technologii, ale stále může být důležité.

| Jazyk nebo platformu | Ukázka | Popis |
| --- | --- | --- |
| C# NEBO ROZHRANÍ .NET |[Ověřování na základě rolí a na základě seznamu ACL v aplikaci Microsoft Azure AD](http://go.microsoft.com/fwlink/?LinkId=331694) |Proveďte ověření na základě role (RBAC) a autorizace na základě seznamu ACL v aplikaci, která je integrovaná se službou Azure AD. |
| C# NEBO ROZHRANÍ .NET |[AAL - service tooREST aplikace Windows Store - ověřování](http://go.microsoft.com/fwlink/?LinkId=330605) |Použití [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) (dříve vrstev AAL) pro Windows Store Beta tooadd uživatele ověřování možnosti tooa aplikace pro Windows Store. |
| C# NEBO ROZHRANÍ .NET |[Ověřování ADAL - service tooREST nativní App - v AAD přes dialogové okno prohlížeče](http://go.microsoft.com/fwlink/?LinkId=259814) |Použití [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) tooadd uživatele ověřování možnosti tooa WPF klienta. |
| C# NEBO ROZHRANÍ .NET |[Ověřování ADAL - service tooREST nativní aplikaci - službou ACS přes dialogové okno prohlížeče](http://code.msdn.microsoft.com/AAL-Native-App-to-REST-de57f2cc) |Použití [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) a [2.0 služby Řízení přístupu (ACS)](http://msdn.microsoft.com/library/azure/hh147631.aspx) tooadd uživatele ověřování možnosti tooa WPF klienta. |
| C# NEBO ROZHRANÍ .NET |[ADAL - tooServer serveru ověřování](http://go.microsoft.com/fwlink/?LinkId=259816) |Použití [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) toosecure služby volání ze serveru straně proces tooan MVC4 webové rozhraní API REST služby. |
| C# NEBO ROZHRANÍ .NET |[Přidání přihlášení tooYour webové aplikace pomocí Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) |Nakonfigurujte .NET aplikace tooperform webové jednotné přihlašování s enterprise adresáře Azure AD. |
| C# NEBO ROZHRANÍ .NET |[Vývoj víceklientské webových aplikací s Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) |Používání Azure AD tooadd toohello jednotné přihlašování a možnosti přístupu directory jeden toowork aplikace .NET v několika organizacích. |
| JAVA |[Ukázková aplikace Java pro Azure AD Graph API](http://go.microsoft.com/fwlink/?LinkId=263969) |Použijte hello rozhraní Graph API tooaccess dat adresáře ze služby Azure AD. |
| PHP |[Ukázková aplikace PHP pro Azure AD Graph API](http://code.msdn.microsoft.com/PHP-Sample-App-For-Windows-228c6ddb) |Použijte hello rozhraní Graph API tooaccess dat adresáře ze služby Azure AD. |
| C# NEBO ROZHRANÍ .NET |[Ukázková aplikace pro Azure AD Graph API](http://go.microsoft.com/fwlink/?LinkID=262648) |Použijte hello rozhraní Graph API tooaccess dat adresáře ze služby Azure AD. |
| C# NEBO ROZHRANÍ .NET |[Ukázková aplikace pro Azure AD Graph rozdílové dotazu](http://go.microsoft.com/fwlink/?LinkId=275398) |Použijte hello rozdílové dotazu v hello rozhraní Graph API tooget pravidelné změny toouser objekty v klient služby Azure AD. |
| C# NEBO ROZHRANÍ .NET |[Ukázková aplikace pro integraci víceklientské cloudových aplikací pro Azure AD](http://go.microsoft.com/fwlink/?LinkId=275397) |Integrované víceklientské aplikace do Azure AD. |
| C# NEBO ROZHRANÍ .NET |[Zabezpečení aplikaci pro Windows Store a webové služby REST pomocí služby Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) |Vytvořte prostředek jednoduché webové rozhraní API a klientské aplikace Windows Store pomocí služby Azure AD a hello [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md). |
| C# NEBO ROZHRANÍ .NET |[Pomocí rozhraní Graph API tooQuery hello Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) |Nakonfigurujte rozhraní Microsoft .NET aplikace toouse hello Azure AD Graph API tooaccess data z klienta adresář služby Azure AD. |

## <a name="see-also"></a>Viz také
##### <a name="other-resources"></a>Další prostředky
[Příručka pro vývojáře Azure Active Directory](active-directory-developers-guide.md)

[Azure AD Graph API koncepční a referenční informace](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure AD Graph API pomocné knihovny](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
