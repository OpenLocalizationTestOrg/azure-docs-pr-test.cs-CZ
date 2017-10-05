---
title: "Azure AD .NET webové rozhraní API Začínáme | Microsoft Docs"
description: "Postup sestavení .NET MVC webového rozhraní API, který se integruje s Azure AD pro ověřování a autorizaci."
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
ms.openlocfilehash: f44d75f45073a5d9aa9b1863ed227aba4efcf785
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="help-protect-a-web-api-by-using-bearer-tokens-from-azure-ad"></a><span data-ttu-id="08799-103">Pomoc při ochraně webového rozhraní API pomocí nosných tokenů z Azure AD</span><span class="sxs-lookup"><span data-stu-id="08799-103">Help protect a web API by using bearer tokens from Azure AD</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="08799-104">Pokud vytváříte aplikace, která poskytuje přístup k chráněným prostředkům, musíte vědět, jak zabránit bude vyplacena neoprávněně přístupu na tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="08799-104">If you’re building an application that provides access to protected resources, you need to know how to prevent unwarranted access to those resources.</span></span>
<span data-ttu-id="08799-105">Díky Azure Active Directory (Azure AD) je jednoduchá a přímočará k ochraně webového rozhraní API pomocí přístupu k nosiče OAuth 2.0 tokeny jenom pár řádků kódu.</span><span class="sxs-lookup"><span data-stu-id="08799-105">Azure Active Directory (Azure AD) makes it simple and straightforward to help protect a web API by using OAuth 2.0 bearer access tokens with only a few lines of code.</span></span>

<span data-ttu-id="08799-106">V aplikacích ASP.NET web můžete provést tuto ochranu pomocí implementace Microsoft OWIN middleware komunitou vytvářený zahrnuté v rozhraní .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="08799-106">In ASP.NET web apps, you can accomplish this protection by using the Microsoft implementation of the community-driven OWIN middleware included in .NET Framework 4.5.</span></span> <span data-ttu-id="08799-107">Zde použijeme OWIN pro sestavení webového rozhraní API "Seznam úkolů", který:</span><span class="sxs-lookup"><span data-stu-id="08799-107">Here we’ll use OWIN to build a "To Do List" web API that:</span></span>

* <span data-ttu-id="08799-108">Označí, které rozhraní API jsou chráněny.</span><span class="sxs-lookup"><span data-stu-id="08799-108">Designates which APIs are protected.</span></span>
* <span data-ttu-id="08799-109">Ověří, že volání webového rozhraní API obsahovat platné přístupový token.</span><span class="sxs-lookup"><span data-stu-id="08799-109">Validates that the web API calls contain a valid access token.</span></span>

<span data-ttu-id="08799-110">Pokud chcete vytvořit pro provést seznamu rozhraní API, musíte nejprve:</span><span class="sxs-lookup"><span data-stu-id="08799-110">To build the To Do List API, you first need to:</span></span>

1. <span data-ttu-id="08799-111">Zaregistrovat aplikaci s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="08799-111">Register an application with Azure AD.</span></span>
2. <span data-ttu-id="08799-112">Nastavení aplikace k použití ověřovacího kanálu OWIN.</span><span class="sxs-lookup"><span data-stu-id="08799-112">Set up the app to use the OWIN authentication pipeline.</span></span>
3. <span data-ttu-id="08799-113">Konfigurovat klientskou aplikaci pro volání webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="08799-113">Configure a client application to call the web API.</span></span>

