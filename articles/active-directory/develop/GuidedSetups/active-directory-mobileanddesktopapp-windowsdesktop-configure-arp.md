---
title: "Azure AD v2 Windows Desktop získávání spuštěno – konfigurace | Microsoft Docs"
description: "Aplikace Windows Desktop .NET (XAML) jak získat přístupový token a volat rozhraní API chráněné službou Azure Active Directory v2 koncový bod."
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
ms.openlocfilehash: 5e83171846517496e221f0a84565cdf7b77514df
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
## <a name="add-the-applications-registration-information-to-your-app"></a><span data-ttu-id="1cdab-103">Přidat informace o registraci aplikace do aplikace</span><span class="sxs-lookup"><span data-stu-id="1cdab-103">Add the application’s registration information to your app</span></span>
<span data-ttu-id="1cdab-104">V tomto kroku budete muset do projektu přidejte Id aplikace.</span><span class="sxs-lookup"><span data-stu-id="1cdab-104">In this step, you need to add the Application Id to your project.</span></span>

1.  <span data-ttu-id="1cdab-105">Otevřete `App.xaml.cs` a nahraďte řádek obsahující `ClientId` se:</span><span class="sxs-lookup"><span data-stu-id="1cdab-105">Open `App.xaml.cs` and replace the line containing the `ClientId` with:</span></span>

```csharp
private static string ClientId = "[Enter the application Id here]";
```

### <a name="what-is-next"></a><span data-ttu-id="1cdab-106">Co je další</span><span class="sxs-lookup"><span data-stu-id="1cdab-106">What is Next</span></span>

[<span data-ttu-id="1cdab-107">Testování a ověření</span><span class="sxs-lookup"><span data-stu-id="1cdab-107">Test and Validate</span></span>](active-directory-mobileanddesktopapp-windowsdesktop-test.md)