---
title: "Začínáme aaaAzure AD v2 iOS – konfigurace | Microsoft Docs"
description: "Jak aplikace pro iOS (Swift) můžete volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: 537cc7f0de6cd947fe340566c9e93f8bb08d57a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="create-an-application-express"></a>Vytvoření aplikace (Express)
Nyní je třeba tooregister svoji aplikaci v hello *portálu pro registraci aplikace Microsoft*:
1. Registrace vaší aplikace prostřednictvím hello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=ios&step=configure)
2.  Zadejte název vaší aplikace a e-mailu
3.  Ujistěte se, že je zaškrtnuté políčko hello pro instalaci na základě
4.  Postupujte podle ID aplikace hello pokyny tooobtain hello a vložte jej do vašeho kódu

### <a name="add-your-application-registration-information-tooyour-solution-advanced"></a>Přidat řešení aplikace registrační informace tooyour (Upřesnit)

1.  Přejděte příliš[portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app)
2.  Zadejte název vaší aplikace a e-mailu
3.  Ujistěte se, že není zaškrtnuto políčko hello pro instalaci na základě
4.  Klikněte na tlačítko `Add Platform`, zvolte položku `Native Application` a klikněte na tlačítko`Save`
5.  Vraťte se zpátky tooXcode. V `ViewController.swift`, nahraďte hello řádku počínaje '`let kClientID`"s ID aplikace hello jste právě zaregistrovali:

```swift
let kClientID = "Your_Application_Id_Here"
```

<!-- Workaround for Docs conversion bug -->
<ol start="6">
<li>
CTRL + klikněte na tlačítko <code>Info.plist</code> toobring až hello kontextové nabídky a potom klikněte na: <code>Open As</code>> <code>Source Code</code>
</li>
<li>
V části hello <code>dict</code> kořenový uzel, přidejte následující hello:
</li>
</ol>

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msal[Your_Application_Id_Here]</string>
            <string>auth</string>
        </array>
    </dict>
</array>
```
<ol start="8">
<li>
Nahraďte <i> <code>[Your_Application_Id_Here]</code> </i> s hello Id aplikace, které jste právě zaregistrovali
</li>
</ol>
