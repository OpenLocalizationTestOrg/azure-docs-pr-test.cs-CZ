---
title: "Azure AD v2 ASP.NET Web Server získávání spuštěno – konfigurace | Microsoft Docs"
description: "Implementace přihlašování společnosti Microsoft na řešení technologie ASP.NET s tradiční webovou aplikací využívajících prohlížeč pomocí OpenID Connect standard"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 0c627802ccfba230dcde2dafffee26cb1c895791
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
## <a name="create-an-application-express"></a><span data-ttu-id="4683d-103">Vytvoření aplikace (Express)</span><span class="sxs-lookup"><span data-stu-id="4683d-103">Create an application (Express)</span></span>
<span data-ttu-id="4683d-104">Nyní je nutné zaregistrovat aplikaci v *portálu pro registraci aplikace Microsoft*:</span><span class="sxs-lookup"><span data-stu-id="4683d-104">Now you need to register your application in the *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="4683d-105">Registrace vaší aplikace pomocí [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)</span><span class="sxs-lookup"><span data-stu-id="4683d-105">Register your application via the [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)</span></span>
2.  <span data-ttu-id="4683d-106">Zadejte název vaší aplikace a e-mailu</span><span class="sxs-lookup"><span data-stu-id="4683d-106">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="4683d-107">Ujistěte se, že je zaškrtnuté políčko pro instalaci na základě</span><span class="sxs-lookup"><span data-stu-id="4683d-107">Make sure the option for Guided Setup is checked</span></span>
4.  <span data-ttu-id="4683d-108">Postupujte podle pokynů k přidání do aplikace adresy URL pro přesměrování</span><span class="sxs-lookup"><span data-stu-id="4683d-108">Follow the instructions to add a Redirect URL to your application</span></span>

## <a name="add-your-application-registration-information-to-your-solution-advanced"></a><span data-ttu-id="4683d-109">Přidat informace o registraci aplikace k řešení (Upřesnit)</span><span class="sxs-lookup"><span data-stu-id="4683d-109">Add your application registration information to your solution (Advanced)</span></span>
<span data-ttu-id="4683d-110">Nyní je nutné zaregistrovat aplikaci v *portálu pro registraci aplikace Microsoft*:</span><span class="sxs-lookup"><span data-stu-id="4683d-110">Now you need to register your application in the *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="4683d-111">Přejděte na [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app) zaregistrovat aplikaci</span><span class="sxs-lookup"><span data-stu-id="4683d-111">Go to the [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app) to register an application</span></span>
2. <span data-ttu-id="4683d-112">Zadejte název vaší aplikace a e-mailu</span><span class="sxs-lookup"><span data-stu-id="4683d-112">Enter a name for your application and your email</span></span> 
3.  <span data-ttu-id="4683d-113">Ujistěte se, že není zaškrtnuto políčko pro instalaci na základě</span><span class="sxs-lookup"><span data-stu-id="4683d-113">Make sure the option for Guided Setup is unchecked</span></span>
4.  <span data-ttu-id="4683d-114">Klikněte na tlačítko `Add Platform`, zvolte položku`Web`</span><span class="sxs-lookup"><span data-stu-id="4683d-114">Click `Add Platform`, then select `Web`</span></span>
5.  <span data-ttu-id="4683d-115">Přejděte zpět do Visual Studio a v Průzkumníku řešení vyberte projekt a podívejte se na okno vlastností (Pokud se nezobrazí okno Vlastnosti stisknutím klávesy F4)</span><span class="sxs-lookup"><span data-stu-id="4683d-115">Go back to Visual Studio and, in Solution Explorer, select the project and look at the Properties window (if you don’t see a Properties window, press F4)</span></span>
6.  <span data-ttu-id="4683d-116">Změna SSL povoleno`True`</span><span class="sxs-lookup"><span data-stu-id="4683d-116">Change SSL Enabled to `True`</span></span>
7.  <span data-ttu-id="4683d-117">Zkopírujte adresu URL protokolu SSL a přidejte tuto adresu URL do seznamu adres URL pro přesměrování v portálu pro registraci seznam adres URL pro přesměrování:</span><span class="sxs-lookup"><span data-stu-id="4683d-117">Copy the SSL URL and add this URL to the list of Redirect URLs in the Registration Portal’s list of Redirect URLs:</span></span><br/><br/>![Vlastnosti projektu](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)<br />
8.  <span data-ttu-id="4683d-119">Přidejte následující v `web.config` umístěné v kořenové složce části `configuration\appSettings`:</span><span class="sxs-lookup"><span data-stu-id="4683d-119">Add the following in `web.config` located in the root folder under the section `configuration\appSettings`:</span></span>

```xml
<add key="ClientId" value="Enter_the_Application_Id_here" />
<add key="redirectUri" value="Enter_the_Redirect_URL_here" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
<!-- Workaround for Docs conversion bug -->
<ol start="9">
<li>
<span data-ttu-id="4683d-120">Nahraďte `ClientId` s Id aplikace, který jste právě zaregistrovali</span><span class="sxs-lookup"><span data-stu-id="4683d-120">Replace `ClientId` with the Application Id you just registered</span></span>
</li>
<li>
<span data-ttu-id="4683d-121">Nahraďte `redirectUri` pomocí adresy URL protokolu SSL vašeho projektu</span><span class="sxs-lookup"><span data-stu-id="4683d-121">Replace `redirectUri` with the SSL URL of your project</span></span>
</li>
</ol>
<!-- End Docs -->
