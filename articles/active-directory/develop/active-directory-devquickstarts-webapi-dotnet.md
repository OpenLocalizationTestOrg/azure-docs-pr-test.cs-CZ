---
title: "aaaAzure AD .NET webové rozhraní API Začínáme | Microsoft Docs"
description: "Jak: toobuild webového rozhraní API .NET MVC, který umožňuje integraci s Azure AD pro ověřování a autorizaci."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 67e74774-1748-43ea-8130-55275a18320f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 91c93e1fe18855f5648076e59e2ccf081eec34bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="help-protect-a-web-api-by-using-bearer-tokens-from-azure-ad"></a><span data-ttu-id="56eb5-103">Pomoc při ochraně webového rozhraní API pomocí nosných tokenů z Azure AD</span><span class="sxs-lookup"><span data-stu-id="56eb5-103">Help protect a web API by using bearer tokens from Azure AD</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="56eb5-104">Pokud vytváříte aplikace, která poskytuje přístup tooprotected prostředky, je třeba tooknow jak tooprevent bude vyplacena neoprávněně přístup k prostředkům toothose.</span><span class="sxs-lookup"><span data-stu-id="56eb5-104">If you’re building an application that provides access tooprotected resources, you need tooknow how tooprevent unwarranted access toothose resources.</span></span>
<span data-ttu-id="56eb5-105">Azure Active Directory (Azure AD) umožňuje jednoduché a přehledné toohelp chránit webového rozhraní API pomocí přístupových tokenů nosiče OAuth 2.0 jenom pár řádků kódu.</span><span class="sxs-lookup"><span data-stu-id="56eb5-105">Azure Active Directory (Azure AD) makes it simple and straightforward toohelp protect a web API by using OAuth 2.0 bearer access tokens with only a few lines of code.</span></span>

<span data-ttu-id="56eb5-106">V aplikacích ASP.NET web můžete provést tuto ochranu pomocí implementace Microsoft hello hello komunitou vytvářený middleware OWIN zahrnuté v rozhraní .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="56eb5-106">In ASP.NET web apps, you can accomplish this protection by using hello Microsoft implementation of hello community-driven OWIN middleware included in .NET Framework 4.5.</span></span> <span data-ttu-id="56eb5-107">Zde použijeme toobuild OWIN web API "tooDo seznamu" který:</span><span class="sxs-lookup"><span data-stu-id="56eb5-107">Here we’ll use OWIN toobuild a "tooDo List" web API that:</span></span>

* <span data-ttu-id="56eb5-108">Označí, které rozhraní API jsou chráněny.</span><span class="sxs-lookup"><span data-stu-id="56eb5-108">Designates which APIs are protected.</span></span>
* <span data-ttu-id="56eb5-109">Ověří, že volání webového rozhraní API hello obsahovat platné přístupový token.</span><span class="sxs-lookup"><span data-stu-id="56eb5-109">Validates that hello web API calls contain a valid access token.</span></span>

<span data-ttu-id="56eb5-110">toobuild hello tooDo rozhraní API seznamu, je nejprve třeba:</span><span class="sxs-lookup"><span data-stu-id="56eb5-110">toobuild hello tooDo List API, you first need to:</span></span>

1. <span data-ttu-id="56eb5-111">Zaregistrovat aplikaci s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="56eb5-111">Register an application with Azure AD.</span></span>
2. <span data-ttu-id="56eb5-112">Nastavte hello aplikace toouse hello OWIN ověřovacího kanálu.</span><span class="sxs-lookup"><span data-stu-id="56eb5-112">Set up hello app toouse hello OWIN authentication pipeline.</span></span>
3. <span data-ttu-id="56eb5-113">Konfigurace klienta aplikace toocall hello webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="56eb5-113">Configure a client application toocall hello web API.</span></span>