<span data-ttu-id="08799-114">Abyste mohli začít, [stáhnout kostru aplikace](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) nebo [stažení je hotová ukázka](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="08799-114">To get started, [download the app skeleton](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) or [download the completed sample](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="08799-115">Každá je řešením sady Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="08799-115">Each is a Visual Studio 2013 solution.</span></span> <span data-ttu-id="08799-116">Musíte taky klient služby Azure AD, do kterého chcete registrace vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="08799-116">You also need an Azure AD tenant in which to register your application.</span></span> <span data-ttu-id="08799-117">Pokud již, nemáte [zjistěte, jak získat](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="08799-117">If you don't have one already, [learn how to get one](active-directory-howto-tenant.md).</span></span>

## <a name="step-1-register-an-application-with-azure-ad"></a><span data-ttu-id="08799-118">Krok 1: Zaregistrujte aplikaci s Azure AD</span><span class="sxs-lookup"><span data-stu-id="08799-118">Step 1: Register an application with Azure AD</span></span>
<span data-ttu-id="08799-119">K zabezpečení vaší aplikace, musíte nejprve vytvořit aplikaci v klientovi a Azure AD poskytují několik důležité informace.</span><span class="sxs-lookup"><span data-stu-id="08799-119">To help secure your application, you first need to create an application in your tenant and provide Azure AD with a few key pieces of information.</span></span>

1. <span data-ttu-id="08799-120">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="08799-120">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="08799-121">Na horním panelu klikněte na váš účet.</span><span class="sxs-lookup"><span data-stu-id="08799-121">On the top bar, click your account.</span></span> <span data-ttu-id="08799-122">V **Directory** vyberte klienta Azure AD, kam chcete registrace vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="08799-122">In the **Directory** list, choose the Azure AD tenant where you want to register your application.</span></span>

3. <span data-ttu-id="08799-123">Klikněte na tlačítko **více služeb** v levém podokně a potom vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="08799-123">Click **More Services** in the left pane, and then select **Azure Active Directory**.</span></span>

4. <span data-ttu-id="08799-124">Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="08799-124">Click **App registrations**, and then select **Add**.</span></span>

5. <span data-ttu-id="08799-125">Postupujte podle výzev a vytvořte novou **webové aplikace nebo webové rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="08799-125">Follow the prompts and create a new **Web Application and/or Web API**.</span></span>
  * <span data-ttu-id="08799-126">**Název** popisuje vaší aplikace pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="08799-126">**Name** describes your application to users.</span></span> <span data-ttu-id="08799-127">Zadejte **seznam úkolů služby**.</span><span class="sxs-lookup"><span data-stu-id="08799-127">Enter **To Do List Service**.</span></span>
  * <span data-ttu-id="08799-128">**Identifikátor Uri pro přesměrování** je kombinace schéma a řetězec, Azure AD pomocí, která vrátí všechny tokeny, které vaše aplikace vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="08799-128">**Redirect Uri** is a scheme and string combination that Azure AD uses to return any tokens that your app has requested.</span></span> <span data-ttu-id="08799-129">Zadejte `https://localhost:44321/` pro tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="08799-129">Enter `https://localhost:44321/` for this value.</span></span>

6. <span data-ttu-id="08799-130">Z **nastavení** -> **vlastnosti** stránky pro aplikace, aktualizujte identifikátor ID URI aplikace.</span><span class="sxs-lookup"><span data-stu-id="08799-130">From the **Settings** -> **Properties** page for your application, update the App ID URI.</span></span> <span data-ttu-id="08799-131">Zadejte identifikátor konkrétního klienta.</span><span class="sxs-lookup"><span data-stu-id="08799-131">Enter a tenant-specific identifier.</span></span> <span data-ttu-id="08799-132">Zadejte například `https://contoso.onmicrosoft.com/TodoListService`.</span><span class="sxs-lookup"><span data-stu-id="08799-132">For example, enter `https://contoso.onmicrosoft.com/TodoListService`.</span></span>

7. <span data-ttu-id="08799-133">Uložte konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="08799-133">Save the configuration.</span></span> <span data-ttu-id="08799-134">Ponechte portálu otevřené, protože musíte také zaregistrovat klientské aplikace za chvíli.</span><span class="sxs-lookup"><span data-stu-id="08799-134">Leave the portal open, because you'll also need to register your client application shortly.</span></span>

## <a name="step-2-set-up-the-app-to-use-the-owin-authentication-pipeline"></a><span data-ttu-id="08799-135">Krok 2: Nastavení aplikace pro použití ověřovacího kanálu OWIN</span><span class="sxs-lookup"><span data-stu-id="08799-135">Step 2: Set up the app to use the OWIN authentication pipeline</span></span>
<span data-ttu-id="08799-136">Chcete-li ověřit příchozí požadavky a tokeny, nastavte aplikaci komunikovat s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="08799-136">To validate incoming requests and tokens, you need to set up your application to communicate with Azure AD.</span></span>

1. <span data-ttu-id="08799-137">Pokud chcete začít, otevřete řešení a přidání balíčků NuGet middleware OWIN do projektu TodoListService pomocí konzoly Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="08799-137">To begin, open the solution and add the OWIN middleware NuGet packages to the TodoListService project by using the Package Manager Console.</span></span>

    ```
    PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
    PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
    ```

2. <span data-ttu-id="08799-138">Přidejte třídu OWIN při spuštění do projektu TodoListService názvem `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="08799-138">Add an OWIN Startup class to the TodoListService project called `Startup.cs`.</span></span>  <span data-ttu-id="08799-139">Klikněte pravým tlačítkem na projekt, vyberte **přidat** > **nová položka**a poté vyhledejte **OWIN**.</span><span class="sxs-lookup"><span data-stu-id="08799-139">Right-click the project, select **Add** > **New Item**, and then search for **OWIN**.</span></span> <span data-ttu-id="08799-140">Middleware OWIN při spuštění vaší aplikace vyvolá metodu `Configuration(…)`.</span><span class="sxs-lookup"><span data-stu-id="08799-140">The OWIN middleware will invoke the `Configuration(…)` method when your app starts.</span></span>

3. <span data-ttu-id="08799-141">Změňte deklaraci třídy k `public partial class Startup`.</span><span class="sxs-lookup"><span data-stu-id="08799-141">Change the class declaration to `public partial class Startup`.</span></span> <span data-ttu-id="08799-142">Již implementovali jsme součástí této třídy pro vás v jiném souboru.</span><span class="sxs-lookup"><span data-stu-id="08799-142">We’ve already implemented part of this class for you in another file.</span></span> <span data-ttu-id="08799-143">V `Configuration(…)` metoda, zkontrolujte zavolá `ConfgureAuth(…)` nastavení ověřování pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="08799-143">In the `Configuration(…)` method, make a call to `ConfgureAuth(…)` to set up authentication for your web app.</span></span>

    ```C#
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
    ```

4. <span data-ttu-id="08799-144">Otevřete soubor `App_Start\Startup.Auth.cs` a implementovat `ConfigureAuth(…)` metoda.</span><span class="sxs-lookup"><span data-stu-id="08799-144">Open the file `App_Start\Startup.Auth.cs` and implement the `ConfigureAuth(…)` method.</span></span> <span data-ttu-id="08799-145">Parametry, které zadáte v `WindowsAzureActiveDirectoryBearerAuthenticationOptions` bude sloužit jako souřadnice pro vaši aplikaci komunikovat s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="08799-145">The parameters that you provide in `WindowsAzureActiveDirectoryBearerAuthenticationOptions` will serve as coordinates for your app to communicate with Azure AD.</span></span>

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

5. <span data-ttu-id="08799-146">Nyní můžete pomocí `[Authorize]` atributů k ochraně vašich řadičů a akce s ověřování nosiče tokenu pro webové JSON (JWT).</span><span class="sxs-lookup"><span data-stu-id="08799-146">Now you can use `[Authorize]` attributes to help protect your controllers and actions with JSON Web Token (JWT) bearer authentication.</span></span> <span data-ttu-id="08799-147">Uspořádání `Controllers\TodoListController.cs` se značky autorizovat.</span><span class="sxs-lookup"><span data-stu-id="08799-147">Decorate the `Controllers\TodoListController.cs` class with an authorize tag.</span></span> <span data-ttu-id="08799-148">Tato akce vynutí uživatele k přihlášení před přístupem k této stránce.</span><span class="sxs-lookup"><span data-stu-id="08799-148">This will force the user to sign in before accessing that page.</span></span>

    ```C#
    [Authorize]
    public class TodoListController : ApiController
    {
    ```

    <span data-ttu-id="08799-149">Když oprávnění volající úspěšně vyvolá jeden z `TodoListController` rozhraní API, akce může potřebovat přístup k informacím o volajícím.</span><span class="sxs-lookup"><span data-stu-id="08799-149">When an authorized caller successfully invokes one of the `TodoListController` APIs, the action might need access to information about the caller.</span></span> <span data-ttu-id="08799-150">OWIN poskytuje přístup k deklarace identity uvnitř tokenu nosiče prostřednictvím `ClaimsPrincpal` objektu.</span><span class="sxs-lookup"><span data-stu-id="08799-150">OWIN provides access to the claims inside the bearer token via the `ClaimsPrincpal` object.</span></span>  

6. <span data-ttu-id="08799-151">Běžné požadavky pro webová rozhraní API jsou ověření „oborů“ v tokenu.</span><span class="sxs-lookup"><span data-stu-id="08799-151">A common requirement for web APIs is to validate the "scopes" present in the token.</span></span> <span data-ttu-id="08799-152">Tím se zajistí, že uživatel souhlasí s tím oprávnění požadovaná pro přístup k provést seznamu službu.</span><span class="sxs-lookup"><span data-stu-id="08799-152">This ensures that the user has consented to the permissions required to access the To Do List Service.</span></span>

    ```C#
    public IEnumerable<TodoItem> Get()
    {
        // user_impersonation is the default permission exposed by applications in Azure AD
        if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
        {
            throw new HttpResponseException(new HttpResponseMessage {
              StatusCode = HttpStatusCode.Unauthorized,
              ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found"
            });
        }
        ...
    }
    ```

7. <span data-ttu-id="08799-153">Otevřete `web.config` souboru v kořenovém adresáři projektu TodoListService a zadejte svoje hodnoty konfigurace v `<appSettings>` oddílu.</span><span class="sxs-lookup"><span data-stu-id="08799-153">Open the `web.config` file in the root of the TodoListService project, and enter your configuration values in the `<appSettings>` section.</span></span>
  * <span data-ttu-id="08799-154">`ida:Tenant`je název vašeho klienta Azure AD – například contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="08799-154">`ida:Tenant` is the name of your Azure AD tenant--for example, contoso.onmicrosoft.com.</span></span>
  * <span data-ttu-id="08799-155">`ida:Audience`je identifikátor ID URI aplikace aplikace, která jste zadali v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="08799-155">`ida:Audience` is the App ID URI of the application that you entered in the Azure portal.</span></span>

## <a name="step-3-configure-a-client-application-and-run-the-service"></a><span data-ttu-id="08799-156">Krok 3: Konfigurace klientské aplikace a spustit službu</span><span class="sxs-lookup"><span data-stu-id="08799-156">Step 3: Configure a client application and run the service</span></span>
<span data-ttu-id="08799-157">Než budete moct vidět na udělat seznamu službu v akci, budete muset nakonfigurovat klienta seznam úkolů, aby mohl získat tokeny z Azure AD a provádět volání do služby.</span><span class="sxs-lookup"><span data-stu-id="08799-157">Before you can see the To Do List Service in action, you need to configure the To Do List client so it can get tokens from Azure AD and make calls to the service.</span></span>

1. <span data-ttu-id="08799-158">Přejděte zpět [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="08799-158">Go back to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="08799-159">Vytvoření nové aplikace v klientovi služby Azure AD a vyberte **nativní klientská aplikace** výsledné řádku.</span><span class="sxs-lookup"><span data-stu-id="08799-159">Create a new application in your Azure AD tenant, and select **Native Client Application** in the resulting prompt.</span></span>
  * <span data-ttu-id="08799-160">**Název** popisuje vaší aplikace pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="08799-160">**Name** describes your application to users.</span></span>
  * <span data-ttu-id="08799-161">Zadejte `http://TodoListClient/` pro **identifikátor Uri pro přesměrování** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="08799-161">Enter `http://TodoListClient/` for the **Redirect Uri** value.</span></span>

3. <span data-ttu-id="08799-162">Po dokončení registrace Azure AD jedinečný Identifikátor aplikace přiřadí vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="08799-162">After you finish registration, Azure AD assigns a unique application ID to your app.</span></span> <span data-ttu-id="08799-163">Tuto hodnotu budete potřebovat v dalších krocích, zkopírujte jej ze stránky aplikace.</span><span class="sxs-lookup"><span data-stu-id="08799-163">You’ll need this value in the next steps, so copy it from the application page.</span></span>

4. <span data-ttu-id="08799-164">Z **nastavení** vyberte **požadovaných oprávnění**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="08799-164">From the **Settings** page, select **Required Permissions**, and then select **Add**.</span></span> <span data-ttu-id="08799-165">Vyhledejte a vyberte k provést seznamu služby, přidejte **přístup TodoListService** oprávnění v rámci **delegovaná oprávnění**a potom klikněte na **provádí**.</span><span class="sxs-lookup"><span data-stu-id="08799-165">Locate and select the To Do List Service, add the **Access TodoListService** permission under **Delegated Permissions**, and then click **Done**.</span></span>

5. <span data-ttu-id="08799-166">V sadě Visual Studio otevřete `App.config` v TodoListClient projektu a pak zadejte svoje hodnoty konfigurace v `<appSettings>` oddílu.</span><span class="sxs-lookup"><span data-stu-id="08799-166">In Visual Studio, open `App.config` in the TodoListClient project, and then enter your configuration values in the `<appSettings>` section.</span></span>

  * <span data-ttu-id="08799-167">`ida:Tenant`je název vašeho klienta Azure AD – například contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="08799-167">`ida:Tenant` is the name of your Azure AD tenant--for example, contoso.onmicrosoft.com.</span></span>
  * <span data-ttu-id="08799-168">`ida:ClientId`je ID aplikace, kterou jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="08799-168">`ida:ClientId` is the app ID that you copied from the Azure portal.</span></span>
  * <span data-ttu-id="08799-169">`todo:TodoListResourceId`je identifikátor ID URI aplikace do služby seznamu se aplikace, která jste zadali v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="08799-169">`todo:TodoListResourceId` is the App ID URI of the To Do List Service application that you entered in the Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="08799-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="08799-170">Next steps</span></span>
<span data-ttu-id="08799-171">Nakonec vyčistit, sestavte a spusťte každý projekt.</span><span class="sxs-lookup"><span data-stu-id="08799-171">Finally, clean, build, and run each project.</span></span> <span data-ttu-id="08799-172">Pokud jste to ještě neudělali, nyní je čas vytvořit nového uživatele ve vašem klientovi s *. onmicrosoft.com domény.</span><span class="sxs-lookup"><span data-stu-id="08799-172">If you haven’t already, now is the time to create a new user in your tenant with a *.onmicrosoft.com domain.</span></span> <span data-ttu-id="08799-173">Přihlaste se na seznam úkolů klienta se tento uživatel a některé úlohy přidat do seznamu úkolů uživatele.</span><span class="sxs-lookup"><span data-stu-id="08799-173">Sign in to the To Do List client with that user, and add some tasks to the user's to-do list.</span></span>

<span data-ttu-id="08799-174">Pro srovnání je hotová ukázka (bez vašich hodnot nastavení) k dispozici v [Githubu](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="08799-174">For reference, the completed sample (without your configuration values) is available in [GitHub](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="08799-175">Nyní se můžete přesunout další identity scénářů.</span><span class="sxs-lookup"><span data-stu-id="08799-175">You can now move on to more identity scenarios.</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
