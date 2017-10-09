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
## <a name="create-an-application-express"></a><span data-ttu-id="f4599-104">Vytvoření aplikace (Express)</span><span class="sxs-lookup"><span data-stu-id="f4599-104">Create an application (Express)</span></span>
<span data-ttu-id="f4599-105">Nyní je třeba tooregister svoji aplikaci v hello *portálu pro registraci aplikace Microsoft*:</span><span class="sxs-lookup"><span data-stu-id="f4599-105">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="f4599-106">Registrace vaší aplikace prostřednictvím hello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure)</span><span class="sxs-lookup"><span data-stu-id="f4599-106">Register your application via hello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure)</span></span>
2.  <span data-ttu-id="f4599-107">Zadejte název vaší aplikace a e-mailu</span><span class="sxs-lookup"><span data-stu-id="f4599-107">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="f4599-108">Ujistěte se, že je zaškrtnuté políčko hello pro instalaci na základě</span><span class="sxs-lookup"><span data-stu-id="f4599-108">Make sure hello option for Guided Setup is checked</span></span>
4.  <span data-ttu-id="f4599-109">Postupujte podle ID aplikace hello pokyny tooobtain hello a vložte jej do vašeho kódu</span><span class="sxs-lookup"><span data-stu-id="f4599-109">Follow hello instructions tooobtain hello application ID and paste it into your code</span></span>

### <a name="add-your-application-registration-information-tooyour-solution-advanced"></a><span data-ttu-id="f4599-110">Přidat řešení aplikace registrační informace tooyour (Upřesnit)</span><span class="sxs-lookup"><span data-stu-id="f4599-110">Add your application registration information tooyour solution (Advanced)</span></span>
<span data-ttu-id="f4599-111">Nyní je třeba tooregister svoji aplikaci v hello *portálu pro registraci aplikace Microsoft*:</span><span class="sxs-lookup"><span data-stu-id="f4599-111">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="f4599-112">Přejděte toohello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app) tooregister aplikace</span><span class="sxs-lookup"><span data-stu-id="f4599-112">Go toohello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app) tooregister an application</span></span>
2. <span data-ttu-id="f4599-113">Zadejte název vaší aplikace a e-mailu</span><span class="sxs-lookup"><span data-stu-id="f4599-113">Enter a name for your application and your email</span></span> 
3. <span data-ttu-id="f4599-114">Ujistěte se, že není zaškrtnuto políčko hello pro instalaci na základě</span><span class="sxs-lookup"><span data-stu-id="f4599-114">Make sure hello option for Guided Setup is unchecked</span></span>
4. <span data-ttu-id="f4599-115">Klikněte na tlačítko `Add Platform`, zvolte položku `Native Application` a klikněte na tlačítko Uložit</span><span class="sxs-lookup"><span data-stu-id="f4599-115">Click `Add Platform`, then select `Native Application` and hit Save</span></span>
5. <span data-ttu-id="f4599-116">Zkopírujte hello identifikátor GUID v ID aplikace, přejděte zpět tooVisual Studio otevřete `App.xaml.cs` a nahraďte `your_client_id_here` s hello ID aplikace, které jste právě zaregistrovali:</span><span class="sxs-lookup"><span data-stu-id="f4599-116">Copy hello GUID in Application ID, go back tooVisual Studio, open `App.xaml.cs` and replace `your_client_id_here` with hello Application ID you just registered:</span></span>

```csharp
private static string ClientId = "your_application_id_here";
```
