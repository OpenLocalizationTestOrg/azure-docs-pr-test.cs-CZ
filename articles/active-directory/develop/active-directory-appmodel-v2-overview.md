---
title: "Azure Active Directory koncového bodu v2.0 | Microsoft Docs"
description: "Úvod do vytváření aplikace Microsoft Account a Azure Active Directory přihlášení."
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
ms.openlocfilehash: 1e286044fb1a1b367fcac2dc14c47f68d5ed120d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a><span data-ttu-id="37659-103">Přihlášení uživatele Azure AD v jediné aplikaci & Account Microsoft</span><span class="sxs-lookup"><span data-stu-id="37659-103">Sign-in Microsoft Account & Azure AD users in a single app</span></span>
<span data-ttu-id="37659-104">V minulosti vývojář aplikace, kteří chtěli podporovat i osobní účty Microsoft a pracovní účty ze služby Azure Active Directory vyžaduje integrovat dvou samostatných systémech.</span><span class="sxs-lookup"><span data-stu-id="37659-104">In the past, an app developer who wanted to support both personal Microsoft accounts and work accounts from Azure Active Directory was required to integrate with two separate systems.</span></span>  <span data-ttu-id="37659-105">**Koncového bodu Azure AD v2.0** zavádí novou verzi ověřování rozhraní API, která umožňuje přihlásit oba typy účtů pomocí jednoho Jednoduchá integrace.</span><span class="sxs-lookup"><span data-stu-id="37659-105">The **Azure AD v2.0 endpoint** introduces a new authentication API version that enables you to sign in both types of accounts using one simple integration.</span></span>  <span data-ttu-id="37659-106">Aplikace, které používají koncového bodu v2.0 můžete také používat rozhraní REST API z [Microsoft Graph](https://graph.microsoft.io) pomocí buď typu účtu.</span><span class="sxs-lookup"><span data-stu-id="37659-106">Apps that use the v2.0 endpoint can also consume REST APIs from the [Microsoft Graph](https://graph.microsoft.io) using either type of account.</span></span>

## <a name="getting-started"></a><span data-ttu-id="37659-107">Začínáme</span><span class="sxs-lookup"><span data-stu-id="37659-107">Getting Started</span></span>
<span data-ttu-id="37659-108">Vyberte oblíbenou platformu z následujícího seznamu k sestavení aplikace pomocí našich opensourcové knihovny & architektury.</span><span class="sxs-lookup"><span data-stu-id="37659-108">Choose your favorite platform from the following list to build an app using our open source libraries & frameworks.</span></span>  <span data-ttu-id="37659-109">Alternativně můžete dokumentaci protokol OAuth 2.0 & OpenID Connect odesílat a přijímat zprávy protokolu přímo bez použití knihovny ověřování.</span><span class="sxs-lookup"><span data-stu-id="37659-109">Alternatively, you can use our OAuth 2.0 & OpenID Connect protocol documentation to send & receive protocol messages directly without using an auth library.</span></span>

<br />

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a><span data-ttu-id="37659-110">Novinky</span><span class="sxs-lookup"><span data-stu-id="37659-110">What's New</span></span>
<span data-ttu-id="37659-111">Zde uvedené informace budou užitečné porozumět, co je & co není možné s koncovým bodem v2.0.</span><span class="sxs-lookup"><span data-stu-id="37659-111">The information here will be useful in understanding what is & what isn't possible with the v2.0 endpoint.</span></span>

* <span data-ttu-id="37659-112">Další informace o [typy aplikací můžete vytvořit s koncovým bodem v2.0](active-directory-v2-flows.md).</span><span class="sxs-lookup"><span data-stu-id="37659-112">Learn about the [types of apps you can build with the v2.0 endpoint](active-directory-v2-flows.md).</span></span>
* <span data-ttu-id="37659-113">Pochopení [omezení, omezení a omezení](active-directory-v2-limitations.md) s koncovým bodem v2.0.</span><span class="sxs-lookup"><span data-stu-id="37659-113">Understand the [limitations, restrictions, and constraints](active-directory-v2-limitations.md) with the v2.0 endpoint.</span></span>
* <span data-ttu-id="37659-114">Podívejte se na tento přehled – video pro koncový bod v2.0:</span><span class="sxs-lookup"><span data-stu-id="37659-114">Check out this overview video for the v2.0 endpoint:</span></span>

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="reference"></a><span data-ttu-id="37659-115">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="37659-115">Reference</span></span>
<span data-ttu-id="37659-116">Následující odkazy budou užitečné při prozkoumávání platformou podrobněji:</span><span class="sxs-lookup"><span data-stu-id="37659-116">These links will be useful for exploring the platform in depth:</span></span>

* [<span data-ttu-id="37659-117">referenční informace o protokolu v2.0</span><span class="sxs-lookup"><span data-stu-id="37659-117">v2.0 Protocol Reference</span></span>](active-directory-v2-protocols.md)
* [<span data-ttu-id="37659-118">Odkaz tokenu v2.0</span><span class="sxs-lookup"><span data-stu-id="37659-118">v2.0 Token Reference</span></span>](active-directory-v2-tokens.md)
* [<span data-ttu-id="37659-119">Referenční příručka knihovny v2.0</span><span class="sxs-lookup"><span data-stu-id="37659-119">v2.0 Library Reference</span></span>](active-directory-v2-libraries.md)
* [<span data-ttu-id="37659-120">Obory a souhlasu v koncového bodu v2.0</span><span class="sxs-lookup"><span data-stu-id="37659-120">Scopes and Consent in the v2.0 endpoint</span></span>](active-directory-v2-scopes.md)
* [<span data-ttu-id="37659-121">Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="37659-121">The Microsoft Graph</span></span>](https://graph.microsoft.io)

## <a name="help--support"></a><span data-ttu-id="37659-122">Nápověda a podpora</span><span class="sxs-lookup"><span data-stu-id="37659-122">Help & Support</span></span>
<span data-ttu-id="37659-123">Na těchto místech získáte nejlepší pomoc s vývojem v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="37659-123">These are the best places to get help with developing on Azure Active Directory.</span></span>

* [<span data-ttu-id="37659-124">Značky `azure-active-directory` a `adal` na Stack Overflow</span><span class="sxs-lookup"><span data-stu-id="37659-124">Stack Overflow's `azure-active-directory` and `adal` tags</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory+or+adal)
* [<span data-ttu-id="37659-125">Zpětná vazba k Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="37659-125">Feedback on Azure Active Directory</span></span>](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences)


> [!NOTE]
> <span data-ttu-id="37659-126">Pokud potřebujete přihlásit pracovní a školní účty ze služby Azure Active Directory, měli byste začít s naše [Příručka pro vývojáře Azure AD](active-directory-developers-guide.md).</span><span class="sxs-lookup"><span data-stu-id="37659-126">If you only need to sign in work and school accounts from Azure Active Directory, you should start with our [Azure AD developer's guide](active-directory-developers-guide.md).</span></span>  <span data-ttu-id="37659-127">Koncový bod v2.0 je určená pro vývojáře, kteří explicitně se muset přihlásit osobní účty Microsoft.</span><span class="sxs-lookup"><span data-stu-id="37659-127">The v2.0 endpoint is intended for use by developers who explicitly need to sign in Microsoft personal accounts.</span></span>

