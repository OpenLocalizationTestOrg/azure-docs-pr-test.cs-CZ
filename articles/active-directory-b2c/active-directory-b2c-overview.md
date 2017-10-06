---
title: "Přehled – Azure AD B2C | Dokumentace Microsoftu"
description: "Vývoj aplikací určených uživatelům pomocí Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: c465dbde-f800-4f2e-8814-0ff5f5dae610
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/06/2016
ms.author: saeedakhter-msft
ms.openlocfilehash: abfd742e710458de3193dc5051de7818a112376c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-focus-on-your-app-let-us-worry-about-sign-up-and-sign-in"></a>Azure AD B2C: Soustřeďte se na svoji aplikaci, o registraci a přihlašování se postaráme my

Azure AD B2C je cloudové řešení správy identit pro webové a mobilní aplikace. Je vysoce dostupnou globální službu, která je škálovatelná toohundreds milionů identit. Řešení Azure AD B2C je postavené na platformě zabezpečené na podnikové úrovni a chrání tak vaše aplikace, firmu i zákazníky.

S minimální konfigurací Azure AD B2C umožňuje tooauthenticate vaší aplikace:

* **Sociální účty** (jako je Facebook, Google, LinkedIn a další)
* **Účty organizací** (využívající protokoly s otevřenými standardy, OpenID Connect nebo SAML)
* **Místní účty** (e-mailová adresa a heslo nebo uživatelské jméno a heslo)

## <a name="get-started"></a>Začínáme

Nejprve vlastního klienta získáte pomocí hello kroků uvedených v [vytvoření klienta Azure AD B2C](active-directory-b2c-get-started.md).

Zvolte svůj scénář vývoje aplikací:

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Mobilní aplikace a aplikace počítače](../active-directory/develop/media/active-directory-developers-guide/NativeApp_Icon.png)<br />Mobilní aplikace a aplikace počítače</center> | [Přehled](active-directory-b2c-reference-oauth-code.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br /><br />[iOS](https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal)<br /><br />[Android](https://github.com/Azure-Samples/active-directory-b2c-android-native-msal) | [.NET](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop)<br /><br />[Xamarin](https://github.com/Azure-Samples/active-directory-b2c-xamarin-native) |  |
| <center>![Web Apps](../active-directory/develop/media/active-directory-developers-guide/Web_app.png)<br />Web Apps</center> | [Přehled](active-directory-b2c-reference-oidc.md)<br /><br />[ASP.NET](active-directory-b2c-devquickstarts-web-dotnet-susi.md)<br /><br />[Jádro ASP.NET](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapp) | [Node.js](active-directory-b2c-devquickstarts-web-node.md) |  |
| <center>![Jednostránkové aplikace](../active-directory/develop/media/active-directory-developers-guide/SPA.png)<br />Jednostránkové aplikace</center> | [Přehled](active-directory-b2c-reference-spa.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)<br /><br /> |  |  |
| <center>![Webová rozhraní API](../active-directory/develop/media/active-directory-developers-guide/Web_API.png)<br />Webová rozhraní API</center> | [ASP.NET](active-directory-b2c-devquickstarts-api-dotnet.md)<br /><br /> [Jádro ASP.NET](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi)<br /><br /> [Node.js](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi) | [Volání webového rozhraní API .NET](active-directory-b2c-devquickstarts-web-api-dotnet.md) |

## <a name="whats-new"></a>Co je nového

Pravidelně kontrolujte často toolearn o budoucí změny toohello Azure Active Directory B2C. O všech aktualizacích také tweetujeme na @AzureAD.

* Kromě toho, příliš hello "Předdefinované zásady" (Obecné dostupnosti), ["Vlastní zásady"](active-directory-b2c-overview-custom.md) funkce je teď dostupná ve verzi public preview.  Vlastní zásady jsou pro profesionály identity, které je třeba kontrolu nad hello složení jejich prostředí identit.
* Hello [tokenu přístupu](https://azure.microsoft.com/en-us/blog/azure-ad-b2c-access-tokens-now-in-public-preview) funkce je teď dostupná ve verzi public preview.
* Proběhlo oznámení [všeobecné dostupnosti služby Azure AD B2C pro Evropu](https://azure.microsoft.com/en-us/blog/azuread-b2c-ga-eu/).
* Projděte si naši rostoucí knihovnu [ukázek kódů na GitHubu](https://github.com/Azure-Samples?q=b2c)!

## <a name="how-tooarticles"></a>Jak tooarticles

Zjistěte, jak konkrétních funkcí Azure Active Directory B2C toouse:

* Nastavení [Facebooku](active-directory-b2c-setup-fb-app.md), [Google+](active-directory-b2c-setup-goog-app.md), [účtu Microsoft](active-directory-b2c-setup-msa-app.md), účtů [Amazon](active-directory-b2c-setup-amzn-app.md), a [inkedIn](active-directory-b2c-setup-li-app.md) pro použití v aplikacích určených uživatelům.
* [Použít vlastní atributy toocollect informací o uživatelích](active-directory-b2c-reference-custom-attr.md).
* [Povolení Azure Multi-Factor Authentication v aplikacích určených uživatelům](active-directory-b2c-reference-mfa.md).
* [Nastavení samoobslužného resetování hesla pro uživatele](active-directory-b2c-reference-sspr.md).
* [Přizpůsobit hello vzhledu a chování registrace, přihlášení v a dalších stránek určených](active-directory-b2c-reference-ui-customization.md) které obsluhuje Azure Active Directory B2C.
* [Použití hello Azure Active Directory Graph API tooprogrammatically vytvářet, číst, aktualizovat a odstraňovat příjemci](active-directory-b2c-devquickstarts-graph-dotnet.md) v klientovi služby Azure Active Directory B2C.

## <a name="next-steps"></a>Další kroky

Tyto odkazy jsou užitečné při prozkoumávání služby hello:

* V tématu hello [informace o cenách Azure Active Directory B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/).
* Projděte si naše [ukázky kódu](https://azure.microsoft.com/en-us/resources/samples/?service=active-directory&term=b2c) pro Azure Active Directory B2C. 
* Získejte pomoc na Stack Overflow pomocí hello [azure-ad b2c](http://stackoverflow.com/questions/tagged/azure-ad-b2c) značky.
* Sdělte nám své myšlenky pomocí [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c), chceme toohear je!
* Zkontrolujte hello [referenční informace o Azure AD B2C protokolu](active-directory-b2c-reference-protocols.md).
* Zkontrolujte hello [Azure odkaz tokenu B2C AD](active-directory-b2c-reference-tokens.md).
* Čtení hello [Azure Active Directory B2C nejčastější dotazy k](active-directory-b2c-faqs.md).
* [Podávejte požadavky na podporu pro Azure Active Directory B2C](active-directory-b2c-support.md).

## <a name="get-security-updates-for-our-products"></a>Získejte bezpečnostní aktualizace našich produktů

Doporučujeme vám tooget oznámení o bezpečnostních incidentech navštivte stránky [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru tooSecurity Advisory Alerts.

