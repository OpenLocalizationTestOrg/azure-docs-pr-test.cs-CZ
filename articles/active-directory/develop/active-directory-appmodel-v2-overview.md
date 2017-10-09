---
title: "koncový bod v2.0 aaaAzure služby Active Directory | Microsoft Docs"
description: "Aplikace Úvod toobuilding Account Microsoft a Azure Active Directory přihlášení."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 2dee579f-fdf6-474b-bc2c-016c931eaa27
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: ae5946b02c50ae5a6137dc1decae66c96647e875
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a><span data-ttu-id="8f66b-103">Přihlášení uživatele Azure AD v jediné aplikaci & Account Microsoft</span><span class="sxs-lookup"><span data-stu-id="8f66b-103">Sign-in Microsoft Account & Azure AD users in a single app</span></span>
<span data-ttu-id="8f66b-104">V hello po vývojář aplikace, kteří chtěli toosupport i osobní účty Microsoft a pracovních účtů ze služby Azure Active Directory byla požadovaná toointegrate dva samostatné systémy.</span><span class="sxs-lookup"><span data-stu-id="8f66b-104">In hello past, an app developer who wanted toosupport both personal Microsoft accounts and work accounts from Azure Active Directory was required toointegrate with two separate systems.</span></span>  <span data-ttu-id="8f66b-105">Hello **koncového bodu Azure AD v2.0** zavádí novou verzi ověřování rozhraní API, která vám umožní toosign v obou typech účty pomocí jednoho Jednoduchá integrace.</span><span class="sxs-lookup"><span data-stu-id="8f66b-105">hello **Azure AD v2.0 endpoint** introduces a new authentication API version that enables you toosign in both types of accounts using one simple integration.</span></span>  <span data-ttu-id="8f66b-106">Aplikace, které používají koncového bodu v2.0 hello můžete také používat rozhraní REST API z hello [Microsoft Graph](https://graph.microsoft.io) pomocí buď typu účtu.</span><span class="sxs-lookup"><span data-stu-id="8f66b-106">Apps that use hello v2.0 endpoint can also consume REST APIs from hello [Microsoft Graph](https://graph.microsoft.io) using either type of account.</span></span>

## <a name="getting-started"></a><span data-ttu-id="8f66b-107">Začínáme</span><span class="sxs-lookup"><span data-stu-id="8f66b-107">Getting Started</span></span>
<span data-ttu-id="8f66b-108">Vyberte oblíbenou platformu z hello následující seznamu toobuild aplikace pomocí našich opensourcové knihovny & architektury.</span><span class="sxs-lookup"><span data-stu-id="8f66b-108">Choose your favorite platform from hello following list toobuild an app using our open source libraries & frameworks.</span></span>  <span data-ttu-id="8f66b-109">Alternativně můžete použít toosend naše OAuth 2.0 & OpenID Connect dokumentaci protokolu a přijímat zprávy protokolu přímo bez použití knihovny ověřování.</span><span class="sxs-lookup"><span data-stu-id="8f66b-109">Alternatively, you can use our OAuth 2.0 & OpenID Connect protocol documentation toosend & receive protocol messages directly without using an auth library.</span></span>

<br />

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a><span data-ttu-id="8f66b-110">Novinky</span><span class="sxs-lookup"><span data-stu-id="8f66b-110">What's New</span></span>
<span data-ttu-id="8f66b-111">Zde Hello informace budou užitečné porozumět, co je & co není možné s koncovým bodem v2.0 hello.</span><span class="sxs-lookup"><span data-stu-id="8f66b-111">hello information here will be useful in understanding what is & what isn't possible with hello v2.0 endpoint.</span></span>

* <span data-ttu-id="8f66b-112">Další informace o hello [typy aplikací můžete vytvořit s koncovým bodem v2.0 hello](active-directory-v2-flows.md).</span><span class="sxs-lookup"><span data-stu-id="8f66b-112">Learn about hello [types of apps you can build with hello v2.0 endpoint](active-directory-v2-flows.md).</span></span>
* <span data-ttu-id="8f66b-113">Pochopení hello [omezení, omezení a omezení](active-directory-v2-limitations.md) s koncovým bodem v2.0 hello.</span><span class="sxs-lookup"><span data-stu-id="8f66b-113">Understand hello [limitations, restrictions, and constraints](active-directory-v2-limitations.md) with hello v2.0 endpoint.</span></span>
* <span data-ttu-id="8f66b-114">Podívejte se na tento přehled – video pro koncový bod v2.0 hello:</span><span class="sxs-lookup"><span data-stu-id="8f66b-114">Check out this overview video for hello v2.0 endpoint:</span></span>

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="reference"></a><span data-ttu-id="8f66b-115">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="8f66b-115">Reference</span></span>
<span data-ttu-id="8f66b-116">Následující odkazy budou užitečné při prozkoumávání hello platformy podrobněji:</span><span class="sxs-lookup"><span data-stu-id="8f66b-116">These links will be useful for exploring hello platform in depth:</span></span>

* [<span data-ttu-id="8f66b-117">referenční informace o protokolu v2.0</span><span class="sxs-lookup"><span data-stu-id="8f66b-117">v2.0 Protocol Reference</span></span>](active-directory-v2-protocols.md)
* [<span data-ttu-id="8f66b-118">Odkaz tokenu v2.0</span><span class="sxs-lookup"><span data-stu-id="8f66b-118">v2.0 Token Reference</span></span>](active-directory-v2-tokens.md)
* [<span data-ttu-id="8f66b-119">Referenční příručka knihovny v2.0</span><span class="sxs-lookup"><span data-stu-id="8f66b-119">v2.0 Library Reference</span></span>](active-directory-v2-libraries.md)
* [<span data-ttu-id="8f66b-120">Obory a souhlasu v koncového bodu v2.0 hello</span><span class="sxs-lookup"><span data-stu-id="8f66b-120">Scopes and Consent in hello v2.0 endpoint</span></span>](active-directory-v2-scopes.md)
* [<span data-ttu-id="8f66b-121">Hello Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="8f66b-121">hello Microsoft Graph</span></span>](https://graph.microsoft.io)

## <a name="help--support"></a><span data-ttu-id="8f66b-122">Nápověda a podpora</span><span class="sxs-lookup"><span data-stu-id="8f66b-122">Help & Support</span></span>
<span data-ttu-id="8f66b-123">Jedná se o hello nejlepší místech tooget pomoc s vývojem v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="8f66b-123">These are hello best places tooget help with developing on Azure Active Directory.</span></span>

* [<span data-ttu-id="8f66b-124">Značky `azure-active-directory` a `adal` na Stack Overflow</span><span class="sxs-lookup"><span data-stu-id="8f66b-124">Stack Overflow's `azure-active-directory` and `adal` tags</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory+or+adal)
* [<span data-ttu-id="8f66b-125">Zpětná vazba k Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8f66b-125">Feedback on Azure Active Directory</span></span>](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences)


> [!NOTE]
> <span data-ttu-id="8f66b-126">Pokud potřebujete jenom toosign v pracovním a školním účtům z Azure Active Directory, měli byste začít s naše [Příručka pro vývojáře Azure AD](active-directory-developers-guide.md).</span><span class="sxs-lookup"><span data-stu-id="8f66b-126">If you only need toosign in work and school accounts from Azure Active Directory, you should start with our [Azure AD developer's guide](active-directory-developers-guide.md).</span></span>  <span data-ttu-id="8f66b-127">koncový bod v2.0 Hello je určená pro vývojáře, kteří potřebují explicitně toosign v osobní účty Microsoft.</span><span class="sxs-lookup"><span data-stu-id="8f66b-127">hello v2.0 endpoint is intended for use by developers who explicitly need toosign in Microsoft personal accounts.</span></span>

