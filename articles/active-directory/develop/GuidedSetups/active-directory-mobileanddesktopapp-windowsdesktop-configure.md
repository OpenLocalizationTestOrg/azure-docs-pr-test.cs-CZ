---
title: "Azure AD v2 Windows Desktop získávání spuštěno – konfigurace | Microsoft Docs"
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
ms.openlocfilehash: 1dfaa7ade664e43dcb9aa788b0197ca17e6ec4cc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
## <a name="create-an-application-express"></a><span data-ttu-id="87dc6-104">Vytvoření aplikace (Express)</span><span class="sxs-lookup"><span data-stu-id="87dc6-104">Create an application (Express)</span></span>
<span data-ttu-id="87dc6-105">Nyní je nutné zaregistrovat aplikaci v *portálu pro registraci aplikace Microsoft*:</span><span class="sxs-lookup"><span data-stu-id="87dc6-105">Now you need to register your application in the *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="87dc6-106">Registrace vaší aplikace pomocí [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure)</span><span class="sxs-lookup"><span data-stu-id="87dc6-106">Register your application via the [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure)</span></span>
2.  <span data-ttu-id="87dc6-107">Zadejte název vaší aplikace a e-mailu</span><span class="sxs-lookup"><span data-stu-id="87dc6-107">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="87dc6-108">Ujistěte se, že je zaškrtnuté políčko pro instalaci na základě</span><span class="sxs-lookup"><span data-stu-id="87dc6-108">Make sure the option for Guided Setup is checked</span></span>
4.  <span data-ttu-id="87dc6-109">Postupujte podle pokynů a získat ID aplikace a vložte jej do vašeho kódu</span><span class="sxs-lookup"><span data-stu-id="87dc6-109">Follow the instructions to obtain the application ID and paste it into your code</span></span>

### <a name="add-your-application-registration-information-to-your-solution-advanced"></a><span data-ttu-id="87dc6-110">Přidat informace o registraci aplikace k řešení (Upřesnit)</span><span class="sxs-lookup"><span data-stu-id="87dc6-110">Add your application registration information to your solution (Advanced)</span></span>
<span data-ttu-id="87dc6-111">Nyní je nutné zaregistrovat aplikaci v *portálu pro registraci aplikace Microsoft*:</span><span class="sxs-lookup"><span data-stu-id="87dc6-111">Now you need to register your application in the *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="87dc6-112">Přejděte na [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app) zaregistrovat aplikaci</span><span class="sxs-lookup"><span data-stu-id="87dc6-112">Go to the [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app) to register an application</span></span>
2. <span data-ttu-id="87dc6-113">Zadejte název vaší aplikace a e-mailu</span><span class="sxs-lookup"><span data-stu-id="87dc6-113">Enter a name for your application and your email</span></span> 
3. <span data-ttu-id="87dc6-114">Ujistěte se, že není zaškrtnuto políčko pro instalaci na základě</span><span class="sxs-lookup"><span data-stu-id="87dc6-114">Make sure the option for Guided Setup is unchecked</span></span>
4. <span data-ttu-id="87dc6-115">Klikněte na tlačítko `Add Platform`, zvolte položku `Native Application` a klikněte na tlačítko Uložit</span><span class="sxs-lookup"><span data-stu-id="87dc6-115">Click `Add Platform`, then select `Native Application` and hit Save</span></span>
5. <span data-ttu-id="87dc6-116">Zkopírovat identifikátor GUID v ID aplikace, přejděte zpět do Visual Studio, otevřete `App.xaml.cs` a nahraďte `your_client_id_here` s ID aplikace, který jste právě zaregistrovali:</span><span class="sxs-lookup"><span data-stu-id="87dc6-116">Copy the GUID in Application ID, go back to Visual Studio, open `App.xaml.cs` and replace `your_client_id_here` with the Application ID you just registered:</span></span>

```csharp
private static string ClientId = "your_application_id_here";
```
