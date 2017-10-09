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
## <a name="create-an-application-express"></a><span data-ttu-id="7105e-103">Vytvoření aplikace (Express)</span><span class="sxs-lookup"><span data-stu-id="7105e-103">Create an application (Express)</span></span>
<span data-ttu-id="7105e-104">Nyní je třeba tooregister svoji aplikaci v hello *portálu pro registraci aplikace Microsoft*:</span><span class="sxs-lookup"><span data-stu-id="7105e-104">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="7105e-105">Registrace vaší aplikace prostřednictvím hello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=ios&step=configure)</span><span class="sxs-lookup"><span data-stu-id="7105e-105">Register your application via hello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=ios&step=configure)</span></span>
2.  <span data-ttu-id="7105e-106">Zadejte název vaší aplikace a e-mailu</span><span class="sxs-lookup"><span data-stu-id="7105e-106">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="7105e-107">Ujistěte se, že je zaškrtnuté políčko hello pro instalaci na základě</span><span class="sxs-lookup"><span data-stu-id="7105e-107">Make sure hello option for Guided Setup is checked</span></span>
4.  <span data-ttu-id="7105e-108">Postupujte podle ID aplikace hello pokyny tooobtain hello a vložte jej do vašeho kódu</span><span class="sxs-lookup"><span data-stu-id="7105e-108">Follow hello instructions tooobtain hello application ID and paste it into your code</span></span>

### <a name="add-your-application-registration-information-tooyour-solution-advanced"></a><span data-ttu-id="7105e-109">Přidat řešení aplikace registrační informace tooyour (Upřesnit)</span><span class="sxs-lookup"><span data-stu-id="7105e-109">Add your application registration information tooyour solution (Advanced)</span></span>

1.  <span data-ttu-id="7105e-110">Přejděte příliš[portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app)</span><span class="sxs-lookup"><span data-stu-id="7105e-110">Go too[Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app)</span></span>
2.  <span data-ttu-id="7105e-111">Zadejte název vaší aplikace a e-mailu</span><span class="sxs-lookup"><span data-stu-id="7105e-111">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="7105e-112">Ujistěte se, že není zaškrtnuto políčko hello pro instalaci na základě</span><span class="sxs-lookup"><span data-stu-id="7105e-112">Make sure hello option for Guided Setup is unchecked</span></span>
4.  <span data-ttu-id="7105e-113">Klikněte na tlačítko `Add Platform`, zvolte položku `Native Application` a klikněte na tlačítko`Save`</span><span class="sxs-lookup"><span data-stu-id="7105e-113">Click `Add Platform`, then select `Native Application` and click `Save`</span></span>
5.  <span data-ttu-id="7105e-114">Vraťte se zpátky tooXcode.</span><span class="sxs-lookup"><span data-stu-id="7105e-114">Go back tooXcode.</span></span> <span data-ttu-id="7105e-115">V `ViewController.swift`, nahraďte hello řádku počínaje '`let kClientID`"s ID aplikace hello jste právě zaregistrovali:</span><span class="sxs-lookup"><span data-stu-id="7105e-115">In `ViewController.swift`, replace hello line starting with '`let kClientID`' with hello application ID you just registered:</span></span>

```swift
let kClientID = "Your_Application_Id_Here"
```

<!-- Workaround for Docs conversion bug -->
<ol start="6">
<li>
<span data-ttu-id="7105e-116">CTRL + klikněte na tlačítko <code>Info.plist</code> toobring až hello kontextové nabídky a potom klikněte na: <code>Open As</code>> <code>Source Code</code>
</span><span class="sxs-lookup"><span data-stu-id="7105e-116">Control+click <code>Info.plist</code> toobring up hello contextual menu, and then click: <code>Open As</code> > <code>Source Code</code>
</span></span></li>
<li>
<span data-ttu-id="7105e-117">V části hello <code>dict</code> kořenový uzel, přidejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="7105e-117">Under hello <code>dict</code> root node, add hello following:</span></span>
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
<span data-ttu-id="7105e-118">Nahraďte <i> <code>[Your_Application_Id_Here]</code> </i> s hello Id aplikace, které jste právě zaregistrovali</span><span class="sxs-lookup"><span data-stu-id="7105e-118">Replace <i><code>[Your_Application_Id_Here]</code></i> with hello Application Id you just registered</span></span>
</li>
</ol>
