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
## <a name="create-an-application-express"></a>Vytvoření aplikace (Express)
Nyní je třeba tooregister svoji aplikaci v hello *portálu pro registraci aplikace Microsoft*:
1. Registrace vaší aplikace prostřednictvím hello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)
2.  Zadejte název vaší aplikace a e-mailu
3.  Ujistěte se, že je zaškrtnuté políčko hello pro instalaci na základě
4.  Postupujte podle pokynů tooadd hello k aplikaci tooyour adresy URL pro přesměrování

## <a name="add-your-application-registration-information-tooyour-solution-advanced"></a>Přidat řešení aplikace registrační informace tooyour (Upřesnit)
Nyní je třeba tooregister svoji aplikaci v hello *portálu pro registraci aplikace Microsoft*:
1. Přejděte toohello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app) tooregister aplikace
2. Zadejte název vaší aplikace a e-mailu 
3.  Ujistěte se, že není zaškrtnuto políčko hello pro instalaci na základě
4.  Klikněte na tlačítko `Add Platform`, zvolte položku`Web`
5.  Přejděte zpět tooVisual Studio, vyberte hello projekt v Průzkumníku řešení klikněte a podívejte se na vlastnosti – okno hello (Pokud se nezobrazí okno Vlastnosti stisknutím klávesy F4)
6.  Příliš změnit povolen protokol SSL`True`
7.  Zkopírujte hello adresy URL protokolu SSL a přidejte tuto adresu URL toohello seznam adres URL pro přesměrování v portálu registrace hello seznam adres URL pro přesměrování:<br/><br/>![Vlastnosti projektu](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)<br />
8.  Přidejte následující hello `web.config` umístěné v kořenové složce hello části hello `configuration\appSettings`:

```xml
<add key="ClientId" value="Enter_the_Application_Id_here" />
<add key="redirectUri" value="Enter_the_Redirect_URL_here" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
<!-- Workaround for Docs conversion bug -->
<ol start="9">
<li>
Nahraďte `ClientId` s hello Id aplikace, které jste právě zaregistrovali
</li>
<li>
Nahraďte `redirectUri` s hello SSL URL projektu
</li>
</ol>
<!-- End Docs -->
