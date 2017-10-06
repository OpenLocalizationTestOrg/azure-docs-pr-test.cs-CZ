---
title: "aaaAzure AD Windows Store Začínáme | Microsoft Docs"
description: "Aplikace pro Windows Store sestavení, které integraci se službou Azure AD pro přihlášení a volání služby Azure AD chráněné rozhraní API pomocí metody OAuth."
services: active-directory
documentationcenter: windows
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 3b96a6d1-270b-4ac1-b9b5-58070c896a68
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 09/16/2016
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 1d12c7b928bc0e94fb823f8db4a09ff416205e2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-with-windows-store-apps"></a>Integrace Azure AD s aplikací pro Windows Store
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> Windows Store 8.1 a předchozí verze projekty nejsou podporované v Visual Studio 2017.  Další informace najdete v tématu [Cílení na platformy a kompatibilita v sadě Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

Pokud vyvíjíte aplikace pro Windows Store hello, Azure Active Directory (Azure AD) umožňuje jednoduchá a přímočará tooauthenticate vaši uživatelé s účty služby Active Directory. Díky integraci s Azure AD, můžete aplikaci bezpečně využívat žádné webové rozhraní API, který je chráněný službou Azure AD, jako je například hello rozhraní API Office 365 nebo hello rozhraní API služby Azure.

Pro desktopové aplikace Windows Store, které vyžadují tooaccess chráněné zdroje Azure AD poskytuje hello Active Directory Authentication Library (ADAL). jediným účelem Hello adal je toomake snadno hello aplikace tooget přístupových tokenů. jak snadné je, tento článek ukazuje, jak pro toobuild DirectorySearcher Windows Store toodemonstrate aplikace který:

* Získá přístup k tokeny pro volání rozhraní API Azure AD Graph hello pomocí hello [protokol ověřování OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Vyhledá adresář pro uživatele s hlavní název daného uživatele (UPN).
* Uživatelé přihlásí se.

## <a name="before-you-get-started"></a>Než začnete
* Stáhnout hello [kostru projektu](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip), nebo stáhnout hello [hotová ukázka](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip). Každého stažení je řešením sady Visual Studio 2015.
* Musíte taky klient služby Azure AD, ve které toocreate uživatelé a aplikace hello registrace. Pokud ještě nemáte klienta, [zjistěte, jak tooget jeden](active-directory-howto-tenant.md).

Až budete připravení, hello postupy hello postupujte podle kroků v následujících třech částech.

## <a name="step-1-register-hello-directorysearcher-app"></a>Krok 1: Zaregistrujte hello DirectorySearcher aplikace
tooenable hello aplikace tooget tokeny, musíte nejprve tooregister ji ve službě Azure AD klienta a udělit mu oprávnění tooaccess hello Azure AD Graph API. Zde je uveden postup:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Na horním panelu hello klikněte na váš účet. Potom v části hello **Directory** seznamu, vyberte hello klienta služby Active Directory místo tooregister hello aplikace.
3. Klikněte na tlačítko **více služeb** v levém podokně text hello a potom vyberte **Azure Active Directory**.
4. Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.
5. Postupujte podle pokynů toocreate hello **nativní klientská aplikace**.
  * **Název** popisuje toousers aplikace hello.
  * **Identifikátor URI pro přesměrování** schématu a řetězec kombinací, Azure AD používá tooreturn odpovědi tokenu. Zadejte hodnotu zástupného symbolu pro nyní (například **http://DirectorySearcher**). Nahradíte hodnotu hello později.
6. Po dokončení registrace hello, Azure AD přiřadí aplikace hello ID jedinečný aplikace. Zkopírujte hodnotu hello na hello **aplikace** kartě, protože ho budete potřebovat později.
7. Na hello **nastavení** vyberte **požadovaných oprávnění**a potom vyberte **přidat**.
8. Pro hello **Azure Active Directory** aplikace, vyberte **Microsoft Graph** jako hello rozhraní API.
9. V části **delegovaná oprávnění**, přidejte hello **přístup k adresáři hello jako hello přihlášeného uživatele** oprávnění. Díky tomu hello aplikace tooquery hello rozhraní Graph API pro uživatele.

## <a name="step-2-install-and-configure-adal"></a>Krok 2: Instalace a konfigurace ADAL
Nyní když máte aplikaci ve službě Azure AD, můžete nainstalovat ADAL a zadejte kód, týkající se identity. tooenable ADAL toocommunicate s Azure AD, poskytněte některé informace o registraci aplikace hello.

1. Přidejte ADAL toohello DirectorySearcher projekt pomocí hello Konzola správce balíčků.

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

2. V projektu DirectorySearcher hello otevřete MainPage.xaml.cs.
3. Nahraďte hodnoty hello v hello **konfiguračních hodnot** oblasti s hello hodnotami, které jste zadali v hello portálu Azure. Váš kód odkazoval toothese hodnoty vždy, když ho využívá ADAL.
  * Hello *klienta* je hello domény klienta služby Azure AD (například contoso.onmicrosoft.com).
  * Hello *clientId* je ID klienta hello hello aplikace, které jste zkopírovali z portálu hello.
4. Teď musíte zpětného volání hello toodiscover identifikátor URI pro aplikace pro Windows Store. Na tomto řádku v hello zarážku `MainPage` metoda:
    ```
    redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
    ```
5. Vytvoření hello řešení, a ujistěte se, že se obnoví všechny odkazy na balíček. Pokud chybí všechny balíčky, otevřete hello Správce balíčků NuGet a jejich obnovení.
6. Spuštění aplikace hello a zkopírujte hodnotu hello `redirectUri` při průchodu zarážek hello. Hello hodnota by měla vypadat podobně jako následující hello:

    ```
    ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
    ```

7. Zpět na hello **nastavení** přidat na kartě aplikace hello v hello portál Azure, **RedirectUri** s hello předcházející hodnotu.  

## <a name="step-3-use-adal-tooget-tokens-from-azure-ad"></a>Krok 3: Použití ADAL tooget tokeny z Azure AD
Hello základní princip za ADAL je, že vždy, když aplikace hello je přístupový token, jednoduše volá `authContext.AcquireToken(…)`, a ADAL hello rest.  

1. Inicializace aplikace hello `AuthenticationContext`, což je primární třídou hello adal. Tato akce předá ADAL hello souřadnice ho potřebuje toocommunicate s Azure AD a určit, jak toocache tokeny.

    ```C#
    public MainPage()
    {
        ...

        authContext = new AuthenticationContext(authority);
    }
    ```

2. Vyhledejte hello `Search(...)` metodu, která je volána, když uživatelé kliknou na hello **vyhledávání** tlačítko na hello uživatelském rozhraní aplikace. Tato metoda vytváří tooquery toohello Azure AD Graph API požadavek get pro uživatele, jehož UPN začíná hello zadaný hledaný termín. tooquery hello rozhraní Graph API zahrnout přístupový token požadavku hello **autorizace** záhlaví. Toto je, kde odeslán ADAL.

    ```C#
    private async void Search(object sender, RoutedEventArgs e)
    {
        ...
        AuthenticationResult result = null;
        try
        {
            result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectURI, new PlatformParameters(PromptBehavior.Auto, false));
        }
        catch (AdalException ex)
        {
            if (ex.ErrorCode != "authentication_canceled")
            {
                ShowAuthError(string.Format("If hello error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
            }
            return;
        }
        ...
    }
    ```
    Když aplikace hello vyžaduje token voláním `AcquireTokenAsync(...)`, ADAL pokusí tooreturn token bez nutnosti hello uživatelské přihlašovací údaje. Pokud ADAL zjistí, že tento uživatel hello je toosign v tooget token, zobrazí přihlašovací dialogové okno, shromažďuje přihlašovací údaje uživatele hello a vrátí token po úspěšném provedení ověřování. Pokud ADAL nelze tooreturn token z jakéhokoli důvodu, hello *AuthenticationResult* stav je k chybě.
3. Nyní je čas toouse hello přístupový token, který jste získali. Také v hello `Search(...)` metoda, připojte hello tokenu toohello rozhraní Graph API získat žádost o v hello **autorizace** hlavičky:

    ```C#
    // Add hello access token toohello Authorization header of hello call toohello Graph API, and call hello Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

    ```
4. Můžete použít hello `AuthenticationResult` objektu toodisplay informace o uživateli hello hello aplikace, jako je například ID uživatele hello:

    ```C#
    // Update hello page UI toorepresent hello signed-in user
    ActiveUser.Text = result.UserInfo.DisplayableId;
    ```
5. Můžete také použít ADAL toosign uživatelé mimo aplikaci hello. Když uživatel hello klikne hello **Odhlásit** tlačítko, ujistěte se, že další volání hello příliš`AcquireTokenAsync(...)` zobrazení přihlášení. Pomocí knihovny ADAL tato akce je stejně snadná jako vymazání mezipamětí tokenů hello:

    ```C#
    private void SignOut()
    {
        // Clear session state from hello token cache.
        authContext.TokenCache.Clear();

        ...
    }
    ```

## <a name="whats-next"></a>Kam dál
Teď máte funkční aplikace pro Windows Store, můžete ověřovat uživatele, bezpečně volání webových rozhraní API pomocí OAuth 2.0 a získat základní informace o uživateli hello.

Pokud již nejsou naplněny vašeho klienta s uživateli, teď proto je toodo čas hello.
1. Spuštění aplikace DirectorySearcher a pak se přihlaste pomocí jeden z uživatelů hello.
2. Hledání jiných uživatelů podle jejich UPN.
3. Zavření aplikace hello a znovu jej spusťte. Všimněte si, jak hello uživatelské relace zůstává beze změn.
4. Odhlásit se kliknutím pravým tlačítkem na toodisplay hello dolním panelu a pak se přihlaste zpět v jako jiný uživatel.

ADAL umožňuje snadno tooincorporate všechny tyto běžné funkce identity do aplikace hello. Se postará všechny pracovní dirty hello, jako je například Správa mezipaměti podpora protokolu OAuth, prezentací hello uživatele s přihlašovacími údaji uživatelského rozhraní, a aktualizovat platnost tokenů. Je třeba volání tooknow pouze jediného rozhraní API `authContext.AcquireToken*(…)`.

Odkaz, stáhněte si hello [hotová ukázka](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip) (bez vašich hodnot nastavení).

Nyní se můžete přesunout na tooadditional identity scénáře. Zkuste například [zabezpečení webového rozhraní API .NET s Azure AD](active-directory-devquickstarts-webapi-dotnet.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
