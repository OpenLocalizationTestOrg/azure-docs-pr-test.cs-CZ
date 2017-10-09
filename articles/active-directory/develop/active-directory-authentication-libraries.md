---
title: "aaaAzure knihovny ověřování služby Active Directory | Microsoft Docs"
description: "Hello Azure AD Authentication Library (ADAL), umožňuje klientským vývojáři aplikace tooeasily ověření uživatelé toocloud nebo místní služby Active Directory (AD) a získat přístupové tokeny zabezpečení volání rozhraní API."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: mbaldwin
ms.assetid: 2e4fc79a-0285-40be-8c77-65edee408a22
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/02/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: 20fae18807ef03463ab1bc218e5f3548b5bd5717
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-authentication-libraries"></a>Knihovny ověřování služby Azure Active Directory
Hello Azure Active Directory Authentication Library (ADAL) umožňuje klienta aplikace vývojáři tooeasily ověření uživatelé toocloud nebo místní služby Active Directory (AD) a získat přístupové tokeny zabezpečení volání rozhraní API. ADAL usnadňuje ověřování pro vývojáře prostřednictvím funkcí, jako například:
 - Podpora pro volání asynchronních metod
 - Konfigurovat tokenu mezipaměti, aby úložiště přístup tokeny a obnovovacích tokenů
 - Automatická aktualizace tokenu, když vyprší platnost přístupového tokenu a obnovovací token je k dispozici
 - a další
 
ADAL zpracování většinu hello složitost, pomáhá vývojářům zaměřit se na obchodní logiky a snadno zabezpečení prostředků, aniž by musel být odborník v zabezpečení.

ADAL je k dispozici v mnoha různých platforem.

### <a name="client-libraries"></a>Klientské knihovny

