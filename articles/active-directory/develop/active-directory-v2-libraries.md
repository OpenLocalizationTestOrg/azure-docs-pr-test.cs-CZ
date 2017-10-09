---
title: "knihovny ověřování v2.0 aaaAzure služby Active Directory | Microsoft Docs"
description: "Kompatibilní klientské knihovny a knihovny middlewaru serveru a související knihovny, zdroje a odkazy ukázky pro koncový bod v2.0 hello Azure Active Directory."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 19cec615-e51f-4141-9f8c-aaf38ff9f746
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: d7affdaac3a087b951d54d96fa68edde2a065172
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-v20-authentication-libraries"></a>Knihovny ověřování Azure Active Directory v2.0
Hello koncového bodu v2.0 Azure Active Directory (Azure AD) podporuje protokoly OAuth 2.0 a OpenID Connect 1.0 standardní hello. Můžete použít různé knihovny od společnosti Microsoft a jiných organizací s koncovým bodem v2.0 hello.

Když vytvoříte aplikaci, která používá koncového bodu v2.0 hello, doporučujeme používat knihovny, které jsou zapsány odborníky protokolu domény, kteří podle metodika životního cyklu SDL (Security Development), jako je třeba [jeden následuje Microsofthello] [Microsoft-SDL]. Pokud se rozhodnete toohand kódu podpoře protokolů hello, doporučujeme, postupujte podle SDL metodika a zaměřit se aspekty zabezpečení toohello do specifikací hello standardy pro každý protokol.

## <a name="types-of-libraries"></a>Knihovny typů
Azure AD v2.0 funguje s dva typy knihovny:

* **Knihovny klienta**. Nativní klienty a servery používat pro volání prostředku, jako je například Microsoft Graph tokeny přístupu tooget knihovny klienta.
* **Server knihovny middleware**. Webové aplikace používat server knihovny middleware pro přihlášení uživatele. Webová rozhraní API pomocí serveru middleware knihovny toovalidate tokeny, které jsou odesílány nativní klienty nebo jiné servery.

## <a name="library-support"></a>Podpora knihovny
Vzhledem k tomu, že při použití koncového bodu v2.0 hello můžete žádné kompatibilní se standardy knihovny, je důležité tooknow kde toogo pro podporu. Pro problémy a žádosti o funkce v kódu knihovny obraťte se na vlastníka knihovny hello. Problémy a žádosti o funkce v implementaci protokolu na straně služby hello od společnosti Microsoft.

Knihovny se ve dvou kategoriích podpory:

* **Podporované Microsoft**. Společnost Microsoft poskytuje opravy pro tyto knihovny a provedla SDL kvůli opatrností na tyto knihovny.
* **Kompatibilní**. Společnost Microsoft tyto knihovny otestována základní scénáře a potvrdit, že fungují s koncovým bodem v2.0 hello. Společnost Microsoft neposkytuje opravy pro tyto knihovny a nebyl provádí kontrolu tyto knihovny. Problémy a žádosti o funkce by měla být směrovanou toohello knihovny open-source projekt.

Seznam knihoven, které pracují s koncovým bodem v2.0 hello najdete v tématu hello další části v tomto článku.


## <a name="microsoft-supported-client-libraries"></a>Knihovny Microsoft podporované klienta

> [!IMPORTANT]
> Hello MSAL preview knihovny jsou vhodné pro použití v provozním prostředí. Poskytujeme hello stejné produkční úrovně podpory pro tyto knihovny jako naše aktuální produkční knihovny (ADAL). Během hello preview můžeme provést změny toohello MSAL API formátu vnitřní mezipaměti, a další mechanismy tyto knihovny bez upozornění, která bude potřeba tootake společně s opravy chyb nebo vylepšení funkcí. To může mít vliv na vaše aplikace. Pro instanci formátu změnu toohello mezipaměti může mít vliv na uživatele, jako je vyžadování je znovu toosign v. O rozhraní API změnu může vyžadovat tooupdate vám kód. Když jsme poskytují hello obecné dostupnosti verze jsme bude vyžadovat tooupdate toohello obecné dostupnosti verze šest měsíců, jako aplikace napsané v náhledu verzi knihovny může přestane fungovat.

