---
title: "Jak vybrat oprávnění pro dané rozhraní API | Microsoft Docs"
description: "Jak najít koncové body ověřování pro vlastní aplikaci vývoji nebo Probíhá registrace ve službě Azure AD."
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 6966cf145375bf3d830d476564c428502ae40fd4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-select-permissions-for-a-given-api"></a><span data-ttu-id="dcbad-103">Jak vybrat oprávnění pro dané rozhraní API</span><span class="sxs-lookup"><span data-stu-id="dcbad-103">How to select permissions for a given API</span></span>

<span data-ttu-id="dcbad-104">Můžete najít koncové body ověřování pro aplikaci v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="dcbad-104">You can find the authentication endpoints for your application in the [Azure portal](https://portal.azure.com).</span></span>

-   <span data-ttu-id="dcbad-105">Přejděte na [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="dcbad-105">Navigate to the [Azure portal](https://portal.azure.com).</span></span>

-   <span data-ttu-id="dcbad-106">V levém navigačním podokně klikněte na tlačítko **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="dcbad-106">From the left navigation pane, click **Azure Active Directory**.</span></span>

-   <span data-ttu-id="dcbad-107">Klikněte na tlačítko **registrace aplikace** a zvolte **koncové body**.</span><span class="sxs-lookup"><span data-stu-id="dcbad-107">Click **App Registrations** and choose **Endpoints**.</span></span>

-   <span data-ttu-id="dcbad-108">Otevře **koncové body** stránku, který zobrazí seznam všech koncových bodů ověřování pro vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="dcbad-108">This open up the **Endpoints** page, which list all the authentication endpoints for your tenant.</span></span>

-   <span data-ttu-id="dcbad-109">Použít specifické pro ověřovací protokol, který používáte koncový bod, ve spojení s ID aplikace vytvořit ověření požadavku specifický pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dcbad-109">Use the endpoint specific to the authentication protocol you are using, in conjunction with the application ID to craft the authentication request specific to your application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dcbad-110">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dcbad-110">Next steps</span></span>
[<span data-ttu-id="dcbad-111">Příručka pro vývojáře pro službu Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dcbad-111">Azure Active Directory developer's guide</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-developers-guide#authentication-and-authorization-protocols)
