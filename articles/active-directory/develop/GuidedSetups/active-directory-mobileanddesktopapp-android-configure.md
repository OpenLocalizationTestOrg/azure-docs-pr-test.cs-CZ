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
## <a name="create-an-application-express"></a><span data-ttu-id="d1f56-103">Vytvoření aplikace (Express)</span><span class="sxs-lookup"><span data-stu-id="d1f56-103">Create an application (Express)</span></span>
<span data-ttu-id="d1f56-104">Nyní je třeba tooregister svoji aplikaci v hello *portálu pro registraci aplikace Microsoft*:</span><span class="sxs-lookup"><span data-stu-id="d1f56-104">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="d1f56-105">Registrace vaší aplikace prostřednictvím hello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure)</span><span class="sxs-lookup"><span data-stu-id="d1f56-105">Register your application via hello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure)</span></span>
2.  <span data-ttu-id="d1f56-106">Zadejte název vaší aplikace a e-mailu</span><span class="sxs-lookup"><span data-stu-id="d1f56-106">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="d1f56-107">Ujistěte se, že je zaškrtnuté políčko hello pro instalaci na základě</span><span class="sxs-lookup"><span data-stu-id="d1f56-107">Make sure hello option for Guided Setup is checked</span></span>
4.  <span data-ttu-id="d1f56-108">Postupujte podle ID aplikace hello pokyny tooobtain hello a vložte jej do vašeho kódu</span><span class="sxs-lookup"><span data-stu-id="d1f56-108">Follow hello instructions tooobtain hello application ID and paste it into your code</span></span>

### <a name="add-your-application-registration-information-tooyour-solution-advanced"></a><span data-ttu-id="d1f56-109">Přidat řešení aplikace registrační informace tooyour (Upřesnit)</span><span class="sxs-lookup"><span data-stu-id="d1f56-109">Add your application registration information tooyour solution (Advanced)</span></span>
<span data-ttu-id="d1f56-110">Nyní je třeba tooregister svoji aplikaci v hello *portálu pro registraci aplikace Microsoft*:</span><span class="sxs-lookup"><span data-stu-id="d1f56-110">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="d1f56-111">Přejděte toohello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app) tooregister aplikace</span><span class="sxs-lookup"><span data-stu-id="d1f56-111">Go toohello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app) tooregister an application</span></span>
2. <span data-ttu-id="d1f56-112">Zadejte název vaší aplikace a e-mailu</span><span class="sxs-lookup"><span data-stu-id="d1f56-112">Enter a name for your application and your email</span></span> 
3. <span data-ttu-id="d1f56-113">Ujistěte se, že není zaškrtnuto políčko hello pro instalaci na základě</span><span class="sxs-lookup"><span data-stu-id="d1f56-113">Make sure hello option for Guided Setup is unchecked</span></span>
4. <span data-ttu-id="d1f56-114">Klikněte na tlačítko `Add Platform`, zvolte položku `Native Application` a klikněte na tlačítko Uložit</span><span class="sxs-lookup"><span data-stu-id="d1f56-114">Click `Add Platform`, then select `Native Application` and hit Save</span></span>
5.  <span data-ttu-id="d1f56-115">Otevřete `MainActivity` (v části `app`  >  `java`  >   *`{host}.{namespace}`* )</span><span class="sxs-lookup"><span data-stu-id="d1f56-115">Open `MainActivity` (under `app` > `java` > *`{host}.{namespace}`*)</span></span>
6.  <span data-ttu-id="d1f56-116">Nahraďte hello *[Zadejte sem hello aplikace Id]* v řádku hello počínaje `final static String CLIENT_ID` s ID aplikace hello jste právě zaregistrovali:</span><span class="sxs-lookup"><span data-stu-id="d1f56-116">Replace hello *[Enter hello application Id here]* in hello line starting with `final static String CLIENT_ID` with hello application ID you just registered:</span></span>

```java
final static String CLIENT_ID = "[Enter hello application Id here]";
```
<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
<span data-ttu-id="d1f56-117">Otevřete `AndroidManifest.xml` (v části `app`  >  `manifests`) hello přidat následující aktivitu příliš`manifest\application` uzlu.</span><span class="sxs-lookup"><span data-stu-id="d1f56-117">Open `AndroidManifest.xml` (under `app` > `manifests`) Add hello following activity too`manifest\application` node.</span></span> <span data-ttu-id="d1f56-118">Takovém postupu zaregistruje `BrowserTabActivity` tooallow hello OS tooresume aplikaci po dokončení ověření hello:</span><span class="sxs-lookup"><span data-stu-id="d1f56-118">This registers a `BrowserTabActivity` tooallow hello OS tooresume your application after completing hello authentication:</span></span>
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
<span data-ttu-id="d1f56-119">V hello `BrowserTabActivity`, nahraďte `[Enter hello application Id here]` s ID hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="d1f56-119">In hello `BrowserTabActivity`, replace `[Enter hello application Id here]` with hello application ID.</span></span>
</li>
</ol>
