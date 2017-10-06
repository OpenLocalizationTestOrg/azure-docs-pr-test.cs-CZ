---
title: "aaaHow tooconfigure novou aplikaci víceklientské | Microsoft Docs"
description: "Jak tooconfigure jednotné přihlašování pro vlastní aplikaci vývoji a Probíhá registrace ve službě Azure AD."
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
ms.openlocfilehash: 4d3499d8885933516d6597fa9f87bcf88cd5a428
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-a-new-multi-tenant-application"></a><span data-ttu-id="2b946-103">Jak tooconfigure novou aplikaci pro více klientů</span><span class="sxs-lookup"><span data-stu-id="2b946-103">How tooconfigure a new multi-tenant application</span></span>

<span data-ttu-id="2b946-104">Povolení federované jednotné přihlašování (SSO) v aplikaci je automaticky povolen při federaci prostřednictvím služby Azure AD pro OpenID Connect SAML 2.0 a WS-Fed.</span><span class="sxs-lookup"><span data-stu-id="2b946-104">Enabling federated single sign-on (SSO) in your app is automatically enabled when federating through Azure AD for OpenID Connect, SAML 2.0, or WS-Fed.</span></span> <span data-ttu-id="2b946-105">Pokud vaši koncoví uživatelé jsou s toosign navzdory již má existující relaci s Azure AD, je pravděpodobné, že vaše aplikace může být nesprávné.</span><span class="sxs-lookup"><span data-stu-id="2b946-105">If your end users are having toosign in despite already having an existing session with Azure AD, it’s likely your app may be misconfigured.</span></span>

* <span data-ttu-id="2b946-106">Pokud používáte ADAL/MSAL, ujistěte se, máte **PromptBehavior** nastavit příliš**automaticky** místo **vždy**.</span><span class="sxs-lookup"><span data-stu-id="2b946-106">If you’re using ADAL/MSAL, make sure you have **PromptBehavior** set too**Auto** rather than **Always**.</span></span>

* <span data-ttu-id="2b946-107">Pokud vytváříte mobilní aplikace, budete potřebovat další konfigurace tooenable zprostředkované nebo jiných zprostředkované jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2b946-107">If you’re building a mobile app, you may need additional configurations tooenable brokered or non-brokered SSO.</span></span>

<span data-ttu-id="2b946-108">Pro Android, najdete v části [povolení mezi aplikace jednotné přihlašování v Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android).</span><span class="sxs-lookup"><span data-stu-id="2b946-108">For Android, see [Enabling Cross App SSO in Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android).</span></span><br>

<span data-ttu-id="2b946-109">Pro iOS, najdete v části [povolení mezi aplikace jednotné přihlašování v iOS](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios).</span><span class="sxs-lookup"><span data-stu-id="2b946-109">For iOS, see [Enabling Cross App SSO in iOS](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2b946-110">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2b946-110">Next steps</span></span>

[<span data-ttu-id="2b946-111">Jednotné přihlašování Azure AD</span><span class="sxs-lookup"><span data-stu-id="2b946-111">Azure AD SSO</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)<br>

[<span data-ttu-id="2b946-112">Povolení křížové jednotného přihlašování k aplikaci v Android</span><span class="sxs-lookup"><span data-stu-id="2b946-112">Enabling Cross App SSO in Android</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android)<br>

[<span data-ttu-id="2b946-113">Povolení křížové aplikace jednotné přihlašování v iOS</span><span class="sxs-lookup"><span data-stu-id="2b946-113">Enabling Cross App SSO in iOS</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios)<br>

[<span data-ttu-id="2b946-114">Integrace aplikace tooAzureAD</span><span class="sxs-lookup"><span data-stu-id="2b946-114">Integrating Apps tooAzureAD</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)<br>

[<span data-ttu-id="2b946-115">Souhlasu a systému oprávnění rolích pro AzureAD v2.0 konvergované aplikace</span><span class="sxs-lookup"><span data-stu-id="2b946-115">Consent and Permissioning for AzureAD v2.0 converged Apps</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[<span data-ttu-id="2b946-116">AzureAD StackOverflow</span><span class="sxs-lookup"><span data-stu-id="2b946-116">AzureAD StackOverflow</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)
