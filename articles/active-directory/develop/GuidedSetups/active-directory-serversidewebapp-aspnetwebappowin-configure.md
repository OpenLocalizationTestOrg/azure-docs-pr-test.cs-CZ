---
title: "aaaAzure AD v2 ASP.NET Web Server Začínáme - Config | Microsoft Docs"
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
ms.openlocfilehash: e666be4622ad30aaa1e12e49ae56bbe1e129b2a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="create-an-application-express"></a><span data-ttu-id="dd5ad-103">Vytvoření aplikace (Express)</span><span class="sxs-lookup"><span data-stu-id="dd5ad-103">Create an application (Express)</span></span>
<span data-ttu-id="dd5ad-104">Nyní je třeba tooregister svoji aplikaci v hello *portálu pro registraci aplikace Microsoft*:</span><span class="sxs-lookup"><span data-stu-id="dd5ad-104">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="dd5ad-105">Registrace vaší aplikace prostřednictvím hello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)</span><span class="sxs-lookup"><span data-stu-id="dd5ad-105">Register your application via hello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)</span></span>
2.  <span data-ttu-id="dd5ad-106">Zadejte název vaší aplikace a e-mailu</span><span class="sxs-lookup"><span data-stu-id="dd5ad-106">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="dd5ad-107">Ujistěte se, že je zaškrtnuté políčko hello pro instalaci na základě</span><span class="sxs-lookup"><span data-stu-id="dd5ad-107">Make sure hello option for Guided Setup is checked</span></span>
4.  <span data-ttu-id="dd5ad-108">Postupujte podle pokynů tooadd hello k aplikaci tooyour adresy URL pro přesměrování</span><span class="sxs-lookup"><span data-stu-id="dd5ad-108">Follow hello instructions tooadd a Redirect URL tooyour application</span></span>

## <a name="add-your-application-registration-information-tooyour-solution-advanced"></a><span data-ttu-id="dd5ad-109">Přidat řešení aplikace registrační informace tooyour (Upřesnit)</span><span class="sxs-lookup"><span data-stu-id="dd5ad-109">Add your application registration information tooyour solution (Advanced)</span></span>
<span data-ttu-id="dd5ad-110">Nyní je třeba tooregister svoji aplikaci v hello *portálu pro registraci aplikace Microsoft*:</span><span class="sxs-lookup"><span data-stu-id="dd5ad-110">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="dd5ad-111">Přejděte toohello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app) tooregister aplikace</span><span class="sxs-lookup"><span data-stu-id="dd5ad-111">Go toohello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app) tooregister an application</span></span>
2. <span data-ttu-id="dd5ad-112">Zadejte název vaší aplikace a e-mailu</span><span class="sxs-lookup"><span data-stu-id="dd5ad-112">Enter a name for your application and your email</span></span> 
3.  <span data-ttu-id="dd5ad-113">Ujistěte se, že není zaškrtnuto políčko hello pro instalaci na základě</span><span class="sxs-lookup"><span data-stu-id="dd5ad-113">Make sure hello option for Guided Setup is unchecked</span></span>
4.  <span data-ttu-id="dd5ad-114">Klikněte na tlačítko `Add Platform`, zvolte položku`Web`</span><span class="sxs-lookup"><span data-stu-id="dd5ad-114">Click `Add Platform`, then select `Web`</span></span>
5.  <span data-ttu-id="dd5ad-115">Přejděte zpět tooVisual Studio, vyberte hello projekt v Průzkumníku řešení klikněte a podívejte se na vlastnosti – okno hello (Pokud se nezobrazí okno Vlastnosti stisknutím klávesy F4)</span><span class="sxs-lookup"><span data-stu-id="dd5ad-115">Go back tooVisual Studio and, in Solution Explorer, select hello project and look at hello Properties window (if you don’t see a Properties window, press F4)</span></span>
6.  <span data-ttu-id="dd5ad-116">Příliš změnit povolen protokol SSL`True`</span><span class="sxs-lookup"><span data-stu-id="dd5ad-116">Change SSL Enabled too`True`</span></span>
7.  <span data-ttu-id="dd5ad-117">Zkopírujte hello adresy URL protokolu SSL a přidejte tuto adresu URL toohello seznam adres URL pro přesměrování v portálu registrace hello seznam adres URL pro přesměrování:</span><span class="sxs-lookup"><span data-stu-id="dd5ad-117">Copy hello SSL URL and add this URL toohello list of Redirect URLs in hello Registration Portal’s list of Redirect URLs:</span></span><br/><br/>![Vlastnosti projektu](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)<br />
8.  <span data-ttu-id="dd5ad-119">Přidejte následující hello `web.config` umístěné v kořenové složce hello části hello `configuration\appSettings`:</span><span class="sxs-lookup"><span data-stu-id="dd5ad-119">Add hello following in `web.config` located in hello root folder under hello section `configuration\appSettings`:</span></span>

```xml
<add key="ClientId" value="Enter_the_Application_Id_here" />
<add key="redirectUri" value="Enter_the_Redirect_URL_here" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
<!-- Workaround for Docs conversion bug -->
<ol start="9">
<li>
<span data-ttu-id="dd5ad-120">Nahraďte `ClientId` s hello Id aplikace, které jste právě zaregistrovali</span><span class="sxs-lookup"><span data-stu-id="dd5ad-120">Replace `ClientId` with hello Application Id you just registered</span></span>
</li>
<li>
<span data-ttu-id="dd5ad-121">Nahraďte `redirectUri` s hello SSL URL projektu</span><span class="sxs-lookup"><span data-stu-id="dd5ad-121">Replace `redirectUri` with hello SSL URL of your project</span></span>
</li>
</ol>
<!-- End Docs -->
