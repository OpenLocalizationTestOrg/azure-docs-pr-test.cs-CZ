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
ms.openlocfilehash: eaa41805c92212154ee8d51d3eb3aee1202eef1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="add-hello-applications-registration-information-tooyour-app"></a>Přidání aplikace hello registrační informace tooyour aplikace

V tomto kroku je nutné tooadd hello ID klienta tooyour projektu.

1.  Otevřete `MainActivity` (v části `app`  >  `java`  >   *`{host}.{namespace}`* )
2.  Nahraďte hello řádku počínaje `final static String CLIENT_ID` se:
```java
final static String CLIENT_ID = "[Enter hello application Id here]";
```
3. Otevřete:`app` > `manifests` > `AndroidManifest.xml`
4. Přidejte následující aktivitu příliš hello`manifest\application` uzlu. Tato registrace `BrowserTabActivity` tooallow hello OS tooresume aplikaci po dokončení ověření hello:

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

### <a name="what-is-next"></a>Co je další

[Testování a ověření](active-directory-mobileanddesktopapp-android-test.md)
