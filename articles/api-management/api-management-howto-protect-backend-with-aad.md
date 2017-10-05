---
title: "Ochrana back-endu webového rozhraní API pomocí Azure Active Directory a API Management | Microsoft Docs"
description: "Zjistěte, jak k ochraně back-endu webového rozhraní API pomocí Azure Active Directory a API Management."
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
ms.openlocfilehash: 0dfb4102904c2e972e6617fd3851fb1c50147357
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-protect-a-web-api-backend-with-azure-active-directory-and-api-management"></a><span data-ttu-id="a218a-103">Jak chránit, back-endu webového rozhraní API pomocí Azure Active Directory a API Management</span><span class="sxs-lookup"><span data-stu-id="a218a-103">How to protect a Web API backend with Azure Active Directory and API Management</span></span>
<span data-ttu-id="a218a-104">Následující video ukazuje, jak vytvářet back-end webového rozhraní API a chránit pomocí Azure Active Directory a rozhraní API správy protokolu OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="a218a-104">The following video shows how to build a Web API backend and protect it using OAuth 2.0 protocol with Azure Active Directory and API Management.</span></span>  <span data-ttu-id="a218a-105">Tento článek obsahuje přehled a další informace o kroky v videa.</span><span class="sxs-lookup"><span data-stu-id="a218a-105">This article provides an overview and additional information for the steps in the video.</span></span> <span data-ttu-id="a218a-106">Následující 24 minutu video ukazuje, jak na:</span><span class="sxs-lookup"><span data-stu-id="a218a-106">This 24 minute video shows you how to:</span></span>

* <span data-ttu-id="a218a-107">Sestavení webového rozhraní API back-end a zabezpečte ji pomocí AAD - počínaje 1:30</span><span class="sxs-lookup"><span data-stu-id="a218a-107">Build a Web API backend and secure it with AAD - starting at 1:30</span></span>
* <span data-ttu-id="a218a-108">Importovat rozhraní API do rozhraní API Management – počínaje 7:10</span><span class="sxs-lookup"><span data-stu-id="a218a-108">Import the API into API Management - starting at 7:10</span></span>
* <span data-ttu-id="a218a-109">Konfigurace portálu pro vývojáře pro volání rozhraní API – počínaje 9:09</span><span class="sxs-lookup"><span data-stu-id="a218a-109">Configure the Developer portal to call the API - starting at 9:09</span></span>
* <span data-ttu-id="a218a-110">Konfigurace aplikace pro volání rozhraní API – od 18:08</span><span class="sxs-lookup"><span data-stu-id="a218a-110">Configure a desktop application to call the API - starting at 18:08</span></span>
* <span data-ttu-id="a218a-111">Konfigurace zásad ověřování tokenů JWT předem autorizovat požadavky - začínající na 20:47</span><span class="sxs-lookup"><span data-stu-id="a218a-111">Configure a JWT validation policy to pre-authorize requests - starting at 20:47</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Protecting-Web-API-Backend-with-Azure-Active-Directory-and-API-Management/player]
> 
> 

