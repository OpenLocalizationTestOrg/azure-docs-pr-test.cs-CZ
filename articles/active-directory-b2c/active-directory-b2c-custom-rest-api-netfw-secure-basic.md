---
title: "Azure Active Directory B2C: Zabezpečte vaše RESTful služby pomocí základního ověřování protokolu HTTP"
description: "Zabezpečit vaše vlastní výměnu REST API deklarace identity ve vašem Azure AD B2C pomocí základního ověřování protokolu HTTP"
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 09/25/2017
ms.author: yoelh
ms.openlocfilehash: 0d4594f5e7c0a13d50993dd42d4780c1ba703140
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="secure-your-restful-services-by-using-http-basic-authentication"></a><span data-ttu-id="91465-103">Zabezpečení vašich služeb RESTful pomocí základního ověřování protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="91465-103">Secure your RESTful services by using HTTP basic authentication</span></span>
<span data-ttu-id="91465-104">V [souvisejícím článku Azure AD B2C](active-directory-b2c-custom-rest-api-netfw.md), vytvoření RESTful služby (webového rozhraní API), který se integruje se službou Azure Active Directory B2C cesty (Azure AD B2C) uživatele bez ověřování.</span><span class="sxs-lookup"><span data-stu-id="91465-104">In a [related Azure AD B2C article](active-directory-b2c-custom-rest-api-netfw.md), you create a RESTful service (web API) that integrates with Azure Active Directory B2C (Azure AD B2C) user journeys without authentication.</span></span> 

<span data-ttu-id="91465-105">V tomto článku základní ověřování protokolu HTTP přidat k RESTful službě tak, aby jen ověření uživatelé, včetně B2C, můžete přístup k rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="91465-105">In this article, you add HTTP basic authentication to your RESTful service so that only verified users, including B2C, can access your API.</span></span> <span data-ttu-id="91465-106">Základní ověřování protokolu HTTP nastavte přihlašovací údaje uživatele (ID aplikace a tajný klíč aplikace) ve vlastních zásadách.</span><span class="sxs-lookup"><span data-stu-id="91465-106">With HTTP basic authentication, you set the user credentials (app ID and app secret) in your custom policy.</span></span> 

