---
title: "Azure AD v2 iOS Začínáme - konfigurace (ARP) | Microsoft Docs"
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
ms.openlocfilehash: 50cb4a2803b6aebe8b39ec9fb02da2293c1065fa
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
## <a name="add-the-applications-registration-information-to-your-app"></a><span data-ttu-id="468c3-103">Přidat informace o registraci aplikace do aplikace</span><span class="sxs-lookup"><span data-stu-id="468c3-103">Add the application’s registration information to your app</span></span>

<span data-ttu-id="468c3-104">V tomto kroku budete muset do projektu přidejte Id aplikace:</span><span class="sxs-lookup"><span data-stu-id="468c3-104">In this step, you need to add the Application Id to your project:</span></span>

1.  <span data-ttu-id="468c3-105">V `ViewController.swift`, nahraďte řádek '`let kClientID`' s:</span><span class="sxs-lookup"><span data-stu-id="468c3-105">In `ViewController.swift`, replace the line starting with '`let kClientID`' with:</span></span>
```swift
let kClientID = "[Enter the application Id here]"
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
<span data-ttu-id="468c3-106">CTRL + klikněte na tlačítko <code>Info.plist</code> zprovoznit v kontextové nabídce a potom klikněte na: <code>Open As</code>> <code>Source Code</code>
</span><span class="sxs-lookup"><span data-stu-id="468c3-106">Control+click <code>Info.plist</code> to bring up the contextual menu, and then click: <code>Open As</code> > <code>Source Code</code>
</span></span></li>
<li>
<span data-ttu-id="468c3-107">V části <code>dict</code> kořenový uzel, přidejte následující:</span><span class="sxs-lookup"><span data-stu-id="468c3-107">Under the <code>dict</code> root node, add the following:</span></span>
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
            <string>msal[Enter the application Id here]</string>
            <string>auth</string>
        </array>
    </dict>
</array>
```
