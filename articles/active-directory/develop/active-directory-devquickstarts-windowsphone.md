---
title: "aaaAzure AD Windows Phone Začínáme | Microsoft Docs"
description: "Tom, jak toobuild aplikace Windows Phone, která se integruje se službou Azure AD pro přihlašování a volá Azure AD chráněný rozhraní API pomocí metody OAuth."
services: active-directory
documentationcenter: windows
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 66f5ac20-5e1f-4b9d-bb99-9b3305e26416
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: e766bfcdfae10483772154f4b5facdec05fc846f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-with-a-windows-phone-app"></a>Integrace Azure AD a aplikaci pro Windows Phone
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> Projekty pro Windows Phone 8.1 a starší verze nejsou podporovány v sadě Visual Studio 2017.  Další informace najdete v tématu [Cílení na platformy a kompatibilita v sadě Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

Pokud vyvíjíte aplikace Windows Phone 8.1, Azure AD je jednoduchá a přímočará pro tooauthenticate můžete uživatelům pomocí jejich účtů služby Active Directory.  Umožňuje také toosecurely vaše aplikace využívat žádné webové rozhraní API, které jsou chráněné službou Azure AD, jako například hello rozhraní API Office 365 nebo hello rozhraní API služby Azure.

> [!NOTE]
> Tento příklad používá ADAL v2.0.  Pro hello nejnovější technologie, může být vhodné tooinstead zkuste naše [kurz pro univerzální aplikace Windows pomocí ADAL v3.0](active-directory-devquickstarts-windowsstore.md).  Pokud vytváříte skutečně aplikace pro Windows Phone 8.1, je to hello správném místě.  ADAL v2.0 je stále plně podporovaná a je hello doporučeným způsobem agianst vývoj aplikace Windows Phone 8.1 pomocí služby Azure AD.
> 
> 

Pro nativní klienty .NET, které vyžadují tooaccess chráněné zdroje Azure AD poskytuje hello knihovna ověřování Active Directory nebo ADAL.  Jediným účelem je ADAL v životnosti je toomake ho snadno pro vaše aplikace tooget přístupových tokenů.  jak lze snadno, je, toodemonstrate Zde jsme budete sestavení "Directory osobou hledající" aplikace Windows Phone 8.1:

* Získá přístup k tokeny pro volání rozhraní API Azure AD Graph hello pomocí hello [protokol ověřování OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Vyhledá adresář pro uživatele s danou UPN.
* Uživatelé přihlásí se.

toobuild hello dokončení pracovní aplikace, budete muset:

1. Registrace vaší aplikace s Azure AD.
2. Instalace a konfigurace ADAL.
3. Pomocí ADAL tooget tokeny z Azure AD.

spuštění, tooget [stáhnout kostru projektu](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) nebo [stažení ukázky hello Dokončit](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  Každá je řešením sady Visual Studio 2013.  Budete také potřebovat klient služby Azure AD, ve kterém můžete vytvořit uživatele a zaregistrovat aplikaci.  Pokud ještě nemáte klienta, [zjistěte, jak tooget jeden](active-directory-howto-tenant.md).

## <a name="1-register-hello-directory-searcher-application"></a>1. Zaregistrovat hello aplikace Directory osobou hledající
tooenable tokeny tooget vaší aplikace, je nutné nejdříve tooregister ji ve službě Azure AD klienta a udělit mu oprávnění tooaccess hello Azure AD Graph API:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Na horním panelu hello, klikněte na tlačítko na vašem účtu a v části hello **Directory** vyberte klienta hello služby Active Directory, kde chcete tooregister vaší aplikace.
3. Klikněte na **více služeb** v hello navigaci vlevo a zvolte **Azure Active Directory**.
4. Klikněte na **registrace aplikace** a zvolte **přidat**.
5. Postupujte podle pokynů hello a vytvořte novou **nativní klientská aplikace**.
  * Hello **název** z hello aplikace popíše aplikaci tooend uživatelé
  * Hello **identifikátor Uri pro přesměrování** schématu a řetězec kombinací, Azure AD použijete tooreturn odpovědi tokenu.  Zadejte hodnotu zástupného symbolu pro nyní, například `http://DirectorySearcher`.  Tato hodnota není později budete nahradit.
6. Po dokončení registrace AAD přiřadí vaší aplikace, jedinečné ID aplikace  Tuto hodnotu budete potřebovat v dalších částech hello, takže zkopírujte jej z karty aplikace hello.
7. Z hello **nastavení** vyberte **požadovaných oprávnění** a zvolte **přidat**. Vyberte hello **Microsoft Graph** jako hello rozhraní API a přidejte hello **čtení dat adresáře** oprávnění v rámci **delegovaná oprávnění**.  Tato akce povolí vaší aplikace tooquery hello rozhraní Graph API pro uživatele.

## <a name="2-install--configure-adal"></a>2. Instalace a konfigurace ADAL
Teď, když máte aplikaci ve službě Azure AD, můžete nainstalovat ADAL a zadejte kód, týkající se identity.  Aby mohl mít toocommunicate ADAL toobe s Azure AD, je nutné tooprovide její některé informace o registraci vaší aplikace.

* Začněte tím, že přidání ADAL toohello DirectorySearcher projekt pomocí hello Konzola správce balíčků.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* V projektu DirectorySearcher hello otevřete `MainPage.xaml.cs`.  Nahraďte hodnoty hello v hello `Config Values` oblast tooreflect hello hodnoty, vstup do hello portálu Azure.  Váš kód bude odkazovat tyto hodnoty vždy, když ho využívá ADAL.
  * Hello `tenant` je hello domény klienta služby Azure AD, např. contoso.onmicrosoft.com
  * Hello `clientId` je hello clientId vaší aplikace, které jste zkopírovali z portálu hello.
* Teď musíte identifikátor uri zpětného volání hello toodiscover pro aplikace pro Windows Phone.  Na tomto řádku v hello zarážku `MainPage` metoda:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
* Spuštění aplikace hello a zkopírujte z produkce hello hodnotu `redirectUri` při průchodu zarážek hello.  Měl by vypadat nějak podobně jako

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

* Zpět na hello **konfigurace** kartě vaší aplikace v hello portálu pro správu Azure, nahraďte hodnotu hello hello **RedirectUri** s touto hodnotou.  

## <a name="3-use-adal-tooget-tokens-from-aad"></a>3. Použití ADAL tooGet tokeny od AAD
Hello základní princip za ADAL je, že vždy, když aplikace potřebuje přístupový token, jednoduše volá `authContext.AcquireToken(…)`, a ADAL hello rest.  

* prvním krokem Hello je tooinitialize vaší aplikace `AuthenticationContext` -ADAL je primární třídou.  Toto je, kde je předat ADAL hello souřadnice potřebuje toocommunicate s Azure AD a určit, jak toocache tokeny.

```C#
public MainPage()
{
    ...

    // ADAL for Windows Phone 8.1 builds AuthenticationContext instances through a factory
    authContext = AuthenticationContext.CreateAsync(authority).GetResults();
}
```

* Nyní najít hello `Search(...)` metodu, která bude vyvolán při hello uživatele cliks hello tlačítko "Search" v uživatelském rozhraní aplikace hello.  Tato metoda vytváří tooquery toohello Azure AD Graph API požadavek GET pro uživatele, jehož UPN začíná hello zadaný hledaný termín.  Ale v pořadí tooquery hello rozhraní Graph API, musíte tooinclude access_token v hello `Authorization` záhlaví hello – to přichází ADAL požadavku.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...

    // Try tooget a token without triggering any user prompt.
    // ADAL will check whether hello requested token is in ADAL's token cache or can otherwise be obtained without user interaction.
    AuthenticationResult result = await authContext.AcquireTokenSilentAsync(graphResourceId, clientId);
    if (result != null && result.Status == AuthenticationStatus.Success)
    {
        // A token was successfully retrieved.
        QueryGraph(result);
    }
    else
    {
        // Acquiring a token without user interaction was not possible.
        // Trigger an authentication experience and specify that once a token has been obtained hello QueryGraph method should be called
        authContext.AcquireTokenAndContinue(graphResourceId, clientId, redirectURI, QueryGraph);
    }
}
```
* Pokud je nutné interaktivního ověřování, ADAL použije webové ověřování zprostředkovatele (soubor WAB) Windows Phone a [pokračování modelu](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) toodisplay hello Azure AD přihlašovací stránky.  Když hello uživatel přihlásí, vaše aplikace musí toopass ADAL hello výsledky hello WAB interakce.  To je jednoduché, implementace hello `ContinueWebAuthentication` rozhraní:

```C#
// This method is automatically invoked when hello application
// is reactivated after an authentication interaction through WebAuthenticationBroker.
public async void ContinueWebAuthentication(WebAuthenticationBrokerContinuationEventArgs args)
{
    // pass hello authentication interaction results tooADAL, which will
    // conclude hello token acquisition operation and invoke hello callback specified in AcquireTokenAndContinue.
    await authContext.ContinueAcquireTokenAsync(args);
}
```

* Nyní je čas toouse hello `AuthenticationResult` této ADAL vrátil tooyour aplikace.  V hello `QueryGraph(...)` zpětné volání, připojte hello access_token jste získali požadavek GET toohello v hlavičce autorizace hello:

```C#
private async void QueryGraph(AuthenticationResult result)
{
    if (result.Status != AuthenticationStatus.Success)
    {
        MessageDialog dialog = new MessageDialog(string.Format("If hello error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", result.Error, result.ErrorDescription), "Sorry, an error occurred while signing you in.");
        await dialog.ShowAsync();
    }

    // Add hello access token toohello Authorization Header of hello call toohello Graph API, and call hello Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

    ...
}
```
* Můžete taky hello `AuthenticationResult` objektu toodisplay informace o hello uživateli ve vaší aplikaci. V hello `QueryGraph(...)` metoda, použijte hello výsledek tooshow hello id uživatele na stránku hello:

```C#
// Update hello Page UI toorepresent hello signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
* Nakonec můžete ADAL toosign hello uživatele mimo i aplikace.  Když hello uživatel klikne na tlačítko "Odhlásit" hello, chceme tooensure, který hello další volání příliš`AcquireTokenSilentAsync(...)` se nezdaří.  Pomocí knihovny ADAL to je stejně snadná jako vymazání mezipamětí tokenů hello:

```C#
private void SignOut()
{
    // Clear session state from hello token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Blahopřejeme! Teď máte funkční aplikaci pro Windows Phone, která má hello možnost tooauthenticate uživatelé, bezpečně volání webového rozhraní API pomocí OAuth 2.0 a získat základní informace o uživateli hello.  Pokud jste to ještě neudělali, nyní je čas toopopulate hello vašeho klienta s některými uživateli.  Spuštění aplikace DirectorySearcher a přihlaste se pomocí jeden z těchto uživatelů.  Hledání jiných uživatelů podle jejich UPN.  Zavření aplikace hello a znovu jej spusťte.  Všimněte si, jak hello uživatelské relace zůstává beze změn.  Odhlásit se a přihlaste se zpět jako jiný uživatel.

ADAL umožňuje snadno tooincorporate všechny tyto běžné funkce identity do aplikace.  Postará všechny pracovní dirty hello vám - Správa mezipaměti, podpora protokolu OAuth, prezentací hello uživatele s přihlašovacími údaji uživatelského rozhraní, aktualizace vypršení platnosti tokenů a další.  Všechny skutečně potřebujete tooknow je jednoho volání rozhraní API `authContext.AcquireToken*(…)`.

Pro odkaz, hello dokončit ukázka (bez vašich hodnot nastavení) zajišťuje [zde](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  Nyní se můžete přesunout na tooadditional identity scénáře.  Může být vhodné tootry:

[Zabezpečení rozhraní .NET webového rozhraní API pomocí Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