<span data-ttu-id="91465-107">Další informace najdete v tématu [základní ověřování v rozhraní ASP.NET web API](https://docs.microsoft.com/aspnet/web-api/overview/security/basic-authentication).</span><span class="sxs-lookup"><span data-stu-id="91465-107">For more information, see [Basic authentication in ASP.NET web API](https://docs.microsoft.com/aspnet/web-api/overview/security/basic-authentication).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="91465-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="91465-108">Prerequisites</span></span>
<span data-ttu-id="91465-109">Proveďte kroky v [integrovat REST API deklarací výměn v vám dobře slouží uživatele Azure AD B2C](active-directory-b2c-custom-rest-api-netfw.md) článku.</span><span class="sxs-lookup"><span data-stu-id="91465-109">Complete the steps in the [Integrate REST API claims exchanges in your Azure AD B2C user journey](active-directory-b2c-custom-rest-api-netfw.md) article.</span></span>

## <a name="step-1-add-authentication-support"></a><span data-ttu-id="91465-110">Krok 1: Přidání Podpora ověřování</span><span class="sxs-lookup"><span data-stu-id="91465-110">Step 1: Add authentication support</span></span>

### <a name="step-11-add-application-settings-to-your-projects-webconfig-file"></a><span data-ttu-id="91465-111">Krok 1.1: Přidání nastavení aplikace do souboru web.config vašeho projektu</span><span class="sxs-lookup"><span data-stu-id="91465-111">Step 1.1: Add application settings to your project's web.config file</span></span>
1. <span data-ttu-id="91465-112">Otevřete projekt Visual Studio, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="91465-112">Open the Visual Studio project that you created earlier.</span></span> 

2. <span data-ttu-id="91465-113">Přidejte následující nastavení aplikace v souboru web.config v části `appSettings` element:</span><span class="sxs-lookup"><span data-stu-id="91465-113">Add the following application settings to the web.config file under the `appSettings` element:</span></span>

    ```XML
    <add key="WebApp:ClientId" value="B2CServiceUserAccount" />
    <add key="WebApp:ClientSecret" value="your secret" />
    ```

3. <span data-ttu-id="91465-114">Vytvořte heslo a potom nastavte `WebApp:ClientSecret` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="91465-114">Create a password, and then set the `WebApp:ClientSecret` value.</span></span>

    <span data-ttu-id="91465-115">Pokud chcete vygenerovat složité heslo, spusťte následující kód prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="91465-115">To generate a complex password, run the following PowerShell code.</span></span> <span data-ttu-id="91465-116">Můžete použít všechny libovolná hodnota.</span><span class="sxs-lookup"><span data-stu-id="91465-116">You can use any arbitrary value.</span></span>

    ```PowerShell
    $bytes = New-Object Byte[] 32
    $rand = [System.Security.Cryptography.RandomNumberGenerator]::Create()
    $rand.GetBytes($bytes)
    $rand.Dispose()
    [System.Convert]::ToBase64String($bytes)
    ```

### <a name="step-12-install-owin-libraries"></a><span data-ttu-id="91465-117">Krok 1.2: Instalace OWIN knihovny</span><span class="sxs-lookup"><span data-stu-id="91465-117">Step 1.2: Install OWIN libraries</span></span>
<span data-ttu-id="91465-118">Pokud chcete začít, přidejte do projektu balíčky NuGet middleware OWIN pomocí konzoly Správce balíčků Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="91465-118">To begin, add the OWIN middleware NuGet packages to the project by using the Visual Studio Package Manager Console:</span></span>

```
PM> Install-Package Microsoft.Owin
PM> Install-Package Owin
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

### <a name="step-13-add-an-authentication-middleware-class"></a><span data-ttu-id="91465-119">Krok 1.3: Přidejte třídu middleware ověřování</span><span class="sxs-lookup"><span data-stu-id="91465-119">Step 1.3: Add an authentication middleware class</span></span>
<span data-ttu-id="91465-120">Přidat `ClientAuthMiddleware.cs` třídy v části *App_Start* složky.</span><span class="sxs-lookup"><span data-stu-id="91465-120">Add the `ClientAuthMiddleware.cs` class under the *App_Start* folder.</span></span> <span data-ttu-id="91465-121">Postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="91465-121">To do so:</span></span>

1. <span data-ttu-id="91465-122">Klikněte pravým tlačítkem myši *App_Start* složky, vyberte **přidat**a potom vyberte **třída**.</span><span class="sxs-lookup"><span data-stu-id="91465-122">Right-click the *App_Start* folder, select **Add**, and then select **Class**.</span></span>

   ![Přidání třídy ClientAuthMiddleware.cs ve složce App_Start](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-OWIN-startup-auth1.png)

2. <span data-ttu-id="91465-124">V **název** zadejte **ClientAuthMiddleware.cs**.</span><span class="sxs-lookup"><span data-stu-id="91465-124">In the **Name** box, type **ClientAuthMiddleware.cs**.</span></span>

   ![Vytvořte novou třídu C#](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-OWIN-startup-auth2.png)

3. <span data-ttu-id="91465-126">Otevřete *App_Start\ClientAuthMiddleware.cs* souboru a nahradit soubor obsahu s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="91465-126">Open the *App_Start\ClientAuthMiddleware.cs* file, and replace the file content with following code:</span></span>

    ```csharp
    
    using Microsoft.Owin;
    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.Linq;
    using System.Security.Principal;
    using System.Text;
    using System.Threading.Tasks;
    using System.Web;
    
    namespace Contoso.AADB2C.API
    {
        /// <summary>
        /// Class to create a custom owin middleware to check for client authentication
        /// </summary>
        public class ClientAuthMiddleware
        {
            private static readonly string ClientID = ConfigurationManager.AppSettings["WebApp:ClientId"];
            private static readonly string ClientSecret = ConfigurationManager.AppSettings["WebApp:ClientSecret"];
    
            /// <summary>
            /// Gets or sets the next owin middleware
            /// </summary>
            private Func<IDictionary<string, object>, Task> Next { get; set; }
    
            /// <summary>
            /// Initializes a new instance of the <see cref="ClientAuthMiddleware"/> class.
            /// </summary>
            /// <param name="next"></param>
            public ClientAuthMiddleware(Func<IDictionary<string, object>, Task> next)
            {
                this.Next = next;
            }
    
            /// <summary>
            /// Invoke client authentication middleware during each request.
            /// </summary>
            /// <param name="environment">Owin environment</param>
            /// <returns></returns>
            public Task Invoke(IDictionary<string, object> environment)
            {
                // Get wrapper class for the environment
                var context = new OwinContext(environment);
    
                // Check whether the authorization header is available. This contains the credentials.
                var authzValue = context.Request.Headers.Get("Authorization");
                if (string.IsNullOrEmpty(authzValue) || !authzValue.StartsWith("Basic ", StringComparison.OrdinalIgnoreCase))
                {
                    // Process next middleware
                    return Next(environment);
                }
    
                // Get credentials
                var creds = authzValue.Substring("Basic ".Length).Trim();
                string clientId;
                string clientSecret;
    
                if (RetrieveCreds(creds, out clientId, out clientSecret))
                {
                    // Set transaction authenticated as client
                    context.Request.User = new GenericPrincipal(new GenericIdentity(clientId, "client"), new string[] { "client" });
                }
    
                return Next(environment);
            }
    
            /// <summary>
            /// Retrieve credentials from header
            /// </summary>
            /// <param name="credentials">Authorization header</param>
            /// <param name="clientId">Client identifier</param>
            /// <param name="clientSecret">Client secret</param>
            /// <returns>True if valid credentials were presented</returns>
            private bool RetrieveCreds(string credentials, out string clientId, out string clientSecret)
            {
                string pair;
                clientId = clientSecret = string.Empty;
    
                try
                {
                    pair = Encoding.UTF8.GetString(Convert.FromBase64String(credentials));
                }
                catch (FormatException)
                {
                    return false;
                }
                catch (ArgumentException)
                {
                    return false;
                }
    
                var ix = pair.IndexOf(':');
                if (ix == -1)
                {
                    return false;
                }
    
                clientId = pair.Substring(0, ix);
                clientSecret = pair.Substring(ix + 1);
    
                // Return whether credentials are valid
                return (string.Compare(clientId, ClientAuthMiddleware.ClientID) == 0 &&
                    string.Compare(clientSecret, ClientAuthMiddleware.ClientSecret) == 0);
            }
        }
    }
    ```

### <a name="step-14-add-an-owin-startup-class"></a><span data-ttu-id="91465-127">Krok 1.4: Přidání třídy pro spuštění OWIN</span><span class="sxs-lookup"><span data-stu-id="91465-127">Step 1.4: Add an OWIN startup class</span></span>
<span data-ttu-id="91465-128">Přidání třídy pro spuštění OWIN s názvem `Startup.cs` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="91465-128">Add an OWIN startup class named `Startup.cs` to the API.</span></span> <span data-ttu-id="91465-129">Postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="91465-129">To do so:</span></span>
1. <span data-ttu-id="91465-130">Klikněte pravým tlačítkem na projekt, vyberte **přidat** > **nová položka**a poté vyhledejte **OWIN**.</span><span class="sxs-lookup"><span data-stu-id="91465-130">Right-click the project, select **Add** > **New Item**, and then search for **OWIN**.</span></span>

   ![Přidání třídy pro spuštění OWIN](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-OWIN-startup.png)

2. <span data-ttu-id="91465-132">Otevřete *Startup.cs* souboru a nahradit soubor obsahu s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="91465-132">Open the *Startup.cs* file, and replace the file content with following code:</span></span>

    ```csharp
    using Microsoft.Owin;
    using Owin;
    
    [assembly: OwinStartup(typeof(Contoso.AADB2C.API.Startup))]
    namespace Contoso.AADB2C.API
    {
        public class Startup
        {
            public void Configuration(IAppBuilder app)
            {
                    app.Use<ClientAuthMiddleware>();
            }
        }
    }
    ```

### <a name="step-15-protect-the-identity-api-class"></a><span data-ttu-id="91465-133">Krok 1.5: Ochrana rozhraní API Identity – třída</span><span class="sxs-lookup"><span data-stu-id="91465-133">Step 1.5: Protect the Identity API class</span></span>
<span data-ttu-id="91465-134">Otevřete Controllers\IdentityController.cs a přidejte `[Authorize]` značka do třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="91465-134">Open Controllers\IdentityController.cs, and add the `[Authorize]` tag to the controller class.</span></span> <span data-ttu-id="91465-135">Tato značka omezuje přístup k řadiči pro uživatele, kteří splní požadavek na autorizaci.</span><span class="sxs-lookup"><span data-stu-id="91465-135">This tag restricts access to the controller to users who meet the authorization requirement.</span></span>

![Přidání značky autorizovat do kontroleru](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-authorize.png)

## <a name="step-2-publish-to-azure"></a><span data-ttu-id="91465-137">Krok 2: Publikování aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="91465-137">Step 2: Publish to Azure</span></span>
<span data-ttu-id="91465-138">K publikování projektu v Průzkumníku řešení, klikněte pravým tlačítkem myši **Contoso.AADB2C.API** projektu a potom vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="91465-138">To publish your project, in Solution Explorer, right-click the **Contoso.AADB2C.API** project, and then select **Publish**.</span></span>

## <a name="step-3-add-the-restful-services-app-id-and-app-secret-to-azure-ad-b2c"></a><span data-ttu-id="91465-139">Krok 3: Přidání služeb RESTful ID a aplikace tajný klíč aplikace do Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="91465-139">Step 3: Add the RESTful services app ID and app secret to Azure AD B2C</span></span>
<span data-ttu-id="91465-140">Jakmile služby RESTful je chráněný pomocí ID klienta (username) a tajný klíč, je třeba uložit přihlašovací údaje v klientovi služby Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="91465-140">After your RESTful service is protected by the client ID (username) and secret, you must store the credentials in your Azure AD B2C tenant.</span></span> <span data-ttu-id="91465-141">Vaše vlastní zásada poskytuje pověření, pokud vyvolá vaší služby RESTful.</span><span class="sxs-lookup"><span data-stu-id="91465-141">Your custom policy provides the credentials when it invokes your RESTful services.</span></span> 

### <a name="step-31-add-a-restful-services-client-id"></a><span data-ttu-id="91465-142">Krok 3.1: ID klienta služby RESTful přidáte</span><span class="sxs-lookup"><span data-stu-id="91465-142">Step 3.1: Add a RESTful services client ID</span></span>
1. <span data-ttu-id="91465-143">Ve vašem klientu Azure AD B2C, vyberte **nastavení B2C** > **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="91465-143">In your Azure AD B2C tenant, select **B2C Settings** > **Identity Experience Framework**.</span></span>


2. <span data-ttu-id="91465-144">Vyberte **zásad klíče** zobrazíte klíčů, které jsou k dispozici ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="91465-144">Select **Policy Keys** to view the keys that are available in your tenant.</span></span>

3. <span data-ttu-id="91465-145">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="91465-145">Select **Add**.</span></span>

4. <span data-ttu-id="91465-146">Pro **možnosti**, vyberte **ruční**.</span><span class="sxs-lookup"><span data-stu-id="91465-146">For **Options**, select **Manual**.</span></span>

5. <span data-ttu-id="91465-147">Pro **název**, typ **B2cRestClientId**.</span><span class="sxs-lookup"><span data-stu-id="91465-147">For **Name**, type **B2cRestClientId**.</span></span>  
    <span data-ttu-id="91465-148">Předpona *B2C_1A_* může automaticky přidat.</span><span class="sxs-lookup"><span data-stu-id="91465-148">The prefix *B2C_1A_* might be added automatically.</span></span>

6. <span data-ttu-id="91465-149">V **tajný klíč** zadejte ID aplikace, kterou jste definovali dříve.</span><span class="sxs-lookup"><span data-stu-id="91465-149">In the **Secret** box, enter the app ID that you defined earlier.</span></span>

7. <span data-ttu-id="91465-150">Pro **použití klíče**, vyberte **tajný klíč**.</span><span class="sxs-lookup"><span data-stu-id="91465-150">For **Key usage**, select **Secret**.</span></span>

8. <span data-ttu-id="91465-151">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="91465-151">Select **Create**.</span></span>

9. <span data-ttu-id="91465-152">Potvrďte, že jste vytvořili `B2C_1A_B2cRestClientId` klíč.</span><span class="sxs-lookup"><span data-stu-id="91465-152">Confirm that you've created the `B2C_1A_B2cRestClientId` key.</span></span>

### <a name="step-32-add-a-restful-services-client-secret"></a><span data-ttu-id="91465-153">Krok 3.2: Přidejte sdílený tajný klíč klienta služby RESTful</span><span class="sxs-lookup"><span data-stu-id="91465-153">Step 3.2: Add a RESTful services client secret</span></span>
1. <span data-ttu-id="91465-154">Ve vašem klientu Azure AD B2C, vyberte **nastavení B2C** > **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="91465-154">In your Azure AD B2C tenant, select **B2C Settings** > **Identity Experience Framework**.</span></span>

2. <span data-ttu-id="91465-155">Vyberte **zásad klíče** zobrazíte klíče, které jsou k dispozici ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="91465-155">Select **Policy Keys** to view the keys available in your tenant.</span></span>

3. <span data-ttu-id="91465-156">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="91465-156">Select **Add**.</span></span>

4. <span data-ttu-id="91465-157">Pro **možnosti**, vyberte **ruční**.</span><span class="sxs-lookup"><span data-stu-id="91465-157">For **Options**, select **Manual**.</span></span>

5. <span data-ttu-id="91465-158">Pro **název**, typ **B2cRestClientSecret**.</span><span class="sxs-lookup"><span data-stu-id="91465-158">For **Name**, type **B2cRestClientSecret**.</span></span>  
    <span data-ttu-id="91465-159">Předpona *B2C_1A_* může automaticky přidat.</span><span class="sxs-lookup"><span data-stu-id="91465-159">The prefix *B2C_1A_* might be added automatically.</span></span>

6. <span data-ttu-id="91465-160">V **tajný klíč** zadejte tajný klíč aplikace, kterou jste definovali dříve.</span><span class="sxs-lookup"><span data-stu-id="91465-160">In the **Secret** box, enter the app secret that you defined earlier.</span></span>

7. <span data-ttu-id="91465-161">Pro **použití klíče**, vyberte **tajný klíč**.</span><span class="sxs-lookup"><span data-stu-id="91465-161">For **Key usage**, select **Secret**.</span></span>

8. <span data-ttu-id="91465-162">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="91465-162">Select **Create**.</span></span>

9. <span data-ttu-id="91465-163">Potvrďte, že jste vytvořili `B2C_1A_B2cRestClientSecret` klíč.</span><span class="sxs-lookup"><span data-stu-id="91465-163">Confirm that you've created the `B2C_1A_B2cRestClientSecret` key.</span></span>

## <a name="step-4-change-the-technical-profile-to-support-basic-authentication-in-your-extension-policy"></a><span data-ttu-id="91465-164">Krok 4: Změňte profilem technické podpory základní ověřování v zásadě rozšíření</span><span class="sxs-lookup"><span data-stu-id="91465-164">Step 4: Change the technical profile to support basic authentication in your extension policy</span></span>
1. <span data-ttu-id="91465-165">V pracovním adresáři otevřete soubor rozšíření zásad (TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="91465-165">In your working directory, open the extension policy file (TrustFrameworkExtensions.xml).</span></span>

2. <span data-ttu-id="91465-166">Vyhledejte `<TechnicalProfile>` uzlu, který zahrnuje `Id="REST-API-SignUp"`.</span><span class="sxs-lookup"><span data-stu-id="91465-166">Search for the `<TechnicalProfile>` node that includes `Id="REST-API-SignUp"`.</span></span>

3. <span data-ttu-id="91465-167">Vyhledejte `<Metadata>` elementu.</span><span class="sxs-lookup"><span data-stu-id="91465-167">Locate the `<Metadata>` element.</span></span>

4. <span data-ttu-id="91465-168">Změna *AuthenticationType* k *základní*, a to takto:</span><span class="sxs-lookup"><span data-stu-id="91465-168">Change the *AuthenticationType* to *Basic*, as follows:</span></span>
    ```xml
    <Item Key="AuthenticationType">Basic</Item>
    ```

5. <span data-ttu-id="91465-169">Ihned po zavření `<Metadata>` elementu, přidejte následující fragment kódu XML:</span><span class="sxs-lookup"><span data-stu-id="91465-169">Immediately after the closing `<Metadata>` element, add the following XML snippet:</span></span> 

    ```xml
    <CryptographicKeys>
        <Key Id="BasicAuthenticationUsername" StorageReferenceId="B2C_1A_B2cRestClientId" />
        <Key Id="BasicAuthenticationPassword" StorageReferenceId="B2C_1A_B2cRestClientSecret" />
    </CryptographicKeys>
    ```
    <span data-ttu-id="91465-170">Po přidání fragmentu váš technické profil by měl vypadat podobně jako následující kód XML:</span><span class="sxs-lookup"><span data-stu-id="91465-170">After you add the snippet, your technical profile should look like the following XML code:</span></span>
    
    ![Přidat elementů XML základní ověřování](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-add-1.png)

## <a name="step-5-upload-the-policy-to-your-tenant"></a><span data-ttu-id="91465-172">Krok 5: Nahrajte zásady klienta</span><span class="sxs-lookup"><span data-stu-id="91465-172">Step 5: Upload the policy to your tenant</span></span>

1. <span data-ttu-id="91465-173">V [portál Azure](https://portal.azure.com), přepnout [kontextu klienta služby Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)a pak otevřete **Azure AD B2C**.</span><span class="sxs-lookup"><span data-stu-id="91465-173">In the [Azure portal](https://portal.azure.com), switch to the [context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and then open **Azure AD B2C**.</span></span>

2. <span data-ttu-id="91465-174">Vyberte **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="91465-174">Select **Identity Experience Framework**.</span></span>

3. <span data-ttu-id="91465-175">Otevřete **všechny zásady**.</span><span class="sxs-lookup"><span data-stu-id="91465-175">Open **All Policies**.</span></span>

4. <span data-ttu-id="91465-176">Vyberte **nahrát zásady**.</span><span class="sxs-lookup"><span data-stu-id="91465-176">Select **Upload Policy**.</span></span>

5. <span data-ttu-id="91465-177">Vyberte **přepsat zásady, pokud existuje** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="91465-177">Select the **Overwrite the policy if it exists** check box.</span></span>

6. <span data-ttu-id="91465-178">Nahrát *TrustFrameworkExtensions.xml* souboru a pak se ujistěte, že předává ověření.</span><span class="sxs-lookup"><span data-stu-id="91465-178">Upload the *TrustFrameworkExtensions.xml* file, and then ensure that it passes validation.</span></span>

## <a name="step-6-test-the-custom-policy-by-using-run-now"></a><span data-ttu-id="91465-179">Krok 6: Testování spustit pomocí vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="91465-179">Step 6: Test the custom policy by using Run Now</span></span>
1. <span data-ttu-id="91465-180">Otevřete **nastavení Azure AD B2C**a potom vyberte **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="91465-180">Open **Azure AD B2C Settings**, and then select **Identity Experience Framework**.</span></span>

    >[!NOTE]
    ><span data-ttu-id="91465-181">Spustit nyní vyžaduje alespoň jedné aplikace do být preregistered u klienta.</span><span class="sxs-lookup"><span data-stu-id="91465-181">Run Now requires at least one application to be preregistered on the tenant.</span></span> <span data-ttu-id="91465-182">Další postup registrace aplikace najdete v tématu Azure AD B2C [Začínáme](active-directory-b2c-get-started.md) článek nebo [registrace aplikace](active-directory-b2c-app-registration.md) článku.</span><span class="sxs-lookup"><span data-stu-id="91465-182">To learn how to register applications, see the Azure AD B2C [Get started](active-directory-b2c-get-started.md) article or the [Application registration](active-directory-b2c-app-registration.md) article.</span></span>

2. <span data-ttu-id="91465-183">Otevřete **B2C_1A_signup_signin**, předávající stranu vlastních zásad, které jste nahráli a potom vyberte **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="91465-183">Open **B2C_1A_signup_signin**, the relying party (RP) custom policy that you uploaded, and then select **Run now**.</span></span>

3. <span data-ttu-id="91465-184">Otestujte proces zadáním **Test** v **křestní jméno** pole.</span><span class="sxs-lookup"><span data-stu-id="91465-184">Test the process by typing **Test** in the **Given Name** box.</span></span>  
    <span data-ttu-id="91465-185">Azure AD B2C zobrazí chybovou zprávu v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="91465-185">Azure AD B2C displays an error message at the top of the window.</span></span>

    ![Testování vaší identity rozhraní API](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-test.png)

4. <span data-ttu-id="91465-187">V **křestní jméno** zadejte název (jiné než "Test").</span><span class="sxs-lookup"><span data-stu-id="91465-187">In the **Given Name** box, type a name (other than "Test").</span></span>  
    <span data-ttu-id="91465-188">Azure AD B2C zaregistruje uživatele a pak odešle číslo věrného do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="91465-188">Azure AD B2C signs up the user and then sends a loyalty number to your application.</span></span> <span data-ttu-id="91465-189">Poznamenejte si číslo v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="91465-189">Note the number in this example:</span></span>

    ```
    {
      "typ": "JWT",
      "alg": "RS256",
      "kid": "X5eXk4xyojNFum1kl2Ytv8dlNP4-c57dO6QGTVBwaNk"
    }.{
      "exp": 1507125903,
      "nbf": 1507122303,
      "ver": "1.0",
      "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
      "aud": "e1d2612f-c2bc-4599-8e7b-d874eaca1ee1",
      "acr": "b2c_1a_signup_signin",
      "nonce": "defaultNonce",
      "iat": 1507122303,
      "auth_time": 1507122303,
      "loyaltyNumber": "290",
      "given_name": "Emily",
      "emails": ["B2cdemo@outlook.com"]
    }
    ```

## <a name="optional-download-the-complete-policy-files-and-code"></a><span data-ttu-id="91465-190">(Volitelné) Stáhnout soubory dokončení zásad a kódu</span><span class="sxs-lookup"><span data-stu-id="91465-190">(Optional) Download the complete policy files and code</span></span>
* <span data-ttu-id="91465-191">Po dokončení [začít pracovat s vlastními zásadami](active-directory-b2c-get-started-custom.md) návod, doporučujeme vám vytvořit váš scénář pomocí vlastních zásad pro soubory.</span><span class="sxs-lookup"><span data-stu-id="91465-191">After you complete the [Get started with custom policies](active-directory-b2c-get-started-custom.md) walkthrough, we recommend that you build your scenario by using your own custom policy files.</span></span> <span data-ttu-id="91465-192">Pro vaši informaci uvádíme [ukázkové soubory zásad](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw-secure-basic).</span><span class="sxs-lookup"><span data-stu-id="91465-192">For your reference, we have provided [Sample policy files](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw-secure-basic).</span></span>
* <span data-ttu-id="91465-193">Si můžete stáhnout kompletní kód z [řešení sady Visual Studio ukázkový pro referenci](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw/Contoso.AADB2C.API).</span><span class="sxs-lookup"><span data-stu-id="91465-193">You can download the complete code from [Sample Visual Studio solution for reference](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw/Contoso.AADB2C.API).</span></span>

## <a name="next-steps"></a><span data-ttu-id="91465-194">Další postup</span><span class="sxs-lookup"><span data-stu-id="91465-194">Next steps</span></span>
* [<span data-ttu-id="91465-195">Používat klientské certifikáty k zabezpečení rozhraní RESTful API.</span><span class="sxs-lookup"><span data-stu-id="91465-195">Use client certificates to secure your RESTful API</span></span>](active-directory-b2c-custom-rest-api-netfw-secure-cert.md)

