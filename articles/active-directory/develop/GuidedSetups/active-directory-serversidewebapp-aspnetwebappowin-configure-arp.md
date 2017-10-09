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
ms.openlocfilehash: badc47e131290a56a507592f944a0fc7093260a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="configure-your-aspnet-web-app-with-hello-applications-registration-information"></a><span data-ttu-id="09bb2-103">Konfigurace webové aplikace ASP.NET s informace o registraci aplikace hello</span><span class="sxs-lookup"><span data-stu-id="09bb2-103">Configure your ASP.NET Web App with hello application's registration information</span></span>

<span data-ttu-id="09bb2-104">V tomto kroku konfigurace vašeho projektu toouse SSL a pak použít hello SSL URL tooconfigure informace o registraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="09bb2-104">In this step, you will configure your project toouse SSL, and then use hello SSL URL tooconfigure your application’s registration information.</span></span> <span data-ttu-id="09bb2-105">Po této, přidání aplikace hello' registrační informace tooyour řešení prostřednictvím *web.config*.</span><span class="sxs-lookup"><span data-stu-id="09bb2-105">After this, add hello application’ registration information tooyour solution via *web.config*.</span></span>

1.  <span data-ttu-id="09bb2-106">V Průzkumníku řešení, vyberte hello projekt a podívejte se na hello `Properties` okno (Pokud se nezobrazí okno Vlastnosti stisknutím klávesy F4)</span><span class="sxs-lookup"><span data-stu-id="09bb2-106">In Solution Explorer, select hello project and look at hello `Properties` window (if you don’t see a Properties window, press F4)</span></span>
2.  <span data-ttu-id="09bb2-107">Změna `SSL Enabled` příliš`True`</span><span class="sxs-lookup"><span data-stu-id="09bb2-107">Change `SSL Enabled` too`True`</span></span>
3.  <span data-ttu-id="09bb2-108">Zkopírujte hodnotu hello z `SSL URL` výše a vložte ji do hello `Redirect URL` pole na hello horní části této stránky a pak klikněte na *aktualizace*:</span><span class="sxs-lookup"><span data-stu-id="09bb2-108">Copy hello value from `SSL URL` above and paste it in hello `Redirect URL` field on hello top of this page, then click *Update*:</span></span><br/><br/><span data-ttu-id="09bb2-109">![Vlastnosti projektu](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)</span><span class="sxs-lookup"><span data-stu-id="09bb2-109">![Project properties](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)</span></span><br />
4.  <span data-ttu-id="09bb2-110">Přidejte následující hello `web.config` soubor umístěný na kořenové složky, části `configuration\appSettings`:</span><span class="sxs-lookup"><span data-stu-id="09bb2-110">Add hello following in `web.config` file located in root’s folder, under section `configuration\appSettings`:</span></span>

```xml
<add key="ClientId" value="[Enter hello application Id here]" />
<add key="RedirectUri" value="[Enter hello Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
