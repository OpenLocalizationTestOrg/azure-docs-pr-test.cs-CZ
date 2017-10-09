---
title: "aaaAzure AD v2 Windows Desktop Začínáme - Config | Microsoft Docs"
description: "Aplikace Windows Desktop .NET (XAML) jak získat přístupový token a volat rozhraní API chráněné službou Azure Active Directory v2 koncový bod. | Microsoft Azure | Microsoft Azure"
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
ms.openlocfilehash: 39c257e3e0cb09491f6fe005877601bd46824d12
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="create-an-application-express"></a>Vytvoření aplikace (Express)
Nyní je třeba tooregister svoji aplikaci v hello *portálu pro registraci aplikace Microsoft*:
1. Registrace vaší aplikace prostřednictvím hello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure)
2.  Zadejte název vaší aplikace a e-mailu
3.  Ujistěte se, že je zaškrtnuté políčko hello pro instalaci na základě
4.  Postupujte podle ID aplikace hello pokyny tooobtain hello a vložte jej do vašeho kódu

### <a name="add-your-application-registration-information-tooyour-solution-advanced"></a>Přidat řešení aplikace registrační informace tooyour (Upřesnit)
Nyní je třeba tooregister svoji aplikaci v hello *portálu pro registraci aplikace Microsoft*:
1. Přejděte toohello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app) tooregister aplikace
2. Zadejte název vaší aplikace a e-mailu 
3. Ujistěte se, že není zaškrtnuto políčko hello pro instalaci na základě
4. Klikněte na tlačítko `Add Platform`, zvolte položku `Native Application` a klikněte na tlačítko Uložit
5. Zkopírujte hello identifikátor GUID v ID aplikace, přejděte zpět tooVisual Studio otevřete `App.xaml.cs` a nahraďte `your_client_id_here` s hello ID aplikace, které jste právě zaregistrovali:

```csharp
private static string ClientId = "your_application_id_here";
```
