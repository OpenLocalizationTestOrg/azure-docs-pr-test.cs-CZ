---
title: "aaaProtect back-endu webového rozhraní API pomocí Azure Active Directory a API Management | Microsoft Docs"
description: "Zjistěte, jak tooprotect back-endu webového rozhraní API pomocí Azure Active Directory a API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: f856ff03-64a1-4548-9ec4-c0ec4cc1600f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: f4b323034354aa09579c643bade47257fbf1e5c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooprotect-a-web-api-backend-with-azure-active-directory-and-api-management"></a><span data-ttu-id="c702f-103">Jak tooprotect back-endu webového rozhraní API pomocí Azure Active Directory a API Management</span><span class="sxs-lookup"><span data-stu-id="c702f-103">How tooprotect a Web API backend with Azure Active Directory and API Management</span></span>
<span data-ttu-id="c702f-104">následující video ukazuje, jak Hello toobuild back-end webového rozhraní API a chránit pomocí Azure Active Directory a rozhraní API správy protokolu OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="c702f-104">hello following video shows how toobuild a Web API backend and protect it using OAuth 2.0 protocol with Azure Active Directory and API Management.</span></span>  <span data-ttu-id="c702f-105">Tento článek obsahuje přehled a další informace o hello kroky hello video.</span><span class="sxs-lookup"><span data-stu-id="c702f-105">This article provides an overview and additional information for hello steps in hello video.</span></span> <span data-ttu-id="c702f-106">Následující 24 minutu video ukazuje, jak na:</span><span class="sxs-lookup"><span data-stu-id="c702f-106">This 24 minute video shows you how to:</span></span>

* <span data-ttu-id="c702f-107">Sestavení webového rozhraní API back-end a zabezpečte ji pomocí AAD - počínaje 1:30</span><span class="sxs-lookup"><span data-stu-id="c702f-107">Build a Web API backend and secure it with AAD - starting at 1:30</span></span>
* <span data-ttu-id="c702f-108">Importovat rozhraní API hello do rozhraní API Management – počínaje 7:10</span><span class="sxs-lookup"><span data-stu-id="c702f-108">Import hello API into API Management - starting at 7:10</span></span>
* <span data-ttu-id="c702f-109">Konfigurace hello vývojáře portálu toocall hello API – počínaje 9:09</span><span class="sxs-lookup"><span data-stu-id="c702f-109">Configure hello Developer portal toocall hello API - starting at 9:09</span></span>
* <span data-ttu-id="c702f-110">Konfigurace desktopová aplikace toocall hello API – od 18:08</span><span class="sxs-lookup"><span data-stu-id="c702f-110">Configure a desktop application toocall hello API - starting at 18:08</span></span>
* <span data-ttu-id="c702f-111">Konfigurace JWT ověření zásad toopre-autorizaci požadavků - začínající na 20:47</span><span class="sxs-lookup"><span data-stu-id="c702f-111">Configure a JWT validation policy toopre-authorize requests - starting at 20:47</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Protecting-Web-API-Backend-with-Azure-Active-Directory-and-API-Management/player]
> 
> 

