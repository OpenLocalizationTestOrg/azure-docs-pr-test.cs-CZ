---
title: "aaaAzure AD Xamarin Začínáme | Microsoft Docs"
description: "Sestavení Xamarin aplikace, které integrují s Azure AD pro přihlášení a volání Azure AD chráněné rozhraní API pomocí metody OAuth."
services: active-directory
documentationcenter: xamarin
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 198cd2c3-f7c8-4ec2-b59d-dfdea9fe7d95
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 6a0d189648b7071558ac1cf2b908808668960a4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-with-xamarin-apps"></a>Integrace Azure AD s aplikacemi Xamarin
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

S Xamarinem může zapisovat mobilní aplikace v jazyce C#, můžete spustit na iOS, Android a Windows (mobilních zařízení a počítačů). Pokud vytváříte aplikace pomocí Xamarinu, Azure Active Directory (Azure AD) umožňuje jednoduché tooauthenticate uživatelů s účty služby Azure AD. aplikace Hello můžete také bezpečně využívat žádné webové rozhraní API, který je chráněný službou Azure AD, jako je například hello rozhraní API Office 365 nebo hello rozhraní API služby Azure.

Pro aplikace Xamarin, které vyžadují tooaccess chráněné zdroje Azure AD poskytuje hello Active Directory Authentication Library (ADAL). jediným účelem Hello adal je toomake snadno aplikace tooget přístupových tokenů. toodemonstrate jak snadné je, tento článek ukazuje, jak aplikace DirectorySearcher toobuild který:

* Spusťte na iOS, Android, Windows Desktop, Windows Phone a Windows Store.
* Použití jedné třídy přenosné knihovny (PCL) tooauthenticate uživatelů a získat tokeny pro hello Azure AD Graph API.
* Vyhledejte adresář pro uživatele s danou UPN.

