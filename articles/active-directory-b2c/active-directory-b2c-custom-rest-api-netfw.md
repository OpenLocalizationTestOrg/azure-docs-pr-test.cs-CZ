---
title: "Azure Active Directory B2C: Integrace rozhraní REST API deklarace identity výměn v vám dobře slouží Azure AD B2C uživatele jako ověřování vstupu uživatele"
description: "Integrace rozhraní REST API deklarace identity výměn v vám dobře slouží Azure AD B2C uživatele jako ověřování vstupu uživatele."
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
ms.date: 09/30/2017
ms.author: yoelh
ms.openlocfilehash: fd9c95ae78590aa772fde10c8c80914c905767a8
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-validation-of-user-input"></a><span data-ttu-id="32d41-103">Integrace rozhraní REST API deklarace identity výměn v vám dobře slouží Azure AD B2C uživatele jako ověřování vstupu uživatele</span><span class="sxs-lookup"><span data-stu-id="32d41-103">Integrate REST API claims exchanges in your Azure AD B2C user journey as validation of user input</span></span>
<span data-ttu-id="32d41-104">S použitím rozhraní Framework Identity, které základem Azure Active Directory B2C (Azure AD B2C), kterou můžete integrovat se rozhraní RESTful API cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="32d41-104">With the Identity Experience Framework, which underlies Azure Active Directory B2C (Azure AD B2C), you can integrate with a RESTful API in a user journey.</span></span> <span data-ttu-id="32d41-105">V tomto návodu se dozvíte, jak Azure AD B2C komunikuje s rozhraní .NET Framework RESTful služeb (webové rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="32d41-105">In this walkthrough, you'll learn how Azure AD B2C interacts with .NET Framework RESTful services (web API).</span></span>

## <a name="introduction"></a><span data-ttu-id="32d41-106">Úvod</span><span class="sxs-lookup"><span data-stu-id="32d41-106">Introduction</span></span>
<span data-ttu-id="32d41-107">Pomocí Azure AD B2C můžete přidat vlastní obchodní logiku cesty uživatele při volání služby RESTful.</span><span class="sxs-lookup"><span data-stu-id="32d41-107">By using Azure AD B2C, you can add your own business logic to a user journey by calling your own RESTful service.</span></span> <span data-ttu-id="32d41-108">Rozhraní Framework prostředí Identity odešle data do služby RESTful *vstupní deklarace identity* kolekce a přijímá data zpět z dosáhl standardu RESTful v *výstup deklarace identity* kolekce.</span><span class="sxs-lookup"><span data-stu-id="32d41-108">The Identity Experience Framework sends data to the RESTful service in an *Input claims* collection and receives data back from RESTful in an *Output claims* collection.</span></span> <span data-ttu-id="32d41-109">Díky integraci služba RESTful můžete:</span><span class="sxs-lookup"><span data-stu-id="32d41-109">With RESTful service integration, you can:</span></span>

* <span data-ttu-id="32d41-110">**Ověření uživatele vstupní data**: Tato akce zabrání poškozených datových uložením do služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="32d41-110">**Validate user input data**: This action prevents malformed data from persisting into Azure AD.</span></span> <span data-ttu-id="32d41-111">Pokud hodnota od uživatele není platná, služby RESTful vrátí chybovou zprávu, která se dá pokyn, aby uživatel zadal položku.</span><span class="sxs-lookup"><span data-stu-id="32d41-111">If the value from the user is not valid, your RESTful service returns an error message that instructs the user to provide an entry.</span></span> <span data-ttu-id="32d41-112">Ověřte například, že e-mailovou adresu poskytl uživatel existuje v databázi vašeho zákazníka.</span><span class="sxs-lookup"><span data-stu-id="32d41-112">For example, you can verify that the email address provided by the user exists in your customer's database.</span></span>
* <span data-ttu-id="32d41-113">**Přepsat vstupních deklarací identity**: například, pokud uživatel zadá název první v celé malými písmeny a všechna písmena velká písmena, můžete ho naformátovat název s velkými písmeny pouze prvních písmen.</span><span class="sxs-lookup"><span data-stu-id="32d41-113">**Overwrite input claims**: For example, if a user enters the first name in all lowercase or all uppercase letters, you can format the name with only the first letter capitalized.</span></span>
* <span data-ttu-id="32d41-114">**Rozšíření dat uživatele integrací další s podnikovým aplikacím obchodní**: vaše RESTful služby může přijímat e-mailovou adresu uživatele, dotazování databáze zákazníka a vrátí číslo věrného uživatele do Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="32d41-114">**Enrich user data by further integrating with corporate line-of-business applications**: Your RESTful service can receive the user's email address, query the customer's database, and return the user's loyalty number to Azure AD B2C.</span></span> <span data-ttu-id="32d41-115">Návratový deklarace identity můžou být uložené v účtu uživatele Azure AD, vyhodnotí v dalším *kroků Orchestrace*, nebo zahrnuté v tokenu přístupu.</span><span class="sxs-lookup"><span data-stu-id="32d41-115">The return claims can be stored in the user's Azure AD account, evaluated in the next *Orchestration Steps*, or included in the access token.</span></span>
* <span data-ttu-id="32d41-116">**Spusťte vlastní obchodní logiku**: můžete odesílat nabízená oznámení, aktualizovat podnikové databáze, spusťte proces migrace uživatele, správě oprávnění, audit databáze a provádět další akce.</span><span class="sxs-lookup"><span data-stu-id="32d41-116">**Run custom business logic**: You can send push notifications, update corporate databases, run a user migration process, manage permissions, audit databases, and perform other actions.</span></span>

<span data-ttu-id="32d41-117">Integrace s služeb RESTful můžete navrhnout následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="32d41-117">You can design the integration with the RESTful services in the following ways:</span></span>

* <span data-ttu-id="32d41-118">**Profil ověření technické**: volání službu RESTful se stane v rámci profilu ověření technické zadaný technické profilu.</span><span class="sxs-lookup"><span data-stu-id="32d41-118">**Validation technical profile**: The call to the RESTful service happens within the validation technical profile of the specified technical profile.</span></span> <span data-ttu-id="32d41-119">Technické profil ověření ověří zadaný uživatelem data před cesty uživatel přesune dál.</span><span class="sxs-lookup"><span data-stu-id="32d41-119">The validation technical profile validates the user-provided data before the user journey moves forward.</span></span> <span data-ttu-id="32d41-120">Technické profil ověření můžete:</span><span class="sxs-lookup"><span data-stu-id="32d41-120">With the validation technical profile, you can:</span></span>
   * <span data-ttu-id="32d41-121">Odešlete vstupní deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="32d41-121">Send input claims.</span></span>
   * <span data-ttu-id="32d41-122">Ověření mezi vstupními deklaracemi identity a výjimku vlastní chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="32d41-122">Validate the input claims and throw custom error messages.</span></span>
   * <span data-ttu-id="32d41-123">Odešlete deklarace identity back výstup.</span><span class="sxs-lookup"><span data-stu-id="32d41-123">Send back output claims.</span></span>

* <span data-ttu-id="32d41-124">**Deklarace identity exchange**: Tento návrh se podobá technické profil ověření, ale Odehrává se v rámci na krok orchestration.</span><span class="sxs-lookup"><span data-stu-id="32d41-124">**Claims exchange**: This design is similar to the validation technical profile, but it happens within an orchestration step.</span></span> <span data-ttu-id="32d41-125">Tato definice je omezený na:</span><span class="sxs-lookup"><span data-stu-id="32d41-125">This definition is limited to:</span></span>
   * <span data-ttu-id="32d41-126">Odešlete vstupní deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="32d41-126">Send input claims.</span></span>
   * <span data-ttu-id="32d41-127">Odešlete deklarace identity back výstup.</span><span class="sxs-lookup"><span data-stu-id="32d41-127">Send back output claims.</span></span>

## <a name="restful-walkthrough"></a><span data-ttu-id="32d41-128">RESTful návod</span><span class="sxs-lookup"><span data-stu-id="32d41-128">RESTful walkthrough</span></span>
<span data-ttu-id="32d41-129">V tomto návodu vytvořte rozhraní .NET Framework webového rozhraní API, která ověří vstup uživatele a poskytuje věrného číslo uživatele.</span><span class="sxs-lookup"><span data-stu-id="32d41-129">In this walkthrough, you develop a .NET Framework web API that validates the user input and provides a user loyalty number.</span></span> <span data-ttu-id="32d41-130">Aplikace může například udělit přístup k *platinové výhody* podle počtu věrného.</span><span class="sxs-lookup"><span data-stu-id="32d41-130">For example, your application can grant access to *platinum benefits* based on the loyalty number.</span></span>

<span data-ttu-id="32d41-131">Přehled:</span><span class="sxs-lookup"><span data-stu-id="32d41-131">Overview:</span></span>
* <span data-ttu-id="32d41-132">Vytvořte službu RESTful (rozhraní .NET Framework webového rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="32d41-132">Develop the RESTful service (.NET Framework web API).</span></span>
* <span data-ttu-id="32d41-133">Použijte službu RESTful v cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="32d41-133">Use the RESTful service in the user journey.</span></span>
* <span data-ttu-id="32d41-134">Odesílat vstupní deklarace identity a přečtěte si je v kódu.</span><span class="sxs-lookup"><span data-stu-id="32d41-134">Send input claims and read them in your code.</span></span>
* <span data-ttu-id="32d41-135">Ověřte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="32d41-135">Validate the user's first name.</span></span>
* <span data-ttu-id="32d41-136">Odešlete zpět věrného číslo.</span><span class="sxs-lookup"><span data-stu-id="32d41-136">Send back a loyalty number.</span></span> 
* <span data-ttu-id="32d41-137">Přidejte číslo věrného k JSON Web Token (JWT).</span><span class="sxs-lookup"><span data-stu-id="32d41-137">Add the loyalty number to a JSON Web Token (JWT).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="32d41-138">Požadavky</span><span class="sxs-lookup"><span data-stu-id="32d41-138">Prerequisites</span></span>
<span data-ttu-id="32d41-139">Proveďte kroky v [Začínáme s vlastními zásadami](active-directory-b2c-get-started-custom.md) článku.</span><span class="sxs-lookup"><span data-stu-id="32d41-139">Complete the steps in the [Getting started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

## <a name="step-1-create-an-aspnet-web-api"></a><span data-ttu-id="32d41-140">Krok 1: Vytvoření rozhraní ASP.NET web API</span><span class="sxs-lookup"><span data-stu-id="32d41-140">Step 1: Create an ASP.NET web API</span></span>

1. <span data-ttu-id="32d41-141">V sadě Visual Studio vytvořte projekt tak, že vyberete **soubor** > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="32d41-141">In Visual Studio, create a project by selecting **File** > **New** > **Project**.</span></span>

2. <span data-ttu-id="32d41-142">V **nový projekt** vyberte **Visual C#** > **webové** > **webové aplikace ASP.NET (rozhraní .NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="32d41-142">In the **New Project** window, select **Visual C#** > **Web** > **ASP.NET Web Application (.NET Framework)**.</span></span>

3. <span data-ttu-id="32d41-143">V **název** zadejte název pro aplikaci (například *Contoso.AADB2C.API*) a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="32d41-143">In the **Name** box, type a name for the application (for example, *Contoso.AADB2C.API*), and then select **OK**.</span></span>

    ![Vytvoření nového projektu sady visual studio](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-create-project.png)

4. <span data-ttu-id="32d41-145">V **nové webové aplikace ASP.NET** vyberte **webového rozhraní API** nebo **aplikace Azure API** šablony.</span><span class="sxs-lookup"><span data-stu-id="32d41-145">In the **New ASP.NET Web Application** window, select a **Web API** or **Azure API app** template.</span></span>

    ![Vyberte šablonu, webové rozhraní API](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-select-web-api.png)

5. <span data-ttu-id="32d41-147">Ujistěte se, že, ověření je nastavena na **bez ověřování**.</span><span class="sxs-lookup"><span data-stu-id="32d41-147">Make sure that authentication is set to **No Authentication**.</span></span>

6. <span data-ttu-id="32d41-148">Vyberte **OK** a vytvořte tak projekt.</span><span class="sxs-lookup"><span data-stu-id="32d41-148">Select **OK** to create the project.</span></span>

## <a name="step-2-prepare-the-rest-api-endpoint"></a><span data-ttu-id="32d41-149">Krok 2: Příprava koncový bod REST API</span><span class="sxs-lookup"><span data-stu-id="32d41-149">Step 2: Prepare the REST API endpoint</span></span>

### <a name="step-21-add-data-models"></a><span data-ttu-id="32d41-150">Krok 2.1: Přidání datové modely</span><span class="sxs-lookup"><span data-stu-id="32d41-150">Step 2.1: Add data models</span></span>
<span data-ttu-id="32d41-151">Modely představují mezi vstupními deklaracemi identity a deklarací výstupní data ve službě RESTful.</span><span class="sxs-lookup"><span data-stu-id="32d41-151">The models represent the input claims and output claims data in your RESTful service.</span></span> <span data-ttu-id="32d41-152">Váš kód čte vstupní data pomocí deserializace model vstupních deklarací identity z řetězce formátu JSON na objekt C# (modelu).</span><span class="sxs-lookup"><span data-stu-id="32d41-152">Your code reads the input data by deserializing the input claims model from a JSON string to a C# object (your model).</span></span> <span data-ttu-id="32d41-153">Rozhraní ASP.NET web API automaticky deserializuje výstupní deklarace identity model zpět do formátu JSON a pak zapíše serializovaná data do textu zprávy s odpovědí HTTP.</span><span class="sxs-lookup"><span data-stu-id="32d41-153">The ASP.NET web API automatically deserializes the output claims model back to JSON and then writes the serialized data to the body of the HTTP response message.</span></span> 

<span data-ttu-id="32d41-154">Vytvoření modelu, který představuje vstupních deklarací identity následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="32d41-154">Create a model that represents input claims by doing the following:</span></span>

1. <span data-ttu-id="32d41-155">Pokud Průzkumníku řešení ještě není otevřený, vyberte **zobrazení** > **Průzkumníku řešení**.</span><span class="sxs-lookup"><span data-stu-id="32d41-155">If Solution Explorer is not already open, select **View** > **Solution Explorer**.</span></span> 
2. <span data-ttu-id="32d41-156">V Průzkumníku řešení klikněte pravým tlačítkem myši **modely** složky, vyberte **přidat**a potom vyberte **třída**.</span><span class="sxs-lookup"><span data-stu-id="32d41-156">In Solution Explorer, right-click the **Models** folder, select **Add**, and then select **Class**.</span></span>

    ![Přidání modelu](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-add-model.png)

3. <span data-ttu-id="32d41-158">Název třídy `InputClaimsModel`a poté přidejte následující vlastnosti pro `InputClaimsModel` třídy:</span><span class="sxs-lookup"><span data-stu-id="32d41-158">Name the class `InputClaimsModel`, and then add the following properties to the `InputClaimsModel` class:</span></span>

    ```csharp
    namespace Contoso.AADB2C.API.Models
    {
        public class InputClaimsModel
        {
            public string email { get; set; }
            public string firstName { get; set; }
            public string lastName { get; set; }
        }
    }
    ```

4. <span data-ttu-id="32d41-159">Vytvořit nový model, `OutputClaimsModel`a poté přidejte následující vlastnosti pro `OutputClaimsModel` třídy:</span><span class="sxs-lookup"><span data-stu-id="32d41-159">Create a new model, `OutputClaimsModel`, and then add the following properties to the `OutputClaimsModel` class:</span></span>

    ```csharp
    namespace Contoso.AADB2C.API.Models
    {
        public class OutputClaimsModel
        {
            public string loyaltyNumber { get; set; }
        }
    }
    ```

5. <span data-ttu-id="32d41-160">Vytvořte jeden další model `B2CResponseContent`, který použijete k throw ověření vstupu chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="32d41-160">Create one more model, `B2CResponseContent`, which you use to throw input-validation error messages.</span></span> <span data-ttu-id="32d41-161">Přidejte následující vlastnosti pro `B2CResponseContent` třídy zadejte odkazy na chybějící a pak soubor uložte:</span><span class="sxs-lookup"><span data-stu-id="32d41-161">Add the following properties to the `B2CResponseContent` class, provide the missing references, and then save the file:</span></span>

    ```csharp
    namespace Contoso.AADB2C.API.Models
    {
        public class B2CResponseContent
        {
            public string version { get; set; }
            public int status { get; set; }
            public string userMessage { get; set; }

            public B2CResponseContent(string message, HttpStatusCode status)
            {
                this.userMessage = message;
                this.status = (int)status;
                this.version = Assembly.GetExecutingAssembly().GetName().Version.ToString();
            }    
        }
    }
    ```

### <a name="step-22-add-a-controller"></a><span data-ttu-id="32d41-162">Krok 2.2: Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="32d41-162">Step 2.2: Add a controller</span></span>
<span data-ttu-id="32d41-163">Ve webové rozhraní API _řadič_ je objekt, který zpracovává požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="32d41-163">In the web API, a _controller_ is an object that handles HTTP requests.</span></span> <span data-ttu-id="32d41-164">Řadičem vrátí výstup deklarací, nebo pokud křestní jméno není platné, vyvolá chybová zpráva o konfliktu HTTP.</span><span class="sxs-lookup"><span data-stu-id="32d41-164">The controller returns output claims or, if the first name is not valid, throws a Conflict HTTP error message.</span></span>

1. <span data-ttu-id="32d41-165">V Průzkumníku řešení klikněte pravým tlačítkem myši **řadiče** složky, vyberte **přidat**a potom vyberte **řadič**.</span><span class="sxs-lookup"><span data-stu-id="32d41-165">In Solution Explorer, right-click the **Controllers** folder, select **Add**, and then select **Controller**.</span></span>

    ![Přidat nový řadič](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-add-controller-1.png)

2. <span data-ttu-id="32d41-167">V **přidat vygenerované uživatelské rozhraní** vyberte **kontroler Web API – prázdný**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="32d41-167">In the **Add Scaffold** window, select **Web API Controller - Empty**, and then select **Add**.</span></span>

    ![Prázdný kontroler – vyberte webového rozhraní API 2](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-add-controller-2.png)

3. <span data-ttu-id="32d41-169">V **přidat kontroler** období, název kontroleru **IdentityController**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="32d41-169">In the **Add Controller** window, name the controller **IdentityController**, and then select **Add**.</span></span>

    ![Zadejte název řadiče](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-add-controller-3.png)

    <span data-ttu-id="32d41-171">Generování uživatelského rozhraní vytvoří soubor s názvem *IdentityController.cs* v *řadiče* složky.</span><span class="sxs-lookup"><span data-stu-id="32d41-171">The scaffolding creates a file named *IdentityController.cs* in the *Controllers* folder.</span></span>

4. <span data-ttu-id="32d41-172">Pokud *IdentityController.cs* soubor již není otevřený, dvakrát na ni klikněte a pak nahraďte kód v souboru s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="32d41-172">If the *IdentityController.cs* file is not open already, double-click it, and then replace the code in the file with the following code:</span></span>

    ```csharp
    using Contoso.AADB2C.API.Models;
    using Newtonsoft.Json;
    using System;
    using System.NET;
    using System.Web.Http;

    namespace Contoso.AADB2C.API.Controllers
    {
        public class IdentityController: ApiController
        {
            [HttpPost]
            public IHttpActionResult SignUp()
            {
                // If no data came in, then return
                if (this.Request.Content == null) throw new Exception();

                // Read the input claims from the request body
                string input = Request.Content.ReadAsStringAsync().Result;

                // Check the input content value
                if (string.IsNullOrEmpty(input))
                {
                    return Content(HttpStatusCode.Conflict, new B2CResponseContent("Request content is empty", HttpStatusCode.Conflict));
                }

                // Convert the input string into an InputClaimsModel object
                InputClaimsModel inputClaims = JsonConvert.DeserializeObject(input, typeof(InputClaimsModel)) as InputClaimsModel;

                if (inputClaims == null)
                {
                    return Content(HttpStatusCode.Conflict, new B2CResponseContent("Can not deserialize input claims", HttpStatusCode.Conflict));
                }

                // Run an input validation
                if (inputClaims.firstName.ToLower() == "test")
                {
                    return Content(HttpStatusCode.Conflict, new B2CResponseContent("Test name is not valid, please provide a valid name", HttpStatusCode.Conflict));
                }

                // Create an output claims object and set the loyalty number with a random value
                OutputClaimsModel outputClaims = new OutputClaimsModel();
                outputClaims.loyaltyNumber = new Random().Next(100, 1000).ToString();

                // Return the output claim(s)
                return Ok(outputClaims);
            }
        }
    }
    ```

## <a name="step-3-publish-the-project-to-azure"></a><span data-ttu-id="32d41-173">Krok 3: Publikování projektu v Azure</span><span class="sxs-lookup"><span data-stu-id="32d41-173">Step 3: Publish the project to Azure</span></span>
1. <span data-ttu-id="32d41-174">V Průzkumníku řešení klikněte pravým tlačítkem myši **Contoso.AADB2C.API** projektu a potom vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="32d41-174">In Solution Explorer, right-click the **Contoso.AADB2C.API** project, and then select **Publish**.</span></span>

    ![Publikování do služby Microsoft Azure App Service](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-publish-to-azure-1.png)

2. <span data-ttu-id="32d41-176">V **publikovat** vyberte **Microsoft Azure App Service**a potom vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="32d41-176">In the **Publish** window, select **Microsoft Azure App Service**, and then select **Publish**.</span></span>

    ![Vytvořit nový Microsoft Azure App Service](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-publish-to-azure-2.png)

    <span data-ttu-id="32d41-178">**Vytvořit službu App Service** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="32d41-178">The **Create App Service** window opens.</span></span> <span data-ttu-id="32d41-179">V něm vytvořit všechny potřebné prostředky Azure ke spouštění webové aplikace ASP.NET v Azure.</span><span class="sxs-lookup"><span data-stu-id="32d41-179">In it, you create all the necessary Azure resources to run the ASP.NET web app in Azure.</span></span>

    > [!NOTE]
    ><span data-ttu-id="32d41-180">Další informace o tom, jak publikovat najdete v tématu [vytvoření webové aplikace ASP.NET v Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet#publish-to-azure).</span><span class="sxs-lookup"><span data-stu-id="32d41-180">For more information about how to publish, see [Create an ASP.NET web app in Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet#publish-to-azure).</span></span>

3. <span data-ttu-id="32d41-181">V **název webové aplikace** zadejte jedinečným názvem aplikace (jsou platné znaky-z, 0 – 9 a pomlčky (-).</span><span class="sxs-lookup"><span data-stu-id="32d41-181">In the **Web App Name** box, type a unique app name (valid characters are a-z, 0-9, and hyphens (-).</span></span> <span data-ttu-id="32d41-182">Adresa URL webové aplikace je http://<app_name>.azurewebsites.NET, kde *app_name* je název vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="32d41-182">The URL of the web app is http://<app_name>.azurewebsites.NET, where *app_name* is the name of your web app.</span></span> <span data-ttu-id="32d41-183">Můžete přijmout automaticky vygenerovaný název, který je jedinečný.</span><span class="sxs-lookup"><span data-stu-id="32d41-183">You can accept the automatically generated name, which is unique.</span></span>

    ![Zadejte vlastnosti služby App Service](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-publish-to-azure-3.png)

4. <span data-ttu-id="32d41-185">Chcete-li začít vytvářet prostředky Azure, vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="32d41-185">To start creating Azure resources, select **Create**.</span></span>  
    <span data-ttu-id="32d41-186">Po vytvoření webové aplikace ASP.NET, Průvodce publikuje do Azure a pak spustí aplikaci ve výchozím prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="32d41-186">After the ASP.NET web app has been created, the wizard publishes it to Azure and then starts the app in the default browser.</span></span>

6. <span data-ttu-id="32d41-187">Zkopírujte adresu URL webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="32d41-187">Copy the web app's URL.</span></span>

## <a name="step-4-add-the-new-loyaltynumber-claim-to-the-schema-of-your-trustframeworkextensionsxml-file"></a><span data-ttu-id="32d41-188">Krok 4: Přidejte nové `loyaltyNumber` deklarace schématu TrustFrameworkExtensions.xml souboru</span><span class="sxs-lookup"><span data-stu-id="32d41-188">Step 4: Add the new `loyaltyNumber` claim to the schema of your TrustFrameworkExtensions.xml file</span></span>
<span data-ttu-id="32d41-189">`loyaltyNumber` Deklarace identity ještě není definován v našem schématu.</span><span class="sxs-lookup"><span data-stu-id="32d41-189">The `loyaltyNumber` claim is not yet defined in our schema.</span></span> <span data-ttu-id="32d41-190">Přidejte definici v rámci `<BuildingBlocks>` element, který můžete najít na začátku *TrustFrameworkExtensions.xml* souboru.</span><span class="sxs-lookup"><span data-stu-id="32d41-190">Add a definition within the `<BuildingBlocks>` element, which you can find at the beginning of the *TrustFrameworkExtensions.xml* file.</span></span>

```xml
<BuildingBlocks>
    <ClaimsSchema>
        <ClaimType Id="loyaltyNumber">
            <DisplayName>loyaltyNumber</DisplayName>
            <DataType>string</DataType>
            <UserHelpText>Customer loyalty number</UserHelpText>
        </ClaimType>
    </ClaimsSchema>
</BuildingBlocks>
```

## <a name="step-5-add-a-claims-provider"></a><span data-ttu-id="32d41-191">Krok 5: Přidejte zprostředkovatele deklarací identity</span><span class="sxs-lookup"><span data-stu-id="32d41-191">Step 5: Add a claims provider</span></span> 
<span data-ttu-id="32d41-192">Každý poskytovatel deklarací identity musí mít jeden nebo více technické profily, které určují koncových bodů a protokoly nutné pro komunikaci se zprostředkovateli deklarací.</span><span class="sxs-lookup"><span data-stu-id="32d41-192">Every claims provider must have one or more technical profiles, which determine the endpoints and protocols needed to communicate with the claims provider.</span></span> 

<span data-ttu-id="32d41-193">Poskytovatele deklarací identity může mít několik technické profilů z různých důvodů.</span><span class="sxs-lookup"><span data-stu-id="32d41-193">A claims provider can have multiple technical profiles for various reasons.</span></span> <span data-ttu-id="32d41-194">Víc technických profilů například může definovat, protože zprostředkovatele deklarací identity podporuje více protokolů, koncových bodů může mít různou možnosti nebo verze můžete obsahují deklarace identity, které mají různé úrovně záruky.</span><span class="sxs-lookup"><span data-stu-id="32d41-194">For example, multiple technical profiles might be defined because the claims provider supports multiple protocols, endpoints can have varying capabilities, or releases can contain claims that have a variety of assurance levels.</span></span> <span data-ttu-id="32d41-195">Může to být přijatelné k uvolnění citlivé deklarací cesty jednoho uživatele, ale ne v jiném.</span><span class="sxs-lookup"><span data-stu-id="32d41-195">It might be acceptable to release sensitive claims in one user journey but not in another.</span></span> 

<span data-ttu-id="32d41-196">Následující fragment kódu XML obsahuje uzel zprostředkovatele deklarací se dva technické profily:</span><span class="sxs-lookup"><span data-stu-id="32d41-196">The following XML snippet contains a claims provider node with two technical profiles:</span></span>

* <span data-ttu-id="32d41-197">**TechnicalProfile Id = "REST API SignUp"**: definuje služby RESTful.</span><span class="sxs-lookup"><span data-stu-id="32d41-197">**TechnicalProfile Id="REST-API-SignUp"**: Defines your RESTful service.</span></span> 
   * <span data-ttu-id="32d41-198">`Proprietary` je označen jako protokol pro zprostředkovatele na základě dosáhl standardu RESTful.</span><span class="sxs-lookup"><span data-stu-id="32d41-198">`Proprietary` is described as the protocol for a RESTful-based provider.</span></span> 
   * <span data-ttu-id="32d41-199">`InputClaims` definuje deklarace identity, které budou odesílané z Azure AD B2C ke službě REST.</span><span class="sxs-lookup"><span data-stu-id="32d41-199">`InputClaims` defines the claims that will be sent from Azure AD B2C to the REST service.</span></span> 

   <span data-ttu-id="32d41-200">V tomto příkladu obsah deklarace `givenName` se odešle do služby REST jako `firstName`, obsah deklarace `surname` se odešle do služby REST jako `lastName`, a `email` odešle jako je.</span><span class="sxs-lookup"><span data-stu-id="32d41-200">In this example, the content of the claim `givenName` sends to the REST service as `firstName`, the content of the claim `surname` sends to the REST service as `lastName`, and `email` sends as is.</span></span> <span data-ttu-id="32d41-201">`OutputClaims` Element definuje deklarace identity, které jsou načteny z služba RESTful zpět do Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="32d41-201">The `OutputClaims` element defines the claims that are retrieved from RESTful service back to Azure AD B2C.</span></span>

* <span data-ttu-id="32d41-202">**TechnicalProfile Id = "LocalAccountSignUpWithLogonEmail"**: technické profil ověření přidá do stávající profil technické (definovanou v základní zásady).</span><span class="sxs-lookup"><span data-stu-id="32d41-202">**TechnicalProfile Id="LocalAccountSignUpWithLogonEmail"**: Adds a validation technical profile to an existing technical profile (defined in base policy).</span></span> <span data-ttu-id="32d41-203">Během registrace cesty vyvolá technické profil ověření předchozí technické profilu.</span><span class="sxs-lookup"><span data-stu-id="32d41-203">During the sign-up journey, the validation technical profile invokes the preceding technical profile.</span></span> <span data-ttu-id="32d41-204">Pokud službu RESTful vrátí chybu HTTP 409 (Chyba konflikt), zobrazí se chybová zpráva pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="32d41-204">If the RESTful service returns an HTTP error 409 (a conflict error), the error message is displayed to the user.</span></span> 

<span data-ttu-id="32d41-205">Vyhledejte `<ClaimsProviders>` uzel a poté přidejte následující fragment kódu XML v části `<ClaimsProviders>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="32d41-205">Locate the `<ClaimsProviders>` node, and then add the following XML snippet under the `<ClaimsProviders>` node:</span></span>

```xml
<ClaimsProvider>
    <DisplayName>REST APIs</DisplayName>
    <TechnicalProfiles>
    
    <!-- Custom Restful service -->
    <TechnicalProfile Id="REST-API-SignUp">
        <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
        <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
        <Metadata>
        <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
        <Item Key="AuthenticationType">None</Item>
        <Item Key="SendClaimsIn">Body</Item>
        </Metadata>
        <InputClaims>
        <InputClaim ClaimTypeReferenceId="email" />
        <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName" />
        <InputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName" />
        </InputClaims>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="loyaltyNumber" PartnerClaimType="loyaltyNumber" />
        </OutputClaims>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
    </TechnicalProfile>

<!-- Change LocalAccountSignUpWithLogonEmail technical profile to support your validation technical profile -->
    <TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="loyaltyNumber" PartnerClaimType="loyaltyNumber" />
        </OutputClaims>
        <ValidationTechnicalProfiles>
        <ValidationTechnicalProfile ReferenceId="REST-API-SignUp" />
        </ValidationTechnicalProfiles>
    </TechnicalProfile>

    </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="step-6-add-the-loyaltynumber-claim-to-your-relying-party-policy-file-so-the-claim-is-sent-to-your-application"></a><span data-ttu-id="32d41-206">Krok 6: Přidejte `loyaltyNumber` deklarace identity do souboru zásad předávající strany, deklarace identity je odeslána do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="32d41-206">Step 6: Add the `loyaltyNumber` claim to your relying party policy file so the claim is sent to your application</span></span>
<span data-ttu-id="32d41-207">Upravit vaše *SignUpOrSignIn.xml* předávající stranu soubor a upravte TechnicalProfile Id = "PolicyProfile" elementu, který chcete přidat následující: `<OutputClaim ClaimTypeReferenceId="loyaltyNumber" />`.</span><span class="sxs-lookup"><span data-stu-id="32d41-207">Edit your *SignUpOrSignIn.xml* relying party (RP) file, and modify the TechnicalProfile Id="PolicyProfile" element to add the following: `<OutputClaim ClaimTypeReferenceId="loyaltyNumber" />`.</span></span>

<span data-ttu-id="32d41-208">Po přidání nových deklarací identity, předávající strany kódu vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="32d41-208">After you add the new claim, the relying party code looks like this:</span></span>

```xml
<RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <TechnicalProfile Id="PolicyProfile">
        <DisplayName>PolicyProfile</DisplayName>
        <Protocol Name="OpenIdConnect" />
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="email" />
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
        <OutputClaim ClaimTypeReferenceId="identityProvider" />
        <OutputClaim ClaimTypeReferenceId="loyaltyNumber" DefaultValue="" />
        </OutputClaims>
        <SubjectNamingInfo ClaimType="sub" />
    </TechnicalProfile>
    </RelyingParty>
</TrustFrameworkPolicy>
```

## <a name="step-7-upload-the-policy-to-your-tenant"></a><span data-ttu-id="32d41-209">Krok 7: Nahrajte zásady klienta</span><span class="sxs-lookup"><span data-stu-id="32d41-209">Step 7: Upload the policy to your tenant</span></span>

1. <span data-ttu-id="32d41-210">V [portál Azure](https://portal.azure.com), přepnout [kontextu klienta služby Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)a pak otevřete **Azure AD B2C**.</span><span class="sxs-lookup"><span data-stu-id="32d41-210">In the [Azure portal](https://portal.azure.com), switch to the [context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and then open **Azure AD B2C**.</span></span>

2. <span data-ttu-id="32d41-211">Vyberte **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="32d41-211">Select **Identity Experience Framework**.</span></span>

3. <span data-ttu-id="32d41-212">Otevřete **všechny zásady**.</span><span class="sxs-lookup"><span data-stu-id="32d41-212">Open **All Policies**.</span></span> 

4. <span data-ttu-id="32d41-213">Vyberte **nahrát zásady**.</span><span class="sxs-lookup"><span data-stu-id="32d41-213">Select **Upload Policy**.</span></span>

5. <span data-ttu-id="32d41-214">Vyberte **přepsat zásady, pokud existuje** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="32d41-214">Select the **Overwrite the policy if it exists** check box.</span></span>

6. <span data-ttu-id="32d41-215">Nahrajte soubor TrustFrameworkExtensions.xml a ujistěte se, že předává ověření.</span><span class="sxs-lookup"><span data-stu-id="32d41-215">Upload the TrustFrameworkExtensions.xml file, and ensure that it passes validation.</span></span>

7. <span data-ttu-id="32d41-216">Opakujte předchozí krok souborem SignUpOrSignIn.xml.</span><span class="sxs-lookup"><span data-stu-id="32d41-216">Repeat the preceding step with the SignUpOrSignIn.xml file.</span></span>

## <a name="step-8-test-the-custom-policy-by-using-run-now"></a><span data-ttu-id="32d41-217">Krok 8: Testování spustit pomocí vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="32d41-217">Step 8: Test the custom policy by using Run Now</span></span>
1. <span data-ttu-id="32d41-218">Vyberte **nastavení Azure AD B2C**a pak přejděte na **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="32d41-218">Select **Azure AD B2C Settings**, and then go to **Identity Experience Framework**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="32d41-219">**Spustit nyní** vyžaduje alespoň jedné aplikace do být preregistered u klienta.</span><span class="sxs-lookup"><span data-stu-id="32d41-219">**Run now** requires at least one application to be preregistered on the tenant.</span></span> <span data-ttu-id="32d41-220">Další postup registrace aplikace najdete v tématu Azure AD B2C [Začínáme](active-directory-b2c-get-started.md) článek nebo [registrace aplikace](active-directory-b2c-app-registration.md) článku.</span><span class="sxs-lookup"><span data-stu-id="32d41-220">To learn how to register applications, see the Azure AD B2C [Get started](active-directory-b2c-get-started.md) article or the [Application registration](active-directory-b2c-app-registration.md) article.</span></span>

2. <span data-ttu-id="32d41-221">Otevřete **B2C_1A_signup_signin**, předávající stranu vlastních zásad, které jste nahráli a potom vyberte **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="32d41-221">Open **B2C_1A_signup_signin**, the relying party (RP) custom policy that you uploaded, and then select **Run now**.</span></span>

    ![Okno B2C_1A_signup_signin](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-run.png)

3. <span data-ttu-id="32d41-223">Otestujte proces zadáním **Test** v **křestní jméno** pole.</span><span class="sxs-lookup"><span data-stu-id="32d41-223">Test the process by typing **Test** in the **Given Name** box.</span></span>  
    <span data-ttu-id="32d41-224">Azure AD B2C zobrazí chybovou zprávu v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="32d41-224">Azure AD B2C displays an error message at the top of the window.</span></span>

    ![Vaše zásada testování](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-test.png)

4.  <span data-ttu-id="32d41-226">V **křestní jméno** zadejte název (jiné než "Test").</span><span class="sxs-lookup"><span data-stu-id="32d41-226">In the **Given Name** box, type a name (other than "Test").</span></span>  
    <span data-ttu-id="32d41-227">Azure AD B2C zaregistruje uživatele a pak odešle loyaltyNumber do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="32d41-227">Azure AD B2C signs up the user and then sends a loyaltyNumber to your application.</span></span> <span data-ttu-id="32d41-228">Poznamenejte si číslo v této JWT.</span><span class="sxs-lookup"><span data-stu-id="32d41-228">Note the number in this JWT.</span></span>

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

## <a name="optional-download-the-complete-policy-files-and-code"></a><span data-ttu-id="32d41-229">(Volitelné) Stáhnout soubory dokončení zásad a kódu</span><span class="sxs-lookup"><span data-stu-id="32d41-229">(Optional) Download the complete policy files and code</span></span>
* <span data-ttu-id="32d41-230">Po dokončení [začít pracovat s vlastními zásadami](active-directory-b2c-get-started-custom.md) návod, doporučujeme vám vytvořit váš scénář pomocí vlastních zásad pro soubory.</span><span class="sxs-lookup"><span data-stu-id="32d41-230">After you complete the [Get started with custom policies](active-directory-b2c-get-started-custom.md) walkthrough, we recommend that you build your scenario by using your own custom policy files.</span></span> <span data-ttu-id="32d41-231">Pro vaši informaci uvádíme [ukázkové soubory zásad](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw).</span><span class="sxs-lookup"><span data-stu-id="32d41-231">For your reference, we have provided [Sample policy files](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw).</span></span>
* <span data-ttu-id="32d41-232">Si můžete stáhnout kompletní kód z [řešení sady Visual Studio ukázkový pro referenci](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw/).</span><span class="sxs-lookup"><span data-stu-id="32d41-232">You can download the complete code from [Sample Visual Studio solution for reference](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw/).</span></span>
    
## <a name="next-steps"></a><span data-ttu-id="32d41-233">Další postup</span><span class="sxs-lookup"><span data-stu-id="32d41-233">Next steps</span></span>
* [<span data-ttu-id="32d41-234">Zabezpečení rozhraní RESTful API pomocí základní ověřování (uživatelské jméno a heslo)</span><span class="sxs-lookup"><span data-stu-id="32d41-234">Secure your RESTful API with basic authentication (username and password)</span></span>](active-directory-b2c-custom-rest-api-netfw-secure-basic.md)
* [<span data-ttu-id="32d41-235">Zabezpečení rozhraní RESTful API pomocí klientských certifikátů</span><span class="sxs-lookup"><span data-stu-id="32d41-235">Secure your RESTful API with client certificates</span></span>](active-directory-b2c-custom-rest-api-netfw-secure-cert.md)