## <a name="create-an-azure-ad-directory"></a><span data-ttu-id="a218a-112">Vytvořte adresář služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a218a-112">Create an Azure AD directory</span></span>
<span data-ttu-id="a218a-113">K zabezpečení vašeho webového rozhraní API zálohovaný pomocí Azure Active Directory je nutné nejdříve vytvořit klienta služby AAD.</span><span class="sxs-lookup"><span data-stu-id="a218a-113">To secure your Web API backed using Azure Active Directory you must first have a an AAD tenant.</span></span> <span data-ttu-id="a218a-114">V tomto videu klienta s názvem **APIMDemo** se používá.</span><span class="sxs-lookup"><span data-stu-id="a218a-114">In this video a tenant named **APIMDemo** is used.</span></span> <span data-ttu-id="a218a-115">Vytvoření klienta služby AAD, přihlaste se do [portálu Azure Classic](https://manage.windowsazure.com) a klikněte na tlačítko **nový**->**App Services**->**služby Active Directory**->**Directory**->**vytvořit vlastní**.</span><span class="sxs-lookup"><span data-stu-id="a218a-115">To create an AAD tenant, sign-in to the [Azure Classic Portal](https://manage.windowsazure.com) and click **New**->**App Services**->**Active Directory**->**Directory**->**Custom Create**.</span></span> 

![Azure Active Directory][api-management-create-aad-menu]

<span data-ttu-id="a218a-117">V tomto příkladu adresář s názvem **APIMDemo** je vytvořena s výchozí doménu s názvem **DemoAPIM.onmicrosoft.com**.</span><span class="sxs-lookup"><span data-stu-id="a218a-117">In this example a directory named **APIMDemo** is created with a default domain named **DemoAPIM.onmicrosoft.com**.</span></span> <span data-ttu-id="a218a-118">Tento adresář se používá napříč videa.</span><span class="sxs-lookup"><span data-stu-id="a218a-118">This directory is used throughout the video.</span></span>

![Azure Active Directory][api-management-create-aad]

## <a name="create-a-web-api-service-secured-by-azure-active-directory"></a><span data-ttu-id="a218a-120">Vytvoření webového rozhraní API služby Zabezpečené přes Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a218a-120">Create a Web API service secured by Azure Active Directory</span></span>
<span data-ttu-id="a218a-121">V tomto kroku se vytvoří webového rozhraní API back-end pomocí Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="a218a-121">In this step, a Web API backend is created using Visual Studio 2013.</span></span> <span data-ttu-id="a218a-122">Tento krok videa se spustí v 1:30.</span><span class="sxs-lookup"><span data-stu-id="a218a-122">This step of the video starts at 1:30.</span></span> <span data-ttu-id="a218a-123">Vytvoření projektu webového rozhraní API back-end v sadě Visual Studio klikněte na položku **soubor**->**nový**->**projektu**a zvolte **webové aplikace ASP.NET** z **webové** seznamu šablon.</span><span class="sxs-lookup"><span data-stu-id="a218a-123">To create Web API backend project in Visual Studio click **File**->**New**->**Project**, and choose **ASP.NET Web Application** from the **Web** templates list.</span></span> <span data-ttu-id="a218a-124">V tomto videu projektu jmenuje **APIMAADDemo**.</span><span class="sxs-lookup"><span data-stu-id="a218a-124">In this video the project is named **APIMAADDemo**.</span></span> <span data-ttu-id="a218a-125">Kliknutím na tlačítko **OK** vytvořte projekt.</span><span class="sxs-lookup"><span data-stu-id="a218a-125">Click **OK** to create the project.</span></span> 

![Visual Studio][api-management-new-web-app]

<span data-ttu-id="a218a-127">Klikněte na tlačítko **webového rozhraní API** z **vyberte seznam šablony** k vytvoření projektu webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a218a-127">Click **Web API** from the **Select a template list** to create a Web API project.</span></span> <span data-ttu-id="a218a-128">Ke konfiguraci klikněte na tlačítko Azure Directory Authentication **změna ověřování**.</span><span class="sxs-lookup"><span data-stu-id="a218a-128">To configure Azure Directory Authentication click **Change Authentication**.</span></span>

![Nový projekt][api-management-new-project]

<span data-ttu-id="a218a-130">Klikněte na tlačítko **účty organizace**a zadejte **domény** klienta služby AAD.</span><span class="sxs-lookup"><span data-stu-id="a218a-130">Click **Organizational Accounts**, and specify the **Domain** of your AAD tenant.</span></span> <span data-ttu-id="a218a-131">V tomto příkladu Doména je **DemoAPIM.onmicrosoft.com**.</span><span class="sxs-lookup"><span data-stu-id="a218a-131">In this example the domain is **DemoAPIM.onmicrosoft.com**.</span></span> <span data-ttu-id="a218a-132">Můžete získat doménu adresáře **domén** svůj adresář.</span><span class="sxs-lookup"><span data-stu-id="a218a-132">The domain of your directory can be obtained from the **Domains** tab of your directory.</span></span>

![Domény][api-management-aad-domains]

<span data-ttu-id="a218a-134">Nakonfigurujte požadovaná nastavení v **změna ověřování** dialogové okno a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="a218a-134">Configure the desired settings in the **Change Authentication** dialog box and click **OK**.</span></span>

![Změna ověřování][api-management-change-authentication]

<span data-ttu-id="a218a-136">Když kliknete na tlačítko **OK** Visual Studio se pokusí o aplikaci zaregistrovat u svého adresáře Azure AD a budete vyzváni k přihlášení pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a218a-136">When you click **OK** Visual Studio will attempt to register your application with your Azure AD directory and you may be prompted to sign in by Visual Studio.</span></span> <span data-ttu-id="a218a-137">Přihlaste se pomocí účtu správce pro váš adresář.</span><span class="sxs-lookup"><span data-stu-id="a218a-137">Sign in using an administrative account for your directory.</span></span>

![Přihlaste se k sadě Visual Studio][api-management-sign-in-vidual-studio]

<span data-ttu-id="a218a-139">Konfigurace tohoto projektu jako webového rozhraní API Azure, zaškrtněte políčko pro **hostitel v cloudu** a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a218a-139">To configure this project as an Azure Web API check the box for **Host in the cloud** and then click **OK**.</span></span>

![Nový projekt][api-management-new-project-cloud]

<span data-ttu-id="a218a-141">Můžete být vyzváni k přihlášení do Azure a pak můžete nakonfigurovat webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a218a-141">You may be prompted to sign in to Azure, and then you can configure the Web App.</span></span>

![Konfigurace][api-management-configure-web-app]

<span data-ttu-id="a218a-143">V tomto příkladu a nové **plán služby App Service** s názvem **APIMAADDemo** je zadán.</span><span class="sxs-lookup"><span data-stu-id="a218a-143">In this example a new **App Service plan** named **APIMAADDemo** is specified.</span></span>

<span data-ttu-id="a218a-144">Klikněte na tlačítko **OK** ke konfiguraci webové aplikace a vytvořte tak projekt.</span><span class="sxs-lookup"><span data-stu-id="a218a-144">Click **OK** to configure the Web App and create the project.</span></span>

## <a name="add-the-code-to-the-web-api-project"></a><span data-ttu-id="a218a-145">Přidejte do projektu webového rozhraní API kód</span><span class="sxs-lookup"><span data-stu-id="a218a-145">Add the code to the Web API project</span></span>
<span data-ttu-id="a218a-146">Dalším krokem při přehrávání videa kód přidá do projektu webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a218a-146">The next step in the video adds the code to the Web API project.</span></span> <span data-ttu-id="a218a-147">Tento krok se spustí při 4:35.</span><span class="sxs-lookup"><span data-stu-id="a218a-147">This step starts at 4:35.</span></span>

<span data-ttu-id="a218a-148">Webové rozhraní API v tomto příkladu implementuje základní kalkulačky služby pomocí modelu a kontroleru.</span><span class="sxs-lookup"><span data-stu-id="a218a-148">The Web API in this example implements a basic calculator service using a model and a controller.</span></span> <span data-ttu-id="a218a-149">Chcete-li přidat model pro službu, klikněte pravým tlačítkem **modely** v **Průzkumníku řešení** a zvolte **přidat**, **třída**.</span><span class="sxs-lookup"><span data-stu-id="a218a-149">To add the model for the service, right-click **Models** in **Solution Explorer** and choose **Add**, **Class**.</span></span> <span data-ttu-id="a218a-150">Název třídy `CalcInput` a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="a218a-150">Name the class `CalcInput` and click **Add**.</span></span>

<span data-ttu-id="a218a-151">Přidejte následující `using` příkaz do horní části `CalcInput.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="a218a-151">Add the following `using` statement to the top of the `CalcInput.cs` file.</span></span>

```c#
using Newtonsoft.Json;
```

<span data-ttu-id="a218a-152">Generovaná třída nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="a218a-152">Replace the generated class with the following code.</span></span>

```c#
public class CalcInput
{
    [JsonProperty(PropertyName = "a")]
    public int a;

    [JsonProperty(PropertyName = "b")]
    public int b;
}
```

<span data-ttu-id="a218a-153">Klikněte pravým tlačítkem na **řadiče** v **Průzkumníku řešení** a zvolte **přidat**->**řadič**.</span><span class="sxs-lookup"><span data-stu-id="a218a-153">Right-click **Controllers** in **Solution Explorer** and choose **Add**->**Controller**.</span></span> <span data-ttu-id="a218a-154">Zvolte **webové rozhraní API 2 řadiče - prázdný** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="a218a-154">Choose **Web API 2 Controller - Empty** and click **Add**.</span></span> <span data-ttu-id="a218a-155">Typ **CalcController** pro řadič název a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="a218a-155">Type **CalcController** for the Controller name and click **Add**.</span></span>

![Přidání Kontroleru][api-management-add-controller]

<span data-ttu-id="a218a-157">Přidejte následující `using` příkaz do horní části `CalcController.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="a218a-157">Add the following `using` statement to the top of the `CalcController.cs` file.</span></span>

```c#
using System.IO;
using System.Web;
using APIMAADDemo.Models;
```

<span data-ttu-id="a218a-158">Třídy generované kontroleru nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="a218a-158">Replace the generated controller class with the following code.</span></span> <span data-ttu-id="a218a-159">Tento kód implementuje `Add`, `Subtract`, `Multiply`, a `Divide` operace rozhraní API základní kalkulačky.</span><span class="sxs-lookup"><span data-stu-id="a218a-159">This code implements the `Add`, `Subtract`, `Multiply`, and `Divide` operations of the Basic Calculator API.</span></span>

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

<span data-ttu-id="a218a-160">Stiskněte klávesu **F6** sestavení a ověřte řešení.</span><span class="sxs-lookup"><span data-stu-id="a218a-160">Press **F6** to build and verify the solution.</span></span>

## <a name="publish-the-project-to-azure"></a><span data-ttu-id="a218a-161">Publikování projektu do Azure</span><span class="sxs-lookup"><span data-stu-id="a218a-161">Publish the project to Azure</span></span>
<span data-ttu-id="a218a-162">V tomto kroku sady Visual Studio publikování projektu do Azure.</span><span class="sxs-lookup"><span data-stu-id="a218a-162">In this step the Visual Studio project is published to Azure.</span></span> <span data-ttu-id="a218a-163">Tento krok videa začíná na 5:45.</span><span class="sxs-lookup"><span data-stu-id="a218a-163">This step of the video starts at 5:45.</span></span>

<span data-ttu-id="a218a-164">K publikování tohoto projektu v Azure, klikněte pravým tlačítkem myši **APIMAADDemo** projektu v sadě Visual Studio a zvolte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="a218a-164">To publish the project to Azure, right-click the **APIMAADDemo** project in Visual Studio and choose **Publish**.</span></span> <span data-ttu-id="a218a-165">Potvrďte výchozí nastavení **Publikovat Web** dialogové okno a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="a218a-165">Keep the default settings in the **Publish Web** dialog box and click **Publish**.</span></span>

![Publikování webu][api-management-web-publish]

## <a name="grant-permissions-to-the-azure-ad-backend-service-application"></a><span data-ttu-id="a218a-167">Udělení oprávnění k back-end aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a218a-167">Grant permissions to the Azure AD backend service application</span></span>
<span data-ttu-id="a218a-168">V adresáři služby Azure AD jako součást procesu konfigurace a publikování projektu webového rozhraní API je vytvořena nová aplikace pro back-end službu.</span><span class="sxs-lookup"><span data-stu-id="a218a-168">A new application for the backend service is created in your Azure AD directory as part of the configuring and publishing process of your Web API project.</span></span> <span data-ttu-id="a218a-169">V tomto kroku videa, od 6:13 jsou udělena oprávnění pro back-end webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a218a-169">In this step of the video, starting at 6:13, permissions are granted to the Web API backend.</span></span>

![Aplikace][api-management-aad-backend-app]

<span data-ttu-id="a218a-171">Klikněte na název aplikace a nakonfigurujte požadovaná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a218a-171">Click the name of the application to configure the required permissions.</span></span> <span data-ttu-id="a218a-172">Přejděte na **konfigurace** kartě a přejděte dolů k položce **oprávnění k ostatním aplikacím** části.</span><span class="sxs-lookup"><span data-stu-id="a218a-172">Navigate to the **Configure** tab and scroll down to the **permissions to other applications** section.</span></span> <span data-ttu-id="a218a-173">Klikněte **oprávnění aplikací** rozevíracího seznamu vedle položky **Windows** **Azure Active Directory**, zaškrtněte políčko pro **čtení dat adresáře**a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a218a-173">Click the **Application Permissions** drop-down beside **Windows** **Azure Active Directory**, check the box for **Read directory data**, and click **Save**.</span></span>

![Přidání oprávnění][api-management-aad-add-permissions]

> [!NOTE]
> <span data-ttu-id="a218a-175">Pokud **Windows** **Azure Active Directory** nejsou uvedené v části oprávnění k ostatním aplikacím, klikněte na tlačítko **přidat aplikaci** a přidejte ji ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="a218a-175">If **Windows** **Azure Active Directory** is not listed under permissions to other applications, click **Add application** and add it from the list.</span></span>
> 
> 

<span data-ttu-id="a218a-176">Poznamenejte si **identifikátor Id URI aplikace** pro použití v následném kroku při aplikaci Azure AD je nakonfigurován pro portál pro vývojáře API Management.</span><span class="sxs-lookup"><span data-stu-id="a218a-176">Make a note of the **App Id URI** for use in a subsequent step when an Azure AD application is configured for the API Management developer portal.</span></span>

![Id URI aplikace][api-management-aad-sso-uri]

## <a name="import-the-web-api-into-api-management"></a><span data-ttu-id="a218a-178">Importovat webové rozhraní API do rozhraní API Management</span><span class="sxs-lookup"><span data-stu-id="a218a-178">Import the Web API into API Management</span></span>
<span data-ttu-id="a218a-179">Rozhraní API se konfigurují na rozhraní API portálu vydavatele, který je přístupný prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a218a-179">APIs are configured from the API publisher portal, which is accessed through the Azure Portal.</span></span> <span data-ttu-id="a218a-180">K dosažení ho, klikněte na tlačítko **portál vydavatele** na panelu nástrojů služby API Management.</span><span class="sxs-lookup"><span data-stu-id="a218a-180">To reach it, click **Publisher portal** from the toolbar of your API Management service.</span></span> <span data-ttu-id="a218a-181">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v [Správa vašeho prvního rozhraní API] [ Manage your first API] kurzu.</span><span class="sxs-lookup"><span data-stu-id="a218a-181">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Manage your first API][Manage your first API] tutorial.</span></span>

![Portál vydavatele][api-management-management-console]

<span data-ttu-id="a218a-183">Operace jde [ručně přidat do rozhraní API](api-management-howto-add-operations.md), nebo může být importován.</span><span class="sxs-lookup"><span data-stu-id="a218a-183">Operations can be [added to APIs manually](api-management-howto-add-operations.md), or they can be imported.</span></span> <span data-ttu-id="a218a-184">V tomto videu se operace importují ve formátu Swagger od 6:40.</span><span class="sxs-lookup"><span data-stu-id="a218a-184">In this video, operations are imported in Swagger format starting at 6:40.</span></span>

<span data-ttu-id="a218a-185">Vytvořte soubor s názvem `calcapi.json` s následující obsah a uložte ho do počítače.</span><span class="sxs-lookup"><span data-stu-id="a218a-185">Create a file named `calcapi.json` with following contents and save it to your computer.</span></span> <span data-ttu-id="a218a-186">Ujistěte se, že `host` atribut body back-end vašeho webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a218a-186">Ensure that the `host` attribute points to your Web API backend.</span></span> <span data-ttu-id="a218a-187">V tomto příkladu `"host": "apimaaddemo.azurewebsites.net"` se používá.</span><span class="sxs-lookup"><span data-stu-id="a218a-187">In this example `"host": "apimaaddemo.azurewebsites.net"` is used.</span></span>

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

<span data-ttu-id="a218a-188">Pokud chcete importovat rozhraní API kalkulačky, klikněte v nabídce **API Management** na levé straně na **Rozhraní API** a potom na **Importovat rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="a218a-188">To import the calculator API, click **APIs** from the **API Management** menu on the left, and then click **Import API**.</span></span>

![Tlačítko Importovat rozhraní API][api-management-import-api]

<span data-ttu-id="a218a-190">Proveďte následující kroky konfigurace rozhraní API kalkulačky.</span><span class="sxs-lookup"><span data-stu-id="a218a-190">Perform the following steps to configure the calculator API.</span></span>

1. <span data-ttu-id="a218a-191">Klikněte na tlačítko **ze souboru**, vyhledejte `calculator.json` soubor uložit a klikněte na tlačítko **Swagger** přepínač.</span><span class="sxs-lookup"><span data-stu-id="a218a-191">Click **From file**, browse to the `calculator.json` file you saved, and click the **Swagger** radio button.</span></span>
2. <span data-ttu-id="a218a-192">Typ **calc** do **přípona adresy URL webového rozhraní API** textové pole.</span><span class="sxs-lookup"><span data-stu-id="a218a-192">Type **calc** into the **Web API URL suffix** textbox.</span></span>
3. <span data-ttu-id="a218a-193">Klikněte do pole **Produkty (volitelné)** a zvolte **Starter**.</span><span class="sxs-lookup"><span data-stu-id="a218a-193">Click in the **Products (optional)** box and choose **Starter**.</span></span>
4. <span data-ttu-id="a218a-194">Kliknutím na **Uložit** naimportujte rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a218a-194">Click **Save** to import the API.</span></span>

![Přidání nového rozhraní API][api-management-import-new-api]

<span data-ttu-id="a218a-196">Po importu rozhraní API se na portálu vydavatele zobrazí souhrnná stránka rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a218a-196">Once the API is imported, the summary page for the API is displayed in the publisher portal.</span></span>

## <a name="call-the-api-unsuccessfully-from-the-developer-portal"></a><span data-ttu-id="a218a-197">Volání rozhraní API neúspěšně z portálu pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="a218a-197">Call the API unsuccessfully from the developer portal</span></span>
<span data-ttu-id="a218a-198">V tomto okamžiku rozhraní API byla naimportována do rozhraní API správy, ale nejde ještě volat úspěšně z portálu pro vývojáře protože službě back-end je chráněn pomocí ověřování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a218a-198">At this point, the API has been imported into API Management, but cannot yet be called successfully from the developer portal because the backend service is protected with Azure AD authentication.</span></span> <span data-ttu-id="a218a-199">Tento postup je znázorněn ve videu od 7:40 pomocí následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="a218a-199">This is demonstrated in the video starting at 7:40 using the following steps.</span></span>

<span data-ttu-id="a218a-200">Klikněte na tlačítko **portál pro vývojáře** z pravé horní části portálu vydavatele.</span><span class="sxs-lookup"><span data-stu-id="a218a-200">Click **Developer portal** from the top-right side of the publisher portal.</span></span>

![Portál pro vývojáře][api-management-developer-portal-menu]

<span data-ttu-id="a218a-202">Klikněte na tlačítko **rozhraní API** a klikněte na **kalkulačky** rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a218a-202">Click **APIs** and click the **Calculator** API.</span></span>

![Portál pro vývojáře][api-management-dev-portal-apis]

<span data-ttu-id="a218a-204">Klikněte na tlačítko **vyzkoušet**.</span><span class="sxs-lookup"><span data-stu-id="a218a-204">Click **Try it**.</span></span>

![Vyzkoušet][api-management-dev-portal-try-it]

<span data-ttu-id="a218a-206">Klikněte na tlačítko **odeslat** a poznamenejte si stav odpovědi **401 – Neověřeno**.</span><span class="sxs-lookup"><span data-stu-id="a218a-206">Click **Send** and note the response status of **401 Unauthorized**.</span></span>

![Odeslat][api-management-dev-portal-send-401]

<span data-ttu-id="a218a-208">Požadavek není autorizovaný, protože rozhraní API back-end je chráněn službou Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a218a-208">The request is unauthorized because the backend API is protected by Azure Active Directory.</span></span> <span data-ttu-id="a218a-209">Před úspěšně voláním rozhraní API vývojář portál musí být nakonfigurované k autorizaci vývojáře, kteří používají OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="a218a-209">Before successfully calling the API the developer portal must be configured to authorize developers using OAuth 2.0.</span></span> <span data-ttu-id="a218a-210">Tento proces je popsán v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="a218a-210">This process is described in the following sections.</span></span>

## <a name="register-the-developer-portal-as-an-aad-application"></a><span data-ttu-id="a218a-211">Zaregistrujte se jako aplikaci AAD portál pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="a218a-211">Register the developer portal as an AAD application</span></span>
<span data-ttu-id="a218a-212">Prvním krokem při konfiguraci portálu pro vývojáře k autorizaci vývojáře, kteří používají OAuth 2.0 je k registraci portál pro vývojáře jako aplikaci AAD.</span><span class="sxs-lookup"><span data-stu-id="a218a-212">The first step in configuring the developer portal to authorize developers using OAuth 2.0 is to register the developer portal as an AAD application.</span></span> <span data-ttu-id="a218a-213">Tento postup je znázorněn od 8:27 na videu.</span><span class="sxs-lookup"><span data-stu-id="a218a-213">This is demonstrated starting at 8:27 in the video.</span></span>

<span data-ttu-id="a218a-214">Přejděte na klienta Azure AD v prvním kroku toto video, v tomto příkladu **APIMDemo** a přejděte do **aplikace** kartě.</span><span class="sxs-lookup"><span data-stu-id="a218a-214">Navigate to the Azure AD tenant from the first step of this video, in this example **APIMDemo** and navigate to the **Applications** tab.</span></span>

![Nová aplikace][api-management-aad-new-application-devportal]

<span data-ttu-id="a218a-216">Klikněte **přidat** tlačítko Vytvořit novou aplikaci Azure Active Directory a vyberte **přidat aplikaci, kterou vyvíjí Moje organizace**.</span><span class="sxs-lookup"><span data-stu-id="a218a-216">Click the **Add** button to create a new Azure Active Directory application, and choose **Add an application my organization is developing**.</span></span>

![Nová aplikace][api-management-new-aad-application-menu]

<span data-ttu-id="a218a-218">Zvolte **webové aplikace nebo webové rozhraní API**, zadejte název a klikněte na šipku Další.</span><span class="sxs-lookup"><span data-stu-id="a218a-218">Choose **Web application and/or Web API**, enter a name, and click the next arrow.</span></span> <span data-ttu-id="a218a-219">V tomto příkladu **APIMDeveloperPortal** se používá.</span><span class="sxs-lookup"><span data-stu-id="a218a-219">In this example **APIMDeveloperPortal** is used.</span></span>

![Nová aplikace][api-management-aad-new-application-devportal-1]

<span data-ttu-id="a218a-221">Pro **přihlašovací adresa URL** zadejte adresu URL služby API Management a připojit `/signin`.</span><span class="sxs-lookup"><span data-stu-id="a218a-221">For **Sign-on URL** enter the URL of your API Management service and append `/signin`.</span></span> <span data-ttu-id="a218a-222">V tomto příkladu `https://contoso5.portal.azure-api.net/signin` se používá.</span><span class="sxs-lookup"><span data-stu-id="a218a-222">In this example `https://contoso5.portal.azure-api.net/signin` is used.</span></span>

<span data-ttu-id="a218a-223">Pro **URL Id aplikace** zadejte adresu URL služby API Management a připojit některé jedinečných znaků.</span><span class="sxs-lookup"><span data-stu-id="a218a-223">For **App Id URL** enter the URL of your API Management service and append some unique characters.</span></span> <span data-ttu-id="a218a-224">To může být jakékoli požadované znaky a v tomto příkladu `https://contoso5.portal.azure-api.net/dp` se používá.</span><span class="sxs-lookup"><span data-stu-id="a218a-224">These can be any desired characters and in this example `https://contoso5.portal.azure-api.net/dp` is used.</span></span> <span data-ttu-id="a218a-225">Pokud požadovaný **vlastností aplikace** jsou nakonfigurovaná, klikněte na tlačítko zaškrtnutí pro vytvoření aplikace.</span><span class="sxs-lookup"><span data-stu-id="a218a-225">When the  desired **App properties** are configured, click the check mark to create the application.</span></span>

![Nová aplikace][api-management-aad-new-application-devportal-2]

## <a name="configure-an-api-management-oauth-20-authorization-server"></a><span data-ttu-id="a218a-227">Konfigurace serveru autorizace OAuth 2.0 rozhraní API Management</span><span class="sxs-lookup"><span data-stu-id="a218a-227">Configure an API Management OAuth 2.0 authorization server</span></span>
<span data-ttu-id="a218a-228">Dalším krokem je konfigurace serveru autorizace OAuth 2.0 ve službě API Management.</span><span class="sxs-lookup"><span data-stu-id="a218a-228">The next step is to configure an OAuth 2.0 authorization server in API Management.</span></span> <span data-ttu-id="a218a-229">Tento krok je znázorněn v videa začínající na 9:43.</span><span class="sxs-lookup"><span data-stu-id="a218a-229">This step is demonstrated in the video starting at 9:43.</span></span>

<span data-ttu-id="a218a-230">Klikněte na tlačítko **zabezpečení** nabídce API Management na levé straně klikněte na **OAuth 2.0**a potom klikněte na **přidat autorizační** serveru.</span><span class="sxs-lookup"><span data-stu-id="a218a-230">Click **Security** from the API Management menu on the left, click **OAuth 2.0**, and then click **Add authorization** server.</span></span>

![Přidání serveru ověřování][api-management-add-authorization-server]

<span data-ttu-id="a218a-232">Zadejte název a volitelný popis v **název** a **popis** pole.</span><span class="sxs-lookup"><span data-stu-id="a218a-232">Enter a name and an optional description in the **Name** and **Description** fields.</span></span> <span data-ttu-id="a218a-233">Tato pole se používají k identifikaci serveru ověřování OAuth 2.0 v rámci instance služby API Management.</span><span class="sxs-lookup"><span data-stu-id="a218a-233">These fields are used to identify the OAuth 2.0 authorization server within the API Management service instance.</span></span> <span data-ttu-id="a218a-234">V tomto příkladu **ukázkový server autorizace** se používá.</span><span class="sxs-lookup"><span data-stu-id="a218a-234">In this example **Authorization server demo** is used.</span></span> <span data-ttu-id="a218a-235">Později při zadávání serveru OAuth 2.0, který se má použít pro ověření pro rozhraní API, vyberete tento název.</span><span class="sxs-lookup"><span data-stu-id="a218a-235">Later when you specify an OAuth 2.0 server to be used for authentication for an API, you will select this name.</span></span>

<span data-ttu-id="a218a-236">Pro **adresa URL stránky registrace klienta** zadejte hodnotu zástupného symbolu, jako `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="a218a-236">For the **Client registration page URL** enter a placeholder value such as `http://localhost`.</span></span>  <span data-ttu-id="a218a-237">**Adresa URL stránky registrace klienta** odkazuje na stránku, uživatelé můžete vytvořit a nakonfigurovat svoje vlastní účty zprostředkovatelů OAuth 2.0, které podporují správu uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="a218a-237">The **Client registration page URL** points to the page that users can use to create and configure their own accounts for OAuth 2.0 providers that support user management of accounts.</span></span> <span data-ttu-id="a218a-238">V tomto příkladu uživatele není vytvořit a nakonfigurovat svoje vlastní účty, tak se používá zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="a218a-238">In this example users do not create and configure their own accounts so a placeholder is used.</span></span>

![Přidání serveru ověřování][api-management-add-authorization-server-1]

<span data-ttu-id="a218a-240">Potom zadejte **adresu URL koncového bodu autorizace** a **adresu URL koncového bodu Token**.</span><span class="sxs-lookup"><span data-stu-id="a218a-240">Next, specify **Authorization endpoint URL** and **Token endpoint URL**.</span></span>

![Autorizace serveru][api-management-add-authorization-server-1a]

<span data-ttu-id="a218a-242">Tyto hodnoty lze získat z **koncových bodů aplikace** stránky AAD aplikace, který jste vytvořili pro portál pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="a218a-242">These values can be retrieved from the **App Endpoints** page of the AAD application you created for the developer portal.</span></span> <span data-ttu-id="a218a-243">Pro přístup koncových bodů, přejděte na **konfigurace** AAD aplikace a klikněte na **zobrazit koncové body**.</span><span class="sxs-lookup"><span data-stu-id="a218a-243">To access the endpoints navigate to the **Configure** tab for the AAD application and click **View endpoints**.</span></span>

![Aplikace][api-management-aad-devportal-application]

![Zobrazit koncové body][api-management-aad-view-endpoints]

<span data-ttu-id="a218a-246">Kopírování **koncový bod autorizace OAuth 2.0** a vložte ji do **adresu URL koncového bodu autorizace** textové pole.</span><span class="sxs-lookup"><span data-stu-id="a218a-246">Copy the **OAuth 2.0 authorization endpoint** and paste it into the **Authorization endpoint URL** textbox.</span></span>

![Přidání serveru ověřování][api-management-add-authorization-server-2]

<span data-ttu-id="a218a-248">Kopírování **koncový bod tokenu OAuth 2.0** a vložte ji do **adresu URL koncového bodu Token** textové pole.</span><span class="sxs-lookup"><span data-stu-id="a218a-248">Copy the **OAuth 2.0 token endpoint** and paste it into the **Token endpoint URL** textbox.</span></span>

![Přidání serveru ověřování][api-management-add-authorization-server-2a]

<span data-ttu-id="a218a-250">Kromě vkládání v koncový bod token, přidáte další text parametr s názvem **prostředků** a používat hodnotu **identifikátor Id URI aplikace** z AAD aplikace pro back-end službu, která byla vytvořena, když byla publikována projekt Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a218a-250">In addition to pasting in the token endpoint, add an additional body parameter named **resource** and for the value use the **App Id URI** from the AAD application for the backend service that was created when the Visual Studio project was published.</span></span>

![Id URI aplikace][api-management-aad-sso-uri]

<span data-ttu-id="a218a-252">Potom zadejte pověření klienta.</span><span class="sxs-lookup"><span data-stu-id="a218a-252">Next, specify the client credentials.</span></span> <span data-ttu-id="a218a-253">Jedná se o pověření pro prostředek, který má přístup, v tomto případě portál pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="a218a-253">These are the credentials for the resource you want to access, in this case the developer portal.</span></span>

![Pověření klienta][api-management-client-credentials]

<span data-ttu-id="a218a-255">Chcete-li získat **Id klienta**, přejděte na **konfigurace** AAD aplikace pro portál pro vývojáře a zkopírujte **Id klienta**.</span><span class="sxs-lookup"><span data-stu-id="a218a-255">To get the **Client Id**, navigate to the **Configure** tab of the AAD application for the developer portal and copy the **Client Id**.</span></span>

<span data-ttu-id="a218a-256">Získat **tajný klíč klienta** klikněte na tlačítko **vyberte dobu trvání** rozevírací seznam v **klíče** části a zadat interval.</span><span class="sxs-lookup"><span data-stu-id="a218a-256">To get the **Client Secret** click the **Select duration** drop-down in the **Keys** section and specify an interval.</span></span> <span data-ttu-id="a218a-257">V tomto příkladu se používá 1 rok.</span><span class="sxs-lookup"><span data-stu-id="a218a-257">In this example 1 year is used.</span></span>

![ID klienta][api-management-aad-client-id]

<span data-ttu-id="a218a-259">Klikněte na tlačítko **Uložit** zobrazí klíč a uložte konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="a218a-259">Click **Save** to save the configuration and display the key.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="a218a-260">Tento klíč si poznamenejte.</span><span class="sxs-lookup"><span data-stu-id="a218a-260">Make a note of this key.</span></span> <span data-ttu-id="a218a-261">Po zavření okna konfigurace Azure Active Directory, klíč nelze zobrazit znovu.</span><span class="sxs-lookup"><span data-stu-id="a218a-261">Once you close the Azure Active Directory configuration window, the key cannot be displayed again.</span></span>
> 
> 

<span data-ttu-id="a218a-262">Klíč zkopírujte do schránky, přepněte zpět na portálu vydavatele, vložte klíč do **tajný klíč klienta** textovému poli a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a218a-262">Copy the key to the clipboard, switch back to the publisher portal, paste the key into the **Client Secret** textbox, and click **Save**.</span></span>

![Přidání serveru ověřování][api-management-add-authorization-server-3]

<span data-ttu-id="a218a-264">Okamžitě následující pověření klienta je udělení autorizačního kódu.</span><span class="sxs-lookup"><span data-stu-id="a218a-264">Immediately following the client credentials is an authorization code grant.</span></span> <span data-ttu-id="a218a-265">Kopírování tento autorizační kód a přepínač zpět do aplikace Azure AD vývojáře portálu konfigurace stránky a vložte udělení autorizace do **adresa URL odpovědi** pole a klikněte na tlačítko **Uložit** znovu.</span><span class="sxs-lookup"><span data-stu-id="a218a-265">Copy this authorization code and switch back to your Azure AD developer portal application configure page, and paste the authorization grant into the **Reply URL** field, and click **Save** again.</span></span>

![Adresa URL odpovědi][api-management-aad-reply-url]

<span data-ttu-id="a218a-267">Dalším krokem je konfigurace oprávnění pro portál pro vývojáře AAD aplikace.</span><span class="sxs-lookup"><span data-stu-id="a218a-267">The next step is to configure the permissions for the developer portal AAD application.</span></span> <span data-ttu-id="a218a-268">Klikněte na tlačítko **oprávnění aplikací** a zaškrtněte políčko pro **čtení dat adresáře**.</span><span class="sxs-lookup"><span data-stu-id="a218a-268">Click **Application Permissions** and check the box for **Read directory data**.</span></span> <span data-ttu-id="a218a-269">Klikněte na tlačítko **Uložit** uložte tuto změnu, a pak klikněte na **přidat aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="a218a-269">Click **Save** to save this change, and then click **Add application**.</span></span>

![Přidání oprávnění][api-management-add-devportal-permissions]

<span data-ttu-id="a218a-271">Klikněte na ikonu hledání typu **APIM** do počáteční s poli, vyberte **APIMAADDemo**a klikněte na políčko pro uložení.</span><span class="sxs-lookup"><span data-stu-id="a218a-271">Click the search icon, type **APIM** into the Starting with box, select **APIMAADDemo**, and click the check mark to save.</span></span>

![Přidání oprávnění][api-management-aad-add-app-permissions]

<span data-ttu-id="a218a-273">Klikněte na tlačítko **delegovaná oprávnění** pro **APIMAADDemo** a zaškrtněte políčko pro **přístup APIMAADDemo**a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a218a-273">Click **Delegated Permissions** for **APIMAADDemo** and check the box for **Access APIMAADDemo**, and click **Save**.</span></span> <span data-ttu-id="a218a-274">To umožňuje vývojáři aplikace portálu přístup ke službě back-end.</span><span class="sxs-lookup"><span data-stu-id="a218a-274">This allows the developer portal application to access the backend service.</span></span>

![Přidání oprávnění][api-management-aad-add-delegated-permissions]

## <a name="enable-oauth-20-user-authorization-for-the-calculator-api"></a><span data-ttu-id="a218a-276">Povolení autorizace uživatelů OAuth 2.0 pro rozhraní API kalkulačky</span><span class="sxs-lookup"><span data-stu-id="a218a-276">Enable OAuth 2.0 user authorization for the Calculator API</span></span>
<span data-ttu-id="a218a-277">Teď, když server OAuth 2.0 je nakonfigurovaný, můžete v nastavení zabezpečení pro vaše rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a218a-277">Now that the OAuth 2.0 server is configured, you can specify it in the security settings for your API.</span></span> <span data-ttu-id="a218a-278">Tento krok je znázorněn v videa začínající na 14:30.</span><span class="sxs-lookup"><span data-stu-id="a218a-278">This step is demonstrated in the video starting at 14:30.</span></span>

<span data-ttu-id="a218a-279">Klikněte na tlačítko **rozhraní API** v levé nabídce a klikněte na **kalkulačky** můžete zobrazit a konfigurovat jeho nastavení.</span><span class="sxs-lookup"><span data-stu-id="a218a-279">Click **APIs** in the left menu, and click  **Calculator** to view and configure its settings.</span></span>

![API kalkulačky][api-management-calc-api]

<span data-ttu-id="a218a-281">Přejděte na **zabezpečení** zaškrtněte políčko **OAuth 2.0** zaškrtávací políčko, vyberte požadované autorizace server z **serveru ověřování** rozevíracího seznamu a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a218a-281">Navigate to the **Security** tab, check the **OAuth 2.0** checkbox, select the desired authorization server from the **Authorization server** drop-down, and click **Save**.</span></span>

![API kalkulačky][api-management-enable-aad-calculator]

## <a name="successfully-call-the-calculator-api-from-the-developer-portal"></a><span data-ttu-id="a218a-283">Rozhraní API kalkulačky úspěšně volejte z portálu pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="a218a-283">Successfully call the Calculator API from the developer portal</span></span>
<span data-ttu-id="a218a-284">Teď, když autorizace OAuth 2.0 je nakonfigurovaná na volání rozhraní API, můžete jeho operace úspěšně volat z středisku pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="a218a-284">Now that the OAuth 2.0 authorization is configured on the API, its operations can be successfully called from the developer center.</span></span> <span data-ttu-id="a218a-285">Tento krok je znázorněn v videa začínající na 15:00.</span><span class="sxs-lookup"><span data-stu-id="a218a-285">THis step is demonstrated in the video starting at 15:00.</span></span>

<span data-ttu-id="a218a-286">Přejděte zpět **přidat dvě celá čísla** operaci kalkulačky služby v portálu pro vývojáře a klikněte na **vyzkoušet**.</span><span class="sxs-lookup"><span data-stu-id="a218a-286">Navigate back to the **Add two integers** operation of the calculator service in the developer portal and click **Try it**.</span></span> <span data-ttu-id="a218a-287">Poznámka: novou položku **autorizace** části odpovídající serveru ověřování, který jste právě přidali.</span><span class="sxs-lookup"><span data-stu-id="a218a-287">Note the new item in the **Authorization** section corresponding to the authorization server you just added.</span></span>

![API kalkulačky][api-management-calc-authorization-server]

<span data-ttu-id="a218a-289">Vyberte **autorizační kód** z autorizace rozevíracího seznamu a zadejte přihlašovací údaje pro účet, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="a218a-289">Select **Authorization code** from the authorization drop-down list and enter the credentials of the account to use.</span></span> <span data-ttu-id="a218a-290">Pokud jste již přihlášeni pomocí účtu, nemusí se zobrazí výzva.</span><span class="sxs-lookup"><span data-stu-id="a218a-290">If you are already signed in with the account you may not be prompted.</span></span>

![API kalkulačky][api-management-devportal-authorization-code]

<span data-ttu-id="a218a-292">Klikněte na tlačítko **odeslat** a poznamenejte si **stav odpovědi** z **200 OK** a výsledky operace v obsahu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="a218a-292">Click **Send** and note the **Response status** of **200 OK** and the results of the operation in the response content.</span></span>

![API kalkulačky][api-management-devportal-response]

## <a name="configure-a-desktop-application-to-call-the-api"></a><span data-ttu-id="a218a-294">Konfigurace aplikace pro volání rozhraní API</span><span class="sxs-lookup"><span data-stu-id="a218a-294">Configure a desktop application to call the API</span></span>
<span data-ttu-id="a218a-295">Následující postup ve videu začíná na 16:30 a nakonfiguruje jednoduché aplikace pracovní plochy pro volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a218a-295">The next procedure in the video starts at 16:30 and configures a simple desktop application to call the API.</span></span> <span data-ttu-id="a218a-296">Prvním krokem je registrace klientů aplikace ve službě Azure AD a poskytněte přístup k adresáři a ke službě back-end.</span><span class="sxs-lookup"><span data-stu-id="a218a-296">The first step is to register the desktop application in Azure AD and give it access to the directory and to the backend service.</span></span> <span data-ttu-id="a218a-297">Na 18:25 je ukázka aplikace pracovní plochy volání operace na rozhraní API kalkulačky.</span><span class="sxs-lookup"><span data-stu-id="a218a-297">At 18:25 there is a demonstration of the desktop application calling an operation on the calculator API.</span></span>

## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a><span data-ttu-id="a218a-298">Konfigurace zásad ověřování JWT předem autorizovat požadavků</span><span class="sxs-lookup"><span data-stu-id="a218a-298">Configure a JWT validation policy to pre-authorize requests</span></span>
<span data-ttu-id="a218a-299">Poslední postup ve videu začíná na 20:48 a ukazuje, jak používat [ověření JWT](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) zásad předem autorizovat požadavky ověřením přístupových tokenů každého příchozího požadavku.</span><span class="sxs-lookup"><span data-stu-id="a218a-299">The final procedure in the video starts at 20:48 and shows you how to use the [Validate JWT](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) policy to pre-authorize requests by validating the access tokens of each incoming request.</span></span> <span data-ttu-id="a218a-300">Pokud požadavek není ověřen pomocí zásad ověřování tokenů JWT, žádost je blokována API Management a není předají back-end.</span><span class="sxs-lookup"><span data-stu-id="a218a-300">If the request is not validated by the Validate JWT policy, the request is blocked by API Management and is not passed along to the backend.</span></span>

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

<span data-ttu-id="a218a-301">Jiné předvedení konfiguraci a použití této zásady, najdete v části [cloudu zahrnují díl 177: víc funkcí správy rozhraní API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a rychlé převíjení vpřed na 13:50.</span><span class="sxs-lookup"><span data-stu-id="a218a-301">For another demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 13:50.</span></span> <span data-ttu-id="a218a-302">Rychlé převinutí vpřed do 15:00 v tématu Zásady nakonfigurované v editoru zásad a potom do 18:50 pro předvedení volání operace z portálu pro vývojáře s i bez požadované autorizační token.</span><span class="sxs-lookup"><span data-stu-id="a218a-302">Fast forward to 15:00 to see the policies configured in the policy editor and then to 18:50 for a demonstration of calling an operation from the developer portal both with and without the required authorization token.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a218a-303">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a218a-303">Next steps</span></span>
* <span data-ttu-id="a218a-304">Podívejte se na další [videa](https://azure.microsoft.com/documentation/videos/index/?services=api-management) o službě API Management.</span><span class="sxs-lookup"><span data-stu-id="a218a-304">Check out more [videos](https://azure.microsoft.com/documentation/videos/index/?services=api-management) about API Management.</span></span>
* <span data-ttu-id="a218a-305">Další způsoby zabezpečení back-end službu, naleznete v části [vzájemného ověření certifikátů](api-management-howto-mutual-certificates.md).</span><span class="sxs-lookup"><span data-stu-id="a218a-305">For other ways to secure your backend service, see [Mutual Certificate authentication](api-management-howto-mutual-certificates.md).</span></span>

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