## <a name="before-you-get-started"></a>Než začnete
* Stáhnout hello [kostru projektu](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip), nebo stáhnout hello [hotová ukázka](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Každého stažení je řešením sady Visual Studio 2013.
* Musíte taky klient služby Azure AD, ve které toocreate uživatelé a aplikace hello registrace. Pokud ještě nemáte klienta, [zjistěte, jak tooget jeden](active-directory-howto-tenant.md).

Až budete připravení, postupujte podle pokynů hello v hello další čtyři části.

## <a name="step-1-set-up-your-xamarin-development-environment"></a>Krok 1: Nastavení vývojového prostředí Xamarin
Tento kurz zahrnuje projekty pro iOS, Android a Windows, a proto musíte Visual Studio a Xamarin. toocreate hello nezbytné prostředí, proces dokončení hello v [nastavit až a nainstalujte Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) na webu MSDN. Hello pokyny zahrnují materiálu, kterou můžete zkontrolovat informace o Xamarin toolearn, zatímco čekáte toobe hello instalace byla dokončena.

Po dokončení instalace hello, otevřete v sadě Visual Studio hello řešení. Zde najdete šesti projekty: pět projekty specifické pro platformu a jeden PCL DirectorySearcher.cs, které budou sdílet všechny platformy.

## <a name="step-2-register-hello-directorysearcher-app"></a>Krok 2: Registrace hello DirectorySearcher aplikace
tooenable hello aplikace tooget tokeny, musíte nejprve tooregister ji ve službě Azure AD klienta a udělit mu oprávnění tooaccess hello Azure AD Graph API. Zde je uveden postup:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Na horním panelu hello klikněte na váš účet. Potom v části hello **Directory** seznamu, vyberte hello klienta služby Active Directory místo tooregister hello aplikace.
3. Klikněte na tlačítko **více služeb** v levém podokně text hello a potom vyberte **Azure Active Directory**.
4. Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.
5. toocreate nový **nativní klientská aplikace**, postupujte podle pokynů hello.
  * **Název** popisuje toousers aplikace hello.
  * **Identifikátor URI pro přesměrování** schématu a řetězec kombinací, Azure AD používá tooreturn odpovědi tokenu. Zadejte hodnotu (například http://DirectorySearcher).
6. Po dokončení registrace Azure AD přiřadí aplikace hello ID jedinečný aplikace. Zkopírujte hodnotu hello z hello **aplikace** kartě, protože ho budete potřebovat později.
7. Na hello **nastavení** vyberte **požadovaných oprávnění**a potom vyberte **přidat**.
8. Vyberte **Microsoft Graph** jako hello rozhraní API. V části **delegovaná oprávnění**, přidejte hello **čtení dat adresáře** oprávnění.  
Tato akce umožní hello aplikace tooquery hello rozhraní Graph API pro uživatele.

## <a name="step-3-install-and-configure-adal"></a>Krok 3: Instalace a konfigurace ADAL
Nyní když máte aplikaci ve službě Azure AD, můžete nainstalovat ADAL a zadejte kód, týkající se identity. tooenable ADAL toocommunicate s Azure AD, poskytněte některé informace o registraci aplikace hello.

1. Přidejte ADAL toohello DirectorySearcher projekt pomocí hello Konzola správce balíčků.

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
    `

    Všimněte si, že dva odkazy knihovny jsou přidány tooeach projektu: část PCL hello ADAL a část specifické pro platformu.
2. V projektu DirectorySearcherLib hello otevřete DirectorySearcher.cs.
3. Nahraďte hodnoty členů třídy hello hello hodnoty, které jste zadali v hello portálu Azure. Váš kód odkazoval toothese hodnoty vždy, když ho využívá ADAL.

  * Hello *klienta* je hello domény klienta služby Azure AD (například contoso.onmicrosoft.com).
  * Hello *clientId* je ID klienta hello hello aplikace, které jste zkopírovali z portálu hello.
  * Hello *returnUri* je identifikátor URI, který jste zadali v portálu hello (například http://DirectorySearcher) hello přesměrování.

## <a name="step-4-use-adal-tooget-tokens-from-azure-ad"></a>Krok 4: Použití ADAL tooget tokeny z Azure AD
Téměř všechny logiku ověřování aplikace hello spočívá v `DirectorySearcher.SearchByAlias(...)`. Všechny, které je nezbytné v projektech specifické pro platformu hello je toopass toohello kontextových parametrů `DirectorySearcher` PCL.

1. Otevřete DirectorySearcher.cs a pak přidejte nový parametr toohello `SearchByAlias(...)` metoda. `IPlatformParameters`je hello kontextové parametr, který zapouzdřuje hello specifické platformy objekty, ADAL potřebám tooperform hello ověření.

    ```C#
    public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
    {
    ```

2. Inicializace `AuthenticationContext`, což je primární třídou hello adal.  
Tato akce hello ADAL předává jej koordinuje potřebám toocommunicate s Azure AD.
3. Volání `AcquireTokenAsync(...)`, který přijímá hello `IPlatformParameters` objektu a vyvolá hello tok ověřování, který je nutné tooreturn tokenu toohello aplikace.

    ```C#
    ...
        AuthenticationResult authResult = null;
        try
        {
            AuthenticationContext authContext = new AuthenticationContext(authority);
            authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
        }
        catch (Exception ee)
        {
            results.Add(new User { error = ee.Message });
            return results;
        }
    ...
    ```

    `AcquireTokenAsync(...)`první tooreturn pokusy o token pro hello požadovaný prostředek (hello rozhraní Graph API v tomto případě) bez vyzvání uživatele tooenter přihlašovacích údajů (prostřednictvím ukládání do mezipaměti nebo obnovení původního tokeny). Podle potřeby zobrazuje hello Azure AD přihlašovací stránka uživatelé před získání tokenu požadovaný hello.
4. Připojte hello žádost o rozhraní Graph API tokenu toohello přístup v hello **autorizace** hlavičky:

    ```C#
    ...
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
    ...
    ```

To je všechno pro hello `DirectorySearcher` kódu spojené s PCL a hello aplikace. Zbývá toocall hello `SearchByAlias(...)` metoda v zobrazení pro každou platformu a v případě potřeby tooadd kód pro zpracování správně hello životní cyklus uživatelského rozhraní.

### <a name="android"></a>Android
1. V MainActivity.cs, přidejte volání příliš`SearchByAlias(...)` v hello tlačítko klikněte na obslužnou rutinu:

    ```C#
    List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
    ```
2. Přepsání hello `OnActivityResult` životního cyklu metoda tooforward přesměruje back toohello odpovídající metodu ověřování. ADAL poskytuje Pomocná metoda pro to v Android:

    ```C#
    ...
    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {
        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
    }
    ...
    ```

### <a name="windows-desktop"></a>Windows Desktop
V MainWindow.xaml.cs, zkontrolujte volání příliš`SearchByAlias(...)` předáním `WindowInteropHelper` v hello plochy `PlatformParameters` objektu:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

#### <a name="ios"></a>iOS
V DirSearchClient_iOSViewController.cs, hello iOS `PlatformParameters` objekt trvá odkaz toohello řadiče zobrazení:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

### <a name="windows-universal"></a>Univerzální pro Windows
Otevřete MainPage.xaml.cs v univerzální pro Windows a potom implementovat hello `Search` metoda. Tato metoda používá metodu helper v sdílený projekt tooupdate uživatelského rozhraní podle potřeby.

```C#
...
List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

## <a name="whats-next"></a>Kam dál
Teď máte funkční aplikaci Xamarin, můžete ověřovat uživatele a bezpečně volat webové rozhraní API pomocí standardu OAuth 2.0 napříč pět různých platformách.

Pokud již nejsou naplněny vašeho klienta s uživateli, teď proto je toodo čas hello.

1. Spuštění aplikace DirectorySearcher a pak se přihlaste pomocí jeden z uživatelů hello.
2. Hledání jiných uživatelů podle jejich UPN.

ADAL umožňuje snadno tooincorporate běžné funkce identity do aplikace hello. Se postará všechny pracovní dirty hello, jako je například Správa mezipaměti podpora protokolu OAuth, prezentací hello uživatele s přihlašovacími údaji uživatelského rozhraní, a aktualizovat platnost tokenů. Je třeba volání tooknow pouze jediného rozhraní API `authContext.AcquireToken*(…)`.

Odkaz, stáhněte si hello [hotová ukázka](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip) (bez vašich hodnot nastavení).

Nyní se můžete přesunout na tooadditional identity scénáře. Zkuste například [zabezpečení webového rozhraní API .NET s Azure AD](active-directory-devquickstarts-webapi-dotnet.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