| Platforma | Knihovna | Ke stažení | Zdrojový kód | Ukázka | Referenční informace
| --- | --- | --- | --- | --- | --- |
| Klient .NET, Windows Store, UWP Xamarin iOS a Android | MSAL .NET (Preview) |[NuGet](https://www.nuget.org/packages/Microsoft.Identity.Client) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | [Aplikace na ploše](guidedsetups/active-directory-mobileanddesktopapp-windowsdesktop-intro.md) |  |
| JavaScript | MSAL.js (Preview) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) | [Jedna stránka aplikace](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2) |  |
| iOS, systému macOS | MSAL (Preview) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) | [Aplikace pro iOS](https://github.com/Azure-Samples/active-directory-msal-ios-swift) |  |
| Android | MSAL (Preview) | [Hello centrální úložiště](https://repo1.maven.org/maven2/com/microsoft/identity/client/msal/) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) | [Aplikace pro Android](guidedsetups/active-directory-mobileanddesktopapp-android-intro.md) | [JavaDocs](http://javadoc.io/doc/com.microsoft.identity.client/msal) |

## <a name="microsoft-supported-server-middleware-libraries"></a>Knihovny Microsoft podporované server middleware

| Platforma | Knihovna | Ke stažení | Zdrojový kód | Ukázka | Referenční informace
| --- | --- | --- | --- | --- | --- |
| Rozhraní .NET 4.x | Middleware OWIN OpenID Connect |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[Webu CodePlex](http://katanaproject.codeplex.com) |[Aplikace MVC](guidedsetups/active-directory-serversidewebapp-aspnetwebappowin-intro.md) | |
| Rozhraní .NET 4.x | Middleware OWIN nosiče OAuth pro AzureAD |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[Webu CodePlex](http://katanaproject.codeplex.com) |  | |
| Rozhraní .NET 4.x | Obslužná rutina JWT pro rozhraní .NET 4.5 | [NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt/4.0.4.403061554) | [GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET Core | Middleware OpenID Connect ASP.NET |[Microsoft.AspNetCore.Authentication.OpenIdConnect (NuGet)][ServerLib-NetCore-Owin-Oidc-Lib] |[Zabezpečení technologie ASP.NET (Githubu)][ServerLib-NetCore-Owin-Oidc-Repo] |[Aplikace MVC](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2) |
| .NET Core | Middleware nosiče OAuth ASP.NET |[Microsoft.AspNetCore.Authentication.OAuth (NuGet)][ServerLib-NetCore-Owin-Oauth-Lib] |[Zabezpečení technologie ASP.NET (Githubu)][ServerLib-NetCore-Owin-Oauth-Repo] |  |
| .NET Core | Obslužná rutina JWT pro .NET Core  |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| Node.js |Azure AD Passport |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Webové aplikace](active-directory-v2-devquickstarts-node-web.md)| |

## <a name="compatible-client-libraries"></a>Kompatibilní klient knihovny
| Platforma | Název knihovny | Otestované verze | Zdrojový kód | Ukázka |
|:---:|:---:|:---:|:---:|:---:|
| Android |[OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib/wiki) |0.2.1 |[OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib) |[Ukázka nativní aplikace](active-directory-v2-devquickstarts-android.md) |
| iOS |[NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) |1.2.8 |[NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) |[Ukázka nativní aplikace](active-directory-v2-devquickstarts-ios.md) |
| JavaScript |[Hello.js](https://adodson.com/hello.js/) |1.13.5 |[Hello.js](https://github.com/MrSwitch/hello.js) |[SPA](https://github.com/Azure-Samples/active-directory-javascript-graphapi-web-v2) |

## <a name="compatible-server-middleware-libraries"></a>Server kompatibilní middleware knihovny
| Platforma | Název knihovny | Otestované verze | Zdrojový kód | Ukázka |
|:---:|:---:|:---:|:---:|:---:|
| Java | [Scribejava Scribe Java](https://github.com/scribejava/scribejava) | [Verze 3.2.0](https://github.com/scribejava/scribejava/releases/tag/scribejava-3.2.0) | [ScribeJava](https://github.com/scribejava/scribejava/archive/scribejava-3.2.0.zip) | |
| PHP | [PHP ligy oauth2-client Hello](https://github.com/thephpleague/oauth2-client) | [Verze 1.4.2](https://github.com/thephpleague/oauth2-client/releases/tag/1.4.2) | [oauth2 klienta](https://github.com/thephpleague/oauth2-client/archive/1.4.2.zip) | |
| Python Flask |[Flask OAuthlib](https://github.com/lepture/flask-oauthlib) |0.9.3 |[Flask OAuthlib](https://github.com/lepture/flask-oauthlib) |[Webová aplikace](https://github.com/Azure-Samples/active-directory-python-flask-graphapi-web-v2) |
| Ruby |[OmniAuth](https://github.com/omniauth/omniauth/wiki) |omniauth:1.3.1</br>omniauth-oauth2:1.4.0 |[OmniAuth](https://github.com/omniauth/omniauth)</br>[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) |  |

## <a name="related-content"></a>Související obsah
Další informace o koncového bodu v2.0 hello Azure AD, najdete v části hello [přehled v2.0 modelu aplikace Azure AD][AAD-App-Model-V2-Overview].

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: ../active-directory-appmodel-v2-overview.md
[ClientLib-NET-Lib]: http://www.nuget.org/packages/Microsoft.Identity.Client
[ClientLib-NET-Repo]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[ClientLib-NET-Sample]: active-directory-v2-devquickstarts-wpf.md
[ClientLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ClientLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad
[ClientLib-Node-Sample]:/
[ClientLib-Iosmac-Lib]:/
[ClientLib-Iosmac-Repo]:/
[ClientLib-Iosmac-Sample]:/
[ClientLib-Android-Lib]:/
[ClientLib-Android-Repo]:/
[ClientLib-Android-Sample]:/
[ClientLib-Js-Lib]:/
[ClientLib-Js-Repo]:/
[ClientLib-Js-Sample]:/

[Microsoft-SDL]: http://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oauth-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-dotnet-api/
[ServerLib-Net-Jwt-Lib]: https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt
[ServerLib-Net-Jwt-Repo]: https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
[ServerLib-Net-Jwt-Sample]:/
[ServerLib-NetCore-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/
[ServerLib-NetCore-Owin-Oidc-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oidc-Sample]: https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2
[ServerLib-NetCore-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OAuth/
[ServerLib-NetCore-Owin-Oauth-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oauth-Sample]:/
[ServerLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ServerLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad/
[ServerLib-Node-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-node-web/
