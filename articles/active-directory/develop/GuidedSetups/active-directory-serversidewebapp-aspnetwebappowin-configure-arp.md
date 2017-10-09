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
## <a name="configure-your-aspnet-web-app-with-hello-applications-registration-information"></a>Konfigurace webové aplikace ASP.NET s informace o registraci aplikace hello

V tomto kroku konfigurace vašeho projektu toouse SSL a pak použít hello SSL URL tooconfigure informace o registraci vaší aplikace. Po této, přidání aplikace hello' registrační informace tooyour řešení prostřednictvím *web.config*.

1.  V Průzkumníku řešení, vyberte hello projekt a podívejte se na hello `Properties` okno (Pokud se nezobrazí okno Vlastnosti stisknutím klávesy F4)
2.  Změna `SSL Enabled` příliš`True`
3.  Zkopírujte hodnotu hello z `SSL URL` výše a vložte ji do hello `Redirect URL` pole na hello horní části této stránky a pak klikněte na *aktualizace*:<br/><br/>![Vlastnosti projektu](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)<br />
4.  Přidejte následující hello `web.config` soubor umístěný na kořenové složky, části `configuration\appSettings`:

```xml
<add key="ClientId" value="[Enter hello application Id here]" />
<add key="RedirectUri" value="[Enter hello Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
