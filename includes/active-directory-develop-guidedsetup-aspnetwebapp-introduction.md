# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Přidání přihlášení se společností Microsoft do webové aplikace ASP.NET

Tato příručka ukazuje, jak implementovat přihlásit se pomocí rozhraní ASP.NET MVC řešení s tradiční webové aplikace založené na prohlížeči pomocí OpenID Connect společností Microsoft. 

Na konci tohoto průvodce vaše aplikace bude moci přijmout sign in osobní účty (včetně live.com, outlook.com a dalších) a také pracovní a školní účty z jakékoli společnosti nebo organizace, která má integrované s Azure Active Directory. 

> Tato příručka vyžaduje Visual Studio 2015 Update 3 nebo Visual Studio 2017.  Nemáte ho?  [Stáhněte si Visual Studio 2017 zdarma](https://www.visualstudio.com/downloads/)

## <a name="how-this-guide-works"></a>Jak funguje tato příručka

![Jak funguje tato příručka](media/active-directory-develop-guidedsetup-aspnetwebapp-intro/aspnetbrowsergeneral.png)

Tato příručka je založena na scénář, kde prohlížeče přistupuje ke webovou stránku ASP.NET požaduje uživatele k ověřování prostřednictvím tlačítko přihlášení. V tomto scénáři proběhne většinu práce k vykreslení webové stránky na straně serveru.

## <a name="libraries"></a>Knihovny

Tato příručka používá následující knihovny:

|Knihovna|Popis|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Middleware, který umožňuje aplikaci používat OpenIdConnect pro ověřování|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Middleware, který umožňuje aplikaci spravovat pomocí souborů cookie relace uživatele|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|Umožňuje aplikacím na základě OWIN běží ve službě IIS pomocí kanál požadavku|

