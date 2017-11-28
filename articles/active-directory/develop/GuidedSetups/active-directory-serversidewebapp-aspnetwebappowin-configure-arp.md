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
ms.openlocfilehash: 8a1650a65e7980f4a13fa4edc7918b0099bb5464
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
## <a name="configure-your-aspnet-web-app-with-the-applications-registration-information"></a><span data-ttu-id="18d26-103">Konfigurace webové aplikace ASP.NET s informace o registraci aplikace</span><span class="sxs-lookup"><span data-stu-id="18d26-103">Configure your ASP.NET Web App with the application's registration information</span></span>

<span data-ttu-id="18d26-104">V tomto kroku nakonfigurujete svůj projekt na používání protokolu SSL a pak použijte adresu URL SSL konfigurovat informace o registraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="18d26-104">In this step, you will configure your project to use SSL, and then use the SSL URL to configure your application’s registration information.</span></span> <span data-ttu-id="18d26-105">Potom tuto aplikaci přidat, informace o registraci do řešení prostřednictvím *web.config*.</span><span class="sxs-lookup"><span data-stu-id="18d26-105">After this, add the application’ registration information to your solution via *web.config*.</span></span>

1.  <span data-ttu-id="18d26-106">V Průzkumníku řešení, vyberte projekt a podívejte se na `Properties` okno (Pokud se nezobrazí okno Vlastnosti stisknutím klávesy F4)</span><span class="sxs-lookup"><span data-stu-id="18d26-106">In Solution Explorer, select the project and look at the `Properties` window (if you don’t see a Properties window, press F4)</span></span>
2.  <span data-ttu-id="18d26-107">Změna `SSL Enabled` na`True`</span><span class="sxs-lookup"><span data-stu-id="18d26-107">Change `SSL Enabled` to `True`</span></span>
3.  <span data-ttu-id="18d26-108">Zkopírujte hodnotu z `SSL URL` výše a vložte jej do `Redirect URL` pole nahoře na této stránce a pak klikněte na *aktualizace*:</span><span class="sxs-lookup"><span data-stu-id="18d26-108">Copy the value from `SSL URL` above and paste it in the `Redirect URL` field on the top of this page, then click *Update*:</span></span><br/><br/><span data-ttu-id="18d26-109">![Vlastnosti projektu](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)</span><span class="sxs-lookup"><span data-stu-id="18d26-109">![Project properties](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)</span></span><br />
4.  <span data-ttu-id="18d26-110">Přidejte následující v `web.config` soubor umístěný na kořenové složky, části `configuration\appSettings`:</span><span class="sxs-lookup"><span data-stu-id="18d26-110">Add the following in `web.config` file located in root’s folder, under section `configuration\appSettings`:</span></span>

```xml
<add key="ClientId" value="[Enter the application Id here]" />
<add key="RedirectUri" value="[Enter the Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