| Platforma | Knihovna | Ke stažení | Zdrojový kód | Ukázka | Referenční informace
| --- | --- | --- | --- | --- | --- |
| Klient .NET, Windows Store, UWP Xamarin iOS a Android |MSAL pro platformu .NET (preview) |[NuGet](https://www.nuget.org/packages/Microsoft.Identity.Client/1.1.0-preview) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | [Aplikace na ploše](~/articles/active-directory/develop/guidedsetups/active-directory-windesktop.md) |[Referenční informace](https://docs.microsoft.com/dotnet/api/?view=identityclient-1.1.0-preview) | 
| JavaScript |MSAL pro jazyk JavaScript (preview) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) | [Jedna stránka aplikace](~/articles/active-directory/develop/GuidedSetups/active-directory-javascriptspa.md) | [Referenční informace](https://htmlpreview.github.io/?https://raw.githubusercontent.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/docs/classes/_useragentapplication_.msal.useragentapplication.html) | 
| iOS |MSAL pro iOS (preview) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) | [aplikace pro iOS](~/articles/active-directory/develop/GuidedSetups/active-directory-ios.md) | [Referenční informace](https://azuread.github.io/docs/objc/) |
| Android |MSAL pro Android (preview) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) | [Aplikace pro Android](~/articles/active-directory/develop/GuidedSetups/active-directory-android.md) | [Referenční informace](http://javadoc.io/doc/com.microsoft.identity.client/msal/0.1.1) |
| Klient .NET, Windows Store, UWP Xamarin iOS a Android |ADAL .NET v3 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) | [Aplikace na ploše](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-dotnet) |[Referenční informace](https://docs.microsoft.com/dotnet/api/?view=identitymodelclientsad-3.13.9) | 
| Rozhraní .NET klienta Windows Store, Windows Phone 8.1 |ADAL .NET v2 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.4) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.4) | [Aplikace na ploše](https://github.com/AzureADQuickStarts/NativeClient-DotNet/releases/tag/v2.X) | | 
| JavaScript |ADAL.js |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[Jedna stránka aplikace](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) | |
| iOS, systému macOS |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc/releases) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc) |[aplikace pro iOS](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-ios) | [Referenční informace](https://cocoapods.org/pods/ADAL)|
| Android |ADAL |[Hello centrální úložiště](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android) |[Aplikace pro Android](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-android) | [JavaDocs](http://javadoc.io/doc/com.microsoft.aad/adal/)|
| Node.js |ADAL |[npm](https://www.npmjs.com/package/adal-node) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs) | | |
| Java |ADAL4J |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[Webová aplikace Java](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapp-java) | |
| Python |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) | | |
### <a name="server-libraries"></a>Server knihovny 

| Platforma | Knihovna | Ke stažení | Zdrojový kód | Ukázka | Referenční informace
| --- | --- | --- | --- | --- | --- |
| .NET |OWIN pro AzureAD|[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[Webu CodePlex](http://katanaproject.codeplex.com) |[Aplikace MVC](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapp-dotnet) | |
| .NET |OWIN pro OpenIDConnect |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[Webu CodePlex](http://katanaproject.codeplex.com) |[Webová aplikace](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet) | |
| Node.js |Azure AD Passport |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Webové rozhraní API](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapi-nodejs)| |
| .NET |OWIN pro WS-Federation |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) |[Webu CodePlex](http://katanaproject.codeplex.com) |[Webové aplikace MVC](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) | |
| .NET |Rozšíření protokolu identity pro .NET 4.5 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET |Obslužná rutina JWT pro rozhraní .NET 4.5 |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |



## <a name="scenarios"></a>Scénáře

Tady jsou tři běžné scénáře, ve kterých lze použít ADAL pro ověřování klienta, který používá vzdálený prostředek:  

### <a name="authenticating-users-of-a-native-client-application-running-on-a-device"></a>Ověřování uživatelů nativní klientskou aplikaci spouštění v zařízení 

V tomto scénáři má vývojář klientskou aplikaci WPF, který potřebuje tooaccess vzdálený prostředek zabezpečené službou Azure AD, jako jsou webové rozhraní API. Mu má předplatné Azure, vědět, jak tooinvoke hello podřízené webové rozhraní API a zná hello Azure AD klienta, který hello používá webové rozhraní API. V důsledku toho může použít ověřování ADAL toofacilitate s Azure AD, plně delegování tooADAL zkušeností ověřování hello nebo explicitně zpracovávat přihlašovací údaje uživatele. ADAL umožňuje snadno tooauthenticate hello uživatele, získat přístupový a obnovovací token ze služby Azure AD a potom pomocí žádosti o token toomake hello přístup toohello webového rozhraní API.

Ukázka kódu, který ukazuje, tento scénář pomocí ověřování tooAzure AD, najdete v části [nativní klientská aplikace WPF tooWeb rozhraní API](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-confidential-client-application-running-on-a-web-server"></a>Ověřování důvěrné klientské aplikace běžící na webovém serveru

V tomto scénáři má vývojář aplikace běžící na serveru, který potřebuje tooaccess vzdálený prostředek zabezpečené službou Azure AD, jako jsou webové rozhraní API. Mu má předplatné Azure, vědět, jak tooinvoke hello podřízené služby a ví, že používá hello Azure AD klienta hello webového rozhraní API. V důsledku toho může použít ověřování ADAL toofacilitate s Azure AD pomocí explicitně zpracování přihlašovací údaje hello aplikací. ADAL umožňuje snadno tooretrieve tokenu z Azure AD pomocí pověření klienta hello aplikace a pak pomocí tohoto tokenu toomake požadavky toohello webové rozhraní API. ADAL také popisovače Správa hello životnost hello přístupový token mezipaměti a obnovení podle potřeby. Ukázka kódu, který ukazuje, tento scénář, najdete v části [démon konzole aplikace tooWeb rozhraní API](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-confidential-client-application-running-on-a-server-on-behalf-of-a-user"></a>Ověřování důvěrné klientské aplikace běží na serveru, jménem uživatele 

V tomto scénáři má vývojář aplikace běžící na serveru, který potřebuje tooaccess vzdálený prostředek zabezpečené službou Azure AD, jako jsou webové rozhraní API. požadavek Hello musí také toobe provedené jménem uživatele Azure AD. Mu má předplatné Azure, vědět, jak tooinvoke hello podřízené webové rozhraní API a ví, že používá hello služba hello klienta Azure AD. Jakmile je uživatel hello ověřené toohello webové aplikace, aplikace hello může získat autorizační kód pro hello uživatele z Azure AD. Webová aplikace Hello potom můžete pomocí ADAL tooobtain přístupový token a obnovovací token jménem uživatele s využitím hello kód a klient přihlašovací údaje pro autorizaci přidruženou k aplikaci hello z Azure AD. Jakmile hello webové aplikace je k dispozici hello přístupový token, může zavolat hello webového rozhraní API do vypršení platnosti tokenu hello. Když vyprší platnost tokenu hello, hello webové aplikace můžete použít ADAL tooget nový přístupový token pomocí hello obnovovací token, který jste dříve získali. Ukázka kódu, který ukazuje, tento scénář, najdete v části [nativního klienta tooWeb rozhraní API tooWeb rozhraní API](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof).

## <a name="see-also"></a>Viz také

- [Hello Příručka pro vývojáře Azure Active Directory](active-directory-developers-guide.md)
- [Scénáře ověřování pro Azure Active directory](active-directory-authentication-scenarios.md)
- [Ukázky kódu Azure Active Directory](active-directory-code-samples.md)