<span data-ttu-id="56eb5-114">spuštění, tooget [stáhnout kostru aplikace hello](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) nebo [stažení ukázky hello Dokončit](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="56eb5-114">tooget started, [download hello app skeleton](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) or [download hello completed sample](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="56eb5-115">Každá je řešením sady Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="56eb5-115">Each is a Visual Studio 2013 solution.</span></span> <span data-ttu-id="56eb5-116">Musíte taky klient služby Azure AD, ve které tooregister vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="56eb5-116">You also need an Azure AD tenant in which tooregister your application.</span></span> <span data-ttu-id="56eb5-117">Pokud již, nemáte [zjistěte, jak tooget jeden](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="56eb5-117">If you don't have one already, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

## <a name="step-1-register-an-application-with-azure-ad"></a><span data-ttu-id="56eb5-118">Krok 1: Zaregistrujte aplikaci s Azure AD</span><span class="sxs-lookup"><span data-stu-id="56eb5-118">Step 1: Register an application with Azure AD</span></span>
<span data-ttu-id="56eb5-119">toohelp zabezpečení vaší aplikace, je nejprve nutné toocreate aplikace ve vašem klientovi a Azure AD poskytují několik důležité informace.</span><span class="sxs-lookup"><span data-stu-id="56eb5-119">toohelp secure your application, you first need toocreate an application in your tenant and provide Azure AD with a few key pieces of information.</span></span>

1. <span data-ttu-id="56eb5-120">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="56eb5-120">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="56eb5-121">Na horním panelu hello klikněte na váš účet.</span><span class="sxs-lookup"><span data-stu-id="56eb5-121">On hello top bar, click your account.</span></span> <span data-ttu-id="56eb5-122">V hello **Directory** vyberte místo, kam chcete tooregister klienta hello Azure AD vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="56eb5-122">In hello **Directory** list, choose hello Azure AD tenant where you want tooregister your application.</span></span>

3. <span data-ttu-id="56eb5-123">Klikněte na tlačítko **více služeb** v levém podokně text hello a potom vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="56eb5-123">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>

4. <span data-ttu-id="56eb5-124">Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="56eb5-124">Click **App registrations**, and then select **Add**.</span></span>

5. <span data-ttu-id="56eb5-125">Postupujte podle pokynů hello a vytvořte novou **webové aplikace nebo webové rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="56eb5-125">Follow hello prompts and create a new **Web Application and/or Web API**.</span></span>
  * <span data-ttu-id="56eb5-126">**Název** popisuje toousers vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="56eb5-126">**Name** describes your application toousers.</span></span> <span data-ttu-id="56eb5-127">Zadejte **tooDo seznamu služby**.</span><span class="sxs-lookup"><span data-stu-id="56eb5-127">Enter **tooDo List Service**.</span></span>
  * <span data-ttu-id="56eb5-128">**Identifikátor Uri pro přesměrování** je kombinací schématu a řetězec používající tooreturn všechny tokeny, že vaše aplikace vyžaduje Azure AD.</span><span class="sxs-lookup"><span data-stu-id="56eb5-128">**Redirect Uri** is a scheme and string combination that Azure AD uses tooreturn any tokens that your app has requested.</span></span> <span data-ttu-id="56eb5-129">Zadejte `https://localhost:44321/` pro tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="56eb5-129">Enter `https://localhost:44321/` for this value.</span></span>

6. <span data-ttu-id="56eb5-130">Z hello **nastavení** -> **vlastnosti** stránky pro aplikace, aktualizujte hello identifikátor ID URI aplikace.</span><span class="sxs-lookup"><span data-stu-id="56eb5-130">From hello **Settings** -> **Properties** page for your application, update hello App ID URI.</span></span> <span data-ttu-id="56eb5-131">Zadejte identifikátor konkrétního klienta.</span><span class="sxs-lookup"><span data-stu-id="56eb5-131">Enter a tenant-specific identifier.</span></span> <span data-ttu-id="56eb5-132">Zadejte například `https://contoso.onmicrosoft.com/TodoListService`.</span><span class="sxs-lookup"><span data-stu-id="56eb5-132">For example, enter `https://contoso.onmicrosoft.com/TodoListService`.</span></span>

7. <span data-ttu-id="56eb5-133">Uložte konfiguraci hello.</span><span class="sxs-lookup"><span data-stu-id="56eb5-133">Save hello configuration.</span></span> <span data-ttu-id="56eb5-134">Ponechte hello portál otevřít, protože budete také potřebovat tooregister klientské aplikace za chvíli.</span><span class="sxs-lookup"><span data-stu-id="56eb5-134">Leave hello portal open, because you'll also need tooregister your client application shortly.</span></span>

## <a name="step-2-set-up-hello-app-toouse-hello-owin-authentication-pipeline"></a><span data-ttu-id="56eb5-135">Krok 2: Nastavení hello aplikace toouse hello OWIN ověřovacího kanálu</span><span class="sxs-lookup"><span data-stu-id="56eb5-135">Step 2: Set up hello app toouse hello OWIN authentication pipeline</span></span>
<span data-ttu-id="56eb5-136">toovalidate příchozí požadavky a tokeny, musíte tooset až toocommunicate vaší aplikace s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="56eb5-136">toovalidate incoming requests and tokens, you need tooset up your application toocommunicate with Azure AD.</span></span>

1. <span data-ttu-id="56eb5-137">toobegin, otevřete řešení hello a přidejte hello OWIN middleware NuGet balíčky toohello TodoListService projektu pomocí hello Konzola správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="56eb5-137">toobegin, open hello solution and add hello OWIN middleware NuGet packages toohello TodoListService project by using hello Package Manager Console.</span></span>

    ```
    PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
    PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
    ```

2. <span data-ttu-id="56eb5-138">Přidat OWIN při spuštění třída toohello TodoListService projekt s názvem `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="56eb5-138">Add an OWIN Startup class toohello TodoListService project called `Startup.cs`.</span></span>  <span data-ttu-id="56eb5-139">Klikněte pravým tlačítkem na projekt hello, vyberte **přidat** > **nová položka**a poté vyhledejte **OWIN**.</span><span class="sxs-lookup"><span data-stu-id="56eb5-139">Right-click hello project, select **Add** > **New Item**, and then search for **OWIN**.</span></span> <span data-ttu-id="56eb5-140">Hello OWIN middleware vyvolá hello `Configuration(…)` metoda při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="56eb5-140">hello OWIN middleware will invoke hello `Configuration(…)` method when your app starts.</span></span>

3. <span data-ttu-id="56eb5-141">Změňte deklaraci třídy hello příliš`public partial class Startup`.</span><span class="sxs-lookup"><span data-stu-id="56eb5-141">Change hello class declaration too`public partial class Startup`.</span></span> <span data-ttu-id="56eb5-142">Již implementovali jsme součástí této třídy pro vás v jiném souboru.</span><span class="sxs-lookup"><span data-stu-id="56eb5-142">We’ve already implemented part of this class for you in another file.</span></span> <span data-ttu-id="56eb5-143">V hello `Configuration(…)` metody se volat příliš`ConfgureAuth(…)` tooset až ověřování pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="56eb5-143">In hello `Configuration(…)` method, make a call too`ConfgureAuth(…)` tooset up authentication for your web app.</span></span>

    ```C#
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
    ```

4. <span data-ttu-id="56eb5-144">Soubor otevřete hello `App_Start\Startup.Auth.cs` a implementovat hello `ConfigureAuth(…)` metoda.</span><span class="sxs-lookup"><span data-stu-id="56eb5-144">Open hello file `App_Start\Startup.Auth.cs` and implement hello `ConfigureAuth(…)` method.</span></span> <span data-ttu-id="56eb5-145">Hello parametry, které zadáte v `WindowsAzureActiveDirectoryBearerAuthenticationOptions` bude sloužit jako souřadnice toocommunicate vaší aplikace s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="56eb5-145">hello parameters that you provide in `WindowsAzureActiveDirectoryBearerAuthenticationOptions` will serve as coordinates for your app toocommunicate with Azure AD.</span></span>

    ```C#
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseWindowsAzureActiveDirectoryBearerAuthentication(
            new WindowsAzureActiveDirectoryBearerAuthenticationOptions
            {
                Audience = ConfigurationManager.AppSettings["ida:Audience"],
                Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
            });
    }
    ```

5. <span data-ttu-id="56eb5-146">Nyní můžete pomocí `[Authorize]` atributy toohelp chránit řadiče a akce s ověřování nosiče tokenu pro webové JSON (JWT).</span><span class="sxs-lookup"><span data-stu-id="56eb5-146">Now you can use `[Authorize]` attributes toohelp protect your controllers and actions with JSON Web Token (JWT) bearer authentication.</span></span> <span data-ttu-id="56eb5-147">Uspořádání hello `Controllers\TodoListController.cs` se značky autorizovat.</span><span class="sxs-lookup"><span data-stu-id="56eb5-147">Decorate hello `Controllers\TodoListController.cs` class with an authorize tag.</span></span> <span data-ttu-id="56eb5-148">Tato akce vynutí hello toosign uživatele v před přístupem k této stránce.</span><span class="sxs-lookup"><span data-stu-id="56eb5-148">This will force hello user toosign in before accessing that page.</span></span>

    ```C#
    [Authorize]
    public class TodoListController : ApiController
    {
    ```

    <span data-ttu-id="56eb5-149">Když oprávnění volající úspěšně vyvolá jeden hello `TodoListController` rozhraní API, hello akce může potřebovat přístup k tooinformation o hello volajícího.</span><span class="sxs-lookup"><span data-stu-id="56eb5-149">When an authorized caller successfully invokes one of hello `TodoListController` APIs, hello action might need access tooinformation about hello caller.</span></span> <span data-ttu-id="56eb5-150">OWIN poskytuje přístup toohello deklarace identity uvnitř tokenu nosiče hello prostřednictvím hello `ClaimsPrincpal` objektu.</span><span class="sxs-lookup"><span data-stu-id="56eb5-150">OWIN provides access toohello claims inside hello bearer token via hello `ClaimsPrincpal` object.</span></span>  

6. <span data-ttu-id="56eb5-151">Běžné požadavky pro webové rozhraní API je toovalidate hello "obory" v hello token.</span><span class="sxs-lookup"><span data-stu-id="56eb5-151">A common requirement for web APIs is toovalidate hello "scopes" present in hello token.</span></span> <span data-ttu-id="56eb5-152">Tím se zajistí, že tento hello uživatel souhlasí toohello oprávnění požadované tooaccess hello tooDo seznamu služby.</span><span class="sxs-lookup"><span data-stu-id="56eb5-152">This ensures that hello user has consented toohello permissions required tooaccess hello tooDo List Service.</span></span>

    ```C#
    public IEnumerable<TodoItem> Get()
    {
        // user_impersonation is hello default permission exposed by applications in Azure AD
        if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
        {
            throw new HttpResponseException(new HttpResponseMessage {
              StatusCode = HttpStatusCode.Unauthorized,
              ReasonPhrase = "hello Scope claim does not contain 'user_impersonation' or scope claim not found"
            });
        }
        ...
    }
    ```

7. <span data-ttu-id="56eb5-153">Otevřete hello `web.config` souboru v kořenovém hello hello TodoListService projektu a zadejte hodnoty konfigurace v hello `<appSettings>` části.</span><span class="sxs-lookup"><span data-stu-id="56eb5-153">Open hello `web.config` file in hello root of hello TodoListService project, and enter your configuration values in hello `<appSettings>` section.</span></span>
  * <span data-ttu-id="56eb5-154">`ida:Tenant`je název hello klienta služby Azure AD – například contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="56eb5-154">`ida:Tenant` is hello name of your Azure AD tenant--for example, contoso.onmicrosoft.com.</span></span>
  * <span data-ttu-id="56eb5-155">`ida:Audience`identifikátor ID URI aplikace hello aplikace, kterou jste zadali v hello portál Azure je hello.</span><span class="sxs-lookup"><span data-stu-id="56eb5-155">`ida:Audience` is hello App ID URI of hello application that you entered in hello Azure portal.</span></span>

## <a name="step-3-configure-a-client-application-and-run-hello-service"></a><span data-ttu-id="56eb5-156">Krok 3: Konfigurace klientské aplikace a spustit službu hello</span><span class="sxs-lookup"><span data-stu-id="56eb5-156">Step 3: Configure a client application and run hello service</span></span>
<span data-ttu-id="56eb5-157">Než budete moct vidět hello tooDo seznamu služby v akci, musíte tooconfigure hello tooDo seznamu klienta, můžete získat tokeny z Azure AD a ujistěte se, volání toohello služby.</span><span class="sxs-lookup"><span data-stu-id="56eb5-157">Before you can see hello tooDo List Service in action, you need tooconfigure hello tooDo List client so it can get tokens from Azure AD and make calls toohello service.</span></span>

1. <span data-ttu-id="56eb5-158">Přejděte zpět toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="56eb5-158">Go back toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="56eb5-159">Vytvoření nové aplikace v klientovi služby Azure AD a vyberte **nativní klientská aplikace** hello výsledné řádku.</span><span class="sxs-lookup"><span data-stu-id="56eb5-159">Create a new application in your Azure AD tenant, and select **Native Client Application** in hello resulting prompt.</span></span>
  * <span data-ttu-id="56eb5-160">**Název** popisuje toousers vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="56eb5-160">**Name** describes your application toousers.</span></span>
  * <span data-ttu-id="56eb5-161">Zadejte `http://TodoListClient/` pro hello **identifikátor Uri pro přesměrování** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="56eb5-161">Enter `http://TodoListClient/` for hello **Redirect Uri** value.</span></span>

3. <span data-ttu-id="56eb5-162">Po dokončení registrace přiřadí Azure AD aplikace jedinečné ID tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="56eb5-162">After you finish registration, Azure AD assigns a unique application ID tooyour app.</span></span> <span data-ttu-id="56eb5-163">Tuto hodnotu budete potřebovat v dalších krocích hello, takže zkopírujte jej ze stránky aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="56eb5-163">You’ll need this value in hello next steps, so copy it from hello application page.</span></span>

4. <span data-ttu-id="56eb5-164">Z hello **nastavení** vyberte **požadovaných oprávnění**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="56eb5-164">From hello **Settings** page, select **Required Permissions**, and then select **Add**.</span></span> <span data-ttu-id="56eb5-165">Vyhledejte a vyberte hello tooDo seznamu služby, přidejte hello **přístup TodoListService** oprávnění v rámci **delegovaná oprávnění**a potom klikněte na **provádí**.</span><span class="sxs-lookup"><span data-stu-id="56eb5-165">Locate and select hello tooDo List Service, add hello **Access TodoListService** permission under **Delegated Permissions**, and then click **Done**.</span></span>

5. <span data-ttu-id="56eb5-166">V sadě Visual Studio otevřete `App.config` v hello TodoListClient projektu a potom zadejte hodnoty konfigurace v hello `<appSettings>` části.</span><span class="sxs-lookup"><span data-stu-id="56eb5-166">In Visual Studio, open `App.config` in hello TodoListClient project, and then enter your configuration values in hello `<appSettings>` section.</span></span>

  * <span data-ttu-id="56eb5-167">`ida:Tenant`je název hello klienta služby Azure AD – například contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="56eb5-167">`ida:Tenant` is hello name of your Azure AD tenant--for example, contoso.onmicrosoft.com.</span></span>
  * <span data-ttu-id="56eb5-168">`ida:ClientId`je hello ID aplikace, který jste zkopírovali ze hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="56eb5-168">`ida:ClientId` is hello app ID that you copied from hello Azure portal.</span></span>
  * <span data-ttu-id="56eb5-169">`todo:TodoListResourceId`je hello identifikátor ID URI aplikace z hello tooDo aplikace seznamu služby, kterou jste zadali v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="56eb5-169">`todo:TodoListResourceId` is hello App ID URI of hello tooDo List Service application that you entered in hello Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="56eb5-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="56eb5-170">Next steps</span></span>
<span data-ttu-id="56eb5-171">Nakonec vyčistit, sestavte a spusťte každý projekt.</span><span class="sxs-lookup"><span data-stu-id="56eb5-171">Finally, clean, build, and run each project.</span></span> <span data-ttu-id="56eb5-172">Pokud jste to ještě neudělali, nyní je čas toocreate hello nového uživatele ve vašem klientovi s *. onmicrosoft.com domény.</span><span class="sxs-lookup"><span data-stu-id="56eb5-172">If you haven’t already, now is hello time toocreate a new user in your tenant with a *.onmicrosoft.com domain.</span></span> <span data-ttu-id="56eb5-173">Přihlášení toohello tooDo seznamu klienta s tímto uživatelem a přidejte některé úlohy toohello seznam úkolů uživatele.</span><span class="sxs-lookup"><span data-stu-id="56eb5-173">Sign in toohello tooDo List client with that user, and add some tasks toohello user's to-do list.</span></span>

<span data-ttu-id="56eb5-174">Pro srovnání je k dispozici v hello dokončit ukázka (bez vašich hodnot nastavení) [Githubu](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="56eb5-174">For reference, hello completed sample (without your configuration values) is available in [GitHub](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="56eb5-175">Nyní se můžete přesunout na toomore identity scénáře.</span><span class="sxs-lookup"><span data-stu-id="56eb5-175">You can now move on toomore identity scenarios.</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