## <a name="create-an-azure-ad-directory"></a><span data-ttu-id="c702f-112">Vytvořte adresář služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c702f-112">Create an Azure AD directory</span></span>
<span data-ttu-id="c702f-113">toosecure zálohovaný vašeho webového rozhraní API pomocí Azure Active Directory je nutné nejdříve vytvořit klienta služby AAD.</span><span class="sxs-lookup"><span data-stu-id="c702f-113">toosecure your Web API backed using Azure Active Directory you must first have a an AAD tenant.</span></span> <span data-ttu-id="c702f-114">V tomto videu klienta s názvem **APIMDemo** se používá.</span><span class="sxs-lookup"><span data-stu-id="c702f-114">In this video a tenant named **APIMDemo** is used.</span></span> <span data-ttu-id="c702f-115">toocreate klienta služby AAD přihlášení toohello [portálu Azure Classic](https://manage.windowsazure.com) a klikněte na tlačítko **nový**->**App Services**->**Active Adresář**->**Directory**->**vytvořit vlastní**.</span><span class="sxs-lookup"><span data-stu-id="c702f-115">toocreate an AAD tenant, sign-in toohello [Azure Classic Portal](https://manage.windowsazure.com) and click **New**->**App Services**->**Active Directory**->**Directory**->**Custom Create**.</span></span> 

![Azure Active Directory][api-management-create-aad-menu]

<span data-ttu-id="c702f-117">V tomto příkladu adresář s názvem **APIMDemo** je vytvořena s výchozí doménu s názvem **DemoAPIM.onmicrosoft.com**. Tento adresář se používá napříč hello video.</span><span class="sxs-lookup"><span data-stu-id="c702f-117">In this example a directory named **APIMDemo** is created with a default domain named **DemoAPIM.onmicrosoft.com**. This directory is used throughout hello video.</span></span>

![Azure Active Directory][api-management-create-aad]

## <a name="create-a-web-api-service-secured-by-azure-active-directory"></a><span data-ttu-id="c702f-119">Vytvoření webového rozhraní API služby Zabezpečené přes Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c702f-119">Create a Web API service secured by Azure Active Directory</span></span>
<span data-ttu-id="c702f-120">V tomto kroku se vytvoří webového rozhraní API back-end pomocí Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="c702f-120">In this step, a Web API backend is created using Visual Studio 2013.</span></span> <span data-ttu-id="c702f-121">Tento krok hello video se spustí v 1:30.</span><span class="sxs-lookup"><span data-stu-id="c702f-121">This step of hello video starts at 1:30.</span></span> <span data-ttu-id="c702f-122">toocreate webového rozhraní API back-end projektu v sadě Visual Studio klikněte na položku **soubor**->**nový**->**projektu**a zvolte **technologie ASP.NET Aplikace** z hello **webové** seznamu šablon.</span><span class="sxs-lookup"><span data-stu-id="c702f-122">toocreate Web API backend project in Visual Studio click **File**->**New**->**Project**, and choose **ASP.NET Web Application** from hello **Web** templates list.</span></span> <span data-ttu-id="c702f-123">V této video hello projektu jmenuje **APIMAADDemo**.</span><span class="sxs-lookup"><span data-stu-id="c702f-123">In this video hello project is named **APIMAADDemo**.</span></span> <span data-ttu-id="c702f-124">Klikněte na tlačítko **OK** toocreate hello projektu.</span><span class="sxs-lookup"><span data-stu-id="c702f-124">Click **OK** toocreate hello project.</span></span> 

![Visual Studio][api-management-new-web-app]

<span data-ttu-id="c702f-126">Klikněte na tlačítko **webového rozhraní API** z hello **vyberte seznam šablony** toocreate projekt webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c702f-126">Click **Web API** from hello **Select a template list** toocreate a Web API project.</span></span> <span data-ttu-id="c702f-127">Klikněte na tlačítko tooconfigure ověřování Azure Directory **změna ověřování**.</span><span class="sxs-lookup"><span data-stu-id="c702f-127">tooconfigure Azure Directory Authentication click **Change Authentication**.</span></span>

![Nový projekt][api-management-new-project]

<span data-ttu-id="c702f-129">Klikněte na tlačítko **účty organizace**a zadejte hello **domény** klienta služby AAD.</span><span class="sxs-lookup"><span data-stu-id="c702f-129">Click **Organizational Accounts**, and specify hello **Domain** of your AAD tenant.</span></span> <span data-ttu-id="c702f-130">Tento příklad hello domény je **DemoAPIM.onmicrosoft.com**. hello domény adresáře můžete získat hello **domén** svůj adresář.</span><span class="sxs-lookup"><span data-stu-id="c702f-130">In this example hello domain is **DemoAPIM.onmicrosoft.com**. hello domain of your directory can be obtained from hello **Domains** tab of your directory.</span></span>

![Domény][api-management-aad-domains]

<span data-ttu-id="c702f-132">Konfigurace nastavení hello potřeby v hello **změna ověřování** dialogové okno a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c702f-132">Configure hello desired settings in hello **Change Authentication** dialog box and click **OK**.</span></span>

![Změna ověřování][api-management-change-authentication]

<span data-ttu-id="c702f-134">Když kliknete na tlačítko **OK** Visual Studio se pokusí tooregister vaší aplikace pomocí svého adresáře Azure AD a může být výzvami toosign ve Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c702f-134">When you click **OK** Visual Studio will attempt tooregister your application with your Azure AD directory and you may be prompted toosign in by Visual Studio.</span></span> <span data-ttu-id="c702f-135">Přihlaste se pomocí účtu správce pro váš adresář.</span><span class="sxs-lookup"><span data-stu-id="c702f-135">Sign in using an administrative account for your directory.</span></span>

![Přihlaste se tooVisual Studio][api-management-sign-in-vidual-studio]

<span data-ttu-id="c702f-137">tooconfigure tohoto projektu jako hello webového rozhraní API Azure zkontrolujte pole pro **hostitel v cloudu hello** a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c702f-137">tooconfigure this project as an Azure Web API check hello box for **Host in hello cloud** and then click **OK**.</span></span>

![Nový projekt][api-management-new-project-cloud]

<span data-ttu-id="c702f-139">Může být výzvami toosign v tooAzure, a pak můžete nakonfigurovat hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c702f-139">You may be prompted toosign in tooAzure, and then you can configure hello Web App.</span></span>

![Konfigurace][api-management-configure-web-app]

<span data-ttu-id="c702f-141">V tomto příkladu a nové **plán služby App Service** s názvem **APIMAADDemo** je zadán.</span><span class="sxs-lookup"><span data-stu-id="c702f-141">In this example a new **App Service plan** named **APIMAADDemo** is specified.</span></span>

<span data-ttu-id="c702f-142">Klikněte na tlačítko **OK** tooconfigure hello webové aplikace a vytvoření projektu hello.</span><span class="sxs-lookup"><span data-stu-id="c702f-142">Click **OK** tooconfigure hello Web App and create hello project.</span></span>

## <a name="add-hello-code-toohello-web-api-project"></a><span data-ttu-id="c702f-143">Přidat projekt webového rozhraní API toohello hello kódu</span><span class="sxs-lookup"><span data-stu-id="c702f-143">Add hello code toohello Web API project</span></span>
<span data-ttu-id="c702f-144">Hello další krok v hello video přidá projekt webového rozhraní API toohello hello kódu.</span><span class="sxs-lookup"><span data-stu-id="c702f-144">hello next step in hello video adds hello code toohello Web API project.</span></span> <span data-ttu-id="c702f-145">Tento krok se spustí při 4:35.</span><span class="sxs-lookup"><span data-stu-id="c702f-145">This step starts at 4:35.</span></span>

<span data-ttu-id="c702f-146">Hello webového rozhraní API v tomto příkladu implementuje základní kalkulačky služby pomocí modelu a kontroleru.</span><span class="sxs-lookup"><span data-stu-id="c702f-146">hello Web API in this example implements a basic calculator service using a model and a controller.</span></span> <span data-ttu-id="c702f-147">Klikněte pravým tlačítkem na model hello tooadd pro službu hello, **modely** v **Průzkumníku řešení** a zvolte **přidat**, **třída**.</span><span class="sxs-lookup"><span data-stu-id="c702f-147">tooadd hello model for hello service, right-click **Models** in **Solution Explorer** and choose **Add**, **Class**.</span></span> <span data-ttu-id="c702f-148">Název třídy hello `CalcInput` a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c702f-148">Name hello class `CalcInput` and click **Add**.</span></span>

<span data-ttu-id="c702f-149">Přidejte následující hello `using` příkaz toohello začátek hello `CalcInput.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="c702f-149">Add hello following `using` statement toohello top of hello `CalcInput.cs` file.</span></span>

```c#
using Newtonsoft.Json;
```

<span data-ttu-id="c702f-150">Nahraďte hello generované třídy hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="c702f-150">Replace hello generated class with hello following code.</span></span>

```c#
public class CalcInput
{
    [JsonProperty(PropertyName = "a")]
    public int a;

    [JsonProperty(PropertyName = "b")]
    public int b;
}
```

<span data-ttu-id="c702f-151">Klikněte pravým tlačítkem na **řadiče** v **Průzkumníku řešení** a zvolte **přidat**->**řadič**.</span><span class="sxs-lookup"><span data-stu-id="c702f-151">Right-click **Controllers** in **Solution Explorer** and choose **Add**->**Controller**.</span></span> <span data-ttu-id="c702f-152">Zvolte **webové rozhraní API 2 řadiče - prázdný** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c702f-152">Choose **Web API 2 Controller - Empty** and click **Add**.</span></span> <span data-ttu-id="c702f-153">Typ **CalcController** hello řadiče název a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c702f-153">Type **CalcController** for hello Controller name and click **Add**.</span></span>

![Přidání Kontroleru][api-management-add-controller]

<span data-ttu-id="c702f-155">Přidejte následující hello `using` příkaz toohello začátek hello `CalcController.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="c702f-155">Add hello following `using` statement toohello top of hello `CalcController.cs` file.</span></span>

```c#
using System.IO;
using System.Web;
using APIMAADDemo.Models;
```

<span data-ttu-id="c702f-156">Třída controller hello generované nahraďte hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="c702f-156">Replace hello generated controller class with hello following code.</span></span> <span data-ttu-id="c702f-157">Tento kód implementuje hello `Add`, `Subtract`, `Multiply`, a `Divide` operace hello rozhraní API základní kalkulačky.</span><span class="sxs-lookup"><span data-stu-id="c702f-157">This code implements hello `Add`, `Subtract`, `Multiply`, and `Divide` operations of hello Basic Calculator API.</span></span>

```c#
[Authorize]
public class CalcController : ApiController
{
    [Route("api/add")]
    [HttpGet]
    public HttpResponseMessage GetSum([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a + b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/sub")]
    [HttpGet]
    public HttpResponseMessage GetDiff([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a - b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/mul")]
    [HttpGet]
    public HttpResponseMessage GetProduct([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a * b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/div")]
    [HttpGet]
    public HttpResponseMessage GetDiv([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a / b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }
}
```

<span data-ttu-id="c702f-158">Stiskněte klávesu **F6** toobuild a ověřte hello řešení.</span><span class="sxs-lookup"><span data-stu-id="c702f-158">Press **F6** toobuild and verify hello solution.</span></span>

## <a name="publish-hello-project-tooazure"></a><span data-ttu-id="c702f-159">Publikování projektu tooAzure hello</span><span class="sxs-lookup"><span data-stu-id="c702f-159">Publish hello project tooAzure</span></span>
<span data-ttu-id="c702f-160">Tento krok hello Visual Studio je projekt publikované tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c702f-160">In this step hello Visual Studio project is published tooAzure.</span></span> <span data-ttu-id="c702f-161">Tento krok hello video začíná na 5:45.</span><span class="sxs-lookup"><span data-stu-id="c702f-161">This step of hello video starts at 5:45.</span></span>

<span data-ttu-id="c702f-162">toopublish hello tooAzure projektu, klikněte pravým tlačítkem na hello **APIMAADDemo** projektu v sadě Visual Studio a zvolte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="c702f-162">toopublish hello project tooAzure, right-click hello **APIMAADDemo** project in Visual Studio and choose **Publish**.</span></span> <span data-ttu-id="c702f-163">Ponechat výchozí nastavení hello v hello **Publikovat Web** dialogové okno a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="c702f-163">Keep hello default settings in hello **Publish Web** dialog box and click **Publish**.</span></span>

![Publikování webu][api-management-web-publish]

## <a name="grant-permissions-toohello-azure-ad-backend-service-application"></a><span data-ttu-id="c702f-165">Udělení oprávnění aplikace služby back-end toohello Azure AD</span><span class="sxs-lookup"><span data-stu-id="c702f-165">Grant permissions toohello Azure AD backend service application</span></span>
<span data-ttu-id="c702f-166">V adresáři služby Azure AD jako součást konfigurace hello a proces publikování projektu webového rozhraní API je vytvořena nová aplikace pro hello back-end službu.</span><span class="sxs-lookup"><span data-stu-id="c702f-166">A new application for hello backend service is created in your Azure AD directory as part of hello configuring and publishing process of your Web API project.</span></span> <span data-ttu-id="c702f-167">V tomto kroku hello video od 6:13 oprávnění toohello webového rozhraní API back-end.</span><span class="sxs-lookup"><span data-stu-id="c702f-167">In this step of hello video, starting at 6:13, permissions are granted toohello Web API backend.</span></span>

![Aplikace][api-management-aad-backend-app]

<span data-ttu-id="c702f-169">Klikněte na název hello hello aplikace tooconfigure hello požadované oprávnění.</span><span class="sxs-lookup"><span data-stu-id="c702f-169">Click hello name of hello application tooconfigure hello required permissions.</span></span> <span data-ttu-id="c702f-170">Přejděte toohello **konfigurace** a přejděte dolů toohello **oprávnění tooother aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="c702f-170">Navigate toohello **Configure** tab and scroll down toohello **permissions tooother applications** section.</span></span> <span data-ttu-id="c702f-171">Klikněte na tlačítko hello **oprávnění aplikací** rozevíracího seznamu vedle položky **Windows** **Azure Active Directory**, zaškrtněte políčko hello pro **čtení dat adresáře**a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c702f-171">Click hello **Application Permissions** drop-down beside **Windows** **Azure Active Directory**, check hello box for **Read directory data**, and click **Save**.</span></span>

![Přidání oprávnění][api-management-aad-add-permissions]

> [!NOTE]
> <span data-ttu-id="c702f-173">Pokud **Windows** **Azure Active Directory** nejsou uvedené v části oprávnění tooother aplikace, klikněte na tlačítko **přidat aplikaci** a přidejte ho do seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="c702f-173">If **Windows** **Azure Active Directory** is not listed under permissions tooother applications, click **Add application** and add it from hello list.</span></span>
> 
> 

<span data-ttu-id="c702f-174">Poznamenejte si hello **identifikátor Id URI aplikace** pro použití v následném kroku při aplikaci Azure AD je nakonfigurován pro portál pro vývojáře hello API Management.</span><span class="sxs-lookup"><span data-stu-id="c702f-174">Make a note of hello **App Id URI** for use in a subsequent step when an Azure AD application is configured for hello API Management developer portal.</span></span>

![Id URI aplikace][api-management-aad-sso-uri]

## <a name="import-hello-web-api-into-api-management"></a><span data-ttu-id="c702f-176">Importovat hello webového rozhraní API do rozhraní API Management</span><span class="sxs-lookup"><span data-stu-id="c702f-176">Import hello Web API into API Management</span></span>
<span data-ttu-id="c702f-177">Rozhraní API se konfigurují na hello rozhraní API portálu vydavatele, který je přístupný prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c702f-177">APIs are configured from hello API publisher portal, which is accessed through hello Azure Portal.</span></span> <span data-ttu-id="c702f-178">tooreach, klikněte na tlačítko **portál vydavatele** z hello nástrojů služby API Management.</span><span class="sxs-lookup"><span data-stu-id="c702f-178">tooreach it, click **Publisher portal** from hello toolbar of your API Management service.</span></span> <span data-ttu-id="c702f-179">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Správa vašeho prvního rozhraní API] [ Manage your first API] kurzu.</span><span class="sxs-lookup"><span data-stu-id="c702f-179">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Manage your first API][Manage your first API] tutorial.</span></span>

![Portál vydavatele][api-management-management-console]

<span data-ttu-id="c702f-181">Operace jde [tooAPIs přidat ručně](api-management-howto-add-operations.md), nebo může být importován.</span><span class="sxs-lookup"><span data-stu-id="c702f-181">Operations can be [added tooAPIs manually](api-management-howto-add-operations.md), or they can be imported.</span></span> <span data-ttu-id="c702f-182">V tomto videu se operace importují ve formátu Swagger od 6:40.</span><span class="sxs-lookup"><span data-stu-id="c702f-182">In this video, operations are imported in Swagger format starting at 6:40.</span></span>

<span data-ttu-id="c702f-183">Vytvořte soubor s názvem `calcapi.json` s následující obsah a uložit ho tooyour počítače.</span><span class="sxs-lookup"><span data-stu-id="c702f-183">Create a file named `calcapi.json` with following contents and save it tooyour computer.</span></span> <span data-ttu-id="c702f-184">Ujistěte se, že hello `host` atribut odkazuje tooyour webového rozhraní API back-end.</span><span class="sxs-lookup"><span data-stu-id="c702f-184">Ensure that hello `host` attribute points tooyour Web API backend.</span></span> <span data-ttu-id="c702f-185">V tomto příkladu `"host": "apimaaddemo.azurewebsites.net"` se používá.</span><span class="sxs-lookup"><span data-stu-id="c702f-185">In this example `"host": "apimaaddemo.azurewebsites.net"` is used.</span></span>

```json
{
  "swagger": "2.0",
  "info": {
    "title": "Calculator",
    "description": "Arithmetics over HTTP!",
    "version": "1.0"
  },
  "host": "apimaaddemo.azurewebsites.net",
  "basePath": "/api",
  "schemes": [
    "http"
  ],
  "paths": {
    "/add?a={a}&b={b}": {
      "get": {
        "description": "Responds with a sum of two numbers.",
        "operationId": "Add two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>51</code>.",
            "required": true,
            "type": "string",
            "default": "51",
            "enum": [
              "51"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>49</code>.",
            "required": true,
            "type": "string",
            "default": "49",
            "enum": [
              "49"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/sub?a={a}&b={b}": {
      "get": {
        "description": "Responds with a difference between two numbers.",
        "operationId": "Subtract two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>100</code>.",
            "required": true,
            "type": "string",
            "default": "100",
            "enum": [
              "100"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>50</code>.",
            "required": true,
            "type": "string",
            "default": "50",
            "enum": [
              "50"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/div?a={a}&b={b}": {
      "get": {
        "description": "Responds with a quotient of two numbers.",
        "operationId": "Divide two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>100</code>.",
            "required": true,
            "type": "string",
            "default": "100",
            "enum": [
              "100"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>20</code>.",
            "required": true,
            "type": "string",
            "default": "20",
            "enum": [
              "20"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/mul?a={a}&b={b}": {
      "get": {
        "description": "Responds with a product of two numbers.",
        "operationId": "Multiply two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>20</code>.",
            "required": true,
            "type": "string",
            "default": "20",
            "enum": [
              "20"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>5</code>.",
            "required": true,
            "type": "string",
            "default": "5",
            "enum": [
              "5"
            ]
          }
        ],
        "responses": { }
      }
    }
  }
}
```

<span data-ttu-id="c702f-186">tooimport hello kalkulačky rozhraní API, klikněte na tlačítko **rozhraní API** z hello **API Management** nabídky na levé hello a pak klikněte na tlačítko **rozhraní API pro Import**.</span><span class="sxs-lookup"><span data-stu-id="c702f-186">tooimport hello calculator API, click **APIs** from hello **API Management** menu on hello left, and then click **Import API**.</span></span>

![Tlačítko Importovat rozhraní API][api-management-import-api]

<span data-ttu-id="c702f-188">Proveďte následující kroky rozhraní API kalkulačky hello tooconfigure hello.</span><span class="sxs-lookup"><span data-stu-id="c702f-188">Perform hello following steps tooconfigure hello calculator API.</span></span>

1. <span data-ttu-id="c702f-189">Klikněte na tlačítko **ze souboru**, procházet toohello `calculator.json` soubor uložit a klikněte na tlačítko hello **Swagger** přepínač.</span><span class="sxs-lookup"><span data-stu-id="c702f-189">Click **From file**, browse toohello `calculator.json` file you saved, and click hello **Swagger** radio button.</span></span>
2. <span data-ttu-id="c702f-190">Typ **calc** do hello **přípona adresy URL webového rozhraní API** textové pole.</span><span class="sxs-lookup"><span data-stu-id="c702f-190">Type **calc** into hello **Web API URL suffix** textbox.</span></span>
3. <span data-ttu-id="c702f-191">Klikněte na tlačítko v hello **produkty (volitelné)** pole a zvolte **Starter**.</span><span class="sxs-lookup"><span data-stu-id="c702f-191">Click in hello **Products (optional)** box and choose **Starter**.</span></span>
4. <span data-ttu-id="c702f-192">Klikněte na tlačítko **Uložit** tooimport hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c702f-192">Click **Save** tooimport hello API.</span></span>

![Přidání nového rozhraní API][api-management-import-new-api]

<span data-ttu-id="c702f-194">Po importu rozhraní API hello hello souhrnná stránka rozhraní API hello zobrazí na portálu vydavatele hello.</span><span class="sxs-lookup"><span data-stu-id="c702f-194">Once hello API is imported, hello summary page for hello API is displayed in hello publisher portal.</span></span>

## <a name="call-hello-api-unsuccessfully-from-hello-developer-portal"></a><span data-ttu-id="c702f-195">Volání rozhraní API hello neúspěšně z portálu pro vývojáře hello</span><span class="sxs-lookup"><span data-stu-id="c702f-195">Call hello API unsuccessfully from hello developer portal</span></span>
<span data-ttu-id="c702f-196">V tomto okamžiku hello rozhraní API byla naimportována do rozhraní API správy, ale nejde ještě volat úspěšně z portálu pro vývojáře hello protože hello back-end službu chráněné pomocí ověřování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c702f-196">At this point, hello API has been imported into API Management, but cannot yet be called successfully from hello developer portal because hello backend service is protected with Azure AD authentication.</span></span> <span data-ttu-id="c702f-197">Tento postup je znázorněn v hello videa začínající na 7:40 pomocí hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="c702f-197">This is demonstrated in hello video starting at 7:40 using hello following steps.</span></span>

<span data-ttu-id="c702f-198">Klikněte na tlačítko **portál pro vývojáře** z hello horní pravé části portálu vydavatele hello.</span><span class="sxs-lookup"><span data-stu-id="c702f-198">Click **Developer portal** from hello top-right side of hello publisher portal.</span></span>

![Portál pro vývojáře][api-management-developer-portal-menu]

<span data-ttu-id="c702f-200">Klikněte na tlačítko **rozhraní API** a klikněte na tlačítko hello **kalkulačky** rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c702f-200">Click **APIs** and click hello **Calculator** API.</span></span>

![Portál pro vývojáře][api-management-dev-portal-apis]

<span data-ttu-id="c702f-202">Klikněte na tlačítko **vyzkoušet**.</span><span class="sxs-lookup"><span data-stu-id="c702f-202">Click **Try it**.</span></span>

![Vyzkoušet][api-management-dev-portal-try-it]

<span data-ttu-id="c702f-204">Klikněte na tlačítko **odeslat** a poznamenejte si stav odezvy hello **401 – Neověřeno**.</span><span class="sxs-lookup"><span data-stu-id="c702f-204">Click **Send** and note hello response status of **401 Unauthorized**.</span></span>

![Odeslat][api-management-dev-portal-send-401]

<span data-ttu-id="c702f-206">Hello požadavek není autorizovaný, protože hello back-end rozhraní API je chráněn službou Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c702f-206">hello request is unauthorized because hello backend API is protected by Azure Active Directory.</span></span> <span data-ttu-id="c702f-207">Před úspěšně volání rozhraní API hello portál pro vývojáře hello musí být nakonfigurované tooauthorize vývojáře, kteří používají OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="c702f-207">Before successfully calling hello API hello developer portal must be configured tooauthorize developers using OAuth 2.0.</span></span> <span data-ttu-id="c702f-208">Tento proces je popsán v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="c702f-208">This process is described in hello following sections.</span></span>

## <a name="register-hello-developer-portal-as-an-aad-application"></a><span data-ttu-id="c702f-209">Zaregistrujte se jako aplikaci AAD hello portál pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="c702f-209">Register hello developer portal as an AAD application</span></span>
<span data-ttu-id="c702f-210">Hello prvním krokem při konfiguraci hello vývojáře portálu tooauthorize vývojáře, kteří používají OAuth 2.0 je portál pro vývojáře hello tooregister jako aplikaci AAD.</span><span class="sxs-lookup"><span data-stu-id="c702f-210">hello first step in configuring hello developer portal tooauthorize developers using OAuth 2.0 is tooregister hello developer portal as an AAD application.</span></span> <span data-ttu-id="c702f-211">Tento postup je znázorněn od 8:27 v hello video.</span><span class="sxs-lookup"><span data-stu-id="c702f-211">This is demonstrated starting at 8:27 in hello video.</span></span>

<span data-ttu-id="c702f-212">Přejděte toohello Azure AD tenant z první krok hello toto video, v tomto příkladu **APIMDemo** a přejděte toohello **aplikace** kartě.</span><span class="sxs-lookup"><span data-stu-id="c702f-212">Navigate toohello Azure AD tenant from hello first step of this video, in this example **APIMDemo** and navigate toohello **Applications** tab.</span></span>

![Nová aplikace][api-management-aad-new-application-devportal]

<span data-ttu-id="c702f-214">Klikněte na tlačítko hello **přidat** tlačítko toocreate novou aplikaci Azure Active Directory a vyberte **přidat aplikaci, kterou vyvíjí Moje organizace**.</span><span class="sxs-lookup"><span data-stu-id="c702f-214">Click hello **Add** button toocreate a new Azure Active Directory application, and choose **Add an application my organization is developing**.</span></span>

![Nová aplikace][api-management-new-aad-application-menu]

<span data-ttu-id="c702f-216">Zvolte **webové aplikace nebo webové rozhraní API**, zadejte název a klikněte na šipku další hello.</span><span class="sxs-lookup"><span data-stu-id="c702f-216">Choose **Web application and/or Web API**, enter a name, and click hello next arrow.</span></span> <span data-ttu-id="c702f-217">V tomto příkladu **APIMDeveloperPortal** se používá.</span><span class="sxs-lookup"><span data-stu-id="c702f-217">In this example **APIMDeveloperPortal** is used.</span></span>

![Nová aplikace][api-management-aad-new-application-devportal-1]

<span data-ttu-id="c702f-219">Pro **přihlašovací adresa URL** zadejte adresu URL hello služby API Management a připojte `/signin`.</span><span class="sxs-lookup"><span data-stu-id="c702f-219">For **Sign-on URL** enter hello URL of your API Management service and append `/signin`.</span></span> <span data-ttu-id="c702f-220">V tomto příkladu `https://contoso5.portal.azure-api.net/signin` se používá.</span><span class="sxs-lookup"><span data-stu-id="c702f-220">In this example `https://contoso5.portal.azure-api.net/signin` is used.</span></span>

<span data-ttu-id="c702f-221">Pro **URL Id aplikace** zadejte adresu URL hello služby API Management a připojte některé jedinečných znaků.</span><span class="sxs-lookup"><span data-stu-id="c702f-221">For **App Id URL** enter hello URL of your API Management service and append some unique characters.</span></span> <span data-ttu-id="c702f-222">To může být jakékoli požadované znaky a v tomto příkladu `https://contoso5.portal.azure-api.net/dp` se používá.</span><span class="sxs-lookup"><span data-stu-id="c702f-222">These can be any desired characters and in this example `https://contoso5.portal.azure-api.net/dp` is used.</span></span> <span data-ttu-id="c702f-223">Když hello potřeby **vlastností aplikace** jsou nakonfigurovaná, klikněte na tlačítko hello zaškrtnutí toocreate hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="c702f-223">When hello  desired **App properties** are configured, click hello check mark toocreate hello application.</span></span>

![Nová aplikace][api-management-aad-new-application-devportal-2]

## <a name="configure-an-api-management-oauth-20-authorization-server"></a><span data-ttu-id="c702f-225">Konfigurace serveru autorizace OAuth 2.0 rozhraní API Management</span><span class="sxs-lookup"><span data-stu-id="c702f-225">Configure an API Management OAuth 2.0 authorization server</span></span>
<span data-ttu-id="c702f-226">dalším krokem Hello je tooconfigure serveru autorizace OAuth 2.0 ve službě API Management.</span><span class="sxs-lookup"><span data-stu-id="c702f-226">hello next step is tooconfigure an OAuth 2.0 authorization server in API Management.</span></span> <span data-ttu-id="c702f-227">Tento krok je znázorněn v videa začínající na 9:43 hello.</span><span class="sxs-lookup"><span data-stu-id="c702f-227">This step is demonstrated in hello video starting at 9:43.</span></span>

<span data-ttu-id="c702f-228">Klikněte na tlačítko **zabezpečení** hello API Management na nabídce hello vlevo, klikněte na tlačítko **OAuth 2.0**a potom klikněte na **přidat autorizační** serveru.</span><span class="sxs-lookup"><span data-stu-id="c702f-228">Click **Security** from hello API Management menu on hello left, click **OAuth 2.0**, and then click **Add authorization** server.</span></span>

![Přidání serveru ověřování][api-management-add-authorization-server]

<span data-ttu-id="c702f-230">Zadejte název a volitelný popis v hello **název** a **popis** pole.</span><span class="sxs-lookup"><span data-stu-id="c702f-230">Enter a name and an optional description in hello **Name** and **Description** fields.</span></span> <span data-ttu-id="c702f-231">Tato pole jsou použité tooidentify hello OAuth 2.0 autorizace serveru v rámci instance služby API Management hello.</span><span class="sxs-lookup"><span data-stu-id="c702f-231">These fields are used tooidentify hello OAuth 2.0 authorization server within hello API Management service instance.</span></span> <span data-ttu-id="c702f-232">V tomto příkladu **ukázkový server autorizace** se používá.</span><span class="sxs-lookup"><span data-stu-id="c702f-232">In this example **Authorization server demo** is used.</span></span> <span data-ttu-id="c702f-233">Později při zadávání toobe server OAuth 2.0 používat k ověřování pro rozhraní API, vyberete tento název.</span><span class="sxs-lookup"><span data-stu-id="c702f-233">Later when you specify an OAuth 2.0 server toobe used for authentication for an API, you will select this name.</span></span>

<span data-ttu-id="c702f-234">Pro hello **adresa URL stránky registrace klienta** zadejte hodnotu zástupného symbolu, jako `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="c702f-234">For hello **Client registration page URL** enter a placeholder value such as `http://localhost`.</span></span>  <span data-ttu-id="c702f-235">Hello **adresa URL stránky registrace klienta** body toohello stránka, kterou můžou uživatelé použít toocreate a nakonfigurovat svoje vlastní účty zprostředkovatelů OAuth 2.0, které podporují správu uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="c702f-235">hello **Client registration page URL** points toohello page that users can use toocreate and configure their own accounts for OAuth 2.0 providers that support user management of accounts.</span></span> <span data-ttu-id="c702f-236">V tomto příkladu uživatele není vytvořit a nakonfigurovat svoje vlastní účty, tak se používá zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="c702f-236">In this example users do not create and configure their own accounts so a placeholder is used.</span></span>

![Přidání serveru ověřování][api-management-add-authorization-server-1]

<span data-ttu-id="c702f-238">Potom zadejte **adresu URL koncového bodu autorizace** a **adresu URL koncového bodu Token**.</span><span class="sxs-lookup"><span data-stu-id="c702f-238">Next, specify **Authorization endpoint URL** and **Token endpoint URL**.</span></span>

![Autorizace serveru][api-management-add-authorization-server-1a]

<span data-ttu-id="c702f-240">Tyto hodnoty lze získat z hello **koncových bodů aplikace** stránku hello AAD aplikace, které jste vytvořili pro portál pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="c702f-240">These values can be retrieved from hello **App Endpoints** page of hello AAD application you created for hello developer portal.</span></span> <span data-ttu-id="c702f-241">Koncové body hello tooaccess přejděte toohello **konfigurace** hello AAD aplikace a klikněte na **zobrazit koncové body**.</span><span class="sxs-lookup"><span data-stu-id="c702f-241">tooaccess hello endpoints navigate toohello **Configure** tab for hello AAD application and click **View endpoints**.</span></span>

![Aplikace][api-management-aad-devportal-application]

![Zobrazit koncové body][api-management-aad-view-endpoints]

<span data-ttu-id="c702f-244">Kopírování hello **koncový bod autorizace OAuth 2.0** a vložte jej do hello **adresu URL koncového bodu autorizace** textové pole.</span><span class="sxs-lookup"><span data-stu-id="c702f-244">Copy hello **OAuth 2.0 authorization endpoint** and paste it into hello **Authorization endpoint URL** textbox.</span></span>

![Přidání serveru ověřování][api-management-add-authorization-server-2]

<span data-ttu-id="c702f-246">Kopírování hello **koncový bod tokenu OAuth 2.0** a vložte jej do hello **adresu URL koncového bodu Token** textové pole.</span><span class="sxs-lookup"><span data-stu-id="c702f-246">Copy hello **OAuth 2.0 token endpoint** and paste it into hello **Token endpoint URL** textbox.</span></span>

![Přidání serveru ověřování][api-management-add-authorization-server-2a]

<span data-ttu-id="c702f-248">Kromě toho toopasting v hello koncovému bodu tokenu, přidáte další text parametr s názvem **prostředků** a pro hodnotu hello používat hello **identifikátor Id URI aplikace** z hello AAD aplikace hello back-end službu, která byla vytvoří, když byla publikována hello projektu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c702f-248">In addition toopasting in hello token endpoint, add an additional body parameter named **resource** and for hello value use hello **App Id URI** from hello AAD application for hello backend service that was created when hello Visual Studio project was published.</span></span>

![Id URI aplikace][api-management-aad-sso-uri]

<span data-ttu-id="c702f-250">Potom zadejte pověření klienta hello.</span><span class="sxs-lookup"><span data-stu-id="c702f-250">Next, specify hello client credentials.</span></span> <span data-ttu-id="c702f-251">Tyto jsou hello přihlašovací údaje pro prostředek hello chcete tooaccess, v takovém případě hello portál pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="c702f-251">These are hello credentials for hello resource you want tooaccess, in this case hello developer portal.</span></span>

![Pověření klienta][api-management-client-credentials]

<span data-ttu-id="c702f-253">tooget hello **Id klienta**, přejděte toohello **konfigurace** kartě hello AAD aplikace pro vývojáře hello portál a zkopírujte hello **Id klienta**.</span><span class="sxs-lookup"><span data-stu-id="c702f-253">tooget hello **Client Id**, navigate toohello **Configure** tab of hello AAD application for hello developer portal and copy hello **Client Id**.</span></span>

<span data-ttu-id="c702f-254">tooget hello **tajný klíč klienta** klikněte na tlačítko hello **vyberte dobu trvání** rozevírací seznam v hello **klíče** části a zadat interval.</span><span class="sxs-lookup"><span data-stu-id="c702f-254">tooget hello **Client Secret** click hello **Select duration** drop-down in hello **Keys** section and specify an interval.</span></span> <span data-ttu-id="c702f-255">V tomto příkladu se používá 1 rok.</span><span class="sxs-lookup"><span data-stu-id="c702f-255">In this example 1 year is used.</span></span>

![ID klienta][api-management-aad-client-id]

<span data-ttu-id="c702f-257">Klikněte na tlačítko **Uložit** toosave hello konfiguraci a zobrazení hello klíč.</span><span class="sxs-lookup"><span data-stu-id="c702f-257">Click **Save** toosave hello configuration and display hello key.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="c702f-258">Tento klíč si poznamenejte.</span><span class="sxs-lookup"><span data-stu-id="c702f-258">Make a note of this key.</span></span> <span data-ttu-id="c702f-259">Po zavření okna konfigurace Azure Active Directory hello hello klíč nelze zobrazit znovu.</span><span class="sxs-lookup"><span data-stu-id="c702f-259">Once you close hello Azure Active Directory configuration window, hello key cannot be displayed again.</span></span>
> 
> 

<span data-ttu-id="c702f-260">Kopírování hello klíče toohello schránky, portál vydavatele back toohello přepínače, vložte klíč hello do hello **tajný klíč klienta** textovému poli a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c702f-260">Copy hello key toohello clipboard, switch back toohello publisher portal, paste hello key into hello **Client Secret** textbox, and click **Save**.</span></span>

![Přidání serveru ověřování][api-management-add-authorization-server-3]

<span data-ttu-id="c702f-262">Hned za hello pověření klienta je udělení autorizačního kódu.</span><span class="sxs-lookup"><span data-stu-id="c702f-262">Immediately following hello client credentials is an authorization code grant.</span></span> <span data-ttu-id="c702f-263">Zkopírujte tento autorizační kód a přepínač back tooyour aplikace portálu služby Azure AD vývojáře konfigurace stránky a vložte udělení autorizace hello do hello **adresa URL odpovědi** pole a klikněte na tlačítko **Uložit** znovu.</span><span class="sxs-lookup"><span data-stu-id="c702f-263">Copy this authorization code and switch back tooyour Azure AD developer portal application configure page, and paste hello authorization grant into hello **Reply URL** field, and click **Save** again.</span></span>

![Adresa URL odpovědi][api-management-aad-reply-url]

<span data-ttu-id="c702f-265">dalším krokem Hello je tooconfigure hello oprávnění pro portál pro vývojáře hello AAD aplikace.</span><span class="sxs-lookup"><span data-stu-id="c702f-265">hello next step is tooconfigure hello permissions for hello developer portal AAD application.</span></span> <span data-ttu-id="c702f-266">Klikněte na tlačítko **oprávnění aplikací** a zaškrtněte políčko hello pro **čtení dat adresáře**.</span><span class="sxs-lookup"><span data-stu-id="c702f-266">Click **Application Permissions** and check hello box for **Read directory data**.</span></span> <span data-ttu-id="c702f-267">Klikněte na tlačítko **Uložit** toosave to změnit a potom klikněte na **přidat aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="c702f-267">Click **Save** toosave this change, and then click **Add application**.</span></span>

![Přidání oprávnění][api-management-add-devportal-permissions]

<span data-ttu-id="c702f-269">Klikněte na ikonu hledání hello, typ **APIM** do hello počínaje pole, vyberte **APIMAADDemo**a klikněte na tlačítko zaškrtnutí toosave hello.</span><span class="sxs-lookup"><span data-stu-id="c702f-269">Click hello search icon, type **APIM** into hello Starting with box, select **APIMAADDemo**, and click hello check mark toosave.</span></span>

![Přidání oprávnění][api-management-aad-add-app-permissions]

<span data-ttu-id="c702f-271">Klikněte na tlačítko **delegovaná oprávnění** pro **APIMAADDemo** a zaškrtněte políčko hello pro **přístup APIMAADDemo**a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c702f-271">Click **Delegated Permissions** for **APIMAADDemo** and check hello box for **Access APIMAADDemo**, and click **Save**.</span></span> <span data-ttu-id="c702f-272">To umožňuje vývojáři hello aplikace portálu tooaccess hello back-end službu.</span><span class="sxs-lookup"><span data-stu-id="c702f-272">This allows hello developer portal application tooaccess hello backend service.</span></span>

![Přidání oprávnění][api-management-aad-add-delegated-permissions]

## <a name="enable-oauth-20-user-authorization-for-hello-calculator-api"></a><span data-ttu-id="c702f-274">Povolení autorizace uživatelů OAuth 2.0 pro rozhraní API kalkulačky hello</span><span class="sxs-lookup"><span data-stu-id="c702f-274">Enable OAuth 2.0 user authorization for hello Calculator API</span></span>
<span data-ttu-id="c702f-275">Teď, když hello OAuth 2.0 je server nakonfigurovaný, můžete je zadat v hello nastavení zabezpečení pro vaše rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c702f-275">Now that hello OAuth 2.0 server is configured, you can specify it in hello security settings for your API.</span></span> <span data-ttu-id="c702f-276">Tento krok je znázorněn v hello videa začínající na 14:30.</span><span class="sxs-lookup"><span data-stu-id="c702f-276">This step is demonstrated in hello video starting at 14:30.</span></span>

<span data-ttu-id="c702f-277">Klikněte na tlačítko **rozhraní API** v levé nabídce text hello a klikněte na tlačítko **kalkulačky** tooview a konfigurovat jeho nastavení.</span><span class="sxs-lookup"><span data-stu-id="c702f-277">Click **APIs** in hello left menu, and click  **Calculator** tooview and configure its settings.</span></span>

![API kalkulačky][api-management-calc-api]

<span data-ttu-id="c702f-279">Přejděte toohello **zabezpečení** kartě, zkontrolujte hello **OAuth 2.0** zaškrtávací políčko, vyberte hello serveru požadované ověřování z hello **serveru ověřování** rozevíracího seznamu a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c702f-279">Navigate toohello **Security** tab, check hello **OAuth 2.0** checkbox, select hello desired authorization server from hello **Authorization server** drop-down, and click **Save**.</span></span>

![API kalkulačky][api-management-enable-aad-calculator]

## <a name="successfully-call-hello-calculator-api-from-hello-developer-portal"></a><span data-ttu-id="c702f-281">Úspěšně hello rozhraní API kalkulačky volejte z portálu pro vývojáře hello</span><span class="sxs-lookup"><span data-stu-id="c702f-281">Successfully call hello Calculator API from hello developer portal</span></span>
<span data-ttu-id="c702f-282">Teď, když na hello rozhraní API je nakonfigurováno autorizace hello OAuth 2.0, můžete jeho operace úspěšně volat z středisku pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="c702f-282">Now that hello OAuth 2.0 authorization is configured on hello API, its operations can be successfully called from hello developer center.</span></span> <span data-ttu-id="c702f-283">Tento krok je znázorněn v hello videa začínající na 15:00.</span><span class="sxs-lookup"><span data-stu-id="c702f-283">THis step is demonstrated in hello video starting at 15:00.</span></span>

<span data-ttu-id="c702f-284">Přejděte zpět toohello **přidat dvě celá čísla** operaci hello kalkulačky služby v portálu pro vývojáře hello a klikněte na **vyzkoušet**.</span><span class="sxs-lookup"><span data-stu-id="c702f-284">Navigate back toohello **Add two integers** operation of hello calculator service in hello developer portal and click **Try it**.</span></span> <span data-ttu-id="c702f-285">Poznámka: hello novou položku hello **autorizace** části odpovídající serveru ověřování toohello jste právě přidali.</span><span class="sxs-lookup"><span data-stu-id="c702f-285">Note hello new item in hello **Authorization** section corresponding toohello authorization server you just added.</span></span>

![API kalkulačky][api-management-calc-authorization-server]

<span data-ttu-id="c702f-287">Vyberte **autorizační kód** z hello autorizace rozevíracího seznamu a zadejte přihlašovací údaje účtu toouse hello hello.</span><span class="sxs-lookup"><span data-stu-id="c702f-287">Select **Authorization code** from hello authorization drop-down list and enter hello credentials of hello account toouse.</span></span> <span data-ttu-id="c702f-288">Pokud jste již přihlášeni pomocí účtu hello nemusí zobrazí se výzva.</span><span class="sxs-lookup"><span data-stu-id="c702f-288">If you are already signed in with hello account you may not be prompted.</span></span>

![API kalkulačky][api-management-devportal-authorization-code]

<span data-ttu-id="c702f-290">Klikněte na tlačítko **odeslat** a Poznámka hello **stav odpovědi** z **200 OK** a výsledky hello hello operace v obsahu odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="c702f-290">Click **Send** and note hello **Response status** of **200 OK** and hello results of hello operation in hello response content.</span></span>

![API kalkulačky][api-management-devportal-response]

## <a name="configure-a-desktop-application-toocall-hello-api"></a><span data-ttu-id="c702f-292">Konfigurace desktopová aplikace toocall hello rozhraní API</span><span class="sxs-lookup"><span data-stu-id="c702f-292">Configure a desktop application toocall hello API</span></span>
<span data-ttu-id="c702f-293">Další postup Hello v hello video začíná na 16:30 a nakonfiguruje hello toocall jednoduchou aplikaci plochy rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c702f-293">hello next procedure in hello video starts at 16:30 and configures a simple desktop application toocall hello API.</span></span> <span data-ttu-id="c702f-294">prvním krokem Hello je tooregister hello desktopová aplikace ve službě Azure AD a poskytněte přístup toohello adresář a toohello back-end službu.</span><span class="sxs-lookup"><span data-stu-id="c702f-294">hello first step is tooregister hello desktop application in Azure AD and give it access toohello directory and toohello backend service.</span></span> <span data-ttu-id="c702f-295">Na 18:25 je ukázka aplikace pracovní plochy hello volání operace na API kalkulačky hello.</span><span class="sxs-lookup"><span data-stu-id="c702f-295">At 18:25 there is a demonstration of hello desktop application calling an operation on hello calculator API.</span></span>

## <a name="configure-a-jwt-validation-policy-toopre-authorize-requests"></a><span data-ttu-id="c702f-296">Konfigurace JWT ověření zásad toopre-autorizaci požadavků</span><span class="sxs-lookup"><span data-stu-id="c702f-296">Configure a JWT validation policy toopre-authorize requests</span></span>
<span data-ttu-id="c702f-297">Hello poslední postup v hello video začíná na 20:48 a ukazuje, jak toouse hello [ověření JWT](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) zásad toopre-autorizaci požadavků na ověřením hello přístupové tokeny každého příchozího požadavku.</span><span class="sxs-lookup"><span data-stu-id="c702f-297">hello final procedure in hello video starts at 20:48 and shows you how toouse hello [Validate JWT](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) policy toopre-authorize requests by validating hello access tokens of each incoming request.</span></span> <span data-ttu-id="c702f-298">Pokud hello požadavek není ověřen hello zásady ověřování tokenů JWT, žádost o hello je blokována API Management a není předají toohello back-end.</span><span class="sxs-lookup"><span data-stu-id="c702f-298">If hello request is not validated by hello Validate JWT policy, hello request is blocked by API Management and is not passed along toohello backend.</span></span>

```xml
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
    <openid-config url="https://login.microsoftonline.com/DemoAPIM.onmicrosoft.com/.well-known/openid-configuration" />
    <required-claims>
        <claim name="aud">
            <value>https://DemoAPIM.NOTonmicrosoft.com/APIMAADDemo</value>
        </claim>
    </required-claims>
</validate-jwt>
```

<span data-ttu-id="c702f-299">Jiné předvedení konfiguraci a použití této zásady, najdete v části [cloudu zahrnují díl 177: víc funkcí správy rozhraní API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a převinutí vpřed too13:50.</span><span class="sxs-lookup"><span data-stu-id="c702f-299">For another demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too13:50.</span></span> <span data-ttu-id="c702f-300">Rychlé převinutí vpřed too15:00 toosee hello zásady nakonfigurované v editoru zásad hello, a potom too18:50 pro předvedení volání operace z portálu pro vývojáře hello s i bez hello vyžaduje autorizační token.</span><span class="sxs-lookup"><span data-stu-id="c702f-300">Fast forward too15:00 toosee hello policies configured in hello policy editor and then too18:50 for a demonstration of calling an operation from hello developer portal both with and without hello required authorization token.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c702f-301">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c702f-301">Next steps</span></span>
* <span data-ttu-id="c702f-302">Podívejte se na další [videa](https://azure.microsoft.com/documentation/videos/index/?services=api-management) o službě API Management.</span><span class="sxs-lookup"><span data-stu-id="c702f-302">Check out more [videos](https://azure.microsoft.com/documentation/videos/index/?services=api-management) about API Management.</span></span>
* <span data-ttu-id="c702f-303">Pro jiné způsoby toosecure službě back-end, najdete v části [vzájemného ověření certifikátů](api-management-howto-mutual-certificates.md).</span><span class="sxs-lookup"><span data-stu-id="c702f-303">For other ways toosecure your backend service, see [Mutual Certificate authentication](api-management-howto-mutual-certificates.md).</span></span>

[api-management-management-console]: ./media/api-management-howto-protect-backend-with-aad/api-management-management-console.png

[api-management-import-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-new-api.png
[api-management-create-aad-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad-menu.png
[api-management-create-aad]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad.png
[api-management-new-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-web-app.png
[api-management-new-project]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project.png
[api-management-new-project-cloud]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project-cloud.png
[api-management-change-authentication]: ./media/api-management-howto-protect-backend-with-aad/api-management-change-authentication.png
[api-management-sign-in-vidual-studio]: ./media/api-management-howto-protect-backend-with-aad/api-management-sign-in-vidual-studio.png
[api-management-configure-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-configure-web-app.png
[api-management-aad-domains]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-domains.png
[api-management-add-controller]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-controller.png
[api-management-web-publish]: ./media/api-management-howto-protect-backend-with-aad/api-management-web-publish.png
[api-management-aad-backend-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-backend-app.png
[api-management-aad-add-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-permissions.png
[api-management-developer-portal-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-developer-portal-menu.png
[api-management-dev-portal-apis]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-apis.png
[api-management-dev-portal-try-it]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-try-it.png
[api-management-dev-portal-send-401]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-send-401.png
[api-management-aad-new-application-devportal]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal.png
[api-management-aad-new-application-devportal-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-1.png
[api-management-aad-new-application-devportal-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-2.png
[api-management-aad-devportal-application]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-devportal-application.png
[api-management-add-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server.png
[api-management-aad-sso-uri]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-sso-uri.png
[api-management-aad-view-endpoints]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-view-endpoints.png
[api-management-aad-client-id]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-client-id.png
[api-management-add-authorization-server-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1.png
[api-management-add-authorization-server-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2.png
[api-management-add-authorization-server-2a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2a.png
[api-management-add-authorization-server-3]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-3.png
[api-management-aad-reply-url]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-reply-url.png
[api-management-add-devportal-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-devportal-permissions.png
[api-management-aad-add-app-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-app-permissions.png
[api-management-aad-add-delegated-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-delegated-permissions.png
[api-management-calc-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-api.png
[api-management-enable-aad-calculator]: ./media/api-management-howto-protect-backend-with-aad/api-management-enable-aad-calculator.png
[api-management-devportal-authorization-code]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-authorization-code.png
[api-management-devportal-response]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-response.png
[api-management-calc-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-authorization-server.png
[api-management-add-authorization-server-1a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1a.png
[api-management-client-credentials]: ./media/api-management-howto-protect-backend-with-aad/api-management-client-credentials.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-aad-application-menu.png

[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Manage your first API]: api-management-get-started.md
