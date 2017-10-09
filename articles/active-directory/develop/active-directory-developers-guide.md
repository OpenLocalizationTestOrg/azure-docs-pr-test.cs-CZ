---
title: "aaaAzure služby Active Directory pro vývojáře | Microsoft Docs"
description: "Tento článek obsahuje přehled přihlašování pracovních a školních účtů Microsoft pomocí Azure Active Directory."
services: active-directory
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 4dbbea6c1e0b8a70c0c36ddd1caec5658130a003
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-for-developers"></a>Azure Active Directory pro vývojáře
Azure Active Directory je Cloudová služba identity, která umožňuje vývojářům toosecurely přihlásit založenou na každý uživatel s pracovní nebo školní účet Microsoft.  Hello dokumentace zde dozvíte, jak tooadd Azure AD podporuje tooyour aplikací pomocí OAuth a OpenID Connect standardní ověřovací protokoly odvětví.

| | |
| --- | --- |
|[Základy ověřování](active-directory-authentication-scenarios.md) | Úvod tooauthentication s Azure AD |
|[Typy aplikací](active-directory-authentication-scenarios.md#application-types-and-scenarios) | Přehled scénářů ověřování hello podporovaný službou Azure AD |                                
                                                                              
## <a name="get-started"></a>Začínáme
Tato nastavení s průvodcem vás provede procesem pomocí našich toosign knihovny ověřování v Azure Active Directory uživatelé.

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Mobilní aplikace a aplikace počítače](./media/active-directory-developers-guide/NativeApp_Icon.png)<br />Mobilní aplikace a aplikace počítače</center> | [Přehled](active-directory-authentication-scenarios.md#native-application-to-web-api)<br /><br />[iOS](active-directory-devquickstarts-ios.md)<br /><br />[Android](active-directory-devquickstarts-android.md) | [.NET](active-directory-devquickstarts-dotnet.md)<br /><br />[Windows](active-directory-devquickstarts-windowsstore.md)<br /><br />[Xamarin](active-directory-devquickstarts-xamarin.md) | [Cordova](active-directory-devquickstarts-cordova.md)<br /><br />[OAuth 2.0](active-directory-protocols-oauth-code.md) |
| <center>![Web Apps](./media/active-directory-developers-guide/Web_app.png)<br />Web Apps</center> | [Přehled](active-directory-authentication-scenarios.md#web-browser-to-web-application)<br /><br />[ASP.NET](active-directory-devquickstarts-webapp-dotnet.md)<br /><br />[Java](active-directory-devquickstarts-webapp-java.md) | [NodeJS](active-directory-devquickstarts-openidconnect-nodejs.md)<br /><br />[OpenID Connect 1.0](active-directory-protocols-openid-connect-code.md) |  |
| <center>![Jednostránkové aplikace](./media/active-directory-developers-guide/SPA.png)<br />Jednostránkové aplikace</center> | [Přehled](active-directory-authentication-scenarios.md#single-page-application-spa)<br /><br />[AngularJS](active-directory-devquickstarts-angular.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |  |  |
| <center>![Webová rozhraní API](./media/active-directory-developers-guide/Web_API.png)<br />Webová rozhraní API</center> | [Přehled](active-directory-authentication-scenarios.md#web-application-to-web-api)<br /><br />[ASP.NET](active-directory-devquickstarts-webapi-dotnet.md)<br /><br />[NodeJS](active-directory-devquickstarts-webapi-nodejs.md) | &nbsp; |
| <center>![Služba-služba](./media/active-directory-developers-guide/Service_App.png)<br />Služba-služba</center> | [Přehled](active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api)<br /><br />[.NET](active-directory-code-samples.md#server-or-daemon-application-to-web-api)<br /><br />[Přihlašovací údaje pro klienta OAuth 2.0](active-directory-protocols-oauth-service-to-service.md) |  |

## <a name="guides"></a>Průvodci
Tyto články informujte, jak tooperform běžné úlohy s Azure Active Directory.

|                                                                           |  |
|---------------------------------------------------------------------------| --- |
|[Registrace aplikace](active-directory-integrating-applications.md)           | Jak tooregister aplikace ve službě Azure AD |
|[Aplikace s více tenanty](active-directory-devhowto-multi-tenant-overview.md)    | Jak fungují toosign v veškeré účtu |
|[OAuth a OpenID Connect](active-directory-protocols-openid-connect-code.md)| Jak toosign-in Uživatelé a volání webových rozhraní API pomocí našich protokolů moderní ověřování |
|[Další průvodci...](active-directory-developers-guide-index.md#guides)        |     |

## <a name="reference"></a>Referenční informace
Tyto články poskytují podrobné informace o rozhraních API, zprávách protokolů a termínech používaných v Azure Active Directory.

|                                                                                   | |
| ----------------------------------------------------------------------------------| --- |
| [Knihovny ověřování (ADAL)](active-directory-authentication-libraries.md)   | Přehled knihovny hello & SDK poskytuje Azure AD |
| [Ukázky kódu](active-directory-code-samples.md)                                  | Seznam všech ukázek kódu Azure AD |
| [Glosář](active-directory-dev-glossary.md)                                      | Terminologie a definice slov, která se používají v této dokumentaci |
| [Další referenční materiály...](active-directory-developers-guide-index.md#reference)|     |

## <a name="help--support"></a>Nápověda a podpora
Jedná se o hello nejlepší místech tooget pomoc s vývojem v Azure Active Directory.

|  |  
|---|
|[Značky `azure-active-directory` a `adal` na Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory+or+adal)      |
|[Zpětná vazba k Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences)|
| [Vyzkoušejte Microsoft Dev Chat (zdarma po omezenou dobu)](http://aka.ms/devchat) |

<br />

> [!NOTE]
> Pokud potřebujete toosign-in Microsoft osobní účty, můžete pomocí hello tooconsider [koncového bodu Azure AD v2.0](active-directory-appmodel-v2-overview.md).  koncový bod v2.0 Hello Azure AD je hello sjednocení osobní účty Microsoft & pracovních účtů Microsoft (z Azure AD) do jedné ověřování systému.
