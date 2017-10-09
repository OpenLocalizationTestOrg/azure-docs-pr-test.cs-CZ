---
title: "aaaAzure AD v2 Android Začínáme – konfigurace | Microsoft Docs"
description: "Jak získat přístupový token a volání rozhraní API, které vyžadují přístupové tokeny z koncového bodu Azure Active Directory v2 nebo Microsoft Graph API aplikace pro Android"
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
ms.openlocfilehash: e14796c37ab0c30d948b6f783dac80059375afa3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="create-an-application-express"></a>Vytvoření aplikace (Express)
Nyní je třeba tooregister svoji aplikaci v hello *portálu pro registraci aplikace Microsoft*:
1. Registrace vaší aplikace prostřednictvím hello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure)
2.  Zadejte název vaší aplikace a e-mailu
3.  Ujistěte se, že je zaškrtnuté políčko hello pro instalaci na základě
4.  Postupujte podle ID aplikace hello pokyny tooobtain hello a vložte jej do vašeho kódu

### <a name="add-your-application-registration-information-tooyour-solution-advanced"></a>Přidat řešení aplikace registrační informace tooyour (Upřesnit)
Nyní je třeba tooregister svoji aplikaci v hello *portálu pro registraci aplikace Microsoft*:
1. Přejděte toohello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app) tooregister aplikace
2. Zadejte název vaší aplikace a e-mailu 
3. Ujistěte se, že není zaškrtnuto políčko hello pro instalaci na základě
4. Klikněte na tlačítko `Add Platform`, zvolte položku `Native Application` a klikněte na tlačítko Uložit
5.  Otevřete `MainActivity` (v části `app`  >  `java`  >   *`{host}.{namespace}`* )
6.  Nahraďte hello *[Zadejte sem hello aplikace Id]* v řádku hello počínaje `final static String CLIENT_ID` s ID aplikace hello jste právě zaregistrovali:

```java
final static String CLIENT_ID = "[Enter hello application Id here]";
```
<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
Otevřete `AndroidManifest.xml` (v části `app`  >  `manifests`) hello přidat následující aktivitu příliš`manifest\application` uzlu. Takovém postupu zaregistruje `BrowserTabActivity` tooallow hello OS tooresume aplikaci po dokončení ověření hello:
</li>
</ol>

```xml
<!--Intent filter toocapture System Browser calling back tooour app after Sign In-->
<activity
    android:name="com.microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        
        <!--Add in your scheme/host from registered redirect URI-->
        <!--By default, hello scheme should be similar too'msal[appId]' -->
        <data android:scheme="msal[Enter hello application Id here]"
            android:host="auth" />
    </intent-filter>
</activity>
```
<!-- Workaround for Docs conversion bug -->
<ol start="8">
<li>
V hello `BrowserTabActivity`, nahraďte `[Enter hello application Id here]` s ID hello aplikace.
</li>
</ol>
